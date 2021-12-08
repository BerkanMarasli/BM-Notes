# Scala

INSERT PROGRAMMINGKNOWLEDGE SCALA TUTORIAL VIDEO LINK USED TO CREATE THIS SECTION.



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



## Classes

object keyword used to create a single-ton class. No new instance of this class can be created.

class keyword used to create class that can have multiple instances.

```scala
// Basic definition
class className
var newInstance = new className

// Primary constructor in class
class className(define constructur variables)
class User(var name: String, var age: Int)
var newUser = new User("Max", 28)
newUser.name // returns Max
newUser.age // returns 28
newUser.name = "Tom"
newUser.age = 22

// private and public - private members can only be accessed inside the class
class User(private var name: String, var age: Int) {
  def printName { println(name) }
}
newUser.name // exception
newUser.printName // valid as accessing member through class method

// var getter setter
// val getter ------
// no val/var - no getter setter
```

### Auxiliary constructors

Alternative constructor for a class. They must have different signatures and must call the previously defined constructor.

Defined as a method with the name 'this'

```scala
class User(var name: String, var age: Int) {
  def this() {
    this("Tom", 32) // You must call the previously defined constructor
  }
  def this(name : String) {
    this(name, 32)
  }
}

var user1 = new User("Max", 28)
var user2 = new User()
var user3 = new User("Max")
```

### Inheritance - extending a class

Classes in Scala can be extended, creating new classes which retain characteristics of the base class. Involves a superclass and subclass. The subclass inherits the members of the superclass, on top of which it can add its own members.

```scala
// use extends keyword

// Polygon.scala
class Polygon {
  def area: Double = 0.0
}

object Polygon {
  def main(args: Array[String]) {
    var poly = new Polygon
    var tri = new Triangle
    var rect = new Rectangle(55.2, 20.0)
    printArea(poly) // 0.0
    printArea(tri) // 0.0
    printArea(rect) // 1104.0
  }
  def printArea(p: Polygon) {
    println(p.area)
  }
}

// Triangle.scala
class Triangle extends Polygon {
  
}

// Rectangle.scala
class Rectangle(var width: Double, var height: Double) extends Polygon {
  override def area: Double = width * height
}
```

### Abstract classes

Used when:

- You want to restrict the instantiation of the superclass.
- To ensure neccessary override methods are implemented in every case.

If as class is declared abstract, it cannot be instantiated. An abstract class does a few things for the inheriting subclass:

- Define methods which can be used by the inheriting subclass
- Define abstract methods which the inheriting subclass must implement
- provide a common interface which allows the subclass to be interchanged with all other subclasses

```scala
// Add keyword 'abstract' in front of class declaration
abstract class Polygon {
  def area: Double // No body provided means it is an abstract method - the area method must be implemented inside all subclasses
}
new Polygon // exception
```

### Methods

```scala
// .copy
case class Rectangle(val width : Int, val height : Int) {
}
val smallRectangle = Rectangle(width = 3, height = 4)
val largeRectangle = smallRectangle.copy(width = smallRectangle.width * 2)
```



### Traits

Scala doesnt allow multiple inhertience (extends) from more than one class.

Interface - describes a set of methods and properties that an implementing class must have.

Traits - partially implemented interfaces. Do not introduce constructors.

```scala
trait traitName {
}
// Can contain abstract and non-abstract methods
// But at least one method should be an abstract method

trait Shape {
  def colour: String
}
abstract class Polygon {
  def area: Double
}
class Rectangle extends Polygon with Shape {
  override def area: Double = 2929
  def colour: String = "Blue" // Note: you dont need override keyword when overriding trait method
}
// Multiple with's to inherit from multiple traits
```



### Abstract Classes vs Traits

Useful YouTube video explaining the concept: https://www.youtube.com/watch?v=_7ULjOILxhI



























## Testing (MUnit)

Test files go under the `src/test/scala` directory.

```scala
// Add following settings to build.sbt file:
libraryDependencies += "org.scalameta" %% "munit" % "0.7.29" % Test
```

