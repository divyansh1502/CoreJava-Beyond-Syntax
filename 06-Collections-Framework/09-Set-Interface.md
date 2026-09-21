# 09 — Set Interface

> **Package:** `java.util`  
> **Type:** Interface  
> **Extends:** `Collection<E>`  
> **Primary Property:** Does not allow duplicate elements  
> **Common Implementations:** `HashSet`, `LinkedHashSet`, `TreeSet`

---

# Table of Contents

1. Introduction
2. What is Set
3. Set Hierarchy
4. Why Set Exists
5. Key Characteristics
6. Set vs Collection
7. Set vs List
8. Creating a Set
9. Adding Elements
10. Duplicate Elements
11. Null Elements
12. Removing Elements
13. Searching Elements
14. Size and Empty Checks
15. Iterating a Set
16. Set Interface Methods
17. Bulk Operations
18. Mathematical Set Operations
19. containsAll()
20. addAll()
21. retainAll()
22. removeAll()
23. removeIf()
24. clear()
25. Set and equals()
26. Set and hashCode()
27. How Set Prevents Duplicates
28. HashSet
29. LinkedHashSet
30. TreeSet
31. SortedSet
32. NavigableSet
33. Set Ordering
34. Set Implementations Comparison
35. Set and null
36. Set Performance
37. Set Use Cases
38. Set in DSA
39. Advantages
40. Disadvantages
41. Common Mistakes
42. Interview Traps
43. Practical Examples
44. Top Interview Questions
45. 30-Second Interview Answer
46. Cheat Sheet
47. Quick Revision

---

# 1. Introduction

`Set` is an interface in the Java Collections Framework.

Package:

    java.util

Declaration conceptually:

    public interface Set<E>
        extends Collection<E>

The most important property of a Set is:

> A Set cannot contain duplicate elements.

Example:

    Set<Integer> numbers =
        new HashSet<>();

    numbers.add(10);
    numbers.add(20);
    numbers.add(10);

The final Set contains:

    [10, 20]

The second `10` is not added because `10` already exists.

---

# 2. What is Set?

A `Set` is a collection that does not contain duplicate elements.

Example:

    Set<String> languages =
        new HashSet<>();

    languages.add("Java");
    languages.add("C");
    languages.add("Java");

Result:

    [Java, C]

Only one `"Java"` exists.

---

# 3. Set Hierarchy

The basic hierarchy is:

    Iterable<E>
         |
    Collection<E>
         |
       Set<E>
       /   \
      /     \
HashSet   SortedSet
   |          |
LinkedHashSet NavigableSet
                |
             TreeSet

More accurately:

    Iterable
       |
    Collection
       |
      Set
       |
       +---------------- HashSet
       |                     |
       |               LinkedHashSet
       |
       +---------------- SortedSet
                              |
                         NavigableSet
                              |
                           TreeSet

Important:

    Set
        -> Interface

    HashSet
        -> Class

    LinkedHashSet
        -> Class

    SortedSet
        -> Interface

    NavigableSet
        -> Interface

    TreeSet
        -> Class

---

# 4. Why Set Exists

Sometimes we do not want duplicate data.

For example:

    User IDs
    Unique usernames
    Unique email addresses
    Unique tags
    Unique numbers
    Visited nodes
    Unique permissions

Using a List:

    [10, 20, 10, 30, 20]

Using a Set:

    [10, 20, 30]

The Set abstraction expresses the requirement:

    "Every element must be unique."

---

# 5. Key Characteristics

## 5.1 No Duplicates

The primary property of Set:

    No duplicate elements

Example:

    Set<Integer> numbers =
        new HashSet<>();

    numbers.add(10);
    numbers.add(10);

Only one `10` remains.

---

## 5.2 Set Is an Interface

You cannot directly create:

    new Set<>();

This is invalid.

Instead:

    Set<Integer> numbers =
        new HashSet<>();

or:

    Set<Integer> numbers =
        new LinkedHashSet<>();

or:

    Set<Integer> numbers =
        new TreeSet<>();

---

## 5.3 Set Extends Collection

Relationship:

    Set
      |
      extends
      |
    Collection

Therefore Set inherits Collection methods such as:

    add()
    remove()
    contains()
    size()
    isEmpty()
    clear()
    iterator()
    toArray()

---

## 5.4 Ordering Depends on Implementation

