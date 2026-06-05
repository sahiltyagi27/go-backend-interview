# 07 - Observability and Production

## Topics From Both Checklists

### logging

- Structured logs
- Request IDs
- Log levels
- Avoid logging secrets

### metrics

- Request count
- Latency
- Error rate
- Saturation
- RED method

### tracing

- Distributed tracing
- Trace ID/span ID
- HTTP to Kafka to worker tracing

### production debugging

- Check logs
- Check metrics
- Check recent deploys
- Check dependency health
- Reproduce issue
- Root cause analysis

### memory/performance

- Go `pprof`
- CPU profile
- Memory profile
- Goroutine leaks
- Connection pool issues

---

## Existing Coverage

Partial coverage:

- goroutine leaks are covered in `/Users/sahiltyagi/Desktop/personal projects/go-concurrency-demo`

---

## Covered In This Folder

- `debugging-pprof.md`

This covers:

- production incident debugging checklist
- structured logging
- RED metrics
- trace propagation
- Go `pprof` CPU/memory/goroutine profiles
- connection pool debugging
