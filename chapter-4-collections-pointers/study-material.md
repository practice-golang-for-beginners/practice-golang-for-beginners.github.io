---
layout: default
title: Study Material
parent: Chapter 4 – Collections, Pointers & Memory
nav_order: 2
---

## 🎯 Goal

Understand how Go handles slices, maps, pointers, and memory so that you can:

* Avoid subtle bugs
* Write predictable code
* Understand value vs reference semantics
* Avoid `nil`-related panics
* Make informed design decisions

This chapter is where Go starts to feel *different* from Python.

---

# 1️⃣ Arrays vs Slices – The Foundation

## 1.1 Arrays in Go

An array in Go has:

* Fixed size
* Size as part of its type
* Value semantics

Example:

```go
var numbers [3]int
```

Important:

```
[3]int != [4]int
```

They are different types.

### Arrays are Values

If you pass an array to a function:

```go
func modify(arr [3]int) {
    arr[0] = 100
}
```

The original array will NOT change.

Why?

Because arrays are copied when passed.

This is different from Python lists, which are references.

---

## 1.2 Why Arrays Are Rarely Used

In production Go:

* Arrays are mostly used internally
* Slices are used almost everywhere

Arrays exist mainly because:

* They are part of Go’s type system
* Slices are built on top of them

---

# 2️⃣ Slices – The Real Collection Type

Slices are:

* Dynamic
* Flexible
* Backed by arrays
* Reference-like (but not fully references)

---

## 2.1 What Is a Slice?

A slice is a small struct:

It contains:

* Pointer to an underlying array
* Length
* Capacity

Conceptually:

```
slice -> { ptr, len, cap }
```

---

## 2.2 Creating Slices

### Literal

```go
nums := []int{1, 2, 3}
```

### Using make

```go
nums := make([]int, 3)
```

This creates:

* length = 3
* capacity = 3
* all values initialized to zero

---

## 2.3 Length vs Capacity

```go
nums := make([]int, 3, 5)
```

* len = 3
* cap = 5

Capacity defines how much it can grow before allocating new memory.

This matters for performance.

---

## 2.4 Append Behavior

```go
nums := []int{1, 2}
nums = append(nums, 3)
```

If capacity is exceeded:

* Go allocates new array
* Copies old data
* Updates pointer

This is why modifying slices can be tricky.

---

## 2.5 Slice Sharing Pitfall

Example:

```go
a := []int{1, 2, 3, 4}
b := a[:2]
b[0] = 100
```

Now `a` becomes:

```
[100 2 3 4]
```

Because both share the same underlying array.

This surprises many developers.

---

## 2.6 Safe Copying

To avoid shared-memory issues:

```go
copySlice := make([]int, len(original))
copy(copySlice, original)
```

Now they are independent.

---

# 3️⃣ Maps – Reference Types with Rules

Maps in Go are:

* Built-in reference types
* Must be initialized before use
* Not thread-safe

---

## 3.1 Creating Maps

### Literal

```go
m := map[string]int{
    "a": 1,
}
```

### Using make

```go
m := make(map[string]int)
```

---

## 3.2 Zero Value of Map

```go
var m map[string]int
```

This creates:

```
m == nil
```

Trying to assign:

```go
m["a"] = 1
```

Will panic.

Why?

Because the map is nil.

You must initialize with `make`.

---

## 3.3 Reading From Map

Reading from a nil map is safe:

```go
value := m["a"]  // returns zero value
```

But writing is not.

---

## 3.4 Checking Existence

```go
value, ok := m["a"]
```

This is the correct pattern.

Never assume key existence.

---

## 3.5 Maps Are Reference Types

When you pass a map to a function:

* It is not copied
* Changes affect original

Example:

```go
func update(m map[string]int) {
    m["x"] = 10
}
```

Original map is modified.

This is different from arrays.

---

# 4️⃣ Pointers – Controlled Memory Access

Go does not allow pointer arithmetic.

Pointers are safe and restricted.

---

## 4.1 Basic Pointer

```go
x := 10
p := &x
```

* `&` gives address
* `*p` dereferences

---

## 4.2 Why Use Pointers?

Three main reasons:

1. Avoid copying large structs
2. Modify original data
3. Indicate optional values

---

## 4.3 Structs and Pointers

Example:

```go
type User struct {
    Name string
}
```

### Value receiver:

```go
func (u User) UpdateName(name string) {
    u.Name = name
}
```

This does NOT modify original.

### Pointer receiver:

```go
func (u *User) UpdateName(name string) {
    u.Name = name
}
```

This modifies original.

---

## 4.4 When to Use Pointer Receivers

Use pointer receivers when:

* Struct is large
* You need to modify state
* Consistency across methods is needed

Rule of thumb:

If one method needs pointer receiver → make all methods pointer receivers.

---

# 5️⃣ Value vs Reference Semantics

This is critical.

In Go:

| Type    | Behavior When Passed                     |
| ------- | ---------------------------------------- |
| int     | Copied                                   |
| string  | Copied                                   |
| array   | Copied                                   |
| struct  | Copied                                   |
| slice   | Descriptor copied (shared backing array) |
| map     | Reference                                |
| pointer | Reference                                |

Understanding this prevents subtle bugs.

---

# 6️⃣ The `nil` Pitfalls

`nil` is a valid zero value for:

* slices
* maps
* pointers
* interfaces
* channels
* functions

---

## 6.1 Nil Slice vs Empty Slice

```go
var s []int
```

This is nil.

```
s == nil → true
len(s) → 0
```

But:

```go
s := []int{}
```

This is not nil.

In most cases, prefer returning nil slices.

Why?

Because:

* `len(nilSlice)` is safe
* It behaves naturally

---

## 6.2 Nil Map Panic

```go
var m map[string]int
m["x"] = 1  // panic
```

Always initialize maps.

---

## 6.3 Nil Pointer Panic

```go
var u *User
u.Name = "Aditya"  // panic
```

Always check before dereferencing.

---

## 6.4 Interface Nil Trap

This is advanced but important.

```go
var u *User = nil
var i interface{} = u
```

Now:

```
i != nil
```

Even though underlying pointer is nil.

Because interface holds:

* Type
* Value

This confuses many developers.

---

# 7️⃣ Memory Model Basics (Conceptual)

Go has:

* Stack allocation
* Heap allocation
* Garbage collector

You don't manually manage memory.

But:

If something “escapes” function scope → it goes to heap.

Example:

```go
func createUser() *User {
    u := User{Name: "A"}
    return &u
}
```

This is safe.

Go performs escape analysis.

---

# 8️⃣ Common Beginner Mistakes

1. Forgetting to initialize map
2. Confusing slice copy vs reference
3. Using value receiver unintentionally
4. Dereferencing nil pointer
5. Ignoring capacity behavior
6. Assuming slice append modifies original always

---

# 9️⃣ Practical Refactoring Guidance

Take previous weeks’ code and:

* Replace fixed arrays with slices
* Replace struct value receivers with pointer receivers
* Replace global maps with initialized maps
* Add defensive nil checks
* Add tests for edge cases

This is how knowledge becomes practical.

---

# 1️⃣0️⃣ Summary

After this chapter, you should understand:

* Why slices are not arrays
* Why maps must be initialized
* When to use pointer receivers
* How value semantics differ from Python
* How nil behaves in Go
* How to avoid subtle memory bugs

This chapter builds your mental model of Go’s memory and data structures.

Without mastering this, concurrency (Week 5) will feel dangerous.

With this mastered, Go becomes predictable and powerful.

---
