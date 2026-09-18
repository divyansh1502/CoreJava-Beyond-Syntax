# ☕ Java — Introduction

> **Write Once, Run Anywhere.**
> A high-level, class-based, object-oriented programming language designed to be portable, robust, secure, and widely applicable.

---

## 📌 What is Java?

**Java** is a high-level, general-purpose, class-based, object-oriented programming language developed by **Sun Microsystems** and first released publicly in **1995**.

Java was designed around an important idea:

```text
Source Code
     ↓
Java Compiler
     ↓
Bytecode (.class)
     ↓
JVM
     ↓
Machine Code
     ↓
CPU
```

Unlike languages that compile directly to a specific machine's native instructions, Java source code is compiled into **bytecode**, which is executed by the **Java Virtual Machine (JVM)**.

This architecture is one of the main reasons Java can run on different operating systems.

---

# 🧠 The Basic Idea Behind Java

Imagine writing:

```java
System.out.println("Hello Java");
```

You don't want to write a separate version for:

```text
Windows
Linux
macOS
```

Instead, Java follows this model:

```text
                    Java Source Code
                           │
                           ▼
                    ┌─────────────┐
                    │   javac     │
                    │  Compiler   │
                    └──────┬──────┘
                           │
                           ▼
                     Bytecode
                     Hello.class
                           │
              ┌────────────┼────────────┐
              ▼            ▼            ▼
            JVM          JVM          JVM
          Windows        Linux        macOS
              │            │            │
              ▼            ▼            ▼
            CPU          CPU          CPU
```

The **same bytecode** can be executed by a compatible JVM on different platforms.

---

# 🚀 Why Was Java Created?

Before Java became popular, developers often had to consider the target platform while developing software.

Java aimed to make software development:

* Portable
* Object-oriented
* Secure
* Robust
* Easier to develop
* Suitable for networked applications
* Less dependent on a specific operating system

One of Java's famous goals was:

> **Write Once, Run Anywhere (WORA)**

This does **not** mean that Java magically runs without any platform-specific software.

It means that the **same Java bytecode can run on different platforms when a suitable JVM exists for that platform.**

---

# 🏗️ Java's Design Philosophy

Java was designed around several major principles.

```text
                 JAVA
                  │
      ┌───────────┼───────────┐
      ▼           ▼           ▼
  Simplicity   Portability   Security
      │           │           │
      ▼           ▼           ▼
  Robustness  Reliability   Performance
      │           │           │
      └───────────┼───────────┘
                  ▼
             Productivity
```

These ideas influenced many of Java's language and runtime decisions.

---

# ⭐ Major Features of Java

## 1. High-Level Language

Java is a **high-level programming language**.

You work with abstractions such as:

```java
class
object
String
ArrayList
Thread
```

rather than directly manipulating CPU registers or raw memory addresses.

---

## 2. Object-Oriented

Java strongly supports object-oriented programming.

Its major OOP concepts include:

```text
Encapsulation
Inheritance
Polymorphism
Abstraction
```

Example:

```java
class Employee {

    private int id;
    private String name;

    void work() {
        System.out.println("Employee is working");
    }
}
```

OOP will be covered in much greater depth in:

```text
02-OOP/
```

---

## 3. Platform Independent

Java source code is compiled into **bytecode**.

```text
.java
  ↓
javac
  ↓
.class
  ↓
JVM
  ↓
Machine Code
```

The JVM is platform-specific, while Java bytecode is designed to be platform-independent.

Therefore:

```text
Windows JVM → executes bytecode
Linux JVM   → executes bytecode
macOS JVM   → executes bytecode
```

This is the foundation of Java's portability.

---

## 4. Robust

Java provides mechanisms that help developers write reliable programs.

Important examples include:

```text
Strong type checking
Exception handling
Automatic memory management
Garbage collection
Array bounds checking
```

For example:

```java
int[] arr = {10, 20, 30};

System.out.println(arr[10]);
```

Java detects that the requested index is invalid and throws an exception instead of allowing unrestricted memory access.

---

## 5. Secure

Java was designed with security in mind.

Some important aspects include:

```text
No direct pointer manipulation
Bytecode verification
Class loading mechanisms
Runtime checks
Access control
```

Java does **not expose raw memory pointers like C/C++**.

