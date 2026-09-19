# ☕ Java OOP — Complete Interview Questions & Answers

> **Final OOP interview revision sheet covering all 15 topics from the Java OOP section.**
>
> Questions are selected based on commonly asked concepts, interview traps, and important follow-ups.

---

# 📚 Topics Covered

```text
01. OOP Introduction
02. Class and Object
03. Encapsulation
04. Inheritance
05. Polymorphism
06. Method Overloading
07. Method Overriding
08. Abstraction
09. Interface
10. Abstract Class
11. Constructor
12. this and super
13. static
14. final
15. Association, Aggregation & Composition
```

---

# 01 — OOP Introduction

## Q1. What is OOP?

### Answer

OOP stands for **Object-Oriented Programming**.

It is a programming paradigm that organizes software around **objects**, which contain:

```text
State      → Data / fields
Behavior   → Methods
```

Java uses OOP concepts such as:

```text
Encapsulation
Inheritance
Polymorphism
Abstraction
```

---

## Q2. What are the four pillars of OOP?

### Answer

The four major pillars are:

```text
1. Encapsulation
2. Inheritance
3. Polymorphism
4. Abstraction
```

Memory trick:

```text
EIPA
```

---

## Q3. What is encapsulation?

### Answer

Encapsulation is the process of **bundling data and the methods that operate on that data into a class while controlling access to the internal state**.

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

The `balance` cannot be directly modified from outside.

---

## Q4. What is inheritance?

### Answer

Inheritance allows a child class to acquire accessible properties and behavior from a parent class.

```java
class Animal {
    void eat() {
        System.out.println("Eating");
    }
}

class Dog extends Animal {
}
```

Now:

```text
Dog IS-A Animal
```

---

## Q5. What is polymorphism?

### Answer

Polymorphism means **one interface/reference can represent different forms of behavior**.

Two commonly discussed forms are:

```text
Compile-time polymorphism → Overloading
Runtime polymorphism      → Overriding
```

---

## Q6. What is abstraction?

### Answer

Abstraction means exposing the **essential behavior** while hiding unnecessary implementation details.

Java provides abstraction mainly through:

```text
Abstract classes
Interfaces
```

---

## Q7. Is Java completely object-oriented?

### Answer

No.

Java is strongly object-oriented, but it is not considered purely object-oriented because it supports **primitive data types** such as:

```java
int
char
boolean
double
```

These are not objects.

---

## Q8. What is IS-A vs HAS-A?

### Answer

```text
IS-A
 ↓
Inheritance

HAS-A
 ↓
Object relationship / composition / aggregation
```

Example:

```java
class Dog extends Animal {
}
```

```text
Dog IS-A Animal
```

Example:

```java
class Car {
    Engine engine;
}
```

```text
Car HAS-A Engine
```

---

# 02 — Class and Object

## Q1. What is a class?

### Answer

A class is a **blueprint/template** that defines the state and behavior that its objects can have.

```java
class Student {

    String name;

    void study() {
        System.out.println("Studying");
    }
}
```

---

## Q2. What is an object?

### Answer

An object is a **runtime instance of a class**.

```java
Student s = new Student();
```

Here:

```text
Student → class
s       → reference
new Student() → object
```

---

## Q3. Class vs Object?

### Answer

| Class                                    | Object                         |
| ---------------------------------------- | ------------------------------ |
| Blueprint                                | Instance                       |
| Logical definition                       | Runtime entity                 |
| Does not represent one specific instance | Represents a specific instance |
| Defines state/behavior                   | Has actual state               |

---

## Q4. What happens when `new` is used?

### Answer

The `new` operator requests creation of an object and returns a reference to that object.

For:

```java
Student s = new Student();
```

conceptually:

```text
new Student()
     ↓
Object created
     ↓
Constructor executes
     ↓
Reference returned
     ↓
s stores reference
```

---

## Q5. Where are Java objects stored?

### Answer

Objects are generally allocated in the **heap**.

Local reference variables are typically associated with the executing thread's stack frame when they are local variables.

Example:

```java
Student s = new Student();
```

Conceptually:

```text
Stack
  ↓
s ───────────────┐
                 ↓
              Heap
                 ↓
             Student object
```

---

## Q6. Can multiple references point to one object?

### Answer

Yes.

