# 10 — HashSet

> **Package:** `java.util`  
> **Type:** Class  
> **Implements:** `Set<E>`  
> **Internal concept:** Hash table  
> **Main property:** Unique elements with no guaranteed iteration order

---

# Table of Contents

1. Introduction
2. What is HashSet
3. HashSet Hierarchy
4. Why HashSet Exists
5. Key Characteristics
6. Creating HashSet
7. Adding Elements
8. Duplicate Elements
9. How HashSet Prevents Duplicates
10. hashCode() and equals()
11. Hash Collision
12. Internal Working
13. HashSet and Buckets
14. HashSet and null
15. Ordering
16. Removing Elements
17. Searching Elements
18. Size and Empty Checks
19. Iterating HashSet
20. HashSet Methods
21. add()
22. addAll()
23. remove()
24. removeAll()
25. retainAll()
26. contains()
27. containsAll()
28. removeIf()
29. clear()
30. isEmpty()
31. size()
32. iterator()
33. toArray()
34. HashSet with Custom Objects
35. equals() and hashCode() Contract
36. Mutable Objects Trap
37. Time Complexity
38. Capacity
39. Load Factor
40. Rehashing
41. Initial Capacity
42. HashSet vs LinkedHashSet
43. HashSet vs TreeSet
44. HashSet vs List
45. HashSet vs HashMap
46. When to Use HashSet
47. When Not to Use HashSet
48. Advantages
49. Disadvantages
50. Common Mistakes
51. Interview Traps
52. Practical Examples
53. Top Interview Questions
54. 30-Second Interview Answer
55. Cheat Sheet
56. Quick Revision

---

# 1. Introduction

`HashSet` is one of the most commonly used implementations of the `Set` interface.

Package:

    java.util

Declaration:

    public class HashSet<E>
        extends AbstractSet<E>
        implements Set<E>, Cloneable, Serializable

Its primary purpose is to store:

    UNIQUE ELEMENTS

Example:

    Set<Integer> numbers =
        new HashSet<>();

    numbers.add(10);
    numbers.add(20);
    numbers.add(10);

The result contains:

    10
    20

The duplicate `10` is not stored.

---

# 2. What is HashSet?

`HashSet` is a hash-table-based implementation of the `Set` interface.

It provides:

    No duplicate elements
    Fast average insertion
    Fast average searching
    Fast average removal
    No guaranteed iteration order
    One null element

Example:

    HashSet<Integer> numbers =
        new HashSet<>();

    numbers.add(10);
    numbers.add(20);
    numbers.add(30);

---

# 3. HashSet Hierarchy

The inheritance hierarchy is:

    Object
       |
    AbstractCollection
       |
    AbstractSet
       |
    HashSet
       |
    LinkedHashSet

HashSet also implements:

    Set
      |
    Collection
      |
    Iterable

Conceptually:

    Iterable
       |
    Collection
       |
      Set
       |
    AbstractSet
       |
    HashSet
       |
    LinkedHashSet

Important:

    HashSet
        -> Class

    Set
        -> Interface

---

# 4. Why HashSet Exists

Suppose you want to store:

    10
    20
    10
    30
    20

If you use a List:

    [10, 20, 10, 30, 20]

But sometimes duplicates are unwanted.

With HashSet:

    [10, 20, 30]

HashSet provides a convenient way to maintain:

    uniqueness
        +
    efficient membership checking

---

# 5. Key Characteristics

## 5.1 No Duplicates

HashSet does not allow duplicate elements.

    Set<Integer> set =
        new HashSet<>();

    set.add(10);
    set.add(10);
    set.add(10);

Only one `10` is stored.

---

## 5.2 No Guaranteed Order

HashSet does not guarantee insertion order.

Example:

    set.add(30);
    set.add(10);
    set.add(20);

Do not expect:

    [30, 10, 20]

The iteration order can be different.

Important:

> Never write code that depends on HashSet's iteration order.

---

## 5.3 Allows One Null

HashSet allows one `null`.

Example:

    Set<String> set =
        new HashSet<>();

    set.add(null);
    set.add(null);

Only one null is stored.

---

## 5.4 Fast Average Operations

Typical average complexity:

    add()
        -> O(1)

    remove()
        -> O(1)

    contains()
        -> O(1)

These are average-case complexities.

They are not universal guarantees for every situation.

---

# 6. Creating HashSet

## Basic

    HashSet<Integer> numbers =
        new HashSet<>();

---

## Using Set Reference

Preferred when programming to an interface:

    Set<Integer> numbers =
        new HashSet<>();

This gives flexibility to replace the implementation later.

---

## With Initial Capacity

    HashSet<Integer> numbers =
        new HashSet<>(100);

---

## With Capacity and Load Factor

    HashSet<Integer> numbers =
        new HashSet<>(
            100,
            0.75f
        );

