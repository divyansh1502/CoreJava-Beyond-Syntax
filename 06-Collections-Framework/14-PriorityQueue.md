# 14 — PriorityQueue ⭐

> **Package:** `java.util`  
> **Type:** Class  
> **Implements:** `Queue<E>`  
> **Main purpose:** Processes elements according to priority rather than insertion order  
> **Underlying structure:** Heap  
> **Default ordering:** Natural ordering

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [What is PriorityQueue?](#2-what-is-priorityqueue)
3. [Why PriorityQueue Exists](#3-why-priorityqueue-exists)
4. [PriorityQueue Hierarchy](#4-priorityqueue-hierarchy)
5. [FIFO vs Priority-Based Processing](#5-fifo-vs-priority-based-processing)
6. [Creating a PriorityQueue](#6-creating-a-priorityqueue)
7. [Adding Elements](#7-adding-elements)
8. [Examining the Head](#8-examining-the-head)
9. [Removing Elements](#9-removing-elements)
10. [Core Methods](#10-core-methods)
11. [PriorityQueue Method Summary](#11-priorityqueue-method-summary)
12. [Natural Ordering](#12-natural-ordering)
13. [Custom Ordering](#13-custom-ordering)
14. [PriorityQueue with Integers](#14-priorityqueue-with-integers)
15. [PriorityQueue with Strings](#15-priorityqueue-with-strings)
16. [PriorityQueue with Custom Objects](#16-priorityqueue-with-custom-objects)
17. [Comparable with PriorityQueue](#17-comparable-with-priorityqueue)
18. [Comparator with PriorityQueue](#18-comparator-with-priorityqueue)
19. [Min Heap](#19-min-heap)
20. [Max Heap](#20-max-heap)
21. [How to Create a Max Heap](#21-how-to-create-a-max-heap)
22. [Internal Working](#22-internal-working)
23. [Heap Structure](#23-heap-structure)
24. [Array Representation of Heap](#24-array-representation-of-heap)
25. [Insertion Internal Working](#25-insertion-internal-working)
26. [Removal Internal Working](#26-removal-internal-working)
27. [Why peek() is O(1)](#27-why-peek-is-o1)
28. [Time Complexity](#28-time-complexity)
29. [Space Complexity](#29-space-complexity)
30. [PriorityQueue and Duplicates](#30-priorityqueue-and-duplicates)
31. [PriorityQueue and null](#31-priorityqueue-and-null)
32. [PriorityQueue and Iteration](#32-priorityqueue-and-iteration)
33. [PriorityQueue and Sorting](#33-priorityqueue-and-sorting)
34. [PriorityQueue vs Queue](#34-priorityqueue-vs-queue)
35. [PriorityQueue vs ArrayDeque](#35-priorityqueue-vs-arraydeque)
36. [PriorityQueue vs TreeSet](#36-priorityqueue-vs-treeset)
37. [PriorityQueue vs Sorting](#37-priorityqueue-vs-sorting)
38. [When to Use PriorityQueue](#38-when-to-use-priorityqueue)
39. [When Not to Use PriorityQueue](#39-when-not-to-use-priorityqueue)
40. [Real-World Applications](#40-real-world-applications)
41. [DSA Patterns](#41-dsa-patterns)
42. [DSA Pattern 1 — Kth Largest](#42-dsa-pattern-1--kth-largest)
43. [DSA Pattern 2 — Kth Smallest](#43-dsa-pattern-2--kth-smallest)
44. [DSA Pattern 3 — Top K Elements](#44-dsa-pattern-3--top-k-elements)
45. [DSA Pattern 4 — Merge K Sorted Lists](#45-dsa-pattern-4--merge-k-sorted-lists)
46. [DSA Pattern 5 — K Closest Elements](#46-dsa-pattern-5--k-closest-elements)
47. [DSA Pattern 6 — Two Heaps](#47-dsa-pattern-6--two-heaps)
48. [DSA Pattern 7 — Running Median](#48-dsa-pattern-7--running-median)
49. [DSA Pattern 8 — Scheduling](#49-dsa-pattern-8--scheduling)
50. [DSA Pattern 9 — Dijkstra](#50-dsa-pattern-9--dijkstra)
51. [DSA Pattern 10 — Greedy Algorithms](#51-dsa-pattern-10--greedy-algorithms)
52. [Common Mistakes](#52-common-mistakes)
53. [Interview Traps](#53-interview-traps)
54. [Top Interview Questions](#54-top-interview-questions)
55. [30-Second Interview Answer](#55-30-second-interview-answer)
56. [Cheat Sheet](#56-cheat-sheet)
57. [Quick Revision](#57-quick-revision)

---

# 1. Introduction ⭐

`PriorityQueue` is a class in Java's Collections Framework.

It implements:

    Queue<E>

Unlike a normal FIFO Queue, `PriorityQueue` processes elements according to their priority.

For example:

    Insert:

    30
    10
    20

A normal FIFO Queue gives:

    30
    10
    20

But a `PriorityQueue<Integer>` using natural ordering gives:

    10
    20
    30

when repeatedly removed.

The important idea is:

    Queue
       |
       +-- FIFO Queue
       |
       +-- Priority Queue

---

# 2. What is PriorityQueue? 🎯

`PriorityQueue` is a Queue implementation where the head is determined by priority.

By default, the smallest element has the highest priority when natural ordering is used.

Example:

    PriorityQueue<Integer> pq =
        new PriorityQueue<>();

    pq.offer(30);
    pq.offer(10);
    pq.offer(20);

    System.out.println(pq.poll());

Output:

    10

Then:

    System.out.println(pq.poll());

Output:

    20

Then:

    System.out.println(pq.poll());

Output:

    30

Therefore:

    10 -> 20 -> 30

is the removal order.

---

# 3. Why PriorityQueue Exists 🧠

A normal Queue answers:

> Who arrived first?

A PriorityQueue answers:

> Who has the highest priority?

Example:

    Normal Queue:

    Task A
    Task B
    Task C

Processing:

    A
    B
    C

Priority Queue:

    Task A -> priority 5
    Task B -> priority 1
    Task C -> priority 3

Processing may be:

    B
    C
    A

The ordering is determined by the priority rule.

---

# 4. PriorityQueue Hierarchy 🌳

The hierarchy is:

    Iterable
       |
    Collection
       |
      Queue
       |
    PriorityQueue

`PriorityQueue` is a concrete class.

Conceptually:

    Collection<E>
          |
        Queue<E>
          |
    PriorityQueue<E>

---

# 5. FIFO vs Priority-Based Processing ⚖️

Normal Queue:

    offer(30)
    offer(10)
    offer(20)

FIFO removal:

    30
    10
    20

PriorityQueue:

    offer(30)
    offer(10)
    offer(20)

Natural ordering removal:

    10
    20
    30

Therefore:

    Queue
        -> usually FIFO

    PriorityQueue
        -> priority based

---

# 6. Creating a PriorityQueue 💻

## Basic

    PriorityQueue<Integer> pq =
        new PriorityQueue<>();

---

## With Initial Capacity

    PriorityQueue<Integer> pq =
        new PriorityQueue<>(20);

The number represents the initial capacity of the internal storage.

It does not mean:

    maximum size = 20

The queue can grow beyond this capacity.

---

## With Comparator

    PriorityQueue<Integer> pq =
        new PriorityQueue<>(
            Comparator.reverseOrder()
        );

This creates max-heap-like behavior for integers.

---

## From Another Collection

    List<Integer> numbers =
        List.of(30, 10, 20);

    PriorityQueue<Integer> pq =
        new PriorityQueue<>(
            numbers
        );

The elements are inserted into the PriorityQueue according to its heap structure.

---

# 7. Adding Elements ➕

The main insertion methods are:

    add()
    offer()

Example:

    PriorityQueue<Integer> pq =
        new PriorityQueue<>();

    pq.add(30);
    pq.add(10);
    pq.add(20);

or:

    pq.offer(30);
    pq.offer(10);
    pq.offer(20);

Both insert elements.

For an unbounded `PriorityQueue`, insertion normally succeeds.

---

# 8. Examining the Head 👀

Use:

    peek()

Example:

    PriorityQueue<Integer> pq =
        new PriorityQueue<>();

    pq.offer(30);
    pq.offer(10);
    pq.offer(20);

    System.out.println(
        pq.peek()
    );

Output:

    10

Important:

    peek()

does not remove the element.

Queue remains logically:

    [10, 20, 30]

---

# 9. Removing Elements ➖

Use:

    poll()

Example:

    PriorityQueue<Integer> pq =
        new PriorityQueue<>();

    pq.offer(30);
    pq.offer(10);
    pq.offer(20);

    System.out.println(
        pq.poll()
    );

Output:

    10

Then:

    pq.poll()
        -> 20

Then:

    pq.poll()
        -> 30

---

## remove()

`remove()` can also remove the head.

    pq.remove();

Difference:

    poll()
        -> null when empty

    remove()
        -> exception when empty

---

# 10. Core Methods 🛠️

## add()

    pq.add(10);

Adds an element.

---

## offer()

    pq.offer(10);

Adds an element.

---

## peek()

    pq.peek();

Returns the head without removing it.

---

## poll()

    pq.poll();

Removes and returns the head.

---

## remove()

    pq.remove();

Removes the head.

---

## contains()

    pq.contains(10);

Checks whether an element exists.

---

## size()

    pq.size();

Returns the number of elements.

---

## isEmpty()

    pq.isEmpty();

Checks whether the queue is empty.

---

## clear()

    pq.clear();

Removes all elements.

---

# 11. PriorityQueue Method Summary 📋

| Method | Purpose | Typical Complexity |
|---|---|---:|
| add() | Insert | O(log n) |
| offer() | Insert | O(log n) |
| peek() | View head | O(1) |
| poll() | Remove head | O(log n) |
| remove() | Remove head | O(log n) |
| contains() | Search | O(n) |
| size() | Size | O(1) |
| isEmpty() | Empty check | O(1) |
| clear() | Remove all | O(n) |

---

# 12. Natural Ordering 🔢

If no Comparator is supplied, PriorityQueue uses natural ordering.

For integers:

    10 < 20 < 30

Therefore:

    10

has the highest priority.

Example:

    PriorityQueue<Integer> pq =
        new PriorityQueue<>();

    pq.offer(50);
    pq.offer(10);
    pq.offer(30);
    pq.offer(20);

Repeated polling:

    10
    20
    30
    50

---

## Strings

Strings use their natural ordering.

Example:

    PriorityQueue<String> pq =
        new PriorityQueue<>();

    pq.offer("Dog");
    pq.offer("Apple");
    pq.offer("Cat");

Polling according to natural String ordering:

    Apple
    Cat
    Dog

---

# 13. Custom Ordering 🔧

We can provide a `Comparator`.

Example:

    PriorityQueue<Integer> pq =
        new PriorityQueue<>(
            Comparator.reverseOrder()
        );

Now:

    30
    20
    10

will be removed in that order.

Example:

    pq.offer(10);
    pq.offer(30);
    pq.offer(20);

    System.out.println(
        pq.poll()
    );

Output:

    30

---

# 14. PriorityQueue with Integers 🔢

## Min Heap

    PriorityQueue<Integer> pq =
        new PriorityQueue<>();

    pq.offer(40);
    pq.offer(10);
    pq.offer(30);
    pq.offer(20);

    while (!pq.isEmpty()) {

        System.out.println(
            pq.poll()
        );
    }

Output:

    10
    20
    30
    40

---

# 15. PriorityQueue with Strings 🔤

    PriorityQueue<String> pq =
        new PriorityQueue<>();

    pq.offer("Java");
    pq.offer("C");
    pq.offer("Python");
    pq.offer("Go");

    while (!pq.isEmpty()) {

        System.out.println(
            pq.poll()
        );
    }

Natural ordering determines the priority.

---

# 16. PriorityQueue with Custom Objects 👨‍💻

Suppose:

    Student

has:

    name
    marks

We can define priority using a Comparator.

Example:

    class Student {

        String name;
        int marks;

        Student(
            String name,
            int marks
        ) {

            this.name = name;
            this.marks = marks;
        }
    }

Then:

    PriorityQueue<Student> pq =
        new PriorityQueue<>(
            (a, b) ->
                Integer.compare(
                    a.marks,
                    b.marks
                )
        );

Now the student with the lowest marks will be at the head.

---

# 17. Comparable with PriorityQueue 🔄

A class can implement:

    Comparable<T>

Then define:

    compareTo()

Example:

    class Student
        implements Comparable<Student> {

        String name;
        int marks;

        Student(
            String name,
            int marks
        ) {

            this.name = name;
            this.marks = marks;
        }

        @Override
        public int compareTo(
            Student other
        ) {

            return Integer.compare(
                this.marks,
                other.marks
            );
        }
    }

Then:

    PriorityQueue<Student> pq =
        new PriorityQueue<>();

The PriorityQueue uses:

    compareTo()

to determine priority.

---

# 18. Comparator with PriorityQueue 🔧

Comparator allows priority to be defined externally.

Example:

    class Student {

        String name;
        int marks;

        Student(
            String name,
            int marks
        ) {

            this.name = name;
            this.marks = marks;
        }
    }

Create:

    PriorityQueue<Student> pq =
        new PriorityQueue<>(
            (a, b) ->
                Integer.compare(
                    b.marks,
                    a.marks
                )
        );

This creates highest-marks-first behavior.

Example:

    Student A -> 80
    Student B -> 95
    Student C -> 70

Polling:

    B
    A
    C

---

# 19. Min Heap 📉

A min heap keeps the smallest element at the root.

Example:

             10
            /  \
          20    30
         / \
        40  50

The root:

    10

is the minimum.

Java's default:

    PriorityQueue<Integer>

behaves like a min heap for integers.

---

# 20. Max Heap 📈

A max heap keeps the largest element at the root.

Example:

             50
            /  \
          40    30
         / \
        20  10

The root:

    50

is the maximum.

Java's PriorityQueue is naturally min-oriented for types with ascending natural ordering.

To create max-heap behavior:

    Comparator.reverseOrder()

---

# 21. How to Create a Max Heap 💡

For integers:

    PriorityQueue<Integer> maxHeap =
        new PriorityQueue<>(
            Comparator.reverseOrder()
        );

Example:

    maxHeap.offer(10);
    maxHeap.offer(30);
    maxHeap.offer(20);

    System.out.println(
        maxHeap.poll()
    );

Output:

    30

Repeated:

    30
    20
    10

---

# 22. Internal Working ⚙️

The most important internal concept is:

    PriorityQueue
          |
          v
        Heap
          |
          v
       Array

Java's `PriorityQueue` is backed by an array-based heap.

It is not implemented as:

    sorted array

and it is not implemented as:

    balanced BST

Its primary structure is a heap.

---

# 23. Heap Structure 🌳

A binary heap is a complete binary tree.

Example:

             10
            /  \
          20    30
         / \    /
        40 50  60

Complete tree means:

- Every level except possibly the last is full.
- The last level is filled from left to right.

---

## Min Heap Property

For every parent:

    parent <= children

Example:

             10
            /  \
          20    30
         / \
        40  50

---

## Max Heap Property

For every parent:

    parent >= children

Example:

             50
            /  \
          40    30
         / \
        20  10

---

# 24. Array Representation of Heap 📦

A heap does not need explicit tree nodes.

It can be represented using an array.

Example:

    Tree:

             10
            /  \
          20    30
         / \
        40  50

Array:

    [10, 20, 30, 40, 50]

For zero-based indexing:

    Parent:

    (i - 1) / 2

    Left child:

    2 * i + 1

    Right child:

    2 * i + 2

These formulas are extremely important for understanding heap implementation.

---

# 25. Insertion Internal Working 🔼

Suppose:

    PriorityQueue:

    [10, 20, 30]

Add:

    5

Initially:

    [10, 20, 30, 5]

The new element is placed at the end.

Then heap ordering is restored.

The process is called:

    sift up

or:

    bubble up

Conceptually:

    5
    |
    v

    compare with parent

If:

    child < parent

swap.

Eventually:

             5
            / \
          10   30
          /
        20

The operation takes:

    O(log n)

because the element may travel from a leaf to the root.

---

# 26. Removal Internal Working 🔽

Suppose:

             10
            /  \
          20    30
         / \
        40  50

Remove:

    10

The last element is moved to the root:

             50
            /  \
          20    30
         /
        40

Now heap property is violated.

The element is moved downward.

This process is called:

    sift down

or:

    bubble down

Eventually:

             20
            /  \
          40    30
              

The operation takes:

    O(log n)

---

# 27. Why peek() is O(1) ⚡

In a heap:

    root
      =
    highest-priority element

The root is stored at:

    index 0

Therefore:

    peek()

can directly access the root.

No traversal is required.

Hence:

    peek()
        -> O(1)

---

# 28. Time Complexity ⏱️

| Operation | Complexity |
|---|---:|
| offer() | O(log n) |
| add() | O(log n) |
| peek() | O(1) |
| poll() | O(log n) |
| remove() | O(log n) |
| contains() | O(n) |
| size() | O(1) |
| isEmpty() | O(1) |
| clear() | O(n) |

Important:

> PriorityQueue gives fast access to the highest-priority element, not fast arbitrary searching.

---

# 29. Space Complexity 💾

For `n` elements:

    Space = O(n)

because the heap stores all elements.

---

# 30. PriorityQueue and Duplicates 🔁

PriorityQueue allows duplicates.

Example:

    PriorityQueue<Integer> pq =
        new PriorityQueue<>();

    pq.offer(10);
    pq.offer(10);
    pq.offer(20);

Valid.

Polling:

    10
    10
    20

Unlike:

    TreeSet

PriorityQueue does not automatically remove duplicates.

---

# 31. PriorityQueue and null ⚠️

`PriorityQueue` does not permit `null` elements.

Example:

    PriorityQueue<Integer> pq =
        new PriorityQueue<>();

    pq.offer(null);

This throws:

    NullPointerException

Why?

Because PriorityQueue needs to compare elements to determine their ordering.

`null` has no natural ordering.

---

# 32. PriorityQueue and Iteration 🔄

This is one of the most important interview traps.

Suppose:

    PriorityQueue<Integer> pq =
        new PriorityQueue<>();

    pq.offer(30);
    pq.offer(10);
    pq.offer(20);
    pq.offer(5);

You might expect iteration to produce:

    5
    10
    20
    30

But this is NOT guaranteed.

Example:

    for (int value : pq) {

        System.out.println(value);
    }

The iteration order follows the internal heap structure, not sorted order.

---

## To Get Priority Order

Use:

    while (!pq.isEmpty()) {

        System.out.println(
            pq.poll()
        );
    }

Now elements are removed according to priority.

---

# 33. PriorityQueue and Sorting 🔢

PriorityQueue is not a sorting data structure.

Its purpose is:

    efficiently access
    highest-priority element

If you repeatedly poll:

    O(n log n)

you can obtain elements in priority order.

But if you simply want to sort an array:

    Arrays.sort()

is usually the direct tool.

---

# 34. PriorityQueue vs Queue ⚔️

| Feature | Queue | PriorityQueue |
|---|---|---|
| Type | Interface | Class |
| Common order | FIFO | Priority |
| Main idea | Arrival order | Priority |
| Example | ArrayDeque | PriorityQueue |
| BFS | Yes | Usually no |
| Heap | Not required | Yes |

Important:

    Queue
        =
    abstraction

    PriorityQueue
        =
    implementation of Queue

---

# 35. PriorityQueue vs ArrayDeque ⚔️

| Feature | ArrayDeque | PriorityQueue |
|---|---|---|
| Main behavior | FIFO/Deque | Priority |
| Internal structure | Resizable array/deque structure | Heap |
| offer() | O(1) amortized | O(log n) |
| poll() | O(1) | O(log n) |
| peek() | O(1) | O(1) |
| Null | Not allowed | Not allowed |
| BFS | Excellent fit | Not appropriate |
| Priority processing | No | Yes |

---

# 36. PriorityQueue vs TreeSet ⚔️

Both can provide ordered access, but their purposes are different.

| Feature | PriorityQueue | TreeSet |
|---|---|---|
| Duplicates | Allowed | Not allowed |
| Main structure | Heap | Red-Black Tree |
| Get minimum | O(1) peek | O(log n) first |
| Remove minimum | O(log n) | O(log n) |
| Search | O(n) | O(log n) |
| Fully sorted iteration | No | Yes |
| Main purpose | Priority processing | Sorted unique data |

Important:

> PriorityQueue is not a replacement for TreeSet.

---

# 37. PriorityQueue vs Sorting ⚖️

Suppose you have:

    [30, 10, 20, 50, 40]

If you need every element sorted:

    Arrays.sort()

may be appropriate.

If you repeatedly need:

    smallest element
    process it
    add new elements
    process next smallest

PriorityQueue is usually the better abstraction.

Example:

    while (!pq.isEmpty()) {

        int current =
            pq.poll();

        // process current
    }

---

# 38. When to Use PriorityQueue 🎯

Use PriorityQueue when you repeatedly need the:

    minimum
    maximum
    highest priority
    lowest priority

while elements are dynamically added or removed.

Typical situations:

    Top K problems
    Kth largest
    Kth smallest
    merge K sorted lists
    scheduling
    Dijkstra
    Prim's algorithm
    greedy algorithms
    running median
    closest elements

---

# 39. When Not to Use PriorityQueue 🚫

## Need FIFO

Use:

    ArrayDeque

---

## Need Random Access

Use:

    ArrayList

---

## Need Sorted Unique Elements

Use:

    TreeSet

---

## Need Fast Key-Value Lookup

Use:

    HashMap

---

## Need Fully Sorted Data

Use:

    Arrays.sort()
    Collections.sort()
    TreeSet

depending on the requirement.

---

# 40. Real-World Applications 🌍

## CPU Scheduling

Higher-priority tasks can be processed first.

---

## Hospital Emergency Systems

Patients can be processed according to priority.

---

## Network Routing

Priority-based processing can be used for certain routing/scheduling systems.

---

## Event Simulation

Events can be processed according to timestamp.

Example:

    Event A -> 10:00
    Event B -> 09:30
    Event C -> 11:00

Priority can be:

    earliest timestamp

---

## Task Scheduling

Tasks can be processed based on:

    urgency
    deadline
    importance

---

# 41. DSA Patterns 🧩

PriorityQueue is extremely important in DSA.

Major patterns:

    1. Kth Largest
    2. Kth Smallest
    3. Top K Elements
    4. Merge K Sorted Lists
    5. K Closest Elements
    6. Two Heaps
    7. Running Median
    8. Scheduling
    9. Dijkstra
    10. Greedy Algorithms

The most important mental trigger is:

> "I repeatedly need the smallest/largest element."

Think:

    Heap
       |
    PriorityQueue

---

# 42. DSA Pattern 1 — Kth Largest 🔥

Suppose:

    [3, 2, 1, 5, 6, 4]

Find:

    2nd largest

Instead of sorting the entire array, maintain a min heap of size `k`.

For:

    k = 2

Process elements.

Maintain:

    [3, 5]

Then:

    6

enters and the smallest is removed:

    [5, 6]

Answer:

    5

Pattern:

    Min Heap
       +
    size k
       =
    Kth Largest

Code:

    PriorityQueue<Integer> pq =
        new PriorityQueue<>();

    for (int num : nums) {

        pq.offer(num);

        if (pq.size() > k) {

            pq.poll();
        }
    }

    int answer = pq.peek();

---

# 43. DSA Pattern 2 — Kth Smallest 🔥

For kth smallest, use a max heap of size `k`.

Example:

    [3, 2, 1, 5, 6, 4]

Find:

    2nd smallest

Maintain:

    Max Heap

When size exceeds `k`:

    poll()

the largest.

The root finally represents:

    kth smallest

Code:

    PriorityQueue<Integer> pq =
        new PriorityQueue<>(
            Comparator.reverseOrder()
        );

    for (int num : nums) {

        pq.offer(num);

        if (pq.size() > k) {

            pq.poll();
        }
    }

    int answer = pq.peek();

---

# 44. DSA Pattern 3 — Top K Elements 🏆

Suppose:

    [10, 5, 20, 8, 30]

Find:

    top 2 largest

Use:

    min heap
    size = k

Why?

The smallest among the top `k` stays at the root.

When a better element arrives:

    poll()

the weakest top-k candidate.

---

# 45. DSA Pattern 4 — Merge K Sorted Lists 🔗

Suppose:

    List 1:
    1 -> 4 -> 7

    List 2:
    2 -> 5 -> 8

    List 3:
    3 -> 6 -> 9

We want:

    1 2 3 4 5 6 7 8 9

A PriorityQueue can store the current smallest element from each list.

Initially:

    1
    2
    3

Poll:

    1

Then insert:

    4

Queue:

    2
    3
    4

Continue.

Pattern:

    K sorted sources
        +
    PriorityQueue
        =
    Merge K sorted structures

---

# 46. DSA Pattern 5 — K Closest Elements 📍

Suppose:

    target = 10

Elements:

    2, 5, 8, 11, 15

We may want:

    k closest

Priority can be based on:

    absolute distance

    Math.abs(value - target)

A Comparator can define the required ordering.

---

# 47. DSA Pattern 6 — Two Heaps 🧠

Some problems require two PriorityQueues.

Typical setup:

    Max Heap
        |
    smaller half

    Min Heap
        |
    larger half

Conceptually:

          Median
             |
       +-----+-----+
       |           |
    Max Heap    Min Heap
    smaller     larger
     half        half

This pattern is extremely important for:

    running median

---

# 48. DSA Pattern 7 — Running Median 📊

Suppose numbers arrive one by one:

    10
    20
    30
    40

We want the median after each insertion.

Use:

    max heap
        ->
    smaller half

    min heap
        ->
    larger half

Maintain balanced sizes.

Example:

    Max Heap:
    [10, 20]

    Min Heap:
    [30, 40]

Median:

    (20 + 30) / 2

The exact balancing logic depends on implementation.

---

# 49. DSA Pattern 8 — Scheduling 📅

Suppose tasks have deadlines or priorities.

PriorityQueue can always provide the next task according to the chosen rule.

Example:

    task:
        deadline

Comparator:

    earliest deadline first

Then:

    pq.poll()

returns the next task to process.

---

# 50. DSA Pattern 9 — Dijkstra 🗺️

Dijkstra's algorithm repeatedly chooses the unprocessed node with the smallest known distance.

This is exactly what a min heap provides.

Conceptually:

    distance
        |
        v
    PriorityQueue
        |
        v
    minimum distance node

Typical Java representation:

    PriorityQueue<int[]> pq =
        new PriorityQueue<>(
            (a, b) ->
                Integer.compare(
                    a[1],
                    b[1]
                )
        );

Here:

    a[0] -> node
    a[1] -> distance

---

# 51. DSA Pattern 10 — Greedy Algorithms 🧠

Many greedy problems repeatedly select:

    smallest
    largest
    earliest
    cheapest
    highest priority

PriorityQueue can efficiently provide that element.

Examples:

    Huffman Coding
    meeting scheduling variants
    task scheduling
    minimum cost problems

Mental trigger:

> Repeatedly select the best currently available option.

Consider:

    PriorityQueue

---

# 52. Common Mistakes ⚠️

## Mistake 1 — Thinking PriorityQueue Is Sorted

It is not a sorted collection.

It is a heap-based priority structure.

---

## Mistake 2 — Assuming Iteration Is Sorted

It is not guaranteed.

Use:

    poll()

to retrieve elements according to priority.

---

## Mistake 3 — Assuming FIFO

PriorityQueue is not normal FIFO.

---

## Mistake 4 — Adding null

Not allowed.

---

## Mistake 5 — Forgetting Comparator Direction

This:

    Comparator.naturalOrder()

gives min-oriented behavior.

This:

    Comparator.reverseOrder()

gives max-oriented behavior.

---

## Mistake 6 — Using PriorityQueue for Arbitrary Search

`contains()` is:

    O(n)

PriorityQueue is optimized around the head, not arbitrary lookup.

---

## Mistake 7 — Using PriorityQueue When Full Sorting Is Needed

If you simply need sorted output and have all elements available, direct sorting may be simpler.

---

# 53. Interview Traps 🎤

## Trap 1

> Is PriorityQueue an interface?

No.

It is a class.

---

## Trap 2

> Does PriorityQueue implement Queue?

Yes.

    PriorityQueue<E>
        implements Queue<E>

---

## Trap 3

> Does PriorityQueue follow FIFO?

No.

It follows its priority ordering.

---

## Trap 4

> What is the default PriorityQueue behavior?

Natural ordering.

For integers:

    smallest first

---

## Trap 5

> What is the underlying data structure?

A heap, backed by an array.

---

## Trap 6

> Is it a min heap or max heap by default?

For naturally ordered numbers:

    Min Heap

---

## Trap 7

> How do you create a max heap?

    PriorityQueue<Integer> pq =
        new PriorityQueue<>(
            Comparator.reverseOrder()
        );

---

## Trap 8

> Is peek() O(log n)?

No.

    peek()
        -> O(1)

---

## Trap 9

> Is poll() O(1)?

No.

    poll()
        -> O(log n)

because heap reorganization may be required.

---

## Trap 10

> Is contains() O(log n)?

No.

Generally:

    O(n)

because arbitrary elements are not indexed by value.

---

## Trap 11

> Can PriorityQueue contain duplicates?

Yes.

---

## Trap 12

> Can PriorityQueue contain null?

No.

---

## Trap 13

> Does iteration give sorted order?

No.

---

## Trap 14

> What is the best structure for Kth Largest?

A common approach:

    Min Heap
    size k

---

## Trap 15

> What is the best structure for Kth Smallest?

A common approach:

    Max Heap
    size k

---

# 54. Top Interview Questions 🎤

## Q1. What is PriorityQueue?

`PriorityQueue` is a class implementing `Queue` that processes elements according to priority rather than insertion order.

---

## Q2. What is the default ordering?

Natural ordering.

---

## Q3. What is the underlying data structure?

Array-backed binary heap.

---

## Q4. Is PriorityQueue a min heap by default?

For naturally ordered elements such as integers, yes.

---

## Q5. How do you create a max heap?

    new PriorityQueue<>(
        Comparator.reverseOrder()
    );

---

## Q6. What is the complexity of offer()?

    O(log n)

---

## Q7. What is the complexity of poll()?

    O(log n)

---

## Q8. What is the complexity of peek()?

    O(1)

---

## Q9. What is the complexity of contains()?

    O(n)

---

## Q10. Does PriorityQueue allow duplicates?

Yes.

---

## Q11. Does PriorityQueue allow null?

No.

---

## Q12. Is PriorityQueue FIFO?

No.

---

## Q13. Is PriorityQueue sorted internally?

No.

It maintains heap order, not complete sorted order.

---

## Q14. Does iteration return sorted elements?

No.

---

## Q15. How can you get elements in priority order?

Repeatedly call:

    poll()

---

## Q16. What is a min heap?

A heap where the smallest element is at the root.

---

## Q17. What is a max heap?

A heap where the largest element is at the root.

---

## Q18. How is a heap represented?

Typically using an array.

---

## Q19. Why is peek() O(1)?

The root is directly available at index 0.

---

## Q20. Why is insertion O(log n)?

The inserted element may move upward through the heap.

---

## Q21. Why is poll() O(log n)?

After removing the root, the heap may need to restore its property through sift-down.

---

## Q22. What is Kth Largest using a heap?

Use a min heap of size `k`.

---

## Q23. What is Kth Smallest using a heap?

Use a max heap of size `k`.

---

## Q24. What is the two-heaps pattern?

Using:

    Max Heap
    +
    Min Heap

to maintain two partitions, commonly for median problems.

---

## Q25. Which graph algorithm commonly uses PriorityQueue?

    Dijkstra's Algorithm

---

## Q26. Which greedy algorithm commonly uses PriorityQueue?

    Huffman Coding

---

## Q27. What is the difference between PriorityQueue and TreeSet?

PriorityQueue allows duplicates and is heap-based; TreeSet stores unique elements in sorted tree order.

---

## Q28. What is the difference between PriorityQueue and ArrayDeque?

PriorityQueue provides priority-based removal; ArrayDeque provides efficient deque/FIFO/LIFO operations.

---

## Q29. Can we use Comparator with PriorityQueue?

Yes.

---

## Q30. Can custom objects be stored in PriorityQueue?

Yes, if a Comparator is supplied or the objects have a compatible natural ordering through Comparable.

---

# 55. 30-Second Interview Answer 🎯

If the interviewer asks:

> "Explain PriorityQueue in Java."

Answer:

> "`PriorityQueue` is a class in Java's Collections Framework that implements `Queue` and processes elements according to priority rather than insertion order. By default, it uses natural ordering and is min-oriented for naturally ordered values. Internally, it uses an array-backed heap. `peek()` takes O(1), while `offer()` and `poll()` generally take O(log n). It allows duplicates but does not allow null. A Comparator can be supplied to customize the priority, such as creating max-heap behavior. In DSA, PriorityQueue is commonly used for Kth largest/smallest, Top K problems, merging K sorted lists, running median, Dijkstra, and greedy algorithms."

---

# 56. Cheat Sheet 📋

## Basic

    PriorityQueue<E>

    implements:

    Queue<E>

---

## Default Behavior

    Natural Ordering

For integers:

    smallest
        ->
    highest priority

---

## Internal Structure

    PriorityQueue
          |
          v
        Heap
          |
          v
        Array

---

## Min Heap

    PriorityQueue<Integer> pq =
        new PriorityQueue<>();

---

## Max Heap

    PriorityQueue<Integer> pq =
        new PriorityQueue<>(
            Comparator.reverseOrder()
        );

---

## Main Methods

    offer()
        -> insert

    peek()
        -> inspect priority element

    poll()
        -> remove priority element

---

## Complexity

    offer()
        -> O(log n)

    poll()
        -> O(log n)

    peek()
        -> O(1)

    contains()
        -> O(n)

---

## Properties

    duplicates?
        -> Yes

    null?
        -> No

    FIFO?
        -> No

    sorted iteration?
        -> No

---

## DSA Patterns

    Kth Largest
        -> Min Heap of size k

    Kth Smallest
        -> Max Heap of size k

    Top K
        -> Heap of size k

    Running Median
        -> Two Heaps

    Merge K Lists
        -> Min Heap

    Dijkstra
        -> Min Heap

    Greedy
        -> PriorityQueue

---

# 57. Quick Revision ⚡

Remember:

    1. PriorityQueue is a class.

    2. It implements Queue.

    3. It is priority-based.

    4. It does not guarantee FIFO.

    5. Default ordering is natural ordering.

    6. Integer PriorityQueue is min-oriented by default.

    7. PriorityQueue is backed by a heap.

    8. The heap is represented using an array.

    9. peek() gives the highest-priority element.

    10. peek() does not remove the element.

    11. poll() removes the highest-priority element.

    12. offer() inserts an element.

    13. Duplicates are allowed.

    14. null is not allowed.

    15. peek() is O(1).

    16. offer() is O(log n).

    17. poll() is O(log n).

    18. contains() is O(n).

    19. Iteration is not guaranteed to be sorted.

    20. Repeated poll() gives priority order.

    21. Comparator can customize priority.

    22. Comparable can define natural ordering.

    23. Min heap gives smallest element first.

    24. Max heap gives largest element first.

    25. Max heap can be created with reverseOrder().

    26. Kth Largest commonly uses min heap of size k.

    27. Kth Smallest commonly uses max heap of size k.

    28. Top K problems commonly use a heap.

    29. Running Median commonly uses two heaps.

    30. Dijkstra commonly uses a min heap.

    31. Kahn's Algorithm uses a Queue, not necessarily a PriorityQueue.

    32. PriorityQueue is not a fully sorted collection.

    33. PriorityQueue is not a replacement for TreeSet.

    34. PriorityQueue is ideal when the next element
        must be selected by priority.

---

# Final Memory Trick 🧠

Think:

    PriorityQueue
          |
          v
        HEAP
          |
          +------> Min Heap
          |          |
          |          v
          |       Smallest
          |
          +------> Max Heap
                     |
                     v
                  Largest

And for DSA:

    "Repeatedly need the smallest/largest?"
                    |
                    v
              Think HEAP
                    |
                    v
            PriorityQueue

Most important patterns:

    Kth Largest
        -> Min Heap

    Kth Smallest
        -> Max Heap

    Top K
        -> Heap of size K

    Running Median
        -> Two Heaps

    Merge K Sorted Lists
        -> Min Heap

    Dijkstra
        -> Min Heap

    Greedy selection
        -> PriorityQueue