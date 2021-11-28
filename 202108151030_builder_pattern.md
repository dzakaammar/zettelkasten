---
date: 2021-Aug-15
tags: ["software_design", "design_pattern", "oop", "golang"]
---
# Builder Pattern

Builder pattern menyediakan cara untuk meng-*construct* sebuah object yang kompleks step by step dan memproduksi berbagai macam type atau representasi object dengan satu cara konstruksi yang sama.

Bayangkan sebuah object memiliki beberapa parameter pada saat konstruksi. Parameter-parameter tersebut akan menentukan bagaimana sebuah object akan terbentuk. Parameter akan terus bertambah seiring banyaknya posibilitas konstruksi.

Kita dapat memilih untuk membuat sebuah atau beberapa fungsi untuk menghandle *possibility* tersebut:

```go
package main

type User struct {
	FirstName string
	LastName string
	Email string
}

func NewUserWithoutEmail(fn *string, ln *string) *User {
	u := &User{}
	if fn != nil {
		u.FirstName = fn
	}
	...
	...
}
```

Terlebih, dengan menggunakan metode ini, tidak semua parameter akan digunakan. Dan dengan banyak nya parameter yang ada akan menambah kompleksitas code dan *ugly*.

Builder pattern menyediakan cara yang lebih *proper* untuk menghandle case tersebut.
## Karakteristik
- Tujuannya ingin menghandle konstruksi dari beberapa jenis product dengan *construction step* yang sama
- Ada sebuah interface *Builder* sebagai *construction step*. Dengan menggunakan interface ini, object akan di-*construct* dengan cara yang sama namun implementasi yang berbeda
- Adanya sebuah class ***Director***. Class *Director* akan menyembunyikan detail bagaimana sebuah object akan dikonstruksi dengan menggunakan interface *Builder* yang sama.
- *Builder* juga memungkinkan memiliki fungsi *Result*, untuk mengambil hasil akhir dari object yang telah di build

```go
package main

type Beverage interface {
	Price() int
}

// BeverageBuilder adalah construction steps
type BeverageBuilder interface {
	SweetLevel(i int)
	AddMilk(b bool)
	AddHoney(b bool)
	
	Result() Beverage // Result bisa return abstraction ataupun concrete object
}

// Director bertugas mengimplementasi logic konstruksi
// meng-encapsulate logic tsb dari client
type Director struct {
	bb BeverageBuilder
}

func (d *Director) SetBuilder(cb BeverageBuilder) {
	d.bb = bb
}


func (d *Director) LessSugar() {
	d.bb.SweetLevel(0)
}



func main() {
	director := &Director{}
	coffeeBuilder := &CoffeeBuilder{}
	
	director.SetBuilder(coffeeBuilder)
	director.LessSugar()
	
	coffee := coffeeBuilder.Result()
	coffee.Price()
}

type Coffee struct{
	price int
}

func (c *Coffee) Price() int {
	return c.price
}

// CoffeeBuilder adalah konkret builder untuk produk Coffee
type CoffeeBuilder struct {
	coffee Coffee
}

func (c *CoffeeBuilder) SweetLevel(i int) {
	switch i {
	case 0: 
		return
	default:
		c.coffee.price += 1000
	
	}
}

func (c *CoffeeBuilder) AddHoney(b bool) {
	// coffee + honey is unaccaptable
}

func (c *CoffeeBuilder) AddMilk(b bool) {
	if b {
		c.coffee.price = 1000
	}
}


func (c *CoffeeBuilder) Result() Beverage {
	c.coffee.price += 5000 // base price
	return &c.coffee
}

type Tea struct{
	price int
}

func (c *Tea) Price() int {
	return c.price
}

// TeaBuilder adalah konkret builder untuk produk Tea
type TeaBuilder struct {
	tea Tea
}

func (t *TeaBuilder) SweetLevel(i int) {
	switch i {
	case 0: 
		return
	default:
		t.tea.price += 500
	
	}
}

func (t *TeaBuilder) AddHoney(b bool) {
	t.tea.price = 1000
}

func (t *TeaBuilder) AddMilk(b bool) {
	if b {
		t.tea.price = 1000
	}
}

func (t *TeaBuilder) Result() Beverage {
	t.tea.price += 2000 // base price
	return t.tea
}

```

Gunakan builder pattern jika konstruksi sebuah object ditentukan oleh banyak parameter, dan gunakan metode ini jika tujuan kita adalah ingin membuat code yang dapat menghandle representasi yang berbeda dari tiap object

> **Pastikan terlebih dahulu *common construction steps* yang ingin digunakan. Tanpa itu, builder pattern tidak bisa diimplementasi.**

## Pros
- Dapat mengkonstruksi object dengan step by step
- Reuse code dan constructions code untuk membuat berbagai variasi dari produk
- Single responsibility principle

## Cons
- complex

