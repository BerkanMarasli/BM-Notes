# Scala



## Printing

println - printing, and forcing a new line on the next print

print - printing, wihtout forcing a new line on next print

```scala
println("Hello World!")
println(10)
// Hello World!
// 10

print("Hello World!")
print(10)
// Hello World!10
```

String interpolation - prefix with s and indicate with $

```scala
val n : Int = 45
s"We have $n apples" // "We have 45 apples"

// Expressions inside interpolated strings
val a = Array(11, 9, 6)
s"I am ${a(0) - a(1)} years old" // "I am 2 years old"

// Raw strings ignore special characters
raw"New line \n. return: \r" // "New line \n. return: \r"

// Characters can be escaped
"They stood outside the \"Rose and Crown\"" // "They stood outside the "Rose and Crown"

// Triple double-quotes let strings span multiple rows and contain quotes

// Type-safe string interpolation using f string interpolation...
```



## Types

| Type    | Description                            |
| ------- | -------------------------------------- |
| Boolean | true or false                          |
| Byte    | 8 bit signed value                     |
| Short   | 16 bit signed value                    |
| Char    | 16 bit unsigned Unicode character      |
| Int     | 32 bit signed value                    |
| Long    | 64 bit signed value                    |
| Float   | 32 bit IEEE 754 single-precision float |
| Double  | 64 bit IEEE 754 double-precision float |
| String  | A sequence of characters               |
| Unit    | Corresponds to no value                |
| Null    | Null or empty reference                |
| Nothing | Subtype of every other type            |
| Any     | Supertype of any type                  |
| AnyRef  | Supertype of any reference type        |



## Operations

| Operation            | Example                                    | Example Result             |
| -------------------- | ------------------------------------------ | -------------------------- |
| ! (not)              | !true<br />!false                          | false<br />true            |
| == (equal to)        | true == true<br />true == false            | true<br />false            |
| > or <               | 10 > 5                                     | true                       |
| +<br />-<br />*      | 1 + 1<br />2 - 1<br />5 * 3                | 2<br />1<br />15           |
| /                    | 6 / 2<br />6 / 4<br />6.0 / 4<br />6 / 4.0 | 3<br />1<br />1.5<br />1.5 |
| && or \|\| (and, or) |                                            |                            |
| += or -=             |                                            |                            |



## Variables

val - immutable value (value cannot change after initialisation)

var - mutable value (value can change after initialisation via assignment)

Note:

- Only classes can have declared but unassigned variables
- Scala has built-in data type recognition

```scala
// val/var variableName : dataType = initialValue

val a : Int = 15 // val a : Int = 15
val b : Int = a + 5 // val b : Int = 20
b = 20 // error: reassignment to val b

var c : Int = 10 // var c : Int = 10
c = 5 // c : Int = 5

val d = 12.3 // d : Double = 12.3 (default data type is Double)
val d = 12.3f // d : Float = 12.3

// Multiple expressions using {}
val e = {val f : Int = 200; val g : Int = 300; f + g} // val e : Int = 500
// Note: Scala does not use return and assumes the last expression is the returned
```

Lazy initialisation - avoid allocating memory to value of varibale. Instead, load in the value when variable is needed.

```scala
lazy val a : Int = 500 // val a : Int = <lazy> or lazy val a : Int
val b : Int = a * 2 // val b : Int = 1000
```



## If else statements

```scala
if (expression) {
  // Do something
} else {
  // Do something else
}

// if expression
val varName = if (expression) doThis else doThis
println(if (expression) doThis else doThis)
```



## While & do-while statements

```scala
while (expression) {
  // Do something
}

// The code inside 'do' will execute at least once even if the expression is false to start with
do {
  // Do something
} while (expression)
```



## For statements

For statement will automatically take the 'varName' as a var variable.

