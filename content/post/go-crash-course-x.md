---
title: "Go Crash Course X"
date: 2021-05-24T16:38:40-04:00
draft: false
author: "Mofi Rahman"
description: "Intro to go tutorial series part VIII"
image: "image/get-going-part-x.png"
imageAttr: "Gopher"
categories:
  - go
tags:
  - go
  - programming
  - basic
archives: "2020/05"
---

[Part 9](/post/go-crash-course-ix/)

## Interface

Interface is a collection of method signatures. In go interfaces are implemented implicitly. So it is possible for a single type to implement multiple interfaces. For example the `os.File` [type](https://golang.org/pkg/os/#File) implements dozens of interfaces from the `io` package alone like `Reader`, `Writer`, `ReadCloser`, `ReadSeeker`, `Closer`, `WriteSeeker`. Because it is implicit it can also implement newer interfaces from other packages and from packages you write. 

### Implementing Interface

Any user defined type can implement interfaces. Usually we see interfaces implementation on structs. But we can very easily do this on any other type definitions.

```go
type bob int

func (b bob) ServeHTTP(w http.ResponseWriter, r *http.Request) {
	w.Write([]byte("hello world!"))
}
```

This `bob` type here now implements the [`handler`](https://golang.org/pkg/net/http/#Handler) interface and can be used to serve http request. 

But usually we would be using a struct type to implement interface because we need additional fields in our methods. 

Lets look at the classic shape interface 

```go
type Shape interface {
	Area() float64
}
```

So something is a `Shape` as long as it implements the methods listen in `Shape` interface. 

```go
type Circle struct {
	Radius float64
}

type Rectangle struct {
	Width  float64
	Height float64
}

func (c *Circle) Area() float64 {
	return math.Pi * c.Radius * c.Radius
}

func (r Rectangle) Area() float64 {
	return r.Height * r.Width
}
```

Now we have two struct `Circle` and `Rectangle` that has the `Area` method. Pay attention to the pointer receiver on type `Circle`'s Area method. 

```go
func GetArea(s Shape) float64 {
	return s.Area()
}

func main() {
	circle := Circle{5.0}
	r1 := Rectangle{4.0, 5.0}
	r2 := &Rectangle{10.0, 15.0}
	GetArea(circle) // this will give error
	GetArea(r1)
	GetArea(r2)
}
```

Say we have a function called `GetArea` that takes `Shape`. Then we can create a few variables of type `Circle` and `Rectangle`. `circle` is of type `Circle`, `r1` is of type `Rectangle` and `r2` is of type `*Rectangle`. Since `Area` method is is attached to value receiver of `Rectangle` both `r1` and `r2` can be used as `Shape`. But `circle` does not work as `Shape`. The exact error we get is 

```bash
cannot use circle (type Circle) as type Shape in argument to GetArea:
        Circle does not implement Shape (Area method has pointer receiver)
```

Basically if a method has pointer receiver to satisfy the interface we need the pointer of the type. Which is a bit different to how structs work. We can call `Area` on value of `circle`

```go
area := circle.Area() // this works fine although circle is a value
```

## Type Cast

We can store any value in a variable of type `interface`

```go
var x interface{} = "hello"

str, ok := x.(string)
```

We can create a variable `x` with value `"hello"`. Since we know the value is of type string we can type cast that interface using the `.(type)` syntax. This returns the value and a bool which is set to false if the value could not be converted. Notice we can only do this because the underlying type was string. We can do similar things for structs as well. 

## Type Switches

```go
var y interface{} = "go"
switch v := y.(type) {
case int:
	fmt.Printf("Twice %v is %v\n", v, v*2)
case string:
	fmt.Printf("%q is %v bytes long\n", v, len(v))
default:
	fmt.Printf("I don't know about type %T!\n", v)
}
```

The use cases for these are not as common. Usually in Go we try not to pass around `interface{}` unless we absolutely have to. We might see specific interface being passed to function. But in those cases we never have to type cast them to concrete types as we are more interested in the behavior rather than the underlying fields.

## Next Steps

This is Part 10 of this Go crash course series.


