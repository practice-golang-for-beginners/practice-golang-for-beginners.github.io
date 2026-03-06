---
layout: default
title: Assessment
parent: "Chapter 5 – Concurrency: Goroutines & Channels"
nav_order: 4
---

# Assessment – Concurrency in Go

This assessment evaluates your understanding of Go's concurrency model, including **goroutines**, **channels**, and common concurrency patterns.

Answer the following questions without referring to the solutions first. The goal is to assess both **conceptual understanding** and **practical reasoning**.

---

# Section 1 – Conceptual Understanding

## Question 1

What is the difference between **concurrency** and **parallelism**?

Explain in your own words and provide an example where a system may be concurrent but not parallel.

---

## Question 2

What is a **goroutine**?

Explain:

* How goroutines differ from traditional operating system threads.
* Why Go programs can run thousands of goroutines efficiently.

---

## Question 3

How does Go's concurrency philosophy differ from traditional multithreaded programming?

Explain the meaning of the statement:

> "Do not communicate by sharing memory; share memory by communicating."

---

## Question 4

What is a **channel** in Go?

Explain:

* What channels are used for
* How they help coordinate goroutines
* Why they are considered safe for concurrent communication

---

## Question 5

What is the difference between **buffered** and **unbuffered** channels?

Explain:

* When a sender blocks
* When a receiver blocks
* When buffered channels are useful

---

# Section 2 – Goroutines and Channels

## Question 6

Consider the following code:

```go
func main() {
    go fmt.Println("Hello from goroutine")
}
```

What potential issue exists in this program?

Explain why it occurs and how it can be fixed.

---

## Question 7

What happens if a goroutine attempts to:

* Send to a channel that has no receiver?
* Receive from a channel that has no sender?

Explain the behavior.

---

## Question 8

What is the purpose of the **select** statement?

Provide an example scenario where `select` is useful.

---

# Section 3 – Concurrency Patterns

## Question 9

Explain the **fan-out pattern**.

When would this pattern be useful in real-world systems?

Provide an example.

---

## Question 10

Explain the **fan-in pattern**.

How does it help aggregate results from multiple goroutines?

---

## Question 11

What is a **worker pool**?

Why is it important to limit the number of concurrently executing workers in some systems?

Provide a practical example.

---

# Section 4 – Debugging and Safety

## Question 12

What is a **race condition**?

Provide an example scenario where race conditions may occur in concurrent programs.

---

## Question 13

How does Go detect race conditions?

Explain how to use the **Go race detector**.

Example command:

```bash
go test -race
```

---

## Question 14

What is a **goroutine leak**?

Explain why goroutine leaks are dangerous in long-running systems.

---

# Section 5 – Practical Design Thinking

## Question 15

You need to process **10,000 tasks concurrently** in a Go program.

Would you:

A) Start 10,000 goroutines directly
B) Use a worker pool with a fixed number of workers

Explain which approach is better and why.

---

## Question 16

Suppose you are building a service that processes incoming requests concurrently.

Which of the following Go features would you likely use?

* Goroutines
* Channels
* Worker pools
* Context cancellation

Explain the role of each component.

---

# Reflection

After completing this assessment, reflect on the following:

* Which concurrency concept felt most intuitive?
* Which concept was most confusing?
* What patterns would you like to practice more?

Discuss these during the **weekly review session** with your mentor.

---

# Next Steps

After completing the assessment:

1. Review the **solutions** provided for the exercises.
2. Discuss the **Week 5 questions** during the mentorship review.
3. Begin implementing the **mini-project** introduced in this chapter.

Mastering concurrency is a major milestone in becoming proficient in Go.
