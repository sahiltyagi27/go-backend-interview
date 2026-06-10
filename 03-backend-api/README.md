# 03 - Backend / API Topics

## Topics From Checklist

### REST API design

- HTTP methods
- Status codes
- Idempotency
- Pagination
- Versioning

### idempotency

- Why retries can duplicate requests
- Idempotency keys
- Unique constraints
- Safe retry design

### authentication

- JWT basics
- Access token expiry
- Middleware
- Where to store user ID
- Token validation

### rate limiting

- Fixed window
- Sliding window
- Token bucket
- Leaky bucket
- Redis-based rate limiter

### pagination

- Offset pagination
- Cursor pagination
- Why cursor is better for feeds/messages

### file upload design

- Direct upload to S3/GCS
- Presigned URLs
- Why API server should not proxy large files

---

## Existing Coverage

Partial coverage exists in:

- `/Users/sahiltyagi/Desktop/personal projects/system design/concepts/api-data-modeling.md`
- `/Users/sahiltyagi/Desktop/personal projects/system design/examples/video-upload.md`
- `/Users/sahiltyagi/Desktop/personal projects/system design/examples/url-shortener.md`

---

## Covered In This Folder

- `rest-jwt-pagination.md`
- `idempotency-rate-limit-upload.md`
- `../backend-interview-reality-check.md`

These cover:

- REST methods/status codes/versioning
- middleware pattern
- JWT validation flow
- idempotency key storage
- rate limiter algorithms
- cursor pagination
- presigned upload design notes
- secure API interview answers
- JWT logout tradeoffs
- API Gateway responsibilities
