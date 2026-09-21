# 19 — Comparator Interface 🔀

> **Package:** `java.util`  
> **Type:** Functional Interface  
> **Purpose:** Defines custom/external ordering for objects.  
> **Main Method:** `compare(T o1, T o2)`

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [What is Comparator?](#2-what-is-comparator)
3. [Why Comparator Exists](#3-why-comparator-exists)
4. [Comparator Hierarchy](#4-comparator-hierarchy)
5. [Syntax](#5-syntax)
6. [Basic Example](#6-basic-example)
7. [compare() Method](#7-compare-method)
8. [Return Value of compare()](#8-return-value-of-compare)
9. [Custom Ordering](#9-custom-ordering)
10. [Comparator with Integer](#10-comparator-with-integer)
11. [Comparator with String](#11-comparator-with-string)
12. [Comparator with Custom Objects](#12-comparator-with-custom-objects)
13. [Using a Separate Comparator Class](#13-using-a-separate-comparator-class)
14. [Using Anonymous Class](#14-using-anonymous-class)
15. [Using Lambda Expression](#15-using-lambda-expression)
16. [Comparator with Collections.sort()](#16-comparator-with-collectionssort)
17. [Comparator with List.sort()](#17-comparator-with-listsort)
18. [Comparator with Arrays.sort()](#18-comparator-with-arrayssort)
19. [Comparator with TreeSet](#19-comparator-with-treeset)
20. [Comparator with TreeMap](#20-comparator-with-treemap)
21. [Ascending Order](#21-ascending-order)
22. [Descending Order](#22-descending-order)
23. [Comparing Objects by Different Fields](#23-comparing-objects-by-different-fields)
24. [Comparator.comparing()](#24-comparatorcomparing)
25. [thenComparing()](#25-thencomparing)
26. [reversed()](#26-reversed)
27. [nullsFirst()](#27-nullsfirst)
28. [nullsLast()](#28-nullslast)
29. [naturalOrder()](#29-naturalorder)
30. [reverseOrder()](#30-reverseorder)
31. [Comparing Primitive Fields](#31-comparing-primitive-fields)
32. [Comparator and Multiple Fields](#32-comparator-and-multiple-fields)
33. [Comparator and Tie-Breaking](#33-comparator-and-tie-breaking)
34. [Comparator and Comparable](#34-comparator-and-comparable)
35. [Internal Working](#35-internal-working)
36. [Comparator Contract](#36-comparator-contract)
37. [Consistency with equals()](#37-consistency-with-equals)
38. [Comparator and Generics](#38-comparator-and-generics)
39. [Comparator as Functional Interface](#39-comparator-as-functional-interface)
40. [Method References](#40-method-references)
41. [Common Mistakes](#41-common-mistakes)
42. [Interview Traps](#42-interview-traps)
43. [DSA Patterns](#43-dsa-patterns)
44. [DSA Pattern 1 — Custom Sorting](#44-dsa-pattern-1--custom-sorting)
45. [DSA Pattern 2 — Descending Order](#45-dsa-pattern-2--descending-order)
46. [DSA Pattern 3 — Multi-Level Sorting](#46-dsa-pattern-3--multi-level-sorting)
47. [DSA Pattern 4 — Priority Ordering](#47-dsa-pattern-4--priority-ordering)
48. [DSA Pattern 5 — TreeSet Custom Ordering](#48-dsa-pattern-5--treeset-custom-ordering)
49. [Top Interview Questions](#49-top-interview-questions)
50. [30-Second Interview Answer](#50-30-second-interview-answer)
51. [Cheat Sheet](#51-cheat-sheet)
52. [Quick Revision](#52-quick-revision)

---

# 1. Introduction 🔀

Java provides two major ways to define object ordering:

    Comparable
    Comparator

`Comparable` defines the natural ordering of a class.

`Comparator` allows us to define custom or external ordering.

For example, a `Student` can naturally be sorted by:

    age

but sometimes we want:

    name
    marks
    age
    rollNumber

without changing the `Student` class.

This is where `Comparator` becomes useful.

---

# 2. What is Comparator? 🎯

`Comparator<T>` is an interface from:

    java.util

It is used to define custom ordering between two objects.

Its main method is:

    compare(T o1, T o2)

Conceptually:

    Object 1
       |
       | compare()
       v
    Object 2

The Comparator decides which object should come first.

---

# 3. Why Comparator Exists? 🧠

Suppose we have:

    class Student {

        String name;
        int age;
        int marks;
    }

We may want to sort students by:

    name

or:

    age

or:

    marks

or:

    marks descending

A class can normally have one natural ordering through:

    Comparable

But we can create multiple Comparators:

    NameComparator
    AgeComparator
    MarksComparator

Therefore:

> Comparator is useful when we need flexible, external, or multiple sorting strategies.

---

# 4. Comparator Hierarchy 🌳

The basic relationship is:

    java.util
       |
       +---- Comparator<T>
                  |
                  +---- compare()
                  |
                  +---- reversed()
                  |
                  +---- thenComparing()
                  |
                  +---- comparing()
                  |
                  +---- nullsFirst()
                  |
                  +---- nullsLast()

`Comparator` is an interface.

It is also a:

    Functional Interface

because it has one abstract method:

    compare(T o1, T o2)

---

# 5. Syntax 📝

General syntax:

    class MyComparator
            implements Comparator<Type> {

        @Override
        public int compare(
                Type o1,
                Type o2) {

            // comparison logic
        }
    }

Example:

    class AgeComparator
            implements Comparator<Student> {

        @Override
        public int compare(
                Student s1,
                Student s2) {

            return Integer.compare(
                s1.age,
                s2.age
            );
        }
    }

---

# 6. Basic Example 💻

    import java.util.Comparator;

    class AgeComparator
            implements Comparator<Student> {

        @Override
        public int compare(
                Student s1,
                Student s2) {

            return Integer.compare(
                s1.age,
                s2.age
            );
        }
    }

Usage:

    students.sort(
        new AgeComparator()
    );

Now students are ordered by:

    age

---

# 7. compare() Method 🔍

The main abstract method is:

    int compare(T o1, T o2)

It compares two objects:

    o1
    o2

Example:

    Comparator<Integer> comparator =
        new Comparator<>() {

            @Override
            public int compare(
                    Integer a,
                    Integer b) {

                return Integer.compare(
                    a,
                    b
                );
            }
        };

The comparison result determines their ordering.

---

# 8. Return Value of compare() 🔢

There are three important cases:

    negative
        ->
    o1 comes before o2

    zero
        ->
    o1 and o2 are equivalent
    according to this ordering

    positive
        ->
    o1 comes after o2

Example:

    compare(10, 20)

returns a negative value.

    compare(20, 20)

returns:

    0

    compare(30, 20)

returns a positive value.

Important:

> The sign of the result matters, not the exact negative or positive number.

---

# 9. Custom Ordering 🎨

Suppose:

    Student

has:

    name
    age
    marks

We can define:

    Name Comparator

    Age Comparator

    Marks Comparator

    Salary Comparator

The object class does not need to change.

This is the major idea:

    Object
       |
       +---- Natural Order
       |       Comparable
       |
       +---- Custom Order
               Comparator

---

# 10. Comparator with Integer 🔢

Example:

    Comparator<Integer> comparator =
        (a, b) -> Integer.compare(a, b);

Then:

    comparator.compare(10, 20)

returns:

    negative

because:

    10 < 20

Descending:

    Comparator<Integer> descending =
        (a, b) -> Integer.compare(b, a);

Now:

    descending.compare(10, 20)

returns:

    positive

because 20 should come before 10.

---

# 11. Comparator with String 🔤

Example:

    Comparator<String> comparator =
        (a, b) -> a.compareTo(b);

Then:

    List<String> names =
        new ArrayList<>(
            List.of(
                "Yashu",
                "Aman",
                "Rahul"
            )
        );

    names.sort(comparator);

Result:

    Aman
    Rahul
    Yashu

String's own `compareTo()` is being used to define the Comparator's ordering.

---

# 12. Comparator with Custom Objects 🧑‍💻

Suppose:

    class Student {

        String name;
        int age;
        int marks;

        Student(
            String name,
            int age,
            int marks
        ) {
            this.name = name;
            this.age = age;
            this.marks = marks;
        }
    }

We can sort by age:

    Comparator<Student> byAge =
        (s1, s2) ->
            Integer.compare(
                s1.age,
                s2.age
            );

Sort by marks:

    Comparator<Student> byMarks =
        (s1, s2) ->
            Integer.compare(
                s1.marks,
                s2.marks
            );

Sort by name:

    Comparator<Student> byName =
        (s1, s2) ->
            s1.name.compareTo(
                s2.name
            );

Same class.

Different ordering strategies.

---

# 13. Using a Separate Comparator Class 🏗️

Example:

    class Student {

        String name;
        int age;

        Student(
            String name,
            int age
        ) {
            this.name = name;
            this.age = age;
        }
    }

    class AgeComparator
            implements Comparator<Student> {

        @Override
        public int compare(
                Student s1,
                Student s2) {

            return Integer.compare(
                s1.age,
                s2.age
            );
        }
    }

Usage:

    students.sort(
        new AgeComparator()
    );

This approach is useful when the comparison logic is:

    reusable
    complex
    meaningful as a separate component

---

# 14. Using Anonymous Class 🏗️

Before lambda expressions, Comparator was commonly used with anonymous classes.

Example:

    Comparator<Student> byAge =
        new Comparator<Student>() {

            @Override
            public int compare(
                    Student s1,
                    Student s2) {

                return Integer.compare(
                    s1.age,
                    s2.age
                );
            }
        };

This works but is more verbose than a lambda.

---

# 15. Using Lambda Expression ⚡

Because Comparator is a functional interface, we can use lambda expressions.

Instead of:

    new Comparator<Student>() {

        @Override
        public int compare(
                Student s1,
                Student s2) {

            return Integer.compare(
                s1.age,
                s2.age
            );
        }
    }

we can write:

    (s1, s2) ->
        Integer.compare(
            s1.age,
            s2.age
        );

This is one of the most common modern Java approaches.

---

# 16. Comparator with Collections.sort() 🔃

Example:

    List<Student> students =
        new ArrayList<>();

    students.add(
        new Student("A", 25, 80)
    );

    students.add(
        new Student("B", 20, 90)
    );

    students.add(
        new Student("C", 22, 85)
    );

Sort by age:

    Collections.sort(
        students,
        (s1, s2) ->
            Integer.compare(
                s1.age,
                s2.age
            )
    );

The Comparator defines the ordering.

---

# 17. Comparator with List.sort() 📋

Modern Java provides:

    list.sort(comparator)

Example:

    students.sort(
        (s1, s2) ->
            Integer.compare(
                s1.age,
                s2.age
            )
    );

This is concise and commonly preferred when sorting a List.

---

# 18. Comparator with Arrays.sort() 🔢

Comparator can also be used with object arrays.

Example:

    Student[] students = {
        new Student("A", 25),
        new Student("B", 20),
        new Student("C", 22)
    };

    Arrays.sort(
        students,
        (s1, s2) ->
            Integer.compare(
                s1.age,
                s2.age
            )
    );

The array is sorted according to the supplied Comparator.

---

# 19. Comparator with TreeSet 🌳

`TreeSet` can accept a Comparator.

Example:

    TreeSet<Student> students =
        new TreeSet<>(
            (s1, s2) ->
                Integer.compare(
                    s1.age,
                    s2.age
                )
        );

Now TreeSet uses:

    age

for ordering.

Important:

The Comparator becomes part of the ordering used by the TreeSet.

If:

    compare(s1, s2) == 0

the TreeSet can consider them equivalent for its set semantics.

---

# 20. Comparator with TreeMap 🗺️

`TreeMap` can accept a Comparator for its keys.

Example:

    TreeMap<Integer, String> map =
        new TreeMap<>(
            (a, b) ->
                Integer.compare(b, a)
        );

Now keys are maintained in:

    descending order

Example:

    map.put(10, "A");
    map.put(20, "B");
    map.put(30, "C");

Key order:

    30
    20
    10

---

# 21. Ascending Order ⬆️

For numbers:

    Comparator<Integer> ascending =
        (a, b) ->
            Integer.compare(a, b);

For objects:

    Comparator<Student> byAge =
        (s1, s2) ->
            Integer.compare(
                s1.age,
                s2.age
            );

Ascending means:

    smaller
       ->
    larger

Example:

    10
    20
    30

---

# 22. Descending Order ⬇️

Reverse the arguments:

    Comparator<Integer> descending =
        (a, b) ->
            Integer.compare(b, a);

For students:

    Comparator<Student> byAgeDescending =
        (s1, s2) ->
            Integer.compare(
                s2.age,
                s1.age
            );

Result:

    30
    20
    10

---

# 23. Comparing Objects by Different Fields 🔀

Suppose:

    class Employee {

        String name;
        int age;
        double salary;
    }

We can define:

By name:

    Comparator<Employee> byName =
        (e1, e2) ->
            e1.name.compareTo(e2.name);

By age:

    Comparator<Employee> byAge =
        (e1, e2) ->
            Integer.compare(
                e1.age,
                e2.age
            );

By salary:

    Comparator<Employee> bySalary =
        (e1, e2) ->
            Double.compare(
                e1.salary,
                e2.salary
            );

One class.

Multiple orderings.

This is a major reason to use Comparator.

---

# 24. Comparator.comparing() 🧠

Java provides a convenient factory method:

    Comparator.comparing()

Instead of:

    (s1, s2) ->
        Integer.compare(
            s1.age,
            s2.age
        )

we can write:

    Comparator<Student> byAge =
        Comparator.comparing(
            student -> student.age
        );

For objects:

    Comparator<Employee> byName =
        Comparator.comparing(
            employee -> employee.name
        );

This makes comparator creation concise.

---

# 25. thenComparing() 🔗

`thenComparing()` allows multiple comparison levels.

Suppose:

    Student

has:

    name
    age
    marks

First sort by:

    marks

If marks are equal, sort by:

    name

Example:

    Comparator<Student> comparator =
        Comparator.comparing(
            (Student s) -> s.marks
        ).thenComparing(
            s -> s.name
        );

Concept:

    marks
       |
       +---- different
       |       |
       |       v
       |     final order
       |
       +---- same
               |
               v
              name

---

# 26. reversed() 🔄

`reversed()` returns a Comparator with the reverse ordering.

Example:

    Comparator<Integer> ascending =
        Comparator.naturalOrder();

    Comparator<Integer> descending =
        ascending.reversed();

Result:

    ascending:
        10 20 30

    descending:
        30 20 10

For objects:

    Comparator<Student> byAge =
        Comparator.comparing(
            (Student s) -> s.age
        );

    Comparator<Student> byAgeDescending =
        byAge.reversed();

---

# 27. nullsFirst() 🟡

`nullsFirst()` places `null` before non-null values.

Example:

    Comparator<String> comparator =
        Comparator.nullsFirst(
            Comparator.naturalOrder()
        );

Conceptual result:

    null
    "A"
    "B"
    "C"

This is useful when collections may contain null values.

---

# 28. nullsLast() 🟡

`nullsLast()` places `null` after non-null values.

Example:

    Comparator<String> comparator =
        Comparator.nullsLast(
            Comparator.naturalOrder()
        );

Conceptual result:

    "A"
    "B"
    "C"
    null

---

# 29. naturalOrder() 🌿

`Comparator.naturalOrder()` returns a Comparator that uses the natural ordering of elements.

Example:

    Comparator<Integer> comparator =
        Comparator.naturalOrder();

Equivalent conceptually to:

    Integer.compare(a, b)

for Integer values.

The type must have a compatible natural ordering.

---

# 30. reverseOrder() 🔙

`Comparator.reverseOrder()` returns a Comparator that uses reverse natural ordering.

Example:

    Comparator<Integer> comparator =
        Comparator.reverseOrder();

Result:

    30
    20
    10

For Strings:

    Comparator<String> comparator =
        Comparator.reverseOrder();

This reverses their natural lexicographical ordering.

---

# 31. Comparing Primitive Fields 🔢

For primitive fields, Java provides specialized methods.

### int

    Comparator.comparingInt(
        student -> student.age
    );

### long

    Comparator.comparingLong(
        employee -> employee.id
    );

### double

    Comparator.comparingDouble(
        employee -> employee.salary
    );

These avoid unnecessary boxing in the key-extraction step.

Example:

    Comparator<Student> byAge =
        Comparator.comparingInt(
            student -> student.age
        );

---

# 32. Comparator and Multiple Fields 🔀

Suppose:

    Student

contains:

    name
    age
    marks

Requirement:

    1. Sort by marks
    2. If marks equal, sort by age
    3. If age equal, sort by name

We can write:

    Comparator<Student> comparator =
        Comparator.comparingInt(
            (Student s) -> s.marks
        )
        .thenComparingInt(
            s -> s.age
        )
        .thenComparing(
            s -> s.name
        );

This is called:

    Multi-level sorting

---

# 33. Comparator and Tie-Breaking 🧩

Suppose:

    Student A
        marks = 90
        age = 20

    Student B
        marks = 90
        age = 22

Primary comparison:

    marks

Both have:

    90

So:

    compare() == 0

for the first criterion.

Then:

    thenComparing()

can compare:

    age

Result:

    Student A
        <
    Student B

This is called:

    Tie-breaking

---

# 34. Comparator and Comparable ⚖️

This is one of the most important comparisons in Java.

| Comparable | Comparator |
|---|---|
| `java.lang` | `java.util` |
| Interface | Interface |
| `compareTo()` | `compare()` |
| Natural ordering | Custom ordering |
| Ordering defined by class | Ordering defined externally |
| Usually one natural order | Multiple orderings possible |
| Modifies class | Does not require modifying class |
| Used by default when natural order is needed | Supplied explicitly when custom order is needed |
| `Comparable<T>` | `Comparator<T>` |

Memory trick:

    Comparable
        ->
    "I compare myself."

    Comparator
        ->
    "I compare two objects."

---

# 35. Internal Working ⚙️

Suppose:

    students:

    A -> age 25
    B -> age 20
    C -> age 22

Comparator:

    (s1, s2) ->
        Integer.compare(
            s1.age,
            s2.age
        )

When sorting needs to compare two students:

    compare(A, B)

becomes conceptually:

    compare(25, 20)

Result:

    positive

Therefore:

    B should come before A

Another comparison:

    compare(C, A)

becomes:

    compare(22, 25)

Result:

    negative

Therefore:

    C should come before A

The sorting algorithm uses these comparison results to arrange the elements.

Important:

> Comparator does not itself perform the sorting algorithm.

It supplies the ordering rule.

---

# 36. Comparator Contract 📜

A Comparator should define a consistent ordering.

Important properties include:

### Sign consistency

If:

    compare(a, b) < 0

then:

    compare(b, a) > 0

in the corresponding reverse comparison.

---

### Transitivity

If:

    compare(a, b) < 0

and:

    compare(b, c) < 0

then:

    compare(a, c) < 0

should also hold.

---

### Consistency

The comparison should not randomly change for the same objects unless the underlying state itself changes.

A broken Comparator can cause incorrect sorting or unexpected behavior in ordered collections.

---

# 37. Consistency with equals() ⚠️

A Comparator can be:

    consistent with equals

or:

    inconsistent with equals

Ideally, if:

    compare(a, b) == 0

then:

    a.equals(b)

should also be true.

But this is not mandatory.

Important with:

    TreeSet
    TreeMap

because they use ordering to organize elements/keys.

If:

    comparator.compare(a, b) == 0

the sorted collection may treat them as equivalent for its ordering semantics even when:

    a.equals(b)

is false.

---

# 38. Comparator and Generics 🧬

Use:

    Comparator<Student>

instead of raw:

    Comparator

Example:

    Comparator<Student> byAge =
        (s1, s2) ->
            Integer.compare(
                s1.age,
                s2.age
            );

Generics provide:

    Type Safety
    Better compiler checking
    No unnecessary casts

---

# 39. Comparator as Functional Interface ⚡

`Comparator` is a functional interface.

It has one abstract method:

    compare()

Therefore we can use:

    Lambda Expressions

and:

    Method References

Example:

    Comparator<Integer> comparator =
        (a, b) ->
            Integer.compare(a, b);

This is possible because Comparator is functional.

---

# 40. Method References 🔗

Suppose:

    List<String> names =
        new ArrayList<>(
            List.of(
                "Java",
                "Python",
                "C"
            )
        );

We can use:

    names.sort(
        Comparator.naturalOrder()
    );

For custom objects:

    Comparator.comparing(
        Student::getName
    );

If:

    Student

has:

    getName()

then:

    Student::getName

is a method reference used for extracting the comparison key.

---

# 41. Common Mistakes ⚠️

## Mistake 1

Confusing:

    compare()

with:

    compareTo()

Remember:

    Comparator
        -> compare()

    Comparable
        -> compareTo()

---

## Mistake 2

Thinking Comparator is a class.

It is an interface.

---

## Mistake 3

Thinking Comparator sorts objects itself.

It only defines comparison logic.

---

## Mistake 4

Using subtraction blindly:

    return a.age - b.age;

This can overflow for extreme integer values.

Prefer:

    Integer.compare(
        a.age,
        b.age
    );

---

## Mistake 5

Creating many Comparator classes unnecessarily.

For simple comparisons, lambdas are often cleaner.

---

## Mistake 6

Ignoring tie-breaking.

If the first comparison returns zero, use:

    thenComparing()

when another ordering criterion is required.

---

## Mistake 7

Writing inconsistent comparison logic.

A broken Comparator can cause incorrect ordering.

---

## Mistake 8

Forgetting that `TreeSet` and `TreeMap` use Comparator ordering when one is supplied.

---

# 42. Interview Traps 🎤

## Trap 1

> Which package contains Comparator?

    java.util

---

## Trap 2

> What is the main method?

    compare()

---

## Trap 3

> What is the return type?

    int

---

## Trap 4

> Is Comparator a functional interface?

Yes.

---

## Trap 5

> Can Comparator be used with lambda expressions?

Yes.

---

## Trap 6

> Can a class have multiple Comparators?

Yes.

This is one of its biggest advantages.

---

## Trap 7

> Does Comparator define natural ordering?

No.

Comparable defines natural ordering.

---

## Trap 8

> Can Comparator sort without a sorting mechanism?

No.

It provides comparison logic to sorting/ordered collection mechanisms.

---

## Trap 9

> Can Comparator be used with TreeSet?

Yes.

A Comparator can be supplied to TreeSet.

---

## Trap 10

> Can Comparator be used with TreeMap?

Yes.

A Comparator can be supplied for key ordering.

---

## Trap 11

> What does compare(a, b) == 0 mean?

The two objects are equivalent according to that Comparator's ordering.

---

## Trap 12

> Is compare(a,b) == 0 always the same as a.equals(b)?

No.

They can be inconsistent.

---

## Trap 13

> What is thenComparing() used for?

For additional comparison criteria when previous criteria are equal.

---

## Trap 14

> What does reversed() do?

Returns a Comparator with the reverse ordering.

---

## Trap 15

> What does nullsFirst() do?

Places null values before non-null values.

---

## Trap 16

> What does nullsLast() do?

Places null values after non-null values.

---

## Trap 17

> What does naturalOrder() do?

Returns a Comparator using the elements' natural ordering.

---

## Trap 18

> What does reverseOrder() do?

Returns a Comparator using reverse natural ordering.

---

# 43. DSA Patterns 🧩

Comparator is extremely useful in DSA whenever a problem requires ordering based on a custom rule.

Major patterns:

    1. Custom sorting
    2. Descending sorting
    3. Multi-level sorting
    4. Greedy algorithms
    5. PriorityQueue ordering
    6. TreeSet ordering
    7. TreeMap ordering
    8. Interval sorting
    9. Object sorting
    10. Min/Max based on custom properties

---

# 44. DSA Pattern 1 — Custom Sorting 🔃

Suppose we have:

    intervals:

    [5, 10]
    [1, 3]
    [2, 7]

Many interval problems require sorting by:

    start time

Comparator:

    Arrays.sort(
        intervals,
        (a, b) ->
            Integer.compare(
                a[0],
                b[0]
            )
    );

Now:

    [1, 3]
    [2, 7]
    [5, 10]

This pattern appears frequently in:

    Interval problems
    Greedy algorithms
    Scheduling
    Meeting problems

---

# 45. DSA Pattern 2 — Descending Order ⬇️

Suppose:

    [10, 20, 30]

Need:

    30, 20, 10

Use:

    Arrays.sort(
        numbers,
        Comparator.reverseOrder()
    );

For primitive arrays such as:

    int[]

you cannot directly pass a Comparator to `Arrays.sort()`.

Comparator-based sorting applies to reference/object arrays.

For:

    Integer[]

it works.

---

# 46. DSA Pattern 3 — Multi-Level Sorting 🔀

Common problem:

    Sort employees by:
        salary descending

    If salary same:
        name ascending

Comparator:

    Comparator<Employee> comparator =
        Comparator.comparingDouble(
            (Employee e) -> e.salary
        )
        .reversed()
        .thenComparing(
            e -> e.name
        );

This is a very common interview pattern.

---

# 47. DSA Pattern 4 — Priority Ordering 🏆

`PriorityQueue` can accept a Comparator.

Example:

    PriorityQueue<Integer> pq =
        new PriorityQueue<>(
            Comparator.reverseOrder()
        );

Now it behaves as a max-priority queue:

    30
    20
    10

For custom objects:

    PriorityQueue<Student> pq =
        new PriorityQueue<>(
            Comparator.comparingInt(
                student -> student.marks
            )
        );

The student with the smallest marks has highest priority according to this ordering.

---

# 48. DSA Pattern 5 — TreeSet Custom Ordering 🌳

Example:

    TreeSet<Integer> set =
        new TreeSet<>(
            Comparator.reverseOrder()
        );

Add:

    10
    30
    20

Result:

    [30, 20, 10]

Comparator determines the ordering.

This concept is useful for:

    Sorted data
    Ordered sets
    Range queries
    Floor/ceiling operations

---

# 49. Top Interview Questions 🎤

## Q1. What is Comparator?

Comparator is an interface in `java.util` used to define custom or external ordering between objects.

---

## Q2. What is its main method?

    compare(T o1, T o2)

---

## Q3. What does compare() return?

    int

---

## Q4. What does a negative result mean?

`o1` comes before `o2`.

---

## Q5. What does zero mean?

`o1` and `o2` are equivalent according to the Comparator's ordering.

---

## Q6. What does a positive result mean?

`o1` comes after `o2`.

---

## Q7. Why use Comparator?

To define custom ordering without modifying the class and to support multiple sorting strategies.

---

## Q8. Difference between Comparable and Comparator?

Comparable defines natural ordering through `compareTo()`.

Comparator defines custom ordering through `compare()`.

---

## Q9. Can a class have multiple Comparators?

Yes.

---

## Q10. Can a class have multiple natural orderings using Comparable?

Normally no. A class has one `compareTo()` implementation representing its natural ordering.

---

## Q11. Is Comparator a functional interface?

Yes.

---

## Q12. Can Comparator be implemented using lambda?

Yes.

Example:

    (a, b) ->
        Integer.compare(a, b)

---

## Q13. What is Comparator.comparing()?

It creates a Comparator based on a key extracted from an object.

Example:

    Comparator.comparing(
        Student::getAge
    );

---

## Q14. What is thenComparing()?

It adds another comparison criterion when the previous comparison results in equality.

---

## Q15. What is reversed()?

It returns a Comparator with reversed ordering.

---

## Q16. What is naturalOrder()?

It returns a Comparator using natural ordering.

---

## Q17. What is reverseOrder()?

It returns a Comparator using reverse natural ordering.

---

## Q18. What is nullsFirst()?

It creates a Comparator that places null before non-null values.

---

## Q19. What is nullsLast()?

It creates a Comparator that places null after non-null values.

---

## Q20. Can Comparator be used with TreeSet?

Yes.

---

## Q21. Can Comparator be used with TreeMap?

Yes.

---

## Q22. Can Comparator be used with PriorityQueue?

Yes.

---

## Q23. Can Comparator be used with Arrays.sort()?

Yes, for object/reference arrays.

---

## Q24. Can Comparator be used with primitive int[]?

Not directly with the Comparator overload.

Use:

    Integer[]

if Comparator-based ordering is required.

---

## Q25. Why should we avoid `a - b` in compare()?

Because subtraction can overflow.

Use:

    Integer.compare(a, b)

instead.

---

## Q26. Does Comparator itself perform sorting?

No.

It defines the comparison logic used by sorting or ordered collection mechanisms.

---

## Q27. Can compare() return any negative number?

Yes.

Only the sign is generally important.

---

## Q28. Can compare() return any positive number?

Yes.

Again, the sign is what matters.

---

## Q29. What happens when compare(a,b) returns zero in TreeSet?

The elements are considered equivalent according to the TreeSet's ordering and may not both be stored.

---

## Q30. What happens when compare(k1,k2) returns zero in TreeMap?

The keys are considered equivalent according to the map's ordering, so they refer to the same ordering position/key for the map.

---

# 50. 30-Second Interview Answer 🎯

If the interviewer asks:

> "What is Comparator in Java?"

Answer:

> "`Comparator` is a functional interface from `java.util` used to define custom or external ordering between objects. Its main method is `compare(T o1, T o2)`, which returns a negative value, zero, or a positive value depending on their ordering. Unlike Comparable, which defines a class's natural ordering through `compareTo()`, Comparator allows us to create multiple sorting strategies without modifying the class. It can be used with sorting methods, TreeSet, TreeMap, PriorityQueue, and other ordered structures."

---

# 51. Cheat Sheet 📋

## Interface

    Comparator<T>

---

## Package

    java.util

---

## Functional Interface

    Yes

---

## Main Method

    compare(T o1, T o2)

---

## Return Type

    int

---

## Meaning

    negative
        ->
    o1 before o2

    0
        ->
    same ordering position

    positive
        ->
    o1 after o2

---

## Lambda

    (a, b) ->
        Integer.compare(a, b)

---

## Natural Order

    Comparator.naturalOrder()

---

## Reverse Natural Order

    Comparator.reverseOrder()

---

## Reverse Existing Comparator

    comparator.reversed()

---

## Key-Based Comparison

    Comparator.comparing(
        object -> object.field
    )

---

## Primitive int Key

    Comparator.comparingInt(
        object -> object.intField
    )

---

## Primitive long Key

    Comparator.comparingLong(
        object -> object.longField
    )

---

## Primitive double Key

    Comparator.comparingDouble(
        object -> object.doubleField
    )

---

## Multiple Criteria

    comparator
        .thenComparing(...)

---

## Null First

    Comparator.nullsFirst(...)

---

## Null Last

    Comparator.nullsLast(...)

---

## List Sorting

    list.sort(comparator);

---

## Collections Sorting

    Collections.sort(
        list,
        comparator
    );

---

## Array Sorting

    Arrays.sort(
        array,
        comparator
    );

---

## PriorityQueue

    new PriorityQueue<>(
        comparator
    );

---

## TreeSet

    new TreeSet<>(
        comparator
    );

---

## TreeMap

    new TreeMap<>(
        comparator
    );

---

# 52. Quick Revision ⚡

Remember:

    1. Comparator is an interface.

    2. Package:
       java.util

    3. Comparator is a functional interface.

    4. Main method:
       compare()

    5. Return type:
       int

    6. Negative:
       first object comes before second.

    7. Zero:
       same ordering position.

    8. Positive:
       first object comes after second.

    9. Comparator defines custom ordering.

    10. Comparator does not define
        natural ordering.

    11. Comparable defines natural ordering.

    12. Comparable:
        compareTo()

    13. Comparator:
        compare()

    14. Comparable is implemented by
        the class being compared.

    15. Comparator can exist outside
        the class.

    16. A class can have many Comparators.

    17. Comparator works very well with
        lambda expressions.

    18. Comparator.comparing()
        creates key-based ordering.

    19. thenComparing()
        provides tie-breaking.

    20. reversed()
        reverses ordering.

    21. naturalOrder()
        uses natural ordering.

    22. reverseOrder()
        uses reverse natural ordering.

    23. nullsFirst()
        places null first.

    24. nullsLast()
        places null last.

    25. comparingInt()
        compares int keys.

    26. comparingLong()
        compares long keys.

    27. comparingDouble()
        compares double keys.

    28. Comparator can be used with
        List.sort().

    29. Comparator can be used with
        Collections.sort().

    30. Comparator can be used with
        object arrays.

    31. Comparator can be supplied to
        TreeSet.

    32. Comparator can be supplied to
        TreeMap.

    33. Comparator can be supplied to
        PriorityQueue.

    34. Avoid:
        a - b

        because of possible integer
        overflow.

    35. Prefer:
        Integer.compare(a, b)

    36. compare(a,b) == 0 does not
        necessarily mean equals() == true.

    37. Comparator ordering should be
        transitive and consistent.

    38. Broken comparison logic can cause
        incorrect ordering.

    39. Comparator is heavily used in DSA
        custom sorting.

    40. Comparator is especially important
        for:
        sorting
        greedy algorithms
        intervals
        PriorityQueue
        TreeSet
        TreeMap

---

# Final Memory Trick 🧠

Remember the simplest distinction:

    Comparable
        |
        v
    compareTo()
        |
        v
    "I define my natural order."

    Comparator
        |
        v
    compare()
        |
        v
    "I define how these two objects
     should be ordered."

For multiple sorting strategies:

    Student
       |
       +---- byAge
       |
       +---- byName
       |
       +---- byMarks
       |
       +---- bySalary

That is the real power of:

    Comparator