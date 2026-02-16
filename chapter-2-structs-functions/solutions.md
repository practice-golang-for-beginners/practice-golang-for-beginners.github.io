

# 📘 solutions.md

Below are clean, idiomatic solutions.

---

## Solution 1

```go
func Multiply(a int, b int) int {
    return a * b
}
```

---

## Solution 2

```go
import "fmt"

func SafeDivide(a int, b int) (int, error) {
    if b == 0 {
        return 0, fmt.Errorf("division by zero")
    }
    return a / b, nil
}
```

---

## Solution 3

```go
type Book struct {
    Title  string
    Author string
    Price  float64
}
```

---

## Solution 4

```go
func (b *Book) Discount(percent float64) {
    b.Price = b.Price - (b.Price * percent / 100)
}
```

---

## Solution 5 Explanation

Value receiver will NOT modify original.
Pointer receiver will modify original struct.

---

## Solution 6

```go
func NewBook(title, author string, price float64) *Book {
    if price < 0 {
        price = 0
    }
    return &Book{
        Title:  title,
        Author: author,
        Price:  price,
    }
}
```

---

## Solution 7

```go
func (b Book) IsExpensive() bool {
    return b.Price > 1000
}
```

---

## Solution 8

```go
type Address struct {
    City    string
    Country string
}

type Customer struct {
    Name string
    Address
}
```

---

## Solution 9

Inside `library/book.go`:

```go
package library
```

Import in main:

```go
import "project/library"
```

---

## Solution 10

```go
type User struct {
    Name string
    Age  int
}

func (u User) IsValid() bool {
    return u.Name != "" && u.Age > 0
}
```

---

## Solution 11

```go
func ApplyOperation(a int, b int, op func(int, int) int) int {
    return op(a, b)
}
```

---

## Solution 12

```go
type Printable interface {
    Print() string
}

func (b Book) Print() string {
    return b.Title
}
```

---

## Solution 13

```go
func (b *Book) UpdatePrice(newPrice float64) error {
    if newPrice < 0 {
        return fmt.Errorf("price cannot be negative")
    }
    b.Price = newPrice
    return nil
}
```

---

## Solution 14

```go
func TotalPrice(books []Book) float64 {
    total := 0.0
    for _, b := range books {
        total += b.Price
    }
    return total
}
```

---

## Solution 15 (Correct Version)

```go
func IncreaseAllPrices(books []*Book, percent float64) {
    for _, b := range books {
        b.Price += b.Price * percent / 100
    }
}
```

---

## Solution 16

```go
type Book struct {
    Title  string
    Author string
    price  float64
}

func (b Book) Price() float64 {
    return b.price
}

func (b *Book) SetPrice(p float64) {
    if p >= 0 {
        b.price = p
    }
}
```

---

## Solution 17

```go
func (b *Book) SetPrice(p float64) *Book {
    if p >= 0 {
        b.price = p
    }
    return b
}
```

---

## Solution 18

```go
func NewPremiumBook(title, author string, price float64) *Book {
    return &Book{
        Title:  title,
        Author: author,
        price:  price,
        Premium: true,
    }
}
```

---

## Solution 19

Refactor:

* Make fields unexported
* Provide getters/setters
* Avoid direct modification

---

## Solution 20 (Cart)

```go
type Cart struct {
    Books []Book
}

func (c *Cart) AddBook(b Book) {
    c.Books = append(c.Books, b)
}

func (c *Cart) TotalAmount() float64 {
    return TotalPrice(c.Books)
}
```

---
