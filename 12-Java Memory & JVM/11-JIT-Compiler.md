# 11 — JIT Compiler

## 📚 Table of Contents

1. [Introduction](#1-introduction)
2. [What is JIT Compiler?](#2-what-is-jit-compiler)
3. [Why Does Java Need JIT?](#3-why-does-java-need-jit)
4. [Where JIT Fits in JVM](#4-where-jit-fits-in-jvm)
5. [Java Compilation vs JIT Compilation](#5-java-compilation-vs-jit-compilation)
6. [How JIT Works](#6-how-jit-works)
7. [Interpreter and JIT](#7-interpreter-and-jit)
8. [Hot Code](#8-hot-code)
9. [Runtime Profiling](#9-runtime-profiling)
10. [HotSpot JVM](#10-hotspot-jvm)
11. [Tiered Compilation](#11-tiered-compilation)
12. [C1 Compiler](#12-c1-compiler)
13. [C2 Compiler](#13-c2-compiler)
14. [JIT Compilation Flow](#14-jit-compilation-flow)
15. [Method Compilation](#15-method-compilation)
16. [OSR — On-Stack Replacement](#16-osr--on-stack-replacement)
17. [Code Cache](#17-code-cache)
18. [JIT Optimizations](#18-jit-optimizations)
19. [Method Inlining](#19-method-inlining)
20. [Constant Folding](#20-constant-folding)
21. [Constant Propagation](#21-constant-propagation)
22. [Dead Code Elimination](#22-dead-code-elimination)
23. [Loop Optimization](#23-loop-optimization)
24. [Loop Unrolling](#24-loop-unrolling)
25. [Escape Analysis](#25-escape-analysis)
26. [Scalar Replacement](#26-scalar-replacement)
27. [Lock Elimination](#27-lock-elimination)
28. [Devirtualization](#28-devirtualization)
29. [Range Check Elimination](#29-range-check-elimination)
30. [Speculative Optimization](#30-speculative-optimization)
31. [Deoptimization](#31-deoptimization)
32. [JIT and CPU](#32-jit-and-cpu)
33. [JIT and Memory](#33-jit-and-memory)
34. [JIT and Garbage Collection](#34-jit-and-garbage-collection)
35. [Advantages](#35-advantages)
36. [Disadvantages](#36-disadvantages)
37. [Common Misconceptions](#37-common-misconceptions)
38. [Interview Traps](#38-interview-traps)
39. [Top 20 Interview Questions](#39-top-20-interview-questions)
40. [30-Second Interview Answer](#40-30-second-interview-answer)
41. [Cheat Sheet](#41-cheat-sheet)

---

# 1. Introduction

JIT stands for:

> **Just-In-Time Compiler**

The JIT compiler is a part of the JVM's execution system that compiles frequently executed JVM bytecode into native machine code at runtime.

The basic idea is:

```text
Java Source
     ↓
   javac
     ↓
   Bytecode
     ↓
    JVM
     ↓
 Interpreter
     ↓
 Runtime Profiling
     ↓
  Hot Code
     ↓
 JIT Compiler
     ↓
Native Machine Code
     ↓
    CPU
```

JIT is one of the major reasons modern Java applications can achieve high runtime performance while retaining Java's bytecode-based execution model.

---

# 2. What is JIT Compiler?

### Definition

> A JIT compiler is a runtime compiler that converts frequently executed JVM bytecode into native machine code and optimizes that code using information collected during program execution.

JIT compilation happens **during application execution**.

That is why it is called:

```text
Just-In-Time
```

The JVM waits until runtime to determine which code is worth compiling and optimizing.

---

# 3. Why Does Java Need JIT?

Without JIT, the JVM could rely heavily on interpretation:

```text
Bytecode
   ↓
Interpreter
   ↓
Instruction
   ↓
Instruction
   ↓
Instruction
   ↓
Instruction
```

For code that executes millions of times, repeatedly interpreting the same bytecode can be inefficient.

JIT changes the situation:

```text
Bytecode
   ↓
Interpreter
   ↓
Detect frequently executed code
   ↓
JIT Compile
   ↓
Native Machine Code
   ↓
Execute repeatedly
```

Instead of repeatedly interpreting hot code, the JVM can execute the compiled native version.

---

# 4. Where JIT Fits in JVM

A simplified JVM architecture:

```text
                       JVM
                        │
          ┌─────────────┼─────────────┐
          │             │             │
          ▼             ▼             ▼
    Class Loader   Runtime Areas   Execution Engine
                                      │
                           ┌──────────┴──────────┐
                           │                     │
                           ▼                     ▼
                      Interpreter             JIT
                                                │
                                      ┌─────────┴─────────┐
                                      │                   │
                                      ▼                   ▼
                                     C1                  C2
                                      │                   │
                                      └─────────┬─────────┘
                                                ▼
                                       Native Machine Code
                                                │
                                                ▼
                                               CPU
```

### Important

JIT is part of the JVM's execution machinery.

It is not part of:

```text
javac
```

and it does not normally compile `.java` source files directly.

---

# 5. Java Compilation vs JIT Compilation

There are two important compilation stages to understand.

## Stage 1 — Java Compilation

The Java compiler:

```text
javac
```

takes:

```text
.java
```

and produces:

```text
.class
```

Conceptually:

```text
Java Source
    ↓
   javac
    ↓
JVM Bytecode
```

---

## Stage 2 — JIT Compilation

The JVM takes bytecode and may compile hot sections into native machine code:

```text
Bytecode
   ↓
JVM
   ↓
JIT
   ↓
Native Machine Code
```

### Comparison

| Feature | `javac` | JIT |
|---|---|---|
| Runs | Before application execution | During execution |
| Input | Java source | JVM bytecode |
| Output | Bytecode | Native machine code |
| Uses runtime profiling | No | Yes |
| Main goal | Compile Java source | Optimize runtime execution |
| Runs inside JVM | No | Yes |

### Memory Trick

```text
javac
Java → Bytecode

JIT
Bytecode → Native Code
```

---

# 6. How JIT Works

A simplified JIT workflow:

```text
1. Java program starts
        ↓
2. Classes are loaded
        ↓
3. Bytecode begins execution
        ↓
4. Interpreter executes bytecode
        ↓
5. JVM collects profiling information
        ↓
6. Frequently executed code is identified
        ↓
7. JIT compiles hot code
        ↓
8. JIT optimizes generated code
        ↓
9. Native code is stored
        ↓
10. CPU executes native code
```

The important idea is:

```text
Execution
   ↓
Profiling
   ↓
Optimization
```

This makes JIT different from traditional static compilation.

---

# 7. Interpreter and JIT

Modern JVM execution can involve both:

```text
Interpreter
+
JIT Compiler
```

The interpreter is useful for code that is:

```text
Rarely executed
Short-lived
Not worth compiling
```

The JIT is useful for:

```text
Frequently executed
Performance-critical
Hot
```

Conceptually:

```text
                    Bytecode
                       │
                       ▼
                  Interpreter
                       │
                 Runtime Profile
                       │
                       ▼
                   Is it hot?
                  /           \
                No             Yes
                │               │
                ▼               ▼
            Continue           JIT
            interpreting         │
                                 ▼
                         Native Machine Code
```

---

# 8. Hot Code

### Definition

> Hot code is code that is executed frequently enough for the JVM to consider optimizing or compiling it.

Example:

```java
public static int square(int x) {
    return x * x;
}

public static void main(String[] args) {

    for (int i = 0; i < 1_000_000; i++) {
        square(i);
    }
}
```

The `square()` method may become hot because it is repeatedly invoked.

The loop itself can also become hot.

Conceptually:

```text
square()
   ↓
Called repeatedly
   ↓
Hot
   ↓
JIT compilation
```

### Important

"Hot" does not simply mean:

> The method is large.

It generally refers to runtime execution frequency and profiling information.

---

# 9. Runtime Profiling

One of the biggest advantages of JIT compilation is that the JVM has runtime information available.

The JVM can observe things such as:

```text
Which methods are called frequently
Which branches are commonly taken
Which types appear frequently
Which loops execute repeatedly
Which code paths are performance-critical
```

This information can guide optimization.

Conceptually:

```text
Application Running
       ↓
Runtime Profiling
       ↓
Execution Information
       ↓
JIT Compiler
       ↓
Optimization Decisions
```

### Static Compiler vs JIT

A static compiler primarily has information available before execution.

A JIT compiler can use information collected while the application is running.

---

# 10. HotSpot JVM

HotSpot is a JVM implementation commonly used in OpenJDK distributions.

It includes runtime execution technologies such as:

```text
Interpreter
JIT Compilation
Runtime Profiling
Optimizations
Deoptimization
```

The name HotSpot relates to identifying frequently executed "hot spots" in the application.

Conceptually:

```text
Application
     ↓
Runtime Profile
     ↓
Hot Methods / Hot Loops
     ↓
JIT
     ↓
Optimization
```

### Important

Do not say:

> HotSpot = JIT

A better understanding is:

```text
HotSpot
   ↓
JVM implementation

JIT
   ↓
Runtime compilation technology
```

---

# 11. Tiered Compilation

Modern HotSpot JVMs commonly use **tiered compilation**.

Instead of immediately using the most expensive optimization strategy, the JVM can gradually move code through different execution levels.

A simplified model:

```text
Bytecode
   ↓
Interpreter
   ↓
C1
   ↓
C2
```

The goal is to balance:

```text
Startup Speed
+
Compilation Cost
+
Peak Performance
```

A method may initially execute through interpretation, then receive quicker compilation, and eventually receive more aggressive optimization if it remains sufficiently hot.

---

# 12. C1 Compiler

C1 is traditionally associated with the **client compiler** in HotSpot.

Its main characteristic is:

```text
Fast compilation
+
Lower compilation overhead
```

It can produce reasonably optimized native code without spending as much compilation time as more aggressive optimization.

Conceptually:

```text
Hot Code
   ↓
C1
   ↓
Quick Native Compilation
```

### Why C1?

If a method is moderately hot, spending a huge amount of time optimizing it may not be worthwhile.

C1 provides a faster route to compiled execution.

---

# 13. C2 Compiler

C2 is traditionally associated with the **server compiler**.

It focuses on more aggressive optimization of sufficiently hot code.

Conceptually:

```text
Very Hot Code
     ↓
    C2
     ↓
Aggressive Optimization
     ↓
Highly Optimized Native Code
```

C2 can use extensive profiling information to perform optimizations such as:

```text
Inlining
Escape Analysis
Loop Optimizations
Devirtualization
Dead Code Elimination
```

### Simple Comparison

```text
C1
→ Compile faster

C2
→ Optimize more aggressively
```

---

# 14. JIT Compilation Flow

A simplified flow:

```text
                 Bytecode
                    │
                    ▼
              Interpreter
                    │
                    ▼
             Runtime Profiling
                    │
                    ▼
               Hot Code?
              /          \
            No            Yes
            │              │
            ▼              ▼
       Interpretation     C1
                           │
                           ▼
                      More Profiling
                           │
                           ▼
                          C2
                           │
                           ▼
                   Optimized Native Code
                           │
                           ▼
                         CPU
```

The exact compilation strategy depends on the JVM version and configuration.

---

# 15. Method Compilation

The JVM does not need to compile every method immediately.

Consider:

```java
public static void methodA() {
    System.out.println("A");
}

public static void methodB() {
    for (int i = 0; i < 1_000_000; i++) {
        calculate();
    }
}
```

If:

```text
methodA()
```

runs once, compiling it may provide little benefit.

If:

```text
methodB()
```

runs repeatedly, compiling it may provide significant benefit.

Therefore:

```text
Rare Method
    ↓
May remain interpreted

Hot Method
    ↓
JIT Compilation
```

---

# 16. OSR — On-Stack Replacement

OSR stands for:

> **On-Stack Replacement**

It allows a currently executing method or loop to transition from interpreted execution to compiled execution.

Consider:

```java
while (true) {
    process();
}
```

Suppose this loop becomes hot while it is already running.

The JVM does not necessarily need to wait for the method to return.

It can compile the hot loop and transfer execution into the compiled version.

Conceptually:

```text
Running Method
      ↓
Hot Loop Detected
      ↓
JIT Compiles Loop
      ↓
OSR
      ↓
Continue execution
in compiled code
```

### Why OSR Matters

Without OSR, a long-running loop could remain interpreted for a long time before the method invocation finishes.

---

# 17. Code Cache

The native machine code generated by JIT needs somewhere to be stored.

HotSpot uses a memory region called the:

> **Code Cache**

Conceptually:

```text
JIT Compiler
      ↓
Native Machine Code
      ↓
Code Cache
      ↓
CPU
```

### Important

Code Cache is not the Java heap.

```text
Java Heap
   ≠
Code Cache
```

The Code Cache is part of the JVM's native/runtime memory infrastructure.

---

# 18. JIT Optimizations

The JIT compiler can perform many optimizations.

Important examples:

```text
1. Method Inlining
2. Constant Folding
3. Constant Propagation
4. Dead Code Elimination
5. Loop Optimization
6. Loop Unrolling
7. Escape Analysis
8. Scalar Replacement
9. Lock Elimination
10. Devirtualization
11. Range Check Elimination
12. Speculative Optimization
```

The purpose is generally:

```text
Less Work
+
Fewer Instructions
+
Better CPU Utilization
=
Better Runtime Performance
```

---

# 19. Method Inlining

Inlining is one of the most important JIT optimizations.

Consider:

```java
static int square(int x) {
    return x * x;
}

int result = square(10);
```

Conceptually, the JIT may transform the optimized representation from:

```text
Call square()
      ↓
Return result
```

toward:

```text
result = 10 * 10
```

### Why?

Inlining can:

```text
Reduce method-call overhead
Expose more code to optimizer
Enable additional optimizations
```

### Important

The Java source file is not literally rewritten.

Inlining happens in the compiler's internal representation/generated code.

---

# 20. Constant Folding

Constant folding means evaluating an expression at compile/optimization time when its result is known.

Example:

```java
int x = 10 * 20;
```

Conceptually:

```text
10 * 20
   ↓
200
```

Instead of generating instructions to perform the multiplication at runtime, the compiler may use the already-known result.

### Example

```java
int value = 100 + 50;
```

Can conceptually become:

```java
int value = 150;
```

---

# 21. Constant Propagation

Constant propagation uses known constant values to simplify later operations.

Example:

```java
int x = 10;
int y = x + 20;
```

The compiler may determine:

```text
x = 10
y = 30
```

Conceptually:

```text
x = 10
   ↓
y = x + 20
   ↓
y = 30
```

This can enable further optimizations.

---

# 22. Dead Code Elimination

Dead code is code whose execution does not contribute to observable program behavior under the relevant assumptions.

Example:

```java
int x = 10;

if (false) {
    System.out.println("Hello");
}
```

The branch can potentially be eliminated.

Conceptually:

```text
Original
    ↓
Analyze
    ↓
Unnecessary code
    ↓
Remove
```

### Important

The optimizer must preserve observable behavior.

It cannot remove arbitrary code simply because the result appears unused if the code has observable side effects.

---

# 23. Loop Optimization

Loops are extremely important for JIT optimization because they can execute millions or billions of times.

Example:

```java
for (int i = 0; i < 1_000_000; i++) {
    sum += values[i];
}
```

The JIT may apply transformations such as:

```text
Loop unrolling
Loop invariant code motion
Range-check elimination
Induction-variable optimization
```

The exact optimization depends on the code and runtime profile.

---

# 24. Loop Unrolling

Loop unrolling reduces loop-control overhead by processing multiple iterations together.

Original:

```java
for (int i = 0; i < 4; i++) {
    sum += values[i];
}
```

Conceptually, an unrolled representation could look like:

```text
sum += values[0];
sum += values[1];
sum += values[2];
sum += values[3];
```

Instead of repeatedly performing:

```text
increment
compare
branch
```

for every iteration.

### Important

The JIT decides whether unrolling is beneficial.

More unrolling is not always better because it can increase generated code size.

---

# 25. Escape Analysis

Escape analysis determines whether an object escapes a particular scope or thread.

Example:

```java
static int calculate() {

    Point point = new Point(10, 20);

    return point.x + point.y;
}
```

If the JVM determines that `point` does not escape, it may perform optimizations.

Potential consequences include:

```text
Allocation elimination
Scalar replacement
Lock elimination
```

### Important

Escape analysis does **not** simply mean:

> "Put every non-escaping object on the stack."

The JVM may completely eliminate the allocation or represent the object's fields separately.

---

# 26. Scalar Replacement

Suppose:

```java
class Point {

    int x;
    int y;
}
```

And:

```java
static int calculate() {

    Point p = new Point();

    p.x = 10;
    p.y = 20;

    return p.x + p.y;
}
```

If the object does not escape, the JIT may represent the fields separately rather than maintaining the full object allocation.

Conceptually:

```text
Normal:

Point Object
+---------+
| x = 10  |
| y = 20  |
+---------+
```

Potential optimized representation:

```text
x → 10
y → 20
```

The actual object allocation may disappear from the generated machine code.

---

# 27. Lock Elimination

Suppose:

```java
Object lock = new Object();

synchronized (lock) {
    calculate();
}
```

If the JVM proves that `lock` cannot be observed or accessed by another thread, the synchronization may be unnecessary.

The JIT can potentially eliminate it.

Conceptually:

```text
Object
  ↓
Escape Analysis
  ↓
Cannot escape
  ↓
No possible contention
  ↓
Remove unnecessary lock
```

### Important

This is only possible when the JVM can safely prove that synchronization can be removed without changing observable behavior.

---

# 28. Devirtualization

Java supports polymorphism:

```java
Animal animal = new Dog();

animal.sound();
```

At the source-code level, the exact implementation may depend on the runtime type.

If runtime profiling shows that a particular implementation is consistently used, the JIT may optimize the call.

Conceptually:

```text
Virtual Call
     ↓
Runtime Profile
     ↓
Mostly Dog
     ↓
Optimize toward Dog implementation
```

This can enable further optimizations such as inlining.

### Important

The JVM must preserve correctness if other valid runtime types appear.

---

# 29. Range Check Elimination

Java array accesses normally require bounds checking.

Example:

```java
for (int i = 0; i < array.length; i++) {
    sum += array[i];
}
```

Conceptually, every access could require:

```text
0 <= i < array.length
```

The JIT may prove that the loop already guarantees valid indexes.

It can then eliminate redundant bounds checks in optimized machine code.

Conceptually:

```text
Array Access
     ↓
Bounds Check
     ↓
JIT proves loop is safe
     ↓
Remove redundant checks
```

This can improve performance in tight loops.

---

# 30. Speculative Optimization

JIT compilers can optimize based on runtime observations.

Suppose profiling shows:

```text
99% of calls → Dog
1% of calls  → Cat
```

The JIT may optimize for the common case.

Conceptually:

```text
Runtime Profile
      ↓
Common Case Identified
      ↓
Optimize Common Case
      ↓
Rare Case → fallback/deoptimization path
```

This is called **speculative optimization**.

### Important

Speculation must remain safe.

If the assumption becomes invalid, the JVM can deoptimize and continue execution correctly.

---

# 31. Deoptimization

JIT optimizations may depend on assumptions.

For example:

```text
Profile:
Only Dog objects observed
```

The JIT optimizes based on this.

Later:

```text
Cat object appears
```

The previous optimization may no longer be valid.

The JVM can perform:

> **Deoptimization**

Conceptually:

```text
Optimized Native Code
        ↓
Assumption becomes invalid
        ↓
Deoptimization
        ↓
Return to safer execution
        ↓
Re-profile / recompile if necessary
```

### Important

Deoptimization is not necessarily an error.

It is a normal mechanism used to maintain correctness while allowing aggressive runtime optimization.

---

# 32. JIT and CPU

The JIT ultimately generates machine code for the target platform.

Conceptually:

```text
JVM Bytecode
     ↓
JIT
     ↓
Native Instructions
     ↓
CPU
```

For example:

```text
Bytecode
   ↓
x86-64 machine code
```

or:

```text
Bytecode
   ↓
ARM64 machine code
```

This is one reason the same Java bytecode can run on different platforms while still achieving platform-specific native execution.

---

# 33. JIT and Memory

JIT compilation interacts with several memory areas.

### Java Heap

Used for Java objects.

```text
new Employee()
      ↓
     Heap
```

### JVM Stack

Contains stack frames for method execution.

```text
Thread
  ↓
JVM Stack
  ↓
Stack Frames
```

### Metaspace

Stores class metadata in HotSpot.

```text
Class Metadata
      ↓
  Metaspace
```

### Code Cache

Stores JIT-generated compiled code.

```text
JIT
 ↓
Native Code
 ↓
Code Cache
```

### Important

Do not say:

> JIT stores machine code in the Java heap.

The generated compiled code is associated with the JVM's Code Cache rather than normal Java heap storage.

---

# 34. JIT and Garbage Collection

JIT compilation and Garbage Collection have different responsibilities.

### JIT

```text
Compile
Optimize
Execute hot code
```

### GC

```text
Find unreachable objects
Reclaim heap memory
```

However, they interact.

For example:

```text
JIT Optimization
      ↓
Escape Analysis
      ↓
Allocation may be eliminated
      ↓
Fewer heap allocations
      ↓
Potentially less GC pressure
```

This does not mean:

> JIT performs garbage collection.

It does not.

---

# 35. Advantages

## 35.1 High Runtime Performance

Hot code can run as native machine code.

## 35.2 Runtime Profiling

The JIT can use actual application behavior.

## 35.3 Dynamic Optimization

The JVM can optimize code based on what actually happens.

## 35.4 Platform-Specific Code Generation

The JIT can generate machine code appropriate for the current CPU architecture.

## 35.5 Adaptive Optimization

The JVM can change optimization decisions as runtime behavior changes.

---

# 36. Disadvantages

## 36.1 Compilation Overhead

JIT compilation consumes CPU time.

```text
Application
    +
JIT Compilation
```

Both require resources.

## 36.2 Warm-Up Time

An application may initially run differently from its steady-state performance because optimization happens during execution.

## 36.3 Memory Usage

Compiled code and compiler data require memory.

## 36.4 Complexity

The JVM has to manage:

```text
Profiling
Compilation
Optimization
Code Cache
Deoptimization
```

## 36.5 Unpredictable Optimization Decisions

The exact JIT behavior depends on:

```text
JVM implementation
JVM version
Hardware
Runtime profile
Flags/configuration
Application behavior
```

---

# 37. Common Misconceptions

## Misconception 1

> JIT compiles Java source code.

### Correct

JIT compiles JVM bytecode into native machine code.

```text
javac:
.java → bytecode

JIT:
bytecode → native code
```

---

## Misconception 2

> JIT compiles everything.

### Correct

JIT compilation is selective.

Hot code receives more optimization attention.

---

## Misconception 3

> Java is either interpreted or compiled.

### Correct

Modern JVM execution commonly uses both interpretation and JIT compilation.

---

## Misconception 4

> JIT happens before the program starts.

### Correct

JIT compilation happens during JVM execution.

---

## Misconception 5

> JIT always makes the program faster immediately.

### Correct

JIT compilation has overhead, so there can be a warm-up period.

---

## Misconception 6

> C1 and C2 are two completely separate JVMs.

### Correct

They are traditionally associated with different JIT compilation tiers within HotSpot.

---

## Misconception 7

> JIT permanently changes the `.class` file.

### Correct

JIT compilation creates runtime native code. It does not rewrite the original `.class` file.

---

## Misconception 8

> JIT optimization can change program behavior if it thinks the change is faster.

### Correct

Optimizations must preserve the Java program's observable semantics.

---

## Misconception 9

> Deoptimization means the JVM crashed.

### Correct

Deoptimization is a normal runtime mechanism used when an optimization assumption becomes invalid.

---

## Misconception 10

> JIT machine code is stored on the Java heap.

### Correct

JIT-generated compiled code is stored in the JVM's Code Cache.

---

# 38. Interview Traps

## Trap 1

### Question

What is JIT?

### Answer

JIT is a runtime compiler that converts frequently executed JVM bytecode into native machine code and optimizes it.

---

## Trap 2

### Question

When does JIT compilation happen?

### Answer

During JVM runtime, after the JVM has observed application execution and identified code worth compiling.

---

## Trap 3

### Question

Why not compile all bytecode immediately?

### Answer

Compilation costs CPU time and memory. Runtime profiling allows the JVM to focus optimization effort on code that actually matters.

---

## Trap 4

### Question

What is hot code?

### Answer

Frequently executed or performance-critical code identified through runtime profiling.

---

## Trap 5

### Question

What is the difference between `javac` and JIT?

### Answer

```text
javac:
Java source → JVM bytecode

JIT:
JVM bytecode → Native machine code
```

---

## Trap 6

### Question

What is C1?

### Answer

C1 is a HotSpot compilation tier focused on relatively fast compilation with lower compilation overhead.

---

## Trap 7

### Question

What is C2?

### Answer

C2 is a HotSpot compilation tier designed for more aggressive optimization of sufficiently hot code.

---

## Trap 8

### Question

What is OSR?

### Answer

On-Stack Replacement allows currently running code, particularly hot loops, to transition from interpreted execution to compiled execution.

---

## Trap 9

### Question

What is Code Cache?

### Answer

A JVM-managed memory area used to store JIT-generated native machine code.

---

## Trap 10

### Question

What happens when a JIT assumption becomes invalid?

### Answer

The JVM can deoptimize the compiled code and return to a safer execution path.

---

# 39. Top 20 Interview Questions

## Q1. What is JIT Compiler?

### Answer

JIT stands for Just-In-Time Compiler. It compiles frequently executed JVM bytecode into native machine code during runtime.

---

## Q2. Why is JIT required?

### Answer

JIT improves performance by compiling hot code into optimized native machine code instead of repeatedly interpreting the same bytecode.

---

## Q3. Is JIT part of JVM?

### Answer

Yes. JIT compilation is part of the JVM's runtime execution machinery.

---

## Q4. What is the difference between `javac` and JIT?

### Answer

```text
javac:
.java → .class bytecode

JIT:
bytecode → native machine code
```

`javac` works before runtime, while JIT works during runtime.

---

## Q5. What is hot code?

### Answer

Hot code is frequently executed or otherwise identified through runtime profiling as important for optimization.

---

## Q6. Does JIT compile every method?

### Answer

No. The JVM selectively compiles code based on runtime behavior and compilation heuristics.

---

## Q7. What is runtime profiling?

### Answer

Runtime profiling is the collection of execution information such as method invocation frequency, branch behavior, and type information that can guide JIT optimizations.

---

## Q8. What is HotSpot?

### Answer

HotSpot is a JVM implementation that provides runtime profiling, interpretation, JIT compilation, optimization, and deoptimization mechanisms.

---

## Q9. What is tiered compilation?

### Answer

Tiered compilation uses multiple execution and compilation levels to balance startup performance, compilation overhead, and peak runtime performance.

---

## Q10. What are C1 and C2?

### Answer

C1 is traditionally the faster-compiling HotSpot tier, while C2 is designed for more aggressive optimization of sufficiently hot code.

---

## Q11. What is method inlining?

### Answer

Inlining replaces a method call with the method's body in optimized compiled code when beneficial, reducing call overhead and exposing more optimization opportunities.

---

## Q12. What is OSR?

### Answer

On-Stack Replacement allows currently executing code to switch from interpreted execution to compiled execution while the method or loop is still running.

---

## Q13. What is Code Cache?

### Answer

Code Cache is JVM-managed memory used to store compiled native machine code generated by JIT compilers.

---

## Q14. What is escape analysis?

### Answer

Escape analysis determines whether an object escapes a particular scope or thread and enables optimizations such as allocation elimination, scalar replacement, and lock elimination.

---

## Q15. What is deoptimization?

### Answer

Deoptimization is the process of abandoning optimized compiled code when assumptions used by that optimization become invalid.

---

## Q16. What is speculative optimization?

### Answer

Speculative optimization uses runtime observations to optimize common cases under assumptions that can later be checked and, if invalidated, deoptimized.

---

## Q17. Does JIT modify `.class` files?

### Answer

No. JIT-generated native code is created at runtime and does not normally rewrite the original class file.

---

## Q18. Does JIT always improve performance?

### Answer

JIT can significantly improve performance of hot code, but compilation itself has CPU and memory costs, so the benefits depend on application behavior.

---

## Q19. Where is JIT-generated code stored?

### Answer

It is stored in the JVM's Code Cache.

---

## Q20. Is Java interpreted or compiled?

### Answer

Java uses multiple stages:

```text
Java source
    ↓
javac
    ↓
Bytecode
    ↓
Interpreter / JIT
    ↓
Native execution
```

So modern Java execution involves both interpretation and runtime compilation.

---

# 40. 30-Second Interview Answer

> JIT stands for Just-In-Time Compiler. It is part of the JVM execution system and compiles frequently executed JVM bytecode into native machine code at runtime. The JVM initially uses interpretation and collects runtime profiling information. When code becomes hot, the JIT can compile and optimize it using techniques such as inlining, escape analysis, loop optimization, constant folding, and devirtualization. HotSpot commonly uses tiered compilation involving C1 and C2. The generated native code is stored in the Code Cache, and if optimization assumptions become invalid, the JVM can deoptimize and safely continue execution.

---

# 41. Cheat Sheet

```text
╔══════════════════════════════════════════════════════════╗
║                    JIT COMPILER                         ║
╠══════════════════════════════════════════════════════════╣
║ JIT             → Just-In-Time Compiler                 ║
║ Purpose         → Runtime bytecode → native code        ║
║ Runs            → During JVM execution                  ║
║ Input           → JVM bytecode                          ║
║ Output          → Native machine code                   ║
║ Hot Code        → Frequently executed code              ║
║ Profiling       → Runtime execution information         ║
║ C1              → Faster compilation tier               ║
║ C2              → Aggressive optimization tier          ║
║ OSR             → Switch running code to compiled code  ║
║ Code Cache      → Stores compiled native code           ║
║ Deoptimization  → Abandon invalid optimization          ║
╚══════════════════════════════════════════════════════════╝
```

## 🧠 Main Memory Trick

```text
javac
 ↓
Java → Bytecode

JVM
 ↓
Interpreter
 ↓
Profile
 ↓
Hot?
 ├── No  → Keep interpreting
 │
 └── Yes → JIT
             ↓
            C1
             ↓
        More Profiling
             ↓
            C2
             ↓
      Native Machine Code
             ↓
         Code Cache
             ↓
            CPU
```

## 🔥 Major JIT Optimizations

```text
JIT
 │
 ├── Inlining
 │
 ├── Constant Folding
 │
 ├── Constant Propagation
 │
 ├── Dead Code Elimination
 │
 ├── Loop Optimization
 │
 ├── Loop Unrolling
 │
 ├── Escape Analysis
 │
 ├── Scalar Replacement
 │
 ├── Lock Elimination
 │
 ├── Devirtualization
 │
 ├── Range Check Elimination
 │
 └── Speculative Optimization
```

## 🔥 JIT + Deoptimization

```text
Runtime Profile
      ↓
JIT makes assumption
      ↓
Optimized Native Code
      ↓
Program behavior changes
      ↓
Assumption invalid
      ↓
Deoptimization
      ↓
Safe execution continues
```

## 🔥 JIT + Memory

```text
Java Objects
    ↓
   Heap

Method Execution
    ↓
   Stack

Class Metadata
    ↓
 Metaspace

JIT Native Code
    ↓
 Code Cache
```

## ⭐ Final Interview Rule

```text
javac
→ Java source → Bytecode

Interpreter
→ Initial bytecode execution

Runtime Profiling
→ Find hot code

JIT
→ Hot bytecode → Native machine code

C1
→ Faster compilation

C2
→ More aggressive optimization

OSR
→ Compile already-running hot code

Code Cache
→ Stores JIT-generated native code

Inlining
→ Reduce method-call overhead

Escape Analysis
→ Analyze object escape

Deoptimization
→ Recover when optimization assumptions fail
```

## 🚀 One-Line Revision

```text
Java Source → javac → Bytecode → Interpreter → Profiling → Hot Code → JIT → Optimized Native Code → CPU
```