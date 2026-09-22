```md
# 🔐 04 — hashCode() and equals()

> **Java Collections Deep Dive → Map Framework**
>
> `hashCode()` and `equals()` are two of the most important methods behind the correct working of `HashMap`, `HashSet`, and other hash-based collections.

---

# 📑 Table of Contents

- [🧠 1. Introduction](#-1-introduction)
- [🎯 2. Why hashCode() and equals() Matter](#-2-why-hashcode-and-equals-matter)
- [🧬 3. Object Class Connection](#-3-object-class-connection)
- [🔍 4. equals()](#-4-equals)
- [🔢 5. hashCode()](#-5-hashcode)
- [⚖️ 6. == vs equals()](#-6--vs-equals)
- [📜 7. equals() Contract](#-7-equals-contract)
- [🔗 8. hashCode() Contract](#-8-hashcode-contract)
- [💥 9. Golden Rule](#-9-golden-rule)
- [🗺️ 10. HashMap Connection](#-10-hashmap-connection)
- [➕ 11. HashMap put() Flow](#-11-hashmap-put-flow)
- [🔎 12. HashMap get() Flow](#-12-hashmap-get-flow)
- [💣 13. Override Only equals()](#-13-override-only-equals)
- [💣 14. Override Only hashCode()](#-14-override-only-hashcode)
- [🧱 15. Hash Collision](#-15-hash-collision)
- [🔄 16. Mutable Keys](#-16-mutable-keys)
- [🛡️ 17. Immutable Keys](#-17-immutable-keys)
- [🧵 18. String Example](#-18-string-example)
- [📦 19. Wrapper Classes](#-19-wrapper-classes)
- [🧠 20. HashSet Connection](#-20-hashset-connection)
- [🎯 21. DSA Connection](#-21-dsa-connection)
- [🧩 22. Complete Example](#-22-complete-example)
- [⚠️ 23. Common Mistakes](#-23-common-mistakes)
- [🎤 24. 30-Second Interview Answer](#-24-30-second-interview-answer)
- [⚡ 25. Cheat Sheet](#-25-cheat-sheet)
- [🎯 26. Interview Questions](#-26-interview-questions)

---

# 🧠 1. Introduction

Every Java class directly or indirectly inherits from:

    Object

`Object` provides important methods such as:

    equals()
    hashCode()

These methods become especially important when working with:

    HashMap
    HashSet
    Hashtable
    Hash-based DSA problems

Think of them as two different responsibilities:

    equals()
        ↓
    "Are these objects logically equal?"

    hashCode()
        ↓
    "Which hash location should I search?"

---

# 🎯 2. Why hashCode() and equals() Matter

Consider:

    Map<Student, String> map = new HashMap<>();

Suppose:

    Student s1 = new Student(101);
    Student s2 = new Student(101);

If the `id` represents the identity of a Student, we may want:

    s1.equals(s2)

to return:

    true

But HashMap also depends on:

    s1.hashCode()
    s2.hashCode()

For equal objects:

    s1.hashCode() == s2.hashCode()

must be true.

Therefore:

    equals()
        +
    hashCode()

must be implemented consistently.

---

# 🧬 3. Object Class Connection

Every Java class ultimately inherits from:

    java.lang.Object

Object contains:

    public boolean equals(Object obj)

and:

    public native int hashCode();

Therefore even this class:

    class Student {
    }

automatically has:

    equals()
    hashCode()

available.

Example:

    Student s = new Student();

    s.equals(...);
    s.hashCode();

---

# 🔍 4. equals()

`equals()` is used to determine whether two objects are logically equal.

Method signature:

    public boolean equals(Object obj)

Example:

    String s1 = new String("Java");
    String s2 = new String("Java");

    System.out.println(s1 == s2);
    System.out.println(s1.equals(s2));

Output:

    false
    true

Why?

    s1 == s2

checks whether both references point to the same object.

String's `equals()` compares the contents.

---

# 🔢 5. hashCode()

`hashCode()` returns an integer associated with an object.

Method:

    public int hashCode()

Example:

    String str = "Java";

    int hash = str.hashCode();

Hash-based collections use this value to help locate elements.

Common examples:

    HashMap
    HashSet
    Hashtable

Important:

> `hashCode()` is NOT guaranteed to be unique.

---

# ⚖️ 6. == vs equals()

## `==`

For primitives:

    int a = 10;
    int b = 10;

    a == b

compares values.

For object references:

    Student s1 = new Student();
    Student s2 = new Student();

    s1 == s2

checks whether both references point to the same object.

---

## equals()

For objects:

    s1.equals(s2)

checks logical equality according to the class implementation.

---

# 🧠 Quick Comparison

    ┌───────────────┬──────────────────────────┐
    │ ==            │ equals()                 │
    ├───────────────┼──────────────────────────┤
    │ Operator      │ Method                   │
    │ Identity      │ Logical equality         │
    │ Reference     │ Depends on implementation│
    │ Can compare   │ Primarily objects        │
    │ primitives    │                          │
    └───────────────┴──────────────────────────┘

---

# 📜 7. equals() Contract

The `equals()` contract contains five important properties:

    1. Reflexive
    2. Symmetric
    3. Transitive
    4. Consistent
    5. Non-null

---

## 7.1 Reflexive

An object must equal itself.

    x.equals(x)

must return:

    true

---

## 7.2 Symmetric

If:

    x.equals(y)

is true, then:

    y.equals(x)

must also be true.

Therefore:

    x.equals(y) == y.equals(x)

---

## 7.3 Transitive

If:

    x.equals(y)
    y.equals(z)

are true, then:

    x.equals(z)

must also be true.

---

## 7.4 Consistent

Repeated calls should produce the same result as long as the relevant state has not changed.

    x.equals(y)
    x.equals(y)
    x.equals(y)

should remain consistent.

---

## 7.5 Non-null

For a non-null object:

    x.equals(null)

should return:

    false

---

# 🔗 8. hashCode() Contract

The most important rule is:

> If two objects are equal according to `equals()`, they MUST have the same hash code.

Therefore:

    x.equals(y) == true

implies:

    x.hashCode() == y.hashCode()

But the reverse is NOT required.

This is valid:

    x.hashCode() == y.hashCode()

while:

    x.equals(y) == false

This situation is called a:

    Hash Collision

---

# 💥 9. Golden Rule

Memorize this rule for interviews:

    x.equals(y) == true
            ↓
    x.hashCode() == y.hashCode()

But NOT:

    x.hashCode() == y.hashCode()
            ↓
    x.equals(y) == true

The second implication is incorrect.

---

# 🗺️ 10. HashMap Connection

Suppose:

    Map<Student, String> map = new HashMap<>();

When we execute:

    map.put(student, "Java");

HashMap conceptually uses:

    student.hashCode()
            ↓
       hash calculation
            ↓
       bucket selection
            ↓
       compare candidate keys
            ↓
          equals()
            ↓
       identify the key

Therefore:

    hashCode()
        ↓
    WHERE should I look?

and:

    equals()
        ↓
    IS this the key?

This is one of the most important HashMap mental models.

---

# ➕ 11. HashMap put() Flow

Suppose:

    map.put(key, value);

Conceptually:

    key
     ↓
    hashCode()
     ↓
    hash calculation
     ↓
    bucket index
     ↓
    bucket
     ↓
    existing node?
       ↙       ↘
     no         yes
     ↓           ↓
   insert      compare
                 ↓
              equals()
                 ↓
        same key / collision

If the key is logically equal to an existing key, HashMap updates the corresponding value.

---

# 🔎 12. HashMap get() Flow

Suppose:

    map.get(key);

Conceptually:

    key
     ↓
    hashCode()
     ↓
    bucket index
     ↓
    bucket
     ↓
    candidate entries
     ↓
    compare hash
     ↓
    equals()
     ↓
    matching key
     ↓
    return value

So:

    hashCode()
        ↓
    narrows the search

    equals()
        ↓
    confirms the key

---

# 💣 13. Override Only equals()

Suppose we write:

    class Student {

        int id;

        @Override
        public boolean equals(Object obj) {

            if (!(obj instanceof Student)) {
                return false;
            }

            Student other = (Student) obj;

            return this.id == other.id;
        }
    }

Now:

    Student s1 = new Student(101);
    Student s2 = new Student(101);

may produce:

    s1.equals(s2)
        → true

But if `hashCode()` is not overridden, they may have different hash codes.

That violates the hashCode contract.

---

## 🚨 HashSet Problem

Suppose:

    Set<Student> set = new HashSet<>();

    set.add(s1);
    set.add(s2);

You may expect:

    size = 1

because:

    s1.equals(s2) == true

But if their hash codes differ, HashSet may place them into different buckets.

Therefore the result can incorrectly behave as if they are different elements.

---

# 💣 14. Override Only hashCode()

Suppose we write:

    @Override
    public int hashCode() {
        return Integer.hashCode(id);
    }

but don't override `equals()`.

Then two objects can have:

    same hashCode

while:

    equals() == false

because Object's default equality is identity-based.

This is legal.

Remember:

    same hashCode
        ≠
    equal objects

---

# 🧱 15. Hash Collision

A hash collision occurs when different objects produce the same hash code or ultimately map to the same bucket.

Example:

    Object A
       ↓
    hash = 100

    Object B
       ↓
    hash = 100

But:

    A.equals(B)

may be:

    false

This is completely valid.

Hash-based collections are designed to handle collisions.

---

# 🧠 Collision Mental Model

    Object A ──→ hash 100 ──→ bucket 5
                                  │
                                  ├── A
                                  │
                                  └── B

    Object B ──→ hash 100 ──→ bucket 5

Then:

    equals()

helps determine whether A and B are actually the same logical key.

---

# 🔄 16. Mutable Keys

One of the most important HashMap traps is modifying a key after insertion.

Suppose:

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

            if (!(obj instanceof Student)) {
                return false;
            }

            Student other = (Student) obj;

            return this.id == other.id;
        }
    }

Now:

    Student s = new Student(101);

    Map<Student, String> map = new HashMap<>();

    map.put(s, "Java");

Initially:

    id = 101
       ↓
    hash = H1
       ↓
    bucket = B1

Now suppose:

    s.id = 999;

Then:

    id = 999
       ↓
    hash = H2
       ↓
    bucket = B2

But the existing entry is still physically stored according to its original location.

Therefore:

    map.get(s)

may fail to find the entry.

---

# 🚨 Mutable Key Problem

The dangerous sequence is:

    create key
       ↓
    put key into HashMap
       ↓
    modify field used by
    equals()/hashCode()
       ↓
    hash changes
       ↓
    lookup fails

Therefore:

> Avoid modifying fields that participate in `equals()` and `hashCode()` while the object is being used as a HashMap key.

---

# 🛡️ 17. Immutable Keys

Immutable objects are generally safer as HashMap keys.

Examples:

    String
    Integer
    Long
    Enum

Why?

Because their equality-related state does not change after creation.

For example:

    String key = "Java";

The String's contents cannot be modified.

Therefore its hash-related behavior remains stable.

---

# 🧵 18. String Example

String overrides:

    equals()
    hashCode()

Example:

    String s1 = new String("Java");
    String s2 = new String("Java");

Then:

    s1.equals(s2)

returns:

    true

and:

    s1.hashCode() == s2.hashCode()

returns:

    true

Therefore String works correctly as a HashMap key.

Example:

    Map<String, Integer> map = new HashMap<>();

    map.put("Java", 100);

    System.out.println(map.get("Java"));

Output:

    100

---

# 📦 19. Wrapper Classes

Wrapper classes provide suitable implementations of equality and hashing.

Examples:

    Integer
    Long
    Short
    Byte
    Character
    Boolean

Example:

    Integer a = 100;
    Integer b = 100;

    System.out.println(a.equals(b));

Output:

    true

Their hash codes are also consistent with equality.

Therefore they are commonly used as HashMap keys.

---

# 🧠 20. HashSet Connection

HashSet also depends on hashing and equality.

Suppose:

    Set<Integer> set = new HashSet<>();

    set.add(10);
    set.add(10);

Conceptually:

    10
     ↓
    hashCode()
     ↓
    bucket
     ↓
    equals()
     ↓
    already exists?
     ↓
    yes
     ↓
    don't add duplicate

Therefore:

    HashSet
       ↓
    hashCode()
       +
    equals()

---

# 🎯 21. DSA Connection

Understanding `hashCode()` and `equals()` directly helps with common DSA patterns.

---

## 🔥 Pattern 1 — Duplicate Detection

Problem:

    Find whether an array contains duplicates.

Approach:

    HashSet

Code:

    Set<Integer> set = new HashSet<>();

    for (int num : nums) {

        if (!set.add(num)) {
            return true;
        }
    }

    return false;

Expected complexity:

    Time:  O(n)
    Space: O(n)

---

## 🔥 Pattern 2 — Frequency Counting

Problem:

    Count frequency of each number.

Code:

    Map<Integer, Integer> freq = new HashMap<>();

    for (int num : nums) {

        freq.put(
            num,
            freq.getOrDefault(num, 0) + 1
        );
    }

Complexity:

    Time:  O(n) expected
    Space: O(n)

---

## 🔥 Pattern 3 — Two Sum

Code:

    Map<Integer, Integer> map = new HashMap<>();

    for (int i = 0; i < nums.length; i++) {

        int required = target - nums[i];

        if (map.containsKey(required)) {

            return new int[] {
                map.get(required),
                i
            };
        }

        map.put(nums[i], i);
    }

    return new int[0];

The important HashMap operations are expected O(1):

    containsKey()
    get()
    put()

Therefore:

    O(n)

overall expected time.

---

# 🧩 22. Complete Example

    import java.util.*;

    class Student {

        private int id;
        private String name;

        public Student(int id, String name) {
            this.id = id;
            this.name = name;
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

        @Override
        public int hashCode() {
            return Integer.hashCode(id);
        }
    }

    public class Main {

        public static void main(String[] args) {

            Student s1 =
                new Student(101, "Rahul");

            Student s2 =
                new Student(101, "Aman");

            System.out.println(
                s1.equals(s2)
            );

            System.out.println(
                s1.hashCode() == s2.hashCode()
            );

            Set<Student> set =
                new HashSet<>();

            set.add(s1);
            set.add(s2);

            System.out.println(set.size());
        }
    }

Expected output:

    true
    true
    1

Why?

Both objects have:

    id = 101

and our equality logic uses only:

    id

Therefore:

    s1.equals(s2)
        → true

and their hash codes are equal.

---

# ⚠️ 23. Common Mistakes

## ❌ Mistake 1

    Same hashCode means objects are equal.

Correct:

    Same hashCode
        ≠
    equals true

---

## ❌ Mistake 2

    Equal objects can have different hashCodes.

Incorrect.

Correct:

    equals true
        ⇒
    same hashCode

---

## ❌ Mistake 3

Overriding only `equals()`.

Better:

    Override equals()
        +
    Override hashCode()

---

## ❌ Mistake 4

Thinking hashCode is a unique ID.

It isn't.

Multiple objects can have the same hash code.

---

## ❌ Mistake 5

Thinking hashCode is a memory address.

Don't assume this.

The Java contract does not define hashCode as a memory address.

---

## ❌ Mistake 6

Using mutable fields in a HashMap key and modifying them later.

This can make the key effectively unreachable.

---

## ❌ Mistake 7

Comparing Strings using:

    ==

Use:

    equals()

for content comparison.

---

# 🎤 24. 30-Second Interview Answer

> `equals()` is used to determine logical equality between objects, while `hashCode()` returns an integer used by hash-based collections such as HashMap and HashSet to help locate objects efficiently. The most important contract is that if two objects are equal according to `equals()`, they must have the same hash code. However, the same hash code does not guarantee equality because collisions are possible. HashMap uses the hash code to locate a bucket and then equality checks to identify the correct key. Therefore, when overriding equals(), we should also override hashCode() consistently.

---

# ⚡ 25. Cheat Sheet

| Concept | Meaning |
|---|---|
| `==` | Identity/reference comparison for objects |
| `equals()` | Logical equality |
| `hashCode()` | Integer hash value |
| Equal objects | Must have same hash code |
| Same hash code | Does not mean equal |
| Collision | Different objects share a hash/bucket |
| HashMap | Uses hashCode + equals |
| HashSet | Uses hashCode + equals |
| String | Overrides equals + hashCode |
| Wrapper classes | Override equality/hashing appropriately |
| Mutable key | Dangerous |
| Immutable key | Generally safer |
| `equals()` only | Can break hash-based collections |
| `hashCode()` only | Doesn't establish logical equality |
| Expected HashMap lookup | O(1) |
| Expected HashMap insertion | O(1) |
| HashSet duplicate check | Expected O(1) |

---

# 🧠 Core Mental Model

    KEY
     │
     ▼
    hashCode()
     │
     ▼
    hash calculation
     │
     ▼
    bucket
     │
     ▼
    candidate entries
     │
     ▼
    equals()
     │
     ▼
    matching key
     │
     ▼
    VALUE

Remember:

    hashCode()
        ↓
    WHERE should I look?

    equals()
        ↓
    WHICH key is it?

---

# 🎯 26. Interview Questions

## Q1. What is hashCode()?

`hashCode()` returns an integer hash value associated with an object and is heavily used by hash-based collections.

---

## Q2. What is equals()?

`equals()` determines logical equality between two objects according to the implementation of the class.

---

## Q3. What is the contract between equals() and hashCode()?

If:

    a.equals(b) == true

then:

    a.hashCode() == b.hashCode()

must be true.

---

## Q4. Can two unequal objects have the same hash code?

Yes.

That is a hash collision.

---

## Q5. Can two equal objects have different hash codes?

No, assuming the methods correctly follow their contract.

---

## Q6. Why override hashCode() when overriding equals()?

Because HashMap and HashSet use hash codes to determine where to search. Equal objects must therefore produce the same hash code.

---

## Q7. What happens if only equals() is overridden?

Equal objects may generate different hash codes, causing incorrect behavior in hash-based collections.

---

## Q8. What happens if only hashCode() is overridden?

Objects may have the same hash code but still be unequal because `equals()` may still use identity-based equality.

---

## Q9. Is hashCode() unique?

No.

---

## Q10. Is hashCode() the object's memory address?

No. That is not part of the Java specification.

---

## Q11. Why are immutable keys preferred?

Because changing a field involved in equality or hashing after insertion can make a HashMap entry difficult or impossible to locate through normal lookup.

---

## Q12. Why does HashMap use equals() after hashCode()?

Because multiple keys can have the same hash code or bucket. `equals()` determines whether the candidate key is logically the requested key.

---

## Q13. What is the difference between identity and equality?

Identity means the references point to the same object.

Equality means the objects are logically equivalent according to `equals()`.

---

## Q14. Which method determines the bucket?

Conceptually, the key's hash value contributes to determining the bucket.

---

## Q15. Which method confirms the matching key?

`equals()`.

---

# 🏆 Final Memory Trick

    hashCode()
        ↓
    LOCATION

    equals()
        ↓
    EQUALITY

Therefore:

    HashMap

        KEY
         ↓
      hashCode()
         ↓
       bucket
         ↓
       equals()
         ↓
       VALUE

---

# 🚀 Final Takeaway

> **`hashCode()` helps HashMap find WHERE to search, while `equals()` determines WHETHER the key found is logically the same key.**

The golden rule:

    equals() == true
            ↓
    hashCode() MUST be same

But:

    hashCode() same
            ↓
    does NOT guarantee
    equals() == true

Once this relationship is clear, the behavior of:

    HashMap
    HashSet
    Hashtable
    Duplicate Detection
    Frequency Maps
    Two Sum
    Many Hashing DSA Problems

becomes much easier to understand.
```
