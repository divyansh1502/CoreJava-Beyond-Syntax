# 10 — JVM Execution Engine

## 📚 Table of Contents

1. [Introduction](#1-introduction)
2. [What is the Execution Engine?](#2-what-is-the-execution-engine)
3. [Where Execution Engine Fits in JVM](#3-where-execution-engine-fits-in-jvm)
4. [Bytecode Execution](#4-bytecode-execution)
5. [Interpreter](#5-interpreter)
6. [How the Interpreter Works](#6-how-the-interpreter-works)
7. [Advantages of Interpretation](#7-advantages-of-interpretation)
8. [Disadvantages of Interpretation](#8-disadvantages-of-interpretation)
9. [JIT Compiler](#9-jit-compiler)
10. [Why JIT is Needed](#10-why-jit-is-needed)
11. [JIT Compilation Working](#11-jit-compilation-working)
12. [Hot Code](#12-hot-code)
13. [HotSpot](#13-hotspot)
14. [Method Compilation](#14-method-compilation)
15. [On-Stack Replacement](#15-on-stack-replacement)
16. [Code Cache](#16-code-cache)
17. [Tiered Compilation](#17-tiered-compilation)
18. [C1 and C2 Compilers](#18-c1-and-c2-compilers)
19. [JIT Optimizations](#19-jit-optimizations)
20. [Inlining](#20-inlining)
21. [Dead Code Elimination](#21-dead-code-elimination)
22. [Loop Optimizations](#22-loop-optimizations)
23. [Escape Analysis](#23-escape-analysis)
24. [Lock Elimination](#24-lock-elimination)
25. [Scalar Replacement](#25-scalar-replacement)
26. [Deoptimization](#26-deoptimization)
27. [Interpreter vs JIT](#27-interpreter-vs-jit)
28. [JVM Execution Flow](#28-jvm-execution-flow)
29. [Execution Engine and Memory](#29-execution-engine-and-memory)
30. [Execution Engine and Garbage Collection](#30-execution-engine-and-garbage-collection)
31. [Common Misconceptions](#31-common-misconceptions)
32. [Interview Traps](#32-interview-traps)
33. [Top 15 Interview Questions](#33-top-15-interview-questions)
34. [30-Second Interview Answer](#34-30-second-interview-answer)
35. [Cheat Sheet](#35-cheat-sheet)

---

# 1. Introduction

The **JVM Execution Engine** is the component of the JVM responsible for executing Java bytecode.

When Java source code is compiled:

```text
.java
  ↓
javac
  ↓
.class
  ↓
Bytecode
```

The JVM loads that bytecode and the Execution Engine executes it.

The simplified flow is:

```text
Java Source Code
       ↓
     javac
       ↓
   Bytecode
       ↓
+------------------+
|       JVM        |
|                  |
| Class Loader     |
| Runtime Memory   |
| Execution Engine |
+------------------+
       ↓
   Native Machine
      Code
```

The Execution Engine mainly works through:

```text
Interpreter
     +
JIT Compiler
     +
Native execution
```

---

# 2. What is the Execution Engine?

### Definition

> The JVM Execution Engine executes Java bytecode by interpreting it and, where beneficial, compiling frequently executed code into native machine code using JIT compilation.

The Execution Engine does not normally execute `.java` source code directly.

It works with bytecode loaded into the JVM.

### Example

Java source:

```java
int sum = a + b;
```

After compilation, this becomes JVM bytecode instructions.

The Execution Engine executes those instructions.

Conceptually:

```text
Java Code
   ↓
Bytecode
   ↓
Execution Engine
   ↓
CPU
```

---

# 3. Where Execution Engine Fits in JVM

A simplified JVM architecture:

```text
                    JVM
                     |
       +-------------+-------------+
       |             |             |
       v             v             v
 Class Loader   Runtime Data   Execution Engine
                   Areas
                                     |
                            +--------+--------+
                            |                 |
                            v                 v
                       Interpreter          JIT
                            |                 |
                            +--------+--------+
                                     |
                                     v
                              Native Machine Code
                                     |
                                     v
                                    CPU
```

### Main JVM Components

```text
1. Class Loader
2. Runtime Data Areas
3. Execution Engine
4. Native Method Interface
5. Native Libraries
```

The Execution Engine is responsible for actually executing the loaded bytecode.

---

# 4. Bytecode Execution

Suppose we have:

```java
public class Demo {

    public static int add(int a, int b) {
        return a + b;
    }
}
```

The Java compiler converts this source code into bytecode.

Conceptually:

```text
add()
   ↓
bytecode instructions
   ↓
Execution Engine
```

The JVM does not need to generate a separate `.exe` file for every operating system.

Instead:

```text
Same .class bytecode
       ↓
Windows JVM → executes it
Linux JVM   → executes it
macOS JVM   → executes it
```

This is a major part of Java's platform-independent execution model.

---

# 5. Interpreter

The **Interpreter** executes bytecode instruction by instruction.

Conceptually:

```text
Bytecode:

Instruction 1
Instruction 2
Instruction 3
Instruction 4
     ↓
Interpreter
     ↓
Native execution
```

For example:

```text
Bytecode
   ↓
Read instruction
   ↓
Interpret instruction
   ↓
Execute
   ↓
Read next instruction
   ↓
Repeat
```

### Important

The interpreter does not mean:

> Java source code is interpreted line by line.

It interprets **bytecode instructions**.

---

# 6. How the Interpreter Works

Suppose bytecode contains conceptually:

```text
load a
load b
add
return
```

The interpreter processes these instructions.

```text
load a
  ↓
load b
  ↓
add
  ↓
return
```

Each instruction is processed by the JVM execution machinery.

### Simplified Flow

```text
Bytecode
   ↓
Fetch instruction
   ↓
Decode instruction
   ↓
Execute instruction
   ↓
Fetch next instruction
```

This is similar to the basic fetch-decode-execute idea used by processors, although the JVM interpreter is software executing JVM bytecode rather than the CPU directly executing Java bytecode as native instructions.

---

# 7. Advantages of Interpretation

## 7.1 Fast Startup

The JVM can start executing methods without first compiling every method into native code.

## 7.2 No Need to Compile Everything

Only code that is actually executed needs to be interpreted.

## 7.3 Useful for Infrequently Executed Code

Compiling code that runs only once or a few times may not be worth the compilation overhead.

---

# 8. Disadvantages of Interpretation

The interpreter can be slower than optimized native code for frequently executed code.

Consider:

```java
for (int i = 0; i < 1_000_000_000; i++) {
    calculate();
}
```

If the JVM repeatedly interprets the same code, it may spend significant time processing the same bytecode instructions.

This is where JIT compilation becomes important.

---

# 9. JIT Compiler

JIT stands for:

> **Just-In-Time Compiler**

The JIT compiler identifies code that is executed frequently and compiles it into native machine code.

Conceptually:

```text
Bytecode
   ↓
Interpreter
   ↓
Frequently executed?
   |
   +---- No ----> Continue interpreting
   |
   +---- Yes ---> JIT Compiler
                       ↓
                Native Machine Code
                       ↓
                      CPU
```

### Main Idea

Instead of repeatedly interpreting hot bytecode:

```text
Bytecode
 ↓
Interpret
 ↓
Interpret
 ↓
Interpret
 ↓
Interpret
```

the JVM can compile the hot code:

```text
Bytecode
   ↓
JIT
   ↓
Native Code
   ↓
CPU
```

---

# 10. Why JIT is Needed

Suppose a method is executed millions of times.

```java
static int square(int x) {
    return x * x;
}
```

If the method becomes hot, repeatedly interpreting its bytecode may be inefficient.

The JVM can compile it into optimized native machine code.

Then:

```text
square()
   ↓
JIT compiled native code
   ↓
CPU
```

### Key Principle

```text
Cold Code
   ↓
Interpret

Hot Code
   ↓
JIT Compile
   ↓
Optimized Native Code
```

This allows the JVM to combine:

```text
Fast startup
+
Runtime profiling
+
Native-code performance
```

---

# 11. JIT Compilation Working

A simplified process:

```text
1. Class loaded
       ↓
2. Bytecode available
       ↓
3. Method starts executing
       ↓
4. Interpreter executes it
       ↓
5. JVM collects runtime information
       ↓
6. Method becomes hot
       ↓
7. JIT compiles it
       ↓
8. Optimized machine code generated
       ↓
9. CPU executes native code
```

### Important

JIT compilation happens **at runtime**.

That is why it is called:

```text
Just-In-Time
```

The JVM waits until runtime information can help it make optimization decisions.

---

# 12. Hot Code

**Hot code** is code that executes frequently or is otherwise important enough for the JVM to optimize.

Typical hot code includes:

```text
Frequently called methods
Frequently executed loops
Performance-critical application paths
```

Example:

```java
for (int i = 0; i < 1_000_000; i++) {
    calculate(i);
}
```

The loop and/or method may become hot.

### Hotness

Conceptually:

```text
Execution Count
      |
      v
   1 time
   10 times
   100 times
   1000 times
      |
      v
Hot Code
      |
      v
JIT Compilation
```

The actual compilation thresholds and decisions are implementation-specific and can depend on JVM flags, runtime profiling, and collector/JVM configuration.

---

# 13. HotSpot

**HotSpot** is a JVM implementation developed by Oracle/OpenJDK.

It is famous for its runtime profiling and JIT compilation technology.

The name is based on the idea of identifying frequently executed "hot spots" in the application.

Conceptually:

```text
Application
    ↓
Runtime Profiling
    ↓
Identify Hot Code
    ↓
JIT Compile
    ↓
Optimize
```

### Important

Do not confuse:

```text
HotSpot
```

with:

```text
JIT
```

HotSpot is a JVM implementation.

JIT compilation is a technique used by JVM implementations.

---

# 14. Method Compilation

A method does not necessarily need to be compiled into native code immediately.

Conceptually:

```text
Method Loaded
      ↓
Interpreted
      ↓
Execution Profile Collected
      ↓
Becomes Hot
      ↓
JIT Compiled
      ↓
Native Code
```

This avoids spending compilation resources on code that may never become important.

---

# 15. On-Stack Replacement

**On-Stack Replacement (OSR)** allows the JVM to transition a currently running method or loop from interpreted execution to compiled execution.

Consider:

```java
while (condition) {
    calculate();
}
```

Suppose the loop is already executing and becomes hot.

Instead of waiting for the entire method invocation to finish, the JVM may compile the hot loop and transfer execution to the compiled version.

Conceptually:

```text
Running Method
      |
      v
Hot Loop Detected
      |
      v
JIT Compiles Loop
      |
      v
OSR
      |
      v
Continue execution
using compiled code
```

### Why Important?

Without OSR, a long-running loop could remain interpreted for a significant amount of time before the method returns and gets invoked again.

---

# 16. Code Cache

When the JIT compiler generates native machine code, the JVM needs memory to store that compiled code.

This is associated with the **Code Cache**.

Conceptually:

```text
JIT Compiler
     |
     v
Native Machine Code
     |
     v
Code Cache
     |
     v
CPU executes it
```

### Important

The Code Cache is separate from the Java heap.

```text
Java Heap
   ≠
Code Cache
```

The JVM manages the Code Cache as part of its native/runtime memory infrastructure.

---

# 17. Tiered Compilation

Modern HotSpot JVMs commonly use **tiered compilation**.

The basic idea is to use different levels of execution and compilation rather than immediately applying the most expensive optimizations.

Conceptually:

```text
Bytecode
   ↓
Interpreter
   ↓
Profiling
   ↓
C1 compilation
   ↓
More profiling
   ↓
C2 compilation
   ↓
Highly optimized native code
```

### Goal

Balance:

```text
Startup Performance
       +
Compilation Cost
       +
Peak Performance
```

---

# 18. C1 and C2 Compilers

Traditional HotSpot terminology commonly refers to two JIT compiler tiers:

```text
C1
C2
```

### C1

C1 is generally designed for relatively fast compilation with lower compilation overhead.

Conceptually:

```text
C1
 ↓
Quick compilation
 ↓
Good performance
```

### C2

C2 is a more aggressive optimizing compiler designed to spend more compilation effort on hot code.

Conceptually:

```text
C2
 ↓
More profiling
 ↓
More optimization
 ↓
Potentially better peak performance
```

### Simplified Flow

```text
Interpreter
     ↓
    C1
     ↓
    C2
```

The exact compilation behavior is more complex than this simplified picture.

---

# 19. JIT Optimizations

The JIT compiler can use runtime information to optimize code.

Common optimization concepts include:

```text
Inlining
Dead Code Elimination
Loop Optimizations
Escape Analysis
Lock Elimination
Scalar Replacement
Constant Folding
Devirtualization
```

### Important

The JIT does not blindly apply every optimization.

It makes decisions based on runtime profiling, correctness constraints, compilation heuristics, and JVM implementation details.

---

# 20. Inlining

Inlining replaces a method call with the method's body when the JVM determines that doing so is beneficial.

Example:

```java
static int square(int x) {
    return x * x;
}

int result = square(5);
```

Conceptually:

```text
Before:

result = square(5);

After inlining:

result = 5 * 5;
```

### Benefits

Inlining can:

- reduce method-call overhead
- expose more code to further optimization
- enable constant propagation
- enable dead-code elimination
- improve optimization opportunities

### Important

Inlining does not mean the Java source code itself is rewritten.

It is a JIT-level optimization of generated machine code.

---

# 21. Dead Code Elimination

Dead code is code whose result or effects are determined to be unnecessary under the relevant execution assumptions.

Example:

```java
int x = 10;

if (false) {
    System.out.println("Hello");
}
```

The unreachable branch can potentially be eliminated.

Conceptually:

```text
Original
   ↓
Analyze
   ↓
Determine unnecessary code
   ↓
Remove from generated code
```

### Important

The optimizer must preserve observable program behavior.

It cannot simply remove code because it "looks unused" if that code has required side effects.

---

# 22. Loop Optimizations

Loops are often performance-critical.

Example:

```java
for (int i = 0; i < 1_000_000; i++) {
    sum += values[i];
}
```

The JIT can apply optimizations such as:

```text
Loop unrolling
Loop invariant code motion
Range-check optimizations
Other loop transformations
```

### Example: Loop Invariant Code

Suppose:

```java
for (int i = 0; i < 1000; i++) {
    result += calculateConstant();
}
```

If `calculateConstant()` can safely be determined to produce the same result and has no relevant side effects, the JVM may move or simplify the calculation.

---

# 23. Escape Analysis

Escape analysis determines whether an object escapes a particular scope or thread.

Example:

```java
static int calculate() {

    Point point = new Point(10, 20);

    return point.x + point.y;
}
```

If the JVM determines that `point` does not escape the method/thread, it may optimize its representation.

Potential optimizations can include:

```text
Scalar Replacement
Lock Elimination
Allocation Elimination
```

### Important

Escape analysis does not mean:

> "All non-escaping objects are automatically moved to the stack."

That is a common misconception.

The JVM may eliminate the allocation entirely or represent the object's fields separately if optimization is safe.

---

# 24. Lock Elimination

Suppose an object is used for synchronization:

```java
Object lock = new Object();

synchronized (lock) {
    calculate();
}
```

If the JVM proves that the lock object cannot be observed by another thread, synchronization may be unnecessary.

The JIT may eliminate the locking operations.

Conceptually:

```text
Synchronization
      ↓
Escape Analysis
      ↓
Prove lock cannot be contended
      ↓
Eliminate unnecessary locking
```

### Important

This is only possible when the JVM can prove that removing synchronization does not change observable behavior.

---

# 25. Scalar Replacement

Suppose an object does not escape:

```java
class Point {
    int x;
    int y;
}
```

Instead of allocating the object as one heap object, the JIT may represent its fields separately in registers or stack-like compiler-managed locations when safe.

Conceptually:

```text
Normal:

Point Object
+---------+
| x       |
| y       |
+---------+
```

Potential optimized representation:

```text
x → separate value
y → separate value
```

The object allocation may disappear from the optimized machine code.

### Important

This is an optimization performed by the JIT.

It does not change Java's language-level object model.

---

# 26. Deoptimization

JIT optimizations are based on assumptions.

Sometimes those assumptions become invalid.

Example:

```text
Runtime Profile
      ↓
JIT makes assumption
      ↓
Generates optimized code
      ↓
Application behavior changes
      ↓
Assumption becomes invalid
      ↓
Deoptimization
```

The JVM can abandon the optimized version and return execution to a safer representation, often interpreted or differently compiled code.

### Example Concept

Suppose the JIT assumes a particular type is commonly used:

```text
Profile:
Dog → very common
```

It optimizes based on that observation.

Later:

```text
Cat → appears
```

If the optimization's assumptions no longer hold, the JVM can deoptimize and continue safely.

---

# 27. Interpreter vs JIT

| Feature | Interpreter | JIT Compiler |
|---|---|---|
| Execution | Bytecode interpretation | Native machine code |
| Startup | Usually fast | Compilation adds overhead |
| Repeated hot code | Less efficient | Highly optimized |
| Runtime profiling | Can provide profiling data | Uses profiling |
| Compilation cost | Minimal | Present |
| Peak performance | Usually lower | Usually higher for hot code |
| Best for | Cold/infrequent code | Hot/frequently executed code |

### Simplified Strategy

```text
Cold Code
   ↓
Interpreter

Hot Code
   ↓
JIT
```

---

# 28. JVM Execution Flow

The complete simplified execution flow is:

```text
                 .java
                   |
                   v
                javac
                   |
                   v
              .class file
                   |
                   v
             Class Loader
                   |
                   v
          Runtime Data Areas
                   |
                   v
             Bytecode
                   |
                   v
             Interpreter
                   |
          Runtime Profiling
                   |
             Is it hot?
              /       \
            No         Yes
            |           |
            v           v
       Interpret      JIT
                        |
                        v
                Native Machine Code
                        |
                        v
                    Code Cache
                        |
                        v
                       CPU
```

---

# 29. Execution Engine and Memory

The Execution Engine interacts with multiple JVM-managed memory areas.

### Stack

Each thread has a JVM stack containing frames for method execution.

Conceptually:

```text
Thread
  ↓
JVM Stack
  ↓
Stack Frame
  ↓
Local Variables
Operand Stack
Return Information
```

### Heap

Objects are allocated in the Java heap according to JVM and collector behavior.

```text
new Employee()
      ↓
     Heap
```

### Metaspace

Class metadata is stored in Metaspace in HotSpot.

```text
Class Metadata
      ↓
  Metaspace
```

### Code Cache

Compiled native code generated by JIT is stored in the Code Cache.

```text
JIT
 ↓
Native Code
 ↓
Code Cache
```

---

# 30. Execution Engine and Garbage Collection

The Execution Engine and Garbage Collector have different responsibilities.

### Execution Engine

```text
Execute bytecode
JIT compile hot code
Optimize execution
```

### Garbage Collector

```text
Identify unreachable objects
Reclaim heap memory
Compact/evacuate where applicable
Manage collector-specific memory regions
```

They work together during JVM execution.

Example:

```text
Application executes
      ↓
Objects allocated
      ↓
Heap grows
      ↓
GC required
      ↓
Garbage Collector runs
      ↓
Execution continues
```

### Important

The JIT compiler can also affect allocation and object lifetime behavior through optimizations such as escape analysis and allocation elimination.

---

# 31. Common Misconceptions

## Misconception 1

> JVM directly executes Java source code.

### Correct

The normal flow is:

```text
Java Source
   ↓
Bytecode
   ↓
JVM
```

The JVM executes bytecode.

---

## Misconception 2

> Java is purely interpreted.

### Correct

Modern JVMs commonly combine interpretation with JIT compilation.

```text
Interpreter + JIT
```

---

## Misconception 3

> JIT compiles the entire application immediately.

### Correct

JIT compilation is selective and runtime-driven.

Hot code receives compilation attention.

---

## Misconception 4

> JIT means Java becomes C++.

### Correct

JIT compiles JVM bytecode into native machine code suitable for the target platform.

It does not convert Java into C++.

---

## Misconception 5

> HotSpot is the JIT compiler.

### Correct

HotSpot is a JVM implementation that contains runtime execution technologies including JIT compilation.

---

## Misconception 6

> Every method is interpreted forever.

### Correct

Frequently executed methods can be JIT compiled.

---

## Misconception 7

> Every object that does not escape is placed on the stack.

### Correct

Escape analysis may allow the JVM to eliminate an allocation or use scalar replacement.

It does not imply a mandatory stack allocation.

---

## Misconception 8

> JIT optimization always makes code faster.

### Correct

Compilation and optimization have costs. The JVM uses runtime heuristics to determine when optimization is worthwhile.

---

## Misconception 9

> JIT changes the Java source code.

### Correct

JIT optimizes generated machine code/runtime execution.

The original Java source code is not rewritten.

---

## Misconception 10

> Deoptimization means the program failed.

### Correct

Deoptimization is a normal JVM mechanism for safely abandoning an optimized version when its assumptions are no longer valid.

---

# 32. Interview Traps

## Trap 1

### Question

What is the main job of the Execution Engine?

### Answer

To execute JVM bytecode, using interpretation and JIT compilation as appropriate.

---

## Trap 2

### Question

What is JIT?

### Answer

Just-In-Time compilation converts frequently executed JVM bytecode into native machine code at runtime.

---

## Trap 3

### Question

Why doesn't the JVM compile everything immediately?

### Answer

Compilation has overhead. Runtime profiling allows the JVM to focus compilation and optimization effort on code that is actually important.

---

## Trap 4

### Question

What is hot code?

### Answer

Frequently executed or performance-critical code identified through runtime profiling.

---

## Trap 5

### Question

What is OSR?

### Answer

On-Stack Replacement allows currently executing code, particularly a hot loop, to transition from interpreted execution to compiled execution without waiting for the current invocation to finish.

---

## Trap 6

### Question

Where is JIT-generated code stored?

### Answer

In JVM-managed Code Cache memory.

---

## Trap 7

### Question

What are C1 and C2?

### Answer

They refer to JIT compiler tiers in HotSpot. C1 generally emphasizes faster compilation, while C2 performs more aggressive optimization for hot code.

---

## Trap 8

### Question

What is inlining?

### Answer

Replacing a method call with the method's body in optimized compiled code when the JVM determines it is beneficial and safe.

---

## Trap 9

### Question

What is deoptimization?

### Answer

It is the process of abandoning optimized compiled code when assumptions used for that optimization become invalid.

---

## Trap 10

### Question

Does JIT compilation happen during `javac`?

### Answer

No.

`javac` normally compiles Java source into JVM bytecode.

JIT compilation occurs later at JVM runtime.

---

# 33. Top 15 Interview Questions

## Q1. What is the JVM Execution Engine?

### Answer

The Execution Engine executes JVM bytecode using interpretation and JIT compilation.

---

## Q2. What is an interpreter in Java?

### Answer

The JVM interpreter executes bytecode instructions directly, one instruction at a time from the JVM's perspective.

---

## Q3. What is JIT?

### Answer

JIT stands for Just-In-Time Compiler. It compiles frequently executed bytecode into native machine code at runtime.

---

## Q4. Why does Java use both Interpreter and JIT?

### Answer

The interpreter provides fast initial execution, while JIT compilation improves performance of frequently executed code by generating optimized native machine code.

---

## Q5. What is hot code?

### Answer

Hot code is frequently executed or otherwise identified by runtime profiling as important enough for optimization.

---

## Q6. What is HotSpot?

### Answer

HotSpot is a JVM implementation from the OpenJDK/Oracle ecosystem that uses runtime profiling and JIT compilation to optimize application execution.

---

## Q7. What is tiered compilation?

### Answer

Tiered compilation uses multiple execution and compilation levels to balance startup speed, profiling, compilation cost, and peak performance.

---

## Q8. What are C1 and C2?

### Answer

They are commonly used names for two HotSpot JIT compilation tiers. C1 generally compiles quickly, while C2 performs more aggressive optimization for sufficiently hot code.

---

## Q9. What is method inlining?

### Answer

Inlining replaces a method call with the method's body in generated optimized code, reducing call overhead and enabling additional optimizations.

---

## Q10. What is On-Stack Replacement?

### Answer

OSR allows the JVM to transition an already-running method or loop from interpreted execution to compiled execution.

---

## Q11. What is Code Cache?

### Answer

Code Cache is JVM-managed memory used to store compiled native machine code generated by the JIT compiler.

---

## Q12. What is escape analysis?

### Answer

Escape analysis determines whether an object escapes a method or thread and can enable optimizations such as allocation elimination, lock elimination, and scalar replacement.

---

## Q13. What is deoptimization?

### Answer

Deoptimization allows the JVM to stop using optimized compiled code when assumptions behind that optimization become invalid.

---

## Q14. Does JIT compile all Java code?

### Answer

No.

The JVM selectively compiles code based on runtime execution and profiling information.

---

## Q15. Is Java interpreted or compiled?

### Answer

Both concepts are involved.

```text
javac
 ↓
Source → Bytecode

JVM
 ↓
Interpretation + JIT Compilation
```

Java source is compiled to bytecode, and the JVM can interpret that bytecode and JIT-compile hot code into native machine code.

---

# 34. 30-Second Interview Answer

> The JVM Execution Engine is responsible for executing Java bytecode. Initially, bytecode can be executed by the interpreter while the JVM collects runtime profiling information. When code becomes hot, the JIT compiler can compile it into optimized native machine code, which is stored in the Code Cache and executed directly by the CPU. Modern HotSpot JVMs use tiered compilation and optimizations such as inlining, escape analysis, loop optimizations, and deoptimization to balance startup time and peak performance.

---

# 35. Cheat Sheet

```text
╔══════════════════════════════════════════════════════════╗
║              JVM EXECUTION ENGINE                       ║
╠══════════════════════════════════════════════════════════╣
║ Main Job       → Execute JVM bytecode                    ║
║                                                          ║
║ Interpreter    → Executes bytecode                       ║
║ JIT            → Compiles hot code                      ║
║ Hot Code       → Frequently executed code               ║
║ HotSpot        → JVM implementation                      ║
║ C1             → Faster compilation tier                ║
║ C2             → More aggressive optimization            ║
║ OSR            → Switch running code to compiled code    ║
║ Code Cache     → Stores compiled native code             ║
║                                                          ║
║ Inlining       → Remove method-call overhead             ║
║ Escape Analysis→ Analyze object escape                   ║
║ Scalar Replace → Replace object with individual values   ║
║ Lock Elim.     → Remove unnecessary synchronization      ║
║ Deoptimization → Abandon invalid optimization            ║
╚══════════════════════════════════════════════════════════╝
```

## 🧠 Execution Memory Trick

```text
BYTECODE
   ↓
INTERPRETER
   ↓
PROFILE
   ↓
HOT?
  / \
NO   YES
 |     |
 ↓     ↓
KEEP   JIT
       ↓
   NATIVE CODE
       ↓
   CODE CACHE
       ↓
      CPU
```

## 🔥 JVM Execution Architecture

```text
Java Source
    ↓
  javac
    ↓
 Bytecode
    ↓
Class Loader
    ↓
Runtime Data Areas
    ↓
Execution Engine
    |
    +---- Interpreter
    |
    +---- JIT Compiler
             |
             +---- C1
             |
             +---- C2
    |
    v
Native Machine Code
    |
    v
Code Cache
    |
    v
CPU
```

## 🔥 Important Optimizations

```text
JIT
 |
 +---- Inlining
 |
 +---- Dead Code Elimination
 |
 +---- Loop Optimizations
 |
 +---- Escape Analysis
 |
 +---- Lock Elimination
 |
 +---- Scalar Replacement
 |
 +---- Devirtualization
 |
 +---- Constant Folding
```

## ⭐ Final Interview Rule

```text
javac
→ Java source → JVM bytecode

Interpreter
→ Bytecode execution

JIT
→ Hot bytecode → Native machine code

HotSpot
→ JVM implementation

C1
→ Faster compilation

C2
→ More aggressive optimization

OSR
→ Compile currently running hot code

Code Cache
→ Stores JIT-generated native code

Inlining
→ Replace method call with body

Escape Analysis
→ Determine whether object escapes

Deoptimization
→ Fall back when optimization assumptions become invalid
```

## 🚀 One-Line Revision

```text
Java Source → javac → Bytecode → Interpreter → Profiling → JIT → Native Code → CPU
```