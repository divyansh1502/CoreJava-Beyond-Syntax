# 13 — PC Register

## 📚 Table of Contents

1. [Introduction](#1-introduction)
2. [What is the PC Register?](#2-what-is-the-pc-register)
3. [Why Does JVM Need a PC Register?](#3-why-does-jvm-need-a-pc-register)
4. [PC Register in JVM Runtime Data Areas](#4-pc-register-in-jvm-runtime-data-areas)
5. [What Does PC Stand For?](#5-what-does-pc-stand-for)
6. [What Does the PC Register Store?](#6-what-does-the-pc-register-store)
7. [PC Register and Bytecode](#7-pc-register-and-bytecode)
8. [PC Register and Threads](#8-pc-register-and-threads)
9. [Why is PC Register Thread-Private?](#9-why-is-pc-register-thread-private)
10. [PC Register During Method Execution](#10-pc-register-during-method-execution)
11. [PC Register and JVM Stack](#11-pc-register-and-jvm-stack)
12. [PC Register and Native Methods](#12-pc-register-and-native-methods)
13. [PC Register and Multithreading](#13-pc-register-and-multithreading)
14. [PC Register and Method Calls](#14-pc-register-and-method-calls)
15. [PC Register and Loops](#15-pc-register-and-loops)
16. [PC Register and Branching](#16-pc-register-and-branching)
17. [PC Register and Exceptions](#17-pc-register-and-exceptions)
18. [PC Register and JIT](#18-pc-register-and-jit)
19. [PC Register and JVM Specification](#19-pc-register-and-jvm-specification)
20. [PC Register vs CPU Program Counter](#20-pc-register-vs-cpu-program-counter)
21. [PC Register vs JVM Stack](#21-pc-register-vs-jvm-stack)
22. [PC Register vs Native Method Stack](#22-pc-register-vs-native-method-stack)
23. [PC Register and Memory](#23-pc-register-and-memory)
24. [PC Register and Garbage Collection](#24-pc-register-and-garbage-collection)
25. [What Happens When a Thread Switches?](#25-what-happens-when-a-thread-switches)
26. [Simple Example](#26-simple-example)
27. [Internal Execution Example](#27-internal-execution-example)
28. [Common Misconceptions](#28-common-misconceptions)
29. [Interview Traps](#29-interview-traps)
30. [Top 20 Interview Questions](#30-top-20-interview-questions)
31. [30-Second Interview Answer](#31-30-second-interview-answer)
32. [Cheat Sheet](#32-cheat-sheet)

---

# 1. Introduction

The JVM Runtime Data Areas contain several important memory/runtime regions.

One of them is the:

> **PC Register**

PC stands for:

> **Program Counter**

The PC Register keeps track of the instruction that the current Java thread should execute next.

A simplified view of JVM Runtime Data Areas is:

```text
JVM Runtime Data Areas
│
├── PC Register
├── JVM Stack
├── Heap
├── Method Area
└── Native Method Stack
```

The PC Register is special because it is:

```text
Thread-private
```

Every JVM thread has its own PC Register.

---

# 2. What is the PC Register?

### Definition

> The PC Register is a per-thread JVM runtime data area that keeps track of the current position in the execution of JVM instructions for that thread.

In simplified terms:

```text
PC Register
     ↓
"Which instruction should this thread execute next?"
```

For example:

```text
Instruction 1
Instruction 2
Instruction 3
Instruction 4
```

The PC Register tracks the current execution position.

Conceptually:

```text
PC = Instruction 1
        ↓
PC = Instruction 2
        ↓
PC = Instruction 3
        ↓
PC = Instruction 4
```

---

# 3. Why Does JVM Need a PC Register?

A Java application can have multiple threads:

```text
Thread 1
Thread 2
Thread 3
```

Each thread may be executing a different method or a different point inside the same method.

Therefore, each thread needs its own execution position.

For example:

```text
Thread 1 → executing instruction 20
Thread 2 → executing instruction 57
Thread 3 → executing instruction 12
```

The JVM needs to know where each thread should continue execution.

The PC Register provides this execution-position information.

---

# 4. PC Register in JVM Runtime Data Areas

The JVM Runtime Data Areas can be broadly represented as:

```text
                    JVM
                     │
          Runtime Data Areas
                     │
       ┌─────────────┼─────────────┐
       │             │             │
       ▼             ▼             ▼
 PC Register       JVM Stack      Heap
   │
   │
 Thread-private

Method Area
   │
   └── Class-level information

Native Method Stack
   │
   └── Native method execution
```

A more precise classification:

```text
Per Thread
│
├── PC Register
├── JVM Stack
└── Native Method Stack

Shared
│
├── Heap
└── Method Area
```

### Important

The PC Register is:

```text
Per-thread
```

while the Heap is:

```text
Shared among JVM threads
```

---

# 5. What Does PC Stand For?

PC stands for:

> **Program Counter**

It is also sometimes described conceptually as the:

> **Instruction pointer**

Its purpose is to track execution progress.

Memory trick:

```text
PC
↓
Program Counter
↓
"Where am I in execution?"
```

---

# 6. What Does the PC Register Store?

For a thread executing a Java method, the PC Register conceptually indicates the next JVM instruction to execute.

For example, suppose bytecode instructions are:

```text
0
1
2
3
4
5
```

The PC may conceptually progress:

```text
PC = 0
PC = 1
PC = 2
PC = 3
...
```

The exact representation is JVM implementation-specific.

### Important

Do not think of the PC Register as simply storing:

```text
"line number of Java source code"
```

It is related to the JVM's instruction execution, not directly to source-code line numbers.

---

# 7. PC Register and Bytecode

Java source code:

```java
int x = 10;
int y = 20;
int z = x + y;
```

is compiled into JVM bytecode.

Conceptually:

```text
Java Source
     ↓
javac
     ↓
Bytecode Instructions
     ↓
JVM Execution
     ↓
PC Register tracks execution position
```

The PC Register is therefore closely associated with bytecode execution.

---

# 8. PC Register and Threads

Every JVM thread has its own PC Register.

Consider:

```java
public class Demo {

    public static void main(String[] args) {

        Thread t1 = new Thread(() -> {
            calculate();
        });

        Thread t2 = new Thread(() -> {
            calculate();
        });

        t1.start();
        t2.start();
    }

    static void calculate() {

        for (int i = 0; i < 100; i++) {
            System.out.println(i);
        }
    }
}
```

Conceptually:

```text
Thread 1
   │
   ├── JVM Stack
   └── PC Register

Thread 2
   │
   ├── JVM Stack
   └── PC Register
```

Both threads can execute the same method:

```text
calculate()
```

but each thread maintains its own execution state.

---

# 9. Why is PC Register Thread-Private?

Suppose two threads share one PC Register:

```text
Thread 1 ──┐
           ├── Shared PC
Thread 2 ──┘
```

This would make it impossible to independently track where each thread should continue execution.

Instead:

```text
Thread 1 → PC Register 1
Thread 2 → PC Register 2
Thread 3 → PC Register 3
```

Each thread can therefore maintain its own execution position.

### Key Point

> The PC Register is thread-private because each thread independently executes JVM instructions.

---

# 10. PC Register During Method Execution

Consider:

```java
static int add(int a, int b) {

    int result = a + b;

    return result;
}
```

Conceptually:

```text
Method starts
    ↓
Instruction A
    ↓
Instruction B
    ↓
Instruction C
    ↓
return
```

The PC Register changes as execution progresses.

```text
PC
↓
A
↓
B
↓
C
↓
Return
```

The actual JVM implementation may represent execution state differently when code is interpreted or compiled, but conceptually the JVM must track where execution should continue.

---

# 11. PC Register and JVM Stack

The PC Register and JVM Stack are both thread-private runtime areas, but they have different purposes.

### PC Register

Tracks:

```text
Current execution position
```

### JVM Stack

Contains:

```text
Stack frames
```

Each method invocation creates a stack frame.

Conceptually:

```text
Thread
 │
 ├── PC Register
 │
 └── JVM Stack
       │
       ├── Frame: main()
       │
       ├── Frame: calculate()
       │
       └── Frame: add()
```

The PC Register tells the JVM where the thread is executing.

The JVM Stack stores the execution context associated with method invocations.

---

# 12. PC Register and Native Methods

The JVM specification has a special rule for native methods.

When a thread is executing a native method:

```text
Java
  ↓
Native Method
  ↓
JNI / Native Code
```

the PC Register's value is not required to represent a JVM bytecode instruction in the same way as it does for a Java method.

The JVM specification states that the value of the PC register is undefined when the currently executing method is native.

### Important Interview Point

```text
Java method
→ PC tracks JVM instruction execution

Native method
→ PC value is undefined according to JVM specification
```

This is an important detail that is frequently missed in interview answers.

---

# 13. PC Register and Multithreading

Suppose:

```text
Thread A
Thread B
```

Both execute Java code.

The operating system/JVM may switch execution between them.

Conceptually:

```text
Thread A
   ↓
Execute
   ↓
Pause

Thread B
   ↓
Execute
   ↓
Pause

Thread A
   ↓
Continue
```

When Thread A resumes, the JVM needs its execution state.

The PC Register is part of the per-thread execution state used to identify where execution should continue.

---

# 14. PC Register and Method Calls

Consider:

```java
public static void main(String[] args) {

    methodA();
    methodB();
}
```

Execution conceptually:

```text
main()
  ↓
methodA()
  ↓
return
  ↓
methodB()
```

When `methodA()` is called:

```text
main() frame
       ↓
methodA() frame
```

The JVM Stack manages method frames.

The PC Register tracks the current instruction position for the executing thread.

When `methodA()` returns:

```text
methodA() frame removed
       ↓
main() continues
```

The JVM resumes execution at the appropriate instruction in `main()`.

---

# 15. PC Register and Loops

Consider:

```java
for (int i = 0; i < 3; i++) {

    System.out.println(i);
}
```

The loop contains multiple bytecode instructions.

Conceptually:

```text
Initialize i
    ↓
Check condition
    ↓
Execute body
    ↓
Increment i
    ↓
Jump back
    ↓
Check condition
```

The PC Register follows the instruction flow.

Conceptually:

```text
      ┌──────────────┐
      │              ▼
Initialize → Check → Body
              │       │
              │       ▼
              │     Increment
              │       │
              └───────┘
```

The PC therefore does not simply increase forever.

Branch and jump instructions can change the next execution position.

---

# 16. PC Register and Branching

Bytecode contains control-flow instructions.

For example:

```java
if (x > 10) {
    System.out.println("Large");
} else {
    System.out.println("Small");
}
```

Conceptually:

```text
Condition
   ↓
 ┌─┴─┐
True False
 ↓     ↓
A      B
```

The JVM's execution position changes according to the branch result.

Therefore:

```text
PC
 ↓
Instruction
 ↓
Branch
 ↓
New execution position
```

The PC Register helps represent the current position in this instruction flow.

---

# 17. PC Register and Exceptions

Consider:

```java
try {

    int result = 10 / 0;

} catch (ArithmeticException e) {

    System.out.println("Exception");
}
```

When an exception occurs, normal sequential execution changes.

Conceptually:

```text
Normal execution
      ↓
Exception
      ↓
Find matching handler
      ↓
Transfer execution
      ↓
Catch block
```

The JVM must change its execution path.

The PC/execution state is therefore part of the overall mechanism that allows the thread to continue from the appropriate bytecode location.

---

# 18. PC Register and JIT

JIT compilation changes how bytecode is executed.

Initially:

```text
Bytecode
   ↓
Interpreter
```

Later:

```text
Hot Bytecode
     ↓
JIT
     ↓
Native Machine Code
```

The conceptual role of the PC is still about tracking execution position, but compiled code execution may use native CPU registers and machine-code instruction addresses rather than interpreting JVM bytecode one instruction at a time.

Therefore, avoid saying:

> "The PC Register always contains the current JVM bytecode instruction."

That is an oversimplification once compiled/native execution is involved.

A better statement is:

> For a thread executing a Java method, the JVM specification defines the PC register as the current JVM instruction position; for native methods its value is undefined. The concrete implementation of execution state can differ when code is compiled.

---

# 19. PC Register and JVM Specification

The JVM Specification defines the PC register as one of the runtime data areas.

Each JVM thread has its own PC register.

For a thread executing a Java method, the PC register contains the address of the JVM instruction currently being executed.

For a native method, its value is undefined.

### Important

The specification does not require a particular physical hardware implementation.

Therefore:

```text
JVM specification
        ↓
Defines required behavior/semantics
        ↓
JVM implementation
        ↓
Chooses actual internal representation
```

This distinction is important in JVM internals.

---

# 20. PC Register vs CPU Program Counter

These concepts are related but should not be treated as identical.

### JVM PC Register

Tracks the execution position of JVM instructions for a Java thread.

### CPU Program Counter / Instruction Pointer

Tracks the address of the native machine instruction being executed by the CPU.

Conceptually:

```text
JVM level:

PC Register
    ↓
JVM instruction


CPU level:

Instruction Pointer
    ↓
Machine instruction
```

When JIT-generated native code is executing, the CPU's own instruction pointer becomes relevant to native execution.

### Interview Answer

> The JVM PC register is a JVM-level execution-state concept, while the CPU program counter/instruction pointer is a hardware-level register used to track native machine-code execution.

---

# 21. PC Register vs JVM Stack

| Feature | PC Register | JVM Stack |
|---|---|---|
| Purpose | Tracks execution position | Stores method execution frames |
| Scope | Per thread | Per thread |
| Stores | Current JVM instruction position | Frames, local variables, operand stack, etc. |
| Created per thread | Yes | Yes |
| Shared? | No | No |
| Associated with method execution | Yes | Yes |
| Stores objects | No | References/local data can point to objects |
| Main role | Instruction tracking | Method execution state |

### Memory Trick

```text
PC
→ Where am I?

Stack
→ What am I executing with?
```

---

# 22. PC Register vs Native Method Stack

### PC Register

Tracks JVM instruction execution for Java methods.

### Native Method Stack

Supports execution of native methods.

Conceptually:

```text
Java Method
    ↓
JVM Stack
    +
PC Register


Native Method
    ↓
Native Method Stack
```

The exact implementation of native method stacks is JVM-specific.

---

# 23. PC Register and Memory

The PC Register is a JVM runtime data area.

It is not:

```text
Java object
```

and it is not:

```text
Java heap memory
```

It is associated with a particular JVM thread.

Conceptually:

```text
Thread
│
├── PC Register
├── JVM Stack
└── Native Method Stack
```

Meanwhile:

```text
Heap
│
└── Shared Java objects
```

---

# 24. PC Register and Garbage Collection

The PC Register does not store Java objects.

Therefore, it is not a normal target for garbage collection.

Garbage collection primarily deals with Java heap objects.

Conceptually:

```text
PC Register
     │
     └── Execution position

Heap
     │
     └── Java objects
             ↓
            GC
```

### Important

Do not say:

> "GC clears the PC Register."

Garbage collection and PC register management are separate concepts.

---

# 25. What Happens When a Thread Switches?

Suppose:

```text
Thread A
```

is executing:

```text
Instruction 50
```

Then the scheduler switches to:

```text
Thread B
```

which is at:

```text
Instruction 120
```

Conceptually:

```text
Thread A
PC → 50
   ↓
Pause

Thread B
PC → 120
   ↓
Execute

Thread A
PC → Continue from its saved execution state
```

Each thread has independent execution state.

This is why PC registers are thread-private.

### Important

The exact interaction between JVM thread state, OS scheduling, compiled code, and hardware registers is JVM/OS implementation-specific.

---

# 26. Simple Example

Consider:

```java
public class Demo {

    public static void main(String[] args) {

        int x = 10;

        int y = 20;

        int result = x + y;

        System.out.println(result);
    }
}
```

Conceptually:

```text
main()
   ↓
instruction: initialize x
   ↓
instruction: initialize y
   ↓
instruction: calculate result
   ↓
instruction: call println
   ↓
return
```

The PC Register tracks the current execution position of the thread while these JVM instructions are being executed.

---

# 27. Internal Execution Example

Consider:

```java
static int add(int a, int b) {

    return a + b;
}
```

Suppose the JVM bytecode is conceptually:

```text
Instruction 0
Instruction 1
Instruction 2
Instruction 3
Instruction 4
```

Execution:

```text
PC → 0
     ↓
execute instruction
     ↓
PC → 1
     ↓
execute instruction
     ↓
PC → 2
     ↓
execute instruction
     ↓
PC → 3
     ↓
return
```

This is a simplified conceptual model.

Actual JVM implementations can execute the method through:

```text
Interpreter
```

or:

```text
JIT-generated native code
```

so the physical implementation of execution state can differ.

---

# 28. Common Misconceptions

## Misconception 1

> PC Register stores the current Java source-code line number.

### Correct

It tracks the JVM instruction execution position, not directly the Java source-code line number.

---

## Misconception 2

> There is one PC Register for the entire JVM.

### Correct

Each JVM thread has its own PC Register.

---

## Misconception 3

> PC Register is stored in the Java heap.

### Correct

It is a per-thread JVM runtime data area, not a normal Java heap object.

---

## Misconception 4

> PC Register stores local variables.

### Correct

Local variables are stored in JVM stack frames.

The PC Register tracks execution position.

---

## Misconception 5

> PC Register stores the current object.

### Correct

It tracks instruction execution, not object references.

---

## Misconception 6

> PC Register is exactly the same as the CPU's program counter.

### Correct

They are conceptually related, but the JVM PC register is a JVM-level runtime concept, while the CPU instruction pointer is a hardware-level concept.

---

## Misconception 7

> PC Register always contains a JVM bytecode address.

### Correct

For a Java method, the specification defines a JVM instruction address. For a native method, the PC value is undefined.

Also, compiled execution can involve native machine instructions and implementation-specific execution state.

---

## Misconception 8

> Garbage Collector manages the PC Register.

### Correct

GC manages Java heap objects. The PC Register is part of per-thread execution state.

---

## Misconception 9

> PC Register is shared by all threads.

### Correct

Every JVM thread has its own PC Register.

---

## Misconception 10

> PC Register contains the entire execution history.

### Correct

It represents the current execution position, not the complete history of executed instructions.

---

# 29. Interview Traps

## Trap 1

### Question

What is the PC Register?

### Answer

The PC Register is a thread-private JVM runtime data area that tracks the execution position of JVM instructions for the current Java thread.

---

## Trap 2

### Question

Is PC Register shared?

### Answer

No. Each JVM thread has its own PC Register.

---

## Trap 3

### Question

What does PC stand for?

### Answer

Program Counter.

---

## Trap 4

### Question

Does PC Register store Java source-code line numbers?

### Answer

No. It tracks JVM instruction execution, not directly source-code line numbers.

---

## Trap 5

### Question

Where is the PC Register located?

### Answer

It is a per-thread JVM runtime data area. The JVM specification does not require a specific physical memory location or implementation.

---

## Trap 6

### Question

What happens to PC Register during a native method?

### Answer

According to the JVM specification, its value is undefined while the thread is executing a native method.

---

## Trap 7

### Question

What is the difference between PC Register and JVM Stack?

### Answer

```text
PC Register
→ Current execution position

JVM Stack
→ Method frames and method execution data
```

---

## Trap 8

### Question

Why does every thread need its own PC Register?

### Answer

Because every thread can be executing a different instruction or different method at the same time.

---

## Trap 9

### Question

Does GC clean the PC Register?

### Answer

No. The PC Register is execution state, not Java heap object memory managed by GC.

---

## Trap 10

### Question

Is JVM PC Register the same as CPU program counter?

### Answer

No. They serve analogous purposes at different abstraction levels.

---

# 30. Top 20 Interview Questions

## Q1. What is a PC Register in Java?

### Answer

It is a thread-private JVM runtime data area that tracks the current execution position of a Java thread.

---

## Q2. What does PC stand for?

### Answer

Program Counter.

---

## Q3. Is the PC Register shared among threads?

### Answer

No. Each JVM thread has its own PC Register.

---

## Q4. Why is PC Register thread-private?

### Answer

Because each thread independently executes instructions and therefore needs its own execution position.

---

## Q5. What does the PC Register contain?

### Answer

For a thread executing a Java method, it contains the address/position of the JVM instruction currently being executed, according to the JVM specification.

---

## Q6. Does PC Register store source-code line numbers?

### Answer

No. It tracks JVM instruction execution rather than directly storing Java source-code line numbers.

---

## Q7. Is PC Register part of JVM Runtime Data Areas?

### Answer

Yes.

---

## Q8. Is PC Register part of heap memory?

### Answer

No. It is a per-thread runtime data area.

---

## Q9. Does PC Register store local variables?

### Answer

No. Local variables are associated with JVM stack frames.

---

## Q10. What happens to PC Register when a thread executes a native method?

### Answer

Its value is undefined according to the JVM specification.

---

## Q11. What is the difference between PC Register and JVM Stack?

### Answer

The PC Register tracks execution position, while the JVM Stack contains stack frames for method execution.

---

## Q12. What is the difference between PC Register and CPU program counter?

### Answer

The JVM PC Register is a JVM-level execution concept, while the CPU program counter/instruction pointer tracks native machine-code execution.

---

## Q13. Does every JVM thread have a PC Register?

### Answer

Yes.

---

## Q14. Does garbage collection affect the PC Register?

### Answer

The PC Register is not Java heap memory managed by garbage collection.

---

## Q15. How does PC Register help in multithreading?

### Answer

Each thread maintains its own execution position, allowing threads to independently pause and resume their execution.

---

## Q16. Does the PC Register always increase sequentially?

### Answer

No. Control-flow instructions such as branches, loops, method returns, and exception handling can change the next execution position.

---

## Q17. Is PC Register physically a CPU register?

### Answer

Not necessarily. The JVM specification defines its semantics but does not require a particular physical implementation.

---

## Q18. Is PC Register relevant when JIT is used?

### Answer

The JVM still maintains the required execution state, but compiled execution uses native machine code and CPU-level execution state, so the simple bytecode-PC model should not be treated as the complete implementation detail.

---

## Q19. What happens to the PC when a method is called?

### Answer

Execution transfers to the called method, whose execution state is represented through a new stack frame and the thread's current execution state.

---

## Q20. Why is PC Register important for JVM internals?

### Answer

It is one of the JVM Runtime Data Areas and is fundamental to understanding how each thread tracks its instruction execution independently.

---

# 31. 30-Second Interview Answer

> The PC Register, or Program Counter, is a thread-private JVM Runtime Data Area that tracks the execution position of the current Java thread. For a Java method, it represents the address of the JVM instruction currently being executed. Every thread has its own PC Register because threads execute independently. It is different from the JVM Stack, which stores method frames and execution data. When a native method is executing, the JVM specification defines the PC register's value as undefined. It should also be distinguished from the CPU's program counter, which tracks native machine-code execution.

---

# 32. Cheat Sheet

```text
╔══════════════════════════════════════════════════════════╗
║                     PC REGISTER                         ║
╠══════════════════════════════════════════════════════════╣
║ PC              → Program Counter                       ║
║ Purpose         → Tracks execution position             ║
║ Scope           → Per thread                            ║
║ Shared?         → No                                    ║
║ Java Method     → JVM instruction position              ║
║ Native Method   → Value is undefined                    ║
║ Heap?           → No                                    ║
║ Stores objects? → No                                    ║
║ Stores locals?  → No                                    ║
║ GC managed?     → No                                    ║
╚══════════════════════════════════════════════════════════╝
```

## 🧠 Easy Memory Trick

```text
PC
↓
Program Counter
↓
"Where am I executing?"
```

Compare:

```text
PC Register
→ Where am I?

JVM Stack
→ What method am I executing + its execution data?

Heap
→ Which Java objects exist?

Method Area
→ What class-level information exists?

Native Method Stack
→ Native method execution support
```

---

## 🔥 JVM Runtime Data Areas

```text
                    JVM Runtime Data Areas
                             │
             ┌───────────────┴───────────────┐
             │                               │
        Per Thread                         Shared
             │                               │
     ┌───────┼────────┐                ┌─────┴─────┐
     │       │        │                │           │
     ▼       ▼        ▼                ▼           ▼
    PC     JVM     Native           Heap       Method
 Register  Stack   Method Stack                 Area
```

### Per-Thread

```text
PC Register
JVM Stack
Native Method Stack
```

### Shared

```text
Heap
Method Area
```

---

## 🔥 PC Register vs JVM Stack

```text
PC Register
    ↓
Current instruction position

JVM Stack
    ↓
Current method execution data
    ↓
Stack Frames
    ↓
Local Variables
Operand Stack
Frame Data
```

---

## 🔥 Execution Flow

```text
Java Source
     ↓
    javac
     ↓
  Bytecode
     ↓
    JVM
     ↓
Current Thread
     │
     ├── PC Register
     │
     └── JVM Stack
             │
             └── Current Stack Frame
                      ↓
                Execute JVM Instructions
```

---

## 🔥 Multithreading

```text
Thread 1
├── PC₁
└── Stack₁

Thread 2
├── PC₂
└── Stack₂

Thread 3
├── PC₃
└── Stack₃
```

Each thread has independent execution state.

---

## 🔥 Native Method Special Case

```text
Java Method
     ↓
PC Register
     ↓
JVM instruction position


Native Method
     ↓
Native execution
     ↓
PC Register value
     ↓
Undefined according to JVM specification
```

---

## ⭐ Final Interview Rule

```text
PC Register
→ One per JVM thread
→ Tracks Java instruction execution position
→ Part of Runtime Data Areas
→ Not the Java Heap
→ Not the JVM Stack
→ Not source-code line number
→ Not necessarily a physical CPU register
→ Native method → PC value undefined
```

## 🚀 One-Line Revision

```text
PC Register = Thread-private JVM execution pointer that tracks where a Java thread is in its JVM instruction execution.
```