```java
Student s1 = new Student();

Student s2 = s1;
```

Now:

```text
s1 ──┐
     ├──> Same object
s2 ──┘
```

Changing the object's state through one reference can be observed through the other.

---

## Q7. What is the difference between an object and a reference?

### Answer

The **object** is the actual runtime entity.

The **reference** is a value used to locate/access that object.

```java
Student s = new Student();
```

```text
s              → reference
new Student()   → object
```

---

# 03 — Encapsulation

## Q1. How is encapsulation implemented in Java?

### Answer

Commonly using:

```text
private fields
+
controlled public/protected methods
```

Example:

```java
class Employee {

    private double salary;

    public double getSalary() {
        return salary;
    }

    public void setSalary(double salary) {
        if (salary >= 0) {
            this.salary = salary;
        }
    }
}
```

---

## Q2. Why should fields be private?

### Answer

To prevent uncontrolled direct access and allow the class to enforce rules when its state changes.

For example:

```java
account.balance = -50000;
```

can be prevented by keeping `balance` private and validating through methods.

---

## Q3. Is encapsulation the same as data hiding?

### Answer

They are closely related but not identical.

```text
Encapsulation
→ Bundling data + behavior and controlling access

Data hiding
→ Restricting direct access to implementation details
```

Access modifiers such as `private` help achieve data hiding.

---

## Q4. Is a getter and setter always required for encapsulation?

### Answer

No.

Encapsulation does not mean every private field must have a getter and setter.

A class can expose behavior instead:

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

---

## Q5. Encapsulation vs Abstraction?

### Answer

```text
Encapsulation
→ Controls access to state/implementation

Abstraction
→ Hides unnecessary implementation complexity
  and exposes essential behavior
```

Example:

```text
private balance
→ Encapsulation

pay()
→ Abstraction of payment process
```

---

# 04 — Inheritance

## Q1. What is inheritance?

### Answer

Inheritance allows a subclass to inherit accessible members and behavior from a superclass.

```java
class Animal {
    void eat() {
    }
}

class Dog extends Animal {
}
```

---

## Q2. What types of inheritance are supported through Java classes?

### Answer

Java classes directly support:

```text
Single
Multilevel
Hierarchical
```

Java does **not** support multiple inheritance of classes.

---

## Q3. Why doesn't Java support multiple inheritance through classes?

### Answer

A major reason is ambiguity.

Suppose:

```text
        A
       / \
      B   C
       \ /
        D
```

If both `B` and `C` provide the same method, it becomes ambiguous which implementation `D` should inherit.

This is commonly known as the **diamond problem**.

Java avoids this for classes by allowing a class to extend only one class.

---

## Q4. Can a class extend multiple classes?

### Answer

No.

This is invalid:

```java
class C extends A, B {
}
```

A class can directly extend only one class.

---

## Q5. Can an interface extend multiple interfaces?

### Answer

Yes.

```java
interface A {
}

interface B {
}

interface C extends A, B {
}
```

---

## Q6. Are constructors inherited?

### Answer

No.

Constructors belong to their declaring class and are not inherited.

However, when a child object is created, the parent constructor is invoked through constructor chaining.

---

## Q7. Are private members inherited?

### Answer

Private members belong to the declaring class and are not directly accessible from subclasses.

A subclass cannot directly access:

```java
private int balance;
```

declared in the parent.

---

## Q8. Can a final class be inherited?

### Answer

No.

```java
final class Animal {
}
```

This is invalid:

```java
class Dog extends Animal {
}
```

A final class cannot be extended.

---

## Q9. Inheritance vs Composition?

### Answer

```text
Inheritance
→ IS-A

Composition
→ HAS-A
```

Example:

```text
Dog IS-A Animal
Car HAS-A Engine
```

---

# 05 — Polymorphism

## Q1. What is polymorphism?

### Answer

Polymorphism allows the same operation/reference to work with different forms of objects.

Example:

```java
Animal a = new Dog();
a.sound();
```

If `Dog` overrides `sound()`, the dog's implementation executes.

---

## Q2. What are the main types of polymorphism in Java?

### Answer

```text
Compile-time
→ Method overloading

Runtime
→ Method overriding
```

---

## Q3. What is runtime polymorphism?

### Answer

