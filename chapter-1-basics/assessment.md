---
layout: default
title: Assessment
parent: Chapter 1 – Basics
nav_order: 6
---



# 📊 Chapter 1 – End of Week Assessment

**Topic:** Go Fundamentals & Tooling
**Duration:** 90–120 minutes
**Format:** Hands-on coding + short written reflection

---

# 🧪 Part 1 – Practical Coding Assessment (70%)

Create a small CLI application called:

```
basic-user-service
```

---

## ✅ Task 1 – User Struct & Display (20%)

1. Create a struct:

```go
type User struct {
    Name  string
    Age   int
    Email string
}
```

2. Create a function:

```go
func displayUser(u User)
```

It should print user details in formatted output.

✔ Must use:

* Struct
* Function
* Proper formatting

---

## ✅ Task 2 – Age Validator (15%)

Create:

```go
func validateAge(age int) error
```

Rules:

* If age < 0 → return error
* If age > 120 → return error
* Otherwise return nil

Must:

* Return proper error
* Handle it in main()

---

## ✅ Task 3 – Safe Division Utility (15%)

Reuse the `divide()` concept from exercises.

Requirements:

* Must return `(int, error)`
* Must not panic
* Must properly handle error in caller

---

## ✅ Task 4 – Loop & Classification (10%)

Write a function:

```go
func classifyNumber(n int) string
```

Return:

* "Even"
* "Odd"

Loop from 1–20 and print classification.

---

## ✅ Task 5 – Unit Tests (10%)

Write tests for:

* `validateAge`
* `divide`
* `classifyNumber`

Must include:

* At least 3 test cases per function
* At least 1 edge case
* Table-driven test style preferred

Run:

```bash
go test
```

---

# 📝 Part 2 – Conceptual Reflection (30%)

Short written answers (in a markdown file).

---

### 1️⃣ What is the difference between Python exception handling and Go error handling?

(Explain conceptually, not just syntax.)

---

### 2️⃣ What are zero values in Go and why are they useful?

---

### 3️⃣ Why does Go enforce formatting using `go fmt`?

---

### 4️⃣ What is the benefit of multiple return values?

---

# 📊 Rubric (Evaluation Criteria)

You can use this during Friday review.

---

## 🟢 1. Code Correctness (25%)

| Score | Criteria                               |
| ----- | -------------------------------------- |
| 5     | Code compiles, runs, no logical errors |
| 3     | Minor logical issues                   |
| 1     | Major issues / does not compile        |

---

## 🟢 2. Error Handling Discipline (15%)

| Score | Criteria                               |
| ----- | -------------------------------------- |
| 5     | Proper error creation and handling     |
| 3     | Errors created but not handled cleanly |
| 1     | Errors ignored                         |

---

## 🟢 3. Testing Quality (20%)

| Score | Criteria                         |
| ----- | -------------------------------- |
| 5     | Good coverage, edge cases tested |
| 3     | Basic tests only                 |
| 1     | No tests or incorrect tests      |

---

## 🟢 4. Code Readability & Style (15%)

Check:

* Proper naming
* Idiomatic structure
* go fmt compliance
* No unused variables/imports

---

## 🟢 5. Understanding of Concepts (15%)

Based on reflection answers:

* Depth of explanation
* Clarity of thought
* Comparison with Python

---

## 🟢 6. Tooling Confidence (10%)

Observe:

* Can she run `go test` independently?
* Can she initialize module?
* Can she fix compile errors without panic?

---

# 🏆 Grading Scale

| Score   | Level               |
| ------- | ------------------- |
| 85–100% | Strong Foundation   |
| 70–84%  | Good Progress       |
| 50–69%  | Needs Reinforcement |
| < 50%   | Revisit Basics      |

---

# 🎯 What This Assessment Evaluates

This is not about syntax memorization.

It evaluates:

* Thinking in Go
* Error discipline
* Test mindset
* Structural clarity
* Confidence with toolchain

---

