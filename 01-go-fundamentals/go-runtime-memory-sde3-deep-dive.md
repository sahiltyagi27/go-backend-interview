# Go Runtime Memory Deep Dive for SDE-3

Main line:

```text
At SDE-3 level, know how Go allocates memory, when values escape to heap, how GC works, and how slices/maps behave internally.
```

## Why This Matters

Senior Go interviews often move from syntax to runtime behavior:

```text
Why did this allocation happen?
Why is this code creating GC pressure?
Why is this map unsafe concurrently?
Why did append change the backing array?
How would you debug high memory usage?
```

The important topics:

- escape analysis
- stack vs heap allocation
- allocator: `mcache`, `mcentral`, `mheap`
- tri-color concurrent garbage collector
- write barrier
- slice header and growth
- map internals: `hmap`, `bmap`, buckets, overflow
- performance debugging with `pprof` and compiler escape output

---

## 1. Stack vs Heap

## Stack

Stack allocation is fast.

```text
Function call creates a stack frame.
Local values can live inside that frame.
When function returns, the frame is gone.
No garbage collector work is needed.
```

Interview line:

```text
Stack allocation is cheap because it is mostly pointer movement. It has almost no GC overhead.
```

## Heap

Heap allocation is used when data must outlive the current function frame or cannot safely fit on the stack.

```text
Heap objects are tracked by the runtime and later reclaimed by garbage collection.
```

Interview line:

```text
Heap allocation is more expensive because the runtime allocator must manage it and the GC must later scan or reclaim it.
```

---

## 2. Escape Analysis

Escape analysis is a compiler pass that decides whether a value can stay on the stack or must move to the heap.

Interview answer:

```text
The compiler checks whether a value is used outside the lifetime of the function that created it. If the value may outlive the function frame, it escapes to the heap.
```

How to inspect it:

```bash
go build -gcflags="-m" ./...
```

More verbose:

```bash
go build -gcflags="-m -m" ./...
```

## Common Escape Triggers

### Returning a Pointer

```go
type User struct {
	Name string
}

func createUser() *User {
	u := User{Name: "Sahil"}
	return &u
}
```

`u` likely escapes because the caller needs it after `createUser` returns.

Interview line:

```text
Returning the address of a local variable usually forces it to heap because the value must survive after the function returns.
```

### Capturing Variables in Goroutines or Closures

```go
func start() {
	name := "Sahil"

	go func() {
		fmt.Println(name)
	}()
}
```

`name` may escape because the goroutine can run after `start` returns.

### Interface Indirection

Passing a value to an interface can cause escape.

Example:

```go
fmt.Println(user)
```

`fmt.Println` accepts `...any`, so values may be boxed into interfaces and handled through reflection.

Nuance:

```text
Passing to an interface does not always force heap allocation, but it is a common reason values escape in real code, especially with fmt/logging/reflection.
```

### Dynamic Size or Large Allocations

Example:

```go
buf := make([]byte, n)
```

If `n` is runtime-determined or the object is large, the compiler/runtime may place the backing array on the heap.

Nuance:

```text
Runtime size alone does not mean every value always escapes, but dynamic or large backing arrays commonly end up on the heap.
```

## Why Escape Analysis Matters

Heap allocations increase:

- allocator work
- GC scan/reclaim work
- latency risk under high load
- memory pressure

Senior answer:

```text
Escape analysis matters because fewer heap allocations mean less GC pressure and often better latency. For hot paths, I check allocation counts using benchmarks, pprof, and compiler escape output.
```

Benchmark allocation check:

```bash
go test -bench=. -benchmem ./...
```

---

## 3. Go Memory Allocator: mcache, mcentral, mheap

Go's allocator is designed to reduce global lock contention.

Simple picture:

```text
Goroutine
   |
   v
P-local mcache  -> fast allocation path
   |
   v
mcentral        -> shared pool for size classes
   |
   v
mheap           -> large/global heap spans from OS
```

## mcache

Each P has a local `mcache`.

```text
Small allocations can often be served from the current P's mcache without taking a global lock.
```

## mcentral

`mcentral` manages spans for a specific size class.

```text
If mcache runs out, it refills from mcentral.
```

## mheap

`mheap` manages large spans and memory obtained from the OS.

```text
Large allocations or central refills eventually come from mheap.
```

Interview line:

```text
Go uses per-P caches for fast small allocations, central lists for size classes, and a global heap for larger spans. This reduces allocator contention.
```

---

## 4. Garbage Collector: Tri-Color Mark and Sweep

Go uses a concurrent, tri-color, mark-and-sweep garbage collector.

Important properties:

```text
concurrent: runs mostly alongside application goroutines
tri-color: white, gray, black marking model
non-moving: currently Go does not compact/move objects
low-latency: designed to keep stop-the-world pauses short
```

## Colors

```text
White = not visited yet, potential garbage
Gray  = visited, but children not fully scanned
Black = visited and children scanned
```

Simple picture:

```text
White -> Gray -> Black
```

## Mark Phase

1. Start with objects as white.
2. Roots are marked gray.
3. Roots include goroutine stacks, globals, and runtime references.
4. GC scans gray objects.
5. Objects referenced by gray objects become gray.
6. Scanned gray objects become black.

