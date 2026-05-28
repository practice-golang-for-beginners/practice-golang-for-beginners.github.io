---
layout: default
title: Excercises
parent: Chapter 4 – Collections, Pointers & Memory
nav_order: 3
---



# Chapter 4 – Exercises - Collections, Pointers & Memory

---

# 🟢 Level 1 – Fundamentals (Warm-up)

---

### 1️⃣ Array Copy Behavior

Create a function:

```go
func modify(arr [3]int)
```

Inside the function, change the first element to `100`.

Call this function from `main` and print the array before and after.

**Question:**
Why does the original array not change?

---

### 2️⃣ Slice Modification

Create a slice:

```go
nums := []int{1, 2, 3}
```

Pass it to a function that modifies `nums[0] = 100`.

Print before and after.

**Question:**
Why does this change affect the original slice?

---

### 3️⃣ Slice Length & Capacity

Create a slice using:

```go
make([]int, 2, 5)
```

Append elements and print:

* length
* capacity
* values

After each append.

Observe when capacity changes.

---

### 4️⃣ Nil Slice vs Empty Slice

Create:

```go
var a []int
b := []int{}
```

Print:

* `a == nil`
* `b == nil`
* `len(a)`
* `len(b)`

Explain the difference.

---

### 5️⃣ Safe Slice Copy

Write a function:

```go
func clone(nums []int) []int
```

Return a deep copy of the slice.

Verify that modifying the cloned slice does not affect original.

---

# 🟡 Level 2 – Maps & Semantics

---

### 6️⃣ Nil Map Panic

Create:

```go
var m map[string]int
```

Attempt to assign a value.

Observe the panic.

Fix the code.

---

### 7️⃣ Safe Map Lookup

Create a map of string → int.

Write a function that safely checks if a key exists using:

```go
value, ok := m[key]
```

Print appropriate messages.

---

### 8️⃣ Map Passed to Function

Create a function:

```go
func update(m map[string]int)
```

Modify the map inside.

Verify whether the original changes.

Explain why.

---

### 9️⃣ Slice Sharing Pitfall

Create:

```go
a := []int{1, 2, 3, 4}
b := a[:2]
```

Modify `b[0]`.

Print both.

Now create a proper independent copy of `b`.

Verify isolation.

---

### 🔟 Remove Element from Slice

Write a function:

```go
func removeAt(nums []int, index int) []int
```

Remove element at given index.

Test with:

* index 0
* last index
* middle index
* invalid index

---

# 🟠 Level 3 – Pointers & Struct Behavior

---

### 1️⃣1️⃣ Value Receiver vs Pointer Receiver

Create:

```go
type Counter struct {
    Count int
}
```

Implement:

* `Increment()` using value receiver
* `IncrementPointer()` using pointer receiver

Test both.

Explain the difference.

---

### 1️⃣2️⃣ Pointer to Struct

Create a `User` struct.

Write a function that takes `*User` and modifies a field.

Test what happens if you pass:

* A value
* A pointer

---

### 1️⃣3️⃣ Nil Pointer Panic

Create:

```go
var u *User
```

Attempt to access `u.Name`.

Handle it safely.

Add a nil check.

---

### 1️⃣4️⃣ Method Consistency

Create a struct with 3 methods.

Make one method pointer receiver.

Make others value receivers.

Observe behavior.

Refactor for consistency.

---

### 1️⃣5️⃣ Large Struct Efficiency

Create a struct with 10+ fields.

Pass it to a function:

* Once as value
* Once as pointer

Use `fmt.Printf("%p", &struct)` inside function.

Observe address differences.

Explain performance implications.

---

# 🔴 Level 4 – Edge Cases & Tricky Scenarios

---

### 1️⃣6️⃣ Slice Append Trap

Create:

```go
func add(nums []int) {
    nums = append(nums, 100)
}
```

Call it with:

```go
a := []int{1, 2}
add(a)
```

Print `a`.

Why does it not change?

Fix the function.

---

### 1️⃣7️⃣ Map of Structs – Modification Trap

Create:

```go
type User struct {
    Name string
}
```

Create:

```go
m := map[string]User{}
```

Add one user.

Attempt to modify:

```go
m["u1"].Name = "NewName"
```

Observe compiler error.

Fix it properly.

---

### 1️⃣8️⃣ Interface Nil Trap (Advanced)

Create:

```go
var u *User = nil
var i interface{} = u
```

Check:

```go
if i == nil
```

Print result.

Explain why it behaves this way.

---

### 1️⃣9️⃣ Concurrent Map Access (Preview of Week 5)

Create a map.

Start two goroutines:

* One writing
* One reading

Observe behavior.

Research why this is unsafe.

Do NOT fix using mutex yet — just understand the problem.

---

### 2️⃣0️⃣ Memory Escape Thought Exercise

Write:

```go
func createUser() *User {
    u := User{Name: "Test"}
    return &u
}
```

Is this safe?

Why does it not cause invalid memory access?

Research and explain escape analysis.

---

# 🎯 Bonus Reflection Questions

After completing all exercises, answer:

1. Why are slices not pure references?
2. Why does Go prefer returning nil slices?
3. When should you use pointer receivers?
4. Why must maps be initialized?
5. What is the most common slice bug you encountered?

---

These exercises are intentionally layered.

They will:

* Break incorrect mental models
* Strengthen memory understanding
* Prepare her for concurrency next week
* Build real debugging intuition

---

