---
date: 2024-05-15T11:00:00.000Z
layout: post
title: A Guide to Pattern Matching and Sealed Classes in Java 21
subtitle: Writing Safer and More Expressive Java Code
description: A comprehensive introduction to pattern matching and sealed classes in Java 21, including practical examples and best practices.
image: /assets/img/uploads/java-pattern.matching.png
optimized_image: /assets/img/uploads/java-pattern.matching.png
category: code
tags:
  - code
  - java
  - pattern-matching
  - sealed-classes
  - java21
author: jaimedearcos
paginate: false
---

> Java 21 introduces powerful enhancements that improve readability, safety, and expressiveness in your code. Among these, **pattern matching** and **sealed classes** stand out, allowing developers to write concise, readable, and safe code. Let's explore both concepts with practical examples.

## Pattern Matching for Switch

Pattern matching extends the classic `switch` statement, making code simpler and clearer by reducing the need for explicit casting and type checking.

### Traditional Switch

```java
Object obj = "Hello";

if (obj instanceof String) {
    String s = (String) obj;
    System.out.println(s.toLowerCase());
}
```

### Pattern Matching Switch

```java
Object obj = "Hello";

switch (obj) {
    case String s -> System.out.println(s.toLowerCase());
    case Integer i -> System.out.println(i + 1);
    default -> System.out.println("Unknown type");
}
```

### Benefits:

- Reduced boilerplate code
- Enhanced readability
- Improved type safety

## Sealed Classes

Sealed classes allow you to define which subclasses or implementations are permitted. This helps in modeling clear and controlled domain hierarchies.

### Declaring a Sealed Class

```java
public sealed abstract class Shape
    permits Circle, Rectangle {}

public final class Circle extends Shape {
    double radius;
}

public final class Rectangle extends Shape {
    double length, width;
}
```

### Using Pattern Matching with Sealed Classes
```java
Shape shape = new Circle();

double area = switch (shape) {
    case Circle c -> Math.PI * c.radius * c.radius;
    case Rectangle r -> r.length * r.width;
};

System.out.println("Area: " + area);
```

### Benefits:
- Enhanced clarity of inheritance hierarchies
- Compile-time enforcement of permitted subclasses
- Simplified, safer code when used with pattern matching

## Practical Applications
- Domain modeling with restricted class hierarchies
- Type-safe parsing and processing of structured data
- Cleaner implementations of polymorphic behavior

## Conclusion

Pattern matching and sealed classes provide significant improvements in Java 21, helping you write cleaner, safer, and more maintainable code. Mastering these tools allows developers to leverage the full expressive power of modern Java.

# Further Reading
- [Java 21 Pattern Matching JEP](https://openjdk.org/jeps/441)
- [Java 21 Sealed Classes JEP](https://openjdk.org/jeps/409)
