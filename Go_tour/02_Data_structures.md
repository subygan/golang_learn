---
emoji: 🐭
title: Learning golang 02 - DS and algo
description: Data structures from the go tour
layout: base
type: page
---

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