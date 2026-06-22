# Distributed Tracing Interview Notes

Distributed tracing helps you follow one request as it moves across multiple services.

Use it when a request goes through:

```text
Client -> API Gateway -> Service A -> Service B -> Database
                         |
                         +-> Kafka -> Worker -> External API
```

Without tracing, each service has separate logs and it is hard to know where the request failed or became slow.

With tracing, all work for the same request is connected under one trace.

## Core Terms

### Trace

A trace is the full journey of one request across services.

Example:

```text
Trace = everything that happened for request abc123
```

### Trace ID

The trace ID is the shared identifier for the full request path.

All services involved in the same request should use the same trace ID.

### Span

A span is one operation inside the trace.

Examples:

```text
HTTP handler span
DB query span
Redis call span
Kafka publish span
Kafka consume span
External API call span
```

### Span ID

Each span has its own span ID.

### Parent-Child Span

Spans are connected like a tree.

```text
Trace ID: t-100

API request span
  -> user-service HTTP span
      -> Redis cache span
      -> DB query span
  -> Kafka publish span
      -> worker consume span
          -> external API span
```

This parent-child structure tells you which operation caused another operation.

## Context Propagation

Context propagation means passing trace information from one service to the next.

For HTTP or gRPC, pass trace context through request headers.

Common header standard:

```text
traceparent
```

This comes from the W3C Trace Context standard.

For Kafka or queues, pass trace context in message headers.

Example:

```text
HTTP headers:
traceparent: 00-<trace-id>-<span-id>-01

Kafka headers:
traceparent=<trace context>
```

## OpenTelemetry

OpenTelemetry is the common open-source standard for collecting traces, metrics, and logs.

Typical setup:

```text
Application code
  -> OpenTelemetry SDK
  -> OpenTelemetry Collector
  -> Jaeger / Tempo / Datadog / New Relic
```

The application creates spans.

The collector receives telemetry data.

The tracing backend stores and visualizes traces.

## What To Instrument

Create spans for important operations:

```text
incoming HTTP request
outgoing HTTP/gRPC call
database query
Redis command
Kafka publish
Kafka consume
external API call
background job
```

Add useful span attributes:

```text
service.name
http.method
http.route
http.status_code
db.system
db.operation
messaging.system
messaging.destination
error
```

Avoid adding secrets, tokens, passwords, or full payment details.

## How It Helps During Incidents

Tracing answers:

```text
Which service failed?
Which downstream dependency was slow?
Which span has the error?
How much time was spent in DB vs Redis vs external API?
Did the Kafka worker process the message?
Was the trace broken because context was not propagated?
```

Example:

```text
API latency = 2.4 seconds

Trace timeline:
API handler        2400 ms
user-service call  300 ms
payment-service   1900 ms
DB query           100 ms
external gateway  1700 ms
```

This shows the external payment gateway is the likely bottleneck.

## Interview Answer

Use this when asked:

> How would you implement spans across multiple microservices to understand where a request failed?

Answer:

> I would instrument each service with OpenTelemetry. When a request enters the system, we either create a new trace or continue the incoming trace from headers. Each service creates spans for important operations like HTTP handlers, DB queries, Redis calls, Kafka publish/consume, and external API calls. The trace context is propagated through HTTP or gRPC headers, and for async flows through Kafka message headers. This creates parent-child spans under the same trace ID. We export the spans through an OpenTelemetry Collector to Jaeger, Tempo, Datadog, or New Relic. During debugging, I can inspect the trace timeline to see which service failed, which span has an error, and where latency increased.

## Short Version

```text
Trace = full request journey
Span = one operation in that journey
Trace ID = common ID across services
Span ID = ID of one operation
Context propagation = passing trace info through headers
OpenTelemetry = standard instrumentation framework
Jaeger/Tempo/Datadog/New Relic = tracing backends
```

## Common Mistakes

```text
Only saying "use logs" and not mentioning trace/span.
Not propagating trace context across service boundaries.
Forgetting async propagation through Kafka headers.
Creating spans but not exporting them anywhere.
Logging trace IDs but not connecting parent-child spans.
Adding sensitive data as span attributes.
```

