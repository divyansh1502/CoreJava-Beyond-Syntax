# 🔥 05 — map(), filter(), reduce()

`map()`, `filter()`, and `reduce()` are three of the most important Stream API operations.

They represent three fundamental data-processing patterns:

```text
filter() → SELECT
map()    → TRANSFORM
reduce() → COMBINE
```

> 💡 **Core idea:** `filter()` decides which elements should remain, `map()` transforms elements, and `reduce()` combines multiple elements into a single result.

---

## 📌 Table of Contents

1. [Introduction](#1-introduction)
2. [The Three Core Patterns](#2-the-three-core-patterns)
3. [filter()](#3-filter)
4. [map()](#4-map)
5. [reduce()](#5-reduce)
6. [Identity in reduce()](#6-identity-in-reduce)
7. [map() + filter()](#7-map--filter)
8. [filter() + reduce()](#8-filter--reduce)
9. [map() + reduce()](#9-map--reduce)
10. [map() + filter() + reduce()](#10-map--filter--reduce)
11. [Execution Order](#11-execution-order)
12. [map() vs filter()](#12-map-vs-filter)
13. [reduce() vs collect()](#13-reduce-vs-collect)
14. [reduce() vs sum()](#14-reduce-vs-sum)
15. [Primitive Streams](#15-primitive-streams)
16. [Common Patterns](#16-common-patterns)
17. [DSA Connection](#17-dsa-connection)
18. [Common Mistakes](#18-common-mistakes)
19. [Interview Traps](#19-interview-traps)
20. [Top 15 Interview Questions](#20-top-15-interview-questions)
21. [30-Second Interview Answer](#21-30-second-interview-answer)
22. [Cheat Sheet](#22-cheat-sheet)

---

# 1. Introduction

Most Stream-based data-processing problems can be understood using three operations:

```text
filter()
map()
reduce()
```

Suppose we have:

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5);
```

We want:

> Take even numbers, square them, and calculate their sum.

Pipeline:

```text
[1, 2, 3, 4, 5]
        ↓
     filter()
        ↓
     [2, 4]
        ↓
      map()
        ↓
    [4, 16]
        ↓
     reduce()
        ↓
       20
```

Code:

```java
int result = numbers.stream()
        .filter(n -> n % 2 == 0)
        .map(n -> n * n)
        .reduce(0, Integer::sum);

System.out.println(result);
```

Output:

```text
20
```

---

# 2. The Three Core Patterns

## filter()

```text
SELECT
```

Question:

> Which elements should remain?

Example:

```java
numbers.stream()
        .filter(n -> n > 3);
```

---

## map()

```text
TRANSFORM
```

Question:

> What should each element become?

Example:

```java
numbers.stream()
        .map(n -> n * 2);
```

---

## reduce()

```text
COMBINE
```

Question:

> How can all elements be combined into one result?

Example:

```java
numbers.stream()
        .reduce(0, Integer::sum);
```

---

## Mental Model

```text
FILTER → What should stay?
MAP    → What should it become?
REDUCE → What should everything become together?
```

---

# 3. filter()

## Definition

`filter()` selects elements that satisfy a condition.

### Syntax

```java
Stream<T> filter(Predicate<? super T> predicate);
```

It accepts:

```text
Predicate<T>
```

A Predicate represents:

```text
T → boolean
```

---

## Basic Example

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5, 6);

List<Integer> result = numbers.stream()
        .filter(n -> n % 2 == 0)
        .toList();

System.out.println(result);
```

Output:

```text
[2, 4, 6]
```

---

## Internal Concept

For every element:

```text
element
   ↓
predicate
   ↓
true? ── yes → keep
   │
   no
   ↓
discard
```

Example:

```text
1 → false → discard
2 → true  → keep
3 → false → discard
4 → true  → keep
5 → false → discard
6 → true  → keep
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

## Multiple Conditions

```java
List<Integer> numbers = List.of(
        5, 10, 15, 20, 25, 30
);

List<Integer> result = numbers.stream()
        .filter(n -> n > 10)
        .filter(n -> n % 5 == 0)
        .toList();

System.out.println(result);
```

Output:

```text
[15, 20, 25, 30]
```

---

## One Predicate with &&

```java
List<Integer> result = numbers.stream()
        .filter(n -> n > 10 && n % 5 == 0)
        .toList();
```

Both approaches can represent the same logical condition.

---

# 4. map()

## Definition

`map()` transforms every element into another value.

### Syntax

```java
<R> Stream<R> map(Function<? super T, ? extends R> mapper);
```

It accepts:

```text
Function<T, R>
```

A Function represents:

```text
T → R
```

---

## Basic Example

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

## Changing the Data Type

`map()` can change the element type.

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

Conceptually:

```text
Stream<Integer>
      ↓
    map()
      ↓
Stream<String>
```

---

## Object Transformation

```java
record Student(String name, int marks) {
}
```

```java
List<Student> students = List.of(
        new Student("Aman", 85),
        new Student("Rahul", 92),
        new Student("Alex", 78)
);

List<String> names = students.stream()
        .map(Student::name)
        .toList();

System.out.println(names);
```

Output:

```text
[Aman, Rahul, Alex]
```

---

# 5. reduce()

## Definition

`reduce()` combines Stream elements into a single result.

It is a **reduction operation**.

### Syntax

```java
Optional<T> reduce(BinaryOperator<T> accumulator);
```

Another important overload:

```java
T reduce(T identity, BinaryOperator<T> accumulator);
```

---

## Basic Sum

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5);

int sum = numbers.stream()
        .reduce(0, (a, b) -> a + b);

System.out.println(sum);
```

Output:

```text
15
```

---

## Method Reference

The same operation can be written:

```java
int sum = numbers.stream()
        .reduce(0, Integer::sum);
```

---

## Reduction Process

For:

```text
[1, 2, 3, 4, 5]
```

The reduction conceptually combines values:

```text
0 + 1 → 1
1 + 2 → 3
3 + 3 → 6
6 + 4 → 10
10 + 5 → 15
```

Final result:

```text
15
```

---

## Product

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5);

int product = numbers.stream()
        .reduce(1, (a, b) -> a * b);

System.out.println(product);
```

Output:

```text
120
```

Identity:

```text
1
```

because:

```text
1 × x = x
```

---

# 6. Identity in reduce()

The identity is the starting value of the reduction.

### Addition

```java
int sum = numbers.stream()
        .reduce(0, Integer::sum);
```

Identity:

```text
0
```

Because:

```text
0 + x = x
```

---

### Multiplication

```java
int product = numbers.stream()
        .reduce(1, (a, b) -> a * b);
```

Identity:

```text
1
```

Because:

```text
1 × x = x
```

---

## Why Identity Matters

The identity should be a neutral value for the operation.

Examples:

| Operation | Identity |
|---|---:|
| Addition | `0` |
| Multiplication | `1` |
| String concatenation | `""` |

Example:

```java
String result = Stream.of("Java", " ", "Stream")
        .reduce("", String::concat);

System.out.println(result);
```

Output:

```text
Java Stream
```

---

## Wrong Identity

Be careful with:

```java
int sum = numbers.stream()
        .reduce(10, Integer::sum);
```

The result becomes:

```text
25
```

because the identity is included in the reduction.

The identity is not simply a value that is ignored.

---

# 7. map() + filter()

`map()` and `filter()` are frequently combined.

Example:

> Find squares of even numbers.

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5, 6);

List<Integer> result = numbers.stream()
        .filter(n -> n % 2 == 0)
        .map(n -> n * n)
        .toList();

System.out.println(result);
```

Output:

```text
[4, 16, 36]
```

Pipeline:

```text
[1,2,3,4,5,6]
        ↓
     filter
        ↓
     [2,4,6]
        ↓
      map
        ↓
   [4,16,36]
```

---

## Why filter First?

Compare:

```java
numbers.stream()
        .filter(n -> n % 2 == 0)
        .map(n -> n * n)
        .toList();
```

with:

```java
numbers.stream()
        .map(n -> n * n)
        .filter(n -> n % 2 == 0)
        .toList();
```

For this specific condition, both can produce the same result.

But filtering first can avoid unnecessary transformations.

General principle:

> If a cheap filter can eliminate many elements, applying it earlier can reduce downstream work.

---

# 8. filter() + reduce()

Example:

> Find the sum of all even numbers.

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5, 6);

int sum = numbers.stream()
        .filter(n -> n % 2 == 0)
        .reduce(0, Integer::sum);

System.out.println(sum);
```

Output:

```text
12
```

Pipeline:

```text
[1,2,3,4,5,6]
        ↓
     filter
        ↓
     [2,4,6]
        ↓
     reduce
        ↓
       12
```

---

# 9. map() + reduce()

Example:

> Find the sum of squares.

```java
List<Integer> numbers = List.of(1, 2, 3, 4);

int result = numbers.stream()
        .map(n -> n * n)
        .reduce(0, Integer::sum);

System.out.println(result);
```

Output:

```text
30
```

Because:

```text
1² + 2² + 3² + 4²
= 1 + 4 + 9 + 16
= 30
```

---

## Another Example

> Find the total length of all names.

```java
List<String> names = List.of(
        "Aman",
        "Rahul",
        "Alex"
);

int totalLength = names.stream()
        .map(String::length)
        .reduce(0, Integer::sum);

System.out.println(totalLength);
```

Output:

```text
12
```

---

# 10. map() + filter() + reduce()

This is one of the most important Stream patterns.

Problem:

> Find the sum of squares of even numbers.

Input:

```text
[1, 2, 3, 4, 5, 6]
```

Pipeline:

```text
filter even
    ↓
[2, 4, 6]

map square
    ↓
[4, 16, 36]

reduce sum
    ↓
56
```

Code:

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5, 6);

int result = numbers.stream()
        .filter(n -> n % 2 == 0)
        .map(n -> n * n)
        .reduce(0, Integer::sum);

System.out.println(result);
```

Output:

```text
56
```

---

## Real-World Example

Suppose we have:

```java
record Employee(
        String name,
        String department,
        int salary
) {
}
```

We want:

> Total salary of employees in the IT department.

```java
List<Employee> employees = List.of(
        new Employee("Aman", "IT", 50000),
        new Employee("Rahul", "HR", 45000),
        new Employee("Alex", "IT", 60000),
        new Employee("Ravi", "Sales", 40000)
);

int totalSalary = employees.stream()
        .filter(employee -> employee.department().equals("IT"))
        .map(Employee::salary)
        .reduce(0, Integer::sum);

System.out.println(totalSalary);
```

Output:

```text
110000
```

Pipeline:

```text
Employees
    ↓
filter department = IT
    ↓
IT employees
    ↓
map salary
    ↓
[50000, 60000]
    ↓
reduce sum
    ↓
110000
```

---

# 11. Execution Order

A common misconception is that Streams always process the entire result of one intermediate operation before moving to the next.

Conceptually, Stream pipelines can be fused and process elements through multiple stages.

Example:

```java
List<Integer> numbers = List.of(1, 2, 3, 4);

numbers.stream()
        .filter(n -> {
            System.out.println("filter: " + n);
            return n % 2 == 0;
        })
        .map(n -> {
            System.out.println("map: " + n);
            return n * 10;
        })
        .forEach(n -> System.out.println("result: " + n));
```

Possible output:

```text
filter: 1
filter: 2
map: 2
result: 20
filter: 3
filter: 4
map: 4
result: 40
```

Notice that the pipeline can process an element through multiple stages before moving to the next element.

Conceptually:

```text
Element 1
   ↓
filter
   ↓
discard

Element 2
   ↓
filter
   ↓
map
   ↓
terminal operation

Element 3
   ↓
filter
   ↓
discard

Element 4
   ↓
filter
   ↓
map
   ↓
terminal operation
```

This is one reason Stream pipelines can be efficient.

---

# 12. map() vs filter()

| Feature | `filter()` | `map()` |
|---|---|---|
| Purpose | Select | Transform |
| Functional interface | `Predicate` | `Function` |
| Main result | Keeps/discards | Converts |
| Return type | `Stream<T>` | `Stream<R>` |
| Can change type? | No | Yes |
| Typical question | "Should I keep it?" | "What should it become?" |

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
filter → [2, 4]
map    → [10, 20, 30, 40]
```

---

# 13. reduce() vs collect()

Both can produce a final result, but their purposes differ.

## reduce()

Best for combining values into a single result.

Examples:

```text
sum
product
maximum
minimum
concatenation
```

Example:

```java
int sum = numbers.stream()
        .reduce(0, Integer::sum);
```

---

## collect()

Best for building result containers.

Examples:

```text
List
Set
Map
grouping
partitioning
joining
```

Example:

```java
List<Integer> result = numbers.stream()
        .filter(n -> n % 2 == 0)
        .collect(Collectors.toList());
```

---

## Rule of Thumb

```text
One combined value → reduce()

Collection/result container → collect()
```

---

# 14. reduce() vs sum()

For numeric Streams, `sum()` is usually more direct than manually using `reduce()`.

Example:

```java
int sum = numbers.stream()
        .mapToInt(Integer::intValue)
        .sum();
```

Compared with:

```java
int sum = numbers.stream()
        .reduce(0, Integer::sum);
```

Both can calculate the sum.

But primitive Stream operations are often clearer for straightforward numeric aggregation.

---

## Common Numeric Operations

```java
int sum = numbers.stream()
        .mapToInt(Integer::intValue)
        .sum();

OptionalInt min = numbers.stream()
        .mapToInt(Integer::intValue)
        .min();

OptionalInt max = numbers.stream()
        .mapToInt(Integer::intValue)
        .max();

double average = numbers.stream()
        .mapToInt(Integer::intValue)
        .average()
        .orElse(0.0);
```

---

# 15. Primitive Streams

For numerical processing, Java provides:

```text
IntStream
LongStream
DoubleStream
```

Instead of:

```text
Stream<Integer>
Stream<Long>
Stream<Double>
```

---

## mapToInt()

Example:

```java
List<String> values = List.of("10", "20", "30");

int sum = values.stream()
        .mapToInt(Integer::parseInt)
        .sum();

System.out.println(sum);
```

Output:

```text
60
```

---

## mapToInt() + filter() + sum()

```java
List<String> values = List.of(
        "10", "15", "20", "25"
);

int result = values.stream()
        .mapToInt(Integer::parseInt)
        .filter(n -> n % 2 == 0)
        .sum();

System.out.println(result);
```

Output:

```text
30
```

Because:

```text
10 + 20 = 30
```

---

# 16. Common Patterns

## Pattern 1: Filter Even Numbers

```java
List<Integer> result = numbers.stream()
        .filter(n -> n % 2 == 0)
        .toList();
```

---

## Pattern 2: Square Every Number

```java
List<Integer> result = numbers.stream()
        .map(n -> n * n)
        .toList();
```

---

## Pattern 3: Sum All Numbers

```java
int result = numbers.stream()
        .reduce(0, Integer::sum);
```

---

## Pattern 4: Sum Even Numbers

```java
int result = numbers.stream()
        .filter(n -> n % 2 == 0)
        .reduce(0, Integer::sum);
```

---

## Pattern 5: Sum Squares

```java
int result = numbers.stream()
        .map(n -> n * n)
        .reduce(0, Integer::sum);
```

---

## Pattern 6: Sum Squares of Even Numbers

```java
int result = numbers.stream()
        .filter(n -> n % 2 == 0)
        .map(n -> n * n)
        .reduce(0, Integer::sum);
```

---

## Pattern 7: Count Positive Numbers

```java
long count = numbers.stream()
        .filter(n -> n > 0)
        .count();
```

---

## Pattern 8: Find Maximum

```java
Optional<Integer> max = numbers.stream()
        .reduce(Integer::max);
```

---

## Pattern 9: Convert Names to Uppercase

```java
List<String> result = names.stream()
        .map(String::toUpperCase)
        .toList();
```

---

## Pattern 10: Filter + Map Objects

```java
List<String> result = employees.stream()
        .filter(employee -> employee.salary() > 50000)
        .map(Employee::name)
        .toList();
```

---

# 17. DSA Connection

`filter()`, `map()`, and `reduce()` correspond closely to common DSA patterns.

---

## 17.1 Filtering = Selection

Traditional loop:

```java
List<Integer> result = new ArrayList<>();

for (int number : numbers) {
    if (number % 2 == 0) {
        result.add(number);
    }
}
```

Stream:

```java
List<Integer> result = numbers.stream()
        .filter(n -> n % 2 == 0)
        .toList();
```

Pattern:

```text
Condition
   ↓
Select elements
```

---

## 17.2 Mapping = Transformation

Traditional loop:

```java
List<Integer> result = new ArrayList<>();

for (int number : numbers) {
    result.add(number * 2);
}
```

Stream:

```java
List<Integer> result = numbers.stream()
        .map(n -> n * 2)
        .toList();
```

Pattern:

```text
Input
  ↓
Transformation
  ↓
Output
```

---

## 17.3 Reduction = Aggregation

Traditional loop:

```java
int sum = 0;

for (int number : numbers) {
    sum += number;
}
```

Stream:

```java
int sum = numbers.stream()
        .reduce(0, Integer::sum);
```

Pattern:

```text
Many values
    ↓
Accumulator
    ↓
One value
```

---

## 17.4 Three Fundamental DSA Questions

When solving a problem, ask:

```text
1. Which elements do I need?
        ↓
     filter()

2. What should those elements become?
        ↓
      map()

3. How should I combine them?
        ↓
     reduce()
```

---

# 18. Common Mistakes

## Mistake 1: Using map() for Filtering

Incorrect thinking:

```java
.map(n -> n % 2 == 0)
```

This produces:

```text
Stream<Boolean>
```

It does not remove elements.

Use:

```java
.filter(n -> n % 2 == 0)
```

---

## Mistake 2: Forgetting the Terminal Operation

This does not consume the Stream:

```java
numbers.stream()
        .filter(n -> n > 5)
        .map(n -> n * 2);
```

Use:

```java
numbers.stream()
        .filter(n -> n > 5)
        .map(n -> n * 2)
        .toList();
```

---

## Mistake 3: Wrong Identity

For sum:

```java
.reduce(0, Integer::sum);
```

For product:

```java
.reduce(1, (a, b) -> a * b);
```

Do not arbitrarily choose an identity.

---

## Mistake 4: Using reduce() to Build Collections

Avoid manually accumulating a mutable collection with `reduce()`.

Prefer:

```java
.collect(Collectors.toList());
```

or:

```java
.toList();
```

---

## Mistake 5: Ignoring Type Changes

This:

```java
.map(String::valueOf)
```

changes:

```text
Stream<Integer>
```

into:

```text
Stream<String>
```

Always track the Stream's element type.

---

## Mistake 6: Forgetting Primitive Streams

For numerical calculations, consider:

```java
mapToInt()
mapToLong()
mapToDouble()
```

instead of unnecessary boxing.

---

# 19. Interview Traps

### Trap 1

**What does filter() return?**

```text
Stream<T>
```

It does not return a boolean.

The predicate returns boolean.

---

### Trap 2

**What does map() return?**

```text
Stream<R>
```

The output type can differ from the input type.

---

### Trap 3

**What does reduce() return?**

It depends on the overload.

Without identity:

```java
Optional<T>
```

With identity:

```java
T
```

---

### Trap 4

**Is reduce() an intermediate or terminal operation?**

Terminal.

---

### Trap 5

**Is map() lazy?**

Yes.

`map()` is an intermediate operation.

---

### Trap 6

**Can filter() change the number of elements?**

Yes.

It can remove elements.

---

### Trap 7

**Can map() change the number of elements?**

Ordinary `map()` conceptually performs one output transformation per input element, so it normally preserves the number of elements.

`flatMap()` is used for one-to-many transformations and can change the number of resulting elements.

---

### Trap 8

**Why can reduce() return Optional?**

Because the Stream may be empty when no identity value is supplied.

---

### Trap 9

**Can reduce() be used with parallel streams?**

Yes, but the accumulator and identity must satisfy the required reduction properties for correct parallel behavior.

---

### Trap 10

**Why is filter often placed before map?**

If filtering can eliminate elements cheaply, applying it first can reduce the number of elements that need transformation.

---

# 20. Top 15 Interview Questions

## Q1. What are filter(), map(), and reduce()?

They represent three fundamental Stream operations:

```text
filter → select
map    → transform
reduce → combine
```

---

## Q2. What functional interface does filter() use?

```java
Predicate<T>
```

It returns:

```text
boolean
```

---

## Q3. What functional interface does map() use?

```java
Function<T, R>
```

It transforms:

```text
T → R
```

---

## Q4. What functional interface does reduce() commonly use?

The two-argument form uses:

```java
BinaryOperator<T>
```

which represents:

```text
(T, T) → T
```

---

## Q5. Is filter() lazy?

Yes.

It is an intermediate operation and executes when a terminal operation consumes the pipeline.

---

## Q6. Is map() lazy?

Yes.

It is an intermediate operation.

---

## Q7. Is reduce() lazy?

No.

`reduce()` is a terminal operation and triggers Stream processing.

---

## Q8. What is the purpose of identity in reduce()?

The identity provides the initial value and acts as the neutral value for the reduction operation.

Examples:

```text
sum → 0
product → 1
```

---

## Q9. What is the difference between map() and flatMap()?

`map()` transforms each element into one result.

`flatMap()` transforms elements into Streams and then flattens them into one Stream.

---

## Q10. Can map() change the element type?

Yes.

Example:

```java
List<String> result = List.of(1, 2, 3)
        .stream()
        .map(String::valueOf)
        .toList();
```

---

## Q11. Can filter() change the element type?

No.

`filter()` selects or removes elements but preserves the Stream element type.

---

## Q12. What is a common use of reduce()?

Aggregation:

```text
sum
product
maximum
minimum
concatenation
```

---

## Q13. Why use mapToInt() instead of map() for numeric operations?

`mapToInt()` produces an `IntStream`, which provides primitive numeric operations such as:

```text
sum()
average()
min()
max()
```

and can avoid unnecessary boxing.

---

## Q14. Why should filter() often come before map()?

Because filtering first may reduce the number of elements that need transformation.

---

## Q15. Explain this pipeline:

```java
int result = numbers.stream()
        .filter(n -> n % 2 == 0)
        .map(n -> n * n)
        .reduce(0, Integer::sum);
```

Answer:

```text
filter → keeps even numbers
map    → squares them
reduce → adds all squares
```

---

# 21. 30-Second Interview Answer

> **filter(), map(), and reduce() represent three fundamental Stream processing patterns. filter() selects elements using a Predicate, map() transforms elements using a Function, and reduce() combines multiple elements into a single result. For example, I can filter even numbers, map them to their squares, and reduce them into a sum. filter() and map() are lazy intermediate operations, while reduce() is a terminal operation that triggers execution.**

---

# 22. Cheat Sheet

| Operation | Role | Functional Interface | Return |
|---|---|---|---|
| `filter()` | Select | `Predicate<T>` | `Stream<T>` |
| `map()` | Transform | `Function<T,R>` | `Stream<R>` |
| `mapToInt()` | Numeric transform | `ToIntFunction<T>` | `IntStream` |
| `mapToLong()` | Numeric transform | `ToLongFunction<T>` | `LongStream` |
| `mapToDouble()` | Numeric transform | `ToDoubleFunction<T>` | `DoubleStream` |
| `reduce()` | Combine | `BinaryOperator<T>` | `Optional<T>` / `T` |

---

## 🧠 Ultimate Memory Trick

```text
FILTER
"Which elements should survive?"
        ↓
SELECT

MAP
"What should each element become?"
        ↓
TRANSFORM

REDUCE
"How can all elements become one result?"
        ↓
COMBINE
```

---

## 🔥 Most Important Pipeline

```java
int result = numbers.stream()
        .filter(n -> n % 2 == 0)
        .map(n -> n * n)
        .reduce(0, Integer::sum);
```

Think:

```text
INPUT
  ↓
FILTER
  ↓
SELECT
  ↓
MAP
  ↓
TRANSFORM
  ↓
REDUCE
  ↓
COMBINE
  ↓
FINAL RESULT
```

---

# 🎯 Final Takeaway

The three operations can be remembered as:

```text
filter() → SELECT
map()    → TRANSFORM
reduce() → COMBINE
```

A large number of Stream problems can be decomposed into these three questions:

```text
1. What do I keep?
        ↓
    filter()

2. What do I transform?
        ↓
      map()

3. What do I aggregate?
        ↓
     reduce()
```

Once this mental model becomes natural, Stream pipelines become much easier to read, design, and explain in interviews.