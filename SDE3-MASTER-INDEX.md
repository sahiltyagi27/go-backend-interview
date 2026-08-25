# SDE-3 Master Index

Main line:

```text
Detailed notes are the library.
This file is the map.
Do not read randomly. Pick one path and revise from here.
```

## How To Use This File

Use this as the first file for senior Go/backend prep.

If you have:

```text
30 minutes  -> read Quick Revision Path
2 hours     -> read one deep-dive track
1 full day  -> do Go runtime + backend systems + one DSA drill
Interview   -> read Final Interview Checklist
```

Do not open ten files at once. Open the linked file only when the current topic needs depth.

---

## SDE-3 Priority Order

For UKG / OCI / HCL BigFix / Trackier / Innovaccer style roles:

```text
1. Go runtime and concurrency
2. Backend API and production debugging
3. SQL, Redis, Kafka, microservices
4. System design and senior tradeoffs
5. DSA patterns and live coding
6. Resume/project storytelling
```

---

## Quick Revision Path

Use this when you are restarting after a break.

| Order | Topic | Read |
|---:|---|---|
| 1 | Go runtime memory | [Go memory runtime SDE-3 deep dive](01-go-fundamentals/go-runtime-memory-sde3-deep-dive.md) |
| 2 | Go scheduler/channels/leaks | [Go runtime concurrency SDE-3 deep dive](02-go-concurrency/go-runtime-sde3-deep-dive.md) |
| 3 | Practical concurrency | [Go concurrency README](02-go-concurrency/README.md) |
| 4 | SQL/indexes/transactions | [SQL, indexes, transactions](04-database-redis/sql-indexes-transactions.md) |
| 5 | Redis/caching | [Redis and caching](04-database-redis/redis-caching.md) |
| 6 | Kafka | [Kafka interview answers](05-microservices-distributed/kafka.md) |
| 7 | Observability/pprof | [Production debugging and pprof](07-observability-production/debugging-pprof.md) |
| 8 | System design cases | [System design cases README](08-system-design-cases/README.md) |
| 9 | DSA code recall | [Vanilla Go interview code](09-data-structures-algorithms/vanilla-go-interview-code.md) |
| 10 | Senior communication | [Career positioning](10-senior-engineer-skills/career-positioning.md) |

---

## Track 1: Go Runtime and Language Internals

This is the SDE-3 Go depth track.

| Topic | Why It Matters | Read |
|---|---|---|
| defer/panic/recover | cleanup, panic boundaries, LIFO behavior | [defer, panic, recover](01-go-fundamentals/defer-panic-recover.md) |
| interface internals | nil surprises, dynamic dispatch, allocations | [interfaces, pointers, slices, maps](01-go-fundamentals/interfaces-pointers-slices-maps.md) |
| escape analysis | stack vs heap, allocation pressure | [Go memory runtime SDE-3 deep dive](01-go-fundamentals/go-runtime-memory-sde3-deep-dive.md) |
| GC internals | latency, write barrier, tuning | [Go memory runtime SDE-3 deep dive](01-go-fundamentals/go-runtime-memory-sde3-deep-dive.md) |
| slices/maps internals | append, retention, map safety | [Go memory runtime SDE-3 deep dive](01-go-fundamentals/go-runtime-memory-sde3-deep-dive.md) |
| errors | production-safe error handling | [error handling](01-go-fundamentals/errors.md) |
| generics | modern Go APIs and reusable code | [generics](01-go-fundamentals/generics.md) |

Must be able to answer:

```text
How does escape analysis decide stack vs heap?
How does Go GC work?
What is a write barrier?
What happens when append exceeds capacity?
Why are maps not thread-safe?
What is eface vs iface?
Why does defer print old value sometimes?
```

---

## Track 2: Go Concurrency and Scheduler

This is the senior concurrency track.

| Topic | Why It Matters | Read |
|---|---|---|
| GMP scheduler | runtime scheduling depth | [Go runtime concurrency SDE-3 deep dive](02-go-concurrency/go-runtime-sde3-deep-dive.md) |
| work stealing | how Go keeps CPUs busy | [Go runtime concurrency SDE-3 deep dive](02-go-concurrency/go-runtime-sde3-deep-dive.md) |
| channel internals | `hchan`, `sendq`, `recvq`, parking | [Go runtime concurrency SDE-3 deep dive](02-go-concurrency/go-runtime-sde3-deep-dive.md) |
| goroutine leaks | production reliability | [Go runtime concurrency SDE-3 deep dive](02-go-concurrency/go-runtime-sde3-deep-dive.md) |
| graceful shutdown | OS signals, HTTP shutdown, worker cleanup | [graceful shutdown](02-go-concurrency/graceful-shutdown.md) |
| channel patterns | basic interview fluency | [channels ping-pong](02-go-concurrency/channels-ping-pong.md) |

