# ☕ Java OOP — Introduction

> **Object-Oriented Programming (OOP) is a programming paradigm that organizes software around objects, where objects combine data and the behavior that operates on that data.**

---

## 📚 Table of Contents

* [What is OOP?](#-what-is-oop)
* [Why Do We Need OOP?](#-why-do-we-need-oop)
* [Procedural vs Object-Oriented Programming](#-procedural-vs-object-oriented-programming)
* [Real-World Analogy](#-real-world-analogy)
* [Core Concepts of OOP](#-core-concepts-of-oop)
* [Object](#-object)
* [Class](#-class)
* [Encapsulation](#-encapsulation)
* [Inheritance](#-inheritance)
* [Polymorphism](#-polymorphism)
* [Abstraction](#-abstraction)
* [How OOP Works in Java](#-how-oop-works-in-java)
* [OOP and Java](#-oop-and-java)
* [Is Java Completely Object-Oriented?](#-is-java-completely-object-oriented)
* [Advantages of OOP](#-advantages-of-oop)
* [Disadvantages of OOP](#-disadvantages-of-oop)
* [Common Misconceptions](#-common-misconceptions)
* [Interview Questions](#-interview-questions)
* [Top 10 Interview Questions](#-top-10-interview-questions)
* [Quick Revision](#-quick-revision)
* [30-Second Interview Answer](#-30-second-interview-answer)
* [Memory Trick](#-memory-trick)

---

# 🧠 What is OOP?

**OOP stands for Object-Oriented Programming.**

It is a programming paradigm in which a program is designed around **objects** rather than simply around a sequence of functions.

An object generally contains:

```text
┌──────────────────────────┐
│          Object          │
├──────────────────────────┤
│ Data / State             │
│                          │
│ Behavior / Methods       │
└──────────────────────────┘
```

For example, consider a `Car`.

### State / Data

```text
brand
color
speed
model
```

### Behavior

```text
start()
accelerate()
brake()
stop()
```

So we can think of:

```text
Car
 ├── Data
 │    ├── brand
 │    ├── color
 │    └── speed
 │
 └── Behavior
      ├── start()
      ├── accelerate()
      └── brake()
```

The fundamental idea is:

> **Keep related data and behavior together and model the program as interacting objects.**

---

# 🤔 Why Do We Need OOP?

Before OOP became dominant, many programs were written using **procedural programming**.

In procedural programming, the focus is mainly on:

```text
Data
  ↓
Functions
  ↓
Operations
```

As applications become larger, managing thousands of variables and functions separately can become difficult.

For example, imagine an employee management system.

Without a structured object-oriented design, you might have:

```java
String employeeName;
int employeeAge;
double employeeSalary;

void calculateSalary() {
    // ...
}

void displayEmployee() {
    // ...
}
```

Now imagine:

```text
10 employees
100 employees
10,000 employees
```

Managing individual variables becomes increasingly difficult.

OOP allows us to group related information and behavior:

```java
class Employee {

    String name;
    int age;
    double salary;

    void displayEmployee() {
        // ...
    }

    void calculateSalary() {
        // ...
    }
}
```

Now each employee can be represented as an object:

```java
Employee e1 = new Employee();
Employee e2 = new Employee();
Employee e3 = new Employee();
```

This makes large programs easier to structure and maintain.

---

# 🆚 Procedural vs Object-Oriented Programming

| Procedural Programming                    | Object-Oriented Programming                                 |
| ----------------------------------------- | ----------------------------------------------------------- |
| Focuses mainly on functions               | Focuses on objects                                          |
| Data and functions can be separate        | Data and behavior are grouped                               |
| Usually follows a top-down approach       | Commonly designed around interacting objects                |
| Data protection depends heavily on design | Encapsulation provides stronger data hiding                 |
| Reusability mainly through functions      | Reusability through classes, inheritance, composition, etc. |
| Suitable for smaller/simple programs      | Well suited to large, complex systems                       |
| Example: C                                | Example: Java, C++, C#                                      |

> ⚠️ This is a conceptual comparison, not an absolute rule. Procedural languages can have strong modular designs, and OOP languages can contain procedural-style code.

---

# 🌎 Real-World Analogy

Think about a **Bank Account**.

A bank account has:

### State

```text
accountNumber
accountHolderName
balance
```

### Behavior

```text
deposit()
withdraw()
checkBalance()
```

Instead of treating these as unrelated pieces, OOP models them together:

```text
             BankAccount
          ┌─────────────────┐
          │ State           │
          │                 │
          │ accountNumber   │
          │ accountHolder   │
          │ balance         │
          │                 │
          │ Behavior        │
          │                 │
          │ deposit()       │
          │ withdraw()      │
          │ checkBalance()  │
          └─────────────────┘
```

This is the basic philosophy behind object-oriented design.

---

# 🧱 Core Concepts of OOP

The four commonly taught pillars of OOP are:

```text
             OOP
              │
      ┌───────┼────────┐
      │       │        │
      ▼       ▼        ▼
 Encapsulation Inheritance Polymorphism
              │
              ▼
          Abstraction
```

More clearly:

### 1️⃣ Encapsulation

> **Bundling data and the methods that operate on that data, while controlling access to the internal state.**

Example:

```java
class BankAccount {

    private double balance;

    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
        }
    }

    public double getBalance() {
        return balance;
    }
}
```

The `balance` is not directly accessible from outside.

---

### 2️⃣ Inheritance

> **A mechanism through which one class can acquire and extend properties and behavior of another class.**

Example:

```java
class Animal {

    void eat() {
        System.out.println("Eating...");
    }
}

class Dog extends Animal {

    void bark() {
        System.out.println("Barking...");
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

`Dog` inherits `eat()` from `Animal`.

---

### 3️⃣ Polymorphism

**Poly = Many**

**Morph = Forms**

Therefore:

> **Polymorphism means the ability of the same interface/reference/operation to represent or invoke different behavior depending on the context.**

Two major forms in Java:

```text
Polymorphism
     │
     ├── Compile-time
     │      └── Method Overloading
     │
     └── Runtime
            └── Method Overriding
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
        System.out.println("Dog barks");
    }
}
```

Then:

```java
Animal a = new Dog();

a.sound();
```

Output:

```text
Dog barks
```

The reference type is `Animal`, but the actual object is `Dog`.

---

### 4️⃣ Abstraction

> **Abstraction means exposing the essential functionality while hiding unnecessary implementation details.**

Example:

```java
abstract class Vehicle {

    abstract void start();
}
```

The user knows that a vehicle can be started, but the exact implementation can differ.

```java
class Car extends Vehicle {

    @Override
    void start() {
        System.out.println("Car starts using engine");
    }
}
```

---

# 📦 Object

An **object** is a runtime entity created from a class.

An object generally has three important characteristics:

```text
Object
 ├── State
 ├── Behavior
 └── Identity
```

### State

The current values of its fields.

```java
name = "Rahul";
age = 22;
```

### Behavior

What the object can do.

```java
study();
work();
sleep();
```

### Identity

A unique identity that distinguishes one object from another.

Example:

```java
Student s1 = new Student();
Student s2 = new Student();
```

Even if `s1` and `s2` contain identical data, they are separate object instances.

---

# 🏗️ Class

A **class** is a blueprint or template from which objects can be created.

Example:

```java
class Student {

    String name;
    int age;

    void study() {
        System.out.println("Student is studying");
    }
}
```

Creating objects:

```java
Student s1 = new Student();
Student s2 = new Student();
```

Conceptually:

```text
             Student Class
          ┌──────────────────┐
          │ name             │
          │ age              │
          │ study()          │
          └────────┬─────────┘
                   │
             creates objects
             ┌─────┴─────┐
             ▼           ▼
            s1           s2
```

> 💡 A class itself is not simply "an object." A class is a type definition, while an object is an instance of that type.

---

# 🔐 Encapsulation

Encapsulation is primarily about **controlling access to an object's internal state**.

Example:

```java
class Employee {

    private double salary;

    public void setSalary(double salary) {

        if (salary >= 0) {
            this.salary = salary;
        }
    }

    public double getSalary() {
        return salary;
    }
}
```

Without encapsulation:

```java
employee.salary = -50000;
```

With controlled access:

```java
employee.setSalary(-50000);
```

The class can reject invalid data.

### Key idea

```text
Encapsulation
      ↓
Protect internal state
      ↓
Control access
      ↓
Maintain valid object state
```

---

# 🧬 Inheritance

Inheritance allows a new class to reuse and extend an existing class.

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

`Car` gets the inherited `start()` method.

### Important

Inheritance represents an **IS-A** relationship.

```text
Car IS-A Vehicle
Dog IS-A Animal
Manager IS-A Employee
```

---

# 🔄 Polymorphism

Polymorphism allows one common type/interface to work with different implementations.

Example:

```java
Animal a1 = new Dog();
Animal a2 = new Cat();

a1.sound();
a2.sound();
```

Possible output:

```text
Dog barks
Cat meows
```

Same method call:

```java
sound()
```

Different runtime behavior.

---

# 🎭 Abstraction

Abstraction focuses on:

> **What an object does rather than how it does it.**

For example:

```java
interface Payment {

    void pay();
}
```

Different implementations can provide different behavior:

```java
class UPI implements Payment {

    public void pay() {
        System.out.println("Payment through UPI");
    }
}

class Card implements Payment {

    public void pay() {
        System.out.println("Payment through Card");
    }
}
```

The caller can work with:

```java
Payment payment;
```

without depending on the concrete implementation.

---

# ⚙️ How OOP Works in Java

Consider:

```java
class Student {

    String name;

    void study() {
        System.out.println("Studying...");
    }
}
```

Then:

```java
Student s = new Student();
```

Conceptually, several things happen.

### Step 1 — Class loading

When the JVM needs the class, Java's class-loading mechanism loads the class information.

### Step 2 — Object creation

The `new` operator requests creation of an object.

```java
new Student();
```

The object is created in the JVM-managed heap.

### Step 3 — Reference assignment

```java
Student s = new Student();
```

Conceptually:

```text
Stack                    Heap

s ────────────────────► Student Object
                         ┌─────────────┐
                         │ name        │
                         │ ...         │
                         └─────────────┘
```

The local variable `s` holds a reference to the object.

> ⚠️ The exact JVM implementation details are more nuanced than this simplified diagram. Java language specifications and JVM specifications should be distinguished from implementation-specific details.

---

# ☕ OOP and Java

Java was designed as a strongly object-oriented, class-based programming language.

Most Java application code is organized around classes and objects.

Example:

```java
class Employee {

    private String name;

    void work() {
        System.out.println(name + " is working");
    }
}
```

Java provides mechanisms supporting:

```text
Classes
Objects
Encapsulation
Inheritance
Polymorphism
Abstraction
Interfaces
Composition
Access Control
```

---

# ❓ Is Java Completely Object-Oriented?

### Short answer:

**No, Java is not considered a purely object-oriented language.**

One major reason is that Java supports **primitive data types**.

Examples:

```java
int
char
double
boolean
byte
short
long
float
```

These are not objects.

For example:

```java
int age = 20;
```

`age` is a primitive value, not an object reference.

Java provides wrapper classes when an object representation is needed:

```java
int     → Integer
double  → Double
char    → Character
boolean → Boolean
```

Example:

```java
Integer x = 10;
```

Here Java's **autoboxing** converts the primitive value into an `Integer` object representation.

### Other practical reasons

Java also supports:

* `static` members
* primitive types
* operators
* procedural-style code inside methods

Therefore:

> **Java is object-oriented, but it is not a pure object-oriented language.**

---

# 🧩 OOP Relationships

OOP is not limited to the four pillars.

Objects can also have relationships.

### IS-A

Usually represented through inheritance.

```text
Dog IS-A Animal
```

### HAS-A

Usually represented through composition/aggregation.

```text
Car HAS-A Engine
```

Example:

```java
class Engine {
}

class Car {

    Engine engine;
}
```

These relationships become extremely important when designing larger systems.

---

# 🚀 Advantages of OOP

## 1. 🔄 Reusability

Existing classes and components can be reused.

Inheritance, composition, interfaces, and other mechanisms support reuse.

---

## 2. 🔐 Data Protection

Encapsulation can restrict direct access to internal state.

---

## 3. 🧩 Modularity

Large systems can be divided into smaller classes and components.

---

## 4. 🛠️ Maintainability

Well-designed objects can make changes easier to isolate.

---

## 5. 📈 Scalability

OOP can provide useful structures for large applications.

---

## 6. 🔁 Flexibility

Polymorphism allows code to work with common abstractions.

---

## 7. 🧠 Real-World Modeling

Many domains can naturally be represented as interacting entities.

Examples:

```text
Banking
E-commerce
Hospital Management
Food Ordering
HR Management
Library Management
```

---

# ⚠️ Disadvantages of OOP

OOP is powerful, but it is not automatically the best solution for every problem.

### 1. More design overhead

Small programs can become unnecessarily complicated if too many classes are introduced.

### 2. Memory overhead

Objects and associated runtime structures can require additional memory compared with some simpler procedural approaches.

### 3. Learning curve

Understanding:

```text
Inheritance
Polymorphism
Abstraction
Composition
Interfaces
Design principles
```

takes time.

### 4. Poor design can become complex

Bad inheritance hierarchies or excessive abstraction can make a system harder to understand.

### 5. Not every problem naturally maps to objects

Some problems may be expressed more naturally using procedural, functional, or other paradigms.

---

# ⚠️ Common Misconceptions

## ❌ "OOP means everything must be an object."

Not necessarily.

Java itself has primitives:

```java
int x = 10;
```

---

## ❌ "Class and object are the same thing."

No.

```text
Class  → Type / definition
Object → Instance of that type
```

---

## ❌ "Inheritance is the only way to achieve code reuse."

No.

Composition is often a powerful alternative.

```java
class Car {

    private Engine engine;
}
```

---

## ❌ "Encapsulation simply means private variables."

Not exactly.

`private` is an important access-control mechanism, but encapsulation is the broader design concept of **controlling access to and maintaining the integrity of an object's state and behavior**.

---

## ❌ "Abstraction means hiding all code."

No.

Abstraction hides **unnecessary implementation details** from the user of an abstraction.

---

## ❌ "Polymorphism only means method overloading."

No.

Java supports both commonly discussed forms:

```text
Compile-time → Overloading
Runtime      → Overriding
```

---

## ❌ "OOP always makes programs better."

No.

Good design matters more than simply using OOP.

---

# 💼 Interview Questions

## 🟢 Basic Questions

### 1. What is OOP?

OOP is a programming paradigm that organizes software around objects containing state and behavior.

---

### 2. What are the four pillars of OOP?

```text
Encapsulation
Inheritance
Polymorphism
Abstraction
```

---

### 3. What is a class?

A class is a type definition that describes the data and behavior that its objects can have.

---

### 4. What is an object?

An object is a runtime instance of a class.

---

### 5. What is encapsulation?

Encapsulation is the bundling of state and behavior with controlled access to the internal state.

---

### 6. What is inheritance?

Inheritance allows a class to acquire and extend accessible members of another class.

---

### 7. What is polymorphism?

Polymorphism allows a common type or operation to work with different implementations or forms.

---

### 8. What is abstraction?

Abstraction exposes essential behavior while hiding unnecessary implementation details.

---

### 9. Is Java completely object-oriented?

No. Java has primitive data types and other language features that mean it is not a purely object-oriented language.

---

### 10. What is the difference between class and object?

```text
Class  → Defines a type
Object → Runtime instance of that type
```

---

# 🟡 Intermediate Interview Questions

### 11. Why is OOP useful for large applications?

Because it provides mechanisms such as modularity, encapsulation, abstraction, polymorphism, and reuse that can help structure complex systems.

---

### 12. What is an IS-A relationship?

It represents inheritance.

```text
Dog IS-A Animal
```

---

### 13. What is a HAS-A relationship?

It represents a relationship where one object contains or uses another object.

```text
Car HAS-A Engine
```

---

### 14. What is the difference between abstraction and encapsulation?

| Abstraction                                             | Encapsulation                                         |
| ------------------------------------------------------- | ----------------------------------------------------- |
| Focuses on essential behavior                           | Focuses on controlling access to state/implementation |
| Answers "What?"                                         | Often addresses "How is access controlled?"           |
| Uses abstract classes/interfaces among other mechanisms | Uses access modifiers and class design                |
| Hides unnecessary implementation details                | Protects and manages internal state                   |

---

### 15. What is the difference between inheritance and composition?

**Inheritance:**

```text
IS-A
```

**Composition:**

```text
HAS-A
```

Example:

```java
class Dog extends Animal {
}
```

vs.

```java
class Car {

    private Engine engine;
}
```

---

### 16. Why is composition often preferred over inheritance?

Composition can reduce tight coupling and allow behavior to be assembled from independent components.

However, this does **not** mean inheritance is always bad.

---

### 17. What is runtime polymorphism?

Runtime polymorphism occurs when an overridden method is selected based on the actual object at runtime.

```java
Animal a = new Dog();

a.sound();
```

---

### 18. What is compile-time polymorphism?

Method overloading is commonly described as compile-time polymorphism.

```java
void add(int a, int b)

void add(int a, int b, int c)
```

The compiler determines which overloaded method applies.

---

# 🔴 Advanced / Tricky Interview Questions

### 19. Is a class an object in Java?

A class and an object are conceptually different.

A class defines a type.

An object is an instance of that type.

However, Java's runtime representation of classes involves objects such as `Class` objects, so avoid saying simply "a class can never be represented by an object."

---

### 20. Does encapsulation require getters and setters?

**No.**

Getters and setters are common techniques, but encapsulation is broader.

A class may expose behavior without exposing direct accessors for every field.

Example:

```java
class BankAccount {

    private double balance;

    public void deposit(double amount) {

        if (amount > 0) {
            balance += amount;
        }
    }
}
```

There does not need to be a public `setBalance()`.

---

### 21. Is inheritance necessary for polymorphism?

**No.**

Polymorphism can also arise through interfaces and other forms of substitutability.

Example:

```java
interface Payment {
    void pay();
}

class UPI implements Payment {
    public void pay() {
        System.out.println("UPI payment");
    }
}
```

---

### 22. Can OOP be used without inheritance?

**Yes.**

A system can heavily use:

```text
Classes
Encapsulation
Composition
Interfaces
Abstraction
Polymorphism
```

without creating deep inheritance hierarchies.

---

### 23. Which is better: inheritance or composition?

There is no universal winner.

Use inheritance when there is a genuine substitutable **IS-A** relationship.

Use composition when an object should **HAVE-A** or delegate to another component.

---

### 24. Why does Java favor composition in many designs?

Because composition can provide flexible behavior without creating strong inheritance coupling.

For example:

```java
class Car {

    private Engine engine;

    Car(Engine engine) {
        this.engine = engine;
    }
}
```

The `Car` can work with an appropriate `Engine` implementation without inheriting from it.

---

### 25. Is OOP a programming language?

❌ No.

OOP is a **programming paradigm**.

Java is a programming language that supports object-oriented programming.

---

### 26. Is Java 100% OOP?

❌ No.

The strongest simple reason:

```java
int x = 10;
```

`int` is a primitive type, not a class/object.

---

### 27. What makes an object different from another object?

An object has identity in addition to its state and behavior.

Two objects may have identical field values while still being distinct instances.

---

### 28. Can a program use both procedural and OOP concepts?

Yes.

Java programs can contain methods with procedural logic while the overall application is organized using classes and objects.

---

### 29. Is encapsulation the same as data hiding?

They are closely related but not identical.

```text
Encapsulation → broader design concept
Data hiding   → restricting visibility/access
```

Access modifiers such as `private` are tools used to achieve data hiding and support encapsulation.

---

### 30. Does OOP guarantee code reusability?

No.

OOP provides mechanisms that **can facilitate reuse**, but poor design can still produce duplicated or tightly coupled code.

---

# 🔥 Top 10 OOP Interview Questions

> ⭐ These are the questions you should be able to answer confidently before moving forward.

### 1️⃣ What is OOP?

OOP is a programming paradigm that organizes software around objects containing state and behavior.

---

### 2️⃣ What are the four pillars of OOP?

```text
Encapsulation
Inheritance
Polymorphism
Abstraction
```

---

### 3️⃣ What is the difference between a class and an object?

```text
Class  → Defines a type
Object → Instance of that type
```

---

### 4️⃣ What is encapsulation?

Bundling state and behavior while controlling access to the internal state.

---

### 5️⃣ What is inheritance?

A mechanism through which a class can acquire and extend accessible members of another class.

---

### 6️⃣ What is polymorphism?

The ability to use a common type or operation with different implementations/forms.

---

### 7️⃣ What is abstraction?

Exposing essential behavior while hiding unnecessary implementation details.

---

### 8️⃣ Is Java completely object-oriented?

No. Java has primitive types and therefore is not a purely object-oriented language.

---

### 9️⃣ Inheritance vs Composition?

```text
Inheritance → IS-A
Composition → HAS-A
```

---

### 🔟 Why is OOP useful?

It can help organize complex software through:

```text
Modularity
Encapsulation
Abstraction
Reuse
Polymorphism
Maintainability
```

---

# 🧠 Quick Revision

```text
                    ☕ OOP
                      │
        ┌─────────────┼─────────────┐
        │             │             │
     OBJECT         CLASS       RELATIONSHIPS
        │             │             │
   State            Blueprint     IS-A / HAS-A
   Behavior             │
   Identity             │
                        ▼
                Creates Objects
```

### Four Pillars

```text
🔐 Encapsulation
    ↓
Control access to internal state

🧬 Inheritance
    ↓
Reuse/extend class behavior

🔄 Polymorphism
    ↓
One common type → different behavior

🎭 Abstraction
    ↓
Expose essential behavior
Hide unnecessary details
```

### Most Important Relationships

```text
IS-A   → Inheritance

HAS-A  → Composition / Aggregation
```

### Java

```text
Java
 ├── Object-Oriented
 ├── Class-Based
 ├── Supports OOP principles
 └── Not Pure OOP
       ↓
    Primitive types
```

---

# ⚡ 30-Second Interview Answer

> **"OOP stands for Object-Oriented Programming. It is a programming paradigm where software is organized around objects that contain state and behavior. Java supports OOP through classes, objects, encapsulation, inheritance, polymorphism, and abstraction. Encapsulation controls access to internal state, inheritance enables reuse and extension, polymorphism allows different implementations to be used through common types, and abstraction hides unnecessary implementation details. OOP helps structure large applications into modular and maintainable components. Java is object-oriented, but it is not a purely object-oriented language because it also supports primitive data types."**

---

# 🏆 Memory Trick

Remember the four pillars as:

```text
🔐 E → Encapsulation
🧬 I → Inheritance
🔄 P → Polymorphism
🎭 A → Abstraction

             EIPA
```

Or remember the questions they answer:

```text
Encapsulation  → "Who can access my data?"
Inheritance    → "What can I reuse/extend?"
Polymorphism   → "What form/behavior can this take?"
Abstraction    → "What should I expose?"
```

### One-line master memory

> 🧠 **OOP = Model the problem as objects, protect their state, reuse/compose behavior, and program against abstractions.**

---

# 🎯 Final Takeaway

OOP is not simply:

```text
Class + Object
```

It is a **design approach** for organizing software.

The important concepts are:

```text
Class
 ↓
Object
 ↓
Encapsulation
 ↓
Inheritance / Composition
 ↓
Polymorphism
 ↓
Abstraction
 ↓
Object Relationships
 ↓
Maintainable Software
```

> ☕ **Mastering OOP means understanding not just what each concept is, but why it exists, how Java implements it, when to use it, and what problems it solves.**
