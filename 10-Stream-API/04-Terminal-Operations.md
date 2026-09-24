# 🏁 04 — Terminal Operations

Terminal operations are the **final operations of a Stream pipeline**. They trigger the actual execution of the Stream and produce a result, side effect, or another non-Stream value.

> 💡 **Core idea:** Intermediate operations build the pipeline; a terminal operation **consumes the Stream and produces the final result**.

---

## 📌 Table of Contents

1. [Introduction](#1-introduction)
2. [Why Terminal Operations?](#2-why-terminal-operations)
3. [Terminal Operation Categories](#3-terminal-operation-categories)
4. [forEach()](#4-foreach)
5. [forEachOrdered()](#5-foreachordered)
6. [toArray()](#6-toarray)
7. [reduce()](#7-reduce)
8. [reduce() with Identity](#8-reduce-with-identity)
9. [reduce() with Identity and Combiner](#9-reduce-with-identity-and-combiner)
10. [collect()](#10-collect)
11. [toList()](#11-tolist)
12. [toSet()](#12-toset)
13. [toMap()](#13-tomap)
14. [count()](#14-count)
15. [min()](#15-min)
16. [max()](#16-max)
17. [findFirst()](#17-findfirst)
18. [findAny()](#18-findany)
19. [anyMatch()](#19-anymatch)
20. [allMatch()](#20-allmatch)
21. [noneMatch()](#21-nonematch)
22. [Optional in Terminal Operations](#22-optional-in-terminal-operations)
23. [Terminal Operations and Stream Consumption](#23-terminal-operations-and-stream-consumption)
24. [Short-Circuiting Terminal Operations](#24-short-circuiting-terminal-operations)
25. [Mutable Reduction vs Reduction](#25-mutable-reduction-vs-reduction)
26. [Common Mistakes](#26-common-mistakes)
27. [Interview Traps](#27-interview-traps)
28. [DSA Connection](#28-dsa-connection)
29. [Top 10 Interview Questions](#29-top-10-interview-questions)
30. [30-Second Interview Answer](#30-30-second-interview-answer)
31. [Cheat Sheet](#31-cheat-sheet)

---

# 1. Introduction

A **terminal operation** is an operation that ends a Stream pipeline.

Example:

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5);

numbers.stream()
        .filter(n -> n % 2 == 0)
        .map(n -> n * 10)
        .forEach(System.out::println);
```

Here:

```text
filter() → Intermediate
map()    → Intermediate
forEach() → Terminal
```

The terminal operation:

```java
forEach(...)
```

triggers the actual processing of the Stream.

---

## Definition

> **A terminal operation is a Stream operation that consumes the Stream pipeline and produces a final result or side effect.**

---

## Important Properties

Terminal operations:

- trigger Stream execution
- consume the Stream
- end the Stream pipeline
- cannot normally be followed by another Stream operation on the same Stream
- may return a value
- may produce a side effect
- some are short-circuiting

---

# 2. Why Terminal Operations?

Intermediate operations only describe **how data should be processed**.

A terminal operation tells the Stream:

> "Now actually process the data and give me the result."

Example:

```java
List<Integer> numbers = List.of(1, 2, 3, 4);

numbers.stream()
        .filter(n -> n % 2 == 0)
        .map(n -> n * 10);
```

Nothing useful is produced because there is no terminal operation.

Add:

```java
List<Integer> result = numbers.stream()
        .filter(n -> n % 2 == 0)
        .map(n -> n * 10)
        .toList();
```

Now the pipeline executes.

---

# 3. Terminal Operation Categories

Important terminal operations include:

| Category | Operations |
|---|---|
| Iteration | `forEach()`, `forEachOrdered()` |
| Array conversion | `toArray()` |
| Reduction | `reduce()` |
| Collection | `collect()`, `toList()`, `toSet()` |
| Counting | `count()` |
| Min/Max | `min()`, `max()` |
| Finding | `findFirst()`, `findAny()` |
| Matching | `anyMatch()`, `allMatch()`, `noneMatch()` |

---

## Classification

```text
Terminal Operations
│
├── Iteration
│   ├── forEach()
│   └── forEachOrdered()
│
├── Reduction
│   ├── reduce()
│   ├── count()
│   ├── min()
│   └── max()
│
├── Collection
│   ├── collect()
│   ├── toList()
│   └── toSet()
│
├── Finding
│   ├── findFirst()
│   └── findAny()
│
└── Matching
    ├── anyMatch()
    ├── allMatch()
    └── noneMatch()
```

---

# 4. forEach()

## Definition

`forEach()` performs an action for every element in the Stream.

### Syntax

```java
void forEach(Consumer<? super T> action);
```

It accepts a:

```text
Consumer<T>
```

A Consumer represents:

```text
T → void
```

---

## Example

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5);

numbers.stream()
        .forEach(System.out::println);
```

Output:

```text
1
2
3
4
5
```

---

## With Intermediate Operations

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

---

## forEach() Does Not Return a Result

```java
numbers.stream()
        .forEach(System.out::println);
```

Return type:

```text
void
```

Therefore, it is mainly used when you want to perform an action rather than construct a result.

---

# 5. forEachOrdered()

`forEachOrdered()` performs an action for each element while respecting the Stream's encounter order when the Stream has one.

### Syntax

```java
void forEachOrdered(Consumer<? super T> action);
```

Example:

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5);

numbers.parallelStream()
        .forEachOrdered(System.out::println);
```

The output respects encounter order:

```text
1
2
3
4
5
```

---

## forEach() vs forEachOrdered()

| `forEach()` | `forEachOrdered()` |
|---|---|
| Does not guarantee encounter order in all parallel cases | Preserves encounter order when defined |
| Can be useful for parallel processing | Explicitly respects order |
| May provide more freedom for parallel execution | Ordering can reduce parallel flexibility |

---

# 6. toArray()

`toArray()` collects Stream elements into an array.

### Syntax

```java
Object[] toArray();
```

Example:

```java
List<Integer> numbers = List.of(1, 2, 3, 4);

Object[] result = numbers.stream()
        .toArray();

System.out.println(Arrays.toString(result));
```

Output:

```text
[1, 2, 3, 4]
```

---

## Typed Array

A better option when the desired array type is known:

```java
Integer[] result = numbers.stream()
        .toArray(Integer[]::new);

System.out.println(Arrays.toString(result));
```

Output:

```text
[1, 2, 3, 4]
```

---

# 7. reduce()

## Definition

`reduce()` combines Stream elements into a single result.

It is one of the most important **reduction operations**.

### Common Concept

```text
1
↓
1 + 2
↓
3 + 3
↓
6 + 4
↓
10 + 5
↓
15
```

---

## Syntax

One overload is:

```java
Optional<T> reduce(BinaryOperator<T> accumulator);
```

A `BinaryOperator<T>` represents:

```text
(T, T) → T
```

---

## Example

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5);

Optional<Integer> result = numbers.stream()
        .reduce((a, b) -> a + b);

System.out.println(result);
```

Output:

```text
Optional[15]
```

---

## Why Optional?

The Stream may be empty.

For example:

```java
List<Integer> numbers = List.of();

Optional<Integer> result = numbers.stream()
        .reduce((a, b) -> a + b);
```

There is no value to return.

Therefore:

```text
Optional.empty()
```

---

# 8. reduce() with Identity

Another overload accepts an identity value.

### Syntax

```java
T reduce(T identity, BinaryOperator<T> accumulator);
```

Example:

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

## Identity

The identity acts as the initial value.

For addition:

```text
identity = 0
```

For multiplication:

```text
identity = 1
```

Example:

```java
int product = numbers.stream()
        .reduce(1, (a, b) -> a * b);

System.out.println(product);
```

Output:

```text
120
```

---

## Important Identity Rule

The identity should behave as a neutral element.

For addition:

```text
0 + x = x
```

For multiplication:

```text
1 × x = x
```

---

# 9. reduce() with Identity and Combiner

Another overload is:

```java
<R> R reduce(
        R identity,
        BiFunction<R, ? super T, R> accumulator,
        BinaryOperator<R> combiner
);
```

This form is especially useful when the result type differs from the stream element type and for parallel reduction.

Example:

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5);

int sum = numbers.parallelStream()
        .reduce(
                0,
                (total, number) -> total + number,
                Integer::sum
        );

System.out.println(sum);
```

Output:

```text
15
```

---

## Why Combiner?

In parallel processing, data can be divided into multiple portions.

Conceptually:

```text
[1, 2] → partial result
[3, 4] → partial result
[5]    → partial result

       ↓

combine partial results

       ↓

final result
```

The `combiner` combines partial results.

---

# 10. collect()

`collect()` performs a **mutable reduction**.

It is commonly used to collect Stream elements into:

- Lists
- Sets
- Maps
- Strings
- custom result containers

### Syntax

```java
<R, A> R collect(Collector<? super T, A, R> collector);
```

---

## Example

```java
List<Integer> numbers = List.of(1, 2, 3, 4);

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

## Modern toList()

Modern Java also provides:

```java
List<Integer> result = numbers.stream()
        .filter(n -> n % 2 == 0)
        .toList();
```

---

## collect() with Set

```java
List<Integer> numbers = List.of(1, 2, 2, 3, 3, 4);

Set<Integer> result = numbers.stream()
        .collect(Collectors.toSet());

System.out.println(result);
```

Output contains unique values:

```text
[1, 2, 3, 4]
```

---

# 11. toList()

`toList()` collects Stream elements into a List.

Example:

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5);

List<Integer> result = numbers.stream()
        .filter(n -> n % 2 == 0)
        .toList();

System.out.println(result);
```

Output:

```text
[2, 4]
```

---

## Important Property

The List returned by `Stream.toList()` is **unmodifiable**.

Example:

```java
List<Integer> result = Stream.of(1, 2, 3)
        .toList();

result.add(4);
```

This results in:

```text
UnsupportedOperationException
```

---

## Mutable List

If you need a mutable List:

```java
List<Integer> result = Stream.of(1, 2, 3)
        .collect(Collectors.toCollection(ArrayList::new));

result.add(4);
```

---

# 12. toSet()

There is no `Stream.toSet()` method corresponding directly to `toList()`.

Instead, use:

```java
Set<Integer> result = numbers.stream()
        .collect(Collectors.toSet());
```

Example:

```java
List<Integer> numbers = List.of(
        1, 2, 2, 3, 3, 4
);

Set<Integer> result = numbers.stream()
        .collect(Collectors.toSet());

System.out.println(result);
```

Output:

```text
[1, 2, 3, 4]
```

---

# 13. toMap()

`Collectors.toMap()` collects elements into a Map.

Example:

```java
record Student(int id, String name) {
}
```

```java
List<Student> students = List.of(
        new Student(1, "Aman"),
        new Student(2, "Rahul"),
        new Student(3, "Alex")
);

Map<Integer, String> result = students.stream()
        .collect(Collectors.toMap(
                Student::id,
                Student::name
        ));

System.out.println(result);
```

Result:

```text
{1=Aman, 2=Rahul, 3=Alex}
```

---

## Duplicate Key Problem

This can throw an exception:

```java
List<Student> students = List.of(
        new Student(1, "Aman"),
        new Student(1, "Rahul")
);

Map<Integer, String> result = students.stream()
        .collect(Collectors.toMap(
                Student::id,
                Student::name
        ));
```

Because both students have:

```text
key = 1
```

The result cannot determine which value should be stored.

---

## Handling Duplicate Keys

Provide a merge function:

```java
Map<Integer, String> result = students.stream()
        .collect(Collectors.toMap(
                Student::id,
                Student::name,
                (oldValue, newValue) -> oldValue
        ));
```

---

# 14. count()

`count()` returns the number of elements in the Stream.

### Syntax

```java
long count();
```

Example:

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5);

long count = numbers.stream()
        .count();

System.out.println(count);
```

Output:

```text
5
```

---

## With filter()

```java
long evenCount = numbers.stream()
        .filter(n -> n % 2 == 0)
        .count();

System.out.println(evenCount);
```

Output:

```text
2
```

---

## Return Type

Important:

```text
count() → long
```

not:

```text
int
```

---

# 15. min()

`min()` finds the minimum element according to a Comparator.

### Syntax

```java
Optional<T> min(Comparator<? super T> comparator);
```

Example:

```java
List<Integer> numbers = List.of(5, 2, 8, 1, 4);

Optional<Integer> result = numbers.stream()
        .min(Integer::compareTo);

System.out.println(result);
```

Output:

```text
Optional[1]
```

---

## Extracting the Value

```java
int minimum = numbers.stream()
        .min(Integer::compareTo)
        .orElseThrow();

System.out.println(minimum);
```

Output:

```text
1
```

---

# 16. max()

`max()` finds the maximum element according to a Comparator.

### Syntax

```java
Optional<T> max(Comparator<? super T> comparator);
```

Example:

```java
List<Integer> numbers = List.of(5, 2, 8, 1, 4);

Optional<Integer> result = numbers.stream()
        .max(Integer::compareTo);

System.out.println(result);
```

Output:

```text
Optional[8]
```

---

## min() vs max()

| Operation | Purpose |
|---|---|
| `min()` | Finds smallest element |
| `max()` | Finds largest element |

Both return:

```text
Optional<T>
```

because the Stream may be empty.

---

# 17. findFirst()

`findFirst()` returns the first element according to encounter order when such an order exists.

### Syntax

```java
Optional<T> findFirst();
```

Example:

```java
List<Integer> numbers = List.of(10, 20, 30, 40);

Optional<Integer> result = numbers.stream()
        .findFirst();

System.out.println(result);
```

Output:

```text
Optional[10]
```

---

## With filter()

```java
Optional<Integer> result = numbers.stream()
        .filter(n -> n > 15)
        .findFirst();

System.out.println(result);
```

Output:

```text
Optional[20]
```

---

## Short-Circuiting

`findFirst()` is a short-circuiting terminal operation.

Once the first required element is found, processing can stop.

---

# 18. findAny()

`findAny()` returns some element from the Stream.

### Syntax

```java
Optional<T> findAny();
```

Example:

```java
Optional<Integer> result = Stream.of(1, 2, 3, 4, 5)
        .findAny();

System.out.println(result);
```

A possible output:

```text
Optional[1]
```

But `findAny()` does not promise that the first encounter-order element will always be returned.

---

## Why findAny()?

It can be useful with parallel streams where any matching element is sufficient.

Example:

```java
Optional<Integer> result = numbers.parallelStream()
        .filter(n -> n > 10)
        .findAny();
```

---

## findFirst() vs findAny()

| `findFirst()` | `findAny()` |
|---|---|
| Respects encounter order when applicable | May return any element |
| Deterministic for ordered sequential streams | More flexible |
| Useful when first element matters | Useful when any matching element is enough |
| Short-circuiting | Short-circuiting |

---

# 19. anyMatch()

`anyMatch()` checks whether **at least one** element satisfies a condition.

### Syntax

```java
boolean anyMatch(Predicate<? super T> predicate);
```

Example:

```java
List<Integer> numbers = List.of(1, 3, 5, 8, 9);

boolean result = numbers.stream()
        .anyMatch(n -> n % 2 == 0);

System.out.println(result);
```

Output:

```text
true
```

Because:

```text
8 → even
```

---

## Short-Circuiting

Once a matching element is found, the Stream can stop.

```text
1 → false
3 → false
5 → false
8 → true
     ↓
   stop
```

---

# 20. allMatch()

`allMatch()` checks whether **every element** satisfies a condition.

### Syntax

```java
boolean allMatch(Predicate<? super T> predicate);
```

Example:

```java
List<Integer> numbers = List.of(2, 4, 6, 8);

boolean result = numbers.stream()
        .allMatch(n -> n % 2 == 0);

System.out.println(result);
```

Output:

```text
true
```

---

## Short-Circuiting

If one element fails:

```java
List<Integer> numbers = List.of(2, 4, 7, 8);

boolean result = numbers.stream()
        .allMatch(n -> n % 2 == 0);
```

Processing can stop at:

```text
7 → false
```

Result:

```text
false
```

---

# 21. noneMatch()

`noneMatch()` checks whether **no element** satisfies the condition.

### Syntax

```java
boolean noneMatch(Predicate<? super T> predicate);
```

Example:

```java
List<Integer> numbers = List.of(1, 3, 5, 7);

boolean result = numbers.stream()
        .noneMatch(n -> n % 2 == 0);

System.out.println(result);
```

Output:

```text
true
```

---

## Example with Match

```java
List<Integer> numbers = List.of(1, 3, 4, 7);

boolean result = numbers.stream()
        .noneMatch(n -> n % 2 == 0);

System.out.println(result);
```

Output:

```text
false
```

Because:

```text
4 → even
```

---

# 22. Optional in Terminal Operations

Several Stream terminal operations return `Optional`.

Examples:

```text
reduce()
min()
max()
findFirst()
findAny()
```

Why?

Because a Stream can be empty.

Example:

```java
List<Integer> numbers = List.of();

Optional<Integer> result = numbers.stream()
        .findFirst();

System.out.println(result);
```

Output:

```text
Optional.empty
```

---

## Optional Handling

Use:

```java
orElse()
```

Example:

```java
int value = numbers.stream()
        .findFirst()
        .orElse(0);
```

Use:

```java
orElseThrow()
```

when absence should be treated as an error:

```java
int value = numbers.stream()
        .findFirst()
        .orElseThrow();
```

---

# 23. Terminal Operations and Stream Consumption

A terminal operation consumes the Stream.

Example:

```java
Stream<Integer> stream = Stream.of(1, 2, 3, 4);

long count = stream.count();
```

After:

```java
count()
```

the Stream has been consumed.

Trying:

```java
stream.forEach(System.out::println);
```

causes:

```text
IllegalStateException
```

---

## Correct Approach

Create a new Stream:

```java
List<Integer> numbers = List.of(1, 2, 3, 4);

long count = numbers.stream()
        .count();

numbers.stream()
        .forEach(System.out::println);
```

---

# 24. Short-Circuiting Terminal Operations

Some terminal operations do not necessarily process every element.

Important short-circuiting terminal operations:

```text
findFirst()
findAny()
anyMatch()
allMatch()
noneMatch()
```

---

## anyMatch()

```java
boolean result = Stream.of(1, 2, 3, 4, 5)
        .anyMatch(n -> n > 2);
```

Processing can stop after:

```text
3 → true
```

---

## allMatch()

```java
boolean result = Stream.of(2, 4, 6, 7, 8)
        .allMatch(n -> n % 2 == 0);
```

Processing can stop after:

```text
7 → false
```

---

## noneMatch()

```java
boolean result = Stream.of(1, 3, 5, 8)
        .noneMatch(n -> n % 2 == 0);
```

Processing can stop when:

```text
8 → true
```

for the matching condition, resulting in:

```text
false
```

---

# 25. Mutable Reduction vs Reduction

Two important concepts:

```text
reduce()
collect()
```

---

## reduce()

`reduce()` combines elements into one result.

Example:

```java
int sum = Stream.of(1, 2, 3, 4)
        .reduce(0, Integer::sum);
```

Conceptually:

```text
1 + 2 + 3 + 4
        ↓
       10
```

---

## collect()

`collect()` accumulates elements into a mutable result container.

Example:

```java
List<Integer> result = Stream.of(1, 2, 3, 4)
        .collect(Collectors.toList());
```

Conceptually:

```text
1 → List
2 → List
3 → List
4 → List
```

---

## reduce() vs collect()

| `reduce()` | `collect()` |
|---|---|
| General reduction | Mutable reduction |
| Produces one combined result | Builds result container |
| Good for sum/product/etc. | Good for List/Set/Map/grouping |
| Uses accumulator | Uses Collector |
| Commonly immutable-style combination | Designed for mutable accumulation |

---

# 26. Common Mistakes

## Mistake 1: Thinking Intermediate Operations Execute Automatically

Wrong:

```java
numbers.stream()
        .filter(n -> n > 5);
```

No terminal operation means no actual consumption.

Correct:

```java
numbers.stream()
        .filter(n -> n > 5)
        .toList();
```

---

## Mistake 2: Reusing a Stream

Wrong:

```java
Stream<Integer> stream = numbers.stream();

stream.count();
stream.forEach(System.out::println);
```

Correct:

```java
numbers.stream().count();

numbers.stream().forEach(System.out::println);
```

---

## Mistake 3: Assuming findAny() Means First Element

`findAny()` does not guarantee the first encounter-order element.

Use:

```java
findFirst()
```

when first element semantics matter.

---

## Mistake 4: Forgetting Optional

Wrong:

```java
int result = numbers.stream()
        .findFirst();
```

Correct:

```java
int result = numbers.stream()
        .findFirst()
        .orElse(0);
```

---

## Mistake 5: Assuming toList() Returns Mutable List

This:

```java
List<Integer> result = numbers.stream()
        .toList();
```

returns an unmodifiable List.

If mutability is required:

```java
List<Integer> result = new ArrayList<>(
        numbers.stream().toList()
);
```

---

## Mistake 6: Duplicate Keys in toMap()

This can fail:

```java
students.stream()
        .collect(Collectors.toMap(
                Student::id,
                Student::name
        ));
```

when multiple elements produce the same key.

Use a merge function when duplicates are possible.

---

# 27. Interview Traps

### Trap 1

**What triggers Stream execution?**

A terminal operation.

---

### Trap 2

**Can there be multiple terminal operations in one Stream pipeline?**

No.

A Stream is consumed by a terminal operation.

---

### Trap 3

**Which terminal operations return Optional?**

Common examples:

```text
reduce() without identity
min()
max()
findFirst()
findAny()
```

---

### Trap 4

**Which matching operations are short-circuiting?**

```text
anyMatch()
allMatch()
noneMatch()
```

---

### Trap 5

**Difference between findFirst() and findAny()?**

`findFirst()` respects encounter order when applicable.

`findAny()` may return any element and can provide more flexibility for parallel processing.

---

### Trap 6

**What does count() return?**

```text
long
```

not:

```text
int
```

---

### Trap 7

**Does toList() return a mutable ArrayList?**

No.

`Stream.toList()` returns an unmodifiable List.

---

### Trap 8

**Can reduce() return Optional?**

Yes.

The overload without an identity returns:

```java
Optional<T>
```

because the Stream may be empty.

---

# 28. DSA Connection

Terminal operations are heavily useful in DSA-style problems.

---

## Pattern 1: Count Elements

Problem:

> Count even numbers.

```java
long count = numbers.stream()
        .filter(n -> n % 2 == 0)
        .count();
```

Pattern:

```text
filter → count
```

---

## Pattern 2: Sum

```java
int sum = numbers.stream()
        .reduce(0, Integer::sum);
```

Or for primitive numbers:

```java
int sum = numbers.stream()
        .mapToInt(Integer::intValue)
        .sum();
```

---

## Pattern 3: Minimum

```java
int min = numbers.stream()
        .min(Integer::compareTo)
        .orElseThrow();
```

---

## Pattern 4: Maximum

```java
int max = numbers.stream()
        .max(Integer::compareTo)
        .orElseThrow();
```

---

## Pattern 5: Existence Check

Problem:

> Does the array contain an even number?

```java
boolean exists = numbers.stream()
        .anyMatch(n -> n % 2 == 0);
```

---

## Pattern 6: All Elements Satisfy Condition

```java
boolean allPositive = numbers.stream()
        .allMatch(n -> n > 0);
```

---

## Pattern 7: No Element Satisfies Condition

```java
boolean noNegative = numbers.stream()
        .noneMatch(n -> n < 0);
```

---

## Pattern 8: First Matching Element

```java
Optional<Integer> result = numbers.stream()
        .filter(n -> n > 50)
        .findFirst();
```

---

## Problem-Solving Mindset

Ask:

```text
Need to print/process every element?
        ↓
    forEach()

Need one combined value?
        ↓
    reduce()

Need a collection?
        ↓
    collect() / toList()

Need number of elements?
        ↓
    count()

Need smallest/largest?
        ↓
    min() / max()

Need one element?
        ↓
    findFirst() / findAny()

Need existence check?
        ↓
    anyMatch()

Need every element to satisfy condition?
        ↓
    allMatch()

Need no element to satisfy condition?
        ↓
    noneMatch()
```

---

# 29. Top 10 Interview Questions

## Q1. What is a terminal operation?

A terminal operation consumes the Stream and produces a final result or side effect. It also triggers execution of the Stream pipeline.

---

## Q2. Give examples of terminal operations.

Common examples include:

```text
forEach()
forEachOrdered()
toArray()
reduce()
collect()
toList()
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

## Q3. What happens after a terminal operation?

The Stream is consumed and cannot normally be reused.

---

## Q4. What is reduce()?

`reduce()` combines Stream elements into a single result using an accumulator.

Example:

```java
int sum = Stream.of(1, 2, 3, 4)
        .reduce(0, Integer::sum);
```

---

## Q5. Why does reduce() sometimes return Optional?

The overload without an identity may receive an empty Stream, so there may be no result.

Therefore:

```java
Optional<T>
```

is returned.

---

## Q6. What is the difference between reduce() and collect()?

`reduce()` combines elements into one result.

`collect()` performs a mutable reduction and is commonly used to create collections or other result containers.

---

## Q7. What is the difference between findFirst() and findAny()?

`findFirst()` returns the first element according to encounter order when defined.

`findAny()` can return any element and is more flexible for parallel processing.

---

## Q8. What are short-circuiting terminal operations?

Common examples:

```text
findFirst()
findAny()
anyMatch()
allMatch()
noneMatch()
```

They may stop processing before consuming all elements.

---

## Q9. What does count() return?

`count()` returns a `long`.

```java
long count = stream.count();
```

---

## Q10. What is the difference between forEach() and forEachOrdered()?

`forEach()` does not guarantee encounter order in all parallel contexts.

`forEachOrdered()` respects encounter order when the Stream has a defined encounter order.

---

# 30. 30-Second Interview Answer

> **Terminal operations are the final operations of a Java Stream pipeline. They trigger the execution of intermediate operations and consume the Stream. Common terminal operations include forEach(), reduce(), collect(), count(), min(), max(), findFirst(), findAny(), anyMatch(), allMatch(), and noneMatch(). Some operations return results, while others perform side effects. Several, such as findFirst() and anyMatch(), are short-circuiting.**

---

# 31. Cheat Sheet

| Operation | Purpose | Return Type | Short-Circuiting |
|---|---|---|---|
| `forEach()` | Process every element | `void` | No |
| `forEachOrdered()` | Process in encounter order | `void` | No |
| `toArray()` | Convert to array | `Object[]` / array | No |
| `reduce()` | Combine elements | `T` / `Optional<T>` | No |
| `collect()` | Mutable reduction | `R` | No |
| `toList()` | Create List | `List<T>` | No |
| `count()` | Count elements | `long` | No |
| `min()` | Minimum | `Optional<T>` | No |
| `max()` | Maximum | `Optional<T>` | No |
| `findFirst()` | First element | `Optional<T>` | Yes |
| `findAny()` | Any element | `Optional<T>` | Yes |
| `anyMatch()` | At least one matches | `boolean` | Yes |
| `allMatch()` | Every element matches | `boolean` | Yes |
| `noneMatch()` | No element matches | `boolean` | Yes |

---

# 🧠 Memory Trick

```text
forEach       → DO SOMETHING
forEachOrdered → DO IN ORDER

toArray       → ARRAY

reduce        → COMBINE
collect       → BUILD RESULT

toList        → LIST

count         → HOW MANY?

min           → SMALLEST
max           → LARGEST

findFirst     → FIRST
findAny       → ANY

anyMatch      → AT LEAST ONE?
allMatch      → EVERY ONE?
noneMatch     → NONE?
```

---

# 🔥 Intermediate vs Terminal

| Intermediate | Terminal |
|---|---|
| `filter()` | `forEach()` |
| `map()` | `reduce()` |
| `flatMap()` | `collect()` |
| `distinct()` | `toList()` |
| `sorted()` | `count()` |
| `limit()` | `min()` |
| `skip()` | `max()` |
| `peek()` | `findFirst()` |
| `takeWhile()` | `findAny()` |
| `dropWhile()` | `anyMatch()` |
| `unordered()` | `allMatch()` |
|  | `noneMatch()` |

### Core Difference

```text
Intermediate Operation
        ↓
Returns Stream
        ↓
Builds pipeline
        ↓
Lazy
```

```text
Terminal Operation
        ↓
Consumes Stream
        ↓
Produces result / side effect
        ↓
Triggers execution
```

---

# 🎯 Final Takeaway

The most important Stream concept is:

```text
SOURCE
  ↓
INTERMEDIATE OPERATIONS
  ↓
TERMINAL OPERATION
```

For example:

```java
List<Integer> result = numbers.stream()
        .filter(n -> n % 2 == 0)
        .map(n -> n * 10)
        .sorted()
        .toList();
```

Think:

```text
filter() → SELECT
map()    → TRANSFORM
sorted() → ORDER
toList() → FINISH
```

> 🔥 **Intermediate operations describe the processing pipeline; the terminal operation starts the actual processing and consumes the Stream.**