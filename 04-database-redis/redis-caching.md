# Redis and Caching Patterns

## Redis Use Cases

- cache
- TTL data
- counters
- sessions
- rate limiting
- distributed locks
- sorted sets
- queues for simple workloads

## TTL

TTL automatically expires keys.

```text
SET user:123 "{...}" EX 300
```

Good for:

- sessions
- cache entries
- OTPs
- temporary locks

## Counters

Atomic increment:

```text
INCR views:post:123
```

Good for:

- rate limits
- page views
- retry attempts

## Sorted Sets

Sorted sets store members ordered by score.

```text
ZADD feed:user123 1893456000 post999
ZREVRANGE feed:user123 0 19 WITHSCORES
```

Good for:

- ranked feeds
- leaderboards
- delayed jobs

## Cache-Aside

Application controls cache.

```text
read cache
if miss:
  read DB
  write cache
return data
```

Most common pattern.

Risk:

- stale cache
- cache stampede

## Write-Through

Write cache and DB together.

Benefit:

- cache stays warm

Cost:

- write latency increases

## Write-Behind

Write cache first, DB later asynchronously.

Benefit:

- very fast writes

Risk:

- data loss if cache fails before DB write

## Cache Invalidation

Hard problem:

- delete cache on write
- use short TTL
- version keys
- event-based invalidation

## Hot Keys

A hot key receives too much traffic.

Solutions:

- local in-process cache
- key replication
- request coalescing
- split key by shard when possible

## Distributed Locks

Basic Redis lock:

```text
SET lock:job123 owner NX PX 30000
```

Unlock only if owner matches.

Warning:

> Distributed locks are tricky. Prefer idempotency and database constraints when possible.

