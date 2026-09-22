# ⚙️ 03 — HashMap Internal Working

> **Java Collections Deep Dive → Map Framework**
>
> This note goes beneath the HashMap API and explains what actually happens inside HashMap when you call `put()`, `get()`, `remove()`, and other operations.
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
- [🧠 22. Why Power of Two Capacity?](#-22-why-power-of-two-capacity)
- [🎯 23. Why (n - 1) & hash?](#-23-why-n---1--hash)
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

    map.put("Java", 90);

is easy.

An interviewer may ask:

> "What happens internally when you call put()?"

Now you need to understand:

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

This is one of the most important Java Collections internals.

---

# 🏗️ 2. High-Level Architecture

Conceptually:

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

Each bucket can contain:

    null

or:

    Node

or:

    multiple Nodes

or, after treeification:

    TreeNode structure

---

# 📦 3. Internal Data Structure

In modern Java implementations, HashMap internally maintains a table of nodes.

Conceptually:

    Node<K,V>[] table;

A node conceptually contains:

    static class Node<K,V>
        implements Map.Entry<K,V> {

        final int hash;
        final K key;
        V value;
        Node<K,V> next;
    }

The exact implementation can vary by JDK version, so focus on the behavior rather than memorizing source-code details.

---

# 🔢 4. What is a Bucket?

A bucket is a position in HashMap's internal table.

Suppose:

    table length = 16

Then there are conceptually:

    0
    1
    2
    ...
    15

bucket positions.

When a key arrives, HashMap calculates which bucket should contain the mapping.

Example:

    "Java"
       ↓
    hash
       ↓
    bucket 7

So the mapping goes into bucket 7.

---

# 🧮 5. Hashing

Hashing converts information about a key into a hash value.

For an object:

    key.hashCode()

returns an integer.

Example:

    String key = "Java";

    int h = key.hashCode();

The hash code is then processed by HashMap before the bucket index is calculated.

---

# ⚙️ 6. HashMap hash() Method

Conceptually, modern Java HashMap uses a hash-spreading operation similar to:

    static final int hash(Object key) {

        int h;

        return (key == null)
            ? 0
            : (h = key.hashCode())
                ^ (h >>> 16);
    }

The important operation is:

    h ^ (h >>> 16)

where:

    ^  = bitwise XOR
    >>> = unsigned right shift

---

# 🧠 Why Spread the Hash?

The goal is to improve how hash-code bits influence bucket selection.

The original hash code contains 32 bits.

When the table index is calculated using a power-of-two table length, the lower bits have strong influence on the bucket.

HashMap mixes higher bits into lower bits using:

    h ^ (h >>> 16)

This can help distribute keys more effectively.

---

# 📍 7. Calculating Bucket Index

For a table length `n`, the bucket index is conceptually calculated as:

    (n - 1) & hash

Example:

    n = 16

Then:

    n - 1 = 15

Binary:

    15 = 1111

Suppose:

    hash = 101101101010

Only the relevant lower bits influence the final index:

    1111
      &
    hash
      ↓
    bucket index

Therefore the result is between:

    0 and 15

---

# 🎯 Why This Works

If:

    n = 16

then:

    n - 1 = 15

and:

    15 = 1111₂

Therefore:

    hash & 1111₂

extracts the lower four bits.

This is efficient and is one reason HashMap uses power-of-two table capacities.

---

# ➕ 8. Internal Working of put()

Suppose:

    map.put("Java", 90);

Let's follow the process.

---

## Step 1 — Receive Key

    key = "Java"

    value = 90

---

## Step 2 — Calculate Hash

HashMap obtains:

    key.hashCode()

Then applies its internal hash-spreading logic.

Conceptually:

    hashCode()
       ↓
    hash()
       ↓
    spread hash

---

## Step 3 — Calculate Bucket

Suppose:

    table length = 16

Then:

    index =
        (16 - 1) & hash

Result:

    bucket index

---

## Step 4 — Check Bucket

HashMap checks:

    table[index]

There are two main possibilities.

### Case A — Bucket is Empty

    table[index] == null

Then a new node can be inserted.

    bucket
      ↓
    Node
      ├── hash
      ├── key = "Java"
      ├── value = 90
      └── next = null

---

### Case B — Bucket Already Contains Something

Then HashMap checks whether the existing entry corresponds to the same key.

It considers:

    hash

and:

    equals()

---

# 🔍 Same Key Case

Suppose:

    map.put("Java", 90);

Then:

    map.put("Java", 100);

The bucket is already occupied.

HashMap finds the existing key.

If the keys match:

    existing value = 90
    new value      = 100

The value is replaced.

Final:

    Java → 100

---

# 💥 Collision Case

Suppose:

    Key A → Bucket 5

and:

    Key B → Bucket 5

but:

    A.equals(B) == false

Then we have a collision.

Both mappings must coexist.

Conceptually:

    Bucket 5
       ↓
    Node A
       ↓
    Node B
       ↓
    Node C

This is where collision handling becomes important.

---

# 🔗 12. Collision Handling with Linked Nodes

Initially, collisions can be represented as a linked chain.

Conceptually:

    table[5]
       ↓
    Node A
       ↓
    Node B
       ↓
    Node C
       ↓
    null

Each node contains a reference to the next node.

When searching:

    get(key)

HashMap traverses candidate nodes and compares keys.

---

# 🧠 Important Point

HashMap does NOT say:

    "Same bucket = same key"

Instead:

    Same bucket
        ↓
    Candidate entries
        ↓
    Compare hash
        ↓
    Compare equals()
        ↓
    Correct key?

Therefore:

    Same hash
        ≠
    Same object/key

---

# 🌳 13. Treeification

If too many entries accumulate in a bucket, traversing a long linked chain can become inefficient.

Modern Java HashMap can convert a heavily collided bucket into a tree structure under specific conditions.

Conceptually:

    Before:

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

    After treeification:

             C
           /   \
          A     D
           \     \
            B     E

The actual structure is a red-black tree.

---

# 🌲 14. Red-Black Tree Concept

A red-black tree is a self-balancing binary search tree.

Its purpose is to maintain approximately:

    O(log n)

search behavior.

Therefore, under suitable tree-bin conditions, heavily collided HashMap buckets can avoid a long linear linked-list search.

---

# 📊 15. Treeification Threshold

A commonly discussed implementation constant is:

    TREEIFY_THRESHOLD = 8

Meaning a bucket may be considered for treeification when its bin becomes sufficiently large.

But there is an important condition.

HashMap also has:

    MIN_TREEIFY_CAPACITY = 64

If the table is still too small, HashMap may resize instead of immediately treeifying the bucket.

Therefore:

    bucket becomes large
           ↓
    Is table sufficiently large?
        ↙           ↘
      No             Yes
      ↓               ↓
    Resize        Treeify may occur

Do not memorize this as:

    "8 entries always means treeification."

That statement is incomplete.

---

# ↩️ 16. Untreeification

If a treeified bucket becomes sufficiently small after removals, it can be converted back into a simpler node representation.

Conceptually:

    Tree
      ↓
    Entries removed
      ↓
    Bin becomes small
      ↓
    Untreeify
      ↓
    Linked representation

A commonly referenced implementation threshold is:

    UNTREEIFY_THRESHOLD = 6

The exact behavior depends on the JDK implementation.

---

# 📈 17. Resizing

HashMap does not keep the same table size forever.

As more mappings are added, the table can become too full according to its threshold.

Then HashMap resizes.

Conceptually:

    capacity = 16

        ↓
    threshold reached

        ↓

    capacity = 32

        ↓
    threshold recalculated

        ↓

    entries redistributed

---

# 🎚️ 18. Capacity

Capacity represents the number of buckets in the internal table.

Common capacities are powers of two:

    16
    32
    64
    128
    ...

The initial capacity configured by the constructor is not necessarily the same as an immediately allocated internal table size before the table is first initialized.

---

# ⚖️ 19. Load Factor

Default load factor:

    0.75f

Conceptually:

    threshold =
        capacity × loadFactor

For:

    capacity = 16
    loadFactor = 0.75

we get:

    threshold = 12

When the relevant threshold is reached, HashMap can resize.

---

# 🚦 20. Threshold

Threshold determines approximately when resizing should occur.

Conceptually:

    threshold = capacity × loadFactor

Example:

    capacity = 16
    loadFactor = 0.75

    threshold = 12

Then the table grows when the number of mappings reaches the relevant resize condition.

---

# 🔄 21. Resize Example

Start:

    capacity = 16
    load factor = 0.75

Therefore:

    threshold = 12

Suppose entries increase:

    1
    2
    3
    ...
    12

At the resize threshold:

    HashMap expands

Conceptually:

    16 buckets
        ↓
    32 buckets

Then entries are redistributed according to the new table structure.

---

# 🧠 22. Why Power of Two Capacity?

HashMap uses power-of-two capacities because it allows efficient bucket-index calculation.

Instead of:

    hash % n

the implementation can use:

    (n - 1) & hash

when `n` is a power of two.

For example:

    n = 16

    n - 1 = 15

    15 = 1111₂

Therefore:

    hash & 1111₂

efficiently selects the relevant lower bits.

---

# 🎯 23. Why (n - 1) & hash?

Suppose:

    n = 16

Then:

    n - 1 = 15

Binary:

    0000 1111

Suppose:

    hash =
    1011 0110

Then:

    1011 0110
    0000 1111
    ----------
    0000 0110

Therefore:

    index = 6

So the key goes to:

    bucket 6

---

# 🧩 24. hashCode() vs HashMap hash()

These are NOT the same thing.

## hashCode()

Defined by:

    Object

and potentially overridden by your class.

Example:

    key.hashCode()

returns an integer.

---

## HashMap hash()

HashMap internally processes the hash code.

Conceptually:

    hashCode()
       ↓
    bit spreading
       ↓
    HashMap hash value
       ↓
    bucket index

So:

    hashCode()
        ≠
    final bucket index

---

# 🔐 25. Role of equals()

Suppose two keys land in the same bucket.

HashMap needs to determine:

> "Is this actually the same key?"

It uses equality comparison.

Conceptually:

    hash matches?
        ↓
    equals()?
        ↓
    same key?

For example:

    map.put("Java", 90);

Later:

    map.get(new String("Java"));

The two String objects can be different objects, but:

    "Java".equals(
        new String("Java")
    )

is true.

Therefore HashMap can find the existing mapping.

---

# 🧬 26. Complete put() Flow

Let's visualize:

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
       ┌─────┴─────┐
       │           │
     null       non-null
       │           │
       ▼           ▼
   Create Node   Compare entries
                   │
             ┌─────┴──────┐
             │            │
          Same key     Different key
             │            │
             ▼            ▼
       Replace value   Collision
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

---

# 🔎 27. Complete get() Flow

Suppose:

    map.get(key)

Flow:

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

---

# 🧹 28. Complete remove() Flow

Suppose:

    map.remove(key)

Flow:

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

If the bucket is treeified, the tree-specific removal logic is used.

---

# 🧠 29. Why HashMap is O(1) Expected

Suppose there are:

    1,000,000 entries

If HashMap distributes keys effectively, each lookup does not need to inspect all:

    1,000,000

entries.

Instead:

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

This is why basic operations are expected to be:

    O(1)

under normal hash distribution.

---

# ⚠️ 30. Worst-Case Complexity

If many keys collide into the same bucket, the bucket can become expensive to search.

Historically, a long linked chain could lead toward:

    O(n)

lookup behavior.

Modern Java HashMap can treeify heavily collided bins under suitable conditions, giving tree-bin lookup approximately:

    O(log n)

rather than a long linear chain.

Therefore interview wording should be:

    Average / expected:
        O(1)

    Heavy collision linked structure:
        potentially O(n)

    Treeified bin:
        approximately O(log n)

Do not claim HashMap is mathematically guaranteed to be O(1).

---

# 🧵 31. HashMap and Threads

HashMap is not designed as a thread-safe Map.

Suppose:

    Thread A
        ↓
    map.put()

and simultaneously:

    Thread B
        ↓
    map.put()

Without appropriate synchronization, concurrent modifications can cause unsafe behavior.

For concurrent use cases, consider:

    ConcurrentHashMap

or an appropriate synchronization strategy.

---

# 💾 32. Memory Perspective

Suppose:

    map.put("Java", 90);

Conceptually:

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

The actual memory layout depends on the JVM and implementation details.

---

# 🧪 33. Custom Key Example

Consider:

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

            Student other =
                    (Student) obj;

            return this.id == other.id;
        }
    }

