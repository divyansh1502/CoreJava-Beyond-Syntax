# ☕ Java OOP — Interface

> **An interface is a contract that defines what a class must do, while the implementing class decides how it does it.**

---

# 1. What is an Interface?

An interface in Java is a reference type used to define a contract that implementing classes must follow.

```java
interface Animal {

    void sound();

}
```

A class implements the interface:

```java
class Dog implements Animal {

    @Override
    public void sound() {

        System.out.println("Dog barks");

    }

}
```

Here:

```text
Animal
   ↓
Interface / Contract
   ↓
Dog
   ↓
Implementation
```

The interface says:

```text
"Every Animal must have sound()."
```

The `Dog` class decides how `sound()` works.

---

# 2. Why Do We Need Interfaces?

Interfaces are mainly used for:

```text
Abstraction
Loose Coupling
Polymorphism
Multiple Inheritance of Type
Standardization
Dependency Injection
Extensibility
```

Suppose we have different payment methods:

```text
Payment
   |
   +-- UPI
   +-- Card
   +-- Cash
   +-- NetBanking
```

Instead of making the application depend directly on every implementation, we can define:

```java
interface Payment {

    void pay();

}
```

Then:

```java
class UPI implements Payment {

    @Override
    public void pay() {

        System.out.println("Payment through UPI");

    }

}
```

```java
class CardPayment implements Payment {

    @Override
    public void pay() {

        System.out.println("Payment through Card");

    }

}
```

Now client code can depend on:

```java
Payment
```

instead of:

```text
UPI
CardPayment
```

This reduces coupling.

---

# 3. Basic Syntax

```java
interface InterfaceName {

    // constants

    // abstract methods

    // default methods

    // static methods

    // private methods

}
```

Implementation:

```java
class ClassName implements InterfaceName {

    // implementation

}
```

Example:

```java
interface Vehicle {

    void start();

}
```

```java
class Car implements Vehicle {

    @Override
    public void start() {

        System.out.println("Car starts");

    }

}
```

---

# 4. `implements` Keyword

A class uses:

```java
implements
```

to implement an interface.

Example:

```java
interface Animal {

    void sound();

}

class Dog implements Animal {

    @Override
    public void sound() {

        System.out.println("Bark");

    }

}
```

Remember:

```text
class → extends → class

class → implements → interface

interface → extends → interface
```

---

# 5. Interface Reference

An interface can be used as a reference type.

```java
Animal a = new Dog();
```

Here:

```text
Reference Type → Animal
Object Type    → Dog
```

Calling:

```java
a.sound();
```

executes:

```java
Dog.sound();
```

This is runtime polymorphism.

---

# 6. Interface and Runtime Polymorphism

Example:

```java
interface Payment {

    void pay();

}
```

```java
class UPI implements Payment {

    @Override
    public void pay() {

        System.out.println("UPI Payment");

    }

}
```

```java
class Card implements Payment {

    @Override
    public void pay() {

        System.out.println("Card Payment");

    }

}
```

Now:

```java
Payment payment;

payment = new UPI();
payment.pay();

payment = new Card();
payment.pay();
```

Output:

```text
UPI Payment
Card Payment
```

The same interface reference can point to different implementations.

---

# 7. Interface Is a Contract

Think of an interface as a contract.

```java
interface Payment {

    void pay();

}
```

The contract says:

```text
Any Payment implementation
must provide pay().
```

For example:

```java
class UPI implements Payment {

    @Override
    public void pay() {

        System.out.println("UPI");

    }

}
```

```java
class Card implements Payment {

    @Override
    public void pay() {

        System.out.println("Card");

    }

}
```

The implementation can differ, but the contract remains the same.

---

# 8. Interface Methods

Modern Java interfaces can contain different kinds of methods:

```text
Abstract methods
Default methods
Static methods
Private methods
```

Example:

```java
interface Test {

    void abstractMethod();

    default void defaultMethod() {

        System.out.println("Default");

    }

    static void staticMethod() {

        System.out.println("Static");

    }

    private void privateMethod() {

        System.out.println("Private");

    }

}
```

---

# 9. Abstract Methods in Interface

An abstract method has no implementation.

```java
interface Animal {

    void sound();

}
```

This is implicitly:

```java
public abstract void sound();
```

So:

```java
void sound();
```

means:

```java
public abstract void sound();
```

for a normal interface method without a body.

---

# 10. Interface Abstract Methods Are Public

Consider:

```java
interface Animal {

    void sound();

}
```

The method is implicitly:

```java
public abstract void sound();
```

Therefore, the implementing class cannot reduce visibility.

Wrong:

```java
class Dog implements Animal {

    @Override
    protected void sound() {

    }

}
```

This gives a compile-time error.

Correct:

```java
class Dog implements Animal {

    @Override
    public void sound() {

    }

}
```

---

# 11. Why Must the Implementation Be Public?

Because the interface method is public.

Think:

```text
Interface
   ↓
public abstract method
   ↓
Implementation cannot reduce visibility
```

So:

```text
public → public       ✅
public → protected    ❌
public → private      ❌
```

---

# 12. Can an Interface Have Variables?

Yes.

