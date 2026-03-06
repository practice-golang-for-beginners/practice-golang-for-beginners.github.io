---

layout: default
title: Exercises
parent: "Chapter 5 – Concurrency: Goroutines & Channels"
nav_order: 3
------------

# Exercises – Concurrency in Go

These exercises are designed to help you build practical experience with Go's concurrency model using **goroutines** and **channels**.

The exercises progress from basic goroutine usage to more advanced concurrency patterns such as **fan-out/fan-in pipelines** and **worker pools**.

Try to solve each exercise independently before referring to the solutions.

---

# Exercise 1 – Running a Function in a Goroutine

Write a Go program that launches a goroutine which prints numbers from **1 to 5**.

The main function should also print a message indicating it is running concurrently.

Observe the output and explain why the order of messages may vary.

**Objective**

Understand how goroutines execute concurrently with the main function.

---

# Exercise 2 – Multiple Goroutines

Create three goroutines that print the following messages:

* Worker A running
* Worker B running
* Worker C running

Each worker should print its message **three times** with a short delay.

**Objective**

Observe how Go schedules multiple goroutines.

---

# Exercise 3 – Sending Data Through Channels

Create a channel that transmits integers.

Write a goroutine that sends numbers **1 through 5** into the channel.

The main function should receive the numbers and print them.

**Objective**

Understand basic **channel communication** between goroutines.

---

# Exercise 4 – Buffered Channels

Modify Exercise 3 to use a **buffered channel** with capacity **3**.

Observe how buffered channels behave differently from unbuffered channels.

Answer the following questions:

* When does the sender block?
* When does the receiver block?

**Objective**

Understand the difference between **buffered and unbuffered channels**.

---

# Exercise 5 – Parallel Data Processing

Write a program that processes a slice of integers concurrently.

Each goroutine should:

1. Take a number
2. Square the number
3. Send the result back through a channel

The main goroutine should collect all results and print them.

Example input:

```
[1,2,3,4,5]
```

Example output:

```
1 4 9 16 25
```

**Objective**

Practice **parallel data processing** with goroutines.

---

# Exercise 6 – Fan-Out Pattern

Implement the **fan-out pattern**.

Steps:

1. Create a channel containing tasks (numbers).
2. Start **three worker goroutines**.
3. Each worker reads tasks from the channel and processes them.

The workers should print which worker processed each task.

Example output:

```
Worker 1 processed 2
Worker 2 processed 3
Worker 3 processed 4
```

**Objective**

Learn how to distribute work across multiple goroutines.

---

# Exercise 7 – Fan-In Pattern

Create three goroutines that generate numbers.

Each goroutine sends results into a shared channel.

The main goroutine should collect results from all generators and print them.

**Objective**

Understand how to combine results from multiple concurrent producers.

---

# Exercise 8 – Worker Pool Implementation

Implement a **worker pool** with the following behavior:

1. Create a job queue containing **10 tasks**
2. Start **4 worker goroutines**
3. Each worker should process tasks from the queue
4. Workers should print which job they completed

Example output:

```
Worker 2 processing job 4
Worker 1 processing job 7
Worker 3 processing job 1
```

**Objective**

Learn how to control concurrency using worker pools.

---

# Exercise 9 – Using Select

Create two channels:

* channelA
* channelB

Start two goroutines that send messages to these channels at different intervals.

Use a **select statement** in the main function to receive from whichever channel becomes ready.

**Objective**

Understand how `select` coordinates multiple channels.

---

# Exercise 10 – Detecting a Race Condition

Write a program where multiple goroutines increment the same shared counter.

Run the program using the Go race detector:

```
go test -race
```

Observe the race condition and explain why it occurs.

Then fix the issue using one of the following approaches:

* channels
* synchronization primitives

**Objective**

Understand how race conditions occur and how Go detects them.

---

# Recommended Practice Strategy

For each exercise:

1. Write the solution independently.
2. Run the program multiple times to observe different execution orders.
3. Add logging statements to better understand concurrency behavior.
4. Write small tests where possible.

Understanding concurrency often requires **experimentation and observation**, not just reading code.

---

# Next Step

Once you have attempted these exercises, proceed to:

* **Assessment** – Conceptual questions to validate understanding
* **Solutions** – Reference implementations and explanations
* **Week 5 Questions** – Discussion topics for mentorship review

