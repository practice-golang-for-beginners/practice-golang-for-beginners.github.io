---
layout: default
title: Mini Project
parent: Chapter 6 – Context, Timeouts & Cancellation
nav_order: 7
---

# 🚀 Mini Project – Context-Aware API Service

---

## 🎯 Objective

Build a **context-aware backend service** that:

* Handles request timeouts
* Supports cancellation
* Simulates real-world API behavior
* Uses proper Go concurrency patterns

---

## 🧩 Problem Statement

You are building a simple service:

👉 **"User Data Aggregator Service"**

This service:

* Receives an HTTP request
* Calls multiple internal functions (simulating external services)
* Aggregates results
* Returns a response

⚠️ Each internal call:

* May take time
* May fail
* Must respect context

---

## 🏗️ Requirements

### 1. HTTP Endpoint

Create:

```id="vfs3v5"
GET /user?id=123
```

---

### 2. Internal Services (Simulated)

Create functions:

```go id="n67qgz"
func fetchUserProfile(ctx context.Context) (string, error)
func fetchUserOrders(ctx context.Context) (string, error)
func fetchUserRecommendations(ctx context.Context) (string, error)
```

Each function:

* Sleeps for 1–3 seconds
* Uses `select` with `ctx.Done()`
* Returns data or error

---

### 3. Concurrency

* Call all 3 services **concurrently using goroutines**
* Use channels or sync mechanisms
* Aggregate results

---

### 4. Context Propagation

* Extract context from HTTP request:

```go id="l50ipz"
ctx := r.Context()
```

* Pass it to all internal functions

---

### 5. Timeout Handling

* Add a timeout of **2 seconds** for the entire request:

```go id="h7nq3m"
ctx, cancel := context.WithTimeout(r.Context(), 2*time.Second)
defer cancel()
```

---

### 6. Cancellation Handling

* If client disconnects → request should stop
* If timeout occurs → return error

---

### 7. Response

Return JSON:

```json id="o3x0mk"
{
  "profile": "...",
  "orders": "...",
  "recommendations": "...",
  "error": "optional"
}
```

---

## 🧠 Expected Behavior

| Scenario           | Outcome                   |
| ------------------ | ------------------------- |
| All services fast  | Full response             |
| One service slow   | Timeout triggered         |
| Client disconnects | Request cancelled         |
| Error in service   | Partial or error response |

---

## 🧪 Bonus Features (Optional but Recommended)

### ⭐ Partial Response Handling

Return available data even if one service fails.

---

### ⭐ Logging

Log:

* Start time
* Cancellation
* Timeout

---

### ⭐ Retry Mechanism

Retry a failed service once (context-aware).

---

## 🧱 Suggested Structure

```id="9q7nvx"
project/
│
├── main.go
├── handler.go
├── service.go
├── model.go
```

---

## 🧠 Key Concepts Applied

* Context propagation
* Goroutines & concurrency
* Timeout control
* Cancellation handling
* Clean service design

---

## ⚠️ Common Mistakes to Avoid

* ❌ Ignoring `ctx.Done()`
* ❌ Not cancelling context
* ❌ Blocking goroutines
* ❌ Not handling partial failures
* ❌ Using global context

---

## 🧪 How to Test

### Case 1 – Normal Flow

* All services respond quickly
* Expect full response

### Case 2 – Timeout

* Increase sleep time
* Expect timeout error

### Case 3 – Cancellation

* Stop request midway
* Expect graceful exit

---

## 🎯 Evaluation Criteria

| Area           | Expectation                   |
| -------------- | ----------------------------- |
| Code Quality   | Clean, readable, idiomatic    |
| Context Usage  | Proper propagation & handling |
| Concurrency    | Safe and efficient            |
| Error Handling | Robust                        |
| Design         | Modular                       |

---

## 🚀 Stretch Goal (Advanced)

Convert this into:

* A real HTTP server
* Add middleware
* Add structured logging

---

## 💬 Mentor Review Discussion

* How did you handle concurrency?
* Where did you check `ctx.Done()`?
* What would happen under heavy load?
* How would you improve this for production?

---

## 🏁 Outcome

After completing this project, you should be able to:

* Build context-aware services
* Handle timeouts and cancellations correctly
* Think like a backend engineer

---

🎉 Congratulations — you’ve completed one of the most important Go concepts.

---