But interface fields are implicitly:

```java
public static final
```

Example:

```java
interface Config {

    int MAX = 100;

}
```

This is equivalent to:

```java
interface Config {

    public static final int MAX = 100;

}
```

Therefore:

```java
Config.MAX
```

can be accessed.

---

# 13. Interface Variables Are Constants

Because interface fields are:

```text
public
static
final
```

they cannot be changed.

```java
interface Config {

    int MAX = 100;

}
```

This is invalid:

```java
Config.MAX = 200;
```

because `MAX` is final.

---

# 14. Does an Interface Have Instance Variables?

No.

An interface does not have instance fields.

If you write:

```java
interface Test {

    int x = 10;

}
```

`x` is not an instance variable.

It is:

```java
public static final int x = 10;
```

So it belongs to the interface type, not to each object.

---

# 15. Interface Fields Must Be Initialized

Because interface fields are `final`, they must be initialized.

Valid:

```java
interface Test {

    int VALUE = 10;

}
```

Invalid:

```java
interface Test {

    int VALUE;

}
```

Why?

Because:

```text
VALUE
 ↓
final
 ↓
must have a value
```

---

# 16. Can Interface Fields Be `private`?

No.

Interface fields are implicitly:

```java
public static final
```

Therefore:

```java
private int x = 10;
```

is invalid as an interface field.

---

# 17. Multiple Interfaces

A class can implement multiple interfaces.

Example:

```java
interface A {

    void methodA();

}
```

```java
interface B {

    void methodB();

}
```

A class can implement both:

```java
class Test implements A, B {

    @Override
    public void methodA() {

        System.out.println("A");

    }

    @Override
    public void methodB() {

        System.out.println("B");

    }

}
```

This is one of the major reasons interfaces are important in Java.

---

# 18. Multiple Inheritance Through Interfaces

Java does not allow:

```java
class C extends A, B {
}
```

because multiple class inheritance can create ambiguity.

But Java allows:

```java
class C implements A, B {
}
```

This provides multiple inheritance of type/contract.

Example:

```java
interface Camera {

    void takePhoto();

}
```

```java
interface MusicPlayer {

    void playMusic();

}
```

```java
class Smartphone implements Camera, MusicPlayer {

    @Override
    public void takePhoto() {

        System.out.println("Taking photo");

    }

    @Override
    public void playMusic() {

        System.out.println("Playing music");

    }

}
```

A smartphone has both capabilities.

---

# 19. Interface Extending Interface

An interface can extend another interface.

```java
interface Animal {

    void eat();

}
```

```java
interface Dog extends Animal {

    void bark();

}
```

Now any class implementing `Dog` must implement both:

```java
class Labrador implements Dog {

    @Override
    public void eat() {

        System.out.println("Eating");

    }

    @Override
    public void bark() {

        System.out.println("Barking");

    }

}
```

---

# 20. Can an Interface Extend Multiple Interfaces?

Yes.

This is another important difference from classes.

```java
interface A {

    void methodA();

}
```

```java
interface B {

    void methodB();

}
```

```java
interface C extends A, B {

    void methodC();

}
```

A class implementing `C` must satisfy all inherited contracts.

---

# 21. Class + Multiple Interfaces

A class can extend one class and implement multiple interfaces.

```java
class Child extends Parent implements A, B {

}
```

This is valid.

So:

```text
One superclass
      +
Multiple interfaces
```

is allowed.

---

# 22. Interface Cannot Extend a Class

This is invalid:

```java
interface Test extends SomeClass {

}
```

An interface can extend only interfaces.

Correct:

```java
interface B extends A {

}
```

---

# 23. Class Cannot `implement` a Class

This is invalid:

```java
class Child implements Parent {

}
```

If `Parent` is a class.

Correct:

```java
class Child extends Parent {

}
```

---

# 24. Interface Cannot Be Instantiated

This is invalid:

```java
Animal a = new Animal();
```

because an interface does not represent a concrete implementation.

But:

```java
Animal a = new Dog();
```

is valid.

This is:

```text
Interface reference
        ↓
Concrete object
```

---

# 25. Can an Interface Have a Constructor?

No.

Interfaces cannot have constructors.

Why?

Because constructors initialize objects, and interfaces cannot be instantiated directly.

```text
Interface
   ↓
No direct object
   ↓
No constructor
```

---

# 26. Can an Interface Have a `main()` Method?

Yes.

Since Java allows static methods in interfaces:

```java
interface Test {

    static void main(String[] args) {

        System.out.println("Hello");

    }

}
```

This can be executed as an entry point if the interface is run appropriately.

Important:

```text
main()
↓
static
↓
does not require an object
```

---

# 27. Default Methods

Java 8 introduced `default` methods in interfaces.

Before Java 8, adding a new abstract method to an existing interface could break implementing classes because they would need to implement the new method.

Java 8 introduced:

```java
default
```

Example:

```java
interface Vehicle {

    void start();

    default void stop() {

        System.out.println("Vehicle stopped");

    }

}
```

A class implementing `Vehicle` does not have to override `stop()`.

```java
class Car implements Vehicle {

    @Override
    public void start() {

        System.out.println("Car started");

    }

}
```

