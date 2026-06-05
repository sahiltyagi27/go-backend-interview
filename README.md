# Go Backend Interview Prep

This folder segregates the complete Zoop.one / Golang backend interview checklist into focused study areas.

It does not replace the repos we already created. Instead, it acts as the master roadmap and points to:

- `go-concurrency-demo`
- `data structures`
- `algos`
- `system design`

---

## Folder Map

| Folder | Topics |
|---|---|
| [01-go-fundamentals](01-go-fundamentals/README.md) | defer, panic/recover, interfaces, pointers, slices, maps, structs, errors |
| [02-go-concurrency](02-go-concurrency/README.md) | goroutines, channels, select, context, mutex, worker pools, races, graceful shutdown |
| [03-backend-api](03-backend-api/README.md) | REST, idempotency, JWT, middleware, rate limiting, pagination, file upload |
| [04-database-redis](04-database-redis/README.md) | SQL, indexes, transactions, locking, normalization, NoSQL, Redis, caching |
| [05-microservices-distributed](05-microservices-distributed/README.md) | microservices, sync/async communication, retries, Kafka, DLQ, outbox, distributed locks |
| [06-cloud-devops](06-cloud-devops/README.md) | Docker, Kubernetes, CI/CD, GCP, IAM |
| [07-observability-production](07-observability-production/README.md) | logging, metrics, tracing, production debugging, pprof, memory/performance |
| [08-system-design-cases](08-system-design-cases/README.md) | URL shortener, rate limiter, Instagram feed, chat, payments, notifications, analytics |
| [09-data-structures-algorithms](09-data-structures-algorithms/README.md) | data structures and algorithms repos |

---

## Detailed Topic Links

| Topic | File |
|---|---|
| defer, panic, recover | [01-go-fundamentals/defer-panic-recover.md](01-go-fundamentals/defer-panic-recover.md) |
| interfaces, pointers, slices, maps | [01-go-fundamentals/interfaces-pointers-slices-maps.md](01-go-fundamentals/interfaces-pointers-slices-maps.md) |
| error handling | [01-go-fundamentals/errors.md](01-go-fundamentals/errors.md) |
| graceful shutdown | [02-go-concurrency/graceful-shutdown.md](02-go-concurrency/graceful-shutdown.md) |
| REST, JWT, pagination | [03-backend-api/rest-jwt-pagination.md](03-backend-api/rest-jwt-pagination.md) |
| idempotency, rate limiting, upload | [03-backend-api/idempotency-rate-limit-upload.md](03-backend-api/idempotency-rate-limit-upload.md) |
| SQL, indexes, transactions | [04-database-redis/sql-indexes-transactions.md](04-database-redis/sql-indexes-transactions.md) |
| Redis and caching | [04-database-redis/redis-caching.md](04-database-redis/redis-caching.md) |
| Kafka, DLQ, outbox | [05-microservices-distributed/kafka-dlq-outbox.md](05-microservices-distributed/kafka-dlq-outbox.md) |
| retries, timeouts, circuit breaker | [05-microservices-distributed/retries-timeouts-circuit-breaker.md](05-microservices-distributed/retries-timeouts-circuit-breaker.md) |
| Docker, Kubernetes, CI/CD, GCP | [06-cloud-devops/docker-kubernetes-cicd-gcp.md](06-cloud-devops/docker-kubernetes-cicd-gcp.md) |
| production debugging and pprof | [07-observability-production/debugging-pprof.md](07-observability-production/debugging-pprof.md) |
| rate limiter design | [08-system-design-cases/rate-limiter.md](08-system-design-cases/rate-limiter.md) |
| payment retry system | [08-system-design-cases/payment-retry-system.md](08-system-design-cases/payment-retry-system.md) |
| analytics pipeline | [08-system-design-cases/analytics-pipeline.md](08-system-design-cases/analytics-pipeline.md) |
| extra DSA patterns | [09-data-structures-algorithms/missing-patterns.md](09-data-structures-algorithms/missing-patterns.md) |

---

## Do We Need This New Folder?

Yes.

Reason:

- `go-concurrency-demo` is focused only on concurrency.
- `data structures` is focused only on data structures.
- `algos` is focused only on algorithms.
- `system design` is focused on architecture and case studies.
- The full checklist also includes backend/API, SQL/Redis, Kafka, Docker/Kubernetes, observability, debugging, and Go fundamentals.

So `go-backend-interview` is the right master folder for the remaining backend interview syllabus.

---

## Current Coverage Summary

Covered well:

- Go concurrency
- data structures
- algorithms
- core system design
- several system design cases

Now covered in this folder:

- Go fundamentals deep notes
- backend/API topics
- rate limiting
- Redis and caching
- idempotency
- SQL/indexes/transactions
- Kafka/DLQ/outbox
- Docker/Kubernetes/GCP
- production debugging
- Go pprof
- payment retry system
- analytics pipeline