Set itself does not promise a general ordering.

For example:

    HashSet
        -> no guaranteed iteration order

    LinkedHashSet
        -> insertion order

    TreeSet
        -> sorted order

This is extremely important.

---

# 6. Set vs Collection

`Collection` is a broader interface.

    Collection
       |
       +--- List
       |
       +--- Set
       |
       +--- Queue

Set adds a stronger rule:

    No duplicates

Example:

    List:
    [A, A, B]

    Set:
    [A, B]

So:

    Collection
        -> General collection abstraction

    Set
        -> Collection with uniqueness semantics

---

# 7. Set vs List

This is one of the most important comparisons.

| Feature | List | Set |
|---|---|---|
| Duplicates | Allowed | Not allowed |
| Index-based access | Yes | No |
| `get(index)` | Yes | No |
| Ordering | Usually insertion order | Depends on implementation |
| Main purpose | Ordered sequence | Unique elements |
| Common implementations | ArrayList, LinkedList | HashSet, LinkedHashSet, TreeSet |

Example:

    List:

    [10, 20, 10, 30]

Set:

    [10, 20, 30]

A Set does not provide:

    get(index)

because Set is not fundamentally an indexed sequence.

---

# 8. Creating a Set

## HashSet

    Set<String> names =
        new HashSet<>();

---

## LinkedHashSet

    Set<String> names =
        new LinkedHashSet<>();

---

## TreeSet

    Set<String> names =
        new TreeSet<>();

---

## Immutable Set

Using:

    Set.of()

Example:

    Set<String> names =
        Set.of(
            "Java",
            "Spring",
            "React"
        );

Important:

`Set.of()` creates an unmodifiable Set.

You cannot perform:

    names.add("Python");

after creation.

It throws:

    UnsupportedOperationException

---

# 9. Adding Elements

The main method is:

    add()

Example:

    Set<String> languages =
        new HashSet<>();

    languages.add("Java");
    languages.add("C");
    languages.add("Python");

---

## add() Return Value

This is important.

`add()` returns:

    true
        -> element was added

    false
        -> element was not added

because it already existed.

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

# 10. Duplicate Elements

Suppose:

    Set<Integer> numbers =
        new HashSet<>();

    numbers.add(10);
    numbers.add(20);
    numbers.add(10);
    numbers.add(30);
    numbers.add(20);

The logical contents are:

    10
    20
    30

The duplicates are ignored.

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

# 11. Null Elements

Whether null is allowed depends on the Set implementation.

## HashSet

Allows one null element.

Example:

    Set<String> set =
        new HashSet<>();

    set.add(null);
    set.add(null);

Only one null can exist.

---

## LinkedHashSet

Also allows one null element.

---

## TreeSet

Normally does not allow null with natural ordering.

Example:

    Set<Integer> numbers =
        new TreeSet<>();

    numbers.add(null);

This results in a `NullPointerException` in typical natural-ordering usage.

Important:

> Do not assume all Set implementations have the same null behavior.

---

# 12. Removing Elements

## remove()

    Set<String> names =
        new HashSet<>();

    names.add("Java");
    names.add("Spring");

    names.remove("Java");

Now:

    [Spring]

---

## remove() Return Value

`remove()` returns:

    true
        -> element existed and was removed

    false
        -> element did not exist

Example:

    System.out.println(
        names.remove("Java")
    );

---

# 13. Searching Elements

Set provides:

    contains()

Example:

    Set<String> names =
        new HashSet<>();

    names.add("Java");
    names.add("Spring");

    System.out.println(
        names.contains("Java")
    );

Output:

    true

---

## contains() Complexity

Complexity depends on the implementation.

Typical:

    HashSet
        -> O(1) average

    TreeSet
        -> O(log n)

A Set interface itself does not guarantee one universal complexity.

---

# 14. Size and Empty Checks

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

# 15. Iterating a Set

Since Set extends Collection, it supports:

    Iterator

Example:

    Set<String> names =
        new HashSet<>();

    names.add("Java");
    names.add("Spring");
    names.add("React");

    Iterator<String> iterator =
        names.iterator();

    while (iterator.hasNext()) {

        System.out.println(
            iterator.next()
        );
    }

---

## Enhanced For Loop

    for (String name : names) {

        System.out.println(name);
    }

---

## forEach()

    names.forEach(
        name -> System.out.println(name)
    );

Remember:

The iteration order depends on the Set implementation.

---

# 16. Set Interface Methods

Because Set extends Collection, it inherits most of its methods.

Important methods:

    add(E e)

    addAll(Collection<? extends E> c)

    remove(Object o)

    removeAll(Collection<?> c)

    retainAll(Collection<?> c)

    contains(Object o)

    containsAll(Collection<?> c)

    size()

    isEmpty()

    clear()

    iterator()

    toArray()

    removeIf(Predicate<? super E> filter)

    stream()

    parallelStream()

    spliterator()

---

# 17. Bulk Operations

Set becomes especially powerful when performing operations on groups of elements.

Important methods:

    addAll()
    containsAll()
    retainAll()
    removeAll()

These can be used to perform mathematical-style Set operations.

---

# 18. Mathematical Set Operations

Suppose:

    A = {1, 2, 3}

    B = {3, 4, 5}

We can perform:

    Union
    Intersection
    Difference

Using Java Set operations.

---

## Union

Union contains elements from both sets.

    A ∪ B

Result:

    {1, 2, 3, 4, 5}

Java:

    Set<Integer> union =
        new HashSet<>(A);

    union.addAll(B);

---

## Intersection

Intersection contains elements common to both sets.

    A ∩ B

Result:

    {3}

Java:

    Set<Integer> intersection =
        new HashSet<>(A);

    intersection.retainAll(B);

---

## Difference

Difference contains elements in A that are not in B.

    A - B

Result:

    {1, 2}

Java:

    Set<Integer> difference =
        new HashSet<>(A);

    difference.removeAll(B);

---

# 19. containsAll()

`containsAll()` checks whether one collection contains every element of another collection.

Example:

    Set<Integer> A =
        new HashSet<>();

    A.add(10);
    A.add(20);
    A.add(30);

    Set<Integer> B =
        new HashSet<>();

    B.add(10);
    B.add(20);

    System.out.println(
        A.containsAll(B)
    );

Output:

    true

Meaning:

    B ⊆ A

---

# 20. addAll()

`addAll()` adds all elements from another Collection.

Example:

    Set<Integer> A =
        new HashSet<>();

    A.add(10);
    A.add(20);

    Set<Integer> B =
        new HashSet<>();

    B.add(20);
    B.add(30);

    A.addAll(B);

Result:

    [10, 20, 30]

Duplicates are automatically eliminated.

This is why:

    addAll()

is useful for Set union.

---

# 21. retainAll()

`retainAll()` keeps only elements that are also present in another Collection.

Example:

    Set<Integer> A =
        new HashSet<>(
            Set.of(1, 2, 3, 4)
        );

    Set<Integer> B =
        new HashSet<>(
            Set.of(3, 4, 5, 6)
        );

    A.retainAll(B);

Result:

    [3, 4]

Therefore:

    retainAll()
        -> Intersection

---

# 22. removeAll()

`removeAll()` removes all elements that are present in another Collection.

Example:

    Set<Integer> A =
        new HashSet<>(
            Set.of(1, 2, 3, 4)
        );

    Set<Integer> B =
        new HashSet<>(
            Set.of(3, 4, 5, 6)
        );

    A.removeAll(B);

Result:

    [1, 2]

Therefore:

    removeAll()
        -> Difference

---

# 23. removeIf()

`removeIf()` removes elements that satisfy a condition.

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

Important:

`removeIf()` was introduced as a default method on `Collection` in Java 8.

---

# 24. clear()

`clear()` removes all elements.

Example:

    Set<Integer> numbers =
        new HashSet<>();

    numbers.add(10);
    numbers.add(20);

    numbers.clear();

    System.out.println(numbers);

Output:

    []

---

# 25. Set and equals()

Set uniqueness is based on the equality semantics of the elements.

For hash-based Sets such as HashSet, `equals()` and `hashCode()` are especially important.

Example:

    String a = new String("Java");
    String b = new String("Java");

    System.out.println(
        a.equals(b)
    );

Output:

    true

Therefore, adding both to a HashSet results in one logical element.

Example:

    Set<String> set =
        new HashSet<>();

    set.add(a);
    set.add(b);

Size:

    1

---

# 26. Set and hashCode()

For hash-based Sets:

    hashCode()
        +
    equals()

are fundamental to duplicate detection.

Important contract:

If:

    a.equals(b)

