# 🧵 Java Multithreading — Interview Questions & Answers

> **A focused collection of Java Multithreading interview questions with concise, interview-ready answers.**

---

# 📑 Table of Contents

- [1. Multithreading Basics](#1-multithreading-basics)
- [2. Thread and Runnable](#2-thread-and-runnable)
- [3. Thread Lifecycle](#3-thread-lifecycle)
- [4. Thread Methods](#4-thread-methods)
- [5. Synchronization](#5-synchronization)
- [6. Inter-Thread Communication](#6-inter-thread-communication)
- [7. Locks](#7-locks)
- [8. Executor Framework](#8-executor-framework)
- [9. Callable and Future](#9-callable-and-future)
- [10. Atomicity, Visibility and Volatile](#10-atomicity-visibility-and-volatile)
- [11. Deadlock and Concurrency Problems](#11-deadlock-and-concurrency-problems)
- [12. Tricky Multithreading Questions](#12-tricky-multithreading-questions)
- [13. Rapid Multithreading Revision](#13-rapid-multithreading-revision)

---

# 1. Multithreading Basics

## 1. What is a process?

A process is an independent program in execution.

A process has its own:

```text
Memory space
Resources
Execution context
```

---

## 2. What is a thread?

A thread is a lightweight unit of execution within a process.

Multiple threads can exist inside the same process and share many process resources.

---

## 3. Process vs Thread?

```text
Process
→ independent execution environment
→ separate memory space


Thread
→ execution unit inside a process
→ shares process resources
```

---

## 4. What is multithreading?

Multithreading is the execution of multiple threads within a single process.

It can improve responsiveness and throughput when tasks can overlap or execute concurrently.

---

## 5. What is concurrency?

Concurrency means multiple tasks make progress during overlapping periods.

They do not necessarily execute simultaneously.

---

## 6. What is parallelism?

Parallelism means multiple tasks execute simultaneously, typically on multiple CPU cores.

---

## 7. Concurrency vs Parallelism?

```text
Concurrency
→ dealing with multiple tasks at overlapping times


Parallelism
→ executing multiple tasks simultaneously
```

---

## 8. Why use multithreading?

Common reasons include:

```text
Better responsiveness
Improved resource utilization
Background processing
Handling multiple requests
Parallel computation
Higher throughput
```

---

## 9. What is context switching?

Context switching occurs when the CPU switches execution from one thread or process to another.

The system must save and restore execution state.

Too many context switches can reduce performance.

---

## 10. What is a thread-safe class?

A class is thread-safe when its behavior remains correct when accessed concurrently according to its documented contract.

Thread safety can be achieved through mechanisms such as:

```text
Synchronization
Locks
Immutability
Atomic classes
Concurrent collections
```

---

# 2. Thread and Runnable

## 11. How can you create a thread in Java?

Common approaches include:

```text
Extending Thread
Implementing Runnable
Using Callable with ExecutorService
```

Modern applications commonly prefer tasks with the Executor framework rather than manually creating many Thread objects.

---

## 12. How do you create a thread by extending Thread?

Example:

```java
class MyThread extends Thread {

    @Override
    public void run() {

        System.out.println(
            "Thread running"
        );
    }
}
```

Start it with:

```java
MyThread t =
    new MyThread();

t.start();
```

---

## 13. How do you create a thread using Runnable?

Example:

```java
class MyTask
    implements Runnable {

    @Override
    public void run() {

        System.out.println(
            "Task running"
        );
    }
}
```

Then:

```java
Thread t =
    new Thread(
        new MyTask()
    );

t.start();
```

---

## 14. Runnable vs Thread?

```text
Thread
→ represents a thread of execution
→ extending it prevents extending another class


Runnable
→ represents a task
→ allows the class to extend another class
→ separates task from execution mechanism
```

---

## 15. Why is Runnable generally preferred over extending Thread?

Because Java supports single class inheritance.

If a class extends Thread, it cannot extend another class.

With Runnable:

```text
Task
↓
Runnable

Execution
↓
Thread / Executor
```

This provides better separation of concerns.

---

## 16. What is the difference between `start()` and `run()`?

```text
start()
→ starts a new thread


run()
→ normal method call if called directly
```

Example:

```java
Thread t =
    new Thread(() -> {

        System.out.println(
            "Running"
        );
    });

t.start();
```

Calling:

```java
t.run();
```

does not create a new thread.

---

## 17. Can `start()` be called twice on the same Thread?

No.

Calling `start()` more than once on the same Thread object results in:

```text
IllegalThreadStateException
```

---

## 18. Can the same Runnable be used by multiple Threads?

Yes.

Example:

```java
Runnable task =
    () -> System.out.println(
        "Running"
    );

Thread t1 =
    new Thread(task);

Thread t2 =
    new Thread(task);

t1.start();
t2.start();
```

The same task object can be executed by multiple threads.

---

## 19. What is a daemon thread?

A daemon thread is a background thread that does not prevent the JVM from shutting down when no non-daemon threads remain.

Examples can include background support tasks.

---

## 20. How do you make a thread daemon?

Call:

```java
thread.setDaemon(true);
```

before starting it.

Once a Thread has been started, its daemon status cannot be changed.

---

## 21. What is the main thread?

The JVM starts execution of the application's `main()` method on the main thread.

---

# 3. Thread Lifecycle

## 22. What are the states of a Java Thread?

Java defines these states:

```text
NEW
RUNNABLE
BLOCKED
WAITING
TIMED_WAITING
TERMINATED
```

---

## 23. What is NEW state?

A Thread is in NEW state after creation but before `start()` is invoked.

---

## 24. What is RUNNABLE state?

A thread is RUNNABLE when it is ready to run or is running under the JVM's thread-state model.

Java's Thread.State does not have a separate RUNNING state.

---

## 25. What is BLOCKED state?

A thread enters BLOCKED when it is waiting to acquire a monitor lock.

For example, when trying to enter a synchronized block currently owned by another thread.

---

## 26. What is WAITING state?

A thread is WAITING indefinitely for another thread to perform a particular action.

Examples include:

```text
Object.wait()
Thread.join()
LockSupport.park()
```

depending on the conditions.

---

## 27. What is TIMED_WAITING?

A thread is waiting for a specified amount of time.

Examples:

```text
Thread.sleep()
Object.wait(timeout)
Thread.join(timeout)
```

---

## 28. What is TERMINATED state?

A thread enters TERMINATED after its `run()` method finishes or terminates due to an uncaught exception.

---

## 29. Can a terminated thread be restarted?

No.

A Thread object can be started only once.

---

# 4. Thread Methods

## 30. What does `sleep()` do?

`Thread.sleep()` pauses the current thread for at least approximately the specified duration, subject to scheduling and system timing.

---

## 31. Does `sleep()` release a synchronized lock?

No.

If a thread sleeps while holding a monitor lock, it continues to hold that lock during the sleep.

---

## 32. What does `yield()` do?

`yield()` is a scheduling hint suggesting that the current thread is willing to let other runnable threads execute.

The scheduler is free to ignore the hint.

---

## 33. What does `join()` do?

`join()` causes the calling thread to wait until the target thread terminates, or until a specified timeout expires.

---

## 34. What does `interrupt()` do?

`interrupt()` requests that a thread be interrupted.

If the target thread is blocked in an interruptible operation such as `sleep()`, `wait()`, or `join()`, it may receive `InterruptedException`.

Otherwise, the thread's interrupt status is set.

---

## 35. Does `interrupt()` forcibly kill a thread?

No.

It is a cooperative interruption mechanism.

The interrupted thread must respond appropriately.

---

## 36. What is interrupted status?

A thread has an interrupt status flag.

Calling:

```java
Thread.currentThread()
    .interrupt();
```

sets the current thread's interrupt status.

Some interruptible methods clear the status when throwing `InterruptedException`.

---

## 37. `isInterrupted()` vs `interrupted()`?

```text
isInterrupted()
→ checks a thread's interrupt status
→ does not clear it


Thread.interrupted()
→ checks current thread's status
→ clears the status if set
```

---

## 38. What does `currentThread()` return?

It returns the Thread object representing the currently executing thread.

---

## 39. What is thread priority?

Thread priority is a scheduling-related value associated with a thread.

Java defines priorities from:

```text
Thread.MIN_PRIORITY
→ 1

Thread.NORM_PRIORITY
→ 5

Thread.MAX_PRIORITY
→ 10
```

Priority is a scheduling hint, not a guarantee of execution order.

---

# 5. Synchronization

## 40. What is synchronization?

Synchronization is a mechanism used to coordinate concurrent access to shared state.

It can prevent race conditions and establish memory-visibility guarantees when used correctly.

---

## 41. What is a race condition?

A race condition occurs when program correctness depends on the timing or interleaving of concurrent operations.

Example:

```text
Thread A reads 10
Thread B reads 10

Thread A writes 11
Thread B writes 11
```

Expected:

```text
12
```

Actual:

```text
11
```

---

## 42. What is a critical section?

A critical section is a section of code that accesses shared state and must be protected against unsafe concurrent execution.

---

## 43. What is a synchronized method?

A method declared with `synchronized` acquires the appropriate monitor before entering and releases it when leaving.

Example:

```java
synchronized void increment() {

    count++;
}
```

---

## 44. What is a synchronized block?

A synchronized block protects only a specific section of code.

Example:

```java
synchronized (lock) {

    count++;
}
```

---

## 45. Synchronized method vs synchronized block?

```text
Synchronized method
→ locks the method's associated monitor


Synchronized block
→ locks a specific object
→ can protect only the critical section
```

A block can therefore provide more precise locking.

---

## 46. What object is locked by an instance synchronized method?

For an instance synchronized method:

```text
this
```

is the monitor used for synchronization.

---

## 47. What object is locked by a static synchronized method?

The monitor associated with the Class object.

Conceptually:

```text
ClassName.class
```

---

## 48. Can two threads execute two different synchronized methods of the same object simultaneously?

If both are instance synchronized methods using the same object's monitor, no.

They require the same monitor lock.

---

## 49. Can synchronized methods of different objects execute simultaneously?

Yes.

Each object has its own monitor.

---

## 50. What is an intrinsic lock?

Every Java object can be associated with a monitor used by `synchronized`.

This is commonly called an intrinsic lock or monitor lock.

---

## 51. Does synchronization guarantee visibility?

Proper synchronization establishes happens-before relationships that provide the necessary memory visibility for protected operations.

---

## 52. Is synchronized reentrant?

Yes.

A thread that already owns a monitor can acquire the same monitor again.

The monitor keeps track of the reentrant acquisition count.

---

# 6. Inter-Thread Communication

## 53. What are `wait()`, `notify()`, and `notifyAll()`?

They are methods of `Object` used for coordination between threads using an object's monitor.

```text
wait()
→ releases the monitor and waits


notify()
→ wakes one waiting thread


notifyAll()
→ wakes all waiting threads
```

---

## 54. Does `wait()` release the lock?

Yes.

When a thread calls `wait()` while owning the object's monitor, it releases that monitor and enters a waiting state.

---

## 55. Does `sleep()` release the lock?

No.

This is a common interview question.

```text
wait()
→ releases monitor


sleep()
→ does not release monitor
```

---

## 56. Where must `wait()`, `notify()`, and `notifyAll()` be called?

The calling thread must own the object's monitor.

Otherwise:

```text
IllegalMonitorStateException
```

can occur.

---

## 57. Why should `wait()` usually be called inside a loop?

Because the condition must be checked again after waking.

Threads can experience spurious wakeups, and another thread may consume the condition before the awakened thread reacquires the lock.

Typical pattern:

```java
synchronized (lock) {

    while (!condition) {

        lock.wait();
    }

    // continue
}
```

---

## 58. Why use `notifyAll()` instead of `notify()`?

`notify()` wakes one waiting thread, but that thread may not be able to make progress for the required condition.

`notifyAll()` wakes all waiting threads, allowing them to re-check their conditions.

The appropriate choice depends on the synchronization protocol.

---

## 59. What is inter-thread communication?

It is coordination between threads so that one thread can wait for or signal conditions related to shared work or state.

Mechanisms include:

```text
wait/notify
BlockingQueue
CountDownLatch
Semaphore
Future
CompletableFuture
Locks and Conditions
```

---

# 7. Locks

## 60. What is Lock?

`Lock` is an interface in `java.util.concurrent.locks` providing explicit locking operations.

---

## 61. What is ReentrantLock?

`ReentrantLock` is a Lock implementation that provides explicit, reentrant mutual exclusion.

---

## 62. synchronized vs ReentrantLock?

```text
synchronized
→ simpler
→ automatic lock release
→ language-level feature


ReentrantLock
→ explicit lock/unlock
→ tryLock()
→ interruptible lock acquisition
→ optional fairness setting
```

---

## 63. Why must `unlock()` be placed in finally?

Because the lock should be released even if an exception occurs.

Typical pattern:

```java
lock.lock();

try {

    // critical section

} finally {

    lock.unlock();
}
```

---

## 64. What is `tryLock()`?

It attempts to acquire a lock without waiting indefinitely.

Example:

```java
if (lock.tryLock()) {

    try {

        // critical section

    } finally {

        lock.unlock();
    }
}
```

---

## 65. What is a ReentrantLock?

A lock that allows the same thread to acquire the same lock multiple times.

The thread must release it the corresponding number of times.

---

## 66. What is ReadWriteLock?

`ReadWriteLock` separates access into:

```text
Read lock
Write lock
```

Multiple readers can often proceed concurrently, while writing requires exclusive access.

---

## 67. What is ReentrantReadWriteLock?

It is an implementation of `ReadWriteLock` that provides reentrant read and write locks.

---

# 8. Executor Framework

## 68. What is Executor Framework?

The Executor Framework provides APIs for managing task execution separately from the creation and management of individual threads.

Main interfaces include:

```text
Executor
ExecutorService
ScheduledExecutorService
```

---

## 69. Why use ExecutorService instead of creating threads manually?

It provides:

```text
Thread pooling
Task management
Lifecycle control
Future results
Better resource management
```

---

## 70. What is a thread pool?

A thread pool is a collection of reusable worker threads used to execute submitted tasks.

Instead of creating a new thread for every task, existing worker threads can be reused.

---

## 71. What is Executors class?

`Executors` is a utility class that provides factory methods for creating common executor configurations.

Examples include:

```text
newFixedThreadPool()
newSingleThreadExecutor()
newScheduledThreadPool()
```

Modern applications should also consider configuring `ThreadPoolExecutor` directly when precise control is required.

---

## 72. What is FixedThreadPool?

A fixed thread pool maintains a fixed number of worker threads.

Example:

```java
ExecutorService service =
    Executors.newFixedThreadPool(3);
```

---

## 73. What is SingleThreadExecutor?

It uses a single worker thread to execute submitted tasks sequentially.

---

## 74. What is ScheduledExecutorService?

It supports delayed and periodic task execution.

---

## 75. What is `execute()`?

`execute()` submits a Runnable task for execution without returning a result object.

---

## 76. What is `submit()`?

`submit()` submits a task and returns a `Future`.

It can accept:

```text
Runnable
Callable
```

---

## 77. `execute()` vs `submit()`?

```text
execute()
→ Runnable
→ no Future result


submit()
→ Runnable or Callable
→ returns Future
```

---

## 78. What does `shutdown()` do?

It initiates an orderly shutdown.

Previously submitted tasks can continue executing, but new tasks are rejected.

---

## 79. What does `shutdownNow()` do?

It attempts to stop currently executing tasks, typically by interrupting worker threads, and prevents waiting tasks from starting.

It does not guarantee that running tasks will immediately stop.

---

## 80. What is the difference between shutdown and shutdownNow?

```text
shutdown()
→ graceful shutdown


shutdownNow()
→ attempts immediate shutdown
→ interrupts running workers
→ returns tasks that never started
```

---

# 9. Callable and Future

## 81. What is Callable?

`Callable<V>` represents a task that:

```text
returns a result
and
can throw checked exceptions
```

---

## 82. Runnable vs Callable?

```text
Runnable
→ run()
→ no return value
→ cannot declare checked exceptions


Callable<V>
→ call()
→ returns V
→ can throw Exception
```

---

## 83. What is Future?

`Future` represents the result of an asynchronous computation.

It can be used to:

```text
Check completion
Retrieve result
Cancel task
Check cancellation
```

---

## 84. What does `Future.get()` do?

It returns the task's result.

If the task has not completed, `get()` waits until the result becomes available or an exception occurs.

---

## 85. What is the problem with blocking on Future.get()?

If used carelessly, it can reduce concurrency because the calling thread waits for the result.

It can also contribute to thread-pool starvation in poorly designed systems.

---

## 86. What is CompletableFuture?

`CompletableFuture` represents an asynchronous computation that can be explicitly completed and composed with other asynchronous stages.

It supports operations such as:

```text
thenApply()
thenAccept()
thenCompose()
thenCombine()
exceptionally()
```

---

# 10. Atomicity, Visibility and Volatile

## 87. What is atomicity?

Atomicity means an operation appears indivisible with respect to the relevant concurrent observers.

A compound operation such as:

```java
count++;
```

is not automatically atomic.

---

## 88. Is `count++` atomic?

No.

It conceptually involves:

```text
Read
+
Increment
+
Write
```

Another thread can interleave between these steps.

---

## 89. What is visibility?

Visibility means that changes made by one thread become observable by another thread according to the Java Memory Model's happens-before rules.

---

## 90. What is `volatile`?

`volatile` is a Java keyword that provides visibility guarantees for reads and writes of a variable and restricts certain compiler/CPU reorderings around those accesses.

It does not make arbitrary compound operations atomic.

---

## 91. Does volatile make `count++` thread-safe?

No.

This is still unsafe:

```java
volatile int count;

count++;
```

The increment consists of multiple operations.

---

## 92. When is volatile useful?

It is useful when threads need visibility of independent reads/writes to a shared variable and the access pattern does not require compound atomic operations.

---

## 93. synchronized vs volatile?

```text
volatile
→ visibility/order guarantees
→ does not provide mutual exclusion


synchronized
→ mutual exclusion
→ visibility guarantees
→ supports compound critical sections
```

---

## 94. What are atomic classes?

Classes in `java.util.concurrent.atomic` provide atomic operations on shared variables.

Examples:

```text
AtomicInteger
AtomicLong
AtomicBoolean
AtomicReference
```

---

## 95. How does AtomicInteger help?

It provides atomic operations such as:

```text
incrementAndGet()
decrementAndGet()
compareAndSet()
```

This can avoid explicit locking for certain simple shared-state operations.

---

## 96. What is CAS?

CAS means:

```text
Compare-And-Set
```

It atomically checks whether a value equals an expected value and, if so, replaces it with a new value.

---

# 11. Deadlock and Concurrency Problems

## 97. What is deadlock?

Deadlock occurs when threads wait indefinitely for resources held by each other.

Example:

```text
Thread A
holds Lock 1
waits for Lock 2

Thread B
holds Lock 2
waits for Lock 1
```

---

## 98. What are the four Coffman conditions for deadlock?

Deadlock requires:

```text
Mutual exclusion
Hold and wait
No preemption
Circular wait
```

---

## 99. How can deadlock be prevented?

Common strategies include:

```text
Consistent lock ordering
Avoid unnecessary nested locks
Use tryLock() with timeouts
Keep critical sections small
Avoid holding locks while performing unrelated blocking operations
```

---

## 100. What is starvation?

Starvation occurs when a thread repeatedly fails to obtain the resources or scheduling opportunity it needs to make progress.

---

## 101. What is livelock?

Livelock occurs when threads remain active and repeatedly respond to each other but make no useful progress.

---

## 102. Deadlock vs starvation vs livelock?

```text
Deadlock
→ threads are blocked waiting for each other


Starvation
→ a thread cannot obtain needed resources/opportunity


Livelock
→ threads keep changing/responding but make no progress
```

---

## 103. What is thread starvation?

A thread experiences starvation when other threads continuously consume the resources or scheduling opportunities it needs.

---

## 104. What is a race condition?

A race condition occurs when the result depends on the timing/interleaving of concurrent operations.

---

## 105. What is a data race?

A data race occurs when multiple threads access the same variable concurrently, at least one access is a write, and there is no proper happens-before relationship between the accesses.

---

# 12. Tricky Multithreading Questions

## 106. What happens if `run()` is called directly?

No new thread is created.

It executes like a normal method call in the current thread.

---

## 107. What happens if `start()` is called twice?

The second call throws:

```text
IllegalThreadStateException
```

---

## 108. Does sleep release the lock?

```text
No
```

---

## 109. Does wait release the lock?

```text
Yes
```

The monitor is released while waiting.

---

## 110. Can wait() be called outside synchronized code?

Not correctly.

The thread must own the object's monitor, otherwise:

```text
IllegalMonitorStateException
```

can occur.

---

## 111. Can notify() be called outside synchronized code?

The calling thread must own the object's monitor.

Otherwise:

```text
IllegalMonitorStateException
```

can occur.

---

## 112. Can two threads execute the same synchronized instance method simultaneously on the same object?

```text
No
```

They compete for the same object's monitor.

---

## 113. Can two synchronized methods run simultaneously on different objects?

```text
Yes
```

Each object has a separate monitor.

---

## 114. Can a synchronized method call another synchronized method of the same object?

Yes.

The lock is reentrant.

---

## 115. Is `volatile` enough for `count++`?

```text
No
```

`count++` is a compound operation.

---

## 116. Is `i++` atomic?

Not generally.

It involves reading, modifying, and writing the value.

---

## 117. Does Thread priority guarantee execution order?

No.

Priority is a scheduling hint and behavior depends on the JVM and operating system scheduler.

---

## 118. Does `yield()` guarantee another thread will run?

No.

It is only a scheduling hint.

---

## 119. Does `sleep(0)` guarantee a context switch?

No.

It does not guarantee that another thread will execute.

---

## 120. Can a daemon thread keep JVM alive?

No.

When all non-daemon threads have terminated, the JVM can terminate even if daemon threads remain.

---

## 121. Is String thread-safe?

String is immutable, so its state cannot be modified after creation.

Sharing String objects between threads is therefore safe with respect to mutation of the String itself.

---

## 122. Is StringBuilder thread-safe?

No.

---

## 123. Is StringBuffer thread-safe?

Its methods are synchronized.

However, thread-safe individual methods do not automatically make arbitrary sequences of multiple operations atomic.

---

## 124. Is ArrayList thread-safe?

No.

---

## 125. Is Vector thread-safe?

Its methods are synchronized.

But compound sequences of operations may still require external synchronization.

---

## 126. Is HashMap thread-safe?

No.

For concurrent access requiring thread safety, use appropriate synchronization or a concurrent collection.

---

## 127. Is ConcurrentHashMap thread-safe?

Yes, it is designed for concurrent access.

---

## 128. Can ConcurrentHashMap contain null?

```text
No
```

---

# 13. Rapid Multithreading Revision

## 129. Process vs Thread?

```text
Process
→ independent execution environment


Thread
→ execution unit inside process
```

---

## 130. Concurrency vs Parallelism?

```text
Concurrency
→ overlapping progress


Parallelism
→ simultaneous execution
```

---

## 131. `start()` vs `run()`?

```text
start()
→ creates/schedules new thread execution


run()
→ normal method call
```

---

## 132. Can start() be called twice?

```text
No
```

---

## 133. Does sleep() release lock?

```text
No
```

---

## 134. Does wait() release lock?

```text
Yes
```

---

## 135. What are Java thread states?

```text
NEW
RUNNABLE
BLOCKED
WAITING
TIMED_WAITING
TERMINATED
```

---

## 136. What is synchronization?

```text
Coordination of concurrent access to shared state.
```

---

## 137. What is race condition?

```text
Result depends on concurrent timing/interleaving.
```

---

## 138. What is deadlock?

```text
Threads wait indefinitely for resources
held by each other.
```

---

## 139. What is starvation?

```text
A thread repeatedly fails to obtain resources
or scheduling opportunity.
```

---

## 140. What is livelock?

```text
Threads remain active but make no useful progress.
```

---

## 141. What does volatile provide?

```text
Visibility
+
ordering guarantees
```

It does not make compound operations atomic.

---

## 142. What is AtomicInteger?

```text
Class providing atomic operations on int values.
```

---

## 143. What is CAS?

```text
Compare-And-Set
```

---

## 144. What is ExecutorService?

```text
Framework for managing asynchronous task execution.
```

---

## 145. execute() vs submit()?

```text
execute()
→ no Future


submit()
→ returns Future
```

---

## 146. Runnable vs Callable?

```text
Runnable
→ no result


Callable
→ returns result
→ can throw checked exceptions
```

---

## 147. What is Future?

```text
Represents the result of an asynchronous computation.
```

---

## 148. What is ReentrantLock?

```text
Explicit reentrant Lock implementation.
```

---

## 149. What is Reentrant?

```text
A thread that owns a lock can acquire
the same lock again.
```

---

## 150. What is thread-safe?

```text
Correct behavior under permitted concurrent access.
```

---

# 🧠 Multithreading Memory Map

```text
                       MULTITHREADING
                             |
          +------------------+------------------+
          |                  |                  |
       Threads          Synchronization      Executors
          |                  |                  |
    +-----+-----+       synchronized       Thread Pool
    |           |       Lock               Future
 Runnable      Thread    volatile           Callable
                         Atomic
                             |
                    Inter-thread Communication
                             |
                    +--------+--------+
                    |        |        |
                   wait    notify   notifyAll
```

---

# ⚡ Most Important Multithreading Comparisons

```text
Process
vs
Thread


Concurrency
vs
Parallelism


Thread
vs
Runnable


start()
vs
run()


sleep()
vs
wait()


wait()
vs
notify()
vs
notifyAll()


synchronized
vs
volatile


synchronized
vs
ReentrantLock


Runnable
vs
Callable


execute()
vs
submit()


Future
vs
CompletableFuture


ArrayList
vs
Concurrent Collections


Deadlock
vs
Starvation
vs
Livelock


Atomicity
vs
Visibility
vs
Ordering
```

---

