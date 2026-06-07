# Testing Strategy

Senior engineers think about confidence, risk, and feedback speed.

## Test Pyramid

```text
        E2E tests
      Integration tests
    Unit tests
```

Unit tests:

- fast
- isolated
- test business logic

Integration tests:

- test DB, Redis, Kafka, external boundaries
- slower but higher confidence

E2E tests:

- test full user flows
- slowest and most brittle

## What To Test

Prioritize:

- business rules
- edge cases
- error handling
- idempotency
- retries
- authorization checks
- database transaction behavior

## Go Unit Test Example

```go
func TestCalculateTotal(t *testing.T) {
    got := CalculateTotal([]Item{{Price: 100}, {Price: 50}})
    want := 150

    if got != want {
        t.Fatalf("got %d, want %d", got, want)
    }
}
```

## Table-Driven Tests

Common Go style:

```go
func TestIsValidStatus(t *testing.T) {
    tests := []struct {
        name string
        in   string
        want bool
    }{
        {"paid", "PAID", true},
        {"bad", "UNKNOWN", false},
    }

    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            if got := IsValidStatus(tt.in); got != tt.want {
                t.Fatalf("got %v, want %v", got, tt.want)
            }
        })
    }
}
```

## Test Doubles

Use interfaces to replace dependencies.

```go
type fakeStore struct {
    user User
}

func (f fakeStore) FindUser(ctx context.Context, id string) (User, error) {
    return f.user, nil
}
```

## Integration Tests

Use integration tests for:

- SQL queries
- migrations
- Redis behavior
- Kafka consumers
- HTTP handlers with real routing

## Interview Line

> I prefer fast unit tests for business rules, integration tests for database and messaging boundaries, and a small number of E2E tests for critical user flows.

