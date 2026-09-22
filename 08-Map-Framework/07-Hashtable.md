# 🧱 07 — Hashtable

> **Java Collections Deep Dive → Map Framework**
>
> `Hashtable` is a legacy, synchronized `Map` implementation that stores key-value pairs using hashing.

---

# 📑 Table of Contents

- [🧠 1. Introduction](#-1-introduction)
- [🎯 2. Why Hashtable Exists](#-2-why-hashtable-exists)
- [🧬 3. Hashtable Hierarchy](#-3-hashtable-hierarchy)
- [⚙️ 4. Internal Structure](#-4-internal-structure)
- [🔐 5. Synchronization](#-5-synchronization)
- [🚫 6. Null Keys and Values](#-6-null-keys-and-values)
- [➕ 7. put()](#-7-put)
- [🔍 8. get()](#-8-get)
- [🗑️ 9. remove()](#-9-remove)
- [🔎 10. containsKey() and containsValue()](#-10-containskey-and-containsvalue)
- [📦 11. Iteration](#-11-iteration)
- [🛠️ 12. Constructors](#-12-constructors)
- [📈 13. Capacity and Load Factor](#-13-capacity-and-load-factor)
- [🔄 14. Rehashing](#-14-rehashing)
- [⚔️ 15. Hashtable vs HashMap](#-15-hashtable-vs-hashmap)
- [🔒 16. Hashtable vs ConcurrentHashMap](#-16-hashtable-vs-concurrenthashmap)
- [🧩 17. Legacy Methods](#-17-legacy-methods)
- [⚠️ 18. Common Mistakes](#-18-common-mistakes)
- [🧮 19. Complexity](#-19-complexity)
- [🎯 20. DSA Connection](#-20-dsa-connection)
- [🧠 21. Problem-Solving Pattern](#-21-problem-solving-pattern)
- [🧪 22. Complete Example](#-22-complete-example)
- [🎤 23. 30-Second Interview Answer](#-23-30-second-interview-answer)
- [⚡ 24. Cheat Sheet](#-24-cheat-sheet)
- [🎯 25. Interview Questions](#-25-interview-questions)
- [🏁 26. Final Mental Model](#-26-final-mental-model)

---

# 🧠 1. Introduction

`Hashtable` is one of Java's original collection classes.

It implements:

```java
Map<K, V>
```

and extends:

```java
Dictionary<K, V>
```

Declaration:

```java
public class Hashtable<K, V>
    extends Dictionary<K, V>
    implements Map<K, V>,
               Cloneable,
               Serializable
```

It stores data as:

```text
key → value
```

using hashing.

Example:

```java
Hashtable<Integer, String> table =
    new Hashtable<>();

table.put(101, "Java");
table.put(102, "Spring");
```

---

# 🎯 2. Why Hashtable Exists

`Hashtable` was introduced before the modern Collections Framework.

It is:

```text
OLD / LEGACY
      +
HASH-BASED
      +
SYNCHRONIZED
```

Historically, it provided thread-safe access to a hash table.

However, for new concurrent applications, `ConcurrentHashMap` is generally the more appropriate modern choice.

---

# 🧬 3. Hashtable Hierarchy

The hierarchy is different from `HashMap`.

```text
Object
   ↓
Dictionary
   ↓
Hashtable
```

And `Hashtable` also implements:

```text
Map
Cloneable
Serializable
```

Important:

> `Hashtable` is a legacy class, but it still implements the modern `Map` interface.

---

# ⚙️ 4. Internal Structure

Conceptually:

```text
Hashtable
    │
    └── Hash table
            │
            ├── Bucket 0
            ├── Bucket 1
            ├── Bucket 2
            ├── ...
            └── Bucket n
```

A key is processed using its hash.

Conceptually:

```text
key
 ↓
hashCode()
 ↓
hash
 ↓
bucket index
 ↓
bucket
 ↓
matching key
 ↓
value
```

If multiple keys map to the same bucket, collisions need to be handled.

The exact internal implementation details can vary across Java versions, so avoid assuming that every implementation has exactly the same bucket-node structure.

---

# 🔐 5. Synchronization

One of the most important characteristics of `Hashtable` is that its major methods are synchronized.

Conceptually:

```text
put()
get()
remove()
containsKey()
...
```

are synchronized operations.

Example:

```java
Hashtable<Integer, String> table =
    new Hashtable<>();

table.put(1, "Java");
```

The operation is synchronized.

## ⚠️ Important Thread-Safety Point

Synchronized does **NOT** automatically mean:

> "Every multi-step operation is automatically safe."

For example:

```java
if (!table.containsKey(key)) {
    table.put(key, value);
}
```

The individual methods are synchronized, but the entire:

```text
containsKey()
      +
    put()
```

sequence is not automatically one atomic operation.

Another thread can modify the table between them.

This is an important interview trap.

---

# 🚫 6. Null Keys and Values

Unlike `HashMap`, `Hashtable` does **NOT** allow:

```text
null key
```

or:

```text
null value
```

Example:

```java
Hashtable<Integer, String> table =
    new Hashtable<>();

table.put(null, "Java");
```

This results in:

```text
NullPointerException
```

Similarly:

```java
table.put(1, null);
```

also results in:

```text
NullPointerException
```

## 🧠 Why?

`Hashtable` requires keys and values to be non-null.

Remember:

```text
HashMap
    → one null key
    → multiple null values

Hashtable
    → null key ❌
    → null value ❌
```

---

# ➕ 7. put()

`put()` inserts or updates a key-value pair.

Example:

```java
Hashtable<Integer, String> table =
    new Hashtable<>();

table.put(1, "Java");
table.put(2, "Spring");
table.put(3, "React");
```

Conceptually:

```text
1 → Java
2 → Spring
3 → React
```

Expected average complexity:

```text
O(1)
```

assuming good hashing and normal conditions.

## 🔄 Updating an Existing Key

Example:

```java
table.put(1, "Java");

table.put(1, "Advanced Java");
```

Result:

```text
1 → Advanced Java
```

The key is not duplicated.

---

# 🔍 8. get()

Example:

```java
String value = table.get(2);
```

Result:

```text
Spring
```

Expected average complexity:

```text
O(1)
```

Conceptually:

```text
key
 ↓
hash
 ↓
bucket
 ↓
matching key
 ↓
value
```

---

# 🗑️ 9. remove()

Example:

```java
table.remove(2);
```

The mapping:

```text
2 → Spring
```

is removed.

Expected average complexity:

```text
O(1)
```

assuming good hashing.

---

# 🔎 10. containsKey() and containsValue()

## containsKey()

Checks whether a key exists.

```java
table.containsKey(1);
```

Result:

```text
true
```

Expected average complexity:

```text
O(1)
```

## containsValue()

Checks whether a value exists.

```java
table.containsValue("Java");
```

Result:

```text
true
```

This requires searching through entries.

Typical complexity:

```text
O(n)
```

---

# 📦 11. Iteration

`Hashtable` can be traversed using modern `Map` methods.

Example:

```java
for (Map.Entry<Integer, String> entry :
     table.entrySet()) {

    System.out.println(
        entry.getKey() + " = " +
        entry.getValue()
    );
}
```

You can also use:

```java
table.keySet();
table.values();
table.entrySet();
```

## 🧓 Enumeration

Because `Hashtable` is a legacy class, it also supports `Enumeration`.

Example:

```java
Enumeration<Integer> keys =
    table.keys();

while (keys.hasMoreElements()) {

    Integer key =
        keys.nextElement();

    System.out.println(key);
}
```

`Enumeration` is an older traversal mechanism.

Modern Java code generally prefers:

```text
Iterator
```

or:

```text
for-each
```

over legacy `Enumeration` when possible.

---

# 🛠️ 12. Constructors

Common constructors include:

```java
Hashtable()
```

```java
Hashtable(int initialCapacity)
```

```java
Hashtable(
    int initialCapacity,
    float loadFactor
)
```

```java
Hashtable(
    Map<? extends K, ? extends V> t
)
```

## Default Constructor

```java
Hashtable<Integer, String> table =
    new Hashtable<>();
```

## Initial Capacity

```java
Hashtable<Integer, String> table =
    new Hashtable<>(32);
```

## Capacity + Load Factor

```java
Hashtable<Integer, String> table =
    new Hashtable<>(
        32,
        0.75f
    );
```

---

# 📈 13. Capacity and Load Factor

Two important concepts:

```text
Capacity
```

and:

```text
Load Factor
```

## Capacity

Capacity represents the number of buckets available in the internal hash table.

```text
Capacity
   ↓
Number of buckets
```

## Load Factor

Load factor controls how full the table can become before resizing.

Conceptually:

```text
threshold =
    capacity × loadFactor
```

When the number of entries reaches the resizing threshold, rehashing can occur.

---

# 🔄 14. Rehashing

Suppose:

```text
capacity = 11

loadFactor = 0.75
```

Then approximately:

```text
threshold =
11 × 0.75

= 8.25
```

When the table reaches its resizing threshold, `Hashtable` expands and redistributes entries.

Conceptually:

```text
Old Table
    ↓
[0][1][2][3][4]
    ↓
  resize
    ↓
New Table
    ↓
[0][1][2][3][4][5][6]...
```

The exact resizing formula is implementation-specific, so focus on the concept rather than memorizing a particular formula.

---

# ⚔️ 15. Hashtable vs HashMap

| Feature | Hashtable | HashMap |
|---|---|---|
| Introduced | Legacy Java | Collections Framework |
| Thread-safe methods | Yes, synchronized | No |
| Null key | ❌ | One allowed |
| Null values | ❌ | Multiple allowed |
| Performance | Generally slower under contention | Generally faster for non-concurrent use |
| Legacy | Yes | No |
| Map interface | Yes | Yes |
| Expected `get()` | O(1) | O(1) |
| Expected `put()` | O(1) | O(1) |
| Modern default choice | Usually no | Yes for non-concurrent use |

Mental model:

```text
Hashtable
    ↓
Legacy + synchronized

HashMap
    ↓
Modern + non-synchronized
```

---

# 🔒 16. Hashtable vs ConcurrentHashMap

This is an important interview comparison.

| Feature | Hashtable | ConcurrentHashMap |
|---|---|---|
| Thread-safe | Yes | Yes |
| Legacy | Yes | No |
| Null key | ❌ | ❌ |
| Null value | ❌ | ❌ |
| Concurrency design | Broad synchronization | Designed for concurrent access |
| Modern concurrent choice | Usually no | Yes |
| Performance under concurrency | Can be limiting | Generally scales better |

The key difference is the concurrency design.

`Hashtable` synchronizes its operations broadly.

`ConcurrentHashMap` was designed specifically for highly concurrent access.

---

# 🧩 17. Legacy Methods

Because `Hashtable` extends the old `Dictionary` class, it contains older methods such as:

```text
put()
get()
remove()
keys()
elements()
isEmpty()
size()
```

Modern code can still use the `Map` interface methods:

```text
keySet()
values()
entrySet()
containsKey()
containsValue()
```

---

# ⚠️ 18. Common Mistakes

## ❌ Mistake 1 — Thinking Hashtable and HashMap are identical

They are not.

Important differences include:

```text
synchronization
null handling
legacy status
```

---

## ❌ Mistake 2 — Thinking Hashtable allows null

It doesn't.

```text
null key    ❌
null value  ❌
```

---

## ❌ Mistake 3 — Thinking Hashtable is the modern concurrent Map

For modern concurrent applications:

```text
ConcurrentHashMap
```

is generally the relevant choice.

---

## ❌ Mistake 4 — Thinking synchronized methods make every compound operation atomic

Example:

```java
if (!table.containsKey(key)) {
    table.put(key, value);
}
```

This entire sequence is not automatically atomic.

---

## ❌ Mistake 5 — Thinking synchronization means faster

Synchronization introduces coordination overhead and can restrict concurrent access.

---

## ❌ Mistake 6 — Thinking Hashtable sorts entries

It does not.

There is no sorted ordering like:

```text
TreeMap
```

---

# 🧮 19. Complexity

Assuming good hash distribution:

| Operation | Average |
|---|---:|
| `put()` | O(1) |
| `get()` | O(1) |
| `remove()` | O(1) |
| `containsKey()` | O(1) |
| `containsValue()` | O(n) |
| Iteration | O(n) |
| Space | O(n) |

Worst-case behavior can depend on the implementation and collision distribution.

For interview purposes:

```text
Hash-based lookup
       ↓
Expected O(1)
```

is the important point.

---

# 🎯 20. DSA Connection

`Hashtable` is historically important for understanding:

```text
Hashing
Hash tables
Collision handling
Key-value lookup
Synchronization
```

However, in modern DSA problems, you will usually encounter:

```text
HashMap
```

rather than:

```text
Hashtable
```

because `HashMap` is the standard choice when synchronization is not required.

---

# 🧠 21. Problem-Solving Pattern

When solving a problem, think:

## Need fast key-value lookup?

```text
HashMap
```

## Need fast lookup + insertion/access order?

```text
LinkedHashMap
```

## Need sorted keys + navigation?

```text
TreeMap
```

## Need modern concurrent key-value access?

```text
ConcurrentHashMap
```

## Hashtable?

```text
Legacy synchronized Map
```

This distinction is very useful in interviews.

---

# 🧪 22. Complete Example

```java
import java.util.Hashtable;
import java.util.Map;

public class Main {

    public static void main(String[] args) {

        Hashtable<Integer, String> table =
            new Hashtable<>();

        table.put(101, "Java");
        table.put(102, "Spring");
        table.put(103, "React");

        System.out.println(
            table.get(101)
        );

        System.out.println(
            table.containsKey(102)
        );

        table.remove(103);

        for (Map.Entry<Integer, String> entry :
             table.entrySet()) {

            System.out.println(
                entry.getKey() +
                " = " +
                entry.getValue()
            );
        }
    }
}
```

Possible output:

```text
Java
true
102 = Spring
101 = Java
```

> Do not depend on the iteration order.

---

# 🚫 Null Example

```java
Hashtable<Integer, String> table =
    new Hashtable<>();

table.put(null, "Java");
```

This throws:

```text
NullPointerException
```

Likewise:

```java
table.put(1, null);
```

also throws:

```text
NullPointerException
```

---

# 🔥 Hashtable Mental Model

```text
Hashtable
     │
     ├── Hashing
     │      ↓
     │   Fast lookup
     │
     ├── Synchronization
     │      ↓
     │   Thread-safe individual operations
     │
     └── Legacy
            ↓
        Older collection
```

---

# 🎤 23. 30-Second Interview Answer

> `Hashtable` is a legacy hash-based implementation of the `Map` interface. Its methods are synchronized, making individual operations thread-safe, and it does not permit null keys or null values. Its basic operations have expected O(1) complexity with good hashing. However, for modern applications, `HashMap` is normally preferred when synchronization isn't required, while `ConcurrentHashMap` is generally preferred for concurrent access.

---

# ⚡ 24. Cheat Sheet

```text
Hashtable
    ↓
Legacy Map
    ↓
Hash-based
    ↓
Synchronized
    ↓
No null key
No null value
```

Complexity:

```text
put()            → O(1) expected
get()            → O(1) expected
remove()         → O(1) expected
containsKey()    → O(1) expected
containsValue()  → O(n)
```

Modern comparison:

```text
HashMap
    ↓
non-synchronized Map

Hashtable
    ↓
legacy synchronized Map

ConcurrentHashMap
    ↓
modern concurrent Map
```

---

# 🧠 Memory Trick

Remember:

```text
HASH + TABLE
      ↓
Hash-based storage

TABLE + synchronized
      ↓
Hashtable

TABLE + old
      ↓
Legacy
```

And:

```text
Hashtable
    ❌ null key
    ❌ null value
```

---

# 🎯 25. Interview Questions

## Q1. What is Hashtable?

`Hashtable` is a legacy, hash-based implementation of the `Map` interface whose methods are synchronized.

---

## Q2. Is Hashtable thread-safe?

Its individual methods are synchronized, providing thread-safe access to individual operations.

---

## Q3. Does Hashtable allow null keys?

No.

---

## Q4. Does Hashtable allow null values?

No.

---

## Q5. What is the average complexity of `get()`?

Expected:

```text
O(1)
```

assuming good hash distribution.

---

## Q6. Is Hashtable ordered?

No.

It does not maintain insertion order or sorted key order.

---

## Q7. Why is Hashtable considered legacy?

It predates the Java Collections Framework and uses the older `Dictionary` class hierarchy.

---

## Q8. Hashtable vs HashMap?

The major differences are:

```text
Hashtable
    → synchronized
    → no null key/value
    → legacy

HashMap
    → not synchronized
    → allows null
    → modern Collections Framework
```

---

## Q9. Hashtable vs ConcurrentHashMap?

Both support concurrent access, but `ConcurrentHashMap` was specifically designed as a modern concurrent collection with better concurrency characteristics.

---

## Q10. Why is Hashtable generally slower than HashMap?

Because its synchronized operations introduce synchronization overhead and can limit concurrent access.

---

## Q11. Can Hashtable have duplicate keys?

No.

Map keys must be unique.

---

## Q12. Can Hashtable have duplicate values?

Yes.

Multiple keys can map to the same value.

---

## Q13. What happens when `put()` is called with an existing key?

The old value is replaced.

Example:

```java
table.put(1, "Java");
table.put(1, "Advanced Java");
```

The final mapping is:

```text
1 → Advanced Java
```

---

## Q14. What is rehashing?

Rehashing is the process of resizing the internal hash table and redistributing existing entries into the new table.

---

## Q15. What is the difference between HashMap and Hashtable regarding null?

```text
HashMap:
    one null key
    multiple null values

Hashtable:
    no null key
    no null values
```

---

# 🏁 26. Final Mental Model

```text
                         Map
                          │
          ┌───────────────┼───────────────┐
          │               │               │
          ▼               ▼               ▼
       HashMap      LinkedHashMap      TreeMap
          │               │               │
          │               │               │
       Hashing       Hashing +       Red-Black
                     Linked List        Tree
          │               │               │
          ▼               ▼               ▼
       Fast          Fast +          Sorted +
      Lookup          Order          Navigation


                  Legacy Branch
                        │
                        ▼
                   Hashtable
                        │
              ┌─────────┴─────────┐
              │                   │
           Hashing          Synchronization
              │                   │
              └─────────┬─────────┘
                        ▼
                   Legacy Map
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

Hashtable
→ "Give me the old synchronized hash-based Map."
```

---

# 🚀 Final Takeaway

> **`Hashtable` is a legacy, synchronized, hash-based `Map` that provides expected O(1) basic lookup but does not allow null keys or values.**

The most important things to remember are:

```text
Hashtable
    ↓
Legacy
    ↓
Hash-based
    ↓
Synchronized methods
    ↓
No null key
No null value
    ↓
Expected O(1) basic operations
```

For modern Java:

```text
HashMap
    → ordinary non-concurrent Map

ConcurrentHashMap
    → modern concurrent Map

TreeMap
    → sorted + navigable Map

Hashtable
    → legacy synchronized Map
```

The key interview distinction is:

```text
Hashtable ≠ HashMap

Hashtable
→ synchronized
→ legacy
→ no null key/value

HashMap
→ non-synchronized
→ modern
→ allows null key/value
```