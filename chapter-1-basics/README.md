---
layout: default
title: Chapter 1 – Basics
parent: Chapters
nav_order: 1
has_children: true
---


## 📘 Chapter Overview

Welcome to your first step into Go 🚀

In this chapter, you will learn the **fundamental building blocks of Go programming**.  
Everything you learn here will be used throughout the rest of the course.

This chapter focuses on understanding how Go programs are structured and how basic logic works.

---

## 🎯 What You Will Learn

By the end of this chapter, you will be able to:

- ✅ Understand Go program structure
- ✅ Declare and use variables
- ✅ Work with basic data types
- ✅ Use constants
- ✅ Take input and print output
- ✅ Write conditional statements (`if`, `switch`)
- ✅ Use loops (`for`)
- ✅ Write simple functions

---

## 🧠 Core Concepts Covered

### 1️⃣ Program Structure

Understanding:

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, Go!")
}
```

- `package main`
- `import`
- `func main()`

---

### 2️⃣ Variables and Data Types

- `int`
- `float64`
- `string`
- `bool`

Examples:

```go
var age int = 25
name := "Alice"
```

---

### 3️⃣ Constants

```go
const Pi = 3.14
```

---

### 4️⃣ Control Flow

#### ✅ If Statement

```go
if age > 18 {
    fmt.Println("Adult")
}
```

#### ✅ Switch Statement

```go
switch day {
case "Monday":
    fmt.Println("Start of week")
}
```

---

### 5️⃣ Loops (Go Has Only One Loop – `for`)

```go
for i := 0; i < 5; i++ {
    fmt.Println(i)
}
```

While-style loop:

```go
for condition {
    // logic
}
```

---

### 6️⃣ Functions (Introduction)

```go
func add(a int, b int) int {
    return a + b
}
```

---

## 🚀 Why This Chapter Matters

This chapter builds your:

- Logical thinking
- Syntax familiarity
- Confidence writing Go programs

Without mastering basics, advanced topics like:
- Structs
- Concurrency
- HTTP services
- Production systems

will feel overwhelming.

---

## ✅ Before Moving to Chapter 2

Make sure you can:

- Write a Go program without looking at examples
- Use conditionals confidently
- Write loops without confusion
- Define and call simple functions

If not — practice more exercises before proceeding.

---

## 🏁 Next Step

Continue to:

➡ **Study Material**  
➡ **Exercises**  
➡ **Solutions**

Take your time. Strong foundations matter.
