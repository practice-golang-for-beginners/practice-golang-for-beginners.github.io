---
layout: default
title: Weekly Discussion Questions
parent: Chapter 7 – HTTP Services
nav_order: 6
---

# 💬 Week 7 – Discussion Questions (HTTP Services)

These questions are designed for **mentor-led discussions**.
They focus on **deep understanding, design thinking, and real-world application** of Go HTTP services.

---

## 🎯 Discussion Goals

* Strengthen conceptual clarity
* Encourage design thinking
* Understand real-world trade-offs
* Build confidence in backend development

---

# 🧠 Section 1 – Core Understanding

### 1. Why does Go rely heavily on the standard library (`net/http`) instead of frameworks?

* What are the advantages?
* What are the trade-offs compared to frameworks like Flask/Django (Python)?

---

### 2. What makes a Go HTTP handler “idiomatic”?

* What should a good handler **not** do?
* Where should business logic live?

---

### 3. Why is explicit error handling important in Go APIs?

* How does it impact readability and maintainability?
* Compare with exception-based handling in Python

---

# 🧩 Section 2 – Design Thinking

### 4. How would you design a scalable HTTP service in Go?

Discuss:

* Project structure
* Separation of concerns
* Dependency injection

---

### 5. When should you use middleware vs adding logic inside handlers?

Examples:

* Logging
* Authentication
* Request validation

---

### 6. How would you structure a REST API for long-term maintainability?

---

# 🔄 Section 3 – Real-World Scenarios

### 7. Your API is returning inconsistent responses. How would you debug this?

---

### 8. A client reports that your service is slow. What steps would you take?

Think about:

* Logging
* Profiling
* Bottlenecks

---

### 9. How would you handle high traffic in a Go HTTP service?

---

# 🧪 Section 4 – Testing & Quality

### 10. What makes a test “good” in Go?

* What should you test?
* What should you avoid testing?

---

### 11. Why are table-driven tests preferred in Go?

---

### 12. How do interfaces improve testability?

---

# 🏗️ Section 5 – Production Readiness

### 13. What are the key components of a production-ready HTTP service?

Examples:

* Logging
* Timeouts
* Graceful shutdown
* Monitoring

---

### 14. Why is graceful shutdown important?

* What could go wrong without it?

---

### 15. What would you improve in your mini-project to make it production-ready?

---

# 💡 Reflection Questions

* What was the most challenging concept in this chapter?
* What felt different compared to Python?
* What would you do differently if you rewrote your code?

---

## 🧑‍🏫 Mentor Notes

* Encourage open discussion (no “right” answers)
* Focus on reasoning, not memorization
* Ask follow-up questions:

  * “Why?”
  * “What trade-off?”
  * “What happens at scale?”

---

## 📌 Final Thought

> “Good backend engineers don’t just write code—they design systems that work under real-world conditions.”

---
