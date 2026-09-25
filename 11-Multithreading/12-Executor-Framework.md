# 12 — Executor Framework

> **The Executor Framework provides a higher-level way to create, manage, and execute threads without manually creating and managing every thread.**

---

## 📌 Table of Contents

1. [Introduction](#-introduction)
2. [Why Executor Framework?](#-why-executor-framework)
3. [Thread Creation vs Executor Framework](#-thread-creation-vs-executor-framework)
4. [Executor Framework Architecture](#-executor-framework-architecture)
5. [Executor](#-executor)
6. [ExecutorService](#-executorservice)
7. [Executors Class](#-executors-class)
8. [Fixed Thread Pool](#-fixed-thread-pool)
9. [Cached Thread Pool](#-cached-thread-pool)
10. [Single Thread Executor](#-single-thread-executor)
11. [Scheduled Thread Pool](#-scheduled-thread-pool)
12. [execute()](#-execute)
13. [submit()](#-submit)
14. [shutdown()](#-shutdown)
15. [shutdownNow()](#-shutdownnow)
16. [isShutdown()](#-isshutdown)
17. [isTerminated()](#-isterminated)
18. [awaitTermination()](#-awaittermination)
19. [Runnable with ExecutorService](#-runnable-with-executorservice)
20. [Callable and Future Preview](#-callable-and-future-preview)
21. [Thread Pool](#-thread-pool)
22. [Task vs Thread](#-task-vs-thread)
23. [How ExecutorService Works Internally](#-how-executorservice-works-internally)
24. [ExecutorService Lifecycle](#-executorservice-lifecycle)
25. [Advantages](#-advantages)
26. [Disadvantages](#-disadvantages)
27. [Common Mistakes](#-common-mistakes)
28. [Executor vs ExecutorService](#-executor-vs-executorservice)
29. [ExecutorService vs Thread](#-executorservice-vs-thread)
30. [DSA / Problem-Solving Relevance](#-dsa--problem-solving-relevance)
31. [30-Second Interview Answer](#-30-second-interview-answer)
32. [Cheat Sheet](#-cheat-sheet)
33. [Top 10 Interview Questions](#-top-10-interview-questions)

---

# 🔹 Introduction

Earlier, we learned how to create threads manually:

```java
Thread thread = new Thread(() -> {

    System.out.println("Task running");

});

thread.start();
```

This approach is fine for learning and small programs.

But in real applications, we may have many tasks.

Creating a new thread for every task can become difficult to manage.

Java provides the:

```text
Executor Framework
```

to separate:

```text
Task submission
```

from:

```text
Thread management
```

The main classes and interfaces are in:

```java
java.util.concurrent
```

---

# 🔹 Why Executor Framework?

Suppose we have 100 tasks.

A naive approach would be:

```java
for(int i = 0; i < 100; i++) {

    Thread thread = new Thread(() -> {

        // task

    });

    thread.start();
}
```

This can create many threads.

Too many threads can cause:

```text
Memory overhead
       ↓
Context switching
       ↓
Resource consumption
       ↓
Difficult lifecycle management
```

Instead, we can create a pool of worker threads:

```text
              100 Tasks
                  |
                  ↓
          ExecutorService
                  |
        +---------+---------+
        |         |         |
       T1        T2        T3
        |         |         |
      Task      Task      Task
```

Only a controlled number of threads execute tasks.

---

# 🔹 Thread Creation vs Executor Framework

## Manual Thread

```java
Thread thread = new Thread(() -> {

    System.out.println("Task");

});

thread.start();
```

Here, you directly create and start the thread.

---

## Executor Framework

```java
ExecutorService executor =
        Executors.newFixedThreadPool(3);

executor.execute(() -> {

    System.out.println("Task");

});
```

Here:

```text
You provide the task
        ↓
Executor manages execution
```

This is one of the biggest ideas behind the Executor Framework.

---

# 🔹 Executor Framework Architecture

The basic architecture is:

```text
              Tasks
                |
                ↓
          ExecutorService
                |
                ↓
           Task Queue
                |
       +--------+--------+
       |        |        |
      T1       T2       T3
       |        |        |
     Worker   Worker   Worker
```

The important components are:

```text
Executor
   ↓
ExecutorService
   ↓
Thread Pool
   ↓
Worker Threads
   ↓
Tasks
```

---

# 🔹 Executor

`Executor` is the most basic interface in the Executor Framework.

Package:

```java
java.util.concurrent.Executor
```

It provides one main method:

```java
void execute(Runnable command);
```

Example:

```java
Executor executor = command -> {

    new Thread(command).start();

};

executor.execute(() -> {

    System.out.println("Task running");

});
```

The important concept is:

> `Executor` provides a way to submit a task for execution without requiring the caller to manage exactly how the task is executed.

---

# 🔹 ExecutorService

`ExecutorService` extends `Executor`.

It provides additional functionality such as:

```text
Task submission
Shutdown
Lifecycle management
Future handling
```

Example:

```java
ExecutorService executor =
        Executors.newFixedThreadPool(3);
```

Then:

```java
executor.execute(() -> {

    System.out.println("Task running");

});
```

Finally:

```java
executor.shutdown();
```

---

# 🔹 ExecutorService Hierarchy

Conceptually:

```text
Executor
   ↑
ExecutorService
   ↑
ScheduledExecutorService
```

`ExecutorService` provides more functionality than `Executor`.

---

# 🔹 Executors Class

`Executors` is a utility class that provides factory methods for creating common executor configurations.

Package:

```java
java.util.concurrent.Executors
```

Common methods include:

```java
Executors.newFixedThreadPool()
Executors.newCachedThreadPool()
Executors.newSingleThreadExecutor()
Executors.newScheduledThreadPool()
```

Example:

```java
ExecutorService executor =
        Executors.newFixedThreadPool(3);
```

---

# 🔹 Fixed Thread Pool

A fixed thread pool contains a fixed number of worker threads.

Example:

```java
ExecutorService executor =
        Executors.newFixedThreadPool(3);
```

This creates an executor configured with:

```text
3 worker threads
```

Suppose we submit 6 tasks:

```java
for(int i = 1; i <= 6; i++) {

    final int taskNumber = i;

    executor.execute(() -> {

        System.out.println(
                "Task " + taskNumber +
                " executed by " +
                Thread.currentThread().getName()
        );

    });
}
```

Conceptually:

```text
Task 1 ──→ Thread 1
Task 2 ──→ Thread 2
Task 3 ──→ Thread 3

Task 4 ──→ waits / queued
Task 5 ──→ waits / queued
Task 6 ──→ waits / queued
```

As worker threads become available, queued tasks can execute.

---

# 🔹 Why Fixed Thread Pool?

Useful when you want to control the number of concurrently executing tasks.

Example:

```java
ExecutorService executor =
        Executors.newFixedThreadPool(5);
```

Meaning:

```text
At most 5 worker threads
```

can execute tasks concurrently under normal operation.

---

# 🔹 Cached Thread Pool

Created using:

```java
ExecutorService executor =
        Executors.newCachedThreadPool();
```

It is designed for workloads with many short-lived asynchronous tasks.

It can create new threads when necessary and reuse previously created idle threads when possible.

Example:

```java
ExecutorService executor =
        Executors.newCachedThreadPool();

for(int i = 1; i <= 5; i++) {

    final int task = i;

    executor.execute(() -> {

        System.out.println(
                "Task " + task
        );

    });
}
```

Important:

> A cached thread pool does not impose a fixed maximum number of worker threads in the way a fixed thread pool does.

Therefore, it should be chosen carefully for workloads that may generate large numbers of concurrent tasks.

---

# 🔹 Single Thread Executor

Created using:

```java
ExecutorService executor =
        Executors.newSingleThreadExecutor();
```

It uses one worker thread.

Example:

```java
executor.execute(() -> {

    System.out.println("Task 1");

});

executor.execute(() -> {

    System.out.println("Task 2");

});

executor.execute(() -> {

    System.out.println("Task 3");

});
```

Conceptually:

```text
Task 1
  ↓
Thread 1
  ↓
Task 2
  ↓
Thread 1
  ↓
Task 3
```

Tasks are executed sequentially by the single worker.

---

# 🔹 Scheduled Thread Pool

Created using:

```java
ScheduledExecutorService executor =
        Executors.newScheduledThreadPool(2);
```

It is useful for delayed or periodic tasks.

Example:

```java
executor.schedule(() -> {

    System.out.println("Executed after delay");

}, 3, TimeUnit.SECONDS);
```

This task executes after approximately:

```text
3 seconds
```

---

# 🔹 execute()

`execute()` is defined by `Executor`.

Syntax:

```java
executor.execute(Runnable);
```

Example:

```java
ExecutorService executor =
        Executors.newFixedThreadPool(2);

executor.execute(() -> {

    System.out.println("Hello from executor");

});

executor.shutdown();
```

Important:

```text
execute()
   ↓
Runnable
   ↓
No Future returned
```

---

# 🔹 submit()

`submit()` is provided by `ExecutorService`.

It can accept:

```text
Runnable
Callable
```

and returns a:

```java
Future
```

Example:

```java
ExecutorService executor =
        Executors.newFixedThreadPool(2);

Future<?> future = executor.submit(() -> {

    System.out.println("Task running");

});

executor.shutdown();
```

The `Future` can be used to track the submitted task.

---

# 🔹 execute() vs submit()

| `execute()` | `submit()` |
|---|---|
| Defined in Executor | Defined in ExecutorService |
| Accepts Runnable | Accepts Runnable and Callable |
| Returns nothing | Returns Future |
| Mainly fire-and-forget | Can track result/completion |
| Exceptions are handled differently | Exceptions can be retrieved through Future |

Example:

```java
executor.execute(() -> {

    System.out.println("Task");

});
```

versus:

```java
Future<?> future = executor.submit(() -> {

    System.out.println("Task");

});
```

---

# 🔹 shutdown()

After finishing task submission, the executor should be shut down.

Example:

```java
ExecutorService executor =
        Executors.newFixedThreadPool(3);

executor.execute(() -> {

    System.out.println("Task");

});

executor.shutdown();
```

`shutdown()` means:

```text
Stop accepting new tasks
        +
Allow already submitted tasks to finish
```

It does not normally interrupt tasks that are already running.

---

# 🔹 shutdownNow()

`shutdownNow()` attempts to stop the executor more aggressively.

Example:

```java
executor.shutdownNow();
```

Conceptually:

```text
Stop accepting new tasks
        ↓
Attempt to interrupt running tasks
        ↓
Return tasks that never started
```

Important:

> `shutdownNow()` does not guarantee that every running task immediately stops. It relies on interruption and on tasks responding appropriately to interruption.

---

# 🔹 shutdown() vs shutdownNow()

| `shutdown()` | `shutdownNow()` |
|---|---|
| Graceful shutdown | Immediate/forceful attempt |
| No new tasks accepted | No new tasks accepted |
| Existing tasks are allowed to complete | Attempts to interrupt running tasks |
| Preferred for normal shutdown | Used when more immediate termination is needed |

---

# 🔹 isShutdown()

Checks whether shutdown has been initiated.

Example:

```java
ExecutorService executor =
        Executors.newFixedThreadPool(2);

System.out.println(executor.isShutdown());

executor.shutdown();

System.out.println(executor.isShutdown());
```

Output:

```text
false
true
```

Important:

```text
isShutdown()
```

does not mean all tasks have finished.

It only tells us that shutdown has been initiated.

---

# 🔹 isTerminated()

Checks whether the executor has completely terminated.

Example:

```java
executor.shutdown();

System.out.println(executor.isTerminated());
```

Immediately after shutdown, it may still be:

```text
false
```

because tasks may still be running.

After all tasks finish:

```text
true
```

---

# 🔹 isShutdown() vs isTerminated()

```text
shutdown()
    ↓
isShutdown() = true
    ↓
Tasks continue finishing
    ↓
All tasks finish
    ↓
isTerminated() = true
```

Therefore:

```text
isShutdown()
→ shutdown started

isTerminated()
→ shutdown completed
```

---

# 🔹 awaitTermination()

`awaitTermination()` allows the current thread to wait for executor termination for a specified amount of time.

Example:

```java
executor.shutdown();

try {

    if(executor.awaitTermination(
            5,
            TimeUnit.SECONDS)) {

        System.out.println("All tasks completed");

    } else {

        System.out.println("Timeout");

    }

} catch(InterruptedException e) {

    Thread.currentThread().interrupt();
}
```

It returns:

```text
true
→ executor terminated within the timeout

false
→ timeout elapsed before termination
```

---

# 🔹 Runnable with ExecutorService

Example:

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class Main {

    public static void main(String[] args) {

        ExecutorService executor =
                Executors.newFixedThreadPool(2);

        Runnable task = () -> {

            System.out.println(
                    "Running on: " +
                    Thread.currentThread().getName()
            );

        };

        executor.execute(task);
        executor.execute(task);
        executor.execute(task);

        executor.shutdown();
    }
}
```

Here:

```text
Main Thread
     |
     ↓
ExecutorService
     |
     ↓
Thread Pool
   /   \
 T1     T2
```

---

# 🔹 Callable and Future Preview

`ExecutorService` can also execute `Callable`.

`Callable` differs from `Runnable` because it can:

```text
Return a result
+
Throw checked exceptions
```

Example:

```java
ExecutorService executor =
        Executors.newFixedThreadPool(2);

Future<Integer> future =
        executor.submit(() -> {

            return 10 + 20;

        });

executor.shutdown();
```

Later:

```java
System.out.println(future.get());
```

Output:

```text
30
```

`Callable` and `Future` will be covered in depth in:

```text
13-Callable-and-Future.md
```

---

# 🔹 Thread Pool

A thread pool is a collection of reusable worker threads.

Instead of:

```text
Task
 ↓
Create Thread
 ↓
Execute
 ↓
Destroy Thread
```

we can have:

```text
Thread Pool
 ├── Worker 1
 ├── Worker 2
 └── Worker 3

Tasks
 ├── Task 1
 ├── Task 2
 ├── Task 3
 ├── Task 4
 └── Task 5
```

Workers repeatedly take tasks and execute them.

---

# 🔹 Why Reuse Threads?

Creating threads has overhead.

Instead of repeatedly doing:

```text
Create
 ↓
Run
 ↓
Destroy
```

a thread pool can do:

```text
Create worker
      ↓
Execute task
      ↓
Wait for another task
      ↓
Execute another task
      ↓
...
```

This can reduce thread-creation overhead and make concurrency easier to control.

---

# 🔹 Task vs Thread

This distinction is extremely important.

### Task

A task represents:

> **What should be done?**

Example:

```java
Runnable task = () -> {

    System.out.println("Processing order");

};
```

### Thread

A thread represents:

> **Who performs the task?**

The Executor Framework allows you to focus primarily on:

```text
Task
```

while the framework handles:

```text
Thread management
```

---

# 🔹 How ExecutorService Works Internally

A simplified model of a typical thread-pool executor is:

```text
             submit task
                  |
                  ↓
          ExecutorService
                  |
                  ↓
             Task Queue
                  |
        +---------+---------+
        |         |         |
        ↓         ↓         ↓
     Worker 1  Worker 2  Worker 3
        |         |         |
        ↓         ↓         ↓
      Task      Task      Task
```

A worker thread generally:

```text
Take task
   ↓
Execute task
   ↓
Take another task
   ↓
Execute
   ↓
...
```

The exact behavior depends on the executor implementation and configuration.

---

# 🔹 Task Queue

A thread pool may maintain a queue of tasks waiting for available workers.

For example:

```text
Worker 1 → Running Task 1
Worker 2 → Running Task 2

Queue:
Task 3
Task 4
Task 5
Task 6
```

When a worker becomes available:

```text
Worker 1
   ↓
takes Task 3
```

Then:

```text
Queue:
Task 4
Task 5
Task 6
```

---

# 🔹 ExecutorService Lifecycle

A simplified lifecycle is:

```text
             Created
                |
                ↓
        Accepting Tasks
                |
                ↓
        Running Tasks
                |
                ↓
           shutdown()
                |
                ↓
      No New Tasks Accepted
                |
                ↓
       Existing Tasks Finish
                |
                ↓
           Terminated
```

---

# 🔹 Advantages

## 1. Thread Reuse

Worker threads can execute multiple tasks.

---

## 2. Resource Management

The number of concurrent worker threads can be controlled.

---

## 3. Cleaner Code

You submit tasks instead of manually managing every thread.

---

## 4. Task Queuing

Tasks can wait until worker threads become available.

---

## 5. Lifecycle Management

Methods such as:

```java
shutdown()
shutdownNow()
awaitTermination()
```

help manage the executor lifecycle.

---

## 6. Result Handling

`submit()` works with:

```java
Future
```

which allows task results and completion to be handled.

---

# 🔹 Disadvantages

## 1. Incorrect Pool Size

An inappropriate pool configuration can hurt performance.

---

## 2. Resource Leaks

Forgetting to shut down an executor can leave threads alive longer than intended.

---

## 3. Queue Growth

If tasks arrive faster than they can be processed, the task queue may grow depending on the executor configuration.

---

## 4. Concurrency Bugs Still Exist

ExecutorService does not automatically make your shared data thread-safe.

For example:

```java
int count = 0;
```

can still suffer from race conditions if multiple executor tasks modify it unsafely.

---

# 🔹 Common Mistakes

## ❌ Mistake 1 — Forgetting shutdown()

Bad:

```java
ExecutorService executor =
        Executors.newFixedThreadPool(3);

executor.execute(() -> {

    System.out.println("Task");

});
```

Better:

```java
ExecutorService executor =
        Executors.newFixedThreadPool(3);

executor.execute(() -> {

    System.out.println("Task");

});

executor.shutdown();
```

---

## ❌ Mistake 2 — Thinking shutdown() kills running tasks

Wrong.

```java
executor.shutdown();
```

generally means:

```text
No new tasks
+
allow already submitted tasks to complete
```

---

## ❌ Mistake 3 — Thinking shutdownNow() guarantees immediate stopping

It does not.

It attempts to interrupt running tasks.

Tasks must respond properly to interruption.

---

## ❌ Mistake 4 — Confusing execute() and submit()

```text
execute()
→ no Future

submit()
→ Future
```

---

## ❌ Mistake 5 — Thinking ExecutorService makes shared data safe

Example:

```java
int count = 0;
```

If multiple tasks execute:

```java
count++;
```

you can still have a race condition.

The executor manages task execution, not your application's shared-state correctness.

---

# 🔹 Executor vs ExecutorService

| Executor | ExecutorService |
|---|---|
| Basic abstraction | Extended abstraction |
| `execute()` | `execute()` |
| Simple task execution | Task submission |
| No lifecycle shutdown methods | Shutdown methods |
| No Future support | Future support |
| Smaller API | Richer API |

Hierarchy:

```text
Executor
   ↑
ExecutorService
```

---

# 🔹 ExecutorService vs Thread

| Thread | ExecutorService |
|---|---|
| Represents a thread | Manages task execution |
| Manually create threads | Uses managed worker threads |
| One Thread object per thread | Can manage a pool |
| Manual lifecycle | Executor lifecycle APIs |
| Suitable for simple direct threading | Suitable for multiple asynchronous tasks |

Manual:

```java
Thread thread = new Thread(() -> {

    System.out.println("Task");

});

thread.start();
```

Executor:

```java
ExecutorService executor =
        Executors.newFixedThreadPool(2);

executor.execute(() -> {

    System.out.println("Task");

});

executor.shutdown();
```

---

# 🔹 DSA / Problem-Solving Relevance

Executor Framework is not a traditional DSA pattern, but it is important when DSA algorithms are executed concurrently.

### Example: Parallel Tasks

Suppose we have multiple independent operations:

```text
Task 1 → Process array section 1
Task 2 → Process array section 2
Task 3 → Process array section 3
```

An executor can manage the worker threads:

```text
              Executor
                 |
       +---------+---------+
       |         |         |
      T1        T2        T3
       |         |         |
    Part 1    Part 2    Part 3
```

---

### Shared Counter

If multiple tasks update:

```java
int count;
```

you still need proper synchronization.

For example:

```java
AtomicInteger count =
        new AtomicInteger();

executor.execute(() -> {

    count.incrementAndGet();

});
```

---

### Problem-Solving Question

Whenever using an executor, ask:

```text
Are tasks independent?
        ↓
Can they safely execute concurrently?
        ↓
Do they share mutable state?
        ↓
If yes, how is that state protected?
```

This is more important than simply creating a thread pool.

---

# 🔹 30-Second Interview Answer

> The Executor Framework is a Java concurrency framework used to manage and execute tasks using managed threads. Instead of creating a new thread for every task, we can use an `ExecutorService` with a thread pool. Tasks can be submitted using `execute()` or `submit()`. `submit()` can return a `Future` for tracking the task or obtaining its result. After submitting tasks, the executor should normally be shut down using `shutdown()`.

---

# 🔹 Cheat Sheet

## Create ExecutorService

```java
ExecutorService executor =
        Executors.newFixedThreadPool(3);
```

---

## Execute Runnable

```java
executor.execute(() -> {

    System.out.println("Task");

});
```

---

## Submit Task

```java
Future<?> future =
        executor.submit(() -> {

            System.out.println("Task");

        });
```

---

## Shutdown

```java
executor.shutdown();
```

---

## Immediate Shutdown Attempt

```java
executor.shutdownNow();
```

---

## Check Shutdown

```java
executor.isShutdown();
```

---

## Check Termination

```java
executor.isTerminated();
```

---

## Wait for Termination

```java
executor.awaitTermination(
        5,
        TimeUnit.SECONDS
);
```

---

## Fixed Pool

```java
Executors.newFixedThreadPool(5);
```

---

## Cached Pool

```java
Executors.newCachedThreadPool();
```

---

## Single Thread

```java
Executors.newSingleThreadExecutor();
```

---

## Scheduled Pool

```java
Executors.newScheduledThreadPool(2);
```

---

# 🧠 Memory Tricks

### Executor

> **"Give me the task; I'll decide how it runs."**

---

### ExecutorService

> **"Executor + task management + lifecycle."**

---

### Fixed Thread Pool

```text
Fixed
 ↓
Fixed number of workers
```

---

### Single Thread Executor

```text
1 worker
 ↓
Tasks execute sequentially
```

---

### execute vs submit

```text
execute()
   ↓
Runnable
   ↓
No Future

submit()
   ↓
Runnable / Callable
   ↓
Future
```

---

### shutdown vs shutdownNow

```text
shutdown()
→ Finish submitted work

shutdownNow()
→ Attempt to interrupt running work
```

---

# 🔥 Top 10 Interview Questions

## 1. What is the Executor Framework?

It is a Java concurrency framework for managing and executing tasks using executors and thread pools instead of manually creating and managing every thread.

---

## 2. What is Executor?

`Executor` is an interface that provides the basic task-execution abstraction:

```java
void execute(Runnable command);
```

---

## 3. What is ExecutorService?

`ExecutorService` extends `Executor` and provides additional capabilities such as:

```text
Task submission
Future support
Shutdown
Lifecycle management
```

---

## 4. What is a thread pool?

A thread pool is a collection of worker threads that can execute multiple submitted tasks over their lifetime.

---

## 5. Difference between execute() and submit()?

```text
execute()
→ accepts Runnable
→ returns nothing

submit()
→ accepts Runnable / Callable
→ returns Future
```

---

## 6. What does shutdown() do?

It stops accepting new tasks while allowing previously submitted tasks to complete.

---

## 7. What does shutdownNow() do?

It attempts to stop the executor more immediately by interrupting running tasks and returning tasks that were waiting to start.

---

## 8. Difference between isShutdown() and isTerminated()?

```text
isShutdown()
→ shutdown has been initiated

isTerminated()
→ executor has completely terminated
```

---

## 9. Why use a thread pool?

Thread pools provide:

```text
Thread reuse
Resource control
Task queuing
Lifecycle management
```

and avoid creating a new thread for every task.

---

## 10. Does ExecutorService make shared data thread-safe?

No.

For example:

```java
int count = 0;
```

can still have race conditions if multiple executor tasks modify it concurrently.

You still need appropriate synchronization or atomic/concurrent mechanisms.

---

# 🎯 Final Summary

```text
                    Executor Framework
                            |
              +-------------+-------------+
              |                           |
           Executor                ExecutorService
              |                           |
          execute()            +----------+----------+
                               |          |          |
                            submit()   shutdown()   Future
                               |
                               ↓
                           Thread Pool
                               |
                    +----------+----------+
                    |          |          |
                  Worker     Worker     Worker
                    |          |          |
                   Task       Task       Task
```

### ⭐ Core Idea

Instead of:

```java
new Thread(task).start();
```

for every task:

```java
ExecutorService executor =
        Executors.newFixedThreadPool(3);

executor.execute(task);

executor.shutdown();
```

---

### ⭐ Most Important Methods

```text
execute()
submit()
shutdown()
shutdownNow()
isShutdown()
isTerminated()
awaitTermination()
```

---

### ⭐ Most Important Executors

```text
newFixedThreadPool()
newCachedThreadPool()
newSingleThreadExecutor()
newScheduledThreadPool()
```

---

### ⭐ One-Line Interview Memory

> **Executor Framework separates task submission from thread management and provides thread pools, task queuing, result handling, and executor lifecycle management.**