The pointer/reference distinction will be covered separately.

---

## 6. Automatic Memory Management

Java manages memory automatically using the JVM's memory-management mechanisms and **Garbage Collector (GC)**.

For example:

```java
Employee e = new Employee();
```

When an object is no longer reachable from the running program, it can eventually become eligible for garbage collection.

You don't normally write:

```text
free()
delete
```

for Java objects.

---

## 7. Multithreading Support

Java provides built-in support for concurrent programming.

For example:

```java
Thread t = new Thread(() -> {
    System.out.println("Running...");
});

t.start();
```

Java provides APIs for:

```text
Threads
Synchronization
Locks
Executors
Concurrent Collections
Atomic operations
```

Multithreading will be covered separately.

---

## 8. Rich Standard Library

Java provides a large standard library.

Examples:

```text
java.lang
java.util
java.io
java.nio
java.time
java.sql
java.net
```

For example:

```java
ArrayList<Integer> numbers = new ArrayList<>();
```

You don't have to implement a dynamic array from scratch.

---

## 9. Distributed / Network-Oriented

Java has extensive networking APIs and has historically been widely used for distributed applications and server-side software.

Examples include APIs for:

```text
Networking
HTTP
Sockets
Remote communication
Database connectivity
```

Modern Java backend development commonly uses Java together with frameworks such as Spring.

---

## 10. Dynamic

Java supports dynamic behavior at runtime through mechanisms such as:

```text
Class loading
Reflection
Runtime linking
Dynamic method dispatch
```

The JVM can load classes when they are needed rather than requiring every class to be permanently loaded at compile time.

---

# ⚙️ How Java Code Runs

Consider:

```java
class Main {

    public static void main(String[] args) {
        System.out.println("Hello Java");
    }
}
```

The basic process is:

```text
             Main.java
                 │
                 ▼
          Java Compiler
             javac
                 │
                 ▼
             Main.class
             (Bytecode)
                 │
                 ▼
                JVM
                 │
        ┌────────┴────────┐
        ▼                 ▼
   Interpreter           JIT
        │                 │
        └────────┬────────┘
                 ▼
          Native Machine Code
                 │
                 ▼
                CPU
```

### Important distinction

The:

```text
Java Compiler
```

and:

```text
JIT Compiler
```

are **not the same thing**.

The Java compiler primarily converts:

```text
Java source → Bytecode
```

The JIT compiler works **inside the JVM at runtime** and can compile frequently executed bytecode into native machine code.

JIT will be explored deeply in:

```text
03-JVM-and-Memory/
```

---

# 🧩 Java Program vs JVM

A common beginner misconception is:

> "Java runs directly on the operating system."

More accurately:

```text
Java Program
     ↓
Bytecode
     ↓
JVM
     ↓
Operating System
     ↓
Hardware
```

The JVM acts as the runtime environment that executes Java bytecode.

---

# ☕ Why Is Java Called "Java"?

Java was originally developed at **Sun Microsystems** by a team led by **James Gosling**.

The project initially had the name:

```text
Oak
```

The name was later changed to:

```text
Java
```

The language was publicly released in **1995**.

---

# 📈 Where Is Java Used?

Java is used across many areas of software development.

### Backend Development

```text
Spring
Spring Boot
Jakarta EE
```

### Enterprise Applications

```text
Banking
Insurance
Large-scale business systems
Government systems
```

### Android

Java was historically one of the primary languages used for Android development, although modern Android development heavily uses Kotlin as well.

### Big Data

Java has been used extensively in technologies such as:

```text
Apache Hadoop
Apache Kafka
Apache Spark
```

### Desktop Applications

Java provides GUI technologies such as:

```text
JavaFX
Swing
```

### Cloud & Distributed Systems

Java is widely used for:

```text
Microservices
REST APIs
Cloud applications
Distributed systems
```

---

# 🆚 Java vs JavaScript

A very common beginner confusion:

```text
Java ≠ JavaScript
```

They are different programming languages.

| Java                         | JavaScript                                                |
| ---------------------------- | --------------------------------------------------------- |
| General-purpose language     | Primarily used for web scripting and broader applications |
| Statically typed             | Dynamically typed                                         |
| Runs on JVM                  | Commonly runs in browsers and JavaScript runtimes         |
| `.java` source files         | `.js` source files                                        |
| Common in backend/enterprise | Common in frontend and full-stack web development         |