is true, then:

    a.hashCode() == b.hashCode()

must also be true.

For HashSet, conceptually:

    add(element)
         |
         v
    hashCode()
         |
         v
    Find bucket
         |
         v
    Compare using equals()
         |
      /     \
    equal   different
      |        |
   duplicate   add

---

# 27. How Set Prevents Duplicates

The exact mechanism depends on the implementation.

## HashSet

Uses hashing.

Conceptually:

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
       +---- equal ------> duplicate
       |
       +---- different --> store

---

## LinkedHashSet

Uses hash-based lookup while additionally maintaining insertion-order information.

---

## TreeSet

Uses ordering rather than hashing.

It uses:

    Comparable

or:

    Comparator

to determine element ordering and whether elements are considered equivalent for Set purposes.

This is an extremely important distinction.

---

# 28. HashSet

`HashSet` is the most commonly used general-purpose Set implementation.

Characteristics:

    No guaranteed iteration order
    Allows one null
    No duplicates
    Average O(1) add
    Average O(1) remove
    Average O(1) contains

Example:

    Set<Integer> numbers =
        new HashSet<>();

    numbers.add(30);
    numbers.add(10);
    numbers.add(20);

The displayed order should not be relied upon.

---

# 29. LinkedHashSet

`LinkedHashSet` extends HashSet and maintains insertion order.

Example:

    Set<Integer> numbers =
        new LinkedHashSet<>();

    numbers.add(30);
    numbers.add(10);
    numbers.add(20);

Iteration order:

    30
    10
    20

It combines:

    HashSet-style uniqueness
             +
    insertion-order tracking

---

# 30. TreeSet

`TreeSet` stores elements according to their sorted ordering.

Example:

    Set<Integer> numbers =
        new TreeSet<>();

    numbers.add(30);
    numbers.add(10);
    numbers.add(20);

Iteration:

    10
    20
    30

TreeSet is useful when you need:

    uniqueness
        +
    sorted order

Typical complexity:

    add()
        -> O(log n)

    remove()
        -> O(log n)

    contains()
        -> O(log n)

---

# 31. SortedSet

`SortedSet` is an interface extending Set.

Hierarchy:

    Set
      |
    SortedSet
      |
    NavigableSet
      |
    TreeSet

SortedSet provides a sorted-set abstraction.

Important methods include:

    comparator()

    first()

    last()

    headSet()

    tailSet()

    subSet()

---

## Example

    SortedSet<Integer> numbers =
        new TreeSet<>();

    numbers.add(10);
    numbers.add(20);
    numbers.add(30);

    System.out.println(
        numbers.first()
    );

    System.out.println(
        numbers.last()
    );

Output:

    10
    30

---

# 32. NavigableSet

`NavigableSet` extends `SortedSet`.

Hierarchy:

    Set
      |
    SortedSet
      |
    NavigableSet
      |
    TreeSet

NavigableSet adds navigation methods such as:

    lower()
    floor()
    ceiling()
    higher()

and:

    pollFirst()
    pollLast()

It also supports:

    descendingSet()
    descendingIterator()

---

## Example

    NavigableSet<Integer> numbers =
        new TreeSet<>(
            Set.of(
                10,
                20,
                30,
                40
            )
        );

    System.out.println(
        numbers.lower(30)
    );

    System.out.println(
        numbers.floor(30)
    );

    System.out.println(
        numbers.ceiling(30)
    );

    System.out.println(
        numbers.higher(30)
    );

Output:

    20
    30
    30
    40

Meaning:

    lower(x)
        -> strictly less than x

    floor(x)
        -> less than or equal to x

    ceiling(x)
        -> greater than or equal to x

    higher(x)
        -> strictly greater than x

---

# 33. Set Ordering

Do not assume:

    Set = sorted

That is incorrect.

Different implementations have different ordering behavior.

## HashSet

    No guaranteed iteration order

## LinkedHashSet

    Insertion order

## TreeSet

    Sorted order

Therefore:

    Set
      |
      +--> HashSet
      |      -> no guaranteed order
      |
      +--> LinkedHashSet
      |      -> insertion order
      |
      +--> TreeSet
             -> sorted order

---

# 34. Set Implementations Comparison

