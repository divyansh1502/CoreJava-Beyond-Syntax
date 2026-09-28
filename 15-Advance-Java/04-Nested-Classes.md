# 🧩 Nested Classes in Java

> **A nested class is a class declared inside another class or interface. Java supports static nested classes and non-static inner classes, including member, local, and anonymous classes.**

---

# 📑 Table of Contents

- [1. What Is a Nested Class?](#1-what-is-a-nested-class)
- [2. Why Do We Need Nested Classes?](#2-why-do-we-need-nested-classes)
- [3. Types of Nested Classes](#3-types-of-nested-classes)
- [4. Static Nested Class](#4-static-nested-class)
- [5. Creating an Object of a Static Nested Class](#5-creating-an-object-of-a-static-nested-class)
- [6. Accessing Outer Class Members](#6-accessing-outer-class-members)
- [7. Non-Static Member Inner Class](#7-non-static-member-inner-class)
- [8. Creating an Inner Class Object](#8-creating-an-inner-class-object)
- [9. Accessing Outer Class Members from Inner Class](#9-accessing-outer-class-members-from-inner-class)
- [10. Outer Class Reference](#10-outer-class-reference)
- [11. Private Members and Nested Classes](#11-private-members-and-nested-classes)
- [12. Local Classes](#12-local-classes)
- [13. Anonymous Classes](#13-anonymous-classes)
- [14. Nested Class Inside an Interface](#14-nested-class-inside-an-interface)
- [15. Static Nested Class vs Inner Class](#15-static-nested-class-vs-inner-class)
- [16. Member Nested Class](#16-member-nested-class)
- [17. Access Modifiers](#17-access-modifiers)
- [18. `this` in Nested Classes](#18-this-in-nested-classes)
- [19. `OuterClass.this`](#19-outerclassthis)
- [20. Shadowing in Nested Classes](#20-shadowing-in-nested-classes)
- [21. Multiple Levels of Nesting](#21-multiple-levels-of-nesting)
- [22. Nested Classes and Inheritance](#22-nested-classes-and-inheritance)
- [23. Nested Classes and Static Members](#23-nested-classes-and-static-members)
- [24. Java 16+ Static Members in Inner Classes](#24-java-16-static-members-in-inner-classes)
- [25. Nested Classes and Encapsulation](#25-nested-classes-and-encapsulation)
- [26. Nested Classes and Generics](#26-nested-classes-and-generics)
- [27. Nested Classes and Interfaces](#27-nested-classes-and-interfaces)
- [28. Bytecode and JVM View](#28-bytecode-and-jvm-view)
- [29. Synthetic Access and Modern Java](#29-synthetic-access-and-modern-java)
- [30. When Should You Use Nested Classes?](#30-when-should-you-use-nested-classes)
- [31. Advantages](#31-advantages)
- [32. Limitations](#32-limitations)
- [33. Common Mistakes](#33-common-mistakes)
- [34. Top 25 Interview Questions](#34-top-25-interview-questions)
- [35. 30-Second Interview Answer](#35-30-second-interview-answer)
- [36. Cheat Sheet](#36-cheat-sheet)
- [37. Final Mental Model](#37-final-mental-model)

---

# 1. What Is a Nested Class?

A nested class is a class declared inside another class or interface.

Example:

```java
class Outer {

    class Inner {
    }
}
```

Here:

```text
Outer
 └── Inner
```

`Inner` is nested inside `Outer`.

Nested classes are useful when a class is closely related to another class and does not need to be exposed as a completely separate top-level type.

---

# 2. Why Do We Need Nested Classes?

Nested classes are commonly used for:

```text
Encapsulation
Logical grouping
Improved readability
Implementation details
Builder patterns
Helper classes
Callbacks
Event handling
Data structures
```

For example, suppose a `LinkedList` contains a node type that is meaningful mainly as an implementation detail.

Conceptually:

```text
LinkedList
 └── Node
```

The `Node` class does not necessarily need to be exposed as an independent top-level class.

A nested class keeps related implementation together.

---

# 3. Types of Nested Classes

Java broadly divides nested classes into:

```text
Nested Classes
│
├── Static Nested Class
│
└── Inner Classes
    │
    ├── Member Inner Class
    ├── Local Inner Class
    └── Anonymous Inner Class
```

Important terminology:

```text
Nested class
→ any class declared inside another class/interface

Inner class
→ a non-static nested class
```

Therefore:

```text
Every inner class is nested.

Not every nested class is an inner class.
```

---

# 4. Static Nested Class

A static nested class is a nested class declared using `static`.

Example:

```java
class Outer {

    static class Nested {

        void display() {

            System.out.println(
                "Static nested class"
            );
        }
    }
}
```

Here:

```text
Outer
 └── static Nested
```

The nested class does not require an instance of `Outer` to be created.

---

# 5. Creating an Object of a Static Nested Class

A static nested class is created using the outer class name.

Example:

```java
class Outer {

    static class Nested {

        void display() {

            System.out.println(
                "Hello"
            );
        }
    }
}
```

Create object:

```java
Outer.Nested obj =
    new Outer.Nested();
```

Call method:

```java
obj.display();
```

Important:

```text
Outer object
→ not required

Outer.Nested object
→ required
```

---

# 6. Accessing Outer Class Members

A static nested class can directly access static members of the outer class.

Example:

```java
class Outer {

    static int value = 100;

    static class Nested {

        void display() {

            System.out.println(
                value
            );
        }
    }
}
```

But it cannot directly access an outer instance field without an outer object reference.

Example:

```java
class Outer {

    int value = 100;

    static class Nested {

        void display() {

            // Cannot directly access
            // outer instance field.
        }
    }
}
```

The reason is simple:

```text
Static nested class
→ does not automatically have
  an Outer object reference.
```

---

# 7. Non-Static Member Inner Class

A non-static class declared directly inside another class is called a member inner class.

Example:

```java
class Outer {

    class Inner {

        void display() {

            System.out.println(
                "Inner class"
            );
        }
    }
}
```

This is:

```text
non-static nested class
```

Therefore it is an:

```text
inner class
```

---

# 8. Creating an Inner Class Object

Unlike a static nested class, a member inner class requires an instance of the outer class.

Example:

```java
class Outer {

    class Inner {

        void display() {

            System.out.println(
                "Hello"
            );
        }
    }
}
```

Create outer object:

```java
Outer outer =
    new Outer();
```

Create inner object:

```java
Outer.Inner inner =
    outer.new Inner();
```

Call method:

```java
inner.display();
```

The syntax is important:

```text
outer.new Inner()
```

not:

```text
new Outer.Inner()
```

---

# 9. Accessing Outer Class Members from Inner Class

An inner class can directly access members of its enclosing outer class.

Example:

```java
class Outer {

    private int value = 100;

    class Inner {

        void display() {

            System.out.println(
                value
            );
        }
    }
}
```

The inner class can access:

```text
private
default
protected
public
```

members of the enclosing class, subject to the normal language rules.

This is one of the useful properties of inner classes.

---

# 10. Outer Class Reference

A non-static inner class is associated with an instance of the outer class.

Conceptually:

```text
Outer object
      ↑
      |
Inner object
```

The inner object has access to its enclosing outer instance.

This allows:

```java
class Outer {

    int value = 100;

    class Inner {

        void display() {

            System.out.println(
                value
            );
        }
    }
}
```

to access:

```text
Outer.this.value
```

---

# 11. Private Members and Nested Classes

Nested classes can access private members of their enclosing class.

Example:

```java
class BankAccount {

    private double balance =
        5000;

    class Details {

        void showBalance() {

            System.out.println(
                balance
            );
        }
    }
}
```

The nested class can access:

```text
private balance
```

because nested classes are part of the same enclosing type's implementation context.

---

# 12. Local Classes

A local class is a class declared inside a method, constructor, or block.

Example:

```java
class Outer {

    void display() {

        class Local {

            void message() {

                System.out.println(
                    "Local class"
                );
            }
        }

        Local obj =
            new Local();

        obj.message();
    }
}
```

The scope of `Local` is limited to the block in which it is declared.

Outside that method:

```text
Local
```

cannot normally be referenced by its source-level name.

---

# 13. Anonymous Classes

An anonymous class is a class without an explicit class name.

Example:

```java
abstract class Animal {

    abstract void sound();
}
```

Anonymous implementation:

```java
Animal animal =
    new Animal() {

        @Override
        void sound() {

            System.out.println(
                "Bark"
            );
        }
    };
```

There is no explicit class name.

Anonymous classes are useful when:

```text
One-time implementation
Small callback
Quick interface implementation
Quick abstract class implementation
```

is required.

---

# 14. Nested Class Inside an Interface

A nested type declared inside an interface is implicitly:

```text
public
static
```

Example:

```java
interface Vehicle {

    class Engine {

        void start() {

            System.out.println(
                "Engine started"
            );
        }
    }
}
```

Conceptually:

```text
Vehicle.Engine
```

is a static nested class.

Create object:

```java
Vehicle.Engine engine =
    new Vehicle.Engine();
```

---

# 15. Static Nested Class vs Inner Class

| Feature | Static Nested Class | Inner Class |
|---|---|---|
| `static` | Yes | No |
| Outer object required | No | Yes |
| Has enclosing instance | No | Yes |
| Direct access to outer instance fields | No | Yes |
| Access static outer members | Yes | Yes |
| Creation syntax | `new Outer.Nested()` | `outer.new Inner()` |

Example static:

```java
Outer.Nested obj =
    new Outer.Nested();
```

Example inner:

```java
Outer outer =
    new Outer();

Outer.Inner obj =
    outer.new Inner();
```

---

# 16. Member Nested Class

A member nested class is declared directly inside another class.

It can be:

```text
static nested class
```

or:

```text
non-static inner class
```

Example:

```java
class Outer {

    static class StaticNested {
    }

    class Inner {
    }
}
```

Both are member classes.

---

# 17. Access Modifiers

Nested classes can have access modifiers when they are members.

Example:

```java
class Outer {

    private class PrivateInner {
    }

    protected class ProtectedInner {
    }

    public class PublicInner {
    }

    class PackageInner {
    }
}
```

The access modifier controls who can access the nested class.

Local and anonymous classes have different rules because their scope is determined by where they are declared.

---

# 18. `this` in Nested Classes

Inside an inner class:

```java
this
```

refers to the current inner-class object.

Example:

```java
class Outer {

    class Inner {

        void display() {

            System.out.println(
                this
            );
        }
    }
}
```

Here:

```text
this
→ Inner object
```

It does not refer to the outer object.

---

# 19. `OuterClass.this`

To explicitly refer to the enclosing outer object, use:

```text
OuterClass.this
```

Example:

```java
class Outer {

    int value = 100;

    class Inner {

        int value = 200;

        void display() {

            System.out.println(
                this.value
            );

            System.out.println(
                Outer.this.value
            );
        }
    }
}
```

Output:

```text
200
100
```

Meaning:

```text
this.value
→ Inner.value

Outer.this.value
→ Outer.value
```

This is extremely important in nested-class interviews.

---

# 20. Shadowing in Nested Classes

A variable in an inner class can have the same name as an outer variable.

Example:

```java
class Outer {

    int value = 100;

    class Inner {

        int value = 200;

        void display() {

            System.out.println(
                value
            );

            System.out.println(
                this.value
            );

            System.out.println(
                Outer.this.value
            );
        }
    }
}
```

Output:

```text
200
200
100
```

The names resolve as:

```text
value
→ nearest matching member

this.value
→ inner class member

Outer.this.value
→ outer class member
```

---

# 21. Multiple Levels of Nesting

Classes can be nested multiple levels deep.

Example:

```java
class A {

    class B {

        class C {

            void display() {

                System.out.println(
                    "C"
                );
            }
        }
    }
}
```

Object creation:

```java
A a =
    new A();
```

```java
A.B b =
    a.new B();
```

```java
A.B.C c =
    b.new C();
```

Nested structures should be used carefully because excessive nesting can reduce readability.

---

# 22. Nested Classes and Inheritance

A nested class can extend another class.

Example:

```java
class Outer {

    static class Child
        extends Parent {
    }
}
```

Nested classes themselves can also participate in normal inheritance relationships.

Example:

```java
class Parent {

    void display() {
    }
}
```

```java
class Outer {

    class Child
        extends Parent {

        @Override
        void display() {

            System.out.println(
                "Child"
            );
        }
    }
}
```

Being nested does not prevent normal inheritance.

---

# 23. Nested Classes and Static Members

A static nested class can freely declare static members.

Example:

```java
class Outer {

    static class Nested {

        static int count = 10;

        static void display() {

            System.out.println(
                count
            );
        }
    }
}
```

Access:

```java
System.out.println(
    Outer.Nested.count
);
```

Call:

```java
Outer.Nested.display();
```

---

# 24. Java 16+ Static Members in Inner Classes

Modern Java allows inner classes to declare static members more freely than older Java versions.

Example:

```java
class Outer {

    class Inner {

        static int count = 10;

        static void display() {

            System.out.println(
                count
            );
        }
    }
}
```

This became possible with Java 16.

For modern Java development, do not rely on old rules that claim:

```text
"An inner class can never have static members."
```

That statement is outdated.

The older restriction was relaxed in Java 16.

---

# 25. Nested Classes and Encapsulation

Nested classes can help hide implementation details.

Example:

```java
class LinkedList {

    private class Node {

        int data;
        Node next;

        Node(int data) {

            this.data = data;
        }
    }
}
```

The `Node` type is an implementation detail of the linked list.

Conceptually:

```text
LinkedList
 └── Node
```

This keeps related implementation together.

---

# 26. Nested Classes and Generics

Nested classes can be generic.

Example:

```java
class Outer<T> {

    class Inner {

        T value;

        Inner(T value) {

            this.value = value;
        }
    }
}
```

Usage:

```java
Outer<String> outer =
    new Outer<>();
```

```java
Outer<String>.Inner inner =
    outer.new Inner("Java");
```

The inner class can use the type parameter of the enclosing generic class.

A static nested class cannot directly use the outer class's type parameter because it is not associated with an outer instance.

Example:

```java
class Outer<T> {

    static class Nested {

        // Cannot directly use T
        // here as an instance type.
    }
}
```

---

# 27. Nested Classes and Interfaces

A nested class can implement an interface.

Example:

```java
interface Printable {

    void print();
}
```

```java
class Printer {

    static class Job
        implements Printable {

        @Override
        public void print() {

            System.out.println(
                "Printing"
            );
        }
    }
}
```

Usage:

```java
Printable job =
    new Printer.Job();

job.print();
```

Nested classes can therefore participate in polymorphism normally.

---

# 28. Bytecode and JVM View

At the source-code level:

```java
class Outer {

    static class Nested {
    }

    class Inner {
    }
}
```

The compiler generates separate class files for the nested types.

Conceptually:

```text
Outer.class
Outer$Nested.class
Outer$Inner.class
```

The `$` naming convention is commonly visible in compiled class files.

However, this naming should be treated as a compiler/class-file representation detail rather than a Java source-level rule.

---

# 29. Synthetic Access and Modern Java

Older Java compiler implementations sometimes generated synthetic accessor methods to allow nested types to access private members of their enclosing classes.

Modern Java has nest-based access support in the JVM, introduced with Java 11, which provides a more direct mechanism for access between nestmates.

The key interview idea is:

```text
Nested classes can access private
members of their enclosing types.
```

Do not over-focus on synthetic accessor implementation details unless the interviewer specifically asks about bytecode or JVM internals.

---

# 30. When Should You Use Nested Classes?

Use a nested class when:

```text
The class has a strong relationship
with the enclosing class.

The class is mainly an implementation detail.

The class is useful only within
a particular context.

You want better encapsulation.

A helper type belongs conceptually
to one outer type.
```

Example:

```text
LinkedList
 └── Node
```

is a natural use case.

A class that has independent responsibilities and is used throughout an application may be better as a top-level class.

---

# 31. Advantages

## 1. Encapsulation

Implementation details can remain inside the enclosing class.

---

## 2. Logical Grouping

Related classes stay together.

---

## 3. Access to Outer Members

Inner classes can access members of their enclosing instance.

---

## 4. Better Locality

Small helper classes can stay close to the code that uses them.

---

## 5. Useful for Design Patterns

Nested classes are commonly seen in:

```text
Builder pattern
Iterator implementations
Helper classes
Callbacks
Factory implementations
```

---

# 32. Limitations

## 1. Can Reduce Readability

Deep nesting can make code harder to understand.

---

## 2. Inner Classes Carry an Enclosing Relationship

A non-static inner class is associated with an outer instance.

---

## 3. Potential Memory Retention

If an inner-class object outlives the outer object through another reference, its association with the outer instance can keep that outer object reachable.

This matters especially in long-lived callbacks/listeners.

---

## 4. More Complex Syntax

Creating inner-class objects requires syntax such as:

```java
outer.new Inner();
```

---

## 5. Overuse

Not every helper class needs to be nested.

---

# 33. Common Mistakes

## ❌ Mistake 1 — Confusing Nested and Inner Classes

Remember:

```text
Nested
→ static + non-static

Inner
→ non-static nested only
```

---

## ❌ Mistake 2 — Creating Inner Class Without Outer Object

This is invalid:

```java
Outer.Inner obj =
    new Outer.Inner();
```

For a non-static inner class use:

```java
Outer outer =
    new Outer();

Outer.Inner obj =
    outer.new Inner();
```

---

## ❌ Mistake 3 — Thinking Static Nested Class Needs Outer Object

It does not.

```java
Outer.Nested obj =
    new Outer.Nested();
```

---

## ❌ Mistake 4 — Confusing `this` and `Outer.this`

Inside an inner class:

```text
this
→ inner object

Outer.this
→ enclosing outer object
```

---

## ❌ Mistake 5 — Assuming Inner Class Cannot Access Private Members

It can access members of its enclosing class.

---

## ❌ Mistake 6 — Saying Inner Classes Can Never Have Static Members

That is outdated.

Modern Java, since Java 16, allows static members in inner classes under the updated language rules.

---

## ❌ Mistake 7 — Assuming Anonymous Classes Have Names

An anonymous class has no explicit source-level class name.

---

# 34. Top 25 Interview Questions

## 🔥 Q1. What is a nested class?

A class declared inside another class or interface.

---

## 🔥 Q2. What is an inner class?

A non-static nested class.

---

## 🔥 Q3. What are the types of nested classes?

The main categories are:

```text
Static nested class
Member inner class
Local class
Anonymous class
```

---

## 🔥 Q4. What is a static nested class?

A nested class declared with `static`.

It does not require an instance of the outer class.

---

## 🔥 Q5. What is a member inner class?

A non-static class declared directly inside another class.

---

## 🔥 Q6. How do you create a static nested class object?

```java
Outer.Nested obj =
    new Outer.Nested();
```

---

## 🔥 Q7. How do you create a member inner class object?

```java
Outer outer =
    new Outer();

Outer.Inner inner =
    outer.new Inner();
```

---

## 🔥 Q8. Why does an inner class require an outer object?

Because a non-static inner class is associated with an enclosing instance.

---

## 🔥 Q9. Can an inner class access private outer members?

Yes.

---

## 🔥 Q10. What does `this` refer to inside an inner class?

It refers to the current inner-class object.

---

## 🔥 Q11. How do you access the outer object explicitly?

Use:

```java
Outer.this
```

---

## 🔥 Q12. What is a local class?

A class declared inside a method, constructor, or block.

---

## 🔥 Q13. What is an anonymous class?

A class declared and instantiated without an explicit source-level class name.

---

## 🔥 Q14. Can a nested class implement an interface?

Yes.

---

## 🔥 Q15. Can a nested class extend another class?

Yes.

---

## 🔥 Q16. Can an inner class be static?

The terminology matters.

A class declared `static` inside another class is called a static nested class, not an inner class.

---

## 🔥 Q17. Can an inner class have static members?

Yes, under modern Java language rules; the previous broad restriction was relaxed in Java 16.

---

## 🔥 Q18. What is the difference between nested and inner class?

```text
Nested
→ all classes declared inside another type

Inner
→ non-static nested classes
```

---

## 🔥 Q19. What is the difference between static nested and inner class?

```text
Static nested
→ no enclosing instance

Inner
→ associated with enclosing instance
```

---

## 🔥 Q20. Can an interface contain a nested class?

Yes.

A nested class declared in an interface is implicitly public and static.

---

## 🔥 Q21. Why use nested classes?

For:

```text
Encapsulation
Logical grouping
Implementation hiding
Better organization
```

---

## 🔥 Q22. Can a static nested class access outer instance fields directly?

No.

It has no implicit enclosing instance.

---

## 🔥 Q23. Can an inner class access static outer members?

Yes.

---

## 🔥 Q24. What does the compiler-generated class file often look like?

Conceptually:

```text
Outer$Inner.class
```

---

## 🔥 Q25. When should you avoid nested classes?

When nesting makes the design harder to understand or when the class has an independent responsibility and should be reusable as a separate type.

---

# 35. 30-Second Interview Answer

> A nested class is a class declared inside another class or interface. Java has static nested classes and non-static inner classes, with member, local, and anonymous classes being the main forms. A static nested class does not require an outer object, while a non-static inner class is associated with an enclosing instance and can directly access its members, including private members. Nested classes are useful for encapsulation, logical grouping, and hiding implementation details. Inside an inner class, `this` refers to the inner object, while `OuterClass.this` refers to the enclosing outer object.

---

# 36. Cheat Sheet

```text
========================================================
                  JAVA NESTED CLASSES
========================================================


CORE IDEA
--------------------------------------------------------

Class inside another class/interface
→ Nested class


========================================================

MAIN TYPES
--------------------------------------------------------

Nested Classes
│
├── Static Nested Class
│
└── Inner Classes
    │
    ├── Member Inner Class
    ├── Local Class
    └── Anonymous Class


========================================================

IMPORTANT DEFINITION
--------------------------------------------------------

Nested
→ any class inside another type


Inner
→ non-static nested class


Therefore:


Every Inner Class
→ Nested Class


Every Nested Class
→ NOT necessarily Inner Class


========================================================

STATIC NESTED CLASS
--------------------------------------------------------

class Outer {

    static class Nested {
    }
}


Creation:

Outer.Nested obj =
    new Outer.Nested();


Outer object
→ NOT required


========================================================

INNER CLASS
--------------------------------------------------------

class Outer {

    class Inner {
    }
}


Creation:

Outer outer =
    new Outer();


Outer.Inner obj =
    outer.new Inner();


Outer object
→ REQUIRED


========================================================

ACCESS
--------------------------------------------------------

Static Nested:
→ static outer members directly


Inner:
→ outer instance members
→ outer static members


Both:
→ can access permitted outer members


========================================================

THIS
--------------------------------------------------------

this
→ current inner object


Outer.this
→ outer object


========================================================

SHADOWING
--------------------------------------------------------

this.value
→ inner value


Outer.this.value
→ outer value


========================================================

LOCAL CLASS
--------------------------------------------------------

class Outer {

    void test() {

        class Local {
        }
    }
}


Scope
→ local block


========================================================

ANONYMOUS CLASS
--------------------------------------------------------

Animal animal =
    new Animal() {

        @Override
        void sound() {
        }
    };


No explicit class name


========================================================

INTERFACE NESTED CLASS
--------------------------------------------------------

interface Vehicle {

    class Engine {
    }
}


Implicitly:

public static


========================================================

INHERITANCE
--------------------------------------------------------

Nested class
→ can extend class


Nested class
→ can implement interface


========================================================

GENERIC OUTER
--------------------------------------------------------

class Outer<T> {

    class Inner {

        T value;
    }
}


Inner can use
outer's type parameter.


========================================================

STATIC NESTED
--------------------------------------------------------

Static nested class
→ no outer instance
→ cannot directly use outer instance fields


========================================================

MODERN JAVA
--------------------------------------------------------

Java 16+
→ static members are allowed
   in inner classes under
   updated language rules.


========================================================

JVM / CLASS FILE
--------------------------------------------------------

Outer.class

Outer$Nested.class

Outer$Inner.class


$ naming
→ class-file representation detail


========================================================

MAIN USE CASE
--------------------------------------------------------

Outer class
    ↓
Implementation detail
    ↓
Nested class


Example:


LinkedList
    ↓
Node


========================================================

MEMORY TRICK
--------------------------------------------------------

STATIC NESTED
→ No outer object


INNER
→ Needs outer object


LOCAL
→ Inside method/block


ANONYMOUS
→ No explicit class name


========================================================
```

---

# 37. Final Mental Model

```text
                         NESTED CLASSES
                                |
              +-----------------+-----------------+
              |                                   |
              ↓                                   ↓
        STATIC NESTED                         INNER
              |                                   |
              |                     +-------------+-------------+
              |                     |             |             |
              ↓                     ↓             ↓             ↓
        No outer object          Member        Local       Anonymous
              |                  Inner         Class         Class
              |
              ↓
       Outer.Nested


========================================================

STATIC NESTED


Outer
  |
  └── static Nested

Creation:

new Outer.Nested()


Relationship:

Nested
  X
  |
  X
No implicit Outer object


========================================================

INNER CLASS


Outer object
     |
     ↓
Inner object


Creation:

outer.new Inner()


Relationship:

Inner
  |
  ↓
Outer.this


========================================================

THIS vs OUTER.THIS


Inside Inner:


this
 ↓
Inner object


Outer.this
 ↓
Outer object


Example:


class Outer {

    int value = 100;

    class Inner {

        int value = 200;

        void display() {

            this.value;
            Outer.this.value;
        }
    }
}


this.value
→ 200


Outer.this.value
→ 100


========================================================

NESTED CLASS ACCESS


Static Nested
      |
      +----→ static outer members


Inner
      |
      +----→ instance outer members
      |
      +----→ static outer members
      |
      +----→ private outer members


========================================================

NESTED CLASS PURPOSE


Related functionality
        ↓
Logical grouping
        ↓
Encapsulation
        ↓
Implementation hiding


========================================================

MOST IMPORTANT INTERVIEW LINE


"Every inner class is a nested class,
but every nested class is not an inner class."


========================================================

SECOND MOST IMPORTANT


"Static nested classes do not require
an instance of the enclosing class,
whereas non-static inner classes
are associated with an enclosing instance."


========================================================

FINAL MEMORY MAP


Nested
 |
 +-- Static Nested
 |      |
 |      +-- No outer object
 |
 +-- Inner
        |
        +-- Member
        |
        +-- Local
        |
        +-- Anonymous


========================================================
```

---

# 🏁 Final Revision Checklist

- [ ] What is a nested class?
- [ ] What is an inner class?
- [ ] Nested vs inner class
- [ ] Static nested class
- [ ] Member inner class
- [ ] Local class
- [ ] Anonymous class
- [ ] Static nested class object creation
- [ ] Inner class object creation
- [ ] Why inner class needs outer object
- [ ] Accessing outer members
- [ ] Accessing private outer members
- [ ] `this`
- [ ] `OuterClass.this`
- [ ] Variable shadowing
- [ ] Nested class inside interface
- [ ] Nested class access modifiers
- [ ] Nested class inheritance
- [ ] Nested class interfaces
- [ ] Nested classes with generics
- [ ] Static members in inner classes
- [ ] Java 16 change
- [ ] JVM/class-file representation
- [ ] Encapsulation use cases
- [ ] Memory implications of inner classes
- [ ] When to use nested classes
- [ ] When to avoid nested classes

---

# 🧠 One-Line Memory

```text
Nested = class inside another type

Static Nested
→ no outer object

Inner
→ has outer-instance relationship

Member
→ inside class

Local
→ inside method/block

Anonymous
→ no explicit class name

this
→ inner object

Outer.this
→ outer object
```

