# Interfaces, Pointers, Slices, and Maps

## Interfaces

An interface describes behavior through methods.

```go
type Store interface {
    Get(id string) (User, error)
}
```

Any type with matching methods satisfies it automatically.

```go
type PostgresStore struct{}

func (PostgresStore) Get(id string) (User, error) {
    return User{}, nil
}
```

## Empty Interface / any

`any` is an alias for `interface{}`.

```go
var x any = 10
var y interface{} = "hello"
```

It means no required methods, so every type satisfies it.

## Interface Internals: eface vs iface

At runtime, an interface value is not only the value. It also carries type information.

There are two important internal shapes:

```text
eface = empty interface: interface{} / any
iface = non-empty interface: interface with methods
```

## eface: Empty Interface

Used for:

```go
var x any = 10
var y interface{} = "hello"
```

Conceptually, an empty interface has two words:

```text
_type -> pointer to concrete type metadata
data  -> pointer to actual value data
```

Simple picture:

```text
interface{} / any
    |
    |-- _type -> int/string/User type metadata
    |-- data  -> actual value
```

Interview line:

```text
An empty interface stores the concrete type metadata and a pointer to the underlying data.
```

## iface: Non-Empty Interface

Used for interfaces with methods:

```go
type Reader interface {
	Read(p []byte) (int, error)
}
```

Conceptually, a non-empty interface has:

```text
tab  -> itab pointer
data -> pointer to actual value data
```

`itab` contains:

```text
interface type
concrete type
function pointers for methods
```

Simple picture:

```text
io.Reader
    |
    |-- tab  -> itab(interface type + concrete type + method table)
    |-- data -> actual value
```

Interview line:

```text
A non-empty interface stores an itab, which connects the interface type to the concrete type and method implementations, plus a data pointer.
```

Why this matters:

```text
It explains nil-interface surprises, dynamic dispatch, and why passing values through interfaces can sometimes cause allocations.
```

## Interface nil vs Concrete nil

An interface value has two parts:

- dynamic type
- dynamic value

This surprises people:

```go
var p *User = nil
var x any = p

fmt.Println(x == nil) // false
```

`x` is not nil because it contains a dynamic type `*User`, even though the dynamic value is nil.

## Interfaces for Testing

Interfaces let tests replace real dependencies.

```go
type EmailSender interface {
    Send(to, body string) error
}

type fakeEmailSender struct{}

func (fakeEmailSender) Send(to, body string) error {
    return nil
}
```

## Pointers

Use pointer receivers when:

- method mutates the receiver
- struct is large and copying is expensive
- type has sync fields like `sync.Mutex`
- you want consistent method sets

```go
func (u *User) Rename(name string) {
    u.Name = name
}
```

Use value receivers when:

- type is small
- method does not mutate
- value semantics are desirable

## Slices

A slice has:

- pointer to backing array
- length
- capacity

```go
s := []int{1, 2, 3}
fmt.Println(len(s), cap(s))
```

Append may reuse the same backing array or allocate a new one.

```go
a := []int{1, 2, 3}
b := a[:2]
b[0] = 99
fmt.Println(a) // [99 2 3]
```

## Slice Memory Leak

If you keep a tiny slice of a huge array, the huge array can stay in memory.

```go
small := huge[:10] // huge backing array is still referenced
```

Fix by copying:

```go
small := append([]byte(nil), huge[:10]...)
```

## Maps

Zero value map is nil. Reads are okay, writes panic.

```go
var m map[string]int
fmt.Println(m["x"]) // 0
m["x"] = 1          // panic
```

Initialize before writing:

```go
m := make(map[string]int)
```

Check key existence:

```go
value, ok := m["x"]
```

## Concurrent Map Access

Normal maps are not safe for concurrent writes.

Use:

- `sync.Mutex` around map
- `sync.RWMutex` for read-heavy maps
- `sync.Map` for special concurrent cache-like workloads

Interview line:

> Built-in maps are safe for concurrent reads only if no goroutine writes. Concurrent writes require synchronization.
