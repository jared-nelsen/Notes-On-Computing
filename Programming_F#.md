# Programming F# 3.0

F# is a multiparadigm programming language built on the .NET platform. It supports several different types of programming styles out of the box.

- F# supports imperative programming. You can modify the contents of memory, read and write files, and set data over a network.
- F# supports object oriented programming.
- F# supports functional programming. Recall that functional programming emphasizes what a program should do and not explicitly how it should do it.
- F# is statically typed. Type information is known at compile time, leading to type safe code.
- F# is a .NET language. It runs on the CLR. This means that it gets garbage collection and class libraries.

## Part 1. Multiparadigm Programming

```F#
let toPigLatin (word: string) = 
	let isVowel (c: char) =
		match c with
		| 'a' | 'e' |
		| 'A' | 'E' | -> true
		|_ -> false
		
	if isVowel word.[0] then
		word + "yay"
	else
		word.[1..] + string(word.[0]) + "ay"
```

F# is evaluated. It uses the FSI process: F# Interactive:

- Submitting this code will:
  1. Start the FSI process
  2. Send the code to the FSI process
  3. The FSI process evaluated the code you sent over.

### Values

Values are **immutable** and cannot be changed.

```F#
let greeting, thing = args.[0], args.[1]
```

F# allows you to use whitespace as delimiters. Tabs are prohibited.

```F#
// This is a comment
(* This is a block comment *) 
```

### F# Interactive

In FSI, every statement is terminated with a `;;`

### Managing F# Source Files

You can only call into functions and classes that are defined earlier in a code file or in a separate file compiled before the file where the code in question is. This is an artifact of type inference.

The order of the files in VS is important.

## 2. Fundamentals

### Primitives

To create a value, use the `let` keyword:

```f#
let x = 1;;
	-> val x : int = 1
```

Typical numeric primitives.

Typical numeric operations and types.

```F#
//Characters
let x = 'a';;
```

Typical escape sequences.

```f#
//Strings
let x = "toot";;
//Get a char
let y = x.[0];;
	-> val y : char = 't'; 
```

```f#
//Boolean
true
false
&& , || , not
```

Typical comparison arguments. <> is 'not equal to'

### Functions

```f#
let functionName parameterList = functionality;;
let square x = x * x;
// Call it
square 4;;
	-> val it : int = 16
```

There is no `return` statement. The function returns the last thing it evaluates.

### Type inference

```f#
// Integer is assumed if no other explicit information is given
let mult x y = x * y;;
	-> val mult : int -> int -> int
	
let mult x y = x * y;
let result = mult 5.0 4.0
	-> val mult : float -> float -> float
```

You can add a type annotation `(ident: type)`:

```f#
let add (x : float) y = x + y;;
	--> val add : float -> float -> float
```

### Scope

By default, values have `module` scope. 

Functions can be nested:

```f#
let f fParam =
    let g gParam = fParam + gParam + 1
    
    let a = g 1
    let b = g 2
    a + b;
    
val f : int -> int
```

### Control Flow 

```F#
if x = true then
	y + 1
else

You can also use elif
```

### Core Types

| unit       | Unit          | The unit value               | ()              |
| ---------- | ------------- | ---------------------------- | --------------- |
| int, float | Concrete Type | A concrete type              | 42, 5.2         |
| 'a         | Generic Type  | A free type                  |                 |
| 'a -> 'b   | Function Type | A function returning a value | fun x -> x + 1  |
| 'a * 'b    | Tuple Type    | An ordered group of values   | ("eggs", "ham") |
| 'a list    | List Type     | A list of values             | [1;2;3]         |
| 'a option  | Option Type   | An Optional Value            | Some(3), None   |

### Unit

The `unit` type is a value signifying nothing of consequence. Represented by `()`

```F#
let x = ();;
	-> val it : unit = ()
	
let square x = x * x;;
val square : int -> int
ignore(square 4);;
val it : unit = ()
```

### Tuple

Tuples are ordered collections of data. They can be nested.

```F#
let dinner = ("green eggs", "ham")
	-> val dinner : string * string = ("green eggs", "ham")
	
// You can extract values using let
let x, y = dinner;;

x, y = ("green eggs", "ham")
```

Functions can return tuples

### Lists