Must be able to answer:

```text
What are G, M, and P?
What does GOMAXPROCS control?
What happens when a goroutine blocks on a channel?
What happens when a goroutine blocks in a syscall?
How does work stealing work?
How do goroutine leaks happen?
How do you detect leaks in production?
How do signal.Notify and signal.NotifyContext fit into graceful shutdown?
```

External practice repo:

```text
/Users/sahiltyagi/Desktop/personal projects/go-concurrency-demo
```

---

## Track 3: Backend API and Production Behavior

This is the daily work + interview maturity track.

| Topic | Why It Matters | Read |
|---|---|---|
| REST/JWT/pagination | API design basics | [REST, JWT, pagination](03-backend-api/rest-jwt-pagination.md) |
| idempotency/rate limiting/upload | senior API behavior | [idempotency, rate limiting, upload](03-backend-api/idempotency-rate-limit-upload.md) |
| production debugging | real incident handling | [Production debugging and pprof](07-observability-production/debugging-pprof.md) |
| distributed tracing | microservice observability | [Distributed tracing](07-observability-production/distributed-tracing.md) |
| incident communication | senior stakeholder handling | [Incident communication](07-observability-production/incident-communication.md) |

Must be able to answer:

```text
How do you design a rate limiter?
What is idempotency and where do you use it?
How do you debug a slow API?
What logs/metrics/traces do you check?
How do you communicate a production incident?
```

---

## Track 4: Database, Redis, and Caching

This is important for backend rounds and system design.

| Topic | Why It Matters | Read |
|---|---|---|
| SQL/indexes/transactions | core backend storage | [SQL, indexes, transactions](04-database-redis/sql-indexes-transactions.md) |
| Redis/cache patterns | scale, latency, rate limiting | [Redis and caching](04-database-redis/redis-caching.md) |

Must be able to answer:

```text
What is an index?
When does an index hurt?
What is a transaction?
What is isolation?
What is normalization vs denormalization?
What is cache-aside?
How do you handle cache invalidation?
What causes hot keys?
```

---

## Track 5: Microservices and Distributed Systems

This is the backend platform track.

| Topic | Why It Matters | Read |
|---|---|---|
| Kafka basics | async backend communication | [Kafka interview answers](05-microservices-distributed/kafka.md) |
| Kafka/DLQ/outbox | reliability and failure handling | [Kafka, DLQ, outbox](05-microservices-distributed/kafka-dlq-outbox.md) |
| retries/timeouts/circuit breaker | resilience | [Retries, timeouts, circuit breaker](05-microservices-distributed/retries-timeouts-circuit-breaker.md) |

Must be able to answer:

```text
What is topic, partition, offset, consumer group?
When do you commit offset?
How do retries and DLQ work?
How do you avoid duplicate processing?
What is the outbox pattern?
Why do we need timeouts and circuit breakers?
```

---

## Track 6: System Design Cases

Use these for HLD practice.

| Case | Read |
|---|---|
| Rate limiter | [rate limiter](08-system-design-cases/rate-limiter.md) |
| Payment retry system | [payment retry system](08-system-design-cases/payment-retry-system.md) |
| Analytics pipeline | [analytics pipeline](08-system-design-cases/analytics-pipeline.md) |
| Full case index | [system design cases README](08-system-design-cases/README.md) |

System design answer shape:

```text
Requirements
APIs
Data model
Core flow
Scale bottlenecks
Failure handling
Observability
Tradeoffs
```

Must be able to design:

```text
Rate limiter
URL shortener
Instagram/feed
Chat/messaging
Notification system
Payment retry system
Analytics pipeline
```

---

## Track 7: DSA and Live Coding

Use this for coding confidence, not endless random problems.

| Topic | Read |
|---|---|
| Vanilla Go interview snippets | [vanilla Go interview code](09-data-structures-algorithms/vanilla-go-interview-code.md) |
| Missing patterns | [missing patterns](09-data-structures-algorithms/missing-patterns.md) |
| Grid DFS problem | [grid DFS surrounded cities](09-data-structures-algorithms/grid-dfs-surrounded-cities.md) |
| DSA + system design checklist | [DSA + system design most asked](dsa-system-design-most-asked.md) |

