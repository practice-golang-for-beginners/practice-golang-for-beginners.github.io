---
layout: default
title: Mini Project – Concurrent URL Fetcher
parent: "Chapter 5 – Concurrency: Goroutines & Channels"
nav_order: 7
---

# Mini Project – Concurrent URL Fetcher

Starting from this chapter, you will build a small but practical Go system that demonstrates real-world concurrency patterns.

The goal of this project is to implement a **concurrent URL fetcher** that retrieves multiple web pages efficiently.

This project will evolve across multiple weeks as you learn more Go concepts.

---

# Project Overview

The program will:

1. Accept a list of URLs
2. Fetch the URLs concurrently
3. Collect the results
4. Print response status and response time

Example input:

```
https://golang.org
https://example.com
https://github.com
```

Example output:

```
Fetched https://golang.org in 210 ms (Status 200)
Fetched https://example.com in 180 ms (Status 200)
Fetched https://github.com in 240 ms (Status 200)
```

---

# Week 5 Objective

In Week 5 we will focus on:

* Goroutines
* Channels
* Fan-out / fan-in
* Basic concurrency coordination

---

# Architecture

The system will follow this pattern:

```
URL List
   |
   v
Producer Goroutine
   |
   v
Jobs Channel
   |
   v
Worker Goroutines (Fan-out)
   |
   v
Results Channel
   |
   v
Aggregator (Fan-in)
   |
   v
Output
```

---

# Step 1 – Define Data Structures

Create a structure to store results.

```go
type Result struct {
	URL        string
	StatusCode int
	Duration   int64
	Error      error
}
```

---

# Step 2 – Create Worker Function

Workers will fetch URLs from the jobs channel.

Example structure:

```go
func worker(jobs <-chan string, results chan<- Result) {

	for url := range jobs {

		start := time.Now()

		resp, err := http.Get(url)

		duration := time.Since(start)

		if err != nil {

			results <- Result{
				URL: url,
				Error: err,
			}

			continue
		}

		results <- Result{
			URL: url,
			StatusCode: resp.StatusCode,
			Duration: duration.Milliseconds(),
		}

		resp.Body.Close()
	}
}
```

---

# Step 3 – Create Worker Pool

Start multiple workers.

Example:

```go
for w := 1; w <= 5; w++ {
	go worker(jobs, results)
}
```

This creates a worker pool with **5 concurrent workers**.

---

# Step 4 – Send Jobs

Send URLs to the jobs channel.

```go
for _, url := range urls {
	jobs <- url
}

close(jobs)
```

---

# Step 5 – Collect Results (Fan-In)

Receive results from workers.

```go
for i := 0; i < len(urls); i++ {

	result := <-results

	if result.Error != nil {

		fmt.Println("Error fetching:", result.URL)

		continue
	}

	fmt.Printf(
		"Fetched %s in %d ms (Status %d)\n",
		result.URL,
		result.Duration,
		result.StatusCode,
	)
}
```

---

# Example Program Structure

```
main.go
worker.go
models.go
```

This introduces good project structure early.

---

# What You Will Learn

By completing this mini project you will learn:

* How to coordinate goroutines
* How to use channels for communication
* How to build worker pools
* How to collect results from concurrent workers

These are core skills used in real Go systems.

---

# Expected Output Example

```
Fetched https://golang.org in 210 ms (Status 200)
Fetched https://github.com in 198 ms (Status 200)
Fetched https://example.com in 175 ms (Status 200)
```

Note that the **order of output may vary**, because the requests are executed concurrently.

---

# Optional Challenge

Enhance the program to:

* Accept URLs from a file
* Limit concurrency
* Display total execution time

---

# Next Steps (Week 6)

In Week 6 this project will be extended to include:

* Context cancellation
* Request timeouts
* Graceful shutdown
* Improved error handling
