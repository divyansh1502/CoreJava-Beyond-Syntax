# 08 — Generational Garbage Collection

## 📚 Table of Contents

1. [Introduction](#1-introduction)
2. [Why Generational Garbage Collection?](#2-why-generational-garbage-collection)
3. [Generational Hypothesis](#3-generational-hypothesis)
4. [Heap Organization](#4-heap-organization)
5. [Young Generation](#5-young-generation)
6. [Eden Space](#6-eden-space)
7. [Survivor Spaces](#7-survivor-spaces)
8. [Old Generation](#8-old-generation)
9. [Object Lifecycle](#9-object-lifecycle)
10. [Object Age](#10-object-age)
11. [Minor GC](#11-minor-gc)
12. [Major GC](#12-major-gc)
13. [Full GC](#13-full-gc)
14. [Promotion](#14-promotion)
15. [Promotion Threshold](#15-promotion-threshold)
16. [Young GC Working](#16-young-gc-working)
17. [Promotion During Young GC](#17-promotion-during-young-gc)
18. [Inter-Generational References](#18-inter-generational-references)
19. [Remembered Sets](#19-remembered-sets)
20. [Card Tables](#20-card-tables)
21. [Write Barriers](#21-write-barriers)
22. [Old Generation Collection](#22-old-generation-collection)
23. [Why Generational GC is Efficient](#23-why-generational-gc-is-efficient)
24. [Advantages](#24-advantages)
25. [Disadvantages](#25-disadvantages)
26. [Generational GC and Different Collectors](#26-generational-gc-and-different-collectors)
27. [Common Misconceptions](#27-common-misconceptions)
28. [Interview Traps](#28-interview-traps)
29. [Top 15 Interview Questions](#29-top-15-interview-questions)
30. [30-Second Interview Answer](#30-30-second-interview-answer)
31. [Cheat Sheet](#31-cheat-sheet)

---

# 1. Introduction

Generational Garbage Collection is a Garbage Collection strategy based on the observation that many objects are short-lived.

Instead of treating every object in the heap identically, objects can be grouped according to their age or expected lifetime.

The basic idea is:

```text
New Object
    ↓
Young Generation
    ↓
Survives GC
    ↓
Survives More GCs
    ↓
Old Generation
```

### Definition

> Generational Garbage Collection divides the heap into logical generations and collects younger objects more frequently than older objects.

### Core Idea

```text
Young Objects
     ↓
Frequently Collected

Old Objects
     ↓
Collected Less Frequently
```

This allows the Garbage Collector to focus more frequently on areas where garbage is expected to be abundant.

---

# 2. Why Generational Garbage Collection?

Consider an application that continuously creates temporary objects.

```text
Object A → created
Object B → created
Object C → created
Object D → created
Object E → created
```

After a short period:

```text
A → Garbage
B → Garbage
C → Garbage
D → Live
E → Garbage
```

Most objects may die quickly.

If the JVM repeatedly processed the entire heap to find these objects, it could perform unnecessary work.

Generational GC takes advantage of this behavior.

```text
New Objects
     ↓
Young Generation
     ↓
Collect Frequently
     ↓
Most Die
     ↓
Only Survivors Continue
```

### Main Goal

Reduce unnecessary GC work by treating young and old objects differently.

---

# 3. Generational Hypothesis

Generational GC is based on the **generational hypothesis**.

The simplified idea is:

> Most newly created objects become unreachable relatively quickly, while objects that survive multiple collections are more likely to remain alive for longer.

Conceptually:

```text
Many New Objects
       |
       v
+----------------+
| Young Objects  |
+----------------+
       |
       +----> Many die
       |
       v
    Survivors
       |
       v
+----------------+
| Older Objects  |
+----------------+
       |
       v
   Long-lived
```

### Important

This is an empirical observation, not a guarantee.

An application may have unusual workloads where many young objects survive for a long time.

---

# 4. Heap Organization

A traditional generational heap can be represented as:

```text
                         HEAP
                          |
              +-----------+-----------+
              |                       |
              v                       v
      Young Generation          Old Generation
              |
       +------+------+
       |             |
       v             v
     Eden       Survivor Spaces
                  /       \
                 S0        S1
```

A simplified layout:

```text
+------------------------------------------------------+
|                      JAVA HEAP                       |
+------------------------------+-----------------------+
|      Young Generation        |    Old Generation    |
|                              |                       |
| +--------+ +------+ +------+ |                       |
| |  Eden  | |  S0  | |  S1  | |                       |
| +--------+ +------+ +------+ |                       |
+------------------------------+-----------------------+
```

### Important

The exact physical layout and implementation depend on the Garbage Collector and JDK version.

For example, modern region-based collectors such as G1 organize the heap differently from the traditional contiguous young/old layout.

---

# 5. Young Generation

The Young Generation contains relatively new objects.

Traditionally it contains:

```text
Young Generation
      |
      +---- Eden
      |
      +---- Survivor 0
      |
      +---- Survivor 1
```

### Typical Object Flow

```text
New Object
    ↓
Eden
    ↓
Young GC
    ↓
Survivor
    ↓
Another Young GC
    ↓
Survivor
    ↓
Eventually Old Generation
```

### Why Collect Young Generation Frequently?

Because many young objects become unreachable quickly.

```text
Many Objects Created
        ↓
Many Objects Die Young
        ↓
Young GC Can Reclaim Them
```

---

# 6. Eden Space

Eden is the area where newly allocated objects are commonly placed in a traditional generational collector.

Example:

```java
Employee employee = new Employee();
```

The `Employee` object is allocated on the heap, and in a traditional generational setup it would typically start in the young generation, commonly Eden.

### Conceptual Flow

```text
new Object
     ↓
   Eden
```

Suppose:

```text
Eden:

[A] [B] [C] [D] [E]
```

After some time:

```text
A → Garbage
B → Live
C → Garbage
D → Garbage
E → Live
```

A Young GC can reclaim the unreachable objects and process the survivors.

---

# 7. Survivor Spaces

Traditional generational collectors commonly use two Survivor spaces:

```text
S0
S1
```

They are often used alternately during young-generation collection.

Conceptually:

```text
Eden
  |
  | Young GC
  v
S0
  |
  | Next Young GC
  v
S1
  |
  | Next Young GC
  v
S0
```

### Important

At a given point, one Survivor space may act as the source while the other acts as the destination for surviving objects.

Example:

```text
Before GC:

Eden     → objects
S0       → existing survivors
S1       → destination

After GC:

Eden     → reclaimed
S0       → old/source role changes
S1       → new survivors
```

The roles alternate.

### Why Two Survivor Spaces?

They allow the collector to copy surviving objects between spaces while keeping the destination compact.

---

# 8. Old Generation

The Old Generation contains objects that have survived enough young-generation collections or have otherwise been allocated/promoted into an older area.

Conceptually:

```text
Young Generation
       |
       | Survivors
       v
Old Generation
```

Objects in the old generation tend to be longer-lived.

### Example

```text
Object created
     ↓
Eden
     ↓
Survives Young GC
     ↓
Survivor
     ↓
Survives more Young GCs
     ↓
Old Generation
```

### Important

The exact promotion behavior depends on the collector and JVM implementation.

---

# 9. Object Lifecycle

A simplified object lifecycle is:

```text
             Object Created
                    |
                    v
                  Eden
                    |
                    v
               Young GC
              /         \
             /           \
        Garbage        Survivor
                         |
                         v
                    Young GC
                   /         \
                  /           \
             Garbage       Survivor
                              |
                              v
                         More GC Cycles
                              |
                              v
                           Promote
                              |
                              v
                        Old Generation
```

### Key Idea

Objects do not automatically become old merely because a fixed amount of wall-clock time has passed.

Promotion is related to surviving collection cycles and collector-specific policies.

---

# 10. Object Age

Object age is conceptually related to how many collection cycles an object has survived.

Example:

```text
Age 0
 ↓
New Object

Age 1
 ↓
Survived one relevant collection

Age 2
 ↓
Survived another collection

Age 3
 ↓
Survived another collection
```

Eventually, the object may be promoted.

### Important

"Age" does not necessarily mean:

```text
Object has existed for 3 seconds
```

It generally refers to survival through relevant GC processing.

---

# 11. Minor GC

A Minor GC traditionally refers to collection of the Young Generation.

Conceptually:

```text
Young Generation
      ↓
  Young / Minor GC
      ↓
Dead objects reclaimed
Survivors processed
```

### Example

Before:

```text
Eden:

[A] [B] [C] [D] [E]

A → Dead
B → Live
C → Dead
D → Live
E → Dead
```

After collection:

```text
Survivors:

[B] [D]
```

The dead objects' memory can be reclaimed.

### Important

"Minor GC" is a traditional generational term. Modern collectors may use different terminology depending on their implementation.

---

# 12. Major GC

Major GC traditionally refers to collection involving the Old Generation.

However, the exact meaning of "Major GC" is not universally consistent across JVM implementations and collectors.

Therefore, in interviews, be careful.

### Simplified Traditional View

```text
Young GC
    ↓
Young Generation

Major GC
    ↓
Old Generation
```

### Important

Do not assume:

```text
Major GC = Full GC
```

They are not necessarily identical.

---

# 13. Full GC

A Full GC traditionally refers to a collection that processes a much larger portion of the heap, potentially including both young and old regions and other associated JVM memory-management work depending on the collector.

Conceptually:

```text
Young Generation
        +
Old Generation
        ↓
     Full GC
```

### Important

The exact scope and behavior of a Full GC depend on:

- JVM implementation
- Garbage Collector
- JDK version
- current heap state
- triggering condition

### Interview Rule

Do not define Full GC simply as:

> "GC of the entire JVM memory."

That is incorrect because areas such as Metaspace are managed differently from the Java heap.

---

# 14. Promotion

Promotion means moving a surviving object from a younger area to an older area.

Typical simplified flow:

```text
Eden
  ↓
Survivor
  ↓
Survivor
  ↓
Old Generation
```

### Why Promote?

If an object repeatedly survives Young GC, repeatedly processing it as a young object becomes less useful.

Therefore, the JVM may promote it to an older region.

---

# 15. Promotion Threshold

A promotion threshold represents the survival-age criteria used by a collector to determine when an object can be promoted.

Conceptually:

```text
Age 0 → Young
Age 1 → Young
Age 2 → Young
Age 3 → Promote
```

### Important

The actual policy is collector-dependent.

It should not be assumed that:

```text
"Every object is promoted exactly after N collections."
```

Modern collectors may use adaptive policies and other factors.

---

# 16. Young GC Working

Let's follow a simplified example.

### Initial State

```text
Eden:

[A] [B] [C] [D] [E]
```

Suppose:

```text
A → Dead
B → Live
C → Dead
D → Live
E → Dead
```

### Young GC Begins

The collector identifies surviving objects.

```text
Live:
B
D
```

They are moved into a Survivor area.

```text
Survivor:

[B] [D]
```

Eden can then be reclaimed.

```text
Eden:

[Free][Free][Free][Free][Free]
```

### After Another Young GC

Suppose:

```text
B → Dead
D → Live
```

Then:

```text
D
↓
Survivor
```

The age/survival information for `D` is updated.

---

# 17. Promotion During Young GC

Suppose an object has survived enough collections.

Before GC:

```text
Eden
   |
   +---- New Objects

S0
   |
   +---- Older Survivors

S1
   |
   +---- Destination
```

During GC:

```text
Dead Objects
     ↓
Reclaimed

Surviving Objects
     ↓
Copied / Evacuated
     ↓
Survivor or Old Generation
```

If an object qualifies for promotion:

```text
Survivor
    |
    | Promotion
    v
Old Generation
```

### Important

Promotion can happen because of age-related policies, but other collector-specific conditions can also affect placement.

---

# 18. Inter-Generational References

Consider:

```text
Old Object
    |
    | reference
    v
Young Object
```

This creates a reference from an old region to a young region.

Why is this important?

Suppose the JVM performs a Young GC.

It cannot simply inspect only Young Generation roots if old objects can point to young objects.

It needs to know about these cross-generation references.

### Problem

Scanning the entire Old Generation every Young GC would be expensive.

Therefore, JVM collectors use mechanisms such as:

```text
Write Barriers
      +
Remembered Sets
      +
Card Tables
```

to track relevant references.

---

# 19. Remembered Sets

A remembered set stores information about references from outside a collection region into that region.

Example:

```text
Old Region
    |
    | reference
    v
Young Region
```

The collector can use remembered information to identify relevant incoming references.

### Without Remembered Information

```text
Scan entire Old Generation
          ↓
Find references to Young
```

Potentially expensive.

### With Remembered Information

```text
Remembered Information
          ↓
Inspect relevant locations
          ↓
Find references to Young
```

This can reduce unnecessary scanning.

### Important

The exact remembered-set implementation varies by Garbage Collector.

---

# 20. Card Tables

A card table divides memory into smaller logical cards.

Conceptually:

```text
Heap:

+----+----+----+----+----+----+
| C1 | C2 | C3 | C4 | C5 | C6 |
+----+----+----+----+----+----+
```

Suppose a reference is modified in `C4`.

The corresponding card can be marked dirty.

```text
C1 → Clean
C2 → Clean
C3 → Clean
C4 → Dirty
C5 → Clean
C6 → Clean
```

During GC, the collector can inspect relevant dirty cards rather than blindly scanning the entire heap.

### Key Idea

```text
Reference Update
      ↓
Card Marked
      ↓
GC Uses Card Information
```

---

# 21. Write Barriers

A write barrier is bookkeeping associated with certain reference updates.

Suppose:

```java
oldObject.reference = youngObject;
```

The JVM may perform additional work around this reference write.

Conceptually:

```text
Reference Write
      ↓
Write Barrier
      ↓
Update GC Metadata
      ↓
Continue Execution
```

### Why?

The Garbage Collector needs to know about changes to the object graph.

Write barriers are important for mechanisms such as:

- remembered sets
- card tables
- concurrent marking
- generational collection

### Important

A write barrier is not a Java keyword or method that developers normally call directly.

It is JVM/runtime/GC machinery.

---

# 22. Old Generation Collection

Old-generation collection is more expensive in many generational designs because old-generation objects tend to survive longer.

Conceptually:

```text
Young Generation
      |
      | Many objects die
      v
Small number survive
      |
      v
Old Generation
      |
      | Long-lived objects
      v
Less frequent collection
```

### Why Not Collect Old Generation Every Time?

Because many objects there are likely to remain alive.

Repeatedly scanning a large set of long-lived objects can produce unnecessary work.

---

# 23. Why Generational GC is Efficient

Generational GC exploits object lifetime patterns.

Suppose:

```text
1,000,000 objects created
```

If:

```text
900,000 die young
100,000 survive
```

Then focusing collection effort on the young generation can be much more efficient than treating all 1,000,000 objects equally every time.

### Simplified Flow

```text
1,000,000 New Objects
          |
          v
   Young Generation
          |
          v
   Young GC
          |
     +----+----+
     |         |
   900K      100K
   Dead     Survive
     |         |
     v         v
 Reclaim    Survivor
               |
               v
          Eventually Old
```

### Main Principle

> Collect where garbage is most likely to be found.

---

# 24. Advantages

## 24.1 Efficient Collection of Short-Lived Objects

Young objects are collected frequently.

## 24.2 Reduced Unnecessary Scanning

Long-lived objects do not need to be processed as frequently as young objects in a traditional generational design.

## 24.3 Better GC Performance

The collector can focus resources on areas with high allocation and mortality rates.

## 24.4 Works Well With Common Application Workloads

Many applications create large numbers of temporary objects.

## 24.5 Supports Different Collection Strategies

Different generations can be handled using different policies or algorithms.

---

# 25. Disadvantages

## 25.1 Additional Complexity

The JVM must maintain:

- generations
- survivor information
- promotion information
- cross-generation references
- remembered sets or equivalent metadata
- barriers

## 25.2 Promotion Costs

Moving surviving objects between regions requires work.

## 25.3 Cross-Generation References

Old-to-young references complicate young-generation collection.

## 25.4 Not Perfect for Every Workload

If an application creates many long-lived objects immediately, the generational hypothesis may provide less benefit.

## 25.5 Additional Memory Overhead

Metadata such as remembered information and card tables consumes memory.

---

# 26. Generational GC and Different Collectors

Generational collection is a strategy, not simply the name of one Garbage Collector.

Different JVM collectors can implement generations differently.

### Traditional Generational Collectors

A classic layout may look like:

```text
Young
 |
 +-- Eden
 +-- S0
 +-- S1

Old
```

### G1

G1 divides the heap into regions.

Conceptually:

```text
+----+----+----+----+
| R1 | R2 | R3 | R4 |
+----+----+----+----+
| R5 | R6 | R7 | R8 |
+----+----+----+----+
```

Regions can be used for young and old objects according to G1's policies.

G1 combines concepts such as:

- regions
- generational collection
- remembered sets
- evacuation
- concurrent marking

### ZGC and Shenandoah

Modern low-latency collectors have historically evolved their generational capabilities differently across JDK releases.

Therefore:

> Always consider the exact JDK version and collector when discussing whether a collector uses generational collection and how it implements it.

---

# 27. Common Misconceptions

## Misconception 1

> Young Generation means objects are stored on the stack.

### Correct

Young Generation is part of the Java heap.

```text
Stack
  ≠
Heap

Young Generation
  ⊂
Heap
```

---

## Misconception 2

> Every new object always starts in Eden.

### Correct

In a traditional generational model, objects commonly begin in Eden, but allocation behavior can depend on the JVM and collector.

Large-object handling and other implementation details can change the exact path.

---

## Misconception 3

> Old objects are never collected.

### Correct

Old-generation objects can become garbage and can be collected.

```text
Old Object
    ↓
Becomes unreachable
    ↓
Garbage
    ↓
Eventually reclaimed
```

---

## Misconception 4

> Minor GC only deletes objects.

### Correct

A Young GC can:

- identify dead objects
- copy/evacuate survivors
- update object age
- promote some objects
- reclaim young-generation space

---

## Misconception 5

> Major GC and Full GC are always identical.

### Correct

Their terminology and scope vary by JVM and collector.

Do not treat them as universally interchangeable.

---

## Misconception 6

> Object age means seconds or milliseconds.

### Correct

In traditional generational GC discussions, age generally refers to survival through collection cycles.

---

## Misconception 7

> Generational GC means only two generations exist.

### Correct

A traditional model commonly discusses:

```text
Young
Old
```

with Eden and Survivor spaces inside Young.

But actual collector implementations can differ substantially.

---

## Misconception 8

> The Young Generation is physically always one contiguous block.

### Correct

That depends on the collector.

Traditional collectors and region-based collectors organize the heap differently.

---

# 28. Interview Traps

## Trap 1

### Question

Why are young objects collected frequently?

### Answer

Because many newly created objects become unreachable quickly.

---

## Trap 2

### Question

Why are old objects collected less frequently?

### Answer

Objects that survive multiple collections are more likely to remain alive, so repeatedly scanning them may provide less benefit.

---

## Trap 3

### Question

What is Eden?

### Answer

In a traditional generational layout, Eden is the primary area where newly allocated objects are placed in the Young Generation.

---

## Trap 4

### Question

What are Survivor spaces?

### Answer

They are spaces used to hold objects that survive young-generation collections in traditional generational collectors.

---

## Trap 5

### Question

What happens to a surviving object during Young GC?

### Answer

It may be copied or evacuated to a Survivor space, or promoted to the Old Generation if it satisfies the collector's promotion policy.

---

## Trap 6

### Question

What is promotion?

### Answer

Promotion is moving an object from a younger generation/region into an older one because it has survived enough collection processing or meets collector-specific criteria.

---

## Trap 7

### Question

Why are remembered sets needed?

### Answer

They help track references entering a region or generation from elsewhere so the collector does not have to scan the entire heap or old generation.

---

## Trap 8

### Question

What is the relationship between a card table and a remembered set?

### Answer

A card table is a low-level bookkeeping structure that tracks modified portions of memory. Remembered-set information can use such mechanisms to help identify relevant cross-region references.

---

## Trap 9

### Question

Does generational GC guarantee better performance?

### Answer

No.

It is based on common object-lifetime behavior and can be very effective for suitable workloads, but performance depends on the application, allocation rate, heap size, collector, and workload.

---

## Trap 10

### Question

Is generational GC itself a specific collector?

### Answer

No.

It is a garbage-collection strategy that can be implemented by different collectors.

---

# 29. Top 15 Interview Questions

## Q1. What is Generational Garbage Collection?

### Answer

Generational GC divides heap management into age-based generations and collects young objects more frequently than old objects because many objects become unreachable shortly after creation.

---

## Q2. Why does generational GC work?

### Answer

It exploits the observation that many objects are short-lived while objects surviving several collections are more likely to be long-lived.

---

## Q3. What is the Young Generation?

### Answer

It is the area of a traditional generational heap where relatively new objects are managed.

It commonly contains:

```text
Eden
Survivor 0
Survivor 1
```

---

## Q4. What is Eden Space?

### Answer

Eden is the primary allocation area for newly created objects in a traditional generational collector.

---

## Q5. What are Survivor spaces?

### Answer

Survivor spaces hold objects that survive young-generation collection and are commonly used alternately as source and destination spaces.

---

## Q6. What is Object Promotion?

### Answer

Promotion is the movement of a surviving object from a younger area to an older area according to the collector's policies.

---

## Q7. What is a Minor GC?

### Answer

Traditionally, a Minor GC refers to a collection focused on the Young Generation.

---

## Q8. What is a Major GC?

### Answer

Traditionally, Major GC refers to collection involving the Old Generation, but the exact terminology varies between JVM implementations and collectors.

---

## Q9. What is Full GC?

### Answer

Full GC generally refers to a collection involving a broad portion of the Java heap, often including both young and old areas, though the exact scope depends on the collector and JVM.

---

## Q10. Why are two Survivor spaces used?

### Answer

They allow surviving objects to be copied between spaces while keeping the destination compact and allowing the roles of the two spaces to alternate.

---

## Q11. What happens during a Young GC?

### Answer

The collector identifies unreachable young objects, reclaims their memory, processes surviving objects by copying or evacuation, updates their age, and may promote some survivors.

---

## Q12. Why are remembered sets required?

### Answer

They help identify references entering the collection region from other regions or generations without scanning all other memory.

---

## Q13. What is a write barrier?

### Answer

A write barrier is GC-related bookkeeping performed around certain reference updates to maintain information required by mechanisms such as generational collection and concurrent marking.

---

## Q14. Does Old Generation mean permanent objects?

### Answer

No.

Old-generation objects can still become unreachable and eventually be collected.

---

## Q15. What is the biggest advantage of Generational GC?

### Answer

It allows the collector to focus frequent collection effort on young objects, where garbage is often abundant, while avoiding unnecessary repeated processing of long-lived objects.

---

# 30. 30-Second Interview Answer

> Generational Garbage Collection is a strategy based on the observation that many objects are short-lived. The heap is logically divided according to object age, traditionally into Young and Old generations. New objects commonly start in Eden, and objects that survive Young GC can move through Survivor spaces and eventually be promoted to the Old Generation. Young objects are collected more frequently because they are more likely to become garbage. Mechanisms such as remembered sets, card tables, and write barriers help handle references between generations efficiently.

---

# 31. Cheat Sheet

```text
╔══════════════════════════════════════════════════════════╗
║             GENERATIONAL GC CHEAT SHEET                 ║
╠══════════════════════════════════════════════════════════╣
║ Main Idea       → Young objects die frequently          ║
║                                                          ║
║ Young Gen       → Newer objects                          ║
║ Eden            → Common initial allocation area         ║
║ Survivor        → Objects surviving Young GC             ║
║ Old Gen         → Long-lived / promoted objects          ║
║                                                          ║
║ Young GC        → Traditionally focuses on Young Gen     ║
║ Major GC        → Traditionally involves Old Gen         ║
║ Full GC         → Broad heap collection                  ║
║                                                          ║
║ Promotion       → Young → Old                            ║
║ Object Age      → Survival through GC cycles             ║
║                                                          ║
║ Remembered Set  → Tracks relevant cross-region refs      ║
║ Card Table      → Tracks modified memory portions        ║
║ Write Barrier   → GC bookkeeping on reference updates    ║
║                                                          ║
║ Main Benefit    → Focus GC where garbage is abundant     ║
║ Main Assumption → Many objects are short-lived           ║
╚══════════════════════════════════════════════════════════╝
```

## 🧠 Object Lifecycle Memory Trick

```text
NEW
 ↓
EDEN
 ↓
YOUNG GC
 ↓
SURVIVOR
 ↓
YOUNG GC
 ↓
SURVIVOR
 ↓
PROMOTION
 ↓
OLD GENERATION
 ↓
BECOMES UNREACHABLE
 ↓
OLD-GENERATION COLLECTION
 ↓
RECLAIMED
```

## 🔥 Traditional Young Generation

```text
              YOUNG GENERATION
                     |
          +----------+----------+
          |          |          |
          v          v          v
        Eden        S0         S1
          |
          | New Objects
          v
        Young GC
          |
          v
      Survivors
          |
          v
   S0 ↔ S1 alternation
          |
          v
       Promotion
          |
          v
     Old Generation
```

## 🔥 Most Important Relationships

```text
Generational GC
      |
      +---- Young Generation
      |        |
      |        +---- Eden
      |        +---- Survivor Spaces
      |
      +---- Old Generation
      |
      +---- Promotion
      |
      +---- Remembered Sets
      |
      +---- Card Tables
      |
      +---- Write Barriers
```

## ⭐ Final Interview Rule

```text
Generational GC
→ Divide objects by age/lifetime

Young
→ Many objects die quickly

Eden
→ Common initial allocation area

Survivor
→ Holds surviving young objects

Promotion
→ Move survivors toward Old

Old
→ Long-lived objects

Young GC
→ Frequently handles young objects

Remembered Set
→ Track relevant cross-generation/region references

Card Table
→ Track modified memory portions

Write Barrier
→ Maintain GC bookkeeping when references change
```

## 🚀 One-Line Revision

```text
Young objects → Collect frequently
Survivors     → Survivor spaces
Long-lived    → Old Generation
Promotion     → Young → Old
Old → Young references → Remembered information
Reference updates → Write barriers
```