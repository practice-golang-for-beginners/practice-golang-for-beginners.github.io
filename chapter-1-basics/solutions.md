---
layout: default
title: Solutions
parent: Chapter 1 – Basics
nav_order: 3
---

# 📘 Chapter 1 – Detailed Solutions

*(Go Fundamentals & Tooling)*

---

# 🔹 Section 1 – Setup & Toolchain

---

## ✅ Solution 1 – Environment Verification

### Commands

```bash
go version
go env
go mod init github.com/yourname/chapter1
go mod tidy
```

### Explanation

* `go.mod` defines:

  * Module name
  * Go version
  * Dependencies
* Module naming is important because:

  * It defines import paths
  * It ensures reproducible builds
  * It avoids dependency conflicts

Go modules replace older GOPATH-based dependency management.

---

## ✅ Solution 2 – First Program

```go
package main

import "fmt"

func main() {
	fmt.Println("Welcome to Go Programming")
}
```

### Difference Between `go run` and `go build`

* `go run`:

  * Compiles + runs in one step
  * Temporary binary
* `go build`:

  * Produces executable binary
  * No execution happens

Use `go build` for production-ready builds.

---

# 🔹 Section 2 – Variables & Types

---

## ✅ Solution 3 – Type Declaration

```go
package main

import "fmt"

func main() {
	var age int = 30
	var height float64 = 5.9
	var isActive bool = true
	var name string = "Aditya"

	fmt.Printf("%T\n", age)
	fmt.Printf("%T\n", height)
	fmt.Printf("%T\n", isActive)
	fmt.Printf("%T\n", name)
}
```

### Static Typing

Once declared, type cannot change.

This prevents runtime surprises common in dynamically typed languages.

---

## ✅ Solution 4 – Type Inference

```go
age := 30
height := 5.9
isActive := true
name := "Aditya"
```

### When `:=` Cannot Be Used

* Outside functions
* When variable already declared

Example:

```go
var age int
age = 30   // correct
age := 30  // compile error
```

---

## ✅ Solution 5 – Zero Values

```go
var a int
var b string
var c bool

fmt.Println(a) // 0
fmt.Println(b) // ""
fmt.Println(c) // false
```

Zero values eliminate uninitialized memory errors.

---

# 🔹 Section 3 – Control Flow

---

## ✅ Solution 6 – Conditional Logic

```go
number := 10

if number > 0 {
	fmt.Println("Positive")
} else if number < 0 {
	fmt.Println("Negative")
} else {
	fmt.Println("Zero")
}
```

### With Initialization

```go
if n := number; n > 0 {
	fmt.Println("Positive")
}
```

---

## ✅ Solution 7 – Looping

### Classic For

```go
for i := 1; i <= 20; i++ {
	fmt.Println(i)
}
```

### While-style

```go
i := 1
for i <= 20 {
	fmt.Println(i)
	i++
}
```

### Infinite Loop

```go
i := 1
for {
	if i > 20 {
		break
	}
	fmt.Println(i)
	i++
}
```

---

## ✅ Solution 8 – Switch Case

```go
day := "Saturday"

switch day {
case "Saturday", "Sunday":
	fmt.Println("Weekend")
default:
	fmt.Println("Weekday")
}
```

Switch without variable:

```go
switch {
case day == "Saturday" || day == "Sunday":
	fmt.Println("Weekend")
default:
	fmt.Println("Weekday")
}
```

---

# 🔹 Section 4 – Functions

---

## ✅ Solution 9 – Multiply Function

```go
func multiply(a, b int) int {
	return a * b
}
```

Call:

```go
result := multiply(3, 4)
fmt.Println(result)
```

---

## ✅ Solution 10 – Divide Function

```go
func divide(a, b int) (int, error) {
	if b == 0 {
		return 0, fmt.Errorf("division by zero")
	}
	return a / b, nil
}
```

Calling:

```go
result, err := divide(10, 2)
if err != nil {
	fmt.Println("Error:", err)
	return
}
fmt.Println(result)
```

Explicit error handling ensures predictable control flow.

---