```f#
//Declaring lists
let someVowels = ['a'; 'e'; 'i';]
let emptyList = [];;

//Adding to the list with the 'cons' operator ::
let sometimes = 'y' :: someVowels;;
	-> val sometimes = char list = ['y'; 'a'; 'e', 'i']
	
//Appending a list to the end of another list
let first = [1;2;3]
let second = [4;5;6]
first @ second;;
	-> val it : int list = [1;2;3;4;5;6]
// Create numeric lists based on ranges
let x = [1 .. 10] // A List of intergers 1 to 10 inclusive
// Create a stepped list
let x = [0 .. 10 .. 50]
	-> val it : list = [0;10;20;30;40;50]
```

List comprehensions allow you to create lists in-line:

```f#
// Simple list comprehensions
let numbersNear x =
	[
		yield x - 1
		yield x
		yield x + 1
	]
-> val numbersNear : int -> int list

numbersNear 3;;
val it : int list = [2,3,4]

> // More complex list comprehensions
let x =
    [  let negate x = -x
       for i in 1 .. 10 do
           if i % 2 = 0 then
               yield negate i
           else
               yield i ];;

val x : int list = [1; −2; 3; −4; 5; −6; 7; −8; 9; −10]
```

A substitute for the 'do yield' clause is the -> operator

```f#
// Generate the first ten multiples of a number
let multiplesOf x = [ for i in 1 .. 10 do yield x * i ]

// Simplified list comprehension
let multiplesOf2 x = [ for i in 1 .. 10 -> x * i ]
```

### List Functions

| Length of a list                                             | List.length    |
| ------------------------------------------------------------ | -------------- |
| First element in a list                                      | List.head      |
| Returns the rest of a list without the head                  | List.tail      |
| Checks element existence                                     | List.exists    |
| Reverses elements in a list                                  | List.rev       |
| Returns some(x) where x is the first element for which the element returns true | List.tryfind   |
| Returns a joined list of tuples if the given lists are of the same length | List.zip       |
| Returns a list with only the elements for which the given function returns true | List.filter    |
| Look up this description ever time                           | List.partition |

### Aggregate Operators

- List.map

```f#
let square x = x * x;;
List.map square [1 .. 10] // maps the square function onto the range list
```

- List.fold

  ​	Use if you have a list of values that you want to distill down to a single piece of data.

  - List.reduce

    ```f#
    let insertCommas (acc : string) item = acc + ", " + item;;
    
    List.reduce insertCommas ["Jack"; "Jill"]
    val it = string = "Jack, Jill"
    ```

Using List.fold is an **accumulator** pattern:

```F#
let addAccToListItem acc i = acc + i;;

List.fold addAccToListItem 0 [1;2;3]
val it : int = 6
```

- List.iter

  List.iter iterates through a list and calls a function that you pass as a parameter. Because List.iter return unit, it is predominantly used for evaluating the side effect of the given method

```f#
// Using List.iter
let printNum x = printfn "Printing %d" x
List.iter printNumber [1 .. 5]

.. Prints 1 to 5
```

### Option

