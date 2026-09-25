# 07 — Synchronization

> **Synchronization is a mechanism used to control access to shared resources when multiple threads execute concurrently.**

---

## 📌 Table of Contents

1. [Introduction](#-introduction)
2. [Why Synchronization is Needed](#-why-synchronization-is-needed)
3. [Shared Resource](#-shared-resource)
4. [Race Condition](#-race-condition)
5. [synchronized Keyword](#-synchronized-keyword)
6. [Synchronized Method](#-synchronized-method)
7. [Synchronized Block](#-synchronized-block)
8. [Object Lock](#-object-lock)
9. [Intrinsic Lock / Monitor](#-intrinsic-lock--monitor)
10. [How synchronized Works](#-how-synchronized-works)
11. [Example Without Synchronization](#-example-without-synchronization)
12. [Example With Synchronization](#-example-with-synchronization)
13. [Synchronized Instance Method](#-synchronized-instance-method)
14. [Synchronized Static Method](#-synchronized-static-method)
15. [Instance Lock vs Class Lock](#-instance-lock-vs-class-lock)
16. [Synchronized Block](#-synchronized-block-1)
17. [this as Lock](#-this-as-lock)
18. [Custom Lock Object](#-custom-lock-object)
19. [Synchronized Method vs Block](#-synchronized-method-vs-block)
20. [Reentrant Synchronization](#-reentrant-synchronization)
21. [What Happens When a Thread Cannot Get the Lock](#-what-happens-when-a-thread-cannot-get-the-lock)
22. [sleep() and synchronized](#-sleep-and-synchronized)
23. [Synchronization and Atomicity](#-synchronization-and-atomicity)
24. [Advantages](#-advantages)
25. [Disadvantages](#-disadvantages)
26. [Common Mistakes](#-common-mistakes)
27. [Interview Traps](#-interview-traps)
28. [DSA / Problem-Solving Relevance](#-dsa--problem-solving-relevance)
29. [30-Second Interview Answer](#-30-second-interview-answer)
30. [Cheat Sheet](#-cheat-sheet)
31. [Top 10 Interview Questions](#-top-10-interview-questions)

---

# 🔹 Introduction

When multiple threads access the same resource at the same time, unexpected results can occur.

Example:

    Thread 1 → balance = 1000
    Thread 2 → balance = 1000

Both threads try to withdraw money simultaneously.

Without proper synchronization, both threads may read the same old value before either thread updates it.

This can cause incorrect results.

Synchronization helps control this access.

---

# 🔹 Why Synchronization is Needed

Consider:

    int count = 0;

Two threads execute:

    count++;

At first glance:

    count++

looks like one operation.

Internally, it involves multiple steps:

    READ count
        ↓
    ADD 1
        ↓
    WRITE count

Suppose:

    count = 0

Two threads can interleave their operations:

    Thread 1 → READ 0
    Thread 2 → READ 0
    Thread 1 → WRITE 1
    Thread 2 → WRITE 1

Expected:

    2

Actual:

    1

This is a classic race condition.

---

# 🔹 Shared Resource

A shared resource is data or an object that can be accessed by multiple threads.

Examples:

- Shared counter
- Bank account
- Shared collection
- File
- Database connection
- Common object
- Shared variable

Example:

    class Counter {

        int count = 0;

        void increment() {
            count++;
        }
    }

If multiple threads use the same `Counter` object, `count` becomes shared state.

---

# 🔹 Race Condition

A race condition occurs when the result depends on the timing or ordering of concurrent operations.

Example:

    class Counter {

        int count = 0;

        void increment() {
            count++;
        }
    }

Multiple threads:

    Thread 1 → increment()
    Thread 2 → increment()
    Thread 3 → increment()

The final result may be less than expected.

Synchronization can prevent multiple threads from simultaneously executing a critical section protected by the same lock.

---

# 🔹 synchronized Keyword

Java provides the:

    synchronized

keyword for synchronization.

It can be used with:

- Instance methods
- Static methods
- Blocks

Its main purpose is to provide mutual exclusion around synchronized code using an intrinsic monitor.

---

# 🔹 Synchronized Method

Syntax:

    synchronized void method() {
        // critical section
    }

Example:

    class Counter {

        private int count = 0;

        synchronized void increment() {
            count++;
        }

        int getCount() {
            return count;
        }
    }

If multiple threads call `increment()` on the same `Counter` object, only one thread at a time can enter that synchronized instance method for that object.

---

# 🔹 Synchronized Block

Instead of synchronizing the entire method, we can synchronize only a specific section.

Syntax:

    synchronized(lockObject) {
        // critical section
    }

Example:

    class Counter {

        private int count = 0;

        void increment() {

            synchronized(this) {
                count++;
            }
        }
    }

Only the critical section is protected.

---

# 🔹 Object Lock

Every Java object has an associated intrinsic monitor.

A synchronized instance method uses the monitor associated with the object.

Example:

    Counter counter = new Counter();

    synchronized(counter) {
        // protected code
    }

Here:

    counter

is the object whose monitor is used.

A thread must acquire that monitor before entering the synchronized block.

---

# 🔹 Intrinsic Lock / Monitor

The terms:

    intrinsic lock

and:

    monitor

are closely related to Java's built-in synchronization mechanism.

Conceptually, an object has a monitor that can be acquired by one thread at a time.

Example:

    synchronized(obj) {
        // critical section
    }

Flow:

    Thread
       ↓
    requests obj monitor
       ↓
    monitor available?
       ↓
      YES
       ↓
    enters synchronized block
       ↓
    executes code
       ↓
    exits block
       ↓
    monitor released

If another thread tries to acquire the same monitor while it is held, that thread cannot enter the protected region until the monitor becomes available.

---

# 🔹 How synchronized Works

Consider:

    synchronized(lock) {
        // critical section
    }

Conceptually:

    Thread 1
       ↓
    acquire lock
       ↓
    execute critical section
       ↓
    release lock

At the same time:

    Thread 2
       ↓
    tries to acquire same lock
       ↓
    cannot enter yet
       ↓
    waits
       ↓
    lock released
       ↓
    acquires lock
       ↓
    executes

This provides mutual exclusion for code protected by that same monitor.

---

# 🔹 Example Without Synchronization

    class Counter {

        int count = 0;

        void increment() {
            count++;
        }
    }

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

            System.out.println(counter.count);
        }
    }

Expected:

    2000

But without synchronization, the result may be less than:

    2000

because `count++` is not an atomic operation.

---

# 🔹 Example With Synchronization

    class Counter {

        int count = 0;

        synchronized void increment() {
            count++;
        }
    }

Now:

    Thread 1
        ↓
    acquire Counter lock
        ↓
    increment
        ↓
    release lock

    Thread 2
        ↓
    acquire Counter lock
        ↓
    increment
        ↓
    release lock

After both threads finish, the expected result is:

    2000

---

# 🔹 Synchronized Instance Method

Example:

    class Counter {

        private int count;

        synchronized void increment() {
            count++;
        }
    }

This is effectively associated with:

    synchronized(this) {
        count++;
    }

for an instance method.

The lock is associated with the current object.

---

# 🔹 Important: Same Object vs Different Objects

Consider:

    Counter c1 = new Counter();
    Counter c2 = new Counter();

Thread 1:

    c1.increment();

Thread 2:

    c2.increment();

Even if `increment()` is synchronized, the two calls use different object monitors:

    c1 → Lock A

    c2 → Lock B

Therefore, they do not block each other merely because the method is synchronized.

This is a very important interview concept.

---

# 🔹 Synchronized Static Method

A static synchronized method uses the monitor associated with the class object.

Example:

    class Counter {

        static int count = 0;

        static synchronized void increment() {
            count++;
        }
    }

Conceptually, it is associated with:

    synchronized(Counter.class) {
        count++;
    }

The lock is on the class object, not on an individual instance.

---

# 🔹 Instance Lock vs Class Lock

### Instance synchronized method

    synchronized void increment() {
        // ...
    }

Uses:

    this

object's monitor.

---

### Static synchronized method

    static synchronized void increment() {
        // ...
    }

Uses:

    ClassName.class

monitor.

Example:

    synchronized(Counter.class) {
        // ...
    }

Memory trick:

    synchronized instance method
        ↓
    object lock

    static synchronized method
        ↓
    class lock

---

# 🔹 Synchronized Block

A synchronized block gives more control.

Example:

    class Counter {

        private int count = 0;

        void increment() {

            synchronized(this) {
                count++;
            }
        }
    }

Only the code inside the block requires the lock.

This can be preferable when the rest of the method does not need synchronization.

---

# 🔹 Why Synchronized Block Can Be Better

Consider:

    void process() {

        performLongCalculation();

        synchronized(this) {
            updateSharedData();
        }

        performAnotherOperation();
    }

Only:

    updateSharedData();

needs protection.

The lock is held for a smaller portion of the method.

This can reduce unnecessary contention.

However, whether it improves performance depends on the actual workload.

---

# 🔹 `this` as Lock

Inside an instance method:

    synchronized(this) {
        // protected section
    }

uses the current object as the monitor.

Example:

    class BankAccount {

        private int balance = 1000;

        void withdraw(int amount) {

            synchronized(this) {
                if(balance >= amount) {
                    balance -= amount;
                }
            }
        }
    }

Here:

    this

represents the current `BankAccount` object.

---

# 🔹 Custom Lock Object

It is often better to use a private lock object when you do not want external code to synchronize on your object's public identity.

Example:

    class Counter {

        private final Object lock = new Object();

        private int count = 0;

        void increment() {

            synchronized(lock) {
                count++;
            }
        }
    }

Here:

    lock

is the monitor used for synchronization.

Because it is private, outside code cannot normally acquire the same lock through that reference.

---

# 🔹 Synchronized Method vs Block

| Feature | Synchronized Method | Synchronized Block |
|---|---|---|
| Scope | Entire method | Selected section |
| Lock control | Less control | More control |
| Syntax | Simple | More explicit |
| Useful when | Whole method needs protection | Only part needs protection |
| Lock object | Implicit | Explicit |

Example method:

    synchronized void update() {
        // entire method
    }

Example block:

    void update() {

        // non-critical work

        synchronized(this) {
            // critical work
        }
    }

---

# 🔹 Reentrant Synchronization

Java's intrinsic locks are reentrant.

This means:

> A thread that already owns a monitor can acquire the same monitor again.

Example:

    class Demo {

        synchronized void method1() {
            method2();
        }

        synchronized void method2() {
            System.out.println("Inside method2");
        }
    }

If the same thread calls:

    method1()

then it already owns the object's monitor.

When it calls:

    method2()

it can acquire the same monitor again.

The JVM tracks the reentrant ownership.

Conceptually:

    Thread
       ↓
    acquire lock
       ↓
    method1()
       ↓
    method2()
       ↓
    same thread acquires same lock again
       ↓
    allowed

---

# 🔹 What Happens When a Thread Cannot Get the Lock

Suppose:

    synchronized(lock) {
        // critical section
    }

Thread 1 acquires the lock.

Thread 2 tries to acquire the same lock.

Thread 2 cannot enter the synchronized block while Thread 1 owns the monitor.

Conceptually:

    Thread 1
       ↓
    LOCK
       ↓
    CRITICAL SECTION

    Thread 2
       ↓
    REQUEST SAME LOCK
       ↓
    WAIT/BLOCK

When Thread 1 exits the synchronized region:

    Thread 1
       ↓
    RELEASE LOCK

Thread 2 can then compete to acquire the monitor.

---

# 🔹 sleep() and synchronized

Consider:

    synchronized(lock) {

        Thread.sleep(5000);

    }

The thread sleeps while still holding the intrinsic monitor associated with `lock`.

Important:

    sleep()
        ↓
    does NOT release monitor

Therefore, another thread trying to enter:

    synchronized(lock)

cannot acquire that same monitor while it is held.

---

# 🔹 Synchronization and Atomicity

Synchronization can make a compound operation effectively atomic with respect to other threads using the same synchronization protocol.

Example:

    synchronized void increment() {
        count++;
    }

Without synchronization:

    READ
      ↓
    MODIFY
      ↓
    WRITE

can interleave between threads.

With synchronization:

    acquire lock
       ↓
    READ
       ↓
    MODIFY
       ↓
    WRITE
       ↓
    release lock

Other synchronized threads using the same monitor cannot enter the protected section simultaneously.

---

# 🔹 Synchronization and Visibility

Synchronization also provides memory-visibility guarantees between threads that synchronize using the same monitor.

When a thread exits a synchronized block, its actions before the unlock become visible to a thread that subsequently acquires the same monitor.

Therefore synchronization helps with both:

    Mutual Exclusion
          +
    Visibility

---

# 🔹 Advantages

### 1. Prevents race conditions

Proper synchronization can prevent conflicting concurrent updates.

### 2. Provides mutual exclusion

Only one thread can hold a particular intrinsic monitor at a time.

### 3. Provides visibility guarantees

Synchronization establishes important happens-before relationships.

### 4. Built into Java

No external library is required for intrinsic synchronization.

### 5. Simple for basic cases

The `synchronized` keyword is straightforward for many synchronization requirements.

---

# 🔹 Disadvantages

### 1. Thread contention

Multiple threads may compete for the same lock.

### 2. Reduced concurrency

Only one thread can execute a particular synchronized critical section protected by the same monitor at a time.

### 3. Possible deadlock

Poor lock design can result in deadlocks.

### 4. Performance overhead

Lock acquisition and contention can introduce overhead.

### 5. Large critical sections

Holding a lock for too long can unnecessarily block other threads.

---

# 🔹 Common Mistakes

## ❌ Mistake 1 — Synchronizing different objects

Example:

    synchronized(new Object()) {
        count++;
    }

This creates a new object each time.

Different threads may therefore synchronize on different monitors.

Synchronization is effective only when threads use the same lock for the shared resource.

---

## ❌ Mistake 2 — Synchronizing only one side

Suppose:

    synchronized(lock) {
        count++;
    }

but another thread directly accesses:

    count++;

The second operation does not automatically become synchronized.

All accesses that need coordination must follow a consistent synchronization strategy.

---

## ❌ Mistake 3 — Assuming synchronized makes everything thread-safe

Synchronization protects the code associated with a particular lock.

It does not automatically make every field, method, or object in a class thread-safe.

---

## ❌ Mistake 4 — Using a huge synchronized block

Example:

    synchronized(this) {

        doVeryLongOperation();

        doAnotherLongOperation();

        updateSharedData();
    }

If only:

    updateSharedData();

requires synchronization, the lock is being held unnecessarily long.

---

## ❌ Mistake 5 — Confusing synchronization with parallel execution

Synchronization does not make threads execute in parallel.

It controls access to protected critical sections.

---

# 🔹 Interview Traps

### Trap 1: Can two threads execute the same synchronized instance method simultaneously?

Not on the **same object monitor**.

If both calls target the same object, only one can hold that object's monitor at a time.

---

### Trap 2: Can two synchronized methods execute simultaneously?

It depends on their locks.

For the same object:

    synchronized method A
    synchronized method B

both use the same instance monitor, so they cannot execute concurrently on that object.

---

### Trap 3: Can synchronized methods on different objects execute simultaneously?

Yes.

Different objects have different intrinsic monitors.

---

### Trap 4: Does synchronized guarantee ordering?

No.

It provides mutual exclusion and memory-visibility guarantees, but does not guarantee a particular scheduling order among waiting threads.

---

### Trap 5: Does sleep() release a synchronized lock?

No.

---

### Trap 6: Is synchronized reentrant?

Yes.

The thread that owns a monitor can acquire it again.

---

### Trap 7: What lock does a static synchronized method use?

The monitor associated with the class object.

Example:

    Counter.class

---

### Trap 8: What lock does an instance synchronized method use?

The monitor associated with the current object.

Conceptually:

    this

---

# 🔹 DSA / Problem-Solving Relevance

Synchronization becomes important when DSA operations are performed concurrently.

Examples:

- Shared counters
- Concurrent queues
- Producer-consumer problems
- Shared caches
- Concurrent data structures
- Parallel algorithms
- Thread-safe collections

Example concept:

    Thread 1 ──┐
               ├──> Shared Counter
    Thread 2 ──┘

Without synchronization:

    Race Condition

With proper synchronization:

    Thread 1 → Lock → Update → Unlock
    Thread 2 → Lock → Update → Unlock

---

# 🔹 30-Second Interview Answer

> Synchronization in Java is a mechanism for controlling concurrent access to shared resources. The `synchronized` keyword provides mutual exclusion using an object's intrinsic monitor and also provides memory-visibility guarantees. A synchronized instance method locks the current object, while a static synchronized method locks the class object. Synchronization helps prevent race conditions, but excessive synchronization can cause contention and reduce concurrency.

---

# 🔹 Cheat Sheet

## `synchronized` method

    synchronized void increment() {
        count++;
    }

Means:

    lock current object
        ↓
    execute method
        ↓
    release lock

---

## `synchronized` block

    synchronized(lock) {
        count++;
    }

Means:

    acquire lock
        ↓
    execute block
        ↓
    release lock

---

## Instance synchronized method

    synchronized void method()

Lock:

    this

---

## Static synchronized method

    static synchronized void method()

Lock:

    ClassName.class

---

## Key Properties

    synchronized
        ↓
    Mutual Exclusion
        +
    Visibility
        +
    Reentrant

---

# 🧠 Memory Tricks

### Synchronization

> **One lock → One thread at a time**

### Instance method

> **Object lock**

### Static method

> **Class lock**

### sleep()

> **Does NOT release monitor**

### Reentrant

> **Same thread can acquire the same lock again**

---

# 🔥 Top 10 Interview Questions

## 1. What is synchronization?

Synchronization is a mechanism for controlling concurrent access to shared resources.

---

## 2. Why is synchronization required?

To prevent race conditions and provide appropriate memory-visibility guarantees when threads access shared mutable state.

---

## 3. What does `synchronized` do?

It uses an intrinsic monitor to provide mutual exclusion around synchronized code and establishes memory-visibility guarantees.

---

## 4. What lock does a synchronized instance method use?

The monitor associated with the current object.

Conceptually:

    this

---

## 5. What lock does a static synchronized method use?

The monitor associated with the class object.

Example:

    Counter.class

---

## 6. What is a synchronized block?

A block of code protected by a specified monitor.

Example:

    synchronized(lock) {
        // critical section
    }

---

## 7. Can two threads execute synchronized methods simultaneously?

On the same object and same intrinsic monitor, they cannot simultaneously execute synchronized instance methods.

Different objects can have different monitors.

---

## 8. Does sleep() release a lock?

No.

A thread sleeping while holding an intrinsic monitor continues to hold that monitor.

---

## 9. Is synchronized reentrant?

Yes.

The same thread can acquire the same intrinsic monitor multiple times.

---

## 10. What are the disadvantages of synchronization?

Potential disadvantages include:

- Lock contention
- Reduced concurrency
- Performance overhead
- Deadlock risk with poor locking design
- Longer waiting times

---

# 🎯 Final Summary

The core idea of synchronization is:

    Shared Resource
          ↓
    Multiple Threads
          ↓
    Concurrent Access
          ↓
    Race Condition
          ↓
    synchronized
          ↓
    One thread at a time
          ↓
    Safer shared-state access

Remember:

    synchronized method
        ↓
    locks object/class

    synchronized block
        ↓
    locks specified object

    sleep()
        ↓
    does NOT release monitor

    synchronized
        ↓
    mutual exclusion + visibility

    synchronized locks
        ↓
    are reentrant