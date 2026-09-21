17-ListIterator.md

# 17 — ListIterator Interface 🔄

> **Package:** `java.util`  
> **Type:** Interface  
> **Extends:** `Iterator<E>`  
> **Purpose:** Provides bidirectional traversal and modification of elements in a `List`.

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [What is ListIterator?](#2-what-is-listiterator)
3. [Why ListIterator Exists](#3-why-listiterator-exists)
4. [ListIterator Hierarchy](#4-listiterator-hierarchy)
5. [Iterator vs ListIterator](#5-iterator-vs-listiterator)
6. [Creating a ListIterator](#6-creating-a-listiterator)
7. [Basic Example](#7-basic-example)
8. [ListIterator Methods](#8-listiterator-methods)
9. [hasNext()](#9-hasnext)
10. [next()](#10-next)
11. [hasPrevious()](#11-hasprevious)
12. [previous()](#12-previous)
13. [nextIndex()](#13-nextindex)
14. [previousIndex()](#14-previousindex)
15. [remove()](#15-remove)
16. [set()](#16-set)
17. [add()](#17-add)
18. [Forward Traversal](#18-forward-traversal)
19. [Backward Traversal](#19-backward-traversal)
20. [Bidirectional Traversal](#20-bidirectional-traversal)
21. [ListIterator Cursor](#21-listiterator-cursor)
22. [Important Cursor Concept](#22-important-cursor-concept)
23. [next() and previous() Relationship](#23-next-and-previous-relationship)
24. [nextIndex() and previousIndex()](#24-nextindex-and-previousindex)
25. [Adding Elements](#25-adding-elements)
26. [Removing Elements](#26-removing-elements)
27. [Replacing Elements](#27-replacing-elements)
28. [ListIterator with ArrayList](#28-listiterator-with-arraylist)
29. [ListIterator with LinkedList](#29-listiterator-with-linkedlist)
30. [Starting from a Specific Index](#30-starting-from-a-specific-index)
31. [ListIterator with Strings](#31-listiterator-with-strings)
32. [Internal Working](#32-internal-working)
33. [ListIterator and Fail-Fast](#33-listiterator-and-fail-fast)
34. [ConcurrentModificationException](#34-concurrentmodificationexception)
35. [ListIterator and Generics](#35-listiterator-and-generics)
36. [ListIterator and Enhanced for Loop](#36-listiterator-and-enhanced-for-loop)
37. [ListIterator and forEach()](#37-listiterator-and-foreach)
38. [ListIterator Restrictions](#38-listiterator-restrictions)
39. [ListIterator vs Iterator](#39-listiterator-vs-iterator)
40. [ListIterator vs Enumeration](#40-listiterator-vs-enumeration)
41. [Advantages](#41-advantages)
42. [Limitations](#42-limitations)
43. [DSA Patterns](#43-dsa-patterns)
44. [DSA Pattern 1 — Bidirectional Traversal](#44-dsa-pattern-1--bidirectional-traversal)
45. [DSA Pattern 2 — In-place Modification](#45-dsa-pattern-2--in-place-modification)
46. [DSA Pattern 3 — Reverse Traversal](#46-dsa-pattern-3--reverse-traversal)
47. [Common Mistakes](#47-common-mistakes)
48. [Interview Traps](#48-interview-traps)
49. [Top Interview Questions](#49-top-interview-questions)
50. [30-Second Interview Answer](#50-30-second-interview-answer)
51. [Cheat Sheet](#51-cheat-sheet)
52. [Quick Revision](#52-quick-revision)

---

# 1. Introduction 🔄

`ListIterator` is an interface from the Java Collections Framework.

It is a specialized version of `Iterator` designed specifically for:

    List

It provides:

- Forward traversal
- Backward traversal
- Element replacement
- Element insertion
- Element removal
- Index information

The biggest difference is:

> `Iterator` moves mainly forward, while `ListIterator` can move in both directions.

---

# 2. What is ListIterator? 🎯

`ListIterator<E>` is an interface that extends:

    Iterator<E>

Therefore it inherits:

    hasNext()
    next()
    remove()

and adds additional capabilities:

    hasPrevious()
    previous()
    nextIndex()
    previousIndex()
    set()
    add()

Conceptually:

    Iterator
        |
        v
    ListIterator

So:

    ListIterator
        =
    Iterator
        +
    Bidirectional traversal
        +
    List modification
        +
    Index information

---

# 3. Why ListIterator Exists 🧠

Normal `Iterator` provides forward traversal:

    A -> B -> C -> D

But sometimes we need:

    A -> B -> C -> D

and then:

    D -> C -> B -> A

We may also need to:

    add
    remove
    replace

elements during traversal.

`ListIterator` provides all these capabilities.

---

# 4. ListIterator Hierarchy 🌳

The relationship is:

    Iterable
       |
    Collection
       |
      List
       |
       +-------------------+
       |                   |
    ArrayList          LinkedList
       |                   |
       +--------+----------+
                |
        listIterator()
                |
                v
          ListIterator
                |
                v
            Iterator

Important:

    ListIterator extends Iterator

It does NOT extend List.

It is a traversal interface used by List implementations.

---

# 5. Iterator vs ListIterator ⚖️

| Feature | Iterator | ListIterator |
|---|---|---|
| Interface | Yes | Yes |
| Forward traversal | Yes | Yes |
| Backward traversal | No | Yes |
| `hasNext()` | Yes | Yes |
| `next()` | Yes | Yes |
| `hasPrevious()` | No | Yes |
| `previous()` | No | Yes |
| `remove()` | Yes | Yes |
| `add()` | No | Yes |
| `set()` | No | Yes |
| `nextIndex()` | No | Yes |
| `previousIndex()` | No | Yes |
| Works with Set | Yes | No |
| Works with List | Yes | Yes |

Memory trick:

    Iterator
        ->
    Forward

    ListIterator
        ->
    Forward + Backward + Modify

---

# 6. Creating a ListIterator 🛠️

A List provides:

    listIterator()

Example:

    List<Integer> list =
        new ArrayList<>();

    list.add(10);
    list.add(20);
    list.add(30);

    ListIterator<Integer> iterator =
        list.listIterator();

Now we can traverse the list.

---

# 7. Basic Example 💻

    import java.util.ArrayList;
    import java.util.List;
    import java.util.ListIterator;

    public class Main {

        public static void main(String[] args) {

            List<Integer> list =
                new ArrayList<>();

            list.add(10);
            list.add(20);
            list.add(30);

            ListIterator<Integer> iterator =
                list.listIterator();

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

---

# 8. ListIterator Methods 📚

ListIterator provides:

    hasNext()
    next()

    hasPrevious()
    previous()

    nextIndex()
    previousIndex()

    remove()
    set()
    add()

Methods inherited from Iterator:

    hasNext()
    next()
    remove()

Methods specifically added by ListIterator:

    hasPrevious()
    previous()
    nextIndex()
    previousIndex()
    set()
    add()

---

# 9. hasNext() 🔍

Checks whether an element exists in the forward direction.

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

---

# 10. next() ➡️

Returns the next element and moves the cursor forward.

Example:

    List<Integer> list =
        List.of(10, 20, 30);

    ListIterator<Integer> iterator =
        list.listIterator();

    System.out.println(
        iterator.next()
    );

Output:

    10

Another call:

    iterator.next()

Output:

    20

Another:

    iterator.next()

Output:

    30

Calling `next()` when there is no next element throws:

    NoSuchElementException

---

# 11. hasPrevious() ⬅️

Checks whether an element exists in the backward direction.

Return type:

    boolean

Example:

    List<Integer> list =
        new ArrayList<>(
            List.of(10, 20, 30)
        );

    ListIterator<Integer> iterator =
        list.listIterator();

    iterator.next();
    iterator.next();

    if (iterator.hasPrevious()) {

        System.out.println(
            iterator.previous()
        );
    }

Output:

    20

---

# 12. previous() ⬅️

Returns the previous element and moves the cursor backward.

Example:

    List<Integer> list =
        new ArrayList<>(
            List.of(10, 20, 30)
        );

    ListIterator<Integer> iterator =
        list.listIterator();

    iterator.next();
    iterator.next();

    System.out.println(
        iterator.previous()
    );

Output:

    20

Important:

`previous()` returns the element immediately before the cursor.

---

# 13. nextIndex() 🔢

Returns the index of the element that would be returned by:

    next()

Example:

    List<Integer> list =
        List.of(10, 20, 30);

    ListIterator<Integer> iterator =
        list.listIterator();

Initially:

    nextIndex()
        -> 0

After:

    iterator.next();

the cursor moves forward.

Now:

    nextIndex()
        -> 1

So `nextIndex()` tells us:

> Which index will `next()` return?

---

# 14. previousIndex() 🔢

Returns the index of the element that would be returned by:

    previous()

Initially:

    previousIndex()
        -> -1

For:

    [10, 20, 30]

after one `next()`:

    cursor
       |
       v
    [10] [20] [30]

Now:

    previousIndex()
        -> 0

So `previousIndex()` tells us:

> Which index will `previous()` return?

---

# 15. remove() 🗑️

`remove()` removes the last element returned by:

    next()

or:

    previous()

Example:

    List<Integer> list =
        new ArrayList<>(
            List.of(10, 20, 30)
        );

    ListIterator<Integer> iterator =
        list.listIterator();

    while (iterator.hasNext()) {

        int value =
            iterator.next();

        if (value == 20) {
            iterator.remove();
        }
    }

Result:

    [10, 30]

Important:

You cannot arbitrarily call `remove()`.

It must follow an appropriate:

    next()

or:

    previous()

operation.

---

# 16. set() ✏️

`set(E e)` replaces the last element returned by:

    next()

or:

    previous()

Example:

    List<Integer> list =
        new ArrayList<>(
            List.of(10, 20, 30)
        );

    ListIterator<Integer> iterator =
        list.listIterator();

    while (iterator.hasNext()) {

        int value =
            iterator.next();

        if (value == 20) {
            iterator.set(200);
        }
    }

Result:

    [10, 200, 30]

Important:

`set()` replaces an existing element.

It does not change the list size.

---

# 17. add() ➕

`add(E e)` inserts an element at the iterator's current position.

Example:

    List<Integer> list =
        new ArrayList<>(
            List.of(10, 20, 30)
        );

    ListIterator<Integer> iterator =
        list.listIterator();

    iterator.next();

    iterator.add(15);

Result:

    [10, 15, 20, 30]

The new element is inserted before the element that would be returned by the next call to `next()`.

---

# 18. Forward Traversal ➡️

Use:

    hasNext()
    next()

Example:

    List<Integer> list =
        List.of(10, 20, 30, 40);

    ListIterator<Integer> iterator =
        list.listIterator();

    while (iterator.hasNext()) {

        System.out.println(
            iterator.next()
        );
    }

Output:

    10
    20
    30
    40

---

# 19. Backward Traversal ⬅️

Use:

    hasPrevious()
    previous()

Example:

    List<Integer> list =
        List.of(10, 20, 30, 40);

    ListIterator<Integer> iterator =
        list.listIterator(
            list.size()
        );

    while (iterator.hasPrevious()) {

        System.out.println(
            iterator.previous()
        );
    }

Output:

    40
    30
    20
    10

This is one of the major advantages of `ListIterator`.

---

# 20. Bidirectional Traversal 🔄

A `ListIterator` can move:

    Forward
       ↓
    10 -> 20 -> 30 -> 40
                          |
                          ↓
    Backward
    40 -> 30 -> 20 -> 10

Example:

    List<Integer> list =
        new ArrayList<>(
            List.of(10, 20, 30)
        );

    ListIterator<Integer> iterator =
        list.listIterator();

    System.out.println(
        iterator.next()
    );

    System.out.println(
        iterator.next()
    );

    System.out.println(
        iterator.previous()
    );

Output:

    10
    20
    20

---

# 21. ListIterator Cursor 📍

The most important concept is the cursor.

Suppose:

    [10] [20] [30]

Initially the cursor is before the first element:

    | [10] [20] [30]

After:

    next()

the cursor becomes:

    [10] | [20] [30]

After another:

    next()

it becomes:

    [10] [20] | [30]

After another:

    next()

it becomes:

    [10] [20] [30] |

The cursor represents the position between elements.

---

# 22. Important Cursor Concept 🧠

ListIterator does not point directly "on" an element.

Conceptually, the cursor sits between elements.

For:

    [10] [20] [30]

there are four possible cursor positions:

    | 10 | 20 | 30 |

More clearly:

         0       1       2       3
         |       |       |       |
        [10]    [20]    [30]

At position:

    0

there is no previous element.

The next element is:

    10

At position:

    1

previous is:

    10

next is:

    20

At position:

    3

previous is:

    30

there is no next element.

---

# 23. next() and previous() Relationship 🔄

Suppose:

    [10] [20] [30]

Cursor:

    [10] | [20] [30]

Call:

    next()

returns:

    20

Cursor becomes:

    [10] [20] | [30]

Now call:

    previous()

returns:

    20

Cursor returns to:

    [10] | [20] [30]

Therefore:

    previous()

returns the same element that was most recently returned by:

    next()

when called immediately afterward.

Similarly, if `previous()` is called and then `next()` immediately, `next()` returns the same element.

---

# 24. nextIndex() and previousIndex() 🔢

For:

    [10, 20, 30]

initial cursor:

    | 10 20 30

Indexes:

    nextIndex()
        -> 0

    previousIndex()
        -> -1

After:

    next()

cursor:

    10 | 20 30

Now:

    nextIndex()
        -> 1

    previousIndex()
        -> 0

After another:

    next()

cursor:

    10 20 | 30

Now:

    nextIndex()
        -> 2

    previousIndex()
        -> 1

At the end:

    10 20 30 |

Then:

    nextIndex()
        -> 3

    previousIndex()
        -> 2

---

# 25. Adding Elements ➕

Example:

    List<Integer> list =
        new ArrayList<>(
            List.of(10, 20, 30)
        );

    ListIterator<Integer> iterator =
        list.listIterator();

    iterator.next();

    iterator.add(15);

Result:

    [10, 15, 20, 30]

Important behavior:

After `add()`:

- The inserted element becomes part of the list.
- The cursor moves after the inserted element.
- A subsequent `remove()` or `set()` cannot immediately target the inserted element unless an appropriate `next()` or `previous()` operation occurs first.

---

# 26. Removing Elements 🗑️

Example:

    List<Integer> list =
        new ArrayList<>(
            List.of(10, 20, 30)
        );

    ListIterator<Integer> iterator =
        list.listIterator();

    iterator.next();
    iterator.next();

    iterator.remove();

Result:

    [10, 30]

The last element returned was:

    20

Therefore `remove()` removes:

    20

---

# 27. Replacing Elements ✏️

Use:

    set()

Example:

    List<String> names =
        new ArrayList<>(
            List.of(
                "Java",
                "Python",
                "C"
            )
        );

    ListIterator<String> iterator =
        names.listIterator();

    while (iterator.hasNext()) {

        String language =
            iterator.next();

        if (language.equals("C")) {

            iterator.set(
                "C++"
            );
        }
    }

Result:

    [Java, Python, C++]

---

# 28. ListIterator with ArrayList 📦

Example:

    ArrayList<Integer> list =
        new ArrayList<>(
            List.of(10, 20, 30)
        );

    ListIterator<Integer> iterator =
        list.listIterator();

    while (iterator.hasNext()) {

        System.out.println(
            iterator.next()
        );
    }

Backward:

    while (iterator.hasPrevious()) {

        System.out.println(
            iterator.previous()
        );
    }

Output:

    10
    20
    30

then:

    30
    20
    10

---

# 29. ListIterator with LinkedList 🔗

`LinkedList` also implements `List`.

Therefore it provides:

    listIterator()

Example:

    LinkedList<Integer> list =
        new LinkedList<>(
            List.of(10, 20, 30)
        );

    ListIterator<Integer> iterator =
        list.listIterator();

    while (iterator.hasNext()) {

        System.out.println(
            iterator.next()
        );
    }

ListIterator provides the same interface regardless of the List implementation.

---

# 30. Starting from a Specific Index 🎯

`List` provides:

    listIterator(int index)

Example:

    List<Integer> list =
        List.of(
            10, 20, 30, 40, 50
        );

    ListIterator<Integer> iterator =
        list.listIterator(2);

The cursor starts before:

    30

Conceptually:

    10 20 | 30 40 50

Therefore:

    iterator.next()

returns:

    30

And:

    iterator.previous()

returns:

    20

---

# 31. ListIterator with Strings 🔤

Example:

    List<String> languages =
        new ArrayList<>(
            List.of(
                "Java",
                "Python",
                "C++"
            )
        );

    ListIterator<String> iterator =
        languages.listIterator();

    while (iterator.hasNext()) {

        String language =
            iterator.next();

        System.out.println(
            language
        );
    }

Output:

    Java
    Python
    C++

---

# 32. Internal Working ⚙️

The exact implementation depends on the List.

For `ArrayList`, the iterator can work using array indexes.

Conceptually:

    [10][20][30][40]
       ^
       |
     cursor

For `LinkedList`, traversal is based on linked nodes.

Therefore:

    ArrayList
        |
        +--> index-based traversal

    LinkedList
        |
        +--> node-based traversal

But the programmer uses the same:

    ListIterator

interface.

This is abstraction.

---

# 33. ListIterator and Fail-Fast ⚡

Many standard List implementations provide fail-fast iterators.

Example:

    List<Integer> list =
        new ArrayList<>(
            List.of(10, 20, 30)
        );

    ListIterator<Integer> iterator =
        list.listIterator();

    list.add(40);

    iterator.next();

This can result in:

    ConcurrentModificationException

because the list was structurally modified outside the iterator.

---

# 34. ConcurrentModificationException ⚠️

Avoid modifying the list directly while traversing with a fail-fast `ListIterator`.

Unsafe:

    while (iterator.hasNext()) {

        int value =
            iterator.next();

        if (value == 20) {
            list.remove(
                Integer.valueOf(20)
            );
        }
    }

Better:

    while (iterator.hasNext()) {

        int value =
            iterator.next();

        if (value == 20) {
            iterator.remove();
        }
    }

The iterator remains aware of its own modification.

---

# 35. ListIterator and Generics 🧬

ListIterator supports generics.

Example:

    ListIterator<String> iterator =
        list.listIterator();

Then:

    iterator.next()

returns:

    String

Without generics:

    ListIterator iterator =
        list.listIterator();

the result is treated as:

    Object

Generics provide:

    Type Safety
    Cleaner Code
    No Unnecessary Casting

---

# 36. ListIterator and Enhanced for Loop 🔁

Enhanced for:

    for (Integer value : list) {

        System.out.println(value);
    }

is useful for simple traversal.

But it does not directly expose:

    previous()
    nextIndex()
    previousIndex()
    set()
    add()

If advanced bidirectional traversal or iterator-controlled modification is needed, use:

    ListIterator

---

# 37. ListIterator and forEach() 🔁

Using:

    list.forEach(
        value -> System.out.println(value)
    );

is concise.

But `forEach()` does not expose the ListIterator controls:

    previous()
    nextIndex()
    previousIndex()
    set()
    add()

For fine-grained traversal:

    ListIterator

is more suitable.

---

# 38. ListIterator Restrictions ⚠️

## 1. Only for List

You cannot directly use:

    ListIterator

with:

    HashSet
    TreeSet

because they are not Lists.

---

## 2. Must Follow Iterator State Rules

You cannot arbitrarily call:

    remove()
    set()

without an appropriate traversal operation.

---

## 3. No Direct Random Access

ListIterator itself does not provide:

    get(index)

It provides:

    nextIndex()
    previousIndex()

for index information.

---

## 4. Concurrent Modification

External structural modification may cause:

    ConcurrentModificationException

for fail-fast implementations.

---

# 39. ListIterator vs Iterator ⚖️

| Feature | Iterator | ListIterator |
|---|---|---|
| Parent interface | None | Iterator |
| Forward | Yes | Yes |
| Backward | No | Yes |
| `next()` | Yes | Yes |
| `previous()` | No | Yes |
| `remove()` | Yes | Yes |
| `set()` | No | Yes |
| `add()` | No | Yes |
| Index information | No | Yes |
| Works with Set | Yes | No |
| Works with List | Yes | Yes |

Memory:

    Iterator
        =
    Basic traversal

    ListIterator
        =
    Advanced List traversal

---

# 40. ListIterator vs Enumeration ⚖️

| Feature | ListIterator | Enumeration |
|---|---|---|
| Modern collections | Yes | Legacy |
| Forward traversal | Yes | Yes |
| Backward traversal | Yes | No |
| Remove | Yes | No |
| Add | Yes | No |
| Replace | Yes | No |
| Index information | Yes | No |

`ListIterator` is much more powerful.

---

# 41. Advantages ✅

## 1. Bidirectional Traversal

Can move:

    Forward
    Backward

---

## 2. Element Modification

Supports:

    add()
    set()
    remove()

---

## 3. Index Information

Provides:

    nextIndex()
    previousIndex()

---

## 4. List-Specific

Designed specifically for:

    List

---

## 5. Type Safe

Supports:

    ListIterator<T>

---

# 42. Limitations ⚠️

## 1. Only Works with List

Not available for:

    Set
    Map directly

---

## 2. More Complex Than Iterator

There are more state rules to understand.

---

## 3. No Random Access

It does not provide:

    get(index)

---

## 4. External Structural Modification

Can trigger:

    ConcurrentModificationException

with fail-fast implementations.

---

# 43. DSA Patterns 🧩

Important DSA uses/concepts:

    1. Bidirectional traversal
    2. Reverse traversal
    3. In-place modification
    4. Ordered List processing
    5. Cursor-based traversal

ListIterator itself is less commonly the central DSA tool than structures such as:

    ArrayList
    LinkedList
    Deque
    Stack

But it is important for understanding Java's List traversal and modification model.

---

# 44. DSA Pattern 1 — Bidirectional Traversal 🔄

Given:

    [10, 20, 30, 40]

Forward:

    10 -> 20 -> 30 -> 40

Backward:

    40 -> 30 -> 20 -> 10

Code:

    ListIterator<Integer> iterator =
        list.listIterator();

    while (iterator.hasNext()) {

        System.out.println(
            iterator.next()
        );
    }

    while (iterator.hasPrevious()) {

        System.out.println(
            iterator.previous()
        );
    }

---

# 45. DSA Pattern 2 — In-place Modification ✏️

Suppose:

    [10, 20, 30]

Replace every value with its double.

Concept:

    next()
      |
      v
    value
      |
      v
    set(value * 2)

Code:

    ListIterator<Integer> iterator =
        list.listIterator();

    while (iterator.hasNext()) {

        int value =
            iterator.next();

        iterator.set(
            value * 2
        );
    }

Result:

    [20, 40, 60]

---

# 46. DSA Pattern 3 — Reverse Traversal 🔙

Start at:

    list.size()

Then:

    hasPrevious()
    previous()

Example:

    ListIterator<Integer> iterator =
        list.listIterator(
            list.size()
        );

    while (iterator.hasPrevious()) {

        System.out.println(
            iterator.previous()
        );
    }

This gives reverse order without creating another list.

---

# 47. Common Mistakes ⚠️

## Mistake 1

Trying:

    listIterator()

on a Set.

Not supported.

---

## Mistake 2

Calling:

    previous()

at the beginning.

Result:

    NoSuchElementException

---

## Mistake 3

Calling:

    next()

at the end.

Result:

    NoSuchElementException

---

## Mistake 4

Calling:

    remove()

before `next()` or `previous()`.

This violates the iterator state rules and can result in:

    IllegalStateException

---

## Mistake 5

Calling `set()` without a valid last `next()` or `previous()`.

Can result in:

    IllegalStateException

---

## Mistake 6

Calling `set()` immediately after `add()`.

This is not permitted because `add()` invalidates the last returned element for `set()`/`remove()` purposes.

---

## Mistake 7

Modifying the List directly during iteration.

May cause:

    ConcurrentModificationException

---

# 48. Interview Traps 🎤

## Trap 1

> Is ListIterator a class?

No.

It is an interface.

---

## Trap 2

> Does ListIterator extend Iterator?

Yes.

---

## Trap 3

> Can Iterator move backward?

No.

---

## Trap 4

> Can ListIterator move backward?

Yes.

Using:

    hasPrevious()
    previous()

---

## Trap 5

> Can ListIterator add elements?

Yes.

Using:

    add()

---

## Trap 6

> Can ListIterator replace elements?

Yes.

Using:

    set()

---

## Trap 7

> Can ListIterator be used with HashSet?

No.

It is designed for Lists.

---

## Trap 8

> What does nextIndex() return?

The index of the element that would be returned by the next call to `next()`.

---

## Trap 9

> What does previousIndex() return?

The index of the element that would be returned by the next call to `previous()`.

---

## Trap 10

> What happens when previous() is called at the beginning?

    NoSuchElementException

---

## Trap 11

> What happens when next() is called at the end?

    NoSuchElementException

---

## Trap 12

> Can remove() be called anytime?

No.

It must follow an appropriate `next()` or `previous()` call and must not be invalidated by `add()` or another removal.

---

## Trap 13

> Can set() be called anytime?

No.

It requires a valid last element returned by `next()` or `previous()`.

---

## Trap 14

> What is the major difference between Iterator and ListIterator?

ListIterator provides:

    Backward traversal
    add()
    set()
    Index information

---

# 49. Top Interview Questions 🎤

## Q1. What is ListIterator?

`ListIterator` is an interface used to traverse and modify elements of a List in both forward and backward directions.

---

## Q2. Which interface does ListIterator extend?

    Iterator<E>

---

## Q3. Which package contains ListIterator?

    java.util

---

## Q4. Can ListIterator be used with Set?

No.

It is designed for Lists.

---

## Q5. Can ListIterator move backward?

Yes.

Using:

    hasPrevious()
    previous()

---

## Q6. What is the difference between next() and previous()?

`next()` returns the next element and moves forward.

`previous()` returns the previous element and moves backward.

---

## Q7. What does add() do?

It inserts an element at the current cursor position.

---

## Q8. What does set() do?

It replaces the last element returned by `next()` or `previous()`.

---

## Q9. What does remove() do?

It removes the last element returned by `next()` or `previous()`.

---

## Q10. What does nextIndex() return?

The index of the element that would be returned by `next()`.

---

## Q11. What does previousIndex() return?

The index of the element that would be returned by `previous()`.

---

## Q12. What happens if next() is called at the end?

    NoSuchElementException

---

## Q13. What happens if previous() is called at the beginning?

    NoSuchElementException

---

## Q14. What happens if remove() is called before next() or previous()?

    IllegalStateException

---

## Q15. What happens if set() is called without a valid last returned element?

    IllegalStateException

---

## Q16. Can add() be followed immediately by set()?

No.

After `add()`, there is no valid last element returned by `next()` or `previous()` for `set()` to modify.

---

## Q17. Can ListIterator traverse an ArrayList?

Yes.

---

## Q18. Can ListIterator traverse LinkedList?

Yes.

---

## Q19. Is ListIterator thread-safe?

No.

The interface itself does not provide thread safety.

---

## Q20. What is the main advantage of ListIterator over Iterator?

It supports bidirectional traversal and additional List-specific modification operations.

---

# 50. 30-Second Interview Answer 🎯

If the interviewer asks:

> "What is ListIterator in Java?"

Answer:

> "`ListIterator` is an interface in `java.util` that extends `Iterator` and is specifically designed for List implementations. Unlike Iterator, it supports traversal in both forward and backward directions using `next()` and `previous()`. It also provides `nextIndex()`, `previousIndex()`, `add()`, and `set()` in addition to `remove()`. It can be created using `listIterator()` or `listIterator(index)`. It is useful when we need advanced traversal or modification of a List while maintaining iterator state."

---

# 51. Cheat Sheet 📋

## Definition

    ListIterator
        =
    Advanced Iterator for List

---

## Relationship

    Iterator
        |
        v
    ListIterator

---

## Package

    java.util

---

## Forward

    hasNext()
    next()

---

## Backward

    hasPrevious()
    previous()

---

## Index

    nextIndex()
    previousIndex()

---

## Modification

    add()
    remove()
    set()

---

## Create

    ListIterator<T> iterator =
        list.listIterator();

---

## Start at Index

    ListIterator<T> iterator =
        list.listIterator(index);

---

## Forward Traversal

    while (iterator.hasNext()) {

        System.out.println(
            iterator.next()
        );
    }

---

## Backward Traversal

    while (iterator.hasPrevious()) {

        System.out.println(
            iterator.previous()
        );
    }

---

## Replace

    iterator.next();
    iterator.set(value);

---

## Remove

    iterator.next();
    iterator.remove();

---

## Add

    iterator.add(value);

---

## Important Exceptions

    next() at end
        ->
    NoSuchElementException

    previous() at beginning
        ->
    NoSuchElementException

    invalid remove()
        ->
    IllegalStateException

    invalid set()
        ->
    IllegalStateException

    external structural modification
        ->
    ConcurrentModificationException
    for fail-fast implementations

---

# 52. Quick Revision ⚡

Remember:

    1. ListIterator is an interface.

    2. Package:
       java.util

    3. ListIterator extends Iterator.

    4. ListIterator works with List.

    5. It supports forward traversal.

    6. It supports backward traversal.

    7. Forward:
       hasNext()
       next()

    8. Backward:
       hasPrevious()
       previous()

    9. Index:
       nextIndex()
       previousIndex()

    10. Modification:
        add()
        remove()
        set()

    11. Iterator cannot move backward.

    12. ListIterator can move backward.

    13. Iterator cannot add elements.

    14. ListIterator can add elements.

    15. Iterator cannot set elements.

    16. ListIterator can set elements.

    17. ListIterator cannot be used with Set.

    18. ArrayList supports ListIterator.

    19. LinkedList supports ListIterator.

    20. listIterator() starts at index 0.

    21. listIterator(index) starts at the
        specified cursor position.

    22. next() returns the element after
        the cursor.

    23. previous() returns the element before
        the cursor.

    24. nextIndex() gives the index of the
        next element.

    25. previousIndex() gives the index of
        the previous element.

    26. next() at the end can throw
        NoSuchElementException.

    27. previous() at the beginning can throw
        NoSuchElementException.

    28. Invalid remove() can throw
        IllegalStateException.

    29. Invalid set() can throw
        IllegalStateException.

    30. External structural modification may
        cause ConcurrentModificationException
        for fail-fast implementations.

    31. ListIterator is useful for
        bidirectional traversal.

    32. ListIterator is useful for
        in-place List modification.

---

# Final Memory Trick 🧠

Think of:

    Iterator

as:

    Forward

while:

    ListIterator

is:

    Forward
       +
    Backward
       +
    Add
       +
    Remove
       +
    Replace
       +
    Index

Visualize:

          previous()
              <---
               |
    [10] [20] [30] [40]
       ---> ---> --->
              |
             next()

And remember:

    Iterator
        ->
    General traversal

    ListIterator
        ->
    Advanced List traversal