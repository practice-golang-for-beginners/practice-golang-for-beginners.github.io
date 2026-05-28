---
layout: default
title: Exercises
parent: Chapter 8 – Production Ready Go
nav_order: 3
---

# Exercises

These exercises are designed to strengthen your understanding of:
- Production-grade Go development
- Application structure
- Logging
- Configuration management
- Testing
- Benchmarking
- Profiling
- Observability
- Deployment readiness

Focus on:
- Clean code
- Idiomatic Go
- Maintainability
- Simplicity

---

# Exercise 1 – Project Structure Design

## Objective

Create a production-ready folder structure for a Go REST API application.

## Requirements

Design the following directories:

```text
cmd/
internal/
pkg/
configs/
tests/
````

## Tasks

1. Explain the purpose of each directory.
2. Create sample placeholder files.
3. Add a small README describing the structure.

---

# Exercise 2 – Environment Variable Configuration

## Objective

Learn how to manage application configuration using environment variables.

## Requirements

Write a Go program that:

* Reads:

  * APP_NAME
  * APP_PORT
  * DB_HOST
* Prints default values if variables are missing
* Logs the configuration during startup

## Example Output

```text
Application: demo-service
Port: 8080
Database Host: localhost
```

## Bonus

Add validation for missing mandatory variables.

---

# Exercise 3 – Structured Logging

## Objective

Implement structured logging for an application.

## Requirements

Create a simple HTTP server that logs:

* Request path
* HTTP method
* Timestamp
* Response status code

## Tasks

1. Use the standard `log` package initially.
2. Add middleware for logging.
3. Improve log readability.

## Bonus

Use Go's `slog` package.

---

# Exercise 4 – Error Handling Improvement

## Objective

Refactor poor error handling into production-quality error handling.

## Starter Code

```go id="q1lyj6"
func LoadFile() string {
    data, err := os.ReadFile("data.txt")

    if err != nil {
        panic(err)
    }

    return string(data)
}
```

## Tasks

1. Remove `panic`.
2. Return errors properly.
3. Add contextual error wrapping.
4. Write tests for failure scenarios.

---

# Exercise 5 – Graceful Shutdown

## Objective

Implement graceful shutdown for an HTTP service.

## Requirements

Create an HTTP server that:

* Listens on port 8080
* Handles `/health`
* Shuts down cleanly on `CTRL+C`

## Tasks

1. Use `context.Context`
2. Use `signal.NotifyContext`
3. Add shutdown timeout handling

---

# Exercise 6 – Unit Testing

## Objective

Practice writing maintainable tests.

## Requirements

Create the following function:

```go id="e1x5g7"
func Divide(a int, b int) (int, error)
```

## Tasks

1. Return an error for division by zero.
2. Write unit tests.
3. Add table-driven tests.

## Bonus

Measure code coverage.

---

# Exercise 7 – Benchmark Testing

## Objective

Understand Go benchmark testing.

## Requirements

Create a function that processes a large slice.

Example:

```go id="6xcl2u"
func ProcessNumbers(numbers []int) int
```

## Tasks

1. Write a benchmark test.
2. Run benchmark analysis.
3. Compare performance for:

   * Small input
   * Large input

---

# Exercise 8 – Profiling Practice

## Objective

Learn how profiling helps optimize applications.

## Requirements

Create a CPU-intensive function.

Example:

```go id="ok9xk0"
func HeavyComputation()
```

## Tasks

1. Generate CPU load.
2. Run Go profiling tools.
3. Analyze bottlenecks.
4. Document findings.

---

# Exercise 9 – CI/CD Awareness

## Objective

Understand basic CI/CD workflows.

## Requirements

Create a simple GitHub Actions workflow that:

* Builds the application
* Runs tests
* Runs formatting checks

## Suggested File

```text
.github/workflows/go.yml
```

## Bonus

Add linting support.

---

# Exercise 10 – Production Readiness Checklist

## Objective

Create a checklist for evaluating Go services before deployment.

## Include

* Logging
* Error handling
* Configuration management
* Health endpoints
* Graceful shutdown
* Testing
* Security
* Documentation

## Deliverable

Create:

```text
production-checklist.md
```

---

# Mini Challenge – Build a Production Ready Service

## Objective

Combine all concepts learned in this chapter.

## Requirements

Build a small REST API service that includes:

* Structured project layout
* Configuration management
* Logging
* Error handling
* Graceful shutdown
* Unit tests

## Suggested APIs

Choose one:

* Task Manager
* Notes Service
* Inventory Service
* User Service

## Bonus Features

* Docker support
* Health check endpoint
* Metrics endpoint
* Request middleware

---

# Submission Guidelines

Before submission:

## Format Code

```bash id="a8u6uy"
go fmt ./...
```

## Run Tests

```bash id="u8f6tf"
go test ./...
```

## Run Benchmarks

```bash id="z4w4kh"
go test -bench=.
```

---

# Learning Goals

After completing these exercises, you should be able to:

* Structure production-ready Go applications
* Write maintainable and testable code
* Handle errors properly
* Use logging effectively
* Build reliable backend services
* Understand performance optimization basics
* Prepare applications for deployment

---

# Recommended Practice

Do not stop after completing the exercises.

Try:

* Refactoring your older code
* Building larger APIs
* Adding middleware
* Improving observability
* Exploring cloud-native Go development

Production readiness comes from consistent engineering discipline and real-world practice.
