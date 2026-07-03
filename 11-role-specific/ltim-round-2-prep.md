# LTIM Round 2 Prep

## Current Status

```text
Interview: LTIMindtree Round 2
Time: Monday 3 PM
Role: Go + Node.js Backend Engineer
Goal: clear technical + scenario discussion
Documents / UAN issue: can hold for now
Priority: Interview prep mode activated
```

Main line:

```text
Round 2 is now real. Documents paused. Interview prep starts now.
```

How to use this file:

```text
Read answers aloud.
Do not memorize word by word.
Memorize the structure and key phrases.
Sound calm, practical, and honest.
```

Resume anchors:

```text
5 years backend / infrastructure experience
Current company: Brevo, Software Engineer, Jan 2023-present
Previous role: Brevo / Sendinblue, Associate Software Engineer, Jun 2021-Dec 2022
Earlier: Invatu Technologies internship, Aug 2020-Feb 2021
Core stack: Go, Node.js, REST APIs, Kafka, ClickHouse, Redis, MongoDB, GCP, OVH Cloud
Main themes: backend services, internal tools, URL shortener, cloud migration, data pipelines, production troubleshooting, performance optimization
```

## 1. Opening / HR-Technical Questions

### Q. Tell me about yourself.

Answer:

```text
I am a Backend and Infrastructure Engineer with around 5 years of experience, mostly working on backend services, APIs, data services, cloud migrations, and production troubleshooting.

My recent stronger experience is in Go, and I have also worked with Node.js, REST APIs, Kafka, ClickHouse, Redis, MongoDB, GCP, and OVH Cloud.

At Brevo, I worked on scalable backend services and internal tools. Some of my work involved a URL shortener service, migration of services to cloud infrastructure, Kafka and ClickHouse-based data services, API optimization, and production issue resolution.

For this role, I see a good fit because it needs Go and Node.js backend development, REST APIs, database understanding, and real production problem-solving. My recent experience is closest to backend/platform engineering, which aligns well with this role.
```

### Q. Why are you a fit for this Go + Node.js backend role?

Answer:

```text
I fit this role because my background is backend-heavy. I have worked on backend services, REST APIs, internal tools, data pipelines, databases, messaging, cloud infrastructure, and production issues.

My stronger recent hands-on experience is in Go, especially backend services and infrastructure-related work. I also have Node.js experience and I am currently refreshing Node.js through a TypeScript Express project so I can be comfortable in both stacks.

I may not claim equal depth in every technology, but I understand backend fundamentals well: APIs, database design, messaging, error handling, async processing, logging, troubleshooting, and deployment concerns.
```

### Q. How comfortable are you with Node.js if your recent experience is stronger in Go?

Answer:

```text
My recent stronger experience is definitely Go, but I started with Node.js earlier and I understand the fundamentals: event loop, async I/O, promises, async-await, Express middleware, REST APIs, error handling, and package management.

I am being honest that Go is more recent for me, but I am actively refreshing Node.js hands-on through a small TypeScript Express backend project. So I am comfortable contributing to Node.js services and ramping up quickly.
```

### Q. What are you currently revising for this interview?

Answer:

```text
I am revising Go fundamentals, Node.js fundamentals, SQL joins and database concepts, REST APIs, Kafka/Redis basics, production debugging, and common coding patterns.

I am also preparing my project explanations clearly using problem, architecture, my role, challenge, solution, and result.
```

## 2. Resume / Project Explanation

### Project Answer Template

Use this for every project:

```text
Problem:
What problem were we solving?

Architecture:
What services, APIs, databases, queues, caches, and cloud pieces were involved?

My role:
What did I personally design, build, debug, or own?

Challenge:
What was difficult?

Solution:
What did I do and why?

Result:
What improved?
```

### Q. Explain your current or recent backend work.

Answer:

```text
In my recent role at Brevo, I worked as a Software Engineer on backend and infrastructure-related systems.

My work involved scalable backend services, internal tools, data services, cloud migration, and production troubleshooting. The stack included Go, Kafka, ClickHouse, Redis, MongoDB, GCP, and OVH Cloud depending on the project.

One important part was working on services and pipelines where events or data needed to be ingested, processed, transferred, and queried efficiently. I also worked on API/backend improvements, operational reliability, and debugging production issues.

My main learning is that backend engineering is not only writing APIs. It also includes reliability, performance, data correctness, deployment safety, observability, and clear communication during production issues.
```

