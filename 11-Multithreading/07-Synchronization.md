# 07 — Synchronization

> **Synchronization controls access to shared resources so that multiple threads can safely work with shared mutable data.**

---

## 📌 Table of Contents

1. [Introduction](#-introduction)
2. [Why Synchronization?](#-why-synchronization)
3. [Shared Resource](#-shared-resource)
4. [Race Condition](#-race-condition)
5. [Critical Section](#-critical-section)
6. [What is Synchronization?](#-what-is-synchronization)
7. [synchronized Keyword](#-synchronized-keyword)
8. [Synchronized Instance Method](#-synchronized-instance-method)
9. [Synchronized Block](#-synchronized-block)
10. [Object Monitor / Intrinsic Lock](#-object-monitor--intrinsic-lock)
11. [How synchronized Works Internally](#-how-synchronized-works-internally)
12. [Same Object Lock](#-same-object-lock)
13. [Different Object Locks](#-different-object-locks)
14. [Static Synchronization](#-static-synchronization)
15. [Class-Level Lock](#-class-level-lock)
16. [Instance vs Static Synchronization](#-instance-vs-static-synchronization)
17. [Synchronized Method vs Block](#-synchronized-method-vs-block)
18. [Reentrant Synchronization](#-reentrant-synchronization)
19. [Synchronization and sleep()](#-synchronization-and-sleep)
20. [Synchronization and join()](#-synchronization-and-join)
21. [Synchronization and Memory Visibility](#-synchronization-and-memory-visibility)
22. [Advantages](#-advantages)
23. [Disadvantages](#-disadvantages)
24. [Common Mistakes](#-common-mistakes)
25. [Interview Traps](#-interview-traps)
26. [DSA / Problem-Solving Relevance](#-dsa--problem-solving-relevance)
27. [30-Second Interview Answer](#-30-second-interview-answer)
28. [Cheat Sheet](#-cheat-sheet)
29. [Top 10 Interview Questions](#-top-10-interview-questions)

---

# 🔹 Introduction

When multiple threads access the same mutable data, problems can occur.

Example:

```text
Thread 1
   |
   ↓
Shared Resource
   ↑
   |
Thread 2
```

If both threads modify the shared data at the same time, the final result may become incorrect.

Java provides the:

```java
synchronized
```

keyword to coordinate access to critical sections.

The main goals of synchronization are:

- Prevent race conditions
- Provide mutual exclusion
- Maintain consistency of shared data
- Provide important memory-visibility guarantees

---

# 🔹 Why Synchronization?

Consider a simple counter:

```java
class Counter {

    int count = 0;

    void increment() {

        count++;
    }
}
```

Now suppose two threads execute:

```java
count++;
```

at the same time.

It looks like one operation, but conceptually it involves multiple steps:

```text
Read count
    ↓
Add 1
    ↓
Write count
```

Suppose:

```text
Initial count = 0
```

Thread 1:

```text
Read 0
```

Thread 2:

```text
Read 0
```

Thread 1:

```text
Write 1
```

Thread 2:

```text
Write 1
```

Final result:

```text
1
```

But we expected:

```text
2
```

This is a classic race-condition scenario.

---

# 🔹 Shared Resource

A **shared resource** is data or an object that can be accessed by multiple threads.

Examples:

- Counter
- Bank account
- Array
- Collection
- File
- Database record
- Object fields

Example:

```java
class Counter {

    int count = 0;
}
```

Suppose multiple threads use the same object:

```java
Counter counter = new Counter();

Thread t1 = new Thread(() -> counter.count++);
Thread t2 = new Thread(() -> counter.count++);
```

Both threads access:

```java
counter.count
```

Therefore, `count` is shared mutable state.

---

# 🔹 Race Condition

A **race condition** occurs when multiple threads access shared data concurrently and the final result depends on the timing or ordering of their operations.

Example:

```java
class Counter {

    int count = 0;

    void increment() {

        count++;
    }
}
```

Now:

```java
Counter counter = new Counter();

Thread t1 = new Thread(counter::increment);
Thread t2 = new Thread(counter::increment);

t1.start();
t2.start();
```

Both threads modify the same variable.

The operation:

```java
count++;
```

is not guaranteed to behave as one indivisible operation.

---

# 🔹 Critical Section

A **critical section** is a part of code that accesses shared mutable state and needs controlled access.

Example:

```java
void increment() {

    count++;
}
```

The critical section is:

```java
count++;
```

Conceptually:

```text
Thread 1 ─────┐
              ↓
          Critical
           Section
              ↓
Thread 2 ─────┘
```

We can protect the critical section using synchronization.

---

# 🔹 What is Synchronization?

Synchronization is a mechanism that controls access to shared resources when multiple threads are executing concurrently.

The basic idea is:

```text
Multiple Threads
       ↓
Shared Resource
       ↓
Synchronization
       ↓
Controlled Access
```

For a synchronized critical section:

```text
Thread 1 → enters
             ↓
        Critical Section
             ↓
         exits

Thread 2 → waits
             ↓
        enters later
```

This provides **mutual exclusion**.

---

# 🔹 synchronized Keyword

Java provides the:

```java
synchronized
```

keyword.

It can be used with:

- Instance methods
- Static methods
- Blocks

Example:

```java
synchronized void increment() {

    count++;
}
```

Or:

```java
synchronized(this) {

    count++;
}
```

The exact lock used depends on where synchronization is applied.

---

# 🔹 Synchronized Instance Method

A method can be declared:

```java
synchronized
```

Example:

```java
class Counter {

    private int count = 0;

    synchronized void increment() {

        count++;
    }
}
```

Now only one thread at a time can execute this synchronized method on the **same object**.

---

# 🔹 Example

```java
class Counter {

    private int count = 0;

    synchronized void increment() {

        count++;
    }

    int getCount() {

        return count;
    }
}
```

Create one shared object:

```java
class Main {

    public static void main(String[] args)
            throws InterruptedException {

        Counter counter = new Counter();

        Thread t1 = new Thread(() -> {

            for(int i = 0; i < 1000; i++) {

                counter.increment();
            }
        });

        Thread t2 = new Thread(() -> {

            for(int i = 0; i < 1000; i++) {

                counter.increment();
            }
        });

        t1.start();
        t2.start();

        t1.join();
        t2.join();

        System.out.println(counter.getCount());
    }
}
```

Expected result:

```text
2000
```

The exact scheduling of the threads is not important for the final count because access to `increment()` is synchronized.

---

# 🔹 How Synchronized Instance Method Works

For:

```java
synchronized void increment() {

    count++;
}
```

the lock is associated with the current object.

Conceptually:

```text
Counter object
      ↓
Intrinsic lock
      ↓
increment()
```

If:

```java
Counter counter = new Counter();
```

then a synchronized instance method on `counter` uses the lock associated with that object.

---

# 🔹 Synchronized Block

Instead of synchronizing an entire method, we can synchronize only a particular section.

Syntax:

```java
synchronized(lockObject) {

    // critical section
}
```

Example:

```java
class Counter {

    private int count = 0;

    void increment() {

        synchronized(this) {

            count++;
        }
    }
}
```

Only:

```java
count++;
```

is protected.

---

# 🔹 Why Use a Synchronized Block?

Suppose a method contains a lot of code:

```java
void process() {

    // non-critical work

    // more non-critical work

    // shared data access

    // more non-critical work
}
```

We may only need synchronization around the shared resource.

Instead of:

```java
synchronized void process() {

    // everything is locked
}
```

we can use:

```java
void process() {

    // non-critical work

    synchronized(this) {

        // critical section
    }

    // more non-critical work
}
```

This can reduce the amount of code that must execute while holding the lock.

---

# 🔹 Object Monitor / Intrinsic Lock

Every Java object can be associated with an **intrinsic lock**, also called a **monitor**.

When a thread enters:

```java
synchronized(object) {

    // critical section
}
```

it attempts to acquire that object's monitor.

Conceptually:

```text
Object
  |
  ↓
Intrinsic Lock / Monitor
  |
  ↓
Thread acquires lock
  |
  ↓
Enters synchronized block
```

When the thread exits the synchronized block, the monitor is released.

---

# 🔹 How synchronized Works Internally

Consider:

```java
synchronized(lock) {

    count++;
}
```

Conceptually:

```text
Thread
   ↓
Try to acquire lock
   ↓
Is lock available?
   |
   +---- YES ----→ Acquire lock
   |                  ↓
   |             Execute code
   |                  ↓
   |              Release lock
   |
   +---- NO ----→ Wait until available
```

Only one thread can own a particular intrinsic lock at a time.

---

# 🔹 Same Object Lock

Consider:

```java
class Counter {

    synchronized void increment() {

        System.out.println("Increment");
    }

    synchronized void decrement() {

        System.out.println("Decrement");
    }
}
```

Suppose:

```java
Counter counter = new Counter();

Thread t1 = new Thread(counter::increment);
Thread t2 = new Thread(counter::decrement);
```

Both methods synchronize on the same `counter` object.

Therefore:

```text
counter
   |
   ↓
one intrinsic lock
   |
   +---- increment()
   |
   +---- decrement()
```

If one thread owns the lock, another thread must wait before entering another synchronized instance method on the same object.

---

# 🔹 Different Object Locks

Now consider:

```java
Counter c1 = new Counter();
Counter c2 = new Counter();
```

These are two different objects.

Therefore:

```text
c1 → Lock 1

c2 → Lock 2
```

A synchronized instance method on `c1` does not use the same intrinsic lock as a synchronized instance method on `c2`.

Example:

```java
Counter c1 = new Counter();
Counter c2 = new Counter();

Thread t1 = new Thread(c1::increment);
Thread t2 = new Thread(c2::increment);

t1.start();
t2.start();
```

The two threads can potentially execute their synchronized methods concurrently because they are locking different objects.

---

# 🔹 Static Synchronization

A static method can also be synchronized.

Example:

```java
class Counter {

    private static int count = 0;

    static synchronized void increment() {

        count++;
    }
}
```

Here the lock is associated with the `Class` object rather than an individual instance.

Conceptually:

```text
Counter.class
     ↓
Class-level lock
     ↓
static synchronized method
```

---

# 🔹 Class-Level Lock

For a static synchronized method:

```java
static synchronized void increment() {

    count++;
}
```

the lock is effectively associated with:

```java
Counter.class
```

Conceptually similar to:

```java
static void increment() {

    synchronized(Counter.class) {

        count++;
    }
}
```

This is the important distinction:

```text
synchronized instance method
→ object-level lock

static synchronized method
→ class-level lock
```

---

# 🔹 Instance vs Static Synchronization

| Instance Synchronization | Static Synchronization |
|---|---|
| Locks an object | Locks the class object |
| `synchronized void method()` | `static synchronized void method()` |
| Different instances have different locks | Same class-level lock |
| Object-specific | Class-specific |

Example:

```java
class Example {

    synchronized void instanceMethod() {

        System.out.println("Instance");
    }

    static synchronized void staticMethod() {

        System.out.println("Static");
    }
}
```

Conceptually:

```text
instanceMethod()
       ↓
this object lock


staticMethod()
       ↓
Example.class lock
```

---

# 🔹 Synchronized Method vs Block

## Synchronized Method

```java
synchronized void increment() {

    count++;
}
```

The entire method is synchronized.

---

## Synchronized Block

```java
void increment() {

    synchronized(this) {

        count++;
    }
}
```

Only the block is synchronized.

---

## Main Difference

```text
Method
→ larger synchronization scope

Block
→ smaller synchronization scope
```

A synchronized block also lets you explicitly choose the lock object.

Example:

```java
synchronized(lock) {

    count++;
}
```

---

# 🔹 Custom Lock Object

You do not always have to synchronize on `this`.

Example:

```java
class Counter {

    private int count = 0;

    private final Object lock = new Object();

    void increment() {

        synchronized(lock) {

            count++;
        }
    }
}
```

Here:

```text
lock object
    ↓
protects count
```

This can be useful when you want to control exactly which operations share the same lock.

---

# 🔹 Why Use a Private Lock?

A private lock:

```java
private final Object lock = new Object();
```

cannot normally be accessed directly by outside code.

Therefore, external code cannot intentionally synchronize on that same object.

Example:

```java
class Counter {

    private final Object lock = new Object();

    private int count;

    void increment() {

        synchronized(lock) {

            count++;
        }
    }
}
```

This is often preferable to synchronizing on publicly accessible objects.

---

# 🔹 Reentrant Synchronization

Java's intrinsic locks are **reentrant**.

Reentrant means:

> A thread that already owns a lock can acquire the same lock again.

Example:

```java
class Example {

    synchronized void method1() {

        method2();
    }

    synchronized void method2() {

        System.out.println("Inside method2");
    }
}
```

Suppose a thread enters:

```java
method1()
```

It acquires the object's lock.

Then:

```java
method1()
    ↓
method2()
```

The same thread enters another synchronized method using the same object lock.

This is allowed because the lock is reentrant.

Conceptually:

```text
Thread owns lock
      ↓
Calls another synchronized method
      ↓
Same thread requests same lock
      ↓
Allowed
```

---

# 🔹 Synchronization and sleep()

Important:

> `Thread.sleep()` does not release an intrinsic monitor lock.

Example:

```java
class Example {

    synchronized void work() {

        try {

            Thread.sleep(3000);

        } catch(InterruptedException e) {

            Thread.currentThread().interrupt();
        }
    }
}
```

While the thread sleeps:

```text
Thread owns lock
       ↓
sleep()
       ↓
Thread pauses
       ↓
Lock remains held
```

Another thread cannot acquire that same lock merely because the first thread is sleeping.

---

# 🔹 Synchronization and join()

`join()` makes the current thread wait for another thread.

Example:

```java
class Main {

    public static void main(String[] args)
            throws InterruptedException {

        Thread worker = new Thread(() -> {

            System.out.println("Worker running");
        });

        worker.start();

        worker.join();

        System.out.println("Worker finished");
    }
}
```

`join()` itself is not a synchronization mechanism for protecting shared data.

It is primarily used for thread coordination.

However, joining a thread also establishes an important memory-visibility relationship: actions performed by a thread happen-before another thread successfully returns from `join()` on it.

---

# 🔹 Synchronization and Memory Visibility

Synchronization is not only about preventing two threads from entering a critical section simultaneously.

It also provides **memory visibility guarantees**.

Suppose:

```java
class SharedData {

    int value = 0;

    synchronized void write() {

        value = 100;
    }

    synchronized int read() {

        return value;
    }
}
```

When one thread performs the synchronized write and another later acquires the same monitor and performs the synchronized read, the synchronization establishes a happens-before relationship between those actions.

Conceptually:

```text
Thread 1
   |
   | synchronized write
   ↓
value = 100
   |
   ↓
release lock
   |
   ↓
acquire lock
   |
   ↓
Thread 2
   |
   | synchronized read
   ↓
sees value = 100
```

This is one reason synchronization is important for thread safety.

---

# 🔹 Happens-Before

The Java Memory Model defines ordering relationships called **happens-before**.

For synchronized blocks/methods using the same monitor:

```text
Unlock
  ↓
happens-before
  ↓
subsequent lock of same monitor
```

This means actions before releasing the monitor become visible to a thread that subsequently acquires that same monitor.

This topic becomes especially important when studying the **Java Memory Model (JMM)**.

---

# 🔹 Advantages

### 1. Prevents race conditions

Synchronization can protect critical sections from simultaneous access.

---

### 2. Provides mutual exclusion

Only one thread at a time can hold a particular intrinsic lock.

---

### 3. Provides memory visibility

Synchronization establishes important happens-before relationships.

---

### 4. Simple built-in mechanism

Java provides synchronization directly through:

```java
synchronized
```

No external library is required.

---

### 5. Reentrant

The same thread can acquire the same intrinsic lock multiple times.

---

# 🔹 Disadvantages

### 1. Performance overhead

Lock acquisition and release have overhead.

---

### 2. Reduced concurrency

If too much code is synchronized, threads may spend significant time waiting.

---

### 3. Possible deadlocks

Poorly designed locking can result in deadlocks.

Example concept:

```text
Thread 1
  ↓
Lock A
  ↓
waits for B

Thread 2
  ↓
Lock B
  ↓
waits for A
```

Both can become stuck.

Deadlock is covered in the next related topic.

---

### 4. Lock contention

Many threads competing for the same lock can reduce throughput.

```text
Thread 1 ──┐
Thread 2 ──┤
Thread 3 ──┼──→ Same Lock
Thread 4 ──┤
Thread 5 ──┘
```

Only one can own the lock at a time.

---

### 5. Over-synchronization

Synchronizing code that does not need synchronization can unnecessarily reduce concurrency.

---

# 🔹 Common Mistakes

## ❌ Mistake 1 — Thinking synchronized makes everything thread-safe

Synchronization only protects the code and shared state covered by the synchronization strategy.

Example:

```java
class Counter {

    int count;

    synchronized void increment() {

        count++;
    }

    void reset() {

        count = 0;
    }
}
```

`increment()` is synchronized, but `reset()` is not.

So the overall class may still require careful analysis.

---

# 🔹 Mistake 2 — Using different locks accidentally

Consider:

```java
class Counter {

    private final Object lock1 = new Object();
    private final Object lock2 = new Object();

    void increment() {

        synchronized(lock1) {

            // ...
        }
    }

    void decrement() {

        synchronized(lock2) {

            // ...
        }
    }
}
```

These are different locks.

Therefore, synchronization between them is not automatically established.

---

# 🔹 Mistake 3 — Thinking sleep() releases lock

It does not.

```java
synchronized(lock) {

    Thread.sleep(5000);
}
```

The lock remains held while the thread sleeps.

---

# 🔹 Mistake 4 — Synchronizing on a changing object

Avoid designs where the object used as a lock can be replaced.

For example:

```java
Object lock = new Object();
```

and later:

```java
lock = new Object();
```

Different threads may end up synchronizing on different objects.

A dedicated lock is commonly declared:

```java
private final Object lock = new Object();
```

---

# 🔹 Mistake 5 — Synchronizing on public objects

Example:

```java
synchronized("LOCK") {

    // ...
}
```

Using publicly accessible objects as locks can create unintended lock contention.

Prefer a private lock when appropriate:

```java
private final Object lock = new Object();
```

---

# 🔹 Interview Traps

### Q1. What is synchronization?

It is a mechanism for controlling concurrent access to shared resources and maintaining thread safety.

---

### Q2. What does synchronized do?

It provides mutual exclusion around the synchronized region and establishes relevant memory-visibility guarantees.

---

### Q3. What lock does a synchronized instance method use?

The intrinsic lock associated with the current object (`this`).

---

### Q4. What lock does a static synchronized method use?

The intrinsic lock associated with the class object.

For example:

```java
Example.class
```

---

### Q5. Can two threads execute a synchronized method simultaneously?

It depends on the lock.

For the same object and the same intrinsic lock:

```text
No
```

For different objects:

```text
Yes, potentially
```

---

### Q6. Does synchronized guarantee ordering?

No.

Synchronization provides mutual exclusion and memory-visibility guarantees, but it does not mean threads execute in a predetermined order.

---

### Q7. Does sleep() release a synchronized lock?

No.

---

### Q8. Are synchronized locks reentrant?

Yes.

Java's intrinsic monitors are reentrant.

---

### Q9. Can synchronized be used with a block?

Yes.

```java
synchronized(lock) {

    // critical section
}
```

---

### Q10. Can static methods be synchronized?

Yes.

```java
static synchronized void method() {

}
```

---

# 🔹 DSA / Problem-Solving Relevance

Synchronization is not a DSA pattern itself, but it becomes important when DSA structures are shared between threads.

Examples:

- Shared counters
- Concurrent queues
- Producer-consumer problems
- Thread-safe collections
- Parallel processing
- Shared caches
- Concurrent graph processing

Example conceptual problem:

```text
Multiple threads
       ↓
Shared Queue
       ↓
Thread-safe access
       ↓
Synchronization
```

The key question in a concurrent problem is:

> **Which data is shared, and which operations must be atomic with respect to other threads?**

---

# 🔹 Problem-Solving Mindset

When analyzing a multithreaded problem, ask:

```text
1. What data is shared?
        ↓
2. Is it mutable?
        ↓
3. Can multiple threads access it?
        ↓
4. Is an operation made of multiple steps?
        ↓
5. Can operations interleave?
        ↓
6. What needs protection?
        ↓
7. What lock should protect it?
```

This thought process is more important than simply adding `synchronized` everywhere.

---

# 🔹 30-Second Interview Answer

> Synchronization in Java is a mechanism used to safely coordinate access to shared mutable resources between multiple threads. The `synchronized` keyword provides mutual exclusion for a particular intrinsic lock and also establishes important memory-visibility guarantees. It can be applied to instance methods, static methods, or blocks. Instance synchronized methods use the object's monitor, while static synchronized methods use the class object's monitor.

---

# 🔹 Cheat Sheet

## Synchronized Instance Method

```java
synchronized void increment() {

    count++;
}
```

Lock:

```text
this
```

---

## Synchronized Block

```java
synchronized(lock) {

    count++;
}
```

Lock:

```text
lock object
```

---

## Static Synchronized Method

```java
static synchronized void increment() {

    count++;
}
```

Lock:

```text
Class object
```

Example:

```java
Counter.class
```

---

## Static Synchronized Block

```java
static void increment() {

    synchronized(Counter.class) {

        count++;
    }
}
```

---

# 🔹 Quick Comparison

| Type | Lock |
|---|---|
| `synchronized` instance method | `this` |
| `synchronized(this)` | Current object |
| `synchronized(lock)` | `lock` object |
| `static synchronized` method | Class object |
| `synchronized(ClassName.class)` | Class object |

---

# 🔹 Synchronization Flow

```text
Thread
   ↓
Requests Lock
   ↓
Is Lock Available?
   |
   +---- YES ----→ Acquire Lock
   |                  ↓
   |             Critical Section
   |                  ↓
   |              Release Lock
   |
   +---- NO ----→ Wait
                      ↓
                 Lock Available
                      ↓
                 Acquire Lock
```

---

# 🧠 Memory Tricks

### `synchronized`

> **One lock → controlled access**

### Instance method

> **Object lock**

```text
synchronized method
        ↓
      this
```

### Static method

> **Class lock**

```text
static synchronized
        ↓
    ClassName.class
```

### Block

> **Choose the lock**

```java
synchronized(lock) {

}
```

### sleep()

> **Pause but keep lock**

```text
sleep()
   ↓
Thread pauses
   ↓
Lock remains held
```

---

# 🔥 Top 10 Interview Questions

## 1. What is synchronization?

Synchronization controls concurrent access to shared resources and helps maintain thread safety.

---

## 2. Why is synchronization needed?

Because multiple threads accessing shared mutable data can cause race conditions and inconsistent results.

---

## 3. What is a race condition?

A race condition occurs when the result depends on the timing or ordering of concurrent operations on shared data.

---

## 4. What is a critical section?

A critical section is code that accesses shared mutable state and needs controlled concurrent access.

---

## 5. What lock does a synchronized instance method use?

The intrinsic lock associated with the current object:

```java
this
```

---

## 6. What lock does a static synchronized method use?

The intrinsic lock associated with the class object:

```java
ClassName.class
```

---

## 7. What is the difference between synchronized method and block?

A synchronized method protects the entire method.

A synchronized block protects only a selected section and allows you to specify the lock object.

---

## 8. Does sleep() release a lock?

No.

The thread continues to own the intrinsic lock while sleeping.

---

## 9. Is synchronized reentrant?

Yes.

A thread that already owns an intrinsic lock can acquire the same lock again.

---

## 10. Does synchronized guarantee execution order?

No.

It controls access to the protected region but does not guarantee which thread gets the lock first or a fixed execution order.

---

# 🎯 Final Summary

```text
                 SYNCHRONIZATION
                       |
                       ↓
              Shared Mutable Data
                       |
                       ↓
                Multiple Threads
                       |
                       ↓
               synchronized
                       |
             +---------+---------+
             |                   |
             ↓                   ↓
       Mutual Exclusion    Memory Visibility
             |
             ↓
       Critical Section
```

### ⭐ Most Important Concepts

```text
Race Condition
      ↓
Problem caused by concurrent access

Critical Section
      ↓
Code accessing shared mutable state

synchronized
      ↓
Controls access to critical section

Instance synchronized
      ↓
Object-level lock

Static synchronized
      ↓
Class-level lock

Synchronized block
      ↓
Explicitly choose lock

Intrinsic Lock / Monitor
      ↓
Lock associated with an object

Reentrant
      ↓
Same thread can acquire same lock again
```

### ⭐ Most Important Code

```java
class Counter {

    private int count = 0;

    synchronized void increment() {

        count++;
    }

    int getCount() {

        return count;
    }
}
```

### ⭐ One-Line Interview Memory

> **Synchronization protects shared mutable data by controlling which thread can enter a critical section at a time, while also providing important memory-visibility guarantees.**