# Error Handling

Go treats errors as values.

## Basic Error

```go
return errors.New("user not found")
```

## Add Context

```go
return fmt.Errorf("load user %s: %w", id, err)
```

`%w` wraps the original error so callers can inspect it.

## errors.Is

Use `errors.Is` to check a known sentinel error.

```go
var ErrNotFound = errors.New("not found")

if errors.Is(err, ErrNotFound) {
    return http.StatusNotFound
}
```

## errors.As

Use `errors.As` to extract a custom error type.

```go
type ValidationError struct {
    Field string
    Msg   string
}

func (e ValidationError) Error() string {
    return e.Field + ": " + e.Msg
}

var validationErr ValidationError
if errors.As(err, &validationErr) {
    fmt.Println(validationErr.Field)
}
```

## Custom Error Type

Use custom error types when callers need structured details.

```go
type APIError struct {
    Code int
    Msg  string
}

func (e APIError) Error() string {
    return e.Msg
}
```

Interview line:

> Return errors for expected failures, wrap errors with `%w` to preserve cause, use `errors.Is` for sentinel errors, and `errors.As` for typed errors.

