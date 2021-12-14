---
emoji: 🐭
title: Learning golang 04 - Goroutine
description: Learning Go routine from go tour
layout: base
type: page
---

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