Runtime polymorphism occurs when an overridden instance method is selected based on the **actual object at runtime**.

```java
Animal a = new Dog();

a.sound();
```

If `Dog` overrides `sound()`:

```text
Dog.sound()
```

executes.

---

## Q4. What is dynamic method dispatch?

### Answer

Dynamic method dispatch is the runtime mechanism by which Java selects the overridden instance method based on the actual object.

```text
Animal a = new Dog();

a.sound()
   ↓
Actual object = Dog
   ↓
Dog.sound()
```

---

## Q5. What is upcasting?

### Answer

Upcasting means treating a child object as a parent type.

```java
Animal a = new Dog();
```

Because:

```text
Dog IS-A Animal
```

Upcasting is implicit.

---

## Q6. What is downcasting?

### Answer

Downcasting converts a parent reference back to a child reference.

```java
Animal a = new Dog();

Dog d = (Dog) a;
```

It should be done only when the actual object is compatible with the target type.

---

## Q7. What happens when a parent reference points to a child object?

### Answer

```java
Animal a = new Dog();
```

The reference type controls what members are available at compile time.

For an overridden instance method, the actual object determines which implementation executes at runtime.

```text
Reference → Animal
Object    → Dog
Method    → Dog implementation
```

---

# 06 — Method Overloading

## Q1. What is method overloading?

### Answer

Method overloading occurs when multiple methods have the same name but different parameter lists.

```java
void add(int a, int b) {
}

void add(double a, double b) {
}
```

---

## Q2. Can we overload by changing only return type?

### Answer

No.

This is invalid:

```java
int show() {
}

double show() {
}
```

Return type alone cannot distinguish overloaded methods.

---

## Q3. Can we overload by changing parameter type?

### Answer

Yes.

```java
void show(int x) {
}

void show(double x) {
}
```

---

## Q4. Can we overload by changing parameter order?

### Answer

Yes, if the resulting parameter list is different.

```java
void show(int x, double y) {
}

void show(double x, int y) {
}
```

---

## Q5. Can static methods be overloaded?

### Answer

Yes.

```java
static void show() {
}

static void show(int x) {
}
```

---

## Q6. Can constructors be overloaded?

### Answer

Yes.

```java
class Student {

    Student() {
    }

    Student(String name) {
    }

    Student(String name, int age) {
    }
}
```

---

## Q7. Can `main()` be overloaded?

### Answer

Yes.

You can define additional overloaded `main` methods.

However, the JVM uses:

```java
public static void main(String[] args)
```

as the standard entry point.

---

## Q8. Is overloading compile-time polymorphism?

### Answer

Yes.

The compiler determines which overloaded method should be invoked based on the method signature and available compile-time information.

---

# 07 — Method Overriding

## Q1. What is method overriding?

### Answer

Method overriding occurs when a subclass provides its own implementation of an inherited instance method with a compatible signature.

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
```

---

## Q2. What are the main overriding rules?

### Answer

Important rules:

```text
Inheritance is required
Same method signature
Return type must be same/covariant
Access cannot be reduced
final methods cannot be overridden
static methods are hidden
private methods are not overridden
constructors cannot be overridden
```

---

## Q3. Can static methods be overridden?

### Answer

No.

Static methods are associated with the class rather than participating in normal runtime instance-method dispatch.

When a subclass declares a static method with the same signature, it is called **method hiding**.

---

## Q4. Can private methods be overridden?

### Answer

No.

A private method is not available to subclasses for overriding.

A same-named method in the child is a separate method.

---

## Q5. Can final methods be overridden?

### Answer

No.

```java
final void show() {
}
```

A final method cannot be replaced by a subclass.

---

## Q6. What is a covariant return type?

### Answer

An overriding method can return the same type or a subtype of the parent's return type.

```java
Animal getAnimal() {
    return new Animal();
}
```

Child:

```java
@Override
Dog getAnimal() {
    return new Dog();
}
```

Because:

```text
Dog IS-A Animal
```

this is valid.

---

## Q7. Can overriding reduce access?

### Answer

No.

For example:

```text
public → protected ❌
protected → private ❌
```

But increasing visibility is allowed:

```text
protected → public ✅
```

---

## Q8. What is `@Override`?

### Answer

`@Override` tells the compiler that the programmer intends the method to override a superclass or interface method.

It helps detect mistakes such as:

```java
void Sound() {
}
```

when the parent has:

```java
void sound() {
}
```

---

## Q9. What is method hiding?

### Answer

Method hiding occurs when a subclass declares a static method with the same signature as a static method in the parent.

```java
class Parent {
    static void show() {
    }
}

