# HighLevel SDE-3 Prep

Main positioning:

```text
Production owner, not just SDE.
```

HighLevel-style backend work is about SaaS scale, CRM workflows, messaging, webhooks, tenant isolation, automation, reliability, and production ownership.

Your best angle:

```text
I have around 5 years of backend experience building production systems in Go, Kafka, Redis, MongoDB, ClickHouse, and GCP. I have worked on event-driven services, data pipelines, URL shortener/analytics systems, cloud migration, and production debugging. For HighLevel, I can map that experience to CRM automation, webhooks, notifications, workflow execution, and multi-tenant SaaS reliability.
```

---

## 1. What HighLevel Cares About

HighLevel is a SaaS platform around:

```text
CRM
contacts
pipelines
conversations
workflow automation
email/SMS/WhatsApp messaging
calendars
webhooks
payments
analytics
AI automation
multi-tenant business operations
```

Backend themes:

```text
multi-tenant APIs
workflow/event processing
reliable async delivery
notifications/messaging
rate limiting
webhook retries
deduplication
observability
tenant isolation
provider integrations
```

---

## 2. Prep Strategy

Do not prepare 10 HLDs deeply.

Prepare:

```text
1 deep HLD + 2 medium HLDs
```

Target depth:

```text
Webhook Delivery Engine:       10/10
Notification / Messaging:       7/10
URL Shortener + Analytics:      7/10
```

Why this works:

```text
The deep design becomes your main system-design weapon.
The two medium designs cover product-specific and resume-specific follow-ups.
```

## Shared Architecture Backbone

All three HLDs share the same backend backbone:

```text
Ingestion API
  ->
Idempotency / deduplication
  ->
Async queue or broker
  ->
Workers
  ->
Redis / rate limits / cache
  ->
Primary data store / analytics store
  ->
Observability
```

For your three systems:

| System | Ingestion | Async Backbone | Worker Responsibility | Store |
|---|---|---|---|---|
| Webhook engine | webhook event API | Kafka/NATS + outbox | HTTP delivery + retries | DB + DLQ |
| Notification system | campaign/workflow trigger | Kafka/queue | provider delivery + status | DB + provider callbacks |
| URL shortener analytics | create/redirect APIs | Kafka click events | batch analytics insert | Redis + DB + ClickHouse |

Memory hook:

```text
Master webhook engine once.
Reuse the same mental model for notifications and analytics.
```

## 3-HLD Arsenal

```text
1. Webhook Delivery Engine      -> deep, boss-fight system
2. Notification / Messaging     -> medium, HighLevel core domain
3. URL Shortener + Analytics    -> medium, Brevo-backed real experience
```

---

## 3. Prioritized Topic Stack

## Tier 1: Must Prepare Deeply

```text
Webhook Delivery System end-to-end
Notification / Messaging Engine
Distributed Rate Limiter
URL Shortener + Analytics Pipeline
Go Worker Pool + Graceful Shutdown
```

## Tier 2: Core Principles

```text
Multi-tenant data isolation
Compound indexes
Redis caching and TTLs
Deduplication keys
Idempotency keys
Retry backoff strategies
Dead Letter Queues
Failure recovery
Observability: p99, logs, metrics, traces
```

## Tier 3: Know Lightly

```text
Redis Lua script syntax
Deep Kafka tuning configs
Specific circuit breaker library names
Complex sharding math
```

Rule:

```text
Know the principles deeply. Do not get stuck memorizing library names.
```

---

## 4. HLD 1 Deep: Multi-Tenant Webhook Delivery Engine

Use this as your boss-fight system design.

Good for questions like:

```text
Design a webhook system.
Design an async delivery system.
Design an event processing system.
Design a reliable queue-based backend.
Design a multi-tenant worker system.
Design retry and DLQ architecture.
```

## Master Blueprint

```text
Incoming Event
     |
     v
API Gateway
Auth + validation + idempotency check
     |
     v
Database local transaction
Business record + outbox record
     |
     v
Outbox publisher
Polls/tails outbox and publishes to broker
     |
     v
Kafka / NATS stream
Partitioned by tenant_id where ordering per tenant matters
     |
     v
Go worker pool
Tenant token-bucket rate limiter
     |
     v
HTTP dispatch
     |
     |-- 2xx success          -> mark DELIVERED
     |-- 4xx client error     -> mark FAILED, usually non-retryable
     |-- 429 / 5xx / timeout  -> retry with jittered exponential backoff
                                  |
                                  |-- max retries not reached -> requeue
                                  |-- max retries exceeded    -> DLQ
```

