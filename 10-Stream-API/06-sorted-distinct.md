# 🔥 06 — sorted() & distinct()

`sorted()` and `distinct()` are important **intermediate Stream operations** used for ordering elements and removing duplicate elements.

```text
distinct() → REMOVE DUPLICATES
sorted()   → ORDER ELEMENTS
```

> 💡 **Core idea:** Both operations return a new Stream and are lazy. They do not execute until a terminal operation is invoked.

---

# 📌 Table of Contents

1. [Introduction](#1-introduction)
2. [sorted()](#2-sorted)
3. [Natural Ordering](#3-natural-ordering)
4. [Custom Sorting with Comparator](#4-custom-sorting-with-comparator)
5. [Reverse Sorting](#5-reverse-sorting)
6. [sorted() with Objects](#6-sorted-with-objects)
7. [distinct()](#7-distinct)
8. [How distinct() Works](#8-how-distinct-works)
9. [distinct() with Objects](#9-distinct-with-objects)
10. [equals() and hashCode()](#10-equals-and-hashcode)
11. [sorted() + distinct()](#11-sorted--distinct)
12. [distinct() + sorted()](#12-distinct--sorted)
13. [Operation Order](#13-operation-order)
14. [sorted() and Parallel Streams](#14-sorted-and-parallel-streams)
15. [distinct() and Parallel Streams](#15-distinct-and-parallel-streams)
16. [Complexity](#16-complexity)
17. [Common Mistakes](#17-common-mistakes)
18. [Interview Traps](#18-interview-traps)
19. [DSA Connection](#19-dsa-connection)
20. [Top 15 Interview Questions](#20-top-15-interview-questions)
21. [30-Second Interview Answer](#21-30-second-interview-answer)
22. [Cheat Sheet](#22-cheat-sheet)

---

# 1. Introduction

Java Stream API provides two important intermediate operations for organizing data:

```java
sorted()
distinct()
```

Example:

```java
List<Integer> numbers = List.of(
        5, 2, 3, 2, 5, 1, 4
);

List<Integer> result = numbers.stream()
        .distinct()
        .sorted()
        .toList();

System.out.println(result);
```

Output:

```text
[1, 2, 3, 4, 5]
```

The pipeline performs:

```text
[5, 2, 3, 2, 5, 1, 4]
            ↓
        distinct()
            ↓
      [5, 2, 3, 1, 4]
            ↓
         sorted()
            ↓
      [1, 2, 3, 4, 5]
```

---

# 2. sorted()

## Definition

`sorted()` is an intermediate operation that returns a Stream whose elements are ordered according to a sorting rule.

There are two important forms:

```java
Stream<T> sorted();
```

and:

```java
Stream<T> sorted(Comparator<? super T> comparator);
```

---

## Basic Syntax

```java
stream.sorted()
```

Example:

```java
List<Integer> numbers = List.of(5, 2, 8, 1, 3);

List<Integer> result = numbers.stream()
        .sorted()
        .toList();

System.out.println(result);
```

Output:

```text
[1, 2, 3, 5, 8]
```

---

## Important

`sorted()` does **not** modify the original collection.

```java
List<Integer> numbers = List.of(5, 2, 8, 1, 3);

List<Integer> result = numbers.stream()
        .sorted()
        .toList();

System.out.println(numbers);
System.out.println(result);
```

Output:

```text
[5, 2, 8, 1, 3]
[1, 2, 3, 5, 8]
```

The Stream produces an ordered result without changing the source List.

---

# 3. Natural Ordering

The no-argument version:

```java
sorted()
```

uses the elements' **natural ordering**.

For example:

```java
List<Integer> numbers = List.of(5, 1, 8, 2, 3);

List<Integer> result = numbers.stream()
        .sorted()
        .toList();
```

Output:

```text
[1, 2, 3, 5, 8]
```

---

## Strings

Strings have a natural ordering based on their `Comparable` implementation.

```java
List<String> names = List.of(
        "Rahul",
        "Aman",
        "Zoya",
        "Alex"
);

List<String> result = names.stream()
        .sorted()
        .toList();

System.out.println(result);
```

Output:

```text
[Aman, Alex, Rahul, Zoya]
```

---

## Comparable

For:

```java
sorted()
```

the element type must provide a compatible natural ordering.

The relevant interface is:

```java
Comparable<T>
```

Conceptually:

```text
Element
   ↓
Comparable
   ↓
Natural ordering
   ↓
sorted()
```

---

# 4. Custom Sorting with Comparator

When you need a sorting rule different from natural ordering, use:

```java
sorted(Comparator)
```

Example:

```java
List<Integer> numbers = List.of(5, 1, 8, 2, 3);

List<Integer> result = numbers.stream()
        .sorted(Comparator.reverseOrder())
        .toList();

System.out.println(result);
```

Output:

```text
[8, 5, 3, 2, 1]
```

---

## Custom Comparator

```java
List<Integer> numbers = List.of(5, 1, 8, 2, 3);

List<Integer> result = numbers.stream()
        .sorted((a, b) -> b - a)
        .toList();

System.out.println(result);
```

Output:

```text
[8, 5, 3, 2, 1]
```

> ⚠️ For general integer comparisons, prefer `Integer.compare(b, a)` over `b - a` because subtraction can overflow for extreme integer values.

Better:

```java
List<Integer> result = numbers.stream()
        .sorted((a, b) -> Integer.compare(b, a))
        .toList();
```

---

# 5. Reverse Sorting

The easiest way to reverse natural ordering is:

```java
Comparator.reverseOrder()
```

Example:

```java
List<Integer> numbers = List.of(5, 2, 8, 1, 3);

List<Integer> result = numbers.stream()
        .sorted(Comparator.reverseOrder())
        .toList();

System.out.println(result);
```

Output:

```text
[8, 5, 3, 2, 1]
```

---

## Strings in Reverse Order

```java
List<String> names = List.of(
        "Aman",
        "Rahul",
        "Alex",
        "Zoya"
);

List<String> result = names.stream()
        .sorted(Comparator.reverseOrder())
        .toList();

System.out.println(result);
```

Output:

```text
[Zoya, Rahul, Alex, Aman]
```

---

## Natural vs Reverse

```text
sorted()
        ↓
Ascending natural order

sorted(Comparator.reverseOrder())
        ↓
Descending natural order
```

---

# 6. sorted() with Objects

Consider:

```java
record Student(
        String name,
        int marks
) {
}
```

Example:

```java
List<Student> students = List.of(
        new Student("Aman", 85),
        new Student("Rahul", 92),
        new Student("Alex", 78)
);
```

We can sort by marks using:

```java
List<Student> result = students.stream()
        .sorted(Comparator.comparingInt(Student::marks))
        .toList();
```

Output order:

```text
Alex   78
Aman   85
Rahul  92
```

---

## Descending Marks

```java
List<Student> result = students.stream()
        .sorted(
                Comparator.comparingInt(Student::marks)
                        .reversed()
        )
        .toList();
```

Output order:

```text
Rahul  92
Aman   85
Alex   78
```

---

## Sort by Name

```java
List<Student> result = students.stream()
        .sorted(Comparator.comparing(Student::name))
        .toList();
```

---

## Multiple Sorting Conditions

Suppose we want:

```text
1. Higher marks first
2. If marks are equal, name alphabetically
```

Use:

```java
List<Student> result = students.stream()
        .sorted(
                Comparator.comparingInt(Student::marks)
                        .reversed()
                        .thenComparing(Student::name)
        )
        .toList();
```

This is a very common real-world Stream pattern.

---

# 7. distinct()

## Definition

`distinct()` removes duplicate elements from the Stream according to equality semantics.

### Syntax

```java
Stream<T> distinct();
```

Example:

```java
List<Integer> numbers = List.of(
        1, 2, 2, 3, 3, 3, 4
);

List<Integer> result = numbers.stream()
        .distinct()
        .toList();

System.out.println(result);
```

Output:

```text
[1, 2, 3, 4]
```

---

## Important Property

`distinct()` is an intermediate operation.

Therefore:

```java
numbers.stream()
        .distinct();
```

does not execute the operation immediately.

A terminal operation is required:

```java
numbers.stream()
        .distinct()
        .toList();
```

---

# 8. How distinct() Works

For ordinary object streams, `distinct()` relies on equality semantics.

Conceptually, it keeps track of elements that have already appeared.

Example:

```text
Input:

1 2 2 3 1 4 3

Process:

1 → new → keep
2 → new → keep
2 → duplicate → discard
3 → new → keep
1 → duplicate → discard
4 → new → keep
3 → duplicate → discard

Result:

1 2 3 4
```

---

## Encounter Order

For an ordered Stream, `distinct()` keeps the first occurrence of each distinct element.

Example:

```java
List<Integer> numbers = List.of(
        5, 2, 5, 3, 2, 1
);

List<Integer> result = numbers.stream()
        .distinct()
        .toList();

System.out.println(result);
```

Output:

```text
[5, 2, 3, 1]
```

Notice:

```text
5 → first occurrence kept
2 → first occurrence kept
5 → removed
3 → kept
2 → removed
1 → kept
```

`distinct()` removes duplicates; it does **not** sort the data.

---

# 9. distinct() with Objects

Consider:

```java
record Student(
        int id,
        String name
) {
}
```

Because records automatically provide value-based `equals()` and `hashCode()` implementations, duplicate records can be recognized according to their component values.

Example:

```java
List<Student> students = List.of(
        new Student(1, "Aman"),
        new Student(2, "Rahul"),
        new Student(1, "Aman"),
        new Student(3, "Alex")
);

List<Student> result = students.stream()
        .distinct()
        .toList();

System.out.println(result);
```

Result:

```text
[
    Student[id=1, name=Aman],
    Student[id=2, name=Rahul],
    Student[id=3, name=Alex]
]
```

---

## Important: Objects Without Proper Equality

Consider a normal class:

```java
class Student {

    int id;
    String name;

    Student(int id, String name) {
        this.id = id;
        this.name = name;
    }
}
```

If `equals()` and `hashCode()` are not overridden appropriately, two different objects containing the same values may still be treated as different objects.

Therefore:

```java
new Student(1, "Aman")
```

and:

```java
new Student(1, "Aman")
```

are not automatically equal merely because their fields contain the same values.

---

# 10. equals() and hashCode()

This is one of the most important interview concepts behind `distinct()`.

Java's equality contract is based on:

```java
equals()
hashCode()
```

For objects that are considered equal:

```text
a.equals(b) == true
```

they must also have:

```text
a.hashCode() == b.hashCode()
```

---

## Example

```java
class Student {

    private int id;
    private String name;

    Student(int id, String name) {
        this.id = id;
        this.name = name;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) {
            return true;
        }

        if (!(obj instanceof Student other)) {
            return false;
        }

        return id == other.id
                && Objects.equals(name, other.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(id, name);
    }
}
```

Now:

```java
Student s1 = new Student(1, "Aman");
Student s2 = new Student(1, "Aman");
```

can be treated as equal by equality-based operations.

---

# 11. sorted() + distinct()

You can combine both operations.

Example:

```java
List<Integer> numbers = List.of(
        5, 2, 3, 2, 5, 1, 4
);

List<Integer> result = numbers.stream()
        .distinct()
        .sorted()
        .toList();

System.out.println(result);
```

Output:

```text
[1, 2, 3, 4, 5]
```

Pipeline:

```text
Original
[5,2,3,2,5,1,4]
        ↓
distinct()
        ↓
[5,2,3,1,4]
        ↓
sorted()
        ↓
[1,2,3,4,5]
```

---

# 12. distinct() + sorted()

You can also write:

```java
List<Integer> result = numbers.stream()
        .sorted()
        .distinct()
        .toList();
```

The final result is still:

```text
[1, 2, 3, 4, 5]
```

However, the amount of work can differ depending on the data and pipeline.

---

## Which Order Is Better?

For:

```text
distinct() → sorted()
```

you first remove duplicates and then sort fewer elements.

For:

```text
sorted() → distinct()
```

you sort duplicates as well.

If duplicate removal can substantially reduce the number of elements, doing:

```java
distinct()
```

before:

```java
sorted()
```

can reduce the amount of data that must be sorted.

---

## General Principle

```text
Reduce the amount of data early
        ↓
Then perform expensive operations
```

But always consider the actual problem and semantics rather than blindly reordering operations.

---

# 13. Operation Order

The order of Stream operations matters.

Example:

```java
List<Integer> numbers = List.of(
        5, 2, 5, 3, 2, 1
);
```

### distinct() then sorted()

```java
List<Integer> result = numbers.stream()
        .distinct()
        .sorted()
        .toList();
```

Result:

```text
[1, 2, 3, 5]
```

### sorted() then distinct()

```java
List<Integer> result = numbers.stream()
        .sorted()
        .distinct()
        .toList();
```

Result:

```text
[1, 2, 3, 5]
```

The result is equal, but the intermediate work can differ.

---

## With filter()

Consider:

```java
numbers.stream()
        .filter(n -> n > 2)
        .distinct()
        .sorted()
        .toList();
```

This is often a sensible pipeline:

```text
filter
  ↓
remove irrelevant values
  ↓
distinct
  ↓
remove duplicates
  ↓
sorted
  ↓
order remaining values
```

---

# 14. sorted() and Parallel Streams

`sorted()` has important behavior with parallel streams.

Example:

```java
List<Integer> result = numbers.parallelStream()
        .sorted()
        .toList();
```

Even though processing can happen in parallel, the resulting Stream is sorted according to the sorting rule.

However, sorting is a **stateful intermediate operation**.

It generally needs to see and organize elements rather than independently processing each element in isolation.

---

## Stateless vs Stateful Operations

This distinction is important.

### Stateless

Examples:

```text
filter()
map()
peek()
```

Each element can generally be processed independently.

### Stateful

Examples:

```text
sorted()
distinct()
```

These may need information about other elements.

---

## Why sorted() Is Stateful

To know where:

```text
5
```

belongs, the operation may need to consider other elements:

```text
1, 2, 3, 4, 5, 6...
```

Therefore, sorting cannot simply transform one element independently.

---

# 15. distinct() and Parallel Streams

`distinct()` is also a stateful operation.

In a parallel Stream:

```java
List<Integer> result = numbers.parallelStream()
        .distinct()
        .toList();
```

the implementation must coordinate duplicate detection across processing portions.

---

## Ordered vs Unordered

For ordered Streams, encounter-order semantics matter.

For example:

```java
numbers.stream()
        .distinct()
        .toList();
```

preserves the first occurrence of each distinct element in encounter order.

For an unordered Stream, encounter order is not a required semantic guarantee.

---

# 16. Complexity

Complexity depends on the implementation, data type, stream characteristics, and execution mode, so these are useful general expectations rather than strict guarantees for every implementation.

---

## sorted()

For `n` elements, comparison-based sorting is generally:

```text
Time: O(n log n)
```

Additional memory may be required:

```text
Space: O(n)
```

depending on the implementation and execution mode.

---

## distinct()

Duplicate detection generally requires tracking previously encountered values.

A useful average-case mental model is:

```text
Time: O(n)
Space: O(n)
```

because previously seen elements need to be tracked.

---

## Combined

For:

```java
stream.distinct()
      .sorted()
```

a useful high-level estimate is:

```text
distinct → O(n)
sorted   → O(n log n)
```

Overall:

```text
O(n log n)
```

with additional memory requirements.

---

# 17. Common Mistakes

## Mistake 1: Thinking sorted() Modifies the Source

Wrong assumption:

```java
numbers.stream().sorted();
```

changes `numbers`.

It does not.

Streams do not mutate the source merely because `sorted()` is used.

---

## Mistake 2: Forgetting Terminal Operation

This:

```java
numbers.stream()
        .sorted();
```

does not consume the Stream.

Use:

```java
numbers.stream()
        .sorted()
        .toList();
```

---

## Mistake 3: Assuming distinct() Sorts

This:

```java
numbers.stream()
        .distinct()
        .toList();
```

only removes duplicates.

It does not guarantee ascending order.

---

## Mistake 4: Assuming sorted() Removes Duplicates

This:

```java
numbers.stream()
        .sorted()
        .toList();
```

can still contain duplicates.

Example:

```text
Input:
5 2 2 1

Output:
1 2 2 5
```

Use:

```java
.distinct()
.sorted()
```

if you need unique sorted values.

---

## Mistake 5: Using sorted() Without a Comparable-Compatible Natural Order

This:

```java
objects.stream()
        .sorted()
        .toList();
```

requires an appropriate natural ordering.

For custom objects, use:

```java
.sorted(Comparator.comparing(...))
```

or implement `Comparable`.

---

## Mistake 6: Expecting distinct() to Work by Field Automatically

Suppose two employees have:

```text
same id
different name
```

`distinct()` does not automatically know:

> "Employee ID is the unique identity."

It follows equality semantics.

If uniqueness must be based on a specific field, use an appropriate design or a key-based collection approach.

---

# 18. Interview Traps

## Trap 1

**Are sorted() and distinct() intermediate or terminal operations?**

Both are:

```text
Intermediate
```

---

## Trap 2

**Are sorted() and distinct() stateless?**

No.

Both are generally considered **stateful intermediate operations**.

---

## Trap 3

**Does distinct() use == to compare objects?**

For ordinary object streams, distinctness is based on equality semantics, not simply reference identity.

---

## Trap 4

**Does sorted() always use Comparator?**

No.

Two forms exist:

```java
sorted()
```

uses natural ordering.

```java
sorted(comparator)
```

uses the supplied Comparator.

---

## Trap 5

**Can sorted() change the original List?**

No.

The source is not modified merely by calling Stream `sorted()`.

---

## Trap 6

**Does distinct() guarantee sorting?**

No.

It only removes duplicates.

---

## Trap 7

**Does sorted() remove duplicates?**

No.

Sorting and duplicate removal are separate operations.

---

## Trap 8

**Why is distinct() related to hashCode()?**

Equality-based duplicate detection requires correct equality semantics, including the `equals()`/`hashCode()` contract for ordinary objects.

---

## Trap 9

**Can sorted() be used on custom objects without Comparable?**

Yes, if a Comparator is supplied:

```java
.sorted(Comparator.comparing(Student::marks))
```

---

## Trap 10

**Why can operation order affect performance?**

Because stateful operations such as sorting may process fewer elements if earlier operations remove unnecessary data.

---

# 19. DSA Connection

`sorted()` and `distinct()` appear frequently in DSA problems.

---

## 19.1 Remove Duplicates

Problem:

> Remove duplicates from an array.

```java
int[] numbers = {
        5, 2, 5, 3, 2, 1
};

List<Integer> result = Arrays.stream(numbers)
        .distinct()
        .boxed()
        .toList();
```

Result:

```text
[5, 2, 3, 1]
```

---

## 19.2 Unique Sorted Values

Problem:

> Find all unique values in sorted order.

```java
List<Integer> result = numbers.stream()
        .distinct()
        .sorted()
        .toList();
```

Pattern:

```text
distinct → sorted
```

---

## 19.3 Top-Level Sorting

Problem:

> Sort numbers in ascending order.

```java
List<Integer> result = numbers.stream()
        .sorted()
        .toList();
```

---

## 19.4 Descending Order

```java
List<Integer> result = numbers.stream()
        .sorted(Comparator.reverseOrder())
        .toList();
```

---

## 19.5 Unique Even Numbers in Sorted Order

```java
List<Integer> result = numbers.stream()
        .filter(n -> n % 2 == 0)
        .distinct()
        .sorted()
        .toList();
```

Mental model:

```text
filter
  ↓
even values
  ↓
distinct
  ↓
unique values
  ↓
sorted
  ↓
ascending order
```

---

## 19.6 Find Second Largest Distinct Number

This is a useful DSA pattern:

```java
Optional<Integer> secondLargest = numbers.stream()
        .distinct()
        .sorted(Comparator.reverseOrder())
        .skip(1)
        .findFirst();
```

Example:

```java
List<Integer> numbers = List.of(
        10, 20, 30, 30, 20, 40
);

Optional<Integer> result = numbers.stream()
        .distinct()
        .sorted(Comparator.reverseOrder())
        .skip(1)
        .findFirst();

System.out.println(result);
```

Output:

```text
Optional[30]
```

Pipeline:

```text
[10,20,30,30,20,40]
        ↓
distinct
        ↓
[10,20,30,40]
        ↓
descending sort
        ↓
[40,30,20,10]
        ↓
skip(1)
        ↓
[30,20,10]
        ↓
findFirst
        ↓
30
```

---

## 19.7 Top K Distinct Values

A simple Stream approach:

```java
List<Integer> topThree = numbers.stream()
        .distinct()
        .sorted(Comparator.reverseOrder())
        .limit(3)
        .toList();
```

For very large datasets, however, a heap-based DSA solution may be more appropriate depending on the problem constraints.

---

# 20. Top 15 Interview Questions

## Q1. What is sorted()?

`sorted()` is a stateful intermediate Stream operation that orders elements according to natural ordering or a supplied Comparator.

---

## Q2. What is distinct()?

`distinct()` is a stateful intermediate operation that removes duplicate elements according to equality semantics.

---

## Q3. Are sorted() and distinct() terminal operations?

No.

Both are intermediate operations.

---

## Q4. What is the difference between sorted() and sorted(Comparator)?

```java
sorted()
```

uses natural ordering.

```java
sorted(comparator)
```

uses custom ordering defined by the Comparator.

---

## Q5. What is natural ordering?

Natural ordering is the default ordering defined by an object's `Comparable` implementation.

---

## Q6. How do you sort in descending order?

```java
.sorted(Comparator.reverseOrder())
```

---

## Q7. Can sorted() sort custom objects?

Yes, using a Comparator:

```java
.sorted(Comparator.comparing(Student::marks))
```

---

## Q8. How does distinct() identify duplicates?

For ordinary object Streams, it relies on equality semantics, which depend on correct `equals()` and `hashCode()` implementations.

---

## Q9. Does distinct() preserve order?

For an ordered Stream, `distinct()` preserves the encounter order of the first occurrence of each distinct element.

---

## Q10. Are sorted() and distinct() lazy?

Yes.

Like other intermediate operations, they are lazy and require a terminal operation to trigger processing.

---

## Q11. Why are sorted() and distinct() called stateful operations?

Because their processing can require information about multiple elements rather than independently processing each element in isolation.

---

## Q12. Does sorted() modify the original collection?

No.

It produces an ordered Stream without directly modifying the source collection.

---

## Q13. How do you find the second-largest distinct number?

One simple Stream approach is:

```java
Optional<Integer> result = numbers.stream()
        .distinct()
        .sorted(Comparator.reverseOrder())
        .skip(1)
        .findFirst();
```

---

## Q14. Which is usually better: distinct().sorted() or sorted().distinct()?

Both can produce the same result for suitable ordered data, but `distinct().sorted()` can reduce the number of elements that need sorting when duplicates are common.

---

## Q15. What is the complexity of sorting?

Comparison-based sorting is generally:

```text
O(n log n)
```

although exact behavior depends on the implementation and execution mode.

---

# 21. 30-Second Interview Answer

> **sorted() and distinct() are stateful intermediate Stream operations. sorted() orders elements using either natural ordering or a Comparator, while distinct() removes duplicate elements according to equality semantics. Both are lazy and require a terminal operation to execute. For example, I can use distinct().sorted() to obtain unique values in ascending order. For custom objects, sorted() commonly uses Comparator, while distinct() depends on correct equals() and hashCode() behavior.**

---

# 22. Cheat Sheet

| Operation | Purpose | Type | Stateful? |
|---|---|---|---|
| `sorted()` | Natural ordering | Intermediate | Yes |
| `sorted(Comparator)` | Custom ordering | Intermediate | Yes |
| `distinct()` | Remove duplicates | Intermediate | Yes |

---

## sorted() Syntax

```java
stream.sorted()
```

Natural ordering.

```java
stream.sorted(comparator)
```

Custom ordering.

---

## Common Comparator Patterns

```java
.sorted(Comparator.naturalOrder())
```

```java
.sorted(Comparator.reverseOrder())
```

```java
.sorted(Comparator.comparing(Student::name))
```

```java
.sorted(Comparator.comparingInt(Student::marks))
```

```java
.sorted(
        Comparator.comparingInt(Student::marks)
                .reversed()
)
```

```java
.sorted(
        Comparator.comparingInt(Student::marks)
                .reversed()
                .thenComparing(Student::name)
)
```

---

## distinct() Syntax

```java
stream.distinct()
```

---

# 🧠 Ultimate Memory Trick

```text
sorted()
   ↓
ORDER

distinct()
   ↓
UNIQUE
```

Remember:

```text
sorted()   → "How should elements be arranged?"
distinct() → "Which duplicate elements should be removed?"
```

---

# 🔥 Important Pipeline Patterns

### Unique + Sorted

```java
numbers.stream()
        .distinct()
        .sorted()
        .toList();
```

### Unique + Descending

```java
numbers.stream()
        .distinct()
        .sorted(Comparator.reverseOrder())
        .toList();
```

### Filter + Unique + Sorted

```java
numbers.stream()
        .filter(n -> n > 10)
        .distinct()
        .sorted()
        .toList();
```

### Unique + Descending + Top 3

```java
numbers.stream()
        .distinct()
        .sorted(Comparator.reverseOrder())
        .limit(3)
        .toList();
```

### Second Largest Distinct

```java
numbers.stream()
        .distinct()
        .sorted(Comparator.reverseOrder())
        .skip(1)
        .findFirst();
```

---

# 🎯 Final Takeaway

The key idea is:

```text
filter()
    ↓
SELECT relevant data

map()
    ↓
TRANSFORM data

distinct()
    ↓
REMOVE duplicates

sorted()
    ↓
ORDER data

reduce()
    ↓
COMBINE data
```

A powerful Stream pipeline can therefore look like:

```java
List<Integer> result = numbers.stream()
        .filter(n -> n > 0)
        .distinct()
        .sorted(Comparator.reverseOrder())
        .map(n -> n * n)
        .toList();
```

Read it from left to right:

```text
positive
   ↓
unique
   ↓
descending
   ↓
squared
   ↓
List
```

> 🚀 **For DSA and interviews, remember: `filter → select`, `map → transform`, `distinct → unique`, `sorted → order`, `reduce → combine`.**