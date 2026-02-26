---
layout: default
title: Assessment
parent: Chapter 2 – Structs & Functions
nav_order: 6
---


# Chapter 2 – End of Week Assessment
## Functions, Structs & Packages

### Objective

This assessment evaluates your understanding of:

- Function design
- Multiple return values
- Struct modeling
- Pointer vs value receivers
- Constructors
- Package organization
- Export rules
- Encapsulation

You are expected to write clean, idiomatic, testable Go code.

---

# Part 1 – Function Design (20 Marks)

1. Write a function:

   CalculateDiscount(price float64, percent float64) (float64, error)

Requirements:
- Return error if percent < 0 or > 100
- No panic
- Clean error messages

2. Write table-driven tests for it.

---

# Part 2 – Struct & Methods (30 Marks)

Design a struct:

Product
- ID (int)
- Name (string)
- Price (float64)
- stock (int)   // must NOT be exported

Requirements:

1. Create constructor:
   NewProduct(id int, name string, price float64, stock int) (*Product, error)

2. Add methods:
   - UpdatePrice(newPrice float64) error
   - AddStock(qty int)
   - Purchase(qty int) error
   - InStock() bool

Rules:
- Price cannot be negative
- Purchase cannot exceed stock
- stock must remain private

---

# Part 3 – Pointer vs Value Understanding (15 Marks)

Write two methods:

- One using value receiver
- One using pointer receiver

Demonstrate (in main.go) the behavioral difference.

Explain in comments what happened and why.

---

# Part 4 – Package Design (20 Marks)

Create a package:

inventory/

Move Product struct and logic into that package.

In main.go:

- Import inventory
- Create product
- Perform operations
- Print results

Follow:
- Proper export rules
- Clean package naming
- No circular imports

---

# Part 5 – Code Quality & Design (15 Marks)

Ensure:

- No unnecessary global variables
- Small functions
- Proper error handling
- No ignored errors
- Idiomatic formatting (`go fmt`)

---

# Submission Requirements

- Proper folder structure
- go.mod included
- Tests passing
- Code compiles without warnings
- No unused variables


---

# 📊 rubric.md


# Chapter 2 – Evaluation Rubric

Total: 100 Marks

---

## 1. Function Design (20 Marks)

| Criteria | Marks |
|----------|-------|
| Correct signature | 4 |
| Proper validation logic | 5 |
| Idiomatic error handling | 5 |
| Table-driven tests | 4 |
| Edge cases covered | 2 |

---

## 2. Struct & Methods Design (30 Marks)

| Criteria | Marks |
|----------|-------|
| Proper struct modeling | 5 |
| Constructor validation | 5 |
| Pointer receivers used correctly | 6 |
| Encapsulation maintained | 5 |
| Correct stock management logic | 5 |
| Clean method separation | 4 |

---

## 3. Pointer vs Value Clarity (15 Marks)

| Criteria | Marks |
|----------|-------|
| Correct demonstration | 5 |
| Clear explanation in comments | 5 |
| Correct reasoning about memory semantics | 5 |

---

## 4. Package Organization (20 Marks)

| Criteria | Marks |
|----------|-------|
| Proper folder structure | 5 |
| Correct export usage | 5 |
| Clean imports | 5 |
| No circular dependencies | 5 |

---

## 5. Code Quality (15 Marks)

| Criteria | Marks |
|----------|-------|
| Idiomatic Go style | 5 |
| No ignored errors | 4 |
| go fmt applied | 2 |
| Clear naming | 2 |
| Readability & simplicity | 2 |

---

# Performance Bands

| Score | Level |
|-------|--------|
| 90–100 | Strong production-ready understanding |
| 75–89  | Solid grasp with minor gaps |
| 60–74  | Conceptually correct but needs refinement |
| <60    | Needs revision before moving ahead |

---

# Mentor Notes (Not Shared Publicly)

Look for:

- Does she overuse value receivers?
- Does she ignore errors?
- Does she understand encapsulation?
- Is the constructor meaningful or superficial?
- Is testing thoughtful or mechanical?

The goal is not perfection — it is clarity of thinking.
