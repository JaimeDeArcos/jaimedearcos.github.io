---
date: 2023-10-02 18:00:00
layout: post
title: Introduction to Hexagonal Architecture
subtitle: A brief introduction
description: Introduction to Hexagonal Architecture
image: /assets/img/uploads/hexagonal-architecture.jpeg
optimized_image: /assets/img/uploads/hexagonal-architecture.jpeg
category: code
tags:
  - code
  - java
  - architecture
author: jaimedearcos
paginate: false
---
 
In the world of software development, architectural patterns play a pivotal role in creating robust, maintainable, and flexible systems. One such architectural pattern that has gained popularity in recent years is Hexagonal Architecture. In this article, we will delve into the fundamentals of Hexagonal Architecture, explore its advantages, and illustrate how to structure packages in a Java project following this architectural style.

## What is Hexagonal Architecture?

Hexagonal Architecture, also known as **Ports and Adapters** or the **Onion Architecture**, is a design pattern that promotes the separation of concerns and modularization in software systems. It was first introduced by Alistair Cockburn in 2005. At its core, Hexagonal Architecture revolves around the idea of isolating the core business logic from external dependencies and infrastructure concerns.

The central concept of Hexagonal Architecture is the division of an application into two main parts:

1. **Core Business Logic**: This is the heart of the application where the domain-specific rules and functionality reside. It should be completely isolated from any external factors like databases, user interfaces, or third-party services.

2. **External Interfaces**: These are the entry points and exit points to the application. They include adapters that communicate with the external world, such as REST APIs, databases, UI frameworks, and messaging systems.

The goal of Hexagonal Architecture is to ensure that changes in the external interfaces (adapters) do not impact the core business logic, and vice versa. This separation enables better testability, maintainability, and scalability of your software.

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*ebUHgG6JwawEifXlARgfzg.png)
_<center>source: <a href="https://blog.devgenius.io/flask-blog-tutorial-with-hexagonal-architecture-part-1-6446e7e9aaaa"> medium.com</a></center>_


## Advantages of Hexagonal Architecture

Utilizing Hexagonal Architecture offers several advantages:

1. **Modularity**: The separation of concerns into core business logic and external interfaces makes it easier to maintain and extend your application. Changes in one part of the system have minimal impact on the other.

2. **Testability**: Isolating the core logic allows for comprehensive unit testing without the need for complex mocking of external dependencies.

3. **Flexibility**: You can switch out external components (e.g., databases, UI frameworks) without affecting the core business logic. This adaptability is particularly beneficial when upgrading or changing technology stacks.

4. **Clean Code**: Hexagonal Architecture encourages clean and organized code. It promotes the use of interfaces and clear boundaries between components, making the codebase more understandable.

5. **Business Focus**: Developers can focus on implementing business requirements without being distracted by technical details, resulting in a more business-centric codebase.


## Ports and Adapters in Hexagonal Architecture

In Hexagonal Architecture, the concepts of "Ports" and "Adapters" play a central role in achieving its architectural goals. Let's take a closer look at what these terms mean:

### Ports

Ports are interfaces or contracts that define the interactions between the core business logic of your application and the external world. They represent the entry points and exit points for data and functionality. In simpler terms, ports are the "what" your application needs from the outside world.

- **Primary Ports (Inbound)**: These are entry points for external interactions with your application. Examples include REST API endpoints, command handlers, or event listeners.

- **Secondary Ports (Outbound)**: These are exit points through which your application interacts with external resources, such as databases, external services, or message queues. They define what your application provides to the external world.

Ports are defined as Java interfaces, for example, in the context of Spring Boot, they can be defined as Spring beans, ensuring that the core business logic interacts with the external world through these well-defined interfaces.

### Adapters

Adapters, on the other hand, are responsible for implementing the ports and enabling communication between the core business logic and external resources. Adapters bridge the gap between the application's internal interfaces (ports) and the external technologies and systems.

- **Primary Adapters (Inbound)**: These are implementations of the primary ports, allowing external systems to communicate with your application. For example, a REST controller that handles HTTP requests and routes them to the application's use cases.

- **Secondary Adapters (Outbound)**: These are implementations of the secondary ports, facilitating communication between your application and external resources. This could include database repositories, external API clients, or message queue consumers.

Adapters are responsible for translating external input into a format understandable by the core business logic and vice versa. They ensure that the application remains isolated from the specifics of the external technologies it interacts with.

## Structuring a Spring Boot Project with Hexagonal Architecture

Now, let's explore how you can structure a Java (Spring Boot) project using Hexagonal Architecture. Here's an example structure:

```css
myapp/
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   ├── com/
│   │   │   │   ├── myapp/
│   │   │   │   │   ├── application/
│   │   │   │   │   │   ├── MyApplication.java
│   │   │   │   │   │   └── ...
│   │   │   │   │   ├── domain/
│   │   │   │   │   │   ├── MyEntity.java
│   │   │   │   │   │   └── ...
│   │   │   │   │   ├── ports/
│   │   │   │   │   │   ├── MyRepository.java
│   │   │   │   │   │   └── ...
│   │   │   │   │   └── ...
│   │   │   │   ├── infrastructure/
│   │   │   │   │   ├── adapter/
│   │   │   │   │   │   ├── MyRepositoryImpl.java
│   │   │   │   │   │   └── ...
│   │   │   │   │   ├── web/
│   │   │   │   │   │   ├── MyController.java
│   │   │   │   │   │   └── ...
│   │   │   │   │   └── ...
│   ├── resources/
│   └── ...
└── ...
```

In this structure:

- `application`: Contains use cases and application services that orchestrate the core business logic.

- `domain`: Holds the domain model, including entities, value objects, and domain-specific logic.

- `ports`: Defines interfaces (ports) that the core business logic requires from external dependencies. Implementations of these interfaces are provided in the infrastructure package.

- `infrastructure`: Contains external adapters and implementations.

    - `adapter`: Houses adapters that connect the application to external resources such as databases or message queues. For example, MyRepositoryImpl implements the MyRepository interface.

    - `web`: This is where you define your REST API controllers. MyController represents a RESTful API controller.

With this structure, you maintain a clear separation between the core business logic (inside the application and domain packages) and the external interfaces (inside the infrastructure and web packages). This promotes testability, maintainability, and scalability while allowing you to leverage the Spring Boot framework for building RESTful APIs.


## Further Reading

If you're interested in diving deeper into Hexagonal Architecture and its application in Spring Boot projects, here are some valuable resources to explore:

- **[Hexagonal Architecture by Alistair Cockburn](https://alistair.cockburn.us/hexagonal-architecture/)**: The original article by Alistair Cockburn, the creator of Hexagonal Architecture, provides an in-depth explanation of the concept.
- **[Clean Architecture by Robert C. Martin](https://www.oreilly.com/library/view/clean-architecture-a/9780134494272/)**: Although not strictly Hexagonal Architecture, Clean Architecture shares similar principles and is worth exploring for a broader perspective on architectural patterns.

These resources will provide you with a solid foundation and practical insights into Hexagonal Architecture, helping you design and build well-structured and maintainable software applications.
