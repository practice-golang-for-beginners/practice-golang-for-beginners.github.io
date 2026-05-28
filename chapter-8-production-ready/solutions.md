---
layout: default
title: Solutions
parent: Chapter 8 – Production Ready Go
nav_order: 5
---

# Solutions

This document contains reference solutions and implementation guidance for the exercises in Chapter 8.

The goal is not only to make the code work, but also to:
- Follow idiomatic Go practices
- Improve maintainability
- Handle errors properly
- Structure applications professionally

---

# Solution 1 – Project Structure Design

## Recommended Structure

```text
project-name/
│
├── cmd/
│   └── app/
│       └── main.go
│
├── internal/
│   ├── config/
│   ├── handler/
│   ├── logger/
│   └── service/
│
├── configs/
│
├── tests/
│
├── go.mod
├── go.sum
└── README.md
````

---

## Explanation

| Directory | Purpose                        |
| --------- | ------------------------------ |
| cmd/      | Application entry points       |
| internal/ | Private application packages   |
| configs/  | Configuration files            |
| tests/    | Integration and external tests |
| go.mod    | Dependency definition          |
| go.sum    | Dependency checksum validation |

---

# Solution 2 – Environment Variable Configuration

## Example

```go id="mflrll"
package main

import (
    "fmt"
    "log"
    "os"
)

func getEnv(key string, fallback string) string {
    value := os.Getenv(key)

    if value == "" {
        return fallback
    }

    return value
}

func main() {
    appName := getEnv("APP_NAME", "demo-service")
    port := getEnv("APP_PORT", "8080")
    dbHost := getEnv("DB_HOST", "localhost")

    log.Println("Application Starting")

    fmt.Println("Application:", appName)
    fmt.Println("Port:", port)
    fmt.Println("Database Host:", dbHost)
}
```

---

# Solution 3 – Structured Logging

## Example Logging Middleware

```go id="n4kh7h"
package main

import (
    "log"
    "net/http"
    "time"
)

func loggingMiddleware(next http.Handler) http.Handler {
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        start := time.Now()

        next.ServeHTTP(w, r)

        log.Printf(
            "method=%s path=%s duration=%s",
            r.Method,
            r.URL.Path,
            time.Since(start),
        )
    })
}
```

---

# Solution 4 – Improved Error Handling

## Original Problem

The original implementation used:

```go id="6i1u6t"
panic(err)
```

This is not suitable for normal application flow.

---

## Improved Version

```go id="v2rkzi"
package main

import (
    "fmt"
    "os"
)

func LoadFile() (string, error) {
    data, err := os.ReadFile("data.txt")

    if err != nil {
        return "", fmt.Errorf("failed to read file: %w", err)
    }

    return string(data), nil
}
```

---

# Solution 5 – Graceful Shutdown

## Example

```go id="cly9x5"
package main

import (
    "context"
    "log"
    "net/http"
    "os"
    "os/signal"
    "time"
)

func main() {
    mux := http.NewServeMux()

    mux.HandleFunc("/health", func(w http.ResponseWriter, r *http.Request) {
        w.Write([]byte("OK"))
    })

    server := &http.Server{
        Addr:    ":8080",
        Handler: mux,
    }

    go func() {
        log.Println("Server started on :8080")

        if err := server.ListenAndServe(); err != nil && err != http.ErrServerClosed {
            log.Fatal(err)
        }
    }()

    ctx, stop := signal.NotifyContext(context.Background(), os.Interrupt)
    defer stop()

    <-ctx.Done()

    log.Println("Shutting down server...")

    shutdownCtx, cancel := context.WithTimeout(context.Background(), 5*time.Second)
    defer cancel()

    if err := server.Shutdown(shutdownCtx); err != nil {
        log.Fatal(err)
    }

    log.Println("Server exited cleanly")
}
```

---

# Solution 6 – Unit Testing

## Function

```go id="gwltc4"
package mathutil

import "errors"

func Divide(a int, b int) (int, error) {
    if b == 0 {
        return 0, errors.New("division by zero")
    }

    return a / b, nil
}
```

---

## Unit Test

```go id="31m1l0"
package mathutil

