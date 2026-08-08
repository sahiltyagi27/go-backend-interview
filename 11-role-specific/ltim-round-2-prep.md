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

### Q. What is a goroutine leak?

Answer:

```text
A goroutine leak happens when a goroutine keeps running or stays blocked even though it is no longer needed.

Common causes are waiting forever on a channel, sending to a channel with no receiver, missing context cancellation, or not closing a jobs channel.

To avoid it, I use context cancellation, timeouts, clear channel ownership, closing channels from the producer side, and WaitGroup to ensure goroutines exit.
```

### Q. What is a deadlock in Go?

Answer:

```text
A deadlock happens when goroutines are waiting on each other and no one can make progress.

For example, sending on an unbuffered channel without a receiver can block forever. Another example is locking a mutex and never unlocking it.

To avoid deadlocks, I keep channel flow clear, use defer unlock for mutexes, use context/timeouts where needed, and avoid circular waiting.
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

### Q. What is go.mod?

Answer:

```text
go.mod is the module definition file for a Go project.

It defines the module name, Go version, and direct dependencies required by the project.

It is similar in purpose to package.json in Node.js, but for Go modules.
```

### Q. What is go.sum?

Answer:

```text
go.sum stores cryptographic checksums for module dependencies.

It helps Go verify that downloaded dependencies are exactly the expected versions and have not been tampered with.

go.mod defines what the project needs. go.sum helps verify dependency integrity.
```

Interview line:

```text
go.mod defines the Go module and dependencies. go.sum records checksums to verify dependency integrity and reproducible builds.
```

### Q. How does garbage collection work in Go?

Answer:

```text
Go has automatic garbage collection. The Go runtime tracks heap allocations and frees memory that is no longer reachable.

Go's garbage collector is designed for low-latency services. It runs concurrently with the program as much as possible, so it reduces long stop-the-world pauses.

As a backend engineer, I still try to avoid unnecessary allocations in hot paths because too many allocations increase GC pressure and can affect latency.
```

Interview line:

```text
Go has automatic low-latency garbage collection, but we still care about allocation patterns because high allocation rate increases GC work.
```

## 4. Node.js Questions And Answers

### Q. What is Node.js event loop?

Answer:

```text
The event loop is what allows Node.js to handle many asynchronous operations without blocking the main JavaScript thread.

JavaScript execution is single-threaded, but async I/O is handled through the event loop and libuv, so Node can serve many I/O-heavy requests efficiently.
```

### Q. How does Node.js handle async operations?

Answer:

```text
Node.js starts async operations like file I/O, network calls, timers, or DB calls and does not block the main JavaScript thread while waiting.

The event loop continues running other work. When the async operation completes, its callback, promise resolution, or async-await continuation is scheduled back on the event loop.

Some operations are handled by the OS, and some use libuv's thread pool, such as file system, DNS, and crypto.
```

Interview line:

```text
Node.js is single-threaded for JavaScript execution, but async I/O is handled through the event loop and libuv.
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

Timeline:

```text
Promises were standardized in ES6 / ES2015.
async/await was standardized in ES2017.
```

### Q. Give an example of Promise with then/catch.

Example:

```js
fetchUser(1)
  .then((user) => {
    console.log("User:", user);
  })
  .catch((err) => {
    console.error("Error:", err);
  });
```

Say:

```text
then handles the resolved value, and catch handles rejection/error.
```

### Q. Give an example of Promise resolve/reject.

Example:

```js
function fetchUser(id) {
  return new Promise((resolve, reject) => {
    if (!id) {
      reject(new Error("id is required"));
      return;
    }

    resolve({
      id,
      name: "Sahil",
    });
  });
}
```

Say:

```text
resolve completes the Promise successfully. reject marks it as failed.
```

### Q. Give an example of async/await.

Example:

```js
async function printUser(id) {
  try {
    const user = await fetchUser(id);
    console.log("User:", user);
  } catch (err) {
    console.error("Error:", err);
  }
}
```

Say:

```text
async makes the function return a Promise. await pauses only this async function until the Promise resolves or rejects. It does not block the Node.js event loop.
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

For example, instead of reading a 1GB file fully into memory, we can create a readable stream and pipe it to a writable stream.
```

Interview line:

```text
Streams are for memory-efficient I/O, not CPU parallelism.
```

### Q. What is backpressure in streams?

Answer:

```text
Backpressure happens when the producer is faster than the consumer.

For example, a readable stream may produce data faster than the writable stream can write it. Node streams handle this by slowing down the producer so memory does not keep growing.

In practice, using pipeline is safer because it handles stream errors and cleanup better than manual pipe chains.
```

