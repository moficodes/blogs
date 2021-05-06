---
title: "Go Crash Course Part III: Variables and Functions"
date: 2021-05-04T22:12:42-04:00
draft: false
author: "Mofi Rahman"
description: "Intro to go tutorial series part III"
image: "image/get-going-part-iii.png"
imageAttr: "Gopher"
categories:
  - go
tags:
  - go
  - programming
  - basic
archives: "2020/05"
---

[Part 2](/post/go-crash-course-ii/)

## Variables

In Go using the `var` keyword we can declare a list of variables of same type. 

```go
var a, b, c int
```

Variables are mutable types. Go does not support immutable types. (_Go does have constants but they are not the same as immutable types_). We can also define variables in the global scope. Once a variable is defined it can be used in its scope. 

> Scope is where a particular variable is valid. We will dive much deeper into scope in a later chapter. 

If we do also declare the variable the same time we define it. We can also declare a variable without defining its type.  For simple types

```go
var a = 10
var b int32 = 26
```

As we can see we can create variables with type declared and not declared. When we don't declare type, the type is inferred. Type inference always chooses the default type for value. For example 

```go
var a = 10 // int
var b = 1.5 // float64
```

We can also use short variable declaration.

```go
short := 10
```

One thing to note though, short variable declaration can not be used in global scope. 

### Naming Variables

- Has to start with Alphabets or _ `count or _data or _1`
- Should be Camel Case `userID` 
- Short names preferred over long 

[This Talk from 2014](https://talks.golang.org/2014/names.slide#1) sums it up nicely.

### Shadowing

Any variable defined in an outer scope can be shadowed by a new variable in inner scope. When shadowing we can also change type of variable.

## Function

We already saw some examples of functions. Our `func main` was a function. It is a special type of function. There is another special type of function. It is the `init` function. 

### init

`init` function gets executed before anything else in the package. We can also have more than 1 `init` function per package. All the `init` functions will execute before anything else in the package. If we have `init` function in multiple files in the same package, `init` function is executed in lexical file order.

For example in our [git repository](https://github.com/moficodes/go-crash-course) change directory into `functions` folder. 

We have 3 files in there. `a.go`, `b.go` and `main.go` we have a total of 5 `init` functions. 3 in `main.go` and 1 each on `a.go` and `b.go`.

```bash
cd function
go run *
```

We would see 

```bash
init from file a
init from file b
hello from init 1
hello from init 2
hello from init 3
Hello World
```

> This information should not be something we rely on while building application. This would be a terrible practice to build software on such assumption.

### main

`main` is the entry point to our application. After all `init` steps has finished execution of `main` begins. We can have only one `main` function per application. And `main` package can not be exported or imported by other.

Other than `main` and `init` users have free reign to define 
any functions in go. 

### Anatomy of functions

```go
func <funcname> (0 or more parameters)(0 or more return types) {

}
``` 

Function has a name. It can take 0 or more parameters and it can return 0 or more things from a function. 

For example:

```go
func Greet() {
	fmt.Println("hello")
}

func Add(a, b int) int {
	return a + b
}

func Divide(a, b int) (int, int) {
	return a / b, a % b
}
```

### Named returns

We can also have named return. Named returns can be useful when we have multiple returns and its not immediately clear what the values represent when returned. 

```go
func NamedDivide(a, b int) (quotient int, remainder int) {
	quotient, remainder = a/b, a%b
	return
}
```

In the previous `Divide` function. We had to look at the code to figure out that the first response was the quotient and the second was the remainder. In this example the function definition makes it super clear. We probably won't even need any more documentation.

### Naming functions

Same rules as variable naming apply. 

### Functions are values

Functions are first class citizens in Go. That means we can create variables that are of type function. As well as pass functions as function parameters and receive function from function return. 

```go
func TakeFunc(take func() string) {
	data := take()
	fmt.Println(data)
}

func GiveFunc() func() string {
	f := func() string {
		return "give"
	}
	return f
}
```

### Overloading

Go does not support function overloading. That means we can not have two function that has the same name but different signature. But we can always shadow a function in inner scope. 

For example we can define a new function in `main` called `Greet`. Now when we use `Greet()` we are using the inner function.

```go
Greet := func() string {
	return "shadow outer Greet"
}
```

## Next Steps

This is Part 3 of this Go crash course series.

[Part IV](/post/go-crash-course-iv/)
