---
emoji: 🐭
title: Learning golang 03 - Functions
description: Functions from the go tour
layout: base
tags: ["tech", "programming"]
---

### Functions and pointers
Functions can receive pointers.
Any change to the pointer would change the value.

```go
package main

import "fmt"


func change(v *int) {
    *v = 10
//  ^ required, mentioning that the base value is being changed
}

func main() {
    a := 12

    change(&a)
    //     ^ sending argument as pointer. Throws an error if not.

    fmt.Println(a)

}

```


### Function closures

A function closure references variable from outside the function. Each closure is left to its own values

```go
package main

import "fmt"

func adder() func(int) int {
    sum:=0 //note this variable this is key

    return func (x int) int {
        return sum += x
    }
}

func main(){
    pos, neg = adder(), adder()  // here the closure is returned with seperate sum variable each

    for i:=0;i<10;i++ {
        fmt.Println(
            pos(i),
            neg(-1*i)
        ) //notice that the values get added, the value from the previous iteration is stored in the su variable
    }
    
}
```

## Methods
Go does not have objects but you can write methods on types.
This is done using a special **__receiver__** argument.
Receiver comes between the func keyword and the function name

A receiver does not directly change the underlying value. There is a section below about pointer receivers.

```go
package main

import "fmt"

type Vertex struct{
    x,y int
}

func (v Vertex) Abs() float64 {
//     ^^^ Note this, this is the receiver argument comes between func key word and function name
return math.Sqrt(v.x * v.x + v.y * v.y )
}

func main(){
    v := Vertex{3,4}
    fmt.Println(v.Abs())
}
```

### Methods are functions
Methods are functions with the receiver argument

Same program as above written as a function

```go
package main

import "fmt"

type Vertex struct{
    x,y int
}

func Abs(v Vertex) float64{
    return math.Sqrt(v.x*v.x + v.y*v.y)
}

func main() {
    v = Vertex{1,2}
    fmt.Println(Abs(v))
}
```

A Method with a receiver can only be created in the __same package__ 

Methods can be declared on non struct types as well,
for example,

```go
package main

import "fmt"

type Myfloat float64   // non-struct type

func (f Myfloat) Abs() Myfloat {
    return f*f
}

func main() {
    var a Myfloat = 1.234

    fmt.Println(a.Abs())
}

```

### Pointer Receivers in methods

Receivers can be pointers as well, This helps in mutating the values.

```go
package main

import "fmt"

type Vertex struct{
    X,Y int
}

func (v *Vertex) change() {
    // ^^^ this is very important without it, a new instance of vertex is sent to the method, with those values. which means the changes done below won't reflect in the original value

    v.X = 1
    v.Y = 3
}

func main(){
    
    v := Vertex{10,20}
    v.change()
    fmt.Println(v) //see the changes here.

}   
```

here the method receives a pointer to the instance mutating the instance itself.


## Interfaces

An interface type can hold any value that implements those methods

```go 
package main

import "fmt"

type AreaFinder interface{
        //      ^^^^ note the key word, means that this can hold any type that has the method below.   
    Area()
}

// below comes standard types
type Circle struct{
    R int
}

type Square struct{
    A int
}

//Random won't have the Area() method
type Random struct{
    A int
}


func (c Circle) Area(){
    fmt.Println(3 * c.R * c.R)
}

func (s Square) Area(){
    fmt.Println(s.A * s.A)
}

func main (){

    var a AreaFinder // Now 'a' can be assigned a Circle or a square but not a Random
    
    a = Circle{3} //this works
	
	a = Square{4} //this works as well 🤯
	
	a = Random{6}
	fmt.Println(a)

}
```

Interfaces for Methods that implement using pointers, expect only a pointer to that struct. if anything else is given, error is thrown.

__even though this would be valued exactly the same way as a method__

```go
package main

import "fmt"

type F struct {
    a float64
}

type I interface {
    Abs()
}

func (f *F) Abs(){
    //  ^ note that this is a pointer receiver
    fmt.Println("Abs is called")
}


func main() {
    var g,h I

    g = &F{2} // this will work as we are assigning only pointer

    h = F{3}  // this will fail as this is not a pointer.
}
```