---

## From Another Collection

    List<Integer> numbers =
        List.of(
            10,
            20,
            10,
            30
        );

    Set<Integer> unique =
        new HashSet<>(numbers);

Result:

    [10, 20, 30]

---

# 7. Adding Elements

Use:

    add()

Example:

    Set<String> languages =
        new HashSet<>();

    languages.add("Java");
    languages.add("C");
    languages.add("Python");

---

## Return Value

`add()` returns:

    true
        -> element was added

    false
        -> element already existed

Example:

    Set<Integer> numbers =
        new HashSet<>();

    System.out.println(
        numbers.add(10)
    );

    System.out.println(
        numbers.add(10)
    );

Output:

    true
    false

---

# 8. Duplicate Elements

Consider:

    Set<Integer> numbers =
        new HashSet<>();

    numbers.add(10);
    numbers.add(20);
    numbers.add(10);
    numbers.add(30);
    numbers.add(20);

Logical contents:

    10
    20
    30

The duplicate values are ignored.

Conceptually:

    add(10)
        -> true

    add(20)
        -> true

    add(10)
        -> false

    add(30)
        -> true

    add(20)
        -> false

---

# 9. How HashSet Prevents Duplicates

This is one of the most important HashSet interview topics.

For normal hash-based objects, HashSet relies on:

    hashCode()
        +
    equals()

Conceptually:

    add(element)
          |
          v
    calculate hashCode()
          |
          v
      find bucket
          |
          v
    compare existing objects
          |
       equals()
          |
       +--+--+
       |     |
      true  false
       |     |
   duplicate add element

Important:

`hashCode()` helps locate the possible location.

`equals()` confirms logical equality.

---

# 10. hashCode() and equals()

Suppose:

    String a = new String("Java");

    String b = new String("Java");

Then:

    a.equals(b)

returns:

    true

Therefore their hash codes must also be equal.

So:

    a.hashCode()
        ==
    b.hashCode()

The HashSet recognizes them as logically equal.

Example:

    Set<String> set =
        new HashSet<>();

    set.add(a);
    set.add(b);

Size:

    1

---

# 11. Hash Collision

Different objects can have the same hash code.

Example conceptually:

    Object A
        |
    hashCode()
        |
       100

    Object B
        |
    hashCode()
        |
       100

This is called:

    Hash Collision

Important:

> Same hash code does NOT mean objects are equal.

The Set must still use equality checking.

Conceptually:

    same hashCode
         |
         v
    possible collision
         |
         v
      equals()
         |
      +--+--+
      |     |
     true  false
      |     |
   duplicate separate objects

---

# 12. Internal Working

A simplified view:

    HashSet
       |
       v
    HashMap
       |
       v
    Hash table
       |
       v
    Buckets

In OpenJDK, `HashSet` is implemented using a `HashMap` internally.

Conceptually, when you write:

    set.add("Java");

HashSet internally stores the element as a key in a backing HashMap.

Conceptually:

    HashSet

    "Java"
       |
       v
    HashMap

    key = "Java"
    value = dummy object

The actual implementation details are JDK-specific, but the important interview concept is:

> HashSet is backed by a HashMap.

---

# 13. HashSet and Buckets

A hash table consists conceptually of buckets.

Example:

    Bucket 0
    Bucket 1
    Bucket 2
    Bucket 3
    Bucket 4
    Bucket 5
    ...

When an element is added:

    element
       |
       v
    hashCode()
       |
       v
    hash calculation
       |
       v
    bucket index
       |
       v
    store/check element

If multiple elements map to the same bucket:

    Bucket
      |
      +---- Element A
      |
      +---- Element B
      |
      +---- Element C

This is a collision.

Modern Java HashMap-based implementations can use tree structures for heavily populated buckets under certain conditions.

---

# 14. HashSet and null

HashSet allows one `null`.

Example:

    Set<String> names =
        new HashSet<>();

    names.add(null);
    names.add(null);

Result:

    [null]

Why only one?

Because Set does not allow duplicates.

Therefore:

    null
    null

represents the same logical element.

---

# 15. Ordering

HashSet does not guarantee:

    insertion order

or:

    sorted order

Example:

    Set<Integer> numbers =
        new HashSet<>();

    numbers.add(50);
    numbers.add(10);
    numbers.add(30);

Do not assume output:

    [50, 10, 30]

or:

    [10, 30, 50]

The order is unspecified.

If you need insertion order:

    LinkedHashSet

If you need sorted order:

    TreeSet

---

# 16. Removing Elements

The main method is:

    remove()

Example:

    Set<Integer> numbers =
        new HashSet<>();

    numbers.add(10);
    numbers.add(20);

    numbers.remove(10);

Now:

    [20]

---

# 17. Searching Elements

