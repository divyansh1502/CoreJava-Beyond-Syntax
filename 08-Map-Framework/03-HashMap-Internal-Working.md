# ⚙️ 03 — HashMap Internal Working

> **Java Collections Deep Dive → Map Framework**
>
> This note goes beneath the `HashMap` API and explains what actually happens inside `HashMap` when you call `put()`, `get()`, `remove()`, and other operations.
>
> **Prerequisite:** `02-HashMap.md`
>
> **Next:** `04-HashCode-and-Equals.md`

---

# 📑 Table of Contents

- [🧠 1. Why Learn Internal Working?](#-1-why-learn-internal-working)
- [🏗️ 2. High-Level Architecture](#-2-high-level-architecture)
- [📦 3. Internal Data Structure](#-3-internal-data-structure)
- [🔢 4. What is a Bucket?](#-4-what-is-a-bucket)
- [🧮 5. Hashing](#-5-hashing)
- [⚙️ 6. HashMap `hash()` Method](#-6-hashmap-hash-method)
- [📍 7. Calculating Bucket Index](#-7-calculating-bucket-index)
- [➕ 8. Internal Working of `put()`](#-8-internal-working-of-put)
- [🔍 9. Internal Working of `get()`](#-9-internal-working-of-get)
- [🗑️ 10. Internal Working of `remove()`](#-10-internal-working-of-remove)
- [💥 11. Collision](#-11-collision)
- [🔗 12. Collision Handling with Linked Nodes](#-12-collision-handling-with-linked-nodes)
- [🌳 13. Treeification](#-13-treeification)
- [🌲 14. Red-Black Tree Concept](#-14-red-black-tree-concept)
- [📊 15. Treeification Threshold](#-15-treeification-threshold)
- [↩️ 16. Untreeification](#-16-untreeification)
- [📈 17. Resizing](#-17-resizing)
- [🎚️ 18. Capacity](#-18-capacity)
- [⚖️ 19. Load Factor](#-19-load-factor)
- [🚦 20. Threshold](#-20-threshold)
- [🔄 21. Resize Example](#-21-resize-example)
- [🧠 22. Why Power of Two Capacity?](#-22-why-power-of-two-capacity)
- [🎯 23. Why `(n - 1) & hash`?](#-23-why-n---1--hash)
- [🧩 24. `hashCode()` vs HashMap `hash()`](#-24-hashcode-vs-hashmap-hash)
- [🔐 25. Role of `equals()`](#-25-role-of-equals)
- [🧬 26. Complete `put()` Flow](#-26-complete-put-flow)
- [🔎 27. Complete `get()` Flow](#-27-complete-get-flow)
- [🧹 28. Complete `remove()` Flow](#-28-complete-remove-flow)
- [🧠 29. Why HashMap is O(1) Expected](#-29-why-hashmap-is-o1-expected)
- [⚠️ 30. Worst-Case Complexity](#-30-worst-case-complexity)
- [🧵 31. HashMap and Threads](#-31-hashmap-and-threads)
- [💾 32. Memory Perspective](#-32-memory-perspective)
- [🧪 33. Custom Key Example](#-33-custom-key-example)
- [🪤 34. Internal Working Interview Traps](#-34-internal-working-interview-traps)
- [🎯 35. DSA Connection](#-35-dsa-connection)
- [🎤 36. 30-Second Interview Answer](#-36-30-second-interview-answer)
- [⚡ 37. Quick Revision](#-37-quick-revision)
- [📋 38. Internal Working Cheat Sheet](#-38-internal-working-cheat-sheet)
- [🏁 39. Final Mental Model](#-39-final-mental-model)

---

# 🧠 1. Why Learn Internal Working?

Knowing:

```java
map.put("Java", 90);
```

is easy.

An interviewer may ask:

> **"What happens internally when you call `put()`?"**

Now you need to understand:

```text
key
 ↓
hashCode()
 ↓
HashMap hash()
 ↓
bucket index
 ↓
table
 ↓
collision check
 ↓
equals()
 ↓
Node
 ↓
value
```

This is one of the most important Java Collections internals.

---

# 🏗️ 2. High-Level Architecture

Conceptually:

```text
HashMap
   │
   ▼
Node<K,V>[] table
   │
   ├── bucket 0
   ├── bucket 1
   ├── bucket 2
   ├── bucket 3
   ├── ...
   └── bucket n
```

Each bucket can contain:

```text
null
```

or:

```text
Node
```

or:

```text
multiple Nodes
```

or, after treeification:

```text
TreeNode structure
```

### High-Level Mental Model

```text
HashMap
   ↓
Internal table
   ↓
Bucket
   ↓
Node(s)
   ↓
Key + Value
```

---

# 📦 3. Internal Data Structure

In modern Java implementations, `HashMap` internally maintains a table of nodes.

Conceptually:

```java
Node<K,V>[] table;
```

A node conceptually contains:

```java
static class Node<K,V>
        implements Map.Entry<K,V> {

    final int hash;
    final K key;
    V value;
    Node<K,V> next;
}
```

### Important Fields

| Field | Purpose |
|---|---|
| `hash` | Stores the processed hash value |
| `key` | Stores the key |
| `value` | Stores the associated value |
| `next` | Points to the next node in the same bucket |

The exact implementation can vary by JDK version, so focus on the behavior rather than memorizing source-code details.

---

# 🔢 4. What is a Bucket?

A **bucket** is a position in HashMap's internal table.

Suppose:

```text
table length = 16
```

Then there are conceptually:

```text
0
1
2
3
4
...
15
```

bucket positions.

When a key arrives, HashMap calculates which bucket should contain the mapping.

Example:

```text
"Java"
   ↓
hash
   ↓
bucket 7
```

So the mapping goes into bucket 7.

### Important

A bucket is **not a separate collection object**.

It is simply an index in the internal table that can reference an entry or a structure containing multiple entries.

---

# 🧮 5. Hashing

Hashing converts information about a key into a hash value.

For an object:

```java
String key = "Java";

int h = key.hashCode();
```

The `hashCode()` method returns an `int`.

The hash code is then processed by HashMap before the bucket index is calculated.

### General Flow

```text
Key
 ↓
hashCode()
 ↓
HashMap hash spreading
 ↓
Bucket index
```

### Important Distinction

```text
hashCode()
    ≠
bucket index
```

The hash code is an integer.

The bucket index is a valid position inside the current table.

---

# ⚙️ 6. HashMap `hash()` Method

Conceptually, modern Java HashMap uses a hash-spreading operation similar to:

```java
static final int hash(Object key) {
    int h;

    return (key == null)
            ? 0
            : (h = key.hashCode()) ^ (h >>> 16);
}
```

The important operation is:

```text
h ^ (h >>> 16)
```

where:

```text
^   = bitwise XOR
>>> = unsigned right shift
```

### Why Does HashMap Spread the Hash?

The original hash code contains 32 bits.

When a power-of-two table size is used, the bucket calculation depends heavily on lower bits.

HashMap therefore mixes higher bits into lower bits using:

```text
h ^ (h >>> 16)
```

This helps improve the distribution of keys across buckets.

### Example

Conceptually:

```text
Original hash
      ↓
  h ^ (h >>> 16)
      ↓
Spread hash
      ↓
Bucket index
```

---

# 📍 7. Calculating Bucket Index

For a table length `n`, the bucket index is conceptually calculated as:

```text
(n - 1) & hash
```

Example:

```text
n = 16
```

Then:

```text
n - 1 = 15
```

Binary:

```text
15 = 1111₂
```

Therefore:

```text
1111
  &
hash
  ↓
bucket index
```

The result is always between:

```text
0 and n - 1
```

For `n = 16`:

```text
0 to 15
```

---

# 🎯 8. Internal Working of `put()`

Suppose:

```java
Map<String, Integer> map = new HashMap<>();

map.put("Java", 90);
```

Let's follow the process.

---

## Step 1 — Receive Key and Value

```text
key   = "Java"
value = 90
```

---

## Step 2 — Calculate Hash

HashMap obtains:

```java
key.hashCode();
```

Then applies its internal hash-spreading logic.

Conceptually:

```text
hashCode()
   ↓
hash()
   ↓
spread hash
```

---

## Step 3 — Calculate Bucket

Suppose:

```text
table length = 16
```

Then:

```text
index = (16 - 1) & hash
```

Result:

```text
bucket index
```

---

## Step 4 — Check Bucket

HashMap checks:

```text
table[index]
```

There are two major possibilities.

### Case A — Bucket is Empty

```text
table[index] == null
```

A new node can be inserted.

Conceptually:

```text
bucket
  ↓
Node
 ├── hash
 ├── key = "Java"
 ├── value = 90
 └── next = null
```

### Case B — Bucket Already Contains Something

HashMap checks whether an existing entry represents the same key.

It considers:

```text
hash
```

and:

```text
equals()
```

---

# 🔍 Same Key Case

Suppose:

```java
map.put("Java", 90);
map.put("Java", 100);
```

The bucket is already occupied.

HashMap finds the existing key.

If the keys match:

```text
existing value = 90
new value      = 100
```

The value is replaced.

Final mapping:

```text
Java → 100
```

### Important

The key is not duplicated.

The existing mapping's value is updated.

---

# 💥 9. Collision

Suppose:

```text
Key A → Bucket 5
Key B → Bucket 5
```

but:

```text
A.equals(B) == false
```

Then we have a **collision**.

Both mappings must coexist.

Conceptually:

```text
Bucket 5
   ↓
Node A
   ↓
Node B
   ↓
Node C
```

A collision occurs when **different keys map to the same bucket index**.

### Important

A collision does **not** necessarily mean:

```text
hashCode(A) == hashCode(B)
```

Different hash codes can also end up selecting the same bucket because the bucket index is derived from the processed hash and current table capacity.

---

# 🔗 10. Collision Handling with Linked Nodes

Initially, collisions can be represented through linked nodes.

Conceptually:

```text
table[5]
   ↓
Node A
   ↓
Node B
   ↓
Node C
   ↓
null
```

Each node contains a reference to the next node.

When searching:

```java
map.get(key);
```

HashMap examines the entries in the relevant bucket.

It compares candidate entries using their hash and key equality.

---

# 🧠 Important Point

HashMap does **not** say:

```text
Same bucket = same key
```

Instead:

```text
Same bucket
    ↓
Candidate entries
    ↓
Compare hash
    ↓
Compare equals()
    ↓
Correct key?
```

Therefore:

```text
Same bucket
    ≠
Same key
```

And:

```text
Same hash
    ≠
Same object/key
```

---

# 🌳 11. Treeification

If too many entries accumulate in one bucket, traversing a long linked structure can become inefficient.

Modern Java HashMap can convert a heavily collided bucket into a tree structure under suitable conditions.

Conceptually:

### Before

```text
Bucket
   ↓
A
   ↓
B
   ↓
C
   ↓
D
   ↓
E
```

### After Treeification

```text
        C
       / \
      A   D
       \   \
        B   E
```

The actual tree-bin structure is based on a red-black tree.

---

# 🌲 12. Red-Black Tree Concept

A **red-black tree** is a self-balancing binary search tree.

Its purpose is to keep the tree approximately balanced.

Therefore, tree operations can have approximately:

```text
O(log n)
```

behavior.

This helps HashMap avoid very long linear searches inside heavily collided bins.

### Simplified Comparison

```text
Linked structure:

A → B → C → D → E

Search:
O(n)
```

versus:

```text
Tree structure:

       C
      / \
     A   D
      \   \
       B   E

Search:
O(log n)
```

The exact tree-bin behavior has implementation details beyond this simplified model.

---

# 📊 13. Treeification Threshold

A commonly discussed implementation constant is:

```text
TREEIFY_THRESHOLD = 8
```

This means a bucket may be considered for treeification when its bin becomes sufficiently large.

But there is an important condition.

HashMap also has:

```text
MIN_TREEIFY_CAPACITY = 64
```

If the table is still too small, HashMap may resize instead of immediately treeifying the bucket.

Therefore:

```text
bucket becomes large
        ↓
Is table sufficiently large?
     ↙       ↘
   No         Yes
   ↓           ↓
Resize    Treeify may occur
```

### Important Interview Point

Do **not** memorize:

> "8 entries always means treeification."

That statement is incomplete.

The table capacity and other implementation conditions also matter.

---

# ↩️ 14. Untreeification

If a treeified bucket becomes sufficiently small after removals, it can be converted back to a simpler node representation.

Conceptually:

```text
Tree
 ↓
Entries removed
 ↓
Bin becomes small
 ↓
Untreeify
 ↓
Linked representation
```

A commonly referenced implementation threshold is:

```text
UNTREEIFY_THRESHOLD = 6
```

The exact behavior depends on the implementation.

### Important

Do not interpret:

```text
6 entries = always untreeify
```

as an absolute rule.

The threshold is part of the implementation's tree-bin management logic.

---

# 📈 15. Resizing

HashMap does not keep the same table size forever.

As more mappings are added, the table can become too full according to its threshold.

Then HashMap resizes.

Conceptually:

```text
capacity = 16
      ↓
threshold reached
      ↓
capacity = 32
      ↓
threshold recalculated
      ↓
entries redistributed
```

### Why Resize?

A table that becomes too crowded can increase collisions.

More collisions can increase the amount of work needed to find an entry.

Resizing increases the number of buckets and can improve distribution.

---

# 🎚️ 16. Capacity

**Capacity** represents the number of buckets in the internal table.

Common capacities are powers of two:

```text
16
32
64
128
256
...
```

The initial capacity configured by a constructor is not necessarily the same as an immediately allocated internal table size.

Modern HashMap uses lazy table initialization.

For example:

```java
Map<String, Integer> map = new HashMap<>();
```

does not mean that a fully allocated 16-bucket table must already exist immediately after this constructor call.

The internal table is initialized when needed.

---

# ⚖️ 17. Load Factor

The default load factor of HashMap is:

```text
0.75
```

Conceptually:

```text
threshold = capacity × loadFactor
```

For:

```text
capacity   = 16
loadFactor = 0.75
```

we get:

```text
threshold = 16 × 0.75
          = 12
```

### What Does Load Factor Mean?

Load factor controls how full the table can become before HashMap grows.

A lower load factor generally means:

```text
More buckets
↓
Fewer collisions
↓
More memory
```

A higher load factor generally means:

```text
Fewer buckets
↓
Potentially more collisions
↓
Less table memory
```

Therefore load factor represents a **space-versus-collision trade-off**.

---

# 🚦 18. Threshold

Threshold determines approximately when resizing should occur.

Conceptually:

```text
threshold = capacity × loadFactor
```

Example:

```text
capacity   = 16
loadFactor = 0.75
threshold  = 12
```

So once the relevant resize condition is reached, HashMap grows the table.

### Important Distinction

```text
Capacity
   ↓
Number of table positions

Load Factor
   ↓
Controls how full the table should become

Threshold
   ↓
Resize trigger derived from capacity and load factor
```

---

# 🔄 19. Resize Example

Start:

```text
capacity   = 16
load factor = 0.75
```

Therefore:

```text
threshold = 12
```

Suppose entries increase:

```text
1
2
3
...
12
```

When the relevant resize condition is reached, HashMap expands.

Conceptually:

```text
16 buckets
    ↓
32 buckets
```

The resize process also redistributes entries according to the new table structure.

### Important Optimization

Because HashMap capacities are powers of two, resizing can efficiently determine whether an entry remains at its old position or moves by the old capacity.

Conceptually:

```text
old index
   ↓
new index

either:
old index

or:
old index + old capacity
```

This is one of the useful consequences of power-of-two capacities.

---

# 🧠 20. Why Power-of-Two Capacity?

HashMap uses power-of-two capacities because it allows efficient bucket-index calculation.

Instead of:

```text
hash % n
```

the implementation can use:

```text
(n - 1) & hash
```

when `n` is a power of two.

For example:

```text
n = 16

n - 1 = 15

15 = 1111₂
```

Therefore:

```text
hash & 1111₂
```

efficiently selects the relevant lower bits.

### Additional Benefit During Resize

Power-of-two capacity also makes redistribution efficient.

When capacity doubles:

```text
old capacity = 16
new capacity = 32
```

an entry's new position can be determined based on one additional bit of the hash.

Conceptually:

```text
old index
   ↓
new position

old index
   OR
old index + old capacity
```

---

# 🎯 21. Why `(n - 1) & hash`?

Suppose:

```text
n = 16
```

Then:

```text
n - 1 = 15
```

Binary:

```text
0000 1111
```

Suppose:

```text
hash = 1011 0110
```

Then:

```text
1011 0110
0000 1111
-----------
0000 0110
```

Therefore:

```text
index = 6
```

So the key goes to:

```text
bucket 6
```

### Why Not `%`?

Mathematically, for a power-of-two `n`:

```text
hash % n
```

and:

```text
hash & (n - 1)
```

produce the same range of indices for the relevant non-negative index calculation.

The bitwise operation is particularly efficient and fits the internal HashMap design.

---

# 🧩 22. `hashCode()` vs HashMap `hash()`

These are **not the same thing**.

## `hashCode()`

Defined by:

```text
Object
```

and potentially overridden by your class.

Example:

```java
String key = "Java";

int hashCode = key.hashCode();
```

This returns an integer hash code.

---

## HashMap `hash()`

HashMap internally processes the hash code.

Conceptually:

```text
hashCode()
   ↓
bit spreading
   ↓
HashMap hash value
   ↓
bucket index
```

Therefore:

```text
hashCode()
    ≠
HashMap hash()
    ≠
bucket index
```

These are different stages.

---

# 🔐 23. Role of `equals()`

Suppose two keys land in the same bucket.

HashMap needs to determine:

> **"Is this actually the same key?"**

It uses equality comparison.

Conceptually:

```text
hash matches?
     ↓
equals()?
     ↓
same logical key?
```

For example:

```java
Map<String, Integer> map = new HashMap<>();

map.put("Java", 90);

System.out.println(
    map.get(new String("Java"))
);
```

The two String objects can be different objects, but:

```java
"Java".equals(new String("Java"))
```

returns:

```text
true
```

Therefore HashMap can locate the existing mapping.

### Key Principle

HashMap generally uses:

```text
hash
  +
equals()
```

to identify a key.

---

# 🧬 24. Complete `put()` Flow

Let's visualize:

```text
map.put(key, value)
        │
        ▼
Is table initialized?
        │
        ▼
Calculate hash
        │
        ▼
Calculate index
        │
        ▼
table[index]
        │
   ┌────┴─────┐
   │          │
 null      non-null
   │          │
   ▼          ▼
Create      Compare
 Node       entries
              │
        ┌─────┴──────┐
        │            │
    Same key     Different key
        │            │
        ▼            ▼
Replace value    Collision
                     │
                     ▼
               Add/traverse node
                     │
                     ▼
               Treeify if required
                     │
                     ▼
                  size++
                     │
                     ▼
             Check resize threshold
```

### Simplified Algorithm

```java
map.put(key, value);
```

Conceptually:

```text
1. Calculate hash.
2. Calculate bucket index.
3. Check table[index].
4. If empty → create node.
5. If occupied → compare candidate key.
6. If same key → replace value.
7. If different key → handle collision.
8. Treeify if required.
9. Increase size when a new mapping is added.
10. Resize if threshold is reached.
```

---

# 🔎 25. Complete `get()` Flow

Suppose:

```java
map.get(key);
```

Flow:

```text
get(key)
   │
   ▼
Calculate hash
   │
   ▼
Calculate bucket index
   │
   ▼
table[index]
   │
   ▼
First node?
   │
   ├── Matching key → return value
   │
   ▼
Traverse collision nodes
   │
   ▼
Compare hash + equals()
   │
   ├── Found → return value
   │
   └── Not found → null
```

### Important

`get()` does not search the entire HashMap.

It first uses the key's hash to locate the relevant bucket.

Then it searches only the candidate entries inside that bucket.

---

# 🧹 26. Complete `remove()` Flow

Suppose:

```java
map.remove(key);
```

Flow:

```text
remove(key)
   │
   ▼
Calculate hash
   │
   ▼
Calculate bucket index
   │
   ▼
Locate bucket
   │
   ▼
Compare candidate key
   │
   ├── Not found → null
   │
   ▼
Found
   │
   ▼
Remove node
   │
   ▼
Return previous value
```

If the bucket is treeified, tree-specific removal logic is used.

### Important

Removing an entry decreases the map's size.

If a tree bin becomes sufficiently small, the implementation may convert it back to a simpler representation.

---

# 🧠 27. Why HashMap is O(1) Expected

Suppose there are:

```text
1,000,000 entries
```

If HashMap distributes keys effectively, each lookup does not need to inspect all:

```text
1,000,000
```

entries.

Instead:

```text
key
 ↓
hash
 ↓
bucket
 ↓
small number of candidates
 ↓
equals()
 ↓
value
```

This is why basic operations are **expected O(1)** under normal hash distribution.

### Important

The `O(1)` claim is an expected/average-case characterization, not an unconditional guarantee.

---

# ⚠️ 28. Worst-Case Complexity

If many keys collide into the same bucket, the bucket can become expensive to search.

A heavily collided linked structure can approach:

```text
O(n)
```

lookup behavior.

Modern Java HashMap can treeify heavily collided bins under suitable conditions, giving tree-bin lookup approximately:

```text
O(log n)
```

rather than a long linear linked-chain search.

Therefore interview wording should be:

```text
Expected:

    O(1)

Heavy collision in linked structure:

    potentially O(n)

Treeified bin:

    approximately O(log n)
```

### Interview Tip

Do not say:

> "HashMap is always O(1)."

Say:

> **"HashMap provides expected O(1) average-time performance for basic operations under good hash distribution; collision-heavy bins can have worse behavior, with tree bins providing approximately O(log n) lookup under suitable conditions."**

---

# 🧵 29. HashMap and Threads

`HashMap` is **not thread-safe**.

Suppose:

```text
Thread A
   ↓
map.put()

Thread B
   ↓
map.put()
```

occur concurrently without appropriate synchronization.

Concurrent modifications can result in unsafe behavior.

For concurrent use cases, consider:

```java
Map<String, Integer> map =
        new ConcurrentHashMap<>();
```

or an appropriate synchronization strategy.

### Important

Do not confuse:

```text
HashMap
```

with:

```text
ConcurrentHashMap
```

They have different concurrency characteristics.

---

# 💾 30. Memory Perspective

Suppose:

```java
Map<String, Integer> map = new HashMap<>();

map.put("Java", 90);
```

Conceptually:

```text
Stack
  │
  │ map reference
  ▼
Heap
  │
  ▼
HashMap object
  │
  ▼
table[]
  │
  ▼
Node
 ├── hash
 ├── key reference
 ├── value reference
 └── next reference
```

### Important

The exact memory layout depends on:

- JVM implementation
- JVM architecture
- Object headers
- Compressed references
- Alignment
- JDK implementation
- Garbage collector

Therefore, this is a conceptual memory model rather than an exact byte-level layout.

---

# 🧪 31. Custom Key Example

Consider:

```java
class Student {

    int id;

    Student(int id) {
        this.id = id;
    }

    @Override
    public int hashCode() {
        return Integer.hashCode(id);
    }

    @Override
    public boolean equals(Object obj) {

        if (this == obj) {
            return true;
        }

        if (!(obj instanceof Student)) {
            return false;
        }

        Student other = (Student) obj;

        return this.id == other.id;
    }
}
```

Now:

```java
Map<Student, String> map = new HashMap<>();

Student s1 = new Student(101);
Student s2 = new Student(101);

map.put(s1, "Yash");

System.out.println(map.get(s2));
```

Result:

```text
Yash
```

### Why?

We have:

```text
s1.id == s2.id
       ↓
equals() == true
```

and:

```text
s1.hashCode() == s2.hashCode()
```

Therefore HashMap can locate the same logical key.

### Key Contract

For objects used as HashMap keys:

```text
If a.equals(b) == true

then:

a.hashCode() == b.hashCode()
```

must also be true.

---

# 🪤 32. Internal Working Interview Traps

## ❌ Trap 1

> **"HashMap directly uses `hashCode()` as the array index."**

Incorrect.

Conceptually:

```text
hashCode()
   ↓
HashMap hash/spreading
   ↓
bucket index
```

---

## ❌ Trap 2

> **"Same hash code means same key."**

Incorrect.

Two unequal objects can have the same hash code.

Therefore:

```text
same hash
   ≠
same key
```

---

## ❌ Trap 3

> **"Different hash codes always mean objects cannot be equal."**

For a correctly implemented `equals()` / `hashCode()` contract:

```text
equals() == true
       ⇒
hashCode() must be same
```

Therefore:

```text
different hash codes
       ⇒
objects cannot be equal
```

under a correctly implemented contract.

---

## ❌ Trap 4

> **"HashMap always uses LinkedList for collisions."**

Incomplete/outdated.

Modern Java HashMap can use tree bins under suitable conditions.

---

## ❌ Trap 5

> **"At exactly 8 nodes, HashMap always treeifies."**

Incorrect.

Treeification also depends on table capacity and implementation conditions.

---

## ❌ Trap 6

> **"Load factor means percentage of used buckets."**

Not exactly.

Load factor is used with capacity to determine the resize threshold.

---

## ❌ Trap 7

> **"Initial capacity means exactly that many buckets are immediately allocated."**

Not necessarily.

Modern HashMap uses lazy table initialization.

---

## ❌ Trap 8

> **"HashMap is always O(1)."**

Incorrect.

Better:

```text
Expected → O(1)
```

under normal hash distribution.

Collision-heavy cases can be worse.

---

## ❌ Trap 9

> **"Two keys must have different hash codes to occupy different entries."**

Incorrect.

Two different keys may have the same hash code.

They can still coexist because `equals()` distinguishes them.

---

## ❌ Trap 10

> **"Changing a key after insertion is harmless."**

Dangerous.

If a mutable key's fields involved in `hashCode()` or `equals()` are changed after insertion, the entry may no longer be found using the mutated key.

Example:

```java
Map<Student, String> map = new HashMap<>();

Student student = new Student(101);

map.put(student, "Yash");

// If fields participating in hashCode()
// and equals() are changed here,
// lookup behavior can become problematic.

System.out.println(map.get(student));
```

This is why keys used in HashMap should generally be immutable with respect to the fields involved in equality and hashing.

---

# 🎯 33. DSA Connection

Understanding HashMap internals helps explain why common DSA patterns are fast.

---

## Two Sum

Typical idea:

```text
value
  ↓
HashMap
  ↓
lookup complement
  ↓
O(1) expected
```

Overall:

```text
O(n)
```

expected.

---

## Frequency Counting

Typical idea:

```text
number
   ↓
frequency lookup
   ↓
increment
   ↓
store
```

Expected overall complexity:

```text
O(n)
```

---

## Value → Index

When a problem asks:

> "Where did I see this value?"

Think:

```text
value → index
```

using a HashMap.

---

## Prefix Sum

Typical idea:

```text
prefixSum
    ↓
HashMap
    ↓
previous prefix/count/index
    ↓
O(1) expected lookup
```

This is the foundation of many:

```text
subarray
prefix sum
counting
duplicate
pair
```

problems.

---

# 🧠 DSA Pattern Memory

When you see:

> **"Have I seen this before?"**

Think:

```text
HashMap / HashSet
```

When you see:

> **"How many times?"**

Think:

```text
Frequency Map
```

When you see:

> **"Where did I see it?"**

Think:

```text
Value → Index
```

When you see:

> **"Have I seen this prefix sum?"**

Think:

```text
Prefix Sum + HashMap
```

---

# 🎤 34. 30-Second Interview Answer

> **Internally, HashMap maintains a table of buckets. When a key is inserted, HashMap obtains its hash code, applies internal hash spreading, and calculates a bucket index based on the table capacity. If the bucket is empty, a node is inserted. If it already contains entries, HashMap compares the hash and keys using `equals()` to determine whether the key already exists or whether a collision has occurred. Collisions can initially be represented through linked nodes, and heavily collided bins can be treeified under suitable conditions. As the map grows beyond its threshold, the table is resized.**

---

# ⚡ 35. Quick Revision

```text
put(key, value)
      │
      ▼
  hashCode()
      │
      ▼
    hash()
      │
      ▼
 bucket index
      │
      ▼
 table[index]
      │
   ┌──┴──┐
   ▼     ▼
 empty  occupied
   │       │
   ▼       ▼
 Node    compare
           │
      ┌────┴────┐
      ▼         ▼
   same key   collision
      │         │
      ▼         ▼
 replace      chain/tree
 value
```

### Core Flow

```text
KEY
 ↓
hashCode()
 ↓
HashMap hash()
 ↓
bucket index
 ↓
table[index]
 ↓
compare candidates
 ↓
equals()
 ↓
VALUE
```

---

# 📋 36. Internal Working Cheat Sheet

| Concept | Meaning |
|---|---|
| `HashMap` | Hash-based `Map` implementation |
| Table | Internal bucket array |
| Bucket | One position in the table |
| Node | Stores mapping information |
| `hashCode()` | Key's hash code |
| `hash()` | HashMap's internal hash-spreading operation |
| Index | Bucket selected for an entry |
| Collision | Different keys reach the same bucket |
| `equals()` | Determines logical key equality |
| Capacity | Number of positions in the internal table |
| Load Factor | Controls when the table should grow |
| Threshold | Resize trigger derived from capacity and load factor |
| Resize | Increase table capacity |
| Treeification | Convert a heavily collided bin to a tree structure |
| Untreeification | Convert a tree bin back to a simpler representation |
| Default Load Factor | `0.75` |
| Common default capacity | `16` when the default table is initialized |
| Treeify Threshold | `8`, subject to implementation conditions |
| Minimum Treeify Capacity | `64` in common OpenJDK implementations |
| Untreeify Threshold | `6` in common OpenJDK implementations |
| Expected `get()` | O(1) |
| Expected `put()` | O(1) |
| Tree-bin lookup | Approximately O(log n) |
| Collision-heavy linked lookup | Potentially O(n) |

---

# 🏁 37. Final Mental Model

## 🔥 Remember HashMap Like This

```text
┌──────────────────────────────────────┐
│              HashMap                 │
└───────────────────┬──────────────────┘
                    │
                 key,value
                    │
                    ▼
               hashCode()
                    │
                    ▼
               hash spread
                    │
                    ▼
             (n - 1) & hash
                    │
                    ▼
                  INDEX
                    │
                    ▼
              table[index]
                    │
          ┌─────────┴─────────┐
          │                   │
        Empty              Occupied
          │                   │
          ▼                   ▼
      New Node           hash + equals()
                              │
                    ┌─────────┴─────────┐
                    │                   │
                 Same Key          Different Key
                    │                   │
                    ▼                   ▼
              Replace Value        Collision
                                        │
                              ┌─────────┴─────────┐
                              │                   │
                           Nodes             Tree Nodes
                              │                   │
                              └─────────┬─────────┘
                                        │
                                        ▼
                                 Resize when
                              threshold reached
```

---

# 🧠 Golden Memory Trick

```text
H → B → C → E

H = Hash
B = Bucket
C = Collision
E = Equals
```

For a deeper version:

```text
KEY
 ↓
hashCode()
 ↓
hash spread
 ↓
bucket index
 ↓
bucket
 ↓
hash comparison
 ↓
equals()
 ↓
VALUE
```

And for growth:

```text
Capacity
    ↓
Load Factor
    ↓
Threshold
    ↓
Resize
    ↓
Redistribute
```

---

# 🚀 What You Must Be Able to Explain in an Interview

Before moving to `04-HashCode-and-Equals.md`, make sure you can explain these without notes:

1. What is a bucket?
2. How does HashMap calculate a bucket index?
3. Why does HashMap use power-of-two capacities?
4. What is a collision?
5. How are collisions handled?
6. What is treeification?
7. Why is the treeification threshold not simply "8 = tree"?
8. What is capacity?
9. What is load factor?
10. What is threshold?
11. Why does HashMap resize?
12. Why are `hashCode()` and `equals()` both important?
13. Why is HashMap expected O(1)?
14. Why can HashMap become O(n) in collision-heavy linked bins?
15. What happens internally during `get()`?
16. What happens internally during `put()`?
17. What happens internally during `remove()`?
18. Why are mutable keys dangerous?

---

> **🔥 Core Takeaway**
>
> `HashMap` is essentially:
>
> **`hash → bucket → compare → value`**
>
> The performance comes from quickly locating a bucket using hashing. `equals()` then identifies the correct logical key within that bucket. Collisions are handled through node structures, heavily collided bins can be treeified, and the table grows through resizing when the load threshold is reached.