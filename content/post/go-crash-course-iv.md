---
title: "Go Crash Course IV: Types, Conversion and Inference"
date: 2021-05-05T23:13:02-04:00
draft: false
author: "Mofi Rahman"
description: "Intro to go tutorial series part IV"
image: "image/get-going-part-iv.png"
imageAttr: "Gopher"
categories:
  - go
tags:
  - go
  - programming
  - basic
archives: "2020/05"
---

[Part III](/post/go-crash-course-iii/)

The code for this part is in the `types` folder under [this github repo](https://github.com/moficodes/go-crash-course). 

## Types

Go is a typed language. That means everything we create must have a type. 

There are a few basic types. 

### Boolean Types

Represents set of boolean value. With predefined constant `true` and `false`

```go
bool        true or false
```

### Numeric Types

Integer and Floating point values. Following are architecture independent numeric types.

```go

uint8       the set of all unsigned  8-bit integers (0 to 255)
uint16      the set of all unsigned 16-bit integers (0 to 65535)
uint32      the set of all unsigned 32-bit integers (0 to 4294967295)
uint64      the set of all unsigned 64-bit integers (0 to 18446744073709551615)

int8        the set of all signed  8-bit integers (-128 to 127)
int16       the set of all signed 16-bit integers (-32768 to 32767)
int32       the set of all signed 32-bit integers (-2147483648 to 2147483647)
int64       the set of all signed 64-bit integers (-9223372036854775808 to 9223372036854775807)

float32     the set of all IEEE-754 32-bit floating-point numbers
float64     the set of all IEEE-754 64-bit floating-point numbers

complex64   the set of all complex numbers with float32 real and imaginary parts
complex128  the set of all complex numbers with float64 real and imaginary parts

byte        alias for uint8
rune        alias for int32
```

These are architecture dependent. (Could be either 32 bit or 64 bit)

```go
uint     either 32 or 64 bits
int      same size as uint
uintptr  an unsigned integer large enough to store the uninterpreted bits of a pointer value
```

### String Types

String is a sequence of bytes. 

### Composite Types

#### Array

Arrays are fixed length and fixed type. 

```go
var arr = [5]int{}
```

#### Slice

Contiguous segment of underlying array. Has fixed type. Length can be dynamically reallocated. (More on this on a later chapter)

```go
var slice = []int{}
```

#### Maps

Key value store. Similar to dictionary in python or object in JavaScript. But it still has a fixed type.

```go
var maps = map[int]int{}
```

#### Functions

Functions are also types.

```go
var function = func() int {
	return 42
}
```

`function` has a type `func() int`

#### Struct
Custom type defined by user. Its a sequence of named elements  with name and types. Struct can be empty too.

```go
type empty struct{}
var x = empty{}
```

#### Channel

Channels are typed conduit between running go routines. We will hold off on talking about channels until we talk about go routines. 

### Interface

An interface type specifies a method set called its interface. If a any type has the method set of the interface, that type implements the interface. All types implement the empty interface `interface{}`. Interface does not have any type. Its always the underlying components type. A type can implement any number of interfaces. We will talk about interface in great details in a later post.

```go
var y interface{} = x // y now has a type of empty since that is the type of x.
```

### Type Alias and Type Definition

We already saw 2 examples of type aliases. `byte` and `rune` are type aliases of `uint8` and `uint32` respectively. This means every time we use `byte` we are basically using an `uint8`. We can also define our own type from another base type. 

```go
// type alias
type myFloat = float64
var aliasFloat myFloat = 5.0 // has type float64
// type definition
type myint int
var aliasInt myint = 1 // has type myint
```

Type alias is useful for moving types between packages for refactoring. Lets say we were using a struct type called `coolstuff` in a package. If we move the `coolstuff` to a different package, we would have to refactor all the places where `coolstuff` were being used. One way to mitigate issues during the refactor type is to create a type alias in the old package where `coolstuff` was.

```go
type coolstuff = newpackage.Coolstuff
```

Type definition is used usually for creating enums. Or sometimes to extend existing types by attaching methods. For example:

```go
func (m myint) double() myint {
	return m * 2
}
```

Type int does not have any methods. But with `myint` we can create a `double()` method on our type. We have yet to talk about method and method receiver. We will talk about it in depth.

## Type Conversion

Basic numeric types can be converted with `T()` syntax. Where `T` is the type to be converted to.

```go
var i int = 355        // type int
f := float64(i)        // type float64
by := byte(i)          // type byte (uint8)
```

Struct type can be converted to and from each other as long as they have identical field names and types in identical order.

```go
type person struct {
	age  int
	name string
}

type student struct {
	age  int // try swapping these fields
	name string
}

p := person{age: 20, name: "john"}
std := student{age: 15, name: "paul"}

p2 := person(std)
s2 := student(p)
```

> There is a way to type case any struct to any other struct using the `unsafe` package. This is rarely used. If you write code that uses unsafe package, you better have a solid reasoning for it. 

On function calls type conversion is automatic if the parameter can be converted to the desired type it gets converted. `int` will be converted to `float` but not the other way around because float to int can loose precision.

For defined types, the conversion is uni directional. You can always use the underlying type in place of the defined type but not the other way around.

```go
func definedTypeConversion(mi myint) {
	fmt.Printf("Type of mi=%T\n", mi)
}

func takesInt(i int) {
	fmt.Printf("Type of i=%d", i)
}

definedTypeConversion(aliasInt) 
definedTypeConversion(5) // this get converted to myint type

takesInt(5)
takesInt(aliasInt) // this does not work
```

## Inference

We have seen example of inference a few times already. Every time we use the `:=` operator, we are inferring the type of the left hand operand. 

```go
x := 5 // type inferred to int
y := x // type inferred to int because of the type of x
```

Type inference also happens for numeric constants when passing to a function if the compiler does not detect a loss of precision or overflow.

## Next Steps 

This is Part 4 of this Go crash course series.

[Part 5](/post/go-crash-course-v/)