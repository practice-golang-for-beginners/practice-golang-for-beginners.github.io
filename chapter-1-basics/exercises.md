---
layout: default
title: Exercises
parent: Chapter 1 – Basics
nav_order: 2
---

# 📘 Chapter 1 – Exercises

*(Go Fundamentals & Tooling)*

---

## 🔹 Section 1 – Setup & Toolchain

---

### Exercise 1 – Environment Verification

**Objective:** Understand Go installation and toolchain.

1. Install Go.
2. Run:

   ```
   go version
   go env
   ```
3. Create a new module:

   ```
   go mod init github.com/yourname/chapter1
   ```
4. Run `go mod tidy`.

📌 Write a short explanation:

* What does `go.mod` do?
* Why is module naming important?

---

### Exercise 2 – First Program

**Objective:** Write and run a Go program.

Create `main.go` that prints:

```
Welcome to Go Programming
```

Then:

* Run it using `go run`
* Build it using `go build`
* Execute the compiled binary

📌 Question:

* What is the difference between `go run` and `go build`?

---

## 🔹 Section 2 – Variables & Types

---

### Exercise 3 – Type Declaration

Write a program that declares:

* An integer
* A float
* A boolean
* A string

Print their types using:

```go
fmt.Printf("%T\n", variable)
```

📌 Explain:

* What is static typing?
* How is this different from Python?

---

### Exercise 4 – Type Inference

Rewrite Exercise 3 using short declaration (`:=`).

📌 Question:

* When can you NOT use `:=`?

---

### Exercise 5 – Zero Values

Declare variables without assigning values:

```go
var a int
var b string
var c bool
```

Print them.

📌 Explain:

* What are zero values?
* Why is this useful?

---

## 🔹 Section 3 – Control Flow

---

### Exercise 6 – Conditional Logic

Write a program that:

* Takes an integer variable
* Prints:

  * "Positive"
  * "Negative"
  * "Zero"

📌 Extend:

* Add initialization inside the `if` condition.

---

### Exercise 7 – Looping

Print numbers from 1 to 20 using:

1. Classic `for` loop
2. While-style loop
3. Infinite loop with break

📌 Question:

* Why does Go not have a `while` keyword?

---

### Exercise 8 – Switch Case

Write a program that:

* Accepts a string day of week
* Prints:

  * "Weekday"
  * "Weekend"

📌 Extend:

* Use switch without specifying a variable.

---

## 🔹 Section 4 – Functions

---

### Exercise 9 – Simple Function

Create a function:

```go
func multiply(a, b int) int
```

Call it from `main`.

---

### Exercise 10 – Multiple Return Values

Create a function:

```go
func divide(a, b int) (int, error)
```

Handle division-by-zero properly.

📌 Reflect:

* Why does Go return errors instead of throwing exceptions?

---

### Exercise 11 – Ignoring Return Values

Call your divide function and:

* Ignore the result
* Ignore the error

Observe compiler behavior.

📌 Question:

* Why does Go not allow unused variables?

---

## 🔹 Section 5 – Error Handling Practice

---

### Exercise 12 – Custom Error

Create a function that:

* Accepts age
* Returns error if age < 18

Use `fmt.Errorf`.

---

### Exercise 13 – Error Propagation

Create:

* A function that validates input
* Another function that calls it

Propagate error upward properly.

📌 Do NOT log and return. Choose one.

---

## 🔹 Section 6 – Formatting & Tooling Discipline

---

### Exercise 14 – Break Formatting

Write badly formatted Go code intentionally.

Run:

```
go fmt
```

Observe changes.

📌 Reflect:

* Why is enforced formatting beneficial in teams?

---

### Exercise 15 – Unused Import

Add an unused import.

Observe compiler error.

📌 Explain:

* Why does Go treat unused imports as errors?

---

## 🔹 Section 7 – Testing Basics

---

### Exercise 16 – First Test

Write a test for your `multiply` function.

Run:

```
go test
```

---

### Exercise 17 – Failing Test

Intentionally break your function.

Run `go test`.

Observe output.

📌 What does Go test output show?

---

### Exercise 18 – Table-Driven Test

Convert your multiply test into a table-driven test.

Include at least 5 test cases.

---

## 🔹 Section 8 – Mini Integration Task

---

### Exercise 19 – Calculator CLI

Build a small CLI program that:

* Accepts two integers
* Performs:

  * Addition
  * Subtraction
  * Multiplication
  * Division

Handle division by zero.

Include tests.

---

### Exercise 20 – Reflection & Comparison

Write a short Markdown file:

```
python_vs_go.md
```

Discuss:

1. 5 differences you observed
2. 2 things you liked in Go
3. 2 things you found difficult
4. When would you choose Go over Python?

This builds engineering maturity, not just syntax knowledge.

---

# 🎯 Outcome of These 20 Exercises

By completing these, the learner will:

* Use Go toolchain confidently
* Understand static typing
* Write idiomatic functions
* Handle errors properly
* Write tests
* Think critically about language design

---
