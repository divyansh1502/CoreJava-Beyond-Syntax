# 🧬 Java Generics — Interview Questions & Answers

> **A focused collection of Java Generics interview questions with concise, interview-ready answers.**

---

# 📑 Table of Contents

- [1. Generics Basics](#1-generics-basics)
- [2. Generic Classes and Methods](#2-generic-classes-and-methods)
- [3. Type Parameters](#3-type-parameters)
- [4. Wildcards](#4-wildcards)
- [5. Bounded Types](#5-bounded-types)
- [6. Type Erasure](#6-type-erasure)
- [7. Generic Interfaces and Inheritance](#7-generic-interfaces-and-inheritance)
- [8. Tricky Generics Questions](#8-tricky-generics-questions)
- [9. Rapid Generics Revision](#9-rapid-generics-revision)

---

# 1. Generics Basics

## 1. What are Generics in Java?

Generics allow classes, interfaces, and methods to operate on types specified by the programmer.

They provide:

```text
Type safety
Code reusability
Compile-time checking
Reduced explicit casting
```

---

## 2. Why were Generics introduced?

Before generics, collections commonly stored `Object`.

This required explicit casting:

```java
Object value =
    list.get(0);

String s =
    (String) value;
```

With generics:

```java
String s =
    list.get(0);
```

The compiler knows the expected type.

---

## 3. What is type safety?

Type safety means preventing incompatible types from being used where a particular type is expected.

Example:

```java
List<String> names =
    new ArrayList<>();
```

The compiler prevents adding an integer:

```java
names.add(100);
```

This results in a compile-time error.

---

## 4. What is a generic type?

A generic type is a class or interface that uses one or more type parameters.

Example:

```java
class Box<T> {

    T value;
}
```

Here:

```text
T
```

is a type parameter.

---

## 5. What is a type parameter?

A type parameter is a placeholder representing a type.

Common naming conventions include:

```text
T → Type
E → Element
K → Key
V → Value
N → Number
R → Result
```

These are conventions, not mandatory names.

---

## 6. What is a parameterized type?

A parameterized type is a generic type supplied with an actual type argument.

Example:

```java
List<String>
```

Here:

```text
List
→ generic type

String
→ type argument
```

---

## 7. What is the difference between type parameter and type argument?

```text
Type parameter
→ placeholder


Type argument
→ actual type supplied
```

Example:

```java
class Box<T> {

}
```

`T` is the type parameter.

```java
Box<String> box;
```

`String` is the type argument.

---

## 8. Can generics work with primitive types?

No.

This is invalid:

```java
List<int> list =
    new ArrayList<>();
```

Use wrapper types:

```java
List<Integer> list =
    new ArrayList<>();
```

Autoboxing and unboxing make this convenient.

---

## 9. Why don't generics support primitive types?

Java generics operate with reference types, while primitive types such as `int` and `double` are not reference types.

Wrapper classes provide the object representation:

```text
int
↓
Integer
```

---

## 10. What are the advantages of Generics?

Main advantages:

```text
Compile-time type safety
Less casting
Reusable code
Cleaner APIs
Better readability
```

---

# 2. Generic Classes and Methods

## 11. How do you create a generic class?

Example:

```java
class Box<T> {

    private T value;

    void set(T value) {
        this.value = value;
    }

    T get() {
        return value;
    }
}
```

---

## 12. How do you use a generic class?

```java
Box<String> box =
    new Box<>();

box.set("Java");

String value =
    box.get();
```

---

## 13. Can a generic class have multiple type parameters?

Yes.

Example:

```java
class Pair<K, V> {

    K key;
    V value;
}
```

Usage:

```java
Pair<Integer, String> pair =
    new Pair<>();
```

---

## 14. What is a generic method?

A generic method declares its own type parameter.

Example:

```java
static <T> void print(T value) {

    System.out.println(value);
}
```

The method's type parameter is independent of the class's type parameters.

---

## 15. Where is the type parameter declared in a generic method?

Before the return type.

Example:

```java
static <T> T identity(T value) {

    return value;
}
```

Here:

```text
<T>
```

appears before:

```text
T
```

the return type.

---

## 16. Can a non-generic class contain a generic method?

Yes.

Example:

```java
class Utility {

    static <T> void print(T value) {

        System.out.println(value);
    }
}
```

The class does not need to be generic.

---

## 17. Can a generic class have a non-generic method?

Yes.

Example:

```java
class Box<T> {

    T value;

    void set(T value) {
        this.value = value;
    }

    void print() {
        System.out.println(value);
    }
}
```

The class is generic, but `print()` does not introduce a new type parameter.

---

## 18. Can static members use a class's type parameter?

No.

A class type parameter belongs to an instance-level parameterization, while static members belong to the class itself.

For example, this is invalid:

```java
class Test<T> {

    static T value;
}
```

A static member cannot directly use the class's `T`.

A static method can declare its own type parameter:

```java
class Test<T> {

    static <E> void print(E value) {

        System.out.println(value);
    }
}
```

---

## 19. Can constructors be generic?

Yes.

A constructor can declare its own type parameter.

Example:

```java
class Box<T> {

    <E> Box(E value) {

        System.out.println(value);
    }
}
```

---

## 20. Can a generic class extend another generic class?

Yes.

Example:

```java
class Parent<T> {

}

class Child<T>
    extends Parent<T> {

}
```

---

# 3. Type Parameters

## 21. Can a type parameter have multiple bounds?

Yes.

Example:

```java
<T extends Number & Comparable<T>>
```

The class bound, if present, must come first, followed by interface bounds.

---

## 22. What does `extends` mean in a generic bound?

In generic bounds, `extends` means that the type must be a subtype of the specified bound.

It is used for both class and interface bounds.

Example:

```java
<T extends Number>
```

means `T` must be `Number` or a subclass of `Number`.

---

## 23. Can a generic type parameter use `super` directly?

No.

Type parameters use:

```text
extends
```

for upper bounds.

`super` is used with wildcards.

---

## 24. What is a bounded type parameter?

A bounded type parameter restricts which types can be used.

Example:

```java
<T extends Number>
```

Only `Number` and its subclasses can be used as `T`.

---

## 25. What is an unbounded type parameter?

An unbounded type parameter has no explicit restriction.

Example:

```java
<T>
```

It can represent any reference type.

---

# 4. Wildcards

## 26. What is a wildcard?

A wildcard represents an unknown type.

It is written as:

```text
?
```

Example:

```java
List<?> list;
```

---

## 27. What does `List<?>` mean?

It means:

```text
A List of some unknown type
```

The exact type is unknown to the method using it.

---

## 28. Can you add elements to `List<?>`?

Generally, no specific non-null value can be safely added.

This is invalid:

```java
List<?> list =
    new ArrayList<String>();

list.add("Java");
```

The compiler does not know the actual element type.

The exception is:

```text
null
```

because null is compatible with every reference type.

---

## 29. What is an unbounded wildcard?

```java
List<?> list;
```

The `?` has no upper bound other than `Object`.

---

## 30. What is an upper-bounded wildcard?

An upper-bounded wildcard uses:

```text
extends
```

Example:

```java
List<? extends Number>
```

It represents a list whose element type is `Number` or a subtype of `Number`.

---

## 31. What is a lower-bounded wildcard?

A lower-bounded wildcard uses:

```text
super
```

Example:

```java
List<? super Integer>
```

It represents a list whose element type is `Integer` or a supertype of `Integer`.

---

## 32. What does `List<? extends Number>` mean?

It could represent:

```text
List<Integer>
List<Double>
List<Float>
List<Number>
```

The exact type is unknown.

You can safely read elements as `Number`.

---

## 33. What does `List<? super Integer>` mean?

It could represent:

```text
List<Integer>
List<Number>
List<Object>
```

You can safely add an `Integer`.

---

## 34. Why can't you add to `List<? extends Number>`?

Suppose:

```text
List<? extends Number>
```

could actually be:

```text
List<Integer>
```

Adding a `Double` would be unsafe.

Therefore, Java prevents adding arbitrary values.

---

## 35. Why can you add Integer to `List<? super Integer>`?

The actual list type is guaranteed to be `Integer` or one of its supertypes.

Therefore, an `Integer` can safely be inserted.

---

## 36. What is PECS?

PECS stands for:

```text
Producer Extends
Consumer Super
```

Use:

```text
? extends
```

when a structure produces values for you.

Use:

```text
? super
```

when a structure consumes values from you.

---

## 37. Give a simple PECS example.

Producer:

```java
static double sum(
    List<? extends Number> list) {

    double total = 0;

    for (Number n : list) {
        total += n.doubleValue();
    }

    return total;
}
```

Consumer:

```java
static void addNumbers(
    List<? super Integer> list) {

    list.add(10);
    list.add(20);
}
```

---

## 38. What is the difference between `List<Object>` and `List<?>`?

They are not the same.

```text
List<Object>
→ specifically a list whose element type is Object


List<?>
→ list of some unknown type
```

A `List<String>` can be assigned to:

```java
List<?> list;
```

but not to:

```java
List<Object> list;
```

---

## 39. Can `List<String>` be assigned to `List<Object>`?

No.

Generics are invariant.

```java
List<String> strings =
    new ArrayList<>();

List<Object> objects =
    strings;
```

This is a compile-time error.

---

## 40. Why are Java generics invariant?

Because allowing covariance for mutable collections would break type safety.

If `List<String>` were a subtype of `List<Object>`, code could insert an Integer into a list that is supposed to contain only Strings.

---

## 41. Is `List<Integer>` a subtype of `List<Number>`?

No.

Even though:

```text
Integer extends Number
```

it does not mean:

```text
List<Integer>
extends
List<Number>
```

---

# 5. Bounded Types

## 42. What is an upper bound?

An upper bound restricts a type to a class or its subclasses.

Example:

```java
<T extends Number>
```

---

## 43. What is a lower bound?

A lower bound restricts a wildcard to a type or its supertypes.

Example:

```java
<? super Integer>
```

---

## 44. Can a wildcard have multiple bounds?

Yes, intersection bounds can be used with wildcards in appropriate type forms, such as:

```text
? extends SomeClass & Interface
```

However, wildcard bounds have more restrictions than type-parameter bounds, and the syntax must follow Java's type rules.

---

## 45. Can a type parameter extend multiple classes?

No.

Java does not support multiple class inheritance.

This is invalid:

```text
<T extends A & B>
```

if both `A` and `B` are classes.

A type parameter can have at most one class bound, followed by interface bounds.

---

## 46. Can a type parameter have multiple interface bounds?

Yes.

Example:

```java
<T extends Comparable<T>
        & Serializable>
```

---

## 47. Can `extends` be used with interfaces in generics?

Yes.

Example:

```java
<T extends Comparable<T>>
```

Even though `Comparable` is an interface, `extends` is used in generic bounds.

---

# 6. Type Erasure

## 48. What is type erasure?

Type erasure is the mechanism by which Java removes most generic type information from runtime representations while preserving type safety through compile-time checks and generated casts where needed.

---

## 49. Why does Java use type erasure?

A major reason is backward compatibility with Java code written before generics were introduced.

Generics were added in Java 5.

---

## 50. Is generic type information completely unavailable at runtime?

Not always.

Some generic type information can remain in class-file metadata and may be accessible through reflection.

However, ordinary generic type arguments are generally erased from the runtime type of objects.

---

## 51. What does `List<String>` become conceptually after erasure?

Conceptually:

```text
List
```

The runtime object does not have a separate class for:

```text
List<String>
```

versus:

```text
List<Integer>
```

---

## 52. Can you create an array of a parameterized type?

Generally, no.

This is invalid:

```java
List<String>[] arr =
    new List<String>[10];
```

Java prevents direct creation of arrays of non-reifiable parameterized types.

---

## 53. Can you create an array of an unbounded wildcard type?

Yes.

For example:

```java
List<?>[] arr =
    new List<?>[10];
```

---

## 54. What is a reifiable type?

A reifiable type is a type whose runtime representation contains enough information to fully represent the type.

Examples include:

```text
String
int
String[]
List<?>
```

Non-reifiable examples include:

```text
List<String>
List<Integer>
```

---

## 55. Can you use `instanceof List<String>`?

No.

This is invalid:

```java
if (obj instanceof List<String>) {

}
```

Because the type argument is erased.

You can use:

```java
if (obj instanceof List<?>) {

}
```

---

## 56. Can you use a generic type with `instanceof`?

Only when the runtime type is reifiable.

For example:

```java
if (obj instanceof List<?>) {

}
```

is valid.

---

## 57. Can a generic type parameter be used with `new`?

Not directly.

This is invalid:

```java
class Box<T> {

    T create() {
        return new T();
    }
}
```

The actual type is not known at runtime in a way that permits direct construction.

---

## 58. Can you create a static variable of type `T`?

No, not when `T` is the class's type parameter.

Example:

```java
class Test<T> {

    static T value;
}
```

is invalid.

---

## 59. Can generic exceptions be created?

A generic class cannot directly extend `Throwable`.

Java does not allow a type parameter to be used as the exception type in the way ordinary generic classes are used.

---

# 7. Generic Interfaces and Inheritance

## 60. Can an interface be generic?

Yes.

Example:

```java
interface Repository<T> {

    void save(T value);
}
```

---

## 61. Can a class implement a generic interface?

Yes.

```java
class StringRepository
    implements Repository<String> {

    public void save(String value) {

        System.out.println(value);
    }
}
```

---

## 62. Can a generic class implement a generic interface?

Yes.

```java
class RepositoryImpl<T>
    implements Repository<T> {

    public void save(T value) {

        System.out.println(value);
    }
}
```

---

## 63. Can a child class change the generic type of its parent?

It depends on how inheritance is declared.

Example:

```java
class Parent<T> {

}

class Child
    extends Parent<String> {

}
```

Here, `Child` fixes the parent type to `String`.

---

## 64. Can the same generic interface be implemented with different type arguments?

A class generally cannot implement the same generic interface with different parameterizations.

This would create an inheritance conflict.

For example, conceptually:

```text
Runnable<String>
Runnable<Integer>
```

cannot both be implemented by the same class.

---

## 65. What is a raw type?

A raw type is a generic type used without specifying its type argument.

Example:

```java
List list =
    new ArrayList();
```

Instead of:

```java
List<String> list =
    new ArrayList<>();
```

---

## 66. Why should raw types be avoided?

They remove much of the compile-time type safety provided by generics and can produce unchecked warnings.

---

## 67. What is an unchecked warning?

It is a compiler warning indicating that the compiler cannot fully verify type safety.

Example:

```java
List list =
    new ArrayList();

List<String> names =
    list;
```

Operations involving raw types can trigger unchecked warnings.

---

## 68. What is a raw type vs wildcard?

```text
Raw type
→ List


Wildcard
→ List<?>
```

`List<?>` retains generic type information in the form of an unknown type and is generally safer than the raw type.

---

# 8. Tricky Generics Questions

## 69. What is the output?

```java
List<String> a =
    new ArrayList<>();

List<?> b =
    a;

System.out.println(
    b.size()
);
```

This is valid because `List<?>` can reference a List of any type.

---

## 70. Is this valid?

```java
List<Number> list =
    new ArrayList<Integer>();
```

No.

Generics are invariant.

---

## 71. Is this valid?

```java
List<?> list =
    new ArrayList<String>();
```

Yes.

`List<?>` can refer to a List of any reference type.

---

## 72. Is this valid?

```java
List<? extends Number> list =
    new ArrayList<Integer>();
```

Yes.

`Integer` extends `Number`.

---

## 73. Is this valid?

```java
List<? super Integer> list =
    new ArrayList<Number>();
```

Yes.

`Number` is a supertype of `Integer`.

---

## 74. Can you add Integer to `List<? super Integer>`?

Yes.

```java
list.add(10);
```

---

## 75. Can you add Double to `List<? super Integer>`?

No.

The actual list could be:

```text
List<Integer>
```

so inserting a Double would not be safe.

---

## 76. Can you add Integer to `List<? extends Number>`?

No.

The actual list could be:

```text
List<Double>
```

Therefore, Java prevents arbitrary insertion.

---

## 77. What can you safely read from `List<? extends Number>`?

You can safely read elements as:

```text
Number
```

because every possible element type is a subtype of Number.

---

## 78. What can you safely read from `List<? super Integer>`?

The only universally safe value type is:

```text
Object
```

because the actual list could be:

```text
List<Integer>
List<Number>
List<Object>
```

---

## 79. Why does PECS work?

Because:

```text
extends
→ gives you values safely as the upper-bound type


super
→ accepts values safely as the lower-bound type
```

---

## 80. Can a generic method be overloaded only by changing its type parameter?

No.

Type parameters are erased, so methods that would have the same erased signature cannot coexist merely because their generic declarations differ.

---

## 81. Can generic methods be overloaded?

Yes, provided their resulting signatures are distinguishable.

Generics alone cannot be used to create two methods with the same erased signature.

---

## 82. What is type inference with the diamond operator?

The diamond operator:

```text
<>
```

allows the compiler to infer generic type arguments from context.

Example:

```java
List<String> names =
    new ArrayList<>();
```

The compiler infers:

```text
String
```

for the constructor.

---

## 83. What is the diamond operator?

It is the `<>` syntax introduced in Java 7 for reducing redundant generic type arguments during object creation.

---

## 84. Can generic classes have static methods?

Yes.

But static methods cannot directly use the class's type parameter.

They can declare their own:

```java
static <T> void print(T value) {

}
```

---

## 85. What is the difference between `T extends Number` and `? extends Number`?

```text
<T extends Number>
→ type parameter
→ introduces a named type


<? extends Number>
→ wildcard
→ represents an unknown subtype
```

---

## 86. When should you use a type parameter instead of a wildcard?

Use a type parameter when the same type needs to be related across multiple positions.

Example:

```java
static <T> T first(
    List<T> list) {

    return list.get(0);
}
```

A wildcard is useful when the exact type relationship does not need to be named.

---

## 87. Can a wildcard be used as a method type parameter?

No.

`?` is a wildcard used in parameterized types.

A type parameter is declared using a name such as:

```text
T
```

---

# 9. Rapid Generics Revision

## 88. What is Generic?

```text
A mechanism for type-safe reusable code.
```

---

## 89. What is T?

Conventionally:

```text
Type
```

---

## 90. What is E?

Conventionally:

```text
Element
```

---

## 91. What is K?

Conventionally:

```text
Key
```

---

## 92. What is V?

Conventionally:

```text
Value
```

---

## 93. Can generics use primitive types?

```text
No
```

Use wrapper classes.

---

## 94. What is `<?>`?

```text
Unknown type
```

---

## 95. What is `<? extends T>`?

```text
Unknown subtype of T
```

---

## 96. What is `<? super T>`?

```text
Unknown supertype of T
```

---

## 97. What does PECS mean?

```text
Producer Extends
Consumer Super
```

---

## 98. Is `List<Integer>` a subtype of `List<Number>`?

```text
No
```

---

## 99. Is `List<Integer>` assignable to `List<?>`?

```text
Yes
```

---

## 100. Is `List<Integer>` assignable to `List<? extends Number>`?

```text
Yes
```

---

## 101. Is `List<Number>` assignable to `List<? super Integer>`?

```text
Yes
```

---

## 102. What is type erasure?

```text
Generic type information is largely removed
from the runtime representation.
```

---

## 103. Why does Java use type erasure?

```text
Backward compatibility
```

is a major reason.

---

## 104. Can you do this?

```java
new T();
```

```text
No
```

---

## 105. Can you do this?

```java
obj instanceof List<String>
```

```text
No
```

---

## 106. Can you do this?

```java
obj instanceof List<?>
```

```text
Yes
```

---

## 107. Can you create `new T[10]`?

```text
No
```

---

## 108. Can generic classes have static fields using T?

```text
No
```

---

## 109. What is a raw type?

```text
A generic type used without type arguments.
```

Example:

```text
List
```

---

## 110. Raw type vs wildcard?

```text
List
→ raw
→ less type-safe


List<?>
→ unknown generic type
→ type-safe
```

---

## 111. What is Comparable's generic form?

```text
Comparable<T>
```

---

## 112. What is Comparator's generic form?

```text
Comparator<T>
```

---

## 113. Main benefit of Generics?

```text
Compile-time type safety
```

---

# 🧠 Generics Memory Map

```text
                         GENERICS
                            |
              +-------------+-------------+
              |             |             |
          Type Params    Wildcards    Type Erasure
              |             |             |
          <T>           <? extends T>    Runtime
          <K,V>         <? super T>      limitations
              |             |
              |            PECS
              |             |
              |      +------+------+
              |      |             |
              |   Producer      Consumer
              |    extends        super
              |
        Bounded Types
              |
       <T extends Number>
```

---

# ⚡ Most Important Generics Comparisons

```text
Type Parameter
vs
Type Argument


<T>
vs
<?>


<T extends Number>
vs
<? extends Number>


<? extends T>
vs
<? super T>


List<T>
vs
List<?>


List<Object>
vs
List<?>


Raw Type
vs
Wildcard


Comparable<T>
vs
Comparator<T>


Generic Class
vs
Generic Method


Compile Time
vs
Type Erasure Runtime Behavior
```

---