```scala
for (varName <- range) {
  // Do something
}

// Example - prints 1, 2, 3, 4, 5
for (i <- 1 to 5) {
  println(i)
}

// Example 2 - prints 1, 2, 3, 4, 5
for (i <- 1 until 6) {
  println(i)
}

// 1 to 5 == 1.to(5)
// 1 until 6 == 1.until(6)
// <- is called the generator

// Multiple ranges (nested for loop)
for (i <- 1 to 3; j <- 1 to 3) {
  println(i + " " + j)
}
// 1 1, 1 2, 1 3, 2 1, 2 2, 2 3, 3 1, 3 2, 3 3

// Loop through lists
val lst = List(1, 2, 3, 4, 5)
for (i <- lst) {
  println(i)
}
// 1, 2, 3, 4, 5

// Loop through lists with filter
val lst = List(1, 2, 3, 4, 5)
for (i <- lst; if i < 4) {
  println(i)
}
// 1, 2, 3

// For loop as expression
val lst = List(1, 2, 3, 4, 5)
val result = for {i <- lst; if i < 4} yield {
  i * i
}
println(result)
// List(1, 4, 9)
```



## Match expressions

```scala
val age : Int = 18
age match {
  case 20 => println(age)
  case 18 => println(age)
  case 16 => println(age)
  case _ => println("default case")
}

val result = age match {
  case 20 => age
  case 18 => age
  case 16 => age
  case _ => "default case"
}
println(result)

val i : Int = 7
i match {
  case 1 | 3 | 5 | 7 | 9 => println("odd")
  case 2 | 4 | 6 | 8 | 10 => println("even")
  case _ => "Not in specified range"
}
```



## Functions

The last line of the function is the return value/expression.

If the function does not return anything, the returnDataType is Unit.

Function names can be defined as operators (e.g. +, -, **).

```scala
def functionName(argument : argumentDataType, ...) : returnDataType = // Do something (one line)

def functionName(argument : argumentDataType, ...) : returnDataType = {
  // Do something (multi line)
}

// Function that takes only one argument inside object...
object Math {
  def square(x : Int) : Int = x * x
}
println(Math.square(3)) == println(Math square 3)
```

A default value for the argument of a function can be provided.

```scala
def functionName(argument : argumentDataType = defaultValue, ...) : returnDataType = // Do something (one line)

// Example
object Math {
  def square(x : Int = 4) : Int = x * x
}
println(Math.square()) // 16
```

Functions are treated as first class citizens. This means a function can be assigned to a variable using annoymous functions.

```scala
val add = (x : Int, y : Int) => x + y
println(add(3, 5)) // 8
```



## Higher-order functions

Higher-order functions can take functions as arugments and are able to return functions.

```scala
def functionNameA(argument : argumentDataType, ..., functionNameB : (argumentDataType, ...) => returnDataTypeB) : returnDataTypeA = functionNameB(argument, ...)

// Example
def math(x : Double, y : Double, f : (Double, Double) => Double) : Double = f(x, y)
val result = math(5, 2, (x,y) => x + y) // Passing annoymous function

// Example - reuse function
def math(x : Double, y : Double, z : Double, f : (Double, Double) => Double) : Double = f(f(x, y), z)
val result = math(5, 2, 10, (x,y) => x + y) // Passing annoymous function

// The annoymous function can be replaced with _+_
// The _ is known as a wildcard
```



## Partially Applied Functions

Not providing all the arguments of a function.

```scala
val sum = (a : Int, b : Int, c : Int) => a + b + c
sum(10, 20, 30) // 60
val f = sum(10, 20, _ : Int)
sum(30) // 60
val f = sum(10, _ : Int, _ : Int)
sum(20, 30) // 60
```



## Closures

A closure is a function which uses one or more variables declared outside this function.

Inpure closure - the variable outside the function can change (i.e. var).

Pure closure - the variable outside the function cannot change (i.e. val).

```scala
val add = (x : Int) => x + 10
add(20) // 30

// Closure:
val number: Int = 10
val add = (x : Int) => x + number
add(20) // 30
```



## Function currying

Currying is the technique of transforming a function that takes multiple arguments into a function that takes a single argument.

```scala
def add(x : Int, y : Int) = x + y
add(20, 10) // 30

// Function currying
def addFC(x : Int) = (y : Int) => x + y
addFC(20)(10) // 30

// Further use example
def addFC(x : Int) = (y : Int) => x + y
val sum40 = addFC(40)
sum40(60) // 100

// Simple way of function currying
def addFCSimple(x : Int)(y : Int) = x + y
addFCSimple(2)(3) // 5
val sum5 = addFCSimple(5)_
sum5(10) // 5
```



