---
layout: default
title: Test Guide – Concurrency Exercises
parent: "Chapter 5 – Concurrency: Goroutines & Channels"
nav_order: 2
---

# Testing Concurrency in Go

Testing concurrent programs requires careful design. Unlike sequential programs, concurrent programs may produce results in **different execution orders**, making some tests more complex.

Go provides strong built-in support for testing through the **testing package**.

In this section we demonstrate how to write tests for the concurrency exercises.

---

# Running Tests

Run tests using the Go testing tool:

```bash
go test ./...
```

To detect race conditions:

```bash
go test -race ./...
```

The **race detector** identifies unsafe access to shared variables.

---

# Test Example – Exercise 3 (Channel Communication)

Suppose we have the following function that sends numbers to a channel.

```go
func SendNumbers(ch chan int) {
	for i := 1; i <= 5; i++ {
		ch <- i
	}
	close(ch)
}
```

### Test

```go
package concurrency

import "testing"

func TestSendNumbers(t *testing.T) {

	ch := make(chan int)

	go SendNumbers(ch)

	expected := []int{1,2,3,4,5}

	i := 0

	for num := range ch {

		if num != expected[i] {
			t.Fatalf("expected %d but got %d", expected[i], num)
		}

		i++
	}
}
```

This test verifies that the channel produces the expected sequence.

---

# Test Example – Exercise 5 (Parallel Data Processing)

Function under test:

```go
func Square(n int) int {
	return n * n
}
```

### Test

```go
func TestSquare(t *testing.T) {

	tests := []struct{
		input int
		expected int
	}{
		{1,1},
		{2,4},
		{3,9},
		{4,16},
	}

	for _, tt := range tests {

		result := Square(tt.input)

		if result != tt.expected {
			t.Fatalf("expected %d got %d", tt.expected, result)
		}
	}
}
```

This uses **table-driven testing**, a common Go testing pattern.

---

# Test Example – Worker Pool

Suppose we have a worker pool function.

```go
func Worker(id int, jobs <-chan int, results chan<- int) {
	for job := range jobs {
		results <- job * 2
	}
}
```

### Test

```go
func TestWorkerPool(t *testing.T) {

	jobs := make(chan int, 5)
	results := make(chan int, 5)

	for w := 1; w <= 2; w++ {
		go Worker(w, jobs, results)
	}

	for j := 1; j <= 5; j++ {
		jobs <- j
	}

	close(jobs)

	for i := 0; i < 5; i++ {

		result := <-results

		if result % 2 != 0 {
			t.Fatalf("expected even number but got %d", result)
		}
	}
}
```

Since execution order is not guaranteed, the test checks **correctness of results rather than ordering**.

---

# Testing Fan-Out / Fan-In Pipelines

Fan-in pipelines combine multiple producers.

Example generator:

```go
func Generator(start int, ch chan int) {

	for i := start; i < start+3; i++ {
		ch <- i
	}
}
```

### Test

```go
func TestGenerator(t *testing.T) {

	ch := make(chan int)

	go Generator(1, ch)

	count := 0

	for i := 0; i < 3; i++ {
		<-ch
		count++
	}

	if count != 3 {
		t.Fatalf("expected 3 values but got %d", count)
	}
}
```

---

# Testing with Timeouts

Sometimes tests must ensure goroutines do not block forever.

Example:

```go
func TestChannelTimeout(t *testing.T) {

	ch := make(chan int)

	select {

	case ch <- 1:
		t.Fatal("unexpected send")

	case <-time.After(time.Second):
		// expected timeout
	}
}
```

Timeout tests help detect **deadlocks**.

---

# Using the Race Detector

Race conditions occur when multiple goroutines access shared memory unsafely.

Example race-prone code:

```go
var counter int

func increment() {
	counter++
}
```

Test:

```bash
go test -race
```

The race detector will report concurrent writes.

---

# Best Practices for Testing Concurrent Code

When testing concurrent programs:

1. Avoid relying on execution order.
2. Prefer **channels for synchronization**.
3. Use **WaitGroups** when necessary.
4. Avoid unnecessary sleep statements.
5. Always run tests with **race detection**.

---

# Recommended Testing Workflow

1. Write the concurrent function.
2. Write deterministic unit tests.
3. Run tests with the race detector.

```bash
go test -race ./...
```

4. Refactor if race conditions are detected.

---

# Next Step

After running these tests:

* Review the **exercise solutions**
* Discuss **Week 5 questions**
* Begin implementing the **mini-project**

Testing concurrent systems is a critical skill for building reliable Go applications.