Now:

```java
Car c = new Car();

c.start();
c.stop();
```

Output:

```text
Car started
Vehicle stopped
```

---

# 28. Why Were Default Methods Introduced?

The major reason was:

```text
Interface evolution
```

Suppose:

```java
interface Payment {

    void pay();

}
```

Hundreds of classes implement it.

Later Java designers want:

```java
void refund();
```

If `refund()` is abstract:

```java
interface Payment {

    void pay();

    void refund();

}
```

every existing implementation would need to implement `refund()`.

A default method can provide a backward-compatible implementation:

```java
interface Payment {

    void pay();

    default void refund() {

        System.out.println("Refund processing");

    }

}
```

---

# 29. Can Default Methods Be Overridden?

Yes.

```java
interface Animal {

    default void sound() {

        System.out.println("Animal");

    }

}
```

Implementation:

```java
class Dog implements Animal {

    @Override
    public void sound() {

        System.out.println("Dog");

    }

}
```

Now:

```java
Dog d = new Dog();

d.sound();
```

Output:

```text
Dog
```

---

# 30. Interface Static Methods

Interfaces can contain static methods.

```java
interface MathUtil {

    static int square(int x) {

        return x * x;

    }

}
```

Call it using:

```java
MathUtil.square(5);
```

Not:

```java
MathUtil obj = new MathUtil();
obj.square(5);
```

Static methods belong to the interface itself.

---

# 31. Can Interface Static Methods Be Overridden?

No.

Interface static methods are not inherited as instance methods and cannot be overridden.

They are hidden by a separate static method in another type only in contexts where the language permits such declarations; they are not subject to runtime overriding.

The correct way to call an interface static method is:

```java
InterfaceName.method();
```

---

# 32. Private Methods in Interfaces

Java 9 introduced private methods in interfaces.

Example:

```java
interface Payment {

    default void pay() {

        validate();

        System.out.println("Payment");

    }

    private void validate() {

        System.out.println("Validation");

    }

}
```

Private interface methods are useful for sharing implementation logic between default/static methods inside the same interface.

They are not inherited by implementing classes.

---

# 33. Why Were Private Interface Methods Introduced?

Suppose an interface has multiple default methods:

```java
interface Payment {

    default void pay() {

        validate();

    }

    default void refund() {

        validate();

    }

}
```

Both need:

```java
validate();
```

Instead of duplicating the logic, Java 9 allows:

```java
private void validate() {

}
```

This provides code reuse inside the interface.

---

# 34. Interface Methods — Modern Java Summary

```text
Interface methods

├── Abstract
│     └── public abstract
│
├── Default
│     └── public, inherited by implementation
│
├── Static
│     └── belongs to interface
│
└── Private
      └── internal helper
```

Important Java versions:

```text
Java 8 → default + static methods
Java 9 → private methods
```

---

# 35. Interface Method Rules

### Abstract method

```java
void show();
```

implicitly:

```java
public abstract void show();
```

### Default method

```java
default void show() {

}
```

is implicitly public.

### Static method

```java
static void show() {

}
```

is implicitly public.

### Private method

```java
private void helper() {

}
```

is explicitly private.

---

# 36. Default Method Conflict

Suppose:

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
class C implements A, B {

}
```

This creates ambiguity.

Which `show()` should be used?

```text
A.show()
       \
        → C
       /
B.show()
```

Java requires the class to resolve the conflict.

---

# 37. Resolving Default Method Conflict

The class can override the method:

```java
class C implements A, B {

    @Override
    public void show() {

        System.out.println("C");

    }

}
```

Now there is no ambiguity.

---

# 38. Calling a Specific Interface Default Method

Java allows:

```java
InterfaceName.super.method();
```

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

```java
class C implements A, B {

    @Override
    public void show() {

        A.super.show();
        B.super.show();

    }

}
```

Output:

```text
A
B
```

Important:

```java
A.super.show();
```

works for an inherited default interface method.

---

# 39. Class Method Has Priority Over Interface Default Method

Suppose:

```java
class Parent {

    public void show() {

        System.out.println("Parent");

    }

}
```

```java
interface Test {

    default void show() {

        System.out.println("Interface");

    }

}
```

```java
class Child extends Parent implements Test {

}
```

Calling:

```java
Child c = new Child();

c.show();
```

Output:

```text
Parent
```

The class/superclass implementation wins over the interface default method.

Memory rule:

```text
Class wins over interface default
```

---

# 40. Functional Interface

A functional interface is an interface containing exactly one abstract method.

Example:

```java
@FunctionalInterface
interface Calculator {

    int calculate(int a, int b);

}
```

It can contain:

```text
1 abstract method
+
any number of default methods
+
any number of static methods
+
private methods
```

The important restriction is:

```text
Exactly one abstract method
```

---

# 41. `@FunctionalInterface`

Java provides:

```java
@FunctionalInterface
```

Example:

```java
@FunctionalInterface
interface Calculator {

    int add(int a, int b);

}
```

The annotation tells the compiler:

```text
"This interface must remain functional."
```

If someone adds another abstract method:

```java
@FunctionalInterface
interface Calculator {

