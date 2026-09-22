# ☕ Java OOP — Method Overriding

> **Method Overriding occurs when a child class provides its own implementation of an inherited instance method from its parent class with the same method signature.**

---

# 📚 Table of Contents

- [1. What is Method Overriding?](#1-what-is-method-overriding)
- [2. Why Do We Need Method Overriding?](#2-why-do-we-need-method-overriding)
- [3. Basic Example](#3-basic-example)
- [4. The `@Override` Annotation](#4-the-override-annotation)
- [5. Why Should We Use `@Override`?](#5-why-should-we-use-override)
- [6. Rules of Method Overriding](#6-rules-of-method-overriding)
- [7. Same Parameter List](#7-same-parameter-list)
- [8. Different Parameters = Overloading](#8-different-parameters--overloading)
- [9. Overriding vs Overloading](#9-overriding-vs-overloading)
- [10. Runtime Polymorphism](#10-runtime-polymorphism)
- [11. Dynamic Method Dispatch](#11-dynamic-method-dispatch)
- [12. Reference Type vs Object Type](#12-reference-type-vs-object-type)
- [13. Upcasting and Overriding](#13-upcasting-and-overriding)
- [14. Downcasting](#14-downcasting)
- [15. Covariant Return Type](#15-covariant-return-type)
- [16. What is NOT Covariant?](#16-what-is-not-covariant)
- [17. Access Modifier Rules](#17-access-modifier-rules)
- [18. Package-Private / Default Access](#18-package-private--default-access)
- [19. `final` Methods](#19-final-methods)
- [20. `static` Methods](#20-static-methods)
- [21. Private Methods](#21-private-methods)
- [22. Why Can't Private Methods Be Overridden?](#22-why-cant-private-methods-be-overridden)
- [23. Constructors Cannot Be Overridden](#23-constructors-cannot-be-overridden)
- [24. `super` and Method Overriding](#24-super-and-method-overriding)
- [25. `super` vs `this`](#25-super-vs-this)
- [26. Abstract Methods and Overriding](#26-abstract-methods-and-overriding)
- [27. Interface Methods and Overriding](#27-interface-methods-and-overriding)
- [28. Multiple Levels of Overriding](#28-multiple-levels-of-overriding)
- [29. Calling Parent Implementation from Multiple Levels](#29-calling-parent-implementation-from-multiple-levels)
- [30. Exceptions and Method Overriding](#30-exceptions-and-method-overriding)
- [31. Runtime Exceptions](#31-runtime-exceptions)
- [32. Can We Override a Method with a Different Parameter Type?](#32-can-we-override-a-method-with-a-different-parameter-type)
- [33. Can We Override a Method with a Different Return Type?](#33-can-we-override-a-method-with-a-different-return-type)
- [34. Can We Override a `public` Method with `protected`?](#34-can-we-override-a-public-method-with-protected)
- [35. Can We Override a `protected` Method with `public`?](#35-can-we-override-a-protected-method-with-public)
- [36. Can We Override a Default Interface Method?](#36-can-we-override-a-default-interface-method)
- [37. Can We Override a `default` Method with `public`?](#37-can-we-override-a-default-method-with-public)
- [38. Fields vs Overridden Methods](#38-fields-vs-overridden-methods)
- [39. `static` vs Instance Method](#39-static-vs-instance-method)
- [40. Method Overriding and `final`](#40-method-overriding-and-final)
- [41. Overriding and Object Class](#41-overriding-and-object-class)
- [42. Why `equals()` and `hashCode()` Matter](#42-why-equals-and-hashcode-matter)
- [43. Real-World Example — Payment System](#43-real-world-example--payment-system)
- [44. Real-World Backend Example](#44-real-world-backend-example)
- [45. Advantages of Method Overriding](#45-advantages-of-method-overriding)
- [46. Common Interview Traps](#46-common-interview-traps)
- [47. Method Overriding vs Method Hiding](#47-method-overriding-vs-method-hiding)
- [48. Method Overriding vs Method Overloading](#48-method-overriding-vs-method-overloading)
- [49. Complete Mental Model](#49-complete-mental-model)
- [50. 30-Second Interview Answer](#50-30-second-interview-answer)
- [⭐ Final Memory Trick](#-final-memory-trick)
- [🔥 Top 10 Interview Questions & Answers](#-top-10-interview-questions--answers)
- [🧠 10-Question Quick Revision](#-10-question-quick-revision)
- [🎯 30-Second Interview Answer](#-30-second-interview-answer)


---

# 1. What is Method Overriding?

Suppose a parent class defines:

```java
class Animal {

    void sound() {
        System.out.println("Animal makes a sound");
    }
}
```

A child class can provide a more specific implementation:

```java
class Dog extends Animal {

    @Override
    void sound() {
        System.out.println("Dog barks");
    }
}
```

Here:

```text
Animal → sound()
Dog    → sound()
```

The `Dog` version **overrides** the inherited `sound()` method.

---

# 2. Why Do We Need Method Overriding?

Inheritance allows a child to reuse parent behavior.

But sometimes the child needs behavior that is **specific to itself**.

For example:

```text
Animal
 ├── Dog
 ├── Cat
 └── Cow
```

All animals have:

```text
sound()
```

But their sounds are different:

```text
Dog → Bark
Cat → Meow
Cow → Moo
```

Instead of changing the parent method, each child can override it.

```java
class Animal {

    void sound() {
        System.out.println("Animal sound");
    }
}

class Dog extends Animal {

    @Override
    void sound() {
        System.out.println("Bark");
    }
}

class Cat extends Animal {

    @Override
    void sound() {
        System.out.println("Meow");
    }
}
```

This allows each subclass to provide its own behavior.

---

# 3. Basic Example

```java
class Parent {

    void show() {
        System.out.println("Parent");
    }
}

class Child extends Parent {

    @Override
    void show() {
        System.out.println("Child");
    }
}
```

Now:

```java
Child c = new Child();

c.show();
```

Output:

```text
Child
```

The child implementation is used instead of the inherited parent implementation.

---

# 4. The `@Override` Annotation

Java provides:

```java
@Override
```

to indicate that a method is intended to override a parent method.

Example:

```java
class Dog extends Animal {

    @Override
    void sound() {
        System.out.println("Bark");
    }
}
```

`@Override` is not required for overriding.

This also works:

```java
class Dog extends Animal {

    void sound() {
        System.out.println("Bark");
    }
}
```

However, using `@Override` is strongly recommended because the compiler can catch mistakes.

---

# 5. Why Should We Use `@Override`?

Consider:

```java
class Animal {

    void sound() {
        System.out.println("Animal");
    }
}

class Dog extends Animal {

    @Override
    void Sound() {
        System.out.println("Dog");
    }
}
```

Notice:

```text
sound()
Sound()
```

Java is case-sensitive.

Without `@Override`, you might accidentally create a new method.

With:

```java
@Override
```

the compiler reports an error because `Sound()` does not override anything.

So:

```text
@Override
    ↓
Compiler verification
    ↓
Prevents accidental mistakes
```

---

# 6. Rules of Method Overriding

For normal instance-method overriding:

### Rule 1 — Inheritance is required

The child must inherit from the parent.

```java
class Child extends Parent
```

---

### Rule 2 — Same method name

Parent:

```java
void show()
```

Child:

```java
void show()
```

---

### Rule 3 — Same parameter list

Parent:

```java
void show(int x)
```

Child must have:

```java
void show(int x)
```

Not:

```java
void show(double x)
```

That would be overloading, not overriding.

---

### Rule 4 — Return type must be compatible

The child may return:

* the same type, or
* a subtype of the parent's return type.

This is called a **covariant return type**.

---

### Rule 5 — Access cannot be reduced

If the parent method is:

```java
public
```

the child cannot make it:

```java
protected
```

or:

```java
private
```

The child must maintain or increase accessibility.

---

### Rule 6 — `final` methods cannot be overridden

```java
final void show() {
}
```

cannot be overridden.

---

### Rule 7 — `static` methods are not overridden

Static methods are **hidden**, not overridden.

---

### Rule 8 — `private` methods are not overridden

Private methods are not inherited in the normal sense required for overriding.

---

# 7. Same Parameter List

This is overriding:

```java
class Parent {

    void show(int x) {
        System.out.println("Parent");
    }
}

class Child extends Parent {

    @Override
    void show(int x) {
        System.out.println("Child");
    }
}
```

The signatures match:

```text
show(int)
show(int)
```

---

# 8. Different Parameters = Overloading

This is NOT overriding:

```java
class Parent {

    void show(int x) {
    }
}

class Child extends Parent {

    void show(double x) {
    }
}
```

These are:

```text
show(int)
show(double)
```

So this is:

```text
Method Overloading
```

not overriding.

---

# 9. Overriding vs Overloading

| Feature      | Overriding      | Overloading        |
| ------------ | --------------- | ------------------ |
| Classes      | Parent + Child  | Usually same class |
| Method name  | Same            | Same               |
| Parameters   | Same            | Different          |
| Inheritance  | Required        | Not required       |
| Binding      | Runtime         | Compile-time       |
| Polymorphism | Runtime         | Compile-time       |
| Return type  | Same/covariant  | Cannot distinguish |
| `static`     | Not overridden  | Can be overloaded  |
| `final`      | Cannot override | Can overload       |
| `private`    | Cannot override | Can overload       |

---

# 10. Runtime Polymorphism

Method overriding is the primary mechanism behind runtime polymorphism in Java.

Example:

```java
class Animal {

    void sound() {
        System.out.println("Animal");
    }
}

class Dog extends Animal {

    @Override
    void sound() {
        System.out.println("Dog");
    }
}
```

Now:

```java
Animal a = new Dog();

a.sound();
```

Output:

```text
Dog
```

Notice:

```text
Reference type → Animal
Object type    → Dog
```

The compiler knows that `Animal` has a `sound()` method.

At runtime, the JVM sees that the actual object is a `Dog`.

Therefore:

```text
Dog.sound()
```

executes.

---

# 11. Dynamic Method Dispatch

This runtime selection of an overridden method is called:

> **Dynamic Method Dispatch**

Example:

```java
Animal animal;

animal = new Dog();
animal.sound();

animal = new Cat();
animal.sound();
```

The same reference:

```java
animal
```

can refer to different objects.

The method implementation changes according to the actual object.

```text
animal.sound()
      ↓
Runtime checks actual object
      ↓
Dog → Dog.sound()
Cat → Cat.sound()
```

---

# 12. Reference Type vs Object Type

This is one of the most important interview concepts.

```java
Animal a = new Dog();
```

Here:

```text
Reference Type → Animal
Object Type    → Dog
```

The reference type controls what the compiler allows you to access.

The actual object controls which overridden instance method executes.

Example:

```java
class Animal {

    void sound() {
        System.out.println("Animal");
    }

    void eat() {
        System.out.println("Animal eats");
    }
}

class Dog extends Animal {

    @Override
    void sound() {
        System.out.println("Bark");
    }

    void bark() {
        System.out.println("Bark");
    }
}
```

Now:

```java
Animal a = new Dog();

a.sound(); // Bark
a.eat();   // Animal eats
a.bark();  // Compile-time error
```

Why is `bark()` inaccessible?

Because:

```text
Reference type = Animal
```

and `Animal` does not declare `bark()`.

---

# 13. Upcasting and Overriding

This is extremely common:

```java
Animal a = new Dog();
```

This is upcasting:

```text
Dog
 ↓
Animal
```

It is allowed because:

```text
Dog IS-A Animal
```

Now runtime polymorphism becomes possible:

```java
a.sound();
```

and:

```text
Dog.sound()
```

executes.

---

# 14. Downcasting

If we need child-specific functionality:

```java
Animal a = new Dog();

Dog d = (Dog) a;

d.bark();
```

This is downcasting.

But it must be done carefully.

Incorrect:

```java
Animal a = new Cat();

Dog d = (Dog) a;
```

The compiler allows the cast because the types are related.

But at runtime:

```text
ClassCastException
```

occurs because the actual object is a `Cat`.

---

# 15. Covariant Return Type

Java allows an overriding method to return a subtype of the parent's return type.

Example:

```java
class Animal {
}

class Dog extends Animal {
}

class Parent {

    Animal getAnimal() {
        return new Animal();
    }
}

class Child extends Parent {

    @Override
    Dog getAnimal() {
        return new Dog();
    }
}
```

Parent returns:

```text
Animal
```

Child returns:

```text
Dog
```

Since:

```text
Dog IS-A Animal
```

this is valid.

This is called:

> **Covariant return type**

---

# 16. What is NOT Covariant?

A completely unrelated return type is not allowed.

```java
class Parent {

    Animal getAnimal() {
        return new Animal();
    }
}

class Child extends Parent {

    @Override
    String getAnimal() {
        return "Dog";
    }
}
```

❌ Compile-time error.

Because:

```text
String
```

is not a subtype of:

```text
Animal
```

---

# 17. Access Modifier Rules

An overriding method cannot reduce visibility.

Think:

```text
private
   ↓
default
   ↓
protected
   ↓
public
```

A child can move toward greater accessibility, but not toward less accessibility.

Example:

```java
class Parent {

    protected void show() {
    }
}

class Child extends Parent {

    @Override
    public void show() {
    }
}
```

✅ Valid.

But:

```java
class Parent {

    public void show() {
    }
}

class Child extends Parent {

    @Override
    protected void show() {
    }
}
```

❌ Invalid.

The child is reducing visibility.

---

# 18. Package-Private / Default Access

Suppose:

```java
class Parent {

    void show() {
    }
}
```

The method has package-private access.

A child can override it with:

```java
@Override
public void show() {
}
```

or:

```java
@Override
protected void show() {
}
```

But not:

```java
@Override
private void show() {
}
```

because `private` is more restrictive.

---

# 19. `final` Methods

A `final` method cannot be overridden.

```java
class Parent {

    final void show() {
        System.out.println("Parent");
    }
}
```

This is illegal:

```java
class Child extends Parent {

    @Override
    void show() {
        System.out.println("Child");
    }
}
```

Compiler error.

Why?

Because `final` means:

```text
This method's implementation cannot be replaced by subclasses.
```

---

# 20. `static` Methods

Static methods belong to the class rather than participating in normal instance-method dispatch.

Example:

```java
class Parent {

    static void show() {
        System.out.println("Parent");
    }
}

class Child extends Parent {

    static void show() {
        System.out.println("Child");
    }
}
```

Now:

```java
Parent p = new Child();

p.show();
```

Output:

```text
Parent
```

This is called:

> **Method hiding**

not overriding.

The method selected depends on the compile-time type/class context, not the actual object in the way an overridden instance method does.

---

# 21. Private Methods

Private methods cannot be overridden.

Example:

```java
class Parent {

    private void show() {
        System.out.println("Parent");
    }
}

class Child extends Parent {

    private void show() {
        System.out.println("Child");
    }
}
```

These are two separate methods.

The child method is not an override of the parent's private method.

---

# 22. Why Can't Private Methods Be Overridden?

Because the child class cannot directly access the parent's private method.

Private means:

```text
Accessible only within the declaring class
```

Therefore, there is no inherited overridable method available to the subclass.

---

# 23. Constructors Cannot Be Overridden

Constructors are not inherited.

Therefore:

```text
Constructors
    ↓
Cannot be overridden
```

They can only be overloaded.

Example:

```java
class Parent {

    Parent() {
        System.out.println("Parent");
    }
}

class Child extends Parent {

    Child() {
        System.out.println("Child");
    }
}
```

Creating:

```java
new Child();
```

calls:

```text
Parent constructor
        ↓
Child constructor
```

This is constructor chaining, not overriding.

---

# 24. `super` and Method Overriding

Sometimes a child overrides a method but still wants to execute the parent's implementation.

Use:

```java
super.methodName();
```

Example:

```java
class Animal {

    void sound() {
        System.out.println("Animal sound");
    }
}

class Dog extends Animal {

    @Override
    void sound() {

        super.sound();

        System.out.println("Dog barks");
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
Animal sound
Dog barks
```

So:

```text
super.sound()
      ↓
Parent implementation
```

---

# 25. `super` vs `this`

In overriding:

```text
this
 ↓
Current object/current class context
```

while:

```text
super
 ↓
Parent class implementation/context
```

Example:

```java
class Child extends Parent {

    @Override
    void show() {

        this.show();   // Calls Child.show() again → recursion
        super.show();  // Calls Parent.show()
    }
}
```

Be careful with `this.show()` inside an overridden method because it calls the current implementation again.

---

# 26. Abstract Methods and Overriding

Abstract methods must be implemented by concrete subclasses.

Example:

```java
abstract class Animal {

    abstract void sound();
}
```

Child:

```java
class Dog extends Animal {

    @Override
    void sound() {
        System.out.println("Bark");
    }
}
```

Here the child provides the implementation required by the abstract method.

This is closely connected to abstraction and polymorphism.

---

# 27. Interface Methods and Overriding

Interfaces also enable overriding.

```java
interface Payment {

    void pay();
}
```

Implementation:

```java
class UPI implements Payment {

    @Override
    public void pay() {
        System.out.println("Paid using UPI");
    }
}
```

Here:

```text
Payment.pay()
      ↓
UPI.pay()
```

The implementing class provides the required implementation.

---

# 28. Multiple Levels of Overriding

Overriding can occur across multiple levels of inheritance.

```java
class Animal {

    void sound() {
        System.out.println("Animal");
    }
}

class Dog extends Animal {

    @Override
    void sound() {
        System.out.println("Dog");
    }
}

class Puppy extends Dog {

    @Override
    void sound() {
        System.out.println("Puppy");
    }
}
```

Now:

```java
Animal a = new Puppy();

a.sound();
```

Output:

```text
Puppy
```

The most specific overridden implementation is selected.

---

# 29. Calling Parent Implementation from Multiple Levels

Suppose:

```java
class Animal {

    void sound() {
        System.out.println("Animal");
    }
}

class Dog extends Animal {

    @Override
    void sound() {
        System.out.println("Dog");
        super.sound();
    }
}

class Puppy extends Dog {

    @Override
    void sound() {
        System.out.println("Puppy");
        super.sound();
    }
}
```

Calling:

```java
Puppy p = new Puppy();

p.sound();
```

Output:

```text
Puppy
Dog
Animal
```

Because each level calls its immediate parent's implementation.

---

# 30. Exceptions and Method Overriding

An overriding method has special rules for checked exceptions.

A child method cannot introduce a broader checked exception than the parent method declares.

Example:

```java
class Parent {

    void show() throws IOException {
    }
}
```

The child can declare:

```java
class Child extends Parent {

    @Override
    void show() throws IOException {
    }
}
```

or:

```java
@Override
void show() throws FileNotFoundException {
}
```

because:

```text
FileNotFoundException
        ↓
IOException
```

It is a subtype.

The child can also throw no checked exception.

But it cannot do:

```java
@Override
void show() throws Exception {
}
```

because `Exception` is broader than `IOException`.

---

# 31. Runtime Exceptions

Unchecked exceptions do not have the same restriction.

For example:

```java
class Parent {

    void show() {
    }
}

class Child extends Parent {

    @Override
    void show() throws RuntimeException {
    }
}
```

This is allowed.

The special restriction mainly concerns **checked exceptions**.

---

# 32. Can We Override a Method with a Different Parameter Type?

No.

This:

```java
class Parent {

    void show(int x) {
    }
}

class Child extends Parent {

    void show(long x) {
    }
}
```

is not overriding.

It is overloading.

Because:

```text
show(int)
show(long)
```

are different signatures.

---

# 33. Can We Override a Method with a Different Return Type?

Only if the return type is:

```text
Same type
```

or:

```text
Subtype of parent's return type
```

Example:

```text
Parent → Animal
Child  → Dog
```

✅ Valid.

But:

```text
Parent → Animal
Child  → String
```

❌ Invalid if `String` is unrelated to `Animal`.

---

# 34. Can We Override a `public` Method with `protected`?

No.

```java
class Parent {

    public void show() {
    }
}

class Child extends Parent {

    protected void show() {
    }
}
```

❌ Compile-time error.

Reason:

```text
public
 ↓
more accessible

protected
 ↓
less accessible
```

Overriding cannot reduce visibility.

---

# 35. Can We Override a `protected` Method with `public`?

Yes.

```java
class Parent {

    protected void show() {
    }
}

class Child extends Parent {

    @Override
    public void show() {
    }
}
```

✅ Valid.

---

# 36. Can We Override a Default Interface Method?

Yes.

Example:

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
        System.out.println("Bark");
    }
}
```

The class provides its own implementation.

---

# 37. Can We Override a `default` Method with `public`?

Yes.

Interface methods that are implemented by a class must satisfy the interface's accessibility requirements.

```java
interface Animal {

    default void sound() {
    }
}

class Dog implements Animal {

    @Override
    public void sound() {
    }
}
```

This is valid.

---

# 38. Fields vs Overridden Methods

This is a major interview trap.

```java
class Parent {

    int value = 10;

    void show() {
        System.out.println("Parent");
    }
}

class Child extends Parent {

    int value = 20;

    @Override
    void show() {
        System.out.println("Child");
    }
}
```

Now:

```java
Parent p = new Child();

System.out.println(p.value);
p.show();
```

Output:

```text
10
Child
```

Why?

```text
Field
 ↓
Reference type

Overridden instance method
 ↓
Actual object
```

So:

```text
p.value → Parent.value
p.show() → Child.show()
```

---

# 39. Static vs Instance Method

Remember:

```text
Instance method
    ↓
Can be overridden
    ↓
Runtime dispatch
```

```text
Static method
    ↓
Cannot be overridden
    ↓
Method hiding
```

This distinction is extremely important in interviews.

---

# 40. Method Overriding and `final`

There are three related uses of `final`:

```text
final variable
    ↓
Cannot be reassigned

final method
    ↓
Cannot be overridden

final class
    ↓
Cannot be extended
```

Example:

```java
final class Dog {
}
```

Then:

```java
class Puppy extends Dog {
}
```

❌ Invalid.

Because a final class cannot be inherited.

---

# 41. Overriding and Object Class

Every Java class ultimately inherits from:

```java
Object
```

Therefore, methods such as:

```text
toString()
equals()
hashCode()
```

can be overridden.

Example:

```java
class Student {

    private String name;

    Student(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Student{name='" + name + "'}";
    }
}
```

Now:

```java
Student s = new Student("Rahul");

System.out.println(s);
```

The overridden `toString()` is used.

This is an important real-world use of overriding.

---

# 42. Why `equals()` and `hashCode()` Matter

Because `Object` provides implementations of:

```java
equals()
hashCode()
```

classes can override them to define their own equality behavior.

For example:

```java
class Student {

    int id;

    Student(int id) {
        this.id = id;
    }

    @Override
    public boolean equals(Object obj) {

        if (this == obj) {
            return true;
        }

        if (!(obj instanceof Student other)) {
            return false;
        }

        return this.id == other.id;
    }

    @Override
    public int hashCode() {
        return Integer.hashCode(id);
    }
}
```

This becomes especially important when you study:

```text
HashMap
HashSet
equals()
hashCode()
```

later in Collections.

---

# 43. Real-World Example — Payment System

```java
class Payment {

    void pay() {
        System.out.println("Processing payment");
    }
}
```

UPI:

```java
class UPI extends Payment {

    @Override
    void pay() {
        System.out.println("Processing UPI payment");
    }
}
```

Card:

```java
class CardPayment extends Payment {

    @Override
    void pay() {
        System.out.println("Processing card payment");
    }
}
```

Now:

```java
Payment payment;

payment = new UPI();
payment.pay();

payment = new CardPayment();
payment.pay();
```

Output:

```text
Processing UPI payment
Processing card payment
```

This is runtime polymorphism through method overriding.

---

# 44. Real-World Backend Example

Suppose an application has:

```text
Notification
    |
    +-- EmailNotification
    +-- SMSNotification
    +-- PushNotification
```

Parent:

```java
interface Notification {

    void send();
}
```

Implementations:

```java
class EmailNotification implements Notification {

    @Override
    public void send() {
        System.out.println("Sending email");
    }
}
```

```java
class SMSNotification implements Notification {

    @Override
    public void send() {
        System.out.println("Sending SMS");
    }
}
```

Now:

```java
Notification notification;

notification = new EmailNotification();
notification.send();

notification = new SMSNotification();
notification.send();
```

The client code depends on:

```text
Notification
```

rather than concrete implementations.

This is one of the foundations of loosely coupled software design.

---

# 45. Advantages of Method Overriding

### 1. Runtime Polymorphism

Different objects can respond differently to the same method call.

### 2. Specialization

Child classes can specialize parent behavior.

### 3. Extensibility

New subclasses can provide new implementations.

### 4. Loose Coupling

Code can work against a parent class or interface.

### 5. Reusability

Common behavior can remain in the parent while specialized behavior goes into subclasses.

---

# 46. Common Interview Traps

### Trap 1

```java
Parent p = new Child();
p.show();
```

If `show()` is overridden:

```text
Child.show()
```

executes.

---

### Trap 2

Static methods are not overridden.

They are hidden.

---

### Trap 3

Private methods are not overridden.

---

### Trap 4

Final methods cannot be overridden.

---

### Trap 5

Constructors cannot be overridden.

---

### Trap 6

Fields are not dynamically dispatched.

---

### Trap 7

Changing parameters creates overloading, not overriding.

---

### Trap 8

Return type can be covariant, but an unrelated return type is invalid.

---

### Trap 9

An overriding method cannot reduce access.

```text
public → protected ❌
protected → private ❌
```

But:

```text
protected → public ✅
```

---

### Trap 10

`@Override` is not what creates overriding.

The language rules create the override.

`@Override` simply tells the compiler:

> "I intend this method to override a superclass/interface method."

---

# 47. Method Overriding vs Method Hiding

| Overriding             | Hiding                                          |
| ---------------------- | ----------------------------------------------- |
| Instance methods       | Static methods                                  |
| Runtime dispatch       | Compile-time/class-based selection              |
| Actual object matters  | Reference/class context matters                 |
| Dynamic polymorphism   | Not runtime overriding                          |
| `@Override` applicable | `@Override` cannot make static methods override |

---

# 48. Method Overriding vs Method Overloading

```text
                    Methods
                       |
             +---------+---------+
             |                   |
        Overloading          Overriding
             |                   |
       Same class usually     Parent + Child
             |                   |
       Parameters differ     Parameters same
             |                   |
       Compile time          Runtime
             |                   |
      Static binding       Dynamic dispatch
```

---

# 49. Complete Mental Model

When you see:

```java
Parent p = new Child();
```

think:

```text
             Parent reference
                    |
                    ↓
             Child object
```

Then ask:

### What member are we accessing?

If it is an ordinary field:

```text
Reference type matters
```

If it is an overridden instance method:

```text
Actual object matters
```

If it is static:

```text
Class/reference context matters
```

If it is private:

```text
No overriding
```

If it is final:

```text
No overriding
```

---

# 50. 30-Second Interview Answer

> **Method overriding occurs when a subclass provides its own implementation of an inherited instance method using the same method signature. It requires inheritance and enables runtime polymorphism. When a parent reference refers to a child object, the JVM dynamically selects the overridden method based on the actual object. The overriding method can have the same or a covariant return type and cannot reduce the parent's access level. Static, private, and final methods cannot be overridden.**

Example:

```java
class Animal {

    void sound() {
        System.out.println("Animal");
    }
}

class Dog extends Animal {

    @Override
    void sound() {
        System.out.println("Bark");
    }
}

Animal a = new Dog();

a.sound();
```

Output:

```text
Bark
```

Because:

```text
Reference → Animal
Object    → Dog
Method    → Dog.sound()
```

---

# ⭐ Final Memory Trick

```text
OVERRIDING

Parent
  ↓
Inherited instance method
  ↓
Child provides same signature
  ↓
Runtime
  ↓
Actual object decides implementation
```

Remember:

> **Overloading asks: "Which method signature matches?" → Compile time.**

> **Overriding asks: "Which object's implementation should run?" → Runtime.**

---

# 🔥 Top 10 Interview Questions & Answers

## 1. What is Method Overriding?

### Answer

Method overriding occurs when a subclass provides its own implementation of an inherited instance method using the same method signature as the parent method.

```java
class Animal {

    void sound() {
        System.out.println("Animal");
    }
}

class Dog extends Animal {

    @Override
    void sound() {
        System.out.println("Dog");
    }
}
```

Here, `Dog` overrides the `sound()` method of `Animal`.

**Key point:**

```text
Inheritance
+
Same method signature
+
Different implementation
=
Method Overriding
```

---

## 2. What are the rules of Method Overriding?

### Answer

The important rules are:

1. Inheritance must exist.
2. Method name must be the same.
3. Parameter list must be the same.
4. Return type must be the same or covariant.
5. Access level cannot be reduced.
6. `final` methods cannot be overridden.
7. `static` methods are hidden, not overridden.
8. `private` methods cannot be overridden.
9. Constructors cannot be overridden.
10. Checked exceptions cannot be broader than those declared by the parent method.

---

## 3. Why is Method Overriding called Runtime Polymorphism?

### Answer

Because the method implementation is selected at runtime based on the **actual object**, not simply the reference type.

```java
class Animal {

    void sound() {
        System.out.println("Animal");
    }
}

class Dog extends Animal {

    @Override
    void sound() {
        System.out.println("Dog");
    }
}

Animal a = new Dog();

a.sound();
```

Output:

```text
Dog
```

Here:

```text
Reference Type → Animal
Object Type    → Dog
```

The compiler checks that `Animal` has `sound()`.

At runtime, the actual object is `Dog`, so:

```text
Dog.sound()
```

executes.

Therefore, overriding enables **runtime polymorphism**.

---

## 4. What is Dynamic Method Dispatch?

### Answer

Dynamic Method Dispatch is the mechanism by which Java determines at runtime which overridden instance method should execute based on the actual object.

```java
Animal animal;

animal = new Dog();
animal.sound();

animal = new Cat();
animal.sound();
```

The same reference:

```text
animal
```

can refer to different objects.

Therefore:

```text
animal → Dog → Dog.sound()

animal → Cat → Cat.sound()
```

The method call is dynamically dispatched at runtime.

---

## 5. Can Static Methods be Overridden?

### Answer

**No. Static methods cannot be overridden.**

If a child class defines a static method with the same signature as the parent, it is called **method hiding**, not overriding.

```java
class Parent {

    static void show() {
        System.out.println("Parent");
    }
}

class Child extends Parent {

    static void show() {
        System.out.println("Child");
    }
}

Parent p = new Child();

p.show();
```

Output:

```text
Parent
```

Why?

Because static methods belong to the class and are not dynamically dispatched based on the object.

Remember:

```text
Instance method → Overriding → Runtime dispatch

Static method   → Hiding      → Class/reference context
```

---

## 6. Can Private Methods be Overridden?

### Answer

**No. Private methods cannot be overridden.**

```java
class Parent {

    private void show() {
        System.out.println("Parent");
    }
}

class Child extends Parent {

    private void show() {
        System.out.println("Child");
    }
}
```

These are two separate methods.

```text
Parent.show()
Child.show()
```

The child method is **not** overriding the parent's private method.

Private methods are accessible only within the class that declares them and are not inherited in the manner required for overriding.

---

## 7. Can Final Methods be Overridden?

### Answer

**No. A `final` method cannot be overridden.**

```java
class Parent {

    final void show() {
        System.out.println("Parent");
    }
}
```

This is invalid:

```java
class Child extends Parent {

    @Override
    void show() {
        System.out.println("Child");
    }
}
```

The compiler reports an error because `final` prevents a subclass from replacing the method implementation.

Remember:

```text
final variable → Cannot be reassigned

final method   → Cannot be overridden

final class    → Cannot be extended
```

---

## 8. What is a Covariant Return Type?

### Answer

A covariant return type means that an overriding method can return the **same type or a subtype** of the return type declared by the parent method.

```java
class Animal {
}

class Dog extends Animal {
}

class Parent {

    Animal getAnimal() {
        return new Animal();
    }
}

class Child extends Parent {

    @Override
    Dog getAnimal() {
        return new Dog();
    }
}
```

Here:

```text
Parent → Animal
Child  → Dog
```

Since:

```text
Dog IS-A Animal
```

the overriding method is valid.

This is called a **covariant return type**.

An unrelated return type is not allowed.

```text
Animal → String ❌
Animal → Dog    ✅
```

---

## 9. Why can't an Overriding Method Reduce Access?

### Answer

An overriding method cannot have a more restrictive access modifier than the parent method.

For example:

```java
class Parent {

    public void show() {
    }
}
```

This is invalid:

```java
class Child extends Parent {

    @Override
    protected void show() {
    }
}
```

Because:

```text
public
  ↓
protected
```

reduces accessibility.

However, increasing accessibility is allowed:

```java
class Parent {

    protected void show() {
    }
}

class Child extends Parent {

    @Override
    public void show() {
    }
}
```

This is valid.

### Memory Trick

```text
Can increase visibility → ✅

Can reduce visibility  → ❌
```

---

## 10. What happens when a Parent reference points to a Child object?

### Answer

Consider:

```java
Parent p = new Child();
```

There are two types:

```text
Reference Type → Parent
Object Type    → Child
```

The **reference type** determines what members the compiler allows you to access.

The **actual object** determines which overridden instance method executes.

Example:

```java
class Parent {

    void show() {
        System.out.println("Parent");
    }
}

class Child extends Parent {

    @Override
    void show() {
        System.out.println("Child");
    }

    void specificMethod() {
        System.out.println("Child-specific");
    }
}

Parent p = new Child();

p.show();
```

Output:

```text
Child
```

Because `show()` is overridden and the actual object is a `Child`.

But:

```java
p.specificMethod();
```

does not compile because `specificMethod()` is not declared in `Parent`.

So remember:

```text
Parent p = new Child();

        ↓

Reference Type → controls compile-time accessibility

Object Type    → controls overridden instance method
                at runtime
```

### ⭐ Interview Trap

```java
class Parent {

    int x = 10;

    void show() {
        System.out.println("Parent");
    }
}

class Child extends Parent {

    int x = 20;

    @Override
    void show() {
        System.out.println("Child");
    }
}

Parent p = new Child();

System.out.println(p.x);
p.show();
```

Output:

```text
10
Child
```

Why?

```text
Field
 ↓
Reference type

Overridden instance method
 ↓
Actual object
```

Therefore:

```text
p.x     → Parent.x
p.show() → Child.show()
```

---

# 🧠 10-Question Quick Revision

| #  | Question                         | Core Answer                                                         |
| -- | -------------------------------- | ------------------------------------------------------------------- |
| 1  | What is overriding?              | Child provides its own implementation of inherited instance method  |
| 2  | Rules?                           | Same name + same parameters + compatible return type + inheritance  |
| 3  | Why runtime polymorphism?        | Actual object decides overridden method at runtime                  |
| 4  | Dynamic dispatch?                | Runtime selection of overridden instance method                     |
| 5  | Static methods?                  | Not overridden; they are hidden                                     |
| 6  | Private methods?                 | Cannot be overridden                                                |
| 7  | Final methods?                   | Cannot be overridden                                                |
| 8  | Covariant return type?           | Same or subtype return type                                         |
| 9  | Reduce access?                   | No, visibility cannot be reduced                                    |
| 10 | Parent reference → Child object? | Reference controls accessibility; object controls overridden method |

---

# 🎯 30-Second Interview Answer

> **Method overriding occurs when a subclass provides its own implementation of an inherited instance method using the same method signature. It requires inheritance and enables runtime polymorphism. When a parent reference refers to a child object, the overridden instance method is selected based on the actual object at runtime. The overriding method can have the same or a covariant return type and cannot reduce the parent's access level. Static, private, and final methods cannot be overridden.**

```java
Animal animal = new Dog();

animal.sound();
```

If `Dog` overrides `sound()`:

```text
Reference → Animal
Object    → Dog
Method    → Dog.sound()
```

So the key interview concept is:

```text
Overloading
→ Compile Time
→ Different parameters

Overriding
→ Runtime
→ Same parameters
→ Child implementation
```