## Strings

Strings are assigned using double quotes "" only.

Scala makes use of Java methods for strings.



## Arrays

Data structure that can store FIXED SIZE sequential elements of SAME DATA TYPE.

Arrays are zero indexed.

Unassigned elements will contain the default value for that data type.

```scala
val arrayName : Array[dataType] = new Array[dataType](arraySize)
val arrayName = new Array[dataType](arraySize)
val arrayName = Array(1, 2, 3, 4, 5)

for (i <- arrayName) {
  println(i)
}

for (i <- 0 to (arrayName.length - 1)) {
  println(arrayName(i))
}
```



## Lists

Arrays are mutable whereas lists are IMMUTABLE (cannot change element value of lists once assigned).

Arrays are flat whereas lists represent LINKED LISTS.

Lists are zero indexed.

```scala
val listName : List[dataType] = List(1, 2, 3, 4, 5)
println(listName) // List(1, 2, 3, 4, 5)

// :: prepends a single item (makes a new List so needs to be assigned to a new variable)
1 :: List(2, 3) returns List(1, 2, 3)
// ::: prepends a complete list (makes a new List so needs to be assigned to a new variable)
List(1, 2) ::: List(3, 4) returns List(1, 2, 3 ,4)

// Nil
1 :: 5 :: 9 :: Nil returns List(1, 5, 9)

.head - first value of List
.tail - last value of List (whatever remains after removing first value from List)
.isEmpty - boolean value of whether List is empty
.reverse - reverses the List
.max - max value in List
List.fill(numberOfElements)(whatElement) // List.fill(5)(2) -> List(2, 2, 2, 2, 2)

// Iterate over List using foreach
listName.foreach(operation)
listName.foreach(println)
var sum : Int = 0;
listName.foreach(sum += _)

// Iterate over List using for loop
for (i <- listName) {
  // Do something
}
```



## Sets

Collection of different elements (no duplicates) of same data type. All element values must be unique.

Two types of sets: mutable and immutable. All sets are immutable by default.

```scala
val setName : Set[dataType] = Set(1, 2, 3, 4, 4, 4) // Set(1, 2, 3, 4)

// To declare mutable Set
var setName : scala.collection.mutable.Set[dataType] = scala.collection.mutable.Set(1, 2, 3, 4)

// Add element to mutable Set (makes a new Set so needs to be assigned to a new variable)
// Sets in scala are not ordered!
setName + elementToAdd
println(setName + 5) // Set(1, 5, 2, 3, 4)

// Check whether element is present in Set
setName(elementToCheck)
println(setName(3)) // true

.head - first value of Set
.tail - last value of Set (remaining Set once first value is removed)
.isEmpty - checks if Set is empty (returns boolean value)
setOne ++ setTwo == setOne.++(setTwo) // concatinate Sets
setOne & setTwo == setOne.&(setTwo) == setOne.intersect(setTwo) // common values between Sets (intersection of Sets)
.min - minimum value in Set
.max - maximum value in Set

// Iterating Set with foreach
setName.foreach(operation)

// Iterating Set with for loop
for (i <- setName) {
  // Do something
}
```



## Maps

Collection of key value pairs. Keys are unique.

Two types of maps: mutable and immutable. All maps are immutable by default.

```scala
val mapName : Map[keyDataType, valueDataType] = Map(key -> value, ...)

// Access specific value
mapName(key)

.keys - all keys of the Map (returns a Set)
.values - all values of the Map
.isEmpty - checks if Map is empty (returns a boolean)
.contains(key) - checks if key is present in Map (returns a boolean)
.size - returns number of key value pairs in Map

// Concatinate two Maps
mapOne ++ mapTwo

// Iterate using foreach
mapName.keys.foreach { key => println("key " + key); println("value " + mapName(key))}
```



## Tuples

A class that can contain different data types of elements.

They are of FIXED SIZE. You cannot change the values of tuples once declared.

Tuples can contain up to 22 elements.