Use the `option` operator to represent a value that may or may not exist. The option type has only two possible values: Some('a) and None.

```F#
let isInteger str = 
    let successful, result = Int32.TryParse(str)
    if successful
    then Some(result)
    else None;;
```

| Option.isSome | Returns true if the option is Some  |
| ------------- | ----------------------------------- |
| Option.isNone | Returns false if the option is Some |

## Organizing F# code

### Modules

All code in F# is in a module. If the module is not named, i.e is anonymous, then it is in the namespace of the file it is in. They can be nested.

To create a module use the module keyword at the top of the file. All code in that file will then belong to that module.

```F#
module Alpha

//To refer to this value outside of the module use Alpha.x
let x = 1
```

### Namespaces

Namespaces cannot contain values, only type declarations.

```F#
namespace PlayingCards

type suit =
	| Spade
	| club
	
type PlayingCard =
	| Ace of suit
```

Modules are optimized for rapid prototyping whereas namespaces are geared towards larger scale projects using an object oriented approach.

## Program Startup

 Use [<EntryPoint>] to declare the main method of a program. For it to work it needs to:

- Be that last function defined in the last compile file in your project.
- Take a single parameter of type string array
- Return an integer, the exit code

```F#
[<EntryPoint>]
let main (args : string[]) = 

	...
	
	//Return 0
	0
```

# Chapter 3: Functional Programming

### Immutability

Values in functional programming are immutable by default.

Side effects are effects that execute outside of the logical flow of events like printing something or writing to a file.

```F#
let square x = x * x
let imperativeSum numbers =
	let mutable total = 0 //The mutable keyword must be called out in order to change a value
	for i in numbers do
		let x = square i
		total <- total + x
	total
let functionalSum numbers =
	numbers
	|> Seq.map square
	|> Seq.sum
```

### Function Values

In a functional programming language, functions are treated like any other piece of data. Functions can be passed as parameters to other functions. Functions can create and return new functions.

- **Higher Order Functions** are functions that take or return other functions as inputs or outputs.

```F#
// A higher order function
let negate x = -x

List.map negate [1 .. 10]
val it : int list = [-1;-2;-3 and the rest]
```

- **Lambda functions (anonymous functions)** can be used in the form `fun x -> functionBody`

```f#
(fun x = x + 3) 5
	-> val it : int = 8
	
//Rewriting our negation function
List.map (fun x = -x) [1 .. 10]
```

Be careful to keep lambdas simple

- **Partial function application** - The ability to specify some subset of parameters of a function and produce a new version of the function where those parameters are fixed

```f#
// This function appends text to a log file
open System.IO

let appendFile (filename : string) (text : string) = 
	use file = new StreamWriter(fileName, true)
	file.WriteLine(text)
	file.Close()
	
appendFile "An Address" "Some content"

//But we dont always want to pass in the address so...

let appendLogFile = appendFile "An address" // We redefined the function by applying half its arguments
appendLogFile = "Some content"
```

- **Currying** - Currying is the ability to transform a function taking n arguments into a chain of functions each taking one argument.

#### Recursive Functions

Use the `rec` keyword informs the type inference system to allow the function to be used as part of the type inference process.

```F#
let rec factorial x = //Notice the rec keyword
	if x <= 1 then
		1
	else
		x * factorial (x - 1)
```

### Mutual Recursion

There is a problem that is faced by the f# compiler when it compiles recursive functions that depend on one another because the order matters. To get around this we use the `and` keyword between function definitions:

```f#
let rec isOdd x = //This one rec keyword is used for both functions connected with the and keyword
	if x = 0 then false
	elif x = 1 then true
	else isEven(x - 1)
and let isEven = //Notice the and keyword
	if x = 0 then true
	elif x = 1 then false
	else isOdd(x - 1)
```

### Symbolic Operators

You can create a symbolic operator in F# similar to + - / or *. They can be made up of combinations of the ASCII special characters.

```f#
let rec (!) x =
	if x <= then 1
	else x * !(x - 1)
	
		val ( ! ) : int -> int
		
!5;;
	val it : int = 120
```

By default they use infix notation when they have more than one parameter.

Symbols can be passed around to higher order functions if you put parenthesis around the symbol.

```f#
let minus = (-);;
	val minus : (int -> int -> int)
List.fold minus 10 [3;3;3]
	val it : int = 1
```

#### The pipe forward operator

This is the pipe forward operator: `|>` . It allows you to rearrange the parameters of a function so that you present the last parameter of the function first. The benefit of the operator is such that you can continually reapply it to chain functions together.

```f#
// This function uses the pipe forward operator to find the size of all of the files in a directory
let sizeOfFolder folder = 
	let getFiles path =
		Directory.GetFiles(path, "*.*", SearchOption.AllDirectories)
	
	let totalSize =
		folder
		|> getFiles
		|> Array.map (fun file -> new FileInfo(file))
		|> Array.map (fun info -> info.Length)
		|> Array.sum
		
	totalSize
```

### The forward composition operator

`>>` is the forward composition operator. It joins two functions together with the function on the left being called first.

```f#
// This is how to string functions together
let square x = x * x
let toString (x : int) = x.ToString()
let strLen (x : string) = x.Length
let lenOfSquare = square >> toString >> strLen;;
```

#### The pipe-backward operator

If you want to call a function and pass the result to another function you have two choices: add parenthesis around the expression or use the pipe backward operator:

```f#
//With parens
printfn "The result of sprintf is %s" (sprintf "(%d, %d)" 1 2)
//With pipe forward
printfn "The result of sprintf is %s" <| sprintf "(%d, %d)" 1 2
```

The pipe backward operator isn't as common as the forward composition operator or the pipe forward operator but it serves to clean up F#.

#### The Backwards Composition Operator

The backwards composition operator `<<` takes two functions and applies the right function first and then the left.

```f#
//Backwards composition
let square x = x * x
let negate x = -x

// Using the >> negates the square
(square >> negate) 10
	val it : int = -100
// But what we really want is the square of the negation
(square << negate) 10
	val it : int = 100
```

Think of both the forward and backward composition operators as a tool to change precedence.

```f#
// You can use the << operator to filter
[ [1]; []; [3;4] [];]
List.filter(not << List.isEmpty)
```

### Pattern matching

Pattern matching is used to sort and sift through data. It is similar to a switch statement in OO languages but much more powerful.

Use the `match` and `with` keywords with `->`

```f#
let isOdd x = (x % 2 = 1)

let describeNumber x =
	match isOdd with
	| true -> printfn "x is odd"
	| false -> printfn "x is even"
	
// Another example
let testAnd x y =
	match x, y with
	| true, true -> true
	| true, false -> false
	| false, true -> false
	| false, false -> false
	
// _, means 'wildcard'
let testAnd x y = 
	match x, y with
	| true, true -> true
	| _, _ -> false
	
// Named patterns
let greet name =
	match name with
	| "Robert" -> printfn "Hello, Bob"
	| "William" -> printfn "Hello, Bill"
	| x -> printfn "Hello, %s" x;;
```

### When guards

Sometimes you need custom logic to determine whether a rule should match: if so, use a `when` guard. If a pattern is matched, the optional `when` guard will execute and the rule will fire if and only if the `when` expression evaluates to true.

```f#
match guess with
| _ when guess > secretNumber
	-> print "The secret number is lower"
...The rest of the cases
```

### Grouping Patterns

``` F#
let vowelTest c =
	match c with
	| 'a' | 'e' | 'i' | 'o'
		-> true
	| _ -> false
```

### Matching the Structure of Data

#### Tuples

```f#
let testXor x y =
	match x, y with
	| tuple when fst tuple <> snd tuple
		-> true
	| true, true -> false
	| false, false -> false
```

#### Lists

```f#
let rec listLength 1 =
	match l with
	| [] -> 0
	| [_] -> 1
	| [_;_] -> 2
	| hd :: tail -> 1 + listLength tail
```

#### Options

```f#
let describeOption 0 =
	match o with
	| Some(42) -> "The answer was 42, but what was the question?"
	| Some(x) -> sprintf "The answer was %d" x
	| None -> "There is no answer"
```

### Discriminated Unions

A DU is a foundational type. It is a type that can only be a one of a set of possible values. Each possible value value of a DU is referred to as a union case.

A Discriminated Union:

```f#
type Suit =
	| Heart
	| Diamond
	| Spade
	| Club
```

You can also optionally associate data with each union case:

```f#
type PlayingCard =
	| Ace of Suit
	| King of Suit
	| Queen of Suit
	| ValueCard of int * Suit
```

### Using DUs for Tree Structures

```F#
type BinaryTree =
	| Node of int * BinaryTree * BinaryTree
	| Empty
let rec printInOrderTree =
	match tree with
	| Node (data, left, right)
		-> printInOrder left
		   printfn "Node %d" data
		   printInOrder right
```

### Methods and Properties

You can add methods and properties to a DU:

```f#
type PlayingCard =
	| Ace of Suit
	... more
	
	member this.Value =
		match DO SOMETHING
```

### Records

Records are used to group data into a structured format without the need for hefty syntax. 

Records are a series of name - type pairs. To create an instance of that record simply provide a value for each record field.

```f#
type PersonRec = {First : string; Last : string; Age : int}
val steven : PersonRec = {First : "Steven"; Last = "Johnson"}
```

Advantages to Records:

1. Type inference can infer a records type.
2. Immutable by default 
3. Records cannot be subclassed giving a safety guarantee
4. Can be used with Pattern matching
5. Get structural comparison for free

Records can be cloned with the `with` keyword:

```f#
let car = {Make = "Lotus"; Model = "Elise"}
let nextYear = { car with Year = 2021}
```

You can add methods and properties to records as well. 

## Lazy Evaluation

F# supports lazy evaluation in two ways:

1. The `Lazy` type
2. Sequences

### Lazy Types

```F#
let y = lazy (printfn "Evaluating y..."; x.Value + x.Value)

//When the values are used they are evaluated
y.Value;;
Evaluating y...
Evaluating x...
val it : int = 20

//Using the value again will pull it from the cache
```

### Sequences

Lazy Evaluation is more commonly used in sequence form.

```f#
let seqOfNumbers = seq { 1 .. 5};

val seqOfNumbers : seq<int>

> seqOfNumbers |> Seq.iter (printfn "%d");;
1
2
..
5
val it : unit = ()
```

Seq has a lot of the same built in functions as List:

1. length
2. exists
3. tryFind
4. filter
5. concat

# Chapter 4: Imperative Programming

Imperative programming covers how to change program state and alter control flow.

Some benefits of imperative programming in F# are:

1. Improved performance
2. Ease of maintenance with clearer code
3. Interoperability 

### Mutable

The `mutable` keyword must be used to introduce mutable variables

```f#
let mutable message = "World"

printfn "Hello, %s" message :: Hello World
message <- "Universe"
printfn "Hello, %s" message :: Hello, Universe

```

The two restrictions on mutable values are:

1. Mutable values cannot be returned from functions
2. Mutable Values cannot be captured in inner functions (closures)

These issues can be rectified by storing the mutable data on the heap using a `ref` cell.

### Reference Cells

The `ref` type or 'ref cell' allows you to store mutable data on the heap. To retrieve the value of a `ref` cell you use a bang: `!` and to set the value you use the `:=` operator.

`ref` is not only the name of a type but also of a function that produces `ref` values. This `ref` function takes a value and returns a copy of it wrapped in a `ref` cell. 

```f#
let planets  = ref ["Mercury", "Venus"]

planets := !planets |> SOME OPERATION
```

#### Mutable Records

Record fields can be marked as `mutable` as well. This allows you to use records in the imperative style.

```F#
type MutableCar = {Make:string; Model:string; mutable Miles: int}

let driveAWhile car =
	let rng = new Random()
	car.Miles <- car.Miles + rng.Next() % 10000;
	
let kitt = {Make = "Honda"; Model = "S2000"; Miles = 0}
driveAWhile kitt;;
kitt car NOW HAS MORE MILES ON IT
```

### Units of Measure

There is a general problem in programming with the opacity of measuring something. Is it meters, feet, or inches. Sometime it is not enough to just pass around a value. ***Units of Measure*** in F# provide a way to avoid this class of issues.

```F#
[<Measure>]
type farenheit

let printTemperature (temp : float<farenheit>) // Noties that we typify the value with a type
```

Units of measure compound with multiplication or division. So if you divide a `meter` by another such as `second` the result will be encoded as `float<meter/second>`.

#### Defining a Unit of Measure

To define a Unit of Measure simply add the `[<Measure>]` attribute on top of a type declaration. Typically, Units of Measure are defined as *Opaque Types* meaning that they have no methods or properties at all. 

Sometimes it can be useful to add static methods that convert between units.

Units of Measure for most units come built into F#.

Units of Measure are also useful for abstract units like clicks, pixels, tiles and so on.

## Arrays

In F# Lists suffer from the same inefficiencies as other languages. Luckily we can use arrays to combat this.

Arrays can be constructed using *array comprehensions*:

```F#
let perfectSquares = [| for i in 1 .. 7 -> i * i|]

or

let perfectSquares2 = [| 1; 4; 9; 16; 25;|]
```

To retrieve an element from an array you can use the indexer operator `.[]`

```f#
let x = [|1;2;4|]
let y = x.[0] -> 1
let z = x.[1] -> 2
```

Out of range accesses will result in an `IndexOutOfRangeException`.

### Array Slices

Arrays can be sliced with this syntax: `array.[lowerBoundInclusive..upperBoundInclusive]`

Optionally: `array.[lowerBound..]` will slice to the right. and `array.[..upperBound]` will slice to the left.

### Array Pattern Matching

```f#
//Describe an Array
let describeArray arr =
	match arr with
	| null -> "The array is null"
	| [| |] -> "The array is empty"
	| [[x]] -> "The array has one element"
```

### Array Equality

Arrays are compared structurally: they are considered equal if they have the same rank, length, and elements.

```f#
[| 1 .. 5|] = [|1;2;3;4;5;|]
val it : bool = true
```

### Array Functions

Arrays have basically the same methods as Lists. 

## Multidimensional Arrays

### Rectangular Arrays

Rectangular arrays are solid blocks that have an elements for every space in an m X n grid.

```f#
let identityMatrix = float[,] = Array2D.zeroCreate 3 3
```

### Jagged Arrays

Jagged arrays are single dimensional arrays of single dimensional arrays.

```F#
let jaggedArray = int[][] = Array.zeroCreate 3
```

## Mutable Collection Types

### Lists

Mutable Lists act the same as C# and Java and are backed by arrays. 

```F#
open System.Collections.Generic
let planets = new List<string>();;
planets.Add("Mercury")
planets.Add("Venus")
planets.Count;; val it : int 4
planets.Remove("Mercury")
```

Lists have similar built in functions to any other language.

### Dictionary

Dictionaries have the same semantics as other languages.

### HashSet

```F#
let bestPicture = new HashSet<string>()
```

## Looping Constructs

F# has:

1. While Loops
2. For Loops
3. Enumerable For Loops

## Exceptions

The simplest way to report an error in F# is with the `failwith` function. It will throw an instance of a C# `System.Exception`

```f#
let divide x y =
	if y = 0 then failwith "Cannot divide by 0!"
```

### try catch finally

F# contains the traditional `try-catch` and `try-catch-finally` blocks.

### Defining Exceptions

You can define your own exceptions by inheriting from `System.Exception`

# Chapter 5: Object Oriented Programming

## System.Object

Everything in .NET is an instance of `System.Object` which means that some basic methods are avaiable. In F# this is abbreviated by `obj`. 

Some common methods are `toString`, `getHashCode`, and `Equals`.

## Object Equality

Objects are compared on a bit pattern level. However, reference types need more complex semantics. In this case references are equal if they are pointing to the same object.

Records and DUs are equal if all of the component parts are the same. This is called `generated equality`. 

## OOP is basically the same as C# except for keywords. Just look it up if you need it

# Chapter 7: Applied Functional Programming

This chapter covers some of the more powerful aspects of FP in F#.

## Active Patters

Active patterns are special functions that can be used inside of pattern match rules. They help capture succinct expressiveness.

Active Patters come in several forms:

1. Single Case active patterns convert the input into a new value
2. Partial Case active patters carve out an incomplete piece of the input space.
3. Multi Case active patters partition the input space into one of several values

### Single Case Active Patterns

Allows you to use the existing pattern match syntax on classes and values that otherwise wouldnt be able to be used.

To define an active pattern, define the function with the name enclosed with banana clips: `(| |)`

```f#
let (|FileExtension|) filePath =
	Path.GetExtension(filePath)
	
// Without active patters
let determineFileType (filePath : string) =
	match filePath with
	| filePath when Path.GetExtensions(filePath) = ".txt" -> printfn "This is a text file"
	
// With an active pattern
let determineFileType (filePath : string) =
	match filePath with
	| FileExtension ".jpg"
	| FileExtension ".png"
		-> printfn "It is an image file."
```

### Partial Active Patterns

There are cases when there isnt a 1:1 correspondence between input and output. For example, what happens when you write an active pattern to convert strings to integers.

This example proves why we need partial active patterns

```F#
//Active pattern to convert strings to ints
let (|ToInt|) x = Int32.Parse(x);;

//Check if the input string parses as the number 4
let isFour str =
	match str with
	| ToInt 4 -> true
	| _ -> false;;
	
isFour "4" -> val it : bool = true
isFour "x" -> Sytem.FormatException: Input string was not in a correct format
```

Partial Active Patterns allow you to define active patterns that dont always convert the input data. To do this partial active patterns return a `option` type. If the match succeeds it returns `Some` otherwise `None. The option type is removed as part of the pattern match when the result is bound.

To define a partial active pattern simple enclose with `(indent|_|)`

Another way to think about partial active patterns is that they are optional single-case active patterns.

```f#
//Partial Active Pattern example
let (|ToBool|_|) x =
	let success, result = Boolean.TryParse(x)
	if success then Some(result)
	else 		   None 
	
let (|ToInt|_|) x =
	let success, result = Int32.TryParse(x)
	if success then Some(result)
	else 			None
	
let describeString str =
	match str with
	| ToBool b -> printfn "%s is a bool with value %b" str b
	| ToInt i -> printfn "%s is an integer with value %d" str i
```

### Parameterized Active Patterns

Active Patterns can take parameters.

```f#
let (|RegexMatch|_|) (pattern : string) (input : string) =
	//Some pattern matching
	
let exampleMatch input =
	match input with
	| RegexMatch "some regex pattern" (month, day, year)
```

### Multicase Active Patterns

Sometimes you want to partition the input space into multiple categories. You can partition the input space into a known set of possible values. Use banana clips again but include multiple identifiers to identify the categories of output.

```F#
let (|Paragraph|Sentence|Word|Whitespace) (input : string)
```

### Using Active Patterns

Active patterns can be used in place of any pattern matching element: as part of let bindings or as parameters in lamdas.

### Combining Active Patterns

```F#
let (|KBInSize|MBInSize|GBInSize) filePath =
	let file = File.Open(filePath, FileMode.Open)
	if file.length < 1024L & 1024L then
		KBInSize
	elif file.length < 1024L * 1024L * 1024L then
		MBInSize
	else
		GBInSize
		
let (|IsImageFile|_|) filePath =
    match filePath with
    | EndsWithExtension ".jpg"
    | EndsWithExtension ".bmp"
    | EndsWithExtension ".gif"
        -> Some()
    | _ -> None
   
let ImageTooBigForEmail filePath =
	match filePath with
	| IsImageFile & (MBInSize | GBInSize)
		-> true
	| _ -> false
```

Active Patterns can be nested.

## Using Modules

Namespaces are a way to organize types. Modules behave like namespaces except that modules can contain values as well as types.

Typically, modules are not good to be shared and should be converted to classes instead.

## Mastering Lists

Lists in F# have similar performance pitfalls as other languages. They should be used accordingly.

### Cons

Cons `::` adds an element to the front of the list. The result of cons is a new list with the new element attached to the front. The old list keeps its contents. Cons is very fast

```F#
let x = [2;3;4];;

val x : int list = [2;3;4]

1 :: x;;
val it : int list [1;2;3;4;]

x;;
val it: int list [2;3;4]
```

## Tail Recursion

Tail Recursion is a special form of recursion where the compiler can optimize the recursive call to not consume any additional stack space. Tail calls drop the current stack frame before making the recursive call

An example of recursion that wont work without tail recursion:

```f#
let createImmutableList() = 
	let rec createList i max = 
		if i = max then
			[]
		else
			i :: createList (i + 1) max
		createList 0 1000;
```

An example of tail recursion

```f#
let factorial x =
	let rec tailRecusiveFactorial x acc =
		if x <= 1 then
			acc
		else
			tailRecursiveFactorial (x - 1) (acc * x)
		tailRecursiveFactorial x 1
```

### The Accumulator Pattern

```F#
let map f list =
	let rec mapTR f list acc = 
		match list with
		| [] -> acc
		| hd :: tl -> mapTR f tl (f hd :: acc)
	mapTR f (List.rev list) []
```

### Continuations

Imagine rather than passing the current state of the accumulator pattern so far as a parameter to the next function call you instead passed a function value representing the rest of the code to execute. This is known as continuation passing. Conceptually this is trading stack space with heap space.

```F#
> // Print a list in revere using a continuation
let printListRev list =
    let rec printListRevTR list cont =
        match list with
        // For an empy list, execute the continuation
        | [] -> cont()
        // For other lists, add printing the current
        // node as part of the continuation.
        | hd :: tl ->
            printListRevTR tl (fun () -> printf "%d " hd
                                         cont() )

    printListRevTR list (fun () -> printfn "Done!");;

val printListRev : int list -> unit

> printListRev [1 .. 10];;
10 9 8 7 6 5 4 3 2 1 Done!
val it : unit = ()
```

## Programming with Functions