Use:

    contains()

Example:

    Set<String> languages =
        new HashSet<>();

    languages.add("Java");
    languages.add("Python");

    System.out.println(
        languages.contains("Java")
    );

Output:

    true

Typical HashSet average complexity:

    O(1)

---

# 18. Size and Empty Checks

## size()

    Set<Integer> numbers =
        new HashSet<>();

    numbers.add(10);
    numbers.add(20);

    System.out.println(
        numbers.size()
    );

Output:

    2

---

## isEmpty()

    System.out.println(
        numbers.isEmpty()
    );

Output:

    false

---

# 19. Iterating HashSet

## Enhanced For Loop

    Set<String> languages =
        new HashSet<>();

    languages.add("Java");
    languages.add("Spring");
    languages.add("React");

    for (String language : languages) {

        System.out.println(language);
    }

Remember:

> The order of output is not guaranteed.

---

## Iterator

    Iterator<String> iterator =
        languages.iterator();

    while (iterator.hasNext()) {

        System.out.println(
            iterator.next()
        );
    }

---

## forEach()

    languages.forEach(
        language ->
            System.out.println(language)
    );

---

# 20. HashSet Methods

Important methods:

    add()
    addAll()

    remove()
    removeAll()

    retainAll()

    contains()
    containsAll()

    removeIf()

    size()
    isEmpty()

    clear()

    iterator()

    toArray()

    stream()

    parallelStream()

    spliterator()

---

# 21. add()

Adds an element if it does not already exist.

Syntax:

    boolean add(E e)

Example:

    Set<Integer> set =
        new HashSet<>();

    boolean result =
        set.add(10);

    System.out.println(result);

Output:

    true

Duplicate:

    result = set.add(10);

Output:

    false

---

# 22. addAll()

Adds all elements from another Collection.

Example:

    Set<Integer> A =
        new HashSet<>(
            Set.of(1, 2, 3)
        );

    Set<Integer> B =
        new HashSet<>(
            Set.of(3, 4, 5)
        );

    A.addAll(B);

Result:

    [1, 2, 3, 4, 5]

This is useful for:

    Union

---

# 23. remove()

Removes a specific element.

Example:

    Set<String> names =
        new HashSet<>();

    names.add("Java");
    names.add("Python");

    boolean result =
        names.remove("Java");

    System.out.println(result);

Output:

    true

If the element doesn't exist:

    false

---

# 24. removeAll()

Removes all elements that are also present in another Collection.

Example:

    Set<Integer> A =
        new HashSet<>(
            Set.of(1, 2, 3, 4)
        );

    Set<Integer> B =
        new HashSet<>(
            Set.of(3, 4, 5)
        );

    A.removeAll(B);

Result:

    [1, 2]

This represents:

    A - B

---

# 25. retainAll()

Keeps only elements that exist in another Collection.

Example:

    Set<Integer> A =
        new HashSet<>(
            Set.of(1, 2, 3, 4)
        );

    Set<Integer> B =
        new HashSet<>(
            Set.of(3, 4, 5)
        );

    A.retainAll(B);

Result:

    [3, 4]

This represents:

    A ∩ B

---

# 26. contains()

Checks whether an element exists.

Example:

    Set<Integer> numbers =
        new HashSet<>();

    numbers.add(10);
    numbers.add(20);

    System.out.println(
        numbers.contains(20)
    );

Output:

    true

Typical average complexity:

    O(1)

---

# 27. containsAll()

Checks whether all elements of another Collection exist.

Example:

    Set<Integer> A =
        new HashSet<>(
            Set.of(
                10,
                20,
                30
            )
        );

    Set<Integer> B =
        new HashSet<>(
            Set.of(
                10,
                20
            )
        );

    System.out.println(
        A.containsAll(B)
    );

Output:

    true

---

# 28. removeIf()

Removes elements matching a condition.

Example:

    Set<Integer> numbers =
        new HashSet<>(
            Set.of(
                10,
                15,
                20,
                25
            )
        );

    numbers.removeIf(
        n -> n % 2 != 0
    );

Result:

    [10, 20]

---

# 29. clear()

Removes all elements.

Example:

    Set<Integer> numbers =
        new HashSet<>(
            Set.of(10, 20, 30)
        );

    numbers.clear();

    System.out.println(numbers);

Output:

    []

---

# 30. isEmpty()

Checks whether the HashSet contains zero elements.

Example:

    Set<Integer> numbers =
        new HashSet<>();

    System.out.println(
        numbers.isEmpty()
    );

Output:

    true

---

# 31. size()

Returns the number of elements.

Example:

    Set<String> languages =
        new HashSet<>();

    languages.add("Java");
    languages.add("C");
    languages.add("Python");

    System.out.println(
        languages.size()
    );

Output:

    3

---

# 32. iterator()

