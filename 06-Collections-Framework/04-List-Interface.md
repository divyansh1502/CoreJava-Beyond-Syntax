# 04 — List Interface 📋

> **Package:** `java.util` 📦  
> **Parent Interface:** `Collection<E>`  
> **Type:** Generic Interface 🧬  
> **Purpose:** Represents an ordered collection that allows duplicate elements.

---

# 📚 Table of Contents

1. [Introduction](#1-introduction) 🚀
2. [Why List Interface?](#2-why-list-interface) 🤔
3. [List Hierarchy](#3-list-hierarchy) 🌳
4. [What is List?](#4-what-is-list) 🧠
5. [Important Characteristics](#5-important-characteristics) 🔑
6. [List vs Collection](#6-list-vs-collection) ⚡
7. [List Implementations](#7-list-implementations) 🏗️
8. [Creating a List](#8-creating-a-list) 💻
9. [Adding Elements](#9-adding-elements) ➕
10. [Accessing Elements](#10-accessing-elements) 👀
11. [Updating Elements](#11-updating-elements) 🔄
12. [Removing Elements](#12-removing-elements) ❌
13. [Searching in List](#13-searching-in-list) 🔍
14. [Index-Based Operations](#14-index-based-operations) 📍
15. [List-Specific Methods](#15-list-specific-methods) 🛠️
16. [SubList](#16-sublist) ✂️
17. [List Iteration](#17-list-iteration) 🔄
18. [ListIterator](#18-listiterator) 🔁
19. [Sorting a List](#19-sorting-a-list) 📊
20. [Reversing a List](#20-reversing-a-list) 🔃
21. [Immutable Lists](#21-immutable-lists) 🔒
22. [Unmodifiable Lists](#22-unmodifiable-lists) 🛡️
23. [Null and Duplicate Elements](#23-null-and-duplicate-elements) ⚠️
24. [Internal Working](#24-internal-working) ⚙️
25. [Time Complexity](#25-time-complexity) ⏱️
26. [List vs Set](#26-list-vs-set) ⚔️
27. [ArrayList vs LinkedList](#27-arraylist-vs-linkedlist) 🆚
28. [Common Mistakes](#28-common-mistakes) 🚨
29. [Interview Traps](#29-interview-traps) 🎯
30. [DSA Relevance](#30-dsa-relevance) 🧠
31. [DSA Examples](#31-dsa-examples) 💡
32. [Top Interview Questions](#32-top-interview-questions) 💼
33. [30-Second Interview Answer](#33-30-second-interview-answer) ⏱️
34. [Cheat Sheet](#34-cheat-sheet) 📌
35. [Quick Revision](#35-quick-revision) ⚡

---

# 🚀 1. Introduction

`List` is an interface in the Java Collection Framework.

It represents an **ordered collection of elements**.

Package:

    java.util.List

Hierarchy:

    Iterable<E>
         |
         v
    Collection<E>
         |
         v
    List<E>

The most important characteristics of a `List` are:

- 📋 Maintains an ordered sequence
- 🔢 Supports index-based access
- 🔁 Allows duplicate elements
- 🧬 Supports generics
- ➕ Allows insertion of elements
- ❌ Allows removal of elements
- 🔄 Allows updating elements
- 🔍 Supports searching
- 📍 Supports positional/index-based operations

Example:

    import java.util.*;

    public class Main {

        public static void main(String[] args) {

            List<String> languages = new ArrayList<>();

            languages.add("Java");
            languages.add("Python");
            languages.add("Java");

            System.out.println(languages);
        }
    }

Output:

    [Java, Python, Java]

Notice that `"Java"` appears twice.

This is allowed because:

> A `List` generally permits duplicate elements.

---

# 🤔 2. Why List Interface?

Suppose we want to store:

    Java
    Python
    JavaScript
    Java

An array can store these values:

    String[] languages = {
        "Java",
        "Python",
        "JavaScript",
        "Java"
    };

But arrays have a fixed size.

A `List` provides a dynamic collection abstraction.

Example:

    List<String> languages = new ArrayList<>();

    languages.add("Java");
    languages.add("Python");
    languages.add("JavaScript");
    languages.add("Java");

The list can grow and shrink dynamically.

---

# 🌳 3. List Hierarchy

The basic hierarchy is:

    Iterable<E>
         |
         v
    Collection<E>
         |
         v
    List<E>
         |
         +----------------+
         |                |
     ArrayList        LinkedList
                          |
                          +--- Queue
                          |
                          +--- Deque

Other List implementations include:

    Vector
      |
      +--- Stack

So an important simplified hierarchy is:

    Iterable
       |
    Collection
       |
      List
       |
       +--- ArrayList
       |
       +--- LinkedList
       |
       +--- Vector
              |
              +--- Stack

⚠️ `LinkedList` implements both:

    List
    Deque

⚠️ `Vector` is a legacy synchronized List implementation.

⚠️ `Stack` extends `Vector` and is also a legacy class.

---

# 🧠 4. What is List?

A `List` is an ordered collection in which every element has a position represented by an index.

Indexes start from:

    0

Example:

    List<String> names = new ArrayList<>();

    names.add("A");
    names.add("B");
    names.add("C");

Conceptually:

    Index:     0       1       2
              +-------+-------+-------+
    Value:    |   A   |   B   |   C   |
              +-------+-------+-------+

Therefore:

    names.get(0) -> A
    names.get(1) -> B
    names.get(2) -> C

---

# 🔑 5. Important Characteristics

## 5.1 Ordered 📋

A List maintains a defined sequence.

Example:

    List<Integer> numbers = new ArrayList<>();

    numbers.add(30);
    numbers.add(10);
    numbers.add(20);

The List contains:

    [30, 10, 20]

The insertion sequence is preserved by implementations such as `ArrayList`.

---

## 5.2 Allows Duplicates 🔁

Example:

    List<String> names = new ArrayList<>();

    names.add("Java");
    names.add("Java");
    names.add("Java");

Result:

    [Java, Java, Java]

---

## 5.3 Index-Based Access 📍

Example:

    List<String> names = new ArrayList<>();

    names.add("Java");
    names.add("Spring");
    names.add("React");

    System.out.println(names.get(1));

Output:

    Spring

---

## 5.4 Supports Null

Many List implementations allow null values.

Example:

    List<String> names = new ArrayList<>();

    names.add("Java");
    names.add(null);
    names.add("Spring");

    System.out.println(names);

Output:

    [Java, null, Spring]

⚠️ Null support depends on the concrete implementation.

---

# ⚡ 6. List vs Collection

`List` extends `Collection`.

Therefore, List inherits Collection methods.

Example:

    List<String> names = new ArrayList<>();

Collection methods:

    add()
    remove()
    contains()
    size()
    isEmpty()
    clear()
    addAll()
    removeAll()
    retainAll()

List adds additional capabilities such as:

    get()
    set()
    add(index, element)
    remove(index)
    indexOf()
    lastIndexOf()
    subList()
    listIterator()

Conceptually:

    Collection
         |
         +--- Common collection operations
         |
         v
       List
         |
         +--- Index-based operations
         +--- Positional insertion
         +--- Element replacement
         +--- List-specific operations

---

# 🏗️ 7. List Implementations

Important implementations:

    ArrayList
    LinkedList
    Vector
    Stack

---

## ArrayList

Uses a dynamically resizable array internally.

Best known for:

    Fast random access

Example:

    List<Integer> numbers = new ArrayList<>();

---

## LinkedList

Uses a linked-node structure.

It implements:

    List
    Deque

Example:

    List<Integer> numbers = new LinkedList<>();

---

## Vector

A legacy synchronized dynamic array implementation.

Example:

    List<Integer> numbers = new Vector<>();

Modern applications generally prefer `ArrayList` when synchronization is not specifically required.

---

## Stack

A legacy class that extends `Vector`.

Example:

    Stack<Integer> stack = new Stack<>();

Modern stack-style code commonly uses:

    Deque<Integer> stack = new ArrayDeque<>();

---

# 💻 8. Creating a List

## Using ArrayList

    List<String> names = new ArrayList<>();

---

## Using LinkedList

    List<String> names = new LinkedList<>();

---

## Using Vector

    List<String> names = new Vector<>();

---

## Using Arrays.asList()

    List<String> names =
        Arrays.asList("Java", "Spring", "React");

⚠️ The returned list has a fixed size.

You can use:

    names.set(0, "Python");

But:

    names.add("C++");

can throw:

    UnsupportedOperationException

---

## Using List.of()

Modern Java provides:

    List<String> names =
        List.of("Java", "Spring", "React");

`List.of()` creates an unmodifiable List.

This means:

    names.add("Python");

throws:

    UnsupportedOperationException

It also does not permit null elements.

---

## Creating Mutable List from List.of()

If a mutable list is required:

    List<String> names =
        new ArrayList<>(List.of("Java", "Spring", "React"));

Now:

    names.add("Python");

is allowed.

---

# ➕ 9. Adding Elements

## 9.1 add(element)

Adds an element at the end.

    List<String> names = new ArrayList<>();

    names.add("Java");
    names.add("Spring");

    System.out.println(names);

Output:

    [Java, Spring]

---

## 9.2 add(index, element)

Adds an element at a specific index.

    List<String> names = new ArrayList<>();

    names.add("Java");
    names.add("Spring");

    names.add(1, "Python");

    System.out.println(names);

Output:

    [Java, Python, Spring]

The existing elements from that position are shifted to the right.

---

## Index Validation

If the index is invalid:

    names.add(10, "React");

an:

    IndexOutOfBoundsException

can occur.

---

## 9.3 addAll()

Adds all elements from another collection.

    List<String> first =
        new ArrayList<>(List.of("Java", "Spring"));

    List<String> second =
        new ArrayList<>(List.of("React", "Node"));

    first.addAll(second);

    System.out.println(first);

Output:

    [Java, Spring, React, Node]

---

## 9.4 addAll(index, collection)

Adds a collection at a particular position.

    List<String> first =
        new ArrayList<>(List.of("Java", "React"));

    List<String> second =
        new ArrayList<>(List.of("Spring", "Hibernate"));

    first.addAll(1, second);

    System.out.println(first);

Output:

    [Java, Spring, Hibernate, React]

---

# 👀 10. Accessing Elements

## 10.1 get(index)

Returns the element at a specific index.

    List<String> languages =
        new ArrayList<>(List.of("Java", "Python", "C++"));

    String language = languages.get(1);

    System.out.println(language);

Output:

    Python

---

## Index Formula

For a List containing `n` elements:

    Valid indexes:

    0 to n - 1

Example:

    List size = 5

    Valid indexes:

    0
    1
    2
    3
    4

Invalid:

    5

---

# 🔄 11. Updating Elements

## set(index, element)

Replaces the element at a given index.

Example:

    List<String> languages =
        new ArrayList<>(List.of("Java", "Python", "C++"));

    languages.set(1, "JavaScript");

    System.out.println(languages);

Output:

    [Java, JavaScript, C++]

Important:

    set()
        -> replaces an existing element

    add()
        -> inserts a new element

---

## add() vs set()

Suppose:

    [A, B, C]

Using:

    list.add(1, "X");

Result:

    [A, X, B, C]

Using:

    list.set(1, "X");

Result:

    [A, X, C]

Memory trick:

    add  -> INSERT ➕

    set  -> REPLACE 🔄

---

# ❌ 12. Removing Elements

List has overloaded remove methods.

Important:

    remove(int index)

and inherited:

    remove(Object object)

---

## 12.1 remove(index)

    List<String> names =
        new ArrayList<>(List.of("Java", "Spring", "React"));

    names.remove(1);

    System.out.println(names);

Output:

    [Java, React]

---

## 12.2 remove(Object)

    List<String> names =
        new ArrayList<>(List.of("Java", "Spring", "React"));

    names.remove("Spring");

    System.out.println(names);

Output:

    [Java, React]

---

## ⚠️ Integer Removal Trap

This is extremely important.

Consider:

    List<Integer> numbers =
        new ArrayList<>(List.of(10, 20, 30));

Now:

    numbers.remove(1);

removes the element at index `1`.

Result:

    [10, 30]

It does NOT remove the value `1`.

To remove an Integer value:

    numbers.remove(Integer.valueOf(20));

Result:

    [10, 30]

Memory:

    remove(int)
        -> index

    remove(Object)
        -> object/value

---

# 🔍 13. Searching in List

## 13.1 contains()

Checks whether an element exists.

    List<String> names =
        new ArrayList<>(List.of("Java", "Spring", "React"));

    System.out.println(names.contains("Java"));

Output:

    true

---

## 13.2 indexOf()

Returns the first index of an element.

    List<String> names =
        new ArrayList<>(List.of("Java", "Spring", "Java"));

    System.out.println(names.indexOf("Java"));

Output:

    0

If the element does not exist:

    -1

---

## 13.3 lastIndexOf()

Returns the last index of an element.

    List<String> names =
        new ArrayList<>(List.of("Java", "Spring", "Java"));

    System.out.println(names.lastIndexOf("Java"));

Output:

    2

If not found:

    -1

---

## indexOf vs lastIndexOf

    [Java, Spring, Java, React, Java]

    indexOf("Java")
        -> 0

    lastIndexOf("Java")
        -> 4

---

# 📍 14. Index-Based Operations

The List interface is different from Collection because it provides positional operations.

Important methods:

    get(index)
    set(index, element)
    add(index, element)
    remove(index)

Example:

    List<String> languages =
        new ArrayList<>(List.of("Java", "Python", "C++"));

    System.out.println(languages.get(0));

    languages.set(0, "JavaScript");

    languages.add(1, "Spring");

    languages.remove(2);

    System.out.println(languages);

---

## Why Collection Doesn't Have get()

Because not every Collection is index-based.

For example:

    HashSet

does not conceptually provide:

    element at index 3

Therefore:

    get(index)

belongs to:

    List

not:

    Collection

---

# 🛠️ 15. List-Specific Methods

Important methods introduced by List:

    get()
    set()
    add(index, element)
    addAll(index, collection)
    remove(index)
    indexOf()
    lastIndexOf()
    listIterator()
    subList()
    replaceAll()
    sort()

---

## replaceAll()

Applies an operation to every element.

Example:

    List<String> names =
        new ArrayList<>(List.of("java", "spring", "react"));

    names.replaceAll(String::toUpperCase);

    System.out.println(names);

Output:

    [JAVA, SPRING, REACT]

Another example:

    List<Integer> numbers =
        new ArrayList<>(List.of(1, 2, 3, 4));

    numbers.replaceAll(n -> n * 2);

    System.out.println(numbers);

Output:

    [2, 4, 6, 8]

---

# ✂️ 16. SubList

`subList()` returns a view of a portion of the list.

Syntax:

    list.subList(fromIndex, toIndex)

Important:

    fromIndex -> inclusive

    toIndex -> exclusive

Example:

    List<String> names =
        new ArrayList<>(
            List.of("A", "B", "C", "D", "E")
        );

    List<String> sub =
        names.subList(1, 4);

    System.out.println(sub);

Output:

    [B, C, D]

Indexes:

    1 -> B
    2 -> C
    3 -> D

Index `4` is excluded.

---

## ⚠️ Important: subList Is a View

The returned sublist is generally backed by the original list.

Example:

    List<String> names =
        new ArrayList<>(
            List.of("A", "B", "C", "D")
        );

    List<String> sub =
        names.subList(1, 3);

    sub.set(0, "X");

    System.out.println(names);

Output:

    [A, X, C, D]

Changing the sublist affected the original list.

Therefore:

> `subList()` should be treated as a view, not automatically as an independent copy.

---

# 🔄 17. List Iteration

A List can be traversed using:

    1. for loop
    2. enhanced for-loop
    3. Iterator
    4. ListIterator
    5. forEach
    6. Stream

---

## Normal for Loop

    List<String> names =
        new ArrayList<>(List.of("Java", "Spring", "React"));

    for (int i = 0; i < names.size(); i++) {
        System.out.println(names.get(i));
    }

---

## Enhanced for Loop

    for (String name : names) {
        System.out.println(name);
    }

---

## forEach

    names.forEach(System.out::println);

---

## Iterator

    Iterator<String> iterator =
        names.iterator();

    while (iterator.hasNext()) {
        System.out.println(iterator.next());
    }

---

# 🔁 18. ListIterator

`ListIterator` is a specialized iterator designed specifically for Lists.

It supports:

    Forward traversal
    Backward traversal
    add()
    set()
    remove()
    nextIndex()
    previousIndex()

Example:

    List<String> names =
        new ArrayList<>(List.of("Java", "Spring", "React"));

    ListIterator<String> iterator =
        names.listIterator();

    while (iterator.hasNext()) {
        System.out.println(iterator.next());
    }

Output:

    Java
    Spring
    React

---

## Backward Traversal

    ListIterator<String> iterator =
        names.listIterator(names.size());

    while (iterator.hasPrevious()) {
        System.out.println(iterator.previous());
    }

Output:

    React
    Spring
    Java

---

## Iterator vs ListIterator

    Iterator
        |
        +--- Forward traversal
        +--- remove()
        +--- Works with Collection

    ListIterator
        |
        +--- Forward traversal
        +--- Backward traversal
        +--- add()
        +--- set()
        +--- remove()
        +--- Index information
        +--- Works only with List

---

# 📊 19. Sorting a List

The List interface provides:

    sort(Comparator<? super E> comparator)

Example:

    List<Integer> numbers =
        new ArrayList<>(List.of(40, 10, 30, 20));

    numbers.sort(null);

    System.out.println(numbers);

Output:

    [10, 20, 30, 40]

---

## Descending Order

    List<Integer> numbers =
        new ArrayList<>(List.of(40, 10, 30, 20));

    numbers.sort(Comparator.reverseOrder());

    System.out.println(numbers);

Output:

    [40, 30, 20, 10]

---

## Custom Sorting

    List<String> names =
        new ArrayList<>(List.of("Java", "Spring", "React"));

    names.sort(
        (a, b) -> Integer.compare(a.length(), b.length())
    );

    System.out.println(names);

The comparator determines the sorting rule.

---

# 🔃 20. Reversing a List

Use:

    Collections.reverse(list)

Example:

    List<Integer> numbers =
        new ArrayList<>(List.of(10, 20, 30, 40));

    Collections.reverse(numbers);

    System.out.println(numbers);

Output:

    [40, 30, 20, 10]

---

# 🔒 21. Immutable Lists

Java provides factory methods such as:

    List.of()

Example:

    List<String> names =
        List.of("Java", "Spring", "React");

Attempting modification:

    names.add("Python");

throws:

    UnsupportedOperationException

Similarly:

    names.set(0, "C++");

also throws:

    UnsupportedOperationException

And:

    names.remove("Java");

also throws:

    UnsupportedOperationException

---

## Null Restriction

`List.of()` does not allow null elements.

Example:

    List<String> names =
        List.of("Java", null, "Spring");

This throws:

    NullPointerException

---

# 🛡️ 22. Unmodifiable Lists

`Collections.unmodifiableList()` creates an unmodifiable view.

Example:

    List<String> original =
        new ArrayList<>();

    original.add("Java");
    original.add("Spring");

    List<String> view =
        Collections.unmodifiableList(original);

    view.add("React");

This throws:

    UnsupportedOperationException

Important difference:

    List.of()
        -> creates an unmodifiable List

    Collections.unmodifiableList()
        -> creates an unmodifiable view of another list

The underlying list may still be changed directly.

Example:

    original.add("React");

    System.out.println(view);

The view can reflect that change.

---

# ⚠️ 23. Null and Duplicate Elements

## Duplicates

Lists generally allow duplicates.

Example:

    List<Integer> numbers =
        new ArrayList<>();

    numbers.add(10);
    numbers.add(10);
    numbers.add(10);

Result:

    [10, 10, 10]

---

## Null

Many List implementations permit null.

Example:

    List<String> names =
        new ArrayList<>();

    names.add(null);

    System.out.println(names);

Output:

    [null]

But:

    List.of(null);

throws:

    NullPointerException

Therefore:

> Always consider the specific List implementation when discussing null behavior.

---

# ⚙️ 24. Internal Working

The `List` interface itself does not define how elements are physically stored.

The implementation decides that.

---

## ArrayList

Conceptually:

    List
      |
      v
    ArrayList
      |
      v
    Dynamic Array
      |
      +--- element 0
      +--- element 1
      +--- element 2
      +--- ...

Therefore:

    get(index)

is efficient.

---

## LinkedList

Conceptually:

    List
      |
      v
    LinkedList
      |
      v
    Node <-> Node <-> Node <-> Node

Each node contains references connecting it to other nodes.

This gives different performance characteristics from ArrayList.

---

## Vector

Conceptually similar to a dynamically resizable array, but its legacy methods are synchronized.

---

## Stack

`Stack` extends `Vector`.

Conceptually:

    Stack
      |
      v
    Vector
      |
      v
    Dynamic Array

---

# ⏱️ 25. Time Complexity

Complexity depends on the implementation.

Typical `ArrayList` characteristics:

    get(index)       -> O(1) average
    set(index)       -> O(1)
    add(end)         -> O(1) amortized
    add(index)       -> O(n)
    remove(index)    -> O(n)
    contains()       -> O(n)
    indexOf()        -> O(n)

Typical `LinkedList` characteristics:

    get(index)       -> O(n)
    set(index)       -> O(n)
    add(first)       -> O(1)
    add(last)        -> O(1)
    remove(first)    -> O(1)
    remove(last)     -> O(1)
    contains()       -> O(n)

⚠️ These are typical complexity characteristics, not universal guarantees for every implementation.

---

# ⚔️ 26. List vs Set

| Feature | List 📋 | Set 🎯 |
|---|---|---|
| Duplicate elements | Allowed | Not allowed |
| Index-based access | Yes | No |
| Ordering | Defined by List semantics | Implementation dependent |
| Main purpose | Ordered sequence | Unique elements |
| `get(index)` | Yes | No |
| Example | ArrayList | HashSet |
| Duplicate removal | No | Yes |

Example:

    List<Integer> list =
        new ArrayList<>(List.of(10, 10, 20));

    Set<Integer> set =
        new HashSet<>(List.of(10, 10, 20));

Results:

    list -> [10, 10, 20]

    set -> [10, 20]

---

# 🆚 27. ArrayList vs LinkedList

| Feature | ArrayList | LinkedList |
|---|---|---|
| Internal structure | Dynamic array | Linked nodes |
| Random access | Fast | Slow |
| `get(index)` | O(1) average | O(n) |
| Insert at middle | O(n) | O(n) to locate position, then local link changes |
| Remove at middle | O(n) | O(n) to locate position, then local link changes |
| Memory overhead | Lower | Higher |
| Implements List | Yes | Yes |
| Implements Deque | No | Yes |

Important:

> Do not memorize "LinkedList insertion is always O(1)."

If you already have a reference to the appropriate node/position, the link update can be constant time. But finding an arbitrary index can take O(n).

---

# 🚨 28. Common Mistakes

## Mistake 1 — Thinking List doesn't allow duplicates

Wrong:

    List removes duplicate values.

Correct:

    List allows duplicate elements.

---

## Mistake 2 — Thinking index starts from 1

Wrong:

    First element -> index 1

Correct:

    First element -> index 0

---

## Mistake 3 — Confusing add() and set()

    add()
        -> inserts

    set()
        -> replaces

---

## Mistake 4 — Confusing remove(index) and remove(value)

For:

    List<Integer> numbers =
        new ArrayList<>(List.of(10, 20, 30));

    numbers.remove(1);

removes index `1`.

To remove value `1`:

    numbers.remove(Integer.valueOf(1));

---

## Mistake 5 — Thinking subList() creates a copy

It generally returns a view backed by the original List.

---

## Mistake 6 — Thinking List always allows null

Different implementations have different rules.

`List.of()` does not permit null.

---

## Mistake 7 — Thinking LinkedList is always faster than ArrayList

Not true.

Performance depends on the operation and access pattern.

---

# 🎯 29. Interview Traps

## Trap 1

Question:

> Why does List extend Collection?

Answer:

Because List needs all common Collection operations while adding List-specific behavior such as ordering and index-based operations.

---

## Trap 2

Question:

> Does List allow duplicates?

Answer:

Yes, List permits duplicate elements.

---

## Trap 3

Question:

> Does List guarantee insertion order?

Answer:

The List abstraction represents an ordered sequence, and List implementations maintain the sequence defined by their operations.

---

## Trap 4

Question:

> Why doesn't Set have get(index)?

Because Set is not defined as an index-based collection.

---

## Trap 5

Question:

> What is the difference between add() and set()?

    add(index, value)
        -> inserts
        -> shifts existing elements

    set(index, value)
        -> replaces
        -> does not increase size

---

## Trap 6

Question:

> What does remove(1) mean for List<Integer>?

It means:

    remove element at index 1

because `List` has:

    remove(int index)

Use:

    remove(Integer.valueOf(1))

to remove the value `1`.

---

## Trap 7

Question:

> What does subList() return?

It returns a view of a portion of the List rather than automatically creating an independent copy.

---

## Trap 8

Question:

> Can we instantiate List?

No.

`List` is an interface.

Correct:

    List<Integer> numbers =
        new ArrayList<>();

---

## Trap 9

Question:

> Which is faster, ArrayList or LinkedList?

There is no universal answer.

It depends on the operation and workload.

---

# 🧠 30. DSA Relevance

List is one of the most frequently used structures in DSA.

It is useful for:

- Arrays and dynamic arrays
- Sequences
- Two-pointer problems
- Sliding window
- Prefix sums
- Sorting
- Searching
- Graph adjacency lists
- Storing intermediate results
- Implementing other data structures

---

## Graph Adjacency List

A graph can be represented using Lists.

Example:

    List<List<Integer>> graph =
        new ArrayList<>();

    int vertices = 4;

    for (int i = 0; i < vertices; i++) {
        graph.add(new ArrayList<>());
    }

    graph.get(0).add(1);
    graph.get(0).add(2);

    graph.get(1).add(3);

Conceptually:

    0 -> 1, 2
    1 -> 3
    2 ->
    3 ->

This is one of the most important DSA applications of List.

---

# 💡 31. DSA Examples

## Example 1 — Reverse a List

    List<Integer> numbers =
        new ArrayList<>(List.of(1, 2, 3, 4, 5));

    Collections.reverse(numbers);

    System.out.println(numbers);

Output:

    [5, 4, 3, 2, 1]

---

## Example 2 — Find Maximum

    List<Integer> numbers =
        new ArrayList<>(List.of(10, 50, 20, 40, 30));

    int max = Collections.max(numbers);

    System.out.println(max);

Output:

    50

---

## Example 3 — Find Minimum

    List<Integer> numbers =
        new ArrayList<>(List.of(10, 50, 20, 40, 30));

    int min = Collections.min(numbers);

    System.out.println(min);

Output:

    10

---

## Example 4 — Sort

    List<Integer> numbers =
        new ArrayList<>(List.of(40, 10, 30, 20));

    Collections.sort(numbers);

    System.out.println(numbers);

Output:

    [10, 20, 30, 40]

---

## Example 5 — Binary Search

The List must be sorted before using binary search.

    List<Integer> numbers =
        new ArrayList<>(List.of(10, 20, 30, 40, 50));

    int index =
        Collections.binarySearch(numbers, 30);

    System.out.println(index);

Output:

    2

---

## Example 6 — Remove Duplicates

    List<Integer> numbers =
        new ArrayList<>(List.of(1, 2, 2, 3, 3, 4));

    List<Integer> unique =
        new ArrayList<>(new LinkedHashSet<>(numbers));

    System.out.println(unique);

Output:

    [1, 2, 3, 4]

---

## Example 7 — Two-Pointer Style Access

    List<Integer> numbers =
        new ArrayList<>(List.of(10, 20, 30, 40, 50));

    int left = 0;
    int right = numbers.size() - 1;

    while (left < right) {

        System.out.println(
            numbers.get(left) + " " +
            numbers.get(right)
        );

        left++;
        right--;
    }

Output:

    10 50
    20 40

---

# 💼 32. Top Interview Questions

## Q1. What is List in Java?

`List` is an interface in `java.util` that extends `Collection` and represents an ordered sequence of elements with index-based operations.

---

## Q2. Does List allow duplicates?

Yes.

Example:

    List<Integer> list =
        new ArrayList<>(List.of(10, 10, 20));

---

## Q3. Does List allow null?

Many implementations allow null, but the behavior depends on the implementation.

For example:

    ArrayList -> allows null

    List.of() -> does not allow null

---

## Q4. What is the difference between List and Set?

List:

    Ordered
    Allows duplicates
    Supports indexes

Set:

    Unique elements
    No index-based access
    Ordering depends on implementation

---

## Q5. Why does List have get() but Collection does not?

Because List is index-based while Collection is a more general abstraction that also includes non-indexed structures such as Set and Queue.

---

## Q6. Difference between add() and set()?

    add()
        -> inserts a new element

    set()
        -> replaces an existing element

---

## Q7. What is the difference between remove(int) and remove(Object)?

    remove(int)
        -> removes element at index

    remove(Object)
        -> removes matching object

This creates the famous Integer removal trap.

---

## Q8. What does subList() return?

A view of a range of the original List.

---

## Q9. What are common List implementations?

    ArrayList
    LinkedList
    Vector
    Stack

---

## Q10. Which List implementation is generally preferred for random access?

`ArrayList`, because it is backed by a dynamically resizable array and provides efficient indexed access.

---

## Q11. Why is LinkedList slower for get(index)?

It may need to traverse nodes until reaching the requested position.

---

## Q12. Is LinkedList always better for insertion?

No.

The operation's position, how that position is reached, and the workload matter.

---

## Q13. What is ListIterator?

A specialized iterator for Lists that supports forward and backward traversal and modification operations such as `add()`, `set()`, and `remove()`.

---

## Q14. Difference between Iterator and ListIterator?

    Iterator
        -> Forward traversal
        -> remove()
        -> General collections

    ListIterator
        -> Forward + backward traversal
        -> add()
        -> set()
        -> remove()
        -> List only

---

## Q15. What is List.of()?

It creates an unmodifiable List.

It does not permit:

    add()
    remove()
    set()

and does not permit null elements.

---

## Q16. What is Arrays.asList()?

It creates a fixed-size List backed by an array.

Element replacement is supported, but structural changes such as adding or removing elements are not supported.

---

## Q17. Can List be instantiated?

No.

It is an interface.

Use:

    List<String> list =
        new ArrayList<>();

---

## Q18. What is the difference between List.of() and Arrays.asList()?

    List.of()
        -> unmodifiable
        -> no null elements

    Arrays.asList()
        -> fixed-size
        -> supports set()
        -> backed by an array
        -> permits null according to array behavior

---

## Q19. What is the difference between ArrayList and LinkedList?

ArrayList:

    Dynamic array
    Fast random access

LinkedList:

    Linked nodes
    Efficient endpoint operations
    Slower random access

---

## Q20. Why should we prefer List reference?

Example:

    List<String> names =
        new ArrayList<>();

It reduces coupling to the implementation and makes it easier to change implementations when needed.

---

# ⏱️ 33. 30-Second Interview Answer

If the interviewer asks:

> "What is List in Java?"

Answer:

> "`List` is a generic interface in the `java.util` package that extends `Collection`. It represents an ordered sequence of elements, supports index-based operations, and generally allows duplicate elements. Common implementations include `ArrayList`, `LinkedList`, `Vector`, and `Stack`. The List interface provides operations such as `get()`, `set()`, indexed `add()`, indexed `remove()`, `indexOf()`, `subList()`, and `listIterator()`. The actual performance and internal behavior depend on the concrete implementation."

---

# 📌 34. Cheat Sheet

## Hierarchy

    Iterable
        |
    Collection
        |
    List
        |
        +--- ArrayList
        |
        +--- LinkedList
        |
        +--- Vector
              |
              +--- Stack

---

## Core Properties

    List
      |
      +--- Ordered 📋
      |
      +--- Duplicates allowed 🔁
      |
      +--- Index-based 📍
      |
      +--- Generic 🧬
      |
      +--- Dynamic size in common implementations 📈

---

## Main Methods

    add(E)
    add(index, E)

    addAll(Collection)
    addAll(index, Collection)

    get(index)

    set(index, E)

    remove(index)
    remove(Object)

    contains(Object)

    indexOf(Object)
    lastIndexOf(Object)

    subList(from, to)

    listIterator()

    replaceAll()

    sort()

---

## Useful Utility Operations

    Collections.sort(list)

    Collections.reverse(list)

    Collections.max(list)

    Collections.min(list)

    Collections.binarySearch(list, key)

---

## Creation

    List<String> a =
        new ArrayList<>();

    List<String> b =
        new LinkedList<>();

    List<String> c =
        new ArrayList<>(
            List.of("Java", "Spring")
        );

---

# ⚡ 35. Quick Revision

Remember:

    LIST = ORDER + INDEX + DUPLICATES

```text
                    Iterable
                       |
                   Collection
                       |
                      List
                       |
          +------------+------------+
          |            |            |
      ArrayList    LinkedList     Vector
                                    |
                                  Stack