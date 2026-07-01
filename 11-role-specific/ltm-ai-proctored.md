# LTM AI-Proctored Interview Pack - Go + Node Backend

Use this for a 2-3 hour revision before the 9 PM AI interview.

Priority:
- Main: Go, REST APIs, microservices, databases
- Secondary: Node.js safety answers
- Optional: Kafka/cloud/CI-CD only as quick backend revision

## 1. 2-Minute Self-Introduction

I am a backend engineer with experience building services using Go and Node.js, with stronger recent hands-on work in Go. My work has involved REST APIs, microservices, event-driven flows, databases, Redis caching, Kafka-style messaging concepts, and production debugging.

In recent backend work, I have focused on writing reliable services, handling concurrency safely, designing APIs, optimizing database access, and debugging production issues using logs, metrics, and traces. I am comfortable working with Go concepts like goroutines, channels, context cancellation, interfaces, error handling, and worker pools.

I started earlier with Node.js, so I understand async I/O, the event loop, middleware, streams, promises, and API development patterns. My current stronger coding confidence is in Go, but I can work with Node.js backend services and ramp quickly where needed.

For this role, I bring backend fundamentals, system thinking, and practical experience with APIs, databases, and production behavior.

## 2. Project Explanation Template

Use this exact structure:

- Problem: What problem existed?
- Architecture: What services, APIs, queues, DBs, caches were involved?
- My role: What did I personally build or own?
- Challenge: What was hard? Scale, correctness, latency, failures, data quality?
- Solution: What design or code change solved it?
- Result: What improved? Reliability, latency, cost, developer speed, customer impact?

Spoken template:
- In one backend project, the problem was ...
- We designed it using ...
- My responsibility was ...
- One challenge was ...
- I handled it by ...
- The result was ...

## 3. Go Development Revision

### Goroutine

- Definition: A lightweight concurrent function managed by the Go runtime.
- Interview answer: Goroutines let us run work concurrently without manually managing OS threads. They are cheap, but still need proper cancellation, synchronization, and resource control.
- Example use: Background jobs, parallel API calls, workers.
- Follow-up: How do you stop a goroutine?
- Safe answer: Use `context.Context`, done channels, or close input channels so goroutines exit cleanly.

### Channel

- Definition: A typed communication pipe between goroutines.
- Interview answer: Channels are used when goroutines need to communicate or coordinate ownership of data.
- Example use: Send jobs to workers, send results back.
- Follow-up: Who should close a channel?
- Safe answer: Usually the sender closes the channel because the sender knows when no more values will be sent.

### Buffered vs Unbuffered Channel

- Definition: Unbuffered blocks until sender and receiver meet. Buffered allows limited queued values.
- Interview answer: Unbuffered channels synchronize directly. Buffered channels decouple sender and receiver up to capacity.
- Follow-up: When use buffered?
- Safe answer: When small bursts are acceptable or for worker queues, but avoid hiding backpressure with huge buffers.

### WaitGroup

- Definition: Waits for a group of goroutines to finish.
- Interview answer: Use `sync.WaitGroup` when you start known goroutines and need main/control flow to wait for them.
- Follow-up: Where call `Add`?
- Safe answer: Call `Add` before starting goroutines to avoid races.

### Mutex

- Definition: Lock to protect shared memory.
- Interview answer: Use mutex when multiple goroutines access shared state like maps or counters.
- Follow-up: Channel or mutex?
- Safe answer: Use channels for communication/work ownership; use mutex for protecting shared in-memory state.

### Context Cancellation

- Definition: Carries cancellation, timeout, deadline, and request-scoped values.
- Interview answer: In backend systems, context lets us cancel work when requests timeout, clients disconnect, or shutdown begins.
- Follow-up: Where pass context?
- Safe answer: Pass it down call chains to DB calls, HTTP calls, goroutines, and workers.

### Defer

- Definition: Runs a function at the end of the current function, in LIFO order.
- Interview answer: Useful for cleanup like closing files, unlocking mutexes, and calling `cancel`.
- Follow-up: Is defer a stack?
- Safe answer: Deferred calls execute in last-in-first-out order.

### Error Handling

- Definition: Go returns errors as values.
- Interview answer: Prefer returning errors, wrapping with context, and checking with `errors.Is` or `errors.As`.
- Follow-up: Panic vs error?
- Safe answer: Use errors for expected failures. Use panic for unrecoverable programmer errors, not normal business flow.

### Interfaces

