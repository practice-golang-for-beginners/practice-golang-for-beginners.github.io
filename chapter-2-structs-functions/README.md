---
layout: default
title: Chapter 2 – Structs & Functions
parent: Chapters
nav_order: 1
has_children: true
---

## 🎯 Goal of This Chapter

This chapter teaches you how to write **structured, maintainable Go code** — the way production systems expect it.

In Python, structuring code often revolves around:
- Classes
- Dynamic typing
- Flexible imports

In Go, structuring code means:
- Explicit types
- Composition over inheritance
- Clear package boundaries
- Predictable design

By the end of this chapter, you’ll be able to write Go code that is:
✔ Organized  
✔ Idiomatic  
✔ Easy to test and maintain

---

## 🧠 What You Will Learn

### 🧩 Functions in Go

In Go:
- Types must be declared explicitly.
- Functions can return multiple values — a core Go pattern for results + error.  
  For example, Go’s `divide()` returns both a result and an error — making error handling explicit. :contentReference[oaicite:0]{index=0}

Concepts covered:
- Basic function syntax
- Multiple return values
- Named return values
- Variadic functions
- Functions as first-class values

---

### 🧱 Structs: Go’s Core Building Blocks

Go does not have traditional classes.  
Instead, it uses **structs + methods** to model real-world entities. :contentReference[oaicite:1]{index=1}

You will learn:

- How to define custom types (`struct`)
- How to instantiate and use structs
- Value semantics vs pointer semantics
- When and how to modify struct fields

Example:
```go
type User struct {
    ID    int
    Name  string
    Email string
}
````

---

### 🔁 Methods on Structs

Go attaches behavior to types by defining **methods** on them.

You will learn:

* Value receivers
* Pointer receivers
* When to use pointer receivers (modifying state, avoiding copies) ([GitHub][1])

---

### 🧬 Composition Over Inheritance

Go avoids classical inheritance.

Instead, Go uses **composition** — assembling behavior by embedding structs within structs. ([GitHub][1])

Example:

```go
type Address struct {
    City    string
    Country string
}

type Customer struct {
    Name    string
    Address
}
```

---

### 📦 Export Rules & Naming Conventions

Go enforces visibility via casing:

* `User` (capitalized) → exported (public)
* `user` (lowercase) → unexported (private) ([GitHub][1])

This replaces convention-based visibility in Python.

---

### 🗂 Packages & Project Structure

You will also learn how to create and organize code into:

* Modules (`go mod init`)
* Packages
* Clean directory structures

A basic layout:

```
project/
├── main.go
├── go.mod
└── user/
    └── user.go
```

([GitHub][1])

---

## 🔎 Idiomatic Design Principles

As a mentor, focus on:

* Keeping functions small and focused
* Avoiding unnecessary abstraction
* Prefer composition over inheritance
* Returning errors instead of panicking
* Making the zero value useful

These principles make Go code predictable and maintainable. ([GitHub][1])

---

## 🚩 Common Python → Go Mistakes

Avoid the following:
❌ Trying to simulate classes
❌ Overusing interfaces early
❌ Ignoring error values
❌ Using global variables
❌ Writing deep inheritance hierarchies

Go rewards clarity over cleverness. ([GitHub][1])

---

## 🗓 Weekly Flow

During this chapter:

* Study the concepts in `study-material.md`
* Build example programs
* Practice exercises in `exercises.md`
* Review solutions in `solutions.md`

---

## 📌 By the End of This Chapter You Should Be Able To:

✔ Write functions with multiple return values
✔ Use variadic functions and function values
✔ Define and use structs
✔ Attach methods to structs
✔ Understand value vs pointer receivers
✔ Create packages and organize code
✔ Follow Go export rules and naming conventions ([GitHub][1])

---

## 📁 Resources in This Folder

* 📄 `study-material.md` — Conceptual material
* 📘 `exercises.md` — Practice problems
* 📗 `solutions.md` — Idiomatic solutions

---

## 👨‍💻 Maintained By

**Aditya Pratap Bhuyan**
Practice GoLang for Beginners
Licensed under GNU GPL v3

