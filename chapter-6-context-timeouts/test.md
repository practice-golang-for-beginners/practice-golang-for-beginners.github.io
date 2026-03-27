---
layout: default
title: Test Cases
parent: Chapter 6 – Context, Timeouts & Cancellation
nav_order: 4
---

# 🔬 Testing – Context, Timeouts & Cancellation

---

## 🎯 Objective

Learn how to test:

* Context-aware functions
* Timeout behavior
* Cancellation flows
* Concurrent operations safely

---

## 🧠 Why Testing Context is Important

In real systems:

* Timeouts prevent system overload
* Cancellation avoids resource leaks
* Bugs often appear in **edge cases**

👉 Testing ensures:

* Your code **stops when it should**
* Your system is **resilient under stress**

---

## 🧪 1. Basic Context Testing

### Example Function

```go id="l6y7qv"
func doWork(ctx context.Context) error {
	select {
	case <-time.After(2 * time.Second):
		return nil
	case <-ctx.Done():
		return ctx.Err()
	}
}
```

---

### Test Case

```go id="pj4rj6"
func TestDoWork_Success(t *testing.T) {
	ctx := context.Background()

	err := doWork(ctx)

	if err != nil {
		t.Errorf("expected no error, got %v", err)
	}
}
```

---

## ⏱ 2. Testing Timeout Behavior

```go id="qiv8fb"
func TestDoWork_Timeout(t *testing.T) {
	ctx, cancel := context.WithTimeout(context.Background(), time.Second)
	defer cancel()

	err := doWork(ctx)

	if err == nil {
		t.Errorf("expected timeout error, got nil")
	}

	if err != context.DeadlineExceeded {
		t.Errorf("expected DeadlineExceeded, got %v", err)
	}
}
```

🧠 Key Learning:

* Always verify **specific error type**

---

## 🚫 3. Testing Cancellation

```go id="7j8j0x"
func TestDoWork_Cancel(t *testing.T) {
	ctx, cancel := context.WithCancel(context.Background())

	cancel() // cancel immediately

	err := doWork(ctx)

	if err != context.Canceled {
		t.Errorf("expected Canceled error, got %v", err)
	}
}
```

---

## ⚡ 4. Testing Fast Execution (Avoid Slow Tests)

❌ Bad:

```go id="xvq6c7"
time.Sleep(5 * time.Second)
```

✅ Good:

* Use shorter durations in tests
* Use dependency injection if needed

```go id="a8d3zg"
func doWorkWithDelay(ctx context.Context, delay time.Duration) error {
	select {
	case <-time.After(delay):
		return nil
	case <-ctx.Done():
		return ctx.Err()
	}
}
```

---

## 🔁 5. Table-Driven Tests

```go id="94chhc"
func TestDoWork_TableDriven(t *testing.T) {
	tests := []struct {
		name      string
		timeout   time.Duration
		expectErr bool
	}{
		{"no-timeout", 3 * time.Second, false},
		{"timeout", 1 * time.Second, true},
	}

	for _, tt := range tests {
		t.Run(tt.name, func(t *testing.T) {
			ctx, cancel := context.WithTimeout(context.Background(), tt.timeout)
			defer cancel()

			err := doWork(ctx)

			if tt.expectErr && err == nil {
				t.Errorf("expected error, got nil")
			}
		})
	}
}
```

🧠 Key Learning:

* Scalable testing pattern
* Clean and reusable

---

## 🧵 6. Testing Concurrent Code Safely

### Example

```go id="3dtpb0"
func worker(ctx context.Context, done chan bool) {
	select {
	case <-time.After(2 * time.Second):
		done <- true
	case <-ctx.Done():
		done <- false
	}
}
```

---

### Test

```go id="r2a1nm"
func TestWorker_Cancel(t *testing.T) {
	ctx, cancel := context.WithCancel(context.Background())
	done := make(chan bool)

	go worker(ctx, done)

	cancel()

	result := <-done

	if result != false {
		t.Errorf("expected false, got true")
	}
}
```

🧠 Key Learning:

* Always synchronize goroutines in tests
* Avoid race conditions

---

## 🧪 7. Using `go test -race`

Run:

```bash
go test -race ./...
```

👉 Detects:

* Race conditions
* Unsafe concurrent access

---

## ⚠️ Common Testing Mistakes

* ❌ Using long sleeps (slow tests)
* ❌ Not cancelling context
* ❌ Ignoring error values
* ❌ Not testing cancellation paths
* ❌ Flaky timing-based tests

---

## 🧠 Best Practices

* Keep tests **fast (<1 second ideally)**
* Always test **both success and failure paths**
* Use **table-driven tests**
* Inject dependencies (timeouts, delays)
* Prefer **deterministic behavior**

---

## 🚀 Final Takeaways

* Context testing is about **control and predictability**
* Always verify:

  * Timeout
  * Cancellation
  * Success path
* Good tests = production reliability

---

👉 Next: [Assessment](./assessment)

---
