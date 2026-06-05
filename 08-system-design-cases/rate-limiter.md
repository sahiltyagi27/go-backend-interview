# Design a Rate Limiter

## Requirements

- limit requests per user/IP/API key
- work across multiple API servers
- low latency
- configurable limits
- return `429 Too Many Requests`

## API Flow

```text
Request
  |
API Gateway / Middleware
  |
Rate Limiter
  |
Allow or Reject
```

## Token Bucket

Each user has a bucket.

```text
capacity = 100 tokens
refill = 10 tokens/sec
each request costs 1 token
```

If token exists, allow. Otherwise reject.

## Redis Design

Keys:

```text
rate:{user_id}:tokens
rate:{user_id}:last_refill
```

Use Lua script so refill and decrement happen atomically.

## Distributed Rate Limiting

Why Redis?

> Multiple API servers need shared rate-limit state.

Risks:

- Redis latency
- Redis outage
- hot users/IPs

Fallback options:

- fail open for low-risk APIs
- fail closed for sensitive APIs
- local emergency limiter

## Response

```http
HTTP/1.1 429 Too Many Requests
Retry-After: 10
```

