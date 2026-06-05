# defer, panic, and recover

## defer

`defer` schedules a function call to run when the surrounding function returns.

Key rules:

- deferred calls run in LIFO order
- deferred function arguments are evaluated immediately
- commonly used for cleanup, unlock, close, and recover

```go
func demo() {
    defer fmt.Println("third")
    defer fmt.Println("second")
    defer fmt.Println("first")
}
```

Output:

```text
first
second
third
```

## panic

`panic` stops normal execution in the current goroutine. Deferred functions still run while the stack unwinds.

Use panic for programmer mistakes or unrecoverable states. For normal expected failures, return `error`.

Good:

```go
return fmt.Errorf("find user: %w", err)
```

Usually bad:

```go
panic("user not found")
```

## recover

`recover` catches a panic only when called inside a deferred function in the same goroutine.

```go
func safeRun() {
    defer func() {
        if r := recover(); r != nil {
            fmt.Println("recovered:", r)
        }
    }()

    panic("boom")
}
```

## Panic Inside Goroutines

Recover does not cross goroutine boundaries.

```go
func main() {
    defer recoverMain()

    go func() {
        panic("this is not recovered by main")
    }()
}
```

Each goroutine that may panic needs its own deferred recovery.

```go
go func() {
    defer func() {
        if r := recover(); r != nil {
            log.Println("worker recovered:", r)
        }
    }()

    doWork()
}()
```

Interview line:

> Panic is for exceptional programmer errors. Recover only works in a deferred function in the same goroutine. Normal application failures should be returned as errors.

