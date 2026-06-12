# Go Backend Interview Roadmap

Visual reference:

- [Eraser roadmap workspace](https://app.eraser.io/workspace/KSd4MA0EwyE2iNbvpl45)

Note: the Eraser workspace is the visual source, but this Markdown file is the searchable revision version.

---

## Goal

Prepare for Golang backend and senior software engineer interviews by covering:

- Go language fundamentals
- Go concurrency
- backend/API design
- SQL, Redis, and caching
- microservices and distributed systems
- cloud/devops basics
- observability and production debugging
- system design cases
- data structures and algorithms
- senior engineer skills

High-signal revision checklist:

- [DSA + system design most asked](dsa-system-design-most-asked.md)

For 5 YOE Go backend roles, do not treat DSA as the whole interview. The practical priority is:

```text
1. Go + backend fundamentals
2. System design
3. DSA patterns
4. Resume project explanation
```

---

## Phase 1 - Go Fundamentals

Start here if Go syntax and behavior are not automatic yet.

Read:

- [defer, panic, recover](01-go-fundamentals/defer-panic-recover.md)
- [interfaces, pointers, slices, maps](01-go-fundamentals/interfaces-pointers-slices-maps.md)
- [error handling](01-go-fundamentals/errors.md)
- [generics](01-go-fundamentals/generics.md)

Must know:

- `defer` LIFO order
- `panic` vs `error`
- why `recover` only works inside deferred functions
- interface nil vs concrete nil
- `any` vs `interface{}`
- pointer vs value receiver
- slice length/capacity/backing array
- map key existence and concurrent map access
- error wrapping with `%w`
- `errors.Is` and `errors.As`
- generic functions with `T any`
- generic types and constraints

---

## Phase 2 - Go Concurrency

Read:

- [go-concurrency-demo repo](../go-concurrency-demo/README.md)
- [ping-pong with channels](02-go-concurrency/channels-ping-pong.md)
- [graceful shutdown](02-go-concurrency/graceful-shutdown.md)

Must know:

- goroutines
- channels
- buffered vs unbuffered channels
- read-only and write-only channel syntax
- `select`
- `context.Context`
- `sync.WaitGroup`
- worker pool
- fan-in and fan-out
- mutex vs channels
- race conditions
- goroutine leaks
- graceful shutdown

Interview drill:

> Explain how two unbuffered channels can force `ping` and `pong` to print in order.

Practice:

```bash
cd "/Users/sahiltyagi/Desktop/personal projects/go-concurrency-demo"
go run .
go run . -timeout
go run -race . -race-demo
```

---

## Phase 3 - Backend/API

Read:

- [REST, JWT, pagination](03-backend-api/rest-jwt-pagination.md)
- [idempotency, rate limiting, upload](03-backend-api/idempotency-rate-limit-upload.md)

Must know:

- REST methods and status codes
- API versioning
- JWT validation
- middleware pattern
- idempotency keys
- safe retry design
- fixed window, sliding window, token bucket, leaky bucket
- offset vs cursor pagination
- presigned upload URLs

Interview line:

> Design APIs to be predictable, secure, idempotent where needed, and easy to operate at scale.

---

## Phase 4 - Database, Redis, and Caching

Read:

- [SQL, indexes, transactions](04-database-redis/sql-indexes-transactions.md)
- [Redis and caching](04-database-redis/redis-caching.md)
- [backend interview reality check](backend-interview-reality-check.md)

Must know:

- joins
- constraints and foreign keys
- B-tree indexes
- composite indexes
- when indexes hurt writes
- ACID
- transaction isolation
- optimistic and pessimistic locking
- SQL vs NoSQL tradeoffs
- Redis TTL, counters, sorted sets
- cache-aside, write-through, write-behind
- hot keys
- distributed lock risks
- cache invalidation on writes
- TTL as eventual cleanup, not exact business timing

---

## Phase 5 - Microservices and Distributed Systems

Read:

- [Kafka, DLQ, outbox](05-microservices-distributed/kafka-dlq-outbox.md)
- [retries, timeouts, circuit breaker](05-microservices-distributed/retries-timeouts-circuit-breaker.md)
- [backend interview reality check](backend-interview-reality-check.md)

Must know:

- why split services
- service ownership
- REST/gRPC for sync communication
- Kafka/queue for async communication
- retries with backoff and jitter
- retry storms
- timeouts on every network call
- circuit breakers
- eventual consistency
- Kafka topics, partitions, offsets, consumer groups
- at-most-once vs at-least-once
- idempotent consumers
- DLQ
- outbox pattern
- message queue vs Redis Pub/Sub
- consumer crash and redelivery behavior

---

## Phase 6 - Cloud and DevOps

Read:

- [Docker, Kubernetes, CI/CD, GCP](06-cloud-devops/docker-kubernetes-cicd-gcp.md)

Must know:

- image vs container
- Dockerfile
- Docker Compose
- Kubernetes Pod, Deployment, Service, Ingress
- ConfigMap and Secret
- HPA
- CI/CD pipeline steps
- GCP basics: Compute, Cloud Storage, Pub/Sub, Bigtable, Logging, Monitoring, IAM

---

## Phase 7 - Observability and Production Debugging

Read:

- [production debugging and pprof](07-observability-production/debugging-pprof.md)

Must know:

- structured logs
- request IDs
- log levels
- RED metrics
- tracing
- production debugging checklist
- recent deploy checks
- dependency health checks
- root cause analysis
- Go `pprof`
- CPU profile
- memory profile
- goroutine profile
- connection pool issues

---

## Phase 8 - System Design Cases

Read:

- [system-design repo](../system%20design/README.md)
- [DSA + system design most asked](dsa-system-design-most-asked.md)
- [rate limiter](08-system-design-cases/rate-limiter.md)
- [payment retry system](08-system-design-cases/payment-retry-system.md)
- [analytics pipeline](08-system-design-cases/analytics-pipeline.md)

Core cases:

- URL shortener
- rate limiter
- Instagram feed
- chat/messaging
- notification system
- payment retry system
- analytics pipeline
- video upload/transcoding
- e-commerce checkout

System design flow:

1. Clarify requirements.
2. Estimate scale.
3. Define APIs.
4. Define data model.
5. Draw high-level architecture.
6. Deep dive on the hardest part.
7. Discuss failures and tradeoffs.
8. Add observability.

---

## Phase 9 - Data Structures and Algorithms

Read:

- [data-structures repo](../data%20structures/README.md)
- [algos repo](../algos/README.md)
- [DSA + system design most asked](dsa-system-design-most-asked.md)
- [extra DSA patterns](09-data-structures-algorithms/missing-patterns.md)
- [grid DFS surrounded cities](09-data-structures-algorithms/grid-dfs-surrounded-cities.md)

Must know:

- arrays and slices
- maps and sets
- stack and queue
- linked list
- tree
- graph
- heap
- binary search
- two pointers
- sliding window
- recursion
- BFS/DFS
- dynamic programming
- greedy
- backtracking

LeetCode practice:

- Two Sum
- Valid Parentheses
- Longest Substring Without Repeating Characters
- Container With Most Water
- Trapping Rain Water
- Top K Frequent Elements
- Course Schedule
- Surrounded cities/grid DFS
- LRU/LFU Cache
- Median from Data Stream
- Word Ladder
- Merge K Sorted Lists
- Maximum Subarray Sum
- Kth Largest in Stream

Daily loop:

```text
1 DSA problem
1 backend/system design topic
1 Go/backend revision topic
```

---

## Phase 10 - Senior Engineer Skills

Read:

- [clean code, SOLID, design patterns](10-senior-engineer-skills/clean-code-solid-patterns.md)
- [testing strategy](10-senior-engineer-skills/testing-strategy.md)
- [DDD and event-driven design](10-senior-engineer-skills/ddd-event-driven-design.md)
- [capacity planning and architecture decisions](10-senior-engineer-skills/capacity-planning-architecture-decisions.md)
- [career positioning](10-senior-engineer-skills/career-positioning.md)
- [AI skills](10-senior-engineer-skills/ai-skills.md)

Must know:

- clean code habits
- SOLID in Go
- useful backend design patterns
- unit/integration/E2E testing strategy
- DDD terms: entity, value object, aggregate, bounded context
- event-driven design
- capacity planning
- architecture decision records
- well-architected thinking
- project storytelling
- AI-assisted development
- prompt engineering
- RAG
- MCP

---

## 7-Day Revision Plan

### Day 1

- Go fundamentals
- errors
- slices/maps/interfaces/pointers

### Day 2

- Go concurrency
- worker pools
- context
- graceful shutdown

### Day 3

- REST/JWT/idempotency
- rate limiting
- pagination

### Day 4

- SQL/indexes/transactions
- Redis/caching

### Day 5

- Kafka/DLQ/outbox
- retries/timeouts/circuit breaker

### Day 6

- system design cases
- Instagram feed
- payment retry
- analytics pipeline

### Day 7

- production debugging
- pprof
- senior engineer skills
- resume/project storytelling

---

## Interview Readiness Checklist

You are ready when you can explain these without reading:

- how Go interfaces work
- how slices share backing arrays
- how to stop goroutines safely
- how a worker pool works
- how to design an idempotent API
- when to use cursor pagination
- how SQL indexes work
- how Redis cache-aside works
- what Kafka partitions and consumer groups are
- why DLQ and outbox patterns exist
- how to debug a production incident
- how to design Instagram feed
- how to discuss tradeoffs clearly
