# 🎯 10 — Stream API Interview Questions

> **Purpose:** A focused interview-preparation file covering the most important Java Stream API questions from beginner to advanced level, including concepts, internal working, traps, practical examples, and DSA-oriented thinking.

---

# 📌 Table of Contents

1. [Introduction](#1-introduction)
2. [Stream API Quick Revision](#2-stream-api-quick-revision)
3. [Q1. What is a Stream in Java?](#3-q1-what-is-a-stream-in-java)
4. [Q2. Stream vs Collection](#4-q2-stream-vs-collection)
5. [Q3. How do you create a Stream?](#5-q3-how-do-you-create-a-stream)
6. [Q4. What is the difference between intermediate and terminal operations?](#6-q4-what-is-the-difference-between-intermediate-and-terminal-operations)
7. [Q5. Why are intermediate operations lazy?](#7-q5-why-are-intermediate-operations-lazy)
8. [Q6. Can a Stream be reused?](#8-q6-can-a-stream-be-reused)
9. [Q7. Difference between map() and filter()](#9-q7-difference-between-map-and-filter)
10. [Q8. What is reduce()?](#10-q8-what-is-reduce)
11. [Q9. map() vs flatMap()](#11-q9-map-vs-flatmap)
12. [Q10. What is the difference between map() and mapToInt()?](#12-q10-what-is-the-difference-between-map-and-maptoint)
13. [Q11. What is filter()?](#13-q11-what-is-filter)
14. [Q12. What is distinct()?](#14-q12-what-is-distinct)
15. [Q13. What is sorted()?](#15-q13-what-is-sorted)
16. [Q14. sorted() vs Comparator](#16-q14-sorted-vs-comparator)
17. [Q15. What is limit()?](#17-q15-what-is-limit)
18. [Q16. What is skip()?](#18-q16-what-is-skip)
19. [Q17. What are short-circuiting operations?](#19-q17-what-are-short-circuiting-operations)
20. [Q18. findFirst() vs findAny()](#20-q18-findfirst-vs-findany)
21. [Q19. anyMatch(), allMatch(), noneMatch()](#21-q19-anymatch-allmatch-nonematch)
22. [Q20. forEach() vs forEachOrdered()](#22-q20-foreach-vs-foreachordered)
23. [Q21. What is collect()?](#23-q21-what-is-collect)
24. [Q22. collect() vs reduce()](#24-q22-collect-vs-reduce)
25. [Q23. What is Collectors.groupingBy()?](#25-q23-what-is-collectorsgroupingby)
26. [Q24. What is partitioningBy()?](#26-q24-what-is-partitioningby)
27. [Q25. groupingBy() vs partitioningBy()](#27-q25-groupingby-vs-partitioningby)
28. [Q26. How do you convert a Stream into a List?](#28-q26-how-do-you-convert-a-stream-into-a-list)
29. [Q27. How do you remove duplicates using Streams?](#29-q27-how-do-you-remove-duplicates-using-streams)
30. [Q28. How do you find the maximum and minimum?](#30-q28-how-do-you-find-the-maximum-and-minimum)
31. [Q29. How do you find the second-highest number?](#31-q29-how-do-you-find-the-second-highest-number)
32. [Q30. How do you count frequency of elements?](#32-q30-how-do-you-count-frequency-of-elements)
33. [Q31. How do you find duplicate elements?](#33-q31-how-do-you-find-duplicate-elements)
34. [Q32. How do you find the first non-repeated character?](#34-q32-how-do-you-find-the-first-non-repeated-character)
35. [Q33. How do you sort a Map using Streams?](#35-q33-how-do-you-sort-a-map-using-streams)
36. [Q34. How do you sort objects using Streams?](#36-q34-how-do-you-sort-objects-using-streams)
37. [Q35. What is Optional in Stream operations?](#37-q35-what-is-optional-in-stream-operations)
38. [Q36. What are primitive Streams?](#38-q36-what-are-primitive-streams)
39. [Q37. What is a Parallel Stream?](#39-q37-what-is-a-parallel-stream)
40. [Q38. How do Parallel Streams work internally?](#40-q38-how-do-parallel-streams-work-internally)
41. [Q39. When should you avoid Parallel Streams?](#41-q39-when-should-you-avoid-parallel-streams)
42. [Q40. What are stateful and stateless operations?](#42-q40-what-are-stateful-and-stateless-operations)
43. [Q41. What is lazy evaluation?](#43-q41-what-is-lazy-evaluation)
44. [Q42. What is pipeline fusion?](#44-q42-what-is-pipeline-fusion)
45. [Q43. Does Stream create a new collection?](#45-q43-does-stream-create-a-new-collection)
46. [Q44. Can Streams modify the source collection?](#46-q44-can-streams-modify-the-source-collection)
47. [Q45. What is the difference between Stream and Iterator?](#47-q45-what-is-the-difference-between-stream-and-iterator)
48. [Q46. What is a Spliterator?](#48-q46-what-is-a-spliterator)
49. [Q47. What is a Stream pipeline?](#49-q47-what-is-a-stream-pipeline)
50. [Q48. What happens when a terminal operation is called?](#50-q48-what-happens-when-a-terminal-operation-is-called)
51. [Q49. What is the difference between sequential and parallel Streams?](#51-q49-what-is-the-difference-between-sequential-and-parallel-streams)
52. [Q50. What are common Stream API mistakes?](#52-q50-what-are-common-stream-api-mistakes)
53. [Coding Interview Questions](#53-coding-interview-questions)
54. [DSA Connection](#54-dsa-connection)
55. [Interview Traps](#55-interview-traps)
56. [Top 10 Must-Know Questions](#56-top-10-must-know-questions)
57. [30-Second Interview Answer](#57-30-second-interview-answer)
58. [Final Cheat Sheet](#58-final-cheat-sheet)
59. [Final Takeaway](#59-final-takeaway)

---

# 1. Introduction

The Java Stream API was introduced in **Java 8** to provide a declarative way to process data.

Instead of manually writing loops:

```java
List<Integer> evenNumbers = new ArrayList<>();

for (Integer number : numbers) {
    if (number % 2 == 0) {
        evenNumbers.add(number);
    }
}
```

we can write:

```java
List<Integer> evenNumbers = numbers.stream()
        .filter(number -> number % 2 == 0)
        .toList();
```

The Stream API is heavily used in:

- Java backend development
- Spring Boot applications
- Collection processing
- Data transformation
- Filtering
- Sorting
- Grouping
- Aggregation
- Interview coding questions

---

# 2. Stream API Quick Revision

The basic Stream pipeline is:

```text
SOURCE
  ↓
INTERMEDIATE OPERATIONS
  ↓
TERMINAL OPERATION
```

Example:

```java
numbers.stream()
        .filter(n -> n % 2 == 0)
        .map(n -> n * 2)
        .toList();
```

Here:

```text
numbers
   ↓
stream()
   ↓
filter()
   ↓
map()
   ↓
toList()
```

---

## Stream Categories

### Intermediate Operations

Return another Stream:

```text
filter()
map()
flatMap()
sorted()
distinct()
limit()
skip()
peek()
```

### Terminal Operations

End the Stream:

```text
forEach()
collect()
toList()
reduce()
count()
min()
max()
findFirst()
findAny()
anyMatch()
allMatch()
noneMatch()
```

---

# 3. Q1. What is a Stream in Java?

### Answer

A Stream is a sequence of elements supporting functional-style operations for processing data.

Example:

```java
List<String> names = List.of(
        "Amit",
        "Rahul",
        "Yashu"
);

names.stream()
        .filter(name -> name.length() > 4)
        .forEach(System.out::println);
```

Important:

> A Stream is not a data structure. It does not store elements itself.

---

# 4. Q2. Stream vs Collection

| Collection | Stream |
|---|---|
| Stores data | Processes data |
| Can be reused | Normally cannot be reused |
| External iteration is common | Internal iteration |
| Represents data | Represents computation |
| Can add/remove elements | Does not directly modify source |
| Eager data structure | Supports lazy processing |

Example:

```java
List<Integer> numbers = List.of(1, 2, 3, 4);
```

The List stores:

```text
1 2 3 4
```

The Stream processes them:

```java
numbers.stream()
        .filter(n -> n % 2 == 0)
        .forEach(System.out::println);
```

---

# 5. Q3. How do you create a Stream?

## From Collection

```java
List<Integer> numbers = List.of(1, 2, 3);

Stream<Integer> stream = numbers.stream();
```

## From Collection in Parallel

```java
Stream<Integer> stream = numbers.parallelStream();
```

## From Array

```java
int[] numbers = {1, 2, 3, 4};

IntStream stream = Arrays.stream(numbers);
```

## Using Stream.of()

```java
Stream<String> stream = Stream.of(
        "Java",
        "Spring",
        "SQL"
);
```

## Using Stream.iterate()

```java
Stream<Integer> stream = Stream.iterate(
        1,
        n -> n + 1
);
```

---

# 6. Q4. What is the difference between intermediate and terminal operations?

## Intermediate

Intermediate operations return another Stream.

Example:

```java
Stream<Integer> result = numbers.stream()
        .filter(n -> n > 2)
        .map(n -> n * 2);
```

Examples:

```text
filter()
map()
flatMap()
sorted()
distinct()
limit()
skip()
```

---

## Terminal

Terminal operations produce a result or side effect and finish the Stream pipeline.

Example:

```java
long count = numbers.stream()
        .filter(n -> n > 2)
        .count();
```

Examples:

```text
count()
collect()
toList()
reduce()
forEach()
findFirst()
```

---

# 7. Q5. Why are intermediate operations lazy?

Intermediate operations do not execute immediately.

Example:

```java
numbers.stream()
        .filter(n -> {
            System.out.println("Filtering " + n);
            return n > 2;
        });
```

Nothing is printed because there is no terminal operation.

Now:

```java
numbers.stream()
        .filter(n -> {
            System.out.println("Filtering " + n);
            return n > 2;
        })
        .count();
```

The pipeline executes.

### Interview Answer

Intermediate operations are lazy because Java can optimize the pipeline and process elements only when a terminal result is required.

---

# 8. Q6. Can a Stream be reused?

No.

Once a terminal operation consumes the Stream, it cannot normally be reused.

Example:

```java
Stream<Integer> stream = numbers.stream();

stream.count();

stream.forEach(System.out::println);
```

This causes:

```text
IllegalStateException
```

### Correct approach

Create a new Stream:

```java
numbers.stream().count();

numbers.stream().forEach(System.out::println);
```

---

# 9. Q7. Difference between map() and filter()

## filter()

Used to select elements.

```java
List<Integer> result = numbers.stream()
        .filter(n -> n % 2 == 0)
        .toList();
```

Input:

```text
1 2 3 4
```

Output:

```text
2 4
```

---

## map()

Used to transform elements.

```java
List<Integer> result = numbers.stream()
        .map(n -> n * 2)
        .toList();
```

Input:

```text
1 2 3 4
```

Output:

```text
2 4 6 8
```

### Memory Trick

```text
filter → select
map    → transform
```

---

# 10. Q8. What is reduce()?

`reduce()` combines Stream elements into a single result.

Example:

```java
int sum = numbers.stream()
        .reduce(0, Integer::sum);
```

Conceptually:

```text
1 + 2 + 3 + 4
        ↓
       10
```

Another example:

```java
int product = numbers.stream()
        .reduce(1, (a, b) -> a * b);
```

---

# 11. Q9. map() vs flatMap()

## map()

Transforms each element into another element.

```java
List<String> names = List.of(
        "Java",
        "Spring"
);

List<Integer> lengths = names.stream()
        .map(String::length)
        .toList();
```

Result:

```text
4 6
```

---

## flatMap()

Used when each element produces multiple elements and you want one flattened Stream.

```java
List<List<Integer>> numbers = List.of(
        List.of(1, 2),
        List.of(3, 4),
        List.of(5, 6)
);

List<Integer> result = numbers.stream()
        .flatMap(List::stream)
        .toList();
```

Result:

```text
1 2 3 4 5 6
```

### Memory Trick

```text
map     → one-to-one transformation
flatMap → flatten nested structure
```

---

# 12. Q10. What is the difference between map() and mapToInt()?

`map()` produces an object Stream.

```java
Stream<Integer> stream = numbers.stream()
        .map(n -> n * 2);
```

`mapToInt()` produces an `IntStream`.

```java
IntStream stream = numbers.stream()
        .mapToInt(Integer::intValue);
```

Primitive Streams can provide primitive-specific operations such as:

```java
int sum = numbers.stream()
        .mapToInt(Integer::intValue)
        .sum();
```

---

# 13. Q11. What is filter()?

`filter()` selects elements that satisfy a condition.

```java
List<Integer> result = numbers.stream()
        .filter(n -> n > 5)
        .toList();
```

Only elements for which the predicate returns `true` remain.

Conceptually:

```text
Input
 ↓
Predicate
 ↓
true  → keep
false → discard
```

---

# 14. Q12. What is distinct()?

`distinct()` removes duplicate elements according to the Stream's equality semantics.

Example:

```java
List<Integer> numbers = List.of(
        1, 2, 2, 3, 3, 4
);

List<Integer> result = numbers.stream()
        .distinct()
        .toList();
```

Result:

```text
1 2 3 4
```

For objects, correct `equals()` and `hashCode()` implementations are important.

---

# 15. Q13. What is sorted()?

`sorted()` sorts Stream elements.

Natural ordering:

```java
List<Integer> result = numbers.stream()
        .sorted()
        .toList();
```

Descending:

```java
List<Integer> result = numbers.stream()
        .sorted(Comparator.reverseOrder())
        .toList();
```

---

# 16. Q14. sorted() vs Comparator

`sorted()` defines the Stream operation.

`Comparator` defines how objects should be compared.

Example:

```java
List<String> names = List.of(
        "Rahul",
        "Amit",
        "Yashu"
);

List<String> result = names.stream()
        .sorted(Comparator.comparing(String::length))
        .toList();
```

Here:

```text
sorted()
    → performs sorting

Comparator
    → defines ordering
```

---

# 17. Q15. What is limit()?

`limit(n)` restricts the Stream to at most `n` elements.

```java
List<Integer> result = numbers.stream()
        .limit(3)
        .toList();
```

Input:

```text
1 2 3 4 5
```

Output:

```text
1 2 3
```

It is also a short-circuiting operation.

---

# 18. Q16. What is skip()?

`skip(n)` ignores the first `n` elements.

```java
List<Integer> result = numbers.stream()
        .skip(2)
        .toList();
```

Input:

```text
1 2 3 4 5
```

Output:

```text
3 4 5
```

---

# 19. Q17. What are short-circuiting operations?

A short-circuiting operation can stop processing once the required result is known.

Examples:

```text
findFirst()
findAny()
anyMatch()
allMatch()
noneMatch()
limit()
```

Example:

```java
boolean result = numbers.stream()
        .anyMatch(n -> n > 100);
```

Once a matching element is found, the Stream may stop processing further elements.

---

# 20. Q18. findFirst() vs findAny()

## findFirst()

Returns the first element according to encounter order for an ordered Stream.

```java
Optional<Integer> result = numbers.stream()
        .findFirst();
```

## findAny()

Returns some element.

```java
Optional<Integer> result = numbers.stream()
        .findAny();
```

With parallel Streams, `findAny()` gives the implementation more freedom.

### Memory Trick

```text
findFirst → first
findAny   → any
```

---

# 21. Q19. anyMatch(), allMatch(), noneMatch()

## anyMatch()

At least one element must match.

```java
boolean result = numbers.stream()
        .anyMatch(n -> n > 10);
```

---

## allMatch()

Every element must match.

```java
boolean result = numbers.stream()
        .allMatch(n -> n > 0);
```

---

## noneMatch()

No element should match.

```java
boolean result = numbers.stream()
        .noneMatch(n -> n < 0);
```

All three are short-circuiting terminal operations.

---

# 22. Q20. forEach() vs forEachOrdered()

```java
numbers.parallelStream()
        .forEach(System.out::println);
```

does not guarantee encounter order.

Whereas:

```java
numbers.parallelStream()
        .forEachOrdered(System.out::println);
```

respects encounter order for an ordered Stream.

---

# 23. Q21. What is collect()?

`collect()` is a terminal operation used to accumulate Stream elements into a result.

Example:

```java
List<Integer> result = numbers.stream()
        .filter(n -> n % 2 == 0)
        .collect(Collectors.toList());
```

Modern Java can also use:

```java
List<Integer> result = numbers.stream()
        .filter(n -> n % 2 == 0)
        .toList();
```

`collect()` is especially powerful with `Collectors`.

---

# 24. Q22. collect() vs reduce()

## reduce()

Generally used to combine elements into a single value.

```java
int sum = numbers.stream()
        .reduce(0, Integer::sum);
```

## collect()

Generally used to accumulate elements into mutable result containers or more structured results.

```java
List<Integer> result = numbers.stream()
        .collect(Collectors.toList());
```

### Memory Trick

```text
reduce  → combine into a value
collect → accumulate into a result structure
```

---

# 25. Q23. What is Collectors.groupingBy()?

`groupingBy()` groups elements according to a classification function.

Example:

```java
List<String> names = List.of(
        "Amit",
        "Ankit",
        "Rahul",
        "Ravi"
);

Map<Integer, List<String>> result = names.stream()
        .collect(Collectors.groupingBy(String::length));
```

Conceptually:

```text
Length 4 → [Amit, Ravi]
Length 5 → [Ankit, Rahul]
```

The exact grouping depends on the input.

---

# 26. Q24. What is partitioningBy()?

`partitioningBy()` divides elements into two groups based on a predicate:

```text
true
false
```

Example:

```java
Map<Boolean, List<Integer>> result = numbers.stream()
        .collect(Collectors.partitioningBy(
                n -> n % 2 == 0
        ));
```

Conceptually:

```text
true  → even numbers
false → odd numbers
```

---

# 27. Q25. groupingBy() vs partitioningBy()

| `groupingBy()` | `partitioningBy()` |
|---|---|
| Groups by classifier | Splits by predicate |
| Can produce many groups | Produces true/false groups |
| Returns Map<K, ...> | Returns Map<Boolean, ...> |
| Useful for categories | Useful for two-way classification |

---

# 28. Q26. How do you convert a Stream into a List?

Modern Java:

```java
List<Integer> result = numbers.stream()
        .filter(n -> n > 5)
        .toList();
```

Using Collector:

```java
List<Integer> result = numbers.stream()
        .filter(n -> n > 5)
        .collect(Collectors.toList());
```

Important distinction:

`Stream.toList()` and `Collectors.toList()` do not have identical mutability guarantees.

Do not assume a list returned by `toList()` is modifiable.

---

# 29. Q27. How do you remove duplicates using Streams?

Use:

```java
List<Integer> result = numbers.stream()
        .distinct()
        .toList();
```

Example:

```text
Input:
1 2 2 3 3 3 4

Output:
1 2 3 4
```

---

# 30. Q28. How do you find the maximum and minimum?

## Maximum

```java
Optional<Integer> max = numbers.stream()
        .max(Integer::compareTo);
```

## Minimum

```java
Optional<Integer> min = numbers.stream()
        .min(Integer::compareTo);
```

For primitive values:

```java
OptionalInt max = numbers.stream()
        .mapToInt(Integer::intValue)
        .max();
```

---

# 31. Q29. How do you find the second-highest number?

One approach:

```java
Optional<Integer> secondHighest = numbers.stream()
        .distinct()
        .sorted(Comparator.reverseOrder())
        .skip(1)
        .findFirst();
```

Pipeline:

```text
distinct
   ↓
sorted descending
   ↓
skip highest
   ↓
find first
```

Example:

```text
Input:
10 20 30 30 40

Distinct:
10 20 30 40

Descending:
40 30 20 10

Skip:
30 20 10

Result:
30
```

---

# 32. Q30. How do you count frequency of elements?

Use `groupingBy()` with `counting()`.

```java
Map<Integer, Long> frequency = numbers.stream()
        .collect(Collectors.groupingBy(
                n -> n,
                Collectors.counting()
        ));
```

For:

```text
1 2 2 3 3 3
```

Conceptually:

```text
1 → 1
2 → 2
3 → 3
```

---

# 33. Q31. How do you find duplicate elements?

Example:

```java
Set<Integer> duplicates = numbers.stream()
        .collect(Collectors.groupingBy(
                n -> n,
                Collectors.counting()
        ))
        .entrySet()
        .stream()
        .filter(entry -> entry.getValue() > 1)
        .map(Map.Entry::getKey)
        .collect(Collectors.toSet());
```

Logic:

```text
Group
 ↓
Count
 ↓
count > 1
 ↓
Duplicate
```

---

# 34. Q32. How do you find the first non-repeated character?

Example:

```java
String text = "swiss";

Character result = text.chars()
        .mapToObj(c -> (char) c)
        .collect(Collectors.groupingBy(
                c -> c,
                LinkedHashMap::new,
                Collectors.counting()
        ))
        .entrySet()
        .stream()
        .filter(entry -> entry.getValue() == 1)
        .map(Map.Entry::getKey)
        .findFirst()
        .orElse(null);
```

Why `LinkedHashMap`?

Because we need to preserve insertion order.

Pipeline:

```text
Characters
    ↓
Frequency
    ↓
Preserve order
    ↓
Find frequency == 1
    ↓
First result
```

---

# 35. Q33. How do you sort a Map using Streams?

Example:

```java
Map<String, Integer> map = Map.of(
        "A", 30,
        "B", 10,
        "C", 20
);

Map<String, Integer> sorted = map.entrySet()
        .stream()
        .sorted(Map.Entry.comparingByValue())
        .collect(Collectors.toMap(
                Map.Entry::getKey,
                Map.Entry::getValue,
                (a, b) -> a,
                LinkedHashMap::new
        ));
```

`LinkedHashMap` is used to preserve the resulting iteration order.

---

# 36. Q34. How do you sort objects using Streams?

Suppose:

```java
class Employee {

    private String name;
    private double salary;

    public String getName() {
        return name;
    }

    public double getSalary() {
        return salary;
    }
}
```

Sort by salary:

```java
List<Employee> result = employees.stream()
        .sorted(Comparator.comparingDouble(Employee::getSalary))
        .toList();
```

Descending:

```java
List<Employee> result = employees.stream()
        .sorted(
                Comparator.comparingDouble(Employee::getSalary)
                        .reversed()
        )
        .toList();
```

---

# 37. Q35. What is Optional in Stream operations?

Some Stream operations may produce no result.

For example:

```java
Optional<Integer> result = numbers.stream()
        .filter(n -> n > 1000)
        .findFirst();
```

Instead of returning `null`, the API uses `Optional`.

You can check:

```java
if (result.isPresent()) {
    System.out.println(result.get());
}
```

Or:

```java
result.ifPresent(System.out::println);
```

---

# 38. Q36. What are primitive Streams?

Java provides specialized Streams:

```text
IntStream
LongStream
DoubleStream
```

Example:

```java
int sum = numbers.stream()
        .mapToInt(Integer::intValue)
        .sum();
```

Advantages include primitive-oriented operations and avoiding some boxing/unboxing overhead.

---

# 39. Q37. What is a Parallel Stream?

A Parallel Stream allows Stream processing to be divided into tasks that may execute concurrently.

Example:

```java
numbers.parallelStream()
        .map(n -> n * 2)
        .forEach(System.out::println);
```

Parallel Streams typically use the common ForkJoinPool.

---

# 40. Q38. How do Parallel Streams work internally?

Simplified flow:

```text
Source
  ↓
Spliterator
  ↓
Split workload
  ↓
ForkJoinPool
  ↓
Worker tasks
  ↓
Process partitions
  ↓
Combine results
```

Important components:

```text
Spliterator
ForkJoinPool
Stream pipeline
Reduction / Collector
```

---

# 41. Q39. When should you avoid Parallel Streams?

Avoid blindly using them when:

- Dataset is small
- Operations are cheap
- Ordering is critical
- Heavy synchronization is required
- Operations are blocking I/O
- Shared mutable state is involved
- Workload does not split efficiently

Most importantly:

```text
parallel ≠ automatically faster
```

---

# 42. Q40. What are stateful and stateless operations?

## Stateless

Each element can be processed independently.

Examples:

```text
map()
filter()
peek()
```

Conceptually:

```text
element → process → result
```

---

## Stateful

May need information about other elements.

Examples:

```text
sorted()
distinct()
```

Conceptually:

```text
multiple elements
       ↓
maintain state
       ↓
produce result
```

---

# 43. Q41. What is lazy evaluation?

Lazy evaluation means intermediate operations are not executed until a terminal operation is invoked.

Example:

```java
numbers.stream()
        .filter(n -> {
            System.out.println(n);
            return n > 2;
        });
```

Nothing executes.

Now:

```java
numbers.stream()
        .filter(n -> {
            System.out.println(n);
            return n > 2;
        })
        .count();
```

The pipeline executes.

---

# 44. Q42. What is pipeline fusion?

Stream operations can often be processed as one pipeline rather than creating separate intermediate collections for every operation.

Example:

```java
numbers.stream()
        .filter(n -> n > 5)
        .map(n -> n * 2)
        .filter(n -> n < 50)
        .toList();
```

Conceptually, Java can process an element through the pipeline:

```text
element
  ↓
filter
  ↓
map
  ↓
filter
  ↓
result
```

rather than necessarily creating:

```text
temporary collection
    ↓
temporary collection
    ↓
final collection
```

This is one reason Streams can be expressive and efficient.

---

# 45. Q43. Does Stream create a new collection?

No.

Creating a Stream does not automatically create a new collection.

```java
Stream<Integer> stream = numbers.stream();
```

The Stream represents a pipeline over the source.

A terminal operation such as:

```java
.toList()
```

can create a result collection.

---

# 46. Q44. Can Streams modify the source collection?

Stream operations generally should not modify the source while it is being traversed.

Bad pattern:

```java
numbers.stream()
        .forEach(n -> numbers.remove(n));
```

This can cause concurrent modification problems.

Prefer creating a new result:

```java
List<Integer> result = numbers.stream()
        .filter(n -> n % 2 == 0)
        .toList();
```

---

# 47. Q45. What is the difference between Stream and Iterator?

| Stream | Iterator |
|---|---|
| Functional-style processing | Explicit traversal |
| Supports pipelines | Manual iteration |
| map/filter/reduce | next/hasNext |
| Can be parallel | Usually sequential traversal |
| Declarative | More imperative |

Iterator:

```java
Iterator<Integer> iterator = numbers.iterator();

while (iterator.hasNext()) {
    System.out.println(iterator.next());
}
```

Stream:

```java
numbers.stream()
        .forEach(System.out::println);
```

---

# 48. Q46. What is a Spliterator?

`Spliterator` is an interface designed to traverse and partition elements.

Important methods:

```text
tryAdvance()
forEachRemaining()
trySplit()
estimateSize()
characteristics()
```

`trySplit()` is especially important for parallel processing.

---

# 49. Q47. What is a Stream pipeline?

A Stream pipeline consists of:

```text
Source
+
Zero or more intermediate operations
+
One terminal operation
```

Example:

```java
List<Integer> result = numbers.stream()
        .filter(n -> n > 5)
        .map(n -> n * 2)
        .sorted()
        .toList();
```

Pipeline:

```text
numbers
  ↓
stream()
  ↓
filter()
  ↓
map()
  ↓
sorted()
  ↓
toList()
```

---

# 50. Q48. What happens when a terminal operation is called?

When a terminal operation is invoked:

```java
numbers.stream()
        .filter(n -> n > 5)
        .map(n -> n * 2)
        .toList();
```

the Stream pipeline is evaluated.

Conceptually:

```text
Build pipeline
      ↓
Terminal operation
      ↓
Traversal begins
      ↓
Intermediate operations execute
      ↓
Result produced
```

After that, the Stream is considered consumed.

---

# 51. Q49. What is the difference between sequential and parallel Streams?

| Sequential | Parallel |
|---|---|
| Sequential execution | Can execute tasks concurrently |
| Usually simpler | More coordination |
| No parallel scheduling overhead | Has parallel overhead |
| Predictable processing model | More complex execution |
| Good default for many workloads | Useful for suitable workloads |

Sequential:

```java
numbers.stream()
        .map(n -> n * 2)
        .toList();
```

Parallel:

```java
numbers.parallelStream()
        .map(n -> n * 2)
        .toList();
```

---

# 52. Q50. What are common Stream API mistakes?

## Mistake 1 — Reusing a Stream

```java
Stream<Integer> stream = numbers.stream();

stream.count();
stream.count();
```

Wrong.

---

## Mistake 2 — Forgetting terminal operation

```java
numbers.stream()
        .filter(n -> n > 5);
```

No processing happens yet.

---

## Mistake 3 — Using side effects unnecessarily

Avoid:

```java
List<Integer> result = new ArrayList<>();

numbers.stream()
        .forEach(result::add);
```

Prefer:

```java
List<Integer> result = numbers.stream()
        .toList();
```

---

## Mistake 4 — Using parallelStream() everywhere

Parallelism has overhead.

---

## Mistake 5 — Confusing map() and flatMap()

```text
map    → transform
flatMap → flatten
```

---

## Mistake 6 — Ignoring encounter order

Especially with parallel Streams.

---

# 53. Coding Interview Questions

## 53.1 Find Even Numbers

```java
List<Integer> result = numbers.stream()
        .filter(n -> n % 2 == 0)
        .toList();
```

---

## 53.2 Find Odd Numbers

```java
List<Integer> result = numbers.stream()
        .filter(n -> n % 2 != 0)
        .toList();
```

---

## 53.3 Square Every Number

```java
List<Integer> result = numbers.stream()
        .map(n -> n * n)
        .toList();
```

---

## 53.4 Sum Numbers

```java
int sum = numbers.stream()
        .mapToInt(Integer::intValue)
        .sum();
```

---

## 53.5 Find Maximum

```java
Optional<Integer> max = numbers.stream()
        .max(Integer::compareTo);
```

---

## 53.6 Find Minimum

```java
Optional<Integer> min = numbers.stream()
        .min(Integer::compareTo);
```

---

## 53.7 Remove Duplicates

```java
List<Integer> result = numbers.stream()
        .distinct()
        .toList();
```

---

## 53.8 Sort Ascending

```java
List<Integer> result = numbers.stream()
        .sorted()
        .toList();
```

---

## 53.9 Sort Descending

```java
List<Integer> result = numbers.stream()
        .sorted(Comparator.reverseOrder())
        .toList();
```

---

## 53.10 Find Second Highest

```java
Optional<Integer> result = numbers.stream()
        .distinct()
        .sorted(Comparator.reverseOrder())
        .skip(1)
        .findFirst();
```

---

## 53.11 Count Frequencies

```java
Map<Integer, Long> frequency = numbers.stream()
        .collect(Collectors.groupingBy(
                n -> n,
                Collectors.counting()
        ));
```

---

## 53.12 Find Duplicate Elements

```java
Set<Integer> duplicates = numbers.stream()
        .collect(Collectors.groupingBy(
                n -> n,
                Collectors.counting()
        ))
        .entrySet()
        .stream()
        .filter(entry -> entry.getValue() > 1)
        .map(Map.Entry::getKey)
        .collect(Collectors.toSet());
```

---

## 53.13 Join Strings

```java
String result = names.stream()
        .collect(Collectors.joining(", "));
```

Example:

```text
Amit, Rahul, Yashu
```

---

## 53.14 Find Strings Starting With A

```java
List<String> result = names.stream()
        .filter(name -> name.startsWith("A"))
        .toList();
```

---

## 53.15 Find Longest String

```java
Optional<String> result = names.stream()
        .max(Comparator.comparingInt(String::length));
```

---

## 53.16 Find Shortest String

```java
Optional<String> result = names.stream()
        .min(Comparator.comparingInt(String::length));
```

---

## 53.17 Convert Strings to Uppercase

```java
List<String> result = names.stream()
        .map(String::toUpperCase)
        .toList();
```

---

## 53.18 Flatten Nested Lists

```java
List<Integer> result = nestedLists.stream()
        .flatMap(List::stream)
        .toList();
```

---

## 53.19 Partition Even and Odd Numbers

```java
Map<Boolean, List<Integer>> result = numbers.stream()
        .collect(Collectors.partitioningBy(
                n -> n % 2 == 0
        ));
```

---

## 53.20 Group Employees by Department

```java
Map<String, List<Employee>> result = employees.stream()
        .collect(Collectors.groupingBy(
                Employee::getDepartment
        ));
```

---

# 54. DSA Connection

The Stream API is useful for expressing common DSA patterns, but it should not replace understanding the underlying algorithm.

---

## Pattern 1 — Filtering

Equivalent DSA idea:

```text
Traverse array
    ↓
Check condition
    ↓
Store matching values
```

Stream:

```java
List<Integer> result = numbers.stream()
        .filter(n -> n > 10)
        .toList();
```

---

## Pattern 2 — Transformation

Equivalent:

```text
Traverse
 ↓
Transform each element
 ↓
Store
```

Stream:

```java
List<Integer> result = numbers.stream()
        .map(n -> n * 2)
        .toList();
```

---

## Pattern 3 — Frequency Map

Classic DSA pattern:

```text
value → frequency
```

Stream:

```java
Map<Integer, Long> frequency = numbers.stream()
        .collect(Collectors.groupingBy(
                n -> n,
                Collectors.counting()
        ));
```

---

## Pattern 4 — Duplicate Detection

Classic DSA pattern:

```text
HashMap / HashSet
```

Stream:

```java
Set<Integer> duplicates = numbers.stream()
        .collect(Collectors.groupingBy(
                n -> n,
                Collectors.counting()
        ))
        .entrySet()
        .stream()
        .filter(entry -> entry.getValue() > 1)
        .map(Map.Entry::getKey)
        .collect(Collectors.toSet());
```

---

## Pattern 5 — Sorting

Classic DSA:

```text
sort
```

Stream:

```java
List<Integer> result = numbers.stream()
        .sorted()
        .toList();
```

Remember:

```text
Stream syntax ≠ algorithmic understanding
```

You still need to know the underlying complexity.

---

# 55. Interview Traps

## Trap 1

### "Stream stores elements."

❌ Wrong.

Stream processes elements from a source.

---

## Trap 2

### "Streams modify the original collection."

❌ Not inherently.

Stream operations generally produce processed results without modifying the source.

---

## Trap 3

### "Intermediate operations execute immediately."

❌ Wrong.

They are generally lazy.

---

## Trap 4

### "A Stream can be reused."

❌ No.

After consumption, create another Stream.

---

## Trap 5

### "map() removes elements."

❌ No.

`map()` transforms.

`filter()` selects/removes from the pipeline.

---

## Trap 6

### "flatMap() is just map()."

❌ No.

`flatMap()` additionally flattens nested results.

---

## Trap 7

### "parallelStream() is always faster."

❌ Wrong.

Performance depends on workload and overhead.

---

## Trap 8

### "forEach() preserves order."

❌ Not necessarily, especially for parallel Streams.

---

## Trap 9

### "reduce() and collect() are identical."

❌ No.

They solve different aggregation problems.

---

## Trap 10

### "sorted() is stateless."

❌ No.

Sorting requires state and coordination.

---

# 56. Top 10 Must-Know Questions

For a Java Backend interview, make sure you can answer these without hesitation:

### 1. What is Stream API?

```text
A functional-style API for processing sequences of data.
```

### 2. Stream vs Collection?

```text
Collection stores data.
Stream processes data.
```

### 3. Intermediate vs terminal?

```text
Intermediate → returns Stream
Terminal     → produces final result / side effect
```

### 4. Why are Streams lazy?

```text
To defer execution and enable efficient pipeline processing.
```

### 5. map() vs filter()?

```text
map    → transform
filter → select
```

### 6. map() vs flatMap()?

```text
map    → transformation
flatMap → transformation + flattening
```

### 7. reduce() vs collect()?

```text
reduce  → combine values
collect → accumulate into result structure
```

### 8. findFirst() vs findAny()?

```text
findFirst → encounter-order first
findAny   → any element
```

### 9. Sequential vs Parallel Stream?

```text
Sequential → sequential processing
Parallel   → potentially concurrent processing
```

### 10. Can Stream be reused?

```text
No.
```

---

# 57. 30-Second Interview Answer

> **Java Stream API is a Java 8 feature that provides a declarative and functional-style way to process data from sources such as collections and arrays. A Stream pipeline consists of a source, intermediate operations such as filter, map, and sorted, and a terminal operation such as collect, reduce, or forEach. Intermediate operations are lazy, and a Stream normally cannot be reused after a terminal operation. Streams can also execute in parallel, but parallel processing should only be used when the workload benefits from it.**

---

# 58. Final Cheat Sheet

| Concept | Remember |
|---|---|
| Stream | Processes data |
| Collection | Stores data |
| `stream()` | Sequential Stream |
| `parallelStream()` | Parallel Stream |
| Intermediate | Returns Stream |
| Terminal | Ends pipeline |
| `filter()` | Select |
| `map()` | Transform |
| `flatMap()` | Flatten |
| `distinct()` | Remove duplicates |
| `sorted()` | Sort |
| `limit()` | Take first N |
| `skip()` | Skip first N |
| `peek()` | Observe/debug |
| `reduce()` | Combine |
| `collect()` | Accumulate |
| `count()` | Count |
| `findFirst()` | First |
| `findAny()` | Any |
| `anyMatch()` | At least one |
| `allMatch()` | Every |
| `noneMatch()` | None |
| `groupingBy()` | Group |
| `partitioningBy()` | True/false split |
| `toList()` | Create List result |
| `IntStream` | Primitive int Stream |
| `Optional` | Possible absence of result |
| `Spliterator` | Traverse + split |
| Parallel Stream | Concurrent-capable processing |

---

# 59. Final Takeaway

The entire Stream API can be remembered using this model:

```text
                  STREAM API
                      │
             ┌────────┴────────┐
             ↓                 ↓
          SOURCE            PIPELINE
                              │
                    ┌─────────┴─────────┐
                    ↓                   ↓
              INTERMEDIATE          TERMINAL
                    │                   │
             ┌──────┼──────┐      ┌────┼────┐
             ↓      ↓      ↓      ↓    ↓    ↓
           filter   map   flatMap  collect reduce forEach
             │      │      │
             └──────┴──────┘
                    ↓
                  RESULT
```

## 🧠 Ultimate Memory Trick

```text
FILTER  → SELECT
MAP     → TRANSFORM
FLATMAP → FLATTEN
SORTED  → ORDER
DISTINCT → UNIQUE
LIMIT   → TAKE
SKIP    → IGNORE
REDUCE  → COMBINE
COLLECT → ACCUMULATE
MATCH   → CHECK
FIND    → SEARCH
```

## 🔥 Stream Pipeline Formula

```text
SOURCE
  ↓
0 or more INTERMEDIATE OPERATIONS
  ↓
1 TERMINAL OPERATION
  ↓
RESULT
```

Example:

```java
List<Integer> result = numbers.stream()
        .filter(n -> n % 2 == 0)
        .map(n -> n * n)
        .distinct()
        .sorted()
        .toList();
```

Think:

```text
numbers
   ↓
filter → select
   ↓
map → transform
   ↓
distinct → unique
   ↓
sorted → order
   ↓
toList → collect result
```

> 🚀 **Interview mastery rule:** Don't just memorize Stream methods. Be able to explain **what the operation does, whether it is intermediate or terminal, whether it is lazy, whether it is stateful or stateless, what it returns, and what its performance/concurrency implications are.**