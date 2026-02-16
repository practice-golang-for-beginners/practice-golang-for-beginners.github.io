
# 📘 exercises.md

# Chapter 2 – Exercises

**Functions, Structs & Packages**

---

## 🟢 Basic Level

### Exercise 1 – Simple Function

Write a function `Multiply(a int, b int) int` that returns the product of two integers.

---

### Exercise 2 – Multiple Return Values

Write a function `SafeDivide(a int, b int) (int, error)`
Return an error if `b == 0`.

---

### Exercise 3 – Struct Creation

Create a struct `Book` with fields:

* Title (string)
* Author (string)
* Price (float64)

Create one instance and print it.

---

### Exercise 4 – Method on Struct

Add a method to `Book`:

```
Discount(percent float64)
```

It should reduce the price accordingly.

---

### Exercise 5 – Value vs Pointer Receiver

Modify Exercise 4 so that:

* One version uses value receiver
* One version uses pointer receiver

Observe the difference.

---

## 🟡 Intermediate Level

### Exercise 6 – Constructor Pattern

Create a constructor:

```
NewBook(title string, author string, price float64) *Book
```

Ensure price cannot be negative.

---

### Exercise 7 – Validation Method

Add a method:

```
IsExpensive() bool
```

Return true if price > 1000.

---

### Exercise 8 – Struct Composition

Create:

```
type Address struct {
    City string
    Country string
}
```

Embed it inside:

```
type Customer struct {
    Name string
    Address
}
```

Create a sample customer.

---

### Exercise 9 – Package Organization

Move `Book` into its own package called `library`.

Use it in `main.go`.

---

### Exercise 10 – Zero Value Safety

Create a `User` struct with:

* Name
* Age

Write a method `IsValid()` that returns false if zero values are detected.

---

## 🔵 Advanced Level

### Exercise 11 – Function as Parameter

Create a function:

```
ApplyOperation(a int, b int, op func(int, int) int) int
```

Test with addition and multiplication.

---

### Exercise 12 – Interface Preparation

Define an interface:

```
type Printable interface {
    Print() string
}
```

Make `Book` implement it.

---

### Exercise 13 – Struct Method Returning Error

Add a method to `Book`:

```
UpdatePrice(newPrice float64) error
```

Return error if price is negative.

---

### Exercise 14 – Slice of Structs

Create a slice of `Book`.
Write a function:

```
TotalPrice(books []Book) float64
```

---

### Exercise 15 – Pointer Semantics

Create a function:

```
IncreaseAllPrices(books []Book, percent float64)
```

Observe if changes persist. Fix it properly.

---

### Exercise 16 – Encapsulation Practice

Make the `price` field unexported.
Create getter and setter methods.

---

### Exercise 17 – Method Chaining

Modify setter methods to return pointer to struct for chaining.

---

### Exercise 18 – Factory Logic

Create a function:

```
NewPremiumBook(...)
```

It automatically marks a boolean `Premium = true`.

---

### Exercise 19 – Small Refactor

Refactor code so:

* No direct struct field access outside package
* All modification via methods

---

### Exercise 20 – Mini Design Task

Design a `Cart` struct:

* AddBook()
* RemoveBook()
* TotalAmount()

Organize cleanly inside a package.

---