## 10-Step Senior Explanation Flow

Use this as your 10-15 minute HLD flow.

## 1. Functional and Non-Functional Requirements

Functional:

```text
Ingest webhook triggers.
Dispatch HTTP POST payloads to customer URLs.
Retry transient failures.
Log execution history.
Allow manual replay of failed webhooks.
```

Non-functional:

```text
High availability
At-least-once delivery
Tenant isolation
Reliable recovery path for unpublished events
Low dispatch latency under normal load
Observable failures and replayability
```

## 2. Ingestion and Idempotency

Incoming trigger hits the API layer.

The API checks:

```text
auth
tenant_id
payload validity
target webhook config
idempotency_key
```

Idempotency key:

```text
tenant_id:source_event_id
```

Storage options:

```text
Redis with TTL for fast duplicate suppression
DB unique constraint for stronger correctness
```

Example:

```text
tenant_123:event_abc
TTL: 24 hours, depending on retry window/product policy
```

## 3. Atomic Persistence: Transactional Outbox

Avoid this dual-write risk:

```text
DB write succeeds
Kafka publish fails
event is lost
```

Instead, write both records in one DB transaction:

```text
webhook_events:
id, tenant_id, payload_ref, target_url, status=PENDING

outbox:
id, event_id, event_type, payload_ref, status=UNPUBLISHED
```

Interview line:

```text
The outbox pattern significantly reduces message loss risk by committing the business record and the outbox record atomically in the same database transaction. It gives us a reliable recovery path for unpublished events.
```

## 4. Reliable Outbox Publishing

An outbox publisher:

```text
polls UNPUBLISHED rows or uses CDC
publishes to Kafka/NATS
waits for broker ACK
marks outbox row as PUBLISHED
```

If the app crashes:

```text
UNPUBLISHED rows remain in DB and can be retried by the publisher.
```

Interview line:

```text
Even if Kafka is temporarily unavailable or the app crashes after the DB commit, the outbox row remains and publishing can resume later.
```

Senior language:

```text
Avoid saying "guaranteed zero message loss" too casually. Say "transactional outbox reduces dual-write loss risk and gives a reliable recovery path for unpublished events."
```

## 5. Consuming and Tenant Isolation

Workers consume from the broker.

Partition strategy:

```text
Partition by tenant_id if per-tenant ordering matters.
Partition by event/webhook id if higher distribution matters.
For massive tenants, partition by tenant_id + endpoint_id or tenant_id + event_type.
```

Senior partition nuance:

```text
tenant_id alone gives strict ordering per tenant, but a huge enterprise tenant can become a hot partition.

tenant_id + endpoint_id preserves ordering per destination endpoint while spreading load better.

tenant_id + event_type can work if ordering is needed only within event categories.
```

Interview line:

```text
I would start with tenant_id for tenant-level ordering, but for very large tenants I would use a compound partition key like tenant_id plus endpoint_id to avoid hot partitions while preserving endpoint-level ordering.
```

Before HTTP dispatch:

```text
load tenant config
check Redis token bucket for tenant_id
check endpoint/provider limits
apply timeout
```

If one tenant is noisy:

```text
tenant-level rate limit prevents starvation of shared worker capacity.
```

Options:

```text
per-tenant token bucket
per-tenant queues
fair scheduling
retry budget per tenant
```

## 6. Fine-Grained HTTP Response Handling

2xx:

```text
200, 201, 202, 204
Mark DELIVERED.
Record status code and latency.
```

4xx:

```text
400, 401, 403, 404
Usually non-retryable.
Mark FAILED because payload, auth, endpoint, or config is likely invalid.
```

429:

```text
Retryable.
Respect Retry-After header if present.
Otherwise use exponential backoff with jitter.
```

5xx and network timeout:

```text
500, 502, 503, 504, DNS error, connection timeout
Treat as transient.
Retry with backoff.
```

Interview line:

```text
I would not retry all failures equally. Most 4xx errors are permanent configuration or contract issues, while 429, 5xx, and network timeouts are retryable.
```

## Core Flow

