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

- **객체 지향 프로그래밍(OOP)**:
    - **클래스와 객체**: Java는 클래스를 통해 객체를 생성하고, 객체는 클래스의 인스턴스입니다.
    - **상속**: 기존 클래스(슈퍼클래스)의 기능을 확장하여 새로운 클래스(서브클래스)를 만들 수 있습니다.
    - **다형성**: 하나의 인터페이스나 부모 클래스에 대해 여러 구현을 사용할 수 있습니다.
    - **캡슐화**: 데이터와 메서드를 하나의 단위로 묶어 관리하며, 외부에서 직접 접근하지 못하도록 합니다.
    - **추상화**: 불필요한 세부 사항을 숨기고 중요한 정보만 노출하여 복잡성을 줄입니다.
- **플랫폼 독립성**:
    - **Java Virtual Machine (JVM)**: Java 바이트코드를 실행하는 가상 머신으로, 운영체제에 종속되지 않습니다.
    - **바이트코드**: Java 컴파일러가 생성한 중간 코드로, JVM에서 실행됩니다.
    - **Write Once, Run Anywhere (WORA)**: 한 번 작성된 Java 코드는 어느 플랫폼에서나 실행될 수 있습니다.
- **메모리 관리**:
    - **Garbage Collection (GC)**: 자동 메모리 관리 시스템으로, 사용되지 않는 객체를 자동으로 정리하여 메모리 누수를 방지합니다.
- **표준 라이브러리**:
    - **Java Standard Library**: 다양한 기능을 제공하는 표준 라이브러리로, 컬렉션 프레임워크, I/O 처리, 네트워킹, 멀티스레딩, 유틸리티 클래스 등이 포함됩니다.
- **멀티스레딩**:
    - **Thread 클래스와 Runnable 인터페이스**: 동시에 여러 작업을 수행할 수 있도록 지원합니다.
    - **동기화**: 여러 스레드가 공유 자원을 안전하게 접근할 수 있도록 하는 메커니즘입니다.
- **예외 처리**:
    - **try-catch 블록**: 예외를 처리하여 프로그램이 비정상적으로 종료되지 않도록 합니다.
    - **checked 예외와 unchecked 예외**: 컴파일러가 검사하는 예외와 검사하지 않는 예외로 구분됩니다.
- **Java Development Kit (JDK)**:
    - **컴파일러**: Java 소스 코드를 바이트코드로 컴파일합니다.
    - **Java Runtime Environment (JRE)**: JVM과 표준 라이브러리를 포함하여 Java 프로그램을 실행하는 환경을 제공합니다.
- **인터페이스와 추상 클래스**:
    - **인터페이스**: 메서드의 시그니처만을 정의하고, 클래스에서 이를 구현합니다.
    - **추상 클래스**: 일부 메서드만 구현하거나 추상 메서드를 포함할 수 있는 클래스입니다.