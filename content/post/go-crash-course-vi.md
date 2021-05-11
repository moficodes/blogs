---
title: "Go Crash Course VI: Blocks, Scopes and Shadowing"
date: 2021-05-07T03:09:13-04:00
draft: false
author: "Mofi Rahman"
description: "Intro to go tutorial series part VI"
image: "image/get-going-part-vi.png"
imageAttr: "Gopher"
categories:
  - go
tags:
  - go
  - programming
  - basic
archives: "2020/05"
---

[Part 5](/post/go-crash-course-v/)

## Blocks

In go a block is defined with a set of `{}`. Usually when we create a function we are in a block already. But we can define a block pretty much anywhere.

```go
i := 10
{
	i := 5
	fmt.Println(i) // i is 5
}
fmt.Println(i) // i is 10
```

## Scope

Scope defines where certain variable would be defined in. A scope can be block scoped, function scoped or package scoped. Each scope encompasses the previous. A package scoped variable would be available to the function and block but not the other way around. 

```go
x := 10
var z int // z declared here
{
	fmt.Println(x) // this is fine
	y := 15
	z = 20 // defined here 
}
fmt.Println(y) // this is not fine
fmt.Println(z) // this is fine
```

## Shadowing

We can shadow any variable that is defined in the outer scope. 

```go
x := 10
{
	x := 15
	{
		x := 20
		fmt.Println(x) // 20
	}
	fmt.Println(x) // 15
}
fmt.Println(x) // 10
```

Just block level shadowing is uncommon. There are rare use cases for this. But I haven't found really good use case for block level shadowing or even for blocks for that matter.

## Next Steps

This is Part 6 of this Go crash course series.

[Part 7](/post/go-crash-course-vii/)