---
date: 2021-Aug-15
tags: ["software_design", "design_pattern", "oop", "golang"]
---
# Prototype Pattern

Prototype pattern menyediakan cara untuk meng-*copy* existing object tanpa membuat code harus dependen terhadap class mereka

## Karakteristik
- Tujuannya ingin membuat sebuat object baru dengan cara mengcopy object yang telah ada secara komplit (tidak bisa dilakukan secara direct, karena mungkin ada private field yang tidak bisa terlihat atau tidak dapat diakses dari sisi client)
- Mengcopy object tanpa harus terikat dengan logic bagaimana seharusnya object tersebut dicopy
- Adanya sebuah interface ***Prototype*** untuk mengabstraksi proses cloning. Biasanya sebuah interface dengan hanya sebuah method `clone`
- Konkrit implementasi dari *Prototype* tidak hanya meng-copy object tapi menghandle *edge cases* dari object tersebut

```go
package main

import "fmt"

// BeverageType adalah data yang digunakan untuk menentukan
// object mana yang akan di-clone
type BeverageType uint
const (
	Cold BeverageType = iota
	Hot
)

// Beverage adalah interface product
type Beverage interface{
	Drink()
}

// BeverageCloner adalah interface untuk cloner
type BeverageCloner interface{
	Clone(b BeverageType) Coffee
}

func main() {
	coffeeCloner := &CoffeeCache{}
	beverage := coffeeCloner.Clone(Cold)
	beverage.Drink()
	
	arr := make()
}

// Coffee adalah concrete product dari Beverage
type Coffee struct{
	price
}

func (c Coffee) Drink() {
	fmt.Println("drink coffee")
}

var (
	hotCoffeePrototype *Coffee = &Coffee{}
	coldCoffeePrototype *Coffee = &Coffee{}
)

// CoffeeCache adalah concrete object dari Cloner
type CofeeCache struct {}
func (c *CoffeeCache) Clone(bt BeverageType) Beverage {
	swtich bt {
	case Cold:
		res := *coldCoffeePrototype
		return res
	default:
		res := *hotCoffeePrototype
		return res
	}
}

```

## Pros
- Tidak perlu menggunakan inisialisasi code, cukup clone *pre-built prototype*
- Membuat sebuah kompleks object menjadi lebih sederhana
- Clone object tanpa harus terikat/coupled dengan concrete class


## Cons
- Meng-clone object yang kompleks dan memiliki *circular reference* akan menjadi lebih rumit