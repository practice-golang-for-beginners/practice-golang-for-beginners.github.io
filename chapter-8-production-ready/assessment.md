---
layout: default
title: Assessment
parent: Chapter 8 – Production Ready Go
nav_order: 4
---

# Assessment

This assessment evaluates your ability to apply production-ready Go development concepts in realistic engineering scenarios.

The assessment focuses on:
- Application structure
- Configuration management
- Logging
- Error handling
- Graceful shutdown
- Testing
- Benchmarking
- Profiling
- CI/CD awareness
- Maintainability

---

# Assessment Objectives

By completing this assessment, you should demonstrate the ability to:

- Build maintainable Go applications
- Write idiomatic Go code
- Structure backend services professionally
- Implement testing and benchmarking
- Handle errors properly
- Apply production engineering practices

---

# Assessment Format

| Section | Topic | Marks |
|---|---|---|
| Section 1 | Theory and Concepts | 20 |
| Section 2 | Code Review and Refactoring | 20 |
| Section 3 | Implementation Tasks | 40 |
| Section 4 | Production Readiness Evaluation | 20 |

Total Marks: **100**

---

# Section 1 – Theory and Concepts

## Question 1

Explain why Go applications commonly use the following directories:

```text
cmd/
internal/
pkg/
configs/
````

---

## Question 2

What are the advantages of:

* Environment variables
* Structured logging
* Graceful shutdown

in production systems?

---

## Question 3

What is observability?

Explain the difference between:

* Logs
* Metrics
* Traces

---

## Question 4

Why are small interfaces preferred in Go?

---

# Section 2 – Code Review and Refactoring

## Question 5

Review the following code:

```go id="jngb9q"
package main

import (
    "fmt"
    "os"
)

func main() {
    port := "8080"

    if os.Getenv("APP_PORT") != "" {
        port = os.Getenv("APP_PORT")
    }

    fmt.Println("Starting server on port", port)

    panic("server crashed")
}
```

## Tasks

1. Identify problems in the implementation.
2. Refactor the code using:

   * Logging
   * Proper error handling
   * Configuration management
3. Explain your improvements.

---

## Question 6

Review the following function:

```go id="1y8x8g"
func FetchData() string {
    data, _ := os.ReadFile("data.txt")
    return string(data)
}
```

## Tasks

1. Identify the issue.
2. Rewrite using proper production-quality error handling.
3. Add contextual errors.

---

# Section 3 – Implementation Tasks

## Question 7 – Configuration Loader

### Objective

Create a reusable configuration package.

### Requirements

* Read:

  * APP_NAME
  * APP_PORT
  * DB_HOST
* Provide default values
* Validate required fields

### Deliverables

```text
internal/config/
```

---

## Question 8 – Structured Logger

### Objective

Create a reusable logger package.

### Requirements

* Log:

  * Timestamp
  * Severity
  * Message
* Add helper functions:

  * Info()
  * Error()

### Deliverables

```text
internal/logger/
```

---

## Question 9 – Graceful HTTP Server

### Objective

Build a production-style HTTP server.

### Requirements

* `/health` endpoint
* Graceful shutdown
* Request logging middleware
* Environment-based port configuration

### Expected Features

* Clean shutdown on interrupt
* Logging during startup and shutdown
* Proper error handling

---

## Question 10 – Unit Testing

### Objective

Write tests for the following function:

```go id="3r1c7w"
func Subtract(a int, b int) int
```

### Requirements

* Unit tests
* Table-driven tests
* Edge-case validation

---

# Section 4 – Production Readiness Evaluation

## Question 11

Create a production readiness checklist for a Go backend service.

Include:

* Logging
* Configuration
* Testing
* Monitoring
* Security
* Deployment readiness

---

## Question 12

Explain how CI/CD improves software reliability.

Include:

* Build automation
* Testing automation
* Deployment consistency

---

# Final Capstone Task

## Objective

Design a small production-ready Go service.

Choose one:

* Task Management API
* Inventory Service
* Notes API
* User Management Service

---

# Requirements

Your project should include:

## Project Structure

```text
cmd/
internal/
configs/
tests/
```

## Features

* REST API
* Logging
* Configuration management
* Graceful shutdown
* Unit tests
* Error handling

## Bonus Features

* Dockerfile
* Health endpoint
* Metrics endpoint
* Middleware support

---

# Evaluation Rubric

| Criteria             | Marks |
| -------------------- | ----- |
| Code Readability     | 20    |
| Go Best Practices    | 20    |
| Error Handling       | 15    |
| Testing Quality      | 15    |
| Project Structure    | 10    |
| Maintainability      | 10    |
| Production Awareness | 10    |

---

# Submission Guidelines

Before submission:

## Format Code

```bash id="0yrjhj"
go fmt ./...
```

## Run Tests

```bash id="ffuk07"
go test ./...
```

## Run Benchmarks

```bash id="uydndd"
go test -bench=.
```

## Optional

Run linting:

```bash id="tmxv14"
golangci-lint run
```

---

# Recommended Deliverables

Your submission should contain:

```text
README.md
go.mod
go.sum
cmd/
internal/
tests/
configs/
```

---

# Assessment Expectations

A successful submission should demonstrate:

* Clear engineering thinking
* Idiomatic Go practices
* Maintainable architecture
* Proper testing discipline
* Production-readiness awareness

The goal is not just to write working code.

The goal is to write software that:

* Other engineers can maintain
* Can safely run in production
* Can evolve over time

---

# Completion

Congratulations on completing the final assessment of the 8-Week Go Mentorship Program.

You now have exposure to:

* Core Go development
* Concurrency
* HTTP services
* Production engineering practices

Continue building real-world projects and exploring:

* Cloud-native Go
* Kubernetes operators
* Distributed systems
* Observability tooling
* Infrastructure automation

Consistent practice and real engineering experience will strengthen your Go expertise over time.
