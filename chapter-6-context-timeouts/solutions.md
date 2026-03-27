---
layout: default
title: Solutions
parent: Chapter 6 – Context, Timeouts & Cancellation
nav_order: 3
---

# ✅ Solutions – Context, Timeouts & Cancellation

---

## 🟢 Level 1 – Basics

### ✅ Exercise 1: Basic Context Creation

```go
package main

import (
	"context"
	"fmt"
)

func greet(ctx context.Context) {
	fmt.Println("Hello from function")
}

func main() {
	ctx := context.Background()
	greet(ctx)
}
```

🧠 Key Learning:

* Context is passed explicitly
* Even if unused, it establishes pattern early

---

### ✅ Exercise 2: Manual Cancellation

```go
package main

import (
	"context"
	"fmt"
	"time"
)

func worker(ctx context.Context) {
	for {
		select {
		case <-ctx.Done():
			fmt.Println("Worker stopped:", ctx.Err())
			return
		default:
			fmt.Println("Working...")
			time.Sleep(time.Second)
		}
	}
}

func main() {
	ctx, cancel := context.WithCancel(context.Background())

	go worker(ctx)

	time.Sleep(3 * time.Second)
	cancel()

	time.Sleep(1 * time.Second)
}
```

🧠 Key Learning:

* Always listen to `ctx.Done()`
* Graceful shutdown pattern

---

### ✅ Exercise 3: Timeout Context

```go
package main

import (
	"context"
	"fmt"
	"time"
)

func task(ctx context.Context) error {
	select {
	case <-time.After(5 * time.Second):
		fmt.Println("Task completed")
		return nil
	case <-ctx.Done():
		return ctx.Err()
	}
}

func main() {
	ctx, cancel := context.WithTimeout(context.Background(), 2*time.Second)
	defer cancel()

	err := task(ctx)
	fmt.Println(err)
}
```

🧠 Key Learning:

* Timeout automatically cancels context
* Always return `ctx.Err()`

---

## 🟡 Level 2 – Intermediate

### ✅ Exercise 4: Respect Cancellation

```go
func longTask(ctx context.Context) {
	for i := 1; i <= 10; i++ {
		select {
		case <-ctx.Done():
			fmt.Println("Cancelled:", ctx.Err())
			return
		default:
			fmt.Println("Step", i)
			time.Sleep(time.Second)
		}
	}
}
```

🧠 Key Learning:

* Never block blindly
* Always check cancellation inside loops

---

### ✅ Exercise 5: Context Propagation

```go
func repository(ctx context.Context) {
	select {
	case <-time.After(2 * time.Second):
		fmt.Println("Repository done")
	case <-ctx.Done():
		fmt.Println("Repo cancelled")
	}
}

func service(ctx context.Context) {
	repository(ctx)
}

func main() {
	ctx, cancel := context.WithTimeout(context.Background(), time.Second)
	defer cancel()

	service(ctx)
}
```

🧠 Key Learning:

* Context flows top → down
* No global context

---

### ✅ Exercise 6: Timeout Wrapper

```go
func executeWithTimeout(parent context.Context) {
	ctx, cancel := context.WithTimeout(parent, 2*time.Second)
	defer cancel()

	select {
	case <-time.After(3 * time.Second):
		fmt.Println("Completed")
	case <-ctx.Done():
		fmt.Println("Timeout:", ctx.Err())
	}
}
```

🧠 Key Learning:

* Derived contexts control scope
* Always `defer cancel()`

---

## 🔵 Level 3 – Real-World Scenarios

### ✅ Exercise 7: Worker with Cancellation

```go
func worker(ctx context.Context) {
	for {
		select {
		case <-ctx.Done():
			fmt.Println("Worker shutting down...")
			return
		default:
			fmt.Println("Processing job...")
			time.Sleep(time.Second)
		}
	}
}
```

🧠 Key Learning:

* Long-running workers must be stoppable

---

### ✅ Exercise 8: API Simulation

```go
func callAPI(ctx context.Context) error {
	select {
	case <-time.After(3 * time.Second):
		fmt.Println("API Success")
		return nil
	case <-ctx.Done():
		return ctx.Err()
	}
}
```

🧠 Key Learning:

* Always make external calls context-aware

---

### ✅ Exercise 9: Multiple Goroutines

```go
func worker(ctx context.Context, id int) {
	for {
		select {
		case <-ctx.Done():
			fmt.Println("Worker", id, "stopped")
			return
		default:
			fmt.Println("Worker", id, "running")
			time.Sleep(time.Second)
		}
	}
}

func main() {
	ctx, cancel := context.WithCancel(context.Background())

	for i := 1; i <= 3; i++ {
		go worker(ctx, i)
	}

	time.Sleep(2 * time.Second)
	cancel()

	time.Sleep(1 * time.Second)
}
```

🧠 Key Learning:

* Shared context = coordinated shutdown

---

## 🔴 Level 4 – Advanced

### ✅ Exercise 10: Prevent Goroutine Leak

```go
func safeWorker(ctx context.Context) {
	go func() {
		for {
			select {
			case <-ctx.Done():
				fmt.Println("Clean exit")
				return
			default:
				time.Sleep(time.Second)
			}
		}
	}()
}
```

🧠 Key Learning:

* Every goroutine must have exit condition

---

### ✅ Exercise 11: Context + Channel

```go
func process(ctx context.Context, ch <-chan int) {
	for {
		select {
		case val := <-ch:
			fmt.Println("Received:", val)
		case <-ctx.Done():
			fmt.Println("Stopped processing")
			return
		}
	}
}
```

🧠 Key Learning:

* Combine channels + context safely

---

### ✅ Exercise 12: HTTP Context

```go
func handler(w http.ResponseWriter, r *http.Request) {
	ctx := r.Context()

	select {
	case <-time.After(3 * time.Second):
		fmt.Fprintln(w, "Success")
	case <-ctx.Done():
		fmt.Println("Request cancelled")
	}
}
```

🧠 Key Learning:

* HTTP context cancels automatically
* Critical for production systems

---

## ⭐ Bonus: Retry with Context

```go
func retry(ctx context.Context) error {
	for {
		select {
		case <-ctx.Done():
			return ctx.Err()
		default:
			fmt.Println("Retrying...")
			time.Sleep(time.Second)

			// Simulate success condition
			if time.Now().Unix()%2 == 0 {
				fmt.Println("Success")
				return nil
			}
		}
	}
}
```

🧠 Key Learning:

* Always make retry loops cancellable

---

## 🚀 Final Takeaways

* Always respect `ctx.Done()`
* Always `defer cancel()`
* Never ignore context in long-running tasks
* Context enables **safe, scalable, production systems**

---

👉 Next: [Testing](./test)

---
