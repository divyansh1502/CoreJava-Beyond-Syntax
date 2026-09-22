# 01 — Generics Introduction

> **Java Generics = Type-safe, reusable code that works with different data types without unnecessary casting.**

---

## Table of Contents

- [1. What Are Generics?](#1-what-are-generics)
- [2. Why Were Generics Introduced?](#2-why-were-generics-introduced)
- [3. Generics vs Non-Generics](#3-generics-vs-non-generics)
- [4. Main Benefits of Generics](#4-main-benefits-of-generics)
- [5. Generic Syntax](#5-generic-syntax)
- [6. Type Parameter](#6-type-parameter)
- [7. Type Argument](#7-type-argument)
- [8. Generic Class Overview](#8-generic-class-overview)
- [9. Generic Interface Overview](#9-generic-interface-overview)
- [10. Generic Method Overview](#10-generic-method-overview)
- [11. Common Type Parameter Naming Conventions](#11-common-type-parameter-naming-conventions)
- [12. Generics with Wrapper Classes](#12-generics-with-wrapper-classes)
- [13. Generics and Primitive Types](#13-generics-and-primitive-types)
- [14. Generics and Inheritance](#14-generics-and-inheritance)
- [15. Important Rules](#15-important-rules)
- [16. Common Mistakes](#16-common-mistakes)
- [17. Interview Traps](#17-interview-traps)
- [18. DSA Relevance](#18-dsa-relevance)
- [19. Top 10 Interview Questions](#19-top-10-interview-questions)
- [20. 30-Second Interview Answer](#20-30-second-interview-answer)
- [21. Cheat Sheet](#21-cheat-sheet)

---

# 1. What Are Generics?

**Generics** are a feature introduced in **Java 5** that allows classes, interfaces, and methods to work with different data types using **type parameters**.

Instead of fixing a specific data type, we can define a placeholder for the type.

### Example

    class Box<T> {

        T value;

        Box(T value) {
            this.value = value;
        }

        T getValue() {
            return value;
        }
    }

Here:

- `T` → Type parameter
- `Box<T>` → Generic class
- `T value` → Variable whose type is determined later

Usage:

    Box<String> stringBox = new Box<>("Java");
    Box<Integer> integerBox = new Box<>(100);

---

# 2. Why Were Generics Introduced?

Before Java 5, collections commonly stored values as `Object`.

    ArrayList list = new ArrayList();

    list.add("Java");
    list.add(100);

Because the collection does not know the exact type, explicit casting is required.

    String value = (String) list.get(0);

This can cause runtime errors.

    String value = (String) list.get(1);

The second element is an `Integer`, so this results in:

    ClassCastException

Generics solve this problem by providing compile-time type checking.

    ArrayList<String> list = new ArrayList<>();

    list.add("Java");
    list.add("Spring");

    String value = list.get(0);

Now:

    list.add(100);

produces a compile-time error.

---

# 3. Generics vs Non-Generics

| Feature | Without Generics | With Generics |
|---|---|---|
| Type safety | Runtime-oriented | Compile-time |
| Type casting | Required | Usually unnecessary |
| Wrong type insertion | Possible | Prevented |
| Readability | Lower | Higher |
| Reusability | Lower | Higher |
| ClassCastException risk | Higher | Reduced |

### Without Generics

    ArrayList list = new ArrayList();

    list.add("Java");

    String value = (String) list.get(0);

### With Generics

    ArrayList<String> list = new ArrayList<>();

    list.add("Java");

    String value = list.get(0);

---

# 4. Main Benefits of Generics

## 4.1 Type Safety

    ArrayList<String> names = new ArrayList<>();

    names.add("Java");
    names.add("Spring");

    // names.add(100);   // Compile-time error

---

## 4.2 Compile-Time Checking

    ArrayList<Integer> numbers = new ArrayList<>();

    numbers.add(10);
    numbers.add(20);

    // numbers.add("Java");   // Compile-time error

---

## 4.3 Reduced Type Casting

Without generics:

    ArrayList list = new ArrayList();

    list.add("Java");

    String value = (String) list.get(0);

With generics:

    ArrayList<String> list = new ArrayList<>();

    list.add("Java");

    String value = list.get(0);

---

## 4.4 Code Reusability

    class Box<T> {

        T value;

        Box(T value) {
            this.value = value;
        }

        T getValue() {
            return value;
        }
    }

Same class:

    Box<String> box1 = new Box<>("Java");
    Box<Integer> box2 = new Box<>(100);
    Box<Double> box3 = new Box<>(10.5);

---

# 5. Generic Syntax

General syntax:

    ClassName<Type>

Example:

    ArrayList<String>

Multiple type parameters:

    HashMap<Integer, String>

Here:

    Integer → Key type
    String  → Value type

---

# 6. Type Parameter

A **type parameter** is a placeholder used when defining a generic class, interface, or method.

    class Box<T> {

        T value;
    }

Here:

    T → Type Parameter

Common type parameter names:

    T → Type
    E → Element
    K → Key
    V → Value
    N → Number
    S → State

These are conventions, not mandatory rules.

---

# 7. Type Argument

A **type argument** is the actual type supplied when using a generic type.

    Box<String> box = new Box<>("Java");

Here:

    T       → Type Parameter
    String  → Type Argument

Another example:

    Box<Integer> box = new Box<>(100);

Here:

    T        → Type Parameter
    Integer  → Type Argument

### Memory Trick

    Definition → Parameter
    Usage      → Argument

---

# 8. Generic Class Overview

A generic class contains one or more type parameters.

    class Box<T> {

        private T value;

        Box(T value) {
            this.value = value;
        }

        T getValue() {
            return value;
        }
    }

Usage:

    Box<String> stringBox = new Box<>("Java");

    Box<Integer> integerBox = new Box<>(100);

The type is determined when the object is created.

---

# 9. Generic Interface Overview

Interfaces can also use generics.

    interface Container<T> {

        void add(T value);

        T get();
    }

Implementation:

    class StringContainer implements Container<String> {

        private String value;

        public void add(String value) {
            this.value = value;
        }

        public String get() {
            return value;
        }
    }

The implementation specifies the actual type.

---

# 10. Generic Method Overview

A method can have its own type parameter.

    public static <T> void print(T value) {
        System.out.println(value);
    }

Usage:

    print("Java");
    print(100);
    print(10.5);

Here:

    <T> → Method type parameter
    T   → Parameter type

Important:

The `<T>` before `void` declares the method's type parameter.

---

# 11. Common Type Parameter Naming Conventions

Java commonly follows these conventions:

| Symbol | Meaning |
|---|---|
| `T` | Type |
| `E` | Element |
| `K` | Key |
| `V` | Value |
| `N` | Number |
| `S` | State |
| `R` | Result |

Example:

    class Pair<K, V> {

        K key;
        V value;
    }

This is a convention only.

You could technically write:

    class Pair<A, B> {

        A key;
        B value;
    }

But meaningful conventional names improve readability.

---

# 12. Generics with Wrapper Classes

Generics work with reference types.

Primitive types cannot directly be used as generic type arguments.

Invalid:

    ArrayList<int> numbers;

Correct:

    ArrayList<Integer> numbers;

Other examples:

    ArrayList<Double>
    ArrayList<Character>
    ArrayList<Boolean>
    ArrayList<Long>

Java provides **autoboxing** to convert primitives into wrapper objects automatically.

    ArrayList<Integer> numbers = new ArrayList<>();

    numbers.add(10);

Conceptually:

    int
     ↓
    Integer

---

# 13. Generics and Primitive Types

This is invalid:

    List<int> numbers;

This is valid:

    List<Integer> numbers;

Reason:

> Java Generics work with reference types, not primitive types.

Use wrapper classes:

    int       → Integer
    double    → Double
    char      → Character
    boolean   → Boolean
    long      → Long
    float     → Float
    short     → Short
    byte      → Byte

---

# 14. Generics and Inheritance

A generic type does not automatically follow normal subtype relationships.

For example:

    ArrayList<String>

is **not** a subtype of:

    ArrayList<Object>

Even though:

    String

is a subtype of:

    Object

This is important and will be explored deeply in the **Wildcards** topic.

Example:

    List<String> strings = new ArrayList<>();

This is valid because:

    ArrayList<String>
            ↓
    List<String>

But:

    List<Object> objects = new ArrayList<String>();

is invalid.

Generics are generally **invariant**.

---

# 15. Important Rules

## Rule 1 — Generics use reference types

    List<Integer> list = new ArrayList<>();

Correct.

    List<int> list = new ArrayList<>();

Invalid.

---

## Rule 2 — Generic type information is checked at compile time

    List<String> list = new ArrayList<>();

    list.add("Java");

    // list.add(100);  // Compile-time error

---

## Rule 3 — Generics reduce explicit casting

    List<String> list = new ArrayList<>();

    String value = list.get(0);

No explicit cast is required.

---

## Rule 4 — Generic types can have multiple parameters

    Map<Integer, String> map = new HashMap<>();

Here:

    Integer → Key
    String  → Value

---

## Rule 5 — Generic classes can be reused

    Box<String>
    Box<Integer>
    Box<Double>

Same class, different type arguments.

---

# 16. Common Mistakes

## Mistake 1 — Using primitive types

Wrong:

    List<int> numbers;

Correct:

    List<Integer> numbers;

---

## Mistake 2 — Confusing parameter and argument

In:

    class Box<T>

`T` is a **type parameter**.

In:

    Box<String>

`String` is a **type argument**.

---

## Mistake 3 — Assuming generic types are covariant

Wrong assumption:

    String → Object

Therefore:

    List<String> → List<Object>

This is not valid.

Generics are generally invariant.

---

## Mistake 4 — Thinking Generics work only with Collections

Generics can be used with:

- Classes
- Interfaces
- Methods
- Constructors
- Collections
- Custom data structures

Example:

    class Node<T> {

        T data;
        Node<T> next;
    }

This is extremely common in DSA.

---

# 17. Interview Traps

### Trap 1

Is `T` a class?

**No.**

`T` is a type parameter.

---

### Trap 2

Can we use `int` in `List<int>`?

**No.**

Use:

    List<Integer>

---

### Trap 3

Does `List<String>` extend `List<Object>`?

**No.**

Generic types are generally invariant.

---

### Trap 4

Were generics present from Java 1.0?

**No.**

Generics were introduced in **Java 5**.

---

### Trap 5

Do generics completely eliminate runtime type errors?

**No.**

They provide strong compile-time type checking, but they do not eliminate every possible runtime error.

---

# 18. DSA Relevance

Generics are extremely important in DSA because Java Collections use generics.

Examples:

    ArrayList<Integer>

    HashSet<Integer>

    HashMap<Integer, Integer>

    Queue<Integer>

    Stack<Integer>

    PriorityQueue<Integer>

    Deque<Integer>

They allow data structures to work with a specific type safely.

---

## DSA Example — Generic Node

A linked-list node can be made generic:

    class Node<T> {

        T data;
        Node<T> next;

        Node(T data) {
            this.data = data;
        }
    }

Usage:

    Node<Integer> node1 = new Node<>(10);
    Node<Integer> node2 = new Node<>(20);

    node1.next = node2;

Now the same structure can also work with:

    Node<String>

    Node<Double>

This is one of the major practical uses of generics in DSA.

---

# 19. Top 10 Interview Questions

## Q1. What are Generics in Java?

Generics allow classes, interfaces, and methods to operate on different types using type parameters while providing compile-time type safety.

---

## Q2. When were Generics introduced?

Generics were introduced in **Java 5**.

---

## Q3. Why do we use Generics?

Main reasons:

- Type safety
- Compile-time checking
- Reduced casting
- Code reusability
- Better readability

---

## Q4. What is a type parameter?

A type parameter is a placeholder for a type.

Example:

    class Box<T> {
        T value;
    }

Here `T` is the type parameter.

---

## Q5. What is a type argument?

The actual type supplied to a generic type.

    Box<String>

Here `String` is the type argument.

---

## Q6. Can we use primitive types with Generics?

No.

Wrong:

    List<int>

Correct:

    List<Integer>

---

## Q7. What is the difference between type parameter and type argument?

    class Box<T>

`T` is a type parameter.

    Box<String>

`String` is a type argument.

---

## Q8. Are Generics limited to Collections?

No.

They can be used with:

- Classes
- Interfaces
- Methods
- Constructors
- Collections
- Custom data structures

---

## Q9. Does `List<String>` extend `List<Object>`?

No.

Generic types are generally invariant.

---

## Q10. What problem did Generics solve?

Before Generics, collections commonly stored values as `Object`, requiring explicit casting and allowing heterogeneous data.

Generics provide compile-time type checking and reduce the need for casting.

---

# 20. 30-Second Interview Answer

> **Generics were introduced in Java 5 to provide compile-time type safety and code reusability. They allow us to define classes, interfaces, and methods using type parameters. For example, `List<String>` ensures that only Strings can be added to the list and eliminates the need for explicit casting when retrieving values. Generics mainly improve type safety, readability, and reusability.**

---

# 21. Cheat Sheet

```text
GENERICS
│
├── Introduced → Java 5
│
├── Main Purpose
│   ├── Type Safety
│   ├── Compile-Time Checking
│   ├── Code Reusability
│   └── Reduced Casting
│
├── Type Parameter
│   └── T
│
├── Type Argument
│   └── String / Integer / etc.
│
├── Common Names
│   ├── T → Type
│   ├── E → Element
│   ├── K → Key
│   ├── V → Value
│   └── N → Number
│
├── Generic Types
│   ├── Class
│   ├── Interface
│   └── Method
│
├── Primitive Types
│   └── Not directly supported
│
└── Important Concept
    └── Generic types are generally invariant