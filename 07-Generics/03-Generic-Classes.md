# 03 — Generic Classes

> **A generic class is a class that uses one or more type parameters so the same class can work safely with different data types.**

---

## Table of Contents

- [1. What Is a Generic Class?](#1-what-is-a-generic-class)
- [2. Basic Syntax](#2-basic-syntax)
- [3. Simple Generic Class](#3-simple-generic-class)
- [4. Creating Objects of Generic Classes](#4-creating-objects-of-generic-classes)
- [5. How Type Parameters Work](#5-how-type-parameters-work)
- [6. Generic Class with Multiple Type Parameters](#6-generic-class-with-multiple-type-parameters)
- [7. Generic Class with Methods](#7-generic-class-with-methods)
- [8. Generic Constructors](#8-generic-constructors)
- [9. Generic Class with Static Members](#9-generic-class-with-static-members)
- [10. Generic Class and Inheritance](#10-generic-class-and-inheritance)
- [11. Generic Class Implementing a Generic Interface](#11-generic-class-implementing-a-generic-interface)
- [12. Diamond Operator](#12-diamond-operator)
- [13. Raw Generic Classes](#13-raw-generic-classes)
- [14. Generic Class with Wrapper Types](#14-generic-class-with-wrapper-types)
- [15. Generic Class in DSA](#15-generic-class-in-dsa)
- [16. Internal Working](#16-internal-working)
- [17. Important Rules](#17-important-rules)
- [18. Common Mistakes](#18-common-mistakes)
- [19. Interview Traps](#19-interview-traps)
- [20. Top 10 Interview Questions](#20-top-10-interview-questions)
- [21. 30-Second Interview Answer](#21-30-second-interview-answer)
- [22. Cheat Sheet](#22-cheat-sheet)

---

# 1. What Is a Generic Class?

A **generic class** is a class that declares one or more **type parameters**.

The type parameter acts as a placeholder for an actual type.

Example:

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

    Box → Class name
    T   → Type parameter
    T value → Variable whose type is determined later

The same class can now work with different types.

    Box<String> stringBox = new Box<>("Java");

    Box<Integer> integerBox = new Box<>(100);

    Box<Double> doubleBox = new Box<>(10.5);

---

# 2. Basic Syntax

The basic syntax of a generic class is:

    class ClassName<T> {

        // class members
    }

Here:

    T → Type parameter

Multiple type parameters can also be declared:

    class Pair<K, V> {

        K key;
        V value;
    }

Here:

    K → Key type
    V → Value type

Usage:

    Pair<Integer, String> pair = new Pair<>();

---

# 3. Simple Generic Class

Consider a normal class:

    class Box {

        Object value;

        Box(Object value) {
            this.value = value;
        }

        Object getValue() {
            return value;
        }
    }

Because the field is `Object`, casting is usually required.

    Box box = new Box("Java");

    String value = (String) box.getValue();

A generic class improves this design.

    class Box<T> {

        T value;

        Box(T value) {
            this.value = value;
        }

        T getValue() {
            return value;
        }
    }

Now:

    Box<String> box = new Box<>("Java");

    String value = box.getValue();

No explicit casting is required.

---

# 4. Creating Objects of Generic Classes

A generic class can be instantiated by providing a type argument.

    class Box<T> {

        T value;

        Box(T value) {
            this.value = value;
        }

        T getValue() {
            return value;
        }
    }

### String

    Box<String> stringBox = new Box<>("Java");

### Integer

    Box<Integer> integerBox = new Box<>(100);

### Double

    Box<Double> doubleBox = new Box<>(10.5);

### Character

    Box<Character> charBox = new Box<>('A');

The same class is reused for different types.

---

# 5. How Type Parameters Work

Consider:

    class Box<T> {

        T value;

        Box(T value) {
            this.value = value;
        }

        T getValue() {
            return value;
        }
    }

When we write:

    Box<String> box = new Box<>("Java");

conceptually:

    T → String

So the class behaves conceptually like:

    class Box {

        String value;

        Box(String value) {
            this.value = value;
        }

        String getValue() {
            return value;
        }
    }

For:

    Box<Integer> box = new Box<>(100);

conceptually:

    T → Integer

Important:

> The source code uses the type parameter `T`, while the compiler performs generic type checking based on the supplied type argument.

---

# 6. Generic Class with Multiple Type Parameters

A generic class can have multiple type parameters.

Example:

    class Pair<K, V> {

        private K key;
        private V value;

        Pair(K key, V value) {
            this.key = key;
            this.value = value;
        }

        K getKey() {
            return key;
        }

        V getValue() {
            return value;
        }
    }

Usage:

    Pair<Integer, String> student = new Pair<>(101, "Rahul");

Here:

    K → Integer
    V → String

Therefore:

    student.getKey();

returns:

    Integer

And:

    student.getValue();

returns:

    String

---

# 7. Generic Class with Methods

Methods inside a generic class can use the class's type parameter.

Example:

    class Container<T> {

        private T value;

        void set(T value) {
            this.value = value;
        }

        T get() {
            return value;
        }
    }

Usage:

    Container<String> container = new Container<>();

    container.set("Java");

    String value = container.get();

The method:

    void set(T value)

works with the type specified for the object.

---

## Important Distinction

The `T` in:

    class Container<T>

is a **class-level type parameter**.

A method can also declare its own type parameter.

Example:

    class Container<T> {

        T value;

        <E> void print(E data) {
            System.out.println(data);
        }
    }

Here:

    T → Class type parameter
    E → Method type parameter

They are independent type parameters.

Generic methods will be covered deeply in:

    04-Generic-Methods.md

---

# 8. Generic Constructors

A generic class can have a normal constructor that uses the class's type parameter.

Example:

    class Box<T> {

        private T value;

        Box(T value) {
            this.value = value;
        }
    }

Usage:

    Box<String> box = new Box<>("Java");

The constructor receives:

    String

because:

    T → String

---

## Constructor Does Not Need `<T>`

This is correct:

    class Box<T> {

        Box(T value) {
            // ...
        }
    }

We do NOT write:

    class Box<T> {

        <T> Box(T value) {
            // ...
        }
    }

unless we intentionally want to declare a separate method/constructor type parameter.

---

# 9. Generic Class with Static Members

A class type parameter belongs to an object/type instance, not to the class itself.

Therefore a static field cannot directly use the class's type parameter.

Invalid:

    class Box<T> {

        static T value;
    }

Why?

Because static members belong to the class, while `T` is associated with a particular parameterized type.

For example:

    Box<String>

and:

    Box<Integer>

would require different meanings for the same static field.

This is not allowed.

---

## Static Methods

Similarly:

    class Box<T> {

        static T getValue() {
            return null;
        }
    }

is invalid.

Instead, a static method can declare its own type parameter:

    class Utility {

        static <T> T getValue(T value) {
            return value;
        }
    }

Here:

    <T>

belongs to the method, not the class.

---

# 10. Generic Class and Inheritance

A generic class can participate in inheritance.

Example:

    class Box<T> {

        T value;
    }

A subclass can preserve the generic type:

    class StringBox extends Box<String> {
    }

Now:

    StringBox box = new StringBox();

The inherited `value` is treated as a `String`.

---

## Generic Subclass

A subclass can also remain generic:

    class ColoredBox<T> extends Box<T> {

        String color;
    }

Usage:

    ColoredBox<String> box = new ColoredBox<>();

Here:

    T → String

---

## Important

These are different relationships:

    Box<String>

and:

    Box<Integer>

They are parameterized types of the same generic class, but they are not interchangeable.

---

# 11. Generic Class Implementing a Generic Interface

A generic class can implement a generic interface.

Example:

    interface Storage<T> {

        void store(T value);

        T get();
    }

Implementation:

    class DataStorage<T> implements Storage<T> {

        private T value;

        public void store(T value) {
            this.value = value;
        }

        public T get() {
            return value;
        }
    }

Usage:

    DataStorage<String> storage = new DataStorage<>();

    storage.store("Java");

    String value = storage.get();

The type flows through the class:

    Storage<T>
       ↓
    DataStorage<T>
       ↓
    DataStorage<String>
       ↓
    T = String

---

# 12. Diamond Operator

Java allows the compiler to infer the type argument on the right side using the **diamond operator**:

    <>

Example:

    ArrayList<String> list = new ArrayList<>();

Without diamond:

    ArrayList<String> list = new ArrayList<String>();

With diamond:

    ArrayList<String> list = new ArrayList<>();

The compiler infers:

    String

from the left-hand side.

---

## Generic Class Example

Without diamond:

    Box<String> box = new Box<String>("Java");

With diamond:

    Box<String> box = new Box<>("Java");

The diamond operator was introduced in **Java 7**.

---

# 13. Raw Generic Classes

A generic class can technically be used without specifying its type argument.

Example:

    Box box = new Box("Java");

This is called a **raw type**.

Raw types are generally discouraged because they remove generic type checking.

Example:

    Box box = new Box("Java");

    String value = (String) box.getValue();

Compare:

    Box<String> box = new Box<>("Java");

    String value = box.getValue();

The parameterized version is safer.

Raw types are covered deeply in:

    06-Raw-Types.md

---

# 14. Generic Class with Wrapper Types

Generics cannot directly use primitive types.

Invalid:

    Box<int> box;

Correct:

    Box<Integer> box;

Similarly:

    Box<Double>

    Box<Character>

    Box<Boolean>

    Box<Long>

Java uses wrapper classes because generic type arguments must be reference types.

---

# 15. Generic Class in DSA

Generic classes are extremely useful when implementing data structures.

---

## Generic Linked List Node

    class Node<T> {

        T data;
        Node<T> next;

        Node(T data) {
            this.data = data;
        }
    }

Integer node:

    Node<Integer> node1 = new Node<>(10);

String node:

    Node<String> node2 = new Node<>("Java");

The same node class can store different data types.

---

## Generic Stack

    class Stack<T> {

        private ArrayList<T> data = new ArrayList<>();

        void push(T value) {
            data.add(value);
        }

        T pop() {
            return data.remove(data.size() - 1);
        }
    }

Usage:

    Stack<Integer> numbers = new Stack<>();

    numbers.push(10);
    numbers.push(20);

    int value = numbers.pop();

The same stack can be used with:

    Stack<String>

or:

    Stack<Character>

---

## Generic Tree Node

    class TreeNode<T> {

        T data;

        TreeNode<T> left;
        TreeNode<T> right;

        TreeNode(T data) {
            this.data = data;
        }
    }

Usage:

    TreeNode<Integer> root = new TreeNode<>(10);

Generics therefore play an important role in reusable DSA implementations.

---

# 16. Internal Working

At the source-code level:

    class Box<T> {

        T value;
    }

The compiler uses the supplied type argument for compile-time type checking.

Example:

    Box<String> box = new Box<>("Java");

The compiler ensures that operations involving `T` are compatible with `String`.

However, Java Generics are implemented primarily through **type erasure**.

Conceptually:

    Source Code
        ↓
    Generic Type Checking
        ↓
    Compiler
        ↓
    Type Erasure
        ↓
    Bytecode

For an unbounded type parameter such as:

    T

the erased representation is generally based on:

    Object

For a bounded type parameter such as:

    T extends Number

the erasure is based on the bound:

    Number

Type erasure, bridge methods, and related compiler behavior are covered in:

    11-Type-Erasure.md
    12-Bridge-Methods.md

---

# 17. Important Rules

## Rule 1 — A generic class can have one or more type parameters

    class Box<T>

    class Pair<K, V>

    class Triple<A, B, C>

---

## Rule 2 — Type parameters can be used as field types

    class Box<T> {

        T value;
    }

---

## Rule 3 — Type parameters can be used as method parameter types

    class Box<T> {

        void set(T value) {
            // ...
        }
    }

---

## Rule 4 — Type parameters can be used as return types

    class Box<T> {

        T get() {
            return null;
        }
    }

---

## Rule 5 — Static members cannot directly use class type parameters

Invalid:

    class Box<T> {

        static T value;
    }

---

## Rule 6 — Generic classes can extend generic classes

    class Child<T> extends Parent<T> {
    }

---

## Rule 7 — Generic classes can implement generic interfaces

    class Storage<T> implements Container<T> {
    }

---

## Rule 8 — Primitive types cannot be used as type arguments

Invalid:

    Box<int>

Correct:

    Box<Integer>

---

## Rule 9 — Different parameterized types are not interchangeable

    Box<String>

is not the same type as:

    Box<Integer>

---

# 18. Common Mistakes

## Mistake 1 — Using Primitive Types

Wrong:

    Box<int> box;

Correct:

    Box<Integer> box;

---

## Mistake 2 — Using Class Type Parameter in Static Field

Wrong:

    class Box<T> {

        static T value;
    }

The type parameter belongs to the parameterized instance/type, not directly to the static member.

---

## Mistake 3 — Assuming `T` Means Only `Type`

`T` is just a conventional name.

You could write:

    class Box<X> {

        X value;
    }

This is valid.

But `T` is more readable because it conventionally means `Type`.

---

## Mistake 4 — Confusing Generic Class and Generic Method

Generic class:

    class Box<T> {
    }

Generic method:

    <T> void print(T value) {
    }

The first declares a type parameter at the class level.

The second declares a type parameter at the method level.

---

## Mistake 5 — Using Raw Types

Avoid:

    Box box = new Box("Java");

Prefer:

    Box<String> box = new Box<>("Java");

---

# 19. Interview Traps

### Trap 1

**Can a generic class have multiple type parameters?**

Yes.

    class Pair<K, V> {
    }

---

### Trap 2

**Can a static variable use the class's generic type parameter?**

No.

Invalid:

    class Box<T> {

        static T value;
    }

---

### Trap 3

**Can a generic class use primitive types?**

Not directly.

Use wrapper classes.

    Box<Integer>

instead of:

    Box<int>

---

### Trap 4

**Can a generic class extend another generic class?**

Yes.

    class Child<T> extends Parent<T> {
    }

---

### Trap 5

**Can a generic class implement a generic interface?**

Yes.

    class Storage<T> implements Container<T> {
    }

---

### Trap 6

**What does `<>` mean?**

It is the **diamond operator**, which allows the compiler to infer generic type arguments in many constructor expressions.

---

### Trap 7

**Can `Box<String>` and `Box<Integer>` be assigned to each other?**

No.

They are different parameterized types.

---

# 20. Top 10 Interview Questions

## Q1. What is a generic class?

A generic class is a class that declares one or more type parameters and can operate on different types while maintaining compile-time type safety.

---

## Q2. Give an example of a generic class.

    class Box<T> {

        T value;

        Box(T value) {
            this.value = value;
        }

        T getValue() {
            return value;
        }
    }

---

## Q3. Can a generic class have multiple type parameters?

Yes.

    class Pair<K, V> {

        K key;
        V value;
    }

---

## Q4. Can a static member use a class type parameter?

No.

The class type parameter is associated with parameterized types, while static members belong to the class itself.

---

## Q5. Can a generic class extend another class?

Yes.

    class Child<T> extends Parent<T> {
    }

---

## Q6. Can a generic class implement an interface?

Yes.

    class Storage<T> implements Container<T> {
    }

---

## Q7. Can a generic class use primitive types?

No.

Use wrapper types.

    Box<Integer>

instead of:

    Box<int>

---

## Q8. What is the diamond operator?

The `<>` operator allows the compiler to infer type arguments.

Example:

    List<String> list = new ArrayList<>();

---

## Q9. What is a raw type?

Using a generic class without specifying its type argument.

Example:

    Box box = new Box("Java");

Raw types should generally be avoided because they weaken type safety.

---

## Q10. What happens to generics at runtime?

Java primarily implements generics through **type erasure**. Generic type information is mainly used for compile-time checking, with erasure transforming generic types for the runtime representation.

---

# 21. 30-Second Interview Answer

> **A generic class is a class that uses type parameters to work with different data types while maintaining compile-time type safety. For example, `class Box<T>` can be used as `Box<String>` or `Box<Integer>`. The type parameter can be used for fields, method parameters, and return types. Generic classes improve reusability and reduce casting. They can also be used with multiple type parameters, generic interfaces, and DSA structures such as linked-list nodes and trees.**

---

# 22. Cheat Sheet

    GENERIC CLASS
    │
    ├── Syntax
    │   └── class Box<T>
    │
    ├── Type Parameter
    │   └── T
    │
    ├── Usage
    │   ├── Box<String>
    │   ├── Box<Integer>
    │   └── Box<Double>
    │
    ├── Multiple Parameters
    │   └── Pair<K, V>
    │
    ├── Can Be Used In
    │   ├── Fields
    │   ├── Methods
    │   ├── Constructors
    │   └── Return Types
    │
    ├── Cannot Directly Use
    │   └── Primitive Types
    │
    ├── Static Members
    │   └── Cannot directly use class type parameter
    │
    ├── Can Extend
    │   └── Generic Classes
    │
    ├── Can Implement
    │   └── Generic Interfaces
    │
    ├── Diamond Operator
    │   └── <>
    │
    └── Runtime
        └── Type Erasure

---

## Generic Class Mental Model

    class Box<T>
          │
          │
          ▼
    ┌───────────────┐
    │      Box      │
    │               │
    │   T value     │
    │               │
    │   set(T)      │
    │   get() → T   │
    └───────────────┘
          │
          ├───────────────┐
          │               │
          ▼               ▼
    Box<String>      Box<Integer>
          │               │
          ▼               ▼
      T = String       T = Integer

---

## One-Line Memory Trick

> **Generic Class = One class + Type Parameter + Multiple type-safe uses.**