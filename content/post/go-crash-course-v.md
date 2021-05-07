---
title: "Go Crash Course V: Constants, Operators and Reserved Keywords"
date: 2021-05-07T02:25:06-04:00
draft: false
author: "Mofi Rahman"
description: "Intro to go tutorial series part V"
image: "image/get-going-part-v.png"
imageAttr: "Gopher"
categories:
  - go
tags:
  - go
  - programming
  - basic
archives: "2020/05"
---

[Part 4](/post/go-crash-course-iv/)

## Constants

In a previous post I said go does not have immutable types. I was only half right. Go does have constants denoted with `const` but its not comparable to `final` in Java or `const` in JavaScript because `const` in go only works with basic types and for things we can know in compile time. Like boolean values, integer, floating point, characters, runes and strings. We can not create constants of composite types like struct, func or interface. All values must be known at compile time. You could store almost any arbitrary numeric constant in a untyped const. The problem happens when you try to use the value. 

```go
const (
	BigConst = math.MaxFloat64 * math.MaxFloat64
)
```

If you have this at the top of your go code and compile the code. It will be fine. But if you try to print this or try to cast

```go
fmt.Println(BigConst) // this code wont compile
```

This code won't compile because `BigConst` won't fit in a `float64`. 

### iota

We can generate a series of constants with `iota` constant generator.

```go
const (
	a0 = iota // 0
	a1 = iota // 1
	a2 = iota // 2
	a3 = iota // 3
)
```

We can also use the shorthand by skipping assignment to set values for a series of constants.

```
const (
	b0 = iota // 0
	b1        // 1
	b2        // 2
	b3        // 3
)
```

If we skip one or two value the `iota` generator continues counting.

```go
const (
	c0 = iota // 0
	c1 = 43
	c2 = 75
	c3 = iota // 3
)
```

I can't imagine a scenario where this will be useful though. But it's there.

`iota` generator is reevaluated per `const` block.

```go
const d0 = iota //0
const d1 = iota //0
const d2 = iota //0
const d3 = iota //0
```

`const` is uses almost like `enum` in other languages or for mathmetical constants. 

## Operators

Go supports a number of operators. Some of them are arithmetic operators. Some are punctuation. Other have specific meaning based on context. 

```
+    &     +=    &=     &&    ==    !=    (    )
-    |     -=    |=     ||    <     <=    [    ]
*    ^     *=    ^=     <-    >     >=    {    }
/    <<    /=    <<=    ++    =     :=    ,    ;
%    >>    %=    >>=    --    !     ...   .    :
     &^          &^=
```

[The official language spec](https://golang.org/ref/spec#Operators) talks about each of the operator in great details. 

Operators that are used for calculating values have a set precedence order. Operators of same precedence get evaluated left to right. 

```
Precedence    Operator
    5             *  /  %  <<  >>  &  &^
    4             +  -  |  ^
    3             ==  !=  <  <=  >  >=
    2             &&
    1             ||
``` 

For example 
```go
x := 5*10 + 6/3 | 7%4&15 
// (5*10) + (6/3) | (7%4)&15 => 50 + 2 | 3&15 => 52 | 3 = 55
```

This evaluates without ambiguity. But this is a classic example of just because you could, does not mean you should. This is not easily human readable. Using proper brackets will  increase readability.

## Reserved Keywords

Go has a fairly small set of reserved keywords. Other languages has a fairly large reserved keywords. c++ 95, Java 53, JS 64 etc. Go has only 25 reserved keywords. Reserved keyword mean in the syntax we are not allowed name anything with this names. 

```
break        default      func         interface    select
case         defer        go           map          struct
chan         else         goto         package      switch
const        fallthrough  if           range        type
continue     for          import       return       var
```

Having such a limited set of keywords has some drawbacks too.

As you can see none of the types are actually part of the reserved keywords. So one can create a variable called int. Now if we try to create a variable of type int, it won't work. 

```go
int := 10 // this is valid code
var c int = 10 // this doesnt work now
```

## Next Steps 

This is Part 5 of this Go crash course series.

[Part 6](/post/go-crash-course-vi/)