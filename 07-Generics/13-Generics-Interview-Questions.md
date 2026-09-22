# 13 — Generics Interview Questions

> Complete interview-focused revision of Java Generics — fundamentals, wildcards, PECS, type erasure, bridge methods, common traps, coding questions, and DSA connections.

---

# Table of Contents

- [1. Generics Interview Roadmap](#1-generics-interview-roadmap)
- [2. What Are Generics](#2-what-are-generics)
- [3. Why Were Generics Introduced](#3-why-were-generics-introduced)
- [4. Benefits of Generics](#4-benefits-of-generics)
- [5. Type Parameter vs Type Argument](#5-type-parameter-vs-type-argument)
- [6. Generic Classes](#6-generic-classes)
- [7. Generic Methods](#7-generic-methods)
- [8. Generic Interfaces](#8-generic-interfaces)
- [9. Generic Constructors](#9-generic-constructors)
- [10. Type Safety](#10-type-safety)
- [11. Raw Types](#11-raw-types)
- [12. Wildcards](#12-wildcards)
- [13. T vs ?](#13-t-vs-)
- [14. Unbounded Wildcard](#14-unbounded-wildcard)
- [15. Upper-Bounded Wildcard](#15-upper-bounded-wildcard)
- [16. Lower-Bounded Wildcard](#16-lower-bounded-wildcard)
- [17. PECS](#17-pecs)
- [18. Bounded Type Parameters](#18-bounded-type-parameters)
- [19. Multiple Bounds](#19-multiple-bounds)
- [20. Invariance](#20-invariance)
- [21. List<Object> vs List<?>](#21-listobject-vs-list)
- [22. List<?> vs Raw List](#22-list-vs-raw-list)
- [23. Type Erasure](#23-type-erasure)
- [24. Bridge Methods](#24-bridge-methods)
- [25. Reifiable Types](#25-reifiable-types)
- [26. Generic Arrays](#26-generic-arrays)
- [27. Generic Object Creation](#27-generic-object-creation)
- [28. Static Members and Generics](#28-static-members-and-generics)
- [29. Generic Method Overloading](#29-generic-method-overloading)
- [30. Type Inference](#30-type-inference)
- [31. Diamond Operator](#31-diamond-operator)
- [32. Heap Pollution](#32-heap-pollution)
- [33. Wildcard Capture](#33-wildcard-capture)
- [34. Recursive Type Bounds](#34-recursive-type-bounds)
- [35. Generics and Collections](#35-generics-and-collections)
- [36. Generics and DSA](#36-generics-and-dsa)
- [37. Coding Interview Questions](#37-coding-interview-questions)
- [38. Common Mistakes](#38-common-mistakes)
- [39. Interview Traps](#39-interview-traps)
- [40. Top 30 Interview Questions](#40-top-30-interview-questions)
- [41. Rapid-Fire Revision](#41-rapid-fire-revision)
- [42. 30-Second Interview Answer](#42-30-second-interview-answer)
- [43. Cheat Sheet](#43-cheat-sheet)

---

# 1. Generics Interview Roadmap

For interviews, understand Generics in this order:

    Generics
       ↓
    Why Generics
       ↓
    Type Safety
       ↓
    Generic Classes
       ↓
    Generic Methods
       ↓
    Wildcards
       ↓
    Bounded Wildcards
       ↓
    PECS
       ↓
    Raw Types
       ↓
    Type Erasure
       ↓
    Bridge Methods
       ↓
    Heap Pollution
       ↓
    Interview Problems

---

# 2. What Are Generics

Generics allow classes, interfaces, and methods to work with different types while providing compile-time type safety.

Example:

    List<String> names = new ArrayList<>();

    names.add("Java");
    names.add("Spring");

Here:

    String

is the type argument.

The compiler prevents:

    names.add(100);

---

# 3. Why Were Generics Introduced

Before Java 5:

    List list = new ArrayList();

    list.add("Java");

    String value = (String) list.get(0);

Problems:

- Explicit casting was required.
- Type errors could appear at runtime.
- Collections could contain unrelated types.
- Code was less readable.

With Generics:

    List<String> list = new ArrayList<>();

    list.add("Java");

    String value = list.get(0);

The compiler knows that the list contains Strings.

---

# 4. Benefits of Generics

## 4.1 Type Safety

    List<String> names = new ArrayList<>();

    names.add("Java");

    // names.add(100); // Compile-time error

---

## 4.2 Less Casting

Without Generics:

    String value = (String) list.get(0);

With Generics:

    String value = list.get(0);

---

## 4.3 Code Reusability

Instead of creating:

    StringBox
    IntegerBox
    DoubleBox

we can create:

    class Box<T> {
    }

---

## 4.4 Better Readability

    List<Employee>

immediately communicates the expected element type.

---

# 5. Type Parameter vs Type Argument

## Type Parameter

A placeholder declared in a generic declaration.

    class Box<T> {
    }

Here:

    T

is the type parameter.

---

## Type Argument

The actual type supplied when using the generic type.

    Box<String> box;

Here:

    String

is the type argument.

---

## Memory Trick

    Parameter → Placeholder
    Argument  → Actual type

---

# 6. Generic Classes

A generic class uses one or more type parameters.

    class Box<T> {

        private T value;

        Box(T value) {
            this.value = value;
        }

        T getValue() {
            return value;
        }

        void setValue(T value) {
            this.value = value;
        }
    }

Usage:

    Box<String> box =
            new Box<>("Java");

    String value = box.getValue();

Here:

    T = String

The same class can work with another type:

    Box<Integer> box =
            new Box<>(100);

Now:

    T = Integer

---

# 7. Generic Methods

A generic method declares its own type parameter.

    public static <T> void print(T value) {
        System.out.println(value);
    }

Usage:

    print("Java");
    print(100);
    print(10.5);

The method can work with different types.

---

## Generic Method Returning a Value

    public static <T> T identity(T value) {
        return value;
    }

Usage:

    String name = identity("Java");

    Integer number = identity(100);

---

## Important

The `<T>` before the return type means:

    This method declares its own type parameter.

It does not necessarily come from a class-level generic parameter.

---

# 8. Generic Interfaces

Interfaces can also be generic.

    interface Container<T> {

        void add(T value);

        T get();
    }

Implementation:

    class StringContainer
            implements Container<String> {

        private String value;

        @Override
        public void add(String value) {
            this.value = value;
        }

        @Override
        public String get() {
            return value;
        }
    }

Here:

    T = String

---

# 9. Generic Constructors

A constructor can use the class's type parameter.

    class Box<T> {

        private T value;

        Box(T value) {
            this.value = value;
        }
    }

A constructor can also declare its own type parameter.

    class Utility {

        <T> Utility(T value) {
            System.out.println(value);
        }
    }

Here:

    T

belongs to the constructor.

---

# 10. Type Safety

Type safety means preventing incompatible types from being used where another type is expected.

Example:

    List<String> names =
            new ArrayList<>();

    names.add("Java");

This is invalid:

    names.add(100);

The compiler catches the problem before runtime.

Therefore:

    Generics
        ↓
    Compile-time checking
        ↓
    Type Safety

---

# 11. Raw Types

A raw type is a generic type used without specifying its type argument.

Example:

    List list = new ArrayList<>();

Instead of:

    List<String> list =
            new ArrayList<>();

Raw types are mainly supported for backward compatibility with pre-Java-5 code.

---

## Problem

    List list = new ArrayList();

    list.add("Java");
    list.add(100);

Different types can be inserted.

Later:

    String value = (String) list.get(1);

This can cause:

    ClassCastException

---

## Recommendation

Prefer:

    List<String>

over:

    List

---

# 12. Wildcards

The wildcard is:

    ?

It represents an unknown type.

Example:

    List<?> list;

This can refer to:

    List<String>
    List<Integer>
    List<Employee>

but the exact type is unknown.

---

# 13. T vs ?

This is a very common interview question.

## T

`T` is a named type parameter.

Example:

    static <T> void print(T value) {
        System.out.println(value);
    }

The caller's type is represented by `T`.

---

## ?

`?` represents an unknown type.

Example:

    static void print(List<?> list) {
        System.out.println(list);
    }

We do not care about the exact element type.

---

## Important Difference

    T
    ↓
    Named type parameter

    ?
    ↓
    Unknown type

---

## Example Showing the Difference

    static <T> T getFirst(List<T> list) {
        return list.get(0);
    }

Here the same `T` connects the input and return type.

But:

    static void print(List<?> list) {
        Object value = list.get(0);
    }

Here the exact element type is unknown.

---

# 14. Unbounded Wildcard

Syntax:

    ?

Example:

    List<?> list;

This means:

    A List of some unknown type.

You can safely read:

    Object value = list.get(0);

But you cannot generally add a specific value:

    list.add("Java"); // Compile-time error

The only universally safe value to add is:

    list.add(null);

---

# 15. Upper-Bounded Wildcard

Syntax:

    ? extends Type

Example:

    List<? extends Number>

This can represent:

    List<Integer>
    List<Double>
    List<Float>
    List<Number>

---

## Reading

You can safely read elements as:

    Number number = list.get(0);

because every possible element is a Number.

---

## Adding

You generally cannot add a specific Number:

    list.add(10); // Not allowed

Why?

Because the actual list could be:

    List<Double>

Adding Integer would be unsafe.

---

## Mental Model

    ? extends Number

means:

    "Some unknown subtype of Number."

---

# 16. Lower-Bounded Wildcard

Syntax:

    ? super Type

Example:

    List<? super Integer>

This can represent:

    List<Integer>
    List<Number>
    List<Object>

---

## Adding

You can safely add Integer:

    list.add(10);

because Integer can be stored in:

    Integer
    Number
    Object

---

## Reading

The exact type is unknown.

Therefore:

    Object value = list.get(0);

is safe.

But:

    Integer value = list.get(0);

is not generally safe.

---

# 17. PECS

PECS means:

    Producer Extends
    Consumer Super

This is one of the most important Generics interview rules.

---

## Producer

If a structure produces values for you:

    ? extends T

Example:

    List<? extends Number>

You mainly read from it.

---

## Consumer

If a structure consumes values from you:

    ? super T

Example:

    List<? super Integer>

You can add Integer values.

---

## Memory Trick

    Producer → extends
    Consumer → super

---

## Example

Producer:

    static double sum(
            List<? extends Number> list) {

        double sum = 0;

        for (Number number : list) {
            sum += number.doubleValue();
        }

        return sum;
    }

Consumer:

    static void addNumbers(
            List<? super Integer> list) {

        list.add(10);
        list.add(20);
    }

---

# 18. Bounded Type Parameters

A type parameter can have an upper bound.

Example:

    <T extends Number>

This means:

    T must be Number or a subclass of Number.

Example:

    static <T extends Number>
    double square(T value) {

        double number =
                value.doubleValue();

        return number * number;
    }

Valid:

    square(10);
    square(10.5);
    square(20L);

---

# 19. Multiple Bounds

A type parameter can have multiple bounds.

Example:

    <T extends Number & Comparable<T>>

This means `T` must:

- Extend Number
- Implement Comparable<T>

Important syntax rule:

    Class must come first.
    Interfaces come after it.

Correct:

    <T extends Number & Comparable<T>>

Invalid:

    <T extends Comparable<T> & Number>

---

# 20. Invariance

Java Generics are invariant.

This is invalid:

    List<Object> objects =
            new ArrayList<String>();

Even though:

    String extends Object

we do NOT have:

    List<String> extends List<Object>

---

## Why?

If it were allowed:

    List<Object> objects =
            new ArrayList<String>();

Then:

    objects.add(100);

would be legal.

But the actual list is:

    List<String>

So an Integer would enter a String list.

That would violate type safety.

---

## Correct Alternative

Use:

    List<?> list;

or, when appropriate:

    List<? extends Object> list;

---

# 21. List<Object> vs List<?>

These are different.

## List<Object>

Means:

    A list whose exact element type is Object.

Example:

    List<Object> list =
            new ArrayList<>();

    list.add("Java");
    list.add(100);
    list.add(10.5);

---

## List<?>

Means:

    A list of some unknown type.

Example:

    List<?> list =
            new ArrayList<String>();

You can read:

    Object value = list.get(0);

But cannot generally add a specific object.

---

## Key Difference

    List<Object>
        ↓
    Known type = Object

    List<?>
        ↓
    Unknown type

---

# 22. List<?> vs Raw List

## Raw List

    List list;

The generic type information is omitted.

It may generate unchecked warnings.

---

## Wildcard List

    List<?> list;

The list is still treated as a generic type.

The exact element type is unknown.

The compiler therefore applies type-safety rules.

---

## Prefer

    List<?>

over:

    List

when the exact element type does not matter.

---

# 23. Type Erasure

Java implements generics using type erasure.

At compile time, generic type information is used for type checking.

At runtime, most generic type arguments are erased.

Example:

    class Box<T> {

        T value;

        T getValue() {
            return value;
        }
    }

Conceptually after erasure:

    class Box {

        Object value;

        Object getValue() {
            return value;
        }
    }

For:

    T extends Number

the erasure is:

    Number

because the first bound becomes the erased type.

---

## Important

Generics mainly provide compile-time type safety.

They do not create separate runtime classes for:

    List<String>
    List<Integer>

There is still one runtime class:

    ArrayList

---

# 24. Bridge Methods

Bridge methods are compiler-generated methods used to preserve polymorphism after type erasure.

Example:

    class Parent<T> {

        T get() {
            return null;
        }
    }

    class Child extends Parent<String> {

        @Override
        String get() {
            return "Java";
        }
    }

Source-level methods:

    Parent:
        T get()

    Child:
        String get()

After erasure:

    Parent:
        Object get()

    Child:
        String get()

The compiler can generate a bridge method conceptually:

    Object get() {
        return get();
    }

The bridge delegates to:

    String get()

---

## Reflection

A bridge method can be detected with:

    method.isBridge();

Synthetic status:

    method.isSynthetic();

---

# 25. Reifiable Types

A reifiable type is a type whose runtime representation contains enough information about the type.

Examples:

    String
    Object
    List
    List<?>
    String[]
    int[]

Non-reifiable examples:

    List<String>
    List<Integer>
    List<Employee>
    T
    T[]

---

## Example

This is allowed:

    if (value instanceof List<?>) {
        // valid
    }

This is not allowed:

    if (value instanceof List<String>) {
        // compile-time error
    }

because:

    List<String>

is non-reifiable.

---

# 26. Generic Arrays

This is invalid:

    class Box<T> {

        T[] values = new T[10];
    }

Why?

Arrays know their component type at runtime.

Generic type parameters are erased.

The runtime cannot directly create:

    new T[10]

---

## Common Alternative

Internally, generic data structures sometimes use:

    Object[] array;

and carefully perform type-safe operations around it.

---

## Important Comparison

Arrays:

    Runtime type information

Generics:

    Mostly compile-time type information

This difference is one reason generic arrays are restricted.

---

# 27. Generic Object Creation

This is invalid:

    class Box<T> {

        T create() {
            return new T();
        }
    }

Why?

Because at runtime the exact class represented by `T` is not generally available.

---

## Alternative 1 — Class<T>

    static <T> T create(
            Class<T> type)
            throws Exception {

        return type.getDeclaredConstructor()
                .newInstance();
    }

Usage:

    String value =
            create(String.class);

---

## Alternative 2 — Supplier<T>

    static <T> T create(
            Supplier<T> supplier) {

        return supplier.get();
    }

Usage:

    String value =
            create(() -> "Java");

---

# 28. Static Members and Generics

This is invalid:

    class Box<T> {

        static T value;
    }

Why?

Because static members belong to the class itself.

There is only one class-level static field:

    Box.value

not separate fields for:

    Box<String>
    Box<Integer>

---

## Generic Static Method

This is valid:

    class Utility {

        static <T> T identity(T value) {
            return value;
        }
    }

Here:

    T

belongs to the method.

---

# 29. Generic Method Overloading

You cannot overload methods only by changing generic type arguments.

Invalid:

    void process(List<String> list) {
    }

    void process(List<Integer> list) {
    }

After type erasure, both become conceptually:

    void process(List list)

Therefore their signatures collide.

---

## Valid

    void process(List<String> list) {
    }

    void process(Set<String> set) {
    }

After erasure:

    void process(List list)

    void process(Set set)

These are different methods.

---

# 30. Type Inference

Type inference means the compiler determines the appropriate generic type from context.

Example:

    static <T> T identity(T value) {
        return value;
    }

Usage:

    String value =
            identity("Java");

The compiler infers:

    T = String

Another example:

    List<String> names =
            new ArrayList<>();

The compiler infers the constructor's type argument from the target type.

---

# 31. Diamond Operator

The diamond operator is:

    <>

It reduces redundant generic type declarations.

Without diamond:

    List<String> names =
            new ArrayList<String>();

With diamond:

    List<String> names =
            new ArrayList<>();

The diamond operator was introduced in Java 7.

---

# 32. Heap Pollution

Heap pollution occurs when a parameterized reference points to an object that does not conform to its parameterized type.

Example:

    List<String> strings =
            new ArrayList<>();

    List raw = strings;

    raw.add(100);

Now the underlying list contains an Integer even though the parameterized reference expects Strings.

Later:

    String value = strings.get(0);

may cause:

    ClassCastException

---

## Common Sources

    Raw types
    Unsafe casts
    Unchecked operations
    Certain generic varargs situations

---

# 33. Wildcard Capture

Consider:

    void print(List<?> list) {
        // ...
    }

The `?` represents an unknown type.

Internally, the compiler can treat it as a captured type.

Conceptually:

    List<?> 
       ↓
    List<CAP>

where `CAP` represents an unknown captured type.

---

## Example

    static void swapFirstTwo(
            List<?> list) {

        swapHelper(list, 0, 1);
    }

    private static <T> void swapHelper(
            List<T> list,
            int i,
            int j) {

        T temp = list.get(i);

        list.set(i, list.get(j));

        list.set(j, temp);
    }

The helper captures the unknown type as `T`.

---

# 34. Recursive Type Bounds

A type parameter can refer to itself in its bound.

Classic example:

    <T extends Comparable<T>>

Example:

    class Student
            implements Comparable<Student> {

        int marks;

        @Override
        public int compareTo(Student other) {

            return Integer.compare(
                    this.marks,
                    other.marks
            );
        }
    }

Here:

    T extends Comparable<T>

means:

    T must be comparable to itself.

This pattern is called:

    Recursive Type Bound

or:

    Self-Referential Generic Bound

---

# 35. Generics and Collections

Generics are heavily used throughout the Java Collections Framework.

Examples:

    List<String>

    Set<Integer>

    Map<String, Integer>

    Queue<Employee>

    Stack<Double>

---

## Map Example

    Map<String, Integer> marks =
            new HashMap<>();

    marks.put("Java", 90);
    marks.put("DSA", 85);

Now:

    String

is the key type.

    Integer

is the value type.

---

## Why This Is Important

Without generics:

    Map map = new HashMap();

We would lose compile-time information.

With:

    Map<String, Integer>

the compiler knows:

    Key   → String
    Value → Integer

---

# 36. Generics and DSA

Generics are extremely important when implementing DSA in Java.

---

## Generic Stack

    class Stack<T> {

        private Object[] data;

        private int top = -1;

        Stack(int size) {
            data = new Object[size];
        }

        void push(T value) {
            data[++top] = value;
        }

        @SuppressWarnings("unchecked")
        T pop() {
            return (T) data[top--];
        }
    }

Usage:

    Stack<Integer> stack =
            new Stack<>(10);

---

## Generic Node

    class Node<T> {

        T data;
        Node<T> next;

        Node(T data) {
            this.data = data;
        }
    }

Usage:

    Node<Integer> node =
            new Node<>(10);

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

---

## DSA Patterns Related to Generics

Important areas:

    Generic Node
    Generic Linked List
    Generic Stack
    Generic Queue
    Generic Tree
    Generic Graph
    Comparable
    Comparator
    PriorityQueue
    Collections
    HashMap
    HashSet

---

# 37. Coding Interview Questions

## Question 1 — Generic Swap

Write a generic method to swap two elements of an array.

Solution:

    public static <T> void swap(
            T[] arr,
            int i,
            int j) {

        T temp = arr[i];

        arr[i] = arr[j];

        arr[j] = temp;
    }

Complexity:

    Time:  O(1)
    Space: O(1)

---

## Question 2 — Generic Maximum

Find the maximum element using a generic method.

    public static <T extends Comparable<T>>
    T maximum(T[] arr) {

        T max = arr[0];

        for (T value : arr) {

            if (value.compareTo(max) > 0) {
                max = value;
            }
        }

        return max;
    }

Complexity:

    Time:  O(n)
    Space: O(1)

---

## Question 3 — Generic Search

    public static <T> int search(
            T[] arr,
            T target) {

        for (int i = 0;
             i < arr.length;
             i++) {

            if (arr[i].equals(target)) {
                return i;
            }
        }

        return -1;
    }

Complexity:

    Time:  O(n)
    Space: O(1)

---

## Question 4 — Print Any List

    public static void print(
            List<?> list) {

        for (Object value : list) {
            System.out.println(value);
        }
    }

Why `?`?

Because the method does not need to know the exact element type.

---

## Question 5 — Add Integers

    public static void addNumbers(
            List<? super Integer> list) {

        list.add(10);
        list.add(20);
        list.add(30);
    }

Why `super`?

Because the list is a consumer of Integer values.

---

## Question 6 — Read Numbers

    public static double sum(
            List<? extends Number> list) {

        double sum = 0;

        for (Number number : list) {
            sum += number.doubleValue();
        }

        return sum;
    }

Why `extends`?

Because the list is producing Number values.

---

## Question 7 — Reverse a Generic List

    public static <T> void reverse(
            List<T> list) {

        int left = 0;
        int right = list.size() - 1;

        while (left < right) {

            T temp = list.get(left);

            list.set(left, list.get(right));

            list.set(right, temp);

            left++;
            right--;
        }
    }

Complexity for an ArrayList:

    Time:  O(n)
    Space: O(1)

---

# 38. Common Mistakes

## Mistake 1

Thinking:

    List<String>

is a subtype of:

    List<Object>

It is not.

Java Generics are invariant.

---

## Mistake 2

Thinking `?` means Object.

It does not.

    ?

means:

    Unknown type

while:

    Object

is an actual known type.

---

## Mistake 3

Thinking `List<Object>` and `List<?>` are identical.

They are not.

    List<Object>
        ↓
    Element type is Object

    List<?>
        ↓
    Element type is unknown

---

## Mistake 4

Thinking `extends` always means inheritance in the same sense.

In:

    ? extends Number

it represents an unknown subtype bounded by Number.

---

## Mistake 5

Thinking `? super Integer` means only `Integer`.

It can represent:

    Integer
    Number
    Object

---

## Mistake 6

Trying to create:

    new T()

Generic type parameters do not provide enough runtime type information for direct construction.

---

## Mistake 7

Trying:

    new T[10]

Generic arrays cannot generally be created directly.

---

## Mistake 8

Using generic type parameters in static fields.

Invalid:

    static T value;

Static members belong to the class, while `T` belongs to a generic instance/type context.

---

## Mistake 9

Thinking generics are fully available at runtime.

Most generic type arguments are erased.

---

## Mistake 10

Thinking bridge methods are manually written.

They are generally compiler-generated.

---

# 39. Interview Traps

## Trap 1

Can you add an Integer to:

    List<? extends Number>

No.

The actual list could be:

    List<Double>

---

## Trap 2

Can you add an Integer to:

    List<? super Integer>

Yes.

The list could be:

    List<Integer>
    List<Number>
    List<Object>

All can safely accept Integer.

---

## Trap 3

Can you read from:

    List<? extends Number>

Yes, safely as:

    Number

---

## Trap 4

Can you read from:

    List<? super Integer>

Yes, but safely only as:

    Object

---

## Trap 5

Can you do:

    new ArrayList<String>()

when assigning to:

    List<?> list

Yes.

---

## Trap 6

Can you check:

    instanceof List<String>

No.

But:

    instanceof List<?>

is valid.

---

## Trap 7

Can a static method be generic?

Yes.

Example:

    static <T> T identity(T value) {
        return value;
    }

---

## Trap 8

Can a class be generic and also have generic methods?

Yes.

Example:

    class Box<T> {

        <U> void print(U value) {
            System.out.println(value);
        }
    }

---

## Trap 9

Does every generic method create a bridge method?

No.

Bridge methods are generated only when required.

---

## Trap 10

Are Generics and Collections the same thing?

No.

Generics are a language feature.

Collections are library classes/interfaces.

Generics are heavily used by Collections.

---

# 40. Top 30 Interview Questions

## Q1. What are Generics?

Generics allow classes, interfaces, and methods to work with different types while providing compile-time type safety.

---

## Q2. Why were Generics introduced?

To provide type safety, reduce casting, and improve code reusability and readability.

---

## Q3. What is a type parameter?

A placeholder such as:

    T

declared in a generic class, interface, or method.

---

## Q4. What is a type argument?

The actual type supplied to a generic type.

Example:

    String

in:

    List<String>

---

## Q5. What is a raw type?

A generic type used without specifying its type argument.

Example:

    List list;

---

## Q6. What is a wildcard?

    ?

represents an unknown type.

---

## Q7. What is `T`?

A named type parameter.

---

## Q8. What is `?`?

An unknown type represented using a wildcard.

---

## Q9. What does `? extends Number` mean?

Some unknown type that is Number or a subtype of Number.

---

## Q10. What does `? super Integer` mean?

Some unknown type that is Integer or a supertype of Integer.

---

## Q11. What is PECS?

    Producer Extends
    Consumer Super

---

## Q12. Why can't we add values to `List<? extends Number>`?

Because the exact subtype is unknown.

The list could be:

    List<Double>

---

## Q13. Why can we add Integer to `List<? super Integer>`?

Because the actual list type must be Integer or one of its supertypes.

---

## Q14. Are Java Generics covariant?

No.

They are invariant.

---

## Q15. Is `List<String>` a subtype of `List<Object>`?

No.

---

## Q16. What is type erasure?

The process by which most generic type arguments are removed from runtime representation while maintaining compile-time generic checking.

---

## Q17. What does `T` erase to?

For an unbounded type parameter:

    Object

---

## Q18. What does `<T extends Number>` erase to?

Conceptually:

    Number

---

## Q19. What is a bridge method?

A compiler-generated method used to preserve polymorphism after type erasure.

---

## Q20. How do you detect a bridge method?

Using:

    method.isBridge()

---

## Q21. Why can't we create `new T()`?

Because the runtime does not generally know the concrete type represented by `T`.

---

## Q22. Why can't we create `new T[10]`?

Because generic type parameters are erased while arrays require runtime component-type information.

---

## Q23. Why can't a static field use `T`?

Because static members belong to the class, while the generic type parameter belongs to the generic type context.

---

## Q24. What is heap pollution?

A situation where a parameterized reference points to an object whose actual contents violate the expected parameterized type.

---

## Q25. What is the difference between `List<Object>` and `List<?>`?

`List<Object>` specifically represents a list of Object.

`List<?>` represents a list of an unknown type.

---

## Q26. What is the diamond operator?

    <>

It allows the compiler to infer generic type arguments.

---

## Q27. What is type inference?

The compiler determines the generic type from available context.

---

## Q28. What is a recursive type bound?

A bound where the type parameter refers to itself.

Example:

    <T extends Comparable<T>>

---

## Q29. Can generic methods be static?

Yes.

Example:

    static <T> T identity(T value) {
        return value;
    }

---

## Q30. What is the main purpose of Generics?

Compile-time type safety and reusable type-independent code.

---

# 41. Rapid-Fire Revision

### Generics introduced in?

    Java 5

### Main purpose?

    Compile-time type safety

### Generic placeholder?

    T

### Unknown type?

    ?

### Producer?

    extends

### Consumer?

    super

### Unbounded wildcard?

    ?

### Upper bound?

    ? extends T

### Lower bound?

    ? super T

### Generic class?

    class Box<T>

### Generic method?

    <T> T method(T value)

### Raw type?

    List

### Parameterized type?

    List<String>

### Type erasure?

    Generic type information is mostly removed at runtime.

### Unbounded T erasure?

    Object

### Bounded T erasure?

    First bound

### Bridge method?

    Compiler-generated method preserving polymorphism.

### Bridge reflection method?

    isBridge()

### Synthetic reflection method?

    isSynthetic()

### Generic array?

    Cannot directly create new T[]

### Generic object?

    Cannot directly create new T()

### Static T field?

    Not allowed

### PECS?

    Producer Extends, Consumer Super

### Generic inheritance?

    Supported

### Generic variance?

    Java Generics are invariant

### Diamond operator?

    <>

### Heap pollution?

    Parameterized reference and actual object type become inconsistent.

---

# 42. 30-Second Interview Answer

> **Java Generics allow us to write reusable code while maintaining compile-time type safety. They are used with classes, interfaces, methods, and collections. Important concepts include type parameters, wildcards, bounded wildcards, and PECS — Producer Extends and Consumer Super. Java Generics are implemented mainly through type erasure, so generic type arguments are mostly removed at runtime. Because erasure can cause method-signature differences, the compiler may generate bridge methods to preserve polymorphism. Generics also explain concepts such as raw types, heap pollution, generic arrays, and invariance.**

---

# 43. Cheat Sheet

## Core

    Generics
       ↓
    Reusable + Type Safe Code

---

## Type Parameter

    class Box<T>

`T` = type parameter.

---

## Type Argument

    Box<String>

`String` = type argument.

---

## Generic Method

    static <T> T identity(T value)

---

## Wildcard

    ?

Unknown type.

---

## Upper Bound

    ? extends Number

Think:

    READ / PRODUCER

---

## Lower Bound

    ? super Integer

Think:

    WRITE / CONSUMER

---

## PECS

    Producer → extends
    Consumer → super

---

## Invariance

    List<String>
        ≠
    List<Object>

---

## Raw Type

    List

Avoid unless required for legacy compatibility.

---

## Unbounded Wildcard

    List<?>

Can read as:

    Object

Cannot generally add a specific value.

---

## Type Erasure

    T
    ↓
    Object

For:

    T extends Number

the erasure is:

    Number

---

## Bridge Method

    Erasure
       ↓
    Signature mismatch
       ↓
    Compiler generates bridge
       ↓
    Polymorphism preserved

---

## Generic Array

Invalid:

    new T[10]

---

## Generic Object

Invalid:

    new T()

---

## Static Generic Field

Invalid:

    static T value;

---

## Static Generic Method

Valid:

    static <T> T identity(T value)

---

## Diamond Operator

    List<String> list =
            new ArrayList<>();

---

## Heap Pollution

Usually associated with:

    Raw types
    Unsafe casts
    Unchecked operations
    Generic varargs

---

## Reifiable

Examples:

    String
    List
    List<?>
    String[]

Non-reifiable:

    List<String>
    List<Integer>
    T
    T[]

---

## Recursive Bound

    <T extends Comparable<T>>

---

## DSA Relevance

Generics are heavily used in:

    LinkedList<T>
    Stack<T>
    Queue<T>
    TreeNode<T>
    Graph<T>
    PriorityQueue<T>
    HashMap<K, V>
    HashSet<T>
    Comparable<T>
    Comparator<T>

---

# Final Generics Revision

    Generic Class
          ↓
    Generic Method
          ↓
    Type Safety
          ↓
    Wildcards
          ↓
    ? extends
          ↓
    ? super
          ↓
    PECS
          ↓
    Type Erasure
          ↓
    Bridge Methods
          ↓
    Heap Pollution
          ↓
    Interview Questions
          ↓
    DSA Applications

---

# One-Line Memory Tricks

    T  → Named type parameter

    ?  → Unknown type

    extends → Producer

    super → Consumer

    Generic → Compile-time type safety

    Erasure → Runtime generic information mostly removed

    Bridge → Preserves polymorphism after erasure

    List<String> ≠ List<Object>

    List<?> → Unknown list type

    List<Object> → Specifically Object list

    new T() → Not allowed

    new T[] → Not allowed

    static T → Not allowed

    <T> method() → Allowed

    PECS → Producer Extends, Consumer Super