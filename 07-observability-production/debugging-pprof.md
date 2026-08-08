# Production Debugging and Go pprof

## Production Debugging Checklist

When an issue happens, move calmly through signals.

1. Check logs.
2. Check metrics.
3. Check recent deploys.
4. Check dependency health.
5. Reproduce if possible.
6. Identify blast radius.
7. Mitigate first if users are impacted.
8. Find root cause.
9. Add prevention.

## Logs

Use structured logs.

Fields:

- timestamp
- level
- request_id
- user_id if safe
- route
- status
- latency_ms
- error

Avoid logging:

- passwords
- tokens
- secrets
- full payment details

## Metrics

RED method:

- Rate: requests per second
- Errors: failure rate
- Duration: latency

Also watch saturation:

- CPU
- memory
- goroutine count
- DB connections
- queue depth

## Tracing

Tracing follows one request across services.

Example:

```text
HTTP request -> API -> Kafka -> Worker -> Database
```

Use trace ID and span ID to connect the path.

## Go pprof

Import:

```go
import _ "net/http/pprof"
```

Expose debug server:

```go
go func() {
    log.Println(http.ListenAndServe("localhost:6060", nil))
}()
```

CPU profile:

```bash
go tool pprof http://localhost:6060/debug/pprof/profile?seconds=30
```

Memory profile:

```bash
go tool pprof http://localhost:6060/debug/pprof/heap
```

Goroutine profile:

```bash
go tool pprof http://localhost:6060/debug/pprof/goroutine
```

## Race Detector

Go has a built-in data race detector.

Use it during tests:

```bash
go test -race ./...
```

Use it while running a program:

```bash
go run -race main.go
```

What it detects:

```text
Two goroutines access the same memory at the same time,
at least one access is a write,
and there is no synchronization.
```

Example race:

```go
var count int

go func() {
	count++
}()

go func() {
	count++
}()
```

Fix options:

- `sync.Mutex`
- `sync.RWMutex`
- channels when ownership transfer makes sense
- `sync/atomic` for simple counters/flags

Interview line:

```text
I use go test -race during development and CI to catch data races. If a shared variable is accessed by multiple goroutines, I protect it using mutexes, atomics, or channel ownership.
```

Senior nuance:

```text
The race detector adds overhead, so it is usually used in tests, CI, staging, or local debugging, not always enabled in production binaries.
```

## pprof Profiles To Know

CPU profile:

```text
Where CPU time is being spent.
Useful for hot loops, expensive JSON processing, compression, crypto, or inefficient algorithms.
```

Heap profile:

```text
What is currently retained in memory.
Useful for memory leaks or unexpected heap growth.
```

Allocs profile:

```text
Where allocations are happening over time.
Useful for allocation-heavy hot paths and GC pressure.
```

Goroutine profile:

```text
What goroutines exist and where they are blocked.
Useful for goroutine leaks, deadlocks, channel waits, DB waits, and stuck workers.
```

Block profile:

```text
Where goroutines are blocked waiting on synchronization.
Useful for channel/mutex contention.
```

Mutex profile:

```text
Where goroutines spend time waiting for locks.
Useful for lock contention.
```

Interview line:

```text
For performance issues, I first identify whether the symptom is CPU, memory, goroutine leak, blocking, or lock contention, then capture the matching pprof profile.
```

## Connection Pool Issues

Symptoms:

- requests hang
- DB latency increases
- many goroutines waiting
- max connections reached

Check:

- max open connections
- max idle connections
- connection lifetime
- slow queries
- leaked rows not closed

Go reminder:

```go
rows, err := db.QueryContext(ctx, query)
if err != nil {
    return err
}
defer rows.Close()
```
