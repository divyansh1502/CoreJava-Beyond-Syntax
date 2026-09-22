# 04 — Generic Methods

> **A generic method is a method that declares its own type parameter and can work with different data types independently of the class.**

---

## Table of Contents

- [1. What Is a Generic Method?](#1-what-is-a-generic-method)
- [2. Basic Syntax](#2-basic-syntax)
- [3. Simple Generic Method](#3-simple-generic-method)
- [4. Why Do We Need Generic Methods?](#4-why-do-we-need-generic-methods)
- [5. Generic Method vs Normal Method](#5-generic-method-vs-normal-method)
- [6. Type Parameter Position](#6-type-parameter-position)
- [7. Calling a Generic Method](#7-calling-a-generic-method)
- [8. Type Inference](#8-type-inference)
- [9. Generic Method with Multiple Type Parameters](#9-generic-method-with-multiple-type-parameters)
- [10. Generic Method with Return Type](#10-generic-method-with-return-type)
- [11. Generic Method with Parameters](#11-generic-method-with-parameters)
- [12. Generic Method Inside a Non-Generic Class](#12-generic-method-inside-a-non-generic-class)
- [13. Generic Method Inside a Generic Class](#13-generic-method-inside-a-generic-class)
- [14. Static Generic Methods](#14-static-generic-methods)
- [15. Instance Generic Methods](#15-instance-generic-methods)
- [16. Generic Constructor vs Generic Method](#16-generic-constructor-vs-generic-method)
- [17. Bounded Generic Methods](#17-bounded-generic-methods)
- [18. Generic Methods with Collections](#18-generic-methods-with-collections)
- [19. Generic Methods in DSA](#19-generic-methods-in-dsa)
- [20. Internal Working](#20-internal-working)
- [21. Important Rules](#21-important-rules)
- [22. Common Mistakes](#22-common-mistakes)
- [23. Interview Traps](#23-interview-traps)
- [24. Top 10 Interview Questions](#24-top-10-interview-questions)
- [25. 30-Second Interview Answer](#25-30-second-interview-answer)
- [26. Cheat Sheet](#26-cheat-sheet)

---

# 1. What Is a Generic Method?

A **generic method** is a method that declares its own type parameter.

Example:

    public static <T> void print(T value) {
        System.out.println(value);
    }

Here:

    <T> → Type parameter of the method
    T   → Type used by the parameter
    void → Return type
    print → Method name

The same method can work with different types.

    print("Java");

    print(100);

    print(10.5);

    print(true);

The method does not need to be overloaded for every type.

---

# 2. Basic Syntax

The general syntax is:

    <T> returnType methodName(T parameter) {
        // method body
    }

Example:

    public static <T> void display(T value) {
        System.out.println(value);
    }

Important:

The `<T>` appears **before the return type**.

Correct:

    public static <T> void display(T value)

Incorrect:

    public static void <T> display(T value)

---

# 3. Simple Generic Method

Example:

    class Utility {

        public static <T> void print(T value) {
            System.out.println(value);
        }
    }

Calling the method:

    Utility.print("Java");

    Utility.print(100);

    Utility.print(10.5);

    Utility.print(true);

The compiler determines the appropriate type for `T`.

Conceptually:

    print("Java")
         ↓
    T = String

    print(100)
         ↓
    T = Integer

    print(10.5)
         ↓
    T = Double

    print(true)
         ↓
    T = Boolean

---

# 4. Why Do We Need Generic Methods?

Without generic methods, we might use `Object`.

Example:

    public static void print(Object value) {
        System.out.println(value);
    }

This accepts many types, but the method loses specific type information.

Generic method:

    public static <T> void print(T value) {
        System.out.println(value);
    }

The generic version explicitly expresses that the method is parameterized by a type.

Generic methods are especially useful when:

- The method needs to work with multiple types
- The method needs to preserve type information
- We want reusable utilities
- We want compile-time type checking
- We do not want multiple overloaded methods

---

# 5. Generic Method vs Normal Method

## Normal Method

    public static void print(String value) {
        System.out.println(value);
    }

This method accepts only `String`.

---

## Another Normal Method

    public static void print(Integer value) {
        System.out.println(value);
    }

Now we need another method.

---

## Generic Method

    public static <T> void print(T value) {
        System.out.println(value);
    }

One method can work with multiple reference types.

---

## Comparison

| Feature | Normal Method | Generic Method |
|---|---|---|
| Type flexibility | Limited | High |
| Reusability | Lower | Higher |
| Type parameter | No | Yes |
| Compile-time type information | Depends on parameters | Strong |
| Multiple types | Often needs overloads | Same method can support many types |

---

# 6. Type Parameter Position

This is one of the most important things to understand.

Consider:

    public static <T> T identity(T value) {
        return value;
    }

The structure is:

    public static <T> T identity(T value)
                │  │       │
                │  │       └── Parameter type
                │  └────────── Return type
                └───────────── Type parameter declaration

The first `<T>` declares the type parameter.

The second `T` is the return type.

The third `T` is the parameter type.

---

# 7. Calling a Generic Method

Suppose we have:

    public static <T> void print(T value) {
        System.out.println(value);
    }

We can call it normally:

    print("Java");

    print(100);

    print(10.5);

Java can infer the type.

We can also explicitly provide the type argument.

    Main.<String>print("Java");

    Main.<Integer>print(100);

Usually explicit type arguments are unnecessary because Java can infer them.

---

# 8. Type Inference

Java can often determine the type parameter automatically.

Example:

    public static <T> T identity(T value) {
        return value;
    }

Calling:

    String result = identity("Java");

The compiler infers:

    T = String

Another example:

    Integer result = identity(100);

The compiler infers:

    T = Integer

Another:

    Double result = identity(10.5);

The compiler infers:

    T = Double

This is called **type inference**.

---

# 9. Generic Method with Multiple Type Parameters

A method can have more than one type parameter.

Example:

    public static <K, V> void printPair(K key, V value) {
        System.out.println(key + " : " + value);
    }

Calling:

    printPair(101, "Rahul");

Here:

    K = Integer
    V = String

Another call:

    printPair("ID", 500);

Now:

    K = String
    V = Integer

---

## Example with Return Value

    public static <K, V> V getValue(K key, V value) {
        return value;
    }

Usage:

    String value = getValue(101, "Rahul");

Here:

    K = Integer
    V = String

---

# 10. Generic Method with Return Type

A generic method can return the generic type.

Example:

    public static <T> T identity(T value) {
        return value;
    }

Usage:

    String name = identity("Java");

    Integer number = identity(100);

    Double decimal = identity(10.5);

The return type is determined by `T`.

---

## Another Example

    public static <T> T getFirst(T[] arr) {
        return arr[0];
    }

Usage:

    String[] names = {"Java", "Spring"};

    String first = getFirst(names);

For:

    Integer[] numbers = {10, 20, 30};

we can write:

    Integer first = getFirst(numbers);

Same method, different types.

---

# 11. Generic Method with Parameters

A generic method can use its type parameter as a parameter type.

Example:

    public static <T> void display(T value) {
        System.out.println(value);
    }

Here:

    T value

means the method parameter can be of the inferred type.

---

## Multiple Parameters

    public static <T> void printBoth(T first, T second) {
        System.out.println(first);
        System.out.println(second);
    }

Usage:

    printBoth("Java", "Spring");

Both arguments are compatible with:

    T = String

Another:

    printBoth(10, 20);

Here:

    T = Integer

---

# 12. Generic Method Inside a Non-Generic Class

A class does not need to be generic for its method to be generic.

Example:

    class Utility {

        public static <T> void print(T value) {
            System.out.println(value);
        }
    }

The class:

    Utility

is not generic.

But the method:

    <T> void print(T value)

is generic.

Usage:

    Utility.print("Java");

    Utility.print(100);

This is a very common interview concept.

---

# 13. Generic Method Inside a Generic Class

A generic class can also contain generic methods.

Example:

    class Box<T> {

        private T value;

        Box(T value) {
            this.value = value;
        }

        T getValue() {
            return value;
        }

        <E> void print(E data) {
            System.out.println(data);
        }
    }

Here:

    T → Class-level type parameter
    E → Method-level type parameter

Usage:

    Box<String> box = new Box<>("Java");

    box.print(100);

The object has:

    T = String

But the method can independently have:

    E = Integer

---

## Important

The class type parameter and method type parameter are independent.

    class Box<T> {

        <E> void print(E value) {
        }
    }

Here:

    T ≠ E

They are two different type parameters.

---

# 14. Static Generic Methods

Static methods cannot directly use a class's type parameter.

Invalid:

    class Box<T> {

        static T getValue() {
            return null;
        }
    }

Why?

Because `T` belongs to the generic class type.

Static members belong to the class itself.

---

## Correct Approach

A static method can declare its own type parameter.

    class Utility {

        static <T> T identity(T value) {
            return value;
        }
    }

Usage:

    String name = Utility.identity("Java");

    Integer number = Utility.identity(100);

Here `<T>` belongs to the method.

---

# 15. Instance Generic Methods

Generic methods can also be instance methods.

Example:

    class Utility {

        public <T> void print(T value) {
            System.out.println(value);
        }
    }

Usage:

    Utility utility = new Utility();

    utility.print("Java");

    utility.print(100);

The method has its own type parameter regardless of whether it is static or instance.

---

# 16. Generic Constructor vs Generic Method

These concepts can look similar.

## Generic Method

    public <T> void print(T value) {
        System.out.println(value);
    }

The `<T>` belongs to the method.

---

## Generic Constructor in Generic Class

    class Box<T> {

        Box(T value) {
            // ...
        }
    }

Here `T` comes from the class.

---

## Generic Constructor with Its Own Type Parameter

A constructor can also declare its own type parameter.

Example:

    class Box<T> {

        <E> Box(E value) {
            System.out.println(value);
        }
    }

Here:

    T → Class type parameter
    E → Constructor type parameter

These are independent.

---

# 17. Bounded Generic Methods

A generic method can restrict the allowed types using bounds.

Example:

    public static <T extends Number> void print(T value) {
        System.out.println(value);
    }

Now `T` must be a subtype of `Number`.

Allowed:

    print(10);

    print(10.5);

    print(100L);

Not allowed:

    print("Java");

Because:

    String

does not extend:

    Number

---

## Why Use Bounds?

Bounds allow us to safely use operations provided by the bound.

Example:

    public static <T extends Number> double getDoubleValue(T value) {
        return value.doubleValue();
    }

Here we know that `T` is a `Number`, so:

    value.doubleValue()

is available.

Bounded type parameters are covered more deeply in:

    10-Bounded-Wildcards.md

---

# 18. Generic Methods with Collections

Generic methods are extremely useful with collections.

Example:

    public static <T> void printList(List<T> list) {

        for(T value : list) {
            System.out.println(value);
        }
    }

Usage:

    List<String> names = new ArrayList<>();

    names.add("Java");
    names.add("Spring");

    printList(names);

For integers:

    List<Integer> numbers = new ArrayList<>();

    numbers.add(10);
    numbers.add(20);

    printList(numbers);

The same method works for both.

---

## Generic Method to Find Maximum

    public static <T extends Comparable<T>> T max(List<T> list) {

        T max = list.get(0);

        for(T value : list) {

            if(value.compareTo(max) > 0) {
                max = value;
            }
        }

        return max;
    }

The bound:

    T extends Comparable<T>

means `T` must implement `Comparable<T>`.

This allows the method to call:

    compareTo()

---

# 19. Generic Methods in DSA

Generic methods are useful when creating reusable DSA utilities.

---

## Generic Array Printer

    public static <T> void printArray(T[] arr) {

        for(T value : arr) {
            System.out.print(value + " ");
        }
    }

Usage:

    Integer[] numbers = {1, 2, 3};

    String[] names = {"Java", "Spring"};

    printArray(numbers);

    printArray(names);

---

## Generic Swap

    public static <T> void swap(T[] arr, int i, int j) {

        T temp = arr[i];

        arr[i] = arr[j];

        arr[j] = temp;
    }

Usage:

    Integer[] numbers = {10, 20, 30};

    swap(numbers, 0, 2);

The same method can work with:

    String[]

    Character[]

    Double[]

and other reference-type arrays.

---

## Generic Search

    public static <T> int search(T[] arr, T target) {

        for(int i = 0; i < arr.length; i++) {

            if(arr[i].equals(target)) {
                return i;
            }
        }

        return -1;
    }

Usage:

    Integer[] numbers = {10, 20, 30};

    int index = search(numbers, 20);

Or:

    String[] names = {"Java", "Spring"};

    int index = search(names, "Spring");

---

# 20. Internal Working

Consider:

    public static <T> T identity(T value) {
        return value;
    }

The compiler uses `T` for compile-time type checking and inference.

Example:

    String result = identity("Java");

The compiler determines:

    T = String

Conceptually:

    String identity(String value)

For:

    Integer result = identity(100);

Conceptually:

    Integer identity(Integer value)

However, Java implements generics primarily through **type erasure**.

Conceptually:

    Generic Source Code
            ↓
    Compile-Time Type Checking
            ↓
    Type Inference
            ↓
       Type Erasure
            ↓
         Bytecode

For an unbounded type parameter:

    <T>

the erased type is generally:

    Object

For:

    <T extends Number>

the erased type is generally:

    Number

Detailed type-erasure behavior is covered in:

    11-Type-Erasure.md

---

# 21. Important Rules

## Rule 1

A generic method declares its own type parameter.

    <T> T method(T value)

---

## Rule 2

The type parameter declaration appears before the return type.

    <T> void print(T value)

---

## Rule 3

A generic method can exist inside a non-generic class.

    class Utility {

        <T> void print(T value) {
        }
    }

---

## Rule 4

A generic method can exist inside a generic class.

    class Box<T> {

        <E> void print(E value) {
        }
    }

---

## Rule 5

Class-level and method-level type parameters are independent.

    class Box<T> {

        <E> void print(E value) {
        }
    }

---

## Rule 6

Static methods cannot directly use the class's type parameter.

Invalid:

    class Box<T> {

        static T get() {
            return null;
        }
    }

---

## Rule 7

A static method can declare its own generic type parameter.

    static <T> T identity(T value) {
        return value;
    }

---

## Rule 8

Generic methods can have multiple type parameters.

    <K, V> void print(K key, V value)

---

## Rule 9

Generic methods can have bounded type parameters.

    <T extends Number>

---

## Rule 10

Generic methods can return generic types.

    <T> T getValue(T value)

---

# 22. Common Mistakes

## Mistake 1 — Wrong Position of `<T>`

Wrong:

    public static void <T> print(T value)

Correct:

    public static <T> void print(T value)

---

## Mistake 2 — Thinking Every `T` Is the Same

These are separate declarations:

    class Box<T> {
    }

and:

    <T> void print(T value) {
    }

Their `T` parameters are independent.

---

## Mistake 3 — Using Class `T` Inside Static Method

Wrong:

    class Box<T> {

        static void print(T value) {
        }
    }

Correct:

    class Box<T> {

        static <E> void print(E value) {
        }
    }

---

## Mistake 4 — Thinking Generic Method Requires Generic Class

Not true.

This is completely valid:

    class Utility {

        static <T> void print(T value) {
            System.out.println(value);
        }
    }

---

## Mistake 5 — Confusing Generic Method with Method Overloading

Instead of:

    void print(String value)

    void print(Integer value)

    void print(Double value)

we can often use:

    <T> void print(T value)

when the implementation does not depend on a specific type.

---

# 23. Interview Traps

### Trap 1

**Where is `<T>` placed in a generic method?**

Before the return type:

    <T> T method(T value)

---

### Trap 2

**Can a non-generic class contain a generic method?**

Yes.

    class Utility {

        static <T> void print(T value) {
        }
    }

---

### Trap 3

**Can a static method be generic?**

Yes.

But it must declare its own type parameter.

    static <T> void print(T value)

---

### Trap 4

**Can a static method directly use a class-level `T`?**

No.

    class Box<T> {

        static T value;
    }

is invalid.

---

### Trap 5

**Are class-level `T` and method-level `T` the same?**

No.

Example:

    class Box<T> {

        <T> void print(T value) {
        }
    }

The two `T`s represent independent type parameters.

---

### Trap 6

**Can a generic method have multiple type parameters?**

Yes.

    <K, V> void print(K key, V value)

---

### Trap 7

**Can generic methods have bounds?**

Yes.

    <T extends Number>

---

# 24. Top 10 Interview Questions

## Q1. What is a generic method?

A generic method is a method that declares its own type parameter and can work with different types.

---

## Q2. What is the syntax of a generic method?

    <T> returnType methodName(T parameter)

Example:

    public static <T> T identity(T value) {
        return value;
    }

---

## Q3. Why is `<T>` written before the return type?

Because Java needs to declare the method's type parameter before using it in the return type or parameters.

---

## Q4. Can a non-generic class have a generic method?

Yes.

    class Utility {

        static <T> void print(T value) {
        }
    }

---

## Q5. Can a generic class have a generic method?

Yes.

    class Box<T> {

        <E> void print(E value) {
        }
    }

---

## Q6. Can static methods be generic?

Yes.

The method must declare its own type parameter.

    static <T> T identity(T value)

---

## Q7. Why can't a static method directly use the class type parameter?

Because the class type parameter belongs to the parameterized type, while static members belong to the class itself.

---

## Q8. Can a generic method have multiple type parameters?

Yes.

    <K, V> void print(K key, V value)

---

## Q9. What is type inference?

Type inference is the compiler's ability to determine the appropriate type argument from the method invocation.

Example:

    String value = identity("Java");

The compiler infers:

    T = String

---

## Q10. Can a generic method have bounded type parameters?

Yes.

Example:

    <T extends Number> void print(T value)

This restricts `T` to `Number` or its subclasses.

---

# 25. 30-Second Interview Answer

> **A generic method is a method that declares its own type parameter using syntax such as `<T>`. It can work with different types while maintaining compile-time type safety. The type parameter is declared before the return type, for example `<T> T identity(T value)`. Generic methods can exist inside both generic and non-generic classes, can be static or instance methods, can have multiple type parameters, and can also use bounds such as `<T extends Number>`.**

---

# 26. Cheat Sheet

    GENERIC METHOD
    │
    ├── Syntax
    │   └── <T> returnType method(T value)
    │
    ├── Type Parameter
    │   └── Declared before return type
    │
    ├── Can Be
    │   ├── Static
    │   └── Instance
    │
    ├── Can Exist Inside
    │   ├── Generic Class
    │   └── Non-Generic Class
    │
    ├── Can Have
    │   ├── One Type Parameter
    │   ├── Multiple Type Parameters
    │   └── Bounded Type Parameters
    │
    ├── Type Inference
    │   └── Compiler determines T
    │
    └── Important
        └── Method T ≠ Class T

---

## Generic Class vs Generic Method

    Generic Class:

    class Box<T> {
        T value;
    }

    T belongs to the class.


    Generic Method:

    <T> T identity(T value)

    T belongs to the method.

---

## Generic Method Mental Model

    <T> T identity(T value)
     │  │          │
     │  │          └── Parameter uses T
     │  └───────────── Return type uses T
     └──────────────── Type parameter declaration

---

## Final Memory Trick

> **Generic class → type parameter belongs to the class.**

> **Generic method → type parameter belongs to the method.**

> **`<T>` always comes before the return type in a generic method.**