Small Node.js example:

```js
const fs = require("fs");

const readable = fs.createReadStream("input.txt");
const writable = fs.createWriteStream("output.txt");

readable.pipe(writable);
```

Say:

```text
Here one stream reads chunks from input.txt and another stream writes chunks to output.txt. pipe connects the readable stream to the writable stream.
```

Safer pipeline example:

```js
const fs = require("fs");
const { pipeline } = require("stream/promises");

async function copyFile(inputPath, outputPath) {
  await pipeline(
    fs.createReadStream(inputPath),
    fs.createWriteStream(outputPath)
  );
}
```

Say:

```text
pipeline connects streams and handles errors/cleanup better than manual pipe usage.
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

### Q. What is package.json?

Answer:

```text
package.json is the metadata file for a Node.js project.

It contains project information, dependencies, devDependencies, scripts, entry point, and sometimes configuration for tools.
```

### Q. What is package-lock.json?

Answer:

```text
package-lock.json locks the exact dependency versions installed in a Node.js project.

package.json may allow version ranges, but package-lock.json records the exact resolved versions, including nested dependencies.

This helps make installs more reproducible across machines and environments.
```

Interview line:

```text
package.json describes dependencies and scripts. package-lock.json locks the exact resolved dependency tree for reproducible installs.
```

### Q. What are npm scripts?

Answer:

```text
npm scripts are commands defined in package.json.

For example, scripts can run the dev server, build TypeScript, start production server, run tests, lint code, or format code.
```

Example:

```json
{
  "scripts": {
    "dev": "tsx watch src/server.ts",
    "build": "tsc",
    "start": "node dist/server.js"
  }
}
```

### Q. How does TypeScript help in Express projects?

Answer:

```text
TypeScript adds static typing, which helps catch mistakes earlier.

In Express projects, it helps type request bodies, route params, response shapes, config values, and service inputs/outputs.

It improves maintainability, especially as the backend grows.
```

### Q. What is the difference between var and let?

Answer:

```text
var is function-scoped, while let is block-scoped.

var declarations are hoisted and initialized with undefined, so they can be accessed before declaration but the value will be undefined.

let declarations are also hoisted, but they stay in the temporal dead zone until the declaration line is executed, so accessing them before declaration gives a ReferenceError.

In modern JavaScript, I prefer let for variables that change and const for variables that should not be reassigned. I avoid var in new code.
```

Example:

```js
function testVar() {
  if (true) {
    var x = 10;
  }
  console.log(x); // 10, because var is function-scoped
}

function testLet() {
  if (true) {
    let y = 20;
  }
  console.log(y); // ReferenceError, because let is block-scoped
}
```

Interview line:

```text
var is function-scoped and can cause bugs due to hoisting. let is block-scoped and safer for modern JavaScript.
```

### Q. How does garbage collection work in Node.js?

Answer:

```text
Node.js runs JavaScript on the V8 engine, and V8 provides automatic garbage collection.

V8 manages memory for JavaScript objects on the heap and frees objects that are no longer reachable.

In Node.js backend services, memory leaks can still happen if we accidentally keep references alive, for example global arrays, caches without limits, event listeners not removed, or closures holding large objects.
```

Interview line:

```text
Node.js has automatic garbage collection through V8, but we still need to avoid memory leaks by not keeping unnecessary references alive.
```

### Q. How is Go garbage collection different from Node.js garbage collection?

Answer:

```text
Both Go and Node.js have automatic garbage collection, but they are managed by different runtimes.

Go GC is part of the Go runtime and is designed for backend services with many goroutines and low-latency goals.

Node.js GC is handled by V8, the JavaScript engine. It manages JavaScript heap memory and is optimized for JavaScript workloads.

In practice, Go gives more predictable control for backend services where performance and concurrency are important. Node.js GC is convenient, but memory leaks can happen if JavaScript code keeps references alive, such as unbounded caches or event listeners.
```

Practical difference:

```text
Go concern: avoid high allocation rate in hot paths to reduce GC pressure.
Node.js concern: avoid holding references forever and avoid unbounded in-memory objects/caches.
```

### Q. Why Go over Node.js?

Answer:

```text
I would choose Go when I need strong concurrency, CPU efficiency, simple deployment, and high-performance backend services.

Go has goroutines, channels, a compiled binary, good performance, and a simple standard library. It is a strong fit for infrastructure services, workers, APIs, and concurrent processing.
```

Examples:

```text
I would choose Go for a high-throughput worker service that consumes Kafka messages and processes them concurrently.

