# 03 — Collection Interface

> **Package:** `java.util`  
> **Since:** Java 1.2  
> **Parent Interface:** `Iterable<E>`  
> **Type:** Generic Interface  
> **Purpose:** Represents a group of objects as a single unit.

---

# 📚 Table of Contents

1. [Introduction](#1-introduction)
2. [Why Collection Interface?](#2-why-collection-interface)
3. [Collection Hierarchy](#3-collection-hierarchy)
4. [Collection vs Collections](#4-collection-vs-collections)
5. [Collection Interface Declaration](#5-collection-interface-declaration)
6. [Important Characteristics](#6-important-characteristics)
7. [Collection Does Not Store Data Directly](#7-collection-does-not-store-data-directly)
8. [Creating Collection References](#8-creating-collection-references)
9. [Collection Methods](#9-collection-methods)
10. [Adding Elements](#10-adding-elements)
11. [Removing Elements](#11-removing-elements)
12. [Searching Elements](#12-searching-elements)
13. [Size and State Methods](#13-size-and-state-methods)
14. [Bulk Operations](#14-bulk-operations)
15. [Array Conversion](#15-array-conversion)
16. [Iteration](#16-iteration)
17. [Iterator](#17-iterator)
18. [forEach](#18-foreach)
19. [Spliterator](#19-spliterator)
20. [Stream Support](#20-stream-support)
21. [Optional Operations](#21-optional-operations)
22. [UnsupportedOperationException](#22-unsupportedoperationexception)
23. [Null Handling](#23-null-handling)
24. [Duplicate Handling](#24-duplicate-handling)
25. [Ordering](#25-ordering)
26. [Collection Implementations](#26-collection-implementations)
27. [List vs Set vs Queue](#27-list-vs-set-vs-queue)
28. [Internal Working](#28-internal-working)
29. [Generic Nature](#29-generic-nature)
30. [Polymorphism](#30-polymorphism)
31. [Common Mistakes](#31-common-mistakes)
32. [Interview Traps](#32-interview-traps)
33. [DSA Relevance](#33-dsa-relevance)
34. [DSA Practice Questions](#34-dsa-practice-questions)
35. [Top Interview Questions](#35-top-interview-questions)
36. [30-Second Interview Answer](#36-30-second-interview-answer)
37. [Cheat Sheet](#37-cheat-sheet)
38. [Quick Revision](#38-quick-revision)

---

# 1. Introduction

The `Collection` interface is one of the most important interfaces of the Java Collection Framework.

It represents a **group of objects as a single unit**.

The interface belongs to:

    java.util.Collection

Basic relationship:

    Iterable<E>
         |
         v
    Collection<E>

`Collection` provides common operations such as:

- Adding elements
- Removing elements
- Searching elements
- Checking size
- Checking whether empty
- Checking whether an element exists
- Removing all elements
- Comparing collections
- Converting collections to arrays
- Iterating over elements

Example:

    import java.util.*;

    public class Main {
        public static void main(String[] args) {

            Collection<String> names = new ArrayList<>();

            names.add("Java");
            names.add("Spring");
            names.add("React");

            System.out.println(names);
        }
    }

Output:

    [Java, Spring, React]

The important idea is:

> `Collection` defines common behavior, while concrete classes provide the actual implementation.

---

# 2. Why Collection Interface?

Before the Collection Framework, programmers frequently used arrays for storing groups of objects.

Example:

    String[] names = new String[3];

The major problem is that an array has a fixed length.

Once created:

    String[] names = new String[3];

Its length cannot dynamically increase.

Collections solve this problem by providing dynamic data structures.

Example:

    Collection<String> names = new ArrayList<>();

    names.add("A");
    names.add("B");
    names.add("C");
    names.add("D");

The collection can grow dynamically depending on its implementation.

---

## Programming to an Interface

Instead of:

    ArrayList<String> names = new ArrayList<>();

we can write:

    Collection<String> names = new ArrayList<>();

Now the variable depends on the `Collection` abstraction rather than directly depending on `ArrayList`.

The implementation can later be changed:

    Collection<String> names = new HashSet<>();

or:

    Collection<String> names = new LinkedList<>();

The code using common Collection operations can remain unchanged.

This is called:

> **Programming to an interface rather than an implementation.**

---

# 3. Collection Hierarchy

The simplified Collection hierarchy is:

    Iterable<E>
        |
        v
    Collection<E>
        |
        +----------------+----------------+
        |                |                |
       List              Set             Queue
        |                |                |
        |                |                +--- PriorityQueue
        |                |
        |                +--- HashSet
        |                |
        |                +--- LinkedHashSet
        |                |
        |                +--- SortedSet
        |                       |
        |                       +--- NavigableSet
        |                               |
        |                               +--- TreeSet
        |
        +--- ArrayList
        |
        +--- LinkedList

Important:

`Map` is **NOT** a child of `Collection`.

The hierarchy is:

    Iterable
        |
    Collection
        |
    List / Set / Queue

But:

    Map

is a separate hierarchy.

Example:

    Map<String, Integer> map = new HashMap<>();

`HashMap` does not implement `Collection`.

---

# 4. Collection vs Collections

These two names are commonly confused.

## Collection

`Collection` is an **interface**.

    Collection<String> names = new ArrayList<>();

It defines common operations for groups of objects.

---

## Collections

`Collections` is a **utility class**.

    Collections.sort(list);
    Collections.reverse(list);
    Collections.max(list);
    Collections.min(list);

Package:

    java.util.Collections

---

## Difference

    Collection
        |
        +--- Interface
        +--- Represents a group of objects
        +--- Parent of List, Set and Queue

    Collections
        |
        +--- Utility class
        +--- Provides static helper methods
        +--- Sorting
        +--- Searching
        +--- Reversing
        +--- Synchronization
        +--- Unmodifiable wrappers

Memory trick:

    Collection  = What a collection IS

    Collections = Utility methods for collections

---

# 5. Collection Interface Declaration

Conceptually, the interface is:

    public interface Collection<E> extends Iterable<E>

`E` represents the element type.

For example:

    Collection<String>

means:

    E = String

And:

    Collection<Integer>

means:

    E = Integer

The actual JDK interface contains many methods, including default methods introduced in later Java versions.

---

# 6. Important Characteristics

The `Collection` interface:

- Is generic
- Extends `Iterable`
- Represents a group of elements
- Provides common operations
- Does not specify one particular data structure
- Does not itself determine ordering
- Does not itself determine duplicate behavior
- Does not itself determine null behavior
- Is implemented by multiple collection types
- Supports polymorphism

Important:

> The behavior regarding ordering, duplicates, nulls and performance depends on the concrete implementation.

For example:

    Collection<Integer> a = new ArrayList<>();

    Collection<Integer> b = new HashSet<>();

Both are `Collection`, but their behavior is different.

---

# 7. Collection Does Not Store Data Directly

This is a very important concept.

`Collection` is an interface.

It does not contain a concrete internal data structure like:

    Array
    Linked Nodes
    Hash Table
    Tree

Instead, the implementing class decides how elements are stored.

Example:

    Collection<Integer> numbers = new ArrayList<>();

Here:

    Collection
        |
        | reference
        v
    ArrayList object
        |
        v
    internal dynamic array

Another example:

    Collection<Integer> numbers = new HashSet<>();

Here:

    Collection
        |
        | reference
        v
    HashSet object
        |
        v
    hash-table-based structure

Therefore:

> The reference type determines what operations are accessible, while the actual object determines the implementation and runtime behavior.

---

# 8. Creating Collection References

A Collection reference can point to different implementations.

Example:

    Collection<Integer> numbers;

    numbers = new ArrayList<>();

Another implementation:

    Collection<Integer> numbers = new HashSet<>();

Another:

    Collection<Integer> numbers = new LinkedList<>();

Another:

    Collection<Integer> numbers = new PriorityQueue<>();

This demonstrates polymorphism.

---

## Why Can't We Do This?

We cannot directly instantiate an interface.

Invalid:

    Collection<Integer> numbers = new Collection<>();

Reason:

`Collection` is an interface and does not provide a concrete object implementation.

We need an implementing class:

    Collection<Integer> numbers = new ArrayList<>();

---

# 9. Collection Methods

The major methods can be grouped into:

    1. Adding
    2. Removing
    3. Searching
    4. Size/state
    5. Bulk operations
    6. Array conversion
    7. Iteration
    8. Stream/Spliterator support

---

# 10. Adding Elements

## 10.1 add()

Syntax:

    boolean add(E e);

Adds one element to the collection.

Example:

    Collection<String> names = new ArrayList<>();

    names.add("Java");
    names.add("Spring");

    System.out.println(names);

Output:

    [Java, Spring]

---

## Return Value

`add()` returns a boolean.

Example:

    boolean result = names.add("Java");

    System.out.println(result);

For most mutable collections:

    true

However, the exact behavior depends on the collection implementation.

---

## Important Point

For a `Set`, if the element already exists, `add()` may return `false`.

Example:

    Set<Integer> numbers = new HashSet<>();

    System.out.println(numbers.add(10));
    System.out.println(numbers.add(10));

Output:

    true
    false

The second `10` is not added because a Set does not permit duplicate elements.

---

## 10.2 addAll()

Adds all elements from another collection.

Syntax:

    boolean addAll(Collection<? extends E> c);

Example:

    Collection<Integer> numbers = new ArrayList<>();

    numbers.add(10);
    numbers.add(20);

    Collection<Integer> moreNumbers = new ArrayList<>();

    moreNumbers.add(30);
    moreNumbers.add(40);

    numbers.addAll(moreNumbers);

    System.out.println(numbers);

Output:

    [10, 20, 30, 40]

---

# 11. Removing Elements

## 11.1 remove(Object)

Removes one matching element.

Syntax:

    boolean remove(Object o);

Example:

    Collection<String> names = new ArrayList<>();

    names.add("Java");
    names.add("Spring");
    names.add("React");

    names.remove("Spring");

    System.out.println(names);

Output:

    [Java, React]

---

## Important

For collections containing objects:

    remove(Object)

removes the matching object.

Example:

    Collection<Integer> numbers = new ArrayList<>();

    numbers.add(10);
    numbers.add(20);
    numbers.add(30);

    numbers.remove(Integer.valueOf(20));

    System.out.println(numbers);

Output:

    [10, 30]

---

## Integer Removal Trap

This is a famous interview trap.

Consider:

    List<Integer> numbers = new ArrayList<>();

    numbers.add(10);
    numbers.add(20);
    numbers.add(30);

    numbers.remove(1);

The expression:

    remove(1)

matches:

    remove(int index)

because `List` has an overloaded `remove(int index)` method.

So it removes the element at index `1`.

Result:

    [10, 30]

To remove the integer value `1`, use:

    numbers.remove(Integer.valueOf(1));

---

## 11.2 removeAll()

Removes all elements from the current collection that are also present in another collection.

Example:

    Collection<Integer> numbers = new ArrayList<>();

    numbers.add(10);
    numbers.add(20);
    numbers.add(30);
    numbers.add(40);

    Collection<Integer> removeNumbers = new ArrayList<>();

    removeNumbers.add(20);
    removeNumbers.add(40);

    numbers.removeAll(removeNumbers);

    System.out.println(numbers);

Output:

    [10, 30]

---

## 11.3 removeIf()

Removes elements that satisfy a condition.

Syntax:

    boolean removeIf(Predicate<? super E> filter);

Example:

    Collection<Integer> numbers = new ArrayList<>();

    numbers.add(10);
    numbers.add(15);
    numbers.add(20);
    numbers.add(25);

    numbers.removeIf(n -> n % 2 == 0);

    System.out.println(numbers);

Output:

    [15, 25]

Here:

    n -> n % 2 == 0

means:

> Remove the element if it is even.

---

## 11.4 clear()

Removes all elements.

Example:

    Collection<String> names = new ArrayList<>();

    names.add("Java");
    names.add("Spring");
    names.add("React");

    names.clear();

    System.out.println(names);

Output:

    []

Important:

`clear()` removes elements from the collection.

It does not destroy the collection object itself.

---

# 12. Searching Elements

## 12.1 contains()

Checks whether an element exists.

Syntax:

    boolean contains(Object o);

Example:

    Collection<String> names = new ArrayList<>();

    names.add("Java");
    names.add("Spring");

    System.out.println(names.contains("Java"));
    System.out.println(names.contains("Python"));

Output:

    true
    false

---

## Internal Concept

For:

    contains("Java")

the implementation determines whether the requested object exists.

For many collections, this operation relies on:

    equals()

For hash-based collections, `hashCode()` also plays an important role.

---

## 12.2 containsAll()

Checks whether all elements of another collection are present.

Example:

    Collection<Integer> numbers = new ArrayList<>();

    numbers.add(10);
    numbers.add(20);
    numbers.add(30);

    Collection<Integer> search = new ArrayList<>();

    search.add(10);
    search.add(20);

    System.out.println(numbers.containsAll(search));

Output:

    true

If:

    search.add(50);

then:

    numbers.containsAll(search)

returns:

    false

---

# 13. Size and State Methods

## 13.1 size()

Returns the number of elements.

Example:

    Collection<String> names = new ArrayList<>();

    names.add("A");
    names.add("B");
    names.add("C");

    System.out.println(names.size());

Output:

    3

---

## 13.2 isEmpty()

Checks whether the collection contains zero elements.

Example:

    Collection<String> names = new ArrayList<>();

    System.out.println(names.isEmpty());

Output:

    true

After:

    names.add("Java");

then:

    System.out.println(names.isEmpty());

Output:

    false

---

# 14. Bulk Operations

Bulk operations work with multiple elements.

Important methods:

    addAll()
    removeAll()
    retainAll()
    containsAll()

---

## 14.1 retainAll()

Keeps only elements that are also present in another collection.

Example:

    Collection<Integer> numbers = new ArrayList<>();

    numbers.add(10);
    numbers.add(20);
    numbers.add(30);
    numbers.add(40);

    Collection<Integer> keep = new ArrayList<>();

    keep.add(20);
    keep.add(40);

    numbers.retainAll(keep);

    System.out.println(numbers);

Output:

    [20, 40]

Think:

    removeAll()
        = remove matching elements

    retainAll()
        = keep matching elements

---

## Bulk Operations Summary

    addAll()
        Add everything from another collection.

    removeAll()
        Remove everything that matches another collection.

    retainAll()
        Keep only elements that match another collection.

    containsAll()
        Check whether all elements exist.

---

# 15. Array Conversion

A Collection can be converted into an array.

There are two important `toArray()` forms.

---

## 15.1 Object[] toArray()

Example:

    Collection<String> names = new ArrayList<>();

    names.add("Java");
    names.add("Spring");

    Object[] arr = names.toArray();

    for (Object value : arr) {
        System.out.println(value);
    }

Output:

    Java
    Spring

Return type:

    Object[]

---

## 15.2 Generic toArray(T[])

Example:

    Collection<String> names = new ArrayList<>();

    names.add("Java");
    names.add("Spring");

    String[] arr = names.toArray(new String[0]);

    for (String value : arr) {
        System.out.println(value);
    }

Output:

    Java
    Spring

This is useful when we need a specific array type.

---

## 15.3 Modern toArray(IntFunction)

Modern Java also provides:

    <T> T[] toArray(IntFunction<T[]> generator);

Example:

    Collection<String> names = new ArrayList<>();

    names.add("Java");
    names.add("Spring");

    String[] arr = names.toArray(String[]::new);

    System.out.println(Arrays.toString(arr));

Output:

    [Java, Spring]

---

# 16. Iteration

Because `Collection` extends `Iterable`, its elements can be iterated using the enhanced for-loop.

Example:

    Collection<String> names = new ArrayList<>();

    names.add("Java");
    names.add("Spring");
    names.add("React");

    for (String name : names) {
        System.out.println(name);
    }

Output:

    Java
    Spring
    React

---

# 17. Iterator

`Collection` inherits:

    iterator()

from `Iterable`.

Example:

    Collection<String> names = new ArrayList<>();

    names.add("Java");
    names.add("Spring");
    names.add("React");

    Iterator<String> iterator = names.iterator();

    while (iterator.hasNext()) {
        String name = iterator.next();
        System.out.println(name);
    }

Output:

    Java
    Spring
    React

---

## Iterator Methods

The important methods are:

    hasNext()
    next()
    remove()

Example:

    Iterator<Integer> iterator = numbers.iterator();

    while (iterator.hasNext()) {

        Integer number = iterator.next();

        if (number % 2 == 0) {
            iterator.remove();
        }
    }

This safely removes elements during iteration.

---

# 18. forEach

Because `Collection` extends `Iterable`, we can use `forEach()`.

Example:

    Collection<String> names = new ArrayList<>();

    names.add("Java");
    names.add("Spring");
    names.add("React");

    names.forEach(name -> System.out.println(name));

Output:

    Java
    Spring
    React

Method reference can also be used:

    names.forEach(System.out::println);

---

# 19. Spliterator

`Collection` also supports:

    spliterator()

A `Spliterator` is designed for traversing and potentially partitioning elements.

Example:

    Collection<Integer> numbers = new ArrayList<>();

    numbers.add(10);
    numbers.add(20);
    numbers.add(30);

    Spliterator<Integer> spliterator = numbers.spliterator();

    spliterator.forEachRemaining(System.out::println);

Output:

    10
    20
    30

Important:

`Spliterator` is particularly useful for stream processing and parallel processing.

---

# 20. Stream Support

Collection provides methods for creating streams.

Important methods:

    stream()
    parallelStream()

Example:

    Collection<Integer> numbers = new ArrayList<>();

    numbers.add(10);
    numbers.add(20);
    numbers.add(30);
    numbers.add(40);

    long count = numbers.stream()
                        .filter(n -> n > 20)
                        .count();

    System.out.println(count);

Output:

    2

---

## parallelStream()

Example:

    Collection<Integer> numbers = new ArrayList<>();

    numbers.add(10);
    numbers.add(20);
    numbers.add(30);

    numbers.parallelStream()
           .forEach(System.out::println);

Important:

Parallel execution does not guarantee the same output order as sequential iteration for unordered operations.

---

# 21. Optional Operations

Some Collection operations are called **optional operations**.

This does not mean the method itself may be absent.

It means:

> An implementation is allowed to not support a modifying operation.

For example:

    add()
    remove()
    clear()
    removeAll()
    retainAll()

may be unsupported by an immutable or unmodifiable collection.

---

# 22. UnsupportedOperationException

Consider:

    List<String> names = List.of("Java", "Spring");

    names.add("React");

This throws:

    UnsupportedOperationException

Why?

Because `List.of()` creates an unmodifiable list.

Similarly:

    Collection<String> names = Collections.unmodifiableCollection(
        new ArrayList<>()
    );

Attempting to modify it:

    names.add("Java");

can throw:

    UnsupportedOperationException

Important distinction:

    Interface defines the operation.

    Implementation decides whether modification is supported.

---

# 23. Null Handling

The `Collection` interface itself does not universally prohibit or allow `null`.

The behavior depends on the implementation.

Example:

    Collection<String> names = new ArrayList<>();

    names.add(null);

This is allowed by `ArrayList`.

But different collections may have different restrictions.

Therefore:

> Never assume that every Collection implementation handles `null` in the same way.

Always check the concrete implementation's contract.

---

# 24. Duplicate Handling

The `Collection` interface itself does not say:

> Duplicates are always allowed.

or:

> Duplicates are always forbidden.

The implementation determines this behavior.

Example:

    Collection<Integer> list = new ArrayList<>();

    list.add(10);
    list.add(10);

Result:

    [10, 10]

But:

    Collection<Integer> set = new HashSet<>();

    set.add(10);
    set.add(10);

Result:

    [10]

Therefore:

    List
        -> generally allows duplicates

    Set
        -> does not allow duplicate elements

The Collection abstraction itself does not impose the duplicate policy.

---

# 25. Ordering

The `Collection` interface itself does not guarantee a particular ordering.

Different implementations have different ordering characteristics.

Example:

    ArrayList
        -> maintains insertion order

    LinkedHashSet
        -> maintains insertion order

    HashSet
        -> does not guarantee insertion order

    TreeSet
        -> maintains sorted order

Therefore:

> Ordering is an implementation-specific property.

---

# 26. Collection Implementations

Important implementations include:

## List

    ArrayList
    LinkedList
    Vector
    Stack

Characteristics:

- Ordered
- Index-based operations available through List
- Usually allows duplicates

---

## Set

    HashSet
    LinkedHashSet
    TreeSet

Characteristics:

- Does not permit duplicate elements
- Ordering depends on implementation

---

## Queue

    PriorityQueue
    LinkedList

Characteristics:

- Designed for processing elements
- Often follows FIFO or priority-based behavior depending on implementation

---

## Deque

    ArrayDeque
    LinkedList

`Deque` extends `Queue`.

It supports insertion and removal from both ends.

Hierarchy:

    Collection
        |
        +--- Queue
              |
              +--- Deque
                    |
                    +--- ArrayDeque

---

# 27. List vs Set vs Queue

| Feature | List | Set | Queue |
|---|---|---|---|
| Duplicates | Usually allowed | Not allowed | Usually allowed |
| Ordering | Usually defined | Implementation dependent | Processing order |
| Index access | Yes | No | No |
| Main purpose | Sequence | Unique elements | Processing elements |
| Examples | ArrayList | HashSet | PriorityQueue |

---

# 28. Internal Working

`Collection` is an interface, so it does not have one universal internal implementation.

The internal working depends on the implementation.

For example:

    Collection<Integer> numbers = new ArrayList<>();

Internally:

    Collection reference
            |
            v
       ArrayList object
            |
            v
     resizable array

But:

    Collection<Integer> numbers = new HashSet<>();

Internally:

    Collection reference
            |
            v
       HashSet object
            |
            v
       hash-based structure

And:

    Collection<Integer> numbers = new TreeSet<>();

Internally:

    Collection reference
            |
            v
       TreeSet object
            |
            v
      balanced tree structure

Therefore:

> `Collection` provides abstraction, while implementation classes provide data-structure-specific behavior.

---

# 29. Generic Nature

Collection is generic:

    Collection<E>

This provides compile-time type safety.

Example:

    Collection<String> names = new ArrayList<>();

Valid:

    names.add("Java");

Invalid:

    names.add(100);

The compiler prevents inserting an `Integer` into a `Collection<String>`.

---

## Without Generics

Legacy raw type:

    Collection names = new ArrayList();

    names.add("Java");
    names.add(100);

This is allowed but unsafe.

Retrieval can cause problems:

    String name = (String) names.get(1);

This results in:

    ClassCastException

Generics prevent many such errors at compile time.

---

# 30. Polymorphism

One of the most important concepts is:

    Collection<Integer> numbers = new ArrayList<>();

The reference type is:

    Collection<Integer>

The actual object type is:

    ArrayList<Integer>

This is runtime polymorphism.

Another example:

    Collection<Integer> numbers = new HashSet<>();

Now the same interface reference points to a different implementation.

This allows flexible code.

Example:

    public static void printCollection(Collection<String> collection) {

        for (String value : collection) {
            System.out.println(value);
        }
    }

Now we can pass different Collection implementations:

    ArrayList<String> list = new ArrayList<>();

    HashSet<String> set = new HashSet<>();

    printCollection(list);
    printCollection(set);

The method only depends on Collection behavior.

---

# 31. Common Mistakes

## Mistake 1 — Thinking Collection is a class

Wrong:

    Collection<String> names = new Collection<>();

Correct:

    Collection<String> names = new ArrayList<>();

---

## Mistake 2 — Thinking Map extends Collection

Wrong:

    Map is a child of Collection.

Correct:

    Map is a separate hierarchy.

---

## Mistake 3 — Thinking Collection guarantees order

Wrong:

    Collection always preserves insertion order.

Correct:

    Ordering depends on the implementation.

---

## Mistake 4 — Thinking Collection always allows duplicates

Wrong:

    Every Collection allows duplicates.

Correct:

    Duplicate behavior depends on the implementation.

---

## Mistake 5 — Thinking Collection always accepts null

Wrong:

    Every Collection allows null.

Correct:

    Null handling depends on the implementation.

---

## Mistake 6 — Confusing Collection and Collections

    Collection
        -> Interface

    Collections
        -> Utility class

---

## Mistake 7 — Thinking Collection determines performance

The interface defines operations, but actual time complexity depends on the implementation.

For example:

    contains()

may be approximately:

    ArrayList -> O(n)

while:

    HashSet -> average O(1)

under normal hashing assumptions.

---

# 32. Interview Traps

## Trap 1

Question:

> Is Collection a class or interface?

Answer:

    Collection is an interface in java.util.

---

## Trap 2

Question:

> Is Map a child of Collection?

Answer:

    No.

`Map` is a separate hierarchy.

---

## Trap 3

Question:

> Does Collection guarantee insertion order?

Answer:

    No.

The concrete implementation determines ordering behavior.

---

## Trap 4

Question:

> Does Collection allow duplicate elements?

Answer:

    The Collection interface itself does not impose one universal duplicate policy.

For example:

    ArrayList -> duplicates allowed

    HashSet -> duplicates not allowed

---

## Trap 5

Question:

> Can we create an object of Collection?

Answer:

    No.

It is an interface.

We instantiate an implementing class.

---

## Trap 6

Question:

> Why use Collection reference instead of ArrayList reference?

Answer:

It allows programming to the abstraction and makes code more flexible and less tightly coupled to a particular implementation.

---

## Trap 7

Question:

> Who decides how elements are stored?

Answer:

The concrete implementation class.

For example:

    ArrayList -> dynamic array

    HashSet -> hash-based structure

    TreeSet -> tree-based structure

---

# 33. DSA Relevance

The Collection interface is extremely important for DSA because many Java data structures are accessed through Collection-based abstractions.

Important DSA structures:

    ArrayList
    LinkedList
    HashSet
    TreeSet
    PriorityQueue
    ArrayDeque

Understanding Collection helps you understand:

- Dynamic arrays
- Linked lists
- Hashing
- Sets
- Trees
- Heaps
- Queues
- Deques
- Iteration
- Searching
- Filtering
- Bulk operations

---

## DSA Pattern 1 — Frequency Counting

Although `Map` is not a Collection, it is commonly used together with collections.

Example:

    int[] arr = {1, 2, 2, 3, 3, 3};

    Map<Integer, Integer> frequency = new HashMap<>();

    for (int value : arr) {
        frequency.put(
            value,
            frequency.getOrDefault(value, 0) + 1
        );
    }

Result:

    {
        1=1,
        2=2,
        3=3
    }

---

## DSA Pattern 2 — Duplicate Detection

A Set is useful when the problem asks whether duplicates exist.

Example:

    int[] arr = {10, 20, 30, 20};

    Set<Integer> seen = new HashSet<>();

    boolean duplicate = false;

    for (int value : arr) {

        if (!seen.add(value)) {
            duplicate = true;
            break;
        }
    }

    System.out.println(duplicate);

Output:

    true

Important trick:

    Set.add()
        |
        +--- true  -> new element
        |
        +--- false -> duplicate already exists

---

## DSA Pattern 3 — Removing Duplicates

Example:

    int[] arr = {1, 2, 2, 3, 3, 4};

    Set<Integer> unique = new LinkedHashSet<>();

    for (int value : arr) {
        unique.add(value);
    }

    System.out.println(unique);

Output:

    [1, 2, 3, 4]

`LinkedHashSet` preserves insertion order.

---

## DSA Pattern 4 — Filtering

Example:

    Collection<Integer> numbers = new ArrayList<>();

    numbers.add(10);
    numbers.add(15);
    numbers.add(20);
    numbers.add(25);

    numbers.removeIf(n -> n % 2 == 0);

    System.out.println(numbers);

Output:

    [15, 25]

---

## DSA Pattern 5 — Intersection

Using Collection operations:

    Collection<Integer> first = new ArrayList<>();

    first.add(1);
    first.add(2);
    first.add(3);
    first.add(4);

    Collection<Integer> second = new ArrayList<>();

    second.add(3);
    second.add(4);
    second.add(5);

    first.retainAll(second);

    System.out.println(first);

Output:

    [3, 4]

This represents the intersection of the two collections.

---

# 34. DSA Practice Questions

## Question 1 — Detect Duplicate

Given an array, determine whether it contains duplicates.

Approach:

    Use HashSet.

Example:

    int[] nums = {1, 2, 3, 1};

    Set<Integer> seen = new HashSet<>();

    boolean duplicate = false;

    for (int num : nums) {

        if (!seen.add(num)) {
            duplicate = true;
            break;
        }
    }

    System.out.println(duplicate);

Complexity:

    Time: O(n) average

    Space: O(n)

---

## Question 2 — Remove Duplicates

Example:

    int[] nums = {1, 1, 2, 2, 3};

    Set<Integer> unique = new LinkedHashSet<>();

    for (int num : nums) {
        unique.add(num);
    }

    System.out.println(unique);

Output:

    [1, 2, 3]

---

## Question 3 — Find Common Elements

Example:

    Collection<Integer> first =
        new ArrayList<>(Arrays.asList(1, 2, 3, 4));

    Collection<Integer> second =
        new ArrayList<>(Arrays.asList(3, 4, 5, 6));

    first.retainAll(second);

    System.out.println(first);

Output:

    [3, 4]

---

## Question 4 — Remove Even Numbers

Example:

    Collection<Integer> numbers =
        new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5, 6));

    numbers.removeIf(n -> n % 2 == 0);

    System.out.println(numbers);

Output:

    [1, 3, 5]

---

## Question 5 — Find Maximum

Example:

    Collection<Integer> numbers =
        new ArrayList<>(Arrays.asList(10, 50, 20, 40, 30));

    int max = Collections.max(numbers);

    System.out.println(max);

Output:

    50

Time complexity:

    O(n)

---

# 35. Top Interview Questions

## Q1. What is Collection in Java?

`Collection` is an interface in `java.util` that represents a group of objects and defines common operations such as adding, removing, searching and iterating over elements.

---

## Q2. Is Collection a class or interface?

It is an interface.

    java.util.Collection

---

## Q3. Which interface is the parent of Collection?

    Iterable<E>

Relationship:

    Iterable
        |
    Collection

---

## Q4. Which interfaces directly extend Collection?

Important direct subinterfaces include:

    List
    Set
    Queue

`Deque` extends `Queue`.

---

## Q5. Is Map a Collection?

No.

`Map` is a separate hierarchy designed for key-value mappings.

---

## Q6. Why is Collection generic?

Generics provide compile-time type safety.

Example:

    Collection<String> names = new ArrayList<>();

Only `String` elements can be added through that reference.

---

## Q7. Can Collection be instantiated?

No.

It is an interface.

Use an implementation:

    Collection<String> names = new ArrayList<>();

---

## Q8. Does Collection guarantee ordering?

No.

Ordering depends on the implementation.

---

## Q9. Does Collection allow duplicates?

The interface itself does not impose one universal duplicate policy.

For example:

    ArrayList -> allows duplicates

    HashSet -> does not allow duplicates

---

## Q10. What is the difference between Collection and Collections?

    Collection
        -> Interface

    Collections
        -> Utility class

---

## Q11. What is the difference between Collection and Iterable?

`Iterable` provides the basic ability to obtain an `Iterator` and support enhanced for-loop iteration.

`Collection` extends `Iterable` and adds operations for managing groups of elements.

---

## Q12. What does add() return?

It returns a boolean indicating whether the collection changed as a result of the operation.

For example, `HashSet.add()` returns `false` if the element was already present.

---

## Q13. What is the difference between removeAll() and retainAll()?

    removeAll()
        -> removes matching elements

    retainAll()
        -> keeps matching elements

---

## Q14. What does clear() do?

It removes all elements from the collection.

---

## Q15. What is removeIf()?

It removes elements that satisfy a given predicate.

Example:

    collection.removeIf(x -> x % 2 == 0);

---

## Q16. Why does Collection not define get(index)?

Because not every Collection is index-based.

For example:

    ArrayList
        -> supports index access

    HashSet
        -> does not provide index-based access

Therefore `get(index)` belongs to `List`, not `Collection`.

---

## Q17. Why does Collection not define sort()?

Because not every Collection has an ordering model.

Sorting is relevant to ordered/sequential structures.

A `Set` such as `HashSet` does not provide index-based ordering.

Sorting functionality can be provided by:

    Collections.sort(list)

or:

    list.sort(comparator)

for Lists.

---

## Q18. What is an optional operation?

An operation defined by the interface that a particular implementation may choose not to support.

Unsupported modification can result in:

    UnsupportedOperationException

---

## Q19. What is the difference between Collection and Array?

Array:

    Fixed size
    Can store primitives
    Basic language construct

Collection:

    Dynamic data structures
    Works with objects/generics
    Rich API
    Multiple implementations

---

## Q20. Why is Collection important in Java?

Because it provides a common abstraction for working with groups of objects and forms the foundation of Java's List, Set and Queue hierarchy.

---

# 36. 30-Second Interview Answer

If the interviewer asks:

> "What is the Collection interface?"

Answer:

> "`Collection` is a generic interface in the `java.util` package and extends `Iterable`. It represents a group of objects and defines common operations such as adding, removing, searching, checking size, bulk operations and iteration. Interfaces like `List`, `Set`, and `Queue` extend it. The Collection interface itself doesn't determine ordering, duplicate handling or internal storage; those behaviors depend on the concrete implementation such as `ArrayList`, `HashSet`, or `PriorityQueue`."

---

# 37. Cheat Sheet

## Hierarchy

    Iterable
        |
    Collection
        |
        +--- List
        |
        +--- Set
        |
        +--- Queue
              |
              +--- Deque

---

## Important Methods

    add()
    addAll()

    remove()
    removeAll()
    removeIf()
    retainAll()
    clear()

    contains()
    containsAll()

    size()
    isEmpty()

    toArray()

    iterator()
    spliterator()

    forEach()

    stream()
    parallelStream()

---

## Method Categories

    ADD
        add()
        addAll()

    REMOVE
        remove()
        removeAll()
        removeIf()
        retainAll()
        clear()

    SEARCH
        contains()
        containsAll()

    STATE
        size()
        isEmpty()

    CONVERSION
        toArray()

    ITERATION
        iterator()
        spliterator()
        forEach()

    STREAM
        stream()
        parallelStream()

---

# 38. Quick Revision

```text
Collection
    |
    +--- Interface
    |
    +--- java.util
    |
    +--- extends Iterable
    |
    +--- Represents group of objects
    |
    +--- Parent of:
    |       |
    |       +--- List
    |       +--- Set
    |       +--- Queue
    |
    +--- Does NOT include Map
    |
    +--- Does NOT define:
            |
            +--- Universal ordering
            +--- Universal duplicate policy
            +--- Universal null policy
            +--- One specific internal data structure