# 📚 Java Collections — Interview Questions & Answers

> **A focused collection of Java Collections Framework interview questions with concise, interview-ready answers.**

---

# 📑 Table of Contents

- [1. Collection Framework Basics](#1-collection-framework-basics)
- [2. List](#2-list)
- [3. ArrayList](#3-arraylist)
- [4. LinkedList](#4-linkedlist)
- [5. Vector and Stack](#5-vector-and-stack)
- [6. Set](#6-set)
- [7. Map](#7-map)
- [8. Queue and Deque](#8-queue-and-deque)
- [9. Iterator and ListIterator](#9-iterator-and-listiterator)
- [10. Comparable and Comparator](#10-comparable-and-comparator)
- [11. Hashing and Internal Working](#11-hashing-and-internal-working)
- [12. Collection Tricky Questions](#12-collection-tricky-questions)
- [13. Rapid Collection Revision](#13-rapid-collection-revision)

---

# 1. Collection Framework Basics

## 1. What is the Java Collections Framework?

The Java Collections Framework is a unified architecture for storing and manipulating groups of objects.

It provides:

```text
Interfaces
Implementations
Algorithms
Utility methods
Iterators
```

---

## 2. What is a Collection?

`Collection` is an interface representing a group of objects.

It is the root interface for the main collection hierarchy:

```text
Collection
├── List
├── Set
└── Queue
```

`Map` is part of the Collections Framework but does **not** extend `Collection`.

---

## 3. What is the difference between Collection and Collections?

```text
Collection
→ interface


Collections
→ utility class
```

`Collections` provides static utility methods such as:

```text
sort()
reverse()
shuffle()
min()
max()
binarySearch()
```

---

## 4. What is the difference between Collection and Collections Framework?

```text
Collection
→ one interface


Collections Framework
→ complete architecture containing
  interfaces, classes, algorithms,
  iterators and utilities
```

---

## 5. What is the root interface of the Collection hierarchy?

```text
Collection
```

But `Map` is outside this hierarchy.

---

## 6. Does Map extend Collection?

No.

`Map` represents key-value mappings rather than a collection of individual elements.

---

## 7. What are the major Collection interfaces?

```text
List
Set
Queue
Deque
```

And separately:

```text
Map
```

---

## 8. What is the difference between List, Set and Map?

```text
List
→ ordered
→ duplicates allowed
→ index-based access


Set
→ duplicates not allowed
→ no general index-based access


Map
→ key-value pairs
→ keys are unique
```

---

## 9. Why are collections preferred over arrays?

Collections provide dynamic sizing and many built-in operations.

Arrays provide fixed size and can store primitives directly.

Collections generally store objects, although autoboxing allows primitive values to be used conveniently.

---

## 10. Can collections store primitive data types?

No.

Collections store objects.

Wrapper classes are used through autoboxing:

```text
int
↓
Integer
```

---

# 2. List

## 11. What is List?

`List` is an ordered collection that allows duplicate elements.

It supports positional/index-based operations.

Common implementations:

```text
ArrayList
LinkedList
Vector
Stack
```

---

## 12. Does List maintain insertion order?

Yes.

The List contract defines an ordered sequence.

---

## 13. Does List allow duplicates?

Yes.

Example:

```text
[10, 20, 10, 30]
```

is valid.

---

## 14. Can List contain null?

Generally yes, depending on the implementation.

For example, `ArrayList` allows null elements.

---

## 15. ArrayList vs LinkedList?

```text
ArrayList
→ dynamic array
→ fast random access
→ costly middle insertion/removal


LinkedList
→ doubly linked list
→ efficient insertion/removal when node position is known
→ slower random access
```

---

# 3. ArrayList

## 16. What is ArrayList?

`ArrayList` is a resizable-array implementation of the `List` interface.

---

## 17. Is ArrayList dynamically sized?

Yes.

It automatically grows when its current capacity is insufficient.

---

## 18. What is the default initial capacity of ArrayList?

For modern Java implementations, a newly constructed `ArrayList` generally starts with an empty backing array and allocates its default capacity lazily when the first element is added.

When default capacity is allocated, it is commonly:

```text
10
```

The exact internal implementation is not a public API guarantee.

---

## 19. What happens when ArrayList becomes full?

It creates a larger backing array and copies the existing elements into it.

The old backing array becomes eligible for garbage collection when no longer referenced.

---

## 20. What is the time complexity of ArrayList operations?

Typical complexity:

```text
get()
→ O(1)


set()
→ O(1)


add(element)
→ amortized O(1)


add(index, element)
→ O(n)


remove(index)
→ O(n)


contains()
→ O(n)
```

---

## 21. Why is ArrayList random access fast?

Elements are stored in an array-like contiguous backing structure.

The index can be directly mapped to the corresponding array position.

---

## 22. Why is insertion in the middle of ArrayList expensive?

Elements after the insertion position must be shifted.

Example:

```text
Before:
[10, 20, 30, 40]

Insert 15:

[10, 15, 20, 30, 40]
```

Several elements must move.

---

## 23. Is ArrayList synchronized?

No.

If multiple threads modify the same ArrayList concurrently, appropriate synchronization or another concurrency strategy is required.

---

## 24. Does ArrayList allow null?

Yes.

It can contain multiple null elements.

---

## 25. Can ArrayList store duplicate elements?

Yes.

---

## 26. ArrayList size vs capacity?

```text
size
→ number of elements currently stored


capacity
→ amount of backing-array storage currently available
```

Capacity is an implementation detail and should not normally be relied upon.

---

## 27. How can ArrayList capacity be optimized?

If the approximate number of elements is known, an initial capacity can be supplied:

```java
ArrayList<Integer> list =
    new ArrayList<>(1000);
```

This can reduce repeated resizing.

---

# 4. LinkedList

## 28. What is LinkedList?

`LinkedList` is a doubly linked list implementation of `List` and `Deque`.

---

## 29. How does LinkedList store elements?

Conceptually, each node contains:

```text
Element
Previous reference
Next reference
```

---

## 30. Is LinkedList good for random access?

No.

Accessing an element by index generally requires traversal.

Typical complexity:

```text
get(index)
→ O(n)
```

---

## 31. What is the advantage of LinkedList?

When insertion/removal occurs at a known node or at the ends, it can perform the structural modification efficiently without shifting an array of elements.

---

## 32. Does LinkedList allow null?

Yes.

---

## 33. Does LinkedList allow duplicates?

Yes.

---

## 34. Is LinkedList synchronized?

No.

---

## 35. ArrayList or LinkedList for frequent random access?

ArrayList generally provides much faster random access.

---

# 5. Vector and Stack

## 36. What is Vector?

`Vector` is a legacy dynamic-array implementation of `List`.

Its methods are synchronized.

---

## 37. Vector vs ArrayList?

```text
ArrayList
→ unsynchronized
→ modern general-purpose List


Vector
→ synchronized methods
→ legacy class
```

---

## 38. What is Stack?

`Stack` is a legacy class that extends `Vector` and provides LIFO stack operations.

Common methods include:

```text
push()
pop()
peek()
```

---

## 39. What should generally be preferred over Stack?

For a stack data structure, `Deque` implementations such as `ArrayDeque` are generally preferred in modern Java.

---

# 6. Set

## 40. What is Set?

`Set` is a collection that does not allow duplicate elements according to its equality semantics.

---

## 41. What are common Set implementations?

```text
HashSet
LinkedHashSet
TreeSet
```

---

## 42. HashSet vs LinkedHashSet vs TreeSet?

```text
HashSet
→ no guaranteed iteration order
→ hash-based


LinkedHashSet
→ maintains insertion order
→ hash table + linked structure


TreeSet
→ sorted order
→ tree-based
```

---

## 43. Does HashSet allow duplicates?

No.

If an equal element is already present, adding another equal element does not add a second copy.

---

## 44. Does HashSet allow null?

Yes.

A HashSet generally permits one null element.

---

## 45. Does LinkedHashSet allow null?

Yes.

It generally permits one null element.

---

## 46. Does TreeSet allow null?

With natural ordering, null is generally not permitted because comparison with null cannot be performed.

A comparator could theoretically define special handling, but this should not be confused with the normal natural-ordering behavior.

---

## 47. How does HashSet identify duplicates?

It relies on hashing and equality.

Conceptually:

```text
hashCode()
↓
bucket selection
↓
equals()
↓
duplicate determination
```

---

## 48. Why must equals() and hashCode() be consistent?

If two objects are equal according to `equals()`, they must return the same `hashCode()`.

Otherwise, hash-based collections can behave incorrectly.

---

## 49. What is LinkedHashSet's main advantage?

It maintains insertion order while providing hash-based Set behavior.

---

## 50. What is TreeSet's main advantage?

It maintains elements in sorted order.

---

## 51. What is the typical complexity of HashSet operations?

Average-case:

```text
add()
→ O(1)


remove()
→ O(1)


contains()
→ O(1)
```

Worst-case behavior depends on collisions and implementation details.

---

# 7. Map

## 52. What is Map?

`Map` stores data as key-value pairs.

Example:

```text
101 → "Java"
102 → "Spring"
103 → "SQL"
```

---

## 53. Can Map contain duplicate keys?

No.

Adding a value with an existing key replaces the previous mapping for that key.

---

## 54. Can Map contain duplicate values?

Yes.

Different keys can map to the same value.

---

## 55. Common Map implementations?

```text
HashMap
LinkedHashMap
TreeMap
Hashtable
ConcurrentHashMap
```

---

## 56. HashMap vs LinkedHashMap vs TreeMap?

```text
HashMap
→ no guaranteed iteration order


LinkedHashMap
→ maintains insertion/access order depending on configuration


TreeMap
→ sorted by keys
```

---

## 57. Does HashMap allow null?

Yes.

A HashMap permits:

```text
One null key
Multiple null values
```

---

## 58. Does TreeMap allow null keys?

With natural ordering, null keys are generally not permitted because keys need to be compared.

---

## 59. Does LinkedHashMap allow null?

Yes.

It follows HashMap's general null-key/value support while maintaining linked iteration order.

---

## 60. Is HashMap synchronized?

No.

Concurrent access requiring thread safety should use appropriate synchronization or a concurrent collection such as ConcurrentHashMap depending on the use case.

---

## 61. What is the time complexity of HashMap operations?

Average-case:

```text
put()
→ O(1)


get()
→ O(1)


remove()
→ O(1)
```

Worst-case behavior depends on collisions and implementation.

---

## 62. How does HashMap work internally?

Conceptually:

```text
put(key, value)
      ↓
hash key
      ↓
calculate bucket
      ↓
find matching entry
      ↓
equals()
      ↓
insert/update
```

---

## 63. What happens when two keys have the same hash code?

A collision occurs.

The map stores both entries in the same bucket and uses equality checks to distinguish keys.

Modern Java implementations can use balanced tree structures for sufficiently large collision chains under certain conditions.

---

## 64. What is a hash collision?

A hash collision occurs when two different objects produce the same hash value.

```text
Key A → hash 100
Key B → hash 100
```

The keys may still be different according to `equals()`.

---

## 65. What is load factor?

Load factor controls when a hash table should resize based on its capacity and number of stored entries.

For HashMap, the default load factor is commonly:

```text
0.75
```

---

## 66. Why is load factor used?

It provides a trade-off between:

```text
Memory usage
vs
Hash-table performance
```

A lower load factor generally means more empty space, while a higher one can reduce memory overhead but increase collision probability.

---

## 67. What is initial capacity in HashMap?

It is the initial sizing parameter used for the hash table.

It affects when resizing may occur.

---

## 68. HashMap vs Hashtable?

```text
HashMap
→ modern
→ not synchronized
→ allows null key and null values


Hashtable
→ legacy
→ synchronized methods
→ does not allow null keys or values
```

---

## 69. Why is Hashtable considered legacy?

It predates the modern Collections Framework design and has older synchronization semantics.

Modern applications generally use HashMap or appropriate concurrent collections instead.

---

## 70. What is ConcurrentHashMap?

`ConcurrentHashMap` is a thread-safe hash-based Map designed for concurrent access.

It supports concurrent reads and updates with better scalability than synchronizing an entire map in many use cases.

---

## 71. Does ConcurrentHashMap allow null?

No.

It does not permit null keys or null values.

---

# 8. Queue and Deque

## 72. What is Queue?

A Queue is designed for holding elements before processing.

A common queue discipline is:

```text
FIFO
First In, First Out
```

---

## 73. What are common Queue implementations?

```text
LinkedList
PriorityQueue
ArrayDeque
```

---

## 74. What is Deque?

Deque means:

```text
Double Ended Queue
```

It supports insertion and removal from both ends.

---

## 75. What is ArrayDeque?

`ArrayDeque` is a resizable-array implementation of `Deque`.

It can be used as:

```text
Queue
Stack
Deque
```

---

## 76. ArrayDeque vs Stack?

For stack behavior:

```text
ArrayDeque
→ generally preferred


Stack
→ legacy class
```

---

## 77. Does ArrayDeque allow null?

No.

---

## 78. What is PriorityQueue?

`PriorityQueue` processes elements according to priority rather than simple insertion order.

By default, it uses natural ordering.

---

## 79. Does PriorityQueue maintain sorted iteration order?

No.

Its iterator does not guarantee sorted traversal.

The head of the queue is the least element according to its ordering.

---

## 80. Does PriorityQueue allow null?

No.

---

## 81. What is FIFO?

```text
First In
First Out
```

The earliest inserted eligible element is processed first.

---

## 82. What is LIFO?

```text
Last In
First Out
```

The most recently inserted element is processed first.

---

# 9. Iterator and ListIterator

## 83. What is Iterator?

`Iterator` provides a standard way to traverse collection elements.

Main methods:

```text
hasNext()
next()
remove()
```

---

## 84. What is ListIterator?

`ListIterator` is a specialized iterator for Lists.

It supports traversal in both directions.

Important methods include:

```text
hasNext()
next()
hasPrevious()
previous()
add()
remove()
set()
```

---

## 85. Iterator vs ListIterator?

```text
Iterator
→ forward traversal
→ works with many collections


ListIterator
→ forward + backward traversal
→ List only
→ supports add/set
```

---

## 86. Can Iterator remove elements?

Yes.

If supported by the collection, `Iterator.remove()` can safely remove the last element returned by `next()`.

---

## 87. Why should Iterator.remove() be preferred over removing directly during iteration?

Direct structural modification while iterating can cause `ConcurrentModificationException` for fail-fast iterators.

Using the iterator's own `remove()` method communicates the removal through the iterator.

---

## 88. What is ConcurrentModificationException?

It is a runtime exception commonly thrown by fail-fast iterators when a collection is structurally modified in an unsupported way while it is being iterated.

It is not a guaranteed thread-safety mechanism.

---

## 89. Is every Iterator fail-fast?

No.

Fail-fast behavior is an implementation characteristic of many standard collections, not a universal Iterator contract.

---

# 10. Comparable and Comparator

## 90. What is Comparable?

`Comparable<T>` defines a class's natural ordering.

It provides:

```java
compareTo()
```

---

## 91. What is Comparator?

`Comparator<T>` defines an external or alternative ordering.

It provides:

```java
compare()
```

---

## 92. Comparable vs Comparator?

```text
Comparable
→ natural ordering
→ implemented by the class


Comparator
→ external/custom ordering
→ separate object/function
```

---

## 93. Can a class have multiple Comparator objects?

Yes.

This allows different sorting strategies.

Example:

```text
Sort by name
Sort by age
Sort by salary
```

---

## 94. Can a class have multiple natural orderings using Comparable?

Normally, a class defines one natural ordering through `Comparable`.

Multiple alternative orderings are better represented using `Comparator`s.

---

## 95. Which method does Collections.sort() use?

When no Comparator is supplied, sorting generally uses the elements' natural ordering.

When a Comparator is supplied, that Comparator defines the ordering.

---

# 11. Hashing and Internal Working

## 96. What is hashCode()?

`hashCode()` returns an integer hash value representing an object.

It is used extensively by hash-based collections.

---

## 97. What is the equals-hashCode contract?

If:

```text
a.equals(b)
```

is true, then:

```text
a.hashCode() == b.hashCode()
```

must also be true.

The reverse is not required.

---

## 98. Can two unequal objects have the same hashCode?

Yes.

That is a hash collision.

---

## 99. Can two equal objects have different hashCodes?

No.

That violates the `equals()`/`hashCode()` contract.

---

## 100. Why should immutable objects be preferred as HashMap keys?

Changing a key's state after insertion can change its hash code or equality behavior.

Then the map may no longer be able to locate the entry correctly.

---

## 101. What happens if a HashMap key is modified after insertion?

If the modification changes the properties used by `equals()` or `hashCode()`, a subsequent lookup may fail to find the entry using the modified key.

This is why stable key state is important.

---

## 102. What is fail-fast behavior?

A fail-fast iterator attempts to detect structural modification outside the iterator and may throw `ConcurrentModificationException`.

It is primarily a bug-detection mechanism.

---

## 103. What is fail-safe behavior?

"Fail-safe" is an informal term commonly used for iterators that operate over a snapshot or weakly consistent view rather than throwing the same fail-fast exception behavior.

The Java API documentation does not define a universal "fail-safe iterator" category.

---

# 12. Collection Tricky Questions

## 104. Which collection allows duplicates and maintains insertion order?

```text
List
```

Implementations such as:

```text
ArrayList
LinkedList
```

---

## 105. Which Set maintains insertion order?

```text
LinkedHashSet
```

---

## 106. Which Set maintains sorted order?

```text
TreeSet
```

---

## 107. Which Map maintains insertion order?

```text
LinkedHashMap
```

---

## 108. Which Map maintains sorted key order?

```text
TreeMap
```

---

## 109. Can HashMap have duplicate keys?

No.

An existing mapping is replaced when the same key is inserted again according to key equality.

---

## 110. Can HashMap have duplicate values?

Yes.

---

## 111. Can a Set contain objects with the same hashCode?

Yes.

If they are not equal, both can exist.

---

## 112. What happens when add() is called on a Set with a duplicate?

The Set remains unchanged.

For standard Set implementations, `add()` returns:

```text
false
```

when the element was already present.

---

## 113. Can ArrayList and HashSet contain null?

Yes.

Both generally allow null.

---

## 114. Can TreeSet contain null?

Not with its normal natural ordering.

---

## 115. Can HashMap contain null?

Yes.

One null key and multiple null values are permitted.

---

## 116. Can Hashtable contain null?

No.

Neither null keys nor null values are allowed.

---

## 117. Which is faster: ArrayList or LinkedList?

It depends on the operation.

For random access:

```text
ArrayList
```

is generally much faster.

For structural insertion/removal at a known linked-list position:

```text
LinkedList
```

can avoid array shifting.

---

## 118. Which is faster: HashSet or TreeSet?

They have different guarantees.

Typical:

```text
HashSet
→ average O(1) basic operations


TreeSet
→ O(log n) basic operations
→ maintains sorted order
```

---

## 119. Which is faster: HashMap or TreeMap?

For basic lookup/update:

```text
HashMap
→ average O(1)


TreeMap
→ O(log n)
```

TreeMap provides sorted key ordering.

---

## 120. Can a collection be made synchronized?

Yes.

Older utility methods include wrappers such as:

```text
Collections.synchronizedList()
Collections.synchronizedSet()
Collections.synchronizedMap()
```

Modern concurrent collections may be more appropriate depending on the workload.

---

## 121. What is an unmodifiable collection?

An unmodifiable collection is a view that prevents modification through that particular reference.

Example concept:

```text
Collections.unmodifiableList()
```

The underlying collection may still change if another reference can modify it.

---

## 122. What is an immutable collection?

An immutable collection cannot have its state changed after creation.

Modern Java provides factory methods such as:

```text
List.of()
Set.of()
Map.of()
```

These create unmodifiable collection instances with immutable-style contents.

---

## 123. What is the difference between unmodifiable and immutable?

```text
Unmodifiable view
→ this reference cannot modify
→ underlying collection may still change


Immutable collection
→ collection state cannot be changed
```

---

# 13. Rapid Collection Revision

## 124. List allows duplicates?

```text
Yes
```

---

## 125. Set allows duplicates?

```text
No
```

---

## 126. Map allows duplicate keys?

```text
No
```

---

## 127. Map allows duplicate values?

```text
Yes
```

---

## 128. Map extends Collection?

```text
No
```

---

## 129. ArrayList random access complexity?

```text
O(1)
```

---

## 130. LinkedList random access complexity?

```text
O(n)
```

---

## 131. HashMap average get complexity?

```text
O(1)
```

---

## 132. TreeMap get complexity?

```text
O(log n)
```

---

## 133. HashSet average contains complexity?

```text
O(1)
```

---

## 134. TreeSet contains complexity?

```text
O(log n)
```

---

## 135. Which List is array-based?

```text
ArrayList
```

---

## 136. Which List is linked-node based?

```text
LinkedList
```

---

## 137. Which Set maintains insertion order?

```text
LinkedHashSet
```

---

## 138. Which Set maintains sorted order?

```text
TreeSet
```

---

## 139. Which Map maintains insertion order?

```text
LinkedHashMap
```

---

## 140. Which Map maintains sorted key order?

```text
TreeMap
```

---

## 141. Which Map is not synchronized?

```text
HashMap
```

---

## 142. Which Map is designed for concurrent access?

```text
ConcurrentHashMap
```

---

## 143. Does ConcurrentHashMap allow null?

```text
No
```

---

## 144. Does HashMap allow null?

```text
Yes
```

---

## 145. Does Hashtable allow null?

```text
No
```

---

## 146. What is FIFO?

```text
First In
First Out
```

---

## 147. What is LIFO?

```text
Last In
First Out
```

---

## 148. What is Deque?

```text
Double Ended Queue
```

---

## 149. Which modern collection is commonly preferred for stack behavior?

```text
ArrayDeque
```

---

## 150. Comparable vs Comparator?

```text
Comparable
→ natural ordering


Comparator
→ custom/alternative ordering
```

---

## 151. Iterator vs ListIterator?

```text
Iterator
→ forward traversal


ListIterator
→ forward + backward traversal
→ List only
```

---

## 152. Why override hashCode() when overriding equals()?

Because equal objects must have equal hash codes for hash-based collections to work correctly.

---

# 🧠 Collection Framework Memory Map

```text
                    COLLECTIONS
                         |
          +--------------+--------------+
          |              |              |
         List           Set           Queue
          |              |              |
    +-----+-----+    +---+---+      +---+---+
    |           |    |       |      |       |
ArrayList  LinkedList HashSet TreeSet Deque  PriorityQueue
    |           |      |
    +-----------+      |
                       |
                  LinkedHashSet

                         MAP
                          |
             +------------+------------+
             |            |            |
          HashMap   LinkedHashMap   TreeMap
```

---

# ⚡ Most Important Collection Comparisons

```text
ArrayList
vs
LinkedList


ArrayList
vs
Vector


Vector
vs
Stack


HashSet
vs
LinkedHashSet
vs
TreeSet


HashMap
vs
LinkedHashMap
vs
TreeMap


HashMap
vs
Hashtable


HashMap
vs
ConcurrentHashMap


Iterator
vs
ListIterator


Comparable
vs
Comparator


Collection
vs
Collections


Unmodifiable
vs
Immutable
```

---

