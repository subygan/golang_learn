
# golang_learn
Learning Golang

## Intro
- Every go program is made of packages and "main" package is run first.

```go
package main

import (
	"fmt"
	"math/rand"
)

func main() {
	fmt.Println("My favorite number is", rand.Intn(10))
}
```
In the above program math/rand has a file with the first statement *package rand*


### Import statements
these are called factored imports

```go
import (
	"fmt"
	"math/rand"
)
```

imports can be done this way as well

```go
import "fmt"
import "math/rand"
```

### Functions
functions can take zero or more variables. The type comes after the variable names

```go
package main

import "fmt"

func add(x int, y int) int {
	return x + y
}

func main() {
	fmt.Println(add(42, 13))
}
```

If similar stuff are together they can be clubbed together like so,

```go
a, b int
```

__functions can return any number of results__
#### Named return value:
Function values can be named for example

```go
package main

import "fmt"

func split(sum int) (x,y int) { // note the return declaration

    x = sum * 4 / 9;
	y = sum - x;

    return
    //only an empty return statement is enough
}
```

### Variables with initializers
 A *var* declaration can derive type from the initializer value

 ```go
package main

import "fmt"
func main() {
    var x = 10;
    fmt.Println("%T:  %v \n",x,x);
}
```

#### Short variable declaration using :=
Inside a function := is available to implicitly type derive from initialisor.    
Outside a function all declaration has to use the suffix (var, func and so on) so the short variable declarator is not available.

### Basic types

```
bool

string

int  int8  int16  int32  int64
uint uint8 uint16 uint32 uint64 uintptr

byte // alias for uint8

rune // alias for int32
     // represents a Unicode code point

float32 float64

complex64 complex128
```

### Zero values:

Variables declared without an explicit initial value are given their zero value

```
0 for numeric types,
false for the boolean type, and
"" (the empty string) for strings.
```

### type conversion
The expression *T(v)* is used to type cast between compatible types.

```go
var i int = 42
var f float64 = float64(i)
var u uint = uint(f)
```

or

```go
i := 42
f := float64(i)
u := uint(f)
//Note that these are only possible inside functions and not outside
```

#### Type inference

When the right element was type unspecified and the left element is type specified, then it is easily derived

```go
var i int
j := i
```

But in case of values it depends on the type of value

```go
i := 42 //int
j := 3.142 //float
k := 0.867 + 0.5i //complex
```

### Constants
Constants are similar to var, but cannot be changed later on
constants cannot be declared using := assignment

# Flow control statements

## For loop

There is only *for* loop in golang.
It has 3 components seperated by semicolons

-   The init statement executed in the first iteration
-   The condition expression that gets evaluated before every iteration
-   The post statement: executed at the end of the statement

Unlike C and C++ it doesn't require parenthesis. but the curly braces is necessary.

```go
package main

import "fmt"

func main() {
    sum := 0

    for i:=0; i<100; i++ {
        sum+=i
    }
    fmt.Println(sum)
}

```
The init and post statements are optional

```go
package main

import "fmt"

func main() {
    sum := 0
    i:=0

    for ; i<100 ; { //Notice that there are no init or post statements
        sum+=i
    }

}
```

You can drop the semicolons as well

```go
package main

import "fmt"

func main() {
    sum := 0
    i := 0

    for i<100 { //note no semicolon unlike previous
        sum+=i
    }

}
```
if the init condition is omitted, then it loops forever. There is no while in golang

```go
package main

import "fmt"

func main() {

    for {
        //this repeates forever
    }

}
```

## if statement

similar to for, if statements don't require parentheses () but they need braces {}

```go
package main

import(
    "fmt"
    "math"
)

func sqrt(x float64) string{
    
    if x<0 {
        return sqrt(-x)+"i"
    }
    return fmt.Sprint(math.sqrt(x))
}

func main() {
    
    fmt.Println(sqrt(2), sqrt(-4))

}

// prints: 1.4142135623730951 2i
```

### If with a short statement

If can have initialising statement. the variable is only available within the scope of the if.

```go
package main

import ( 
    "fmt"
    "math"
)

func main(){

    if v:=1;v<10 { //note the initialising statement
        fmt.Println(v);
    }

}
```

### if and else blocks
Any variable initialised at the if block is also available at the else block

## Switch statements


