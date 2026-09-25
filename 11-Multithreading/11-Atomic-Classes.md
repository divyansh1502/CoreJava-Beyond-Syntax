# 11 — Atomic Classes

> **Atomic classes provide thread-safe operations on single variables without requiring traditional `synchronized` blocks for those atomic operations.**

---

## 📌 Table of Contents

1. [Introduction](#-introduction)
2. [What Are Atomic Classes?](#-what-are-atomic-classes)
3. [Why Atomic Classes Are Needed](#-why-atomic-classes-are-needed)
4. [The Problem with `count++`](#-the-problem-with-count)
5. [AtomicInteger](#-atomicinteger)
6. [Creating AtomicInteger](#-creating-atomicinteger)
7. [get()](#-get)
8. [set()](#-set)
9. [incrementAndGet()](#-incrementandget)
10. [getAndIncrement()](#-getandincrement)
11. [decrementAndGet()](#-decrementandget)
12. [getAndDecrement()](#-getanddecrement)
13. [addAndGet()](#-addandget)
14. [getAndAdd()](#-getandadd)
15. [compareAndSet()](#-compareandset)
16. [compareAndExchange()](#-compareandexchange)
17. [AtomicInteger Example](#-atomicinteger-example)
18. [AtomicLong](#-atomiclong)
19. [AtomicBoolean](#-atomicboolean)
20. [AtomicReference](#-atomicreference)
21. [CAS — Compare-And-Swap](#-cas--compare-and-swap)
22. [How Atomic Classes Work Internally](#-how-atomic-classes-work-internally)
23. [Atomic Classes vs volatile](#-atomic-classes-vs-volatile)
24. [Atomic Classes vs synchronized](#-atomic-classes-vs-synchronized)
25. [Atomic Classes vs Locks](#-atomic-classes-vs-locks)
26. [When to Use Atomic Classes](#-when-to-use-atomic-classes)
27. [When Not to Use Atomic Classes](#-when-not-to-use-atomic-classes)
28. [Common Mistakes](#-common-mistakes)
29. [Interview Traps](#-interview-traps)
30. [DSA / Problem-Solving Relevance](#-dsa--problem-solving-relevance)
31. [30-Second Interview Answer](#-30-second-interview-answer)
32. [Cheat Sheet](#-cheat-sheet)
33. [Top 10 Interview Questions](#-top-10-interview-questions)

---

# 🔹 Introduction

In multithreaded programs, multiple threads may need to update the same variable.

Consider:

```java
int count = 0;
```

Two threads performing:

```java
count++;
```

can cause a race condition.

One solution is:

```java
synchronized
```

Another solution for simple atomic updates is:

```java
AtomicInteger
```

Atomic classes are available in:

```java
java.util.concurrent.atomic
```

---

# 🔹 What Are Atomic Classes?

Atomic classes are classes in the Java Concurrency API that provide atomic operations on individual variables.

Important classes include:

```text
AtomicInteger
AtomicLong
AtomicBoolean
AtomicReference
```

They are located in:

```java
java.util.concurrent.atomic
```

Example:

```java
AtomicInteger count = new AtomicInteger(0);
```

Then:

```java
count.incrementAndGet();
```

performs an atomic increment.

---

# 🔹 Why Atomic Classes Are Needed

Consider:

```java
class Counter {

    int count = 0;

    void increment() {

        count++;
    }
}
```

Multiple threads can execute:

```java
counter.increment();
```

simultaneously.

The problem is:

```text
Thread 1 → Read count
Thread 2 → Read count
Thread 1 → Write count + 1
Thread 2 → Write count + 1
```

An update can be lost.

Atomic classes provide operations designed to perform such updates atomically.

---

# 🔹 The Problem with `count++`

Consider:

```java
int count = 0;

count++;
```

It looks like one operation.

Conceptually, it is:

```text
Read count
     ↓
Add 1
     ↓
Write count
```

With multiple threads:

```text
T1 → Read 0
T2 → Read 0

T1 → Calculate 1
T2 → Calculate 1

T1 → Write 1
T2 → Write 1
```

Final:

```text
1
```

Expected:

```text
2
```

This is a classic lost-update problem.

---

# 🔹 AtomicInteger

`AtomicInteger` is used when multiple threads need to safely update an integer.

Import:

```java
import java.util.concurrent.atomic.AtomicInteger;
```

Example:

```java
AtomicInteger count = new AtomicInteger(0);
```

Increment:

```java
count.incrementAndGet();
```

Read:

```java
count.get();
```

---

# 🔹 Creating AtomicInteger

You can create an `AtomicInteger` with an initial value:

```java
AtomicInteger count = new AtomicInteger(0);
```

Or:

```java
AtomicInteger count = new AtomicInteger();
```

The default value is:

```text
0
```

Example:

```java
AtomicInteger count = new AtomicInteger();

System.out.println(count.get());
```

Output:

```text
0
```

---

# 🔹 get()

`get()` returns the current value.

```java
AtomicInteger count = new AtomicInteger(10);

System.out.println(count.get());
```

Output:

```text
10
```

---

# 🔹 set()

`set()` changes the current value.

```java
AtomicInteger count = new AtomicInteger(10);

count.set(50);

System.out.println(count.get());
```

Output:

```text
50
```

---

# 🔹 incrementAndGet()

`incrementAndGet()`:

```text
Increment
   ↓
Return updated value
```

Example:

```java
AtomicInteger count = new AtomicInteger(10);

int result = count.incrementAndGet();

System.out.println(result);
```

Output:

```text
11
```

After execution:

```text
count = 11
```

---

# 🔹 getAndIncrement()

`getAndIncrement()`:

```text
Return current value
        ↓
Increment
```

Example:

```java
AtomicInteger count = new AtomicInteger(10);

int result = count.getAndIncrement();

System.out.println(result);
System.out.println(count.get());
```

Output:

```text
10
11
```

Important difference:

```text
incrementAndGet()
→ increment first
→ return new value

getAndIncrement()
→ return old value
→ increment afterward
```

---

# 🔹 decrementAndGet()

`decrementAndGet()` decreases the value and returns the updated value.

```java
AtomicInteger count = new AtomicInteger(10);

int result = count.decrementAndGet();

System.out.println(result);
```

Output:

```text
9
```

---

# 🔹 getAndDecrement()

`getAndDecrement()` returns the current value and then decreases it.

```java
AtomicInteger count = new AtomicInteger(10);

int result = count.getAndDecrement();

System.out.println(result);
System.out.println(count.get());
```

Output:

```text
10
9
```

---

# 🔹 addAndGet()

`addAndGet(value)` adds a value and returns the updated result.

```java
AtomicInteger count = new AtomicInteger(10);

int result = count.addAndGet(5);

System.out.println(result);
```

Output:

```text
15
```

---

# 🔹 getAndAdd()

`getAndAdd(value)` returns the old value and then adds the specified amount.

```java
AtomicInteger count = new AtomicInteger(10);

int result = count.getAndAdd(5);

System.out.println(result);
System.out.println(count.get());
```

Output:

```text
10
15
```

---

# 🔹 compareAndSet()

`compareAndSet()` is one of the most important atomic operations.

Syntax:

```java
compareAndSet(expectedValue, newValue)
```

It means:

> If the current value equals the expected value, replace it with the new value.

Example:

```java
AtomicInteger count = new AtomicInteger(10);

boolean changed = count.compareAndSet(10, 20);

System.out.println(changed);
System.out.println(count.get());
```

Output:

```text
true
20
```

Because:

```text
Current = 10
Expected = 10
```

So:

```text
10 → 20
```

---

# 🔹 compareAndSet() Failure

Example:

```java
AtomicInteger count = new AtomicInteger(10);

boolean changed = count.compareAndSet(5, 20);

System.out.println(changed);
System.out.println(count.get());
```

Output:

```text
false
10
```

Why?

Because:

```text
Current value = 10
Expected value = 5
```

They are different.

Therefore the update does not happen.

---

# 🔹 compareAndExchange()

`compareAndExchange()` also compares the current value with an expected value.

Example:

```java
AtomicInteger count = new AtomicInteger(10);

int previous = count.compareAndExchange(10, 20);

System.out.println(previous);
System.out.println(count.get());
```

Output:

```text
10
20
```

Unlike `compareAndSet()`, which returns a boolean, `compareAndExchange()` returns the observed previous value.

---

# 🔹 AtomicInteger Example

A complete counter:

```java
import java.util.concurrent.atomic.AtomicInteger;

class Counter {

    private final AtomicInteger count =
            new AtomicInteger(0);

    void increment() {

        count.incrementAndGet();
    }

    int getCount() {

        return count.get();
    }
}
```

Multiple threads:

```java
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

Expected:

```text
200000
```

The increment operation is performed atomically.

---

# 🔹 AtomicLong

`AtomicLong` provides atomic operations for `long` values.

Import:

```java
import java.util.concurrent.atomic.AtomicLong;
```

Example:

```java
AtomicLong count = new AtomicLong(0);

count.incrementAndGet();

System.out.println(count.get());
```

Output:

```text
1
```

Useful for things such as:

```text
Counters
IDs
Statistics
Sequence numbers
```

---

# 🔹 AtomicBoolean

`AtomicBoolean` provides atomic operations for boolean values.

Import:

```java
import java.util.concurrent.atomic.AtomicBoolean;
```

Example:

```java
AtomicBoolean running =
        new AtomicBoolean(true);

if(running.compareAndSet(true, false)) {

    System.out.println("Stopped");
}
```

This is useful when a state transition must happen atomically.

For example:

```text
true → false
```

only if the current value is still:

```text
true
```

---

# 🔹 AtomicReference

`AtomicReference<T>` provides atomic operations on object references.

Example:

```java
import java.util.concurrent.atomic.AtomicReference;

AtomicReference<String> name =
        new AtomicReference<>("Java");

System.out.println(name.get());

name.set("Spring");

System.out.println(name.get());
```

Output:

```text
Java
Spring
```

It can also perform CAS operations:

```java
AtomicReference<String> name =
        new AtomicReference<>("Java");

boolean changed =
        name.compareAndSet("Java", "Spring");

System.out.println(changed);
```

Output:

```text
true
```

---

# 🔹 CAS — Compare-And-Swap

CAS stands for:

> **Compare-And-Swap**

The basic idea is:

```text
Read current value
       ↓
Compare with expected value
       ↓
Are they equal?
       ↓
   Yes → update
   No  → do nothing / retry
```

Example:

```java
count.compareAndSet(10, 20);
```

means:

```text
If count == 10
    ↓
change count to 20
```

Otherwise:

```text
Do not change it
```

---

# 🔹 How Atomic Classes Work Internally

Atomic classes commonly use **CAS-based atomic operations** and JVM/hardware-supported atomic primitives.

Conceptually:

```text
Thread
  ↓
Read current value
  ↓
Compare with expected value
  ↓
If unchanged
  ↓
Perform atomic update
```

If another thread changes the value before the CAS succeeds:

```text
Expected value ≠ Current value
```

the CAS fails.

A higher-level operation may then retry.

This is commonly associated with **lock-free** algorithms.

Important:

> You should not think of atomic classes as simply "using no synchronization." They provide atomicity through JVM and hardware-supported mechanisms rather than ordinary Java monitor locking.

---

# 🔹 Atomic Classes vs volatile

This is a very important comparison.

| `volatile` | Atomic Classes |
|---|---|
| Visibility | Visibility |
| Ordering | Ordering |
| No general compound-operation atomicity | Atomic operations |
| Simple reads/writes | Increment, CAS, update operations |
| No lock | Typically CAS-based operations |
| `volatile int count` | `AtomicInteger count` |

Example:

```java
volatile int count;
```

does not make:

```java
count++;
```

atomic.

But:

```java
AtomicInteger count =
        new AtomicInteger();

count.incrementAndGet();
```

does.

---

# 🔹 Atomic Classes vs synchronized

Both can provide safe updates, but their mechanisms and use cases differ.

### synchronized

```java
synchronized void increment() {

    count++;
}
```

Uses:

```text
Intrinsic lock
     ↓
Critical section
     ↓
Unlock
```

### AtomicInteger

```java
count.incrementAndGet();
```

Uses atomic operations such as CAS where applicable.

Comparison:

| Atomic Classes | synchronized |
|---|---|
| Excellent for simple atomic state | Useful for larger critical sections |
| CAS-based operations | Monitor-based locking |
| No explicit lock required | Uses intrinsic lock |
| Individual atomic variables | Multiple related operations/state |
| Simple counters are a common use | Complex invariants are a common use |

---

# 🔹 Atomic Classes vs Locks

For a simple counter:

```java
AtomicInteger count =
        new AtomicInteger();
```

can be very convenient.

With an explicit lock:

```java
Lock lock = new ReentrantLock();

lock.lock();

try {

    count++;

} finally {

    lock.unlock();
}
```

Locks are more suitable when multiple operations or multiple variables need to be protected as one critical section.

---

# 🔹 When to Use Atomic Classes

Use atomic classes when you need:

### 1. Atomic counter

```java
AtomicInteger count =
        new AtomicInteger();
```

---

### 2. Atomic ID generation

```java
AtomicLong id =
        new AtomicLong(1000);

long nextId =
        id.incrementAndGet();
```

---

### 3. Atomic state transition

```java
AtomicBoolean started =
        new AtomicBoolean(false);

started.compareAndSet(false, true);
```

---

### 4. Atomic reference updates

```java
AtomicReference<String> state =
        new AtomicReference<>("NEW");
```

---

# 🔹 When Not to Use Atomic Classes

Atomic classes are not automatically the best solution for every concurrency problem.

For example, suppose two variables must change together:

```java
balance
transactionCount
```

If the operation requires:

```text
Update balance
+
Update transactionCount
```

as one indivisible operation, using two separate atomic variables may not be enough.

You may need:

```text
synchronized
Lock
Immutable state + atomic reference
Other concurrency design
```

depending on the requirements.

---

# 🔹 Common Mistakes

## ❌ Mistake 1 — Thinking AtomicInteger is just volatile int

Wrong.

```java
volatile int count;
```

and:

```java
AtomicInteger count;
```

provide different capabilities.

---

## ❌ Mistake 2 — Using `get()` and `set()` as a compound operation

Consider:

```java
if(count.get() == 10) {

    count.set(20);
}
```

This entire sequence is not necessarily atomic.

Another thread can change `count` between:

```java
get()
```

and:

```java
set()
```

Use an appropriate atomic operation such as:

```java
compareAndSet(10, 20);
```

when the state transition itself must be atomic.

---

## ❌ Mistake 3 — Confusing `incrementAndGet()` with `getAndIncrement()`

Remember:

```text
incrementAndGet()
→ increment first
→ return new value
```

```text
getAndIncrement()
→ return old value
→ increment
```

---

## ❌ Mistake 4 — Assuming AtomicReference makes the object immutable

Example:

```java
AtomicReference<Person> person;
```

The reference update can be atomic.

It does not automatically make:

```java
Person
```

internally thread-safe.

---

## ❌ Mistake 5 — Using AtomicInteger for everything

Atomic classes are excellent for suitable atomic state operations.

They are not a universal replacement for:

```text
Locks
synchronized
Concurrent collections
Higher-level concurrency utilities
```

---

# 🔹 Interview Traps

### Q1. What is an atomic operation?

An operation that appears indivisible to other threads: it either happens as a single atomic state transition or does not happen.

---

### Q2. What is AtomicInteger?

A class that provides atomic operations on an `int` value.

---

### Q3. Why is AtomicInteger better than volatile int for a counter?

Because AtomicInteger provides atomic update operations such as:

```java
incrementAndGet()
```

while `volatile int` does not make:

```java
count++;
```

atomic.

---

### Q4. What is CAS?

CAS means:

```text
Compare-And-Swap
```

It updates a value only if the current value still matches the expected value.

---

### Q5. What does compareAndSet() return?

A boolean:

```text
true
→ update succeeded

false
→ expected value did not match
```

---

### Q6. Difference between incrementAndGet() and getAndIncrement()?

```text
incrementAndGet()
→ new value

getAndIncrement()
→ old value
```

---

### Q7. Is AtomicInteger lock-free?

Its individual atomic operations are designed around lock-free atomic mechanisms such as CAS on supported platforms, but the exact progress guarantees should be considered at the operation/class level rather than assuming every composite algorithm using it is lock-free.

---

### Q8. Can AtomicInteger prevent every race condition?

No.

It protects the atomic operations it provides, but a larger multi-variable or multi-step operation may still require additional synchronization/design.

---

### Q9. AtomicInteger or synchronized?

For a simple atomic counter, AtomicInteger is often a natural choice.

For a larger critical section involving multiple variables or operations, `synchronized` or an explicit lock may be more appropriate.

---

### Q10. What package contains atomic classes?

```java
java.util.concurrent.atomic
```

---

# 🔹 DSA / Problem-Solving Relevance

Atomic classes are mainly a concurrency topic, but they become useful when DSA operations are performed concurrently.

### Shared Counter

```java
AtomicInteger count =
        new AtomicInteger();

count.incrementAndGet();
```

---

### Unique ID Generation

```java
AtomicLong id =
        new AtomicLong();

long nextId =
        id.incrementAndGet();
```

---

### Concurrent State

```java
AtomicReference<String> state =
        new AtomicReference<>("WAITING");
```

---

### CAS-Based Algorithms

CAS is important in understanding:

```text
Lock-free algorithms
Non-blocking algorithms
Concurrent data structures
```

The main DSA connection is understanding how shared state can be updated safely without treating every operation as a traditional lock-based critical section.

---

# 🔹 Problem-Solving Mindset

When you see a shared variable, ask:

```text
1. Is it accessed by multiple threads?
        ↓
2. Does it need atomic updates?
        ↓
3. Is it only a simple read/write?
        ↓
4. Do I need CAS?
        ↓
5. Are multiple variables involved?
        ↓
6. Does the whole operation need to be atomic?
```

Example:

```java
count++;
```

Ask:

```text
Do I need atomic increment?
        ↓
Yes
        ↓
AtomicInteger
```

But:

```text
balance update
+
transaction update
+
logging
```

may require a larger synchronization strategy.

---

# 🔹 30-Second Interview Answer

> Atomic classes are part of Java's `java.util.concurrent.atomic` package and provide thread-safe atomic operations on individual variables. Common classes include `AtomicInteger`, `AtomicLong`, `AtomicBoolean`, and `AtomicReference`. They commonly use CAS-based atomic operations. For example, `AtomicInteger.incrementAndGet()` provides an atomic increment, unlike `count++` on a volatile integer. They are useful for simple shared state such as counters, while larger multi-variable critical sections may require locks or synchronization.

---

# 🔹 Cheat Sheet

## AtomicInteger

```java
AtomicInteger count =
        new AtomicInteger(0);
```

---

## Read

```java
count.get();
```

---

## Write

```java
count.set(10);
```

---

## Increment

```java
count.incrementAndGet();
```

---

## Increment — Return Old Value

```java
count.getAndIncrement();
```

---

## Decrement

```java
count.decrementAndGet();
```

---

## Add

```java
count.addAndGet(5);
```

---

## CAS

```java
count.compareAndSet(10, 20);
```

---

## AtomicLong

```java
AtomicLong value =
        new AtomicLong(0);
```

---

## AtomicBoolean

```java
AtomicBoolean flag =
        new AtomicBoolean(false);
```

---

## AtomicReference

```java
AtomicReference<String> state =
        new AtomicReference<>("NEW");
```

---

# 🧠 Memory Tricks

### AtomicInteger

> **"Integer + Atomic Operations."**

---

### CAS

> **"If expected is still there, replace it."**

```text
Expected == Current
        ↓
     Update
```

---

### `incrementAndGet()`

> **"Increment → Get new."**

```text
10 → 11
return 11
```

---

### `getAndIncrement()`

> **"Get old → Increment."**

```text
10 → 11
return 10
```

---

### volatile vs AtomicInteger

> **"Volatile helps you see it; AtomicInteger helps you update it atomically."**

---

# 🔥 Top 10 Interview Questions

## 1. What are atomic classes?

Classes from:

```java
java.util.concurrent.atomic
```

that provide atomic operations on shared variables.

---

## 2. Why do we need AtomicInteger?

To perform atomic operations such as incrementing an integer safely across threads.

---

## 3. Is `count++` atomic?

No.

It is a read-modify-write operation.

---

## 4. What is CAS?

Compare-And-Swap is an atomic operation that updates a value only when it still equals an expected value.

---

## 5. What does compareAndSet() do?

```java
count.compareAndSet(expected, update);
```

If:

```text
current == expected
```

then:

```text
current = update
```

and it returns:

```text
true
```

Otherwise:

```text
false
```

---

## 6. Difference between getAndIncrement() and incrementAndGet()?

```text
getAndIncrement()
→ returns old value

incrementAndGet()
→ returns new value
```

---

## 7. AtomicInteger vs volatile int?

```text
volatile int
→ visibility + ordering

AtomicInteger
→ visibility + ordering + atomic operations
```

---

## 8. AtomicInteger vs synchronized?

```text
AtomicInteger
→ ideal for simple atomic state operations

synchronized
→ useful for larger critical sections and multiple related state changes
```

---

## 9. Can AtomicInteger replace every lock?

No.

It is designed for atomic operations on individual variables, not arbitrary multi-step critical sections.

---

## 10. Where are atomic classes located?

```java
java.util.concurrent.atomic
```

---

# 🎯 Final Summary

```text
                    ATOMIC CLASSES
                           |
                           ↓
             java.util.concurrent.atomic
                           |
          +----------------+----------------+
          |                |                |
   AtomicInteger      AtomicLong      AtomicBoolean
          |
          ↓
   Atomic Operations
          |
          ↓
         CAS
          |
          ↓
   Safe State Updates
```

### ⭐ Classic Problem

```java
volatile int count = 0;

count++;
```

Not atomic.

---

### ⭐ Atomic Solution

```java
AtomicInteger count =
        new AtomicInteger(0);

count.incrementAndGet();
```

---

### ⭐ CAS

```java
count.compareAndSet(10, 20);
```

Means:

```text
If current == 10
       ↓
change to 20
```

---

### ⭐ Most Important Difference

```text
volatile
   ↓
Visibility + Ordering

AtomicInteger
   ↓
Visibility + Ordering
       +
Atomic Operations

synchronized
   ↓
Mutual Exclusion
       +
Visibility + Ordering
```

---

### ⭐ One-Line Interview Memory

> **Atomic classes provide atomic, thread-safe operations on shared variables, commonly using CAS, making them especially useful for counters and simple concurrent state updates.**