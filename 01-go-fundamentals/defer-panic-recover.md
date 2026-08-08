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

## defer Internals for Senior Interviews

Mechanics:

```text
defer registers a function call to run right before the surrounding function returns.
```

Each goroutine has its own defer chain/stack for active function calls.

Deferred calls run in LIFO order:

```text
Last deferred = first executed
```

Example:

```go
func demo() {
	defer fmt.Println("one")
	defer fmt.Println("two")
	defer fmt.Println("three")
}
```

Output:

```text
three
two
one
```

Interview line:

```text
defer behaves like a per-goroutine LIFO stack of cleanup calls. When the function returns normally or through panic unwinding, deferred calls execute in reverse order.
```

## defer Argument Evaluation

Arguments passed to a deferred function are evaluated immediately when `defer` is declared, not when the deferred call runs.

```go
func printVal() {
	x := 10
	defer fmt.Println("Deferred:", x)

	x = 20
	fmt.Println("Normal:", x)
}
```

Output:

```text
Normal: 20
Deferred: 10
```

Why:

```text
The value x=10 is captured as an argument at the time defer is registered.
```

If you want the latest value, use a closure that reads the variable at execution time:

```go
func printLatest() {
	x := 10
	defer func() {
		fmt.Println("Deferred:", x)
	}()

	x = 20
	fmt.Println("Normal:", x)
}
```

Output:

```text
Normal: 20
Deferred: 20
```

Senior nuance:

```text
defer arguments are evaluated immediately, but variables referenced inside a deferred closure are read when the closure executes.
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
