# 01 — Process and Thread

> **A process is an independent program in execution, while a thread is the smallest unit of execution within a process.**

---

## 📌 Table of Contents

1. [Introduction](#-introduction)
2. [Why Processes and Threads Matter](#-why-processes-and-threads-matter)
3. [What is a Process?](#-what-is-a-process)
4. [Process Characteristics](#-process-characteristics)
5. [Process Memory](#-process-memory)
6. [What is a Thread?](#-what-is-a-thread)
7. [Thread Characteristics](#-thread-characteristics)
8. [Process vs Thread](#-process-vs-thread)
9. [Relationship Between Process and Thread](#-relationship-between-process-and-thread)
10. [Single-Threaded Process](#-single-threaded-process)
11. [Multithreaded Process](#-multithreaded-process)
12. [Why Multithreading?](#-why-multithreading)
13. [Concurrency vs Parallelism](#-concurrency-vs-parallelism)
14. [Thread Memory](#-thread-memory)
15. [Java and Threads](#-java-and-threads)
16. [Simple Java Example](#-simple-java-example)
17. [Internal Working](#-internal-working)
18. [JVM Perspective](#-jvm-perspective)
19. [Real-World Examples](#-real-world-examples)
20. [Advantages](#-advantages)
21. [Disadvantages](#-disadvantages)
22. [Common Mistakes](#-common-mistakes)
23. [Interview Traps](#-interview-traps)
24. [DSA / Problem-Solving Relevance](#-dsa--problem-solving-relevance)
25. [30-Second Interview Answer](#-30-second-interview-answer)
26. [Cheat Sheet](#-cheat-sheet)
27. [Top 10 Interview Questions](#-top-10-interview-questions)

---

# 🔹 Introduction

When a program runs, the operating system needs to manage its execution.

Two fundamental concepts involved in program execution are:

- **Process**
- **Thread**

A **process** represents a program that is currently executing.

A **thread** represents an individual path of execution inside a process.

### Basic relationship

```text
Program
   ↓
Process
   ↓
One or more Threads
```

A process can contain one or multiple threads.

```text
                 Process
                    │
        ┌───────────┼───────────┐
        ↓           ↓           ↓
     Thread 1    Thread 2    Thread 3
```

---

# 🔹 Why Processes and Threads Matter

Understanding processes and threads is fundamental to:

- Multithreading
- Concurrency
- Backend development
- Web servers
- Operating systems
- Database systems
- Asynchronous programming
- Performance optimization
- Executor Framework
- `CompletableFuture`

For example, a web server may receive multiple requests:

```text
Client 1 ─────→ Thread 1
Client 2 ─────→ Thread 2
Client 3 ─────→ Thread 3
Client 4 ─────→ Thread 4
                    ↓
                  Server
```

Threads allow multiple tasks to make progress within the same process.

---

# 🔹 What is a Process?

A **process** is a program that is currently being executed by the operating system.

A program stored on disk is passive.

When the operating system starts executing that program, it becomes a process.

### Example

```text
Program stored on disk
        ↓
      Execute
        ↓
     Process
```

### Definition

> **A process is an independent execution environment containing the resources required to execute a program.**

---

# 🔹 Process Characteristics

A process generally has its own:

- Address space
- Memory
- Resources
- Program counter
- Registers
- File/resource handles
- One or more threads

Simplified structure:

```text
Process
│
├── Code
├── Data
├── Heap
├── Resources
└── Threads
```

### Important point

Each process normally has an **isolated virtual address space**.

For example:

```text
Process A                    Process B

┌─────────────┐              ┌─────────────┐
│ Code        │              │ Code        │
│ Data        │              │ Data        │
│ Heap        │              │ Heap        │
│ Threads     │              │ Threads     │
└─────────────┘              └─────────────┘
```

Process A normally cannot directly access Process B's memory.

Communication between processes requires operating-system mechanisms such as:

- Pipes
- Sockets
- Shared memory
- Message queues

---

# 🔹 What is a Thread?

A **thread** is the smallest unit of execution within a process.

A process can contain:

- One thread
- Multiple threads

### Definition

> **A thread is an independent path of execution within a process.**

Example:

```text
Process
│
├── Thread 1
├── Thread 2
└── Thread 3
```

All these threads belong to the same process.

---

# 🔹 Thread Characteristics

Threads within the same process generally share:

- Code
- Heap memory
- Static data
- Process resources

But each thread has its own:

- Stack
- Program counter
- Registers
- Execution state

### Simplified structure

```text
                    Process
                       │
        ┌──────────────┼──────────────┐
        │              │              │
        ↓              ↓              ↓
     Thread 1       Thread 2       Thread 3
        │              │              │
     Stack 1        Stack 2        Stack 3
```

The threads share the process's common resources but maintain their own execution context.

---

# 🔹 Process vs Thread

| Feature | Process | Thread |
|---|---|---|
| Definition | Program in execution | Execution unit within a process |
| Ownership | Independent | Belongs to a process |
| Memory | Has its own address space | Shares process memory |
| Heap | Separate between processes | Shared with threads of same process |
| Stack | Process contains thread stacks | Each thread has its own stack |
| Communication | More expensive | Easier because memory is shared |
| Creation | Relatively expensive | Relatively lightweight |
| Context switching | Generally more expensive | Generally cheaper |
| Failure | Process isolation can limit impact | Thread failure can affect the process |
| Number | Usually fewer | A process can contain many threads |

### Memory comparison

```text
PROCESS

Process A
┌──────────────────────────┐
│ Own Address Space        │
│                          │
│ Code                     │
│ Data                     │
│ Heap                     │
│                          │
│ Thread 1 → Stack         │
│ Thread 2 → Stack         │
│ Thread 3 → Stack         │
└──────────────────────────┘
```

Threads inside the same process share the process's address space.

---

# 🔹 Relationship Between Process and Thread

A process is a **container for resources and threads**.

A thread is an **execution path inside that process**.

Think of it like:

```text
Process = House
Threads = People working inside the house
```

The house contains shared resources.

The people perform individual tasks.

Another analogy:

```text
Process
   ↓
Restaurant
   ↓
Workers = Threads
```

Multiple workers can work simultaneously within the same restaurant.

---

# 🔹 Single-Threaded Process

A process containing only one thread is called a **single-threaded process**.

```text
Process
   │
   └── Thread 1
```

The thread executes tasks sequentially.

For example:

```text
Task A
  ↓
Task B
  ↓
Task C
```

Task B starts after Task A progresses/completes according to the program's execution flow.

---

# 🔹 Multithreaded Process

A process containing multiple threads is called a **multithreaded process**.

```text
                 Process
                    │
       ┌────────────┼────────────┐
       ↓            ↓            ↓
    Thread 1     Thread 2     Thread 3
       │            │            │
     Task A       Task B       Task C
```

Different threads can make progress on different tasks.

For example, a web server might handle multiple requests concurrently.

```text
Request 1 → Thread 1
Request 2 → Thread 2
Request 3 → Thread 3
Request 4 → Thread 4
```

---

# 🔹 Why Multithreading?

Multithreading can improve application responsiveness and resource utilization.

### Common reasons

#### 1. Responsiveness

One thread can perform a long-running operation while another remains responsive.

#### 2. Concurrent task execution

Multiple independent tasks can make progress without requiring one task to finish before another starts.

#### 3. Better CPU utilization

On a multicore processor, multiple threads can potentially execute simultaneously.

#### 4. I/O handling

While one thread waits for I/O, another thread may perform useful work.

Examples of I/O:

- Network requests
- File operations
- Database operations

---

# 🔹 Concurrency vs Parallelism

These terms are related but not identical.

## Concurrency

Concurrency means multiple tasks are **in progress during the same period**, even if they are not literally executing at the exact same instant.

Example on a single CPU:

```text
Time →
──────────────────────────────>

Task A ────┐      ┌──────
            │      │
Task B      └──────┘
```

The CPU can switch between tasks.

---

## Parallelism

Parallelism means multiple tasks are **actually executing at the same time**, typically on multiple CPU cores.

```text
Core 1 → Task A ──────────────

Core 2 → Task B ──────────────
```

### Simple distinction

> **Concurrency = dealing with multiple tasks at once.**

> **Parallelism = executing multiple tasks at the same time.**

---

# 🔹 Thread Memory

Each thread has some execution-specific memory.

The most important part is the **thread stack**.

### Simplified model

```text
Process
│
├── Shared Heap
│
├── Shared Code
│
├── Shared Data
│
├── Thread 1
│    └── Stack 1
│
├── Thread 2
│    └── Stack 2
│
└── Thread 3
     └── Stack 3
```

### Shared

Threads within the same process generally share:

- Heap
- Code
- Static variables
- Process resources

### Private

Each thread has its own:

- Stack
- Program counter
- Registers
- Execution state

This distinction becomes extremely important when studying:

- Race conditions
- Synchronization
- `volatile`
- Atomic classes
- Deadlocks

---

# 🔹 Java and Threads

Java provides built-in support for multithreading.

The primary class is:

```java
Thread
```

Java also provides the `Runnable` interface.

A basic example:

```java
class MyTask implements Runnable {

    @Override
    public void run() {
        System.out.println("Task is running...");
    }
}
```

A thread can execute the task:

```java
class Main {
    public static void main(String[] args) {

        Runnable task = new MyTask();

        Thread thread = new Thread(task);

        thread.start();
    }
}
```

### Important

Calling:

```java
thread.start();
```

starts a new thread of execution.

Calling:

```java
thread.run();
```

does **not** start a new thread by itself.

It simply invokes the `run()` method like a normal method call.

This distinction becomes important in the next topics.

---

# 🔹 Simple Java Example

```java
class Main {

    public static void main(String[] args) {

        Thread thread = new Thread(() -> {
            System.out.println("Worker thread is running");
        });

        thread.start();

        System.out.println("Main thread is running");
    }
}
```

Possible output:

```text
Main thread is running
Worker thread is running
```

or:

```text
Worker thread is running
Main thread is running
```

The exact order is not guaranteed because thread scheduling is controlled by the runtime and operating system.

---

# 🔹 The Main Thread

When a Java application starts, the JVM creates a thread that begins execution from:

```java
public static void main(String[] args)
```

This is commonly called the **main thread**.

Example:

```java
class Main {

    public static void main(String[] args) {

        Thread current = Thread.currentThread();

        System.out.println(current.getName());
    }
}
```

Typical output:

```text
main
```

The main thread itself is a thread.

```text
JVM Process
    │
    └── main thread
```

If you create additional threads:

```text
JVM Process
│
├── main thread
├── worker thread
└── worker thread
```

---

# 🔹 Internal Working

When a Java application starts, the operating system creates a process for the JVM/application environment.

Inside that process, threads execute Java code.

Simplified:

```text
Java Application
       ↓
      JVM
       ↓
    Process
       ↓
 ┌─────┼─────┐
 ↓     ↓     ↓
Main  T1     T2
```

When a thread needs CPU time, the operating system's scheduler determines when it gets CPU execution.

A simplified flow:

```text
Thread Ready
     ↓
Scheduler
     ↓
CPU
     ↓
Executing
     ↓
Waiting / Ready / Finished
```

The exact scheduling behavior depends on the operating system, JVM implementation, and runtime conditions.

---

# 🔹 Context Switching

When the CPU switches execution from one thread to another, the execution state of the current thread must be preserved and the state of another thread restored.

This is called a **context switch**.

Simplified:

```text
Thread A executing
       ↓
Save A's execution state
       ↓
Load B's execution state
       ↓
Thread B executing
```

Context switching has overhead.

Therefore:

> Creating more threads does not automatically make a program faster.

Too many threads can increase scheduling and memory overhead.

---

# 🔹 Process Context Switching vs Thread Context Switching

### Process switching

Switching between processes generally requires switching between separate address spaces and process-level resources.

```text
Process A
   ↓
Save context
   ↓
Load Process B
   ↓
Process B
```

### Thread switching

Threads belonging to the same process share many resources.

```text
Thread A
   ↓
Save thread context
   ↓
Load Thread B context
   ↓
Thread B
```

Thread switching is generally cheaper than switching between independent processes, although the exact cost depends on the operating system and hardware.

---

# 🔹 JVM Perspective

The JVM executes Java bytecode and manages Java threads.

When a Java thread is created, the JVM creates/manages the corresponding Java thread execution environment, with the underlying operating system involved in scheduling native execution.

Conceptually:

```text
Java Thread
     ↓
JVM Thread Management
     ↓
Operating System
     ↓
CPU
```

Modern JVM implementations use operating-system threads for traditional Java platform threads.

### Important distinction

Do not think:

> "JVM itself is the operating system."

The JVM runs **inside a process managed by the operating system**.

The operating system manages:

- Processes
- CPU scheduling
- System resources
- Native threads
- Memory protection

The JVM manages Java-level runtime responsibilities such as:

- Java threads
- Heap
- Garbage collection
- Class loading
- Bytecode execution
- JIT compilation

---

# 🔹 Real-World Examples

## 1. Web Server

A server may receive many requests:

```text
Request 1 → Worker Thread 1
Request 2 → Worker Thread 2
Request 3 → Worker Thread 3
Request 4 → Worker Thread 4
```

---

## 2. Music Application

A music application might perform:

```text
Thread 1 → Play audio
Thread 2 → Download song
Thread 3 → Update UI
Thread 4 → Handle user input
```

---

## 3. IDE

An IDE can perform multiple activities:

```text
Thread → Code analysis
Thread → File indexing
Thread → UI operations
Thread → Background compilation
```

---

## 4. Backend Application

A backend service may handle:

```text
Client Request
      ↓
Server
      ↓
Worker Thread
      ↓
Database / Service
      ↓
Response
```

Modern Java applications often use thread pools rather than creating a completely new thread for every task.

---

# 🔹 Advantages

## Process Advantages

- Strong isolation between applications
- Better fault isolation
- Separate address spaces
- Independent resource management

## Thread Advantages

- Lightweight compared with processes
- Faster communication within the same process
- Shared memory allows efficient data exchange
- Useful for concurrent tasks
- Can improve responsiveness
- Can utilize multiple CPU cores

---

# 🔹 Disadvantages

## Process Disadvantages

- Higher creation overhead
- Higher memory usage
- Inter-process communication can be expensive
- Context switching can be expensive

## Thread Disadvantages

- Shared memory can create synchronization problems
- Race conditions can occur
- Deadlocks can occur
- Debugging can become difficult
- Too many threads can cause overhead
- Incorrect synchronization can reduce performance

---

# 🔹 Common Mistakes

## Mistake 1: Thinking Process and Thread are the same

They are not.

```text
Process
   ↓
contains
   ↓
Threads
```

---

## Mistake 2: Thinking every thread has its own heap

Threads belonging to the same process generally share the process heap.

They have separate stacks.

```text
Shared:
    Heap

Private:
    Thread Stack
```

---

## Mistake 3: Thinking multiple threads always mean parallel execution

Multiple threads can provide concurrency without actual parallel execution.

Parallel execution depends on available CPU cores and scheduling.

---

## Mistake 4: Thinking `run()` creates a new thread

It doesn't.

```java
thread.run();
```

is a normal method invocation.

Whereas:

```java
thread.start();
```

starts the thread.

---

## Mistake 5: Thinking more threads always mean better performance

More threads can also mean:

- More memory usage
- More context switching
- More synchronization
- More contention

Therefore, thread count should be appropriate for the workload.

---

# 🔹 Interview Traps

### Trap 1

**Question:** Does every process have a thread?

**Answer:** A process needs at least one thread of execution to execute program instructions. In practical terms, a running process contains one or more threads.

---

### Trap 2

**Question:** Do threads have separate memory?

**Answer:** Threads have their own execution-specific memory such as stacks and registers, but threads within the same process generally share the process's heap and other resources.

---

### Trap 3

**Question:** Is a thread a process?

**Answer:** No. A thread is an execution unit within a process.

---

### Trap 4

**Question:** Is multithreading the same as parallelism?

**Answer:** No.

Multithreading can provide concurrency. Parallelism specifically means simultaneous execution, typically on multiple CPU cores.

---

### Trap 5

**Question:** Can two threads access the same object?

Yes.

If the object is accessible to both threads, they can access the same object because threads within the same process generally share heap memory.

This is also why synchronization becomes important.

---

# 🔹 DSA / Problem-Solving Relevance

Processes and threads are **not primarily DSA patterns**, but they are important for understanding concurrent algorithms and systems.

Relevant concepts include:

- Thread-safe data structures
- Concurrent queues
- Producer-consumer problems
- Synchronization
- Race conditions
- Deadlocks
- Atomic operations
- Concurrent programming

### Important interview problems

1. Producer-Consumer
2. Dining Philosophers
3. Readers-Writers
4. Thread-safe counter
5. Print numbers using multiple threads
6. Alternate printing using two threads

These become more relevant when studying synchronization and concurrency.

---

# 🔹 Process vs Thread — Quick Comparison

```text
Process
│
├── Independent execution environment
├── Own address space
├── Own resources
└── Contains one or more threads


Thread
│
├── Execution unit
├── Exists inside a process
├── Shares process resources
├── Own stack
├── Own registers
└── Own execution state
```

### Memory trick

> **Process = Resource container**

> **Thread = Execution unit**

---

# 🔹 30-Second Interview Answer

> A process is an independent program in execution with its own address space and resources. A thread is the smallest unit of execution inside a process. Multiple threads within the same process share resources such as heap memory and code, while each thread maintains its own stack and execution state. Threads are lighter than processes and are commonly used to achieve concurrency and, when hardware allows, parallel execution.

---

# 🔹 Cheat Sheet

| Concept | Remember |
|---|---|
| Process | Program in execution |
| Thread | Execution unit inside process |
| Process memory | Normally isolated from other processes |
| Thread memory | Shares process memory |
| Thread stack | Private to each thread |
| Heap | Shared among threads of same process |
| Process communication | IPC |
| Thread communication | Shared memory |
| Context switching | Switching execution between tasks |
| Concurrency | Multiple tasks making progress |
| Parallelism | Multiple tasks executing simultaneously |
| Main thread | Thread executing `main()` |
| `start()` | Starts a new thread |
| `run()` | Normal method invocation when called directly |
| Too many threads | Can increase overhead |
| Multithreading | Multiple threads in a process |

---

# 🔥 Top 10 Interview Questions

## 1. What is a process?

A process is an independent program in execution with its own address space and resources.

---

## 2. What is a thread?

A thread is the smallest unit of execution within a process.

---

## 3. What is the difference between a process and a thread?

A process is an independent execution environment, while a thread is an execution unit inside a process. Processes have separate address spaces, while threads within the same process generally share memory and resources.

---

## 4. Do threads share memory?

Threads of the same process generally share:

- Heap
- Code
- Static data
- Process resources

But each thread has its own stack and execution state.

---

## 5. Why are threads considered lightweight?

Threads share many resources with their process, so creating and switching between threads generally requires less overhead than creating and switching between independent processes.

---

## 6. What is concurrency?

Concurrency means multiple tasks are making progress during overlapping periods of time.

---

## 7. What is parallelism?

Parallelism means multiple tasks are actually executing simultaneously, usually on multiple CPU cores.

---

## 8. Does calling `run()` create a new thread?

No.

Calling:

```java
thread.run();
```

directly invokes the method on the current thread.

Calling:

```java
thread.start();
```

starts a new thread of execution.

---

## 9. Can multiple threads access the same object?

Yes.

Threads within the same process generally share heap memory, so they can access the same object if they have a reference to it.

However, shared mutable state can lead to race conditions and therefore may require synchronization.

---

## 10. Does more threads always mean better performance?

No.

Too many threads can increase:

- Context switching
- Memory usage
- Scheduling overhead
- Lock contention
- Synchronization overhead

The appropriate number of threads depends on the workload and hardware.

---

# 🧠 Final Memory Trick

```text
PROCESS
= Resource Container

THREAD
= Execution Unit

PROCESS
    ↓
contains
    ↓
THREADS

Threads share:
    → Heap
    → Code
    → Process resources

Threads have their own:
    → Stack
    → Registers
    → Program Counter
    → Execution State
```

> **Process gives you the resources. Thread gives you the execution.** ⚡

---

## 🚀 Next Topic

**02 — Thread Lifecycle**

You will learn how a Java thread moves through states such as:

```text
NEW
 ↓
RUNNABLE
 ↓
RUNNING
 ↓
WAITING / BLOCKED / TIMED_WAITING
 ↓
TERMINATED
```