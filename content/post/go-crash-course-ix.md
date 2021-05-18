---
title: "Go Crash Course Ix"
date: 2021-05-17T22:25:52-04:00
draft: false
author: "Mofi Rahman"
description: "Intro to go tutorial series part IX"
image: "image/get-going-part-ix.png"
imageAttr: "Gopher"
categories:
  - go
tags:
  - go
  - programming
  - basic
archives: "2020/05"
---

[Part 8](/post/go-crash-course-viii/)

## Struct

If you are coming from a OOP language like Java finding struct in Go might make you relieved. Although it is possible to do most OOP like things in Go. It is not the Go way. Structs are not objects. Structs are values. Struct are a collection of fields.

```go
type Circle struct {
	X      float64
	Y      float64
	Radius float64
}
```

Here we have defined a struct type `Circle` that has 3 fields. To use a struct we can declare a new `Circle` variable like any other type. 

```go
c1 := Circle{
	X:      15.0,
	Y:      12.0,
	Radius: 8.5,
}

c2 := Circle{15.0, 12.0, 8.5}
```

These two are identical definition. Notice in `c1` we put the field name and in `c2` we omit it. We can only omit field names if a) we initialize all the fields and b) we omit all the fields. For example this is illegal

```go
c2 := Circle{15.0, Y: 18.0, 8.5} // This will not compile
```

### Accessing Fields

If a struct has exported fields and methods, we can use it in any other package after importing the package. We can not use unexported method or fields outside the package. To use a field or a method on a struct type we can use the `.` notation. 

```go
fmt.Println(c1.X, c1.Y, c1.Radius)
```

## Methods

We talked about function in a previous post. Methods are just like functions but with a receiver argument.

```go
func (c Circle) Area() float64 {
	return c.Radius * c.Radius * math.Pi
}

func Area(c Circle) float64 {
	return c.Radius * c.Radius * math.Pi
}
```

You can think of `func (c Circle) Area() float64` as the same as `func Area(c Circle) float64`. In the `Area` method we will have a variable `c` of type `Circle` in scope, so we can use the value. 

### Receiver vs Pointer Receiver

We can declare methods with pointer receivers. Pointer receivers can be a bit confusing at times. You may think if you have a pointer receiver method defined you won't be able to use that method on the value of the struct. That is not the case. One of the main reasons you would want to use the pointer receiver is to mutate the value the other would be if the value is two large and value receiver would have to make a big copy operation. [This StackOverflow Answer](https://stackoverflow.com/a/27775558/10272405) outlines it in much more details.  

```go
func (c *Circle) Area() float64 {
	return c.Radius * c.Radius * math.Pi
}

func (c Circle) Area2() float64 {
	return c.Radius * c.Radius * math.Pi
}
```

We can use the function as such

```go
fmt.Println(c1.Area() == c1.Area2())
```

## Embedded Structs

In Go we usually do no promote the idea of [inheritance](https://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)). In OOP inheritance is the concept of one type inheriting its methods and fields from another type.   Instead what we do have is embedded structs. In Go we try to compose as much as possible. We embed a struct in another by not adding a name to it.

```go
type Wheel struct {
	Circle
	Material string
	Color    string
	X        float64
}
```

With struct embedding we can use the methods and fields of `Circle` type from `Wheel` as if they were defined on the `Wheel` type. 

```go
w1 := Wheel{
	Circle: Circle{
		Radius: 10.0,
		X:      15.0,
	},
	Material: "Rubber",
	Color:    "Black",
	X:        5.0,
}

fmt.Println(w1.Area())
```

### Shadowing Embedded Structs

Let's take another look at our `Wheel` type.

```go
type Wheel struct {
	Circle
	Material string
	Color    string
	X        float64
}
```

We have a field `X` of type `float64` defined on `Wheel`. We also have a embedded struct `Circle` which also have field `X` defined. What would happen if we tried to print `w1.X`. Would it print `15` from the embedded `Circle` type or `5` from `X` defined on `Wheel` type. 

```go
fmt.Println(w1.X) // 5
```

This will print 5. `X` in `Wheel` shadows the `X` field in `Circle`. We can still access the `X` in `Circle` by accessing fields using the `.` notation.

```go
fmt.Println(w1.Circle.X) // 15
```

We can also shadow methods from embedded structs. We can embed any number of structs. Although this will make it harder to read our code.

The main difference between this type of composition and inheritance is that inheritance tries create a is relationship where composition create a has relationship. 

## Next Steps

This is Part 9 of this Go crash course series.