import "testing"

func TestDivide(t *testing.T) {
    result, err := Divide(10, 2)

    if err != nil {
        t.Fatalf("unexpected error: %v", err)
    }

    if result != 5 {
        t.Fatalf("expected 5, got %d", result)
    }
}
```

---

## Table-Driven Test

```go id="gx6h0x"
func TestDivideTable(t *testing.T) {
    tests := []struct {
        a        int
        b        int
        expected int
    }{
        {10, 2, 5},
        {20, 4, 5},
        {9, 3, 3},
    }

    for _, tt := range tests {
        result, _ := Divide(tt.a, tt.b)

        if result != tt.expected {
            t.Errorf("expected %d, got %d", tt.expected, result)
        }
    }
}
```

---

# Solution 7 – Benchmark Testing

## Example

```go id="j8t43t"
package benchmark

func ProcessNumbers(numbers []int) int {
    total := 0

    for _, n := range numbers {
        total += n
    }

    return total
}
```

---

## Benchmark

```go id="38bzmr"
package benchmark

import "testing"

func BenchmarkProcessNumbers(b *testing.B) {
    numbers := make([]int, 10000)

    for i := 0; i < b.N; i++ {
        ProcessNumbers(numbers)
    }
}
```

---

## Run Benchmark

```bash id="y3gns5"
go test -bench=.
```

---

# Solution 8 – Profiling

## CPU Profiling Example

```go id="ryjlwm"
package main

func HeavyComputation() {
    total := 0

    for i := 0; i < 100000000; i++ {
        total += i
    }
}
```

---

## Generate Profile

```bash id="5p32i4"
go test -cpuprofile=cpu.prof
```

---

## Analyze Profile

```bash id="f1py2v"
go tool pprof cpu.prof
```

---

# Solution 9 – GitHub Actions CI Pipeline

## Example Workflow

```yaml id="wgbwn2"
name: Go CI

on:
  push:
  pull_request:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Setup Go
        uses: actions/setup-go@v5
        with:
          go-version: '1.24'

      - name: Install Dependencies
        run: go mod tidy

      - name: Run Tests
        run: go test ./...

      - name: Format Check
        run: go fmt ./...
```

---

# Solution 10 – Production Readiness Checklist

## Example Checklist

```text
✔ Configuration via environment variables
✔ Structured logging enabled
✔ Graceful shutdown implemented
✔ Unit tests passing
✔ Health endpoint available
✔ Error handling implemented
✔ go fmt executed
✔ Benchmarks reviewed
✔ Dependencies validated
✔ Documentation updated
```

---

# Mini Challenge Reference Architecture

## Suggested Layout

```text
task-service/
│
├── cmd/
│   └── server/
│
├── internal/
│   ├── config/
│   ├── handler/
│   ├── logger/
│   ├── service/
│   └── repository/
│
├── tests/
│
├── Dockerfile
├── go.mod
└── README.md
```

---

# Example REST API Endpoints

| Method | Endpoint | Description  |
| ------ | -------- | ------------ |
| GET    | /tasks   | List tasks   |
| POST   | /tasks   | Create task  |
| GET    | /health  | Health check |

---

# Best Practices Summary

## Recommended Practices

* Keep interfaces small
* Return errors explicitly
* Avoid unnecessary abstractions
* Prefer composition over inheritance
* Write tests early
* Keep packages focused

---

## Avoid

* Excessive global variables
* Overengineering
* Panic-based control flow
* Java-style class hierarchies
* Deep package nesting

---

# Production Engineering Mindset

Production-ready software should be:

* Reliable
* Observable
* Testable
* Maintainable
* Secure

The best Go applications are usually:

* Simple
* Explicit
* Easy to read
* Easy to debug

---

# Final Notes

Completing this chapter means you now understand:

* Core Go engineering practices
* Backend service development
* Production-readiness concepts
* Testing and observability fundamentals

Continue practicing by:

* Building APIs
* Writing concurrent systems
* Exploring Kubernetes tooling
* Studying open-source Go projects

Mastery comes from consistent real-world implementation and continuous learning.
