---
date: 2021-Aug-13
tags: ["software_design", "design_pattern", "oop", "golang"]
---

# Factory Pattern

Factory method menyediakan cara untuk membuat sebuat object pada superclass, namun memberikan cara subclass untuk merubah type nya. 

## Karakteristik
- Ada abstraksi dari produk object menggunakan interface
- Setiap concrete implementation object produk tersebut, harus men-*satisfy* interface
- Ada sebuah function yang menjadi logic *creational* nya
- Function tersebut membutuhkan "data", untuk menentukan object mana yang harus di *create*

```go
package main

// Coffee adalah abstraksi dari product
type Coffee interface {
	Make()
}

// Arabica adalah konkrit product
type Arabica struct {}
func(a Arabica) Make() {}

// Robusta adalah konkrit product
type Robusta struct {}
func(r Robusta) Make() {}

// Data yang dibutuhkan untuk menentukan tipe produk
const (
	Arabica = iota
	Robusta
)

// GetCoffee adalah fungsi creational dari produk Coffee
func GetCoffee(t int) Coffee {
	switch t {
	case Arabica:
		return Arabica{}
	default:
		return Robusta{}
	}
}

func main() {
	coffee := GetCoffee(Arabica)
	coffee.Make()
}
```

Gunakan factory method ketika ingin menyediakan sebuah fungsi kepada client/pengguna package, namun dengan cara yang dapat meng-*extend* internal component

## Pros
- Decoupled antara code *creator* dan *concrete* produk
- Single responsibility priciple. Tiap jenis product diimplementasi pada konkrit produk yang berbeda, dan hanya memiliki *single responsibility* 
- Open/Closed Principle. Kita dapat menambahkan jenis produk baru tanpa harus merubah implementasi pada tipe produk yang sudah ada.

## Cons
- Code mungkin akan terlihat kompleks, semakin banyak tipe produk akan semakin kompleks.