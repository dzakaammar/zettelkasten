---
date: 2021-Aug-14
tags: ["software_design", "design_pattern", "oop", "golang"]
---
# Abstract Factory Pattern

Abstract factory menyediakan produksi *families* dari objek-objek yang terkait tanpa harus menentukan konkrit class nya

## Karakteristik
- Ada banyak tipe produk dan banyak variant dari produk. Contoh tipe produk: meja, kursi, sofa. Contoh variant dari produk: style modern, style kuno, style minimalis
- Famili produk (tipe produk + jenis variant nya)
- Tujuannya ingin membuat sebuah *factory* untuk memproduksi famili dari produk
- Adanya abstraksi dari masing-masing tipe object (abstract product). Contoh: interface meja, kursi, dan sofa
- Adanya abstraksi yang men-*define* struktur dari sebuah famili produk (abstract factory)
- Adanya sebuah fungsi/method yang menentukan factory mana yang digunakan berdasarkan data yang diberikan

```go
package main

const (
	Breakfast = iota
	Lunch
)

// Coffee adalah abstract product
type Coffee interface {
	Drink()
}

// Bread adalah abstract product
type Bread interface {
	Eat()
}

// Menu adalah abstract factory
type Menu interface{
	GetCoffee() Coffee
	GetBread() Bread
}

// GetMenu adalah fungsi untuk menentukan factory mana yang digunakan
// berdasarkan data yang diberikan. i.e Breakfast atau Lunch
func GetMenuFor(i int) Menu {
	switch i {
	case Breakfast:
		return BreakfastMenu{}
		
	default:
		return LunchMenu{}
	}
}

func main() {
	menu := GetMenuFor(Lunch)
	coffee := menu.GetCoffee()
	bread := menu.GetBread()
	
	coffee.Drink()
	bread.Eat()

}

// ArabicaCoffee adalah konkrit product dari Coffee
type ArabicaCoffee struct {}
func(a ArabicaCoffee) Drink()   {}

// RobustaCoffee adalah konkrit product dari Coffee
type RobustaCoffee struct {}
func(r RobustaCoffee) Drink()   {}

// Croissant adalah konkrit product dari Bread
type Croissant struct   {}
func(c Croissant) Eat() {}

// Sandwich adalah konkrit product dari Bread
type Sandwich struct   {}
func(c Sandwich) Eat() {}

// BreakfastMenu adalah konkrit factory dari Menu
type BreakfastMenu struct {}

func (b BreakfastMenu) GetCoffee() Coffee {
	return ArabicaCoffee{}
}

func (b BreakfastMenu) GetBread() Bread {
	return Croissant {}
}

// LunchMenu adalah konkrit factory dari Menu
type LunchMenu struct {}

func (l LunchMenu) GetCoffee() Coffee {
	return RobustaCoffee{}
}

func (l LunchMenu) GetBread() Bread {
	return Sandwich {}
}

```

Pertimbangkan untuk menggunakan Abstract Factory ketika telah menggunakan Factory method. Ketika sebuah class menghandle lebih dari satu tipe product, mungkin akan lebih layak untuk diextract kedalah sebuah Factory baru atau menggunakan Abstract method secara keseluruhan

## Pros
- Yakin dengan bahwa setiap product yang dihasilkan akan selalu compatible
- decoupled antara setiap implementasi product ataupun variant product
- Single responsibility principle
- open/closed principle

## Cons
- complex