# 06 — LinkedList

> **Package:** `java.util`  
> **Since:** Java 1.2  
> **Implements:** `List<E>`, `Deque<E>`, `Cloneable`, `Serializable`  
> **Internal Structure:** Doubly linked list  
> **Purpose:** A List and Deque implementation based on linked nodes.

---

# 📚 Table of Contents

1. [Introduction](#1-introduction)
2. [Why LinkedList?](#2-why-linkedlist)
3. [LinkedList Hierarchy](#3-linkedlist-hierarchy)
4. [What is LinkedList?](#4-what-is-linkedlist)
5. [Important Characteristics](#5-important-characteristics)
6. [Creating a LinkedList](#6-creating-a-linkedlist)
7. [Adding Elements](#7-adding-elements)
8. [Accessing Elements](#8-accessing-elements)
9. [Updating Elements](#9-updating-elements)
10. [Removing Elements](#10-removing-elements)
11. [First and Last Element Operations](#11-first-and-last-element-operations)
12. [Queue Operations](#12-queue-operations)
13. [Deque Operations](#13-deque-operations)
14. [Stack-Like Operations](#14-stack-like-operations)
15. [Internal Working](#15-internal-working)
16. [Node Structure](#16-node-structure)
17. [How Traversal Works](#17-how-traversal-works)
18. [Insertion Internally](#18-insertion-internally)
19. [Removal Internally](#19-removal-internally)
20. [Why get(index) is O(n)](#20-why-getindex-is-on)
21. [Why LinkedList Can Still Insert Efficiently](#21-why-linkedlist-can-still-insert-efficiently)
22. [Iterator and ListIterator](#22-iterator-and-listiterator)
23. [Descending Iterator](#23-descending-iterator)
24. [Searching](#24-searching)
25. [SubList](#25-sublist)
26. [Clone](#26-clone)
27. [Array Conversion](#27-array-conversion)
28. [Null and Duplicate Elements](#28-null-and-duplicate-elements)
29. [Time Complexity](#29-time-complexity)
30. [Memory Usage](#30-memory-usage)
31. [ArrayList vs LinkedList](#31-arraylist-vs-linkedlist)
32. [LinkedList vs Array](#32-linkedlist-vs-array)
33. [LinkedList as Queue](#33-linkedlist-as-queue)
34. [LinkedList as Deque](#34-linkedlist-as-deque)
35. [LinkedList as Stack](#35-linkedlist-as-stack)
36. [Common Mistakes](#36-common-mistakes)
37. [Interview Traps](#37-interview-traps)
38. [DSA Relevance](#38-dsa-relevance)
39. [DSA Examples](#39-dsa-examples)
40. [Top Interview Questions](#40-top-interview-questions)
41. [30-Second Interview Answer](#41-30-second-interview-answer)
42. [Cheat Sheet](#42-cheat-sheet)
43. [Quick Revision](#43-quick-revision)

---

# 🚀 1. Introduction

`LinkedList` is a class from the Java Collection Framework.

It implements both:

    List<E>

and:

    Deque<E>

This makes LinkedList different from ArrayList.

Its standard implementation is based on a:

    Doubly Linked List

Package:

    java.util.LinkedList

Basic relationship:

    Iterable
        |
    Collection
        |
       List
        |
    LinkedList
        |
       Deque

Example:

    import java.util.*;

    public class Main {

        public static void main(String[] args) {

            LinkedList<String> names =
                new LinkedList<>();

            names.add("Java");
            names.add("Spring");
            names.add("React");

            System.out.println(names);
        }
    }

Output:

    [Java, Spring, React]

---

# 🤔 2. Why LinkedList?

A major limitation of an array-based structure is that inserting or removing elements from the middle can require shifting many elements.

For example:

    [A][B][C][D][E]

Insert `X` at index `2`:

    [A][B][X][C][D][E]

In an array-based structure, elements may need to be shifted.

A linked list works differently.

Conceptually:

    A <-> B <-> C <-> D <-> E

Insert X between B and C:

    A <-> B <-> X <-> C <-> D <-> E

Only links around the insertion point need to be changed.

However, there is an important catch:

> LinkedList must first reach the required position.

Therefore:

    Finding position
        -> O(n)

while:

    Relinking nodes
        -> O(1)

if the node position is already known.

This distinction is extremely important in interviews.

---

# 🌳 3. LinkedList Hierarchy

A simplified hierarchy:

    Iterable<E>
         |
    Collection<E>
         |
       List<E>
         |
    LinkedList<E>
         |
       Deque<E>

LinkedList also implements:

    Cloneable
    Serializable

Important:

Unlike ArrayList, LinkedList also implements:

    Deque

This allows it to work as:

    List
    Queue
    Deque
    Stack-like structure

---

# 🧠 4. What is LinkedList?

`LinkedList` is a doubly linked list implementation of both the `List` and `Deque` interfaces.

Each node conceptually contains:

    previous reference
    element
    next reference

Example:

    null
      |
      v
    [prev | A | next]
                |
                v
          [prev | B | next]
                      |
                      v
                [prev | C | next]
                              |
                              v
                             null

Therefore:

    A <-> B <-> C

Each node knows:

    previous node
    next node

This allows traversal in both directions.

---

# 🔑 5. Important Characteristics

## 5.1 Maintains Insertion Order

Example:

    LinkedList<String> names =
        new LinkedList<>();

    names.add("A");
    names.add("C");
    names.add("B");

    System.out.println(names);

Output:

    [A, C, B]

---

## 5.2 Allows Duplicates

    LinkedList<Integer> numbers =
        new LinkedList<>();

    numbers.add(10);
    numbers.add(10);
    numbers.add(20);

Output:

    [10, 10, 20]

---

## 5.3 Allows Null

    LinkedList<String> names =
        new LinkedList<>();

    names.add(null);

    System.out.println(names);

Output:

    [null]

---

## 5.4 Dynamic Size

Unlike an array:

    LinkedList<Integer> numbers =
        new LinkedList<>();

    numbers.add(10);
    numbers.add(20);
    numbers.add(30);

No fixed capacity needs to be declared.

---

## 5.5 Doubly Linked

Standard Java LinkedList uses a doubly linked structure.

Conceptually:

    A <-> B <-> C <-> D

Each node contains links in both directions.

---

## 5.6 Supports List and Deque

This is one of the most important features.

As a List:

    list.get(index)

As a Queue:

    queue.offer(element)

As a Deque:

    deque.addFirst(element)
    deque.addLast(element)

As a Stack-like structure:

    deque.push(element)
    deque.pop()

---

# 💻 6. Creating a LinkedList

## 6.1 Empty LinkedList

    LinkedList<String> names =
        new LinkedList<>();

---

## 6.2 Using List Reference

    List<String> names =
        new LinkedList<>();

This allows List operations.

But Deque-specific methods are not directly available through the `List` reference.

---

## 6.3 Using Deque Reference

    Deque<String> deque =
        new LinkedList<>();

Now Deque operations are available:

    deque.addFirst("A");
    deque.addLast("B");

---

## 6.4 Creating from Collection

    LinkedList<String> names =
        new LinkedList<>(
            List.of("Java", "Spring", "React")
        );

System output:

    [Java, Spring, React]

---

# ➕ 7. Adding Elements

## 7.1 add(E)

Adds at the end.

    LinkedList<String> names =
        new LinkedList<>();

    names.add("Java");
    names.add("Spring");

    System.out.println(names);

Output:

    [Java, Spring]

---

## 7.2 add(index, element)

    LinkedList<String> names =
        new LinkedList<>(
            List.of("Java", "React")
        );

    names.add(1, "Spring");

    System.out.println(names);

Output:

    [Java, Spring, React]

---

## 7.3 addFirst()

Adds at the beginning.

    LinkedList<String> names =
        new LinkedList<>(
            List.of("Spring", "React")
        );

    names.addFirst("Java");

    System.out.println(names);

Output:

    [Java, Spring, React]

---

## 7.4 addLast()

Adds at the end.

    LinkedList<String> names =
        new LinkedList<>(
            List.of("Java", "Spring")
        );

    names.addLast("React");

    System.out.println(names);

Output:

    [Java, Spring, React]

---

## 7.5 addAll()

    LinkedList<String> first =
        new LinkedList<>(
            List.of("Java", "Spring")
        );

    LinkedList<String> second =
        new LinkedList<>(
            List.of("React", "Docker")
        );

    first.addAll(second);

    System.out.println(first);

Output:

    [Java, Spring, React, Docker]

---

## 7.6 addAll(index, collection)

    LinkedList<String> first =
        new LinkedList<>(
            List.of("Java", "React")
        );

    first.addAll(
        1,
        List.of("Spring", "Hibernate")
    );

    System.out.println(first);

Output:

    [Java, Spring, Hibernate, React]

---

# 👀 8. Accessing Elements

## get(index)

    LinkedList<String> names =
        new LinkedList<>(
            List.of("Java", "Spring", "React")
        );

    System.out.println(
        names.get(1)
    );

Output:

    Spring

Important:

Unlike ArrayList:

    LinkedList.get(index)

is typically:

    O(n)

---

## getFirst()

    System.out.println(
        names.getFirst()
    );

Output:

    Java

---

## getLast()

    System.out.println(
        names.getLast()
    );

Output:

    React

---

## peekFirst()

Returns the first element without removing it.

    System.out.println(
        names.peekFirst()
    );

---

## peekLast()

    System.out.println(
        names.peekLast()
    );

---

# 🔄 9. Updating Elements

LinkedList implements List, so it supports:

    set(index, element)

Example:

    LinkedList<String> names =
        new LinkedList<>(
            List.of("Java", "Spring", "React")
        );

    names.set(1, "Hibernate");

    System.out.println(names);

Output:

    [Java, Hibernate, React]

---

# ❌ 10. Removing Elements

## 10.1 remove()

Removes the first element.

    LinkedList<String> names =
        new LinkedList<>(
            List.of("Java", "Spring", "React")
        );

    names.remove();

    System.out.println(names);

Output:

    [Spring, React]

---

## 10.2 remove(index)

    names.remove(1);

Removes the element at index 1.

---

## 10.3 remove(Object)

    names.remove("Spring");

Removes the first matching occurrence.

---

## 10.4 removeFirst()

    names.removeFirst();

---

## 10.5 removeLast()

    names.removeLast();

---

## 10.6 removeFirstOccurrence()

    LinkedList<Integer> numbers =
        new LinkedList<>(
            List.of(10, 20, 10, 30)
        );

    numbers.removeFirstOccurrence(10);

    System.out.println(numbers);

Output:

    [20, 10, 30]

---

## 10.7 removeLastOccurrence()

    LinkedList<Integer> numbers =
        new LinkedList<>(
            List.of(10, 20, 10, 30)
        );

    numbers.removeLastOccurrence(10);

    System.out.println(numbers);

Output:

    [10, 20, 30]

---

## 10.8 clear()

    names.clear();

Result:

    []

---

# 🔝 11. First and Last Element Operations

LinkedList provides many convenient methods for its two ends.

## First Element

    addFirst()
    offerFirst()
    push()

    getFirst()
    peekFirst()

    removeFirst()
    pollFirst()

---

## Last Element

    addLast()
    offerLast()

    getLast()
    peekLast()

    removeLast()
    pollLast()

These operations are generally efficient because LinkedList maintains references to both ends.

---

# 🚚 12. Queue Operations

LinkedList implements `Queue` indirectly through `Deque`.

Queue concept:

    FIFO

    First In
    First Out

Example:

    Queue<String> queue =
        new LinkedList<>();

    queue.offer("A");
    queue.offer("B");
    queue.offer("C");

    System.out.println(queue);

Output:

    [A, B, C]

Remove:

    System.out.println(
        queue.poll()
    );

Output:

    A

Remaining:

    [B, C]

---

## Queue Methods

    offer()
        -> insert

    poll()
        -> remove and return head

    peek()
        -> return head without removing

---

# ↔️ 13. Deque Operations

Deque means:

    Double Ended Queue

It allows insertion and removal from both ends.

Example:

    Deque<Integer> deque =
        new LinkedList<>();

    deque.addFirst(20);
    deque.addLast(30);
    deque.addFirst(10);

    System.out.println(deque);

Output:

    [10, 20, 30]

Remove first:

    deque.removeFirst();

Remove last:

    deque.removeLast();

---

## Deque Method Families

First end:

    addFirst()
    offerFirst()

    removeFirst()
    pollFirst()

    getFirst()
    peekFirst()

Last end:

    addLast()
    offerLast()

    removeLast()
    pollLast()

    getLast()
    peekLast()

---

# 📚 14. Stack-Like Operations

A stack follows:

    LIFO

    Last In
    First Out

LinkedList can provide stack-like operations through Deque methods.

Example:

    Deque<Integer> stack =
        new LinkedList<>();

    stack.push(10);
    stack.push(20);
    stack.push(30);

    System.out.println(stack);

Output:

    [30, 20, 10]

Pop:

    System.out.println(
        stack.pop()
    );

Output:

    30

Remaining:

    [20, 10]

Important:

For modern Java code, `Deque` implementations such as `ArrayDeque` are generally preferred for stack/queue use cases when their semantics fit the problem.

---

# ⚙️ 15. Internal Working

This is the most important LinkedList concept.

LinkedList does NOT use one continuous array.

Instead, it maintains linked nodes.

Conceptually:

    head
     |
     v
    [A]
     |
     v
    [B]
     |
     v
    [C]
     |
     v
    tail

Because it is doubly linked:

    A <-> B <-> C

A simplified node structure is:

    Node<E> {

        E item;

        Node<E> next;

        Node<E> prev;
    }

The actual JDK implementation uses an internal Node structure similar to this conceptual model.

---

# 🧩 16. Node Structure

Suppose:

    LinkedList<Integer> numbers =
        new LinkedList<>();

    numbers.add(10);
    numbers.add(20);
    numbers.add(30);

Conceptually:

    head
     |
     v

    null <-> [10] <-> [20] <-> [30] <-> null
                                      ^
                                      |
                                     tail

Each node contains:

    prev
    item
    next

For node `20`:

    prev -> 10
    item -> 20
    next -> 30

For node `10`:

    prev -> null
    item -> 10
    next -> 20

For node `30`:

    prev -> 20
    item -> 30
    next -> null

---

# 🚶 17. How Traversal Works

Suppose:

    A <-> B <-> C <-> D <-> E

We request:

    get(4)

LinkedList does not directly jump to index 4 like an array.

It must traverse.

However, LinkedList can choose the closer direction.

Conceptually:

    If index is near beginning:
        start from head

    If index is near end:
        start from tail

For:

    get(1)

start from:

    head

For:

    get(4)

in a list of size 5, starting from:

    tail

may be more efficient.

Therefore LinkedList's traversal can be approximately:

    O(min(index, size - index))

but in Big-O terms, arbitrary indexed access is:

    O(n)

---

# 🔗 18. Insertion Internally

Suppose:

    A <-> B <-> C

We want to insert:

    X

between B and C.

Before:

    B <-> C

After:

    B <-> X <-> C

Conceptually, links change:

    B.next = X
    X.prev = B

    X.next = C
    C.prev = X

Only nearby links need modification.

Therefore:

> Once the target node is already known, insertion can be O(1).

But if the user asks:

    add(100, X)

LinkedList first needs to locate index 100.

That traversal costs:

    O(n)

Then the actual relinking is efficient.

This distinction is one of the biggest LinkedList interview traps.

---

# ❌ 19. Removal Internally

Suppose:

    A <-> B <-> C <-> D

Remove:

    C

Before:

    B <-> C <-> D

After:

    B <-> D

Conceptually:

    B.next = D
    D.prev = B

The removed node is disconnected.

Therefore, if the node is already known:

    unlink(node)

can be O(1).

But finding the node by index or value can require traversal.

---

# 🎯 20. Why get(index) is O(n)

Consider:

    [A] <-> [B] <-> [C] <-> [D] <-> [E]

For:

    get(4)

LinkedList cannot calculate an address like:

    base + index

because nodes are not stored as one continuous array.

It must follow links:

    A -> B -> C -> D -> E

or from the tail:

    E -> D -> C -> B -> A

Therefore:

    get(index)
        -> O(n)

This is a major difference from ArrayList.

---

# ⚡ 21. Why LinkedList Can Still Insert Efficiently

Consider:

    A <-> B <-> C

Suppose we already have a reference to node B.

Insert X after B:

    A <-> B <-> X <-> C

Only a few references need to change.

Therefore:

    Relinking
        -> O(1)

But:

    Find node
        -> O(n)

So saying:

> "LinkedList insertion is always O(1)"

is incomplete.

A better statement is:

> Insertion/removal is O(1) once the relevant node position is already known, but locating an arbitrary index generally takes O(n).

---

# 🔄 22. Iterator and ListIterator

LinkedList supports:

    Iterator
    ListIterator

Example:

    LinkedList<String> names =
        new LinkedList<>(
            List.of("Java", "Spring", "React")
        );

    Iterator<String> iterator =
        names.iterator();

    while (iterator.hasNext()) {

        System.out.println(
            iterator.next()
        );
    }

Output:

    Java
    Spring
    React

---

## ListIterator

    ListIterator<String> iterator =
        names.listIterator();

    while (iterator.hasNext()) {
        System.out.println(iterator.next());
    }

Backward traversal:

    ListIterator<String> iterator =
        names.listIterator(names.size());

    while (iterator.hasPrevious()) {
        System.out.println(
            iterator.previous()
        );
    }

Output:

    React
    Spring
    Java

---

# 🔽 23. Descending Iterator

Because LinkedList implements Deque, it provides:

    descendingIterator()

Example:

    LinkedList<String> names =
        new LinkedList<>(
            List.of("A", "B", "C")
        );

    Iterator<String> iterator =
        names.descendingIterator();

    while (iterator.hasNext()) {

        System.out.println(
            iterator.next()
        );
    }

Output:

    C
    B
    A

This is useful when you want to traverse from the tail toward the head.

---

# 🔍 24. Searching

LinkedList inherits List search methods.

## contains()

    LinkedList<String> names =
        new LinkedList<>(
            List.of("Java", "Spring", "React")
        );

    System.out.println(
        names.contains("Spring")
    );

Output:

    true

---

## indexOf()

    System.out.println(
        names.indexOf("Spring")
    );

Output:

    1

---

## lastIndexOf()

    LinkedList<String> names =
        new LinkedList<>(
            List.of("Java", "Spring", "Java")
        );

    System.out.println(
        names.lastIndexOf("Java")
    );

Output:

    2

Searching is generally:

    O(n)

---

# ✂️ 25. SubList

LinkedList also supports:

    subList()

Example:

    LinkedList<String> names =
        new LinkedList<>(
            List.of("A", "B", "C", "D", "E")
        );

    List<String> sub =
        names.subList(1, 4);

    System.out.println(sub);

Output:

    [B, C, D]

The returned sublist is a view backed by the original list.

Therefore:

    sub.set(0, "X");

can affect the original list.

---

# 🧬 26. Clone

LinkedList implements:

    Cloneable

It supports:

    clone()

Example:

    LinkedList<String> original =
        new LinkedList<>(
            List.of("Java", "Spring")
        );

    LinkedList<String> copy =
        (LinkedList<String>) original.clone();

    System.out.println(copy);

Output:

    [Java, Spring]

Important:

> The clone is shallow.

The list structure is copied, but referenced element objects are not recursively cloned.

---

# 🔄 27. Array Conversion

## toArray()

    LinkedList<String> names =
        new LinkedList<>(
            List.of("Java", "Spring")
        );

    Object[] array =
        names.toArray();

---

## Typed Array

    String[] array =
        names.toArray(new String[0]);

---

## Modern Form

    String[] array =
        names.toArray(String[]::new);

---

# ⚠️ 28. Null and Duplicate Elements

LinkedList allows:

    duplicate elements

and:

    null elements

Example:

    LinkedList<String> names =
        new LinkedList<>();

    names.add("Java");
    names.add("Java");
    names.add(null);
    names.add(null);

    System.out.println(names);

Output:

    [Java, Java, null, null]

---

# ⏱️ 29. Time Complexity

Typical LinkedList complexity:

| Operation | Typical Complexity |
|---|---:|
| `get(index)` | O(n) |
| `set(index)` | O(n) |
| `add(element)` at end | O(1) |
| `addFirst()` | O(1) |
| `addLast()` | O(1) |
| `removeFirst()` | O(1) |
| `removeLast()` | O(1) |
| `add(index, element)` | O(n) |
| `remove(index)` | O(n) |
| `contains()` | O(n) |
| `indexOf()` | O(n) |
| `size()` | O(1) |
| `peekFirst()` | O(1) |
| `peekLast()` | O(1) |
| `pollFirst()` | O(1) |
| `pollLast()` | O(1) |

Important:

    add(index)
        -> locating position: O(n)
        -> relinking: O(1)

Therefore overall:

    O(n)

---

# 💾 30. Memory Usage

LinkedList generally requires more memory than ArrayList for the same number of elements.

Why?

Each node contains:

    element reference
    next reference
    previous reference

Conceptually:

    [prev | element | next]

Compare with ArrayList:

    [ref][ref][ref][ref]

ArrayList mainly needs an array of references, while LinkedList needs a separate node object for each element plus links.

Therefore:

> LinkedList has higher per-element memory overhead.

---

# 🆚 31. ArrayList vs LinkedList

| Feature | ArrayList | LinkedList |
|---|---|---|
| Internal structure | Dynamic array | Doubly linked nodes |
| Random access | Fast | Slow |
| `get(index)` | O(1) | O(n) |
| End insertion | O(1) amortized | O(1) |
| Beginning insertion | O(n) | O(1) |
| Middle insertion | O(n) | O(n) to locate position |
| Beginning removal | O(n) | O(1) |
| End removal | O(1) | O(1) |
| Memory overhead | Lower | Higher |
| Cache locality | Generally better | Generally worse |
| `RandomAccess` | Yes | No |
| Implements Deque | No | Yes |
| Typical general-purpose List | Common choice | Less common |

Important:

Do not conclude:

    LinkedList = always better for insertion

The position still needs to be located.

---

# 🆚 32. LinkedList vs Array

| Feature | Array | LinkedList |
|---|---|---|
| Size | Fixed | Dynamic |
| Index access | O(1) | O(n) |
| Insert beginning | O(n) | O(1) after position known |
| Remove beginning | O(n) | O(1) |
| Memory overhead | Lower | Higher |
| Random access | Excellent | Poor |
| Resizing | Manual | Dynamic nodes |

---

# 🚚 33. LinkedList as Queue

LinkedList can implement Queue behavior.

Example:

    Queue<String> queue =
        new LinkedList<>();

    queue.offer("A");
    queue.offer("B");
    queue.offer("C");

    System.out.println(
        queue.poll()
    );

Output:

    A

Remaining:

    [B, C]

FIFO:

    First In
       |
       v
    First Out

---

## Queue Method Comparison

| Operation | Exception Form | Special Value Form |
|---|---|---|
| Insert | `add()` | `offer()` |
| Remove | `remove()` | `poll()` |
| Examine | `element()` | `peek()` |

Important:

The special-value versions generally return:

    false

or:

    null

instead of throwing an exception for certain failure/empty cases.

---

# ↔️ 34. LinkedList as Deque

Deque:

    Double Ended Queue

Example:

    Deque<Integer> deque =
        new LinkedList<>();

    deque.offerFirst(20);
    deque.offerLast(30);
    deque.offerFirst(10);

    System.out.println(deque);

Output:

    [10, 20, 30]

Remove:

    deque.pollFirst();

    deque.pollLast();

---

## Deque Concept

    Front                         Rear
      |                            |
      v                            v

    [10] <-> [20] <-> [30]

      ^                            ^
      |                            |
    remove                        remove
    first                          last

This makes LinkedList suitable for operations at both ends.

---

# 📚 35. LinkedList as Stack

A stack follows:

    LIFO

Example:

    Deque<Integer> stack =
        new LinkedList<>();

    stack.push(10);
    stack.push(20);
    stack.push(30);

    System.out.println(
        stack.pop()
    );

Output:

    30

Stack:

    push(10)
    push(20)
    push(30)

        |
        v

    [30]
    [20]
    [10]

    pop()
      |
      v
    30

For modern Java stack usage, prefer the `Deque` abstraction and an implementation such as `ArrayDeque` when appropriate.

---

# 🚨 36. Common Mistakes

## Mistake 1 — Thinking LinkedList Is Always Faster

Not true.

For frequent indexed access:

    ArrayList

is generally much better.

---

## Mistake 2 — Saying get(index) Is O(1)

Wrong.

For LinkedList:

    get(index)
        -> O(n)

because traversal is required.

---

## Mistake 3 — Saying All Insertions Are O(1)

Incomplete.

If the node is already known:

    insertion
        -> O(1)

But:

    add(index, element)

usually requires locating the position:

    O(n)

---

## Mistake 4 — Thinking LinkedList Uses an Array

Wrong.

Standard Java LinkedList uses linked nodes.

---

## Mistake 5 — Ignoring Memory Overhead

Each node requires additional references:

    prev
    next

Therefore LinkedList has higher memory overhead.

---

## Mistake 6 — Using LinkedList for Random Access

If your algorithm repeatedly does:

    list.get(i)

LinkedList can become inefficient.

---

## Mistake 7 — Using LinkedList as a Stack Without Knowing Deque

Modern Java code should generally use:

    Deque

as the abstraction for stack behavior.

---

## Mistake 8 — Confusing remove() Methods

For example:

    LinkedList<Integer> numbers =
        new LinkedList<>(
            List.of(10, 20, 30)
        );

    numbers.remove(1);

This removes:

    index 1

not:

    value 1

---

# 🎯 37. Interview Traps

## Trap 1

Question:

> Is LinkedList internally doubly linked?

Answer:

Yes. The standard JDK LinkedList implementation uses doubly linked nodes.

---

## Trap 2

Question:

> Why is get(index) O(n)?

Because LinkedList must traverse nodes to reach the requested position.

---

## Trap 3

Question:

> Why is addFirst() O(1)?

Because LinkedList maintains a reference to the first node and can link the new node directly at the beginning.

---

## Trap 4

Question:

> Why is addLast() O(1)?

Because LinkedList maintains a reference to the last node.

---

## Trap 5

Question:

> Is LinkedList synchronized?

No.

It is not synchronized by default.

---

## Trap 6

Question:

> Does LinkedList allow null?

Yes.

---

## Trap 7

Question:

> Does LinkedList allow duplicates?

Yes.

---

## Trap 8

Question:

> Does LinkedList implement RandomAccess?

No.

---

## Trap 9

Question:

> Which interfaces does LinkedList implement?

Important ones:

    List
    Deque
    Cloneable
    Serializable

---

## Trap 10

Question:

> Can LinkedList be used as a Queue?

Yes.

---

## Trap 11

Question:

> Can LinkedList be used as a Deque?

Yes.

---

## Trap 12

Question:

> Can LinkedList be used as a Stack?

It can provide stack-like behavior through Deque methods such as:

    push()
    pop()
    peek()

But modern Java code generally prefers the Deque abstraction.

---

## Trap 13

Question:

> Is insertion into LinkedList always O(1)?

No.

Insertion is O(1) once the target node is known, but locating an arbitrary index can take O(n).

---

## Trap 14

Question:

> Why does LinkedList consume more memory?

Each node stores multiple references in addition to the element reference.

---

## Trap 15

Question:

> Why might ArrayList outperform LinkedList even for some insert/remove workloads?

ArrayList has better memory locality and lower per-element overhead, while LinkedList requires pointer traversal and separate node objects.

---

# 🧠 38. DSA Relevance

Linked lists are fundamental to DSA.

They teach:

- Node-based data structures
- References
- Pointer manipulation
- Traversal
- Insertion
- Deletion
- Doubly linked structures
- Queue implementation
- Deque implementation
- LRU cache concepts
- Memory relationships

Important:

> Java's `LinkedList` is useful for learning and certain API-level use cases, but DSA interviews often expect you to implement a linked list yourself rather than simply use `java.util.LinkedList`.

---

# 💡 39. DSA Examples

## Example 1 — Reverse Traversal

    LinkedList<Integer> numbers =
        new LinkedList<>(
            List.of(10, 20, 30, 40)
        );

    Iterator<Integer> iterator =
        numbers.descendingIterator();

    while (iterator.hasNext()) {

        System.out.println(
            iterator.next()
        );
    }

Output:

    40
    30
    20
    10

---

## Example 2 — Queue

    Queue<Integer> queue =
        new LinkedList<>();

    queue.offer(10);
    queue.offer(20);
    queue.offer(30);

    while (!queue.isEmpty()) {

        System.out.println(
            queue.poll()
        );
    }

Output:

    10
    20
    30

---

## Example 3 — Deque

    Deque<Integer> deque =
        new LinkedList<>();

    deque.addFirst(20);
    deque.addLast(30);
    deque.addFirst(10);

    System.out.println(deque);

Output:

    [10, 20, 30]

---

## Example 4 — Stack Behavior

    Deque<Integer> stack =
        new LinkedList<>();

    stack.push(10);
    stack.push(20);
    stack.push(30);

    while (!stack.isEmpty()) {

        System.out.println(
            stack.pop()
        );
    }

Output:

    30
    20
    10

---

## Example 5 — Remove First and Last

    LinkedList<Integer> numbers =
        new LinkedList<>(
            List.of(10, 20, 30, 40)
        );

    numbers.removeFirst();
    numbers.removeLast();

    System.out.println(numbers);

Output:

    [20, 30]

---

## Example 6 — Add at Both Ends

    LinkedList<String> names =
        new LinkedList<>();

    names.addFirst("Spring");
    names.addLast("React");
    names.addFirst("Java");

    System.out.println(names);

Output:

    [Java, Spring, React]

---

# 💼 40. Top Interview Questions

## Q1. What is LinkedList?

`LinkedList` is a doubly linked implementation of the `List` and `Deque` interfaces.

---

## Q2. How does LinkedList work internally?

It stores elements in linked nodes. Each node conceptually contains an element and references to the previous and next nodes.

---

## Q3. Is LinkedList doubly linked?

Yes.

---

## Q4. What is the time complexity of get(index)?

Typically:

    O(n)

---

## Q5. Why is get(index) O(n)?

Because LinkedList must traverse nodes to reach the requested index.

---

## Q6. What is the time complexity of addFirst()?

Typically:

    O(1)

---

## Q7. What is the time complexity of addLast()?

Typically:

    O(1)

---

## Q8. What is the complexity of removeFirst()?

Typically:

    O(1)

---

## Q9. What is the complexity of removeLast()?

Typically:

    O(1)

---

## Q10. Is insertion in LinkedList O(1)?

Only when the insertion position/node is already known.

Finding an arbitrary position can take:

    O(n)

---

## Q11. Why does LinkedList use more memory?

Every node requires references for:

    previous
    next

in addition to the element reference.

---

## Q12. Does LinkedList implement RandomAccess?

No.

---

## Q13. Does LinkedList allow duplicates?

Yes.

---

## Q14. Does LinkedList allow null?

Yes.

---

## Q15. Is LinkedList synchronized?

No.

---

## Q16. Which interfaces does LinkedList implement?

Important interfaces:

    List
    Deque
    Cloneable
    Serializable

---

## Q17. Can LinkedList work as a Queue?

Yes.

---

## Q18. Can LinkedList work as a Deque?

Yes.

---

## Q19. Can LinkedList work as a Stack?

Yes, through Deque methods.

---

## Q20. Why is ArrayList generally better for random access?

ArrayList uses an array, so indexed access can directly locate the required element.

LinkedList must traverse nodes.

---

## Q21. Why can LinkedList insertion be O(1)?

Once the target node is already known, only a small number of references need to be changed.

---

## Q22. What is the difference between ArrayList and LinkedList?

ArrayList:

    Dynamic array
    O(1) indexed access
    Lower memory overhead

LinkedList:

    Doubly linked nodes
    O(n) indexed access
    Efficient end operations
    Higher memory overhead
    Implements Deque

---

## Q23. What is Deque?

Deque stands for:

    Double Ended Queue

It supports insertion and removal from both ends.

---

## Q24. What is the difference between poll() and remove()?

Both remove the head.

But:

    remove()
        -> throws exception if empty

    poll()
        -> returns null if empty

---

## Q25. What is the difference between peek() and element()?

Both examine the head without removing it.

But:

    element()
        -> throws exception if empty

    peek()
        -> returns null if empty

---

## Q26. What is descendingIterator()?

It returns an iterator that traverses the LinkedList from the tail toward the head.

---

## Q27. Is LinkedList suitable for frequent get(index)?

Generally no.

If frequent indexed access is required, ArrayList is usually more appropriate.

---

## Q28. Why doesn't LinkedList implement RandomAccess?

Because indexed access requires traversal rather than direct array indexing.

---

## Q29. What is a node?

A node is an object that stores:

    element
    link to previous node
    link to next node

in a doubly linked list.

---

## Q30. Why does LinkedList have head and tail?

Maintaining references to both ends makes operations such as:

    addFirst()
    addLast()
    removeFirst()
    removeLast()

efficient.

---

# ⏱️ 41. 30-Second Interview Answer

If the interviewer asks:

> "What is LinkedList and how does it work internally?"

Answer:

> "`LinkedList` is a doubly linked implementation of the `List` and `Deque` interfaces. Internally, it stores elements in nodes where each node contains the element, a reference to the previous node, and a reference to the next node. It provides O(1) operations at the ends, such as adding or removing the first or last element. However, indexed access such as `get(index)` is typically O(n) because the list has to traverse its nodes. LinkedList also uses more memory than ArrayList because every node contains additional link references and has separate node-object overhead."

---

# 📌 42. Cheat Sheet

## Basic

    LinkedList
        -> java.util
        -> Doubly linked
        -> Implements List
        -> Implements Deque
        -> Ordered
        -> Allows duplicates
        -> Allows null
        -> Not synchronized
        -> No RandomAccess

---

## Node

    Node
      |
      +--- prev
      |
      +--- item
      |
      +--- next

---

## List Methods

    add()
    add(index, element)

    addAll()

    get()
    set()

    remove()
    remove(index)
    remove(Object)

    contains()

    indexOf()
    lastIndexOf()

    size()
    isEmpty()

    clear()

    subList()

---

## First / Last Methods

    addFirst()
    addLast()

    getFirst()
    getLast()

    peekFirst()
    peekLast()

    removeFirst()
    removeLast()

    pollFirst()
    pollLast()

    offerFirst()
    offerLast()

---

## Queue Methods

    offer()
    poll()
    peek()

---

## Stack-Like Methods

    push()
    pop()
    peek()

---

## Iterator Methods

    iterator()
    listIterator()
    descendingIterator()

---

## Complexity

    get(index)
        -> O(n)

    set(index)
        -> O(n)

    addFirst()
        -> O(1)

    addLast()
        -> O(1)

    removeFirst()
        -> O(1)

    removeLast()
        -> O(1)

    contains()
        -> O(n)

    size()
        -> O(1)

---

# ⚡ 43. Quick Revision

Remember:

    LinkedList = Doubly Linked Nodes

Mental model:

    head
      |
      v

    [A] <-> [B] <-> [C] <-> [D]
                              ^
                              |
                             tail

Each node:

    [prev | data | next]

---

## Most Important Concepts

    1. LinkedList implements List and Deque.

    2. Standard Java LinkedList uses doubly linked nodes.

    3. Each node has previous and next links.

    4. get(index) is typically O(n).

    5. addFirst() is O(1).

    6. addLast() is O(1).

    7. removeFirst() is O(1).

    8. removeLast() is O(1).

    9. Arbitrary indexed insertion/removal is typically O(n)
       because the position must first be located.

    10. Once a node is known, relinking can be O(1).

    11. LinkedList uses more memory than ArrayList.

    12. LinkedList does not implement RandomAccess.

    13. LinkedList can work as a List.

    14. LinkedList can work as a Queue.

    15. LinkedList can work as a Deque.

    16. LinkedList can provide stack-like operations.

    17. LinkedList allows duplicates.

    18. LinkedList allows null.

    19. LinkedList is not synchronized by default.

    20. ArrayList is generally preferred when frequent indexed access is required.

---

# 🎯 Interview Must-Know

Before moving to `07-Vector.md`, make sure you can explain:

    1. What is LinkedList?
    2. Why is it called doubly linked?
    3. What does a node contain?
    4. What are head and tail?
    5. How does get(index) work internally?
    6. Why is get(index) O(n)?
    7. Why is addFirst() O(1)?
    8. Why is addLast() O(1)?
    9. Is insertion always O(1)?
    10. Why does LinkedList use more memory?
    11. Why doesn't LinkedList implement RandomAccess?
    12. What is Deque?
    13. How can LinkedList work as a Queue?
    14. How can LinkedList work as a Stack?
    15. What is descendingIterator()?
    16. LinkedList vs ArrayList?
    17. When would you choose ArrayList?
    18. When would you choose LinkedList?
    19. What is the difference between poll() and remove()?
    20. What is the difference between peek() and element()?

---

# 🚀 Final Takeaway

The most important mental model is:

    List
      |
      v
    LinkedList
      |
      v
    Doubly Linked Nodes

    [prev | data | next]
          |
          v
    [prev | data | next]
          |
          v
    [prev | data | next]

The key interview line to remember:

> **LinkedList is a doubly linked implementation of List and Deque. It provides efficient operations at both ends, but indexed access is typically O(n) because elements must be reached through node traversal.**