Now:

    Map<Student, String> map =
            new HashMap<>();

    Student s1 =
            new Student(101);

    Student s2 =
            new Student(101);

    map.put(s1, "Yash");

    System.out.println(
        map.get(s2)
    );

Result:

    Yash

Why?

    s1.id == s2.id
          ↓
    equals() == true

and:

    s1.hashCode()
        ==
    s2.hashCode()

Therefore HashMap can locate the same logical key.

---

# 🪤 34. Internal Working Interview Traps

## ❌ Trap 1

> HashMap directly uses hashCode() as the array index.

Incorrect.

Conceptually:

    hashCode()
       ↓
    HashMap hash/spreading
       ↓
    bucket index

---

## ❌ Trap 2

> Same hash code means same key.

Incorrect.

Two unequal objects can have the same hash code.

---

## ❌ Trap 3

> Different hash codes always mean objects cannot be equal.

For correctly implemented `equals()` / `hashCode()`:

    equals() == true
        ⇒
    hashCode() must be same

So if hash codes differ, objects cannot satisfy equality.

---

## ❌ Trap 4

> HashMap always uses LinkedList for collisions.

Incomplete/outdated.

Modern Java HashMap can use tree bins under suitable conditions.

---

## ❌ Trap 5

> At exactly 8 nodes, HashMap always treeifies.

