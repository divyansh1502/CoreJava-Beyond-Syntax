# 🧩 Design Patterns in Java

> **A design pattern is a reusable, proven approach to solving a recurring software design problem; it is not a ready-made piece of code or a complete application architecture.**

---

# 📑 Table of Contents

- [1. What Is a Design Pattern?](#1-what-is-a-design-pattern)
- [2. Why Design Patterns?](#2-why-design-patterns)
- [3. Design Pattern vs Algorithm](#3-design-pattern-vs-algorithm)
- [4. Design Pattern vs Framework](#4-design-pattern-vs-framework)
- [5. Design Pattern vs Architecture](#5-design-pattern-vs-architecture)
- [6. Gang of Four](#6-gang-of-four)
- [7. Three Main Categories](#7-three-main-categories)
- [8. Creational Patterns](#8-creational-patterns)
- [9. Structural Patterns](#9-structural-patterns)
- [10. Behavioral Patterns](#10-behavioral-patterns)
- [11. Singleton Pattern](#11-singleton-pattern)
- [12. Factory Method Pattern](#12-factory-method-pattern)
- [13. Abstract Factory Pattern](#13-abstract-factory-pattern)
- [14. Builder Pattern](#14-builder-pattern)
- [15. Prototype Pattern](#15-prototype-pattern)
- [16. Adapter Pattern](#16-adapter-pattern)
- [17. Decorator Pattern](#17-decorator-pattern)
- [18. Facade Pattern](#18-facade-pattern)
- [19. Proxy Pattern](#19-proxy-pattern)
- [20. Composite Pattern](#20-composite-pattern)
- [21. Bridge Pattern](#21-bridge-pattern)
- [22. Observer Pattern](#22-observer-pattern)
- [23. Strategy Pattern](#23-strategy-pattern)
- [24. Command Pattern](#24-command-pattern)
- [25. Template Method Pattern](#25-template-method-pattern)
- [26. State Pattern](#26-state-pattern)
- [27. Iterator Pattern](#27-iterator-pattern)
- [28. Chain of Responsibility](#28-chain-of-responsibility)
- [29. Mediator Pattern](#29-mediator-pattern)
- [30. Memento Pattern](#30-memento-pattern)
- [31. Visitor Pattern](#31-visitor-pattern)
- [32. Interpreter Pattern](#32-interpreter-pattern)
- [33. Dependency Injection](#33-dependency-injection)
- [34. SOLID and Design Patterns](#34-solid-and-design-patterns)
- [35. Composition Over Inheritance](#35-composition-over-inheritance)
- [36. Design Pattern Selection](#36-design-pattern-selection)
- [37. Common Mistakes](#37-common-mistakes)
- [38. Top 30 Interview Questions](#38-top-30-interview-questions)
- [39. 30-Second Interview Answer](#39-30-second-interview-answer)
- [40. Cheat Sheet](#40-cheat-sheet)
- [41. Final Mental Model](#41-final-mental-model)

---

# 1. What Is a Design Pattern?

A design pattern is a general solution to a recurring software design problem.

It is not:

```text
A complete application
A library
A framework
A copy-paste code snippet
```

Instead:

```text
Problem
   ↓
Known design approach
   ↓
Adapt it to your situation
```

For example, suppose many parts of an application need access to one shared configuration object.

A possible design approach is:

```text
Singleton Pattern
```

The pattern describes the structure and rules.

You still have to implement it according to your application.

---

# 2. Why Design Patterns?

Design patterns can help developers:

```text
Reduce repeated design mistakes
Improve maintainability
Communicate design ideas
Separate responsibilities
Reduce coupling
Increase flexibility
Reuse proven design approaches
```

Instead of explaining a design with a long description, a developer can say:

```text
"Use a Strategy here."
```

That communicates a known family of design ideas.

---

# 3. Design Pattern vs Algorithm

An algorithm mainly describes:

```text
How to solve a computational problem
```

Example:

```text
Binary Search
Merge Sort
Two Pointers
Dijkstra
```

A design pattern describes:

```text
How software components can be organized
to solve a recurring design problem.
```

Example:

```text
Strategy
Observer
Factory
Decorator
Adapter
```

Think:

```text
Algorithm
→ computational steps

Design Pattern
→ software structure/interaction
```

---

# 4. Design Pattern vs Framework

A framework is reusable software infrastructure.

Examples:

```text
Spring
Hibernate
JUnit
```

A design pattern is a conceptual design approach.

For example:

```text
Spring
→ framework

Dependency Injection
→ design technique/pattern-related concept
```

A framework may internally use many design patterns.

---

# 5. Design Pattern vs Architecture

Architecture operates at a larger system level.

Examples:

```text
Monolithic Architecture
Microservices Architecture
Layered Architecture
Event-Driven Architecture
```

Design patterns usually operate at a smaller design level.

Example:

```text
Architecture
→ Microservices

Inside one service:
→ Strategy
→ Factory
→ Builder
→ Adapter
```

So:

```text
Architecture
→ system-level organization

Design Pattern
→ recurring design-level solution
```

---

# 6. Gang of Four

The term:

```text
Gang of Four
```

refers to the four authors of the influential book:

```text
Design Patterns:
Elements of Reusable Object-Oriented Software
```

Authors:

```text
Erich Gamma
Richard Helm
Ralph Johnson
John Vlissides
```

They described **23 classic object-oriented design patterns**.

These patterns are commonly grouped into:

```text
Creational
Structural
Behavioral
```

---

# 7. Three Main Categories

The classic GoF patterns are divided into three categories.

```text
Design Patterns
      |
      +----------------+
      |                |
      ↓                ↓
 Creational        Structural
      |
      |
      +----------------------+
                             |
                             ↓
                         Behavioral
```

More clearly:

```text
Creational
→ object creation

Structural
→ object/class composition

Behavioral
→ communication and responsibility
```

---

# 8. Creational Patterns

Creational patterns deal with object creation.

Classic GoF creational patterns:

```text
1. Singleton
2. Factory Method
3. Abstract Factory
4. Builder
5. Prototype
```

Main idea:

```text
Control or simplify object creation.
```

---

# 9. Structural Patterns

Structural patterns deal with how classes and objects are composed.

Classic patterns:

```text
1. Adapter
2. Bridge
3. Composite
4. Decorator
5. Facade
6. Flyweight
7. Proxy
```

Main idea:

```text
How objects/classes are connected.
```

---

# 10. Behavioral Patterns

Behavioral patterns focus on communication and responsibility.

Classic patterns:

```text
1. Chain of Responsibility
2. Command
3. Interpreter
4. Iterator
5. Mediator
6. Memento
7. Observer
8. State
9. Strategy
10. Template Method
11. Visitor
```

Main idea:

```text
How objects communicate
and distribute responsibilities.
```

---

# 11. Singleton Pattern

The Singleton pattern ensures that a class provides a single shared instance according to the design's lifecycle and access requirements.

Basic example:

```java
class Singleton {

    private static Singleton instance;

    private Singleton() {
    }

    public static Singleton getInstance() {

        if (instance == null) {

            instance = new Singleton();
        }

        return instance;
    }
}
```

Usage:

```java
Singleton a =
    Singleton.getInstance();

Singleton b =
    Singleton.getInstance();
```

Check:

```java
System.out.println(
    a == b
);
```

Output:

```text
true
```

Both references point to the same instance.

---

## 11.1 Why Private Constructor?

The constructor is:

```java
private Singleton() {
}
```

This prevents outside code from directly creating objects using:

```java
new Singleton();
```

---

## 11.2 Singleton and Multithreading

The simple implementation above is not thread-safe.

Multiple threads could simultaneously observe:

```text
instance == null
```

and create multiple objects.

One approach is synchronized access:

```java
class Singleton {

    private static Singleton instance;

    private Singleton() {
    }

    public static synchronized Singleton getInstance() {

        if (instance == null) {

            instance =
                new Singleton();
        }

        return instance;
    }
}
```

Modern Java also provides approaches such as:

```text
Enum Singleton
Initialization-on-demand holder
Double-checked locking
```

Each has trade-offs.

---

# 12. Factory Method Pattern

The Factory Method pattern moves object creation behind a method or abstraction.

Instead of client code directly deciding which concrete class to instantiate:

```java
Car car =
    new Car();
```

the creation can be delegated:

```java
Vehicle vehicle =
    VehicleFactory.create(
        "CAR"
    );
```

Example:

```java
interface Vehicle {

    void drive();
}
```

```java
class Car
    implements Vehicle {

    @Override
    public void drive() {

        System.out.println(
            "Car driving"
        );
    }
}
```

```java
class Bike
    implements Vehicle {

    @Override
    public void drive() {

        System.out.println(
            "Bike driving"
        );
    }
}
```

Factory:

```java
class VehicleFactory {

    static Vehicle create(
        String type
    ) {

        if (type.equals("CAR")) {

            return new Car();
        }

        if (type.equals("BIKE")) {

            return new Bike();
        }

        throw new IllegalArgumentException(
            "Unknown vehicle"
        );
    }
}
```

Client:

```java
Vehicle vehicle =
    VehicleFactory.create(
        "CAR"
    );

vehicle.drive();
```

The client depends on the abstraction:

```text
Vehicle
```

rather than directly constructing every concrete implementation.

---

# 13. Abstract Factory Pattern

Abstract Factory provides an interface for creating families of related objects.

Suppose an application supports:

```text
Windows UI
Mac UI
```

Each platform has:

```text
Button
Checkbox
```

The abstract factory can create a consistent family.

Example:

```java
interface Button {

    void paint();
}
```

```java
interface Checkbox {

    void check();
}
```

Factory:

```java
interface GUIFactory {

    Button createButton();

    Checkbox createCheckbox();
}
```

Windows factory:

```java
class WindowsFactory
    implements GUIFactory {

    @Override
    public Button createButton() {

        return new WindowsButton();
    }

    @Override
    public Checkbox createCheckbox() {

        return new WindowsCheckbox();
    }
}
```

The key idea:

```text
Factory Method
→ usually creates one product type

Abstract Factory
→ creates related product families
```

---

# 14. Builder Pattern

Builder is useful when creating complex objects, especially when there are many optional parameters.

Without Builder:

```java
User user =
    new User(
        "Divyansh",
        22,
        "Lucknow",
        "Java",
        true
    );
```

This can become difficult to read.

Builder:

```java
class User {

    private String name;
    private int age;
    private String city;

    private User(
        Builder builder
    ) {

        this.name = builder.name;
        this.age = builder.age;
        this.city = builder.city;
    }

    static class Builder {

        private String name;
        private int age;
        private String city;

        Builder name(String name) {

            this.name = name;
            return this;
        }

        Builder age(int age) {

            this.age = age;
            return this;
        }

        Builder city(String city) {

            this.city = city;
            return this;
        }

        User build() {

            return new User(this);
        }
    }
}
```

Usage:

```java
User user =
    new User.Builder()
        .name("Divyansh")
        .age(22)
        .city("Lucknow")
        .build();
```

The builder makes object construction readable.

---

# 15. Prototype Pattern

Prototype creates new objects by copying an existing object.

Conceptually:

```text
Existing Object
      |
      ↓
     copy
      |
      ↓
New Object
```

Java provides `Cloneable`, but using `clone()` has several complexities.

A simpler explicit approach is often preferable.

Example:

```java
class User {

    String name;

    User(String name) {

        this.name = name;
    }

    User copy() {

        return new User(
            this.name
        );
    }
}
```

Usage:

```java
User original =
    new User("Java");

User copy =
    original.copy();
```

Now:

```text
original != copy
```

but their copied state can be equal.

---

# 16. Adapter Pattern

Adapter makes incompatible interfaces work together.

Conceptually:

```text
Client
  |
  ↓
Adapter
  |
  ↓
Existing incompatible class
```

Example:

```java
interface Charger {

    void charge();
}
```

Existing class:

```java
class OldCharger {

    void oldCharge() {

        System.out.println(
            "Charging with old charger"
        );
    }
}
```

Adapter:

```java
class ChargerAdapter
    implements Charger {

    private OldCharger charger =
        new OldCharger();

    @Override
    public void charge() {

        charger.oldCharge();
    }
}
```

Client:

```java
Charger charger =
    new ChargerAdapter();

charger.charge();
```

The Adapter translates one interface into another expected by the client.

---

# 17. Decorator Pattern

Decorator dynamically adds responsibilities to an object without changing its original class.

Concept:

```text
Base Object
    ↓
Decorator
    ↓
Another Decorator
    ↓
Final Object
```

Example:

```java
interface Coffee {

    String getDescription();

    double getCost();
}
```

Basic coffee:

```java
class SimpleCoffee
    implements Coffee {

    @Override
    public String getDescription() {

        return "Coffee";
    }

    @Override
    public double getCost() {

        return 50;
    }
}
```

Decorator:

```java
class MilkDecorator
    implements Coffee {

    private Coffee coffee;

    MilkDecorator(
        Coffee coffee
    ) {

        this.coffee = coffee;
    }

    @Override
    public String getDescription() {

        return coffee.getDescription()
            + " + Milk";
    }

    @Override
    public double getCost() {

        return coffee.getCost()
            + 10;
    }
}
```

Usage:

```java
Coffee coffee =
    new MilkDecorator(
        new SimpleCoffee()
    );
```

This pattern is commonly associated with Java I/O stream design.

---

# 18. Facade Pattern

Facade provides a simplified interface to a complex subsystem.

Without Facade:

```text
Client
 ↓
Subsystem A
 ↓
Subsystem B
 ↓
Subsystem C
```

With Facade:

```text
Client
 ↓
Facade
 ↓
A + B + C
```

Example:

```java
class CPU {

    void start() {
    }
}
```

```java
class Memory {

    void load() {
    }
}
```

```java
class ComputerFacade {

    private CPU cpu =
        new CPU();

    private Memory memory =
        new Memory();

    void startComputer() {

        cpu.start();
        memory.load();

        System.out.println(
            "Computer started"
        );
    }
}
```

Client:

```java
ComputerFacade computer =
    new ComputerFacade();

computer.startComputer();
```

The client does not need to understand all subsystem details.

---

# 19. Proxy Pattern

A Proxy provides a substitute or intermediary for another object.

Common purposes:

```text
Access control
Lazy loading
Caching
Logging
Remote access
```

Concept:

```text
Client
  |
  ↓
Proxy
  |
  ↓
Real Object
```

Example:

```java
interface Service {

    void execute();
}
```

Real object:

```java
class RealService
    implements Service {

    @Override
    public void execute() {

        System.out.println(
            "Executing service"
        );
    }
}
```

Proxy:

```java
class ServiceProxy
    implements Service {

    private RealService service =
        new RealService();

    @Override
    public void execute() {

        System.out.println(
            "Checking access"
        );

        service.execute();
    }
}
```

---

# 20. Composite Pattern

Composite lets individual objects and groups of objects be treated uniformly.

Concept:

```text
Component
   |
   +── Leaf
   |
   +── Composite
          |
          +── Leaf
          +── Leaf
```

Example:

```java
interface Employee {

    void showDetails();
}
```

Leaf:

```java
class Developer
    implements Employee {

    @Override
    public void showDetails() {

        System.out.println(
            "Developer"
        );
    }
}
```

Composite:

```java
class Team
    implements Employee {

    private List<Employee> employees =
        new ArrayList<>();

    void add(Employee employee) {

        employees.add(employee);
    }

    @Override
    public void showDetails() {

        for (
            Employee employee :
            employees
        ) {

            employee.showDetails();
        }
    }
}
```

Composite is useful for tree-like structures.

---

# 21. Bridge Pattern

Bridge separates an abstraction from its implementation so they can evolve independently.

Concept:

```text
Abstraction
     |
     ↓
Implementation
```

Instead of building a large inheritance hierarchy:

```text
Shape
├── RedCircle
├── BlueCircle
├── RedSquare
└── BlueSquare
```

Bridge can separate:

```text
Shape
   |
   +── Circle
   +── Square

Color
   |
   +── Red
   +── Blue
```

This reduces combinations caused by multiple independent dimensions.

---

# 22. Observer Pattern

Observer creates a one-to-many relationship.

When one object changes state:

```text
Subject
   |
   +── Observer A
   +── Observer B
   +── Observer C
```

Observers can be notified.

Example:

```java
interface Observer {

    void update();
}
```

Subject:

```java
class Subject {

    private List<Observer> observers =
        new ArrayList<>();

    void addObserver(
        Observer observer
    ) {

        observers.add(observer);
    }

    void notifyObservers() {

        for (
            Observer observer :
            observers
        ) {

            observer.update();
        }
    }
}
```

This pattern is common in event-driven systems.

---

# 23. Strategy Pattern

Strategy encapsulates interchangeable algorithms or behaviors.

Suppose payment methods are:

```text
UPI
Card
Cash
```

Instead of putting everything into one large conditional:

```text
if UPI
else if Card
else Cash
```

define a strategy:

```java
interface PaymentStrategy {

    void pay(double amount);
}
```

UPI strategy:

```java
class UpiPayment
    implements PaymentStrategy {

    @Override
    public void pay(
        double amount
    ) {

        System.out.println(
            "Paid using UPI: "
            + amount
        );
    }
}
```

Context:

```java
class PaymentContext {

    private PaymentStrategy strategy;

    PaymentContext(
        PaymentStrategy strategy
    ) {

        this.strategy = strategy;
    }

    void pay(double amount) {

        strategy.pay(amount);
    }
}
```

Usage:

```java
PaymentContext context =
    new PaymentContext(
        new UpiPayment()
    );

context.pay(500);
```

Strategy is particularly useful when behavior needs to vary independently from the object using it.

---

# 24. Command Pattern

Command encapsulates a request as an object.

Concept:

```text
Client
  ↓
Command
  ↓
Receiver
```

Example:

```java
interface Command {

    void execute();
}
```

Receiver:

```java
class Light {

    void on() {

        System.out.println(
            "Light ON"
        );
    }
}
```

Command:

```java
class LightOnCommand
    implements Command {

    private Light light;

    LightOnCommand(
        Light light
    ) {

        this.light = light;
    }

    @Override
    public void execute() {

        light.on();
    }
}
```

This allows requests to be stored, queued, logged, or undone depending on the design.

---

# 25. Template Method Pattern

Template Method defines the skeleton of an algorithm while allowing subclasses to customize certain steps.

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

        System.out.println(
            "Saving data"
        );
    }
}
```

Subclass:

```java
class CSVProcessor
    extends DataProcessor {

    @Override
    void readData() {

        System.out.println(
            "Reading CSV"
        );
    }

    @Override
    void processData() {

        System.out.println(
            "Processing CSV"
        );
    }
}
```

The overall algorithm remains controlled by the superclass.

---

# 26. State Pattern

State allows an object's behavior to change when its internal state changes.

Concept:

```text
Object
  |
  +── State A
  |
  +── State B
  |
  +── State C
```

For example:

```text
Order
 ↓
Pending
 ↓
Paid
 ↓
Shipped
 ↓
Delivered
```

Instead of putting every state transition into one huge conditional block, behavior can be represented by separate state objects.

---

# 27. Iterator Pattern

Iterator provides a standard way to traverse a collection without exposing its internal representation.

Java already provides this concept through:

```java
Iterator
```

Example:

```java
List<String> names =
    new ArrayList<>();

names.add("Java");
names.add("Spring");

Iterator<String> iterator =
    names.iterator();
```

Traverse:

```java
while (iterator.hasNext()) {

    System.out.println(
        iterator.next()
    );
}
```

The client does not need to know whether the underlying collection uses an array, linked nodes, or another structure.

---

# 28. Chain of Responsibility

A request is passed through a chain of handlers.

Concept:

```text
Request
   ↓
Handler A
   ↓
Handler B
   ↓
Handler C
```

Each handler can:

```text
Handle request
OR
Pass request to next handler
```

Example use cases:

```text
Authentication
Authorization
Validation
Logging
Exception processing
```

This pattern can reduce a long chain of conditional logic.

---

# 29. Mediator Pattern

Mediator centralizes communication between related objects.

Without Mediator:

```text
A ↔ B
A ↔ C
B ↔ C
B ↔ D
C ↔ D
```

Communication becomes tightly connected.

With Mediator:

```text
A ─┐
B ─┤
C ─┼── Mediator
D ─┘
```

Objects communicate through the mediator instead of directly knowing every other object.

---

# 30. Memento Pattern

Memento captures an object's state so it can later be restored without exposing the object's internal implementation.

Common conceptual use:

```text
Undo
Rollback
Snapshots
```

Concept:

```text
Object State
    ↓
Memento
    ↓
Restore later
```

For example:

```text
Editor
 ↓
Save state
 ↓
Memento
 ↓
Modify document
 ↓
Undo
 ↓
Restore memento
```

---

# 31. Visitor Pattern

Visitor separates an operation from the object structure on which it operates.

Concept:

```text
Object Structure
      |
      ↓
   Visitor
```

Useful when:

```text
Object structure is relatively stable
+
New operations are added frequently
```

The visitor can perform different operations for different element types.

---

# 32. Interpreter Pattern

Interpreter represents a language or grammar using an object structure and evaluates expressions.

Concept:

```text
Expression
   |
   +── Terminal Expression
   |
   +── Non-terminal Expression
```

Possible use cases:

```text
Simple rule engines
Expression evaluators
Domain-specific languages
```

It is less common in ordinary application code than patterns such as Strategy, Factory, or Builder.

---

# 33. Dependency Injection

Dependency Injection means an object receives its dependencies from outside rather than constructing them internally.

Without DI:

```java
class OrderService {

    private PaymentService payment =
        new PaymentService();
}
```

The class directly controls dependency creation.

With constructor injection:

```java
class OrderService {

    private PaymentService payment;

    OrderService(
        PaymentService payment
    ) {

        this.payment = payment;
    }
}
```

Now the dependency is supplied from outside.

This improves:

```text
Testability
Flexibility
Decoupling
Configuration
```

Frameworks such as Spring heavily use Dependency Injection.

---

# 34. SOLID and Design Patterns

Design patterns and SOLID principles often work together.

## Single Responsibility Principle

A class should have a focused responsibility.

Patterns such as:

```text
Strategy
Command
Facade
```

can help organize responsibilities.

---

## Open/Closed Principle

Software should be open for extension while minimizing modification of existing behavior.

Useful patterns include:

```text
Strategy
Decorator
Factory
```

---

## Liskov Substitution Principle

Subtypes should behave consistently with their abstraction.

Patterns based heavily on inheritance should be designed carefully around this principle.

---

## Interface Segregation Principle

Clients should not be forced to depend on methods they do not need.

Patterns often use small interfaces to reduce coupling.

---

## Dependency Inversion Principle

High-level modules should depend on abstractions rather than concrete implementations.

This is strongly related to:

```text
Strategy
Factory
Dependency Injection
```

---

# 35. Composition Over Inheritance

A common design principle is:

```text
Prefer composition when it provides
more flexibility than inheritance.
```

Inheritance:

```text
Class A
   ↑
Class B
```

Composition:

```text
Class B
   |
   +── has-a → Class A
```

Strategy is a strong example.

Instead of:

```text
PaymentService
├── UpiPaymentService
├── CardPaymentService
└── CashPaymentService
```

you can compose:

```text
PaymentService
      |
      +── PaymentStrategy
              |
              +── UPI
              +── Card
              +── Cash
```

Behavior can be changed without changing the context class's inheritance hierarchy.

---

# 36. Design Pattern Selection

Do not choose a pattern simply because it sounds advanced.

Start with:

```text
What problem am I solving?
```

Then ask:

```text
What changes frequently?
What should remain stable?
Who should create the object?
Who should own the responsibility?
How tightly coupled are the classes?
Do I need interchangeable behavior?
Do I need to hide subsystem complexity?
Do I need to adapt an existing API?
```

Examples:

```text
Complex object creation
→ Builder

Object creation selection
→ Factory

Interchangeable behavior
→ Strategy

Incompatible interfaces
→ Adapter

Add behavior dynamically
→ Decorator

Simplify complex subsystem
→ Facade

One-to-many notifications
→ Observer

Request as object
→ Command

Traversal abstraction
→ Iterator
```

---

# 37. Common Mistakes

## ❌ Mistake 1 — Memorizing Patterns Without Problems

Do not memorize only:

```text
Strategy = interface
Factory = if/else
```

Understand:

```text
Problem
→ Structure
→ Consequences
```

---

## ❌ Mistake 2 — Using Patterns Everywhere

A design pattern is not automatically better.

Overengineering can make simple code unnecessarily complicated.

---

## ❌ Mistake 3 — Confusing Factory and Abstract Factory

```text
Factory Method
→ creation mechanism for a product

Abstract Factory
→ family of related products
```

---

## ❌ Mistake 4 — Confusing Adapter and Decorator

```text
Adapter
→ changes interface

Decorator
→ adds behavior
```

---

## ❌ Mistake 5 — Confusing Strategy and State

```text
Strategy
→ client chooses/interchanges behavior

State
→ object's behavior changes according to state
```

---

## ❌ Mistake 6 — Thinking Singleton Means Global Variable

Singleton controls instance creation/access.

It is not simply:

```text
static variable = global variable
```

---

## ❌ Mistake 7 — Ignoring SOLID

Patterns work best when combined with sound design principles.

---

## ❌ Mistake 8 — Copying Pattern Code Without Understanding

The implementation should be adapted to the actual problem.

---

# 38. Top 30 Interview Questions

## 🔥 Q1. What is a design pattern?

A reusable general approach to a recurring software design problem.

---

## 🔥 Q2. Who are the Gang of Four?

Erich Gamma, Richard Helm, Ralph Johnson, and John Vlissides.

---

## 🔥 Q3. How many classic GoF patterns are there?

23.

---

## 🔥 Q4. What are the three GoF categories?

```text
Creational
Structural
Behavioral
```

---

## 🔥 Q5. Name the creational patterns.

```text
Singleton
Factory Method
Abstract Factory
Builder
Prototype
```

---

## 🔥 Q6. Name some structural patterns.

```text
Adapter
Bridge
Composite
Decorator
Facade
Proxy
Flyweight
```

---

## 🔥 Q7. Name some behavioral patterns.

```text
Observer
Strategy
Command
State
Iterator
Template Method
Chain of Responsibility
```

---

## 🔥 Q8. What is Singleton?

A pattern designed to provide a single shared instance according to its defined lifecycle/access rules.

---

## 🔥 Q9. Why is the Singleton constructor private?

To prevent direct external instantiation.

---

## 🔥 Q10. What is Factory?

It encapsulates or centralizes object creation decisions.

---

## 🔥 Q11. Factory vs Abstract Factory?

```text
Factory
→ creation of a product

Abstract Factory
→ creation of related product families
```

---

## 🔥 Q12. What is Builder?

A pattern that separates complex object construction from the final object representation.

---

## 🔥 Q13. What is Adapter?

It converts one interface into another interface expected by the client.

---

## 🔥 Q14. Adapter vs Decorator?

```text
Adapter
→ changes interface

Decorator
→ adds behavior
```

---

## 🔥 Q15. What is Facade?

A simplified interface over a complex subsystem.

---

## 🔥 Q16. What is Proxy?

An intermediary object that controls or mediates access to another object.

---

## 🔥 Q17. What is Strategy?

Encapsulates interchangeable algorithms or behaviors behind a common interface.

---

## 🔥 Q18. Strategy vs State?

```text
Strategy
→ behavior selected/interchanged by context

State
→ behavior changes based on object's state
```

---

## 🔥 Q19. What is Observer?

A one-to-many relationship where observers are notified of changes in a subject.

---

## 🔥 Q20. What is Command?

Encapsulates a request as an object.

---

## 🔥 Q21. What is Template Method?

Defines an algorithm skeleton in a superclass while allowing subclasses to customize certain steps.

---

## 🔥 Q22. What is Iterator?

Provides a standard way to traverse a collection without exposing its internal representation.

---

## 🔥 Q23. What is Composite?

Allows individual objects and compositions of objects to be treated uniformly.

---

## 🔥 Q24. What is Dependency Injection?

Supplying dependencies from outside instead of having a class construct them itself.

---

## 🔥 Q25. Why prefer composition over inheritance?

Composition often provides more flexibility because behavior can be changed by replacing contained objects rather than changing an inheritance hierarchy.

---

## 🔥 Q26. What is Prototype?

Creates objects by copying an existing object's state.

---

## 🔥 Q27. What is Bridge?

Separates abstraction from implementation so both can evolve independently.

---

## 🔥 Q28. What is Chain of Responsibility?

Passes a request through a chain of handlers until one handles it or the chain ends.

---

## 🔥 Q29. Is a design pattern the same as a framework?

No.

```text
Pattern
→ design approach

Framework
→ reusable software infrastructure
```

---

## 🔥 Q30. Should every problem use a design pattern?

No.

Patterns should be introduced when they address an actual design problem and provide useful structure.

---

# 39. 30-Second Interview Answer

> A design pattern is a reusable general solution to a recurring software design problem. The classic Gang of Four patterns are divided into creational, structural, and behavioral categories. Creational patterns such as Factory and Builder deal with object creation, structural patterns such as Adapter and Decorator deal with composition, and behavioral patterns such as Strategy and Observer deal with communication and responsibility. Design patterns are not copy-paste code; they are design templates that should be adapted to the problem.

---

# 40. Cheat Sheet

```text
========================================================
                  JAVA DESIGN PATTERNS
========================================================


GOF
--------------------------------------------------------

Gang of Four:

Erich Gamma
Richard Helm
Ralph Johnson
John Vlissides


Classic GoF Patterns
→ 23


========================================================

CREATIONAL
--------------------------------------------------------

Purpose:
Object creation


1. Singleton
2. Factory Method
3. Abstract Factory
4. Builder
5. Prototype


Memory:

"How should objects be created?"


========================================================

STRUCTURAL
--------------------------------------------------------

Purpose:
Object/class composition


1. Adapter
2. Bridge
3. Composite
4. Decorator
5. Facade
6. Flyweight
7. Proxy


Memory:

"How should objects be connected?"


========================================================

BEHAVIORAL
--------------------------------------------------------

Purpose:
Communication + responsibility


1. Chain of Responsibility
2. Command
3. Interpreter
4. Iterator
5. Mediator
6. Memento
7. Observer
8. State
9. Strategy
10. Template Method
11. Visitor


Memory:

"How should objects communicate?"


========================================================

SINGLETON
--------------------------------------------------------

One shared instance


private constructor
+
static access


========================================================

FACTORY
--------------------------------------------------------

Encapsulates creation decision


Client
 ↓
Factory
 ↓
Concrete Object


========================================================

ABSTRACT FACTORY
--------------------------------------------------------

Creates related product families


Factory
 ↓
Product A
Product B
Product C


========================================================

BUILDER
--------------------------------------------------------

Complex object construction


new Builder()
    .x(...)
    .y(...)
    .build()


========================================================

PROTOTYPE
--------------------------------------------------------

Copy existing object


Existing
   ↓
 Copy
   ↓
New object


========================================================

ADAPTER
--------------------------------------------------------

Changes interface


Client
 ↓
Adapter
 ↓
Existing class


========================================================

DECORATOR
--------------------------------------------------------

Adds behavior


Object
 ↓
Decorator
 ↓
Decorator


========================================================

FACADE
--------------------------------------------------------

Simplifies subsystem


Client
 ↓
Facade
 ↓
Subsystem


========================================================

PROXY
--------------------------------------------------------

Controls access


Client
 ↓
Proxy
 ↓
Real object


========================================================

COMPOSITE
--------------------------------------------------------

Tree structure


Component
 ├── Leaf
 └── Composite
       ├── Leaf
       └── Leaf


========================================================

BRIDGE
--------------------------------------------------------

Separates:

Abstraction
     +
Implementation


========================================================

OBSERVER
--------------------------------------------------------

One-to-many notification


Subject
 ├── Observer
 ├── Observer
 └── Observer


========================================================

STRATEGY
--------------------------------------------------------

Interchangeable behavior


Context
   ↓
Strategy
   ↓
Implementation


========================================================

COMMAND
--------------------------------------------------------

Request becomes object


Client
 ↓
Command
 ↓
Receiver


========================================================

TEMPLATE METHOD
--------------------------------------------------------

Algorithm skeleton


Template
 ├── Step A
 ├── Step B
 └── Step C

Subclass customizes steps.


========================================================

STATE
--------------------------------------------------------

Behavior depends on state


Object
 ↓
State A / B / C


========================================================

ITERATOR
--------------------------------------------------------

Traverse collection


hasNext()
next()


========================================================

CHAIN OF RESPONSIBILITY
--------------------------------------------------------

Request
 ↓
Handler A
 ↓
Handler B
 ↓
Handler C


========================================================

MEDIATOR
--------------------------------------------------------

Central communication


A ─┐
B ─┤
C ─┼── Mediator
D ─┘


========================================================

MEMENTO
--------------------------------------------------------

Save + restore state


Object
 ↓
Memento
 ↓
Restore


========================================================

VISITOR
--------------------------------------------------------

Add operations
without changing
object structure.


========================================================

INTERPRETER
--------------------------------------------------------

Represent and evaluate
language/grammar expressions.


========================================================

DEPENDENCY INJECTION
--------------------------------------------------------

Dependency supplied
from outside.


Class
 ↑
Dependency


========================================================

IMPORTANT COMPARISONS
--------------------------------------------------------

Factory
→ object creation


Builder
→ complex object construction


Adapter
→ interface conversion


Decorator
→ behavior addition


Facade
→ subsystem simplification


Proxy
→ access control/intermediary


Strategy
→ interchangeable behavior


State
→ state-dependent behavior


Observer
→ notification


Command
→ request as object


========================================================

DESIGN PRINCIPLES
--------------------------------------------------------

SOLID
+
Composition over inheritance
+
Low coupling
+
High cohesion


========================================================

MOST IMPORTANT RULE
--------------------------------------------------------

Do not use a pattern
because it sounds advanced.

Use it because
it solves a real design problem.


========================================================
```

---

# 41. Final Mental Model

```text
                         DESIGN PATTERNS
                                |
             +------------------+------------------+
             |                  |                  |
             ↓                  ↓                  ↓
        CREATIONAL         STRUCTURAL         BEHAVIORAL
             |                  |                  |
             ↓                  ↓                  ↓
         Creation          Composition       Communication
             |                  |                  |
       +-----+-----+      +-----+------+      +----+-----+
       |     |     |      |     |      |      |    |     |
       ↓     ↓     ↓      ↓     ↓      ↓      ↓    ↓     ↓
 Singleton Factory Builder Adapter Decorator Facade Strategy
 Abstract  Prototype        Proxy Composite    Observer Command
 Factory                                      State Iterator
                                              Template
                                              Chain
                                              Mediator
                                              Memento
                                              Visitor
                                              Interpreter


========================================================

PROBLEM-FIRST THINKING


What problem?
      |
      ↓
What changes?
      |
      ↓
What should remain stable?
      |
      ↓
Where should responsibility live?
      |
      ↓
Which objects should know about each other?
      |
      ↓
Choose a suitable design
      |
      ↓
Pattern if useful


========================================================

COMMON MAPPINGS


Need controlled creation
        ↓
Factory


Need complex construction
        ↓
Builder


Need one shared instance
        ↓
Singleton


Need incompatible interface compatibility
        ↓
Adapter


Need dynamically added behavior
        ↓
Decorator


Need simplified subsystem
        ↓
Facade


Need controlled access
        ↓
Proxy


Need interchangeable algorithms
        ↓
Strategy


Need state-dependent behavior
        ↓
State


Need one-to-many notifications
        ↓
Observer


Need request as an object
        ↓
Command


Need collection traversal
        ↓
Iterator


Need request processing chain
        ↓
Chain of Responsibility


========================================================

PATTERN vs ALGORITHM


Algorithm
   ↓
How to compute


Design Pattern
   ↓
How to organize software


Framework
   ↓
Reusable software infrastructure


Architecture
   ↓
System-level structure


========================================================

FINAL MEMORY


Creational
→ "How do I create objects?"


Structural
→ "How do I connect objects?"


Behavioral
→ "How do objects communicate?"


========================================================

ONE-LINE MEMORY


"Design patterns are reusable design ideas,
not ready-made code."


========================================================
```

---

# 🏁 Final Revision Checklist

- [ ] Definition of design pattern
- [ ] Why design patterns are used
- [ ] Pattern vs algorithm
- [ ] Pattern vs framework
- [ ] Pattern vs architecture
- [ ] Gang of Four
- [ ] 23 classic GoF patterns
- [ ] Creational patterns
- [ ] Structural patterns
- [ ] Behavioral patterns
- [ ] Singleton
- [ ] Factory Method
- [ ] Abstract Factory
- [ ] Builder
- [ ] Prototype
- [ ] Adapter
- [ ] Bridge
- [ ] Composite
- [ ] Decorator
- [ ] Facade
- [ ] Proxy
- [ ] Observer
- [ ] Strategy
- [ ] Command
- [ ] Template Method
- [ ] State
- [ ] Iterator
- [ ] Chain of Responsibility
- [ ] Mediator
- [ ] Memento
- [ ] Visitor
- [ ] Interpreter
- [ ] Dependency Injection
- [ ] SOLID relationship
- [ ] Composition over inheritance
- [ ] Pattern selection
- [ ] Common mistakes
- [ ] Important pattern comparisons
- [ ] Interview questions

---

# 🧠 One-Line Memory

```text
Design Pattern
= Reusable design approach
for a recurring software problem.

Creational
→ creation

Structural
→ composition

Behavioral
→ communication

Most important:
Understand the PROBLEM first,
then choose the PATTERN.
```