```text
Client/API trigger
  ->
API layer validates request
  ->
Idempotency check using tenant_id + event_id
  ->
Persist event as PENDING
  ->
Publish through transactional outbox
  ->
Kafka/NATS topic
  ->
Go worker pool
  ->
Tenant rate limiter
  ->
HTTP delivery to destination
  ->
Success/failure status update
  ->
Retry queue or DLQ
  ->
Metrics/logs/traces
```

Simple diagram:

```text
Incoming Event
     |
     v
API Layer
     |
     | idempotency key: tenant_id:event_id
     v
Database / Outbox
     |
     v
Kafka / NATS
     |
     v
Go Worker Pool
     |
     | tenant rate limit + retry policy
     v
Webhook HTTP POST
     |
     | success -> DELIVERED
     | failure -> retry / DLQ
     v
Metrics + Logs + Traces
```

## Key Architecture Mechanics

## Ingestion and Idempotency

```text
Validate payload.
Identify tenant.
Create idempotency key: tenant_id + event_id.
Check Redis/DB to avoid duplicate processing.
```

Interview line:

```text
I would make the API idempotent because webhook/event triggers can be retried by clients or duplicated by upstream systems.
```

## Persistence First

Persist before publish:

```text
Write event as PENDING in DB.
Write outbox row in same transaction.
Separate publisher reads outbox and publishes to Kafka/NATS.
```

Why:

```text
This avoids losing events if the DB write succeeds but broker publish fails.
```

Interview line:

```text
I would use transactional outbox so database state and event publishing remain consistent.
```

## Worker Processing

Workers:

```text
consume event
load tenant/webhook config
check tenant rate limit
make HTTP POST with timeout
record delivery attempt
update status
```

Go concepts:

```text
worker pool
context timeout
WaitGroup
graceful shutdown
signal.NotifyContext
rate limiter
```

## Tenant Isolation

Problem:

```text
One noisy tenant should not starve other tenants.
```

Solutions:

```text
tenant-level rate limits
per-tenant queue partitioning
fair scheduling
Redis token bucket per tenant
limits on retries per tenant
```

## Retry Strategy

Retry on:

```text
network timeout
5xx response
429 too many requests
temporary provider failure
```

Do not retry blindly on:

```text
400 bad request
401 unauthorized
403 forbidden
404 bad endpoint, depending on product policy
```

Retry pattern:

```text
exponential backoff + jitter
max retry count
DLQ after max retries
manual replay option
```

Backoff formula:

```text
delay = min(initialDelay * 2^attempt, maxDelay) + jitter
```

Why jitter:

```text
Jitter prevents thundering herd retries when many failed webhooks retry at the same time.
```

Interview line:

```text
I would retry only transient failures with exponential backoff and jitter. Permanent failures should be marked failed to avoid wasting worker capacity.
```

## 7. Retry Pipeline

Retry event stores:

```text
event_id
tenant_id
attempt_number
next_attempt_at
last_status_code
last_error
```

Retry implementation options:

```text
delayed queue
scheduled DB polling by next_attempt_at
Kafka retry topics by delay bucket
NATS delayed delivery if supported
```

Senior tradeoff:

```text
Kafka does not naturally delay individual messages like a scheduler, so retry topics or DB-backed scheduled retries are common choices.
```

## DLQ

DLQ stores:

```text
event id
tenant id
destination URL
payload reference
failure reason
attempt count
last error
timestamps
```

DLQ use:

```text
manual inspection
replay after fixing destination/provider
alerting on high failure rate
```

## 8. DLQ and Manual Replay

After max retries:

```text
status = PERMANENTLY_FAILED
store failure metadata
move to DLQ
alert if DLQ growth spikes
```

Manual replay should support:

```text
single event replay
bulk replay by tenant/webhook/time range
replay after endpoint/config is fixed
```

Important:

```text
Replay must still respect idempotency and tenant rate limits.
```

## Observability

Metrics:

```text
webhook_events_received_total
webhook_delivery_success_total
webhook_delivery_failed_total
webhook_retry_total
webhook_dlq_total
webhook_delivery_latency_ms
webhook_queue_lag
tenant_rate_limited_total
consumer_group_lag
```

Logs:

```text
event_id
tenant_id
webhook_id
attempt
status_code
latency_ms
error
trace_id
```

Traces:

```text
API request -> DB/outbox -> broker publish -> worker consume -> HTTP delivery
```

## 9. Observability and Alerting

Alert on:

