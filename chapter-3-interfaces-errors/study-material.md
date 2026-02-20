---
layout: default
title: Study Material
parent: Chapters
nav_order: 2
---

## Goal

Learn Go’s most powerful design tools:

* Interfaces (implicit implementation)
* Duck typing (Python analogy)
* Idiomatic error handling
* Custom errors and wrapping
* Designing testable systems using interfaces

This chapter is critical. If understood properly, it unlocks idiomatic Go design.

---

# 1️⃣ Understanding Interfaces in Go

## 1.1 What is an Interface?

An interface in Go defines **behavior**, not structure.

It is a contract that says:

> “Any type that implements these methods satisfies this interface.”

Example:

```go
type Notifier interface {
    Send(message string) error
}
```

This means:

* Any type with a method:

  ```go
  Send(string) error
  ```

  automatically satisfies the `Notifier` interface.

There is **no explicit declaration** like:

```go
class EmailNotifier implements Notifier
```

Go does not require explicit implementation.

---

## 1.2 Implicit Implementation (Very Important)

In Go, interfaces are satisfied implicitly.

Example:

```go
type EmailNotifier struct{}

func (e EmailNotifier) Send(message string) error {
    fmt.Println("Sending email:", message)
    return nil
}
```

Even though we never wrote:

```go
implements Notifier
```

This type satisfies the interface because:

* It has the required method signature.

This design reduces coupling dramatically.

---

# 2️⃣ Interfaces vs Python Duck Typing

Python example:

```python
class EmailNotifier:
    def send(self, message):
        print("Sending email:", message)
```

In Python, if an object has a `send()` method, it works.

This is called **duck typing**:

> "If it walks like a duck and quacks like a duck, it's a duck."

Go works similarly — but with compile-time safety.

Key differences:

| Python                            | Go                                  |
| --------------------------------- | ----------------------------------- |
| Dynamic typing                    | Static typing                       |
| Errors at runtime                 | Errors at compile-time              |
| No explicit interface definitions | Interfaces define expected behavior |

Go gives you duck typing **with compile-time guarantees**.

---

# 3️⃣ Why Interfaces Matter in Real Systems

Interfaces allow:

* Decoupling components
* Easier testing
* Swapping implementations
* Clean architecture

Bad design (tight coupling):

```go
type Service struct {
    email EmailNotifier
}
```

Better design:

```go
type Service struct {
    notifier Notifier
}
```

Now:

* You can inject EmailNotifier
* Or SMSNotifier
* Or MockNotifier for tests

This is foundational for scalable backend design.

---

# 4️⃣ Small Interfaces > Large Interfaces

In Go, interfaces should be small.

Prefer:

```go
type Writer interface {
    Write([]byte) (int, error)
}
```

Over:

```go
type BigService interface {
    Save()
    Delete()
    Update()
    Find()
    Validate()
}
```

Why?

Because small interfaces:

* Are easier to implement
* Are easier to test
* Reduce dependency coupling

This philosophy is core to Go design.

---

# 5️⃣ Designing Service Interfaces

Let’s define a storage abstraction:

```go
type Storage interface {
    Save(data string) error
    Get(id string) (string, error)
}
```

Implementation 1:

```go
type InMemoryStorage struct {
    data map[string]string
}
```

Implementation 2:

```go
type FileStorage struct {
    filePath string
}
```

Your business logic should depend only on:

```go
type Storage
```

Not concrete types.

This enables clean architecture principles.

---

# 6️⃣ Error Handling in Go (The Big Shift)

In Python:

```python
try:
    result = do_something()
except Exception as e:
    print(e)
```

In Go:

```go
result, err := doSomething()
if err != nil {
    return err
}
```

Go uses **explicit error returns**, not exceptions.

This means:

* No hidden control flow
* No stack unwinding surprises
* Error handling is visible

This is deliberate design.

---

# 7️⃣ Idiomatic Error Handling Patterns

## 7.1 Always Check Errors

Never ignore:

```go
value, err := something()
if err != nil {
    return err
}
```

Avoid:

```go
value, _ := something()  // bad practice
```

---

## 7.2 Wrap Errors (Go 1.13+)

Instead of:

```go
return err
```

Use:

```go
return fmt.Errorf("failed to save data: %w", err)
```

Why?

* Adds context
* Preserves original error
* Supports `errors.Is()` and `errors.As()`

---

## 7.3 Comparing Errors

Define sentinel error:

```go
var ErrNotFound = errors.New("not found")
```

Then check:

```go
if errors.Is(err, ErrNotFound) {
    // handle not found
}
```

Never compare error strings.

---

# 8️⃣ Creating Custom Errors

Sometimes you need structured errors.

Example:

```go
type ValidationError struct {
    Field string
    Message string
}

func (v ValidationError) Error() string {
    return fmt.Sprintf("validation failed on %s: %s", v.Field, v.Message)
}
```

Now it satisfies the built-in `error` interface:

```go
type error interface {
    Error() string
}
```

This allows rich error types.

---

# 9️⃣ Error Propagation Strategy

Bad:

```go
if err != nil {
    fmt.Println(err)
}
```

Good:

* Log at the boundary
* Return errors upward
* Add context when needed

Design principle:

> Handle errors where you have enough context to make a decision.

---

# 🔟 Combining Interfaces and Errors (Realistic Design)

Example:

```go
type Notifier interface {
    Send(message string) error
}
```

Your service:

```go
type OrderService struct {
    notifier Notifier
}

func (s OrderService) PlaceOrder() error {
    err := s.notifier.Send("Order placed")
    if err != nil {
        return fmt.Errorf("order notification failed: %w", err)
    }
    return nil
}
```

Notice:

* The service does not know implementation details.
* Errors are wrapped.
* The system is testable.

This is idiomatic Go.

---

# 11️⃣ Testing with Interfaces

Interfaces enable mocking.

Example mock:

```go
type MockNotifier struct {
    ShouldFail bool
}

func (m MockNotifier) Send(message string) error {
    if m.ShouldFail {
        return errors.New("mock failure")
    }
    return nil
}
```

Now you can:

* Test success path
* Test failure path
* Assert behavior

This is why interfaces are critical for clean testing.

---

# 12️⃣ Common Beginner Mistakes

1. Creating interfaces too early
2. Creating huge interfaces
3. Comparing error strings
4. Swallowing errors
5. Not wrapping errors
6. Logging errors multiple times

Avoid these and you’re already ahead of many Go developers.

---

# 13️⃣ Design Philosophy Summary

Interfaces in Go are:

* Behavior contracts
* Implicitly implemented
* Designed for decoupling
* Meant to be small

Error handling in Go is:

* Explicit
* Simple
* Context-rich
* Transparent

Together, they form the backbone of idiomatic Go systems.

---

# 🎯 End of Week 3 Outcome

By the end of this week, the learner should:

* Understand implicit interfaces deeply
* Design small service abstractions
* Implement multiple implementations
* Handle errors idiomatically
* Wrap and propagate errors correctly
* Write testable code using interfaces

---

