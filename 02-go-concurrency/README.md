# 02 - Go Concurrency

## Topics From Checklist

- goroutines
- channels
- buffered vs unbuffered channels
- closing channels
- reading from closed channel
- deadlocks
- `select`
- timeout using `time.After`
- cancellation using `ctx.Done()`
- `context`
- mutex vs channels
- worker pool
- race conditions
- `WaitGroup`
- graceful shutdown
- goroutine leaks

---

## Existing Coverage

Covered well in:

`/Users/sahiltyagi/Desktop/personal projects/go-concurrency-demo`

Important files:

- `pipeline/generator.go`
- `pipeline/processor.go`
- `pipeline/aggregator.go`
- `patterns/fanin.go`
- `patterns/fanout.go`
- `patterns/select_demo.go`
- `concepts/context.go`
- `concepts/mutex.go`
- `concepts/pitfalls.go`

---

## Covered In This Folder

- `graceful-shutdown.md`

This adds:

- stop accepting new HTTP requests
- wait for in-flight requests
- cancel background workers
- close DB/Kafka/Redis clients