Interview hook:

```text
Recent role = Brevo Software Engineer.
Strong themes = Go backend, Kafka/ClickHouse data services, cloud migration, production troubleshooting.
```

### Q. Explain the URL shortener project.

Answer:

```text
At Brevo, I worked on a URL shortener service used to streamline internal communication workflows across teams.

The basic idea of a URL shortener is to accept a long URL, generate a short code, store the mapping, and redirect users from the short URL to the original URL with low latency.

Important backend concerns are short code generation, avoiding collisions, database schema, redirect latency, caching, analytics if needed, and rate limiting or abuse prevention.

In an interview, I would design it with an API to create short links, a redirect endpoint, a database table for mappings, and a cache for frequently accessed short codes. For scale, I would keep the redirect path very fast because that is the most frequently used operation.
```

Follow-up points:

```text
Short code generation can be random, hash-based, or sequence-based with Base62 encoding.
Cache can reduce DB reads during redirect.
DB schema can include id, short_code, original_url, created_by, created_at, expires_at, click_count.
```

### Q. Explain your Kafka / ClickHouse data pipeline work.

Answer:

```text
At Brevo, I worked around large-scale data services where Kafka and ClickHouse were used for efficient data ingestion and transfer pipelines.

At a high level, backend services produce events, Kafka handles asynchronous streaming, consumers process or transform those events, and ClickHouse stores data for analytics-style queries because it is column-oriented and efficient for large read-heavy workloads.

The important backend concerns were duplicate handling, retries, offset management, data consistency, and query performance.

For Kafka, I prefer committing offsets only after successful processing. If processing fails, I would retry with backoff, and after max retries send the message to a DLQ. Since duplicate processing can happen in at-least-once systems, consumers should be idempotent using event IDs, unique constraints, or processed-event tracking.
```

Follow-up points:

```text
Kafka = ingestion and decoupling.
Consumers = processing / transformation / delivery.
ClickHouse = fast analytical reads.
Main risks = duplicate events, retry handling, offset commits, schema design, and query performance.
```

### Q. Explain one production issue you handled.

Answer:

```text
At Brevo, I participated in production issue resolution and system troubleshooting. I would explain a production issue using this structure: problem, impact, investigation, fix, result, and prevention.

First I identify the user or business impact. Then I check logs, metrics, recent deploys, dependency health, and whether the issue can be reproduced.

If the issue is severe, the first priority is to stop the bleeding. That can mean rollback, disabling a feature, applying a quick config change, or reducing traffic to the faulty path.

After mitigation, I continue root cause analysis and add prevention, such as better logs, alerts, tests, validation, dashboard checks, or deployment safeguards.
```

Safe example if asked for details:

```text
One safe example is a backend service or data pipeline issue where processing slowed down or errors increased. I would check logs and metrics, identify whether the issue came from code, DB, Kafka consumer lag, external dependency, or recent deployment, then mitigate first and follow up with RCA and prevention.
```

### Q. Explain your cloud migration work.

Answer:

```text
At Brevo, I contributed to migration work involving Google Cloud Platform and later OVH cloud infrastructure.

The main goal of cloud migration is to move services or workloads while keeping downtime, cost, and risk controlled.

The important steps are understanding the current architecture, service dependencies, data flow, configuration, secrets, networking, deployment process, and monitoring.

My approach is to migrate incrementally, validate each service, keep rollback options ready, and monitor logs, metrics, latency, errors, and resource usage after migration.
```

Follow-up points:

```text
GCP migration experience from Associate Software Engineer role.
OVH migration contribution from Software Engineer role.
Talk about performance, cost, reliability, validation, and rollback.
```

## 3. Go Questions And Answers

### Q. What is a goroutine?

Answer:

```text
A goroutine is a lightweight unit of execution managed by the Go runtime. It lets us run functions concurrently.

Goroutines are much cheaper than OS threads, so Go can run many concurrent tasks efficiently. But they are not free, so in production I avoid unlimited goroutines and prefer bounded concurrency using worker pools, semaphores, or rate limits.
```

### Q. What is a channel?

Answer:

```text
A channel is used for communication between goroutines. One goroutine can send data into a channel and another can receive from it.

I use channels when goroutines need to communicate or coordinate. If the problem is only protecting shared data, a mutex can be simpler and clearer.
```

