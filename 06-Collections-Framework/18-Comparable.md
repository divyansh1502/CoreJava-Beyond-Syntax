18-Comparable.md

# 18 — Comparable Interface ⚖️

> **Package:** `java.lang`  
> **Type:** Interface  
> **Purpose:** Defines the natural ordering of objects.  
> **Main Method:** `compareTo()`

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [What is Comparable?](#2-what-is-comparable)
3. [Why Comparable Exists](#3-why-comparable-exists)
4. [Comparable Hierarchy](#4-comparable-hierarchy)
5. [Syntax](#5-syntax)
6. [Basic Example](#6-basic-example)
7. [compareTo() Method](#7-compareto-method)
8. [Return Value of compareTo()](#8-return-value-of-compareto)
9. [Natural Ordering](#9-natural-ordering)
10. [Comparable with Integer](#10-comparable-with-integer)
11. [Comparable with String](#11-comparable-with-string)
12. [Comparable with Custom Objects](#12-comparable-with-custom-objects)
13. [Implementing Comparable](#13-implementing-comparable)
14. [Sorting Using Comparable](#14-sorting-using-comparable)
15. [Collections.sort() and Comparable](#15-collectionssort-and-comparable)
16. [Arrays.sort() and Comparable](#16-arrayssort-and-comparable)
17. [Comparable and TreeSet](#17-comparable-and-treeset)
18. [Comparable and TreeMap](#18-comparable-and-treemap)
19. [Comparable vs Comparator](#19-comparable-vs-comparator)
20. [Internal Working](#20-internal-working)
21. [Comparison Logic](#21-comparison-logic)
22. [Ascending Order](#22-ascending-order)
23. [Descending Order](#23-descending-order)
24. [Comparing Multiple Fields](#24-comparing-multiple-fields)
25. [Tie-Breaking](#25-tie-breaking)
26. [Comparable Contract](#26-comparable-contract)
27. [compareTo() Contract](#27-compareto-contract)
28. [Consistency with equals()](#28-consistency-with-equals)
29. [Important Exceptions](#29-important-exceptions)
30. [Generics and Comparable](#30-generics-and-comparable)
31. [Comparable with Inheritance](#31-comparable-with-inheritance)
32. [Comparable and Immutable Classes](#32-comparable-and-immutable-classes)
33. [Common Mistakes](#33-common-mistakes)
34. [Interview Traps](#34-interview-traps)
35. [DSA Patterns](#35-dsa-patterns)
36. [DSA Pattern 1 — Sorting](#36-dsa-pattern-1--sorting)
37. [DSA Pattern 2 — Min/Max](#37-dsa-pattern-2--minmax)
38. [DSA Pattern 3 — Ordered Data](#38-dsa-pattern-3--ordered-data)
39. [DSA Pattern 4 — TreeSet Deduplication](#39-dsa-pattern-4--treeset-deduplication)
40. [Top Interview Questions](#40-top-interview-questions)
41. [30-Second Interview Answer](#41-30-second-interview-answer)
42. [Cheat Sheet](#42-cheat-sheet)
43. [Quick Revision](#43-quick-revision)

---

# 1. Introduction ⚖️

Java often needs to determine the ordering of objects.

For example:

    10 < 20
    20 < 30

This is easy for numbers.

But consider custom objects:

    Student
    Employee
    Product
    Book

How should Java decide:

    Student A < Student B

or:

    Employee A < Employee B

Java provides the `Comparable` interface to define the object's:

    Natural Ordering

---

# 2. What is Comparable? 🎯

`Comparable<T>` is an interface that allows a class to define how its objects should naturally be ordered.

It belongs to:

    java.lang

Its main method is:

    compareTo()

The relationship is:

    Class
       |
       implements
       v
    Comparable<T>
       |
       v
    compareTo()

Example:

    class Student
            implements Comparable<Student> {

        @Override
        public int compareTo(Student other) {
            return this.age - other.age;
        }
    }

Now Java knows how to naturally order `Student` objects.

---

# 3. Why Comparable Exists? 🧠

Suppose we have:

    Student s1
    Student s2
    Student s3

Java does not automatically know which student should come first.

We need to define a comparison rule.

For example:

    age

or:

    marks

or:

    name

Comparable allows the class itself to define this rule.

Therefore:

> Comparable defines the default or natural ordering of objects.

---

# 4. Comparable Hierarchy 🌳

Comparable is a simple interface.

    java.lang
       |
       +---- Comparable<T>
                 |
                 +---- compareTo()

A custom class can implement it:

    class Student
        implements Comparable<Student>

Then:

    Student
       |
       v
    Comparable<Student>
       |
       v
    compareTo(Student)

Important:

`Comparable` is not part of the `java.util` package.

It is in:

    java.lang

Therefore it does not require an explicit import.

---

# 5. Syntax 📝

General syntax:

    class ClassName
            implements Comparable<ClassName> {

        @Override
        public int compareTo(ClassName other) {

            // comparison logic

        }
    }

Example:

    class Student
            implements Comparable<Student> {

        int age;

        @Override
        public int compareTo(Student other) {

            return Integer.compare(
                this.age,
                other.age
            );
        }
    }

---

# 6. Basic Example 💻

    class Student
            implements Comparable<Student> {

        int age;

        Student(int age) {
            this.age = age;
        }

        @Override
        public int compareTo(Student other) {

            return Integer.compare(
                this.age,
                other.age
            );
        }
    }

Usage:

    Student s1 = new Student(20);
    Student s2 = new Student(22);

    System.out.println(
        s1.compareTo(s2)
    );

Output:

    negative value

Because:

    20 < 22

---

# 7. compareTo() Method 🔍

The main method of Comparable is:

    int compareTo(T o)

Example:

    public int compareTo(Student other) {

        return Integer.compare(
            this.age,
            other.age
        );
    }

It compares:

    this object

with:

    other object

Conceptually:

    this
      |
      | compareTo()
      v
    other

---

# 8. Return Value of compareTo() 🔢

`compareTo()` returns an `int`.

There are three important cases:

    negative
        ->
    this object comes before other

    zero
        ->
    both are considered equal
    according to ordering

    positive
        ->
    this object comes after other

Example:

    10.compareTo(20)

returns a negative value.

    20.compareTo(20)

returns:

    0

    30.compareTo(20)

returns a positive value.

Important:

> The exact negative or positive number usually does not matter. The sign matters.

---

# 9. Natural Ordering 🌿

Natural ordering means the default ordering associated with a class.

Examples:

    Integer
        -> numeric order

    String
        -> lexicographical order

    Character
        -> character/Unicode value order

    LocalDate
        -> chronological order

For custom classes, the developer defines natural ordering by implementing:

    Comparable

---

# 10. Comparable with Integer 🔢

`Integer` implements Comparable.

Example:

    Integer a = 10;
    Integer b = 20;

    System.out.println(
        a.compareTo(b)
    );

Result:

    negative value

Because:

    10 < 20

Reverse:

    System.out.println(
        b.compareTo(a)
    );

Result:

    positive value

---

# 11. Comparable with String 🔤

`String` also implements Comparable.

Example:

    String a = "Apple";
    String b = "Banana";

    System.out.println(
        a.compareTo(b)
    );

The result is negative because:

    "Apple"

comes before:

    "Banana"

This is lexicographical comparison.

Example:

    System.out.println(
        "Java".compareTo("Java")
    );

Output:

    0

---

# 12. Comparable with Custom Objects 🧑‍💻

Suppose:

    Student

has:

    name
    age
    marks

We need to decide which field defines natural ordering.

For example:

    age

Then:

    class Student
            implements Comparable<Student> {

        String name;
        int age;

        @Override
        public int compareTo(Student other) {

            return Integer.compare(
                this.age,
                other.age
            );
        }
    }

Now:

    Student

objects are naturally ordered by:

    age

---

# 13. Implementing Comparable 🛠️

Example:

    class Employee
            implements Comparable<Employee> {

        int salary;

        Employee(int salary) {
            this.salary = salary;
        }

        @Override
        public int compareTo(Employee other) {

            return Integer.compare(
                this.salary,
                other.salary
            );
        }
    }

This tells Java:

    Employee natural order
        =
    salary order

---

# 14. Sorting Using Comparable 🔃

Example:

    import java.util.ArrayList;
    import java.util.Collections;
    import java.util.List;

    class Student
            implements Comparable<Student> {

        int age;

        Student(int age) {
            this.age = age;
        }

        @Override
        public int compareTo(Student other) {

            return Integer.compare(
                this.age,
                other.age
            );
        }

        @Override
        public String toString() {
            return String.valueOf(age);
        }
    }

    public class Main {

        public static void main(String[] args) {

            List<Student> students =
                new ArrayList<>();

            students.add(new Student(25));
            students.add(new Student(20));
            students.add(new Student(22));

            Collections.sort(students);

            System.out.println(students);
        }
    }

Output:

    [20, 22, 25]

`Collections.sort()` can use the natural ordering defined by Comparable.

---

# 15. Collections.sort() and Comparable 📚

There are two important ideas:

    Collections.sort(list)

uses the elements' natural ordering when no Comparator is supplied.

Therefore the element type should have a compatible natural ordering.

Example:

    List<Integer> numbers =
        new ArrayList<>(
            List.of(30, 10, 20)
        );

    Collections.sort(numbers);

Result:

    [10, 20, 30]

`Integer` already implements Comparable.

---

# 16. Arrays.sort() and Comparable 🔢

Arrays can also be sorted using natural ordering.

Example:

    Integer[] numbers =
        {30, 10, 20};

    Arrays.sort(numbers);

Result:

    [10, 20, 30]

For object arrays, natural ordering can be used when the elements implement `Comparable`.

---

# 17. Comparable and TreeSet 🌳

`TreeSet` maintains sorted order.

If no Comparator is provided, the elements can use their natural ordering.

Example:

    class Student
            implements Comparable<Student> {

        int age;

        Student(int age) {
            this.age = age;
        }

        @Override
        public int compareTo(Student other) {

            return Integer.compare(
                this.age,
                other.age
            );
        }
    }

Then:

    TreeSet<Student> students =
        new TreeSet<>();

The TreeSet can use:

    compareTo()

to determine ordering.

---

# 18. Comparable and TreeMap 🗺️

`TreeMap` maintains keys in sorted order.

Without an explicitly supplied Comparator, keys can use their natural ordering.

Example:

    TreeMap<Integer, String> map =
        new TreeMap<>();

    map.put(30, "C");
    map.put(10, "A");
    map.put(20, "B");

Resulting key order:

    10
    20
    30

Because `Integer` has a natural ordering.

---

# 19. Comparable vs Comparator ⚖️

This is one of the most important Java interview questions.

| Comparable | Comparator |
|---|---|
| `java.lang` | `java.util` |
| Defines natural ordering | Defines external/custom ordering |
| Method: `compareTo()` | Method: `compare()` |
| Implemented by the class being compared | Usually separate class/object/lambda |
| Usually one natural ordering | Can define multiple orderings |
| Modifies class definition | Does not require modifying the target class |
| Used by default sorting mechanisms | Supplied when custom ordering is needed |

Example:

    Comparable
        ->
    Student defines:
        natural order = age

    Comparator
        ->
    Sort Student by:
        name
        marks
        age
        salary
        etc.

Memory trick:

    Comparable
        =
    "I can compare myself."

    Comparator
        =
    "I can compare two objects."

---

# 20. Internal Working ⚙️

Suppose:

    students = [
        Student(30),
        Student(20),
        Student(25)
    ]

When sorting is requested, the sorting mechanism needs to compare elements.

Conceptually:

    Student(30)
          |
          | compareTo
          v
    Student(20)

Result:

    positive

Therefore:

    20 should come before 30

Another comparison:

    Student(25)
          |
          | compareTo
          v
    Student(30)

Result:

    negative

The sorting algorithm uses these comparison results to arrange elements.

Important:

> Comparable does not itself perform sorting.

It only defines the comparison rule.

The sorting algorithm performs the actual rearrangement.

---

# 21. Comparison Logic 🔍

Suppose:

    this.age = 20
    other.age = 25

Then:

    compareTo()

should indicate:

    20 < 25

Therefore return:

    negative

If:

    this.age = 25
    other.age = 25

return:

    0

If:

    this.age = 30
    other.age = 25

return:

    positive

Conceptually:

    this < other
        -> negative

    this == other
        -> 0

    this > other
        -> positive

---

# 22. Ascending Order ⬆️

Natural ascending order:

    10
    20
    30

Example:

    @Override
    public int compareTo(Student other) {

        return Integer.compare(
            this.age,
            other.age
        );
    }

This means:

    smaller age
        ->
    comes first

---

# 23. Descending Order ⬇️

Comparable can technically define descending natural order.

Example:

    @Override
    public int compareTo(Student other) {

        return Integer.compare(
            other.age,
            this.age
        );
    }

Then:

    30
    20
    10

However, in many real applications, it is often preferable to keep a sensible natural order and use a Comparator when different orderings are required.

---

# 24. Comparing Multiple Fields 🔀

Suppose:

    Student

has:

    marks
    age

We want:

    marks first

and if marks are equal:

    age

Example:

    @Override
    public int compareTo(Student other) {

        int result =
            Integer.compare(
                this.marks,
                other.marks
            );

        if (result != 0) {
            return result;
        }

        return Integer.compare(
            this.age,
            other.age
        );
    }

Logic:

    Compare marks
        |
        +---- different
        |       |
        |       v
        |     return result
        |
        +---- same
                |
                v
            Compare age

---

# 25. Tie-Breaking 🧩

Suppose:

    Student A
        marks = 90
        age = 20

    Student B
        marks = 90
        age = 22

First comparison:

    marks

Result:

    0

So we need a tie-breaker:

    age

Then:

    20 < 22

Therefore Student A comes first.

This technique is called:

    Tie-breaking

---

# 26. Comparable Contract 📜

Comparable has an important mathematical-style contract.

For objects `x`, `y`, and `z`:

### Sign consistency

The sign of:

    x.compareTo(y)

should be consistent with:

    -x.compareTo(y)

in the corresponding reverse comparison:

    y.compareTo(x)

Conceptually:

    x < y
        =>
    y > x

---

### Transitivity

If:

    x < y

and:

    y < z

then:

    x < z

should also hold.

---

### Equality of ordering

If:

    x.compareTo(y) == 0

then for another object `z`, the sign of:

    x.compareTo(z)

should be consistent with:

    y.compareTo(z)

This ensures a coherent ordering.

---

# 27. compareTo() Contract 📐

The most important interpretation is:

    compareTo() < 0
        ->
    this comes before other

    compareTo() == 0
        ->
    same ordering position

    compareTo() > 0
        ->
    this comes after other

Important:

> `compareTo() == 0` means the objects are considered equal according to the ordering, but it does not necessarily mean `equals()` returns true.

This distinction is very important for TreeSet and TreeMap.

---

# 28. Consistency with equals() ⚠️

Ideally:

    x.compareTo(y) == 0

should be consistent with:

    x.equals(y)

But this is not an absolute requirement of the Comparable interface.

Some Java classes have natural orderings that are inconsistent with equals.

This can produce surprising behavior in sorted collections.

For example:

    TreeSet

uses ordering to determine whether an element is already present.

Therefore, if:

    compareTo() == 0

TreeSet may treat two objects as duplicates even if:

    equals()

would return false.

Important interview point:

> Sorted collections such as TreeSet and TreeMap use comparison ordering, not simply equals/hashCode, to organize and identify keys/elements.

---

# 29. Important Exceptions ⚠️

## ClassCastException

A `compareTo()` implementation may fail if the object passed is not of the expected compatible type.

With generics, this is usually reduced by type safety.

---

## NullPointerException

Many natural-order comparisons do not support comparing against `null`.

For example:

    object.compareTo(null)

may throw:

    NullPointerException

The exact behavior depends on the implementation.

---

# 30. Generics and Comparable 🧬

The recommended pattern is:

    class Student
            implements Comparable<Student>

rather than raw Comparable.

Why?

Because:

    Comparable<Student>

means:

    compareTo(Student)

instead of accepting an arbitrary Object.

Example:

    class Student
            implements Comparable<Student> {

        @Override
        public int compareTo(
                Student other) {

            return Integer.compare(
                this.age,
                other.age
            );
        }
    }

Benefits:

    Type Safety
    No unnecessary casting
    Better compiler checking

---

# 31. Comparable with Inheritance 🧬

Suppose:

    class Person
            implements Comparable<Person>

and:

    class Student extends Person

Inheritance can make natural ordering design more complicated.

The natural ordering should remain logically consistent across the relevant type hierarchy.

Be careful when comparing objects of related classes because the ordering contract must remain:

    consistent
    transitive
    predictable

Poorly designed comparison logic can cause incorrect behavior in sorted collections and sorting algorithms.

---

# 32. Comparable and Immutable Classes 🔒

Comparable is often used with immutable value-like classes.

Examples from Java include:

    Integer
    String
    BigDecimal
    LocalDate

These classes have well-defined natural orderings.

For example:

    LocalDate

naturally orders dates chronologically.

Conceptually:

    2025-01-01
        <
    2025-06-01
        <
    2026-01-01

---

# 33. Common Mistakes ⚠️

## Mistake 1

Returning only:

    -1
    0
    1

is not required.

Any negative value, zero, or positive value is acceptable.

---

## Mistake 2

Using subtraction blindly:

    return this.age - other.age;

This can overflow for extreme integer values.

Prefer:

    Integer.compare(
        this.age,
        other.age
    )

---

## Mistake 3

Thinking Comparable sorts objects.

Comparable defines comparison.

The sorting algorithm performs sorting.

---

## Mistake 4

Thinking every custom class automatically has natural ordering.

It does not.

You need to define it.

---

## Mistake 5

Thinking compareTo() == 0 always means equals() == true.

Not necessarily.

---

## Mistake 6

Defining an inconsistent comparison.

Bad comparison logic can break:

    sorting
    TreeSet
    TreeMap

---

## Mistake 7

Using Comparable when many unrelated sorting orders are needed.

Comparator is often more appropriate for multiple custom orderings.

---

# 34. Interview Traps 🎤

## Trap 1

> Which package contains Comparable?

    java.lang

---

## Trap 2

> What is the main method of Comparable?

    compareTo()

---

## Trap 3

> What does compareTo() return?

    int

---

## Trap 4

> What does a negative result mean?

The current object comes before the other object according to the ordering.

---

## Trap 5

> What does zero mean?

Both objects occupy the same position according to the ordering.

---

## Trap 6

> What does a positive result mean?

The current object comes after the other object.

---

## Trap 7

> Does Comparable define natural ordering?

Yes.

---

## Trap 8

> Can a class have multiple natural orderings through Comparable?

A class normally defines one natural ordering through its `compareTo()` implementation.

For multiple alternative orderings, use Comparators.

---

## Trap 9

> Does Comparable perform sorting?

No.

It defines the comparison rule.

---

## Trap 10

> Does Comparator extend Comparable?

No.

They are separate interfaces with different purposes.

---

## Trap 11

> Can TreeSet use Comparable?

Yes.

If no Comparator is supplied, TreeSet can use natural ordering.

---

## Trap 12

> Can TreeMap use Comparable?

Yes.

Its keys can use natural ordering when no Comparator is supplied.

---

## Trap 13

> Is Comparable in java.util?

No.

It is in:

    java.lang

---

## Trap 14

> Why is Comparable automatically available?

Because `java.lang` is automatically imported.

---

# 35. DSA Patterns 🧩

Comparable becomes important in DSA when objects need ordering.

Major patterns:

    1. Sorting
    2. Min/Max
    3. Ordered Collections
    4. Priority-based processing
    5. Tree-based structures
    6. Searching in sorted data

---

# 36. DSA Pattern 1 — Sorting 🔃

Suppose:

    Student

objects are naturally ordered by:

    marks

Then:

    Collections.sort(students);

can arrange them using their natural ordering.

Concept:

    Objects
       |
       v
    compareTo()
       |
       v
    Sorting Algorithm
       |
       v
    Sorted List

---

# 37. DSA Pattern 2 — Min/Max 🔍

If objects have natural ordering, minimum and maximum can be determined according to that ordering.

Example:

    List<Integer> numbers =
        List.of(30, 10, 20);

    int min =
        Collections.min(numbers);

    int max =
        Collections.max(numbers);

Result:

    min = 10
    max = 30

The comparison is based on natural ordering.

---

# 38. DSA Pattern 3 — Ordered Data 🌳

Natural ordering is heavily used by sorted collections.

For example:

    TreeSet<Integer>

stores:

    10
    20
    30

and:

    TreeMap<Integer, String>

keeps keys ordered.

The key concept is:

    Natural Ordering
          |
          v
    compareTo()
          |
          v
    Ordered Collection

---

# 39. DSA Pattern 4 — TreeSet Deduplication 🌳

TreeSet uses ordering to determine element placement.

Suppose:

    compareTo()

returns:

    0

for two objects.

TreeSet can treat them as equivalent for its ordering purposes.

Therefore:

    compareTo() == 0

can affect whether a new element is added.

This is why a correct comparison contract is extremely important.

---

# 40. Top Interview Questions 🎤

## Q1. What is Comparable?

Comparable is an interface used to define the natural ordering of objects.

---

## Q2. Which package contains Comparable?

    java.lang

---

## Q3. What is the method of Comparable?

    compareTo()

---

## Q4. What is the return type of compareTo()?

    int

---

## Q5. What does a negative result mean?

The current object comes before the other object.

---

## Q6. What does zero mean?

The two objects are considered equal according to the ordering.

---

## Q7. What does a positive result mean?

The current object comes after the other object.

---

## Q8. What is natural ordering?

The default ordering defined by a class.

---

## Q9. How do you define natural ordering for a custom class?

Implement:

    Comparable<ClassName>

and override:

    compareTo()

---

## Q10. Difference between Comparable and Comparator?

Comparable defines natural ordering inside the class.

Comparator defines external/custom ordering outside the class.

---

## Q11. Can a class have multiple Comparators?

Yes.

Multiple Comparator objects or comparator expressions can define different orderings.

---

## Q12. Can a class have multiple Comparable implementations?

No.

A class implements Comparable once and therefore has one `compareTo()` contract.

---

## Q13. Does Comparable perform sorting?

No.

It provides the comparison logic used by sorting mechanisms.

---

## Q14. Can TreeSet use Comparable?

Yes.

TreeSet can use natural ordering when no Comparator is supplied.

---

## Q15. Can TreeMap use Comparable?

Yes.

TreeMap can use natural ordering of keys when no Comparator is supplied.

---

## Q16. What happens when compareTo() returns 0?

The objects are considered equivalent according to the ordering.

Sorted collections may therefore treat them as the same ordering position.

---

## Q17. Is compareTo() == 0 the same as equals() == true?

Not necessarily.

They should ideally be consistent, but the Comparable contract does not require equality in all cases.

---

## Q18. Why is Integer.compare() preferred over subtraction?

Because subtraction can overflow.

Prefer:

    Integer.compare(a, b)

over:

    a - b

for robust comparison.

---

## Q19. What happens if compareTo() violates transitivity?

Sorting and ordered collections can behave incorrectly or unpredictably.

---

## Q20. Why is Comparable important in DSA?

Because many DSA operations depend on ordering, including sorting, ordered collections, min/max operations, and tree-based structures.

---

# 41. 30-Second Interview Answer 🎯

If the interviewer asks:

> "What is Comparable in Java?"

Answer:

> "`Comparable` is an interface from `java.lang` used to define the natural ordering of objects. A class implements `Comparable<T>` and overrides the `compareTo()` method. The method returns a negative value, zero, or a positive value when the current object is smaller than, equal to, or greater than the other object according to the defined ordering. Comparable is commonly used by sorting mechanisms and ordered collections such as TreeSet and TreeMap. If we need multiple or external sorting strategies, we generally use Comparator."

---

# 42. Cheat Sheet 📋

## Interface

    Comparable<T>

---

## Package

    java.lang

---

## Main Method

    compareTo(T other)

---

## Return Type

    int

---

## Meaning

    negative
        ->
    this < other

    0
        ->
    this == other
    according to ordering

    positive
        ->
    this > other

---

## Natural Ordering

    Comparable
        |
        v
    compareTo()
        |
        v
    Default ordering

---

## Custom Class

    class Student
            implements Comparable<Student> {

        @Override
        public int compareTo(
                Student other) {

            return Integer.compare(
                this.age,
                other.age
            );
        }
    }

---

## Sorting

    Collections.sort(list);

---

## Arrays

    Arrays.sort(array);

---

## TreeSet

    TreeSet<T>

uses natural ordering when no Comparator is supplied.

---

## TreeMap

    TreeMap<K, V>

uses natural ordering of keys when no Comparator is supplied.

---

## Comparable vs Comparator

    Comparable
        ->
    compareTo()
        ->
    Natural Ordering

    Comparator
        ->
    compare()
        ->
    Custom Ordering

---

## Safe Integer Comparison

Prefer:

    Integer.compare(a, b)

instead of:

    a - b

---

# 43. Quick Revision ⚡

Remember:

    1. Comparable is an interface.

    2. Package:
       java.lang

    3. Main method:
       compareTo()

    4. Return type:
       int

    5. Comparable defines natural ordering.

    6. A class implements:
       Comparable<T>

    7. Negative result means:
       this object comes before other.

    8. Zero means:
       same ordering position.

    9. Positive result means:
       this object comes after other.

    10. Comparable does not perform sorting.

    11. It defines the comparison rule
        used by sorting mechanisms.

    12. Integer implements Comparable.

    13. String implements Comparable.

    14. LocalDate has natural chronological
        ordering.

    15. TreeSet can use natural ordering.

    16. TreeMap can use natural ordering
        of keys.

    17. Comparable usually provides one
        natural ordering.

    18. Comparator can provide multiple
        alternative orderings.

    19. Comparable:
        java.lang

    20. Comparator:
        java.util

    21. Comparable method:
        compareTo()

    22. Comparator method:
        compare()

    23. compareTo() should be transitive.

    24. compareTo() should provide
        consistent ordering.

    25. compareTo() == 0 does not
        necessarily mean equals() == true.

    26. TreeSet uses comparison ordering
        when determining element equivalence.

    27. TreeMap uses comparison ordering
        for keys.

    28. Avoid:
        a - b

        for integer comparison when overflow
        is possible.

    29. Prefer:
        Integer.compare(a, b)

    30. Comparable is an important concept
        for sorting and ordered DSA structures.

---

# Final Memory Trick 🧠

Remember:

    Comparable
        |
        v
    "I define my natural order."
        |
        v
    compareTo()
        |
        +---- negative -> before
        |
        +---- zero     -> same order
        |
        +---- positive -> after

And:

    Comparable
        =
    Natural Ordering

    Comparator
        =
    Custom Ordering

The easiest interview line:

    Comparable -> compareTo()
              -> Natural Order

    Comparator -> compare()
              -> Custom Order