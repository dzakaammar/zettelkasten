---
date: 2021-Aug-15
tags: ["software_design", "design_pattern", "oop", "golang"]
---
# Bridge Pattern

Bridge pattern memisahkan sebuah object besar menjadi 2 struktur hirarki yaitu *Abstraction* dan *Implementation*. *Abstraction* adalah sebuah interface yang mendefine sebuah object. Sementara *Implementation* adalah interface yang mendefine apa saja yang dilakukan object tersebut.

## Karakteristik
- Adanya sebuah  *Abstraction* yang menyediakan kontrol logic secara *high-level* dan bergantung pada *Implementation* untuk melakukan *actual work* secara low-level
- Adanya sebuah *Implementation* interface
- Adanya *Concrete Implementation*. Bisa lebih dari satu.
- Tugas *Client* adalah menyambungkan atau link antara *Abstraction* dengan *Implementation*
- *Abstraction* dapat berupa interface atau konkret type. Gunakan interface jika terdapat banyak variant.

```go
package user

type Model struct {
	Name, Email string
}

// Repository adalah Implementaion
type Repository interface {
	Create(u *Model) error	
}

// Service adalah Abstraction
// sebagai contoh adalah konkret object
type Service struct {
	r Repository
}

func NewService(r Repository) *Service {
	return &Service{r: r}
}

func (s Service) Create(u *Model) error {
	return s.r.Create(u)
	
}

```

```go
package inmemory

// UserRepository adalah implementasi/konkret type dari Repository
// yang bertindak sebagai Implementation. Dalam hal ini bertindak
// sebagai storage ke in-memory (hash map)
type UserRepository struct{
	m map[string]string
}

func New() *UserRepository {
	return &UserRepository{
		m: make(map[string]string),
	}
}

func (ur *UserRepository) Create(u *user.Model) error {
	ur.m.[u.Name] = u.Email
	return nil
}

```

```go
package db

import (
	"database/sql"
	"errors"
)

// UserRepository adalah implementasi/konkret type dari Repository
// yang bertindak sebagai Implementation. Dalam hal ini bertindak
// sebagai storage ke db sql.
type UserRepository struct{
	db *sql.DB
}

func New(db *sql.DB) *UserRepository {
	return &UserRepository{
		db: db,
	}
}

func (ur *UserRepository) Create(u *user.Model) error {
	if ur.db == nil {
		return errors.New("db is nil")
	}
	return nil
}

```

```go
package main

import (
	"adapter/inmemory"
	"adapter/user"
	"adapter/db"
)

func main() {
	inmemRepo := inmemory.NewUserRepository()
	service := user.NewService(inmemRepo)
	err := service.Create(&user.Model{
		Name: "foo",
		Email: "foo@bar.com",
	})
	if err != nil { panic(err) } // tidak akan panic
	
	dbRepo := db.NewUserRepository(nil) // set nil
	service := user.NewService(dbRepo)
	err := service.Create(&user.Model{
		Name: "foo",
		Email: "foo@bar.com",
	})
	if err != nil { panic(err) } // akan panic
	
}
```

Bridge pattern menyediakan cara untuk men-*split* monolithic class/type beberapa class/type yang hirarkis, agar masing-masing class/type tersebut dapat berkembang secara independen. Hal ini memudahkan untuk memaintain code dan mencegah terjadi nya kerusakan pada existing code.

## Pros
- Orthogonal (independent) structure
- Client code hanya mengetahui high-level abstractions saja, tidak perlu mengekspose detail dan implementasi platform
- Open/Closed Principle
- Single Responsibilty Principle

## Cons
- Kompleksitas implementasi code.