- Definition: A contract of methods. Types implement interfaces implicitly.
- Interview answer: Interfaces allow loose coupling and make code easier to test by depending on behavior, not concrete types.
- Follow-up: What is `interface{}` / `any`?
- Safe answer: `any` is an alias for `interface{}` and can hold values of any type.

### Slices and Maps

- Slice: Dynamic view over an array with length and capacity.
- Map: Hash table key-value structure.
- Interview answer: Slices can grow with `append`; maps must be initialized before writes.
- Follow-up: Nil map?
- Safe answer: Reading from nil map is okay, writing to nil map panics.

### Worker Pool

- Definition: Fixed number of workers processing jobs from a channel.
- Interview answer: A worker pool limits concurrency and prevents spawning unlimited goroutines.
- Follow-up: How close result channel?
- Safe answer: Wait for all workers with `WaitGroup`, then close the result channel.

### Rate Limiter

- Definition: Controls request rate per user/IP/key.
- Interview answer: Simple in-memory limiter can use map + mutex; distributed production limiter often uses Redis/token bucket/sliding window.
- Follow-up: What status code?
- Safe answer: Return HTTP `429 Too Many Requests`.

## 4. Node.js Development Revision

### Event Loop

- Definition: Node.js mechanism that schedules async callbacks.
- Interview answer: JavaScript runs on a single main thread, while async I/O is handled non-blockingly and callbacks are processed by the event loop.
- Follow-up: What blocks Node?
- Safe answer: CPU-heavy synchronous work blocks the event loop.

### libuv

- Definition: Native library used by Node.js for event loop, async I/O, and thread pool.
- Interview answer: libuv helps Node handle non-blocking I/O. Some tasks like fs, crypto, DNS use the libuv thread pool.
- Follow-up: Is `Promise.all` creating threads?
- Safe answer: No. It starts async operations concurrently; actual I/O is handled by Node/libuv/system.

### Async I/O

- Definition: I/O that does not block the JS main thread.
- Interview answer: Node is strong for I/O-heavy APIs because it can handle many concurrent requests without one thread per request.
- Follow-up: Is Node good for CPU-heavy work?
- Safe answer: Not directly on main thread; use worker threads or separate services.

### Promise

- Definition: Represents a future async result.
- Interview answer: Promises help compose async operations and handle success/failure cleanly.
- Follow-up: Why use `Promise.all`?
- Safe answer: To run independent async operations concurrently and wait for all results.

### Async/Await

- Definition: Syntax over promises for readable async code.
- Interview answer: It makes async flows easier to read, but errors still need `try/catch`.
- Follow-up: Does await block Node?
- Safe answer: It pauses that async function, not the entire event loop.

### Middleware

- Definition: Function in request pipeline.
- Interview answer: Middleware is used for auth, logging, validation, rate limiting, error handling, and request enrichment.
- Follow-up: Express error middleware?
- Safe answer: Error middleware has four args: `err, req, res, next`.

### Streams

- Definition: Process data chunk by chunk.
- Interview answer: Streams are useful for large files/uploads/downloads because they avoid loading everything into memory.
- Follow-up: What is backpressure?
- Safe answer: When consumer is slower than producer, stream mechanism slows producer to avoid memory overload.

### Error Handling

- Definition: Handle sync errors with try/catch and async errors through rejected promises or error middleware.
- Interview answer: In APIs, return proper status codes, log context, and avoid leaking internal errors.
- Follow-up: How handle async route errors?
- Safe answer: Use `try/catch`, async wrapper, or pass errors to central error middleware.

### Worker Threads

- Definition: Node.js feature for CPU-heavy work in separate threads.
- Interview answer: Use worker threads when CPU-bound tasks would block the event loop.
- Follow-up: When not needed?
- Safe answer: Normal I/O-heavy API calls usually do not need worker threads.

### Safe Node.js Positioning Answer

Question: You have more recent Go experience. How comfortable are you with Node.js?

Answer: My recent stronger production work and interview coding confidence is in Go, but I started earlier with Node.js and understand backend fundamentals like Express middleware, async/await, promises, event loop, streams, and error handling. I would position myself as stronger in Go today, but comfortable enough with Node.js backend work and able to ramp quickly.

## 5. Microservices & REST API Revision

### REST Methods

- Answer: `GET` reads, `POST` creates/action, `PUT` replaces, `PATCH` partially updates, `DELETE` removes.
- Example: `GET /users/1`, `POST /orders`.

### HTTP Status Codes

