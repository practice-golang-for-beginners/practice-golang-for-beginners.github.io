---
layout: default
title: Tests
parent: Chapter 2 – Structs & Functions
nav_order: 5
---



**Structs, Functions & Packages**

These tests are written in a **clear, idiomatic Go** style using table-driven tests, proper assertions, and no unnecessary output.
They should guide learners to test their implementations correctly.

---

## 📦 Test Setup

Before writing tests, make sure your functions and types are in appropriate packages.
Example project layout for chapter:

```
chapter-2-structs-functions/
│
├── user.go
├── user_test.go
├── mathutil.go
├── mathutil_test.go
├── ...
```

---

## 🧠 Tests for Functions

### 1️⃣ Test Divide Function

```go
package mathutil

import "testing"

func TestDivide(t *testing.T) {
    tests := []struct {
        name     string
        a, b     int
        want     int
        wantErr  bool
    }{
        {"normal division", 10, 2, 5, false},
        {"division by zero", 10, 0, 0, true},
    }

    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            got, err := Divide(tt.a, tt.b)
            if (err != nil) != tt.wantErr {
                t.Errorf("Divide() error = %v, wantErr %v", err, tt.wantErr)
            }
            if got != tt.want {
                t.Errorf("Divide() got = %v, want %v", got, tt.want)
            }
        })
    }
}
```

---

## 🧱 Tests for Structs & Methods

### 2️⃣ Test Value & Pointer Receiver Behavior

```go
package user

import "testing"

func TestUpdateName(t *testing.T) {
    user := User{ID: 1, Name: "Alice"}

    user.UpdateName("Bob")
    if user.Name != "Bob" {
        t.Errorf("expected name to be Bob, got %s", user.Name)
    }
}

func TestUpdateEmailPointer(t *testing.T) {
    user := &User{ID: 1, Name: "Alice", Email: "a@example.com"}

    user.UpdateEmail("bob@example.com")
    if user.Email != "bob@example.com" {
        t.Errorf("expected email to be updated, got %s", user.Email)
    }
}
```

---

## 🧬 Tests for Composition

### 3️⃣ Embed Struct Behavior

```go
package customer

import (
    "testing"
)

func TestCustomerAddressEmbedding(t *testing.T) {
    c := Customer{
        Name: "Test",
        Address: Address{
            City:    "Bangalore",
            Country: "India",
        },
    }

    if c.City != "Bangalore" || c.Country != "India" {
        t.Errorf("Expected embedded fields to be accessible directly")
    }
}
```

---

## 🧪 Tests for Export Rules

### 4️⃣ Visibility & Package Export

```go
package helper

import "testing"

func TestExportedFunc(t *testing.T) {
    got := ExportedFunc()
    if got != "hello" {
        t.Errorf("ExportedFunc() = %q, want %q", got, "hello")
    }
}

// This test *should* fail to compile if you try to reference an unexported symbol
// Uncomment to verify:
// func TestUnexportedFunc(t *testing.T) {
//     _ = unexportedFunc()  // should not compile
// }
```

---

## 📂 Tests for Package Structure

### 5️⃣ Creating Instances from Another Package

```go
package main

import (
    "testing"
    "yourmodule/user"
)

func TestNewUserInstance(t *testing.T) {
    u := user.New("1", "Alice", "a@example.com")
    if u.Name != "Alice" {
        t.Errorf("expected user name Alice, got %s", u.Name)
    }
}
```

> Adjust `"yourmodule/user"` to match your actual module path.

---

## 📦 Bonus: Table-Driven Test for Multiple Struct Behaviors

```go
package user

import "testing"

func TestMultipleUserUpdates(t *testing.T) {
    tests := []struct {
        name        string
        initialName string
        updateEmail string
        wantEmail   string
    }{
        {"update only email", "Alice", "alice@x.com", "alice@x.com"},
        {"no change to email", "Bob", "", ""},
    }

    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            u := &User{Name: tt.initialName, Email: ""}
            u.UpdateEmail(tt.updateEmail)
            if u.Email != tt.wantEmail {
                t.Errorf("email = %s, want %s", u.Email, tt.wantEmail)
            }
        })
    }
}
```

---

## ✅ Test Best Practices Covered

✔ Table-driven tests
✔ Clear assertion messages
✔ Minimal duplication
✔ Separation of responsibilities
✔ No reliance on global state

---

## 🔍 Tips for Learners

* Run tests with `go test ./...`
* Use `go test -cover` to see coverage
* Use `go test -failfast` to stop on first failing test
* Write tests **before** writing implementation (TDD mindset)

---