Must code from memory:

```text
Two Sum
Valid Anagram
Valid Parentheses
Min Stack
BFS
DFS
Course Schedule / Kahn
Worker Pool
Rate Limiter
```

Default Go interview rules:

```text
Queue = []int
Stack = []int
Graph with 0..n-1 nodes = [][]int
Visited for 0..n-1 nodes = []bool
No custom stack/queue unless asked.
```

---

## Track 8: Senior Engineer Skills and LLD

This is the leadership/design-quality track.

| Topic | Read |
|---|---|
| Clean code, SOLID, patterns | [clean code, SOLID, design patterns](10-senior-engineer-skills/clean-code-solid-patterns.md) |
| Testing strategy | [testing strategy](10-senior-engineer-skills/testing-strategy.md) |
| DDD and event-driven design | [DDD and event-driven design](10-senior-engineer-skills/ddd-event-driven-design.md) |
| Capacity planning and architecture decisions | [capacity planning and architecture decisions](10-senior-engineer-skills/capacity-planning-architecture-decisions.md) |
| Career positioning | [career positioning](10-senior-engineer-skills/career-positioning.md) |

Must be able to answer:

```text
How do you structure a backend service?
How do you decide service boundaries?
How do you test backend code?
How do you make an architecture decision?
How do you explain tradeoffs?
How do you mentor or review code?
```

---

## Track 9: Role-Specific Prep

Use these only when that company/interview is active.

| Company/Role | Read |
|---|---|
| LTIM Round 2 | [LTIM round 2 prep](11-role-specific/ltim-round-2-prep.md) |
| HighLevel SDE-3 | [HighLevel SDE-3 prep](11-role-specific/highlevel.md) |
| GreyOrange | [GreyOrange prep](11-role-specific/greyorange.md) |
| MongoDB | [MongoDB prep](11-role-specific/mongodb.md) |
| Common role strategy | [common company prep strategy](11-role-specific/common-prep-strategy.md) |
| LTM AI proctored | [LTM AI proctored](11-role-specific/ltm-ai-proctored.md) |

Rule:

```text
Role-specific notes are for final targeting.
Core SDE-3 tracks are for building actual strength.
```

---

## Final Interview Checklist

Before any senior Go/backend interview, revise these:

```text
Go:
- GMP scheduler
- channels and goroutine leaks
- escape analysis
- GC and write barrier
- slices/maps internals
- defer/interface internals

Backend:
- REST, idempotency, pagination
- SQL indexes and transactions
- Redis caching
- Kafka offset/retry/DLQ/idempotency
- retries, timeouts, circuit breaker

Production:
- pprof
- race detector
- logs/metrics/traces
- incident communication

Coding:
- Worker pool
- Rate limiter
- Course Schedule
- Min Stack
- BFS/DFS

System Design:
- rate limiter
- URL shortener
- notification system
- analytics pipeline
- payment retry system
```

---

## 7-Day SDE-3 Restart Plan

Use this when coming back after a gap.

| Day | Focus | Output |
|---:|---|---|
| 1 | Go runtime memory | Explain escape analysis + GC aloud |
| 2 | Go scheduler/concurrency | Explain GMP + channel blocking + leaks |
| 3 | SQL + Redis | Write SQL examples + explain cache-aside |
| 4 | Kafka + distributed systems | Explain offset/retry/DLQ/outbox |
| 5 | System design | Do rate limiter or analytics pipeline |
| 6 | DSA live coding | Worker pool + Course Schedule + Min Stack |
| 7 | Mock day | Resume story + one system design + one coding |

Daily minimum:

```text
10 applications or pipeline work
45-60 minutes study
1 spoken answer
1 code/query recall
```

---

## What Not To Do

```text
Do not keep adding notes without linking them here.
Do not read role-specific notes before core tracks.
Do not study ten topics in one day.
Do not only read. Speak and code.
Do not wait for one offer. Apply + prepare.
```

## Current Command Center

Use these three files together:

| Purpose | File |
|---|---|
| What to read | [SDE-3 Master Index](SDE3-MASTER-INDEX.md) |
| Daily execution | [daily-board/today.md](daily-board/today.md) |
| Spaced revision | [revision-cycle.md](revision-cycle.md) |
