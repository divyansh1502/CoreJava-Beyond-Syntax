# 🎯 Functional Programming Interview Questions in Java

> This file contains important **interview questions and answers** covering Functional Interfaces, Lambda Expressions, Method References, Default Methods, Static Methods, and core Java functional-programming concepts.

---

## 📌 Table of Contents

1. [Introduction](#-introduction)
2. [Functional Programming Basics](#-functional-programming-basics)
3. [Functional Interface Questions](#-functional-interface-questions)
4. [Lambda Expression Questions](#-lambda-expression-questions)
5. [Method Reference Questions](#-method-reference-questions)
6. [Default and Static Method Questions](#-default-and-static-method-questions)
7. [Advanced Questions](#-advanced-questions)
8. [Code-Based Questions](#-code-based-questions)
9. [Tricky Interview Questions](#-tricky-interview-questions)
10. [DSA Connection](#-dsa-connection)
11. [30-Second Revision](#-30-second-revision)
12. [Top 25 Interview Questions](#-top-25-interview-questions)
13. [Final Cheat Sheet](#-final-cheat-sheet)

---

# 🚀 Introduction

Java introduced several functional-programming features in **Java 8**.

The most important concepts are:

```text
Functional Interface
        ↓
Lambda Expression
        ↓
Method Reference
        ↓
Functional Programming
        ↓
Collections + Streams + APIs
```

Important Java functional interfaces are available mainly in:

```java
java.util.function
```

Common interfaces include:

```text
Predicate
Function
Consumer
Supplier
UnaryOperator
BinaryOperator
```

---

# 🧠 Functional Programming Basics

## Q1. What is Functional Programming?

Functional Programming is a programming style where computation is expressed using functions and behavior can be passed around as values.

In Java, functional programming is supported through:

- Lambda Expressions
- Functional Interfaces
- Method References
- Streams
- Higher-order operations
- Immutable-style programming

Example:

```java
Function<Integer, Integer> square = number -> number * number;

System.out.println(square.apply(5));
```

Output:

```text
25
```

---

## Q2. Is Java a purely functional programming language?

**No.**

Java is primarily an **object-oriented, multi-paradigm programming language**.

It supports functional programming features, but it is not a purely functional language.

Java supports:

```text
Object-Oriented Programming
Functional Programming
Imperative Programming
Generic Programming
```

---

# 🔗 Functional Interface Questions

## Q3. What is a Functional Interface?

A functional interface is an interface that contains exactly **one abstract method**.

Example:

```java
@FunctionalInterface
interface Calculator {

    int calculate(int a, int b);
}
```

It can be used with a Lambda:

```java
Calculator calculator = (a, b) -> a + b;

System.out.println(calculator.calculate(10, 20));
```

Output:

```text
30
```

---

## Q4. What is the purpose of `@FunctionalInterface`?

`@FunctionalInterface` tells the compiler that an interface is intended to be a functional interface.

Example:

```java
@FunctionalInterface
interface Calculator {

    int calculate(int a, int b);
}
```

If we accidentally add another abstract method:

```java
@FunctionalInterface
interface Calculator {

    int calculate(int a, int b);

    int subtract(int a, int b);
}
```

The compiler reports an error.

### Important

`@FunctionalInterface` is not mandatory.

This is still a valid functional interface:

```java
interface Calculator {

    int calculate(int a, int b);
}
```

The annotation simply provides compiler validation and communicates intent.

---

## Q5. Can a Functional Interface contain default methods?

Yes.

Example:

```java
@FunctionalInterface
interface Calculator {

    int calculate(int a, int b);

    default void info() {
        System.out.println("Calculator");
    }
}
```

It still has only one abstract method.

---

## Q6. Can a Functional Interface contain static methods?

Yes.

```java
@FunctionalInterface
interface Calculator {

    int calculate(int a, int b);

    static void info() {
        System.out.println("Calculator");
    }
}
```

The static method does not count as an abstract method.

---

## Q7. Can a Functional Interface contain private methods?

Yes.

Modern Java interfaces can contain private methods.

```java
@FunctionalInterface
interface Calculator {

    int calculate(int a, int b);

    private void helper() {
        System.out.println("Helper");
    }
}
```

The private method does not count as an abstract method.

---

## Q8. Can methods inherited from `Object` affect functional-interface status?

Methods that correspond to public methods of `Object` do not make an interface non-functional merely because they are declared abstract in the interface.

For example:

```java
@FunctionalInterface
interface Task {

    void execute();

    boolean equals(Object obj);
}
```

The `equals(Object)` declaration corresponds to a public method of `Object`, so the interface can still qualify as functional based on its single independent abstract method.

---

# ⚡ Lambda Expression Questions

## Q9. What is a Lambda Expression?

A Lambda Expression is a concise expression that provides an implementation for a compatible functional interface.

Example:

```java
Runnable task = () -> System.out.println("Running");

task.run();
```

---

## Q10. When were Lambda Expressions introduced?

Lambda Expressions were introduced in **Java 8**.

---

## Q11. What is the basic syntax of Lambda?

```java
(parameters) -> expression
```

Or:

```java
(parameters) -> {
    statements;
}
```

Example:

```java
(a, b) -> a + b
```

---

## Q12. Can Lambda expressions exist without a target type?

A Lambda expression needs a compatible **target type**.

For normal Java Lambda usage, that target is commonly a functional interface.

Example:

```java
Function<Integer, Integer> square = number -> number * number;
```

The compiler uses `Function<Integer, Integer>` to determine the Lambda's parameter and return types.

---

## Q13. Can a Lambda have zero parameters?

Yes.

Use empty parentheses:

```java
() -> System.out.println("Hello");
```

---

## Q14. Can a Lambda have one parameter?

Yes.

Parentheses can be omitted:

```java
name -> System.out.println(name);
```

Or written explicitly:

```java
(name) -> System.out.println(name);
```

---

## Q15. Can a Lambda have multiple parameters?

Yes.

Parentheses are required:

```java
(a, b) -> a + b
```

---

## Q16. Can Lambda parameters have explicit types?

Yes.

```java
(int a, int b) -> a + b
```

But parameter types must be specified consistently.

Invalid:

```java
(int a, b) -> a + b
```

Valid:

```java
(int a, int b) -> a + b
```

---

## Q17. What is type inference in Lambda expressions?

Java can infer Lambda parameter types from the target functional interface.

Example:

```java
Function<Integer, Integer> square = number -> number * number;
```

The compiler knows that:

```text
number → Integer
```

because of:

```java
Function<Integer, Integer>
```

---

## Q18. What is the difference between expression body and block body?

Expression body:

```java
(a, b) -> a + b
```

The result is implicitly returned.

Block body:

```java
(a, b) -> {
    int result = a + b;
    return result;
}
```

The return statement must be explicit.

---

## Q19. Can a Lambda contain multiple statements?

Yes.

Use braces:

```java
Runnable task = () -> {
    System.out.println("Starting");
    System.out.println("Running");
};
```

---

# 🔒 Lambda Variable Questions

## Q20. What is an effectively final variable?

A local variable is effectively final if its value is not changed after initialization.

Example:

```java
int number = 10;

Runnable task = () -> System.out.println(number);

task.run();
```

`number` is effectively final.

---

## Q21. Can Lambda modify a local variable?

No, not directly if that local variable is captured by the Lambda.

Invalid:

```java
int count = 0;

Runnable task = () -> {
    count++;
};
```

The local variable must be final or effectively final.

---

## Q22. Why must captured local variables be final or effectively final?

Local variables live in the method's local execution context, while a Lambda may outlive the immediate execution of that method.

Java therefore captures the value in a way that avoids mutable local-variable capture semantics.

The restriction ensures that the captured local variable does not change unexpectedly.

---

# 🎯 Lambda and `this`

## Q23. What does `this` mean inside a Lambda?

Inside a Lambda, `this` refers to the enclosing instance.

Example:

```java
class Demo {

    private String name = "Java";

    void show() {

        Runnable task = () -> {
            System.out.println(this.name);
        };

        task.run();
    }
}
```

Output:

```text
Java
```

---

# 🔗 Method Reference Questions

## Q24. What is a Method Reference?

A Method Reference is a shorthand syntax for referring to an existing method or constructor.

It uses:

```java
::
```

Example:

```java
Consumer<String> printer = System.out::println;
```

---

## Q25. What are the four types of Method References?

### 1. Static method

```java
Math::abs
```

### 2. Instance method of a particular object

```java
System.out::println
```

### 3. Instance method of an arbitrary object

```java
String::length
```

### 4. Constructor

```java
ArrayList::new
```

---

## Q26. Lambda vs Method Reference?

Lambda:

```java
name -> System.out.println(name)
```

Method reference:

```java
System.out::println
```

A method reference is generally useful when the Lambda simply delegates to an existing method.

---

## Q27. Does a Method Reference immediately execute the method?

No.

This:

```java
System.out::println
```

does not print anything by itself.

It creates a method reference.

The referenced method is invoked later through the target functional interface.

---

## Q28. What is a Constructor Reference?

A constructor reference uses:

```java
ClassName::new
```

Example:

```java
Supplier<ArrayList<String>> creator = ArrayList::new;

ArrayList<String> list = creator.get();
```

Equivalent Lambda:

```java
Supplier<ArrayList<String>> creator = () -> new ArrayList<>();
```

---

# ⚙️ Default Method Questions

## Q29. What is a default method?

A default method is an instance method inside an interface that contains an implementation.

Example:

```java
interface Vehicle {

    default void stop() {
        System.out.println("Vehicle stops");
    }
}
```

---

## Q30. Why were default methods introduced?

One major purpose was to allow interfaces to evolve without requiring every existing implementation to immediately implement newly added abstract methods.

Example:

```java
interface Vehicle {

    void start();

    default void stop() {
        System.out.println("Vehicle stops");
    }
}
```

---

## Q31. Can default methods be overridden?

Yes.

```java
interface Vehicle {

    default void stop() {
        System.out.println("Vehicle stops");
    }
}
```

```java
class Car implements Vehicle {

    @Override
    public void stop() {
        System.out.println("Car stops");
    }
}
```

---

## Q32. What happens if two interfaces contain the same default method?

The implementing class gets a conflict and must resolve it.

```java
interface A {

    default void show() {
        System.out.println("A");
    }
}
```

```java
interface B {

    default void show() {
        System.out.println("B");
    }
}
```

```java
class Demo implements A, B {

    @Override
    public void show() {
        A.super.show();
    }
}
```

---

## Q33. Which has priority: class method or interface default method?

A class method has priority over an interface default method.

Example:

```java
class Parent {

    public void show() {
        System.out.println("Parent");
    }
}
```

```java
interface A {

    default void show() {
        System.out.println("A");
    }
}
```

```java
class Child extends Parent implements A {
}
```

Calling:

```java
new Child().show();
```

uses:

```text
Parent.show()
```

---

# 🧱 Static Interface Method Questions

## Q34. Can an interface contain static methods?

Yes.

Example:

```java
interface Calculator {

    static int add(int a, int b) {
        return a + b;
    }
}
```

Call:

```java
int result = Calculator.add(10, 20);
```

---

## Q35. Are static interface methods inherited?

No.

They belong to the interface itself.

Correct:

```java
Calculator.add(10, 20);
```

Not:

```java
Demo.add(10, 20);
```

when `Demo` merely implements `Calculator`.

---

## Q36. Can a static interface method be overridden?

No.

Static methods are not polymorphic instance methods.

They belong to the interface that declares them.

---

# 🧠 Built-in Functional Interface Questions

## Q37. What is `Predicate<T>`?

`Predicate<T>` accepts one input and returns a boolean.

Its main method is:

```java
boolean test(T t);
```

Example:

```java
Predicate<Integer> even = number -> number % 2 == 0;

System.out.println(even.test(10));
```

Output:

```text
true
```

---

## Q38. What is `Function<T, R>`?

`Function<T, R>` accepts a value of type `T` and returns a value of type `R`.

Its main method is:

```java
R apply(T t);
```

Example:

```java
Function<Integer, Integer> square = number -> number * number;

System.out.println(square.apply(5));
```

Output:

```text
25
```

---

## Q39. What is `Consumer<T>`?

`Consumer<T>` accepts one input and returns no result.

Its main method is:

```java
void accept(T t);
```

Example:

```java
Consumer<String> printer = message -> System.out.println(message);

printer.accept("Java");
```

---

## Q40. What is `Supplier<T>`?

`Supplier<T>` takes no input and produces a result.

Its main method is:

```java
T get();
```

Example:

```java
Supplier<String> supplier = () -> "Java";

System.out.println(supplier.get());
```

---

## Q41. Difference between Predicate, Function, Consumer and Supplier?

| Interface | Input | Output | Main Method |
|---|---|---|---|
| `Predicate<T>` | T | boolean | `test()` |
| `Function<T,R>` | T | R | `apply()` |
| `Consumer<T>` | T | void | `accept()` |
| `Supplier<T>` | None | T | `get()` |

### Memory Trick

```text
Predicate
T → boolean

Function
T → R

Consumer
T → nothing

Supplier
nothing → T
```

---

# 🧮 UnaryOperator and BinaryOperator

## Q42. What is `UnaryOperator<T>`?

It accepts one value and returns the same type.

```java
UnaryOperator<Integer> square = number -> number * number;

System.out.println(square.apply(5));
```

Output:

```text
25
```

Conceptually:

```text
T → T
```

---

## Q43. What is `BinaryOperator<T>`?

It accepts two values of the same type and returns that same type.

```java
BinaryOperator<Integer> addition = (a, b) -> a + b;

System.out.println(addition.apply(10, 20));
```

Conceptually:

```text
T, T → T
```

---

# 🌊 Functional Programming and Streams

## Q44. How are Lambda expressions used with Streams?

Lambdas provide behavior to Stream operations.

Example:

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6);

numbers.stream()
        .filter(number -> number % 2 == 0)
        .forEach(number -> System.out.println(number));
```

Output:

```text
2
4
6
```

Here:

```java
number -> number % 2 == 0
```

is used by `filter()`.

And:

```java
number -> System.out.println(number)
```

is used by `forEach()`.

---

# 🔥 Code-Based Questions

## Q45. What is the output?

```java
Function<Integer, Integer> square = x -> x * x;

System.out.println(square.apply(5));
```

### Answer

```text
25
```

---

## Q46. What is the output?

```java
Predicate<Integer> predicate = x -> x > 10;

System.out.println(predicate.test(5));
System.out.println(predicate.test(20));
```

### Answer

```text
false
true
```

---

## Q47. What is the output?

```java
Consumer<String> consumer = System.out::println;

consumer.accept("Java");
```

### Answer

```text
Java
```

---

## Q48. What is the output?

```java
Supplier<Integer> supplier = () -> 100;

System.out.println(supplier.get());
```

### Answer

```text
100
```

---

## Q49. What is the output?

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4);

numbers.forEach(number -> {
    if (number % 2 == 0) {
        System.out.println(number);
    }
});
```

### Answer

```text
2
4
```

---

# 🪤 Tricky Interview Questions

## Q50. Is a Lambda an object?

A Lambda expression is an expression that can be evaluated to an instance of a functional interface target type.

For interview purposes, the important distinction is:

```text
Lambda Expression
        ↓
Provides behavior
        ↓
Target functional interface
```

---

## Q51. Can one Lambda implement two different functional interfaces?

The same Lambda expression can be compatible with different functional-interface target types when their function descriptors are compatible.

Example:

```java
Predicate<String> predicate = text -> text.isEmpty();

Function<String, Boolean> function = text -> text.isEmpty();
```

The Lambda body is similar, but the target types are different.

---

## Q52. Can a Lambda throw exceptions?

Yes, but checked exceptions must be compatible with the abstract method's `throws` declaration.

Example:

```java
@FunctionalInterface
interface Task {
    void execute() throws Exception;
}
```

Then:

```java
Task task = () -> {
    throw new Exception("Something went wrong");
};
```

---

## Q53. Can a Lambda access instance variables?

Yes.

```java
class Demo {

    private int number = 10;

    void show() {

        Runnable task = () -> {
            System.out.println(number);
        };

        task.run();
    }
}
```

The Lambda can access instance state through the enclosing object.

---

## Q54. Can a Lambda access static variables?

Yes.

```java
class Demo {

    static int number = 10;

    public static void main(String[] args) {

        Runnable task = () -> {
            System.out.println(number);
        };

        task.run();
    }
}
```

---

## Q55. Can a Lambda modify an instance variable?

Yes.

The effectively-final restriction applies to captured **local variables**, not fields.

Example:

```java
class Counter {

    private int count = 0;

    void increment() {

        Runnable task = () -> {
            count++;
        };

        task.run();
    }
}
```

The field can be modified.

---

# ⚙️ Internal Working Questions

## Q56. How does Java implement Lambda expressions internally?

Modern Java uses mechanisms involving:

```text
invokedynamic
LambdaMetafactory
```

rather than simply generating an ordinary anonymous inner class for every Lambda.

Conceptually:

```text
Lambda Source
      ↓
Compiler
      ↓
invokedynamic
      ↓
LambdaMetafactory
      ↓
Functional Interface Implementation
```

---

## Q57. Is Lambda the same as an anonymous inner class?

No.

Although both can provide behavior for an interface, their language semantics differ.

One important difference is `this`.

### Lambda

```java
this
```

refers to the enclosing instance.

### Anonymous Class

```java
this
```

refers to the anonymous class instance.

---

# 🔥 Advanced Questions

## Q58. What is a target type?

A target type is the type Java uses to determine the expected type of a Lambda or method reference.

Example:

```java
Function<Integer, Integer> square = x -> x * x;
```

Here:

```java
Function<Integer, Integer>
```

is the target type.

It tells the compiler:

```text
Input  → Integer
Output → Integer
```

---

## Q59. What is a function descriptor?

The function descriptor describes the abstract method signature of a functional interface.

For:

```java
Function<Integer, String>
```

the abstract method is conceptually:

```java
String apply(Integer value);
```

Therefore, the Lambda must be compatible with:

```text
Integer → String
```

---

## Q60. What is the relationship between Lambda and functional interface?

Think:

```text
Functional Interface
        ↓
Defines the contract
        ↓
Lambda
        ↓
Provides the implementation
```

Example:

```java
@FunctionalInterface
interface Calculator {
    int calculate(int a, int b);
}
```

Lambda:

```java
Calculator calculator = (a, b) -> a + b;
```

---

# 🧠 Functional Programming Design Questions

## Q61. What does "passing behavior as an argument" mean?

Instead of passing only data:

```java
calculate(10, 20);
```

we can pass an operation:

```java
calculate(10, 20, (a, b) -> a + b);
```

Example:

```java
static void operate(
        int a,
        int b,
        BinaryOperator<Integer> operation) {

    System.out.println(operation.apply(a, b));
}
```

Usage:

```java
operate(10, 20, (a, b) -> a + b);

operate(10, 20, (a, b) -> a * b);
```

The method stays the same while the behavior changes.

---

# 🧠 DSA Connection

Functional programming features appear frequently in Java DSA solutions.

## 1. Sorting

```java
Arrays.sort(numbers, (a, b) -> Integer.compare(a, b));
```

## 2. Custom Object Sorting

```java
students.sort(
        (a, b) -> Integer.compare(a.getMarks(), b.getMarks())
);
```

## 3. Filtering

```java
numbers.removeIf(number -> number % 2 == 0);
```

## 4. Traversal

```java
numbers.forEach(System.out::println);
```

## 5. Priority Queue Ordering

```java
PriorityQueue<Integer> queue =
        new PriorityQueue<>((a, b) -> Integer.compare(b, a));
```

This creates a max-heap ordering.

---

# 🧠 How to Think in DSA Interviews

When you see:

> "Sort using a custom condition."

Think:

```java
(a, b) -> comparison
```

When you see:

> "Keep only elements satisfying a condition."

Think:

```java
x -> condition
```

When you see:

> "Transform each element."

Think:

```java
x -> transformedValue
```

When you see:

> "Perform an action on every element."

Think:

```java
x -> action(x)
```

---

# 📊 Functional Interface Selection Cheat Sheet

```text
Need boolean?
       ↓
Predicate<T>
```

```text
Need transformation?
       ↓
Function<T, R>
```

```text
Need side effect / action?
       ↓
Consumer<T>
```

```text
Need value without input?
       ↓
Supplier<T>
```

```text
Need T → T?
       ↓
UnaryOperator<T>
```

```text
Need T,T → T?
       ↓
BinaryOperator<T>
```

---

# 🧠 Most Important Interview Traps

### Trap 1

```java
Predicate<Integer>
```

does not return an `Integer`.

It returns:

```text
boolean
```

---

### Trap 2

```java
Consumer<String>
```

does not return a value.

Its return type is:

```text
void
```

---

### Trap 3

```java
Supplier<String>
```

does not accept an input.

It produces a String.

---

### Trap 4

A default method does not count as an abstract method.

---

### Trap 5

A static interface method is not inherited.

---

### Trap 6

Lambda-captured local variables must be final or effectively final.

---

### Trap 7

`this` inside Lambda refers to the enclosing instance.

---

### Trap 8

Method reference:

```java
System.out::println
```

is not the same as:

```java
System.out.println(...)
```

The first references behavior; the second invokes a method.

---

### Trap 9

Lambda expressions require a compatible target type.

---

### Trap 10

Lambda implementation is not simply:

```text
Lambda = Anonymous Class
```

Modern Java uses `invokedynamic`-based infrastructure.

---

# ⚡ Rapid-Fire Revision

### Java 8 introduced?

```text
Lambda Expressions
Method References
Default Methods
Static Interface Methods
Stream API
Functional Interfaces in java.util.function
```

### Functional Interface?

```text
Exactly one abstract method
```

### Lambda?

```text
Concise implementation of a functional interface
```

### Method Reference?

```text
Shorthand for referring to an existing method/constructor
```

### Default Method?

```text
Interface instance method with implementation
```

### Static Interface Method?

```text
Interface-level method, not inherited
```

### Predicate?

```text
T → boolean
```

### Function?

```text
T → R
```

### Consumer?

```text
T → void
```

### Supplier?

```text
() → T
```

---

# 🎯 Top 25 Interview Questions

## 1. What is functional programming?

A programming style where computation is expressed using functions and behavior can be treated as a value.

---

## 2. Is Java purely functional?

No. Java is a multi-paradigm language with strong object-oriented foundations and functional programming support.

---

## 3. What is a functional interface?

An interface containing exactly one abstract method.

---

## 4. Is `@FunctionalInterface` mandatory?

No. It is optional but useful for compiler validation and documentation.

---

## 5. Can functional interfaces have default methods?

Yes.

---

## 6. Can functional interfaces have static methods?

Yes.

---

## 7. Can functional interfaces have private methods?

Yes, in modern Java.

---

## 8. What is a Lambda expression?

A concise expression that provides an implementation for a compatible functional interface.

---

## 9. What is the syntax of Lambda?

```java
(parameters) -> expression
```

---

## 10. What is a method reference?

A shorthand syntax for referencing an existing method or constructor using `::`.

---

## 11. What are the four types of method references?

```text
Static method
Particular object instance method
Arbitrary object instance method
Constructor
```

---

## 12. What is a default method?

An interface instance method with an implementation.

---

## 13. Why were default methods introduced?

Primarily to help evolve interfaces while maintaining compatibility with existing implementations.

---

## 14. Can default methods be overridden?

Yes.

---

## 15. Can static interface methods be overridden?

No.

---

## 16. Are static interface methods inherited?

No.

---

## 17. What happens when two interfaces have conflicting default methods?

The implementing class must resolve the conflict.

---

## 18. Which wins: class method or interface default method?

The class method takes priority.

---

## 19. What is `Predicate<T>`?

A functional interface representing a boolean-valued condition.

---

## 20. What is `Function<T,R>`?

A functional interface representing a transformation from `T` to `R`.

---

## 21. What is `Consumer<T>`?

A functional interface that accepts `T` and returns no result.

---

## 22. What is `Supplier<T>`?

A functional interface that takes no input and supplies a `T`.

---

## 23. What is effectively final?

A local variable that is not modified after initialization and can therefore be captured by a Lambda.

---

## 24. What does `this` mean inside Lambda?

It refers to the enclosing instance.

---

## 25. How does Lambda work internally?

Modern Java uses mechanisms such as `invokedynamic` and `LambdaMetafactory` to create behavior compatible with the target functional interface.

---

# 🏆 Final Functional Programming Cheat Sheet

```text
                    FUNCTIONAL PROGRAMMING
                              │
              ┌───────────────┼───────────────┐
              │               │               │
      Functional         Lambda          Method Reference
       Interface           │               │
              │            │               │
       One abstract     Concise         Existing method
          method        behavior        or constructor
              │            │               │
              └────────────┼───────────────┘
                           │
                     Java 8 Features
                           │
              ┌────────────┼────────────┐
              │            │            │
          Collections    Streams      Threads
```

## Functional Interfaces

```text
Predicate<T>
T → boolean

Function<T,R>
T → R

Consumer<T>
T → void

Supplier<T>
() → T

UnaryOperator<T>
T → T

BinaryOperator<T>
(T,T) → T
```

## Lambda

```java
(parameters) -> expression
```

## Method Reference

```java
ClassName::method
object::method
ClassName::new
```

## Default Method

```java
default void method() {
    // implementation
}
```

## Static Interface Method

```java
static void method() {
    // implementation
}
```

---

# 🎤 Final Interview Summary

> **Java's functional programming support was introduced mainly with Java 8. Functional interfaces provide the target type, Lambda expressions provide concise implementations, and method references provide an even shorter way to reference existing methods or constructors. Default methods allow interfaces to provide reusable instance behavior, while static methods provide interface-level utility behavior. These concepts are heavily used throughout the Collections, Streams, Comparator, and functional APIs.**

---

# 🔥 Final Mental Model

```text
FUNCTIONAL INTERFACE
        ↓
One Abstract Method
        ↓
      LAMBDA
        ↓
Concise Behavior
        ↓
METHOD REFERENCE
        ↓
Reuse Existing Method
        ↓
COLLECTIONS / STREAMS / APIs
```

> **Master these concepts together:**  
> `Functional Interface → Lambda → Method Reference → Default/Static Methods → Streams`