---
theme: seriph
background: https://source.unsplash.com/collection/94734566/1920x1080
class: text-center
highlighter: shiki
lineNumbers: false
info: |
  ## Slidev Starter Template

  Learn more at [Sli.dev](https://sli.dev)
drawings:
  persist: false
transition: slide-left
title: Welcome to Slidev
---

# Welcome to F# (F Sharp)


<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Press Space for next page <carbon:arrow-right class="inline"/>
  </span>
</div>

<div class="abs-br m-6 flex gap-2">
  <button @click="$slidev.nav.openInEditor()" title="Open in Editor" class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon:edit />
  </button>
  <a href="https://github.com/slidevjs/slidev" target="_blank" alt="GitHub"
    class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->
---

# Paradigm

F# is a multi-paradigm programming language that supports both functional and object-oriented programming techniques. 

Functional programming emphasizes on expressions and declarations rather than execution of statements. It allows developers to write programs that don’t have side effects.

Object-Oriented Programming (OOP) models the program as an interaction between the objects. In F#, objects are instances of classes used to interact with one another to design applications and computer programs.

---

# Syntax

Let's dive into some basic syntax elements of F#

---

# Let

- `let` is used to bind names to values. It works with type inference, but you can also make the type explicit.

```fsharp
let x = 5  // Type is inferred as int

let y : string = "Hello"  // Type is explicitly specified as string
```

- let also introduces mutable bindings.

```fsharp
let mutable z = 42 // Mutable binding
z <- 50  // Mutation
```

- we can shadow a let declaration to change value

```fsharp
let x = 1
...
let x = 5
```

---

# Function

- By default, F# is whitespace sensitive. For functions, this means that the last line of a function is its return value, and the body of a function is denoted by indentation.

```fsharp
let add x y = 
    x + y  // Function body. Last line is return value.

// nesting functions
let quadruple x =    
    let double x =
        x * 2

    double(double(x))

let result = quadruple 4
```

- Function's type can be explicitly declared.

```fsharp
let removeSpace (text: string) =
    text.Replace(" ", "")
```

- The fun keyword allows you to create a function inline without giving it a name. These functions are known as anonymous functions, lambdas, or lambda functions

```fsharp
// Lambda functions
let square = (fun x -> x * x)

// F# supports higher-order functions and function composition
// F# provides automatic currying, partial application
let addThree = add 3
let result = addThree 5 // Result is 8

// Non-curried function, simply use tuple as param
let multiply (x, y) = x * y
```

---

# Order of Evaluation

- Sometimes you need parenthesis to group things

```fsharp
let result = add (add 5 8) (add 1 1)
// Backward Pipe Operator
let result = (2, 3) |> add
```

- You can also use backward pipe operator to group things

```fsharp
let add x y =
    x + y
let double x =
    x * 2

let result = double <| add 5 8 // 26
```

---

# Unit

- The unit type in F# is a type that indicates no specific value or it can be thought of as a signal that some operation has been performed.

```fsharp
// parameterless function takes unit as their argument
let printHello =
    printfn "Hello, world!" 
```

- The function `printHello` gets the type `unit -> unit` because it takes no meaningful input (you call it with `()`) and returns no meaningful output.

---

# Tuple

- Tuples in F# are similar to arrays or lists, but you can store different types of data in them.

```fsharp
let myTuple : (int * string) = (1, "hello")  // Explicit type declaration
let myTuple2 = (2, "world")  // Type is inferred

let items = ("apple", "dog")
let fruit = fst items
let animal = snd items
```

- You can also extract the tuple using pattern matching

```fsharp
let items = ("apple", "dog", "Mustang")
let fruit, animal, car = items
// let fruit, _, car = items
```

- We can use tuple to "return multiple values" on F# as in golang 

---

# String

- You can concatenate and format strings in F#:

```fsharp
let name = "John"
let greeting = "Hello " + name  // String concatenation

let formattedGreeting = sprintf "Hello %s" name  // Format string value

// %A will format anything
```

---

# Branching

- In languages like C#, if statements do not yield results; they can only cause side effects. If statements in F# return values due to F#'s functional programming roots.

```fsharp
let max a b =
    if a > b then a
    else b
```

- Branching with pattern match

```fsharp
let sign x = 
    match x with
    | 0 -> "zero"
    | x when x < 0 -> "negative"
    | _ -> "positive"

// we don't want this
let getDinnerUgly x = 
  let name, foodChoice = x
  
  if foodChoice = "veggies" || foodChoice ="fish" || 
      foodChoice = "chicken" then
      sprintf "%s doesn't want red meat" name
  else
      sprintf "%s wants 'em some %s" name foodChoice

// nice for complex conditional
let getDinner x =
    match x with
    | (name, "veggies")
    | (name, "fish")
    | (name, "chicken") -> sprintf "%s doesn't want red meat" name
    | (name, foodChoice) -> sprintf "%s wants 'em some %s" name foodChoice 
    
let person1 = ("Bob", "fish")
let person2 = ("Sally", "Burger")

getDinner person1 //"Bob doesn't want red meat" 
getDinner person2 //"Sally wants 'em some Burger" 
```

---

# Lists

- F# provides the list type to help you store and manipulate a series of values.

```fsharp
let evens = [0; 2; 4; 6; 8; 10]  // Building list with range
let odds = [for x in 1 .. 10 do if x % 2 <> 0 then yield x]  // List comprehension with condition
let squares = List.map (fun x -> x * x) evens  // Using map function
let divisibleBySix = List.filter (fun x -> x % 6 = 0) evens  // Using filter function

// range operation
let list = [0..4] // [0; 1; 2; 3; 4] inclusive

// Building new lists from existing ones
let first = ["grape"; "peach"]
let second = "pear" :: first
let third = "apple" :: second

// concatenating list
let first = ["apple"; "pear"; "grape"]
let second = first @ ["peach"]
```

---

# Pipelining

- The pipeline operator |> is used to chain function calls:

```fsharp
let output = [1 .. 10] 
             |> List.map (fun x -> x * x)
             |> List.reduce (+)
```

---

# Arrays

- Arrays allow you to store multiple values that are contiguous in memory.

```fsharp
let primes: int [] = [|2; 3; 5; 7; 11|]  // array creation

let squares = Array.map (fun x -> x * x) primes // using map function on array
```

---

# Looping

- Looping over a List and Arrays

```fsharp
for prime in primes do 
    printfn "%d" prime 

let list = [1..10]
for i in list do 
    printfn "%d" i 
```

- Looping with expression

```fsharp
let mutable sum = 0
for i = 1 to 5 do
    sum <- sum + i
```

- Looping with while:

```fsharp
let mutable i = 1
while i < 10 do 
    printfn "%d" i 
    i <- i + 1  // increment the counter
```

---

# .Net Collections

- F# has top-level access to all .NET libraries, including collections.

```fsharp
let netList = new System.Collections.Generic.List<int>() // .Net List
let netDict = new System.Collections.Generic.Dictionary<string,int>() // .Net Dictionary
```

- Allows functions like Seq.map, Seq.skip, Seq.toArray, Seq.max, Seq.maxBy:

```fsharp
let seq1 = seq {1 .. 10} 
let seq2 = seq1 |> Seq.map (fun x -> x * x) 
let threeToFive = seq1 |> Seq.skip 2 |> Seq.take 3 //skip first 2, take next 3
```

---

# RecordType

- F# provides record types, which are essentially named field aggregates

```fsharp
type Person = { FirstName: string; LastName: string }

let john = { FirstName = "John"; LastName = "Doe" } // like class
let jane = { john with FirstName = "Jane" }  // Creating record from an existing one
```


- Pattern matching with record types is also possible

```fsharp
let greetPerson person = 
    match person with
    | { FirstName = fname; LastName = lname } -> printfn "Hello %s %s" fname lname
```

---

# Option Types

- Option type encapsulates a value or the absence of a value

```fsharp
let validValue = Some 42  // Option with value
let noValue = None  // Option with no value
```

- Pattern matching with Option type:

```fsharp
let printOptionValue (opt : int option) = 
    match opt with
    | Some x -> printfn "%i" x
    | None -> printfn "No value"

// Projecting values from option types
let chronoTrigger = { Name = "Chrono Trigger"; Platform = "SNES"; Score = Some 5 }
let halo = { Name = "Halo"; Platform = "Xbox"; Score = None }
let decideOn game =
    game.Score
    |> Option.map (fun score -> if score > 3 then "play it" else "don't play")

decideOn chronoTrigger // (Some "play it")
decideOn halo // None
```

---

# DiscriminatedUnions

- Discriminated unions in F# are used to implement data types that can have multiple forms but are still strongly typed.
- Think of them like a much more powerful version of enums

```fsharp
type Shape = 
    | Rectangle of width : float * length : float
    | Circle of radius : float
```

- Pattern matching can be used on Discriminated Unions:

```fsharp
let getArea shape = 
    match shape with
    | Rectangle (w, l) -> w * l
    | Circle r -> Math.PI * r * r
```

---

# Modules

- F# uses modules to organize code in a logical manner.

```fsharp
module MyModule =
    let sayHello name = printfn "Hello, %s!" name
```

---

# Filtering

- The `filter` function is a powerful tool in F# used to filter collections based on a predicate.

```fsharp
let numbers = [1..10] // list of integers

// Filtering even numbers
let evens = List.filter (fun x -> x % 2 = 0) numbers
```

- Apart from lists, you can use filter with arrays, sequences and other collections.

```fsharp
let seq_nums = seq {1..20}

// Filtering numbers that are divisible by 3 in a sequence
let divisible_by_3 = Seq.filter (fun x -> x % 3 = 0) seq_nums
```

- F# also provides other useful functions like choose which filters and maps collections in one go.

```fsharp
let mixed = [Some 1; None; Some 2; None; None; Some 3]

// Filtering out the 'None' values and extracting values from 'Some'
let some_values = List.choose id mixed
```

---

# Resource

[https://fsharp.org/learn/](https://fsharp.org/learn/)