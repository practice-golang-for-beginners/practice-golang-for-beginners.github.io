---
layout: default
title: Study Material
parent: Chapter 6 – Context, Timeouts & Cancellation
nav_order: 1
---

# 📖 Study Material – Context, Timeouts & Cancellation

---

## 🧠 1. Why Context Exists

In real-world systems, operations often:

* Take time (API calls, DB queries)
* Depend on other services
* May need to be **stopped midway**

Without control:

* Requests hang forever ❌
* Resources get exhausted ❌
* Systems become unstable ❌

Go introduces `context.Context` to solve this.

👉 It provides:

* **Cancellation**
* **Timeouts**
* **Deadlines**
* **Request-scoped values (limited use)**

---

## 🆚 Python vs Go Mental Model

| Python                           | Go                       |
| -------------------------------- | ------------------------ |
| Implicit request handling        | Explicit context passing |
| Threading / async                | Goroutines + channels    |
| No standard cancellation pattern | Built-in context system  |

👉 In Go, **you must explicitly pass control signals**.

---

## 📦 2. What is `context.Context`?

`context.Context` is an interface used to:

* Signal cancellation
* Set deadlines
* Carry metadata across API boundaries

### Core Methods

```go
type Context interface {
    Done() <-chan struct{}
    Err() error
    Deadline() (deadline time.Time, ok bool)
    Value(key interface{}) interface{}
}
```

---

## ⚙️ 3. Creating Contexts

### 3.1 Root Contexts

```go
ctx := context.Background()
```

Used for:

* Main functions
* Initialization
* Tests

---

### 3.2 Cancelable Context

```go
ctx, cancel := context.WithCancel(context.Background())
defer cancel()
```

👉 Use when:

* You want manual control to stop execution

---

### 3.3 Timeout Context

```go
ctx, cancel := context.WithTimeout(context.Background(), 2*time.Second)
defer cancel()
```

👉 Automatically cancels after timeout

---

### 3.4 Deadline Context

```go
ctx, cancel := context.WithDeadline(context.Background(), time.Now().Add(2*time.Second))
defer cancel()
```

---

## 🚦 4. Listening for Cancellation

Always check:

```go
select {
case <-ctx.Done():
    fmt.Println("Operation cancelled:", ctx.Err())
    return
default:
    // continue work
}
```

---

## 🔁 5. Context Propagation

Context must be passed **down the call chain**:

```go
func process(ctx context.Context) {
    fetchData(ctx)
}

func fetchData(ctx context.Context) {
    // use ctx here
}
```

👉 Rule:

> Always pass context as the **first parameter**

---

## ⚠️ 6. Common Mistakes (Very Important)

### ❌ Ignoring cancellation

Bad:

```go
time.Sleep(5 * time.Second)
```

Good:

```go
select {
case <-time.After(5 * time.Second):
case <-ctx.Done():
    return
}
```

---

### ❌ Storing context in struct

Bad:

```go
type Service struct {
    ctx context.Context
}
```

Good:

```go
func (s *Service) Process(ctx context.Context) {}
```

---

### ❌ Passing nil context

Always use:

```go
context.Background()
```

---

### ❌ Forgetting cancel()

Leads to:

* Memory leaks
* Goroutine leaks

---

## 🌐 7. Context in HTTP Servers

In Go HTTP:

```go
func handler(w http.ResponseWriter, r *http.Request) {
    ctx := r.Context()
}
```

👉 This context:

* Cancels when client disconnects
* Carries request lifecycle

---

## 🔄 8. Real Example – API Call with Timeout

```go
func fetch(ctx context.Context) error {
    select {
    case <-time.After(3 * time.Second):
        fmt.Println("Completed")
        return nil
    case <-ctx.Done():
        return ctx.Err()
    }
}

func main() {
    ctx, cancel := context.WithTimeout(context.Background(), 1*time.Second)
    defer cancel()

    err := fetch(ctx)
    fmt.Println(err)
}
```

👉 Output:

```
context deadline exceeded
```

---

## 🧪 9. Testing Context-Based Code

Use timeout contexts:

```go
ctx, cancel := context.WithTimeout(context.Background(), time.Second)
defer cancel()
```

Test:

* Cancellation paths
* Timeout behavior
* Error handling

---

## 🧩 10. When to Use Context

Use context when:

* Making HTTP calls
* Calling databases
* Running goroutines
* Handling request lifecycles

Do NOT use context for:

* Passing business data
* Optional parameters
* Configuration

---

## 🧠 Key Takeaways

* Context is **mandatory in production Go**
* Always pass it explicitly
* Respect cancellation signals
* Use timeouts to protect systems
* Keep it simple and idiomatic

---

## 🚀 What’s Next?

Now move to:
👉 [Exercises](./exercises)

Practice is where context truly “clicks”.

---
