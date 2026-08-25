# Go Runtime Deep Dive for SDE-3

Main line:

```text
At SDE-3 level, do not only say "goroutines are lightweight".
Explain how the Go runtime schedules, parks, wakes, and prevents leaks.
```

## Why This Matters

Basic answer:

```text
Goroutines are lightweight threads managed by the Go runtime.
```

Senior answer:

```text
Go uses its own scheduler to multiplex many goroutines over fewer OS threads. It parks goroutines when they block on channels or I/O, wakes them when work is available, and uses work stealing to keep CPUs busy.
```

This helps in interviews because it shows you understand:

- why goroutines are cheap
- why blocked goroutines do not always block OS threads
- how channel blocking works internally
- why goroutine leaks happen
- how to design cancellation-safe concurrent code

---

## 1. GMP Scheduler

Go uses a scheduler commonly explained as the GMP model.

```text
G = Goroutine
M = Machine / OS thread
P = Processor / logical execution context
```

## Best Interview Answer

```text
Go uses an M:N scheduler called the GMP model, where G, M, and P are runtime structures under the hood.

G is a goroutine with its own dynamic stack and execution state, M represents a physical OS thread, and P is the logical execution context limited by GOMAXPROCS. An OS thread, M, must hold a P to execute user Go code.

Each P has its own local run queue and memory cache. If an M gets stuck in a heavy blocking syscall, Go detaches the P and hands it to another thread so the remaining goroutines keep running.
```

Simple picture:

```text
                 Global Run Queue
                         |
              -----------------------
              |                     |
             P1                    P2
        Local Run Queue       Local Run Queue
              |                     |
             M1                    M2
              |                     |
             G1                    G2
```

## What Is G?

`G` means goroutine.

It stores runtime state such as:

- stack
- instruction pointer
- status: runnable, running, waiting, dead
- information needed to resume execution later

Interview line:

```text
A goroutine is not an OS thread. It is a lightweight unit of execution managed by the Go runtime.
```

Goroutines start with a small stack, around a few KB, and the stack can grow/shrink as needed.

## What Is M?

`M` means machine.

It represents an actual OS thread.

Interview line:

```text
M is the real operating system thread on which Go code eventually runs.
```

## What Is P?

`P` means processor.

It is not a CPU core directly. It is the Go runtime's logical execution context required to run Go code.

The number of active Ps is controlled by `GOMAXPROCS`.

Interview line:

```text
P owns a local run queue and is required for an M to execute Go code. GOMAXPROCS controls how many Ps can execute Go code in parallel.
```

## GOMAXPROCS

`GOMAXPROCS` controls how many OS threads can execute Go code at the same time.

Example:

```go
runtime.GOMAXPROCS(4)
```

This means up to 4 Ps can run Go code in parallel.

Important:

```text
GOMAXPROCS does not limit the total number of goroutines.
It limits parallel execution of Go code.
```

---

## 2. Local Run Queue, Global Run Queue, and Work Stealing

Each P has a local run queue.

```text
P1 -> local runnable goroutines
P2 -> local runnable goroutines
```

There is also a global run queue shared by the scheduler.

When a goroutine becomes runnable, the runtime usually tries to put it on a local queue. Sometimes it goes to the global queue.

## Work Stealing

If one P runs out of work:

1. It checks its own local run queue.
2. It checks the global run queue.
3. If still empty, it steals work from another P's local run queue.

Interview line:

```text
Work stealing helps keep all processors busy. If one P has no goroutines to run, it can steal runnable goroutines from another P's local queue.
```

Why steal half?

```text
Stealing a batch reduces scheduling overhead and balances work faster than stealing one goroutine at a time.
```

---

## 3. What Happens During Blocking Syscalls?

Example:

```text
G1 is running on M1 using P1.
G1 makes a blocking syscall.
```

What happens:

1. `G1` and `M1` go into the blocking syscall.
2. `P1` is detached from `M1`.
3. Another idle/new thread `M2` takes `P1`.
4. `M2` continues running other goroutines from `P1`'s queue.

Interview line:

```text
If a goroutine blocks in a syscall, the runtime can detach the P from that blocked OS thread and let another thread continue running Go code. This keeps the program from wasting available CPU.
```

Important nuance:

```text
The goroutine doing the syscall is blocked.
The whole Go program is not blocked.
```

---

## 4. Channel Internals

Under the hood, a Go channel is represented by a runtime structure often explained as `hchan`.

Important parts:

```text
buf     -> circular buffer for buffered channels
lock    -> mutex protecting channel state
sendq   -> queue of goroutines waiting to send
recvq   -> queue of goroutines waiting to receive
qcount  -> number of elements currently in buffer
dataqsiz -> buffer capacity
sendx   -> send index in circular buffer
recvx   -> receive index in circular buffer
```

Simple picture:

```text
                 hchan
    --------------------------------
    lock     : protects channel state
    buf      : circular buffer
    sendq    : blocked senders
    recvq    : blocked receivers
    qcount   : current buffered elements
    dataqsiz : buffer capacity
    sendx    : next send position
    recvx    : next receive position
    --------------------------------
```

## Buffered Channel

```go
ch := make(chan int, 3)
```

This channel can hold 3 values without an immediate receiver.

```text
send succeeds while buffer has space
send blocks when buffer is full
receive succeeds while buffer has values
receive blocks when buffer is empty
```

## Unbuffered Channel

```go
ch := make(chan int)
```

This channel has no storage.

Send and receive must meet.

Interview line:

```text
An unbuffered channel is synchronous. The sender and receiver rendezvous at the same time.
```

---

## 5. What Happens When Sending to a Full Buffered Channel?

Example:

```go
ch := make(chan int, 1)
ch <- 10
ch <- 20 // blocks until someone receives
```

Runtime behavior:

1. Goroutine tries to send.
2. Runtime locks the channel.
3. Buffer is full.
4. Runtime wraps the goroutine in a wait structure called `sudog`.
5. The goroutine is added to `sendq`.
6. Runtime calls `gopark`.
7. The goroutine is parked and stops running.
8. The M can run another goroutine.

Important:

```text
The goroutine is blocked, but it is not spinning CPU.
The OS thread is not necessarily blocked.
```

Interview answer:

```text
When sending to a full buffered channel, only that goroutine is parked. The runtime puts it in the channel's send queue and schedules another runnable goroutine. It does not waste CPU by spinning.
```

---

## 6. What Happens When Receiving From an Empty Channel?

Example:

```go
ch := make(chan int)
v := <-ch // blocks until someone sends
```

Runtime behavior:

1. Goroutine tries to receive.
2. Runtime locks the channel.
3. No value is available.
4. Runtime wraps the goroutine in `sudog`.
5. The goroutine is added to `recvq`.
6. Runtime calls `gopark`.
7. The goroutine waits until a sender arrives.
8. Sender later makes it runnable using `goready`.

Interview answer:

```text
Receiving from an empty channel parks the receiver goroutine in the channel's receive queue. When a sender arrives, the runtime transfers the value and wakes the receiver.
```

## Does This Cause Deadlock?

Not immediately.

Blocking on a channel blocks one goroutine.

Program-level deadlock happens when:

```text
all goroutines are asleep and no goroutine can make progress
```

Example:

```go
func main() {
	ch := make(chan int)
	ch <- 1
}
```

Here `main` blocks sending, and no other goroutine can receive.

That causes:

```text
fatal error: all goroutines are asleep - deadlock!
```

---

## 7. Close Channel Behavior

Close means:

```text
No more values will be sent.
```

Only the sender should close the channel.

Reading from a closed channel:

```go
v, ok := <-ch
```

If channel is closed and empty:

```text
v  = zero value
ok = false
```

Sending to a closed channel:

```text
panic
```

Closing an already closed channel:

```text
panic
```

Interview line:

```text
Close is a signal from sender to receiver that no more values are coming. Receivers should not close a channel they do not own.
```

---

## 8. Goroutine Leak

A goroutine leak happens when a goroutine remains blocked forever and can never complete.

Common leak patterns:

- sending on a channel after the receiver has returned
- receiving forever from a channel that is never closed
- background goroutine ignores context cancellation
- ticker is not stopped
- worker waits forever because job channel is never closed
- HTTP/database call has no timeout

Interview line:

```text
A goroutine leak is when a goroutine is still alive but no longer useful. It usually happens because it is blocked forever on a channel, I/O, or missing cancellation.
```

---

## 9. Classic Leak Example: Timeout Wins Before Send

Problem:

```go
func fetchUserData(ctx context.Context, userID string) (*UserData, error) {
	ch := make(chan *UserData)

	go func() {
		data := queryDatabase(userID)
		ch <- data
	}()

	select {
	case res := <-ch:
		return res, nil
	case <-ctx.Done():
		return nil, ctx.Err()
	}
}
```

Why this leaks:

1. `ch` is unbuffered.
2. `ctx.Done()` wins first.
3. `fetchUserData` returns.
4. The background goroutine finishes `queryDatabase`.
5. It tries `ch <- data`.
6. No receiver exists anymore.
7. The goroutine blocks forever.

