---
layout: default
title: Detailed Solutions
parent: Chapter 4 – Collections, Pointers & Memory
nav_order: 4
---



# Chapter 4 – Solutions

Collections, Pointers & Memory

---

# 🟢 Level 1 – Fundamentals

---

## 1️⃣ Array Copy Behavior

### Code

```go
package main

import "fmt"

func modify(arr [3]int) {
	arr[0] = 100
}

func main() {
	a := [3]int{1, 2, 3}
	fmt.Println("Before:", a)
	modify(a)
	fmt.Println("After:", a)
}
```

### Output

```
Before: [1 2 3]
After:  [1 2 3]
```

### Why?

Arrays are **value types**.

When passed to a function, the entire array is copied.

You modified the copy — not the original.

### Mental Model

Array = full value copy.

---

## 2️⃣ Slice Modification

### Code

```go
func modify(nums []int) {
	nums[0] = 100
}
```

### Why does it change original?

Slices contain:

* Pointer to array
* Length
* Capacity

When passed:

* The slice descriptor is copied
* But both descriptors point to the same underlying array

So modifying `nums[0]` modifies shared memory.

### Mental Model

Slice = shared underlying array.

---

## 3️⃣ Slice Length & Capacity

```go
nums := make([]int, 2, 5)

for i := 0; i < 5; i++ {
	nums = append(nums, i)
	fmt.Println("Len:", len(nums), "Cap:", cap(nums))
}
```

### Observation

Capacity grows automatically when exceeded.

Go allocates a new array and copies values.

### Mental Model

Append may reallocate memory.

Never assume append modifies original slice safely unless returned.

---

## 4️⃣ Nil Slice vs Empty Slice

```go
var a []int
b := []int{}

fmt.Println(a == nil) // true
fmt.Println(b == nil) // false
fmt.Println(len(a))   // 0
fmt.Println(len(b))   // 0
```

### Explanation

Both behave similarly in most operations.

Nil slice is usually preferred when returning from functions.

---

## 5️⃣ Safe Slice Copy

```go
func clone(nums []int) []int {
	copySlice := make([]int, len(nums))
	copy(copySlice, nums)
	return copySlice
}
```

### Why?

Without `copy()`, both slices share underlying array.

---

# 🟡 Level 2 – Maps & Semantics

---

## 6️⃣ Nil Map Panic

```go
var m map[string]int
m["a"] = 1  // panic
```

### Fix

```go
m := make(map[string]int)
```

### Why?

Nil map has no backing storage.

Reading is safe. Writing is not.

---

## 7️⃣ Safe Map Lookup

```go
value, ok := m["key"]
if ok {
	fmt.Println("Found:", value)
} else {
	fmt.Println("Not found")
}
```

### Why?

Map returns zero value if key doesn’t exist.

The `ok` prevents ambiguity.

---

## 8️⃣ Map Passed to Function

```go
func update(m map[string]int) {
	m["x"] = 10
}
```

Original map changes.

### Why?

Maps are reference types.

---

## 9️⃣ Slice Sharing Pitfall

```go
a := []int{1, 2, 3, 4}
b := a[:2]
b[0] = 100

fmt.Println(a) // [100 2 3 4]
```

### Proper copy

```go
b := make([]int, 2)
copy(b, a[:2])
```

---

## 🔟 Remove Element from Slice

```go
func removeAt(nums []int, index int) []int {
	if index < 0 || index >= len(nums) {
		return nums
	}
	return append(nums[:index], nums[index+1:]...)
}
```

### Explanation

We:

* Slice before index
* Slice after index
* Append together

---

# 🟠 Level 3 – Pointers & Struct Behavior

---

## 1️⃣1️⃣ Value vs Pointer Receiver

```go
type Counter struct {
	Count int
}

func (c Counter) Increment() {
	c.Count++
}

func (c *Counter) IncrementPointer() {
	c.Count++
}
```

### Why first doesn’t work?

Value receiver copies struct.

Pointer receiver modifies original.

---

## 1️⃣2️⃣ Pointer to Struct

```go
func update(u *User) {
	u.Name = "Updated"
}
```

Must pass:

```go
update(&user)
```

---

## 1️⃣3️⃣ Nil Pointer Panic

```go
var u *User
if u != nil {
	fmt.Println(u.Name)
}
```

Always check before dereferencing.

---

## 1️⃣4️⃣ Method Consistency

If one method uses pointer receiver:

Make all methods pointer receivers.

Consistency avoids confusion.

---

## 1️⃣5️⃣ Large Struct Efficiency

Passing large structs by value copies memory.

Passing pointer avoids copying.

Use pointer receiver for large structs.

---

# 🔴 Level 4 – Advanced & Tricky

---

## 1️⃣6️⃣ Slice Append Trap

```go
func add(nums []int) {
	nums = append(nums, 100)
}
```

Does not modify original because:

* Append may reallocate new array
* Slice descriptor inside function changes
* Caller slice unchanged

### Fix

```go
func add(nums []int) []int {
	return append(nums, 100)
}
```

Call:

```go
a = add(a)
```

---

## 1️⃣7️⃣ Map of Struct Trap

```go
m["u1"].Name = "New"
```

Compiler error because map returns copy.

### Fix

```go
u := m["u1"]
u.Name = "New"
m["u1"] = u
```

OR

Use pointer in map:

```go
map[string]*User
```

---

## 1️⃣8️⃣ Interface Nil Trap

```go
var u *User = nil
var i interface{} = u

fmt.Println(i == nil) // false
```

Why?

Interface stores:

* Type
* Value

Type is `*User`, value is nil.

Therefore interface is not nil.

---

## 1️⃣9️⃣ Concurrent Map Access

Concurrent writes cause:

```
fatal error: concurrent map writes
```

Maps are not thread-safe.

Must use:

* sync.Mutex
* sync.Map
* channels

(Week 5 topic)

---

## 2️⃣0️⃣ Escape Analysis

```go
func createUser() *User {
	u := User{Name: "Test"}
	return &u
}
```

This is safe.

Go performs escape analysis.

Since pointer escapes function scope:

* It allocates on heap
* Not stack

Memory is safe.

---

# 🎯 Final Takeaways

After these solutions, she should understand:

* Arrays are value types
* Slices share underlying arrays
* Maps must be initialized
* Pointer receivers modify original
* Nil behaves differently across types
* Interface nil is tricky
* Append must be returned
* Map of structs returns copies
* Concurrency + map = unsafe
* Escape analysis protects memory

---

This chapter is now strong enough to:

* Prepare the student for concurrency
* Build debugging confidence
* Eliminate beginner memory bugs

---

