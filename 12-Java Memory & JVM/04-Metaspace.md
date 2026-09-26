# 🧠 Metaspace in Java

> **Metaspace** is a HotSpot JVM memory area introduced in Java 8 to store class metadata. Unlike the old PermGen, Metaspace uses **native memory** rather than the Java heap.

---

## 📌 Table of Contents

1. [Introduction](#-introduction)
2. [Why Was Metaspace Introduced?](#-why-was-metaspace-introduced)
3. [PermGen Before Java 8](#-permgen-before-java-8)
4. [PermGen Problems](#-permgen-problems)
5. [Metaspace in Java 8+](#-metaspace-in-java-8)
6. [Method Area vs Metaspace](#-method-area-vs-metaspace)
7. [What Does Metaspace Store?](#-what-does-metaspace-store)
8. [Class Metadata](#-class-metadata)
9. [Class Loading and Metaspace](#-class-loading-and-metaspace)
10. [Metaspace and Heap](#-metaspace-and-heap)
11. [Metaspace and Native Memory](#-metaspace-and-native-memory)
12. [Metaspace Growth](#-metaspace-growth)
13. [Metaspace and Class Unloading](#-metaspace-and-class-unloading)
14. [ClassLoader and Metaspace](#-classloader-and-metaspace)
15. [Metaspace Memory Leak](#-metaspace-memory-leak)
16. [OutOfMemoryError: Metaspace](#-outofmemoryerror-metaspace)
17. [Metaspace Configuration](#-metaspace-configuration)
18. [`-XX:MetaspaceSize`](#-xxmetaspacesize)
19. [`-XX:MaxMetaspaceSize`](#-xxmaxmetaspacesize)
20. [Compressed Class Pointers](#-compressed-class-pointers)
21. [Metaspace and Garbage Collection](#-metaspace-and-garbage-collection)
22. [Metaspace and Dynamic Class Generation](#-metaspace-and-dynamic-class-generation)
23. [Example of Excessive Class Loading](#-example-of-excessive-class-loading)
24. [Monitoring Metaspace](#-monitoring-metaspace)
25. [Common Misconceptions](#-common-misconceptions)
26. [Interview Traps](#-interview-traps)
27. [Quick Comparison](#-quick-comparison)
28. [Cheat Sheet](#-cheat-sheet)
29. [30-Second Interview Answer](#-30-second-interview-answer)
30. [Top 10 Interview Questions](#-top-10-interview-questions)

---

# 🔹 Introduction

In previous versions of HotSpot, class metadata was largely stored in a memory area called:

```text
PermGen
```

Starting with:

```text
Java 8
```

HotSpot removed PermGen and introduced:

```text
Metaspace
```

The major difference is:

```text
PermGen
→ Java heap

Metaspace
→ Native memory
```

This change made class metadata management more flexible.

---

# 🔹 Why Was Metaspace Introduced?

The old PermGen had several limitations.

A major issue was that its size was constrained by a fixed or configured region of the Java heap.

If too much class metadata was loaded:

```text
Class loading
     ↓
PermGen fills
     ↓
Not enough space
     ↓
OutOfMemoryError
```

Developers often had to carefully tune:

```text
-XX:PermSize
-XX:MaxPermSize
```

Metaspace changed this model.

Instead of using a fixed heap region for class metadata, HotSpot moved class metadata to native memory.

---

# 🔹 PermGen Before Java 8

Before Java 8, HotSpot had:

```text
Java Heap
│
├── Young Generation
├── Old Generation
└── Permanent Generation (PermGen)
```

PermGen was part of the heap in HotSpot.

It contained various class-related structures and other data associated with the permanent generation.

---

# 🔹 PermGen Problems

Suppose:

```text
PermGen size = limited
```

and the application dynamically loads many classes.

Then:

```text
Class 1
Class 2
Class 3
...
Class N
   ↓
PermGen fills
   ↓
No sufficient space
   ↓
OutOfMemoryError
```

Applications that dynamically generated classes could therefore encounter:

```text
java.lang.OutOfMemoryError:
PermGen space
```

---

# 🔹 Metaspace in Java 8+

Java 8 removed PermGen from HotSpot.

The conceptual model became:

```text
JVM
│
├── Java Heap
│
└── Native Memory
      |
      └── Metaspace
            |
            └── Class Metadata
```

Therefore:

> **Metaspace is outside the Java heap in HotSpot.**

---

# 🔥 Method Area vs Metaspace

This distinction is extremely important.

### Method Area

A **JVM specification concept**.

It defines a logical runtime data area containing class/interface-related information.

### Metaspace

A **HotSpot implementation detail**.

It is the native-memory-based implementation used for class metadata since Java 8.

Therefore:

```text
Method Area
     ↓
Specification
     ↓
HotSpot implementation
     ↓
Metaspace
```

But:

```text
Method Area ≠ Metaspace
```

as general JVM terminology.

---

# 🔹 What Does Metaspace Store?

Metaspace primarily stores **class metadata**.

This includes implementation-specific structures associated with loaded classes.

Conceptually:

```text
Metaspace
│
├── Class metadata
├── Method metadata
├── Field metadata
├── Class hierarchy information
├── Runtime class structures
└── Other HotSpot class-related metadata
```

The exact internal structures depend on the HotSpot version and implementation.

---

# 🔹 Class Metadata

Consider:

```java
class Employee {

    int id;

    String name;

    void work() {
        System.out.println("Working");
    }
}
```

When this class is loaded, the JVM needs information describing:

```text
Employee
│
├── Class identity
├── Superclass
├── Interfaces
├── Fields
├── Methods
├── Access information
└── Other runtime metadata
```

HotSpot stores relevant class metadata in Metaspace.

---

# 🔹 Class Loading and Metaspace

Consider:

```java
Student s = new Student();
```

Before the JVM can use `Student`, the class must be loaded if it has not already been loaded.

Conceptually:

```text
Student.class
      ↓
ClassLoader
      ↓
Class loading
      ↓
Class metadata created
      ↓
Metaspace
      ↓
Student class available
```

Then:

```java
new Student()
```

can create an instance.

Conceptually:

```text
Metaspace
Student class metadata
        |
        ↓
Heap
Student object
```

---

# 🔹 Metaspace and Heap

This is one of the most important distinctions.

Consider:

```java
class Student {

    int age;
}
```

and:

```java
Student s = new Student();
```

Conceptually:

```text
Metaspace                         Heap

Student class metadata           Student object
       |                              |
       |                              |
       +------------------------------+
                  |
             class information
```

The `Student` object is a heap object.

The metadata describing the `Student` class is associated with Metaspace in HotSpot.

---

# 🔹 What Happens to Object Fields?

Suppose:

```java
class Student {

    int age;
    String name;
}
```

When:

```java
Student s = new Student();
```

the object's instance state:

```text
age
name
```

belongs to the `Student` object.

Conceptually:

```text
Heap

Student object
┌─────────────────┐
│ age             │
│ name reference  │
└─────────────────┘
```

Metaspace does **not** store the individual `age` and `name` values for every Student object.

It stores class metadata describing the class.

---

# 🔹 Metaspace and Native Memory

Metaspace uses **native memory**.

This means it is not part of the normal Java heap.

Conceptually:

```text
Process Memory
│
├── Java Heap
│
├── Metaspace
│
├── Thread Stacks
│
├── Code Cache
│
└── Other Native Memory
```

Therefore:

```text
-Xmx
```

controls the Java heap maximum.

It does **not** directly define the maximum Metaspace size.

---

# 🔥 Important

Suppose:

```text
-Xmx512m
```

This means the maximum Java heap is configured around 512 MB.

It does **not** mean:

```text
Entire JVM memory = 512 MB
```

The JVM can use additional native memory for:

- Metaspace
- Thread stacks
- Code cache
- Native libraries
- JVM internal structures
- Other native allocations

---

# 🔹 Metaspace Growth

Unlike a fixed-size PermGen region, Metaspace can dynamically grow as more class metadata is needed.

Conceptually:

```text
Application starts
       ↓
Few classes
       ↓
Small Metaspace usage
       ↓
More classes loaded
       ↓
Metaspace grows
```

The JVM requests native memory as required.

---

# 🔹 Metaspace and Class Unloading

Class metadata can be reclaimed when classes are unloaded.

This requires the class to become eligible for unloading.

A major factor is the class loader.

Conceptually:

```text
ClassLoader
    |
    +── Class A
    +── Class B
    +── Class C
```

If the class loader and relevant classes become unreachable and the JVM determines they can be unloaded:

```text
ClassLoader becomes unreachable
          ↓
Classes become unloadable
          ↓
Class unloading
          ↓
Class metadata can be reclaimed
```

---

# 🔥 Why ClassLoader Matters

Suppose an application repeatedly creates class loaders:

```text
ClassLoader 1
    ↓
Many classes

ClassLoader 2
    ↓
Many classes

ClassLoader 3
    ↓
Many classes
```

If those class loaders cannot be collected because something still references them:

```text
ClassLoader 1 → alive
ClassLoader 2 → alive
ClassLoader 3 → alive
```

their classes may remain loaded.

Therefore:

```text
More loaded classes
       ↓
More metadata
       ↓
More Metaspace usage
```

This can eventually cause:

```text
OutOfMemoryError: Metaspace
```

---

# 🔹 Metaspace Memory Leak

A "Metaspace leak" often means that classes continue to remain loaded because their class loaders cannot be unloaded.

Example scenario:

```text
Web application deployed
       ↓
ClassLoader created
       ↓
Classes loaded
       ↓
Application redeployed
       ↓
New ClassLoader created
       ↓
Old ClassLoader still referenced
       ↓
Old classes cannot unload
       ↓
Metaspace keeps growing
```

This is especially relevant in:

- Application servers
- Plugin architectures
- Hot deployment
- Dynamic proxies
- Runtime bytecode generation
- Framework-heavy applications

---

# 🔹 OutOfMemoryError: Metaspace

If Metaspace cannot satisfy a class-metadata allocation request, HotSpot can throw:

```text
java.lang.OutOfMemoryError: Metaspace
```

A simplified chain:

```text
More classes
    ↓
More metadata
    ↓
More Metaspace
    ↓
Native memory limit reached
    ↓
OutOfMemoryError: Metaspace
```

---

# 🔹 Example of Excessive Class Loading

Consider a framework that continuously generates new classes.

Conceptually:

```java
while (true) {

    generateNewClass();

}
```

If each generated class remains loaded:

```text
Class 1
Class 2
Class 3
...
Class 100000
```

then:

```text
Class metadata
      ↓
Metaspace usage increases
```

Eventually:

```text
OutOfMemoryError: Metaspace
```

---

# 🔹 Dynamic Proxies

Java applications can generate classes dynamically.

For example, frameworks may use:

- Dynamic proxies
- Bytecode generation
- Runtime enhancement
- ORM-generated classes
- Dependency injection infrastructure
- Reflection-related infrastructure

If dynamically generated classes are continually created and retained, Metaspace usage can grow.

---

# 🔹 Metaspace Configuration

HotSpot provides JVM options for configuring Metaspace.

Two important options are:

```text
-XX:MetaspaceSize
```

and:

```text
-XX:MaxMetaspaceSize
```

---

# 🔹 `-XX:MetaspaceSize`

`MetaspaceSize` is related to the initial/high-water-mark threshold that influences when a metadata-related GC is triggered.

It is **not simply "the initial Metaspace allocation size."**

Example:

```bash
java -XX:MetaspaceSize=128m MyApp
```

Do not interpret this as:

```text
Metaspace starts with exactly 128 MB allocated
```

It is more accurately understood as a threshold used by HotSpot's class-metadata/GC management.

---

# 🔹 `-XX:MaxMetaspaceSize`

This option limits the maximum amount of native memory that HotSpot can use for Metaspace.

Example:

```bash
java -XX:MaxMetaspaceSize=256m MyApp
```

Conceptually:

```text
Metaspace
    |
    | grows
    ↓
256 MB limit
    ↓
Unable to allocate more
    ↓
OutOfMemoryError
```

---

# 🔹 Why Configure `MaxMetaspaceSize`?

Possible reasons include:

- Preventing unlimited native-memory growth
- Making memory usage more predictable
- Detecting class-loading problems earlier
- Controlling JVM resource usage in constrained environments

But setting it too low can cause:

```text
OutOfMemoryError: Metaspace
```

even for a healthy application.

---

# 🔹 Compressed Class Pointers

HotSpot can use **compressed class pointers** as an optimization on supported configurations.

A reference associated with an object's class is called a:

> **Klass pointer / class pointer**

Instead of using a full-width pointer in every object header, HotSpot can sometimes use a compressed representation.

Conceptually:

```text
Object Header
┌───────────────────────────────┐
│ Mark Word                     │
├───────────────────────────────┤
│ Compressed Class Pointer      │
└───────────────────────────────┘
```

This can reduce memory overhead for object headers.

---

## Important

Compressed class pointers are:

- HotSpot implementation details
- Architecture/configuration dependent
- Different from ordinary Java references

Do not confuse:

```text
Compressed Class Pointer
```

with:

```text
Java object reference
```

---

# 🔹 Metaspace and Garbage Collection

Metaspace itself is not simply collected like ordinary Java heap objects.

Instead, class metadata can be reclaimed through:

```text
Class unloading
```

which is associated with garbage collection and class-loader reachability.

Conceptually:

```text
GC cycle
   ↓
Can classes be unloaded?
   ↓
Yes
   ↓
Unload classes
   ↓
Reclaim associated metadata
```

The exact behavior depends on the selected GC and JVM implementation.

---

# 🔹 Metaspace and Class Unloading

Consider:

```text
Application
     |
     ↓
ClassLoader
     |
     +---- A.class
     +---- B.class
     +---- C.class
```

If:

```text
ClassLoader
     ↓
becomes unreachable
```

and no other relevant references prevent unloading:

```text
A, B, C
  ↓
Class unloading
  ↓
Metadata reclaimed
```

This is why class-loader lifecycle is directly related to Metaspace management.

---

# 🔹 Example: Web Application Redeployment

Imagine:

```text
Server starts
      ↓
Application V1
      ↓
ClassLoader V1
      ↓
Loads 5,000 classes
```

Then application is redeployed:

```text
Application V2
      ↓
ClassLoader V2
      ↓
Loads another 5,000 classes
```

If ClassLoader V1 becomes unreachable:

```text
ClassLoader V1
      ↓
unreachable
      ↓
classes can potentially unload
      ↓
metadata reclaimed
```

But if some object accidentally retains ClassLoader V1:

```text
Some static object
      ↓
ClassLoader V1
      ↓
5,000 classes
```

then those classes may remain loaded.

Repeated redeployments can cause:

```text
Metaspace
  ↑
  ↑
  ↑
  ↑
```

and eventually:

```text
OutOfMemoryError: Metaspace
```

---

# 🔹 Monitoring Metaspace

When diagnosing memory problems, useful JVM tools include:

```text
jcmd
jstat
jmap
JFR
VisualVM
```

For example:

```bash
jcmd <pid> VM.native_memory summary
```

Native Memory Tracking can help investigate JVM native-memory usage when enabled appropriately.

Class-loading statistics can also be inspected with JVM diagnostic tools.

---

# 🔹 Useful JVM Options

### Print class loading/unloading information

Depending on the JDK version, unified logging can be used:

```bash
-Xlog:class+load=info
```

and:

```bash
-Xlog:class+unload=info
```

These can help determine whether classes are continuously being loaded or unloaded.

---

# 🔹 Metaspace vs Heap

| Feature | Metaspace | Heap |
|---|---|---|
| HotSpot-specific? | Yes | JVM concept + implementation |
| Main purpose | Class metadata | Objects/arrays |
| Memory type in HotSpot | Native memory | Java heap |
| GC | Class unloading related | Object garbage collection |
| Stores normal objects | No | Yes |
| Stores arrays | No | Yes |
| Java 8 change | Introduced | No equivalent change |
| Maximum option | `MaxMetaspaceSize` | `-Xmx` |
| Typical OOME | `Metaspace` | `Java heap space` |

---

# 🔹 Metaspace vs PermGen

| Feature | PermGen | Metaspace |
|---|---|---|
| Used by HotSpot | Before Java 8 | Java 8+ |
| Memory location | Java heap | Native memory |
| Class metadata | Yes | Yes |
| Maximum option | `MaxPermSize` | `MaxMetaspaceSize` |
| Main limitation | Fixed heap region | Native-memory availability / configured limit |
| Status | Removed in Java 8 | Current HotSpot approach |

---

# 🔹 Common Misconceptions

## ❌ 1. Metaspace is the same as Method Area

Not exactly.

```text
Method Area = specification concept
Metaspace = HotSpot implementation
```

---

## ❌ 2. Metaspace is part of the Java heap

Wrong for HotSpot.

Metaspace uses native memory.

---

## ❌ 3. Metaspace stores all objects

Wrong.

Normal Java objects and arrays are allocated in the heap.

---

## ❌ 4. Metaspace stores every String

Wrong.

Normal `String` objects are heap objects.

Interned strings are also represented as heap objects in modern HotSpot.

---

## ❌ 5. `-Xmx` limits Metaspace

No.

`-Xmx` controls the Java heap maximum.

Metaspace has separate configuration, notably:

```text
-XX:MaxMetaspaceSize
```

---

## ❌ 6. Metaspace has an unlimited size

Not exactly.

By default, it can grow based on available native memory and JVM policies, but it can be explicitly limited using:

```text
-XX:MaxMetaspaceSize
```

The process is also limited by the operating system/container's available memory.

---

## ❌ 7. More objects always means more Metaspace

Wrong.

Creating millions of ordinary objects primarily affects heap usage.

Metaspace usage is mainly associated with loaded class metadata.

---

## ❌ 8. Garbage Collection immediately removes unused classes

Not necessarily.

Class unloading depends on whether classes and their class loaders are eligible for unloading and on the GC/JVM behavior.

---

# 🔹 Interview Traps

### Trap 1

**Q: What replaced PermGen in Java 8?**

Answer:

> HotSpot replaced PermGen with Metaspace for class metadata storage.

Do not say:

> "The Method Area was replaced."

---

### Trap 2

**Q: Is Metaspace inside the heap?**

Answer:

> No. In HotSpot, Metaspace uses native memory outside the Java heap.

---

### Trap 3

**Q: Which option limits heap memory?**

Answer:

```text
-Xmx
```

Not:

```text
-XX:MaxMetaspaceSize
```

---

### Trap 4

**Q: Which option limits Metaspace?**

Answer:

```text
-XX:MaxMetaspaceSize
```

---

### Trap 5

**Q: Can a class be unloaded while an instance exists?**

A class generally cannot be unloaded while instances or other references requiring the class remain reachable.

Class unloading is tied to the class loader becoming eligible for unloading and the JVM's class-unloading conditions.

---

### Trap 6

**Q: Why can a class-loader leak cause Metaspace OOME?**

Because if an old class loader remains reachable, its loaded classes can remain loaded, preventing their metadata from being reclaimed.

Repeated loading can therefore increase Metaspace usage.

---

# 🔹 Quick Comparison

```text
                 MEMORY
                   |
       +-----------+-----------+
       |                       |
    Java Heap             Native Memory
       |                       |
 Objects / Arrays           Metaspace
                               |
                         Class Metadata
```

---

# 🔹 Complete Relationship

```text
                 Java Source
                      |
                      ↓
                   javac
                      |
                      ↓
                 .class file
                      |
                      ↓
                 ClassLoader
                      |
                      ↓
               Class Loading
                      |
                      ↓
              Class Metadata
                      |
                      ↓
                  Metaspace
                      |
                      ↓
              Class available
                      |
                      ↓
                new Student()
                      |
                      ↓
                    Heap
                      |
                      ↓
              Student object
```

---

# 🔹 Cheat Sheet

```text
METASPACE
│
├── HotSpot implementation
├── Introduced in Java 8
├── Replaced PermGen
├── Uses native memory
├── Stores class metadata
├── Outside Java heap
│
├── Related to class loading
├── Related to class unloading
├── Depends heavily on ClassLoader lifecycle
│
├── -XX:MetaspaceSize
│   └── Threshold influencing metadata GC behavior
│
└── -XX:MaxMetaspaceSize
    └── Maximum Metaspace size
```

### Errors

```text
Heap exhausted
      ↓
OutOfMemoryError: Java heap space

Metaspace exhausted
      ↓
OutOfMemoryError: Metaspace
```

---

# 🔥 30-Second Interview Answer

### Q: What is Metaspace?

> Metaspace is a HotSpot JVM implementation introduced in Java 8 to store class metadata using native memory instead of the Java heap. It replaced PermGen, which was used in older HotSpot versions. Metaspace grows dynamically as class metadata is needed and can be reclaimed when classes are unloaded. Class-loader leaks can prevent class unloading and cause continuous Metaspace growth, potentially resulting in `OutOfMemoryError: Metaspace`. `-XX:MaxMetaspaceSize` can be used to configure an upper limit.

---

# 🔥 Top 10 Interview Questions

## 1. What is Metaspace?

**Answer:**

Metaspace is a HotSpot JVM memory area introduced in Java 8 for storing class metadata in native memory.

---

## 2. What replaced PermGen?

**Answer:**

HotSpot replaced PermGen with Metaspace in Java 8.

---

## 3. Why was PermGen removed?

**Answer:**

PermGen had limitations because class metadata was stored in a fixed-size region of the Java heap. Metaspace provides a more flexible native-memory-based approach.

---

## 4. Is Metaspace part of the Java heap?

**Answer:**

No.

HotSpot Metaspace uses native memory outside the Java heap.

---

## 5. What does Metaspace store?

**Answer:**

It stores class metadata and other implementation-specific structures associated with loaded classes.

---

## 6. What is the difference between Metaspace and Method Area?

**Answer:**

Method Area is a JVM specification concept, while Metaspace is a HotSpot implementation used for class metadata storage.

---

## 7. What causes `OutOfMemoryError: Metaspace`?

**Answer:**

It can occur when HotSpot cannot allocate additional native memory for class metadata, especially due to excessive class loading, dynamically generated classes, or class-loader leaks.

---

## 8. What is a class-loader leak?

**Answer:**

A class-loader leak occurs when an otherwise obsolete class loader remains reachable, preventing its loaded classes from being unloaded and their metadata from being reclaimed.

---

## 9. What is `-XX:MaxMetaspaceSize`?

**Answer:**

It sets an upper limit on the amount of native memory HotSpot can use for Metaspace.

---

## 10. Does `-Xmx` control Metaspace?

**Answer:**

No.

`-Xmx` controls the maximum Java heap size.

Metaspace is native memory and has separate configuration.

---

# 🧠 Final Memory Trick

```text
Java 7 and earlier:
PermGen
    ↓
Class metadata
    ↓
Heap

Java 8+ HotSpot:
Metaspace
    ↓
Class metadata
    ↓
Native Memory
```

Remember:

```text
Method Area
    ↓
JVM SPECIFICATION

Metaspace
    ↓
HOTSPOT IMPLEMENTATION

PermGen
    ↓
OLD HOTSPOT IMPLEMENTATION
```

And:

```text
-Xmx
    ↓
Java Heap

-XX:MaxMetaspaceSize
    ↓
Metaspace
```

> ⭐ **Golden Rule:**  
> **Metaspace is not "Java's class memory" as a universal JVM concept. It is HotSpot's native-memory implementation for class metadata. The specification-level concept is the Method Area.**