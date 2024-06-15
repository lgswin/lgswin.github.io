---
layout: post
title: "Kotlin Cheat sheet"
date: 2024-06-15
categories: [Kotlin]
tags: [Kotlin, Cheat Sheet]
---

### Basic Structure

```kotlin
fun main() {
    println("Hello, World!")
}

```

### Variable Declaration

```kotlin
// Immutable variable (read-only)
val name: String = "John"

// Mutable variable (modifiable)
var age: Int = 30

// Type inference
val city = "New York"
var isStudent = true

```

### Functions

```kotlin
// Function declaration
fun sum(a: Int, b: Int): Int {
    return a + b
}

// Expression body function
fun multiply(a: Int, b: Int) = a * b

// Default arguments
fun greet(name: String = "Guest") {
    println("Hello, $name")
}

```

### Conditional Statements

```kotlin
// if-else
val max = if (a > b) a else b

// when
when (x) {
    1 -> println("x is 1")
    2 -> println("x is 2")
    else -> println("x is neither 1 nor 2")
}

```

### Loops

```kotlin
// for loop
for (i in 1..5) {
    println(i)
}

// while loop
var i = 0
while (i < 5) {
    println(i)
    i++
}

```

### Classes and Objects

```kotlin
class Person(val name: String, var age: Int)

fun main() {
    val person = Person("John", 30)
    println("Name: ${person.name}, Age: ${person.age}")
}

```

### Inheritance

```kotlin
open class Animal {
    open fun sound() {
        println("Animal makes a sound")
    }
}

class Dog : Animal() {
    override fun sound() {
        println("Dog barks")
    }
}

fun main() {
    val dog = Dog()
    dog.sound()
}

```

### Data Classes

```kotlin
data class User(val name: String, val age: Int)

fun main() {
    val user = User("Alice", 25)
    println(user)
}

```

### Interfaces

```kotlin
interface Animal {
    fun sound()
}

class Dog : Animal {
    override fun sound() {
        println("Dog barks")
    }
}

fun main() {
    val dog = Dog()
    dog.sound()
}

```

### Null Safety

```kotlin
// Nullable type
var name: String? = "John"
name = null

// Safe call operator
val length = name?.length

// Elvis operator
val length = name?.length ?: 0

// Non-null assertion operator
val length = name!!.length

```

### Collections

```kotlin
// List
val fruits = listOf("Apple", "Banana", "Cherry")
println(fruits[0]) // Output: Apple

// MutableList
val vegetables = mutableListOf("Carrot", "Potato")
vegetables.add("Tomato")
println(vegetables)

// Map
val map = mapOf("name" to "John", "age" to 30)
println(map["name"]) // Output: John

```

### Higher-Order Functions and Lambdas

```kotlin
// Higher-order function
fun calculate(a: Int, b: Int, operation: (Int, Int) -> Int): Int {
    return operation(a, b)
}

val sum = calculate(5, 10) { x, y -> x + y }
println(sum) // Output: 15

// Lambda expression
val double = { x: Int -> x * 2 }
println(double(4)) // Output: 8

```

### Extension Functions

```kotlin
fun String.lastChar(): Char = this[this.length - 1]

fun main() {
    println("Kotlin".lastChar()) // Output: n
}

```

### Coroutines

```kotlin
import kotlinx.coroutines.*

fun main() = runBlocking {
    launch {
        delay(1000L)
        println("World!")
    }
    println("Hello,")
}

```