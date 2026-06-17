# GreyOrange Backend / Platform Interview Prep

Use this note for GreyOrange-style backend/platform interviews.

## Fit Summary

Fit score:

```text
8/10 for backend/platform role
```

Why this is a good fit:

| GreyOrange stack | Your match |
|---|---|
| Golang | Strong match |
| Java / Python | Partial, can ramp up |
| Microservices | Good match |
| REST APIs | Strong match |
| Kafka | Strong match |
| MongoDB | Strong match |
| Redis | Strong match |
| PostgreSQL | Needs revision but manageable |
| GCP / AWS | GCP strong, AWS basic |
| Kubernetes / Docker | Needs focused prep |
| Cloud-native systems | Good direction |
| Distributed orchestration | Very relevant |

Best angle:

> Backend engineer for distributed, event-driven, cloud-native orchestration systems.

## HR Positioning

Use this:

> My strongest experience is backend engineering with Go, Node.js, Kafka, Redis, MongoDB, ClickHouse and GCP. I have worked on backend services, cloud migration, data pipelines and production troubleshooting. Since GreyOrange works on microservices, event-driven systems and cloud-native orchestration, I feel my backend experience aligns well with the role.

If robotics comes up:

> I have not worked directly in robotics, but I understand backend systems that coordinate services, events, state and APIs. I would be excited to apply that backend experience to warehouse automation and orchestration systems.

## Technical Positioning

Say this early when asked about your background:

> My backend strength is building services that move data reliably between APIs, queues, databases, caches and cloud systems. I am comfortable with Go services, Kafka-based event flows, Redis caching/rate limiting, MongoDB, GCP, observability and production troubleshooting.

Avoid saying:

```text
I know robotics.
```

Prefer:

```text
I understand backend orchestration and distributed systems.
```

## Priority Prep Topics

### 1. Go Concurrency

Revise:

- goroutines
- channels
- worker pool
- `context.Context`
- graceful shutdown
- `sync.WaitGroup`
- mutex vs channel
- goroutine leaks

Practice:

- [go-concurrency-demo](../../go-concurrency-demo/README.md)
- [Go concurrency decision guide](../../go-concurrency-demo/DECISION_GUIDE.md)
- [worker pool live coding drill](../../backend-live-coding-practice/go/worker-pool/README.md)

Interview line:

> I use goroutines for concurrent work, channels for communication, WaitGroup to wait for known goroutines, context for cancellation/timeouts, and worker pools when I need bounded concurrency.

### 2. Kafka

Revise:

- producer and consumer
- topics and partitions
- consumer groups
- ordering guarantees
- offsets
- retries
- DLQ
- idempotent consumer
- outbox pattern

Practice:

- [Kafka, DLQ, outbox](../05-microservices-distributed/kafka-dlq-outbox.md)

Interview line:

> Kafka is useful when services need asynchronous communication, durability, replay, and independent scaling. I design consumers to be idempotent because duplicate delivery can happen.

### 3. Redis

Revise:

- cache-aside
- TTL
- counters
- rate limiting
- sorted sets
- hot keys
- distributed lock basics

Practice:

- [Redis and caching](../04-database-redis/redis-caching.md)
- [rate limiter design](../08-system-design-cases/rate-limiter.md)
- [rate limiter live coding drill](../../backend-live-coding-practice/go/rate-limiter/README.md)

Interview line:

> Redis is useful for low-latency reads, counters, rate limiting and temporary state, but I do not treat cache expiry as exact business logic timing.

### 4. Microservices Reliability

Revise:

- service-to-service calls
- timeout on every network call
- retries with backoff and jitter
- circuit breaker
- idempotency
- tracing
- duplicate event handling
- graceful degradation

Practice:

- [retries, timeouts, circuit breaker](../05-microservices-distributed/retries-timeouts-circuit-breaker.md)
- [distributed tracing live coding drill](../../backend-live-coding-practice/go/tracing-context/README.md)

Interview line:

> In distributed systems, failures are normal. I use timeouts, retries with backoff, circuit breakers, idempotency and tracing so the system is reliable and debuggable.

### 5. Databases

Revise:

- MongoDB vs PostgreSQL
- indexes
- transactions
- schema design
- query patterns
- consistency requirements

