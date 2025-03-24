---
date: 2024-04-15T11:00:00.000Z
layout: post
title: 'Asynchronous Programming in Java: CompletableFuture vs Reactive Streams'
subtitle: Choosing the Right Tool for Concurrency in Modern Java
description: A deep dive into asynchronous programming in Java comparing CompletableFuture and Reactive Streams, with examples and output.
image: /assets/img/uploads/java-async.png
optimized_image: /assets/img/uploads/java-async.png
category: code
tags:
  - code
  - java
  - asynchronous
  - reactive
  - concurrency
author: jaimedearcos
paginate: false
---

> Asynchronous programming enables applications to execute non-blocking operations, crucial for performance in modern, concurrent systems. In Java, two powerful tools are commonly used: `CompletableFuture` and **Reactive Streams** (e.g., Project Reactor or RxJava). In this post, we’ll compare both approaches through real code examples and output results.

## 1. CompletableFuture: Java’s Built-in Promise API

`CompletableFuture` is a class introduced in Java 8 to write asynchronous, non-blocking code. It's suitable for simple async flows and integrates well with imperative codebases.

### Example 1: Basic Async Execution
```java
import java.util.concurrent.CompletableFuture;

public class CompletableFutureDemo {
    public static void main(String[] args) {
        CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
            sleep(1000);
            return "Hello from CompletableFuture!";
        });

        future.thenAccept(System.out::println);
        sleep(1500); // wait for completion
    }

    static void sleep(long millis) {
        try { Thread.sleep(millis); } catch (InterruptedException e) { }
    }
}
```

**Output:**
```
Hello from CompletableFuture!
```

### Example 2: Chaining and Combining
```java
import java.util.concurrent.CompletableFuture;

public class CompletableFutureCombineDemo {
    public static void main(String[] args) {
        CompletableFuture<String> greeting = CompletableFuture.supplyAsync(() -> "Hello");
        CompletableFuture<String> name = CompletableFuture.supplyAsync(() -> "World");

        CompletableFuture<String> result = greeting.thenCombine(name, (g, n) -> g + ", " + n);
        System.out.println(result.join());
    }
}
```

**Output:**
```
Hello, World
```

### Pros and Cons of CompletableFuture
**Pros:**
- Native Java API, no dependencies
- Familiar to imperative developers
- Easy for linear async flows

**Cons:**
- Limited operators compared to reactive libraries
- Complex error handling in larger flows
- Not suitable for high-throughput event streams

---

## 2. Reactive Streams: Declarative and Event-Driven

Reactive programming models async execution as a stream of data. With libraries like **Project Reactor** (`Mono`, `Flux`) or **RxJava**, you can build pipelines to transform, filter, and combine async data.

### Example 1: Mono (Single Async Value)
```java
import reactor.core.publisher.Mono;
import java.time.Duration;

public class ReactorMonoExample {
    public static void main(String[] args) {
        Mono.just("Hello from Mono!")
            .delayElement(Duration.ofMillis(500))
            .subscribe(System.out::println);

        sleep(1000);
    }

    static void sleep(long millis) {
        try { Thread.sleep(millis); } catch (InterruptedException e) { }
    }
}
```

**Output:**
```
Hello from Mono!
```

### Example 2: Flux (Multiple Async Events)
```java
import reactor.core.publisher.Flux;
import java.time.Duration;

public class ReactorFluxExample {
    public static void main(String[] args) {
        Flux.interval(Duration.ofMillis(300))
            .take(5)
            .map(i -> "Tick " + i)
            .subscribe(System.out::println);

        sleep(2000);
    }

    static void sleep(long millis) {
        try { Thread.sleep(millis); } catch (InterruptedException e) { }
    }
}
```

**Output:**
```
Tick 0
Tick 1
Tick 2
Tick 3
Tick 4
```

### Pros and Cons of Reactive Streams
**Pros:**
- Rich operator set for complex flows
- Built-in backpressure handling
- Ideal for data streams and event-driven apps

**Cons:**
- Requires learning new paradigm
- Can be overkill for simple use cases
- More dependencies (Reactor, RxJava)

---

## 3. When to Use Each?

| Use Case | CompletableFuture | Reactive Streams |
|----------|-------------------|------------------|
| Simple async call | ✅ | ❌ |
| API call + transform | ✅ | ✅ |
| Data pipeline (streaming) | ❌ | ✅ |
| Reactive UI/backend | ❌ | ✅ |
| Minimal dependencies | ✅ | ❌ |
| Advanced composition | ❌ | ✅ |

---

## Conclusion

Both `CompletableFuture` and Reactive Streams serve asynchronous needs, but they excel in different areas. For straightforward async calls, `CompletableFuture` is often enough. For complex, event-driven, and high-throughput systems, reactive programming shines.

By mastering both, you’ll be ready to choose the right tool for every concurrency challenge in modern Java applications.

# Further Reading
- [CompletableFuture Javadoc](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/CompletableFuture.html)
- [Project Reactor Documentation](https://projectreactor.io/docs)
- [Reactive Manifesto](https://www.reactivemanifesto.org/) 
