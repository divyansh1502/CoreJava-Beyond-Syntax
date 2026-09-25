# 08 — Race Condition

> **A race condition occurs when multiple threads access shared mutable data concurrently and the program's result depends on the timing or order of their execution.**

---

## 📌 Table of Contents

1. [Introduction](#-introduction)
2. [What is a Race Condition?](#-what-is-a-race-condition)
3. [Why Race Conditions Occur](#-why-race-conditions-occur)
4. [Shared Mutable Data](#-shared-mutable-data)
5. [Simple Example](#-simple-example)
6. [How `count++` Causes a Race Condition](#-how-count-causes-a-race-condition)
7. [Thread Interleaving](#-thread-interleaving)
8. [Lost Update](#-lost-update)
9. [Demonstrating a Race Condition](#-demonstrating-a-race-condition)
10. [Why the Result Changes](#-why-the-result-changes)
11. [Fixing a Race Condition with synchronized](#-fixing-a-race-condition-with-synchronized)
12. [Synchronized Block](#-synchronized-block)
13. [Atomic Classes](#-atomic-classes)
14. [Race Condition vs Data Race](#-race-condition-vs-data-race)
15. [Race Condition vs Deadlock](#-race-condition-vs-deadlock)
16. [Race Condition and Collections](#-race-condition-and-collections)
17. [Race Condition and Multiple Objects](#-race-condition-and-multiple-objects)
18. [Common Mistakes](#-common-mistakes)
19. [Interview Traps](#-interview-traps)
20. [DSA / Problem-Solving Relevance](#-dsa--problem-solving-relevance)
21. [30-Second Interview Answer](#-30-second-interview-answer)
22. [Cheat Sheet](#-cheat-sheet)
23. [Top 10 Interview Questions](#-top-10-interview-questions)

---

# 🔹 Introduction

In multithreaded programs, multiple threads may execute at the same time.

If they access the same mutable data without proper coordination, their operations can interfere with each other.

This can produce incorrect or unpredictable results.

Example:

```text
Thread 1 ───────┐
                ↓
            Shared Data
                ↑
Thread 2 ───────┘
```

The problem becomes especially important when an operation consists of multiple steps.

For example:

```java
count++;
```

looks like one operation, but conceptually it involves:

```text
Read
 ↓
Modify
 ↓
Write
```

Another thread can interfere between these steps.

---

# 🔹 What is a Race Condition?

A **race condition** occurs when the correctness of a program depends on the relative timing or ordering of concurrent operations.

In simple words:

> **Multiple threads race to access or modify shared data, and the result depends on who gets there first or how their operations interleave.**

Example:

```java
class Counter {

    int count = 0;

    void increment() {

        count++;
    }
}
```

Suppose two threads execute:

```java
counter.increment();
```

at the same time.

The expected result might be:

```text
2
```

but the actual result can be:

```text
1
```

because of an interleaving of operations.

---

# 🔹 Why Race Conditions Occur

A race condition generally requires some combination of:

```text
Multiple threads
      ↓
Shared resource
      ↓
Mutable state
      ↓
Concurrent access
      ↓
Non-atomic operation
      ↓
Race condition
```

The important words are:

- Multiple threads
- Shared data
- Mutable state
- Concurrent access
- Unsafe interleaving

---

# 🔹 Shared Mutable Data

Consider:

```java
class Counter {

    int count = 0;
}
```

The variable:

```java
count
```

is mutable because its value can change.

If multiple threads use the same `Counter` object:

```java
Counter counter = new Counter();

Thread t1 = new Thread(() -> counter.count++);
Thread t2 = new Thread(() -> counter.count++);
```

then both threads access the same variable.

Conceptually:

```text
             Counter Object
                   |
                 count
                /     \
               /       \
          Thread 1   Thread 2
```

This is shared mutable state.

---

# 🔹 Simple Example

Consider:

```java
class Counter {

    int count = 0;

    void increment() {

        count++;
    }
}
```

Now create two threads:

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

        System.out.println(counter.count);
    }
}
```

We might expect:

```text
2000
```

But because `count++` is not an atomic read-modify-write operation, the result can be less than expected.

The exact result is not guaranteed.

---

# 🔹 How `count++` Causes a Race Condition

This:

```java
count++;
```

is conceptually similar to:

```text
1. Read count
2. Add 1
3. Write count
```

Suppose:

```text
count = 0
```

Two threads execute it.

### Step 1

Thread 1 reads:

```text
0
```

Thread 2 also reads:

```text
0
```

### Step 2

Thread 1 calculates:

```text
0 + 1 = 1
```

Thread 2 calculates:

```text
0 + 1 = 1
```

### Step 3

Thread 1 writes:

```text
count = 1
```

Thread 2 writes:

```text
count = 1
```

Final result:

```text
1
```

Expected:

```text
2
```

One update was lost.

---

# 🔹 Thread Interleaving

The key problem is **interleaving**.

Suppose:

```text
T1: Read count
T1: Add 1
T1: Write count

T2: Read count
T2: Add 1
T2: Write count
```

This ordering is safe in this particular example because the operations do not overlap.

But this ordering can cause a problem:

```text
T1: Read count
T2: Read count

T1: Add 1
T2: Add 1

T1: Write count
T2: Write count
```

The second thread overwrites the first thread's update.

---

# 🔹 Lost Update

A **lost update** occurs when one thread's modification is overwritten by another thread.

Example:

```text
Initial count = 0

Thread 1 reads 0
Thread 2 reads 0

Thread 1 calculates 1
Thread 2 calculates 1

Thread 1 writes 1
Thread 2 writes 1
```

Final:

```text
1
```

But two increments happened.

Expected:

```text
2
```

The first increment was effectively lost.

---

# 🔹 Demonstrating a Race Condition

A larger example makes the problem easier to observe.

```java
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

            for(int i = 0; i < 100000; i++) {

                counter.increment();
            }
        });

        Thread t2 = new Thread(() -> {

            for(int i = 0; i < 100000; i++) {

                counter.increment();
            }
        });

        t1.start();
        t2.start();

        t1.join();
        t2.join();

        System.out.println("Expected: 200000");
        System.out.println("Actual: " + counter.count);
    }
}
```

The actual result may be less than:

```text
200000
```

because of race conditions.

However, the exact result is timing-dependent and should not be assumed.

---

# 🔹 Why the Result Changes

Thread scheduling is not deterministic.

The operating system and JVM runtime determine when threads get CPU time.

Therefore, different executions can have different interleavings.

Conceptually:

```text
Run 1
T1 → T1 → T2 → T1 → T2

Run 2
T2 → T1 → T2 → T2 → T1

Run 3
T1 → T2 → T2 → T1 → T1
```

Different interleavings can produce different results when shared state is accessed unsafely.

---

# 🔹 Race Condition with Bank Account

Race conditions are not limited to counters.

Consider a bank account:

```java
class BankAccount {

    private int balance = 1000;

    void withdraw(int amount) {

        if(balance >= amount) {

            balance -= amount;
        }
    }

    int getBalance() {

        return balance;
    }
}
```

Suppose two threads withdraw:

```text
₹700
```

at approximately the same time.

Both threads may observe:

```text
balance = 1000
```

before either performs the update.

Then both may proceed.

This can result in an incorrect balance or violate the intended business rule.

The exact behavior depends on how the operations are implemented and interleaved.

---

# 🔹 Fixing a Race Condition with synchronized

One common solution is synchronization.

```java
class Counter {

    int count = 0;

    synchronized void increment() {

        count++;
    }
}
```

Now only one thread at a time can execute `increment()` on the same `Counter` object.

Conceptually:

```text
Thread 1
   ↓
Acquire lock
   ↓
count++
   ↓
Release lock
   ↓
Thread 2
   ↓
Acquire lock
   ↓
count++
   ↓
Release lock
```

This prevents the two executions from entering the critical section simultaneously.

---

# 🔹 Complete Synchronized Example

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

class Main {

    public static void main(String[] args)
            throws InterruptedException {

        Counter counter = new Counter();

        Thread t1 = new Thread(() -> {

            for(int i = 0; i < 100000; i++) {

                counter.increment();
            }
        });

        Thread t2 = new Thread(() -> {

            for(int i = 0; i < 100000; i++) {

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

Expected final value:

```text
200000
```

because every increment is protected by the same object's intrinsic lock.

---

# 🔹 Synchronized Block

Instead of synchronizing the entire method:

```java
synchronized void increment() {

    count++;
}
```

we can protect only the critical section:

```java
void increment() {

    synchronized(this) {

        count++;
    }
}
```

This is useful when only a small part of a method requires synchronization.

---

# 🔹 Using a Private Lock

We can also use a dedicated lock:

```java
class Counter {

    private int count = 0;

    private final Object lock = new Object();

    void increment() {

        synchronized(lock) {

            count++;
        }
    }

    int getCount() {

        return count;
    }
}
```

Conceptually:

```text
Thread 1 ──┐
           ↓
        lock object
           ↑
Thread 2 ──┘
```

Only one thread can hold that intrinsic lock at a time.

---

# 🔹 Atomic Classes

Another common solution for simple atomic operations is Java's atomic classes.

For example:

```java
import java.util.concurrent.atomic.AtomicInteger;

class Counter {

    private final AtomicInteger count = new AtomicInteger();

    void increment() {

        count.incrementAndGet();
    }

    int getCount() {

        return count.get();
    }
}
```

Here:

```java
count.incrementAndGet();
```

provides an atomic increment operation.

Atomic classes are covered in detail in:

```text
11-Atomic-Classes.md
```

---

# 🔹 synchronized vs AtomicInteger

For a simple counter:

```java
synchronized
```

and:

```java
AtomicInteger
```

can both provide safe increments, but they use different mechanisms.

### synchronized

Uses intrinsic locking:

```text
Lock
 ↓
Critical section
 ↓
Unlock
```

### AtomicInteger

Uses atomic operations designed for lock-free updates in common cases.

Example:

```java
AtomicInteger count = new AtomicInteger();

count.incrementAndGet();
```

The choice depends on the problem.

---

# 🔹 Race Condition vs Data Race

These terms are related but are not always interchangeable.

### Data Race

A data race generally refers to conflicting unsynchronized accesses to the same memory location from multiple threads, where at least one access is a write.

### Race Condition

A race condition is broader.

It means correctness depends on the timing or ordering of concurrent operations.

Therefore:

```text
Data race
→ specific low-level concurrency problem

Race condition
→ broader correctness problem caused by timing/order
```

A program can have a race condition involving operations that are individually synchronized but coordinated incorrectly at a higher level.

---

# 🔹 Race Condition vs Deadlock

These are different concurrency problems.

## Race Condition

The problem is:

```text
Incorrect result
```

because threads interfere with shared state.

Example:

```text
T1 updates
T2 updates
T1/T2 overwrite each other
```

---

## Deadlock

The problem is:

```text
Threads wait forever
```

Example:

```text
Thread 1
   ↓
holds Lock A
   ↓
waits for Lock B

Thread 2
   ↓
holds Lock B
   ↓
waits for Lock A
```

Comparison:

| Race Condition | Deadlock |
|---|---|
| Incorrect/unpredictable result | Threads become stuck |
| Caused by unsafe concurrent access/order | Caused by circular waiting for locks/resources |
| Data consistency problem | Progress problem |
| Can produce different outputs | Can cause indefinite waiting |

---

# 🔹 Race Condition and Collections

Collections can also be involved in race conditions.

Example:

```java
List<Integer> list = new ArrayList<>();

Thread t1 = new Thread(() -> {

    list.add(10);
});

Thread t2 = new Thread(() -> {

    list.add(20);
});
```

Using a non-thread-safe collection concurrently without appropriate coordination can lead to unsafe behavior.

Thread safety depends on:

- The collection implementation
- The operations being performed
- Whether the collection is shared
- Whether external synchronization is used

---

# 🔹 Race Condition and Multiple Objects

Synchronization only coordinates threads that use the same lock.

Example:

```java
Counter c1 = new Counter();
Counter c2 = new Counter();
```

If `increment()` is synchronized:

```java
synchronized void increment() {

    count++;
}
```

then:

```text
c1 → Lock 1
c2 → Lock 2
```

The locks are different.

Therefore, threads using `c1` and `c2` are not mutually excluded by those instance locks.

This is important:

> **Synchronization works through a particular lock.**

---

# 🔹 Common Mistakes

## ❌ Mistake 1 — Thinking `count++` is atomic

This:

```java
count++;
```

is not generally an atomic read-modify-write operation.

Conceptually:

```text
Read
 ↓
Modify
 ↓
Write
```

---

## ❌ Mistake 2 — Assuming volatile fixes every race condition

`volatile` provides visibility and ordering guarantees for a variable, but it does not make compound operations such as:

```java
count++;
```

atomic.

Example:

```java
volatile int count = 0;
```

does not make:

```java
count++;
```

a safe atomic increment.

---

## ❌ Mistake 3 — Adding synchronized randomly

Simply adding:

```java
synchronized
```

without identifying the shared state and correct lock does not automatically solve every concurrency problem.

You must identify:

```text
What is shared?
What must be protected?
Which lock protects it?
```

---

## ❌ Mistake 4 — Synchronizing on different objects

This:

```java
synchronized(lock1) {

}
```

does not coordinate with:

```java
synchronized(lock2) {

}
```

if:

```text
lock1 != lock2
```

---

## ❌ Mistake 5 — Thinking race conditions happen every time

A race condition may exist even if the program appears to work correctly in many runs.

Thread scheduling is timing-dependent.

Therefore:

```text
Works once
≠
Thread-safe
```

---

# 🔹 Interview Traps

### Q1. Is `count++` atomic?

No.

It is a read-modify-write operation.

---

### Q2. Can a single variable cause a race condition?

Yes, if multiple threads access shared mutable state without appropriate synchronization and at least one access modifies it.

---

### Q3. Does `volatile` make `count++` atomic?

No.

`volatile` does not make compound read-modify-write operations atomic.

---

### Q4. Does `synchronized` prevent race conditions?

It can prevent race conditions when the relevant shared state and operations are correctly protected by the same lock.

---

### Q5. Is synchronization only about mutual exclusion?

No.

It also provides important memory-visibility guarantees through happens-before relationships.

---

### Q6. Can race conditions occur with collections?

Yes.

Concurrent access to non-thread-safe mutable collections can cause concurrency problems.

---

### Q7. Does a race condition always produce an incorrect result?

A race condition is a correctness problem caused by timing/order. Its manifestation can vary; it may produce incorrect or inconsistent behavior rather than failing every time.

---

### Q8. Is a race condition the same as deadlock?

No.

A race condition concerns correctness under concurrent execution.

A deadlock concerns threads becoming unable to make progress because they are waiting on each other.

---

### Q9. Can two synchronized methods run simultaneously?

It depends on their lock objects.

Two synchronized instance methods on the same object cannot be entered simultaneously by different threads.

The same methods on different objects can potentially execute concurrently.

---

### Q10. How do you prevent a race condition?

Depending on the problem, possible techniques include:

- `synchronized`
- Atomic classes
- Locks
- Thread-safe collections
- Immutability
- Proper concurrent design

---

# 🔹 DSA / Problem-Solving Relevance

Race conditions become relevant when an algorithm is executed concurrently.

Important scenarios include:

### 1. Shared Counter

```text
Thread 1 → increment
Thread 2 → increment
Thread 3 → increment
```

Need safe updates.

---

### 2. Shared Queue

```text
Producer
    ↓
Shared Queue
    ↑
Consumer
```

Concurrent insertion/removal must be coordinated correctly.

---

### 3. Parallel Search

```text
Array
 ↓
Split into sections
 ↓
Thread 1 → Search section 1
Thread 2 → Search section 2
Thread 3 → Search section 3
```

If threads write to shared result state, synchronization or another safe coordination mechanism may be required.

---

### 4. Shared HashMap

Multiple threads modifying a normal mutable map concurrently can create unsafe behavior.

Thread-safe alternatives or appropriate synchronization may be required.

---

# 🔹 Problem-Solving Mindset

Whenever you see multithreaded code, ask:

```text
1. What data is shared?
        ↓
2. Is that data mutable?
        ↓
3. Can multiple threads access it?
        ↓
4. Can their operations overlap?
        ↓
5. Is the operation atomic?
        ↓
6. What happens if operations interleave?
        ↓
7. What mechanism protects the operation?
```

This is the correct way to reason about race conditions.

---

# 🔹 30-Second Interview Answer

> A race condition occurs when multiple threads concurrently access shared mutable data and the correctness of the program depends on their execution timing or ordering. A common example is `count++`, which is a read-modify-write operation rather than a single atomic operation. Race conditions can be prevented or controlled using mechanisms such as `synchronized`, atomic classes, locks, thread-safe collections, or other concurrency-safe designs.

---

# 🔹 Cheat Sheet

## Race Condition

```text
Multiple Threads
       ↓
Shared Mutable Data
       ↓
Concurrent Access
       ↓
Unsafe Interleaving
       ↓
Race Condition
```

---

## `count++`

```text
Read count
    ↓
Add 1
    ↓
Write count
```

Not generally atomic.

---

## Synchronized Fix

```java
synchronized void increment() {

    count++;
}
```

---

## Synchronized Block

```java
synchronized(lock) {

    count++;
}
```

---

## Atomic Fix

```java
AtomicInteger count = new AtomicInteger();

count.incrementAndGet();
```

---

# 🧠 Memory Tricks

### Race Condition

> **"Who gets there first can change the result."**

---

### Shared State

> **"Same data + multiple threads."**

---

### `count++`

> **"Looks one-step, works multiple-step."**

```text
Read
 ↓
Modify
 ↓
Write
```

---

### synchronized

> **"One lock, one owner at a time."**

---

### volatile

> **"Visibility, not compound-operation atomicity."**

---

# 🔥 Top 10 Interview Questions

## 1. What is a race condition?

A race condition occurs when the correctness of a program depends on the timing or ordering of concurrent operations.

---

## 2. Give an example of a race condition.

A shared counter:

```java
class Counter {

    int count = 0;

    void increment() {

        count++;
    }
}
```

When multiple threads execute `increment()` concurrently, updates can be lost.

---

## 3. Why is `count++` unsafe?

Because conceptually it consists of:

```text
Read → Modify → Write
```

Multiple threads can interleave these operations.

---

## 4. How can you fix a race condition?

Depending on the problem:

```text
synchronized
Atomic classes
Locks
Thread-safe collections
Immutability
Concurrent algorithms
```

---

## 5. Does volatile make `count++` safe?

No.

```java
volatile int count;
```

does not make:

```java
count++;
```

atomic.

---

## 6. What is a lost update?

A lost update occurs when one thread's update is overwritten by another thread's update.

---

## 7. What is a critical section?

A critical section is a section of code that accesses shared mutable state and requires coordinated access.

---

## 8. Is race condition the same as deadlock?

No.

```text
Race Condition
→ correctness problem

Deadlock
→ progress problem
```

---

## 9. Can synchronization prevent race conditions?

Yes, when the correct shared state and operations are protected using the same appropriate lock.

---

## 10. What is the key question when debugging a race condition?

Ask:

> **Which shared mutable data is being accessed concurrently, and can the operations on it interleave unsafely?**

---

# 🎯 Final Summary

```text
                 RACE CONDITION
                       |
                       ↓
              Multiple Threads
                       |
                       ↓
              Shared Mutable Data
                       |
                       ↓
             Concurrent Operations
                       |
                       ↓
              Unsafe Interleaving
                       |
                       ↓
                Wrong / Unsafe
                   Behavior
```

### ⭐ Core Example

```java
class Counter {

    int count = 0;

    void increment() {

        count++;
    }
}
```

Multiple threads:

```java
Counter counter = new Counter();

Thread t1 = new Thread(counter::increment);
Thread t2 = new Thread(counter::increment);

t1.start();
t2.start();
```

Potential problem:

```text
T1 → Read 0
T2 → Read 0
T1 → Write 1
T2 → Write 1

Expected → 2
Actual   → 1
```

### ⭐ Common Solutions

```text
Race Condition
      ↓
+-----------------------+
| synchronized          |
| Atomic Classes        |
| Locks                 |
| Thread-safe Objects   |
| Immutability          |
| Proper Concurrency    |
+-----------------------+
```

### ⭐ One-Line Interview Memory

> **A race condition occurs when multiple threads access shared mutable state and the result depends on how their operations are interleaved.**