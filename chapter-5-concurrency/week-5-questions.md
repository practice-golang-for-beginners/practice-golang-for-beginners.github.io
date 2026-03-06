---
layout: default
title: Post Week Questions
parent: "Chapter 5 – Concurrency: Goroutines & Channels"
nav_order: 6
---

# Week 5 – Discussion Questions

These questions are intended for the **weekly mentorship discussion session**.
They focus on building deeper intuition about Go's concurrency model rather than just writing code.

Discuss these topics and explain your reasoning clearly.

---

# Understanding Concurrency

## Question 1

Why is concurrency considered one of Go's most important features?

Discuss situations where concurrency significantly improves system performance.

---

## Question 2

Explain the difference between **concurrency** and **parallelism**.

Can a program be concurrent but not parallel?
Provide an example.

---

## Question 3

Why are **goroutines considered lightweight** compared to traditional threads?

Discuss how the Go runtime scheduler helps manage goroutines efficiently.

---

# Goroutines in Practice

## Question 4

What problems can occur if a Go program starts many goroutines without control?

Examples may include:

* memory pressure
* scheduling overhead
* system resource exhaustion

How can these issues be prevented?

---

## Question 5

When launching a goroutine, what must a developer consider to ensure the goroutine finishes correctly?

Discuss:

* synchronization
* waiting mechanisms
* lifecycle management

---

# Channels and Communication

## Question 6

Explain how **channels enable safe communication between goroutines**.

Why are channels often preferred over shared memory?

---

## Question 7

When should you use:

* **unbuffered channels**
* **buffered channels**

Provide examples where each is appropriate.

---

# Select Statement

## Question 8

What problem does the **select statement** solve?

Discuss situations where a program must wait on **multiple communication events**.

---

# Concurrency Patterns

## Question 9

Explain the **fan-out pattern**.

In what real-world systems might this pattern be used?

Examples may include:

* processing large datasets
* handling multiple API requests
* distributed job processing

---

## Question 10

Explain the **fan-in pattern**.

Why is it useful when aggregating results from multiple goroutines?

---

## Question 11

What is a **worker pool**?

Why is it important to limit the number of workers in large systems?

Provide an example where uncontrolled goroutines could cause problems.

---

# Debugging and Safety

## Question 12

What is a **race condition**?

Why can race conditions be difficult to detect in concurrent systems?

---

## Question 13

How does the Go **race detector** help identify concurrency bugs?

Why is it recommended to run tests with:

```bash
go test -race
```

---

# Design Thinking

## Question 14

Suppose you need to process **100,000 tasks** concurrently.

Would you:

* Launch 100,000 goroutines?
* Use a worker pool?

Explain your reasoning.

---

## Question 15

If multiple goroutines need to update shared data, what approaches can be used to maintain safety?

Discuss options such as:

* channels
* synchronization primitives
* avoiding shared state

---

# Reflection

After completing this chapter, reflect on the following:

* Which concurrency concept felt most intuitive?
* Which concept felt most difficult?
* What part of Go's concurrency model would you like to explore further?

These reflections can guide further learning and discussion.

---

# Next Step

After discussing these questions:

1. Review the exercise solutions.
2. Begin implementing the **Week 5 mini-project**.
3. Apply goroutines and channels in a real program.

Concurrency is a core skill for Go developers and is widely used in building scalable backend systems.
