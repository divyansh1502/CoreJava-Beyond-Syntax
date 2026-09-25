# 13 — Callable and Future

> **`Callable` represents a task that can return a result and throw checked exceptions, while `Future` represents the result of an asynchronous computation.**

---

## 📌 Table of Contents

1. [Introduction](#-introduction)
2. [Why Callable and Future?](#-why-callable-and-future)
3. [Callable](#-callable)
4. [Callable vs Runnable](#-callable-vs-runnable)
5. [Creating a Callable](#-creating-a-callable)
6. [Submitting Callable](#-submitting-callable)
7. [Future](#-future)
8. [Future.get()](#-futureget)
9. [Future.get() with Timeout](#-futureget-with-timeout)
10. [Future.isDone()](#-futureisdone)
11. [Future.isCancelled()](#-futureiscancelled)
12. [Future.cancel()](#-futurecancel)
13. [Callable and Future Example](#-callable-and-future-example)
14. [Multiple Callable Tasks](#-multiple-callable-tasks)
15. [Future with Runnable](#-future-with-runnable)
16. [Exception Handling](#-exception-handling)
17. [Blocking Nature of get()](#-blocking-nature-of-get)
18. [Cancellation](#-cancellation)
19. [How Callable and Future Work](#-how-callable-and-future-work)
20. [Future Lifecycle](#-future-lifecycle)
21. [Advantages](#-advantages)
22. [Disadvantages](#-disadvantages)
23. [Common Mistakes](#-common-mistakes)
24. [Callable vs Runnable](#-callable-vs-runnable)
25. [Future vs Thread](#-future-vs-thread)
26. [Future vs CompletableFuture](#-future-vs-completablefuture)
27. [DSA / Problem-Solving Relevance](#-dsa--problem-solving-relevance)
28. [30-Second Interview Answer](#-30-second-interview-answer)
29. [Cheat Sheet](#-cheat-sheet)
30. [Top 10 Interview Questions](#-top-10-interview-questions)

---

# 🔹 Introduction

In the previous topic, we learned about:

```text
Executor
ExecutorService
Thread Pool
execute()
submit()
```

Now we go one step further.

Sometimes we don't just want to execute a task.

We also want:

```text
Task
  ↓
Perform computation
  ↓
Return result
```

For this, Java provides:

```text
Callable
Future
```

Both are part of:

```java
java.util.concurrent
```

---

# 🔹 Why Callable and Future?

`Runnable` is useful when we simply want to execute some code.

Example:

```java
Runnable task = () -> {

    System.out.println("Task running");

};
```

But suppose we want the task to calculate:

```text
100 + 200
```

and return:

```text
300
```

`Runnable` cannot directly return a result.

This is where `Callable` comes in.

```java
Callable<Integer> task = () -> {

    return 100 + 200;

};
```

When submitted to an executor, we receive a:

```java
Future<Integer>
```

which represents the result that will become available.

---

# 🔹 Callable

`Callable` is a functional interface in:

```java
java.util.concurrent
```

Its main method is:

```java
V call() throws Exception;
```

The important features are:

```text
Can return a value
Can throw checked exceptions
```

Example:

```java
Callable<Integer> task = () -> {

    return 10 + 20;

};
```

Here:

```text
Callable<Integer>
        ↓
Returns Integer
```

---

# 🔹 Callable vs Runnable

### Runnable

```java
Runnable task = () -> {

    System.out.println("Running");

};
```

Its method:

```java
void run()
```

Therefore:

```text
No return value
```

---

### Callable

```java
Callable<Integer> task = () -> {

    return 10 + 20;

};
```

Its method:

```java
V call()
```

Therefore:

```text
Can return value
```

---

# 🔹 Creating a Callable

Example:

```java
Callable<Integer> task = () -> {

    int a = 10;
    int b = 20;

    return a + b;

};
```

The return type is:

```java
Integer
```

because:

```java
Callable<Integer>
```

was declared.

---

# 🔹 Callable with String

Callable can return any reference type.

Example:

```java
Callable<String> task = () -> {

    return "Hello Java";

};
```

---

# 🔹 Callable with List

```java
Callable<List<Integer>> task = () -> {

    return Arrays.asList(10, 20, 30);

};
```

The generic type determines the result type.

---

# 🔹 Submitting Callable

Create an executor:

```java
ExecutorService executor =
        Executors.newFixedThreadPool(2);
```

Create Callable:

```java
Callable<Integer> task = () -> {

    return 10 + 20;

};
```

Submit it:

```java
Future<Integer> future =
        executor.submit(task);
```

Then retrieve the result:

```java
Integer result = future.get();
```

Finally:

```java
executor.shutdown();
```

Complete example:

```java
import java.util.concurrent.Callable;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

public class Main {

    public static void main(String[] args)
            throws Exception {

        ExecutorService executor =
                Executors.newFixedThreadPool(2);

        Callable<Integer> task = () -> {

            return 10 + 20;

        };

        Future<Integer> future =
                executor.submit(task);

        Integer result = future.get();

        System.out.println(result);

        executor.shutdown();
    }
}
```

Output:

```text
30
```

---

# 🔹 Future

`Future` represents the result of an asynchronous computation.

Package:

```java
java.util.concurrent.Future
```

Example:

```java
Future<Integer> future =
        executor.submit(task);
```

At this moment, the task may still be running.

The `Future` acts as a handle through which we can:

```text
Get result
Check completion
Cancel task
Check cancellation
```

---

# 🔹 Future.get()

`get()` retrieves the result.

Example:

```java
Future<Integer> future =
        executor.submit(() -> {

            return 100;

        });

Integer result = future.get();
```

Output:

```text
100
```

But there is an important point.

If the task has not finished:

```java
future.get();
```

waits for it.

Therefore:

```text
get()
 ↓
May block
 ↓
Until result is available
```

---

# 🔹 Future.get() with Timeout

We can specify a maximum waiting time.

Syntax:

```java
future.get(timeout, unit);
```

Example:

```java
Integer result =
        future.get(5, TimeUnit.SECONDS);
```

This means:

```text
Wait up to 5 seconds
```

If the task does not finish within that time, a:

```java
TimeoutException
```

may be thrown.

---

# 🔹 Future.isDone()

`isDone()` checks whether the task has completed.

Example:

```java
Future<Integer> future =
        executor.submit(() -> {

            return 100;

        });

System.out.println(future.isDone());

Integer result = future.get();

System.out.println(future.isDone());
```

Possible output:

```text
false
true
```

The first value depends on timing.

Important:

```text
isDone() == true
```

means the computation has completed, including normally, exceptionally, or through cancellation.

---

# 🔹 Future.isCancelled()

Checks whether the task was cancelled before completion.

Example:

```java
if(future.isCancelled()) {

    System.out.println("Task was cancelled");

}
```

Returns:

```text
true
```

if the Future was successfully cancelled.

---

# 🔹 Future.cancel()

Used to attempt cancellation.

Example:

```java
future.cancel(true);
```

The argument:

```text
true
```

means the executor should attempt to interrupt the thread running the task if it has started.

Example:

```java
Future<Integer> future =
        executor.submit(() -> {

            Thread.sleep(5000);

            return 100;

        });

future.cancel(true);
```

After successful cancellation:

```java
future.isCancelled()
```

can return:

```text
true
```

---

# 🔹 cancel(false) vs cancel(true)

### cancel(false)

```java
future.cancel(false);
```

Does not attempt to interrupt a task that is already running.

---

### cancel(true)

```java
future.cancel(true);
```

Attempts to interrupt the thread executing the task if it has started.

Important:

> Interruption is a request, not a guarantee that arbitrary code immediately stops.

---

# 🔹 Callable and Future Example

```java
import java.util.concurrent.Callable;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

public class Main {

    public static void main(String[] args)
            throws Exception {

        ExecutorService executor =
                Executors.newFixedThreadPool(2);

        Callable<Integer> task = () -> {

            System.out.println(
                    "Calculating..."
            );

            Thread.sleep(2000);

            return 50 + 50;
        };

        Future<Integer> future =
                executor.submit(task);

        System.out.println(
                "Task submitted"
        );

        Integer result =
                future.get();

        System.out.println(
                "Result: " + result
        );

        executor.shutdown();
    }
}
```

Conceptually:

```text
Main Thread
     |
     | submit()
     ↓
Executor
     |
     ↓
Worker Thread
     |
     ↓
Callable
     |
     ↓
Returns 100
     |
     ↓
Future
     |
     ↓
future.get()
     |
     ↓
Main Thread receives 100
```

---

# 🔹 Multiple Callable Tasks

We can submit multiple Callable tasks.

Example:

```java
ExecutorService executor =
        Executors.newFixedThreadPool(3);

Callable<Integer> task1 = () -> {

    return 10;

};

Callable<Integer> task2 = () -> {

    return 20;

};

Callable<Integer> task3 = () -> {

    return 30;

};

Future<Integer> future1 =
        executor.submit(task1);

Future<Integer> future2 =
        executor.submit(task2);

Future<Integer> future3 =
        executor.submit(task3);

System.out.println(future1.get());
System.out.println(future2.get());
System.out.println(future3.get());

executor.shutdown();
```

Output:

```text
10
20
30
```

---

# 🔹 Multiple Tasks Running Concurrently

Consider:

```java
Callable<Integer> task1 = () -> {

    Thread.sleep(2000);

    return 10;

};

Callable<Integer> task2 = () -> {

    Thread.sleep(2000);

    return 20;

};
```

With:

```java
ExecutorService executor =
        Executors.newFixedThreadPool(2);
```

both tasks can execute concurrently.

Conceptually:

```text
Worker 1 → Task 1 → 2 seconds
Worker 2 → Task 2 → 2 seconds
```

instead of:

```text
Task 1 → 2 seconds
Task 2 → 2 seconds
Total ≈ 4 seconds
```

the execution can overlap:

```text
Total ≈ 2 seconds
```

ignoring scheduling and other overhead.

---

# 🔹 Future with Runnable

`submit()` can also accept a `Runnable`.

Example:

```java
Runnable task = () -> {

    System.out.println("Running");

};

Future<?> future =
        executor.submit(task);
```

The Future does not contain a meaningful computed result from the Runnable.

Calling:

```java
future.get();
```

can be used to wait for completion.

The result is:

```text
null
```

for the ordinary `Runnable` overload.

---

# 🔹 Callable with Exception

Callable can throw checked exceptions.

Example:

```java
Callable<Integer> task = () -> {

    Thread.sleep(1000);

    return 100;

};
```

`Thread.sleep()` throws:

```java
InterruptedException
```

Callable allows this checked exception to be declared through:

```java
V call() throws Exception;
```

When calling:

```java
future.get();
```

you must handle possible exceptions such as:

```text
InterruptedException
ExecutionException
```

and with timed `get()`:

```text
TimeoutException
```

---

# 🔹 Exception Handling

Example:

```java
try {

    Integer result = future.get();

    System.out.println(result);

} catch(InterruptedException e) {

    Thread.currentThread().interrupt();

} catch(ExecutionException e) {

    System.out.println(
            "Task failed: " +
            e.getCause()
    );
}
```

Important:

If the Callable throws an exception, `Future.get()` typically throws:

```java
ExecutionException
```

The original exception can be accessed through:

```java
e.getCause()
```

---

# 🔹 Blocking Nature of get()

This is one of the most important concepts.

Suppose:

```java
Callable<Integer> task = () -> {

    Thread.sleep(5000);

    return 100;

};
```

Then:

```java
Future<Integer> future =
        executor.submit(task);

Integer result =
        future.get();
```

If the task is still running:

```text
future.get()
      ↓
Main thread waits
      ↓
Task completes
      ↓
Result returned
```

Therefore:

> `Future.get()` is potentially blocking.

---

# 🔹 Non-Blocking Check with isDone()

Instead of immediately calling:

```java
future.get();
```

we can check:

```java
if(future.isDone()) {

    System.out.println(
            future.get()
    );

}
```

But be careful.

Repeatedly checking:

```java
while(!future.isDone()) {

    // keep checking
}
```

can waste CPU.

This is called:

```text
Busy waiting / polling
```

depending on the implementation.

---

# 🔹 Cancellation

A task can be cancelled through its Future.

Example:

```java
Future<Integer> future =
        executor.submit(() -> {

            Thread.sleep(10000);

            return 100;

        });

future.cancel(true);
```

Then:

```java
future.isCancelled()
```

may return:

```text
true
```

Calling:

```java
future.get();
```

after successful cancellation can throw:

```java
CancellationException
```

---

# 🔹 How Callable and Future Work

The simplified flow is:

```text
Callable
   |
   | submit()
   ↓
ExecutorService
   |
   ↓
Thread Pool
   |
   ↓
Worker Thread
   |
   ↓
Callable executes
   |
   ↓
Result produced
   |
   ↓
Future stores/represents result
   |
   ↓
future.get()
   |
   ↓
Caller receives result
```

---

# 🔹 Future Lifecycle

A simplified Future lifecycle:

```text
          Created
             |
             ↓
          Running
             |
       +-----+-----+
       |           |
       ↓           ↓
   Completed    Cancelled
       |
       ↓
   Result/Error
```

A Future may represent:

```text
Successful completion
Exceptional completion
Cancellation
```

---

# 🔹 Advantages

## 1. Return Values

Callable can return results.

```java
Callable<Integer> task = () -> {

    return 100;

};
```

---

## 2. Asynchronous Execution

The task can execute on a worker thread while the submitting thread continues.

---

## 3. Result Tracking

Future provides:

```java
future.get();
```

to retrieve the result.

---

## 4. Cancellation

Tasks can be cancelled:

```java
future.cancel(true);
```

---

## 5. Completion Checking

```java
future.isDone();
```

---

# 🔹 Disadvantages

## 1. get() Can Block

```java
future.get();
```

may wait until the task completes.

---

## 2. Difficult Composition

Traditional Future has limited support for composing asynchronous operations.

This is one reason Java provides:

```text
CompletableFuture
```

which will be covered in the next topic.

---

## 3. Manual Coordination

Managing many Futures can become complicated.

---

## 4. Cancellation Is Cooperative

Calling:

```java
cancel(true);
```

does not guarantee immediate termination of arbitrary code.

---

# 🔹 Common Mistakes

## ❌ Mistake 1 — Thinking Callable automatically runs asynchronously

Creating:

```java
Callable<Integer> task = () -> {

    return 10;

};
```

does not execute it.

You need an executor:

```java
Future<Integer> future =
        executor.submit(task);
```

---

## ❌ Mistake 2 — Thinking Future contains the result immediately

This:

```java
Future<Integer> future =
        executor.submit(task);
```

may happen before the task completes.

The Future represents the eventual result.

---

## ❌ Mistake 3 — Forgetting get() can block

```java
future.get();
```

may wait.

---

## ❌ Mistake 4 — Confusing cancellation with guaranteed termination

```java
future.cancel(true);
```

requests interruption.

It does not guarantee that arbitrary code immediately stops.

---

## ❌ Mistake 5 — Forgetting shutdown

Always consider the executor lifecycle:

```java
executor.shutdown();
```

---

# 🔹 Callable vs Runnable

| Callable | Runnable |
|---|---|
| `call()` | `run()` |
| Can return a value | Cannot return a value |
| Can throw checked exceptions | Cannot declare checked exceptions through `run()` |
| Used with `submit()` | Used with `execute()` or `submit()` |
| Returns generic type | Returns `void` |

Example:

```java
Callable<Integer> task = () -> {

    return 100;

};
```

versus:

```java
Runnable task = () -> {

    System.out.println("Hello");

};
```

---

# 🔹 Future vs Thread

| Future | Thread |
|---|---|
| Represents asynchronous computation/result | Represents execution thread |
| Can retrieve result | Does not directly represent a returned result |
| Can cancel task | Can be interrupted |
| Can check completion | Can check alive state |
| Usually obtained from ExecutorService | Created directly or managed by executor |

Think:

```text
Thread
→ Who executes?

Future
→ What happened to my submitted task/result?
```

---

# 🔹 Future vs CompletableFuture

`Future` provides basic asynchronous result handling.

```java
Future<Integer> future =
        executor.submit(task);
```

`CompletableFuture` provides richer asynchronous composition.

For example:

```text
Task 1
  ↓
then Task 2
  ↓
then Task 3
```

Future:

```text
Submit
 ↓
get()
```

CompletableFuture:

```text
Async Task
    ↓
thenApply()
    ↓
thenCompose()
    ↓
thenAccept()
```

`CompletableFuture` is covered in:

```text
14-CompletableFuture.md
```

---

# 🔹 DSA / Problem-Solving Relevance

Callable and Future are useful when independent computations can be executed concurrently.

For example:

```text
Large Array
    |
    +------ Part 1 → Callable
    |
    +------ Part 2 → Callable
    |
    +------ Part 3 → Callable
```

Each task can return a result.

Example:

```java
Callable<Integer> task = () -> {

    int sum = 0;

    for(int i = 0; i < 100; i++) {

        sum += i;
    }

    return sum;
};
```

Then:

```java
Future<Integer> future =
        executor.submit(task);
```

And:

```java
int result = future.get();
```

This concept becomes useful when learning:

```text
Parallel processing
Concurrent algorithms
Task decomposition
Producer/consumer systems
Concurrent data processing
```

---

# 🔹 Problem-Solving Mindset

When you see a problem that can be divided into independent tasks, ask:

```text
Can the tasks execute independently?
            ↓
          Yes
            ↓
Can each task return a result?
            ↓
          Yes
            ↓
Callable
            ↓
submit()
            ↓
Future
            ↓
get()
```

But also ask:

```text
Will get() block?
How many worker threads?
Do tasks share mutable state?
How are failures handled?
What happens if a task is cancelled?
```

---

# 🔹 30-Second Interview Answer

> `Callable` is a functional interface used to represent a task that can return a result and throw checked exceptions. When a Callable is submitted to an `ExecutorService`, it returns a `Future`. The Future acts as a handle to the asynchronous computation and provides methods such as `get()`, `isDone()`, `isCancelled()`, and `cancel()`. `get()` can block until the computation completes. For more advanced asynchronous composition, Java provides `CompletableFuture`.

---

# 🔹 Cheat Sheet

## Callable

```java
Callable<Integer> task = () -> {

    return 100;

};
```

---

## Submit Callable

```java
Future<Integer> future =
        executor.submit(task);
```

---

## Get Result

```java
Integer result =
        future.get();
```

---

## Check Completion

```java
future.isDone();
```

---

## Check Cancellation

```java
future.isCancelled();
```

---

## Cancel

```java
future.cancel(true);
```

---

## Timed get

```java
future.get(
        5,
        TimeUnit.SECONDS
);
```

---

## Runnable + Future

```java
Future<?> future =
        executor.submit(() -> {

            System.out.println("Task");

        });
```

---

# 🧠 Memory Tricks

### Callable

> **"Callable can Call and return."**

```text
call()
 ↓
return value
```

---

### Runnable

> **"Runnable runs; Callable calculates and returns."**

---

### Future

> **"Future = handle to the result that may arrive later."**

---

### `get()`

> **"Get the result — and possibly wait."**

---

### `isDone()`

> **"Has it finished?"**

---

### `isCancelled()`

> **"Was it cancelled?"**

---

### `cancel()`

> **"Request cancellation."**

---

# 🔥 Top 10 Interview Questions

## 1. What is Callable?

`Callable` is a functional interface representing a task that can return a result and throw checked exceptions.

---

## 2. What is Future?

`Future` represents the result of an asynchronous computation and provides methods to retrieve, inspect, or cancel that computation.

---

## 3. Difference between Runnable and Callable?

```text
Runnable
→ run()
→ no return value

Callable
→ call()
→ returns value
→ can throw checked exceptions
```

---

## 4. What does submit() return for Callable?

```java
Future<V>
```

where `V` is the result type of the Callable.

---

## 5. Is Future.get() blocking?

Yes, it can block until the computation completes.

---

## 6. How can you check whether a Future is complete?

Use:

```java
future.isDone();
```

---

## 7. How can you cancel a Future?

Use:

```java
future.cancel(true);
```

or:

```java
future.cancel(false);
```

depending on whether interruption should be requested for a running task.

---

## 8. What happens if Callable throws an exception?

The task completes exceptionally, and calling:

```java
future.get();
```

typically throws:

```java
ExecutionException
```

whose cause is the original exception.

---

## 9. What happens if you call get() after cancellation?

A successful cancellation causes:

```java
future.get();
```

to throw:

```java
CancellationException
```

---

## 10. Why use CompletableFuture instead of Future?

`CompletableFuture` provides richer asynchronous composition and non-blocking continuation APIs, such as:

```java
thenApply()
thenCompose()
thenAccept()
```

---

# 🎯 Final Summary

```text
                  Callable
                     |
                     | submit()
                     ↓
              ExecutorService
                     |
                     ↓
                Thread Pool
                     |
                     ↓
                Worker Thread
                     |
                     ↓
              Callable executes
                     |
                     ↓
                  Result
                     |
                     ↓
                  Future
                     |
          +----------+----------+
          |          |          |
        get()     isDone()   cancel()
          |
          ↓
       Result
```

### ⭐ Core Difference

```text
Runnable
   ↓
run()
   ↓
No result

Callable
   ↓
call()
   ↓
Returns result
```

### ⭐ Future

```text
Future
  ↓
Represents asynchronous result
  ↓
get()
isDone()
isCancelled()
cancel()
```

### ⭐ Most Important Concept

```java
Future<Integer> future =
        executor.submit(() -> {

            return 10 + 20;

        });

int result = future.get();
```

Flow:

```text
submit task
    ↓
task runs asynchronously
    ↓
Future represents its result
    ↓
get()
    ↓
result = 30
```

### ⭐ One-Line Interview Memory

> **Callable produces a result, Future represents that result, and ExecutorService manages the execution.**