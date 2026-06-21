# MongoDB Role Interview Prep

Use this note for MongoDB-style backend/platform interviews.

## Role Fit

MongoDB is the strongest brand and highest-upside role in the current pipeline.

Treat it as:

```text
stretch-good role
core platform / distributed systems / AI application platform engineering
not basic CRUD backend
```

## Role Focus

Likely focus areas:

```text
distributed systems
high-scale compute/data systems
Python / Go / Java
cloud services
performance tuning
concurrent systems
MongoDB fundamentals
AI application platform
architecture ownership
```

## Main Topics

Must know:

```text
MongoDB indexes
compound indexes
explain()
COLLSCAN vs IXSCAN
aggregation pipeline
schema design
replica set
sharding
transactions
read concern / write concern
distributed systems basics
Go concurrency
system design
```

Good to know:

```text
oplog
change streams
TTL indexes
connection pooling
query optimization
Kubernetes basics
AI infra basics
```

## MongoDB Indexes

Good answer:

> Indexes improve query performance by allowing MongoDB to avoid scanning the whole collection. Without an index, MongoDB may do a collection scan. With the right index, MongoDB can perform an index scan.

Interview details:

```text
COLLSCAN = collection scan, scans documents
IXSCAN = index scan, uses index
indexes improve reads
indexes add write/storage overhead
```

## Compound Index

Good answer:

> A compound index supports queries using the leftmost prefix. For example, an index on `userId` and `createdAt` helps queries filtering by `userId` and sorting or filtering by `createdAt`.

Example:

```text
index: { userId: 1, createdAt: -1 }
```

Good for:

```text
find by userId
find by userId sorted by createdAt
find by userId and createdAt range
```

Not as useful for:

```text
query only by createdAt
```

because `createdAt` is not the leftmost prefix.

## explain()

Good answer:

> I would use `explain()` to check whether the query uses `IXSCAN` or does `COLLSCAN`. If it is doing a collection scan, I would look at query shape, indexes, sort fields, and cardinality to optimize it.

Check:

```text
winningPlan
COLLSCAN vs IXSCAN
keys examined
documents examined
execution time
sort stage
```

Interview line:

> I use explain to verify the query plan instead of guessing whether an index is being used.

## Embed Vs Reference

Good answer:

> If data is mostly read together and bounded in size, I would embed it. If data grows unbounded, is shared independently, or changes frequently on its own, I would reference it.

Embed when:

```text
one-to-few
data read together
bounded array size
child does not need independent lifecycle
```

Reference when:

```text
one-to-many or unbounded
large arrays
shared entity
independent updates
separate access patterns
```

## Replica Set

Good answer:

> A replica set provides high availability. It has a primary and secondary nodes. Writes go to the primary, secondaries replicate using the oplog, and if the primary fails, election happens and a secondary can become primary.

Key points:

```text
primary handles writes
secondaries replicate
oplog stores ordered operations
election happens on primary failure
improves availability
```

## Sharding

Good answer:

> Sharding is used to horizontally scale data across multiple shards. The shard key decides how data is distributed. A good shard key should have high cardinality and avoid hotspots.

Good shard key:

```text
high cardinality
good distribution
matches query patterns
avoids monotonically increasing hot shard
```

Bad shard key:

```text
low cardinality
hot values
always increasing value like timestamp alone
not used in queries
```

## Transactions

Good answer:

> MongoDB supports multi-document transactions, but I would not use them casually. I would first try to model data so common operations are atomic within a document. I use transactions when correctness requires updating multiple documents atomically.

Useful line:

> In MongoDB, schema design should reduce the need for distributed-style transactions when possible.

## Read Concern / Write Concern

Read concern:

> Controls the consistency level of data returned by reads.

Write concern:

> Controls acknowledgement level for writes, for example whether a write must be acknowledged by primary only or by majority.

Interview line:

> Stronger concerns improve consistency/durability but can increase latency.

## Aggregation Pipeline

Good answer:

> Aggregation pipeline processes documents through stages like match, group, sort, project and lookup. I try to filter early with `$match`, use indexes where possible, and avoid expensive stages on huge data without need.

Common stages:

```text
$match
$project
$group
$sort
$lookup
$unwind
```

Performance thought:

```text
filter early
project only needed fields
avoid large in-memory sort
use indexes for match/sort when possible
```

## MongoDB Role Pitch

Use this:

> My strongest experience is in backend systems using Go, MongoDB, Kafka, Redis, ClickHouse, and GCP. I have worked on production systems, cloud migration, troubleshooting, and performance improvements. This MongoDB role interests me because it combines distributed systems, data infrastructure, cloud platform work, and the new AI application platform direction.

## System Design Angles

Prepare:

```text
AI application deployment platform
scalable backend API service
Kafka-based event pipeline
query optimization / analytics service
worker/task processing system
```

Design checklist:

```text
API layer
auth/user identity
metadata database
object storage
queue/Kafka
workers
observability
rate limiting
autoscaling
failure handling
retry/DLQ
```

## Salary Positioning

```text
Target: 35-45 LPA
Minimum serious discussion: 30 LPA+
```

Important:

> Do not quote MAI Labs or GreyOrange range for MongoDB. MongoDB is a much stronger brand and role.

## Quick Revision

Must be safe on:

```text
indexes
compound indexes
explain()
COLLSCAN vs IXSCAN
embed vs reference
replica set
sharding
transactions
read/write concern
Go concurrency
system design
project explanation
```
