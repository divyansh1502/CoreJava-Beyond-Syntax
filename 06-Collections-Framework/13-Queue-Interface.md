# 13 — Queue Interface 🚶‍♂️

> **Package:** `java.util`  
> **Type:** Interface  
> **Extends:** `Collection<E>`  
> **Main purpose:** Represents a collection designed for holding elements before processing  
> **Common implementations:** `LinkedList`, `PriorityQueue`, `ArrayDeque`

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [What is Queue?](#2-what-is-queue)
3. [Why Queue Exists](#3-why-queue-exists)
4. [Queue Hierarchy](#4-queue-hierarchy)
5. [FIFO Principle](#5-fifo-principle)
6. [Queue vs Stack](#6-queue-vs-stack)
7. [Queue Implementations](#7-queue-implementations)
8. [Creating a Queue](#8-creating-a-queue)
9. [Core Queue Operations](#9-core-queue-operations)
10. [add()](#10-add)
11. [offer()](#11-offer)
12. [remove()](#12-remove)
13. [poll()](#13-poll)
14. [element()](#14-element)
15. [peek()](#15-peek)
16. [add vs offer](#16-add-vs-offer)
17. [remove vs poll](#17-remove-vs-poll)
18. [element vs peek](#18-element-vs-peek)
19. [The Queue Method Family](#19-the-queue-method-family)
20. [Queue with LinkedList](#20-queue-with-linkedlist)
21. [Queue with ArrayDeque](#21-queue-with-arraydeque)
22. [PriorityQueue](#22-priorityqueue)
23. [Queue and null](#23-queue-and-null)
24. [Queue Does Not Mean Always FIFO](#24-queue-does-not-mean-always-fifo)
25. [Queue Iteration](#25-queue-iteration)
26. [Queue with Generics](#26-queue-with-generics)
27. [Internal Working](#27-internal-working)
28. [Time Complexity](#28-time-complexity)
29. [Queue vs List](#29-queue-vs-list)
30. [Queue vs Stack](#30-queue-vs-stack)
31. [Queue vs Deque](#31-queue-vs-deque)
32. [Queue vs PriorityQueue](#32-queue-vs-priorityqueue)
33. [When to Use Queue](#33-when-to-use-queue)
34. [When Not to Use Queue](#34-when-not-to-use-queue)
35. [Real-World Examples](#35-real-world-examples)
36. [DSA Patterns](#36-dsa-patterns)
37. [DSA Problem-Solving Examples](#37-dsa-problem-solving-examples)
38. [Common Mistakes](#38-common-mistakes)
39. [Interview Traps](#39-interview-traps)
40. [Top Interview Questions](#40-top-interview-questions)
41. [30-Second Interview Answer](#41-30-second-interview-answer)
42. [Cheat Sheet](#42-cheat-sheet)
43. [Quick Revision](#43-quick-revision)

---

# 1. Introduction 🚶‍♂️

`Queue` is an interface in Java's Collections Framework.

It represents a collection designed to hold elements before they are processed.

The most common queue principle is:

    FIFO

which means:

    First In
        ->
    First Out

Example:

    Add:

    10
    20
    30

    Queue:

    10 -> 20 -> 30

When removing:

    10

is removed first because it entered first.

---

# 2. What is Queue? 📚

`Queue<E>` extends `Collection<E>`.

Its purpose is to provide operations for:

    adding elements
    removing elements
    examining elements

The three major operation groups are:

    Insert
        add()
        offer()

    Remove
        remove()
        poll()

    Examine
        element()
        peek()

The methods come in pairs because they behave differently when the queue is empty or capacity-restricted.

---

# 3. Why Queue Exists 🎯

Suppose you have tasks:

    Task A
    Task B
    Task C

You want to process them in the same order they arrived.

A Queue provides an abstraction for exactly this requirement.

Instead of worrying about the internal data structure, you can write:

    Queue<Task> tasks;

Then choose an implementation:

    LinkedList
    ArrayDeque
    PriorityQueue

This is one of the major benefits of programming to an interface.

---

# 4. Queue Hierarchy 🌳

The basic hierarchy is:

    Iterable
       |
    Collection
       |
      Queue
       |
       +----------------+
       |                |
     Deque          PriorityQueue
       |
       +----------------+
       |
    ArrayDeque
    LinkedList

More precisely:

    Collection
        |
       Queue
        |
      Deque
        |
    ArrayDeque

And:

    Queue
       |
    PriorityQueue

`LinkedList` implements both:

    List
    Deque

Therefore it can also be used as a Queue.

---

# 5. FIFO Principle 🔄

The traditional Queue follows:

    FIFO

Meaning:

    First In
        |
        v
    First Out

Example:

    Queue:

    FRONT
      |
      v
    [10] [20] [30]
                 ^
                 |
               REAR

Remove:

    10

Remaining:

    [20] [30]

Then:

    remove()
        -> 20

Then:

    remove()
        -> 30

---

# 6. Queue vs Stack ⚔️

These two structures use opposite processing orders.

## Queue

    FIFO

    First In
        ->
    First Out

Example:

    [10] [20] [30]

Remove:

    10

---

## Stack

    LIFO

    Last In
        ->
    First Out

Example:

    [10] [20] [30]

Remove:

    30

---

## Memory Trick

    Queue
        -> line at a counter

    Stack
        -> pile of plates

---

# 7. Queue Implementations 🧩

The `Queue` interface itself does not provide the actual storage implementation.

Common implementations include:

    LinkedList
    ArrayDeque
    PriorityQueue

---

## LinkedList

Can act as:

    List
    Queue
    Deque

Example:

    Queue<Integer> queue =
        new LinkedList<>();

---

## ArrayDeque

Designed specifically for efficient double-ended queue operations.

Example:

    Queue<Integer> queue =
        new ArrayDeque<>();

For normal FIFO queue behavior, `ArrayDeque` is often a strong general-purpose choice.

---

## PriorityQueue

Does not follow normal insertion-order FIFO processing.

Instead, elements are processed according to priority/order.

Example:

    Queue<Integer> queue =
        new PriorityQueue<>();

---

# 8. Creating a Queue 💻

## Using LinkedList

    Queue<Integer> queue =
        new LinkedList<>();

---

## Using ArrayDeque

    Queue<Integer> queue =
        new ArrayDeque<>();

---

## Using PriorityQueue

    Queue<Integer> queue =
        new PriorityQueue<>();

---

## Using String

    Queue<String> queue =
        new ArrayDeque<>();

---

## Using Interface Reference

Prefer:

    Queue<Integer> queue =
        new ArrayDeque<>();

instead of:

    ArrayDeque<Integer> queue =
        new ArrayDeque<>();

when you only need Queue operations.

This is programming to the interface.

---

# 9. Core Queue Operations 🛠️

The six most important methods are:

    add()
    offer()

    remove()
    poll()

    element()
    peek()

Think:

    INSERT
       |
    +--+--+
    |     |
   add  offer

    REMOVE
       |
    +--+--+
    |     |
 remove  poll

    EXAMINE
       |
    +--+--+
    |     |
 element peek

---

# 10. add() ➕

`add()` inserts an element into the Queue.

Example:

    Queue<Integer> queue =
        new ArrayDeque<>();

    queue.add(10);
    queue.add(20);
    queue.add(30);

    System.out.println(queue);

Output:

    [10, 20, 30]

Return value:

    true

If insertion cannot be performed because of a capacity restriction, `add()` may throw an exception.

---

# 11. offer() 📥

`offer()` also inserts an element.

Example:

    Queue<Integer> queue =
        new ArrayDeque<>();

    queue.offer(10);
    queue.offer(20);
    queue.offer(30);

    System.out.println(queue);

Output:

    [10, 20, 30]

Difference:

    add()
        -> may throw exception if insertion fails

    offer()
        -> returns false if insertion fails

For an unbounded queue such as the commonly used `ArrayDeque`, both normally succeed.

---

# 12. remove() ➖

`remove()` removes and returns the head of the Queue.

Example:

    Queue<Integer> queue =
        new ArrayDeque<>();

    queue.add(10);
    queue.add(20);
    queue.add(30);

    int value =
        queue.remove();

    System.out.println(value);
    System.out.println(queue);

Output:

    10
    [20, 30]

Important:

> `remove()` throws an exception if the Queue is empty.

---

# 13. poll() 📤

`poll()` removes and returns the head.

Example:

    Queue<Integer> queue =
        new ArrayDeque<>();

    queue.add(10);
    queue.add(20);

    System.out.println(
        queue.poll()
    );

Output:

    10

Remaining:

    [20]

The major difference from `remove()`:

    remove()
        -> exception if empty

    poll()
        -> null if empty

Example:

    Queue<Integer> queue =
        new ArrayDeque<>();

    System.out.println(
        queue.poll()
    );

Output:

    null

---

# 14. element() 👀

`element()` returns the head without removing it.

Example:

    Queue<Integer> queue =
        new ArrayDeque<>();

    queue.add(10);
    queue.add(20);

    System.out.println(
        queue.element()
    );

    System.out.println(queue);

Output:

    10
    [10, 20]

The element remains in the Queue.

If the Queue is empty:

    element()

throws an exception.

---

# 15. peek() 🔍

`peek()` returns the head without removing it.

Example:

    Queue<Integer> queue =
        new ArrayDeque<>();

    queue.add(10);
    queue.add(20);

    System.out.println(
        queue.peek()
    );

Output:

    10

The Queue remains:

    [10, 20]

If the Queue is empty:

    peek()

returns:

    null

---

# 16. add vs offer ⚖️

Both are insertion operations.

| Method | Success | Failure |
|---|---|---|
| add() | Adds element | Throws exception |
| offer() | Adds element | Returns false |

Memory:

    add
      ->
    "Add or complain"

    offer
      ->
    "Offer and tell me whether it worked"

---

# 17. remove vs poll ⚖️

Both remove the head.

| Method | Success | Empty Queue |
|---|---|---|
| remove() | Returns removed element | Throws exception |
| poll() | Returns removed element | Returns null |

Memory:

    remove
        -> strict

    poll
        -> safe empty result

---

# 18. element vs peek ⚖️

Both examine the head without removing it.

| Method | Success | Empty Queue |
|---|---|---|
| element() | Returns head | Throws exception |
| peek() | Returns head | Returns null |

Memory:

    element
        -> strict

    peek
        -> safe

---

# 19. The Queue Method Family 🧠

This table is extremely important for interviews.

| Operation | Exception Version | Special-Value Version |
|---|---|---|
| Insert | add(e) | offer(e) |
| Remove | remove() | poll() |
| Examine | element() | peek() |

Remember:

    add      -> offer
    remove   -> poll
    element  -> peek

The right-side methods generally provide non-exceptional special-value behavior.

---

# 20. Queue with LinkedList 🔗

`LinkedList` implements `Queue`.

Example:

    import java.util.*;

    public class Main {

        public static void main(
            String[] args
        ) {

            Queue<Integer> queue =
                new LinkedList<>();

            queue.offer(10);
            queue.offer(20);
            queue.offer(30);

            System.out.println(queue);

            System.out.println(
                queue.poll()
            );

            System.out.println(queue);
        }
    }

Output:

    [10, 20, 30]
    10
    [20, 30]

---

# 21. Queue with ArrayDeque 🚀

`ArrayDeque` is designed for efficient operations at both ends.

For normal FIFO Queue usage:

    Queue<Integer> queue =
        new ArrayDeque<>();

Example:

    queue.offer(10);
    queue.offer(20);
    queue.offer(30);

    System.out.println(
        queue.poll()
    );

Output:

    10

---

## Why ArrayDeque Is Important

For many single-threaded Queue/Deque use cases:

    ArrayDeque

is generally preferred over using `LinkedList` as a Queue because it avoids the per-node overhead of a linked structure and is designed specifically for deque operations.

---

# 22. PriorityQueue ⭐

`PriorityQueue` is also a Queue implementation, but it does not represent normal FIFO behavior.

Example:

    Queue<Integer> queue =
        new PriorityQueue<>();

    queue.offer(30);
    queue.offer(10);
    queue.offer(20);

    System.out.println(
        queue.poll()
    );

Output:

    10

The smallest element has priority under natural ordering.

---

## Important

Do not think:

    Queue
        always means
    insertion-order FIFO

The `Queue` abstraction supports different ordering policies.

Examples:

    ArrayDeque
        -> FIFO when used as Queue

    PriorityQueue
        -> priority-based removal

---

# 23. Queue and null ⚠️

Different Queue implementations have different null behavior.

For example:

    ArrayDeque

does not permit null elements.

Example:

    Queue<Integer> queue =
        new ArrayDeque<>();

    queue.offer(null);

This throws:

    NullPointerException

Why?

Because `null` is used as the special return value for operations such as:

    poll()
    peek()

Allowing null would make it difficult to distinguish:

    "There is no element"

from:

    "The element itself is null"

---

## PriorityQueue

`PriorityQueue` also does not permit null elements.

---

## LinkedList

A `LinkedList` can contain null elements.

Example:

    Queue<Integer> queue =
        new LinkedList<>();

    queue.add(null);

This is allowed.

However, using null as a Queue element is generally a poor design choice because it conflicts conceptually with Queue methods that use null as an empty-result value.

---

# 24. Queue Does Not Mean Always FIFO 🔄

This is an important conceptual point.

The Queue interface represents:

> A collection designed for holding elements before processing.

The exact ordering policy depends on implementation.

Examples:

    ArrayDeque
        -> FIFO when used through Queue

    PriorityQueue
        -> priority ordering

Therefore:

    Queue
        !=
    always FIFO

More accurate:

    Queue
        -> processing-order abstraction

Commonly:

    FIFO

but implementation can define another ordering policy.

---

# 25. Queue Iteration 🔁

You can iterate through a Queue.

Example:

    Queue<Integer> queue =
        new ArrayDeque<>();

    queue.offer(10);
    queue.offer(20);
    queue.offer(30);

    for (int number : queue) {

        System.out.println(number);
    }

Output for ArrayDeque:

    10
    20
    30

---

## Important PriorityQueue Warning

Do not use iteration of a PriorityQueue as proof of sorted order.

Example:

    PriorityQueue<Integer> queue =
        new PriorityQueue<>();

    queue.offer(30);
    queue.offer(10);
    queue.offer(20);

    for (int value : queue) {

        System.out.println(value);
    }

The iteration order is not guaranteed to be globally sorted.

If you need elements in priority order, repeatedly use:

    poll()

---

# 26. Queue with Generics 🧩

Queue should normally be used with generics.

Good:

    Queue<Integer> numbers =
        new ArrayDeque<>();

    Queue<String> names =
        new ArrayDeque<>();

    Queue<Double> prices =
        new ArrayDeque<>();

Avoid raw types:

    Queue queue =
        new ArrayDeque();

Why?

Generics provide:

    compile-time type safety
    fewer casts
    clearer code

---

# 27. Internal Working ⚙️

The Queue interface itself does not specify one internal data structure.

The implementation determines how the Queue works.

---

## ArrayDeque

Conceptually uses:

    resizable array
        +
    circular/buffer-style indexing

This allows efficient insertion/removal from both ends.

---

## LinkedList

Uses linked nodes.

Conceptually:

    [10] <-> [20] <-> [30]

Each node contains references to neighboring nodes.

---

## PriorityQueue

Uses a heap-based structure.

Conceptually:

             10
            /  \
          20    30
         / \
        40  50

The smallest element is at the head under natural ordering.

---

# 28. Time Complexity ⏱️

Complexity depends on implementation.

## ArrayDeque

Typical:

| Operation | Complexity |
|---|---:|
| offer() | O(1) amortized |
| poll() | O(1) |
| peek() | O(1) |

---

## LinkedList

Typical end operations:

| Operation | Complexity |
|---|---:|
| offer() | O(1) |
| poll() | O(1) |
| peek() | O(1) |

---

## PriorityQueue

| Operation | Complexity |
|---|---:|
| offer() | O(log n) |
| poll() | O(log n) |
| peek() | O(1) |
| contains() | O(n) |
| remove(Object) | O(n) |

Important:

> Never give a Queue complexity without considering its implementation.

---

# 29. Queue vs List ⚔️

| Feature | Queue | List |
|---|---|---|
| Main purpose | Processing order | Sequence |
| Index access | No | Usually yes |
| Typical processing | FIFO/priority | Position-based |
| Main methods | offer/poll/peek | add/get/set/remove |
| Example | ArrayDeque | ArrayList |

Use Queue when your problem is about:

    who should be processed next?

Use List when your problem is about:

    what is stored at index i?

---

# 30. Queue vs Stack ⚔️

| Feature | Queue | Stack |
|---|---|---|
| Principle | FIFO | LIFO |
| Insert | Rear | Top |
| Remove | Front | Top |
| Example | Queue | Deque |
| Typical use | BFS | DFS/backtracking |

Example:

    Queue:

    10 -> 20 -> 30

    poll()
        -> 10

    Stack:

    10 -> 20 -> 30

    pop()
        -> 30

---

# 31. Queue vs Deque ⚔️

`Deque` means:

    Double Ended Queue

A Queue generally exposes:

    insert at rear
    remove from front

A Deque allows operations at both ends.

Conceptually:

    FRONT
      |
    [10] [20] [30]
                  |
                REAR

Deque allows:

    addFirst()
    addLast()

    removeFirst()
    removeLast()

    peekFirst()
    peekLast()

Therefore:

    Queue
        -> restricted end operations

    Deque
        -> both-end operations

---

# 32. Queue vs PriorityQueue ⚔️

| Feature | Queue with ArrayDeque | PriorityQueue |
|---|---|---|
| Processing | FIFO | Priority |
| Duplicates | Yes | Yes |
| poll() | Oldest element | Highest-priority element |
| offer() | O(1) amortized | O(log n) |
| peek() | O(1) | O(1) |
| Typical use | BFS | Heap problems |

Example:

    ArrayDeque:

    offer(30)
    offer(10)
    offer(20)

    poll()
        -> 30

PriorityQueue:

    offer(30)
    offer(10)
    offer(20)

    poll()
        -> 10

This difference is extremely important.

---

# 33. When to Use Queue 🎯

Use Queue when you need:

## 1. FIFO Processing

Example:

    tasks
    requests
    customers

---

## 2. BFS

Breadth-First Search uses a Queue.

---

## 3. Level Order Traversal

Binary tree level-order traversal commonly uses a Queue.

---

## 4. Producer-Consumer Design

One side produces tasks.

Another side processes them.

Conceptually:

    Producer
       |
       v
    Queue
       |
       v
    Consumer

---

## 5. Scheduling

Tasks can wait in a Queue before processing.

---

## 6. Simulation Problems

Many DSA problems simulate:

    people
    cars
    tasks
    requests
    processes

using Queue.

---

# 34. When Not to Use Queue 🚫

Do not use Queue if you need:

## 1. Random Index Access

Use:

    ArrayList

---

## 2. LIFO Behavior

Use:

    Deque

as a stack.

---

## 3. Sorted Unique Data

Use:

    TreeSet

---

## 4. Fast Membership

Use:

    HashSet

---

## 5. Priority-Based Processing

Use:

    PriorityQueue

---

# 35. Real-World Examples 🌍

## Printer Queue

    Document A
    Document B
    Document C

Processing:

    A
    B
    C

---

## Customer Service

    Customer 1
    Customer 2
    Customer 3

First customer arrives first and is normally served first.

---

## CPU Task Scheduling

    Task A
    Task B
    Task C

Tasks wait before processing.

---

## Network Requests

Requests can be placed into a queue before being processed.

---

## Message Processing

Messages can wait in a queue before consumers process them.

---

# 36. DSA Patterns 🧩

Queue is one of the most important data structures in DSA.

The major patterns are:

---

## Pattern 1 — BFS

Breadth-First Search is the most important Queue pattern.

General structure:

    add starting node

    while queue is not empty:

        remove front

        process node

        add unvisited neighbors

Pseudo-code:

    Queue<Node> queue =
        new ArrayDeque<>();

    queue.offer(start);

    while (!queue.isEmpty()) {

        Node current =
            queue.poll();

        for (
            Node neighbor :
            current.neighbors
        ) {

            if (!visited) {

                visited = true;

                queue.offer(
                    neighbor
                );
            }
        }
    }

---

## Pattern 2 — Level Order Traversal

For a binary tree:

    Queue<TreeNode> queue =
        new ArrayDeque<>();

    queue.offer(root);

    while (!queue.isEmpty()) {

        int size = queue.size();

        for (
            int i = 0;
            i < size;
            i++
        ) {

            TreeNode node =
                queue.poll();

            // process node

            if (node.left != null) {

                queue.offer(
                    node.left
                );
            }

            if (node.right != null) {

                queue.offer(
                    node.right
                );
            }
        }
    }

The key pattern is:

    int size = queue.size();

    for (
        int i = 0;
        i < size;
        i++
    )

This lets you process one complete level at a time.

---

## Pattern 3 — Multi-Source BFS

Instead of adding one starting point:

    queue.offer(source);

you add multiple sources initially.

Example:

    for each source:

        queue.offer(source);

Then perform BFS.

This is useful for:

    Rotting Oranges
    distance problems
    spreading processes
    shortest distance from multiple sources

---

## Pattern 4 — Shortest Path in Unweighted Graph

For an unweighted graph:

    BFS
       +
    Queue

can find the shortest number of edges from a source.

General idea:

    source
      |
      v
    Queue
      |
      v
    BFS
      |
      v
    shortest distance

---

## Pattern 5 — Sliding Window with Deque

Important distinction:

Basic Queue:

    FIFO

Deque:

    both ends

Many advanced sliding-window problems use:

    Deque

rather than a simple Queue.

Typical pattern:

    remove elements from front
    add elements at back

or:

    remove smaller elements from back

This is used in:

    Sliding Window Maximum

---

## Pattern 6 — Monotonic Queue

A Deque can maintain elements in increasing or decreasing order.

Conceptually:

    [large ... small]

or:

    [small ... large]

This allows efficient window maximum/minimum problems.

---

## Pattern 7 — Simulation

When a problem says:

    first person waits
    first request processed
    first task executed

think:

    Queue

---

## Pattern 8 — Producer / Consumer

Tasks enter:

    Queue

Consumers remove:

    poll()

This models:

    producer
        |
        v
      Queue
        |
        v
     consumer

---

## Pattern 9 — Topological Processing

Kahn's Algorithm for topological sorting uses a Queue.

General idea:

    calculate indegree

    add all nodes
    with indegree 0

    while queue not empty:

        node = poll()

        process node

        decrease neighbor indegree

        if indegree == 0:

            offer(neighbor)

---

## Pattern 10 — BFS State Search

In problems where each state produces new states:

    current state
         |
         v
    generate states
         |
         v
       Queue
         |
         v
    process next state

Examples include:

    grid problems
    word transformations
    shortest transformation
    state-space search

---

# 37. DSA Problem-Solving Examples 🧠

## Problem 1 — Basic Queue Processing

Given:

    [10, 20, 30]

Process all elements in FIFO order.

Code:

    Queue<Integer> queue =
        new ArrayDeque<>();

    queue.offer(10);
    queue.offer(20);
    queue.offer(30);

    while (!queue.isEmpty()) {

        int value =
            queue.poll();

        System.out.println(value);
    }

Output:

    10
    20
    30

Pattern:

    FIFO processing

---

# Problem 2 — BFS Traversal

Graph:

    0 --- 1
    |     |
    2 --- 3

Adjacency list:

    0 -> [1, 2]
    1 -> [0, 3]
    2 -> [0, 3]
    3 -> [1, 2]

BFS from 0:

    Queue:

    [0]

Process:

    0

Add:

    1, 2

Queue:

    [1, 2]

Process:

    1

Add:

    3

Queue:

    [2, 3]

Process:

    2

Queue:

    [3]

Process:

    3

Traversal:

    0 1 2 3

Code:

    import java.util.*;

    public class Main {

        static void bfs(
            List<List<Integer>> graph,
            int start
        ) {

            boolean[] visited =
                new boolean[
                    graph.size()
                ];

            Queue<Integer> queue =
                new ArrayDeque<>();

            queue.offer(start);
            visited[start] = true;

            while (!queue.isEmpty()) {

                int current =
                    queue.poll();

                System.out.print(
                    current + " "
                );

                for (
                    int neighbor :
                    graph.get(current)
                ) {

                    if (
                        !visited[neighbor]
                    ) {

                        visited[neighbor] =
                            true;

                        queue.offer(
                            neighbor
                        );
                    }
                }
            }
        }
    }

Important pattern:

    Queue
       +
    visited[]
       +
    BFS

---

# Problem 3 — Binary Tree Level Order Traversal

For:

              1
             / \
            2   3
           / \
          4   5

Level order:

    1
    2 3
    4 5

Code:

    import java.util.*;

    class TreeNode {

        int value;

        TreeNode left;
        TreeNode right;

        TreeNode(int value) {

            this.value = value;
        }
    }

    public class Main {

        static void levelOrder(
            TreeNode root
        ) {

            if (root == null) {
                return;
            }

            Queue<TreeNode> queue =
                new ArrayDeque<>();

            queue.offer(root);

            while (!queue.isEmpty()) {

                int size =
                    queue.size();

                for (
                    int i = 0;
                    i < size;
                    i++
                ) {

                    TreeNode node =
                        queue.poll();

                    System.out.print(
                        node.value + " "
                    );

                    if (
                        node.left != null
                    ) {

                        queue.offer(
                            node.left
                        );
                    }

                    if (
                        node.right != null
                    ) {

                        queue.offer(
                            node.right
                        );
                    }
                }

                System.out.println();
            }
        }
    }

Pattern:

    queue.size()
        ->
    process one level

---

# Problem 4 — Shortest Distance in Unweighted Graph

Suppose every edge has equal cost.

Then BFS can find the shortest distance.

General idea:

    distance[start] = 0

    Queue:
        start

For every neighbor:

    distance[neighbor]
        =
    distance[current] + 1

Code:

    Queue<Integer> queue =
        new ArrayDeque<>();

    queue.offer(start);

    distance[start] = 0;

    while (!queue.isEmpty()) {

        int current =
            queue.poll();

        for (
            int neighbor :
            graph.get(current)
        ) {

            if (
                distance[neighbor] == -1
            ) {

                distance[neighbor] =
                    distance[current] + 1;

                queue.offer(
                    neighbor
                );
            }
        }
    }

Important:

> BFS gives shortest path by number of edges in an unweighted graph.

---

# Problem 5 — Kahn's Algorithm

Topological sorting of a directed graph can be done using:

    indegree[]
        +
    Queue

General algorithm:

    1. Calculate indegree.

    2. Add all nodes with
       indegree 0.

    3. Poll a node.

    4. Process it.

    5. Decrease indegree
       of its neighbors.

    6. If a neighbor becomes
       indegree 0, add it.

Code pattern:

    Queue<Integer> queue =
        new ArrayDeque<>();

    for (
        int i = 0;
        i < n;
        i++
    ) {

        if (indegree[i] == 0) {

            queue.offer(i);
        }
    }

    while (!queue.isEmpty()) {

        int node =
            queue.poll();

        for (
            int neighbor :
            graph.get(node)
        ) {

            indegree[neighbor]--;

            if (
                indegree[neighbor] == 0
            ) {

                queue.offer(
                    neighbor
                );
            }
        }
    }

Pattern:

    indegree
       +
    Queue
       =
    Kahn's Algorithm

---

# Problem 6 — Multi-Source BFS

Suppose several cells are starting points.

Instead of:

    queue.offer(source);

for one source, add all sources first:

    for each source {

        queue.offer(source);
        distance[source] = 0;
    }

Then:

    while (!queue.isEmpty()) {

        current = queue.poll();

        ...
    }

All sources begin BFS simultaneously.

This is useful for:

    Rotting Oranges
    nearest source
    distance to nearest 1
    fire spreading
    infection spreading

---

# Problem 7 — Queue-Based Simulation

Suppose customers arrive:

    A
    B
    C

Queue:

    [A, B, C]

Process:

    A

Queue:

    [B, C]

Add:

    D

Queue:

    [B, C, D]

Process:

    B

This is the standard simulation pattern:

    offer()
        ->
    add to waiting line

    poll()
        ->
    process next

---

# 38. Common Mistakes ⚠️

## Mistake 1 — Calling get(0)

Queue does not provide:

    get(0)

Use:

    peek()

to inspect the head.

---

## Mistake 2 — Using remove() Without Checking Empty

This can throw:

    NoSuchElementException

Safer:

    poll()

when you want special-value behavior.

---

## Mistake 3 — Confusing peek() and poll()

    peek()
        -> inspect

    poll()
        -> remove

Memory:

    peek
        -> look

    poll
        -> take

---

## Mistake 4 — Confusing remove() and poll()

    remove()
        -> exception when empty

    poll()
        -> null when empty

---

## Mistake 5 — Assuming PriorityQueue Is FIFO

PriorityQueue processes according to priority/order, not insertion order.

---

## Mistake 6 — Assuming PriorityQueue Iteration Is Sorted

It is not guaranteed to be sorted.

Use:

    poll()

repeatedly for priority order.

---

## Mistake 7 — Adding null to ArrayDeque

Not allowed.

---

## Mistake 8 — Using List When You Need Queue Semantics

If your problem says:

    first arrived
    first processed

Queue communicates the intention better.

---

## Mistake 9 — Using Stack for BFS

BFS requires:

    Queue

DFS commonly uses:

    Stack/Deque

---

# 39. Interview Traps 🎤

## Trap 1

Question:

> Is Queue a class?

No.

`Queue` is an interface.

---

## Trap 2

Question:

> Which interface does Queue extend?

    Collection

---

## Trap 3

Question:

> Does Queue always mean FIFO?

No.

Common Queue implementations can have different ordering policies.

`ArrayDeque` used as Queue gives FIFO behavior.

`PriorityQueue` uses priority ordering.

---

## Trap 4

Question:

> What are the six major Queue methods?

    add()
    offer()

    remove()
    poll()

    element()
    peek()

---

## Trap 5

Question:

> Difference between offer() and add()?

Both insert.

    add()
        -> exception on insertion failure

    offer()
        -> false on insertion failure

---

## Trap 6

Question:

> Difference between poll() and remove()?

Both remove the head.

    poll()
        -> null if empty

    remove()
        -> exception if empty

---

## Trap 7

Question:

> Difference between peek() and element()?

Both inspect the head.

    peek()
        -> null if empty

    element()
        -> exception if empty

---

## Trap 8

Question:

> Which Queue implementation is useful for BFS?

    ArrayDeque

commonly used as:

    Queue

---

## Trap 9

Question:

> Which Queue is used for priority-based processing?

    PriorityQueue

---

## Trap 10

Question:

> What is the best general-purpose stack/deque implementation?

    ArrayDeque

is generally preferred for many single-threaded use cases over the legacy `Stack` class.

---

## Trap 11

Question:

> Why is Queue important in DSA?

Because it is fundamental to:

    BFS
    level-order traversal
    shortest path in unweighted graphs
    multi-source BFS
    simulations
    topological sorting with Kahn's algorithm

---

## Trap 12

Question:

> Can Queue contain duplicate elements?

Yes.

Queue does not enforce uniqueness.

Example:

    [10, 10, 20]

is valid.

---

## Trap 13

Question:

> Does Queue support index access?

No.

Queue is designed around:

    head
    tail

rather than:

    index

---

# 40. Top Interview Questions 🎤

## Q1. What is Queue?

Queue is an interface in Java's Collections Framework representing a collection designed for holding elements before processing.

---

## Q2. Which interface does Queue extend?

    Collection<E>

---

## Q3. What is the normal Queue principle?

    FIFO

First In, First Out.

---

## Q4. What are the common Queue implementations?

    LinkedList
    ArrayDeque
    PriorityQueue

---

## Q5. What are the six main Queue methods?

    add()
    offer()

    remove()
    poll()

    element()
    peek()

---

## Q6. Difference between add() and offer()?

Both insert an element.

`add()` may throw an exception if insertion fails.

`offer()` returns `false` if insertion fails.

---

## Q7. Difference between remove() and poll()?

Both remove the head.

`remove()` throws an exception if empty.

`poll()` returns `null` if empty.

---

## Q8. Difference between element() and peek()?

Both inspect the head.

`element()` throws an exception if empty.

`peek()` returns `null` if empty.

---

## Q9. Can Queue contain duplicates?

Yes.

---

## Q10. Can ArrayDeque contain null?

No.

---

## Q11. Can LinkedList contain null?

Yes.

But null is generally not recommended when using it as a Queue because null is also used as an empty-result value by `poll()` and `peek()`.

---

## Q12. What is FIFO?

First In, First Out.

---

## Q13. What is LIFO?

Last In, First Out.

LIFO is associated with stacks.

---

## Q14. Queue or Stack for BFS?

    Queue

---

## Q15. Queue or Stack for DFS?

A stack-like structure, commonly:

    Deque

---

## Q16. Which Queue implementation uses priority ordering?

    PriorityQueue

---

## Q17. Is PriorityQueue FIFO?

No.

---

## Q18. Is PriorityQueue fully sorted?

No.

It maintains heap ordering sufficient to identify the highest-priority element.

---

## Q19. What is the complexity of PriorityQueue.offer()?

    O(log n)

---

## Q20. What is the complexity of PriorityQueue.peek()?

    O(1)

---

## Q21. What is the complexity of PriorityQueue.poll()?

    O(log n)

---

## Q22. Why is Queue used in BFS?

Because BFS explores nodes level by level, and FIFO processing ensures earlier-discovered nodes are processed before later-discovered nodes.

---

## Q23. What is multi-source BFS?

BFS where multiple starting nodes are inserted into the Queue initially.

---

## Q24. Which algorithm uses Queue for topological sorting?

    Kahn's Algorithm

---

## Q25. What is the main difference between Queue and Deque?

Queue normally exposes one-directional processing behavior, while Deque supports insertion and removal at both ends.

---

## Q26. Why is ArrayDeque commonly preferred over LinkedList for Queue use?

It is specifically optimized for deque operations and avoids the per-node overhead of a linked structure.

---

## Q27. Does Queue provide index access?

No.

---

## Q28. Does Queue enforce uniqueness?

No.

---

## Q29. What is the difference between Queue and PriorityQueue?

Queue implementations such as ArrayDeque process in FIFO order, while PriorityQueue processes according to priority.

---

## Q30. What is the most important DSA use of Queue?

    BFS

---

# 41. 30-Second Interview Answer 🎯

If the interviewer asks:

> "Explain Queue in Java."

Answer:

> "`Queue` is an interface in Java's Collections Framework that extends `Collection` and represents elements waiting to be processed. The standard Queue behavior is FIFO, or First In First Out. Its main methods are `offer`, `poll`, and `peek`, with corresponding exception-based methods `add`, `remove`, and `element`. Common implementations include `ArrayDeque`, `LinkedList`, and `PriorityQueue`. In DSA, Queue is especially important for BFS, level-order traversal, shortest paths in unweighted graphs, multi-source BFS, simulations, and Kahn's topological sorting algorithm."

---

# 42. Cheat Sheet 📋

## Queue

    Queue<E>

    extends:

    Collection<E>

---

## Main Principle

    FIFO

    First In
        ->
    First Out

---

## Insert

    add(e)
        -> exception if failure

    offer(e)
        -> false if failure

---

## Remove

    remove()
        -> exception if empty

    poll()
        -> null if empty

---

## Examine

    element()
        -> exception if empty

    peek()
        -> null if empty

---

## Method Pairs

    add      <-> offer

    remove   <-> poll

    element  <-> peek

---

## Implementations

    ArrayDeque
        -> FIFO/Deque

    LinkedList
        -> List + Queue + Deque

    PriorityQueue
        -> priority-based

---

## DSA

    Queue
        |
        +-- BFS
        |
        +-- Level Order
        |
        +-- Shortest Path
        |
        +-- Multi-Source BFS
        |
        +-- Simulation
        |
        +-- Kahn's Algorithm

---

## Complexity

    ArrayDeque:

    offer()
        -> O(1) amortized

    poll()
        -> O(1)

    peek()
        -> O(1)

    PriorityQueue:

    offer()
        -> O(log n)

    poll()
        -> O(log n)

    peek()
        -> O(1)

---

# 43. Quick Revision ⚡

Remember:

    1. Queue is an interface.

    2. Queue extends Collection.

    3. Normal Queue behavior is FIFO.

    4. Queue does not enforce uniqueness.

    5. Queue does not provide index access.

    6. add() inserts.

    7. offer() inserts.

    8. remove() removes the head.

    9. poll() removes the head.

    10. element() examines the head.

    11. peek() examines the head.

    12. add() may throw an exception on insertion failure.

    13. offer() returns false on insertion failure.

    14. remove() throws an exception when empty.

    15. poll() returns null when empty.

    16. element() throws an exception when empty.

    17. peek() returns null when empty.

    18. ArrayDeque is commonly used for FIFO Queue behavior.

    19. LinkedList can also implement Queue.

    20. PriorityQueue uses priority ordering.

    21. PriorityQueue is not FIFO.

    22. PriorityQueue iteration is not guaranteed to be sorted.

    23. ArrayDeque does not allow null.

    24. Queue can contain duplicates.

    25. Queue is fundamental to BFS.

    26. Queue is used in level-order traversal.

    27. Queue is used for shortest paths in unweighted graphs.

    28. Queue is used in multi-source BFS.

    29. Queue is used in Kahn's algorithm.

    30. Queue is useful for simulations.

---

# Final Memory Trick 🧠

Remember the three pairs:

    INSERT
        add()
        offer()

    REMOVE
        remove()
        poll()

    EXAMINE
        element()
        peek()

And remember the behavior:

    Queue
        =
    FIFO

    Stack
        =
    LIFO

    PriorityQueue
        =
    Priority

For DSA:

    BFS?
        -> Queue

    Level Order?
        -> Queue

    Unweighted Shortest Path?
        -> Queue + BFS

    Multi-Source BFS?
        -> Queue

    Topological Sort?
        -> Queue + Kahn's Algorithm

    Sliding Window Maximum?
        -> Deque

    Priority Processing?
        -> PriorityQueue