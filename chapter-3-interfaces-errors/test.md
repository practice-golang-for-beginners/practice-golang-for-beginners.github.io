---
layout: default
title: Chapter 3 - Tests
parent: Chapter 3 – Interfaces & Errors
nav_order: 5
---


Perfect — let’s elevate the testing quality.

Below is a **clean, idiomatic, production-style unit test suite** for Week 3 concepts.
This reflects:

* Table-driven tests
* Proper error checking (`errors.Is`, `errors.As`)
* No string comparison for errors
* Clean test naming
* No unnecessary logging
* Focused assertions


---

# 🧪 service.go (Reference Implementation Under Test)

```go
package service

import (
	"errors"
	"fmt"
)

var ErrStorageFailure = errors.New("storage failure")

type Storage interface {
	Save(data string) error
}

type ValidationError struct {
	Field   string
	Message string
}

func (v ValidationError) Error() string {
	return fmt.Sprintf("validation failed on %s: %s", v.Field, v.Message)
}

type UserService struct {
	storage Storage
}

func NewUserService(s Storage) *UserService {
	return &UserService{storage: s}
}

func (u *UserService) CreateUser(name string) error {
	if name == "" {
		return ValidationError{
			Field:   "name",
			Message: "cannot be empty",
		}
	}

	if err := u.storage.Save(name); err != nil {
		return fmt.Errorf("failed to create user: %w", err)
	}

	return nil
}
```

---

# 🧪 service_test.go (Clean, Idiomatic Tests)

```go
package service

import (
	"errors"
	"testing"
)
```

---

## 🔹 Mock Storage (Clean & Minimal)

```go
type mockStorage struct {
	shouldFail bool
}

func (m *mockStorage) Save(data string) error {
	if m.shouldFail {
		return ErrStorageFailure
	}
	return nil
}
```

---

# 1️⃣ Test Success Path

```go
func TestCreateUser_Success(t *testing.T) {
	storage := &mockStorage{shouldFail: false}
	service := NewUserService(storage)

	err := service.CreateUser("Aditya")
	if err != nil {
		t.Fatalf("expected no error, got %v", err)
	}
}
```

Why this is clean:

* No unnecessary checks
* Fails fast
* Clear intent

---

# 2️⃣ Test Validation Error (Using errors.As)

```go
func TestCreateUser_ValidationError(t *testing.T) {
	storage := &mockStorage{}
	service := NewUserService(storage)

	err := service.CreateUser("")
	if err == nil {
		t.Fatal("expected validation error, got nil")
	}

	var ve ValidationError
	if !errors.As(err, &ve) {
		t.Fatalf("expected ValidationError, got %T", err)
	}

	if ve.Field != "name" {
		t.Fatalf("expected field 'name', got %s", ve.Field)
	}
}
```

Why this is idiomatic:

* Uses `errors.As`
* Does not compare strings
* Verifies structured data

---

# 3️⃣ Test Wrapped Storage Error (Using errors.Is)

```go
func TestCreateUser_StorageFailure(t *testing.T) {
	storage := &mockStorage{shouldFail: true}
	service := NewUserService(storage)

	err := service.CreateUser("Aditya")
	if err == nil {
		t.Fatal("expected error, got nil")
	}

	if !errors.Is(err, ErrStorageFailure) {
		t.Fatalf("expected wrapped ErrStorageFailure, got %v", err)
	}
}
```

Why this is correct:

* Uses `errors.Is`
* Does not rely on error string
* Validates wrapping behavior

---

# 4️⃣ Table-Driven Test (Professional Pattern)

```go
func TestCreateUser_TableDriven(t *testing.T) {
	tests := []struct {
		name        string
		input       string
		shouldFail  bool
		expectError bool
	}{
		{
			name:        "valid user",
			input:       "Aditya",
			shouldFail:  false,
			expectError: false,
		},
		{
			name:        "empty name",
			input:       "",
			shouldFail:  false,
			expectError: true,
		},
		{
			name:        "storage failure",
			input:       "Aditya",
			shouldFail:  true,
			expectError: true,
		},
	}

	for _, tt := range tests {
		t.Run(tt.name, func(t *testing.T) {
			storage := &mockStorage{shouldFail: tt.shouldFail}
			service := NewUserService(storage)

			err := service.CreateUser(tt.input)

			if tt.expectError && err == nil {
				t.Fatal("expected error, got nil")
			}

			if !tt.expectError && err != nil {
				t.Fatalf("unexpected error: %v", err)
			}
		})
	}
}
```

Why this is senior-level:

* Scalable
* Clean test names
* No duplicated logic
* Clear scenario separation

---

# 🧠 What Makes These Tests “Clean”

### ✔ Small, focused

Each test verifies one behavior.

### ✔ No string comparison of errors

We use:

* `errors.Is`
* `errors.As`

### ✔ No unnecessary logging

Only fail when necessary.

### ✔ No shared mutable global state

### ✔ Proper naming convention

`TestFunction_Scenario`

---

# 🎯 What Student Learns From This

* Interface-based mocking
* Structured error testing
* Proper use of `errors.Is` and `errors.As`
* Table-driven test design
* Writing maintainable tests

This is exactly how Go is tested in real-world backend systems.

---