class Child extends Parent {
    static void show() {
    }
}
```

This is hiding, not overriding.

---

# 08 — Abstraction

## Q1. What is abstraction?

### Answer

Abstraction means exposing essential functionality while hiding unnecessary implementation details.

Java mainly achieves abstraction through:

```text
Abstract classes
Interfaces
```

---

## Q2. Why do we need abstraction?

### Answer

Abstraction helps:

```text
Reduce complexity
Hide implementation details
Define contracts
Improve maintainability
Support loose coupling
```

---

## Q3. How is abstraction achieved in Java?

### Answer

Primarily through:

```text
Abstract classes
Interfaces
```

---

## Q4. Real-world example of abstraction?

### Answer

When we call:

```java
payment.pay();
```

we care about:

```text
"Make the payment"
```

We do not necessarily need to know every internal step of processing the payment.

---

## Q5. Abstraction vs Encapsulation?

### Answer

```text
Abstraction
→ What should the object expose?

Encapsulation
→ How is the object's state/implementation controlled?
```

Example:

```text
ATM withdrawal operation
→ Abstraction

Private account balance
→ Encapsulation
```

---

# 09 — Interface

## Q1. What is an interface?

### Answer

An interface defines a contract that implementing classes agree to follow.

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
        System.out.println("UPI Payment");
    }
}
```

---

## Q2. Why do we use interfaces?

### Answer

Interfaces help provide:

```text
Abstraction
Loose coupling
Polymorphism
Multiple inheritance of type
Contract-based design
```

---

## Q3. Can an interface have variables?

### Answer

Yes.

Interface fields are implicitly:

```text
public
static
final
```

Example:

```java
interface Payment {

    int MAX_LIMIT = 100000;
}
```

Conceptually:

```java
public static final int MAX_LIMIT = 100000;
```

---

## Q4. Can an interface have instance variables?

### Answer

No.

Interface fields are static and final; interfaces do not have per-object instance fields.

---

## Q5. Can an interface have constructors?

### Answer

No.

Interfaces are not instantiated directly, so they do not have constructors.

---

## Q6. Can an interface contain method implementations?

### Answer

Yes.

Modern Java interfaces can contain:

```text
default methods
static methods
private methods
```

They can also declare abstract methods.

---

## Q7. Can a class implement multiple interfaces?

### Answer

Yes.

```java
class SmartPhone implements Camera, GPS, MusicPlayer {
}
```

This is how Java supports multiple inheritance of type.

---

## Q8. Can an interface extend another interface?

### Answer

Yes.

```java
interface A {
}

interface B extends A {
}
```

An interface can also extend multiple interfaces.

---

## Q9. Can a class extend an interface?

### Answer

No.

A class uses:

```java
implements
```

with interfaces.

```java
class Dog implements Animal {
}
```

---

## Q10. Can an interface extend a class?

### Answer

No.

An interface can extend interfaces, not classes.

---

# 10 — Abstract Class

## Q1. What is an abstract class?

### Answer

An abstract class is a class declared with `abstract` that can define common state and behavior for subclasses and can also declare abstract methods.

```java
abstract class Animal {

    abstract void sound();

    void eat() {
        System.out.println("Eating");
    }
}
```

---

## Q2. Can we create an object of an abstract class?

### Answer

No.

```java
Animal a = new Animal();
```

is invalid if `Animal` is abstract.

But we can create a reference:

```java
Animal a = new Dog();
```

---

## Q3. Can an abstract class have constructors?

### Answer

Yes.

```java
abstract class Animal {

    Animal() {
        System.out.println("Animal constructor");
    }
}
```

The constructor executes when a subclass object is created.

---

## Q4. Can an abstract class have normal methods?

### Answer

Yes.

An abstract class can contain:

```text
abstract methods
concrete methods
fields
constructors
static methods
final methods
```

subject to normal Java rules.

---

## Q5. Can an abstract class have zero abstract methods?

### Answer

Yes.

