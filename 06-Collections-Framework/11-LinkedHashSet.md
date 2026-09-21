# 11 — LinkedHashSet

> **Package:** `java.util`  
> **Type:** Class  
> **Extends:** `HashSet<E>`  
> **Implements:** `Set<E>`, `Cloneable`, `Serializable`  
> **Main property:** Unique elements + insertion-order iteration  
> **Internal concept:** Hash table + doubly linked list

---

# Table of Contents

1. Introduction
2. What is LinkedHashSet
3. LinkedHashSet Hierarchy
4. Why LinkedHashSet Exists
5. Key Characteristics
6. Creating LinkedHashSet
7. Adding Elements
8. Duplicate Elements
9. Insertion Order
10. Internal Working
11. Hash Table + Linked List
12. How Duplicates Are Detected
13. hashCode() and equals()
14. Hash Collision
15. null in LinkedHashSet
16. LinkedHashSet Methods
17. add()
18. addAll()
19. remove()
20. removeAll()
21. retainAll()
22. contains()
23. containsAll()
24. removeIf()
25. clear()
26. isEmpty()
27. size()
28. iterator()
29. forEach()
30. toArray()
31. LinkedHashSet with Custom Objects
32. equals() and hashCode() Contract
33. Mutable Object Trap
34. Capacity
35. Load Factor
36. Resizing
37. Time Complexity
38. Memory Usage
39. LinkedHashSet vs HashSet
40. LinkedHashSet vs TreeSet
41. LinkedHashSet vs ArrayList
42. LinkedHashSet vs LinkedList
43. When to Use LinkedHashSet
44. When Not to Use LinkedHashSet
45. Advantages
46. Disadvantages
47. Common Mistakes
48. Interview Traps
49. Practical Examples
50. Top Interview Questions
51. 30-Second Interview Answer
52. Cheat Sheet
53. Quick Revision

---

# 1. Introduction

`LinkedHashSet` is a class from the `java.util` package.

It is a specialized implementation of the `Set` interface that combines:

    HashSet
        +
    insertion-order maintenance

It provides:

    Unique elements
    Fast average lookup
    Fast average insertion
    Fast average removal
    Predictable insertion-order iteration
    One null element

Example:

    Set<String> languages =
        new LinkedHashSet<>();

    languages.add("Java");
    languages.add("Python");
    languages.add("C");

Iteration follows:

    Java
    Python
    C

The important difference from `HashSet` is:

> LinkedHashSet maintains insertion order during iteration.

---

# 2. What is LinkedHashSet?

`LinkedHashSet` is a hash-table and linked-list implementation of the `Set` interface.

Declaration:

    public class LinkedHashSet<E>
        extends HashSet<E>
        implements Set<E>, Cloneable, Serializable

It inherits most Set operations from `HashSet`, while maintaining a linked structure that preserves insertion order.

Example:

    Set<Integer> numbers =
        new LinkedHashSet<>();

    numbers.add(30);
    numbers.add(10);
    numbers.add(20);

Iteration:

    30
    10
    20

The order in which elements were successfully inserted is preserved.

---

# 3. LinkedHashSet Hierarchy

The hierarchy is:

    Object
       |
    AbstractCollection
       |
    AbstractSet
       |
    HashSet
       |
    LinkedHashSet

Interfaces:

    Iterable
       |
    Collection
       |
    Set
       |
    HashSet
       |
    LinkedHashSet

Important:

    HashSet
        -> Parent class

    LinkedHashSet
        -> Child class

    Set
        -> Interface

---

# 4. Why LinkedHashSet Exists

Suppose you want:

    Unique elements

and also:

    Insertion order

A normal `HashSet` gives uniqueness but does not guarantee iteration order.

Example:

    Set<String> set =
        new HashSet<>();

    set.add("A");
    set.add("B");
    set.add("C");

You cannot depend on:

    A
    B
    C

If you use `LinkedHashSet`:

    Set<String> set =
        new LinkedHashSet<>();

    set.add("A");
    set.add("B");
    set.add("C");

Iteration is:

    A
    B
    C

So:

    HashSet
        -> Unique

    LinkedHashSet
        -> Unique + Insertion Order

---

# 5. Key Characteristics

## 5.1 No Duplicates

LinkedHashSet follows the Set contract.

Therefore:

    duplicates
        -> not allowed

Example:

    Set<Integer> numbers =
        new LinkedHashSet<>();

    numbers.add(10);
    numbers.add(20);
    numbers.add(10);

Contents:

    10
    20

---

## 5.2 Maintains Insertion Order

This is the defining feature.

