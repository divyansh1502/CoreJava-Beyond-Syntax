# 05 — Type Safety

> **Type safety means allowing a program to operate on values only when their types are compatible with the operation. Java Generics provide strong compile-time type safety.**

---

## Table of Contents

- [1. What Is Type Safety?](#1-what-is-type-safety)
- [2. Why Type Safety Matters](#2-why-type-safety-matters)
- [3. Type Safety Before Generics](#3-type-safety-before-generics)
- [4. Type Safety With Generics](#4-type-safety-with-generics)
- [5. Compile-Time Type Checking](#5-compile-time-type-checking)
- [6. Runtime Type Checking](#6-runtime-type-checking)
- [7. ClassCastException](#7-classcastexception)
- [8. How Generics Prevent Type Errors](#8-how-generics-prevent-type-errors)
- [9. Type Safety With Collections](#9-type-safety-with-collections)
- [10. Type Safety With Generic Classes](#10-type-safety-with-generic-classes)
- [11. Type Safety With Generic Methods](#11-type-safety-with-generic-methods)
- [12. Type Safety and Casting](#12-type-safety-and-casting)
- [13. Type Safety and Raw Types](#13-type-safety-and-raw-types)
- [14. Type Safety and Unchecked Operations](#14-type-safety-and-unchecked-operations)
- [15. Generics Do Not Guarantee Complete Runtime Safety](#15-generics-do-not-guarantee-complete-runtime-safety)
- [16. Type Safety and Primitive Types](#16-type-safety-and-primitive-types)
- [17. Type Safety and Invariance](#17-type-safety-and-invariance)
- [18. Type Safety in DSA](#18-type-safety-in-dsa)
- [19. Internal Working](#19-internal-working)
- [20. Important Rules](#20-important-rules)
- [21. Common Mistakes](#21-common-mistakes)
- [22. Interview Traps](#22-interview-traps)
- [23. Top 10 Interview Questions](#23-top-10-interview-questions)
- [24. 30-Second Interview Answer](#24-30-second-interview-answer)
- [25. Cheat Sheet](#25-cheat-sheet)

---

# 1. What Is Type Safety?

**Type safety** means that a program prevents incompatible types from being used where another type is expected.

For example:

    String name = "Java";

Here:

    name → String

Therefore assigning an incompatible type is rejected:

    // name = 100;

This is a compile-time type error.

---

## Simple Definition

> **Type safety ensures that values are used according to their declared or expected types.**

In Generics, type safety mainly means:

    Generic Type
          ↓
    Restricts allowed values
          ↓
    Compiler checks types
          ↓
    Invalid types rejected

---

# 2. Why Type Safety Matters

Without type safety, incorrect data can enter a program and cause failures later.

Example:

    Object value = "Java";

    Integer number = (Integer) value;

The compiler allows the cast because `Object` can reference an `Integer`.

But at runtime:

    String
      ↓
    Cannot become Integer
      ↓
    ClassCastException

Strong type checking helps detect such mistakes earlier.

---

## Main Benefits

- Detect errors earlier
- Reduce invalid operations
- Reduce unnecessary casting
- Improve code readability
- Improve API reliability
- Reduce certain runtime exceptions
- Make large applications easier to maintain

---

# 3. Type Safety Before Generics

Before Java 5, collections commonly used raw types.

Example:

    ArrayList list = new ArrayList();

    list.add("Java");
    list.add(100);

Both values are accepted.

Why?

Because the collection can treat them as:

    Object

When retrieving values:

    String name = (String) list.get(0);

    Integer number = (Integer) list.get(1);

This works if the casts match the actual objects.

But an incorrect cast can cause:

    ClassCastException

Example:

    String value = (String) list.get(1);

The actual object is an `Integer`.

Therefore the program fails at runtime.

---

# 4. Type Safety With Generics

Generics allow us to specify the expected type.

Example:

    ArrayList<String> list = new ArrayList<>();

Now:

    list.add("Java");
    list.add("Spring");

are valid.

But:

    // list.add(100);

is rejected by the compiler.

The generic declaration:

    ArrayList<String>

communicates:

> This collection is intended to contain `String` values.

---

## Type Safety Flow

    ArrayList<String>
            ↓
      Type parameter
            ↓
       String only
            ↓
    Compiler checks
            ↓
    Invalid type rejected

---

# 5. Compile-Time Type Checking

The major advantage of Generics is that many type errors are detected during compilation.

Example:

    List<Integer> numbers = new ArrayList<>();

    numbers.add(10);
    numbers.add(20);

Invalid:

    // numbers.add("Java");

The compiler knows:

    numbers → List<Integer>

Therefore `"Java"` is incompatible.

This is better than allowing the error to reach runtime.

---

## Compile-Time Error

Conceptually:

    List<Integer> numbers = new ArrayList<>();

    numbers.add("Java");

    ↓

    Compile-Time Error

The program should be corrected before execution.

---

# 6. Runtime Type Checking

Some type checks happen during runtime.

Example:

    Object value = "Java";

    Integer number = (Integer) value;

Compilation may succeed because `value` is an `Object`.

But during execution:

    Object
      ↓
    Actual object = String
      ↓
    Cast to Integer
      ↓
    ClassCastException

This demonstrates the difference between compile-time and runtime type checking.

---

## Comparison

| Compile Time | Runtime |
|---|---|
| Before execution | During execution |
| Compiler performs checking | JVM/runtime performs checking |
| Many generic type errors detected here | Some casts/errors happen here |
| Earlier feedback | Later failure |

---

# 7. ClassCastException

`ClassCastException` occurs when an object is cast to an incompatible type.

Example:

    Object value = "Java";

    Integer number = (Integer) value;

The actual object is:

    String

but the code requests:

    Integer

Therefore:

    ClassCastException

---

## Generics Reduce This Problem

Without Generics:

    List list = new ArrayList();

    list.add("Java");

    String value = (String) list.get(0);

With Generics:

    List<String> list = new ArrayList<>();

    list.add("Java");

    String value = list.get(0);

The compiler already knows that the result is a `String`.

---

# 8. How Generics Prevent Type Errors

Consider:

    class Box<T> {

        private T value;

        void set(T value) {
            this.value = value;
        }

        T get() {
            return value;
        }
    }

Now:

    Box<String> box = new Box<>();

    box.set("Java");

This is valid.

But:

    // box.set(100);

is rejected.

Why?

Because:

    Box<String>

means:

    T = String

Therefore:

    set(T value)

becomes conceptually:

    set(String value)

So an `Integer` cannot be passed.

---

# 9. Type Safety With Collections

Generics are heavily used in the Collections Framework.

## List

    List<String> names = new ArrayList<>();

    names.add("Java");
    names.add("Spring");

---

## Set

    Set<Integer> numbers = new HashSet<>();

    numbers.add(10);
    numbers.add(20);

---

## Map

    Map<Integer, String> students = new HashMap<>();

    students.put(101, "Rahul");
    students.put(102, "Aman");

Here:

    Key   → Integer
    Value → String

Therefore:

    // students.put("101", "Rahul");

is invalid because `"101"` is a `String`.

---

## Queue

    Queue<Integer> queue = new LinkedList<>();

    queue.offer(10);
    queue.offer(20);

---

## PriorityQueue

    PriorityQueue<Integer> pq = new PriorityQueue<>();

    pq.offer(30);
    pq.offer(10);

Generics ensure that the collection API works with the specified types.

---

# 10. Type Safety With Generic Classes

Consider:

    class Box<T> {

        private T value;

        Box(T value) {
            this.value = value;
        }

        T getValue() {
            return value;
        }
    }

Create:

    Box<String> stringBox = new Box<>("Java");

Now:

    String value = stringBox.getValue();

The return type is known.

Another object:

    Box<Integer> integerBox = new Box<>(100);

Now:

    Integer number = integerBox.getValue();

The same class provides different type-safe versions.

---

# 11. Type Safety With Generic Methods

Generic methods can also provide type safety.

Example:

    public static <T> T identity(T value) {
        return value;
    }

Usage:

    String name = identity("Java");

    Integer number = identity(100);

The compiler determines the appropriate type.

---

## Generic Array Example

    public static <T> T first(T[] array) {
        return array[0];
    }

Usage:

    String[] names = {"Java", "Spring"};

    String first = first(names);

Another:

    Integer[] numbers = {10, 20, 30};

    Integer first = first(numbers);

The same method works with different types while preserving type information.

---

# 12. Type Safety and Casting

One of the major benefits of Generics is reducing explicit casts.

## Without Generics

    List list = new ArrayList();

    list.add("Java");

    String value = (String) list.get(0);

Explicit cast:

    (String)

is required.

---

## With Generics

    List<String> list = new ArrayList<>();

    list.add("Java");

    String value = list.get(0);

No explicit cast is required.

---

## Why?

The compiler knows:

    List<String>
         ↓
    get()
         ↓
    String

Therefore the result can directly be assigned to:

    String

---

# 13. Type Safety and Raw Types

A **raw type** is a generic type used without specifying its type argument.

Example:

    List list = new ArrayList();

This removes much of the compile-time type checking provided by Generics.

Example:

    list.add("Java");
    list.add(100);

Both are accepted.

Compare:

    List<String> list = new ArrayList<>();

Now only Strings are accepted.

---

## Avoid Raw Types

Prefer:

    List<String> names = new ArrayList<>();

instead of:

    List names = new ArrayList();

Raw types mainly exist for backward compatibility with pre-Java 5 code.

Detailed raw-type behavior is covered in:

    06-Raw-Types.md

---

# 14. Type Safety and Unchecked Operations

When generic type information is missing or cannot be fully verified, Java may produce an **unchecked warning**.

Example:

    List list = new ArrayList();

    List<String> names = list;

The compiler may issue an unchecked conversion warning.

Another example:

    List rawList = new ArrayList();

    rawList.add(100);

    List<String> names = rawList;

Now the compiler cannot guarantee that all elements are actually Strings.

---

## Why Is This Dangerous?

Later:

    String value = names.get(0);

The compiler believes the value is a `String`.

But the actual object may be an `Integer`.

This can eventually lead to:

    ClassCastException

Therefore:

> **Unchecked operations weaken the guarantees provided by Generics.**

---

# 15. Generics Do Not Guarantee Complete Runtime Safety

Generics provide strong compile-time type safety, but they do not prevent every runtime error.

Example:

    List<String> names = new ArrayList<>();

    names.add("Java");

This is type-safe.

But:

    String value = names.get(10);

can still cause:

    IndexOutOfBoundsException

The type is correct, but the index is invalid.

---

## Another Example

    List<String> names = new ArrayList<>();

    names.add(null);

This is type-compatible because `null` can be assigned to a reference type.

But using the value incorrectly could cause:

    NullPointerException

Therefore:

    Generics
       ↓
    Strong Type Safety
       ↓
    Not Complete Runtime Safety

---

# 16. Type Safety and Primitive Types

Generics do not directly accept primitive types.

Invalid:

    List<int> numbers;

Correct:

    List<Integer> numbers;

The wrapper class is used:

    int → Integer

Other examples:

    double  → Double
    char    → Character
    boolean → Boolean
    long    → Long
    float   → Float
    short   → Short
    byte    → Byte

---

## Autoboxing

Java automatically converts primitive values to wrapper objects when needed.

Example:

    List<Integer> numbers = new ArrayList<>();

    numbers.add(10);

Conceptually:

    int
     ↓
    Integer

This automatic conversion is called **autoboxing**.

When retrieving:

    int value = numbers.get(0);

Java can automatically convert:

    Integer
       ↓
      int

This is called **unboxing**.

---

# 17. Type Safety and Invariance

Generics are generally **invariant**.

Suppose:

    String extends Object

This does NOT mean:

    List<String> extends List<Object>

Therefore this is invalid:

    List<Object> objects = new ArrayList<String>();

Why?

If this were allowed:

    objects.add(100);

Then an `Integer` could enter what was originally a:

    List<String>

This would violate type safety.

Therefore Java does not allow this assignment.

Wildcards provide controlled flexibility:

    List<? extends Object>

The detailed concept is covered in:

    07-Wildcards.md

---

# 18. Type Safety in DSA

Generics are extremely important in DSA because they allow data structures to be strongly typed.

## Generic Node

    class Node<T> {

        T data;
        Node<T> next;

        Node(T data) {
            this.data = data;
        }
    }

Usage:

    Node<Integer> node = new Node<>(10);

Now:

    node.data

is an `Integer`.

---

## Generic Stack

    class Stack<T> {

        private List<T> data = new ArrayList<>();

        void push(T value) {
            data.add(value);
        }

        T pop() {
            return data.remove(data.size() - 1);
        }
    }

Usage:

    Stack<Integer> stack = new Stack<>();

    stack.push(10);
    stack.push(20);

The stack cannot accept an unrelated type.

---

## Generic Binary Tree Node

    class TreeNode<T> {

        T data;

        TreeNode<T> left;
        TreeNode<T> right;

        TreeNode(T data) {
            this.data = data;
        }
    }

Usage:

    TreeNode<Integer> root = new TreeNode<>(50);

This provides compile-time type safety throughout the data structure.

---

# 19. Internal Working

Generics are primarily a **compile-time feature**.

Consider:

    List<String> names = new ArrayList<>();

The compiler knows:

    names → List<String>

Therefore:

    names.add(100);

is rejected.

During compilation, generic type information is used for checking and type inference.

Java then applies **type erasure** to generic types.

Conceptually:

    Source Code
         ↓
    Generic Type Checking
         ↓
    Compile
         ↓
    Type Erasure
         ↓
    Bytecode

For an unbounded type:

    T

the erased representation is generally based on:

    Object

For:

    T extends Number

the erased representation is generally based on:

    Number

This means that Generics provide most of their type-safety guarantees during compilation.

Detailed type erasure is covered in:

    11-Type-Erasure.md

---

# 20. Important Rules

## Rule 1

Generics provide compile-time type safety.

---

## Rule 2

Generics reduce explicit casting.

---

## Rule 3

A parameterized collection restricts the allowed type.

    List<String>

means the list is parameterized with `String`.

---

## Rule 4

Raw types weaken generic type safety.

    List

is less type-safe than:

    List<String>

---

## Rule 5

Unchecked operations can bypass some generic checks.

---

## Rule 6

Generics cannot directly use primitive types.

    List<int>       // Invalid
    List<Integer>   // Valid

---

## Rule 7

Generic types are generally invariant.

    List<String>

is not a subtype of:

    List<Object>

---

## Rule 8

Generics do not prevent unrelated runtime errors.

For example:

    IndexOutOfBoundsException

can still occur.

---

## Rule 9

Type safety is one of the main reasons Generics were introduced.

---

## Rule 10

Generic type checking mainly happens during compilation.

---

# 21. Common Mistakes

## Mistake 1 — Thinking Generics Prevent Every Error

Wrong idea:

> "If I use Generics, my code cannot have runtime errors."

Correct:

> Generics mainly prevent type-related errors at compile time.

---

## Mistake 2 — Confusing Type Safety With Immutability

A generic collection can still be modified.

Example:

    List<String> names = new ArrayList<>();

    names.add("Java");
    names.add("Spring");

Generics specify the type; they do not make the collection immutable.

---

## Mistake 3 — Thinking `Object` Is Type-Safe

This:

    List list = new ArrayList();

is less type-safe than:

    List<String> list = new ArrayList<>();

because the raw list can contain unrelated types.

---

## Mistake 4 — Assuming `List<String>` Can Become `List<Object>`

It cannot.

    List<Object> objects = new ArrayList<String>();

is invalid.

---

## Mistake 5 — Ignoring Raw Types

Raw types may compile with warnings and weaken compile-time checking.

Prefer parameterized types.

---

# 22. Interview Traps

### Trap 1

**Does Generics provide runtime type safety?**

Generics primarily provide **compile-time type safety**.

Runtime checks may still occur for some operations such as casts.

---

### Trap 2

**Can Generics prevent `IndexOutOfBoundsException`?**

No.

Generics are concerned mainly with type compatibility.

---

### Trap 3

**Can `List<String>` be assigned to `List<Object>`?**

No.

Generic types are generally invariant.

---

### Trap 4

**Can raw types bypass type safety?**

Yes.

Raw types remove much of the compile-time checking provided by parameterized types.

---

### Trap 5

**Can Generics use `int` directly?**

No.

Use:

    Integer

---

### Trap 6

**Do Generics completely eliminate `ClassCastException`?**

No.

They greatly reduce type-related casting errors when used correctly, but raw types, unchecked operations, explicit unsafe casts, and legacy code can still cause `ClassCastException`.

---

# 23. Top 10 Interview Questions

## Q1. What is type safety?

Type safety means ensuring that values are used only where their types are compatible with the expected type.

---

## Q2. How do Generics provide type safety?

Generics specify the expected type at compile time, allowing the compiler to reject incompatible values.

Example:

    List<String> names = new ArrayList<>();

    // names.add(100);

---

## Q3. Why are Generics better than using Object?

Generics provide stronger compile-time type checking, reduce explicit casting, improve readability, and make APIs safer.

---

## Q4. What happens without Generics?

Collections can accept different types and retrieved values often require explicit casting.

Example:

    List list = new ArrayList();

    list.add("Java");

    String value = (String) list.get(0);

---

## Q5. What is ClassCastException?

It occurs when an object is cast to an incompatible type.

Example:

    Object value = "Java";

    Integer number = (Integer) value;

---

## Q6. Can Generics prevent all runtime errors?

No.

They mainly provide compile-time type safety and do not prevent errors such as:

    NullPointerException
    IndexOutOfBoundsException
    ArithmeticException

---

## Q7. What is an unchecked operation?

An unchecked operation is an operation where the compiler cannot fully verify the generic type safety.

Raw types are a common source.

---

## Q8. Why is `List<String>` not a subtype of `List<Object>`?

Because allowing that relationship would allow values of other types to be inserted into a `List<String>`, violating type safety.

---

## Q9. What is the relationship between Generics and type erasure?

Generics are mainly checked at compile time. Java uses type erasure to translate generic code into a runtime representation compatible with Java's generic implementation.

---

## Q10. What is the biggest benefit of Generics?

The major benefit is **compile-time type safety**, along with reduced casting, improved readability, and reusable code.

---

# 24. 30-Second Interview Answer

> **Type safety means ensuring that values are used only with compatible types. Java Generics provide strong compile-time type safety by allowing us to specify the expected type, such as `List<String>`. The compiler then prevents incompatible values from being inserted and usually eliminates the need for explicit casting when retrieving values. Generics reduce type-related runtime errors such as certain `ClassCastException` cases, although they do not prevent every kind of runtime exception.**

---

# 25. Cheat Sheet

    TYPE SAFETY
    │
    ├── Main Goal
    │   └── Use compatible types
    │
    ├── Generics
    │   └── Compile-Time Type Safety
    │
    ├── Without Generics
    │   ├── Object
    │   ├── Casting
    │   └── Higher ClassCastException Risk
    │
    ├── With Generics
    │   ├── Specific Type
    │   ├── Compile-Time Checking
    │   └── Less Casting
    │
    ├── Raw Types
    │   └── Weaken Type Safety
    │
    ├── Unchecked Operations
    │   └── Compiler Cannot Fully Verify Types
    │
    ├── Primitive Types
    │   └── Use Wrapper Classes
    │
    ├── Generic Invariance
    │   └── List<String> ≠ List<Object>
    │
    └── Runtime
        └── Generics do NOT prevent every runtime error

---

## Type Safety Mental Model

    WITHOUT GENERICS

    List
     ↓
    Object
     ↓
    Different Types
     ↓
    Explicit Casting
     ↓
    Possible ClassCastException


    WITH GENERICS

    List<String>
         ↓
    String Only
         ↓
    Compiler Checks
         ↓
    Invalid Type Rejected
         ↓
    Less Casting

---

## One-Line Memory Trick

> **Generics tell the compiler what type is allowed before the program runs.**

---

## Important Connection

    01 — Generics Introduction
             ↓
    02 — Why Generics
             ↓
    03 — Generic Classes
             ↓
    04 — Generic Methods
             ↓
    05 — Type Safety
             ↓
    06 — Raw Types
             ↓
    07 — Wildcards
             ↓
    08 — PECS
             ↓
    09 — T vs ?
             ↓
    10 — Bounded Wildcards
             ↓
    11 — Type Erasure
             ↓
    12 — Bridge Methods