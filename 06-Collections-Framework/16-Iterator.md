16-Iterator.md

# 16 — Iterator Interface 🔄

> **Package:** `java.util`  
> **Type:** Interface  
> **Purpose:** Provides a standard way to traverse elements of a collection one by one.

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [What is Iterator?](#2-what-is-iterator)
3. [Why Iterator Exists](#3-why-iterator-exists)
4. [Iterator Hierarchy](#4-iterator-hierarchy)
5. [Creating an Iterator](#5-creating-an-iterator)
6. [Basic Iterator Example](#6-basic-iterator-example)
7. [Iterator Methods](#7-iterator-methods)
8. [hasNext()](#8-hasnext)
9. [next()](#9-next)
10. [remove()](#10-remove)
11. [forEachRemaining()](#11-foreachremaining)
12. [Iterator Traversal](#12-iterator-traversal)
13. [Internal Working](#13-internal-working)
14. [Iterator and Collection](#14-iterator-and-collection)
15. [Iterator and Iterable](#15-iterator-and-iterable)
16. [Iterable vs Iterator](#16-iterable-vs-iterator)
17. [Iterator vs Enhanced for Loop](#17-iterator-vs-enhanced-for-loop)
18. [Iterator vs forEach()](#18-iterator-vs-foreach)
19. [Iterator with ArrayList](#19-iterator-with-arraylist)
20. [Iterator with LinkedList](#20-iterator-with-linkedlist)
21. [Iterator with HashSet](#21-iterator-with-hashset)
22. [Iterator with TreeSet](#22-iterator-with-treeset)
23. [Iterator with HashMap](#23-iterator-with-hashmap)
24. [Removing Elements Using Iterator](#24-removing-elements-using-iterator)
25. [ConcurrentModificationException](#25-concurrentmodificationexception)
26. [Fail-Fast Iterator](#26-fail-fast-iterator)
27. [Structural Modification](#27-structural-modification)
28. [Iterator State](#28-iterator-state)
29. [next() Without hasNext()](#29-next-without-hasnext)
30. [Multiple Iterators](#30-multiple-iterators)
31. [Iterator and Generics](#31-iterator-and-generics)
32. [Iterator with Custom Collection](#32-iterator-with-custom-collection)
33. [Iterator and Thread Safety](#33-iterator-and-thread-safety)
34. [Iterator vs ListIterator](#34-iterator-vs-listiterator)
35. [Iterator vs Enumeration](#35-iterator-vs-enumeration)
36. [Advantages](#36-advantages)
37. [Limitations](#37-limitations)
38. [Common Mistakes](#38-common-mistakes)
39. [Interview Traps](#39-interview-traps)
40. [DSA Patterns](#40-dsa-patterns)
41. [DSA Pattern 1 — Traversal](#41-dsa-pattern-1--traversal)
42. [DSA Pattern 2 — Safe Removal](#42-dsa-pattern-2--safe-removal)
43. [DSA Pattern 3 — Filtering](#43-dsa-pattern-3--filtering)
44. [Top Interview Questions](#44-top-interview-questions)
45. [30-Second Interview Answer](#45-30-second-interview-answer)
46. [Cheat Sheet](#46-cheat-sheet)
47. [Quick Revision](#47-quick-revision)

---

# 1. Introduction 🔄

`Iterator` is an interface from the Java Collections Framework.

It provides a common mechanism to traverse elements of a collection.

Instead of depending on the internal structure of:

    ArrayList
    LinkedList
    HashSet
    TreeSet

we can use:

    Iterator

Example:

    ArrayList<Integer> list =
        new ArrayList<>();

    Iterator<Integer> iterator =
        list.iterator();

Now the collection can be traversed through the iterator.

---

# 2. What is Iterator? 🎯

`Iterator<E>` represents an object that can move through a collection one element at a time.

Conceptually:

    Collection
        |
        | iterator()
        v
    Iterator
        |
        v
    element 1
        |
        v
    element 2
        |
        v
    element 3

Important:

> Iterator is not a collection.

It is a traversal mechanism.

---

# 3. Why Iterator Exists? 🧠

Different collections use different internal data structures.

For example:

    ArrayList
        -> Dynamic Array

    LinkedList
        -> Doubly Linked List

    HashSet
        -> Hash Table based structure

    TreeSet
        -> Tree-based structure

If Java forced the programmer to understand each internal structure for traversal, collection usage would become complicated.

Iterator provides a common abstraction.

Therefore:

    Different Collections
           |
           v
        Iterator
           |
           v
      Common Traversal

This is an example of abstraction.

---

# 4. Iterator Hierarchy 🌳

`Iterator` is an interface.

Important hierarchy:

    Iterable
       |
       | iterator()
       v
    Iterator

Do not confuse this with:

    Collection
       |
       +---- Iterator

Iterator is NOT a child of Collection.

Rather:

    Collection implements Iterable

and Iterable provides:

    iterator()

which returns:

    Iterator

Conceptually:

    Iterable
       |
       +---- iterator()
                |
                v
             Iterator

---

# 5. Creating an Iterator 🛠️

Most collections provide:

    iterator()

method.

Example:

    List<Integer> list =
        new ArrayList<>();

    list.add(10);
    list.add(20);
    list.add(30);

    Iterator<Integer> iterator =
        list.iterator();

Now:

    iterator

points before the first element.

Conceptually:

    iterator
       |
       v
    [10] [20] [30]

The iterator has not returned `10` yet.

---

# 6. Basic Iterator Example 💻

Example:

    import java.util.ArrayList;
    import java.util.Iterator;

    public class Main {

        public static void main(String[] args) {

            ArrayList<Integer> list =
                new ArrayList<>();

            list.add(10);
            list.add(20);
            list.add(30);

            Iterator<Integer> iterator =
                list.iterator();

            while (iterator.hasNext()) {

                System.out.println(
                    iterator.next()
                );
            }
        }
    }

Output:

    10
    20
    30

The important pattern is:

    while(iterator.hasNext()) {
        iterator.next();
    }

---

# 7. Iterator Methods 📚

Modern `Iterator` provides four important methods:

    hasNext()
    next()
    remove()
    forEachRemaining()

Their purpose:

| Method | Purpose |
|---|---|
| hasNext() | Checks whether another element exists |
| next() | Returns the next element |
| remove() | Removes the last element returned by iterator |
| forEachRemaining() | Performs an action on remaining elements |

---

# 8. hasNext() 🔍

`hasNext()` checks whether another element is available.

Return type:

    boolean

Example:

    if (iterator.hasNext()) {
        System.out.println(
            iterator.next()
        );
    }

If another element exists:

    true

Otherwise:

    false

Typical usage:

    while (iterator.hasNext()) {
        System.out.println(iterator.next());
    }

---

# 9. next() ➡️

`next()` returns the next element.

Example:

    Iterator<Integer> iterator =
        list.iterator();

    System.out.println(
        iterator.next()
    );

If:

    list = [10, 20, 30]

First call:

    next()
        -> 10

Second call:

    next()
        -> 20

Third call:

    next()
        -> 30

After the iterator reaches the end, another call to `next()` throws:

    NoSuchElementException

Therefore, normally use:

    hasNext()

before:

    next()

---

# 10. remove() 🗑️

`remove()` removes the last element returned by the iterator.

Example:

    ArrayList<Integer> list =
        new ArrayList<>();

    list.add(10);
    list.add(20);
    list.add(30);

    Iterator<Integer> iterator =
        list.iterator();

    while (iterator.hasNext()) {

        int value = iterator.next();

        if (value == 20) {
            iterator.remove();
        }
    }

Result:

    [10, 30]

Important:

> Iterator's remove() safely removes the element from the underlying collection through the iterator.

---

# 11. forEachRemaining() 🔁

`forEachRemaining()` processes all elements that have not yet been traversed.

Example:

    Iterator<Integer> iterator =
        list.iterator();

    System.out.println(
        iterator.next()
    );

    iterator.forEachRemaining(
        value -> System.out.println(value)
    );

If:

    list = [10, 20, 30]

and the first `next()` returned:

    10

then `forEachRemaining()` processes:

    20
    30

---

# 12. Iterator Traversal 🚶

Suppose:

    list = [10, 20, 30]

Initially:

    iterator
        |
        v
    [10] [20] [30]

After:

    iterator.next()

the iterator moves forward.

Conceptually:

    [10] [20] [30]
          ^
          |
       iterator

Another:

    iterator.next()

then:

    [10] [20] [30]
                ^
                |
             iterator

The iterator maintains traversal state.

---

# 13. Internal Working ⚙️

The exact implementation depends on the collection.

For an `ArrayList`, an iterator can internally maintain a cursor/index.

Conceptually:

    cursor = 0

For:

    [10, 20, 30]

`next()` can return:

    element[cursor]

then advance:

    cursor++

Conceptually:

    cursor = 0
        |
        v
    [10] [20] [30]

After next():

    cursor = 1

Then:

    [10] [20] [30]
          ^
          |
        cursor

For linked or tree-based collections, the iterator's internal mechanism is different.

The important point is:

> Iterator hides the collection's traversal implementation.

---

# 14. Iterator and Collection 🔗

Collections expose an iterator through:

    iterator()

Example:

    Collection<Integer> collection =
        new ArrayList<>();

    Iterator<Integer> iterator =
        collection.iterator();

The collection owns the elements.

The iterator provides traversal.

Therefore:

    Collection
        |
        | iterator()
        v
    Iterator
        |
        v
    Elements

---

# 15. Iterator and Iterable 🔗

This relationship is extremely important.

`Iterable<T>` provides:

    Iterator<T> iterator()

Therefore:

    Iterable
        |
        +---- iterator()
                    |
                    v
                 Iterator

Example:

    ArrayList<Integer> list =
        new ArrayList<>();

    Iterator<Integer> iterator =
        list.iterator();

`ArrayList` implements `List`, and `List` ultimately extends `Collection`, while `Collection` extends `Iterable`.

Therefore it gets the `iterator()` contract.

---

# 16. Iterable vs Iterator ⚖️

This is a common interview question.

| Iterable | Iterator |
|---|---|
| Represents something that can provide an iterator | Represents the iterator itself |
| Has `iterator()` | Has `hasNext()` and `next()` |
| Used as source of traversal | Used to perform traversal |
| Supports enhanced for loop | Controls traversal manually |

Think:

    Iterable
        =
    "Give me an iterator."

    Iterator
        =
    "I will traverse the elements."

---

# 17. Iterator vs Enhanced for Loop ⚖️

Enhanced for loop:

    for (Integer value : list) {
        System.out.println(value);
    }

Internally, conceptually, it uses an iterator for `Iterable` collections.

Equivalent conceptual form:

    Iterator<Integer> iterator =
        list.iterator();

    while (iterator.hasNext()) {

        Integer value =
            iterator.next();

        System.out.println(value);
    }

Therefore:

    for-each loop
          |
          v
    Iterator concept

However, the compiler-generated details depend on the target being an array or an `Iterable`.

---

# 18. Iterator vs forEach() ⚖️

Example using `forEach()`:

    list.forEach(
        value -> System.out.println(value)
    );

Example using Iterator:

    Iterator<Integer> iterator =
        list.iterator();

    while (iterator.hasNext()) {

        Integer value =
            iterator.next();

        System.out.println(value);
    }

Iterator gives more explicit control over traversal and supports iterator-based removal.

---

# 19. Iterator with ArrayList 📦

Example:

    ArrayList<String> names =
        new ArrayList<>();

    names.add("A");
    names.add("B");
    names.add("C");

    Iterator<String> iterator =
        names.iterator();

    while (iterator.hasNext()) {

        String name =
            iterator.next();

        System.out.println(name);
    }

Output:

    A
    B
    C

---

# 20. Iterator with LinkedList 🔗

Example:

    LinkedList<Integer> list =
        new LinkedList<>();

    list.add(10);
    list.add(20);
    list.add(30);

    Iterator<Integer> iterator =
        list.iterator();

    while (iterator.hasNext()) {

        System.out.println(
            iterator.next()
        );
    }

Output:

    10
    20
    30

The same Iterator interface works even though LinkedList uses a different internal structure.

---

# 21. Iterator with HashSet 🧩

Example:

    HashSet<Integer> set =
        new HashSet<>();

    set.add(10);
    set.add(20);
    set.add(30);

    Iterator<Integer> iterator =
        set.iterator();

    while (iterator.hasNext()) {

        System.out.println(
            iterator.next()
        );
    }

Important:

> HashSet does not guarantee iteration order.

Therefore, do not assume the output will necessarily be:

    10
    20
    30

---

# 22. Iterator with TreeSet 🌳

Example:

    TreeSet<Integer> set =
        new TreeSet<>();

    set.add(30);
    set.add(10);
    set.add(20);

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

TreeSet's iterator traverses according to the set's sorted order.

---

# 23. Iterator with HashMap 🗺️

`Map` does not directly extend `Collection`.

Therefore:

    Map

does not directly provide:

    iterator()

Instead, obtain an iterator from a collection view.

For example:

    HashMap<Integer, String> map =
        new HashMap<>();

    map.put(1, "A");
    map.put(2, "B");

    Iterator<Integer> iterator =
        map.keySet().iterator();

Here:

    keySet()
        -> Set<K>
        -> Collection
        -> Iterable
        -> iterator()

You can also iterate over:

    entrySet()

or:

    values()

Example:

    Iterator<Map.Entry<Integer, String>> iterator =
        map.entrySet().iterator();

---

# 24. Removing Elements Using Iterator 🗑️

Suppose:

    list = [10, 20, 30, 40]

We want to remove:

    20

Correct approach:

    Iterator<Integer> iterator =
        list.iterator();

    while (iterator.hasNext()) {

        int value =
            iterator.next();

        if (value == 20) {
            iterator.remove();
        }
    }

Result:

    [10, 30, 40]

Why use:

    iterator.remove()

instead of:

    list.remove()

during traversal?

Because directly structurally modifying many collections while their iterator is active can cause:

    ConcurrentModificationException

---

# 25. ConcurrentModificationException ⚠️

Consider:

    ArrayList<Integer> list =
        new ArrayList<>();

    list.add(10);
    list.add(20);
    list.add(30);

    Iterator<Integer> iterator =
        list.iterator();

    while (iterator.hasNext()) {

        int value =
            iterator.next();

        if (value == 20) {
            list.remove(Integer.valueOf(20));
        }
    }

This may throw:

    ConcurrentModificationException

because the collection was structurally modified outside the iterator.

Correct approach:

    iterator.remove();

instead of:

    list.remove(...);

when removing the element currently returned by that iterator.

---

# 26. Fail-Fast Iterator ⚡

Many standard Java collection iterators are described as:

    fail-fast

A fail-fast iterator attempts to detect structural modification of the collection outside the iterator and may throw:

    ConcurrentModificationException

Example:

    Iterator<Integer> iterator =
        list.iterator();

    list.add(40);

    iterator.next();

This can result in:

    ConcurrentModificationException

Important:

> Fail-fast behavior is a best-effort mechanism for detecting bugs. It should not be used as a synchronization mechanism.

---

# 27. Structural Modification 🏗️

Structural modification generally means a change that changes the collection's size or structure.

Examples:

    add()
    remove()
    clear()

For an ArrayList:

    add()
    remove()

are structural modifications.

Changing an existing element with:

    set()

does not change the size.

Example:

    list.set(0, 100);

This replaces an element rather than structurally changing the collection.

---

# 28. Iterator State 🧠

An iterator has a current traversal state.

For:

    [10, 20, 30]

initially:

    before 10

After:

    next()

state moves after 10.

Then:

    next()

moves after 20.

The important conceptual states are:

    Before next()
        |
        v
    next()
        |
        v
    Element returned
        |
        v
    Iterator advances

This is important for understanding:

    remove()

because `remove()` applies to the element most recently returned by `next()`.

---

# 29. next() Without hasNext() ⚠️

You can technically call:

    iterator.next()

without first calling:

    hasNext()

But if no element remains, `next()` throws:

    NoSuchElementException

Unsafe pattern:

    while (true) {
        System.out.println(
            iterator.next()
        );
    }

Safe pattern:

    while (iterator.hasNext()) {
        System.out.println(
            iterator.next()
        );
    }

---

# 30. Multiple Iterators 🔄

A collection can have multiple iterators.

Example:

    Iterator<Integer> first =
        list.iterator();

    Iterator<Integer> second =
        list.iterator();

Both iterators maintain their own traversal state.

Conceptually:

    Collection
      |
      +---- Iterator A
      |
      +---- Iterator B

They can independently move through the collection.

However, structural modifications can affect both iterators and may trigger fail-fast behavior.

---

# 31. Iterator and Generics 🧬

Iterator supports generics.

Example:

    Iterator<String> iterator =
        list.iterator();

This means:

    next()

returns:

    String

instead of:

    Object

Without generics:

    Iterator iterator =
        list.iterator();

then:

    Object value =
        iterator.next();

Generics provide:

    Type Safety
    No unnecessary casting

---

# 32. Iterator with Custom Collection 🛠️

A custom class can implement:

    Iterable<T>

and provide its own iterator.

Example:

    class MyCollection
            implements Iterable<Integer> {

        private int[] data =
            {10, 20, 30};

        @Override
        public Iterator<Integer> iterator() {

            return new Iterator<Integer>() {

                private int index = 0;

                @Override
                public boolean hasNext() {
                    return index < data.length;
                }

                @Override
                public Integer next() {

                    if (!hasNext()) {
                        throw new NoSuchElementException();
                    }

                    return data[index++];
                }
            };
        }
    }

Usage:

    MyCollection collection =
        new MyCollection();

    for (Integer value : collection) {
        System.out.println(value);
    }

This demonstrates:

    Iterable
        |
        v
    iterator()
        |
        v
    custom Iterator

---

# 33. Iterator and Thread Safety 🔒

Iterator itself does not automatically make a collection thread-safe.

For example:

    ArrayList

is not thread-safe.

Its iterator does not magically make it thread-safe.

If multiple threads modify a collection concurrently, appropriate synchronization or concurrent collections may be required.

Some concurrent collections provide different iterator consistency guarantees.

For example:

    ConcurrentHashMap

has iterators designed for concurrent access and does not behave like ordinary fail-fast collection iterators.

---

# 34. Iterator vs ListIterator ⚖️

| Feature | Iterator | ListIterator |
|---|---|---|
| Forward traversal | Yes | Yes |
| Backward traversal | No | Yes |
| Works with List | Yes | Yes |
| Works with Set | Yes | No |
| add() | No | Yes |
| set() | No | Yes |
| remove() | Yes | Yes |
| previous() | No | Yes |
| nextIndex() | No | Yes |
| previousIndex() | No | Yes |

`ListIterator` is specifically designed for:

    List

while `Iterator` is more general.

---

# 35. Iterator vs Enumeration ⚖️

`Enumeration` is an older traversal interface.

| Feature | Iterator | Enumeration |
|---|---|---|
| Modern collections | Yes | Mostly legacy |
| hasNext() | Yes | No |
| next() | Yes | nextElement() |
| remove() | Yes | No |
| Recommended for new code | Yes | Generally no |

Modern Java code generally uses:

    Iterator

instead of:

    Enumeration`

for collection traversal.

---

# 36. Advantages ✅

## 1. Common Traversal Mechanism

Works across many collections.

---

## 2. Abstraction

You do not need to know the internal data structure.

---

## 3. Safe Iterator-Based Removal

Supports:

    iterator.remove()

---

## 4. Generic

Supports:

    Iterator<T>

---

## 5. Works with Unordered Collections

Useful for:

    HashSet

where index-based traversal is unavailable.

---

## 6. Foundation for Enhanced For Loop

The enhanced for loop uses the `Iterable`/iterator mechanism for iterable objects.

---

# 37. Limitations ⚠️

## 1. Forward Traversal Only

Iterator normally moves:

    forward

For backward traversal use:

    ListIterator

---

## 2. No Index

Iterator does not provide:

    get(index)

---

## 3. No Direct Addition

Iterator does not provide:

    add()

`ListIterator` does.

---

## 4. No Direct Replacement

Iterator does not provide:

    set()

`ListIterator` does.

---

## 5. Structural Modification Restrictions

External modification can result in:

    ConcurrentModificationException

for fail-fast iterators.

---

# 38. Common Mistakes ⚠️

## Mistake 1

Calling:

    next()

after the iterator is exhausted.

Result:

    NoSuchElementException

---

## Mistake 2

Removing directly from collection while iterating.

Potential result:

    ConcurrentModificationException

Use:

    iterator.remove()

when appropriate.

---

## Mistake 3

Thinking Iterator stores the collection.

Iterator does not represent the collection itself.

It provides traversal over it.

---

## Mistake 4

Thinking Iterator is a class.

It is an:

    interface

---

## Mistake 5

Thinking Map directly implements Iterable.

Map does not extend Collection.

Use:

    keySet()
    values()
    entrySet()

for iterable views.

---

## Mistake 6

Expecting HashSet iteration order.

HashSet does not guarantee insertion order.

---

# 39. Interview Traps 🎤

## Trap 1

> Is Iterator a class?

No.

It is an interface.

---

## Trap 2

> What package contains Iterator?

    java.util

---

## Trap 3

> Which method creates an Iterator?

    iterator()

---

## Trap 4

> Which method checks for another element?

    hasNext()

---

## Trap 5

> Which method retrieves the next element?

    next()

---

## Trap 6

> What happens if next() is called after the end?

    NoSuchElementException

---

## Trap 7

> Can Iterator remove elements?

Yes.

Using:

    remove()

---

## Trap 8

> Can Iterator add elements?

No.

`ListIterator` can.

---

## Trap 9

> Can Iterator move backward?

No.

Use:

    ListIterator

for lists.

---

## Trap 10

> Does Map implement Collection?

No.

Therefore Map does not directly inherit Collection's `iterator()` method.

---

## Trap 11

> Why use Iterator instead of indexing?

Because not all collections provide index-based access.

For example:

    HashSet

---

## Trap 12

> Is Iterator thread-safe?

No.

Iterator itself does not guarantee thread safety.

---

## Trap 13

> What is fail-fast?

An iterator detects certain structural modifications outside itself and may throw `ConcurrentModificationException`.

---

# 40. DSA Patterns 🧩

Iterator is not usually the main DSA data structure, but it is useful for:

    1. Collection Traversal
    2. Filtering
    3. Safe Removal
    4. Custom Data Structure Traversal
    5. Graph/Tree Traversal APIs

The important DSA idea is:

> Traverse without depending on the underlying data structure.

---

# 41. DSA Pattern 1 — Traversal 🚶

Basic traversal:

    Iterator<Integer> iterator =
        collection.iterator();

    while (iterator.hasNext()) {

        int value =
            iterator.next();

        // process value
    }

This works for many collection types.

---

# 42. DSA Pattern 2 — Safe Removal 🗑️

Suppose:

    [10, 15, 20, 25]

Remove all even numbers.

Use:

    Iterator<Integer> iterator =
        list.iterator();

    while (iterator.hasNext()) {

        int value =
            iterator.next();

        if (value % 2 == 0) {
            iterator.remove();
        }
    }

Result:

    [15, 25]

The important pattern:

    next()
      |
      v
    check
      |
      v
    iterator.remove()

---

# 43. DSA Pattern 3 — Filtering 🔎

Iterator can process elements based on a condition.

Example:

    Iterator<Integer> iterator =
        list.iterator();

    while (iterator.hasNext()) {

        int value =
            iterator.next();

        if (value > 50) {
            System.out.println(value);
        }
    }

This is a basic filtering pattern.

---

# 44. Top Interview Questions 🎤

## Q1. What is Iterator?

Iterator is an interface used to traverse elements of a collection sequentially.

---

## Q2. Which package contains Iterator?

    java.util

---

## Q3. What are the main Iterator methods?

    hasNext()
    next()
    remove()
    forEachRemaining()

---

## Q4. What does hasNext() return?

    boolean

It returns true if another element is available.

---

## Q5. What does next() do?

It returns the next element and advances the iterator.

---

## Q6. What happens if next() is called when no element remains?

    NoSuchElementException

---

## Q7. Can Iterator remove elements?

Yes.

Using:

    remove()

---

## Q8. Can Iterator add elements?

No.

`ListIterator` provides `add()`.

---

## Q9. Can Iterator traverse backward?

No.

`ListIterator` can traverse backward.

---

## Q10. What is Iterable?

`Iterable` is an interface that provides:

    iterator()

---

## Q11. Difference between Iterable and Iterator?

Iterable provides an iterator.

Iterator performs the traversal.

---

## Q12. Does Map implement Iterable?

No.

Map provides iterable views such as:

    keySet()
    values()
    entrySet()

---

## Q13. What is fail-fast Iterator?

An iterator that may throw `ConcurrentModificationException` when it detects certain structural modifications outside the iterator.

---

## Q14. Why does ConcurrentModificationException occur?

Because a collection may be structurally modified while an iterator is traversing it outside the iterator's supported modification mechanism.

---

## Q15. How can an element be safely removed during Iterator traversal?

Use:

    iterator.remove()

---

## Q16. Is Iterator thread-safe?

No.

---

## Q17. What is the difference between Iterator and ListIterator?

Iterator supports forward traversal and removal.

ListIterator additionally supports bidirectional traversal and list modification operations.

---

## Q18. Why is Iterator useful for HashSet?

HashSet has no index-based access, but Iterator provides a standard traversal mechanism.

---

## Q19. Does Iterator know the internal structure of every collection?

The iterator implementation knows how to traverse its particular collection, while the programmer uses the common Iterator interface.

---

## Q20. What is the purpose of forEachRemaining()?

It performs an action on every remaining element that has not yet been traversed.

---

# 45. 30-Second Interview Answer 🎯

If the interviewer asks:

> "What is Iterator in Java?"

Answer:

> "`Iterator` is an interface from `java.util` used to traverse elements of a collection one by one without exposing the collection's internal data structure. Its main methods are `hasNext()`, `next()`, `remove()`, and `forEachRemaining()`. `hasNext()` checks whether another element exists, while `next()` returns it and advances the iterator. Iterator also supports removing the last returned element using `remove()`. It is commonly used with collections such as ArrayList, LinkedList, HashSet, and TreeSet. For bidirectional traversal of a List, Java provides `ListIterator`."

---

# 46. Cheat Sheet 📋

## Definition

    Iterator
        =
    Interface for traversing collections

---

## Package

    java.util

---

## Main Methods

    hasNext()
    next()
    remove()
    forEachRemaining()

---

## Traversal

    Iterator<T> iterator =
        collection.iterator();

    while (iterator.hasNext()) {

        T value =
            iterator.next();
    }

---

## hasNext()

    Checks:
    Is another element available?

    Returns:
    boolean

---

## next()

    Returns:
    next element

    Can throw:
    NoSuchElementException

---

## remove()

    Removes:
    last element returned by next()

---

## forEachRemaining()

    Processes:
    all remaining elements

---

## Iterable Relationship

    Iterable
        |
        +---- iterator()
                  |
                  v
               Iterator

---

## Safe Removal

    iterator.next()
          |
          v
       condition
          |
          v
    iterator.remove()

---

## Fail-Fast

    External structural modification
              |
              v
    ConcurrentModificationException
              |
              v
        for many standard
        collection iterators

---

## ListIterator

    Iterator
        +
    backward traversal
        +
    add()
        +
    set()
        +
    indexes

---

# 47. Quick Revision ⚡

Remember:

    1. Iterator is an interface.

    2. Package:
       java.util

    3. Iterator traverses collections.

    4. iterator() creates an Iterator.

    5. hasNext() checks availability.

    6. next() returns the next element.

    7. next() advances the iterator.

    8. next() after the end can throw
       NoSuchElementException.

    9. remove() removes the last element
       returned by next().

    10. forEachRemaining() processes
        remaining elements.

    11. Iterator normally moves forward.

    12. ListIterator can move forward
        and backward.

    13. Iterator does not provide add().

    14. ListIterator provides add().

    15. Iterator does not provide set().

    16. ListIterator provides set().

    17. Iterable provides iterator().

    18. Iterable and Iterator are different.

    19. Iterable is the source of traversal.

    20. Iterator performs traversal.

    21. Map does not implement Collection.

    22. Map can be traversed through:
        keySet()
        values()
        entrySet()

    23. HashSet can be traversed using Iterator.

    24. TreeSet can be traversed using Iterator.

    25. HashSet does not guarantee iteration order.

    26. TreeSet iterator follows sorted order.

    27. Iterator does not automatically provide
        thread safety.

    28. Many standard collection iterators
        are fail-fast.

    29. External structural modification
        may cause ConcurrentModificationException.

    30. Iterator-based remove() is the
        correct removal mechanism during traversal.

---

# Final Memory Trick 🧠

Think:

              ITERABLE
                  |
            iterator()
                  |
                  v
              ITERATOR
                  |
          +-------+-------+
          |       |       |
      hasNext() next() remove()
                  |
                  v
             Traverse
             Collection

And remember:

    Iterable
        =
    "Can give me an Iterator."

    Iterator
        =
    "I traverse the elements."

    Iterator
        -> Forward

    ListIterator
        -> Forward + Backward

    Iterator
        -> General collection traversal

    ListIterator
        -> List-specific advanced traversal