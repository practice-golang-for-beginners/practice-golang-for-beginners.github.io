---
layout: default
title: Solutions – Concurrency Exercises
parent: "Chapter 5 – Concurrency: Goroutines & Channels"
nav_order: 5
---

# Solutions – Concurrency Exercises

This section contains reference implementations for the exercises in this chapter.

The goal of these solutions is not only to produce correct results but also to demonstrate **idiomatic Go concurrency patterns**.

Note: Multiple correct implementations may exist.

---

# Exercise 1 – Running a Function in a Goroutine

```go
package main

import (
	"fmt"
	"time"
)

func printNumbers() {
	for i := 1; i <= 5; i++ {
		fmt.Println("Goroutine:", i)
		time.Sleep(100 * time.Millisecond)
	}
}

func main() {

	go printNumbers()

	fmt.Println("Main function running")

	time.Sleep(1 * time.Second)
}
```

Key idea:

The `Sleep` is used here to allow the goroutine time to execute before the program exits.

---

# Exercise 2 – Multiple Goroutines

```go
package main

import (
	"fmt"
	"time"
)

func worker(name string) {
	for i := 0; i < 3; i++ {
		fmt.Println(name, "running")
		time.Sleep(200 * time.Millisecond)
	}
}

func main() {

	go worker("Worker A")
	go worker("Worker B")
	go worker("Worker C")

	time.Sleep(1 * time.Second)
}
```

Key idea:

Multiple goroutines run concurrently and their execution order may vary.

---

# Exercise 3 – Sending Data Through Channels

```go
package main

import "fmt"

func sendNumbers(ch chan int) {

	for i := 1; i <= 5; i++ {
		ch <- i
	}

	close(ch)
}

func main() {

	ch := make(chan int)

	go sendNumbers(ch)

	for num := range ch {
		fmt.Println(num)
	}
}
```

Key idea:

Channels enable safe communication between goroutines.

---

# Exercise 4 – Buffered Channels

```go
package main

import "fmt"

func main() {

	ch := make(chan int, 3)

	ch <- 1
	ch <- 2
	ch <- 3

	fmt.Println(<-ch)
	fmt.Println(<-ch)
	fmt.Println(<-ch)
}
```

Key idea:

Buffered channels allow sending multiple values before blocking.

---

# Exercise 5 – Parallel Data Processing

```go
package main

import "fmt"

func square(n int, ch chan int) {
	ch <- n * n
}

func main() {

	numbers := []int{1, 2, 3, 4, 5}

	ch := make(chan int)

	for _, num := range numbers {
		go square(num, ch)
	}

	for i := 0; i < len(numbers); i++ {
		fmt.Println(<-ch)
	}
}
```

Key idea:

Each number is processed concurrently.

---

# Exercise 6 – Fan-Out Pattern

```go
package main

import (
	"fmt"
)

func worker(id int, jobs <-chan int) {

	for job := range jobs {
		fmt.Printf("Worker %d processed job %d\n", id, job)
	}
}

func main() {

	jobs := make(chan int)

	for w := 1; w <= 3; w++ {
		go worker(w, jobs)
	}

	for j := 1; j <= 5; j++ {
		jobs <- j
	}

	close(jobs)
}
```

Key idea:

Multiple workers consume tasks from a shared job channel.

---

# Exercise 7 – Fan-In Pattern

```go
package main

import (
	"fmt"
)

func generator(start int, ch chan int) {

	for i := start; i < start+3; i++ {
		ch <- i
	}
}

func main() {

	ch := make(chan int)

	go generator(1, ch)
	go generator(10, ch)
	go generator(100, ch)

	for i := 0; i < 9; i++ {
		fmt.Println(<-ch)
	}
}
```

Key idea:

Multiple producers send data into a shared channel.

---

# Exercise 8 – Worker Pool

```go
package main

import (
	"fmt"
)

func worker(id int, jobs <-chan int, results chan<- int) {

	for job := range jobs {
		fmt.Printf("Worker %d processing job %d\n", id, job)
		results <- job * 2
	}
}

func main() {

	jobs := make(chan int, 10)
	results := make(chan int, 10)

	for w := 1; w <= 4; w++ {
		go worker(w, jobs, results)
	}

	for j := 1; j <= 10; j++ {
		jobs <- j
	}

	close(jobs)

	for r := 1; r <= 10; r++ {
		fmt.Println("Result:", <-results)
	}
}
```

Key idea:

Worker pools control concurrency while processing many tasks.

---

# Exercise 9 – Using Select

```go
package main

import (
	"fmt"
	"time"
)

func main() {

	channelA := make(chan string)
	channelB := make(chan string)

	go func() {
		time.Sleep(1 * time.Second)
		channelA <- "Message from A"
	}()

	go func() {
		time.Sleep(2 * time.Second)
		channelB <- "Message from B"
	}()

	select {
	case msg := <-channelA:
		fmt.Println(msg)
	case msg := <-channelB:
		fmt.Println(msg)
	}
}
```

Key idea:

`select` waits on multiple channel operations.

---

# Exercise 10 – Race Condition

```go
package main

import (
	"fmt"
	"sync"
)

func main() {

	var counter int
	var wg sync.WaitGroup
	var mu sync.Mutex

	for i := 0; i < 1000; i++ {

		wg.Add(1)

		go func() {

			mu.Lock()
			counter++
			mu.Unlock()

			wg.Done()

		}()
	}

	wg.Wait()

	fmt.Println("Counter:", counter)
}
```

Key idea:

A mutex protects shared state from race conditions.

---

# Summary

These solutions demonstrate several important concurrency concepts:

* Launching goroutines
* Communicating using channels
* Fan-out and fan-in patterns
* Worker pools
* Coordinating multiple channel operations with `select`
* Protecting shared data using synchronization primitives

Understanding these patterns is essential for building scalable Go applications.