```java
abstract class Animal {

    void eat() {
    }
}
```

It can be abstract simply to prevent direct instantiation or to communicate a design intention.

---

## Q6. Abstract class vs interface?

### Answer

| Abstract Class                            | Interface                                           |
| ----------------------------------------- | --------------------------------------------------- |
| Can have instance fields                  | Cannot have instance fields                         |
| Can have constructors                     | Cannot have constructors                            |
| Can maintain object state                 | Fields are static/final                             |
| Class extends one class                   | Class can implement multiple interfaces             |
| Can contain concrete and abstract methods | Can contain abstract/default/static/private methods |

---

# 11 — Constructor

## Q1. What is a constructor?

### Answer

A constructor is a special member used during object creation to initialize the object.

Example:

```java
class Student {

    Student() {
        System.out.println("Constructor");
    }
}
```

---

## Q2. What are the rules of constructors?

### Answer

Important rules:

```text
Same name as class
No return type
Called during object creation
Can be overloaded
Cannot be overridden
Not inherited
```

---

## Q3. Can a constructor return a value?

### Answer

No.

A constructor has no return type.

This:

```java
Student() {
}
```

is a constructor.

This:

```java
void Student() {
}
```

is a method, not a constructor.

---

## Q4. What is a default constructor?

### Answer

If a class declares **no constructor**, the compiler can provide a no-argument constructor automatically.

Example:

```java
class Student {
}
```

The compiler provides a default constructor conceptually equivalent to:

```java
Student() {
    super();
}
```

If you declare any constructor yourself, the compiler does not automatically provide this constructor.

---

## Q5. Can constructors be overloaded?

### Answer

Yes.

```java
Student() {
}

Student(String name) {
}

Student(String name, int age) {
}
```

---

## Q6. Can constructors be inherited?

### Answer

No.

Constructors belong to the class that declares them.

---

## Q7. Can constructors be overridden?

### Answer

No.

Constructors are not inherited, so they cannot be overridden.

---

## Q8. What is constructor chaining?

### Answer

Constructor chaining means one constructor invokes another constructor.

Within the same class:

```java
this();
```

Parent constructor:

```java
super();
```

---

# 12 — `this` and `super`

## Q1. What is `this`?

### Answer

`this` refers to the **current object**.

Example:

```java
class Student {

    String name;

    Student(String name) {
        this.name = name;
    }
}
```

Here:

```text
this.name → instance variable
name      → constructor parameter
```

---

## Q2. What are the uses of `this`?

### Answer

Common uses:

```text
Refer to current object
Access current object's fields
Call current class method
Call another constructor using this()
Pass current object as an argument
Return current object
```

---

## Q3. What is `super`?

### Answer

`super` refers to the parent-class portion of the current object and is used to access superclass members.

Example:

```java
super.show();
```

---

## Q4. What are the uses of `super`?

### Answer

```text
Call parent constructor
Access parent field
Call parent method
```

Examples:

```java
super();
super.name;
super.show();
```

---

## Q5. Can `this()` and `super()` be used together in one constructor?

### Answer

Not as separate constructor-invocation statements.

Both must appear as the first statement if used, so only one can directly be the constructor invocation.

You cannot write:

```java
this();
super();
```

in the same constructor.

---

## Q6. Why must `this()` or `super()` be the first statement?

### Answer

Constructor invocation establishes the constructor chain before the rest of the constructor body executes.

Java therefore requires explicit constructor invocation to be the first statement.

---

# 13 — `static`

## Q1. What is `static`?

### Answer

`static` indicates that a member belongs to the **class rather than each individual object**.

Example:

```java
class Student {

    static String college;
}
```

There is one class-level `college` field shared by instances.

---

## Q2. What is a static variable?

### Answer

A static variable belongs to the class and is shared among instances.

```java
class Student {

    static String college = "AIET";
}
```

All `Student` objects access the same class variable.

---

## Q3. What is a static method?

### Answer

A static method belongs to the class.

```java
class MathUtil {

    static int square(int x) {
        return x * x;
    }
}
```

Call:

```java
MathUtil.square(5);
```

---

## Q4. Can a static method access instance variables directly?

### Answer

No.

A static method has no implicit `this` reference.

Therefore:

```java
class Test {

    int x;

    static void show() {
        System.out.println(x); // Error
    }
}
```

