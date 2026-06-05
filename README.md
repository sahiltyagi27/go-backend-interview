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
| `01-go-fundamentals` | defer, panic/recover, interfaces, pointers, slices, maps, structs, errors |
| `02-go-concurrency` | goroutines, channels, select, context, mutex, worker pools, races, graceful shutdown |
| `03-backend-api` | REST, idempotency, JWT, middleware, rate limiting, pagination, file upload |
| `04-database-redis` | SQL, indexes, transactions, locking, normalization, NoSQL, Redis, caching |
| `05-microservices-distributed` | microservices, sync/async communication, retries, Kafka, DLQ, outbox, distributed locks |
| `06-cloud-devops` | Docker, Kubernetes, CI/CD, GCP, IAM |
| `07-observability-production` | logging, metrics, tracing, production debugging, pprof, memory/performance |
| `08-system-design-cases` | URL shortener, rate limiter, Instagram feed, chat, payments, notifications, analytics |
| `09-data-structures-algorithms` | data structures and algorithms repos |

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
