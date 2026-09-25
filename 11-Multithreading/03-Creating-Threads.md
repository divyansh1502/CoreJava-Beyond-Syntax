# 03 — Creating Threads

> **Java provides multiple ways to create and execute threads, mainly by extending `Thread` or implementing `Runnable`.**

---

## 📌 Table of Contents

1. [Introduction](#-introduction)
2. [Why Create Threads](#-why-create-threads)
3. [What Is a Thread Task](#-what-is-a-thread-task)
4. [Approach 1 — Extending Thread](#-approach-1--extending-thread)
5. [Approach 2 — Implementing Runnable](#-approach-2--implementing-runnable)
6. [Approach 3 — Runnable with Lambda](#-approach-3--runnable-with-lambda)
7. [start() vs run()](#-start-vs-run)
8. [Why start() Creates a New Thread](#-why-start-creates-a-new-thread)
9. [Why run() Does Not Create a New Thread](#-why-run-does-not-create-a-new-thread)
10. [Thread Object vs Runnable Task](#-thread-object-vs-runnable-task)
11. [Anonymous Runnable](#-anonymous-runnable)
12. [Creating Multiple Threads](#-creating-multiple-threads)
13. [Thread Naming](#-thread-naming)
14. [Getting the Current Thread](#-getting-the-current-thread)
15. [Checking Thread State](#-checking-thread-state)
16. [Passing Data to a Thread](#-passing-data-to-a-thread)
17. [Thread Constructor with Runnable](#-thread-constructor-with-runnable)
18. [Internal Working](#-internal-working)
19. [JVM Perspective](#-jvm-perspective)
20. [Thread Lifecycle Connection](#-thread-lifecycle-connection)
21. [Which Approach Should You Use](#-which-approach-should-you-use)
22. [Common Mistakes](#-common-mistakes)
23. [Interview Traps](#-interview-traps)
24. [DSA / Problem-Solving Relevance](#-dsa--problem-solving-relevance)
25. [30-Second Interview Answer](#-30-second-interview-answer)
26. [Cheat Sheet](#-cheat-sheet)
27. [Top 10 Interview Questions](#-top-10-interview-questions)

---

# 🔹 Introduction

A thread represents an independent path of execution within a program.

Java allows us to create threads in different ways.

The most fundamental approaches are:

1. Extend the `Thread` class.
2. Implement the `Runnable` interface.
3. Use a lambda expression with `Runnable`.

The first two are the traditional approaches and are important for understanding Java multithreading.

---

# 🔹 Why Create Threads?

Threads allow multiple tasks to make progress concurrently.

For example, an application may need to:

- Process user requests
- Perform background work
- Read files
- Perform network operations
- Process data
- Handle multiple clients

Without concurrency, tasks may execute sequentially:

    Task A
      ↓
    Finish
      ↓
    Task B
      ↓
    Finish
      ↓
    Task C

With multiple threads, tasks can make progress concurrently:

    Task A ─────────────────→

    Task B ─────────────────→

    Task C ─────────────────→

The exact execution order is controlled by the JVM and underlying operating-system scheduler.

---

# 🔹 What Is a Thread Task?

Before creating a thread, understand the difference between:

```text
Task
```

and:

```text
Thread
```

A **task** is the work that needs to be performed.

A **thread** is the execution mechanism that can execute that work.

For example:

```java
Runnable task = () -> {
    System.out.println("Processing data");
};
```

Here:

```text
Runnable
   ↓
represents the task
```

Then:

```java
Thread thread = new Thread(task);
```

Here:

```text
Thread
   ↓
executes the task
```

This separation becomes especially important when learning the Executor Framework.

---

# 🔹 Approach 1 — Extending Thread

The first way is to create a class that extends `Thread`.

```java
class MyThread extends Thread {

    @Override
    public void run() {
        System.out.println("My thread is running");
    }
}
```

Create the object:

```java
MyThread thread = new MyThread();
```

Start the thread:

```java
thread.start();
```

Complete example:

```java
class MyThread extends Thread {

    @Override
    public void run() {
        System.out.println("My thread is running");
    }
}

class Main {

    public static void main(String[] args) {

        MyThread thread = new MyThread();

        thread.start();
    }
}
```

Possible output:

```text
My thread is running
```

---

# 🔹 What Happens Here?

When we create:

```java
MyThread thread = new MyThread();
```

the thread object is created.

Its initial state is:

```text
NEW
```

When:

```java
thread.start();
```

is called:

```text
NEW
 ↓
RUNNABLE
```

The JVM then arranges for the thread's `run()` method to execute.

---

# 🔹 Why Override `run()`?

The `run()` method contains the work that the thread should perform.

Example:

```java
class MyThread extends Thread {

    @Override
    public void run() {

        for (int i = 1; i <= 5; i++) {
            System.out.println(i);
        }
    }
}
```

Then:

```java
MyThread thread = new MyThread();

thread.start();
```

The new thread executes the code inside:

```text
run()
```

---

# 🔹 Important: `run()` Is Not the Thread Starter

A very common beginner mistake is:

```java
thread.run();
```

thinking it starts a new thread.

It does not.

Calling `run()` directly is simply a normal method call.

Example:

```java
class MyThread extends Thread {

    @Override
    public void run() {
        System.out.println("Running");
    }
}

class Main {

    public static void main(String[] args) {

        MyThread thread = new MyThread();

        thread.run();

        System.out.println("Main thread");
    }
}
```

The `run()` method executes on the current thread.

No new thread is created.

---

# 🔹 Approach 2 — Implementing Runnable

The second approach is to implement the `Runnable` interface.

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

Create a `Thread` using that task:

```java
Thread thread = new Thread(task);
```

Then start the thread:

```java
thread.start();
```

Complete example:

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

Possible output:

```text
Task is running
```

---

# 🔹 How Runnable Works

Here the responsibilities are separated:

```text
Runnable
   ↓
Defines WHAT to execute

Thread
   ↓
Defines WHERE/HOW the task executes
```

The `run()` method belongs to the task:

```java
class MyTask implements Runnable {

    @Override
    public void run() {
        System.out.println("Task");
    }
}
```

The `Thread` object executes that task:

```java
Thread thread = new Thread(task);
```

Then:

```java
thread.start();
```

starts the new thread.

---

# 🔹 Approach 3 — Runnable with Lambda

`Runnable` is a functional interface because it contains one abstract method:

```java
void run();
```

Therefore, we can use a lambda expression.

Instead of:

```java
class MyTask implements Runnable {

    @Override
    public void run() {
        System.out.println("Task is running");
    }
}
```

we can write:

```java
Runnable task = () -> {
    System.out.println("Task is running");
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
            System.out.println("Task is running");
        };

        Thread thread = new Thread(task);

        thread.start();
    }
}
```

This is shorter and commonly used in modern Java.

---

# 🔹 Even Shorter Version

Because `Thread` has a constructor that accepts a `Runnable`, we can directly write:

```java
Thread thread = new Thread(() -> {
    System.out.println("Task is running");
});

thread.start();
```

This is a very common form.

---

# 🔹 start() vs run()

This is one of the most important concepts in Java multithreading.

## `start()`

```java
thread.start();
```

Requests the JVM to start a new thread of execution.

Conceptually:

```text
Current Thread
      │
      ├──────────────→ New Thread
      │                    ↓
      │                  run()
      │
      ↓
 continues execution
```

---

## `run()`

```java
thread.run();
```

is simply a normal method invocation.

It does **not** create a new thread.

Conceptually:

```text
Current Thread
      ↓
   run()
      ↓
task executes on current thread
```

---

# 🔹 Example: `start()`

```java
class MyThread extends Thread {

    @Override
    public void run() {
        System.out.println("Worker: " + Thread.currentThread().getName());
    }
}

class Main {

    public static void main(String[] args) {

        MyThread thread = new MyThread();

        thread.start();

        System.out.println("Main: " + Thread.currentThread().getName());
    }
}
```

The worker code executes on a separate thread.

The exact output order is not guaranteed.

---

# 🔹 Example: `run()`

```java
class MyThread extends Thread {

    @Override
    public void run() {
        System.out.println("Worker: " + Thread.currentThread().getName());
    }
}

class Main {

    public static void main(String[] args) {

        MyThread thread = new MyThread();

        thread.run();

        System.out.println("Main: " + Thread.currentThread().getName());
    }
}
```

Here `run()` executes on the main thread.

No new thread is created.

---

# 🔹 Why `start()` Creates a New Thread

The important difference is:

```text
start()
  ↓
asks JVM to start a separate execution path
  ↓
JVM schedules the thread
  ↓
run() gets executed by that thread
```

Whereas:

```text
run()
  ↓
ordinary method call
  ↓
current thread executes it
```

### Memory Trick

> **`start()` starts. `run()` runs.**

Or:

> **Never call `run()` when your intention is to create a new thread.**

---

# 🔹 Thread Object vs Runnable Task

This distinction is extremely important.

## Thread

```java
Thread thread = new Thread(task);
```

A `Thread` represents the execution mechanism.

## Runnable

```java
Runnable task = () -> {
    System.out.println("Work");
};
```

A `Runnable` represents the work/task.

Conceptually:

```text
Runnable
   ↓
What should be done?

Thread
   ↓
Which thread executes it?
```

This separation is one reason `Runnable` is generally preferred for simple tasks.

---

# 🔹 Anonymous Runnable

Before lambdas, an anonymous class could be used.

```java
Runnable task = new Runnable() {

    @Override
    public void run() {
        System.out.println("Task is running");
    }
};
```

Then:

```java
Thread thread = new Thread(task);

thread.start();
```

This works but is more verbose than a lambda.

---

# 🔹 Runnable Lambda Equivalent

Anonymous class:

```java
Runnable task = new Runnable() {

    @Override
    public void run() {
        System.out.println("Task is running");
    }
};
```

Lambda:

```java
Runnable task = () -> {
    System.out.println("Task is running");
};
```

Both represent the same basic task.

The lambda is simply more concise.

---

# 🔹 Creating Multiple Threads

We can create multiple threads from the same task.

```java
Runnable task = () -> {
    System.out.println(Thread.currentThread().getName());
};

Thread thread1 = new Thread(task);
Thread thread2 = new Thread(task);
Thread thread3 = new Thread(task);

thread1.start();
thread2.start();
thread3.start();
```

Each thread can execute the same task.

Conceptually:

```text
             Runnable Task
             /     |      \
            ↓      ↓       ↓
        Thread1 Thread2 Thread3
```

The output order is not guaranteed.

---

# 🔹 Thread Naming

A thread can have a name.

```java
Thread thread = new Thread(
    () -> System.out.println("Running"),
    "Worker-1"
);
```

Then:

```java
thread.start();
```

We can retrieve its name:

```java
System.out.println(thread.getName());
```

Output:

```text
Worker-1
```

---

# 🔹 Setting Thread Name

We can also set the name using:

```java
thread.setName("Worker-1");
```

Example:

```java
Thread thread = new Thread(() -> {
    System.out.println("Task running");
});

thread.setName("Worker-1");

thread.start();
```

---

# 🔹 Getting the Current Thread

Java provides:

```java
Thread.currentThread()
```

It returns the thread that is currently executing the code.

Example:

```java
class Main {

    public static void main(String[] args) {

        System.out.println(Thread.currentThread().getName());
    }
}
```

Typical output:

```text
main
```

Inside another thread:

```java
Thread thread = new Thread(() -> {
    System.out.println(Thread.currentThread().getName());
});

thread.start();
```

The output will normally be the worker thread's name.

---

# 🔹 Checking Thread State

We can use:

```java
getState()
```

Example:

```java
Thread thread = new Thread(() -> {
    System.out.println("Running");
});

System.out.println(thread.getState());

thread.start();
```

Before `start()`:

```text
NEW
```

After starting, the state can be:

```text
RUNNABLE
```

or potentially another state depending on timing.

---

# 🔹 Passing Data to a Thread

A task can access data available to it.

Example:

```java
class Main {

    public static void main(String[] args) {

        int number = 10;

        Thread thread = new Thread(() -> {
            System.out.println(number * 2);
        });

        thread.start();
    }
}
```

Output:

```text
20
```

The lambda can use the local variable because it is effectively final.

---

# 🔹 Passing Data Through a Runnable Class

We can also pass data through a constructor.

```java
class MyTask implements Runnable {

    private int number;

    MyTask(int number) {
        this.number = number;
    }

    @Override
    public void run() {
        System.out.println(number * 2);
    }
}
```

Then:

```java
class Main {

    public static void main(String[] args) {

        MyTask task = new MyTask(10);

        Thread thread = new Thread(task);

        thread.start();
    }
}
```

Output:

```text
20
```

---

# 🔹 Thread Constructor with Runnable

The `Thread` class provides constructors that accept a `Runnable`.

Common form:

```java
Thread thread = new Thread(task);
```

where:

```java
Runnable task = () -> {
    System.out.println("Work");
};
```

Then:

```java
thread.start();
```

The relationship is:

```text
Runnable object
       ↓
Thread constructor
       ↓
Thread object
       ↓
start()
       ↓
run()
```

---

# 🔹 Internal Working

Consider:

```java
Runnable task = () -> {
    System.out.println("Hello");
};

Thread thread = new Thread(task);

thread.start();
```

Conceptually:

```text
1. Create Runnable
        ↓
2. Create Thread
        ↓
3. Thread initially NEW
        ↓
4. Call start()
        ↓
5. JVM creates/starts execution for the thread
        ↓
6. Thread becomes RUNNABLE
        ↓
7. JVM invokes the task's run()
        ↓
8. Task executes
        ↓
9. Execution completes
        ↓
10. Thread becomes TERMINATED
```

---

# 🔹 What Does `Thread.start()` Ultimately Execute?

When we use:

```java
thread.start();
```

the thread eventually executes its `run()` method.

For a `Thread` subclass:

```java
class MyThread extends Thread {

    @Override
    public void run() {
        System.out.println("Work");
    }
}
```

the overridden `run()` is executed.

For a `Runnable`:

```java
Runnable task = () -> {
    System.out.println("Work");
};
```

the `Runnable` task's `run()` is executed by the thread.

---

# 🔹 JVM Perspective

The overall flow can be understood as:

```text
Java Code
   ↓
Thread.start()
   ↓
JVM / Java Runtime
   ↓
OS-level scheduling
   ↓
CPU executes thread
   ↓
run()
```

The scheduler determines when a runnable thread receives CPU execution time.

Therefore, you should never rely on a particular execution order unless you explicitly coordinate the threads.

---

# 🔹 Thread Lifecycle Connection

Thread creation connects directly to the lifecycle discussed in the previous topic.

When we write:

```java
Thread thread = new Thread();
```

the thread is:

```text
NEW
```

When we call:

```java
thread.start();
```

it moves toward:

```text
RUNNABLE
```

Then it may enter:

```text
BLOCKED
WAITING
TIMED_WAITING
```

and eventually:

```text
TERMINATED
```

So:

```text
Creating Thread
      ↓
NEW
      ↓
start()
      ↓
RUNNABLE
      ↓
Execution
      ↓
TERMINATED
```

---

# 🔹 Which Approach Should You Use?

There are two fundamental approaches:

| Approach | Mechanism |
|---|---|
| Extend `Thread` | Class becomes a thread |
| Implement `Runnable` | Class represents a task |

### Extending `Thread`

```java
class MyThread extends Thread {

    @Override
    public void run() {
        System.out.println("Work");
    }
}
```

### Implementing `Runnable`

```java
class MyTask implements Runnable {

    @Override
    public void run() {
        System.out.println("Work");
    }
}
```

For general task-based design, `Runnable` provides better separation between the task and the thread that executes it.

Also, Java supports single class inheritance.

If you extend `Thread`:

```java
class MyClass extends Thread {
}
```

you cannot extend another class.

With `Runnable`:

```java
class MyClass extends SomeClass implements Runnable {
}
```

you can still extend another class.

This is a major reason the `Runnable` approach is commonly preferred when directly creating threads.

---

# 🔹 Common Mistakes

## ❌ Mistake 1: Calling `run()` instead of `start()`

Wrong when you want a new thread:

```java
thread.run();
```

Correct:

```java
thread.start();
```

---

## ❌ Mistake 2: Calling `start()` twice

Example:

```java
Thread thread = new Thread(() -> {
    System.out.println("Hello");
});

thread.start();
thread.start();
```

This causes:

```text
IllegalThreadStateException
```

A `Thread` object can be started only once.

---

## ❌ Mistake 3: Thinking Thread and Runnable are the same

They are related but conceptually different.

```text
Runnable → task
Thread   → execution mechanism
```

---

## ❌ Mistake 4: Assuming thread execution order

Example:

```java
thread1.start();
thread2.start();
```

does not guarantee:

```text
Thread 1
Thread 2
```

The scheduler controls execution.

---

## ❌ Mistake 5: Assuming `start()` immediately executes the thread

Calling:

```java
thread.start();
```

makes the thread eligible to run.

It does not mean the thread executes at that exact moment.

---

# 🔹 Interview Traps

### Trap 1: What is the difference between `start()` and `run()`?

`start()` initiates a new thread of execution.

`run()` is the method containing the task and calling it directly does not create a new thread.

---

### Trap 2: Can we call `start()` twice?

No.

It results in:

```text
IllegalThreadStateException
```

---

### Trap 3: Can we call `run()` twice?

Yes.

It is just a normal method call when invoked directly.

However, it does not create a new thread.

---

### Trap 4: Which is generally more flexible: extending Thread or implementing Runnable?

Implementing `Runnable` is generally more flexible because the task is separated from the thread and the class can still extend another class.

---

### Trap 5: Does `start()` directly call `run()` like a normal method?

Conceptually, the new thread eventually executes `run()`, but `start()` itself is not equivalent to directly invoking `run()`.

---

### Trap 6: What does `Thread.currentThread()` return?

It returns the `Thread` object representing the thread currently executing the code.

---

### Trap 7: What does `getName()` return?

It returns the name of the thread.

Example:

```java
Thread thread = new Thread();

thread.setName("Worker");

System.out.println(thread.getName());
```

Output:

```text
Worker
```

---

# 🔹 DSA / Problem-Solving Relevance

Creating threads is not itself a DSA pattern.

However, thread creation becomes useful when studying concurrency problems such as:

- Producer-Consumer
- Thread-safe counters
- Alternate printing
- Ordered execution
- Concurrent data processing
- Synchronization problems

The important mental model is:

```text
Task
 ↓
Runnable
 ↓
Thread
 ↓
start()
 ↓
Concurrent execution
```

---

# 🔹 30-Second Interview Answer

> Java provides multiple ways to create threads. Traditionally, we can extend the `Thread` class or implement the `Runnable` interface. With `Runnable`, the task is separated from the thread that executes it, which provides better flexibility because Java supports single inheritance. After creating a `Thread`, we call `start()` to initiate a new thread of execution. Calling `run()` directly does not create a new thread; it behaves like a normal method call. Modern Java also commonly uses lambda expressions with `Runnable` for concise task definitions.

---

# 🔹 Cheat Sheet

| Concept | Meaning |
|---|---|
| `Thread` | Represents a thread of execution |
| `Runnable` | Represents a task to be executed |
| `run()` | Contains the task logic |
| `start()` | Starts a new thread |
| `currentThread()` | Returns currently executing thread |
| `getName()` | Gets thread name |
| `setName()` | Sets thread name |
| `getState()` | Gets current thread state |
| `NEW` | Thread created but not started |
| `RUNNABLE` | Thread ready/running |
| `TERMINATED` | Thread execution completed |

---

# 🔥 Core Difference

```text
Extending Thread

MyThread
   ↓
extends Thread
   ↓
override run()
   ↓
start()
```

```text
Implementing Runnable

MyTask
   ↓
implements Runnable
   ↓
override run()
   ↓
new Thread(task)
   ↓
start()
```

---

# 🧠 Memory Trick

Remember:

> **Runnable = WHAT to do**

> **Thread = WHO/WHAT executes it**

And:

> **`start()` = start a new execution path**

> **`run()` = execute the task method**

---

# 🔥 Top 10 Interview Questions

## 1. How can you create a thread in Java?

Two traditional approaches are:

```text
1. Extend Thread
2. Implement Runnable
```

Modern Java also commonly uses lambda expressions with `Runnable`.

---

## 2. What is the difference between `start()` and `run()`?

```text
start()
→ starts a new thread of execution

run()
→ normal method execution when called directly
```

---

## 3. Why is `Runnable` often preferred over extending `Thread`?

Because it separates the task from the execution mechanism and allows the class to extend another class.

---

## 4. Can a class extend Thread and another class?

No.

Java supports single class inheritance.

---

## 5. Can a class extend another class and implement Runnable?

Yes.

```java
class MyClass extends Parent implements Runnable {

    @Override
    public void run() {
        System.out.println("Work");
    }
}
```

---

## 6. What happens when `start()` is called?

The thread becomes eligible for execution and the JVM eventually executes its `run()` method on that thread.

---

## 7. What happens if `run()` is called directly?

It executes like an ordinary method call on the current thread.

No new thread is created.

---

## 8. Can `start()` be called twice?

No.

It results in:

```text
IllegalThreadStateException
```

---

## 9. Can `run()` be called multiple times directly?

Yes.

But each direct call is just a normal method call and does not create a new thread.

---

## 10. What is the purpose of `Runnable`?

`Runnable` represents a task whose work is defined inside its:

```java
run()
```

method.

A `Thread` can then execute that task.

---

# 🎯 Final Summary

```text
                 CREATING THREADS
                        │
             ┌──────────┴──────────┐
             ↓                     ↓
       Extend Thread         Implement Runnable
             │                     │
             ↓                     ↓
        override run()        implement run()
             │                     │
             ↓                     ↓
         start()              new Thread(task)
                                   │
                                   ↓
                                 start()
```

### Most important points

- `Thread` represents an execution thread.
- `Runnable` represents a task.
- Override `run()` to define the work.
- Call `start()` to start a new thread.
- Calling `run()` directly does **not** create a new thread.
- A thread can be started only once.
- `Runnable` provides better separation of task and execution.
- Lambda expressions can make `Runnable` concise.
- `Thread.currentThread()` gives the currently executing thread.
- `getName()` gets a thread's name.
- `getState()` gives its current lifecycle state.

> **Define the task → create the thread → call `start()` → let the JVM schedule the execution.**