## ✅ Solution 11 – Ignoring Return Values

Incorrect:

```go
result, err := divide(10, 2)
```

If unused:

```go
_, err := divide(10, 2)
```

Compiler does NOT allow:

```go
result, err := divide(10, 2)
// if result not used → compile error
```

This prevents sloppy coding.

---

# 🔹 Section 5 – Error Handling

---

## ✅ Solution 12 – Custom Error

```go
func validateAge(age int) error {
	if age < 18 {
		return fmt.Errorf("age must be at least 18")
	}
	return nil
}
```

---

## ✅ Solution 13 – Error Propagation

```go
func process(age int) error {
	err := validateAge(age)
	if err != nil {
		return err
	}
	fmt.Println("Valid age")
	return nil
}
```

Do NOT log and return. Choose one responsibility.

---

# 🔹 Section 6 – Formatting & Tooling

---

## ✅ Solution 14 – go fmt

Before:

```go
func main(){fmt.Println("bad formatting")}
```

After:

```go
func main() {
	fmt.Println("bad formatting")
}
```

Uniform formatting improves team collaboration.

---

## ✅ Solution 15 – Unused Import

```go
import "math"
```

Compiler error:

```
imported and not used: "math"
```

This keeps code clean and dependency-aware.

---

# 🔹 Section 7 – Testing

---

## ✅ Solution 16 – Multiply Test

```go
func TestMultiply(t *testing.T) {
	result := multiply(2, 3)
	if result != 6 {
		t.Errorf("Expected 6, got %d", result)
	}
}
```

---

## ✅ Solution 17 – Failing Test

If function returns wrong value:

```go
return a + b
```

`go test` output:

```
--- FAIL: TestMultiply
    main_test.go:8: Expected 6, got 5
FAIL
```

Go shows:

* Test name
* Line number
* Expected vs actual

---

## ✅ Solution 18 – Table-Driven Test

```go
func TestMultiply(t *testing.T) {
	tests := []struct {
		a, b     int
		expected int
	}{
		{2, 3, 6},
		{0, 5, 0},
		{-1, 2, -2},
		{5, 5, 25},
		{1, 1, 1},
	}

	for _, tt := range tests {
		result := multiply(tt.a, tt.b)
		if result != tt.expected {
			t.Errorf("Expected %d, got %d", tt.expected, result)
		}
	}
}
```

This pattern scales cleanly in production.

---

# 🔹 Section 8 – Calculator CLI

---

## ✅ Solution 19 – Calculator CLI

```go
package main

import (
	"fmt"
	"os"
	"strconv"
)

func main() {
	if len(os.Args) < 4 {
		fmt.Println("Usage: <num1> <operator> <num2>")
		return
	}

	a, _ := strconv.Atoi(os.Args[1])
	operator := os.Args[2]
	b, _ := strconv.Atoi(os.Args[3])

	switch operator {
	case "+":
		fmt.Println(a + b)
	case "-":
		fmt.Println(a - b)
	case "*":
		fmt.Println(a * b)
	case "/":
		result, err := divide(a, b)
		if err != nil {
			fmt.Println(err)
			return
		}
		fmt.Println(result)
	default:
		fmt.Println("Invalid operator")
	}
}
```

Production improvement:

* Do NOT ignore `Atoi` errors in real systems.

---

## ✅ Solution 20 – Reflection (Sample Structure)

```markdown
# Python vs Go – Chapter 1 Reflection

## Differences
1. Static typing vs dynamic typing
2. Explicit error handling vs exceptions
3. Single loop construct
4. Strict formatting rules
5. No inheritance model

## Things I Liked
1. Simplicity
2. Built-in testing

## Things I Found Difficult
1. Verbose error handling
2. Strict compiler rules

## When I Would Choose Go
- Backend microservices
- High-performance systems
- Concurrent processing
```

Encourage honest reflection.

---

# 🎯 Final Outcome After Chapter 1

The learner can now:

* Use Go tooling confidently
* Understand static typing
* Write idiomatic functions
* Handle errors properly
* Write tests
* Build a simple CLI
* Compare Go vs Python thoughtfully

---