Returns an Iterator.

Example:

    Set<Integer> numbers =
        new HashSet<>(
            Set.of(10, 20, 30)
        );

    Iterator<Integer> iterator =
        numbers.iterator();

    while (iterator.hasNext()) {

        Integer number =
            iterator.next();

        System.out.println(number);
    }

---

## Removing Through Iterator

This is a safe way to remove the current element during iteration.

    Iterator<Integer> iterator =
        numbers.iterator();

    while (iterator.hasNext()) {

        Integer number =
            iterator.next();

        if (number > 20) {

            iterator.remove();
        }
    }

---

# 33. toArray()

Converts the Set into an array.

Example:

    Set<String> names =
        new HashSet<>(
            Set.of(
                "Java",
                "Spring",
                "React"
            )
        );

    Object[] array =
        names.toArray();

---

Typed version:

    String[] array =
        names.toArray(
            new String[0]
        );

---

# 34. HashSet with Custom Objects

Consider:

    class Student {

        int id;
        String name;

        Student(
            int id,
            String name
        ) {
            this.id = id;
            this.name = name;
        }
    }

Now:

    Set<Student> students =
        new HashSet<>();

    students.add(
        new Student(
            1,
            "Rahul"
        )
    );

    students.add(
        new Student(
            1,
            "Rahul"
        )
    );

Without properly defining equality semantics, HashSet may treat these as different objects because they are different object instances.

This is why custom classes should correctly implement:

    equals()
    hashCode()

when logical equality is required.

---

# 35. equals() and hashCode() Contract

For a class used in HashSet:

If:

    a.equals(b)

is true,

then:

    a.hashCode() == b.hashCode()

must also be true.

Example:

    class Student {

        int id;
        String name;

        Student(
            int id,
            String name
        ) {
            this.id = id;
            this.name = name;
        }

        @Override
        public boolean equals(
            Object obj
        ) {

            if (this == obj) {
                return true;
            }

            if (!(obj instanceof Student)) {
                return false;
            }

            Student other =
                (Student) obj;

            return id == other.id
                && Objects.equals(
                    name,
                    other.name
                );
        }

        @Override
        public int hashCode() {

            return Objects.hash(
                id,
                name
            );
        }
    }

Now logically equal Student objects can be treated as duplicates by HashSet.

---

# 36. Mutable Objects Trap

This is an important advanced interview concept.

Suppose an object is added to HashSet.

Its `hashCode()` depends on:

    id

Then after insertion:

    id

is changed.

Now its hash code may change.

The object may no longer be found where HashSet expects it.

Example conceptually:

    Student
       |
    id = 10
       |
    hashCode = 500

Add to HashSet.

Then:

    id = 20

Now:

    hashCode = 800

The object may physically remain in the old bucket while lookups use the new hash.

Therefore:

> Avoid mutating fields used by equals() and hashCode() while an object is stored in a HashSet.

---

# 37. Time Complexity

Typical average complexities:

| Operation | Average Complexity |
|---|---:|
| add() | O(1) |
| remove() | O(1) |
| contains() | O(1) |
| size() | O(1) |
| isEmpty() | O(1) |
| clear() | O(n) |
| iteration | O(n) |

Important:

HashSet performance is based on hashing and depends on factors such as:

    hash distribution
    collisions
    table capacity
    implementation details

Do not claim every HashSet operation is always O(1).

---

# 38. Capacity

Capacity represents the number of buckets available in the underlying hash table.

Conceptually:

    HashSet
       |
       v
    HashMap
       |
       v
    bucket array

Example:

    HashSet<Integer> set =
        new HashSet<>(100);

The constructor provides an initial capacity hint.

Important:

> Capacity is not the same thing as size.

Example:

    capacity = 100

does not mean:

    size = 100

You can have:

    capacity = 100
    size = 10

---

# 39. Load Factor

Load factor determines how full the hash table can become before resizing.

Default HashSet construction uses a default load factor of:

    0.75

Conceptually:

    threshold =
        capacity × loadFactor

Example:

    capacity = 16
    loadFactor = 0.75

Then:

    threshold = 16 × 0.75
              = 12

When the table reaches its resizing threshold, the underlying table can be resized.

---

# 40. Rehashing

When the hash table becomes sufficiently full, it is resized.

Conceptually:

    Initial table
        |
    [16 buckets]
        |
    becomes sufficiently full
        |
        v
    resize
        |
        v
    larger table
        |
        v
    redistribute/reposition entries

This process is commonly referred to as:

    Rehashing

More precisely, modern HashMap implementations resize the table and redistribute entries according to the new capacity.

Why?

To maintain efficient access.

---

# 41. Initial Capacity

You can provide initial capacity.

Example:

    HashSet<Integer> numbers =
        new HashSet<>(1000);