```text
DLQ growth rate spike
consumer lag above SLA
p99 delivery latency breach
tenant throttling spike
provider error rate spike
outbox unpublished rows growing
```

Dashboard views:

```text
per-tenant success/failure rate
provider failure rate
retry volume
DLQ count
queue lag
p95/p99 delivery latency
```

## Tradeoffs

```text
Kafka vs NATS:
Kafka gives durable ordered logs and replay.
NATS can be simpler/lower latency for messaging.

Redis idempotency vs DB idempotency:
Redis is fast but needs TTL strategy.
DB unique constraint is stronger but may add write pressure.

At-least-once delivery:
Accept duplicates are possible.
Use idempotency keys and delivery attempt tracking.
```

## 10. Senior Realities and Tradeoffs

At-least-once delivery:

```text
Network partitions can cause duplicate delivery. For example, customer endpoint processes the webhook but our ACK/response is lost, so we retry.
```

Receiver responsibility:

```text
Document that webhook receivers should deduplicate using event_id.
```

Exactly-once:

```text
End-to-end exactly-once over HTTP is not realistic.
Aim for at-least-once delivery plus idempotency.
```

Data retention:

```text
Keep delivery logs for a defined period.
Archive old payloads if needed.
Avoid storing sensitive payload data longer than necessary.
```

## Schema and Manual Replay APIs

Keep this in your back pocket if the interviewer asks for schema.

```sql
CREATE TABLE webhook_events (
    id UUID PRIMARY KEY,
    tenant_id VARCHAR(64) NOT NULL,
    endpoint_id VARCHAR(64) NOT NULL,
    payload JSONB NOT NULL,
    status VARCHAR(32) NOT NULL,
    attempt_count INT DEFAULT 0,
    next_retry_at TIMESTAMPTZ,
    last_response_code INT,
    last_error TEXT,
    created_at TIMESTAMPTZ NOT NULL,
    updated_at TIMESTAMPTZ NOT NULL
);
```

```sql
CREATE TABLE outbox_events (
    id UUID PRIMARY KEY,
    event_id UUID NOT NULL REFERENCES webhook_events(id),
    topic VARCHAR(128) NOT NULL,
    payload JSONB NOT NULL,
    status VARCHAR(32) NOT NULL,
    created_at TIMESTAMPTZ NOT NULL
);
```

Status values:

```text
PENDING
DELIVERED
FAILED
PERMANENTLY_FAILED
UNPUBLISHED
PUBLISHED
```

Management APIs:

```text
POST /v1/webhooks/events
GET  /v1/webhooks/events?status=PERMANENTLY_FAILED
POST /v1/webhooks/events/{id}/replay
POST /v1/webhooks/events/replay-bulk
```

API explanation:

```text
The ingest API creates events. The failed-events API lets operators or users inspect DLQ-style failures. Replay APIs allow single or bulk replay after endpoint/configuration fixes.
```

## Outbox vs Direct Producer Tradeoff

This is an important senior nuance.

## Option 1: Transactional Outbox

Flow:

```text
API -> DB transaction -> outbox row -> async publisher -> Kafka
```

Use when:

```text
financial events
billing events
critical state changes
cases where event loss is unacceptable
```

How it works:

```text
Save event as PENDING.
Write outbox row in the same local ACID transaction.
Publisher later sends to Kafka and marks outbox PUBLISHED.
```

Tradeoff:

```text
Reliable recovery path and safer consistency.
More DB IOPS.
More moving parts.
Higher write-path latency.
```

## Option 2: Direct Producer + Fail Fast

Flow:

```text
API -> Kafka producer with broker ACK -> consumer -> DB
```

Use when:

```text
high-throughput public APIs
webhook ingestion at very high volume
systems where upstream callers can retry
low-latency ingestion matters more than local DB transaction first
```

How it works:

```text
API publishes directly to Kafka with acks=1 or acks=all.
If Kafka publish succeeds, return accepted.
If Kafka is unavailable or publish times out, fail fast with HTTP 503.
Upstream caller retries.
```

Tradeoff:

```text
Lower latency and stateless API scale.
Depends on Kafka availability.
Requires clear retry contract with callers.
```

Interview line:

```text
For critical business state, I prefer transactional outbox. For very high-throughput APIs where upstream retry is acceptable, direct Kafka producer with broker ACK and fail-fast 503 can be a practical tradeoff.
```

