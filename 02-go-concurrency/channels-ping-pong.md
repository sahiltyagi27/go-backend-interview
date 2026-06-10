# Channels Ping Pong

Interview question:

> Print `ping` and `pong` alternately using goroutines and channels.

This tests whether you understand unbuffered channel synchronization, goroutine ordering, and how to keep `main` alive until background work finishes.

## Code

```go
package main

import "fmt"

func main() {
	pingCh := make(chan struct{})
	pongCh := make(chan struct{})
	done := make(chan struct{})

	n := 5

	go func() {
		for i := 0; i < n; i++ {
			<-pingCh
			fmt.Println("ping")
			pongCh <- struct{}{}
		}
	}()

	go func() {
		for i := 0; i < n; i++ {
			<-pongCh
			fmt.Println("pong")
			if i < n-1 {
				pingCh <- struct{}{}
			}
		}
		done <- struct{}{}
	}()

	pingCh <- struct{}{}
	<-done
}
```

Output:

```text
ping
pong
ping
pong
ping
pong
ping
pong
ping
pong
```

## Explanation

`pingCh`, `pongCh`, and `done` are channels of `struct{}`. The empty struct carries no data, so it is used only as a signal.

The first goroutine waits for `pingCh`, prints `ping`, then signals `pongCh`.

The second goroutine waits for `pongCh`, prints `pong`, then signals `pingCh`.
On the final iteration, it does not signal `pingCh` again because the ping goroutine has already completed. Sending one extra signal would deadlock.

This line starts the sequence:

```go
pingCh <- struct{}{}
```

Without that initial send, both goroutines would wait forever.

The `done` channel keeps `main` alive until the second goroutine finishes.

## Interview Answer

> I used two unbuffered channels. Since unbuffered channels block until the other goroutine is ready, they act as synchronization points. The ping goroutine waits on `pingCh`, prints `ping`, then signals `pongCh`. The pong goroutine waits on `pongCh`, prints `pong`, then signals `pingCh` only if another round remains. The first send to `pingCh` starts the alternation, and `done` lets `main` wait for completion.

Important point:

> Unbuffered channel = synchronization point.

## Common Mistakes

- Forgetting the initial send, which causes both goroutines to block forever.
- Forgetting `done`, which can let `main` exit before goroutines finish.
- Sending one extra value after the other goroutine has already exited.
- Using `time.Sleep` instead of proper synchronization.

The naive version below prints the expected output but then deadlocks:

```go
for i := 0; i < n; i++ {
	<-pongCh
	fmt.Println("pong")
	pingCh <- struct{}{}
}
```

After the final `pong`, the ping goroutine has already completed, so nobody receives the last send on `pingCh`.