This can be useful when you know approximately how many elements will be stored.

Why?

It can reduce repeated resizing.

---

## Capacity vs Size

Remember:

    Capacity
        -> internal bucket capacity

    Size
        -> number of stored elements

Example:

    HashSet<Integer> set =
        new HashSet<>(100);

    set.add(10);

Then conceptually:

    capacity ≠ 1

    size = 1

---

# 42. HashSet vs LinkedHashSet

| Feature | HashSet | LinkedHashSet |
|---|---|---|
| Duplicates | No | No |
| Ordering | No guaranteed order | Insertion order |
| Hash-based | Yes | Yes |
| Extra linked structure | No | Yes |
| Average add | O(1) | O(1) |
| Average contains | O(1) | O(1) |
| Memory | Lower than LinkedHashSet | Higher |

Use:

    HashSet

when order is irrelevant.

Use:

    LinkedHashSet

when insertion order matters.

---

# 43. HashSet vs TreeSet

| Feature | HashSet | TreeSet |
|---|---|---|
| Duplicates | No | No |
| Ordering | No guaranteed order | Sorted |
| Data structure concept | Hash table | Tree |
| add() average | O(1) | O(log n) |
| contains() average | O(1) | O(log n) |
| Navigation | No | Yes |
| Null | One null | Normally no null with natural ordering |

Use HashSet when:

    uniqueness + fast average lookup

Use TreeSet when:

    uniqueness + sorted order + navigation

---

# 44. HashSet vs List

| Feature | HashSet | List |
|---|---|---|
| Duplicates | No | Yes |
| Index | No | Yes |
| get(index) | No | Yes |
| Membership search | Average O(1) | Usually O(n) |
| Ordering | No guaranteed order | Depends on implementation, commonly insertion order |
| Main purpose | Uniqueness | Ordered sequence |

Example:

Use List:

    [A, B, A, C]

Use HashSet:

    [A, B, C]

---

# 45. HashSet vs HashMap

This is a very common interview question.

## HashSet

Stores:

    Elements

Example:

    Set<String> names

---

## HashMap

Stores:

    Key-Value pairs

Example:

    Map<Integer, String> students

Conceptually:

    HashSet
        |
        v
    HashMap
        |
        +---- element as key
        |
        +---- dummy value

Therefore:

> HashSet is implemented using a HashMap internally in the JDK.

---

# 46. When to Use HashSet

Use HashSet when you need:

## 1. Unique Elements

    Set<Integer> ids =
        new HashSet<>();

---

## 2. Fast Membership Checking

    if (ids.contains(id)) {
        // exists
    }

---

## 3. Duplicate Detection

    Set<Integer> seen =
        new HashSet<>();

---

## 4. Remove Duplicates

    Set<Integer> unique =
        new HashSet<>(numbers);

---

## 5. Visited Nodes

    Set<Integer> visited =
        new HashSet<>();

Useful in:

    BFS
    DFS
    Graph traversal

---

# 47. When Not to Use HashSet

Do not use HashSet when:

## 1. You Need Index Access

Use:

    ArrayList

---

## 2. You Need Insertion Order

Use:

    LinkedHashSet

---

## 3. You Need Sorted Data

Use:

    TreeSet

---

## 4. You Need Key-Value Pairs

Use:

    HashMap

---

## 5. You Need Duplicates

Use:

    List

---

# 48. Advantages

## 1. Prevents Duplicates

Uniqueness is automatic.

---

## 2. Fast Average Lookup

Typical:

    contains()
        -> O(1)

---

## 3. Fast Average Insertion

Typical:

    add()
        -> O(1)

---

## 4. Fast Average Removal

Typical:

    remove()
        -> O(1)

---

## 5. Excellent for DSA

Useful for:

    duplicate detection
    membership checking
    visited tracking
    unique values

---

# 49. Disadvantages

## 1. No Ordering Guarantee

You cannot rely on iteration order.

---

## 2. No Index Access

There is no:

    get(index)

---

## 3. Hashing Dependency

Performance depends on good hash distribution.

---

## 4. Custom Objects Need Correct Equality

Incorrect:

    equals()
    hashCode()

can cause unexpected behavior.

---

## 5. Additional Memory

Hash tables require internal bucket structures and associated memory.

---

# 50. Common Mistakes

## Mistake 1 — Assuming Insertion Order

Wrong:

    HashSet
        -> insertion order

Correct:

    HashSet
        -> no guaranteed iteration order

---

## Mistake 2 — Assuming Sorted Order

Wrong:

    HashSet
        -> sorted

Correct:

    TreeSet
        -> sorted

---

## Mistake 3 — Using get(index)

Wrong:

    set.get(0)

HashSet does not support indexed access.

---

## Mistake 4 — Assuming add() Always Adds

Wrong:

    set.add(10)
        -> always true