### Unit Testing

Test with parameters for which we know what the expected result should be.

- A test suite is a class that extends `munit.FunSuite`.
- Use `test` and `assetEquals` methods, from `munit.FunSuite` to write test specifications.

```scala
class TestFile extends munit.FunSuite {
  test("testName") {
    val obtained = add(1,1)
    val expected = 2
    assertEquals(obtained, expected)
  }
  test("test2Name") {
    // Test specification
  }
}
```

### Property-Based Testing

Specify general properties that must be correct for all possible inputs.

ScalaCheck is a library for doing property-based testing and integrates with MUnit.

```scala
// Add following settings to build.sbt file:
libraryDependencies += "org.scalacheck" %% "scalacheck" % "1.14.1" % "test"
```

- A test suite containing properites is a class that extends `munit.ScalaCheckSuite`.
- Use `property` method to define a property.

```scala
import org.scalacheck.Prop.forAll

class ProgramProperties extends munit.ScalaCheckSuite {
  // Will fail for test case n = 1
  property("fibonacci(n) == fibonacci(n-1) + fibonacci(n-2)") {
    forAll { (n: Int) =>
      fibonacci(n) == fibonacci(n-1) + fibonacci(n-2)
    }
  }
  // Specify range of values n >= 3
  val fibonacciDomain: Gen[Int] = Gen.choose(3, Int.MaxValue)
  property("fibonacci(n) == fibonacci(n-1) + fibonacci(n-2)") {
    forAll(fibonacciDomain) { (n: Int) =>
      fibonacci(n) == fibonacci(n-1) + fibonacci(n-2)
    }
  }
  // Or exclude within forAll
  val fibonacciDomain: Gen[Int] = Gen.choose(1, Int.MaxValue)
  property("fibonacci(n) == fibonacci(n-1) + fibonacci(n-2)") {
    forAll(fibonacciDomain.suchThat(n => n >= 3)) { (n: Int) =>
      fibonacci(n) == fibonacci(n-1) + fibonacci(n-2)
    }
  }
  
  // ==============================================================
  
  // Find edge cases: fibonacci(48) = -1323752223 because of type Int
  property("fibonacci numbers are positive") {
    forAll(fibonacciDomain) { (n: Int) =>
      fibonacci(n) >= 0
    }
  }
  // Correct:
  val fibonacciDomain: Gen[Int] = Gen.choose(1, 47)
  property("fibonacci(n) == fibonacci(n-1) + fibonacci(n-2)") {
    forAll(fibonacciDomain.suchThat(n => n >= 3)) { (n: Int) =>
      fibonacci(n) == fibonacci(n-1) + fibonacci(n-2)
    }
  }
}

/*
The forAll method takes as parameter a function that receives an arbitrary value (here n, of type Int) and reutrns a Boolean value.
*/
```

### Mocking

Mock the components that the test unit is dependent on; provide a fake implementation of a component that can be subtituted to the componnent.

ScalaMock is a mocking library that can be used.

### Integration Testing

Process of testing a complete system, with no stubs or mocks. You need to programmatically set up the entire stack.

MUnit lets you set up and shut down a resource for the lifetime of a single test.

```scala
// Http server example
class HttpServerSuite extends munit.FunSuite {
  val withHttpServer = FunFixture[HttpServer](
  	setup = test => {
      val httpServer = httpServer()
      httpServer.start(8888)
      httpServer
    },
    teardown = httpServer => httpServer.stop()
  )
  withHttpServer.test("server is running") { httpServer =>
  	// Perform HTTP request here
  }
}

/*
Sometimes, it's considerably more performant to set up the stack once at the beginning, then run all the tests, and eventually shut down the system. We acheive that by overriding the methods beforeAll and afterAll of the FunSuite class.
*/
class HttpServerSuite extends munit.FunSuite {
  val httpServer = HttpServer()
  override def beforeAll(): Unit = httpServer.start(8888)
  override def afterAll(): Unit = httpServer.stop()
  
  test("server is running") {
    // Perform HTTP request here
  }
}
```