Incorrect.

Treeification also depends on table capacity and implementation conditions.

---

## ❌ Trap 6

> Load factor means percentage of used buckets.

Not exactly.

It is used with capacity to determine the resize threshold.

---

## ❌ Trap 7

> Initial capacity means exactly that many buckets are immediately allocated.

Not necessarily.

Table initialization is lazy in modern HashMap implementations.

---

## ❌ Trap 8

> HashMap is always O(1).

Incorrect.

The better statement is:

    Expected O(1)
    under normal conditions.

---

# 🎯 35. DSA Connection

Understanding HashMap internals helps explain why these patterns are fast.

## Two Sum

    value
      ↓
    hash
      ↓
    bucket
      ↓
    lookup
      ↓
    O(1) expected

Therefore:

    O(n)

overall.

---

## Frequency Counting

    number
       ↓
    hash
       ↓
    frequency lookup
       ↓
    update

Therefore:

    O(n)

expected overall.

---

## Prefix Sum

    prefixSum
        ↓
    HashMap
        ↓
    previous index/count
        ↓
    O(1) expected lookup

This is the foundation of many:

    subarray
    prefix sum
    counting
    duplicate
    pair

problems.

---

# 🧠 DSA Pattern Memory

When you see:

> "Have I seen this before?"

Think:

    HashMap / HashSet

