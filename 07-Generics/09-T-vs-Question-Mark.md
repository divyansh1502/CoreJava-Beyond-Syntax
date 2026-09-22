# 09 — T vs ?

> **`T` and `?` are both used in Java Generics, but they solve different problems. `T` is a named type parameter, while `?` is a wildcard representing an unknown type.**

---

## Table of Contents

- [1. Introduction](#1-introduction)
- [2. What Is T?](#2-what-is-t)
- [3. What Is ?](#3-what-is-)
- [4. Basic Difference](#4-basic-difference)
- [5. T Is a Named Type](#5-t-is-a-named-type)
- [6. ? Is an Unknown Type](#6--is-an-unknown-type)
- [7. T With Generic Classes](#7-t-with-generic-classes)
- [8. T With Generic Methods](#8-t-with-generic-methods)
- [9. ? With Collections](#9--with-collections)
- [10. T vs ? in Method Parameters](#10-t-vs--in-method-parameters)
- [11. T vs ? in Return Types](#11-t-vs--in-return-types)
- [12. T Maintains Type Relationships](#12-t-maintains-type-relationships)
- [13. Multiple Uses of T](#13-multiple-uses-of-t)
- [14. Multiple Wildcards](#14-multiple-wildcards)
- [15. List<T> vs List<?>](#15-listt-vs-list)
- [16. List<T> vs List<? extends T>](#16-listt-vs-list-extends-t)
- [17. List<T> vs List<? super T>](#17-listt-vs-list-super-t)
- [18. T With extends](#18-t-with-extends)
- [19. T With super](#19-t-with-super)
- [20. When to Use T](#20-when-to-use-t)
- [21. When to Use ?](#21-when-to-use-)
- [22. Internal Working](#22-internal-working)
- [23. DSA Patterns](#23-dsa-patterns)
- [24. Common Mistakes](#24-common-mistakes)
- [25. Interview Traps](#25-interview-traps)
- [26. Top 10 Interview Questions](#26-top-10-interview-questions)
- [27. 30-Second Interview Answer](#27-30-second-interview-answer)
- [28. Cheat Sheet](#28-cheat-sheet)

---

# 1. Introduction

Java Generics commonly use:

    T

and:

    ?

They may look similar, but they have different purposes.

The easiest mental model is:

    T
    ↓
    A type with a name

    ?
    ↓
    An unknown type

Another way to remember:

    T = "I need to refer to this type."

    ? = "I don't need to know the exact type."

---

# 2. What Is T?

`T` is a **type parameter**.

Example:

    class Box<T> {

        T value;

        public void set(T value) {
            this.value = value;
        }

        public T get() {
            return value;
        }
    }

Here:

    T

represents a type that will be supplied later.

Example:

    Box<String> box = new Box<>();

Now:

    T → String

So conceptually:

    T value;

becomes:

    String value;

Another example:

    Box<Integer> box = new Box<>();

Now:

    T → Integer

Therefore:

    T value;

represents:

    Integer value;

---

# 3. What Is ?

`?` is a **wildcard**.

It represents an unknown type.

Example:

    List<?> list;

This means:

    List of some unknown type

The actual list could be:

    List<String>
    List<Integer>
    List<Double>
    List<Employee>

Important:

    ? does NOT mean Object.

It means:

    Some unknown type.

---

# 4. Basic Difference

| `T` | `?` |
|---|---|
| Type parameter | Wildcard |
| Gives a type a name | Represents an unknown type |
| Can be referred to repeatedly | Cannot directly name the unknown type |
| Useful for maintaining relationships | Useful when exact type is not important |
| Declared using `<T>` | Written as `?` |
| Can be used as a return type | Usually used inside parameterized types |
| Can connect multiple positions | Represents an unknown type at that position |

---

## Memory Trick

    T
    ↓
    TYPE WITH A NAME

    ?
    ↓
    UNKNOWN TYPE

---

# 5. T Is a Named Type

Consider:

    public static <T> T identity(T value) {
        return value;
    }

Here `T` is a named type parameter.

The method says:

    Input type  = T
    Return type = T

Therefore the same type is maintained.

Example:

    String name = identity("Java");

The compiler infers:

    T → String

Therefore:

    Input  → String
    Return → String

Another example:

    Integer number = identity(100);

Now:

    T → Integer

Therefore:

    Input  → Integer
    Return → Integer

---

# 6. ? Is an Unknown Type

Consider:

    public static void print(List<?> list) {

        for(Object value : list) {
            System.out.println(value);
        }
    }

The method doesn't need to know the exact element type.

It only needs to know:

    There is some element type.

That is why `?` is appropriate.

Possible arguments:

    List<String> names = new ArrayList<>();

    List<Integer> numbers = new ArrayList<>();

    List<Double> values = new ArrayList<>();

All can be passed to:

    print(...);

because all are compatible with:

    List<?>

---

# 7. T With Generic Classes

`T` is commonly used with generic classes.

Example:

    class Box<T> {

        private T value;

        public Box(T value) {
            this.value = value;
        }

        public T getValue() {
            return value;
        }
    }

String box:

    Box<String> box = new Box<>("Java");

    String value = box.getValue();

Here:

    T → String

Integer box:

    Box<Integer> box = new Box<>(100);

    Integer value = box.getValue();

Here:

    T → Integer

Why use `T`?

Because the class needs to refer to the same type in multiple places:

    constructor parameter
            ↓
            T

    field
            ↓
            T

    return type
            ↓
            T

---

# 8. T With Generic Methods

A generic method can declare its own type parameter.

Example:

    public static <T> T identity(T value) {
        return value;
    }

The syntax:

    <T>

before the return type declares the type parameter.

Another example:

    public static <T> void print(T value) {
        System.out.println(value);
    }

Usage:

    print("Java");

    print(100);

    print(10.5);

Here the compiler determines the appropriate type for `T`.

---

# 9. ? With Collections

Wildcard syntax is especially common with collections.

Example:

    public static void print(List<?> list) {

        for(Object value : list) {
            System.out.println(value);
        }
    }

This method can accept:

    List<String>

    List<Integer>

    List<Double>

    List<Employee>

The method does not need to know the exact element type.

---

## Why Object?

Because every Java reference type ultimately extends:

    Object

Therefore the safest type to retrieve from:

    List<?>

is:

    Object

Example:

    Object value = list.get(0);

---

# 10. T vs ? in Method Parameters

Consider:

    public static <T> void method(List<T> list) {
    }

Here `T` is a named type parameter.

Now:

    public static void method(List<?> list) {
    }

Here `?` represents an unknown type.

The important difference is whether the method needs to **name and reuse the type**.

---

## Using T

    public static <T> T getFirst(List<T> list) {
        return list.get(0);
    }

The method preserves the type.

If input is:

    List<String>

return type is:

    String

If input is:

    List<Integer>

return type is:

    Integer

---

## Using ?

    public static Object getFirst(List<?> list) {
        return list.get(0);
    }

The return type is safely:

    Object

because the exact element type is unknown.

---

# 11. T vs ? in Return Types

`T` can directly represent a meaningful return type.

Example:

    public static <T> T getValue(T value) {
        return value;
    }

Usage:

    String s = getValue("Hello");

    Integer i = getValue(100);

The return type is connected to the input type.

---

A wildcard can also appear in a return type:

    public static List<?> getList() {
        return new ArrayList<String>();
    }

But now the caller only knows:

    List of some unknown type

For example:

    List<?> list = getList();

The exact element type is not directly available to the caller.

Therefore, when a method needs to preserve a type relationship, `T` is usually more appropriate.

---

# 12. T Maintains Type Relationships

One of the biggest advantages of `T` is that it can connect multiple parts of an API.

Example:

    public static <T> T first(T a, T b) {
        return a;
    }

Here the same `T` is used for:

    a
    b
    return value

Another example:

    public static <T> T getFirst(List<T> list) {
        return list.get(0);
    }

If:

    List<Integer>

is passed:

    T → Integer

Therefore:

    return type → Integer

This is a type relationship.

---

# 13. Multiple Uses of T

Consider:

    public static <T> void compare(T first, T second) {

        System.out.println(first);
        System.out.println(second);
    }

Both parameters use the same named type parameter:

    T

This allows the method to express a relationship between them.

---

## More Important Example

    public static <T> void copy(
            List<? super T> destination,
            List<? extends T> source) {

        for(T value : source) {
            destination.add(value);
        }
    }

Here `T` connects:

    source
       ↓
       T
       ↓
    destination

The source produces `T`.

The destination consumes `T`.

This is a classic combination of:

    T
    +
    ? extends T
    +
    ? super T

---

# 14. Multiple Wildcards

Consider:

    Map<?, ?> map;

The two `?` represent independent unknown types.

For example:

    Map<String, Integer>

can be assigned to:

    Map<?, ?> map;

The first wildcard represents the unknown key type.

The second wildcard represents the unknown value type.

Conceptually:

    Map<
        unknown key type,
        unknown value type
    >

The two unknown types do not have to be the same.

---

## Important

This:

    Map<?, ?>

does NOT mean:

    Map<T, T>

Instead:

    Map<?, ?>

means:

    Map<unknown, unknown>

where each wildcard can represent a different type.

---

# 15. List<T> vs List<?>

These are different.

## List<T>

    List<T>

means:

    List containing the named type T

Example:

    public static <T> void method(List<T> list) {
        T value = list.get(0);
    }

Here the type is named and can be reused.

---

## List<?>

    List<?>

means:

    List containing some unknown type

Example:

    public static void method(List<?> list) {
        Object value = list.get(0);
    }

Here the exact type does not matter.

---

## Comparison

    List<T>
        ↓
    Named type


    List<?>
        ↓
    Unknown type

---

# 16. List<T> vs List<? extends T>

These are also different.

## List<T>

    List<T>

means the list contains exactly the type represented by `T`.

---

## List<? extends T>

    List<? extends T>

means the list contains:

    T
    OR
    a subtype of T

Example:

    List<? extends Number>

can represent:

    List<Integer>
    List<Double>
    List<Float>
    List<Number>

---

## Visual

    List<T>
       ↓
    exactly T


    List<? extends T>
       ↓
    T or subtype of T

---

## Example

    public static <T> void read(
            List<? extends T> list) {

        T value = list.get(0);

        System.out.println(value);
    }

If:

    T = Number

then:

    List<Integer>
    List<Double>
    List<Float>
    List<Number>

can be used.

---

# 17. List<T> vs List<? super T>

Again, these are different.

## List<T>

    List<T>

means:

    exactly T

---

## List<? super T>

    List<? super T>

means:

    T
    OR
    a superclass of T

Example:

    List<? super Integer>

can represent:

    List<Integer>
    List<Number>
    List<Object>

---

## Visual

    List<T>
       ↓
    exactly T


    List<? super T>
       ↓
    T or superclass of T

---

## Example

    public static <T> void write(
            List<? super T> list,
            T value) {

        list.add(value);
    }

If:

    T = Integer

then:

    List<Integer>
    List<Number>
    List<Object>

can accept the Integer.

---

# 18. T With extends

`T` itself can have an upper bound.

Example:

    public static <T extends Number> void print(T value) {
        System.out.println(value.doubleValue());
    }

Now `T` cannot be any arbitrary type.

It must be:

    Number
    or
    a subtype of Number

Valid:

    print(10);

    print(10.5);

    print(10.5f);

because:

    Integer extends Number
    Double extends Number
    Float extends Number

---

## Syntax

    <T extends Bound>

Example:

    <T extends Number>

---

## Multiple Bounds

A type parameter can have multiple bounds.

Example:

    <T extends Number & Comparable<T>>

This means `T` must satisfy both bounds.

The class bound, if present, must come first.

Example:

    <T extends SomeClass & Interface1 & Interface2>

---

# 19. T With super

A type parameter itself cannot be declared using:

    <T super Number>

This is invalid Java syntax.

`super` is used with wildcards:

    ? super T

For example:

    List<? super Integer>

is valid.

But:

    <T super Integer>

is not valid.

---

## Important Difference

Valid:

    <T extends Number>

Valid:

    List<? super Integer>

Invalid:

    <T super Integer>

This is an important interview point.

---

# 20. When to Use T

Use `T` when you need to **name and reuse a type**.

Common situations:

### 1. Return type related to input

    public static <T> T identity(T value) {
        return value;
    }

---

### 2. Multiple parameters share a type

    public static <T> void compare(T a, T b) {
    }

---

### 3. Generic classes

    class Box<T> {
        T value;
    }

---

### 4. Generic methods

    public static <T> T getValue(T value) {
        return value;
    }

---

### 5. Connecting source and destination

    public static <T> void copy(
            List<? super T> destination,
            List<? extends T> source) {
    }

---

# 21. When to Use ?

Use `?` when the exact type is not important.

Example:

    public static void print(List<?> list) {

        for(Object value : list) {
            System.out.println(value);
        }
    }

The method doesn't care whether the list contains:

    String
    Integer
    Double
    Employee

It only needs to read and print the values.

---

## Use `? extends`

When the object is a producer.

    List<? extends Number>

Main operation:

    READ

---

## Use `? super`

When the object is a consumer.

    List<? super Integer>

Main operation:

    WRITE

This follows:

    PECS

    Producer Extends
    Consumer Super

---

# 22. Internal Working

Both `T` and `?` are part of Java's compile-time generic type system.

Consider:

    List<T>

`T` is a type parameter.

The compiler tracks the type represented by `T`.

Example:

    List<String>

Conceptually:

    T → String

---

## Wildcard

Consider:

    List<?>

The compiler knows that there is a specific element type, but that type is unknown to the code using the wildcard.

Conceptually:

    ? → unknown captured type

Java internally performs wildcard capture when necessary.

For example:

    List<?> list;

The compiler can conceptually treat the unknown type as something like:

    CAP#1

where:

    CAP#1 = some unknown type

The exact compiler representation is an implementation detail, but the important concept is **capture of the unknown wildcard type**.

---

## Type Erasure

Generic type information is mainly used by the compiler for type checking.

Java uses:

    Type Erasure

to implement generics at runtime.

Detailed type erasure is covered in:

    11-Type-Erasure.md

---

# 23. DSA Patterns

Generics are not usually a direct algorithmic pattern, but understanding `T` and `?` helps when implementing reusable data structures.

---

## Pattern 1 — Generic Stack

    class Stack<T> {

        private List<T> elements =
                new ArrayList<>();

        public void push(T value) {
            elements.add(value);
        }

        public T pop() {
            return elements.remove(elements.size() - 1);
        }
    }

Usage:

    Stack<Integer> stack = new Stack<>();

    stack.push(10);
    stack.push(20);

    Integer value = stack.pop();

Here:

    T → Integer

---

## Pattern 2 — Generic Queue

    class Queue<T> {

        private List<T> elements =
                new ArrayList<>();

        public void add(T value) {
            elements.add(value);
        }

        public T remove() {
            return elements.remove(0);
        }
    }

Usage:

    Queue<String> queue = new Queue<>();

    queue.add("A");
    queue.add("B");

---

## Pattern 3 — Generic Search

    public static <T> int find(
            List<T> list,
            T target) {

        for(int i = 0; i < list.size(); i++) {

            if(list.get(i).equals(target)) {
                return i;
            }
        }

        return -1;
    }

This maintains the relationship:

    List<T>
       +
    target T

---

## Pattern 4 — Generic Copy

    public static <T> void copy(
            List<? super T> destination,
            List<? extends T> source) {

        for(T value : source) {
            destination.add(value);
        }
    }

This combines:

    T
    ? extends T
    ? super T

and is a practical example of PECS.

---

# 24. Common Mistakes

## Mistake 1 — Thinking T and ? Are Identical

They are not.

    T
    ↓
    Named type parameter

    ?
    ↓
    Unknown type

---

## Mistake 2 — Thinking ? Can Be Used Everywhere T Can

This is invalid:

    public static ? getValue() {
    }

Instead use:

    public static <T> T getValue(T value) {
        return value;
    }

---

## Mistake 3 — Thinking List<T> and List<?> Are the Same

They are different.

    List<T>
        ↓
    Named type

    List<?>
        ↓
    Unknown type

---

## Mistake 4 — Thinking ? extends T Means Exactly T

It does not.

It means:

    T
    OR
    subtype of T

---

## Mistake 5 — Thinking ? super T Means Only the Parent

It can represent:

    T

or:

    any superclass of T

Example:

    List<? super Integer>

can represent:

    List<Integer>
    List<Number>
    List<Object>

---

## Mistake 6 — Trying to Use super With a Type Parameter Declaration

Invalid:

    <T super Number>

Valid:

    <T extends Number>

and:

    List<? super Number>

---

# 25. Interview Traps

## Trap 1

### Is `T` a keyword?

No.

`T` is simply a conventional name for a type parameter.

Other common names include:

    E
    K
    V
    N
    R
    S
    U

---

## Trap 2

### Is `?` a type parameter?

No.

It is a wildcard.

---

## Trap 3

### Can T be used as a return type?

Yes.

Example:

    public static <T> T get(T value) {
        return value;
    }

---

## Trap 4

### Can ? be used as a normal variable type?

No.

This is invalid:

    ? value;

---

## Trap 5

### What does List<T> mean?

A List whose element type is the named type `T`.

---

## Trap 6

### What does List<?> mean?

A List whose element type is some unknown type.

---

## Trap 7

### What does List<? extends T> mean?

A List whose element type is `T` or a subtype of `T`.

---

## Trap 8

### What does List<? super T> mean?

A List whose element type is `T` or a superclass of `T`.

---

## Trap 9

### Can `<T super Number>` be declared?

No.

Type parameters support upper bounds using `extends`.

Lower bounds are available for wildcards using `super`.

---

## Trap 10

### Why use T instead of ?

Use `T` when you need to name the type and establish a relationship between multiple parts of the API.

---

# 26. Top 10 Interview Questions

## Q1. What is T in Java Generics?

`T` is a type parameter used to represent a type that can be specified later.

---

## Q2. What is ? in Java Generics?

`?` is a wildcard representing an unknown type.

---

## Q3. What is the main difference between T and ?

`T` gives a type a name, while `?` represents an unknown type without naming it.

---

## Q4. Why would you use T instead of ?

Use `T` when you need to refer to the same type multiple times or maintain a relationship between inputs, outputs, or different generic components.

---

## Q5. Why would you use ? instead of T?

Use `?` when the exact type is not important for the operation.

---

## Q6. What is the difference between List<T> and List<?>?

    List<T>
        ↓
    List of the named type T

    List<?>
        ↓
    List of some unknown type

---

## Q7. What is List<? extends T>?

A List whose element type is `T` or a subtype of `T`.

---

## Q8. What is List<? super T>?

A List whose element type is `T` or a superclass of `T`.

---

## Q9. Can a wildcard maintain a type relationship like T?

A wildcard can express bounds, but it does not give the unknown type a reusable name. When a method needs to explicitly connect multiple positions to the same type, a type parameter is generally appropriate.

---

## Q10. Is T a Java keyword?

No.

It is a conventional name for a type parameter.

---

# 27. 30-Second Interview Answer

> **`T` and `?` are both used in Java Generics, but they have different purposes. `T` is a named type parameter, so we can refer to the same type multiple times and maintain relationships between inputs and outputs. `?` is a wildcard representing an unknown type, and it is useful when the exact type doesn't matter. For example, `List<T>` works with a named type, while `List<?>` means a List of some unknown type. Bounded wildcards such as `? extends T` and `? super T` provide controlled flexibility for reading and writing.**

---

# 28. Cheat Sheet

## Core Difference

    T
    ↓
    Named Type Parameter
    ↓
    "I need to refer to this type."


    ?
    ↓
    Wildcard
    ↓
    "I don't need to know the exact type."

---

## T

    <T>

Used when:

    Type needs a name
          ↓
    Type needs to be reused
          ↓
    Type relationship matters

Example:

    public static <T> T identity(T value) {
        return value;
    }

---

## ?

    ?

Used when:

    Exact type doesn't matter

Example:

    public static void print(List<?> list) {
    }

---

## ? extends T

    T or subtype

Example:

    List<? extends Number>

Use mainly for:

    READ

Producer:

    Producer → Extends

---

## ? super T

    T or superclass

Example:

    List<? super Integer>

Use mainly for:

    WRITE

Consumer:

    Consumer → Super

---

## Final Comparison

| Syntax | Meaning | Main Use |
|---|---|---|
| `<T>` | Declares a named type parameter | Define a generic type |
| `T` | The named type | Reuse the type |
| `<?>` | Unknown type | Exact type doesn't matter |
| `<? extends T>` | T or subtype | Producer / reading |
| `<? super T>` | T or superclass | Consumer / writing |
| `<T extends Number>` | T must be Number or subtype | Restrict a type parameter |

---

## T vs ?

```text
                    GENERICS
                       │
              ┌────────┴────────┐
              │                 │
              T                 ?
              │                 │
       Named type          Unknown type
              │                 │
       Reuse/relate       Exact type irrelevant
              │                 │
              │          ┌──────┴──────┐
              │          │             │
              │       extends        super
              │          │             │
              │       Producer       Consumer
              │          │             │
              │         READ          WRITE
              │
              ▼
        Type relationship