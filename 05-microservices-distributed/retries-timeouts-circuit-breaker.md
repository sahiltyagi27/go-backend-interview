# Retries, Timeouts, Circuit Breakers, and Consistency

## Timeouts

Every network call needs a timeout.

Without timeout:

> One slow dependency can hang request goroutines and exhaust resources.

Go example:

```go
ctx, cancel := context.WithTimeout(parentCtx, 200*time.Millisecond)
defer cancel()

req = req.WithContext(ctx)
```

## Retries

Retries help temporary failures.

Use:

- max retry count
- exponential backoff
- jitter
- retry only safe errors
- idempotency keys

Bad:

```text
retry immediately from every server
```

This causes retry storms.

## Circuit Breaker

States:

- closed: normal calls
- open: dependency considered unhealthy, fail fast
- half-open: try limited calls to see if recovered

Benefit:

> Avoid wasting resources on a dependency that is already failing.

## Sync vs Async

Sync:

- REST
- gRPC
- caller waits for response

Async:

- Kafka
- queue
- event bus
- caller does not wait for processing

Tradeoff:

| Communication | Benefit | Cost |
|---|---|---|
| Sync | simple request/response | tight coupling, latency |
| Async | decoupled, resilient | eventual consistency, harder debugging |

## Eventual Consistency

Some data can be temporarily stale.

Good examples:

- like counts
- feed updates
- notifications
- analytics

Bad examples:

- payment balance
- inventory final commit
- driver assigned to two rides

