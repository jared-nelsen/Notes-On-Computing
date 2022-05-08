# Go In Action

Started 10-16-19

## Goals of Go

Go is designed to be able to build simple and reliable systems. Simplicity is also a goal. One goal is to balance low-level systems language features with some high level features. Concurrency is also a major goal.

## Chapter 1 : Introducing Go

### Go routines

***Goroutines are functions that run concurrently with other goroutines.*** In other languages you would use threads to do the same thing but in Go many goroutines run on a single thread. In other words there can be many goroutines per thread. Goroutines use less memory than threads and are automatically scheduled against a set of logical processes.

``` Go
func log(msg string) {
    ... some logging code here
}

//This go keyword is all you need to kick off a goroutine
go log("HELP!")
```

Using the go keyword here kicks off a goroutine which will execute concurrently with other goroutines. It is not uncommon to kick off thousands of goroutines at once.

### Channels

***Channels are data structures that enable safe data communication between goroutines.*** Channels help you to avoid problems typically seen in programming languages around shared memory access. Channels help enforce a pattern that only one goroutine should modify the data at any time. The exchange of data between goroutines is synchronized: once the handoff occurs, both goroutines know about the exhange. Then a third goroutine can take the data. In this way, chains of execution on the same data can be implemented. If **copies** of the data are exchanged through a channel then each goroutine has its own copy and it **can be altered safely.** However, when **pointers** are exchanged then each goroutine **needs to be synchronized** if reads and writes will be performed by the different goroutines. 

### Go's Type System

Go provides a flexible **hierarchy-free** type system that enables code reuse with minimal refactoring. However, Go is still considered object-oriented.***Composition*** is used instead of inheritance. In Go, **types are composed of smaller types**. Go has a unique interface implementation that allows you to model behavior instead of modeling types. Most of Go's built in libraries are very small.

#### Types are simple

Go has built in types like int and string and also user-defined types. Go's types are ***similar to structs in C.*** However, types also declare **methods** that operate on that data.

#### Go interfaces model small behaviors

***Interfaces*** allow you to **express the behavior of a type.** If a value of a type implements an interface then it means the value has a specific set of behaviors. You dont need need to declare that youre implementing an interface; you just need to write the implementation. 

An example of an interface is the io.Reader interface:

``` go
type Reader interface {
    Read(p []byte) (n int, err error)
}
```

To write a type that implements the io.Reader interface you just need to implement a Read method that accepts a slice of bytes and returns an integer and a possible error. Go's interfaces are small and more aligned with single actions.

### Memory Management

Go is ***garbage collected!***

### Hello World in Go

```Go
package main
import "fmt"
func main(){
    fmt.Println("Hello, World!")
}
```

## Chapter 2: Go quick-start

```go
package main // Your main function MUST belong to the main package to produce an executable!

func init() {
    // Init is called before main
}
func main() {
    // This is the entry point to the program
}
```

The Go compiler wont let you declare a package to be imported if its not used. All init functions will be called before the main function in any file.

Each file in a package must contain the package it resides in as a package statement.

There are two environment variables that dictate where go will look for things:

```
GOROOT = "/Users/jared.nelsen/go
GOPATH = "Users/jared.nelsen/spaces/go/projects" // Like a workspace folder
```

A package-level variable is located outside of any function:

```go
var matchers = make(map[string]Matcher) // A map of Matchers with strings as keys. Make sure that the package level variables start with a lower case letter.
```

In Go, ***identifiers are either exported or unexported from a package.***

```go
//Exported identifiers start with a capital letter
int Help; //This is 'public' to other packages
int help; //This is 'private' to other packages
```

In Go, all variables are initialized to their **zero value:**

```
numeric types = 0
strings = ""
Booleans = false
pointers = nil
```

***Functions can have multiple return values!*** It is common to have functions return a value and a possible error value.

```go
// Return values are assigned in order of their declaration
x, err := RetrieveFeeds()
if err != nil {
    log.Fatal(err)
}
```

```go
// The := operator is used to declare and initialize a variable at the same time
feeds, err := RetrieveFeeds()
```

Once the **main** function returns in Go, the runtime will be terminated. It is good practice to gracefully terminate any goroutines that were launched prior to letting the main function return. This can be effected by a ***WaitGroup***.

```go
// Shows an anonymous function that waits until it is done to terminate
go func(matcher Matcher, feed *Feed) {
    Match(matcher, feed, searchTerm, results)
    waitGroup.Done()
}(matcher, feed)
```

***The blank identifier: _,*** is used as a substitution for variables. When you have a function that returns multiple values and you dont have the need for one you can use the blank identifier to ignore those values. If we wont be using the return value of a function then we can ignore it with _, .

```go
// Use of the blank identifier
for _, feed : range feeds {...}
```

***Anonymous functions*** can be lauched like:

```go
go func(x int, y int) {
    add(x, y);
    waitGroup.Done()
} (1, 2)
```

## Chapter 3: Packaging and Tooling

#### Packages

All Go programs are organized into ***packages.***  All .go files must declare the package that they belong to in the first line of the file. Packages are contained in a single directory.; you may not have multiple packages in the same directory or split a package across multiple directories. This means that all .go files in a single directory must declare the same package name.

#### Package Naming

Packages should use the same name as the directory containing it.

#### The Main Package

Being the main package will cause the go command to compile into a binary executable. Thus, all programs you build in go must have a main package. When the main function is encountered by the compiler it looks for the ***main()*** function. If it is not found then the compilation will fail. The final binary will take the name of the directory that it is contained in.

#### Package Information

You can get extra information on a package by calling ***godoc [package-name].***

#### Imports

