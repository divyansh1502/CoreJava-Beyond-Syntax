````md
# ⚙️ 03 — HashMap Internal Working

> **Java Collections Deep Dive → Map Framework**
>
> This note goes beneath the HashMap API and explains what actually happens inside `HashMap` when you call `put()`, `get()`, `remove()`, and other operations.
>
> **Prerequisite:** `02-HashMap.md`
>
> **Next:** `04-HashCode-and-Equals.md`

---

# 📑 Table of Contents

- [🧠 1. Why Learn Internal Working](#-1-why-learn-internal-working)
- [🏗️ 2. High-Level Architecture](#-2-high-level-architecture)
- [📦 3. Internal Data Structure](#-3-internal-data-structure)
- [🔢 4. What is a Bucket](#-4-what-is-a-bucket)
- [🧮 5. Hashing](#-5-hashing)
- [⚙️ 6. HashMap hash() Method](#-6-hashmap-hash-method)
- [📍 7. Calculating Bucket Index](#-7-calculating-bucket-index)
- [➕ 8. Internal Working of put()](#-8-internal-working-of-put)
- [🔍 9. Internal Working of get()](#-9-internal-working-of-get)
- [🗑️ 10. Internal Working of remove()](#-10-internal-working-of-remove)
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
- [🧠 22. Why Power of Two Capacity](#-22-why-power-of-two-capacity)
- [🎯 23. Why `(n - 1) & hash`](#-23-why-n---1--hash)
- [🧩 24. hashCode() vs HashMap hash()](#-24-hashcode-vs-hashmap-hash)
- [🔐 25. Role of equals()](#-25-role-of-equals)
- [🧬 26. Complete put() Flow](#-26-complete-put-flow)
- [🔎 27. Complete get() Flow](#-27-complete-get-flow)
- [🧹 28. Complete remove() Flow](#-28-complete-remove-flow)
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

### 🎯 Core Idea

HashMap can be remembered as:

```text
HASH → BUCKET → COMPARE → VALUE
```

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

### Mental Model

```text
HashMap
   ↓
Array of buckets
   ↓
Each bucket stores entries
   ↓
Hash decides bucket
   ↓
equals() identifies exact key
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
| `hash` | Stores the processed hash |
| `key` | Stores the key reference |
| `value` | Stores the value |
| `next` | Points to the next node in the bucket |

> The exact implementation can vary by JDK version, so focus on the behavior rather than memorizing source-code details.

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
5
6
7
8
9
10
11
12
13
14
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

So the mapping may be stored in:

```text
table[7]
```

### Important

A bucket is **not a separate collection object**.

It is simply a position in the internal table that can reference an entry or a chain/tree of entries.

---

# 🧮 5. Hashing

Hashing converts information about a key into a hash value.

For an object:

```java
key.hashCode()
```

returns an integer.

Example:

```java
String key = "Java";

int h = key.hashCode();
```

The returned value is not directly used as the final array index.

HashMap processes the hash before calculating the bucket.

### Flow

```text
key
 ↓
hashCode()
 ↓
HashMap hash spreading
 ↓
bucket index
```

---

# ⚙️ 6. HashMap hash() Method

Conceptually, modern Java `HashMap` uses a hash-spreading operation similar to:

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
^  = bitwise XOR
>>> = unsigned right shift
```

### Example

Conceptually:

```text
original hash
      ↓
shift right by 16 bits
      ↓
XOR with original hash
      ↓
spread hash
```

---

# 🧠 Why Spread the Hash?

The goal is to improve how hash-code bits influence bucket selection.

The original hash code contains 32 bits.

When the table index is calculated using a power-of-two table length, the lower bits have strong influence on the bucket.

HashMap mixes higher bits into lower bits using:

```text
h ^ (h >>> 16)
```

This can help distribute keys more effectively.

### Important Distinction

```text
key.hashCode()
```

is the key's hash code.

```text
HashMap hash()
```

processes that hash code.

And:

```text
bucket index
```

is calculated from the processed hash.

Therefore:

```text
hashCode()
    ≠
HashMap hash()
    ≠
bucket index
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

Suppose the relevant lower bits of the hash are:

```text
hash = xxxx xxxx 1010
```

Then:

```text
  1010
& 1111
------
  1010
```

Therefore:

```text
bucket index = 10
```

The result is always between:

```text
0
```

and:

```text
n - 1
```

---

# 🎯 Why This Works

If:

```text
n = 16
```

then:

```text
n - 1 = 15
```

and:

```text
15 = 1111₂
```

Therefore:

```text
hash & 1111₂
```

extracts the relevant lower four bits.

This is efficient and is one reason HashMap uses power-of-two table capacities.

---

# ➕ 8. Internal Working of put()

Suppose:

```java
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

## Step 2 — Initialize Table if Required

If the internal table has not yet been created, HashMap initializes it.

Conceptually:

```text
table == null
      ↓
initialize table
```

Modern HashMap uses lazy table initialization.

---

## Step 3 — Calculate Hash

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

## Step 4 — Calculate Bucket

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

## Step 5 — Check Bucket

HashMap checks:

```text
table[index]
```

There are two main possibilities.

### Case A — Bucket is Empty

```text
table[index] == null
```

Then a new node can be inserted.

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

---

### Case B — Bucket Already Contains Something

Then HashMap checks whether the existing entry corresponds to the same key.

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

The key is not duplicated.

---

# 💥 Collision Case

Suppose:

```text
Key A → Bucket 5
Key B → Bucket 5
```

but:

```text
A.equals(B) == false
```

Then we have a collision.

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

This is where collision handling becomes important.

---

# 🔗 12. Collision Handling with Linked Nodes

Initially, collisions can be represented as a linked chain.

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

```text
get(key)
```

HashMap traverses candidate nodes and compares keys.

### Important Point

HashMap does **not** say:

```text
"Same bucket = same key"
```

Instead:

```text
Same bucket
    ↓
Candidate entries
    ↓
Compare hash
    ↓
Compare keys using equals()
    ↓
Correct key?
```

Therefore:

```text
Same bucket
    ≠
Same key
```

and:

```text
Same hash
    ≠
Same key
```

---

# 🌳 13. Treeification

If too many entries accumulate in a bucket, traversing a long linked chain can become inefficient.

Modern Java HashMap can convert a heavily collided bucket into a tree structure under specific conditions.

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

The actual tree-bin structure is based on a **Red-Black tree**.

---

# 🌲 14. Red-Black Tree Concept

A Red-Black tree is a self-balancing binary search tree.

Its purpose is to maintain approximately:

```text
O(log n)
```

search behavior.

Therefore, under suitable tree-bin conditions, heavily collided HashMap buckets can avoid a long linear linked-list search.

### Comparison

Linked structure:

```text
A → B → C → D → E
```

Potential search:

```text
O(n)
```

Tree structure:

```text
       C
      / \
     A   D
      \   \
       B   E
```

Potential search:

```text
O(log n)
```

for the tree-bin case.

---

# 📊 15. Treeification Threshold

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
       / \
     No   Yes
     ↓     ↓
  Resize  Treeify may occur
```

### ❌ Do Not Memorize

```text
"8 entries always means treeification."
```

That statement is incomplete.

### ✅ Better Interview Answer

> A bucket can become eligible for treeification around the treeification threshold, but HashMap also considers the table capacity. If the table is too small, resizing may occur instead of treeification.

---

# ↩️ 16. Untreeification

If a treeified bucket becomes sufficiently small after removals, it can be converted back into a simpler node representation.

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

The exact implementation behavior can vary by JDK version.

### Memory Trick

```text
Many collisions
      ↓
Treeify

Few entries after removal
      ↓
Untreeify
```

---

# 📈 17. Resizing

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

Because too many entries relative to the number of buckets can increase collisions.

Increasing the table capacity provides more bucket positions.

---

# 🎚️ 18. Capacity

Capacity represents the number of buckets in the internal table.

Common capacities are powers of two:

```text
16
32
64
128
256
...
```

### Important

The constructor's configured initial capacity is not necessarily the same as an immediately allocated internal table size.

Modern HashMap uses **lazy table initialization**.

For example:

```java
Map<Integer, String> map = new HashMap<>();
```

does not mean that a 16-element table must already have been allocated at construction time.

---

# ⚖️ 19. Load Factor

Default load factor:

```text
0.75f
```

Conceptually:

```text
threshold = capacity × loadFactor
```

For:

```text
capacity = 16
loadFactor = 0.75
```

we get:

```text
threshold = 16 × 0.75
          = 12
```

Therefore, approximately:

```text
12 entries
```

is the resize threshold for that table size under the default configuration.

### Why 0.75?

It represents a practical trade-off between:

```text
memory usage
```

and:

```text
collision frequency
```

A lower load factor generally means more buckets and potentially fewer collisions, but more memory usage.

A higher load factor can use memory more efficiently but may increase collisions.

---

# 🚦 20. Threshold

Threshold determines approximately when resizing should occur.

Conceptually:

```text
threshold = capacity × loadFactor
```

Example:

```text
capacity = 16
loadFactor = 0.75
threshold = 12
```

When the relevant resize condition is reached, HashMap can increase the table capacity.

### Important Distinction

```text
Capacity
   ↓
Number of table positions

Load Factor
   ↓
Controls how full the table is allowed to become

Threshold
   ↓
Resize trigger derived from capacity and load factor
```

---

# 🔄 21. Resize Example

Start:

```text
capacity = 16
load factor = 0.75
```

Therefore:

```text
threshold = 16 × 0.75
          = 12
```

Suppose entries increase:

```text
1
2
3
4
5
6
7
8
9
10
11
12
```

When the resize condition is reached:

```text
HashMap expands
```

Conceptually:

```text
16 buckets
    ↓
32 buckets
```

Then the entries are redistributed according to the new table structure.

### Important

Resizing is more expensive than a normal insertion because multiple entries may need to be repositioned.

This is one reason resizing is an important internal performance consideration.

---

# 🧠 22. Why Power of Two Capacity?

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

### Why Not Any Number?

If:

```text
n = 16
```

then:

```text
n - 1 = 15
```

which is:

```text
1111₂
```

The binary form contains consecutive lower `1` bits.

This makes the bitwise AND operation equivalent to taking the remainder for a power-of-two divisor:

```text
hash % 16
```

can be represented efficiently as:

```text
hash & 15
```

---

# 🎯 23. Why `(n - 1) & hash`?

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
& 0000 1111
------------
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

### Memory Trick

```text
capacity = 16
     ↓
n - 1 = 15
     ↓
1111
     ↓
AND with hash
     ↓
bucket index
```

---

# 🧩 24. hashCode() vs HashMap hash()

These are **not the same thing**.

## `hashCode()`

Defined by:

```text
Object
```

and potentially overridden by your class.

Example:

```java
key.hashCode();
```

returns an integer.

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
final bucket index
```

### Example

```java
String key = "Java";

int originalHash = key.hashCode();
```

HashMap can then process that hash internally before using it to locate the bucket.

---

# 🔐 25. Role of equals()

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
map.put("Java", 90);

map.get(new String("Java"));
```

The two `String` objects can be different objects, but:

```java
"Java".equals(new String("Java"))
```

returns:

```text
true
```

Therefore HashMap can find the existing mapping.

### Important

HashMap uses hashing to narrow the search.

Then equality determines the exact logical key.

```text
Hash
 ↓
Candidate bucket
 ↓
equals()
 ↓
Exact key
```

---

# 🧬 26. Complete put() Flow

Let's visualize the complete process:

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
   ┌────┴────┐
   │         │
 null     non-null
   │         │
   ▼         ▼
Create    Compare entries
 Node         │
              │
        ┌─────┴─────┐
        │           │
    Same key    Different key
        │           │
        ▼           ▼
 Replace       Collision
  value            │
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

### Simplified Interview Flow

```text
put()
 ↓
hashCode()
 ↓
hash spread
 ↓
index
 ↓
bucket
 ↓
empty?
 ├── yes → create node
 └── no  → compare hash + equals()
              ├── same key → replace value
              └── different key → collision handling
                                      ↓
                                  node/tree
                                      ↓
                                    resize
```

---

# 🔎 27. Complete get() Flow

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

`get()` does not scan the entire HashMap.

It first uses the hash to locate the relevant bucket.

Then it searches only the candidate entries in that bucket.

---

# 🧹 28. Complete remove() Flow

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

### Example

```java
Map<Integer, String> map = new HashMap<>();

map.put(1, "Java");
map.put(2, "Spring");

String removed = map.remove(1);
```

The mapping:

```text
1 → Java
```

is removed and:

```text
removed = "Java"
```

---

# 🧠 29. Why HashMap is O(1) Expected

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

This is why basic operations are expected to be:

```text
O(1)
```

under normal hash distribution.

### Important Interview Wording

Say:

> HashMap provides expected or average O(1) time for basic operations under good hash distribution.

Do **not** say:

> HashMap is always O(1).

---

# ⚠️ 30. Worst-Case Complexity

If many keys collide into the same bucket, the bucket can become expensive to search.

Historically, a long linked chain could lead toward:

```text
O(n)
```

lookup behavior.

Modern Java HashMap can treeify heavily collided bins under suitable conditions, giving tree-bin lookup approximately:

```text
O(log n)
```

rather than a long linear chain.

Therefore interview wording should be:

| Situation | Approximate Complexity |
|---|---:|
| Expected normal lookup | O(1) |
| Heavy collision with linked structure | O(n) |
| Treeified bin | O(log n) |

### Important

Do not claim:

```text
HashMap = guaranteed O(1)
```

Better:

```text
HashMap = expected O(1)
```

---

# 🧵 31. HashMap and Threads

HashMap is **not thread-safe**.

Suppose:

```text
Thread A
   ↓
map.put()
```

and simultaneously:

```text
Thread B
   ↓
map.put()
```

Without appropriate synchronization, concurrent modifications can cause unsafe behavior and race conditions.

For concurrent use cases, consider:

```text
ConcurrentHashMap
```

or:

```text
appropriate synchronization
```

### Important

HashMap itself does not automatically coordinate multiple threads modifying the same map.

---

# 💾 32. Memory Perspective

Suppose:

```java
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
- object headers
- compressed references
- JDK implementation
- garbage collector

Therefore the above is a conceptual memory model, not a byte-level memory layout.

---

# 🧪 33. Custom Key Example

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

Output:

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
s1.hashCode()
      ==
s2.hashCode()
```

Therefore HashMap can locate the same logical key.

### Internal Flow

```text
s2
 ↓
hashCode()
 ↓
same hash
 ↓
same bucket
 ↓
equals(s1)
 ↓
true
 ↓
return "Yash"
```

---

# 🪤 34. Internal Working Interview Traps

## ❌ Trap 1

> HashMap directly uses `hashCode()` as the array index.

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

> Same hash code means same key.

Incorrect.

Two unequal objects can have the same hash code.

```text
same hash
   ≠
same key
```

---

## ❌ Trap 3

> Different hash codes always mean objects cannot be equal.

For a correctly implemented `equals()` / `hashCode()` contract:

```text
equals() == true
      ⇒
hashCode() must be same
```

Therefore, if two objects have different hash codes, they cannot satisfy `equals() == true`.

---

## ❌ Trap 4

> HashMap always uses LinkedList for collisions.

Incomplete and outdated.

Modern Java HashMap can use tree bins under suitable conditions.

Better:

```text
collision
   ↓
linked nodes initially
   ↓
treeification under suitable conditions
```

---

## ❌ Trap 5

> At exactly 8 nodes, HashMap always treeifies.

Incorrect.

Treeification also depends on table capacity and implementation conditions.

The commonly discussed values are:

```text
TREEIFY_THRESHOLD = 8
MIN_TREEIFY_CAPACITY = 64
```

---

## ❌ Trap 6

> Load factor means percentage of used buckets.

Not exactly.

Load factor is used with capacity to determine the resize threshold.

Conceptually:

```text
threshold = capacity × load factor
```

---

## ❌ Trap 7

> Initial capacity means exactly that many buckets are immediately allocated.

Not necessarily.

Modern HashMap uses lazy table initialization.

---

## ❌ Trap 8

> HashMap is always O(1).

Incorrect.

Better:

```text
Expected → O(1)
```

Heavy collision behavior can be different.

---

## ❌ Trap 9

> `hashCode()` returns the bucket index.

Incorrect.

The flow is:

```text
key
 ↓
hashCode()
 ↓
HashMap hash spreading
 ↓
bucket index
```

---

## ❌ Trap 10

> If two objects have the same hash, HashMap automatically replaces the old value.

Incorrect.

Hash collision does not necessarily mean duplicate key.

HashMap still needs equality checking.

```text
same bucket
   ↓
compare candidate keys
   ↓
equals()
   ↓
same key?
```

---

# 🎯 35. DSA Connection

Understanding HashMap internals helps explain why common DSA patterns are fast.

---

## Two Sum

Suppose:

```text
target = 9
```

and:

```text
nums = [2, 7, 11, 15]
```

We can store previously seen values in a HashMap.

Conceptually:

```text
value
  ↓
hash
  ↓
bucket
  ↓
lookup
  ↓
O(1) expected
```

Therefore:

```text
n elements
×
O(1) expected lookup
=
O(n) expected
```

---

## Frequency Counting

For every number:

```text
number
  ↓
hash
  ↓
frequency lookup
  ↓
update
```

Therefore:

```text
O(n)
```

expected overall for `n` elements.

---

## Prefix Sum

Conceptually:

```text
prefixSum
    ↓
HashMap
    ↓
previous index/count
    ↓
O(1) expected lookup
```

This is the foundation of many:

- subarray problems
- prefix-sum problems
- counting problems
- duplicate problems
- pair problems

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

### DSA Decision Pattern

```text
Need fast membership?
        ↓
HashSet

Need key → value?
        ↓
HashMap

Need frequency?
        ↓
HashMap<Key, Integer>

Need index?
        ↓
HashMap<Value, Index>

Need grouping?
        ↓
HashMap<Key, List<Value>>
```

---

# 🎤 36. 30-Second Interview Answer

> **Internally, HashMap maintains a table of buckets. When a key is inserted, HashMap obtains its hash code, applies internal hash spreading, and calculates a bucket index using the table capacity. If the bucket is empty, a node is inserted. If it already contains entries, HashMap compares the hash and keys using `equals()` to determine whether the key already exists or whether a collision has occurred. Collisions can initially be represented through linked nodes, and heavily collided bins can be treeified under suitable conditions. As the map grows beyond its threshold, the table is resized.**

### Shorter Version

> **HashMap essentially works as hash → bucket → compare → value. The hash helps locate the bucket quickly, while `equals()` identifies the exact logical key. Collisions are handled using nodes and potentially tree bins, and the table resizes when its threshold is reached.**

---

# ⚡ 37. Quick Revision

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
    replace    chain/tree
      value
```

### `get()`

```text
get(key)
   ↓
hashCode()
   ↓
hash spread
   ↓
bucket index
   ↓
bucket
   ↓
hash + equals()
   ↓
value
```

### `remove()`

```text
remove(key)
   ↓
hashCode()
   ↓
hash spread
   ↓
bucket index
   ↓
bucket
   ↓
hash + equals()
   ↓
remove node
   ↓
return previous value
```

---

# 📋 38. Internal Working Cheat Sheet

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
| `equals()` | Confirms logical key equality |
| Capacity | Number of table positions |
| Load Factor | Controls resize threshold |
| Threshold | Resize trigger derived from capacity/load factor |
| Resize | Increase table capacity |
| Treeification | Convert a heavily collided bin to tree structure |
| Untreeification | Convert a tree bin back to simpler nodes |
| Default Load Factor | `0.75` |
| Common Default Initial Capacity | `16` |
| Treeify Threshold | `8` in common OpenJDK implementation |
| Minimum Treeify Capacity | `64` in common OpenJDK implementation |
| Untreeify Threshold | `6` in common OpenJDK implementation |
| Expected `get()` | O(1) |
| Expected `put()` | O(1) |
| Expected `remove()` | O(1) |
| Tree-bin lookup | Approximately O(log n) |
| Heavy linked collision lookup | Potentially O(n) |
| Thread-safe | ❌ |

---

# 🏁 39. Final Mental Model

## 🔥 Remember HashMap Like This

```text
┌──────────────────────────────────────────────┐
│                  HashMap                     │
└──────────────────────┬───────────────────────┘
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
              ┌────────┴────────┐
              │                 │
            Empty            Occupied
              │                 │
              ▼                 ▼
          New Node        hash + equals()
                                │
                       ┌────────┴────────┐
                       │                 │
                   Same Key        Different Key
                       │                 │
                       ▼                 ▼
                 Replace Value       Collision
                                         │
                              ┌──────────┴──────────┐
                              │                     │
                          Linked Nodes          Tree Nodes
                              │                     │
                              └──────────┬──────────┘
                                         │
                                         ▼
                                  Resize when
                                  threshold reached
```

---

# 🧠 Golden Memory Trick

Remember:

```text
H → B → C → E
```

Where:

```text
H = Hash
B = Bucket
C = Collision
E = Equals
```

For the deeper version:

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

# 🔥 Ultimate HashMap Flow

## `put()`

```text
key + value
    ↓
hashCode()
    ↓
HashMap hash()
    ↓
bucket index
    ↓
table[index]
    ↓
┌───────────────────────────┐
│                           │
▼                           ▼
Empty                    Occupied
│                           │
▼                           ▼
Create Node          Compare hash + equals()
                            │
                     ┌──────┴──────┐
                     │             │
                  Same key     Different key
                     │             │
                     ▼             ▼
               Replace value   Collision
                                   │
                              Node / Tree
                                   │
                                   ▼
                                Resize
```

---

# 🎯 What You Must Be Able to Explain in an Interview

Before moving to `04-HashCode-and-Equals.md`, make sure you can explain these without notes:

```text
[ ] 1. What is a bucket?

[ ] 2. How does HashMap calculate a bucket index?

[ ] 3. Why does HashMap use power-of-two capacities?

[ ] 4. What is a collision?

[ ] 5. How are collisions handled?

[ ] 6. What is treeification?

[ ] 7. Why is the treeification threshold not simply "8 = tree"?

[ ] 8. What is capacity?

[ ] 9. What is load factor?

[ ] 10. What is threshold?

[ ] 11. Why does HashMap resize?

[ ] 12. Why are hashCode() and equals() both important?

[ ] 13. Why is HashMap expected O(1)?

[ ] 14. Why can HashMap become O(n) in collision-heavy linked bins?

[ ] 15. What happens internally during get()?

[ ] 16. What happens internally during put()?

[ ] 17. What happens internally during remove()?

[ ] 18. Why are mutable keys dangerous?
```

---

# 🧠 Core Interview Concepts to Connect

You should be able to connect these concepts rather than memorize them separately:

```text
                 HashMap
                    │
          ┌─────────┴─────────┐
          │                   │
       Hashing             Storage
          │                   │
          ▼                   ▼
     hashCode()             Node
          │                   │
          ▼                   ▼
     hash spreading       key/value
          │                   │
          ▼                   ▼
     bucket index          next
          │
          ▼
       Bucket
          │
      ┌───┴───┐
      │       │
    Empty   Collision
              │
        ┌─────┴─────┐
        │           │
      Nodes       Tree
                    │
                    ▼
              Red-Black Tree
```

---

# ⚡ One-Line Revision

```text
HashMap = hash → index → bucket → hash + equals() → value
```

```text
Collision = different keys → same bucket
```

```text
Treeification = heavily collided bucket → tree structure
```

```text
Load Factor = controls how full the table can become before resizing
```

```text
Threshold = capacity × load factor
```

```text
Resize = increase table capacity and reorganize entries
```

```text
Expected HashMap lookup = O(1)
```

```text
Tree-bin lookup = approximately O(log n)
```

---

# 🚀 Final Interview Takeaway

> **🔥 HashMap is essentially `hash → bucket → compare → value`. The hash helps HashMap quickly locate a bucket, while `equals()` identifies the exact logical key among candidate entries. Different keys can collide into the same bucket, where HashMap can use linked nodes and, under suitable conditions, tree bins. As the table becomes sufficiently full, HashMap resizes according to its threshold.**

### The Complete Mental Picture

```text
                    KEY
                     │
                     ▼
                hashCode()
                     │
                     ▼
              HashMap hash()
                     │
                     ▼
              Bucket Index
                     │
                     ▼
                  TABLE
                     │
                     ▼
                 BUCKET
                     │
          ┌──────────┴──────────┐
          │                     │
        Empty                Occupied
          │                     │
          ▼                     ▼
       New Node          hash comparison
                                │
                                ▼
                            equals()
                                │
                    ┌───────────┴───────────┐
                    │                       │
                 Same Key             Different Key
                    │                       │
                    ▼                       ▼
              Update Value              Collision
                                            │
                                   ┌────────┴────────┐
                                   │                 │
                               Linked Nodes      Tree Nodes
                                   │                 │
                                   └────────┬────────┘
                                            │
                                            ▼
                                       Map grows
                                            │
                                            ▼
                                         Resize
```

> **🔥 Core takeaway:**  
> `HashMap` is **hash → bucket → compare → value**. The performance comes from quickly locating a bucket using hashing. `equals()` then identifies the correct logical key within that bucket. Collisions are handled through node structures, heavily collided bins can be treeified, and the table grows through resizing when the load threshold is reached.

---

# 🏆 Final Memory Formula

```text
KEY
 ↓
hashCode()
 ↓
HASH SPREADING
 ↓
(n - 1) & hash
 ↓
BUCKET
 ↓
hash + equals()
 ↓
NODE
 ↓
VALUE
```

For growth:

```text
CAPACITY
    ↓
LOAD FACTOR
    ↓
THRESHOLD
    ↓
RESIZE
    ↓
MORE BUCKETS
```

For collisions:

```text
DIFFERENT KEYS
      ↓
SAME BUCKET
      ↓
COLLISION
      ↓
LINKED NODES
      ↓
TREEIFICATION
      ↓
RED-BLACK TREE
```

For complexity:

```text
Good distribution
      ↓
Expected O(1)

Heavy linked collision
      ↓
Potential O(n)

Treeified bin
      ↓
Approximately O(log n)
```

---

# 🎯 Final One-Liner

> **HashMap internally uses hashing to map a key to a bucket, `equals()` to identify the exact key, nodes/tree bins to handle collisions, and resizing to maintain efficient performance as the map grows.**

````
