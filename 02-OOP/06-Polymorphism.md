# ☕ Java OOP — Polymorphism

> **Polymorphism = One interface/reference, multiple forms of behavior.**

## 📚 Table of Contents

- [1. What is Polymorphism?](#1-what-is-polymorphism)
- [2. Why Do We Need Polymorphism?](#2-why-do-we-need-polymorphism)
- [3. Real-Life Example](#3-real-life-example)
- [4. Types of Polymorphism in Java](#4-types-of-polymorphism-in-java)
- [5. Compile-Time Polymorphism](#5-compile-time-polymorphism)
- [6. Why Is It Called Compile-Time Polymorphism?](#6-why-is-it-called-compile-time-polymorphism)
- [7. Runtime Polymorphism](#7-runtime-polymorphism)
- [8. The Most Important Rule](#8-the-most-important-rule)
- [9. Upcasting and Runtime Polymorphism](#9-upcasting-and-runtime-polymorphism)
- [10. Polymorphism Through Parent Reference](#10-polymorphism-through-parent-reference)
- [11. Polymorphism with Arrays](#11-polymorphism-with-arrays)
- [12. Polymorphism with Collections](#12-polymorphism-with-collections)
- [13. Method Overloading vs Method Overriding](#13-method-overloading-vs-method-overriding)
- [14. Static Binding](#14-static-binding)
- [15. Dynamic Binding](#15-dynamic-binding)
- [16. Dynamic Method Dispatch](#16-dynamic-method-dispatch)
- [17. Fields Do NOT Behave Like Overridden Methods](#17-fields-do-not-behave-like-overridden-methods)
- [18. Static Methods and Polymorphism](#18-static-methods-and-polymorphism)
- [19. Private Methods and Polymorphism](#19-private-methods-and-polymorphism)
- [20. Final Methods and Polymorphism](#20-final-methods-and-polymorphism)
- [21. Constructors and Polymorphism](#21-constructors-and-polymorphism)
- [22. Covariant Return Type](#22-covariant-return-type)
- [23. Polymorphism and Casting](#23-polymorphism-and-casting)
- [24. instanceof with Polymorphism](#24-instanceof-with-polymorphism)
- [25. Polymorphism Does Not Mean Everything Is Dynamic](#25-polymorphism-does-not-mean-everything-is-dynamic)
- [26. Polymorphism and Abstraction](#26-polymorphism-and-abstraction)
- [27. Real-World Backend Example](#27-real-world-backend-example)
- [28. Main Advantages of Polymorphism](#28-main-advantages-of-polymorphism)
- [29. Common Interview Traps](#29-common-interview-traps)
- [30. Polymorphism vs Inheritance](#30-polymorphism-vs-inheritance)
- [31. Polymorphism vs Abstraction](#31-polymorphism-vs-abstraction)
- [32. Core Mental Model](#32-core-mental-model)
- [33. 30-Second Interview Answer](#33-30-second-interview-answer)
- [34. Top Interview Questions](#34-top-interview-questions)
- [35. DSA & Problem-Solving Patterns](#35-dsa--problem-solving-patterns)
- [36. DSA Practice Questions](#36-dsa-practice-questions)
- [37. Quick Revision Cheat Sheet](#37-quick-revision-cheat-sheet)

---

# 1. What is Polymorphism?

Polymorphism is one of the four major pillars of Object-Oriented Programming:

    Encapsulation
    Inheritance
    Polymorphism
    Abstraction

The word **Polymorphism** comes from two Greek words:

    Poly  → Many
    Morph → Forms

So, polymorphism literally means:

> **One thing having many forms.**

In Java, polymorphism allows the same method call or reference to represent different forms of behavior.

Example:

    Animal animal;

    animal = new Dog();
    animal.sound();

    animal = new Cat();
    animal.sound();

The same method call:

    animal.sound();

can produce different behavior:

    Dog → Bark
    Cat → Meow

This is polymorphism.

---

# 2. Why Do We Need Polymorphism?

Without polymorphism, code can become tightly coupled to concrete classes.

Example:

    Dog dog = new Dog();
    dog.sound();

    Cat cat = new Cat();
    cat.sound();

With polymorphism:

    Animal animal;

    animal = new Dog();
    animal.sound();

    animal = new Cat();
    animal.sound();

Now the calling code depends on:

    Animal

instead of directly depending on every concrete implementation.

This provides:

    Flexibility
    Loose coupling
    Extensibility
    Maintainability
    Reusability

---

# 3. Real-Life Example

Imagine a payment system.

A user wants to perform:

    pay()

The payment can happen through:

    UPI
    Credit Card
    Debit Card
    Net Banking
    Wallet

Conceptually:

    Payment payment;

    payment = new UPI();
    payment.pay();

    payment = new CreditCard();
    payment.pay();

    payment = new DebitCard();
    payment.pay();

Same method:

    pay()

Different implementation.

That is polymorphism.

---

# 4. Types of Polymorphism in Java

Java mainly supports two commonly discussed types:

    Polymorphism
         |
         +-------------------+
         |                   |
    Compile-Time          Runtime
    Polymorphism          Polymorphism
         |                   |
    Method Overloading   Method Overriding

## Compile-Time Polymorphism

Usually achieved through:

    Method Overloading

The compiler selects the appropriate overloaded method.

## Runtime Polymorphism

Achieved through:

    Method Overriding

The overridden instance method is selected based on the actual object at runtime.

---

# 5. Compile-Time Polymorphism

Compile-time polymorphism means method selection is performed during compilation.

The common example is **method overloading**.

Example:

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

Usage:

    Calculator calculator = new Calculator();

    calculator.add(10, 20);
    calculator.add(10, 20, 30);
    calculator.add(10.5, 20.5);

The compiler determines which overloaded method matches the arguments.

---

# 6. Why Is It Called Compile-Time Polymorphism?

Consider:

    calculator.add(10, 20);

The compiler identifies:

    add(int, int)

Therefore:

    int add(int a, int b)

is selected.

Similarly:

    calculator.add(10, 20, 30);

matches:

    int add(int a, int b, int c)

So:

    Method Overloading
            ↓
    Compile-Time Polymorphism
            ↓
    Early / Static Binding

---

# 7. Runtime Polymorphism

Runtime polymorphism occurs when a child class overrides an instance method inherited from its parent.

Example:

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

Now:

    Animal animal;

    animal = new Dog();
    animal.sound();

    animal = new Cat();
    animal.sound();

Output:

    Dog barks
    Cat meows

The reference type is:

    Animal

The actual objects are:

    Dog
    Cat

The overridden method is selected according to the actual object.

---

# 8. The Most Important Rule

One of the most important rules in Java polymorphism is:

> **Reference type decides what members are accessible, while the actual object decides which overridden instance method executes.**

Example:

    Animal animal = new Dog();

Here:

    Reference type → Animal
    Actual object  → Dog

Suppose:

    class Animal {

        void sound() {
            System.out.println("Animal");
        }

        void eat() {
            System.out.println("Eating");
        }
    }

    class Dog extends Animal {

        @Override
        void sound() {
            System.out.println("Dog");
        }

        void bark() {
            System.out.println("Barking");
        }
    }

Now:

    Animal animal = new Dog();

This works:

    animal.sound();
    animal.eat();

But this does not compile:

    animal.bark();

Why?

Because the reference type is:

    Animal

and `bark()` is not declared in `Animal`.

However:

    animal.sound();

executes:

    Dog.sound()

because `sound()` is overridden and the actual object is a `Dog`.

---

# 9. Upcasting and Runtime Polymorphism

Upcasting means assigning a child object to a parent reference.

Example:

    Dog dog = new Dog();

    Animal animal = dog;

Or directly:

    Animal animal = new Dog();

This is called:

    Upcasting

It is generally implicit because every `Dog` is an `Animal`.

Relationship:

    Animal
       ↑
      Dog

Therefore:

    Animal animal = new Dog();

is valid.

The important point is:

    Reference Type → Animal
    Object Type    → Dog

This is the foundation of runtime polymorphism.

---

# 10. Polymorphism Through Parent Reference

A parent reference can point to different child objects.

Example:

    Animal animal;

    animal = new Dog();
    animal.sound();

    animal = new Cat();
    animal.sound();

    animal = new Cow();
    animal.sound();

This gives us one common reference:

    Animal

with multiple possible implementations:

    Dog
    Cat
    Cow

This is one of the most useful forms of runtime polymorphism.

---

# 11. Polymorphism with Arrays

An array of a parent type can store objects of different child types.

Example:

    Animal[] animals = {
        new Dog(),
        new Cat(),
        new Cow()
    };

Now:

    for (Animal animal : animals) {
        animal.sound();
    }

Possible output:

    Dog barks
    Cat meows
    Cow moos

The array type is:

    Animal[]

but it contains different objects.

This is polymorphism.

---

# 12. Polymorphism with Collections

Polymorphism is heavily used with Java Collections.

Example:

    List<Animal> animals = new ArrayList<>();

    animals.add(new Dog());
    animals.add(new Cat());
    animals.add(new Cow());

Now:

    for (Animal animal : animals) {
        animal.sound();
    }

The collection works with the common parent type:

    Animal

while storing different implementations.

This concept is extremely important in real-world Java and Spring Boot development.

---

# 13. Method Overloading vs Method Overriding

| Feature | Method Overloading | Method Overriding |
|---|---|---|
| Meaning | Same method name, different parameters | Child provides new implementation |
| Relationship | Usually same class | Parent-child relationship |
| Binding | Compile-time | Runtime |
| Polymorphism | Compile-time | Runtime |
| Parameters | Must differ | Must be same |
| Return type | Can differ, but cannot distinguish overload by return type alone | Same or covariant |
| `static` | Can be overloaded | Cannot be overridden |
| `private` | Can be overloaded | Cannot be overridden |
| `final` | Can be overloaded | Cannot be overridden |
| Main purpose | Convenience | Different implementation |

Example of overloading:

    class Calculator {

        int add(int a, int b) {
            return a + b;
        }

        int add(int a, int b, int c) {
            return a + b + c;
        }
    }

Example of overriding:

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

---

# 14. Static Binding

Static binding means the method call is resolved at compile time.

It is also called:

    Early Binding
    Static Binding

Examples include:

    Method Overloading
    Static methods
    Private methods

Example:

    class Calculator {

        void show(int x) {
            System.out.println("int");
        }

        void show(double x) {
            System.out.println("double");
        }
    }

The compiler decides which method matches:

    calculator.show(10);

Therefore:

    show(int)

is selected.

---

# 15. Dynamic Binding

Dynamic binding means the overridden instance method is selected at runtime.

It is also called:

    Late Binding
    Dynamic Binding

Example:

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

    Animal animal = new Dog();

    animal.sound();

Output:

    Dog

The reference is:

    Animal

but the actual object is:

    Dog

Therefore the overridden method from `Dog` executes.

---

# 16. Dynamic Method Dispatch

Dynamic Method Dispatch is the mechanism through which Java determines the overridden method to execute at runtime.

Example:

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

    class Cat extends Animal {

        @Override
        void sound() {
            System.out.println("Cat");
        }
    }

    Animal animal;

    animal = new Dog();
    animal.sound();

    animal = new Cat();
    animal.sound();

At runtime:

    animal → Dog object
             ↓
         Dog.sound()

Then:

    animal → Cat object
             ↓
         Cat.sound()

The JVM dynamically dispatches the overridden instance method.

---

# 17. Important: Fields Do NOT Behave Like Overridden Methods

This is a common interview trap.

Fields are not dynamically dispatched like overridden instance methods.

Example:

    class Parent {

        int value = 10;
    }

    class Child extends Parent {

        int value = 20;
    }

Now:

    Parent p = new Child();

    System.out.println(p.value);

Output:

    10

The field is selected based on the **reference type**.

But methods behave differently.

Example:

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

Now:

    Parent p = new Child();

    p.show();

Output:

    Child

Remember:

    Fields  → Reference type
    Methods → Actual object for overridden instance methods

---

# 18. Static Methods and Polymorphism

Static methods belong to the class, not the object.

They are **hidden**, not overridden.

Example:

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

Now:

    Parent p = new Child();

    p.show();

Output:

    Parent

Therefore:

    Instance method → Overriding + Runtime Polymorphism
    Static method   → Method Hiding

Static methods are resolved using the reference/class context rather than runtime object dispatch.

---

# 19. Private Methods and Polymorphism

Private methods are not inherited in the normal overriding sense.

Therefore, they cannot be overridden.

Example:

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

These are two separate methods.

They do not participate in runtime overriding.

Remember:

    private → not overridden

---

# 20. Final Methods and Polymorphism

A `final` method cannot be overridden.

Example:

    class Parent {

        final void show() {
            System.out.println("Parent");
        }
    }

This is illegal:

    class Child extends Parent {

        @Override
        void show() {
            System.out.println("Child");
        }
    }

Because:

    final method
         ↓
    cannot be overridden
         ↓
    no runtime replacement

---

# 21. Constructors and Polymorphism

Constructors are not inherited and cannot be overridden.

Therefore:

    Constructors → No method overriding

However, constructors participate in constructor chaining.

Example:

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

Now:

    Child child = new Child();

Output:

    Parent constructor
    Child constructor

This is:

    Constructor Chaining

not polymorphism.

---

# 22. Covariant Return Type

A child class can override a method and return a more specific subtype.

This is called a **covariant return type**.

Example:

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

Relationship:

    Animal
       ↑
      Dog

The child method returns:

    Dog

which is a subtype of:

    Animal

Therefore, this is valid.

---

# 23. Polymorphism and Casting

Suppose:

    Animal animal = new Dog();

This is upcasting.

If we need Dog-specific functionality:

    Dog dog = (Dog) animal;

This is downcasting.

Now:

    dog.bark();

can be called.

But incorrect downcasting can cause:

    ClassCastException

Example:

    Animal animal = new Cat();

    Dog dog = (Dog) animal;

This compiles but fails at runtime because the actual object is a `Cat`.

Important:

    Reference type → Animal
    Actual object  → Cat

You cannot treat a Cat object as a Dog object.

---

# 24. instanceof with Polymorphism

Before downcasting, we can check the actual object type.

Example:

    if (animal instanceof Dog) {
        Dog dog = (Dog) animal;
        dog.bark();
    }

Modern Java also supports pattern matching:

    if (animal instanceof Dog dog) {
        dog.bark();
    }

This combines:

    Type Checking
    +
    Casting

in a concise form.

---

# 25. Polymorphism Does Not Mean "Everything Is Dynamic"

A common misconception is:

> "If Java has polymorphism, every method call is decided at runtime."

That is incorrect.

Java has both:

    Compile-time binding
    Runtime binding

Important cases:

    Overloading → Compile-time
    Overriding  → Runtime
    Static      → Class/reference based
    Private     → Not overridden
    Final       → Cannot be overridden

Therefore, always identify what kind of member or method is involved.

---

# 26. Polymorphism and Abstraction

Polymorphism works especially well with abstraction.

Example:

    abstract class Payment {

        abstract void pay();
    }

Implementations:

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

Now:

    Payment payment;

    payment = new UPI();
    payment.pay();

    payment = new Card();
    payment.pay();

The parent type defines the common contract:

    Payment

while child classes provide different implementations.

This combination is extremely important in software design.

---

# 27. Real-World Backend Example

Imagine a backend application supporting multiple notification types.

Common contract:

    interface Notification {

        void send();
    }

Implementations:

    class EmailNotification implements Notification {

        @Override
        public void send() {
            System.out.println("Sending Email");
        }
    }

    class SMSNotification implements Notification {

        @Override
        public void send() {
            System.out.println("Sending SMS");
        }
    }

    class WhatsAppNotification implements Notification {

        @Override
        public void send() {
            System.out.println("Sending WhatsApp message");
        }
    }

Now:

    Notification notification;

    notification = new EmailNotification();
    notification.send();

    notification = new SMSNotification();
    notification.send();

    notification = new WhatsAppNotification();
    notification.send();

The business code can depend on:

    Notification

rather than every concrete implementation.

This is one of the foundations of extensible backend architecture.

---

# 28. Main Advantages of Polymorphism

## 1. Flexibility

The same code can work with different object types.

## 2. Loose Coupling

Code can depend on parent classes or interfaces instead of concrete implementations.

## 3. Extensibility

New child classes can often be introduced without changing existing client code.

## 4. Maintainability

Behavior can be separated into specialized classes.

## 5. Reusability

Generic methods can operate on parent types.

Example:

    void processPayment(Payment payment) {
        payment.pay();
    }

The method can accept:

    UPI
    Card
    NetBanking
    Wallet

as long as they implement or extend `Payment`.

---

# 29. Common Interview Traps

## Trap 1 — Reference Type vs Object Type

Example:

    Animal animal = new Dog();

Answer:

    Reference type → Animal
    Object type    → Dog

---

## Trap 2 — Which method executes?

Example:

    animal.sound();

If `sound()` is overridden:

    Actual object determines the implementation.

---

## Trap 3 — Fields

Example:

    Parent parent = new Child();

    parent.value;

Field selection depends on:

    Reference type

Fields are not dynamically dispatched like overridden instance methods.

---

## Trap 4 — Static Methods

Static methods are not overridden.

They are:

    Hidden

---

## Trap 5 — Private Methods

Private methods cannot be overridden.

---

## Trap 6 — Final Methods

Final methods cannot be overridden.

---

## Trap 7 — Constructors

Constructors cannot be overridden.

---

## Trap 8 — Overloading vs Overriding

    Overloading → Compile-time
    Overriding  → Runtime

---

## Trap 9 — Downcasting

Incorrect downcasting can cause:

    ClassCastException

---

# 30. Polymorphism vs Inheritance

These concepts are related but not identical.

Inheritance:

    Inheritance
         ↓
    Creates IS-A relationship
         ↓
    Allows child classes to inherit/extend behavior

Polymorphism:

    Polymorphism
         ↓
    Allows common parent/interface references
         ↓
    To work with different implementations

Inheritance is one common mechanism that enables runtime polymorphism, while interfaces can also enable runtime polymorphism.

---

# 31. Polymorphism vs Abstraction

| Polymorphism | Abstraction |
|---|---|
| Multiple forms of behavior | Hides implementation details |
| Focuses on behavior selection | Focuses on essential features |
| Achieved through overloading/overriding | Achieved through abstract classes/interfaces |
| Supports flexibility | Supports contracts/design |
| Example: `animal.sound()` behaves differently | Example: `Animal` defines `sound()` contract |

They often work together.

Example:

    Payment payment = new UPI();

Here:

    Abstraction → Payment hides implementation details
    Polymorphism → UPI provides the runtime behavior

---

# 32. Core Mental Model

Remember this:

    Polymorphism
         |
         +----------------------+
         |                      |
    Compile-Time             Runtime
         |                      |
    Method Overloading      Method Overriding
         |                      |
    Early Binding           Dynamic Binding
                                |
                           Actual Object
                           decides method

And:

    Parent reference
           +
      Child object
           ↓
    Runtime Polymorphism

Example:

    Animal animal = new Dog();

Think:

    Reference → Animal
    Object    → Dog
    Method    → Dog's overridden method

---

# 33. 30-Second Interview Answer

> **Polymorphism is an OOP concept where the same interface, reference, or method call can represent different forms of behavior. In Java, it is commonly divided into compile-time polymorphism through method overloading and runtime polymorphism through method overriding. In runtime polymorphism, a parent reference can refer to a child object, and the overridden instance method is selected based on the actual object at runtime.**

Example:

    Animal animal = new Dog();
    animal.sound();

Here:

    Animal → Reference type
    Dog    → Actual object
    Dog.sound() → Executed at runtime

---

# 34. Top Interview Questions

## Basic

1. What is polymorphism?
2. Why is polymorphism important?
3. What are the types of polymorphism in Java?
4. What is compile-time polymorphism?
5. What is runtime polymorphism?
6. How is method overloading related to polymorphism?
7. How is method overriding related to polymorphism?

## Intermediate

8. What is dynamic method dispatch?
9. What is static binding?
10. What is dynamic binding?
11. What happens when a parent reference points to a child object?
12. What determines which members are accessible?
13. What determines which overridden method executes?
14. Can static methods be overridden?
15. Can private methods be overridden?
16. Can final methods be overridden?
17. Can constructors be overridden?
18. Are fields polymorphic?

## Advanced

19. What is a covariant return type?
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

# 35. 🧩 DSA & Problem-Solving Patterns

Polymorphism itself is not a DSA algorithm, but it is important for understanding how Java's collection and object-oriented APIs work.

## DSA-Relevant Concepts

### 1. Programming to an Interface

Very important when working with Java Collections.

Example:

    List<Integer> list = new ArrayList<>();

Here:

    List → Interface
    ArrayList → Implementation

This allows the implementation to be changed without changing the code that uses the `List` abstraction.

Example:

    List<Integer> list = new LinkedList<>();

The variable type remains:

    List<Integer>

while the implementation changes.

---

### 2. Parent Type with Different Implementations

Example:

    List<Integer> list;

    list = new ArrayList<>();
    list.add(10);

    list = new LinkedList<>();
    list.add(20);

The same interface can represent different implementations.

This concept is heavily used throughout the Collections Framework.

---

### 3. Polymorphic Traversal

Collections can be processed through common interfaces:

    Collection<Integer> numbers = new ArrayList<>();

    for (Integer number : numbers) {
        System.out.println(number);
    }

The traversal code does not need to know the concrete collection implementation.

---

### 4. Strategy Pattern Connection

A common DSA/interview design pattern is the **Strategy Pattern**.

Different algorithms can implement the same interface.

Example:

    interface SortStrategy {

        void sort(int[] arr);
    }

    class BubbleSort implements SortStrategy {

        @Override
        public void sort(int[] arr) {
            // Bubble Sort
        }
    }

    class MergeSort implements SortStrategy {

        @Override
        public void sort(int[] arr) {
            // Merge Sort
        }
    }

Then:

    SortStrategy strategy = new MergeSort();
    strategy.sort(arr);

The caller works with:

    SortStrategy

while the actual algorithm can change.

---

# 36. DSA Practice Questions

## Question 1 — Parent Reference

What is the output?

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

    public class Main {

        public static void main(String[] args) {

            Animal animal = new Dog();

            animal.sound();
        }
    }

Answer:

    Dog

Reason:

    Reference type → Animal
    Actual object  → Dog
    Overridden method → Dog.sound()

---

## Question 2 — Polymorphic Array

What will this print?

    Animal[] animals = {
        new Dog(),
        new Cat()
    };

    for (Animal animal : animals) {
        animal.sound();
    }

Answer:

    Dog barks
    Cat meows

Pattern:

    Parent array
         ↓
    Different child objects
         ↓
    Runtime dispatch

---

## Question 3 — Collections

Which declaration demonstrates programming to an interface?

    ArrayList<Integer> list = new ArrayList<>();

or:

    List<Integer> list = new ArrayList<>();

Answer:

    List<Integer> list = new ArrayList<>();

Why?

Because the variable depends on the abstraction:

    List

rather than the concrete implementation:

    ArrayList

---

## Question 4 — Strategy Pattern

Suppose we have:

    interface SearchStrategy {
        void search(int[] arr, int target);
    }

and:

    class LinearSearch implements SearchStrategy {
        public void search(int[] arr, int target) {
            // Linear Search
        }
    }

    class BinarySearch implements SearchStrategy {
        public void search(int[] arr, int target) {
            // Binary Search
        }
    }

Then:

    SearchStrategy strategy = new BinarySearch();

    strategy.search(arr, target);

Question:

Which algorithm runs?

Answer:

    Binary Search

Reason:

The actual object is:

    BinarySearch

The overridden method is selected at runtime.

---

# 37. Quick Revision Cheat Sheet

    POLYMORPHISM
    │
    ├── Meaning
    │   └── One thing → Multiple forms
    │
    ├── Compile-Time
    │   └── Method Overloading
    │       └── Early / Static Binding
    │
    └── Runtime
        └── Method Overriding
            └── Dynamic / Late Binding

---

    Animal animal = new Dog();

    Reference Type → Animal
    Object Type    → Dog

    Accessible Members
           ↓
    Reference Type

    Overridden Instance Method
           ↓
    Actual Object

---

    Fields           → Not dynamically dispatched
    Static methods   → Hidden
    Private methods  → Not overridden
    Final methods    → Cannot be overridden
    Constructors     → Cannot be overridden
    Instance methods → Runtime polymorphism possible

---

# 🧠 Final Memory Trick

Remember:

    "REFERENCE DECIDES ACCESS,
     OBJECT DECIDES OVERRIDDEN BEHAVIOR."

Or even shorter:

    Access → Reference
    Behavior → Object

And the most important runtime polymorphism pattern:

    Parent reference
           +
      Child object
           ↓
    Overridden method
           ↓
    Runtime Polymorphism

Example:

    Animal animal = new Dog();
    animal.sound();

    Animal → What can I access?
    Dog    → What implementation runs?

---

# 🎯 One-Line Interview Definition

> **Polymorphism allows a common reference or interface to represent different implementations, with overloaded methods selected at compile time and overridden instance methods selected at runtime.**