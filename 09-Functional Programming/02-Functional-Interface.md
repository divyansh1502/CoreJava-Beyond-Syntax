````md
# 🔗 Functional Interface in Java

> A **Functional Interface** is an interface that contains exactly one abstract method, making it the target type for lambda expressions and method references.

---

# 📚 Table of Contents

- [1. What is a Functional Interface?](#-1-what-is-a-functional-interface)
- [2. Why Functional Interfaces?](#-2-why-functional-interfaces)
- [3. Single Abstract Method (SAM)](#-3-single-abstract-method-sam)
- [4. Basic Syntax](#-4-basic-syntax)
- [5. `@FunctionalInterface` Annotation](#-5-functionalinterface-annotation)
- [6. Functional Interface with Lambda](#-6-functional-interface-with-lambda)
- [7. Functional Interface with Method Reference](#-7-functional-interface-with-method-reference)
- [8. Default Methods](#-8-default-methods)
- [9. Static Methods](#-9-static-methods)
- [10. Private Methods](#-10-private-methods)
- [11. Object Class Methods](#-11-object-class-methods)
- [12. Built-in Functional Interfaces](#-12-built-in-functional-interfaces)
- [13. Predicate](#-13-predicate)
- [14. Function](#-14-function)
- [15. Consumer](#-15-consumer)
- [16. Supplier](#-16-supplier)
- [17. UnaryOperator](#-17-unaryoperator)
- [18. BinaryOperator](#-18-binaryoperator)
- [19. BiPredicate](#-19-bipredicate)
- [20. BiFunction](#-20-bifunction)
- [21. BiConsumer](#-21-biconsumer)
- [22. Primitive Functional Interfaces](#-22-primitive-functional-interfaces)
- [23. Functional Interface Hierarchy](#-23-functional-interface-hierarchy)
- [24. Internal Working](#-24-internal-working)
- [25. Functional Interface vs Normal Interface](#-25-functional-interface-vs-normal-interface)
- [26. Functional Interface vs Abstract Class](#-26-functional-interface-vs-abstract-class)
- [27. Advantages](#-27-advantages)
- [28. Common Mistakes](#-28-common-mistakes)
- [29. Interview Traps](#-29-interview-traps)
- [30. 30-Second Interview Answer](#-30-30-second-interview-answer)
- [31. Cheat Sheet](#-31-cheat-sheet)
- [32. Top Interview Questions](#-32-top-interview-questions)
- [33. DSA Connection](#-33-dsa-connection)

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

The interface contains only one abstract method:

```text
calculate()
```

Therefore, it is a functional interface.

---

# 🤔 2. Why Functional Interfaces?

Lambda expressions need a **target type**.

Java uses functional interfaces as the target type of lambda expressions.

Example:

```java
@FunctionalInterface
interface Calculator {

    int calculate(int a, int b);
}
```

Now we can provide its implementation using a lambda:

```java
Calculator addition =
        (a, b) -> a + b;
```

Relationship:

```text
Lambda Expression
        ↓
Functional Interface
        ↓
Single Abstract Method
```

The functional interface tells Java what method the lambda is implementing.

---

# 3. Single Abstract Method (SAM)

The most important rule is:

> A functional interface must have exactly **one abstract method**.

Valid:

```java
@FunctionalInterface
interface Printer {

    void print();
}
```

Another valid example:

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

# 📝 4. Basic Syntax

A functional interface can be declared like this:

```java
@FunctionalInterface
interface MyInterface {

    void execute();
}
```

A lambda can implement it:

```java
MyInterface obj =
        () -> System.out.println("Executing");
```

Calling the method:

```java
obj.execute();
```

Output:

```text
Executing
```

---

# 🏷️ 5. `@FunctionalInterface` Annotation

Java provides the:

```java
@FunctionalInterface
```

annotation to explicitly indicate that an interface is intended to be a functional interface.

Example:

```java
@FunctionalInterface
interface Greeting {

    void greet();
}
```

### Important

`@FunctionalInterface` is **not mandatory**.

This is also a valid functional interface:

```java
interface Greeting {

    void greet();
}
```

The annotation simply tells the compiler to verify the functional-interface contract.

If another abstract method is added:

```java
@FunctionalInterface
interface Greeting {

    void greet();

    void welcome();
}
```

The compiler reports an error.

---

# 🔥 6. Functional Interface with Lambda

Example:

```java
@FunctionalInterface
interface Calculator {

    int calculate(int a, int b);
}
```

Lambda implementation:

```java
Calculator addition =
        (a, b) -> a + b;
```

Calling the method:

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

Class:

```java
class MessagePrinter {

    static void printMessage(String message) {
        System.out.println(message);
    }
}
```

Method reference:

```java
Printer printer =
        MessagePrinter::printMessage;
```

Calling:

```java
printer.print("Hello Java");
```

Output:

```text
Hello Java
```

---

# ⚙️ 8. Default Methods

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

```text
calculate()    → abstract
printMessage() → default
```

Only one method is abstract.

Therefore:

```text
Abstract methods = 1
```

---

# ⚡ 9. Static Methods

A functional interface can contain static methods.

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

This is valid because static methods are not abstract instance methods.

Calling:

```java
Calculator.info();
```

---

# 🔐 10. Private Methods

A functional interface can also contain private methods.

Example:

```java
@FunctionalInterface
interface Calculator {

    int calculate(int a, int b);

    private void helper() {
        System.out.println("Helper");
    }
}
```

Private interface methods do not count as abstract methods.

Therefore, the interface remains functional.

---

# 🧬 11. Object Class Methods

Important interview point:

> Methods that correspond to public methods of `java.lang.Object` do not count toward the single abstract method requirement.

Example:

```java
@FunctionalInterface
interface Example {

    void execute();

    boolean equals(Object obj);
}
```

This can still be a functional interface.

Why?

```text
execute() → abstract method
equals()  → corresponds to Object.equals()
```

The `equals()` declaration does not create an additional independent abstract-method requirement.

---

# ☕ 12. Built-in Functional Interfaces

Java provides many ready-made functional interfaces in:

```text
java.util.function
```

Important interfaces:

| Interface           | Input | Output  |
| ------------------- | ----- | ------- |
| `Predicate<T>`      | T     | boolean |
| `Function<T,R>`     | T     | R       |
| `Consumer<T>`       | T     | void    |
| `Supplier<T>`       | None  | T       |
| `UnaryOperator<T>`  | T     | T       |
| `BinaryOperator<T>` | T, T  | T       |
| `BiPredicate<T,U>`  | T, U  | boolean |
| `BiFunction<T,U,R>` | T, U  | R       |
| `BiConsumer<T,U>`   | T, U  | void    |

---

# 🔍 13. Predicate

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

```text
Predicate

T → boolean
```

---

# 🔄 14. Function

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

```text
Function

T → R
```

---

# 📤 15. Consumer

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

```text
Consumer

T → void
```

---

# 📥 16. Supplier

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

```text
Supplier

() → T
```

---

# 🔁 17. UnaryOperator

`UnaryOperator<T>` represents an operation where the input and output have the same type.

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
UnaryOperator

T → T
```

---

# ➕ 18. BinaryOperator

`BinaryOperator<T>` accepts two values of the same type and returns the same type.

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
BinaryOperator

T + T → T
```

---

# 🔍 19. BiPredicate

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
BiPredicate

T + U → boolean
```

---

# 🔄 20. BiFunction

`BiFunction<T, U, R>` accepts two values and returns a result.

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
BiFunction

T + U → R
```

---

# 📤 21. BiConsumer

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
BiConsumer

T + U → void
```

---

# ⚡ 22. Primitive Functional Interfaces

Generic functional interfaces normally work with wrapper types.

Example:

```java
Function<Integer, Integer> square =
        number -> number * number;
```

Here `Integer` is used instead of primitive `int`.

This can involve boxing and unboxing.

Java provides primitive-specialized interfaces to avoid unnecessary boxing in many cases.

### Important Interfaces

| Primitive | Functional Interfaces                                                                                                  |
| --------- | ---------------------------------------------------------------------------------------------------------------------- |
| `int`     | `IntPredicate`, `IntFunction`, `IntConsumer`, `IntSupplier`, `IntUnaryOperator`, `IntBinaryOperator`                   |
| `long`    | `LongPredicate`, `LongFunction`, `LongConsumer`, `LongSupplier`, `LongUnaryOperator`, `LongBinaryOperator`             |
| `double`  | `DoublePredicate`, `DoubleFunction`, `DoubleConsumer`, `DoubleSupplier`, `DoubleUnaryOperator`, `DoubleBinaryOperator` |

Example:

```java
IntPredicate isEven =
        number -> number % 2 == 0;
```

Here the predicate works directly with primitive `int`.

---

# 🧬 23. Functional Interface Hierarchy

Important relationships:

```text
Function<T, R>
        │
        └── UnaryOperator<T>
                │
                └── T → T
```

```text
BiFunction<T, U, R>
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

# ⚙️ 24. Internal Working

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
Lambda Implementation
```

At the bytecode/runtime level, Java commonly uses:

```text
invokedynamic
```

along with the JVM's lambda metafactory mechanism.

Important point:

> A lambda is not simply compiled into an anonymous inner class.

The runtime implementation mechanism is different.

---

# 🆚 25. Functional Interface vs Normal Interface

| Functional Interface                    | Normal Interface                                 |
| --------------------------------------- | ------------------------------------------------ |
| Exactly one abstract method             | Can have multiple abstract methods               |
| Designed for lambda expressions         | Not necessarily lambda-compatible                |
| Can use `@FunctionalInterface`          | Cannot use it if multiple abstract methods exist |
| Represents one primary behavior         | Can represent multiple behaviors                 |
| Commonly used in functional programming | General-purpose abstraction                      |

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

# 🆚 26. Functional Interface vs Abstract Class

| Functional Interface                       | Abstract Class                       |
| ------------------------------------------ | ------------------------------------ |
| Interface                                  | Class                                |
| Exactly one abstract method                | Can have multiple abstract methods   |
| Can be a lambda target                     | Cannot directly be a lambda target   |
| Supports multiple interface inheritance    | A class can extend only one class    |
| No instance state like a class             | Can contain instance fields          |
| Can contain default/static/private methods | Can contain concrete methods         |
| Primarily useful for behavior              | Useful for shared state and behavior |

Example:

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

An abstract class cannot be used directly as a lambda target.

---

# ⚡ 27. Advantages

### 1. Enables Lambda Expressions

Functional interfaces provide the target type for lambdas.

### 2. Reduces Boilerplate

Behavior can be represented concisely.

### 3. Improves API Flexibility

Methods can accept behavior as parameters.

### 4. Supports Composition

Functional operations can be combined and chained.

### 5. Enables Functional Programming

They form the foundation of Java's functional programming features.

---

# ❌ 28. Common Mistakes

### Mistake 1: Thinking `@FunctionalInterface` Makes an Interface Functional

Incorrect:

> An interface becomes functional because we add the annotation.

Correct:

> The interface must satisfy the **single abstract method** rule.

The annotation only allows the compiler to verify the rule.

---

### Mistake 2: Counting Default Methods

Default methods are not abstract.

Therefore this is valid:

```java
@FunctionalInterface
interface Example {

    void execute();

    default void display() {
        System.out.println("Display");
    }
}
```

---

### Mistake 3: Counting Static Methods

Static methods are not abstract instance methods.

Therefore they do not break the functional-interface contract.

---

### Mistake 4: Thinking Every Interface Can Be a Lambda Target

No.

A lambda requires a compatible functional-interface target type.

---

### Mistake 5: Confusing `Function` and `Consumer`

```text
Function  → returns a value
Consumer  → returns nothing
```

---

# 🧨 29. Interview Traps

### Trap 1: Can a functional interface have more than one method?

Yes, but it can have only **one abstract method**.

It may also contain:

* Default methods
* Static methods
* Private methods
* Methods corresponding to public methods of `Object`

---

### Trap 2: Can a functional interface extend another interface?

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

### Trap 3: Can a functional interface extend two interfaces?

Yes, provided the resulting interface still has exactly one abstract method.

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

### Trap 4: Can an interface contain private methods and remain functional?

Yes.

Private interface methods are not abstract methods.

---

### Trap 5: Can an abstract class be a functional interface?

No.

A functional interface must be an interface.

---

# 🧠 30. 30-Second Interview Answer

> **A functional interface is an interface that contains exactly one abstract method, also called a SAM interface. It provides the target type for lambda expressions and method references. The `@FunctionalInterface` annotation is optional and allows the compiler to verify the contract. A functional interface can still contain default, static, and private methods, and methods corresponding to public methods of `Object`, because these do not create additional abstract-method requirements.**

---

# 📌 31. Cheat Sheet

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

### Built-in Functional Interfaces

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

# 🎯 32. Top Interview Questions

### Q1. What is a functional interface?

An interface containing exactly one abstract method.

### Q2. What is SAM?

**SAM = Single Abstract Method.**

### Q3. Is `@FunctionalInterface` mandatory?

No. It is optional but recommended.

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

A functional interface that accepts a value and returns a boolean.

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

`UnaryOperator<T>` requires the same input and output type:

```text
T → T
```

---

# 🧩 33. DSA Connection

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

The algorithm does not need to know the exact filtering condition.

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

> "Generate or provide a value."

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
