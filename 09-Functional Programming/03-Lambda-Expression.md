# ⚡ Lambda Expression in Java

> A **Lambda Expression** is a concise way to provide the implementation of the single abstract method of a **functional interface**.

---

## 📌 Table of Contents

1. [Introduction](#-introduction)
2. [Why Lambda Expressions?](#-why-lambda-expressions)
3. [What is a Lambda Expression?](#-what-is-a-lambda-expression)
4. [Basic Syntax](#-basic-syntax)
5. [Lambda Expression Components](#-lambda-expression-components)
6. [Simple Examples](#-simple-examples)
7. [Lambda with Functional Interface](#-lambda-with-functional-interface)
8. [Lambda with Parameters](#-lambda-with-parameters)
9. [Lambda with Multiple Parameters](#-lambda-with-multiple-parameters)
10. [Lambda with Return Value](#-lambda-with-return-value)
11. [Lambda with Multiple Statements](#-lambda-with-multiple-statements)
12. [Type Inference](#-type-inference)
13. [Parentheses Rules](#-parentheses-rules)
14. [Curly Braces Rules](#-curly-braces-rules)
15. [Lambda vs Anonymous Class](#-lambda-vs-anonymous-class)
16. [Lambda and Functional Interfaces](#-lambda-and-functional-interfaces)
17. [Built-in Functional Interfaces](#-built-in-functional-interfaces)
18. [Lambda with Collections](#-lambda-with-collections)
19. [Lambda with `forEach`](#-lambda-with-foreach)
20. [Lambda with Comparator](#-lambda-with-comparator)
21. [Lambda with Threads](#-lambda-with-threads)
22. [Variable Scope](#-variable-scope)
23. [Effectively Final Variables](#-effectively-final-variables)
24. [Lambda and `this`](#-lambda-and-this)
25. [Lambda vs Method Reference](#-lambda-vs-method-reference)
26. [Internal Working](#-internal-working)
27. [JVM Perspective](#-jvm-perspective)
28. [Advantages](#-advantages)
29. [Disadvantages](#-disadvantages)
30. [Common Mistakes](#-common-mistakes)
31. [Interview Traps](#-interview-traps)
32. [DSA Connection](#-dsa-connection)
33. [How to Think About Lambda](#-how-to-think-about-lambda)
34. [Quick Cheat Sheet](#-quick-cheat-sheet)
35. [30-Second Interview Answer](#-30-second-interview-answer)
36. [Top 10 Interview Questions](#-top-10-interview-questions)

---

# 🚀 Introduction

Lambda Expressions were introduced in **Java 8**.

They provide a concise way to represent the implementation of a functional interface.

Before Java 8, anonymous classes were commonly used to provide behavior.

### Before Java 8

```java
Runnable task = new Runnable() {
    @Override
    public void run() {
        System.out.println("Task running");
    }
};
```

### With Lambda

```java
Runnable task = () -> {
    System.out.println("Task running");
};
```

### Even shorter

```java
Runnable task = () -> System.out.println("Task running");
```

Lambda expressions are one of the major features that enabled Java to support a more functional programming style.

---

# 🤔 Why Lambda Expressions?

Without Lambda, passing behavior often requires an anonymous class.

### Without Lambda

```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

names.forEach(new Consumer<String>() {
    @Override
    public void accept(String name) {
        System.out.println(name);
    }
});
```

### With Lambda

```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

names.forEach(name -> System.out.println(name));
```

### Lambda provides

- Less boilerplate code
- Better readability
- Easy behavior passing
- Better support for functional programming
- Cleaner Collection operations
- Cleaner Stream operations
- Convenient thread creation

---

# 🧠 What is a Lambda Expression?

A Lambda Expression is an **anonymous implementation of the abstract method of a functional interface**.

Example:

```java
() -> System.out.println("Hello");
```

This Lambda:

- Takes no parameter
- Executes an operation
- Returns nothing

Another example:

```java
number -> number * number
```

This Lambda:

- Takes one parameter
- Performs a calculation
- Returns the result

### Important

A Lambda is **not a standalone method**.

It gets its meaning from a **target functional interface**.

For example:

```java
Calculator calculator = (a, b) -> a + b;
```

Here, the Lambda gets its target type from `Calculator`.

---

# 🧩 Basic Syntax

The general syntax is:

```java
(parameters) -> expression
```

Or:

```java
(parameters) -> {
    statements;
}
```

### Example

```java
(int a, int b) -> a + b
```

Here:

- `(int a, int b)` → parameters
- `->` → Lambda operator
- `a + b` → Lambda body
- Result of `a + b` → returned value

---

# 🧱 Lambda Expression Components

A Lambda expression can contain three major components.

```java
(parameters) -> expression
```

## 1. Parameters

```java
(a, b)
```

Parameters represent the input values.

## 2. Arrow Operator

```java
->
```

The arrow separates parameters from the body.

## 3. Body

```java
a + b
```

The body contains the operation performed by the Lambda.

---

# 🧪 Simple Examples

## 1. No Parameter

```java
() -> System.out.println("Hello");
```

The empty parentheses mean the Lambda accepts no parameters.

---

## 2. One Parameter

```java
name -> System.out.println(name);
```

When there is exactly one parameter, parentheses can be omitted.

The following is also valid:

```java
(name) -> System.out.println(name);
```

---

## 3. Multiple Parameters

```java
(a, b) -> System.out.println(a + b);
```

Multiple parameters require parentheses.

---

## 4. Expression Body

```java
(a, b) -> a + b
```

The value of the expression is automatically returned.

---

## 5. Block Body

```java
(a, b) -> {
    int sum = a + b;
    return sum;
}
```

When a block body is used, a `return` statement is required when the functional interface method has a return value.

---

# 🔗 Lambda with Functional Interface

A Lambda requires a **target type**.

A functional interface is commonly used as that target type.

### Example

```java
@FunctionalInterface
interface Calculator {
    int calculate(int a, int b);
}
```

Lambda implementation:

```java
Calculator calculator = (a, b) -> a + b;

System.out.println(calculator.calculate(10, 20));
```

Output:

```text
30
```

### How it works

```java
Calculator calculator = (a, b) -> a + b;
```

The compiler knows:

```java
int calculate(int a, int b);
```

Therefore, the Lambda:

```java
(a, b) -> a + b
```

must be compatible with that method.

---

# 🔢 Lambda with Parameters

A Lambda can accept parameters.

### One Parameter

```java
@FunctionalInterface
interface Printer {
    void print(String message);
}
```

Implementation:

```java
Printer printer = message -> System.out.println(message);

printer.print("Hello Java");
```

Output:

```text
Hello Java
```

### Multiple Parameters

```java
@FunctionalInterface
interface Calculator {
    int add(int a, int b);
}
```

Implementation:

```java
Calculator calculator = (a, b) -> a + b;

System.out.println(calculator.add(10, 20));
```

Output:

```text
30
```

---

# ➕ Lambda with Multiple Parameters

When there are multiple parameters, parentheses are mandatory.

Valid:

```java
(a, b) -> a + b
```

Invalid:

```java
a, b -> a + b
```

### Example

```java
@FunctionalInterface
interface MathOperation {
    int operate(int a, int b);
}

MathOperation multiply = (a, b) -> a * b;

System.out.println(multiply.operate(5, 4));
```

Output:

```text
20
```

---

# 🔄 Lambda with Return Value

A Lambda can return a value.

### Expression Body

```java
(a, b) -> a + b
```

The expression result is automatically returned.

Example:

```java
@FunctionalInterface
interface Calculator {
    int calculate(int a, int b);
}

Calculator calculator = (a, b) -> a + b;

int result = calculator.calculate(10, 20);

System.out.println(result);
```

Output:

```text
30
```

### Block Body

```java
Calculator calculator = (a, b) -> {
    int result = a + b;
    return result;
};
```

With a block body, `return` must be explicitly written.

---

# 🧱 Lambda with Multiple Statements

A Lambda can contain multiple statements by using `{}`.

```java
Calculator calculator = (a, b) -> {
    System.out.println("Calculating...");
    int result = a + b;
    return result;
};

System.out.println(calculator.calculate(10, 20));
```

Output:

```text
Calculating...
30
```

### Important Rule

If a Lambda has multiple statements, use braces.

```java
(a, b) -> {
    int sum = a + b;
    return sum;
}
```

---

# 🧠 Type Inference

Java can often infer the parameter types of a Lambda from the target functional interface.

Consider:

```java
@FunctionalInterface
interface Calculator {
    int calculate(int a, int b);
}
```

We can write:

```java
Calculator calculator = (a, b) -> a + b;
```

We don't need:

```java
Calculator calculator = (int a, int b) -> a + b;
```

The compiler knows that:

```java
a
```

and:

```java
b
```

are `int` because of the functional interface method.

### Explicit Types

This is also valid:

```java
Calculator calculator = (int a, int b) -> a + b;
```

### Important Rule

When explicitly specifying parameter types, all parameter types must be specified.

Valid:

```java
(int a, int b) -> a + b
```

Invalid:

```java
(int a, b) -> a + b
```

---

# 📝 Parentheses Rules

## Zero Parameters

Parentheses are mandatory.

```java
() -> System.out.println("Hello");
```

Invalid:

```java
-> System.out.println("Hello");
```

---

## One Parameter

Parentheses can be omitted.

```java
name -> System.out.println(name);
```

Or:

```java
(name) -> System.out.println(name);
```

Both are valid.

---

## Multiple Parameters

Parentheses are mandatory.

```java
(a, b) -> a + b
```

---

# 🔲 Curly Braces Rules

Curly braces are optional when the Lambda body contains a single expression.

```java
(a, b) -> a + b
```

For multiple statements, braces are required.

```java
(a, b) -> {
    int result = a + b;
    return result;
}
```

### Important Difference

Expression body:

```java
(a, b) -> a + b
```

Block body:

```java
(a, b) -> {
    return a + b;
}
```

---

# ⚖️ Lambda vs Anonymous Class

## Anonymous Class

```java
Runnable task = new Runnable() {
    @Override
    public void run() {
        System.out.println("Running");
    }
};
```

## Lambda

```java
Runnable task = () -> System.out.println("Running");
```

### Comparison

| Feature | Anonymous Class | Lambda |
|---|---|---|
| Syntax | Verbose | Concise |
| Introduced | Older Java feature | Java 8 |
| Main use | Implement/extend types anonymously | Implement functional interfaces |
| Boilerplate | More | Less |
| `this` | Refers to anonymous class instance | Refers to enclosing instance |
| Multiple abstract methods | Possible depending on interface | No |
| Functional interface required | No | Yes |

### Important

A Lambda can target a **functional interface**, whereas an anonymous class can implement interfaces that have multiple abstract methods.

---

# 🔗 Lambda and Functional Interfaces

A Lambda can implement a functional interface.

Example:

```java
@FunctionalInterface
interface Greeting {
    void greet(String name);
}
```

Lambda:

```java
Greeting greeting = name -> System.out.println("Hello " + name);

greeting.greet("Divyansh");
```

Output:

```text
Hello Divyansh
```

### Functional Interface Rule

A functional interface must have exactly **one abstract method**.

It can still contain:

- Default methods
- Static methods
- Methods inherited from `Object`

Example:

```java
@FunctionalInterface
interface Greeting {

    void greet(String name);

    default void message() {
        System.out.println("Welcome");
    }

    static void info() {
        System.out.println("Greeting interface");
    }
}
```

---

# 🏗️ Built-in Functional Interfaces

Java provides many functional interfaces in `java.util.function`.

The most important ones are:

| Interface | Input | Output | Main Method |
|---|---|---|---|
| `Predicate<T>` | T | boolean | `test()` |
| `Function<T, R>` | T | R | `apply()` |
| `Consumer<T>` | T | void | `accept()` |
| `Supplier<T>` | None | T | `get()` |
| `UnaryOperator<T>` | T | T | `apply()` |
| `BinaryOperator<T>` | T, T | T | `apply()` |

---

# 🔍 Predicate

`Predicate<T>` represents a condition that returns `boolean`.

```java
Predicate<Integer> isEven = number -> number % 2 == 0;

System.out.println(isEven.test(10));
System.out.println(isEven.test(7));
```

Output:

```text
true
false
```

---

# 🔧 Function

`Function<T, R>` accepts one input and returns one output.

```java
Function<Integer, Integer> square = number -> number * number;

System.out.println(square.apply(5));
```

Output:

```text
25
```

---

# 📢 Consumer

`Consumer<T>` accepts an input and returns nothing.

```java
Consumer<String> printer = message -> System.out.println(message);

printer.accept("Hello Java");
```

Output:

```text
Hello Java
```

---

# 🎁 Supplier

`Supplier<T>` takes no input and returns a value.

```java
Supplier<String> supplier = () -> "Java";

System.out.println(supplier.get());
```

Output:

```text
Java
```

---

# ➗ UnaryOperator

`UnaryOperator<T>` accepts and returns the same type.

```java
UnaryOperator<Integer> square = number -> number * number;

System.out.println(square.apply(6));
```

Output:

```text
36
```

---

# ✖️ BinaryOperator

`BinaryOperator<T>` accepts two values of the same type and returns the same type.

```java
BinaryOperator<Integer> addition = (a, b) -> a + b;

System.out.println(addition.apply(10, 20));
```

Output:

```text
30
```

---

# 📦 Lambda with Collections

Lambda expressions are heavily used with Collections.

Example:

```java
List<String> names = new ArrayList<>();

names.add("Alice");
names.add("Bob");
names.add("Charlie");

names.forEach(name -> System.out.println(name));
```

Output:

```text
Alice
Bob
Charlie
```

---

# 🔁 Lambda with `forEach`

The `forEach()` method accepts a `Consumer`.

Example:

```java
List<Integer> numbers = Arrays.asList(10, 20, 30, 40);

numbers.forEach(number -> System.out.println(number));
```

Output:

```text
10
20
30
40
```

### Multiple Statements

```java
numbers.forEach(number -> {
    int square = number * number;
    System.out.println(square);
});
```

---

# 📊 Lambda with Comparator

Lambda expressions make sorting concise.

### Traditional Comparator

```java
List<Integer> numbers = Arrays.asList(5, 2, 8, 1);

numbers.sort(new Comparator<Integer>() {
    @Override
    public int compare(Integer a, Integer b) {
        return a - b;
    }
});
```

### Lambda Comparator

```java
List<Integer> numbers = Arrays.asList(5, 2, 8, 1);

numbers.sort((a, b) -> a - b);
```

Result:

```text
[1, 2, 5, 8]
```

### Descending Order

```java
numbers.sort((a, b) -> b - a);
```

For production code, `Integer.compare()` is generally safer than subtraction-based comparison because subtraction can overflow.

```java
numbers.sort((a, b) -> Integer.compare(b, a));
```

---

# 🧵 Lambda with Threads

Lambda expressions can simplify thread creation because `Runnable` is a functional interface.

### Traditional Approach

```java
Thread thread = new Thread(new Runnable() {
    @Override
    public void run() {
        System.out.println("Task running");
    }
});

thread.start();
```

### Lambda Approach

```java
Thread thread = new Thread(() -> {
    System.out.println("Task running");
});

thread.start();
```

### Short Form

```java
new Thread(() -> System.out.println("Task running")).start();
```

---

# 🔐 Variable Scope

A Lambda can access variables from its enclosing scope.

```java
public class Main {

    public static void main(String[] args) {

        String message = "Hello";

        Runnable task = () -> {
            System.out.println(message);
        };

        task.run();
    }
}
```

Output:

```text
Hello
```

The Lambda can read the local variable because it belongs to the enclosing scope.

---

# 🔒 Effectively Final Variables

A local variable used inside a Lambda must be **final or effectively final**.

### Valid

```java
public class Main {

    public static void main(String[] args) {

        int number = 10;

        Runnable task = () -> {
            System.out.println(number);
        };

        task.run();
    }
}
```

`number` is effectively final because its value is never changed.

### Invalid

```java
public class Main {

    public static void main(String[] args) {

        int number = 10;

        Runnable task = () -> {
            System.out.println(number);
        };

        number = 20;

        task.run();
    }
}
```

The compiler rejects this because `number` is modified after being captured.

### Explicit `final`

```java
final int number = 10;

Runnable task = () -> System.out.println(number);
```

---

# 🎯 Lambda and `this`

One important difference between Lambda and anonymous classes is the meaning of `this`.

Inside a Lambda, `this` refers to the **enclosing object**.

Example:

```java
public class Demo {

    private String name = "Outer";

    void show() {

        Runnable task = () -> {
            System.out.println(this.name);
        };

        task.run();
    }

    public static void main(String[] args) {
        new Demo().show();
    }
}
```

Output:

```text
Outer
```

With an anonymous class, `this` refers to the anonymous class instance.

### Interview Point

```text
Lambda `this`
        ↓
Enclosing instance
```

```text
Anonymous class `this`
        ↓
Anonymous class instance
```

---

# 🔄 Lambda vs Method Reference

A Lambda can sometimes be replaced by a method reference.

### Lambda

```java
Consumer<String> printer = message -> System.out.println(message);
```

### Method Reference

```java
Consumer<String> printer = System.out::println;
```

Both represent the same behavior.

Method references are covered in detail in:

```text
04-Method-Reference.md
```

---

# ⚙️ Internal Working

A Lambda is not simply converted into an ordinary anonymous inner class.

Modern Java uses the `invokedynamic` mechanism to support Lambda expressions.

At a high level:

```text
Lambda Source Code
       ↓
Java Compiler
       ↓
Bytecode containing invokedynamic
       ↓
LambdaMetafactory
       ↓
Functional Interface implementation
       ↓
Lambda execution
```

The exact runtime implementation is JVM-dependent and should not be treated as a guaranteed generated class structure.

---

# 🧠 JVM Perspective

Consider:

```java
Runnable task = () -> System.out.println("Hello");
```

Conceptually:

```text
Source Code
     ↓
javac
     ↓
Bytecode
     ↓
invokedynamic
     ↓
LambdaMetafactory
     ↓
Runnable-compatible implementation
     ↓
run()
```

### Important

The JVM does not require Lambda expressions to be implemented as anonymous inner classes.

This is one reason Lambda implementation details should not be explained simply as:

> "Compiler creates an anonymous class."

That is an oversimplification.

---

# 🧮 Lambda and Functional Programming

Lambda expressions allow behavior to be treated more conveniently.

For example:

```java
Function<Integer, Integer> square = number -> number * number;
```

The function can be:

- Stored in a variable
- Passed as an argument
- Returned from another method
- Used by Collection APIs
- Used by Stream operations

Example:

```java
static int calculate(int number, Function<Integer, Integer> operation) {
    return operation.apply(number);
}
```

Usage:

```java
int result = calculate(5, number -> number * number);

System.out.println(result);
```

Output:

```text
25
```

This is an important concept behind Java's functional programming features.

---

# 🛠️ Passing Behavior as an Argument

Traditional programming often passes data.

Lambda allows behavior to be passed as an argument.

```java
static void operate(int a, int b, BinaryOperator<Integer> operation) {
    System.out.println(operation.apply(a, b));
}
```

Different behavior can be passed:

```java
operate(10, 5, (a, b) -> a + b);

operate(10, 5, (a, b) -> a - b);

operate(10, 5, (a, b) -> a * b);
```

The method stays the same while the behavior changes.

---

# 🧠 How to Think About Lambda

When you see:

```java
(a, b) -> a + b
```

Think:

```text
Input
  ↓
(a, b)
  ↓
Operation
  ↓
a + b
  ↓
Output
```

When you see:

```java
name -> System.out.println(name)
```

Think:

```text
Input
  ↓
name
  ↓
print it
  ↓
no return value
```

When you see:

```java
() -> System.out.println("Hello")
```

Think:

```text
No input
   ↓
Perform operation
   ↓
No return value
```

---

# 🚨 Common Mistakes

## 1. Forgetting the Target Type

This is not valid as a standalone statement:

```java
(a, b) -> a + b;
```

The Lambda needs a target type.

Valid:

```java
BinaryOperator<Integer> addition = (a, b) -> a + b;
```

---

## 2. Forgetting Parentheses for Multiple Parameters

Invalid:

```java
a, b -> a + b
```

Valid:

```java
(a, b) -> a + b
```

---

## 3. Mixing Explicit and Implicit Parameter Types

Invalid:

```java
(int a, b) -> a + b
```

Valid:

```java
(int a, int b) -> a + b
```

Or:

```java
(a, b) -> a + b
```

---

## 4. Forgetting `return` in a Block Body

Invalid:

```java
(a, b) -> {
    a + b;
}
```

Valid:

```java
(a, b) -> {
    return a + b;
}
```

---

## 5. Trying to Modify a Captured Local Variable

Invalid:

```java
int count = 10;

Runnable task = () -> {
    System.out.println(count);
};

count++;
```

A captured local variable must be final or effectively final.

---

# 🪤 Interview Traps

## Trap 1: Is Lambda a Functional Interface?

No.

A Lambda is an expression.

A functional interface is an interface with exactly one abstract method.

---

## Trap 2: Can Lambda Work Without Functional Interface?

A Lambda requires a **target functional interface type** or another compatible target type defined by Java's Lambda typing rules.

For normal Java usage, think:

```text
Lambda
   ↓
Functional Interface
```

---

## Trap 3: Does Lambda Create an Anonymous Class?

Not necessarily.

Modern Java uses mechanisms such as `invokedynamic` and `LambdaMetafactory`.

---

## Trap 4: Can a Functional Interface Have Default Methods?

Yes.

It can have any number of default and static methods while still having exactly one abstract method.

---

## Trap 5: Can Lambda Have Multiple Statements?

Yes.

Use braces:

```java
() -> {
    statement1();
    statement2();
}
```

---

## Trap 6: Can Lambda Access Local Variables?

Yes, if the local variables are **final or effectively final**.

---

## Trap 7: What does `this` mean inside Lambda?

`this` refers to the enclosing instance.

---

# 🧩 Lambda and `var`

Since Java 11, `var` can be used for Lambda parameters, but only when `var` is used consistently for all parameters.

Valid:

```java
(var a, var b) -> a + b
```

Invalid:

```java
(var a, b) -> a + b
```

This feature is useful when annotations need to be applied to Lambda parameters.

Example:

```java
(var a, var b) -> a + b
```

For ordinary code, type inference is usually simpler:

```java
(a, b) -> a + b
```

---

# 📊 Lambda vs Method vs Anonymous Class

| Feature | Normal Method | Anonymous Class | Lambda |
|---|---|---|---|
| Named | Yes | Usually no | No |
| Standalone | Yes | Yes, as anonymous type instance | No |
| Functional Interface required | No | No | Yes |
| Concise | Medium | Low | High |
| Multiple methods | Yes | Yes | No |
| Introduced in Java 8 | No | No | Yes |
| Common functional-style usage | Low | Medium | High |

---

# ⚡ Advantages

### 1. Concise Syntax

```java
x -> x * x
```

instead of verbose anonymous-class syntax.

### 2. Better Readability

The behavior is often visible directly where it is used.

### 3. Behavior as Argument

```java
calculate(10, number -> number * 2);
```

### 4. Works Well with Collections

```java
numbers.forEach(number -> System.out.println(number));
```

### 5. Works Well with Streams

```java
numbers.stream()
       .filter(number -> number % 2 == 0)
       .forEach(number -> System.out.println(number));
```

### 6. Useful for Multithreading

```java
new Thread(() -> System.out.println("Running")).start();
```

---

# ⚠️ Disadvantages

### 1. Can Become Difficult to Read

Very complex Lambdas can reduce readability.

```java
data.stream()
    .filter(x -> x != null && x.getValue() > 10 && x.getName().startsWith("A"))
    .map(x -> transform(calculate(x)))
    .forEach(x -> process(x));
```

Breaking complex behavior into named methods can sometimes be clearer.

### 2. Debugging Can Be Less Familiar

Lambda-heavy code may be harder for beginners to debug.

### 3. Requires Understanding of Functional Interfaces

Lambdas become much easier after understanding functional interfaces.

### 4. Captured Variables Have Restrictions

Local variables captured by Lambda must be final or effectively final.

---

# 🧠 DSA Connection

Lambda expressions are very useful in DSA implementations because they make custom operations concise.

## Sorting

```java
List<Integer> numbers = Arrays.asList(5, 1, 8, 2, 3);

numbers.sort((a, b) -> Integer.compare(a, b));
```

## Custom Object Sorting

```java
students.sort((s1, s2) -> Integer.compare(s1.getAge(), s2.getAge()));
```

## Filtering

```java
numbers.removeIf(number -> number % 2 == 0);
```

## Traversal

```java
numbers.forEach(number -> System.out.println(number));
```

### Important DSA Patterns

Lambda frequently appears with:

- Sorting
- Filtering
- Searching
- Custom Comparators
- Collection traversal
- Stream processing
- Priority Queue ordering
- Map operations

---

# 🧠 How to Think About Lambda in DSA

When a problem says:

> "Sort according to a custom rule."

Think:

```java
(a, b) -> customComparison
```

Example:

```java
Arrays.sort(numbers, (a, b) -> Integer.compare(a, b));
```

For objects:

```java
students.sort((a, b) -> Integer.compare(a.getMarks(), b.getMarks()));
```

For descending order:

```java
students.sort((a, b) -> Integer.compare(b.getMarks(), a.getMarks()));
```

The Lambda expresses the **comparison rule**.

---

# 📝 Quick Cheat Sheet

| Situation | Lambda |
|---|---|
| No parameter | `() -> statement` |
| One parameter | `x -> statement` |
| Multiple parameters | `(a, b) -> statement` |
| Return expression | `(a, b) -> a + b` |
| Multiple statements | `(a, b) -> { statements; }` |
| Explicit types | `(int a, int b) -> a + b` |
| Predicate | `x -> x > 10` |
| Consumer | `x -> System.out.println(x)` |
| Function | `x -> x * x` |
| Supplier | `() -> "Java"` |
| Comparator | `(a, b) -> Integer.compare(a, b)` |
| Runnable | `() -> task()` |

---

# 🎯 Lambda Pattern Recognition

### Pattern 1

```java
() -> action()
```

Means:

```text
No input → perform action
```

### Pattern 2

```java
x -> operation(x)
```

Means:

```text
One input → process it
```

### Pattern 3

```java
(a, b) -> operation(a, b)
```

Means:

```text
Two inputs → process them
```

### Pattern 4

```java
x -> booleanCondition(x)
```

Usually represents a `Predicate`.

### Pattern 5

```java
x -> transformedValue(x)
```

Usually represents a `Function`.

### Pattern 6

```java
x -> System.out.println(x)
```

Usually represents a `Consumer`.

---

# 🎤 30-Second Interview Answer

> "A Lambda Expression is a concise way to implement the abstract method of a functional interface. It was introduced in Java 8 to reduce boilerplate code and support functional programming. A Lambda has parameters, an arrow operator, and a body. For example, `(a, b) -> a + b` can implement a functional interface method that accepts two integers and returns their sum. Lambdas are heavily used with Collections, Streams, Comparators, and functional interfaces such as Predicate, Function, Consumer, and Supplier."

---

# 🎯 Top 10 Interview Questions

## 1. What is a Lambda Expression?

**Answer:**

A Lambda Expression is a concise expression that provides an implementation for the abstract method of a functional interface.

Example:

```java
Runnable task = () -> System.out.println("Running");
```

---

## 2. When were Lambda Expressions introduced?

**Answer:**

Lambda Expressions were introduced in **Java 8**.

---

## 3. What is the basic syntax of a Lambda?

**Answer:**

```java
(parameters) -> expression
```

Or:

```java
(parameters) -> {
    statements;
}
```

---

## 4. Can Lambda exist without a functional interface?

**Answer:**

A Lambda needs a compatible **target type**. In normal Java Lambda usage, this is a functional interface.

Example:

```java
Runnable task = () -> System.out.println("Hello");
```

---

## 5. Can a Lambda have multiple statements?

**Answer:**

Yes.

Use a block body:

```java
(a, b) -> {
    int result = a + b;
    return result;
}
```

---

## 6. What is the difference between Lambda and anonymous class?

**Answer:**

A Lambda provides a concise implementation of a functional interface, while an anonymous class can create an anonymous implementation of a class or interface and can contain multiple methods.

Also, `this` behaves differently.

In a Lambda, `this` refers to the enclosing instance.

---

## 7. What is effectively final?

**Answer:**

A local variable is effectively final when it is assigned once and never modified afterward.

Such a variable can be captured by a Lambda.

Example:

```java
int number = 10;

Runnable task = () -> System.out.println(number);
```

---

## 8. Does Lambda create an anonymous class?

**Answer:**

Not necessarily.

Java uses mechanisms including `invokedynamic` and `LambdaMetafactory` to implement Lambda expressions.

---

## 9. What does `this` refer to inside a Lambda?

**Answer:**

`this` refers to the enclosing instance.

---

## 10. Why are Lambda Expressions important in Java?

**Answer:**

They reduce boilerplate and make it easier to pass behavior as an argument. They are heavily used with functional interfaces, Collections, Streams, Comparators, and multithreading.

---

# 🔥 Interview Memory Trick

Remember:

```text
Lambda
   ↓
Java 8
   ↓
Functional Interface
   ↓
One Abstract Method
   ↓
Concise Implementation
   ↓
Functional Programming
```

And remember the basic pattern:

```java
(parameters) -> body
```

---

# 📌 Key Takeaways

- Lambda Expressions were introduced in **Java 8**.
- A Lambda provides an implementation for a compatible functional interface.
- Lambda syntax uses the `->` operator.
- Zero parameters require `()`.
- One parameter may omit parentheses.
- Multiple parameters require parentheses.
- Expression bodies can return implicitly.
- Block bodies require explicit `return` when returning a value.
- Lambda-captured local variables must be final or effectively final.
- `this` inside a Lambda refers to the enclosing instance.
- Lambdas are heavily used with Collections and Streams.
- Lambdas work with functional interfaces such as `Predicate`, `Function`, `Consumer`, and `Supplier`.
- Lambda implementation uses JVM mechanisms such as `invokedynamic`.
- Lambda is not simply an anonymous inner class.
- Lambda expressions are an important foundation for Java functional programming.

---

# 🧠 Final Mental Model

```text
Functional Interface
        ↓
One Abstract Method
        ↓
        Lambda
        ↓
Concise Implementation
        ↓
Behavior can be passed around
        ↓
Collections / Streams / Threads / APIs
```

> **Remember:** Lambda is not the functional interface itself.  
> The **functional interface provides the target type**, while the **Lambda provides the implementation of its abstract method**.