Interview line:

```text
The receiver returned due to timeout, but the sender still tried to send on an unbuffered channel. Since nobody can receive, the goroutine leaks.
```

---

## 10. Fix 1: Buffered Channel of Size 1

```go
func fetchUserData(ctx context.Context, userID string) (*UserData, error) {
	ch := make(chan *UserData, 1)

	go func() {
		data := queryDatabase(userID)
		ch <- data
	}()

	select {
	case res := <-ch:
		return res, nil
	case <-ctx.Done():
		return nil, ctx.Err()
	}
}
```

Why this helps:

```text
Even if the receiver returns, the goroutine can place one result into the buffer and exit.
```

Important limitation:

```text
This does not cancel queryDatabase itself.
If queryDatabase takes very long, the goroutine still remains busy until the query returns.
```

---

## 11. Fix 2: Context-Aware Send

```go
func fetchUserData(ctx context.Context, userID string) (*UserData, error) {
	ch := make(chan *UserData, 1)

	go func() {
		data := queryDatabase(userID)

		select {
		case ch <- data:
		case <-ctx.Done():
			return
		}
	}()

	select {
	case res := <-ch:
		return res, nil
	case <-ctx.Done():
		return nil, ctx.Err()
	}
}
```

This protects the send.

Better senior answer:

```text
I would make both the query and the send context-aware. The buffered channel prevents a send leak, and passing context into the database call prevents wasted work.
```

---

## 12. Best Fix: Make the Operation Context-Aware

Best shape:

```go
func fetchUserData(ctx context.Context, userID string) (*UserData, error) {
	return queryDatabase(ctx, userID)
}
```

For actual database calls, prefer APIs that accept context:

```go
row := db.QueryRowContext(ctx, "SELECT id, name FROM users WHERE id = ?", userID)
```

Interview line:

```text
The best fix is to propagate context into the actual blocking operation. A buffered channel only prevents the send from leaking; it does not stop the underlying work.
```

---

## 13. How To Detect Goroutine Leaks

Practical tools:

```go
runtime.NumGoroutine()
```

Use it in tests or debug logs to see whether goroutine count keeps increasing.

Production tools:

- pprof goroutine profile
- metrics for goroutine count
- logs around worker start/stop
- timeouts in tests
- load tests watching goroutine growth

pprof command idea:

```text
go tool pprof http://localhost:6060/debug/pprof/goroutine
```

Interview answer:

```text
I would look at goroutine count over time, take a pprof goroutine dump, check where goroutines are blocked, and then fix missing cancellation, unclosed channels, or blocked sends/receives.
```

---

## 14. SDE-3 Quick Interview Answers

### Explain the Go scheduler.

```text
Go uses an M:N scheduler. Many goroutines are multiplexed over OS threads. G represents goroutine state, M is the OS thread, and P is the logical processor required to run Go code. Each P has a local run queue, there is also a global queue, and the scheduler uses work stealing to keep processors busy.
```

### Is a goroutine the same as an OS thread?

```text
No. A goroutine is managed by the Go runtime. OS threads are heavier and managed by the kernel. The runtime schedules many goroutines over fewer OS threads.
```

### What happens if a goroutine blocks on a channel?

```text
The goroutine is parked by the runtime. It is added to the channel's send or receive queue and stops consuming CPU. Another runnable goroutine can use the thread.
```

### What happens when sending to a full buffered channel?

```text
The sender goroutine is parked in the channel's send queue until buffer space is available or a receiver arrives. It does not spin CPU.
```

### What happens when receiving from an empty channel?

```text
The receiver goroutine is parked in the channel's receive queue until a sender provides a value.
```

### How do you avoid goroutine leaks?

```text
Use context cancellation, timeouts, close channels from the sender side, make blocking operations context-aware, avoid sends when the receiver may exit, stop tickers, and ensure workers have a clear shutdown path.
```

### How do you debug goroutine leaks?

```text
Track goroutine count, take pprof goroutine profiles, inspect blocked stack traces, and look for goroutines stuck on channel send/receive, network calls, mutexes, or tickers.
```

## Memory Hooks

```text
G = goroutine state
M = OS thread
P = permission/context to run Go code

Channel full send = sender parked in sendq
Channel empty receive = receiver parked in recvq

gopark = sleep this goroutine
goready = make this goroutine runnable again

Leak = goroutine alive but no longer useful
Best leak fix = context-aware operation + safe send/receive path
```
