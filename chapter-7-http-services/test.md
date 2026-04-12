---
layout: default
title: Testing HTTP Services
parent: Chapter 7 – HTTP Services
nav_order: 2
---

# 🧪 Testing HTTP Services in Go

Testing is a **first-class citizen in Go**. The standard library provides powerful tools to test HTTP handlers without needing external frameworks.

In this section, you will learn how to write **clean, reliable, and idiomatic tests** for HTTP services.

---

## 🎯 Learning Objectives

By the end of this section, you will be able to:

* Test HTTP handlers using `net/http/httptest`
* Validate response status codes and bodies
* Write table-driven tests
* Mock dependencies using interfaces
* Test JSON APIs effectively

---

## 🧰 1. Introduction to `httptest`

The `httptest` package allows you to simulate HTTP requests and record responses.

### Basic Example

```go id="t1basic"
package main

import (
	"net/http"
	"net/http/httptest"
	"testing"
)

func TestHelloHandler(t *testing.T) {
	req := httptest.NewRequest(http.MethodGet, "/hello", nil)
	rec := httptest.NewRecorder()

	helloHandler(rec, req)

	if rec.Code != http.StatusOK {
		t.Errorf("expected status 200, got %d", rec.Code)
	}

	expected := "Hello, World!"
	if rec.Body.String() != expected {
		t.Errorf("expected body %q, got %q", expected, rec.Body.String())
	}
}
```

---

## 📊 2. Validating Response

Always validate:

* Status code
* Response body
* Headers (if applicable)

### Example

```go id="t2resp"
if rec.Header().Get("Content-Type") != "application/json" {
	t.Errorf("expected application/json header")
}
```

---

## 🔁 3. Table-Driven Tests

Table-driven tests are idiomatic in Go and allow multiple test cases.

### Example

```go id="t3table"
func TestHelloHandler_TableDriven(t *testing.T) {
	tests := []struct {
		name       string
		method     string
		expectedCode int
	}{
		{"Valid GET", http.MethodGet, http.StatusOK},
		{"Invalid POST", http.MethodPost, http.StatusMethodNotAllowed},
	}

	for _, tt := range tests {
		t.Run(tt.name, func(t *testing.T) {
			req := httptest.NewRequest(tt.method, "/hello", nil)
			rec := httptest.NewRecorder()

			helloHandler(rec, req)

			if rec.Code != tt.expectedCode {
				t.Errorf("expected %d, got %d", tt.expectedCode, rec.Code)
			}
		})
	}
}
```

---

## 🧩 4. Testing JSON APIs

### Example Handler

```go id="t4handler"
type User struct {
	Name string `json:"name"`
}

func userHandler(w http.ResponseWriter, r *http.Request) {
	user := User{Name: "Aditya"}
	w.Header().Set("Content-Type", "application/json")
	json.NewEncoder(w).Encode(user)
}
```

### Test

```go id="t4test"
func TestUserHandler(t *testing.T) {
	req := httptest.NewRequest(http.MethodGet, "/user", nil)
	rec := httptest.NewRecorder()

	userHandler(rec, req)

	var user User
	err := json.Unmarshal(rec.Body.Bytes(), &user)
	if err != nil {
		t.Fatalf("failed to parse response: %v", err)
	}

	if user.Name != "Aditya" {
		t.Errorf("expected Aditya, got %s", user.Name)
	}
}
```

---

## 🔌 5. Mocking Dependencies with Interfaces

To make handlers testable, avoid tight coupling.

### Example Interface

```go id="t5interface"
type UserService interface {
	GetUser() string
}
```

### Mock Implementation

```go id="t5mock"
type MockUserService struct{}

func (m MockUserService) GetUser() string {
	return "MockUser"
}
```

### Inject into Handler

```go id="t5handler"
func userHandler(service UserService) http.HandlerFunc {
	return func(w http.ResponseWriter, r *http.Request) {
		name := service.GetUser()
		w.Write([]byte(name))
	}
}
```

### Test with Mock

```go id="t5test"
func TestUserHandler_WithMock(t *testing.T) {
	mock := MockUserService{}
	handler := userHandler(mock)

	req := httptest.NewRequest(http.MethodGet, "/user", nil)
	rec := httptest.NewRecorder()

	handler(rec, req)

	if rec.Body.String() != "MockUser" {
		t.Errorf("unexpected response")
	}
}
```

---

## ⚠️ 6. Common Testing Mistakes

* Not testing error scenarios
* Ignoring headers
* Writing overly complex tests
* Not using table-driven approach
* Mixing business logic with HTTP layer

---

## 🧠 Best Practices

* Keep tests **small and focused**
* Test both **success and failure paths**
* Use **interfaces for mocking**
* Prefer **table-driven tests**
* Keep handlers thin and logic separate

---

## 🚀 Summary

You now know how to:

* Test HTTP handlers using `httptest`
* Validate responses effectively
* Write scalable table-driven tests
* Mock dependencies for isolation

---

## 📌 Key Insight

> “If your Go code is hard to test, it’s probably not well designed.”

Testing is not just validation—it’s a **design tool**.

---