    int add(int a, int b);

    int subtract(int a, int b);

}
```

the compiler reports an error.

---

# 42. Lambda and Functional Interface

Functional interfaces are used with lambda expressions.

```java
@FunctionalInterface
interface Calculator {

    int calculate(int a, int b);

}
```

Lambda:

```java
Calculator c = (a, b) -> a + b;
```

Then:

```java
System.out.println(c.calculate(10, 20));
```

Output:

```text
30
```

This becomes important when learning:

```text
Java 8
Lambda Expressions
Functional Interfaces
Streams
```

---

# 43. Marker Interface

A marker interface contains no abstract methods.

Example from Java:

```java
Serializable
```

It acts as metadata/tagging.

Conceptually:

```text
Class
  ↓
implements Serializable
  ↓
Class gets a specific capability/meaning recognized by Java APIs
```

Other historical examples include:

```text
Cloneable
```

Marker interfaces have no methods to implement.

---

# 44. Interface vs Abstract Class

| Feature              | Interface             | Abstract Class               |
| -------------------- | --------------------- | ---------------------------- |
| Keyword              | `interface`           | `abstract class`             |
| Class relationship   | `implements`          | `extends`                    |
| Multiple inheritance | Multiple interfaces   | One class                    |
| Instance variables   | No                    | Yes                          |
| Constructors         | No                    | Yes                          |
| Instance state       | No                    | Yes                          |
| Abstract methods     | Yes                   | Yes                          |
| Concrete methods     | Yes, modern Java      | Yes                          |
| Static methods       | Yes                   | Yes                          |
| Default methods      | Yes                   | No `default` concept         |
| Private methods      | Yes, Java 9+          | Yes                          |
| Fields               | `public static final` | Any valid field              |
| Main purpose         | Contract/capability   | Shared base/state + behavior |

---

# 45. Interface vs Abstract Class — Core Idea

Use an interface when you mainly want to define:

```text
WHAT an object can do
```

Example:

```text
Flyable
Payable
Serializable
Comparable
Runnable
```

Use an abstract class when you need a common:

```text
state
+
behavior
+
base implementation
```

Example:

```text
Animal
Employee
Vehicle
```

Memory trick:

```text
Interface
    ↓
Capability / Contract

Abstract Class
    ↓
Common Base / Shared State
```

---

# 46. Can an Interface Have Concrete Methods?

Yes.

This is an important modern Java interview question.

Before Java 8, ordinary interface methods were abstract.

Since Java 8, interfaces can have:

```text
default methods
static methods
```

Java 9 additionally introduced:

```text
private methods
```

Therefore:

```text
Interface ≠ "only abstract methods"
```

That statement is outdated for modern Java.

---

# 47. Can an Interface Have a `final` Method?

Interface methods cannot be declared `final` in the usual interface method declarations.

Why?

Because:

```text
final method
     ↓
cannot be overridden
```

while interface instance behavior is designed around implementation/overriding of its applicable methods.

However, interface fields are implicitly final.

---

# 48. Can an Interface Be `final`?

No.

```java
final interface Test {

}
```

is invalid.

Why?

Interfaces are meant to be implemented or extended.

A `final` interface would contradict that purpose.

---

# 49. Can an Interface Be Abstract?

Interfaces are inherently abstract in the sense that they define a type contract rather than a directly instantiable class.

You do not write:

```java
abstract interface Test
```

as a meaningful distinction for normal interface design.

The `interface` declaration already provides the relevant semantics.

---

# 50. Can One Interface Be Implemented by Multiple Classes?

Absolutely.

```java
interface Payment {

    void pay();

}
```

Multiple classes:

```java
class UPI implements Payment {

    public void pay() {

        System.out.println("UPI");

    }

}
```

```java
class Card implements Payment {

    public void pay() {

        System.out.println("Card");

    }

}
```

This is one of the biggest benefits of interfaces.

---

# 51. Can Multiple Classes Implement the Same Interface?

Yes.

```text
Payment
   |
   +── UPI
   +── Card
   +── Cash
   +── NetBanking
```

Each class provides its own implementation.

---

# 52. Can an Interface Extend Multiple Interfaces?

Yes.

```java
interface A {

}

interface B {

}

interface C extends A, B {

}
```

This is valid.

---

# 53. Can a Class Implement Multiple Interfaces?

Yes.

```java
class Smartphone implements Camera, GPS, MusicPlayer {

}
```

This allows a class to have multiple capabilities.

---

# 54. Interface and Loose Coupling

Consider:

```java
class OrderService {

    private UPI payment;

}
```

Now `OrderService` is tightly coupled to UPI.

Better:

```java
class OrderService {

    private Payment payment;

}
```

where:

```java
interface Payment {

    void pay();

}
```

Now:

```text
OrderService
     ↓
 Payment interface
     ↑
 ┌───┴────┐
UPI      Card
```

The implementation can be changed without changing the high-level code.

This is a major real-world use of interfaces.

---

# 55. Interface and Dependency Injection

In backend development, interfaces are commonly used for dependency injection.

Example:

```java
interface PaymentService {