Correct:

    first add
        -> true

    duplicate add
        -> false

---

## Mistake 5 — Forgetting equals() and hashCode()

Custom objects require correct equality semantics when logical uniqueness is based on object state.

---

## Mistake 6 — Thinking Same hashCode Means Same Object

Wrong.

Two different objects can have the same hash code.

---

## Mistake 7 — Modifying Objects After Insertion

Changing equality/hash-related fields after insertion can make the object difficult to find.

---

## Mistake 8 — Claiming O(1) Always

Correct statement:

    HashSet basic operations
        -> O(1) average

Not:

    HashSet operations
        -> always O(1)

---

# 51. Interview Traps

## Trap 1

Question:

> Is HashSet a class or interface?

Answer:

    Class

---

## Trap 2

Question:

> Which interface does HashSet implement?

Answer:

    Set

---

## Trap 3

Question:

> Does HashSet allow duplicates?

Answer:

No.

---

## Trap 4

Question:

> Does HashSet maintain insertion order?

Answer:

No guaranteed iteration order.

---

## Trap 5

Question:

> Does HashSet allow null?

Answer:

Yes, one null element.

---

## Trap 6

Question:

> What does add() return when adding a duplicate?

Answer:

    false

---

## Trap 7

Question:

> What is average contains() complexity?

Answer:

    O(1)

---

## Trap 8

Question:

> What does HashSet use internally?

Answer:

A HashMap-backed hash table implementation.

---

## Trap 9

Question:

> How does HashSet detect duplicates?

Answer:

Conceptually through:

    hashCode()
    +
    equals()

---

## Trap 10

Question:

> Can different objects have the same hash code?

Answer:

Yes.

That is a hash collision.

---

## Trap 11

Question:

> If two objects have the same hash code, are they necessarily equal?

Answer:

No.

---

## Trap 12

Question:

> If two objects are equal, can they have different hash codes?

Answer:

No, not if the `equals()`/`hashCode()` contract is correctly implemented.

---

## Trap 13

Question:

> Why does HashSet not support get(index)?

Because it is a Set rather than an indexed sequence.

---

## Trap 14

Question:

> HashSet or TreeSet for sorted unique values?

    TreeSet

---

## Trap 15

Question:

> HashSet or LinkedHashSet for insertion order?

    LinkedHashSet

---

## Trap 16

Question:

> HashSet or HashMap for key-value data?

    HashMap

---

# 52. Practical Examples

## Example 1 — Basic HashSet

    import java.util.HashSet;
    import java.util.Set;

    public class Main {

        public static void main(String[] args) {

            Set<Integer> numbers =
                new HashSet<>();

            numbers.add(10);
            numbers.add(20);
            numbers.add(30);

            System.out.println(numbers);
        }
    }

---

## Example 2 — Duplicate Detection

    import java.util.*;

    public class Main {

        public static void main(String[] args) {

            Set<Integer> numbers =
                new HashSet<>();

            System.out.println(
                numbers.add(10)
            );

            System.out.println(
                numbers.add(20)
            );

            System.out.println(
                numbers.add(10)
            );
        }
    }

Output:

    true
    true
    false

---

## Example 3 — Remove Duplicates

    import java.util.*;

    public class Main {

        public static void main(String[] args) {

            List<Integer> numbers =
                List.of(
                    10,
                    20,
                    10,
                    30,
                    20
                );

            Set<Integer> unique =
                new HashSet<>(numbers);

            System.out.println(unique);
        }
    }

---

## Example 4 — contains()

    import java.util.*;

    public class Main {

        public static void main(String[] args) {

            Set<String> languages =
                new HashSet<>();

            languages.add("Java");
            languages.add("Python");

            if (languages.contains("Java")) {

                System.out.println(
                    "Java exists"
                );
            }
        }
    }

Output:

    Java exists

---

## Example 5 — Set Union

    import java.util.*;

    public class Main {

        public static void main(String[] args) {

            Set<Integer> A =
                new HashSet<>(
                    Set.of(1, 2, 3)
                );

            Set<Integer> B =
                new HashSet<>(
                    Set.of(3, 4, 5)
                );

            A.addAll(B);

            System.out.println(A);
        }
    }

Result:

    [1, 2, 3, 4, 5]

---

## Example 6 — Set Intersection

    import java.util.*;

    public class Main {

        public static void main(String[] args) {

            Set<Integer> A =
                new HashSet<>(
                    Set.of(1, 2, 3)
                );

            Set<Integer> B =
                new HashSet<>(
                    Set.of(3, 4, 5)
                );

            A.retainAll(B);

            System.out.println(A);
        }
    }

Output:

    [3]

---