| Feature | HashSet | LinkedHashSet | TreeSet |
|---|---|---|---|
| Interface | Set | Set | NavigableSet |
| Duplicates | No | No | No |
| Ordering | No guaranteed order | Insertion order | Sorted |
| Internal concept | Hash table | Hash table + linked structure | Tree |
| Average add | O(1) | O(1) | O(log n) |
| Average contains | O(1) | O(1) | O(log n) |
| Allows null | One null | One null | Normally no null with natural ordering |
| Sorted | No | No | Yes |
| Navigation | No | No | Yes |

---

# 35. Set and null

Remember this implementation-specific behavior:

    HashSet
        -> one null allowed

    LinkedHashSet
        -> one null allowed

    TreeSet
        -> normally no null with natural ordering

Example:

    Set<String> set =
        new HashSet<>();

    set.add(null);
    set.add(null);

Result:

    [null]

Only one null is allowed because null itself cannot be duplicated.

---

# 36. Set Performance

The Set interface does not define one universal complexity.

Performance depends on implementation.

## HashSet

Typical:

    add()
        -> O(1) average

    remove()
        -> O(1) average

    contains()
        -> O(1) average

---

## LinkedHashSet

Typical:

    add()
        -> O(1) average

    remove()
        -> O(1) average

    contains()
        -> O(1) average

with additional memory for maintaining linked iteration order.

---

## TreeSet

Typically:

    add()
        -> O(log n)

    remove()
        -> O(log n)

    contains()
        -> O(log n)

because it is tree-based.

---

# 37. Set Use Cases

Set is useful whenever uniqueness matters.

## 1. Remove Duplicates

Example:

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

Result:

    [10, 20, 30]

---

## 2. Fast Membership Checking

Example:

    if (userIds.contains(id)) {
        // user exists
    }

---

## 3. Unique Tags

    Set<String> tags =
        new HashSet<>();

---

## 4. Visited Nodes

Graph algorithms often use:

    Set<Integer> visited

to avoid processing the same node repeatedly.

---

## 5. Unique Permissions

    Set<String> permissions

can represent unique permissions.

---

## 6. Sorted Unique Values

Use:

    TreeSet

when you need:

    unique + sorted

---

# 38. Set in DSA

Set is extremely useful in DSA.

Common problems:

    Remove duplicates
    Detect duplicates
    Two Sum variants
    Longest consecutive sequence
    Union of arrays
    Intersection of arrays
    Duplicate detection
    Frequency-related problems
    Graph traversal
    Cycle detection
    Visited-node tracking

Example:

    Set<Integer> seen =
        new HashSet<>();

    for (int number : numbers) {

        if (seen.contains(number)) {
            System.out.println(
                "Duplicate found"
            );
        }

        seen.add(number);
    }

---

# 39. Advantages

## 1. Prevents Duplicates

Uniqueness is built into the abstraction.

---

## 2. Efficient Searching

HashSet provides average O(1) membership checking.

---

## 3. Multiple Implementations

You can choose based on your requirement:

    HashSet
        -> fast general-purpose Set

    LinkedHashSet
        -> insertion order

    TreeSet
        -> sorted order

---

## 4. Useful Mathematical Operations

Set methods naturally support:

    Union
    Intersection
    Difference

---

## 5. Excellent for DSA

Sets are heavily used for:

    duplicate detection
    visited tracking
    membership checks

---

# 40. Disadvantages

## 1. No Index-Based Access

You cannot directly do:

    set.get(2)

---

## 2. Ordering Depends on Implementation

A general Set does not promise insertion or sorted order.

---

## 3. Hash-Based Sets Require Correct equals/hashCode

For custom objects, incorrect equality contracts can cause unexpected behavior.

---

## 4. TreeSet Has Additional Requirements

Elements generally need to be mutually comparable or a suitable Comparator must be supplied.

---

## 5. Memory Overhead

Hash-based and tree-based implementations generally require more internal structure than a simple array.

---

# 41. Common Mistakes

## Mistake 1 — Thinking Set Is Always Sorted

Wrong.

    HashSet
        -> no guaranteed order

    LinkedHashSet
        -> insertion order

    TreeSet
        -> sorted order

---

## Mistake 2 — Thinking Set Supports get(index)

Wrong.

Set does not provide indexed access.

---

## Mistake 3 — Thinking Set Does Not Allow null

Not universally true.

HashSet and LinkedHashSet allow one null.

TreeSet normally does not allow null with natural ordering.

---

## Mistake 4 — Thinking add() Always Returns true

