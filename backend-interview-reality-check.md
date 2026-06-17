# Backend Interview Reality Check

These questions are not only Node.js questions. Most are backend system design and production reliability questions.

Main buckets:

```text
1. Message queues: RabbitMQ/Kafka reliability
2. Redis/cache invalidation
3. Microservices failure handling
4. API security and API Gateway
5. Containers/Kubernetes production basics
6. DB indexing and WebSocket scaling
```

For Go backend roles, the Node.js event loop question is less central. The Go equivalents to know are goroutines, channels, scheduler basics, context cancellation, worker pools, race conditions, and graceful shutdown.

## RabbitMQ Consumer Crash

Question:

> What happens if your RabbitMQ consumer crashes before processing a message?

Good answer:

> I would use manual acknowledgement. The consumer only acknowledges the message after successful processing. If the consumer crashes before ack, the broker can redeliver the message. Since redelivery can happen, the consumer should be idempotent to avoid duplicate side effects.

For Kafka, the related concept is offset commits. Commit the offset only after processing succeeds.

## Redis Cache Invalidation

Question:

> How do you handle cache invalidation in Redis when data updates?

Use cache-aside:

```text
Read:
- check Redis
- if miss, read DB
- store in Redis with TTL

Write:
- update DB first
- delete or update related Redis key
```

Good answer:

> On update, I would write to the database first, then invalidate or delete the related cache key. I would also use TTL as a safety net so stale data eventually disappears even if invalidation fails.

## Reliable Microservice Communication

Question:

> How do microservices communicate reliably with each other?

Good answer:

> For real-time request/response, I would use REST or gRPC with timeouts, retries, and circuit breakers. For event-driven workflows, I would use Kafka or RabbitMQ so services are decoupled and messages can be retried or moved to a DLQ.

Reliability tools:

```text
timeouts
retries with backoff
idempotency
circuit breakers
dead-letter queues
observability
```

## API Gateway

Question:

> What exactly does an API Gateway solve in a system?

Good answer:

> An API Gateway centralizes cross-cutting concerns. Instead of every service implementing auth, rate limiting, routing, and logging separately, the gateway handles these at the edge and forwards requests to internal services.

It often handles routing, auth, rate limiting, validation, TLS termination, logging, monitoring, load balancing, and versioning.

## Downstream Service Failure

Question:

> What will you do if Service A calls Service B and Service B is down?

Good answer:

> Service A should have a timeout while calling Service B. For transient failures, retry with exponential backoff. If failures continue, open a circuit breaker to avoid cascading failures. Depending on the use case, either return a fallback response or push the work to a queue for later processing.

## Kubernetes Pod Restart

Question:

> How do you ensure containers or pods automatically restart in production?

Good answer:

> In Kubernetes, a Deployment maintains the desired number of pod replicas. If a pod crashes, Kubernetes restarts it. Liveness probes detect unhealthy containers and restart them. Readiness probes ensure traffic is only sent to healthy pods.

Mention `restartPolicy`, Deployments, ReplicaSets, liveness probes, and readiness probes.

## Secure APIs

Question:

> How do you design secure APIs?

Good answer:

> I would use authentication such as JWT or session-based auth, enforce authorization at the resource level, validate all inputs, use HTTPS, rate-limit sensitive endpoints, avoid leaking internal errors, and log security-relevant events safely.

## WebSocket Scaling

Question:

> How do you scale WebSocket connections across multiple servers?

Good answer:

> Since WebSocket connections are long-lived, multiple servers need a way to share events. We can use a load balancer and either sticky sessions or a shared pub/sub layer like Redis Pub/Sub, Kafka, or NATS. When one server receives a message, it publishes it to the broker, and the server holding the target user connection delivers it.

## JWT Logout

Question:

> How do you invalidate JWT tokens on logout?

Good answer:

> For JWT logout, I would keep access tokens short-lived and manage refresh tokens server-side. On logout, revoke or delete the refresh token. If immediate access-token invalidation is required, store the JWT ID in a Redis blacklist until the token expires.

## Socket Rooms And Namespaces

Question:

> What are rooms and namespaces in sockets, and when would you use them?

