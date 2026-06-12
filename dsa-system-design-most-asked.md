# DSA + System Design Most Asked

LinkedIn takeaway:

> If you want to clear software engineer interviews, the practical formula is DSA + System Design.

For Go backend roles, that is useful but incomplete. A better formula is:

```text
DSA + System Design + Go/Backend Fundamentals + Resume Project Explanation
```

For freshers, DSA dominates. For 5 YOE backend roles, system design and backend fundamentals become equally important.

Use this as a revision tracker. The goal is not to memorize answers, but to know the pattern, tradeoffs, and the first 5 minutes of how you would start solving each one.

## Priority For Your Target Roles

You are not preparing only for generic SDE-1 interviews. You are preparing for:

```text
Go backend
distributed systems
data infrastructure
platform roles
```

Priority order:

```text
1. Go + backend fundamentals
2. System design
3. DSA patterns
4. Instagram clone / Kafka / ClickHouse project explanation
```

Interviewers may ask DSA, but they can also ask:

```text
How does Kafka work?
How do you design retries?
How do you handle idempotency?
How do you scale APIs?
How do you debug production issues?
How do goroutines/channels work?
How do you design a feed/data pipeline/notification system?
```

## DSA Questions

| # | Question | Pattern | Current Home | Status |
|---|---|---|---|---|
| 1 | Implement LRU Cache | Hash map + doubly linked list | `algos`, `09-data-structures-algorithms/missing-patterns.md` | Needs runnable implementation |
| 2 | Implement LFU Cache | Hash map + frequency buckets | `algos` | Missing |
| 3 | Find Median from Data Stream | Two heaps | `algos` | Missing |
| 4 | Word Ladder shortest transformation | BFS | `algos` | Missing |
| 5 | Merge K Sorted Lists | Heap / divide and conquer | `algos` | Missing |
| 6 | Detect Cycle in a Directed Graph | DFS colors / topological sort | `algos/patterns/topologicalsort` | Partially covered |
| 7 | Maximum Subarray Sum | Kadane | `algos` | Missing |
| 8 | Kth Largest Element in a Stream | Min heap | `algos` | Missing |
| 9 | Design a Scheduler with Task Priorities | Heap / priority queue | `algos` + backend design | Missing |

## DSA Priority Order

Do these first:

```text
1. LRU Cache
2. Merge K Sorted Lists
3. Detect Cycle in Directed Graph
4. Maximum Subarray Sum
5. Kth Largest Element in a Stream
6. Find Median from Data Stream
7. Word Ladder
8. Task Scheduler with Priorities
```

Why this order:

```text
LRU is common and connects to Redis/cache design.
Merge K and Kth Largest teach heaps.
Cycle detection is important for graph/topological sort.
Kadane is easy but must know.
Median stream and Word Ladder are harder but important.
Scheduler is more design-ish and less standard.
```

## DSA Interview Angles

LRU Cache:

```text
Need O(1) get and put.
Use map[key]*node for lookup.
Use doubly linked list for recency order.
On get/put, move node to front.
When capacity is exceeded, evict tail.
```

LFU Cache:

```text
Need key lookup and frequency tracking.
Use key -> node map.
Use freq -> ordered set/list of nodes.
Track minFreq for eviction.
Evict least frequently used; break ties by least recently used.
```

Median From Data Stream:

```text
Use two heaps.
Max heap stores smaller half.
Min heap stores larger half.
Keep heap sizes balanced.
Median is top of one heap or average of both tops.
```

Word Ladder:

```text
This is shortest path in an unweighted graph.
Use BFS from beginWord.
Neighbors are words that differ by one character.
Optimize neighbor lookup using wildcard patterns like h*t.
```

Merge K Sorted Lists:

```text
Use min heap of current list heads.
Pop smallest node, append to result, push that node's next.
Time: O(N log k), where N is total nodes and k is number of lists.
```

Directed Cycle:

```text
Use DFS with 3 colors:
0 = unvisited
1 = visiting
2 = visited
If DFS reaches a visiting node, there is a cycle.
```

Kadane:

```text
At each index:
current = max(nums[i], current + nums[i])
best = max(best, current)
Handles "best subarray ending here" vs global best.
```

