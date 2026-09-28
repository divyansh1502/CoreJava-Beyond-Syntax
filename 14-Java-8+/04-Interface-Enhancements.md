
# 🔌 Interface Enhancements in Java

> **Java 8 significantly enhanced interfaces by allowing default and static methods, while later Java versions added private interface methods, making interfaces more flexible without removing their core contract-based design.**

---

# 📑 Table of Contents

- [1. Why Were Interfaces Enhanced?](#1-why-were-interfaces-enhanced)
- [2. Interface Before Java 8](#2-interface-before-java-8)
- [3. Java 8 Interface Enhancements](#3-java-8-interface-enhancements)
- [4. Default Methods](#4-default-methods)
- [5. Why Default Methods Were Introduced](#5-why-default-methods-were-introduced)
- [6. Default Method Syntax](#6-default-method-syntax)
- [7. Implementing a Default Method](#7-implementing-a-default-method)
- [8. Overriding Default Methods](#8-overriding-default-methods)
- [9. Interface Inheritance with Default Methods](#9-interface-inheritance-with-default-methods)
- [10. Multiple Interface Default Method Conflict](#10-multiple-interface-default-method-conflict)
- [11. Resolving Default Method Conflict](#11-resolving-default-method-conflict)
- [12. Interface vs Class Priority](#12-interface-vs-class-priority)
- [13. Calling Interface Default Method Explicitly](#13-calling-interface-default-method-explicitly)
- [14. Static Methods in Interfaces](#14-static-methods-in-interfaces)
- [15. Interface Static Method Rules](#15-interface-static-method-rules)
- [16. Default vs Static Methods](#16-default-vs-static-methods)
- [17. Private Methods in Interfaces](#17-private-methods-in-interfaces)
- [18. Why Private Interface Methods Were Introduced](#18-why-private-interface-methods-were-introduced)
- [19. Private Static Methods](#19-private-static-methods)
- [20. Interface Variables](#20-interface-variables)
- [21. Functional Interfaces](#21-functional-interfaces)
- [22. Interface Enhancements and Lambda Expressions](#22-interface-enhancements-and-lambda-expressions)
- [23. Object Methods in Interfaces](#23-object-methods-in-interfaces)
- [24. Interface Evolution](#24-interface-evolution)
- [25. Common Mistakes](#25-common-mistakes)
- [26. Interview Questions](#26-interview-questions)
- [27. 30-Second Interview Answer](#27-30-second-interview-answer)
- [28. Cheat Sheet](#28-cheat-sheet)
- [29. Final Mental Model](#29-final-mental-model)

---

# 1. Why Were Interfaces Enhanced?

Before Java 8, an interface could primarily contain:

```text
Abstract methods
Constants
```

Suppose an interface was already implemented by hundreds of classes:

```java
interface Payment {

    void pay();
}
```

Later, suppose the interface designer wanted to add:

```java
void refund();
```

Existing implementing classes would fail to compile because they were suddenly required to implement the new abstract method.

This created an interface evolution problem.

Java 8 introduced:

```text
default methods
static methods
```

to make interfaces more flexible.

Java 9 later introduced:

```text
private interface methods
```

---

# 2. Interface Before Java 8

A traditional interface could contain abstract methods:

```java
interface Vehicle {

    void start();

    void stop();
}
```

A class implements the interface:

```java
class Car implements Vehicle {

    public void start() {
        System.out.println("Car started");
    }

    public void stop() {
        System.out.println("Car stopped");
    }
}
```

The implementing class had to provide implementations for the abstract methods.

---

# 3. Java 8 Interface Enhancements

Java 8 introduced two major interface features:

```text
default methods
static methods
```

Example:

```java
interface Vehicle {

    void start();

    default void horn() {
        System.out.println("Horn");
    }

    static void info() {
        System.out.println("Vehicle interface");
    }
}
```

Java 9 added:

```text
private methods
private static methods
```

Example:

```java
interface Vehicle {

    private void helper() {
        System.out.println("Helper");
    }

    private static void staticHelper() {
        System.out.println("Static helper");
    }
}
```

---

# 4. Default Methods

A default method is a method inside an interface that has an implementation.

Syntax:

```java
interface Vehicle {

    default void horn() {
        System.out.println("Horn");
    }
}
```

The implementing class does not have to override the default method.

Example:

```java
interface Vehicle {

    default void horn() {
        System.out.println("Horn");
    }
}
```

```java
class Car implements Vehicle {
}
```

Now:

```java
Car car =
    new Car();

car.horn();
```

Output:

```text
Horn
```

---

# 5. Why Default Methods Were Introduced

The major reason was:

```text
Interface evolution
```

Suppose an existing interface is:

```java
interface Payment {

    void pay();
}
```

Many classes implement it:

```java
class CardPayment implements Payment {

    public void pay() {
        System.out.println("Card payment");
    }
}
```

Later, we want to add:

```java
void refund();
```

If we add it as an abstract method:

```java
interface Payment {

    void pay();

    void refund();
}
```

then every implementing class must implement `refund()`.

Instead, Java 8 allows:

```java
interface Payment {

    void pay();

    default void refund() {
        System.out.println(
            "Default refund"
        );
    }
}
```

Existing implementations can continue working.

---

# 6. Default Method Syntax

The keyword is:

```text
default
```

Example:

```java
interface Animal {

    default void sound() {
        System.out.println("Some sound");
    }
}
```

Important:

```text
default methods
→ must have a body
```

---

# 7. Implementing a Default Method

A class can inherit a default method without overriding it.

Example:

```java
interface Animal {

    default void eat() {
        System.out.println("Eating");
    }
}
```

```java
class Dog implements Animal {
}
```

Then:

```java
Dog dog =
    new Dog();

dog.eat();
```

Output:

```text
Eating
```

---

# 8. Overriding Default Methods

A class can override a default method.

Example:

```java
interface Animal {

    default void sound() {
        System.out.println("Animal sound");
    }
}
```

```java
class Dog implements Animal {

    @Override
    public void sound() {
        System.out.println("Bark");
    }
}
```

Now:

```java
Dog dog =
    new Dog();

dog.sound();
```

Output:

```text
Bark
```

The class implementation takes precedence over the inherited default implementation.

---

# 9. Interface Inheritance with Default Methods

Interfaces can extend other interfaces.

Example:

```java
interface A {

    default void show() {
        System.out.println("A");
    }
}
```

```java
interface B extends A {
}
```

A class implementing `B` can use the inherited default method:

```java
class Test implements B {
}
```

```java
Test obj =
    new Test();

obj.show();
```

Output:

```text
A
```

---

# 10. Multiple Interface Default Method Conflict

Java allows a class to implement multiple interfaces.

Example:

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

This creates ambiguity.

Which `show()` should `Test` inherit?

```text
A.show()
       \
        → ???
       /
B.show()
```

Java does not automatically choose one.

The class must resolve the conflict.

---

# 11. Resolving Default Method Conflict

The implementing class can override the method.

```java
class Test implements A, B {

    @Override
    public void show() {
        System.out.println("Test");
    }
}
```

Now there is no ambiguity.

---

## Calling a Specific Interface's Default Method

The class can explicitly choose one interface implementation.

Syntax:

```text
InterfaceName.super.methodName()
```

Example:

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

# 12. Interface vs Class Priority

A class implementation has priority over an interface default method.

Suppose:

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
        System.out.println("Interface");
    }
}
```

Now:

```java
class Child
    extends Parent
    implements A {
}
```

Calling:

```java
Child obj =
    new Child();

obj.show();
```

Output:

```text
Parent
```

The inherited class method wins over the interface default method.

---

## Important Rule

Remember:

```text
Class method
     ↓
wins over
     ↓
Interface default method
```

This prevents an interface from unexpectedly overriding an existing class implementation.

---

# 13. Calling Interface Default Method Explicitly

An implementing class can explicitly invoke an interface's default method.

Example:

```java
interface A {

    default void show() {
        System.out.println("A");
    }
}
```

```java
class Test implements A {

    @Override
    public void show() {

        System.out.println("Test");

        A.super.show();
    }
}
```

Output:

```text
Test
A
```

Important:

```text
A.super.show()
```

is used for a default interface method.

---

# 14. Static Methods in Interfaces

Java 8 also allows static methods in interfaces.

Example:

```java
interface Calculator {

    static int add(
        int a,
        int b
    ) {
        return a + b;
    }
}
```

Call it using the interface name:

```java
int result =
    Calculator.add(10, 20);

System.out.println(result);
```

Output:

```text
30
```

---

# 15. Interface Static Method Rules

A static interface method belongs to the interface itself.

It is not inherited by implementing classes in the normal instance-method sense.

Correct:

```java
Calculator.add(10, 20);
```

Not:

```java
Calculator calculator =
    new Calculator();
```

and not:

```java
calculator.add(10, 20);
```

The important access form is:

```text
InterfaceName.staticMethod()
```

---

## Static Methods Have Bodies

Unlike abstract methods, interface static methods must have an implementation.

Example:

```java
interface Utility {

    static void print() {
        System.out.println("Utility");
    }
}
```

---

# 16. Default vs Static Methods

| Feature | Default Method | Static Method |
|---|---|---|
| Introduced | Java 8 | Java 8 |
| Has body | Yes | Yes |
| Called through object | Yes | No |
| Called through interface | Not normally | Yes |
| Inherited by implementing class | Yes | No |
| Can be overridden | Yes | No |
| Uses `this` | Yes | No |
| Purpose | Add instance behavior | Utility/interface-level behavior |

Example default:

```java
interface A {

    default void show() {
        System.out.println("Default");
    }
}
```

Call:

```java
A obj =
    new Test();

obj.show();
```

Example static:

```java
interface A {

    static void show() {
        System.out.println("Static");
    }
}
```

Call:

```java
A.show();
```

---

# 17. Private Methods in Interfaces

Java 9 introduced private methods in interfaces.

They allow interfaces to contain helper methods used internally by their own default or static methods.

Example:

```java
interface Printer {

    default void print() {
        validate();
        System.out.println("Printing");
    }

    private void validate() {
        System.out.println("Validating");
    }
}
```

The private method is accessible only inside the interface.

---

# 18. Why Private Interface Methods Were Introduced

Consider multiple default methods sharing common logic.

Without a private helper:

```java
interface Service {

    default void start() {

        System.out.println(
            "Validation"
        );

        System.out.println(
            "Start"
        );
    }

    default void stop() {

        System.out.println(
            "Validation"
        );

        System.out.println(
            "Stop"
        );
    }
}
```

The validation logic is duplicated.

A private helper can remove duplication:

```java
interface Service {

    default void start() {

        validate();

        System.out.println(
            "Start"
        );
    }

    default void stop() {

        validate();

        System.out.println(
            "Stop"
        );
    }

    private void validate() {

        System.out.println(
            "Validation"
        );
    }
}
```

Now both default methods reuse the same internal logic.

---

# 19. Private Static Methods

Interfaces can also have private static methods.

Example:

```java
interface Utility {

    static void methodOne() {

        helper();
    }

    static void methodTwo() {

        helper();
    }

    private static void helper() {

        System.out.println(
            "Common logic"
        );
    }
}
```

The private static method is accessible only inside the interface.

---

## Version

Private interface methods were introduced in:

```text
Java 9
```

---

# 20. Interface Variables

Interface variables are implicitly:

```text
public
static
final
```

Example:

```java
interface Constants {

    int MAX =
        100;
}
```

This is equivalent to:

```java
interface Constants {

    public static final int MAX =
        100;
}
```

Therefore:

```java
System.out.println(
    Constants.MAX
);
```

---

## Cannot Reassign

Because interface fields are final:

```java
interface Constants {

    int MAX = 100;
}
```

This is invalid:

```java
Constants.MAX = 200;
```

---

# 21. Functional Interfaces

A functional interface contains exactly one abstract method.

Example:

```java
@FunctionalInterface
interface Calculator {

    int calculate(
        int a,
        int b
    );
}
```

It can have:

```text
One abstract method
+
Multiple default methods
+
Multiple static methods
+
Private helper methods
```

The rule applies to:

```text
Abstract methods
```

not total methods.

---

## Example

```java
@FunctionalInterface
interface Calculator {

    int calculate(
        int a,
        int b
    );

    default void print() {
        System.out.println("Calculator");
    }

    static void info() {
        System.out.println("Info");
    }
}
```

This is still a functional interface because it has only one abstract method.

---

# 22. Interface Enhancements and Lambda Expressions

Functional interfaces can be implemented using lambda expressions.

Example:

```java
@FunctionalInterface
interface Calculator {

    int add(
        int a,
        int b
    );
}
```

Lambda:

```java
Calculator calculator =
    (a, b) -> a + b;
```

Use:

```java
int result =
    calculator.add(10, 20);

System.out.println(result);
```

Output:

```text
30
```

---

## Important Connection

```text
Java 8
   |
   +── Lambda Expressions
   |
   +── Functional Interfaces
   |
   +── Default Methods
   |
   +── Static Interface Methods
```

These features work together to support functional-style programming while maintaining backward compatibility.

---

# 23. Object Methods in Interfaces

A functional interface cannot simply declare arbitrary abstract methods that duplicate methods from `Object` and expect those to count toward its single abstract method requirement.

Methods such as:

```text
toString()
equals()
hashCode()
```

have special treatment when determining whether an interface is functional.

Example:

```java
@FunctionalInterface
interface Test {

    void run();

    String toString();
}
```

This can still qualify as a functional interface because `toString()` corresponds to a public method from `Object`.

---

# 24. Interface Evolution

One of the biggest reasons for default methods was maintaining compatibility with existing implementations.

Imagine:

```text
Version 1

interface Payment {
    void pay();
}
```

Many classes implement it.

Later:

```text
Version 2

interface Payment {
    void pay();

    default void refund() {
        // default behavior
    }
}
```

Existing implementations do not necessarily need to change immediately.

This allows interfaces in libraries and APIs to evolve more safely.

---

# 25. Common Mistakes

## ❌ Mistake 1 — Calling Static Interface Method Through Object

Wrong:

```java
Calculator calculator =
    new CalculatorImpl();

calculator.add(10, 20);
```

Use:

```java
Calculator.add(10, 20);
```

---

## ❌ Mistake 2 — Assuming Static Interface Methods Are Inherited

Static methods belong to the interface.

They are accessed through:

```text
InterfaceName.method()
```

---

## ❌ Mistake 3 — Forgetting Default Method Conflict

If two interfaces provide the same default method:

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

then:

```java
class Test implements A, B {
}
```

causes a compile-time conflict.

The class must resolve it.

---

## ❌ Mistake 4 — Thinking Interface Default Methods Beat Class Methods

They do not.

A class implementation has priority.

---

## ❌ Mistake 5 — Thinking Default Means Final

A default method can be overridden.

```java
interface A {

    default void show() {
        System.out.println("A");
    }
}
```

```java
class Test implements A {

    @Override
    public void show() {
        System.out.println("Test");
    }
}
```

---

## ❌ Mistake 6 — Thinking Private Interface Methods Are Inherited

They are not.

They are accessible only inside the interface.

---

## ❌ Mistake 7 — Thinking Default Methods Make Interfaces Classes

An interface is still an interface.

Default methods simply allow certain instance methods to have implementations.

---

# 26. Interview Questions

## 🔥 Q1. What changes were introduced in interfaces in Java 8?

Java 8 introduced:

```text
default methods
static methods
```

with implementations.

---

## 🔥 Q2. Why were default methods introduced?

Primarily to allow interfaces to evolve by adding behavior without forcing every existing implementation to immediately provide a new abstract method implementation.

---

## 🔥 Q3. Can an interface have a method body?

Yes.

Modern Java interfaces can contain:

```text
default methods
static methods
private methods
private static methods
```

---

## 🔥 Q4. Can default methods be overridden?

Yes.

An implementing class can override a default method.

---

## 🔥 Q5. Can static interface methods be overridden?

No.

Static methods belong to the interface and are not overridden as instance methods.

---

## 🔥 Q6. How do you call a static interface method?

Using the interface name:

```java
InterfaceName.method();
```

---

## 🔥 Q7. Can an interface have private methods?

Yes.

Private interface methods were introduced in Java 9.

---

## 🔥 Q8. Why were private interface methods introduced?

To allow common implementation logic to be reused internally by default and static methods without exposing helper methods as part of the interface's public API.

---

## 🔥 Q9. What happens when two interfaces have the same default method?

The implementing class gets a default-method conflict and must resolve it.

---

## 🔥 Q10. How do you resolve a default method conflict?

Override the method in the implementing class.

Example:

```java
class Test implements A, B {

    @Override
    public void show() {
        A.super.show();
    }
}
```

---

## 🔥 Q11. What is `InterfaceName.super.method()`?

It explicitly invokes a specific interface's default method from an implementing class.

---

## 🔥 Q12. Which has priority: class method or interface default method?

The class method has priority.

Conceptually:

```text
Class implementation
       ↓
Interface default
```

---

## 🔥 Q13. Can default methods use instance state?

A default method executes in the context of the implementing object, so it can use instance behavior available through `this`.

Example:

```java
interface Vehicle {

    default void show() {
        System.out.println(this);
    }
}
```

---

## 🔥 Q14. Can a static interface method use `this`?

No.

A static method does not have an instance context.

---

## 🔥 Q15. Can an interface have fields?

Yes.

Interface fields are implicitly:

```text
public static final
```

---

## 🔥 Q16. Can interface variables be changed?

No.

They are implicitly final.

---

## 🔥 Q17. Can a functional interface have default methods?

Yes.

A functional interface is defined by having exactly one abstract method, not exactly one total method.

---

## 🔥 Q18. Can a functional interface have static methods?

Yes.

Static methods do not count as abstract methods.

---

## 🔥 Q19. Can a functional interface have private methods?

Yes.

Private methods do not count as abstract methods.

---

## 🔥 Q20. Can an interface have multiple default methods?

Yes.

There is no one-default-method restriction.

---

## 🔥 Q21. What is the difference between default and abstract methods?

```text
Abstract method
→ declaration only
→ implementing class must provide implementation

Default method
→ has implementation
→ implementing class may override it
```

---

## 🔥 Q22. Can an interface extend another interface containing default methods?

Yes.

The child interface inherits the default behavior unless it overrides or otherwise resolves it.

---

## 🔥 Q23. Can an interface extend multiple interfaces?

Yes.

For example:

```java
interface C
    extends A, B {
}
```

If inherited default methods conflict, the conflict must be resolved according to Java's interface rules.

---

## 🔥 Q24. Can a class override a default method with a more restrictive access modifier?

No.

The overriding method cannot reduce visibility.

A public interface default method must remain public when implemented.

---

## 🔥 Q25. What is the biggest practical advantage of default methods?

They allow library and API designers to evolve interfaces while providing default behavior for new methods.

---

# 27. 30-Second Interview Answer

> Java 8 enhanced interfaces by introducing default and static methods. A default method can contain an implementation and can be inherited or overridden by implementing classes. Static interface methods belong to the interface and are called using the interface name. These features were particularly important for interface evolution because new behavior could be added without necessarily breaking every existing implementation. Java 9 further introduced private interface methods, which allow default and static methods to share internal helper logic. If multiple interfaces provide conflicting default methods, the implementing class must resolve the conflict, typically by overriding the method and optionally calling a specific interface implementation using `InterfaceName.super.method()`.

---

# 28. Cheat Sheet

```text
================ INTERFACE ENHANCEMENTS =================


JAVA 8
---------------------------------

default methods
static methods


JAVA 9
---------------------------------

private methods
private static methods


DEFAULT METHOD
---------------------------------

interface A {

    default void show() {
        // body
    }
}


DEFAULT METHOD
→ has implementation
→ inherited by implementing class
→ can be overridden
→ instance method


STATIC METHOD
---------------------------------

interface A {

    static void show() {
        // body
    }
}


STATIC METHOD
→ belongs to interface
→ called using InterfaceName.method()
→ not overridden
→ no this


PRIVATE METHOD
---------------------------------

interface A {

    private void helper() {
        // body
    }
}


PRIVATE METHOD
→ Java 9
→ internal interface helper
→ not accessible outside interface


PRIVATE STATIC
---------------------------------

interface A {

    private static void helper() {
        // body
    }
}


DEFAULT CONFLICT
---------------------------------

interface A {
    default void show() {}
}

interface B {
    default void show() {}
}

class C implements A, B {

    @Override
    public void show() {

        A.super.show();
    }
}


CLASS VS INTERFACE
---------------------------------

Class method
     ↓
wins over
     ↓
Interface default method


DEFAULT VS STATIC
---------------------------------

Default:
→ object.method()

Static:
→ Interface.method()


INTERFACE FIELDS
---------------------------------

public
static
final


FUNCTIONAL INTERFACE
---------------------------------

Exactly ONE abstract method

Can still contain:

default methods
static methods
private methods


LAMBDA
---------------------------------

Functional Interface
        ↓
Lambda Expression


INTERFACE EVOLUTION
---------------------------------

Java 8 default methods
        ↓
Add behavior
        ↓
Reduce compatibility problems
        ↓
Existing implementations can
continue using default behavior


CONFLICT RULE
---------------------------------

Two default methods
        ↓
Conflict
        ↓
Implementing class must resolve


EXPLICIT DEFAULT CALL
---------------------------------

InterfaceName.super.method()


VERSION MEMORY
---------------------------------

Java 8
→ default
→ static

Java 9
→ private
→ private static


========================================================
```

---

# 29. Final Mental Model

```text
                       INTERFACE
                           |
            +--------------+--------------+
            |              |              |
        ABSTRACT        DEFAULT         STATIC
         METHOD         METHOD          METHOD
            |              |              |
       no body          has body        has body
            |              |              |
       implemented      inherited       interface
       by class          by class       level
                           |
                      can override


                           |
                           ↓

                    JAVA 9 ADDITION
                           |
                     PRIVATE METHODS
                           |
                  internal helper logic
                           |
             +-------------+-------------+
             |                           |
        private method            private static
             |                           |
       default methods            static methods
       can use it                 can use it


====================================================

DEFAULT METHOD CONFLICT

        Interface A
             |
        default show()
             |
             +---------+
                       |
                    Class C
                       |
             +---------+
             |
        Interface B
             |
        default show()


                  ↓

              CONFLICT


                  ↓

       Class C must override


                  ↓

       +---------------------+
       |                     |
 A.super.show()       B.super.show()
       |                     |
       ↓                     ↓
   A implementation      B implementation


====================================================

PRIORITY RULE

             Class Method
                  |
                  ↓
               FIRST
                  |
                  ↓
       Interface Default Method
                  |
                  ↓
               SECOND


====================================================

FUNCTIONAL INTERFACE

             Interface
                 |
        +--------+--------+
        |        |        |
     Abstract Default   Static
      Method    Methods  Methods
        |
        ↓
   EXACTLY ONE


                 +
                 |
              Lambda
                 |
                 ↓
       Functional behavior


====================================================

THE CORE IDEA

Before Java 8:

Interface
   ↓
Mostly contract

Java 8:

Interface
   ↓
Contract
+
Default behavior
+
Static utility behavior

Java 9:

Interface
   ↓
Contract
+
Default behavior
+
Static behavior
+
Private internal helpers


====================================================

MEMORY TRICK

DEFAULT
→ object behavior

STATIC
→ interface behavior

PRIVATE
→ interface internal helper

ABSTRACT
→ contract


====================================================
```

---

# 🏁 Final Takeaways

- Java 8 introduced default methods and static methods in interfaces.
- Default methods can contain implementations.
- Default methods are inherited by implementing classes unless overridden or otherwise resolved.
- Default methods were primarily introduced to support interface evolution.
- A class can override a default method.
- If two interfaces provide conflicting default methods, the implementing class must resolve the conflict.
- `InterfaceName.super.method()` explicitly invokes a specific interface's default implementation.
- A class method has priority over an interface default method.
- Interface static methods belong to the interface and are called using the interface name.
- Interface static methods are not overridden as instance methods.
- Java 9 introduced private methods and private static methods in interfaces.
- Private interface methods are useful for sharing internal implementation logic.
- Interface fields are implicitly `public static final`.
- A functional interface has exactly one abstract method.
- Functional interfaces can still contain multiple default, static, and private methods.
- Default, static, and private methods do not count as additional abstract methods.
- Lambda expressions work with functional interfaces.
- The key purpose of default methods is interface evolution and backward compatibility.
- The key purpose of static methods is interface-level utility behavior.
- The key purpose of private methods is internal code reuse within the interface.

---

