# Generics

Generics let you write functions and types that work with different concrete types while still keeping compile-time type safety.

Before generics, you often had to either:

- duplicate code for each type
- use `interface{}` / `any` and type assertions

With generics, you can keep the type flexible without losing type information.

---

## Basic Generic Function

```go
func Print[T any](value T) {
    fmt.Println(value)
}
```

Breakdown:

```go
func Print
```

Function name.

```go
[T any]
```

`T` is a type parameter.

`any` means `T` can be any type.

```go
value T
```

The function accepts a value whose type is `T`.

Usage:

```go
Print[int](10)
Print[string]("hello")
```

Usually Go can infer the type:

```go
Print(10)
Print("hello")
```

---

## Generic Function With Return Value

```go
func First[T any](items []T) (T, bool) {
    if len(items) == 0 {
        var zero T
        return zero, false
    }
    return items[0], true
}
```

Usage:

```go
n, ok := First([]int{1, 2, 3})
s, ok := First([]string{"go", "java"})
```

Important:

```go
var zero T
```

This gives the zero value for whatever type `T` is.

---

## Generic Type

```go
type Stack[T any] struct {
    items []T
}

func (s *Stack[T]) Push(value T) {
    s.items = append(s.items, value)
}

func (s *Stack[T]) Pop() (T, bool) {
    if len(s.items) == 0 {
        var zero T
        return zero, false
    }

    last := len(s.items) - 1
    value := s.items[last]
    s.items = s.items[:last]
    return value, true
}
```

Usage:

```go
var intStack Stack[int]
intStack.Push(10)

var stringStack Stack[string]
stringStack.Push("hello")
```

---

## Type Constraints

Sometimes `any` is too broad.

Example:

```go
func Add[T any](a, b T) T {
    return a + b // compile error
}
```

Why?

Because not every type supports `+`.

Use a constraint:

```go
type Number interface {
    ~int | ~int64 | ~float64
}

func Add[T Number](a, b T) T {
    return a + b
}
```

Now `T` must be one of the allowed number-like types.

The `~` means:

> allow types whose underlying type is this type.

Example:

```go
type Age int

Add(Age(10), Age(20)) // works because Age's underlying type is int
```

---

## Generic Channel Example

This is from the concurrency repo:

```go
func FanOut[T any](in <-chan T, n int) []<-chan T
```

Meaning:

- `T any`: works with any value type
- `in <-chan T`: input is a receive-only channel of `T`
- `n int`: number of output channels
- `[]<-chan T`: returns a slice of receive-only channels of `T`

So it can work with:

```go
chan int
chan string
chan Article
```

without rewriting the function.

---

## any vs interface{}

`any` is an alias for `interface{}`.

These are the same:

```go
func Print[T any](value T)
func Print[T interface{}](value T)
```

`any` is preferred in generic code because it reads better.

---

## Generics vs interface{}

Using `interface{}`:

```go
func Print(value interface{}) {
    fmt.Println(value)
}
```

This accepts anything, but the value loses its specific static type inside the function.

Using generics:

```go
func Identity[T any](value T) T {
    return value
}
```

This preserves the type:

```go
x := Identity(10)      // x is int
y := Identity("hello") // y is string
```

Interview line:

> `interface{}` is useful when behavior truly does not depend on the type. Generics are useful when the code should work for many types while preserving type safety.

---

## When To Use Generics

Use generics for:

- reusable data structures
- helper functions over slices/maps
- algorithms that work across types
- channel/pipeline helpers

Avoid generics when:

- a simple concrete type is clearer
- an interface based on behavior is better
- the generic code becomes harder to read than duplication

Interview answer:

> Generics let Go functions and types work with multiple concrete types while keeping compile-time type safety. `T any` means `T` can be any type, and constraints can restrict `T` to types that support required operations.

