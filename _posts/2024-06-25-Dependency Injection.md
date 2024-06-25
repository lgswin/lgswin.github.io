---
layout: post
title: "Dependency Injection"
date: 2024-06-25
categories: [Design pattern]
tags: [Design pattern, Dependency Injection]
---

Dependency Injection (DI) is a design pattern used in software development to achieve Inversion of Control (IoC) between classes and their dependencies. Instead of a class creating its own dependencies, they are injected by an external entity, often a framework or container. This approach promotes loose coupling, easier testing, and better maintainability of the code.

### Key Concepts

1. **Dependency**:
    - An object that a class requires to function correctly. For example, a `UserService` class might depend on a `UserRepository` to fetch user data.
2. **Injection**:
    - The process of passing the dependencies to a class, rather than the class creating them itself. This can be done through constructor injection, setter injection, or method injection.

### Types of Dependency Injection

1. **Constructor Injection**:
    - Dependencies are provided through a class constructor.
    
    ```java
    public class UserService {
        private final UserRepository userRepository;
    
        public UserService(UserRepository userRepository) {
            this.userRepository = userRepository;
        }
    
        // methods using userRepository
    }
    
    ```
    
2. **Setter Injection**:
    - Dependencies are provided through setter methods.
    
    ```java
    public class UserService {
        private UserRepository userRepository;
    
        public void setUserRepository(UserRepository userRepository) {
            this.userRepository = userRepository;
        }
    
        // methods using userRepository
    }
    
    ```
    
3. **Method Injection**:
    - Dependencies are provided through methods that require them.
    
    ```java
    public class UserService {
        private UserRepository userRepository;
    
        public void performAction(UserRepository userRepository) {
            // use userRepository
        }
    }
    
    ```
    

### Benefits

1. **Loose Coupling**:
    - Classes depend on abstractions (interfaces) rather than concrete implementations, making them easier to change or replace.
2. **Easier Testing**:
    - Dependencies can be mocked or stubbed, facilitating unit testing.
3. **Better Maintainability**:
    - Changes to dependencies have minimal impact on the classes that use them.
4. **Flexibility**:
    - Dependencies can be changed at runtime without altering the classes that use them.

### Example

Consider a simple example where a `Car` class depends on an `Engine` class.

### Without Dependency Injection:

```java
public class Car {
    private Engine engine;

    public Car() {
        this.engine = new Engine();
    }

    public void start() {
        engine.run();
    }
}

```

### With Dependency Injection:

```java
public class Car {
    private final Engine engine;

    public Car(Engine engine) {
        this.engine = engine;
    }

    public void start() {
        engine.run();
    }
}

```

Here, the `Car` class does not create the `Engine` instance itself but expects it to be provided. This can be managed by a DI container or framework.

### Dependency Injection Frameworks

Various frameworks and containers help manage dependency injection:

1. **Java**:
    - Spring Framework
    - Google Guice
2. **.NET**:
    - .NET Core Dependency Injection
    - Autofac
    - Ninject
3. **JavaScript**:
    - Angular (for front-end applications)
    - InversifyJS (for Node.js applications)

### Example with Spring (Java):

```java
@Service
public class UserService {
    private final UserRepository userRepository;

    @Autowired
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    // methods using userRepository
}

```

Here, the `@Autowired` annotation tells Spring to inject the `UserRepository` dependency when creating an instance of `UserService`.

### Conclusion

Dependency Injection is a powerful pattern that improves code quality, testability, and maintainability by decoupling classes from their dependencies. By using DI, developers can create more modular, flexible, and easily testable code.