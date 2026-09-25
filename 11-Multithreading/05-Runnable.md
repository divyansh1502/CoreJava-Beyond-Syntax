# 05 — Runnable

> **`Runnable` is a functional interface that represents a task that can be executed by a thread.**

---

## 📌 Table of Contents

1. [Introduction](#-introduction)
2. [What is Runnable?](#-what-is-runnable)
3. [Runnable Interface](#-runnable-interface)
4. [run() Method](#-run-method)
5. [Creating a Runnable](#-creating-a-runnable)
6. [Runnable with Thread](#-runnable-with-thread)
7. [Lambda Expression with Runnable](#-lambda-expression-with-runnable)
8. [Runnable vs Thread](#-runnable-vs-thread)
9. [Multiple Tasks with Runnable](#-multiple-tasks-with-runnable)
10. [Passing Runnable to Thread](#-passing-runnable-to-thread)
11. [Anonymous Runnable](#-anonymous-runnable)
12. [Runnable and Functional Interface](#-runnable-and-functional-interface)
13. [Runnable with Thread Name](#-runnable-with-thread-name)
14. [Runnable and `start()`](#-runnable-and-start)
15. [Runnable and `run()`](#-runnable-and-run)
16. [Internal Working](#-internal-working)
17. [Why Prefer Runnable?](#-why-prefer-runnable)
18. [Common Mistakes](#-common-mistakes)
19. [Interview Traps](#-interview-traps)
20. [DSA / Problem-Solving Relevance](#-dsa--problem-solving-relevance)
21. [30-Second Interview Answer](#-30-second-interview-answer)
22. [Cheat Sheet](#-cheat-sheet)
23. [Top 10 Interview Questions](#-top-10-interview-questions)

---

# 🔹 Introduction

`Runnable` is an interface in Java used to represent a task that can be executed by a thread.

It belongs to:

    java.lang

Therefore, no explicit import is required.

The basic idea is:

    Runnable
        ↓
    represents a task

    Thread
        ↓
    executes the task

---

# 🔹 What is Runnable?

`Runnable` is an interface with one main abstract method:

    run()

Its purpose is to define **what a thread should do**.

Example:

    Runnable task = () -> {
        System.out.println("Task is running");
    };

Here:

    Runnable
        ↓
    task

The task itself does not automatically execute.

It needs a thread:

    Thread thread = new Thread(task);

Then:

    thread.start();

---

# 🔹 Runnable Interface

Conceptually, the interface looks like:

    @FunctionalInterface
    public interface Runnable {

        void run();
    }

The important method is:

    void run();

Because `Runnable` has one abstract method, it is a **functional interface**.

---

# 🔹 `run()` Method

`run()` contains the actual task logic.

Example:

    Runnable task = new Runnable() {

        @Override
        public void run() {
            System.out.println("Task is running");
        }
    };

The code inside `run()` represents the work that the thread should perform.

---

# 🔹 Creating a Runnable

There are several ways to create a `Runnable`.

## 1. Using a class

    class MyTask implements Runnable {

        @Override
        public void run() {
            System.out.println("Task is running");
        }
    }

Then:

    MyTask task = new MyTask();

---

## 2. Using Anonymous Class

    Runnable task = new Runnable() {

        @Override
        public void run() {
            System.out.println("Task is running");
        }
    };

---

## 3. Using Lambda

Because `Runnable` is a functional interface:

    Runnable task = () -> {
        System.out.println("Task is running");
    };

The lambda approach is the most concise.

---

# 🔹 Runnable with Thread

Creating a `Runnable` alone does not create a new thread.

Example:

    Runnable task = () -> {
        System.out.println("Task is running");
    };

Now create a `Thread`:

    Thread thread = new Thread(task);

Finally:

    thread.start();

Complete:

    Runnable task = () -> {
        System.out.println("Task is running");
    };

    Thread thread = new Thread(task);

    thread.start();

The relationship is:

    Runnable
        ↓
    task
        ↓
    Thread
        ↓
    start()
        ↓
    run()
        ↓
    execution

---

# 🔹 Lambda Expression with Runnable

Since `Runnable` has only one abstract method, we can use a lambda.

Traditional approach:

    Runnable task = new Runnable() {

        @Override
        public void run() {
            System.out.println("Hello");
        }
    };

Lambda approach:

    Runnable task = () -> {
        System.out.println("Hello");
    };

Both represent the same task.

The lambda is simply more concise.

---

# 🔹 Runnable vs Thread

This is one of the most important concepts.

| Runnable | Thread |
|---|---|
| Interface | Class |
| Represents a task | Represents a thread of execution |
| Defines `run()` | Provides thread-control methods |
| Does not itself start a thread | Can start a thread using `start()` |
| Can be implemented by a class | Can be extended by a class |
| Allows the class to extend another class | Java allows only single class inheritance |

Conceptually:

    Runnable
        ↓
    WHAT to do

    Thread
        ↓
    HOW/WHERE execution happens

Memory trick:

> **Runnable = task**

> **Thread = execution**

---

# 🔹 Multiple Tasks with Runnable

We can create multiple independent tasks.

    Runnable task1 = () -> {
        System.out.println("Task 1");
    };

    Runnable task2 = () -> {
        System.out.println("Task 2");
    };

Create threads for them:

    Thread thread1 = new Thread(task1);
    Thread thread2 = new Thread(task2);

Start them:

    thread1.start();
    thread2.start();

Possible output:

    Task 1
    Task 2

or:

    Task 2
    Task 1

The exact order is not guaranteed.

---

# 🔹 Passing Runnable to Thread

The `Thread` class provides a constructor that accepts a `Runnable`.

Example:

    Runnable task = () -> {
        System.out.println("Working...");
    };

    Thread thread = new Thread(task);

Here:

    task

is the `Runnable`.

And:

    thread

is the `Thread`.

When:

    thread.start();

is called, the thread eventually executes:

    task.run();

Conceptually:

    thread.start()
          ↓
    Thread starts execution
          ↓
    Runnable.run()
          ↓
    task executes

---

# 🔹 Anonymous Runnable

Before lambda expressions, an anonymous class was commonly used.

Example:

    Thread thread = new Thread(
        new Runnable() {

            @Override
            public void run() {
                System.out.println("Running...");
            }
        }
    );

    thread.start();

This works because `Thread` accepts a `Runnable`.

---

# 🔹 Runnable with Lambda

Modern Java usually makes this much shorter:

    Thread thread = new Thread(() -> {
        System.out.println("Running...");
    });

    thread.start();

The lambda:

    () -> {
        System.out.println("Running...");
    }

is treated as a `Runnable`.

---

# 🔹 Runnable and Functional Interface

`Runnable` is a functional interface because it has exactly one abstract method:

    run()

Therefore this is valid:

    Runnable task = () -> {
        System.out.println("Hello");
    };

The `@FunctionalInterface` annotation is associated with the interface definition.

Conceptually:

    @FunctionalInterface
    interface Runnable {

        void run();
    }

---

# 🔹 Runnable with Thread Name

We can provide a name while creating the thread.

    Runnable task = () -> {
        System.out.println(
            Thread.currentThread().getName()
        );
    };

    Thread thread = new Thread(task, "Worker-1");

    thread.start();

Possible output:

    Worker-1

Here:

    Runnable
        ↓
    defines the work

    Thread
        ↓
    gives execution context

    "Worker-1"
        ↓
    thread name

---

# 🔹 Runnable and `start()`

This is important:

    Runnable task = () -> {
        System.out.println("Hello");
    };

    Thread thread = new Thread(task);

Creating the `Runnable` does not execute it.

Creating the `Thread` does not execute it either.

Execution starts when:

    thread.start();

is called.

---

# 🔹 Runnable and `run()`

We can directly call:

    task.run();

Example:

    Runnable task = () -> {
        System.out.println("Hello");
    };

    task.run();

This executes the task directly on the current thread.

It does **not** create a new thread.

Compare:

    task.run();

with:

    new Thread(task).start();

The first is a normal method call.

The second starts a separate thread.

---

# 🔥 `run()` vs `start()`

### Direct `run()`

    Runnable task = () -> {
        System.out.println(
            Thread.currentThread().getName()
        );
    };

    task.run();

The task executes on the current thread.

---

### Using `start()`

    Runnable task = () -> {
        System.out.println(
            Thread.currentThread().getName()
        );
    };

    Thread thread = new Thread(task);

    thread.start();

The task executes through a separate thread.

---

# 🔹 Internal Working

Consider:

    Runnable task = () -> {
        System.out.println("Working");
    };

    Thread thread = new Thread(task);

    thread.start();

Conceptually:

    Runnable object
          ↓
    contains task logic
          ↓
    Thread receives Runnable
          ↓
    start()
          ↓
    JVM schedules thread
          ↓
    Thread executes
          ↓
    Runnable.run()
          ↓
    task completes

Important:

`Runnable` itself is not the thread.

It represents the work that the thread performs.

---

# 🔹 Why Prefer Runnable?

Using `Runnable` has several advantages.

## 1. Separation of task and execution

The task is represented separately from the thread.

    Runnable
        ↓
    task

    Thread
        ↓
    execution

This makes the design cleaner.

---

## 2. Avoids extending Thread

Java supports single class inheritance.

If we write:

    class MyTask extends Thread

then `MyTask` cannot extend another class.

With:

    class MyTask implements Runnable

the class can still extend another class.

Example:

    class MyTask extends SomeParent implements Runnable {

        @Override
        public void run() {
            System.out.println("Running");
        }
    }

This is one major reason `Runnable` is useful.

---

## 3. Same task can be used by multiple Threads

A single `Runnable` can be passed to multiple threads.

Example:

    Runnable task = () -> {
        System.out.println("Running");
    };

    Thread thread1 = new Thread(task);
    Thread thread2 = new Thread(task);

    thread1.start();
    thread2.start();

Both threads can execute the same task logic.

---

# 🔹 Runnable with Multiple Threads

Example:

    Runnable task = () -> {

        System.out.println(
            Thread.currentThread().getName()
            + " is running"
        );
    };

    Thread thread1 = new Thread(task, "Worker-1");
    Thread thread2 = new Thread(task, "Worker-2");

    thread1.start();
    thread2.start();

Possible output:

    Worker-1 is running
    Worker-2 is running

The order can vary.

---

# 🔹 Runnable Does Not Have `start()`

This is a common interview point.

`Runnable` provides:

    run()

It does not provide:

    start()

`start()` belongs to:

    Thread

Therefore:

    Runnable task = () -> {
        System.out.println("Hello");
    };

This is invalid:

    task.start();

Correct:

    Thread thread = new Thread(task);
    thread.start();

---

# 🔹 Runnable Does Not Control Thread Lifecycle

`Runnable` defines the task.

Thread lifecycle operations are handled by `Thread`.

For example:

    start()
    sleep()
    join()
    interrupt()
    getState()
    isAlive()

These are associated with `Thread`.

---

# 🔹 Runnable and Return Value

`Runnable.run()` has return type:

    void

Therefore, a `Runnable` task does not directly return a result.

Example:

    Runnable task = () -> {

        int result = 10 + 20;

        System.out.println(result);
    };

There is no return value from:

    run()

If a task needs to return a result, Java provides:

    Callable

This will be covered separately.

---

# 🔹 Runnable and Checked Exceptions

The `run()` method of `Runnable` does not declare checked exceptions.

Conceptually:

    void run();

Therefore, you cannot directly write:

    Runnable task = () -> {
        throw new IOException();
    };

without handling the checked exception.

You need to handle it appropriately inside the task.

---

# 🔹 Runnable Example — Complete Program

    class Main {

        public static void main(String[] args) {

            Runnable task = () -> {

                System.out.println(
                    "Running on: "
                    + Thread.currentThread().getName()
                );
            };

            Thread thread = new Thread(task, "Worker-1");

            thread.start();
        }
    }

Possible output:

    Running on: Worker-1

---

# 🔹 Runnable Using a Class

We can implement `Runnable` in our own class.

    class MyTask implements Runnable {

        @Override
        public void run() {
            System.out.println("Task is running");
        }
    }

Then:

    class Main {

        public static void main(String[] args) {

            MyTask task = new MyTask();

            Thread thread = new Thread(task);

            thread.start();
        }
    }

---

# 🔹 Runnable + Inheritance

One major advantage of `Runnable` is that our class can still extend another class.

Example:

    class Employee {
        
        void work() {
            System.out.println("Employee work");
        }
    }

    class EmployeeTask extends Employee implements Runnable {

        @Override
        public void run() {
            System.out.println("Running task");
        }
    }

This would not be possible if `EmployeeTask` had to extend `Thread`, because Java does not support multiple class inheritance.

---

# 🔹 Thread vs Runnable — Interview Comparison

| Feature | Thread | Runnable |
|---|---|---|
| Type | Class | Interface |
| Represents | Thread/execution mechanism | Task |
| Main method | `run()` | `run()` |
| Starts execution | `start()` | No `start()` |
| Can extend another class? | No, if already extending Thread | Yes |
| Reusable task | Less flexible | More flexible |
| Separation of concerns | Lower | Better |
| Common modern usage | Used to execute tasks | Used to define tasks |

---

# 🔹 Common Mistakes

## ❌ Mistake 1 — Calling `start()` on Runnable

Wrong:

    Runnable task = () -> {
        System.out.println("Hello");
    };

    task.start();

`Runnable` has no `start()` method.

Correct:

    Thread thread = new Thread(task);

    thread.start();

---

## ❌ Mistake 2 — Thinking Runnable itself is a thread

Wrong:

    Runnable = Thread

Correct:

    Runnable = task

    Thread = execution mechanism

---

## ❌ Mistake 3 — Calling `run()` expecting a new thread

Wrong assumption:

    task.run();

means:

    create new thread

It does not.

It is simply a method call.

---

## ❌ Mistake 4 — Forgetting `start()`

Example:

    Runnable task = () -> {
        System.out.println("Running");
    };

    Thread thread = new Thread(task);

If we never call:

    thread.start();

the task will not execute.

---

# 🔹 Interview Traps

### Trap 1: Is Runnable a class?

No.

It is an interface.

---

### Trap 2: Is Runnable a functional interface?

Yes.

It has one abstract method:

    run()

---

### Trap 3: Does Runnable have `start()`?

No.

`start()` belongs to `Thread`.

---

### Trap 4: Does `run()` create a new thread?

No.

Calling `run()` directly is a normal method call.

---

### Trap 5: Can a class implement Runnable and extend another class?

Yes.

Example:

    class MyTask extends Parent implements Runnable {

        @Override
        public void run() {
            System.out.println("Running");
        }
    }

---

### Trap 6: Can the same Runnable be passed to multiple Threads?

Yes.

Example:

    Runnable task = () -> {
        System.out.println("Working");
    };

    Thread t1 = new Thread(task);
    Thread t2 = new Thread(task);

---

### Trap 7: Does Runnable return a value?

No.

Its `run()` method returns:

    void

For tasks that produce a result, `Callable` is used.

---

# 🔹 DSA / Problem-Solving Relevance

`Runnable` is not itself a DSA pattern.

However, it becomes useful when implementing concurrent algorithms and systems.

Examples:

- Producer-Consumer
- Parallel processing
- Concurrent data processing
- Thread-safe operations
- Task execution
- Concurrent searching
- Parallel computation

A useful mental model is:

    Problem
       ↓
    Split into tasks
       ↓
    Runnable
       ↓
    Threads execute tasks
       ↓
    Combine/process results

---

# 🔹 30-Second Interview Answer

> `Runnable` is a functional interface in Java that represents a task that can be executed by a thread. It contains a single abstract method called `run()`. Unlike `Thread`, `Runnable` does not represent the actual execution mechanism and does not have a `start()` method. We normally create a `Runnable`, pass it to a `Thread`, and call `start()` on the thread. Using `Runnable` also allows a class to extend another class because Java supports only single class inheritance.

---

# 🔹 Cheat Sheet

    Runnable
        ↓
    Interface

    Main method:
        run()

    Represents:
        Task

    Does NOT provide:
        start()

    Thread:
        executes Runnable

    Basic pattern:

    Runnable task = () -> {
        // task
    };

    Thread thread = new Thread(task);

    thread.start();

---

# 🔥 Most Important Difference

    Runnable
        ↓
    WHAT should be done?

    Thread
        ↓
    WHO executes it?

---

# 🔥 Execution Flow

    Runnable task
          ↓
    new Thread(task)
          ↓
    thread.start()
          ↓
    New thread becomes eligible
          ↓
    run()
          ↓
    Task executes
          ↓
    Thread terminates

---

# 🔥 Top 10 Interview Questions

## 1. What is Runnable?

`Runnable` is an interface used to represent a task that can be executed by a thread.

---

## 2. Which method does Runnable contain?

The main abstract method is:

    void run()

---

## 3. Is Runnable a functional interface?

Yes.

It has one abstract method, `run()`.

---

## 4. Does Runnable create a thread?

No.

`Runnable` represents the task.

A `Thread` is used to execute that task.

---

## 5. Does Runnable have `start()`?

No.

`start()` belongs to `Thread`.

---

## 6. What happens when `run()` is called directly?

The method executes normally on the current thread.

No new thread is created.

---

## 7. Why use Runnable instead of extending Thread?

Because implementing `Runnable` allows the class to extend another class.

It also separates the task from the execution mechanism.

---

## 8. Can the same Runnable be passed to multiple threads?

Yes.

Example:

    Runnable task = () -> {
        System.out.println("Working");
    };

    Thread t1 = new Thread(task);
    Thread t2 = new Thread(task);

---

## 9. Can Runnable return a result?

No.

`Runnable.run()` returns `void`.

For result-producing tasks, `Callable` is used.

---

## 10. What is the difference between Runnable and Thread?

`Runnable` represents the task, while `Thread` represents the execution mechanism that can execute that task.

---

# 🎯 Final Summary

Remember these three lines:

    Runnable = TASK

    Thread = EXECUTION

    start() = START NEW THREAD

The standard pattern is:

    Runnable task = () -> {
        System.out.println("Working");
    };

    Thread thread = new Thread(task);

    thread.start();

And the most important interview distinction:

    task.run()
        ↓
    normal method call

    thread.start()
        ↓
    starts a new thread
        ↓
    eventually executes run()