```scala
val tupleName = (values) // val myTuple = (1, 2, "hello", true)
val tupleName = new TupleNUMBEROFELEMENTS(values) // val myTuple = new Tuple3(1, 2, true)

// Access specific element
._POSITIONOFELEMENT // myTuple._3 returns "hello"

// Iterate over tuple using foreach
tupleName.productIterator.foreach{i => println(i)}

// Tuple within tuple
myTuple = new Tuple3(1, "hello", (2, 3))
myTuple._3._2 // returns 3
```



## Options type

Container that can give you two values (instance of Some or None).

```scala
// Example code
val lst = List(1, 2, 3)
val map = Map(1 -> "Tom", 2 -> "Max", 3 -> "John")
println(lst.find(_ > 6)) // returns None --- .find has a return type Option[Int]
println(lst.find(_ > 2)) // returns Some(3)
println(map.get(1)) // returns Some("Tom") --- .get has a return type Option[String]
println(map.get(4)) // returns None

// Extract value from Some
println(lst.find(_ > 2).get) // returns 3
println(map.get(1).get) // returns "Tom"
// ^ upgrade from above to avoid exception
println(lst.find(_ > 3).getOrElse("No element found")) // "No element found"

// Define an option
val opt : Option[Int] = Some(5) or None (empty)
print(opt.isEmpty) // true for None, false for Some
```



## Higher-Order Methods

Doesnt modify original collection so should be stored in a new variable.

map - iterate over a collection (List, Array, etc) and then apply a function to each element.

filter - iterate over a collection and filter the collection based on a condition.

```scala
lst.map(operation)

val lst = List(1, 2, 3)
val map = Map(1 -> "Tom", 2 -> "Max", 3 -> "John")
println(lst.map(_ * 2)) == println(lst.map(x => x * 2)) // returns List(2, 4, 6)
println(map.map(x => x)) // returns List((1,Tom), (2,Max), (3,John))
println(map.mapValues(x => "y" + x)) // returns Map(1 -> "yTom", 2 -> "yMax", 3 -> "yJohn")

println("hello".map(_.toUpper)) // returns "HELLO"

// flatten
println(List(List(1,2,3), List(3,4,5))) // returns List(List(1,2,3), List(3,4,5))
println(List(List(1,2,3), List(3,4,5)).flatten) // returns List(1,2,3,3,4,5)

// flatMap
println(lst.flatMap(x => List(x, x+1))) // returns List(1,2,2,3,3,4)

// filter
println(lst.filter(x => x % 2 == 0)) // returns List(2)
```

reduce (Left/Right) - reduces all elements to return one value using an accumulator, currentVal approach.

fold (Left/Right) - specify start value and then apply reduce

scan (Left/Right) - same as fold... but gives map of intermediate result (list of the results along the way).

```scala
val lst1 = List(1, 2, 3, 5, 7, 10, 13)
val lst2 = List("A", "B", "C")

// reduce
println(lst1.reduceLeft(_ + _)) == println(lst1.reduceLeft((x,y) => x + y)) // returns 41
println(lst2.reduceLeft(_ + _)) // returns ABC

// reduce Left vs Right - indicates whether first value is on the left or right of the collection
println(lst1.reduceLeft(_ - _)) // returns -39
println(lst1.reduceRight(_ - _)) // returns 7
```

```scala
val lst1 = List(1, 2, 3, 5, 7, 10, 13)
val lst2 = List("A", "B", "C")

// fold - can pass initial argument (performs same as reduce)
.foldLeft(initialValue)(operation))
println(lst1.foldLeft(100)(_ + _)) // returns 141 (100 + 41 from reduce above)
println(lst1.foldLeft("z")(_ + _)) // returns zABC
```

```scala
val lst1 = List(1, 2, 3, 5, 7, 10, 13)
val lst2 = List("A", "B", "C")

// scan
.scanLeft(initialValue)(operation))
println(lst1.foldLeft(100)(_ + _)) // returns List(100, 101, 103, 106, 111, 118, 128, 141)
println(lst1.foldLeft("z")(_ + _)) // returns List(z, zA, zAB, zABC)
```









Objects are classes which are already instantiated (single-ton). This means you cannot use the new keyword to create a new instant.





