### Q. Buffered vs unbuffered channel?

Answer:

```text
An unbuffered channel blocks the sender until a receiver is ready. It is useful for synchronization.

A buffered channel has capacity, so sends can continue until the buffer is full. It is useful when we want limited queuing or burst handling.
```

### Q. What is WaitGroup?

Answer:

```text
WaitGroup is used to wait for multiple goroutines to finish.

Add increments the counter, Done decrements it, and Wait blocks until the counter becomes zero.
```

### Q. What is context cancellation?

Answer:

```text
Context is used to pass cancellation, timeout, deadline, and request-scoped values across API boundaries.

In backend services, if an HTTP request is cancelled or times out, context helps stop downstream work like DB queries, API calls, or background processing so resources are not wasted.
```

### Q. Explain worker pool.

Answer:

```text
A worker pool limits concurrency by starting a fixed number of goroutines.

There is usually a jobs channel. Workers read jobs from that channel, process them, and optionally send results to a result channel. A WaitGroup waits for all workers to finish. This prevents starting unlimited goroutines and protects the system from overload.
```

### Q. When would you use goroutine without channel?

Answer:

```text
I use a goroutine without a channel when I only need to run independent background work and I do not need a result back through a channel.

For example, logging, metrics emission, cleanup, or starting a background server task can be done with goroutines. If I need to wait, I may use WaitGroup. If I need cancellation, I pass context.
```

### Q. When would you use channel?

Answer:

```text
I use channels when goroutines need to exchange data or signal events.

Examples are producer-consumer, worker pools, fan-in/fan-out, pipelines, cancellation signals, and streaming results.
```

### Q. When would you use mutex?

Answer:

```text
I use a mutex when multiple goroutines need to safely access shared memory, like a map or counter.

For example, Go maps are not safe for concurrent writes, so if many goroutines update a map, I protect it using sync.Mutex or sync.RWMutex.
```

### Q. How does Go handle errors?

Answer:

```text
Go handles errors explicitly using return values. A function usually returns result and error.

I check the error, add context if needed, and return it to the caller. I use errors.Is for checking known/sentinel errors and errors.As when I need to extract a custom error type.
```

### Q. Panic vs error?

Answer:

```text
Errors are for expected failures like invalid input, DB error, network error, or not found.

Panic should be reserved for unrecoverable programmer mistakes or impossible states. In normal backend business logic, I prefer returning errors.
```

### Q. Slice vs map?

Answer:

```text
A slice is an ordered dynamic view over an array. It is good when order matters or when we need indexed iteration.

A map stores key-value pairs and is good for fast lookup by key.

In interviews, for graph problems with nodes 0 to n-1, I use slices like [][]int and []bool. For random IDs or strings, I use map.
```

### Q. What is defer?

Answer:

```text
defer schedules a function to run when the surrounding function returns. Deferred calls run in LIFO order, like a stack.

It is useful for cleanup, such as closing files, unlocking mutexes, or cancelling contexts.
```

## 4. Node.js Questions And Answers

### Q. What is Node.js event loop?

Answer:

```text
The event loop is what allows Node.js to handle many asynchronous operations without blocking the main JavaScript thread.

JavaScript execution is single-threaded, but async I/O is handled through the event loop and libuv, so Node can serve many I/O-heavy requests efficiently.
```

### Q. What is libuv?

Answer:

```text
libuv is the C library underneath Node.js. It provides the event loop, async I/O, timers, networking, and a thread pool for some operations like file system, DNS, and crypto.
```

### Q. Is Node.js single-threaded?

Answer:

```text
JavaScript execution in Node.js is single-threaded, but Node.js as a platform is not only one thread.

It uses libuv and a thread pool for some async operations. For CPU-heavy work, we can use worker_threads or separate services so the event loop does not get blocked.
```

### Q. Promise vs async-await?

Answer:

```text
A Promise represents a future async result. async-await is syntax built on top of promises that makes asynchronous code easier to read.

I still use try-catch with async-await for error handling.
```

### Q. What is middleware in Express?

Answer:

```text
Middleware is a function that runs during the request-response lifecycle.

It can be used for logging, authentication, validation, rate limiting, parsing request bodies, and error handling.
```

### Q. What are streams in Node.js?

Answer:

```text
Streams process data chunk by chunk instead of loading the full data into memory.

They are useful for large files, uploads, downloads, logs, CSV processing, and video/audio transfer.
```

### Q. When would you use worker_threads?

Answer:

```text
I would use worker_threads for CPU-heavy tasks because CPU-heavy work can block the event loop.

For I/O-heavy tasks, normal async I/O and promises are usually enough.
```

### Q. How do you structure an Express API?

Answer:

```text
I prefer separating app setup, server startup, routes, controllers, services, middleware, and config.

Routes define endpoints, controllers handle request/response, services contain business logic, middleware handles cross-cutting concerns like auth, validation, logging, and errors.
```

### Q. How do you handle errors in Express?

Answer:

```text
I use a centralized error-handling middleware. Controllers should pass errors to next or throw inside an async wrapper, and the error middleware converts them into consistent HTTP responses.

This keeps error handling consistent across APIs.
```

## 5. SQL / DB Questions And Answers

### Q. What is INNER JOIN?

Answer:

```text
INNER JOIN returns only matching rows from both tables.

For example, if users and orders are joined on user_id, INNER JOIN returns users who have matching orders.
```

### Q. What is LEFT JOIN?

Answer:

```text
LEFT JOIN returns all rows from the left table and matching rows from the right table. If there is no match, right-side columns are NULL.

It is useful when we want all primary records even if related data is missing.
```

### Q. What is GROUP BY?

Answer:

```text
GROUP BY groups rows so aggregate functions like COUNT, SUM, AVG, MIN, and MAX can be calculated per group.
```

### Q. What is a view?

Answer:

```text
A view is a saved reusable query. It helps simplify complex joins and expose a clean read model.

It does not necessarily store data by itself unless it is a materialized view, depending on the database.
```

### Q. What is the difference between a view and a materialized view?

Answer:

```text
A normal view is like a saved SQL query. It does not usually store the result data separately. When we query the view, the database runs the underlying query and returns the latest data.

A materialized view stores the query result physically. Because the result is precomputed, reads can be faster, especially for heavy joins or aggregations.

The tradeoff is freshness. A normal view usually shows latest data because it runs the query at read time. A materialized view can become stale and needs refresh, either manually or on a schedule depending on the database.
```

Interview line:

```text
View = saved query, fresh but may be slower.
Materialized view = stored result, faster reads but needs refresh.
```

### Q. Normalization vs denormalization?

Answer:

```text
Normalization reduces duplication and improves consistency by splitting data into related tables. It is good for write consistency and clean schema design.

Denormalization stores some duplicated data to make reads faster and queries simpler. The tradeoff is more storage and harder updates.
```

### Q. What is ACID?

Answer:

```text
ACID describes transaction reliability.

Atomicity means all or nothing.
Consistency means data remains valid.
Isolation means concurrent transactions do not corrupt each other.
Durability means committed data survives failures.
```

### Q. What is an index?

Answer:

```text
An index is a data structure that speeds up reads, filters, joins, and sorting.

The tradeoff is extra storage and slower writes because the index also needs to be updated when data changes.
```

### Q. What is replication?

Answer:

```text
Replication means copying data across multiple database nodes.

It helps with high availability, failover, read scaling, and disaster recovery.
```

### Q. SQL vs NoSQL?

Answer:

```text
SQL databases are relational and good for structured data, joins, transactions, and strong consistency.

NoSQL databases like MongoDB are flexible and document-oriented. They are useful when schema changes often or when data naturally fits as documents.

The choice depends on access patterns, consistency needs, query requirements, and scale.
```

## 6. Kafka / Redis / MongoDB / ClickHouse

### Q. What is Kafka?

Answer:

```text
Kafka is used for asynchronous event-driven communication.

Producers write events to topics. Topics are split into partitions. Consumers read messages from partitions, and consumer groups divide partitions among consumers for parallel processing.
```

### Q. What is an offset?

Answer:

```text
Offset is the position of a message inside a Kafka partition.

A consumer commits offsets to tell Kafka how far it has successfully processed.
```

### Q. When should offset be committed?

Answer:

```text
I prefer committing offsets only after processing succeeds.

If processing fails, I retry with backoff. After max retries, I send the message to a DLQ and then commit the original offset so the consumer is not blocked forever.
```

### Q. How do you avoid duplicate processing in Kafka?

Answer:

```text
Kafka commonly gives at-least-once processing, so duplicates can happen.

Consumers should be idempotent using event IDs, unique constraints, or processed-event tracking.
```

### Q. What is a Kafka consumer group?

Answer:

```text
A consumer group is a group of consumers that share work for a topic.

Within the same consumer group, one partition is consumed by only one consumer at a time. This gives parallelism while preserving ordering inside each partition.
```

### Q. Does Kafka guarantee ordering?

Answer:

```text
Kafka guarantees ordering only within a partition, not across the whole topic.

If ordering matters for a specific entity, like user_id or account_id, messages for that entity should use the same partition key so they land in the same partition.
```

### Q. What is DLQ in Kafka?

Answer:

```text
DLQ means dead-letter queue. It stores messages that failed processing even after retries.

This prevents one bad message from blocking the consumer forever and allows the team to inspect or reprocess failed messages later.
```

### Q. What is Redis used for?

Answer:

```text
Redis is an in-memory data store used for caching, counters, rate limiting, sessions, queues, and distributed locks.
```

### Q. Explain cache-aside.

Answer:

```text
In cache-aside, the application checks cache first.

If data is missing, it reads from DB, stores the result in cache with TTL, and then returns it. This reduces database load for frequent reads.
```

### Q. What is cache invalidation?

Answer:

```text
Cache invalidation means removing or updating stale cache entries when underlying data changes.

Common approaches are TTL-based expiry, deleting cache on write, or updating cache after successful DB update. The right choice depends on freshness requirement.
```

### Q. How can Redis be used for rate limiting?

Answer:

```text
Redis can store counters with TTL for fixed-window rate limiting, or tokens for token-bucket style limiting.

For example, per user or per IP, increment a Redis key for each request and set expiry for the time window. If count crosses the limit, return HTTP 429.
```

### Q. What is MongoDB?

Answer:

```text
MongoDB is a document database that stores data in BSON documents.

It is useful when data naturally fits as nested documents or when schema changes frequently. It is not a replacement for every SQL use case; the choice depends on access patterns and consistency needs.
```

### Q. When would you use MongoDB over SQL?

Answer:

```text
I would use MongoDB when the data is document-oriented, schema is flexible, and reads usually need the whole document together.

I would prefer SQL when I need strong relational modeling, joins, complex transactions, and strict consistency.
```

### Q. What are indexes in MongoDB?

Answer:

```text
MongoDB indexes speed up queries on fields, similar to indexes in SQL databases.

They improve read performance but add storage and write overhead. For frequent filters or sort fields, indexes are important.
```

### Q. What is a compound index?

Answer:

```text
A compound index is an index on multiple fields.

For example, if queries often filter by user_id and status together, a compound index on user_id and status can help. Field order matters because MongoDB uses the prefix of the compound index.
```

### Q. What is MongoDB replication?

Answer:

```text
MongoDB replication uses replica sets. Data is copied from a primary node to secondary nodes.

This improves high availability and failover. If primary goes down, a secondary can be elected as primary.
```

### Q. What is ClickHouse?

Answer:

```text
ClickHouse is a column-oriented database used for analytics workloads.

It is good when queries scan many rows but only need a few columns, such as reporting, dashboards, and event analytics.
```

## 7. REST API / Microservices

### Q. What makes an API RESTful?

Answer:

```text
A REST API uses resources, HTTP methods, stateless requests, proper status codes, and predictable URLs.

For example, GET reads data, POST creates data, PUT/PATCH updates data, and DELETE removes data.
```

### Q. Common HTTP status codes?

Answer:

```text
200 means success.
201 means created.
400 means bad request.
401 means unauthenticated.
403 means unauthorized.
404 means not found.
409 means conflict.
429 means too many requests.
500 means server error.
```

### Q. Authentication vs authorization?

Answer:

```text
Authentication answers who the user is.

Authorization answers what the user is allowed to do.
```

### Q. What is idempotency?

Answer:

```text
Idempotency means retrying the same operation multiple times has the same final effect as doing it once.

It is important for payments, retries, order creation, and distributed systems where duplicate requests can happen.
```

### Q. How do microservices communicate?

Answer:

```text
Microservices can communicate synchronously using HTTP or gRPC, and asynchronously using queues or Kafka.

Synchronous calls are simpler for request-response flows, while async messaging improves decoupling and resilience for event-driven workflows.
```

