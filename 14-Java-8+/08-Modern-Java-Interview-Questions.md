
# 🎯 Modern Java — Interview Questions

> **A focused interview revision guide covering the major modern Java features from Java 8 through Java 21+, including functional programming, Optional, the Date/Time API, interface enhancements, records, sealed classes, pattern matching, and related design concepts.**

---

# 📑 Table of Contents

- [1. Java 8 Features](#1-java-8-features)
- [2. Lambda Expressions](#2-lambda-expressions)
- [3. Functional Interfaces](#3-functional-interfaces)
- [4. Method References](#4-method-references)
- [5. Stream API](#5-stream-api)
- [6. `map()` vs `filter()`](#6-map-vs-filter)
- [7. Intermediate vs Terminal Operations](#7-intermediate-vs-terminal-operations)
- [8. Optional](#8-optional)
- [9. Why Use Optional?](#9-why-use-optional)
- [10. `orElse()` vs `orElseGet()`](#10-orelse-vs-orelseget)
- [11. Date and Time API](#11-date-and-time-api)
- [12. `LocalDate` vs `LocalDateTime`](#12-localdate-vs-localdatetime)
- [13. Interface Enhancements](#13-interface-enhancements)
- [14. Default Methods](#14-default-methods)
- [15. Static Methods in Interfaces](#15-static-methods-in-interfaces)
- [16. Private Interface Methods](#16-private-interface-methods)
- [17. Records](#17-records)
- [18. Records vs Normal Classes](#18-records-vs-normal-classes)
- [19. Record Limitations](#19-record-limitations)
- [20. Sealed Classes](#20-sealed-classes)
- [21. `sealed` vs `final` vs `non-sealed`](#21-sealed-vs-final-vs-non-sealed)
- [22. Pattern Matching](#22-pattern-matching)
- [23. Pattern Matching with `instanceof`](#23-pattern-matching-with-instanceof)
- [24. Pattern Matching with `switch`](#24-pattern-matching-with-switch)
- [25. Record Patterns](#25-record-patterns)
- [26. Sealed Classes + Pattern Matching](#26-sealed-classes--pattern-matching)
- [27. Important Java Version Timeline](#27-important-java-version-timeline)
- [28. Common Interview Traps](#28-common-interview-traps)
- [29. Top 25 Interview Questions](#29-top-25-interview-questions)
- [30. Rapid-Fire Questions](#30-rapid-fire-questions)
- [31. 30-Second Interview Answer](#31-30-second-interview-answer)
- [32. Cheat Sheet](#32-cheat-sheet)
- [33. Final Mental Model](#33-final-mental-model)

---

# 1. Java 8 Features

Java 8 introduced several major language and library features.

The most important ones are:

```text
Lambda Expressions
Functional Interfaces
Method References
Stream API
Optional
Default Interface Methods
Static Interface Methods
New Date/Time API
```

These features significantly changed how Java code can be written.

---

# 2. Lambda Expressions

A lambda expression provides a concise way to represent behavior.

Traditional anonymous class:

```java
Runnable task = new Runnable() {

    @Override
    public void run() {

        System.out.println(
            "Running"
        );
    }
};
```

Lambda:

```java
Runnable task = () -> {

    System.out.println(
        "Running"
    );
};
```

Basic syntax:

```text
(parameters) -> expression
```

or:

```text
(parameters) -> {
    statements;
}
```

---

# 3. Functional Interfaces

A functional interface contains exactly one abstract method.

Example:

```java
@FunctionalInterface
interface Calculator {

    int calculate(
        int a,
        int b
    );
}
```

Lambda:

```java
Calculator add =
    (a, b) -> a + b;
```

Common built-in functional interfaces:

```text
Predicate<T>
Consumer<T>
Function<T, R>
Supplier<T>
UnaryOperator<T>
BinaryOperator<T>
```

Important:

A functional interface can still contain:

```text
default methods
static methods
private methods
```

The restriction applies to:

```text
abstract methods
```

---

# 4. Method References

Method references provide a shorter syntax for certain lambdas.

Lambda:

```java
names.forEach(
    name -> System.out.println(name)
);
```

Method reference:

```java
names.forEach(
    System.out::println
);
```

Syntax:

```text
ClassName::methodName
```

or:

```text
object::methodName
```

Common forms:

```text
ClassName::staticMethod
ClassName::instanceMethod
object::instanceMethod
ClassName::new
```

---

# 5. Stream API

The Stream API was introduced in Java 8.

A stream represents a pipeline for processing data.

Example:

```java
List<Integer> numbers =
    List.of(
        10,
        15,
        20,
        25
    );

numbers.stream()
    .filter(n -> n > 15)
    .forEach(
        System.out::println
    );
```

Pipeline:

```text
Source
  ↓
Intermediate Operations
  ↓
Terminal Operation
```

Example:

```text
List
 ↓
stream()
 ↓
filter()
 ↓
map()
 ↓
collect()
```

---

# 6. `map()` vs `filter()`

`filter()` selects elements.

```java
numbers.stream()
    .filter(n -> n > 10);
```

Conceptually:

```text
Input
 ↓
condition
 ↓
selected elements
```

`map()` transforms elements.

```java
numbers.stream()
    .map(n -> n * 2);
```

Conceptually:

```text
Input
 ↓
transformation
 ↓
new values
```

Memory trick:

```text
filter
→ keep/remove

map
→ transform
```

---

# 7. Intermediate vs Terminal Operations

Intermediate operations return another stream.

Examples:

```text
filter()
map()
sorted()
distinct()
limit()
skip()
```

Example:

```java
numbers.stream()
    .filter(n -> n > 10)
    .map(n -> n * 2);
```

No terminal operation has been called yet.

Terminal operations produce a result or side effect.

Examples:

```text
collect()
forEach()
reduce()
count()
min()
max()
findFirst()
anyMatch()
```

Example:

```java
long count =
    numbers.stream()
        .filter(n -> n > 10)
        .count();
```

Important:

```text
Intermediate operations
→ lazy

Terminal operation
→ triggers pipeline execution
```

---

# 8. Optional

`Optional<T>` is a container that may contain:

```text
a value
OR
no value
```

Example:

```java
Optional<String> name =
    Optional.of("Java");
```

Empty:

```java
Optional<String> name =
    Optional.empty();
```

The purpose is to represent possible absence explicitly.

---

# 9. Why Use Optional?

Traditional code can produce:

```java
String name =
    findName();

if (name != null) {

    System.out.println(
        name.length()
    );
}
```

`Optional` can make the absence explicit:

```java
Optional<String> name =
    findName();

name.ifPresent(
    value ->
        System.out.println(
            value.length()
        )
);
```

Important:

`Optional` does not magically eliminate all `NullPointerException`s.

It is mainly a modeling tool for optional return values.

---

# 10. `orElse()` vs `orElseGet()`

This is a common interview question.

`orElse()` receives an already-evaluated fallback expression.

```java
String name =
    optional.orElse(
        getDefaultName()
    );
```

`orElseGet()` receives a `Supplier`.

```java
String name =
    optional.orElseGet(
        () -> getDefaultName()
    );
```

Important distinction:

```text
orElse()
→ fallback expression is evaluated eagerly

orElseGet()
→ supplier is evaluated only when needed
```

Example:

```java
static String getDefault() {

    System.out.println(
        "Default called"
    );

    return "Default";
}
```

```java
Optional<String> value =
    Optional.of("Java");

String result =
    value.orElse(
        getDefault()
    );
```

The fallback method can still execute even though the `Optional` contains a value.

With:

```java
String result =
    value.orElseGet(
        () -> getDefault()
    );
```

the supplier is not invoked when the value is present.

---

# 11. Date and Time API

Java 8 introduced the modern `java.time` API.

Important classes:

```text
LocalDate
LocalTime
LocalDateTime
ZonedDateTime
Instant
Duration
Period
```

Example:

```java
LocalDate today =
    LocalDate.now();
```

Example:

```java
LocalDateTime now =
    LocalDateTime.now();
```

The modern API is designed to be more consistent and safer for date/time operations than the older mutable date/time APIs.

---

# 12. `LocalDate` vs `LocalDateTime`

`LocalDate` stores:

```text
date only
```

Example:

```java
LocalDate date =
    LocalDate.now();
```

`LocalDateTime` stores:

```text
date + time
```

Example:

```java
LocalDateTime dateTime =
    LocalDateTime.now();
```

Neither contains a time-zone.

For timezone-aware values:

```java
ZonedDateTime now =
    ZonedDateTime.now();
```

---

# 13. Interface Enhancements

Java added important capabilities to interfaces.

Java 8 introduced:

```text
default methods
static methods
```

Java 9 introduced:

```text
private methods
```

These features allow interfaces to contain more reusable implementation logic while retaining their core abstraction role.

---

# 14. Default Methods

A default method provides an implementation inside an interface.

Example:

```java
interface Vehicle {

    default void start() {

        System.out.println(
            "Vehicle started"
        );
    }
}
```

A class can inherit it:

```java
class Car
    implements Vehicle {
}
```

Then:

```java
Car car =
    new Car();

car.start();
```

Why were default methods introduced?

One major reason was API evolution.

An interface could gain new behavior without requiring every existing implementing class to immediately implement a new abstract method.

---

# 15. Static Methods in Interfaces

Interfaces can contain static methods.

Example:

```java
interface MathUtil {

    static int square(
        int n
    ) {

        return n * n;
    }
}
```

Call it using the interface name:

```java
int result =
    MathUtil.square(5);
```

Important:

Interface static methods are not inherited by implementing classes.

You should call them using:

```text
InterfaceName.method()
```

---

# 16. Private Interface Methods

Java 9 introduced private methods in interfaces.

Example:

```java
interface Logger {

    default void info() {

        log(
            "INFO"
        );
    }

    default void error() {

        log(
            "ERROR"
        );
    }

    private void log(
        String message
    ) {

        System.out.println(
            message
        );
    }
}
```

The private method is used internally by the interface.

It helps avoid duplication between default methods.

Private interface methods are not accessible from implementing classes.

---

# 17. Records

Records were introduced as a preview feature in Java 14 and finalized in Java 16.

A record is a concise way to model data-oriented classes.

Example:

```java
record Student(
    int id,
    String name
) {
}
```

Java automatically provides important members such as:

```text
private final fields
accessor methods
canonical constructor
equals()
hashCode()
toString()
```

Record accessors use the component name:

```java
Student student =
    new Student(
        101,
        "Rahul"
    );

System.out.println(
    student.name()
);
```

Not:

```java
student.getName();
```

---

# 18. Records vs Normal Classes

Normal class:

```java
class Student {

    private final int id;

    private final String name;

    Student(
        int id,
        String name
    ) {

        this.id = id;
        this.name = name;
    }

    public int id() {

        return id;
    }

    public String name() {

        return name;
    }
}
```

Record:

```java
record Student(
    int id,
    String name
) {
}
```

Records significantly reduce boilerplate for data carriers.

---

# 19. Record Limitations

A record:

```text
cannot extend another class
```

because every record already extends:

```text
java.lang.Record
```

Records are implicitly:

```text
final
```

Therefore:

```text
record
→ cannot be subclassed
```

A record can implement interfaces.

Example:

```java
interface Printable {

    void print();
}
```

```java
record Student(
    int id
) implements Printable {

    @Override
    public void print() {

        System.out.println(
            id
        );
    }
}
```

---

# 20. Sealed Classes

Sealed classes restrict which classes can directly extend them.

Example:

```java
sealed class Payment
    permits CardPayment, CashPayment {
}
```

Permitted subclass:

```java
final class CardPayment
    extends Payment {
}
```

Not permitted:

```java
class UpiPayment
    extends Payment {
}
```

unless it is included in the permitted hierarchy.

Sealed classes were finalized in Java 17.

---

# 21. `sealed` vs `final` vs `non-sealed`

```text
final
→ no subclasses

sealed
→ only specified subclasses

non-sealed
→ inheritance is reopened
```

Example:

```java
sealed class Animal
    permits Dog {
}
```

A permitted subclass can be:

```java
final class Dog
    extends Animal {
}
```

or:

```java
sealed class Dog
    extends Animal
    permits Labrador {
}
```

or:

```java
non-sealed class Dog
    extends Animal {
}
```

---

# 22. Pattern Matching

Pattern matching combines operations such as:

```text
type checking
+
casting
+
variable binding
```

Example:

```java
Object value =
    "Hello";

if (value instanceof String str) {

    System.out.println(
        str.length()
    );
}
```

Instead of:

```java
if (value instanceof String) {

    String str =
        (String) value;

    System.out.println(
        str.length()
    );
}
```

The pattern version removes the explicit cast.

---

# 23. Pattern Matching with `instanceof`

General syntax:

```text
expression instanceof Type variable
```

Example:

```java
if (obj instanceof Integer number) {

    System.out.println(
        number + 10
    );
}
```

The variable:

```text
number
```

is called a pattern variable.

Pattern matching for `instanceof` was finalized in Java 16.

---

# 24. Pattern Matching with `switch`

Modern Java allows type patterns in `switch`.

Example:

```java
static String describe(
    Object value
) {

    return switch (value) {

        case String str ->
            "String: " + str;

        case Integer number ->
            "Integer: " + number;

        default ->
            "Other";
    };
}
```

Pattern matching for `switch` was finalized in Java 21.

---

# 25. Record Patterns

Record patterns allow direct extraction of record components.

Example:

```java
record Point(
    int x,
    int y
) {
}
```

Traditional pattern:

```java
if (obj instanceof Point point) {

    System.out.println(
        point.x()
    );

    System.out.println(
        point.y()
    );
}
```

Record pattern:

```java
if (obj instanceof Point(int x, int y)) {

    System.out.println(x);

    System.out.println(y);
}
```

Record patterns were finalized in Java 21.

---

# 26. Sealed Classes + Pattern Matching

These features work naturally together.

Example:

```java
sealed interface Shape
    permits Circle, Rectangle {
}
```

```java
record Circle(
    double radius
) implements Shape {
}
```

```java
record Rectangle(
    double width,
    double height
) implements Shape {
}
```

Now:

```java
static double area(
    Shape shape
) {

    return switch (shape) {

        case Circle c ->
            Math.PI *
            c.radius() *
            c.radius();

        case Rectangle r ->
            r.width() *
            r.height();
    };
}
```

The compiler knows the permitted variants.

Therefore, the switch can be exhaustive without a `default` case.

---

# 27. Important Java Version Timeline

```text
=========================================================

JAVA 8
---------------------------------------------------------
Lambda expressions
Functional interfaces
Method references
Stream API
Optional
Default methods
Static interface methods
java.time API


JAVA 9
---------------------------------------------------------
Private interface methods


JAVA 10
---------------------------------------------------------
Local variable type inference
var


JAVA 11
---------------------------------------------------------
Additional String methods
HTTP Client API finalized


JAVA 14
---------------------------------------------------------
Records
Preview


JAVA 15
---------------------------------------------------------
Sealed classes
Preview


JAVA 16
---------------------------------------------------------
Pattern matching for instanceof
Finalized

Records
Finalized


JAVA 17
---------------------------------------------------------
Sealed classes and interfaces
Finalized


JAVA 21
---------------------------------------------------------
Pattern matching for switch
Finalized

Record patterns
Finalized


=========================================================
```

---

# 28. Common Interview Traps

## ❌ Trap 1 — `Optional` Means No NullPointerException

False.

`Optional` helps model possible absence.

It does not make all code immune to null-related errors.

---

## ❌ Trap 2 — `orElse()` Is Lazy

False.

The fallback expression passed to `orElse()` is evaluated eagerly.

Use:

```java
orElseGet(
    () -> expensiveOperation()
)
```

when lazy fallback evaluation is desired.

---

## ❌ Trap 3 — Interface Static Methods Are Inherited

False.

Call them using:

```java
InterfaceName.method()
```

---

## ❌ Trap 4 — Default Methods Make Interfaces Abstract Classes

False.

Interfaces and abstract classes remain different language constructs.

Default methods simply allow interfaces to provide implementations.

---

## ❌ Trap 5 — Records Are Mutable

Record components are final references.

However, if a component refers to a mutable object, that object itself can still be mutable.

Example:

```java
record Team(
    List<String> players
) {
}
```

The reference is final, but the list may still be mutable.

Therefore:

```text
final reference
≠
deep immutability
```

---

## ❌ Trap 6 — Records Can Extend Classes

False.

Records cannot extend arbitrary classes.

They implicitly extend:

```text
java.lang.Record
```

---

## ❌ Trap 7 — Sealed Means Immutable

False.

`sealed` controls inheritance.

It does not automatically control object state.

---

## ❌ Trap 8 — `non-sealed` Means Final

Exactly opposite.

```text
non-sealed
→ inheritance is open
```

---

## ❌ Trap 9 — `List<String>` Can Be Used as an `instanceof` Pattern

Normally no.

Due to type erasure:

```java
obj instanceof List<String>
```

is not allowed.

A reifiable form such as:

```java
obj instanceof List<?> list
```

can be used.

---

## ❌ Trap 10 — Every Modern Java Feature Was Introduced in Java 8

False.

Modern Java features arrived across many releases.

Examples:

```text
Records
→ Java 16 finalized

Sealed classes
→ Java 17 finalized

Pattern matching for switch
→ Java 21 finalized
```

---

# 29. Top 25 Interview Questions

## 🔥 Q1. What are the major Java 8 features?

The major features include:

```text
Lambda expressions
Functional interfaces
Stream API
Method references
Optional
Default methods
Static interface methods
New Date/Time API
```

---

## 🔥 Q2. What is a functional interface?

An interface with exactly one abstract method.

It can still have multiple:

```text
default methods
static methods
private methods
```

---

## 🔥 Q3. What is a lambda expression?

A concise representation of behavior that can be assigned to a functional interface.

Example:

```java
Runnable task =
    () -> System.out.println(
        "Hello"
    );
```

---

## 🔥 Q4. What is a method reference?

A shorthand for certain lambda expressions.

Example:

```java
names.forEach(
    System.out::println
);
```

---

## 🔥 Q5. What is a stream?

A stream is a pipeline abstraction used to process data from a source through operations.

It does not itself represent a data structure.

---

## 🔥 Q6. Does a stream store data?

No.

A stream processes elements from a source.

The source may be:

```text
Collection
Array
I/O channel
Generated data
```

---

## 🔥 Q7. What is lazy evaluation in streams?

Intermediate stream operations are generally not executed immediately.

They are evaluated when a terminal operation triggers the pipeline.

---

## 🔥 Q8. What is Optional?

`Optional<T>` is a container that may contain a value or represent no value.

---

## 🔥 Q9. What is the difference between `orElse()` and `orElseGet()`?

```text
orElse()
→ eager fallback evaluation

orElseGet()
→ lazy supplier evaluation
```

---

## 🔥 Q10. What is the Java 8 Date/Time API?

The `java.time` API provides modern date/time classes such as:

```text
LocalDate
LocalTime
LocalDateTime
ZonedDateTime
Instant
Duration
Period
```

---

## 🔥 Q11. Why were default methods introduced?

A major purpose was interface evolution.

They allow interfaces to add implemented behavior without requiring all existing implementing classes to immediately provide an implementation for that new method.

---

## 🔥 Q12. Can interfaces have static methods?

Yes.

They are called using the interface name.

```java
MathUtil.square(5);
```

---

## 🔥 Q13. Can interfaces have private methods?

Yes, since Java 9.

They can be used internally by interface methods.

---

## 🔥 Q14. What is a record?

A record is a concise data-oriented class declaration that automatically provides standard members such as accessors, a canonical constructor, `equals()`, `hashCode()`, and `toString()`.

---

## 🔥 Q15. Are records immutable?

Records provide final component fields, but they do not guarantee deep immutability of referenced mutable objects.

---

## 🔥 Q16. Can a record extend a class?

No.

A record implicitly extends `java.lang.Record`.

---

## 🔥 Q17. Can a record implement an interface?

Yes.

Example:

```java
record Student(
    int id
) implements Printable {
}
```

---

## 🔥 Q18. What is a sealed class?

A class that restricts which classes can directly extend it.

---

## 🔥 Q19. What is the purpose of `permits`?

It specifies the allowed direct subclasses or implementations of a sealed type.

---

## 🔥 Q20. What is `non-sealed`?

It reopens inheritance from that point in a sealed hierarchy.

---

## 🔥 Q21. What is pattern matching?

A language feature that combines operations such as type checking, variable binding, and casting into a more concise form.

---

## 🔥 Q22. When was `instanceof` pattern matching finalized?

Java 16.

---

## 🔥 Q23. When was pattern matching for `switch` finalized?

Java 21.

---

## 🔥 Q24. What are record patterns?

They allow matching a record and extracting its components directly.

---

## 🔥 Q25. Why are sealed classes useful with pattern matching?

Because the compiler knows the permitted variants, which can allow exhaustive pattern matching over the hierarchy.

---

# 30. Rapid-Fire Questions

### Q26. Which Java version introduced lambdas?

```text
Java 8
```

### Q27. Which Java version introduced Streams?

```text
Java 8
```

### Q28. Which Java version introduced Optional?

```text
Java 8
```

### Q29. Which Java version introduced `private` interface methods?

```text
Java 9
```

### Q30. Which Java version finalized records?

```text
Java 16
```

### Q31. Which Java version finalized sealed classes?

```text
Java 17
```

### Q32. Which Java version finalized switch pattern matching?

```text
Java 21
```

### Q33. Which Java version finalized record patterns?

```text
Java 21
```

### Q34. Is `Optional` a replacement for every null check?

```text
No
```

### Q35. Is a stream a collection?

```text
No
```

### Q36. Can an interface have implemented methods?

```text
Yes
```

Through:

```text
default
static
private
```

### Q37. Can an interface have instance fields?

Only implicitly:

```text
public static final
```

constants.

### Q38. Can a record extend another class?

```text
No
```

### Q39. Can a record implement an interface?

```text
Yes
```

### Q40. Can a sealed class be abstract?

```text
Yes
```

### Q41. Can a sealed class be final?

```text
No
```

### Q42. Can a permitted subclass be final?

```text
Yes
```

### Q43. Can a permitted subclass be sealed?

```text
Yes
```

### Q44. Can a permitted subclass be non-sealed?

```text
Yes
```

### Q45. Does `null instanceof String` return true?

```text
No
```

### Q46. Can `List<String>` normally be used in an `instanceof` pattern?

```text
No
```

### Q47. Can `List<?>` be used?

```text
Yes
```

### Q48. Does `orElseGet()` always execute its supplier?

```text
No
```

### Q49. Does `orElse()` evaluate its fallback eagerly?

```text
Yes
```

### Q50. Are intermediate stream operations lazy?

```text
Yes
```

---

# 31. 30-Second Interview Answer

> Modern Java has introduced features that reduce boilerplate and improve expressiveness while retaining Java's strong type system. Java 8 introduced lambdas, functional interfaces, streams, Optional, default methods, and the modern Date/Time API. Java 9 added private interface methods. Records were finalized in Java 16, sealed classes in Java 17, and pattern matching for switch and record patterns in Java 21. Records simplify data carriers, sealed classes provide controlled inheritance, and pattern matching combines type checks with variable binding and can work with sealed hierarchies for exhaustive processing.

---

# 32. Cheat Sheet

```text
========================================================
              MODERN JAVA CHEAT SHEET
========================================================


JAVA 8
--------------------------------------------------------

Lambda
→ concise behavior

Functional Interface
→ exactly one abstract method

Method Reference
→ shorter lambda syntax

Stream
→ data processing pipeline

Optional
→ represents optional value

Default Method
→ implemented interface method

Static Interface Method
→ interface utility method

Date/Time API
→ java.time


========================================================

JAVA 9
--------------------------------------------------------

Private Interface Methods


========================================================

JAVA 16
--------------------------------------------------------

Pattern Matching for instanceof
→ finalized

Records
→ finalized


========================================================

JAVA 17
--------------------------------------------------------

Sealed Classes
→ finalized


========================================================

JAVA 21
--------------------------------------------------------

Pattern Matching for switch
→ finalized

Record Patterns
→ finalized


========================================================


LAMBDA

(parameters) -> expression


========================================================


FUNCTIONAL INTERFACE

Exactly ONE abstract method.


Can also contain:

default
static
private


========================================================


STREAM

Source
  ↓
Intermediate
  ↓
Intermediate
  ↓
Terminal


Intermediate:

filter
map
sorted
distinct
limit
skip


Terminal:

collect
forEach
reduce
count
min
max
findFirst
anyMatch


========================================================


OPTIONAL

Optional.of(value)
→ value must be non-null

Optional.ofNullable(value)
→ value may be null

Optional.empty()
→ no value


orElse()
→ eager fallback

orElseGet()
→ lazy fallback


========================================================


DATE/TIME

LocalDate
→ date

LocalTime
→ time

LocalDateTime
→ date + time

ZonedDateTime
→ date + time + zone

Instant
→ point on timeline

Period
→ date-based amount

Duration
→ time-based amount


========================================================


INTERFACE

default
→ instance behavior

static
→ interface utility

private
→ internal helper


========================================================


RECORD

record Student(
    int id,
    String name
) {
}


Automatically provides:

fields
constructor
accessors
equals()
hashCode()
toString()


Record:

→ implicitly final
→ extends Record
→ can implement interfaces


========================================================


SEALED

sealed
→ controlled inheritance

permits
→ allowed direct types

final
→ stop inheritance

non-sealed
→ reopen inheritance


========================================================


PATTERN MATCHING

Traditional:

if (obj instanceof String) {

    String str =
        (String) obj;
}


Modern:

if (obj instanceof String str) {

    use(str);
}


========================================================


RECORD PATTERN

if (obj instanceof Point(int x, int y)) {

    use(x);
    use(y);
}


========================================================


SEALED + PATTERN

sealed interface Shape
    permits Circle, Rectangle


            ↓

          Shape
         /     \
     Circle   Rectangle
         \     /
          pattern
             ↓
        exhaustive
          switch


========================================================


IMPORTANT INTERVIEW DISTINCTIONS

filter
→ selects

map
→ transforms


Stream
→ processing pipeline

Collection
→ stores data


Optional
→ represents optional value

null
→ absence represented by null


orElse
→ eager

orElseGet
→ lazy


final
→ no inheritance

sealed
→ controlled inheritance

non-sealed
→ open inheritance


record
→ concise data carrier

class
→ general-purpose type


Pattern matching
→ type check + binding


========================================================
```

---

# 33. Final Mental Model

```text
                         MODERN JAVA
                              |
          +-------------------+-------------------+
          |                   |                   |
        JAVA 8             JAVA 9            JAVA 16+
          |                   |                   |
          |                   |                   |
      Lambda              Private             Records
      Stream               Interface          Pattern
      Optional             Methods            Matching
      Method Ref                             
      Date/Time                                  |
      Default                                    |
      Interface                                  |
          |                                      |
          +------------------+-------------------+
                             |
                           JAVA 17+
                             |
                       Sealed Classes
                             |
                             ↓
                           JAVA 21
                             |
              +--------------+--------------+
              |                             |
        Switch Patterns              Record Patterns
              |                             |
              +--------------+--------------+
                             |
                             ↓
                    Stronger Type Modeling


========================================================

THINK OF EACH FEATURE AS A PROBLEM SOLVER


Lambda
→ "Too much anonymous-class boilerplate."


Stream
→ "I need declarative data processing."


Optional
→ "This result may be absent."


Default Method
→ "I need to evolve an interface."


Date/Time API
→ "I need a better date/time model."


Record
→ "I need a simple data carrier."


Sealed Class
→ "Only these types should extend me."


Pattern Matching
→ "I need to check and use a type together."


Record Pattern
→ "I need to match and extract record data."


========================================================

MOST IMPORTANT CONNECTION


                Sealed Type
                     |
                     ↓
              Known Variants
                     |
                     ↓
            Pattern Matching
                     |
                     ↓
          Exhaustive Processing
                     |
                     ↓
              Safer Modeling


========================================================

INTERVIEW MEMORY MAP


Java 8
→ Lambda + Stream + Optional


Java 9
→ Private Interface Methods


Java 16
→ Records + instanceof Patterns


Java 17
→ Sealed Classes


Java 21
→ Switch Patterns + Record Patterns


========================================================

ONE-LINE REVISION


Java 8
→ Functional Programming


Java 9
→ Interface Encapsulation


Java 16
→ Data Modeling + Pattern Matching


Java 17
→ Controlled Inheritance


Java 21
→ Advanced Pattern Matching


========================================================

FINAL IDEA

Modern Java is not about replacing
the old Java language.

It is about making common Java
designs more concise, expressive,
type-safe, and easier to maintain.

========================================================
```

---

# 🏁 Final Revision Checklist

Before an interview, make sure you can explain these without notes:

- [ ] Lambda expressions
- [ ] Functional interfaces
- [ ] Method references
- [ ] Stream pipeline
- [ ] Intermediate vs terminal operations
- [ ] Lazy stream evaluation
- [ ] `map()` vs `filter()`
- [ ] `Optional`
- [ ] `orElse()` vs `orElseGet()`
- [ ] `LocalDate` vs `LocalDateTime`
- [ ] `ZonedDateTime`
- [ ] Default interface methods
- [ ] Static interface methods
- [ ] Private interface methods
- [ ] Records
- [ ] Record limitations
- [ ] Records and immutability
- [ ] Sealed classes
- [ ] `permits`
- [ ] `final` vs `sealed` vs `non-sealed`
- [ ] Pattern matching with `instanceof`
- [ ] Pattern variables
- [ ] Flow scoping
- [ ] Pattern matching with `switch`
- [ ] Pattern dominance
- [ ] Record patterns
- [ ] Sealed classes + exhaustive switch
- [ ] Java 8 / 9 / 16 / 17 / 21 feature timeline

---