    void pay();

}
```

Implementation:

```java
class StripePaymentService implements PaymentService {

    @Override
    public void pay() {

        System.out.println("Stripe");

    }

}
```

Another implementation:

```java
class RazorpayPaymentService implements PaymentService {

    @Override
    public void pay() {

        System.out.println("Razorpay");

    }

}
```

A service can depend on:

```java
PaymentService
```

rather than a concrete implementation.

This makes changing implementations easier.

---

# 56. Interface and Abstraction

Interface provides abstraction by exposing the required behavior while hiding implementation details.

Example:

```java
Payment payment = new UPI();

payment.pay();
```

The caller knows:

```text
pay()
```

but does not need to know the internal UPI payment process.

```text
Caller
  ↓
Payment interface
  ↓
UPI implementation
```

---

# 57. Interface and Polymorphism

Interface references can refer to different implementations.

```java
Payment p;

p = new UPI();
p.pay();

p = new Card();
p.pay();
```

This demonstrates:

```text
One interface
      ↓
Multiple implementations
      ↓
Runtime polymorphism
```

---

# 58. Interface and Upcasting

This is interface upcasting:

```java
Payment p = new UPI();
```

Because:

```text
UPI IS-A Payment
```

Therefore:

```text
UPI object
    ↓
Payment reference
```

This is safe and automatic.

---

# 59. Interface Downcasting

Suppose:

```java
Payment p = new UPI();
```

You can cast:

```java
UPI u = (UPI) p;
```

This is valid because the actual object is a `UPI`.

But:

```java
Payment p = new Card();

UPI u = (UPI) p;
```

causes:

```text
ClassCastException
```

because the actual object is `Card`.

---

# 60. `instanceof` with Interfaces

You can check whether an object implements an interface.

```java
if (obj instanceof Payment) {

    System.out.println("Payment implementation");

}
```

This is useful when working with polymorphic objects.

---

# 61. Nested Interfaces

Interfaces can be declared inside classes or interfaces.

Example:

```java
class Outer {

    interface Inner {

        void show();

    }

}
```

Usage:

```java
class Test implements Outer.Inner {

    @Override
    public void show() {

        System.out.println("Hello");

    }

}
```

This is called a nested interface.

---

# 62. Interface Can Contain Nested Types

An interface can contain nested types such as:

```text
Classes
Interfaces
Enums
Records
```

These nested types have interface-specific accessibility rules.

This is an advanced topic and less commonly asked for beginner interviews.

---

# 63. Interface Inheritance

Interface inheritance:

```java
interface A {

    void a();

}

interface B extends A {

    void b();

}
```

Now:

```text
B
↓
inherits contract of A
```

A class:

```java
class C implements B {

    public void a() {

    }

    public void b() {

    }

}
```

must implement both.

---

# 64. Important Difference: `extends` vs `implements`

Remember:

```text
Class → Class
    extends

Class → Interface
    implements

Interface → Interface
    extends
```

Examples:

```java
class Dog extends Animal
```

```java
class Dog implements Animal
```

where `Animal` is an interface.

```java
interface Dog extends Animal
```

where both are interfaces.

---

# 65. Interface Object — Important Interview Trap

This:

```java
Payment p = new Payment();
```

is invalid.

But this:

```java
Payment p = new UPI();
```

is valid.

Why?

Because:

```text
Interface cannot be directly instantiated
Concrete implementing class can be instantiated
```

---

# 66. Interface Fields — Interview Trap

```java
interface Test {

    int x = 10;

}
```

You might think:

```text
instance variable
```

But actually:

```java
public static final int x = 10;
```

Therefore:

```java
Test.x
```

is the conceptual access form.

---

# 67. Interface Method — Interview Trap

```java
interface Test {

    void show();

}
```

You might think:

```text
default access
```

But it is actually:

```java
public abstract void show();
```

---

# 68. Interface Implementation — Interview Trap

This is wrong:

```java
interface Test {

    void show();

}

class Demo implements Test {

    void show() {

    }

}
```

Because `show()` is public in the interface.

Correct:

```java
class Demo implements Test {

    @Override
    public void show() {

    }

}
```

---

# 69. Interface Default Method Conflict — Interview Trap

```java
interface A {

    default void show() {

        System.out.println("A");

    }

}

interface B {

    default void show() {

        System.out.println("B");

    }

}
```

This is invalid without resolution:

```java
class C implements A, B {

}
```

The class must override:

```java
@Override
public void show() {

}
```

---

# 70. Class vs Interface Default Method Trap

```java
class Parent {

    public void show() {

        System.out.println("Parent");

    }

}

interface Test {

    default void show() {

        System.out.println("Interface");

    }

}

class Child extends Parent implements Test {

}
```

Calling:

```java
new Child().show();
```

prints:

```text
Parent
```

Memory:

```text
Class implementation
      ↓
wins over
      ↓
Interface default
```

---

# 71. Interface Static Method Trap

Suppose:

```java
interface Test {

    static void show() {

        System.out.println("Test");

    }

}
```

Call:

```java
Test.show();
```

Do not expect:

```java
new Test().show();
```

because an interface cannot be instantiated and the static method belongs to the interface itself.

---

# 72. Interface vs Concrete Class

A concrete class provides an implementation.

An interface primarily defines a contract/capability.

```text
Interface
   ↓