Wrong.

For Set:

    add(existingElement)
        -> false

---

## Mistake 5 — Thinking contains() Is Always O(1)

Wrong.

It depends on implementation.

Typical:

    HashSet
        -> O(1) average

    TreeSet
        -> O(log n)

---

## Mistake 6 — Forgetting equals() and hashCode()

For HashSet with custom objects, proper `equals()` and `hashCode()` implementations are essential for correct logical duplicate detection.

---

## Mistake 7 — Thinking TreeSet Uses hashCode()

TreeSet is ordered and relies on natural ordering or a Comparator rather than HashSet-style hashing for its core uniqueness/order decisions.

---

## Mistake 8 — Modifying a Set During for-each

Example:

    for (String value : set) {

        set.remove(value);
    }

This can cause:

    ConcurrentModificationException

Use an Iterator's `remove()` when appropriate, or use suitable collection methods such as `removeIf()`.

---

# 42. Interview Traps

## Trap 1

Question:

> What is the main property of Set?

Answer:

    No duplicate elements.

---

## Trap 2

Question:

> Is Set an interface or class?

Answer:

    Interface

---

## Trap 3

Question:

> Which interface does Set extend?

Answer:

    Collection

---

## Trap 4

Question:

> Can we instantiate Set directly?

Answer:

No.

Use an implementation such as:

    HashSet
    LinkedHashSet
    TreeSet

---

## Trap 5

Question:

> Does Set maintain insertion order?

Answer:

The Set interface itself does not guarantee it.

`LinkedHashSet` does.

---

## Trap 6

Question:

> Does HashSet maintain insertion order?

Answer:

No guaranteed iteration order.

---

## Trap 7

Question:

> Which Set maintains insertion order?

Answer:

    LinkedHashSet

---

## Trap 8

Question:

> Which Set maintains sorted order?

Answer:

    TreeSet

---

## Trap 9

Question:

> What is the parent interface of SortedSet?

Answer:

    Set

---

## Trap 10

Question:

> What does NavigableSet extend?

Answer:

    SortedSet

---

## Trap 11

Question:

> What does TreeSet implement?

TreeSet implements the NavigableSet interface and therefore also fulfills the SortedSet and Set contracts.

---

## Trap 12

Question:

> Can HashSet contain null?

Answer:

Yes, one null element.

---

## Trap 13

Question:

> Can LinkedHashSet contain null?

Answer:

Yes, one null element.

---

## Trap 14

Question:

> Can TreeSet contain null?

Answer:

Normally no when using natural ordering.

---

## Trap 15

Question:

> How does HashSet identify duplicates?

Conceptually using:

    hashCode()
        +
    equals()

---

## Trap 16

Question:

> How does TreeSet identify element equivalence?

Through its ordering mechanism:

    compareTo()

or:

    Comparator.compare()

---

## Trap 17

Question:

> What does add() return if an element already exists?

    false

---

## Trap 18

Question:

> Does Set have get(index)?

No.

---

## Trap 19

Question:

> What is the average complexity of HashSet.contains()?

Typically:

    O(1)

---

## Trap 20

Question:

> What is TreeSet.contains() complexity?

Typically:

    O(log n)

---

# 43. Practical Examples

## Example 1 — Basic Set

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

## Example 2 — Duplicate Removal

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

Possible output:

    [20, 10, 30]

Do not rely on this particular ordering for HashSet.

---

## Example 3 — add() Return Value

    import java.util.*;

    public class Main {

        public static void main(String[] args) {

            Set<Integer> numbers =
                new HashSet<>();

            System.out.println(
                numbers.add(10)
            );

            System.out.println(
                numbers.add(10)
            );
        }
    }

Output:

    true
    false

---

## Example 4 — LinkedHashSet

    import java.util.*;

    public class Main {

        public static void main(String[] args) {

            Set<Integer> numbers =
                new LinkedHashSet<>();

            numbers.add(30);
            numbers.add(10);
            numbers.add(20);

            System.out.println(numbers);
        }
    }

Output:

    [30, 10, 20]

Insertion order is preserved.

---

## Example 5 — TreeSet

    import java.util.*;

    public class Main {

        public static void main(String[] args) {

            Set<Integer> numbers =
                new TreeSet<>();

            numbers.add(30);
            numbers.add(10);
            numbers.add(20);

            System.out.println(numbers);
        }
    }