## 8. Production Issue Scenarios

### Q. How would you debug a failing API in production?

Answer:

```text
First I would check the impact: which users, which endpoint, since when, and error rate.

Then I would check logs, metrics, traces if available, recent deployments, database health, external dependencies, and configuration changes.

If impact is high, I would mitigate first by rollback, disabling a feature, or applying a quick safe fix. After that I would perform root cause analysis and add prevention like alerts, tests, better logs, or dashboard checks.
```

### Q. How do you communicate an incident to non-technical people?

Answer:

```text
I keep it simple and impact-focused.

I mention what happened, who is affected, current status, mitigation, workaround if any, ETA or next update time, and later I share root cause and prevention steps.
```

### Q. How would you investigate high latency?

Answer:

```text
I would check whether latency is from application code, database, external dependency, network, or resource saturation.

I would look at API latency metrics, slow queries, DB indexes, CPU/memory, connection pool usage, logs, recent deploys, and traces if available.
```

### Q. How would you handle unclear requirements?

Answer:

```text
I would first clarify the goal, users, input/output, constraints, edge cases, and success criteria.

If everything is not clear, I would document assumptions, propose a small approach, confirm with stakeholders, and build incrementally.
```

## 9. Coding Patterns

Snippet note:

```text
These snippets are for interview recall.
Use vanilla Go slices and maps.
Worker pool snippet needs imports: context, fmt, sync.
Rate limiter snippet needs imports: sync, time.
```

### Q. Valid Anagram approach?

Answer:

```text
If strings contain lowercase English letters, I use a fixed array of size 26.

I increment counts for characters in the first string and decrement for the second string. If all counts are zero, the strings are anagrams.

Time complexity is O(n), space is O(1) because the array size is fixed.
```

Small Go snippet:

```go
func isAnagram(s string, t string) bool {
	if len(s) != len(t) {
		return false
	}

	count := make([]int, 26)
	for i := 0; i < len(s); i++ {
		count[s[i]-'a']++
		count[t[i]-'a']--
	}

	for _, c := range count {
		if c != 0 {
			return false
		}
	}
	return true
}
```

### Q. Two Sum approach?

Answer:

```text
Use a map from number to index.

For each number, calculate complement = target - number. If complement exists in map, return both indexes. Otherwise store current number and index.

Time complexity is O(n), space is O(n).
```

Small Go snippet:

```go
func twoSum(nums []int, target int) []int {
	seen := map[int]int{}

	for i, num := range nums {
		need := target - num
		if j, ok := seen[need]; ok {
			return []int{j, i}
		}
		seen[num] = i
	}

	return nil
}
```

### Q. Min Stack approach?

Answer:

```text
Use two stacks.

The main stack stores all values. The min stack stores the current minimum at every level.

On push, push the value to main stack and push min(value, currentMin) to min stack. On pop, pop from both. getMin returns top of min stack.
```

Small Go snippet:

```go
type MinStack struct {
	stack []int
	min   []int
}

func (m *MinStack) Push(val int) {
	m.stack = append(m.stack, val)

	if len(m.min) == 0 || val < m.min[len(m.min)-1] {
		m.min = append(m.min, val)
		return
	}

	m.min = append(m.min, m.min[len(m.min)-1])
}

func (m *MinStack) Pop() {
	if len(m.stack) == 0 {
		return
	}

	m.stack = m.stack[:len(m.stack)-1]
	m.min = m.min[:len(m.min)-1]
}

func (m *MinStack) Top() int {
	return m.stack[len(m.stack)-1]
}

func (m *MinStack) GetMin() int {
	return m.min[len(m.min)-1]
}
```

### Q. BFS approach?

Answer:

```text
BFS uses a queue and explores level by level.

For LeetCode-style graphs with nodes 0 to n-1, I use [][]int for graph, []bool for visited, and []int as queue.
```

Small Go snippet:

```go
func bfs(graph [][]int, start int) []int {
	visited := make([]bool, len(graph))
	queue := []int{start}
	visited[start] = true

	var order []int
	for len(queue) > 0 {
		node := queue[0]
		queue = queue[1:]
		order = append(order, node)

		for _, next := range graph[node] {
			if visited[next] {
				continue
			}
			visited[next] = true
			queue = append(queue, next)
		}
	}

	return order
}
```

### Q. DFS approach?