WHAT

Class
   ↓
HOW
```

This is a useful conceptual model, although modern interfaces can also contain some implementation through default/static/private methods.

---

# 73. Interface and Multiple Inheritance Problem

Java avoids multiple inheritance of classes:

```java
class C extends A, B
```

because if both `A` and `B` provide the same method, ambiguity can occur.

With interfaces, Java resolves default-method conflicts explicitly.

```text
A.show()
     \
      C
     /
B.show()
```

If both provide default `show()`, `C` must resolve the conflict.

---

# 74. Interface and `Object` Methods

Methods such as:

```text
toString()
equals()
hashCode()
```

come from `Object`.

An interface does not inherit from `Object` in the same way a class does.

However, implementing classes inherit `Object` methods through their class hierarchy and may override them.

This is an important conceptual distinction.

---

# 75. Real-World Example — QRder

For a project like QRder, you could define:

```java
interface Payment {

    void pay(double amount);

}
```

Different implementations:

```java
class UPI implements Payment {

    @Override
    public void pay(double amount) {

        System.out.println("Paid ₹" + amount + " using UPI");

    }

}
```

```java
class CardPayment implements Payment {

    @Override
    public void pay(double amount) {

        System.out.println("Paid ₹" + amount + " using Card");

    }

}
```

Then:

```java
Payment payment = new UPI();

payment.pay(499);
```

Later you can change:

```java
Payment payment = new CardPayment();
```

without changing the code that works with the `Payment` contract.

---

# 76. Real-World Backend Example

Suppose a Spring Boot application has:

```text
PaymentService
      |
      +── RazorpayPaymentService
      +── StripePaymentService
      +── MockPaymentService
```

Interface:

```java
public interface PaymentService {

    void processPayment(double amount);

}
```

Implementation:

```java
public class RazorpayPaymentService
        implements PaymentService {

    @Override
    public void processPayment(double amount) {

        System.out.println("Processing Razorpay payment");

    }

}
```

The rest of the application can depend on:

```java
PaymentService
```

rather than:

```java
RazorpayPaymentService
```

This is a practical example of programming to an abstraction.

---

# 77. Advantages of Interfaces

### 1. Abstraction

Hide implementation details.

### 2. Loose Coupling

Classes depend on contracts rather than concrete implementations.

### 3. Runtime Polymorphism

One interface reference can refer to multiple implementations.

### 4. Multiple Inheritance of Type

A class can implement multiple interfaces.

### 5. Flexibility

Implementations can be replaced.

### 6. Testability

Interfaces make it easier to substitute implementations such as mocks/fakes in testing.

### 7. Extensibility

New implementations can be added without changing code that depends only on the interface.

---

# 78. Limitations of Interfaces

Interfaces are not always the right choice.

They cannot directly provide per-object instance state through instance fields.

They also cannot have constructors.

If several classes need substantial shared state and implementation, an abstract class may be more appropriate.

Modern interfaces can contain implementation, but they are still fundamentally designed around contracts/capabilities rather than shared object state.

---

# 79. Common Interview Traps

```text
1. Interface cannot be instantiated directly.

2. Interface fields are public static final.

3. Interface abstract methods are public abstract.

4. Implementation cannot reduce method visibility.

5. A class can implement multiple interfaces.

6. An interface can extend multiple interfaces.

7. An interface cannot extend a class.

8. A class uses implements for an interface.

9. An interface uses extends for another interface.

10. Static interface methods are not overridden.

11. Default methods can be overridden.

12. Private interface methods cannot be accessed by implementing classes.

13. Constructors cannot exist in interfaces.

14. Interface fields are not instance variables.

15. Two conflicting default methods must be resolved by the implementing class.

16. A superclass method takes priority over an interface default method.

17. Functional interface means exactly one abstract method, not exactly one method total.

18. `@FunctionalInterface` is a compiler-checking annotation.

19. Interface references can point to implementing objects.

20. Downcasting must match the actual object type.
```

---

# 80. Interface — Important Interview Questions & Answers

## Q1. What is an interface in Java?

**Answer:**

An interface is a reference type that defines a contract for classes. A class implements the interface and provides implementations for its required abstract methods. Interfaces support abstraction, polymorphism, loose coupling, and multiple inheritance of type.

---

## Q2. Why do we use interfaces?

**Answer:**

Interfaces are used to define contracts, achieve abstraction, reduce coupling, support runtime polymorphism, allow multiple interface inheritance, and make implementations replaceable.

---

## Q3. Can we create an object of an interface?

**Answer:**

No, an interface cannot be instantiated directly.

But an interface reference can refer to an implementing object:

```java
Payment p = new UPI();
```

---

## Q4. Can an interface have variables?

**Answer:**

Yes. Interface fields are implicitly:

```java
public static final
```

Therefore, they are constants, not instance variables.

---

## Q5. Can an interface have instance variables?

**Answer:**

No. Interface fields are implicitly static and final.

---

## Q6. Can an interface have constructors?

**Answer:**

No. Interfaces cannot have constructors because they cannot be instantiated directly.

---

## Q7. Can an interface contain method implementations?

**Answer:**

Yes. Modern Java interfaces can contain:

```text
default methods
static methods
private methods
```

Java 8 introduced default and static methods, while Java 9 introduced private interface methods.

---

## Q8. What is a default method?

**Answer:**

A default method is a method with an implementation inside an interface.

```java
default void show() {

    System.out.println("Hello");

}
```

It was introduced in Java 8 mainly to allow interfaces to evolve without forcing every existing implementation to immediately implement newly added methods.

---

## Q9. Can default methods be overridden?

**Answer:**

Yes.

```java
interface A {