Simple answer:

```text
Namespace = separate logical endpoint, like /chat or /notifications.
Room = group inside a namespace, like conversation_123 or user_456.
```

For Go/WebSocket interviews, the equivalent ideas are connection groups, topic subscriptions, chat rooms, and user channels.

## Indexing Strategy

Question:

> How do you decide indexing strategy based on different query patterns?

Good answer:

> I look at the most frequent and most expensive queries: filters, joins, sorting, and pagination. Then I create indexes that match WHERE conditions, JOIN keys, ORDER BY fields, and uniqueness constraints. I also avoid over-indexing because indexes slow down writes and consume storage.

Example query:

```sql
WHERE user_id = ? ORDER BY created_at DESC
```

Good index:

```sql
(user_id, created_at DESC)
```

## TTL Indexes And Expiration

Question:

> If multiple TTL indexes or expirations are involved, how do you handle incoming user requests?

Good answer:

> If expiry matters for correctness, store `expires_at` and validate it during request handling. TTL indexes or cache expiry can clean up data eventually, but request logic should not assume deletion happens exactly at expiry time.

TTL should be treated as eventual cleanup, not exact business logic timing.

## Node.js Event Loop

Question:

> Can you explain the event loop phases in Node.js and how they impact execution?

Node.js phases:

```text
timers
pending callbacks
idle/prepare
poll
check
close callbacks
microtasks between phases
```

For Go roles, the closer answer is:

> Goroutines are multiplexed by the Go scheduler over OS threads. Blocking operations can park goroutines. For concurrency control, we use channels, mutexes, context cancellation, WaitGroups, and worker pools.

## Node.js Streaming / Multiple Requests

Question:

> Write a function that can perform streaming for multiple requests or data sources using Node.js/libuv.

Good answer:

> For streaming multiple requests in Node.js, I would use streams so data is processed chunk by chunk. For each request, I can create a readable stream from the source and pipe it to a writable destination using `pipeline`. This avoids loading the full payload into memory. Since Node.js uses non-blocking I/O through libuv, multiple streams can progress concurrently while the event loop schedules callbacks. I would also handle backpressure and errors properly using `stream.pipeline`.

Expected concepts:

```text
fs.createReadStream()
fs.createWriteStream()
stream.pipeline()
Transform streams
backpressure
Promise.all for multiple independent streams
```

Important:

> Streams are for I/O concurrency and memory efficiency, not CPU parallelism.

If CPU-heavy processing is needed inside the stream, use `worker_threads` or a separate worker service because CPU-heavy work can block the event loop.

## Message Queue Vs Redis Pub/Sub

Question:

> How is a message queue like RabbitMQ/Kafka different from Redis Pub/Sub?

Good answer:

> A message queue is used when reliability matters. Messages can be persisted, acknowledged, retried, and processed later. Redis Pub/Sub is mainly for real-time broadcasting. If a subscriber is offline, it can miss messages, so I would not use plain Pub/Sub for critical workflows.

RabbitMQ/Kafka:

```text
persistent
reliable delivery
acknowledgements or offsets
retries
replay possible depending on system
consumer groups
better for critical async processing
```

Redis Pub/Sub:

```text
fire-and-forget
no persistence by default
offline subscribers miss messages
good for real-time notifications
not ideal for durable jobs
```

## Revision Checklist

Redis:

```text
cache-aside
TTL
invalidation
stale data
cache stampede
rate limiting
sorted sets
```

Queues:

```text
ack/offset
retry
DLQ
idempotency
ordering
consumer crash
Kafka vs RabbitMQ vs Redis Pub/Sub
```

Microservices:

```text
timeout
retry
circuit breaker
fallback
service discovery
API gateway
```

DB:

```text
indexing by query pattern
composite indexes
transactions
isolation levels
pagination
```

Security:

```text
auth vs authorization
JWT logout
rate limiting
validation
```

Infra:

```text
Docker
Kubernetes restart
readiness/liveness
logs/metrics/tracing
```

Bottom line:

> Backend interviews are not asking "Do you know Redis?" They are asking "Do you know when Redis fails, goes stale, or becomes the wrong choice?"