Answer:

```text
DFS explores one path deeply before backtracking.

It can be implemented using recursion or an explicit stack. In interviews, recursive DFS is usually fastest to write if recursion depth is safe.
```

Small Go snippet:

```go
func dfs(graph [][]int, start int) []int {
	visited := make([]bool, len(graph))
	var order []int

	var visit func(int)
	visit = func(node int) {
		if visited[node] {
			return
		}

		visited[node] = true
		order = append(order, node)

		for _, next := range graph[node] {
			visit(next)
		}
	}

	visit(start)
	return order
}
```

### Q. Course Schedule approach?

Answer:

```text
Course Schedule is topological sort using Kahn's algorithm.

I build a graph from prerequisite to course and an indegree array where indegree[course] means pending prerequisites.

Then I push all courses with indegree 0 into a queue. I process the queue, reduce indegree of dependent courses, and if any becomes 0, I push it into the queue.

If processed count equals numCourses, all courses can be completed. Otherwise there is a cycle.
```

Memory hook:

```text
graph -> indegree -> zero queue -> process -> unlock -> count
```

Small Go snippet:

```go
func canFinish(numCourses int, prerequisites [][]int) bool {
	graph := make([][]int, numCourses)
	indegree := make([]int, numCourses)

	for _, pre := range prerequisites {
		course := pre[0]
		prereq := pre[1]

		graph[prereq] = append(graph[prereq], course)
		indegree[course]++
	}

	var queue []int
	for course, degree := range indegree {
		if degree == 0 {
			queue = append(queue, course)
		}
	}

	processed := 0
	for len(queue) > 0 {
		course := queue[0]
		queue = queue[1:]
		processed++

		for _, next := range graph[course] {
			indegree[next]--
			if indegree[next] == 0 {
				queue = append(queue, next)
			}
		}
	}

	return processed == numCourses
}
```

### Q. Worker pool coding approach?

Answer:

```text
Create jobs channel and results channel.

Start fixed number of workers as goroutines. Each worker reads from jobs, processes work, and sends result.

Use WaitGroup to wait for workers. Close results only after all workers finish. Use context cancellation for graceful shutdown.
```

Small Go snippet:

```go
type Job struct {
	ID int
}

type Result struct {
	JobID int
	Text  string
}

func spawnWorkers(ctx context.Context, workers int, jobs <-chan Job, wg *sync.WaitGroup) <-chan Result {
	results := make(chan Result)
	wg.Add(workers)

	for i := 1; i <= workers; i++ {
		workerID := i

		go func(workerID int) {
			defer wg.Done()

			for {
				select {
				case <-ctx.Done():
					return
				case job, ok := <-jobs:
					if !ok {
						return
					}

					select {
					case <-ctx.Done():
						return
					case results <- Result{
						JobID: job.ID,
						Text:  fmt.Sprintf("done by worker %d", workerID),
					}:
					}
				}
			}
		}(workerID)
	}

	go func() {
		wg.Wait()
		close(results)
	}()

	return results
}
```

### Q. Rate limiter coding approach?

Answer:

```text
For a simple in-memory rate limiter, I keep a map from user/IP key to request count and window expiry.

I protect the map with mutex because multiple requests can access it concurrently. If the current time is past the window, I reset count and expiry. If count crosses limit, I return false and API returns HTTP 429.

For distributed production systems, I would use Redis instead of local memory.
```

Small Go snippet:

```go
type UserLimit struct {
	Count       int
	WindowEnds time.Time
}

type RateLimiter struct {
	mu     sync.Mutex
	limit  int
	window time.Duration
	users  map[string]UserLimit
}

func NewRateLimiter(limit int, window time.Duration) *RateLimiter {
	return &RateLimiter{
		limit:  limit,
		window: window,
		users:  map[string]UserLimit{},
	}
}

func (r *RateLimiter) Allow(key string) bool {
	now := time.Now()

	r.mu.Lock()
	defer r.mu.Unlock()

	curr := r.users[key]
	if now.After(curr.WindowEnds) {
		curr = UserLimit{
			Count:       0,
			WindowEnds: now.Add(r.window),
		}
	}

	if curr.Count >= r.limit {
		r.users[key] = curr
		return false
	}

	curr.Count++
	r.users[key] = curr
	return true
}
```

## 10. System Design Quick Answers

### Q. How would you design a URL shortener?