    default void show() {

        System.out.println("A");

    }

}

class B implements A {

    @Override
    public void show() {

        System.out.println("B");

    }

}
```

---

## Q10. Can static methods of interfaces be overridden?

**Answer:**

No. Static interface methods are not overridden because overriding is based on instance-method dispatch.

They are accessed through the interface:

```java
InterfaceName.method();
```

---

## Q11. Can an interface have private methods?

**Answer:**

Yes. Java 9 introduced private methods in interfaces.

They are mainly used as helper methods for default and static methods inside the same interface.

---

## Q12. Can a class implement multiple interfaces?

**Answer:**

Yes.

```java
class Smartphone implements Camera, GPS, MusicPlayer {

}
```

This provides multiple inheritance of type/capabilities.

---

## Q13. Can an interface extend multiple interfaces?

**Answer:**

Yes.

```java
interface C extends A, B {

}
```

---

## Q14. Can an interface extend a class?

**Answer:**

No.

An interface can extend only interfaces.

---

## Q15. Can a class extend an interface?

**Answer:**

No.

A class uses:

```java
implements
```

for interfaces.

---

## Q16. What is a functional interface?

**Answer:**

A functional interface has exactly one abstract method.

It may still contain multiple default, static, and private methods.

Example:

```java
@FunctionalInterface
interface Calculator {

    int calculate(int a, int b);

}
```

Functional interfaces can be used with lambda expressions.

---

## Q17. What is a marker interface?

**Answer:**

A marker interface is an interface with no abstract methods. It provides metadata or marks a class for special treatment by Java APIs or frameworks.

Examples include:

```text
Serializable
Cloneable
```

---

## Q18. Why must implemented interface methods be public?

**Answer:**

Because ordinary abstract interface methods are implicitly public. An implementing method cannot reduce the visibility of the inherited contract.

Therefore:

```text
public → public       ✅
public → protected    ❌
public → private      ❌
```

---

## Q19. What happens if two interfaces have the same default method?

**Answer:**

The implementing class must resolve the conflict by overriding the method.

```java
class C implements A, B {

    @Override
    public void show() {

        System.out.println("Resolved");

    }

}
```

---

## Q20. What happens if a superclass and interface both provide the same method?

**Answer:**

The superclass implementation has priority over the interface default method.

```text
Superclass
    ↓
wins
    ↓
Interface default
```

---

## Q21. What is the difference between interface and abstract class?

**Answer:**

An interface primarily defines a contract/capability, while an abstract class can provide shared state, constructors, and common implementation.

A class can extend only one class but can implement multiple interfaces.

---

## Q22. Why does Java allow multiple interfaces but not multiple classes?

**Answer:**

Java avoids multiple inheritance of classes because it can create ambiguity in inherited state and behavior.

Interfaces provide a safer model for multiple contracts, and Java has explicit rules for resolving default-method conflicts.

---

## Q23. Can an interface contain `main()`?

**Answer:**

Yes. Since interfaces can contain static methods, an interface can declare a static `main()` method.

```java
interface Test {

    static void main(String[] args) {

        System.out.println("Hello");

    }

}
```

---

## Q24. Can an interface be final?

**Answer:**

No. An interface is designed to be implemented or extended, so declaring an interface as `final` is invalid.

---

## Q25. Are interface variables mutable?

**Answer:**

No. Interface fields are implicitly `public static final`, so they cannot be reassigned.

---

# 81. Interview Trap Questions

### Trap 1

```java
interface A {

    int x = 10;

}
```

What is `x`?

```text
public static final
```

Not an instance variable.

---

### Trap 2

```java
interface A {

    void show();

}
```

What is the actual declaration?

```java
public abstract void show();
```

---

### Trap 3

```java
class B implements A {

    protected void show() {

    }

}
```

Valid?

```text
NO
```

Because visibility is reduced.

---

### Trap 4

```java
interface A {

    default void show() {

    }

}
```

Can class B override it?

```text
YES
```

---

### Trap 5

```java
interface A {

    static void show() {

    }

}
```

Can B override it?

```text
NO
```

---

### Trap 6

```java
interface A {

    void show();

}

A obj = new A();
```

Valid?

```text
NO
```

---

### Trap 7

```java
interface A {

}

interface B {

}

interface C extends A, B {

}
```

Valid?

```text
YES
```

---

### Trap 8

```java
class C implements A, B {

}
```

Valid?

```text
YES
```

---

### Trap 9

```java
interface A {

    default void show() {

        System.out.println("A");

    }

}