I would choose Go for infrastructure/backend services where I want simple deployment as a single binary, good CPU efficiency, and predictable performance.

I would choose Go for services that need many concurrent tasks, like batch processors, scrapers, queue consumers, or internal platform services.
```

### Q. Why Node.js over Go?

Answer:

```text
I would choose Node.js when the system is I/O-heavy, needs quick API development, uses a JavaScript/TypeScript full-stack team, or benefits from the npm ecosystem.

Node.js is good for REST APIs, BFF services, real-time apps, and services where most time is spent waiting on DB, network, or external APIs.
```

Examples:

```text
I would choose Node.js for a REST API or BFF layer where most work is I/O: calling DB, calling external APIs, validating requests, and returning responses.

I would choose Node.js when the team already uses TypeScript/React and wants faster full-stack development with shared language and tooling.

I would choose Node.js for real-time or event-driven API layers, such as WebSocket services or lightweight integration services, as long as the workload is not CPU-heavy.
```

Balanced answer:

```text
For CPU-heavy or highly concurrent backend workers, I lean toward Go. For I/O-heavy APIs, fast product development, and TypeScript ecosystem benefits, Node.js can be a good choice.
```

Important nuance:

```text
Node.js is not chosen because Go cannot do the job. Go can do almost all backend work Node can do, and Go is generally stronger for raw performance, CPU efficiency, memory usage, and concurrency-heavy workloads.

Node.js is often chosen when team productivity, TypeScript/full-stack alignment, existing company ecosystem, and integration speed matter more than raw runtime performance.
```

Practical examples:

```text
If the frontend team already uses React/TypeScript, Node.js can make onboarding and shared types easier.

If a company already has many Node.js services, adding another Node service may be simpler operationally.

For BFF layers, API aggregation, request shaping, GraphQL gateways, and UI-facing APIs, Node/TypeScript can be very productive.

For SaaS integrations, web tooling, auth, payments, email, and frontend-adjacent packages, Node's npm ecosystem can be convenient.
```

Senior interview line:

```text
Go is generally stronger for performance, CPU efficiency, concurrency-heavy workers, and infrastructure services. Node.js is not chosen because Go cannot do the job; Node.js is often chosen when team productivity, TypeScript/full-stack alignment, existing ecosystem, and integration speed matter more than raw runtime performance.
```

### Q. Compare Node.js concurrency with Go concurrency.

Answer:

```text
Node.js runs JavaScript on one main thread. Concurrency mainly comes from the event loop and async I/O delegation through libuv, the OS, and libuv's thread pool for some operations.

Go uses goroutines. Goroutines are lightweight units managed by the Go runtime. The Go scheduler maps many goroutines onto OS threads.

So Node.js is mostly single-threaded JavaScript plus event-loop-based I/O concurrency. Go supports concurrency through goroutines and can also run work in parallel across multiple OS threads/CPU cores depending on GOMAXPROCS.
```

Node.js:

```text
1 main JavaScript thread
event loop
libuv
libuv thread pool for some operations like fs, DNS, crypto
OS async I/O for networking
```

Go:

```text
goroutines
Go runtime scheduler
OS threads
GOMAXPROCS
network poller
```

Interview line:

```text
Node.js is great for I/O concurrency because the event loop delegates async work and keeps the main thread free. Go is great for concurrency and parallelism because goroutines are scheduled by the Go runtime over OS threads, so it handles both I/O-heavy and CPU-heavy concurrent workloads better.
```

Example:

```text
10,000 HTTP requests waiting on DB:
- Node.js can handle this well because most time is waiting on I/O.
- Go can also handle this well with goroutines.

CPU-heavy image processing:
- Node.js main thread gets blocked unless worker_threads, child processes, or separate services are used.
- Go can run CPU-heavy goroutines in parallel across cores.
```

### Q. What happens if Node receives a CPU-heavy task?

Answer:

```text
CPU-heavy work can block the event loop because JavaScript execution runs on the main thread.

If the event loop is blocked, other requests cannot be handled smoothly. For CPU-heavy work, I would use worker_threads, child processes, a job queue, or move the work to a separate service.
```

### Q. Can Node.js achieve similar CPU parallelism using worker_threads?

Answer:

```text
Node.js can improve CPU-heavy performance using worker_threads. Worker threads can run CPU-heavy work in parallel on separate threads, and we can also build a worker pool in Node.

This prevents CPU-heavy work from blocking the main event loop.

