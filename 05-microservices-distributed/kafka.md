# Kafka Interview Answers

For Kafka right now, mostly theory plus correct explanation is enough.

You do not need to implement Kafka producer/consumer code unless the role or JD specifically says Kafka-heavy coding.

Interviews usually ask Kafka like this:

```text
What is Kafka?
What is topic/partition?
What is consumer group?
What is offset?
How do retries work?
What is DLQ?
How do you avoid duplicate processing?
How does ordering work?
When do you commit offset?
```

Goal:

> Answer these clearly with production behavior, not only definitions.

## Quick Basics Revision

Revise only this when time is short:

```text
Topic:
Logical stream/category of messages.

Partition:
Topic is split into partitions for parallelism and scalability.

Producer:
Sends messages to topic/partition.

Consumer:
Reads messages from topic partitions.

Consumer group:
Multiple consumers share the work. One partition is consumed by only one consumer in the same group at a time.

Offset:
Position of a message inside a partition.

Ordering:
Kafka guarantees ordering only within a partition, not across all partitions.
```

Best interview line:

> Kafka is used for asynchronous, event-driven communication. Producers write events to topics, topics are split into partitions, and consumers in a consumer group divide partitions among themselves for parallel processing.

## 1. Basic Flow

```text
Producer -> Topic -> Partition -> Consumer Group -> Consumer
```

Good answer:

> Kafka is used for asynchronous event-driven communication. Producers publish messages to topics. Topics are split into partitions. Consumers read messages from partitions, and consumer groups allow parallel processing.

Important:

```text
Producer publishes events.
Topic stores a named stream of events.
Partition is an ordered log inside a topic.
Consumer reads events.
Consumer group shares work across consumers.
```

## 2. Topic And Partition

Topic:

> A named stream/category of messages.

Partition:

> A topic is split into partitions. Each partition is an ordered append-only log.

Why partitions matter:

```text
parallelism
scaling
ordering within a partition
```

Interview line:

> Partitions allow Kafka to scale horizontally. More partitions allow more consumers in a group to process messages in parallel, but ordering is only guaranteed inside one partition.

## 3. Offset

Good answer:

> Offset is the position of a message inside a partition. A consumer commits offsets to tell Kafka how far it has processed.

Example:

```text
partition-0:
offset 0 -> event A
offset 1 -> event B
offset 2 -> event C
```

If consumer commits offset `2`, it is saying:

```text
I have processed messages up to this point.
```

Interview line:

> Offset is consumer progress. Committing offset too early can lose messages. Committing after processing gives at-least-once delivery, so the consumer must handle duplicates.

## 4. Consumer Group

Good answer:

> In one consumer group, each partition is assigned to only one consumer at a time. This allows horizontal scaling.

Example:

```text
Topic has 6 partitions.
Consumer group has 3 consumers.
Each consumer may get 2 partitions.
```

Important:

```text
One partition cannot be actively consumed by two consumers in the same group.
If consumers > partitions, extra consumers stay idle.
Different consumer groups can read the same topic independently.
```

Interview line:

> Consumer groups let multiple service instances share work. Kafka assigns partitions among consumers in the same group.

## 5. Ordering

Good answer:

> Kafka guarantees ordering only within a partition, not across the whole topic.

If order matters for one entity:

```text
use the same key
example: user_id, order_id, task_id
```

Messages with the same key usually go to the same partition.

Interview line:

> If I need ordering for a user/order/task, I use that ID as the Kafka key so related events go to the same partition.

## 6. Retry And DLQ

Good answer:

> I would process the message first, and commit the offset only after processing succeeds. If processing fails, I would retry with backoff. After max retries, I would send the message to a DLQ and then commit the original offset so the consumer is not blocked forever.

Common flow:

```text
main topic -> retry topic -> delayed retry topic -> DLQ
```

DLQ means:

> Dead-letter queue. It stores messages that could not be processed after retries.

Why DLQ is useful:

```text
prevents poison messages from blocking a partition
keeps failed messages available for debugging/replay
allows alerting and manual investigation
```

Interview line:

> DLQ is used when a message keeps failing after retries. It prevents one bad message from blocking the whole consumer.

## 7. Duplicate Handling

Good answer:

> Kafka usually gives at-least-once processing in common setups, so duplicate messages can happen. Consumers should be idempotent, for example by storing processed event IDs or using unique constraints.

Idempotency examples:

```text
store processed event_id in DB
use unique constraint on business operation
make updates safe to repeat
check current state before applying transition
```

Example table:

```text
processed_events(event_id unique, processed_at)
```

Interview line:

> I assume duplicate processing can happen, so I design the consumer to be idempotent.

## 8. When To Commit Offset

Best interview answer:

> I prefer committing offsets only after processing succeeds. If processing fails, I retry or send to DLQ after max attempts. This gives at-least-once processing, so the consumer must be idempotent.

Commit before processing:

```text
fast but risky
message can be lost if service crashes after commit but before processing
```

Commit after processing:

```text
safer
message may be reprocessed after crash
requires idempotent consumer
```

## 9. Auto Commit Vs Manual Commit

Auto commit:

```text
Kafka client commits offsets automatically.
Simple but can commit before business processing is truly complete.
```

Manual commit:

```text
Application commits offset after successful processing.
More control.
Preferred when correctness matters.
```

Interview line:

> For important workflows, I prefer manual commit after processing succeeds because it gives better control over reliability.

## 10. Rebalancing

Rebalancing happens when:

```text
consumer joins group
consumer leaves group
consumer crashes
partition count changes
```

Impact:

```text
partitions are reassigned
processing may pause briefly
duplicates can happen around rebalance
```

Interview line:

> During rebalance, partitions are reassigned among consumers. Consumers should handle duplicate processing safely.

## Must-Memorize Answer

> Kafka is used for asynchronous event-driven communication. Producers publish messages to topics. Topics are split into partitions. Consumers read messages from partitions. Consumer groups allow parallel processing, where each partition is assigned to only one consumer within the same group. Kafka tracks progress using offsets. I prefer committing offsets only after processing succeeds. If processing fails, I retry with backoff, and after max retries I send the message to a DLQ and commit the original offset so the consumer does not get stuck. Since this gives at-least-once delivery, consumers should be idempotent.

## Quick Revision Lines

```text
Ordering is guaranteed only within a partition.
Consumer group gives horizontal scaling.
Offset is the consumer's progress inside a partition.
Commit after successful processing.
At-least-once means duplicates can happen.
Idempotent consumer handles duplicates safely.
DLQ prevents poison messages from blocking the partition forever.
Manual commit gives better correctness control than auto commit.
```

## For Today

Learn this much:

```text
Kafka basics: 45 min
Kafka retry/offset/DLQ/idempotency: 45 min
No Kafka coding today
```

Coding priority:

```text
Go worker pool
Rate limiter
Min Stack
Course Schedule
```

Kafka is mainly:

```text
explanation + production behavior
```
