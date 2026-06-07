# DDD and Event-Driven Design

## Domain-Driven Design

DDD means modeling software around the business domain.

Important terms:

| Term | Meaning |
|---|---|
| Domain | business area |
| Entity | object with identity |
| Value Object | object defined by value |
| Aggregate | consistency boundary |
| Repository | persistence abstraction |
| Bounded Context | area with its own model/language |

## Entity

Has identity.

```go
type Order struct {
    ID     string
    Status string
}
```

Even if fields change, the order is still the same order because ID is same.

## Value Object

Defined by values.

```go
type Money struct {
    Amount   int64
    Currency string
}
```

## Aggregate

An aggregate protects business invariants.

Example:

```text
Order
  - OrderItems
  - PaymentStatus
```

Only update order items through the `Order` aggregate so rules stay valid.

## Bounded Context

Same word can mean different things in different contexts.

Example:

- "User" in identity service means login/account.
- "Customer" in billing means payer/subscription.
- "Author" in content service means creator profile.

Senior design move:

> Do not force one global model across the entire company.

## Event-Driven Design

Event-driven systems communicate through events.

Example:

```text
OrderPaid
  |
  ├── Email Service sends receipt
  ├── Analytics Service records revenue
  └── Shipping Service starts fulfillment
```

Benefits:

- decoupling
- async processing
- easier fan-out
- better resilience

Costs:

- eventual consistency
- duplicate events
- harder debugging
- schema evolution

## Event Naming

Events should describe facts that already happened.

Good:

```text
OrderCreated
PaymentSucceeded
UserRegistered
```

Avoid command-like names:

```text
SendEmail
CreateInvoice
```

## Interview Line

> DDD helps define ownership and consistency boundaries. Event-driven design helps services react asynchronously, but it requires idempotency, retries, observability, and careful event schema design.

