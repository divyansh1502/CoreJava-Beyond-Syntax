# ☕ Java OOP — Polymorphism

> **Polymorphism = One interface/reference, multiple forms of behavior.**

Polymorphism is one of the four major pillars of Object-Oriented Programming:

```text
Encapsulation
Inheritance
Polymorphism
Abstraction
```

---

# 1. What is Polymorphism?

The word **Polymorphism** comes from two Greek words:

```text
Poly  → Many
Morph → Forms
```

So, polymorphism literally means:

> **One thing having many forms.**

In Java, polymorphism allows the **same method call or reference** to behave differently depending on the situation.

Example:

```java
Animal a;

a = new Dog();
a.sound();

a = new Cat();
a.sound();
```

The same:

```java
a.sound();
```

can produce different behavior:

```text
Dog  → Bark
Cat  → Meow
```

This is polymorphism.

---

# 2. Why Do We Need Polymorphism?

Without polymorphism, we may need to write separate code for every class.

Suppose we have:

```java
class Dog {
    void sound() {
        System.out.println("Bark");
    }
}

class Cat {
    void sound() {
        System.out.println("Meow");
    }
}
```

Without polymorphism:

```java
Dog d = new Dog();
d.sound();

Cat c = new Cat();
c.sound();
```

The calling code becomes tightly coupled to concrete classes.

With polymorphism:

```java
Animal a;

a = new Dog();
a.sound();

a = new Cat();
a.sound();
```

Now the calling code works with the common parent type:

```java
Animal
```

while the actual behavior comes from the object.

This gives us:

```text
Loose coupling
Extensibility
Flexible code
Runtime method selection
Better maintainability
```

---

# 3. Real-Life Example

Imagine a person using a payment system.

The user only knows:

```text
Pay()
```

But payment can happen through:

```text
UPI
Credit Card
Debit Card
Net Banking
```

The user does not need to know the internal implementation of every payment method.

Conceptually:

```java
Payment p;

p = new UPI();
p.pay();

p = new CreditCard();
p.pay();

p = new DebitCard();
p.pay();
```

Same method:

```java
pay()
```

Different behavior.

That is polymorphism.

---

# 4. Types of Polymorphism in Java

Java mainly supports two types:

```text
                 Polymorphism
                      |
             +--------+--------+
             |                 |
       Compile-Time        Runtime
       Polymorphism        Polymorphism
             |                 |
       Method Overloading  Method Overriding
```

## Compile-Time Polymorphism

Achieved using:

```text
Method Overloading
```

The compiler decides which method should be called.

---

## Runtime Polymorphism

Achieved using:

```text
Method Overriding
```

The JVM determines which overridden method should execute at runtime.

---

# 5. Compile-Time Polymorphism

Compile-time polymorphism means the method call is resolved during compilation.

The most common way to achieve it is:

```text
Method Overloading
```

Example:

```java
class Calculator {

    int add(int a, int b) {
        return a + b;
    }

    int add(int a, int b, int c) {
        return a + b + c;
    }

    double add(double a, double b) {
        return a + b;
    }
}
```

Now:

```java
Calculator c = new Calculator();

c.add(10, 20);

c.add(10, 20, 30);

c.add(10.5, 20.5);
```

The compiler determines which `add()` method matches the arguments.

---

# 6. Why Is It Called Compile-Time Polymorphism?

Consider:

```java
c.add(10, 20);
```

The compiler sees:

```text
add(int, int)
```

So it selects:

```java
int add(int a, int b)
```

Similarly:

```java
c.add(10, 20, 30);
```

matches:

```java
int add(int a, int b, int c)
```

The method selection is determined using the **compile-time type of the reference and the argument types**.

Therefore:

```text
Method Overloading
        ↓
Compile-Time Polymorphism
        ↓
Early / Static Binding
```

---

# 7. Runtime Polymorphism

Runtime polymorphism occurs when a child class overrides a method of its parent class.

Example:

```java
class Animal {

    void sound() {
        System.out.println("Animal makes sound");
    }
}

class Dog extends Animal {

    @Override
    void sound() {
        System.out.println("Dog barks");
    }
}

class Cat extends Animal {

    @Override
    void sound() {
        System.out.println("Cat meows");
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
Dog barks
Cat meows
```

The reference type is:

```java
Animal
```

but the actual object changes:

```text
Animal reference
       |
       +----> Dog object
       |
       +----> Cat object
```

The JVM executes the overridden method belonging to the actual object.

---

# 8. The Most Important Rule

For runtime polymorphism:

> **Reference type decides what members are accessible, while object type decides which overridden instance method executes.**

