---
layout: default
title: Week 3 – Interfaces & Error Handling
parent: Chapters
nav_order: 1
---



## 🎯 Goal of This Week

This week focuses on two of Go’s most powerful and foundational design tools:

* **Interfaces (implicit implementation)**
* **Idiomatic error handling**

By the end of this week, you should be able to:

* Design small, meaningful interfaces
* Understand Go’s implicit interface model
* Handle errors explicitly and idiomatically
* Wrap and propagate errors correctly
* Write clean, testable service-layer code
* Test success and failure paths properly

This week marks a shift from “learning syntax” to **thinking like a Go engineer**.

---

# 📚 What You Will Learn

## 1️⃣ Interfaces in Go

* What interfaces represent (behavior contracts)
* Implicit implementation (no `implements` keyword)
* Compile-time interface checks
* Small interface principle
* Dependency inversion using interfaces

You will learn how Go achieves flexibility similar to Python’s duck typing — but with compile-time safety.

---

## 2️⃣ Error Handling Philosophy

Go does not use exceptions.

Instead, it uses explicit error returns:

```go
value, err := someFunction()
if err != nil {
    return err
}
```

You will learn:

* Why explicit error handling improves clarity
* When to return vs wrap errors
* How to use `%w` for wrapping
* Why string comparison of errors is bad practice

---

## 3️⃣ Sentinel Errors

Defining reusable package-level errors:

```go
var ErrNotFound = errors.New("not found")
```

And checking them properly using:

```go
errors.Is(err, ErrNotFound)
```

---

## 4️⃣ Custom Error Types

Creating structured errors:

```go
type ValidationError struct {
    Field   string
    Message string
}
```

And extracting them safely using:

```go
errors.As(err, &validationErr)
```

---

## 5️⃣ Interface-Based Design

You will implement:

* `Storage` interface
* Multiple storage implementations
* `UserService` that depends only on the interface
* Mock implementations for testing

This introduces you to:

* Dependency injection
* Loose coupling
* Clean architecture principles

---

# 🧪 What You Will Practice

* Implementing multiple types satisfying the same interface
* Wrapping errors with context
* Detecting wrapped errors using `errors.Is`
* Extracting custom errors using `errors.As`
* Writing table-driven tests
* Mocking dependencies using interfaces

---

# 📁 Folder Structure for This Week

```
chapter-3-interfaces-errors/
│
├── README.md
├── study-material.md
├── exercises.md
└── solutions.md
```

---

# 🧠 Key Design Principles to Remember

### ✔ Interfaces should be small

Prefer focused behavior contracts.

### ✔ Do not define interfaces prematurely

Define them where they are used, not where they are implemented.

### ✔ Never compare error strings

Use `errors.Is` or `errors.As`.

### ✔ Wrap errors with context

Add meaning without losing original cause.

### ✔ Log errors at system boundaries

Avoid logging the same error multiple times.

---

# 🚩 Common Mistakes to Avoid

* Creating large “God interfaces”
* Swallowing errors
* Ignoring returned errors
* Comparing error messages
* Logging inside library code unnecessarily
* Overusing interfaces for simple cases

---

# 🗓 Weekly Flow

**During the week:**

* Study the material
* Complete exercises progressively
* Write and run tests locally
* Experiment and refactor

**Friday Review Session:**

* Discuss implementation decisions
* Review error handling style
* Evaluate interface design quality
* Refactor together if needed
* Decide next week’s topics

---

# 🎯 Expected Outcome by Friday

You should be able to:

* Explain implicit interfaces clearly
* Design a service that depends on an abstraction
* Implement at least two implementations of an interface
* Properly wrap and propagate errors
* Write clean unit tests for both success and failure paths

---

# 👨‍💻 Maintained By

**Aditya Pratap Bhuyan**
Practice GoLang for Beginners
Licensed under GNU GPL v3

---
