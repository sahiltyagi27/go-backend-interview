# Graceful Shutdown

Graceful shutdown means the service stops safely.

Steps:

1. Receive shutdown signal from OS.
2. Stop accepting new requests.
3. Let in-flight requests finish.
4. Cancel background workers.
5. Flush queues/logs if needed.
6. Close DB/Kafka/Redis clients.

Main interview line:

```text
Graceful shutdown starts when the process receives an OS signal like SIGINT or SIGTERM. Then the service stops accepting new requests, waits for in-flight work, cancels background workers, and closes external clients before exiting.
```

---

## Why signal.Notify Matters

In a real service, shutdown usually starts outside the Go app.

Examples:

```text
Ctrl+C locally                 -> SIGINT
Docker stop                    -> SIGTERM
Kubernetes pod termination     -> SIGTERM
systemd/process manager stop   -> SIGTERM
```

`signal.Notify` tells Go to deliver these OS signals into a channel.

```go
quit := make(chan os.Signal, 1)
signal.Notify(quit, os.Interrupt, syscall.SIGTERM)

<-quit
```

Meaning:

```text
Block until the process receives Ctrl+C or SIGTERM.
```

Why channel size 1?

```text
The signal channel is usually buffered with size 1 so signal delivery does not block if the program is not ready to receive at that exact moment.
```

Interview line:

```text
signal.Notify is the bridge between OS-level shutdown and application-level cleanup.
```

Common imports:

```go
import (
	"context"
	"log"
	"net/http"
	"os"
	"os/signal"
	"syscall"
	"time"
)
```

## HTTP Server Example

```go
srv := &http.Server{
    Addr:    ":8080",
    Handler: router,
}

go func() {
    if err := srv.ListenAndServe(); err != nil && err != http.ErrServerClosed {
        log.Fatal(err)
    }
}()

quit := make(chan os.Signal, 1)
signal.Notify(quit, os.Interrupt, syscall.SIGTERM)
<-quit

ctx, cancel := context.WithTimeout(context.Background(), 10*time.Second)
defer cancel()

if err := srv.Shutdown(ctx); err != nil {
    log.Println("shutdown error:", err)
}
```

What this does:

```text
ListenAndServe runs in a goroutine.
main waits for OS signal.
After signal arrives, Shutdown(ctx) stops accepting new requests.
Existing requests get time to finish.
If timeout expires, shutdown returns an error.
```

Important:

```text
http.Server.Shutdown does not kill active requests immediately.
It closes listeners, closes idle connections, and waits for active handlers until context timeout.
```

---

## Modern Option: signal.NotifyContext

Go also provides `signal.NotifyContext`.

It creates a context that is cancelled when an OS signal arrives.

```go
ctx, stop := signal.NotifyContext(
	context.Background(),
	os.Interrupt,
	syscall.SIGTERM,
)
defer stop()

<-ctx.Done()
log.Println("shutdown signal received")
```

Why this is clean:

```text
Instead of manually creating a signal channel, you get a context that can be passed into workers and shutdown logic.
```

Interview line:

```text
In modern Go, I often use signal.NotifyContext because it converts OS shutdown signals directly into context cancellation.
```

---

## Full Backend Shutdown Shape

```go
func main() {
	rootCtx, stop := signal.NotifyContext(context.Background(), os.Interrupt, syscall.SIGTERM)
	defer stop()

	srv := &http.Server{
		Addr:    ":8080",
		Handler: router,
	}

	go func() {
		if err := srv.ListenAndServe(); err != nil && err != http.ErrServerClosed {
			log.Fatal(err)
		}
	}()

	<-rootCtx.Done()
	log.Println("shutdown signal received")

	shutdownCtx, cancel := context.WithTimeout(context.Background(), 10*time.Second)
	defer cancel()

	if err := srv.Shutdown(shutdownCtx); err != nil {
		log.Println("http shutdown error:", err)
	}

	// Close DB, Redis, Kafka producers/consumers, loggers, etc.
}
```

Senior explanation:

```text
I use one context to listen for OS signals, and a separate timeout context for shutdown. The shutdown timeout prevents the service from hanging forever during cleanup.
```

## Background Workers

Use context cancellation.

```go
func worker(ctx context.Context, jobs <-chan Job) {
    for {
        select {
        case job, ok := <-jobs:
            if !ok {
                return
            }
            process(job)
        case <-ctx.Done():
            return
        }
    }
}
```

When shutdown starts:

```text
cancel context
stop accepting new jobs
close jobs channel if this service owns it
wait for workers with WaitGroup
close result/output channels from producer side
```

Worker-pool shutdown shape:

```go
func worker(ctx context.Context, jobs <-chan Job, wg *sync.WaitGroup) {
	defer wg.Done()

	for {
		select {
		case <-ctx.Done():
			return
		case job, ok := <-jobs:
			if !ok {
				return
			}
			process(job)
		}
	}
}
```

---

## What To Close During Shutdown

Usually close:

```text
HTTP server
DB connections
Redis clients
Kafka producers/consumers
background workers
metrics/log flushing
file handles
```

For Kafka consumers:

```text
Stop polling new messages.
Finish current message if possible.
Commit offset only after successful processing.
Close consumer.
```

For Kafka producers:

```text
Stop accepting new publish requests.
Flush pending messages if needed.
Close producer.
```

## Common Mistakes

- not listening to OS signals
- closing channels from the receiver side
- not using timeout for shutdown
- forgetting background goroutines
- accepting new traffic while shutting down
- not closing DB/Redis/Kafka clients
- using one context without a shutdown timeout
- committing Kafka offsets before processing is done
- assuming `defer cancel()` alone means graceful shutdown

## Interview Answer

```text
In a real Go service, I listen for SIGINT and SIGTERM using signal.Notify or signal.NotifyContext. Once a signal arrives, I stop accepting new HTTP requests with server.Shutdown(ctx), cancel background workers using context, stop taking new jobs, wait for in-flight work with WaitGroup, flush logs or Kafka producers if needed, and close DB/Redis/Kafka clients. I always use a timeout context so shutdown does not hang forever.
```

## Memory Hook

```text
signal.Notify = OS signal to Go channel
signal.NotifyContext = OS signal to context cancellation
server.Shutdown = stop new requests, wait for old ones
context = tell workers to stop
WaitGroup = wait for workers
timeout = avoid hanging forever
```
