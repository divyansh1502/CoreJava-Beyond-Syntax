# 02 — Thread Lifecycle

> **The thread lifecycle describes the different states a thread passes through from creation until its execution finishes.**

---

## 📌 Table of Contents

1. [Introduction](#-introduction)
2. [Why Thread Lifecycle Matters](#-why-thread-lifecycle-matters)
3. [Thread Lifecycle Overview](#-thread-lifecycle-overview)
4. [Java Thread States](#-java-thread-states)
5. [NEW](#-new)
6. [RUNNABLE](#-runnable)
7. [BLOCKED](#-blocked)
8. [WAITING](#-waiting)
9. [TIMED_WAITING](#-timed_waiting)
10. [TERMINATED](#-terminated)
11. [State Transition Flow](#-state-transition-flow)
12. [How `start()` Changes the State](#-how-start-changes-the-state)
13. [RUNNABLE vs RUNNING](#-runnable-vs-running)
14. [BLOCKED vs WAITING](#-blocked-vs-waiting)
15. [WAITING vs TIMED_WAITING](#-waiting-vs-timed_waiting)
16. [Getting the Current State](#-getting-the-current-state)
17. [Java Example](#-java-example)
18. [Observing Thread States](#-observing-thread-states)
19. [Internal Working](#-internal-working)
20. [JVM Perspective](#-jvm-perspective)
21. [Important State Transitions](#-important-state-transitions)
22. [Can a Thread Go Back to NEW?](#-can-a-thread-go-back-to-new)
23. [Can a Terminated Thread Restart?](#-can-a-terminated-thread-restart)
24. [Common Mistakes](#-common-mistakes)
25. [Interview Traps](#-interview-traps)
26. [DSA / Problem-Solving Relevance](#-dsa--problem-solving-relevance)
27. [30-Second Interview Answer](#-30-second-interview-answer)
28. [Cheat Sheet](#-cheat-sheet)
29. [Top 10 Interview Questions](#-top-10-interview-questions)

---

# 🔹 Introduction

A thread does not remain in one state throughout its lifetime.

From the moment a thread object is created until its execution finishes, it can move through different states.

Conceptually:

    Thread Created
          ↓
        NEW
          ↓
      RUNNABLE
          ↓
    ┌─────┼────────────────┐
    ↓     ↓                ↓
 BLOCKED WAITING    TIMED_WAITING
    │     │                │
    └─────┴────────────────┘
              ↓
          RUNNABLE
              ↓
         TERMINATED

Java officially defines **six thread states**:

1. `NEW`
2. `RUNNABLE`
3. `BLOCKED`
4. `WAITING`
5. `TIMED_WAITING`
6. `TERMINATED`

These states are represented by the nested enum:

```java
Thread.State
```

---

# 🔹 Why Thread Lifecycle Matters

Understanding the thread lifecycle is important for:

- Multithreading
- Synchronization
- Debugging
- Thread scheduling
- `sleep()`
- `wait()`
- `join()`
- Locks
- Deadlocks
- Executor Framework
- Backend development

For example, if a thread appears to be stuck, checking its state can help determine what it is currently doing.

```java
Thread.State state = thread.getState();

System.out.println(state);
```

Possible output:

```text
NEW
RUNNABLE
BLOCKED
WAITING
TIMED_WAITING
TERMINATED
```

---

# 🔹 Thread Lifecycle Overview

A simplified lifecycle looks like:

    NEW
     │
     │ start()
     ↓
    RUNNABLE
     │
     ├──────────────→ BLOCKED
     │                   │
     │                   ↓
     │               RUNNABLE
     │
     ├──────────────→ WAITING
     │                   │
     │                   ↓
     │               RUNNABLE
     │
     ├──────────────→ TIMED_WAITING
     │                   │
     │                   ↓
     │               RUNNABLE
     │
     ↓
    TERMINATED

### Important clarification

Java does **not** define a separate `RUNNING` state.

A thread that is actually executing or is eligible to execute is represented by the Java state:

```text
RUNNABLE
```

---

# 🔹 Java Thread States

Java defines the thread states using:

```java
Thread.State
```

The six constants are:

```java
Thread.State.NEW
Thread.State.RUNNABLE
Thread.State.BLOCKED
Thread.State.WAITING
Thread.State.TIMED_WAITING
Thread.State.TERMINATED
```

You can obtain the current state of a thread using:

```java
thread.getState();
```

---

# 🔹 1. NEW

A thread is in the `NEW` state when:

> A `Thread` object has been created but the thread has not yet started.

Example:

```java
Thread thread = new Thread();

System.out.println(thread.getState());
```

Output:

```text
NEW
```

Lifecycle:

    Thread object created
            ↓
           NEW

At this point:

- The `Thread` object exists.
- The thread has not started.
- Its `run()` method has not begun executing.

### Important

Creating a thread object does **not** start the thread.

```java
Thread thread = new Thread();
```

This only creates the object.

To start it:

```java
thread.start();
```

---

# 🔹 2. RUNNABLE

After calling:

```java
thread.start();
```

the thread enters the `RUNNABLE` state.

Conceptually:

    NEW
     │
     │ start()
     ↓
    RUNNABLE

Example:

```java
Thread thread = new Thread(() -> {
    System.out.println("Worker thread is running");
});

thread.start();
```

### What does RUNNABLE mean?

In Java, `RUNNABLE` includes both:

- A thread that is ready to run
- A thread that is currently running

Java does not expose a separate `RUNNING` state through `Thread.State`.

The operating system scheduler determines when an eligible thread gets CPU time.

### Important

Do not interpret:

```text
RUNNABLE
```

as:

> "The thread is definitely executing on the CPU right now."

It can mean that the thread is eligible to execute.

---

# 🔹 3. BLOCKED

A thread enters the `BLOCKED` state when it is waiting to acquire a **monitor lock** so that it can enter a `synchronized` block or method.

Example:

```java
class SharedResource {

    synchronized void work() {
        System.out.println("Working...");
    }
}
```

Suppose Thread A already owns the monitor lock:

```text
Thread A
   ↓
owns lock
   ↓
synchronized method
```

If Thread B tries to enter the same synchronized method:

```text
Thread B
   ↓
tries to acquire lock
   ↓
lock unavailable
   ↓
BLOCKED
```

### Example

```java
class Main {

    static final Object lock = new Object();

    public static void main(String[] args) {

        Thread t1 = new Thread(() -> {
            synchronized (lock) {
                try {
                    Thread.sleep(3000);
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
            }
        });

        Thread t2 = new Thread(() -> {
            synchronized (lock) {
                System.out.println("Thread 2 acquired the lock");
            }
        });

        t1.start();
        t2.start();
    }
}
```

While `t1` owns the lock, `t2` may become:

```text
BLOCKED
```

because it is waiting to acquire the same monitor.

### Important

`BLOCKED` specifically relates to waiting for a **monitor lock**.

---

# 🔹 4. WAITING

A thread is in the `WAITING` state when it is waiting indefinitely for another thread to perform some action.

Common methods that can cause `WAITING` include:

- `Object.wait()`
- `Thread.join()`
- `LockSupport.park()`

Example:

```java
class Main {

    public static void main(String[] args) throws InterruptedException {

        Thread worker = new Thread(() -> {
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        });

        worker.start();

        worker.join();
    }
}
```

When the main thread calls:

```java
worker.join();
```

the main thread waits until `worker` terminates.

The main thread can therefore enter:

```text
WAITING
```

### Important

`WAITING` generally means:

> "Wait until another thread performs the required action."

There is no timeout associated with ordinary `WAITING`.

---

# 🔹 5. TIMED_WAITING

A thread enters `TIMED_WAITING` when it waits for a specified maximum amount of time.

Common methods include:

- `Thread.sleep(...)`
- `Object.wait(timeout)`
- `Thread.join(timeout)`
- `LockSupport.parkNanos(...)`
- `LockSupport.parkUntil(...)`

### Example using `sleep()`

```java
class Main {

    public static void main(String[] args) {

        Thread thread = new Thread(() -> {

            try {
                Thread.sleep(3000);
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }

        });

        thread.start();
    }
}
```

During the sleep period, the thread is generally in:

```text
TIMED_WAITING
```

### Important

The key difference is:

```text
WAITING
        → waits indefinitely

TIMED_WAITING
        → waits for a specified time
```

---

# 🔹 6. TERMINATED

A thread enters the `TERMINATED` state when its execution has completed.

Example:

```java
class Main {

    public static void main(String[] args) throws InterruptedException {

        Thread thread = new Thread(() -> {
            System.out.println("Task completed");
        });

        thread.start();

        thread.join();

        System.out.println(thread.getState());
    }
}
```

Output:

```text
Task completed
TERMINATED
```

Once the `run()` method finishes:

```text
RUNNABLE
    ↓
TERMINATED
```

A thread cannot return from `TERMINATED` to `RUNNABLE`.

---

# 🔹 State Transition Flow

The important transitions are:

    NEW
     │
     │ start()
     ↓
    RUNNABLE
     │
     ├── waiting for monitor lock ──→ BLOCKED
     │                                  │
     │                                  │ lock acquired
     │                                  ↓
     │                              RUNNABLE
     │
     ├── wait()/join()/park() ─────→ WAITING
     │                                  │
     │                                  │ notification/completion
     │                                  ↓
     │                              RUNNABLE
     │
     ├── sleep()/timed wait/join(t) → TIMED_WAITING
     │                                  │
     │                                  │ timeout/completion
     │                                  ↓
     │                              RUNNABLE
     │
     │
     └── execution completes ─────→ TERMINATED

---

# 🔹 How `start()` Changes the State

When a thread is created:

```java
Thread thread = new Thread();
```

its state is:

```text
NEW
```

When:

```java
thread.start();
```

is called:

```text
NEW
 ↓
RUNNABLE
```

Example:

```java
Thread thread = new Thread(() -> {
    System.out.println("Hello");
});

System.out.println(thread.getState());

thread.start();

System.out.println(thread.getState());
```

Possible output:

```text
NEW
RUNNABLE
```

However, the second state observation can vary depending on timing because the thread may finish very quickly.

---

# 🔹 RUNNABLE vs RUNNING

This is one of the most important interview points.

Many diagrams show:

```text
RUNNABLE → RUNNING
```

But Java's official `Thread.State` does not have a `RUNNING` state.

Java defines:

```text
RUNNABLE
```

and this state includes:

- Ready to run
- Actually running

The operating system scheduler decides which runnable thread gets CPU time.

### Interview answer

> Java's `Thread.State` does not define a separate `RUNNING` state. A thread that is ready to run or currently executing is represented by `RUNNABLE`.

---

# 🔹 BLOCKED vs WAITING

These states are often confused.

## BLOCKED

A thread is waiting to acquire a monitor lock.

Example:

```java
synchronized (lock) {
    // critical section
}
```

If another thread already owns the lock:

```text
Thread A → owns lock

Thread B → waiting for lock
         → BLOCKED
```

---

## WAITING

A thread is waiting for another thread to perform an action.

Examples:

```java
thread.join();
```

or:

```java
object.wait();
```

Conceptually:

```text
BLOCKED
→ waiting for a monitor lock

WAITING
→ waiting for another thread/action
```

---

# 🔹 WAITING vs TIMED_WAITING

The main difference is whether a timeout exists.

### WAITING

```java
thread.join();
```

The thread waits until the target thread finishes.

### TIMED_WAITING

```java
thread.join(2000);
```

The thread waits for at most the specified duration.

Another example:

```java
Thread.sleep(2000);
```

This puts the current thread into `TIMED_WAITING` during the sleep period.

### Memory Trick

```text
WAITING
    = No specified timeout

TIMED_WAITING
    = Timeout exists
```

---

# 🔹 Getting the Current State

Java provides:

```java
getState()
```

through the `Thread` class.

Example:

```java
class Main {

    public static void main(String[] args) {

        Thread thread = new Thread(() -> {
            System.out.println("Running...");
        });

        System.out.println(thread.getState());

        thread.start();

        System.out.println(thread.getState());
    }
}
```

The first output will be:

```text
NEW
```

The second output depends on timing because the thread may execute and terminate very quickly.

---

# 🔹 `Thread.State` is an Enum

`Thread.State` is an enum defined inside the `Thread` class.

You can use it like:

```java
Thread.State state = Thread.State.NEW;

System.out.println(state);
```

Output:

```text
NEW
```

You can also compare states:

```java
if (thread.getState() == Thread.State.RUNNABLE) {
    System.out.println("Thread is runnable");
}
```

---

# 🔹 Observing Thread States

A longer-running thread makes it easier to observe its state.

Example:

```java
class Main {

    public static void main(String[] args) throws InterruptedException {

        Thread thread = new Thread(() -> {

            try {
                Thread.sleep(3000);
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }

        });

        System.out.println("Before start: " + thread.getState());

        thread.start();

        Thread.sleep(100);

        System.out.println("During sleep: " + thread.getState());

        thread.join();

        System.out.println("After completion: " + thread.getState());
    }
}
```

Possible output:

```text
Before start: NEW
During sleep: TIMED_WAITING
After completion: TERMINATED
```

---

# 🔹 Internal Working

A simplified execution flow is:

    1. Create Thread object
            ↓
          NEW
            ↓
    2. Call start()
            ↓
        RUNNABLE
            ↓
    3. Scheduler gives CPU
            ↓
        Execution
            ↓
    4. Thread may temporarily wait
            ↓
    BLOCKED / WAITING / TIMED_WAITING
            ↓
        RUNNABLE
            ↓
    5. run() finishes
            ↓
       TERMINATED

The actual scheduling behavior is controlled by the operating system and JVM implementation.

---

# 🔹 JVM Perspective

Java exposes the lifecycle through:

```java
Thread.State
```

The JVM manages Java threads and interacts with the underlying operating system.

Conceptually:

    Java Thread
         ↓
        JVM
         ↓
    OS Thread Scheduling
         ↓
        CPU

The JVM provides Java-level thread APIs, while the operating system is responsible for actual CPU scheduling of platform threads.

---

# 🔹 Important State Transitions

## NEW → RUNNABLE

Caused by:

```java
thread.start();
```

---

## RUNNABLE → BLOCKED

Can occur when a thread tries to enter a synchronized region while another thread owns the monitor.

```java
synchronized (lock) {
    // critical section
}
```

---

## RUNNABLE → WAITING

Can occur through operations such as:

```java
thread.join();
```

or:

```java
object.wait();
```

or:

```java
LockSupport.park();
```

---

## RUNNABLE → TIMED_WAITING

Can occur through:

```java
Thread.sleep(1000);
```

or:

```java
thread.join(1000);
```

or:

```java
object.wait(1000);
```

---

## WAITING → RUNNABLE

The condition causing the wait is satisfied.

For example, a thread waiting through `join()` can become runnable when the target thread terminates.

---

## BLOCKED → RUNNABLE

The thread acquires the monitor lock it was waiting for.

---

## TIMED_WAITING → RUNNABLE

The timeout expires or the waiting condition is otherwise completed.

---

## RUNNABLE → TERMINATED

The thread's execution completes.

```text
run()
 ↓
returns
 ↓
TERMINATED
```

---

# 🔹 Can a Thread Go Back to NEW?

No.

Once a thread has been started, it cannot return to:

```text
NEW
```

The lifecycle moves forward from creation toward execution and termination.

---

# 🔹 Can a Terminated Thread Restart?

No.

A thread can only be started once.

Example:

```java
Thread thread = new Thread(() -> {
    System.out.println("Hello");
});

thread.start();
```

After the thread terminates:

```java
thread.start();
```

again is illegal and results in:

```text
IllegalThreadStateException
```

### Important

You cannot restart the same `Thread` object.

If you need another execution, create a new `Thread` object.

---

# 🔹 Common Mistakes

## ❌ Mistake 1: Thinking `NEW` means the thread is ready to execute

No.

`NEW` means the `Thread` object exists but `start()` has not been called.

```text
NEW
= Not started yet
```

---

## ❌ Mistake 2: Thinking Java has a `RUNNING` state

Java's official `Thread.State` does not contain:

```text
RUNNING
```

It contains:

```text
RUNNABLE
```

---

## ❌ Mistake 3: Thinking `sleep()` causes `WAITING`

No.

```java
Thread.sleep(1000);
```

causes:

```text
TIMED_WAITING
```

---

## ❌ Mistake 4: Thinking `sleep()` releases a monitor lock

`sleep()` does not release a monitor lock held by the sleeping thread.

This becomes very important when studying synchronization.

---

## ❌ Mistake 5: Thinking `BLOCKED` and `WAITING` are the same

They are not.

```text
BLOCKED
→ waiting to acquire a monitor lock

WAITING
→ waiting for another thread/action
```

---

## ❌ Mistake 6: Thinking `join()` always means TIMED_WAITING

It depends on which overload is used.

```java
thread.join();
```

can put the caller into:

```text
WAITING
```

while:

```java
thread.join(1000);
```

can put the caller into:

```text
TIMED_WAITING
```

---

# 🔹 Interview Traps

### Trap 1: How many states does Java define?

**Six.**

```text
NEW
RUNNABLE
BLOCKED
WAITING
TIMED_WAITING
TERMINATED
```

---

### Trap 2: Is RUNNING a Java thread state?

**No.**

Java's `Thread.State` has no separate `RUNNING` state.

---

### Trap 3: What state does a newly created thread have?

```text
NEW
```

until `start()` is called.

---

### Trap 4: What state can `Thread.sleep()` cause?

```text
TIMED_WAITING
```

---

### Trap 5: What state occurs when waiting for a monitor lock?

```text
BLOCKED
```

---

### Trap 6: What state can `join()` cause?

Without timeout:

```text
WAITING
```

With timeout:

```text
TIMED_WAITING
```

---

### Trap 7: Can a terminated thread be started again?

No.

Calling `start()` again on the same `Thread` object results in:

```text
IllegalThreadStateException
```

---

# 🔹 DSA / Problem-Solving Relevance

Thread lifecycle itself is not a traditional DSA pattern.

However, understanding it helps with concurrency-based problems.

Important problem types include:

- Producer-Consumer
- Thread-safe counter
- Alternate printing
- Ordered thread execution
- Readers-Writers
- Dining Philosophers

These problems rely on concepts such as:

```text
Thread states
     ↓
Synchronization
     ↓
Waiting
     ↓
Notification
     ↓
Coordination
```

The lifecycle becomes particularly important when reasoning about why a thread is:

- Waiting
- Blocked
- Sleeping
- Runnable
- Finished

---

# 🔹 30-Second Interview Answer

> A Java thread has six official states: `NEW`, `RUNNABLE`, `BLOCKED`, `WAITING`, `TIMED_WAITING`, and `TERMINATED`. A newly created thread is `NEW`. After `start()` it becomes `RUNNABLE`. It may become `BLOCKED` while waiting for a monitor lock, `WAITING` when waiting indefinitely for another thread or action, or `TIMED_WAITING` when waiting for a specified duration. After its execution completes, it becomes `TERMINATED`. Java does not define a separate `RUNNING` state.

---

# 🔹 Cheat Sheet

| State | Meaning | Common Cause |
|---|---|---|
| `NEW` | Created but not started | `new Thread()` |
| `RUNNABLE` | Ready/running | `start()` |
| `BLOCKED` | Waiting for monitor lock | `synchronized` |
| `WAITING` | Waiting indefinitely | `wait()`, `join()`, `park()` |
| `TIMED_WAITING` | Waiting for limited time | `sleep()`, timed `join()`, timed `wait()` |
| `TERMINATED` | Execution finished | `run()` completes |

---

## 🔥 State Transition Cheat Sheet

    NEW
     │
     │ start()
     ↓
    RUNNABLE
     │
     ├── lock unavailable ─────→ BLOCKED
     │                              │
     │                              ↓
     │                          RUNNABLE
     │
     ├── wait/join/park ────────→ WAITING
     │                              │
     │                              ↓
     │                          RUNNABLE
     │
     ├── sleep/timed wait ──────→ TIMED_WAITING
     │                              │
     │                              ↓
     │                          RUNNABLE
     │
     └── execution completes ───→ TERMINATED

---

# 🧠 Memory Trick

Remember the lifecycle as:

```text
N → R → B/W/T → R → T
```

Where:

- `N` = NEW
- `R` = RUNNABLE
- `B` = BLOCKED
- `W` = WAITING
- `T` = TIMED_WAITING / TERMINATED depending on context

Better memory:

> **Create → Run → Wait/Block → Run → Finish**

---

# 🔥 Top 10 Interview Questions

## 1. What are the six states of a Java thread?

```text
NEW
RUNNABLE
BLOCKED
WAITING
TIMED_WAITING
TERMINATED
```

---

## 2. What is the state of a thread immediately after creation?

```text
NEW
```

---

## 3. What happens after calling `start()`?

The thread transitions from:

```text
NEW → RUNNABLE
```

---

## 4. Does Java have a `RUNNING` state?

No.

Java's official `Thread.State` uses `RUNNABLE` for both ready-to-run and currently-running execution.

---

## 5. What causes the `BLOCKED` state?

A thread attempting to acquire a monitor lock that is currently owned by another thread.

---

## 6. What causes `WAITING`?

Common examples include:

```java
thread.join();
```

```java
object.wait();
```

```java
LockSupport.park();
```

---

## 7. What causes `TIMED_WAITING`?

Common examples include:

```java
Thread.sleep(1000);
```

```java
thread.join(1000);
```

```java
object.wait(1000);
```

---

## 8. What happens when `run()` finishes?

The thread enters:

```text
TERMINATED
```

---

## 9. Can a terminated thread be restarted?

No.

A `Thread` object can only be started once.

Calling `start()` again results in:

```text
IllegalThreadStateException
```

---

## 10. What is the difference between BLOCKED and WAITING?

`BLOCKED` means the thread is waiting to acquire a monitor lock.

`WAITING` means the thread is waiting indefinitely for another thread or action.

---

# 🎯 Final Summary

```text
Thread Lifecycle

        NEW
         │
      start()
         ↓
      RUNNABLE
       / | \
      /  |  \
     ↓   ↓   ↓
 BLOCKED WAITING TIMED_WAITING
     \    |    /
      \   |   /
       ↓  ↓  ↓
      RUNNABLE
         │
    execution ends
         ↓
     TERMINATED
```

### Core points to remember

- Java has **6 official thread states**.
- `NEW` means created but not started.
- `start()` moves the thread toward `RUNNABLE`.
- Java has **no separate `RUNNING` state**.
- `BLOCKED` means waiting for a monitor lock.
- `WAITING` means waiting indefinitely.
- `TIMED_WAITING` means waiting for a specified duration.
- `sleep()` causes `TIMED_WAITING`.
- `join()` can cause `WAITING` or `TIMED_WAITING`.
- A completed thread becomes `TERMINATED`.
- A `TERMINATED` thread cannot be restarted.
- `getState()` returns the current `Thread.State`.

> **Create → Start → Run → Wait/Block → Resume → Terminate.** ⚡

---

## 🚀 Next Topic

**03 — Creating Threads**

Topics will include:

- Creating threads using `Thread`
- Extending `Thread`
- Implementing `Runnable`
- `start()` vs `run()`
- Thread object vs actual execution
- Which approach is preferred and why
- Internal working
- Interview questions