is invalid.

---

## Q5. Can an instance method access static members?

### Answer

Yes.

An instance method can access static members.

```java
class Test {

    static int x;

    void show() {
        System.out.println(x);
    }
}
```

---

## Q6. Can static methods be overridden?

### Answer

No.

They are hidden rather than overridden.

---

## Q7. Can static methods be overloaded?

### Answer

Yes.

```java
static void show() {
}

static void show(int x) {
}
```

---

## Q8. Can constructors be static?

### Answer

No.

Constructors initialize objects, while static members belong to the class.

A constructor cannot be declared `static`.

---

## Q9. Can a class be static?

### Answer

A top-level class cannot be declared `static`.

But a **nested class** can be static.

```java
class Outer {

    static class Inner {
    }
}
```

---

# 14 — `final`

## Q1. What is `final`?

### Answer

`final` prevents further modification depending on where it is applied.

```text
final variable
→ cannot be reassigned

final method
→ cannot be overridden

final class
→ cannot be extended
```

---

## Q2. What is a final variable?

### Answer

A final variable can be assigned only once.

```java
final int MAX = 100;
```

This is invalid:

```java
MAX = 200;
```

---

## Q3. Can a final reference point to a mutable object?

### Answer

Yes.

```java
final ArrayList<Integer> list = new ArrayList<>();
```

You cannot make `list` refer to another object:

```java
list = new ArrayList<>(); // Error
```

But you can modify the existing object's contents:

```java
list.add(10);
```

Important:

```text
final reference
≠
immutable object
```

---

## Q4. What is a final method?

### Answer

A final method cannot be overridden by subclasses.

```java
final void show() {
}
```

---

## Q5. What is a final class?

### Answer

A final class cannot be extended.

```java
final class Vehicle {
}
```

This is invalid:

```java
class Car extends Vehicle {
}
```

---

## Q6. Can a final method be overloaded?

### Answer

Yes.

`final` prevents overriding, not overloading.

```java
final void show() {
}

void show(int x) {
}
```

This is valid.

---

## Q7. Can a final class have subclasses?

### Answer

No.

That is precisely what `final` prevents.

---

## Q8. Is `final` the same as immutable?

### Answer

No.

`final` prevents reassignment of a variable/reference.

Immutability means the object's observable state cannot be changed after construction.

---

# 15 — Association, Aggregation & Composition

## Q1. What is association?

### Answer

Association is a general relationship between objects where they interact, communicate, or use each other.

Example:

```text
Teacher ───── Student
```

Neither necessarily owns the other.

---

## Q2. What is aggregation?

### Answer

Aggregation is a weak whole-part relationship where the part can exist independently of the whole.

Example:

```text
Department ◇──── Employee
```

The employee can exist independently of the department.

---

## Q3. What is composition?

### Answer

Composition is a strong whole-part relationship where the part's lifecycle is strongly tied to the whole.

Example:

```text
Order ◆──── OrderItem
```

---

## Q4. Aggregation vs Composition?

### Answer

```text
Aggregation
→ Weak ownership
→ Part can exist independently

Composition
→ Strong ownership
→ Part's lifecycle depends on whole
```

---

## Q5. What is HAS-A relationship?

### Answer

HAS-A describes a relationship where one object contains, uses, or collaborates with another object.

Example:

```java
class Car {

    Engine engine;
}
```

```text
Car HAS-A Engine
```

---

## Q6. What is IS-A relationship?

### Answer

IS-A represents inheritance.

```java
class Dog extends Animal {
}
```

```text
Dog IS-A Animal
```

---

## Q7. Does using `new` automatically mean composition?

### Answer

No.

```java
class Car {

    Engine engine = new Engine();
}
```

This can indicate strong ownership, but the `new` keyword itself does not define composition.

The domain relationship and lifecycle are what matter.

---

## Q8. Why is composition often preferred over inheritance?

### Answer

Composition can provide:

```text
Lower coupling
Greater flexibility
Easier replacement of components
Better separation of responsibilities
Less rigid hierarchies
```

But inheritance is still appropriate when there is a genuine IS-A relationship.

---

## Q9. What are the UML symbols?

### Answer

```text
Association   → ─────

Aggregation   → ◇─────

Composition   → ◆─────
```

Memory trick:

