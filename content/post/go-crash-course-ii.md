---
title: "Go Crash Course Part II: Package, Import, and Export"
date: 2021-05-04T04:18:04-04:00
draft: false
author: "Mofi Rahman"
description: "Intro to go tutorial series part II"
image: "image/get-going-part-ii.png"
imageAttr: "Gopher"
categories:
  - go
tags:
  - go
  - programming
  - basic
archives: "2020/05"
---

[Part 1](/post/go-crash-course-i/)

## Package Scope

We structure our code in packages. We also import other packages from standard library and external sources. In the last application we had only one file `main.go`. But we could also have more than one file in our application. All the files in the same folder level are in the same package. All variables and functions in the package scope are shared. 

> If you haven't already clone [this repository](https://dev.to/moficodes/go-crash-course-part-1-18h1#get-coding) from [Part 1](/post/go-crash-course-i/)

```bash
# from the root of the repository.
# if you are in any folder change directory to root
cd packaging
```

The folder structure of packaging folder looks like this 

```bash
.
├── file1.go
├── file2.go
└── main.go
```

The content of the files are following.

```go
// main.go
package main

import "fmt"

func main() {
	fmt.Println(data1, data2)
}

// file1.go
package main

var data2 = "world"

// file2.go
package main

var data2 = "world"
```

The files does not have much things. But it will be enough to demonstrate.

Lets try to build it again.

```bash
go run main.go
```

We get an error. 

```
# command-line-arguments
./main.go:6:14: undefined: data1
./main.go:6:21: undefined: data2
```

Huh! I said just a moment ago that the variables in package are shared. But this is saying the variables are undefined. What could this be. 

The problem is actually with our `go run` command. We are telling `go` to run the `main.go` file. But it is not the only file needed for our application to work. We need to let `go` build tool know that it needs to include the other files too. We can do one of two things

```bash
go run main.go file1.go file2.go
#or
go run *
```

We will talk about how to build packages for external use in a later chapter.

## Import

We can import any standard library packages by putting them at the top of our file. If we have more than one import we can add multiple import statements or putting in an import block.

```go
import (
	"import1"
	"import2"
)
```

There are many packages in the standard library. You can find them [here](https://golang.org/pkg/#stdlib). These packages come bundled with Go runtime. 

But what if we want to use a package that is not part of the standard library. We will use Go modules for that. 

> Go modules are the way we manage dependencies in our Go application. It is comparable to `package.json` for node or `requirements.txt` for python. 

You can create a new module with `go mod init <modulename>`. To download a module we run `go get <modulepath>`. This updates our `go.mod` file with the module. If you clone a repository with a populated `go.mod` file. You can download the necessary modules with `go mod download`.

From the root of the repository

```bash
cd importing
go mod download
go run main.go
```

We should see on our terminal some thing like this print.

```bash
hello world
{"level":"debug","time":"2021-05-04T02:43:23-04:00","message":"hello"}
```

## Exporting

Exporting can mean two different things. One is package level export. Exporting local packages to be used in other local packages. And the other is exporting our amazing library for the world to use. When you are building a Go application that you expect other people to import use the module path that represents the git repository. For example if your code is going to `github.com/username/pkg` name the module as `github.com/username/pkg` 

In our example our code will be living in `github.com/moficodes/go-crash-course/exporting` so we create a new module in the `exporting` folder with that name. We have another folder called `print` with a file `print.go` and  package `print`. The file structure looks like the following.

```bash
.
├── go.mod
├── main.go
└── print
    └── print.go
``` 

We can import this `print` package in the `main` package with 

```go
import "github.com/moficodes/go-crash-course/exporting/print"
```

In our main function we try to use two variables defined in `print` package. 

```go
// print/print.go
package print

var data = "hello"
var Number = 1
```

If we try to run our application from `exporting` folder:

```bash
go run main.go
```

We would see: 

```bash
# command-line-arguments
./main.go:10:14: cannot refer to unexported name print.data
```

What just happened? We got an error about one of the two variables defined in our file as not being exported. But we did not put any special instruction to export either of those variables. 

In Go anything that starts with an uppercase letter gets exported. This is different from languages like Java or JavaScript where you have to explicitly declare something being exported. This is pretty nice because the language semantics actually carry meaning. 

The same uppercase rule is true for imported packages too. 

## Next Steps

This is Part 2 of this Go crash course series.

[Part 3](/post/go-crash-course-iii/)