# 🌊 Java Stream API — Introduction

> **Java Stream API** provides a declarative and functional way to process data from collections, arrays, and other data sources using operations such as filtering, mapping, sorting, and reduction.

---

## 📌 Table of Contents

1. [What is Stream API?](#-what-is-stream-api)
2. [Why Stream API?](#-why-stream-api)
3. [What is a Stream?](#-what-is-a-stream)
4. [Stream API History](#-stream-api-history)
5. [Stream API Package](#-stream-api-package)
6. [Creating a Stream](#-creating-a-stream)
7. [Stream Pipeline](#-stream-pipeline)
8. [Source](#1-source)
9. [Intermediate Operations](#2-intermediate-operations)
10. [Terminal Operation](#3-terminal-operation)
11. [Lazy Evaluation](#-lazy-evaluation)
12. [Streams Do Not Store Data](#-streams-do-not-store-data)
13. [Streams Do Not Modify the Source](#-streams-do-not-modify-the-source)
14. [Streams Can Be Consumed Only Once](#-streams-can-be-consumed-only-once)
15. [Sequential Streams](#-sequential-streams)
16. [Parallel Streams](#-parallel-streams)
17. [Stream vs Collection](#-stream-vs-collection)
18. [Imperative vs Declarative Style](#-imperative-vs-declarative-style)
19. [Internal Working](#-internal-working)
20. [Stateless and Stateful Operations](#-stateless-and-stateful-operations)
21. [Short-Circuiting Operations](#-short-circuiting-operations)
22. [Primitive Streams](#-primitive-streams)
23. [Stream and Functional Programming](#-stream-and-functional-programming)
24. [Advantages](#-advantages)
25. [Disadvantages](#-disadvantages)
26. [Common Mistakes](#-common-mistakes)
27. [Interview Traps](#-interview-traps)
28. [DSA Connection](#-dsa-connection)
29. [Problem-Solving Approach](#-problem-solving-approach)
30. [30-Second Interview Answer](#-30-second-interview-answer)
31. [Cheat Sheet](#-cheat-sheet)
32. [Top 10 Interview Questions](#-top-10-interview-questions)

---

# 🔹 What is Stream API?

The **Stream API** was introduced in **Java 8** to process data from collections and other data sources in a declarative and functional style.

A Stream represents a **sequence of elements** that can be processed through a pipeline of operations.

### Example

```java
List<Integer> numbers = List.of(10, 20, 30, 40, 50);

numbers.stream()
       .filter(n -> n > 25)
       .forEach(System.out::println);
```

Output:

```text
30
40
50
```

A Stream is **not a collection**.

It does not primarily exist to store data.

```text
Collection → stores/manages data
Stream     → processes data
```

---

# 🔹 Why Stream API?

Before Java 8, collection processing commonly required explicit loops.

### Traditional Approach

```java
List<Integer> numbers = List.of(10, 20, 30, 40, 50);

for (Integer number : numbers) {
    if (number > 25) {
        System.out.println(number);
    }
}
```

### Stream Approach

```java
List<Integer> numbers = List.of(10, 20, 30, 40, 50);

numbers.stream()
       .filter(number -> number > 25)
       .forEach(System.out::println);
```

The Stream version expresses the operation as a pipeline:

```text
Take numbers
     ↓
Keep numbers greater than 25
     ↓
Print them
```

### Stream API provides

- Declarative data processing
- Functional-style operations
- Pipeline-based processing
- Lazy evaluation
- Filtering
- Transformation
- Sorting
- Aggregation
- Short-circuiting
- Optional parallel processing

---

# 🔹 What is a Stream?

A Stream is a sequence of elements supporting **sequential and parallel aggregate operations**.

In simple words:

> A Stream is a mechanism for processing elements from a data source through a sequence of operations.

### Example

```java
List<String> names = List.of("Aman", "Rahul", "Yash", "Ravi");

names.stream()
     .filter(name -> name.length() > 4)
     .forEach(System.out::println);
```

Output:

```text
Rahul
```

---

# 🔹 Interview Definition

> A Stream in Java is a sequence of elements obtained from a data source that supports functional-style operations for processing data through a pipeline.

---

# 🔹 Stream API History

The Stream API was introduced in:

```text
Java 8
```

Java 8 introduced several important features:

- Lambda Expressions
- Functional Interfaces
- Stream API
- Method References
- Optional
- Default Interface Methods
- Static Interface Methods

Streams work especially well with lambda expressions.

### Example

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5);

numbers.stream()
       .filter(n -> n % 2 == 0)
       .forEach(System.out::println);
```

Output:

```text
2
4
```

---

# 🔹 Stream API Package

The Stream API is primarily provided by:

```java
java.util.stream
```

Important types include:

```text
Stream<T>
IntStream
LongStream
DoubleStream
BaseStream<T, S>
Collectors
```

The most commonly used interface is:

```java
Stream<T>
```

---

# 🔹 Creating a Stream

There are several ways to create a Stream.

---

## 1. From a Collection

Most collections provide the `stream()` method.

```java
List<Integer> numbers = List.of(10, 20, 30);

Stream<Integer> stream = numbers.stream();
```

The collection remains the data source.

---

## 2. From an Array

### Primitive Array

```java
int[] numbers = {10, 20, 30, 40};

IntStream stream = Arrays.stream(numbers);
```

### Object Array

```java
String[] names = {"Aman", "Rahul", "Ravi"};

Stream<String> stream = Arrays.stream(names);
```

---

## 3. Using `Stream.of()`

```java
Stream<Integer> stream = Stream.of(10, 20, 30, 40);
```

Multiple objects can also be supplied:

```java
Stream<String> names = Stream.of("Aman", "Rahul", "Ravi");
```

---

## 4. Using `Stream.generate()`

`generate()` creates a potentially infinite stream using a `Supplier`.

```java
Stream<Integer> stream = Stream.generate(() -> 10);
```

Because the stream can be infinite, use a limiting operation when required.

```java
Stream.generate(() -> 10)
      .limit(5)
      .forEach(System.out::println);
```

Output:

```text
10
10
10
10
10
```

---

## 5. Using `Stream.iterate()`

`iterate()` creates a stream by repeatedly applying a function.

```java
Stream<Integer> stream = Stream.iterate(1, n -> n + 1);
```

This can produce an infinite stream.

Use `limit()` when a finite sequence is required.

```java
Stream.iterate(1, n -> n + 1)
      .limit(5)
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

# 🔹 Stream Pipeline

A Stream pipeline generally consists of three parts:

```text
Source
   ↓
Intermediate Operations
   ↓
Terminal Operation
```

### Example

```java
List<Integer> numbers = List.of(10, 15, 20, 25, 30);

numbers.stream()
       .filter(n -> n % 2 == 0)
       .map(n -> n * 2)
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
forEach()
```

---

# 1️⃣ Source

The source provides the elements to the stream.

Examples:

```java
List<Integer> numbers = List.of(10, 20, 30);

Stream<Integer> stream = numbers.stream();
```

Another example:

```java
String[] names = {"Aman", "Rahul", "Ravi"};

Stream<String> stream = Arrays.stream(names);
```

Another:

```java
Stream<Integer> stream = Stream.of(10, 20, 30);
```

### Common Sources

```text
Collection
Array
Stream.of()
Stream.generate()
Stream.iterate()
Files.lines()
```

The source is where the stream gets its elements.

---

# 2️⃣ Intermediate Operations

Intermediate operations transform a stream into another stream.

Examples:

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

### Example

```java
List<Integer> numbers = List.of(10, 20, 30, 40);

Stream<Integer> result = numbers.stream()
                                .filter(n -> n > 20);
```

The result is still a Stream.

```text
Stream → Stream
```

Intermediate operations are generally:

- Lazy
- Chainable
- Used for transformation/filtering

---

# 3️⃣ Terminal Operation

A terminal operation ends the Stream pipeline.

Examples:

```text
forEach()
collect()
count()
reduce()
findFirst()
findAny()
anyMatch()
allMatch()
noneMatch()
```

### Example

```java
List<Integer> numbers = List.of(10, 20, 30, 40);

long count = numbers.stream()
                    .filter(n -> n > 20)
                    .count();
```

Here:

```text
stream()
   ↓
filter()
   ↓
count()
```

`count()` is the terminal operation.

---

# 🔹 Intermediate vs Terminal

| Feature | Intermediate | Terminal |
|---|---|---|
| Returns | Stream | Final result / void / Optional / primitive |
| Lazy | Generally yes | No |
| Can chain | Yes | Ends pipeline |
| Examples | `filter()`, `map()` | `forEach()`, `collect()` |
| Executes pipeline | No, by itself | Yes |
| Number in pipeline | Multiple | Usually one final operation |

---

# 🔹 Lazy Evaluation

One of the most important characteristics of Streams is:

> **Intermediate operations are generally lazy.**

Consider:

```java
List<Integer> numbers = List.of(10, 20, 30);

numbers.stream()
       .filter(n -> {
           System.out.println("Filtering " + n);
           return n > 15;
       });
```

Nothing is printed.

Why?

Because there is no terminal operation.

Now add a terminal operation:

```java
numbers.stream()
       .filter(n -> {
           System.out.println("Filtering " + n);
           return n > 15;
       })
       .forEach(System.out::println);
```

Output:

```text
Filtering 10
Filtering 20
20
Filtering 30
30
```

### Key Point

```text
stream()
filter()
map()
sorted()
```

are not necessarily executed immediately.

A terminal operation triggers evaluation of the pipeline.

---

# 🔹 Why Lazy Evaluation?

Lazy evaluation allows Stream processing to avoid unnecessary work in many cases.

Example:

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5);

int result = numbers.stream()
                    .filter(n -> n > 2)
                    .map(n -> n * 10)
                    .findFirst()
                    .orElse(-1);
```

Conceptually:

```text
1 → filter → rejected

2 → filter → rejected

3 → filter → accepted
          ↓
        map
          ↓
         30
          ↓
     findFirst()
          ↓
         STOP
```

The stream does not need to find later matching elements after `findFirst()` has obtained the required result.

---

# 🔹 Streams Do Not Store Data

A Stream is not a data structure for storing elements.

Example:

```java
List<Integer> numbers = List.of(10, 20, 30);

Stream<Integer> stream = numbers.stream();
```

The data remains in:

```text
numbers
```

The Stream provides a way to process that data.

### Remember

```text
Collection → stores/manages data
Stream     → processes data
```

---

# 🔹 Streams Do Not Modify the Source

Stream operations do not inherently modify the source collection.

Example:

```java
List<Integer> numbers = List.of(10, 20, 30, 40);

List<Integer> result = numbers.stream()
                              .filter(n -> n > 20)
                              .toList();
```

Original collection:

```text
[10, 20, 30, 40]
```

Result:

```text
[30, 40]
```

The filtering operation does not remove elements from the original collection.

### Important Nuance

Streams themselves do not inherently mutate the source, but user-provided operations can create side effects.

Example:

```java
List<StringBuilder> names = new ArrayList<>();

names.add(new StringBuilder("A"));
names.add(new StringBuilder("B"));

names.stream()
     .forEach(name -> name.append("X"));
```

Here the referenced `StringBuilder` objects are explicitly mutated.

Therefore, the better interview statement is:

> Stream operations do not inherently modify their source, but user-provided functions can introduce side effects.

---

# 🔹 Streams Can Be Consumed Only Once

A Stream normally cannot be reused after a terminal operation.

Example:

```java
List<Integer> numbers = List.of(10, 20, 30);

Stream<Integer> stream = numbers.stream();

stream.forEach(System.out::println);

stream.forEach(System.out::println);
```

The second operation causes an exception:

```text
java.lang.IllegalStateException:
stream has already been operated upon or closed
```

### Correct Approach

Create another Stream:

```java
Stream<Integer> stream1 = numbers.stream();

stream1.forEach(System.out::println);

Stream<Integer> stream2 = numbers.stream();

stream2.forEach(System.out::println);
```

### Remember

```text
One Stream object
      ↓
One processing lifecycle
```

The source collection itself can still be used to create another stream.

---

# 🔹 Sequential Streams

A Stream created using:

```java
stream()
```

is sequential by default.

Example:

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5);

numbers.stream()
       .forEach(System.out::println);
```

The processing is sequential unless the stream is converted to parallel.

---

# 🔹 Parallel Streams

A parallel Stream can divide stream processing across multiple threads.

Example:

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5);

numbers.parallelStream()
       .forEach(System.out::println);
```

The output order should not be assumed:

```text
3
1
5
2
4
```

or another order may occur.

Parallel Streams are covered in detail in:

```text
09-Parallel-Streams.md
```

---

# 🔹 Stream vs Collection

This is one of the most important Stream API interview questions.

| Collection | Stream |
|---|---|
| Stores/manages data | Processes data |
| Data structure | Processing abstraction |
| Usually reusable | Normally one-time use |
| Can add/remove elements depending on type | Does not provide collection-style mutation |
| Eager data structure | Supports lazy processing |
| Focuses on data management | Focuses on data processing |
| Examples: `List`, `Set` | Example: `Stream<T>` |

### Simple Memory Trick

```text
Collection = WHAT data do I have?
Stream     = WHAT do I want to do with that data?
```

---

# 🔹 Imperative vs Declarative Style

## Imperative Programming

In imperative programming, we explicitly describe **how** the operation should happen.

Example:

```java
List<Integer> numbers = List.of(10, 20, 30, 40, 50);

for (Integer number : numbers) {
    if (number > 25) {
        System.out.println(number);
    }
}
```

We manually control:

- Iteration
- Condition checking
- Execution

---

## Declarative Programming

In declarative programming, we describe **what** should happen.

```java
List<Integer> numbers = List.of(10, 20, 30, 40, 50);

numbers.stream()
       .filter(number -> number > 25)
       .forEach(System.out::println);
```

We describe:

```text
Keep numbers greater than 25
Then print them
```

The Stream API manages the iteration mechanism.

---

# 🔹 Internal Working

Consider:

```java
List<Integer> numbers = List.of(10, 20, 30, 40);

numbers.stream()
       .filter(n -> n > 20)
       .map(n -> n * 2)
       .forEach(System.out::println);
```

Conceptually:

```text
                STREAM PIPELINE

       ┌──────────────────────┐
       │      Data Source     │
       │  List: 10 20 30 40   │
       └──────────┬───────────┘
                  ↓
       ┌──────────────────────┐
       │    stream()          │
       └──────────┬───────────┘
                  ↓
       ┌──────────────────────┐
       │      filter()        │
       │       n > 20         │
       └──────────┬───────────┘
                  ↓
       ┌──────────────────────┐
       │       map()          │
       │       n * 2          │
       └──────────┬───────────┘
                  ↓
       ┌──────────────────────┐
       │     forEach()        │
       │   Terminal Operation │
       └──────────────────────┘
```

A high-level execution model is:

```text
1. Create a stream from the source
2. Build intermediate pipeline stages
3. Wait for a terminal operation
4. Terminal operation triggers traversal
5. Elements flow through the pipeline
6. The final operation produces the result/side effect
```

---

# 🔹 Pipeline Fusion

A Stream pipeline is not necessarily implemented as:

```text
filter every element
       ↓
create collection
       ↓
map every element
       ↓
create collection
       ↓
forEach
```

Instead, stream implementations can process elements through a fused pipeline.

Conceptually:

```text
Element
   ↓
filter
   ↓
map
   ↓
terminal operation
```

For example:

```java
numbers.stream()
       .filter(n -> n > 20)
       .map(n -> n * 2)
       .forEach(System.out::println);
```

An element can conceptually pass through:

```text
10 → filter → rejected

20 → filter → rejected

30 → filter → map → 60 → forEach

40 → filter → map → 80 → forEach
```

This model is useful for understanding why Streams can perform lazy and short-circuiting operations efficiently.

---

# 🔹 Stateless and Stateful Operations

Stream operations can also be categorized as **stateless** or **stateful**.

---

## Stateless Operations

A stateless operation generally processes each element independently without needing information about other elements.

Examples:

```text
filter()
map()
mapToInt()
peek()
```

Example:

```java
numbers.stream()
       .filter(n -> n > 10)
       .map(n -> n * 2)
       .forEach(System.out::println);
```

Each element can be processed without needing to know the other elements.

---

## Stateful Operations

A stateful operation may need information about multiple elements.

Examples:

```text
sorted()
distinct()
```

Example:

```java
List<Integer> numbers = List.of(30, 10, 20, 10);

numbers.stream()
       .sorted()
       .forEach(System.out::println);
```

Output:

```text
10
10
20
30
```

`sorted()` needs information about the elements to establish their order.

---

# 🔹 Short-Circuiting Operations

Some Stream operations can stop processing once the required result has been determined.

Examples include:

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
List<Integer> numbers = List.of(10, 20, 30, 40, 50);

boolean result = numbers.stream()
                        .anyMatch(n -> n > 25);
```

Once `30` satisfies the condition, the answer is already known:

```text
true
```

There is no need to inspect later elements for the purpose of `anyMatch()`.

---

# 🔹 Short-Circuiting Example

```java
List<Integer> numbers = List.of(10, 20, 30, 40, 50);

boolean result = numbers.stream()
                        .peek(n -> System.out.println("Checking " + n))
                        .anyMatch(n -> n > 25);

System.out.println(result);
```

Possible output:

```text
Checking 10
Checking 20
Checking 30
true
```

Processing can stop after `30`.

### Important

`peek()` is mainly useful for debugging and inspection.

It should not normally be used to perform essential business side effects.

---

# 🔹 Primitive Streams

Java provides specialized Streams for primitive numeric types.

```text
IntStream
LongStream
DoubleStream
```

These are available in:

```java
java.util.stream
```

---

## IntStream

```java
IntStream.range(1, 6)
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

## LongStream

```java
LongStream.range(1, 6)
          .forEach(System.out::println);
```

---

## DoubleStream

```java
DoubleStream.of(10.5, 20.5, 30.5)
            .forEach(System.out::println);
```

---

# 🔹 Why Primitive Streams?

Suppose we have:

```java
List<Integer> numbers;
```

The elements are wrapper objects:

```text
Integer
```

Primitive streams such as `IntStream` work directly with primitive `int` values.

This can avoid unnecessary boxing and unboxing in numeric processing.

Example:

```java
int sum = IntStream.of(10, 20, 30, 40)
                   .sum();
```

Output:

```text
100
```

---

# 🔹 Stream and Functional Programming

Streams are closely associated with functional programming concepts.

Important concepts include:

- Lambda Expressions
- Functional Interfaces
- Method References
- Higher-order-style operations
- Immutability-oriented processing
- Function composition

Example:

```java
numbers.stream()
       .filter(n -> n > 10)
       .map(n -> n * 2)
       .forEach(System.out::println);
```

Here:

```text
n -> n > 10
```

is a lambda expression.

```text
n -> n * 2
```

is a lambda expression.

```text
System.out::println
```

is a method reference.

---

# 🔹 Common Functional Interfaces Used with Streams

## Predicate<T>

Used commonly by `filter()`.

```java
Predicate<Integer> condition = n -> n > 10;
```

---

## Function<T, R>

Used commonly by `map()`.

```java
Function<Integer, Integer> square = n -> n * n;
```

---

## Consumer<T>

Used commonly by `forEach()`.

```java
Consumer<Integer> printer = n -> System.out.println(n);
```

---

## Supplier<T>

Used by operations such as `Stream.generate()`.

```java
Supplier<Integer> supplier = () -> 100;
```

These functional interfaces are important for understanding how Stream operations accept behavior.

---

# 🔹 Common Stream Methods

| Method | Category | Purpose |
|---|---|---|
| `filter()` | Intermediate | Select elements |
| `map()` | Intermediate | Transform elements |
| `flatMap()` | Intermediate | Flatten nested streams |
| `sorted()` | Intermediate | Sort elements |
| `distinct()` | Intermediate | Remove duplicates |
| `limit()` | Intermediate | Limit elements |
| `skip()` | Intermediate | Skip elements |
| `peek()` | Intermediate | Inspect elements |
| `forEach()` | Terminal | Perform action |
| `collect()` | Terminal | Collect result |
| `toList()` | Terminal | Create a List result |
| `count()` | Terminal | Count elements |
| `reduce()` | Terminal | Combine elements |
| `findFirst()` | Terminal | Find first element |
| `findAny()` | Terminal | Find any element |
| `anyMatch()` | Terminal | Check whether any element matches |
| `allMatch()` | Terminal | Check whether all elements match |
| `noneMatch()` | Terminal | Check whether no elements match |

Detailed notes for these operations are covered in the later Stream API files.

---

# 🔹 Complete Stream Example

```java
List<String> names = List.of(
        "Aman",
        "Rahul",
        "Alexander",
        "Ravi",
        "Ankit"
);

names.stream()
     .filter(name -> name.length() > 4)
     .map(String::toUpperCase)
     .sorted()
     .forEach(System.out::println);
```

Pipeline:

```text
names
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

Possible output:

```text
ALEXANDER
ANKIT
RAHUL
```

---

# 🔹 Another Example: Employee Salaries

```java
List<Integer> salaries = List.of(
        25000,
        40000,
        55000,
        30000,
        70000
);

salaries.stream()
        .filter(salary -> salary > 40000)
        .forEach(System.out::println);
```

Output:

```text
55000
70000
```

Pipeline:

```text
salaries
    ↓
stream()
    ↓
filter(salary > 40000)
    ↓
forEach()
```

---

# 🔹 Multiple Intermediate Operations

Streams can contain multiple intermediate operations.

```java
List<Integer> numbers = List.of(10, 15, 20, 25, 30, 35);

numbers.stream()
       .filter(n -> n % 2 == 0)
       .map(n -> n * 2)
       .filter(n -> n > 30)
       .forEach(System.out::println);
```

Processing conceptually:

```text
10 → even → 20 → rejected

15 → rejected

20 → even → 40 → accepted

25 → rejected

30 → even → 60 → accepted

35 → rejected
```

Output:

```text
40
60
```

---

# 🔹 Advantages

## 1. Cleaner Code

Streams can reduce boilerplate iteration code.

---

## 2. Declarative Style

You describe what should happen instead of manually controlling every iteration.

---

## 3. Easy Transformation

Operations such as:

```text
map()
filter()
flatMap()
```

make transformations expressive.

---

## 4. Pipeline Composition

Multiple operations can be chained.

```java
numbers.stream()
       .filter(...)
       .map(...)
       .sorted()
       .collect(...);
```

---

## 5. Lazy Evaluation

Intermediate operations can be delayed until a terminal operation is required.

---

## 6. Short-Circuiting

Some operations can stop processing once the answer is known.

Examples:

```text
findFirst()
findAny()
anyMatch()
```

---

## 7. Parallel Processing Support

Java provides:

```java
parallelStream()
```

and:

```java
stream.parallel()
```

for parallel stream processing.

---

# 🔹 Disadvantages

## 1. Can Become Hard to Read

Very long pipelines can reduce readability.

Example:

```java
numbers.stream()
       .filter(...)
       .map(...)
       .flatMap(...)
       .sorted(...)
       .distinct()
       .limit(...)
       .collect(...);
```

Streams should be used when they improve clarity.

---

## 2. Debugging Can Be Less Straightforward

A long pipeline may be harder to debug than a traditional loop.

---

## 3. Not Automatically Faster

Streams do not automatically make code faster.

A traditional loop can be more appropriate in some situations.

---

## 4. Parallel Streams Can Be Misused

Parallel processing introduces overhead and is not beneficial for every workload.

---

## 5. Side Effects Can Cause Problems

Unnecessary mutation inside stream operations can make code harder to reason about.

Avoid patterns like:

```java
List<Integer> result = new ArrayList<>();

numbers.stream()
       .filter(n -> n > 10)
       .forEach(result::add);
```

Prefer an appropriate terminal operation:

```java
List<Integer> result = numbers.stream()
                              .filter(n -> n > 10)
                              .toList();
```

---

# 🔹 Common Mistakes

## ❌ Mistake 1: Thinking Stream Stores Data

Wrong:

```text
Stream = Collection
```

Correct:

```text
Collection → stores/manages data
Stream     → processes data
```

---

## ❌ Mistake 2: Forgetting the Terminal Operation

This pipeline has no terminal operation:

```java
numbers.stream()
       .filter(n -> n > 10)
       .map(n -> n * 2);
```

The intermediate operations are not triggered simply because they were written.

Correct:

```java
numbers.stream()
       .filter(n -> n > 10)
       .map(n -> n * 2)
       .forEach(System.out::println);
```

---

## ❌ Mistake 3: Reusing a Stream

Wrong:

```java
Stream<Integer> stream = numbers.stream();

stream.count();

stream.forEach(System.out::println);
```

A Stream cannot normally be reused after a terminal operation.

---

## ❌ Mistake 4: Assuming `stream()` Means Parallel

Wrong:

```text
stream() = parallel processing
```

Correct:

```text
stream()         → sequential
parallelStream() → parallel
```

---

## ❌ Mistake 5: Assuming Streams Always Improve Performance

Streams mainly provide a powerful processing abstraction and cleaner functional-style code.

Performance depends on:

- Dataset size
- Operation complexity
- Pipeline structure
- Source characteristics
- Sequential vs parallel processing
- Boxing/unboxing
- JVM optimizations

---

## ❌ Mistake 6: Excessive Side Effects

Avoid unnecessary external state mutation inside Stream operations.

Prefer:

```java
List<Integer> result = numbers.stream()
                              .filter(n -> n > 10)
                              .toList();
```

over unnecessary manual accumulation.

---

# 🔥 Interview Traps

## Trap 1

### Does Stream store data?

**Answer:**

> No. A Stream does not store elements. It provides a mechanism for processing elements from a data source.

---

## Trap 2

### Can a Stream be reused?

**Answer:**

> No. A Stream is normally consumed after a terminal operation and cannot be reused. A new Stream can be created from the original source.

---

## Trap 3

### Are intermediate operations executed immediately?

**Answer:**

> No. Intermediate operations are generally lazy and are evaluated when a terminal operation triggers the pipeline.

---

## Trap 4

### Does Stream API modify the collection?

**Answer:**

> Stream operations do not inherently modify the source collection. However, user-provided functions can introduce side effects or mutate referenced objects.

---

## Trap 5

### Is `stream()` parallel?

**Answer:**

> No. `stream()` creates a sequential Stream by default. `parallelStream()` creates a parallel Stream.

---

## Trap 6

### Does Stream API replace Collections?

**Answer:**

> No. Collections and Streams solve different problems. Collections primarily store and manage data, while Streams process data.

---

## Trap 7

### What triggers Stream execution?

**Answer:**

> A terminal operation triggers the evaluation of the Stream pipeline.

---

## Trap 8

### Can a Stream have multiple terminal operations?

**Answer:**

> No. A single Stream pipeline ends with one terminal operation. After that, the Stream is consumed.

---

## Trap 9

### Can a Stream contain only one intermediate operation?

**Answer:**

> No. A pipeline can contain multiple intermediate operations.

Example:

```java
numbers.stream()
       .filter(...)
       .map(...)
       .sorted()
       .distinct()
       .limit(...);
```

---

## Trap 10

### Does Stream API always improve performance?

**Answer:**

> No. Stream API improves expressiveness and provides powerful processing abstractions, but performance depends on the particular workload and implementation.

---

# 🔹 DSA Connection

Streams are useful for many common DSA and data-processing patterns.

---

## Pattern 1 — Filtering

Find all even numbers:

```java
List<Integer> evenNumbers = numbers.stream()
                                   .filter(n -> n % 2 == 0)
                                   .toList();
```

DSA idea:

```text
Select elements satisfying a condition
```

---

## Pattern 2 — Transformation

Find squares:

```java
List<Integer> squares = numbers.stream()
                               .map(n -> n * n)
                               .toList();
```

DSA idea:

```text
Transform every element
```

---

## Pattern 3 — Aggregation

Find the sum:

```java
int sum = numbers.stream()
                 .mapToInt(Integer::intValue)
                 .sum();
```

DSA idea:

```text
Running aggregation
```

---

## Pattern 4 — Searching

Check whether an element exists:

```java
boolean exists = numbers.stream()
                        .anyMatch(n -> n == 50);
```

DSA idea:

```text
Search / existence check
```

---

## Pattern 5 — Counting

Count values greater than 20:

```java
long count = numbers.stream()
                    .filter(n -> n > 20)
                    .count();
```

DSA idea:

```text
Count elements satisfying a condition
```

---

## Pattern 6 — Finding Maximum

```java
Optional<Integer> maximum = numbers.stream()
                                   .max(Integer::compareTo);
```

---

## Pattern 7 — Finding Minimum

```java
Optional<Integer> minimum = numbers.stream()
                                   .min(Integer::compareTo);
```

---

## Pattern 8 — Removing Duplicates

```java
List<Integer> uniqueNumbers = numbers.stream()
                                     .distinct()
                                     .toList();
```

---

# 🔹 Problem-Solving Approach

When solving a data-processing problem using Streams, ask:

```text
1. What is my data source?
          ↓
2. Do I need to filter?
          ↓
3. Do I need to transform?
          ↓
4. Do I need to remove duplicates?
          ↓
5. Do I need to sort?
          ↓
6. Do I need to limit or skip?
          ↓
7. Do I need aggregation/search?
          ↓
8. What should the final result be?
```

---

## Example Problem

> Find the squares of all even numbers.

Think:

```text
Source
  ↓
Filter even numbers
  ↓
Map to squares
  ↓
Collect result
```

Java:

```java
List<Integer> squaresOfEvenNumbers = numbers.stream()
                                            .filter(n -> n % 2 == 0)
                                            .map(n -> n * n)
                                            .toList();
```

---

# 🔹 Important Stream Thinking Pattern

Most basic Stream problems can initially be decomposed into:

```text
FILTER
   ↓
TRANSFORM
   ↓
SORT
   ↓
AGGREGATE
   ↓
COLLECT
```

Not every problem requires every step.

For example:

```java
List<Integer> result = numbers.stream()
                              .filter(n -> n % 2 == 0)
                              .map(n -> n * n)
                              .toList();
```

Here:

```text
FILTER → even numbers
MAP    → square
COLLECT → List
```

---

# 🔹 30-Second Interview Answer

> **Java Stream API was introduced in Java 8 and provides a declarative and functional way to process data from collections and other sources. A Stream does not store data; instead, it creates a processing pipeline consisting of a source, intermediate operations, and a terminal operation. Intermediate operations such as `filter()` and `map()` are generally lazy, while terminal operations such as `collect()`, `forEach()`, and `reduce()` trigger execution. Streams are sequential by default but also support parallel processing.**

---

# 🔹 Cheat Sheet

```text
                         STREAM API
                              │
                ┌─────────────┴─────────────┐
                │                           │
             Java 8                  java.util.stream
                │
                ↓
        Sequence of Elements
                │
                ↓
           Data Processing
                │
                ↓
        ┌───────┴────────┐
        │ Stream Pipeline │
        └───────┬────────┘
                │
        ┌───────┼────────┐
        ↓       ↓        ↓
      Source  Intermediate Terminal
                │          │
                │          │
                ↓          ↓
             filter()   forEach()
             map()      collect()
             sorted()   count()
             distinct() reduce()
             flatMap()  findFirst()
             limit()    findAny()
             skip()     anyMatch()
             peek()     allMatch()
                        noneMatch()
```

---

# 🧠 Stream Memory Trick

Remember:

```text
S → I → T
```

### S = Source

Where does the data come from?

```text
List
Set
Array
Stream.of()
```

### I = Intermediate

What processing should happen?

```text
filter()
map()
sorted()
distinct()
flatMap()
```

### T = Terminal

What final result do I need?

```text
collect()
forEach()
count()
reduce()
findFirst()
```

So:

```text
SOURCE
   ↓
INTERMEDIATE
   ↓
TERMINAL
```

---

# 🔥 Stream API Golden Rules

```text
1. Stream was introduced in Java 8.

2. Stream does not store data.

3. Stream processes data from a source.

4. Intermediate operations are generally lazy.

5. Terminal operation triggers pipeline execution.

6. A Stream normally cannot be reused after termination.

7. stream() creates a sequential Stream by default.

8. parallelStream() creates a parallel Stream.

9. Stream operations do not inherently modify the source.

10. Stream pipelines can contain multiple intermediate operations.

11. A pipeline normally ends with one terminal operation.

12. Streams support declarative and functional-style processing.

13. Streams are not a replacement for Collections.

14. Lazy operations can enable efficient processing.

15. Short-circuiting operations can stop processing early.
```

---

# 🔹 Key Terms to Remember

| Term | Meaning |
|---|---|
| Stream | Processing abstraction over a data source |
| Source | Origin of stream elements |
| Pipeline | Connected sequence of stream operations |
| Intermediate | Operation that returns another Stream |
| Terminal | Operation that ends the pipeline |
| Lazy | Execution delayed until required |
| Sequential | Normal single-thread-oriented stream processing |
| Parallel | Stream processing that can use multiple threads |
| Stateless | Operation generally independent for each element |
| Stateful | Operation may need information about multiple elements |
| Short-circuiting | Can finish without processing every element |
| Primitive Stream | Specialized stream such as `IntStream` |

---

# 🔹 Quick Revision

### What is Stream?

```text
A mechanism for processing a sequence of elements.
```

### Introduced?

```text
Java 8
```

### Package?

```text
java.util.stream
```

### Main interface?

```text
Stream<T>
```

### Does it store data?

```text
No
```

### Does it replace Collections?

```text
No
```

### Intermediate operations?

```text
filter()
map()
sorted()
distinct()
flatMap()
limit()
skip()
```

### Terminal operations?

```text
forEach()
collect()
count()
reduce()
findFirst()
findAny()
```

### Lazy?

```text
Intermediate operations → generally lazy
```

### Trigger?

```text
Terminal operation
```

### Reusable?

```text
Normally no
```

### Default mode?

```text
Sequential
```

### Parallel support?

```text
Yes
```

---

# 🔥 Top 10 Interview Questions

## 1. What is Java Stream API?

### Answer

> Java Stream API is a Java 8 feature that provides a functional and declarative way to process sequences of data from sources such as collections and arrays through pipelines of operations.

---

## 2. What is the difference between Collection and Stream?

### Answer

> A Collection is primarily used to store and manage data, while a Stream is used to process data from a source.

---

## 3. What are the three main parts of a Stream pipeline?

### Answer

```text
Source
   ↓
Intermediate Operations
   ↓
Terminal Operation
```

---

## 4. What is lazy evaluation in Streams?

### Answer

> Lazy evaluation means intermediate operations are generally not executed immediately. Their execution is triggered when a terminal operation is invoked.

---

## 5. Can a Stream be reused?

### Answer

> No. A Stream is normally consumed after a terminal operation. To process the source again, create a new Stream.

---

## 6. Does a Stream store data?

### Answer

> No. A Stream does not store data. It provides a mechanism for processing elements from a data source.

---

## 7. Does `stream()` create a parallel Stream?

### Answer

> No. `stream()` creates a sequential Stream by default. `parallelStream()` creates a parallel Stream.

---

## 8. What is an intermediate operation?

### Answer

> An intermediate operation transforms a Stream into another Stream and is generally lazy.

Examples:

```text
filter()
map()
sorted()
distinct()
flatMap()
```

---

## 9. What is a terminal operation?

### Answer

> A terminal operation ends the Stream pipeline and triggers its evaluation.

Examples:

```text
forEach()
collect()
count()
reduce()
findFirst()
```

---

## 10. Can Stream operations modify the source collection?

### Answer

> Stream operations do not inherently modify the source collection, but user-provided functions can introduce side effects or mutate referenced objects.

---

# 🎯 Final Mental Model

```text
                 DATA
                  │
                  ↓
              DATA SOURCE
                  │
                  ↓
             stream()
                  │
                  ↓
       ┌─────────────────────┐
       │  INTERMEDIATE OPS   │
       │                     │
       │ filter()            │
       │ map()               │
       │ sorted()            │
       │ distinct()          │
       │ flatMap()           │
       └──────────┬──────────┘
                  │
                  ↓
          TERMINAL OPERATION
                  │
                  ↓
               RESULT
```

> 🧠 **Remember:**
>
> **Collection stores the data.**
>
> **Stream processes the data.**
>
> **Intermediate operations build the pipeline.**
>
> **Terminal operation triggers the pipeline.**
>
> **S → I → T = Source → Intermediate → Terminal**