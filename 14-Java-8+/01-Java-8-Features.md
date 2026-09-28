
# 🚀 Java 8 Features

> **Java 8 introduced major language and library improvements, especially Lambda Expressions, Functional Interfaces, Method References, Stream API, Optional, and the modern Date/Time API.**

---

# 📑 Table of Contents

- [1. Why Java 8 Was Important](#1-why-java-8-was-important)
- [2. Major Java 8 Features](#2-major-java-8-features)
- [3. Lambda Expressions](#3-lambda-expressions)
- [4. Functional Interfaces](#4-functional-interfaces)
- [5. Built-in Functional Interfaces](#5-built-in-functional-interfaces)
- [6. Method References](#6-method-references)
- [7. Stream API](#7-stream-api)
- [8. Stream Pipeline](#8-stream-pipeline)
- [9. Intermediate vs Terminal Operations](#9-intermediate-vs-terminal-operations)
- [10. forEach()](#10-foreach)
- [11. filter()](#11-filter)
- [12. map()](#12-map)
- [13. reduce()](#13-reduce)
- [14. sorted()](#14-sorted)
- [15. collect()](#15-collect)
- [16. Optional](#16-optional)
- [17. Interface Default Methods](#17-interface-default-methods)
- [18. Interface Static Methods](#18-interface-static-methods)
- [19. Java 8 Date and Time API](#19-java-8-date-and-time-api)
- [20. CompletableFuture](#20-completablefuture)
- [21. Java 8 vs Before Java 8](#21-java-8-vs-before-java-8)
- [22. Common Mistakes](#22-common-mistakes)
- [23. Interview Questions](#23-interview-questions)
- [24. 30-Second Interview Answer](#24-30-second-interview-answer)
- [25. Cheat Sheet](#25-cheat-sheet)
- [26. Final Mental Model](#26-final-mental-model)

---

# 1. Why Java 8 Was Important

Before Java 8, Java code often relied heavily on:

```text
Anonymous classes
External iteration
Verbose collection processing
Old Date/Calendar APIs
Interfaces without implementation methods
```

Java 8 introduced a more functional programming style.

The biggest shift was:

```text
Java before 8
      ↓
Object-oriented + imperative style

Java 8
      ↓
Object-oriented
      +
Functional programming features
```

Java 8 did **not** turn Java into a purely functional language.

It added functional programming capabilities while keeping Java primarily object-oriented.

---

# 2. Major Java 8 Features

The most important Java 8 features are:

| Feature | Purpose |
|---|---|
| Lambda Expressions | Write behavior/function concisely |
| Functional Interfaces | Represent one abstract operation |
| Method References | Shorter lambda syntax |
| Stream API | Process collections/data declaratively |
| Optional | Represent possible absence of a value |
| Default Methods | Add implementation to interfaces |
| Static Interface Methods | Utility methods inside interfaces |
| New Date/Time API | Better date/time handling |
| CompletableFuture | Asynchronous programming |
| Nashorn | JavaScript engine introduced in Java 8 |

> **Note:** Nashorn was later removed from the JDK. It is therefore a historical Java 8 feature rather than a modern Java recommendation.

---

# 3. Lambda Expressions

A lambda expression is a concise way to represent behavior that can be passed around as a value.

Basic syntax:

```java
(parameters) -> expression
```

or:

```java
(parameters) -> {
    // statements
}
```

---

## Example

Traditional anonymous class:

```java
Runnable task = new Runnable() {

    @Override
    public void run() {
        System.out.println("Running");
    }
};
```

Using lambda:

```java
Runnable task =
    () -> System.out.println("Running");
```

The lambda removes a lot of boilerplate.

---

## Lambda With Parameters

```java
(a, b) -> a + b
```

Example:

```java
interface Calculator {

    int add(int a, int b);
}
```

```java
Calculator calculator =
    (a, b) -> a + b;

System.out.println(
    calculator.add(10, 20)
);
```

Output:

```text
30
```

---

## Lambda With One Parameter

Parentheses can usually be omitted for a single implicitly typed parameter.

```java
x -> x * x
```

Equivalent form:

```java
(x) -> x * x
```

---

## Lambda With Multiple Statements

```java
(a, b) -> {
    int sum = a + b;
    return sum;
}
```

When using a block body with a return value, `return` is required.

---

# 4. Functional Interfaces

A functional interface is an interface containing **exactly one abstract method**.

It may contain:

```text
One abstract method
+
Multiple default methods
+
Multiple static methods
+
Object methods
```

Example:

```java
@FunctionalInterface
interface Calculator {

    int calculate(int a, int b);
}
```

Now a lambda can provide the implementation:

```java
Calculator addition =
    (a, b) -> a + b;
```

---

## @FunctionalInterface

`@FunctionalInterface` tells the compiler that the interface is intended to be functional.

Example:

```java
@FunctionalInterface
interface Greeting {

    void sayHello();
}
```

If we add another abstract method:

```java
@FunctionalInterface
interface Greeting {

    void sayHello();

    void sayBye();
}
```

the compiler reports an error.

---

## Important Interview Point

A functional interface can have multiple methods if only **one is abstract**.

Example:

```java
@FunctionalInterface
interface Calculator {

    int calculate(int a, int b);

    default void print() {
        System.out.println("Calculator");
    }

    static void info() {
        System.out.println("Utility");
    }
}
```

This is still a functional interface.

---

# 5. Built-in Functional Interfaces

Java 8 introduced many functional interfaces in:

```text
java.util.function
```

The most important ones are:

```text
Predicate
Function
Consumer
Supplier
UnaryOperator
BinaryOperator
```

---

## Predicate

Represents a condition.

```text
Input
 ↓
Predicate
 ↓
boolean
```

Example:

```java
Predicate<Integer> isEven =
    n -> n % 2 == 0;

System.out.println(
    isEven.test(10)
);
```

Output:

```text
true
```

Main method:

```java
test()
```

---

## Function

Takes one input and produces one output.

```text
T
 ↓
Function
 ↓
R
```

Example:

```java
Function<String, Integer> length =
    str -> str.length();

System.out.println(
    length.apply("Java")
);
```

Output:

```text
4
```

Main method:

```java
apply()
```

---

## Consumer

Takes input and returns nothing.

```text
T
 ↓
Consumer
 ↓
void
```

Example:

```java
Consumer<String> printer =
    str -> System.out.println(str);

printer.accept("Java");
```

Main method:

```java
accept()
```

---

## Supplier

Takes no input and produces a value.

```text
No input
   ↓
Supplier
   ↓
T
```

Example:

```java
Supplier<Double> randomValue =
    () -> Math.random();

System.out.println(
    randomValue.get()
);
```

Main method:

```java
get()
```

---

## UnaryOperator

Takes one value and returns the same type.

```text
T
 ↓
UnaryOperator
 ↓
T
```

Example:

```java
UnaryOperator<Integer> square =
    n -> n * n;

System.out.println(
    square.apply(5)
);
```

---

## BinaryOperator

Takes two values of the same type and returns the same type.

```text
T + T
 ↓
BinaryOperator
 ↓
T
```

Example:

```java
BinaryOperator<Integer> add =
    (a, b) -> a + b;

System.out.println(
    add.apply(10, 20)
);
```

---

# 6. Method References

Method references provide a shorter syntax for certain lambdas.

Syntax:

```text
ClassName::methodName
```

---

## Example

Lambda:

```java
Consumer<String> printer =
    str -> System.out.println(str);
```

Method reference:

```java
Consumer<String> printer =
    System.out::println;
```

---

## Four Main Forms

### 1. Static Method

```text
ClassName::staticMethod
```

Example:

```java
Function<Integer, Integer> square =
    MathUtils::square;
```

---

### 2. Instance Method of a Particular Object

```text
object::instanceMethod
```

Example:

```java
String prefix = "Hello ";

Function<String, String> greet =
    prefix::concat;
```

---

### 3. Instance Method of an Arbitrary Object of a Type

```text
ClassName::instanceMethod
```

Example:

```java
Function<String, Integer> length =
    String::length;
```

---

### 4. Constructor Reference

```text
ClassName::new
```

Example:

```java
Supplier<ArrayList<String>> listCreator =
    ArrayList::new;
```

---

# 7. Stream API

The Stream API was introduced in Java 8.

It provides a declarative way to process sequences of data.

Package:

```text
java.util.stream
```

Example:

```java
List<Integer> numbers =
    Arrays.asList(1, 2, 3, 4, 5, 6);

numbers.stream()
    .filter(n -> n % 2 == 0)
    .forEach(System.out::println);
```

Output:

```text
2
4
6
```

---

## Stream Is Not a Data Structure

This is a very important interview point.

A Stream:

```text
does not store data
```

Instead, it processes data from a source.

Source examples:

```text
Collection
Array
I/O channel
Generator
```

---

# 8. Stream Pipeline

A stream pipeline generally contains:

```text
Source
  ↓
Intermediate Operations
  ↓
Terminal Operation
```

Example:

```java
numbers.stream()
    .filter(n -> n % 2 == 0)
    .map(n -> n * 10)
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

## Source

```java
numbers.stream()
```

---

## Intermediate Operations

Examples:

```text
filter()
map()
sorted()
distinct()
limit()
skip()
```

---

## Terminal Operations

Examples:

```text
forEach()
collect()
reduce()
count()
min()
max()
anyMatch()
allMatch()
```

---

# 9. Intermediate vs Terminal Operations

## Intermediate Operation

Returns another stream.

Examples:

```text
filter()
map()
sorted()
distinct()
limit()
skip()
```

They are generally **lazy**.

---

## Terminal Operation

Produces a final result or side effect.

Examples:

```text
forEach()
collect()
reduce()
count()
```

After a terminal operation, the stream is considered consumed.

---

## Example

```java
List<Integer> numbers =
    Arrays.asList(1, 2, 3, 4);

numbers.stream()
    .filter(n -> n > 2)
    .map(n -> n * 10)
    .forEach(System.out::println);
```

Here:

```text
filter()
→ intermediate

map()
→ intermediate

forEach()
→ terminal
```

---

# 10. forEach()

`forEach()` is a terminal operation used to perform an action for each element.

Example:

```java
List<String> names =
    Arrays.asList(
        "Amit",
        "Rahul",
        "Priya"
    );

names.stream()
    .forEach(System.out::println);
```

---

## Lambda Version

```java
names.stream()
    .forEach(name ->
        System.out.println(name)
    );
```

---

## Method Reference

```java
names.stream()
    .forEach(System.out::println);
```

---

# 11. filter()

`filter()` selects elements satisfying a condition.

Example:

```java
List<Integer> numbers =
    Arrays.asList(10, 15, 20, 25, 30);

numbers.stream()
    .filter(n -> n % 2 == 0)
    .forEach(System.out::println);
```

Output:

```text
10
20
30
```

---

## Mental Model

```text
Input
 ↓
filter(condition)
 ↓
Only matching elements
```

---

# 12. map()

`map()` transforms each element.

Example:

```java
List<Integer> numbers =
    Arrays.asList(1, 2, 3, 4);

numbers.stream()
    .map(n -> n * 10)
    .forEach(System.out::println);
```

Output:

```text
10
20
30
40
```

---

## filter vs map

```text
filter()
→ selects

map()
→ transforms
```

Example:

```text
[1, 2, 3, 4]
      ↓
filter(even)
      ↓
[2, 4]
```

Whereas:

```text
[1, 2, 3, 4]
      ↓
map(x * 10)
      ↓
[10, 20, 30, 40]
```

---

# 13. reduce()

`reduce()` combines stream elements into a single result.

Example:

```java
List<Integer> numbers =
    Arrays.asList(1, 2, 3, 4, 5);

int sum =
    numbers.stream()
        .reduce(0, (a, b) -> a + b);

System.out.println(sum);
```

Output:

```text
15
```

---

## Mental Model

```text
1 + 2 + 3 + 4 + 5
          ↓
         15
```

---

## Method Reference

```java
int sum =
    numbers.stream()
        .reduce(0, Integer::sum);
```

---

# 14. sorted()

`sorted()` sorts stream elements.

Example:

```java
List<Integer> numbers =
    Arrays.asList(5, 2, 8, 1, 3);

numbers.stream()
    .sorted()
    .forEach(System.out::println);
```

Output:

```text
1
2
3
5
8
```

---

## Reverse Order

```java
numbers.stream()
    .sorted(Comparator.reverseOrder())
    .forEach(System.out::println);
```

---

# 15. collect()

`collect()` gathers stream results into a collection or another result structure.

Example:

```java
List<Integer> numbers =
    Arrays.asList(1, 2, 3, 4, 5, 6);

List<Integer> evenNumbers =
    numbers.stream()
        .filter(n -> n % 2 == 0)
        .collect(Collectors.toList());
```

Result:

```text
[2, 4, 6]
```

---

## Common Collectors

```text
Collectors.toList()
Collectors.toSet()
Collectors.joining()
Collectors.groupingBy()
Collectors.partitioningBy()
Collectors.counting()
```

---

# 16. Optional

`Optional<T>` represents a value that may or may not be present.

Package:

```text
java.util
```

Example:

```java
Optional<String> name =
    Optional.of("Rahul");
```

Empty:

```java
Optional<String> name =
    Optional.empty();
```

---

## Why Optional?

It helps make absence of a value explicit.

Instead of:

```java
String name = null;
```

we can use:

```java
Optional<String> name =
    Optional.empty();
```

This topic is covered deeply in:

```text
02-Optional.md
```

---

# 17. Interface Default Methods

Before Java 8, interfaces could not normally contain implemented instance methods.

Java 8 introduced `default` methods.

Example:

```java
interface Vehicle {

    void start();

    default void stop() {
        System.out.println(
            "Vehicle stopped"
        );
    }
}
```

A class implementing the interface automatically gets the default implementation unless it overrides it.

---

## Example

```java
class Car implements Vehicle {

    @Override
    public void start() {
        System.out.println(
            "Car started"
        );
    }
}
```

Now:

```java
Car car = new Car();

car.start();
car.stop();
```

Output:

```text
Car started
Vehicle stopped
```

---

## Why Were Default Methods Introduced?

A major reason was **interface evolution**.

Suppose an interface is already implemented by many classes.

Adding a new abstract method would force every implementation to implement it.

Default methods allow a method implementation to be added without immediately breaking existing implementations.

---

# 18. Interface Static Methods

Java 8 also allows static methods in interfaces.

Example:

```java
interface Calculator {

    static int square(int n) {
        return n * n;
    }
}
```

Call it using the interface name:

```java
int result =
    Calculator.square(5);
```

Output:

```text
25
```

---

## Important

Interface static methods are called using:

```text
InterfaceName.method()
```

not through an implementing object.

Example:

```java
Calculator calculator =
    new Calculator() {
    };

Calculator.square(5);
```

The normal invocation is through the interface type itself:

```java
Calculator.square(5);
```

---

# 19. Java 8 Date and Time API

Java 8 introduced a much-improved date/time API in:

```text
java.time
```

Important classes:

```text
LocalDate
LocalTime
LocalDateTime
Instant
Duration
Period
ZonedDateTime
```

Example:

```java
LocalDate today =
    LocalDate.now();

System.out.println(today);
```

---

## Why Was It Introduced?

Older APIs such as:

```text
Date
Calendar
SimpleDateFormat
```

had design limitations.

The Java 8 API provides:

```text
Immutable types
Thread-safe types
Clearer API
Better timezone support
Better date/time calculations
```

A dedicated file covers this topic in depth:

```text
03-Date-and-Time-API.md
```

---

# 20. CompletableFuture

Java 8 introduced `CompletableFuture` for asynchronous and composable programming.

Package:

```text
java.util.concurrent
```

Example:

```java
CompletableFuture<String> future =
    CompletableFuture.supplyAsync(
        () -> "Hello Java"
    );

future.thenAccept(
    System.out::println
);
```

---

## Why CompletableFuture?

It allows asynchronous operations to be composed.

Conceptually:

```text
Task A
  ↓
Task B
  ↓
Task C
```

without manually managing every thread and callback.

Important methods include:

```text
supplyAsync()
runAsync()
thenApply()
thenAccept()
thenCompose()
thenCombine()
exceptionally()
```

---

# 21. Java 8 vs Before Java 8

## Collection Processing

Before Java 8:

```java
List<Integer> numbers =
    Arrays.asList(1, 2, 3, 4, 5);

for (Integer number : numbers) {

    if (number % 2 == 0) {
        System.out.println(number);
    }
}
```

Java 8:

```java
numbers.stream()
    .filter(n -> n % 2 == 0)
    .forEach(System.out::println);
```

---

## Behavior

Before Java 8:

```text
Anonymous classes
```

Java 8:

```text
Lambda expressions
```

---

## Interfaces

Before Java 8:

```text
Abstract methods
```

Java 8:

```text
Abstract methods
+
default methods
+
static methods
```

---

## Date/Time

Before Java 8:

```text
Date
Calendar
SimpleDateFormat
```

Java 8:

```text
java.time
```

---

# 22. Common Mistakes

## ❌ Mistake 1 — Thinking Every Lambda Is a Function

A lambda needs a **target type**.

Usually that target type is a functional interface.

This does not stand alone:

```java
(a, b) -> a + b
```

It needs a compatible target type.

For example:

```java
BinaryOperator<Integer> add =
    (a, b) -> a + b;
```

---

## ❌ Mistake 2 — Thinking Stream Stores Data

A Stream does not normally store the collection's data.

It processes elements from a source.

---

## ❌ Mistake 3 — Reusing a Consumed Stream

A stream cannot normally be reused after a terminal operation.

Wrong:

```java
Stream<Integer> stream =
    numbers.stream();

stream.forEach(
    System.out::println
);

stream.forEach(
    System.out::println
);
```

This results in:

```text
IllegalStateException
```

because the stream has already been consumed.

---

## ❌ Mistake 4 — Thinking Intermediate Operations Execute Immediately

Operations such as:

```text
filter()
map()
sorted()
```

are generally lazy.

They execute when a terminal operation triggers the pipeline.

---

## ❌ Mistake 5 — Confusing filter() and map()

Remember:

```text
filter()
→ select

map()
→ transform
```

---

## ❌ Mistake 6 — Calling Interface Static Method Through Object

Prefer:

```java
Calculator.square(5);
```

not:

```java
calculator.square(5);
```

---

## ❌ Mistake 7 — Assuming Optional Eliminates All Nulls

Optional helps model optional results, but it does not magically prevent every possible `NullPointerException`.

---

## ❌ Mistake 8 — Using Parallel Streams Everywhere

Parallel streams can help in appropriate workloads, but they are not automatically faster.

Factors include:

```text
Data size
Work per element
CPU availability
Ordering requirements
Splitting overhead
Shared state
```

---

# 23. Interview Questions

## 🔥 Q1. What were the major features introduced in Java 8?

Important Java 8 features include:

```text
Lambda expressions
Functional interfaces
Method references
Stream API
Optional
Default interface methods
Static interface methods
New Date/Time API
CompletableFuture
```

---

## 🔥 Q2. What is a lambda expression?

A lambda expression is a concise representation of behavior that can be used where a compatible functional interface is expected.

Example:

```java
Runnable task =
    () -> System.out.println("Running");
```

---

## 🔥 Q3. What is a functional interface?

An interface with exactly one abstract method is a functional interface.

It can still contain:

```text
default methods
static methods
Object methods
```

---

## 🔥 Q4. Why use `@FunctionalInterface`?

It allows the compiler to verify that the interface remains a valid functional interface.

---

## 🔥 Q5. Can a functional interface contain default methods?

Yes.

Example:

```java
@FunctionalInterface
interface Test {

    void execute();

    default void print() {
        System.out.println("Test");
    }
}
```

---

## 🔥 Q6. Can a functional interface contain static methods?

Yes.

```java
@FunctionalInterface
interface Test {

    void execute();

    static void info() {
        System.out.println("Info");
    }
}
```

---

## 🔥 Q7. What is a method reference?

A method reference is a shorthand for a lambda when the lambda simply delegates to an existing method.

Example:

```java
System.out::println
```

instead of:

```java
x -> System.out.println(x)
```

---

## 🔥 Q8. What is the Stream API?

The Stream API provides a declarative way to process sequences of data using operations such as:

```text
filter
map
sorted
reduce
collect
```

---

## 🔥 Q9. Is a Stream a data structure?

No.

A stream is a processing abstraction over a source of data.

---

## 🔥 Q10. What is lazy evaluation in streams?

Intermediate stream operations are generally not executed immediately.

They are evaluated when a terminal operation is invoked.

---

## 🔥 Q11. What is the difference between intermediate and terminal operations?

```text
Intermediate
→ returns Stream
→ generally lazy

Terminal
→ produces result/side effect
→ triggers processing
```

---

## 🔥 Q12. Is `filter()` intermediate or terminal?

Intermediate.

```java
stream.filter(...);
```

returns another stream.

---

## 🔥 Q13. Is `forEach()` intermediate or terminal?

Terminal.

```java
stream.forEach(...);
```

consumes the stream.

---

## 🔥 Q14. Difference between `map()` and `filter()`?

```text
map()
→ transforms elements

filter()
→ selects elements
```

---

## 🔥 Q15. What does `reduce()` do?

It combines multiple stream elements into a single result.

Example:

```java
int sum =
    numbers.stream()
        .reduce(0, Integer::sum);
```

---

## 🔥 Q16. Can a Stream be reused?

No.

Once a terminal operation consumes a stream, it cannot normally be reused.

---

## 🔥 Q17. Why were default methods introduced?

Primarily to allow interfaces to evolve by adding behavior without requiring every existing implementation to immediately provide a new implementation.

---

## 🔥 Q18. Can interfaces have static methods in Java 8?

Yes.

They are invoked through the interface name.

```java
Calculator.square(5);
```

---

## 🔥 Q19. What is Optional?

`Optional<T>` is a container type representing a value that may or may not be present.

---

## 🔥 Q20. Why was the Java 8 Date/Time API introduced?

It provides a cleaner, more immutable and thread-safe approach to date/time handling than the older APIs.

---

## 🔥 Q21. What is CompletableFuture?

`CompletableFuture` is an API for asynchronous computation and composing asynchronous operations.

---

## 🔥 Q22. Does Java 8 support functional programming?

Yes.

Java remains primarily object-oriented, but Java 8 introduced important functional programming capabilities such as:

```text
Lambda expressions
Functional interfaces
Higher-order-style APIs
Stream processing
Method references
```

---

## 🔥 Q23. What is the difference between Lambda and Anonymous Class?

A lambda is a concise representation of behavior for a functional interface.

An anonymous class creates an anonymous class instance and can contain fields, multiple methods, initialization blocks, and state.

---

## 🔥 Q24. Can a lambda access local variables?

Yes, but local variables referenced from a lambda must be final or **effectively final**.

Example:

```java
int number = 10;

Runnable task =
    () -> System.out.println(number);
```

This works because `number` is not reassigned.

---

## 🔥 Q25. What is an effectively final variable?

A local variable that is not explicitly declared `final` but is assigned only once and never reassigned.

Example:

```java
int number = 10;

Runnable task =
    () -> System.out.println(number);
```

`number` is effectively final.

---

## 🔥 Q26. What is the difference between `forEach()` and a normal `for` loop?

A normal loop provides explicit control over iteration.

Stream `forEach()` expresses an action over stream elements and is a terminal operation.

For complex control flow such as:

```text
break
continue
```

a normal loop may be more appropriate.

---

## 🔥 Q27. Are streams always faster than loops?

No.

Streams provide expressive data-processing operations, but performance depends on the workload and implementation.

---

## 🔥 Q28. Are parallel streams always faster?

No.

Parallel processing introduces overhead and can be slower for small or unsuitable workloads.

---

## 🔥 Q29. What is the difference between `Collection` and `Stream`?

```text
Collection
→ stores/manages data

Stream
→ processes data
```

A stream does not replace a collection as a data structure.

---

## 🔥 Q30. What is the biggest conceptual change introduced by Java 8?

Java 8 significantly expanded Java's support for functional-style programming through lambdas, functional interfaces, method references, and the Stream API while retaining Java's object-oriented model.

---

# 24. 30-Second Interview Answer

> Java 8 was a major release because it introduced functional programming capabilities into Java. The key features include lambda expressions, functional interfaces, method references, and the Stream API for declarative collection processing. It also introduced default and static methods in interfaces, `Optional` for modeling potentially absent values, the modern `java.time` API, and `CompletableFuture` for asynchronous programming. A key concept is that stream intermediate operations are generally lazy and terminal operations trigger processing.

---

# 25. Cheat Sheet

```text
================ JAVA 8 CHEAT SHEET ================


LAMBDA
---------------------------------

(parameters) -> expression

Example:

x -> x * 2


FUNCTIONAL INTERFACE
---------------------------------

Exactly ONE abstract method

Can also have:

default methods
static methods
Object methods


@FunctionalInterface
---------------------------------

Compiler verification


PREDICATE
---------------------------------

T → boolean

test()


FUNCTION
---------------------------------

T → R

apply()


CONSUMER
---------------------------------

T → void

accept()


SUPPLIER
---------------------------------

() → T

get()


UNARY OPERATOR
---------------------------------

T → T


BINARY OPERATOR
---------------------------------

(T, T) → T


METHOD REFERENCE
---------------------------------

ClassName::method
object::method
ClassName::new


STREAM
---------------------------------

Source
 ↓
Intermediate Operations
 ↓
Terminal Operation


COMMON INTERMEDIATE
---------------------------------

filter()
map()
sorted()
distinct()
limit()
skip()


COMMON TERMINAL
---------------------------------

forEach()
collect()
reduce()
count()
min()
max()
anyMatch()
allMatch()


FILTER
---------------------------------

Select


MAP
---------------------------------

Transform


REDUCE
---------------------------------

Many values
 ↓
One result


COLLECT
---------------------------------

Stream
 ↓
Collection/result


STREAM
---------------------------------

Does NOT store data

Processes data


OPTIONAL
---------------------------------

Value may be present
or absent


INTERFACE DEFAULT
---------------------------------

default method
→ implementation inside interface


INTERFACE STATIC
---------------------------------

static method
→ called using interface name


DATE/TIME
---------------------------------

java.time

LocalDate
LocalTime
LocalDateTime
Instant
Duration
Period
ZonedDateTime


ASYNC
---------------------------------

CompletableFuture


IMPORTANT STREAM RULE
---------------------------------

Intermediate
→ lazy

Terminal
→ triggers pipeline


IMPORTANT STREAM RULE
---------------------------------

Stream cannot normally
be reused after terminal operation.


IMPORTANT LAMBDA RULE
---------------------------------

Captured local variables
must be final or effectively final.
```

---

# 26. Final Mental Model

```text
                         JAVA 8
                           |
       +-------------------+-------------------+
       |                   |                   |
    FUNCTIONAL          STREAMS            API DESIGN
       |                   |                   |
       ↓                   ↓                   ↓
    Lambda              Source            Interfaces
    Predicate             ↓                   |
    Function         Intermediate             ↓
    Consumer              ↓              default
    Supplier          Terminal             static
    Method Ref.           |
                          ↓
                     Data Processing


       +-------------------+-------------------+
       |                   |
    OPTIONAL           DATE/TIME
       |                   |
       ↓                   ↓
 Optional<T>         java.time
       |             LocalDate
       ↓             LocalTime
  value/empty        Instant
                     ZonedDateTime


                    ASYNC
                      |
                      ↓
              CompletableFuture


====================================================

BIGGEST JAVA 8 SHIFT

Before Java 8:

Anonymous Classes
      +
External Iteration
      +
Verbose APIs

After Java 8:

Lambda
   +
Functional Interfaces
   +
Stream API
   +
Method References
   +
Modern APIs


====================================================

STREAM PIPELINE

Collection
    ↓
stream()
    ↓
filter()
    ↓
map()
    ↓
sorted()
    ↓
collect()
    ↓
Result


====================================================

KEY INTERVIEW DIFFERENCES

filter()
→ selects

map()
→ transforms

reduce()
→ combines

collect()
→ gathers

intermediate
→ lazy

terminal
→ executes pipeline


====================================================

LAMBDA

Lambda
   ↓
Needs target type
   ↓
Usually Functional Interface
   ↓
Behavior is supplied


====================================================

FUNCTIONAL INTERFACE

One abstract method
        +
default methods
        +
static methods
        +
Object methods


====================================================

INTERFACE EVOLUTION

Java before 8
        ↓
Adding abstract method
        ↓
Existing implementations may break

Java 8
        ↓
default method
        ↓
Interface can provide implementation


====================================================

CORE JAVA 8 MEMORY TRICK

L F M S O D C

L → Lambda
F → Functional Interface
M → Method Reference
S → Stream API
O → Optional
D → Date/Time API
C → CompletableFuture

Plus:

default + static methods in interfaces
```

---

# 🏁 Final Takeaways

- Java 8 introduced major functional-programming capabilities.
- Lambda expressions provide concise behavior implementations.
- Lambdas generally target functional interfaces.
- A functional interface has exactly one abstract method.
- `@FunctionalInterface` allows compiler verification.
- `Predicate` returns a boolean.
- `Function` transforms one type into another.
- `Consumer` accepts a value and returns nothing.
- `Supplier` produces a value without input.
- `UnaryOperator` maps `T → T`.
- `BinaryOperator` maps `(T, T) → T`.
- Method references shorten certain lambdas.
- Streams process data rather than store it.
- `filter()` selects.
- `map()` transforms.
- `reduce()` combines.
- `collect()` gathers results.
- Intermediate stream operations are generally lazy.
- Terminal operations trigger stream processing.
- A consumed stream cannot normally be reused.
- `Optional` represents a value that may be absent.
- Java 8 added default methods to interfaces.
- Java 8 added static methods to interfaces.
- `java.time` replaced many problematic use cases of older date/time APIs.
- `CompletableFuture` supports asynchronous and composable computations.
- Java 8 made Java more expressive without abandoning its object-oriented foundation.

---