Example:

```java
Animal a = new Dog();
```

Here:

```text
Reference type → Animal
Object type    → Dog
```

Suppose:

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
        System.out.println("Dog");
    }

    void bark() {
        System.out.println("Bark");
    }
}
```

Then:

```java
Animal a = new Dog();

a.sound();  // Dog
a.eat();    // Animal
a.bark();   // Compile-time error
```

Why?

Because:

```text
Reference type = Animal
```

So the compiler only allows members available through `Animal`.

But for an overridden method:

```java
a.sound();
```

the actual object is:

```text
Dog
```

Therefore:

```text
Dog.sound()
```

executes.

---

# 9. Upcasting and Runtime Polymorphism

Runtime polymorphism commonly uses **upcasting**.

```java
Dog d = new Dog();

Animal a = d;
```

Or directly:

```java
Animal a = new Dog();
```

This is:

```text
Child object
     ↓
Parent reference
```

Example:

```java
Animal a = new Dog();
```

This is valid because:

```text
Dog IS-A Animal
```

Now:

```java
a.sound();
```

can execute the Dog implementation.

This is one of the most important uses of inheritance + polymorphism.

---

# 10. Polymorphism Through Parent Reference

Consider:

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

We can write:

```java
Animal a1 = new Dog();
Animal a2 = new Cat();

a1.sound();
a2.sound();
```

Output:

```text
Bark
Meow
```

This allows us to write generic code:

```java
static void makeSound(Animal animal) {
    animal.sound();
}
```

Now:

```java
makeSound(new Dog());
makeSound(new Cat());
```

Output:

```text
Bark
Meow
```

The method does not care about the exact child class.

It only depends on:

```java
Animal
```

This is a major reason polymorphism is powerful in real applications.

---

# 11. Polymorphism with Arrays

Polymorphism becomes especially useful when handling multiple child objects together.

```java
Animal[] animals = {
    new Dog(),
    new Cat(),
    new Dog()
};

for (Animal animal : animals) {
    animal.sound();
}
```

Output:

```text
Bark
Meow
Bark
```

The array type is:

```java
Animal[]
```

but it contains different child objects.

This is a very common real-world pattern.

---

# 12. Polymorphism with Collections

The same idea works with collections.

```java
ArrayList<Animal> animals = new ArrayList<>();

animals.add(new Dog());
animals.add(new Cat());
animals.add(new Dog());

for (Animal animal : animals) {
    animal.sound();
}
```

Here:

```text
List type → Animal
Objects   → Dog, Cat, Dog
```

This gives us a flexible design.

---

# 13. Method Overloading vs Method Overriding

| Feature               | Overloading                  | Overriding                 |
| --------------------- | ---------------------------- | -------------------------- |
| Polymorphism          | Compile-time                 | Runtime                    |
| Usually occurs in     | Same class                   | Parent-child classes       |
| Method name           | Same                         | Same                       |
| Parameters            | Must differ                  | Must be same               |
| Return type           | Cannot distinguish overloads | Same/covariant allowed     |
| Binding               | Static/Early                 | Dynamic/Late               |
| Inheritance required? | No                           | Yes                        |
| Decision              | Compiler                     | Runtime/JVM                |
| Main purpose          | Multiple ways to call        | Specialized child behavior |

---

# 14. Static Binding

Static binding means the method call is resolved at compile time.

Method overloading is an example.

```java
class Demo {

    void show(int x) {
        System.out.println("int");
    }

    void show(double x) {
        System.out.println("double");
    }
}
```

```java
Demo d = new Demo();

d.show(10);
```

Compiler selects:

```java
show(int)
```

before execution.

---

# 15. Dynamic Binding

Dynamic binding means the overridden method is selected based on the actual object at runtime.

```java
Animal a = new Dog();

a.sound();
```

At compile time:

```text
a → Animal reference
```

At runtime:

```text
actual object → Dog
```

Therefore:

```text
Dog.sound()
```

executes.

---

# 16. Dynamic Method Dispatch

The mechanism through which Java selects an overridden method at runtime is commonly called:

> **Dynamic Method Dispatch**

Example:

```java
Animal animal;

animal = new Dog();
animal.sound();

animal = new Cat();
animal.sound();
```

The JVM dynamically dispatches the call to the appropriate overridden method.

---

# 17. Important: Fields Do NOT Behave Like Overridden Methods

This is a common interview trap.

Fields are not dynamically dispatched like overridden instance methods.

Example:

```java
class Parent {
    int value = 10;
}

class Child extends Parent {
    int value = 20;
}
```

Now:

```java
Parent p = new Child();

