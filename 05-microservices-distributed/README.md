# 05 - Microservices and Distributed Systems

## Topics From Checklist

### microservices basics

- Why split services
- Service ownership
- Inter-service communication
- Deployment independence

### sync vs async communication

- REST/gRPC for sync
- Kafka/queue for async
- Tradeoffs

### retries

- Retry with exponential backoff
- Retry storms
- Idempotency with retries

### timeouts

- Why every network call needs timeout
- Context deadline
- Avoid hanging requests

### circuit breaker

- Stop calling failing dependency
- Fail fast
- Recover after cooldown

### eventual consistency

- Why distributed systems are not always immediately consistent
- Examples: like counts, feed updates, notifications

### Kafka basics

- Topics
- Partitions
- Offsets
- Consumer groups
- Ordering
- Rebalancing

### Kafka delivery guarantees

- At-most-once
- At-least-once
- Exactly-once limitations
- Idempotent consumers

### DLQ

- Dead-letter queue
- Why failed messages should not block forever
- When to retry vs DLQ

### outbox pattern

- Prevent DB write success but Kafka publish failure
- Store event in DB transaction
- Background publisher sends event

### distributed locks

- When needed
- Redis lock basics
- Why locks are tricky

---

## Existing Coverage

Partial conceptual coverage exists in:

- `/Users/sahiltyagi/Desktop/personal projects/system design/concepts/scaling-reliability.md`
- `/Users/sahiltyagi/Desktop/personal projects/system design/examples/notification-system.md`

---

## Covered In This Folder

- `kafka.md`
- `kafka-dlq-outbox.md`
- `retries-timeouts-circuit-breaker.md`
- `../backend-interview-reality-check.md`

These cover:

- quick Kafka interview answers
- Kafka topic/partition explanation
- consumer groups
- retry vs DLQ flow
- outbox pattern flow
- idempotent consumer example
- distributed lock pitfalls
- RabbitMQ/Kafka consumer crash behavior
- message queue vs Redis Pub/Sub
- Service A to Service B failure handling
