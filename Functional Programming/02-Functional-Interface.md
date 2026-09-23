````md
# 🔗 Functional Interface in Java

> A **Functional Interface** is an interface that contains exactly **one abstract method**, making it suitable as the target type for lambda expressions and method references.

---

# 📚 Table of Contents

> **Functional Interface** is the foundation that allows Java to represent behavior using lambda expressions and method references.

- [1. What is a Functional Interface?](#-1-what-is-a-functional-interface) — Definition and core concept.
- [2. Why Functional Interfaces?](#-2-why-functional-interfaces) — Why Java needs functional interfaces.
- [3. Basic Syntax](#-3-basic-syntax) — How to declare a functional interface.
- [4. Single Abstract Method](#-4-single-abstract-method) — Understanding the SAM rule.
- [5. `@FunctionalInterface` Annotation](#-5-functionalinterface-annotation) — Compile-time verification of the functional interface contract.
- [6. Functional Interface with Lambda](#-6-functional-interface-with-lambda) — Using lambdas with custom functional interfaces.
- [7. Functional Interface with Method Reference](#-7-functional-interface-with-method-reference) — Using method references.
- [8. Default Methods](#-8-default-methods) — Why default methods do not break the functional interface rule.
- [9. Static Methods](#-9-static-methods) — Static interface methods and functional interfaces.
- [10. Object Class Methods](#-10-object-class-methods) — Why Object methods do not count as abstract methods.
- [11. Built-in Functional Interfaces](#-11-built-in-functional-interfaces) — `Predicate`, `Function`, `Consumer`, `Supplier`, and more.
- [12. Predicate](#-12-predicate) — Functional interface for boolean-valued conditions.
- [13. Function](#-13-function) — Functional interface for transforming one value into another.
- [14. Consumer](#-14-consumer) — Functional interface for consuming a value.
- [15. Supplier](#-15-supplier) — Functional interface for supplying a value.
- [16. UnaryOperator](#-16-unaryoperator) — Same input and output type transformation.
- [17. BinaryOperator](#-17-binaryoperator) — Combining two values of the same type.
- [18. BiPredicate](#-18-bipredicate) — Predicate accepting two arguments.
- [19. BiFunction](#-19-bifunction) — Function accepting two arguments.
- [20. BiConsumer](#-20-biconsumer) — Consumer accepting two arguments.
- [21. Primitive Functional Interfaces](#-21-primitive-functional-interfaces) — Avoiding boxing with primitive-specialized interfaces.
- [22. Functional Interface Hierarchy](#-22-functional-interface-hierarchy) — Understanding the relationships between built-in interfaces.
- [23. Internal Working](#-23-internal-working) — How functional interfaces participate in lambda execution.
- [24. Functional Interface vs Normal Interface](#-24-functional-interface-vs-normal-interface) — Key differences.
- [25. Functional Interface vs Abstract Class](#-25-functional-interface-vs-abstract-class) — Comparing both abstraction mechanisms.
- [26. Advantages](#-26-advantages) — Benefits of functional interfaces.
- [27. Common Mistakes](#-27-common-mistakes) — Frequently made mistakes.
- [28. Interview Traps](#-28-interview-traps) — Important interview edge cases.
- [29. 30-Second Interview Answer](#-29-30-second-interview-answer) — Interview-ready explanation.
- [30. Cheat Sheet](#-30-cheat-sheet) — Quick revision.
- [31. Top Interview Questions](#-31-top-interview-questions) — Important interview questions.
- [32. DSA Connection](#-32-dsa-connection) — Functional interfaces in DSA and problem solving.

---

# 🧠 1. What is a Functional Interface?

A **Functional Interface** is an interface that contains exactly **one abstract method**.

It is also called a:

> **SAM Interface — Single Abstract Method Interface**

Example:

```java
@FunctionalInterface
interface Calculator {

    int calculate(int a, int b);
}
````

The interface has only one abstract method:

```java
calculate()
```

Therefore, it is a functional interface.

---

# 🤔 2. Why Functional Interfaces?

Lambda expressions need a **target type**.

Java uses functional interfaces as the target type of lambdas.

Example:

```java
@FunctionalInterface
interface Calculator {

    int calculate(int a, int b);
}
```

Now we can provide the implementation using a lambda:

```java
Calculator addition =
        (a, b) -> a + b;
```

Here:

```text
Lambda
  ↓
Calculator
  ↓
calculate(int, int)
```

The functional interface tells Java:

> "This lambda represents an implementation of this one abstract method."

---

# 📝 3. Basic Syntax

A functional interface can be declared like this:

```java
@FunctionalInterface
interface MyInterface {

    void execute();
}
```

Then:

```java
MyInterface obj =
        () -> System.out.println("Executing");
```

Calling:

```java
obj.execute();
```

Output:

```text
Executing
```

---

# 🔢 4. Single Abstract Method

The most important rule is:

> A functional interface must have exactly **one abstract method**.

Valid:

```java
@FunctionalInterface
interface Printer {

    void print();
}
```

Also valid:

```java
@FunctionalInterface
interface Calculator {

    int calculate(int a, int b);
}
```

Invalid:

```java
@FunctionalInterface
interface Calculator {

    int add(int a, int b);

    int subtract(int a, int b);
}
```

There are two abstract methods:

```text
add()
subtract()
```

Therefore, it cannot be a functional interface.

---

# 🏷️ 5. `@FunctionalInterface` Annotation

Java provides:

```java
@FunctionalInterface
```

to explicitly declare the developer's intention.

Example:

```java
@FunctionalInterface
interface Greeting {

    void greet();
}
```

The annotation is not what makes an interface functional.

Instead:

> The **single abstract method rule** makes it functional.

The annotation asks the compiler to verify that rule.

---

## Without `@FunctionalInterface`

This can still be a valid functional interface:

```java
interface Greeting {

    void greet();
}
```

The annotation is optional.

---

## With `@FunctionalInterface`

```java
@FunctionalInterface
interface Greeting {

    void greet();
}
```

If another abstract method is added:

```java
@FunctionalInterface
interface Greeting {

    void greet();

    void welcome();
}
```

The compiler reports an error because the interface no longer satisfies the functional-interface contract.

---

# 🔥 6. Functional Interface with Lambda

Example:

```java
@FunctionalInterface
interface Calculator {

    int calculate(int a, int b);
}
```

Lambda:

```java
Calculator addition =
        (a, b) -> a + b;
```

Calling:

```java
int result = addition.calculate(10, 20);

System.out.println(result);
```

Output:

```text
30
```

The lambda provides the implementation of:

```java
calculate(int a, int b)
```

---

# 🔗 7. Functional Interface with Method Reference

Functional interfaces can also be used with method references.

Example:

```java
@FunctionalInterface
interface Printer {

    void print(String message);
}
```

Suppose we have:

```java
class MessagePrinter {

    static void printMessage(String message) {
        System.out.println(message);
    }
}
```

We can use:

```java
Printer printer =
        MessagePrinter::printMessage;
```

Then:

```java
printer.print("Hello Java");
```

Output:

```text
Hello Java
```

The method reference provides the implementation of the functional interface method.

---

# 🧩 8. Default Methods

A functional interface can contain **default methods**.

Example:

```java
@FunctionalInterface
interface Calculator {

    int calculate(int a, int b);

    default void printMessage() {
        System.out.println("Calculator");
    }
}
```

This is still a functional interface.

Why?

Because:

```text
calculate() → abstract
printMessage() → default
```

Only `calculate()` is abstract.

Therefore:

```text
Abstract methods = 1
```

---

# ⚙️ 9. Static Methods

A functional interface can also contain static methods.

Example:

```java
@FunctionalInterface
interface Calculator {

    int calculate(int a, int b);

    static void info() {
        System.out.println("Calculator Interface");
    }
}
```

This is valid.

Static methods do not count as abstract methods.

They belong to the interface itself.

Calling:

```java
Calculator.info();
```

---

# 🧬 10. Object Class Methods

A very important interview point:

> Methods that correspond to public methods of `java.lang.Object` do not count toward the single abstract method requirement.

Example:

```java
@FunctionalInterface
interface Example {

    void execute();

    boolean equals(Object obj);
}
```

This can still be considered a functional interface because:

```text
execute() → abstract method
equals()  → corresponds to Object.equals()
```

The `equals()` declaration does not create another independent abstract-method requirement for the functional interface contract.

This is an important interview edge case.

---

# ☕ 11. Built-in Functional Interfaces

Java provides many functional interfaces in:

```text
java.util.function
```

Important ones include:

| Interface           | Purpose                    |
| ------------------- | -------------------------- |
| `Predicate<T>`      | Tests a condition          |
| `Function<T,R>`     | Converts T into R          |
| `Consumer<T>`       | Consumes T                 |
| `Supplier<T>`       | Supplies T                 |
| `UnaryOperator<T>`  | T → T                      |
| `BinaryOperator<T>` | T + T → T                  |
| `BiPredicate<T,U>`  | Tests two values           |
| `BiFunction<T,U,R>` | Converts two values into R |
| `BiConsumer<T,U>`   | Consumes two values        |

---

# 🔍 12. Predicate

`Predicate<T>` represents a condition.

Its abstract method is:

```java
boolean test(T t);
```

Example:

```java
Predicate<Integer> isEven =
        number -> number % 2 == 0;
```

Usage:

```java
System.out.println(isEven.test(10));
```

Output:

```text
true
```

### Memory Trick

> **Predicate → Problem/condition → boolean**

```text
T → boolean
```

---

# 🔄 13. Function

`Function<T, R>` represents a transformation.

Its abstract method is:

```java
R apply(T t);
```

Example:

```java
Function<Integer, Integer> square =
        number -> number * number;
```

Usage:

```java
System.out.println(square.apply(5));
```

Output:

```text
25
```

### Memory Trick

> **Function → converts one value into another**

```text
T → R
```

---

# 📤 14. Consumer

`Consumer<T>` represents an operation that consumes a value.

Its abstract method is:

```java
void accept(T t);
```

Example:

```java
Consumer<String> printer =
        message -> System.out.println(message);
```

Usage:

```java
printer.accept("Hello");
```

Output:

```text
Hello
```

### Memory Trick

> **Consumer → consumes → returns nothing**

```text
T → void
```

---

# 📥 15. Supplier

`Supplier<T>` represents something that supplies a value.

Its abstract method is:

```java
T get();
```

Example:

```java
Supplier<Double> randomValue =
        () -> Math.random();
```

Usage:

```java
System.out.println(randomValue.get());
```

### Memory Trick

> **Supplier → supplies → takes nothing**

```text
() → T
```

---

# 🔁 16. UnaryOperator

`UnaryOperator<T>` represents an operation where:

```text
Input type = Output type
```

It extends:

```java
Function<T, T>
```

Example:

```java
UnaryOperator<Integer> square =
        number -> number * number;
```

Usage:

```java
System.out.println(square.apply(5));
```

Output:

```text
25
```

### Memory Trick

```text
UnaryOperator<T>

T → T
```

---

# ➕ 17. BinaryOperator

`BinaryOperator<T>` represents an operation that accepts two values of the same type and returns the same type.

It extends:

```java
BiFunction<T, T, T>
```

Example:

```java
BinaryOperator<Integer> addition =
        (a, b) -> a + b;
```

Usage:

```java
System.out.println(addition.apply(10, 20));
```

Output:

```text
30
```

### Memory Trick

```text
BinaryOperator<T>

T + T → T
```

---

# 🔍 18. BiPredicate

`BiPredicate<T, U>` accepts two values and returns a boolean.

Its abstract method is:

```java
boolean test(T t, U u);
```

Example:

```java
BiPredicate<Integer, Integer> isGreater =
        (a, b) -> a > b;
```

Usage:

```java
System.out.println(isGreater.test(20, 10));
```

Output:

```text
true
```

### Memory Trick

```text
T + U → boolean
```

---

# 🔄 19. BiFunction

`BiFunction<T, U, R>` accepts two values and produces a result.

Its abstract method is:

```java
R apply(T t, U u);
```

Example:

```java
BiFunction<Integer, Integer, Integer> addition =
        (a, b) -> a + b;
```

Usage:

```java
System.out.println(addition.apply(10, 20));
```

Output:

```text
30
```

### Memory Trick

```text
T + U → R
```

---

# 📤 20. BiConsumer

`BiConsumer<T, U>` accepts two values and returns nothing.

Its abstract method is:

```java
void accept(T t, U u);
```

Example:

```java
BiConsumer<String, Integer> printer =
        (name, age) ->
                System.out.println(name + " " + age);
```

Usage:

```java
printer.accept("Java", 30);
```

Output:

```text
Java 30
```

### Memory Trick

```text
T + U → void
```

---

# ⚡ 21. Primitive Functional Interfaces

Generic functional interfaces work with wrapper types.

Example:

```java
Function<Integer, Integer> square =
        number -> number * number;
```

Here `Integer` is used instead of primitive `int`.

This can involve **boxing and unboxing**.

Java provides primitive-specialized interfaces to reduce unnecessary boxing.

### Important Interfaces

| Primitive Type | Common Interfaces                                                                                    |
| -------------- | ---------------------------------------------------------------------------------------------------- |
| `int`          | `IntPredicate`, `IntFunction`, `IntConsumer`, `IntSupplier`, `IntUnaryOperator`, `IntBinaryOperator` |
| `long`         | `LongPredicate`, `LongFunction`, `LongConsumer`, `LongSupplier`, etc.                                |
| `double`       | `DoublePredicate`, `DoubleFunction`, `DoubleConsumer`, `DoubleSupplier`, etc.                        |

Example:

```java
IntPredicate isEven =
        number -> number % 2 == 0;
```

No `Integer` wrapper is required for the predicate's input.

---

# 🧬 22. Functional Interface Hierarchy

Important relationships:

```text
Function<T,R>
│
└── UnaryOperator<T>
        │
        └── T → T


BiFunction<T,U,R>
│
└── BinaryOperator<T>
        │
        └── T + T → T
```

Other important functional interfaces:

```text
Predicate<T>
Consumer<T>
Supplier<T>
BiPredicate<T,U>
BiConsumer<T,U>
```

---

# ⚙️ 23. Internal Working

Consider:

```java
@FunctionalInterface
interface Calculator {

    int calculate(int a, int b);
}
```

Lambda:

```java
Calculator calculator =
        (a, b) -> a + b;
```

Conceptually:

```text
Lambda Expression
       ↓
Target Type
       ↓
Calculator
       ↓
Single Abstract Method
       ↓
calculate(int, int)
       ↓
Lambda implementation
```

At the bytecode/runtime level, Java commonly uses:

```text
invokedynamic
```

and the JVM's lambda metafactory mechanisms to create the required implementation.

### Important Point

The compiler does not simply convert every lambda into an anonymous inner class.

This distinction is frequently asked in interviews.

---

# 🆚 24. Functional Interface vs Normal Interface

| Functional Interface                      | Normal Interface                   |
| ----------------------------------------- | ---------------------------------- |
| Exactly one abstract method               | Can have multiple abstract methods |
| Designed for lambda expressions           | Not necessarily lambda-compatible  |
| Can use `@FunctionalInterface`            | Annotation not applicable          |
| Represents one primary behavior           | Can represent multiple behaviors   |
| Commonly used with functional programming | General-purpose abstraction        |

Example:

```java
@FunctionalInterface
interface Calculator {

    int calculate(int a, int b);
}
```

Normal interface:

```java
interface Vehicle {

    void start();

    void stop();

    void accelerate();
}
```

`Vehicle` has multiple abstract methods, so it is not a functional interface.

---

# 🆚 25. Functional Interface vs Abstract Class

| Functional Interface                              | Abstract Class                     |
| ------------------------------------------------- | ---------------------------------- |
| Interface                                         | Class                              |
| One abstract method                               | Can have multiple abstract methods |
| Supports lambda target typing                     | Cannot directly be a lambda target |
| Multiple inheritance of interfaces possible       | Single class inheritance           |
| No instance fields in the traditional class sense | Can contain instance fields        |
| Can contain default/static methods                | Can contain concrete methods       |
| Used heavily for behavior                         | Used for shared state + behavior   |

Example functional interface:

```java
@FunctionalInterface
interface Operation {

    int execute(int a, int b);
}
```

Lambda:

```java
Operation addition =
        (a, b) -> a + b;
```

An abstract class cannot be used this way:

```java
abstract class Operation {

    abstract int execute(int a, int b);
}
```

This is invalid:

```java
Operation addition =
        (a, b) -> a + b;
```

---

# ⚡ 26. Advantages

### 1. Enables Lambda Expressions

Functional interfaces provide the target type for lambdas.

### 2. Reduces Boilerplate

Behavior can be represented concisely.

### 3. Improves API Flexibility

Methods can accept behavior as parameters.

### 4. Supports Composition

Functional interfaces can be combined and chained.

### 5. Enables Functional-Style Programming

They form the foundation for Java's functional programming features.

---

# ❌ 27. Common Mistakes

## Mistake 1: Thinking `@FunctionalInterface` Makes an Interface Functional

Wrong:

> "An interface becomes functional because we add the annotation."

Correct:

> The interface must satisfy the **single abstract method** rule. The annotation only tells the compiler to verify that rule.

---

## Mistake 2: Counting Default Methods

Default methods are not abstract.

Therefore:

```java
@FunctionalInterface
interface Example {

    void execute();

    default void display() {
        System.out.println("Display");
    }
}
```

is valid.

---

## Mistake 3: Counting Static Methods

Static methods are not abstract instance methods.

Therefore they do not break the functional-interface contract.

---

## Mistake 4: Thinking Every Interface Can Be a Lambda Target

No.

A lambda requires a compatible functional interface target type.

---

# 🧨 28. Interview Traps

### Trap 1

**Can a functional interface have more than one method?**

Yes, but it can have only **one abstract method**.

It may also contain:

* Default methods
* Static methods
* Object methods

---

### Trap 2

**Can a functional interface extend another interface?**

Yes.

Example:

```java
@FunctionalInterface
interface Parent {

    void execute();
}

@FunctionalInterface
interface Child extends Parent {

}
```

`Child` still has one abstract method.

---

### Trap 3

**Can a functional interface extend two interfaces?**

Potentially yes, provided the resulting interface still has exactly one abstract method.

Example:

```java
interface A {

    void execute();
}

interface B {

    void execute();
}

@FunctionalInterface
interface C extends A, B {

}
```

The inherited methods have the same signature and represent one abstract method contract.

---

### Trap 4

**Can an interface contain private methods and remain functional?**

Yes.

Private interface methods are not abstract methods and therefore do not violate the functional-interface rule.

---

### Trap 5

**Can an abstract class be a functional interface?**

No.

A functional interface must be an interface.

---

# 🧠 29. 30-Second Interview Answer

> **A functional interface is an interface that contains exactly one abstract method, also called a SAM interface. It provides the target type for lambda expressions and method references. Java provides the `@FunctionalInterface` annotation to allow the compiler to verify this rule. A functional interface can still contain default, static, private methods, and methods corresponding to public methods of `Object`, because these do not create additional abstract-method requirements.**

---

# 📌 30. Cheat Sheet

```text
Functional Interface
        │
        ├── Exactly ONE abstract method
        │
        ├── @FunctionalInterface
        │       └── Optional compiler check
        │
        ├── Default methods
        │       └── Allowed
        │
        ├── Static methods
        │       └── Allowed
        │
        ├── Private methods
        │       └── Allowed
        │
        └── Object methods
                └── Do not break SAM rule
```

### Built-in Interfaces

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
    T + T → T

BiPredicate<T,U>
    T + U → boolean

BiFunction<T,U,R>
    T + U → R

BiConsumer<T,U>
    T + U → void
```

---

# 🧠 31. Top Interview Questions

### Q1. What is a functional interface?

An interface containing exactly one abstract method.

### Q2. What is SAM?

**SAM = Single Abstract Method.**

### Q3. Is `@FunctionalInterface` mandatory?

No. It is optional but recommended because the compiler can verify the contract.

### Q4. Can a functional interface have default methods?

Yes.

### Q5. Can a functional interface have static methods?

Yes.

### Q6. Can a functional interface have private methods?

Yes.

### Q7. Can a functional interface have Object methods?

Yes. Methods corresponding to public methods of `Object` do not count toward the functional-interface abstract-method requirement.

### Q8. Can a functional interface extend another interface?

Yes, provided the resulting interface has exactly one abstract method.

### Q9. Can a functional interface have multiple abstract methods?

No.

### Q10. What is the relationship between lambda and functional interface?

A lambda expression provides an implementation for the single abstract method of a compatible functional interface.

### Q11. What is `Predicate<T>`?

A functional interface that takes a value and returns a boolean.

```text
T → boolean
```

### Q12. What is `Function<T,R>`?

A functional interface that transforms a value of type `T` into type `R`.

```text
T → R
```

### Q13. What is `Consumer<T>`?

A functional interface that consumes a value and returns nothing.

```text
T → void
```

### Q14. What is `Supplier<T>`?

A functional interface that takes no input and supplies a value.

```text
() → T
```

### Q15. What is the difference between `Function` and `UnaryOperator`?

`Function<T,R>` can have different input and output types:

```text
T → R
```

`UnaryOperator<T>` requires the same type:

```text
T → T
```

---

# 🧩 32. DSA Connection

Functional interfaces are useful in DSA because they allow **algorithmic behavior to be passed as an argument**.

## Example: Custom Filtering

```java
static List<Integer> filter(
        List<Integer> numbers,
        Predicate<Integer> condition) {

    List<Integer> result = new ArrayList<>();

    for (Integer number : numbers) {
        if (condition.test(number)) {
            result.add(number);
        }
    }

    return result;
}
```

Usage:

```java
List<Integer> numbers =
        List.of(1, 2, 3, 4, 5, 6);

List<Integer> evenNumbers =
        filter(numbers, number -> number % 2 == 0);
```

Here:

```text
Algorithm
   +
Predicate
   ↓
Custom behavior
```

The algorithm itself does not need to know the exact condition.

---

## Example: Custom Transformation

```java
static List<Integer> transform(
        List<Integer> numbers,
        Function<Integer, Integer> operation) {

    List<Integer> result = new ArrayList<>();

    for (Integer number : numbers) {
        result.add(operation.apply(number));
    }

    return result;
}
```

Usage:

```java
List<Integer> numbers =
        List.of(1, 2, 3, 4, 5);

List<Integer> squares =
        transform(numbers, number -> number * number);
```

Result:

```text
[1, 4, 9, 16, 25]
```

---

# 🎯 DSA Thinking Pattern

Whenever you see:

> "Apply some operation to every element."

Think:

```text
Function<T,R>
```

Whenever you see:

> "Check whether an element satisfies a condition."

Think:

```text
Predicate<T>
```

Whenever you see:

> "Perform an action for every element."

Think:

```text
Consumer<T>
```

Whenever you see:

> "Generate/provide a value."

Think:

```text
Supplier<T>
```

---

# 🏁 Final Takeaway

> **Functional interfaces are the bridge between Java's traditional interface-based design and functional programming.**

The core relationship is:

```text
Functional Interface
        ↓
Provides Target Type
        ↓
Lambda Expression
        ↓
Implementation of SAM
        ↓
Functional Behavior
```

Remember the four most important built-in interfaces:

```text
Predicate  → T → boolean
Function   → T → R
Consumer   → T → void
Supplier   → () → T
```

And the most important rule:

> **Functional Interface = Exactly ONE abstract method.**

```
```