System.out.println(p.value);
```

Output:

```text
10
```

The field is selected based on the **reference type**.

But methods behave differently:

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

```java
Parent p = new Child();

p.show();
```

Output:

```text
Child
```

Remember:

```text
Fields  → Reference type
Methods → Actual object for overridden instance methods
```

---

# 18. Static Methods and Polymorphism

Static methods are associated with the class, not the object.

They are **hidden**, not overridden.

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

Because static method selection depends on the reference/class context.

Therefore:

```text
Instance method → Overriding + Runtime Polymorphism
Static method   → Method hiding
```

---

# 19. Private Methods and Polymorphism

Private methods are not inherited by subclasses in the normal overriding sense.

Therefore, they cannot be overridden.

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

They do not participate in runtime overriding.

---

# 20. Final Methods and Polymorphism

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

Because:

```text
final method
     ↓
cannot be overridden
     ↓
no runtime polymorphic replacement
```

---

# 21. Constructors and Polymorphism

Constructors are not inherited and cannot be overridden.

Therefore:

```text
Constructors → No method overriding
```

However, constructor calls participate in inheritance through constructor chaining.

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

```java
Child c = new Child();
```

Output:

```text
Parent constructor
Child constructor
```

This is constructor chaining, not polymorphism.

---

# 22. Can We Override a Method with a More Specific Return Type?

Yes.

This is called a **covariant return type**.

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

The child method returns a subtype of the parent's return type.

```text
Animal
   ↑
  Dog
```

This is allowed.

---

# 23. Polymorphism and Casting

Suppose:

```java
Animal a = new Dog();
```

This is upcasting.

If we need Dog-specific functionality:

```java
Dog d = (Dog) a;
```

This is downcasting.

Now:

```java
d.bark();
```

can be called.

But incorrect downcasting can cause:

```text
ClassCastException
```

Example:

```java
Animal a = new Cat();

Dog d = (Dog) a;
```

This compiles but fails at runtime because the actual object is a `Cat`.

---

# 24. instanceof with Polymorphism

Before downcasting, we can check the actual object:

```java
if (a instanceof Dog) {
    Dog d = (Dog) a;
    d.bark();
}
```

Modern Java also supports pattern matching:

```java
if (a instanceof Dog d) {
    d.bark();
}
```

This makes type checking and casting more concise.

---

# 25. Polymorphism Does Not Mean "Everything Is Dynamic"

A common misconception is:

> "If Java has polymorphism, every method call is decided at runtime."

Incorrect.

Java has both:

```text
Compile-time binding
Runtime binding
```

For example:

```text
Overloading  → Compile-time
Overriding   → Runtime
Static       → Compile-time
Private      → Compile-time
Final        → Compile-time constraints / no overriding
```

---

# 26. Polymorphism and Abstraction

Polymorphism works especially well with abstraction.

Example:

```java
abstract class Payment {

    abstract void pay();
}
```

Implementations:

```java
class UPI extends Payment {

    @Override
    void pay() {
        System.out.println("Paid using UPI");
    }
}

class Card extends Payment {

