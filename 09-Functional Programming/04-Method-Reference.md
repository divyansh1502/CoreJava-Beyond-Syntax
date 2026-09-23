# 🔗 Method Reference in Java

> A **Method Reference** is a shorthand syntax for a Lambda Expression that simply calls an existing method or constructor.

---

## 📌 Table of Contents

1. [Introduction](#-introduction)
2. [Why Method References?](#-why-method-references)
3. [What is a Method Reference?](#-what-is-a-method-reference)
4. [Lambda vs Method Reference](#-lambda-vs-method-reference)
5. [Method Reference Operator](#-method-reference-operator)
6. [Types of Method References](#-types-of-method-references)
7. [Static Method Reference](#-static-method-reference)
8. [Instance Method of a Particular Object](#-instance-method-of-a-particular-object)
9. [Instance Method of an Arbitrary Object](#-instance-method-of-an-arbitrary-object)
10. [Constructor Reference](#-constructor-reference)
11. [Method Reference with Functional Interfaces](#-method-reference-with-functional-interfaces)
12. [Method Reference with `forEach`](#-method-reference-with-foreach)
13. [Method Reference with Comparator](#-method-reference-with-comparator)
14. [Method Reference with Streams](#-method-reference-with-streams)
15. [Method Reference with Custom Classes](#-method-reference-with-custom-classes)
16. [Method Reference and Overloaded Methods](#-method-reference-and-overloaded-methods)
17. [Method Reference vs Method Invocation](#-method-reference-vs-method-invocation)
18. [Constructor Reference in Detail](#-constructor-reference-in-detail)
19. [Internal Working](#-internal-working)
20. [Advantages](#-advantages)
21. [Disadvantages](#-disadvantages)
22. [Common Mistakes](#-common-mistakes)
23. [Interview Traps](#-interview-traps)
24. [DSA Connection](#-dsa-connection)
25. [How to Think About Method References](#-how-to-think-about-method-references)
26. [Quick Cheat Sheet](#-quick-cheat-sheet)
27. [30-Second Interview Answer](#-30-second-interview-answer)
28. [Top 10 Interview Questions](#-top-10-interview-questions)

---

# 🚀 Introduction

Method References were introduced in **Java 8** along with Lambda Expressions.

They provide a shorter and often more readable way to represent a Lambda when the Lambda's only job is to call an existing method.

### Lambda

```java
Consumer<String> printer = message -> System.out.println(message);
```

### Method Reference

```java
Consumer<String> printer = System.out::println;
```

Both represent the same basic behavior.

---

# 🤔 Why Method References?

Suppose we already have a method that performs the required operation.

Instead of writing:

```java
name -> System.out.println(name)
```

we can directly reference the existing method:

```java
System.out::println
```

### Main Benefits

- Less code
- Better readability
- Reuses existing methods
- Avoids unnecessary Lambda syntax
- Works naturally with functional interfaces
- Commonly used with Collections and Streams

---

# 🧠 What is a Method Reference?

A Method Reference is a compact syntax for referring to an existing method without invoking it immediately.

The syntax uses:

```java
::
```

Example:

```java
System.out::println
```

This does **not** immediately execute `println()`.

Instead, it represents a method that can be invoked later through a compatible functional interface.

---

# 🔥 Lambda vs Method Reference

Consider:

```java
Consumer<String> printer = message -> System.out.println(message);
```

The Lambda receives `message` and passes it to `System.out.println()`.

We can replace it with:

```java
Consumer<String> printer = System.out::println;
```

### Conceptually

```text
Lambda
message -> System.out.println(message)

        ↓

Method Reference
System.out::println
```

### Important

A method reference is mainly useful when the Lambda **only delegates to an existing method**.

---

# 🔣 Method Reference Operator

The method reference operator is:

```java
::
```

General forms include:

```java
ClassName::staticMethod
```

```java
object::instanceMethod
```

```java
ClassName::instanceMethod
```

```java
ClassName::new
```

---

# 🧩 Types of Method References

Java has four major forms.

| Type | Syntax | Example |
|---|---|---|
| Static method | `ClassName::staticMethod` | `Math::abs` |
| Instance method of particular object | `object::method` | `System.out::println` |
| Instance method of arbitrary object | `ClassName::method` | `String::toUpperCase` |
| Constructor | `ClassName::new` | `ArrayList::new` |

---

# 1️⃣ Static Method Reference

A static method can be referenced using:

```java
ClassName::staticMethod
```

### Example

```java
Function<Integer, Integer> absolute = Math::abs;

System.out.println(absolute.apply(-10));
```

Output:

```text
10
```

Equivalent Lambda:

```java
Function<Integer, Integer> absolute = number -> Math.abs(number);
```

### Another Example

```java
Function<Integer, Integer> square = MathUtils::square;
```

Equivalent Lambda:

```java
Function<Integer, Integer> square = number -> MathUtils.square(number);
```

---

# 2️⃣ Instance Method of a Particular Object

Syntax:

```java
object::instanceMethod
```

The object is already known.

### Example

```java
String message = "hello";

Supplier<String> upper = message::toUpperCase;

System.out.println(upper.get());
```

Output:

```text
HELLO
```

Equivalent Lambda:

```java
Supplier<String> upper = () -> message.toUpperCase();
```

---

# 📢 `System.out::println`

This is one of the most common method references.

```java
Consumer<String> printer = System.out::println;

printer.accept("Hello Java");
```

Output:

```text
Hello Java
```

Equivalent Lambda:

```java
Consumer<String> printer = message -> System.out.println(message);
```

Here:

```java
System.out
```

is the object.

```java
println
```

is the instance method.

---

# 3️⃣ Instance Method of an Arbitrary Object

This form is slightly more advanced.

Syntax:

```java
ClassName::instanceMethod
```

Example:

```java
Function<String, String> upper = String::toUpperCase;

System.out.println(upper.apply("java"));
```

Output:

```text
JAVA
```

Equivalent Lambda:

```java
Function<String, String> upper = text -> text.toUpperCase();
```

### Important Difference

Here we do **not** specify a particular String object.

The object is supplied when the functional interface method is called.

Conceptually:

```text
String::toUpperCase

        ↓

text -> text.toUpperCase()
```

---

# 🔍 Another Arbitrary Object Example

```java
Function<String, Integer> length = String::length;

System.out.println(length.apply("Java"));
```

Output:

```text
4
```

Equivalent Lambda:

```java
Function<String, Integer> length = text -> text.length();
```

---

# 4️⃣ Constructor Reference

A constructor reference uses:

```java
ClassName::new
```

It represents a constructor.

### Example

```java
Supplier<ArrayList<String>> listCreator = ArrayList::new;

ArrayList<String> list = listCreator.get();

list.add("Java");

System.out.println(list);
```

Output:

```text
[Java]
```

Equivalent Lambda:

```java
Supplier<ArrayList<String>> listCreator = () -> new ArrayList<>();
```

---

# 🏗️ Constructor Reference with Parameters

Suppose we have:

```java
class Student {

    private String name;

    Student(String name) {
        this.name = name;
    }

    String getName() {
        return name;
    }
}
```

We can create objects using:

```java
Function<String, Student> creator = Student::new;

Student student = creator.apply("Divyansh");

System.out.println(student.getName());
```

Equivalent Lambda:

```java
Function<String, Student> creator = name -> new Student(name);
```

---

# 🔗 Method Reference with Functional Interfaces

Method references need a compatible target type.

### Consumer

```java
Consumer<String> printer = System.out::println;
```

### Function

```java
Function<String, Integer> length = String::length;
```

### Supplier

```java
Supplier<String> text = "Java"::toUpperCase;
```

### Predicate

```java
Predicate<String> empty = String::isEmpty;
```

### Constructor

```java
Supplier<ArrayList<String>> list = ArrayList::new;
```

The functional interface determines how the referenced method is interpreted.

---

# 🔁 Method Reference with `forEach`

Method references are frequently used with `forEach()`.

### Lambda

```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

names.forEach(name -> System.out.println(name));
```

### Method Reference

```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

names.forEach(System.out::println);
```

Output:

```text
Alice
Bob
Charlie
```

This is one of the most common real-world uses of method references.

---

# 📊 Method Reference with Comparator

Suppose we have:

```java
List<String> names = Arrays.asList("Java", "C", "Python", "Go");
```

We can sort according to string length.

### Lambda

```java
names.sort((a, b) -> Integer.compare(a.length(), b.length()));
```

Sometimes a method reference can make a simple comparator cleaner.

For example:

```java
names.sort(Comparator.comparingInt(String::length));
```

Here:

```java
String::length
```

references the `length()` instance method.

---

# 🌊 Method Reference with Streams

Method references are heavily used with Streams.

### Lambda

```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

names.stream()
     .map(name -> name.toUpperCase())
     .forEach(name -> System.out.println(name));
```

### Method References

```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

names.stream()
     .map(String::toUpperCase)
     .forEach(System.out::println);
```

The second version is shorter and directly communicates the existing operations.

---

# 🧑‍💻 Method Reference with Custom Classes

Consider:

```java
class Printer {

    void print(String message) {
        System.out.println(message);
    }
}
```

Create an object:

```java
Printer printer = new Printer();
```

Use a method reference:

```java
Consumer<String> consumer = printer::print;

consumer.accept("Hello");
```

Output:

```text
Hello
```

Equivalent Lambda:

```java
Consumer<String> consumer = message -> printer.print(message);
```

---

# 🏛️ Static Method Reference with Custom Class

Consider:

```java
class Calculator {

    static int add(int a, int b) {
        return a + b;
    }
}
```

Method reference:

```java
BinaryOperator<Integer> addition = Calculator::add;

System.out.println(addition.apply(10, 20));
```

Equivalent Lambda:

```java
BinaryOperator<Integer> addition = (a, b) -> Calculator.add(a, b);
```

Output:

```text
30
```

---

# 🔀 Method Reference with Overloaded Methods

Overloaded methods can sometimes make method references ambiguous.

Consider:

```java
class Printer {

    static void print(int value) {
        System.out.println("Integer: " + value);
    }

    static void print(String value) {
        System.out.println("String: " + value);
    }
}
```

The target functional interface helps determine which overloaded method should be selected.

```java
Consumer<String> printer = Printer::print;

printer.accept("Java");
```

The compiler selects:

```java
print(String)
```

because the target type is:

```java
Consumer<String>
```

---

# ⚠️ Method Reference vs Method Invocation

These are completely different.

### Method Invocation

```java
System.out.println("Hello");
```

The method is executed immediately.

### Method Reference

```java
System.out::println
```

The method is referenced for later invocation through a compatible functional interface.

### Remember

```text
()
 ↓
Invocation
 ↓
Execute now
```

```text
::
 ↓
Reference
 ↓
Execute later
```

---

# 🏗️ Constructor Reference in Detail

Constructor reference:

```java
ClassName::new
```

### No-Argument Constructor

```java
Supplier<StringBuilder> creator = StringBuilder::new;

StringBuilder builder = creator.get();

builder.append("Java");

System.out.println(builder);
```

Output:

```text
Java
```

Equivalent Lambda:

```java
Supplier<StringBuilder> creator = () -> new StringBuilder();
```

---

# 🧱 Constructor with Parameter

```java
Function<String, StringBuilder> creator = StringBuilder::new;

StringBuilder builder = creator.apply("Java");

System.out.println(builder);
```

Equivalent Lambda:

```java
Function<String, StringBuilder> creator = text -> new StringBuilder(text);
```

---

# 🧠 Method Reference and Target Type

A method reference does not independently define its complete signature.

The target functional interface provides the required shape.

Example:

```java
Function<String, Integer> length = String::length;
```

The compiler knows:

```java
Function<String, Integer>
```

means:

```text
Input  → String
Output → Integer
```

Therefore:

```java
String::length
```

must represent a compatible operation.

---

# ⚙️ Internal Working

At a high level, method references are translated into behavior compatible with a functional interface.

For example:

```java
Function<String, Integer> length = String::length;
```

Conceptually:

```text
Method Reference
       ↓
Target Functional Interface
       ↓
Method Resolution
       ↓
Compatible Function
       ↓
Invocation
```

Method references use the same general Lambda infrastructure and JVM mechanisms used by Lambda expressions.

---

# 🧠 JVM Perspective

Method references are not simply executed when the reference is written.

For example:

```java
Consumer<String> printer = System.out::println;
```

The method is not called at that line.

The functional interface receives a callable behavior.

When:

```java
printer.accept("Hello");
```

is executed, the referenced method is invoked.

Conceptually:

```text
System.out::println
        ↓
Functional Interface
        ↓
printer.accept(...)
        ↓
println(...)
```

---

# 🆚 Lambda vs Method Reference

| Feature | Lambda | Method Reference |
|---|---|---|
| Syntax | `x -> method(x)` | `Class::method` |
| Flexibility | Higher | Lower |
| Reuses existing method | Yes | Directly |
| Readability | Good | Often better |
| Requires functional interface target | Yes | Yes |
| Java version | Java 8+ | Java 8+ |
| Best use | Custom logic | Existing method delegation |

---

# ✅ When to Use Method Reference

Use a method reference when the Lambda only calls an existing method.

### Good Candidate

```java
name -> System.out.println(name)
```

Can become:

```java
System.out::println
```

### Another Good Candidate

```java
text -> text.toUpperCase()
```

Can become:

```java
String::toUpperCase
```

### Not Always a Good Candidate

If the Lambda performs additional logic:

```java
name -> {
    String upper = name.toUpperCase();
    System.out.println("Name: " + upper);
}
```

Forcing this into a method reference is not appropriate.

---

# 🚨 Common Mistakes

## 1. Confusing `::` with Method Invocation

Wrong understanding:

```java
System.out::println
```

does not immediately print anything.

It creates a method reference.

---

## 2. Forgetting the Target Type

A method reference generally needs a compatible target type.

```java
Consumer<String> printer = System.out::println;
```

---

## 3. Confusing Static and Instance References

Static method:

```java
Math::abs
```

Particular object:

```java
System.out::println
```

Arbitrary object of a class:

```java
String::toUpperCase
```

Constructor:

```java
ArrayList::new
```

---

## 4. Thinking Every Lambda Can Become a Method Reference

Not every Lambda can be replaced with a method reference.

A method reference is appropriate when the Lambda primarily delegates to an existing method or constructor.

---

# 🪤 Interview Traps

## Trap 1: What does `::` mean?

It is the **method reference operator**.

---

## Trap 2: Does a method reference execute the method?

No.

It references the method.

The method is invoked when the functional interface operation is called.

---

## Trap 3: Is method reference a functional interface?

No.

A method reference is an expression.

It can be assigned to a compatible functional interface target.

---

## Trap 4: What are the four types?

Remember:

```text
1. Static Method
2. Instance Method of Particular Object
3. Instance Method of Arbitrary Object
4. Constructor
```

---

## Trap 5: What is the difference between:

```java
System.out::println
```

and:

```java
System.out.println(...)
```

The first is a method reference.

The second is a method invocation.

---

# 🧩 DSA Connection

Method references appear frequently in modern Java DSA code.

## Sorting

```java
students.sort(Comparator.comparingInt(Student::getMarks));
```

## Mapping

```java
List<String> upperNames = names.stream()
        .map(String::toUpperCase)
        .toList();
```

## Traversal

```java
numbers.forEach(System.out::println);
```

## Filtering

```java
names.stream()
        .filter(String::isEmpty)
        .forEach(System.out::println);
```

In interviews, method references can make Stream and Collection code more concise.

---

# 🧠 How to Think About Method References

Whenever you see:

```java
x -> object.method(x)
```

ask:

> "Can I directly reference this method?"

If yes, it may become:

```java
object::method
```

### Example

Lambda:

```java
name -> name.toUpperCase()
```

Method reference:

```java
String::toUpperCase
```

Another example:

Lambda:

```java
name -> System.out.println(name)
```

Method reference:

```java
System.out::println
```

---

# 🔥 Four Types Memory Trick

Remember:

```text
STATIC
    ↓
ClassName::staticMethod

OBJECT
    ↓
object::instanceMethod

ARBITRARY OBJECT
    ↓
ClassName::instanceMethod

CONSTRUCTOR
    ↓
ClassName::new
```

### Easy Memory

```text
Class :: staticMethod
Object :: instanceMethod
Class :: instanceMethod
Class :: new
```

---

# 📋 Quick Cheat Sheet

| Type | Syntax | Example |
|---|---|---|
| Static method | `Class::method` | `Math::abs` |
| Particular object | `object::method` | `System.out::println` |
| Arbitrary object | `Class::method` | `String::length` |
| Constructor | `Class::new` | `ArrayList::new` |

### Lambda → Method Reference

```java
x -> System.out.println(x)
```

↓

```java
System.out::println
```

---

```java
x -> x.toUpperCase()
```

↓

```java
String::toUpperCase
```

---

```java
x -> Math.abs(x)
```

↓

```java
Math::abs
```

---

```java
() -> new ArrayList<>()
```

↓

```java
ArrayList::new
```

---

# 🎤 30-Second Interview Answer

> "A method reference is a shorthand syntax introduced in Java 8 for referring to an existing method or constructor. It uses the `::` operator and is mainly used when a Lambda simply delegates to an existing method. There are four types: static method references, instance methods of a particular object, instance methods of an arbitrary object, and constructor references. For example, `System.out::println` is equivalent to `message -> System.out.println(message)`."

---

# 🎯 Top 10 Interview Questions

## 1. What is a Method Reference?

**Answer:**

A method reference is a shorthand syntax for referring to an existing method or constructor using the `::` operator.

Example:

```java
System.out::println
```

---

## 2. When were Method References introduced?

**Answer:**

Method References were introduced in **Java 8**.

---

## 3. What operator is used for Method References?

**Answer:**

The double-colon operator:

```java
::
```

---

## 4. What are the four types of Method References?

**Answer:**

1. Static method reference
2. Instance method of a particular object
3. Instance method of an arbitrary object
4. Constructor reference

---

## 5. What is the difference between Lambda and Method Reference?

**Answer:**

A Lambda explicitly describes behavior, while a method reference directly references an existing method or constructor.

Example:

```java
name -> System.out.println(name)
```

can become:

```java
System.out::println
```

---

## 6. Does a Method Reference execute the method immediately?

**Answer:**

No.

It creates a reference to the method. The method is invoked later through the compatible functional interface operation.

---

## 7. Can Method References work without Functional Interfaces?

**Answer:**

A method reference needs a compatible target type. Functional interfaces are the common target type for method references.

---

## 8. What is a Constructor Reference?

**Answer:**

A constructor reference uses:

```java
ClassName::new
```

Example:

```java
Supplier<ArrayList<String>> creator = ArrayList::new;
```

---

## 9. What is the difference between `String::length` and `someString::length`?

**Answer:**

```java
String::length
```

refers to the instance method for an arbitrary String object supplied through the functional interface.

```java
someString::length
```

refers to the method of one particular already-existing String object.

---

## 10. Can every Lambda be converted to a Method Reference?

**Answer:**

No.

A method reference is suitable when the Lambda primarily delegates to an existing method or constructor.

---

# 📌 Key Takeaways

- Method References were introduced in **Java 8**.
- They use the `::` operator.
- They provide shorthand syntax for existing methods and constructors.
- They require a compatible target type.
- They are closely related to Lambda Expressions.
- `System.out::println` is a common example.
- `String::length` represents an instance method of an arbitrary String.
- `Math::abs` represents a static method.
- `ArrayList::new` represents a constructor.
- Method references do not invoke methods immediately.
- They are heavily used with Collections and Streams.
- They can improve readability when a Lambda only delegates to an existing method.

---

# 🧠 Final Mental Model

```text
Lambda
   ↓
x -> object.method(x)
   ↓
If only delegation is happening
   ↓
Method Reference
   ↓
object::method
```

### Four Forms

```text
ClassName::staticMethod
        ↓
Static Method

object::instanceMethod
        ↓
Particular Object

ClassName::instanceMethod
        ↓
Arbitrary Object

ClassName::new
        ↓
Constructor
```

> **Remember:** A Method Reference is essentially a more concise way of saying, **"Use this existing method as the behavior."**