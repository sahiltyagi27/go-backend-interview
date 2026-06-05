# Kafka, DLQ, and Outbox Pattern

## Kafka Basics

Topic:

> A named stream of events.

Partition:

> Ordered log within a topic.

Offset:

> Position of a message in a partition.

Consumer group:

> A set of consumers sharing work for a topic.

## Ordering

Kafka preserves order only within a partition.

Use the same key for related events:

```text
key = user_id
```

This sends a user's events to the same partition.

## Rebalancing

Rebalancing happens when consumers join/leave a group. Partitions are reassigned.

Impact:

- temporary pause
- possible duplicate processing

Consumers should be idempotent.

## Delivery Guarantees

At-most-once:

- commit offset before processing
- may lose messages

At-least-once:

- process before committing offset
- may process duplicates

Exactly-once:

- limited and complex
- often misunderstood

Interview-safe line:

> Most systems use at-least-once delivery with idempotent consumers.

## Idempotent Consumer

Store processed event IDs.

```text
processed_events(event_id unique, processed_at)
```

Flow:

```text
begin transaction
insert event_id
if duplicate, skip
apply business update
commit
```

## DLQ

Dead-letter queue stores messages that repeatedly fail.

Use DLQ when:

- message is malformed
- dependency keeps failing after retries
- processing would block the entire partition

Retry strategy:

```text
main topic -> retry topic -> retry topic with delay -> DLQ
```

## Outbox Pattern

Problem:

```text
DB write succeeds
Kafka publish fails
System is inconsistent
```

Solution:

Write business data and event to same DB transaction.

```text
BEGIN
INSERT INTO orders ...
INSERT INTO outbox_events ...
COMMIT
```

Background publisher:

```text
read unsent outbox events
publish to Kafka
mark as sent
```

Benefit:

> Prevents losing events when DB commit and Kafka publish cannot be atomic together.

