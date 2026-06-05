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

