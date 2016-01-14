# gores [![Build Status](https://travis-ci.org/alioygur/gores.svg?branch=master)](https://travis-ci.org/alioygur/gores)

http response utility library for GO


## installation

`go get gopkg.in/alioygur/gores.v1`


## usage

```go
package main

import (
	"log"
	"net/http"

	"gopkg.in/alioygur/gores.v1"
)

type User struct {
	Name  string
	Email string
	Age   int
}

func StringResponse(w http.ResponseWriter, r *http.Request) {
	gores.String(w, http.StatusOK, "Hello World")
}

func HTMLResponse(w http.ResponseWriter, r *http.Request) {
	gores.HTML(w, http.StatusOK, "<h1>Hello World</h1>")
}

func JSONResponse(w http.ResponseWriter, r *http.Request) {
	user := User{Name: "Ali", Email: "ali@example.com", Age: 28}
	gores.JSON(w, http.StatusOK, user)
}

func FileResponse(w http.ResponseWriter, r *http.Request) {
	err := gores.File(w, r, "./path/to/file.html")

	if err != nil {
		log.Println(err.Error())
	}
}

func DownloadFile(w http.ResponseWriter, r *http.Request) {
	err := gores.Download(w, r, "./path/to/file.pdf", "example.pdf")

	if err != nil {
		log.Println(err.Error())
	}
}

func main() {
	// Plain text response
	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		gores.Plain(w, http.StatusOK, "Hello World")
	})

	// HTML response
	http.HandleFunc("/html", func(w http.ResponseWriter, r *http.Request) {
		gores.HTML(w, http.StatusOK, "<h1>Hello World</h1>")
	})

	// JSON response
	http.HandleFunc("/json", func(w http.ResponseWriter, r *http.Request) {
		user := User{Name: "Ali", Email: "ali@example.com", Age: 28}
		gores.JSON(w, http.StatusOK, user)
	})

	// File response
	http.HandleFunc("/file", func(w http.ResponseWriter, r *http.Request) {
		err := gores.File(w, r, "./path/to/file.html")

		if err != nil {
			log.Println(err.Error())
		}
	})

	// Download file
	http.HandleFunc("/download-file", func(w http.ResponseWriter, r *http.Request) {
		err := gores.Download(w, r, "./path/to/file.pdf", "example.pdf")

		if err != nil {
			log.Println(err.Error())
		}
	})

	// No content
	http.HandleFunc("/no-content", func(w http.ResponseWriter, r *http.Request) {
		gores.NoContent(w)
	})

	// Error response
	http.HandleFunc("/error", func(w http.ResponseWriter, r *http.Request) {
		gores.Error(w, http.StatusInternalServerError, http.StatusText(http.StatusInternalServerError))
	})

	err := http.ListenAndServe(":8000", nil)
	if err != nil {
		log.Fatal("ListenAndServe: ", err)
	}
}
```

for more documentation [godoc](https://godoc.org/github.com/alioygur/gores)

## TODO

- [x] write tests
- [x] add file response
- [ ] more test

## Contribute

**Use issues for everything**

- Report problems
- Discuss before sending a pull request
- Suggest new features/recipes
- Improve/fix documentation

## Thanks & Authors

I use code/got inspiration from these excellent libraries:

- [labstack/echo](https://github.com/labstack/echo) micro web framework
