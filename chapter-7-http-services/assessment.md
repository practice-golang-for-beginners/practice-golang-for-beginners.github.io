---
layout: default
title: Assessment
parent: Chapter 7 – HTTP Services
nav_order: 4
---

# 📝 Assessment – HTTP Services in Go

This assessment is designed to evaluate your understanding of building **HTTP services in Go**, including handler design, JSON processing, middleware, and testing.

---

## 🎯 Assessment Objectives

By completing this assessment, you should be able to:

* Design and implement HTTP handlers
* Handle JSON request/response correctly
* Apply middleware patterns
* Use proper HTTP status codes
* Write clean, testable Go code
* Reason about real-world backend scenarios

---

## 📚 Section 1: Conceptual Questions

### Q1. Explain how `http.Handler` works in Go.

* What is the role of `ServeHTTP`?
* How does `http.HandlerFunc` simplify handler creation?

---

### Q2. Why does Go prefer using the standard library (`net/http`) over heavy frameworks?

Discuss in terms of:

* Performance
* Simplicity
* Maintainability

---

### Q3. What is middleware in Go?

* How is it implemented?
* Provide a real-world example (e.g., logging, authentication).

---

### Q4. Why is explicit error handling preferred in Go HTTP services?

Compare with exception handling in Python.

---

### Q5. What are the benefits of using `context.Context` in HTTP services?

---

## 🧩 Section 2: Code Understanding

### Q6. Analyze the following code:

```go id="a1code"
func handler(w http.ResponseWriter, r *http.Request) {
	if r.Method != http.MethodGet {
		http.Error(w, "Invalid method", http.StatusBadRequest)
		return
	}
	w.Write([]byte("OK"))
}
```

* What is correct in this implementation?
* What improvements would you suggest?

---

### Q7. Identify issues in this JSON handler:

```go id="a2code"
func createUser(w http.ResponseWriter, r *http.Request) {
	var user User
	json.NewDecoder(r.Body).Decode(&user)
	w.WriteHeader(200)
}
```

* List at least 3 problems
* Suggest improvements

---

## 💻 Section 3: Practical Tasks

### Q8. Build a Simple HTTP Endpoint

Create an endpoint:

```
GET /health
```

Requirements:

* Return status `200 OK`
* Response body: `"Service is healthy"`
* Include proper headers

---

### Q9. JSON API Implementation

Create an endpoint:

```
POST /user
```

Requirements:

* Accept JSON input:

  ```json
  { "name": "John" }
  ```
* Return:

  ```json
  { "message": "User created" }
  ```
* Handle invalid JSON properly
* Return appropriate status codes

---

### Q10. Middleware Implementation

Implement a logging middleware that:

* Logs HTTP method and path
* Wraps an existing handler

---

## 🧪 Section 4: Testing

### Q11. Write a test for the `/health` endpoint:

* Validate status code
* Validate response body

---

### Q12. Write a table-driven test for `/user` endpoint:

* Valid request
* Invalid JSON
* Empty body

---

## 🧠 Section 5: Design & Scenario-Based Questions

### Q13. How would you structure a production-ready Go HTTP service?

Discuss:

* Project structure
* Separation of concerns
* Dependency injection

---

### Q14. A service is experiencing high latency. What areas would you investigate?

---

### Q15. How would you handle graceful shutdown in a production Go service?

---

## 📊 Evaluation Criteria

| Area                     | Weightage |
| ------------------------ | --------- |
| Conceptual Understanding | 20%       |
| Code Quality             | 25%       |
| Correctness              | 25%       |
| Testing                  | 15%       |
| Design Thinking          | 15%       |

---

## 💡 Mentor Notes

* Focus on **clarity and correctness over complexity**
* Encourage idiomatic Go patterns
* Look for:

  * Proper error handling
  * Clean handler design
  * Thoughtful testing

---

## 📌 Key Takeaway

This assessment is not just about writing code—it’s about thinking like a **Go backend engineer**.

---