## Two-Tier Deduplication

Tier 1: edge filtering.

```text
Local LRU / Bloom filter
Redis SETNX with short TTL
Fast duplicate suppression before Kafka/DB
```

Tier 2: ground-truth safety.

```text
Database unique constraint:
tenant_id + source_event_id
```

Example SQL idea:

```sql
CREATE UNIQUE INDEX uniq_webhook_source_event
ON webhook_events (tenant_id, endpoint_id, source_event_id);
```

Consumer insert behavior:

```sql
INSERT INTO webhook_events (...)
VALUES (...)
ON CONFLICT (tenant_id, endpoint_id, source_event_id) DO NOTHING;
```

Why this wins:

```text
Redis catches most duplicates quickly.
The DB unique constraint protects correctness if Redis fails, loses data, or has a failover.
```

Interview line:

```text
Redis is a fast duplicate filter, not my only source of truth. The database unique constraint is the final correctness guard.
```

## Best 3-Minute HLD Answer

```text
First, I would clarify requirements: reliable webhook dispatch, retries with backoff, tenant isolation, DLQ replay capabilities, and p95/p99 observability.

For ingestion, I would validate the payload, authenticate the tenant, and enforce idempotency using a tenant_id plus event_id key in Redis or a DB unique constraint.

To avoid dual-write failure risk, I would use the transactional outbox pattern. The API writes both the webhook event and the outbox record in a single database transaction. A separate publisher process reads unpublished outbox records and publishes them to Kafka or NATS.

To avoid hot-partition bottlenecks for large tenants, I would partition by tenant_id plus endpoint_id if endpoint-level ordering is enough. Worker pools consume events, enforce tenant rate limits using Redis token buckets, and dispatch HTTP POST requests with timeouts.

For response handling, 2xx is marked delivered, most 4xx client errors are marked failed immediately, while 429, 5xx, and network timeouts go through retry with jittered exponential backoff.

After max retries, the event moves to a DLQ with failure metadata for inspection and manual replay. The system is at-least-once, because network ACKs can fail after the customer processes the webhook, so we expose event_id and expect receivers to handle idempotency.
```

## HighLevel Pitch Script

Use this when asked why you fit HighLevel:

```text
My core strength is backend ownership in production, not just writing Go code. At Brevo, I worked on backend services, event-driven pipelines, Kafka, Redis, ClickHouse, MongoDB, API performance, and production troubleshooting.

What excites me about HighLevel is the scale and product complexity. A platform handling high API and messaging volume needs reliable async processing, tenant isolation, retries, observability, and careful operational thinking.

That is the kind of backend work I enjoy: owning systems from design to implementation to production support.
```

## Day 1 Speaking Checklist

```text
[ ] Rehearse the 3-minute crisp version out loud twice.
[ ] Rehearse the 15-minute deep-dive version out loud once.
[ ] Rehearse the 30-second positioning pitch out loud once.
```

---

## Distributed System Patterns Toolkit

Use these patterns across HighLevel HLD and follow-up questions.

| Pattern | Failure Scenario It Solves | Key Concept |
|---|---|---|
| Transactional Outbox | DB write succeeds but broker publish fails | Write business row + outbox row in one DB transaction |
| Circuit Breaker | Dead downstream causes cascading timeouts | Trip open after error threshold and fail fast |
| Dead Letter Queue | Poison messages keep failing workers | Retry with backoff, then isolate for replay/inspection |
| Bulkhead | Noisy tenant exhausts shared workers | Isolated worker pools, queues, or connection limits |
| Token Bucket | API abuse or downstream overload | Refill tokens at fixed rate and allow controlled bursts |
| Saga | Multi-service transaction cannot use one DB transaction | Local transactions plus compensating actions |

Memory hook:

```text
Outbox = recovery path
Circuit breaker = stop calling dead dependency
DLQ = isolate poison messages
Bulkhead = isolate noisy tenant
Token bucket = controlled rate
Saga = distributed transaction with compensation
```

---

## 5. HLD 2 Medium: Notification / Messaging System

Why this matters:

```text
HighLevel has conversations, campaigns, automation, email/SMS/WhatsApp style product areas.
```

HighLevel context:

```text
HighLevel automates customer communication across SMS, email, WhatsApp, and campaign/workflow triggers.
```

Core flow:

```text
Workflow trigger
  ->
Template validation
  ->
User/contact preference check
  ->
Message queued
  ->
Provider selection
  ->
Send via email/SMS/WhatsApp provider
  ->
Track status: queued/sent/delivered/failed
  ->
Callback/webhook from provider
  ->
Retry/DLQ if needed
```

Main components:

```text
Notification API
Template service
Preference service
Queue/broker
Provider adapter
Status tracker
Retry/DLQ worker
Callback handler
Observability dashboard
```

Important points:

```text
deduplication key
tenant rate limit
provider rate limit
retry transient failures
do not retry invalid numbers/emails forever
track provider response IDs
store delivery status
respect user unsubscribe/preferences
provider fallback
status callbacks
```

Deduplication key:

```text
tenant_id:user_id:campaign_id
tenant_id:contact_id:workflow_step_id
```

Provider routing:

```text
SMS primary provider: Twilio
SMS fallback provider: Plivo or another configured provider
Email provider: SendGrid / Mailgun style adapter
WhatsApp provider: configured WhatsApp provider
```

Fallback rule:

```text
If primary provider returns 5xx, timeout, or rate-limit response, retry or fail over to secondary provider depending on tenant/provider config.
```

Status callbacks:

```text
SENT -> DELIVERED -> FAILED / READ
```

Provider callbacks feed back into the system:

```text
Provider callback
  ->
callback API
  ->
validate provider signature
  ->
update message status
  ->
emit status event
  ->
analytics/automation follow-up
```

Interview line:

```text
Messaging is very similar to webhook delivery, but the destination is an external communication provider and we also need provider callback handling to update delivery status.
```

Interview line:

```text
I would separate message creation from provider delivery. The API creates a durable notification request, workers deliver through provider adapters, and callbacks update final delivery status.
```

---

## 6. HLD 3 Medium: URL Shortener + Analytics

Why this matters:

```text
This connects directly to Brevo experience and is a safe resume-backed system design.
```

Brevo context:

```text
This is your most authentic HLD because it maps to scalable backend services, URL-shortening workflows, Redis caching, Kafka events, and ClickHouse analytics.
```

Core flow:

```text
Create short URL
  ->
Generate short code
  ->
Store mapping
  ->
Redirect request
  ->
Read from Redis cache
  ->
Fallback to DB
  ->
Redirect with low latency
  ->
Emit click event asynchronously
  ->
Kafka
  ->
ClickHouse analytics
```

Components:

```text
URL creation API
short code generator
URL mapping DB
Redis cache
redirect service
click event producer
Kafka topic
analytics consumer
ClickHouse tables
dashboard/query API
```

Important points:

```text
short code collision handling
custom aliases
expiration
abuse prevention
hot link caching
asynchronous click tracking
rate limiting
ClickHouse for analytics
```

Write path:

```text
Generate 64-bit numeric ID.
Encode ID using Base62.
Store short_code -> long_url mapping in MongoDB/PostgreSQL.
```

Base62 capacity:

```text
62^7 gives around 3.5 trillion combinations.
```

Read path:

```text
GET /{short_code}
  ->
Go redirect service
  ->
check in-memory cache / Redis
  ->
fallback to DB
  ->
return HTTP 302 Found
  ->
emit click event asynchronously
```

Why `302 Found`:

```text
302 keeps redirect traffic flowing through our service so we can continue recording analytics. Permanent 301 redirects may be cached by clients/browsers and reduce future analytics visibility.
```

Analytics event:

```text
short_code
tenant_id
campaign_id
ip
user_agent
referrer
geo
timestamp
```

Analytics pipeline:

```text
redirect service emits click event to Kafka
consumer batches events
batch insert into ClickHouse
dashboard queries aggregated clicks
```

Interview line:

```text
The redirect path must stay fast, so click tracking should be asynchronous and non-blocking. Redis handles hot links, the database is the source of truth, and Kafka plus ClickHouse handle analytics.
```

Interview line:

```text
The redirect path must be extremely fast, so I keep analytics asynchronous. Redis handles hot links, DB stores source of truth, and click events go to Kafka and ClickHouse for analytics.
```

---

## 7. Go LLD and Concurrency Drills

Must be ready to code/explain:

```text
worker pool with jobs/results channels
context cancellation
graceful shutdown with signal.NotifyContext
rate limiter: token bucket / sliding window
fan-out / fan-in
mutex-protected map
retry with backoff
```

