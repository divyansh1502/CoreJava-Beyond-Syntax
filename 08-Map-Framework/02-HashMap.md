# 🚀 02 — HashMap

> **Java Collections Deep Dive → Map Framework**
>
> `HashMap` is one of the most important classes in Java Collections and one of the most frequently asked topics in Java interviews.
>
> This note covers its API, behavior, hashing foundation, null handling, ordering, complexity, resizing overview, collision concept, DSA patterns, interview traps, and practical usage.
>
> 🔗 **Next:** `03-HashMap-Internal-Working.md` → Deep dive into buckets, hashing, collisions, treeification, resizing, load factor, and capacity.

---

# 📌 Table of Contents

- [🧠 1. What is HashMap?](#-1-what-is-hashmap)
- [🏗️ 2. HashMap Hierarchy](#-2-hashmap-hierarchy)
- [🎯 3. Why HashMap?](#-3-why-hashmap)
- [🔑 4. HashMap Structure](#-4-hashmap-structure)
- [📦 5. Creating a HashMap](#-5-creating-a-hashmap)
- [➕ 6. Adding Data with put()](#-6-adding-data-with-put)
- [🔍 7. Retrieving Data with get()](#-7-retrieving-data-with-get)
- [🛡️ 8. containsKey()](#-8-containskey)
- [📦 9. containsValue()](#-9-containsvalue)
- [🗑️ 10. remove()](#-10-remove)
- [🔄 11. Updating Values](#-11-updating-values)
- [⚡ 12. getOrDefault()](#-12-getordefault)
- [🚫 13. putIfAbsent()](#-13-putifabsent)
- [🔁 14. replace()](#-14-replace)
- [🧮 15. compute()](#-15-compute)
- [🧩 16. computeIfAbsent()](#-16-computeifabsent)
- [🧩 17. computeIfPresent()](#-17-computeifpresent)
- [🔀 18. merge()](#-18-merge)
- [📊 19. size(), isEmpty(), clear()](#-19-size-isempty-clear)
- [🔑 20. keySet()](#-20-keyset)
- [💎 21. values()](#-21-values)
- [🎯 22. entrySet()](#-22-entryset)
- [🔄 23. Iterating HashMap](#-23-iterating-hashmap)
- [🧠 24. HashMap and null](#-24-hashmap-and-null)
- [📐 25. Ordering](#-25-ordering)
- [⚡ 26. Time Complexity](#-26-time-complexity)
- [🧠 27. How HashMap Finds Data](#-27-how-hashmap-finds-data)
- [💥 28. Collision](#-28-collision)
- [📈 29. Resizing](#-29-resizing)
- [🎚️ 30. Load Factor and Capacity](#-30-load-factor-and-capacity)
- [🧱 31. HashMap Entry](#-31-hashmap-entry)
- [🔐 32. hashCode() and equals()](#-32-hashcode-and-equals)
- [🧪 33. Mutable Keys](#-33-mutable-keys)
- [🧵 34. Thread Safety](#-34-thread-safety)
- [💾 35. Memory Perspective](#-35-memory-perspective)
- [🎯 36. DSA Patterns](#-36-dsa-patterns)
- [💻 37. DSA Example — Frequency Counter](#-37-dsa-example--frequency-counter)
- [💻 38. DSA Example — Two Sum](#-38-dsa-example--two-sum)
- [💻 39. DSA Example — First Non-Repeating Character](#-39-dsa-example--first-non-repeating-character)
- [💻 40. DSA Example — Prefix Sum](#-40-dsa-example--prefix-sum)
- [🧠 41. HashMap Problem-Solving Pattern](#-41-hashmap-problem-solving-pattern)
- [⚠️ 42. Common Mistakes](#-42-common-mistakes)
- [🪤 43. Interview Traps](#-43-interview-traps)
- [🔥 44. Important Interview Questions](#-44-important-interview-questions)
- [🎤 45. 30-Second Interview Answer](#-45-30-second-interview-answer)
- [⚡ 46. Quick Revision](#-46-quick-revision)
- [📋 47. HashMap Cheat Sheet](#-47-hashmap-cheat-sheet)
- [🏁 48. Final Mental Model](#-48-final-mental-model)

---

# 🧠 1. What is HashMap?

`HashMap` is a hash-table-based implementation of the `Map` interface.

It stores data as:

    Key → Value

Example:

    101 → "Yash"
    102 → "Aman"
    103 → "Rohit"

Java declaration:

```java
public class HashMap<K,V>
    extends AbstractMap<K,V>
    implements Map<K,V>, Cloneable, Serializable
```

Package:

```java
java.util
```

---

# 🏗️ 2. HashMap Hierarchy

The important inheritance relationship is:

    Object
      ↓
    AbstractMap<K,V>
      ↓
    HashMap<K,V>
      ↓
    LinkedHashMap<K,V>

And through interfaces:

    Map<K,V>
       ↑
    HashMap<K,V>

So:

    HashMap IS-A Map

but:

    HashMap IS-NOT-A Collection

---

# 🎯 3. Why HashMap?

Suppose we need:

    Roll Number → Student Name

Using an array would require us to manage indexes manually.

With HashMap:

```java
Map<Integer, String> students =
        new HashMap<>();

students.put(101, "Yash");
```

Now:

```java
students.get(101)
```

directly gives:

    "Yash"

The main advantage is efficient lookup based on the key.

---

# 🔑 4. HashMap Structure

Conceptually:

    HashMap
       │
       ▼
    Internal Table
       │
       ├── Bucket 0
       ├── Bucket 1
       ├── Bucket 2
       ├── Bucket 3
       ├── ...
       └── Bucket n

Each stored mapping contains conceptually:

    hash
    key
    value
    next

The exact internal implementation details are covered in:

    03-HashMap-Internal-Working.md

---

# 📦 5. Creating a HashMap

## Basic

```java
HashMap<String, Integer> map =
        new HashMap<>();
```

---

## Prefer Programming to Interface

```java
Map<String, Integer> map =
        new HashMap<>();
```

This is generally preferred because the variable depends on the interface rather than a specific implementation.

---

## With Initial Capacity

```java
HashMap<String, Integer> map =
        new HashMap<>(32);
```

This creates a HashMap with an initial capacity configuration.

Important:

    Initial capacity

does not necessarily mean that 32 buckets are immediately allocated at construction time.

---

## With Capacity and Load Factor

```java
HashMap<String, Integer> map =
        new HashMap<>(32, 0.75f);
```

The second argument is the load factor.

---

## Copying Another Map

```java
Map<String, Integer> original =
        new HashMap<>();

original.put("Java", 90);

Map<String, Integer> copy =
        new HashMap<>(original);
```

---

# ➕ 6. Adding Data with put()

Syntax:

```java
V put(K key, V value)
```

Example:

```java
Map<String, Integer> marks =
        new HashMap<>();

marks.put("Java", 90);
marks.put("DSA", 85);
marks.put("DBMS", 80);
```

Map:

    Java → 90
    DSA  → 85
    DBMS → 80

---

# 🔄 6.1 What Does put() Return?

This is an important interview question.

`put()` returns:

    Previous value associated with the key

If there was no previous mapping:

    null

Example:

```java
Integer old =
        map.put("Java", 90);
```

Since Java did not exist:

    old == null

Then:

```java
Integer old2 =
        map.put("Java", 95);
```

Now:

    old2 == 90

Final:

    Java → 95

---

# 🔁 6.2 Duplicate Key

Example:

```java
map.put("Java", 90);
map.put("Java", 100);
```

There are NOT two Java entries.

Final:

    Java → 100

The second `put()` replaces the previous value.

---

# 🔍 7. Retrieving Data with get()

Syntax:

```java
V get(Object key)
```

Example:

```java
Integer marks =
        map.get("Java");
```

If:

    Java → 90

then:

    marks = 90

---

## Missing Key

```java
Integer marks =
        map.get("Spring");
```

If Spring does not exist:

    null

may be returned.

---

# ⚠️ 7.1 get() vs containsKey()

Suppose:

```java
map.put("Java", null);
```

Now:

```java
map.get("Java")
```

returns:

    null

But the key actually exists.

Therefore:

    map.get(key) == null

does NOT always mean:

    key does not exist

If you specifically need to check key existence:

```java
map.containsKey(key)
```

---

# 🛡️ 8. containsKey()

Checks whether the specified key exists.

Example:

```java
if (map.containsKey("Java")) {
    System.out.println("Found");
}
```

Return type:

```java
boolean
```

---

## Complexity

Expected:

    O(1)

for a normal HashMap lookup.

---

# 📦 9. containsValue()

Checks whether at least one mapping contains the specified value.

Example:

```java
if (map.containsValue(90)) {
    System.out.println("Found");
}
```

Unlike key lookup, this generally requires scanning entries.

Typical complexity:

    O(n)

---

# 🗑️ 10. remove()

Removes the mapping associated with a key.

Example:

```java
map.remove("Java");
```

If:

    Java → 90

exists, the mapping is removed.

---

## Return Value

`remove(key)` returns the previous value.

Example:

```java
Integer removed =
        map.remove("Java");
```

If Java mapped to 90:

    removed == 90

If the mapping does not exist:

    null

---

## Conditional remove

You can also specify key and expected value:

```java
map.remove("Java", 90);
```

This removes the mapping only when:

    key exists
    AND
    current value equals 90

---

# 🔄 11. Updating Values

The simplest update is:

```java
map.put("Java", 95);
```

If Java already exists:

    old value → replaced

---

## Example

```java
map.put("Java", 90);

map.put("Java", 95);
```

Final:

    Java → 95

---

# ⚡ 12. getOrDefault()

Syntax:

```java
map.getOrDefault(key, defaultValue)
```

Example:

```java
int marks =
        map.getOrDefault(
            "Spring",
            0
        );
```

If Spring exists:

    return its value

Otherwise:

    return 0

Important:

`getOrDefault()` does not automatically insert the default value into the Map.

---

## DSA Usage

This method is extremely useful for frequency counting.

```java
freq.put(
    num,
    freq.getOrDefault(num, 0) + 1
);
```

---

# 🚫 13. putIfAbsent()

Adds a mapping only if the key does not already have a mapping.

Example:

```java
map.put("Java", 90);

map.putIfAbsent("Java", 100);
```

Final:

    Java → 90

because Java already existed.

---

## New Key

```java
map.putIfAbsent("Spring", 80);
```

Now:

    Spring → 80

---

## Mental Model

    put()
        ↓
    Always update/insert

    putIfAbsent()
        ↓
    Insert only when absent

---

# 🔁 14. replace()

Replaces the value associated with an existing key.

Example:

```java
map.put("Java", 90);

map.replace("Java", 95);
```

Final:

    Java → 95

If the key does not exist:

    replace()

does not create the new mapping.

---

## Conditional replace

```java
map.replace(
    "Java",
    90,
    95
);
```

This means:

    If Java currently has 90,
    replace it with 95.

---

# 🧮 15. compute()

`compute()` recalculates a value for a key.

Example:

```java
map.compute(
    "Java",
    (key, value) ->
        value == null
            ? 1
            : value + 1
);
```

The function receives:

    key
    current value

---

## Example

Initial:

    Java → 90

Operation:

```java
map.compute(
    "Java",
    (key, value) -> value + 10
);
```

Result:

    Java → 100

---

# 🧩 16. computeIfAbsent()

Computes a value only when the key has no mapping.

Example:

```java
map.computeIfAbsent(
    "Java",
    key -> 90
);
```

If Java does not exist:

    Java → 90

If Java already exists:

    Existing value remains.

---

## DSA Example

Suppose we need:

    character → list of positions

We can write:

```java
map.computeIfAbsent(
    'a',
    key -> new ArrayList<>()
).add(0);
```

This avoids manually checking whether the key exists.

---

# 🧩 17. computeIfPresent()

Computes a new value only when the key is already mapped.

Example:

```java
map.put("Java", 90);

map.computeIfPresent(
    "Java",
    (key, value) -> value + 10
);
```

Result:

    Java → 100

If Java does not exist, nothing is computed.

---

# 🔀 18. merge()

`merge()` is extremely useful in DSA.

Syntax:

```java
map.merge(
    key,
    value,
    remappingFunction
);
```

Example:

```java
map.merge(
    "Java",
    1,
    Integer::sum
);
```

If Java does not exist:

    Java → 1

If Java already contains 2:

    Java → 3

---

## Frequency Counting

Instead of:

```java
map.put(
    ch,
    map.getOrDefault(ch, 0) + 1
);
```

we can use:

```java
map.merge(
    ch,
    1,
    Integer::sum
);
```

Both approaches are useful.

---

# 📊 19. size(), isEmpty(), clear()

## size()

Returns number of mappings.

```java
int size = map.size();
```

---

## isEmpty()

Checks whether there are zero mappings.

```java
if (map.isEmpty()) {
    System.out.println("Empty");
}
```

---

## clear()

Removes all mappings.

```java
map.clear();
```

After:

    map.size() == 0

---

# 🔑 20. keySet()

Returns a Set view of all keys.

Example:

```java
Set<String> keys =
        map.keySet();
```

If Map contains:

    Java → 90
    DSA  → 85

then:

    keys = [Java, DSA]

Important:

The returned Set is a view backed by the Map.

---

## Iteration

```java
for (String key : map.keySet()) {

    System.out.println(key);
}
```

---

# 💎 21. values()

Returns a Collection view of all values.

Example:

```java
Collection<Integer> values =
        map.values();
```

Important:

Values do not have to be unique.

Example:

    Java   → 90
    DSA    → 90
    DBMS   → 80

values:

    [90, 90, 80]

---

# 🎯 22. entrySet()

Returns all mappings as a Set of `Map.Entry`.

Example:

```java
Set<Map.Entry<String, Integer>>
        entries = map.entrySet();
```

Each Entry contains:

    key
    value

---

## Most Efficient Natural Iteration

When both key and value are required:

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

---

# 🔄 23. Iterating HashMap

## Approach 1 — entrySet()

```java
for (Map.Entry<String, Integer> entry
        : map.entrySet()) {

    String key = entry.getKey();

    Integer value = entry.getValue();
}
```

Recommended when both key and value are needed.

---

## Approach 2 — keySet()

```java
for (String key : map.keySet()) {

    Integer value = map.get(key);
}
```

Useful when you primarily need keys.

---

## Approach 3 — forEach()

```java
map.forEach(
    (key, value) ->
        System.out.println(
            key + " = " + value
        )
);
```

---

# 🧠 24. HashMap and null

`HashMap` allows:

    One null key

and:

    Multiple null values

Example:

```java
Map<String, Integer> map =
        new HashMap<>();

map.put(null, 100);

map.put("Java", null);
map.put("DSA", null);
```

Valid.

---

## Why Only One null Key?

Because keys are unique.

Therefore:

```java
map.put(null, 100);
map.put(null, 200);
```

results in:

    null → 200

not two null-key mappings.

---

# 📐 25. Ordering

A HashMap does not guarantee a predictable iteration order.

Example:

```java
map.put("C", 3);
map.put("A", 1);
map.put("B", 2);
```

You must NOT write code that assumes iteration will always be:

    C
    A
    B

or:

    A
    B
    C

The order is not part of HashMap's contract.

---

## Need Insertion Order?

Use:

```java
LinkedHashMap
```

---

## Need Sorted Key Order?

Use:

```java
TreeMap
```

---

# ⚡ 26. Time Complexity

| Operation | Average / Expected | Worst-Case Discussion |
|---|---:|---:|
| put() | O(1) | O(log n) for tree bins in modern implementations under collision-heavy conditions |
| get() | O(1) | O(log n) for tree bins in modern implementations |
| remove() | O(1) | O(log n) for tree bins in modern implementations |
| containsKey() | O(1) | Similar lookup behavior |
| containsValue() | O(n) | O(n) |
| size() | O(1) | O(1) |
| isEmpty() | O(1) | O(1) |
| clear() | O(n) | O(n) |

The common interview answer for normal HashMap lookup is:

    O(1) average

Do not simply say:

    HashMap is always O(1)

because that ignores collisions and implementation details.

---

# 🧠 27. How HashMap Finds Data

Suppose:

```java
map.put("Java", 90);
```

Conceptually:

    "Java"
       ↓
    hashCode()
       ↓
    hash transformation
       ↓
    bucket index
       ↓
    bucket
       ↓
    compare hash
       ↓
    compare key using equals()
       ↓
    store / retrieve value

For:

```java
map.get("Java")
```

the process is conceptually similar:

    "Java"
       ↓
    hashCode()
       ↓
    bucket index
       ↓
    locate candidate
       ↓
    equals()
       ↓
    return value

The exact implementation details are covered in:

    03-HashMap-Internal-Working.md

---

# 💥 28. Collision

A collision occurs when multiple keys map to the same bucket.

Conceptually:

    Key A
      ↓
    Bucket 5

    Key B
      ↓
    Bucket 5

Both keys need to coexist.

HashMap therefore needs a collision-handling mechanism.

Modern Java HashMap can use:

    Linked nodes

and, under certain conditions:

    Tree nodes

for heavily collided buckets.

---

# 📈 29. Resizing

HashMap has a table capacity and a threshold related to its load factor.

As entries increase, HashMap may resize its internal table.

Conceptually:

    Small Table
         ↓
    Entries increase
         ↓
    Threshold reached
         ↓
    Resize
         ↓
    Larger Table
         ↓
    Entries redistributed

This is important because the bucket locations depend on the table capacity.

---

# 🎚️ 30. Load Factor and Capacity

Two important HashMap concepts:

    Capacity
    Load Factor

---

## Capacity

Capacity refers to the number of buckets in the internal table.

---

## Load Factor

Load factor controls how full the table can become before resizing is triggered.

The default load factor is:

    0.75f

Conceptually:

    threshold ≈ capacity × loadFactor

For example:

    capacity = 16
    load factor = 0.75

then:

    threshold = 16 × 0.75
              = 12

This is a conceptual explanation of the resize threshold.

---

## Why 0.75?

It represents a practical balance between:

    memory usage

and:

    hash-table performance

A lower load factor can reduce collisions but may use more memory.

A higher load factor can reduce memory overhead but may increase collision pressure.

---

# 🧱 31. HashMap Entry

A HashMap mapping is represented internally by a node-like structure.

Conceptually:

    Node<K,V>

contains information similar to:

    hash
    key
    value
    next

Think:

    ┌──────────────────────────┐
    │          Node            │
    ├──────────────────────────┤
    │ hash                     │
    │ key                      │
    │ value                    │
    │ next ────────────────┐   │
    └──────────────────────│───┘
                           │
                           ▼
                         Node
                           │
                           ▼
                         Node

When treeified, the bucket can use tree-based nodes.

The exact implementation should be learned from the JDK version being used.

---

# 🔐 32. hashCode() and equals()

HashMap heavily depends on the relationship between:

    hashCode()
    equals()

For a key:

```java
hashCode()
```

helps locate the appropriate bucket.

Then:

```java
equals()
```

helps determine whether the candidate key is actually equal to the requested key.

---

## Important Contract

If:

```java
a.equals(b) == true
```

then:

```java
a.hashCode() == b.hashCode()
```

must also be true.

The reverse is NOT required.

Two unequal objects can have the same hash code.

That situation is a:

    Collision

---

## Example

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

        Student other =
                (Student) obj;

        return this.id == other.id;
    }
}
```

Then:

```java
Student s1 = new Student(101);
Student s2 = new Student(101);
```

If correctly implemented:

```java
s1.equals(s2)
    → true
```

and:

```java
s1.hashCode() == s2.hashCode()
    → true
```

---

# 🧪 33. Mutable Keys

Using mutable objects as HashMap keys can cause problems.

Example idea:

```java
Student student =
        new Student(101);

map.put(student, "Yash");
```

If the fields used by:

    hashCode()
    equals()

are later changed, the object may no longer be found in the expected bucket.

Conceptually:

    Insert key
        ↓
    hashCode = X
        ↓
    Bucket X

Then mutate key:

    hashCode = Y

Now:

```java
get(key)
```

may fail because lookup may search a different bucket.

---

## Best Practice

Prefer immutable keys such as:

```java
String
Integer
Long
Enum
```

or carefully designed immutable custom classes.

---

# 🧵 34. Thread Safety

`HashMap` is NOT thread-safe for concurrent structural modification.

Do not assume:

```java
HashMap
```

automatically synchronizes access.

If multiple threads need concurrent Map access, consider:

```java
ConcurrentHashMap
```

depending on the requirements.

---

## Alternatives

```java
Collections.synchronizedMap(
    new HashMap<>()
)
```

or:

```java
ConcurrentHashMap
```

These are not identical solutions.

`ConcurrentHashMap` is specifically designed for concurrent access patterns.

---

# 💾 35. Memory Perspective

Suppose:

```java
Map<Integer, String> map =
        new HashMap<>();

map.put(101, "Yash");
```

Conceptually memory involves:

    HashMap object
         ↓
    internal table
         ↓
    bucket
         ↓
    node
       ├── hash
       ├── reference to key
       ├── reference to value
       └── reference to next node

The actual memory consumption depends on:

    JVM
    architecture
    object headers
    references
    compressed ordinary object pointers
    table capacity
    number of entries
    node structure

Therefore, do not memorize a fixed memory size for a HashMap node.

---

# 🎯 36. DSA Patterns

HashMap is one of the most important DSA tools.

Learn these patterns deeply:

    ┌──────────────────────────────┐
    │       HASHMAP PATTERNS       │
    ├──────────────────────────────┤
    │ 1. Frequency Counting       │
    │ 2. Fast Lookup              │
    │ 3. Complement Search        │
    │ 4. Duplicate Detection     │
    │ 5. Index Tracking           │
    │ 6. Prefix Sum               │
    │ 7. Grouping                 │
    │ 8. Sliding Window           │
    │ 9. Pair Counting            │
    │ 10. State Tracking          │
    └──────────────────────────────┘

---

# 💻 37. DSA Example — Frequency Counter

Problem:

    Count frequency of each number.

Input:

    [1, 2, 2, 3, 1, 2]

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

Result:

    1 → 2
    2 → 3
    3 → 1

Complexity:

    Time:  O(n) average
    Space: O(k)

where:

    k = number of distinct elements

---

# 💻 38. DSA Example — Two Sum

Problem:

    nums = [2, 7, 11, 15]
    target = 9

We need:

    2 + 7 = 9

---

## Thought Process

For each current value:

    required = target - current

Then ask:

    "Have I already seen required?"

If yes:

    answer found.

---

## Code

```java
public int[] twoSum(
        int[] nums,
        int target) {

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

    return new int[] {};
}
```

---

## What Does the Map Store?

    value → index

Example after processing 2:

    2 → 0

When current value is 7:

    required = 9 - 7
             = 2

Map contains:

    2 → 0

Therefore:

    answer = [0, 1]

---

## Complexity

    Time:  O(n) average
    Space: O(n)

Compared with brute force:

    Time: O(n²)

---

# 💻 39. DSA Example — First Non-Repeating Character

Problem:

    s = "leetcode"

Find the first character appearing exactly once.

---

## Step 1 — Count

```java
Map<Character, Integer> freq =
        new HashMap<>();

for (char ch : s.toCharArray()) {

    freq.put(
        ch,
        freq.getOrDefault(ch, 0) + 1
    );
}
```

---

## Step 2 — Scan Again

```java
for (int i = 0;
     i < s.length();
     i++) {

    char ch = s.charAt(i);

    if (freq.get(ch) == 1) {
        return i;
    }
}

return -1;
```

---

## Pattern

    String
       ↓
    Frequency Map
       ↓
    Second traversal
       ↓
    Answer

Complexity:

    Time:  O(n)
    Space: O(k)

---

# 💻 40. DSA Example — Prefix Sum

A common advanced HashMap pattern is:

    Prefix Sum + HashMap

Example problem:

Find whether an array contains a subarray whose sum is zero.

Core idea:

If the same prefix sum appears twice:

    prefix[i] == prefix[j]

then:

    sum of subarray (i+1 ... j) = 0

---

## Code

```java
public boolean hasZeroSumSubarray(
        int[] nums) {

    Set<Integer> seen =
            new HashSet<>();

    int prefixSum = 0;

    seen.add(0);

    for (int num : nums) {

        prefixSum += num;

        if (seen.contains(prefixSum)) {
            return true;
        }

        seen.add(prefixSum);
    }

    return false;
}
```

This particular implementation uses `HashSet`, but the broader prefix-sum pattern can also use `HashMap` when index/count information is required.

---

# 🧠 41. HashMap Problem-Solving Pattern

When reading a DSA problem, ask:

    ┌─────────────────────────────┐
    │ Do I need FAST LOOKUP?      │
    └──────────────┬──────────────┘
                   │
                  YES
                   ↓
             Consider HashMap
                   │
                   ↓
          What should be the key?
                   │
        ┌──────────┼───────────┐
        ▼          ▼           ▼
      Value      Prefix      Character
        ↓         Sum           ↓
      Index       ↓           Count
                  Index
                   │
                   ▼
              Solve in O(n)
              average time

---

# ⚠️ 42. Common Mistakes

## ❌ Mistake 1 — Assuming HashMap Is Ordered

Wrong:

    "HashMap stores insertion order."

Correct:

    HashMap does not guarantee predictable iteration order.

---

## ❌ Mistake 2 — Thinking Duplicate Keys Are Stored

Wrong:

```java
map.put("A", 1);
map.put("A", 2);
```

creates two A entries.

Correct:

    A → 2

---

## ❌ Mistake 3 — Using containsValue() for Fast Lookup

`containsValue()` generally requires scanning values.

If you need fast key lookup:

```java
containsKey()
```

---

## ❌ Mistake 4 — Forgetting hashCode()

For custom keys, a correct `equals()` / `hashCode()` contract is critical.

---

## ❌ Mistake 5 — Mutating HashMap Keys

Avoid changing fields that participate in:

    equals()
    hashCode()

after the key has been inserted.

---

## ❌ Mistake 6 — Saying HashMap Is Always O(1)

Better interview wording:

    Expected / average O(1) for basic hash operations under normal conditions.

---

## ❌ Mistake 7 — Confusing get() and containsKey()

If null values are allowed:

    map.get(key) == null

does not prove that the key is absent.

---

# 🪤 43. Interview Traps

### Q1. Can HashMap have null keys?

Yes.

HashMap permits one null key.

---

### Q2. Can HashMap have multiple null values?

Yes.

---

### Q3. What happens when the same key is inserted twice?

The new value replaces the old value.

---

### Q4. What does put() return?

The previous value associated with the key, or null if there was no previous mapping.

---

### Q5. Is HashMap thread-safe?

No.

---

### Q6. Does HashMap preserve insertion order?

No guarantee.

---

### Q7. What is the average complexity of get()?

Expected:

    O(1)

---

### Q8. What is the complexity of containsValue()?

Typically:

    O(n)

---

### Q9. Why are hashCode() and equals() important?

They help HashMap locate and identify keys correctly.

---

### Q10. Can two unequal objects have the same hash code?

Yes.

That is a collision.

---

### Q11. If two objects have the same hash code, are they equal?

No.

Same hash code does not imply equality.

---

### Q12. If equals() returns true, what must be true?

Their hash codes must be equal.

---

### Q13. Why is HashMap faster than scanning a list for lookup?

Hashing allows expected constant-time key-based lookup instead of linear search.

---

### Q14. Which Map should be used for sorted keys?

```java
TreeMap
```

---

### Q15. Which Map should be used for insertion-order iteration?

```java
LinkedHashMap
```

---

# 🔥 44. Important Interview Questions

## Q1. What is HashMap?

`HashMap` is a hash-table-based implementation of the `Map` interface that stores key-value mappings.

---

## Q2. How does HashMap work at a high level?

It uses the key's hash information to determine a bucket and then uses key comparison to locate the correct entry.

---

## Q3. Why is HashMap O(1) on average?

Because hashing allows the implementation to directly identify the likely bucket instead of scanning every mapping.

---

## Q4. What happens during a collision?

Multiple keys can end up in the same bucket. HashMap handles such collisions using linked nodes and, under suitable conditions in modern Java implementations, tree-based nodes.

---

## Q5. What is load factor?

It is a threshold-related factor controlling when the internal table should resize.

Default:

    0.75

---

## Q6. What is initial capacity?

The initial bucket-table capacity configuration used by HashMap.

---

## Q7. What happens when HashMap resizes?

The internal table grows and mappings are redistributed according to the new table capacity.

---

## Q8. Why does HashMap allow null?

HashMap's contract permits one null key and multiple null values.

---

## Q9. Why can mutable keys be dangerous?

If a key's hash/equality state changes after insertion, lookup may no longer locate the entry correctly.

---

## Q10. Is HashMap synchronized?

No.

---

## Q11. What is the difference between HashMap and Hashtable?

| HashMap | Hashtable |
|---|---|
| Modern general-purpose Map | Legacy Map |
| Not synchronized | Synchronized |
| Allows null key | Does not allow null key |
| Allows null values | Does not allow null values |
| Usually preferred for non-concurrent use | Legacy API |

---

## Q12. HashMap vs LinkedHashMap?

| HashMap | LinkedHashMap |
|---|---|
| No ordering guarantee | Predictable ordering |
| Hash-based | Hash + linked ordering |
| Usually slightly less bookkeeping | Maintains linked order |

---

## Q13. HashMap vs TreeMap?

| HashMap | TreeMap |
|---|---|
| Hash-based | Tree-based |
| Expected O(1) lookup | O(log n) lookup |
| No sorted-key guarantee | Sorted keys |
| Allows null key | Natural ordering does not support null key |

---

## Q14. What is entrySet()?

It returns:

```java
Set<Map.Entry<K,V>>
```

representing all key-value mappings.

---

## Q15. Why is entrySet() useful?

When both key and value are needed, each Entry already contains both.

---

# 🎤 45. 30-Second Interview Answer

> **HashMap is a hash-based implementation of the Map interface that stores key-value pairs. It uses the key's hash information to locate a bucket and then uses key comparison to identify the correct entry. Its basic operations such as get, put, and remove have expected O(1) time under normal conditions. HashMap allows one null key and multiple null values, does not guarantee iteration order, and is not thread-safe. It relies heavily on the hashCode and equals contract of keys. In modern Java implementations, heavily collided buckets can use tree-based nodes.**

---

# ⚡ 46. Quick Revision

    HashMap
       ↓
    Map<K,V>
       ↓
    Key → Value

---

## Rules

    Keys → Unique

    Values → Can duplicate

    Null key → One allowed

    Null values → Multiple allowed

    Ordering → No guarantee

    Thread-safe → No

---

## Average Complexity

    put()         → O(1)
    get()         → O(1)
    remove()      → O(1)
    containsKey() → O(1)

    containsValue() → O(n)

---

## Important Concepts

    hashCode()
        ↓
    Hash calculation
        ↓
    Bucket
        ↓
    Collision handling
        ↓
    equals()
        ↓
    Entry

---

## Important Configuration

    Initial Capacity
    Load Factor
    Threshold
    Resize

Default load factor:

    0.75f

---

## Important Methods

    put()
    get()
    getOrDefault()
    putIfAbsent()
    containsKey()
    containsValue()
    remove()
    replace()
    compute()
    computeIfAbsent()
    computeIfPresent()
    merge()
    keySet()
    values()
    entrySet()

---

# 📋 47. HashMap Cheat Sheet

| Concept | HashMap |
|---|---|
| Interface | Map |
| Package | java.util |
| Data | Key → Value |
| Duplicate keys | No |
| Duplicate values | Yes |
| Null key | One |
| Null values | Yes |
| Ordering | No guarantee |
| Thread-safe | No |
| Average get | O(1) |
| Average put | O(1) |
| Average remove | O(1) |
| containsValue | O(n) |
| Key lookup | hash-based |
| Key comparison | equals() |
| Hash calculation | hashCode() |
| Default load factor | 0.75 |
| DSA frequency | Excellent |
| DSA Two Sum | Excellent |
| DSA prefix sum | Excellent |
| DSA fast lookup | Excellent |

---

# 🏁 48. Final Mental Model

    ┌────────────────────────────────────┐
    │              HashMap               │
    └──────────────────┬─────────────────┘
                       │
                       ▼
                 KEY → VALUE
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
              ┌────────┴────────┐
              │                 │
          One entry        Collision
                                │
                       ┌────────┴────────┐
                       ▼                 ▼
                    Nodes          Tree Nodes*
                       │
                       ▼
                    equals()
                       │
                       ▼
                     Value

    * Under suitable collision/treeification conditions.

---

# 🚀 HashMap in DSA

    Problem
       ↓
    Need fast lookup?
       ↓
      YES
       ↓
    HashMap
       ↓
    Choose key
       │
       ├── value → index
       ├── value → frequency
       ├── char → frequency
       ├── prefixSum → index
       ├── state → count
       └── value → occurrence
       ↓
    Expected O(n) solution

---

# 🧠 Golden Interview Memory

    HashMap = Fast Key-Based Lookup

    put()       → Insert / Update

    get()       → Retrieve

    containsKey → Check Key

    keySet()    → Keys

    values()    → Values

    entrySet()  → Key + Value

    hashCode()  → Find candidate bucket

    equals()    → Confirm key identity

    Collision   → Multiple keys, same bucket

    Load Factor → Controls resize threshold

    Mutable Key → Dangerous

    HashMap     → Not synchronized

    HashMap     → No ordering guarantee

    HashMap     → O(1) expected basic operations

---

> **Core takeaway:**  
> `HashMap` is not just a collection you use for storing key-value pairs. For interviews and DSA, think of it as a **fast lookup engine** powered by hashing. Master `put()`, `get()`, `containsKey()`, `getOrDefault()`, `merge()`, `hashCode()`, `equals()`, collisions, resizing, and the **value → index / value → frequency** patterns, and a huge number of Java + DSA problems become easier.