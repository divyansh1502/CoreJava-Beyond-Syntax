# 🌳 06 — TreeMap

> **Java Collections Deep Dive → Map Framework**
>
> `TreeMap` is a sorted `Map` implementation based on a **Red-Black Tree**.
> It stores entries according to the ordering of its keys.

---

# 📑 Table of Contents

- [🧠 1. Introduction](#-1-introduction)
- [🎯 2. Why TreeMap](#-2-why-treemap)
- [🧬 3. TreeMap Hierarchy](#-3-treemap-hierarchy)
- [🌳 4. Internal Data Structure](#-4-internal-data-structure)
- [⚖️ 5. TreeMap vs HashMap](#-5-treemap-vs-hashmap)
- [🔢 6. Key Ordering](#-6-key-ordering)
- [🔤 7. Comparable](#-7-comparable)
- [🧩 8. Comparator](#-8-comparator)
- [🛠️ 9. TreeMap Constructors](#-9-treemap-constructors)
- [➕ 10. put()](#-10-put)
- [🔍 11. get()](#-11-get)
- [🗑️ 12. remove()](#-12-remove)
- [🔎 13. containsKey()](#-13-containskey)
- [📏 14. firstKey() and lastKey()](#-14-firstkey-and-lastkey)
- [⬆️ 15. higherKey() and lowerKey()](#-15-higherkey-and-lowerkey)
- [⬆️ 16. ceilingKey() and floorKey()](#-16-ceilingkey-and-floorkey)
- [📦 17. Entry Navigation Methods](#-17-entry-navigation-methods)
- [✂️ 18. SubMap](#-18-submap)
- [✂️ 19. HeadMap](#-19-headmap)
- [✂️ 20. TailMap](#-20-tailmap)
- [🔄 21. Iteration](#-21-iteration)
- [🧠 22. Null Keys](#-22-null-keys)
- [📊 23. Complexity](#-23-complexity)
- [⚠️ 24. Common Mistakes](#-24-common-mistakes)
- [🎯 25. DSA Connection](#-25-dsa-connection)
- [🧩 26. DSA Patterns](#-26-dsa-patterns)
- [🧪 27. Complete Example](#-27-complete-example)
- [⚖️ 28. TreeMap vs LinkedHashMap](#-28-treemap-vs-linkedhashmap)
- [🎤 29. 30-Second Interview Answer](#-29-30-second-interview-answer)
- [⚡ 30. Cheat Sheet](#-30-cheat-sheet)
- [🎯 31. Interview Questions](#-31-interview-questions)
- [🏁 32. Final Mental Model](#-32-final-mental-model)

---

# 🧠 1. Introduction

`TreeMap` is a class in the Java Collections Framework that implements the `NavigableMap` interface.

It stores key-value pairs in **sorted order according to the keys**.

Declaration:

```java
public class TreeMap<K,V>
    extends AbstractMap<K,V>
    implements NavigableMap<K,V>,
               Cloneable,
               Serializable
```

The important hierarchy is:

```text
Map
 ↓
SortedMap
 ↓
NavigableMap
 ↓
TreeMap
```

---

# 🎯 2. Why TreeMap

Use `TreeMap` when you need:

```text
Key-value storage
        +
Sorted keys
        +
Navigation operations
```

For example:

```java
Map<Integer, String> map = new TreeMap<>();

map.put(50, "A");
map.put(10, "B");
map.put(30, "C");
```

Iteration will be:

```text
10 → B
30 → C
50 → A
```

The keys are automatically sorted.

---

# 🧬 3. TreeMap Hierarchy

Important hierarchy:

> `Map` is **NOT** a child of `Collection`.

The Map hierarchy is separate.

```text
Map
 │
 └── SortedMap
       │
       └── NavigableMap
             │
             └── TreeMap
```

Therefore:

```text
TreeMap IS-A Map
TreeMap IS-A SortedMap
TreeMap IS-A NavigableMap
```

---

# 🌳 4. Internal Data Structure

TreeMap is based on a:

```text
Red-Black Tree
```

A Red-Black Tree is a type of:

```text
Self-Balancing Binary Search Tree
```

Conceptually:

```text
          50
         /  \
       30    70
      / \    / \
    20  40  60  80
```

The tree maintains ordering:

```text
left subtree
     <
node
     <
right subtree
```

Because the tree remains balanced, operations generally take:

```text
O(log n)
```

## 🧠 Why Balancing Matters

A normal unbalanced BST could become:

```text
10
  \
   20
     \
      30
        \
         40
           \
            50
```

This behaves like a linked list.

Searching could become:

```text
O(n)
```

A Red-Black Tree maintains balance so TreeMap operations remain:

```text
O(log n)
```

---

# ⚖️ 5. TreeMap vs HashMap

| Feature | HashMap | TreeMap |
|---|---|---|
| Main structure | Hash table | Red-Black Tree |
| Ordering | No guaranteed order | Sorted by key |
| `get()` | O(1) expected | O(log n) |
| `put()` | O(1) expected | O(log n) |
| `remove()` | O(1) expected | O(log n) |
| Navigation | Limited | Rich |
| Sorted keys | No | Yes |
| Null key | Allows one | Generally not with natural ordering |
| Best use | Fast lookup | Sorted/navigation operations |

### Mental Shortcut

```text
HashMap
   ↓
Fast lookup

TreeMap
   ↓
Sorted + navigable lookup
```

---

# 🔢 6. Key Ordering

TreeMap sorts keys using either:

1. Natural ordering
2. Comparator

---

## Natural Ordering

Example:

```java
TreeMap<Integer, String> map = new TreeMap<>();

map.put(30, "C");
map.put(10, "A");
map.put(20, "B");
```

Result:

```text
10 → A
20 → B
30 → C
```

For `Integer`:

```text
smaller → larger
```

For `String`:

```text
lexicographical ordering
```

---

# 🔤 7. Comparable

Natural ordering is commonly provided through:

```java
Comparable<T>
```

Example:

```text
Integer implements Comparable<Integer>
String implements Comparable<String>
```

`Comparable` defines:

```java
compareTo()
```

Example:

```java
a.compareTo(b);
```

Possible results:

```text
negative
   ↓
a comes before b

zero
   ↓
a and b are considered equal
for ordering

positive
   ↓
a comes after b
```

---

# 🧩 8. Comparator

A `Comparator` allows you to provide custom ordering.

Example:

```java
Comparator<Integer> reverse =
    (a, b) -> b.compareTo(a);
```

Then:

```java
TreeMap<Integer, String> map =
    new TreeMap<>(reverse);
```

Now keys are sorted in descending order.

Example:

```java
map.put(10, "A");
map.put(30, "C");
map.put(20, "B");
```

Iteration:

```text
30 → C
20 → B
10 → A
```

---

# 🛠️ 9. TreeMap Constructors

Common constructors:

```java
TreeMap()

TreeMap(Comparator<? super K> comparator)

TreeMap(Map<? extends K, ? extends V> m)

TreeMap(
    SortedMap<K, ? extends V> m
)
```

## Default Constructor

```java
TreeMap<Integer, String> map =
    new TreeMap<>();
```

Uses natural ordering.

---

## Comparator Constructor

```java
TreeMap<Integer, String> map =
    new TreeMap<>(
        (a, b) -> b.compareTo(a)
    );
```

Uses custom ordering.

---

# ➕ 10. put()

`put()` adds or updates a key-value pair.

Example:

```java
TreeMap<Integer, String> map =
    new TreeMap<>();

map.put(30, "C");
map.put(10, "A");
map.put(20, "B");
```

Result:

```text
10 → A
20 → B
30 → C
```

Complexity:

```text
O(log n)
```

---

## Duplicate Key

Suppose:

```java
map.put(10, "A");
map.put(10, "Updated");
```

The value becomes:

```text
10 → Updated
```

There is still only one key:

```text
10
```

---

# 🔍 11. get()

Example:

```java
String value = map.get(20);
```

TreeMap searches through the balanced tree.

Complexity:

```text
O(log n)
```

Conceptually:

```text
root
 ↓
compare key
 ↓
left OR right
 ↓
compare again
 ↓
continue
 ↓
found
```

---

# 🗑️ 12. remove()

Example:

```java
map.remove(20);
```

The node is removed from the Red-Black Tree and the tree is rebalanced if necessary.

Complexity:

```text
O(log n)
```

---

# 🔎 13. containsKey()

Example:

```java
map.containsKey(30);
```

Returns:

```text
true
```

if the key exists.

Complexity:

```text
O(log n)
```

---

# 📏 14. firstKey() and lastKey()

`firstKey()` returns the smallest key according to the map's ordering.

Example:

```java
TreeMap<Integer, String> map =
    new TreeMap<>();

map.put(30, "C");
map.put(10, "A");
map.put(20, "B");

map.firstKey();
```

Result:

```text
10
```

---

`lastKey()` returns the largest key.

```java
map.lastKey();
```

Result:

```text
30
```

---

# ⬆️ 15. higherKey() and lowerKey()

These are important `NavigableMap` methods.

Suppose:

```text
keys = 10, 20, 30, 40
```

Then:

```java
higherKey(20);
```

returns:

```text
30
```

because `30` is strictly greater than `20`.

And:

```java
lowerKey(20);
```

returns:

```text
10
```

because `10` is strictly smaller than `20`.

---

## 📊 Example

```java
TreeMap<Integer, String> map =
    new TreeMap<>();

map.put(10, "A");
map.put(20, "B");
map.put(30, "C");
map.put(40, "D");

map.higherKey(20);
```

Result:

```text
30
```

```java
map.lowerKey(20);
```

Result:

```text
10
```

---

# ⬆️ 16. ceilingKey() and floorKey()

These methods are slightly different.

## ceilingKey()

Returns the smallest key greater than or equal to the given key.

Example:

```text
keys = 10, 20, 30, 40

ceilingKey(25)
```

returns:

```text
30
```

But:

```text
ceilingKey(20)
```

returns:

```text
20
```

because equality is allowed.

---

## floorKey()

Returns the largest key less than or equal to the given key.

```text
floorKey(25)
```

returns:

```text
20
```

And:

```text
floorKey(20)
```

returns:

```text
20
```

---

# 🧠 Navigation Memory Trick

Remember:

```text
higher
   ↓
strictly greater

lower
   ↓
strictly smaller

ceiling
   ↓
greater OR equal

floor
   ↓
smaller OR equal
```

---

# 📦 17. Entry Navigation Methods

TreeMap also provides:

```text
firstEntry()
lastEntry()

higherEntry()
lowerEntry()

ceilingEntry()
floorEntry()
```

Example:

```java
Map.Entry<Integer, String> entry =
    map.firstEntry();
```

You get the complete key-value pair instead of only the key.

---

## 🔄 Poll Methods

TreeMap also provides:

```text
pollFirstEntry()
pollLastEntry()
```

These:

```text
retrieve
   +
remove
```

the first or last entry.

Example:

```java
map.pollFirstEntry();
```

This returns and removes the smallest entry.

---

# ✂️ 18. SubMap

`subMap()` provides a view of a range of keys.

Example:

```java
TreeMap<Integer, String> map =
    new TreeMap<>();

map.put(10, "A");
map.put(20, "B");
map.put(30, "C");
map.put(40, "D");

map.subMap(20, 40);
```

Represents keys in the range:

```text
20 → 30
```

The default range is:

```text
fromKey inclusive
toKey exclusive
```

So:

```text
[20, 40)
```

---

## Inclusive Version

You can specify boundaries:

```java
map.subMap(
    20,
    true,
    40,
    true
);
```

Now the range is:

```text
[20, 40]
```

---

# ✂️ 19. HeadMap

`headMap()` returns keys before a specified key.

Example:

```text
keys:

10, 20, 30, 40
```

```java
map.headMap(30);
```

Default:

```text
10
20
```

The endpoint is exclusive.

You can also specify inclusivity:

```java
map.headMap(30, true);
```

Now:

```text
10
20
30
```

---

# ✂️ 20. TailMap

`tailMap()` returns keys from a specified key onward.

Example:

```text
keys:

10, 20, 30, 40
```

```java
map.tailMap(30);
```

Result:

```text
30
40
```

By default, the starting key is inclusive.

---

# 🧠 Range Memory Trick

```text
headMap(x)
    ↓
everything BEFORE x

tailMap(x)
    ↓
everything FROM x

subMap(a, b)
    ↓
everything BETWEEN a and b
```

Think:

```text
HEAD → before

TAIL → after/from

SUB → middle section
```

---

# 🔄 21. Iteration

TreeMap's iteration follows key ordering.

Example:

```java
TreeMap<Integer, String> map =
    new TreeMap<>();

map.put(50, "E");
map.put(10, "A");
map.put(30, "C");
```

Iteration:

```text
10 → A
30 → C
50 → E
```

Using:

```java
for (Map.Entry<Integer, String> entry :
     map.entrySet()) {

    System.out.println(
        entry.getKey() + " = " +
        entry.getValue()
    );
}
```

---

## 🔽 Descending Order

TreeMap also supports:

```java
map.descendingMap();
```

If normal order is:

```text
10
20
30
40
```

Descending view:

```text
40
30
20
10
```

You can also use:

```java
map.descendingKeySet();
```

for keys.

---

# 🧠 22. Null Keys

TreeMap generally does not support a null key when using natural ordering.

Example:

```java
TreeMap<Integer, String> map =
    new TreeMap<>();

map.put(null, "A");
```

This can result in:

```text
NullPointerException
```

because TreeMap needs to compare keys.

With a custom `Comparator`, null handling can be explicitly designed if the comparator supports it.

Important interview point:

> Do not assume TreeMap supports null keys like HashMap does.

---

# 📊 23. Complexity

| Operation | TreeMap |
|---|---:|
| `put()` | O(log n) |
| `get()` | O(log n) |
| `remove()` | O(log n) |
| `containsKey()` | O(log n) |
| `firstKey()` | O(log n) or effectively constant navigation |
| `lastKey()` | O(log n) or effectively constant navigation |
| `higherKey()` | O(log n) |
| `lowerKey()` | O(log n) |
| `ceilingKey()` | O(log n) |
| `floorKey()` | O(log n) |
| Iteration | O(n) |
| Space | O(n) |

The key idea:

```text
HashMap
    → expected O(1)

TreeMap
    → O(log n)
```

The trade-off is that TreeMap provides ordering and navigation.

---

# ⚠️ 24. Common Mistakes

## ❌ Mistake 1

Thinking TreeMap uses a normal BST.

It uses a:

```text
Red-Black Tree
```

which is self-balancing.

---

## ❌ Mistake 2

Thinking TreeMap sorts values.

TreeMap sorts:

```text
keys
```

not values.

---

## ❌ Mistake 3

Thinking TreeMap is faster than HashMap for simple lookup.

Typically:

```text
HashMap → expected O(1)

TreeMap → O(log n)
```

---

## ❌ Mistake 4

Thinking TreeMap preserves insertion order.

It doesn't.

It maintains key ordering.

---

## ❌ Mistake 5

Confusing:

```text
higherKey()
```

with:

```text
ceilingKey()
```

Difference:

```text
higher  → strictly greater

ceiling → greater or equal
```

---

## ❌ Mistake 6

Confusing:

```text
lowerKey()
```

with:

```text
floorKey()
```

Difference:

```text
lower → strictly smaller

floor → smaller or equal
```

---

## ❌ Mistake 7

Thinking `subMap()` returns an independent copy.

The returned range is generally a view backed by the original map.

---

# 🎯 25. DSA Connection

TreeMap is extremely useful in problems where you need:

```text
Dynamic ordering
      +
Fast search
      +
Nearest key
      +
Range queries
```

Important patterns:

1. Floor / ceiling
2. Predecessor / successor
3. Dynamic sorted data
4. Range queries
5. Coordinate-like navigation
6. Event scheduling
7. Interval problems
8. Ordered frequency structures

---

# 🧩 26. DSA Patterns

## 🔥 Pattern 1 — Find Ceiling

Suppose:

```text
values = [10, 20, 30, 40]
```

Question:

> Find the smallest value >= 25.

TreeMap:

```java
map.ceilingKey(25);
```

Answer:

```text
30
```

---

## 🔥 Pattern 2 — Find Floor

Question:

> Find the largest value <= 25.

Use:

```java
map.floorKey(25);
```

Answer:

```text
20
```

---

## 🔥 Pattern 3 — Find Nearest Larger Value

Question:

> Find the smallest number strictly greater than x.

Use:

```java
map.higherKey(x);
```

---

## 🔥 Pattern 4 — Find Nearest Smaller Value

Question:

> Find the largest number strictly smaller than x.

Use:

```java
map.lowerKey(x);
```

---

## 🔥 Pattern 5 — Dynamic Sorted Data

Suppose values arrive one by one:

```text
50
10
40
20
```

TreeMap automatically maintains:

```text
10
20
40
50
```

and supports:

```text
floor
ceiling
higher
lower
```

in:

```text
O(log n)
```

---

# 🧪 27. Complete Example

```java
import java.util.*;

public class Main {

    public static void main(String[] args) {

        TreeMap<Integer, String> map =
            new TreeMap<>();

        map.put(50, "E");
        map.put(10, "A");
        map.put(30, "C");
        map.put(20, "B");
        map.put(40, "D");

        System.out.println(map);

        System.out.println(
            map.firstKey()
        );

        System.out.println(
            map.lastKey()
        );

        System.out.println(
            map.lowerKey(30)
        );

        System.out.println(
            map.higherKey(30)
        );

        System.out.println(
            map.floorKey(35)
        );

        System.out.println(
            map.ceilingKey(35)
        );
    }
}
```

Output:

```text
{10=A, 20=B, 30=C, 40=D, 50=E}
10
50
20
40
30
40
```

---

# ⚖️ 28. TreeMap vs LinkedHashMap

| Feature | LinkedHashMap | TreeMap |
|---|---|---|
| Main structure | Hash table + linked list | Red-Black Tree |
| Ordering | Insertion/access | Sorted keys |
| `get()` | O(1) expected | O(log n) |
| `put()` | O(1) expected | O(log n) |
| Key navigation | Limited | Excellent |
| `floorKey()` | No | Yes |
| `ceilingKey()` | No | Yes |
| `higherKey()` | No | Yes |
| `lowerKey()` | No | Yes |
| Range views | Limited | Rich |
| LRU use | Yes | No |
| Sorted keys | No | Yes |

### Mental Model

```text
LinkedHashMap
    ↓
"Keep my order"

TreeMap
    ↓
"Keep my keys sorted"
```

---

# 🎤 29. 30-Second Interview Answer

> `TreeMap` is a `NavigableMap` implementation based on a Red-Black Tree, which is a self-balancing binary search tree. Unlike HashMap, TreeMap keeps its keys sorted according to natural ordering or a supplied Comparator. Its basic operations such as put, get, and remove take O(log n). Its biggest advantage is navigation operations such as floorKey, ceilingKey, lowerKey, higherKey, and range views such as subMap, headMap, and tailMap.

---

# ⚡ 30. Cheat Sheet

```text
TreeMap
   ↓
NavigableMap
   ↓
Red-Black Tree
   ↓
Sorted keys
```

Basic operations:

```text
put()       → O(log n)

get()       → O(log n)

remove()    → O(log n)
```

Navigation:

```text
firstKey()
lastKey()

lowerKey()
higherKey()

floorKey()
ceilingKey()
```

Range:

```text
subMap()
headMap()
tailMap()
```

Reverse:

```text
descendingMap()
descendingKeySet()
```

Ordering:

```text
Comparable
    OR
Comparator
```

---

# 🧠 Navigation Memory Trick

Remember these four together:

```text
LOWER
  ↓
strictly smaller

FLOOR
  ↓
smaller OR equal

CEILING
  ↓
greater OR equal

HIGHER
  ↓
strictly greater
```

For:

```text
keys = 10, 20, 30

x = 20
```

we get:

```text
lowerKey(20)   → 10

floorKey(20)   → 20

ceilingKey(20) → 20

higherKey(20)  → 30
```

This is extremely important for DSA.

---

# 🎯 31. Interview Questions

## Q1. What is TreeMap?

TreeMap is a `NavigableMap` implementation that stores entries in sorted key order using a Red-Black Tree.

---

## Q2. What data structure does TreeMap use?

A self-balancing Red-Black Tree.

---

## Q3. What is the time complexity of TreeMap `get()`?

```text
O(log n)
```

---

## Q4. Why is TreeMap slower than HashMap for basic lookup?

HashMap provides expected O(1) lookup through hashing, while TreeMap maintains a balanced tree and therefore requires O(log n).

---

## Q5. Does TreeMap sort keys or values?

Keys.

---

## Q6. Does TreeMap preserve insertion order?

No.

It maintains sorted key order.

---

## Q7. What is natural ordering?

The ordering provided by a class's `Comparable` implementation.

Examples:

```text
Integer
String
```

---

## Q8. How can custom ordering be provided?

Using:

```text
Comparator
```

---

## Q9. What does `higherKey()` return?

The smallest key strictly greater than the specified key.

---

## Q10. What does `lowerKey()` return?

The largest key strictly smaller than the specified key.

---

## Q11. What does `ceilingKey()` return?

The smallest key greater than or equal to the specified key.

---

## Q12. What does `floorKey()` return?

The largest key less than or equal to the specified key.

---

## Q13. What is the difference between floor and lower?

```text
floor → <= key

lower → < key
```

---

## Q14. What is the difference between ceiling and higher?

```text
ceiling → >= key

higher → > key
```

---

## Q15. Can TreeMap contain duplicate keys?

No.

Like every Map implementation, keys are unique.

---

## Q16. Can TreeMap contain duplicate values?

Yes.

Multiple keys can map to the same value.

---

## Q17. Can TreeMap contain null keys?

With natural ordering, a null key is generally not supported because keys need to be compared.

---

## Q18. What is `subMap()`?

It provides a view of a range of keys.

---

## Q19. What is `headMap()`?

It provides a view of keys before a specified key, with boundary inclusion configurable.

---

## Q20. What is `tailMap()`?

It provides a view of keys from a specified key onward, with boundary inclusion configurable.

---

# 🏁 32. Final Mental Model

Think of the three major Map implementations like this:

```text
                 Map
                  │
        ┌─────────┼─────────┐
        ↓         ↓         ↓
     HashMap  LinkedHashMap TreeMap
        │         │         │
        ↓         ↓         ↓
     Hashing   Hashing +   Red-Black
                Linked List   Tree
        │         │         │
        ↓         ↓         ↓
      Fast      Fast +      Sorted +
     lookup     ordered     navigable
                iteration
```

---

# 🔥 One-Line Memory Trick

```text
HashMap
→ "Give me fast lookup."

LinkedHashMap
→ "Give me fast lookup + order."

TreeMap
→ "Give me sorted keys + navigation."
```

---

# 🚀 Final Takeaway

> **TreeMap trades HashMap's expected O(1) lookup for O(log n) operations in exchange for sorted keys and powerful navigation.**

The most important TreeMap methods for DSA are:

```text
lowerKey()
floorKey()
ceilingKey()
higherKey()
```

Remember:

```text
lower   → <
floor   → <=
ceiling → >=
higher  → >
```

Once you understand these four operations, TreeMap becomes a powerful tool for **nearest-element, predecessor/successor, range-query, and dynamically sorted data problems**.