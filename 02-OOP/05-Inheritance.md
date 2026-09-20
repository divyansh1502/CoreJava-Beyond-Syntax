# 🧬 Java OOP — Inheritance

> **Inheritance is an OOP mechanism in which a class derives from another class, allowing it to reuse and specialize accessible state and behavior while forming an IS-A relationship.**

---

# 📚 Table of Contents

* [1. What is Inheritance?](#-1-what-is-inheritance)
* [2. Why Do We Need Inheritance?](#-2-why-do-we-need-inheritance)
* [3. Real-World Analogy](#-3-real-world-analogy)
* [4. Basic Syntax](#-4-basic-syntax)
* [5. Parent and Child Class](#-5-parent-and-child-class)
* [6. IS-A Relationship](#-6-is-a-relationship)
* [7. What Does a Child Class Inherit?](#-7-what-does-a-child-class-inherit)
* [8. What is Not Inherited?](#-8-what-is-not-inherited)
* [9. Types of Inheritance in Java](#-9-types-of-inheritance-in-java)
* [10. Single Inheritance](#-10-single-inheritance)
* [11. Multilevel Inheritance](#-11-multilevel-inheritance)
* [12. Hierarchical Inheritance](#-12-hierarchical-inheritance)
* [13. Multiple Inheritance](#-13-multiple-inheritance)
* [14. Hybrid Inheritance](#-14-hybrid-inheritance)
* [15. Why Java Does Not Support Multiple Class Inheritance](#-15-why-java-does-not-support-multiple-class-inheritance)
* [16. Inheritance and Constructors](#-16-inheritance-and-constructors)
* [17. Constructor Chaining](#-17-constructor-chaining)
* [18. `super` Keyword](#-18-super-keyword)
* [19. Method Overriding](#-19-method-overriding)
* [20. Inheritance and Polymorphism](#-20-inheritance-and-polymorphism)
* [21. Upcasting](#-21-upcasting)
* [22. Downcasting](#-22-downcasting)
* [23. `instanceof`](#-23-instanceof)
* [24. Access Modifiers and Inheritance](#-24-access-modifiers-and-inheritance)
* [25. `protected` and Inheritance](#-25-protected-and-inheritance)
* [26. `final` and Inheritance](#-26-final-and-inheritance)
* [27. Static Members and Inheritance](#-27-static-members-and-inheritance)
* [28. Private Members and Inheritance](#-28-private-members-and-inheritance)
* [29. Object Class and Inheritance](#-29-object-class-and-inheritance)
* [30. Inheritance vs Composition](#-30-inheritance-vs-composition)
* [31. Advantages](#-31-advantages)
* [32. Disadvantages](#-32-disadvantages)
* [33. Common Mistakes](#-33-common-mistakes)
* [34. Interview Questions](#-34-interview-questions)
* [35. Top 10 Interview Questions](#-35-top-10-interview-questions)
* [36. Quick Revision](#-36-quick-revision)
* [37. 30-Second Interview Answer](#-37-30-second-interview-answer)
* [38. Memory Trick](#-38-memory-trick)

---

# 🧠 1. What is Inheritance?

Inheritance allows one class to derive from another class.

Example:

```java
class Animal {

    void eat() {
        System.out.println("Animal is eating");
    }
}

class Dog extends Animal {

    void bark() {
        System.out.println("Dog is barking");
    }
}
```

Here:

```text
Animal
   ↑
   │ extends
   │
  Dog
```

`Dog` is the child/subclass.

`Animal` is the parent/superclass.

The `Dog` class can use the accessible `eat()` behavior inherited from `Animal`.

```java
Dog d = new Dog();

d.eat();
d.bark();
```

Output:

```text
Animal is eating
Dog is barking
```

---

# 🤔 2. Why Do We Need Inheritance?

Imagine a system containing:

```text
Dog
Cat
Horse
Cow
```

All animals may have common behavior:

```text
eat()
sleep()
breathe()
```

Without inheritance, we may repeat the same code:

```java
class Dog {

    void eat() {
        System.out.println("Eating");
    }
}

class Cat {

    void eat() {
        System.out.println("Eating");
    }
}
```

This duplicates common behavior.

Instead:

```java
class Animal {

    void eat() {
        System.out.println("Eating");
    }
}

class Dog extends Animal {
}

class Cat extends Animal {
}
```

Now:

```text
             Animal
             /    \
            /      \
          Dog      Cat
```

Both can use:

```java
eat();
```

### Core idea

> **Inheritance can provide reuse and specialization when there is a genuine IS-A relationship.**

---

# 🌎 3. Real-World Analogy

Consider an organization.

```text
Employee
   │
   ├── Developer
   ├── Manager
   └── Tester
```

All employees may have:

```text
name
id
salary
work()
```

A developer may additionally have:

```text
writeCode()
```

A manager may have:

```text
manageTeam()
```

Conceptually:

```text
                    Employee
                       │
          ┌────────────┼────────────┐
          ▼            ▼            ▼
      Developer      Manager      Tester
          │            │            │
      writeCode()  manageTeam()  testSoftware()
```

The specialized classes can reuse common employee behavior while adding their own behavior.

---

# 💻 4. Basic Syntax

Java uses the `extends` keyword for class inheritance.

```java
class Parent {

    // members
}

class Child extends Parent {

    // additional members
}
```

Example:

```java
class Vehicle {

    void start() {
        System.out.println("Vehicle starts");
    }
}

class Car extends Vehicle {

    void drive() {
        System.out.println("Car drives");
    }
}
```

Usage:

```java
Car c = new Car();

c.start();
c.drive();
```

---

# 👨‍👦 5. Parent and Child Class

Different terms are used for the same relationship.

### Parent class

Also called:

```text
Superclass
Base class
Parent class
```

Example:

```java
class Animal {
}
```

### Child class

Also called:

```text
Subclass
Derived class
Child class
```

Example:

```java
class Dog extends Animal {
}
```

Diagram:

```text
          Animal
      Superclass / Parent
              ▲
              │
            extends
              │
              │
             Dog
       Subclass / Child
```

---

# 🔗 6. IS-A Relationship

Inheritance normally represents an **IS-A** relationship.

Examples:

```text
Dog IS-A Animal
Car IS-A Vehicle
Manager IS-A Employee
Circle IS-A Shape
```

Example:

```java
class Animal {
}

class Dog extends Animal {
}
```

Conceptually:

```text
Dog
 ↓
IS-A
 ↓
Animal
```

### Interview rule

> If you cannot logically say **"Child IS-A Parent"**, inheritance may not be the right relationship.

---

# 📦 7. What Does a Child Class Inherit?

A child class can use accessible members of its superclass.

Example:

```java
class Animal {

    String name;

    void eat() {
        System.out.println("Eating");
    }
}

class Dog extends Animal {

    void bark() {
        System.out.println("Barking");
    }
}
```

`Dog` can access:

```text
name
eat()
```

and also has:

```text
bark()
```

Conceptually:

```text
Dog
│
├── inherited accessible state/behavior
│     ├── name
│     └── eat()
│
└── own behavior
      └── bark()
```

---

# 🚫 8. What is Not Inherited?

This is an important interview area.

### ❌ Constructors

Constructors are not inherited.

```java
class Parent {

    Parent() {
    }
}

class Child extends Parent {
}
```

The child does not inherit the parent's constructor.

However, the superclass constructor participates in child-object construction through constructor invocation/chaining.

---

### ❌ Private members are not directly accessible

```java
class Parent {

    private int x;
}
```

A subclass cannot directly access:

```java
x
```

through normal source-level access.

---

### ❌ Class-level restrictions

A `final` class cannot be extended.

```java
final class Parent {
}

class Child extends Parent { // ERROR
}
```

---

### ⚠️ Static members

Static members are associated with the class, not individual objects.

They can be accessed through a subclass name in some cases, but this should not be confused with instance-method overriding.

---

# 🌳 9. Types of Inheritance in Java

The commonly discussed inheritance structures are:

```text
1. Single
2. Multilevel
3. Hierarchical
4. Multiple
5. Hybrid
```

Java supports some directly through classes and can express others using interfaces.

---

# 1️⃣0️⃣ Single Inheritance

One child inherits from one parent.

```text
Animal
   ↑
   │
  Dog
```

Java:

```java
class Animal {

    void eat() {
        System.out.println("Eating");
    }
}

class Dog extends Animal {

    void bark() {
        System.out.println("Barking");
    }
}
```

This is supported by Java classes.

---

# 1️⃣1️⃣ Multilevel Inheritance

A class inherits from a class that itself inherits from another class.

```text
Animal
   ↑
 Mammal
   ↑
  Dog
```

Example:

```java
class Animal {

    void eat() {
        System.out.println("Eating");
    }
}

class Mammal extends Animal {

    void walk() {
        System.out.println("Walking");
    }
}

class Dog extends Mammal {

    void bark() {
        System.out.println("Barking");
    }
}
```

Now:

```java
Dog d = new Dog();

d.eat();
d.walk();
d.bark();
```

---

# 1️⃣2️⃣ Hierarchical Inheritance

Multiple child classes inherit from the same parent.

```text
             Animal
             /    \
            /      \
          Dog      Cat
```

Example:

```java
class Animal {

    void eat() {
        System.out.println("Eating");
    }
}

class Dog extends Animal {

    void bark() {
        System.out.println("Barking");
    }
}

class Cat extends Animal {

    void meow() {
        System.out.println("Meowing");
    }
}
```

---

# 1️⃣3️⃣ Multiple Inheritance

Multiple inheritance means a class has more than one direct superclass.

Conceptually:

```text
      A       B
       \     /
        \   /
          C
```

Java **does not allow a class to extend multiple classes**.

This is invalid:

```java
class C extends A, B {
}
```

❌ Compilation error.

However, Java supports multiple inheritance of **type through interfaces**.

Example:

```java
interface A {
    void show();
}

interface B {
    void display();
}

class C implements A, B {

    public void show() {
        System.out.println("A");
    }

    public void display() {
        System.out.println("B");
    }
}
```

So:

```text
Java Classes
→ No multiple class inheritance

Java Interfaces
→ A class can implement multiple interfaces
```

---

# 1️⃣4️⃣ Hybrid Inheritance

Hybrid inheritance combines multiple inheritance structures.

For example:

```text
          A
        /   \
       B     C
        \   /
          D
```

Java does not support such a structure through multiple class inheritance.

But combinations involving interfaces can express complex type relationships.

---

# 🚫 1️⃣5️⃣ Why Java Does Not Support Multiple Class Inheritance

The classic reason discussed in interviews is the **Diamond Problem**.

Suppose:

```text
        A
       / \
      B   C
       \ /
        D
```

Suppose `A` has:

```java
void show() {
    System.out.println("A");
}
```

`B` and `C` inherit it.

Now imagine both `B` and `C` provide their own `show()`.

Which implementation should `D` inherit?

```text
D.show()
   ?
 /   \
B     C
```

This can create ambiguity.

Java avoids this complexity by not allowing:

```java
class D extends B, C {
}
```

---

# 🧩 Interfaces and the Diamond Problem

Java interfaces can have `default` methods, so ambiguity can still arise.

Example:

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

Now:

```java
class C implements A, B {
}
```

The compiler requires `C` to resolve the conflict.

```java
class C implements A, B {

    @Override
    public void show() {
        A.super.show();
    }
}
```

So Java does not simply "avoid every possible diamond."

Instead:

> **Java prevents multiple inheritance of classes and provides explicit rules for resolving interface default-method conflicts.**

---

# 🏗️ 1️⃣6️⃣ Inheritance and Constructors

Constructors are **not inherited**.

But when a child object is created, the superclass constructor must participate in initialization.

Example:

```java
class Parent {

    Parent() {
        System.out.println("Parent constructor");
    }
}

class Child extends Parent {

    Child() {
        System.out.println("Child constructor");
    }
}
```

Now:

```java
Child c = new Child();
```

Output:

```text
Parent constructor
Child constructor
```

Why?

The superclass portion of the object must be initialized before the child constructor body completes.

---

# 🔗 1️⃣7️⃣ Constructor Chaining

Constructor chaining means one constructor invokes another constructor.

In inheritance, a child constructor can invoke a superclass constructor using:

```java
super();
```

Example:

```java
class Parent {

    Parent() {
        System.out.println("Parent");
    }
}

class Child extends Parent {

    Child() {
        super();
        System.out.println("Child");
    }
}
```

Execution:

```text
Child()
  ↓
super()
  ↓
Parent()
  ↓
Child constructor body
```

### Important

If you don't explicitly write a superclass constructor invocation, Java inserts an implicit `super()` when applicable.

If the superclass has no accessible no-argument constructor, the child must explicitly invoke an appropriate superclass constructor.

---

# 🔑 1️⃣8️⃣ `super` Keyword

`super` refers to the superclass portion/context of the current object.

It is commonly used for:

### 1. Calling superclass constructor

```java
super();
```

### 2. Calling superclass method

```java
super.display();
```

### 3. Accessing superclass field

```java
super.name;
```

Example:

```java
class Parent {

    String name = "Parent";

    void show() {
        System.out.println("Parent show");
    }
}

class Child extends Parent {

    String name = "Child";

    void display() {

        System.out.println(name);
        System.out.println(super.name);

        super.show();
    }
}
```

Output:

```text
Child
Parent
Parent show
```

---

# 🔄 1️⃣9️⃣ Method Overriding

Inheritance enables a child class to provide a specialized implementation of an inherited instance method.

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
        System.out.println("Dog barks");
    }
}
```

The child overrides `sound()`.

---

# 🧠 Why `@Override`?

```java
@Override
```

is an annotation that tells the compiler:

> "This method is intended to override a superclass method."

If you accidentally make a signature mistake, the compiler can catch it.

Example:

```java
@Override
void sounds() {
}
```

If `sounds()` does not override anything, the compiler reports an error.

---

# 🔄 2️⃣0️⃣ Inheritance and Polymorphism

Inheritance is closely related to runtime polymorphism.

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
        System.out.println("Dog barks");
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
Dog barks
```

Why?

Because the actual object is:

```text
Dog
```

even though the reference type is:

```text
Animal
```

This is **runtime method dispatch**.

---

# ⬆️ 2️⃣1️⃣ Upcasting

Upcasting means treating a subclass object as a superclass type.

Example:

```java
Dog d = new Dog();

Animal a = d;
```

or:

```java
Animal a = new Dog();
```

Conceptually:

```text
Dog Object
    │
    ▼
Animal Reference
```

This is safe because:

```text
Dog IS-A Animal
```

---

# ⬇️ 2️⃣2️⃣ Downcasting

Downcasting means converting a superclass reference to a subclass reference.

Example:

```java
Animal a = new Dog();

Dog d = (Dog) a;
```

This is allowed when the actual object is compatible with `Dog`.

But:

```java
Animal a = new Cat();

Dog d = (Dog) a;
```

causes:

```text
ClassCastException
```

at runtime.

### Important

The compile-time type of the reference does not guarantee that the actual object is the target subclass.

---

# 🔍 2️⃣3️⃣ `instanceof`

Before downcasting, you can check the object's runtime type compatibility.

```java
Animal a = new Dog();

if (a instanceof Dog) {

    Dog d = (Dog) a;
    d.bark();
}
```

Modern Java also supports pattern matching for `instanceof`:

```java
if (a instanceof Dog d) {
    d.bark();
}
```

This combines the type test and variable declaration.

---

# 🔐 2️⃣4️⃣ Access Modifiers and Inheritance

Inheritance interacts with access modifiers.

| Modifier        |            Accessible in Subclass? |
| --------------- | ---------------------------------: |
| `private`       |                         ❌ Directly |
| package-private |   ✅ If subclass is in same package |
| `protected`     | ✅ With special cross-package rules |
| `public`        |                                  ✅ |

Example:

```java
class Parent {

    private int a;
    int b;
    protected int c;
    public int d;
}
```

A subclass can directly access:

```text
b
c
d
```

subject to package/protected rules.

It cannot directly access:

```text
a
```

---

# 🛡️ 2️⃣5️⃣ `protected` and Inheritance

`protected` is especially relevant to inheritance.

Example:

```java
class Parent {

    protected int value = 10;
}

class Child extends Parent {

    void display() {
        System.out.println(value);
    }
}
```

This works.

But `protected` has nuanced rules when the subclass is in a different package.

A subclass in another package can access the protected member through inheritance, but not through an arbitrary superclass-typed reference.

This distinction is a common interview trap.

---

# 🚫 2️⃣6️⃣ `final` and Inheritance

`final` can restrict inheritance and overriding.

### Final class

```java
final class Animal {
}
```

Cannot be extended:

```java
class Dog extends Animal {
}
```

❌ Error.

---

### Final method

```java
class Parent {

    final void show() {
        System.out.println("Parent");
    }
}
```

Cannot be overridden:

```java
class Child extends Parent {

    void show() {
        // ERROR
    }
}
```

---

# ⚡ 2️⃣7️⃣ Static Members and Inheritance

Static members belong to the class rather than individual objects.

Example:

```java
class Parent {

    static void show() {
        System.out.println("Parent");
    }
}

class Child extends Parent {
}
```

You may write:

```java
Child.show();
```

because the static member is accessible through the subclass name.

But static methods are **not overridden** like instance methods.

They are **hidden** when a subclass declares a static method with the same signature.

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

prints:

```text
Parent
```

because static method selection is based on the reference/class context, not runtime dynamic dispatch.

---

# 🔒 2️⃣8️⃣ Private Members and Inheritance

Consider:

```java
class Parent {

    private int value = 10;
}

class Child extends Parent {
}
```

The child does not have direct source-level access to:

```java
value
```

However, the parent object state still exists as part of a child object.

A subclass can access such state only through accessible superclass behavior, such as:

```java
protected int getValue() {
    return value;
}
```

or:

```java
public int getValue() {
    return value;
}
```

---

# 🌳 2️⃣9️⃣ Object Class and Inheritance

Every Java class ultimately has `Object` as its superclass, except `Object` itself.

Example:

```text
Object
  ↑
Animal
  ↑
Dog
```

So:

```java
Dog d = new Dog();
```

also gives access to methods inherited through `Object`, such as:

```text
toString()
equals()
hashCode()
getClass()
```

This is why every ordinary Java object has these fundamental methods.

---

# 🆚 3️⃣0️⃣ Inheritance vs Composition

This is one of the most important design questions.

## Inheritance

Represents:

```text
IS-A
```

Example:

```java
class Dog extends Animal {
}
```

---

## Composition

Represents:

```text
HAS-A
```

Example:

```java
class Engine {
}

class Car {

    private Engine engine;
}
```

Conceptually:

```text
Dog
 │
 └── IS-A → Animal


Car
 │
 └── HAS-A → Engine
```

### Why composition is often preferred

Inheritance creates stronger coupling between parent and child.

Composition allows components to be replaced or combined more flexibly.

But this does **not** mean inheritance is bad.

Use inheritance when the subtype genuinely satisfies the superclass's abstraction.

---

# 🚀 3️⃣1️⃣ Advantages of Inheritance

## 🔄 1. Code Reuse

Common behavior can be placed in a superclass.

---

## 🧩 2. Specialization

Child classes can add specialized behavior.

```text
Employee
   ↓
Developer
```

---

## 🔄 3. Runtime Polymorphism

Inheritance enables superclass references to work with subclass objects.

---

## 🧱 4. Extensibility

New subclasses can extend existing abstractions.

---

## 🧠 5. Logical Hierarchy

Inheritance can model genuine IS-A relationships.

---

# ⚠️ 3️⃣2️⃣ Disadvantages of Inheritance

## 1. Tight Coupling

A child depends on its superclass design.

---

## 2. Fragile Base Class Problem

Changes to a superclass can unexpectedly affect subclasses.

---

## 3. Deep Hierarchies

Very deep inheritance trees can become difficult to understand.

```text
A
 ↓
B
 ↓
C
 ↓
D
 ↓
E
 ↓
F
```

---

## 4. Reduced Flexibility

A class can have only one direct superclass.

---

## 5. Misuse of IS-A

Using inheritance only for code reuse can produce poor designs.

Example:

```text
Car extends Engine
```

This is logically incorrect because:

```text
Car IS-NOT-A Engine
```

Instead:

```text
Car HAS-A Engine
```

---

# ⚠️ 3️⃣3️⃣ Common Mistakes

## ❌ Mistake 1: Constructors are inherited

False.

Constructors are not inherited.

---

## ❌ Mistake 2: Private fields are inherited and directly accessible

False.

Private members are not directly accessible in subclasses.

---

## ❌ Mistake 3: Java supports multiple class inheritance

False.

```java
class C extends A, B
```

is invalid.

---

## ❌ Mistake 4: Static methods are overridden

False.

Static methods are hidden, not dynamically overridden.

---

## ❌ Mistake 5: `super` means a separate parent object

Not exactly.

`super` provides access to superclass members in the context of the current object.

---

## ❌ Mistake 6: Upcasting creates a new object

No.

```java
Animal a = new Dog();
```

creates one `Dog` object.

The `Animal` variable simply refers to it through a superclass type.

---

## ❌ Mistake 7: Downcasting changes the object

No.

```java
Dog d = (Dog) a;
```

does not transform an `Animal` object into a `Dog`.

It tells Java to treat the existing reference as a `Dog` when the actual object is compatible.

---

## ❌ Mistake 8: Every inheritance relationship is good design

No.

Inheritance should represent a meaningful substitutable IS-A relationship.

---

# 💼 3️⃣4️⃣ Interview Questions

## 🟢 Basic

### 1. What is inheritance?

Inheritance is a mechanism through which a class derives from another class and can reuse and specialize accessible behavior.

---

### 2. What keyword is used for class inheritance?

```java
extends
```

---

### 3. What is a superclass?

The parent/base class from which another class derives.

---

### 4. What is a subclass?

A class that extends another class.

---

### 5. What is an IS-A relationship?

A relationship represented by inheritance.

```text
Dog IS-A Animal
```

---

### 6. What are the types of inheritance?

Commonly:

```text
Single
Multilevel
Hierarchical
Multiple
Hybrid
```

Java supports single, multilevel, and hierarchical class inheritance directly.

Multiple class inheritance is not supported.

---

### 7. Does Java support multiple inheritance?

Not through classes.

A class can implement multiple interfaces.

---

### 8. Are constructors inherited?

No.

---

### 9. Can a subclass access private members of the parent directly?

No.

---

### 10. What is the `extends` keyword?

It declares class inheritance.

---

# 🟡 Intermediate

### 11. What is method overriding?

A subclass provides a compatible implementation of an inherited instance method.

---

### 12. What is constructor chaining?

The process of invoking constructors along an inheritance chain.

---

### 13. What is `super`?

A keyword used to access superclass members and invoke superclass constructors.

---

### 14. What is upcasting?

Treating a subclass object as a superclass type.

```java
Animal a = new Dog();
```

---

### 15. What is downcasting?

Explicitly converting a superclass reference to a subclass reference when the actual object is compatible.

```java
Dog d = (Dog) a;
```

---

### 16. What happens if downcasting is invalid?

A `ClassCastException` occurs at runtime.

---

### 17. What is `instanceof`?

An operator used to test whether an object is compatible with a specified reference type.

---

### 18. Why is multiple class inheritance not supported?

Primarily to avoid ambiguity and complexity such as the classic diamond problem.

---

### 19. Can a final class be inherited?

No.

---

### 20. Can a final method be overridden?

No.

---

# 🔴 Advanced / Tricky

### 21. Are private members inherited?

Interview discussions often simplify this as "private members are not inherited."

The precise point is:

> A subclass cannot directly access a superclass's private members. The private state is still part of the superclass portion of the object, but it is encapsulated within the superclass.

---

### 22. Are static methods inherited?

Static methods can be accessible through a subclass depending on access, but they are class members and are not overridden through runtime polymorphism.

If a subclass declares a same-signature static method, it hides the superclass method.

---

### 23. Why does this print `Parent`?

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

Because static method invocation is resolved using the reference/class context rather than dynamic dispatch.

---

### 24. Why does this print `Dog`?

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

Because `sound()` is an overridden instance method and runtime dispatch selects the implementation belonging to the actual object.

---

### 25. Does `Animal a = new Dog();` create two objects?

No.

It creates one `Dog` object.

The `Animal` reference points to that object.

---

### 26. Can a child class call a parent constructor?

Yes.

Using:

```java
super();
```

or:

```java
super(arguments);
```

---

### 27. Can `super()` and `this()` both be written in the same constructor?

No.

Both invoke another constructor and must be the first statement.

A constructor can explicitly invoke either:

```java
this(...)
```

or:

```java
super(...)
```

but not both.

---

### 28. Can a constructor be overridden?

No.

Constructors are not inherited and therefore cannot be overridden.

---

### 29. Why is composition often preferred over inheritance?

Because composition generally produces looser coupling and more flexible designs when the relationship is not genuinely IS-A.

---

### 30. Can an abstract class be inherited?

Yes.

A subclass can extend an abstract class.

It must implement inherited abstract methods unless the subclass is also abstract.

---

### 31. Can an interface extend another interface?

Yes.

```java
interface A {
}

interface B extends A {
}
```

An interface can also extend multiple interfaces:

```java
interface C extends A, B {
}
```

---

### 32. Can a class extend a class and implement interfaces?

Yes.

```java
class Dog extends Animal implements Runnable, Serializable {
}
```

This is a common Java design pattern.

---

# 🔥 3️⃣5️⃣ Top 10 Interview Questions

## 1️⃣ What is inheritance?

> A mechanism where a subclass derives from a superclass and can reuse and specialize accessible behavior.

---

## 2️⃣ What is an IS-A relationship?

```text
Dog IS-A Animal
Car IS-A Vehicle
```

It commonly represents inheritance.

---

## 3️⃣ Which keyword is used for inheritance?

```java
extends
```

---

## 4️⃣ Does Java support multiple inheritance?

```text
Multiple classes → ❌
Multiple interfaces → ✅
```

---

## 5️⃣ Why doesn't Java support multiple class inheritance?

To avoid ambiguity and complexity, particularly the classic diamond problem.

---

## 6️⃣ Are constructors inherited?

❌ No.

They participate in constructor chaining but are not inherited.

---

## 7️⃣ What is `super`?

Used to:

```text
Call superclass constructor
Access superclass method
Access superclass field
```

---

## 8️⃣ What is upcasting?

```java
Animal a = new Dog();
```

A subclass object is referenced using a superclass type.

---

## 9️⃣ What is downcasting?

```java
Dog d = (Dog) a;
```

Converting a superclass reference to a compatible subclass reference.

---

## 🔟 How is inheritance related to polymorphism?

Inheritance allows a superclass reference to refer to a subclass object, enabling runtime method dispatch for overridden instance methods.

---

# 🧠 3️⃣6️⃣ Quick Revision

```text
                 INHERITANCE
                      │
                      ▼
                 Parent Class
                      ▲
                      │
                    extends
                      │
                      ▼
                 Child Class
```

### Relationship

```text
Dog IS-A Animal
```

### Main types

```text
Single
   A
   ↑
   B

Multilevel
   A
   ↑
   B
   ↑
   C

Hierarchical
      A
     / \
    B   C
```

### Java class inheritance

```text
One direct superclass
        +
Multiple interfaces possible
```

---

# 🔥 Constructor Flow

```text
new Child()
     ↓
Child object creation
     ↓
Superclass constructor
     ↓
Child constructor
```

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

Output:

```text
Parent
Child
```

---

# 🔄 Polymorphism Connection

```java
Animal a = new Dog();

a.sound();
```

Think:

```text
Reference Type
     ↓
  Animal
     │
     │ refers to
     ▼
Actual Object
     ↓
    Dog
     │
     ▼
Runtime dispatch
     │
     ▼
Dog.sound()
```

---

# ⬆️ Upcasting vs ⬇️ Downcasting

```text
UPCASTING
Dog → Animal
Safe / implicit

DOWNCASTING
Animal → Dog
Explicit
May throw ClassCastException
```

---

# ⚡ 3️⃣7️⃣ 30-Second Interview Answer

> **"Inheritance is an OOP mechanism where a subclass derives from a superclass using the `extends` keyword. It allows the subclass to reuse and specialize accessible behavior of the superclass and represents an IS-A relationship, such as Dog IS-A Animal. Java supports single, multilevel, and hierarchical class inheritance, but it does not support multiple inheritance through classes. Java does support implementing multiple interfaces. Constructors are not inherited, but superclass constructors participate in constructor chaining. Inheritance also enables runtime polymorphism through method overriding, where a superclass reference can refer to a subclass object and the overridden method is selected at runtime."**

---

# 🏆 3️⃣8️⃣ Memory Trick

Remember inheritance using:

```text
             🧬 INHERITANCE

             IS-A
              ↓
           extends
              ↓
        Parent → Child
              ↓
         Reuse + Extend
              ↓
       Method Overriding
              ↓
      Runtime Polymorphism
```

### Five keywords/concepts to remember

```text
extends
super
@Override
instanceof
final
```

### One-line master memory

> 🧠 **Inheritance = IS-A + reuse + specialization + polymorphism.**

---

# 🎯 Final Takeaway

Don't learn inheritance as simply:

```text
Child gets Parent's code
```

The deeper idea is:

```text
                 Parent
                   │
          Common abstraction
                   │
              extends
                   │
                 Child
                   │
       ┌───────────┴───────────┐
       ▼                       ▼
 Reuse inherited          Add specialized
   behavior                  behavior
       │                       │
       └───────────┬───────────┘
                   ▼
             Polymorphism
```

And remember the most important distinction:

```text
Inheritance
    ↓
IS-A

Composition
    ↓
HAS-A
```

> 🔥 **Use inheritance when the child genuinely is a specialized form of the parent—not merely because you want to reuse some code.**