interface B {

    default void show() {

        System.out.println("B");

    }

}

class C implements A, B {

}
```

Valid?

```text
NO
```

The conflict must be resolved.

---

### Trap 10

```java
class Parent {

    public void show() {

        System.out.println("Parent");

    }

}

interface A {

    default void show() {

        System.out.println("A");

    }

}

class Child extends Parent implements A {

}
```

Output:

```text
Parent
```

Superclass implementation wins.

---

# 82. Interface — Quick Revision

```text
INTERFACE
   ↓
Contract / Capability
   ↓
Implemented using implements
   ↓
Cannot be instantiated directly
   ↓
No constructors
   ↓
No instance variables
   ↓
Fields → public static final
   ↓
Abstract methods → public abstract
   ↓
Java 8 → default + static methods
   ↓
Java 9 → private methods
   ↓
Multiple interfaces allowed
   ↓
Runtime polymorphism
   ↓
Loose coupling
```

---

# 83. Interface Syntax Cheat Sheet

```java
interface Payment {

    // public abstract
    void pay();

    // public static final
    int MAX_AMOUNT = 100000;

    // Java 8
    default void receipt() {

        System.out.println("Receipt");

    }

    // Java 8
    static void info() {

        System.out.println("Payment");

    }

    // Java 9
    private void validate() {

        System.out.println("Validation");

    }

}
```

Implementation:

```java
class UPI implements Payment {

    @Override
    public void pay() {

        System.out.println("UPI payment");

    }

}
```

---

# 84. 30-Second Interview Answer

> **An interface in Java is a contract that defines a set of capabilities that implementing classes must provide. A class implements an interface using the `implements` keyword, and a class can implement multiple interfaces. Interface fields are implicitly `public static final`, while ordinary abstract methods are implicitly `public abstract`. Since Java 8, interfaces can also contain default and static methods, and since Java 9 they can contain private methods. Interfaces are heavily used for abstraction, loose coupling, runtime polymorphism, and dependency injection.**

---

# 🔥 TOP 10 MOST IMPORTANT INTERVIEW QUESTIONS

## 1. What is an interface and why is it used?

**Answer:**

An interface defines a contract that implementing classes must follow. It is used for abstraction, loose coupling, runtime polymorphism, multiple inheritance of type, and flexible implementations.

---

## 2. Can an interface have variables?

**Answer:**

Yes. Every interface field is implicitly:

```java
public static final
```

Therefore, interface variables are constants, not instance variables.

---

## 3. Can an interface have concrete methods?

**Answer:**

Yes.

Modern Java interfaces can have:

```text
default methods
static methods
private methods
```

Java 8 introduced default/static methods and Java 9 introduced private methods.

---

## 4. Can a class implement multiple interfaces?

**Answer:**

Yes.

```java
class Smartphone implements Camera, GPS, MusicPlayer {

}
```

This is one way Java supports multiple inheritance of type.

---

## 5. Can an interface extend multiple interfaces?

**Answer:**

Yes.

```java
interface C extends A, B {

}
```

---

## 6. What is the difference between an interface and an abstract class?

**Answer:**

An interface primarily defines a contract/capability, while an abstract class can provide shared state, constructors, and common implementation.

A class can extend only one class but can implement multiple interfaces.

---

## 7. What is a default method and why was it introduced?

**Answer:**

A default method is an interface method with an implementation.

It was introduced in Java 8 mainly to allow interfaces to evolve by adding behavior without requiring every existing implementation to immediately provide a new implementation.

---

## 8. What happens when two interfaces have the same default method?

**Answer:**

The implementing class must resolve the conflict by overriding the method.

```java
class C implements A, B {

    @Override
    public void show() {

        System.out.println("Resolved");

    }

}
```

---

## 9. Can static methods of an interface be overridden?

**Answer:**

No.

Static interface methods are not subject to runtime instance-method overriding.

They are accessed using:

```java
InterfaceName.method();
```

---

## 10. Why are interfaces important in real-world backend development?

**Answer:**

Interfaces allow application code to depend on abstractions rather than concrete implementations.

For example:

```text
PaymentService
      |
      +── RazorpayPaymentService
      +── StripePaymentService
      +── MockPaymentService
```

The application can depend on:

```java
PaymentService
```

instead of a specific implementation.

This improves:

```text
Loose Coupling
Testability
Maintainability
Extensibility
Dependency Injection
```

---

# ⭐ FINAL MEMORY TRICK

```text
INTERFACE

WHAT
 ↓
Contract
 ↓
implements
 ↓
Multiple interfaces
 ↓
Abstraction
 ↓
Polymorphism
 ↓
Loose Coupling
```

Remember:

```text
class extends class

class implements interface

interface extends interface
```

And:

```text
Interface fields
→ public static final

Interface abstract methods
→ public abstract

Java 8
→ default + static

Java 9
→ private methods

Multiple interfaces
→ YES

Interface object directly
→ NO

Interface constructor
→ NO

Interface instance variables
→ NO
```

> **Best mental model:**
>
> **An interface tells a class WHAT it must be capable of doing; the implementing class decides HOW that capability is performed.**
