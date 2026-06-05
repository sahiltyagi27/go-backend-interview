# Design a Payment Retry System

## Requirements

- process payments safely
- retry temporary failures
- avoid duplicate charges
- handle provider webhooks
- reconcile final status

## Core Rule

> Payment operations must be idempotent.

## Data Model

```text
Payment(id, order_id, amount, status, idempotency_key, provider_ref, created_at)
PaymentAttempt(id, payment_id, status, error_code, created_at)
WebhookEvent(id, provider_event_id, payload, processed_at)
```

Unique constraints:

```text
idempotency_key unique
provider_event_id unique
```

## States

```text
CREATED
  |
PROCESSING
  |
SUCCEEDED
  |
FAILED_RETRYABLE
  |
FAILED_FINAL
```

## Flow

```text
Client creates payment with idempotency key
  |
Server creates Payment row
  |
Call payment provider
  |
If timeout/temporary error, schedule retry
  |
Provider webhook confirms final status
  |
Reconciliation job fixes mismatches
```

## Retry Strategy

Retry:

- network timeout
- 5xx from provider
- provider says pending

Do not retry:

- invalid card
- insufficient funds
- fraud blocked

Use exponential backoff.

## Webhooks

Provider sends final events asynchronously.

Webhook handler must:

- verify signature
- deduplicate event ID
- update payment state idempotently

## Reconciliation

Periodic job:

```text
find payments stuck in PROCESSING
ask provider for latest status
update local DB
```

## Transaction Consistency

Use DB transaction for local state changes.

For publishing events after payment state changes, use outbox pattern.

