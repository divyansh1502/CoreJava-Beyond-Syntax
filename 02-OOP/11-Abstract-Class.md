# ☕ Java OOP — Abstract Class

> **An abstract class is a class declared with the `abstract` keyword that provides a common base for subclasses and can contain both implemented and abstract behavior.**

---

# 📚 Table of Contents

- [1. What is an Abstract Class?](#1-what-is-an-abstract-class)
- [2. Why Do We Need Abstract Classes?](#2-why-do-we-need-abstract-classes)
- [3. Syntax](#3-syntax)
- [4. Abstract Class Cannot Be Instantiated](#4-abstract-class-cannot-be-instantiated)
- [5. Can We Create a Reference of an Abstract Class?](#5-can-we-create-a-reference-of-an-abstract-class)
- [6. Abstract Method](#6-abstract-method)
- [7. Concrete Method](#7-concrete-method)
- [8. Abstract Class Can Have Both Methods](#8-abstract-class-can-have-both-methods)
- [9. Abstract Class Without Abstract Methods](#9-abstract-class-without-abstract-methods)
- [10. Can an Abstract Class Have Constructors?](#10-can-an-abstract-class-have-constructors)
- [11. Why Does an Abstract Class Need a Constructor?](#11-why-does-an-abstract-class-need-a-constructor)
- [12. Can Abstract Class Have Instance Variables?](#12-can-abstract-class-have-instance-variables)
- [13. Can Abstract Class Have Static Methods?](#13-can-abstract-class-have-static-methods)
- [14. Can Abstract Class Have Final Methods?](#14-can-abstract-class-have-final-methods)
- [15. Can an Abstract Method Be Final?](#15-can-an-abstract-method-be-final)
- [16. Can an Abstract Method Be Static?](#16-can-an-abstract-method-be-static)
- [17. Can an Abstract Method Be Private?](#17-can-an-abstract-method-be-private)
- [18. Can an Abstract Method Be Protected?](#18-can-an-abstract-method-be-protected)
- [19. Can an Abstract Class Implement an Interface?](#19-can-an-abstract-class-implement-an-interface)
- [20. Can an Abstract Class Extend Another Abstract Class?](#20-can-an-abstract-class-extend-another-abstract-class)
- [21. Can a Concrete Class Extend an Abstract Class?](#21-can-a-concrete-class-extend-an-abstract-class)
- [22. What Happens If the Child Doesn't Implement an Abstract Method?](#22-what-happens-if-the-child-doesnt-implement-an-abstract-method)
- [23. Abstract Class and Runtime Polymorphism](#23-abstract-class-and-runtime-polymorphism)
- [24. Abstract Class vs Normal Class](#24-abstract-class-vs-normal-class)
- [25. Abstract Class vs Interface](#25-abstract-class-vs-interface)
- [26. When Should We Use an Abstract Class?](#26-when-should-we-use-an-abstract-class)
- [27. Real-World Example — Employee](#27-real-world-example--employee)
- [28. Abstract Class Can Have a Main Method](#28-abstract-class-can-have-a-main-method)
- [29. Can We Create an Anonymous Class from an Abstract Class?](#29-can-we-create-an-anonymous-class-from-an-abstract-class)
- [30. Abstract Class Reference](#30-abstract-class-reference)
- [31. Abstract Class and `super`](#31-abstract-class-and-super)
- [32. Abstract Class and Access Modifiers](#32-abstract-class-and-access-modifiers)
- [33. Can an Abstract Class Be Final?](#33-can-an-abstract-class-be-final)
- [34. Can an Abstract Class Be Private?](#34-can-an-abstract-class-be-private)
- [35. Can an Abstract Class Be Static?](#35-can-an-abstract-class-be-static)
- [36. Abstract Method Syntax](#36-abstract-method-syntax)
- [37. Abstract Class Can Have Zero Abstract Methods](#37-abstract-class-can-have-zero-abstract-methods)
- [38. Abstract Class Can Have All Concrete Methods](#38-abstract-class-can-have-all-concrete-methods)
- [39. Abstract Class Can Have No Methods](#39-abstract-class-can-have-no-methods)
- [40. Abstract Class and Data Hiding](#40-abstract-class-and-data-hiding)
- [41. Abstract Class and Template Method Pattern](#41-abstract-class-and-template-method-pattern)
- [42. Important Interview Trap — Abstract Does Not Mean "Only Abstract"](#42-important-interview-trap--abstract-does-not-mean-only-abstract)
- [43. Important Interview Trap — Abstract Class Does Not Mean Objectless](#43-important-interview-trap--abstract-class-does-not-mean-objectless)
- [44. Important Interview Trap — Abstract Constructor](#44-important-interview-trap--abstract-constructor)
- [45. Important Interview Trap — Abstract + Static](#45-important-interview-trap--abstract--static)
- [46. Important Interview Trap — Abstract + Final](#46-important-interview-trap--abstract--final)
- [47. Important Interview Trap — Abstract + Private](#47-important-interview-trap--abstract--private)
- [48. Abstract Class Can Be Used as Reference](#48-abstract-class-can-be-used-as-reference)
- [49. Abstract Class and Multiple Inheritance](#49-abstract-class-and-multiple-inheritance)
- [50. Abstract Class — Complete Mental Model](#50-abstract-class--complete-mental-model)
- [51. Abstract Class — 30-Second Interview Answer](#51-abstract-class--30-second-interview-answer)
- [Top 10 Most Important Interview Questions + Answers](#-top-10-most-important-interview-questions--answers)
- [Quick Revision](#-quick-revision)
- [Final Memory Tricks](#-final-memory-tricks)
---

# 1. What is an Abstract Class?

An abstract class is a class that is declared using the `abstract` keyword.

```java
abstract class Animal {

    abstract void sound();

    void eat() {
        System.out.println("Animal eats");
    }
}
```

Here:

```text
Animal
  ↓
Abstract Class
  ↓
Can contain abstract + concrete methods
```

The important point is:

> An abstract class is designed to be inherited rather than directly instantiated.

---

# 2. Why Do We Need Abstract Classes?

Suppose we have:

```text
Animal
 ├── Dog
 ├── Cat
 └── Cow
```

Every animal may have:

```text
eat()
sleep()
```

But every animal can have a different:

```text
sound()
```

Instead of creating completely separate classes, we can define common behavior in an abstract class.

```java
abstract class Animal {

    void eat() {
        System.out.println("Animal eats");
    }

    abstract void sound();
}
```

Then subclasses provide their specific behavior.

```java
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

This gives us:

```text
Common behavior
      ↓
Abstract Class
      ↓
Specific behavior
      ↓
Child Classes
```

---

# 3. Syntax

```java
abstract class ClassName {

    abstract void method1();

    void method2() {
        // implementation
    }
}
```

Example:

```java
abstract class Vehicle {

    abstract void start();

    void stop() {
        System.out.println("Vehicle stopped");
    }
}
```

---

# 4. Abstract Class Cannot Be Instantiated

You cannot directly create an object of an abstract class.

```java
abstract class Animal {

}
```

This is invalid:

```java
Animal a = new Animal();
```

Compile-time error.

Why?

Because an abstract class may contain incomplete behavior.

For example:

```java
abstract class Animal {

    abstract void sound();
}
```

What should this do?

```java
Animal a = new Animal();
a.sound();
```

There is no implementation of `sound()`.

Therefore Java prevents direct instantiation.

---

# 5. Can We Create a Reference of an Abstract Class?

Yes.

This is valid:

```java
Animal a;
```

And this is also valid:

```java
Animal a = new Dog();
```

Example:

```java
abstract class Animal {

    abstract void sound();
}

class Dog extends Animal {

    @Override
    void sound() {
        System.out.println("Bark");
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
Bark
```

Here:

```text
Reference Type → Animal
Object Type    → Dog
```

This enables runtime polymorphism.

---

# 6. Abstract Method

An abstract method is a method declared without an implementation.

```java
abstract void sound();
```

It contains:

```text
Method declaration
        ↓
No method body
```

Example:

```java
abstract class Animal {

    abstract void sound();
}
```

The subclass must normally implement it.

```java
class Dog extends Animal {

    @Override
    void sound() {
        System.out.println("Bark");
    }
}
```

---

# 7. Concrete Method

A concrete method contains an implementation.

```java
void eat() {
    System.out.println("Animal eats");
}
```

An abstract class can contain concrete methods.

Example:

```java
abstract class Animal {

    abstract void sound();

    void eat() {
        System.out.println("Animal eats");
    }
}
```

So:

```text
Abstract Class
     |
     +── Abstract methods
     |
     +── Concrete methods
```

---

# 8. Abstract Class Can Have Both Methods

This is one of the most important concepts.

```java
abstract class Animal {

    abstract void sound();

    void eat() {
        System.out.println("Eating...");
    }
}
```

Here:

```text
sound()
  ↓
Abstract

eat()
  ↓
Concrete
```

The purpose is to combine:

```text
Incomplete behavior
+
Common implemented behavior
```

---

# 9. Abstract Class Without Abstract Methods

An abstract class does NOT have to contain an abstract method.

This is valid:

```java
abstract class Animal {

    void eat() {
        System.out.println("Eating");
    }
}
```

There is no abstract method here.

But:

```java
Animal a = new Animal();
```

is still invalid because the class itself is abstract.

This is an important interview trap.

---

# 10. Can an Abstract Class Have Constructors?

Yes.

This is a very common interview question.

Example:

```java
abstract class Animal {

    Animal() {
        System.out.println("Animal constructor");
    }
}
```

Even though we cannot directly create:

```java
new Animal();
```

the constructor can still execute when a subclass object is created.

Example:

```java
abstract class Animal {

    Animal() {
        System.out.println("Animal constructor");
    }
}

class Dog extends Animal {

    Dog() {
        System.out.println("Dog constructor");
    }
}
```

Now:

```java
Dog d = new Dog();
```

Output:

```text
Animal constructor
Dog constructor
```

Why?

Because superclass construction happens before subclass construction.

---

# 11. Why Does an Abstract Class Need a Constructor?

The abstract class can contain instance variables and common initialization logic.

Example:

```java
abstract class Employee {

    String name;

    Employee(String name) {
        this.name = name;
    }
}
```

Child:

```java
class Developer extends Employee {

    Developer(String name) {
        super(name);
    }
}
```

Now:

```java
Developer d = new Developer("Rahul");

System.out.println(d.name);
```

Output:

```text
Rahul
```

The abstract class constructor initializes the inherited state.

---

# 12. Can Abstract Class Have Instance Variables?

Yes.

```java
abstract class Animal {

    String name;
    int age;
}
```

It can have:

```text
instance variables
static variables
final variables
constants
```

Example:

```java
abstract class Employee {

    protected String name;
    protected double salary;
}
```

---

# 13. Can Abstract Class Have Static Methods?

Yes.

```java
abstract class Animal {

    static void info() {
        System.out.println("Animals");
    }
}
```

You can call:

```java
Animal.info();
```

No object is required because the method is static.

---

# 14. Can Abstract Class Have Final Methods?

Yes.

```java
abstract class Animal {

    final void breathe() {
        System.out.println("Breathing");
    }
}
```

A child class cannot override:

```java
breathe()
```

because it is final.

This is perfectly valid.

```text
Abstract class
     |
     +── abstract methods
     +── concrete methods
     +── final methods
     +── static methods
     +── instance variables
```

---

# 15. Can an Abstract Method Be Final?

No.

This is invalid:

```java
abstract final void sound();
```

Why?

Because:

```text
abstract
   ↓
Must be implemented by subclass

final
   ↓
Cannot be overridden
```

These two requirements contradict each other.

Therefore:

```text
abstract + final method ❌
```

---

# 16. Can an Abstract Method Be Static?

No.

This is invalid:

```java
abstract static void show();
```

Why?

An abstract method requires subclass implementation through overriding.

A static method does not participate in runtime overriding.

Therefore:

```text
abstract + static ❌
```

---

# 17. Can an Abstract Method Be Private?

No.

```java
abstract private void show();
```

is invalid.

Why?

A private method is not inherited by subclasses.

But an abstract method must be implemented by a subclass.

Therefore:

```text
abstract + private ❌
```

---

# 18. Can an Abstract Method Be Protected?

Yes.

```java
abstract class Animal {

    protected abstract void sound();
}
```

The subclass can implement it with equal or greater visibility.

```java
class Dog extends Animal {

    @Override
    protected void sound() {
        System.out.println("Bark");
    }
}
```

Or:

```java
@Override
public void sound() {
    System.out.println("Bark");
}
```

---

# 19. Can an Abstract Class Implement an Interface?

Yes.

An abstract class can implement an interface without implementing all of its methods.

Example:

```java
interface Animal {

    void sound();
    void eat();
}
```

Abstract class:

```java
abstract class Dog implements Animal {

    @Override
    public void sound() {
        System.out.println("Bark");
    }
}
```

`eat()` is still unimplemented.

That's okay because `Dog` is abstract.

A concrete subclass must implement the remaining methods.

```java
class Puppy extends Dog {

    @Override
    public void eat() {
        System.out.println("Puppy eats");
    }
}
```

---

# 20. Can an Abstract Class Extend Another Abstract Class?

Yes.

```java
abstract class Animal {

    abstract void sound();
}
```

Another abstract class:

```java
abstract class Dog extends Animal {

}
```

`Dog` does not have to implement `sound()` because `Dog` itself is abstract.

Eventually, a concrete subclass must implement it.

```java
class Puppy extends Dog {

    @Override
    void sound() {
        System.out.println("Bark");
    }
}
```

---

# 21. Can a Concrete Class Extend an Abstract Class?

Yes.

But it must implement all inherited abstract methods.

```java
abstract class Animal {

    abstract void sound();
}
```

Concrete child:

```java
class Dog extends Animal {

    @Override
    void sound() {
        System.out.println("Bark");
    }
}
```

Now `Dog` is concrete.

Therefore:

```java
Dog d = new Dog();
```

is valid.

---

# 22. What Happens If the Child Doesn't Implement an Abstract Method?

Suppose:

```java
abstract class Animal {

    abstract void sound();
}
```

And:

```java
class Dog extends Animal {

}
```

This causes a compile-time error because `Dog` is concrete but has not implemented `sound()`.

There are two solutions.

### Solution 1 — Implement the method

```java
class Dog extends Animal {

    @Override
    void sound() {
        System.out.println("Bark");
    }
}
```

### Solution 2 — Make Dog abstract

```java
abstract class Dog extends Animal {

}
```

---

# 23. Abstract Class and Runtime Polymorphism

Abstract classes work naturally with runtime polymorphism.

```java
abstract class Animal {

    abstract void sound();
}
```

```java
class Dog extends Animal {

    @Override
    void sound() {
        System.out.println("Bark");
    }
}
```

```java
class Cat extends Animal {

    @Override
    void sound() {
        System.out.println("Meow");
    }
}
```

Now:

```java
Animal a;

a = new Dog();
a.sound();

a = new Cat();
a.sound();
```

Output:

```text
Bark
Meow
```

The reference remains:

```text
Animal
```

but the actual object changes.

---

# 24. Abstract Class vs Normal Class

| Feature                     | Normal Class | Abstract Class |
| --------------------------- | ------------ | -------------- |
| Can create object           | Yes          | No             |
| Can have constructors       | Yes          | Yes            |
| Can have instance variables | Yes          | Yes            |
| Can have static methods     | Yes          | Yes            |
| Can have final methods      | Yes          | Yes            |
| Can have concrete methods   | Yes          | Yes            |
| Can have abstract methods   | No           | Yes            |
| Can be extended             | Yes          | Yes            |
| Can have static variables   | Yes          | Yes            |

---

# 25. Abstract Class vs Interface

This is a major interview topic.

| Feature              | Abstract Class                  | Interface                                                      |
| -------------------- | ------------------------------- | -------------------------------------------------------------- |
| Declared using       | `abstract class`                | `interface`                                                    |
| Object creation      | No                              | No                                                             |
| Constructors         | Yes                             | No                                                             |
| Instance variables   | Yes                             | No instance fields                                             |
| Static fields        | Yes                             | Yes                                                            |
| Abstract methods     | Yes                             | Yes                                                            |
| Concrete methods     | Yes                             | Yes                                                            |
| Instance methods     | Yes                             | Yes                                                            |
| Multiple inheritance | Class can extend only one class | Class can implement multiple interfaces                        |
| State                | Can maintain instance state     | Cannot maintain instance state through instance fields         |
| `this`               | Yes                             | No instance context for fields/method bodies in the same sense |
| `super`              | Yes                             | Interface `super` has special rules                            |
| Static methods       | Yes                             | Yes                                                            |
| Final methods        | Yes                             | No instance method can be declared `final` in an interface     |
| Constructors         | Yes                             | No                                                             |

Important:

> Don't explain interfaces using the outdated rule that "interfaces only contain abstract methods." Modern Java interfaces can contain `default`, `static`, and private methods with implementations.

---

# 26. When Should We Use an Abstract Class?

Use an abstract class when subclasses share:

```text
Common state
+
Common behavior
+
Some behavior that must be implemented differently
```

Example:

```text
Employee
   |
   +── Developer
   +── Manager
   +── Tester
```

Common:

```text
name
salary
id
login()
logout()
```

Different:

```text
work()
```

An abstract class can represent this relationship.

---

# 27. Real-World Example — Employee

```java
abstract class Employee {

    String name;
    double salary;

    Employee(String name, double salary) {
        this.name = name;
        this.salary = salary;
    }

    void login() {
        System.out.println(name + " logged in");
    }

    abstract void work();
}
```

Developer:

```java
class Developer extends Employee {

    Developer(String name, double salary) {
        super(name, salary);
    }

    @Override
    void work() {
        System.out.println("Writing code");
    }
}
```

Manager:

```java
class Manager extends Employee {

    Manager(String name, double salary) {
        super(name, salary);
    }

    @Override
    void work() {
        System.out.println("Managing team");
    }
}
```

Usage:

```java
Employee e1 = new Developer("Rahul", 60000);
Employee e2 = new Manager("Amit", 80000);

e1.login();
e1.work();

e2.login();
e2.work();
```

Output:

```text
Rahul logged in
Writing code
Amit logged in
Managing team
```

This combines:

```text
Abstraction
+
Inheritance
+
Method Overriding
+
Runtime Polymorphism
```

---

# 28. Abstract Class Can Have a Main Method

Yes.

An abstract class can contain:

```java
public static void main(String[] args)
```

Example:

```java
abstract class Animal {

    public static void main(String[] args) {
        System.out.println("Main method");
    }
}
```

You can run the class because `main()` is static.

The restriction is only:

```text
new Animal()
```

not:

```text
Animal.main(...)
```

---

# 29. Can We Create an Anonymous Class from an Abstract Class?

Yes.

This is an important advanced concept.

```java
abstract class Animal {

    abstract void sound();
}
```

You cannot do:

```java
Animal a = new Animal();
```

But you can create an anonymous subclass:

```java
Animal a = new Animal() {

    @Override
    void sound() {
        System.out.println("Anonymous sound");
    }
};
```

Then:

```java
a.sound();
```

Output:

```text
Anonymous sound
```

What's happening?

```text
new Animal()
     ↓
Anonymous subclass created
     ↓
Abstract method implemented
     ↓
Object created
```

You are not actually instantiating the abstract class directly.

---

# 30. Abstract Class Reference

You can use an abstract class as:

```java
Animal a;
```

or:

```java
Animal a = new Dog();
```

But not:

```java
Animal a = new Animal();
```

Remember:

```text
Abstract class reference → YES
Abstract class object    → NO
Child object             → YES
```

---

# 31. Abstract Class and `super`

An abstract class can use `super`.

Example:

```java
abstract class Animal {

    void eat() {
        System.out.println("Animal eats");
    }
}
```

Child:

```java
class Dog extends Animal {

    void eat() {

        super.eat();

        System.out.println("Dog eats");
    }
}
```

Now:

```java
Dog d = new Dog();
d.eat();
```

Output:

```text
Animal eats
Dog eats
```

---

# 32. Abstract Class and Access Modifiers

An abstract class can have:

```text
private
default
protected
public
```

members according to normal Java access rules.

Example:

```java
abstract class Animal {

    private int age;

    protected String name;

    public void eat() {
        System.out.println("Eating");
    }
}
```

The `abstract` keyword does not remove normal access-control rules.

---

# 33. Can an Abstract Class Be Final?

No.

This is invalid:

```java
abstract final class Animal {

}
```

Why?

Because:

```text
abstract class
     ↓
Designed to be inherited

final class
     ↓
Cannot be inherited
```

They contradict each other.

Therefore:

```text
abstract + final class ❌
```

---

# 34. Can an Abstract Class Be Private?

A top-level class cannot be `private`.

But a nested class can have access modifiers.

For example:

```java
class Outer {

    private abstract class Animal {

    }
}
```

This is allowed because `Animal` is a nested class.

---

# 35. Can an Abstract Class Be Static?

A top-level class cannot be static.

But a nested class can be static:

```java
class Outer {

    static abstract class Animal {

    }
}
```

This is valid.

---

# 36. Abstract Method Syntax

Correct:

```java
abstract void sound();
```

Incorrect:

```java
abstract void sound() {
}
```

An abstract method cannot have a body.

If you provide a body, it becomes a concrete method.

---

# 37. Abstract Class Can Have Zero Abstract Methods

This is a classic interview question.

```java
abstract class Animal {

    void eat() {
        System.out.println("Eat");
    }
}
```

Valid.

Why would we do this?

To prevent direct instantiation while still providing a common base class.

---

# 38. Abstract Class Can Have All Concrete Methods

Yes.

```java
abstract class Animal {

    void eat() {
        System.out.println("Eating");
    }

    void sleep() {
        System.out.println("Sleeping");
    }
}
```

No abstract methods are present.

Still:

```java
Animal a = new Animal();
```

is invalid.

---

# 39. Abstract Class Can Have No Methods

Yes.

```java
abstract class Animal {

}
```

This is valid.

The class simply cannot be instantiated directly.

---

# 40. Abstract Class and Data Hiding

Abstract classes can also participate in encapsulation.

```java
abstract class Employee {

    private double salary;

    protected Employee(double salary) {
        this.salary = salary;
    }

    public double getSalary() {
        return salary;
    }

    abstract void work();
}
```

Here:

```text
private salary
      ↓
Encapsulation

abstract work()
      ↓
Abstraction
```

This shows that OOP concepts often work together rather than existing in isolation.

---

# 41. Abstract Class and Template Method Pattern

Abstract classes are commonly used for the Template Method design pattern.

Example:

```java
abstract class DataProcessor {

    final void process() {

        readData();
        processData();
        saveData();
    }

    abstract void readData();

    abstract void processData();

    void saveData() {
        System.out.println("Saving data");
    }
}
```

Child:

```java
class CSVProcessor extends DataProcessor {

    @Override
    void readData() {
        System.out.println("Reading CSV");
    }

    @Override
    void processData() {
        System.out.println("Processing CSV");
    }
}
```

The parent defines the overall algorithm while subclasses customize specific steps.

This is a practical use of:

```text
Abstract Class
+
Abstract Methods
+
Final Method
+
Overriding
```

---

# 42. Important Interview Trap — Abstract Does Not Mean "Only Abstract"

Wrong:

```text
Abstract class = class containing only abstract methods
```

Correct:

```text
Abstract class = class that cannot be directly instantiated
                   and may contain abstract + concrete behavior
```

An abstract class can contain:

```text
constructors
instance variables
static variables
final variables
concrete methods
abstract methods
static methods
final methods
nested classes
```

---

# 43. Important Interview Trap — Abstract Class Does Not Mean Objectless

An abstract class itself cannot be instantiated.

But an object of a concrete subclass contains the inherited state and behavior defined by the abstract class.

Example:

```java
abstract class Animal {

    String name;
}

class Dog extends Animal {

}
```

```java
Dog d = new Dog();
```

The `Dog` object contains the inherited `name` field.

So don't think:

```text
Abstract class → no object-related state
```

Instead:

```text
Abstract class → cannot be directly instantiated
```

---

# 44. Important Interview Trap — Abstract Constructor

Constructors cannot be abstract.

Invalid:

```java
abstract Animal();
```

or:

```java
abstract Animal() {

}
```

Why?

Constructors are used to initialize objects.

An abstract constructor would have no meaningful overriding relationship because constructors are not inherited or overridden.

---

# 45. Important Interview Trap — Abstract + Static

This is invalid:

```java
abstract static void show();
```

Static methods belong to the class and do not participate in normal runtime overriding.

Abstract methods require subclass implementation through overriding.

Therefore:

```text
abstract + static ❌
```

---

# 46. Important Interview Trap — Abstract + Final

Invalid:

```java
abstract final void show();
```

Because:

```text
abstract → must override

final → cannot override
```

Contradiction.

---

# 47. Important Interview Trap — Abstract + Private

Invalid:

```java
abstract private void show();
```

Because:

```text
private → not inherited
abstract → must be implemented by subclass
```

Contradiction.

---

# 48. Important Interview Trap — Abstract Class Can Be Used as Reference

Valid:

```java
abstract class Animal {

    abstract void sound();
}

class Dog extends Animal {

    @Override
    void sound() {
        System.out.println("Bark");
    }
}
```

Then:

```java
Animal a = new Dog();
```

Valid.

This is:

```text
Upcasting
+
Runtime polymorphism
```

---

# 49. Abstract Class and Multiple Inheritance

Java does not allow a class to extend multiple classes.

Invalid:

```java
class C extends A, B {

}
```

This remains true if `A` and `B` are abstract classes.

```text
One class
   ↓
extends
   ↓
Only one class
```

However, a class can implement multiple interfaces.

```java
class C extends A implements X, Y {

}
```

---

# 50. Abstract Class — Complete Mental Model

When you see:

```java
abstract class Animal
```

think:

```text
                Abstract Class
                       |
        +--------------+--------------+
        |              |              |
     State         Common Logic   Abstract Behavior
        |              |              |
   variables      concrete       abstract methods
                     methods
                       |
                       ↓
                  Subclasses
                       |
                       ↓
              Specific behavior
```

The core idea is:

> **Put what is common in the abstract class and force subclasses to provide what is specific.**

---

# 51. Abstract Class — 30-Second Interview Answer

> **An abstract class is a class declared with the `abstract` keyword that cannot be instantiated directly. It is used as a common base for related classes and can contain both abstract methods and concrete methods, along with constructors, variables, static methods, and other normal class members. Subclasses can inherit the common implementation and must implement inherited abstract methods unless the subclass is also abstract. Abstract classes are commonly used with inheritance, method overriding, and runtime polymorphism.**

Example:

```java
abstract class Animal {

    abstract void sound();

    void eat() {
        System.out.println("Eating");
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
a.eat();
```

Output:

```text
Bark
Eating
```

---

# 🔥 Top 10 Most Important Interview Questions + Answers

## 1. What is an abstract class?

**Answer:**

An abstract class is a class declared with the `abstract` keyword that cannot be directly instantiated. It can contain both abstract and concrete methods and is mainly used as a common base for subclasses.

---

## 2. Can we create an object of an abstract class?

**Answer:**

No.

```java
abstract class Animal {
}

Animal a = new Animal(); // ❌
```

However, we can create a reference of an abstract class:

```java
Animal a = new Dog(); // ✅
```

---

## 3. Can an abstract class have constructors?

**Answer:**

Yes.

The constructor executes when a concrete subclass object is created.

```java
abstract class Animal {

    Animal() {
        System.out.println("Animal");
    }
}

class Dog extends Animal {

    Dog() {
        System.out.println("Dog");
    }
}
```

```java
new Dog();
```

Output:

```text
Animal
Dog
```

---

## 4. Can an abstract class have concrete methods?

**Answer:**

Yes.

```java
abstract class Animal {

    abstract void sound();

    void eat() {
        System.out.println("Eating");
    }
}
```

An abstract class can contain both:

```text
Abstract methods
+
Concrete methods
```

---

## 5. Can an abstract class have zero abstract methods?

**Answer:**

Yes.

```java
abstract class Animal {

    void eat() {
        System.out.println("Eating");
    }
}
```

The class remains abstract and cannot be directly instantiated.

---

## 6. Can an abstract class be final?

**Answer:**

No.

```java
abstract final class Animal {
}
```

is invalid.

Reason:

```text
abstract → intended to be inherited

final → cannot be inherited
```

These concepts conflict.

---

## 7. Can an abstract method be static, final, or private?

**Answer:**

No.

```text
abstract + static  ❌
abstract + final   ❌
abstract + private ❌
```

Reason:

```text
abstract → requires subclass implementation

static → does not participate in normal overriding
final → cannot be overridden
private → not inherited
```

---

## 8. Can an abstract class implement an interface?

**Answer:**

Yes.

An abstract class does not have to implement every interface method.

```java
interface Payment {

    void pay();
}

abstract class OnlinePayment implements Payment {

}
```

This is valid because `OnlinePayment` is abstract.

A concrete subclass must eventually provide the required implementation.

---

## 9. Can an abstract class extend another abstract class?

**Answer:**

Yes.

```java
abstract class Animal {

    abstract void sound();
}

abstract class Dog extends Animal {

}
```

`Dog` does not need to implement `sound()` because `Dog` is also abstract.

A concrete subclass must eventually implement it.

---

## 10. What is the difference between an abstract class and an interface?

**Answer:**

An abstract class is useful when related classes need shared state and shared implementation along with abstract behavior.

An interface is primarily used to define a contract/capability that classes can implement, and a class can implement multiple interfaces.

Key differences:

```text
Abstract Class
    ↓
Can have instance state
Can have constructors
Can have concrete methods
Can have abstract methods
Class extends only one class

Interface
    ↓
No instance fields
No constructors
Can have abstract/default/static/private methods
Class can implement multiple interfaces
```

---

# ⚡ Quick Revision

```text
ABSTRACT CLASS

abstract class Animal
        ↓
Cannot be directly instantiated
        ↓
Can have constructors
        ↓
Can have instance variables
        ↓
Can have concrete methods
        ↓
Can have abstract methods
        ↓
Can have static/final methods
        ↓
Can implement interfaces
        ↓
Can extend another class
        ↓
Used as a common base
        ↓
Supports runtime polymorphism
```

---

# 🧠 Final Memory Tricks

```text
abstract class
      ↓
"BASE CLASS WITH SOME INCOMPLETE/COMMON BEHAVIOR"
```

Remember these:

```text
Abstract Class
    → Cannot instantiate directly
    → Can have constructor
    → Can have abstract + concrete methods
    → Can have state
    → Can have static/final members
    → Can be extended
```

And the forbidden combinations:

```text
abstract class + final class     ❌
abstract method + final          ❌
abstract method + static         ❌
abstract method + private        ❌
abstract constructor             ❌
```

Most important relationship:

```text
Abstract Class
      ↓
Inheritance
      ↓
Abstract Method
      ↓
Method Overriding
      ↓
Runtime Polymorphism
```

> **Abstract class = common structure + common implementation + required subclass-specific behavior.**
