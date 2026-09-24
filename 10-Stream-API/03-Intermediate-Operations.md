# 🔄 03 — Intermediate Operations

Intermediate operations are **lazy, chainable Stream operations** used to filter, transform, sort, limit, skip, or otherwise modify elements in a Stream pipeline.

> 💡 **Core idea:** Intermediate operations return another Stream and are not executed immediately. A terminal operation triggers the actual processing.

---

## 📌 Table of Contents

1. [Introduction](#1-introduction)
2. [Why Intermediate Operations?](#2-why-intermediate-operations)
3. [Stream Pipeline](#3-stream-pipeline)
4. [Lazy Evaluation](#4-lazy-evaluation)
5. [filter()](#5-filter)
6. [map()](#6-map)
7. [mapToInt()](#7-maptoint)
8. [mapToLong()](#8-maptolong)
9. [mapToDouble()](#9-maptodouble)
10. [flatMap()](#10-flatmap)
11. [flatMapToInt()](#11-flatmaptoint)
12. [distinct()](#12-distinct)
13. [sorted()](#13-sorted)
14. [sorted(Comparator)](#14-sortedcomparator)
15. [limit()](#15-limit)
16. [skip()](#16-skip)
17. [peek()](#17-peek)
18. [takeWhile()](#18-takewhile)
19. [dropWhile()](#19-dropwhile)
20. [unordered()](#20-unordered)
21. [Stateless vs Stateful Operations](#21-stateless-vs-stateful-operations)
22. [Short-Circuiting](#22-short-circuiting)
23. [Operation Ordering](#23-operation-ordering)
24. [Stream Reuse Trap](#24-stream-reuse-trap)
25. [Common Mistakes](#25-common-mistakes)
26. [Interview Traps](#26-interview-traps)
27. [DSA Connection](#27-dsa-connection)
28. [Top 10 Interview Questions](#28-top-10-interview-questions)
29. [30-Second Interview Answer](#29-30-second-interview-answer)
30. [Cheat Sheet](#30-cheat-sheet)

---

# 1. Introduction

Intermediate operations are operations that process elements of a Stream and return another Stream.

They allow multiple operations to be chained together.

### Definition

> **An intermediate operation is a Stream operation that returns another Stream and is normally evaluated lazily when a terminal operation is invoked.**

Example:

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5);

numbers.stream()
       .filter(n -> n % 2 == 0)
       .map(n -> n * 10)
       .forEach(System.out::println);
```

Output:

```text
20
40
```

Here:

```text
filter() → Intermediate
map()    → Intermediate
forEach() → Terminal
```

---

## Characteristics

Intermediate operations generally:

- return a Stream
- are chainable
- are lazy
- build the Stream pipeline
- do not produce the final result
- execute when a terminal operation is invoked

---

# 2. Why Intermediate Operations?

Intermediate operations make it possible to build a sequence of data-processing steps.

Suppose:

```java
List<Integer> numbers = List.of(5, 2, 8, 1, 4, 7);
```

Requirement:

1. Select even numbers
2. Multiply them by 10
3. Sort them
4. Store the result

Using Streams:

```java
List<Integer> result = numbers.stream()
        .filter(n -> n % 2 == 0)
        .map(n -> n * 10)
        .sorted()
        .toList();
```

Result:

```text
[20, 40, 80]
```

Pipeline:

```text
Source
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

# 3. Stream Pipeline

A Stream pipeline consists of three major parts:

```text
Source
  ↓
Intermediate Operations
  ↓
Terminal Operation
```

Example:

```java
List<String> names = List.of(
        "Aman",
        "Rahul",
        "Alex",
        "Ravi"
);

names.stream()
     .filter(name -> name.length() > 4)
     .map(String::toUpperCase)
     .sorted()
     .forEach(System.out::println);
```

Pipeline:

```text
List
 ↓
stream()
 ↓
filter()
 ↓
map()
 ↓
sorted()
 ↓
forEach()
```

### Important

`filter()`, `map()`, and `sorted()` do not finish the pipeline.

`forEach()` is the terminal operation that triggers execution.

---

# 4. Lazy Evaluation

Intermediate operations are lazy.

Consider:

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5);

Stream<Integer> stream = numbers.stream()
        .filter(n -> {
            System.out.println("Filtering: " + n);
            return n % 2 == 0;
        });
```

Nothing is printed yet.

Why?

Because `filter()` is an intermediate operation.

The pipeline executes only after a terminal operation:

```java
stream.forEach(System.out::println);
```

Now processing starts.

---

## Execution Model

```text
stream()
   ↓
Intermediate operations
   ↓
Pipeline created
   ↓
Terminal operation
   ↓
Pipeline execution
```

---

## Why Lazy Evaluation?

Lazy evaluation can:

- avoid unnecessary processing
- enable short-circuiting
- combine operations
- avoid unnecessary intermediate results
- improve efficiency

Example:

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5);

numbers.stream()
       .filter(n -> n % 2 == 0)
       .map(n -> n * 10)
       .findFirst();
```

The Stream does not need to process every element once the required result is found.

---

# 5. filter()

## Definition

`filter()` selects elements that satisfy a given condition.

### Syntax

```java
Stream<T> filter(Predicate<? super T> predicate);
```

It takes a `Predicate`.

A `Predicate<T>` represents:

```text
T → boolean
```

---

## Example

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5, 6);

List<Integer> evenNumbers = numbers.stream()
        .filter(n -> n % 2 == 0)
        .toList();

System.out.println(evenNumbers);
```

Output:

```text
[2, 4, 6]
```

---

## How It Works

For:

```text
[1, 2, 3, 4, 5, 6]
```

Condition:

```java
n -> n % 2 == 0
```

Processing:

```text
1 → false → removed
2 → true  → kept
3 → false → removed
4 → true  → kept
5 → false → removed
6 → true  → kept
```

Result:

```text
[2, 4, 6]
```

---

## Filtering Strings

```java
List<String> names = List.of(
        "Aman",
        "Rahul",
        "Raj",
        "Alex"
);

List<String> result = names.stream()
        .filter(name -> name.length() > 3)
        .toList();

System.out.println(result);
```

Output:

```text
[Aman, Rahul, Alex]
```

---

## Multiple Filters

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5, 6, 7, 8);

List<Integer> result = numbers.stream()
        .filter(n -> n % 2 == 0)
        .filter(n -> n > 4)
        .toList();

System.out.println(result);
```

Output:

```text
[6, 8]
```

---

## Important Point

`filter()` does not modify the original collection.

```java
List<Integer> numbers = List.of(1, 2, 3, 4);

List<Integer> result = numbers.stream()
        .filter(n -> n % 2 == 0)
        .toList();

System.out.println(numbers);
System.out.println(result);
```

Output:

```text
[1, 2, 3, 4]
[2, 4]
```

---

# 6. map()

## Definition

`map()` transforms every element into another value.

### Syntax

```java
<R> Stream<R> map(Function<? super T, ? extends R> mapper);
```

It takes a `Function`.

A `Function<T, R>` represents:

```text
T → R
```

---

## Example

```java
List<Integer> numbers = List.of(1, 2, 3, 4);

List<Integer> squares = numbers.stream()
        .map(n -> n * n)
        .toList();

System.out.println(squares);
```

Output:

```text
[1, 4, 9, 16]
```

---

## Transformation

```text
1 → 1
2 → 4
3 → 9
4 → 16
```

---

## Integer to String

```java
List<Integer> numbers = List.of(10, 20, 30);

List<String> result = numbers.stream()
        .map(String::valueOf)
        .toList();

System.out.println(result);
```

Output:

```text
[10, 20, 30]
```

---

## String Transformation

```java
List<String> names = List.of("java", "spring", "react");

List<String> result = names.stream()
        .map(String::toUpperCase)
        .toList();

System.out.println(result);
```

Output:

```text
[JAVA, SPRING, REACT]
```

---

## map() vs filter()

| `filter()` | `map()` |
|---|---|
| Selects elements | Transforms elements |
| Uses `Predicate` | Uses `Function` |
| Returns same element type | Can return different type |
| Output size can decrease | Usually preserves number of elements |
| Condition-based | Transformation-based |

Example:

```java
List<Integer> numbers = List.of(1, 2, 3, 4);

List<Integer> filtered = numbers.stream()
        .filter(n -> n % 2 == 0)
        .toList();

List<Integer> mapped = numbers.stream()
        .map(n -> n * 10)
        .toList();
```

Results:

```text
filtered → [2, 4]
mapped   → [10, 20, 30, 40]
```

---

# 7. mapToInt()

`mapToInt()` converts a regular object Stream into an `IntStream`.

### Syntax

```java
IntStream mapToInt(ToIntFunction<? super T> mapper);
```

Example:

```java
List<String> numbers = List.of("10", "20", "30");

IntStream stream = numbers.stream()
        .mapToInt(Integer::parseInt);
```

Now the Stream type is:

```text
Stream<String>
      ↓
mapToInt()
      ↓
IntStream
```

---

## Example with sum()

```java
List<String> numbers = List.of("10", "20", "30");

int sum = numbers.stream()
        .mapToInt(Integer::parseInt)
        .sum();

System.out.println(sum);
```

Output:

```text
60
```

---

## Why IntStream?

Primitive streams avoid unnecessary boxing in many numeric operations.

```text
Stream<Integer>
```

uses wrapper objects.

```text
IntStream
```

works with primitive `int` values.

Useful operations include:

```java
sum()
average()
min()
max()
count()
```

---

# 8. mapToLong()

`mapToLong()` converts a Stream into a `LongStream`.

### Syntax

```java
LongStream mapToLong(ToLongFunction<? super T> mapper);
```

Example:

```java
List<String> numbers = List.of("100000", "200000", "300000");

long sum = numbers.stream()
        .mapToLong(Long::parseLong)
        .sum();

System.out.println(sum);
```

Output:

```text
600000
```

---

# 9. mapToDouble()

`mapToDouble()` converts a Stream into a `DoubleStream`.

### Syntax

```java
DoubleStream mapToDouble(ToDoubleFunction<? super T> mapper);
```

Example:

```java
List<String> values = List.of("10.5", "20.5", "30.5");

double sum = values.stream()
        .mapToDouble(Double::parseDouble)
        .sum();

System.out.println(sum);
```

Output:

```text
61.5
```

---

# 10. flatMap()

## Definition

`flatMap()` is used when each element produces multiple elements or another Stream.

It performs:

```text
map + flatten
```

### Syntax

```java
<R> Stream<R> flatMap(
        Function<? super T, ? extends Stream<? extends R>> mapper
);
```

---

## Problem Without flatMap()

Suppose:

```java
List<List<Integer>> numbers = List.of(
        List.of(1, 2),
        List.of(3, 4),
        List.of(5, 6)
);
```

Using `map()`:

```java
List<Stream<Integer>> result = numbers.stream()
        .map(List::stream)
        .toList();
```

The result is:

```text
List<Stream<Integer>>
```

We still have nested structure.

---

## flatMap() Solution

```java
List<List<Integer>> numbers = List.of(
        List.of(1, 2),
        List.of(3, 4),
        List.of(5, 6)
);

List<Integer> result = numbers.stream()
        .flatMap(List::stream)
        .toList();

System.out.println(result);
```

Output:

```text
[1, 2, 3, 4, 5, 6]
```

---

## Visual Representation

Before:

```text
[
    [1, 2],
    [3, 4],
    [5, 6]
]
```

After `flatMap()`:

```text
[1, 2, 3, 4, 5, 6]
```

---

## map() vs flatMap()

### map()

```java
List<List<Integer>> numbers = List.of(
        List.of(1, 2),
        List.of(3, 4)
);

List<Stream<Integer>> result = numbers.stream()
        .map(List::stream)
        .toList();
```

Conceptually:

```text
Stream<List<Integer>>
        ↓ map()
Stream<Stream<Integer>>
```

### flatMap()

```java
List<Integer> result = numbers.stream()
        .flatMap(List::stream)
        .toList();
```

Conceptually:

```text
Stream<List<Integer>>
        ↓ flatMap()
Stream<Integer>
```

---

## Practical Example

```java
List<String> sentences = List.of(
        "Java is powerful",
        "Streams are useful"
);

List<String> words = sentences.stream()
        .flatMap(sentence -> Arrays.stream(sentence.split(" ")))
        .toList();

System.out.println(words);
```

Output:

```text
[Java, is, powerful, Streams, are, useful]
```

---

# 11. flatMapToInt()

`flatMapToInt()` flattens each element into an `IntStream`.

### Syntax

```java
IntStream flatMapToInt(
        Function<? super T, ? extends IntStream> mapper
);
```

Example:

```java
List<String> values = List.of(
        "10 20",
        "30 40"
);

int sum = values.stream()
        .flatMapToInt(
                value -> Arrays.stream(value.split(" "))
                        .mapToInt(Integer::parseInt)
        )
        .sum();

System.out.println(sum);
```

Output:

```text
100
```

---

# 12. distinct()

## Definition

`distinct()` removes duplicate elements.

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

## How distinct() Works

For object streams, uniqueness is based on equality semantics, effectively using `equals()` and `hashCode()`.

Therefore, custom objects should correctly implement:

```java
equals()
hashCode()
```

Example:

```java
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

        if (!(obj instanceof Student other)) {
            return false;
        }

        return id == other.id;
    }

    @Override
    public int hashCode() {
        return Integer.hashCode(id);
    }
}
```

Then:

```java
List<Student> students = List.of(
        new Student(1, "Aman"),
        new Student(1, "Aman"),
        new Student(2, "Rahul")
);

List<Student> unique = students.stream()
        .distinct()
        .toList();
```

The students with the same logical identity can be removed as duplicates.

---

# 13. sorted()

## Definition

`sorted()` sorts Stream elements according to their natural ordering.

### Syntax

```java
Stream<T> sorted();
```

The element type must have a meaningful natural ordering.

Usually this means implementing:

```java
Comparable<T>
```

---

## Example

```java
List<Integer> numbers = List.of(5, 2, 8, 1, 4);

List<Integer> result = numbers.stream()
        .sorted()
        .toList();

System.out.println(result);
```

Output:

```text
[1, 2, 4, 5, 8]
```

---

## Strings

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
[Alex, Aman, Rahul, Zoya]
```

---

## Natural Ordering

For integers:

```text
ascending numerical order
```

For Strings:

```text
lexicographical order
```

For custom classes:

```text
Comparable
```

usually defines natural ordering.

---

# 14. sorted(Comparator)

`sorted(Comparator)` allows custom sorting.

### Syntax

```java
Stream<T> sorted(Comparator<? super T> comparator);
```

Example:

```java
List<Integer> numbers = List.of(5, 2, 8, 1, 4);

List<Integer> result = numbers.stream()
        .sorted(Comparator.reverseOrder())
        .toList();

System.out.println(result);
```

Output:

```text
[8, 5, 4, 2, 1]
```

---

## Sorting Objects

```java
record Student(String name, int marks) {
}
```

Sort by marks:

```java
List<Student> students = List.of(
        new Student("Aman", 85),
        new Student("Rahul", 92),
        new Student("Alex", 78)
);

List<Student> result = students.stream()
        .sorted(Comparator.comparingInt(Student::marks))
        .toList();
```

---

## Descending Order

```java
List<Student> result = students.stream()
        .sorted(
                Comparator.comparingInt(Student::marks)
                        .reversed()
        )
        .toList();
```

---

## sorted() vs sorted(Comparator)

| `sorted()` | `sorted(Comparator)` |
|---|---|
| Natural ordering | Custom ordering |
| Uses `Comparable` | Uses `Comparator` |
| No argument | Takes Comparator |
| Default sorting | Flexible sorting |

---

# 15. limit()

## Definition

`limit()` keeps only the first N elements.

### Syntax

```java
Stream<T> limit(long maxSize);
```

Example:

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5);

List<Integer> result = numbers.stream()
        .limit(3)
        .toList();

System.out.println(result);
```

Output:

```text
[1, 2, 3]
```

---

## limit() with sorted()

```java
List<Integer> numbers = List.of(50, 10, 40, 20, 30);

List<Integer> result = numbers.stream()
        .sorted()
        .limit(3)
        .toList();

System.out.println(result);
```

Output:

```text
[10, 20, 30]
```

This pattern is very common for:

```text
Top N
```

problems.

---

# 16. skip()

## Definition

`skip()` skips the first N elements.

### Syntax

```java
Stream<T> skip(long n);
```

Example:

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5);

List<Integer> result = numbers.stream()
        .skip(2)
        .toList();

System.out.println(result);
```

Output:

```text
[3, 4, 5]
```

---

## Pagination Concept

A common pagination formula is:

```text
skip = pageNumber × pageSize
limit = pageSize
```

Example:

```java
int pageNumber = 2;
int pageSize = 3;

List<Integer> result = numbers.stream()
        .skip((long) pageNumber * pageSize)
        .limit(pageSize)
        .toList();
```

---

## skip() + limit()

```java
List<Integer> numbers = List.of(
        1, 2, 3, 4, 5,
        6, 7, 8, 9, 10
);

List<Integer> result = numbers.stream()
        .skip(3)
        .limit(4)
        .toList();

System.out.println(result);
```

Output:

```text
[4, 5, 6, 7]
```

---

# 17. peek()

## Definition

`peek()` performs an action on each element as elements pass through the pipeline.

### Syntax

```java
Stream<T> peek(Consumer<? super T> action);
```

It is mainly useful for:

- debugging
- observing pipeline behavior
- temporary logging

---

## Example

```java
List<Integer> numbers = List.of(1, 2, 3, 4);

List<Integer> result = numbers.stream()
        .filter(n -> n % 2 == 0)
        .peek(n -> System.out.println("After filter: " + n))
        .map(n -> n * 10)
        .peek(n -> System.out.println("After map: " + n))
        .toList();

System.out.println(result);
```

Possible output:

```text
After filter: 2
After map: 20
After filter: 4
After map: 40
[20, 40]
```

---

## Important

`peek()` is also lazy.

This alone:

```java
numbers.stream()
       .peek(System.out::println);
```

does not guarantee that anything will be printed because there is no terminal operation.

Add:

```java
numbers.stream()
       .peek(System.out::println)
       .toList();
```

Now the pipeline executes.

---

## Should peek() Be Used for Business Logic?

Generally, no.

Avoid:

```java
stream.peek(x -> database.save(x));
```

Prefer explicit operations for important side effects.

`peek()` is primarily intended for observing elements during stream processing.

---

# 18. takeWhile()

`takeWhile()` is available from Java 9.

It takes elements from the beginning while the predicate remains true.

### Syntax

```java
Stream<T> takeWhile(Predicate<? super T> predicate);
```

Example:

```java
List<Integer> numbers = List.of(2, 4, 6, 8, 9, 10, 12);

List<Integer> result = numbers.stream()
        .takeWhile(n -> n % 2 == 0)
        .toList();

System.out.println(result);
```

Output:

```text
[2, 4, 6, 8]
```

When `9` is encountered:

```text
9 → false
```

`takeWhile()` stops taking subsequent elements.

---

## Important Difference from filter()

`filter()` checks every relevant element.

```java
numbers.stream()
       .filter(n -> n % 2 == 0)
       .toList();
```

Possible result:

```text
[2, 4, 6, 8, 10, 12]
```

`takeWhile()` stops at the first failure.

```java
numbers.stream()
       .takeWhile(n -> n % 2 == 0)
       .toList();
```

Result:

```text
[2, 4, 6, 8]
```

---

# 19. dropWhile()

`dropWhile()` is available from Java 9.

It discards elements from the beginning while the predicate is true.

### Syntax

```java
Stream<T> dropWhile(Predicate<? super T> predicate);
```

Example:

```java
List<Integer> numbers = List.of(2, 4, 6, 8, 9, 10, 12);

List<Integer> result = numbers.stream()
        .dropWhile(n -> n % 2 == 0)
        .toList();

System.out.println(result);
```

Output:

```text
[9, 10, 12]
```

Once the first non-matching element is found, the remaining elements are retained.

---

## takeWhile() vs dropWhile()

| Operation | Behavior |
|---|---|
| `takeWhile()` | Takes while condition is true |
| `dropWhile()` | Drops while condition is true |

Example:

```text
Input:
[2, 4, 6, 8, 9, 10, 12]

takeWhile(even):
[2, 4, 6, 8]

dropWhile(even):
[9, 10, 12]
```

---

# 20. unordered()

`unordered()` tells the Stream that encounter order does not need to be preserved.

### Syntax

```java
Stream<T> unordered();
```

Example:

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5);

List<Integer> result = numbers.stream()
        .unordered()
        .toList();
```

The operation does not necessarily rearrange elements by itself.

Instead, it removes the requirement that the Stream preserve encounter order.

This can potentially provide optimization opportunities, especially with parallel streams.

---

# 21. Stateless vs Stateful Operations

Intermediate operations can be broadly classified into:

```text
Stateless
Stateful
```

---

## Stateless Operations

A stateless operation can process an element without needing information about other elements.

Examples:

```text
filter()
map()
mapToInt()
mapToLong()
mapToDouble()
peek()
```

Example:

```java
numbers.stream()
       .filter(n -> n > 5)
       .map(n -> n * 2);
```

Each element can be handled independently.

---

## Stateful Operations

A stateful operation may need to remember information about previously encountered elements or inspect a larger portion of the input.

Examples:

```text
distinct()
sorted()
```

Potentially:

```text
limit()
skip()
takeWhile()
dropWhile()
```

have additional ordering/short-circuit behavior depending on the Stream and context.

---

## Why Stateful Operations Matter

Consider:

```java
numbers.stream()
       .sorted()
       .forEach(System.out::println);
```

To correctly sort the stream, the implementation generally needs to see enough of the input before producing sorted output.

This is different from:

```java
numbers.stream()
       .map(n -> n * 2)
       .forEach(System.out::println);
```

`map()` can transform elements independently.

---

# 22. Short-Circuiting

Some Stream operations can stop processing once enough information is available.

Intermediate operations such as:

```text
limit()
takeWhile()
```

can participate in short-circuiting pipelines.

Terminal operations such as:

```text
findFirst()
findAny()
anyMatch()
allMatch()
noneMatch()
```

can also short-circuit.

---

## Example

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5);

numbers.stream()
       .filter(n -> {
           System.out.println("Checking " + n);
           return n % 2 == 0;
       })
       .findFirst();
```

The pipeline can stop once the first matching element is found.

Conceptually:

```text
1 → no
2 → yes
   ↓
result found
   ↓
stop
```

It does not need to continue processing all remaining elements.

---

# 23. Operation Ordering

The order of intermediate operations can affect performance and sometimes behavior.

Consider:

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5, 6);
```

### Filter First

```java
List<Integer> result = numbers.stream()
        .filter(n -> n % 2 == 0)
        .map(n -> n * 10)
        .toList();
```

Only even numbers are mapped.

---

## Filter After Map

```java
List<Integer> result = numbers.stream()
        .map(n -> n * 10)
        .filter(n -> n > 30)
        .toList();
```

Now every element is mapped before filtering.

---

## General Principle

If an operation can cheaply eliminate many elements, placing it earlier can reduce later work.

Example:

```java
stream
    .filter(...)
    .map(...)
    .sorted(...)
    .limit(...)
```

can often be more efficient than:

```java
stream
    .map(...)
    .sorted(...)
    .filter(...)
    .limit(...)
```

But operation ordering must always preserve the required semantics.

---

# 24. Stream Reuse Trap

A Stream cannot normally be reused after a terminal operation.

Example:

```java
Stream<Integer> stream = Stream.of(1, 2, 3, 4);

stream.filter(n -> n % 2 == 0)
      .forEach(System.out::println);

stream.filter(n -> n > 2)
      .forEach(System.out::println);
```

The second use throws:

```text
java.lang.IllegalStateException:
stream has already been operated upon or closed
```

---

## Correct Approach

Create a new Stream:

```java
List<Integer> numbers = List.of(1, 2, 3, 4);

numbers.stream()
       .filter(n -> n % 2 == 0)
       .forEach(System.out::println);

numbers.stream()
       .filter(n -> n > 2)
       .forEach(System.out::println);
```

---

# 25. Common Mistakes

## Mistake 1: Expecting Intermediate Operations to Execute Immediately

Wrong assumption:

```java
numbers.stream()
       .filter(n -> {
           System.out.println(n);
           return n > 2;
       });
```

No terminal operation means the pipeline is not executed.

Correct:

```java
numbers.stream()
       .filter(n -> {
           System.out.println(n);
           return n > 2;
       })
       .toList();
```

---

## Mistake 2: Confusing map() and filter()

`filter()`:

```java
.filter(n -> n > 5)
```

means:

```text
Keep elements satisfying condition.
```

`map()`:

```java
.map(n -> n * 2)
```

means:

```text
Transform every element.
```

---

## Mistake 3: Using map() When flatMap() Is Required

Nested data:

```java
List<List<Integer>> numbers = List.of(
        List.of(1, 2),
        List.of(3, 4)
);
```

Use:

```java
numbers.stream()
       .flatMap(List::stream)
       .toList();
```

when a flattened Stream is required.

---

## Mistake 4: Misusing peek()

Do not use `peek()` as the primary mechanism for important business logic.

Prefer:

```java
forEach(...)
```

or explicit processing where side effects are intentionally required.

---

## Mistake 5: Reusing a Stream

A Stream is not a reusable data structure.

Instead of:

```java
Stream<Integer> stream = numbers.stream();
```

and trying to use it multiple times, create a new Stream from the source.

---

# 26. Interview Traps

### Trap 1

**Are intermediate operations eager or lazy?**

Answer:

> Intermediate operations are generally lazy.

---

### Trap 2

**Does filter() modify the original collection?**

No.

It produces another Stream containing elements that satisfy the predicate.

---

### Trap 3

**Does map() always return the same type?**

No.

`map()` can transform:

```text
T → R
```

Example:

```java
List<String> result = List.of(1, 2, 3)
        .stream()
        .map(String::valueOf)
        .toList();
```

Here:

```text
Integer → String
```

---

### Trap 4

**What is the difference between map() and flatMap()?**

`map()` performs one-to-one transformation conceptually:

```text
T → R
```

`flatMap()` handles one-to-many transformations and flattens the resulting Streams:

```text
T → Stream<R>
```

---

### Trap 5

**Is sorted() stateless?**

No.

Sorting is a stateful intermediate operation because ordering requires consideration of multiple elements.

---

### Trap 6

**Is peek() guaranteed to execute?**

No.

It executes only when the pipeline is actually traversed.

---

### Trap 7

**Can a Stream be reused?**

No.

Once a terminal operation has consumed the Stream, it cannot normally be reused.

---

### Trap 8

**What does unordered() do?**

It removes the requirement to preserve encounter order.

It does not simply mean:

```text
"randomly shuffle the elements"
```

---

# 27. DSA Connection

Intermediate Stream operations are useful for many DSA-style problems.

---

## Pattern 1: Filtering

Problem:

> Find all even numbers.

```java
List<Integer> result = numbers.stream()
        .filter(n -> n % 2 == 0)
        .toList();
```

Pattern:

```text
Selection → filter()
```

---

## Pattern 2: Transformation

Problem:

> Generate squares.

```java
List<Integer> result = numbers.stream()
        .map(n -> n * n)
        .toList();
```

Pattern:

```text
Transformation → map()
```

---

## Pattern 3: Remove Duplicates

Problem:

> Remove duplicate values.

```java
List<Integer> result = numbers.stream()
        .distinct()
        .toList();
```

Pattern:

```text
Deduplication → distinct()
```

---

## Pattern 4: Top K Elements

Problem:

> Find the three largest numbers.

```java
List<Integer> result = numbers.stream()
        .sorted(Comparator.reverseOrder())
        .limit(3)
        .toList();
```

Pattern:

```text
Sort
 ↓
Limit K
```

---

## Pattern 5: Flatten Nested Data

Problem:

> Flatten a list of lists.

```java
List<Integer> result = nested.stream()
        .flatMap(List::stream)
        .toList();
```

Pattern:

```text
Nested collection → flatMap()
```

---

## Pattern 6: Pagination

```java
List<Integer> page = numbers.stream()
        .skip((long) pageNumber * pageSize)
        .limit(pageSize)
        .toList();
```

Pattern:

```text
skip()
 +
limit()
```

---

## Problem-Solving Mindset

When solving a Stream-based DSA problem, ask:

```text
1. Do I need to remove elements?
       ↓
   filter()

2. Do I need to transform elements?
       ↓
   map()

3. Do I have nested collections?
       ↓
   flatMap()

4. Do I need unique values?
       ↓
   distinct()

5. Do I need ordering?
       ↓
   sorted()

6. Do I need only first K?
       ↓
   limit()

7. Do I need to ignore first K?
       ↓
   skip()
```

---

# 28. Top 10 Interview Questions

## Q1. What is an intermediate operation?

### Answer

An intermediate operation is a Stream operation that returns another Stream and is generally evaluated lazily when a terminal operation is invoked.

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

## Q2. Why are intermediate operations lazy?

### Answer

Lazy evaluation allows the Stream pipeline to delay processing until a terminal operation is requested. This enables operation fusion, avoids unnecessary work, and supports short-circuiting.

---

## Q3. What is the difference between map() and filter()?

### Answer

`filter()` selects elements based on a condition.

`map()` transforms each element into another value.

```text
filter → selection
map    → transformation
```

---

## Q4. What is the difference between map() and flatMap()?

### Answer

`map()` transforms each element into another value, while `flatMap()` transforms each element into a Stream and then flattens those Streams into a single Stream.

---

## Q5. What does distinct() use to identify duplicates?

### Answer

For object Streams, `distinct()` relies on equality semantics, so correct `equals()` and `hashCode()` implementations are important for custom objects.

---

## Q6. What is the difference between sorted() and sorted(Comparator)?

### Answer

`sorted()` uses natural ordering, usually defined through `Comparable`.

`sorted(Comparator)` uses a supplied Comparator for custom ordering.

---

## Q7. What is peek() used for?

### Answer

`peek()` is mainly intended for observing elements as they pass through a pipeline, especially for debugging or logging.

---

## Q8. What is flatMap() used for?

### Answer

`flatMap()` is used to flatten nested structures such as:

```text
Stream<List<T>>
```

into:

```text
Stream<T>
```

---

## Q9. Can intermediate operations execute without a terminal operation?

### Answer

Normally, no. Intermediate operations are lazy and only participate in actual processing when a terminal operation consumes the Stream.

---

## Q10. Can a Stream be reused?

### Answer

No. Once a terminal operation has consumed a Stream, attempting to operate on it again normally results in `IllegalStateException`.

---

# 29. 30-Second Interview Answer

> **Intermediate operations in Java Streams are lazy operations that return another Stream and allow us to build a processing pipeline. Common examples are filter(), map(), flatMap(), distinct(), sorted(), limit(), and skip(). They don't produce the final result themselves; execution normally starts when a terminal operation is invoked. Their laziness enables efficient processing, operation chaining, and short-circuiting.**

---

# 30. Cheat Sheet

| Operation | Purpose | Functional Interface | Returns |
|---|---|---|---|
| `filter()` | Select elements | `Predicate<T>` | `Stream<T>` |
| `map()` | Transform elements | `Function<T,R>` | `Stream<R>` |
| `mapToInt()` | Convert to int stream | `ToIntFunction<T>` | `IntStream` |
| `mapToLong()` | Convert to long stream | `ToLongFunction<T>` | `LongStream` |
| `mapToDouble()` | Convert to double stream | `ToDoubleFunction<T>` | `DoubleStream` |
| `flatMap()` | Flatten nested streams | `Function<T,Stream<R>>` | `Stream<R>` |
| `flatMapToInt()` | Flatten to int stream | Function | `IntStream` |
| `distinct()` | Remove duplicates | — | `Stream<T>` |
| `sorted()` | Natural sorting | — | `Stream<T>` |
| `sorted(Comparator)` | Custom sorting | `Comparator<T>` | `Stream<T>` |
| `limit()` | Keep first N | — | `Stream<T>` |
| `skip()` | Skip first N | — | `Stream<T>` |
| `peek()` | Observe elements | `Consumer<T>` | `Stream<T>` |
| `takeWhile()` | Take while condition true | `Predicate<T>` | `Stream<T>` |
| `dropWhile()` | Drop while condition true | `Predicate<T>` | `Stream<T>` |
| `unordered()` | Remove order requirement | — | `Stream<T>` |

---

## 🧠 Memory Trick

```text
filter  → SELECT
map     → CHANGE
flatMap → FLATTEN
distinct → UNIQUE
sorted  → ORDER
limit   → FIRST N
skip    → IGNORE FIRST N
peek    → WATCH
takeWhile → TAKE UNTIL FALSE
dropWhile → DROP UNTIL FALSE
unordered → ORDER NOT REQUIRED
```

---

## 🔥 Most Important Interview Concepts

```text
Intermediate operations
        ↓
Lazy
        ↓
Return Stream
        ↓
Chainable
        ↓
Build pipeline
        ↓
Terminal operation triggers execution
```

Remember:

```text
filter()  → "Should I keep this?"
map()     → "What should this become?"
flatMap() → "How do I flatten this?"
distinct() → "Have I seen this value already?"
sorted()  → "How should elements be ordered?"
limit()   → "How many should I keep?"
skip()    → "How many should I ignore?"
peek()    → "What is passing through?"
```

---

# 🎯 Final Takeaway

Intermediate operations are the **processing layer of the Stream pipeline**.

They allow us to:

```text
SELECT
   ↓
TRANSFORM
   ↓
FLATTEN
   ↓
REMOVE DUPLICATES
   ↓
SORT
   ↓
LIMIT / SKIP
   ↓
TERMINAL OPERATION
```

The most important concepts to remember are:

> **Intermediate operations are lazy, return Streams, are chainable, and normally execute only when a terminal operation consumes the pipeline.**