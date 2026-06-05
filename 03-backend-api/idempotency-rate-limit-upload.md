# Idempotency, Rate Limiting, and File Upload

## Idempotency

Retries can duplicate requests.

Example:

```text
Client sends payment request
Server charges card
Network times out before response
Client retries
Without idempotency, user may be charged twice
```

Use an idempotency key:

```http
POST /payments
Idempotency-Key: pay_abc123
```

Store:

```text
idempotency_key
request_hash
status
response_body
created_at
```

Flow:

1. Client sends key.
2. Server inserts key with unique constraint.
3. If key already exists, return stored response.
4. If original is in progress, return 409 or wait.

## Rate Limiting

Rate limiting protects the system from abuse and overload.

### Fixed Window

Allow N requests per minute.

Problem:

> A user can send N requests at the end of one window and N at the start of the next.

### Sliding Window

Tracks a moving time window. More accurate, more storage.

### Token Bucket

Bucket refills at a steady rate. Each request consumes a token.

Good for allowing short bursts while enforcing long-term rate.

### Leaky Bucket

Processes requests at a steady rate. Smooths bursts.

### Redis Token Bucket

Store:

```text
tokens:{user_id}
last_refill:{user_id}
```

Use Lua script for atomic update:

```text
refill tokens
if tokens > 0:
  decrement
  allow
else:
  reject
```

Distributed rate limiting needs a central store like Redis because multiple API servers must share counters.

## File Upload Design

Do not proxy large files through API servers.

Better flow:

```text
Client asks API for presigned URL
API validates user and returns URL
Client uploads directly to S3/GCS
Storage emits event
Worker processes file
```

Benefits:

- API servers stay lightweight
- object storage handles large upload traffic
- retries/resume are easier