Output:

    [10, 20, 30]

---

## Example 6 — Union

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

## Example 7 — Intersection

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

## Example 8 — Difference

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

## Example 9 — containsAll()

    import java.util.*;

    public class Main {

        public static void main(String[] args) {

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
        }
    }

Output:

    true

---

## Example 10 — NavigableSet

    import java.util.*;

    public class Main {

        public static void main(String[] args) {

            NavigableSet<Integer> numbers =
                new TreeSet<>(
                    Set.of(
                        10,
                        20,
                        30,
                        40
                    )
                );

            System.out.println(
                numbers.lower(30)
            );

            System.out.println(
                numbers.floor(30)
            );

            System.out.println(
                numbers.ceiling(30)
            );

            System.out.println(
                numbers.higher(30)
            );
        }
    }

Output:

    20
    30
    30
    40

---

## Example 11 — Remove Duplicates from List

    import java.util.*;

    public class Main {

        public static void main(String[] args) {

            List<String> names =
                List.of(
                    "Java",
                    "Spring",
                    "Java",
                    "React",
                    "Spring"
                );

            Set<String> unique =
                new LinkedHashSet<>(
                    names
                );

            System.out.println(unique);
        }
    }

Output:

    [Java, Spring, React]

Why use LinkedHashSet?

Because we want:

    uniqueness
        +
    original insertion order

---

# 44. Top Interview Questions

## Q1. What is Set in Java?

Set is an interface that represents a collection that does not allow duplicate elements.

---

## Q2. Which interface does Set extend?

    Collection

---

## Q3. Can Set contain duplicate elements?

No.

---

## Q4. Can Set contain null?

Depends on implementation.

HashSet and LinkedHashSet allow one null; TreeSet normally does not allow null with natural ordering.

---

## Q5. Can we instantiate Set directly?

No.

Use an implementation.

---

## Q6. What are common Set implementations?

    HashSet
    LinkedHashSet
    TreeSet

---

## Q7. Difference between HashSet and LinkedHashSet?

HashSet provides no guaranteed iteration order.

LinkedHashSet maintains insertion order.

---

## Q8. Difference between HashSet and TreeSet?

HashSet is hash-based and typically provides average O(1) basic operations.

TreeSet is tree-based and maintains sorted order with typically O(log n) basic operations.

---

## Q9. Which Set maintains insertion order?

    LinkedHashSet

---

## Q10. Which Set maintains sorted order?

    TreeSet

---

## Q11. What is SortedSet?

An interface that extends Set and represents a Set maintaining sorted order.

---

## Q12. What is NavigableSet?

An interface extending SortedSet that provides navigation operations such as:

    lower()
    floor()
    ceiling()
    higher()

---

## Q13. What does HashSet use to identify duplicates?

Conceptually:

    hashCode()
    +
    equals()

---

## Q14. What does TreeSet use to determine ordering?

    Comparable

or:

    Comparator

---

## Q15. Does Set support index-based access?

No.

---

## Q16. What does add() return?

    true
        -> element was added

    false
        -> duplicate already existed

---

## Q17. What is the average complexity of HashSet.contains()?

    O(1)

---

## Q18. What is TreeSet.contains() complexity?

Typically:

    O(log n)

---

## Q19. What is the difference between Set and List?

List allows duplicates and supports index-based access.

Set prevents duplicates and does not provide indexed access.

---

## Q20. How can you remove duplicates from a List?

Convert the List into a Set.

Example:

    Set<Integer> unique =
        new HashSet<>(numbers);

If insertion order must be preserved:

    Set<Integer> unique =
        new LinkedHashSet<>(numbers);

---

## Q21. How do you find union of two Sets?

Use:

    addAll()

---

## Q22. How do you find intersection?

Use:

    retainAll()

---

## Q23. How do you find difference?

Use:

    removeAll()

---

## Q24. What does containsAll() do?

Checks whether all elements of one Collection exist in another.

---

## Q25. Is Set sorted by default?

No.

Sorting depends on the implementation.

---

## Q26. What is the difference between size and capacity in Set?

The Set abstraction does not expose a general capacity concept like Vector.

`size()` gives the number of elements.

---

## Q27. Can a Set contain mutable objects?

Yes, but changing fields that participate in `equals()`/`hashCode()` after insertion into a hash-based Set can break expected lookup behavior.

---

