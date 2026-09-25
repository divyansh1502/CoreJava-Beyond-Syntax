# 04 — Thread Class

> **`Thread` is a Java class used to create, control, and manage threads of execution.**

---

## 📌 Table of Contents

1. [Introduction](#-introduction)
2. [What is a Thread?](#-what-is-a-thread)
3. [Thread Class](#-thread-class)
4. [Creating a Thread by Extending Thread](#-creating-a-thread-by-extending-thread)
5. [run() Method](#-run-method)
6. [start() Method](#-start-method)
7. [start() vs run()](#-start-vs-run)
8. [Thread Name](#-thread-name)
9. [Thread ID](#-thread-id)
10. [currentThread()](#-currentthread)
11. [isAlive()](#-isalive)
12. [join()](#-join)
13. [sleep()](#-sleep)
14. [interrupt()](#-interrupt)
15. [Thread Priority](#-thread-priority)
16. [Daemon Thread](#-daemon-thread)
17. [setDaemon()](#-setdaemon)
18. [Thread State](#-thread-state)
19. [Important Thread Methods](#-important-thread-methods)
20. [Common Mistakes](#-common-mistakes)
21. [Interview Traps](#-interview-traps)
22. [DSA / Problem-Solving Relevance](#-dsa--problem-solving-relevance)
23. [30-Second Interview Answer](#-30-second-interview-answer)
24. [Cheat Sheet](#-cheat-sheet)
25. [Top 10 Interview Questions](#-top-10-interview-questions)

---

# 🔹 Introduction

Java provides the `Thread` class for creating and controlling threads.

The class belongs to:

```java
java.lang.Thread
```

Since `java.lang` is automatically imported, we normally write:

```java
Thread t = new Thread();
```

A thread represents an independent path of execution inside a process.

A Java application can have multiple threads running concurrently.

---

# 🔹 What is a Thread?

A **thread** is a lightweight unit of execution inside a process.

For example:

```text
Process
   |
   |--- Thread 1
   |
   |--- Thread 2
   |
   |--- Thread 3
```

Every Java application starts with a thread that executes the `main()` method.

Example:

```java
public class Main {

    public static void main(String[] args) {

        System.out.println("Main thread");
    }
}
```

The thread executing `main()` is commonly called the **main thread**.

---

# 🔹 Thread Class

The fully qualified name of the class is:

```java
java.lang.Thread
```

The `Thread` class is part of the Java standard library.

It implements:

```java
Runnable
```

Conceptually:

```text
Thread
   |
   implements
   |
Runnable
```

The `Thread` class provides methods for:

- Starting threads
- Waiting for threads
- Sleeping
- Interrupting threads
- Checking thread state
- Getting thread ID
- Setting thread name
- Setting thread priority
- Creating daemon threads

---

# 🔹 Creating a Thread by Extending Thread

One way to create a thread is by extending the `Thread` class.

Example:

```java
class MyThread extends Thread {

    @Override
    public void run() {

        System.out.println("Thread is running");
    }
}
```

Then create and start it:

```java
class Main {

    public static void main(String[] args) {

        MyThread t = new MyThread();

        t.start();
    }
}
```

Output:

```text
Thread is running
```

Here:

```java
MyThread t = new MyThread();
```

creates a `MyThread` object.

Then:

```java
t.start();
```

requests that the JVM start a new thread of execution.

---

# 🔹 Internal Flow

The basic flow is:

```text
new MyThread()
       ↓
Thread object created
       ↓
start()
       ↓
New thread begins execution
       ↓
run()
       ↓
Task executes
       ↓
Thread terminates
```

Important:

> `start()` starts a new thread of execution. `run()` contains the task that the thread executes.

---

# 🔹 run() Method

The `run()` method contains the code that the thread executes.

Example:

```java
class MyThread extends Thread {

    @Override
    public void run() {

        System.out.println("Task is running");
    }
}
```

The method is inherited from `Thread`.

Its basic signature is:

```java
public void run()
```

We override it to define the work performed by our thread.

---

# 🔹 start() Method

The `start()` method is used to start a new thread.

Example:

```java
class MyThread extends Thread {

    @Override
    public void run() {

        System.out.println("Running in another thread");
    }
}

class Main {

    public static void main(String[] args) {

        MyThread thread = new MyThread();

        thread.start();
    }
}
```

Conceptually:

```text
start()
   ↓
new thread is started
   ↓
run()
   ↓
task executes
```

---

# 🔹 start() vs run()

This is one of the most important Thread interview concepts.

### Calling `start()`

```java
Thread t = new MyThread();

t.start();
```

`start()` asks the JVM to start a new thread.

The new thread eventually invokes:

```java
run();
```

---

### Calling `run()` directly

```java
Thread t = new MyThread();

t.run();
```

This does **not** start a new thread.

It is simply a normal method call executed by the thread that called it.

Comparison:

```text
t.start()

Main Thread
     |
     +----> starts MyThread
                  |
                  +----> run()
```

Whereas:

```text
t.run()

Main Thread
     |
     +----> run()
```

### Memory Trick

> **`start()` = start a thread**

> **`run()` = execute the task**

---

# 🔹 Can start() Be Called Twice?

No.

A thread can be started only once.

Example:

```java
MyThread t = new MyThread();

t.start();
t.start();
```

This causes:

```text
java.lang.IllegalThreadStateException
```

The same `Thread` object cannot be restarted after it has already been started.

If another thread is required, create another `Thread` object.

Example:

```java
MyThread t1 = new MyThread();
MyThread t2 = new MyThread();

t1.start();
t2.start();
```

---

# 🔹 Thread Name

Every thread has a name.

We can set the name using:

```java
setName()
```

Example:

```java
class Main {

    public static void main(String[] args) {

        Thread t = new Thread();

        t.setName("Worker-Thread");

        System.out.println(t.getName());
    }
}
```

Output:

```text
Worker-Thread
```

---

# 🔹 Getting Thread Name

Use:

```java
getName()
```

Example:

```java
class MyThread extends Thread {

    @Override
    public void run() {

        System.out.println(Thread.currentThread().getName());
    }
}

class Main {

    public static void main(String[] args) {

        MyThread t = new MyThread();

        t.setName("Backend-Worker");

        t.start();
    }
}
```

Possible output:

```text
Backend-Worker
```

---

# 🔹 Thread ID

Every thread has an ID.

Use:

```java
getId()
```

Example:

```java
class Main {

    public static void main(String[] args) {

        Thread t = Thread.currentThread();

        System.out.println(t.getId());
    }
}
```

The ID is a unique identifier for the thread during its lifetime.

---

# 🔹 currentThread()

`currentThread()` returns the thread that is currently executing the code.

Example:

```java
class Main {

    public static void main(String[] args) {

        Thread t = Thread.currentThread();

        System.out.println(t.getName());
    }
}
```

Usually the output is:

```text
main
```

because the `main()` method is initially executed by the main thread.

---

# 🔹 currentThread() with Custom Thread

Example:

```java
class MyThread extends Thread {

    @Override
    public void run() {

        Thread current = Thread.currentThread();

        System.out.println(current.getName());
    }
}

class Main {

    public static void main(String[] args) {

        MyThread t = new MyThread();

        t.setName("Worker");

        t.start();
    }
}
```

Output:

```text
Worker
```

Important:

```java
Thread.currentThread()
```

means:

> Give me the `Thread` object representing the thread currently executing this code.

---

# 🔹 isAlive()

The `isAlive()` method checks whether a thread has been started and has not yet terminated.

It returns:

```java
boolean
```

Example:

```java
class MyThread extends Thread {

    @Override
    public void run() {

        System.out.println("Running...");
    }
}

class Main {

    public static void main(String[] args) {

        MyThread t = new MyThread();

        System.out.println(t.isAlive());

        t.start();

        System.out.println(t.isAlive());
    }
}
```

Before `start()`:

```text
false
```

After starting, it may be:

```text
true
```

if the thread is still running when checked.

After the thread terminates:

```text
false
```

---

# 🔹 join()

`join()` makes the current thread wait for another thread to terminate.

Example:

```java
class MyThread extends Thread {

    @Override
    public void run() {

        for(int i = 1; i <= 5; i++) {

            System.out.println(i);
        }
    }
}

class Main {

    public static void main(String[] args)
            throws InterruptedException {

        MyThread t = new MyThread();

        t.start();

        t.join();

        System.out.println("Main thread continues");
    }
}
```

Flow:

```text
Main Thread
    |
    +---- start MyThread
    |
    +---- join()
             |
             ↓
        waits for MyThread
             |
             ↓
        MyThread finishes
             |
             ↓
        Main continues
```

---

# 🔹 sleep()

`Thread.sleep()` pauses the currently executing thread for a specified duration.

Example:

```java
class Main {

    public static void main(String[] args)
            throws InterruptedException {

        System.out.println("Start");

        Thread.sleep(2000);

        System.out.println("End");
    }
}
```

The thread pauses for approximately:

```text
2000 milliseconds
```

which is:

```text
2 seconds
```

Important:

> `sleep()` pauses the current thread.

---

# 🔹 sleep() Does Not Release a Monitor

If a thread is inside a synchronized block and calls `sleep()`, it does not release the intrinsic monitor merely because it is sleeping.

Example:

```java
synchronized(lock) {

    Thread.sleep(2000);
}
```

Conceptually:

```text
Acquire lock
     ↓
sleep()
     ↓
Still owns lock
     ↓
Wake up
     ↓
Continue
     ↓
Release lock
```

This is an important interview point.

---

# 🔹 interrupt()

`interrupt()` is used to request interruption of a thread.

It does not forcibly kill the thread.

Example:

```java
class MyThread extends Thread {

    @Override
    public void run() {

        try {

            Thread.sleep(5000);

        } catch(InterruptedException e) {

            System.out.println("Thread was interrupted");
        }
    }
}

class Main {

    public static void main(String[] args) {

        MyThread t = new MyThread();

        t.start();

        t.interrupt();
    }
}
```

If the thread is sleeping, the sleep can be interrupted and an `InterruptedException` can be thrown.

---

# 🔹 Important Point About interrupt()

`interrupt()` does not mean:

```text
KILL THREAD
```

It means:

```text
REQUEST INTERRUPTION
```

The thread should respond appropriately to the interruption.

---

# 🔹 Thread Priority

Java threads have a priority value.

The valid range is:

```text
1 → MIN_PRIORITY
5 → NORM_PRIORITY
10 → MAX_PRIORITY
```

Constants:

```java
Thread.MIN_PRIORITY
Thread.NORM_PRIORITY
Thread.MAX_PRIORITY
```

Example:

```java
class Main {

    public static void main(String[] args) {

        Thread t = new Thread();

        t.setPriority(Thread.MAX_PRIORITY);

        System.out.println(t.getPriority());
    }
}
```

Output:

```text
10
```

---

# 🔹 Important Priority Point

Thread priority is a scheduling hint.

It does **not** guarantee that a higher-priority thread will always execute first.

Do not write logic that depends on a specific scheduling order based solely on priority.

---

# 🔹 Daemon Thread

A daemon thread is a background thread that does not keep the JVM alive by itself after all non-daemon threads have terminated.

Example:

```java
class Worker extends Thread {

    @Override
    public void run() {

        while(true) {

            System.out.println("Background task");
        }
    }
}

class Main {

    public static void main(String[] args) {

        Worker worker = new Worker();

        worker.setDaemon(true);

        worker.start();

        System.out.println("Main finished");
    }
}
```

Once the main thread and all other non-daemon threads finish, the JVM can terminate even if the daemon thread is still running.

---

# 🔹 setDaemon()

Use:

```java
setDaemon(true)
```

to mark a thread as daemon.

Example:

```java
Thread t = new Thread();

t.setDaemon(true);

t.start();
```

Important:

> `setDaemon(true)` must be called before the thread is started.

Calling it after `start()` causes:

```text
IllegalThreadStateException
```

Example:

```java
Thread t = new Thread();

t.start();

t.setDaemon(true);
```

This is invalid.

---

# 🔹 Thread State

A thread has a lifecycle represented by:

```java
Thread.State
```

The major states are:

```text
NEW
RUNNABLE
BLOCKED
WAITING
TIMED_WAITING
TERMINATED
```

---

## NEW

The thread object has been created but `start()` has not been called.

Example:

```java
Thread t = new Thread();
```

State:

```text
NEW
```

---

## RUNNABLE

The thread has been started and is eligible to run.

Example:

```java
t.start();
```

The JVM scheduler determines when it actually executes.

---

## BLOCKED

A thread is waiting to acquire an intrinsic monitor lock.

Example:

```java
synchronized(lock) {

    // critical section
}
```

If another thread owns `lock`, the waiting thread can enter the `BLOCKED` state while waiting for that monitor.

---

## WAITING

A thread waits indefinitely for another thread or event.

Examples include certain uses of:

```java
join()
```

and:

```java
wait()
```

---

## TIMED_WAITING

A thread waits for a specified amount of time.

Examples:

```java
Thread.sleep(1000);
```

and timed versions of waiting methods.

---

## TERMINATED

The thread has completed execution.

Example:

```text
run()
  ↓
task completes
  ↓
TERMINATED
```

---

# 🔹 Getting Thread State

Use:

```java
getState()
```

Example:

```java
class Main {

    public static void main(String[] args) {

        Thread t = new Thread();

        System.out.println(t.getState());

        t.start();

        System.out.println(t.getState());
    }
}
```

The exact state observed after `start()` can depend on timing.

---

# 🔹 Important Thread Methods

| Method | Purpose |
|---|---|
| `start()` | Starts a new thread |
| `run()` | Contains thread task |
| `currentThread()` | Returns currently executing thread |
| `getName()` | Gets thread name |
| `setName()` | Sets thread name |
| `getId()` | Gets thread ID |
| `isAlive()` | Checks whether thread has not terminated |
| `join()` | Waits for another thread to terminate |
| `sleep()` | Pauses current thread |
| `interrupt()` | Requests interruption |
| `isInterrupted()` | Checks interruption status |
| `getPriority()` | Gets priority |
| `setPriority()` | Sets priority |
| `getState()` | Gets thread state |
| `setDaemon()` | Marks thread as daemon |
| `isDaemon()` | Checks daemon status |

---

# 🔹 Common Mistakes

## ❌ Mistake 1 — Calling run() instead of start()

Wrong when you want a new thread:

```java
t.run();
```

Correct:

```java
t.start();
```

---

## ❌ Mistake 2 — Starting the same thread twice

Wrong:

```java
Thread t = new Thread();

t.start();
t.start();
```

This throws:

```text
IllegalThreadStateException
```

---

## ❌ Mistake 3 — Setting daemon after start()

Wrong:

```java
Thread t = new Thread();

t.start();

t.setDaemon(true);
```

Daemon status must be set before starting the thread.

---

## ❌ Mistake 4 — Assuming thread priority guarantees execution order

Priority is not a guarantee of execution order.

---

## ❌ Mistake 5 — Thinking interrupt() kills a thread

It does not forcibly terminate the thread.

It is a cooperative interruption mechanism.

---

# 🔹 Interview Traps

### 1. What is the difference between start() and run()?

`start()` starts a new thread of execution.

`run()` is the method containing the task.

Calling `run()` directly is just a normal method call.

---

### 2. Can we call start() twice?

No.

It causes `IllegalThreadStateException`.

---

### 3. Does sleep() create a new thread?

No.

It pauses the currently executing thread.

---

### 4. Does sleep() release a synchronized lock?

No.

---

### 5. Does interrupt() terminate a thread?

No.

It requests interruption.

---

### 6. Can a daemon thread keep the JVM alive?

No.

The JVM can terminate when all non-daemon threads have finished.

---

### 7. Can setDaemon() be called after start()?

No.

It must be called before the thread is started.

---

### 8. What does currentThread() return?

It returns the `Thread` object representing the thread currently executing the code.

---

# 🔹 DSA / Problem-Solving Relevance

The `Thread` class is useful when understanding concurrent DSA problems such as:

- Producer-consumer
- Concurrent queues
- Shared counters
- Parallel searching
- Parallel processing
- Concurrent data structures
- Multithreaded sorting

A useful mental model is:

```text
Problem
   ↓
Can work be divided?
   ↓
Independent tasks?
   ↓
Create threads
   ↓
Execute concurrently
   ↓
Coordinate results
```

However, simply creating more threads does not automatically make an algorithm faster.

Thread creation, scheduling, synchronization, communication, and hardware limits all affect performance.

---

# 🔹 30-Second Interview Answer

> `Thread` is a class in `java.lang` used to create and manage threads in Java. We can create a thread by extending `Thread` and overriding its `run()` method, then call `start()` to begin a new thread of execution. The class also provides methods such as `sleep()`, `join()`, `interrupt()`, `getName()`, `getState()`, and `setDaemon()`. An important distinction is that calling `run()` directly does not create a new thread, while `start()` does.

---

# 🔹 Cheat Sheet

## Create Thread

```java
class MyThread extends Thread {

    @Override
    public void run() {

        System.out.println("Running");
    }
}
```

---

## Start Thread

```java
MyThread t = new MyThread();

t.start();
```

---

## Current Thread

```java
Thread current = Thread.currentThread();
```

---

## Thread Name

```java
t.setName("Worker");

System.out.println(t.getName());
```

---

## Thread ID

```java
System.out.println(t.getId());
```

---

## Check Alive

```java
System.out.println(t.isAlive());
```

---

## Wait for Thread

```java
t.join();
```

---

## Sleep

```java
Thread.sleep(1000);
```

---

## Interrupt

```java
t.interrupt();
```

---

## Priority

```java
t.setPriority(Thread.MAX_PRIORITY);
```

---

## Daemon

```java
t.setDaemon(true);

t.start();
```

---

## State

```java
System.out.println(t.getState());
```

---

# 🧠 Memory Tricks

### `start()`

> **Start = New Thread**

### `run()`

> **Run = Task**

### `sleep()`

> **Sleep = Pause Current Thread**

### `join()`

> **Join = Wait for Another Thread**

### `interrupt()`

> **Interrupt = Request It to Stop Waiting/Blocking or Respond to Interruption**

### `currentThread()`

> **Current = Who is Running Me?**

### `isAlive()`

> **Alive = Started but Not Yet Terminated**

### `setDaemon(true)`

> **Background Thread**

---

# 🔥 Top 10 Interview Questions

## 1. What is the Thread class?

`Thread` is a Java class in `java.lang` used to create and manage threads.

---

## 2. How do you create a thread by extending Thread?

Override `run()` in a subclass and call `start()` on its object.

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

---

## 3. What is the difference between start() and run()?

```text
start()
   ↓
Starts a new thread
   ↓
run()
```

Whereas:

```text
run()
   ↓
Normal method call
```

---

## 4. Can start() be called twice?

No.

Calling it twice on the same `Thread` object throws `IllegalThreadStateException`.

---

## 5. What does currentThread() do?

It returns the `Thread` object representing the currently executing thread.

---

## 6. What does join() do?

It causes the current thread to wait until the target thread terminates.

---

## 7. What does sleep() do?

It pauses the currently executing thread for a specified amount of time.

---

## 8. Does sleep() release a lock?

No.

If the thread owns an intrinsic monitor, sleeping does not release that monitor.

---

## 9. What is a daemon thread?

A daemon thread is a background thread that does not by itself prevent the JVM from terminating after all non-daemon threads have finished.

---

## 10. What is the difference between Thread and Runnable?

`Thread` represents the thread itself and provides thread-control functionality.

`Runnable` represents a task that can be executed by a thread.

Example:

```java
Runnable task = () -> {

    System.out.println("Task running");
};

Thread t = new Thread(task);

t.start();
```

Using `Runnable` separates:

```text
Task
  ↓
Runnable

Thread
  ↓
Executes task
```

This is generally more flexible than extending `Thread`, especially because Java classes can extend only one class.

---

# 🎯 Final Summary

The most important concepts from `Thread` are:

```text
Thread
  |
  +── start()
  |      ↓
  |   starts new thread
  |
  +── run()
  |      ↓
  |   contains task
  |
  +── sleep()
  |      ↓
  |   pauses current thread
  |
  +── join()
  |      ↓
  |   waits for another thread
  |
  +── interrupt()
  |      ↓
  |   requests interruption
  |
  +── currentThread()
  |      ↓
  |   returns current thread
  |
  +── getState()
  |      ↓
  |   returns thread state
  |
  +── setDaemon()
         ↓
      marks daemon thread
```

### ⭐ Most Important Interview Points

```text
start() ≠ run()

start()
→ starts a new thread

run()
→ normal method if called directly

sleep()
→ pauses current thread
→ does NOT release monitor

join()
→ waits for another thread

interrupt()
→ requests interruption
→ does NOT forcibly kill thread

start() twice
→ IllegalThreadStateException

setDaemon(true)
→ must be called before start()

Thread.currentThread()
→ returns currently executing thread
```

> **Core idea: `Thread` gives Java the basic API to create, start, inspect, coordinate, and control threads.**