## Example 7 — Set Difference

    import java.util.*;

    public class Main {

        public static void main(String[] args) {

            Set<Integer> A =
                new HashSet<>(
                    Set.of(1, 2, 3)
                );

            Set<Integer> B =
                new HashSet<>(
                    Set.of(3, 4, 5)
                );

            A.removeAll(B);

            System.out.println(A);
        }
    }

Output:

    [1, 2]

---

## Example 8 — Iterator

    import java.util.*;

    public class Main {

        public static void main(String[] args) {

            Set<Integer> numbers =
                new HashSet<>(
                    Set.of(10, 20, 30)
                );

            Iterator<Integer> iterator =
                numbers.iterator();

            while (iterator.hasNext()) {

                Integer number =
                    iterator.next();

                System.out.println(number);
            }
        }
    }

---

## Example 9 — Remove Using Iterator

    import java.util.*;

    public class Main {

        public static void main(String[] args) {

            Set<Integer> numbers =
                new HashSet<>(
                    Set.of(
                        10,
                        20,
                        30,
                        40
                    )
                );

            Iterator<Integer> iterator =
                numbers.iterator();

            while (iterator.hasNext()) {

                Integer number =
                    iterator.next();

                if (number > 20) {

                    iterator.remove();
                }
            }

            System.out.println(numbers);
        }
    }

---

## Example 10 — Custom Object

    import java.util.*;

    class Student {

        int id;
        String name;

        Student(
            int id,
            String name
        ) {
            this.id = id;
            this.name = name;
        }

        @Override
        public boolean equals(
            Object obj
        ) {

            if (this == obj) {
                return true;
            }

            if (!(obj instanceof Student)) {
                return false;
            }

            Student other =
                (Student) obj;

            return id == other.id
                && Objects.equals(
                    name,
                    other.name
                );
        }

        @Override
        public int hashCode() {

            return Objects.hash(
                id,
                name
            );
        }
    }

    public class Main {

        public static void main(String[] args) {

            Set<Student> students =
                new HashSet<>();

            students.add(
                new Student(
                    1,
                    "Rahul"
                )
            );

            students.add(
                new Student(
                    1,
                    "Rahul"
                )
            );

            System.out.println(
                students.size()
            );
        }
    }

Output:

    1

---

# 53. Top Interview Questions

## Q1. What is HashSet?

HashSet is a hash-table-based implementation of the Set interface that stores unique elements and provides average O(1) basic operations.

---

## Q2. Does HashSet allow duplicates?

No.

---

## Q3. Does HashSet allow null?

Yes, one null element.

---

## Q4. Does HashSet maintain insertion order?

No guaranteed iteration order.

---

## Q5. What is the average time complexity of HashSet.add()?

    O(1)

---

## Q6. What is the average complexity of contains()?

    O(1)

---

## Q7. What is the average complexity of remove()?

    O(1)

---

## Q8. How does HashSet identify duplicates?

Through hash-based lookup using:

    hashCode()
    +
    equals()

---

## Q9. Why are equals() and hashCode() both important?

`hashCode()` helps locate the appropriate bucket, while `equals()` determines whether two objects are logically equal.

---

## Q10. Can two different objects have the same hash code?

Yes.

This is a hash collision.

---

## Q11. Can two equal objects have different hash codes?

No, not under a correct implementation of the Java equality contract.

---

## Q12. What is HashSet internally backed by?

A HashMap.

---

## Q13. Why does HashSet use HashMap internally?

HashMap already provides efficient hash-based key storage.

HashSet can use elements as keys and associate them with a dummy value.

---

## Q14. HashSet vs LinkedHashSet?

HashSet provides no guaranteed iteration order.

LinkedHashSet maintains insertion order.

---

## Q15. HashSet vs TreeSet?

HashSet is hash-based and typically provides average O(1) basic operations.

TreeSet maintains sorted order and typically provides O(log n) basic operations.

---

## Q16. How can you remove duplicates from a List using HashSet?

    Set<Integer> unique =
        new HashSet<>(list);

---

## Q17. How do you preserve the original insertion order while removing duplicates?

Use:

    LinkedHashSet

Example:

    Set<Integer> unique =
        new LinkedHashSet<>(list);

---

## Q18. What is load factor?

Load factor determines how full the hash table can become before resizing.

Default HashSet construction uses:

    0.75

---

## Q19. What is the default initial capacity?

The underlying HashMap used by a default HashSet is created lazily; its default initial capacity is commonly 16 when the table is first initialized in current OpenJDK implementations.

Important:

Do not confuse this with the table's size before the first insertion.

---

## Q20. What happens when HashSet becomes sufficiently full?

Its underlying hash table is resized and entries are redistributed according to the new table capacity.

---

## Q21. What is a hash collision?

When different keys/elements produce the same hash location or hash-derived bucket index.

---

## Q22. Can HashSet store custom objects?

Yes.

