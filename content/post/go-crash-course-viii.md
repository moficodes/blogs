---
title: "Go Crash Course VIII: If, Switch and For"
date: 2021-05-11T21:49:20-04:00
draft: false
author: "Mofi Rahman"
description: "Intro to go tutorial series part VIII"
image: "image/get-going-part-viii.png"
imageAttr: "Gopher"
categories:
  - go
tags:
  - go
  - programming
  - basic
archives: "2020/05"
---

[Part 7](/post/go-crash-course-vii/)

## If

In programming often we have to have conditional execution of certain statements. In most programming languages we have the idea of `if`. 

```go
var x = 10
if x > 5 {
	fmt.Println("x is greater than 5")
}
```

In this example we have a variable `x` which has value of 10. Then we check whether or not `x` is greater than 5. If it is, we print something. Otherwise we do nothing.

### if - else if - else

Often we are checking for more than one related thing. Lets take the example of [FizzBuzz](https://en.wikipedia.org/wiki/Fizz_buzz) problem. For any number n, we want to print `Fizz` if n is divisible by 3, `Buzz` if n is divisible by 5, `FizzBuzz` if n is divisible by both 3 and 5 and n if its not divisible by either 3 or 5.

```go
if n%15 == 0 {
	fmt.Println("FizzBuzz")
} else if (n % 5) == 0 {
	fmt.Println("Buzz")
} else if (n % 3) == 0 {
	fmt.Println("Fizz")
} else {
	fmt.Println(n)
}
```

We can chain as many if-else if as we want. else is a special case where we do not have to have a conditional operator. It matches the case that no if - else if conditions match. 

It is almost always possible to skip the else branch in functions. For example

```go
func isEven(n int) bool {
	if n%2 == 1 {
		return false
	}
	return true
}
```

Instead of having a else block we can just return the default case. 

### If with statement

Sometimes we want to check some condition of variable we just created and we only need the variable for the condition. An example might help clear this up a bit more.

```go
func minRand(min int) int {
	rand.Seed(time.Now().UnixNano())
	if v := rand.Int(); v > min {
		return v
	}
	return min
}
```

We have a random number `v` and if v is greater than `min` we return `v` if not we would just return `min`. We do not need access to `v` outside the if condition. This helps us with readability as well as variable naming. In go we are not allowed to redefine a variable with a different type. In a large function having every variable in the same scope might cause trouble.

## For

In programming we sometimes need to do something multiple times. We achieve this using loops. Go has only one construct for looping. It is the `for` keyword. But `for` is pretty versatile in go where we do not need anything else like `do-while`, `while` like in some other language. 

### Anatomy of for

Generally a `for` loop has this structure

```go
for i := 0; i < 100; i++ {
}
```

We have a init that initializes our loop invariant. Then we have a condition that is used to terminate the loop and we have a change that is run after each loop iteration. 

All three of these are optional and can be used or omitted as we need.

```go
for i := 0; i < 5; i++ {
	fmt.Println(i)
}

for i := 0; ; {
	if i >= 5 {
		break
	}
	fmt.Println(i)
	i++
}

i := 0
for i < 5 {
	fmt.Println(i)
	i++
}

i = 0
for ; ; i++ {
	if i >= 5 {
		break
	}
	fmt.Println(i)
}

for i := 0; i < 5; {
	fmt.Println(i)
	i++
}

i = 0
for ; i < 5; i++ {
	fmt.Println(i)
}

for i := 0; ; i++ {
	if i >= 5 {
		break
	}
	fmt.Println(i)
}

i = 0
for {
	if i >= 5 {
		break
	}
	fmt.Println(i)
	i++
}
```

These are all possible combination of a for loop with a single invariant. All of these for loop do the exact same thing. 

### Break

At any point in the `for` execution we can use the `break` statement to break out of the closest `for` loop. 

### Mutli invariant for loop

We can also have for loops where we have two variables. 

```go
for i, j := 0, 10; i != j; i, j = i+1, j-1 {
	fmt.Println(i, j)
}
```

The basic structure is the same. We have a init where we initialize `i` and `j`. Condition to chech `i != j` and then we increase `i` and decrease `j`.

### Forever

If we run a for loop with no exit condition we runt he for loop forever. There are times we want to run a process indefinitely. 

```go
for{
// do work indefinitely 
}
```

For example if we look at the source code for [net/http](https://golang.org/src/net/http/server.go?s=99574:99629#L2980) we can see an example of indefinite for loop that waits for new connection to server.

## Next Steps

This is Part 8 of this Go crash course series.

[Part 9](/post/go-crash-course-ix/)