## Q28. Why should equals() and hashCode() be consistent for HashSet elements?

Because HashSet relies on hashing and equality to locate and identify logically equal elements.

---

## Q29. Can two objects have the same hashCode but be different?

Yes.

This is called a hash collision.

Therefore HashSet cannot rely only on hashCode; equality checking is also important.

---

## Q30. Can two objects be equal but have different hashCodes?

No.

If:

    a.equals(b)

is true, their hash codes must be equal according to the Java contract.

---

# 45. 30-Second Interview Answer

If the interviewer asks:

> "What is Set in Java?"

Answer:

> "`Set` is an interface in the Java Collections Framework that extends `Collection` and represents a collection of unique elements. It does not provide index-based access, and ordering depends on the implementation. `HashSet` provides hash-based storage with no guaranteed iteration order, `LinkedHashSet` maintains insertion order, and `TreeSet` maintains sorted order. For HashSet, `hashCode()` and `equals()` are important for duplicate detection, while TreeSet uses natural ordering or a Comparator."

---

# 46. Cheat Sheet

## Core Definition

    Set
        -> Interface
        -> extends Collection
        -> No duplicates
        -> No index-based access

---

## Main Implementations

    HashSet
        -> No guaranteed order
        -> Hash-based
        -> Average O(1)

    LinkedHashSet
        -> Insertion order
        -> Hash-based + linked order
        -> Average O(1)

    TreeSet
        -> Sorted order
        -> Tree-based
        -> O(log n)

---

## Hierarchy

    Iterable
       |
    Collection
       |
      Set
      |
      +---- HashSet
      |       |
      |   LinkedHashSet
      |
      +---- SortedSet
              |
         NavigableSet
              |
           TreeSet

---

## Important Methods

    add()
    addAll()

    remove()
    removeAll()

    contains()
    containsAll()

    retainAll()

    removeIf()

    size()
    isEmpty()

    clear()

    iterator()

    toArray()

---

## Mathematical Operations

    Union
        -> addAll()

    Intersection
        -> retainAll()

    Difference
        -> removeAll()

---

## Null

    HashSet
        -> One null

    LinkedHashSet
        -> One null

    TreeSet
        -> Normally no null with natural ordering

---

## Ordering

    HashSet
        -> No guaranteed order

    LinkedHashSet
        -> Insertion order

    TreeSet
        -> Sorted order

---

# 47. Quick Revision

Remember:

    SET = UNIQUE ELEMENTS

The most important hierarchy:

    Collection
        |
       Set
        |
        +---- HashSet
        |
        +---- LinkedHashSet
        |
        +---- SortedSet
                  |
             NavigableSet
                  |
               TreeSet

---

Key facts:

    1. Set is an interface.

    2. Set extends Collection.

    3. Set does not allow duplicate elements.

    4. Set does not provide index-based access.

    5. Set itself does not guarantee ordering.

    6. HashSet has no guaranteed iteration order.

    7. LinkedHashSet maintains insertion order.

    8. TreeSet maintains sorted order.

    9. HashSet typically provides O(1) average add/contains/remove.

    10. TreeSet typically provides O(log n) add/contains/remove.

    11. HashSet allows one null.

    12. LinkedHashSet allows one null.

    13. TreeSet normally does not allow null with natural ordering.

    14. Set.add() returns false when the element already exists.

    15. HashSet uses hashCode() and equals() for logical duplicate detection.

    16. TreeSet uses natural ordering or a Comparator.

    17. addAll() can be used for union.

    18. retainAll() can be used for intersection.

    19. removeAll() can be used for difference.

    20. containsAll() checks whether all elements exist.

    21. SortedSet extends Set.

    22. NavigableSet extends SortedSet.

    23. TreeSet implements NavigableSet.

    24. Set is heavily used in DSA for uniqueness and membership checking.

---

# Final Memory Trick

Think of Set as:

    "I don't care about positions.
     I care about UNIQUE values."

Then choose the implementation:

    Need unique values
            |
            v
           Set
            |
      +-----+-----+
      |     |     |
      v     v     v
   HashSet Linked  TreeSet
            HashSet
      |       |       |
   No order  Insert  Sorted
             order   order

And remember:

    HashSet
        -> Fast + Unique

    LinkedHashSet
        -> Fast + Unique + Insertion Order

    TreeSet
        -> Unique + Sorted + Navigation