---
layout: default
title: Assessment
parent: Chapter 6 – Context, Timeouts & Cancellation
nav_order: 5
---

# 📝 Assessment – Context, Timeouts & Cancellation

---

## 🎯 Objective

Evaluate your ability to:

* Understand and apply `context.Context`
* Handle timeouts and cancellations correctly
* Write clean, idiomatic Go code
* Think in production scenarios

---

## 🧠 Section 1 – Conceptual Questions

### Q1. What is the purpose of `context.Context` in Go?

Explain in your own words.

---

### Q2. Difference between:

* `context.WithCancel`
* `context.WithTimeout`
* `context.WithDeadline`

---

### Q3. Why should context be passed as the **first parameter**?

---

### Q4. What happens if you do not call `cancel()`?

---

### Q5. Why is storing context in a struct considered bad practice?

---

### Q6. When should you NOT use context?

---

## 💻 Section 2 – Code Writing

### Q7. Write a function:

```go
func process(ctx context.Context) error
```

Requirements:

* Simulate work for 3 seconds
* Return early if context is cancelled
* Return appropriate error

---

### Q8. Create a program:

* Start a goroutine
* Use `context.WithCancel`
* Stop the goroutine after 2 seconds

---

### Q9. Implement timeout handling:

* Create a function that runs for 5 seconds
* Use a 2-second timeout
* Print whether it completed or timed out

---

## 🐞 Section 3 – Debugging

### Q10. Identify the issue:

```go
func badWorker(ctx context.Context) {
	for {
		fmt.Println("Working...")
		time.Sleep(time.Second)
	}
}
```

👉 What is wrong? Fix it.

---

### Q11. Identify the issue:

```go
ctx, _ := context.WithTimeout(context.Background(), 2*time.Second)
```

👉 What is missing? Why is it important?

---

## 🧩 Section 4 – Scenario-Based

### Q12. API Call Scenario

You are calling an external API that may:

* Take too long
* Fail intermittently

👉 How will you:

* Use context to control it?
* Avoid system slowdown?

---

### Q13. Microservice Chain

Service A → Service B → Database

👉 How should context flow across services?

---

### Q14. Goroutine Leak Scenario

You start multiple goroutines but forget to handle context.

👉 What problems can arise?

---

## ⭐ Bonus Challenge

### Q15. Design a retry mechanism:

* Retry every 1 second
* Stop if:

  * Context is cancelled
  * Operation succeeds

Explain your approach.

---

## 📊 Evaluation Criteria

| Area           | What is Expected                     |
| -------------- | ------------------------------------ |
| Concepts       | Clear understanding of context usage |
| Code Quality   | Clean, idiomatic Go                  |
| Error Handling | Proper use of `ctx.Err()`            |
| Concurrency    | Safe and controlled                  |
| Thinking       | Real-world awareness                 |

---

## 🚀 Completion Criteria

You are considered **proficient in this chapter** if you can:

* Correctly implement cancellation and timeouts
* Write context-aware functions
* Avoid common mistakes
* Explain concepts confidently

---

## 💬 Discussion (To be reviewed with mentor)

* What was the most confusing part?
* Where do you see context being used in real projects?
* How is this different from Python?

---

👉 Next: [Weekly Questions](./week-6-questions)

---
