---
layout: default
title: Test Cases
parent: Chapter 1 – Basics
nav_order: 5
---




**Go Fundamentals & Tooling**

## 🧪 Section 1 – Basic Functions & Toolchain

### ✏️ Test 01 – Go Toolchain Verification

* Run `go version` and verify output is a Go version.
* Run `go env` and check that Go environment variables are present.
* Verify `go mod init …` creates a valid `go.mod` file.

---

## 🧪 Section 2 – First Program

### ✏️ Test 02 – Hello World

Create a program:

```go
package main

import "fmt"

func main() {
    fmt.Println("Welcome to Go Programming")
}
```

Run with:

```bash
go run .
```

**Expect:** “Welcome to Go Programming”.

---

## 🧪 Section 3 – Variables & Types

### ✏️ Test 03 – Type Declarations

Write a program declaring:

* `var age int`
* `var height float64`
* `var isActive bool`
* `var name string`

Print types using `%T`.
**Expect:** Correct type names printed.

---

### ✏️ Test 04 – Type Inference

Use `:=` shorthand inside a function.
**Expect:** Types inferred and code compiles.

---

### ✏️ Test 05 – Zero Values

Declare variables without initialization and print them.
**Expect:** Zero values: `0`, `""`, `false`, etc.

---

## 🧪 Section 4 – Control Flow

### ✏️ Test 06 – If / Else Logic

Write a program with an `if/else` to classify a number (>0, <0, =0).
**Expect:** Correct branch output.

---

### ✏️ Test 07 – Looping

Write three loops:

* Classic `for i := 1; i <= 10; i++`
* While-style `for i < …`
* Infinite loop with a `break`

**Expect:** Correct numbers printed.

---

### ✏️ Test 08 – Switch Case

Use `switch` with multiple values.
**Expect:** Correct fallthrough behavior *not* happening by default.

---

## 🧪 Section 5 – Functions

### ✏️ Test 09 – Multiply Function

Implement:

```go
func multiply(a, b int) int
```

Test that `multiply(2,3) == 6`.

---

### ✏️ Test 10 – Divide With Error

Implement:

```go
func divide(a, b int) (int, error)
```

Test:

* `divide(10, 2)` → result `5`, nil error
* `divide(10, 0)` → non-nil error

---

### ✏️ Test 11 – Ignore Return Values

Try ignoring the result value.
**Expect:** Compiler error if result is unused.

---

## 🧪 Section 6 – Error Handling

### ✏️ Test 12 – Custom Error

Create:

```go
func validateAge(age int) error
```

Test:

* `validateAge(20)` returns nil
* `validateAge(15)` returns non-nil error

---

### ✏️ Test 13 – Propagate Errors

Wrap error with a second function:

```go
func process(age int) error
```

Test that invalid age propagates error.

---

## 🧪 Section 7 – Formatting & Tooling

### ✏️ Test 14 – go fmt

Before and after running `go fmt`, formatting should change and tests still pass.

---

### ✏️ Test 15 – Unused Import

Create a file with an unused import:

```go
import "math"
```

**Expect:** Compiler refuses to build.

---

## 🧪 Section 8 – Testing

### ✏️ Test 16 – Multiply Unit Test

In `main_test.go`, test:

```go
func TestMultiply(t *testing.T)
```

Verify `multiply(2,3) == 6`.

---

### ✏️ Test 17 – Failing Test

Deliberately introduce a wrong implementation (e.g., `return a + b`).
**Expect:** `go test` reports failure with expected output vs actual.

---

### ✏️ Test 18 – Table-Driven Test

Write a table test covering:

| a  | b | expected |
| -- | - | -------- |
| 2  | 3 | 5        |
| 0  | 5 | 5        |
| –1 | 1 | 0        |

**Expect:** All subtests pass.

---

## 🧪 Section 9 – Bonus: CLI Calculator Tests

### ✏️ Test 19 – Calculator CLI

Build a small calculator that accepts:

```
./calc 10 + 4
```

**Expect:**

* Correct addition, subtraction, multiplication
* Division with error handling
* “Invalid operator” for bad input

👉 This combines parsing, error handling, and control flow.

---

## 📌 How to Run Tests

From the root of this chapter folder:

```bash
go test ./...
```

Use:

