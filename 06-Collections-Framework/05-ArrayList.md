# 05 — ArrayList

> **Package:** `java.util`  
> **Since:** Java 1.2  
> **Implements:** `List<E>`, `RandomAccess`, `Cloneable`, `Serializable`  
> **Internal Structure:** Dynamically resizable array  
> **Purpose:** General-purpose List implementation for fast indexed access.

---

# 📚 Table of Contents

1. [Introduction](#1-introduction)
2. [Why ArrayList?](#2-why-arraylist)
3. [ArrayList Hierarchy](#3-arraylist-hierarchy)
4. [What is ArrayList?](#4-what-is-arraylist)
5. [Important Characteristics](#5-important-characteristics)
6. [Creating an ArrayList](#6-creating-an-arraylist)
7. [Adding Elements](#7-adding-elements)
8. [Accessing Elements](#8-accessing-elements)
9. [Updating Elements](#9-updating-elements)
10. [Removing Elements](#10-removing-elements)
11. [Searching Elements](#11-searching-elements)
12. [Size and Capacity](#12-size-and-capacity)
13. [ArrayList Capacity](#13-arraylist-capacity)
14. [Internal Working](#14-internal-working)
15. [Dynamic Resizing](#15-dynamic-resizing)
16. [Growth and Capacity](#16-growth-and-capacity)
17. [RandomAccess](#17-randomaccess)
18. [Iterator and ListIterator](#18-iterator-and-listiterator)
19. [forEach](#19-foreach)
20. [SubList](#20-sublist)
21. [Array Conversion](#21-array-conversion)
22. [Sorting](#22-sorting)
23. [replaceAll](#23-replaceall)
24. [Clone](#24-clone)
25. [TrimToSize](#25-trimtow-size)
26. [EnsureCapacity](#26-ensurecapacity)
27. [Null and Duplicate Elements](#27-null-and-duplicate-elements)
28. [ArrayList and Immutability](#28-arraylist-and-immutability)
29. [Time Complexity](#29-time-complexity)
30. [Memory and Performance](#30-memory-and-performance)
31. [ArrayList vs Array](#31-arraylist-vs-array)
32. [ArrayList vs LinkedList](#32-arraylist-vs-linkedlist)
33. [ArrayList vs Vector](#33-arraylist-vs-vector)
34. [Common Mistakes](#34-common-mistakes)
35. [Interview Traps](#35-interview-traps)
36. [DSA Relevance](#36-dsa-relevance)
37. [DSA Examples](#37-dsa-examples)
38. [Top Interview Questions](#38-top-interview-questions)
39. [30-Second Interview Answer](#39-30-second-interview-answer)
40. [Cheat Sheet](#40-cheat-sheet)
41. [Quick Revision](#41-quick-revision)

---

# 🚀 1. Introduction

`ArrayList` is one of the most commonly used classes in the Java Collection Framework.

It is a concrete implementation of the `List` interface.

Package:

    java.util.ArrayList

Basic relationship:

    Iterable
        |
    Collection
        |
       List
        |
    ArrayList

Example:

    import java.util.*;

    public class Main {

        public static void main(String[] args) {

            ArrayList<String> languages = new ArrayList<>();

            languages.add("Java");
            languages.add("Python");
            languages.add("JavaScript");

            System.out.println(languages);
        }
    }

Output:

    [Java, Python, JavaScript]

The most important characteristic of ArrayList is:

> It uses a dynamically resizable array internally.

---

# 🤔 2. Why ArrayList?

A normal Java array has a fixed size.

Example:

    String[] languages = new String[3];

Once created, its length cannot be changed.

If we need more space:

    String[] languages = new String[10];

we must create another array and copy elements manually.

`ArrayList` handles this resizing automatically.

Example:

    ArrayList<String> languages = new ArrayList<>();

    languages.add("Java");
    languages.add("Spring");
    languages.add("React");
    languages.add("Docker");

The internal storage can grow when required.

Therefore:

    Array
        -> Fixed size

    ArrayList
        -> Dynamically resizable

---

# 🌳 3. ArrayList Hierarchy

The simplified hierarchy is:

    Iterable<E>
         |
    Collection<E>
         |
       List<E>
         |
    ArrayList<E>

ArrayList also implements:

    RandomAccess
    Cloneable
    Serializable

Conceptually:

    ArrayList
       |
       +--- List
       |
       +--- RandomAccess
       |
       +--- Cloneable
       |
       +--- Serializable

Important:

`RandomAccess` is a marker interface.

It indicates that indexed access is generally efficient.

---

# 🧠 4. What is ArrayList?

`ArrayList` is a resizable-array implementation of the `List` interface.

It provides:

- Ordered elements
- Duplicate elements
- Index-based access
- Dynamic resizing
- Fast random access
- Generic type safety
- `null` support
- Multiple iteration mechanisms

Example:

    List<Integer> numbers = new ArrayList<>();

    numbers.add(10);
    numbers.add(20);
    numbers.add(10);

    System.out.println(numbers);

Output:

    [10, 20, 10]

Duplicates are allowed.

---

# 🔑 5. Important Characteristics

## 5.1 Ordered

ArrayList maintains the order in which elements are inserted.

Example:

    ArrayList<String> names = new ArrayList<>();

    names.add("A");
    names.add("C");
    names.add("B");

    System.out.println(names);

Output:

    [A, C, B]

The insertion order is maintained.

---

## 5.2 Allows Duplicates

Example:

    ArrayList<Integer> numbers = new ArrayList<>();

    numbers.add(10);
    numbers.add(10);
    numbers.add(20);

Output:

    [10, 10, 20]

---

## 5.3 Allows Null

ArrayList permits null elements.

Example:

    ArrayList<String> names = new ArrayList<>();

    names.add("Java");
    names.add(null);
    names.add("Spring");

    System.out.println(names);

Output:

    [Java, null, Spring]

---

## 5.4 Index-Based Access

Example:

    ArrayList<String> names =
        new ArrayList<>(List.of("Java", "Spring", "React"));

    System.out.println(names.get(1));

Output:

    Spring

Average indexed access:

    O(1)

---

## 5.5 Dynamic Size

Example:

    ArrayList<Integer> numbers = new ArrayList<>();

    for (int i = 1; i <= 100; i++) {
        numbers.add(i);
    }

The programmer does not need to manually create a larger array.

---

# 💻 6. Creating an ArrayList

## 6.1 Empty ArrayList

    ArrayList<String> names = new ArrayList<>();

---

## 6.2 Using List Reference

Recommended when implementation-specific methods are not required:

    List<String> names = new ArrayList<>();

This follows:

> Programming to an interface.

---

## 6.3 Initial Capacity

ArrayList provides a constructor that accepts initial capacity.

    ArrayList<String> names =
        new ArrayList<>(100);

Important:

    100

is the initial capacity.

It is NOT the size.

Immediately after creation:

    names.size()

returns:

    0

---

## 6.4 Creating from Another Collection

    List<String> names =
        new ArrayList<>(
            List.of("Java", "Spring", "React")
        );

Output:

    [Java, Spring, React]

---

# ➕ 7. Adding Elements

## 7.1 add(E)

Adds an element at the end.

    ArrayList<String> names = new ArrayList<>();

    names.add("Java");
    names.add("Spring");
    names.add("React");

    System.out.println(names);

Output:

    [Java, Spring, React]

---

## 7.2 add(index, element)

Adds an element at a specific position.

    ArrayList<String> names =
        new ArrayList<>(
            List.of("Java", "React")
        );

    names.add(1, "Spring");

    System.out.println(names);

Output:

    [Java, Spring, React]

Elements after the insertion point are shifted right.

---

## 7.3 addAll()

Adds another collection.

    ArrayList<String> first =
        new ArrayList<>(
            List.of("Java", "Spring")
        );

    ArrayList<String> second =
        new ArrayList<>(
            List.of("React", "Docker")
        );

    first.addAll(second);

    System.out.println(first);

Output:

    [Java, Spring, React, Docker]

---

## 7.4 addAll(index, collection)

    ArrayList<String> first =
        new ArrayList<>(
            List.of("Java", "React")
        );

    ArrayList<String> second =
        new ArrayList<>(
            List.of("Spring", "Hibernate")
        );

    first.addAll(1, second);

    System.out.println(first);

Output:

    [Java, Spring, Hibernate, React]

---

# 👀 8. Accessing Elements

## get(index)

Returns the element at a given index.

    ArrayList<String> names =
        new ArrayList<>(
            List.of("Java", "Spring", "React")
        );

    String value = names.get(1);

    System.out.println(value);

Output:

    Spring

---

## Index Range

If size is:

    5

valid indexes are:

    0
    1
    2
    3
    4

Invalid:

    5

Attempting:

    names.get(5);

results in:

    IndexOutOfBoundsException

---

# 🔄 9. Updating Elements

Use:

    set(index, element)

Example:

    ArrayList<String> names =
        new ArrayList<>(
            List.of("Java", "Python", "React")
        );

    names.set(1, "Spring");

    System.out.println(names);

Output:

    [Java, Spring, React]

Important:

    set()
        -> replaces

    add()
        -> inserts

---

# ❌ 10. Removing Elements

ArrayList inherits List's overloaded removal methods.

---

## 10.1 remove(index)

    ArrayList<String> names =
        new ArrayList<>(
            List.of("Java", "Spring", "React")
        );

    names.remove(1);

    System.out.println(names);

Output:

    [Java, React]

---

## 10.2 remove(Object)

    ArrayList<String> names =
        new ArrayList<>(
            List.of("Java", "Spring", "React")
        );

    names.remove("Spring");

    System.out.println(names);

Output:

    [Java, React]

---

## 10.3 Integer Trap

Consider:

    ArrayList<Integer> numbers =
        new ArrayList<>(
            List.of(10, 20, 30)
        );

    numbers.remove(1);

This removes:

    index 1

Result:

    [10, 30]

It does NOT remove the value `1`.

To remove an integer value:

    numbers.remove(Integer.valueOf(20));

Result:

    [10, 30]

This happens because List provides:

    remove(int index)

and:

    remove(Object object)

---

## 10.4 removeAll()

    ArrayList<Integer> numbers =
        new ArrayList<>(
            List.of(10, 20, 30, 40)
        );

    numbers.removeAll(
        List.of(20, 40)
    );

    System.out.println(numbers);

Output:

    [10, 30]

---

## 10.5 removeIf()

    ArrayList<Integer> numbers =
        new ArrayList<>(
            List.of(10, 15, 20, 25)
        );

    numbers.removeIf(n -> n % 2 == 0);

    System.out.println(numbers);

Output:

    [15, 25]

---

## 10.6 clear()

    ArrayList<String> names =
        new ArrayList<>(
            List.of("Java", "Spring", "React")
        );

    names.clear();

    System.out.println(names);

Output:

    []

The object still exists.

Only its elements are removed.

---

# 🔍 11. Searching Elements

## 11.1 contains()

    ArrayList<String> names =
        new ArrayList<>(
            List.of("Java", "Spring", "React")
        );

    System.out.println(
        names.contains("Java")
    );

Output:

    true

---

## 11.2 indexOf()

    ArrayList<String> names =
        new ArrayList<>(
            List.of("Java", "Spring", "Java")
        );

    System.out.println(
        names.indexOf("Java")
    );

Output:

    0

---

## 11.3 lastIndexOf()

    ArrayList<String> names =
        new ArrayList<>(
            List.of("Java", "Spring", "Java")
        );

    System.out.println(
        names.lastIndexOf("Java")
    );

Output:

    2

---

# 📏 12. Size and Capacity

Two concepts must be clearly distinguished:

    Size
    Capacity

---

## Size

Number of actual elements currently stored.

Example:

    ArrayList<Integer> numbers =
        new ArrayList<>();

    numbers.add(10);
    numbers.add(20);
    numbers.add(30);

    System.out.println(numbers.size());

Output:

    3

---

## Capacity

The amount of internal storage currently available before another resize is required.

Capacity is an internal implementation detail.

Example:

    ArrayList<Integer> numbers =
        new ArrayList<>(100);

Here:

    size = 0

while the requested initial capacity is:

    100

Therefore:

> Capacity and size are completely different concepts.

---

# ⚙️ 13. ArrayList Capacity

Conceptually:

    ArrayList
        |
        v
    Internal array
        |
        +--- [0]
        +--- [1]
        +--- [2]
        +--- [3]
        +--- ...
        +--- [capacity - 1]

Suppose:

    size = 3
    capacity = 10

Conceptually:

    [10][20][30][ ][ ][ ][ ][ ][ ][ ]

Actual elements:

    3

Available capacity:

    7

The empty positions are not counted by:

    size()

---

# ⚙️ 14. Internal Working

This is one of the most important ArrayList interview topics.

Conceptually, ArrayList contains an internal array that stores references to elements.

Simplified:

    ArrayList
        |
        v
    Object[]
        |
        +--- element 0
        +--- element 1
        +--- element 2
        +--- element 3
        +--- ...

Suppose:

    ArrayList<Integer> numbers =
        new ArrayList<>();

    numbers.add(10);
    numbers.add(20);
    numbers.add(30);

Conceptually:

    Internal array:

    [10][20][30][ ][ ][ ]

The array can grow when more elements need to be stored.

---

# 📈 15. Dynamic Resizing

Suppose the internal array becomes full.

Example:

    Capacity = 3

    [10][20][30]

Now we execute:

    numbers.add(40);

There is no free slot.

ArrayList must resize.

Conceptually:

    Old array:

    [10][20][30]

Then:

    1. Create a larger array.
    2. Copy existing references.
    3. Add the new element.
    4. Replace the old internal storage.

Conceptually:

    Old:
    [10][20][30]

             |
             | resize
             v

    New:
    [10][20][30][40][ ][ ]

The exact growth calculation is an implementation detail and should not be assumed as part of the `List` contract.

---

# 📈 16. Growth and Capacity

A common misconception is:

> "ArrayList always doubles its size."

This is not correct.

Modern OpenJDK implementations generally grow the internal array by approximately 50% when growth is required, subject to implementation details and limits.

For interview purposes:

    ArrayList growth
        -> creates a larger internal array
        -> copies existing references
        -> continues storing elements

Do not rely on an exact growth factor as a universal Java specification rule.

---

## Amortized O(1)

Adding an element at the end is commonly described as:

    O(1) amortized

Why?

Most additions do not require resizing.

Occasionally, a resize requires copying elements:

    O(n)

But over a large sequence of additions, the average cost per insertion remains amortized constant time.

---

# 🎯 17. RandomAccess

ArrayList implements:

    RandomAccess

This is a marker interface.

It does not define methods.

Its purpose is to indicate that indexed access is efficient.

Example:

    list.get(5000);

For ArrayList, accessing an element by index is generally:

    O(1)

because the underlying array allows direct index calculation.

Conceptually:

    base address + index × element-reference-size

The JVM handles the actual memory representation, but conceptually this explains why array indexing is fast.

---

# 🔄 18. Iterator and ListIterator

ArrayList supports:

    Iterator
    ListIterator

---

## Iterator

    ArrayList<String> names =
        new ArrayList<>(
            List.of("Java", "Spring", "React")
        );

    Iterator<String> iterator =
        names.iterator();

    while (iterator.hasNext()) {

        String value = iterator.next();

        System.out.println(value);
    }

Output:

    Java
    Spring
    React

---

## Removing During Iteration

Use the iterator's own remove method.

    ArrayList<Integer> numbers =
        new ArrayList<>(
            List.of(10, 20, 30, 40)
        );

    Iterator<Integer> iterator =
        numbers.iterator();

    while (iterator.hasNext()) {

        Integer number = iterator.next();

        if (number % 2 == 0) {
            iterator.remove();
        }
    }

    System.out.println(numbers);

Output:

    []

---

## ListIterator

    ArrayList<String> names =
        new ArrayList<>(
            List.of("Java", "Spring", "React")
        );

    ListIterator<String> iterator =
        names.listIterator();

    while (iterator.hasNext()) {
        System.out.println(iterator.next());
    }

Output:

    Java
    Spring
    React

Backward:

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

# 🔁 19. forEach

ArrayList supports `forEach()` through `Iterable`.

Example:

    ArrayList<String> names =
        new ArrayList<>(
            List.of("Java", "Spring", "React")
        );

    names.forEach(
        name -> System.out.println(name)
    );

Output:

    Java
    Spring
    React

Method reference:

    names.forEach(System.out::println);

---

# ✂️ 20. SubList

`subList()` returns a view of a portion of the ArrayList.

Example:

    ArrayList<String> names =
        new ArrayList<>(
            List.of("A", "B", "C", "D", "E")
        );

    List<String> sub =
        names.subList(1, 4);

    System.out.println(sub);

Output:

    [B, C, D]

Remember:

    fromIndex -> inclusive

    toIndex -> exclusive

---

## SubList Is a View

Example:

    ArrayList<String> names =
        new ArrayList<>(
            List.of("A", "B", "C", "D")
        );

    List<String> sub =
        names.subList(1, 3);

    sub.set(0, "X");

    System.out.println(names);

Output:

    [A, X, C, D]

The change is visible in the original list.

Therefore:

> `subList()` does not automatically create an independent copy.

---

# 🔄 21. Array Conversion

## toArray()

    ArrayList<String> names =
        new ArrayList<>(
            List.of("Java", "Spring")
        );

    Object[] array =
        names.toArray();

    System.out.println(
        Arrays.toString(array)
    );

Output:

    [Java, Spring]

---

## Typed toArray()

    String[] array =
        names.toArray(new String[0]);

    System.out.println(
        Arrays.toString(array)
    );

Output:

    [Java, Spring]

---

## Modern Form

    String[] array =
        names.toArray(String[]::new);

---

# 📊 22. Sorting

ArrayList inherits List's sorting support.

Example:

    ArrayList<Integer> numbers =
        new ArrayList<>(
            List.of(40, 10, 30, 20)
        );

    numbers.sort(null);

    System.out.println(numbers);

Output:

    [10, 20, 30, 40]

---

## Descending Order

    numbers.sort(
        Comparator.reverseOrder()
    );

Output:

    [40, 30, 20, 10]

---

## Collections.sort()

Older/common utility style:

    Collections.sort(numbers);

Both approaches sort the List.

---

# 🔄 23. replaceAll

`replaceAll()` applies a unary operation to each element.

Example:

    ArrayList<Integer> numbers =
        new ArrayList<>(
            List.of(1, 2, 3, 4)
        );

    numbers.replaceAll(
        n -> n * 10
    );

    System.out.println(numbers);

Output:

    [10, 20, 30, 40]

---

## String Example

    ArrayList<String> names =
        new ArrayList<>(
            List.of("java", "spring", "react")
        );

    names.replaceAll(
        String::toUpperCase
    );

    System.out.println(names);

Output:

    [JAVA, SPRING, REACT]

---

# 🧬 24. Clone

ArrayList implements `Cloneable`.

It provides:

    clone()

Example:

    ArrayList<String> original =
        new ArrayList<>(
            List.of("Java", "Spring")
        );

    ArrayList<String> copy =
        (ArrayList<String>) original.clone();

    System.out.println(copy);

Output:

    [Java, Spring]

Important:

> `clone()` creates a shallow copy.

If the elements themselves are mutable objects, the object references are copied rather than recursively cloning those objects.

---

# 📉 25. TrimToSize

ArrayList provides:

    trimToSize()

It reduces the internal capacity to the current size, subject to implementation behavior.

Example:

    ArrayList<Integer> numbers =
        new ArrayList<>(100);

    numbers.add(10);
    numbers.add(20);
    numbers.add(30);

    numbers.trimToSize();

Now the internal capacity can be reduced to approximately the current size.

Important:

    size()
        -> number of elements

    trimToSize()
        -> adjusts internal capacity

`trimToSize()` is mainly an implementation-level memory optimization and is not usually necessary in everyday code.

---

# 📈 26. EnsureCapacity

ArrayList provides:

    ensureCapacity(int minCapacity)

It can be used when we know approximately how many elements will be added.

Example:

    ArrayList<Integer> numbers =
        new ArrayList<>();

    numbers.ensureCapacity(1000);

    for (int i = 0; i < 1000; i++) {
        numbers.add(i);
    }

This can reduce the number of resizing operations.

Important:

> `ensureCapacity()` affects capacity, not logical size.

After:

    numbers.ensureCapacity(1000);

we still have:

    numbers.size() == 0

---

# ⚠️ 27. Null and Duplicate Elements

## Duplicate Elements

ArrayList allows duplicates.

    ArrayList<String> names =
        new ArrayList<>();

    names.add("Java");
    names.add("Java");

    System.out.println(names);

Output:

    [Java, Java]

---

## Null Elements

ArrayList allows null.

    ArrayList<String> names =
        new ArrayList<>();

    names.add(null);

    System.out.println(names);

Output:

    [null]

Multiple nulls can also exist:

    names.add(null);
    names.add(null);

Result:

    [null, null, null]

---

# 🔒 28. ArrayList and Immutability

A normal ArrayList is mutable.

Example:

    ArrayList<String> names =
        new ArrayList<>(
            List.of("Java", "Spring")
        );

    names.add("React");

    names.set(0, "JavaScript");

    names.remove("Spring");

All of these operations are allowed.

---

## Unmodifiable View

    ArrayList<String> original =
        new ArrayList<>(
            List.of("Java", "Spring")
        );

    List<String> unmodifiable =
        Collections.unmodifiableList(original);

    unmodifiable.add("React");

This throws:

    UnsupportedOperationException

---

## Mutable Copy

If we need a mutable ArrayList from an unmodifiable List:

    List<String> source =
        List.of("Java", "Spring");

    ArrayList<String> mutable =
        new ArrayList<>(source);

    mutable.add("React");

    System.out.println(mutable);

Output:

    [Java, Spring, React]

---

# ⏱️ 29. Time Complexity

Typical ArrayList complexity:

| Operation | Average / Typical Complexity |
|---|---:|
| `get(index)` | O(1) |
| `set(index)` | O(1) |
| `add(element)` at end | O(1) amortized |
| `add(index, element)` | O(n) |
| `remove(index)` | O(n) |
| `remove(Object)` | O(n) |
| `contains()` | O(n) |
| `indexOf()` | O(n) |
| `lastIndexOf()` | O(n) |
| `size()` | O(1) |
| `isEmpty()` | O(1) |
| `clear()` | O(n) |
| `subList()` | O(1) view creation in typical implementation |

Important:

> Complexity can depend on the operation and implementation details. The table describes normal ArrayList behavior.

---

# 💾 30. Memory and Performance

ArrayList stores references in an internal array.

Conceptually:

    ArrayList
        |
        v
    Object[]
        |
        +--- reference
        +--- reference
        +--- reference
        +--- ...

For objects:

    ArrayList<String>

does not store the complete String object inside the array.

It stores references to String objects.

Conceptually:

    ArrayList internal array

    [ref] [ref] [ref] [ref]

      |     |     |
      v     v     v

    String String String

This is an important JVM/memory concept.

---

## Memory Overhead

ArrayList can have unused capacity.

Example:

    size = 5
    capacity = 10

There are:

    5 actual elements

and:

    5 unused slots

This extra capacity helps avoid resizing on every insertion.

---

# 🆚 31. ArrayList vs Array

| Feature | Array | ArrayList |
|---|---|---|
| Size | Fixed | Dynamic |
| Primitive types | Yes | No direct primitive storage |
| Generics | No | Yes |
| Built-in methods | Limited | Many |
| Index access | Yes | Yes |
| Resizing | Manual | Automatic |
| Framework integration | Limited | Full Collection Framework |
| Performance | Very efficient | Very efficient for general object lists |

Important:

    int[] numbers

stores primitive `int` values directly.

But:

    ArrayList<Integer> numbers

stores references to `Integer` objects.

Java may use autoboxing/unboxing when converting between:

    int

and:

    Integer

---

# 🆚 32. ArrayList vs LinkedList

| Feature | ArrayList | LinkedList |
|---|---|---|
| Internal structure | Dynamic array | Doubly linked nodes |
| Random access | Fast | Slow |
| `get(index)` | O(1) | O(n) |
| End insertion | O(1) amortized | O(1) |
| Middle insertion | O(n) | O(n) to locate position |
| Memory overhead | Lower | Higher |
| Cache locality | Generally better | Generally worse |
| Implements Deque | No | Yes |
| Typical use | General-purpose List | List + Deque behavior |

For most general-purpose List use cases:

    ArrayList

is usually the first implementation to consider.

---

# 🆚 33. ArrayList vs Vector

| Feature | ArrayList | Vector |
|---|---|---|
| Introduced | Java 1.2 Collection Framework | Legacy |
| Synchronization | Not synchronized by default | Legacy synchronized methods |
| Performance | Generally better for unsynchronized use | Synchronization overhead |
| Modern choice | Common | Usually avoided for new code |
| Dynamic array | Yes | Yes |

Important:

> `Vector` is a legacy class. Its historical synchronization should not be confused with being the universal modern solution for thread safety.

For concurrent applications, choose a concurrency strategy appropriate to the workload.

---

# 🚨 34. Common Mistakes

## Mistake 1 — Confusing Size and Capacity

Wrong:

    new ArrayList<>(100)

means:

    size = 100

Correct:

    size = 0

    initial capacity = 100

---

## Mistake 2 — Thinking ArrayList Uses Linked Nodes

Wrong:

    ArrayList -> linked list internally

Correct:

    ArrayList -> dynamically resizable array

---

## Mistake 3 — Thinking add() is Always O(1)

More accurately:

    add(end)
        -> O(1) amortized

A resize can require:

    O(n)

copying of references.

---

## Mistake 4 — Thinking LinkedList Is Always Faster

Performance depends on the operation and access pattern.

---

## Mistake 5 — Forgetting Integer remove() Overloading

    numbers.remove(1)

means:

    remove index 1

not:

    remove value 1

---

## Mistake 6 — Thinking subList() Is Independent

It generally represents a view backed by the original List.

---

## Mistake 7 — Thinking ensureCapacity() Changes Size

It doesn't.

    ensureCapacity(1000)

does not add 1000 elements.

---

## Mistake 8 — Thinking trimToSize() Removes Elements

It doesn't.

It adjusts internal capacity.

---

# 🎯 35. Interview Traps

## Trap 1

Question:

> How does ArrayList grow dynamically?

Answer:

When the internal array lacks enough capacity, ArrayList allocates a larger array and copies existing references into it.

---

## Trap 2

Question:

> Is ArrayList synchronized?

Answer:

No.

It is not synchronized by default.

---

## Trap 3

Question:

> What interface indicates efficient random access?

Answer:

    RandomAccess

ArrayList implements it.

---

## Trap 4

Question:

> What is the difference between size and capacity?

Answer:

    size
        -> number of actual elements

    capacity
        -> amount of internal storage available before resizing

---

## Trap 5

Question:

> Is ArrayList internally an array?

Answer:

Yes, conceptually and in standard JDK implementations it uses a resizable array internally.

---

## Trap 6

Question:

> Does ArrayList store objects directly?

More precisely, for reference types, its internal array stores references to objects.

---

## Trap 7

Question:

> Why is get(index) O(1)?

Because the underlying array supports direct indexed access.

---

## Trap 8

Question:

> Why is insertion at the beginning O(n)?

Existing elements need to be shifted to make room for the new element.

---

## Trap 9

Question:

> What happens when ArrayList becomes full?

A larger internal array is allocated and existing element references are copied.

---

## Trap 10

Question:

> Does ArrayList allow null?

Yes.

ArrayList permits null elements.

---

## Trap 11

Question:

> Does ArrayList allow duplicates?

Yes.

---

## Trap 12

Question:

> Is ArrayList thread-safe?

No.

It is not synchronized by default.

---

# 🧠 36. DSA Relevance

ArrayList is extremely useful in DSA.

Common uses:

- Dynamic arrays
- Two-pointer problems
- Sliding window
- Prefix/suffix arrays
- Sorting
- Searching
- Graph adjacency lists
- Storing results
- Matrix-like structures
- Building custom data structures

---

## Graph Representation

Example:

    List<List<Integer>> graph =
        new ArrayList<>();

    int vertices = 5;

    for (int i = 0; i < vertices; i++) {
        graph.add(new ArrayList<>());
    }

    graph.get(0).add(1);
    graph.get(0).add(2);

    graph.get(1).add(3);

    graph.get(2).add(4);

Conceptually:

    0 -> 1, 2
    1 -> 3
    2 -> 4
    3 ->
    4 ->

This is a standard adjacency-list representation.

---

# 💡 37. DSA Examples

## Example 1 — Reverse ArrayList

    ArrayList<Integer> numbers =
        new ArrayList<>(
            List.of(1, 2, 3, 4, 5)
        );

    Collections.reverse(numbers);

    System.out.println(numbers);

Output:

    [5, 4, 3, 2, 1]

---

## Example 2 — Find Maximum

    ArrayList<Integer> numbers =
        new ArrayList<>(
            List.of(10, 50, 20, 40, 30)
        );

    int maximum =
        Collections.max(numbers);

    System.out.println(maximum);

Output:

    50

---

## Example 3 — Find Minimum

    ArrayList<Integer> numbers =
        new ArrayList<>(
            List.of(10, 50, 20, 40, 30)
        );

    int minimum =
        Collections.min(numbers);

    System.out.println(minimum);

Output:

    10

---

## Example 4 — Sort

    ArrayList<Integer> numbers =
        new ArrayList<>(
            List.of(40, 10, 30, 20)
        );

    numbers.sort(null);

    System.out.println(numbers);

Output:

    [10, 20, 30, 40]

---

## Example 5 — Remove Even Numbers

    ArrayList<Integer> numbers =
        new ArrayList<>(
            List.of(1, 2, 3, 4, 5, 6)
        );

    numbers.removeIf(
        n -> n % 2 == 0
    );

    System.out.println(numbers);

Output:

    [1, 3, 5]

---

## Example 6 — Find an Element

    ArrayList<Integer> numbers =
        new ArrayList<>(
            List.of(10, 20, 30, 40)
        );

    int target = 30;

    if (numbers.contains(target)) {
        System.out.println("Found");
    }

Output:

    Found

---

## Example 7 — Two Pointer

    ArrayList<Integer> numbers =
        new ArrayList<>(
            List.of(10, 20, 30, 40, 50)
        );

    int left = 0;
    int right = numbers.size() - 1;

    while (left < right) {

        System.out.println(
            numbers.get(left) +
            " " +
            numbers.get(right)
        );

        left++;
        right--;
    }

Output:

    10 50
    20 40

---

# 💼 38. Top Interview Questions

## Q1. What is ArrayList?

`ArrayList` is a resizable-array implementation of the `List` interface in Java.

---

## Q2. How does ArrayList work internally?

It uses an internal array to store references to elements. When capacity is insufficient, a larger array is created and existing references are copied.

---

## Q3. Is ArrayList synchronized?

No.

It is not synchronized by default.

---

## Q4. Is ArrayList thread-safe?

Not by default.

If multiple threads modify the same ArrayList concurrently, external synchronization or another appropriate concurrent collection may be required depending on the use case.

---

## Q5. What is the default capacity of ArrayList?

This question requires precision.

Do not simply say:

    "10"

Modern JDK implementations may use lazy allocation.

Historically, ArrayList implementations have commonly used a default capacity of 10 when the first element is added, but the exact internal behavior is an implementation detail rather than a guaranteed API contract.

---

## Q6. What is the difference between size and capacity?

    size
        -> number of stored elements

    capacity
        -> internal storage available before resizing

---

## Q7. What happens when ArrayList reaches capacity?

It grows its internal storage by allocating a larger array and copying existing element references.

---

## Q8. What is the time complexity of get()?

Typically:

    O(1)

---

## Q9. What is the time complexity of add(element)?

At the end:

    O(1) amortized

A resize can temporarily make an individual insertion:

    O(n)

---

## Q10. What is the complexity of insertion at index 0?

Typically:

    O(n)

because existing elements need to be shifted.

---

## Q11. What is the complexity of remove(index)?

Typically:

    O(n)

because elements after the removed position may need to be shifted.

---

## Q12. Does ArrayList allow duplicates?

Yes.

---

## Q13. Does ArrayList allow null?

Yes.

---

## Q14. Why is ArrayList faster than LinkedList for get(index)?

ArrayList uses an array that supports direct indexed access, while LinkedList generally needs traversal to reach an arbitrary index.

---

## Q15. Why does ArrayList implement RandomAccess?

To indicate that indexed access is efficient.

---

## Q16. What is ensureCapacity()?

It requests that the ArrayList ensure enough internal capacity for at least the specified number of elements without changing the logical size.

---

## Q17. What is trimToSize()?

It requests that the ArrayList reduce its internal capacity to approximately its current size.

---

## Q18. What is subList()?

It returns a view of a portion of the list.

---

## Q19. What is a shallow copy?

A shallow copy copies references rather than recursively copying the referenced objects.

ArrayList's `clone()` performs a shallow copy.

---

## Q20. ArrayList vs LinkedList?

ArrayList:

    Dynamic array
    Fast random access
    Good cache locality
    General-purpose choice

LinkedList:

    Linked nodes
    Slower random access
    Implements Deque
    Higher node memory overhead

---

## Q21. ArrayList vs Vector?

ArrayList is the modern general-purpose List implementation and is not synchronized by default.

Vector is a legacy synchronized implementation.

---

## Q22. Can ArrayList store primitive types?

No.

Generics work with reference types.

Therefore:

    ArrayList<int>

is invalid.

Use:

    ArrayList<Integer>

Java autoboxing converts:

    int -> Integer

when needed.

---

## Q23. Why is ArrayList<Integer> different from int[]?

    int[]

stores primitive integers directly.

    ArrayList<Integer>

stores references to Integer objects.

---

## Q24. What is fail-fast behavior?

ArrayList iterators are generally fail-fast.

If the ArrayList is structurally modified outside the iterator while the iterator is being used, the iterator may throw:

    ConcurrentModificationException

Example:

    ArrayList<Integer> numbers =
        new ArrayList<>(
            List.of(10, 20, 30)
        );

    for (Integer number : numbers) {

        if (number == 20) {
            numbers.remove(number);
        }
    }

This can result in:

    ConcurrentModificationException

Use the iterator's own remove method when removing during iteration:

    Iterator<Integer> iterator =
        numbers.iterator();

    while (iterator.hasNext()) {

        Integer number = iterator.next();

        if (number == 20) {
            iterator.remove();
        }
    }

Important:

> Fail-fast behavior is a best-effort implementation mechanism, not a synchronization guarantee.

---

# ⏱️ 39. 30-Second Interview Answer

If the interviewer asks:

> "What is ArrayList and how does it work internally?"

Answer:

> "`ArrayList` is a resizable-array implementation of the `List` interface in Java. It maintains insertion order, allows duplicates and null values, and provides efficient indexed access, typically O(1). Internally, it uses an array-like structure to store element references. When the available capacity is insufficient, it creates a larger internal array and copies the existing references. Adding at the end is O(1) amortized, while insertion or removal at arbitrary positions is typically O(n) because elements may need to be shifted. ArrayList is not synchronized by default and implements `RandomAccess` to indicate efficient indexed access."

---

# 📌 40. Cheat Sheet

## Basic

    ArrayList
        -> java.util
        -> Implements List
        -> Resizable array
        -> Ordered
        -> Allows duplicates
        -> Allows null
        -> Not synchronized
        -> Fast random access

---

## Hierarchy

    Iterable
        |
    Collection
        |
    List
        |
    ArrayList

Interfaces:

    RandomAccess
    Cloneable
    Serializable

---

## Important Methods

    add()
    add(index, element)

    addAll()
    addAll(index, collection)

    get()

    set()

    remove(index)
    remove(Object)

    contains()

    indexOf()
    lastIndexOf()

    size()
    isEmpty()

    clear()

    removeAll()
    removeIf()
    retainAll()

    subList()

    iterator()
    listIterator()

    forEach()

    replaceAll()

    sort()

    toArray()

---

## Special Methods

    ensureCapacity()

    trimToSize()

    clone()

---

## Complexity

    get(index)
        -> O(1)

    set(index)
        -> O(1)

    add(end)
        -> O(1) amortized

    add(index)
        -> O(n)

    remove(index)
        -> O(n)

    contains()
        -> O(n)

    indexOf()
        -> O(n)

    size()
        -> O(1)

---

# ⚡ 41. Quick Revision

Remember:

    ArrayList = Dynamic Array

The mental model:

    ArrayList
        |
        v
    Internal Array
        |
        +--- [0]
        +--- [1]
        +--- [2]
        +--- [3]
        +--- ...

When full:

    Old Array
        |
        v
    Larger Array
        |
        v
    Copy References
        |
        v
    Continue Adding

---

## Most Important Concepts

    1. ArrayList implements List.

    2. ArrayList uses a dynamically resizable array.

    3. get(index) is typically O(1).

    4. add(end) is O(1) amortized.

    5. Insertion/removal in the middle is typically O(n).

    6. ArrayList allows duplicates.

    7. ArrayList allows null.

    8. ArrayList is not synchronized.

    9. ArrayList implements RandomAccess.

    10. Size and capacity are different.

    11. ensureCapacity() changes capacity planning, not size.

    12. trimToSize() can reduce unused capacity.

    13. subList() returns a view.

    14. clone() creates a shallow copy.

    15. ArrayList stores references for reference-type elements.

    16. ArrayList is generally preferred over Vector for modern general-purpose List usage.

---

# 🎯 Interview Must-Know

Before moving to `06-LinkedList.md`, make sure you can explain:

    1. What is ArrayList?
    2. How does ArrayList work internally?
    3. What is the difference between size and capacity?
    4. How does ArrayList grow?
    5. What happens when the internal array becomes full?
    6. Why is get(index) O(1)?
    7. Why is insertion at the beginning O(n)?
    8. Why is add(end) O(1) amortized?
    9. What is RandomAccess?
    10. Is ArrayList synchronized?
    11. Does ArrayList allow duplicates?
    12. Does ArrayList allow null?
    13. What is ensureCapacity()?
    14. What is trimToSize()?
    15. What is subList()?
    16. What is a shallow copy?
    17. What does clone() do?
    18. ArrayList vs LinkedList?
    19. ArrayList vs Vector?
    20. Why is ArrayList commonly used in DSA?

---

# 🚀 Final Takeaway

The most important mental model is:

    List
      |
      v
    ArrayList
      |
      v
    Dynamic Array
      |
      +--- Fast indexed access
      |
      +--- Ordered
      |
      +--- Allows duplicates
      |
      +--- Allows null
      |
      +--- Automatically resizes
      |
      +--- Not synchronized by default

The key interview line to remember:

> **ArrayList is a resizable-array implementation of List that provides fast random access through indexes, while insertions and removals at arbitrary positions can be expensive because elements may need to be shifted.**