```text
◇ → Weak

◆ → Strong
```

---

# 🔥 Cross-Topic Interview Questions

These questions combine multiple OOP concepts and are **very important**.

---

## Q1. What is the difference between abstraction and encapsulation?

### Answer

```text
Abstraction
→ Hides unnecessary implementation complexity.

Encapsulation
→ Bundles state/behavior and controls access to internal state.
```

Example:

```text
ATM withdrawal process
→ Abstraction

private balance
→ Encapsulation
```

---

## Q2. What is the difference between overloading and overriding?

### Answer

| Overloading               | Overriding                                  |
| ------------------------- | ------------------------------------------- |
| Same method name          | Same method signature                       |
| Different parameters      | Same parameters                             |
| Compile-time selection    | Runtime dispatch                            |
| Inheritance not required  | Inheritance/interface relationship required |
| Compile-time polymorphism | Runtime polymorphism                        |

---

## Q3. What is the difference between abstract class and interface?

### Answer

```text
Abstract class
→ Can maintain instance state
→ Can have constructors
→ Class can extend only one class

Interface
→ No instance fields
→ No constructors
→ Class can implement multiple interfaces
```

Modern interfaces can contain default, static, and private methods in addition to abstract method declarations.

---

## Q4. What is the difference between inheritance and composition?

### Answer

```text
Inheritance
→ IS-A

Composition
→ HAS-A
```

Example:

```text
Dog IS-A Animal

Car HAS-A Engine
```

Composition is often preferred when you need flexible collaboration rather than a true subtype relationship.

---

## Q5. Why doesn't Java support multiple inheritance of classes?

### Answer

Primarily to avoid ambiguity such as the diamond problem and keep class inheritance simpler.

Java instead allows multiple inheritance of type through interfaces.

---

## Q6. Can we create an object of an interface?

### Answer

No.

```java
Payment p = new Payment();
```

is invalid if `Payment` is an interface.

But we can create an implementing object:

```java
Payment p = new UPI();
```

---

## Q7. Can we create an object of an abstract class?

### Answer

No.

But we can use its reference:

```java
Animal a = new Dog();
```

---

## Q8. Why can an abstract class have a constructor if we cannot instantiate it?

### Answer

Because the constructor initializes the abstract-class portion of a subclass object.

```text
new Dog()
   ↓
Animal constructor
   ↓
Dog constructor
```

---

## Q9. Can an abstract class implement an interface without implementing all methods?

### Answer

Yes.

An abstract class can leave interface methods unimplemented.

A concrete subclass must eventually provide implementations for all required abstract methods.

---

## Q10. Can an interface contain static methods?

### Answer

Yes.

```java
interface Test {

    static void show() {
        System.out.println("Hello");
    }
}
```

Call:

```java
Test.show();
```

Interface static methods belong to the interface and are not inherited as instance methods by implementing classes.

---

# ⚠️ Major OOP Interview Traps

## Trap 1

```java
Parent p = new Child();
```

For an overridden instance method:

```text
Child implementation executes.
```

---

## Trap 2

Fields are not dynamically overridden.

```java
Parent p = new Child();

p.field;
```

Field access is based on the compile-time/reference type.

---

## Trap 3

Static methods are not overridden.

They are hidden.

---

## Trap 4

Private methods are not overridden.

---

## Trap 5

Final methods cannot be overridden.

---

## Trap 6

Constructors cannot be overridden.

---

## Trap 7

Changing only the return type does not create overloading.

---

## Trap 8

A final reference does not make its object immutable.

```java
final List<Integer> list = new ArrayList<>();
```

The reference cannot change, but the list can still be modified.

---

## Trap 9

Abstract classes can have constructors.

---

## Trap 10

Interfaces can have implemented methods in modern Java.

They are not restricted to only abstract method declarations.

---

## Trap 11

An interface cannot have instance variables.

Its fields are implicitly:

```text
public static final
```

---

## Trap 12

A class can implement multiple interfaces.

```java
class C implements A, B {
}
```

---

## Trap 13

A class cannot extend multiple classes.

```java
class C extends A, B // ❌
```

---

## Trap 14

`this` refers to the current object.

`super` is used to access superclass members/constructor.

---

## Trap 15

Aggregation and composition are not Java keywords.

They are design relationships.

