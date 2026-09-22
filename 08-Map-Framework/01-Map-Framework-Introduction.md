# 🗺️ 01 — Map Framework Introduction

> **Java Collections Deep Dive → Map Framework**
>
> Maps are one of the most important data structures in Java for storing data in **key → value** form.
>
> This note builds the foundation required to understand `HashMap`, `LinkedHashMap`, `TreeMap`, `Hashtable`, `ConcurrentHashMap`, and their internal working.

---

# 📌 Table of Contents

- [🧠 1. What is a Map?](#-1-what-is-a-map)
- [🎯 2. Why Do We Need Map?](#-2-why-do-we-need-map)
- [🔑 3. Key-Value Relationship](#-3-key-value-relationship)
- [🏗️ 4. Map Interface](#-4-map-interface)
- [🌳 5. Map Framework Hierarchy](#-5-map-framework-hierarchy)
- [⚔️ 6. Map vs Collection](#-6-map-vs-collection)
- [🔐 7. Keys and Values](#-7-keys-and-values)
- [🧩 8. Important Map Implementations](#-8-important-map-implementations)
- [📊 9. Map Implementations Comparison](#-9-map-implementations-comparison)
- [🚀 10. HashMap](#-10-hashmap)
- [🔗 11. LinkedHashMap](#-11-linkedhashmap)
- [🌲 12. TreeMap](#-12-treemap)
- [🧵 13. Hashtable](#-13-hashtable)
- [⚡ 14. ConcurrentHashMap](#-14-concurrenthashmap)
- [🧠 15. How Map Storage Works Conceptually](#-15-how-map-storage-works-conceptually)
- [🔍 16. Important Map Methods](#-16-important-map-methods)
- [🛠️ 17. Basic Map Example](#-17-basic-map-example)
- [🔄 18. Iterating Over a Map](#-18-iterating-over-a-map)
- [🎯 19. EntrySet](#-19-entryset)
- [⚡ 20. Complexity Overview](#-20-complexity-overview)
- [🧠 21. DSA Patterns](#-21-dsa-patterns)
- [💻 22. DSA Example — Frequency Counting](#-22-dsa-example--frequency-counting)
- [💻 23. DSA Example — Two Sum](#-23-dsa-example--two-sum)
- [⚠️ 24. Common Mistakes](#-24-common-mistakes)
- [🪤 25. Interview Traps](#-25-interview-traps)
- [🔥 26. Frequently Asked Questions](#-26-frequently-asked-questions)
- [⚡ 27. Quick Revision](#-27-quick-revision)
- [🎤 28. 30-Second Interview Answer](#-28-30-second-interview-answer)
- [📋 29. Map Cheat Sheet](#-29-map-cheat-sheet)

---

# 🧠 1. What is a Map?

A `Map` is a Java data structure that stores data in the form:

    KEY → VALUE

Example:

    Roll Number → Student Name

    101 → "Rahul"

    102 → "Aman"

    103 → "Yash"

Instead of accessing data using an index like an array:

```java
arr[0]
```

we access data using a key:

```java
map.get(101)
```

---

# 🎯 2. Why Do We Need Map?

Suppose we have:

```java
String[] names = {
    "Rahul",
    "Aman",
    "Yash",
    "Rohit"
};
```

If we want:

    Roll Number 103 → Yash

An array does not naturally represent the relationship:

    103 → Yash

A Map does:

    103 → "Yash"

Therefore, Maps are useful whenever data has a natural:

    KEY → VALUE

relationship.

---

# 🔑 3. Key-Value Relationship

A Map contains entries.

Each entry contains:

    Key + Value

Example:

    101 → "Yash"

    102 → "Aman"

    103 → "Rohit"

Here:

    Key:

        101
        102
        103

    Value:

        Yash
        Aman
        Rohit

The complete pair is called a:

    Map.Entry

---

## 🔒 3.1 Are Keys Unique?

Yes.

A Map cannot contain duplicate keys.

Example:

```java
map.put(101, "Yash");

map.put(101, "Rahul");
```

The second insertion does not create another key.

Instead:

    101 → "Rahul"

The value associated with key `101` is replaced.

---

## 📦 3.2 Can Values Be Duplicated?

Yes.

Example:

    101 → "Java"

    102 → "Spring"

    103 → "Java"

Here:

    "Java"

appears twice as a value.

That is completely valid.

---

# 🏗️ 4. Map Interface

`Map` is an interface in:

    java.util

Basic declaration:

```java
public interface Map<K, V>
```

Where:

    K → Key type

    V → Value type

Example:

```java
Map<Integer, String>
```

means:

    Integer → Key

    String  → Value

---

# 🌳 5. Map Framework Hierarchy

A simplified hierarchy:

    Map<K, V>

       │

       ├── HashMap<K, V>

       │      │

       │      └── LinkedHashMap<K, V>

       │

       ├── SortedMap<K, V>

       │      │

       │      └── NavigableMap<K, V>

       │             │

       │             └── TreeMap<K, V>

       │

       ├── Hashtable<K, V>

       │

       └── ConcurrentMap<K, V>

              │

              └── ConcurrentHashMap<K, V>

Important:

    HashMap
    LinkedHashMap
    TreeMap
    Hashtable
    ConcurrentHashMap

are implementations/classes.

While:

    Map
    SortedMap
    NavigableMap
    ConcurrentMap

are interfaces.

---

## 🧩 5.1 Map Does NOT Extend Collection

This is a very important interview point.

The hierarchy is:

    Iterable

       ↓

    Collection

       ↓

    List / Set / Queue

Map is separate:

    Map

Therefore:

    Map ≠ Collection

---

# ⚔️ 6. Map vs Collection

| Feature | Collection | Map |
|---|---|---|
| Stores | Individual elements | Key-value pairs |
| Structure | Element | Entry |
| Duplicate handling | Depends on implementation | Keys are unique |
| Main method | `add()` | `put()` |
| Retrieval | Based on iteration/index/etc. | Based on key |
| Example | List, Set | HashMap, TreeMap |
| Interface | Collection | Map |

---

# 🔐 7. Keys and Values

A Map logically contains:

    Entry<K, V>

Example:

```java
Map<Integer, String> students =
        new HashMap<>();

students.put(101, "Yash");

students.put(102, "Aman");
```

Conceptually:

    Entry<Integer, String>

    101 → Yash

    102 → Aman

---

## 🔑 7.1 Key Requirements

The key should have a meaningful:

    hashCode()

    equals()

relationship for hash-based maps.

For `HashMap`, these methods are extremely important because they participate in locating and comparing keys.

This topic is covered deeply in:

    04-HashCode-and-Equals.md

---

## 💎 7.2 Can a Map Have null Keys?

This depends on the implementation.

For example:

    HashMap

allows one `null` key.

But:

    Hashtable

does not allow null keys or null values.

Therefore, never assume that every Map implementation has identical null behavior.

---

# 🧩 8. Important Map Implementations

The most important implementations for interviews are:

### 🚀 HashMap

General-purpose hash-based Map.

    Average:

    O(1)

for basic operations under normal conditions.

---

### 🔗 LinkedHashMap

HashMap-like behavior with predictable iteration order.

It maintains a linked ordering of entries.

---

### 🌲 TreeMap

Stores keys in sorted order.

Typical basic operation:

    O(log n)

It is based on a balanced tree structure.

---

### 🧵 Hashtable

Legacy synchronized Map implementation.

It does not allow:

    null key

    null value

---

### ⚡ ConcurrentHashMap

Designed for concurrent access.

It belongs to the concurrent collections framework.

It does not allow:

    null key

    null value

---

# 📊 9. Map Implementations Comparison

| Implementation | Ordering | Average Get | Average Put | Null Key | Null Values | Thread Safety |
|---|---|---:|---:|---|---|---|
| HashMap | No guaranteed order | O(1) | O(1) | Yes | Yes | No |
| LinkedHashMap | Insertion/access ordering | O(1) | O(1) | Yes | Yes | No |
| TreeMap | Sorted by key | O(log n) | O(log n) | Generally no natural null key | Yes* | No |
| Hashtable | No guaranteed order | O(1) | O(1) | No | No | Synchronized |
| ConcurrentHashMap | No sorted ordering | O(1) average | O(1) average | No | No | Concurrent |

`*` TreeMap's exact null behavior depends on comparator configuration; with natural ordering, null keys are not supported.

---

# 🚀 10. HashMap

`HashMap` is one of the most frequently used Map implementations.

Import:

```java
import java.util.HashMap;
import java.util.Map;
```

Example:

```java
Map<String, Integer> marks =
        new HashMap<>();

marks.put("Java", 90);

marks.put("DSA", 85);

marks.put("DBMS", 80);
```

Retrieval:

```java
int score = marks.get("Java");
```

---

## 🔥 10.1 Why HashMap Is Fast

Conceptually:

    key

      ↓

    hashCode()

      ↓

    hash calculation

      ↓

    bucket

      ↓

    key comparison

      ↓

    value

This allows HashMap to locate entries efficiently.

Its internal structure is discussed in detail in:

    03-HashMap-Internal-Working.md

---

# 🔗 11. LinkedHashMap

`LinkedHashMap` extends `HashMap`.

Conceptually:

    HashMap

       ↓

    LinkedHashMap

It maintains predictable iteration order.

Example:

```java
Map<Integer, String> map =
        new LinkedHashMap<>();

map.put(3, "C");

map.put(1, "A");

map.put(2, "B");
```

Iteration generally follows insertion order:

    3 → C

    1 → A

    2 → B

It can also be configured for access-order behavior.

---

# 🌲 12. TreeMap

`TreeMap` implements:

    NavigableMap

and therefore also participates in:

    SortedMap

Hierarchy:

    Map

      ↓

    SortedMap

      ↓

    NavigableMap

      ↓

    TreeMap

Keys are maintained according to their ordering.

Example:

```java
Map<Integer, String> map =
        new TreeMap<>();

map.put(30, "C");

map.put(10, "A");

map.put(20, "B");
```

Iteration:

    10 → A

    20 → B

    30 → C

Typical operations:

    get()  → O(log n)

    put()  → O(log n)

    remove() → O(log n)

---

# 🧵 13. Hashtable

`Hashtable` is a legacy Map implementation.

Example:

```java
Hashtable<Integer, String> table =
        new Hashtable<>();

table.put(1, "Java");
```

Important characteristics:

- Synchronized
- Legacy
- Does not allow null keys
- Does not allow null values

For modern concurrent applications, `ConcurrentHashMap` is generally the more relevant API.

---

# ⚡ 14. ConcurrentHashMap

`ConcurrentHashMap` belongs to:

    java.util.concurrent

Example:

```java
ConcurrentHashMap<Integer, String> map =
        new ConcurrentHashMap<>();
```

It is designed for concurrent access.

Important:

    ConcurrentHashMap

does not allow:

    null keys

    null values

It provides significantly more suitable concurrency behavior than simply synchronizing every operation on a legacy Hashtable.

Its internal concurrency model is covered separately in:

    08-ConcurrentHashMap.md

---

# 🧠 15. How Map Storage Works Conceptually

Consider:

```java
Map<Integer, String> map =
        new HashMap<>();

map.put(101, "Yash");
```

Conceptually:

    Key

     │

     ▼

    101

     │

     ▼

    hashCode()

     │

     ▼

    Hash calculation

     │

     ▼

    Bucket

     │

     ▼

    Entry

     │

     ├── Key   → 101

     ├── Value → "Yash"

     └── Next  → ...

The exact internal structure depends on the implementation.

For `HashMap`, the internal details involve buckets and nodes, and collision handling can involve linked structures or tree structures.

---

# 🔍 16. Important Map Methods

## `put()`

Adds or updates a key-value pair.

```java
map.put("Java", 90);
```

If the key already exists:

```java
map.put("Java", 95);
```

the existing value is replaced.

---

## `get()`

Retrieves the value associated with a key.

```java
Integer value = map.get("Java");
```

---

## `getOrDefault()`

Returns the value for a key.

If the key is absent, returns the supplied default.

```java
int value =
        map.getOrDefault("Spring", 0);
```

---

## `containsKey()`

Checks whether a key exists.

```java
map.containsKey("Java");
```

Returns:

    true / false

---

## `containsValue()`

Checks whether a value exists.

```java
map.containsValue(90);
```

---

## `remove()`

Removes a mapping using its key.

```java
map.remove("Java");
```

---

## `size()`

Returns the number of mappings.

```java
map.size();
```

---

## `isEmpty()`

Checks whether the Map contains no entries.

```java
map.isEmpty();
```

---

## `clear()`

Removes all mappings.

```java
map.clear();
```

---

## `keySet()`

Returns a Set view of keys.

```java
Set<String> keys =
        map.keySet();
```

---

## `values()`

Returns a Collection view of values.

```java
Collection<Integer> values =
        map.values();
```

---

## `entrySet()`

Returns a Set view of key-value entries.

```java
Set<Map.Entry<String, Integer>> entries =
        map.entrySet();
```

---

## `putIfAbsent()`

Adds the mapping only if the key does not already have a mapping.

```java
map.putIfAbsent("Java", 90);
```

---

## `replace()`

Replaces an existing value.

```java
map.replace("Java", 95);
```

---

## `replaceAll()`

Applies a function to all mappings.

```java
map.replaceAll(
    (key, value) -> value + 5
);
```

---

## `compute()`

Computes a value for a key.

```java
map.compute(
    "Java",
    (key, value) -> value == null
            ? 1
            : value + 1
);
```

---

## `computeIfAbsent()`

Computes a value only when the key is absent.

```java
map.computeIfAbsent(
    "Java",
    key -> 90
);
```

---

## `computeIfPresent()`

Computes a new value only when the key is present.

```java
map.computeIfPresent(
    "Java",
    (key, value) -> value + 10
);
```

---

## `merge()`

Combines an existing value with a new value.

```java
map.merge(
    "Java",
    1,
    Integer::sum
);
```

This is extremely useful for frequency-counting problems.

---

# 🔄 17. Iterating Over a Map

There are several approaches.

---

## Method 1 — `keySet()`

```java
for (String key : map.keySet()) {
    Integer value = map.get(key);

    System.out.println(
        key + " = " + value
    );
}
```

This works, but for simply processing both key and value, `entrySet()` is usually preferable.

---

## Method 2 — `entrySet()`

```java
for (Map.Entry<String, Integer> entry
        : map.entrySet()) {

    System.out.println(
        entry.getKey()
        + " = "
        + entry.getValue()
    );
}
```

This directly accesses both key and value.

---

## Method 3 — `forEach()`

```java
map.forEach(
    (key, value) ->
        System.out.println(
            key + " = " + value
        )
);
```

---

# 🎯 18. EntrySet

`entrySet()` is extremely important.

It returns a:

    Set<Map.Entry<K, V>>

Each Entry represents:

    Key + Value

Example:

```java
for (Map.Entry<Integer, String> entry
        : map.entrySet()) {

    int key = entry.getKey();

    String value = entry.getValue();
}
```

Think:

    Map

      ↓

    entrySet()

      ↓

    Set<Entry<K,V>>

      ↓

    Entry

      ├── getKey()

      └── getValue()

---

## 🧠 18.1 Why entrySet() Is Important

Suppose:

```java
Map<Integer, String> map;
```

Using:

```java
map.keySet()
```

you first get keys and then potentially perform:

```java
map.get(key)
```

Using:

```java
map.entrySet()
```

you already have:

    key + value

Therefore, when you need both key and value, `entrySet()` is the natural choice.

---

# ⚡ 19. Complexity Overview

| Implementation | get() | put() | remove() | Search/Ordering |
|---|---:|---:|---:|---|
| HashMap | O(1) average | O(1) average | O(1) average | Hash-based |
| LinkedHashMap | O(1) average | O(1) average | O(1) average | Hash + linked ordering |
| TreeMap | O(log n) | O(log n) | O(log n) | Sorted |
| Hashtable | O(1) average | O(1) average | O(1) average | Hash-based |
| ConcurrentHashMap | O(1) average | O(1) average | O(1) average | Concurrent hash-based |

Important:

    O(1)

for HashMap means expected/average constant-time behavior under normal hashing conditions.

It does NOT mean every operation is mathematically guaranteed to always take exactly one step.

---

# 🧠 20. Map and Memory

For a typical hash-based Map, conceptually memory contains:

    Map Object

         ↓

    Internal Table

         ↓

    Buckets

         ↓

    Entries / Nodes

         ↓

    Key + Value

For example:

    "Java" → 90

requires storage for the mapping object/node and references to the key and value objects.

The exact memory layout depends on the JVM, object headers, references, implementation, and runtime configuration.

Do not memorize a fixed byte size for a Map entry.

---

# 🧠 21. DSA Patterns

Maps are extremely important in DSA.

The most important patterns are:

    1. Frequency Counting

    2. Fast Lookup

    3. Complement Lookup

    4. Duplicate Detection

    5. Prefix Sum + HashMap

    6. Grouping

    7. Counting Pairs

    8. Sliding Window

    9. Character Frequency

    10. Index Tracking

---

# 💻 22. DSA Example — Frequency Counting

Problem:

Count the frequency of every number.

Input:

    [1, 2, 2, 3, 1, 2]

Expected:

    1 → 2

    2 → 3

    3 → 1

Solution:

```java
int[] nums = {
    1, 2, 2, 3, 1, 2
};

Map<Integer, Integer> freq =
        new HashMap<>();

for (int num : nums) {
    freq.put(
        num,
        freq.getOrDefault(num, 0) + 1
    );
}
```

Output:

    {1=2, 2=3, 3=1}

---

## 🧠 Pattern

Whenever the problem says:

    frequency

    count occurrences

    how many times

    duplicate count

think:

    HashMap

---

## Complexity

    Time:  O(n) average

    Space: O(k)

where:

    k = number of distinct elements

---

# 💻 23. DSA Example — Two Sum

Problem:

Given an array and target, find two numbers whose sum equals the target.

Example:

    nums = [2, 7, 11, 15]

    target = 9

Answer:

    [0, 1]

---

## HashMap Pattern

For each number:

    required = target - current

Then check whether the required value already exists.

```java
Map<Integer, Integer> map =
        new HashMap<>();

for (int i = 0;
     i < nums.length;
     i++) {

    int required =
            target - nums[i];

    if (map.containsKey(required)) {

        return new int[] {
            map.get(required),
            i
        };
    }

    map.put(nums[i], i);
}
```

The Map stores:

    value → index

---

## Complexity

    Time:  O(n) average

    Space: O(n)

This improves over the brute-force:

    O(n²)

approach.

---

# 🎯 23.1 How to Think About Map Problems

When solving a DSA problem, ask:

    "Do I need fast lookup?"

If yes, consider:

    HashMap / HashSet

Then ask:

    "What should be the key?"

Examples:

    value → index

    number → frequency

    character → frequency

    prefixSum → index

    remainder → index

    state → count

Choosing the correct key is often the main insight.

---

# ⚠️ 24. Common Mistakes

## ❌ Mistake 1 — Duplicate Keys

Thinking:

```java
map.put("Java", 90);

map.put("Java", 95);
```

creates two entries.

It does not.

Final mapping:

    Java → 95

---

## ❌ Mistake 2 — Using `add()`

Map does not use:

```java
add()
```

It uses:

```java
put()
```

---

## ❌ Mistake 3 — Assuming Map Extends Collection

It does not.

    Map

      separate hierarchy

---

## ❌ Mistake 4 — Assuming HashMap Is Sorted

HashMap does not provide sorted-key ordering.

For sorted keys, consider:

    TreeMap

---

## ❌ Mistake 5 — Assuming HashMap Maintains Insertion Order

Do not rely on HashMap iteration order.

If predictable insertion ordering is required:

    LinkedHashMap

---

## ❌ Mistake 6 — Using get() to Test Presence

Consider:

```java
map.get(key)
```

If the result is `null`, the key may be absent.

But some Map implementations allow null values.

Therefore, when the question is specifically:

    "Does this key exist?"

use:

```java
map.containsKey(key)
```

---

# 🪤 25. Interview Traps

### Q: Can Map contain duplicate keys?

    No.

### Q: Can Map contain duplicate values?

    Yes.

### Q: Is Map a Collection?

    No.

### Q: Does HashMap guarantee order?

    No.

### Q: Which Map maintains insertion order?

    LinkedHashMap

### Q: Which Map maintains sorted key order?

    TreeMap

### Q: Which Map is legacy synchronized?

    Hashtable

### Q: Which Map is designed for concurrent access?

    ConcurrentHashMap

### Q: Can HashMap have null key?

    Yes, one null key.

### Q: Can Hashtable have null key?

    No.

### Q: What does put() return?

    The previous value associated with the key, or null if there was no previous mapping.

### Q: What does get() return if key is absent?

    null

### Q: What does entrySet() return?

    Set<Map.Entry<K,V>>

### Q: What does keySet() return?

    Set<K>

### Q: What does values() return?

    Collection<V>

---

# 🔥 26. Frequently Asked Questions

## Q1. Why is Map separate from Collection?

Because Collection represents individual elements, while Map represents relationships between keys and values.

---

## Q2. Why can't Map have duplicate keys?

A key identifies a mapping.

If the same key is inserted again, the existing mapping is updated.

---

## Q3. Why can values be duplicated?

Values do not identify mappings.

Multiple keys can point to the same value.

Example:

    101 → Java

    102 → Java

---

## Q4. Which Map should I use for general-purpose lookup?

A common choice is:

    HashMap

when you need hash-based lookup and do not require ordering.

---

## Q5. Which Map should I use when insertion order matters?

    LinkedHashMap

---

## Q6. Which Map should I use when sorted keys matter?

    TreeMap

---

## Q7. Which Map is designed for concurrent access?

    ConcurrentHashMap

---

## Q8. Why is Hashtable considered legacy?

It is an older synchronized collection API with design characteristics that predate the modern concurrent collections.

---

## Q9. Why does ConcurrentHashMap reject null?

Because allowing null would make it harder to distinguish between:

    "key has no mapping"

and:

    "key maps to null"

especially in concurrent operations.

---

## Q10. Why is HashMap usually O(1)?

It uses hashing to locate an appropriate bucket, allowing expected constant-time lookup under good hash distribution.

---

# ⚡ 27. Quick Revision

    Map

     ↓

    Key → Value

---

    Map

      ├── HashMap

      │      └── LinkedHashMap

      │

      ├── SortedMap

      │      └── NavigableMap

      │             └── TreeMap

      │

      ├── Hashtable

      │

      └── ConcurrentMap

             └── ConcurrentHashMap

---

## Key Rules

    Keys → Unique

    Values → Can duplicate

    Map → Not a Collection

    HashMap → Hash-based

    LinkedHashMap → Predictable ordering

    TreeMap → Sorted keys

    Hashtable → Legacy synchronized

    ConcurrentHashMap → Concurrent access

---

## Important Views

    keySet()

        ↓

    Set<K>

    values()

        ↓

    Collection<V>

    entrySet()

        ↓

    Set<Entry<K,V>>

---

## Most Important Methods

    put()

    get()

    getOrDefault()

    containsKey()

    containsValue()

    remove()

    size()

    isEmpty()

    clear()

    keySet()

    values()

    entrySet()

    putIfAbsent()

    replace()

    replaceAll()

    compute()

    computeIfAbsent()

    computeIfPresent()

    merge()

---

# 🎤 28. 30-Second Interview Answer

> **The Map framework in Java is used to store data in key-value form. Unlike Collection, Map does not store individual elements and does not extend the Collection interface. The Map interface has implementations such as HashMap, LinkedHashMap, TreeMap, Hashtable, and ConcurrentHashMap. HashMap provides expected O(1) lookup, LinkedHashMap maintains predictable iteration ordering, TreeMap maintains sorted keys with O(log n) basic operations, Hashtable is a legacy synchronized implementation, and ConcurrentHashMap is designed for concurrent access. Map keys are unique, while values can be duplicated.**

---

# 📋 29. Map Cheat Sheet

| Concept | Remember |
|---|---|
| Map stores | Key → Value |
| Key | Unique |
| Value | Can duplicate |
| Map extends Collection? | No |
| General-purpose Map | HashMap |
| Predictable insertion/access order | LinkedHashMap |
| Sorted keys | TreeMap |
| Legacy synchronized Map | Hashtable |
| Concurrent Map | ConcurrentHashMap |
| HashMap average get | O(1) |
| TreeMap get | O(log n) |
| Add mapping | put() |
| Read value | get() |
| Check key | containsKey() |
| Check value | containsValue() |
| Keys | keySet() |
| Values | values() |
| Entries | entrySet() |
| Frequency pattern | HashMap |
| Fast lookup pattern | HashMap |
| Two Sum pattern | HashMap |
| Prefix Sum pattern | HashMap |
| Producer | extends |
| Consumer | super |

---

# 🚀 Final Mental Model

    ┌───────────────────────────────┐
    │             MAP               │
    │       KEY  ──────► VALUE      │
    └───────────────────────────────┘
                    │
          ┌─────────┼──────────┐
          ▼         ▼          ▼
       HashMap   LinkedHashMap TreeMap
          │         │           │
        Fast      Ordered      Sorted
        Lookup    Iteration     Keys
          │
          ├───────────────┐
          ▼               ▼
      Hashtable     ConcurrentHashMap
       Legacy           Concurrent
      Synchronized        Access

---

# 🧠 The Interview Connection

    Need KEY → VALUE?

            ↓

           MAP

            ↓

    ┌───────┼────────┐
    ▼       ▼        ▼
  Fast    Ordered   Sorted
  Lookup  Order     Keys
    │       │        │
 HashMap LinkedHashMap TreeMap
    │
    ▼
   DSA
    │
    ├── Frequency
    ├── Two Sum
    ├── Duplicate Detection
    ├── Prefix Sum
    ├── Sliding Window
    ├── Grouping
    └── Fast Lookup

---

> **Core takeaway:**  
> `Map<K, V>` represents a relationship between a unique key and a value. Once you understand **keys, values, entries, hashing, ordering, sorting, and lookup complexity**, the individual Map implementations become much easier to understand.