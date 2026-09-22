# 07 — Wildcards

> **A wildcard (`?`) represents an unknown type in Java Generics. Wildcards provide controlled flexibility when working with parameterized types, especially when a method needs to accept different generic types.**

---

## Table of Contents

- [1. What Is a Wildcard?](#1-what-is-a-wildcard)
- [2. Why Do We Need Wildcards?](#2-why-do-we-need-wildcards)
- [3. Basic Syntax](#3-basic-syntax)
- [4. Unbounded Wildcard](#4-unbounded-wildcard)
- [5. Upper-Bounded Wildcard](#5-upper-bounded-wildcard)
- [6. Lower-Bounded Wildcard](#6-lower-bounded-wildcard)
- [7. Wildcard vs Type Parameter](#7-wildcard-vs-type-parameter)
- [8. Reading From Wildcards](#8-reading-from-wildcards)
- [9. Writing to Wildcards](#9-writing-to-wildcards)
- [10. `List<?>`](#10-list)
- [11. `List<? extends Number>`](#11-list-extends-number)
- [12. `List<? super Integer>`](#12-list-super-integer)
- [13. Wildcards and Type Safety](#13-wildcards-and-type-safety)
- [14. Wildcards With Methods](#14-wildcards-with-methods)
- [15. Wildcards With Collections](#15-wildcards-with-collections)
- [16. Wildcards and Invariance](#16-wildcards-and-invariance)
- [17. Wildcard Capture](#17-wildcard-capture)
- [18. Wildcards vs Raw Types](#18-wildcards-vs-raw-types)
- [19. Wildcards in DSA](#19-wildcards-in-dsa)
- [20. Internal Working](#20-internal-working)
- [21. Important Rules](#21-important-rules)
- [22. Common Mistakes](#22-common-mistakes)
- [23. Interview Traps](#23-interview-traps)
- [24. Top 10 Interview Questions](#24-top-10-interview-questions)
- [25. 30-Second Interview Answer](#25-30-second-interview-answer)
- [26. Cheat Sheet](#26-cheat-sheet)

---

# 1. What Is a Wildcard?

A **wildcard** is represented by:

    ?

It represents an **unknown type**.

Example:

    List<?> list;

This means:

> `list` is a List of some unknown type.

The exact type is not specified.

It could be:

    List<String>

    List<Integer>

    List<Double>

    List<Employee>

etc.

---

## Important

A wildcard does **not** mean:

    Object

It means:

    Some unknown type

This distinction is extremely important.

---

# 2. Why Do We Need Wildcards?

Generics are invariant.

For example:

    List<String>

is not a subtype of:

    List<Object>

Even though:

    String extends Object

Therefore this is invalid:

    List<Object> list = new ArrayList<String>();

But sometimes we want a method to accept:

    List<String>

    List<Integer>

    List<Double>

    List<Employee>

without writing separate methods.

Wildcards provide this flexibility.

Example:

    public static void printList(List<?> list) {

        for(Object value : list) {
            System.out.println(value);
        }
    }

Now the method can accept different parameterized Lists.

---

# 3. Basic Syntax

The basic wildcard syntax is:

    ?

There are three major forms:

    1. Unbounded wildcard

       <?>


    2. Upper-bounded wildcard

       <? extends Type>


    3. Lower-bounded wildcard

       <? super Type>

---

## Quick Meaning

    <?>
        ↓
    Unknown type


    <? extends Number>
        ↓
    Number or subclass


    <? super Integer>
        ↓
    Integer or superclass

---

# 4. Unbounded Wildcard

An unbounded wildcard is:

    <?>

Example:

    List<?> list;

It means:

> List of an unknown type.

---

## Example

    List<String> names = new ArrayList<>();

    List<?> list = names;

This is valid.

Another:

    List<Integer> numbers = new ArrayList<>();

    List<?> list = numbers;

Also valid.

---

## Why?

Because:

    List<?>

can represent:

    List<String>
    List<Integer>
    List<Double>
    List<Employee>

and so on.

---

# 5. Upper-Bounded Wildcard

Syntax:

    <? extends Type>

Example:

    List<? extends Number>

This means:

> A List whose element type is `Number` or a subclass of `Number`.

Possible types:

    List<Integer>

    List<Double>

    List<Float>

    List<Long>

because:

    Integer extends Number
    Double extends Number
    Float extends Number
    Long extends Number

---

## Example

    public static void printNumbers(
            List<? extends Number> numbers) {

        for(Number number : numbers) {
            System.out.println(number);
        }
    }

Now we can pass:

    List<Integer>

or:

    List<Double>

or:

    List<Float>

---

## What Can We Read?

We can safely read values as:

    Number

because every allowed type is at least a `Number`.

---

# 6. Lower-Bounded Wildcard

Syntax:

    <? super Type>

Example:

    List<? super Integer>

This means:

> A List whose element type is `Integer` or a superclass of `Integer`.

Possible types include:

    List<Integer>

    List<Number>

    List<Object>

because:

    Integer extends Number
    Number extends Object

---

## Example

    public static void addNumbers(
            List<? super Integer> list) {

        list.add(10);
        list.add(20);
    }

This is safe because every possible list type can accept an `Integer`.

---

## What Can We Read?

When reading from:

    List<? super Integer>

the safest known type is:

    Object

Example:

    Object value = list.get(0);

We cannot safely assume:

    Integer value = list.get(0);

because the actual list could be:

    List<Object>

---

# 7. Wildcard vs Type Parameter

These concepts look similar but serve different purposes.

## Type Parameter

Example:

    public static <T> void print(T value)

Here `T` is a named type parameter.

The method can refer to the same type using:

    T

---

## Wildcard

Example:

    public static void print(List<?> list)

Here `?` represents an unknown type.

The method does not need to name that type.

---

## Comparison

| Type Parameter | Wildcard |
|---|---|
| `<T>` | `?` |
| Named type | Unknown type |
| Can refer to T repeatedly | Type is not named |
| Useful when types need relationships | Useful when exact type is irrelevant |
| Can have bounds | Can have bounds |

---

## Example

Type parameter:

    public static <T> void copy(T value) {
    }

Wildcard:

    public static void print(List<?> list) {
    }

---

# 8. Reading From Wildcards

Understanding what can be read is extremely important.

## Unbounded Wildcard

    List<?> list

We can safely read:

    Object value = list.get(0);

Why?

Because every Java reference type ultimately extends `Object`.

---

## Upper Bound

    List<? extends Number> list

We can safely read:

    Number value = list.get(0);

Because the unknown type is guaranteed to be a subtype of `Number`.

---

## Lower Bound

    List<? super Integer> list

We can safely read:

    Object value = list.get(0);

We cannot safely read:

    Integer value = list.get(0);

because the actual list could be:

    List<Object>

---

## Reading Cheat Sheet

    List<?> 
        ↓
    Read as Object


    List<? extends Number>
        ↓
    Read as Number


    List<? super Integer>
        ↓
    Read as Object

---

# 9. Writing to Wildcards

This is where wildcard behavior becomes important.

## `List<?>`

You generally cannot add a specific value.

Example:

    List<?> list;

    // list.add("Java"); ❌
    // list.add(100);    ❌

Why?

Because the exact type is unknown.

The list could actually be:

    List<Integer>

Adding a String would be unsafe.

---

## `List<? extends Number>`

You generally cannot add a specific Number.

Example:

    List<? extends Number> list;

    // list.add(10);     ❌
    // list.add(10.5);   ❌

Why?

The actual list might be:

    List<Integer>

Adding a Double would violate its type.

---

## `List<? super Integer>`

We can safely add an Integer.

    List<? super Integer> list;

    list.add(10);
    list.add(20);

Why?

Because the actual type must be one of:

    Integer
    Number
    Object

All of these can store an Integer.

---

## Writing Cheat Sheet

    List<?>
        ↓
    Cannot safely add a specific value


    List<? extends Number>
        ↓
    Cannot safely add Number values


    List<? super Integer>
        ↓
    Can safely add Integer

---

# 10. `List<?>`

`List<?>` means:

> List of some unknown type.

Example:

    public static void print(List<?> list) {

        for(Object value : list) {
            System.out.println(value);
        }
    }

Usage:

    List<String> names = List.of("Java", "Spring");

    List<Integer> numbers = List.of(10, 20);

    print(names);

    print(numbers);

The method works with both.

---

## Why Can't We Add?

Suppose:

    List<?> list = new ArrayList<String>();

If Java allowed:

    list.add(100);

then an Integer would enter a:

    List<String>

Therefore adding a specific value is not allowed.

---

# 11. `List<? extends Number>`

This is an upper-bounded wildcard.

Example:

    List<? extends Number> numbers;

It can refer to:

    List<Integer>

    List<Double>

    List<Float>

    List<Long>

---

## Example

    public static double sum(
            List<? extends Number> numbers) {

        double sum = 0;

        for(Number number : numbers) {
            sum += number.doubleValue();
        }

        return sum;
    }

Usage:

    List<Integer> integers = List.of(10, 20, 30);

    List<Double> doubles = List.of(10.5, 20.5);

    System.out.println(sum(integers));

    System.out.println(sum(doubles));

---

## Important Concept

With:

    <? extends Number>

we know:

    Unknown Type
          ↓
       extends
          ↓
       Number

Therefore every element can safely be treated as:

    Number

---

# 12. `List<? super Integer>`

This is a lower-bounded wildcard.

Example:

    List<? super Integer> list;

Possible actual types:

    List<Integer>

    List<Number>

    List<Object>

---

## Example

    public static void addValues(
            List<? super Integer> list) {

        list.add(10);
        list.add(20);
        list.add(30);
    }

Usage:

    List<Integer> integers = new ArrayList<>();

    addValues(integers);


    List<Number> numbers = new ArrayList<>();

    addValues(numbers);


    List<Object> objects = new ArrayList<>();

    addValues(objects);

All are valid.

---

## Why Is Adding Safe?

Because:

    Integer

can be stored in:

    Integer
    Number
    Object

Therefore:

    list.add(Integer)

is safe.

---

# 13. Wildcards and Type Safety

Wildcards provide flexibility without completely abandoning type safety.

Compare:

    List

Raw type:

    Type information is removed.

Versus:

    List<?>

Wildcard:

    Generic relationship is preserved.

---

## Example

    public static void print(List<?> list) {
    }

This method accepts:

    List<String>

    List<Integer>

    List<Double>

while still operating within the generic type system.

---

# 14. Wildcards With Methods

Wildcards are commonly used in method parameters.

---

## Example 1 — Unbounded

    public static void print(List<?> list) {

        for(Object value : list) {
            System.out.println(value);
        }
    }

---

## Example 2 — Upper Bound

    public static void printNumbers(
            List<? extends Number> list) {

        for(Number number : list) {
            System.out.println(number);
        }
    }

---

## Example 3 — Lower Bound

    public static void addIntegers(
            List<? super Integer> list) {

        list.add(10);
        list.add(20);
    }

---

# 15. Wildcards With Collections

Wildcards are frequently used with Java Collections.

---

## List

    List<?> list

---

## Set

    Set<?> set

---

## Map

Maps have two type parameters.

Example:

    Map<?, ?> map

means:

    Unknown key type
    Unknown value type

---

## Upper-Bounded Map

    Map<? extends Number, ? extends Number> map

Both keys and values are restricted to Number or subclasses.

---

## Lower-Bounded Map

    Map<? super Integer, ? super Integer> map

Both keys and values must be Integer or a superclass of Integer.

---

# 16. Wildcards and Invariance

Remember:

    List<String>

is not a subtype of:

    List<Object>

This is because generic types are invariant.

But:

    List<?> 

can refer to both.

Example:

    List<String> strings = new ArrayList<>();

    List<?> list = strings;

Valid.

Also:

    List<Integer> numbers = new ArrayList<>();

    List<?> list = numbers;

Valid.

---

## Mental Model

    List<String>
          │
          ├──────→ List<?>
          │
          └──────→ NOT List<Object>


    List<Integer>
          │
          ├──────→ List<?>
          │
          └──────→ NOT List<Object>

The wildcard provides controlled flexibility.

---

# 17. Wildcard Capture

When Java encounters:

    List<?> list

the `?` represents some specific but unknown type.

Internally, the compiler can conceptually treat it as a captured type.

Example:

    public static void print(List<?> list) {

        printInternal(list);
    }

Conceptually:

    ?
    ↓
    CAP#1

The compiler knows that the list has one consistent unknown type.

For example:

    List<String>

could be viewed conceptually as:

    List<CAP#1>

where:

    CAP#1 = String

You normally do not need to work directly with capture types, but understanding the concept helps explain why wildcard operations behave the way they do.

---

# 18. Wildcards vs Raw Types

This is a very important interview comparison.

## Raw Type

    List list

Means:

> Generic type information has been omitted.

---

## Wildcard

    List<?> list

Means:

> This is a generic List, but its exact type is unknown.

---

## Comparison

| Feature | Raw Type | Wildcard |
|---|---|---|
| Syntax | `List` | `List<?>` |
| Generic information | Omitted | Preserved |
| Type safety | Weaker | Stronger |
| Unchecked warnings | Possible | Generally avoids raw-type warnings |
| Recommended | Usually no | Yes when appropriate |
| Purpose | Legacy compatibility | Generic flexibility |

---

# 19. Wildcards in DSA

Wildcards are useful when designing reusable DSA utility methods.

---

## Printing Any List

    public static void printList(List<?> list) {

        for(Object value : list) {
            System.out.print(value + " ");
        }
    }

This works for:

    List<Integer>

    List<String>

    List<Double>

---

## Reading Numbers

    public static double sum(
            List<? extends Number> list) {

        double sum = 0;

        for(Number value : list) {
            sum += value.doubleValue();
        }

        return sum;
    }

---

## Adding Integers

    public static void addDefaults(
            List<? super Integer> list) {

        list.add(10);
        list.add(20);
        list.add(30);
    }

---

## DSA Mental Pattern

    Need to READ values?
            ↓
    <? extends T>


    Need to WRITE values?
            ↓
    <? super T>


    Need neither exact type nor modification?
            ↓
    <?>

This leads directly to the **PECS principle**.

Detailed PECS is covered in:

    08-PECS.md

---

# 20. Internal Working

Wildcards are primarily a compile-time feature of Java Generics.

Consider:

    List<? extends Number> list;

The compiler knows that the actual element type is some unknown subtype of:

    Number

Therefore:

    Number value = list.get(0);

is safe.

But:

    list.add(10);

is rejected because the compiler does not know whether the actual list is:

    List<Integer>

or:

    List<Double>

or:

    List<Float>

---

## Lower Bound

For:

    List<? super Integer>

the compiler knows:

    Actual type is Integer or a superclass of Integer.

Therefore:

    list.add(10);

is safe.

But when retrieving:

    Object value = list.get(0);

is the safest assumption because the actual type could be:

    Object

---

# 21. Important Rules

## Rule 1

`?` means unknown type.

---

## Rule 2

There are three major wildcard forms:

    <?>

    <? extends T>

    <? super T>

---

## Rule 3

Unbounded wildcard means:

    Unknown type

---

## Rule 4

Upper bound means:

    T or subtype of T

---

## Rule 5

Lower bound means:

    T or supertype of T

---

## Rule 6

From:

    List<?>

you can safely read as:

    Object

---

## Rule 7

From:

    List<? extends Number>

you can safely read as:

    Number

---

## Rule 8

From:

    List<? super Integer>

you can safely add:

    Integer

---

## Rule 9

You generally cannot add a specific value to:

    List<?>

or:

    List<? extends T>

---

## Rule 10

Wildcards provide flexibility while preserving generic type safety.

---

# 22. Common Mistakes

## Mistake 1 — Thinking `?` Means Object

Wrong:

    ? = Object

Correct:

    ? = Unknown type

---

## Mistake 2 — Adding to `List<?>`

Wrong:

    List<?> list = new ArrayList<String>();

    list.add("Java");

This is not allowed because the exact type is unknown.

---

## Mistake 3 — Adding to `List<? extends Number>`

Wrong:

    List<? extends Number> list = ...;

    list.add(10);

The actual list could be:

    List<Double>

---

## Mistake 4 — Reading `List<? super Integer>` as Integer

Wrong:

    Integer value = list.get(0);

Correct:

    Object value = list.get(0);

---

## Mistake 5 — Confusing `extends` With Class Inheritance Only

In:

    List<? extends Number>

`extends` means the unknown type is a subtype of `Number`.

It is a generic upper bound.

---

# 23. Interview Traps

### Trap 1

**What does `List<?>` mean?**

A List of some unknown type.

---

### Trap 2

**Can you add anything to `List<?>`?**

No, except `null`.

A specific value cannot safely be added because the actual element type is unknown.

---

### Trap 3

**Can you read from `List<?>`?**

Yes.

The value can safely be treated as:

    Object

---

### Trap 4

**Can you add Integer to `List<? extends Number>`?**

No.

The actual list might be:

    List<Double>

---

### Trap 5

**Can you add Integer to `List<? super Integer>`?**

Yes.

The actual list could be:

    List<Integer>
    List<Number>
    List<Object>

All can accept Integer.

---

### Trap 6

**What is the safest type when reading from `List<? super Integer>`?**

`Object`.

---

### Trap 7

**Is `List<?>` a raw type?**

No.

It is a parameterized type using an unbounded wildcard.

---

### Trap 8

**Does wildcard remove type safety?**

No.

Wildcards provide controlled flexibility while retaining generic type checking.

---

# 24. Top 10 Interview Questions

## Q1. What is a wildcard in Java?

A wildcard represented by `?` represents an unknown type in a generic type.

---

## Q2. What are the three types of wildcards?

    <?>

    <? extends T>

    <? super T>

---

## Q3. What does `List<?>` mean?

It means a List of some unknown type.

---

## Q4. What is an upper-bounded wildcard?

Example:

    <? extends Number>

It means the unknown type is `Number` or a subtype of `Number`.

---

## Q5. What is a lower-bounded wildcard?

Example:

    <? super Integer>

It means the unknown type is `Integer` or a superclass of `Integer`.

---

## Q6. Why can't we add values to `List<? extends Number>`?

Because the actual list could be a specific subtype such as `List<Integer>` or `List<Double>`, and the compiler cannot safely determine which type it is.

---

## Q7. Why can we add Integer to `List<? super Integer>`?

Because every possible type represented by the wildcard can accept an Integer:

    Integer
    Number
    Object

---

## Q8. What can we read from `List<? extends Number>`?

We can safely read values as:

    Number

---

## Q9. What can we read from `List<? super Integer>`?

The safest type is:

    Object

---

## Q10. What is the difference between `?` and `T`?

`T` is a named type parameter that can be referred to throughout the declaration.

`?` represents an unknown type without giving that type a name.

---

# 25. 30-Second Interview Answer

> **A wildcard in Java Generics is represented by `?` and represents an unknown type. There are three main forms: unbounded `<?>`, upper-bounded `<? extends T>`, and lower-bounded `<? super T>`. An unbounded wildcard is useful when the exact type does not matter, an upper bound is useful when we mainly need to read values as a specific parent type, and a lower bound is useful when we need to safely add values of a specific type. Wildcards provide flexibility while maintaining generic type safety.**

---

# 26. Cheat Sheet

    WILDCARDS
    │
    ├── ?
    │    └── Unknown type
    │
    ├── ? extends T
    │    └── T or subtype
    │
    └── ? super T
         └── T or supertype

---

## Read / Write Rule

    List<?>
        ↓
    READ → Object
    WRITE → ❌ specific values


    List<? extends Number>
        ↓
    READ → Number
    WRITE → ❌ specific values


    List<? super Integer>
        ↓
    READ → Object
    WRITE → Integer ✅

---

## Quick Examples

    List<?> list;


    List<? extends Number> numbers;


    List<? super Integer> integers;


---

## Wildcard Mental Model

    <?>
      ↓
    "I don't know the type."


    <? extends Number>
      ↓
    "I don't know the exact type,
     but it is a Number or subtype."


    <? super Integer>
      ↓
    "I don't know the exact type,
     but it is Integer or a superclass."

---

## Most Important Connection

    07 — Wildcards
          ↓
    08 — PECS
          ↓
    09 — T vs ?
          ↓
    10 — Bounded Wildcards

---

## One-Line Memory Trick

> **`extends` → safely READ from a family of subtypes.**

> **`super` → safely WRITE a specific type.**

> **`?` → exact type is unknown.**