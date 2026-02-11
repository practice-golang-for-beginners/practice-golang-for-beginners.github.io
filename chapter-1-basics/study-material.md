# 📘 Chapter 1 – Go Fundamentals & Tooling

*(Python → Go Transition Guide)*

---

## 1. Why Go? Understanding the Philosophy First

Before writing code, it’s important to understand **why Go exists**.

Go was created at Google by engineers including Robert Griesemer, Rob Pike, and Ken Thompson.

It was designed to solve problems like:

* Slow compilation times
* Complex dependency management
* Over-engineered abstraction layers
* Hard-to-reason-about concurrency

### Core Design Principles of Go

* Simplicity over cleverness
* Explicit over implicit
* Composition over inheritance
* Concurrency as a first-class citizen
* Fast compilation

If you come from Python, you may notice:

| Python          | Go                     |
| --------------- | ---------------------- |
| Dynamic typing  | Static typing          |
| Exceptions      | Explicit error returns |
| OOP-heavy       | Composition-driven     |
| Flexible syntax | Strict formatting      |
| Runtime magic   | Compile-time safety    |

Go removes “magic” and replaces it with clarity.

---

## 2. Installing and Understanding the Go Toolchain

### Installation

Install from the official site:
👉 [https://go.dev](https://go.dev)

Verify installation:

```bash
go version
```

You should see something like:

```
go version go1.22.x darwin/amd64
```

---

### Understanding the Go Toolchain

Go is opinionated. Everything revolves around the `go` command.

Common commands:

```bash
go run main.go
go build
go test
go fmt
go mod init
go mod tidy
```

Key idea:
**You do not manually manage compilation steps. The toolchain does it.**

This is unlike Python, where execution is interpreter-based.

---

## 3. Modules & Project Structure

Initialize a new project:

```bash
mkdir hello-go
cd hello-go
go mod init github.com/yourname/hello-go
```

This creates:

```
go.mod
```

The `go.mod` file tracks dependencies.

Example:

```go
module github.com/yourname/hello-go

go 1.22
```

In Go:

* Dependencies are versioned
* Builds are reproducible
* No virtualenv required

---

## 4. Your First Go Program

Create `main.go`:

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, Go!")
}
```

Run it:

```bash
go run main.go
```

---

### Breaking This Down

```go
package main
```

Every Go file belongs to a package.

```go
import "fmt"
```

Imports are explicit and unused imports cause compilation failure.

```go
func main()
```

The entry point of executable programs.

---

## 5. Variables and Types

Unlike Python:

```python
x = 10
```

In Go:

```go
var x int = 10
```

Or shorter:

```go
x := 10
```

### Type Inference

Go infers types but keeps them static.

```go
name := "Aditya"   // string
age := 30          // int
```

You cannot later do:

```go
age = "thirty"  // Compile-time error
```

This is intentional. It prevents runtime surprises.

---

## 6. Basic Data Types

### Integers

```go
int
int8, int16, int32, int64
uint
```

### Floating Point

```go
float32
float64
```

### Boolean

```go
bool
```

### String

```go
string
```

Go strings are immutable, like Python.

---

## 7. Zero Values (Very Important)

In Go, variables always have a default value.

| Type    | Zero Value |
| ------- | ---------- |
| int     | 0          |
| float   | 0.0        |
| bool    | false      |
| string  | ""         |
| pointer | nil        |

This eliminates uninitialized variable bugs common in C-like languages.

---

## 8. Control Structures

### If Statement

```go
if x > 10 {
    fmt.Println("Greater than 10")
}
```

No parentheses required.
Braces are mandatory.

---

### If with Initialization

```go
if y := compute(); y > 10 {
    fmt.Println(y)
}
```

Scoped initialization inside condition — very common in Go.

---

### For Loop (Only Loop in Go)

Go has only one loop construct: `for`.

Traditional:

```go
for i := 0; i < 5; i++ {
    fmt.Println(i)
}
```

While-style:

```go
i := 0
for i < 5 {
    i++
}
```

Infinite:

```go
for {
    // loop forever
}
```

No `while` keyword exists.

---

## 9. Switch Statements

```go
switch day {
case "Monday":
    fmt.Println("Start of week")
case "Friday":
    fmt.Println("Almost weekend")
default:
    fmt.Println("Another day")
}
```

Go switches:

* Do NOT fall through by default
* Can switch without a variable

Example:

```go
switch {
case x > 10:
    fmt.Println("Large")
case x > 5:
    fmt.Println("Medium")
}
```

---

## 10. Functions

Basic function:

```go
func add(a int, b int) int {
    return a + b
}
```

Shorter parameter syntax:

```go
func add(a, b int) int {
    return a + b
}
```

---

### Multiple Return Values (Very Important)

```go
func divide(a, b int) (int, error) {
    if b == 0 {
        return 0, fmt.Errorf("division by zero")
    }
    return a / b, nil
}
```

This replaces Python-style exceptions.

Calling it:

```go
result, err := divide(10, 2)
if err != nil {
    fmt.Println("Error:", err)
    return
}
fmt.Println(result)
```

Explicit error handling is core to Go.

---

## 11. Understanding Explicit Error Handling

Python:

```python
try:
    ...
except:
    ...
```

Go:

```go
if err != nil {
    return err
}
```

This is deliberate.

It makes failure paths visible and reviewable.

As a mentor, reinforce:

* Never ignore errors
* Handle or propagate them

---

## 12. Formatting & Code Discipline

Go enforces formatting:

```bash
go fmt ./...
```

Unlike Python (PEP8 optional), Go formatting is automatic and universal.

This reduces code review noise and team conflict.

---

## 13. Writing Your First Test

Create:

```
main_test.go
```

Example:

```go
package main

import "testing"

func TestAdd(t *testing.T) {
    result := add(2, 3)
    if result != 5 {
        t.Errorf("Expected 5, got %d", result)
    }
}
```

Run:

```bash
go test
```

Go’s testing is built-in. No external framework required.

---

## 14. Table-Driven Tests (Idiomatic Go)

```go
func TestAdd(t *testing.T) {
    tests := []struct {
        a, b int
        expected int
    }{
        {2, 3, 5},
        {0, 5, 5},
        {-1, 1, 0},
    }

    for _, tt := range tests {
        result := add(tt.a, tt.b)
        if result != tt.expected {
            t.Errorf("Expected %d, got %d", tt.expected, result)
        }
    }
}
```

This pattern is extremely common in production Go.

---

## 15. Key Mindset Shifts for Python Developers

1. Less magic, more clarity
2. No inheritance hierarchies
3. Errors are values
4. Formatting is mandatory
5. Simple > clever

If something feels “too simple”, that is usually the right approach in Go.

---

# End of Chapter 1

### By the End of This Chapter, You Should:

* Install and use Go confidently
* Understand Go syntax basics
* Write simple programs
* Handle errors properly
* Write basic tests
* Use the Go toolchain comfortably

---
