---
title: "Go Crash Course Part I: Introduction"
date: 2021-05-04T02:40:30-04:00
draft: false
author: "Mofi Rahman"
description: "Intro to go tutorial series part I"
image: "image/get-going-part-i.png"
imageAttr: "Gopher"
categories:
  - go
tags:
  - go
  - programming
  - basic
archives: "2020/05"
---

# Go

[Go](https://golang.org) is an open source programming language known for its simplicity and ease of use. It has great concurrency primitives. I have been an user of go for about 4 years now. I will try to collect things I have learned over the last few years to help you get GOing (pun intended 😄!)

## Installation

Go is available to download for all OS.

Instruction for installing go is [here](https://golang.org/doc/install)

> If you are Mac user you can use brew to install Go.

## Go features

There are many programming languages out there. Why would you choose go? I think there are a few reasons why Go can be very useful.

- It is compiled. So small binaries and great for building containers.
- It is fast. Probably not as fast as c/c++ or rust, but for most use cases Go is faster than Java, Python or JavaScript.
- It is simple. Although this is fairly subjective. But for most people familiar with any other language getting comfortable with the go syntax is a matter of days. The language does not have too much things. So the learning curve is not too steep.
- The standard library is very good. For most thing you want to do with go, you don't have to go looking for external libraries. The standard library does an excellent job in giving us the tool we need. For the things we need to use libraries for we can find it [here](https://pkg.go.dev/)
- The community around go is one the best around. From the Go team to the advocates of Go, I find the people very welcoming and helpful. 

## Get Coding

Talk is cheap, show me the code.

```go
package main

import (
    "fmt"
)

func main() {
    fmt.Println("Hello, World")
}
```

All Go source code is organized in `package`. For the entry point of our application we use package main. Our code execution starts at `func main`. That is also how we define a `function` in Go. Go code executes in sequence. All our `import` are at the top of the file. In this example we are importing the `fmt` package. `fmt` package has many io related functions. We use the `Println` method to print to console. 

We can access the source code for this [here](https://github.com/moficodes/go-crash-course)

```bash
git clone git@github.com:moficodes/go-crash-course.git
cd hello-world
go run main.go
```

> If you don't have your ssh keys setup in Github you can also clone using https. But ssh is preferred. 

We should see it output `Hello, World` in our terminal.

## Next Steps

This is Part 1 of this Go crash course series. 

[Part 2](/post/go-crash-course-ii/)