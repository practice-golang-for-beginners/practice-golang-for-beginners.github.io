---
layout: default
title: Assesment
parent: Chapter 4 – Collections, Pointers & Memory
nav_order: 6
---



# Week 4 – End of Week Assessment

Collections, Pointers & Memory

---

# 🎯 Objective

This assessment evaluates whether you:

* Truly understand Go’s memory model
* Can reason about value vs reference semantics
* Can avoid slice and map pitfalls
* Can safely use pointers
* Can handle nil correctly
* Can write testable and predictable code

This is not about syntax.

This is about **mental models and correctness**.

---

# 🧪 Assessment Structure

Total Duration: 90–120 minutes
Total Marks: 100

Sections:

1. Conceptual Questions (20 marks)
2. Code Reasoning (30 marks)
3. Implementation Task (40 marks)
4. Testing & Edge Cases (10 marks)

---

# Section 1 – Conceptual Understanding (20 Marks)

Answer briefly but clearly.

### 1️⃣ Why are arrays rarely used in production Go code? (5 marks)

Explain:

* Type behavior
* Copy semantics
* Practical limitation

---

### 2️⃣ What happens internally when `append()` exceeds slice capacity? (5 marks)

Explain:

* Reallocation
* Copy
* Why returning slice is important

---

### 3️⃣ Why does writing to a nil map panic, but reading does not? (5 marks)

---

### 4️⃣ Explain the interface nil trap with a short example. (5 marks)

---

# Section 2 – Code Reasoning (30 Marks)

For each snippet:

* Predict output
* Explain why

---

### 1️⃣ Slice Sharing (10 marks)

```go
a := []int{1, 2, 3}
b := a[:2]
b[0] = 100
fmt.Println(a)
```

---

### 2️⃣ Append Trap (10 marks)

```go
func add(nums []int) {
    nums = append(nums, 10)
}

func main() {
    a := []int{1, 2}
    add(a)
    fmt.Println(a)
}
```

---

### 3️⃣ Map of Struct Trap (10 marks)

```go
type User struct {
    Name string
}

m := map[string]User{
    "u1": {Name: "Old"},
}

m["u1"].Name = "New"
```

What happens and why?

---

# Section 3 – Implementation Task (40 Marks)

## 🔧 Build a Safe In-Memory User Store

Implement:

```go
type User struct {
    ID   string
    Name string
}

type UserStore struct {
    users map[string]*User
}
```

---

### Requirements

Implement the following methods:

```go
func NewUserStore() *UserStore
```

* Must initialize the map properly

---

```go
func (s *UserStore) AddUser(id, name string) error
```

Rules:

* Do not allow duplicate ID
* Return error if ID exists
* Use pointer receivers

---

```go
func (s *UserStore) GetUser(id string) (*User, error)
```

Rules:

* Return error if user does not exist
* Must not panic

---

```go
func (s *UserStore) DeleteUser(id string) error
```

Rules:

* Handle non-existing user safely

---

### Important

* Avoid nil map panic
* Avoid returning struct copies unintentionally
* Use pointer receivers consistently
* Handle errors properly

---

# Section 4 – Testing & Edge Cases (10 Marks)

Write unit tests that verify:

* Duplicate user cannot be added
* Getting non-existing user returns error
* Deleting non-existing user returns error
* Modifying returned user updates store (pointer behavior check)

---

# 🏆 Grading Rubric (100 Marks)

---

## Section 1 – Conceptual (20 Marks)

| Criteria                                     | Marks |
| -------------------------------------------- | ----- |
| Clear explanation of value semantics         | 5     |
| Correct understanding of append reallocation | 5     |
| Accurate map nil explanation                 | 5     |
| Interface nil trap correctly explained       | 5     |

Deduct marks for:

* Vague answers
* Memorized but incorrect reasoning
* Missing memory explanation

---

## Section 2 – Code Reasoning (30 Marks)

| Criteria                                 | Marks |
| ---------------------------------------- | ----- |
| Correct output prediction                | 10    |
| Correct reasoning about memory           | 10    |
| Correct identification of compiler error | 10    |

Full marks only if explanation includes:

* Backing array behavior
* Descriptor copy
* Map returns copy behavior

---

## Section 3 – Implementation (40 Marks)

| Area                                | Marks |
| ----------------------------------- | ----- |
| Map properly initialized            | 5     |
| Pointer receiver usage correct      | 5     |
| Duplicate handling correct          | 5     |
| Error handling clean and consistent | 5     |
| No panic risk                       | 5     |
| No unnecessary struct copying       | 5     |
| Clean readable code                 | 5     |
| Idiomatic Go patterns               | 5     |

Major deductions for:

* Nil map panic risk
* Value receivers where pointer needed
* Returning struct instead of pointer
* Ignoring error handling

---

## Section 4 – Testing (10 Marks)

| Criteria                      | Marks |
| ----------------------------- | ----- |
| Edge cases covered            | 5     |
| Proper use of testing package | 5     |

Bonus credit if:

* Uses table-driven tests
* Uses `t.Run()`

---

# 🧠 What a High Score Looks Like (85+)

* Clear mental model
* No accidental copying
* Clean error patterns
* Correct pointer semantics
* Safe map usage
* Tests that think about edge cases

---

# 🚨 Red Flags (Needs Improvement)

* Confuses slice with reference type
* Uses value receivers randomly
* Writes to nil map
* Cannot explain append behavior
* Ignores nil checks
* No understanding of interface nil trap

---

# 🎓 Outcome of Passing Week 4

If score ≥ 80:

Ready for:
➡ Week 5 – Concurrency & Goroutines

If score < 70:

Recommend:

* Revisit slice backing array behavior
* Practice pointer receivers
* Redo map exercises

---

# Mentor Notes (Optional – For You)

When reviewing:

Focus on:

* How she explains things
* Whether she truly understands memory
* Whether she writes defensive Go
* Whether she avoids unsafe assumptions

Understanding Week 4 deeply determines how safe she will be in concurrency.

---


