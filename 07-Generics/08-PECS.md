# 08 — PECS

> **PECS stands for Producer Extends, Consumer Super. It is a practical rule for deciding whether to use `? extends T` or `? super T` when designing generic APIs.**

---

## Table of Contents

- [1. What Is PECS?](#1-what-is-pecs)
- [2. Why Do We Need PECS?](#2-why-do-we-need-pecs)
- [3. Producer](#3-producer)
- [4. Consumer](#4-consumer)
- [5. `extends` With Producer](#5-extends-with-producer)
- [6. `super` With Consumer](#6-super-with-consumer)
- [7. Producer Example](#7-producer-example)
- [8. Consumer Example](#8-consumer-example)
- [9. Producer and Consumer Together](#9-producer-and-consumer-together)
- [10. `List<? extends T>`](#10-list-extends-t)
- [11. `List<? super T>`](#11-list-super-t)
- [12. Why Can't Producer Add?](#12-why-cant-producer-add)
- [13. Why Can Consumer Add?](#13-why-can-consumer-add)
- [14. PECS With Methods](#14-pecs-with-methods)
- [15. PECS With Collections](#15-pecs-with-collections)
- [16. Java Copy Example](#16-java-copy-example)
- [17. `Collections.copy()` and PECS](#17-collectionscopy-and-pecs)
- [18. Generic Type Parameter vs PECS](#18-generic-type-parameter-vs-pecs)
- [19. PECS and Invariance](#19-pecs-and-invariance)
- [20. PECS in DSA](#20-pecs-in-dsa)
- [21. Internal Working](#21-internal-working)
- [22. Important Rules](#22-important-rules)
- [23. Common Mistakes](#23-common-mistakes)
- [24. Interview Traps](#24-interview-traps)
- [25. Top 10 Interview Questions](#25-top-10-interview-questions)
- [26. 30-Second Interview Answer](#26-30-second-interview-answer)
- [27. Cheat Sheet](#27-cheat-sheet)

---

# 1. What Is PECS?

**PECS** stands for:

> **Producer Extends, Consumer Super**

It is a rule used with Java Generics and wildcards.

The basic idea is:

    Producer
        ↓
    <? extends T>


    Consumer
        ↓
    <? super T>

---

## Simple Meaning

If a generic object **produces values for you to read**:

    use extends

If a generic object **consumes values that you give it**:

    use super

---

# 2. Why Do We Need PECS?

Java Generics are invariant.

For example:

    List<Integer>

is NOT a subtype of:

    List<Number>

even though:

    Integer extends Number

Therefore this is invalid:

    List<Number> numbers = new ArrayList<Integer>();

But we may want a method that can read from:

    List<Integer>

    List<Double>

    List<Float>

and treat all of them as:

    Number

This is where:

    <? extends Number>

helps.

Similarly, if we want to add Integers into:

    List<Integer>

    List<Number>

    List<Object>

we can use:

    <? super Integer>

PECS gives us the mental rule for choosing between them.

---

# 3. Producer

A **producer** is a source from which we obtain values.

Example:

    List<Integer> numbers

If our method only reads values from this list, the list is acting as a producer.

Example:

    public static void printNumbers(
            List<? extends Number> numbers) {

        for(Number number : numbers) {
            System.out.println(number);
        }
    }

Here:

    numbers
        ↓
    produces Number values
        ↓
    <? extends Number>

---

## Mental Model

    Producer
        ↓
    Gives data to us
        ↓
    We READ
        ↓
    extends

---

# 4. Consumer

A **consumer** is an object that receives values from us.

Example:

    List<Integer> numbers

If our method adds values into the list, the list is acting as a consumer.

Example:

    public static void addNumbers(
            List<? super Integer> numbers) {

        numbers.add(10);
        numbers.add(20);
    }

Here:

    numbers
        ↓
    consumes Integer values
        ↓
    <? super Integer>

---

## Mental Model

    Consumer
        ↓
    Receives data from us
        ↓
    We WRITE
        ↓
    super

---

# 5. `extends` With Producer

Use:

    <? extends T>

when the generic object is primarily a **producer**.

Example:

    List<? extends Number> numbers

The actual list could be:

    List<Integer>

    List<Double>

    List<Float>

The method can safely read each element as:

    Number

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

---

## Why `extends`?

Because the unknown type is guaranteed to be:

    Number
    or
    subtype of Number

Therefore:

    Number value = numbers.get(0);

is safe.

---

# 6. `super` With Consumer

Use:

    <? super T>

when the generic object is primarily a **consumer**.

Example:

    List<? super Integer> numbers

The actual list can be:

    List<Integer>

    List<Number>

    List<Object>

All of these can safely consume an Integer.

---

## Example

    public static void addNumbers(
            List<? super Integer> numbers) {

        numbers.add(10);
        numbers.add(20);
    }

---

## Why `super`?

Because every possible type can accept:

    Integer

For example:

    List<Integer>
    List<Number>
    List<Object>

can all store an Integer.

---

# 7. Producer Example

Suppose we have:

    List<Integer> integers = List.of(10, 20, 30);

We want to calculate their sum.

We don't need to modify the list.

Therefore the list is a:

    Producer

Use:

    <? extends Number>

Example:

    public static double sum(
            List<? extends Number> numbers) {

        double sum = 0;

        for(Number number : numbers) {
            sum += number.doubleValue();
        }

        return sum;
    }

---

## Usage

    List<Integer> integers =
            List.of(10, 20, 30);

    List<Double> doubles =
            List.of(10.5, 20.5);

    System.out.println(sum(integers));

    System.out.println(sum(doubles));

Both work.

---

# 8. Consumer Example

Suppose we want to add default values into a list.

We don't care whether the list is:

    List<Integer>

    List<Number>

    List<Object>

As long as it can consume Integers.

Use:

    <? super Integer>

Example:

    public static void addDefaults(
            List<? super Integer> list) {

        list.add(10);
        list.add(20);
        list.add(30);
    }

---

## Usage

    List<Integer> integers =
            new ArrayList<>();

    List<Number> numbers =
            new ArrayList<>();

    List<Object> objects =
            new ArrayList<>();


    addDefaults(integers);

    addDefaults(numbers);

    addDefaults(objects);

All are valid.

---

# 9. Producer and Consumer Together

Sometimes a method needs both operations.

Example:

    public static void process(
            List<? extends Number> source,
            List<? super Number> destination) {

        for(Number number : source) {
            destination.add(number);
        }
    }

Here:

    source
        ↓
    Produces Number
        ↓
    <? extends Number>


    destination
        ↓
    Consumes Number
        ↓
    <? super Number>

This is the core idea behind many generic APIs.

---

# 10. `List<? extends T>`

Syntax:

    List<? extends T>

Meaning:

> A List containing `T` or some subtype of `T`.

Example:

    List<? extends Number>

can represent:

    List<Integer>

    List<Double>

    List<Float>

---

## Main Property

You can safely **read** values as `T`.

Example:

    Number value = list.get(0);

---

## But

You cannot safely add a specific `T`.

Example:

    // list.add(10); ❌

because the actual list might be:

    List<Double>

---

# 11. `List<? super T>`

Syntax:

    List<? super T>

Meaning:

> A List containing `T` or some supertype of `T`.

Example:

    List<? super Integer>

can represent:

    List<Integer>

    List<Number>

    List<Object>

---

## Main Property

You can safely add:

    Integer

Example:

    list.add(10);

---

## But Reading

The safest type is:

    Object

Example:

    Object value = list.get(0);

---

# 12. Why Can't Producer Add?

Consider:

    List<? extends Number> list

The actual list could be:

    List<Integer>

or:

    List<Double>

Suppose Java allowed:

    list.add(10.5);

If the actual list were:

    List<Integer>

we would insert a Double into a List<Integer>.

That would violate type safety.

Therefore Java prevents adding specific values.

---

## Visual

    List<? extends Number>
             │
             ├── List<Integer>
             ├── List<Double>
             └── List<Float>

                ↓

        Exact type unknown

                ↓

        Cannot safely add

---

# 13. Why Can Consumer Add?

Consider:

    List<? super Integer> list

The actual type could be:

    List<Integer>

    List<Number>

    List<Object>

Now:

    list.add(10);

is always safe.

Why?

Because:

    Integer → Integer
    Integer → Number
    Integer → Object

Every possible destination can store an Integer.

---

## Visual

    List<? super Integer>
             │
             ├── List<Integer>
             ├── List<Number>
             └── List<Object>

                ↓

        All can store Integer

                ↓

        list.add(10) ✅

---

# 14. PECS With Methods

## Producer Method

    public static void print(
            List<? extends Number> list) {

        for(Number number : list) {
            System.out.println(number);
        }
    }

---

## Consumer Method

    public static void add(
            List<? super Integer> list) {

        list.add(10);
    }

---

## Producer + Consumer

    public static void copy(
            List<? extends Number> source,
            List<? super Number> destination) {

        for(Number number : source) {
            destination.add(number);
        }
    }

---

# 15. PECS With Collections

PECS becomes particularly useful when designing methods that work with collections.

---

## List

    List<? extends Number>

    List<? super Integer>

---

## Set

    Set<? extends Number>

    Set<? super Integer>

---

## Queue

    Queue<? extends Number>

    Queue<? super Integer>

---

## Map

Maps can use wildcards independently for keys and values.

Example:

    Map<? extends Number, ? extends Number>

or:

    Map<? super Integer, ? super Integer>

---

# 16. Java Copy Example

Consider a copy operation.

We have:

    source

which provides values.

And:

    destination

which receives values.

Therefore:

    source → Producer

    destination → Consumer

PECS says:

    source → extends

    destination → super

---

## Example

    public static <T> void copy(
            List<? super T> destination,
            List<? extends T> source) {

        for(T value : source) {
            destination.add(value);
        }
    }

---

## Usage

    List<Integer> source =
            List.of(10, 20, 30);

    List<Number> destination =
            new ArrayList<>();

    copy(destination, source);

This works because:

    source
       ↓
    List<Integer>
       ↓
    produces Integer
       ↓
    List<? extends T>


    destination
       ↓
    List<Number>
       ↓
    consumes Integer
       ↓
    List<? super T>

---

# 17. `Collections.copy()` and PECS

Java's Collections API provides:

    Collections.copy()

Conceptually, its generic signature follows the PECS principle:

    public static <T> void copy(
        List<? super T> dest,
        List<? extends T> src
    )

The important idea is:

    dest → Consumer → super

    src → Producer → extends

---

## Why?

The source produces values:

    src
      ↓
    <? extends T>

The destination consumes values:

    dest
      ↓
    <? super T>

This is one of the classic examples used to understand PECS.

---

# 18. Generic Type Parameter vs PECS

Consider:

    <T>

and:

    <? extends T>

They are not interchangeable.

---

## Type Parameter

    public static <T> void method(T value) {
    }

`T` gives us a name for the type.

---

## Wildcard

    public static void method(List<?> list) {
    }

`?` represents an unknown type.

---

## PECS

When we have a generic collection and need controlled flexibility:

    List<? extends T>

or:

    List<? super T>

can be used.

---

# 19. PECS and Invariance

Remember:

    Integer extends Number

but:

    List<Integer>

does not extend:

    List<Number>

This is generic invariance.

PECS gives us a way to work around this restriction safely.

---

## Reading

Instead of requiring:

    List<Number>

we can accept:

    List<? extends Number>

Now:

    List<Integer>

and:

    List<Double>

can both be passed.

---

## Writing

Instead of requiring:

    List<Integer>

we can accept:

    List<? super Integer>

Now:

    List<Integer>

    List<Number>

    List<Object>

can all be passed.

---

# 20. PECS in DSA

PECS is useful when creating generic data structures and utility methods.

---

## Example — Print Any Numeric Collection

    public static void printNumbers(
            List<? extends Number> list) {

        for(Number number : list) {
            System.out.println(number);
        }
    }

This works with:

    List<Integer>

    List<Double>

    List<Float>

---

## Example — Insert Integers

    public static void insertValues(
            List<? super Integer> list) {

        list.add(10);
        list.add(20);
        list.add(30);
    }

Works with:

    List<Integer>

    List<Number>

    List<Object>

---

## Example — Generic Copy

    public static <T> void copy(
            List<? super T> destination,
            List<? extends T> source) {

        for(T value : source) {
            destination.add(value);
        }
    }

This pattern is useful in:

    Array manipulation
    Stack utilities
    Queue utilities
    Tree traversal helpers
    Generic collection utilities

---

# 21. Internal Working

PECS is not a special Java keyword or language feature.

It is a **design principle based on wildcard bounds**.

The compiler uses the declared bounds to determine which operations are type-safe.

---

## Producer

For:

    List<? extends Number>

the compiler knows:

    ActualType extends Number

Therefore:

    Number value = list.get(0);

is safe.

But:

    list.add(10);

is not safe.

---

## Consumer

For:

    List<? super Integer>

the compiler knows:

    Integer extends ActualType

Therefore:

    list.add(10);

is safe.

But when retrieving:

    Integer value = list.get(0);

is not guaranteed.

The safest type is:

    Object

---

# 22. Important Rules

## Rule 1

PECS means:

    Producer Extends
    Consumer Super

---

## Rule 2

Producer means the object primarily provides values.

Use:

    <? extends T>

---

## Rule 3

Consumer means the object primarily receives values.

Use:

    <? super T>

---

## Rule 4

`extends` is mainly useful for reading.

---

## Rule 5

`super` is mainly useful for writing.

---

## Rule 6

From:

    List<? extends T>

you can safely read as:

    T

---

## Rule 7

Into:

    List<? super T>

you can safely write:

    T

---

## Rule 8

From:

    List<? super T>

the safest read type is:

    Object

---

## Rule 9

You generally cannot add a specific value to:

    List<? extends T>

---

## Rule 10

PECS is a guideline, not a Java keyword.

---

# 23. Common Mistakes

## Mistake 1 — Memorizing PECS Without Understanding Producer

Do not simply memorize:

    extends = read
    super = write

Understand why.

Ask:

> Who is producing the data?

or:

> Who is consuming the data?

---

## Mistake 2 — Thinking `extends` Means Only Classes

It represents an upper bound on the type.

Example:

    List<? extends Number>

means the unknown type is:

    Number

or a subtype.

---

## Mistake 3 — Thinking `super` Means Only Direct Parent

`? super Integer` allows:

    Integer

    Number

    Object

It includes the complete superclass hierarchy.

---

## Mistake 4 — Thinking `extends` Allows Adding Any Subtype

Example:

    List<? extends Number> list;

This does NOT mean:

    list.add(Integer)

is safe.

The exact subtype is unknown.

---

## Mistake 5 — Thinking `super` Means You Can Read as T

Example:

    List<? super Integer> list;

This does not guarantee that:

    Integer value = list.get(0);

is safe.

The actual type could be:

    Object.

---

# 24. Interview Traps

### Trap 1

**What does PECS stand for?**

    Producer Extends
    Consumer Super

---

### Trap 2

**Why does a producer use `extends`?**

Because we want to safely read values as the upper-bound type.

---

### Trap 3

**Why does a consumer use `super`?**

Because the actual type is guaranteed to be the specified type or one of its supertypes, so values of the specified type can safely be inserted.

---

### Trap 4

**Can you add to `List<? extends Number>`?**

No specific value can safely be added.

---

### Trap 5

**Can you add Integer to `List<? super Integer>`?**

Yes.

---

### Trap 6

**What can you read from `List<? super Integer>`?**

Safely as `Object`.

---

### Trap 7

**Is PECS a Java feature?**

No.

It is a guideline for using bounded wildcards.

---

### Trap 8

**Can a collection be both producer and consumer?**

Yes.

A collection can be used for both reading and writing, but the wildcard design depends on the operation the API needs to support.

---

# 25. Top 10 Interview Questions

## Q1. What does PECS stand for?

**Producer Extends, Consumer Super.**

---

## Q2. What is a producer?

A producer is an object from which a method primarily reads values.

For a producer, use:

    <? extends T>

---

## Q3. What is a consumer?

A consumer is an object into which a method primarily writes values.

For a consumer, use:

    <? super T>

---

## Q4. Why is `extends` used for producers?

Because the actual type is guaranteed to be `T` or a subtype of `T`, so values can safely be read as `T`.

---

## Q5. Why is `super` used for consumers?

Because the actual type is guaranteed to be `T` or a supertype of `T`, so a `T` can safely be added.

---

## Q6. Can you add an Integer to `List<? extends Number>`?

No.

The actual list might be:

    List<Double>

---

## Q7. Can you add an Integer to `List<? super Integer>`?

Yes.

The actual list can be:

    List<Integer>

    List<Number>

    List<Object>

---

## Q8. What can you read from `List<? extends Number>`?

You can safely read values as:

    Number

---

## Q9. What can you read from `List<? super Integer>`?

The safest type is:

    Object

---

## Q10. Give a real example of PECS.

A copy operation:

    public static <T> void copy(
            List<? super T> destination,
            List<? extends T> source) {

        for(T value : source) {
            destination.add(value);
        }
    }

Here:

    source → Producer → extends

    destination → Consumer → super

---

# 26. 30-Second Interview Answer

> **PECS stands for Producer Extends, Consumer Super. When a generic collection produces values that we want to read, we generally use `? extends T`. When a collection consumes values that we want to add, we generally use `? super T`. For example, `List<? extends Number>` allows us to safely read Numbers from lists of Number subclasses, while `List<? super Integer>` allows us to safely add Integers to lists of Integer, Number, or Object. PECS is a guideline for choosing bounded wildcards in generic APIs.**

---

# 27. Cheat Sheet

    PECS
    │
    ├── PRODUCER
    │      │
    │      ├── Gives data to us
    │      ├── Mainly READ
    │      └── <? extends T>
    │
    └── CONSUMER
           │
           ├── Receives data from us
           ├── Mainly WRITE
           └── <? super T>

---

## Producer

    List<? extends Number>
            ↓
    List<Integer>
    List<Double>
    List<Float>
            ↓
    READ as Number
            ↓
    WRITE specific value ❌

---

## Consumer

    List<? super Integer>
            ↓
    List<Integer>
    List<Number>
    List<Object>
            ↓
    WRITE Integer ✅
            ↓
    READ as Object

---

## The Core Rule

> **Producer → `extends` → READ**

> **Consumer → `super` → WRITE**

---

## Best Mental Model

    DATA FLOW

    Producer
       │
       │ gives data
       ▼
    YOUR METHOD
       │
       │ sends data
       ▼
    Consumer


    Producer  →  extends

    Consumer  →  super

---

## Connection With Previous Topics

    07 — Wildcards
          │
          ├── <?>
          ├── <? extends T>
          └── <? super T>
                    │
                    ▼
    08 — PECS
          │
          ├── Producer → extends
          └── Consumer → super
                    │
                    ▼
    09 — T vs ?
                    │
                    ▼
    10 — Bounded Wildcards