- Answer: `200` success, `201` created, `400` bad request, `401` unauthenticated, `403` unauthorized, `404` not found, `409` conflict, `429` rate limited, `500` server error.
- Example: Invalid payload returns `400`; duplicate idempotency key may return existing result or `409`.

### Idempotency

- Answer: Same request repeated should not create duplicate side effects.
- Example: Payment APIs use idempotency keys to avoid duplicate charges.

### Pagination

- Answer: Used to avoid returning too much data. Offset pagination is simple; cursor pagination is better for large changing datasets.
- Example: `GET /orders?limit=20&cursor=abc`.

### Rate Limiting

- Answer: Protects APIs from abuse and overload.
- Example: Per-user/IP token bucket or Redis counter returning `429`.

### Authentication vs Authorization

- Authentication: Who are you?
- Authorization: What are you allowed to do?
- Example: JWT identifies user; role/permission decides access.

### API Versioning

- Answer: Version APIs to change contracts safely.
- Example: `/v1/orders`, `/v2/orders`.

### Service-to-Service Communication

- Answer: Services communicate using HTTP/gRPC synchronously or queues/events asynchronously.
- Example: Order service calls payment service or emits `OrderCreated`.

### Logging and Tracing Basics

- Answer: Logs show events; metrics show numbers; traces show request flow across services.
- Example: Trace ID propagates across API calls to find slow/failing service.

### Monolith vs Microservices

- Answer: Monolith is simpler to deploy and debug; microservices help scale teams and independent services but add network, consistency, and observability complexity.

### Debugging a Failing API

- Answer: Check logs, metrics, recent deploys, dependency health, reproduce issue, inspect DB/cache, trace request, rollback if needed, then root cause.

## 6. Database Technologies Revision

### SQL vs NoSQL

- Question: When use SQL vs NoSQL?
- Safe answer: SQL for relational data, joins, transactions, strong schema. NoSQL for flexible schema, document-style data, high write scale, or access-pattern-based design.

### MongoDB Basics

- Question: What is MongoDB?
- Safe answer: Document database storing JSON-like BSON documents in collections. Good for flexible schema and nested data.

### Indexes

- Question: Why use indexes?
- Safe answer: Indexes speed reads by avoiding full collection/table scans, but slow writes and consume storage.

### Compound Indexes

- Question: What is a compound index?
- Safe answer: Index on multiple fields. Order matters based on query filters and sort pattern.

### Explain Plan

- Question: How check query performance?
- Safe answer: Use explain plan to see whether query uses index, scans many rows/docs, and where time is spent.

### Schema Design

- Question: Embed or reference in MongoDB?
- Safe answer: Embed when data is read together and bounded. Reference when data is large, shared, or changes independently.

### Transactions

- Question: When need transactions?
- Safe answer: When multiple writes must succeed/fail together, such as financial or consistency-critical operations.

### Replication and Sharding

- Question: Difference?
- Safe answer: Replication copies data for high availability. Sharding splits data across nodes for horizontal scale.

### Redis Caching

- Question: Why Redis?
- Safe answer: Fast in-memory store for cache, counters, sessions, rate limiting, locks, and queues.

### Cache Invalidation

- Question: Hardest part of caching?
- Safe answer: Keeping cache consistent. Common strategies: TTL, delete on write, write-through, cache-aside.

### Query Optimization

- Question: How optimize slow query?
- Safe answer: Check explain plan, add/adjust indexes, reduce selected fields, avoid N+1 queries, paginate, and review schema/access pattern.

## 7. Coding Recall Sheet In Go

Rules:
- No custom queue or stack structs.
- Use slices and maps.
- Keep code short and writable.

### Two Sum

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

- Explanation: Store value to index. For each number, check if complement exists.
- Time: O(n), Space: O(n)

### Valid Parentheses

```go
func isValid(s string) bool {
	stack := []rune{}
	pairs := map[rune]rune{')': '(', ']': '[', '}': '{'}

	for _, ch := range s {
		if ch == '(' || ch == '[' || ch == '{' {
			stack = append(stack, ch)
			continue
		}
		if len(stack) == 0 || stack[len(stack)-1] != pairs[ch] {
			return false
		}
		stack = stack[:len(stack)-1]
	}
	return len(stack) == 0
}
```

- Explanation: Push openings. For closing bracket, top must match.
- Time: O(n), Space: O(n)

### Min Stack Using Two Slices

