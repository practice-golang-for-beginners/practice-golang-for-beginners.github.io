---
layout: default
title: Test
parent: Chapter 8 – Production Ready Go
nav_order: 2
---

# Test

This test evaluates your understanding of production-ready Go development concepts covered in this chapter.

The focus is on:
- Project structure
- Configuration management
- Logging
- Error handling
- Graceful shutdown
- Testing
- Benchmarking
- Profiling
- Observability
- Security
- CI/CD awareness

---

# Instructions

- Answer all questions.
- Write clean and readable code where applicable.
- Follow idiomatic Go practices.
- Prefer simplicity and clarity over unnecessary complexity.
- Time Suggestion: 90 Minutes

---

# Section 1 – Theory Questions

## Question 1

What is the purpose of the `internal/` directory in Go projects?

### Options

A. To store external dependencies  
B. To expose public APIs  
C. To restrict package visibility within the module  
D. To store compiled binaries

---

## Question 2

Why should configuration values not be hardcoded in production applications?

---

## Question 3

What is the difference between logging and debugging?

---

## Question 4

What are the benefits of graceful shutdown in backend services?

---

## Question 5

Explain the purpose of the following files:

- `go.mod`
- `go.sum`

---

# Section 2 – Error Handling

## Question 6

Identify the issue in the following code:

```go id="e2y2lx"
func ReadConfig() {
    data, err := os.ReadFile("config.json")

    if err != nil {
        panic(err)
    }

    fmt.Println(string(data))
}
````

### Tasks

1. Explain why this implementation is not production friendly.
2. Rewrite the function using proper error handling.

---

## Question 7

Why is the following approach preferred?

```go id="mq7m0p"
return fmt.Errorf("failed to load configuration: %w", err)
```

---

# Section 3 – Logging and Configuration

## Question 8

Write a Go program that:

* Reads an environment variable named `APP_PORT`
* Logs the value using the standard `log` package
* Prints a default value if the environment variable is not set

---

## Question 9

What are the advantages of structured logging?

---

# Section 4 – Testing

## Question 10

Write a unit test for the following function:

```go id="zwu9pw"
func Multiply(a int, b int) int {
    return a * b
}
```

---

## Question 11

What characteristics make a test reliable and maintainable?

---

## Question 12

What is the purpose of table-driven testing in Go?

---

# Section 5 – Benchmarking and Profiling

## Question 13

Write a benchmark function for the following method:

```go id="l8u0hq"
func ProcessData() {
    for i := 0; i < 1000; i++ {
    }
}
```

---

## Question 14

What problems can profiling help identify in Go applications?

---

## Question 15

Which command is used to execute benchmarks?

### Options

A.

```bash id="4w4i9h"
go test
```

B.

```bash id="uwalpg"
go benchmark
```

C.

```bash id="zq6c9e"
go test -bench=.
```

D.

```bash id="m6w4fk"
go run benchmark.go
```

---

# Section 6 – Production Readiness

## Question 16

What is observability in distributed systems?

---

## Question 17

List three security best practices for Go backend services.

---

## Question 18

What is the purpose of CI/CD pipelines?

---

## Question 19

Why is `go fmt` considered important in Go projects?

---

## Question 20

Explain the following Go philosophy:

> "Clear is better than clever."

---

# Bonus Challenge

Design a high-level architecture for a production-ready Go REST API.

Your answer should include:

* Project structure
* Logging strategy
* Configuration management
* Testing approach
* Deployment considerations

You may use diagrams or bullet points.

---

# Submission Guidelines

Before submitting:

* Ensure code compiles successfully
* Run:

```bash id="hcb4s7"
go test ./...
```

* Format code:

```bash id="zdr9m0"
go fmt ./...
```

* Ensure all answers are clear and readable

---

# Evaluation Criteria

You will be evaluated on:

* Correctness
* Code readability
* Idiomatic Go usage
* Error handling
* Testing practices
* Production awareness
* Simplicity and maintainability

---

# Completion

Congratulations on reaching the final chapter of the 8-Week Go Mentorship Program.

Continue building:

* Real-world applications
* APIs
* Concurrent systems
* Cloud-native tools

The best way to master Go is through consistent practice and engineering discipline.