Switch statement is a simpler way of writing if else functions.
Go's switch statements have the same syntax as if, they can have an initialising statement which makes them available everywhere.

```go
package main

import (
	"fmt"
	"runtime"
)

func main() {
	fmt.Print("Go runs on ")
	switch os := runtime.GOOS; os {
	case "darwin":
		fmt.Println("OS X.")
	case "linux":
		fmt.Println("Linux.")
	default:
		// freebsd, openbsd,
		// plan9, windows...
		fmt.Printf("%s.\n", os)
	}
}
```

Switch evaluation order
- top to bottom, 
- stops executing whenever the right sequence is seen

#### Switch with no condition.

- Switch without any condition, makes the case as true by default. This makes it an easier way to write if conditions

```go
package main

import ( 
    "fmt"
    "time"
)


func main() {
    t := time.Now()

    switch {
        case t.Hour() < 12 :
            fmt.Println("Good Morning!")
        case t.Hour() < 17 :
            fmt.Println("Good Afternoon")
        default:
            fmt.Println("Good evening")
    }
}
```

## Defer statements
Defer statements execute after everything around it has been executed and returned

Defer functions are pushed into a call stack and executed in last-in-first-out order

Defer statement's __**arguments**__ are evaluated immediately. But are executed at the end
```go
package main
import "fmt"

func main() {
    defer fmt.Println("World") // This will be printed in the end

    fmt.Println("Hello")
}
//This would print: Hello World

```

## Pointer
Pointers are references to the memory ho

```go
var p *int
```

The & operator generates a pointer to its operand

```go
i := 42 //implicit type int
var k *int = &i
```

The * operator denotes the value behind the pointer

```go
fmt.Println(*p) // read i through the pointer p
*p = 21         // set i through the pointer p
```

# Struct 
A struct is a collection of fields. 

```go
package main

import "fmt"

type Vertex struct {
    X int
    Y int
}

func main() {
    fmt.Println(Vertex{1,2})
}
```
## Struct Literals
Structs can be defined by ```{Name: value}``` syntax.

```go
package main

import "fmt"

type Vertex struct{
    X,Y int
}

func main() {
    v = Vertex{X:1, Y:2} //The order doesn't matter

}
```

Struct fields are accessed using a dot

```go
package main

import "fmt"

type Vertex struct {
    X int
    Y int
}

func main() {
    k := Vertex{2,3}

    fmt.Println(k.X)
    fmt.Println(k.Y)
}

```
### Pointers to Struct
Struct fields can be accessed through pointers.
But it is cumbersome to write *<pointer_name> every time. So the * can be ignored without explicit dereference.

```go
package main

import "fmt"

type Vertex struct {
    X, Y int
}

func main () {
    v:=Vertex{1,2}
    v.X = 23
    fmt.Println(v) //prints { 23 2 }

}
```

# Arrays
The type ```[n]T``` creates an array of n values of type T.

```go
var a [10]int
//Creates an array 'a' of int type with size 10
```

### Slices

An array has a fixed size. A slice is dynamically-sized
The type ```[]T``` is a slice of elements with type T
Slices require high and low point in the array ```[low:high]```

```go
package main
import "fmt"

func main(){
    v := [6]int{1,2,3,4,5,6}
    var s []int = v[2:5]
    fmt.Println(s)
    //prints [3,4,5,6]
}
```

### Slices are reference to arrays
If a value in a slice is changed, the underlying array changes as well
Other slices that shares those slices will see the changes too

```go
package main

import "fmt"

func main(){

    names := [4]{
        "Alice", "bob", "Cathie", "David"
    }

    fmt.Println(names)

    a:=names[0:2]
    b:=names[1:3]
    fmt.Println(a,b)
    //prints ["Alice","bob"] ["Cathie", "David"]
    
    b[0] = "XXX"
	fmt.Println(a, b)
	fmt.Println(names)
}
```

### Slice defaults
When slicing, the higher or lower bounds can be ignored. Similar to python
```go
s := [10]int{1,2,3,4,5,6,7,8,9,0}
// s[0:10] = s[:] = s[0:] = s[:10]
```

### Slice length and capacity
length is the length of the slice, it can be got using ```len(s)```
capacity is the length of the underlying array, it can be got using. ```cap(s)```

### Null Slices

