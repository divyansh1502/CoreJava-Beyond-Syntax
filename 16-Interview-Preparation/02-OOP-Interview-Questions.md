# 🧩 OOP — Interview Questions & Answers

> **A focused collection of Object-Oriented Programming interview questions with concise, interview-ready answers.**

---

# 📑 Table of Contents

- [1. OOP Fundamentals](#1-oop-fundamentals)
- [2. Classes and Objects](#2-classes-and-objects)
- [3. Encapsulation](#3-encapsulation)
- [4. Inheritance](#4-inheritance)
- [5. Polymorphism](#5-polymorphism)
- [6. Abstraction](#6-abstraction)
- [7. Interfaces](#7-interfaces)
- [8. Constructors](#8-constructors)
- [9. Method Overloading and Overriding](#9-method-overloading-and-overriding)
- [10. `this` and `super`](#10-this-and-super)
- [11. `static` and `final` in OOP](#11-static-and-final-in-oop)
- [12. Access Modifiers](#12-access-modifiers)
- [13. Association, Aggregation and Composition](#13-association-aggregation-and-composition)
- [14. Advanced OOP Questions](#14-advanced-oop-questions)
- [15. OOP Tricky Questions](#15-oop-tricky-questions)
- [16. Rapid OOP Revision](#16-rapid-oop-revision)

---

# 1. OOP Fundamentals

## 1. What is OOP?

OOP stands for Object-Oriented Programming.

It is a programming paradigm that organizes software around objects containing state and behavior.

The four commonly discussed pillars are:

```text
Encapsulation
Inheritance
Polymorphism
Abstraction
```

---

## 2. What are the four pillars of OOP?

### Encapsulation

Controls access to an object's internal state.

### Inheritance

Allows a class to derive from another class.

### Polymorphism

Allows the same interface/reference to work with different implementations.

### Abstraction

Hides unnecessary implementation details and exposes essential behavior.

---

## 3. Why is OOP used?

OOP helps organize large programs around responsibilities and relationships between objects.

Common benefits include:

```text
Modularity
Reusability
Maintainability
Extensibility
Encapsulation
Abstraction
```

---

## 4. Is Java completely object-oriented?

No.

Java supports object-oriented programming but also has primitive data types such as:

```text
int
char
boolean
double
```

Therefore, Java is not purely object-oriented.

---

## 5. What is an object?

An object is an instance of a class.

It generally has:

```text
Identity
State
Behavior
```

---

## 6. What is a class?

A class is a type definition that describes the state and behavior that objects of that type can have.

---

## 7. Class vs object?

```text
Class
→ blueprint/type definition


Object
→ instance of that type
```

Example concept:

```text
Class  → Car
Object → myCar
```

---

# 2. Classes and Objects

## 8. How is an object created in Java?

Objects are commonly created using `new`.

Example:

```java
class Student {
}
```

```java
Student s =
    new Student();
```

The `new` expression creates an object and invokes an appropriate constructor.

---

## 9. Where is an object stored?

Objects are allocated in the JVM heap according to the JVM's runtime memory model.

A reference variable may be stored in a stack frame when it is a local variable.

---

## 10. What is an instance variable?

A non-static field belonging to an object is an instance variable.

Each object normally has its own instance state.

---

## 11. What is an instance method?

A non-static method associated with an object is an instance method.

It can directly access instance members through the implicit `this` reference.

---

## 12. Can a class have multiple objects?

Yes.

Each object can maintain its own instance state.

---

## 13. Can two references point to the same object?

Yes.

Example:

```java
Student a =
    new Student();

Student b = a;
```

Both references refer to the same object.

---

## 14. What happens when an object has no references pointing to it?

If the object is no longer reachable from GC roots, it becomes eligible for garbage collection.

It is not necessarily collected immediately.

---

# 3. Encapsulation

## 15. What is encapsulation?

Encapsulation is the practice of controlling access to an object's internal state and exposing controlled operations.

A common implementation uses:

```text
private fields
+
public/protected methods
```

---

## 16. How is encapsulation implemented in Java?

A common approach is:

```java
class Employee {

    private double salary;

    public double getSalary() {

        return salary;
    }

    public void setSalary(
        double salary
    ) {

        if (salary >= 0) {
            this.salary = salary;
        }
    }
}
```

The field is not directly accessible from outside the class.

---

## 17. Why are fields usually declared private?

Private fields prevent unrestricted external modification.

The class can validate or control changes through methods.

---

## 18. Is using getters and setters always encapsulation?

Not automatically.

If getters and setters simply expose all internal state without enforcing meaningful invariants, the design may provide limited encapsulation.

Good encapsulation controls how state is accessed and modified.

---

## 19. Encapsulation vs data hiding?

They are related but not identical.

```text
Data hiding
→ restricts direct access to implementation details


Encapsulation
→ bundles state/behavior and controls access to them
```

---

# 4. Inheritance

## 20. What is inheritance?

Inheritance allows a subclass to derive from a superclass.

Example relationship:

```text
Dog IS-A Animal
```

---

## 21. What is a superclass?

A superclass is the class from which another class inherits.

It is also called:

```text
Parent class
Base class
```

---

## 22. What is a subclass?

A subclass is a class that extends another class.

It is also called:

```text
Child class
Derived class
```

---

## 23. Which keyword is used for inheritance?

The `extends` keyword is used for class inheritance.

Example:

```java
class Dog
    extends Animal {
}
```

---

## 24. Does Java support multiple inheritance?

Java does not support multiple inheritance of classes.

However, a class can implement multiple interfaces.

---

## 25. Why does Java not support multiple inheritance of classes?

One important issue is ambiguity.

Suppose two parent classes provide the same method:

```text
Parent A
   ↓
  show()

Parent B
   ↓
  show()

     ↓
 Child
```

The language would need to resolve which implementation should be inherited.

Java avoids this class-level ambiguity by allowing only one direct superclass.

---

## 26. What types of inheritance are possible in Java?

Using classes, Java directly supports:

```text
Single inheritance
Multilevel inheritance
Hierarchical inheritance
```

Multiple inheritance of classes is not supported.

Multiple interface inheritance is supported.

---

## 27. What is multilevel inheritance?

Inheritance across multiple levels.

```text
Animal
   ↓
Dog
   ↓
Puppy
```

---

## 28. What is hierarchical inheritance?

Multiple subclasses inherit from the same superclass.

```text
       Animal
       /    \
     Dog    Cat
```

---

## 29. Are constructors inherited?

No.

Constructors belong to the class in which they are declared and are not inherited.

---

## 30. Are private members inherited?

Private members belong to the declaring class and are not directly accessible in subclasses.

A subclass may still interact with inherited object state through accessible methods defined by the superclass.

---

## 31. Can a final class be inherited?

No.

A `final` class cannot be extended.

---

# 5. Polymorphism

## 32. What is polymorphism?

Polymorphism means that the same interface or reference can work with objects of different types.

In Java, common forms are:

```text
Compile-time polymorphism
→ method overloading


Runtime polymorphism
→ method overriding
```

---

## 33. What is compile-time polymorphism?

Method overloading is commonly called compile-time polymorphism because the compiler determines which overloaded method signature is applicable.

Example:

```java
void print(int value) {
}

void print(String value) {
}
```

---

## 34. What is runtime polymorphism?

Runtime polymorphism occurs when an overridden instance method is selected based on the actual object at runtime.

Example:

```java
Animal animal =
    new Dog();

animal.sound();
```

If `Dog` overrides `sound()`, the Dog implementation is selected.

---

## 35. What is dynamic method dispatch?

Dynamic method dispatch is the runtime mechanism used to select an overridden instance method based on the actual object.

---

## 36. Can runtime polymorphism work with fields?

No.

Field access is not dynamically dispatched like overridden instance methods.

---

## 37. Can static methods participate in runtime polymorphism?

No.

Static methods are hidden, not overridden.

---

## 38. Can private methods participate in runtime polymorphism?

No.

Private methods are not inherited and therefore cannot be overridden.

---

# 6. Abstraction

## 39. What is abstraction?

Abstraction means exposing essential functionality while hiding unnecessary implementation details.

Java primarily provides abstraction through:

```text
Abstract classes
Interfaces
```

---

## 40. What is an abstract class?

An abstract class is declared using `abstract` and cannot be directly instantiated.

It can contain both abstract and concrete methods.

---

## 41. Can an abstract class contain non-abstract methods?

Yes.

Example:

```java
abstract class Animal {

    abstract void sound();

    void sleep() {

        System.out.println(
            "Sleeping"
        );
    }
}
```

---

## 42. Can an abstract class have variables?

Yes.

It can contain:

```text
Instance variables
Static variables
Final variables
```

---

## 43. Can an abstract class have a constructor?

Yes.

Its constructor executes as part of constructing a subclass object.

---

## 44. Can an abstract class be instantiated?

No.

This is invalid:

```java
Animal a =
    new Animal();
```

But a reference of abstract type can refer to a concrete subclass object:

```java
Animal a =
    new Dog();
```

---

## 45. Can an abstract class have no abstract methods?

Yes.

A class can be declared abstract even if it has no abstract methods.

This can be useful when the class should not be instantiated directly.

---

# 7. Interfaces

## 46. What is an interface?

An interface defines a contract that classes can implement.

Example:

```java
interface Payment {

    void pay();
}
```

---

## 47. Can a class implement multiple interfaces?

Yes.

Example:

```java
class SmartDevice
    implements Camera, MusicPlayer {
}
```

---

## 48. Can an interface extend another interface?

Yes.

An interface can extend one or multiple interfaces.

Example:

```java
interface C
    extends A, B {
}
```

---

## 49. Can an interface extend a class?

No.

Interfaces extend interfaces, while classes extend classes and implement interfaces.

---

## 50. Can an interface contain variables?

Yes.

Fields declared in an interface are implicitly:

```text
public
static
final
```

Example:

```java
interface Config {

    int MAX =
        100;
}
```

---

## 51. Can an interface contain method implementations?

Yes.

Modern Java interfaces can contain:

```text
default methods
static methods
private methods
```

They can also declare abstract methods.

---

## 52. What is a default method?

A default method is an interface method with an implementation.

Example:

```java
interface Vehicle {

    default void start() {

        System.out.println(
            "Starting"
        );
    }
}
```

---

## 53. Why were default methods introduced?

They allow interfaces to evolve by adding behavior without necessarily breaking every existing implementing class.

---

## 54. What happens when two interfaces have the same default method?

The implementing class must resolve the conflict.

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

The class must provide its own implementation:

```java
class C
    implements A, B {

    @Override
    public void show() {

        A.super.show();
    }
}
```

---

# 8. Constructors

## 55. What is a constructor?

A constructor initializes a newly created object.

It has:

```text
Same name as class
No return type
```

---

## 56. Can constructors be overloaded?

Yes.

Example:

```java
class Student {

    Student() {
    }

    Student(String name) {
    }
}
```

---

## 57. Can constructors be overridden?

No.

Constructors are not inherited.

---

## 58. Can a constructor be private?

Yes.

A private constructor can restrict object creation from outside the class.

It is commonly used in designs such as:

```text
Singleton
Utility-style classes
Factory-controlled creation
```

---

## 59. What is constructor chaining?

Constructor chaining means one constructor invokes another constructor.

Within the same class:

```java
this();
```

To invoke the superclass constructor:

```java
super();
```

---

## 60. What is the difference between `this()` and `super()`?

```text
this()
→ calls another constructor of the same class


super()
→ calls a constructor of the superclass
```

Both must appear as the first statement of a constructor when explicitly used.

---

# 9. Method Overloading and Overriding

## 61. What is method overloading?

Multiple methods have the same name but different parameter lists.

Example:

```java
void calculate(int a) {
}

void calculate(int a, int b) {
}
```

---

## 62. Can methods be overloaded only by changing return type?

No.

This is not valid:

```java
int getValue() {
    return 1;
}
```

```java
double getValue() {
    return 1.0;
}
```

The parameter list is identical, so return type alone cannot distinguish overloaded methods.

---

## 63. What is method overriding?

A subclass provides a compatible implementation of an inherited overridable instance method.

---

## 64. What are important rules for method overriding?

The overriding method generally:

- Must have the same method signature
- Cannot reduce access visibility
- Cannot override a `final` method
- Cannot override a `private` method
- Must have a compatible return type
- Cannot throw broader checked exceptions than allowed by the overridden method

---

## 65. Can an overriding method have a covariant return type?

Yes.

The overriding method can return a subtype of the original return type when the return types are reference types.

---

## 66. Overloading vs overriding?

```text
Overloading
→ same method name
→ different parameter list
→ compile-time selection


Overriding
→ subclass implementation
→ same signature
→ runtime dispatch
```

---

# 10. `this` and `super`

## 67. What is `this`?

`this` refers to the current object.

It can be used to:

```text
Access instance fields
Call instance methods
Call another constructor
Pass the current object
Return the current object
```

---

## 68. Why use `this`?

A common use is resolving naming conflicts.

Example:

```java
class Student {

    private String name;

    Student(String name) {

        this.name = name;
    }
}
```

---

## 69. What is `super`?

`super` refers to the superclass portion of the current object.

It can be used to:

```text
Access superclass members
Call superclass methods
Call superclass constructor
```

---

## 70. Can `this()` and `super()` be used together in one constructor?

No.

Both must be the first constructor invocation, so they cannot both appear in the same constructor.

---

# 11. `static` and `final` in OOP

## 71. What is a static member?

A static member belongs to the class rather than an individual object.

---

## 72. Can a static method access `this`?

No.

`this` represents an object, while a static method does not have an implicit object context.

---

## 73. Can a static method be overloaded?

Yes.

Static methods can be overloaded if their parameter lists differ.

---

## 74. Can a static method be overridden?

No.

Static methods are hidden.

---

## 75. What is a final variable?

A final variable can be assigned only once.

For a blank final instance field, assignment can occur in a constructor or initializer as permitted by Java's definite-assignment rules.

---

## 76. What is a final method?

A final method cannot be overridden by subclasses.

---

## 77. What is a final class?

A final class cannot be extended.

Example:

```java
final class SecurityManager {
}
```

---

## 78. Is a final reference object immutable?

No.

A final reference cannot be reassigned, but the referenced object's mutable state may still change.

---

# 12. Access Modifiers

## 79. What are Java's access modifiers?

```text
private
default/package-private
protected
public
```

---

## 80. What is private access?

A private member is directly accessible only within its declaring class.

---

## 81. What is default access?

If no access modifier is specified, the member has package-private access.

It is accessible within the same package.

---

## 82. What is protected access?

A protected member is accessible within the same package and also in subclasses under Java's protected-access rules.

---

## 83. What is public access?

A public member can be accessed wherever the containing type and member are accessible.

---

## 84. Which access modifier provides the highest visibility?

```text
public
```

---

# 13. Association, Aggregation and Composition

## 85. What is association?

Association represents a general relationship between objects.

Example:

```text
Teacher ↔ Student
```

A teacher can be associated with students, and both can exist independently.

---

## 86. What is aggregation?

Aggregation represents a weak whole-part relationship.

The contained object can exist independently of the container.

Example:

```text
Department
    |
    +── Teacher
```

A teacher can exist independently of a particular department.

---

## 87. What is composition?

Composition represents a strong whole-part relationship where the part's lifecycle is strongly tied to the whole.

Example:

```text
House
 |
 +── Room
```

In a typical composition model, the room is treated as part of the house's lifecycle.

---

## 88. Aggregation vs composition?

```text
Aggregation
→ weak HAS-A
→ parts can exist independently


Composition
→ strong HAS-A
→ part lifecycle is strongly owned by whole
```

---

## 89. Inheritance vs composition?

```text
Inheritance
→ IS-A


Composition
→ HAS-A
```

Example:

```text
Dog IS-A Animal
```

```text
Car HAS-A Engine
```

---

## 90. Why is composition often preferred over inheritance?

Composition can reduce tight coupling and allow behavior to be changed by replacing contained components.

Inheritance is appropriate when there is a genuine subtype relationship.

---

# 14. Advanced OOP Questions

## 91. What is upcasting?

Upcasting means treating a subclass object as a superclass type.

Example:

```java
Dog dog =
    new Dog();

Animal animal =
    dog;
```

It is generally implicit.

---

## 92. What is downcasting?

Downcasting means converting a superclass reference back to a specific subclass type.

Example:

```java
Animal animal =
    new Dog();

Dog dog =
    (Dog) animal;
```

It is explicit and can throw `ClassCastException` if the actual object is not compatible.

---

## 93. What is `instanceof`?

`instanceof` checks whether an object is compatible with a specified reference type.

Example:

```java
if (animal instanceof Dog) {

    Dog dog =
        (Dog) animal;
}
```

Modern Java also supports pattern matching with `instanceof`.

---

## 94. What is covariant return type?

An overriding method may return a more specific subtype than the return type declared by the overridden method.

---

## 95. Can an abstract class implement an interface without implementing all methods?

Yes.

An abstract class can leave interface methods abstract for its subclasses to implement.

---

## 96. Can a concrete class extend an abstract class without implementing its abstract methods?

No.

A concrete subclass must implement all inherited abstract methods unless it is also declared abstract.

---

## 97. Can an interface reference point to a class object?

Yes.

Example:

```java
Runnable task =
    new MyTask();
```

where `MyTask` implements `Runnable`.

---

## 98. What is the Liskov Substitution Principle?

The Liskov Substitution Principle states that objects of a subtype should be usable wherever objects of the supertype are expected without violating the expected behavior of the program.

---

## 99. What is composition over inheritance?

It is a design principle suggesting that behavior can often be assembled using collaborating objects instead of relying heavily on class inheritance.

Composition can provide more flexibility when behavior changes independently of the type hierarchy.

---

## 100. What is the difference between IS-A and HAS-A?

```text
IS-A
→ inheritance/subtyping


HAS-A
→ composition or aggregation
```

Example:

```text
Dog IS-A Animal
```

```text
Car HAS-A Engine
```

---

# 15. OOP Tricky Questions

## 101. Can an abstract class have a final method?

Yes.

The class can be abstract while individual methods can be final.

---

## 102. Can an abstract class be final?

No.

`abstract` requires the possibility of subclassing, while `final` prevents subclassing.

---

## 103. Can an interface be final?

No.

An interface is designed to be implemented or extended, while `final` prevents inheritance.

---

## 104. Can an interface be instantiated?

No.

But an interface reference can refer to an object of an implementing class.

---

## 105. Can a constructor be inherited?

No.

---

## 106. Can a constructor be overridden?

No.

---

## 107. Can a constructor be overloaded?

Yes.

---

## 108. Can a private method be overloaded?

Yes.

Private methods can be overloaded because overloading is based on different parameter lists.

---

## 109. Can a final method be overloaded?

Yes.

`final` prevents overriding, not overloading.

---

## 110. Can a static method be overloaded?

Yes.

Static methods can have multiple versions with different parameter lists.

---

## 111. Can a static method access an instance variable directly?

No.

It requires an object reference.

---

## 112. Can an object have multiple references?

Yes.

Multiple references can point to the same object.

---

## 113. Does assigning one object reference to another create a new object?

No.

Example:

```java
Student a =
    new Student();

Student b = a;
```

No second `Student` object is created.

Both references point to the same object.

---

## 114. What happens if a subclass does not explicitly call `super()`?

If the constructor does not explicitly invoke another constructor using `this()` or `super()`, Java implicitly inserts a no-argument superclass constructor invocation when applicable.

If the superclass has no accessible no-argument constructor, compilation fails.

---

## 115. Can `super()` call a parameterized constructor?

Yes.

Example:

```java
class Child
    extends Parent {

    Child() {

        super(10);
    }
}
```

---

# 16. Rapid OOP Revision

## 116. What are the four pillars?

```text
Encapsulation
Inheritance
Polymorphism
Abstraction
```

---

## 117. What is IS-A?

Inheritance/subtyping relationship.

```text
Dog IS-A Animal
```

---

## 118. What is HAS-A?

Composition or aggregation relationship.

```text
Car HAS-A Engine
```

---

## 119. Overloading or overriding for runtime polymorphism?

```text
Overriding
```

---

## 120. Can Java classes have multiple direct superclasses?

No.

---

## 121. Can a class implement multiple interfaces?

Yes.

---

## 122. Can an interface extend multiple interfaces?

Yes.

---

## 123. Can an abstract class have a constructor?

Yes.

---

## 124. Can an interface have a constructor?

No.

---

## 125. Can an abstract class have concrete methods?

Yes.

---

## 126. Can an abstract class have zero abstract methods?

Yes.

---

## 127. Can a final method be overridden?

No.

---

## 128. Can a final method be overloaded?

Yes.

---

## 129. Can a static method be overridden?

No.

Static methods are hidden.

---

## 130. Can a private method be overridden?

No.

---

## 131. Can a private method be overloaded?

Yes.

---

## 132. Can a constructor be static?

No.

---

## 133. Can a constructor be final?

No.

---

## 134. Can a constructor be private?

Yes.

---

## 135. Can an abstract class implement an interface?

Yes.

---

## 136. Can an interface contain default methods?

Yes.

---

## 137. Can an interface contain static methods?

Yes.

---

## 138. Can an interface contain private methods?

Yes, modern Java supports private interface methods.

---

## 139. Can interface fields be changed?

No.

They are implicitly `public static final`.

---

## 140. What is dynamic method dispatch?

Runtime selection of an overridden instance method based on the actual object.

---

# ⚡ OOP Interview Memory Map

```text
                    OOP
                     |
        +------------+------------+
        |            |            |
   Encapsulation  Inheritance  Abstraction
        |            |            |
     Hiding       IS-A         Contract
        |            |            |
     private      extends      abstract
                                  |
                              interface
                     |
                 Polymorphism
                     |
             +-------+-------+
             |               |
         Overloading      Overriding
         Compile-time     Runtime
```

---

# 🧠 Most Important OOP Comparisons

```text
Class
vs
Object


Encapsulation
vs
Abstraction


Overloading
vs
Overriding


Abstract Class
vs
Interface


Inheritance
vs
Composition


Aggregation
vs
Composition


this()
vs
super()


this
vs
super


IS-A
vs
HAS-A


Static Method
vs
Instance Method


Upcasting
vs
Downcasting
```

---

