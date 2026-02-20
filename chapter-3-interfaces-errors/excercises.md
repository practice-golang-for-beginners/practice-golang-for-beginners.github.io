---
layout: default
title: Study Material
parent: Chapters
nav_order: 3
---



**Interfaces & Error Handling**

---

# 🟢 Level 1 – Foundations (Warm-up)

### 1️⃣ Define a Simple Interface

Create an interface called `Printer` with a method:

```go
Print(message string) error
```

Then create a struct `ConsolePrinter` that implements it.

---

### 2️⃣ Implicit Implementation Check

Create another struct `FilePrinter` that also implements `Printer`.

Verify (at compile time) that both satisfy the interface using:

```go
var _ Printer = (*YourStruct)(nil)
```

Explain what this line does.

---

### 3️⃣ Small Interface Principle

Define a large interface:

```go
type UserService interface {
    Create()
    Update()
    Delete()
    Find()
}
```

Refactor it into smaller interfaces following Go best practices.

Explain why this is better.

---

### 4️⃣ Python Analogy Reflection

Write a short explanation:

* How is Go’s interface system similar to Python duck typing?
* What safety does Go provide that Python does not?

(No code required — conceptual.)

---

# 🟡 Level 2 – Error Handling Basics

### 5️⃣ Basic Error Return

Write a function:

```go
Divide(a, b int) (int, error)
```

Return an error when dividing by zero.

---

### 6️⃣ Sentinel Error

Create a package-level error:

```go
var ErrUserNotFound = errors.New("user not found")
```

Write a function `FindUser(id string)` that returns this error for unknown users.

---

### 7️⃣ Error Comparison

Write a small program that:

* Calls `FindUser`
* Uses `errors.Is()` to check for `ErrUserNotFound`
* Prints a custom message

---

### 8️⃣ Error Wrapping

Modify `FindUser` so that it wraps the sentinel error using:

```go
fmt.Errorf("database lookup failed: %w", ErrUserNotFound)
```

Then detect it properly using `errors.Is()`.

---

# 🟠 Level 3 – Interfaces + Error Design

### 9️⃣ Storage Interface

Define an interface:

```go
type Storage interface {
    Save(data string) error
    Get(id string) (string, error)
}
```

Create an `InMemoryStorage` implementation using a map.

---

### 🔟 Implement a Failing Storage

Create a second implementation:

```go
type FailingStorage struct{}
```

Its `Save()` method should always return an error.

---

### 1️⃣1️⃣ Service Layer

Create a struct:

```go
type UserService struct {
    storage Storage
}
```

Add a method:

```go
CreateUser(name string) error
```

Use `storage.Save()` internally.

---

### 1️⃣2️⃣ Error Propagation

Modify `CreateUser` to wrap storage errors using `%w`.

Test that the original error can still be detected using `errors.Is()`.

---

# 🔵 Level 4 – Custom Errors

### 1️⃣3️⃣ Create a Custom Error Type

Create:

```go
type ValidationError struct {
    Field string
    Message string
}
```

Implement the `Error()` method.

---

### 1️⃣4️⃣ Use Custom Error

Modify `CreateUser` so that:

* If `name == ""`, return a `ValidationError`.

---

### 1️⃣5️⃣ Detect Custom Error

Write code that:

* Calls `CreateUser("")`
* Uses `errors.As()` to extract `ValidationError`
* Prints the field name

---

# 🟣 Level 5 – Testing & Mocking

### 1️⃣6️⃣ Create a Mock Storage

Create:

```go
type MockStorage struct {
    ShouldFail bool
}
```

Return error when `ShouldFail == true`.

---

### 1️⃣7️⃣ Test Success Path

Write a unit test for `CreateUser`:

* Using `MockStorage` with `ShouldFail = false`
* Ensure no error is returned

---

### 1️⃣8️⃣ Test Failure Path

Write a test where:

* `MockStorage` returns error
* Verify that `CreateUser` wraps the error properly

---

### 1️⃣9️⃣ Test Validation Error

Write a test that:

* Calls `CreateUser("")`
* Asserts that returned error is `ValidationError`
* Use `errors.As()`

---

# 🔴 Level 6 – Design Thinking Challenge

### 2️⃣0️⃣ Mini Architecture Exercise

Design a small notification system:

* Define `Notifier` interface with:

  ```go
  Send(message string) error
  ```
* Implement:

  * `EmailNotifier`
  * `SMSNotifier`
* Create a `NotificationService` that depends only on `Notifier`.
* Properly wrap and propagate errors.
* Write tests covering:

  * Success
  * Failure
  * Error wrapping detection

This exercise should demonstrate:

* Interface-based design
* Error wrapping
* Mocking
* Test coverage of error paths

---

# 🎯 Expected Outcome After These Exercises

If student completes these:

* student understands implicit interfaces deeply
* student knows when and how to design interfaces
* student handles errors idiomatically
* student understands error wrapping
* student can use `errors.Is()` and `errors.As()`
* student can mock dependencies cleanly
* student writes testable Go code

---


