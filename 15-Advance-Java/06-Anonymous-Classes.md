# 🎭 Anonymous Classes in Java

> **An anonymous class is a class without an explicit name that is declared and instantiated at the same time, typically for a one-time implementation of an interface, abstract class, or concrete class.**

---

# 📑 Table of Contents

- [1. What Is an Anonymous Class?](#1-what-is-an-anonymous-class)
- [2. Why Anonymous Classes?](#2-why-anonymous-classes)
- [3. Basic Syntax](#3-basic-syntax)
- [4. Anonymous Class Implementing an Interface](#4-anonymous-class-implementing-an-interface)
- [5. Anonymous Class Extending a Class](#5-anonymous-class-extending-a-class)
- [6. Anonymous Class with Abstract Class](#6-anonymous-class-with-abstract-class)
- [7. Anonymous Class with Concrete Class](#7-anonymous-class-with-concrete-class)
- [8. Calling Methods](#8-calling-methods)
- [9. Anonymous Class Object](#9-anonymous-class-object)
- [10. Anonymous Class vs Named Class](#10-anonymous-class-vs-named-class)
- [11. Anonymous Class vs Inner Class](#11-anonymous-class-vs-inner-class)
- [12. Anonymous Class vs Lambda Expression](#12-anonymous-class-vs-lambda-expression)
- [13. Anonymous Class with Multiple Methods](#13-anonymous-class-with-multiple-methods)
- [14. Overriding Methods](#14-overriding-methods)
- [15. Adding New Methods](#15-adding-new-methods)
- [16. Accessing Outer Class Members](#16-accessing-outer-class-members)
- [17. Accessing Local Variables](#17-accessing-local-variables)
- [18. Effectively Final Variables](#18-effectively-final-variables)
- [19. `this` Inside Anonymous Class](#19-this-inside-anonymous-class)
- [20. Anonymous Class Constructor](#20-anonymous-class-constructor)
- [21. Instance Initializer](#21-instance-initializer)
- [22. Anonymous Class with Threads](#22-anonymous-class-with-threads)
- [23. Anonymous Class with Runnable](#23-anonymous-class-with-runnable)
- [24. Anonymous Class with Comparator](#24-anonymous-class-with-comparator)
- [25. Anonymous Class with Event Handling](#25-anonymous-class-with-event-handling)
- [26. Generic Anonymous Classes](#26-generic-anonymous-classes)
- [27. Anonymous Class and Polymorphism](#27-anonymous-class-and-polymorphism)
- [28. Anonymous Class and Access Modifiers](#28-anonymous-class-and-access-modifiers)
- [29. Anonymous Class and Static Members](#29-anonymous-class-and-static-members)
- [30. JVM View](#30-jvm-view)
- [31. Advantages](#31-advantages)
- [32. Limitations](#32-limitations)
- [33. Common Mistakes](#33-common-mistakes)
- [34. When Should You Use Anonymous Classes?](#34-when-should-you-use-anonymous-classes)
- [35. Top 25 Interview Questions](#35-top-25-interview-questions)
- [36. 30-Second Interview Answer](#36-30-second-interview-answer)
- [37. Cheat Sheet](#37-cheat-sheet)
- [38. Final Mental Model](#38-final-mental-model)

---

# 1. What Is an Anonymous Class?

An anonymous class is a class that:

```text
Has no explicit source-level name
+
Is declared
+
Is instantiated
at the same place
```

Example:

```java
interface Greeting {

    void sayHello();
}
```

Anonymous implementation:

```java
Greeting greeting =
    new Greeting() {

        @Override
        public void sayHello() {

            System.out.println(
                "Hello Java"
            );
        }
    };
```

There is no class name such as:

```text
MyGreeting
```

Instead, the class is created directly after:

```text
new Greeting()
```

---

# 2. Why Anonymous Classes?

Anonymous classes are useful when you need a small implementation for a limited or one-time purpose.

Common use cases:

```text
One-time interface implementation
One-time abstract class implementation
Callbacks
Event handling
Threads
Comparators
GUI listeners
Legacy APIs
```

Instead of creating a separate named class:

```java
class MyRunnable
    implements Runnable {

    @Override
    public void run() {

        System.out.println(
            "Running"
        );
    }
}
```

you can write:

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

# 3. Basic Syntax

General syntax:

```java
ParentType object =
    new ParentType() {

        // class body

    };
```

`ParentType` can be:

```text
Interface
Abstract class
Concrete class
```

For an interface:

```java
Runnable task =
    new Runnable() {

        @Override
        public void run() {

            System.out.println(
                "Task running"
            );
        }
    };
```

---

# 4. Anonymous Class Implementing an Interface

Example:

```java
interface Printable {

    void print();
}
```

Create anonymous implementation:

```java
Printable obj =
    new Printable() {

        @Override
        public void print() {

            System.out.println(
                "Printing..."
            );
        }
    };
```

Call:

```java
obj.print();
```

Output:

```text
Printing...
```

The anonymous class implements:

```text
Printable
```

without creating a named class.

---

# 5. Anonymous Class Extending a Class

An anonymous class can extend a normal class.

Example:

```java
class Animal {

    void sound() {

        System.out.println(
            "Animal sound"
        );
    }
}
```

Anonymous subclass:

```java
Animal animal =
    new Animal() {

        @Override
        void sound() {

            System.out.println(
                "Dog barks"
            );
        }
    };
```

Call:

```java
animal.sound();
```

Output:

```text
Dog barks
```

The anonymous class is effectively a subclass of `Animal`.

---

# 6. Anonymous Class with Abstract Class

Anonymous classes are frequently used to provide an implementation of an abstract class.

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

Call:

```java
animal.sound();
```

Output:

```text
Bark
```

The abstract method must be implemented unless the anonymous class itself remains abstract, which is not possible for an instantiated anonymous class.

---

# 7. Anonymous Class with Concrete Class

An anonymous class does not have to implement an interface or extend an abstract class.

It can extend a concrete class too.

Example:

```java
class Vehicle {

    void start() {

        System.out.println(
            "Vehicle starts"
        );
    }
}
```

Anonymous subclass:

```java
Vehicle vehicle =
    new Vehicle() {

        @Override
        void start() {

            System.out.println(
                "Car starts"
            );
        }
    };
```

Call:

```java
vehicle.start();
```

Output:

```text
Car starts
```

---

# 8. Calling Methods

An anonymous class can override methods and those methods can be called through the reference type.

Example:

```java
interface Greeting {

    void greet();
}
```

```java
Greeting greeting =
    new Greeting() {

        @Override
        public void greet() {

            System.out.println(
                "Hello"
            );
        }
    };
```

Call:

```java
greeting.greet();
```

The reference type is:

```text
Greeting
```

and the actual object is an instance of the anonymous implementation.

---

# 9. Anonymous Class Object

An anonymous class still creates a real object.

Example:

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

The object has:

```text
Object identity
State
Methods
Runtime type
```

The only unusual thing is that its class has no explicit source-level name.

---

# 10. Anonymous Class vs Named Class

Named class:

```java
class Dog
    implements Runnable {

    @Override
    public void run() {

        System.out.println(
            "Running"
        );
    }
}
```

Anonymous class:

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

Comparison:

| Feature | Named Class | Anonymous Class |
|---|---|---|
| Explicit name | Yes | No |
| Reusable by name | Yes | No |
| Declaration | Separate | Inline |
| Best for | Reusable behavior | One-time behavior |
| Readability | Usually better for large logic | Better for small logic |

---

# 11. Anonymous Class vs Inner Class

An anonymous class is a form of nested class, but it does not have an explicit source-level name.

Named member inner class:

```java
class Outer {

    class Inner {

        void display() {
        }
    }
}
```

Anonymous class:

```java
Runnable task =
    new Runnable() {

        @Override
        public void run() {
        }
    };
```

The key difference:

```text
Inner class
→ explicit class declaration/name

Anonymous class
→ no explicit class name
```

---

# 12. Anonymous Class vs Lambda Expression

This is a very important Java 8+ comparison.

Anonymous class:

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

Lambda:

```java
Runnable task =
    () -> System.out.println(
        "Running"
    );
```

Lambda expressions are generally more concise.

However, anonymous classes can do things that lambdas cannot, such as extending a class or implementing an interface while declaring additional instance members.

Lambda expressions require a functional interface.

Anonymous classes do not have the same functional-interface restriction.

---

# 13. Anonymous Class with Multiple Methods

An anonymous class can implement multiple methods of an interface.

Example:

```java
interface Calculator {

    void add();

    void subtract();
}
```

Implementation:

```java
Calculator calculator =
    new Calculator() {

        @Override
        public void add() {

            System.out.println(
                "Addition"
            );
        }

        @Override
        public void subtract() {

            System.out.println(
                "Subtraction"
            );
        }
    };
```

Call:

```java
calculator.add();
```

```java
calculator.subtract();
```

---

# 14. Overriding Methods

The most common purpose of an anonymous class is overriding behavior.

Example:

```java
class Animal {

    void sound() {

        System.out.println(
            "Animal sound"
        );
    }
}
```

Override:

```java
Animal animal =
    new Animal() {

        @Override
        void sound() {

            System.out.println(
                "Meow"
            );
        }
    };
```

The `@Override` annotation is recommended because it lets the compiler verify that the method actually overrides a parent method.

---

# 15. Adding New Methods

An anonymous class can declare additional methods that are not present in the reference type.

Example:

```java
class Animal {

    void sound() {
    }
}
```

Anonymous class:

```java
Animal animal =
    new Animal() {

        @Override
        void sound() {

            System.out.println(
                "Sound"
            );
        }

        void eat() {

            System.out.println(
                "Eating"
            );
        }
    };
```

The important issue is the reference type.

This works:

```java
animal.sound();
```

But this does not:

```java
animal.eat();
```

because:

```text
animal
→ declared type Animal
```

and `Animal` does not declare `eat()`.

---

# 16. Accessing Outer Class Members

An anonymous class declared inside an instance context can access members of the enclosing class.

Example:

```java
class Outer {

    int value = 100;

    void display() {

        Runnable task =
            new Runnable() {

                @Override
                public void run() {

                    System.out.println(
                        value
                    );
                }
            };

        task.run();
    }
}
```

The anonymous class can access:

```text
Outer.this.value
```

through the enclosing-instance relationship.

---

# 17. Accessing Local Variables

An anonymous class can access local variables from its enclosing method if they are final or effectively final.

Example:

```java
class Test {

    void display() {

        int number = 100;

        Runnable task =
            new Runnable() {

                @Override
                public void run() {

                    System.out.println(
                        number
                    );
                }
            };

        task.run();
    }
}
```

This works because:

```text
number
```

is effectively final.

---

# 18. Effectively Final Variables

Consider:

```java
int number = 100;
```

If you never reassign it:

```text
effectively final
```

An anonymous class can capture it.

But this is not allowed:

```java
int number = 100;

number = 200;
```

because the variable has been reassigned.

Therefore this causes a compilation error:

```java
Runnable task =
    new Runnable() {

        @Override
        public void run() {

            System.out.println(
                number
            );
        }
    };
```

The same capture rule applies to local classes and lambda expressions.

---

# 19. `this` Inside Anonymous Class

Inside an anonymous class:

```text
this
```

refers to the anonymous-class object.

Example:

```java
class Outer {

    void display() {

        Runnable task =
            new Runnable() {

                @Override
                public void run() {

                    System.out.println(
                        this
                    );
                }
            };

        task.run();
    }
}
```

Here:

```text
this
→ anonymous class object
```

To refer to the enclosing object:

```text
Outer.this
```

can be used when the anonymous class is declared inside an instance context of `Outer`.

---

# 20. Anonymous Class Constructor

Anonymous classes do not have an explicit class name, so you cannot declare a normal constructor with a class name.

For example, this is not possible:

```java
new Animal() {

    Animal() {
    }
};
```

as a named constructor declaration for the anonymous class.

Instead, initialization can be performed using:

```text
Instance initializer
Field initialization
Outer constructor invocation
```

For constructor arguments of the parent class, arguments are supplied after the parent type.

Example:

```java
class Animal {

    Animal(String name) {

        System.out.println(
            name
        );
    }
}
```

Anonymous subclass:

```java
Animal animal =
    new Animal("Dog") {

        @Override
        void sound() {

            System.out.println(
                "Bark"
            );
        }
    };
```

The `"Dog"` argument invokes the superclass constructor.

---

# 21. Instance Initializer

An anonymous class can use an instance initializer.

Example:

```java
class Animal {

    void sound() {
    }
}
```

```java
Animal animal =
    new Animal() {

        {
            System.out.println(
                "Anonymous object initialized"
            );
        }

        @Override
        void sound() {

            System.out.println(
                "Sound"
            );
        }
    };
```

The initializer executes when the anonymous object is created.

This can be useful, although excessive initializer logic can reduce readability.

---

# 22. Anonymous Class with Threads

Before lambda expressions, anonymous classes were commonly used with threads.

Example:

```java
Thread thread =
    new Thread() {

        @Override
        public void run() {

            System.out.println(
                "Thread running"
            );
        }
    };
```

Start:

```java
thread.start();
```

This creates an anonymous subclass of `Thread`.

---

# 23. Anonymous Class with Runnable

A more common pattern is implementing `Runnable`.

Example:

```java
Runnable task =
    new Runnable() {

        @Override
        public void run() {

            System.out.println(
                "Task running"
            );
        }
    };
```

Pass it to a thread:

```java
Thread thread =
    new Thread(task);
```

Start:

```java
thread.start();
```

Modern Java can simplify this using a lambda:

```java
Runnable task =
    () -> System.out.println(
        "Task running"
    );
```

---

# 24. Anonymous Class with Comparator

Anonymous classes were historically common for custom sorting.

Example:

```java
import java.util.Comparator;

Comparator<Integer> comparator =
    new Comparator<Integer>() {

        @Override
        public int compare(
            Integer a,
            Integer b
        ) {

            return b - a;
        }
    };
```

Modern Java usually uses a lambda:

```java
Comparator<Integer> comparator =
    (a, b) -> b - a;
```

The anonymous-class version is still valid and useful to understand, especially for legacy code and interviews.

---

# 25. Anonymous Class with Event Handling

Anonymous classes are commonly associated with callback-style APIs.

Conceptually:

```java
interface ButtonListener {

    void onClick();
}
```

Register an anonymous implementation:

```java
ButtonListener listener =
    new ButtonListener() {

        @Override
        public void onClick() {

            System.out.println(
                "Button clicked"
            );
        }
    };
```

The listener provides behavior without requiring a separate named class.

---

# 26. Generic Anonymous Classes

Anonymous classes can be created from generic types.

Example:

```java
interface Box<T> {

    T get();
}
```

Implementation:

```java
Box<String> box =
    new Box<String>() {

        @Override
        public String get() {

            return "Java";
        }
    };
```

Call:

```java
System.out.println(
    box.get()
);
```

Output:

```text
Java
```

---

# 27. Anonymous Class and Polymorphism

Anonymous classes participate in normal runtime polymorphism.

Example:

```java
class Animal {

    void sound() {

        System.out.println(
            "Animal"
        );
    }
}
```

Anonymous subclass:

```java
Animal animal =
    new Animal() {

        @Override
        void sound() {

            System.out.println(
                "Dog"
            );
        }
    };
```

The reference type is:

```text
Animal
```

but the actual runtime object is the anonymous subclass.

Therefore:

```java
animal.sound();
```

calls the overridden implementation.

---

# 28. Anonymous Class and Access Modifiers

Members declared inside an anonymous class can use access modifiers according to the normal rules applicable to those members.

Example:

```java
Runnable task =
    new Runnable() {

        private int count = 10;

        @Override
        public void run() {

            System.out.println(
                count
            );
        }
    };
```

The `count` field belongs to the anonymous object.

However, it is not directly accessible through:

```text
Runnable
```

because `Runnable` does not declare it.

---

# 29. Anonymous Class and Static Members

Anonymous classes have restrictions on static declarations similar to the rules for inner classes.

Modern Java permits static members in inner classes under the Java 16 language changes.

However, static declarations in anonymous classes are more restricted than ordinary named classes, and anonymous classes should generally be kept simple.

For normal application design:

```text
Small behavior
→ anonymous class

Complex state/design
→ named class
```

is usually easier to maintain.

---

# 30. JVM View

An anonymous class is still compiled into a class file.

For example:

```java
Runnable task =
    new Runnable() {

        @Override
        public void run() {
        }
    };
```

The compiler may generate a class file with a name similar to:

```text
Test$1.class
```

The exact generated name is a compiler detail.

The important idea is:

```text
Anonymous in source
→ still a real class at runtime
→ still creates an object
```

---

# 31. Advantages

## 1. Concise One-Time Implementation

No separate named class is needed.

---

## 2. Good for Callbacks

Useful for callback-style APIs.

---

## 3. Useful with Legacy APIs

Many older Java APIs were designed around interfaces and anonymous implementations.

---

## 4. Supports Inheritance

Can extend a concrete or abstract class.

---

## 5. Supports Interfaces

Can implement interfaces directly.

---

# 32. Limitations

## 1. No Explicit Name

You cannot reuse the class by its source-level name.

---

## 2. Can Become Hard to Read

Large anonymous classes create deeply nested code.

---

## 3. Poor Reusability

If the same behavior is needed in many places, a named class may be better.

---

## 4. Lambda Often Replaces It

For functional interfaces, lambdas are usually more concise.

---

## 5. Difficult to Test Independently

A complex anonymous implementation is harder to isolate than a named class.

---

# 33. Common Mistakes

## ❌ Mistake 1 — Thinking Anonymous Class Means No Object

Wrong.

An anonymous class still creates a normal object.

---

## ❌ Mistake 2 — Thinking Anonymous Class Can Only Implement Interfaces

Wrong.

It can:

```text
Implement an interface
Extend an abstract class
Extend a concrete class
```

---

## ❌ Mistake 3 — Trying to Give It a Constructor Name

Anonymous classes have no explicit source-level class name, so you cannot declare a normal constructor using a class name.

---

## ❌ Mistake 4 — Forgetting `@Override`

It is not mandatory, but it is strongly recommended when overriding a method.

---

## ❌ Mistake 5 — Calling Anonymous-Specific Methods Through Parent Reference

Example:

```java
Animal animal =
    new Animal() {

        void eat() {
        }
    };
```

This is not accessible through:

```java
animal.eat();
```

because the declared type is `Animal`.

---

## ❌ Mistake 6 — Capturing a Reassigned Local Variable

This is invalid:

```java
int x = 10;

x = 20;
```

when the anonymous class tries to capture `x`.

---

## ❌ Mistake 7 — Using Anonymous Classes for Huge Logic

If the implementation becomes large, consider a named class.

---

# 34. When Should You Use Anonymous Classes?

Anonymous classes are useful when:

```text
The implementation is small.

The implementation is used once.

A legacy API expects an interface/class implementation.

You need to extend a class anonymously.

A lambda cannot express the required behavior.
```

Prefer a named class when:

```text
The behavior is reused.

The implementation is large.

The class has substantial state.

The class has its own meaningful identity.

Testing and maintainability matter.
```

---

# 35. Top 25 Interview Questions

## 🔥 Q1. What is an anonymous class?

A class without an explicit source-level name that is declared and instantiated at the same time.

---

## 🔥 Q2. Why use anonymous classes?

For small, one-time implementations of interfaces or classes.

---

## 🔥 Q3. Can an anonymous class implement an interface?

Yes.

---

## 🔥 Q4. Can an anonymous class extend an abstract class?

Yes.

---

## 🔥 Q5. Can an anonymous class extend a concrete class?

Yes.

---

## 🔥 Q6. Does an anonymous class create an object?

Yes.

---

## 🔥 Q7. Does an anonymous class have a name?

It has no explicit source-level class name.

---

## 🔥 Q8. Can an anonymous class have methods?

Yes.

---

## 🔥 Q9. Can an anonymous class override methods?

Yes.

---

## 🔥 Q10. Can an anonymous class add new methods?

Yes, but those methods cannot be accessed through a reference whose declared type does not expose them.

---

## 🔥 Q11. Can anonymous classes access outer class members?

Yes, when declared in an appropriate enclosing context.

---

## 🔥 Q12. Can anonymous classes access local variables?

Yes, if the captured local variables are final or effectively final.

---

## 🔥 Q13. What does `this` mean inside an anonymous class?

It refers to the anonymous-class object.

---

## 🔥 Q14. How do you refer to the enclosing outer object?

Use:

```java
Outer.this
```

when applicable.

---

## 🔥 Q15. Can an anonymous class have a constructor?

It has no explicitly declared constructor with a class name. Superclass constructor arguments can be supplied during creation.

---

## 🔥 Q16. Can anonymous classes use instance initializers?

Yes.

---

## 🔥 Q17. What is the difference between anonymous class and lambda?

A lambda works only with a functional interface, while an anonymous class can extend classes and implement interfaces and can contain additional members.

---

## 🔥 Q18. Which is shorter: anonymous class or lambda?

Usually a lambda, when the target is a functional interface.

---

## 🔥 Q19. Can an anonymous class extend a class and implement another interface?

An anonymous class creation expression specifies either a class to extend or an interface to implement; it cannot use an `extends` and `implements` clause like a named class declaration.

---

## 🔥 Q20. Can an anonymous class be abstract?

An anonymous class is instantiated immediately, so it cannot remain abstract.

---

## 🔥 Q21. What class file is generated for an anonymous class?

Often something similar to:

```text
Outer$1.class
```

The exact name is compiler-dependent.

---

## 🔥 Q22. Can an anonymous class be reused?

Its behavior can be reused through the object reference, but the anonymous class itself has no source-level name for creating additional instances.

---

## 🔥 Q23. When should you avoid anonymous classes?

When the implementation is large, reused frequently, or needs an independent identity.

---

## 🔥 Q24. Why are anonymous classes common in old Java code?

Before lambdas in Java 8, they were a standard way to provide inline implementations of callback and functional interfaces.

---

## 🔥 Q25. What is the key difference between anonymous and named class?

```text
Named class
→ explicit name
→ reusable by name

Anonymous class
→ no explicit source-level name
→ usually one-time implementation
```

---

# 36. 30-Second Interview Answer

> An anonymous class is a class without an explicit source-level name that is declared and instantiated at the same time. It is commonly used for one-time implementations of interfaces, abstract classes, or concrete classes. Anonymous classes support method overriding, can access enclosing members, and can capture final or effectively final local variables. They were widely used for callbacks and functional interfaces before Java 8 introduced lambdas. Unlike a lambda, an anonymous class can extend a class and can declare additional instance members.

---

# 37. Cheat Sheet

```text
========================================================
                 JAVA ANONYMOUS CLASSES
========================================================


CORE IDEA
--------------------------------------------------------

Anonymous Class
→ class without explicit source-level name

Declared + instantiated
→ together


========================================================

BASIC SYNTAX
--------------------------------------------------------

ParentType obj =
    new ParentType() {

        // class body

    };


========================================================

CAN WORK WITH
--------------------------------------------------------

Interface
Abstract Class
Concrete Class


========================================================

INTERFACE
--------------------------------------------------------

Runnable task =
    new Runnable() {

        @Override
        public void run() {
        }
    };


========================================================

ABSTRACT CLASS
--------------------------------------------------------

Animal animal =
    new Animal() {

        @Override
        void sound() {
        }
    };


========================================================

CONCRETE CLASS
--------------------------------------------------------

Animal animal =
    new Animal() {

        @Override
        void sound() {
        }
    };


========================================================

OBJECT
--------------------------------------------------------

Anonymous class
→ still creates real object


========================================================

POLYMORPHISM
--------------------------------------------------------

Parent reference
        ↓
Anonymous subclass object


========================================================

METHODS
--------------------------------------------------------

Can:

→ override methods
→ declare new methods
→ declare fields
→ use initializers


But:

Parent reference
→ cannot directly access
  anonymous-only members


========================================================

OUTER ACCESS
--------------------------------------------------------

Anonymous class
→ can access enclosing members


this
→ anonymous object


Outer.this
→ enclosing outer object


========================================================

LOCAL VARIABLES
--------------------------------------------------------

Can capture:

final variables

OR

effectively final variables


========================================================

LAMBDA COMPARISON
--------------------------------------------------------

Anonymous class
→ can extend class
→ can implement interface
→ can have additional members


Lambda
→ functional interface only
→ much shorter syntax


========================================================

THREAD
--------------------------------------------------------

Thread thread =
    new Thread() {

        @Override
        public void run() {
        }
    };


========================================================

RUNNABLE
--------------------------------------------------------

Runnable task =
    new Runnable() {

        @Override
        public void run() {
        }
    };


========================================================

COMPARATOR
--------------------------------------------------------

Comparator<Integer> cmp =
    new Comparator<Integer>() {

        @Override
        public int compare(
            Integer a,
            Integer b
        ) {

            return b - a;
        }
    };


========================================================

JVM
--------------------------------------------------------

Anonymous source
→ real runtime class


Common generated form:

Outer$1.class


Exact name
→ compiler detail


========================================================

USE WHEN
--------------------------------------------------------

Small
One-time
Callback
Legacy API
Class extension
Functional interface without lambda


========================================================

AVOID WHEN
--------------------------------------------------------

Large implementation
Repeated use
Complex state
Independent identity
Need easy testing


========================================================

MEMORY TRICK
--------------------------------------------------------

Anonymous
→ NO explicit class name

new Interface()
→ inline implementation

new Class()
→ anonymous subclass


========================================================
```

---

# 38. Final Mental Model

```text
                         ANONYMOUS CLASS
                                |
                                ↓
                    No explicit source name
                                |
                                ↓
                     Declared + instantiated
                                |
              +-----------------+-----------------+
              |                 |                 |
              ↓                 ↓                 ↓
          Interface       Abstract Class     Concrete Class
              |                 |                 |
              +-----------------+-----------------+
                                |
                                ↓
                           Real Object
                                |
                                ↓
                         Normal Polymorphism


========================================================

EXAMPLE


Runnable task =
    new Runnable() {

        @Override
        public void run() {

            System.out.println(
                "Running"
            );
        }
    };


Breakdown:


Runnable
   ↓
Reference type


new Runnable()
   ↓
Anonymous implementation


{ ... }
   ↓
Anonymous class body


task
   ↓
Reference to object


========================================================

ANONYMOUS CLASS vs LAMBDA


Anonymous Class
       |
       +── Interface
       |
       +── Abstract class
       |
       +── Concrete class
       |
       +── Extra members
       |
       +── Multiple methods


Lambda
       |
       +── Functional interface
       |
       +── Concise
       |
       +── Java 8+


========================================================

THIS


Inside anonymous class:


this
 ↓
Anonymous object


Outer.this
 ↓
Enclosing outer object


========================================================

VARIABLE CAPTURE


Local variable
      |
      ↓
final / effectively final
      |
      ↓
Anonymous class
      |
      ↓
Can access


========================================================

REFERENCE TYPE MATTERS


Animal animal =
    new Animal() {

        void eat() {
        }
    };


animal.sound()
→ accessible if Animal declares sound()


animal.eat()
→ NOT accessible through Animal reference


Why?


Declared type
     ↓
Animal


Anonymous-specific method
     ↓
Not part of Animal


========================================================

JVM MODEL


Source:


new Runnable() {
    ...
}


Conceptually:


Runnable reference
       |
       ↓
Anonymous class object
       |
       ↓
Generated class representation


Often:


Outer$1.class


========================================================

BEST MEMORY LINE


"Anonymous class = unnamed class
created inline for a specific implementation."


========================================================

SECOND BEST MEMORY LINE


"Before lambdas, anonymous classes were
the standard way to provide inline behavior
for callback-style interfaces."


========================================================

FINAL MAP


Anonymous Class
      |
      +── No explicit name
      |
      +── Real object
      |
      +── Interface implementation
      |
      +── Abstract class implementation
      |
      +── Concrete class extension
      |
      +── Method overriding
      |
      +── Outer member access
      |
      +── Effectively-final local capture
      |
      +── Often replaced by lambda
         for functional interfaces


========================================================
```

---

# 🏁 Final Revision Checklist

- [ ] Definition of anonymous class
- [ ] Why anonymous classes are used
- [ ] Basic syntax
- [ ] Interface implementation
- [ ] Abstract class implementation
- [ ] Concrete class extension
- [ ] Anonymous object
- [ ] Method overriding
- [ ] Adding new methods
- [ ] Reference-type limitation
- [ ] Anonymous vs named class
- [ ] Anonymous vs inner class
- [ ] Anonymous vs lambda
- [ ] Outer class member access
- [ ] `this`
- [ ] `Outer.this`
- [ ] Local variable capture
- [ ] Effectively final rule
- [ ] Anonymous class constructor rules
- [ ] Superclass constructor arguments
- [ ] Instance initializer
- [ ] Thread example
- [ ] Runnable example
- [ ] Comparator example
- [ ] Event/callback use
- [ ] Generic anonymous class
- [ ] Runtime polymorphism
- [ ] JVM class-file representation
- [ ] Advantages
- [ ] Limitations
- [ ] Common mistakes
- [ ] When to use anonymous classes
- [ ] When to prefer named classes
- [ ] When lambda is preferable

---

# 🧠 One-Line Memory

```text
Anonymous Class
= No explicit name
+ Created inline
+ Real object
+ One-time implementation

Can:
→ implement interface
→ extend abstract class
→ extend concrete class

Can access:
→ outer members
→ final/effectively-final locals

this
→ anonymous object

Outer.this
→ outer object

Lambda
→ shorter alternative for functional interfaces
```