But correct `equals()` and `hashCode()` implementations are important when logical equality is based on object state.

---

## Q23. What happens if hashCode() is overridden but equals() is not?

Objects that are logically equal according to the intended business definition may still be treated as different objects because equality semantics were not correctly defined.

---

## Q24. What happens if equals() is overridden but hashCode() is not?

This violates the Java contract and can cause HashSet to behave incorrectly for logically equal objects.

---

## Q25. Why should objects stored in HashSet generally be immutable?

If fields used by `equals()` or `hashCode()` change after insertion, lookup and removal can fail because the object's hash-based location no longer corresponds to its current state.

---

## Q26. Does HashSet use Comparable?

Not as its fundamental ordering mechanism.

HashSet is based on hashing, not sorted ordering.

---

## Q27. Which Set should be used for sorted unique elements?

    TreeSet

---

## Q28. Which Set should be used for insertion-order unique elements?

    LinkedHashSet

---

## Q29. Which Set should usually be used when order does not matter and fast average lookup is needed?

    HashSet

---

## Q30. Why is HashSet useful in DSA?

Because it provides efficient membership checking and makes it easy to maintain unique elements.

---

# 54. 30-Second Interview Answer

If the interviewer asks:

> "Explain HashSet."

Answer:

> "`HashSet` is a class in `java.util` that implements the `Set` interface and stores unique elements using hash-based storage. It does not guarantee iteration order and allows one `null` element. Its basic operations such as `add()`, `remove()`, and `contains()` are typically O(1) on average. HashSet is backed by a HashMap in the JDK, and for custom objects, correct `equals()` and `hashCode()` implementations are important for proper duplicate detection."

---

# 55. Cheat Sheet

## Definition

    HashSet
        -> Class
        -> Implements Set
        -> Hash-based
        -> Unique elements
        -> No guaranteed order

---

## Main Properties

    Duplicates
        -> Not allowed

    Null
        -> One null allowed

    Insertion order
        -> Not guaranteed

    Sorted order
        -> No

---

## Complexity

    add()
        -> O(1) average

    remove()
        -> O(1) average

    contains()
        -> O(1) average

    iteration
        -> O(n)

---

## Internal Concept

    HashSet
        |
        v
    HashMap
        |
        v
    Hash table
        |
        v
    Buckets

---

## Duplicate Detection

    element
        |
        v
    hashCode()
        |
        v
    bucket
        |
        v
    equals()
        |
      /   \
    equal  different
      |       |
    reject    add

---

## Related Implementations

    HashSet
        -> no guaranteed order

    LinkedHashSet
        -> insertion order

    TreeSet
        -> sorted order

---

## HashSet vs HashMap

    HashSet
        -> values/elements

    HashMap
        -> key-value pairs

Conceptually:

    HashSet element
          |
          v
       HashMap
          |
    key = element
    value = dummy object

---

## Important Concepts

    hashCode()
    equals()
    hash collision
    bucket
    capacity
    load factor
    resizing
    rehashing
    immutable objects

---

# 56. Quick Revision

Remember:

    HASHSET = UNIQUE + HASHING + FAST AVERAGE LOOKUP

Key facts:

    1. HashSet is a class.

    2. HashSet implements Set.

    3. HashSet does not allow duplicates.

    4. HashSet allows one null element.

    5. HashSet does not guarantee iteration order.

    6. HashSet is hash-based.

    7. HashSet is backed by a HashMap in the JDK.

    8. add() is typically O(1) average.

    9. remove() is typically O(1) average.

    10. contains() is typically O(1) average.

    11. add() returns false for an existing element.

    12. hashCode() helps locate a bucket.

    13. equals() helps determine logical equality.

    14. Same hashCode does not necessarily mean equal objects.

    15. Equal objects must have equal hash codes.

    16. Different objects can have the same hash code.

    17. Such a situation is called a hash collision.

    18. HashSet is not sorted.

    19. HashSet does not maintain insertion order.

    20. LinkedHashSet maintains insertion order.

    21. TreeSet maintains sorted order.

    22. Capacity is different from size.

    23. Load factor controls when resizing occurs.

    24. Default load factor is 0.75.

    25. Mutable hash-related fields can cause lookup problems.

    26. HashSet is very useful for duplicate detection.

    27. HashSet is very useful for membership checking.

    28. HashSet is heavily used in DSA.

---

# Final Memory Trick

Think:

    HASHSET
       |
       +---- HASHING
       |
       +---- UNIQUE
       |
       +---- FAST AVERAGE LOOKUP
       |
       +---- NO GUARANTEED ORDER
       |
       +---- ONE NULL

And remember the three major Set implementations:

    HashSet
        -> Unique + Fast

    LinkedHashSet
        -> Unique + Insertion Order

    TreeSet
        -> Unique + Sorted Order