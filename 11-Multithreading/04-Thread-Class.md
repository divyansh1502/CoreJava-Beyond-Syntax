# 04 — Thread Class

> **`Thread` is a Java class that represents a thread of execution and provides methods for creating, starting, inspecting, and controlling threads.**

---

## 📌 Table of Contents

1. [Introduction](#-introduction)
2. [Package](#-package)
3. [Thread Class Hierarchy](#-thread-class-hierarchy)
4. [Creating a Thread Object](#-creating-a-thread-object)
5. [Thread Constructors](#-thread-constructors)
6. [Thread with Runnable](#-thread-with-runnable)
7. [Important Thread Methods](#-important-thread-methods)
8. [start()](#-start)
9. [run()](#-run)
10. [currentThread()](#-currentthread)
11. [getName()](#-getname)
12. [setName()](#-setname)
13. [getId()](#-getid)
14. [getPriority()](#-getpriority)
15. [setPriority()](#-setpriority)
16. [getState()](#-getstate)
17. [isAlive()](#-isalive)
18. [interrupt()](#-interrupt)
19. [isInterrupted()](#-isinterrupted)
20. [sleep()](#-sleep)
21. [join()](#-join)
22. [Thread Priority](#-thread-priority)
23. [Daemon Threads](#-daemon-threads)
24. [setDaemon()](#-setdaemon)
25. [Multiple Threads](#-multiple-threads)
26. [Thread Naming](#-thread-naming)
27. [Internal Working](#-internal-working)
28. [Thread Lifecycle Connection](#-thread-lifecycle-connection)
29. [Common Mistakes](#-common-mistakes)
30. [Interview Traps](#-interview-traps)
31. [DSA / Problem-Solving Relevance](#-dsa--problem-solving-relevance)
32. [30-Second Interview Answer](#-30-second-interview-answer)
33. [Cheat Sheet](#-cheat-sheet)
34. [Top 10 Interview Questions](#-top-10-interview-questions)

---

# 🔹 Introduction

`Thread` is a class in Java used to represent a thread of execution.

It provides functionality for:

- Creating threads
- Starting threads
- Naming threads
- Checking thread state
- Checking whether a thread is alive
- Setting thread priority
- Interrupting threads
- Waiting for another thread
- Working with daemon threads

Example:

    Thread thread = new Thread();

Creating the object does **not** start the thread.

We need:

    thread.start();

to start it.

---

# 🔹 Package

`Thread` belongs to:

    java.lang

`java.lang` is automatically imported by Java.

Therefore, we do not need:

    import java.lang.Thread;

We can directly write:

    Thread thread = new Thread();

---

# 🔹 Thread Class Hierarchy

The simplified hierarchy is:

    Object
       ↓
    Thread

`Thread` extends `Object`.

`Thread` also implements `Runnable`.

Conceptually:

    Object
       ↓
    Thread
       ↘
       Runnable

The important idea is:

- `Runnable` represents a task.
- `Thread` represents the execution mechanism.

---

# 🔹 Creating a Thread Object

The simplest form is:

    Thread thread = new Thread();

At this point, the thread object exists but has not started.

Its state is:

    NEW

Example:

    class Main {

        public static void main(String[] args) {

            Thread thread = new Thread();

            System.out.println(thread.getState());
        }
    }

Output:

    NEW

---

# 🔹 Thread Constructors

The `Thread` class provides several constructors.

## 1. Thread()

Creates a thread without a target task.

    Thread thread = new Thread();

---

## 2. Thread(Runnable target)

Creates a thread associated with a `Runnable`.

    Runnable task = () -> {
        System.out.println("Task is running");
    };

    Thread thread = new Thread(task);

---

## 3. Thread(String name)

Creates a thread with a specified name.

    Thread thread = new Thread("Worker-1");

---

## 4. Thread(Runnable target, String name)

Creates a thread with both a task and a name.

    Runnable task = () -> {
        System.out.println("Task is running");
    };

    Thread thread = new Thread(task, "Worker-1");

---

# 🔹 Thread with Runnable

A common pattern is:

    Runnable task = () -> {
        System.out.println("Task is running");
    };

    Thread thread = new Thread(task);

    thread.start();

The relationship is:

    Runnable
       ↓
    Task
       ↓
    Thread
       ↓
    start()
       ↓
    Execution

---

# 🔹 Important Thread Methods

| Method | Purpose |
|---|---|
| `start()` | Starts a new thread of execution |
| `run()` | Contains the task logic |
| `currentThread()` | Returns the currently executing thread |
| `getName()` | Returns thread name |
| `setName()` | Changes thread name |
| `getId()` | Returns thread ID |
| `getPriority()` | Returns thread priority |
| `setPriority()` | Changes thread priority |
| `getState()` | Returns current thread state |
| `isAlive()` | Checks whether thread is alive |
| `interrupt()` | Requests interruption |
| `isInterrupted()` | Checks interruption status |
| `sleep()` | Makes current thread sleep |
| `join()` | Waits for another thread to finish |
| `setDaemon()` | Marks a thread as daemon |
| `isDaemon()` | Checks whether thread is daemon |

---

# 🔹 `start()`

`start()` starts a new thread of execution.

Example:

    class MyThread extends Thread {

        @Override
        public void run() {
            System.out.println("Worker thread");
        }
    }

    class Main {

        public static void main(String[] args) {

            MyThread thread = new MyThread();

            thread.start();
        }
    }

Important:

    thread.start();

is different from:

    thread.run();

`start()` causes the JVM to arrange for the thread to execute independently.

---

# 🔹 `run()`

`run()` contains the work that the thread performs.

Example:

    class MyThread extends Thread {

        @Override
        public void run() {
            System.out.println("Doing work");
        }
    }

The `run()` method can also be called directly.

But:

    thread.run();

does **not** create a new thread.

It behaves like a normal method call.

---

# 🔹 `start()` vs `run()`

| `start()` | `run()` |
|---|---|
| Starts a new thread | Normal method call |
| Creates a separate execution path | Executes on current thread |
| JVM handles thread scheduling | No new thread is created |
| Can be called only once on a thread object | Can be directly called multiple times |

Memory trick:

    start() → START a new thread

    run() → RUN the task

---

# 🔹 `currentThread()`

`currentThread()` is a static method of `Thread`.

It returns the thread that is currently executing the code.

Example:

    class Main {

        public static void main(String[] args) {

            Thread thread = Thread.currentThread();

            System.out.println(thread);
        }
    }

The main method normally executes inside the:

    main

thread.

---

# 🔹 `currentThread().getName()`

We can combine:

    Thread.currentThread()

with:

    getName()

Example:

    class Main {

        public static void main(String[] args) {

            System.out.println(
                Thread.currentThread().getName()
            );
        }
    }

Typical output:

    main

Here:

    currentThread()

returns the current `Thread` object.

Then:

    getName()

returns its name.

---

# 🔹 `getName()`

`getName()` returns the name of a thread.

Example:

    Thread thread = new Thread();

    System.out.println(thread.getName());

A newly created thread receives a default name if one is not provided.

---

# 🔹 `setName()`

`setName()` changes the thread name.

Example:

    Thread thread = new Thread();

    thread.setName("Worker-1");

    System.out.println(thread.getName());

Output:

    Worker-1

This is useful when debugging multithreaded applications.

---

# 🔹 Creating a Named Thread

Instead of setting the name separately:

    Thread thread = new Thread("Worker-1");

Then:

    System.out.println(thread.getName());

Output:

    Worker-1

---

# 🔹 `getId()`

`getId()` returns the identifier of a thread.

Example:

    Thread thread = new Thread();

    System.out.println(thread.getId());

The ID is assigned by the JVM and is useful for identifying threads.

Modern Java also provides:

    thread.threadId();

for obtaining the thread ID.

---

# 🔹 `getPriority()`

Every thread has a priority.

We can retrieve it using:

    getPriority()

Example:

    Thread thread = new Thread();

    System.out.println(thread.getPriority());

The default priority is normally:

    5

which corresponds to:

    Thread.NORM_PRIORITY

---

# 🔹 Thread Priority Constants

Java provides three commonly used priority constants:

    Thread.MIN_PRIORITY
    Thread.NORM_PRIORITY
    Thread.MAX_PRIORITY

Their values are:

    MIN_PRIORITY  = 1
    NORM_PRIORITY = 5
    MAX_PRIORITY  = 10

Example:

    System.out.println(Thread.MIN_PRIORITY);
    System.out.println(Thread.NORM_PRIORITY);
    System.out.println(Thread.MAX_PRIORITY);

Output:

    1
    5
    10

---

# 🔹 `setPriority()`

We can request a different thread priority.

Example:

    Thread thread = new Thread();

    thread.setPriority(Thread.MAX_PRIORITY);

    System.out.println(thread.getPriority());

Output:

    10

Important:

Thread priority is a scheduling hint.

It does not guarantee that a higher-priority thread will always execute first.

---

# 🔹 `getState()`

`getState()` returns the current state of a thread.

Example:

    Thread thread = new Thread();

    System.out.println(thread.getState());

Output:

    NEW

Possible Java thread states are:

    NEW
    RUNNABLE
    BLOCKED
    WAITING
    TIMED_WAITING
    TERMINATED

These states are represented by:

    Thread.State

---

# 🔹 `Thread.State`

`Thread.State` is an enum inside `Thread`.

Example:

    Thread.State state = thread.getState();

    System.out.println(state);

The possible values are:

    NEW
    RUNNABLE
    BLOCKED
    WAITING
    TIMED_WAITING
    TERMINATED

---

# 🔹 `isAlive()`

`isAlive()` checks whether a thread has been started and has not yet terminated.

Example:

    Thread thread = new Thread(() -> {
        System.out.println("Running");
    });

    System.out.println(thread.isAlive());

    thread.start();

    System.out.println(thread.isAlive());

The first result is:

    false

After `start()`, the thread may be alive while it is executing.

After completion:

    false

---

# 🔹 `interrupt()`

`interrupt()` is used to request that a thread be interrupted.

Example:

    Thread thread = new Thread(() -> {

        try {
            Thread.sleep(5000);
        } catch (InterruptedException e) {
            System.out.println("Thread interrupted");
        }
    });

    thread.start();

    thread.interrupt();

Important:

`interrupt()` does not forcibly kill a thread.

It is a cooperative mechanism for requesting interruption.

---

# 🔹 `isInterrupted()`

`isInterrupted()` checks the interruption status of a thread.

Example:

    Thread thread = new Thread(() -> {

        System.out.println(
            Thread.currentThread().isInterrupted()
        );
    });

    thread.start();

The method returns a boolean:

    true

or:

    false

Important distinction:

    isInterrupted()

checks the interruption status without clearing it.

---

# 🔹 `sleep()`

`sleep()` pauses the currently executing thread for a specified amount of time.

Example:

    try {
        Thread.sleep(1000);
    } catch (InterruptedException e) {
        System.out.println("Interrupted");
    }

The value is in milliseconds.

Therefore:

    1000 milliseconds = 1 second

Example:

    Thread.sleep(2000);

means approximately:

    2 seconds

Important:

`sleep()` is a static method.

It affects the currently executing thread.

---

# 🔹 `join()`

`join()` causes the current thread to wait for another thread to finish.

Example:

    Thread thread = new Thread(() -> {

        for (int i = 1; i <= 5; i++) {
            System.out.println(i);
        }
    });

    thread.start();

    try {
        thread.join();
    } catch (InterruptedException e) {
        System.out.println("Interrupted");
    }

    System.out.println("Main continues");

Conceptually:

    Main Thread
        |
        | start Worker
        ↓
    Worker Thread
        |
        | executes
        ↓
    finishes
        |
        ↓
    Main continues

Without `join()`, the main thread does not have to wait for the worker to finish.

---

# 🔹 Thread Priority

Thread priority is represented by an integer from:

    1 → 10

where:

    1 = minimum priority
    5 = normal priority
    10 = maximum priority

Example:

    Thread thread = new Thread();

    thread.setPriority(8);

    System.out.println(thread.getPriority());

Output:

    8

But priority should not be used as a guarantee of execution order.

---

# 🔹 Daemon Threads

A daemon thread is a background thread that does not normally keep the JVM alive after all user threads have finished.

Example:

    Thread thread = new Thread(() -> {

        while (true) {
            System.out.println("Background work");
        }
    });

    thread.setDaemon(true);

    thread.start();

The important method is:

    setDaemon(true)

---

# 🔹 `setDaemon()`

`setDaemon(true)` marks a thread as a daemon thread.

Example:

    Thread thread = new Thread(() -> {
        System.out.println("Background task");
    });

    thread.setDaemon(true);

    thread.start();

Important:

`setDaemon(true)` must be called before the thread is started.

Wrong:

    thread.start();

    thread.setDaemon(true);

This causes:

    IllegalThreadStateException

---

# 🔹 `isDaemon()`

We can check whether a thread is a daemon thread.

Example:

    Thread thread = new Thread();

    thread.setDaemon(true);

    System.out.println(thread.isDaemon());

Output:

    true

---

# 🔹 Multiple Threads

We can create multiple threads using the `Thread` class.

Example:

    Thread thread1 = new Thread(() -> {
        System.out.println("Thread 1");
    });

    Thread thread2 = new Thread(() -> {
        System.out.println("Thread 2");
    });

    Thread thread3 = new Thread(() -> {
        System.out.println("Thread 3");
    });

    thread1.start();
    thread2.start();
    thread3.start();

The output order is not guaranteed.

Possible output:

    Thread 1
    Thread 3
    Thread 2

Another execution might produce:

    Thread 2
    Thread 1
    Thread 3

Never assume a specific order without synchronization or coordination.

---

# 🔹 Thread Naming

Naming threads is extremely useful for debugging.

Example:

    Thread thread = new Thread(() -> {

        System.out.println(
            Thread.currentThread().getName()
        );

    }, "Database-Worker");

    thread.start();

Possible output:

    Database-Worker

---

# 🔹 Complete Example

    class Main {

        public static void main(String[] args) {

            Thread worker = new Thread(() -> {

                System.out.println(
                    "Running: "
                    + Thread.currentThread().getName()
                );

            }, "Worker-1");

            System.out.println("State: " + worker.getState());

            worker.start();

            System.out.println(
                "Name: " + worker.getName()
            );

            System.out.println(
                "Priority: " + worker.getPriority()
            );
        }
    }

Possible output:

    State: NEW
    Name: Worker-1
    Priority: 5
    Running: Worker-1

The exact order of the last lines can vary because thread scheduling is not deterministic.

---

# 🔹 Internal Working

Consider:

    Runnable task = () -> {
        System.out.println("Working");
    };

    Thread thread = new Thread(task);

    thread.start();

Conceptually:

    1. Runnable task is created
              ↓
    2. Thread object is created
              ↓
    3. Thread is in NEW state
              ↓
    4. start() is called
              ↓
    5. Thread becomes eligible for execution
              ↓
    6. Scheduler gives it CPU execution time
              ↓
    7. run() executes
              ↓
    8. Task completes
              ↓
    9. Thread becomes TERMINATED

---

# 🔹 Thread Object vs Actual Thread Execution

This distinction is important.

When we write:

    Thread thread = new Thread();

we create a Java object representing a thread.

But it has not started execution yet.

Only after:

    thread.start();

does the thread become eligible for execution.

Therefore:

    new Thread()
        ↓
    Thread object created

and:

    start()
        ↓
    execution begins

are two different operations.

---

# 🔹 Thread Lifecycle Connection

The `Thread` class directly connects to the thread lifecycle.

Example:

    Thread thread = new Thread();

Initial state:

    NEW

Then:

    thread.start();

The thread becomes:

    RUNNABLE

During its lifetime it may enter:

    BLOCKED
    WAITING
    TIMED_WAITING

After completing:

    TERMINATED

Conceptually:

    NEW
     ↓
    start()
     ↓
    RUNNABLE
     ↓
    execution
     ↓
    TERMINATED

---

# 🔹 Common Mistakes

## ❌ Mistake 1 — Calling `run()` instead of `start()`

Wrong when you want a new thread:

    thread.run();

Correct:

    thread.start();

---

## ❌ Mistake 2 — Starting a thread twice

Wrong:

    thread.start();
    thread.start();

This causes:

    IllegalThreadStateException

A `Thread` object can be started only once.

---

## ❌ Mistake 3 — Setting daemon after starting

Wrong:

    thread.start();
    thread.setDaemon(true);

Correct:

    thread.setDaemon(true);
    thread.start();

---

## ❌ Mistake 4 — Assuming priority guarantees execution order

Wrong assumption:

    Higher priority
        ↓
    Always executes first

Thread priority is not a guarantee of execution order.

---

## ❌ Mistake 5 — Assuming `sleep()` stops the whole application

`sleep()` pauses the currently executing thread.

It does not pause every thread in the JVM.

---

## ❌ Mistake 6 — Assuming `interrupt()` kills a thread

`interrupt()` does not forcibly terminate a thread.

It communicates an interruption request.

---

# 🔹 Interview Traps

### Trap 1: Where is `Thread` located?

    java.lang.Thread

---

### Trap 2: What is the default priority?

Normally:

    Thread.NORM_PRIORITY

which is:

    5

---

### Trap 3: Can a thread be started twice?

No.

It throws:

    IllegalThreadStateException

---

### Trap 4: What does `currentThread()` return?

It returns the `Thread` object representing the thread currently executing the code.

---

### Trap 5: Is `sleep()` static?

Yes.

It is a static method of `Thread`.

---

### Trap 6: Does `sleep()` release locks?

No.

Sleeping does not release an intrinsic monitor lock held by the thread.

---

### Trap 7: Does `interrupt()` kill a thread?

No.

It requests interruption.

---

### Trap 8: What is `Thread.State`?

It is an enum representing the state of a thread.

Possible values:

    NEW
    RUNNABLE
    BLOCKED
    WAITING
    TIMED_WAITING
    TERMINATED

---

# 🔹 DSA / Problem-Solving Relevance

The `Thread` class itself is not a DSA pattern.

However, understanding `Thread` becomes important when solving concurrency-based problems.

Examples include:

- Producer-Consumer
- Thread-safe counters
- Concurrent queues
- Alternate printing
- Ordered execution
- Resource sharing
- Synchronization problems

The basic mental model is:

    Task
      ↓
    Runnable
      ↓
    Thread
      ↓
    start()
      ↓
    Concurrent execution

---

# 🔹 30-Second Interview Answer

> `Thread` is a class in `java.lang` that represents a thread of execution. It provides methods such as `start()`, `run()`, `sleep()`, `join()`, `interrupt()`, `getState()`, `getName()`, and `setPriority()`. We can create a thread directly or provide a `Runnable` task to its constructor. Calling `start()` begins a new execution path, while calling `run()` directly is just a normal method call. A thread moves through states such as NEW, RUNNABLE, WAITING, and finally TERMINATED.

---

# 🔹 Cheat Sheet

| Method | Type | Purpose |
|---|---|---|
| `start()` | Instance | Starts thread execution |
| `run()` | Instance | Contains/executes task logic |
| `currentThread()` | Static | Returns current thread |
| `getName()` | Instance | Gets thread name |
| `setName()` | Instance | Sets thread name |
| `getId()` | Instance | Gets thread ID |
| `threadId()` | Instance | Gets thread ID in modern Java |
| `getPriority()` | Instance | Gets priority |
| `setPriority()` | Instance | Sets priority |
| `getState()` | Instance | Gets thread state |
| `isAlive()` | Instance | Checks whether thread is alive |
| `interrupt()` | Instance | Requests interruption |
| `isInterrupted()` | Instance | Checks interruption status |
| `sleep()` | Static | Pauses current thread |
| `join()` | Instance | Waits for another thread |
| `setDaemon()` | Instance | Marks thread as daemon |
| `isDaemon()` | Instance | Checks daemon status |

---

# 🔥 Important Constants

    Thread.MIN_PRIORITY
    → 1

    Thread.NORM_PRIORITY
    → 5

    Thread.MAX_PRIORITY
    → 10

Thread states:

    Thread.State.NEW
    Thread.State.RUNNABLE
    Thread.State.BLOCKED
    Thread.State.WAITING
    Thread.State.TIMED_WAITING
    Thread.State.TERMINATED

---

# 🧠 Memory Tricks

### Thread Creation

    new Thread()
        ↓
    start()
        ↓
    run()

### Task vs Thread

    Runnable = WHAT to do

    Thread = execution mechanism

### Important Difference

    start()
    → new thread

    run()
    → normal method call

### Thread Information

    getName()
    getId()
    getPriority()
    getState()

### Thread Control

    start()
    sleep()
    join()
    interrupt()

---

# 🔥 Top 10 Interview Questions

## 1. What is the `Thread` class?

`Thread` is a class in `java.lang` that represents a thread of execution and provides methods for managing and inspecting threads.

---

## 2. What is the difference between `start()` and `run()`?

`start()` initiates a new thread of execution.

Calling `run()` directly executes the method on the current thread and does not create a new thread.

---

## 3. Can `start()` be called twice?

No.

Calling `start()` more than once on the same thread object throws:

    IllegalThreadStateException

---

## 4. What does `Thread.currentThread()` do?

It returns the `Thread` object representing the thread currently executing the code.

Example:

    Thread current = Thread.currentThread();

---

## 5. What does `getName()` do?

It returns the name of the thread.

Example:

    String name = thread.getName();

---

## 6. What does `getState()` return?

It returns the current state of the thread as a `Thread.State` enum value.

---

## 7. What is the default thread priority?

The normal priority is:

    Thread.NORM_PRIORITY

which has the value:

    5

---

## 8. Does higher thread priority guarantee earlier execution?

No.

Priority is a scheduling hint and should not be treated as a strict execution-order guarantee.

---

## 9. What does `interrupt()` do?

It requests that a thread be interrupted.

It does not forcibly kill the thread.

---

## 10. What is a daemon thread?

A daemon thread is a background thread that does not normally prevent the JVM from shutting down once all non-daemon threads have finished.

---

# 🎯 Final Summary

The `Thread` class is one of the fundamental classes for understanding Java multithreading.

The most important concepts are:

    Thread
       ↓
    represents execution

    Runnable
       ↓
    represents task

    start()
       ↓
    starts new execution

    run()
       ↓
    contains task logic

    currentThread()
       ↓
    gets current thread

    getName()
       ↓
    gets thread name

    getState()
       ↓
    gets thread state

    sleep()
       ↓
    pauses current thread

    join()
       ↓
    waits for another thread

    interrupt()
       ↓
    requests interruption

    setDaemon()
       ↓
    marks thread as daemon

> **Core rule: Create the task → create the Thread → call `start()` → JVM schedules the execution.**