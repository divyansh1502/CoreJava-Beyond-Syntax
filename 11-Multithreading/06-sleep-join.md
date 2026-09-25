# 06 — sleep() and join()

> **`sleep()` pauses the current thread for a specified time, while `join()` makes one thread wait for another thread to finish.**

---

## 📌 Table of Contents

1. [Introduction](#-introduction)
2. [sleep()](#-sleep)
3. [Why sleep()?](#-why-sleep)
4. [sleep() Syntax](#-sleep-syntax)
5. [sleep() Example](#-sleep-example)
6. [sleep() and Current Thread](#-sleep-and-current-thread)
7. [sleep() Does Not Create a Thread](#-sleep-does-not-create-a-thread)
8. [sleep() and InterruptedException](#-sleep-and-interruptedexception)
9. [sleep() and Locks](#-sleep-and-locks)
10. [join()](#-join)
11. [Why join()?](#-why-join)
12. [join() Example](#-join-example)
13. [join() Internal Flow](#-join-internal-flow)
14. [join() with Multiple Threads](#-join-with-multiple-threads)
15. [join() vs sleep()](#-join-vs-sleep)
16. [Timed join()](#-timed-join)
17. [Common Mistakes](#-common-mistakes)
18. [Interview Traps](#-interview-traps)
19. [DSA / Problem-Solving Relevance](#-dsa--problem-solving-relevance)
20. [30-Second Interview Answer](#-30-second-interview-answer)
21. [Cheat Sheet](#-cheat-sheet)
22. [Top 10 Interview Questions](#-top-10-interview-questions)

---

# 🔹 Introduction

Two important methods used for basic thread coordination are:

```java
Thread.sleep()
```

and:

```java
Thread.join()
```

Although both can cause a thread to wait, they solve different problems.

### `sleep()`

Pauses the **currently executing thread** for a specified amount of time.

### `join()`

Makes the **current thread wait for another thread to terminate**.

Memory trick:

```text
sleep()
→ Wait for TIME

join()
→ Wait for THREAD
```

---

# 🔹 sleep()

`sleep()` is a static method of the `Thread` class.

It pauses the currently executing thread for a specified amount of time.

Example:

```java
Thread.sleep(2000);
```

This requests the current thread to sleep for approximately:

```text
2000 milliseconds
```

which is:

```text
2 seconds
```

---

# 🔹 Why sleep()?

`sleep()` can be useful when a thread needs to pause temporarily.

Common examples:

- Delaying execution
- Simulating a time-consuming operation
- Polling at intervals
- Retry mechanisms
- Controlling periodic tasks
- Demonstrating thread scheduling

Example:

```java
class Main {

    public static void main(String[] args)
            throws InterruptedException {

        System.out.println("Start");

        Thread.sleep(2000);

        System.out.println("End");
    }
}
```

Output:

```text
Start
```

After approximately 2 seconds:

```text
End
```

---

# 🔹 sleep() Syntax

Common forms include:

```java
Thread.sleep(1000);
```

and:

```java
Thread.sleep(1000, 500000);
```

The first argument represents milliseconds.

The second form additionally specifies nanoseconds.

The commonly used form is:

```java
Thread.sleep(milliseconds);
```

---

# 🔹 sleep() Example

```java
class Main {

    public static void main(String[] args)
            throws InterruptedException {

        for(int i = 1; i <= 5; i++) {

            System.out.println(i);

            Thread.sleep(1000);
        }
    }
}
```

Possible output:

```text
1
2
3
4
5
```

There is approximately a one-second pause between each iteration.

---

# 🔹 sleep() and Current Thread

One of the most important points:

> `sleep()` always affects the **currently executing thread**.

Example:

```java
class Main {

    public static void main(String[] args)
            throws InterruptedException {

        Thread t = new Thread(() -> {

            try {

                Thread.sleep(2000);

            } catch(InterruptedException e) {

                Thread.currentThread().interrupt();
            }
        });

        t.start();
    }
}
```

Here:

```java
Thread.sleep(2000);
```

is executed by `t`.

Therefore:

```text
Thread t
   ↓
sleep()
   ↓
Thread t pauses
```

It does not mean:

```text
Sleep some arbitrary Thread object
```

---

# 🔹 sleep() is Static

The method is static.

Therefore:

```java
Thread.sleep(1000);
```

is the recommended and clear form.

You may technically write:

```java
Thread t = new Thread();

t.sleep(1000);
```

but this is misleading because `sleep()` still affects the **currently executing thread**, not `t`.

Prefer:

```java
Thread.sleep(1000);
```

---

# 🔹 sleep() Does Not Create a Thread

Calling:

```java
Thread.sleep(1000);
```

does not create a new thread.

It simply pauses the current thread.

Conceptually:

```text
Current Thread
      |
      ↓
   sleep()
      |
      ↓
   TIMED_WAITING
      |
      ↓
Time expires
      |
      ↓
Runnable again
```

---

# 🔹 sleep() and Thread State

While a thread is sleeping, its state is generally:

```text
TIMED_WAITING
```

Example:

```java
class MyThread extends Thread {

    @Override
    public void run() {

        try {

            Thread.sleep(5000);

        } catch(InterruptedException e) {

            Thread.currentThread().interrupt();
        }
    }
}
```

While sleeping:

```text
RUNNABLE
   ↓
sleep()
   ↓
TIMED_WAITING
   ↓
time expires
   ↓
RUNNABLE
```

---

# 🔹 sleep() and InterruptedException

`sleep()` can throw:

```java
InterruptedException
```

Therefore, we must handle or declare it.

### Using throws

```java
class Main {

    public static void main(String[] args)
            throws InterruptedException {

        Thread.sleep(1000);

        System.out.println("Done");
    }
}
```

### Using try-catch

```java
class Main {

    public static void main(String[] args) {

        try {

            Thread.sleep(1000);

        } catch(InterruptedException e) {

            Thread.currentThread().interrupt();
        }

        System.out.println("Done");
    }
}
```

---

# 🔹 Why Restore Interrupt Status?

Suppose we catch:

```java
InterruptedException
```

A common good practice is:

```java
Thread.currentThread().interrupt();
```

This restores the interrupted status so higher-level code can observe that interruption occurred.

Example:

```java
try {

    Thread.sleep(1000);

} catch(InterruptedException e) {

    Thread.currentThread().interrupt();
}
```

This is especially important in real-world concurrent code.

---

# 🔹 sleep() and Locks

A very important interview concept:

> `sleep()` does **not** release an intrinsic monitor lock.

Example:

```java
class Main {

    static final Object lock = new Object();

    public static void main(String[] args) {

        synchronized(lock) {

            try {

                Thread.sleep(2000);

            } catch(InterruptedException e) {

                Thread.currentThread().interrupt();
            }
        }
    }
}
```

While sleeping:

```text
Thread owns lock
      ↓
sleep()
      ↓
Thread pauses
      ↓
Lock is still owned
```

Another thread cannot acquire that same intrinsic lock merely because the first thread is sleeping.

---

# 🔹 join()

`join()` is an instance method of `Thread`.

It causes the current thread to wait until the target thread terminates.

Example:

```java
class MyThread extends Thread {

    @Override
    public void run() {

        System.out.println("Worker is running");
    }
}

class Main {

    public static void main(String[] args)
            throws InterruptedException {

        MyThread t = new MyThread();

        t.start();

        t.join();

        System.out.println("Main continues");
    }
}
```

The important line is:

```java
t.join();
```

This means:

> The current thread should wait for `t` to finish.

---

# 🔹 Why join()?

Suppose the main thread starts a worker thread:

```text
Main
  |
  +---- Worker
```

Without `join()`, both may continue independently.

With:

```java
worker.join();
```

the main thread waits for the worker.

Flow:

```text
Main Thread
     |
     ↓
worker.start()
     |
     +----------> Worker executes
     |
     ↓
worker.join()
     |
     | waits
     |
     ↓
Worker finishes
     |
     ↓
Main continues
```

---

# 🔹 join() Example

```java
class Worker extends Thread {

    @Override
    public void run() {

        for(int i = 1; i <= 5; i++) {

            System.out.println("Worker: " + i);

            try {

                Thread.sleep(500);

            } catch(InterruptedException e) {

                Thread.currentThread().interrupt();
            }
        }
    }
}

class Main {

    public static void main(String[] args)
            throws InterruptedException {

        Worker worker = new Worker();

        worker.start();

        worker.join();

        System.out.println("Main thread finished");
    }
}
```

The main thread waits until the worker finishes.

---

# 🔹 join() Internal Flow

Suppose:

```java
worker.start();

worker.join();

System.out.println("Done");
```

Conceptually:

```text
1. Worker thread starts
          ↓
2. Main reaches worker.join()
          ↓
3. Main waits
          ↓
4. Worker executes
          ↓
5. Worker terminates
          ↓
6. Main continues
          ↓
7. "Done"
```

---

# 🔹 join() Does Not Mean "Start"

This:

```java
worker.join();
```

does not start the worker.

You must first start it:

```java
worker.start();

worker.join();
```

If the thread has not been started, the behavior is not equivalent to "start then wait."

Memory:

```text
start()
→ Start thread

join()
→ Wait for thread
```

---

# 🔹 join() with Multiple Threads

Suppose we have:

```java
Thread t1 = new Thread(() -> {

    System.out.println("Task 1");

});

Thread t2 = new Thread(() -> {

    System.out.println("Task 2");

});
```

Start both:

```java
t1.start();
t2.start();
```

Then wait for both:

```java
t1.join();
t2.join();
```

Complete example:

```java
class Main {

    public static void main(String[] args)
            throws InterruptedException {

        Thread t1 = new Thread(() -> {

            System.out.println("Task 1");

        });

        Thread t2 = new Thread(() -> {

            System.out.println("Task 2");

        });

        t1.start();
        t2.start();

        t1.join();
        t2.join();

        System.out.println("Both tasks finished");
    }
}
```

The main thread waits until both threads terminate.

---

# 🔹 Important Point About Multiple join()

The order of:

```java
t1.join();
t2.join();
```

controls when the current thread waits.

It does **not** necessarily mean:

```text
t1 executes completely
       ↓
t2 starts
```

because both were already started:

```java
t1.start();
t2.start();
```

They may execute concurrently.

The `join()` calls simply ensure that the current thread does not continue until the corresponding threads have terminated.

---

# 🔹 join() with Time Limit

There is also a timed version:

```java
join(long millis)
```

Example:

```java
thread.join(2000);
```

This means the current thread waits for the target thread for up to approximately 2000 milliseconds.

It may return earlier if the target thread finishes earlier.

Important:

> Timed `join()` does not guarantee that the target thread has finished after the timeout expires.

---

# 🔹 Timed join() Example

```java
class Main {

    public static void main(String[] args)
            throws InterruptedException {

        Thread worker = new Thread(() -> {

            try {

                Thread.sleep(5000);

            } catch(InterruptedException e) {

                Thread.currentThread().interrupt();
            }

        });

        worker.start();

        worker.join(2000);

        System.out.println("Main continues");
    }
}
```

The main thread waits for at most approximately 2 seconds.

The worker may still be running after that.

---

# 🔹 join() and InterruptedException

Like `sleep()`, `join()` can throw:

```java
InterruptedException
```

Example:

```java
class Main {

    public static void main(String[] args) {

        Thread worker = new Thread(() -> {

            System.out.println("Worker");

        });

        worker.start();

        try {

            worker.join();

        } catch(InterruptedException e) {

            Thread.currentThread().interrupt();
        }
    }
}
```

---

# 🔹 sleep() vs join()

This is one of the most important comparisons.

| `sleep()` | `join()` |
|---|---|
| Static method | Instance method |
| Called as `Thread.sleep()` | Called on a thread object |
| Pauses current thread | Current thread waits for target thread |
| Time-based waiting | Thread-completion-based waiting |
| Does not wait for another thread to finish | Specifically waits for another thread |
| Can use milliseconds/nanoseconds | Can use timeout |
| Does not release intrinsic monitor | `join()` itself is based on waiting for termination |

Memory:

```text
sleep()
→ "Wait for some time."

join()
→ "Wait for that thread."
```

---

# 🔹 Example Comparing Both

```java
class Main {

    public static void main(String[] args)
            throws InterruptedException {

        Thread worker = new Thread(() -> {

            try {

                Thread.sleep(2000);

                System.out.println("Worker finished");

            } catch(InterruptedException e) {

                Thread.currentThread().interrupt();
            }
        });

        worker.start();

        worker.join();

        System.out.println("Main finished");
    }
}
```

Here:

```text
worker.sleep()
→ Worker pauses for 2 seconds

worker.join()
→ Main waits for Worker

Main continues
→ after Worker terminates
```

---

# 🔹 sleep() vs join() — Mental Model

Think about two people:

```text
sleep()
→ "I will wait for 5 seconds."

join()
→ "I will wait until you finish."
```

This is the easiest way to remember the difference.

---

# 🔹 Common Mistakes

## ❌ Mistake 1 — Thinking sleep() pauses another thread

Wrong:

```java
Thread t = new Thread();

Thread.sleep(1000);
```

This does not mean:

```text
Pause t
```

It means:

```text
Pause the currently executing thread
```

---

## ❌ Mistake 2 — Calling join() before start()

Do not think:

```java
t.join();
```

means:

```text
Start t
```

It does not.

Usually the intended sequence is:

```java
t.start();

t.join();
```

---

## ❌ Mistake 3 — Thinking join() pauses the target thread

Wrong idea:

```text
t.join()
→ t pauses
```

Correct:

```text
current thread
      ↓
waits for t
```

The target thread continues executing.

---

## ❌ Mistake 4 — Thinking sleep() releases locks

It does not release intrinsic monitor locks.

---

## ❌ Mistake 5 — Assuming timed join() guarantees completion

Example:

```java
t.join(1000);
```

After this returns, `t` may still be running.

---

# 🔹 Interview Traps

### Q1. Is sleep() static?

Yes.

It is a static method of `Thread`.

Use:

```java
Thread.sleep(1000);
```

---

### Q2. Which thread does sleep() affect?

The currently executing thread.

---

### Q3. Does sleep() release a lock?

No.

---

### Q4. What does join() do?

It makes the current thread wait until the target thread terminates, unless a timeout is used.

---

### Q5. Is join() static?

No.

It is an instance method.

Example:

```java
worker.join();
```

---

### Q6. Does join() start a thread?

No.

Use:

```java
worker.start();
```

to start it.

---

### Q7. Does join() pause the target thread?

No.

The current thread waits; the target thread continues execution.

---

### Q8. Can join() have a timeout?

Yes.

Example:

```java
worker.join(2000);
```

---

### Q9. What exception can sleep() and join() throw?

Both can throw:

```java
InterruptedException
```

---

### Q10. What is the simplest difference between sleep() and join()?

```text
sleep()
→ wait for time

join()
→ wait for thread completion
```

---

# 🔹 DSA / Problem-Solving Relevance

`sleep()` and `join()` are not DSA patterns themselves, but `join()` is useful for understanding concurrent algorithms.

Example:

```text
Main Thread
    |
    +---- Worker 1
    |
    +---- Worker 2
    |
    +---- Worker 3
```

All workers can process separate portions of a problem.

Then:

```text
join Worker 1
join Worker 2
join Worker 3
       ↓
Combine results
```

Conceptually:

```text
Input
  ↓
Split
  ↓
+----------+----------+----------+
| Worker 1 | Worker 2 | Worker 3 |
+----------+----------+----------+
      ↓          ↓          ↓
    Result     Result     Result
      \          |          /
       \         |         /
        +--------+--------+
                 ↓
          Combined Result
```

This idea appears in:

- Parallel searching
- Parallel processing
- Concurrent algorithms
- Divide-and-conquer implementations
- Producer-consumer coordination

---

# 🔹 30-Second Interview Answer

> `sleep()` and `join()` are important thread coordination methods. `Thread.sleep()` pauses the currently executing thread for a specified amount of time, while `join()` makes the current thread wait for another thread to terminate. `sleep()` is static and time-based, whereas `join()` is an instance method and thread-completion-based. Both can throw `InterruptedException`, and `sleep()` does not release intrinsic monitor locks.

---

# 🔹 Cheat Sheet

## Sleep

```java
Thread.sleep(1000);
```

Meaning:

```text
Current thread
      ↓
Pause for ~1 second
```

---

## Sleep with try-catch

```java
try {

    Thread.sleep(1000);

} catch(InterruptedException e) {

    Thread.currentThread().interrupt();
}
```

---

## Join

```java
thread.start();

thread.join();
```

Meaning:

```text
Start thread
     ↓
Current thread waits
     ↓
Target thread finishes
     ↓
Current thread continues
```

---

## Timed Join

```java
thread.join(2000);
```

Meaning:

```text
Wait for target
     ↓
Maximum approximately 2 seconds
```

---

# 🧠 Memory Tricks

### `sleep()`

> **TIME**

```text
sleep()
→ "Wait for some time."
```

### `join()`

> **THREAD**

```text
join()
→ "Wait for that thread."
```

### `start()`

> **START**

```text
start()
→ Start new thread
```

### Combined

```text
start()
  ↓
Thread begins

sleep()
  ↓
Current thread pauses

join()
  ↓
Current thread waits for another thread
```

---

# 🔥 Top 10 Interview Questions

## 1. What is Thread.sleep()?

It pauses the currently executing thread for a specified amount of time.

---

## 2. Is sleep() static?

Yes.

```java
Thread.sleep(1000);
```

---

## 3. Does sleep() release a lock?

No.

Sleeping does not release an intrinsic monitor lock held by the thread.

---

## 4. What is join()?

`join()` makes the current thread wait for another thread to terminate.

---

## 5. Is join() static?

No.

It is called on a particular thread object.

```java
worker.join();
```

---

## 6. Does join() stop the target thread?

No.

The target thread continues running.

The current thread waits for it.

---

## 7. Does join() start a thread?

No.

Usually:

```java
worker.start();
worker.join();
```

---

## 8. What happens with timed join()?

```java
worker.join(2000);
```

The current thread waits for at most approximately 2 seconds, unless the target finishes earlier.

---

## 9. What exception can sleep() and join() throw?

```java
InterruptedException
```

---

## 10. What is the difference between sleep() and join()?

```text
sleep()
→ Current thread waits for time.

join()
→ Current thread waits for another thread.
```

---

# 🎯 Final Summary

```text
                    Thread
                       |
            +----------+----------+
            |                     |
         sleep()               join()
            |                     |
      Time-based wait       Thread-based wait
            |                     |
      Current thread         Current thread
          pauses                 waits
```

### ⭐ `sleep()`

```java
Thread.sleep(1000);
```

- Static
- Pauses current thread
- Time-based
- Causes `TIMED_WAITING`
- Can throw `InterruptedException`
- Does not release intrinsic monitor locks

### ⭐ `join()`

```java
thread.join();
```

- Instance method
- Current thread waits for target thread
- Thread-completion-based
- Can throw `InterruptedException`
- Timed version is available

### ⭐ Core Mental Model

```text
sleep()
→ "Wait for TIME."

join()
→ "Wait for THREAD."
```

> **Core idea: `sleep()` controls how long the current thread pauses, while `join()` coordinates threads by making one thread wait for another to finish.**