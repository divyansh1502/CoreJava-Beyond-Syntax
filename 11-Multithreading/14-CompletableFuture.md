# 14 — CompletableFuture

> **`CompletableFuture` provides a powerful way to build, combine, and handle asynchronous computations without manually blocking on every result.**

---

## 📌 Table of Contents

1. [Introduction](#-introduction)
2. [Why CompletableFuture?](#-why-completablefuture)
3. [What is CompletableFuture?](#-what-is-completablefuture)
4. [Future vs CompletableFuture](#-future-vs-completablefuture)
5. [Creating CompletableFuture](#-creating-completablefuture)
6. [supplyAsync()](#-supplyasync)
7. [runAsync()](#-runasync)
8. [thenApply()](#-thenapply)
9. [thenAccept()](#-thenaccept)
10. [thenRun()](#-thenrun)
11. [thenCompose()](#-thencompose)
12. [thenCombine()](#-thencombine)
13. [allOf()](#-allof)
14. [anyOf()](#-anyof)
15. [Exception Handling](#-exception-handling)
16. [exceptionally()](#-exceptionally)
17. [handle()](#-handle)
18. [whenComplete()](#-whencomplete)
19. [join()](#-join)
20. [get() vs join()](#-get-vs-join)
21. [Async vs Non-Async Methods](#-async-vs-non-async-methods)
22. [Custom Executor](#-custom-executor)
23. [CompletableFuture Pipeline](#-completablefuture-pipeline)
24. [Multiple Asynchronous Tasks](#-multiple-asynchronous-tasks)
25. [How CompletableFuture Works Internally](#-how-completablefuture-works-internally)
26. [Advantages](#-advantages)
27. [Disadvantages](#-disadvantages)
28. [Common Mistakes](#-common-mistakes)
29. [Future vs CompletableFuture](#-future-vs-completablefuture-1)
30. [DSA / Problem-Solving Relevance](#-dsa--problem-solving-relevance)
31. [30-Second Interview Answer](#-30-second-interview-answer)
32. [Cheat Sheet](#-cheat-sheet)
33. [Top 10 Interview Questions](#-top-10-interview-questions)

---

# 🔹 Introduction

In the previous topic, we learned:

```text
Callable
Future
```

A `Future` allows us to obtain the result of an asynchronous computation.

For example:

```java
Future<Integer> future =
        executor.submit(() -> {

            return 10 + 20;

        });

int result = future.get();
```

The problem is:

```java
future.get();
```

can block the current thread.

Also, if we have multiple dependent asynchronous operations, managing multiple Futures can become complicated.

Java provides:

```java
CompletableFuture
```

to make asynchronous programming more flexible and composable.

---

# 🔹 Why CompletableFuture?

Suppose we have:

```text
Get User
   ↓
Get User Orders
   ↓
Calculate Total
   ↓
Send Response
```

With basic `Future`, we may repeatedly call:

```java
future.get();
```

and manually coordinate the results.

With `CompletableFuture`, we can create a pipeline:

```text
Async Task
    ↓
thenApply()
    ↓
thenCompose()
    ↓
thenAccept()
```

This makes asynchronous workflows easier to compose.

---

# 🔹 What is CompletableFuture?

`CompletableFuture<T>` is a class that represents a future result of an asynchronous computation.

It implements:

```java
Future<T>
```

and:

```java
CompletionStage<T>
```

Package:

```java
java.util.concurrent
```

Example:

```java
CompletableFuture<Integer> future =
        CompletableFuture.supplyAsync(() -> {

            return 100;

        });
```

---

# 🔹 Future vs CompletableFuture

A traditional Future mainly gives us:

```text
Submit
   ↓
Future
   ↓
get()
```

`CompletableFuture` gives us:

```text
Async Task
   ↓
Transform
   ↓
Combine
   ↓
Handle errors
   ↓
Continue
```

Important methods include:

```text
supplyAsync()
runAsync()
thenApply()
thenAccept()
thenRun()
thenCompose()
thenCombine()
allOf()
anyOf()
exceptionally()
handle()
whenComplete()
```

---

# 🔹 Creating CompletableFuture

There are several ways to create one.

The most common asynchronous factory methods are:

```java
CompletableFuture.supplyAsync()
```

and:

```java
CompletableFuture.runAsync()
```

---

# 🔹 supplyAsync()

Use `supplyAsync()` when the asynchronous task produces a result.

Example:

```java
CompletableFuture<Integer> future =
        CompletableFuture.supplyAsync(() -> {

            return 10 + 20;

        });
```

The result type is:

```text
Integer
```

Therefore:

```java
CompletableFuture<Integer>
```

---

# 🔹 Getting Result from supplyAsync()

Example:

```java
CompletableFuture<Integer> future =
        CompletableFuture.supplyAsync(() -> {

            return 10 + 20;

        });

System.out.println(future.join());
```

Output:

```text
30
```

`join()` waits if necessary.

---

# 🔹 runAsync()

Use `runAsync()` when the asynchronous task does not return a result.

Example:

```java
CompletableFuture<Void> future =
        CompletableFuture.runAsync(() -> {

            System.out.println("Task running");

        });
```

Notice:

```text
supplyAsync()
→ returns a value

runAsync()
→ no result
```

---

# 🔹 supplyAsync() vs runAsync()

| `supplyAsync()` | `runAsync()` |
|---|---|
| Returns a result | No result |
| Uses `Supplier` | Uses `Runnable` |
| `CompletableFuture<T>` | `CompletableFuture<Void>` |
| Useful for computations | Useful for actions |

Example:

```java
CompletableFuture<Integer> future =
        CompletableFuture.supplyAsync(() -> 100);
```

versus:

```java
CompletableFuture<Void> future =
        CompletableFuture.runAsync(() -> {

            System.out.println("Hello");

        });
```

---

# 🔹 thenApply()

`thenApply()` transforms the result of a previous stage.

Think:

```text
Input
 ↓
Transformation
 ↓
Output
```

Example:

```java
CompletableFuture<Integer> future =
        CompletableFuture.supplyAsync(() -> 10);

CompletableFuture<Integer> result =
        future.thenApply(value -> value * 2);
```

The flow is:

```text
10
 ↓
* 2
 ↓
20
```

Then:

```java
System.out.println(result.join());
```

Output:

```text
20
```

---

# 🔹 Multiple thenApply()

You can chain multiple transformations.

```java
CompletableFuture<Integer> future =
        CompletableFuture.supplyAsync(() -> 10)
                .thenApply(value -> value * 2)
                .thenApply(value -> value + 5);
```

Flow:

```text
10
 ↓
20
 ↓
25
```

Result:

```java
System.out.println(future.join());
```

Output:

```text
25
```

---

# 🔹 thenAccept()

`thenAccept()` consumes the result but does not produce another result.

Example:

```java
CompletableFuture.supplyAsync(() -> 100)
        .thenAccept(value -> {

            System.out.println(
                    "Result: " + value
            );

        });
```

Conceptually:

```text
100
 ↓
Consume
 ↓
No new result
```

Its resulting type is:

```java
CompletableFuture<Void>
```

---

# 🔹 thenRun()

`thenRun()` executes an action after the previous stage completes.

It does not receive the previous result.

Example:

```java
CompletableFuture.supplyAsync(() -> {

    return 100;

}).thenRun(() -> {

    System.out.println("Task completed");

});
```

The `thenRun()` stage does not receive:

```text
100
```

It simply performs an action after completion.

---

# 🔹 thenApply vs thenAccept vs thenRun

| Method | Receives Result? | Returns New Result? |
|---|---:|---:|
| `thenApply()` | Yes | Yes |
| `thenAccept()` | Yes | No |
| `thenRun()` | No | No |

Memory trick:

```text
thenApply
→ Apply transformation

thenAccept
→ Accept result

thenRun
→ Just run something
```

---

# 🔹 thenCompose()

`thenCompose()` is used when one asynchronous operation depends on another asynchronous operation.

Suppose:

```text
Get User
   ↓
Get User Orders
```

The second operation needs the result of the first.

Example:

```java
CompletableFuture<String> getUser() {

    return CompletableFuture.supplyAsync(() -> {

        return "Divyansh";

    });
}
```

Then:

```java
CompletableFuture<String> result =
        getUser()
                .thenCompose(user ->
                        CompletableFuture.supplyAsync(() -> {

                            return user + "'s orders";

                        })
                );
```

Result:

```text
Divyansh's orders
```

---

# 🔹 Why not thenApply()?

Suppose:

```java
CompletableFuture<CompletableFuture<String>>
```

can occur if `thenApply()` returns another CompletableFuture.

Example conceptually:

```java
future.thenApply(value ->
        anotherAsyncOperation(value)
);
```

This can produce:

```text
CompletableFuture
        ↓
CompletableFuture<String>
```

So:

```text
CompletableFuture<CompletableFuture<String>>
```

`thenCompose()` flattens this structure:

```text
CompletableFuture<CompletableFuture<String>>
              ↓
      thenCompose()
              ↓
CompletableFuture<String>
```

Memory trick:

> **`thenCompose()` = chain dependent asynchronous operations and flatten the nested future.**

---

# 🔹 thenCombine()

`thenCombine()` combines the results of two independent asynchronous operations.

Suppose:

```text
Task A → 10
Task B → 20
```

We want:

```text
10 + 20 = 30
```

Example:

```java
CompletableFuture<Integer> future1 =
        CompletableFuture.supplyAsync(() -> 10);

CompletableFuture<Integer> future2 =
        CompletableFuture.supplyAsync(() -> 20);

CompletableFuture<Integer> result =
        future1.thenCombine(
                future2,
                (a, b) -> a + b
        );
```

Then:

```java
System.out.println(result.join());
```

Output:

```text
30
```

---

# 🔹 thenCompose vs thenCombine

### thenCompose

Use when:

```text
Task B depends on Task A
```

```text
A
↓
B
```

### thenCombine

Use when:

```text
A and B are independent
```

```text
A ──┐
    ├── Combine
B ──┘
```

This distinction is very important in interviews.

---

# 🔹 allOf()

`allOf()` waits for multiple CompletableFutures to complete.

Example:

```java
CompletableFuture<Void> task1 =
        CompletableFuture.runAsync(() -> {

            System.out.println("Task 1");

        });

CompletableFuture<Void> task2 =
        CompletableFuture.runAsync(() -> {

            System.out.println("Task 2");

        });

CompletableFuture<Void> all =
        CompletableFuture.allOf(
                task1,
                task2
        );
```

Then:

```java
all.join();
```

waits until both tasks complete.

Important:

> `allOf()` returns `CompletableFuture<Void>`. It signals completion of all supplied stages but does not automatically return their individual results as a collection.

---

# 🔹 anyOf()

`anyOf()` completes when any one of the supplied CompletableFutures completes.

Example:

```java
CompletableFuture<String> task1 =
        CompletableFuture.supplyAsync(() -> {

            return "Task 1";

        });

CompletableFuture<String> task2 =
        CompletableFuture.supplyAsync(() -> {

            return "Task 2";

        });

CompletableFuture<Object> result =
        CompletableFuture.anyOf(
                task1,
                task2
        );
```

Then:

```java
System.out.println(result.join());
```

The result comes from whichever supplied stage completes first.

---

# 🔹 allOf vs anyOf

```text
allOf()
   ↓
Wait for ALL

anyOf()
   ↓
Complete when ANY completes
```

Example:

```text
Task A ── 2 sec
Task B ── 4 sec
Task C ── 1 sec
```

`allOf()`:

```text
Wait ≈ 4 sec
```

`anyOf()`:

```text
Complete ≈ 1 sec
```

Actual timing depends on scheduling and execution conditions.

---

# 🔹 Exception Handling

Asynchronous operations can fail.

Example:

```java
CompletableFuture<Integer> future =
        CompletableFuture.supplyAsync(() -> {

            throw new RuntimeException(
                    "Something went wrong"
            );

        });
```

We can handle the failure using:

```text
exceptionally()
handle()
whenComplete()
```

---

# 🔹 exceptionally()

`exceptionally()` provides a fallback value when the previous stage completes exceptionally.

Example:

```java
CompletableFuture<Integer> future =
        CompletableFuture.supplyAsync(() -> {

            throw new RuntimeException("Error");

        }).exceptionally(ex -> {

            return 0;

        });
```

Then:

```java
System.out.println(future.join());
```

Output:

```text
0
```

Conceptually:

```text
Task
 ↓
Exception
 ↓
exceptionally()
 ↓
Fallback
```

---

# 🔹 handle()

`handle()` receives both:

```text
Result
Exception
```

Example:

```java
CompletableFuture<Integer> future =
        CompletableFuture.supplyAsync(() -> {

            return 100;

        }).handle((result, exception) -> {

            if(exception != null) {

                return 0;

            }

            return result;

        });
```

It can handle both successful and exceptional completion.

---

# 🔹 whenComplete()

`whenComplete()` allows us to perform an action after completion regardless of success or failure.

Example:

```java
CompletableFuture<Integer> future =
        CompletableFuture.supplyAsync(() -> {

            return 100;

        }).whenComplete((result, exception) -> {

            if(exception != null) {

                System.out.println(
                        "Task failed"
                );

            } else {

                System.out.println(
                        "Result: " + result
                );

            }

        });
```

Unlike `exceptionally()`, `whenComplete()` is mainly for observing/performing side effects and does not normally transform the result into a fallback value.

---

# 🔹 exceptionally vs handle vs whenComplete

| Method | Receives Result | Receives Exception | Can Transform Result |
|---|---:|---:|---:|
| `exceptionally()` | No | Yes | Yes |
| `handle()` | Yes | Yes | Yes |
| `whenComplete()` | Yes | Yes | Normally no |

Memory:

```text
exceptionally
→ Handle failure

handle
→ Handle result + failure

whenComplete
→ Observe completion
```

---

# 🔹 join()

`join()` waits for completion and returns the result.

Example:

```java
CompletableFuture<Integer> future =
        CompletableFuture.supplyAsync(() -> 100);

int result = future.join();
```

Output:

```text
100
```

---

# 🔹 get() vs join()

Both can wait for completion.

But their exception behavior differs.

### get()

```java
future.get();
```

can throw checked exceptions such as:

```text
InterruptedException
ExecutionException
```

### join()

```java
future.join();
```

throws an unchecked:

```java
CompletionException
```

when the computation completes exceptionally.

Comparison:

| `get()` | `join()` |
|---|---|
| From Future | CompletableFuture convenience method |
| Checked exceptions | Unchecked CompletionException |
| Requires exception handling | No checked exception handling required |
| Can use timeout overload | Basic join has no timeout |

---

# 🔹 Async vs Non-Async Methods

Consider:

```java
future.thenApply(value -> value * 2);
```

and:

```java
future.thenApplyAsync(value -> value * 2);
```

The `Async` version schedules the continuation asynchronously, typically using the common ForkJoinPool unless an executor is explicitly supplied.

Example:

```java
CompletableFuture<Integer> future =
        CompletableFuture.supplyAsync(() -> 10)
                .thenApplyAsync(value -> value * 2);
```

You can also provide your own executor:

```java
ExecutorService executor =
        Executors.newFixedThreadPool(2);

CompletableFuture<Integer> future =
        CompletableFuture.supplyAsync(
                () -> 10,
                executor
        ).thenApplyAsync(
                value -> value * 2,
                executor
        );
```

---

# 🔹 Custom Executor

By default, async methods such as:

```java
supplyAsync()
runAsync()
thenApplyAsync()
```

may use:

```text
ForkJoinPool.commonPool()
```

unless an executor is explicitly supplied.

Example:

```java
ExecutorService executor =
        Executors.newFixedThreadPool(4);

CompletableFuture<Integer> future =
        CompletableFuture.supplyAsync(
                () -> 100,
                executor
        );
```

This allows you to control the executor used for that asynchronous stage.

---

# 🔹 CompletableFuture Pipeline

A common pattern is:

```java
CompletableFuture
        .supplyAsync(() -> 10)
        .thenApply(value -> value * 2)
        .thenApply(value -> value + 5)
        .thenAccept(value -> {

            System.out.println(value);

        });
```

Flow:

```text
supplyAsync()
      ↓
10
      ↓
thenApply()
      ↓
20
      ↓
thenApply()
      ↓
25
      ↓
thenAccept()
      ↓
Print
```

---

# 🔹 Multiple Asynchronous Tasks

Suppose we have:

```text
Fetch User
Fetch Products
```

Both can execute independently.

```java
CompletableFuture<String> user =
        CompletableFuture.supplyAsync(() -> {

            return "User";

        });

CompletableFuture<String> products =
        CompletableFuture.supplyAsync(() -> {

            return "Products";

        });
```

Then combine:

```java
CompletableFuture<String> result =
        user.thenCombine(
                products,
                (u, p) -> u + " + " + p
        );
```

Then:

```java
System.out.println(result.join());
```

Possible output:

```text
User + Products
```

---

# 🔹 How CompletableFuture Works Internally

A simplified model:

```text
                CompletableFuture
                       |
          +------------+------------+
          |                         |
       Result                    Completion
          |                         |
          ↓                         ↓
     Value / Error          Dependent Stage
                                  |
                                  ↓
                            thenApply()
                                  |
                                  ↓
                            thenCompose()
                                  |
                                  ↓
                            thenAccept()
```

The important concept is that a CompletableFuture can represent both:

```text
Current asynchronous computation
```

and:

```text
Dependent computations
```

that should execute after it completes.

---

# 🔹 Completion Stages

`CompletableFuture` implements:

```java
CompletionStage
```

A completion stage represents a step in an asynchronous pipeline.

Example:

```java
CompletableFuture.supplyAsync(() -> 10)
        .thenApply(value -> value * 2)
        .thenApply(value -> value + 5);
```

There are multiple stages:

```text
Stage 1
   ↓
10

Stage 2
   ↓
20

Stage 3
   ↓
25
```

This is the foundation of CompletableFuture composition.

---

# 🔹 Advantages

## 1. Asynchronous Pipelines

You can chain operations:

```java
thenApply()
thenCompose()
thenAccept()
```

---

## 2. Easy Combination

Independent tasks can be combined using:

```java
thenCombine()
```

---

## 3. Multiple Task Coordination

Use:

```java
allOf()
anyOf()
```

---

## 4. Error Handling

Provides:

```java
exceptionally()
handle()
whenComplete()
```

---

## 5. Less Manual Blocking

Instead of repeatedly calling:

```java
future.get();
```

you can compose dependent operations.

---

# 🔹 Disadvantages

## 1. Complex Chains

Very long pipelines can become difficult to read.

---

## 2. Debugging Difficulty

Asynchronous execution can make debugging more complicated.

---

## 3. Thread Pool Misuse

Poor executor configuration can hurt performance.

---

## 4. Blocking Still Possible

Methods such as:

```java
join()
get()
```

can still block.

CompletableFuture does not magically make every operation non-blocking.

---

# 🔹 Common Mistakes

## ❌ Mistake 1 — Thinking CompletableFuture means no blocking

This:

```java
future.join();
```

can block.

---

## ❌ Mistake 2 — Using thenApply for dependent async operations

If the transformation itself returns a CompletableFuture:

```java
future.thenApply(value ->
        anotherAsyncOperation(value)
);
```

you may get a nested Future.

Use:

```java
thenCompose()
```

when you need to flatten dependent asynchronous operations.

---

## ❌ Mistake 3 — Confusing thenCombine and thenCompose

```text
thenCompose
→ B depends on A

thenCombine
→ A and B are independent
```

---

## ❌ Mistake 4 — Assuming Async means a new thread is always created

Methods such as:

```java
thenApplyAsync()
```

schedule asynchronous execution through an executor.

They do not necessarily mean a brand-new thread is created for every stage.

---

## ❌ Mistake 5 — Ignoring exceptions

Async failures should be handled deliberately.

For example:

```java
.exceptionally(ex -> {

    return fallbackValue;

});
```

---

# 🔹 Future vs CompletableFuture

| Future | CompletableFuture |
|---|---|
| Basic async result | Rich async pipeline |
| `get()` | `get()` + `join()` |
| Basic cancellation | Cancellation support |
| Limited composition | Extensive composition |
| Limited error handling | Built-in error handling |
| Harder to chain | Easy chaining |
| Result retrieval focused | Workflow composition focused |

---

# 🔹 DSA / Problem-Solving Relevance

CompletableFuture is useful when independent computational tasks can execute concurrently.

Example:

```text
Array
 |
 +---- Part 1 → Task A
 |
 +---- Part 2 → Task B
 |
 +---- Part 3 → Task C
```

Each task can produce a result.

Then:

```text
Task A ──┐
Task B ──┼── allOf / combine
Task C ──┘
             ↓
         Final result
```

For example, imagine calculating the sum of independent array segments.

```java
CompletableFuture<Integer> part1 =
        CompletableFuture.supplyAsync(() -> {

            return 10;

        });

CompletableFuture<Integer> part2 =
        CompletableFuture.supplyAsync(() -> {

            return 20;

        });

CompletableFuture<Integer> total =
        part1.thenCombine(
                part2,
                Integer::sum
        );
```

Result:

```java
System.out.println(total.join());
```

Output:

```text
30
```

The important DSA/concurrency idea is:

> Independent work can potentially be decomposed, executed concurrently, and combined afterward.

---

# 🔹 Problem-Solving Mindset

When designing an asynchronous workflow, ask:

```text
Is the task independent?
        |
        +---- Yes → Run concurrently
        |
        +---- No → Chain dependency
```

Then:

```text
Independent tasks
      ↓
thenCombine()
allOf()
anyOf()
```

Dependent tasks:

```text
Task A
  ↓
Task B
  ↓
thenCompose()
```

Transformation:

```text
Result
  ↓
thenApply()
```

Consume:

```text
Result
  ↓
thenAccept()
```

Error:

```text
Exception
  ↓
exceptionally()
handle()
whenComplete()
```

---

# 🔹 30-Second Interview Answer

> `CompletableFuture` is a Java class used for composing asynchronous computations. It implements `Future` and `CompletionStage`. Methods such as `supplyAsync()` and `runAsync()` start asynchronous tasks, while `thenApply()`, `thenCompose()`, and `thenCombine()` allow us to transform, chain, and combine results. It also provides error-handling methods such as `exceptionally()`, `handle()`, and `whenComplete()`. Compared with a traditional Future, CompletableFuture provides much richer asynchronous composition.

---

# 🔹 Cheat Sheet

## Async computation with result

```java
CompletableFuture<Integer> future =
        CompletableFuture.supplyAsync(() -> 100);
```

---

## Async computation without result

```java
CompletableFuture<Void> future =
        CompletableFuture.runAsync(() -> {

            System.out.println("Task");

        });
```

---

## Transform result

```java
future.thenApply(value -> value * 2);
```

---

## Consume result

```java
future.thenAccept(value -> {

    System.out.println(value);

});
```

---

## Run after completion

```java
future.thenRun(() -> {

    System.out.println("Done");

});
```

---

## Chain dependent async task

```java
future.thenCompose(value ->
        anotherAsyncTask(value)
);
```

---

## Combine independent tasks

```java
future1.thenCombine(
        future2,
        (a, b) -> a + b
);
```

---

## Wait for all

```java
CompletableFuture.allOf(
        future1,
        future2
);
```

---

## Complete when any finishes

```java
CompletableFuture.anyOf(
        future1,
        future2
);
```

---

## Error fallback

```java
future.exceptionally(ex -> {

    return 0;

});
```

---

## Handle result/error

```java
future.handle((result, exception) -> {

    if(exception != null) {

        return 0;

    }

    return result;

});
```

---

## Observe completion

```java
future.whenComplete((result, exception) -> {

    System.out.println("Completed");

});
```

---

## Get result

```java
future.join();
```

or:

```java
future.get();
```

---

# 🧠 Memory Tricks

### `supplyAsync()`

> **Supply a result asynchronously.**

```text
Supply → Result
```

---

### `runAsync()`

> **Run asynchronously, no result.**

```text
Run → Nothing
```

---

### `thenApply()`

> **Transform.**

```text
A → B
```

---

### `thenAccept()`

> **Consume.**

```text
A → Done
```

---

### `thenRun()`

> **Run an action after completion.**

```text
Done → Action
```

---

### `thenCompose()`

> **Dependent async → flatten.**

```text
A → B
```

---

### `thenCombine()`

> **Independent async → combine.**

```text
A ──┐
    ├── C
B ──┘
```

---

### `allOf()`

> **ALL must complete.**

---

### `anyOf()`

> **ANY can complete the stage.**

---

### `exceptionally()`

> **Exception → fallback.**

---

### `handle()`

> **Result + Exception → decide.**

---

### `whenComplete()`

> **Observe completion.**

---

# 🔥 Top 10 Interview Questions

## 1. What is CompletableFuture?

`CompletableFuture` is a Java class used to represent and compose asynchronous computations.

---

## 2. What is the difference between Future and CompletableFuture?

`Future` mainly provides result retrieval, cancellation, and completion checking.

`CompletableFuture` additionally provides rich composition, transformation, combination, and exception-handling capabilities.

---

## 3. Difference between supplyAsync() and runAsync()?

```text
supplyAsync()
→ returns result

runAsync()
→ no result
```

---

## 4. What does thenApply() do?

It transforms the result of a completed stage.

```java
.thenApply(value -> value * 2);
```

---

## 5. Difference between thenCompose() and thenCombine()?

```text
thenCompose()
→ dependent asynchronous operations

thenCombine()
→ independent asynchronous operations
```

---

## 6. What does allOf() do?

It creates a CompletableFuture that completes when all supplied futures complete.

---

## 7. What does anyOf() do?

It creates a CompletableFuture that completes when any supplied future completes.

---

## 8. How do you handle exceptions?

Common methods include:

```java
exceptionally()
handle()
whenComplete()
```

---

## 9. Difference between get() and join()?

```text
get()
→ checked exceptions

join()
→ unchecked CompletionException for exceptional completion
```

Both may wait for completion.

---

## 10. Does CompletableFuture always create a new thread?

No.

Async stages use an executor, commonly the common ForkJoinPool when no executor is explicitly supplied. Non-async continuation methods may execute in the thread that completes the previous stage.

---

# 🎯 Final Summary

```text
                    CompletableFuture
                           |
          +----------------+----------------+
          |                |                |
       Create           Transform        Combine
          |                |                |
   supplyAsync()      thenApply()      thenCombine()
   runAsync()         thenCompose()    allOf()
                      thenAccept()     anyOf()
                      thenRun()
          |
          +----------------+----------------+
                           |
                      Error Handling
                           |
              +------------+------------+
              |            |            |
        exceptionally()  handle()  whenComplete()
                           |
                           ↓
                        Result
                           |
                     join() / get()
```

### ⭐ Core Pipeline

```java
CompletableFuture.supplyAsync(() -> 10)
        .thenApply(value -> value * 2)
        .thenApply(value -> value + 5)
        .thenAccept(System.out::println);
```

Flow:

```text
10
 ↓
20
 ↓
25
 ↓
Print
```

### ⭐ Most Important Differences

```text
supplyAsync()
→ async + result

runAsync()
→ async + no result

thenApply()
→ transform

thenCompose()
→ dependent async + flatten

thenCombine()
→ combine independent tasks

allOf()
→ wait for all

anyOf()
→ first completion

exceptionally()
→ fallback on exception

handle()
→ result + exception

whenComplete()
→ observe completion
```

### ⭐ One-Line Interview Memory

> **CompletableFuture lets you build asynchronous pipelines by creating, transforming, combining, and handling the results of asynchronous computations.**