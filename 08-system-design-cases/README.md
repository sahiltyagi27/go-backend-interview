# 08 - System Design Cases

## Topics From Both Checklists

### URL shortener

- Short code generation
- Redirect latency
- Cache
- DB schema

### rate limiter

- Redis counter/token bucket
- Per-user/per-IP limits
- Distributed rate limiting

### Instagram feed

- Fan-out-on-write
- Fan-out-on-read
- Celebrity user problem
- Redis sorted sets
- Cursor pagination

### chat/messaging

- Conversations
- Messages
- WebSocket
- Delivery status
- Offline push

### payment retry system

- Idempotency
- Retry states
- Reconciliation
- Webhooks
- Transaction consistency

### notification system

- Async events
- User preferences
- Retry/DLQ
- Push/email/SMS

### analytics pipeline

- API to Kafka to consumers to ClickHouse
- Batch vs streaming
- Deduplication
- Query performance

### e-commerce checkout

- Cart
- Inventory reservation
- Pricing/coupons
- Payment authorization
- Order state machine
- Saga/outbox

### video streaming

- Upload
- Object storage
- Transcoding
- CDN
- Adaptive bitrate
- Watch progress

---

## Existing Coverage

Covered in `/Users/sahiltyagi/Desktop/personal projects/system design`:

- URL shortener
- Instagram/social feed
- chat/messaging
- notification system
- video upload
- ride sharing

---

## Covered In This Folder

- `rate-limiter.md`
- `payment-retry-system.md`
- `analytics-pipeline.md`

These add:

- rate limiter dedicated case study
- payment retry system case study
- analytics pipeline case study
- Redis sorted sets coverage through the database/Redis folder

Also see:

- `../dsa-system-design-most-asked.md`