Kth Largest Stream:

```text
Maintain a min heap of size k.
Push new number.
If heap size exceeds k, pop smallest.
Heap top is kth largest.
```

Priority Scheduler:

```text
Use priority queue ordered by priority and created_at.
Higher priority executes first.
Use stable tie-breaking for same priority.
For production, discuss retries, delayed jobs, persistence, locks, and worker concurrency.
```

## System Design Questions

| # | Question | Current Home | Status |
|---|---|---|---|
| 1 | Scalable Chat App: WhatsApp/Slack | `system design/examples/chat-app.md` | Covered |
| 2 | URL Shortener | `system design/examples/url-shortener.md` | Covered |
| 3 | Distributed Notification System | `system design/examples/notification-system.md` | Covered |
| 4 | Video Streaming Platform | `system design/examples/video-upload.md` | Partially covered |
| 5 | Payment System: Razorpay/Stripe | `08-system-design-cases/payment-retry-system.md` | Partially covered |
| 6 | API Rate Limiter | `08-system-design-cases/rate-limiter.md` | Covered |
| 7 | E-commerce Checkout System | `go-backend-interview` | Missing |
| 8 | Ride-Hailing App: Uber/Ola | `system design/examples/ride-sharing.md` | Covered |

## System Design Priority Order

For Go/backend roles, do these first:

```text
1. URL Shortener
2. API Rate Limiter
3. Distributed Notification System
4. Chat Application
5. E-commerce Checkout System
6. Payment System
7. Video Streaming Platform
8. Ride-Hailing App
```

Why this order:

```text
URL shortener matches backend API and resume-style experience.
Rate limiter matches Redis/backend roles.
Notification system matches Kafka, queues, retries, and DLQ.
Chat teaches WebSocket, messaging, ordering, and fanout.
Checkout/payment teaches idempotency and consistency.
Video streaming and ride-hailing are larger and less urgent.
```

## System Design Interview Angles

Chat App:

```text
WebSocket gateway
conversation/message schema
online presence
delivery/read receipts
offline push notifications
message ordering
fanout and horizontal scaling
```

URL Shortener:

```text
short code generation
redirect path latency
database schema
cache hot URLs
analytics events
abuse/rate limiting
```

Notification System:

```text
event ingestion
user preferences
template rendering
provider routing
retry and DLQ
deduplication
rate limits
```

Video Streaming:

```text
upload service
object storage
transcoding pipeline
adaptive bitrate formats
CDN delivery
metadata service
watch progress
recommendation/feed integration
```

Payment System:

```text
idempotency keys
payment state machine
webhooks
reconciliation
ledger consistency
retry safely
audit logs
```

Rate Limiter:

```text
fixed window
sliding window
token bucket
Redis counters
distributed consistency
per-user/per-IP limits
fail open vs fail closed
```

E-commerce Checkout:

```text
cart service
inventory reservation
pricing and coupons
payment authorization
order creation
transaction boundaries
saga/outbox for async steps
rollback/compensation
```

Ride-Hailing:

```text
rider/driver location updates
geo indexing
matching
ETA calculation
trip state machine
pricing/surge
notifications
high write volume
```

## How To Use This Checklist

Do not try to do eight problems daily. Use a sustainable loop:

```text
Daily:
- 1 DSA problem
- 1 backend/system design topic
- 1 Go/backend revision topic
```

Smart connected study example:

```text
DSA: LRU Cache design
System Design: API Rate Limiter
Backend: Redis cache-aside + TTL + eviction
```

These connect naturally:

```text
LRU Cache in code -> Redis caching -> API Rate Limiter system design
```

That is smarter than random practice.

For each DSA item, prepare:

```text
pattern
data structures
time complexity
space complexity
edge cases
one clean Go implementation
```

For each system design item, prepare:

```text
requirements
scale assumptions
APIs
data model
high-level architecture
deep dive
failure handling
observability
tradeoffs
```

Bottom line:

```text
Go fundamentals
Concurrency
Kafka/Redis/DB
System design
Moderate DSA
Strong project explanation
Remote job targeting
```

Use the LinkedIn list as a roadmap, not a guilt weapon.
