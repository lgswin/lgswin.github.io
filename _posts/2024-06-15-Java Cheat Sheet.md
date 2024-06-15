---
layout: post
title: "Java Cheat sheet"
date: 2024-06-15
categories: [Java]
tags: [Java, Cheat Sheet]
---

### Basics

```java
// Hello World Program
public class Main {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}

```

### Data Types

```java
// Primitive Data Types
int age = 25;
float price = 10.5f;
double distance = 45.6;
char grade = 'A';
boolean isAvailable = true;

// Reference Data Types
String name = "John";
int[] numbers = {1, 2, 3, 4, 5};

```

### Variables

```java
int x = 10;
final int CONSTANT = 100; // Constant variable

```

### Operators

```java
// Arithmetic Operators
int sum = 10 + 5;
int diff = 10 - 5;
int prod = 10 * 5;
int quot = 10 / 5;
int rem = 10 % 5;

// Comparison Operators
boolean isEqual = (10 == 5);
boolean isNotEqual = (10 != 5);
boolean isGreater = (10 > 5);
boolean isLesser = (10 < 5);
boolean isGreaterOrEqual = (10 >= 5);
boolean isLesserOrEqual = (10 <= 5);

// Logical Operators
boolean andResult = (true && false);
boolean orResult = (true || false);
boolean notResult = !true;

```

### Control Flow

```java
// If-Else
if (x > 0) {
    System.out.println("Positive");
} else if (x < 0) {
    System.out.println("Negative");
} else {
    System.out.println("Zero");
}

// Switch
switch (day) {
    case 1:
        System.out.println("Monday");
        break;
    case 2:
        System.out.println("Tuesday");
        break;
    default:
        System.out.println("Other day");
        break;
}

// Loops
// For Loop
for (int i = 0; i < 5; i++) {
    System.out.println(i);
}

// While Loop
int i = 0;
while (i < 5) {
    System.out.println(i);
    i++;
}

// Do-While Loop
int j = 0;
do {
    System.out.println(j);
    j++;
} while (j < 5);

```

### Methods

```java
public class Main {
    public static void main(String[] args) {
        greet("John");
        int result = add(5, 10);
        System.out.println("Sum: " + result);
    }

    // Method without return type
    public static void greet(String name) {
        System.out.println("Hello, " + name);
    }

    // Method with return type
    public static int add(int a, int b) {
        return a + b;
    }
}

```

### Classes and Objects

```java
public class Person {
    String name;
    int age;

    // Constructor
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // Method
    public void display() {
        System.out.println("Name: " + name + ", Age: " + age);
    }

    public static void main(String[] args) {
        Person person1 = new Person("John", 30);
        person1.display();
    }
}

```

### Inheritance

```java
class Animal {
    void eat() {
        System.out.println("This animal eats food.");
    }
}

class Dog extends Animal {
    void bark() {
        System.out.println("The dog barks.");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog();
        dog.eat();
        dog.bark();
    }
}

```

### Interfaces

```java
interface Animal {
    void eat();
}

class Dog implements Animal {
    public void eat() {
        System.out.println("The dog eats food.");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog();
        dog.eat();
    }
}

```

### Exception Handling

```java
try {
    int[] numbers = {1, 2, 3};
    System.out.println(numbers[10]);
} catch (ArrayIndexOutOfBoundsException e) {
    System.out.println("Array index out of bounds!");
} finally {
    System.out.println("This block is always executed.");
}

```

### Collections

```java
// ArrayList
import java.util.ArrayList;
ArrayList<String> list = new ArrayList<>();
list.add("Apple");
list.add("Banana");
System.out.println(list.get(0)); // Output: Apple

// HashMap
import java.util.HashMap;
HashMap<String, Integer> map = new HashMap<>();
map.put("Apple", 1);
map.put("Banana", 2);
System.out.println(map.get("Apple")); // Output: 1

```

### Lambda Expressions (Java 8+)

```java
import java.util.ArrayList;

ArrayList<Integer> numbers = new ArrayList<>();
numbers.add(5);
numbers.add(10);
numbers.add(15);

numbers.forEach((n) -> {
    System.out.println(n);
});

```

### Streams (Java 8+)

```java
import java.util.Arrays;
import java.util.List;

List<String> names = Arrays.asList("John", "Jane", "Jack");
names.stream()
     .filter(name -> name.startsWith("J"))
     .forEach(System.out::println); // Output: John, Jane, Jack

```