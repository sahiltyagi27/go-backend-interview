# Clean Code, SOLID, and Design Patterns

## Clean Code

Clean code is code that is easy to understand, change, test, and debug.

Good clean-code habits:

- small functions
- clear names
- simple control flow
- explicit errors
- low coupling
- high cohesion
- no hidden side effects
- comments only where they explain why, not what

Interview line:

> Clean code reduces the cost of future changes. Senior engineers optimize for maintainability, not just making code work today.

## SOLID

SOLID is a set of object-oriented design principles. In Go, apply the spirit, not the ceremony.

### S - Single Responsibility

A type/function should have one reason to change.

Bad:

```go
type UserService struct {
    // validates users, writes DB, sends email, logs analytics
}
```

Better:

```go
type UserService struct {
    store UserStore
    email EmailSender
}
```

### O - Open/Closed

Code should be open for extension but closed for modification.

In Go, interfaces help.

```go
type PaymentProvider interface {
    Charge(amount int) error
}
```

Add Stripe/Razorpay/PayPal implementations without rewriting business logic.

### L - Liskov Substitution

If a type implements an interface, callers should be able to use it without surprising behavior.

### I - Interface Segregation

Prefer small interfaces.

Good:

```go
type Reader interface {
    Read([]byte) (int, error)
}
```

Avoid large interfaces that force types to implement methods they do not need.

### D - Dependency Inversion

High-level business logic should depend on abstractions, not concrete low-level details.

```go
type OrderService struct {
    payments PaymentProvider
}
```

## Design Patterns Useful For Backend

### Repository

Separates persistence from business logic.

```go
type UserRepository interface {
    FindByID(ctx context.Context, id string) (User, error)
}
```

### Strategy

Swap algorithms/behaviors.

Examples:

- different payment providers
- different ranking strategies
- different rate-limit algorithms

### Factory

Creates concrete types based on config.

### Decorator / Middleware

Wrap behavior.

Examples:

- logging middleware
- auth middleware
- retry wrapper
- metrics wrapper

### Adapter

Convert third-party API shape into your internal interface.

Interview line:

> Patterns are vocabulary for common tradeoffs. Use them when they simplify code, not to make code look fancy.