When you see:

> "How many times?"

Think:

    Frequency Map

When you see:

> "Where did I see it?"

Think:

    Value → Index

When you see:

> "Have I seen this prefix sum?"

Think:

    Prefix Sum + HashMap

---

# 🎤 36. 30-Second Interview Answer

> **Internally, HashMap maintains a table of buckets. When a key is inserted, HashMap obtains its hash code, applies internal hash spreading, and calculates a bucket index using the table capacity. If the bucket is empty, a node is inserted. If it already contains entries, HashMap compares the hash and keys using equals() to determine whether the key already exists or whether a collision has occurred. Collisions can initially be represented through linked nodes, and heavily collided bins can be treeified under suitable conditions. As the map grows beyond its threshold, the table is resized.**

---

# ⚡ 37. Quick Revision

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
      replace     chain/tree
      value

---

# 📋 38. Internal Working Cheat Sheet

| Concept | Meaning |
|---|---|
| HashMap | Hash-based Map implementation |
| Table | Internal bucket array |
| Bucket | One position in table |
| Node | Stores mapping information |
| hashCode() | Key's hash code |
| hash() | HashMap's internal hash spreading |
| Index | Bucket selected for entry |
| Collision | Different keys reach same bucket |
| equals() | Confirms logical key equality |
| Capacity | Number of table positions |
| Load Factor | Controls resize threshold |
| Threshold | Approximate resize trigger |
| Resize | Increase table capacity |
| Treeification | Convert heavily collided bin to tree structure |
| Untreeification | Convert tree bin back to simpler nodes |
| Default Load Factor | 0.75 |
| Common initial capacity | 16 after initialization under common defaults |
| Treeify threshold | 8 entries, subject to capacity/implementation conditions |
| Minimum treeify capacity | 64 in common OpenJDK implementation |
| Untreeify threshold | 6 in common OpenJDK implementation |
| Expected get() | O(1) |
| Expected put() | O(1) |
| Tree-bin lookup | Approximately O(log n) |

---

# 🏁 39. Final Mental Model

## 🔥 Remember HashMap Like This

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
          New Node          hash + equals()
                                  │
                         ┌────────┴────────┐
                         │                 │
                     Same Key          Different Key
                         │                 │
                         ▼                 ▼
                   Replace Value       Collision
                                           │
                                  ┌────────┴────────┐
                                  │                 │
                                Nodes          Tree Nodes
                                  │                 │
                                  └────────┬────────┘
                                           │
                                           ▼
                                      Resize when
                                      threshold reached

---

# 🧠 Golden Memory Trick

    H → B → C → E

    H = Hash
    B = Bucket
    C = Collision
    E = Equals

For a deeper version:

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

And for growth:

    Capacity
       ↓
    Load Factor
       ↓
    Threshold
       ↓
    Resize
       ↓
    Redistribute

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
    12. Why are hashCode() and equals() both important?
    13. Why is HashMap expected O(1)?
    14. Why can HashMap become O(n) in collision-heavy linked bins?
    15. What happens internally during get()?
    16. What happens internally during put()?
    17. What happens internally during remove()?
    18. Why are mutable keys dangerous?

> **🔥 Core takeaway:**  
> `HashMap` is essentially **hash → bucket → compare → value**. The performance comes from quickly locating a bucket using hashing. `equals()` then identifies the correct logical key within that bucket. Collisions are handled through node structures, heavily collided bins can be treeified, and the table grows through resizing when the load threshold is reached.