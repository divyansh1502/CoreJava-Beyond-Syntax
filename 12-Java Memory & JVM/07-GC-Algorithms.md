# 07 — GC Algorithms

## 📚 Table of Contents

1. [Introduction](#1-introduction)
2. [Why GC Algorithms?](#2-why-gc-algorithms)
3. [What is a GC Algorithm?](#3-what-is-a-gc-algorithm)
4. [Main GC Algorithm Families](#4-main-gc-algorithm-families)
5. [Mark-Sweep](#5-mark-sweep)
6. [Mark-Sweep Working](#6-mark-sweep-working)
7. [Advantages of Mark-Sweep](#7-advantages-of-mark-sweep)
8. [Disadvantages of Mark-Sweep](#8-disadvantages-of-mark-sweep)
9. [Mark-Compact](#9-mark-compact)
10. [Mark-Compact Working](#10-mark-compact-working)
11. [Advantages of Mark-Compact](#11-advantages-of-mark-compact)
12. [Disadvantages of Mark-Compact](#12-disadvantages-of-mark-compact)
13. [Copying Algorithm](#13-copying-algorithm)
14. [Copying Algorithm Working](#14-copying-algorithm-working)
15. [Advantages of Copying](#15-advantages-of-copying)
16. [Disadvantages of Copying](#16-disadvantages-of-copying)
17. [Mark-Sweep vs Mark-Compact vs Copying](#17-mark-sweep-vs-mark-compact-vs-copying)
18. [Generational Collection](#18-generational-collection)
19. [Parallel Garbage Collection](#19-parallel-garbage-collection)
20. [Concurrent Garbage Collection](#20-concurrent-garbage-collection)
21. [Stop-The-World vs Concurrent GC](#21-stop-the-world-vs-concurrent-gc)
22. [Evacuation](#22-evacuation)
23. [Remembered Sets](#23-remembered-sets)
24. [Card Tables](#24-card-tables)
25. [Write Barriers](#25-write-barriers)
26. [Fragmentation](#26-fragmentation)
27. [Compaction vs Evacuation](#27-compaction-vs-evacuation)
28. [Region-Based Collection](#28-region-based-collection)
29. [Modern JVM Garbage Collectors](#29-modern-jvm-garbage-collectors)
30. [GC Algorithm Selection](#30-gc-algorithm-selection)
31. [Advantages and Trade-offs](#31-advantages-and-trade-offs)
32. [Common Misconceptions](#32-common-misconceptions)
33. [Interview Traps](#33-interview-traps)
34. [Top 15 Interview Questions](#34-top-15-interview-questions)
35. [30-Second Interview Answer](#35-30-second-interview-answer)
36. [Cheat Sheet](#36-cheat-sheet)

---

# 1. Introduction

Garbage Collection algorithms define how a Garbage Collector identifies unreachable objects and reclaims their memory.

The JVM can use different collection strategies depending on:

- Heap size
- Application workload
- Throughput requirements
- Latency requirements
- Pause-time requirements
- CPU availability
- Memory requirements

### Core Idea

```text
Objects
   ↓
Reachability Analysis
   ↓
Identify Live Objects
   ↓
Identify Garbage
   ↓
Reclaim Memory
   ↓
Optional Compaction / Evacuation
```

### Definition

> A Garbage Collection algorithm is a strategy used by a Garbage Collector to identify unreachable objects and reclaim or reorganize their memory.

### Important

There is no single GC algorithm that is ideal for every application.

Different algorithms make different trade-offs between:

- Throughput
- Latency
- Pause time
- CPU usage
- Memory overhead
- Fragmentation

---

# 2. Why GC Algorithms?

Different applications have different requirements.

### Application A — Batch Processing

```text
Large amount of data
Long-running calculations
High throughput required
Longer pauses may be acceptable
```

A throughput-oriented collector may be appropriate.

### Application B — Interactive Application

```text
Users expect quick responses
Long pauses are undesirable
Low latency is important
```

A low-pause or concurrent collector may be appropriate.

### Application C — Large Heap Application

```text
Very large heap
Large number of objects
Pause time must remain manageable
```

A region-based or concurrent collector may be useful.

Therefore:

> GC algorithms exist because different applications require different memory-management trade-offs.

---

# 3. What is a GC Algorithm?

A GC algorithm determines how the Garbage Collector performs tasks such as:

1. Finding reachable objects.
2. Identifying unreachable objects.
3. Reclaiming memory.
4. Handling fragmentation.
5. Moving objects.
6. Updating references.
7. Coordinating with application threads.

### Simplified Process

```text
GC Roots
   ↓
Reachability Analysis
   ↓
Live Objects
   ↓
Garbage Objects
   ↓
Reclaim / Move / Compact
```

### Important

Modern Garbage Collectors generally combine multiple techniques.

For example:

```text
Generational
    +
Parallel
    +
Concurrent
    +
Marking
    +
Evacuation
```

Therefore, saying:

> "This collector uses only one algorithm"

is often an oversimplification.

---

# 4. Main GC Algorithm Families

The classical memory-reclamation algorithms are:

```text
1. Mark-Sweep
2. Mark-Compact
3. Copying
```

Modern collectors also use techniques such as:

```text
4. Generational Collection
5. Parallel Collection
6. Concurrent Collection
7. Evacuation
8. Region-Based Collection
9. Remembered Sets
10. Write Barriers
```

### Important Distinction

These terms do not all describe the same thing.

For example:

```text
Mark-Sweep
```

is a memory-reclamation algorithm.

Whereas:

```text
Parallel
Concurrent
Generational
```

describe broader collection strategies or execution characteristics.

---

# 5. Mark-Sweep

Mark-Sweep is one of the classical Garbage Collection algorithms.

It consists mainly of two phases:

```text
Mark
  ↓
Sweep
```

## 5.1 Mark

The collector starts from GC Roots and identifies reachable objects.

Example:

```text
GC Root
   |
   v
[A] ---> [B]
 |
 v
[C]
```

Objects `A`, `B`, and `C` are reachable.

They are marked as live.

---

## 5.2 Sweep

After marking, the collector identifies memory occupied by unreachable objects and reclaims it.

Example:

```text
[A] Live
[B] Garbage
[C] Live
[D] Garbage
```

After sweep:

```text
[A] Live
[Free]
[C] Live
[Free]
```

### Key Characteristic

Mark-Sweep normally does not move all live objects.

Therefore, fragmentation can remain.

---

# 6. Mark-Sweep Working

Consider this simplified heap:

```text
Before GC:

[A] [B] [C] [D] [E] [F]

A → Live
B → Garbage
C → Live
D → Garbage
E → Live
F → Garbage
```

## Step 1 — Mark

The collector traces from GC Roots.

```text
[A] Marked
[B] Unmarked
[C] Marked
[D] Unmarked
[E] Marked
[F] Unmarked
```

## Step 2 — Sweep

Unmarked objects are reclaimed.

```text
[A] [Free] [C] [Free] [E] [Free]
```

### Result

The memory of `B`, `D`, and `F` becomes available.

### Problem

Free memory is scattered:

```text
[Used][Free][Used][Free][Used][Free]
```

This is called **fragmentation**.

---

# 7. Advantages of Mark-Sweep

## 7.1 Conceptually Simple

The algorithm follows a straightforward model:

```text
Mark live objects
       ↓
Sweep dead objects
```

## 7.2 Handles Complex Object Graphs

It works with arbitrary object relationships.

## 7.3 Can Collect Cyclic Structures

Consider:

```text
A → B
↑   ↓
└───┘
```

If neither `A` nor `B` is reachable from a GC Root, the cycle can be collected.

## 7.4 Does Not Require Moving Every Live Object

Live objects can remain at their current locations.

---

# 8. Disadvantages of Mark-Sweep

## 8.1 Fragmentation

Because live objects are not necessarily moved:

```text
[Used][Free][Used][Free][Used][Free]
```

free memory becomes fragmented.

## 8.2 Allocation Complexity

A fragmented heap can make finding sufficiently large contiguous memory regions more difficult.

## 8.3 Marking Overhead

The collector must traverse the reachable object graph.

## 8.4 Potentially Longer Pauses

Depending on the implementation, marking and sweeping can contribute to application pauses.

---

# 9. Mark-Compact

Mark-Compact extends the basic Mark-Sweep idea.

It generally performs:

```text
Mark
  ↓
Identify Live Objects
  ↓
Move / Compact Live Objects
  ↓
Reclaim Remaining Space
```

### Main Difference

Mark-Sweep:

```text
Mark → Sweep
```

Mark-Compact:

```text
Mark → Compact
```

### Goal

The main goal of compaction is to reduce fragmentation.

---

# 10. Mark-Compact Working

Suppose the heap is:

```text
[A] [Free] [B] [Free] [C] [Free]
```

where:

```text
A → Live
B → Live
C → Live
```

A compaction phase can move live objects together.

After compaction:

```text
[A] [B] [C] [Free] [Free] [Free]
```

### Benefits

Free memory becomes more contiguous.

### Conceptual Process

```text
Before:

[Live][Free][Live][Free][Live][Free]

          ↓ Compact

[Live][Live][Live][Free][Free][Free]
```

---

# 11. Advantages of Mark-Compact

## 11.1 Reduces Fragmentation

Live objects are moved together.

## 11.2 Creates Contiguous Free Space

This can simplify future allocation.

## 11.3 Better Heap Utilization

The heap can have larger contiguous free areas.

## 11.4 Suitable for Long-Lived Objects

Compaction can be useful when many long-lived objects remain in memory.

---

# 12. Disadvantages of Mark-Compact

## 12.1 Moving Objects Costs Time

Objects must be relocated.

## 12.2 References Must Be Updated

If an object moves, references pointing to it must continue to refer to the correct object.

The JVM handles this internally.

## 12.3 Can Increase Pause Time

A large compaction operation may require significant work.

## 12.4 Additional Processing

The collector needs to determine new object locations and maintain correct references.

---

# 13. Copying Algorithm

The Copying algorithm divides memory into regions and copies live objects from one region to another.

A simplified model uses two spaces:

```text
From-Space
To-Space
```

Only one space is actively used for allocation at a time.

### Basic Idea

```text
From-Space
    |
    | Copy live objects
    v
To-Space
```

After copying:

```text
Old From-Space → Empty
New To-Space   → Contains live objects
```

The roles can then be exchanged.

---

# 14. Copying Algorithm Working

Suppose:

```text
From-Space:

[A] [B] [C] [D] [E]
```

Assume:

```text
A → Live
B → Garbage
C → Live
D → Garbage
E → Live
```

The live objects are copied:

```text
To-Space:

[A] [C] [E]
```

The original space can then be treated as free:

```text
From-Space:

[Free][Free][Free][Free][Free]
```

### Result

Live objects are automatically packed together.

Therefore, copying also avoids the fragmentation problem within the evacuated region.

---

# 15. Advantages of Copying

## 15.1 Little or No Fragmentation

Live objects are copied into a compact destination area.

## 15.2 Allocation Can Be Simple

The destination space can often use a simple moving allocation pointer.

Conceptually:

```text
[Live][Live][Live][Free][Free]
                     ^
                 allocation
```

## 15.3 Efficient When Most Objects Die

If only a small number of objects survive, only those live objects need to be copied.

## 15.4 Commonly Useful for Young Objects

Young generations often contain many short-lived objects, making copying or evacuation effective.

---

# 16. Disadvantages of Copying

## 16.1 Requires Additional Space

A copying design generally needs another destination area or enough free space for evacuation.

## 16.2 Copying Costs Time

Live objects must be moved.

## 16.3 Expensive If Most Objects Survive

If almost every object is alive:

```text
Many objects
     ↓
Many objects must be copied
     ↓
More work
```

## 16.4 Reference Processing

References to moved objects must remain correct.

---

# 17. Mark-Sweep vs Mark-Compact vs Copying

| Feature | Mark-Sweep | Mark-Compact | Copying |
|---|---|---|---|
| Mark live objects | Yes | Yes | Yes |
| Reclaims garbage | Yes | Yes | Yes |
| Moves live objects | Usually no | Yes | Yes |
| Fragmentation | Possible | Reduced | Very low in destination |
| Extra space | Lower | Lower | Usually higher |
| Object movement cost | Low | Higher | Higher |
| Good when few objects survive | Yes | Yes | Excellent |
| Allocation simplicity | Moderate | Good after compaction | Very good |
| Main issue | Fragmentation | Movement cost | Extra space |

### Memory Trick

```text
Mark-Sweep
    ↓
Remove garbage
    ↓
Fragmentation possible

Mark-Compact
    ↓
Remove garbage + move objects
    ↓
Less fragmentation

Copying
    ↓
Copy live objects elsewhere
    ↓
Old region becomes free
```

---

# 18. Generational Collection

Generational collection is based on the observation that many objects are short-lived.

This is commonly expressed as the:

> Generational Hypothesis

Conceptually:

```text
New Objects
     ↓
Young Generation
     ↓
Many die quickly
     ↓
Some survive
     ↓
Old Generation
```

### Basic Heap Organization

```text
Heap
 |
 +----------------------+
 | Young Generation     |
 |                      |
 | Eden                 |
 | Survivor Spaces      |
 +----------------------+
 |
 +----------------------+
 | Old Generation       |
 +----------------------+
```

### Why Generational GC?

Instead of treating every object equally:

```text
Collect young objects frequently
Collect old objects less frequently
```

This can improve efficiency because many young objects become unreachable quickly.

---

# 19. Parallel Garbage Collection

Parallel GC means multiple GC worker threads perform GC work simultaneously.

Conceptually:

```text
GC Task
  |
  +---- GC Thread 1
  +---- GC Thread 2
  +---- GC Thread 3
  +---- GC Thread 4
```

### Goal

Use multiple CPU cores to complete GC work faster.

### Important

Parallel does not necessarily mean concurrent.

A collector can use multiple GC threads while application threads are paused.

Example:

```text
Application
     |
     v
   PAUSE
     |
     v
GC Thread 1 ─┐
GC Thread 2 ─┤
GC Thread 3 ─┤
GC Thread 4 ─┘
     |
     v
  RESUME
```

This is parallel GC work but still Stop-The-World.

---

# 20. Concurrent Garbage Collection

Concurrent GC performs some GC work while application threads continue running.

Conceptually:

```text
Application Thread ───────────────────────>

GC Thread          ───────────────────────>
```

Both can execute during portions of the collection.

### Goal

Reduce long application pauses.

### Important

Concurrent does not mean:

> "The application never pauses."

Modern concurrent collectors can still require short Stop-The-World phases.

### Main Trade-off

Concurrent GC can reduce pause times but may require:

- additional CPU
- additional memory
- barriers
- more complex algorithms

---

# 21. Stop-The-World vs Concurrent GC

| Feature | Stop-The-World | Concurrent |
|---|---|---|
| Application threads | Paused during STW phase | Continue during concurrent phases |
| GC threads | Run | Run |
| Pause time | Can be higher | Generally designed to reduce pauses |
| CPU usage | Lower during paused phase | Can be higher |
| Complexity | Generally simpler | More complex |
| Goal | Efficient collection | Reduced application interruption |

### Important

This is not an absolute classification of an entire collector.

A modern collector may contain both:

```text
STW phases
+
Concurrent phases
```

---

# 22. Evacuation

Evacuation means moving live objects from one memory region to another.

Conceptually:

```text
Region A
[Live][Garbage][Live][Garbage]
       |
       | Evacuate live objects
       v
Region B
[Live][Live][Free][Free]
```

After successful evacuation:

```text
Region A
[Free][Free][Free][Free]
```

### Why Evacuate?

Evacuation can:

- reduce fragmentation
- compact live objects
- free entire regions
- simplify future allocation

### Copying vs Evacuation

Evacuation is closely related to copying.

A collector may select a region and evacuate its live objects into another region.

---

# 23. Remembered Sets

A remembered set helps a collector track references that may cross collection boundaries.

This becomes important in generational and region-based collectors.

Example:

```text
Old Region
    |
    | reference
    v
Young Region
```

Suppose the collector is collecting the Young Generation.

It needs to know about references from old regions into young objects.

A remembered set helps record relevant cross-region references.

### Why?

Without such information, the collector might need to scan the entire old generation to find references into the young generation.

That would be expensive.

### Core Idea

```text
Old Region
    |
    +---- reference to Young Region
              |
              v
       Remembered Information
```

---

# 24. Card Tables

A card table is a data structure used by some JVM garbage collectors to track which portions of memory may contain references relevant to GC.

The heap can conceptually be divided into cards:

```text
Heap:

+----+----+----+----+----+----+
| C1 | C2 | C3 | C4 | C5 | C6 |
+----+----+----+----+----+----+
```

When a reference update occurs in a card, the card can be marked as dirty.

Conceptually:

```text
Clean
  ↓
Reference update
  ↓
Dirty Card
  ↓
GC can inspect relevant area
```

### Benefit

Instead of scanning the entire heap, the collector can focus on relevant regions.

---

# 25. Write Barriers

A write barrier is a small piece of runtime-generated or compiler/JVM-related machinery associated with reference updates.

It allows the collector to maintain information needed for concurrent or generational collection.

Example concept:

```java
oldObject.reference = youngObject;
```

A write barrier can perform additional bookkeeping when this reference is updated.

Conceptually:

```text
Reference Write
      |
      v
Write Barrier
      |
      +---- Update remembered information
      |
      v
Continue
```

### Why Are Write Barriers Needed?

They help the collector track changes to the object graph while the application is running.

They are particularly important for:

- generational GC
- concurrent marking
- remembered sets
- region-based collectors

---

# 26. Fragmentation

Fragmentation occurs when free memory is divided into multiple separated regions.

Example:

```text
[Used][Free][Used][Free][Used][Free]
```

Total free memory may be large:

```text
Free + Free + Free
```

but it is not contiguous.

### Problem

Suppose a large object requires:

```text
[         Large Object         ]
```

The JVM may not be able to use several small free spaces as one contiguous area.

### Solution

Compaction or evacuation can reduce fragmentation.

```text
Before:

[Used][Free][Used][Free][Used][Free]

After:

[Used][Used][Used][Free][Free][Free]
```

---

# 27. Compaction vs Evacuation

Both can move objects, but the concepts are slightly different.

## Compaction

Compaction generally moves live objects within a heap area so that they become densely packed.

```text
Before:

[Live][Free][Live][Free][Live]

After:

[Live][Live][Live][Free][Free]
```

## Evacuation

Evacuation moves live objects from one region or space to another.

```text
Region A
[Live][Garbage][Live]
      |
      | Evacuate
      v
Region B
[Live][Live][Free]
```

### Memory Trick

```text
Compaction
→ Pack objects together

Evacuation
→ Move objects out of a region
```

---

# 28. Region-Based Collection

Some modern collectors divide the heap into many regions rather than treating the entire heap as one large contiguous area.

Conceptually:

```text
Heap

+----+----+----+----+----+----+
| R1 | R2 | R3 | R4 | R5 | R6 |
+----+----+----+----+----+----+
```

Different regions may contain different kinds or ages of objects.

The collector can select particular regions for collection.

### Benefits

- More flexible heap management
- Better locality
- Ability to prioritize regions
- Useful for large heaps
- Supports evacuation-based designs

This concept is important for understanding collectors such as G1.

---

# 29. Modern JVM Garbage Collectors

Modern Java provides several Garbage Collectors with different goals.

## 29.1 Serial GC

Serial GC uses a relatively simple design and performs collection using a single GC thread.

Conceptually:

```text
One GC Thread
      |
      v
GC Work
```

It can be suitable for small heaps or applications where simplicity is important.

---

## 29.2 Parallel GC

Parallel GC uses multiple GC threads.

Its primary design goal is high application throughput.

Conceptually:

```text
GC
 |
 +---- Thread 1
 +---- Thread 2
 +---- Thread 3
 +---- Thread 4
```

---

## 29.3 G1 Garbage Collector

G1 stands for:

> Garbage-First Garbage Collector

G1 divides the heap into regions.

Conceptually:

```text
+----+----+----+----+
| R1 | R2 | R3 | R4 |
+----+----+----+----+
| R5 | R6 | R7 | R8 |
+----+----+----+----+
```

It can prioritize regions containing more reclaimable space.

G1 uses concepts including:

- regions
- remembered sets
- evacuation
- concurrent marking
- pause-time goals

---

## 29.4 Z Garbage Collector

ZGC is designed for very low pause times, including for large heaps.

It uses highly concurrent techniques and region-based heap management.

Its design emphasizes:

- low latency
- concurrent processing
- large heaps

---

## 29.5 Shenandoah

Shenandoah is another low-pause collector.

It performs substantial GC work concurrently with application execution and uses concurrent compaction techniques.

### Important

The exact implementation details and supported features depend on the JDK version and JVM distribution.

---

# 30. GC Algorithm Selection

Choosing a Garbage Collector depends on application requirements.

Consider:

### 1. Heap Size

```text
Small heap
     ↓
Simpler collectors may be sufficient

Large heap
     ↓
Concurrent / region-based collectors may be considered
```

### 2. Latency Requirements

```text
Low latency required
       ↓
Low-pause collector may be considered
```

### 3. Throughput Requirements

```text
Maximum throughput
       ↓
Throughput-oriented collector may be considered
```

### 4. CPU Availability

Concurrent GC can require additional CPU resources.

### 5. Application Workload

Different allocation rates and object lifetimes can affect collector behavior.

### Important

There is no universally "best" Garbage Collector.

The appropriate choice depends on the application's requirements and actual measurements.

---

# 31. Advantages and Trade-offs

## Mark-Sweep

### Advantages

- Simple concept
- Does not require moving every live object
- Handles cyclic references

### Disadvantages

- Fragmentation
- Allocation can become harder
- Marking and sweeping consume resources

---

## Mark-Compact

### Advantages

- Reduces fragmentation
- Creates contiguous free memory
- Better allocation layout

### Disadvantages

- Objects must move
- References may need adjustment
- Can increase pause or processing cost

---

## Copying

### Advantages

- Fast allocation
- Little fragmentation
- Efficient when few objects survive

### Disadvantages

- Requires destination space
- Live objects must be copied
- Can be expensive when many objects survive

---

## Concurrent Collection

### Advantages

- Reduced long pauses
- GC can overlap with application execution

### Disadvantages

- More complex
- Additional CPU usage
- Requires barriers and bookkeeping
- Still has some STW phases

---

## Parallel Collection

### Advantages

- Uses multiple CPU cores
- Can reduce GC processing time
- Good for throughput-oriented workloads

### Disadvantages

- Uses more CPU resources
- Does not automatically mean low pause time

---

# 32. Common Misconceptions

## Misconception 1

> Mark-Sweep and Mark-Compact are exactly the same.

### Correct

Mark-Sweep reclaims unreachable memory but does not necessarily compact live objects.

Mark-Compact also moves live objects to reduce fragmentation.

---

## Misconception 2

> Copying means copying every object.

### Correct

The collector generally copies live objects from one area to another.

Garbage objects are not copied.

---

## Misconception 3

> Parallel GC means application threads continue running.

### Correct

Parallel refers to multiple GC threads working simultaneously.

The application can still be paused.

---

## Misconception 4

> Concurrent GC means there are no pauses.

### Correct

Concurrent collectors still require some Stop-The-World phases.

---

## Misconception 5

> Compaction and copying are completely unrelated.

### Correct

Both involve moving live objects, but copying generally moves objects between spaces or regions, while compaction packs live objects together within a collection area.

---

## Misconception 6

> Generational GC is one specific algorithm.

### Correct

Generational collection is a strategy that divides objects according to age or expected lifetime and applies collection differently to different generations.

---

## Misconception 7

> G1 means the heap has only one generation.

### Correct

G1 uses a region-based heap and can logically manage regions as young or old, while its implementation is more sophisticated than a simple contiguous young/old split.

---

## Misconception 8

> A modern collector uses only Mark-Sweep.

### Correct

Modern collectors combine multiple algorithms and techniques.

---

# 33. Interview Traps

## Trap 1

### Question

What is the biggest problem with Mark-Sweep?

### Answer

Fragmentation.

---

## Trap 2

### Question

Why does Mark-Compact move objects?

### Answer

To reduce fragmentation and create contiguous free memory.

---

## Trap 3

### Question

Why is copying useful for young objects?

### Answer

Because many young objects are short-lived. If only a small fraction survives, the collector can copy the small set of live objects efficiently.

---

## Trap 4

### Question

Does parallel GC mean low latency?

### Answer

No.

Parallel GC means GC work can be performed using multiple GC threads. It does not necessarily mean that application pauses are minimal.

---

## Trap 5

### Question

Does concurrent GC mean zero pause?

### Answer

No.

Concurrent collectors still have Stop-The-World phases.

---

## Trap 6

### Question

Why are remembered sets required?

### Answer

They help a collector identify relevant cross-region or cross-generation references without scanning the entire heap.

---

## Trap 7

### Question

What is a write barrier?

### Answer

It is runtime/compiler-supported bookkeeping around certain reference updates that helps the Garbage Collector maintain information required for generational or concurrent collection.

---

## Trap 8

### Question

What is evacuation?

### Answer

Evacuation is moving live objects out of a selected region or space into another region or space.

---

## Trap 9

### Question

Which algorithm eliminates fragmentation?

### Answer

Compaction and evacuation can reduce fragmentation. No single algorithm should be described as universally eliminating every form of fragmentation in every implementation.

---

## Trap 10

### Question

Is G1 simply Mark-Sweep?

### Answer

No.

G1 is a region-based Garbage Collector that combines several techniques including concurrent marking, remembered sets, and evacuation.

---

# 34. Top 15 Interview Questions

## Q1. What are the main classical GC algorithms?

### Answer

The main classical algorithms are:

```text
1. Mark-Sweep
2. Mark-Compact
3. Copying
```

---

## Q2. What is Mark-Sweep?

### Answer

Mark-Sweep identifies reachable objects during the Mark phase and reclaims memory belonging to unreachable objects during the Sweep phase.

---

## Q3. What is the major problem with Mark-Sweep?

### Answer

Fragmentation.

Free memory can become distributed between live objects.

---

## Q4. What is Mark-Compact?

### Answer

Mark-Compact identifies live objects and then moves them together to reduce fragmentation and create contiguous free space.

---

## Q5. What is the Copying algorithm?

### Answer

The Copying algorithm moves live objects from one memory space to another, leaving the original space available for reuse.

---

## Q6. Why is Copying effective for young objects?

### Answer

Because many young objects die quickly. If only a small percentage survive, only those surviving objects need to be copied.

---

## Q7. What is the difference between Mark-Sweep and Mark-Compact?

### Answer

Mark-Sweep reclaims unreachable objects but does not necessarily move live objects.

Mark-Compact additionally moves live objects to reduce fragmentation.

---

## Q8. What is the difference between Mark-Compact and Copying?

### Answer

Mark-Compact generally compacts live objects within the collection area.

Copying moves live objects from one space or region to another.

---

## Q9. What does parallel GC mean?

### Answer

It means multiple GC worker threads perform collection work simultaneously.

It does not necessarily mean application threads continue running.

---

## Q10. What does concurrent GC mean?

### Answer

Concurrent GC performs some GC work while application threads continue executing.

Its goal is generally to reduce long application pauses.

---

## Q11. What is evacuation?

### Answer

Evacuation is the process of moving live objects out of a selected region or space into another region or space.

---

## Q12. What are remembered sets?

### Answer

Remembered sets track relevant references crossing region or generation boundaries so the collector can avoid scanning unnecessary parts of the heap.

---

## Q13. What is a write barrier?

### Answer

A write barrier performs bookkeeping when certain references are updated so the Garbage Collector can maintain information needed for generational or concurrent collection.

---

## Q14. Why does generational GC work well?

### Answer

Because many objects are short-lived. Collecting young objects frequently can reclaim large amounts of memory without repeatedly processing long-lived objects.

---

## Q15. What is the difference between throughput and latency in GC?

### Answer

Throughput measures how much useful application work can be completed over time.

Latency focuses on how long the application is paused or delayed.

A collector optimized for maximum throughput may make different trade-offs from one optimized for very low pause times.

---

# 35. 30-Second Interview Answer

> GC algorithms are strategies used by the JVM to identify unreachable objects and reclaim heap memory. The classical algorithms are Mark-Sweep, Mark-Compact, and Copying. Mark-Sweep marks reachable objects and reclaims the rest but can cause fragmentation. Mark-Compact additionally moves live objects to reduce fragmentation. Copying moves live objects to another space and is especially useful when most objects in the collected area are short-lived. Modern collectors combine these techniques with generational, parallel, concurrent, region-based, and evacuation strategies to balance throughput, latency, memory usage, and pause times.

---

# 36. Cheat Sheet

```text
╔══════════════════════════════════════════════════════════╗
║                 GC ALGORITHMS CHEAT SHEET               ║
╠══════════════════════════════════════════════════════════╣
║ Mark-Sweep     → Mark live → Sweep garbage              ║
║ Main issue     → Fragmentation                          ║
║                                                          ║
║ Mark-Compact   → Mark live → Move/compact live objects  ║
║ Main benefit   → Reduced fragmentation                  ║
║                                                          ║
║ Copying        → Copy live objects to another space     ║
║ Main benefit   → Fast allocation / low fragmentation    ║
║ Main cost      → Extra destination space                ║
║                                                          ║
║ Generational   → Collect young objects more frequently  ║
║ Parallel       → Multiple GC threads                     ║
║ Concurrent     → GC overlaps with application execution ║
║ Evacuation     → Move objects out of a region            ║
║ Remembered Set → Track cross-region references          ║
║ Card Table     → Track modified memory areas            ║
║ Write Barrier  → Track relevant reference updates       ║
║                                                          ║
║ G1             → Region-based, evacuation + marking     ║
║ ZGC            → Very low-pause, highly concurrent      ║
║ Shenandoah     → Low-pause, concurrent collection      ║
║ Parallel GC    → Throughput-oriented                    ║
║ Serial GC      → Simple, single GC thread               ║
╚══════════════════════════════════════════════════════════╝
```

## 🧠 Easy Memory Trick

```text
MARK-SWEEP
"Find garbage → remove garbage"

MARK-COMPACT
"Find garbage → remove garbage → pack survivors"

COPYING
"Find survivors → copy survivors elsewhere"

PARALLEL
"Multiple GC workers"

CONCURRENT
"GC + application run together during some phases"

EVACUATION
"Move live objects out"

GENERATIONAL
"Young objects → frequent collection"
```

## 🔥 Most Important Comparison

```text
                    Mark-Sweep
                        |
                        v
                Remove Garbage
                        |
                        v
                 Fragmentation
                        |
                        v
                 Mark-Compact
                        |
                        v
              Move Live Objects
                        |
                        v
             Reduce Fragmentation


                    Copying
                        |
                        v
              Copy Live Objects
                        |
                        v
                Another Space
                        |
                        v
               Old Space Free
```

## ⭐ Final Interview Rule

> **Mark-Sweep removes garbage.**
>
> **Mark-Compact removes garbage and compacts live objects.**
>
> **Copying moves live objects to another space.**
>
> **Parallel means multiple GC workers.**
>
> **Concurrent means GC can execute alongside application threads during some phases.**
>
> **Generational means treating objects differently based on age/lifetime.**
>
> **Evacuation means moving live objects out of a selected region.**

## 🚀 One-Line Revision

```text
Mark-Sweep  → Reclaim
Mark-Compact → Reclaim + Compact
Copying     → Copy Survivors
Generational → Young + Old
Parallel    → Many GC Threads
Concurrent  → GC + Application
Evacuation  → Move Survivors
```