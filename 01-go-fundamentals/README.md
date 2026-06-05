# 01 - Go Fundamentals

## Topics From Checklist

### defer

- Execution order: LIFO
- Arguments are evaluated immediately
- Used for cleanup, unlock, recover, closing resources

### panic and recover

- What panic does
- Why recover only works inside defer
- Panic inside goroutines
- Why normal errors should use `error`, not panic

### interfaces

- Implicit implementation
- Empty interface / `any`
- Interface nil vs concrete nil
- Why interfaces are useful for testing

### pointers

- Value vs pointer receiver
- When to use pointer receiver
- Passing structs by value vs reference
- Mutability and performance

### slices

- Length vs capacity
- Append behavior
- Underlying array sharing
- Slice memory leaks

### maps

- Map zero value
- Checking key existence
- Concurrent map access problem
- `sync.Map` use cases

### structs and methods

- Composition over inheritance
- Embedded structs
- Method receivers

### error handling

- `errors.New`
- `fmt.Errorf("%w", err)`
- `errors.Is`
- `errors.As`
- Custom error types

---

## Existing Coverage

Partial examples exist in:

- `/Users/sahiltyagi/Desktop/personal projects/my-project/defer-panic`
- `/Users/sahiltyagi/Desktop/personal projects/my-project/interface`
- `/Users/sahiltyagi/Desktop/personal projects/my-project/structs`

---

## Covered In This Folder

- `defer-panic-recover.md`
- `interfaces-pointers-slices-maps.md`
- `errors.md`

These cover:

- interface nil vs concrete nil
- panic inside goroutines
- slice memory leaks
- concurrent map access
- `sync.Map` use cases
- error wrapping and unwrapping
- interfaces for testing
