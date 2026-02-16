

# Chapter 2: Functions, Structs & Packages

### Writing Structured Go Code

---

## 1. Why This Chapter Matters

In Python, structuring code usually revolves around:

* Classes
* Dynamic typing
* Duck typing
* Flexible imports

In Go, structuring code is about:

* Explicit types
* Composition over inheritance
* Clear package boundaries
* Predictable design

This chapter teaches how to write **structured, maintainable Go code**, the way production systems expect it.

---

# 2. Functions in Go

## 2.1 Basic Function Syntax

```go
func add(a int, b int) int {
    return a + b
}
```

Unlike Python:

```python
def add(a, b):
    return a + b
```

In Go:

* Types are mandatory
* Return type must be declared
* No implicit dynamic behavior

---

## 2.2 Multiple Return Values (Very Important)

Go commonly returns:

* Value
* Error

```go
func divide(a int, b int) (int, error) {
    if b == 0 {
        return 0, fmt.Errorf("cannot divide by zero")
    }
    return a / b, nil
}
```

Usage:

```go
result, err := divide(10, 2)
if err != nil {
    log.Fatal(err)
}
```

This is a fundamental mindset shift from Python exceptions.

In Python:

```python
try:
    result = divide(10, 2)
except Exception as e:
    print(e)
```

In Go:

* Errors are values
* Error handling is explicit
* You must check them

This makes code verbose — but predictable and safe.

---

## 2.3 Named Return Values

```go
func rectangle(width int, height int) (area int) {
    area = width * height
    return
}
```

⚠ Use sparingly. Clarity > cleverness.

---

## 2.4 Variadic Functions

```go
func sum(nums ...int) int {
    total := 0
    for _, n := range nums {
        total += n
    }
    return total
}
```

Similar to Python’s `*args`.

---

## 2.5 First-Class Functions

Functions can be passed around:

```go
func operate(a int, b int, fn func(int, int) int) int {
    return fn(a, b)
}
```

Used heavily in middleware patterns later (Chapter 7).

---

# 3. Structs: Go’s Core Building Block

Go does NOT have classes.

Instead, it has:

> Struct + Methods

---

## 3.1 Basic Struct

```go
type User struct {
    ID    int
    Name  string
    Email string
}
```

Creating an instance:

```go
u := User{
    ID:    1,
    Name:  "Aditya",
    Email: "aditya@example.com",
}
```

Or:

```go
u := User{}
u.Name = "Aditya"
```

---

## 3.2 Structs Are Value Types

This is critical.

```go
func updateName(u User) {
    u.Name = "Updated"
}
```

This won’t modify the original struct.

Why?

Because Go passes structs by value.

To modify:

```go
func updateName(u *User) {
    u.Name = "Updated"
}
```

Now you're using pointers.

---

# 4. Methods on Structs

Go attaches methods to types.

```go
func (u User) Display() string {
    return fmt.Sprintf("%s (%s)", u.Name, u.Email)
}
```

This is called a **value receiver**.

---

## 4.1 Pointer Receivers

```go
func (u *User) UpdateEmail(newEmail string) {
    u.Email = newEmail
}
```

Use pointer receivers when:

* Modifying struct
* Avoiding large copy
* Struct contains mutable state

Rule of thumb:

> If one method uses pointer receiver, most methods should.

Consistency matters.

---

# 5. Composition Over Inheritance

Python supports inheritance.

Go does NOT.

Instead:

```go
type Address struct {
    City    string
    Country string
}

type Customer struct {
    Name    string
    Address Address
}
```

Or embedding:

```go
type Customer struct {
    Name string
    Address
}
```

Now `Customer` gets Address fields promoted.

This is Go’s version of “inheritance” — but safer.

---

# 6. Export Rules (Very Important)

Capital letter = exported
Lowercase = private

```go
type user struct {        // not exported
    name string           // not exported
}

type User struct {        // exported
    Name string           // exported
}
```

This replaces Python’s “convention-based privacy”.

---

# 7. Constructors (Idiomatic Pattern)

Go has no constructors.

Instead:

```go
func NewUser(name string, email string) *User {
    return &User{
        Name:  name,
        Email: email,
    }
}
```

Convention:

* Constructor starts with `New`
* Returns pointer usually

---

# 8. Packages & Project Structure

## 8.1 Why Packages Matter

Go enforces modularity strongly.

Unlike Python where circular imports are common,
Go pushes clean boundaries.

---

## 8.2 Creating a Module

```
go mod init github.com/practice-golang-for-beginners/chapter2
```

---

## 8.3 Basic Project Structure

```
project/
├── main.go
├── go.mod
└── user/
    └── user.go
```

In `user.go`:

```go
package user
```

In `main.go`:

```go
import "project/user"
```

---

## 8.4 Avoid This Mistake

Don’t create deep nesting early.

Prefer:

```
internal/
pkg/
cmd/
```

(We’ll revisit in Chapter 8.)

---

# 9. Zero Values (Critical Concept)

Go gives every type a default value.

* int → 0
* string → ""
* bool → false
* struct → zeroed fields
* pointer → nil

Example:

```go
var u User
fmt.Println(u.Name) // empty string
```

This reduces need for constructors in simple cases.

---

# 10. Idiomatic Design Principles

As a mentor, emphasize:

### 1. Keep functions small

### 2. Avoid unnecessary abstraction

### 3. Prefer composition

### 4. Return errors, don’t panic

### 5. Make zero value useful

---

# 11. Common Python → Go Mistakes

❌ Trying to simulate classes
❌ Overusing interfaces too early
❌ Ignoring error values
❌ Using global variables
❌ Writing Java-style deep hierarchies

Teach simplicity.

Go rewards clarity over cleverness.

---

# 12. Mini Design Example

Design a simple Order system:

```go
type Order struct {
    ID     int
    Amount float64
}

func (o Order) IsLarge() bool {
    return o.Amount > 1000
}
```

Then:

* Add constructor
* Add validation
* Add method with pointer receiver
* Move to package

This becomes an exercise next.

---

# 13. End of Chapter Learning Outcome

By the end of this chapter, the learner should:

* Understand Go function signatures
* Handle multiple return values
* Write and attach methods
* Understand pointer vs value receivers
* Design clean structs
* Organize code into packages
* Follow export conventions

---