---

# 🏆 TOP 30 MOST IMPORTANT OOP INTERVIEW QUESTIONS

These are the questions I would revise **first** before an interview.

```text
01. What is OOP and why is it used?

02. What are the four pillars of OOP?

03. Is Java completely object-oriented? Why?

04. What is the difference between a class and an object?

05. What happens when we use the new keyword?

06. What is encapsulation and why is it important?

07. Encapsulation vs abstraction?

08. What is inheritance?

09. What types of inheritance does Java support?

10. Why doesn't Java support multiple inheritance through classes?

11. What is polymorphism?

12. Compile-time vs runtime polymorphism?

13. What is method overloading?

14. Can methods be overloaded by changing only return type?

15. What is method overriding?

16. What are the rules of method overriding?

17. Why are static methods not overridden?

18. Why are private methods not overridden?

19. What is dynamic method dispatch?

20. What happens when a parent reference points to a child object?

21. What is an abstract class?

22. Can an abstract class have constructors?

23. Abstract class vs interface?

24. Can an interface have variables, constructors, and implemented methods?

25. Can a class implement multiple interfaces?

26. What is the difference between this and super?

27. What is static and why can't static methods be overridden?

28. What are the three uses of final?

29. Inheritance vs composition?

30. Association vs aggregation vs composition?
```

---

# 🔥 TOP 10 — MUST KNOW

If the interviewer gives you only a few OOP questions, be extremely comfortable with these:

```text
1. Explain the four pillars of OOP with real-world examples.

2. Explain encapsulation vs abstraction.

3. Explain inheritance and its types in Java.

4. Explain polymorphism and its two major forms.

5. Explain method overloading vs method overriding.

6. Explain runtime polymorphism and dynamic method dispatch.

7. Explain abstract class vs interface.

8. Explain this vs super and constructor chaining.

9. Explain static and final with all important restrictions.

10. Explain IS-A vs HAS-A and
    Association vs Aggregation vs Composition.
```

---

# 🧠 ONE-PAGE OOP MEMORY MAP

```text
                         JAVA OOP
                            |
        +-------------------+-------------------+
        |                   |                   |
       IS-A               HAS-A              USES-A
        |                   |                   |
   Inheritance       Association/              Association
                     Aggregation/
                     Composition
        |
        +----------------+
        |                |
   Polymorphism      Reuse
        |
   +----+----+
   |         |
Overloading Overriding
   |         |
Compile      Runtime
time         |
             ↓
      Dynamic Dispatch


Encapsulation
     ↓
Control access to state


Abstraction
     ↓
Hide unnecessary complexity


Interface
     ↓
Contract + abstraction + multiple inheritance of type


Abstract Class
     ↓
Partial abstraction + shared state/behavior


static
     ↓
Class-level member


final
     ↓
Variable → no reassignment
Method   → no overriding
Class    → no inheritance


Composition
     ↓
Strong HAS-A


Aggregation
     ↓
Weak HAS-A
```

---

# 🎯 30-SECOND OOP INTERVIEW ANSWER

> **Java is an object-oriented programming language built around classes and objects. Its major OOP concepts are encapsulation, inheritance, polymorphism, and abstraction. Encapsulation controls access to an object's state, inheritance provides IS-A relationships and reuse, polymorphism allows the same interface or reference to represent different implementations, and abstraction hides unnecessary implementation details. Java supports compile-time polymorphism through method overloading and runtime polymorphism through method overriding. Interfaces and abstract classes are major tools for abstraction, while composition and aggregation model HAS-A relationships.**

---

# ✅ Final Revision Order

For interview preparation, revise in this order:

```text
1. Four pillars of OOP
        ↓
2. Encapsulation vs Abstraction
        ↓
3. Inheritance
        ↓
4. Polymorphism
        ↓
5. Overloading vs Overriding
        ↓
6. Runtime Polymorphism
        ↓
7. Abstract Class vs Interface
        ↓
8. Constructor + this + super
        ↓
9. static + final
        ↓
10. IS-A vs HAS-A
        ↓
11. Association vs Aggregation vs Composition
        ↓
12. Interview Traps
```

> **⭐ Final rule:** Don't just memorize definitions. For every OOP concept, be able to explain **what it is → why we need it → how Java implements it → example → interview trap**.
