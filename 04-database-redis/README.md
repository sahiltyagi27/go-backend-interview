# 04 - Database and Redis

## Topics From Checklist

### SQL basics

- Joins
- Indexes
- Transactions
- Constraints
- Foreign keys

### indexes

- B-tree index
- Composite index
- When index helps
- When index hurts writes

### transactions

- ACID
- Commit/rollback
- Isolation levels
- Dirty read
- Non-repeatable read
- Phantom read

### locking

- Row locks
- Deadlocks
- Optimistic locking
- Pessimistic locking

### normalization

- Avoid duplicate data
- Foreign keys
- When denormalization makes sense

### NoSQL

- MongoDB use cases
- Redis use cases
- Bigtable/Cassandra-style use cases
- SQL vs NoSQL tradeoffs

### Redis

- Cache
- TTL
- Counters
- Sorted sets
- Distributed locks
- Cache invalidation

### caching patterns

- Cache-aside
- Write-through
- Write-behind
- Cache invalidation
- Hot keys

---

## Existing Coverage

Partial coverage exists in:

- `/Users/sahiltyagi/Desktop/personal projects/system design/concepts/core-building-blocks.md`
- `/Users/sahiltyagi/Desktop/personal projects/system design/concepts/api-data-modeling.md`

---

## Covered In This Folder

- `sql-indexes-transactions.md`
- `redis-caching.md`
- `../backend-interview-reality-check.md`

These cover:

- SQL joins and indexes
- transaction isolation
- optimistic locking version column
- Redis cache-aside
- Redis token bucket concepts
- Redis sorted sets for feed ranking
- Redis invalidation on updates
- TTL indexes and expiry correctness
- indexing strategy by query pattern
