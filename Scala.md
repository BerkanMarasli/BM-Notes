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

| Operation       | Example                                    | Example Result             |
| --------------- | ------------------------------------------ | -------------------------- |
| ! (not)         | !true<br />!false                          | false<br />true            |
| == (equal to)   | true == true<br />true == false            | true<br />false            |
| > or <          | 10 > 5                                     | true                       |
| +<br />-<br />* | 1 + 1<br />2 - 1<br />5 * 3                | 2<br />1<br />15           |
| /               | 6 / 2<br />6 / 4<br />6.0 / 4<br />6 / 4.0 | 3<br />1<br />1.5<br />1.5 |



## Variables

val - immutable value (value cannot change after initialisation)

var - mutable value (value can change after initialisation via assignment)

Note:

- Only classes can have declared but unassigned variables
- Scala has built-in data type recognition
- Strings are assigned using double quotes "" only

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









## String methods





