Interfaces are __implemented implicitly__ This means that, there is no need to specify that a certain type implements said method.

Interfaces also take up the type that they are being assigned to. Imagine Interface being a tuple of value and type ```(value, type)``` and it dynamically varies based on the value supplied internally.

```go
package main

type I interface{
    M()
}

type F float64

type T struct{
    S string
}

func ( f F) M(){
    fmt.Println(f)
}

func ( t *T ) M() {
    fmt.Println(t.S)
}

func main(){
    var i I
    describe(i) // This will print an interface with nil values
    
    i = &T{"Hello"}
    describe(i)
    i.M()

    i = F(1.2323)
    describe(i)
    i.M()

}

func describe(s I) {
    fmt.Println("(%v,%T)\n",s,s)
}

//Outputs:
/*
(<nil>, <nil>)
(&{Hello}, *main.T)
Hello
(1.2323, main.F)
1.2323
*/
```

### Interfaces and Nil values

When implementing an interface, nil pointer exceptions need to be handled as that could happen

```go
package main

import "fmt"

type I interface {
    M()
}

type T struct {
    S string
}

func (t *T) M() {
    
    // The below block is important to avoid future errors.
    if t == nil {
    
        fmt.Println("<nil>")
        return
    }
    fmt.Println(t.S)
}

func main() {
    var i I
    describe(i) // (<nil>, <nil>)

    var t *T
    i=t
    describe(i) // (<nil>, *main.T)
    i.M()

    i = &T{Hello}
    describe(i) //(&{hello}, *main.T)

}

func describe(i I){
    fmt.Println(i)
}
```

### The empty interface
Empty interfaces are possible in go

```
interface{}
```
An empty interface may hold value of any type. (Every type implements at least zero methods.)
empty interfaces are used to get in arguments of any type.
For example ```fmt.Println``` takes in and empty interface type to make it accept any value.

```go
package main

import "fmt"

func main() {
    var i interface{}
    describe(i)

    i = 42
    fmt.Println(i)

    i = "Suriya"
    fmt.Println(i)
}

func describe(i interface{}){
    fmt.Printf("(%v, %T)\n",i,i)
}
```


### Type assertions

Type assertion provides access to the interface value's underlying concrete value.

```
t:=i.(T)
```

This statement asserts that the interface value i holds the concrete type.
If i does not hold a T, the statement will trigger a panic.

```
t, ok := i.(T)
```

If i holds a T, then t will be the underlying value and ok will be true.
If not, ok will be false and it will be the zero value of type T, and no panic occurs.

```go
package go

import "fmt"

func do(i interface{}) {

    switch v := i.(type){
        case int:
            fmt.Printf("Twice %v is %v",v,v*2)
        case string:
            fmt.Printf("%q is %v bytes long\n", v, len)
        default:
            fmt.Printf("I don't know the type %T!\n",v)
    }

}
```

### Stringers

One of the most ubiquitous interfaces is stringer defined by the fmt package

```
type Stringer interface{
    String() string
}
```

The fmt package first searches for the Stringer interface in any type

```go
package main

import "fmt"

type Person struct{
    Name string
    Age int
}

//below implementation is what would be picked by fmt this time for printing
func (p Person) String() string {
	return fmt.Sprintf("%v (%v years)", p.Name, p.Age)
}


func main(){
    a := Person{"Arthur Dent", 42}
	z := Person{"Zaphod Beeblebrox", 9001}

    fmt.Println(a,z) //Arthur Dent (42 years) Zaphod Beeblebrox (9001 years)

}
```

### Errors

Similar to Stringer, go programs require error() method to articulate errors

This is to show the error of the specific change.

it is implemented like so

```
type error interface{
    Error()
}
```

Programs would return error value based on something


```go
i, err = strconv.Atoi("42")
if err != nil {
    fmt.Println("123")
}

```