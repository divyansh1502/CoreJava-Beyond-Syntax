# 05 — Runnable

> **`Runnable` represents a task that can be executed by a thread.**

---

## 📌 Table of Contents

1. [Introduction](#-introduction)
2. [What is Runnable?](#-what-is-runnable)
3. [Runnable Interface](#-runnable-interface)
4. [Why Runnable?](#-why-runnable)
5. [Implementing Runnable](#-implementing-runnable)
6. [Starting a Runnable](#-starting-a-runnable)
7. [Runnable vs Thread](#-runnable-vs-thread)
8. [Runnable with Lambda](#-runnable-with-lambda)
9. [Multiple Threads with One Runnable](#-multiple-threads-with-one-runnable)
10. [Runnable and Shared Data](#-runnable-and-shared-data)
11. [Runnable as a Functional Interface](#-runnable-as-a-functional-interface)
12. [run() Method](#-run-method)
13. [Thread Constructor with Runnable](#-thread-constructor-with-runnable)
14. [Runnable Internal Working](#-runnable-internal-working)
15. [Common Mistakes](#-common-mistakes)
16. [Interview Traps](#-interview-traps)
17. [DSA / Problem-Solving Relevance](#-dsa--problem-solving-relevance)
18. [30-Second Interview Answer](#-30-second-interview-answer)
19. [Cheat Sheet](#-cheat-sheet)
20. [Top 10 Interview Questions](#-top-10-interview-questions)

---

# 🔹 Introduction

Java provides the `Runnable` interface to represent a task that can be executed by a thread.

It belongs to:

```java
java.lang.Runnable
```

Because `java.lang` is automatically imported, we can directly use:

```java
Runnable
```

The basic idea is:

```text
Runnable
   ↓
Represents a task

Thread
   ↓
Executes the task
```

This separation between the **task** and the **thread executing the task** is one of the important concepts in Java concurrency.

---

# 🔹 What is Runnable?

`Runnable` is an interface that represents a task that can be executed.

Its main abstract method is:

```java
void run();
```

A class can implement `Runnable` and define its task inside `run()`.

Example:

```java
class MyTask implements Runnable {

    @Override
    public void run() {

        System.out.println("Task is running");
    }
}
```

Here:

```text
MyTask
   ↓
implements Runnable
   ↓
defines run()
```

But remember:

> A `Runnable` object itself is **not a thread**.

It represents the **task**.

A `Thread` can execute that task.

---

# 🔹 Runnable Interface

The basic structure is:

```java
@FunctionalInterface
public interface Runnable {

    void run();
}
```

The important method is:

```java
run()
```

Because `Runnable` has only one abstract method, it is a **functional interface**.

Therefore, it can be used with lambda expressions.

---

# 🔹 Why Runnable?

Suppose we create a thread by extending `Thread`:

```java
class MyThread extends Thread {

    @Override
    public void run() {

        System.out.println("Running");
    }
}
```

The limitation is that Java supports **single inheritance**.

A class can extend only one class.

For example:

```java
class MyClass extends SomeClass {

}
```

It cannot extend another class at the same time:

```java
// Not valid Java

class MyClass extends SomeClass, Thread {

}
```

Therefore, if your class already extends another class, it cannot extend `Thread`.

`Runnable` solves this problem because it is an interface.

A class can extend one class and implement multiple interfaces.

Example:

```java
class MyClass extends SomeClass implements Runnable {

    @Override
    public void run() {

        System.out.println("Task running");
    }
}
```

This is one of the major reasons `Runnable` is preferred when you want to represent a task independently from the thread.

---

# 🔹 Implementing Runnable

The basic syntax is:

```java
class MyTask implements Runnable {

    @Override
    public void run() {

        System.out.println("Task is running");
    }
}
```

Then create the task:

```java
MyTask task = new MyTask();
```

At this point:

```text
task exists
```

but:

```text
No new thread has started
```

because `Runnable` is only the task definition.

---

# 🔹 Starting a Runnable

To execute a `Runnable`, pass it to a `Thread`.

Example:

```java
class MyTask implements Runnable {

    @Override
    public void run() {

        System.out.println("Task is running");
    }
}

class Main {

    public static void main(String[] args) {

        MyTask task = new MyTask();

        Thread thread = new Thread(task);

        thread.start();
    }
}
```

Flow:

```text
MyTask object
     ↓
Runnable
     ↓
passed to Thread
     ↓
thread.start()
     ↓
new thread
     ↓
run()
```

---

# 🔹 Important Difference

This:

```java
Runnable task = new MyTask();
```

does **not** create a new thread.

This:

```java
Thread thread = new Thread(task);
```

creates a `Thread` object associated with the task.

And this:

```java
thread.start();
```

starts the new thread.

---

# 🔹 Runnable vs Thread

## Using Thread

```java
class MyThread extends Thread {

    @Override
    public void run() {

        System.out.println("Running");
    }
}

class Main {

    public static void main(String[] args) {

        MyThread t = new MyThread();

        t.start();
    }
}
```

Here:

```text
MyThread
   ↓
is-a Thread
   ↓
contains task
```

---

## Using Runnable

```java
class MyTask implements Runnable {

    @Override
    public void run() {

        System.out.println("Running");
    }
}

class Main {

    public static void main(String[] args) {

        MyTask task = new MyTask();

        Thread t = new Thread(task);

        t.start();
    }
}
```

Here:

```text
MyTask
   ↓
is-a Runnable
   ↓
represents task

Thread
   ↓
executes task
```

---

# 🔹 Main Difference

| `Thread` | `Runnable` |
|---|---|
| Class | Interface |
| Represents a thread | Represents a task |
| Extend `Thread` | Implement `Runnable` |
| Uses inheritance | Uses interface implementation |
| Cannot extend another class at the same time | Can extend another class |
| Task and thread are more tightly coupled | Task and thread are separated |

---

# 🔹 Runnable with Lambda

Because `Runnable` is a functional interface, we can use a lambda expression.

Instead of:

```java
class MyTask implements Runnable {

    @Override
    public void run() {

        System.out.println("Task running");
    }
}
```

we can write:

```java
Runnable task = () -> {

    System.out.println("Task running");
};
```

Then:

```java
Thread thread = new Thread(task);

thread.start();
```

Complete example:

```java
class Main {

    public static void main(String[] args) {

        Runnable task = () -> {

            System.out.println("Task running");
        };

        Thread thread = new Thread(task);

        thread.start();
    }
}
```

This is very common in modern Java.

---

# 🔹 Multiple Threads with One Runnable

The same `Runnable` object can be passed to multiple threads.

Example:

```java
class MyTask implements Runnable {

    @Override
    public void run() {

        System.out.println(
            Thread.currentThread().getName()
            + " is running"
        );
    }
}

class Main {

    public static void main(String[] args) {

        MyTask task = new MyTask();

        Thread t1 = new Thread(task, "Thread-1");
        Thread t2 = new Thread(task, "Thread-2");
        Thread t3 = new Thread(task, "Thread-3");

        t1.start();
        t2.start();
        t3.start();
    }
}
```

Possible output:

```text
Thread-1 is running
Thread-3 is running
Thread-2 is running
```

The exact order is not guaranteed.

---

# 🔹 One Runnable, Multiple Threads

Conceptually:

```text
                 Runnable Task
                 /     |     \
                /      |      \
               ↓       ↓       ↓
           Thread-1 Thread-2 Thread-3
```

This is useful because the task logic can be reused by multiple threads.

---

# 🔹 Runnable and Shared Data

If multiple threads use the same `Runnable` object, they can access the same instance fields.

Example:

```java
class CounterTask implements Runnable {

    int count = 0;

    @Override
    public void run() {

        count++;

        System.out.println(
            Thread.currentThread().getName()
            + " : "
            + count
        );
    }
}
```

Multiple threads can use the same task:

```java
class Main {

    public static void main(String[] args) {

        CounterTask task = new CounterTask();

        Thread t1 = new Thread(task);
        Thread t2 = new Thread(task);
        Thread t3 = new Thread(task);

        t1.start();
        t2.start();
        t3.start();
    }
}
```

Here:

```text
Same Runnable object
        ↓
      count
        ↑
   shared state
        ↑
        |
   multiple threads
```

This introduces concurrency concerns such as:

- Race conditions
- Data inconsistency
- Synchronization

These concepts will be covered later.

---

# 🔹 Runnable as a Functional Interface

`Runnable` is a functional interface because it has exactly one abstract method:

```java
void run();
```

Therefore, this is valid:

```java
Runnable task = () -> {

    System.out.println("Hello");
};
```

It is equivalent in concept to:

```java
Runnable task = new Runnable() {

    @Override
    public void run() {

        System.out.println("Hello");
    }
};
```

The lambda version is simply more concise.

---

# 🔹 Anonymous Class with Runnable

Before lambdas became available in Java 8, anonymous classes were commonly used.

Example:

```java
class Main {

    public static void main(String[] args) {

        Runnable task = new Runnable() {

            @Override
            public void run() {

                System.out.println("Task running");
            }
        };

        Thread thread = new Thread(task);

        thread.start();
    }
}
```

Modern Java often uses a lambda instead:

```java
class Main {

    public static void main(String[] args) {

        Runnable task = () -> {

            System.out.println("Task running");
        };

        new Thread(task).start();
    }
}
```

---

# 🔹 run() Method

The `Runnable` interface defines:

```java
void run();
```

It represents the task that should be executed.

Example:

```java
Runnable task = () -> {

    System.out.println("Executing task");
};
```

However, calling `run()` directly does not create a new thread.

Example:

```java
Runnable task = () -> {

    System.out.println(
        Thread.currentThread().getName()
    );
};

task.run();
```

The task executes in the current thread.

If called from `main()`, the output will typically be:

```text
main
```

---

# 🔹 start() is Still Called on Thread

Notice:

```java
task.start();
```

is invalid because `Runnable` does not have a `start()` method.

This is wrong:

```java
Runnable task = () -> {

    System.out.println("Task");
};

task.start();
```

`Runnable` only defines the task.

Instead:

```java
Runnable task = () -> {

    System.out.println("Task");
};

Thread thread = new Thread(task);

thread.start();
```

Remember:

```text
Runnable
   ↓
Task

Thread
   ↓
Starts and executes task
```

---

# 🔹 Thread Constructor with Runnable

The `Thread` class provides constructors that accept a `Runnable`.

The commonly used form is:

```java
Thread(Runnable target)
```

Example:

```java
Runnable task = () -> {

    System.out.println("Running task");
};

Thread thread = new Thread(task);

thread.start();
```

You can also provide a thread name:

```java
Runnable task = () -> {

    System.out.println("Running");
};

Thread thread = new Thread(task, "Worker");

thread.start();
```

Inside the task:

```java
Runnable task = () -> {

    System.out.println(
        Thread.currentThread().getName()
    );
};

Thread thread = new Thread(task, "Worker");

thread.start();
```

Possible output:

```text
Worker
```

---

# 🔹 Runnable Internal Working

Consider:

```java
Runnable task = () -> {

    System.out.println("Task");
};

Thread thread = new Thread(task);

thread.start();
```

Conceptually:

```text
1. Create Runnable
       ↓
2. Runnable stores task logic
       ↓
3. Create Thread and give it Runnable
       ↓
4. Call start()
       ↓
5. JVM starts a new thread
       ↓
6. Thread executes Runnable's run()
       ↓
7. Task completes
```

The important relationship is:

```text
Runnable
   |
   | contains
   ↓
Task

Thread
   |
   | executes
   ↓
Runnable
```

---

# 🔹 Runnable Does Not Create a Thread

This is a very important interview concept.

Creating:

```java
Runnable task = () -> {

    System.out.println("Task");
};
```

does not create a new thread.

Creating:

```java
Thread thread = new Thread(task);
```

creates a `Thread` object.

Calling:

```java
thread.start();
```

starts the new thread.

Therefore:

```text
Runnable
= task definition

Thread
= thread object

start()
= begins new thread execution
```

---

# 🔹 Runnable vs Callable

`Runnable` and `Callable` both represent tasks, but they differ in their ability to return a result and throw checked exceptions.

### Runnable

```java
Runnable task = () -> {

    System.out.println("Task");
};
```

`Runnable.run()`:

```java
void run()
```

It does not return a value.

---

### Callable

```java
Callable<Integer> task = () -> {

    return 100;
};
```

`Callable.call()`:

```java
V call() throws Exception
```

It can return a result and declare checked exceptions.

`Callable` is covered later in the Executor/Callable topic.

---

# 🔹 Advantages of Runnable

### 1. Separates task from thread

```text
Task
 ↓
Runnable

Execution mechanism
 ↓
Thread
```

---

### 2. Supports inheritance from another class

Example:

```java
class MyTask extends SomeClass implements Runnable {

    @Override
    public void run() {

        System.out.println("Task");
    }
}
```

---

### 3. Works naturally with lambdas

```java
Runnable task = () -> {

    System.out.println("Task");
};
```

---

### 4. Task can be reused

The same task can be supplied to different threads.

```java
Runnable task = () -> {

    System.out.println("Task");
};

Thread t1 = new Thread(task);
Thread t2 = new Thread(task);

t1.start();
t2.start();
```

---

### 5. Fits Executor Framework

`Runnable` is commonly submitted to executor services.

Example:

```java
ExecutorService executor = Executors.newSingleThreadExecutor();

executor.submit(() -> {

    System.out.println("Task running");
});

executor.shutdown();
```

The Executor Framework will be covered in detail later.

---

# 🔹 Disadvantages / Limitations

### 1. Cannot return a result directly

`run()` returns:

```java
void
```

If you need a result, `Callable` is generally used with an executor.

---

### 2. Cannot directly declare checked exceptions

The `run()` method does not declare checked exceptions.

You cannot write:

```java
@Override
public void run() throws Exception {

}
```

and treat it as overriding the `Runnable.run()` signature.

Checked exceptions must be handled inside the implementation.

Example:

```java
Runnable task = () -> {

    try {

        Thread.sleep(1000);

    } catch (InterruptedException e) {

        Thread.currentThread().interrupt();
    }
};
```

---

# 🔹 Common Mistakes

## ❌ Mistake 1 — Thinking Runnable is a Thread

Wrong concept:

```text
Runnable = Thread
```

Correct:

```text
Runnable = Task
Thread = Executes Task
```

---

## ❌ Mistake 2 — Calling start() on Runnable

Wrong:

```java
Runnable task = () -> {

    System.out.println("Task");
};

task.start();
```

`Runnable` does not have `start()`.

Correct:

```java
Runnable task = () -> {

    System.out.println("Task");
};

Thread thread = new Thread(task);

thread.start();
```

---

## ❌ Mistake 3 — Calling run() expecting a new thread

Wrong assumption:

```java
task.run();
```

Calling `run()` directly is a normal method call.

Correct:

```java
new Thread(task).start();
```

---

## ❌ Mistake 4 — Assuming execution order

With multiple threads:

```java
Thread t1 = new Thread(task);
Thread t2 = new Thread(task);

t1.start();
t2.start();
```

You cannot assume `t1` will always finish before `t2`.

---

# 🔹 Interview Traps

### Q1. Is Runnable a class or interface?

`Runnable` is an interface.

---

### Q2. Is Runnable a thread?

No.

It represents a task that can be executed by a thread.

---

### Q3. Does creating a Runnable start a thread?

No.

Example:

```java
Runnable task = () -> {

    System.out.println("Task");
};
```

No new thread has started.

---

### Q4. How do you execute a Runnable in a new thread?

Pass it to a `Thread` and call `start()`.

```java
Runnable task = () -> {

    System.out.println("Task");
};

Thread t = new Thread(task);

t.start();
```

---

### Q5. Can Runnable be used with lambda expressions?

Yes.

Because `Runnable` is a functional interface.

---

### Q6. Can a class extend another class and implement Runnable?

Yes.

Example:

```java
class MyTask extends SomeClass implements Runnable {

    @Override
    public void run() {

        System.out.println("Task");
    }
}
```

---

### Q7. What does Runnable.run() return?

Nothing.

Its return type is:

```java
void
```

---

### Q8. Can Runnable return a result?

Not directly through `run()`.

For a task that returns a result, `Callable` is designed for that use case.

---

### Q9. What happens when run() is called directly?

It executes like a normal method in the current thread.

It does not create a new thread.

---

### Q10. Why is Runnable often preferred over extending Thread?

Because it separates the task from the thread and allows the class to extend another class.

---

# 🔹 DSA / Problem-Solving Relevance

`Runnable` is not itself a DSA pattern, but it becomes useful when DSA problems involve concurrency.

Relevant areas include:

- Concurrent processing
- Parallel search
- Producer-consumer problems
- Shared counters
- Concurrent queues
- Multithreaded algorithms
- Thread-safe data structures

Example conceptual problem:

```text
Large array
     ↓
Split into parts
     ↓
Runnable Task 1 → Part 1
Runnable Task 2 → Part 2
Runnable Task 3 → Part 3
     ↓
Process concurrently
     ↓
Combine results
```

However, concurrency adds overhead and synchronization requirements, so using multiple threads does not automatically improve performance.

---

# 🔹 30-Second Interview Answer

> `Runnable` is a functional interface in Java that represents a task to be executed by a thread. It contains a single abstract method called `run()`. A `Runnable` itself is not a thread. We normally pass it to a `Thread` object and call `start()` to execute the task in a new thread. Using `Runnable` separates the task from the thread and also allows a class to extend another class, since Java supports single class inheritance.

---

# 🔹 Cheat Sheet

## Implement Runnable

```java
class MyTask implements Runnable {

    @Override
    public void run() {

        System.out.println("Task running");
    }
}
```

---

## Create Runnable

```java
Runnable task = new MyTask();
```

---

## Create Thread

```java
Thread thread = new Thread(task);
```

---

## Start Thread

```java
thread.start();
```

---

## Lambda

```java
Runnable task = () -> {

    System.out.println("Task running");
};
```

---

## Lambda + Thread

```java
Runnable task = () -> {

    System.out.println("Task running");
};

new Thread(task).start();
```

---

## Named Thread

```java
Runnable task = () -> {

    System.out.println(
        Thread.currentThread().getName()
    );
};

Thread thread = new Thread(task, "Worker");

thread.start();
```

---

# 🧠 Memory Tricks

### Runnable

> **Runnable = Work**

### Thread

> **Thread = Worker that executes the work**

### `run()`

> **run() = What should be done**

### `start()`

> **start() = Start a new thread**

### Runnable + Thread

```text
Runnable
   ↓
WHAT to do

Thread
   ↓
WHO executes it
```

---

# 🔥 Runnable vs Thread — Quick Revision

```text
                 Thread              Runnable
                   |                    |
                Class               Interface
                   |                    |
             Represents             Represents
               thread                  task
                   |                    |
             start()                  run()
                   |                    |
          Starts execution       Defines execution
```

---

# 🔥 Top 10 Interview Questions

## 1. What is Runnable?

`Runnable` is an interface that represents a task that can be executed by a thread.

---

## 2. Is Runnable a Thread?

No.

`Runnable` represents a task, while `Thread` represents the thread that executes the task.

---

## 3. What is the main method of Runnable?

```java
void run();
```

---

## 4. How do you execute Runnable?

```java
Runnable task = () -> {

    System.out.println("Task");
};

Thread thread = new Thread(task);

thread.start();
```

---

## 5. Does Runnable create a thread?

No.

A `Thread` is needed to execute the `Runnable` in a separate thread.

---

## 6. What happens if run() is called directly?

It executes as a normal method in the current thread.

---

## 7. Why use Runnable instead of extending Thread?

Because:

1. Java supports single inheritance.
2. The task and thread are separated.
3. The same task can be supplied to different threads.
4. It works naturally with lambda expressions and executor APIs.

---

## 8. Is Runnable a functional interface?

Yes.

It has one abstract method:

```java
void run();
```

---

## 9. Can Runnable return a value?

`Runnable.run()` returns `void`.

For tasks that produce a result, `Callable` is designed for that purpose.

---

## 10. What is the relationship between Thread and Runnable?

A `Runnable` represents the task.

A `Thread` provides the execution mechanism.

Example:

```java
Runnable task = () -> {

    System.out.println("Hello");
};

Thread thread = new Thread(task);

thread.start();
```

Conceptually:

```text
Runnable
   ↓
Task

Thread
   ↓
Executes Task
```

---

# 🎯 Final Summary

The core idea of `Runnable` is:

```text
Runnable
   ↓
Defines a task
   ↓
run()
   ↓
Thread receives Runnable
   ↓
start()
   ↓
New thread executes run()
```

### ⭐ Remember

```text
Runnable ≠ Thread

Runnable
→ task

Thread
→ executes task

run()
→ task logic

start()
→ starts new thread
```

### ⭐ Most Important Code

```java
Runnable task = () -> {

    System.out.println("Task is running");
};

Thread thread = new Thread(task);

thread.start();
```

> **Core idea: `Runnable` separates the work from the thread that performs the work.**