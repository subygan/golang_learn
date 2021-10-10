# go by example.
Following eli bendersky's gobyexample.com tutorial the idea is to capture only things that were not clear in the go tour.


### for loops

- there are no while loops in go. only for.
- There are multiple ways to declare a for loop.

```go
package main

import ("fmt")

func main(){
    i := 2

    // most basic type, with a single condition
    for i<=3{
        fmt.Println(i)
        i+i=1
    }

    // looping with initial/condition/after 

    for j:=7; j<=9; j++ {
        fmt.Println(j)
    }

    // looping infinitely (while loop)

    for n:=0; n<=5; n+={
        if n%2==0{
            continue // continues to the beginning of the loop
        }
        fmt.Println(n)
        
    }

}
```

## if statements

There are no ternary if functions in go

## Switch statements

Switch statements express conditionals across many branches.

```go
package main

import (
    "fmt"
    "time"
)

func main() {

    i := 2
    fmt.Print("write", i, "as")

    //### basic switch

    switch i {
        // ^ this is the case that is to be tested

        case 1:
        //   ^ This is what is compared against the i
            fmt.Println("one")
        case 2:
            fmt.Println("two")
        case 3:
            fmt.Println("three")
        default:
    //  ^^^ This is a fallback    
            fmt.Println("unknown")
    }

    // ### commas to seperate multiple expressions in the same case.

    switch time.Now().Weekday() {
        case time.Saturday, time.Sunday:
        //   ^^^            ^^^^   these two are checked against the given thing.
            fmt.Println("weekend")
        default:
            fmt.Println("weekday")
    }


    switch

}