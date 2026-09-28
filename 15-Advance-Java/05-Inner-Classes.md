# 🧩 Inner Classes in Java

> **An inner class is a non-static nested class that is associated with an instance of its enclosing class and can directly access the enclosing class's members.**

---

# 📑 Table of Contents

- [1. What Is an Inner Class?](#1-what-is-an-inner-class)
- [2. Nested Class vs Inner Class](#2-nested-class-vs-inner-class)
- [3. Types of Inner Classes](#3-types-of-inner-classes)
- [4. Member Inner Class](#4-member-inner-class)
- [5. Creating a Member Inner Class Object](#5-creating-a-member-inner-class-object)
- [6. Why Does an Inner Class Need an Outer Object?](#6-why-does-an-inner-class-need-an-outer-object)
- [7. Accessing Outer Class Members](#7-accessing-outer-class-members)
- [8. Accessing Private Members](#8-accessing-private-members)
- [9. `this` vs `Outer.this`](#9-this-vs-outerthis)
- [10. Variable Shadowing](#10-variable-shadowing)
- [11. Inner Class with Static Members](#11-inner-class-with-static-members)
- [12. Local Inner Classes](#12-local-inner-classes)
- [13. Anonymous Inner Classes](#13-anonymous-inner-classes)
- [14. Inner Class Implementing an Interface](#14-inner-class-implementing-an-interface)
- [15. Inner Class Extending a Class](#15-inner-class-extending-a-class)
- [16. Inner Classes and Inheritance](#16-inner-classes-and-inheritance)
- [17. Inner Classes and Encapsulation](#17-inner-classes-and-encapsulation)
- [18. Inner Classes and Generics](#18-inner-classes-and-generics)
- [19. Inner Classes and Outer Class Type Parameters](#19-inner-classes-and-outer-class-type-parameters)
- [20. Inner Classes and Method Parameters](#20-inner-classes-and-method-parameters)
- [21. Inner Classes and Effectively Final Variables](#21-inner-classes-and-effectively-final-variables)
- [22. Inner Classes and Memory](#22-inner-classes-and-memory)
- [23. JVM View of Inner Classes](#23-jvm-view-of-inner-classes)
- [24. Inner Classes and Synthetic Members](#24-inner-classes-and-synthetic-members)
- [25. Inner Class vs Static Nested Class](#25-inner-class-vs-static-nested-class)
- [26. Inner Class vs Local Class](#26-inner-class-vs-local-class)
- [27. Inner Class vs Anonymous Class](#27-inner-class-vs-anonymous-class)
- [28. When Should You Use Inner Classes?](#28-when-should-you-use-inner-classes)
- [29. Advantages](#29-advantages)
- [30. Limitations](#30-limitations)
- [31. Common Mistakes](#31-common-mistakes)
- [32. Top 25 Interview Questions](#32-top-25-interview-questions)
- [33. 30-Second Interview Answer](#33-30-second-interview-answer)
- [34. Cheat Sheet](#34-cheat-sheet)
- [35. Final Mental Model](#35-final-mental-model)

---

# 1. What Is an Inner Class?

An inner class is a **non-static nested class**.

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

Here:

```text
Outer
 └── Inner
```

`Inner` is:

```text
Nested class
+
Non-static
=
Inner class
```

The important property is that an inner-class object is associated with an instance of the outer class.

---

# 2. Nested Class vs Inner Class

This distinction is extremely important.

## Nested Class

A nested class is any class declared inside another class or interface.

It includes:

```text
Static nested class
Inner class
```

## Inner Class

An inner class is specifically a **non-static nested class**.

Therefore:

```text
Every inner class
→ nested class

Every nested class
→ not necessarily inner class
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

Here:

```text
StaticNested
→ nested
→ NOT inner

Inner
→ nested
→ inner
```

---

# 3. Types of Inner Classes

Java has three commonly discussed forms of inner classes:

```text
Inner Classes
│
├── Member Inner Class
├── Local Inner Class
└── Anonymous Inner Class
```

### Member Inner Class

Declared directly inside another class.

### Local Inner Class

Declared inside a method, constructor, or block.

### Anonymous Inner Class

Declared and instantiated without an explicit class name.

---

# 4. Member Inner Class

A member inner class is a non-static class declared directly inside another class.

Example:

```java
class Outer {

    class Inner {

        void display() {

            System.out.println(
                "Member inner class"
            );
        }
    }
}
```

Because `Inner` is not static:

```text
Inner
→ associated with Outer instance
```

---

# 5. Creating a Member Inner Class Object

First create the outer object:

```java
Outer outer =
    new Outer();
```

Then create the inner object:

```java
Outer.Inner inner =
    outer.new Inner();
```

Then call the method:

```java
inner.display();
```

Complete example:

```java
class Outer {

    class Inner {

        void display() {

            System.out.println(
                "Hello from Inner"
            );
        }
    }

    public static void main(
        String[] args
    ) {

        Outer outer =
            new Outer();

        Outer.Inner inner =
            outer.new Inner();

        inner.display();
    }
}
```

Important syntax:

```text
outer.new Inner()
```

not:

```text
new Outer.Inner()
```

---

# 6. Why Does an Inner Class Need an Outer Object?

A non-static inner class is associated with a particular instance of its enclosing class.

Suppose:

```java
class Outer {

    int value;

    Outer(int value) {

        this.value = value;
    }

    class Inner {

        void display() {

            System.out.println(
                value
            );
        }
    }
}
```

Now:

```java
Outer first =
    new Outer(100);

Outer second =
    new Outer(200);
```

Create two inner objects:

```java
Outer.Inner firstInner =
    first.new Inner();
```

```java
Outer.Inner secondInner =
    second.new Inner();
```

The first inner object is associated with:

```text
first
```

and the second with:

```text
second
```

Therefore:

```text
firstInner → first Outer object

secondInner → second Outer object
```

This is why an outer instance is required.

---

# 7. Accessing Outer Class Members

An inner class can directly access members of its enclosing outer class.

Example:

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

The inner class can access:

```text
value
```

without writing:

```text
Outer.this.value
```

The compiler understands the enclosing-instance relationship.

---

# 8. Accessing Private Members

An inner class can access private members of its enclosing class.

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

Usage:

```java
BankAccount account =
    new BankAccount();

BankAccount.Details details =
    account.new Details();

details.showBalance();
```

Output:

```text
5000.0
```

The `private` access restriction does not prevent the nested class from accessing the enclosing class's private members.

---

# 9. `this` vs `Outer.this`

This is one of the most important interview concepts.

Inside an inner class:

```text
this
```

refers to the inner-class object.

To refer to the outer object:

```text
Outer.this
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

---

# 10. Variable Shadowing

Suppose both classes have a variable with the same name.

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

Why?

```text
value
→ nearest matching member

this.value
→ current Inner object

Outer.this.value
→ enclosing Outer object
```

---

# 11. Inner Class with Static Members

Historically, Java restricted static declarations inside non-static inner classes.

Modern Java changed this.

Since Java 16, inner classes can declare static members under the updated language rules.

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

Access:

```java
System.out.println(
    Outer.Inner.count
);
```

And:

```java
Outer.Inner.display();
```

Important interview point:

```text
"Inner classes can never contain static members"
```

is an outdated statement for modern Java.

---

# 12. Local Inner Classes

A local inner class is declared inside:

```text
Method
Constructor
Block
```

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

The class exists within the scope of the method.

Outside:

```java
display()
```

the source-level name:

```text
Local
```

cannot normally be referenced.

---

# 13. Anonymous Inner Classes

An anonymous class has no explicit class name.

Example:

```java
abstract class Animal {

    abstract void sound();
}
```

Create an anonymous implementation:

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

Call:

```java
animal.sound();
```

Anonymous classes are commonly useful for one-time implementations.

---

# 14. Inner Class Implementing an Interface

An inner class can implement an interface.

Example:

```java
interface Printable {

    void print();
}
```

```java
class Printer {

    class Job
        implements Printable {

        @Override
        public void print() {

            System.out.println(
                "Printing..."
            );
        }
    }
}
```

Create object:

```java
Printer printer =
    new Printer();

Printable job =
    printer.new Job();
```

Call:

```java
job.print();
```

Normal polymorphism still works.

---

# 15. Inner Class Extending a Class

An inner class can extend another class.

Example:

```java
class Parent {

    void display() {

        System.out.println(
            "Parent"
        );
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

Create:

```java
Outer outer =
    new Outer();

Outer.Child child =
    outer.new Child();

child.display();
```

Output:

```text
Child
```

Being an inner class does not prevent normal inheritance.

---

# 16. Inner Classes and Inheritance

An inner class can participate in inheritance just like a normal class.

Example:

```java
class Animal {

    void sound() {
    }
}
```

```java
class Zoo {

    class Dog
        extends Animal {

        @Override
        void sound() {

            System.out.println(
                "Bark"
            );
        }
    }
}
```

The fact that `Dog` is nested inside `Zoo` does not change Java's normal inheritance rules.

---

# 17. Inner Classes and Encapsulation

Inner classes can help hide implementation details.

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

The `Node` class is closely tied to the implementation of:

```text
LinkedList
```

Keeping it nested helps communicate:

```text
Node is an implementation detail
of LinkedList.
```

This can improve encapsulation.

---

# 18. Inner Classes and Generics

An inner class can be declared inside a generic outer class.

Example:

```java
class Outer<T> {

    class Inner {

        T value;

        Inner(T value) {

            this.value = value;
        }

        T getValue() {

            return value;
        }
    }
}
```

Create:

```java
Outer<String> outer =
    new Outer<>();
```

Then:

```java
Outer<String>.Inner inner =
    outer.new Inner("Java");
```

The inner class can use:

```text
T
```

from the enclosing generic class.

---

# 19. Inner Classes and Outer Class Type Parameters

Consider:

```java
class Box<T> {

    class Item {

        T value;

        Item(T value) {

            this.value = value;
        }
    }
}
```

If:

```java
Box<Integer> box =
    new Box<>();
```

then:

```text
T
→ Integer
```

for the outer parameterization.

Create:

```java
Box<Integer>.Item item =
    box.new Item(100);
```

The inner class is associated with the parameterized outer instance.

Important:

A static nested class does not have this enclosing-instance relationship and cannot directly use the outer class's type parameter.

---

# 20. Inner Classes and Method Parameters

An inner class can access method-local variables only under specific rules when the inner class is local or anonymous.

Example:

```java
class Outer {

    void display() {

        int number = 100;

        class Local {

            void show() {

                System.out.println(
                    number
                );
            }
        }

        Local local =
            new Local();

        local.show();
    }
}
```

This works because:

```text
number
```

is not modified after initialization.

---

# 21. Inner Classes and Effectively Final Variables

A local or anonymous class can access a local variable if that variable is:

```text
final
```

or:

```text
effectively final
```

Example:

```java
class Outer {

    void display() {

        int number = 100;

        class Local {

            void show() {

                System.out.println(
                    number
                );
            }
        }

        new Local().show();
    }
}
```

`number` is effectively final because it is never reassigned.

This is invalid:

```java
class Outer {

    void display() {

        int number = 100;

        number = 200;

        class Local {

            void show() {

                System.out.println(
                    number
                );
            }
        }
    }
}
```

The local class cannot capture `number` because it is no longer effectively final.

---

# 22. Inner Classes and Memory

A non-static inner class maintains an association with an enclosing outer instance.

Conceptually:

```text
Inner object
     |
     ↓
Outer object
```

Therefore, if an inner object remains reachable for a long time, the associated outer object may also remain reachable.

This matters in cases such as:

```text
Long-lived callbacks
Listeners
Threads
Executors
Caches
GUI components
```

A static nested class does not automatically carry an enclosing-instance reference.

---

# 23. JVM View of Inner Classes

At source level:

```java
class Outer {

    class Inner {
    }
}
```

The compiler produces separate class-file representations.

Commonly:

```text
Outer.class
Outer$Inner.class
```

The `$` is part of the generated class-file naming convention.

Conceptually:

```text
Outer
  |
  +---- Outer.class
  |
  +---- Outer$Inner.class
```

The exact compiler and bytecode details are implementation concerns.

---

# 24. Inner Classes and Synthetic Members

Older Java implementations could generate synthetic fields or methods to support access between nested classes and their enclosing classes.

Modern JVMs support nest-based access.

The JVM introduced nestmate access in Java 11.

For interviews, remember:

```text
Inner class
→ has special access relationship
  with its enclosing class
```

You normally do not need to manually manage synthetic accessors.

---

# 25. Inner Class vs Static Nested Class

| Feature | Inner Class | Static Nested Class |
|---|---|---|
| `static` declaration | No | Yes |
| Outer instance required | Yes | No |
| Enclosing instance | Yes | No |
| Direct outer instance access | Yes | No |
| Static outer access | Yes | Yes |
| Creation | `outer.new Inner()` | `new Outer.Nested()` |
| Memory association | Outer instance relationship | No automatic outer instance |

Example:

```java
Outer outer =
    new Outer();

Outer.Inner inner =
    outer.new Inner();
```

Static nested:

```java
Outer.Nested nested =
    new Outer.Nested();
```

---

# 26. Inner Class vs Local Class

| Feature | Member Inner | Local Inner |
|---|---|---|
| Declared inside | Class | Method/block |
| Member of outer type | Yes | No |
| Scope | Outer type | Declaring block |
| Can have access modifier | Yes | No normal member access modifiers |
| Outer instance access | Yes | Yes, if applicable |

Example member:

```java
class Outer {

    class Inner {
    }
}
```

Example local:

```java
class Outer {

    void method() {

        class Local {
        }
    }
}
```

---

# 27. Inner Class vs Anonymous Class

| Feature | Member Inner Class | Anonymous Class |
|---|---|---|
| Explicit name | Yes | No |
| Reusable by name | Yes | Usually one-time |
| Declared as member | Yes | Usually expression |
| Can extend class | Yes | Yes |
| Can implement interface | Yes | Yes |
| Best for | Reusable helper | One-time implementation |

Anonymous example:

```java
Runnable task =
    new Runnable() {

        @Override
        public void run() {

            System.out.println(
                "Running"
            );
        }
    };
```

---

# 28. When Should You Use Inner Classes?

Use an inner class when:

```text
The class strongly depends on
the outer class instance.

The class needs direct access
to outer instance state.

The class is tightly coupled
to the outer class.

The class is mainly an
implementation detail.
```

Example:

```text
LinkedList
 └── Node
```

Another example:

```text
GUI Component
 └── Event Handler
```

However, if the nested class does not need an outer instance, a static nested class is often a better structural choice.

---

# 29. Advantages

## 1. Strong Encapsulation

Implementation details can remain inside the enclosing class.

---

## 2. Direct Outer Access

Inner classes can directly access outer instance members.

---

## 3. Logical Grouping

Closely related classes remain together.

---

## 4. Private Member Access

Inner classes can access private members of their enclosing class.

---

## 5. Useful for Callbacks

Inner and anonymous classes are useful for event and callback implementations.

---

# 30. Limitations

## 1. Outer Instance Association

A non-static inner object is associated with an outer object.

---

## 2. More Complex Object Creation

You need:

```java
outer.new Inner();
```

---

## 3. Possible Memory Retention

A long-lived inner object can keep its outer object reachable.

---

## 4. Reduced Reusability

A tightly coupled inner class may not be appropriate for independent reuse.

---

## 5. Excessive Nesting Hurts Readability

Deeply nested classes can make code difficult to maintain.

---

# 31. Common Mistakes

## ❌ Mistake 1 — Calling Every Nested Class an Inner Class

Incorrect.

```text
Static nested class
→ nested
→ not inner
```

Only non-static nested classes are inner classes.

---

## ❌ Mistake 2 — Creating Inner Class with `new Outer.Inner()`

For a member inner class this is invalid.

Use:

```java
Outer outer =
    new Outer();

Outer.Inner inner =
    outer.new Inner();
```

---

## ❌ Mistake 3 — Thinking `this` Means Outer Object

Inside the inner class:

```text
this
→ Inner object
```

Use:

```text
Outer.this
```

for the outer object.

---

## ❌ Mistake 4 — Thinking Inner Class Cannot Access Private Fields

It can access private members of the enclosing class.

---

## ❌ Mistake 5 — Forgetting Effectively Final Rules

Local and anonymous classes cannot capture a local variable that is reassigned.

---

## ❌ Mistake 6 — Saying Inner Classes Cannot Have Static Members

This is outdated for modern Java.

Java 16 relaxed the restriction.

---

## ❌ Mistake 7 — Using an Inner Class When No Outer State Is Needed

If the nested class does not need an enclosing instance, consider a static nested class.

---

# 32. Top 25 Interview Questions

## 🔥 Q1. What is an inner class?

A non-static nested class associated with an instance of its enclosing class.

---

## 🔥 Q2. What is the difference between nested and inner class?

```text
Nested
→ any class declared inside another type

Inner
→ non-static nested class
```

---

## 🔥 Q3. What are the types of inner classes?

Common forms:

```text
Member inner class
Local inner class
Anonymous inner class
```

---

## 🔥 Q4. Why does a member inner class need an outer object?

Because it has an enclosing-instance relationship with a specific outer object.

---

## 🔥 Q5. How do you create a member inner class?

```java
Outer outer =
    new Outer();

Outer.Inner inner =
    outer.new Inner();
```

---

## 🔥 Q6. Can an inner class access private members?

Yes.

---

## 🔥 Q7. What does `this` refer to inside an inner class?

The current inner-class object.

---

## 🔥 Q8. How do you access the outer object?

```java
Outer.this
```

---

## 🔥 Q9. Can an inner class extend another class?

Yes.

---

## 🔥 Q10. Can an inner class implement an interface?

Yes.

---

## 🔥 Q11. Can an inner class have static members?

Yes, under modern Java rules since Java 16.

---

## 🔥 Q12. Can an inner class be declared inside an interface?

A class declared inside an interface is implicitly static, so it is a nested class rather than a non-static inner class.

---

## 🔥 Q13. What is a local inner class?

A class declared inside a method, constructor, or block.

---

## 🔥 Q14. What is an anonymous inner class?

A class without an explicit source-level name, usually created for a one-time implementation.

---

## 🔥 Q15. Can a local class access method variables?

Yes, if those variables are final or effectively final.

---

## 🔥 Q16. What is an effectively final variable?

A variable that is not explicitly declared `final` but is never reassigned after initialization.

---

## 🔥 Q17. Can an inner class access static members of the outer class?

Yes.

---

## 🔥 Q18. Can a static nested class directly access outer instance fields?

No.

---

## 🔥 Q19. What is the JVM relationship between an inner class and outer class?

A non-static inner class has an enclosing-instance relationship with an outer object.

---

## 🔥 Q20. What class file is commonly generated for an inner class?

Conceptually:

```text
Outer$Inner.class
```

---

## 🔥 Q21. Why can an inner class access private outer members?

Java's language and JVM access rules provide the necessary enclosing/nest relationship.

---

## 🔥 Q22. What is a major memory concern with inner classes?

A non-static inner object is associated with an outer object, so a long-lived inner object can keep that outer object reachable.

---

## 🔥 Q23. When should you prefer a static nested class?

When the nested class does not need an enclosing outer instance.

---

## 🔥 Q24. Can an inner class be inherited?

An inner class can extend another class and can itself be inherited like other classes, subject to normal Java access and inheritance rules.

---

## 🔥 Q25. What is the most important difference between inner and static nested class?

```text
Inner
→ enclosing outer instance

Static nested
→ no enclosing outer instance
```

---

# 33. 30-Second Interview Answer

> An inner class is a non-static nested class that is associated with an instance of its enclosing class. Because of this relationship, a member inner class is created using an outer object, such as `outer.new Inner()`, and it can directly access the outer class's members, including private members. Inside the inner class, `this` refers to the inner object, while `Outer.this` refers to the enclosing object. Java also has local and anonymous inner classes. Inner classes are useful for tightly coupled implementation details, callbacks, and encapsulation, while a static nested class should generally be used when no outer instance is required.

---

# 34. Cheat Sheet

```text
========================================================
                    JAVA INNER CLASSES
========================================================


CORE IDEA
--------------------------------------------------------

Inner Class
→ non-static nested class


========================================================

NESTED vs INNER
--------------------------------------------------------

Nested
→ static + non-static nested classes


Inner
→ non-static nested classes only


Memory:

Every Inner
→ Nested


Every Nested
→ NOT necessarily Inner


========================================================

TYPES
--------------------------------------------------------

Member Inner
Local Inner
Anonymous Inner


========================================================

MEMBER INNER
--------------------------------------------------------

class Outer {

    class Inner {
    }
}


Creation:

Outer outer =
    new Outer();


Outer.Inner inner =
    outer.new Inner();


========================================================

WHY OUTER OBJECT?
--------------------------------------------------------

Inner object
     |
     ↓
Outer object


Inner
→ associated with
  specific outer instance


========================================================

ACCESS
--------------------------------------------------------

Inner can directly access:

Outer instance members
Outer static members
Outer private members


========================================================

THIS
--------------------------------------------------------

this
→ Inner object


Outer.this
→ Outer object


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

Inside:

method
constructor
block


========================================================

ANONYMOUS CLASS
--------------------------------------------------------

No explicit class name


new Interface() {

    @Override
    public void run() {
    }
};


========================================================

EFFECTIVELY FINAL
--------------------------------------------------------

Local/anonymous classes
can capture local variables
that are:

final

OR

effectively final


========================================================

STATIC MEMBERS
--------------------------------------------------------

Java 16+
→ inner classes can declare
  static members under modern rules.


========================================================

GENERIC OUTER
--------------------------------------------------------

class Box<T> {

    class Item {

        T value;
    }
}


Inner can use T
from enclosing generic class.


========================================================

MEMORY
--------------------------------------------------------

Inner object
→ enclosing outer instance


Long-lived inner object
→ may keep outer object reachable


========================================================

CLASS FILE
--------------------------------------------------------

Outer.class

Outer$Inner.class


========================================================

STATIC NESTED COMPARISON
--------------------------------------------------------

Inner:
→ outer object required


Static Nested:
→ outer object not required


========================================================

MAIN USE CASES
--------------------------------------------------------

Encapsulation
Implementation details
Callbacks
Event handling
Closely coupled helpers


========================================================
```

---

# 35. Final Mental Model

```text
                         INNER CLASS
                              |
                              ↓
                    Non-Static Nested Class
                              |
                              ↓
                    Enclosing Outer Instance
                              |
              +---------------+---------------+
              |               |               |
              ↓               ↓               ↓
           Member          Local          Anonymous
              |               |               |
              |               |               |
              ↓               ↓               ↓
        Class member      Method/block      No name


========================================================

MEMBER INNER CLASS


Outer object
     |
     ↓
Inner object
     |
     ↓
Outer.this


Creation:

outer.new Inner()


========================================================

THIS vs OUTER.THIS


Inner object
    |
    +── this
    |     ↓
    |   Inner
    |
    +── Outer.this
          ↓
        Outer


========================================================

ACCESS MODEL


Inner
 |
 +----→ Outer private members
 |
 +----→ Outer instance members
 |
 +----→ Outer static members


========================================================

LOCAL CLASS


Method
  |
  +── Local class
       |
       +── can capture
           effectively-final
           local variables


========================================================

ANONYMOUS CLASS


Expression
    |
    ↓
Anonymous implementation
    |
    ↓
One-time usage


========================================================

MEMORY MODEL


Outer Object
     ↑
     |
enclosing relationship
     |
     ↓
Inner Object


Therefore:


Long-lived Inner
      ↓
Outer may remain reachable


========================================================

INNER vs STATIC NESTED


INNER
 |
 +── Non-static
 +── Outer instance
 +── outer.new Inner()


STATIC NESTED
 |
 +── static
 +── No outer instance
 +── new Outer.Nested()


========================================================

MOST IMPORTANT INTERVIEW LINE


"An inner class is a non-static nested class
that has an enclosing-instance relationship
with an object of its outer class."


========================================================

SECOND MOST IMPORTANT


"this refers to the inner object,
while Outer.this refers to the enclosing
outer object."


========================================================

FINAL MEMORY MAP


Nested Classes
       |
       +-------------------+
       |                   |
       ↓                   ↓
Static Nested            Inner
                           |
              +------------+------------+
              |            |            |
              ↓            ↓            ↓
           Member        Local      Anonymous


========================================================
```

---

# 🏁 Final Revision Checklist

- [ ] Definition of inner class
- [ ] Nested vs inner class
- [ ] Member inner class
- [ ] Local inner class
- [ ] Anonymous inner class
- [ ] Creating member inner class objects
- [ ] Why outer object is required
- [ ] Accessing outer members
- [ ] Accessing private members
- [ ] `this`
- [ ] `Outer.this`
- [ ] Variable shadowing
- [ ] Static members in modern inner classes
- [ ] Java 16 change
- [ ] Effectively final variables
- [ ] Local class variable capture
- [ ] Anonymous class variable capture
- [ ] Inner class implementing interface
- [ ] Inner class extending class
- [ ] Inner classes and generics
- [ ] Outer generic type parameters
- [ ] Memory relationship
- [ ] JVM/class-file representation
- [ ] Synthetic/nest access
- [ ] Inner vs static nested
- [ ] Inner vs local
- [ ] Inner vs anonymous
- [ ] Encapsulation use cases
- [ ] When to prefer static nested class

---

# 🧠 One-Line Memory

```text
Inner Class
= Non-static Nested Class
= Has Outer Instance Relationship

outer.new Inner()
→ create inner object

this
→ Inner

Outer.this
→ Outer

Member
→ class level

Local
→ method/block

Anonymous
→ no explicit name
```

