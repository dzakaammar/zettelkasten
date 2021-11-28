---
date: 2021-Aug-15
tags: ["software_design", "design_pattern", "oop", "golang"]
---
# Adapter Pattern

> Bayangkan sebuah adapter dalam perangkat eletronik yang berfungsi menyambungkan antara hardware dengan sumber listrik.
> Analogi yang sama dengan *Adapter Pattern* .

Adapter pattern adalah pattern untuk menghubungkan sebuah interface yang tidak *compatible* dengan existing interface, dengan membuat sebuah interface sebagai "adapter". Secara sederhana, pattern ini digunakan untuk membuat object-object yang *incompatible* secara interface untuk saling berkolaborasi.

Adapter membungkus beberapa object untuk menyembunyikan logic dan kompleksitas *conversion*, sehingga menjadikan object tersebut menjadi *compatible*

## Karakteristik
- Implementasinya menggunakan *composition principle*, mengimplimentasi sebuah interface dan mem-*wrap* interface lainnya
- Ada sebuah *Client Interface* yang digunakan oleh *Client* sebagai adapter
- Ada sebuah *Adapter* class sebagai konkret object dari *Client Interface*, dimana logic untuk konversi ke interface yang dituju berada
- Ada sebuah *Service* a.k.a *API* a.k.a *Third Party* sebagai tujuan konversi

```go
package main

import "fmt"

type Printer interface {
	Print(s string)
}

type ModernPrinter interface {
	PrintBeatifully(s string)
}

type PrinterAdapter struct {
	old Printer
	newOne ModernPrinter
}

func (p *PrinterAdapter) Print(s string) {
	if p.old != nil {
		p.old.Print(s)
	}
	p.newOne.Print(s)
}

func main () {
	var p Printer
	p := &LegacyPrinter{}
	p.Print("single call")
	
	mp := &NewPrinter{}
	mp.Print("single call")
	
	p = PrinterAdapter{old: lp, newOne: mp}
	p.Print("adapter call")
}

type LegacyPrinter struct {}
func (LegacyPrinter) Print(s string) { fmt.Println(s) }


type NewPrinter struct {}
func (NewPrinter) PrintBeautifully(s string) { fmt.Println("Beautiful ", s) }

```

contoh lainnya.

```go
package main

type RoundHole struct { radius int }
func (rh RoundHole) fitsTo (r RounderPeg) bool {
	return r.GetRadius() <= rh.radius
}

type RounderPeg interface {
	GetRadius() int
}

type RoundPeg struct { radius int }
func (r RoundPeg) GetRadius() int {
	return r.radius
}

type SquarePeg struct { width int }

type RounderSquareAdapter struct {
	s SquarePeg
}

func (ra *RounderSquareAdapter) GetRadius() int {
	return ra.s.width * 2^2 / 2
}

func main() {
	hole := &RoundHole{5}
	roundPeg := &RoundPeg{5}
	hole.fitsTo(roundPeg) // true
	
	smallSquarePeg := SquarePeg{5}
	largeSquarePeg := SquarePeg{10}
	
	smallAdapter := &RounderSquareAdapter{smallSquarePeg}
	largeAdapter := &RounderSquareAdapter{largeSquarePeg}
	
	hole.fitsTo(smallAdapter) // true
	hole.fitsTo(largeAdapter) // false
	
}

```

Adapter pattern akan membuat kita menyediakan *middle-layer* yang bertugas sebagai "adapter" atau meng-"adopsi" antara client code dengan *third-party*, *legacy class*, atau interface apapun yang tidak compatible.

## Pros
- Single reponsibility principle. Kita dapat memisahkan logic konversi dengan *primary business logic*
- Open/Closed Principle. Kita dapat meng-extend atau menambah implementasi adapter baru tanpa harus merubah implementas adapter yang sudah ada.

## Cons
- 