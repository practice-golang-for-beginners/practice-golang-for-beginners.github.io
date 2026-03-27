---
layout: default
title: Weekly Questions
parent: Chapter 6 – Context, Timeouts & Cancellation
nav_order: 6
---

# 💬 Weekly Questions – Context, Timeouts & Cancellation

---

## 🎯 Objective

These questions are meant to:

* Encourage deeper thinking
* Strengthen conceptual clarity
* Drive meaningful mentor discussions
* Connect learning to real-world systems

---

## 🧠 Conceptual Reflection

### 1. Why does Go require explicit context passing instead of implicit handling (like Python frameworks)?

👉 What are the advantages and trade-offs?

---

### 2. Context is often described as “request-scoped data + control signals”.

👉 Why is mixing business data into context discouraged?

---

### 3. What is the difference between:

* Cancellation
* Timeout
* Deadline

👉 When would you use each?

---

### 4. Why is `ctx.Done()` implemented as a channel instead of a boolean flag?

---

## 🔍 Code Thinking

### 5. If a function does NOT respect `ctx.Done()`, what problems can occur?

Think in terms of:

* System performance
* Resource usage
* User experience

---

### 6. What happens if you pass `context.Background()` everywhere instead of propagating context?

---

### 7. Why should `cancel()` always be called, even when using timeouts?

---

## 🌐 Real-World Scenarios

### 8. In an HTTP service:

* What happens when a client disconnects?
* How does context help?

---

### 9. Imagine a slow database query:

👉 How would context help prevent system degradation?

---

### 10. In a microservices architecture:

👉 Why is context propagation critical across services?

---

## ⚖️ Design Thinking

### 11. When designing a function:

👉 How do you decide whether it should accept a context?

---

### 12. What are the risks of overusing context?

---

### 13. How would you design a retry mechanism that respects context?

---

## 🧩 Advanced Reflection

### 14. Compare Go’s context model with:

* Python async/await cancellation
* Java thread interruption

👉 Which do you find clearer and why?

---

### 15. What are common mistakes engineers make when first using context?

---

## 💬 Mentor Discussion Prompts

Use these during your Friday session:

* What part of context felt unintuitive?
* Which exercise was most challenging?
* Can you explain context to another developer?
* Where can we apply this in our current projects?

---

## 🚀 Outcome

By the end of this discussion, you should:

* Think beyond syntax
* Understand real-world implications
* Be confident explaining context to others

---

👉 Next: [Mini Project](./mini-project)

---
