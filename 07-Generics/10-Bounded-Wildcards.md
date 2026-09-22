# 10 — Bounded Wildcards

> **Bounded wildcards restrict the types that `?` can represent. Java provides upper-bounded wildcards using `extends` and lower-bounded wildcards using `super`.**

---

# Table of Contents

- [1. Introduction](#1-introduction)
- [2. Why Bounded Wildcards](#2-why-bounded-wildcards)
- [3. What Is a Bound](#3-what-is-a-bound)
- [4. Types of Bounded Wildcards](#4-types-of-bounded-wildcards)
- [5. Upper-Bounded Wildcard](#5-upper-bounded-wildcard)
- [6. Lower-Bounded Wildcard](#6-lower-bounded-wildcard)
- [7. `? extends T`](#7--extends-t)
- [8. `? super T`](#8--super-t)
- [9. Reading From `? extends T`](#9-reading-from--extends-t)
- [10. Writing to `? extends T`](#10-writing-to--extends-t)
- [11. Reading From `? super T`](#11-reading-from--super-t)
- [12. Writing to `? super T`](#12-writing-to--super-t)
- [13. Upper Bound Example](#13-upper-bound-example)
- [14. Lower Bound Example](#14-lower-bound-example)
- [15. `T` vs `? extends T`](#15-t-vs--extends-t)
- [16. `T` vs `? super T`](#16-t-vs--super-t)
- [17. Multiple Bounds](#17-multiple-bounds)
- [18. Bounded Type Parameters vs Bounded Wildcards](#18-bounded-type-parameters-vs-bounded-wildcards)
- [19. Bounded Wildcards With Collections](#19-bounded-wildcards-with-collections)
- [20. Bounded Wildcards With Methods](#20-bounded-wildcards-with-methods)
- [21. Bounded Wildcards and PECS](#21-bounded-wildcards-and-pecs)
- [22. Invariance and Bounded Wildcards](#22-invariance-and-bounded-wildcards)
- [23. Internal Working](#23-internal-working)
- [24. DSA Patterns](#24-dsa-patterns)
- [25. Common Mistakes](#25-common-mistakes)
- [26. Interview Traps](#26-interview-traps)
- [27. Top 10 Interview Questions](#27-top-10-interview-questions)
- [28. 30-Second Interview Answer](#28-30-second-interview-answer)
- [29. Cheat Sheet](#29-cheat-sheet)

---

# 1. Introduction

A wildcard represents an unknown type:

    ?

A bounded wildcard puts a restriction on that unknown type.

Java provides two major bounded wildcard forms:

    ? extends T

and:

    ? super T

The basic idea is:

    ? extends T
           ↓
    T or a subtype of T


    ? super T
         ↓
    T or a superclass of T

---

# 2. Why Bounded Wildcards

Consider:

    List<Integer>
    List<Double>
    List<Float>

All these element types are subclasses of:

    Number

But:

    List<Integer>

is NOT a subtype of:

    List<Number>

This is because Java Generics are invariant.

Therefore:

    List<Integer> integers = new ArrayList<>();

    List<Number> numbers = integers;

does not compile.

But if we only want to read numbers, we can use:

    List<? extends Number>

Example:

    public static void printNumbers(
            List<? extends Number> numbers) {

        for (Number number : numbers) {
            System.out.println(number);
        }
    }

Now all of these can be passed:

    List<Integer>
    List<Double>
    List<Float>
    List<Number>

Bounded wildcards provide controlled flexibility while maintaining type safety.

---

# 3. What Is a Bound

A bound restricts the types that are allowed.

For example:

    ? extends Number

means:

    Number
    OR
    any subtype of Number

Visual:

    Number
      ▲
      │
    Integer
    Double
    Float

Therefore:

    List<? extends Number>

can represent:

    List<Number>
    List<Integer>
    List<Double>
    List<Float>

---

## Lower Bound

Consider:

    ? super Integer

This means:

    Integer
    OR
    any superclass of Integer

Possible types include:

    Integer
    Number
    Object

Therefore:

    List<? super Integer>

can represent:

    List<Integer>
    List<Number>
    List<Object>

---

# 4. Types of Bounded Wildcards

There are two major types:

    Bounded Wildcards
           │
           ├── Upper Bound
           │      │
           │      └── ? extends T
           │
           └── Lower Bound
                  │
                  └── ? super T

---

## Upper Bound

Syntax:

    ? extends T

Meaning:

    T or subtype of T

Main use:

    Reading / Producer

---

## Lower Bound

Syntax:

    ? super T

Meaning:

    T or superclass of T

Main use:

    Writing / Consumer

---

# 5. Upper-Bounded Wildcard

An upper-bounded wildcard uses:

    extends

Syntax:

    ? extends T

Example:

    List<? extends Number>

This means:

    List of Number
    OR
    List of a subclass of Number

Possible:

    List<Integer>
    List<Double>
    List<Float>
    List<Number>

---

## Why Is It Called Upper Bound?

Because the specified type is the upper limit of the allowed type hierarchy.

For:

    ? extends Number

the bound is:

    Number

The actual type can be:

    Number
    Integer
    Double
    Float

but cannot be:

    String

---

# 6. Lower-Bounded Wildcard

A lower-bounded wildcard uses:

    super

Syntax:

    ? super T

Example:

    List<? super Integer>

This means:

    List<Integer>
    OR
    List<Number>
    OR
    List<Object>

---

## Why Is It Called Lower Bound?

Because the specified type is the lower bound of the accepted hierarchy.

For:

    ? super Integer

the accepted type can be:

    Integer
    Number
    Object

but cannot be:

    Double

because Double is not a superclass of Integer.

---

# 7. `? extends T`

Consider:

    List<? extends Number>

The unknown type must be:

    Number

or a subtype of:

    Number

Examples:

    List<Integer>
    List<Double>
    List<Float>
    List<Number>

are valid.

But:

    List<String>

is not valid.

---

## Example

    public static void printNumbers(
            List<? extends Number> list) {

        for (Number number : list) {
            System.out.println(number);
        }
    }

Usage:

    List<Integer> integers =
            Arrays.asList(10, 20, 30);

    List<Double> doubles =
            Arrays.asList(10.5, 20.5);

    printNumbers(integers);
    printNumbers(doubles);

Both work.

---

## Why Can We Read as Number?

Because regardless of the actual unknown type, the compiler knows:

    ? extends Number

must be:

    Number
    or a subtype of Number

Therefore:

    Number number = list.get(0);

is safe.

---

# 8. `? super T`

Consider:

    List<? super Integer>

The unknown type must be:

    Integer

or a superclass of Integer.

Valid:

    List<Integer>
    List<Number>
    List<Object>

---

## Example

    public static void addNumbers(
            List<? super Integer> list) {

        list.add(10);
        list.add(20);
        list.add(30);
    }

Usage:

    List<Integer> integers =
            new ArrayList<>();

    List<Number> numbers =
            new ArrayList<>();

    List<Object> objects =
            new ArrayList<>();

    addNumbers(integers);
    addNumbers(numbers);
    addNumbers(objects);

All are valid.

---

## Why Can We Add Integer?

Because regardless of the actual type represented by:

    ? super Integer

the list is guaranteed to be able to hold an Integer.

If the actual list is:

    List<Integer>

Integer works.

If it is:

    List<Number>

Integer works because Integer extends Number.

If it is:

    List<Object>

Integer works because Integer extends Object.

Therefore:

    list.add(10);

is safe.

---

# 9. Reading From `? extends T`

Consider:

    List<? extends Number> list;

Reading is safe:

    Number number = list.get(0);

Why?

Because the actual type must be Number or a subtype.

For example:

    List<Integer>

returns Integer.

Since Integer is a Number:

    Number number = integerValue;

is valid.

---

## Example

    public static double sum(
            List<? extends Number> numbers) {

        double sum = 0;

        for (Number number : numbers) {
            sum += number.doubleValue();
        }

        return sum;
    }

This works with:

    List<Integer>
    List<Double>
    List<Float>
    List<Long>

---

## What Is the Retrieved Type?

The exact type is unknown.

Therefore:

    list.get(0)

can safely be assigned to:

    Number

but not necessarily to:

    Integer

Example:

    List<? extends Number> list = ...;

This is safe:

    Number value = list.get(0);

This is not generally safe:

    Integer value = list.get(0);

Why?

Because the actual list could be:

    List<Double>

---

# 10. Writing to `? extends T`

This is one of the most important restrictions.

Consider:

    List<? extends Number> list;

You might think we can add any Number:

    list.add(10);

But this is NOT allowed.

Why?

Because the actual list could be:

    List<Double>

If Java allowed:

    list.add(10);

then an Integer could be inserted into:

    List<Double>

which would violate type safety.

Therefore:

    list.add(10);

does not compile.

---

## What Can Be Added?

Only:

    null

can generally be added.

Example:

    list.add(null);

This is allowed because `null` is compatible with all reference types.

---

## Key Rule

For:

    ? extends T

remember:

    READ → YES

    WRITE → NO

---

# 11. Reading From `? super T`

Consider:

    List<? super Integer> list;

Can we read from it?

Yes, but the exact type is unknown.

The safest type is:

    Object

Example:

    Object value = list.get(0);

---

## Why Not Integer?

Because the actual list could be:

    List<Number>

or:

    List<Object>

For example:

    List<Object> objects =
            new ArrayList<>();

    objects.add("Hello");

If:

    List<? super Integer> list = objects;

then:

    list.get(0)

could return:

    String

Therefore this is unsafe:

    Integer value = list.get(0);

But this is safe:

    Object value = list.get(0);

---

# 12. Writing to `? super T`

Writing is safe.

Example:

    List<? super Integer> list;

We can do:

    list.add(10);
    list.add(20);
    list.add(30);

Because the list is guaranteed to be able to hold an Integer.

It could be:

    List<Integer>

or:

    List<Number>

or:

    List<Object>

All can store an Integer.

---

## Key Rule

For:

    ? super T

remember:

    READ → Object

    WRITE → T

---

# 13. Upper Bound Example

Let's create a reusable method.

    public static double calculateSum(
            List<? extends Number> numbers) {

        double sum = 0;

        for (Number number : numbers) {
            sum += number.doubleValue();
        }

        return sum;
    }

Usage:

    List<Integer> integers =
            Arrays.asList(10, 20, 30);

    List<Double> doubles =
            Arrays.asList(1.5, 2.5, 3.5);

    System.out.println(
            calculateSum(integers)
    );

    System.out.println(
            calculateSum(doubles)
    );

This works because both Integer and Double extend Number.

---

# 14. Lower Bound Example

Suppose we want a method that adds Integers.

    public static void addNumbers(
            List<? super Integer> list) {

        list.add(10);
        list.add(20);
        list.add(30);
    }

We can pass:

    List<Integer>
    List<Number>
    List<Object>

Example:

    List<Integer> integers =
            new ArrayList<>();

    List<Number> numbers =
            new ArrayList<>();

    List<Object> objects =
            new ArrayList<>();

    addNumbers(integers);
    addNumbers(numbers);
    addNumbers(objects);

All are valid.

---

# 15. `T` vs `? extends T`

These are different concepts.

## `T`

    List<T>

means:

    List of the named type T

---

## `? extends T`

    List<? extends T>

means:

    List of T
    OR
    List of a subtype of T

---

## Example

    <T extends Number>

and:

    List<? extends Number>

are not identical.

The first declares a type parameter.

The second uses a wildcard.

---

## Important

This:

    <T extends Number>

means:

    "Declare a type T whose upper bound is Number."

While:

    ? extends Number

means:

    "Some unknown type that extends Number."

---

# 16. `T` vs `? super T`

Consider:

    <T>

This declares a named type.

Example:

    public static <T> void process(T value) {
    }

Now:

    ? super T

is a wildcard bound.

Example:

    List<? super T>

It means:

    List of T
    or
    List of a superclass of T

---

## Important

This is valid:

    <T extends Number>

This is valid:

    List<? super Integer>

But this is invalid:

    <T super Number>

Type parameters support:

    extends

for bounds.

Wildcards support:

    extends
    super

for bounds.

---

# 17. Multiple Bounds

A type parameter can have multiple bounds.

Example:

    <T extends Number & Comparable<T>>

Here `T` must:

    1. Extend Number
    2. Implement Comparable<T>

---

## Syntax

    <T extends ClassName & Interface1 & Interface2>

The class, if present, must come first.

Valid:

    <T extends Number & Comparable<T>>

Invalid:

    <T extends Comparable<T> & Number>

because the class bound must appear before interface bounds.

---

## Example

    public static <T extends Number & Comparable<T>>
    T max(T a, T b) {

        return a.compareTo(b) > 0 ? a : b;
    }

The method can use both:

    Number

and:

    Comparable<T>

functionality.

---

# 18. Bounded Type Parameters vs Bounded Wildcards

These are often confused.

## Bounded Type Parameter

    <T extends Number>

Here we declare:

    T

and restrict it.

Example:

    public static <T extends Number>
    void print(T value) {

        System.out.println(
                value.doubleValue()
        );
    }

---

## Bounded Wildcard

    ? extends Number

Here we do not name the unknown type.

Example:

    public static void print(
            List<? extends Number> list) {

        for (Number number : list) {
            System.out.println(number);
        }
    }

---

## Comparison

| Bounded Type Parameter | Bounded Wildcard |
|---|---|
| `<T extends Number>` | `? extends Number` |
| Names the type | Does not name the type |
| Type can be reused | Unknown type is not directly named |
| Useful for type relationships | Useful for flexible method parameters |
| Declares a type parameter | Uses a wildcard |

---

# 19. Bounded Wildcards With Collections

Bounded wildcards are heavily used with Java Collections.

Common examples:

    List<? extends Number>

    List<? super Integer>

    Collection<? extends T>

    Collection<? super T>

    Iterable<? extends T>

---

## Example: Reading

    public static void print(
            Collection<? extends Number> values) {

        for (Number value : values) {
            System.out.println(value);
        }
    }

---

## Example: Writing

    public static void add(
            Collection<? super Integer> values) {

        values.add(100);
        values.add(200);
    }

---

## Real Java API Example

A classic example is:

    Collections.copy(destination, source);

Conceptually, its generic signature is:

    copy(
        List<? super T> dest,
        List<? extends T> src
    )

This demonstrates the power of bounded wildcards.

---

# 20. Bounded Wildcards With Methods

Suppose we have:

    class Animal {
    }

    class Dog extends Animal {
    }

    class Cat extends Animal {
    }

We can create:

    List<Dog> dogs = new ArrayList<>();

    List<Cat> cats = new ArrayList<>();

    List<Animal> animals = new ArrayList<>();

---

## Upper Bound

    public static void processAnimals(
            List<? extends Animal> animals) {

        for (Animal animal : animals) {
            System.out.println(animal);
        }
    }

We can pass:

    processAnimals(dogs);

    processAnimals(cats);

    processAnimals(animals);

---

## Lower Bound

    public static void addDog(
            List<? super Dog> animals) {

        animals.add(new Dog());
    }

We can pass:

    List<Dog>

    List<Animal>

    List<Object>

---

# 21. Bounded Wildcards and PECS

PECS stands for:

> **Producer Extends, Consumer Super**

This is one of the most important rules in Java Generics.

---

## Producer

If a collection produces values for us:

    ? extends T

We mainly:

    READ

Therefore:

    Producer → Extends

---

## Consumer

If a collection consumes values from us:

    ? super T

We mainly:

    WRITE

Therefore:

    Consumer → Super

---

## Memory Trick

    Producer → Extends → READ

    Consumer → Super → WRITE

---

## Example

Source produces values:

    List<? extends T> source

Destination consumes values:

    List<? super T> destination

Therefore:

    source → produces T

    destination → consumes T

---

# 22. Invariance and Bounded Wildcards

Java Generics are invariant.

If:

    Dog extends Animal

it does NOT mean:

    List<Dog> extends List<Animal>

Therefore:

    List<Dog> dogs = new ArrayList<>();

    List<Animal> animals = dogs;

does not compile.

---

## Why?

Imagine Java allowed:

    animals.add(new Cat());

Then the same underlying list would contain:

    Dog
    Cat

But it was originally:

    List<Dog>

This would break type safety.

Therefore Java does not allow the assignment.

---

## Bounded Wildcard Solution

If we only want to read animals:

    List<? extends Animal>

Now:

    List<Dog>

can be passed safely.

Example:

    public static void printAnimals(
            List<? extends Animal> animals) {

        for (Animal animal : animals) {
            System.out.println(animal);
        }
    }

---

# 23. Internal Working

Bounded wildcards are primarily a compile-time type-safety mechanism.

Consider:

    List<? extends Number>

The compiler understands that the unknown element type is some subtype of Number.

It therefore allows:

    Number value = list.get(0);

But it rejects:

    list.add(10);

because the actual list could be:

    List<Double>

---

## Wildcard Capture

Internally, the compiler can capture a wildcard as an unknown type.

For example:

    List<?> list;

can conceptually be treated as:

    List<CAP>

where:

    CAP = unknown captured type

For:

    List<? extends Number>

the captured type satisfies:

    CAP extends Number

For:

    List<? super Integer>

the captured type has a lower-bound relationship with Integer.

The exact capture representation is compiler-internal, but the important concept is:

> The wildcard represents an unknown type that the compiler tracks for type safety.

---

## Runtime

Generic type information is mainly used at compile time.

Java implements generics using:

    Type Erasure

Therefore bounded wildcard information is primarily used by the compiler for:

    Type checking
    Type safety
    Method applicability
    Assignment compatibility

Detailed runtime behavior is covered in:

    11-Type-Erasure.md

---

# 24. DSA Patterns

Bounded wildcards are not themselves an algorithmic pattern, but they are useful when building reusable generic data structures and utility methods.

---

## Pattern 1 — Generic Search

    public static <T> int search(
            List<? extends T> list,
            T target) {

        for (int i = 0; i < list.size(); i++) {

            if (list.get(i).equals(target)) {
                return i;
            }
        }

        return -1;
    }

The list produces values compatible with `T`.

---

## Pattern 2 — Generic Copy

    public static <T> void copy(
            List<? super T> destination,
            List<? extends T> source) {

        for (T value : source) {
            destination.add(value);
        }
    }

This is a classic example of:

    Producer → ? extends T

    Consumer → ? super T

---

## Pattern 3 — Generic Stack

    class Stack<T> {

        private List<T> data =
                new ArrayList<>();

        public void push(T value) {
            data.add(value);
        }

        public T pop() {
            return data.remove(data.size() - 1);
        }
    }

Usage:

    Stack<Integer> stack =
            new Stack<>();

    stack.push(10);
    stack.push(20);

    int value = stack.pop();

---

## Pattern 4 — Generic Queue

    class Queue<T> {

        private List<T> data =
                new ArrayList<>();

        public void add(T value) {
            data.add(value);
        }

        public T remove() {
            return data.remove(0);
        }
    }

Usage:

    Queue<String> queue =
            new Queue<>();

    queue.add("A");
    queue.add("B");

---

## Problem-Solving Pattern

When designing a generic method, ask:

    1. Am I mainly reading?
           ↓
       ? extends T

    2. Am I mainly writing?
           ↓
       ? super T

    3. Do I need to name and reuse the type?
           ↓
       T

---

# 25. Common Mistakes

## Mistake 1 — Thinking `? extends Number` Allows Writing

This is invalid:

    List<? extends Number> list = ...;

    list.add(10);

Because the actual list could be:

    List<Double>

---

## Mistake 2 — Thinking `? super Integer` Returns Integer

This is unsafe:

    Integer value = list.get(0);

The list could actually be:

    List<Object>

Therefore the safe type is:

    Object value = list.get(0);

---

## Mistake 3 — Confusing `extends` With Class Inheritance Only

In generic wildcards:

    ? extends Number

means:

    Number or subtype

It is a generic type bound.

---

## Mistake 4 — Using `super` With a Type Parameter

Invalid:

    <T super Integer>

Valid:

    List<? super Integer>

---

## Mistake 5 — Thinking `List<Integer>` Is a `List<Number>`

It is not.

Java Generics are invariant.

Use:

    List<? extends Number>

when you need read-only flexibility across Number subtypes.

---

## Mistake 6 — Forgetting PECS

Remember:

    Producer → Extends

    Consumer → Super

---

# 26. Interview Traps

### Trap 1

What can you safely read from:

    List<? extends Number>

Answer:

    Number

---

### Trap 2

What can you safely add to:

    List<? extends Number>

Answer:

    Only null.

---

### Trap 3

What can you safely add to:

    List<? super Integer>

Answer:

    Integer and its subtypes.

---

### Trap 4

What can you safely read from:

    List<? super Integer>

Answer:

    Object

---

### Trap 5

Is this valid?

    <T super Number>

No.

---

### Trap 6

Is this valid?

    <T extends Number>

Yes.

---

### Trap 7

Can `List<Integer>` be passed to:

    List<? extends Number>

Yes.

---

### Trap 8

Can `List<Number>` be passed to:

    List<? extends Integer>

No.

Number is not a subtype of Integer.

---

### Trap 9

Can `List<Object>` be passed to:

    List<? super Integer>

Yes.

Object is a superclass of Integer.

---

### Trap 10

Can `List<Double>` be passed to:

    List<? super Integer>

No.

Double and Integer are sibling classes; Double is not a superclass of Integer.

---

# 27. Top 10 Interview Questions

## Q1. What is a bounded wildcard?

A bounded wildcard restricts the unknown type represented by `?`.

The two forms are:

    ? extends T

and:

    ? super T

---

## Q2. What is an upper-bounded wildcard?

An upper-bounded wildcard uses `extends`.

Example:

    List<? extends Number>

It accepts Number or any subtype of Number.

---

## Q3. What is a lower-bounded wildcard?

A lower-bounded wildcard uses `super`.

Example:

    List<? super Integer>

It accepts Integer or any superclass of Integer.

---

## Q4. Why can't we add elements to `List<? extends Number>`?

Because the actual list could be a list of a specific subtype such as:

    List<Double>

Adding an Integer would violate type safety.

---

## Q5. Why can we add Integer to `List<? super Integer>`?

Because the actual list is guaranteed to be:

    List<Integer>
    List<Number>
    or
    List<Object>

All can store an Integer.

---

## Q6. What is PECS?

PECS means:

    Producer Extends
    Consumer Super

Use `extends` for producers and `super` for consumers.

---

## Q7. What is the difference between `<T extends Number>` and `<? extends Number>`?

`<T extends Number>` declares a named type parameter.

`? extends Number` represents an unknown type bounded by Number.

---

## Q8. What can you read from `List<? super Integer>`?

Only safely as:

    Object

because the actual list could be `List<Object>`.

---

## Q9. What can you read from `List<? extends Number>`?

You can safely read values as:

    Number

---

## Q10. Why are bounded wildcards useful?

They provide flexibility while preserving type safety, especially when working with collections and generic APIs.

---

# 28. 30-Second Interview Answer

> **Bounded wildcards restrict the unknown type represented by a wildcard. `? extends T` is an upper bound, meaning the type can be T or one of its subtypes, and it is mainly used for producers or reading. `? super T` is a lower bound, meaning the type can be T or one of its supertypes, and it is mainly used for consumers or writing. This is summarized by PECS: Producer Extends, Consumer Super.**

---

# 29. Cheat Sheet

## Basic Forms

    ? extends T

Meaning:

    T or subtype of T

Main use:

    READ

---

    ? super T

Meaning:

    T or superclass of T

Main use:

    WRITE

---

## Read / Write Table

| Type | Read As | Write |
|---|---|---|
| `List<T>` | `T` | `T` |
| `List<?>` | `Object` | `null` only |
| `List<? extends T>` | `T` | `null` only |
| `List<? super T>` | `Object` | `T` and subtypes |

---

## PECS

    Producer
        ↓
    ? extends T
        ↓
      READ


    Consumer
        ↓
    ? super T
        ↓
      WRITE

---

## Hierarchy Example

    Number
      ▲
      │
    ┌─┼──────────┐
    │ │          │
 Integer      Double
    │
    └── subtype of Number


    ? extends Number
    ├── Number
    ├── Integer
    └── Double


    ? super Integer
    ├── Integer
    ├── Number
    └── Object

---

## T vs Bounded Wildcards

| Syntax | Meaning |
|---|---|
| `<T>` | Declare a type parameter |
| `<T extends Number>` | Declare T with upper bound |
| `?` | Unknown type |
| `? extends Number` | Unknown type that is Number or subtype |
| `? super Integer` | Unknown type that is Integer or superclass |

---

## Most Important Rules

    T
    ↓
    Named type parameter


    ?
    ↓
    Unknown type


    ? extends T
    ↓
    T or subtype
    ↓
    Producer
    ↓
    Read


    ? super T
    ↓
    T or superclass
    ↓
    Consumer
    ↓
    Write

---

## One-Line Memory Trick

> **`extends` gives flexibility for reading, `super` gives flexibility for writing.**

---

# Generics Progress

    01 — Generics Introduction
    02 — Why Generics
    03 — Generic Classes
    04 — Generic Methods
    05 — Type Safety
    06 — Raw Types
    07 — Wildcards
    08 — PECS
    09 — T vs ?
    10 — Bounded Wildcards
    11 — Type Erasure
    12 — Bridge Methods
    13 — Generics Interview Questions