To add to notes:

- ??? allows the code to compile without inserting actual code. useful when constructing a method...
- When u think for loop is JS, think map in Scala
- In Scala, think of recursion and not in terms of loops















# Essential Scala - Noel Welsh and Dave Gumell

Link to book: https://books.underscore.io/essential-scala/essential-scala.html

<br>

## Expressions, Types and Values

#### Theory

A Scala program consists of two stages: compile-time and run-time.
Compilation is a process of checking that a program makes sense; both syntactially correct and type correct.
Even though a program successfully compiles it can still fail at run time; dividing an integer by zero causes a run-time error in Scala. The type of integers, `Int`, allows division so the program type checks successfully. However, at run-time the program fails because there is no `Int` that can represent the result of division.

The defining characteristic of an expression is that it evaluates to a value.
A value is information stored in the computer's memory and exists at run-time.
In Scala, all values are objects.

Types are restrictions on our programs that limit how we can manipulate objects, forcing us to write programs that give a consistent interpretation to values.
Expressions have types but values do not. We cannot inspect an arbitrary piece of the computer's memory and divine how to interpret it without knowing the program that created it.
Types exist at compile-time.
This means type information does not need to be stored with value in memory; a process called type erasure.

Summary:

- Expressions are the parts of a program that evaluate to a value. They are the major part of a Scala program.
- Expressions have types, which express some restrictions on programs. During compile-time the types of our programs are checked. If they are inconsistent then compilation fails and we cannot evaluate, or run, our program.
- Values exist in the computer's memory, and are what a running program manipulates. All values in Scala are objects.

<br>

#### Scala's Type Hierarchy

