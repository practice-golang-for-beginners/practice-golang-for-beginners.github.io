---
layout: default
title: "Chapter 5 – Concurrency: Goroutines & Channels"
parent: Chapters
nav_order: 5
has_children: true
---

Concurrency is one of the most powerful and defining features of the Go programming language. Unlike many traditional languages where concurrency is complex and error-prone, Go provides a simple and elegant model based on **goroutines** and **channels**.

This chapter introduces Go’s concurrency model and helps build the mental framework required to design concurrent systems effectively.

Developers transitioning from languages such as **Python or Java** will notice a significant shift in how concurrency is implemented and managed in Go.

---

# Learning Goal

The goal of this chapter is to build strong intuition about Go’s concurrency model and enable you to write safe and scalable concurrent programs.

By the end of this chapter you should be able to:

- Understand how Go manages concurrent execution
- Write programs using goroutines
- Communicate safely between goroutines using channels
- Coordinate multiple concurrent operations using `select`
- Implement common concurrency design patterns
- Detect and avoid race conditions

---

# Topics Covered

This chapter covers the following concepts:

- Goroutines and lightweight concurrency
- Goroutines vs Python threads
- Channels
- Buffered vs unbuffered channels
- The `select` statement
- Concurrency design patterns
- Common concurrency anti-patterns
- Testing concurrent programs
- Race detection using Go tooling

---

# Chapter Contents

The learning material for this chapter is organized into the following sections:

| Section | Description |
|------|------|
| Study Material | In-depth explanation of Go concurrency concepts |
| Test Guide | How to write and execute concurrency-safe tests |
| Exercises | Hands-on programming tasks to practice concurrency |
| Assessment | Conceptual questions to evaluate understanding |
| Solutions | Reference implementations for exercises |
| Week 5 Questions | Discussion questions for mentorship sessions |

---

# Navigation

Use the links below to navigate through the chapter:

- [Study Material](study-material.md)
- [Test Guide](test.md)
- [Exercises](exercises.md)
- [Assessment](assessment.md)
- [Solutions](solutions.md)
- [Week 5 Questions](week-5-questions.md)

---

# Mini Project (Introduced in Week 5)

Starting from this chapter, you will begin applying concepts through a **mini project** that evolves over multiple weeks.

The project will involve building a **concurrent data processing system** using goroutines, channels, and worker pools.

Through this project you will learn how Go concurrency is used in real-world systems such as:

- distributed systems
- microservices
- data pipelines
- cloud-native applications

---

# Prerequisites

Before starting this chapter you should be comfortable with:

- Go syntax and program structure
- Functions and methods
- Structs and interfaces
- Collections (slices and maps)
- Pointers and memory behavior

These topics were covered in **Chapter 4 – Collections, Pointers & Memory**.

---

# Estimated Time

Recommended time to complete this chapter:

**6–10 hours**

Suggested breakdown:

- Study Material → 2–3 hours  
- Exercises → 2–3 hours  
- Assessment → 1 hour  
- Mini Project Work → 2–3 hours  

---

# Author

Maintained by

**Aditya Pratap Bhuyan**  
Senior Software Engineer  
IBM

LinkedIn:  
https://linkedin.com/in/adityabhuyan

---

# License

This project is licensed under the **GNU General Public License v3.0 (GPL-3.0)**.

You are free to use, modify, and distribute this material under the terms of the license.

---