The zero value of a slice is nil.
A nil slice has a length and capacity of zero and has no underlying array.

```go
package main

import "fmt"

func main(){
    var s[]int //creating a slice
    fmt.Println(s, len(s), cap(s))
    if s == Nil {
        fmt.Println("nil")
    }
}
```

### Creating a slice with make (Dynamic array)

This is how you create a dynamic array

```go
var a:= make([]int, 5)
```

To specify capacity a third argument can be passed 

```go
var b:=make([int])
```

### Slices of slices

Slices can contain any type, including other slices

```go
package main

import "fmt"

func main(){
    board := [][]string {
        []string{'_','_','_'}
        []string{'_','_','_'}
        []string{'_','_','_'}

    }

    board[0][0] = "X"
	board[2][2] = "O"
	board[1][2] = "X"
	board[1][0] = "O"
	board[0][2] = "X"
}
```

### Appending a Slice

```go
package main

import "fmt"

func main() {
    var s[]int
    fmt.Println(s)

    //append works on nil slices, slice grows as needed
    append(s,1)
    fmt.Println(s)

    //more than one element can be added
    append(s,1,2,3,4)
    fmt.Println(s)

}
```
### Iterating through a slice (Range)

Iterating through a slice with the ```range``` key word is possible. each range would return two elements. a key and a value so both of those should be captured.

```go
package main

import "main"

func main(){
    s[]int{1,2,3,4,5,6,7,8}
    for i,v := range s {
        fmt.Println(i,v)
    }
}

```

### Skipping Ranges

index of value in ranges can be skipped by using _ as a placeholder.

note: This is not possible for random variables like rust

```go
package main

import "fmt"

func main(){
    s[]{1,2,3,4,5,6,7}
    for _,v := range s {
        fmt.Println(v)
    }
}
```

### Maps
A map maps keys to values.

```go
package main

import "fmt"

type Vertex struct{
    Lat, Long float64
}

var m map[string]Vertex //type declaration for map

func main(){
    m = make(map[string]Vertex)
    m["A"] = Vertex{1.23412, 1.23456}
    fmt.Println(m["A"])
}
```

### Map Literals

Map literals are like struct literals but the keys are required

```go
package main

import "fmt"

type Vertex struct{
    lat, lang float64
}

var m map[string]Vertex {
    "A": Vertex{1.234, 1.0987},
    "B": Vertex{22.2342, 5.12321}
}

func main(){
    fmt.Println(m)
}
```
The type inside the map can be ignored if it is a top-level type

```go
type Vertex struc{
    lat, long float64
}

var m map[string]Vertex{
    "A": {1.22354, 5.623434 } //note the the 'vertex' Key word is not there
    "B": {6.123412, 9.1232}
}
```

### changing values in maps Mutating Maps

updating value in a map

```
m[key] = elem
````

retrieve a key

```
elem = m[key]
```

delete an element

```
delete(m,key)
```
Test that a key is present

```
elem, ok := m[key]
```
if ok is false i.e key not present then the elem is the zero value of the struct

### Function Values
Functions are values too, they can be passed around just like other values.

```go
package main

import "fmt"

func compute( fn func(float64 float64) float64 ) float64 {
    return fn(3, 4)
}

func main(){
    hypot := func(x,y float64) float64 {
        return math.Sqrt(x*x + y*y)
    }

    fmt.Println(hypot(4,5)) //This is a function call using the variable that was assigned the function

    fmt.Println(compute(hypot)) //sending the hypot function variable to compute, which is another function
}
```
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


## Goroutines

Goroutine is a lightweight thread managed by the Go runtime. 
a "thread" does not mean virtual thread or even multiple thread. it is like async await. meaning there is one main thread and multiple seperate "threads" spawned which are technically running in the same thread instead of spawning one for every process. Each of these threads are blocked when a blocking process is run. and repicked when it is done.


`go f(x,y,z)`  runs a new  go routine with `f(x,y,z)` 
The evaluation for f, x, y, z happens in the current routine  and the execution happens in the new go routine.

```go
package main

import {
    "fmt"
    "time"
}

type Ab int

func say(s string) {
    for i:=0; i<10 ; i++ {
        time.Sleep(100*time.Millisecond)
        fmt.Println(s)
    }
}

func main(){
    go say("hello")
//  ^ This is run in a different thread
    say("world")
}

