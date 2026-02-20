---
layout: default
title: Detailed Solutions
parent: Chapter 3 – Interfaces & Errors
nav_order: 4
---


**Interfaces & Error Handling**

---

# 🟢 Level 1 – Foundations

---

## 1️⃣ Printer Interface

```go
package printer

import "fmt"

type Printer interface {
	Print(message string) error
}

type ConsolePrinter struct{}

func (c ConsolePrinter) Print(message string) error {
	fmt.Println(message)
	return nil
}
```

---

## 2️⃣ Compile-Time Interface Check

```go
var _ Printer = (*ConsolePrinter)(nil)
```

This ensures at compile time that `ConsolePrinter` satisfies `Printer`.

Explanation:

* `(*ConsolePrinter)(nil)` creates a nil pointer of that type.
* If method signatures don’t match, compilation fails.
* It prevents silent interface mismatches.

---

## 3️⃣ Small Interface Refactor

Bad:

```go
type UserService interface {
	Create()
	Update()
	Delete()
	Find()
}
```

Better:

```go
type Creator interface {
	Create() error
}

type Updater interface {
	Update() error
}

type Deleter interface {
	Delete() error
}

type Finder interface {
	Find() (string, error)
}
```

Why better?

* Smaller contracts
* Easier to mock
* More flexible composition

---

## 4️⃣ Python Analogy

Go interfaces resemble Python duck typing because types are compatible based on behavior, not inheritance.

Difference:

* Python checks at runtime.
* Go verifies compatibility at compile time.

This provides safety without sacrificing flexibility.

---

# 🟡 Level 2 – Error Handling

---

## 5️⃣ Divide Function

```go
package mathutil

import "errors"

func Divide(a, b int) (int, error) {
	if b == 0 {
		return 0, errors.New("division by zero")
	}
	return a / b, nil
}
```

---

## 6️⃣ Sentinel Error

```go
package user

import "errors"

var ErrUserNotFound = errors.New("user not found")

func FindUser(id string) (string, error) {
	if id != "123" {
		return "", ErrUserNotFound
	}
	return "Aditya", nil
}
```

---

## 7️⃣ Using errors.Is

```go
result, err := FindUser("999")
if err != nil {
	if errors.Is(err, ErrUserNotFound) {
		fmt.Println("User does not exist")
	}
}
```

---

## 8️⃣ Wrapping Errors

```go
func FindUser(id string) (string, error) {
	if id != "123" {
		return "", fmt.Errorf("database lookup failed: %w", ErrUserNotFound)
	}
	return "Aditya", nil
}
```

Detection:

```go
if errors.Is(err, ErrUserNotFound) {
	// still works
}
```

---

# 🟠 Level 3 – Interfaces + Design

---

## 9️⃣ Storage Interface

```go
package storage

type Storage interface {
	Save(data string) error
	Get(id string) (string, error)
}
```

Implementation:

```go
type InMemoryStorage struct {
	data map[string]string
}

func NewInMemoryStorage() *InMemoryStorage {
	return &InMemoryStorage{
		data: make(map[string]string),
	}
}

func (s *InMemoryStorage) Save(data string) error {
	s.data[data] = data
	return nil
}

func (s *InMemoryStorage) Get(id string) (string, error) {
	val, ok := s.data[id]
	if !ok {
		return "", errors.New("not found")
	}
	return val, nil
}
```

---

## 🔟 Failing Storage

```go
type FailingStorage struct{}

func (f *FailingStorage) Save(data string) error {
	return errors.New("storage failure")
}

func (f *FailingStorage) Get(id string) (string, error) {
	return "", errors.New("storage failure")
}
```

---

## 1️⃣1️⃣ UserService

```go
type UserService struct {
	storage Storage
}

func NewUserService(s Storage) *UserService {
	return &UserService{storage: s}
}

func (u *UserService) CreateUser(name string) error {
	return u.storage.Save(name)
}
```

---

## 1️⃣2️⃣ Error Wrapping