Practice:

- [SQL, indexes, transactions](../04-database-redis/sql-indexes-transactions.md)

MongoDB vs PostgreSQL:

```text
MongoDB: flexible document model, good for nested/variable shape data.
PostgreSQL: relational integrity, joins, transactions, structured querying.
```

Interview line:

> I choose the database based on access patterns, consistency needs, relationships and query complexity, not only based on popularity.

### 6. Cloud / Kubernetes

Revise:

- Docker image vs container
- Kubernetes Pod
- Deployment
- Service
- Ingress
- ConfigMap
- Secret
- HPA
- logs and metrics
- graceful shutdown in containers

Practice:

- [Docker, Kubernetes, CI/CD, GCP](../06-cloud-devops/docker-kubernetes-cicd-gcp.md)

Interview line:

> I know the deployment basics: containerize service, configure environment through ConfigMaps/Secrets, expose through Service/Ingress, scale with replicas/HPA, and shut down gracefully on termination.

## Likely Interview Questions

```text
How would you design a task assignment system for many robots/workers?
How do microservices communicate?
How do you avoid duplicate event processing?
How does a Kafka consumer group work?
How would you retry failed tasks?
How would you trace a request across services?
How would you design a rate limiter?
How do you handle graceful shutdown in Go?
How would you cache inventory/location data?
MongoDB vs PostgreSQL: when would you use each?
How would you debug a production issue after deployment?
What happens when a consumer crashes after processing but before committing offset?
How would you prevent retry storms?
How would you handle stale robot/worker state?
```

## Strong System Design Case

Question:

> Design a task assignment system where many robots/workers pick, move or process warehouse tasks reliably.

High-level components:

```text
API Gateway
Task Service
Scheduler / Orchestrator
Worker / Robot State Service
Kafka
Redis
PostgreSQL or MongoDB
Observability stack
```

Basic flow:

```text
1. Task is created through API or upstream system.
2. Task Service stores durable task record.
3. Task-created event is published to Kafka.
4. Scheduler consumes task events.
5. Scheduler checks worker/robot availability and location/state.
6. Scheduler assigns task and emits task-assigned event.
7. Worker/robot sends status updates.
8. Task reaches completed, failed, timed-out or retry state.
```

Key design points:

- Store durable task state in PostgreSQL/MongoDB.
- Use Kafka for async events.
- Use Redis for fast worker/robot state, heartbeat and temporary assignment cache.
- Use idempotency keys to avoid duplicate task creation.
- Make consumers idempotent because duplicate events can happen.
- Add retry and DLQ for failed task processing.
- Add timeouts for stale assignments.
- Add tracing using trace ID and span ID across services.
- Add metrics for task latency, failure rate, assignment delay and queue depth.

Deep dives to be ready for:

```text
How do you avoid assigning the same task twice?
How do you handle worker/robot heartbeat loss?
How do you retry failed tasks safely?
How do you preserve ordering where needed?
How do you monitor task stuck states?
How do you scale the scheduler?
```

## Salary Stance

Target:

```text
24-28 LPA
```

Good:

```text
25-26 LPA
```

Stretch:

```text
28 LPA
```

Minimum:

```text
22-24 LPA depending on role scope and work model
```

If it is 5 days Gurgaon:

> I would be looking closer to 25-28 LPA because the work model has a higher location and commute cost.

If it is hybrid:

> 24-26 LPA is reasonable depending on the full compensation structure.

Early HR answer:

> I am flexible for the right backend/platform role, but based on my experience and the role expectations, I am looking in the 25-28 LPA range.

## 2-Day Quick Prep Plan

### Day 1

- Go worker pool with graceful shutdown
- Kafka consumer group, partitions, retries, DLQ
- Redis cache-aside and rate limiter
- Microservices retries, timeouts, circuit breaker

### Day 2

- Task assignment system design
- MongoDB vs PostgreSQL
- Docker/Kubernetes basics
- Production debugging story
- HR positioning and salary answer

## Final Positioning

> I am not positioning myself as a robotics engineer. I am positioning myself as a backend engineer who can build reliable services for distributed, event-driven orchestration systems. That is where my Go, Kafka, Redis, MongoDB, GCP and production debugging experience fits GreyOrange well.
