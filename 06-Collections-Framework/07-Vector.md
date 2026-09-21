# 07 — Vector

> **Package:** `java.util`  
> **Since:** Java 1.0  
> **Type:** Class  
> **Implements:** `List<E>`, `RandomAccess`, `Cloneable`, `Serializable`  
> **Internal Structure:** Growable array  
> **Key Feature:** Synchronized legacy List implementation

---

# 📚 Table of Contents

1. [Introduction](#1-introduction)
2. [Why Vector Exists](#2-why-vector-exists)
3. [Vector Hierarchy](#3-vector-hierarchy)
4. [What is Vector](#4-what-is-vector)
5. [Important Characteristics](#5-important-characteristics)
6. [Creating a Vector](#6-creating-a-vector)
7. [Adding Elements](#7-adding-elements)
8. [Accessing Elements](#8-accessing-elements)
9. [Updating Elements](#9-updating-elements)
10. [Removing Elements](#10-removing-elements)
11. [Searching](#11-searching)
12. [Capacity vs Size](#12-capacity-vs-size)
13. [Vector Capacity Methods](#13-vector-capacity-methods)
14. [Internal Working](#14-internal-working)
15. [How Vector Grows](#15-how-vector-grows)
16. [Vector Constructors](#16-vector-constructors)
17. [Enumeration](#17-enumeration)
18. [Iterator and ListIterator](#18-iterator-and-listiterator)
19. [Thread Safety](#19-thread-safety)
20. [Vector and Synchronization](#20-vector-and-synchronization)
21. [Vector vs ArrayList](#21-vector-vs-arraylist)
22. [Vector vs LinkedList](#22-vector-vs-linkedlist)
23. [Vector vs Array](#23-vector-vs-array)
24. [Vector and Stack](#24-vector-and-stack)
25. [Performance](#25-performance)
26. [Advantages](#26-advantages)
27. [Disadvantages](#27-disadvantages)
28. [Common Mistakes](#28-common-mistakes)
29. [Interview Traps](#29-interview-traps)
30. [DSA Relevance](#30-dsa-relevance)
31. [Practical Examples](#31-practical-examples)
32. [Top Interview Questions](#32-top-interview-questions)
33. [30-Second Interview Answer](#33-30-second-interview-answer)
34. [Cheat Sheet](#34-cheat-sheet)
35. [Quick Revision](#35-quick-revision)

---

# 🚀 1. Introduction

`Vector` is one of the oldest collection classes in Java.

It was introduced in:

    Java 1.0

It belongs to:

    java.util

Unlike modern collection classes that were introduced with the Java Collections Framework in Java 1.2, Vector existed before the framework.

Later, Vector was retrofitted to implement:

    List<E>

Vector is essentially a:

    synchronized growable array

Example:

    import java.util.Vector;

    public class Main {

        public static void main(String[] args) {

            Vector<String> names =
                new Vector<>();

            names.add("Java");
            names.add("Spring");
            names.add("React");

            System.out.println(names);
        }
    }

Output:

    [Java, Spring, React]

---

# 🤔 2. Why Vector Exists

Before the Java Collections Framework, Java already had collection-like classes.

Vector was one of them.

It provided a dynamically growing array instead of a fixed-size array.

Normal array:

    int[] numbers = new int[5];

Its size is fixed.

Vector:

    Vector<Integer> numbers =
        new Vector<>();

can grow dynamically.

Historically, Vector also provided synchronized methods.

Therefore, its original design was aimed at use cases where synchronized access was desired.

Today, however, Vector is considered a legacy collection class.

For most new code, developers usually prefer:

    ArrayList

for a normal List, or:

    other modern concurrent collections

when concurrency requirements actually demand them.

---

# 🌳 3. Vector Hierarchy

Simplified hierarchy:

    Iterable<E>
         |
    Collection<E>
         |
       List<E>
         |
       Vector<E>

Vector also implements:

    RandomAccess
    Cloneable
    Serializable

Therefore:

    Vector
       |
       +--- List
       |
       +--- RandomAccess
       |
       +--- Cloneable
       |
       +--- Serializable

Important:

Vector is a class, not an interface.

---

# 🧠 4. What is Vector?

`Vector` is a legacy, synchronized, dynamically resizable array implementation of the `List` interface.

The key characteristics are:

    Dynamic
    Ordered
    Indexed
    Allows duplicates
    Allows null
    Synchronized methods
    Random access
    Growable array

Conceptually:

    Vector
      |
      v

    [A][B][C][D][ ][ ][ ][ ]

The underlying storage is array-based.

Therefore:

    get(index)

is generally:

    O(1)

because Vector implements `RandomAccess`.

---

# 🔑 5. Important Characteristics

## 5.1 Dynamic Size

Vector can grow automatically.

    Vector<Integer> numbers =
        new Vector<>();

    numbers.add(10);
    numbers.add(20);
    numbers.add(30);

No fixed size is required.

---

## 5.2 Maintains Insertion Order

    Vector<String> names =
        new Vector<>();

    names.add("A");
    names.add("C");
    names.add("B");

    System.out.println(names);

Output:

    [A, C, B]

---

## 5.3 Allows Duplicates

    Vector<Integer> numbers =
        new Vector<>();

    numbers.add(10);
    numbers.add(10);

Output:

    [10, 10]

---

## 5.4 Allows Null

    Vector<String> names =
        new Vector<>();

    names.add(null);

Output:

    [null]

---

## 5.5 Supports Index-Based Access

    Vector<String> names =
        new Vector<>();

    names.add("Java");
    names.add("Spring");

    System.out.println(
        names.get(1)
    );

Output:

    Spring

---

## 5.6 Synchronized

Vector's legacy methods are synchronized.

This means method-level operations provide synchronization around individual calls.

Important:

> Synchronized does not automatically make every multi-step operation on a Vector atomic.

---

## 5.7 Implements RandomAccess

Vector implements:

    RandomAccess

Therefore indexed access is intended to be efficient.

---

# 💻 6. Creating a Vector

## 6.1 Default Constructor

    Vector<String> names =
        new Vector<>();

---

## 6.2 Initial Capacity

    Vector<String> names =
        new Vector<>(20);

This creates a Vector with an initial capacity of 20.

Important:

    capacity != size

Initially:

    size = 0
    capacity = 20

---

## 6.3 Initial Capacity + Capacity Increment

    Vector<String> names =
        new Vector<>(10, 5);

Meaning:

    initial capacity = 10
    capacity increment = 5

When the Vector needs to grow, the capacity can increase according to the configured increment.

---

## 6.4 From Collection

    Vector<String> names =
        new Vector<>(
            java.util.List.of(
                "Java",
                "Spring",
                "React"
            )
        );

Output:

    [Java, Spring, React]

---

# ➕ 7. Adding Elements

## 7.1 add()

    Vector<String> names =
        new Vector<>();

    names.add("Java");
    names.add("Spring");

    System.out.println(names);

Output:

    [Java, Spring]

---

## 7.2 add(index, element)

    Vector<String> names =
        new Vector<>();

    names.add("Java");
    names.add("React");

    names.add(1, "Spring");

    System.out.println(names);

Output:

    [Java, Spring, React]

---

## 7.3 addElement()

Vector also has a legacy method:

    addElement()

Example:

    Vector<String> names =
        new Vector<>();

    names.addElement("Java");
    names.addElement("Spring");

This is a legacy Vector-specific method.

For modern code, the List method:

    add()

is generally preferred.

---

## 7.4 addAll()

    Vector<String> names =
        new Vector<>();

    names.addAll(
        java.util.List.of(
            "Java",
            "Spring",
            "React"
        )
    );

---

## 7.5 addAll(index, collection)

    Vector<String> names =
        new Vector<>(
            java.util.List.of(
                "Java",
                "React"
            )
        );

    names.addAll(
        1,
        java.util.List.of(
            "Spring",
            "Hibernate"
        )
    );

Result:

    [Java, Spring, Hibernate, React]

---

# 👀 8. Accessing Elements

## get(index)

    Vector<String> names =
        new Vector<>(
            java.util.List.of(
                "Java",
                "Spring",
                "React"
            )
        );

    System.out.println(
        names.get(1)
    );

Output:

    Spring

Complexity:

    O(1)

---

## elementAt(index)

Vector also provides the legacy method:

    elementAt()

Example:

    System.out.println(
        names.elementAt(1)
    );

Output:

    Spring

Modern List code generally uses:

    get(index)

---

## firstElement()

    System.out.println(
        names.firstElement()
    );

---

## lastElement()

    System.out.println(
        names.lastElement()
    );

---

# 🔄 9. Updating Elements

## set()

    Vector<String> names =
        new Vector<>(
            java.util.List.of(
                "Java",
                "Spring",
                "React"
            )
        );

    names.set(1, "Hibernate");

    System.out.println(names);

Output:

    [Java, Hibernate, React]

---

## setElementAt()

Legacy Vector method:

    names.setElementAt(
        "Spring Boot",
        1
    );

Result:

    [Java, Spring Boot, React]

Modern code generally uses:

    set(index, element)

---

# ❌ 10. Removing Elements

## remove(index)

    Vector<String> numbers =
        new Vector<>(
            java.util.List.of(
                10,
                20,
                30
            )
        );

    numbers.remove(1);

Result:

    [10, 30]

---

## remove(Object)

    Vector<String> names =
        new Vector<>(
            java.util.List.of(
                "Java",
                "Spring",
                "React"
            )
        );

    names.remove("Spring");

Result:

    [Java, React]

---

## removeElement()

Legacy method:

    names.removeElement("Java");

---

## removeElementAt()

    names.removeElementAt(0);

---

## removeAllElements()

Legacy method:

    names.removeAllElements();

Result:

    []

Modern List code generally uses:

    clear()

---

## clear()

    names.clear();

---

# 🔍 11. Searching

## contains()

    Vector<String> names =
        new Vector<>(
            java.util.List.of(
                "Java",
                "Spring",
                "React"
            )
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

    Vector<String> names =
        new Vector<>(
            java.util.List.of(
                "Java",
                "Spring",
                "Java"
            )
        );

    System.out.println(
        names.lastIndexOf("Java")
    );

Output:

    2

---

## containsAll()

    boolean result =
        names.containsAll(
            java.util.List.of(
                "Java",
                "Spring"
            )
        );

---

# 📏 12. Capacity vs Size

This is one of the most important Vector concepts.

### Size

Number of actual elements currently stored.

Example:

    Vector<Integer> numbers =
        new Vector<>(10);

    numbers.add(10);
    numbers.add(20);

Then:

    size = 2

### Capacity

Amount of storage currently available internally.

Initial:

    capacity = 10

So:

    size = 2
    capacity = 10

The remaining capacity is:

    8

Conceptually:

    Capacity = 10

    [10][20][ ][ ][ ][ ][ ][ ][ ][ ]
     ^
     |
    size = 2

---

# 📦 13. Vector Capacity Methods

## capacity()

Returns current capacity.

    Vector<Integer> numbers =
        new Vector<>(10);

    System.out.println(
        numbers.capacity()
    );

Output:

    10

---

## size()

Returns number of actual elements.

    System.out.println(
        numbers.size()
    );

If two elements were added:

    2

---

## ensureCapacity()

Ensures the Vector has at least the requested capacity.

    numbers.ensureCapacity(100);

---

## trimToSize()

Reduces capacity to the current size.

    numbers.trimToSize();

Example:

    size = 5
    capacity = 20

After:

    size = 5
    capacity = 5

This can reduce unused internal capacity.

---

## setSize()

Vector-specific method:

    numbers.setSize(10);

If the new size is larger than the current size, new positions are filled with `null`.

Example:

    Vector<String> names =
        new Vector<>();

    names.add("Java");
    names.setSize(3);

    System.out.println(names);

Output:

    [Java, null, null]

If the new size is smaller, elements at the end are removed.

---

# ⚙️ 14. Internal Working

Vector internally uses a growable array.

Conceptually:

    Vector
       |
       v
    Object[]
       |
       v

    [A][B][C][D][ ][ ][ ][ ]

When the array becomes full:

    Old Array
    [A][B][C][D]

        |
        | grow
        v

    New Array
    [A][B][C][D][ ][ ][ ][ ]

Existing elements are copied into the new array.

Therefore:

    get(index)
        -> O(1)

because it uses array indexing.

But insertion/removal in the middle can require shifting elements.

Therefore:

    add(index, element)
        -> O(n)

    remove(index)
        -> O(n)

---

# 📈 15. How Vector Grows

Vector has a concept of:

    capacityIncrement

If a positive capacity increment is specified, the Vector can grow by that amount when necessary.

Example:

    Vector<Integer> numbers =
        new Vector<>(5, 3);

Initial:

    capacity = 5

When more space is required, the capacity can increase by the configured increment.

Conceptually:

    5
    |
    +--- 3
    |
    8
    |
    +--- 3
    |
    11

When no positive capacity increment is specified, the Vector's growth strategy can increase capacity substantially, historically doubling it.

For interview purposes:

> Vector supports configurable capacity growth through `capacityIncrement`; when no positive increment is configured, its legacy growth behavior can approximately double capacity.

---

# 🏗️ 16. Vector Constructors

Vector provides four commonly documented constructors.

## 1. Default

    Vector()

Creates an empty Vector with its default initial capacity.

---

## 2. Initial Capacity

    Vector(int initialCapacity)

Example:

    Vector<Integer> numbers =
        new Vector<>(20);

---

## 3. Initial Capacity + Increment

    Vector(
        int initialCapacity,
        int capacityIncrement
    )

Example:

    Vector<Integer> numbers =
        new Vector<>(10, 5);

---

## 4. Collection Constructor

    Vector(
        Collection<? extends E> c
    )

Example:

    Vector<String> names =
        new Vector<>(
            java.util.List.of(
                "Java",
                "Spring"
            )
        );

---

# 🔢 17. Enumeration

Vector is a legacy class and supports:

    Enumeration

Example:

    Vector<String> names =
        new Vector<>();

    names.add("Java");
    names.add("Spring");
    names.add("React");

    Enumeration<String> enumeration =
        names.elements();

    while (enumeration.hasMoreElements()) {

        System.out.println(
            enumeration.nextElement()
        );
    }

Output:

    Java
    Spring
    React

`Enumeration` predates `Iterator`.

Modern Java code generally prefers:

    Iterator

or:

    for-each

---

# 🔄 18. Iterator and ListIterator

Vector supports modern Collection traversal mechanisms.

## Iterator

    Vector<String> names =
        new Vector<>(
            java.util.List.of(
                "Java",
                "Spring",
                "React"
            )
        );

    Iterator<String> iterator =
        names.iterator();

    while (iterator.hasNext()) {

        System.out.println(
            iterator.next()
        );
    }

---

## ListIterator

    ListIterator<String> iterator =
        names.listIterator();

    while (iterator.hasNext()) {

        System.out.println(
            iterator.next()
        );
    }

Backward:

    ListIterator<String> iterator =
        names.listIterator(names.size());

    while (iterator.hasPrevious()) {

        System.out.println(
            iterator.previous()
        );
    }

---

# 🔒 19. Thread Safety

One of Vector's major historical features is synchronization.

Many of its public methods are synchronized.

Example conceptually:

    public synchronized boolean add(E e)

The exact implementation details belong to the JDK version, but the important interview concept is:

> Vector synchronizes individual legacy operations.

This means two threads cannot simultaneously execute the same synchronized Vector method on the same Vector object.

However, this does NOT mean every sequence of operations is automatically thread-safe.

Example:

    if (!vector.contains(value)) {
        vector.add(value);
    }

These are two separate operations.

Another thread can change the Vector between them.

Therefore:

    contains()
        +
    add()

is not automatically one atomic operation.

---

# 🔐 20. Vector and Synchronization

Suppose:

    Vector<Integer> numbers =
        new Vector<>();

Two threads:

    Thread A
    Thread B

Both call:

    numbers.add(...)

The individual `add()` operation is synchronized.

Conceptually:

    Thread A
        |
        v
    [ synchronized add ]
        |
        v
    Vector

    Thread B
        |
        | waits
        v
    [ synchronized add ]

This provides synchronization at the method-operation level.

But synchronization has a cost.

Every synchronized method call can introduce locking overhead.

Therefore, using Vector simply because it is synchronized is not generally the modern recommendation.

---

# 🆚 21. Vector vs ArrayList

This is one of the most important interview comparisons.

| Feature | ArrayList | Vector |
|---|---|---|
| Internal structure | Dynamic array | Dynamic array |
| Random access | O(1) | O(1) |
| Implements List | Yes | Yes |
| Implements RandomAccess | Yes | Yes |
| Allows duplicates | Yes | Yes |
| Allows null | Yes | Yes |
| Synchronized methods | No | Yes |
| Legacy | No | Yes |
| Performance | Generally better for single-threaded use | Synchronization overhead |
| Growth control | Automatic | Supports capacity increment |
| Modern preference | Common | Usually not preferred |

### Key Difference

Both are:

    Array-based List implementations

But:

    ArrayList
        -> not synchronized by default

    Vector
        -> synchronized legacy class

---

# 🆚 22. Vector vs LinkedList

| Feature | Vector | LinkedList |
|---|---|---|
| Internal structure | Dynamic array | Doubly linked nodes |
| `get(index)` | O(1) | O(n) |
| Implements RandomAccess | Yes | No |
| Implements Deque | No | Yes |
| End insertion | O(1) amortized | O(1) |
| Middle insertion | O(n) | O(n) to locate position |
| Memory overhead | Lower than LinkedList | Higher |
| Synchronized | Yes | No |
| Legacy | Yes | No |
| Indexed access | Efficient | Inefficient |

---

# 🆚 23. Vector vs Array

| Feature | Array | Vector |
|---|---|---|
| Size | Fixed | Dynamic |
| Index access | O(1) | O(1) |
| Automatic growth | No | Yes |
| Collection API | No | Yes |
| `add()` | No | Yes |
| `remove()` | No | Yes |
| Synchronization | No | Yes |
| Generics | Arrays support type syntax | Yes |
| Legacy collection | No | Yes |

Example array:

    int[] numbers =
        new int[5];

Capacity is fixed at:

    5

Vector:

    Vector<Integer> numbers =
        new Vector<>();

can dynamically grow.

---

# 📚 24. Vector and Stack

A historically important relationship:

    Stack extends Vector

Hierarchy:

    Object
      |
    Vector
      |
    Stack

`Stack` is therefore a subclass of Vector.

Example:

    Stack<Integer> stack =
        new Stack<>();

    stack.push(10);
    stack.push(20);
    stack.push(30);

    System.out.println(
        stack.pop()
    );

Output:

    30

However:

> `Stack` is also a legacy class.

Modern Java code generally prefers:

    Deque

with an implementation such as:

    ArrayDeque

for stack behavior.

Example:

    Deque<Integer> stack =
        new ArrayDeque<>();

    stack.push(10);
    stack.push(20);

    stack.pop();

This is generally the preferred modern approach when applicable.

---

# ⚡ 25. Performance

Because Vector is array-based:

    get(index)
        -> O(1)

because:

    array[index]

provides direct indexed access.

However:

    add(index, element)
        -> O(n)

because elements may need to shift.

Example:

    [A][B][C][D]

Insert X at index 1:

    [A][X][B][C][D]

Elements:

    B
    C
    D

may need to move.

Similarly:

    remove(index)

can require shifting elements left.

---

# 👍 26. Advantages

## 1. Dynamic Size

Vector automatically grows when required.

---

## 2. Fast Indexed Access

Because it is array-based:

    get(index)
        -> O(1)

---

## 3. Built-In Synchronization

Its legacy methods are synchronized.

---

## 4. Supports List API

It works with modern Java Collection Framework APIs.

---

## 5. Supports Legacy Enumeration

Useful when working with older Java code.

---

## 6. Capacity Control

Vector exposes methods such as:

    capacity()
    ensureCapacity()
    trimToSize()
    setSize()

---

# 👎 27. Disadvantages

## 1. Legacy Class

Vector predates the Java Collections Framework.

---

## 2. Synchronization Overhead

Its synchronized methods can introduce unnecessary overhead when synchronization is not required.

---

## 3. Coarse-Grained Synchronization

Method-level synchronization does not automatically make compound operations atomic.

---

## 4. Usually Not Preferred for New Code

For normal List usage:

    ArrayList

is generally preferred.

For concurrency:

    java.util.concurrent

provides more specialized options.

---

## 5. Middle Insertion Is Expensive

Because elements may need to be shifted.

---

## 6. Not a Deque

Unlike LinkedList:

    Vector
        -> List

but not:

    Deque

---

# 🚨 28. Common Mistakes

## Mistake 1 — Thinking Vector Is a Linked List

Wrong.

Vector uses a:

    growable array

---

## Mistake 2 — Thinking Vector Is the Same as LinkedList

Wrong.

Vector:

    array-based

LinkedList:

    node-based

---

## Mistake 3 — Thinking Vector Is Completely Thread-Safe

Not exactly.

Individual synchronized methods are protected, but compound sequences still require external coordination when atomicity is needed.

---

## Mistake 4 — Thinking Synchronization Makes Vector Faster

Synchronization generally adds overhead.

It exists for safety around individual operations, not as a performance optimization.

---

## Mistake 5 — Thinking Vector Has O(n) get()

Wrong.

Vector uses an array and supports:

    O(1)

indexed access.

---

## Mistake 6 — Confusing Size and Capacity

Example:

    capacity = 10
    size = 3

There are:

    3 actual elements

but storage capacity for:

    10 elements

---

## Mistake 7 — Using Legacy Methods Everywhere

Methods such as:

    addElement()
    elementAt()
    removeElement()
    firstElement()

exist for compatibility.

Modern List APIs generally use:

    add()
    get()
    remove()
    getFirst()

where applicable.

---

## Mistake 8 — Assuming Vector Is Recommended for All Multithreaded Programs

No.

Modern concurrent applications often need more specialized concurrency mechanisms depending on the requirement.

---

# 🎯 29. Interview Traps

## Trap 1

Question:

> Is Vector synchronized?

Answer:

Yes. Its legacy public methods are synchronized.

---

## Trap 2

Question:

> Is Vector thread-safe?

Better answer:

Individual synchronized operations are protected, but compound operations are not automatically atomic.

---

## Trap 3

Question:

> Is Vector legacy?

Yes.

It existed before the Java Collections Framework.

---

## Trap 4

Question:

> Does Vector implement List?

Yes.

---

## Trap 5

Question:

> Does Vector implement RandomAccess?

Yes.

---

## Trap 6

Question:

> What is the internal data structure of Vector?

A dynamically growing array.

---

## Trap 7

Question:

> What is the time complexity of get(index)?

O(1).

---

## Trap 8

Question:

> What is the time complexity of add(index, element)?

Typically O(n), because elements may need to be shifted.

---

## Trap 9

Question:

> What is the difference between size and capacity?

Size:

    Number of actual elements.

Capacity:

    Amount of internal storage currently available.

---

## Trap 10

Question:

> What is capacityIncrement?

It controls the amount by which capacity can grow when a positive increment is configured.

---

## Trap 11

Question:

> Can Vector contain duplicates?

Yes.

---

## Trap 12

Question:

> Can Vector contain null?

Yes.

---

## Trap 13

Question:

> Does Vector preserve insertion order?

Yes.

---

## Trap 14

Question:

> What is Enumeration?

A legacy traversal mechanism that predates Iterator.

Vector provides:

    elements()

to obtain an Enumeration.

---

## Trap 15

Question:

> Is ArrayList synchronized?

No, not by default.

This is one of the major differences from Vector.

---

## Trap 16

Question:

> Is Vector faster than ArrayList?

Not generally.

Vector's synchronization can introduce overhead.

---

## Trap 17

Question:

> What class extends Vector?

Historically:

    Stack

---

## Trap 18

Question:

> Should we use Vector for implementing a stack?

Usually no for new code.

Prefer:

    Deque

with:

    ArrayDeque

when appropriate.

---

## Trap 19

Question:

> Does Vector implement Deque?

No.

---

## Trap 20

Question:

> Can Vector grow automatically?

Yes.

It is a dynamically resizable array.

---

# 🧠 30. DSA Relevance

Vector itself is not usually the main focus of modern DSA.

However, understanding Vector helps reinforce:

    Dynamic arrays
    Array resizing
    Capacity
    Amortized analysis
    Indexed access
    Element shifting
    Thread synchronization

The underlying concept is similar to:

    Dynamic Array

which is extremely important in DSA.

---

# 💡 31. Practical Examples

## Example 1 — Basic Vector

    import java.util.Vector;

    public class Main {

        public static void main(String[] args) {

            Vector<String> languages =
                new Vector<>();

            languages.add("Java");
            languages.add("C");
            languages.add("JavaScript");

            System.out.println(languages);
        }
    }

Output:

    [Java, C, JavaScript]

---

## Example 2 — Capacity and Size

    import java.util.Vector;

    public class Main {

        public static void main(String[] args) {

            Vector<Integer> numbers =
                new Vector<>(10);

            numbers.add(10);
            numbers.add(20);
            numbers.add(30);

            System.out.println(
                "Size = " + numbers.size()
            );

            System.out.println(
                "Capacity = " + numbers.capacity()
            );
        }
    }

Output:

    Size = 3
    Capacity = 10

---

## Example 3 — Insertion

    import java.util.Vector;

    public class Main {

        public static void main(String[] args) {

            Vector<String> names =
                new Vector<>();

            names.add("Java");
            names.add("React");

            names.add(1, "Spring");

            System.out.println(names);
        }
    }

Output:

    [Java, Spring, React]

---

## Example 4 — Removal

    import java.util.Vector;

    public class Main {

        public static void main(String[] args) {

            Vector<Integer> numbers =
                new Vector<>();

            numbers.add(10);
            numbers.add(20);
            numbers.add(30);

            numbers.remove(1);

            System.out.println(numbers);
        }
    }

Output:

    [10, 30]

---

## Example 5 — Enumeration

    import java.util.*;

    public class Main {

        public static void main(String[] args) {

            Vector<String> names =
                new Vector<>();

            names.add("Java");
            names.add("Spring");
            names.add("React");

            Enumeration<String> e =
                names.elements();

            while (e.hasMoreElements()) {

                System.out.println(
                    e.nextElement()
                );
            }
        }
    }

Output:

    Java
    Spring
    React

---

## Example 6 — Iterator

    import java.util.*;

    public class Main {

        public static void main(String[] args) {

            Vector<String> names =
                new Vector<>(
                    List.of(
                        "Java",
                        "Spring",
                        "React"
                    )
                );

            Iterator<String> iterator =
                names.iterator();

            while (iterator.hasNext()) {

                System.out.println(
                    iterator.next()
                );
            }
        }
    }

---

## Example 7 — Capacity Increment

    import java.util.Vector;

    public class Main {

        public static void main(String[] args) {

            Vector<Integer> numbers =
                new Vector<>(3, 2);

            numbers.add(10);
            numbers.add(20);
            numbers.add(30);

            System.out.println(
                "Capacity = " +
                numbers.capacity()
            );

            numbers.add(40);

            System.out.println(
                "Capacity = " +
                numbers.capacity()
            );
        }
    }

The exact capacity behavior should be understood from the configured growth policy rather than assuming every JDK version behaves identically in every edge case.

---

## Example 8 — trimToSize()

    import java.util.Vector;

    public class Main {

        public static void main(String[] args) {

            Vector<Integer> numbers =
                new Vector<>(10);

            numbers.add(10);
            numbers.add(20);
            numbers.add(30);

            System.out.println(
                "Before = " +
                numbers.capacity()
            );

            numbers.trimToSize();

            System.out.println(
                "After = " +
                numbers.capacity()
            );
        }
    }

Output:

    Before = 10
    After = 3

---

## Example 9 — ensureCapacity()

    import java.util.Vector;

    public class Main {

        public static void main(String[] args) {

            Vector<Integer> numbers =
                new Vector<>();

            numbers.ensureCapacity(100);

            System.out.println(
                numbers.capacity()
            );
        }
    }

---

## Example 10 — Vector as a List

    import java.util.*;

    public class Main {

        public static void main(String[] args) {

            List<String> names =
                new Vector<>();

            names.add("Java");
            names.add("Spring");

            System.out.println(
                names.get(0)
            );
        }
    }

Important:

The reference type determines which methods are directly available.

Here:

    List<String>

means only List-visible methods are available through the reference.

---

# 💼 32. Top Interview Questions

## Q1. What is Vector?

Vector is a legacy synchronized, dynamically resizable array implementation of the List interface.

---

## Q2. When was Vector introduced?

Vector was introduced in Java 1.0.

---

## Q3. Is Vector part of the Java Collections Framework?

It predates the Java Collections Framework but was later integrated into it by implementing the List interface.

---

## Q4. What is the internal data structure of Vector?

A dynamically growing array.

---

## Q5. Is Vector synchronized?

Yes. Its legacy public methods are synchronized.

---

## Q6. Is Vector thread-safe?

Individual method calls are synchronized, but compound operations are not automatically atomic.

---

## Q7. What is the time complexity of get(index)?

O(1).

---

## Q8. Why is get(index) O(1)?

Because Vector uses an array internally, allowing direct indexed access.

---

## Q9. What is the time complexity of insertion in the middle?

Typically O(n), because elements may need to be shifted.

---

## Q10. What is the difference between size and capacity?

Size is the number of elements currently stored.

Capacity is the amount of internal storage currently allocated.

---

## Q11. What is capacityIncrement?

It specifies a growth increment used when the Vector needs more capacity.

---

## Q12. What is the difference between Vector and ArrayList?

Both use dynamically growing arrays, but Vector synchronizes its legacy methods while ArrayList does not.

---

## Q13. Which is generally preferred for normal List usage?

ArrayList is generally preferred in modern code.

---

## Q14. Does Vector allow duplicates?

Yes.

---

## Q15. Does Vector allow null?

Yes.

---

## Q16. Does Vector preserve insertion order?

Yes.

---

## Q17. Does Vector implement RandomAccess?

Yes.

---

## Q18. Does Vector implement Deque?

No.

---

## Q19. What is Enumeration?

Enumeration is a legacy traversal interface that predates Iterator.

---

## Q20. How do you get Enumeration from Vector?

Using:

    elements()

---

## Q21. What is Stack's relationship with Vector?

`Stack` extends `Vector`.

---

## Q22. Should Stack be used in modern Java?

Usually no. `Deque` is generally preferred for stack behavior.

---

## Q23. What are some legacy Vector methods?

Examples:

    addElement()
    elementAt()
    firstElement()
    lastElement()
    removeElement()
    removeElementAt()
    removeAllElements()

---

## Q24. What are modern alternatives to these methods?

Examples:

    add()
    get()
    getFirst()
    getLast()
    remove()
    remove(index)
    clear()

---

## Q25. Why is Vector considered legacy?

It was designed before the Java Collections Framework and uses broad built-in synchronization that is often unnecessary for modern applications.

---

## Q26. Does synchronized mean Vector is always safe for multithreaded programs?

No.

Compound operations can still require external synchronization or a different concurrency design.

---

## Q27. Can Vector grow dynamically?

Yes.

---

## Q28. Can we control Vector's capacity?

Yes.

Methods include:

    capacity()
    ensureCapacity()
    trimToSize()
    setSize()

---

## Q29. What happens when Vector reaches capacity?

It grows its internal array according to its configured growth policy.

---

## Q30. Why might ArrayList be preferred over Vector?

ArrayList avoids Vector's built-in synchronization overhead for ordinary non-concurrent List use.

---

# ⏱️ 33. 30-Second Interview Answer

If the interviewer asks:

> "What is Vector?"

Answer:

> "`Vector` is a legacy class from `java.util` that implements the `List` interface using a dynamically growing array. It provides O(1) indexed access because it implements `RandomAccess`. The main difference from ArrayList is that Vector's legacy methods are synchronized, which provides synchronization at the individual method level but can introduce unnecessary overhead. Vector also provides capacity-management methods such as `capacity()`, `ensureCapacity()`, and `trimToSize()`. For most modern non-concurrent List use cases, ArrayList is generally preferred."

---

# 📌 34. Cheat Sheet

## Basic

    Vector
        -> java.util
        -> Class
        -> Java 1.0
        -> Legacy
        -> Dynamic array
        -> Synchronized
        -> Ordered
        -> Allows duplicates
        -> Allows null
        -> RandomAccess

---

## Hierarchy

    Iterable
        |
    Collection
        |
    List
        |
    Vector

Interfaces:

    RandomAccess
    Cloneable
    Serializable

---

## Common Methods

    add()
    add(index, element)
    addAll()

    get()
    set()

    remove()
    clear()

    contains()
    indexOf()
    lastIndexOf()

    size()
    isEmpty()

---

## Vector-Specific / Legacy Methods

    addElement()
    elementAt()
    firstElement()
    lastElement()

    removeElement()
    removeElementAt()
    removeAllElements()

    elements()

    capacity()
    ensureCapacity()
    trimToSize()
    setSize()

---

## Complexity

    get(index)
        -> O(1)

    set(index)
        -> O(1)

    add(element)
        -> O(1) amortized

    add(index, element)
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

# ⚡ 35. Quick Revision

Remember:

    Vector = Legacy + Synchronized + Dynamic Array

Mental model:

    Vector
       |
       v
    Object[]
       |
       v

    [A][B][C][D][ ][ ][ ][ ]

Key facts:

    1. Vector was introduced in Java 1.0.

    2. It predates the Java Collections Framework.

    3. It was later integrated into the framework through List.

    4. Vector uses a dynamically growing array.

    5. Vector implements List.

    6. Vector implements RandomAccess.

    7. Vector is synchronized at the individual method level.

    8. Vector allows duplicates.

    9. Vector allows null.

    10. Vector maintains insertion order.

    11. get(index) is O(1).

    12. Middle insertion is typically O(n).

    13. Middle removal is typically O(n).

    14. Vector has both size and capacity.

    15. capacity() returns internal capacity.

    16. size() returns actual element count.

    17. ensureCapacity() can increase available capacity.

    18. trimToSize() can reduce capacity to the current size.

    19. Enumeration is a legacy traversal mechanism.

    20. Stack extends Vector.

    21. ArrayList and Vector are both dynamic-array Lists.

    22. ArrayList is generally preferred for normal modern List usage.

    23. Vector does not implement Deque.

    24. Synchronization does not automatically make compound operations atomic.

---

# 🎯 Interview Must-Know

Before moving to `08-Stack.md`, make sure you can explain:

    1. What is Vector?
    2. Why is Vector called a legacy class?
    3. When was Vector introduced?
    4. What is Vector's internal data structure?
    5. Is Vector synchronized?
    6. What does synchronized mean in Vector?
    7. Does synchronization make every compound operation atomic?
    8. What is RandomAccess?
    9. Why is get(index) O(1)?
    10. What is the difference between size and capacity?
    11. What is capacityIncrement?
    12. How does Vector grow?
    13. What is Enumeration?
    14. What is the difference between Enumeration and Iterator?
    15. What is Vector vs ArrayList?
    16. What is Vector vs LinkedList?
    17. What is Stack's relationship with Vector?
    18. Why is Stack generally not preferred for new code?
    19. What is ensureCapacity()?
    20. What is trimToSize()?

---

# 🧠 Final Memory Trick

Think:

    ArrayList
        |
        | same basic array idea
        |
      Vector
        |
        +--> Legacy
        +--> Synchronized
        +--> Dynamic Array
        +--> RandomAccess

The one-line interview memory:

> **Vector is a legacy synchronized dynamic-array implementation of List, providing O(1) indexed access but generally being less preferred than ArrayList for modern ordinary List usage because of its built-in synchronization.**