But it is not exactly the same as Go goroutines. Worker threads are heavier than goroutines, communication between the main thread and workers has overhead, and data may need to be copied or transferred.

Go was designed for this style of concurrency from the beginning. Goroutines are lightweight and managed naturally by the Go runtime scheduler.
```

Interview line:

```text
Node can parallelize CPU-heavy work with worker_threads, but Go generally handles this more naturally and efficiently with goroutines and the runtime scheduler.
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

### Q. What are 1NF, 2NF, and 3NF?

Answer:

```text
1NF means each column should contain atomic values, not repeating groups or arrays in a single column.

2NF means the table should already be in 1NF, and every non-key column should depend on the whole primary key, not only part of a composite key.

3NF means the table should already be in 2NF, and non-key columns should not depend on other non-key columns. They should depend only on the key.
```

Simple example:

```text
1NF:
Do not store multiple phone numbers in one column like "999,888".
Store them as separate rows.

2NF:
If a table has composite key (student_id, course_id), student_name depends only on student_id, not the full key, so move student details to students table.

3NF:
If employee table has department_id and department_name, department_name depends on department_id, not employee_id, so move department details to departments table.
```

Interview line:

```text
1NF removes repeating groups, 2NF removes partial dependency, and 3NF removes transitive dependency.
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

### SQL Query Practice

Use these tables for examples:

```text
users
- id
- name
- email
- status
- created_at

orders
- id
- user_id
- amount
- status
- created_at

companies
- id
- name

job_applications
- id
- company_id
- role
- status
- applied_date
```

#### Basic SELECT + WHERE

```sql
SELECT name, email
FROM users
WHERE status = 'active';
```

Say:

```text
This selects only active users and returns their name and email.
```

#### INNER JOIN

```sql
SELECT
    u.name,
    o.id AS order_id,
    o.amount
FROM users u
INNER JOIN orders o
    ON u.id = o.user_id;
```

Say:

```text
INNER JOIN returns only users who have matching orders.
```

#### LEFT JOIN

```sql
SELECT
    u.name,
    o.id AS order_id,
    o.amount
FROM users u
LEFT JOIN orders o
    ON u.id = o.user_id;
```

Say:

```text
LEFT JOIN returns all users, even if some users do not have orders. For users without orders, order columns will be NULL.
```

#### GROUP BY + COUNT

```sql
SELECT
    status,
    COUNT(*) AS total_orders
FROM orders
GROUP BY status;
```

Say:

```text
This groups orders by status and counts how many orders exist in each status.
```

#### GROUP BY + SUM

```sql
SELECT
    user_id,
    SUM(amount) AS total_amount
FROM orders
GROUP BY user_id;
```

Say:

```text
This calculates total order amount per user.
```

#### HAVING

```sql
SELECT
    user_id,
    COUNT(*) AS total_orders
FROM orders
GROUP BY user_id
HAVING COUNT(*) > 5;
```

Say:

```text
WHERE filters rows before grouping. HAVING filters groups after aggregation.
```

#### CREATE VIEW

```sql
CREATE VIEW application_summary AS
SELECT
    c.name AS company_name,
    j.role,
    j.status,
    j.applied_date
FROM companies c
INNER JOIN job_applications j
    ON c.id = j.company_id;
```

Say:

```text
This creates a reusable view that joins companies with job applications, so we do not have to rewrite the join every time.
```

#### Query A View

```sql
SELECT *
FROM application_summary
WHERE status = 'interview';
```

Say:

```text
After creating a view, I can query it like a table, but logically it represents the saved SQL query.
```

#### Transaction Example

```sql
BEGIN;

UPDATE accounts
SET balance = balance - 100
WHERE id = 1;

UPDATE accounts
SET balance = balance + 100
WHERE id = 2;

COMMIT;
```

Say:

```text
This should be a transaction because both debit and credit must succeed together. If one operation fails, we should rollback.
```

#### Index Example

```sql
CREATE INDEX idx_orders_user_id
ON orders(user_id);
```

Say:

```text
This index helps queries that filter or join on user_id, but it adds write and storage overhead.
```

#### Compound Index Example

```sql
CREATE INDEX idx_orders_user_status
ON orders(user_id, status);
```

Say:

```text
This helps queries that filter by user_id and status together. Order matters in compound indexes.
```

### DB Fundamentals One-Liners

```text
Primary key = uniquely identifies a row.
Foreign key = links one table to another table.
JOIN = combines related rows from tables.
Index = faster reads, slower writes, more storage.
Transaction = all or nothing group of operations.
ACID = Atomicity, Consistency, Isolation, Durability.
Normalization = reduce duplication and improve consistency.
Denormalization = duplicate some data for faster reads.
Replication = copy data across nodes for availability and failover.
Sharding = split data across nodes for horizontal scale.
Connection pool = reuse DB connections instead of opening a new one for every request.
```

### Q. What is a primary key and foreign key?

Answer:

```text
A primary key uniquely identifies a row in a table.

