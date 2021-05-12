---
title: "Go Crash Course VII: Functions, Closures and Defer"
date: 2021-05-11T02:12:42-04:00
draft: false
author: "Mofi Rahman"
description: "Intro to go tutorial series part VII"
image: "image/get-going-part-vii.png"
imageAttr: "Gopher"
categories:
  - go
tags:
  - go
  - programming
  - basic
archives: "2020/05"
---

[Part 6](/post/go-crash-course-vi/)

## Functions

We talked about functions in some detail in a previous chapter. But lets cover some more features of functions these time. Functions are first class citizens of go syntax. This means functions are like any other types and can be passed to other functions and returned from functions. But one more thing we can do with function is define function types.

```go
type HandlerFunc func(ResponseWriter, *Request)
```

> The above is from actual source code of go pkg net/http `HandlerFunc`

Being able to create type definitions on functions is awesome. One of the main reason we would want to do this for is it lets us write less code. 

```go
func takesFunc(func (int, string, bool) (bool, error)) int

//vs

type CrazyFunc(int, string, bool) (bool, error)
func takesFunc(c CrazyFunc) int
```

Between the two, I would argue the later is more readable and and glance-able. Anyone jumping into the code base can easily know what contract `takesFunc` needs to fulfill. Also if we have multiple functions that takes the `CrazyFunc` as an argument we end up writing less code. Its a win-win.

But the more important benefit of being able to define function types is that we can attach methods to functions. We have not talked about methods yet (We will learn more about methods when we talk about structs), but in short methods are functions attached on a specific type. We denote something as method by using a receiver argument that appears between the `func` keyword and function name. Being able to define methods are really powerful because it lets us implement interfaces with functions. Like the example of the `HandlerFunc` function. We attach a method `ServeHTTP` That calls the function itself and now our `HandlerFunc` implements the `Handler` interface which is needed for building `http` servers.

```go
func (f HandlerFunc) ServeHTTP(w ResponseWriter, r *Request) {
	f(w, r)
}
```

Now in our code where we needed to pass a handler like in the `Handle` method from `*http.ServeMux` we can send any function with the same signature as `HandlerFunc` type casted to `HandlerFunc`. 

> In practice we probably will use the `HandleFunc` method that does the casting for us. But its possible. 

If you did not understand everything we talked about here do not worry. This is meant to give you a quick glimpse of what is possible. We need some more basic knowledge to be able to utilize all of these properly.

## Closures

Before we can understand closures we need to understand anonymous function. 

### Anonymous Function

Anonymous functions as the name suggests are anonymous. They are functions that does not have a name. In go we can define a function inline as a variable or as a return.

```go
adder := func(a int, b int) int {
	return a + b
}
```

### Closures

Closures are anonymous function that refer to variables defined outside the function scope. The scope has to be valid for the function. 

```go
func counter() func() int {
	i := 0
	return func() int {
		i++
		return i
	}
}
```

In this example we have a function `counter` that returns an anonymous function which refers to variable declared in `counter`. So the inner function that we return is a closure.

In our code we can use this function

```go
counter1 := counter()
counter2 := counter()

fmt.Println(counter1()) // 1
fmt.Println(counter2()) // 1
```

We have created two independent counters that will grow every time they are called. A counter is probably a pretty dumb example for closure but I hope it gets the point across. There is [this amazing article by Jon Calhoun](https://www.calhoun.io/5-useful-ways-to-use-closures-in-go/) that goes into even more depth where closures can be utilized to improve our code.

## Defer

In Go the defer keyword is used to postpone the execution of function until the surrounding function returns.

```go
func deferredPrint() {
	defer fmt.Println("defer 1")
	fmt.Println("regular thing")
	defer fmt.Println("defer 2")
}
```

Executing this `deferredPrint` will print

```bash
regular thing
defer 2
defer 1
```

There are two key observations here. First, our `fmt.Println` function call without defer executed first. This makes sense because defer postpones the execution of the other function calls until we reach the end of the function. Second, the print from our defer functions seems to be in reverse order. This is not random. Every defer we have will execute in reverse order. 

Defer is particularly useful when we want to take some action no matter what happens, like closing a db connection or closing a file. Defer is also useful for panic recovery. We have not talked about panic yet. Usually its never a good idea to panic in our program but there are cases when its appropriate. We will talk about panic in details in a later chapter.

## Next Steps

This is Part 7 of this Go crash course series.

[Part 8](/post/go-crash-course-viii/)
