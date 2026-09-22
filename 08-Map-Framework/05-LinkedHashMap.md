# 🔗 05 — LinkedHashMap

> **Java Collections Deep Dive → Map Framework**
>
> `LinkedHashMap` extends the functionality of `HashMap` by maintaining a predictable iteration order.

---

# 📑 Table of Contents

- [🧠 1. Introduction](#-1-introduction)
- [🎯 2. Why LinkedHashMap](#-2-why-linkedhashmap)
- [🧬 3. Hierarchy](#-3-hierarchy)
- [⚙️ 4. Internal Structure](#-4-internal-structure)
- [🔗 5. HashMap + Linked List](#-5-hashmap--linked-list)
- [📌 6. Ordering](#-6-ordering)
- [🔢 7. Insertion Order](#-7-insertion-order)
- [🔄 8. Access Order](#-8-access-order)
- [🛠️ 9. Constructors](#-9-constructors)
- [➕ 10. put()](#-10-put)
- [🔍 11. get()](#-11-get)
- [🗑️ 12. remove()](#-12-remove)
- [📦 13. Iteration](#-13-iteration)
- [⚖️ 14. HashMap vs LinkedHashMap](#-14-hashmap-vs-linkedhashmap)
- [🌳 15. LinkedHashMap vs TreeMap](#-15-linkedhashmap-vs-treemap)
- [🧠 16. Access-Order Mode](#-16-access-order-mode)
- [🔥 17. LRU Cache Concept](#-17-lru-cache-concept)
- [🧪 18. Complete Example](#-18-complete-example)
- [⚠️ 19. Common Mistakes](#-19-common-mistakes)
- [🧮 20. Complexity](#-20-complexity)
- [🎯 21. DSA Connection](#-21-dsa-connection)
- [🧠 22. Problem-Solving Pattern](#-22-problem-solving-pattern)
- [🎤 23. 30-Second Interview Answer](#-23-30-second-interview-answer)
- [⚡ 24. Cheat Sheet](#-24-cheat-sheet)
- [🎯 25. Interview Questions](#-25-interview-questions)

---

# 🧠 1. Introduction

`LinkedHashMap` is a class in the Java Collections Framework.

It combines:

    HashMap
       +
    Doubly Linked List

Therefore it provides:

    Fast hash-based lookup
           +
    Predictable iteration order

Declaration:

    public class LinkedHashMap<K,V>
        extends HashMap<K,V>

It implements:

    Map<K,V>

---

# 🎯 2. Why LinkedHashMap

Normal `HashMap` does NOT guarantee iteration order.

Example:

    Map<Integer, String> map =
        new HashMap<>();

    map.put(3, "C");
    map.put(1, "A");
    map.put(2, "B");

Iteration order should not be assumed to be:

    3 → 1 → 2

With `LinkedHashMap`:

    Map<Integer, String> map =
        new LinkedHashMap<>();

    map.put(3, "C");
    map.put(1, "A");
    map.put(2, "B");

Insertion-order iteration is:

    3 → 1 → 2

Therefore:

> Use `LinkedHashMap` when you need HashMap-style lookup together with predictable iteration order.

---

# 🧬 3. Hierarchy

Conceptually:

    Map
     │
     └── HashMap
           │
           └── LinkedHashMap

Important inheritance:

    Object
       ↓
    AbstractMap
       ↓
    HashMap
       ↓
    LinkedHashMap

Interfaces include:

    Map

`LinkedHashMap` inherits most HashMap behavior and adds ordering functionality.

---

# ⚙️ 4. Internal Structure

The most important internal idea is:

    LinkedHashMap
         │
         ├── Hash table
         │
         └── Doubly linked list

Visual model:

    Hash Table

    Bucket 0
       ↓
    [Entry]

    Bucket 1
       ↓
    [Entry] → [Entry]

    Bucket 2
       ↓
    [Entry]

         +

    Linked List

    HEAD
      ↓
    [A] ⇄ [B] ⇄ [C] ⇄ [D]
                              ↑
                             TAIL

The hash table provides efficient lookup.

The linked list maintains iteration order.

---

# 🔗 5. HashMap + Linked List

The key difference is that `LinkedHashMap` maintains links between entries.

Conceptually each entry contains:

    key
    value
    hash
    next

and additionally maintains:

    before
    after

The extra links form a doubly linked list.

Therefore:

    HashMap
       ↓
    hash-based organization

    LinkedHashMap
       ↓
    hash-based organization
       +
    linked ordering

---

# 📌 6. Ordering

`LinkedHashMap` supports two major ordering modes:

    1. Insertion-order
    2. Access-order

Default:

    Insertion-order

Optional:

    Access-order

---

# 🔢 7. Insertion Order

Insertion order means entries are iterated in the order in which they were inserted.

Example:

    LinkedHashMap<Integer, String> map =
        new LinkedHashMap<>();

    map.put(10, "A");
    map.put(20, "B");
    map.put(30, "C");

Iteration:

    10 → A
    20 → B
    30 → C

The order is predictable.

---

# 🔄 8. Access Order

`LinkedHashMap` can also maintain entries according to access order.

Constructor:

    new LinkedHashMap<>(
        initialCapacity,
        loadFactor,
        true
    );

The third argument:

    true

means:

    accessOrder = true

When entries are accessed, they can be moved toward the end of the linked list.

Example:

    A → B → C

Access:

    A

New order:

    B → C → A

Access:

    C

New order:

    B → A → C

This behavior is useful for implementing LRU-style caches.

---

# 🛠️ 9. Constructors

Common constructors include:

    LinkedHashMap()

    LinkedHashMap(int initialCapacity)

    LinkedHashMap(
        int initialCapacity,
        float loadFactor
    )

    LinkedHashMap(
        int initialCapacity,
        float loadFactor,
        boolean accessOrder
    )

---

## Default Constructor

    Map<Integer, String> map =
        new LinkedHashMap<>();

Uses:

    insertion order

---

## Access Order Constructor

    Map<Integer, String> map =
        new LinkedHashMap<>(
            16,
            0.75f,
            true
        );

Here:

    true

means:

    access order enabled

---

# ➕ 10. put()

`put()` inserts a key-value mapping.

Example:

    LinkedHashMap<Integer, String> map =
        new LinkedHashMap<>();

    map.put(1, "Java");
    map.put(2, "Spring");
    map.put(3, "React");

Iteration:

    1 → Java
    2 → Spring
    3 → React

Expected average complexity:

    O(1)

---

## Duplicate Key

Suppose:

    map.put(1, "Java");

Then:

    map.put(1, "Spring");

The value changes:

    1 → Spring

The key is not duplicated.

In insertion-order mode, updating an existing key does not make it a new insertion.

---

# 🔍 11. get()

Example:

    String value = map.get(2);

Expected average complexity:

    O(1)

In insertion-order mode:

    get()

does not change the iteration order.

In access-order mode:

    get()

can move the accessed entry to the end.

---

# 🗑️ 12. remove()

Example:

    map.remove(2);

The corresponding entry is removed.

Both:

    HashMap

and:

    LinkedHashMap

provide expected O(1) removal by key.

The linked-list links are also adjusted.

Example:

Before:

    A ⇄ B ⇄ C

Remove B:

    A ⇄ C

---

# 📦 13. Iteration

One of the biggest advantages of LinkedHashMap is predictable iteration.

Example:

    LinkedHashMap<Integer, String> map =
        new LinkedHashMap<>();

    map.put(3, "C");
    map.put(1, "A");
    map.put(2, "B");

Iteration:

    for (Map.Entry<Integer, String> entry :
         map.entrySet()) {

        System.out.println(
            entry.getKey() + " = " +
            entry.getValue()
        );
    }

Output:

    3 = C
    1 = A
    2 = B

The iteration follows insertion order.

---

# ⚖️ 14. HashMap vs LinkedHashMap

| Feature | HashMap | LinkedHashMap |
|---|---|---|
| Hash-based | Yes | Yes |
| Key-value storage | Yes | Yes |
| Allows one null key | Yes | Yes |
| Allows null values | Yes | Yes |
| Predictable iteration order | No | Yes |
| Default order | No guaranteed order | Insertion order |
| Access order | No | Optional |
| Average get() | O(1) | O(1) |
| Average put() | O(1) | O(1) |
| Extra linked structure | No | Yes |
| Memory usage | Lower | Higher |
| LRU cache support | Not directly | Yes |

Important:

> `HashMap` should not be used when your logic depends on iteration order.

---

# 🌳 15. LinkedHashMap vs TreeMap

| Feature | LinkedHashMap | TreeMap |
|---|---|---|
| Main structure | Hash table + linked list | Red-black tree |
| Ordering | Insertion/access | Sorted order |
| Average get() | O(1) | O(log n) |
| Average put() | O(1) | O(log n) |
| Keys sorted | No | Yes |
| Null key | Allows one | Generally does not allow natural-order null key |
| Memory | Higher than HashMap | Tree nodes have overhead |
| Use case | Predictable order | Sorted keys |

Mental shortcut:

    HashMap
        ↓
    Fast lookup

    LinkedHashMap
        ↓
    Fast lookup + predictable order

    TreeMap
        ↓
    Sorted keys

---

# 🧠 16. Access-Order Mode

The constructor:

    LinkedHashMap(
        int initialCapacity,
        float loadFactor,
        boolean accessOrder
    )

controls ordering.

If:

    accessOrder = false

then:

    insertion order

If:

    accessOrder = true

then:

    access order

Example:

    LinkedHashMap<Integer, String> map =
        new LinkedHashMap<>(
            16,
            0.75f,
            true
        );

Insert:

    1 → A
    2 → B
    3 → C

Order:

    1 → 2 → 3

Access:

    map.get(1);

New order:

    2 → 3 → 1

Access:

    map.get(2);

New order:

    3 → 1 → 2

The recently accessed entry moves toward the end.

---

# 🔥 17. LRU Cache Concept

LRU means:

    Least Recently Used

Suppose cache capacity is:

    3

Current order:

    A → B → C

Here:

    A = least recently used
    C = most recently used

Access:

    A

Order becomes:

    B → C → A

Now:

    B = least recently used

If a new item D arrives:

    B

can be removed.

New order:

    C → A → D

`LinkedHashMap` provides functionality that makes this style of cache implementation convenient.

---

# 🧪 18. Complete Example

    import java.util.LinkedHashMap;
    import java.util.Map;

    public class Main {

        public static void main(String[] args) {

            LinkedHashMap<Integer, String> map =
                new LinkedHashMap<>();

            map.put(101, "Java");
            map.put(102, "Spring");
            map.put(103, "React");

            for (Map.Entry<Integer, String> entry :
                 map.entrySet()) {

                System.out.println(
                    entry.getKey() + " = " +
                    entry.getValue()
                );
            }
        }
    }

Output:

    101 = Java
    102 = Spring
    103 = React

---

# 🔥 Access-Order Example

    import java.util.*;

    public class Main {

        public static void main(String[] args) {

            LinkedHashMap<Integer, String> map =
                new LinkedHashMap<>(
                    16,
                    0.75f,
                    true
                );

            map.put(1, "A");
            map.put(2, "B");
            map.put(3, "C");

            System.out.println(map);

            map.get(1);

            System.out.println(map);

            map.get(2);

            System.out.println(map);
        }
    }

Output:

    {1=A, 2=B, 3=C}

    {2=B, 3=C, 1=A}

    {3=C, 1=A, 2=B}

The accessed entries move to the end.

---

# ⚠️ 19. Common Mistakes

## ❌ Mistake 1

Thinking LinkedHashMap sorts keys.

It does not.

It maintains insertion order by default.

For sorted keys use:

    TreeMap

---

## ❌ Mistake 2

Thinking LinkedHashMap is just a HashMap with sorted keys.

Incorrect.

It is:

    Hash table
        +
    linked list

---

## ❌ Mistake 3

Assuming HashMap maintains insertion order.

It does not guarantee this.

---

## ❌ Mistake 4

Thinking `get()` always changes LinkedHashMap order.

Only access-order mode changes ordering through access.

Default mode is insertion-order.

---

## ❌ Mistake 5

Thinking updating a key creates a second key.

It doesn't.

A Map cannot contain duplicate keys.

---

## ❌ Mistake 6

Thinking LinkedHashMap has exactly the same memory usage as HashMap.

LinkedHashMap maintains additional links, so it requires extra memory.

---

# 🧮 20. Complexity

Typical expected complexity:

| Operation | LinkedHashMap |
|---|---:|
| `put()` | O(1) expected |
| `get()` | O(1) expected |
| `remove()` | O(1) expected |
| `containsKey()` | O(1) expected |
| `containsValue()` | O(n) |
| Iteration | O(n) |
| Space | O(n) |

The linked list does not make normal key lookup O(n).

The hash table is still responsible for efficient lookup.

---

# 🎯 21. DSA Connection

LinkedHashMap is useful when a problem requires:

    Hashing
       +
    Maintaining order

Common patterns include:

    1. First occurrence tracking
    2. Ordered frequency maps
    3. LRU cache
    4. Maintaining insertion order
    5. Ordered deduplication
    6. Cache design

---

## 🔥 Pattern 1 — Ordered Deduplication

Suppose:

    nums = [4, 2, 4, 1, 2, 3]

You want unique values while preserving first-seen order:

    4 → 2 → 1 → 3

A LinkedHashMap can maintain that order.

---

## 🔥 Pattern 2 — Frequency + Order

Suppose:

    nums = [3, 1, 3, 2, 1]

A LinkedHashMap can maintain:

    3 → 2
    1 → 2
    2 → 1

The keys remain in first-insertion order:

    3 → 1 → 2

---

## 🔥 Pattern 3 — LRU Cache

Use:

    LinkedHashMap

with:

    accessOrder = true

Mental model:

    access
       ↓
    move to end

    new item
       ↓
    remove first item

This gives the basic behavior required by an LRU cache.

---

# 🧠 22. Problem-Solving Pattern

When you see a DSA problem, ask:

### Question 1

Do I need fast key lookup?

    Yes
       ↓
    HashMap family

### Question 2

Do I also need insertion order?

    Yes
       ↓
    LinkedHashMap

### Question 3

Do I need sorted keys?

    Yes
       ↓
    TreeMap

### Question 4

Do I need recently-used ordering?

    Yes
       ↓
    LinkedHashMap
    accessOrder = true

---

# 🎤 23. 30-Second Interview Answer

> `LinkedHashMap` is a subclass of `HashMap` that maintains a doubly linked list of its entries. This allows it to provide hash-based expected O(1) lookup while also maintaining a predictable iteration order. By default, it maintains insertion order, but it can also operate in access-order mode. Access-order is particularly useful for implementing LRU-style caches. The trade-off is additional memory compared with HashMap because LinkedHashMap maintains the linked-list structure.

---

# ⚡ 24. Cheat Sheet

    LinkedHashMap
         │
         ├── Hash table
         │      ↓
         │   fast lookup
         │
         └── Doubly linked list
                ↓
             ordering

Default:

    insertionOrder = true conceptually
    accessOrder = false

Optional:

    accessOrder = true

Complexity:

    put()         → O(1) expected
    get()         → O(1) expected
    remove()      → O(1) expected
    containsKey() → O(1) expected
    iteration     → O(n)

---

# 🧠 Memory Trick

Remember:

    HashMap
        =
    Hashing

    LinkedHashMap
        =
    Hashing
        +
    Linking

    TreeMap
        =
    Tree / Sorting

Therefore:

    HashMap
    → Fast lookup

    LinkedHashMap
    → Fast lookup + order

    TreeMap
    → Sorted keys

---

# 🎯 25. Interview Questions

## Q1. What is LinkedHashMap?

`LinkedHashMap` is a HashMap-based implementation that additionally maintains a doubly linked list to provide predictable iteration order.

---

## Q2. What order does LinkedHashMap maintain by default?

Insertion order.

---

## Q3. Can LinkedHashMap maintain access order?

Yes.

Use the constructor with:

    accessOrder = true

---

## Q4. How does LinkedHashMap maintain order?

It maintains additional links between entries, forming a doubly linked list.

---

## Q5. Is LinkedHashMap sorted?

No.

It maintains insertion order by default, not sorted key order.

---

## Q6. Which collection should be used for sorted keys?

`TreeMap`.

---

## Q7. What is the average time complexity of get()?

Expected:

    O(1)

---

## Q8. What is the difference between HashMap and LinkedHashMap?

HashMap does not guarantee iteration order, while LinkedHashMap maintains predictable insertion order by default.

---

## Q9. Why does LinkedHashMap consume more memory than HashMap?

Because it maintains additional links between entries for ordering.

---

## Q10. What is access-order mode?

In access-order mode, entries are rearranged based on access, with recently accessed entries moved toward the end.

---

## Q11. Why is LinkedHashMap useful for LRU caches?

Because access-order mode can maintain entries from least recently accessed to most recently accessed, allowing the oldest entry to be identified easily.

---

## Q12. Does updating an existing key change insertion order?

In normal insertion-order mode, updating the value associated with an existing key does not create a new entry or make it a new insertion.

---

## Q13. Does get() change order?

In default insertion-order mode, no.

In access-order mode, yes, accessing an entry can move it toward the end.

---

## Q14. What is the internal combination used by LinkedHashMap?

Conceptually:

    Hash table
        +
    Doubly linked list

---

# 🏁 Final Mental Model

    LinkedHashMap
           │
           ├───────────────┐
           ↓               ↓
      Hash Table       Linked List
           │               │
           ↓               ↓
      Fast lookup      Predictable order
           │               │
           └───────┬───────┘
                   ↓
            LinkedHashMap

### Default

    put(A)
    put(B)
    put(C)

    Iteration:

    A → B → C

### Access Order

    A → B → C

    get(A)

    B → C → A

    get(B)

    C → A → B

### One-line memory trick

    HashMap       → hashing
    LinkedHashMap → hashing + order
    TreeMap       → sorted keys

> **🚀 Core takeaway:** LinkedHashMap gives you the speed characteristics of hashing with predictable ordering. When your DSA or backend problem says **"maintain order while doing fast key-based lookup"**, LinkedHashMap should immediately come to mind.