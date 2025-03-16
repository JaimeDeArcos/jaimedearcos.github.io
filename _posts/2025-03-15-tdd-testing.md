---
date: 2025-03-15T11:00:00.000Z
layout: post
title: Test-Driven Development (TDD) in Java with JUnit and Mockito
subtitle: Writing Better Code with Automated Testing
description: Understanding Test-Driven Development (TDD) in Java using JUnit and Mockito with practical examples.
image: /assets/img/uploads/tdd-java.png
optimized_image: /assets/img/uploads/tdd-java.png
category: code
tags:
  - code
  - java
  - testing
  - tdd
author: jaimedearcos
paginate: false 
---


> In modern software development, ensuring code quality and reliability is crucial. Many job interviews for Java developers assess knowledge of automated testing frameworks like JUnit and Mockito. Test-Driven Development (TDD) is a methodology that not only enhances code quality but is also a highly sought-after skill in the industry.

## What is TDD?

Test-Driven Development (TDD) is a software development methodology where tests are written before implementing functionality. It follows a repetitive cycle known as "Red-Green-Refactor":

- Red: Write a test that fails (since the functionality is not yet implemented).
- Green: Write the minimal code required to pass the test.
- Refactor: Improve the code while ensuring the test still passes.

This approach ensures that each piece of code is tested, making maintenance easier and reducing the likelihood of errors.

## Introduction to JUnit and Mockito

**JUnit**

JUnit is the most widely used unit testing framework in Java. It allows developers to write and execute automated tests efficiently.

Example of a simple test with JUnit 5:

```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class CalculatorTest {
    @Test
    void testSum() {
        Calculator calc = new Calculator();
        assertEquals(5, calc.add(2, 3));
    }
}
```
**Mockito**

Mockito is a framework that enables the creation of mock objects for unit testing. It is useful when testing components that depend on external services, such as databases or APIs.

Example using Mockito:

```java
import org.junit.jupiter.api.Test;
import org.mockito.Mockito;
import static org.mockito.Mockito.*;
import static org.junit.jupiter.api.Assertions.*;

class ServiceTest {
    @Test
    void testService() {
        ExternalService serviceMock = mock(ExternalService.class);
        when(serviceMock.getData()).thenReturn("Mocked data");
        
        Client client = new Client(serviceMock);
        assertEquals("Mocked data", client.getInformation());
    }
}

```
 
## TDD in Action: A Practical Example

Let's say we need to develop a user management service. First, we write a failing test:

```java
@Test
void testAddUser() {
    UserRepository repoMock = mock(UserRepository.class);
    UserService service = new UserService(repoMock);
    
    User user = new User("John", "john@example.com");
    when(repoMock.save(user)).thenReturn(true);
    
    assertTrue(service.registerUser(user));
} 
```
Next, we implement the minimal functionality required for the test to pass:

```java
class UserService {
private final UserRepository repository;
    
    public UserService(UserRepository repository) {
        this.repository = repository;
    }
    
    public boolean registerUser(User user) {
        return repository.save(user);
    }
}
```

Finally, we refactor the code if necessary while ensuring all tests continue to pass.

## Conclusion

Using TDD alongside tools like JUnit and Mockito not only improves software quality but also plays a crucial role in technical interviews. Many companies seek developers who understand and apply automated testing, as it ensures robust and maintainable code. Practicing TDD can set you apart in your next interview and enhance your career as a Java developer.

## Further Reading

To deepen your understanding of TDD, JUnit, and Mockito, consider exploring the following resources:

- [JUnit 5 User Guide](https://junit.org/junit5/docs/current/user-guide/)
- [Mockito Documentation](https://site.mockito.org/)
- [Test-Driven Development by Example - Kent Beck](https://www.oreilly.com/library/view/test-driven-development/0321146530/)

Mastering TDD will not only improve your coding skills but also help you build more reliable and maintainable software systems.