# 🔥 08 — Collectors

Collectors are one of the most important parts of the Java Stream API.

They are mainly used with the terminal operation:

```java
collect()
```

to transform Stream elements into useful results such as:

```text
List
Set
Map
String
Grouped data
Partitioned data
Statistics
Summaries
```

> 💡 **Core idea:** A Stream processes data; a Collector describes how the processed data should be accumulated into a final result.

---

# 📌 Table of Contents

1. [Introduction](#1-introduction)
2. [What is a Collector?](#2-what-is-a-collector)
3. [collect() vs Collector](#3-collect-vs-collector)
4. [Why Collectors are Needed](#4-why-collectors-are-needed)
5. [Collectors.toList()](#5-collectorstolist)
6. [Collectors.toSet()](#6-collectorstoset)
7. [Collectors.toCollection()](#7-collectorstocollection)
8. [Collectors.toMap()](#8-collectorstomap)
9. [Duplicate Keys in toMap()](#9-duplicate-keys-in-tomap)
10. [Custom Merge Function](#10-custom-merge-function)
11. [Mapping with toMap()](#11-mapping-with-tomap)
12. [joining()](#12-joining)
13. [joining() with Delimiter](#13-joining-with-delimiter)
14. [joining() with Prefix and Suffix](#14-joining-with-prefix-and-suffix)
15. [counting()](#15-counting)
16. [summingInt()](#16-summingint)
17. [averagingInt()](#17-averagingint)
18. [summarizingInt()](#18-summarizingint)
19. [maxBy() and minBy()](#19-maxby-and-minby)
20. [groupingBy()](#20-groupingby)
21. [groupingBy() with Mapping](#21-groupingby-with-mapping)
22. [groupingBy() with Counting](#22-groupingby-with-counting)
23. [Nested groupingBy()](#23-nested-groupingby)
24. [partitioningBy()](#24-partitioningby)
25. [partitioningBy() with Downstream Collector](#25-partitioningby-with-downstream-collector)
26. [collectingAndThen()](#26-collectingandthen)
27. [filtering()](#27-filtering)
28. [flatMapping()](#28-flatmapping)
29. [teeing()](#29-teeing)
30. [Collector Characteristics](#30-collector-characteristics)
31. [Collector vs reduce()](#31-collector-vs-reduce)
32. [Collectors and Parallel Streams](#32-collectors-and-parallel-streams)
33. [Real-World Examples](#33-real-world-examples)
34. [DSA Connection](#34-dsa-connection)
35. [Common Mistakes](#35-common-mistakes)
36. [Interview Traps](#36-interview-traps)
37. [Top 20 Interview Questions](#37-top-20-interview-questions)
38. [30-Second Interview Answer](#38-30-second-interview-answer)
39. [Cheat Sheet](#39-cheat-sheet)
40. [Final Takeaway](#40-final-takeaway)

---

# 1. Introduction

Suppose we have:

```java
List<Integer> numbers = List.of(
        1, 2, 3, 4, 5
);
```

We can create a Stream:

```java
numbers.stream()
```

and process it:

```java
numbers.stream()
        .filter(n -> n % 2 == 0)
```

But eventually we usually need a final result.

For example:

```text
Stream
   ↓
List
```

or:

```text
Stream
   ↓
Set
```

or:

```text
Stream
   ↓
Map
```

or:

```text
Stream
   ↓
String
```

This is where `Collectors` become useful.

---

# 2. What is a Collector?

`Collector<T, A, R>` is an abstraction that describes how Stream elements are accumulated into a final result.

Conceptually:

```text
T = input element type
A = mutable accumulation type
R = final result type
```

For example:

```text
Stream<Employee>
       ↓
   Collector
       ↓
List<Employee>
```

The `Collectors` utility class provides many ready-made collectors.

```java
Collectors.toList()
Collectors.toSet()
Collectors.toMap()
Collectors.groupingBy()
Collectors.partitioningBy()
Collectors.joining()
```

---

# 3. collect() vs Collector

These two terms are easy to confuse.

## collect()

`collect()` is a **terminal Stream operation**.

Example:

```java
List<Integer> result = numbers.stream()
        .collect(Collectors.toList());
```

---

## Collector

A `Collector` describes how the elements should be accumulated.

Here:

```java
Collectors.toList()
```

returns a Collector.

So:

```text
collect()
   ↓
executes the collection process

Collectors.toList()
   ↓
describes how to collect
```

---

## Simple Mental Model

```text
Stream
   ↓
collect()
   ↓
Collector
   ↓
Final Result
```

---

# 4. Why Collectors are Needed

Without Collectors, you can manually collect results.

Example:

```java
List<Integer> result = new ArrayList<>();

for (Integer number : numbers) {
    if (number % 2 == 0) {
        result.add(number);
    }
}
```

Using Stream API:

```java
List<Integer> result = numbers.stream()
        .filter(n -> n % 2 == 0)
        .collect(Collectors.toList());
```

Collectors provide reusable accumulation strategies.

---

# 5. Collectors.toList()

## Definition

`Collectors.toList()` collects Stream elements into a `List`.

Example:

```java
List<Integer> result = numbers.stream()
        .filter(n -> n % 2 == 0)
        .collect(Collectors.toList());

System.out.println(result);
```

Output:

```text
[2, 4]
```

---

## Important Modern Alternative

Modern Java also provides:

```java
.toList()
```

Example:

```java
List<Integer> result = numbers.stream()
        .filter(n -> n % 2 == 0)
        .toList();
```

Both are useful, but they are not identical in every property.

`Stream.toList()` returns an unmodifiable List.

`Collectors.toList()` does not promise a specific List implementation or mutability characteristic.

Therefore, do not rely on:

```java
Collectors.toList()
```

being mutable unless you explicitly choose the collection type.

---

# 6. Collectors.toSet()

`toSet()` collects elements into a Set.

Example:

```java
List<Integer> numbers = List.of(
        1, 2, 2, 3, 3, 4
);

Set<Integer> result = numbers.stream()
        .collect(Collectors.toSet());

System.out.println(result);
```

Output contains:

```text
[1, 2, 3, 4]
```

The Set removes duplicates according to Set equality semantics.

---

## Important

Do not rely on a particular Set implementation or iteration order when using:

```java
Collectors.toSet()
```

If you need a specific collection type, use:

```java
Collectors.toCollection(...)
```

---

# 7. Collectors.toCollection()

Use `toCollection()` when you want to specify the collection implementation.

### LinkedHashSet

```java
Set<Integer> result = numbers.stream()
        .collect(
                Collectors.toCollection(LinkedHashSet::new)
        );
```

This is useful when you specifically want `LinkedHashSet`.

---

## TreeSet

```java
Set<Integer> result = numbers.stream()
        .collect(
                Collectors.toCollection(TreeSet::new)
        );
```

Now the result is a `TreeSet`.

Therefore:

```text
toSet()
        ↓
Set implementation not specified

toCollection(TreeSet::new)
        ↓
TreeSet explicitly requested
```

---

# 8. Collectors.toMap()

`toMap()` collects Stream elements into a `Map`.

Basic form:

```java
Collectors.toMap(keyMapper, valueMapper)
```

Example:

```java
record Student(
        int id,
        String name
) {
}
```

```java
List<Student> students = List.of(
        new Student(1, "Aman"),
        new Student(2, "Rahul"),
        new Student(3, "Alex")
);
```

Create:

```text
ID → Student Name
```

```java
Map<Integer, String> result = students.stream()
        .collect(
                Collectors.toMap(
                        Student::id,
                        Student::name
                )
        );

System.out.println(result);
```

Output:

```text
{1=Aman, 2=Rahul, 3=Alex}
```

---

# 9. Duplicate Keys in toMap()

This is one of the most important Collector interview traps.

Suppose:

```java
List<Student> students = List.of(
        new Student(1, "Aman"),
        new Student(1, "Rahul")
);
```

Now:

```java
Map<Integer, String> result = students.stream()
        .collect(
                Collectors.toMap(
                        Student::id,
                        Student::name
                )
        );
```

There are two values for key:

```text
1
```

The collector does not know which value should win.

Therefore, this operation throws:

```text
IllegalStateException
```

because of the duplicate key.

---

# 10. Custom Merge Function

To handle duplicate keys, use the three-argument form:

```java
Collectors.toMap(
        keyMapper,
        valueMapper,
        mergeFunction
)
```

Example:

```java
Map<Integer, String> result = students.stream()
        .collect(
                Collectors.toMap(
                        Student::id,
                        Student::name,
                        (oldValue, newValue) -> oldValue
                )
        );
```

Here:

```text
oldValue → existing value
newValue → new value
```

---

## Keep New Value

```java
Map<Integer, String> result = students.stream()
        .collect(
                Collectors.toMap(
                        Student::id,
                        Student::name,
                        (oldValue, newValue) -> newValue
                )
        );
```

---

## Merge Strings

```java
record Employee(
        int id,
        String name
) {
}
```

```java
List<Employee> employees = List.of(
        new Employee(1, "Aman"),
        new Employee(1, "Rahul")
);
```

```java
Map<Integer, String> result = employees.stream()
        .collect(
                Collectors.toMap(
                        Employee::id,
                        Employee::name,
                        (a, b) -> a + ", " + b
                )
        );
```

Result:

```text
{1=Aman, Rahul}
```

---

# 11. Mapping with toMap()

You can transform values while building a Map.

Example:

```java
Map<Integer, String> result = students.stream()
        .collect(
                Collectors.toMap(
                        Student::id,
                        student -> student.name().toUpperCase()
                )
        );
```

Result:

```text
{
    1=Aman,
    2=Rahul,
    3=Alex
}
```

Conceptually:

```text
Student
   ↓
key = id
value = uppercase name
```

---

# 12. joining()

`Collectors.joining()` combines Strings into one String.

Example:

```java
List<String> names = List.of(
        "Aman",
        "Rahul",
        "Alex"
);

String result = names.stream()
        .collect(Collectors.joining());

System.out.println(result);
```

Output:

```text
AmanRahulAlex
```

---

# 13. joining() with Delimiter

Use:

```java
Collectors.joining(", ")
```

Example:

```java
String result = names.stream()
        .collect(Collectors.joining(", "));

System.out.println(result);
```

Output:

```text
Aman, Rahul, Alex
```

---

## Prefix and Suffix

```java
String result = names.stream()
        .collect(
                Collectors.joining(
                        ", ",
                        "[",
                        "]"
                )
        );

System.out.println(result);
```

Output:

```text
[Aman, Rahul, Alex]
```

---

## Syntax

```java
Collectors.joining()
```

```java
Collectors.joining(delimiter)
```

```java
Collectors.joining(
        delimiter,
        prefix,
        suffix
)
```

---

# 14. counting()

`counting()` counts the elements passed to the collector.

Example:

```java
long count = numbers.stream()
        .collect(Collectors.counting());

System.out.println(count);
```

Output:

```text
5
```

For a simple Stream count, this is usually more direct:

```java
long count = numbers.stream()
        .count();
```

`Collectors.counting()` becomes especially useful as a **downstream collector**.

Example:

```java
Map<String, Long> countByDepartment = employees.stream()
        .collect(
                Collectors.groupingBy(
                        Employee::department,
                        Collectors.counting()
                )
        );
```

---

# 15. summingInt()

`summingInt()` calculates the sum of an integer-valued property.

Example:

```java
record Employee(
        String name,
        String department,
        int salary
) {
}
```

```java
List<Employee> employees = List.of(
        new Employee("Aman", "IT", 50000),
        new Employee("Rahul", "HR", 40000),
        new Employee("Alex", "IT", 60000)
);
```

Calculate total salary:

```java
int totalSalary = employees.stream()
        .collect(
                Collectors.summingInt(Employee::salary)
        );

System.out.println(totalSalary);
```

Output:

```text
150000
```

---

## Other Numeric Collectors

```java
Collectors.summingInt(...)
```

```java
Collectors.summingLong(...)
```

```java
Collectors.summingDouble(...)
```

---

# 16. averagingInt()

`averagingInt()` calculates the arithmetic average.

Example:

```java
double averageSalary = employees.stream()
        .collect(
                Collectors.averagingInt(Employee::salary)
        );

System.out.println(averageSalary);
```

Output:

```text
50000.0
```

---

## Other Averaging Collectors

```java
Collectors.averagingInt(...)
```

```java
Collectors.averagingLong(...)
```

```java
Collectors.averagingDouble(...)
```

---

# 17. summarizingInt()

`summarizingInt()` provides multiple statistics in one result.

Example:

```java
IntSummaryStatistics statistics = employees.stream()
        .collect(
                Collectors.summarizingInt(Employee::salary)
        );

System.out.println(statistics);
```

The result contains:

```text
count
sum
min
average
max
```

You can access them:

```java
System.out.println(statistics.getCount());
System.out.println(statistics.getSum());
System.out.println(statistics.getMin());
System.out.println(statistics.getAverage());
System.out.println(statistics.getMax());
```

---

## Why summarizingInt() is Useful

Instead of separately calculating:

```text
count
sum
min
average
max
```

you can collect all these statistics together.

---

# 18. maxBy() and minBy()

These collectors find maximum and minimum according to a Comparator.

Example:

```java
Optional<Employee> highestPaid = employees.stream()
        .collect(
                Collectors.maxBy(
                        Comparator.comparingInt(Employee::salary)
                )
        );
```

---

## Minimum

```java
Optional<Employee> lowestPaid = employees.stream()
        .collect(
                Collectors.minBy(
                        Comparator.comparingInt(Employee::salary)
                )
        );
```

---

## Why Optional?

The Stream may be empty.

Therefore:

```java
maxBy()
```

and:

```java
minBy()
```

return:

```text
Optional<T>
```

---

# 19. groupingBy()

`groupingBy()` is one of the most important collectors.

It groups elements according to a classification function.

Example:

```java
record Employee(
        String name,
        String department,
        int salary
) {
}
```

```java
Map<String, List<Employee>> result = employees.stream()
        .collect(
                Collectors.groupingBy(
                        Employee::department
                )
        );
```

Conceptually:

```text
IT
 ├── Aman
 └── Alex

HR
 └── Rahul
```

The resulting type is:

```text
Map<String, List<Employee>>
```

---

## How groupingBy() Works

Suppose:

```text
Aman  → IT
Rahul → HR
Alex  → IT
Ravi  → Sales
```

`groupingBy(Employee::department)` creates:

```text
IT    → [Aman, Alex]
HR    → [Rahul]
Sales → [Ravi]
```

---

# 20. groupingBy() with Mapping

Sometimes you don't want the entire object in each group.

Suppose we only want employee names grouped by department.

```java
Map<String, List<String>> result = employees.stream()
        .collect(
                Collectors.groupingBy(
                        Employee::department,
                        Collectors.mapping(
                                Employee::name,
                                Collectors.toList()
                        )
                )
        );
```

Result:

```text
{
    IT=[Aman, Alex],
    HR=[Rahul],
    Sales=[Ravi]
}
```

Pattern:

```text
groupingBy()
      ↓
mapping()
      ↓
toList()
```

---

# 21. groupingBy() with Counting

A very common interview problem:

> Count employees in each department.

```java
Map<String, Long> result = employees.stream()
        .collect(
                Collectors.groupingBy(
                        Employee::department,
                        Collectors.counting()
                )
        );
```

Result:

```text
{
    IT=2,
    HR=1,
    Sales=1
}
```

---

## Important Pattern

```text
groupingBy(classifier, downstreamCollector)
```

Here:

```text
classifier → department
downstream  → counting()
```

---

# 22. Nested groupingBy()

You can group by multiple properties.

Suppose:

```java
record Employee(
        String name,
        String department,
        String city
) {
}
```

We want:

```text
department
    ↓
city
    ↓
employees
```

Use:

```java
Map<String, Map<String, List<Employee>>> result =
        employees.stream()
                .collect(
                        Collectors.groupingBy(
                                Employee::department,
                                Collectors.groupingBy(
                                        Employee::city
                                )
                        )
                );
```

Conceptually:

```text
IT
 ├── Lucknow
 │    ├── Aman
 │    └── Alex
 │
 └── Delhi
      └── Ravi

HR
 └── Lucknow
      └── Rahul
```

---

# 23. partitioningBy()

`partitioningBy()` divides elements into exactly two groups based on a boolean condition.

The result is:

```text
Map<Boolean, List<T>>
```

Example:

```java
Map<Boolean, List<Integer>> result = numbers.stream()
        .collect(
                Collectors.partitioningBy(
                        n -> n % 2 == 0
                )
        );
```

Conceptually:

```text
true  → even numbers
false → odd numbers
```

---

## Example

For:

```text
[1, 2, 3, 4, 5, 6]
```

Result:

```text
true  → [2, 4, 6]
false → [1, 3, 5]
```

---

## groupingBy() vs partitioningBy()

Use:

```text
partitioningBy()
```

when the classification is simply:

```text
true / false
```

Use:

```text
groupingBy()
```

when there can be multiple categories.

Example:

```text
IT
HR
Sales
Finance
```

---

# 24. partitioningBy() with Downstream Collector

You can provide another Collector after partitioning.

Example:

```java
Map<Boolean, Long> result = numbers.stream()
        .collect(
                Collectors.partitioningBy(
                        n -> n % 2 == 0,
                        Collectors.counting()
                )
        );
```

Result:

```text
true  → 3
false → 3
```

---

# 25. collectingAndThen()

`collectingAndThen()` performs a collection and then applies another finishing function.

Syntax:

```java
Collectors.collectingAndThen(
        collector,
        finisher
)
```

Example:

```java
List<Integer> result = numbers.stream()
        .collect(
                Collectors.collectingAndThen(
                        Collectors.toList(),
                        list -> List.copyOf(list)
                )
        );
```

Conceptually:

```text
Stream
   ↓
toList()
   ↓
List
   ↓
finisher
   ↓
final result
```

---

## Example: Find Size After Collecting

```java
int size = numbers.stream()
        .collect(
                Collectors.collectingAndThen(
                        Collectors.toList(),
                        List::size
                )
        );
```

Result:

```text
5
```

---

# 26. filtering()

`Collectors.filtering()` is a downstream collector.

It allows filtering to happen within a collector context.

Example:

```java
Map<String, List<Employee>> result = employees.stream()
        .collect(
                Collectors.groupingBy(
                        Employee::department,
                        Collectors.filtering(
                                employee -> employee.salary() > 50000,
                                Collectors.toList()
                        )
                )
        );
```

This means:

```text
Group by department
        ↓
Within each group
        ↓
Keep employees with salary > 50000
```

---

# 27. flatMapping()

`Collectors.flatMapping()` is a downstream collector.

It is useful when each grouped element contains a collection.

Consider:

```java
record Employee(
        String department,
        List<String> skills
) {
}
```

We want unique skills per department.

```java
Map<String, Set<String>> result = employees.stream()
        .collect(
                Collectors.groupingBy(
                        Employee::department,
                        Collectors.flatMapping(
                                employee -> employee.skills().stream(),
                                Collectors.toSet()
                        )
                )
        );
```

Conceptually:

```text
groupingBy department
        ↓
flatMapping skills
        ↓
toSet
```

---

# 28. teeing()

`teeing()` combines two collectors and merges their results.

Conceptually:

```text
                    ┌── Collector 1
Stream ─────────────┤
                    └── Collector 2
                          ↓
                       merger
                          ↓
                     final result
```

Example:

```java
record Result(
        long count,
        int sum
) {
}
```

```java
Result result = numbers.stream()
        .collect(
                Collectors.teeing(
                        Collectors.counting(),
                        Collectors.summingInt(Integer::intValue),
                        Result::new
                )
        );
```

Now:

```java
result.count()
result.sum()
```

contain both results.

---

## Why teeing() is Useful

Suppose you want two different aggregate results from the same Stream traversal.

Instead of manually performing separate operations, `teeing()` combines two collectors.

---

# 29. Collector Characteristics

Collectors can have characteristics that describe how they behave.

Common characteristics include:

```text
CONCURRENT
UNORDERED
IDENTITY_FINISH
```

---

## IDENTITY_FINISH

Indicates that the accumulation type can be used directly as the final result.

---

## UNORDERED

Indicates that the Collector does not require preserving encounter order.

---

## CONCURRENT

Indicates that accumulation can support concurrent execution under the appropriate conditions.

---

## Important

Do not assume that every Collector is:

```text
concurrent
unordered
mutable
```

The characteristics depend on the specific Collector.

---

# 30. Collector vs reduce()

This is a very important interview comparison.

## reduce()

Best for combining values into one result.

Example:

```java
int sum = numbers.stream()
        .reduce(0, Integer::sum);
```

---

## collect()

Best for mutable result containers or structured aggregation.

Example:

```java
List<Integer> result = numbers.stream()
        .collect(Collectors.toList());
```

---

## Mental Model

```text
reduce()
    ↓
Many values → One value

collect()
    ↓
Many values → Result container / structured result
```

---

## Example

Sum:

```java
int sum = numbers.stream()
        .reduce(0, Integer::sum);
```

List:

```java
List<Integer> list = numbers.stream()
        .collect(Collectors.toList());
```

Map:

```java
Map<Integer, String> map = students.stream()
        .collect(
                Collectors.toMap(
                        Student::id,
                        Student::name
                )
        );
```

---

# 31. Collectors and Parallel Streams

Collectors are designed to work with Stream reduction, including parallel streams when the collector's characteristics and accumulation rules support it.

Example:

```java
Map<String, List<Employee>> result =
        employees.parallelStream()
                .collect(
                        Collectors.groupingBy(
                                Employee::department
                        )
                );
```

The Stream implementation can process partitions and combine partial results.

Conceptually:

```text
Input
  ↓
Split
  ↓
Partition 1 → collect
Partition 2 → collect
Partition 3 → collect
  ↓
Combine
  ↓
Final result
```

---

## Important

Parallel execution does not automatically make a pipeline faster.

For small collections, sequential processing can be simpler and sometimes faster.

Parallel Streams are most useful when:

```text
work is substantial
+
data is large enough
+
operations can be safely parallelized
```

---

# 32. Real-World Examples

## 32.1 Group Employees by Department

```java
Map<String, List<Employee>> result = employees.stream()
        .collect(
                Collectors.groupingBy(
                        Employee::department
                )
        );
```

---

## 32.2 Count Employees by Department

```java
Map<String, Long> result = employees.stream()
        .collect(
                Collectors.groupingBy(
                        Employee::department,
                        Collectors.counting()
                )
        );
```

---

## 32.3 Total Salary by Department

```java
Map<String, Integer> result = employees.stream()
        .collect(
                Collectors.groupingBy(
                        Employee::department,
                        Collectors.summingInt(
                                Employee::salary
                        )
                )
        );
```

---

## 32.4 Average Salary by Department

```java
Map<String, Double> result = employees.stream()
        .collect(
                Collectors.groupingBy(
                        Employee::department,
                        Collectors.averagingInt(
                                Employee::salary
                        )
                )
        );
```

---

## 32.5 Employee Names by Department

```java
Map<String, List<String>> result = employees.stream()
        .collect(
                Collectors.groupingBy(
                        Employee::department,
                        Collectors.mapping(
                                Employee::name,
                                Collectors.toList()
                        )
                )
        );
```

---

## 32.6 Highest Salary

```java
Optional<Employee> highestPaid = employees.stream()
        .collect(
                Collectors.maxBy(
                        Comparator.comparingInt(
                                Employee::salary
                        )
                )
        );
```

---

## 32.7 Employees Above Salary Threshold

```java
List<Employee> result = employees.stream()
        .filter(employee -> employee.salary() > 50000)
        .collect(Collectors.toList());
```

---

## 32.8 Unique Departments

```java
Set<String> departments = employees.stream()
        .map(Employee::department)
        .collect(Collectors.toSet());
```

---

## 32.9 Department → Employee Names

```java
Map<String, List<String>> result = employees.stream()
        .collect(
                Collectors.groupingBy(
                        Employee::department,
                        Collectors.mapping(
                                Employee::name,
                                Collectors.toList()
                        )
                )
        );
```

---

## 32.10 Employee Names as String

```java
String result = employees.stream()
        .map(Employee::name)
        .collect(
                Collectors.joining(", ")
        );
```

Output:

```text
Aman, Rahul, Alex
```

---

# 33. DSA Connection

Collectors are extremely useful for DSA-style problems involving:

```text
frequency counting
grouping
duplicate detection
aggregation
partitioning
mapping
```

---

## 33.1 Frequency Map

Given:

```java
List<String> words = List.of(
        "java",
        "sql",
        "java",
        "spring",
        "sql",
        "java"
);
```

Count frequency:

```java
Map<String, Long> frequency = words.stream()
        .collect(
                Collectors.groupingBy(
                        word -> word,
                        Collectors.counting()
                )
        );
```

Result:

```text
{
    java=3,
    sql=2,
    spring=1
}
```

---

## 33.2 Frequency of Integers

```java
Map<Integer, Long> frequency = numbers.stream()
        .collect(
                Collectors.groupingBy(
                        number -> number,
                        Collectors.counting()
                )
        );
```

---

## 33.3 Partition Even and Odd

```java
Map<Boolean, List<Integer>> result = numbers.stream()
        .collect(
                Collectors.partitioningBy(
                        n -> n % 2 == 0
                )
        );
```

---

## 33.4 Find Duplicate Values

One approach:

```java
Map<Integer, Long> frequency = numbers.stream()
        .collect(
                Collectors.groupingBy(
                        n -> n,
                        Collectors.counting()
                )
        );

List<Integer> duplicates = frequency.entrySet()
        .stream()
        .filter(entry -> entry.getValue() > 1)
        .map(Map.Entry::getKey)
        .toList();
```

---

## 33.5 Character Frequency

```java
String text = "banana";

Map<Character, Long> frequency = text.chars()
        .mapToObj(c -> (char) c)
        .collect(
                Collectors.groupingBy(
                        character -> character,
                        Collectors.counting()
                )
        );
```

Result:

```text
{
    b=1,
    a=3,
    n=2
}
```

---

## 33.6 Group Anagrams

A common DSA idea is to create a normalized key for each word.

Example concept:

```text
eat → aet
tea → aet
ate → aet
```

Then group by the normalized key.

```java
List<String> words = List.of(
        "eat",
        "tea",
        "ate",
        "bat"
);

Map<String, List<String>> groups = words.stream()
        .collect(
                Collectors.groupingBy(
                        word -> word.chars()
                                .sorted()
                                .collect(
                                        StringBuilder::new,
                                        StringBuilder::appendCodePoint,
                                        StringBuilder::append
                                )
                                .toString()
                )
        );
```

Conceptually:

```text
aet → [eat, tea, ate]
abt → [bat]
```

---

# 34. Common Mistakes

## Mistake 1: Confusing collect() and Collector

Wrong:

```text
collect() is a Collector
```

Correct:

```text
collect() → terminal Stream operation

Collector → strategy describing accumulation
```

---

## Mistake 2: Duplicate Keys in toMap()

This can fail:

```java
Collectors.toMap(
        Employee::id,
        Employee::name
)
```

if duplicate IDs exist.

Use a merge function when duplicates are possible:

```java
Collectors.toMap(
        Employee::id,
        Employee::name,
        (oldValue, newValue) -> oldValue
)
```

---

## Mistake 3: Assuming toSet() Preserves Order

Do not rely on:

```java
Collectors.toSet()
```

for encounter order.

Use:

```java
Collectors.toCollection(LinkedHashSet::new)
```

when that specific behavior is required.

---

## Mistake 4: Using groupingBy() When partitioningBy() Fits Better

For a simple:

```text
true / false
```

classification:

```java
Collectors.partitioningBy(...)
```

is often the clearer choice.

---

## Mistake 5: Confusing groupingBy() with toMap()

`groupingBy()` naturally supports multiple elements per key:

```text
Map<K, List<T>>
```

`toMap()` is commonly used when you want one mapped value per key, and duplicate keys need explicit handling.

---

## Mistake 6: Ignoring Downstream Collectors

This:

```java
groupingBy(Employee::department)
```

is only the beginning.

You can combine it with:

```text
counting()
mapping()
summingInt()
averagingInt()
maxBy()
minBy()
filtering()
flatMapping()
```

---

## Mistake 7: Assuming collect() Always Returns a Mutable Collection

The mutability depends on the collector being used.

For example:

```java
stream.toList()
```

returns an unmodifiable List.

Do not assume every collection-producing Stream operation has the same mutability semantics.

---

# 35. Interview Traps

## Trap 1

**Is collect() an intermediate operation?**

No.

It is terminal.

---

## Trap 2

**What does Collectors.toList() return?**

A `Collector` that accumulates elements into a List.

---

## Trap 3

**What happens when toMap() receives duplicate keys without a merge function?**

It throws an `IllegalStateException`.

---

## Trap 4

**What is groupingBy() used for?**

Grouping elements according to a classification function.

---

## Trap 5

**What does partitioningBy() return?**

Typically:

```text
Map<Boolean, List<T>>
```

or a Map with another downstream result type when a downstream Collector is supplied.

---

## Trap 6

**What is a downstream collector?**

A Collector applied to the values within each group or partition.

Example:

```java
Collectors.groupingBy(
        Employee::department,
        Collectors.counting()
)
```

Here:

```text
groupingBy → main collector
counting   → downstream collector
```

---

## Trap 7

**What is collectingAndThen()?**

It applies a finishing function after another Collector finishes its accumulation.

---

## Trap 8

**What does joining() do?**

It concatenates CharSequence elements into one String.

---

## Trap 9

**What does summarizingInt() provide?**

```text
count
sum
min
average
max
```

---

## Trap 10

**What is the difference between groupingBy() and partitioningBy()?**

```text
groupingBy     → multiple arbitrary groups

partitioningBy → exactly two boolean groups
```

---

# 36. Top 20 Interview Questions

## Q1. What is a Collector?

A Collector is an object describing how Stream elements should be accumulated into a final result.

---

## Q2. What is collect()?

`collect()` is a terminal Stream operation that performs a mutable reduction using a Collector or, in another overload, a supplier, accumulator, and combiner.

---

## Q3. What is the difference between collect() and Collector?

```text
collect()
    → Stream terminal operation

Collector
    → accumulation strategy
```

---

## Q4. What is Collectors.toList()?

It returns a Collector that accumulates Stream elements into a List.

---

## Q5. What is Collectors.toSet()?

It returns a Collector that accumulates elements into a Set, removing duplicates according to Set semantics.

---

## Q6. What is toMap() used for?

It converts Stream elements into key-value mappings.

Example:

```java
Collectors.toMap(
        Student::id,
        Student::name
)
```

---

## Q7. What happens with duplicate keys in toMap()?

Without a merge function, a duplicate key causes:

```text
IllegalStateException
```

---

## Q8. How do you handle duplicate keys in toMap()?

Provide a merge function:

```java
Collectors.toMap(
        Student::id,
        Student::name,
        (oldValue, newValue) -> oldValue
)
```

---

## Q9. What is groupingBy()?

It groups Stream elements according to a classification function.

Example:

```java
Collectors.groupingBy(Employee::department)
```

---

## Q10. What is a downstream Collector?

It is a Collector applied to the elements associated with each group.

Example:

```java
Collectors.groupingBy(
        Employee::department,
        Collectors.counting()
)
```

---

## Q11. What is partitioningBy()?

It partitions elements into two groups according to a boolean predicate.

---

## Q12. What is the difference between groupingBy() and partitioningBy()?

```text
groupingBy()
    → arbitrary classification keys

partitioningBy()
    → boolean classification
```

---

## Q13. What does joining() do?

It combines String or CharSequence elements into a single String.

---

## Q14. What does summarizingInt() do?

It calculates multiple integer statistics:

```text
count
sum
min
average
max
```

---

## Q15. What does collectingAndThen() do?

It performs collection and then applies a finishing transformation.

---

## Q16. What is the difference between reduce() and collect()?

```text
reduce()
    → combine values into one result

collect()
    → accumulate into collections or structured results
```

---

## Q17. Why is groupingBy() so useful in DSA?

It can implement frequency counting and classification patterns.

Example:

```java
Collectors.groupingBy(
        value -> value,
        Collectors.counting()
)
```

---

## Q18. Can Collectors work with parallel Streams?

Yes.

Collectors participate in Stream reduction and can support parallel collection depending on their characteristics and implementation.

---

## Q19. What is teeing()?

`teeing()` combines two downstream collectors and merges their two results into one final result.

---

## Q20. What are common Collector characteristics?

Important characteristics include:

```text
CONCURRENT
UNORDERED
IDENTITY_FINISH
```

---

# 37. 30-Second Interview Answer

> **Collectors are utility implementations used with the Stream API's collect() terminal operation to accumulate Stream elements into useful results. Common collectors include toList(), toSet(), toMap(), joining(), groupingBy(), partitioningBy(), counting(), summingInt(), averagingInt(), and summarizingInt(). A particularly important concept is downstream collectors, where operations such as counting() or mapping() are applied inside groupingBy(). Collectors are especially useful for converting Stream data into collections, maps, grouped data, and aggregate results.**

---

# 38. Cheat Sheet

| Collector | Purpose | Typical Result |
|---|---|---|
| `toList()` | Collect into List | `List<T>` |
| `toSet()` | Collect unique elements | `Set<T>` |
| `toCollection()` | Choose collection type | Collection |
| `toMap()` | Create Map | `Map<K,V>` |
| `joining()` | Concatenate Strings | `String` |
| `counting()` | Count | `Long` |
| `summingInt()` | Sum integers | `Integer` |
| `averagingInt()` | Average integers | `Double` |
| `summarizingInt()` | Statistics | `IntSummaryStatistics` |
| `maxBy()` | Maximum | `Optional<T>` |
| `minBy()` | Minimum | `Optional<T>` |
| `groupingBy()` | Group elements | `Map<K, ...>` |
| `partitioningBy()` | Split true/false | `Map<Boolean, ...>` |
| `mapping()` | Transform downstream | Depends |
| `filtering()` | Filter downstream | Depends |
| `flatMapping()` | Flatten downstream | Depends |
| `collectingAndThen()` | Finish after collection | Depends |
| `teeing()` | Combine two collectors | Depends |

---

# 39. Final Takeaway

The most important Collector patterns are:

```text
COLLECT INTO LIST
        ↓
Collectors.toList()
```

```text
COLLECT INTO SET
        ↓
Collectors.toSet()
```

```text
CREATE MAP
        ↓
Collectors.toMap()
```

```text
GROUP DATA
        ↓
Collectors.groupingBy()
```

```text
SPLIT INTO TRUE/FALSE
        ↓
Collectors.partitioningBy()
```

```text
JOIN STRINGS
        ↓
Collectors.joining()
```

```text
COUNT
        ↓
Collectors.counting()
```

```text
SUM
        ↓
Collectors.summingInt()
```

```text
AVERAGE
        ↓
Collectors.averagingInt()
```

```text
STATISTICS
        ↓
Collectors.summarizingInt()
```

---

# 🧠 Ultimate Memory Trick

Think of `Collectors` as:

```text
"What final shape do I want?"
```

If you want:

```text
List       → toList()
Set        → toSet()
Map        → toMap()
Groups     → groupingBy()
True/False → partitioningBy()
String     → joining()
Count      → counting()
Sum        → summingInt()
Average    → averagingInt()
Statistics → summarizingInt()
```

---

# 🔥 Most Important Interview Pattern

This pattern appears constantly in Java backend code and interviews:

```java
Map<String, Long> result = employees.stream()
        .collect(
                Collectors.groupingBy(
                        Employee::department,
                        Collectors.counting()
                )
        );
```

Read it as:

```text
employees
    ↓
group by department
    ↓
count employees in each group
    ↓
Map<Department, Count>
```

And the next level:

```java
Map<String, Integer> result = employees.stream()
        .collect(
                Collectors.groupingBy(
                        Employee::department,
                        Collectors.summingInt(
                                Employee::salary
                        )
                )
        );
```

Read it as:

```text
employees
    ↓
group by department
    ↓
sum salaries inside each group
    ↓
Map<Department, Total Salary>
```

---

# 🎯 Final Mental Model

```text
Stream API
    │
    ├── Intermediate Operations
    │      │
    │      ├── filter()
    │      ├── map()
    │      ├── flatMap()
    │      ├── distinct()
    │      └── sorted()
    │
    └── Terminal Operations
           │
           ├── collect()
           │      │
           │      └── Collectors
           │             ├── toList()
           │             ├── toSet()
           │             ├── toMap()
           │             ├── groupingBy()
           │             ├── partitioningBy()
           │             ├── joining()
           │             ├── counting()
           │             ├── summingInt()
           │             ├── averagingInt()
           │             └── summarizingInt()
           │
           ├── reduce()
           ├── forEach()
           ├── count()
           ├── min()
           └── max()
```

> 🚀 **Interview memory line:** `collect()` is the terminal operation; `Collectors` provide reusable strategies for turning a Stream into a List, Set, Map, grouped structure, String, or aggregate result.