The ***import*** statement is used to import packages in order to use the code contained within them.

```go
import(
	"fmt"
    "strings"
)
```

Packages are found on disk based on their relative path to the directories referenced by the Go Environment. Packages in the standard library are found under where Go is installed on your computer. Packages created by you or other Go developers are found inside the GOPATH, which acts like a personal workspace for packages.

#### Remote Imports

Go tooling has built in support for fetching source code from the web. The import path can be used by Go to determine where the code you need fetched is on the web. 

```go
import "github.com/spf/viper"
```

The command ***go get*** will fetch the remote imports.

#### Named imports

There may be cases where the imports of two packages have the same name. Similar to Python, we can just give the import a new name.

```go
import(
	"fmt"
    myfmt "mylib/fmt"
)
```

Not that the Go compiler will fail the build if you do not use a package that was imported.

#### Init

Each package has the ability to provide as many init() functions as necessary to be invoked at the beginning of execution time. All init() functions that are discovered by the compiler will be called before the main() function. The init() functions are great for preemptively setting up packages and initializing variables as needed before execution starts. A good example of this may be setting up database drivers.

#### Dont forget you can use the ***go*** command in the command like to use some powerful tools!

## Chapter 4: Arrays, Slices, and Maps

Go has 3 different collection data structures: arrays, slices, and maps.

#### Arrays

Arrays are a fixed length block of elements of the same type. Arrays are initialized to zero.

```go
// Declare an integer array of five elements
var array [5]int
```

```go
// Declare and initialize an array
array := [2]int{2,3}
```

```go
// Access an element
array := [2]int{2,3}
array[0] gives 2
```

You can have an array of pointers:

```go
// Declare an integer pointer array of five elements using special assignment schema
array := [5]*int{0: new(int), 1: new(int)}

// Assign values to index 0 and 1
*array[0] = 10
*array[1] = 20

Gives: [*->10, *->20, nil, nil, nil]
```

Arrays are values in Go: 

```go
var array1 [5]string
array2 := [5]string{"toot", "bloop"}
array1 = array2

Gives array1 = {"toot", "bloop"}
```

Multidimensional arrays:

```go
var array [4][3]int
// Use an array literal to declare and initialize a two dimensional integer array.
array := [4][2]int{{10, 11}, {20, 21}, {30, 31}, {40, 41}}
// Declare and initialize individual elements of the outer and inner array.
array := [4][2]int{1: {0: 20}, 3: {1: 41}}
// Access elements is the same in C++ and Java
array[0][2] = 10
```

Passing arrays between functions:

```go
var array [1e6]int
//Pass the array to the function foo
foo(array)
func foo(array [1e6]int){}
```

Passing arrays by pointer:

```go
var array [1e6]int
foo(&array)
func foo(array *[1e6]int){}
```

#### Slices

A ***slice*** is a data structure that provides a way for you to ***work with and manage collections of data***. Slices are built around the concept of dynamic memory and simulate the growth and shrinking of lists in a dynamic way by managing the memory underneath them. Memory is organized in slices in contiguous blocks which allows for things like iteration, indexing, and garbage collection.

A slice has ***three*** fields that constitute it: ***a pointer to the underlying array, the length of the amount of elements the slice has access to, and the capacity that the underlying elements have to grow.

Creating and initializing slices:

```go
slice := []string{99: ""} //Initialize the 100th element to an empty string//Create a slice of strings contains a length and capacity of 5
slice := make([]string, 5)
//Create a slice of strings with a length of 3 and a capacity of 5
slice := make([]string, 3, 5)
// The slice literal
slice := []string{"Red", "Blue"}
// Declare a slice with index positions
slice := []string{99: ""} //Initialize the 100th element to an empty string
// Create a nil slice
var slice []int; //Length 0, Capacity 0
// Declaring an empty slice
slice := make([]int, 0)
// An emtpy slice using a slice literal
slice := []int{}
```

Working with slices:

```go
slice := []int{10,20,30,40,50}
// Change a value in a slice
slice[1] = 25
// Take a slice of a slice
newSlice := slice[1,3] -> {20, 30, 40}
// Grow a slice using append()
newOtherSlice = append(newSlice, 60) -> {10,20,30,40,50}
// Iterating over a slice
for index, value := range slice {
    fmt.Printf("Index: %d Value: %d\n", index, value)
}
// Use a for loop to iterate over a slice
for index := 0, index < len(slice); index++ {
    ...
}
```

Multidimensional slices:

```go
// Create a slice of a slice of integers
slice := [][]int{{10}, {100,200}}
// Append the value of 20 to the first slice of integers
slice[0] = append(slice[0], 20) -> {{10, 20},{100, 200}}
```

Slices are passed by value. This is powerful because a slice is only 24 bytes and the underlying array is not copied:

```go
slice = foo(slice)
func foo(slice []int) []int {
    ...
    return slice
}
```

#### Maps

A map in Go is an unordered collection of key/value pairs. Maps are implemented using hash tables.

```go
// Create a map with a key type of string and a value type of int
dict := make(map[string]int)
dict := map[string]int{"Red": 1, "Orange": 2}
colors := map[string]string{} // Create an empty map
// Add to a map
colors["Red"] = "toot"
// Retrieve and test existence
value := colors["Blue"] // A value is always returned, if not exists then return nil
if value != ""{
    fmt.Println(value)
}
// Delete from a map
delete(colors, "Coral")
// Iterate over a map
for key, value := range colors {
    // Do fancy stuff
}
// Pass a map to a function
func removeColor(colors map[string]string, key string) {
    delete(colors, key)
}
```

## Chapter 5: Go's Type System