```bash
go test -failfast
```

for early failures and faster feedback. ([raw.githubusercontent.com][1])

---

## 🧠 Testing Tips for Chapter 1

✔ Add proper `*_test.go` suffix
✔ Use the built-in `testing` package
✔ Avoid silent panics — tests should fail meaningfully ([packtpub.com][2])

---

Perfect 👍
Below is a **complete, clean, idiomatic `main_test.go`** for **Chapter 1 – Basics** based on the earlier exercises (multiply, divide, validateAge, process).

You can place this inside:

```
chapter-1-basics/main_test.go
```

This assumes your implementation file is `main.go` (or similar) in the same package.

---

# 🧪 `main_test.go`

```go
package main

import (
	"errors"
	"testing"
)
```

---

# 1️⃣ Test Multiply (Simple Case)

```go
func TestMultiply(t *testing.T) {
	result := multiply(2, 3)
	expected := 6

	if result != expected {
		t.Fatalf("expected %d, got %d", expected, result)
	}
}
```

✔ Clear
✔ No unnecessary logging
✔ Fails fast

---

# 2️⃣ Table-Driven Test for Multiply (Professional Pattern)

```go
func TestMultiply_TableDriven(t *testing.T) {
	tests := []struct {
		name     string
		a, b     int
		expected int
	}{
		{"positive numbers", 2, 3, 6},
		{"zero value", 0, 5, 0},
		{"negative numbers", -2, 3, -6},
	}

	for _, tt := range tests {
		t.Run(tt.name, func(t *testing.T) {
			result := multiply(tt.a, tt.b)
			if result != tt.expected {
				t.Fatalf("expected %d, got %d", tt.expected, result)
			}
		})
	}
}
```

This is how Go projects typically structure tests.

---

# 3️⃣ Test Divide – Success Case

```go
func TestDivide_Success(t *testing.T) {
	result, err := divide(10, 2)

	if err != nil {
		t.Fatalf("expected no error, got %v", err)
	}

	if result != 5 {
		t.Fatalf("expected 5, got %d", result)
	}
}
```

---

# 4️⃣ Test Divide – Division by Zero

```go
func TestDivide_ByZero(t *testing.T) {
	_, err := divide(10, 0)

	if err == nil {
		t.Fatal("expected error, got nil")
	}
}
```

If you defined a sentinel error like:

```go
var ErrDivideByZero = errors.New("division by zero")
```

Then you can do:

```go
if !errors.Is(err, ErrDivideByZero) {
	t.Fatalf("expected ErrDivideByZero, got %v", err)
}
```

---

# 5️⃣ Test validateAge

```go
func TestValidateAge(t *testing.T) {
	tests := []struct {
		name      string
		age       int
		expectErr bool
	}{
		{"valid age", 20, false},
		{"invalid age", 15, true},
	}

	for _, tt := range tests {
		t.Run(tt.name, func(t *testing.T) {
			err := validateAge(tt.age)

			if tt.expectErr && err == nil {
				t.Fatal("expected error, got nil")
			}

			if !tt.expectErr && err != nil {
				t.Fatalf("unexpected error: %v", err)
			}
		})
	}
}
```

---

# 6️⃣ Test process (Error Propagation)

Assuming:

```go
func process(age int) error {
	if err := validateAge(age); err != nil {
		return err
	}
	return nil
}
```

Test:

```go
func TestProcess_ErrorPropagation(t *testing.T) {
	err := process(15)

	if err == nil {
		t.Fatal("expected propagated error, got nil")
	}
}
```

---

# 7️⃣ Example of Deliberate Failure Test

To demonstrate failing tests:

```go
func TestMultiply_FailureExample(t *testing.T) {
	result := multiply(2, 2)
	expected := 5 // intentionally wrong

	if result == expected {
		t.Fatal("this test should fail to demonstrate failure reporting")
	}
}
```

(Use this only for teaching purposes.)

---

# 🧪 How to Run

From `chapter-1-basics`:

```bash
go test -v
```

For race detection:

```bash
go test -race
```

---

# 🧠 What This Teaches the Student

* Writing `*_test.go` files properly
* Using table-driven tests
* Testing success and failure paths
* Handling errors properly
* Avoiding string comparison of errors
* Clean failure messages
* Professional Go testing structure

---




