# 🧰 Collection Framework — Introduction

> **Java Collection Framework (JCF) is a unified architecture of interfaces, classes, and algorithms used to store, retrieve, manipulate, and process groups of objects efficiently.**

---

# 📌 Table of Contents

1. [What is a Collection?](#1--what-is-a-collection)
2. [Why Do We Need Collections?](#2--why-do-we-need-collections)
3. [What is the Collection Framework?](#3--what-is-the-collection-framework)
4. [Why Was Collection Framework Introduced?](#4--why-was-collection-framework-introduced)
5. [Before Collection Framework](#5--before-collection-framework)
6. [Problems With Arrays](#6--problems-with-arrays)
7. [Collection Framework Architecture](#7--collection-framework-architecture)
8. [Main Components](#8--main-components)
9. [Interfaces](#9--interfaces)
10. [Implementations](#10--implementations)
11. [Algorithms](#11--algorithms)
12. [Map and Collection Framework](#12--map-and-collection-framework)
13. [Collection vs Collections](#13--collection-vs-collections)
14. [Collection vs Collections vs Arrays](#14--collection-vs-collections-vs-arrays)
15. [Iterable and Collection](#15--iterable-and-collection)
16. [Why Does Collection Store Objects?](#16--why-does-collection-store-objects)
17. [Generics and Collections](#17--generics-and-collections)
18. [Autoboxing in Collections](#18--autoboxing-in-collections)
19. [Common Collection Interfaces](#19--common-collection-interfaces)
20. [Common Implementations](#20--common-implementations)
21. [Choosing the Right Collection](#21--choosing-the-right-collection)
22. [Complexity Overview](#22--complexity-overview)
23. [Internal Working — Big Picture](#23--internal-working--big-picture)
24. [Important Design Principles](#24--important-design-principles)
25. [Advantages](#25--advantages)
26. [Disadvantages](#26--disadvantages)
27. [Common Mistakes](#27--common-mistakes)
28. [Interview Traps](#28--interview-traps)
29. [🧩 DSA Connection](#29--dsa-connection)
30. [🔥 Important DSA Questions](#30--important-dsa-questions)
31. [🎯 How to Think About Collection Problems](#31--how-to-think-about-collection-problems)
32. [Top Interview Questions](#32--top-interview-questions)
33. [30-Second Interview Answer](#33--30-second-interview-answer)
34. [Cheat Sheet](#34--cheat-sheet)
35. [Final Revision Checklist](#35--final-revision-checklist)

---

# 1. 🔹 What is a Collection?

A **collection** is a group of objects treated as a single unit.

For example:

    Student 1
    Student 2
    Student 3
    Student 4

Instead of managing every student separately, we can store them together.

Conceptually:

    Collection
        │
        ├── Student
        ├── Student
        ├── Student
        └── Student

In Java, collections are primarily used to store and manipulate groups of objects.

Example:

    ArrayList<String> names = new ArrayList<>();

    names.add("Aman");
    names.add("Rahul");
    names.add("Riya");

Now:

    names
       ↓
    [Aman, Rahul, Riya]

---

# 2. 🤔 Why Do We Need Collections?

Before collections, arrays were commonly used to store multiple values.

Example:

    int[] nums = new int[5];

But arrays have limitations.

### Major problems:

- Fixed size
- Limited built-in operations
- Difficult insertion/deletion
- No ready-made data structures such as sets and queues
- Manual searching/manipulation
- Less flexible for dynamic data

Collections solve many of these problems.

For example:

    ArrayList
    LinkedList
    HashSet
    TreeSet
    PriorityQueue
    ArrayDeque

provide different ways of storing and processing data.

---

# 3. 🏗️ What is the Collection Framework?

The **Java Collection Framework (JCF)** is a standardized architecture for representing and manipulating collections.

It provides:

    Interfaces
        ↓
    Implementations
        ↓
    Algorithms
        ↓
    Utility methods

The framework allows us to work with data structures through common interfaces.

For example:

    List<String> list = new ArrayList<>();

Here:

    List
      ↓
    Interface

    ArrayList
      ↓
    Implementation

This gives us abstraction.

---

# 4. 🕰️ Why Was Collection Framework Introduced?

Java originally provided classes such as:

    Vector
    Hashtable
    Stack

But there was no unified framework.

Different data structures had inconsistent APIs.

The Java Collection Framework was introduced in **Java 2 / JDK 1.2** to provide a standardized architecture for collections.

It introduced major interfaces such as:

    Collection
    List
    Set
    SortedSet

and utility support through:

    Collections

The framework was designed to make data structure usage more consistent and reusable.

---

# 5. 🏚️ Before Collection Framework

Before JCF, programmers commonly used:

    Arrays
    Vector
    Hashtable
    Enumeration

There was no common collection architecture.

For example:

    Vector
        ↓
    old-style dynamic array

    Hashtable
        ↓
    key-value structure

These classes existed before the modern framework.

The Collection Framework unified many concepts under common interfaces.

---

# 6. ⚠️ Problems With Arrays

Suppose:

    int[] arr = new int[5];

The size is fixed.

If we later need 10 elements, we cannot simply change:

    arr.length

Instead, we need a new array.

Conceptually:

    old array
         ↓
    create larger array
         ↓
    copy elements
         ↓
    use new array

Collections can handle dynamic resizing or provide structures designed for different operations.

---

# 7. 🌳 Collection Framework Architecture

A simplified hierarchy:

    Iterable
       │
       ▼
    Collection
       │
       ├──────── List
       │           ├── ArrayList
       │           ├── LinkedList
       │           └── Vector
       │                └── Stack
       │
       ├──────── Set
       │           ├── HashSet
       │           ├── LinkedHashSet
       │           └── SortedSet
       │                └── NavigableSet
       │                     └── TreeSet
       │
       └──────── Queue
                   ├── PriorityQueue
                   └── Deque
                        ├── ArrayDeque
                        └── LinkedList


    Map
       │
       ├── HashMap
       ├── LinkedHashMap
       ├── SortedMap
       │    └── NavigableMap
       │         └── TreeMap
       ├── Hashtable
       ├── WeakHashMap
       ├── IdentityHashMap
       ├── EnumMap
       └── ConcurrentHashMap

IMPORTANT:

    Map

is part of the Java Collections Framework, but:

    Map DOES NOT extend Collection.

This distinction is extremely important for interviews.

---

# 8. 🧩 Main Components

The Collection Framework can be understood through three major categories:

    1. Interfaces
    2. Implementations
    3. Algorithms / Utilities

---

# 9. 🔌 Interfaces

Interfaces define the general behavior.

Important interfaces:

    Iterable
    Collection
    List
    Set
    SortedSet
    NavigableSet
    Queue
    Deque

And separately:

    Map
    SortedMap
    NavigableMap

Example:

    List<String> list;

At this point we are programming to an interface.

We haven't decided which implementation will be used.

---

# 10. 🏗️ Implementations

Implementations provide the actual data structure.

Examples:

    List
      ↓
    ArrayList
    LinkedList
    Vector

    Set
      ↓
    HashSet
    LinkedHashSet
    TreeSet

    Queue
      ↓
    PriorityQueue

    Deque
      ↓
    ArrayDeque
    LinkedList

    Map
      ↓
    HashMap
    LinkedHashMap
    TreeMap
    Hashtable
    ConcurrentHashMap

---

# 11. ⚙️ Algorithms

The framework also provides utility algorithms.

These are primarily available through:

    java.util.Collections

Examples:

    Collections.sort()
    Collections.reverse()
    Collections.shuffle()
    Collections.max()
    Collections.min()
    Collections.frequency()
    Collections.binarySearch()
    Collections.swap()

Example:

    List<Integer> list =
        new ArrayList<>(List.of(5, 2, 8, 1));

    Collections.sort(list);

Result:

    [1, 2, 5, 8]

---

# 12. 🗺️ Map and Collection Framework

This is one of the biggest interview traps.

Many beginners think:

    Collection
       │
       └── Map

This is WRONG.

Correct conceptual view:

    Iterable
       │
       ▼
    Collection
       ├── List
       ├── Set
       └── Queue


    Map
       ├── HashMap
       ├── TreeMap
       ├── LinkedHashMap
       └── ...

`Map` is part of the broader **Collections Framework**, but it is not a subtype of `Collection`.

Why?

Because a `Collection` represents individual elements:

    [A, B, C]

while a `Map` represents mappings:

    key → value

Example:

    ID → Name

    101 → Rahul
    102 → Aman
    103 → Riya

---

# 13. 🆚 Collection vs Collections

This is a very common interview question.

## Collection

`Collection` is an interface.

Package:

    java.util

It represents a group of objects.

Example:

    Collection<Integer> nums;

---

## Collections

`Collections` is a utility class.

Package:

    java.util

It contains static methods for operating on collections.

Example:

    Collections.sort(list);

---

### Memory Trick

    Collection
        ↓
    INTERFACE

    Collections
        ↓
    UTILITY CLASS

---

# 14. 🆚 Collection vs Collections vs Arrays

| Feature | Collection | Collections | Arrays |
|---|---|---|---|
| Type | Interface | Utility class | Utility class |
| Package | `java.util` | `java.util` | `java.util` |
| Purpose | Represents groups | Collection utilities | Array utilities |
| Example | `List` | `Collections.sort()` | `Arrays.sort()` |
| Stores data? | Interface concept | No | No |
| Static methods? | Mostly no | Yes | Yes |

Example:

    Collections.sort(list);

versus:

    Arrays.sort(arr);

---

# 15. 🔄 Iterable and Collection

The hierarchy begins with:

    Iterable<T>

`Iterable` allows an object to be iterated using the enhanced `for` loop.

Example:

    for(String name : names) {
        System.out.println(name);
    }

The enhanced `for` loop works because the object is iterable.

`Collection` extends `Iterable`.

Conceptually:

    Iterable
       ↑
    Collection

Therefore:

    Collection
        ↓
    can be iterated

---

# 16. 📦 Why Does Collection Store Objects?

Traditional Java collections are designed around reference types.

For example:

    List<Integer> nums = new ArrayList<>();

`Integer` is a wrapper class, not primitive `int`.

This is because generics work with reference types.

You cannot write:

    List<int>

This is invalid.

Instead:

    List<Integer>

Java automatically performs boxing/unboxing when appropriate.

---

# 17. 🧬 Generics and Collections

Generics provide type safety.

Without generics:

    ArrayList list = new ArrayList();

We can accidentally insert different types:

    list.add(10);
    list.add("Java");
    list.add(3.14);

This can lead to runtime type problems.

With generics:

    ArrayList<Integer> list = new ArrayList<>();

Now:

    list.add(10);

is valid.

But:

    list.add("Java");

is rejected at compile time.

---

# 18. 🔄 Autoboxing in Collections

Primitive:

    int

Wrapper:

    Integer

Example:

    List<Integer> list = new ArrayList<>();

    list.add(10);

Java automatically converts:

    int
      ↓
    Integer

This is called:

    Autoboxing

When retrieving:

    int x = list.get(0);

Java performs:

    Integer
       ↓
      int

This is:

    Unboxing

---

# 19. 🧩 Common Collection Interfaces

## List

Characteristics:

- Ordered
- Allows duplicates
- Index-based access

Examples:

    ArrayList
    LinkedList
    Vector

Example:

    [10, 20, 10, 30]

Duplicates are allowed.

---

## Set

Characteristics:

- Does not allow duplicate elements
- Ordering depends on implementation

Examples:

    HashSet
    LinkedHashSet
    TreeSet

---

## Queue

Designed primarily for processing elements.

Typical concept:

    FIFO

First In:

    A

Then:

    B

Then:

    C

Processing:

    A → B → C

But not every queue implementation behaves exactly like a simple FIFO structure in all operations.

For example:

    PriorityQueue

uses priority ordering.

---

## Deque

Double-ended queue.

Allows insertion/removal from both ends.

Conceptually:

    ← front
    [A][B][C][D]
              →
              rear

---

# 20. 🏗️ Common Implementations

| Interface | Implementation | Main Idea |
|---|---|---|
| List | ArrayList | Dynamic array |
| List | LinkedList | Doubly linked structure |
| List | Vector | Legacy synchronized dynamic array |
| List | Stack | Legacy LIFO structure |
| Set | HashSet | Hash-based uniqueness |
| Set | LinkedHashSet | Hashing + insertion order |
| Set | TreeSet | Sorted tree-based set |
| Queue | PriorityQueue | Priority-based ordering |
| Deque | ArrayDeque | Efficient double-ended queue |
| Map | HashMap | Hash-based key-value mapping |
| Map | LinkedHashMap | Hashing + encounter order |
| Map | TreeMap | Sorted key-value mapping |
| Map | Hashtable | Legacy synchronized map |

---

# 21. 🎯 Choosing the Right Collection

Do not memorize only class names.

Think about the requirement.

### Need indexed access?

    ArrayList

### Need frequent insertion/removal at list ends?

    LinkedList
    ArrayDeque

But choose based on the actual workload; `ArrayList` can still be excellent for many list operations.

### Need unique elements?

    HashSet

### Need unique elements + insertion order?

    LinkedHashSet

### Need unique + sorted elements?

    TreeSet

### Need priority-based processing?

    PriorityQueue

### Need stack/deque behavior?

    ArrayDeque

### Need key-value mapping?

    HashMap

### Need key-value mapping + ordering?

    LinkedHashMap
    TreeMap

---

# 22. ⏱️ Complexity Overview

These are typical/average expectations, not universal guarantees.

| Structure | Access | Search | Insert | Delete |
|---|---:|---:|---:|---:|
| ArrayList | O(1) | O(n) | O(1)* | O(n) |
| LinkedList | O(n) | O(n) | O(1)** | O(1)** |
| HashSet | N/A | O(1) avg | O(1) avg | O(1) avg |
| TreeSet | N/A | O(log n) | O(log n) | O(log n) |
| PriorityQueue | Peek O(1) | O(n) | O(log n) | O(log n) |
| ArrayDeque | Ends O(1) | O(n) | Ends O(1) | Ends O(1) |
| HashMap | N/A | O(1) avg | O(1) avg | O(1) avg |
| TreeMap | N/A | O(log n) | O(log n) | O(log n) |

`*` ArrayList append is amortized O(1).

`**` For LinkedList, insertion/deletion is O(1) once the node/position is already known; finding that position can take O(n).

---

# 23. 🔬 Internal Working — Big Picture

Different collection implementations use different internal data structures.

### ArrayList

Conceptually:

    ArrayList
        ↓
    resizable array

When capacity is insufficient:

    old array
         ↓
    larger array
         ↓
    copy elements
         ↓
    continue

---

### LinkedList

Conceptually:

    Node ⇄ Node ⇄ Node ⇄ Node

Each node contains links to neighboring nodes.

---

### HashSet

Conceptually based on hashing.

    element
       ↓
    hash
       ↓
    bucket
       ↓
    stored entry

Modern Java implementations can use tree structures within heavily-collided buckets under certain conditions.

---

### TreeSet

Based on a sorted tree structure.

Typical complexity:

    O(log n)

---

### PriorityQueue

Implemented using a heap-based structure.

Typical:

    peek     → O(1)
    offer    → O(log n)
    poll     → O(log n)

---

# 24. 🧠 Important Design Principles

## 1. Programming to an Interface

Prefer:

    List<Integer> list = new ArrayList<>();

instead of unnecessarily tying the variable to:

    ArrayList<Integer> list = new ArrayList<>();

Why?

Because the interface represents the behavior we need.

The implementation can potentially be changed later.

Example:

    List<Integer> list = new LinkedList<>();

The rest of the code can often continue using the `List` API.

---

## 2. Separation of Interface and Implementation

Interface:

    List

Implementation:

    ArrayList

This provides abstraction.

---

## 3. Reusability

Instead of implementing:

    sorting
    searching
    reversing

manually every time, Java provides reusable utility methods.

---

## 4. Type Safety

Generics provide compile-time checking.

Example:

    List<String> names = new ArrayList<>();

---

# 25. ✅ Advantages

### 1. Dynamic Data Structures

Many collections can grow and shrink dynamically.

### 2. Ready-Made Data Structures

Java provides:

    List
    Set
    Queue
    Deque
    Map

### 3. Reusable Algorithms

Examples:

    sort
    reverse
    shuffle
    binarySearch

### 4. Type Safety

Generics reduce runtime type errors.

### 5. Better Abstraction

We can program using interfaces.

### 6. DSA Support

Most common DSA problems can be implemented using collection classes.

---

# 26. ⚠️ Disadvantages

### 1. Collections generally work with objects

Primitive values require wrappers.

Example:

    int → Integer

### 2. Memory overhead

Some collection implementations require additional memory for:

    nodes
    references
    metadata

### 3. Wrong collection choice can hurt performance

For example:

Using:

    LinkedList

when you need frequent random access is usually a poor choice.

### 4. Thread safety is not automatic

Many modern collections are not synchronized by default.

For concurrent use, the appropriate concurrent collection or synchronization strategy should be selected.

---

# 27. 🚨 Common Mistakes

## Mistake 1

Thinking:

    Collection = Map

Wrong.

Map is separate from the `Collection` interface hierarchy.

---

## Mistake 2

Thinking:

    Collections

is an interface.

Wrong.

`Collections` is a utility class.

---

## Mistake 3

Thinking:

    Collection

is a class.

Wrong.

It is an interface.

---

## Mistake 4

Using raw collections unnecessarily:

    ArrayList list = new ArrayList();

Prefer:

    ArrayList<String> list = new ArrayList<>();

---

## Mistake 5

Using the wrong implementation.

Example:

    Need sorted unique values
        ↓
    TreeSet

Not:

    ArrayList

---

# 28. 🎯 Interview Traps

### Trap 1

Q:

    Is Map a child of Collection?

A:

    No.

---

### Trap 2

Q:

    Is ArrayList a Collection?

A:

    Yes.

Because:

    ArrayList
       ↓
    List
       ↓
    Collection

---

### Trap 3

Q:

    Is String a Collection?

A:

    No.

String is a class representing a sequence of characters.

---

### Trap 4

Q:

    Does Collection allow primitive values?

Collections use reference types, so primitives are represented using wrapper classes.

Example:

    List<Integer>

not:

    List<int>

---

### Trap 5

Q:

    Is ArrayList synchronized?

A:

    No, ArrayList is not synchronized by default.

---

### Trap 6

Q:

    Is Vector synchronized?

A:

    Its legacy methods are synchronized.

---

### Trap 7

Q:

    Which is faster: ArrayList or LinkedList?

There is no universal answer.

It depends on the operation and workload.

For example:

    ArrayList
        → excellent random access

    LinkedList
        → efficient node-link operations when the position is already known

---

# 29. 🧩 DSA Connection

Collections are extremely important for DSA.

Instead of implementing every data structure manually, Java provides optimized standard implementations.

---

## 🔥 Pattern 1 — Frequency Counting

Use:

    HashMap

Example problem:

    Find frequency of each number.

Concept:

    number → frequency

Example:

    [1, 2, 2, 3, 1, 2]

Map:

    1 → 2
    2 → 3
    3 → 1

Mini snippet:

    Map<Integer, Integer> freq = new HashMap<>();

    for(int num : nums) {
        freq.put(num, freq.getOrDefault(num, 0) + 1);
    }

Typical complexity:

    Time  → O(n) average
    Space → O(n)

---

# 🔥 Pattern 2 — Duplicate Detection

Use:

    HashSet

Example:

    [1, 2, 3, 2]

Concept:

    If already present → duplicate.

Mini snippet:

    Set<Integer> set = new HashSet<>();

    for(int num : nums) {

        if(!set.add(num)) {
            return true;
        }
    }

    return false;

Typical complexity:

    Time  → O(n) average
    Space → O(n)

---

# 🔥 Pattern 3 — Two Sum

Use:

    HashMap

Example:

    nums = [2, 7, 11, 15]
    target = 9

At each element:

    required = target - current

Check whether required already exists.

Mini idea:

    Map<Integer, Integer> map = new HashMap<>();

    for(int i = 0; i < nums.length; i++) {

        int required = target - nums[i];

        if(map.containsKey(required)) {
            // answer found
        }

        map.put(nums[i], i);
    }

Typical complexity:

    Time  → O(n) average
    Space → O(n)

---

# 🔥 Pattern 4 — Stack Problems

Use:

    Deque

instead of the legacy `Stack` class for most new code.

Example:

    Deque<Integer> stack = new ArrayDeque<>();

    stack.push(10);
    stack.push(20);

    int x = stack.pop();

Useful for:

    Parentheses
    Next Greater Element
    Monotonic Stack
    Undo-style operations
    DFS

---

# 🔥 Pattern 5 — Queue / BFS

Use:

    Queue

Example:

    Queue<Integer> queue = new ArrayDeque<>();

    queue.offer(10);
    queue.offer(20);

    int x = queue.poll();

Used heavily in:

    BFS
    Level Order Traversal
    Scheduling
    Simulation

---

# 🔥 Pattern 6 — Priority Queue / Heap

Use:

    PriorityQueue

Example:

    PriorityQueue<Integer> pq = new PriorityQueue<>();

    pq.offer(30);
    pq.offer(10);
    pq.offer(20);

    System.out.println(pq.poll());

Output:

    10

Used for:

    Kth largest/smallest
    Top K elements
    Merge K sorted lists
    Scheduling
    Median-related problems

---

# 🔥 Pattern 7 — Sorted Unique Values

Use:

    TreeSet

Example:

    TreeSet<Integer> set = new TreeSet<>();

    set.add(30);
    set.add(10);
    set.add(20);

Result:

    [10, 20, 30]

Useful for:

    Sorted unique data
    Floor
    Ceiling
    Range-related problems

---

# 🔥 Pattern 8 — Custom Ordering

Use:

    Comparator

Example:

    List<Integer> nums = new ArrayList<>();

    nums.sort((a, b) -> b - a);

This gives descending order.

Useful for:

    Sorting objects
    Greedy algorithms
    PriorityQueue ordering
    Custom ranking

---

# 30. 🔥 Important DSA Questions

## Beginner

    1. Find maximum element
    2. Find minimum element
    3. Count frequency
    4. Find duplicates
    5. Remove duplicates
    6. Reverse an array
    7. Two Sum
    8. Check if array contains duplicate
    9. Majority Element
    10. Find missing number

---

## Intermediate

    11. Group Anagrams
    12. Top K Frequent Elements
    13. Kth Largest Element
    14. Valid Parentheses
    15. Next Greater Element
    16. Daily Temperatures
    17. Merge K Sorted Lists
    18. Sliding Window Maximum
    19. LRU Cache
    20. Longest Consecutive Sequence

---

# 31. 🎯 How to Think About Collection Problems

When you see a DSA problem, ask:

### Step 1

Do I need:

    frequency?

Use:

    HashMap

---

### Step 2

Do I need:

    uniqueness?

Use:

    HashSet

---

### Step 3

Do I need:

    sorted unique values?

Use:

    TreeSet

---

### Step 4

Do I need:

    FIFO?

Use:

    Queue

---

### Step 5

Do I need:

    LIFO?

Use:

    Deque

---

### Step 6

Do I need:

    smallest/largest repeatedly?

Think:

    PriorityQueue

---

### Step 7

Do I need:

    custom ordering?

Think:

    Comparator

---

### Step 8

Do I need:

    insertion order?

Think:

    LinkedHashSet
    LinkedHashMap

---

# 32. 🎤 Top Interview Questions

## Q1. What is Java Collection Framework?

### Answer

The Java Collection Framework is a unified architecture of interfaces, implementations, and utility algorithms used to store and manipulate groups of objects.

---

## Q2. What is the difference between Collection and Collections?

### Answer

`Collection` is an interface representing a group of objects, while `Collections` is a utility class containing static methods for operating on collections.

---

## Q3. Is Map part of the Collection Framework?

### Answer

Yes, Map is part of the Java Collections Framework, but the `Map` interface does not extend the `Collection` interface.

---

## Q4. Why are collections preferred over arrays?

### Answer

Collections provide more flexible data structures, dynamic sizing for many implementations, built-in operations, generics, and ready-made structures such as lists, sets, queues, and maps.

---

## Q5. What is the difference between List, Set, and Queue?

### List

    Ordered
    Allows duplicates
    Index-based operations

### Set

    Does not allow duplicate elements

### Queue

    Designed primarily for processing elements

---

## Q6. Why can't we use `List<int>`?

Because Java generics work with reference types, not primitive types.

Use:

    List<Integer>

instead.

---

## Q7. What is the root interface of the Collection hierarchy?

The direct root of the collection hierarchy is:

    Collection

But above it is:

    Iterable

So:

    Iterable
       ↓
    Collection

---

## Q8. Is ArrayList a Collection?

Yes.

Hierarchy:

    ArrayList
       ↓
    List
       ↓
    Collection
       ↓
    Iterable

---

## Q9. Which collection should be used for unique elements?

Common choices:

    HashSet
    LinkedHashSet
    TreeSet

Choice depends on whether you need:

    hashing
    insertion order
    sorting

---

## Q10. Which collection is useful for priority-based processing?

    PriorityQueue

---

## Q11. Which collection is commonly used for stack behavior?

For modern Java code:

    ArrayDeque

can be used as a stack.

Example:

    Deque<Integer> stack = new ArrayDeque<>();

---

## Q12. Which collection is useful for BFS?

Typically:

    Queue

with an implementation such as:

    ArrayDeque

---

## Q13. Which collection is useful for frequency counting?

Typically:

    HashMap

---

## Q14. Which collection is useful for duplicate detection?

Typically:

    HashSet

---

## Q15. What does programming to an interface mean?

Instead of depending on a concrete implementation, we use the interface type.

Example:

    List<Integer> list = new ArrayList<>();

The variable depends on `List`, while the actual object is an `ArrayList`.

---

# 33. 🎤 30-Second Interview Answer

> **The Java Collection Framework is a unified architecture introduced in Java 2 for storing and manipulating groups of objects. It provides interfaces such as List, Set, Queue, Deque, and Map, along with implementations such as ArrayList, HashSet, TreeSet, PriorityQueue, and HashMap. It also provides utility algorithms through the Collections class. Collections are important in DSA because they provide ready-made structures for frequency counting, duplicate detection, BFS, stack problems, heap problems, sorting, and many other common patterns.**

---

# 34. 🧾 Cheat Sheet

```text
COLLECTION FRAMEWORK
        │
        ├── Collection
        │      │
        │      ├── List
        │      │    ├── ArrayList
        │      │    ├── LinkedList
        │      │    └── Vector
        │      │         └── Stack
        │      │
        │      ├── Set
        │      │    ├── HashSet
        │      │    ├── LinkedHashSet
        │      │    └── TreeSet
        │      │
        │      └── Queue
        │           ├── PriorityQueue
        │           └── Deque
        │                └── ArrayDeque
        │
        └── Map
               ├── HashMap
               ├── LinkedHashMap
               ├── TreeMap
               ├── Hashtable
               ├── WeakHashMap
               ├── IdentityHashMap
               ├── EnumMap
               └── ConcurrentHashMap