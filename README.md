# simplemux

A simple HTTP mux library for Go.

## Installation

```bash
go get github.com/joeychilson/simplemux
```

## Example

```go
package main

import (
	"fmt"
	"log"
	"net/http"

	"github.com/joeychilson/simplemux"
)

func loggingMiddleware(next http.Handler) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		log.Println("Before handling the request")
		next.ServeHTTP(w, r)
		log.Println("After handling the request")
	})
}

func main() {
	mux := simplemux.New()

	mux.Use(loggingMiddleware)

	mux.Get("/", func(w http.ResponseWriter, r *http.Request) {
		fmt.Fprintln(w, "Hello World")
	})
	mux.Get("/:name", func(w http.ResponseWriter, r *http.Request) {
		name := simplemux.Param(r, "name")
		fmt.Fprintln(w, "Hello", name)
	})
	mux.Get("/users/:id", func(w http.ResponseWriter, r *http.Request) {
		id := simplemux.Param(r, "id")
		fmt.Fprintln(w, "Get user", id)
	})
	mux.Delete("/users/:id", func(w http.ResponseWriter, r *http.Request) {
		id := simplemux.Param(r, "id")
		fmt.Fprintln(w, "Delete user", id)
	})
	mux.Put("/users/:id", func(w http.ResponseWriter, r *http.Request) {
		id := simplemux.Param(r, "id")
		fmt.Fprintln(w, "Replace user", id)
	})
	mux.Patch("/users/:id", func(w http.ResponseWriter, r *http.Request) {
		id := simplemux.Param(r, "id")
		fmt.Fprintln(w, "Update user", id)
	})

	log.Printf("Listening on :8080")
	http.ListenAndServe(":8080", mux)
}
```