    @Override
    void pay() {
        System.out.println("Paid using Card");
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

The parent type defines the common contract:

```text
Payment
```

while child classes provide different implementations.

This combination is extremely important in software design.

---

# 27. Real-World Backend Example

Imagine a Spring Boot application supporting multiple notification types:

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
        System.out.println("Sending Email");
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

```java
class WhatsAppNotification implements Notification {

    @Override
    public void send() {
        System.out.println("Sending WhatsApp message");
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

notification = new WhatsAppNotification();
notification.send();
```

The business code can depend on:

```java
Notification
```

rather than every concrete implementation.

This is one of the foundations of extensible backend architecture.

---

# 28. Main Advantages of Polymorphism

### 1. Flexibility

The same code can work with different object types.

### 2. Loose Coupling

Code can depend on parent classes/interfaces instead of concrete implementations.

### 3. Extensibility

New child classes can often be introduced without changing existing client code.

### 4. Maintainability

Behavior can be separated into specialized classes.

### 5. Reusability

Generic methods can operate on parent types.

Example:

```java
void processPayment(Payment payment) {
    payment.pay();
}
```

The method can accept:

```text
UPI
Card
NetBanking
Wallet
```

as long as they are `Payment` types.

---

# 29. Common Interview Traps

## Trap 1

```java
Animal a = new Dog();
```

What is:

```text
Reference type? → Animal
Object type?    → Dog
```

---

## Trap 2

```java
a.sound();
```

If `sound()` is overridden:

```text
Actual object determines implementation.
```

---

## Trap 3

Fields are not polymorphic like overridden methods.

```java
Parent p = new Child();

p.x;
```

Field selection depends on the reference type.

---

## Trap 4

Static methods are not overridden.

They are:

```text
Hidden
```

---

## Trap 5

Private methods cannot be overridden.

---

## Trap 6

Final methods cannot be overridden.

---

## Trap 7

Constructors cannot be overridden.

---

## Trap 8

Overloading is resolved at compile time.

Overriding is resolved at runtime.

---

# 30. Polymorphism vs Inheritance

These concepts are related but not identical.

```text
Inheritance
    ↓
Creates IS-A relationship
    ↓
Allows child classes to inherit/extend behavior
```

While:

```text
Polymorphism
    ↓
Allows common parent/interface references
    ↓
To work with different implementations
```

Inheritance is one common mechanism that enables runtime polymorphism, but polymorphism can also be achieved through interfaces.

---

# 31. Polymorphism vs Abstraction

| Polymorphism                                  | Abstraction                                  |
| --------------------------------------------- | -------------------------------------------- |
| Multiple forms of behavior                    | Hides implementation details                 |
| Focuses on behavior selection                 | Focuses on exposing essential features       |
| Achieved through overloading/overriding       | Achieved through abstract classes/interfaces |
| Supports flexibility                          | Supports design/contracts                    |
| Example: `animal.sound()` behaves differently | Example: `Animal` defines `sound()` contract |

They often work together.

---

# 32. The Core Mental Model

Remember this:

```text
                Polymorphism
                     |
          +----------+----------+
          |                     |
     Compile-Time          Runtime
          |                     |
    Method Overloading    Method Overriding
          |                     |
      Early Binding        Dynamic Binding
                                |
                         Actual Object
                         decides method
```

And:

```text
Parent reference
       +
Child object
       ↓
Runtime Polymorphism
```

Example:

```java
Animal a = new Dog();
a.sound();
```

Think:

```text
Reference → Animal
Object    → Dog
Method    → Dog.sound()
```

---

# 33. 30-Second Interview Answer

> **Polymorphism is an OOP concept where the same interface, reference, or method call can represent different forms of behavior. In Java, polymorphism is mainly of two types: compile-time polymorphism through method overloading and runtime polymorphism through method overriding. In runtime polymorphism, a parent reference can refer to a child object, and the overridden instance method is selected based on the actual object at runtime.**

Example:

```java
Animal a = new Dog();
a.sound();
```

Here:

```text
Animal → reference type
Dog    → actual object
Dog.sound() → executed at runtime
```

---

# 34. Top Interview Questions

### Basic

1. What is polymorphism?
2. Why is polymorphism important?
3. What are the types of polymorphism in Java?
4. What is compile-time polymorphism?
5. What is runtime polymorphism?
6. How is method overloading related to polymorphism?
7. How is method overriding related to polymorphism?

### Intermediate

8. What is dynamic method dispatch?
9. What is static binding?
10. What is dynamic binding?
11. What happens when a parent reference points to a child object?
12. What determines method accessibility?
13. What determines which overridden method executes?
14. Can static methods be overridden?
15. Can private methods be overridden?
16. Can final methods be overridden?
17. Can constructors be overridden?
18. Are fields polymorphic?

### Advanced

19. What is covariant return type?
20. What is the difference between method hiding and overriding?
21. Why does `Parent p = new Child()` enable runtime polymorphism?
22. How does downcasting work with polymorphism?
23. What happens if an invalid downcast is performed?
24. How does `instanceof` help with polymorphism?
25. How does polymorphism reduce coupling?
26. How does polymorphism support extensibility?
27. How do interfaces enable runtime polymorphism?
28. How are overloaded methods selected?
29. How are overridden methods selected?
30. Explain the difference between reference type and object type.

---

# 35. Quick Revision Cheat Sheet

```text
POLYMORPHISM
│
├── Meaning
│   └── One thing → Multiple forms
│
├── Compile-Time
│   └── Method Overloading
│       └── Static/Early Binding
│
└── Runtime
    └── Method Overriding
        └── Dynamic/Late Binding
```

```text
Animal a = new Dog();

Reference Type → Animal
Object Type    → Dog

Accessible members
        ↓
Reference type

Overridden instance method
        ↓
Actual object
```

```text
Fields          → Not dynamically dispatched
Static methods  → Hidden
Private methods → Not overridden
Final methods   → Cannot be overridden
Constructors    → Cannot be overridden
Instance methods → Runtime polymorphism possible
```

### ⭐ One line to remember

> **Reference decides what you can access; the actual object decides which overridden instance method runs.**