Answer:

```text
I would start with two main APIs: create short URL and redirect short URL.

For create, the service accepts a long URL, generates a short code, stores the mapping in DB, and returns the short URL.

For redirect, the service receives the short code, looks it up in cache first, falls back to DB if missing, and returns HTTP redirect to the original URL.

Important concerns are short code generation, collision handling, low redirect latency, cache, DB schema, expiry, analytics, and rate limiting.
```

Simple components:

```text
API service
DB table for short_code -> original_url
Redis cache for hot short codes
Analytics/event pipeline if click tracking is needed
Rate limiter for abuse control
```

### Q. How would you design a rate limiter?

Answer:

```text
For a single instance, I can use an in-memory map with mutex where key is user ID, IP, or API key, and value stores count and window expiry.

For production distributed systems, I would use Redis because multiple app instances need shared rate limit state.

Common algorithms are fixed window, sliding window, and token bucket. If limit is exceeded, API returns HTTP 429.
```

Interview line:

```text
Local rate limiter = map + mutex.
Distributed rate limiter = Redis counter/token bucket with TTL.
```

### Q. How would you design a notification system?

Answer:

```text
I would keep notification sending asynchronous.

The main service creates a notification event, pushes it to Kafka or a queue, and workers consume events to send email, SMS, or push notification.

I would store user preferences, notification status, retry count, and delivery result. Failed notifications can be retried and eventually sent to DLQ.
```

Components:

```text
API service
Notification DB
Queue / Kafka
Worker service
User preferences
Retry + DLQ
Email/SMS/push providers
```

### Q. How would you design the Job Application Tracker backend?

Answer:

```text
I would build it as a REST backend with users, companies, job applications, interview rounds, and notes.

Users can create job applications, update status, add rounds, add notes, and filter applications by company, status, or applied date.

The backend would use authentication, validation, pagination, and proper status codes. Initially I would use PostgreSQL for relational data because applications, companies, users, and rounds have clear relationships.
```

Tables:

```text
users
companies
job_applications
interview_rounds
application_notes
```

### Q. How do you scale APIs?

Answer:

```text
First I would identify the bottleneck: application CPU, DB queries, cache misses, external dependency, or network.

Common scaling steps are stateless API servers behind a load balancer, DB indexing, caching hot reads, pagination, async processing for heavy work, connection pooling, and monitoring.

I would not scale blindly. I would use metrics and profiling to find the real bottleneck first.
```

### Q. Where would you use Redis cache?

Answer:

```text
I would use Redis for hot read-heavy data, short-lived computed results, sessions, counters, rate limiting, and frequently accessed mappings like URL shortener redirects.

The important concern is cache invalidation. I would use TTL, delete-on-write, or update-on-write depending on freshness needs.
```

### Q. When would you use async processing?

Answer:

```text
I would use async processing when work does not need to finish inside the user request.

Examples are sending notifications, processing analytics events, generating reports, heavy file processing, or retryable external API calls.

This improves API latency and reliability because the request can return quickly while background workers process the task.
```

### Q. How would you debug latency in a distributed system?

Answer:

```text
I would break latency down service by service.

First check API latency metrics, logs, traces, DB query time, cache hit ratio, external dependency latency, queue lag, CPU/memory, and recent deploys.

Distributed tracing is useful because trace ID and spans show where time is spent across services.
```

Interview line:

```text
Logs tell what happened, metrics show system health, traces show request path across services.
```

## 11. Questions To Ask Interviewer

Ask:

```text
What kind of backend services would this role work on?
Is the team using Go, Node.js, or both in active services?
What databases and messaging systems are used most often?
How are production incidents and deployments handled?
What would success look like in the first 3 months?
```

Avoid:

```text
Do not ask salary in technical round unless interviewer brings it up.
Do not sound desperate for any role.
Do not overclaim Node.js as recent strongest experience.
```

## 12. Final Revision Order Before Interview

Last 45 minutes:

```text
1. Tell me about yourself
2. Recent project explanation
3. Go goroutine/channel/WaitGroup/context
4. Node event loop/middleware/async-await
5. SQL JOIN/VIEW/ACID/normalization
6. Kafka offset/retry/DLQ/idempotency
7. Production incident answer
8. URL shortener / rate limiter system design
9. Course Schedule hook
10. Questions to ask interviewer
```
