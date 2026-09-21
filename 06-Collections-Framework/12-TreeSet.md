# 12 — TreeSet 🌳

> **Package:** `java.util`  
> **Type:** Class  
> **Implements:** `NavigableSet<E>`, `SortedSet<E>`, `Set<E>`, `Cloneable`, `Serializable`  
> **Internal data structure:** Self-balancing Red-Black Tree  
> **Main property:** Unique elements + sorted order

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [What is TreeSet?](#2-what-is-treeset)
3. [TreeSet Hierarchy](#3-treeset-hierarchy)
4. [Why TreeSet Exists](#4-why-treeset-exists)
5. [Key Characteristics](#5-key-characteristics)
6. [Creating a TreeSet](#6-creating-a-treeset)
7. [Natural Ordering](#7-natural-ordering)
8. [Custom Ordering with Comparator](#8-custom-ordering-with-comparator)
9. [Internal Working](#9-internal-working)
10. [Red-Black Tree](#10-red-black-tree)
11. [How TreeSet Detects Duplicates](#11-how-treeset-detects-duplicates)
12. [Comparable and Comparator](#12-comparable-and-comparator)
13. [TreeSet Methods](#13-treeset-methods)
14. [add()](#14-add)
15. [remove()](#15-remove)
16. [contains()](#16-contains)
17. [first() and last()](#17-first-and-last)
18. [lower()](#18-lower)
19. [floor()](#19-floor)
20. [ceiling()](#20-ceiling)
21. [higher()](#21-higher)
22. [pollFirst() and pollLast()](#22-pollfirst-and-polllast)
23. [descendingSet()](#23-descendingset)
24. [descendingIterator()](#24-descendingiterator)
25. [headSet()](#25-headset)
26. [tailSet()](#26-tailset)
27. [subSet()](#27-subset)
28. [size(), isEmpty(), clear()](#28-size-isempty-clear)
29. [iterator()](#29-iterator)
30. [forEach()](#30-foreach)
31. [null in TreeSet](#31-null-in-treeset)
32. [TreeSet with Custom Objects](#32-treeset-with-custom-objects)
33. [Mutable Object Trap](#33-mutable-object-trap)
34. [Time Complexity](#34-time-complexity)
35. [Memory Usage](#35-memory-usage)
36. [TreeSet vs HashSet](#36-treeset-vs-hashset)
37. [TreeSet vs LinkedHashSet](#37-treeset-vs-linkedhashset)
38. [TreeSet vs ArrayList](#38-treeset-vs-arraylist)
39. [TreeSet vs PriorityQueue](#39-treeset-vs-priorityqueue)
40. [When to Use TreeSet](#40-when-to-use-treeset)
41. [When Not to Use TreeSet](#41-when-not-to-use-treeset)
42. [Advantages](#42-advantages)
43. [Disadvantages](#43-disadvantages)
44. [Common Mistakes](#44-common-mistakes)
45. [Interview Traps](#45-interview-traps)
46. [Practical Examples](#46-practical-examples)
47. [DSA Patterns](#47-dsa-patterns)
48. [DSA Problem-Solving Examples](#48-dsa-problem-solving-examples)
49. [Top Interview Questions](#49-top-interview-questions)
50. [30-Second Interview Answer](#50-30-second-interview-answer)
51. [Cheat Sheet](#51-cheat-sheet)
52. [Quick Revision](#52-quick-revision)

---

# 1. Introduction 🌱

`TreeSet` is a class from the `java.util` package that implements the `NavigableSet` interface.

It stores:

    Unique elements
        +
    Sorted elements
        +
    Efficient navigation

Example:

    Set<Integer> numbers =
        new TreeSet<>();

    numbers.add(50);
    numbers.add(10);
    numbers.add(30);
    numbers.add(20);

Output:

    [10, 20, 30, 50]

Even though the elements were inserted as:

    50
    10
    30
    20

TreeSet automatically maintains them in sorted order.

---

# 2. What is TreeSet? 🌳

`TreeSet` is a sorted implementation of the `Set` interface.

Its most important characteristics are:

    No duplicates
    Sorted order
    Efficient search
    Efficient insertion
    Efficient removal
    Navigation operations

The underlying implementation is based on a self-balancing tree.

Conceptually:

    TreeSet
       |
       v
    NavigableSet
       |
       v
    SortedSet
       |
       v
    Set
       |
       v
    Collection

The implementation uses a Red-Black Tree through the underlying `TreeMap` implementation.

---

# 3. TreeSet Hierarchy 🌲

The interface hierarchy is:

    Iterable
       |
    Collection
       |
    Set
       |
    SortedSet
       |
    NavigableSet

`TreeSet` implements:

    NavigableSet
    Cloneable
    Serializable

Conceptually:

    Object
       |
    AbstractCollection
       |
    AbstractSet
       |
    TreeSet

And:

    TreeSet
       |
       +---- NavigableSet
                |
             SortedSet
                |
               Set

Important:

> `TreeSet` is a concrete class, while `SortedSet` and `NavigableSet` are interfaces.

---

# 4. Why TreeSet Exists 🎯

Suppose you need:

    Unique elements
        +
    Sorted order
        +
    Nearest-element operations

A `HashSet` provides uniqueness but not sorted order.

A `LinkedHashSet` provides uniqueness and insertion order.

A `TreeSet` provides uniqueness and sorted order.

Example:

    TreeSet<Integer> numbers =
        new TreeSet<>();

    numbers.add(40);
    numbers.add(10);
    numbers.add(30);
    numbers.add(20);

Result:

    [10, 20, 30, 40]

But TreeSet provides more than sorting.

You can ask:

    What is the smallest element >= x?

    What is the largest element <= x?

    What is the smallest element > x?

    What is the largest element < x?

These operations are provided by `NavigableSet`.

---

# 5. Key Characteristics 📌

## 5.1 Unique Elements

Duplicates are not allowed.

    TreeSet<Integer> set =
        new TreeSet<>();

    set.add(10);
    set.add(20);
    set.add(10);

Result:

    [10, 20]

---

## 5.2 Sorted Order

Elements are sorted according to:

    Natural ordering

or:

    Comparator

---

## 5.3 No Index-Based Access

TreeSet does not support:

    get(index)

because it is not an indexed data structure.

---

## 5.4 Navigation Operations

TreeSet provides:

    lower()
    floor()
    ceiling()
    higher()

These are extremely useful in DSA.

---

## 5.5 Logarithmic Basic Operations

Typical complexity:

    add()
        -> O(log n)

    remove()
        -> O(log n)

    contains()
        -> O(log n)

---

## 5.6 No Duplicate Values According to Ordering

This point is extremely important.

TreeSet determines uniqueness based on its ordering mechanism.

If comparison returns:

    0

TreeSet treats the elements as equivalent for Set purposes.

---

# 6. Creating a TreeSet 💻

## Basic

    TreeSet<Integer> numbers =
        new TreeSet<>();

---

## Using Set Reference

    Set<Integer> numbers =
        new TreeSet<>();

---

## Using NavigableSet Reference

    NavigableSet<Integer> numbers =
        new TreeSet<>();

---

## From Another Collection

    List<Integer> numbers =
        List.of(
            50,
            20,
            40,
            10
        );

    Set<Integer> sorted =
        new TreeSet<>(numbers);

Result:

    [10, 20, 40, 50]

---

## With Comparator

    TreeSet<Integer> numbers =
        new TreeSet<>(
            Comparator.reverseOrder()
        );

Result:

    [50, 40, 20, 10]

---

# 7. Natural Ordering 🔢

Natural ordering means elements know how they should normally be compared.

For example:

    Integer
        -> ascending numeric order

    String
        -> lexicographical order

Example:

    TreeSet<Integer> numbers =
        new TreeSet<>();

    numbers.add(30);
    numbers.add(10);
    numbers.add(20);

Output:

    [10, 20, 30]

---

## String Example

    TreeSet<String> names =
        new TreeSet<>();

    names.add("Zebra");
    names.add("Apple");
    names.add("Mango");

Output:

    [Apple, Mango, Zebra]

---

# 8. Custom Ordering with Comparator 🔄

You can provide a `Comparator` when creating the TreeSet.

Example:

    TreeSet<Integer> numbers =
        new TreeSet<>(
            Comparator.reverseOrder()
        );

    numbers.add(10);
    numbers.add(30);
    numbers.add(20);

Output:

    [30, 20, 10]

---

## Custom Comparator

    TreeSet<String> names =
        new TreeSet<>(
            (a, b) ->
                b.compareTo(a)
        );

    names.add("A");
    names.add("B");
    names.add("C");

Output:

    [C, B, A]

---

# 9. Internal Working ⚙️

TreeSet is implemented using a tree-based structure.

In the standard Java implementation, TreeSet is backed by a `TreeMap`.

Conceptually:

    TreeSet<E>
        |
        v
    TreeMap<E, Object>
        |
        v
    Red-Black Tree

The actual TreeSet stores elements as keys in the underlying TreeMap.

Conceptually:

    element
       |
       v
    TreeMap key
       |
       v
    Red-Black Tree

The value is an internal dummy object.

---

## Why TreeMap?

`TreeMap` already provides:

    Sorted keys
    O(log n) lookup
    O(log n) insertion
    O(log n) removal
    Navigation

TreeSet can therefore reuse this functionality to implement a sorted Set.

---

# 10. Red-Black Tree 🔴⚫

A Red-Black Tree is a self-balancing Binary Search Tree.

The tree maintains balancing rules so that its height remains logarithmic.

Conceptually:

              20
             /  \
           10    30
          / \    / \
         5  15  25  40

Because the tree remains balanced, operations can remain approximately:

    O(log n)

---

## Why Balance Matters

Imagine a normal Binary Search Tree receiving:

    10
    20
    30
    40
    50

It could become:

    10
      \
       20
         \
          30
            \
             40
               \
                50

This behaves almost like a linked list.

Search can become:

    O(n)

A self-balancing tree avoids this situation.

TreeSet uses a balanced tree structure so its operations remain logarithmic.

---

# 11. How TreeSet Detects Duplicates 🔍

This is one of the most important TreeSet concepts.

Suppose:

    TreeSet<Integer> set =
        new TreeSet<>();

    set.add(20);

Now:

    set.add(20);

The tree compares the new value against existing values.

Conceptually:

    new element
         |
         v
    compareTo()
         |
         v
    result
      / | \
    <0  0  >0
     |  |   |
    left | right
         |
      duplicate

If comparison returns:

    0

TreeSet considers the element equivalent to an existing element and does not add it.

---

# 12. Comparable and Comparator 🔗

TreeSet needs a way to compare elements.

There are two major mechanisms:

    Comparable
        -> Natural ordering

    Comparator
        -> External/custom ordering

---

## Comparable

Example:

    class Student
        implements Comparable<Student>

Then:

    compareTo()

defines natural ordering.

---

## Comparator

Example:

    Comparator<Student> comparator =
        (a, b) ->
            a.name.compareTo(b.name);

    TreeSet<Student> students =
        new TreeSet<>(comparator);

The Comparator controls ordering.

---

## Important Rule

If TreeSet uses:

    compareTo()
    
or:

    compare()

and it returns:

    0

the elements are treated as duplicates from TreeSet's perspective.

This means:

> TreeSet's uniqueness is based on ordering, not necessarily on `equals()`.

---

# 13. TreeSet Methods 🛠️

TreeSet provides methods from:

    Set
    SortedSet
    NavigableSet

Important methods:

    add()
    remove()
    contains()

    first()
    last()

    lower()
    floor()
    ceiling()
    higher()

    pollFirst()
    pollLast()

    descendingSet()
    descendingIterator()

    headSet()
    tailSet()
    subSet()

    size()
    isEmpty()
    clear()

    iterator()
    forEach()

---

# 14. add() ➕

Adds an element while maintaining sorted order.

    TreeSet<Integer> numbers =
        new TreeSet<>();

    numbers.add(30);
    numbers.add(10);
    numbers.add(20);

    System.out.println(numbers);

Output:

    [10, 20, 30]

Return value:

    true
        -> newly added

    false
        -> already present according to ordering

---

# 15. remove() ➖

Removes an element.

    TreeSet<Integer> numbers =
        new TreeSet<>(
            List.of(
                10,
                20,
                30
            )
        );

    numbers.remove(20);

Output:

    [10, 30]

Typical complexity:

    O(log n)

---

# 16. contains() 🔎

Checks whether an element exists.

    TreeSet<Integer> numbers =
        new TreeSet<>(
            List.of(
                10,
                20,
                30
            )
        );

    System.out.println(
        numbers.contains(20)
    );

Output:

    true

Typical complexity:

    O(log n)

---

# 17. first() and last() 🥇

## first()

Returns the smallest element.

    TreeSet<Integer> numbers =
        new TreeSet<>(
            List.of(
                50,
                10,
                30
            )
        );

    System.out.println(
        numbers.first()
    );

Output:

    10

---

## last()

Returns the largest element.

    System.out.println(
        numbers.last()
    );

Output:

    50

---

## Complexity

    first()
        -> O(log n) or effectively constant in the tree implementation

    last()
        -> O(log n) or effectively constant in the tree implementation

For interview purposes, focus on their role:

    first()
        -> minimum

    last()
        -> maximum

---

# 18. lower() ⬇️

`lower(x)` returns the greatest element strictly less than `x`.

Example:

    TreeSet<Integer> set =
        new TreeSet<>(
            List.of(
                10,
                20,
                30,
                40
            )
        );

    System.out.println(
        set.lower(30)
    );

Output:

    20

Because:

    20 < 30

but:

    30 is excluded

---

# 19. floor() 🧱

`floor(x)` returns the greatest element less than or equal to `x`.

Example:

    TreeSet<Integer> set =
        new TreeSet<>(
            List.of(
                10,
                20,
                30,
                40
            )
        );

    System.out.println(
        set.floor(30)
    );

Output:

    30

If 30 did not exist:

    floor(25)

would return:

    20

---

# 20. ceiling() ⬆️

`ceiling(x)` returns the smallest element greater than or equal to `x`.

Example:

    TreeSet<Integer> set =
        new TreeSet<>(
            List.of(
                10,
                20,
                30,
                40
            )
        );

    System.out.println(
        set.ceiling(30)
    );

Output:

    30

If 30 did not exist:

    ceiling(25)

would return:

    30

---

# 21. higher() 🚀

`higher(x)` returns the smallest element strictly greater than `x`.

Example:

    TreeSet<Integer> set =
        new TreeSet<>(
            List.of(
                10,
                20,
                30,
                40
            )
        );

    System.out.println(
        set.higher(30)
    );

Output:

    40

Compare:

    lower(30)
        -> 20

    floor(30)
        -> 30

    ceiling(30)
        -> 30

    higher(30)
        -> 40

---

# 22. pollFirst() and pollLast() 🎯

## pollFirst()

Removes and returns the smallest element.

    TreeSet<Integer> set =
        new TreeSet<>(
            List.of(
                10,
                20,
                30
            )
        );

    int value =
        set.pollFirst();

    System.out.println(value);
    System.out.println(set);

Output:

    10
    [20, 30]

---

## pollLast()

Removes and returns the largest element.

    int value =
        set.pollLast();

Output:

    30

---

# 23. descendingSet() 🔄

Returns a reverse-order view of the Set.

    TreeSet<Integer> set =
        new TreeSet<>(
            List.of(
                10,
                20,
                30
            )
        );

    NavigableSet<Integer> reverse =
        set.descendingSet();

    System.out.println(reverse);

Output:

    [30, 20, 10]

Important:

> `descendingSet()` returns a view backed by the original set.

Changes made through the view can affect the original Set.

---

# 24. descendingIterator() 🔽

Iterates from largest to smallest.

    TreeSet<Integer> set =
        new TreeSet<>(
            List.of(
                10,
                20,
                30
            )
        );

    Iterator<Integer> iterator =
        set.descendingIterator();

    while (iterator.hasNext()) {

        System.out.println(
            iterator.next()
        );
    }

Output:

    30
    20
    10

---

# 25. headSet() ✂️

Returns a view containing elements before a boundary.

Example:

    TreeSet<Integer> set =
        new TreeSet<>(
            List.of(
                10,
                20,
                30,
                40,
                50
            )
        );

    SortedSet<Integer> result =
        set.headSet(30);

Result:

    [10, 20]

By default, `30` is excluded.

---

## Navigable Version

You can choose whether the boundary is inclusive.

    NavigableSet<Integer> result =
        set.headSet(
            30,
            true
        );

Result:

    [10, 20, 30]

---

# 26. tailSet() ✂️

Returns elements from a boundary onward.

    NavigableSet<Integer> result =
        set.tailSet(
            30,
            true
        );

Result:

    [30, 40, 50]

If:

    false

then:

    [40, 50]

---

# 27. subSet() ✂️

Returns a range of elements.

Example:

    TreeSet<Integer> set =
        new TreeSet<>(
            List.of(
                10,
                20,
                30,
                40,
                50
            )
        );

    NavigableSet<Integer> result =
        set.subSet(
            20,
            true,
            40,
            false
        );

Result:

    [20, 30]

Meaning:

    >= 20
        AND
    < 40

---

## Range Visualization

    10   20   30   40   50
         [---------)
         ^         ^
       inclusive  exclusive

---

# 28. size(), isEmpty(), clear() 🧹

## size()

    System.out.println(
        set.size()
    );

---

## isEmpty()

    System.out.println(
        set.isEmpty()
    );

---

## clear()

    set.clear();

After:

    []

---

# 29. iterator() 🔁

TreeSet's normal iterator follows ascending sorted order.

    TreeSet<Integer> set =
        new TreeSet<>(
            List.of(
                30,
                10,
                20
            )
        );

    Iterator<Integer> iterator =
        set.iterator();

    while (iterator.hasNext()) {

        System.out.println(
            iterator.next()
        );
    }

Output:

    10
    20
    30

---

# 30. forEach() 🔁

Example:

    TreeSet<String> languages =
        new TreeSet<>(
            List.of(
                "Java",
                "C",
                "Python"
            )
        );

    languages.forEach(
        language ->
            System.out.println(language)
    );

Output:

    C
    Java
    Python

---

# 31. null in TreeSet ⚠️

TreeSet generally does not allow `null` when using natural ordering.

Example:

    TreeSet<Integer> set =
        new TreeSet<>();

    set.add(null);

This results in:

    NullPointerException

Why?

Because TreeSet needs to compare elements.

For natural ordering, `null` cannot be compared with normal objects.

---

## Important Interview Point

Do not simply memorize:

    "TreeSet does not allow null."

The deeper reason is:

> Natural ordering requires comparison, and `null` cannot participate in normal `compareTo()` operations.

With a custom Comparator that explicitly handles null, behavior can differ.

Example:

    TreeSet<Integer> set =
        new TreeSet<>(
            Comparator.nullsFirst(
                Comparator.naturalOrder()
            )
        );

    set.add(null);
    set.add(10);
    set.add(20);

Result:

    [null, 10, 20]

So the precise statement is:

> Natural-order TreeSet does not support null elements, while a suitable Comparator can explicitly define null ordering.

---

# 32. TreeSet with Custom Objects 👨‍💻

A custom object must provide a comparison mechanism.

## Using Comparable

    import java.util.*;

    class Student
        implements Comparable<Student> {

        int id;
        String name;

        Student(
            int id,
            String name
        ) {
            this.id = id;
            this.name = name;
        }

        @Override
        public int compareTo(
            Student other
        ) {

            return Integer.compare(
                this.id,
                other.id
            );
        }

        @Override
        public String toString() {

            return id + " - " + name;
        }
    }

    public class Main {

        public static void main(
            String[] args
        ) {

            TreeSet<Student> students =
                new TreeSet<>();

            students.add(
                new Student(
                    103,
                    "Rahul"
                )
            );

            students.add(
                new Student(
                    101,
                    "Aman"
                )
            );

            students.add(
                new Student(
                    102,
                    "Vikas"
                )
            );

            for (Student student :
                 students) {

                System.out.println(student);
            }
        }
    }

Output:

    101 - Aman
    102 - Vikas
    103 - Rahul

---

# 33. Mutable Object Trap ⚠️

Suppose a TreeSet sorts objects based on:

    age

and the object's age changes after insertion.

Example conceptually:

    Student
        age = 20

inserted into TreeSet.

Later:

    age = 30

The object's position inside the tree is not automatically reorganized just because the field changed.

This can break the logical ordering assumptions of the collection.

Therefore:

> Avoid mutating fields that participate in comparison while objects are stored in a TreeSet.

A safer approach is:

    remove object
        |
        v
    modify object
        |
        v
    add object again

---

# 34. Time Complexity ⏱️

For a TreeSet backed by a balanced tree:

| Operation | Complexity |
|---|---:|
| add() | O(log n) |
| remove() | O(log n) |
| contains() | O(log n) |
| first() | O(log n) or effectively constant in implementation |
| last() | O(log n) or effectively constant in implementation |
| lower() | O(log n) |
| floor() | O(log n) |
| ceiling() | O(log n) |
| higher() | O(log n) |
| pollFirst() | O(log n) |
| pollLast() | O(log n) |
| iteration | O(n) |

The important DSA takeaway is:

> TreeSet gives ordered search operations in O(log n).

---

# 35. Memory Usage 💾

TreeSet requires more structural memory than a simple array-based collection.

Each tree node needs information such as:

    element
    left reference
    right reference
    parent/reference information
    color information

Therefore:

    TreeSet
        -> O(n) space

The key trade-off is:

    Extra memory
        in exchange for
    sorted + navigable operations

---

# 36. TreeSet vs HashSet ⚔️

| Feature | TreeSet | HashSet |
|---|---|---|
| Duplicates | No | No |
| Ordering | Sorted | No guaranteed order |
| Internal concept | Red-Black Tree | Hash table |
| add() | O(log n) | O(1) average |
| remove() | O(log n) | O(1) average |
| contains() | O(log n) | O(1) average |
| Navigation | Yes | No |
| Null | Natural ordering generally no | One allowed |
| Main use | Sorted unique data | Fast unique data |

Memory trick:

    HashSet
        -> Fast unique

    TreeSet
        -> Sorted unique

---

# 37. TreeSet vs LinkedHashSet ⚔️

| Feature | TreeSet | LinkedHashSet |
|---|---|---|
| Duplicates | No | No |
| Order | Sorted | Insertion |
| Internal structure | Red-Black Tree | Hash table + linked structure |
| add() | O(log n) | O(1) average |
| contains() | O(log n) | O(1) average |
| Navigation | Yes | No |
| Main use | Sorted data | Insertion-order data |

Remember:

    LinkedHashSet
        -> "What order was inserted?"

    TreeSet
        -> "What is the sorted order?"

---

# 38. TreeSet vs ArrayList ⚔️

| Feature | TreeSet | ArrayList |
|---|---|---|
| Duplicates | No | Yes |
| Index access | No | Yes |
| Sorted automatically | Yes | No |
| contains() | O(log n) | O(n) |
| Insert | O(log n) | O(1) amortized at end |
| Main purpose | Sorted unique data | Indexed sequence |

---

# 39. TreeSet vs PriorityQueue ⚔️

These are often confused in DSA.

| Feature | TreeSet | PriorityQueue |
|---|---|---|
| Duplicates | No | Yes |
| Full sorted iteration | Yes | No |
| Minimum/maximum | Easy | Efficiently access head |
| Navigation | Yes | No |
| Search arbitrary element | O(log n) | O(n) |
| Remove arbitrary element | O(log n) | O(n) |
| Main use | Ordered unique elements | Heap / priority processing |

Important:

> A PriorityQueue is not a sorted collection for iteration.

Example:

    PriorityQueue<Integer> pq =
        new PriorityQueue<>();

    pq.add(30);
    pq.add(10);
    pq.add(20);

    System.out.println(pq);

Do not assume the printed representation is:

    [10, 20, 30]

The heap only guarantees priority at the head.

TreeSet, on the other hand, maintains complete sorted ordering.

---

# 40. When to Use TreeSet 🎯

Use TreeSet when you need:

## 1. Unique + Sorted Data

    TreeSet<Integer>

---

## 2. Nearest Element Queries

For example:

    floor(x)
    ceiling(x)
    lower(x)
    higher(x)

---

## 3. Dynamic Sorted Data

When elements are continuously inserted and removed while maintaining order.

---

## 4. Range Queries

Methods:

    headSet()
    tailSet()
    subSet()

are very useful.

---

## 5. Ordered DSA Problems

TreeSet is particularly useful when a problem requires:

    predecessor
    successor
    nearest smaller
    nearest greater
    dynamic ordering
    range extraction

---

# 41. When Not to Use TreeSet 🚫

## 1. You Only Need Fast Membership

Use:

    HashSet

---

## 2. You Need Insertion Order

Use:

    LinkedHashSet

---

## 3. You Need Duplicates

Use:

    List

---

## 4. You Need Index Access

Use:

    ArrayList

---

## 5. You Need Only Repeated Min/Max Extraction

A:

    PriorityQueue

may be more appropriate depending on the problem.

---

# 42. Advantages ✅

## 1. Automatically Sorted

No need to manually sort after every insertion.

---

## 2. No Duplicates

Set semantics automatically prevent duplicates.

---

## 3. Efficient Search

Typical:

    O(log n)

---

## 4. Efficient Navigation

Provides:

    lower()
    floor()
    ceiling()
    higher()

---

## 5. Efficient Range Operations

Provides:

    headSet()
    tailSet()
    subSet()

---

## 6. Dynamic Ordering

Elements remain ordered as data changes.

---

# 43. Disadvantages ❌

## 1. Slower Than HashSet for Basic Average Operations

    TreeSet
        -> O(log n)

    HashSet
        -> O(1) average

---

## 2. More Structural Overhead

Tree nodes require additional references and balancing information.

---

## 3. No Index Access

No:

    get(index)

---

## 4. Comparison Requirement

Elements need a valid comparison mechanism.

---

## 5. Ordering Can Affect Uniqueness

If comparison returns:

    0

TreeSet may treat two objects as equivalent even if `equals()` says they are different.

---

# 44. Common Mistakes 🧠

## Mistake 1 — Thinking TreeSet Preserves Insertion Order

Wrong:

    TreeSet
        -> insertion order

Correct:

    TreeSet
        -> sorted order

---

## Mistake 2 — Thinking TreeSet Uses Hashing

Wrong:

    TreeSet
        -> hash table

Correct:

    TreeSet
        -> tree-based structure

---

## Mistake 3 — Thinking TreeSet Is Always O(1)

Wrong:

    contains()
        -> O(1)

Correct:

    contains()
        -> O(log n)

---

## Mistake 4 — Thinking TreeSet Allows Duplicates

It does not.

---

## Mistake 5 — Confusing floor() and lower()

    floor(x)
        -> <= x

    lower(x)
        -> < x

---

## Mistake 6 — Confusing ceiling() and higher()

    ceiling(x)
        -> >= x

    higher(x)
        -> > x

---

## Mistake 7 — Thinking PriorityQueue Is Fully Sorted

It is not.

---

## Mistake 8 — Forgetting Comparator/Comparable

TreeSet needs a comparison mechanism.

---

## Mistake 9 — Assuming equals() Alone Determines Duplicates

TreeSet's ordering comparison is central to determining element equivalence.

---

# 45. Interview Traps 🎤

## Trap 1

Question:

> What is TreeSet?

Answer:

A `NavigableSet` implementation that stores unique elements in sorted order using a balanced tree structure.

---

## Trap 2

Question:

> What is the underlying data structure?

Answer:

A Red-Black Tree, through the underlying `TreeMap` implementation.

---

## Trap 3

Question:

> What is the complexity of add()?

Answer:

    O(log n)

---

## Trap 4

Question:

> Does TreeSet allow duplicates?

No.

---

## Trap 5

Question:

> Does TreeSet maintain insertion order?

No.

It maintains sorted order.

---

## Trap 6

Question:

> Does TreeSet allow null?

With natural ordering, null is not supported.

A Comparator can explicitly define null ordering.

---

## Trap 7

Question:

> What does floor(x) return?

Largest element:

    <= x

---

## Trap 8

Question:

> What does lower(x) return?

Largest element:

    < x

---

## Trap 9

Question:

> What does ceiling(x) return?

Smallest element:

    >= x

---

## Trap 10

Question:

> What does higher(x) return?

Smallest element:

    > x

---

## Trap 11

Question:

> What is the difference between Comparable and Comparator?

    Comparable
        -> defines natural ordering inside the class

    Comparator
        -> defines external/custom ordering

---

## Trap 12

Question:

> What happens when compareTo() returns 0?

TreeSet treats the objects as equivalent for Set purposes.

---

## Trap 13

Question:

> TreeSet or HashSet for sorted unique data?

    TreeSet

---

## Trap 14

Question:

> TreeSet or LinkedHashSet for insertion-order unique data?

    LinkedHashSet

---

## Trap 15

Question:

> TreeSet or PriorityQueue for complete sorted iteration?

    TreeSet

---

# 46. Practical Examples 💻

## Example 1 — Basic TreeSet

    import java.util.*;

    public class Main {

        public static void main(
            String[] args
        ) {

            TreeSet<Integer> numbers =
                new TreeSet<>();

            numbers.add(50);
            numbers.add(10);
            numbers.add(30);
            numbers.add(20);

            System.out.println(numbers);
        }
    }

Output:

    [10, 20, 30, 50]

---

## Example 2 — Navigation Methods

    import java.util.*;

    public class Main {

        public static void main(
            String[] args
        ) {

            TreeSet<Integer> set =
                new TreeSet<>(
                    List.of(
                        10,
                        20,
                        30,
                        40,
                        50
                    )
                );

            System.out.println(
                set.lower(30)
            );

            System.out.println(
                set.floor(30)
            );

            System.out.println(
                set.ceiling(30)
            );

            System.out.println(
                set.higher(30)
            );
        }
    }

Output:

    20
    30
    30
    40

---

## Example 3 — Reverse Order

    import java.util.*;

    public class Main {

        public static void main(
            String[] args
        ) {

            TreeSet<Integer> set =
                new TreeSet<>(
                    Comparator.reverseOrder()
                );

            set.add(10);
            set.add(30);
            set.add(20);

            System.out.println(set);
        }
    }

Output:

    [30, 20, 10]

---

## Example 4 — Range Query

    import java.util.*;

    public class Main {

        public static void main(
            String[] args
        ) {

            TreeSet<Integer> set =
                new TreeSet<>(
                    List.of(
                        10,
                        20,
                        30,
                        40,
                        50
                    )
                );

            NavigableSet<Integer> range =
                set.subSet(
                    20,
                    true,
                    40,
                    false
                );

            System.out.println(range);
        }
    }

Output:

    [20, 30]

---

## Example 5 — Minimum and Maximum

    import java.util.*;

    public class Main {

        public static void main(
            String[] args
        ) {

            TreeSet<Integer> set =
                new TreeSet<>(
                    List.of(
                        40,
                        10,
                        30,
                        20
                    )
                );

            System.out.println(
                set.first()
            );

            System.out.println(
                set.last()
            );
        }
    }

Output:

    10
    40

---

## Example 6 — Custom Objects with Comparator

    import java.util.*;

    class Student {

        int id;
        String name;

        Student(
            int id,
            String name
        ) {
            this.id = id;
            this.name = name;
        }

        @Override
        public String toString() {

            return id + " - " + name;
        }
    }

    public class Main {

        public static void main(
            String[] args
        ) {

            TreeSet<Student> students =
                new TreeSet<>(
                    Comparator.comparingInt(
                        student ->
                            student.id
                    )
                );

            students.add(
                new Student(
                    103,
                    "Rahul"
                )
            );

            students.add(
                new Student(
                    101,
                    "Aman"
                )
            );

            students.add(
                new Student(
                    102,
                    "Vikas"
                )
            );

            for (Student student :
                 students) {

                System.out.println(student);
            }
        }
    }

Output:

    101 - Aman
    102 - Vikas
    103 - Rahul

---

# 47. DSA Patterns 🧩

TreeSet is especially important in DSA because it provides an ordered dynamic set.

The major patterns are:

---

## Pattern 1 — Predecessor / Successor

Use:

    lower(x)
    higher(x)

when you need:

    largest element < x

or:

    smallest element > x

Typical problem:

> For every element, find the nearest smaller/larger existing value.

---

## Pattern 2 — Floor / Ceiling

Use:

    floor(x)
    ceiling(x)

when equality is allowed.

Meaning:

    floor(x)
        -> greatest value <= x

    ceiling(x)
        -> smallest value >= x

This is one of the most important TreeSet DSA patterns.

---

## Pattern 3 — Dynamic Ordered Set

Use TreeSet when values are continuously:

    inserted
    removed
    searched

while maintaining sorted order.

Example:

    insert:
        10
        30
        20

    structure:
        [10, 20, 30]

Then:

    remove(20)

    structure:
        [10, 30]

---

## Pattern 4 — Nearest Value

Suppose:

    set = [10, 20, 30, 40]

and:

    target = 25

Then:

    floor(25)
        -> 20

    ceiling(25)
        -> 30

You can compare:

    25 - 20 = 5
    30 - 25 = 5

to find the closest value.

---

## Pattern 5 — Range Query

Use:

    subSet()
    headSet()
    tailSet()

when a problem asks for values inside a dynamic range.

Example:

    [10, 20, 30, 40, 50]

Query:

    values from 20 to 40

Use:

    subSet(
        20,
        true,
        40,
        true
    )

Result:

    [20, 30, 40]

---

## Pattern 6 — Remove Minimum / Maximum

Use:

    pollFirst()

for minimum.

Use:

    pollLast()

for maximum.

This is useful when repeatedly processing the smallest or largest element while maintaining uniqueness.

---

## Pattern 7 — Ordered Deduplication

If you need:

    unique elements
        +
    sorted order

TreeSet is a direct solution.

Example:

    [5, 2, 5, 1, 3, 2]

TreeSet:

    [1, 2, 3, 5]

---

## Pattern 8 — Dynamic Rank-Like Queries

TreeSet can help when you need to find:

    predecessor
    successor
    nearest smaller
    nearest greater

However, standard Java TreeSet does not provide direct:

    kth smallest

in O(log n).

For true order-statistics problems, a different data structure may be required.

---

## Pattern 9 — Sweep-Line / Interval Problems

TreeSet can maintain active ordered values.

General idea:

    events
       |
       v
    insert/remove
       |
       v
    TreeSet
       |
       v
    nearest active value

This is useful in some:

    interval
    scheduling
    geometry
    event-processing

problems.

---

## Pattern 10 — Online Queries

If input arrives dynamically and every query asks something about the current ordered values:

    add(x)
    remove(x)
    floor(x)
    ceiling(x)
    lower(x)
    higher(x)

TreeSet can be a natural choice.

---

# 48. DSA Problem-Solving Examples 🧠

## Problem 1 — Find Closest Element

Given a sorted dynamic set and a target, find the closest value.

Example:

    set = [10, 20, 30, 40]
    target = 26

Check:

    floor(26)
        -> 20

    ceiling(26)
        -> 30

Distances:

    26 - 20 = 6
    30 - 26 = 4

Answer:

    30

Code:

    import java.util.*;

    public class Main {

        static int closest(
            TreeSet<Integer> set,
            int target
        ) {

            Integer lower =
                set.floor(target);

            Integer higher =
                set.ceiling(target);

            if (lower == null) {
                return higher;
            }

            if (higher == null) {
                return lower;
            }

            if (
                target - lower
                <=
                higher - target
            ) {
                return lower;
            }

            return higher;
        }

        public static void main(
            String[] args
        ) {

            TreeSet<Integer> set =
                new TreeSet<>(
                    List.of(
                        10,
                        20,
                        30,
                        40
                    )
                );

            System.out.println(
                closest(set, 26)
            );
        }
    }

Output:

    30

### Pattern

    floor()
        +
    ceiling()

---

## Problem 2 — Find Next Greater Element

For every value, find the smallest value greater than it.

Core operation:

    higher(x)

Example:

    set = [10, 20, 30, 40]

    higher(20)
        -> 30

Code:

    TreeSet<Integer> set =
        new TreeSet<>(
            List.of(
                10,
                20,
                30,
                40
            )
        );

    System.out.println(
        set.higher(20)
    );

Output:

    30

### Pattern

    successor
        ->
    higher()

---

## Problem 3 — Find Previous Smaller Element

Core operation:

    lower(x)

Example:

    set = [10, 20, 30, 40]

    lower(30)
        -> 20

Code:

    TreeSet<Integer> set =
        new TreeSet<>(
            List.of(
                10,
                20,
                30,
                40
            )
        );

    System.out.println(
        set.lower(30)
    );

Output:

    20

### Pattern

    predecessor
        ->
    lower()

---

## Problem 4 — Floor Query

Given:

    set = [10, 20, 30, 40]

Find the largest value <= 27.

Use:

    floor(27)

Result:

    20

Code:

    TreeSet<Integer> set =
        new TreeSet<>(
            List.of(
                10,
                20,
                30,
                40
            )
        );

    System.out.println(
        set.floor(27)
    );

Output:

    20

---

## Problem 5 — Ceiling Query

Given:

    set = [10, 20, 30, 40]

Find the smallest value >= 27.

Use:

    ceiling(27)

Result:

    30

Code:

    TreeSet<Integer> set =
        new TreeSet<>(
            List.of(
                10,
                20,
                30,
                40
            )
        );

    System.out.println(
        set.ceiling(27)
    );

Output:

    30

---

## Problem 6 — Dynamic Minimum

Suppose:

    insert 30
    insert 10
    insert 20

Current set:

    [10, 20, 30]

Get minimum:

    first()

Remove minimum:

    pollFirst()

After:

    [20, 30]

Code:

    TreeSet<Integer> set =
        new TreeSet<>();

    set.add(30);
    set.add(10);
    set.add(20);

    System.out.println(
        set.first()
    );

    System.out.println(
        set.pollFirst()
    );

    System.out.println(set);

Output:

    10
    10
    [20, 30]

---

## Problem 7 — Dynamic Maximum

Use:

    last()

or:

    pollLast()

Code:

    TreeSet<Integer> set =
        new TreeSet<>(
            List.of(
                10,
                20,
                30
            )
        );

    System.out.println(
        set.last()
    );

Output:

    30

---

## Problem 8 — Remove Duplicates and Sort

Input:

    [5, 2, 5, 1, 3, 2]

Use:

    TreeSet<Integer> set =
        new TreeSet<>();

    for (int number : numbers) {

        set.add(number);
    }

Result:

    [1, 2, 3, 5]

Pattern:

    Set
        +
    sorting

---

## Problem 9 — Dynamic Range

Suppose:

    set = [10, 20, 30, 40, 50, 60]

Need:

    values from 25 to 55

Use:

    subSet(
        25,
        true,
        55,
        true
    )

Result:

    [30, 40, 50]

Code:

    TreeSet<Integer> set =
        new TreeSet<>(
            List.of(
                10,
                20,
                30,
                40,
                50,
                60
            )
        );

    NavigableSet<Integer> range =
        set.subSet(
            25,
            true,
            55,
            true
        );

    System.out.println(range);

Output:

    [30, 40, 50]

---

# 49. Top Interview Questions 🎤

## Q1. What is TreeSet?

`TreeSet` is a `NavigableSet` implementation that stores unique elements in sorted order using a balanced tree structure.

---

## Q2. What is the underlying data structure of TreeSet?

A Red-Black Tree through its underlying `TreeMap` implementation.

---

## Q3. What is the complexity of TreeSet operations?

Typical:

    add()
        -> O(log n)

    remove()
        -> O(log n)

    contains()
        -> O(log n)

---

## Q4. Does TreeSet allow duplicates?

No.

---

## Q5. Does TreeSet maintain insertion order?

No.

It maintains sorted order.

---

## Q6. What is natural ordering?

The default ordering defined by the elements themselves through `Comparable`.

---

## Q7. What is Comparator?

A separate object/function that defines custom ordering.

---

## Q8. Difference between Comparable and Comparator?

    Comparable
        -> natural ordering
        -> compareTo()

    Comparator
        -> external/custom ordering
        -> compare()

---

## Q9. What happens when compareTo() returns zero?

TreeSet treats the elements as equivalent for Set purposes.

---

## Q10. What does lower(x) return?

Largest element:

    < x

---

## Q11. What does floor(x) return?

Largest element:

    <= x

---

## Q12. What does ceiling(x) return?

Smallest element:

    >= x

---

## Q13. What does higher(x) return?

Smallest element:

    > x

---

## Q14. What is the difference between floor() and lower()?

    floor(x)
        -> <= x

    lower(x)
        -> < x

---

## Q15. What is the difference between ceiling() and higher()?

    ceiling(x)
        -> >= x

    higher(x)
        -> > x

---

## Q16. What does first() return?

The smallest element.

---

## Q17. What does last() return?

The largest element.

---

## Q18. What does pollFirst() do?

Removes and returns the smallest element.

---

## Q19. What does pollLast() do?

Removes and returns the largest element.

---

## Q20. What does descendingSet() do?

Returns a reverse-order view of the Set.

---

## Q21. What is the difference between TreeSet and HashSet?

    TreeSet
        -> sorted
        -> O(log n)

    HashSet
        -> no guaranteed order
        -> O(1) average

---

## Q22. What is the difference between TreeSet and LinkedHashSet?

    TreeSet
        -> sorted order

    LinkedHashSet
        -> insertion order

---

## Q23. Why is TreeSet useful in DSA?

Because it supports dynamic ordered data and efficient:

    predecessor
    successor
    floor
    ceiling
    range queries

---

## Q24. Can TreeSet store null?

Natural-order TreeSet does not support null. A Comparator can explicitly define null ordering.

---

## Q25. Does TreeSet use equals() to determine duplicates?

Its ordering comparison is used to determine equivalence for Set behavior. If comparison returns zero, TreeSet treats the elements as duplicates/equivalent.

---

## Q26. Can two objects have compareTo() == 0 but equals() == false?

Yes.

This can happen with a custom ordering.

TreeSet will still treat them as equivalent for Set purposes.

---

## Q27. Why is TreeSet slower than HashSet?

TreeSet maintains sorted order using a balanced tree, giving O(log n) operations, while HashSet provides O(1) average basic operations through hashing.

---

## Q28. TreeSet or PriorityQueue?

Use:

    TreeSet
        -> complete sorted unique set
        -> navigation

    PriorityQueue
        -> priority-based processing
        -> duplicates allowed
        -> heap semantics

---

## Q29. What is the biggest DSA advantage of TreeSet?

Efficient predecessor/successor and floor/ceiling queries.

---

## Q30. What is one major limitation of TreeSet for DSA?

It does not directly support order-statistics such as:

    kth smallest

in O(log n).

---

# 50. 30-Second Interview Answer 🎯

If the interviewer asks:

> "Explain TreeSet."

Answer:

> "`TreeSet` is a `NavigableSet` implementation in Java that stores unique elements in sorted order. It is backed by a Red-Black Tree through the underlying `TreeMap` implementation, so basic operations such as `add`, `remove`, and `contains` take O(log n) time. It supports natural ordering through `Comparable` or custom ordering through `Comparator`. Its biggest advantage is that it provides navigation methods such as `lower`, `floor`, `ceiling`, and `higher`, which makes it very useful for ordered-set and DSA problems."

---

# 51. Cheat Sheet 📋

## Definition

    TreeSet
        -> Sorted Set
        -> Unique elements
        -> Navigable
        -> Tree-based
        -> O(log n) basic operations

---

## Internal Structure

    TreeSet
       |
       v
    TreeMap
       |
       v
    Red-Black Tree

---

## Ordering

    Comparable
        ->
    Natural ordering

    Comparator
        ->
    Custom ordering

---

## Navigation

    lower(x)
        -> greatest < x

    floor(x)
        -> greatest <= x

    ceiling(x)
        -> smallest >= x

    higher(x)
        -> smallest > x

---

## Extremes

    first()
        -> minimum

    last()
        -> maximum

    pollFirst()
        -> remove + minimum

    pollLast()
        -> remove + maximum

---

## Range

    headSet()
        -> before value

    tailSet()
        -> from value onward

    subSet()
        -> range

---

## Complexity

    add()
        -> O(log n)

    remove()
        -> O(log n)

    contains()
        -> O(log n)

    lower()
        -> O(log n)

    floor()
        -> O(log n)

    ceiling()
        -> O(log n)

    higher()
        -> O(log n)

---

## Set Comparison

    HashSet
        -> Unique
        -> Hashing
        -> O(1) average
        -> No guaranteed order

    LinkedHashSet
        -> Unique
        -> Hashing
        -> O(1) average
        -> Insertion order

    TreeSet
        -> Unique
        -> Tree
        -> O(log n)
        -> Sorted order
        -> Navigation

---

# 52. Quick Revision ⚡

Remember:

    1. TreeSet is a class.

    2. It belongs to java.util.

    3. It implements NavigableSet.

    4. It stores unique elements.

    5. It maintains sorted order.

    6. It does not maintain insertion order.

    7. It uses a balanced tree structure.

    8. TreeSet is backed by TreeMap.

    9. TreeMap uses a Red-Black Tree.

    10. add() is O(log n).

    11. remove() is O(log n).

    12. contains() is O(log n).

    13. first() gives the minimum.

    14. last() gives the maximum.

    15. lower(x) gives the greatest value < x.

    16. floor(x) gives the greatest value <= x.

    17. ceiling(x) gives the smallest value >= x.

    18. higher(x) gives the smallest value > x.

    19. pollFirst() removes the minimum.

    20. pollLast() removes the maximum.

    21. Comparable defines natural ordering.

    22. Comparator defines custom ordering.

    23. compareTo() returning 0 means TreeSet treats elements as equivalent.

    24. Natural-order TreeSet does not support null.

    25. A Comparator can explicitly define null ordering.

    26. TreeSet is useful for predecessor/successor queries.

    27. TreeSet is useful for floor/ceiling queries.

    28. TreeSet is useful for dynamic sorted data.

    29. TreeSet is useful for range queries.

    30. TreeSet does not provide index access.

    31. TreeSet does not directly provide kth-smallest order statistics.

    32. PriorityQueue is not a replacement for full sorted iteration.

    33. HashSet is generally preferred when ordering is unnecessary.

    34. LinkedHashSet is preferred when insertion order is required.

    35. TreeSet is preferred when sorted order and navigation are required.

---

# Final Memory Trick 🧠

Think:

    HashSet
        = UNIQUE + FAST

    LinkedHashSet
        = UNIQUE + FAST + INSERTION ORDER

    TreeSet
        = UNIQUE + SORTED + NAVIGATION

And remember the four most important TreeSet methods:

    lower(x)
        < x

    floor(x)
        <= x

    ceiling(x)
        >= x

    higher(x)
        > x

Visual memory:

                 TreeSet
                    |
          +---------+---------+
          |         |         |
        lower      floor    ceiling      higher
          |         |         |             |
         <x        <=x       >=x            >x

For DSA:

    Need predecessor?
        -> lower()

    Need successor?
        -> higher()

    Need nearest smaller/equal?
        -> floor()

    Need nearest greater/equal?
        -> ceiling()

    Need sorted unique data?
        -> TreeSet

    Need dynamic ordered queries?
        -> TreeSet