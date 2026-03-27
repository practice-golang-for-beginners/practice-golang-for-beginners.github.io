---
layout: default
title: Exercises
parent: Chapter 6 – Context, Timeouts & Cancellation
nav_order: 2
---

# 🧪 Exercises – Context, Timeouts & Cancellation

---

## 🎯 Objective

These exercises will help you:

* Understand how context works in real programs
* Practice cancellation and timeouts
* Build intuition for production-grade Go code

---

## 🟢 Level 1 – Basics (Warm-up)

### Exercise 1: Basic Context Creation

Create a simple program that:

* Creates a `context.Background()`
* Passes it to a function
* Prints a message from the function

👉 Goal: Understand how context is passed

---

### Exercise 2: Manual Cancellation

Write a program where:

* A goroutine runs a loop printing every second
* Use `context.WithCancel`
* Cancel the context after 3 seconds

👉 Expected: Goroutine should stop gracefully

---

### Exercise 3: Timeout Context

* Use `context.WithTimeout` (2 seconds)
* Simulate a task that takes 5 seconds
* Handle timeout properly

👉 Expected output: `context deadline exceeded`

---

## 🟡 Level 2 – Intermediate

### Exercise 4: Respect Cancellation

Write a function:

```go
func longTask(ctx context.Context)
```

* Runs a loop for 10 iterations
* Each iteration sleeps for 1 second
* Stops early if context is cancelled

👉 Use `select` with `ctx.Done()`

---

### Exercise 5: Context Propagation

Create 3 functions:

* `main → service → repository`

Pass context through all layers.

👉 Goal:

* Maintain context chain
* No global variables

---

### Exercise 6: Timeout Wrapper

Write a function:

```go
func executeWithTimeout(parent context.Context)
```

* Creates a child context with timeout
* Calls another function using this context
* Handles cancellation

👉 Think like a service layer

---

## 🔵 Level 3 – Real-World Scenarios

### Exercise 7: Worker with Cancellation

* Create a worker goroutine
* It processes jobs every second
* Stop worker when context is cancelled

👉 Bonus:

* Print "Worker shutting down..."

---

### Exercise 8: API Simulation

Simulate an API call:

* Function waits for 3 seconds
* If context times out → return error
* Otherwise → return success

👉 Use:

* `select`
* `time.After`

---

### Exercise 9: Multiple Goroutines with Shared Context

* Start 3 goroutines
* All receive same context
* Cancel context after 2 seconds

👉 Expected:

* All goroutines stop together

---

## 🔴 Level 4 – Advanced Thinking

### Exercise 10: Prevent Goroutine Leak

Write a function that:

* Starts a goroutine
* Uses context properly
* Ensures no goroutine leak after cancellation

👉 Hint:

* Always listen to `ctx.Done()`

---

### Exercise 11: Context + Channel Integration

* Send data through a channel
* Stop processing when context is cancelled

👉 Combine:

* Channels
* Context
* Select

---

### Exercise 12: HTTP Context Handling (Conceptual + Code)

Write a handler:

```go
func handler(w http.ResponseWriter, r *http.Request)
```

* Extract context from request
* Pass to a function
* Handle cancellation

👉 Think:

* What happens if client disconnects?

---

## ⭐ Bonus Challenge

### Exercise 13: Retry with Context Awareness

Write a retry function:

* Retries operation every second
* Stops retrying if:

  * Success occurs
  * Context is cancelled

👉 This is **real production pattern**

---

## 🧠 Reflection Questions

After completing exercises, think:

* When should you use context?
* What happens if you ignore `ctx.Done()`?
* Why is context passed explicitly?
* How is this different from Python?

---

## 🚀 Next Step

👉 Move to [Solutions](./solutions)
(Only after attempting all exercises)

---