![](/Users/berkanmarasli/Downloads/Scala's Type Hierarchy.svg)

Scala has a grand supertype called `Any`, under which there are two types, `AnyVal` and `AnyRef`. `AnyVal` is the supertype of all value types, which `AnyRef` is the supertype of all “reference types” or classes. All Scala and Java classes are subtypes of `AnyRef`. There are two special types at the *bottom* of the hierarchy. `Nothing` is the type of `throw` expressions, and `Null` is the type of the value `null`. These special types are subtypes of everything else, which helps us assign types to `throw` and `null` while keeping other types in our code sane.

<br>

### Objects

An object is a grouping of data and operations on that data.
The operations are known as methods. Some methods accept paramters or arguments.
The data is stored in fields.

```scala
// Infix Operator Notation

/*
Any Scala expression written a.b(c) with exactly 1 argument can also be written a b c
Note that a b c d e is equivalent to a.b(c).d(e), not a.b(c, d, e)
*/

"the quick brown fox".split(" ") == "the quick brown fox" split " "
```

Summary:

- All Scala values are objects. We interact with objects by calling methods on them.
- The syntax for a method call is `anExpression.methodName(parameter, ...)` or `anExpression methodName parameter`.
- Scala has very few operators - almost everything is a method call. We use syntactic conventions like infix operator notation to keep our code simple and readable, but we can always fall back to standard method notation.
- Scala's focus on programming with expressions allows us to write much shorter code. It also allows us to reason about code in a very intuitive way using values and types.

<br>

### Literal Object Data Types

#### Numbers

| Data Type | Representation        |
| --------- | --------------------- |
| Int       | 32-bit integers       |
| Long      | 64-bit integers       |
| Float     | 32-bit floating point |
| Double    | 64-bit floating point |

Scala also has 16-bit `Short` integers and 8-bit `Bytes`s, but there is no literal syntax for creating them. Instead, create them using methods called `toShort` and `toByte`.

#### Boolean

`true` or `false`

#### Characters

`Chars` are 16-bit Unicode values written as a single character enclosed in single quotes.

```scala
'a' // res0: Char = a
```

#### Strings

```scala
"this is a string" // res0: String = this is a string
```

#### Null

Not used in Scala. Scala has the means to define opertional values using `Option`.

```scala
null // res0: Null = null
```

#### Unit

Unit is the result of expressions that evaluate to no interesting value.
Used as a placeholder for expressions that don't yield a useful value.

```scala
() // Unit, written ()
:type () // Unit
:type println("something") // Unit
```

<br>

### Object Literals

When we write an object literal we use a declaration, which is a different kind of program to an expression.
A declaration does not evaluate to a value. Instead it associates a name with a value.
This name can then be used to refer to the value in other code.

```scala
// Declare empty object

object Test {}

/*
This is not an expression - it does not evaluate to a value. Rather it binds a name (Test) to a value (an empty object)
Once we have bound the name Test we can use it in expressions, where it evaluates to the object we have declared
*/

Test // res0: Test.type = Test$@779732fb

/*
This expression is equivalent to writing a literal like 123 or "abc"
Note that the type of the object is reported as Test.type
This is not like any type we’ve seen before—it’s a new type, created just for our object, called a singleton type
We cannot create other values of this type
*/
```

```scala
// Method Declaration Syntax

def methodName(paramterName: type, ...): resultType = bodyExpression
```

```scala
// Field Declaration Syntax

val fieldName: type = valueExpression
var fieldName: type = valueExpression
```

The body expression of a field is run only once after which the final value is stored in the object and the expression is never evaluated again.
The body of a method, on the other hand, is evaluated every time we call the method.

<br>

### Compound Expressions

#### Conditionals

Scala's conditional is an expression - it has a type and returns a value.

```scala
// if statement

if (1 < 2) "Yes" else "No" // res0: String = Yes
```

#### Blocks

Blocks are expressions that allow us to sequence computations together.
They are written as a pair of braces containing sub-expressions separated by semicolons or newlines.
A block is also an expression; it executes each of its sub-expressions in order and returns the type and value of the last expression.

```scala
// Block Expression Syntax

{
  declarationOrExpression ...
  expression
}
```

<br>

## Objects and Classes

A class is a template for creating objects that have similar methods and fields.
In Scala a class also defines a type, and objects created from a class all share the same type.
Like an object declaration, a class declaration binds a name and is not an expression. However, unlike an object name, we cannot use a class name in an expression.

```scala
// Example of use

class Person {
  val firstName = "Berkan"
  val lastName = "Welsh"
  def name = firstName + " " + lastName
}

Person // error
val berkan = new Person // berkan: Person = Person@3a1d255f
berkan.firstName // res0: String = Berkan

// Can write a method that takes any Person
object alien {
  def greet(p: Person) = "Greetings, " + p.firstName + " " + p.lastName
}
```

<br>

#### Constructors

A constructor allows us to pass parameters to new objects as we create them.
The constructor parameters can only be used within the body of the class. In order to access them outside the object, we must declare a field or method using `val` or `def`. Scala provides a shorthand to overcome this where you can define a field inside the constructor.

```scala
// Class Declaration Syntax

class className(val parameter: type, ...) {
  declarationOrExpression ...
}

// Default and keyword parameters
def greet(title: String = "Citizen", firstName: String = "Some") = "Greetings, " + title + " " + firstName
greet("Busy") // incorrect -> res0: String = Greetings, Busy Some
greet(firstName = "Busy") // correct -> res1: String = Greetings, Citizen Busy
```

<br>

### Objects as Functions

An object representing a computation - called functions, and are the basis of functional programming.

#### Apply Method - Function Application Syntax

In Scala, by convention, an object can be “called” like a function if it has a method called `apply`. Naming a method `apply` affords us a special shortened call syntax: `foo.apply(args)` becomes `foo(args)`.

```scala
// Example of use

class Adder(amount: Int) {
  def apply(in: Int): Int = in + amount
}

val add3 = new Adder(3) // add3: Adder = Adder@4185f338
add3.apply(2) // res0: Int = 5
add3(4) // shorthand for add3.apply(4) -> res1: Int = 7
```

With this one simple trick, objects can “look” syntactically like functions.
There are lots of things that we can do with objects that we can’t do with methods, including assign them to variables and pass them around as arguments.

<br>

### Companion Objects (Multiple Constructors)

Sometimes we want to create a method that logically belongs to a class but is independent of any particular object.
One common use case is auxiliary constructors. Although Scala does have syntax that lets us define multiple constructors for a class, Scala programmers almost always prefer to implement additional constructors as `apply` methods on an object with the same name as the class. We refer to the object as the *companion object* of the class.

```scala
// Example of use

class Timestamp(val seconds: Long)

object Timestamp {
  def apply(hours: Int, minutes: Int, seconds: Int): Timestamp = new Timestamp(hours*60*60 + minutes*60 + seconds)
}

Timestamp(1, 1, 1).seconds // res0: Long = 3661

// It is important to note that the companion object is not an instance of the class—it is a singleton object with its own type
Timestamp // res1: Timestamp.type = Timestamp$@137bf92e
```

```scala
// Companion Object Syntax

class className {}
object className {}
```

Summary:

- Companion objects provide us with a means to associate functionality with a class without associating it with any instance of that class.
- They are commonly used to provide additional constructors.
- A companion object has the same name as its associated class. This doesn’t cause a naming conflict because Scala has two namespaces: the namespace of values and the namespace of types.
- A companion object must be defined in the same file as the associated class.

<br>

### Case Classes

Case classes are an exceptionally useful shorthand for defining a class, a companion object, and a lot of sensible defaults in one go.
They are ideal for creating lightweight data-holding classes with the minimum of hassle.
They are created by prepending a class definiton with the keyword `case`.
whenever we declare a case class, Scala automatically generates a class and companion object.

#### Features of a case class

1. A field for each constructor argument - we don't need to write `val` in our constructor defintion (no harm in doing so).
2. A default toString method that prints a sensible constructor-like representation of the class (no more @ signs and hex numbers).
3. Sensible `equals`, and `hashCode` methods that operate on the field values in the object.

This makes it easy to use case classes with collections like `Lists`,  `Sets`, and `Maps`.
It also means we can compare objects on the basis of their contents rather than their reference identity.

```scala
new Person("Berkan", "Marasli").equals(new Person("Berkan", "Marasli")) // res0: Boolean = true
new Person("Berkan", "Marasli") == new Person("Berkan", "Marasli") // res1: Boolean = true
```

4. A `copy` method that creates a new object with the same field values as the current one.

```scala
berkan.copy() // res0: Person = Person(Berkan, Marasli)

// Creates and returns a new object of the class rather than returning the current one

/*
The copy method actually accepts optional parameters matching each of the constructor parameters. If a parameter is specified the new object uses that value instead of the existing value from the current object. This is ideal for use with keyword parameters to let us copy an object while changing the values of one or more fields.
*/
berkan.copy(firstName = "Berkan2") // res0: Person = Person(Berkan2, Marasli)
berkan.copy(lastName = "Marasli2") // res0: Person = Person(Berkan, Marasli2)
```

5. Case classes implement two traits: `java.io.Serializable` and `scala.Product`. Neither are used directly. The latter provides methods for inspecting the number of fields and the name of the case class.

<br>

#### Features of a case class companion object

The companion object contains an `apply` method with the same arguments as the class constructor. Scala programmers tend to prefer the `apply` method over the constructor for the brevity of omitting `new`, which makes constructors much easier to read inside expressions.
Finally, the companion object also contains code to implement an extractor pattern for use in pattern matching.

```scala
// Case Class Declaration Syntax

case class className(parameter: type, ...) {
  declarationOrExpression
}
```

<br>

#### Case Objects

A final note. If you find yourself defining a case class with no constructor arguments you can instead a define a *case object*. A case object is defined just like a regular singleton object, but has a more meaningful `toString` method and extends the `Product` and `Serializable` traits.

```scala
case object Citizen {
  def firstName = "John"
  def lastName = "Doe"
  def name = firstName + " " + lastName
}

Citizen.toString // res0: String = Citizen
```

<br>

### Pattern Matching

With case classes, you can interact with objects via pattern matching (instead of calling methods and accessing fields).
Pattern matching is like an extended `if` expression that allows us to evaluate an expression depending on the "shape" of the data.

```scala
// Example of use

case class Person(firstName: String, lastName: String)

object Stormtrooper {
  def inspect(person: Person): String = person match {
    case Person("Luke", "Skywalker") => "Stop, rebel scum!"
    case Person("Han", "Solo") => "Stop, rebel scum!"
    case Person(first, last) => s"Move along, $first"
  }
}

Stormtrooper.inspect(Person("Berkan", "Marasli")) // res0: String = Move along, Berkan
Stormtrooper.inspect(Person("Han", "Solo")) // res1: String = Stop, rebel scum!
```

A pattern can be one of:

- a name, binding any value to that name
- an underscore, matching any value and ignoring it
- a literal, matching the value the literal denotes
- a constructor-style pattern for a case class

```scala
1. A name, which matches any value at that position and binds it to the given name. For example, the pattern Person(first, last) binds the name first to the value "Noel", and the name last to the value "Welsh".
2. An underscore (_), which matches any value and ignores it. For example, as Stormtroopers only care about the first name of ordinary citizens we could just write Person(first, _) to avoid binding a name to the value of the lastName.
3. A literal, which successfully matches only the value the literal respresents. So , for example, the pattern Person("Han", "Solo") matches the Person with first name "Han" and last name "Solo".
4. Another case class using the same constructor style syntax.
```

<br>

## Modelling Data with Traits















# Rock the JVM - Scala at Light Speed

## Collections

### Lists

```scala
val aList = List(1,2,3,4,5)
val firstElement = aList.head
val remainingElements = aList.tail
val aPrependedList = 0 :: aList // List(0,1,2,3,4,5)
val anExtendedList = 0 +: aList :+ 6 // List(0,1,2,3,4,5,6)
```

### Sequences

Sequences are used to access specific elements.

```scala
val aSequence: Seq[Int] = Seq(1,2,3)
val accessedElement = aSequence(1) // Element at index 1 is 2
```

### Vectors

Vectors are a fast implementation of Sequences.

```scala
val aVector = Vector(1,2,3,4,5)
```

### Sets

Sets allow for no duplicates.

```scala
val aSet = Set(1,2,3,4,1,2,3) // Set(1,2,3,4)
val setHas5 = aSet.contains(5) // false
val anAddedSet = aSet + 5 // Set(1,2,3,4,5)
val aRemovedSet = aSet - 3 // Set(1,2,4)
```

### Ranges

```scala
val aRange = 1 to 1000
val twoByTwo = aRange.map(x => x * 2).toList // List(2,4,6,...2000)
```

### Tuples

Groups of values under the same value.

```scala
val aTuple = ("Berkan", 1998, true)
```

### Maps

Key - Value pairs.

```scala
val aPhonebook: Map[String, Int] = Map(
	("Berkan", 078123),
  "Serkan" -> 079635
)
```

## Pattern Matching

- If pattern match doesn't match any case, it will throw a MatchError
- Pattern matching will try all cases in sequence (in order)

### Switch Expression

```scala
val anInteger = 55
val order = anInteger match {
  case 1 => "first"
  case 2 => "second"
  case _ => anInteger + "th"
}
println(order) // 55th
```

### Case Class Decomposition (by structure)

```scala
case class Person(name: String, age: Int)
val berkan = Person("Berkan", 23)
val personGreeting = berkan match {
  case Person(n, a) => s"My name is $n and I am $a years old"
  case _ => "Something else"
}
println(personGreeting) // My name is Berkan and I am 23 years old
```

### Tuple Decomposition (by structure)

```scala
val aTuple = ("Berkan Marasli", "Rock")
val musicDescription = aTuple match {
  case (name, genre) => s"$name likes $genre"
  case _ => "Something else"
}
```

### List Decomposition (by structure)

```scala
val aList = List(1,2,3)
val listDescription = aList match {
  case List(_, 2, _) => "List containing 2 on its second position"
  case _ => "Something else"
}
```







































