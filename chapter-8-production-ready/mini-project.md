---
layout: default
title: Mini Project
parent: Chapter 8 – Production Ready Go
nav_order: 7
---

# Mini Project

# Production Ready Task Management Service

Welcome to the final mini project of the 8-Week Go Mentorship Program.

This project combines all the concepts learned throughout the mentorship journey:
- Go fundamentals
- Structs and interfaces
- Error handling
- Collections and pointers
- Concurrency
- Context propagation
- HTTP services
- Production engineering practices

The goal is to build a small but production-style backend service using Go.

---

# Project Objective

Build a production-ready REST API service for managing tasks.

The application should demonstrate:
- Clean architecture
- Idiomatic Go practices
- Proper error handling
- Logging
- Configuration management
- Graceful shutdown
- Testing discipline

---

# Project Requirements

The service should support:

| Method | Endpoint | Description |
|---|---|---|
| GET | /health | Health check endpoint |
| GET | /tasks | Fetch all tasks |
| GET | /tasks/{id} | Fetch task by ID |
| POST | /tasks | Create new task |
| DELETE | /tasks/{id} | Delete task |

---

# Suggested Project Structure

```text
task-service/
│
├── cmd/
│   └── server/
│       └── main.go
│
├── internal/
│   ├── config/
│   ├── handler/
│   ├── logger/
│   ├── middleware/
│   ├── model/
│   ├── repository/
│   └── service/
│
├── tests/
│
├── configs/
│
├── go.mod
├── go.sum
├── README.md
└── Dockerfile
````

---

# Functional Requirements

## 1. Health Endpoint

### Endpoint

```text
GET /health
```

### Response

```json id="4zjlwm"
{
  "status": "UP"
}
```

---

## 2. Fetch All Tasks

### Endpoint

```text
GET /tasks
```

### Response

```json id="j2m9h8"
[
  {
    "id": 1,
    "title": "Learn Go",
    "completed": false
  }
]
```

---

## 3. Fetch Task By ID

### Endpoint

```text
GET /tasks/{id}
```

### Requirements

* Validate task ID
* Return proper HTTP status codes
* Handle missing tasks gracefully

---

## 4. Create Task

### Endpoint

```text
POST /tasks
```

### Request Body

```json id="63zcwa"
{
  "title": "Write unit tests"
}
```

### Requirements

* Validate request payload
* Return appropriate status code
* Handle malformed JSON

---

## 5. Delete Task

### Endpoint

```text
DELETE /tasks/{id}
```

### Requirements

* Validate task existence
* Return proper status response

---

# Non-Functional Requirements

The project must include:

* Structured logging
* Configuration management
* Graceful shutdown
* Error handling
* Unit tests
* Proper package organization

---

# Configuration Management

The application should read configuration from environment variables.

## Required Variables

| Variable | Description      | Default      |
| -------- | ---------------- | ------------ |
| APP_PORT | HTTP server port | 8080         |
| APP_NAME | Application name | task-service |

---

# Example

```go id="wprg41"
port := os.Getenv("APP_PORT")
```

---

# Logging Requirements

Your application should log:

* Server startup
* Incoming requests
* Errors
* Graceful shutdown events

---

# Suggested Logging Format

```text
timestamp=2026-05-28T10:00:00Z level=INFO message="server started"
```

---

# Graceful Shutdown

The application should:

* Listen for interrupt signals
* Stop accepting new requests
* Complete active requests
* Shutdown cleanly

---

# Example

```go id="a1cs9f"
ctx, stop := signal.NotifyContext(context.Background(), os.Interrupt)
defer stop()
```

---

# Suggested Task Model

```go id="3w1d5w"
type Task struct {
    ID        int    `json:"id"`
    Title     string `json:"title"`
    Completed bool   `json:"completed"`
}
```

---

# Suggested Architecture

## Handler Layer

Responsible for:

* HTTP request handling
* Request validation
* Response formatting

---

## Service Layer

Responsible for:

* Business logic
* Task processing
* Validation rules

---

## Repository Layer

Responsible for:

* Data storage
* Data retrieval

For this project:

* In-memory storage is sufficient

---

# Suggested Middleware

Implement middleware for:

* Logging
* Request timing
* Panic recovery

---

# Error Handling Expectations

Avoid:

```go id="k6k3fk"
panic(err)
```

Prefer:

```go id="v64k8e"
return fmt.Errorf("failed to fetch task: %w", err)
```

---

# Unit Testing Requirements

Write tests for:

* Service layer
* Repository layer
* HTTP handlers

---

# Example Test Cases

| Test Case            | Expected Result |
| -------------------- | --------------- |
| Fetch existing task  | HTTP 200        |
| Fetch missing task   | HTTP 404        |
| Create valid task    | HTTP 201        |
| Invalid JSON payload | HTTP 400        |

---

# Benchmarking (Optional)

Create benchmarks for:

* Task retrieval
* Task creation

Example:

```go id="u1v38g"
func BenchmarkGetTasks(b *testing.B)
```

---

# Profiling (Optional)

Try profiling your service under load using:

```bash id="m8cbz7"
go tool pprof
```

---

# Docker Support (Optional)

Create a Dockerfile for the application.

Example:

```dockerfile id="2wtpj8"
FROM golang:1.24

WORKDIR /app

COPY . .

RUN go build -o task-service ./cmd/server

CMD ["./task-service"]
```

---

# GitHub Actions (Optional)

Add CI pipeline support:

* Build
* Test
* Formatting checks

Suggested file:

```text
.github/workflows/go.yml
```

---

# Expected Deliverables

Your project should contain:

```text
README.md
go.mod
go.sum
cmd/
internal/
configs/
tests/
Dockerfile
```

---

# Evaluation Criteria

| Criteria                  | Weight |
| ------------------------- | ------ |
| Code Readability          | High   |
| Error Handling            | High   |
| Go Best Practices         | High   |
| Testing Discipline        | Medium |
| Logging and Observability | Medium |
| Architecture Simplicity   | High   |

---

# Recommended Development Flow

## Step 1

Create project structure.

---

## Step 2

Implement task model and repository.

---

## Step 3

Implement service layer.

---

## Step 4

Implement HTTP handlers.

---

## Step 5

Add middleware.

---

## Step 6

Add configuration support.

---

## Step 7

Add graceful shutdown.

---

## Step 8

Write tests.

---

# Bonus Enhancements

Try implementing:

* Persistent storage
* SQLite integration
* Request tracing
* Metrics endpoint
* JWT authentication
* Pagination
* Docker Compose setup

---

# Learning Outcomes

After completing this mini project, you should be able to:

* Structure production-grade Go applications
* Build REST APIs
* Apply Go best practices
* Write maintainable backend services
* Handle production concerns properly
* Implement testing and observability basics

---

# Final Thoughts

This project is intentionally designed to simulate a small real-world backend service.

The goal is not perfection.

The goal is to:

* Practice engineering discipline
* Apply Go concepts practically
* Learn maintainable software design
* Think like a backend engineer

Production-ready software is built through:

* Simplicity
* Consistency
* Testing
* Observability
* Continuous improvement

Continue building projects and refining your engineering skills.