Interview line:

```text
The mark phase finds all reachable objects starting from roots. Anything still white after marking is unreachable and can be swept.
```

## Sweep Phase

After marking:

```text
Objects still white are unreachable and can be reclaimed.
```

Sweep is also mostly concurrent.

---

## 5. Stop-The-World Phases

Go GC is concurrent, but not 100 percent free of pauses.

There are short stop-the-world phases, commonly around:

- mark start
- mark termination

Interview line:

```text
Go's GC runs mostly concurrently, but it still has short stop-the-world phases for coordination. The design goal is low latency, not zero pause.
```

---

## 6. Write Barrier

Problem:

```text
Application goroutines keep changing pointers while GC is marking.
```

Without protection, a live object could be hidden from the GC.

Example idea:

```text
Black object starts pointing to a white object while GC is running.
If GC never sees that white object, it may wrongly collect live memory.
```

Write barrier:

```text
During the mark phase, pointer writes go through a small runtime barrier so the GC can keep its view of reachable objects correct.
```

Interview line:

```text
The write barrier protects the tri-color invariant while application goroutines mutate pointers concurrently with the GC.
```

Simple answer:

```text
If a pointer update could make a white object reachable from an already scanned object, the write barrier makes sure that object is marked and not accidentally collected.
```

---

## 7. GOGC and GOMEMLIMIT

## GOGC

`GOGC` controls how aggressively GC runs.

Default:

```text
GOGC=100
```

Meaning:

```text
Run the next GC when heap has roughly grown by 100 percent over the previous live heap size.
```

Higher `GOGC`:

```text
fewer GC cycles, more memory usage
```

Lower `GOGC`:

```text
more GC cycles, less memory usage, more CPU spent on GC
```

## GOMEMLIMIT

`GOMEMLIMIT` sets a soft memory limit for the Go runtime.

Interview line:

```text
GOGC controls GC frequency based on heap growth, while GOMEMLIMIT gives the runtime a soft memory target, useful in containers and memory-constrained environments.
```

---

## 8. Slice Internals

A slice is a small header pointing to an underlying array.

Conceptually:

```go
type SliceHeader struct {
	Data uintptr
	Len  int
	Cap  int
}
```

Important:

```text
The slice header is small.
The actual elements live in the underlying array.
```

## Length vs Capacity

```go
s := make([]int, 0, 3)
```

```text
len = number of elements currently visible
cap = total available space before reallocation
```

## Append Within Capacity

```go
s := make([]int, 0, 3)
s = append(s, 1)
s = append(s, 2)
```

If capacity is available:

```text
append writes into the same underlying array
```

## Append Beyond Capacity

```go
s = append(s, 4, 5, 6, 7)
```

If capacity is exceeded:

```text
Go allocates a new backing array, copies old values, appends new values, and returns a new slice header.
```

Interview line:

```text
append may return a slice pointing to a new backing array, so always use the returned slice.
```

Correct:

```go
s = append(s, 10)
```

Wrong:

```go
append(s, 10)
```

## Slice Growth Rule

Modern Go uses a growth strategy roughly like:

```text
small slices grow close to 2x
larger slices grow more gradually
```

In current Go versions, the runtime uses a smoother growth curve after a threshold around 256.

Important senior nuance:

```text
The final capacity can be larger than the formula because allocation is rounded to allocator size classes.
```

Interview-safe answer:

```text
When append exceeds capacity, Go allocates a new backing array. Small slices usually grow around 2x, and larger slices grow more gradually to reduce memory waste. Actual capacity may be rounded by the allocator.
```

---

## 9. Slice Memory Leak Pattern

Problem:

```go
func firstTen(data []byte) []byte {
	return data[:10]
}
```

If `data` is very large, the returned small slice still points to the large backing array.

Fix:

```go
func firstTen(data []byte) []byte {
	out := make([]byte, 10)
	copy(out, data[:10])
	return out
}
```

Interview line:

```text
A small slice can keep a large backing array alive. If I need only a small part long-term, I copy it into a new slice.
```

---

## 10. Map Internals

A Go map is implemented using a runtime structure commonly explained as `hmap`.

Simple picture:

```text
hmap
  |
  v
bucket array
  |
  v
bmap bucket -> up to 8 key/value entries + overflow pointer
```

Important fields conceptually:

```text
count   -> number of live entries
B       -> log2 number of buckets, bucket count is 2^B
buckets -> pointer to bucket array
oldbuckets -> old bucket array during growth
```

## Bucket Structure

Each bucket stores up to 8 key/value pairs.

It also stores `tophash` values.

```text
tophash = top bits of hash used to quickly filter candidate keys inside a bucket
```

Memory layout is optimized.

Instead of:

```text
K1 V1 K2 V2 K3 V3
```

Go stores keys and values in grouped form roughly like:

```text
K1 K2 K3 ... V1 V2 V3 ...
```

This can reduce padding/alignment waste.

## Map Lookup

High-level flow:

1. Hash the key.
2. Use lower hash bits to find bucket.
3. Use `tophash` to quickly skip non-matching slots.
4. Compare actual keys for candidates.
5. Return value if found.

Interview line:

```text
Go maps use hashed buckets. Each bucket stores multiple entries, and tophash helps quickly identify possible matches before doing full key comparison.
```

## Map Growth

Maps grow when buckets become too full or overflow buckets become inefficient.

Go map growth is incremental.

Interview line:

```text
Go does not usually rehash the entire map in one huge pause. It incrementally evacuates buckets during future map operations.
```

---

## 11. Why Maps Are Not Thread-Safe

This is unsafe:

```go
m := map[string]int{}

go func() {
	m["a"] = 1
}()

go func() {
	m["b"] = 2
}()
```

Concurrent writes can corrupt internal map state.

The runtime detects some unsafe concurrent access and throws fatal errors like:

```text
fatal error: concurrent map writes
fatal error: concurrent map read and map write
```

Important:

```text
This is not a recoverable panic. It is a runtime fatal error.
```

Interview answer:

```text
Go maps are not safe for concurrent writes because writes can mutate buckets, overflow buckets, growth state, and internal flags. Use sync.Mutex, sync.RWMutex, or sync.Map depending on the access pattern.
```

## How To Make Map Access Safe

Use `sync.RWMutex`:

```go
type SafeCounter struct {
	mu sync.RWMutex
	m  map[string]int
}

func (s *SafeCounter) Get(key string) int {
	s.mu.RLock()
	defer s.mu.RUnlock()
	return s.m[key]
}

func (s *SafeCounter) Set(key string, value int) {
	s.mu.Lock()
	defer s.mu.Unlock()
	s.m[key] = value
}
```

Use `sync.Map` when:

```text
many goroutines read/write independent keys
read-heavy cache-like workload
you want built-in concurrent access behavior
```

For normal application maps:

```text
map + mutex is usually clearer.
```

---

## 12. How To Debug Memory and GC Issues

Useful commands:

```bash
go test -bench=. -benchmem ./...
```

```bash
go build -gcflags="-m" ./...
```

pprof heap:

```text
go tool pprof http://localhost:6060/debug/pprof/heap
```

pprof allocs:

```text
go tool pprof http://localhost:6060/debug/pprof/allocs
```

Runtime metrics to watch:

- heap allocation
- allocation rate
- GC pause time
- GC CPU fraction
- goroutine count
- object count

Interview line:

```text
For memory issues, I check heap profiles, allocation profiles, benchmark allocations, escape analysis output, and runtime metrics like heap growth and GC pause time.
```

---

## 13. SDE-3 Quick Interview Answers

### How does Go decide stack vs heap?

```text
The compiler runs escape analysis. If a value cannot outlive the function frame, it can stay on stack. If it may be referenced after the function returns or by another goroutine, it escapes to heap.
```

### Why does heap allocation matter?

```text
Heap allocations create allocator work and GC work. In hot paths, reducing heap allocations can improve throughput and latency.
```

### How does Go GC work?

```text
Go uses a concurrent tri-color mark-and-sweep GC. It marks reachable objects from roots, uses write barriers while the program mutates pointers, and sweeps unreachable objects. It runs mostly concurrently but still has short STW phases.
```

### What is a write barrier?

```text
A write barrier is runtime code that runs on pointer writes during GC marking. It keeps the GC's reachability view correct while application goroutines are changing object references.
```

### What happens when append exceeds slice capacity?

```text
Go allocates a new backing array, copies existing elements, appends the new value, and returns a new slice header pointing to the new array.
```

### Why can a small slice cause memory retention?

```text
Because a slice references an underlying array. A small slice can keep a large backing array alive until the small slice is no longer reachable.
```

### How is a Go map implemented?

```text
A Go map uses an hmap pointing to buckets. Each bucket holds multiple key/value entries and tophash values. Hash bits select the bucket, and tophash speeds up lookup inside the bucket.
```

### Why are maps not thread-safe?

```text
Concurrent writes can mutate internal buckets and growth state at the same time, corrupting the map. Use a mutex, RWMutex, or sync.Map.
```

## Summary Checklist

| Topic | Keywords To Mention |
|---|---|
| Escape analysis | stack, heap, outlives function, `go build -gcflags="-m"` |
| Allocator | `mcache`, `mcentral`, `mheap`, size classes |
| GC | tri-color, concurrent mark/sweep, write barrier, STW, `GOGC`, `GOMEMLIMIT` |
| Slices | header, pointer/len/cap, backing array, append growth, copy on retention |
| Maps | `hmap`, `bmap`, buckets, `tophash`, overflow buckets, incremental growth |
| Concurrency safety | map + mutex, `sync.RWMutex`, `sync.Map`, fatal concurrent writes |

## Memory Hooks

```text
Stack = fast, function scoped, no GC
Heap = shared/lifetime unknown, GC tracked

Escape = value may outlive current function
GC = find live, sweep dead
Write barrier = keep GC correct while program mutates pointers

Slice = pointer + len + cap
append beyond cap = new backing array
small slice can retain big array

Map = hmap -> buckets -> 8 slots + tophash
map writes need lock
```