```go
type MinStack struct {
	stack []int
	mins  []int
}

func (m *MinStack) Push(val int) {
	m.stack = append(m.stack, val)
	if len(m.mins) == 0 || val < m.mins[len(m.mins)-1] {
		m.mins = append(m.mins, val)
	} else {
		m.mins = append(m.mins, m.mins[len(m.mins)-1])
	}
}

func (m *MinStack) Pop() int {
	if len(m.stack) == 0 {
		return -1
	}
	val := m.stack[len(m.stack)-1]
	m.stack = m.stack[:len(m.stack)-1]
	m.mins = m.mins[:len(m.mins)-1]
	return val
}

func (m *MinStack) Top() int {
	if len(m.stack) == 0 {
		return -1
	}
	return m.stack[len(m.stack)-1]
}

func (m *MinStack) GetMin() int {
	if len(m.mins) == 0 {
		return -1
	}
	return m.mins[len(m.mins)-1]
}
```

- Explanation: Main stack stores values; min stack stores minimum at each level.
- Time: O(1) each operation, Space: O(n)

### BFS Using `[][]int`

```go
func bfs(graph [][]int, start int) []int {
	visited := make([]bool, len(graph))
	queue := []int{start}
	visited[start] = true
	order := []int{}

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

- Explanation: Queue gives level-by-level traversal. Mark visited when pushing.
- Time: O(V+E), Space: O(V)

### DFS Using `[][]int`

```go
func dfsTraversal(graph [][]int, start int) []int {
	visited := make([]bool, len(graph))
	order := []int{}

	var dfs func(int)
	dfs = func(node int) {
		if visited[node] {
			return
		}
		visited[node] = true
		order = append(order, node)

		for _, next := range graph[node] {
			dfs(next)
		}
	}

	dfs(start)
	return order
}
```

- Explanation: Recursion goes deep before backtracking. Visited prevents cycles.
- Time: O(V+E), Space: O(V)

### Course Schedule / Kahn's Algorithm

```go
func canFinish(numCourses int, prerequisites [][]int) bool {
	graph := make([][]int, numCourses)
	indegree := make([]int, numCourses)

	for _, pair := range prerequisites {
		course := pair[0]
		prereq := pair[1]
		graph[prereq] = append(graph[prereq], course)
		indegree[course]++
	}

	queue := []int{}
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

- Explanation: Zero-indegree courses have no pending prerequisites. Processing them unlocks dependent courses.
- Time: O(V+E), Space: O(V+E)

## 8. Optional Quick Kafka Revision

- Topic: Logical stream/category of messages.
- Partition: Split of topic for parallelism and ordering.
- Consumer group: Consumers sharing work; one partition assigned to one consumer in same group.
- Offset: Message position in partition.
- Commit: Prefer committing offset after successful processing.
- Retry: Retry failed processing with backoff.
- DLQ: Store failed messages after max retries so consumer is not blocked.
- Idempotency: Required because duplicates can happen with at-least-once processing.

Best line: Kafka is used for asynchronous event-driven communication. Producers write events to topics, topics are split into partitions, and consumers in a consumer group divide partitions for parallel processing.

## 9. AI-Proctored Interview Behavior Checklist

How to speak:
- Speak step-by-step.
- State assumptions.
- Use simple words.
- Think aloud before coding.

How to handle coding:
- Start with brute force if stuck.
- Then optimize.
- Use vanilla Go slices/maps.
- Run through one example manually.
- Mention time and space complexity.

How to handle unknown questions:
- Say what you know.
- Clarify assumptions.
- Give a safe practical answer.
- Do not fake deep expertise.

What not to do:
- Do not switch tabs unless allowed.
- Do not look away repeatedly.
- Do not use phone.
- Do not silently stare for long time.
- Do not over-explain unrelated topics.

10-minute final checklist:
- Water ready.
- Camera/mic ready.
- ID/system checks done.
- Close distractions.
- Keep notepad if allowed.
- Remember: problem -> approach -> code -> dry run -> complexity.

## 10. Final 30-Minute Revision Order

0-5 min:
- Self-introduction.
- Go vs Node positioning answer.

5-12 min:
- Go concurrency: goroutine, channel, WaitGroup, mutex, context, worker pool.

12-18 min:
- Coding recall: Two Sum, Min Stack, BFS/DFS, Course Schedule.

18-23 min:
- REST/microservices: idempotency, status codes, rate limiting, debugging API.

23-27 min:
- Databases: SQL vs NoSQL, MongoDB indexes, Redis caching, query optimization.

27-30 min:
- Kafka quick line.
- Unknown question handling.
- Calm down and breathe.