```

### Channels

channels are typed conduit through which you can send and receive values with the certain operators.

each channel is a seperately spawned goroutines that awaits for new data.


```
ch <- v

v:= <-ch

```

Like maps and slices, channels must be created before being used

` ch := make(chan int)`

Data flows in the direction of the arrows 

By default, sends and receives block until the other side is ready. This allows goroutines to synchronize without explicit locks or condition variables.

```go
package go

import "fmt"

func sum(s []int, c chan int){
    //              ^^^^ note the 'chan' keyword
    sum:=0
    for i,v := range(s) {
        fmt.Println(i)
        sum+=v
    }
    c <- sum
    // ^ sending the sum to the channel
}

func main() {
    s:=[]int{1,2,3,44,6,7,8,9,0}
    
    c:= make(chan int) // making a channel which is able to receive int type

    go sum(s[:len(s)/2],c) //creating seperate runtimes
    go sum(s[len(s)/2:],c) //creating seperate runime

    x,y := <-c, <-c  // getting the variable from the channel

    fmt.Println(x,y,x+y)
}
```

### Buffered Channels
Channels can be buffered to have a certain amount of value.
like a numbered of list of elements in the channel

```go
package main

import (
    "fmt"
)

func main(){
    ch := make(chan int, 2) // -> The 2 say that,only 2 int type variables can be held at any given time
           //      ^^ note the new variable

    c <- 12
    c <- 11

    c <- 10 //REMOVE!!! This would throw error as max only 2 can be assigned

    fmt.Println(<-c) //prints 12


    c <- 10 //This works!! 🤯 because the first one has been relieved

    fmt.Println(<-c) //prints 11
    fmt.Println(<-c) //prints 10


}

```

## Range and Close

A sender can close a channel to indicate that there are no more variables to be sent.
Receivers can test whether a channel has been closed by assigning a second parameter to the receive expression. Like how we do for errors in functions


`v, ok := <-ch`

`ok` is `false` if there are no more values to receive and the channel is closed.

The loop `for i := range c {}` loops infinitely until the channel is empty


Note:

- Only the sender should close a channel, never the receiver. __Sending on a closed channel will cause a panic__
- Channels aren't like files; you don't usually need ot close them. __Closing is only necessary when the receiver must be told there are no more value__

```go
package main

import(
    "fmt"
)

func fibonacci(n int, c chan int){
    x,y := 0,1

    for i := 0; i<n ; i++{
        c <- x
        x,y = y, x+y
    }
    close(c) //closes the channel
}

func main(){
    c := make(chan int, 10)
    go fibonacci(10, c)

    for i := range c {
        //   ^^^^^^^ this creates an iterator of sorts with the stream coming from the channel, until the channel is closed.
        fmt.Println(i)
    }

}
```

## Select

A select is similar to a switch statement. But a select statement works on channels. it uses whatever channel is available now.

select statements blocks and waits on multiple communication operations.
A select statement picks one at random if multiple channels are available.


```go
package main

import "fmt"

func fibonacci(c, quit chan int){
    x,y := 0,1

    for {
        select{
    //   ^^ goroutine is blocked here, until atleast a channel is ready
            case c <- x:
                x, y = y, x+y
            case <- quit:
                fmt.Println("quit")
                return
        }
    }

}


func main(){
    c:= make(chan int)
    quit := make(chan int)

    go func x(c , quit chan int){
//  ^^ starting a go routine here, to push data to the channel    
        for i:=0; i<10; i++{
            fmt.Println(<-c)
        }
    }(c,quit)

    fibonacci(c,quit)
}
```

## Default Selection

The default case in a select is run if no other case is ready.

use a default case to try a send or receive without blocking.

```go
select {
    case i:= <- c:
        //use i
    default:
        // receiving from c would block
}
```

This is helpful to use as a fallback

```go
package main

import "fmt"

func main(){
    tick := time.Tick(100 * time.Millisecond)
    boom := timeAfter(500 * time.Millisecond)

    for {
        select {
            case <- tick:
                fmt.Println("tick.")
            case <- boom:
                fmt.Println("Boom!")
            default:
        //  ^^^^^^^ using default would make this function an unblocking thread
                fmt.Println("      .")
                time.sleep(50* time.Millisecond)
        }
    }
}
```