```go
func (u *UserService) CreateUser(name string) error {
	if err := u.storage.Save(name); err != nil {
		return fmt.Errorf("failed to create user: %w", err)
	}
	return nil
}
```

---

# 🔵 Level 4 – Custom Errors

---

## 1️⃣3️⃣ ValidationError

```go
type ValidationError struct {
	Field   string
	Message string
}

func (v ValidationError) Error() string {
	return fmt.Sprintf("validation failed on %s: %s", v.Field, v.Message)
}
```

---

## 1️⃣4️⃣ Use Custom Error

```go
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

## 1️⃣5️⃣ Extract with errors.As

```go
err := service.CreateUser("")

var ve ValidationError
if errors.As(err, &ve) {
	fmt.Println("Invalid field:", ve.Field)
}
```

---

# 🟣 Level 5 – Mocking & Testing

---

## 1️⃣6️⃣ Mock Storage

```go
type MockStorage struct {
	ShouldFail bool
}

func (m *MockStorage) Save(data string) error {
	if m.ShouldFail {
		return errors.New("mock failure")
	}
	return nil
}

func (m *MockStorage) Get(id string) (string, error) {
	return "mock", nil
}
```

---

## 1️⃣7️⃣ Test Success Path

```go
func TestCreateUserSuccess(t *testing.T) {
	mock := &MockStorage{ShouldFail: false}
	service := NewUserService(mock)

	err := service.CreateUser("Aditya")
	if err != nil {
		t.Fatalf("expected no error, got %v", err)
	}
}
```

---

## 1️⃣8️⃣ Test Failure Path

```go
func TestCreateUserFailure(t *testing.T) {
	mock := &MockStorage{ShouldFail: true}
	service := NewUserService(mock)

	err := service.CreateUser("Aditya")
	if err == nil {
		t.Fatal("expected error, got nil")
	}

	if !strings.Contains(err.Error(), "failed to create user") {
		t.Fatal("expected wrapped error")
	}
}
```

---

## 1️⃣9️⃣ Test Validation Error

```go
func TestCreateUserValidationError(t *testing.T) {
	mock := &MockStorage{}
	service := NewUserService(mock)

	err := service.CreateUser("")
	if err == nil {
		t.Fatal("expected error")
	}

	var ve ValidationError
	if !errors.As(err, &ve) {
		t.Fatal("expected ValidationError type")
	}

	if ve.Field != "name" {
		t.Fatalf("expected field name, got %s", ve.Field)
	}
}
```

---

# 🔴 Level 6 – Mini Architecture

---

## 2️⃣0️⃣ Notification System

Interface:

```go
type Notifier interface {
	Send(message string) error
}
```

Implementations:

```go
type EmailNotifier struct{}

func (e EmailNotifier) Send(message string) error {
	return nil
}

type SMSNotifier struct{}

func (s SMSNotifier) Send(message string) error {
	return nil
}
```

Service:

```go
type NotificationService struct {
	notifier Notifier
}

func NewNotificationService(n Notifier) *NotificationService {
	return &NotificationService{notifier: n}
}

func (s *NotificationService) Notify(message string) error {
	if err := s.notifier.Send(message); err != nil {
		return fmt.Errorf("notification failed: %w", err)
	}
	return nil
}
```

Test:

```go
type MockNotifier struct {
	ShouldFail bool
}

func (m MockNotifier) Send(message string) error {
	if m.ShouldFail {
		return errors.New("send failure")
	}
	return nil
}
```

Test example:

```go
func TestNotifyFailure(t *testing.T) {
	mock := MockNotifier{ShouldFail: true}
	service := NewNotificationService(mock)

	err := service.Notify("hello")
	if err == nil {
		t.Fatal("expected error")
	}
}
```

---

# 🎯 Final Outcome

After completing these solutions, student will:

* Design small, clean interfaces
* Understand implicit interface satisfaction
* Handle and wrap errors properly
* Use sentinel errors
* Implement custom error types
* Use `errors.Is()` and `errors.As()`
* Mock dependencies
* Write clean unit tests

---