The names are historically related through branding, but the languages are fundamentally different.

---

# ⚠️ Is Java 100% Object-Oriented?

Java is an **object-oriented programming language**, but it is generally not considered a **purely object-oriented language**.

Why?

Because Java has primitive data types:

```java
int
byte
short
long
float
double
char
boolean
```

For example:

```java
int age = 22;
```

`int` is a primitive type, not an object.

Java also provides wrapper classes:

```text
int      → Integer
char     → Character
boolean  → Boolean
double   → Double
```

This topic will be covered deeply later.

---

# 🔐 Why Doesn't Java Provide Raw Pointers?

Java does not expose raw pointers and pointer arithmetic like C/C++.

Instead, Java uses **references** to work with objects.

Example:

```java
Employee e = new Employee();
```

Here:

```text
e
↓
reference
↓
Employee object
```

Java hides direct memory-address manipulation from normal application code.

This contributes to Java's safety and abstraction.

---

# 🧠 Java's Core Mental Model

If you're learning Java seriously, keep this model in your head:

```text
                JAVA
                  │
       ┌──────────┼──────────┐
       ▼          ▼          ▼
    Language     JVM       Library
       │          │          │
       ▼          ▼          ▼
     Syntax     Runtime    Collections
     OOP        Memory     Strings
     Types      JIT        I/O
     Methods    GC         Networking
```

Java is therefore **more than just a programming language**.

It consists of:

```text
Language
+
Compiler
+
JVM
+
Standard Libraries
+
Runtime Environment
```

---

# 🎯 Interview Questions From This Topic

Before moving forward, you should be able to answer these:

### 🟢 Basic

1. What is Java?
2. Who developed Java?
3. When was Java first released?
4. What is Java used for?
5. Is Java a high-level language?
6. Is Java object-oriented?

### 🟡 Important

7. Why is Java platform independent?
8. What is bytecode?
9. What is the role of JVM?
10. What is the difference between Java source code and bytecode?
11. Why is Java considered robust?
12. Why doesn't Java support pointers?
13. Why is Java not considered purely object-oriented?

### 🔴 Interview Follow-Ups

14. Is Java compiled or interpreted?
15. What is the difference between `javac` and JIT?
16. Is JVM platform independent?
17. Is bytecode platform independent?
18. Why is JVM platform dependent?
19. What happens when you execute a Java program?
20. How does Java achieve portability?

---

# 🧪 Quick Example

```java
public class Main {

    public static void main(String[] args) {

        int x = 10;

        System.out.println(x);
    }
}
```

Think about its journey:

```text
Main.java
   │
   │ javac
   ▼
Main.class
   │
   │ Bytecode
   ▼
JVM
   │
   ├── Interpreter
   │
   └── JIT Compiler
          │
          ▼
     Native Code
          │
          ▼
         CPU
```

---

# 🧠 30-Second Interview Answer

> **Java is a high-level, class-based, object-oriented programming language developed by Sun Microsystems and released in 1995. Java source code is compiled into platform-independent bytecode, which is executed by a platform-specific JVM. This architecture provides Java with portability, while features such as automatic memory management, exception handling, strong type checking, and runtime checks contribute to its robustness and safety.**

---

# ⚡ Quick Revision

```text
Java
│
├── High-Level
├── General-Purpose
├── Class-Based
├── Object-Oriented
├── Platform Independent
├── Robust
├── Secure
├── Multithreaded
├── Portable
└── Automatic Memory Management
```

### Remember the execution pipeline:

```text
.java
  ↓
javac
  ↓
.class
  ↓
Bytecode
  ↓
JVM
  ↓
Interpreter / JIT
  ↓
Native Machine Code
  ↓
CPU
```

---

## 🔗 Next Topics

```text
01-Java-Introduction.md
        │
        ├──→ 02-JDK-JRE-JVM.md
        │
        ├──→ 03-Why-Java-Platform-Independent.md
        │
        └──→ 04-Compilation-and-Execution.md
```

> **Next:** Understand the difference between **JDK, JRE, and JVM** — one of the most frequently asked Java interview questions.
