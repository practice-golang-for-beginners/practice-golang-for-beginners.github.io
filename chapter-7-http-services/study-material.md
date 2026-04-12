---
layout: default
title: Study Material
parent: Chapter 7 – HTTP Services
nav_order: 1
---

# 📖 Study Material – HTTP Services in Go

In this chapter, we move into **real-world backend development** using Go. The standard library provides everything needed to build robust HTTP services without heavy frameworks.

---

## 🌐 1. Understanding `net/http`

Go’s `net/http` package is minimal, powerful, and production-ready.

### Basic HTTP Server

```go
package main

import (
	"fmt"
	"net/http"
)

func helloHandler(w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "Hello, World!")
}

func main() {
	http.HandleFunc("/hello", helloHandler)
	http.ListenAndServe(":8080", nil)
}
```

### Key Concepts

* `http.ResponseWriter` → used to write response
* `*http.Request` → contains request data
* `HandleFunc` → maps route to handler

---

## 🧩 2. Handlers and Routing

In Go, a handler is anything that implements:

```go
type Handler interface {
	ServeHTTP(ResponseWriter, *Request)
}
```

### Using Custom Handlers

```go
type HelloHandler struct{}

func (h HelloHandler) ServeHTTP(w http.ResponseWriter, r *http.Request) {
	w.Write([]byte("Hello from custom handler"))
}
```

### Routing Options

* Default: `http.HandleFunc`
* Custom multiplexer: `http.NewServeMux()`

```go
mux := http.NewServeMux()
mux.HandleFunc("/hello", helloHandler)
http.ListenAndServe(":8080", mux)
```

---

## 🔄 3. Working with JSON

JSON is the backbone of most APIs.

### Encoding JSON

```go
type User struct {
	Name string `json:"name"`
	Age  int    `json:"age"`
}

func handler(w http.ResponseWriter, r *http.Request) {
	user := User{Name: "Aditya", Age: 30}
	w.Header().Set("Content-Type", "application/json")
	json.NewEncoder(w).Encode(user)
}
```

### Decoding JSON

```go
var user User
err := json.NewDecoder(r.Body).Decode(&user)
if err != nil {
	http.Error(w, "Invalid request", http.StatusBadRequest)
	return
}
```

---

## 🧱 4. Middleware Patterns

Middleware wraps handlers to add cross-cutting concerns.

### Example: Logging Middleware

```go
func loggingMiddleware(next http.Handler) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		fmt.Println("Request:", r.Method, r.URL.Path)
		next.ServeHTTP(w, r)
	})
}
```

### Applying Middleware

```go
mux := http.NewServeMux()
mux.Handle("/hello", loggingMiddleware(http.HandlerFunc(helloHandler)))
```

---

## 📊 5. HTTP Status Codes & Responses

Always return meaningful status codes:

| Scenario       | Status Code |
| -------------- | ----------- |
| Success        | 200 OK      |
| Created        | 201 Created |
| Bad Request    | 400         |
| Not Found      | 404         |
| Internal Error | 500         |

### Example

```go
http.Error(w, "Resource not found", http.StatusNotFound)
```

---

## 🧵 6. Request Context

Context is critical for production systems.

```go
ctx := r.Context()
```

Use it for:

* Timeouts
* Cancellation
* Passing request-scoped data

---

## ⏱️ 7. Timeouts & Server Configuration

Never run production servers without timeouts.

```go
server := &http.Server{
	Addr:         ":8080",
	Handler:      mux,
	ReadTimeout:  5 * time.Second,
	WriteTimeout: 10 * time.Second,
}

server.ListenAndServe()
```

---

## 🛑 8. Graceful Shutdown

Handle shutdown properly:

```go
ctx, stop := signal.NotifyContext(context.Background(), os.Interrupt)
defer stop()

go server.ListenAndServe()

<-ctx.Done()
server.Shutdown(context.Background())
```

---

## 🧪 9. Writing Testable Handlers

Design handlers to be testable:

* Avoid global state
* Use dependency injection
* Keep logic separate from HTTP layer

---

## 🧠 10. Idiomatic Go Practices

* Keep handlers **small and focused**
* Avoid over-engineering (no heavy frameworks)
* Prefer composition over inheritance
* Return errors explicitly
* Use interfaces for testability

---

## ⚠️ Common Mistakes (Especially for Python Developers)

* Trying to use frameworks like Django/Flask mindset
* Ignoring error handling
* Overusing global variables
* Not setting headers properly
* Writing large, untestable handlers

---

## 🚀 Summary

In this chapter, you learned how to:

* Build HTTP servers using Go
* Handle requests and responses
* Work with JSON
* Implement middleware
* Configure production-ready servers
* Write clean and testable HTTP services

---

## 📌 Key Insight

Go’s philosophy is:

> “Simple, explicit, and production-ready by default”

Mastering `net/http` means you can build **high-performance backend systems without external dependencies**.

---

