# 06 — Garbage Collection

## 📚 Table of Contents

1. [Introduction](#1-introduction)
2. [Why Garbage Collection?](#2-why-garbage-collection)
3. [What is Garbage Collection?](#3-what-is-garbage-collection)
4. [What is Garbage?](#4-what-is-garbage)
5. [Reachability](#5-reachability)
6. [How Garbage Collection Works](#6-how-garbage-collection-works)
7. [Object Eligibility for GC](#7-object-eligibility-for-gc)
8. [GC Roots](#8-gc-roots)
9. [Mark Phase](#9-mark-phase)
10. [Sweep Phase](#10-sweep-phase)
11. [Compaction](#11-compaction)
12. [Mark-Sweep-Compact](#12-mark-sweep-compact)
13. [Generational Garbage Collection](#13-generational-garbage-collection)
14. [Young Generation](#14-young-generation)
15. [Old Generation](#15-old-generation)
16. [Minor GC](#16-minor-gc)
17. [Major GC and Full GC](#17-major-gc-and-full-gc)
18. [Stop-The-World](#18-stop-the-world)
19. [GC Does Not Mean Immediate Deallocation](#19-gc-does-not-mean-immediate-deallocation)
20. [System.gc()](#20-systemgc)
21. [finalize() and Garbage Collection](#21-finalize-and-garbage-collection)
22. [Memory Leaks in Java](#22-memory-leaks-in-java)
23. [Common Causes of Memory Leaks](#23-common-causes-of-memory-leaks)
24. [Garbage Collection and Heap](#24-garbage-collection-and-heap)
25. [Garbage Collection and Stack](#25-garbage-collection-and-stack)
26. [Garbage Collection and Object References](#26-garbage-collection-and-object-references)
27. [Strong, Soft, Weak and Phantom References](#27-strong-soft-weak-and-phantom-references)
28. [Advantages](#28-advantages)
29. [Disadvantages](#29-disadvantages)
30. [Common Misconceptions](#30-common-misconceptions)
31. [Interview Traps](#31-interview-traps)
32. [Top 10 Interview Questions](#32-top-10-interview-questions)
33. [30-Second Interview Answer](#33-30-second-interview-answer)
34. [Cheat Sheet](#34-cheat-sheet)

---

# 1. Introduction

Garbage Collection (GC) is Java's automatic memory-management mechanism.

Its primary responsibility is to identify objects that are no longer reachable by the application and reclaim the heap memory occupied by those objects.

Java developers normally do not explicitly deallocate objects using operations such as `free()` or `delete`.

Instead, the JVM manages object memory using a Garbage Collector.

### Definition

> Garbage Collection is the JVM's automatic process of identifying unreachable objects and reclaiming their heap memory.

### Key Points

- GC is automatic.
- GC is managed by the JVM.
- GC primarily manages heap memory.
- GC is based on object reachability.
- GC is performed by a Garbage Collector.
- The programmer cannot force GC to run at an exact time.
- Different JVMs can use different Garbage Collectors.
- An object becoming eligible for GC does not mean it is immediately removed.

---

# 2. Why Garbage Collection?

In languages such as C and C++, programmers can manually manage dynamically allocated memory.

Manual memory management can cause problems such as:

- Memory leaks
- Dangling pointers
- Double deallocation
- Invalid memory access
- Use-after-free errors

Java reduces these problems by automatically reclaiming memory occupied by unreachable objects.

Example:

```java
public class Demo {

    public static void main(String[] args) {

        Employee employee = new Employee();

        employee = null;
    }
}
```

Initially:

```text
GC Root
   |
   v
employee
   |
   v
Employee Object
```

After:

```java
employee = null;
```

the reference is removed:

```text
GC Root
   |
   v
employee ---> null

Employee Object
      X
   unreachable
```

The object becomes eligible for GC.

---

# 3. What is Garbage Collection?

Garbage Collection is the automatic process through which the JVM identifies objects that are no longer reachable and reclaims their memory.

### Interview Definition

> Garbage Collection is an automatic JVM process that identifies unreachable objects in the heap and reclaims the memory occupied by them.

### Important

GC does not simply check whether a reference variable contains `null`.

It determines whether an object can still be reached from GC Roots.

Example:

```java
Employee e1 = new Employee();
Employee e2 = e1;

e1 = null;
```

After:

```java
e1 = null;
```

the object is still reachable through `e2`.

```text
GC Root
   |
   v
  e2
   |
   v
Employee Object
```

Therefore, the object is not garbage.

---

# 4. What is Garbage?

An object is considered garbage when there is no reachable reference path from any GC Root to that object.

Example:

```java
public class Demo {

    public static void main(String[] args) {

        Employee employee = new Employee();

        employee = null;
    }
}
```

After `employee = null`, if no other reference points to the object:

```text
GC Root
   |
   v
employee ---> null

Employee Object
      X
   unreachable
```

The object is eligible for Garbage Collection.

### Correct terminology

Do not say:

> "The object is immediately deleted."

Say:

> "The object has become eligible for Garbage Collection."

---

# 5. Reachability

Reachability is one of the most important concepts in Garbage Collection.

An object is reachable when a valid reference path exists from a GC Root to that object.

Example:

```java
Employee employee = new Employee();
```

Conceptually:

```text
GC Root
   |
   v
employee
   |
   v
Employee Object
```

The object is reachable.

Now:

```java
employee = null;
```

Conceptually:

```text
GC Root
   |
   v
employee ---> null

Employee Object
      X
```

The object becomes unreachable.

## 5.1 Reachability Through Another Object

Consider:

```java
Employee employee = new Employee();
Department department = new Department(employee);

employee = null;
```

Even though `employee` is `null`, the `Employee` object may still be reachable:

```text
GC Root
   |
   v
department
   |
   v
Department Object
   |
   v
Employee Object
```

Therefore:

```text
employee == null
```

does NOT necessarily mean:

```text
Employee Object == garbage
```

### Core Rule

> GC is based on reachability, not simply on whether a reference variable is null.

---

# 6. How Garbage Collection Works

The exact implementation depends on the Garbage Collector being used.

However, the basic conceptual process can be understood as:

```text
1. Find GC Roots
       ↓
2. Trace reachable objects
       ↓
3. Identify unreachable objects
       ↓
4. Reclaim memory
       ↓
5. Optionally compact memory
```

A simplified traditional algorithm is:

```text
Mark
  ↓
Sweep
  ↓
Compact
```

Modern collectors can use more advanced algorithms and perform these activities concurrently.

### Important

There is no single Garbage Collection algorithm used by every JVM.

Different collectors use different strategies depending on goals such as:

- Throughput
- Latency
- Pause time
- Heap size
- Application requirements

---

# 7. Object Eligibility for GC

An object becomes eligible for Garbage Collection when it can no longer be reached from any GC Root.

## 7.1 Nulling a Reference

```java
Employee employee = new Employee();

employee = null;
```

If no other reference points to the object, it becomes eligible.

---

## 7.2 Reassigning a Reference

```java
Employee employee = new Employee();

employee = new Employee();
```

The first `Employee` object may become unreachable.

Conceptually:

```text
Before:

employee
   |
   v
Object A


After:

employee
   |
   v
Object B

Object A
   X
```

Object A becomes eligible for GC if there are no other references to it.

---

## 7.3 Local Object Becoming Unreachable

```java
public static void createEmployee() {

    Employee employee = new Employee();
}
```

After the method returns, the local reference is gone.

If there are no other references to the object:

```text
Employee Object
      X
unreachable
```

The object becomes eligible for GC.

---

## 7.4 Isolated Objects

Objects can become unreachable even when they reference each other.

Example:

```java
class A {
    B b;
}

class B {
    A a;
}
```

Suppose:

```java
A first = new A();
B second = new B();

first.b = second;
second.a = first;

first = null;
second = null;
```

The objects may still reference each other:

```text
Object A  <----->  Object B
```

But there may be no path from a GC Root to either object.

Therefore, both are unreachable and can become eligible for GC.

### Important Interview Point

Java GC can collect cyclic references.

It does not rely only on reference counting.

---

# 8. GC Roots

GC Roots are special references from which the Garbage Collector begins determining object reachability.

Conceptually:

```text
GC Roots
   |
   +----> Reachable Object
   |
   +----> Reachable Object
              |
              +----> Reachable Object
```

Objects that cannot be reached from GC Roots are candidates for collection.

## Common GC Roots

Common examples include:

- Active local variables and parameters in thread stacks
- References from active Java threads
- Static references
- References held by JVM internal structures
- JNI references
- Other JVM-defined root references

### Important

A GC Root is not simply:

> "Every variable."

It is a reference considered a root by the JVM's reachability analysis.

---

# 9. Mark Phase

The Mark phase identifies objects that are reachable.

The Garbage Collector starts from GC Roots and traverses references.

Example:

```text
GC Root
   |
   v
Object A
   |
   +----> Object B
   |
   +----> Object C
```

All these objects are reachable.

Conceptually:

```text
GC Root
   |
   v
[A] ---> [B]
 |
 v
[C]
```

The collector marks these objects as reachable.

An unreachable object:

```text
GC Root
   |
   v
[A]

[B]    [C]
```

may remain unmarked.

### Purpose of Mark Phase

> Determine which objects are still alive/reachable.

---

# 10. Sweep Phase

After marking reachable objects, the collector can reclaim memory belonging to unreachable objects.

Example:

```text
Before Sweep:

[A] Reachable
[B] Garbage
[C] Reachable
[D] Garbage
```

After sweep:

```text
[A] Available
[Free Memory]
[C] Available
[Free Memory]
```

The memory occupied by unreachable objects becomes available for future allocations.

### Problem With Basic Mark-Sweep

It can create fragmentation.

Example:

```text
[Used][Free][Used][Free][Used][Free]
```

There may be enough total free memory, but it can be split into many small regions.

This leads to the need for compaction.

---

# 11. Compaction

Compaction moves live objects closer together so that free memory becomes contiguous.

Before compaction:

```text
[Used][Free][Used][Free][Used][Free]
```

After compaction:

```text
[Used][Used][Used][Free][Free][Free]
```

### Benefits

Compaction can:

- Reduce fragmentation
- Create larger contiguous free regions
- Make future allocation easier

### Cost

Moving objects can be expensive.

References pointing to moved objects must remain valid.

The JVM handles the necessary reference updates.

---

# 12. Mark-Sweep-Compact

A simplified traditional GC algorithm can combine:

```text
Mark
  ↓
Sweep
  ↓
Compact
```

### Step 1 — Mark

Find reachable objects.

```text
[A] Live
[B] Garbage
[C] Live
[D] Garbage
```

### Step 2 — Sweep

Reclaim garbage objects.

```text
[A] Live
[Free]
[C] Live
[Free]
```

### Step 3 — Compact

Move live objects together.

```text
[A] [C] [Free] [Free]
```

### Important

Modern Garbage Collectors are more sophisticated than this simplified model.

Some collectors:

- perform marking concurrently
- perform compaction selectively
- divide the heap into regions
- use evacuation
- perform parts of GC concurrently with application threads

Therefore, `mark → sweep → compact` is a conceptual model, not a description of every modern collector.

---

# 13. Generational Garbage Collection

Java applications usually create many short-lived objects.

Example:

```java
for (int i = 0; i < 1_000_000; i++) {

    Employee employee = new Employee();
}
```

Many objects created inside the loop may become unreachable quickly.

The JVM can exploit this behavior using generational approaches.

The basic idea is:

> Objects are grouped according to their age, and young objects are collected more frequently than old objects.

Conceptually:

```text
Heap
 |
 +----------------------+
 | Young Generation     |
 |                      |
 | Eden                 |
 | Survivor             |
 +----------------------+
 |
 +----------------------+
 | Old Generation       |
 +----------------------+
```

### Generational Hypothesis

A common observation is:

> Most newly created objects become unreachable relatively quickly.

Therefore:

```text
Young objects → collect frequently
Old objects   → collect less frequently
```

This can improve GC efficiency.

---

# 14. Young Generation

The Young Generation is the area associated with newly allocated objects in generational heap designs.

Conceptually it contains:

```text
Young Generation
 |
 +---- Eden
 |
 +---- Survivor
 |
 +---- Survivor
```

The survivor regions are commonly called:

```text
S0
S1
```

### Object Lifecycle

A simplified lifecycle:

```text
Object Created
      ↓
    Eden
      ↓
 Minor GC
      ↓
Survivor Space
      ↓
More Minor GCs
      ↓
Object ages
      ↓
Old Generation
```

Not every object necessarily follows exactly this path because modern collectors may use different allocation and promotion strategies.

---

# 15. Old Generation

The Old Generation contains objects that have survived enough collection cycles or otherwise meet the collector's criteria for being treated as long-lived.

Conceptually:

```text
Young Generation
       |
       | surviving objects
       v
Old Generation
```

Example:

```java
Employee employee = new Employee();

while (applicationIsRunning) {
    // employee remains reachable
}
```

If the object remains alive for a long time, the collector may eventually treat it as an old/long-lived object.

### Why Separate Young and Old Objects?

Because their lifetimes are different.

```text
Many objects:
Created → quickly unreachable

Few objects:
Created → remain alive for a long time
```

Therefore, collecting young objects frequently can be more efficient than scanning the entire heap every time.

---

# 16. Minor GC

A Minor GC traditionally refers to collection involving the Young Generation.

Typical simplified flow:

```text
Eden
  |
  | Minor GC
  v
Reachable objects
  |
  v
Survivor Space
```

Objects that remain reachable can survive into survivor spaces.

Objects that are no longer reachable can have their memory reclaimed.

### Important

The exact terminology can vary between Garbage Collectors and JVM implementations.

Do not assume that every JVM collector uses exactly the same generational structure.

---

# 17. Major GC and Full GC

The terms `Major GC` and `Full GC` are often used in discussions, but their exact meanings can vary by JVM, collector, and documentation.

### Major GC

Often refers to a collection involving the Old Generation.

### Full GC

Generally refers to a collection involving a much larger portion of the heap and potentially other memory-related structures.

A Full GC can be significantly more expensive than a small young-generation collection.

### Interview Tip

Avoid saying:

> "Major GC always means exactly X."

A safer answer is:

> "The terminology varies between collectors. Traditionally, Major GC refers to old-generation collection, while Full GC generally involves the entire heap or a broad set of memory regions."

---

# 18. Stop-The-World

A Stop-The-World (STW) pause means application threads are temporarily paused so the JVM can perform certain GC-related work safely.

Conceptually:

```text
Application Threads
       |
       v
     PAUSE
       |
       v
    GC Work
       |
       v
    RESUME
```

### Important

Stop-The-World does not necessarily mean:

> "The entire JVM is always stopped for the entire Garbage Collection."

Modern collectors can perform substantial GC work concurrently with application threads.

However, many collectors still require some STW phases.

### Why Pause Application Threads?

Because certain operations may require a consistent view of object references and heap state.

---

# 19. GC Does Not Mean Immediate Deallocation

Consider:

```java
Employee employee = new Employee();

employee = null;
```

This means:

```text
Employee Object
      ↓
Eligible for GC
```

It does NOT mean:

```text
Employee Object
      ↓
Immediately destroyed
```

There may be a delay between:

```text
Object becomes unreachable
```

and:

```text
GC reclaims its memory
```

### Important

The JVM decides:

- when GC should run
- which objects to collect
- which collection algorithm to use
- how much memory to reclaim

The Java program does not directly control the exact timing.

---

# 20. System.gc()

Java provides:

```java
System.gc();
```

This requests that the JVM perform garbage collection.

Example:

```java
public class Demo {

    public static void main(String[] args) {

        System.gc();
    }
}
```

### Important

`System.gc()` is only a request.

It does not guarantee that GC will run immediately.

Conceptually:

```text
System.gc()
     |
     v
GC Request
     |
     v
JVM decides what to do
```

### Interview Question

**Does `System.gc()` force Garbage Collection?**

No.

It requests GC, but the JVM is not required to perform it immediately.

---

# 21. finalize() and Garbage Collection

Historically, Java provided:

```java
protected void finalize()
```

The idea was to allow an object to perform cleanup before being reclaimed.

However, finalization has serious problems and has been deprecated for removal in modern Java.

Example of the old concept:

```java
@Override
protected void finalize() throws Throwable {
    System.out.println("Cleanup");
}
```

### Important

Do not use `finalize()` for resource management.

For resources such as:

- files
- sockets
- database connections
- streams

use explicit resource-management mechanisms such as:

```java
try (FileInputStream input = new FileInputStream("data.txt")) {
    // use resource
}
```

### Interview Point

> `finalize()` should not be relied upon for timely or guaranteed cleanup.

Modern Java recommends alternatives such as:

- try-with-resources
- `AutoCloseable`
- explicit `close()`
- `Cleaner` for specialized cases

---

# 22. Memory Leaks in Java

Java has Garbage Collection, but Java applications can still suffer from memory leaks.

A memory leak occurs when an application unintentionally keeps references to objects that it no longer needs.

Because those objects remain reachable, GC cannot reclaim them.

Example:

```java
List<Employee> employees = new ArrayList<>();

employees.add(new Employee());
```

If the application keeps adding objects to a long-lived collection without removing objects that are no longer needed:

```text
GC Root
   |
   v
Long-lived List
   |
   +----> Employee
   +----> Employee
   +----> Employee
   +----> Employee
   +----> ...
```

Those objects remain reachable.

Therefore:

```text
Reachable
    ↓
Not garbage
    ↓
Memory remains occupied
```

### Core Insight

> Garbage Collection prevents many manual memory-management problems, but it cannot detect that a reachable object is logically unnecessary.

---

# 23. Common Causes of Memory Leaks

## 23.1 Static Collections

```java
static List<Employee> employees = new ArrayList<>();
```

If objects are continuously added and never removed:

```text
GC Root
   |
   v
static List
   |
   +----> Object
   +----> Object
   +----> Object
```

The objects remain reachable.

---

## 23.2 Unremoved Listeners

If an object registers a listener and the listener is never removed, references may remain longer than intended.

---

## 23.3 Caches

Poorly designed caches can keep objects alive indefinitely.

---

## 23.4 ThreadLocal

Improper use of `ThreadLocal` can retain objects for the lifetime of a long-running thread.

---

## 23.5 Long-Lived References

A long-lived object holding references to short-lived objects can prevent those objects from being collected.

### General Pattern

```text
Long-lived Object
       |
       v
Unnecessarily retained Object
       |
       v
Cannot be collected
```

---

# 24. Garbage Collection and Heap

Garbage Collection primarily deals with objects allocated in the heap.

Example:

```java
Employee employee = new Employee();
```

Conceptually:

```text
Stack
  |
  | reference
  v
Heap
  |
  v
Employee Object
```

The reference variable may exist in a stack frame, while the actual object is in the heap.

If the stack reference disappears and there are no other references:

```text
Stack
  |
  X

Heap
  |
  v
Employee Object
  X
```

The object can become unreachable.

### Important

Do not say:

> "GC cleans the stack."

GC primarily manages heap objects.

The JVM manages stack frames separately as method calls enter and leave.

---

# 25. Garbage Collection and Stack

Consider:

```java
public static void createEmployee() {

    Employee employee = new Employee();
}
```

During method execution:

```text
Stack Frame
   |
   +---- employee
            |
            v
         Heap Object
```

When the method returns, its stack frame is removed:

```text
Stack Frame
   X
```

If no other reference points to the heap object:

```text
Heap Object
      X
unreachable
```

The object becomes eligible for GC.

### Important Distinction

```text
Stack:
Contains method frames and local references.

Heap:
Contains objects and arrays.
```

The disappearance of a stack reference can make a heap object unreachable.

---

# 26. Garbage Collection and Object References

Consider:

```java
Employee e1 = new Employee();
Employee e2 = e1;
```

There is one object:

```text
        +----------------+
e1 ---->|                |
        | Employee       |
e2 ---->|    Object      |
        +----------------+
```

Now:

```java
e1 = null;
```

The object is still reachable:

```text
e1 ---> null

e2 ----------------+
                   |
                   v
              Employee
```

Now:

```java
e2 = null;
```

If no other references exist:

```text
e1 ---> null
e2 ---> null

Employee Object
      X
unreachable
```

The object becomes eligible for GC.

### Key Rule

> An object can be collected only when no GC Root can reach it.

---

# 27. Strong, Soft, Weak and Phantom References

Java provides different reference strengths through the `java.lang.ref` package.

These are useful for specialized memory-management scenarios.

---

## 27.1 Strong Reference

Normal Java references are strong references.

Example:

```java
Employee employee = new Employee();
```

As long as a strong reference provides reachability to the object, the object normally cannot be reclaimed by GC.

Conceptually:

```text
Strong Reference
      |
      v
   Object
```

---

## 27.2 Soft Reference

A `SoftReference` allows an object to be reclaimed when the JVM needs memory, subject to the JVM's handling of soft references.

Example:

```java
SoftReference<Employee> reference =
        new SoftReference<>(new Employee());
```

Soft references have historically been useful for memory-sensitive caches.

However, their exact reclamation behavior should not be treated as a precise cache policy.

---

## 27.3 Weak Reference

A `WeakReference` does not keep an object strongly reachable.

Example:

```java
WeakReference<Employee> reference =
        new WeakReference<>(new Employee());
```

If no strong references remain, the object can become eligible for collection.

Weak references are useful in specialized structures such as weak-key maps.

---

## 27.4 Phantom Reference

`PhantomReference` is intended for advanced post-mortem processing and coordination with the reference-processing mechanism.

It does not provide normal access to the referent.

Example:

```java
PhantomReference<Employee> reference =
        new PhantomReference<>(employee, queue);
```

Phantom references are an advanced topic and should not be confused with normal object references.

### Reference Strength Summary

```text
Strong
   ↓
Object strongly reachable

Soft
   ↓
Can be reclaimed under memory pressure

Weak
   ↓
Does not strongly keep object alive

Phantom
   ↓
Advanced lifecycle/reference processing
```

---

# 28. Advantages

## 28.1 Automatic Memory Management

Developers do not normally need to manually free objects.

---

## 28.2 Reduces Memory-Management Errors

It reduces problems such as:

- double free
- dangling pointers
- use-after-free caused by manual deallocation

---

## 28.3 Automatic Reclamation

Unused unreachable objects can have their memory reclaimed automatically.

---

## 28.4 Developer Productivity

Developers can focus more on application logic instead of manually tracking every object deallocation.

---

## 28.5 Different GC Strategies

The JVM provides different collectors designed for different application requirements.

---

# 29. Disadvantages

Garbage Collection also has costs.

## 29.1 CPU Overhead

GC consumes CPU resources.

---

## 29.2 Pause Times

Some GC operations can introduce application pauses.

---

## 29.3 Memory Overhead

Garbage Collectors require metadata and additional runtime structures.

---

## 29.4 Unpredictable Timing

The programmer cannot precisely determine when a particular object will be reclaimed.

---

## 29.5 Memory Leaks Are Still Possible

If objects remain reachable unnecessarily, GC cannot reclaim them.

---

# 30. Common Misconceptions

## Misconception 1

> GC destroys objects immediately after `reference = null`.

### Correct

The object becomes eligible for GC.

---

## Misconception 2

> `System.gc()` guarantees GC.

### Correct

It only requests GC.

---

## Misconception 3

> GC checks only null references.

### Correct

GC uses reachability analysis starting from GC Roots.

---

## Misconception 4

> Java cannot have memory leaks.

### Correct

Java can have memory leaks when unnecessary references are retained.

---

## Misconception 5

> Circular references cannot be collected.

### Correct

Java's reachability-based GC can collect isolated cyclic object graphs.

---

## Misconception 6

> GC cleans stack memory.

### Correct

GC primarily manages heap objects.

---

## Misconception 7

> There is one Garbage Collector in Java.

### Correct

The JVM can provide different Garbage Collectors with different algorithms and goals.

---

## Misconception 8

> `finalize()` is guaranteed to execute before an object is removed.

### Correct

Finalization is deprecated and should not be relied upon for cleanup.

---

# 31. Interview Traps

### Trap 1: Null reference vs garbage

```java
Employee e1 = new Employee();
Employee e2 = e1;

e1 = null;
```

Question:

> Is the Employee object garbage?

Answer:

> No, because `e2` still provides a reachable reference.

---

### Trap 2: Circular reference

```text
A → B
↑   ↓
└───┘
```

Question:

> Can GC collect them?

Answer:

> Yes, if the entire cycle is unreachable from GC Roots.

---

### Trap 3: System.gc()

Question:

> Does `System.gc()` force GC?

Answer:

> No. It only requests GC.

---

### Trap 4: Immediate memory release

Question:

> When an object becomes unreachable, is its memory immediately returned to the OS?

Answer:

> Not necessarily. The collector may reclaim the memory later, and heap memory management is controlled by the JVM.

---

### Trap 5: GC and stack

Question:

> Does Garbage Collection remove local variables from the stack?

Answer:

> No. Stack frames are managed as methods execute and return. GC primarily handles heap objects.

---

### Trap 6: Reachability

Question:

> If an object has no variable directly pointing to it, is it always garbage?

Answer:

> No. Another reachable object may still reference it.

---

### Trap 7: Memory leak

Question:

> Can a reachable object cause a memory leak?

Answer:

> Yes. If the application unintentionally retains a reference to an object that it no longer needs, GC considers it reachable and cannot reclaim it.

---

# 32. Top 10 Interview Questions

## Q1. What is Garbage Collection?

### Answer

Garbage Collection is an automatic JVM process that identifies unreachable objects and reclaims their heap memory.

---

## Q2. How does Java determine whether an object is garbage?

### Answer

The JVM determines object reachability starting from GC Roots. If an object cannot be reached from any GC Root, it becomes eligible for collection.

---

## Q3. What are GC Roots?

### Answer

GC Roots are special references from which the JVM starts reachability analysis. Examples include active thread-stack references, static references, active threads, and certain JVM/JNI references.

---

## Q4. Can an object become garbage even when it references another object?

### Answer

Yes.

If the entire object graph is unreachable from GC Roots, all objects in that graph can become eligible for collection.

---

## Q5. Can Java collect cyclic references?

### Answer

Yes.

Java's GC is based on reachability rather than simple reference counting, so isolated cyclic object graphs can be collected.

---

## Q6. Does `System.gc()` guarantee Garbage Collection?

### Answer

No.

`System.gc()` only requests that the JVM perform GC. The JVM decides whether and when to perform it.

---

## Q7. What is the difference between Minor GC and Full GC?

### Answer

Traditionally, Minor GC refers to collection focused on the Young Generation, while Full GC generally involves a much broader part of the heap. Exact terminology and behavior depend on the Garbage Collector.

---

## Q8. What is Stop-The-World?

### Answer

Stop-The-World means application threads are temporarily paused while the JVM performs certain operations that require them to be stopped. Modern collectors can perform substantial GC work concurrently, but may still have STW phases.

---

## Q9. Can Java have memory leaks?

### Answer

Yes.

Java can have logical memory leaks when applications unintentionally keep references to objects that are no longer needed. Because those objects remain reachable, GC cannot reclaim them.

---

## Q10. What happens when an object becomes eligible for GC?

### Answer

It means the object is no longer reachable from GC Roots and may be reclaimed by the Garbage Collector. It does not mean the object is immediately destroyed.

---

# 33. 30-Second Interview Answer

> Garbage Collection is Java's automatic memory-management mechanism. The JVM identifies objects that are no longer reachable from GC Roots and reclaims their heap memory. A simplified GC process can involve marking reachable objects, reclaiming unreachable objects, and sometimes compacting memory. Java commonly uses generational strategies because many objects are short-lived. GC timing is controlled by the JVM, so `System.gc()` only requests collection and does not guarantee it. Java can still experience memory leaks when unnecessary objects remain reachable through long-lived references.

---

# 34. Cheat Sheet

```text
╔══════════════════════════════════════════════════════╗
║              GARBAGE COLLECTION CHEAT SHEET         ║
╠══════════════════════════════════════════════════════╣
║ GC               → Automatic JVM memory management   ║
║ Main target      → Heap objects                     ║
║ Garbage          → Unreachable object               ║
║ Reachability     → Path from GC Root to object       ║
║ GC Root          → Starting point for reachability  ║
║ Mark             → Find reachable objects            ║
║ Sweep            → Reclaim unreachable objects       ║
║ Compact          → Reduce fragmentation              ║
║ Young Generation → New/short-lived objects           ║
║ Old Generation   → Long-lived objects                ║
║ Minor GC         → Traditionally young-gen focused   ║
║ Full GC          → Broad heap collection             ║
║ STW              → Application threads paused        ║
║ System.gc()      → Request, NOT guarantee            ║
║ finalize()       → Deprecated; don't rely on it      ║
║ Memory leak      → Unwanted reachable objects        ║
╚══════════════════════════════════════════════════════╝
```

## 🧠 Memory Trick

```text
GC = "Reachability → Reclaim"

GC Root
   ↓
Reachable?
   ├── YES → Keep object
   │
   └── NO  → Eligible for GC
```

### Most Important Rule

> **An object is eligible for Garbage Collection when it is no longer reachable from any GC Root.**

### Remember

```text
null reference
     ≠
immediate deletion

eligible for GC
     ≠
already collected

System.gc()
     ≠
guaranteed GC

reachable
     ≠
garbage

unreachable
     =
eligible for GC
```

---

# 🔥 Final Interview Summary

```text
Java Program
     |
     v
Objects created
     |
     v
Heap
     |
     v
References connect objects
     |
     v
GC Roots
     |
     v
Reachability Analysis
     |
     +-------------------+
     |                   |
 Reachable          Unreachable
     |                   |
     v                   v
   Keep              Eligible for GC
                         |
                         v
                   Memory Reclaimed
```

> **GC does not ask "Is the reference variable null?"**
>
> **GC asks "Can this object still be reached from a GC Root?"**

That is the core idea behind Java Garbage Collection.