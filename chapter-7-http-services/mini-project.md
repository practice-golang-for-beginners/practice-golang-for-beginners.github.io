---
layout: default
title: Mini Project – HTTP Service
parent: Chapter 7 – HTTP Services
nav_order: 7
---

# 🚀 Mini Project – Build a User Management HTTP Service

In this mini-project, you will build a **production-style HTTP service in Go** that manages users.

This project consolidates everything you’ve learned:

* HTTP handlers
* JSON APIs
* Middleware
* Error handling
* Testing
* Clean architecture

---

## 🎯 Project Objective

Build a **User Management Service** with the following capabilities:

* Create a user
* Fetch all users
* Health check endpoint
* Middleware for logging
* Proper error handling
* Testable design

---

## 🧱 Functional Requirements

### 1. Health Check API

```http
GET /health
```

Response:

```json
{ "status": "UP" }
```

---

### 2. Create User

```http
POST /users
```

Request:

```json
{
  "name": "Aditya",
  "age": 30
}
```

Response:

```json
{
  "message": "User created"
}
```

Requirements:

* Validate input
* Return `400` for invalid data
* Return `201 Created` on success

---

### 3. Get Users

```http
GET /users
```

Response:

```json
[
  { "name": "Aditya", "age": 30 }
]
```

---

## 🧠 Non-Functional Requirements

* Use **idiomatic Go**
* Keep handlers **small and clean**
* Separate **business logic from HTTP layer**
* Use **interfaces for services**
* Add **middleware (logging + headers)**
* Use **proper HTTP status codes**

---

## 🏗️ Suggested Project Structure

```id="projstruct"
project/
│
├── main.go
├── handler/
│   └── user_handler.go
├── service/
│   └── user_service.go
├── middleware/
│   └── middleware.go
├── model/
│   └── user.go
├── test/
│   └── handler_test.go
```

---

## 🔌 Middleware Requirements

Implement:

* Logging middleware → logs method + path
* Header middleware → adds:

  ```id="hdr"
  X-App-Version: 1.0
  ```

---

## 🧪 Testing Requirements

You must write tests for:

* `/health` endpoint
* `/users` (GET and POST)
* Error scenarios:

  * Invalid JSON
  * Missing fields

Use:

* `httptest`
* Table-driven tests

---

## ⚠️ Error Handling Expectations

Handle:

* Invalid JSON → `400 Bad Request`
* Missing fields → `400 Bad Request`
* Internal errors → `500 Internal Server Error`

Return meaningful messages.

---

## 🧩 Bonus Features (Optional)

* Add in-memory storage (slice/map)
* Add request ID middleware
* Add timeout using `context`
* Add basic logging format (timestamped)

---

## 📊 Evaluation Criteria

| Area           | Expectation                   |
| -------------- | ----------------------------- |
| Code Structure | Clean, modular                |
| Readability    | Simple, idiomatic             |
| Error Handling | Explicit and correct          |
| Testing        | Coverage + table-driven tests |
| Design         | Separation of concerns        |

---

## 🧠 Mentor Guidance

* Encourage **iterative development**
* Start simple → improve gradually
* Focus on **correctness first, optimization later**
* Review code for:

  * Handler size
  * Error handling
  * Naming clarity

---

## 🚀 Expected Outcome

By completing this project, you will:

* Build a real HTTP service in Go
* Understand backend service design
* Gain confidence in production-style coding

---

## 📌 Final Insight

> “If you can build and test this service cleanly, you are ready to contribute to real Go backend systems.”

---
