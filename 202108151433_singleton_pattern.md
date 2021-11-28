---
date: 2021-Aug-15
tags: ["software_design", "design_pattern", "oop", "golang"]
---

# Singleton Pattern

Singleton menyediakan cara untuk membuat *single instance* dan menyediakan cara untuk mengakses/menggunakan instance tersebut sebagai *global variable*. Memastikan hanya terdapat satu instance dari produk tertentu selama aplikasi berjalan.

## Karakterstik
- Terdapat global variable untuk mendefine *single instance*. Dalam banyak implementasi, global variable tersebut memiliki default value dan tidak kosong, untuk selanjutnya bisa diganti melalui fungsi *setter*
- Adanya *setter function* untuk mengisi value dari instance tersebut
- Adanya *getter function* untuk memberikan cara agar package lain atau client mengakses instance tersebut
- Instance yang digukan untuk singleton harus bersifat private dan memastikan hanya dapat diubah/diisi sekali dalam compile time atau pun runtime  
- Harus *conccurent safe* dan *nil pointer safe* (jika default value adalah nil)

> Untuk memastikan bahwa instance hanya dapat sekali diubah/diisi, implementasi di Go dapat menggunakan sync.Once.
> 
> Dan jika object yang dijadikan singleton ada structural type, maka bisa menggunakan abstraction atau interface agar object tersebut tidak dapat diubah dari sisi client. Hal ini terjadi karena object singleton acapkali berupa reference type (untuk menghindari passing by reference di *getter function*  dan membuat object bisa diubah dari sisi client), selain itu mencegah passing by value dimana akan selalu mengcopy instance tersebut di *getter function*

```go
package logger

type Logger interface{
	Println(s string)
}

type loggerNoop struct{}
func (l *loggerImpl) Println(s string) {}

var (
	lgr Logger = &loggerNoop{} // default logger noop
	once sync.Once

)

func Set(l Logger) {
	if l == nil { return }
	
	// untuk memastikan bahwa hanya 1 kali call Set yang akan diproses
	// selama runtime
	once.Do(func(){
		lgr = l
	})
}

func Get() Logger {
	return lgr
}

```

Dan implementasi disisi client 
```go
package main

import "singleton/logger"

type loggerWithFile struct{
	filename string
}

func (l loggerWithFile) Println(s string) {
	// implementation
}

func main() {
	logger.Set(&loggerWithFile{filename: "test.txt"})
	
	lgr := logger.Get()
	lgr.Println("test"
}
```

Gunakan singleton pattern ketika dalam keseluruhan program mengharuskan adanya *single instance* yang dapat dishare. Namun, penggunakan singleton harus dibarengi dengan kontrol yang *strict* terhadap global variabel yang dibuat (*gurantees that just one instance of a class*)

## Pros
- Dengan cara ini dapat diyakini hanya akan terdapat *single instance* selama runtime
- Instance tersebut dapat diakses secara global 

## Cons
- Single responsibility principle violation. 
- Perlu special treatment untuk menghandle *concurrent safe*
- Mungkin akan sulit untuk melakukan unit test, karena langsung terikat dengan global variable dan sulit untuk meng-override untuk keperluan mock