---
layout: default
title: Solutions
parent: Chapter 7 – HTTP Services
nav_order: 5
---

# ✅ Solutions – HTTP Services in Go

These solutions demonstrate **idiomatic Go practices**, clean design, and proper error handling.
Focus on understanding *why* the code is written this way.

---

# 🟢 Level 1 – Basics

## Solution 1: Hello Endpoint

```go id="sol1"
func helloHandler(w http.ResponseWriter, r *http.Request) {
	w.WriteHeader(http.StatusOK)
	w.Write([]byte("Hello, Go HTTP!"))
}
```

---

## Solution 2: Method Validation

```go id="sol2"
func helloHandler(w http.ResponseWriter, r *http.Request) {
	if r.Method != http.MethodGet {
		http.Error(w, "Method Not Allowed", http.StatusMethodNotAllowed)
		return
	}

	w.Write([]byte("Hello, Go HTTP!"))
}
```

---

## Solution 3: Custom Handler

```go id="sol3"
type CustomHandler struct{}

func (h CustomHandler) ServeHTTP(w http.ResponseWriter, r *http.Request) {
	w.Write([]byte("Custom Handler Response"))
}
```

---

# 🟡 Level 2 – JSON Handling

## Solution 4: JSON Response

```go id="sol4"
type User struct {
	Name string `json:"name"`
	Role string `json:"role"`
}

func userHandler(w http.ResponseWriter, r *http.Request) {
	user := User{Name: "Aditya", Role: "Developer"}

	w.Header().Set("Content-Type", "application/json")
	json.NewEncoder(w).Encode(user)
}
```

---

## Solution 5: JSON Echo

```go id="sol5"
func echoHandler(w http.ResponseWriter, r *http.Request) {
	var data map[string]interface{}

	err := json.NewDecoder(r.Body).Decode(&data)
	if err != nil {
		http.Error(w, "Invalid JSON", http.StatusBadRequest)
		return
	}

	w.Header().Set("Content-Type", "application/json")
	json.NewEncoder(w).Encode(data)
}
```

---

## Solution 6: Validation

```go id="sol6"
func echoHandler(w http.ResponseWriter, r *http.Request) {
	var data map[string]string

	err := json.NewDecoder(r.Body).Decode(&data)
	if err != nil {
		http.Error(w, "Invalid JSON", http.StatusBadRequest)
		return
	}

	if data["name"] == "" {
		http.Error(w, "Name is required", http.StatusBadRequest)
		return
	}

	json.NewEncoder(w).Encode(data)
}
```

---

# 🟠 Level 3 – Middleware

## Solution 7: Logging Middleware

```go id="sol7"
func loggingMiddleware(next http.Handler) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		fmt.Println(r.Method, r.URL.Path)
		next.ServeHTTP(w, r)
	})
}
```

---

## Solution 8: Header Middleware

```go id="sol8"
func headerMiddleware(next http.Handler) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		w.Header().Set("X-App-Version", "1.0")
		next.ServeHTTP(w, r)
	})
}
```

---

## Solution 9: Middleware Chaining

```go id="sol9"
handler := loggingMiddleware(headerMiddleware(http.HandlerFunc(helloHandler)))
```

---

# 🔵 Level 4 – Real API Patterns

## Solution 10: Health Check

```go id="sol10"
func healthHandler(w http.ResponseWriter, r *http.Request) {
	response := map[string]string{"status": "UP"}

	w.Header().Set("Content-Type", "application/json")
	json.NewEncoder(w).Encode(response)
}
```

---

## Solution 11: User Creation

```go id="sol11"
type User struct {
	Name string `json:"name"`
	Age  int    `json:"age"`
}

func createUser(w http.ResponseWriter, r *http.Request) {
	var user User

	err := json.NewDecoder(r.Body).Decode(&user)
	if err != nil {
		http.Error(w, "Invalid JSON", http.StatusBadRequest)
		return
	}

	if user.Name == "" || user.Age == 0 {
		http.Error(w, "Invalid input", http.StatusBadRequest)
		return
	}

	w.WriteHeader(http.StatusCreated)
	json.NewEncoder(w).Encode(map[string]string{
		"message": "User created",
	})
}
```

---

## Solution 12: Improved Error Handling

```go id="sol12"
func createUser(w http.ResponseWriter, r *http.Request) {
	var user User

	if err := json.NewDecoder(r.Body).Decode(&user); err != nil {
		http.Error(w, "Malformed JSON", http.StatusBadRequest)
		return
	}

	if user.Name == "" {
		http.Error(w, "Name is required", http.StatusBadRequest)
		return
	}

	if user.Age <= 0 {
		http.Error(w, "Invalid age", http.StatusBadRequest)
		return
	}

	w.WriteHeader(http.StatusCreated)
	json.NewEncoder(w).Encode(map[string]string{
		"message": "User created successfully",
	})
}
```

---

# 🔴 Level 5 – Advanced

## Solution 13: Context Timeout

```go id="sol13"
func slowHandler(w http.ResponseWriter, r *http.Request) {
	ctx := r.Context()

	select {
	case <-time.After(3 * time.Second):
		w.Write([]byte("Completed"))
	case <-ctx.Done():
		http.Error(w, "Request cancelled", http.StatusRequestTimeout)
	}
}
```

---

## Solution 14: Graceful Shutdown

```go id="sol14"
server := &http.Server{Addr: ":8080"}

go server.ListenAndServe()

ctx, stop := signal.NotifyContext(context.Background(), os.Interrupt)
defer stop()

<-ctx.Done()
server.Shutdown(context.Background())
```

---

## Solution 15: Refactoring for Testability

```go id="sol15"
type UserService interface {
	CreateUser(name string) error
}

func handler(service UserService) http.HandlerFunc {
	return func(w http.ResponseWriter, r *http.Request) {
		err := service.CreateUser("test")
		if err != nil {
			http.Error(w, "Error", http.StatusInternalServerError)
			return
		}
		w.Write([]byte("OK"))
	}
}
```

---

# 🧪 Bonus Solutions

## Table-Driven Test Example

```go id="sol16"
func TestEchoHandler(t *testing.T) {
	tests := []struct {
		name       string
		body       string
		statusCode int
	}{
		{"Valid", `{"name":"Aditya"}`, 200},
		{"Invalid", `invalid`, 400},
	}

	for _, tt := range tests {
		t.Run(tt.name, func(t *testing.T) {
			req := httptest.NewRequest("POST", "/echo", strings.NewReader(tt.body))
			rec := httptest.NewRecorder()

			echoHandler(rec, req)

			if rec.Code != tt.statusCode {
				t.Errorf("expected %d, got %d", tt.statusCode, rec.Code)
			}
		})
	}
}
```

---

# 🧠 Key Design Takeaways

* Keep handlers **thin**
* Move business logic to services
* Use middleware for cross-cutting concerns
* Always validate input
* Write testable code

---

## 📌 Final Insight

> “Clean Go HTTP code is simple, explicit, and easy to test.”

Focus on **readability and correctness over cleverness**.

---
