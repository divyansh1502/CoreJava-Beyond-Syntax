15-Deque.md

# 15 — Deque Interface 🔄

> **Package:** `java.util`  
> **Full Form:** Double Ended Queue  
> **Type:** Interface  
> **Since:** Java 6  
> **Purpose:** Allows insertion and removal from both ends of a queue.

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [What is Deque?](#2-what-is-deque)
3. [Why Deque Exists](#3-why-deque-exists)
4. [Deque Hierarchy](#4-deque-hierarchy)
5. [Deque vs Queue](#5-deque-vs-queue)
6. [Deque vs Stack](#6-deque-vs-stack)
7. [Deque Operations](#7-deque-operations)
8. [Insertion Methods](#8-insertion-methods)
9. [Removal Methods](#9-removal-methods)
10. [Examination Methods](#10-examination-methods)
11. [First and Last Element](#11-first-and-last-element)
12. [addFirst()](#12-addfirst)
13. [addLast()](#13-addlast)
14. [offerFirst()](#14-offerfirst)
15. [offerLast()](#15-offerlast)
16. [removeFirst()](#16-removefirst)
17. [removeLast()](#17-removelast)
18. [pollFirst()](#18-pollfirst)
19. [pollLast()](#19-polllast)
20. [getFirst()](#20-getfirst)
21. [getLast()](#21-getlast)
22. [peekFirst()](#22-peekfirst)
23. [peekLast()](#23-peeklast)
24. [push() and pop()](#24-push-and-pop)
25. [removeFirstOccurrence()](#25-removefirstoccurrence)
26. [removeLastOccurrence()](#26-removelastoccurrence)
27. [Descending Iterator](#27-descending-iterator)
28. [Deque Implementations](#28-deque-implementations)
29. [ArrayDeque](#29-arraydeque)
30. [LinkedList as Deque](#30-linkedlist-as-deque)
31. [Internal Working](#31-internal-working)
32. [ArrayDeque Internal Structure](#32-arraydeque-internal-structure)
33. [Circular Array Concept](#33-circular-array-concept)
34. [Time Complexity](#34-time-complexity)
35. [Deque and Null](#35-deque-and-null)
36. [Deque and Duplicates](#36-deque-and-duplicates)
37. [Deque as Queue](#37-deque-as-queue)
38. [Deque as Stack](#38-deque-as-stack)
39. [Deque vs ArrayDeque](#39-deque-vs-arraydeque)
40. [Deque vs LinkedList](#40-deque-vs-linkedlist)
41. [Deque vs PriorityQueue](#41-deque-vs-priorityqueue)
42. [When to Use Deque](#42-when-to-use-deque)
43. [Real-World Applications](#43-real-world-applications)
44. [DSA Patterns](#44-dsa-patterns)
45. [DSA Pattern 1 — Sliding Window](#45-dsa-pattern-1--sliding-window)
46. [DSA Pattern 2 — Monotonic Deque](#46-dsa-pattern-2--monotonic-deque)
47. [DSA Pattern 3 — BFS](#47-dsa-pattern-3--bfs)
48. [DSA Pattern 4 — Stack Simulation](#48-dsa-pattern-4--stack-simulation)
49. [DSA Pattern 5 — Palindrome](#49-dsa-pattern-5--palindrome)
50. [DSA Pattern 6 — 0-1 BFS](#50-dsa-pattern-6--0-1-bfs)
51. [Common Mistakes](#51-common-mistakes)
52. [Interview Traps](#52-interview-traps)
53. [Top Interview Questions](#53-top-interview-questions)
54. [30-Second Interview Answer](#54-30-second-interview-answer)
55. [Cheat Sheet](#55-cheat-sheet)
56. [Quick Revision](#56-quick-revision)

---

# 1. Introduction 🔄

`Deque` stands for:

    Double Ended Queue

It is an interface in Java's Collections Framework.

A Deque allows insertion and removal from:

    Front
      |
      v
    [ A ][ B ][ C ][ D ]
                      ^
                      |
                    Rear

Therefore, a Deque can work as:

    Queue
    +
    Stack

The key idea is:

> A Deque allows operations at both ends.

---

# 2. What is Deque? 🎯

`Deque<E>` is an interface that extends:

    Queue<E>

Hierarchy:

    Collection
        |
      Queue
        |
      Deque
        |
    implementations

A Deque provides methods for both:

    First end

and:

    Last end

Example:

    Deque<Integer> deque =
        new ArrayDeque<>();

    deque.addFirst(10);
    deque.addLast(20);

Logical structure:

    [10, 20]

Then:

    deque.addFirst(5);

becomes:

    [5, 10, 20]

And:

    deque.addLast(30);

becomes:

    [5, 10, 20, 30]

---

# 3. Why Deque Exists 🧠

A normal Queue mainly focuses on:

    Insert at rear
    Remove from front

A Stack focuses on:

    Insert at top
    Remove from top

A Deque combines both capabilities.

It allows:

    Insert First
    Insert Last

    Remove First
    Remove Last

Therefore:

    Queue + Stack
          |
          v
        Deque

This makes Deque extremely useful in DSA.

---

# 4. Deque Hierarchy 🌳

The important hierarchy is:

    Iterable
       |
    Collection
       |
      Queue
       |
      Deque
       |
       +----------------+
       |                |
    ArrayDeque       LinkedList

Important:

    Deque

is an interface.

Common implementations:

    ArrayDeque
    LinkedList

---

# 5. Deque vs Queue ⚖️

| Feature | Queue | Deque |
|---|---|---|
| Insert front | Usually no | Yes |
| Insert rear | Yes | Yes |
| Remove front | Yes | Yes |
| Remove rear | Usually no | Yes |
| FIFO | Yes | Can support |
| LIFO | No | Can support |
| Double ended | No | Yes |

Queue:

    Insert -> Rear
    Remove -> Front

Deque:

    Insert -> Front / Rear
    Remove -> Front / Rear

---

# 6. Deque vs Stack ⚖️

| Feature | Stack | Deque |
|---|---|---|
| LIFO | Yes | Yes |
| Insert at top | Yes | Yes |
| Remove at top | Yes | Yes |
| Both ends | No | Yes |
| Recommended modern Java structure | No | Yes |

For stack behavior, Java documentation generally favors:

    Deque

over the legacy:

    Stack

Example:

    Deque<Integer> stack =
        new ArrayDeque<>();

---

# 7. Deque Operations 🔄

A Deque has three major categories.

## Insertion

    addFirst()
    addLast()
    offerFirst()
    offerLast()

## Removal

    removeFirst()
    removeLast()
    pollFirst()
    pollLast()

## Examination

    getFirst()
    getLast()
    peekFirst()
    peekLast()

There are also stack-style methods:

    push()
    pop()

---

# 8. Insertion Methods ➕

There are four major insertion methods:

    addFirst()
    addLast()

    offerFirst()
    offerLast()

Example:

    Deque<Integer> deque =
        new ArrayDeque<>();

    deque.addFirst(10);
    deque.addLast(20);

Result:

    [10, 20]

---

# 9. Removal Methods ➖

There are four main removal methods:

    removeFirst()
    removeLast()

    pollFirst()
    pollLast()

Difference:

    remove...
        -> throws exception if empty

    poll...
        -> returns null if empty

---

# 10. Examination Methods 👀

These methods inspect elements without removing them.

    getFirst()
    getLast()

    peekFirst()
    peekLast()

Difference:

    get...
        -> exception if empty

    peek...
        -> null if empty

---

# 11. First and Last Element 📍

Consider:

    [10, 20, 30, 40]

Then:

    first = 10
    last  = 40

Methods:

    getFirst()
        -> 10

    getLast()
        -> 40

    peekFirst()
        -> 10

    peekLast()
        -> 40

---

# 12. addFirst() ⬅️

Adds an element at the front.

Example:

    Deque<Integer> deque =
        new ArrayDeque<>();

    deque.addFirst(20);
    deque.addFirst(10);

Result:

    [10, 20]

Another:

    deque.addFirst(5);

Result:

    [5, 10, 20]

---

# 13. addLast() ➡️

Adds an element at the rear.

Example:

    Deque<Integer> deque =
        new ArrayDeque<>();

    deque.addLast(10);
    deque.addLast(20);
    deque.addLast(30);

Result:

    [10, 20, 30]

---

# 14. offerFirst() ⬅️

Adds an element at the front.

Example:

    deque.offerFirst(10);

Returns:

    true

For commonly used unbounded Deque implementations such as ArrayDeque, insertion normally succeeds unless an implementation-specific limitation is encountered.

---

# 15. offerLast() ➡️

Adds an element at the rear.

Example:

    deque.offerLast(20);

Result:

    [10, 20]

---

# 16. removeFirst() ❌

Removes and returns the first element.

Example:

    Deque<Integer> deque =
        new ArrayDeque<>();

    deque.addLast(10);
    deque.addLast(20);
    deque.addLast(30);

    System.out.println(
        deque.removeFirst()
    );

Output:

    10

Remaining:

    [20, 30]

If empty:

    removeFirst()

throws:

    NoSuchElementException

---

# 17. removeLast() ❌

Removes and returns the last element.

Example:

    deque.removeLast();

If:

    [10, 20, 30]

Result:

    30

Remaining:

    [10, 20]

If empty:

    NoSuchElementException

---

# 18. pollFirst() 🔽

Removes and returns the first element.

Example:

    Deque<Integer> deque =
        new ArrayDeque<>();

    deque.addLast(10);
    deque.addLast(20);

    System.out.println(
        deque.pollFirst()
    );

Output:

    10

If empty:

    null

---

# 19. pollLast() 🔼

Removes and returns the last element.

Example:

    deque.pollLast();

If:

    [10, 20, 30]

Output:

    30

If empty:

    null

---

# 20. getFirst() 👀

Returns the first element without removing it.

Example:

    Deque<Integer> deque =
        new ArrayDeque<>();

    deque.addLast(10);
    deque.addLast(20);

    System.out.println(
        deque.getFirst()
    );

Output:

    10

If empty:

    NoSuchElementException

---

# 21. getLast() 👀

Returns the last element without removing it.

Example:

    System.out.println(
        deque.getLast()
    );

Output:

    20

If empty:

    NoSuchElementException

---

# 22. peekFirst() 👀

Returns the first element without removing it.

Example:

    System.out.println(
        deque.peekFirst()
    );

If empty:

    null

---

# 23. peekLast() 👀

Returns the last element without removing it.

Example:

    System.out.println(
        deque.peekLast()
    );

If empty:

    null

---

# 24. push() and pop() 📚

Deque can behave like a Stack.

## push()

`push()` adds to the front.

    deque.push(10);

Equivalent to:

    deque.addFirst(10);

---

## pop()

`pop()` removes from the front.

    deque.pop();

Equivalent to:

    deque.removeFirst();

Therefore:

    push()
       |
       v
    addFirst()

    pop()
       |
       v
    removeFirst()

---

# 25. removeFirstOccurrence() 🔍

Removes the first occurrence of a specified element.

Example:

    Deque<Integer> deque =
        new ArrayDeque<>();

    deque.addLast(10);
    deque.addLast(20);
    deque.addLast(10);
    deque.addLast(30);

Structure:

    [10, 20, 10, 30]

Call:

    deque.removeFirstOccurrence(10);

Result:

    [20, 10, 30]

Only the first `10` is removed.

---

# 26. removeLastOccurrence() 🔍

Removes the last occurrence.

Starting with:

    [10, 20, 10, 30]

Call:

    deque.removeLastOccurrence(10);

Result:

    [10, 20, 30]

Only the last `10` is removed.

---

# 27. Descending Iterator 🔄

Deque provides:

    descendingIterator()

It iterates from:

    Last -> First

Example:

    Deque<Integer> deque =
        new ArrayDeque<>();

    deque.addLast(10);
    deque.addLast(20);
    deque.addLast(30);

    Iterator<Integer> it =
        deque.descendingIterator();

    while (it.hasNext()) {

        System.out.println(
            it.next()
        );
    }

Output:

    30
    20
    10

Normal iteration:

    10
    20
    30

Descending iteration:

    30
    20
    10

---

# 28. Deque Implementations 🏗️

Common implementations include:

    ArrayDeque
    LinkedList

Hierarchy:

    Deque
      |
      +---- ArrayDeque
      |
      +---- LinkedList

---

# 29. ArrayDeque ⭐

`ArrayDeque` is a resizable-array implementation of `Deque`.

Example:

    Deque<Integer> deque =
        new ArrayDeque<>();

    deque.addFirst(10);
    deque.addLast(20);

It can be used as:

    Queue

or:

    Stack

or:

    Double-ended Queue

Important characteristics:

    Fast end operations
    Resizable
    No null elements
    Not thread-safe
    No random access by index

---

# 30. LinkedList as Deque 🔗

`LinkedList` implements:

    List
    Deque

Therefore:

    LinkedList<Integer> list =
        new LinkedList<>();

can perform Deque operations.

Example:

    list.addFirst(10);
    list.addLast(20);

Result:

    [10, 20]

LinkedList internally uses linked nodes.

---

# 31. Internal Working ⚙️

The `Deque` interface itself does not define one internal data structure.

The implementation decides how the operations work.

For:

    ArrayDeque

the implementation uses a resizable array-based circular structure.

For:

    LinkedList

the implementation uses a doubly linked list.

Therefore:

    Deque
      |
      +--> ArrayDeque
      |       |
      |       +--> Array-based
      |
      +--> LinkedList
              |
              +--> Doubly linked

---

# 32. ArrayDeque Internal Structure 🧠

Conceptually, ArrayDeque uses an array with positions representing:

    head
    tail

Example:

    [ _ ][ A ][ B ][ C ][ _ ][ _ ]

The structure can wrap around the array.

This avoids shifting all elements when adding/removing from either end.

---

# 33. Circular Array Concept 🔄

Imagine:

    [0][1][2][3][4][5]

After reaching the end, the logical position can wrap around toward the beginning.

Conceptually:

         0
       /   \
      1     5
      |     |
      2     4
       \   /
         3

This is called:

    Circular Array

It allows efficient operations at both ends.

---

# 34. Time Complexity ⏱️

For `ArrayDeque`:

| Operation | Typical Complexity |
|---|---:|
| addFirst() | O(1) amortized |
| addLast() | O(1) amortized |
| offerFirst() | O(1) amortized |
| offerLast() | O(1) amortized |
| removeFirst() | O(1) |
| removeLast() | O(1) |
| pollFirst() | O(1) |
| pollLast() | O(1) |
| peekFirst() | O(1) |
| peekLast() | O(1) |
| push() | O(1) amortized |
| pop() | O(1) |
| contains() | O(n) |

Important:

    ArrayDeque
        ->
    excellent for operations at both ends

---

# 35. Deque and Null ⚠️

The `Deque` interface itself does not universally prohibit null in every possible implementation.

However, important common implementations such as:

    ArrayDeque

do not permit null.

Example:

    Deque<Integer> deque =
        new ArrayDeque<>();

    deque.addFirst(null);

throws:

    NullPointerException

Why?

Because `null` is also used as a special return value by methods such as:

    poll()
    peek()

Allowing null would make it difficult to distinguish:

    "queue is empty"

from:

    "queue contains null"

---

# 36. Deque and Duplicates 🔁

Deque implementations such as ArrayDeque allow duplicates.

Example:

    Deque<Integer> deque =
        new ArrayDeque<>();

    deque.addLast(10);
    deque.addLast(10);
    deque.addLast(20);

Result:

    [10, 10, 20]

Unlike:

    Set

a Deque does not enforce uniqueness.

---

# 37. Deque as Queue 🚶

Deque can behave exactly like a FIFO Queue.

Use:

    addLast()
    removeFirst()

Example:

    Deque<Integer> queue =
        new ArrayDeque<>();

    queue.addLast(10);
    queue.addLast(20);
    queue.addLast(30);

    System.out.println(
        queue.removeFirst()
    );

Output:

    10

Processing:

    10
    20
    30

Therefore:

    addLast()
        +
    removeFirst()
        =
    FIFO Queue

---

# 38. Deque as Stack 📚

Deque can also behave like a LIFO Stack.

Use:

    addFirst()
    removeFirst()

or:

    push()
    pop()

Example:

    Deque<Integer> stack =
        new ArrayDeque<>();

    stack.push(10);
    stack.push(20);
    stack.push(30);

    System.out.println(
        stack.pop()
    );

Output:

    30

Processing:

    30
    20
    10

Therefore:

    push()
        +
    pop()
        =
    LIFO Stack

---

# 39. Deque vs ArrayDeque ⚖️

This distinction is important.

    Deque

is:

    Interface

while:

    ArrayDeque

is:

    Class

Example:

    Deque<Integer> deque =
        new ArrayDeque<>();

Here:

    Deque
        -> reference type

    ArrayDeque
        -> object type

This is standard programming to code against the interface.

---

# 40. Deque vs LinkedList ⚖️

| Feature | ArrayDeque | LinkedList |
|---|---|---|
| Implements Deque | Yes | Yes |
| Internal structure | Array-based | Doubly linked |
| End operations | Very efficient | Efficient |
| Random access | No | Yes, but O(n) |
| Null | No | Yes |
| Memory overhead | Generally lower | Generally higher |
| Stack use | Excellent | Possible |
| Queue use | Excellent | Possible |

For pure queue/deque operations, `ArrayDeque` is often preferred over `LinkedList`.

---

# 41. Deque vs PriorityQueue ⚖️

| Feature | Deque | PriorityQueue |
|---|---|---|
| Main idea | Double-ended access | Priority-based access |
| Remove first | Yes | Yes |
| Remove last | Yes | No direct priority meaning |
| FIFO | Yes | No |
| LIFO | Yes | No |
| Heap | No | Yes |
| Sliding Window | Excellent | Different use case |
| Priority processing | No | Yes |

Think:

    Deque
        ->
    Position-based ends

    PriorityQueue
        ->
    Priority-based processing

---

# 42. When to Use Deque 🎯

Use a Deque when you need:

    insertion/removal at both ends

or when you need:

    Queue behavior

or:

    Stack behavior

or:

    Sliding Window

or:

    Monotonic Queue

or:

    BFS

or:

    0-1 BFS

---

# 43. Real-World Applications 🌍

## Browser History

Can conceptually maintain navigation from both ends.

---

## Task Processing

Tasks can be added or removed from either end.

---

## Undo/Redo

Deque-like structures can help maintain recent operations.

---

## Sliding Window

Maintaining candidates inside a moving window is a major DSA use.

---

## BFS

A queue can be implemented using:

    ArrayDeque

---

## Stack

A stack can also be implemented using:

    ArrayDeque

---

# 44. DSA Patterns 🧩

The most important Deque patterns are:

    1. Sliding Window
    2. Monotonic Deque
    3. BFS
    4. Stack Simulation
    5. Palindrome
    6. 0-1 BFS

The most important mental trigger:

> "I need efficient insertion/removal from both ends."

Think:

    Deque

---

# 45. DSA Pattern 1 — Sliding Window 🔥

Suppose:

    nums = [1, 3, -1, -3, 5, 3, 6, 7]

We need the maximum of every window of size:

    k = 3

A naive solution checks every window completely.

That can become:

    O(n * k)

A Deque can optimize this to:

    O(n)

The Deque stores indices.

Important idea:

    Remove indices outside window.

    Remove smaller values from the back.

    Front contains the maximum candidate.

---

# 46. DSA Pattern 2 — Monotonic Deque 🔥

A monotonic deque maintains elements in increasing or decreasing order.

For sliding-window maximum:

    Deque values

are maintained in:

    decreasing order

Example:

    [9, 7, 5, 3]

The front:

    9

is the maximum.

When a larger value arrives:

    10

remove smaller elements from the back:

    [9, 7, 5, 3]

becomes:

    []

then:

    [10]

This allows the maximum to remain at the front.

---

# 47. DSA Pattern 3 — BFS 🌳

Breadth-First Search uses FIFO behavior.

Java implementation:

    Queue<Integer> queue =
        new ArrayDeque<>();

or:

    Deque<Integer> queue =
        new ArrayDeque<>();

Then:

    queue.offerLast(node);

    int current =
        queue.pollFirst();

This gives:

    FIFO

behavior.

For BFS, a Deque is often used as an efficient queue implementation.

---

# 48. DSA Pattern 4 — Stack Simulation 📚

A Deque can replace Stack.

Instead of:

    Stack<Integer> stack =
        new Stack<>();

prefer:

    Deque<Integer> stack =
        new ArrayDeque<>();

Then:

    stack.push(10);
    stack.push(20);

    int value =
        stack.pop();

This gives:

    LIFO

behavior.

Common applications:

    balanced parentheses
    next greater element
    DFS
    expression processing

---

# 49. DSA Pattern 5 — Palindrome 🔄

A palindrome reads the same forward and backward.

Example:

    MADAM

A Deque allows comparison from both ends.

Concept:

    [M A D A M]
     ^         ^
     |         |
    first     last

Compare:

    first == last

Then remove both:

    [A D A]

Continue.

This naturally uses:

    removeFirst()
    removeLast()

---

# 50. DSA Pattern 6 — 0-1 BFS 🚀

0-1 BFS is used for graphs where edge weights are only:

    0
    1

A Deque is used.

For an edge with weight:

    0

add the new node to the front:

    addFirst()

For an edge with weight:

    1

add the new node to the back:

    addLast()

Concept:

    weight 0
        ->
    front

    weight 1
        ->
    back

This provides efficient processing for 0-1 weighted graphs.

Typical complexity:

    O(V + E)

---

# 51. Common Mistakes ⚠️

## Mistake 1 — Thinking Deque Means Only Queue

Deque can behave as:

    Queue

and:

    Stack

---

## Mistake 2 — Confusing Deque with PriorityQueue

Deque is based on:

    ends

PriorityQueue is based on:

    priority

---

## Mistake 3 — Using LinkedList Automatically

For pure Deque operations, consider:

    ArrayDeque

---

## Mistake 4 — Using Stack

Modern Java code commonly uses:

    Deque

for stack behavior.

---

## Mistake 5 — Thinking ArrayDeque Supports Random Access

It does not provide:

    get(index)

like `ArrayList`.

---

## Mistake 6 — Adding null to ArrayDeque

Not allowed.

---

## Mistake 7 — Confusing poll and remove

Remember:

    poll
        -> null if empty

    remove
        -> exception if empty

---

# 52. Interview Traps 🎤

## Trap 1

> Is Deque a class?

No.

It is an interface.

---

## Trap 2

> What does Deque stand for?

Double Ended Queue.

---

## Trap 3

> Does Deque extend Queue?

Yes.

    Deque extends Queue

---

## Trap 4

> Can Deque work as a Stack?

Yes.

Use:

    push()
    pop()

---

## Trap 5

> Can Deque work as a Queue?

Yes.

Use:

    offerLast()
    pollFirst()

---

## Trap 6

> What is the common implementation?

    ArrayDeque

---

## Trap 7

> Does ArrayDeque allow null?

No.

---

## Trap 8

> Does Deque allow duplicates?

Common implementations such as ArrayDeque do.

---

## Trap 9

> Is ArrayDeque thread-safe?

No.

---

## Trap 10

> What is the complexity of addFirst()?

For ArrayDeque:

    O(1) amortized

---

## Trap 11

> What is the complexity of removeLast()?

For ArrayDeque:

    O(1)

---

## Trap 12

> Which structure is useful for sliding-window maximum?

A:

    Monotonic Deque

---

## Trap 13

> Which structure is used in 0-1 BFS?

    Deque

---

## Trap 14

> Why is Deque preferred over Stack?

Deque provides stack operations and is the modern general-purpose abstraction for stack behavior.

---

# 53. Top Interview Questions 🎤

## Q1. What is Deque?

Deque is a double-ended queue that allows insertion and removal from both ends.

---

## Q2. Is Deque a class or interface?

Interface.

---

## Q3. Does Deque extend Queue?

Yes.

---

## Q4. What is the full form of Deque?

Double Ended Queue.

---

## Q5. What are common Deque implementations?

    ArrayDeque
    LinkedList

---

## Q6. Can Deque work as a Stack?

Yes.

    push()
    pop()

---

## Q7. Can Deque work as a Queue?

Yes.

    offerLast()
    pollFirst()

---

## Q8. What is the difference between removeFirst() and pollFirst()?

    removeFirst()
        -> exception if empty

    pollFirst()
        -> null if empty

---

## Q9. What is the difference between getFirst() and peekFirst()?

    getFirst()
        -> exception if empty

    peekFirst()
        -> null if empty

---

## Q10. What is ArrayDeque?

A resizable-array implementation of the Deque interface.

---

## Q11. Does ArrayDeque allow null?

No.

---

## Q12. Does ArrayDeque allow duplicates?

Yes.

---

## Q13. Is ArrayDeque thread-safe?

No.

---

## Q14. What is the complexity of insertion at either end?

Typically:

    O(1) amortized

for ArrayDeque.

---

## Q15. What is a monotonic deque?

A Deque maintained in monotonic increasing or decreasing order to efficiently solve problems such as sliding-window maximum/minimum.

---

## Q16. What is 0-1 BFS?

A graph traversal algorithm for graphs with edge weights 0 or 1 that uses a Deque.

---

## Q17. Why use ArrayDeque instead of Stack?

ArrayDeque provides efficient stack operations and is the preferred modern Deque-based approach for stack behavior.

---

## Q18. Why use ArrayDeque instead of LinkedList?

For pure Deque/queue operations, ArrayDeque generally has lower memory overhead and good cache locality because it uses an array-based structure.

---

## Q19. Does Deque provide random access?

No.

Deque is optimized for end operations.

---

## Q20. What is the difference between Deque and PriorityQueue?

Deque chooses elements based on their position at the ends.

PriorityQueue chooses elements based on priority.

---

# 54. 30-Second Interview Answer 🎯

If the interviewer asks:

> "Explain Deque in Java."

Answer:

> "`Deque` stands for Double Ended Queue and is an interface that extends `Queue`. It allows insertion and removal from both the front and the rear. Because of this, it can be used both as a FIFO Queue and as a LIFO Stack. Common implementations include `ArrayDeque` and `LinkedList`. `ArrayDeque` uses a resizable array-based structure and provides efficient operations at both ends. It does not allow null elements. In DSA, Deque is especially important for sliding-window problems, monotonic queues, BFS, stack simulation, palindrome problems, and 0-1 BFS."

---

# 55. Cheat Sheet 📋

## Definition

    Deque
        =
    Double Ended Queue

---

## Hierarchy

    Collection
        |
      Queue
        |
      Deque
       / \
      /   \
    ArrayDeque
    LinkedList

---

## Queue Behavior

    addLast()
        +
    removeFirst()

        =
    FIFO

---

## Stack Behavior

    push()
        +
    pop()

        =
    LIFO

---

## Front Operations

    addFirst()
    offerFirst()
    removeFirst()
    pollFirst()
    getFirst()
    peekFirst()

---

## Rear Operations

    addLast()
    offerLast()
    removeLast()
    pollLast()
    getLast()
    peekLast()

---

## Exception vs Null

    add/remove/get
        ->
    exception when appropriate

    offer/poll/peek
        ->
    special-value based behavior

For empty removal/examination:

    remove...
        -> NoSuchElementException

    get...
        -> NoSuchElementException

    poll...
        -> null

    peek...
        -> null

---

## Main Implementations

    ArrayDeque
    LinkedList

---

## ArrayDeque

    Array-based
    Resizable
    No null
    Not thread-safe
    Efficient at both ends

---

## Complexity

    addFirst()
        -> O(1) amortized

    addLast()
        -> O(1) amortized

    removeFirst()
        -> O(1)

    removeLast()
        -> O(1)

    peekFirst()
        -> O(1)

    peekLast()
        -> O(1)

    contains()
        -> O(n)

---

## DSA

    Sliding Window
        -> Deque

    Monotonic Queue
        -> Deque

    BFS
        -> Queue/Deque

    Stack
        -> Deque

    Palindrome
        -> Deque

    0-1 BFS
        -> Deque

---

# 56. Quick Revision ⚡

Remember:

    1. Deque means Double Ended Queue.

    2. Deque is an interface.

    3. Deque extends Queue.

    4. Deque supports both ends.

    5. Insert at front:
       addFirst()
       offerFirst()

    6. Insert at rear:
       addLast()
       offerLast()

    7. Remove from front:
       removeFirst()
       pollFirst()

    8. Remove from rear:
       removeLast()
       pollLast()

    9. Examine front:
       getFirst()
       peekFirst()

    10. Examine rear:
        getLast()
        peekLast()

    11. push() = addFirst()

    12. pop() = removeFirst()

    13. Deque can behave as Queue.

    14. Deque can behave as Stack.

    15. ArrayDeque is a major implementation.

    16. LinkedList also implements Deque.

    17. ArrayDeque uses an array-based structure.

    18. ArrayDeque does not allow null.

    19. ArrayDeque allows duplicates.

    20. ArrayDeque is not thread-safe.

    21. End insertion/removal is efficient.

    22. Deque does not provide random access.

    23. Deque is different from PriorityQueue.

    24. PriorityQueue is priority-based.

    25. Deque is position/end-based.

    26. Sliding Window commonly uses Deque.

    27. Monotonic Queue uses Deque.

    28. BFS can use ArrayDeque.

    29. Stack behavior can use ArrayDeque.

    30. 0-1 BFS uses Deque.

---

# Final Memory Trick 🧠

Think of Deque as:

             DEQUE
               |
       +-------+-------+
       |               |
    FRONT             REAR
       |               |
    insert            insert
    remove            remove

Therefore:

    Both Ends
        |
        v
      DEQUE

For Queue:

    addLast()
        +
    removeFirst()
        =
    FIFO

For Stack:

    push()
        +
    pop()
        =
    LIFO

For DSA:

    Sliding Window
    Monotonic Queue
    BFS
    Palindrome
    0-1 BFS

        ↓

      DEQUE