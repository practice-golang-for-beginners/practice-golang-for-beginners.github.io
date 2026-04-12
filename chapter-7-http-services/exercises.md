---
layout: default
title: Exercises
parent: Chapter 7 – HTTP Services
nav_order: 3
---

# 🧩 Exercises – HTTP Services in Go

These exercises are designed to help you **practice building HTTP services step by step**.
Start simple and gradually move toward more realistic backend scenarios.

---

## 🎯 Exercise Guidelines

* Write clean and readable code
* Handle errors explicitly
* Use proper HTTP status codes
* Keep handlers small and focused
* Write tests wherever applicable

---

# 🟢 Level 1 – Basics (Warm-up)

### Exercise 1: Hello Endpoint

Create an endpoint:

```id="ex1"
GET /hello
```

Requirements:

* Return: `"Hello, Go HTTP!"`
* Status: `200 OK`

---

### Exercise 2: Method Validation

Enhance `/hello`:

* Allow only `GET`
* Return `405 Method Not Allowed` for others

---

### Exercise 3: Custom Handler

Create a custom struct that implements:

```go id="ex3"
ServeHTTP(w http.ResponseWriter, r *http.Request)
```

Use it for a new route:

```id="ex3route"
/custom
```

---

# 🟡 Level 2 – JSON Handling

### Exercise 4: JSON Response

Create:

```id="ex4"
/user
```

Return JSON:

```json
{
  "name": "Aditya",
  "role": "Developer"
}
```

Requirements:

* Set correct `Content-Type`
* Use `json.NewEncoder`

---

### Exercise 5: JSON Request Parsing

Create:

```id="ex5"
POST /echo
```

Requirements:

* Accept JSON input
* Return the same JSON back
* Handle invalid JSON properly

---

### Exercise 6: Validation

Enhance `/echo`:

* Reject empty fields
* Return `400 Bad Request` with message

---

# 🟠 Level 3 – Middleware

### Exercise 7: Logging Middleware

Create middleware that logs:

* HTTP method
* Request path

Apply it to at least one endpoint.

---

### Exercise 8: Header Middleware

Create middleware that:

* Adds a custom header:

  ```id="ex8header"
  X-App-Version: 1.0
  ```

---

### Exercise 9: Middleware Chaining

Chain multiple middleware:

* Logging + Header middleware

---

# 🔵 Level 4 – Real API Patterns

### Exercise 10: Health Check API

Create:

```id="ex10"
/health
```

Return:

* Status: `200 OK`
* JSON:

  ```json
  { "status": "UP" }
  ```

---

### Exercise 11: User Creation API

Create:

```id="ex11"
POST /users
```

Requirements:

* Accept JSON input (`name`, `age`)
* Return success message
* Validate input
* Return appropriate status codes

---

### Exercise 12: Error Handling

Enhance `/users`:

* Return meaningful error messages
* Handle malformed JSON
* Handle missing fields

---

# 🔴 Level 5 – Advanced & Production Thinking

### Exercise 13: Context Timeout

* Add a timeout to a handler
* Simulate a slow operation
* Return timeout error if exceeded

---

### Exercise 14: Graceful Shutdown

Modify your server:

* Handle OS interrupt
* Shutdown cleanly

---

### Exercise 15: Refactoring for Testability

Refactor your handlers:

* Remove business logic from HTTP layer
* Use interfaces for dependencies

---

# 🧪 Bonus Exercises (Optional but Recommended)

### Exercise 16: Table-Driven Testing

* Write table-driven tests for `/echo`

---

### Exercise 17: HTTP Status Coverage

Ensure your APIs correctly use:

* `200`, `201`, `400`, `404`, `500`

---

### Exercise 18: Simple Router Abstraction

* Create a small wrapper around `http.ServeMux`
* Organize routes cleanly

---

# 🧠 Reflection Questions

* What makes a handler “clean” in Go?
* How does Go’s approach differ from Python frameworks?
* Where should business logic live?

---

## 📌 Key Takeaway

> “Practice is where Go starts to feel natural.”

These exercises are designed to move you from:

* Writing handlers ❌
  to
* Designing real backend services ✅

---
