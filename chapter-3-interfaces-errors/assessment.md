---
layout: default
title: Assessment
parent: Chapter 3 – Interfaces & Errors
nav_order: 6
---

# 📘 Week 3 – End of Week Assessment

**Chapter: Interfaces & Error Handling**

---

# 🎯 Assessment Objective

Evaluate the learner’s ability to:

* Design and implement small interfaces
* Use implicit interface satisfaction correctly
* Handle and wrap errors idiomatically
* Create and detect custom error types
* Write clean, structured unit tests
* Apply interface-based mocking

---

# 🧠 Assessment Structure

Total Duration: 2–3 hours
Total Marks: 100

---

# 🧩 Part 1 – Conceptual Understanding (20 Marks)

### Q1 (10 Marks)

Explain in your own words:

1. What does implicit interface implementation mean in Go?
2. How is it similar to Python’s duck typing?
3. What advantage does Go provide over Python in this context?

Evaluation Criteria:

* Clarity
* Correct terminology
* Understanding of compile-time safety

---

### Q2 (10 Marks)

Answer briefly:

* Why are small interfaces preferred in Go?
* Why should we avoid comparing error strings?
* When should errors be wrapped?

---

# 💻 Part 2 – Design & Implementation (40 Marks)

## Problem Statement

Design a small **Payment Processing System**.

### Requirements

1. Define an interface:

```go
type PaymentProcessor interface {
	Process(amount float64) error
}
```

2. Implement:

   * `CreditCardProcessor`
   * `MockProcessor`

3. Create a service:

```go
type PaymentService struct {
	processor PaymentProcessor
}
```

4. Implement:

```go
func (p *PaymentService) MakePayment(amount float64) error
```

### Rules

* If amount <= 0 → return custom `ValidationError`
* If processor fails → wrap the error
* Do not compare error strings
* Follow idiomatic Go patterns

---

# 🧪 Part 3 – Testing (30 Marks)

Write unit tests that:

1. Test successful payment
2. Test validation failure (amount <= 0)
3. Test wrapped processor failure
4. Use:

   * `errors.Is`
   * `errors.As`
5. Use table-driven testing for at least one test

---

# 🧠 Part 4 – Code Review Reflection (10 Marks)

Answer:

* What would happen if we injected a concrete type instead of an interface?
* Where should logging ideally happen?
* What common error-handling mistake did you avoid?

---

# 📊 Evaluation Rubric (100 Marks)

---

## 🟢 1. Interface Design (20 Marks)

| Criteria                | Excellent (16–20) | Good (10–15)     | Needs Improvement (0–9) |
| ----------------------- | ----------------- | ---------------- | ----------------------- |
| Interface size          | Small & focused   | Slightly bloated | Large, tightly coupled  |
| Implicit implementation | Correct           | Minor mistakes   | Incorrect usage         |
| Dependency injection    | Clean & decoupled | Some coupling    | Direct concrete usage   |

---

## 🟡 2. Error Handling (25 Marks)

| Criteria         | Excellent (20–25)      | Good (12–19)     | Needs Improvement (0–11) |
| ---------------- | ---------------------- | ---------------- | ------------------------ |
| Validation error | Custom structured type | Basic error      | String comparison        |
| Error wrapping   | Uses `%w` correctly    | Partial wrapping | No wrapping              |
| Error comparison | `errors.Is/As` used    | Inconsistent     | String matching          |

---

## 🟠 3. Code Quality (20 Marks)

| Criteria       | Excellent (16–20)  | Good (10–15)                | Needs Improvement (0–9)  |
| -------------- | ------------------ | --------------------------- | ------------------------ |
| Naming         | Clear & idiomatic  | Acceptable                  | Poor naming              |
| Structure      | Logical & readable | Minor clutter               | Confusing                |
| Go conventions | Idiomatic          | Some non-idiomatic patterns | Python-style translation |

---

## 🔵 4. Unit Testing (25 Marks)

| Criteria          | Excellent (20–25)          | Good (12–19)       | Needs Improvement (0–11) |
| ----------------- | -------------------------- | ------------------ | ------------------------ |
| Coverage          | Success + failure paths    | Missing edge cases | Only happy path          |
| Mock usage        | Clean interface-based mock | Basic mock         | No mocking               |
| Error assertions  | Proper Is/As               | Mixed approach     | String compare           |
| Table-driven test | Present                    | Partial            | Not used                 |

---

## 🟣 5. Conceptual Clarity (10 Marks)

| Criteria      | Excellent (8–10)  | Good (5–7)     | Needs Improvement (0–4) |
| ------------- | ----------------- | -------------- | ----------------------- |
| Understanding | Deep and accurate | Mostly correct | Superficial             |

---

# 🏆 Grading Interpretation

| Score  | Meaning                                               |
| ------ | ----------------------------------------------------- |
| 85–100 | Production-ready understanding                        |
| 70–84  | Strong fundamentals, minor refinement needed          |
| 50–69  | Conceptual understanding present, implementation weak |
| <50    | Needs reinforcement before progressing                |

---

# 👨‍🏫 Mentor Notes (For You – Optional Use)

During Friday review, observe:

* Does the student thinks in interfaces or concrete types?
* Does the student propagate errors upward cleanly?
* Does the student compare error strings? (Immediate correction)
* Does the student understand why wrapping matters?
* Does the student understand mocking vs real implementation?

Ask:

> “If this were a production payment system, what would you improve?”

That question separates syntax learners from engineers.

---

# 🎯 End-of-Week 3 Milestone

If the student scores 80+:

The student is beginning to think like a Go engineer.

If the student struggles:

Repeat:

* Error wrapping
* `errors.Is` / `errors.As`
* Interface-based design

Week 3 is the architectural maturity checkpoint.

---


