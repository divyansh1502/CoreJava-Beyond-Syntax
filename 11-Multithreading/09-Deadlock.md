# 09 — Deadlock

> **A deadlock occurs when two or more threads become permanently blocked because each thread is waiting for a resource held by another thread.**

---

## 📌 Table of Contents

1. [Introduction](#-introduction)
2. [What is Deadlock?](#-what-is-deadlock)
3. [Simple Example](#-simple-example)
4. [How Deadlock Occurs](#-how-deadlock-occurs)
5. [Two-Thread Deadlock](#-two-thread-deadlock)
6. [Multiple Locks](#-multiple-locks)
7. [Four Necessary Conditions](#-four-necessary-conditions)
8. [Mutual Exclusion](#-mutual-exclusion)
9. [Hold and Wait](#-hold-and-wait)
10. [No Preemption](#-no-preemption)
11. [Circular Wait](#-circular-wait)
12. [Deadlock Cycle](#-deadlock-cycle)
13. [Real-World Example](#-real-world-example)
14. [Deadlock with synchronized](#-deadlock-with-synchronized)
15. [Nested synchronized Blocks](#-nested-synchronized-blocks)
16. [Lock Ordering](#-lock-ordering)
17. [Preventing Deadlock](#-preventing-deadlock)
18. [Using tryLock()](#-using-trylock)
19. [Timeout-Based Lock Acquisition](#-timeout-based-lock-acquisition)
20. [Deadlock Detection](#-deadlock-detection)
21. [Deadlock vs Race Condition](#-deadlock-vs-race-condition)
22. [Deadlock vs Starvation](#-deadlock-vs-starvation)
23. [Deadlock vs Livelock](#-deadlock-vs-livelock)
24. [Common Mistakes](#-common-mistakes)
25. [Interview Traps](#-interview-traps)
26. [DSA / Problem-Solving Relevance](#-dsa--problem-solving-relevance)
27. [30-Second Interview Answer](#-30-second-interview-answer)
28. [Cheat Sheet](#-cheat-sheet)
29. [Top 10 Interview Questions](#-top-10-interview-questions)

---

# 🔹 Introduction

In multithreaded programs, threads may need to acquire multiple locks to safely access shared resources.

If locks are acquired in an unsafe order, threads can become permanently blocked.

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

Neither thread can continue.

This situation is called a **deadlock**.

---

# 🔹 What is Deadlock?

A **deadlock** is a situation where two or more threads are blocked forever because each thread is waiting for a resource or lock held by another thread in the same waiting cycle.

Simple representation:

```text
Thread 1 → Lock A → waits for Lock B
                       ↑
                       |
Thread 2 → Lock B → waits for Lock A
```

Therefore:

```text
T1 waits for T2
T2 waits for T1
```

Neither can proceed.

---

# 🔹 Simple Example

Suppose there are two locks:

```java
Object lock1 = new Object();
Object lock2 = new Object();
```

Thread 1:

```text
Acquire lock1
      ↓
Wait for lock2
```

Thread 2:

```text
Acquire lock2
      ↓
Wait for lock1
```

Result:

```text
T1 → holds lock1 → waits for lock2
T2 → holds lock2 → waits for lock1
```

This is a deadlock.

---

# 🔹 How Deadlock Occurs

A typical deadlock looks like:

```text
Thread 1
   |
   | acquires
   ↓
 Lock A
   |
   | waits for
   ↓
 Lock B
   ↑
   | held by
   |
Thread 2
   |
   | holds
   ↓
 Lock B
   |
   | waits for
   ↓
 Lock A
```

The threads are permanently waiting for each other.

---

# 🔹 Two-Thread Deadlock

Here is a classic example:

```java
public class Main {

    private static final Object lock1 = new Object();
    private static final Object lock2 = new Object();

    public static void main(String[] args) {

        Thread t1 = new Thread(() -> {

            synchronized(lock1) {

                System.out.println("Thread 1 acquired lock1");

                synchronized(lock2) {

                    System.out.println("Thread 1 acquired lock2");
                }
            }
        });

        Thread t2 = new Thread(() -> {

            synchronized(lock2) {

                System.out.println("Thread 2 acquired lock2");

                synchronized(lock1) {

                    System.out.println("Thread 2 acquired lock1");
                }
            }
        });

        t1.start();
        t2.start();
    }
}
```

Possible execution:

```text
Thread 1:
acquires lock1

Thread 2:
acquires lock2

Thread 1:
waits for lock2

Thread 2:
waits for lock1
```

Now:

```text
T1 → Lock 1 → waits for Lock 2
                     ↑
                     |
T2 → Lock 2 → waits for Lock 1
```

Both threads can remain blocked indefinitely.

---

# 🔹 Multiple Locks

Deadlocks commonly appear when code needs more than one lock.

Example:

```java
synchronized(lockA) {

    synchronized(lockB) {

        // critical section
    }
}
```

This means the thread must acquire:

```text
Lock A
   ↓
Lock B
```

If another thread acquires them in reverse order:

```text
Lock B
   ↓
Lock A
```

a deadlock can occur.

---

# 🔹 Four Necessary Conditions

The classic **Coffman conditions** identify four conditions that must all be present for a deadlock to occur:

1. Mutual Exclusion
2. Hold and Wait
3. No Preemption
4. Circular Wait

Memory trick:

```text
M H N C
```

or:

> **Mutual → Hold → No Preemption → Circular**

If at least one of these conditions is prevented, deadlock can be prevented.

---

# 🔹 Mutual Exclusion

A resource can be held by only one thread at a time.

Example:

```text
Lock A
  ↓
Thread 1 owns it

Thread 2
  ↓
must wait
```

This is mutual exclusion.

For intrinsic locks:

```java
synchronized(lock) {

    // protected code
}
```

only one thread can own that monitor at a time.

---

# 🔹 Hold and Wait

A thread holds one resource while waiting for another.

Example:

```text
Thread 1

holds Lock A
     ↓
waits for Lock B
```

At the same time:

```text
Thread 2

holds Lock B
     ↓
waits for Lock A
```

Each thread is holding something while waiting for something else.

---

# 🔹 No Preemption

A resource cannot simply be forcibly taken away from a thread holding it.

For example:

```text
Thread 1 owns Lock A
```

Thread 2 cannot simply say:

```text
"Give me Lock A."
```

and forcibly take it.

The lock must normally be released according to the synchronization/locking mechanism.

---

# 🔹 Circular Wait

This is the most recognizable condition.

Example:

```text
Thread 1
   ↓
holds Lock A
   ↓
waits for Lock B
   ↓
held by Thread 2
   ↓
waits for Lock A
```

This creates a cycle:

```text
T1 → T2 → T1
```

For more threads:

```text
T1 → T2 → T3 → T4 → T1
```

The waiting dependency forms a circle.

---

# 🔹 Deadlock Cycle

Consider:

```text
T1 holds A
T1 waits for B

T2 holds B
T2 waits for C

T3 holds C
T3 waits for A
```

Graphically:

```text
T1
 ↓
T2
 ↓
T3
 ↓
T1
```

This is a circular wait.

---

# 🔹 Real-World Example

Imagine:

```text
Person A has Pen
Person B has Paper
```

Person A:

```text
"I need Paper."
```

Person B:

```text
"I need Pen."
```

Neither gives up what they already have.

```text
A → holds Pen → waits for Paper
B → holds Paper → waits for Pen
```

This is analogous to a deadlock between threads and locks.

---

# 🔹 Deadlock with synchronized

The `synchronized` keyword can be involved in deadlocks if multiple locks are acquired in inconsistent orders.

Example:

```java
class Example {

    private final Object lockA = new Object();
    private final Object lockB = new Object();

    void method1() {

        synchronized(lockA) {

            synchronized(lockB) {

                System.out.println("Method 1");
            }
        }
    }

    void method2() {

        synchronized(lockB) {

            synchronized(lockA) {

                System.out.println("Method 2");
            }
        }
    }
}
```

The order is:

```text
method1:
A → B

method2:
B → A
```

This creates a potential deadlock.

---

# 🔹 Nested synchronized Blocks

Nested synchronization means acquiring another lock while already holding one.

Example:

```java
synchronized(lockA) {

    synchronized(lockB) {

        // code
    }
}
```

The thread owns:

```text
Lock A
```

before requesting:

```text
Lock B
```

This is not automatically a deadlock.

The danger appears when another thread acquires the same locks in the opposite order.

---

# 🔹 Lock Ordering

One of the most common ways to prevent deadlock is to establish a consistent lock order.

Suppose we always acquire:

```text
Lock A
   ↓
Lock B
```

Then every thread must follow:

```text
A → B
```

Never:

```text
B → A
```

Example:

```java
synchronized(lockA) {

    synchronized(lockB) {

        // critical section
    }
}
```

Another method should also use:

```java
synchronized(lockA) {

    synchronized(lockB) {

        // critical section
    }
}
```

This prevents the circular dependency caused by opposite lock ordering.

---

# 🔹 Example of Safe Lock Ordering

```java
class Example {

    private final Object lockA = new Object();
    private final Object lockB = new Object();

    void method1() {

        synchronized(lockA) {

            synchronized(lockB) {

                System.out.println("Method 1");
            }
        }
    }

    void method2() {

        synchronized(lockA) {

            synchronized(lockB) {

                System.out.println("Method 2");
            }
        }
    }
}
```

Both methods follow:

```text
A → B
```

Therefore, they do not create the same circular-wait pattern caused by:

```text
A → B
B → A
```

---

# 🔹 Preventing Deadlock

Deadlock can be prevented by breaking one or more of the necessary conditions.

Common techniques include:

### 1. Consistent lock ordering

Always acquire locks in the same order.

```text
A → B
A → B
A → B
```

---

### 2. Avoid unnecessary nested locks

Instead of:

```java
synchronized(lockA) {

    synchronized(lockB) {

        // large amount of code
    }
}
```

keep the locking scope as small as practical.

---

### 3. Use timeout-based locking

With explicit locks, a thread can attempt to acquire a lock for a limited amount of time.

---

### 4. Use `tryLock()`

`ReentrantLock` provides:

```java
tryLock()
```

which can attempt to acquire a lock without waiting indefinitely.

---

### 5. Reduce shared mutable state

The less shared mutable state you have, the fewer opportunities exist for locking problems.

---

### 6. Use higher-level concurrency utilities

Java provides utilities such as:

```text
ExecutorService
ConcurrentHashMap
BlockingQueue
CountDownLatch
Semaphore
CompletableFuture
```

Depending on the problem, these can reduce the need for manually managing multiple locks.

---

# 🔹 Using tryLock()

The `Lock` API provides more flexible locking than intrinsic `synchronized` blocks.

Example:

```java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

class Example {

    private final Lock lock = new ReentrantLock();

    void process() {

        if(lock.tryLock()) {

            try {

                System.out.println("Lock acquired");

            } finally {

                lock.unlock();
            }

        } else {

            System.out.println("Could not acquire lock");
        }
    }
}
```

The important idea is:

```text
tryLock()
    ↓
attempt to acquire
    ↓
success → enter
failure → continue without waiting indefinitely
```

---

# 🔹 Timeout-Based Lock Acquisition

`tryLock()` can also be used with a timeout.

Example:

```java
import java.util.concurrent.TimeUnit;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

class Example {

    private final Lock lock = new ReentrantLock();

    void process() throws InterruptedException {

        if(lock.tryLock(1, TimeUnit.SECONDS)) {

            try {

                System.out.println("Lock acquired");

            } finally {

                lock.unlock();
            }

        } else {

            System.out.println("Could not acquire lock within timeout");
        }
    }
}
```

Instead of waiting forever:

```text
wait forever
```

the thread waits only for the specified period.

---

# 🔹 Deadlock Detection

Deadlocks can sometimes be detected using monitoring tools and JVM management APIs.

Java provides `ThreadMXBean` for thread monitoring.

Example:

```java
import java.lang.management.ManagementFactory;
import java.lang.management.ThreadInfo;
import java.lang.management.ThreadMXBean;

class DeadlockDetection {

    public static void main(String[] args) {

        ThreadMXBean bean =
                ManagementFactory.getThreadMXBean();

        long[] threadIds =
                bean.findDeadlockedThreads();

        if(threadIds != null) {

            ThreadInfo[] threadInfo =
                    bean.getThreadInfo(threadIds);

            for(ThreadInfo info : threadInfo) {

                System.out.println(info);
            }

        } else {

            System.out.println("No deadlock detected");
        }
    }
}
```

The key method is:

```java
findDeadlockedThreads()
```

It can identify threads involved in monitor/synchronizer deadlocks supported by the JVM's management facilities.

---

# 🔹 Deadlock vs Race Condition

These are different problems.

| Deadlock | Race Condition |
|---|---|
| Threads become blocked | Threads execute concurrently |
| Progress can stop | Results can become incorrect |
| Usually involves circular waiting | Usually involves unsafe shared-state access |
| Threads may wait indefinitely | Behavior may vary between runs |
| Lock ordering is a common concern | Atomicity/synchronization is a common concern |

### Deadlock

```text
T1 → waits for T2
T2 → waits for T1
```

### Race Condition

```text
T1 → modifies shared data
T2 → modifies shared data
        ↓
Unexpected result
```

---

# 🔹 Deadlock vs Starvation

### Deadlock

Threads are waiting in a cycle.

```text
T1 → T2
↑     ↓
└─────┘
```

No thread in the cycle can make progress.

---

### Starvation

A thread keeps getting denied access to a resource and may wait for an extremely long time.

Example:

```text
Thread 1 → repeatedly gets lock

Thread 2 → repeatedly waits
```

There does not have to be a circular dependency.

---

# 🔹 Deadlock vs Livelock

### Deadlock

Threads do nothing because they are blocked.

```text
T1 → waiting
T2 → waiting
```

---

### Livelock

Threads are active but continuously respond to each other without making useful progress.

Conceptually:

```text
T1 → changes state
T2 → responds
T1 → responds
T2 → responds
...
```

The threads are running, but the system does not progress.

---

# 🔹 Common Mistakes

## ❌ Mistake 1 — Thinking multiple synchronized blocks always cause deadlock

This is incorrect.

Multiple locks can be used safely.

The problem occurs when the locking design creates a circular wait.

---

## ❌ Mistake 2 — Acquiring locks in inconsistent order

Danger:

```text
Thread 1:
A → B

Thread 2:
B → A
```

Safer:

```text
Thread 1:
A → B

Thread 2:
A → B
```

---

## ❌ Mistake 3 — Holding locks for too long

Example:

```java
synchronized(lock) {

    performLongOperation();

    performAnotherLongOperation();

    performDatabaseOperation();
}
```

Holding locks unnecessarily for long operations can increase contention and make the system harder to reason about.

---

## ❌ Mistake 4 — Forgetting to unlock a Lock

When using explicit locks:

```java
lock.lock();

try {

    // work

} finally {

    lock.unlock();
}
```

The `finally` block is important.

---

## ❌ Mistake 5 — Assuming synchronized automatically prevents deadlock

`synchronized` provides mutual exclusion, but poor locking design can still produce deadlocks.

---

# 🔹 Interview Traps

### Q1. What is deadlock?

A deadlock occurs when threads are permanently blocked because each is waiting for a resource held by another thread in a cycle.

---

### Q2. What are the four Coffman conditions?

```text
1. Mutual Exclusion
2. Hold and Wait
3. No Preemption
4. Circular Wait
```

---

### Q3. Can synchronized cause deadlock?

Yes.

If multiple locks are acquired in inconsistent orders, deadlock can occur.

---

### Q4. How can deadlock be prevented?

Common techniques include:

- Consistent lock ordering
- Avoiding unnecessary nested locks
- Reducing lock scope
- Timeout-based acquisition
- `tryLock()`
- Higher-level concurrency utilities

---

### Q5. What is circular wait?

A cycle where each thread waits for a resource held by another thread in the cycle.

Example:

```text
T1 → waits for T2
T2 → waits for T1
```

---

### Q6. What is lock ordering?

A rule that all threads acquire multiple locks in the same predefined order.

Example:

```text
Always:
A → B

Never:
B → A
```

---

### Q7. Can a single thread cause deadlock?

A classic deadlock requires a waiting cycle involving multiple resource dependencies, typically across multiple threads. A single thread can, however, become blocked by other locking errors.

---

### Q8. What is the difference between deadlock and starvation?

Deadlock involves a circular waiting dependency.

Starvation occurs when a thread repeatedly fails to obtain the resources or CPU time it needs.

---

### Q9. What is livelock?

Threads remain active and repeatedly change state but fail to make useful progress.

---

### Q10. How can `tryLock()` help?

It allows a thread to attempt lock acquisition without necessarily waiting indefinitely.

---

# 🔹 DSA / Problem-Solving Relevance

Deadlock is mainly a concurrency concept rather than a traditional DSA pattern.

However, it is highly relevant to problems involving:

- Multiple locks
- Resource allocation
- Producer-consumer systems
- Concurrent graphs
- Thread pools
- Shared data structures
- Operating-system synchronization

A useful mental model is a **resource allocation graph**.

Example:

```text
T1 → Lock A → T2 → Lock B → T1
```

The cycle indicates a potential deadlock.

---

# 🔹 Problem-Solving Mindset

When analyzing possible deadlock, ask:

```text
1. How many locks/resources exist?
        ↓
2. Which thread owns each resource?
        ↓
3. Which resource does each thread want next?
        ↓
4. Can a thread hold one resource while waiting for another?
        ↓
5. Can resources be acquired in different orders?
        ↓
6. Is there a circular wait?
```

The most important question:

> **Can I draw a cycle in the waiting relationship?**

If yes, investigate for deadlock.

---

# 🔹 30-Second Interview Answer

> A deadlock is a concurrency problem where two or more threads become permanently blocked because each thread is waiting for a resource held by another thread. The classic four necessary conditions are mutual exclusion, hold and wait, no preemption, and circular wait. A common prevention technique is consistent lock ordering, where all threads acquire multiple locks in the same order. With explicit locks such as `ReentrantLock`, `tryLock()` and timeout-based acquisition can also help avoid indefinite waiting.

---

# 🔹 Cheat Sheet

## Deadlock

```text
Thread A
   ↓
holds Lock 1
   ↓
waits for Lock 2

Thread B
   ↓
holds Lock 2
   ↓
waits for Lock 1
```

---

## Four Coffman Conditions

```text
M → Mutual Exclusion
H → Hold and Wait
N → No Preemption
C → Circular Wait
```

Memory:

> **MHNC**

---

## Dangerous Lock Ordering

```text
T1: A → B

T2: B → A
```

Potential deadlock.

---

## Safe Consistent Ordering

```text
T1: A → B

T2: A → B
```

Avoids this particular circular-wait pattern.

---

## tryLock()

```java
if(lock.tryLock()) {

    try {

        // work

    } finally {

        lock.unlock();
    }
}
```

---

# 🧠 Memory Tricks

### Deadlock

> **"I have what you need, you have what I need."**

---

### Four Conditions

> **"Mutual Hold, No Preemption, Circular Wait."**

```text
M → Mutual Exclusion
H → Hold and Wait
N → No Preemption
C → Circular Wait
```

---

### Lock Ordering

> **"Same order, no circular wait from opposite ordering."**

```text
A → B
A → B
A → B
```

---

### Deadlock vs Race Condition

> **Deadlock = stuck**

> **Race condition = wrong/unpredictable result**

---

### Deadlock vs Livelock

> **Deadlock = not moving**

> **Livelock = moving but getting nowhere**

---

# 🔥 Top 10 Interview Questions

## 1. What is deadlock?

A deadlock is a state where threads become permanently blocked because they are waiting for resources held by one another.

---

## 2. What are the four conditions required for deadlock?

```text
Mutual Exclusion
Hold and Wait
No Preemption
Circular Wait
```

All four are required for the classic Coffman deadlock conditions.

---

## 3. Can synchronized cause deadlock?

Yes.

For example:

```text
T1: Lock A → Lock B

T2: Lock B → Lock A
```

This can create circular waiting.

---

## 4. How can deadlock be prevented?

Common approaches:

```text
Consistent lock ordering
Reduce nested locking
Keep critical sections small
Use tryLock()
Use timeouts
Use higher-level concurrency utilities
```

---

## 5. What is circular wait?

A circular dependency where each thread waits for a resource held by another thread in the cycle.

---

## 6. What is lock ordering?

A rule requiring all threads to acquire multiple locks in a consistent order.

---

## 7. What is the difference between deadlock and race condition?

```text
Deadlock
→ threads cannot make progress

Race condition
→ result depends on unsafe concurrent execution
```

---

## 8. What is starvation?

Starvation occurs when a thread is repeatedly denied the resources or execution time it needs to make progress.

---

## 9. What is livelock?

Livelock occurs when threads remain active and repeatedly react to one another but make no useful progress.

---

## 10. How does tryLock() help?

`tryLock()` lets a thread attempt to acquire a `Lock` without necessarily waiting indefinitely, making certain deadlock-avoidance strategies possible.

---

# 🎯 Final Summary

```text
                      DEADLOCK
                         |
                         ↓
                  Multiple Threads
                         |
                         ↓
                 Multiple Resources
                         |
                         ↓
                  Hold + Wait
                         |
                         ↓
                  Circular Wait
                         |
                         ↓
                  Threads Blocked
                         |
                         ↓
                 No Progress
```

### ⭐ Classic Example

```java
synchronized(lockA) {

    synchronized(lockB) {

        // Thread 1
    }
}
```

while another thread does:

```java
synchronized(lockB) {

    synchronized(lockA) {

        // Thread 2
    }
}
```

Possible dependency:

```text
Thread 1
   |
   | holds A
   ↓
 waits for B
   ↑
   |
Thread 2
   |
   | holds B
   ↓
 waits for A
```

### ⭐ Most Important Prevention

```text
Always acquire locks in the same order.

A → B
A → B
A → B
```

Avoid:

```text
A → B
B → A
```

### ⭐ One-Line Interview Memory

> **Deadlock occurs when threads hold resources while waiting for other resources in a circular dependency, causing them to remain blocked indefinitely.**