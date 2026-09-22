# 06 — Raw Types

> **A raw type is the use of a generic class or interface without specifying its type parameter. Raw types were mainly kept for backward compatibility with pre-Java 5 code, but they weaken compile-time type safety.**

---

## Table of Contents

- [1. What Is a Raw Type?](#1-what-is-a-raw-type)
- [2. Why Were Raw Types Introduced?](#2-why-were-raw-types-introduced)
- [3. Raw Type Example](#3-raw-type-example)
- [4. Parameterized Type vs Raw Type](#4-parameterized-type-vs-raw-type)
- [5. Raw Collections](#5-raw-collections)
- [6. Problems With Raw Types](#6-problems-with-raw-types)
- [7. ClassCastException](#7-classcastexception)
- [8. Raw Types and Generics](#8-raw-types-and-generics)
- [9. Raw Type Assignment](#9-raw-type-assignment)
- [10. Unchecked Warning](#10-unchecked-warning)
- [11. Raw Type With Generic Class](#11-raw-type-with-generic-class)
- [12. Raw Type With Generic Methods](#12-raw-type-with-generic-methods)
- [13. Raw Type vs Object](#13-raw-type-vs-object)
- [14. Raw Type vs Wildcard](#14-raw-type-vs-wildcard)
- [15. Raw Type and Type Safety](#15-raw-type-and-type-safety)
- [16. Heap Pollution](#16-heap-pollution)
- [17. Legacy Code and Raw Types](#17-legacy-code-and-raw-types)
- [18. Why Avoid Raw Types?](#18-why-avoid-raw-types)
- [19. DSA Relevance](#19-dsa-relevance)
- [20. Internal Working](#20-internal-working)
- [21. Important Rules](#21-important-rules)
- [22. Common Mistakes](#22-common-mistakes)
- [23. Interview Traps](#23-interview-traps)
- [24. Top 10 Interview Questions](#24-top-10-interview-questions)
- [25. 30-Second Interview Answer](#25-30-second-interview-answer)
- [26. Cheat Sheet](#26-cheat-sheet)

---

# 1. What Is a Raw Type?

A **raw type** is a generic type used without its type parameter.

Example:

    List<String> names = new ArrayList<>();

This is a **parameterized type**.

But:

    List names = new ArrayList();

is a **raw type**.

The generic type:

    List<T>

has been used without specifying:

    <T>

---

## Simple Definition

> **Raw type = Generic type used without its type argument.**

Example:

    List<String>       // Parameterized type

    List               // Raw type

---

# 2. Why Were Raw Types Introduced?

Java Generics were introduced in **Java 5**.

Before Java 5, generic collections did not exist.

Old code commonly looked like:

    ArrayList list = new ArrayList();

    list.add("Java");
    list.add(100);

When Generics were introduced, Java needed to remain compatible with older code.

Therefore raw types were retained.

This allowed old code to continue compiling.

---

## Historical Idea

    Before Java 5

    List list = new ArrayList();


    Java 5+

    List<String> list = new ArrayList<>();


    Raw types
        ↓
    Backward compatibility

---

# 3. Raw Type Example

Example:

    List list = new ArrayList();

We can add different types:

    list.add("Java");
    list.add(100);
    list.add(10.5);
    list.add(true);

The compiler does not enforce a single element type because no type argument was specified.

---

## Retrieving Values

    String name = (String) list.get(0);

    Integer number = (Integer) list.get(1);

Explicit casting is generally required.

---

## Problem

Suppose:

    String value = (String) list.get(1);

But index `1` contains:

    Integer

Then runtime failure occurs:

    ClassCastException

---

# 4. Parameterized Type vs Raw Type

## Parameterized Type

    List<String> names = new ArrayList<>();

This tells the compiler:

    List
     ↓
    String elements

Therefore:

    names.add("Java");

is valid.

But:

    // names.add(100);

is rejected.

---

## Raw Type

    List names = new ArrayList<>();

Now the compiler does not know that the list is intended to contain only Strings.

Therefore:

    names.add("Java");

    names.add(100);

Both are allowed.

---

## Comparison

| Feature | Parameterized Type | Raw Type |
|---|---|---|
| Example | `List<String>` | `List` |
| Type argument | Present | Missing |
| Compile-time type checking | Strong | Weaker |
| Casting | Usually unnecessary | Often required |
| Type safety | Better | Reduced |
| Recommended | Yes | No |
| Main purpose | Modern generic code | Legacy compatibility |

---

# 5. Raw Collections

Raw types are commonly seen with collections.

## Raw List

    List list = new ArrayList();

---

## Raw Set

    Set set = new HashSet();

---

## Raw Map

    Map map = new HashMap();

---

## Raw Queue

    Queue queue = new LinkedList();

---

## Raw Stack

    Stack stack = new Stack();

These compile, but they remove the generic type information.

---

# 6. Problems With Raw Types

Raw types cause several problems.

## Problem 1 — Reduced Type Safety

Example:

    List list = new ArrayList();

    list.add("Java");
    list.add(100);

Different types can enter the same collection.

---

## Problem 2 — Explicit Casting

    String value = (String) list.get(0);

With Generics:

    List<String> list = new ArrayList<>();

    String value = list.get(0);

No explicit cast is needed.

---

## Problem 3 — Runtime Errors

Incorrect casts can result in:

    ClassCastException

---

## Problem 4 — Compiler Warnings

Java may produce warnings when raw types are used.

For example:

    List list = new ArrayList();

    list.add("Java");

The compiler can warn that an unchecked or unsafe operation is being performed.

---

## Problem 5 — Poor Readability

Compare:

    List list

with:

    List<String> names

The second clearly communicates what the list is intended to contain.

---

# 7. ClassCastException

One of the biggest risks of raw types is incorrect casting.

Example:

    List list = new ArrayList();

    list.add("Java");
    list.add(100);

Now:

    String value = (String) list.get(1);

The actual object is:

    Integer

but the program tries:

    Integer → String

This is invalid.

Result:

    ClassCastException

---

## With Generics

    List<String> list = new ArrayList<>();

    list.add("Java");

    // list.add(100);

The compiler catches the problem before execution.

Therefore:

    Raw Type
       ↓
    Error may appear at runtime

    Generic Type
       ↓
    Error often detected at compile time

---

# 8. Raw Types and Generics

Consider:

    List<String> names = new ArrayList<>();

This is parameterized.

Now:

    List rawList = names;

This is allowed because a parameterized type can be assigned to its raw type.

But type information is effectively lost through the raw reference.

Example:

    List<String> names = new ArrayList<>();

    names.add("Java");

    List rawList = names;

    rawList.add(100);

Now the same underlying list contains:

    "Java"
    100

The original variable still has type:

    List<String>

This creates a dangerous situation.

---

## Later

    String value = names.get(1);

The compiler believes:

    names.get(1)

returns a `String`.

But the actual object is:

    Integer

This can result in:

    ClassCastException

---

# 9. Raw Type Assignment

A parameterized type can be assigned to a raw type.

Example:

    List<String> strings = new ArrayList<>();

    List raw = strings;

This produces a loss of generic type information.

---

## Reverse Direction

You can also assign a raw type to a parameterized type, but this typically generates an **unchecked conversion warning**.

Example:

    List raw = new ArrayList();

    List<String> strings = raw;

The compiler cannot guarantee that `raw` actually contains only Strings.

Therefore the conversion is unsafe.

---

# 10. Unchecked Warning

Raw types commonly produce unchecked warnings.

Example:

    List raw = new ArrayList();

    raw.add("Java");

The compiler may report an unchecked or unsafe operation warning depending on the operation and compiler settings.

Another example:

    List raw = new ArrayList();

    raw.add(100);

    List<String> strings = raw;

The compiler cannot prove that every element is a String.

Therefore it warns:

    unchecked conversion

---

## Important

A warning is not the same as a compilation error.

The code may still compile.

But the compiler is telling you:

> "I cannot guarantee this operation is type-safe."

---

# 11. Raw Type With Generic Class

Raw types are not limited to collections.

Suppose:

    class Box<T> {

        T value;

        Box(T value) {
            this.value = value;
        }

        T getValue() {
            return value;
        }
    }

Parameterized:

    Box<String> box = new Box<>("Java");

Here:

    T = String

So:

    box.getValue()

returns a String.

---

## Raw Version

    Box box = new Box("Java");

Now the class is being used as a raw type.

The type parameter:

    <T>

has been omitted.

The compiler loses the specific generic type information.

---

## Example

    Box box = new Box("Java");

    String value = (String) box.getValue();

Explicit casting may be necessary.

---

# 12. Raw Type With Generic Methods

A raw type can affect the types returned from generic classes.

Example:

    class Box<T> {

        T getValue() {
            return null;
        }
    }

Parameterized:

    Box<String> box = new Box<>();

    String value = box.getValue();

The compiler knows:

    T = String

---

Raw:

    Box box = new Box();

    Object value = box.getValue();

Because the generic type information is missing, the result is treated more generally.

Depending on the generic declaration and bounds, the erased type is used.

---

# 13. Raw Type vs Object

These two concepts are often confused.

## Object

Example:

    List<Object> list = new ArrayList<>();

This is still a **parameterized generic type**.

The list is explicitly declared to accept:

    Object

Therefore the compiler knows the type parameter:

    Object

---

## Raw Type

    List list = new ArrayList<>();

Here the type parameter is completely omitted.

---

## Comparison

    List<Object>
         ↓
    Parameterized type
         ↓
    Type argument = Object


    List
         ↓
    Raw type
         ↓
    Type argument = omitted

---

## Important

`List<Object>` and `List` are NOT the same thing.

---

# 14. Raw Type vs Wildcard

Raw type:

    List list

Wildcard:

    List<?> list

These are also different.

---

## Raw Type

    List list = new ArrayList();

The type parameter is omitted.

This weakens generic checking.

---

## Unbounded Wildcard

    List<?> list = new ArrayList<String>();

The generic type is still known to exist, but the exact type is unknown.

The compiler knows:

    List of some type

but does not know which type.

---

## Comparison

| Raw Type | Wildcard |
|---|---|
| `List` | `List<?>` |
| Generic information omitted | Generic structure preserved |
| Weak type safety | Stronger type safety |
| Legacy compatibility | Modern generic design |
| Avoid when possible | Commonly used |

---

# 15. Raw Type and Type Safety

Raw types weaken the main advantage of Generics.

Consider:

    List<String> names = new ArrayList<>();

The compiler knows:

    names → Strings

But:

    List names = names;

through a raw reference can allow incompatible data to enter the same object.

Example:

    List<String> names = new ArrayList<>();

    List raw = names;

    raw.add(100);

Now:

    names

appears to be:

    List<String>

but actually contains:

    Integer

This situation is dangerous because generic guarantees have been bypassed.

---

# 16. Heap Pollution

**Heap pollution** occurs when a variable of a parameterized type refers to an object that is not of the expected parameterized type.

Raw types can be one source of heap pollution.

Example:

    List<String> names = new ArrayList<>();

    List raw = names;

    raw.add(100);

Now:

    names

is declared as:

    List<String>

but the underlying list contains:

    Integer

This violates the intended generic type constraint.

---

## Conceptual Flow

    List<String>
         ↓
    Raw List reference
         ↓
    Insert Integer
         ↓
    Same underlying object
         ↓
    Generic assumption violated
         ↓
    Heap Pollution

Heap pollution is discussed further with advanced generic concepts.

---

# 17. Legacy Code and Raw Types

Raw types are primarily useful when interacting with old Java code written before Generics.

For example, an old API might return:

    List

instead of:

    List<String>

Modern code may need to interact with such APIs.

In that situation, raw types may appear.

However, when writing new code, prefer parameterized types.

---

## Legacy Example

Old-style:

    List list = oldMethod();

Modern preferred style:

    List<String> list = new ArrayList<>();

Whenever possible, convert or isolate legacy raw-type usage.

---

# 18. Why Avoid Raw Types?

Prefer:

    List<String> names = new ArrayList<>();

instead of:

    List names = new ArrayList();

Because parameterized types provide:

### 1. Better type safety

    names.add("Java");

    // names.add(100); ❌

### 2. Less casting

    String name = names.get(0);

instead of:

    String name = (String) names.get(0);

### 3. Earlier error detection

Invalid operations are detected by the compiler.

### 4. Better readability

    List<String> names

immediately communicates the intended data type.

### 5. Better maintainability

Future developers can understand the expected types without examining every insertion.

---

# 19. DSA Relevance

Raw types are important in DSA mainly because you should know **why modern DSA implementations use generics instead of raw collections**.

---

## Raw Stack

    Stack stack = new Stack();

    stack.push(10);
    stack.push("Java");

This allows unrelated types.

---

## Generic Stack

    Stack<Integer> stack = new Stack<>();

    stack.push(10);
    stack.push(20);

Now:

    // stack.push("Java");

is rejected.

---

## Generic Queue

    Queue<Integer> queue = new LinkedList<>();

    queue.offer(10);
    queue.offer(20);

---

## Generic Set

    Set<Integer> set = new HashSet<>();

    set.add(10);
    set.add(20);

---

## Generic Map

    Map<Integer, String> map = new HashMap<>();

    map.put(101, "Rahul");

The type of both keys and values is explicit.

---

## DSA Interview Pattern

When implementing reusable data structures:

    class Node<T>

    class Stack<T>

    class Queue<T>

    class TreeNode<T>

    class LinkedList<T>

generics provide type safety and reusability.

---

# 20. Internal Working

Raw types are closely connected with **type erasure**.

Suppose:

    List<String> names = new ArrayList<>();

At compile time, the compiler knows:

    String

is the element type.

But after type erasure, generic type arguments are not retained in the same way in ordinary runtime object representation.

Conceptually:

    List<String>
         ↓
    Compile-time checking
         ↓
    Type erasure
         ↓
    Runtime representation

For an unbounded generic type:

    T

the erased type is generally:

    Object

For:

    T extends Number

the erased type is generally:

    Number

---

## Raw Type Meaning

When you write:

    List

you are effectively saying:

> "Use this generic type without specifying its type argument."

The compiler therefore cannot perform the same level of generic type checking that it can perform for:

    List<String>

---

# 21. Important Rules

## Rule 1

Raw type means a generic type without its type argument.

    List

---

## Rule 2

Parameterized type specifies the type argument.

    List<String>

---

## Rule 3

Raw types mainly exist for backward compatibility.

---

## Rule 4

Avoid raw types in new code.

---

## Rule 5

Raw types weaken compile-time type safety.

---

## Rule 6

Raw types may require explicit casting.

---

## Rule 7

Raw types can cause `ClassCastException`.

---

## Rule 8

Raw types can produce unchecked warnings.

---

## Rule 9

`List<Object>` is NOT a raw type.

It is a parameterized type.

---

## Rule 10

`List<?>` is NOT a raw type.

It is a parameterized type using an unbounded wildcard.

---

## Rule 11

Raw references can bypass generic type restrictions.

---

## Rule 12

Raw types can contribute to heap pollution.

---

# 22. Common Mistakes

## Mistake 1 — Calling `List<Object>` a Raw Type

Wrong:

    List<Object>

This is parameterized.

Correct raw type:

    List

---

## Mistake 2 — Thinking `List<?>` Is Raw

Wrong:

    List<?> = raw type

Correct:

    List<?> = parameterized type with an unbounded wildcard

---

## Mistake 3 — Thinking Raw Types Are Completely Forbidden

Raw types are legal in Java.

They are mainly discouraged in modern code because they weaken type safety.

---

## Mistake 4 — Ignoring Warnings

If the compiler reports:

    unchecked

do not blindly suppress the warning.

Investigate why the generic type cannot be verified.

---

## Mistake 5 — Thinking Raw Types Automatically Cause ClassCastException

Not every raw-type usage causes an exception.

The problem is that raw types **allow unsafe operations**, which can eventually lead to runtime type errors.

---

# 23. Interview Traps

### Trap 1

**What is the difference between `List` and `List<Object>`?**

`List` is a raw type.

`List<Object>` is a parameterized type whose element type is explicitly `Object`.

---

### Trap 2

**What is the difference between `List` and `List<?>`?**

`List` removes the generic type parameter.

`List<?>` preserves the fact that the object is a generic List, while the exact element type is unknown.

---

### Trap 3

**Why do raw types exist?**

Mainly for backward compatibility with Java code written before Generics were introduced.

---

### Trap 4

**Should raw types be used in new code?**

Generally no. Parameterized types should be preferred.

---

### Trap 5

**Can raw types cause runtime exceptions?**

Yes. Unsafe insertion and casting through raw types can eventually cause `ClassCastException`.

---

### Trap 6

**Can a parameterized type be assigned to a raw type?**

Yes.

Example:

    List<String> names = new ArrayList<>();

    List raw = names;

But this loses generic type information and can enable unsafe operations.

---

### Trap 7

**Can a raw type be assigned to a parameterized type?**

It can produce an unchecked conversion warning.

Example:

    List raw = new ArrayList();

    List<String> names = raw;

The compiler cannot guarantee the contents are Strings.

---

# 24. Top 10 Interview Questions

## Q1. What is a raw type?

A raw type is a generic class or interface used without specifying its type parameter.

Example:

    List list = new ArrayList();

---

## Q2. Why do raw types exist?

They exist primarily for backward compatibility with pre-Java 5 code that did not use Generics.

---

## Q3. What is the difference between `List` and `List<String>`?

`List` is a raw type, while `List<String>` is a parameterized type with compile-time type checking for Strings.

---

## Q4. Is `List<Object>` a raw type?

No.

`List<Object>` is a parameterized type.

---

## Q5. Is `List<?>` a raw type?

No.

`List<?>` is a parameterized type using an unbounded wildcard.

---

## Q6. What problem can raw types cause?

They weaken type safety, can require explicit casts, generate unchecked warnings, and can lead to runtime `ClassCastException`.

---

## Q7. What is an unchecked warning?

It is a compiler warning indicating that the compiler cannot fully verify the type safety of an operation.

---

## Q8. Can raw types be used with generic classes?

Yes.

Example:

    Box box = new Box("Java");

Here `Box` is used as a raw type.

---

## Q9. Should raw types be used in modern Java code?

Generally no. Parameterized types should be preferred unless interacting with legacy APIs or code where raw types are unavoidable.

---

## Q10. How are raw types related to type erasure?

Both are related to Java's implementation of Generics. Generic type information is primarily used during compilation, while type erasure removes generic type arguments from the runtime representation in ordinary generic code.

---

# 25. 30-Second Interview Answer

> **A raw type is a generic type used without specifying its type parameter, such as `List` instead of `List<String>`. Raw types were retained mainly for backward compatibility with pre-Java 5 code. They weaken compile-time type safety, can require explicit casting, produce unchecked warnings, and may lead to `ClassCastException`. In modern Java, we should generally use parameterized types like `List<String>` instead of raw types.**

---

# 26. Cheat Sheet

    RAW TYPE
       │
       ├── Generic type without type argument
       │
       ├── Example
       │      └── List
       │
       ├── Parameterized Type
       │      └── List<String>
       │
       ├── Main Reason
       │      └── Backward compatibility
       │
       ├── Problems
       │      ├── Weak type safety
       │      ├── Explicit casting
       │      ├── Unchecked warnings
       │      └── Possible ClassCastException
       │
       ├── NOT Raw Types
       │      ├── List<Object>
       │      └── List<?>
       │
       └── Modern Recommendation
              └── Prefer parameterized types

---

## Quick Comparison

    List
      ↓
    RAW TYPE
      ↓
    Type argument omitted


    List<Object>
      ↓
    PARAMETERIZED TYPE
      ↓
    Type argument = Object


    List<?>
      ↓
    PARAMETERIZED TYPE
      ↓
    Type argument = unknown

---

## Memory Trick

> **`List` = "I don't specify the type."**

> **`List<Object>` = "The type is Object."**

> **`List<?>` = "There is a type, but I don't know which one."**

---

## Generic Safety Flow

    List<String>
         ↓
    Compiler knows String
         ↓
    Invalid type rejected
         ↓
    Less casting
         ↓
    Better type safety


    List
      ↓
    Compiler lacks type argument
      ↓
    Different types may enter
      ↓
    Casting may be required
      ↓
    Possible runtime failure