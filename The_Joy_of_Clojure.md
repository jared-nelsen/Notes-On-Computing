# The Joy of Clojure

Read book through what I already knew in Parts 1 and 3. It got interesting in Part 3

# Part 3: Functional Programming

### Chapter 6: Laziness

Laziness is the concept of computing things as needed. It is better to describe complete computations and use these descriptions instead of computing the whole result first.

The  ***Lazy-seq recipe*** is a pattern that you can use to implement laziness to your own code:

1. Use the lazy-seq macro at the outermost level of your lazy sequence producing expression
2. If you happen to be consuming another sequence during your operations then use `rest ` instead of `next`
3. Prefer higher order functions when processing sequences
4. Dont hold on to the head

One of the powers of laziness is that you can query infinite sequences.

#### Key functions

- ***map*** - Map function applies a function to each element in a sequence and returns the resulting sequence
- ***reduce*** - The reduce function applies a function to each value in the sequence and the running result to accumulate a final value
- ***filter*** - The filter function applies a function to each element in a sequence and returns a new sequence of those elements where said function returned a truthy value

#### Force and delay

You can use force and delay within a function as a control mechanism for how laziness should happen.

#### if-let 

***if-let*** and ***when-let*** is a succinct way to avoid nesting if when and let:

```clojure
(if-let [res :truthy-thing] (print res))
```

## Chapter 7: Functional Programming

All Clojure functions are first class.

Clojure collections act as functions and can also act as data: these properties describe ***first-class functions***.

First class properties

1. Can be created on demand
2. Can be stored in a data structure
3. Can be passed as an argument to a function
4. Can be returned as a value of a function

You can build chains of functions using `comp`. Seems silly at first though.

#### Partial functions

You can build a function from partial applications of another.

```clojure
((partial + 5) 100 200)
=> 305
Partial applies + to 5 at first and then when 100 and 200 are passed as args it evaluates them too.
```

You can use the `complement` function to invert truthiness.

A ***higher order function*** is a function that does at least one of the following:

1. Takes one or more functions as arguments
2. Returns a function as a result

You should strive to build programs from existing parts.

`sort-by` is a powerful function for sorting maps by functions and keys.

***Prefer higher order functions when processing sequences!!!*** This encourages laziness. Most collection processing can be performed by the following: map, reduce, filter, for, some, repeatedly, sort-by, keep, take-while, and drop-while.

#### Pure functions

