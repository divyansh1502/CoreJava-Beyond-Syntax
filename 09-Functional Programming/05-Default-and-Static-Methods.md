# ⚙️ Default and Static Methods in Interfaces

> **Default methods** allow interfaces to provide instance method implementations, while **static methods** allow interfaces to define utility methods that belong to the interface itself.

---

## 📌 Table of Contents

1. [Introduction](#-introduction)
2. [Why Default Methods Were Introduced](#-why-default-methods-were-introduced)
3. [Default Method](#-default-method)
4. [Syntax of Default Method](#-syntax-of-default-method)
5. [Using Default Methods](#-using-default-methods)
6. [Overriding Default Methods](#-overriding-default-methods)
7. [Default Method Inheritance](#-default-method-inheritance)
8. [Multiple Interface Conflict](#-multiple-interface-conflict)
9. [Resolving Default Method Conflict](#-resolving-default-method-conflict)
10. [Class vs Interface Priority](#-class-vs-interface-priority)
11. [Static Methods in Interfaces](#-static-methods-in-interfaces)
12. [Calling Interface Static Methods](#-calling-interface-static-methods)
13. [Static Methods Are Not Inherited](#-static-methods-are-not-inherited)
14. [Default vs Static Methods](#-default-vs-static-methods)
15. [Default vs Abstract Methods](#-default-vs-abstract-methods)
16. [Interface Evolution](#-interface-evolution)
17. [Default Methods and Functional Interfaces](#-default-methods-and-functional-interfaces)
18. [Default Methods and Lambda Expressions](#-default-methods-and-lambda-expressions)
19. [Default Methods and Method References](#-default-methods-and-method-references)
20. [Real-World Example](#-real-world-example)
21. [Internal Working](#-internal-working)
22. [JVM Perspective](#-jvm-perspective)
23. [Advantages](#-advantages)
24. [Disadvantages](#-disadvantages)
25. [Common Mistakes](#-common-mistakes)
26. [Interview Traps](#-interview-traps)
27. [DSA Connection](#-dsa-connection)
28. [Quick Cheat Sheet](#-quick-cheat-sheet)
29. [30-Second Interview Answer](#-30-second-interview-answer)
30. [Top 10 Interview Questions](#-top-10-interview-questions)

---

# 🚀 Introduction

Before Java 8, interfaces were primarily used to define contracts through abstract methods and constants.

Example:

```java
interface Vehicle {

    void start();
}
```

A class implementing the interface had to provide the implementation:

```java
class Car implements Vehicle {

    @Override
    public void start() {
        System.out.println("Car starts");
    }
}
```

Java 8 introduced:

- `default` methods
- `static` methods

These features made interfaces more flexible and allowed existing interfaces to evolve without immediately breaking their implementations.

---

# 🤔 Why Were Default Methods Introduced?

Consider an existing interface:

```java
interface Vehicle {

    void start();
}
```

Suppose many classes already implement it:

```java
class Car implements Vehicle {

    @Override
    public void start() {
        System.out.println("Car starts");
    }
}
```

```java
class Bike implements Vehicle {

    @Override
    public void start() {
        System.out.println("Bike starts");
    }
}
```

Now imagine adding another abstract method:

```java
interface Vehicle {

    void start();

    void stop();
}
```

Every existing implementation would need to implement `stop()`.

That can cause compatibility problems for existing implementations.

Instead, Java can provide a default implementation:

```java
interface Vehicle {

    void start();

    default void stop() {
        System.out.println("Vehicle stops");
    }
}
```

Now existing classes can continue working without implementing `stop()`.

---

# 🧩 Default Method

A **default method** is an instance method inside an interface that contains an implementation.

It is declared using the `default` keyword.

Example:

```java
interface Vehicle {

    void start();

    default void stop() {
        System.out.println("Vehicle stops");
    }
}
```

Here:

```java
void start();
```

is an abstract method.

And:

```java
default void stop() {
    System.out.println("Vehicle stops");
}
```

is a default method.

---

# 📝 Syntax of Default Method

General syntax:

```java
interface InterfaceName {

    default returnType methodName(parameters) {
        // implementation
    }
}
```

Example:

```java
interface Greeting {

    default void sayHello() {
        System.out.println("Hello");
    }
}
```

A class can implement the interface:

```java
class Person implements Greeting {
}
```

Then:

```java
Person person = new Person();

person.sayHello();
```

Output:

```text
Hello
```

The implementing class automatically gets the default behavior.

---

# ▶️ Using Default Methods

Example:

```java
interface Vehicle {

    default void stop() {
        System.out.println("Vehicle stops");
    }
}
```

Implementation:

```java
class Car implements Vehicle {
}
```

Usage:

```java
Car car = new Car();

car.stop();
```

Output:

```text
Vehicle stops
```

The `Car` class does not explicitly define `stop()`, but it can use the inherited default method.

---

# 🔄 Overriding Default Methods

A class can override a default method.

Example:

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

Usage:

```java
Car car = new Car();

car.stop();
```

Output:

```text
Car stops
```

The class's implementation replaces the inherited default behavior.

---

# 🧠 Calling the Interface's Default Implementation

Inside an implementing class, we can explicitly call the interface's default implementation using:

```java
InterfaceName.super.methodName();
```

Example:

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

        Vehicle.super.stop();
    }
}
```

Output:

```text
Car stops
Vehicle stops
```

This is useful when the class wants to extend the default behavior rather than completely replace it.

---

# 🧬 Default Method Inheritance

If a class implements an interface containing a default method, the class can inherit that method.

Example:

```java
interface Animal {

    default void sound() {
        System.out.println("Animal makes sound");
    }
}
```

```java
class Dog implements Animal {
}
```

Usage:

```java
Dog dog = new Dog();

dog.sound();
```

Output:

```text
Animal makes sound
```

The `Dog` class inherits the default implementation.

---

# 🔀 Multiple Interface Conflict

Java supports multiple interface inheritance.

Consider:

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

Now:

```java
class Test implements A, B {
}
```

This creates a conflict.

The compiler cannot decide whether `show()` should come from `A` or `B`.

Therefore, the class must resolve the conflict.

---

# 🛠️ Resolving Default Method Conflict

The implementing class can override the conflicting method.

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
class Test implements A, B {

    @Override
    public void show() {
        System.out.println("Test");
    }
}
```

Now:

```java
Test test = new Test();

test.show();
```

Output:

```text
Test
```

---

# 🎯 Choosing One Interface's Default Method

Instead of writing a completely new implementation, the class can explicitly choose one interface's default implementation.

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
class Test implements A, B {

    @Override
    public void show() {
        A.super.show();
    }
}
```

Output:

```text
A
```

Or:

```java
class Test implements A, B {

    @Override
    public void show() {
        B.super.show();
    }
}
```

Output:

```text
B
```

---

# 🥇 Class vs Interface Priority

One important rule is:

> **A class method takes priority over an interface default method.**

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
        System.out.println("Interface A");
    }
}
```

```java
class Child extends Parent implements A {
}
```

Usage:

```java
Child child = new Child();

child.show();
```

Output:

```text
Parent
```

The inherited class method wins over the interface default method.

### Priority Rule

```text
Class method
     ↓
Interface default method
```

This prevents an interface from unexpectedly overriding behavior inherited from a superclass.

---

# 🧱 Static Methods in Interfaces

Java 8 also introduced static methods in interfaces.

A static interface method belongs to the **interface itself**, not to objects implementing the interface.

Example:

```java
interface Calculator {

    static int add(int a, int b) {
        return a + b;
    }
}
```

The method is called using the interface name:

```java
int result = Calculator.add(10, 20);

System.out.println(result);
```

Output:

```text
30
```

---

# 📞 Calling Interface Static Methods

Static interface methods are called using:

```java
InterfaceName.methodName();
```

Example:

```java
interface Utility {

    static void printMessage() {
        System.out.println("Utility method");
    }
}
```

Call it:

```java
Utility.printMessage();
```

Output:

```text
Utility method
```

---

# 🚫 Static Methods Are Not Inherited

This is an important difference between default and static methods.

Consider:

```java
interface Utility {

    static void printMessage() {
        System.out.println("Utility");
    }
}
```

```java
class Demo implements Utility {
}
```

This is valid:

```java
Utility.printMessage();
```

But this is not:

```java
Demo.printMessage();
```

The static method belongs to the interface.

It is not inherited by the implementing class.

---

# ⚖️ Default vs Static Methods

| Feature | Default Method | Static Method |
|---|---|---|
| Keyword | `default` | `static` |
| Has body | Yes | Yes |
| Belongs to | Object / implementation | Interface |
| Inherited by class | Yes | No |
| Called through object | Yes | No |
| Called through interface | Not normally | Yes |
| Can be overridden | Yes | No |
| Can use `this` | Yes | No |
| Main purpose | Default behavior | Utility behavior |

### Example

Default:

```java
interface Vehicle {

    default void start() {
        System.out.println("Starting");
    }
}
```

Static:

```java
interface Vehicle {

    static void info() {
        System.out.println("Vehicle interface");
    }
}
```

Calls:

```java
Car car = new Car();

car.start();

Vehicle.info();
```

---

# 🔄 Default vs Abstract Methods

| Feature | Abstract Method | Default Method |
|---|---|---|
| Has implementation | No | Yes |
| Body | No | Yes |
| Keyword | None required | `default` |
| Implementing class must implement | Yes | No |
| Can be overridden | Yes | Yes |
| Functional interface compatible | Yes | Yes |
| Introduced as Java 8 feature | No | Yes |

Example abstract method:

```java
interface Vehicle {

    void start();
}
```

Example default method:

```java
interface Vehicle {

    default void stop() {
        System.out.println("Stopping");
    }
}
```

---

# 🧬 Interface Evolution

One of the major reasons default methods were introduced was **interface evolution**.

Imagine Java's Collection API already had many implementations.

Adding a new abstract method directly could require every implementation to change.

Default methods allow Java APIs to introduce new behavior while preserving compatibility for existing implementations in many cases.

Example:

```java
interface Vehicle {

    void start();

    default void stop() {
        System.out.println("Default stop");
    }
}
```

Existing implementations don't have to immediately implement `stop()`.

---

# 🔗 Default Methods and Functional Interfaces

A functional interface can contain default methods.

The rule is:

> A functional interface must have exactly one abstract method.

Default methods do not count as abstract methods.

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

This is still a valid functional interface.

It has:

```text
1 abstract method
+
1 default method
```

Therefore, it can be used with a Lambda.

```java
Calculator calculator = (a, b) -> a + b;

System.out.println(calculator.calculate(10, 20));

calculator.info();
```

Output:

```text
30
Calculator
```

---

# 🧠 Default Methods and Lambda Expressions

A Lambda implements the **abstract method** of a functional interface.

It does not implement default methods.

Example:

```java
@FunctionalInterface
interface Greeting {

    void greet();

    default void message() {
        System.out.println("Welcome");
    }
}
```

Lambda:

```java
Greeting greeting = () -> System.out.println("Hello");
```

The Lambda provides the implementation for:

```java
void greet();
```

The default method remains available:

```java
greeting.message();
```

---

# 🔗 Default Methods and Method References

Default methods can coexist with method references.

Example:

```java
interface Printer {

    void print(String message);

    default void info() {
        System.out.println("Printer interface");
    }
}
```

A method reference can implement the abstract method:

```java
Printer printer = System.out::println;

printer.print("Hello");
printer.info();
```

Output:

```text
Hello
Printer interface
```

---

# 🏗️ Real-World Example

Consider a payment system.

```java
interface Payment {

    void pay(double amount);

    default void receipt() {
        System.out.println("Receipt generated");
    }

    static void rules() {
        System.out.println("Payment rules");
    }
}
```

Implementation:

```java
class UpiPayment implements Payment {

    @Override
    public void pay(double amount) {
        System.out.println("Paid using UPI: " + amount);
    }
}
```

Usage:

```java
UpiPayment payment = new UpiPayment();

payment.pay(500);
payment.receipt();

Payment.rules();
```

Output:

```text
Paid using UPI: 500.0
Receipt generated
Payment rules
```

Here:

```text
pay()
 ↓
Abstract method
 ↓
Implemented by class
```

```text
receipt()
 ↓
Default method
 ↓
Inherited by class
```

```text
rules()
 ↓
Static method
 ↓
Called using interface name
```

---

# 🧠 Can Default Methods Be Private?

Yes.

Java 9 introduced private methods in interfaces.

Private interface methods can be used to share implementation logic between methods inside the same interface.

Example:

```java
interface Vehicle {

    default void start() {
        log();
        System.out.println("Vehicle starts");
    }

    default void stop() {
        log();
        System.out.println("Vehicle stops");
    }

    private void log() {
        System.out.println("Vehicle operation");
    }
}
```

The private method:

```java
log()
```

cannot be called from outside the interface or directly by implementing classes.

It is an internal helper method.

---

# 🔒 Private Static Methods in Interfaces

Interfaces can also contain private static methods.

Example:

```java
interface Utility {

    static void methodA() {
        helper();
    }

    static void methodB() {
        helper();
    }

    private static void helper() {
        System.out.println("Common logic");
    }
}
```

The private static method is only accessible inside the interface.

---

# ⚙️ Internal Working

Default methods are real interface methods containing implementations.

When a class does not override a default method, method resolution can use the interface's default implementation.

Conceptually:

```text
Object method call
       ↓
Class method?
       ↓
If not
       ↓
Applicable interface default method
       ↓
Execute default implementation
```

When a class overrides the method:

```text
Object method call
       ↓
Class implementation found
       ↓
Execute class method
```

---

# 🧠 JVM Perspective

A default method is represented as a method in the interface with an implementation.

Unlike abstract interface methods, it has executable bytecode.

Example:

```java
interface Vehicle {

    default void stop() {
        System.out.println("Stopping");
    }
}
```

The JVM can execute the bytecode associated with the default method when that implementation is selected during method resolution.

Static interface methods are associated with the interface and are invoked using the interface type.

---

# 🔍 Important Internal Difference

### Default Method

```java
vehicle.stop();
```

The call operates on an object.

### Static Method

```java
Vehicle.info();
```

The call is associated with the interface itself.

Think:

```text
default
   ↓
object behavior
```

```text
static
   ↓
interface-level utility
```

---

# ⚡ Advantages of Default Methods

## 1. Interface Evolution

Existing implementations can continue working when new behavior is added through defaults.

## 2. Code Reuse

Common behavior can be defined once inside an interface.

## 3. Backward Compatibility

They help evolve widely used APIs without immediately requiring every implementation to change.

## 4. Multiple Interface Composition

Interfaces can provide reusable default behavior.

## 5. Works with Functional Interfaces

Default methods allow functional interfaces to contain additional non-abstract behavior.

---

# ⚠️ Disadvantages of Default Methods

## 1. Multiple Inheritance Conflicts

Two interfaces can provide the same default method.

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

The implementing class must resolve the conflict.

## 2. Can Make Interfaces More Complex

Interfaces can now contain substantial behavior, making design decisions more important.

## 3. Default Behavior May Not Fit Every Implementation

A generic default implementation may not make sense for every implementing class.

---

# 🚨 Common Mistakes

## Mistake 1: Thinking Static Interface Methods Are Inherited

Wrong:

```java
Demo.print();
```

if `print()` is declared as a static method inside an interface implemented by `Demo`.

Correct:

```java
InterfaceName.print();
```

---

## Mistake 2: Thinking Default Methods Are Abstract

A default method already has an implementation.

```java
default void show() {
    System.out.println("Hello");
}
```

The implementing class does not have to override it.

---

## Mistake 3: Forgetting `public` When Implementing an Interface Method

Interface methods are public.

Therefore, an implementation cannot reduce visibility.

Correct:

```java
@Override
public void show() {
    System.out.println("Hello");
}
```

Not:

```java
@Override
void show() {
    System.out.println("Hello");
}
```

---

## Mistake 4: Calling `Interface.super.method()` from an Unrelated Context

This syntax is used inside an implementing class to access a specific interface's default method.

```java
class Demo implements A {

    @Override
    public void show() {
        A.super.show();
    }
}
```

---

# 🪤 Interview Traps

## Trap 1: Can an interface have implemented methods?

Yes.

Since Java 8, interfaces can have:

- Default methods
- Static methods

Since Java 9, interfaces can also have:

- Private methods
- Private static methods

---

## Trap 2: Can a static interface method be overridden?

No.

Static methods belong to the interface itself and are not inherited as instance methods.

---

## Trap 3: Can a default method be overridden?

Yes.

An implementing class can override it.

---

## Trap 4: Can an interface have multiple default methods?

Yes.

There is no rule limiting an interface to one default method.

The one-abstract-method rule applies to **functional interfaces**, not default methods.

---

## Trap 5: Does a default method count as the abstract method of a functional interface?

No.

Default methods are not abstract methods.

Example:

```java
@FunctionalInterface
interface Task {

    void execute();

    default void info() {
        System.out.println("Task");
    }
}
```

This is valid.

---

## Trap 6: Can a default method be static?

No.

`default` methods are instance methods.

Static methods are declared separately using `static`.

---

## Trap 7: Which wins: class method or interface default method?

A class method takes priority over an interface default method.

---

# 🧩 DSA Connection

Default and static interface methods are not usually the main focus of DSA algorithms, but they appear in Java's Collection and functional APIs.

For example, many modern Java interfaces provide useful default behavior.

Common API methods influenced by interface evolution include:

```java
List<String> names = new ArrayList<>();

names.replaceAll(String::toUpperCase);
```

And:

```java
names.removeIf(name -> name.isEmpty());
```

These APIs demonstrate how interface evolution can add behavior without requiring every implementation to independently define the method.

### DSA Relevance

Understand default methods because they help you understand:

- Collection APIs
- Functional interfaces
- Stream-related APIs
- Comparator APIs
- Lambda-based operations
- Modern Java interface design

---

# 🧠 How to Think About Default Methods

Think:

```text
Interface
    ↓
Contract
    +
Reusable default behavior
```

Example:

```java
interface Vehicle {

    void start();

    default void stop() {
        System.out.println("Stopping");
    }
}
```

The interface says:

```text
Every Vehicle must start.
Every Vehicle gets a default way to stop.
```

---

# 🧠 How to Think About Static Methods

Think:

```text
Interface
    ↓
Utility / helper behavior
    ↓
Call through interface name
```

Example:

```java
interface MathUtils {

    static int square(int number) {
        return number * number;
    }
}
```

Call:

```java
MathUtils.square(5);
```

---

# 🔥 Important Rules

```text
DEFAULT METHOD
      ↓
Instance method
      ↓
Can be inherited
      ↓
Can be overridden
      ↓
Called through object
```

```text
STATIC METHOD
      ↓
Interface-level method
      ↓
Not inherited
      ↓
Cannot be overridden
      ↓
Called through interface name
```

---

# 📊 Complete Comparison

| Feature | Abstract | Default | Static |
|---|---|---|---|
| Has body | ❌ | ✅ | ✅ |
| Instance method | ✅ | ✅ | ❌ |
| Can be inherited | ✅ | ✅ | ❌ |
| Can be overridden | ✅ | ✅ | ❌ |
| Called through object | ✅ | ✅ | ❌ |
| Called through interface | ❌ | Not normally | ✅ |
| Can use `this` | Yes | Yes | No |
| Java 8 feature | ❌ | ✅ | ✅ |
| Can participate in functional interface | As the one abstract method | Does not count | Does not count |

---

# 📋 Quick Cheat Sheet

### Default Method

```java
interface A {

    default void show() {
        System.out.println("A");
    }
}
```

Call:

```java
new Demo().show();
```

---

### Override Default Method

```java
class Demo implements A {

    @Override
    public void show() {
        System.out.println("Demo");
    }
}
```

---

### Call Specific Default Method

```java
A.super.show();
```

---

### Static Interface Method

```java
interface A {

    static void info() {
        System.out.println("Info");
    }
}
```

Call:

```java
A.info();
```

---

### Functional Interface with Default Method

```java
@FunctionalInterface
interface Calculator {

    int calculate(int a, int b);

    default void info() {
        System.out.println("Calculator");
    }
}
```

Lambda:

```java
Calculator calculator = (a, b) -> a + b;
```

---

# 🧠 Memory Trick

Remember:

```text
ABSTRACT
    ↓
"You must implement this."

DEFAULT
    ↓
"You may use my implementation."

STATIC
    ↓
"Call me using the interface name."
```

---

# 🎤 30-Second Interview Answer

> "Java 8 introduced default and static methods in interfaces. A default method is an instance method with an implementation, so implementing classes can inherit it or override it. Default methods were mainly introduced to help evolve existing interfaces without forcing every implementation to immediately implement newly added methods. A static interface method belongs to the interface itself, is not inherited by implementing classes, and is called using the interface name. If multiple interfaces provide conflicting default methods, the implementing class must resolve the conflict."

---

# 🎯 Top 10 Interview Questions

## 1. Why were default methods introduced in Java?

**Answer:**

Default methods were introduced mainly to allow interfaces to evolve by adding implementations without forcing every existing implementation to immediately implement a new abstract method.

---

## 2. Can an interface contain implemented methods?

**Answer:**

Yes.

Modern Java interfaces can contain:

- Default methods
- Static methods
- Private methods
- Private static methods

---

## 3. Can default methods be overridden?

**Answer:**

Yes.

```java
interface A {

    default void show() {
        System.out.println("A");
    }
}
```

```java
class B implements A {

    @Override
    public void show() {
        System.out.println("B");
    }
}
```

---

## 4. Can static interface methods be overridden?

**Answer:**

No.

Static methods belong to the interface and are not inherited as instance methods.

---

## 5. How do you call a static method from an interface?

**Answer:**

Use the interface name.

```java
InterfaceName.methodName();
```

Example:

```java
Calculator.add(10, 20);
```

---

## 6. What happens when two interfaces have the same default method?

**Answer:**

The implementing class gets a conflict and must override the method to resolve it.

```java
class Demo implements A, B {

    @Override
    public void show() {
        A.super.show();
    }
}
```

---

## 7. Which has higher priority: class method or interface default method?

**Answer:**

The class method has priority over an interface default method.

---

## 8. Can a functional interface contain default methods?

**Answer:**

Yes.

A functional interface only requires exactly one abstract method.

Default methods do not count as abstract methods.

---

## 9. Can a default method be called using `InterfaceName.super`?

**Answer:**

Yes, from an implementing class when explicitly selecting that interface's default implementation.

```java
A.super.show();
```

---

## 10. What is the difference between default and static methods?

**Answer:**

A default method is an instance method that can be inherited and overridden.

A static interface method belongs to the interface itself, is not inherited, and is called using the interface name.

---

# 🔥 Final Interview Memory

```text
Java 8
   ↓
Interfaces became more powerful
   ↓
DEFAULT
   ↓
Instance behavior
   ↓
Can be inherited + overridden
```

```text
Java 8
   ↓
STATIC
   ↓
Interface utility behavior
   ↓
Not inherited
   ↓
Called using InterfaceName.method()
```

```text
Multiple Default Methods
          ↓
      Conflict
          ↓
Implementing Class
          ↓
       Override
          ↓
A.super.method()
or
B.super.method()
```

> **Core idea:**  
> **Default methods provide reusable instance behavior inside interfaces, while static methods provide interface-level utility behavior.**