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