A foreign key creates a relationship between two tables. For example, orders.user_id can refer to users.id.
```

### Q. What is a connection pool?

Answer:

```text
A connection pool keeps a reusable set of database connections.

Opening a new DB connection for every request is expensive, so backend services reuse connections from the pool. The pool size should be configured carefully because too many connections can overload the database.
```

### Q. What is sharding?

Answer:

```text
Sharding means splitting data across multiple database nodes.

It helps with horizontal scaling when one database node cannot handle the data size or traffic. The hard part is choosing the shard key and handling cross-shard queries.
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

### Q. Explain Kafka producer-consumer flow.

Answer:

```text
A producer publishes messages to a Kafka topic. A topic is split into partitions for scalability and ordering.

Consumers read messages from partitions. Consumers in the same consumer group share partitions, so each message is processed by one consumer in that group.

For reliable processing, I would commit the offset only after successful processing. If processing fails, I retry with backoff. After max retries, I move the message to a DLQ so it does not block the main pipeline.
```

Pseudo-flow:

```text
Producer:
1. Create producer.
2. Send message to topic with key.
3. Kafka writes message to a partition.
4. Producer waits for acknowledgement if reliability is needed.

Consumer:
1. Subscribe to topic.
2. Poll/read message.
3. Process message.
4. If success, commit offset.
5. If failure, retry.
6. If still failing, send to DLQ and commit original offset.
```

Go Kafka interview line:

```text
In Go, I would use a Kafka client library like Sarama or confluent-kafka-go. The consumer would subscribe to a topic, read messages, process each message, and commit the offset only after successful processing. I would add retries, logging, idempotency, and DLQ handling for failed messages.
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

### Q. What are common MongoDB index types?

Answer:

```text
Common MongoDB index types are single field, compound, multikey, text, TTL, unique, and geospatial indexes.
```

Examples:

```js
// Single field index
db.users.createIndex({ email: 1 })

// Compound index
db.orders.createIndex({ userId: 1, status: 1 })

// Multikey index for array field
db.products.createIndex({ tags: 1 })

// Text index for search
db.articles.createIndex({ title: "text", body: "text" })

// TTL index for auto-expiry
db.sessions.createIndex({ createdAt: 1 }, { expireAfterSeconds: 3600 })

// Unique index
db.users.createIndex({ email: 1 }, { unique: true })

// Geospatial index
db.places.createIndex({ location: "2dsphere" })
```

Interview explanation:

```text
Single field index is for queries on one field.
Compound index is for queries using multiple fields; field order matters.
Multikey index is for array fields.
Text index is for text search.
TTL index automatically deletes documents after a time.
Unique index enforces uniqueness.
Geospatial index supports location-based queries.
```

Tradeoff:

```text
Indexes improve reads but add storage and write overhead, so I create them based on actual query patterns, filters, sorting, and explain plan.
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

### Q. Fan-in / fan-out approach?

Answer:

```text
Fan-out means distributing work from one input channel across multiple workers.

Fan-in means collecting results from multiple workers back into one output channel.

It is useful for parallel processing while controlling concurrency.
```

Interview line:

```text
Fan-out distributes work across multiple goroutines, and fan-in collects results back into a single channel.
```

Simple Go pattern:

```go
func worker(id int, jobs <-chan int, results chan<- int, wg *sync.WaitGroup) {
	defer wg.Done()

	for job := range jobs {
		results <- job * 2
	}
}

func runWorkers() {
	jobs := make(chan int)
	results := make(chan int)
	var wg sync.WaitGroup

	for i := 1; i <= 3; i++ {
		wg.Add(1)
		go worker(i, jobs, results, &wg)
	}

	go func() {
		for i := 1; i <= 5; i++ {
			jobs <- i
		}
		close(jobs)
	}()

	go func() {
		wg.Wait()
		close(results)
	}()

	for result := range results {
		fmt.Println(result)
	}
}
```

Key explanation:

```text
jobs channel sends work.
Multiple workers consume from the same jobs channel.
Each worker sends processed result to results channel.
WaitGroup waits for workers.
After workers finish, results channel is closed.
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