Example:

    set.add(50);
    set.add(10);
    set.add(30);

Iteration:

    50
    10
    30

---

## 5.3 Allows One null

Example:

    Set<String> names =
        new LinkedHashSet<>();

    names.add(null);
    names.add(null);

Only one `null` is stored.

---

## 5.4 Average O(1) Basic Operations

Typical average complexity:

    add()
        -> O(1)

    remove()
        -> O(1)

    contains()
        -> O(1)

The linked structure does not turn these basic hash-based operations into O(n).

---

## 5.5 Predictable Iteration

Unlike `HashSet`, iteration order is predictable based on insertion order.

This is useful when output should remain stable.

---

# 6. Creating LinkedHashSet

## Basic

    LinkedHashSet<Integer> numbers =
        new LinkedHashSet<>();

---

## Using Set Reference

Preferred when you only need Set behavior:

    Set<Integer> numbers =
        new LinkedHashSet<>();

---

## With Initial Capacity

    LinkedHashSet<Integer> numbers =
        new LinkedHashSet<>(100);

---

## With Capacity and Load Factor

    LinkedHashSet<Integer> numbers =
        new LinkedHashSet<>(
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
        new LinkedHashSet<>(numbers);

Result:

    10
    20
    30

The first occurrence of each element determines its position.

---

# 7. Adding Elements

Use:

    add()

Example:

    Set<String> languages =
        new LinkedHashSet<>();

    languages.add("Java");
    languages.add("Python");
    languages.add("C");

Iteration:

    Java
    Python
    C

---

## Return Value

`add()` returns:

    true
        -> element was newly added

    false
        -> element already existed

Example:

    Set<Integer> numbers =
        new LinkedHashSet<>();

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
        new LinkedHashSet<>();

    numbers.add(10);
    numbers.add(20);
    numbers.add(10);
    numbers.add(30);
    numbers.add(20);

Final iteration order:

    10
    20
    30

The duplicate elements are ignored.

Important:

> Adding a duplicate does not create a second occurrence.

---

# 9. Insertion Order

Insertion order means:

> The order in which elements are successfully inserted is the order used for iteration.

Example:

    Set<String> names =
        new LinkedHashSet<>();

    names.add("Rahul");
    names.add("Aman");
    names.add("Vikas");

Iteration:

    Rahul
    Aman
    Vikas

---

## Duplicate Does Not Change Position

Example:

    Set<String> names =
        new LinkedHashSet<>();

    names.add("A");
    names.add("B");
    names.add("C");
    names.add("A");

Result:

    A
    B
    C

The second `A` is ignored.

It does not move `A` to the end.

---

# 10. Internal Working

`LinkedHashSet` is based on the same general hashing mechanism as `HashSet`, but additionally maintains a linked structure to preserve insertion order.

Conceptually:

    LinkedHashSet
          |
          +----------------+
          |                |
          v                v
    Hash table       Linked structure
          |                |
          v                v
    Fast lookup       Insertion order

This gives:

    Hashing
       +
    Ordering

---

# 11. Hash Table + Linked List

The most important internal concept is:

    Hash table
        +
    Doubly linked list

Conceptually:

    Hash Table

    Bucket 0
       |
       v
      A

    Bucket 1
       |
       v
      C

    Bucket 2
       |
       v
      B


    Linked order:

    A <-> B <-> C

The hash table helps with:

    add()
    contains()
    remove()

The linked structure maintains:

    insertion order

---

# 12. How Duplicates Are Detected

Suppose:

    set.add("Java");

The hash-based mechanism conceptually performs:

    "Java"
       |
       v
    hashCode()
       |
       v
    bucket calculation
       |
       v
    check existing entries
       |
       v
    equals()
       |
    +--+--+
    |     |
   true  false
    |     |
 duplicate add

If an equal element already exists:

    add()
        -> false

If no equal element exists:

    element is inserted

and its linked-order position is maintained.

---

# 13. hashCode() and equals()

For custom objects, correct:

    hashCode()
    equals()

implementations are important.

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

---

# 14. Hash Collision

Different objects can have the same hash code.

Example:

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

This is a:

    Hash Collision

The same hash code does not mean the objects are equal.

The equality check is still required.

---

# 15. null in LinkedHashSet

LinkedHashSet allows one `null`.

Example:

    Set<String> set =
        new LinkedHashSet<>();

    set.add(null);
    set.add("Java");
    set.add(null);
    set.add("Python");

Iteration contains:

    null
    Java
    Python

The first successful insertion of `null` determines its position.

---

# 16. LinkedHashSet Methods

Since `LinkedHashSet` extends `HashSet`, it inherits Set operations.

Important methods:

    add()
    addAll()

    remove()
    removeAll()

    retainAll()

    contains()
    containsAll()

    removeIf()

    clear()

    isEmpty()
    size()

    iterator()

    forEach()

    toArray()

    stream()

    parallelStream()

    spliterator()

---

# 17. add()

Adds an element if it does not already exist.

Syntax:

    boolean add(E e)

Example:

    Set<Integer> set =
        new LinkedHashSet<>();

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

# 18. addAll()

Adds all elements from another Collection.

Example:

    Set<Integer> A =
        new LinkedHashSet<>(
            Set.of(1, 2, 3)
        );

    Set<Integer> B =
        new LinkedHashSet<>(
            Set.of(3, 4, 5)
        );

    A.addAll(B);

Result:

    1
    2
    3
    4
    5

Important:

Existing elements are not duplicated.

New elements are appended according to the iteration order of the supplied Collection.

---

# 19. remove()

Removes a specific element.

Example:

    Set<String> languages =
        new LinkedHashSet<>();

    languages.add("Java");
    languages.add("Python");
    languages.add("C");

    languages.remove("Python");

Iteration:

    Java
    C

---

# 20. removeAll()

Removes elements that are also present in another Collection.

Example:

    Set<Integer> A =
        new LinkedHashSet<>(
            Set.of(1, 2, 3, 4)
        );

    Set<Integer> B =
        new LinkedHashSet<>(
            Set.of(3, 4, 5)
        );

    A.removeAll(B);

Result:

    1
    2

---

# 21. retainAll()

Keeps only elements that are present in another Collection.

Example:

    Set<Integer> A =
        new LinkedHashSet<>(
            Set.of(1, 2, 3, 4)
        );

    Set<Integer> B =
        new LinkedHashSet<>(
            Set.of(3, 4, 5)
        );

    A.retainAll(B);

Result:

    3
    4

---

# 22. contains()

Checks whether an element exists.

Example:

    Set<String> languages =
        new LinkedHashSet<>();

    languages.add("Java");
    languages.add("Python");

    System.out.println(
        languages.contains("Java")
    );

Output:

    true

Typical average complexity:

    O(1)

---

# 23. containsAll()

Checks whether all elements of another Collection exist.

Example:

    Set<Integer> A =
        new LinkedHashSet<>(
            Set.of(10, 20, 30)
        );

    Set<Integer> B =
        new LinkedHashSet<>(
            Set.of(10, 20)
        );

    System.out.println(
        A.containsAll(B)
    );

Output:

    true

---

# 24. removeIf()

Removes elements matching a condition.

Example:

    Set<Integer> numbers =
        new LinkedHashSet<>(
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

    10
    20

The remaining elements retain their relative insertion order.

---

# 25. clear()

Removes all elements.

Example:

    Set<String> languages =
        new LinkedHashSet<>();

    languages.add("Java");
    languages.add("Python");

    languages.clear();

    System.out.println(languages);

Output:

    []

---

# 26. isEmpty()

Checks whether the Set contains no elements.

Example:

    Set<Integer> numbers =
        new LinkedHashSet<>();

    System.out.println(
        numbers.isEmpty()
    );

Output:

    true

---

# 27. size()

Returns the number of elements.

Example:

    Set<Integer> numbers =
        new LinkedHashSet<>();

    numbers.add(10);
    numbers.add(20);
    numbers.add(30);

    System.out.println(
        numbers.size()
    );

Output:

    3

---

# 28. iterator()

Returns an Iterator.

The iterator follows insertion order.

Example:

    Set<String> languages =
        new LinkedHashSet<>();

    languages.add("Java");
    languages.add("Spring");
    languages.add("React");

    Iterator<String> iterator =
        languages.iterator();

    while (iterator.hasNext()) {

        System.out.println(
            iterator.next()
        );
    }

Output:

    Java
    Spring
    React

---

## Removing Using Iterator

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

# 29. forEach()

Can be used to iterate through the Set.

Example:

    Set<String> languages =
        new LinkedHashSet<>();

    languages.add("Java");
    languages.add("Python");
    languages.add("C");

    languages.forEach(
        language ->
            System.out.println(language)
    );

Output:

    Java
    Python
    C

---

# 30. toArray()

Converts the Set into an array.

Example:

    Set<String> languages =
        new LinkedHashSet<>();

    languages.add("Java");
    languages.add("Python");

    Object[] array =
        languages.toArray();

---

## Typed Array

    String[] array =
        languages.toArray(
            new String[0]
        );

The array follows the Set's iteration order.

---

# 31. LinkedHashSet with Custom Objects

Example:

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

        @Override
        public String toString() {

            return id + " - " + name;
        }
    }

    public class Main {

        public static void main(
            String[] args
        ) {

            Set<Student> students =
                new LinkedHashSet<>();

            students.add(
                new Student(
                    1,
                    "Rahul"
                )
            );

            students.add(
                new Student(
                    2,
                    "Aman"
                )
            );

            students.add(
                new Student(
                    1,
                    "Rahul"
                )
            );

            for (Student student : students) {

                System.out.println(student);
            }
        }
    }

Output:

    1 - Rahul
    2 - Aman

The duplicate Rahul is not stored.

---

# 32. equals() and hashCode() Contract

The fundamental contract is:

If:

    a.equals(b)

returns:

    true

then:

    a.hashCode()
        ==
    b.hashCode()

must be true.

For a Set:

    hashCode()
        -> helps locate candidate bucket

    equals()
        -> confirms logical equality

Therefore both methods must be consistent.

---

# 33. Mutable Object Trap

Consider an object whose hash code depends on:

    id

Suppose:

    Student id = 10

is inserted into LinkedHashSet.

Later:

    id = 20

If `hashCode()` changes, hash-based lookup can become problematic.

The object may still exist inside the Set, but:

    contains(object)

or:

    remove(object)

may not behave as expected.

Therefore:

> Avoid changing fields that participate in `equals()` and `hashCode()` while the object is stored in a hash-based Set.

---

# 34. Capacity

Capacity represents the number of buckets available in the underlying hash table.

Example:

    LinkedHashSet<Integer> set =
        new LinkedHashSet<>(100);

This supplies an initial-capacity hint.

Important:

    capacity
        !=
    size

Example:

    capacity = 100

does not mean:

    size = 100

You may have:

    capacity = 100
    size = 10

---

# 35. Load Factor

Load factor determines how full the hash table can become before resizing.

The default load factor used by the standard HashSet-based implementation is:

    0.75

Example conceptually:

    capacity = 16
    loadFactor = 0.75

Threshold:

    16 × 0.75
        = 12

When the table reaches the resizing threshold, the underlying table can be resized.

---

# 36. Resizing

When the hash table becomes sufficiently full, the underlying table is resized.

Conceptually:

    Small table
        |
        v
    [buckets]
        |
    threshold reached
        |
        v
    resize
        |
        v
    larger table
        |
        v
    redistribute entries

The linked insertion-order structure continues to represent the iteration order.

Important:

> Resizing does not mean that the logical insertion order is lost.

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

The linked structure adds ordering support but does not fundamentally change the average complexity of basic hash-based operations.

---

# 38. Memory Usage

Compared with HashSet, LinkedHashSet generally uses more memory.

Why?

Because it maintains additional links between entries to preserve insertion order.

Conceptually:

    HashSet

    Hash table
        +
    entries


    LinkedHashSet

    Hash table
        +
    entries
        +
    linked-order information

Therefore:

    LinkedHashSet
        -> more memory

    HashSet
        -> generally less memory

The trade-off is:

    predictable insertion-order iteration

---

# 39. LinkedHashSet vs HashSet

| Feature | HashSet | LinkedHashSet |
|---|---|---|
| Duplicates | No | No |
| Hash-based | Yes | Yes |
| Insertion order | Not guaranteed | Maintained |
| Average add | O(1) | O(1) |
| Average contains | O(1) | O(1) |
| Average remove | O(1) | O(1) |
| Memory | Generally lower | Generally higher |
| Null | One allowed | One allowed |
| Main advantage | Fast unique storage | Fast unique storage + order |

Memory trick:

    HashSet
        -> Unique + Fast

    LinkedHashSet
        -> Unique + Fast + Order

---

# 40. LinkedHashSet vs TreeSet

| Feature | LinkedHashSet | TreeSet |
|---|---|---|
| Duplicates | No | No |
| Order | Insertion order | Sorted order |
| Internal concept | Hash table + linked structure | Tree |
| Average add | O(1) | O(log n) |
| Average contains | O(1) | O(log n) |
| Navigation methods | No | Yes |
| Null | One null allowed | Normally no null with natural ordering |
| Main purpose | Unique + insertion order | Unique + sorted order |

Choose based on required order:

    Insertion order
        -> LinkedHashSet

    Sorted order
        -> TreeSet

---

# 41. LinkedHashSet vs ArrayList

| Feature | LinkedHashSet | ArrayList |
|---|---|---|
| Duplicates | No | Yes |
| Index access | No | Yes |
| Insertion order | Yes | Yes |
| Average contains | O(1) | O(n) |
| add at end | O(1) average | O(1) amortized |
| Main purpose | Unique ordered elements | Indexed sequence |

Example:

If you need:

    A
    B
    A
    C

and want to keep duplicates:

    ArrayList

If you need:

    A
    B
    C

while preserving first insertion order:

    LinkedHashSet

---

# 42. LinkedHashSet vs LinkedList

The names can be confusing.

`LinkedHashSet` and `LinkedList` are fundamentally different.

| Feature | LinkedHashSet | LinkedList |
|---|---|---|
| Type | Set | List |
| Duplicates | No | Yes |
| Index access | No | Yes |
| Insertion order | Yes | Yes |
| Hashing | Yes | No |
| Main purpose | Unique elements | Sequence/list |

Important:

> "Linked" does not mean both classes solve the same problem.

`LinkedHashSet` uses linked information to maintain Set iteration order.

`LinkedList` is a List implementation based on linked nodes.

---

# 43. When to Use LinkedHashSet

Use LinkedHashSet when you need:

## 1. Unique Elements

    Set<String> names =
        new LinkedHashSet<>();

---

## 2. Insertion Order

    names.add("A");
    names.add("B");
    names.add("C");

Iteration:

    A
    B
    C

---

## 3. Remove Duplicates While Preserving Order

Example:

    List<Integer> numbers =
        List.of(
            10,
            20,
            10,
            30,
            20,
            40
        );

    Set<Integer> unique =
        new LinkedHashSet<>(numbers);

Result:

    10
    20
    30
    40

---

## 4. Stable Output

When you want unique data to appear consistently in insertion order.

---

## 5. Ordered Visited Elements

For some algorithms, you may want:

    uniqueness
        +
    order of first discovery

LinkedHashSet can be useful for that requirement.

---

# 44. When Not to Use LinkedHashSet

## 1. You Need Index Access

Use:

    ArrayList

---

## 2. You Need Duplicates

Use:

    List

---

## 3. You Need Sorted Order

Use:

    TreeSet

---

## 4. You Do Not Care About Order

If insertion order provides no value, `HashSet` may be sufficient and generally has lower per-entry overhead.

---

## 5. You Need Key-Value Pairs

Use:

    LinkedHashMap

---

# 45. Advantages

## 1. No Duplicate Elements

Set semantics automatically prevent duplicates.

---

## 2. Maintains Insertion Order

Iteration follows successful insertion order.

---

## 3. Fast Average Lookup

Typical:

    contains()
        -> O(1)

---

## 4. Fast Average Insertion

Typical:

    add()
        -> O(1)

---

## 5. Fast Average Removal

Typical:

    remove()
        -> O(1)

---

## 6. Easy Duplicate Removal

It can remove duplicates while preserving first-occurrence order.

---

# 46. Disadvantages

## 1. More Memory Than HashSet

The linked structure requires additional memory.

---

## 2. No Index Access

There is no:

    get(index)

---

## 3. Not Sorted

Insertion order is not the same as sorted order.

---

## 4. Hashing Requirements

Custom objects still require correct:

    equals()
    hashCode()

---

## 5. Basic Operations Are Not Guaranteed O(1)

The commonly quoted O(1) is average-case behavior.

---

# 47. Common Mistakes

## Mistake 1 — Thinking LinkedHashSet Is Sorted

Wrong:

    LinkedHashSet
        -> sorted

Correct:

    LinkedHashSet
        -> insertion order

---

## Mistake 2 — Thinking It Allows Duplicates

Wrong:

    LinkedHashSet
        -> duplicates allowed

Correct:

    LinkedHashSet
        -> duplicates not allowed

---

## Mistake 3 — Thinking Duplicate Moves to the End

Example:

    add(A)
    add(B)
    add(C)
    add(A)

Result:

    A
    B
    C

The second `A` does not move it.

---

## Mistake 4 — Confusing LinkedHashSet and LinkedList

They are completely different Collection types.

---

## Mistake 5 — Assuming HashSet and LinkedHashSet Have the Same Ordering

They do not.

    HashSet
        -> no guaranteed order

    LinkedHashSet
        -> insertion order

---

## Mistake 6 — Forgetting equals() and hashCode()

Custom objects need proper equality semantics for expected duplicate behavior.

---

## Mistake 7 — Assuming O(1) Is an Absolute Guarantee

Say:

    average O(1)

rather than:

    always O(1)

---

## Mistake 8 — Assuming Insertion Order Means Sorted Order

Example:

    add(50)
    add(10)
    add(30)

LinkedHashSet:

    50
    10
    30

TreeSet:

    10
    30
    50

---

# 48. Interview Traps

## Trap 1

Question:

> What is LinkedHashSet?

Answer:

A Set implementation that combines hash-based storage with insertion-order maintenance.

---

## Trap 2

Question:

> Does LinkedHashSet allow duplicates?

Answer:

No.

---

## Trap 3

Question:

> Does LinkedHashSet maintain insertion order?

Answer:

Yes, its iteration order is the insertion order.

---

## Trap 4

Question:

> Does LinkedHashSet allow null?

Answer:

Yes, one null element.

---

## Trap 5

Question:

> Is LinkedHashSet sorted?

Answer:

No.

It maintains insertion order, not sorted order.

---

## Trap 6

Question:

> What is the average complexity of contains()?

Answer:

    O(1)

---

## Trap 7

Question:

> Which is faster, HashSet or LinkedHashSet?

There is no universal answer for every workload.

HashSet avoids the extra linked-order structure, while LinkedHashSet provides predictable insertion-order iteration.

---

## Trap 8

Question:

> Why does LinkedHashSet use more memory than HashSet?

Because it maintains additional linked-order information.

---

## Trap 9

Question:

> What happens if you add a duplicate?

Answer:

Nothing is added and `add()` returns `false`.

---

## Trap 10

Question:

> Does adding an existing element change its position?

Answer:

No.

---

## Trap 11

Question:

> LinkedHashSet or TreeSet for sorted unique data?

    TreeSet

---

## Trap 12

Question:

> LinkedHashSet or HashSet when insertion order matters?

    LinkedHashSet

---

## Trap 13

Question:

> LinkedHashSet or ArrayList when duplicates must be removed while preserving insertion order?

    LinkedHashSet

---

## Trap 14

Question:

> What determines whether two custom objects are duplicates?

The Set relies on equality/hash semantics, particularly `hashCode()` and `equals()`.

---

## Trap 15

Question:

> Can two different objects have the same hash code?

Yes.

That is a hash collision.

---

# 49. Practical Examples

## Example 1 — Basic LinkedHashSet

    import java.util.*;

    public class Main {

        public static void main(
            String[] args
        ) {

            Set<String> languages =
                new LinkedHashSet<>();

            languages.add("Java");
            languages.add("Python");
            languages.add("C");

            System.out.println(languages);
        }
    }

Output:

    [Java, Python, C]

---

## Example 2 — Duplicate Elements

    import java.util.*;

    public class Main {

        public static void main(
            String[] args
        ) {

            Set<Integer> numbers =
                new LinkedHashSet<>();

            System.out.println(
                numbers.add(10)
            );

            System.out.println(
                numbers.add(20)
            );

            System.out.println(
                numbers.add(10)
            );

            System.out.println(numbers);
        }
    }

Output:

    true
    true
    false
    [10, 20]

---

## Example 3 — Preserving Order While Removing Duplicates

    import java.util.*;

    public class Main {

        public static void main(
            String[] args
        ) {

            List<String> names =
                List.of(
                    "A",
                    "B",
                    "A",
                    "C",
                    "B",
                    "D"
                );

            Set<String> unique =
                new LinkedHashSet<>(
                    names
                );

            System.out.println(unique);
        }
    }

Output:

    [A, B, C, D]

---

## Example 4 — null

    import java.util.*;

    public class Main {

        public static void main(
            String[] args
        ) {

            Set<String> set =
                new LinkedHashSet<>();

            set.add(null);
            set.add("Java");
            set.add(null);
            set.add("Python");

            System.out.println(set);
        }
    }

Output:

    [null, Java, Python]

---

## Example 5 — Iterator

    import java.util.*;

    public class Main {

        public static void main(
            String[] args
        ) {

            Set<Integer> numbers =
                new LinkedHashSet<>();

            numbers.add(30);
            numbers.add(10);
            numbers.add(20);

            Iterator<Integer> iterator =
                numbers.iterator();

            while (iterator.hasNext()) {

                System.out.println(
                    iterator.next()
                );
            }
        }
    }

Output:

    30
    10
    20

---

## Example 6 — removeIf()

    import java.util.*;

    public class Main {

        public static void main(
            String[] args
        ) {

            Set<Integer> numbers =
                new LinkedHashSet<>();

            numbers.add(10);
            numbers.add(15);
            numbers.add(20);
            numbers.add(25);

            numbers.removeIf(
                n -> n % 2 != 0
            );

            System.out.println(numbers);
        }
    }

Output:

    [10, 20]

---

## Example 7 — Set Union

    import java.util.*;

    public class Main {

        public static void main(
            String[] args
        ) {

            Set<Integer> first =
                new LinkedHashSet<>();

            first.add(1);
            first.add(2);
            first.add(3);

            Set<Integer> second =
                new LinkedHashSet<>();

            second.add(3);
            second.add(4);
            second.add(5);

            first.addAll(second);

            System.out.println(first);
        }
    }

Output:

    [1, 2, 3, 4, 5]

---

## Example 8 — Set Intersection

    import java.util.*;

    public class Main {

        public static void main(
            String[] args
        ) {

            Set<Integer> first =
                new LinkedHashSet<>(
                    Set.of(1, 2, 3, 4)
                );

            Set<Integer> second =
                new LinkedHashSet<>(
                    Set.of(3, 4, 5)
                );

            first.retainAll(second);

            System.out.println(first);
        }
    }

Output:

    [3, 4]

---

## Example 9 — Set Difference

    import java.util.*;

    public class Main {

        public static void main(
            String[] args
        ) {

            Set<Integer> first =
                new LinkedHashSet<>(
                    Set.of(1, 2, 3, 4)
                );

            Set<Integer> second =
                new LinkedHashSet<>(
                    Set.of(3, 4, 5)
                );

            first.removeAll(second);

            System.out.println(first);
        }
    }

Output:

    [1, 2]

---

## Example 10 — Custom Objects

    import java.util.*;

    class Employee {

        int id;
        String name;

        Employee(
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

            if (!(obj instanceof Employee)) {
                return false;
            }

            Employee other =
                (Employee) obj;

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

        @Override
        public String toString() {

            return id + " - " + name;
        }
    }

    public class Main {

        public static void main(
            String[] args
        ) {

            Set<Employee> employees =
                new LinkedHashSet<>();

            employees.add(
                new Employee(
                    101,
                    "Rahul"
                )
            );

            employees.add(
                new Employee(
                    102,
                    "Aman"
                )
            );

            employees.add(
                new Employee(
                    101,
                    "Rahul"
                )
            );

            for (Employee employee :
                 employees) {

                System.out.println(employee);
            }
        }
    }

Output:

    101 - Rahul
    102 - Aman

---

# 50. Top Interview Questions

## Q1. What is LinkedHashSet?

`LinkedHashSet` is a Set implementation that stores unique elements using hash-based storage while maintaining insertion order for iteration.

---

## Q2. What is the difference between HashSet and LinkedHashSet?

The major difference is ordering:

    HashSet
        -> no guaranteed iteration order

    LinkedHashSet
        -> insertion-order iteration

Both provide hash-based average O(1) basic operations.

---

## Q3. Does LinkedHashSet allow duplicates?

No.

---

## Q4. Does LinkedHashSet allow null?

Yes, one null element.

---

## Q5. Is LinkedHashSet sorted?

No.

It preserves insertion order.

---

## Q6. What is the average complexity of add()?

    O(1)

---

## Q7. What is the average complexity of contains()?

    O(1)

---

## Q8. What is the average complexity of remove()?

    O(1)

---

## Q9. Why does LinkedHashSet maintain insertion order?

It maintains additional linked-order information for its entries.

---

## Q10. What is the internal concept behind LinkedHashSet?

    Hash table
        +
    doubly linked ordering structure

---

## Q11. Why does LinkedHashSet consume more memory than HashSet?

Because it stores additional links required to maintain insertion order.

---

## Q12. Does adding a duplicate move it to the end?

No.

The existing element keeps its original position.

---

## Q13. How does LinkedHashSet identify duplicates?

Through hash-based lookup and equality comparison using `hashCode()` and `equals()`.

---

## Q14. What happens if two objects have the same hash code?

They can be placed in the same hash bucket.

The equality check determines whether they are actually equal.

---

## Q15. Can two different objects have the same hash code?

Yes.

---

## Q16. Can equal objects have different hash codes?

No, assuming a correct implementation of the Java contract.

---

## Q17. Why are equals() and hashCode() important for custom objects?

They determine logical equality and therefore whether an object should be considered a duplicate.

---

## Q18. What happens if an object changes after insertion?

If fields used by `equals()` or `hashCode()` change, hash-based lookup/removal can behave unexpectedly.

---

## Q19. LinkedHashSet or TreeSet?

Use:

    LinkedHashSet
        -> insertion order

    TreeSet
        -> sorted order

---

## Q20. LinkedHashSet or HashSet?

Use:

    HashSet
        -> when order does not matter

    LinkedHashSet
        -> when insertion order matters

---

## Q21. LinkedHashSet or ArrayList for removing duplicates while preserving order?

    LinkedHashSet

---

## Q22. Does LinkedHashSet provide get(index)?

No.

---

## Q23. Can LinkedHashSet contain null and maintain its position?

Yes.

One null is allowed, and its position follows its successful insertion position.

---

## Q24. Does LinkedHashSet guarantee sorted output?

No.

It guarantees insertion-order iteration, not sorting.

---

## Q25. What happens to order after removing an element?

The removed element disappears.

The remaining elements retain their relative insertion order.

---

## Q26. What happens when a new element is added after a removal?

It is inserted at the end of the current insertion order.

Example:

    add(A)
    add(B)
    add(C)

    remove(B)

    add(D)

Order:

    A
    C
    D

---

## Q27. Does re-adding an existing element move it to the end?

No.

Example:

    add(A)
    add(B)
    add(C)
    add(A)

Order:

    A
    B
    C

---

## Q28. What is the default load factor?

    0.75

---

## Q29. Is iteration order guaranteed in LinkedHashSet?

Yes, iteration follows insertion order.

---

## Q30. What is the main advantage of LinkedHashSet?

It provides:

    uniqueness
        +
    hash-based average O(1) operations
        +
    predictable insertion-order iteration

---

# 51. 30-Second Interview Answer

If the interviewer asks:

> "Explain LinkedHashSet."

Answer:

> "`LinkedHashSet` is a class in `java.util` that implements the `Set` contract and extends `HashSet`. It stores unique elements using hash-based storage and additionally maintains insertion order for iteration. Its basic operations such as `add()`, `remove()`, and `contains()` are typically O(1) on average. It allows one `null` element. Compared with `HashSet`, its main advantage is predictable insertion-order iteration, although it requires additional memory for maintaining that order."

---

# 52. Cheat Sheet

## Definition

    LinkedHashSet
        -> Class
        -> Set implementation
        -> Unique elements
        -> Hash-based
        -> Insertion-order iteration

---

## Main Properties

    Duplicates
        -> Not allowed

    Null
        -> One null allowed

    Insertion order
        -> Maintained

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

    LinkedHashSet
         |
         +------------------+
         |                  |
         v                  v
    Hash table       Linked ordering
         |                  |
         v                  v
    Fast lookup       Insertion order

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

## Three Important Set Implementations

    HashSet
        -> Unique
        -> No guaranteed order
        -> Fast average operations

    LinkedHashSet
        -> Unique
        -> Insertion order
        -> Fast average operations

    TreeSet
        -> Unique
        -> Sorted order
        -> O(log n) basic operations

---

## Memory Trick

    HASHSET

    Unique
    + Fast
    + No guaranteed order


    LINKEDHASHSET

    Unique
    + Fast
    + Insertion Order


    TREESET

    Unique
    + Sorted
    + Navigation

---

# 53. Quick Revision

Remember:

    1. LinkedHashSet is a class.

    2. It belongs to java.util.

    3. It follows the Set contract.

    4. It does not allow duplicates.

    5. It allows one null element.

    6. It maintains insertion order during iteration.

    7. It is not sorted.

    8. Its basic operations are typically O(1) average.

    9. It uses hash-based storage.

    10. It maintains additional linked-order information.

    11. It generally uses more memory than HashSet.

    12. HashSet does not guarantee iteration order.

    13. LinkedHashSet preserves insertion order.

    14. TreeSet maintains sorted order.

    15. Adding a duplicate returns false.

    16. Adding a duplicate does not move the element.

    17. Removing an element does not disturb the relative order of remaining elements.

    18. A newly added element goes to the end of the current insertion order.

    19. hashCode() helps hash-based lookup.

    20. equals() determines logical equality.

    21. Equal objects must have equal hash codes.

    22. Different objects can have the same hash code.

    23. Same hash code does not mean equal objects.

    24. Mutable hash-related fields can cause lookup problems.

    25. Capacity is different from size.

    26. Default load factor is 0.75.

    27. LinkedHashSet is useful for removing duplicates while preserving order.

    28. LinkedHashSet does not provide index-based access.

    29. Use TreeSet when sorted order is required.

    30. Use HashSet when insertion order is irrelevant.

---

# Final Memory Trick

Think of LinkedHashSet as:

    HASHSET
       +
    LINKED ORDER

Therefore:

    LinkedHashSet
        =
    UNIQUE
        +
    FAST AVERAGE HASH OPERATIONS
        +
    INSERTION ORDER

The three Set implementations:

    HashSet
        -> Unique + Fast

    LinkedHashSet
        -> Unique + Fast + Insertion Order

    TreeSet
        -> Unique + Sorted Order