Minimum coding list:

```text
Worker Pool
Rate Limiter Middleware
Valid Anagram
Min Stack
Course Schedule
```

For HighLevel, most important Go coding:

```text
worker pool + graceful shutdown
rate limiter
retry worker
```

---

## 8. Five Brevo STAR Stories

Use this structure:

```text
Context
Problem
My Ownership
Technical Choices
Result
```

## Story 1: URL Shortener Service

Focus:

```text
short code generation
Redis caching
redirect latency
click analytics
hot links
Kafka/ClickHouse analytics
```

Positioning:

```text
I worked on a high-read backend system where redirect latency mattered, so we separated the fast redirect path from async analytics processing.
```

## Story 2: Kafka + ClickHouse Pipeline

Focus:

```text
high-throughput ingestion
batching
consumer groups
partitioning
DLQ
ClickHouse query performance
```

Positioning:

```text
I worked on event-driven data ingestion where reliability, throughput, and query performance were important.
```

## Story 3: Production Incident and RCA

Focus:

```text
logs
metrics
traces
DB connection pool
latency spike
rollback/mitigation
RCA
prevention
```

Positioning:

```text
I first reduce user impact, then use logs, metrics, traces, and recent deploys to isolate root cause and prevent recurrence.
```

## Story 4: Cloud Migration / Infrastructure Refactor

Focus:

```text
zero downtime
rollback plan
traffic shifting
observability
performance improvement
cost/reliability
```

Positioning:

```text
I think of migrations as reliability projects, not just deployment tasks.
```

## Story 5: API Performance Optimization

Focus:

```text
pprof
slow queries
missing indexes
N+1
Redis caching
p99 latency
load testing
```

Positioning:

```text
I debug performance by measuring first, then fixing the highest-impact bottleneck.
```

---

## 9. Questions They May Ask

## Go / Backend

```text
How do you design a worker pool?
How do you avoid goroutine leaks?
How does graceful shutdown work in a real service?
What happens when a channel blocks?
How does Go scheduler work?
How does Go GC work?
Why are maps not thread-safe?
```

## System Design

```text
Design a webhook delivery system.
Design a notification system.
Design a rate limiter.
Design URL shortener with analytics.
Design retry/DLQ architecture.
Design a multi-tenant CRM service.
```

## Production

```text
How do you debug a slow API?
How do you handle provider failures?
How do you prevent duplicate event processing?
How do you handle a noisy tenant?
How do you measure reliability?
```

---

## 10. Three-Day Execution Plan

## Day 1: Master System Design

Focus:

```text
Distributed Webhook Delivery Engine
```

Output:

```text
Draw architecture once
Speak 2-minute HLD answer
List APIs, DB tables, queues, retries, DLQ, metrics
```

## Day 2: Go Production LLD

Focus:

```text
worker pool with context and WaitGroup
graceful shutdown with signal.NotifyContext
rate limiter
fan-out/fan-in
retry worker with backoff
```

Output:

```text
Write worker pool once
Write rate limiter skeleton once
Explain graceful shutdown aloud
```

## Day 3: Brevo STAR Polish

Focus:

```text
5 STAR stories
```

Output:

```text
Each story in 90 seconds
No rambling
Problem -> Ownership -> Technical Choice -> Result
```

## Remaining 3-Day Prep Summary

```text
Day 1:
System design HLD.
Webhook deep + notification medium + URL shortener medium.

Day 2:
Go LLD.
Worker Pool + Context Cancellation + WaitGroup + Token Bucket Rate Limiter + Graceful SIGTERM Shutdown.

Day 3:
Brevo STAR stories.
URL Shortener, Kafka + ClickHouse pipeline, production incident, cloud refactor, API optimization.
```

---

## 11. Final Checklist Before Interview

```text
Deep HLD:
- Webhook Delivery Engine

Backup HLD:
- Notification / Messaging
- URL Shortener + Analytics

Go:
- worker pool
- context cancellation
- graceful shutdown
- rate limiter
- goroutine leaks
- scheduler/GC basics

Backend:
- idempotency
- retries
- DLQ
- Redis caching
- Kafka/event processing
- observability

Stories:
- URL shortener
- Kafka + ClickHouse
- production incident
- cloud migration
- API performance
```

Main line:

```text
Do not sound like someone who memorized HLD.
Sound like someone who has operated production backend systems.
```
