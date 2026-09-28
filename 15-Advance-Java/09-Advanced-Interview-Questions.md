# 🎯 Advanced Java — Interview Questions

> **This file covers advanced Java interview questions across Reflection, Annotations, Enums, Nested/Inner/Anonymous Classes, Modules, Design Patterns, JVM concepts, and modern Java features.**

---

# 📑 Table of Contents

- [1. Advanced Java Interview Strategy](#1-advanced-java-interview-strategy)
- [2. Reflection Questions](#2-reflection-questions)
- [3. Annotation Questions](#3-annotation-questions)
- [4. Enum Questions](#4-enum-questions)
- [5. Nested Class Questions](#5-nested-class-questions)
- [6. Inner Class Questions](#6-inner-class-questions)
- [7. Anonymous Class Questions](#7-anonymous-class-questions)
- [8. Module System Questions](#8-module-system-questions)
- [9. Design Pattern Questions](#9-design-pattern-questions)
- [10. JVM and Class Loading Questions](#10-jvm-and-class-loading-questions)
- [11. Advanced OOP Questions](#11-advanced-oop-questions)
- [12. Generics and Type System Questions](#12-generics-and-type-system-questions)
- [13. Exception and Error Questions](#13-exception-and-error-questions)
- [14. Multithreading Questions](#14-multithreading-questions)
- [15. Collection Framework Questions](#15-collection-framework-questions)
- [16. Java I/O Questions](#16-java-io-questions)
- [17. Modern Java Questions](#17-modern-java-questions)
- [18. Design Pattern Comparison Questions](#18-design-pattern-comparison-questions)
- [19. Scenario-Based Questions](#19-scenario-based-questions)
- [20. Rapid-Fire Questions](#20-rapid-fire-questions)
- [21. Top 30 Must-Know Questions](#21-top-30-must-know-questions)
- [22. 30-Second Interview Answer](#22-30-second-interview-answer)
- [23. Advanced Java Cheat Sheet](#23-advanced-java-cheat-sheet)
- [24. Final Revision Checklist](#24-final-revision-checklist)

---

# 1. Advanced Java Interview Strategy

Advanced Java interviews usually test more than syntax.

Interviewers often want to know:

```text
What happens internally?
Why does Java behave this way?
What are the trade-offs?
When would you use this feature?
What problem does it solve?
```

A strong answer should usually follow:

```text
Definition
    ↓
How it works
    ↓
Example
    ↓
Use case
    ↓
Important limitation/trade-off
```

For example, do not answer only:

```text
"Reflection is used to inspect classes."
```

A stronger answer is:

```text
Reflection allows Java code to inspect and interact with
classes, methods, fields, and constructors at runtime.

It is used by frameworks for tasks such as dependency injection,
ORM mapping, and testing, but excessive reflection can reduce
type safety and may introduce performance and maintainability costs.
```

---

# 2. Reflection Questions

## 🔥 Q1. What is Reflection in Java?

Reflection is the ability of a Java program to inspect and interact with classes, fields, methods, constructors, and other metadata at runtime.

Main API:

```java
Class
Method
Field
Constructor
```

---

## 🔥 Q2. What is the `Class` class?

`Class` represents metadata about a Java type at runtime.

Example:

```java
Class<String> clazz =
    String.class;
```

You can also obtain it from an object:

```java
String value = "Java";

Class<?> clazz =
    value.getClass();
```

---

## 🔥 Q3. How can you obtain a Class object?

Three common ways:

```java
Class<String> a =
    String.class;
```

```java
Class<?> b =
    Class.forName("java.lang.String");
```

```java
String text = "Java";

Class<?> c =
    text.getClass();
```

---

## 🔥 Q4. What is `Class.forName()`?

It loads a class by its fully qualified name and returns its `Class` object.

Example:

```java
Class<?> clazz =
    Class.forName(
        "java.lang.String"
    );
```

Historically, it was commonly associated with loading JDBC driver classes explicitly.

Modern JDBC drivers can often be discovered automatically through the service-provider mechanism.

---

## 🔥 Q5. How can Reflection access a private field?

Reflection can inspect declared fields, including private fields.

Example:

```java
class User {

    private String name =
        "Java";
}
```

```java
Field field =
    User.class.getDeclaredField(
        "name"
    );
```

Modern Java strongly encapsulates modules and packages, so reflective access can also be restricted.

---

## 🔥 Q6. What are disadvantages of Reflection?

Important disadvantages include:

```text
Reduced compile-time type safety
Harder debugging
More complex code
Potential performance overhead
Encapsulation can be bypassed in some circumstances
Module access restrictions
```

Reflection is powerful but should be used intentionally.

---

## 🔥 Q7. Where is Reflection commonly used?

Examples:

```text
Dependency Injection frameworks
ORM frameworks
Testing frameworks
Serialization libraries
Plugin systems
Framework configuration
```

---

# 3. Annotation Questions

## 🔥 Q8. What is an annotation?

An annotation provides metadata about program elements.

Example:

```java
@Override
```

Annotations can be applied to:

```text
Classes
Methods
Fields
Parameters
Constructors
Packages
Modules
Type uses
```

depending on the annotation's target.

---

## 🔥 Q9. What is `@Override`?

`@Override` tells the compiler that a method is intended to override a superclass or interface method.

Example:

```java
class Child
    extends Parent {

    @Override
    void show() {

        System.out.println(
            "Child"
        );
    }
}
```

It helps catch accidental signature mistakes.

---

## 🔥 Q10. What is a custom annotation?

You can define your own annotation using `@interface`.

Example:

```java
@interface Important {
}
```

Usage:

```java
@Important
class Service {
}
```

---

## 🔥 Q11. What is Retention Policy?

Retention determines how long annotation information is retained.

Three important policies:

```text
SOURCE
CLASS
RUNTIME
```

### SOURCE

Available in source code but discarded during compilation.

### CLASS

Stored in the class file but not necessarily available through runtime reflection.

### RUNTIME

Available at runtime and can be inspected through reflection.

---

## 🔥 Q12. What is `@Retention(RetentionPolicy.RUNTIME)`?

It tells Java to retain an annotation at runtime.

Example:

```java
@Retention(
    RetentionPolicy.RUNTIME
)
@interface Important {
}
```

This makes the annotation available for runtime inspection.

---

## 🔥 Q13. What is `@Target`?

`@Target` restricts where an annotation can be applied.

Example:

```java
@Target(ElementType.METHOD)
@interface Loggable {
}
```

This annotation can be used on methods.

---

## 🔥 Q14. What is a meta-annotation?

An annotation applied to another annotation.

Examples:

```text
@Retention
@Target
@Documented
@Inherited
```

---

# 4. Enum Questions

## 🔥 Q15. What is an enum?

An enum represents a fixed set of constants.

Example:

```java
enum Day {

    MONDAY,
    TUESDAY,
    WEDNESDAY
}
```

Usage:

```java
Day day =
    Day.MONDAY;
```

---

## 🔥 Q16. Can an enum have fields and methods?

Yes.

Example:

```java
enum Status {

    SUCCESS(200),
    NOT_FOUND(404);

    private int code;

    Status(int code) {

        this.code = code;
    }

    int getCode() {

        return code;
    }
}
```

---

## 🔥 Q17. Can enum have a constructor?

Yes, but enum constructors are implicitly restricted and cannot be invoked directly with `new`.

Example:

```java
enum Level {

    LOW,
    MEDIUM,
    HIGH;

    Level() {

        System.out.println(
            "Enum constructor"
        );
    }
}
```

---

## 🔥 Q18. Can we create an enum object using `new`?

No.

This is invalid:

```java
Level level =
    new Level();
```

Enum constants are created by the JVM according to enum semantics.

---

## 🔥 Q19. Can enum extend another class?

No.

All enums implicitly extend:

```text
java.lang.Enum
```

Java classes have single inheritance, so an enum cannot extend another class.

However, an enum can implement interfaces.

---

## 🔥 Q20. Why is enum often preferred for Singleton?

An enum Singleton can provide a concise implementation with strong guarantees around enum instance creation and serialization.

Example:

```java
enum Singleton {

    INSTANCE;

    void show() {

        System.out.println(
            "Singleton"
        );
    }
}
```

Usage:

```java
Singleton.INSTANCE.show();
```

---

# 5. Nested Class Questions

## 🔥 Q21. What is a nested class?

A class declared inside another class is called a nested class.

Types include:

```text
Static nested class
Inner class
Local class
Anonymous class
```

---

## 🔥 Q22. What is a static nested class?

A static nested class is declared with `static`.

Example:

```java
class Outer {

    static class Nested {

        void show() {

            System.out.println(
                "Nested class"
            );
        }
    }
}
```

It does not require an instance of `Outer` to create an instance of `Nested`.

```java
Outer.Nested obj =
    new Outer.Nested();
```

---

## 🔥 Q23. What is an inner class?

A non-static nested class is an inner class.

Example:

```java
class Outer {

    class Inner {

        void show() {

            System.out.println(
                "Inner class"
            );
        }
    }
}
```

It is associated with an instance of the enclosing class.

---

## 🔥 Q24. Static nested class vs inner class?

| Static Nested Class | Inner Class |
|---|---|
| Declared with `static` | Non-static |
| Does not require outer object | Associated with outer object |
| Cannot directly access outer instance members | Can access outer instance members |
| `Outer.Nested` | `outer.new Inner()` |

---

# 6. Inner Class Questions

## 🔥 Q25. Can an inner class access private members of the outer class?

Yes.

Example:

```java
class Outer {

    private int value =
        100;

    class Inner {

        void show() {

            System.out.println(
                value
            );
        }
    }
}
```

Nested classes have access to members of their enclosing class subject to Java's language rules.

---

## 🔥 Q26. How do you create an inner class object?

Example:

```java
class Outer {

    class Inner {
    }
}
```

Create the outer object first:

```java
Outer outer =
    new Outer();
```

Then:

```java
Outer.Inner inner =
    outer.new Inner();
```

---

## 🔥 Q27. Why use inner classes?

Possible reasons include:

```text
Logical grouping
Encapsulation
Access to outer instance state
Implementation details
Event handling
Helper functionality
```

---

# 7. Anonymous Class Questions

## 🔥 Q28. What is an anonymous class?

An anonymous class is a class without an explicit class name that is declared and instantiated at the same time.

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

---

## 🔥 Q29. Anonymous class vs lambda?

Lambda expressions are designed primarily for functional interfaces.

Example:

```java
Runnable task =
    () -> System.out.println(
        "Running"
    );
```

Anonymous classes can define additional state and methods and can be used where an interface or superclass can be anonymously subclassed.

---

## 🔥 Q30. Can an anonymous class have a constructor?

It does not have a named constructor because it has no class name.

Initialization can instead be done through:

```text
Instance initializer
Captured variables
Constructor arguments passed to superclass/interface implementation context
```

depending on the design.

---

# 8. Module System Questions

## 🔥 Q31. What is the Java Module System?

The Java Platform Module System (JPMS), introduced in Java 9, provides stronger encapsulation and explicit dependencies between modules.

---

## 🔥 Q32. What is `module-info.java`?

It is the module descriptor.

Example:

```java
module com.example.app {

    requires java.sql;

    exports com.example.api;
}
```

---

## 🔥 Q33. What does `requires` mean?

It declares a dependency on another module.

Example:

```java
module app {

    requires java.sql;
}
```

---

## 🔥 Q34. What does `exports` mean?

It makes a package accessible to other modules.

Example:

```java
module app {

    exports com.example.api;
}
```

---

## 🔥 Q35. What does `opens` mean?

`opens` allows reflective access to a package, particularly relevant to frameworks that use reflection.

Example:

```java
module app {

    opens com.example.model;
}
```

---

## 🔥 Q36. `exports` vs `opens`?

```text
exports
→ normal compile-time/public API access


opens
→ deep reflective access
```

They solve different problems.

---

# 9. Design Pattern Questions

## 🔥 Q37. What are the three categories of GoF patterns?

```text
Creational
Structural
Behavioral
```

---

## 🔥 Q38. How many classic GoF patterns are there?

23.

---

## 🔥 Q39. Singleton vs Factory?

```text
Singleton
→ controls instance uniqueness/access


Factory
→ encapsulates object creation decisions
```

They solve different problems.

---

## 🔥 Q40. Factory vs Builder?

```text
Factory
→ decides which object to create


Builder
→ constructs a complex object step by step
```

---

## 🔥 Q41. Adapter vs Decorator?

```text
Adapter
→ changes/adapts interface


Decorator
→ adds behavior while preserving the interface
```

---

## 🔥 Q42. Strategy vs State?

```text
Strategy
→ interchangeable behavior/algorithm


State
→ behavior changes according to internal state
```

---

## 🔥 Q43. What is Dependency Injection?

Dependency Injection means supplying an object's dependencies from outside rather than having the object construct them itself.

Example:

```java
class UserService {

    private UserRepository repository;

    UserService(
        UserRepository repository
    ) {

        this.repository = repository;
    }
}
```

---

## 🔥 Q44. Composition vs inheritance?

Inheritance represents:

```text
IS-A
```

Composition represents:

```text
HAS-A
```

Composition often provides more flexibility when behavior needs to vary independently.

---

# 10. JVM and Class Loading Questions

## 🔥 Q45. What is a ClassLoader?

A ClassLoader loads class definitions into the JVM.

Common built-in loaders include:

```text
Bootstrap Class Loader
Platform Class Loader
System/Application Class Loader
```

---

## 🔥 Q46. What is the ClassLoader delegation model?

A class loader generally delegates class loading requests to its parent before attempting to load the class itself.

Concept:

```text
Application
    ↓
Platform
    ↓
Bootstrap
```

This helps maintain consistency and prevents application classes from accidentally replacing core Java classes.

---

## 🔥 Q47. What happens when a class is loaded?

Conceptually, JVM class loading involves:

```text
Loading
   ↓
Linking
   ↓
Initialization
```

Linking includes:

```text
Verification
Preparation
Resolution
```

Resolution can occur lazily.

---

## 🔥 Q48. What is class initialization?

Initialization involves executing class initialization logic, including static field initialization and static initializer blocks, when required.

Example:

```java
class Test {

    static int value = 100;

    static {

        System.out.println(
            "Class initialized"
        );
    }
}
```

---

## 🔥 Q49. When can static initialization happen?

Typical triggers include active use such as:

```text
Creating an instance
Accessing certain static fields
Invoking certain static methods
Reflection that causes initialization
```

The exact initialization rules are specified by the Java Language Specification and JVM specification.

---

# 11. Advanced OOP Questions

## 🔥 Q50. Why doesn't Java support multiple inheritance of classes?

A major reason is ambiguity and complexity associated with multiple class inheritance, including the diamond problem.

Java instead supports:

```text
Single class inheritance
+
Multiple interface inheritance
```

---

## 🔥 Q51. Can an interface contain fields?

Yes.

Interface fields are implicitly:

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

## 🔥 Q52. Can an interface have methods with implementation?

Yes.

Modern interfaces can contain:

```text
default methods
static methods
private methods
```

along with abstract methods.

---

## 🔥 Q53. What is the diamond problem with interfaces?

If multiple interfaces provide conflicting default methods, the implementing class must resolve the conflict.

Example:

```java
interface A {

    default void show() {

        System.out.println(
            "A"
        );
    }
}
```

```java
interface B {

    default void show() {

        System.out.println(
            "B"
        );
    }
}
```

A class implementing both must resolve the conflict:

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

## 🔥 Q54. What is covariant return type?

An overriding method can return a subtype of the original method's return type.

Example:

```java
class Parent {

    Number getValue() {

        return 10;
    }
}
```

```java
class Child
    extends Parent {

    @Override
    Integer getValue() {

        return 10;
    }
}
```

---

# 12. Generics and Type System Questions

## 🔥 Q55. What is type erasure?

Java generics are primarily implemented through type erasure.

Generic type information is largely removed from runtime representations, while the compiler inserts necessary casts and performs type checking.

Example:

```java
List<String> names =
    new ArrayList<>();
```

At runtime, the collection does not retain `String` as its ordinary generic type parameter in the same way a reified generic system would.

---

## 🔥 Q56. Why can't we create generic arrays directly?

This is not allowed:

```java
List<String>[] lists =
    new List<String>[10];
```

Generic arrays are restricted because arrays are reified while generic type parameters are erased, which could lead to type-safety problems.

---

## 🔥 Q57. What is PECS?

PECS means:

```text
Producer Extends
Consumer Super
```

Producer:

```java
List<? extends Number>
```

Consumer:

```java
List<? super Integer>
```

---

## 🔥 Q58. `? extends` vs `? super`?

```text
? extends T
→ mainly read/produce T values


? super T
→ mainly write/consume T values
```

---

# 13. Exception and Error Questions

## 🔥 Q59. Exception vs Error?

Both extend `Throwable`, but they represent different categories.

```text
Exception
→ conditions applications may often handle


Error
→ serious JVM/system-level problems
```

Examples:

```text
Exception
→ IOException
→ SQLException


Error
→ OutOfMemoryError
→ StackOverflowError
```

Errors generally should not be treated as ordinary recoverable application conditions.

---

## 🔥 Q60. Why should we not catch `Throwable` generally?

Because `Throwable` includes both exceptions and serious errors.

Catching everything can hide failures that should propagate or terminate the affected operation.

---

## 🔥 Q61. What is try-with-resources?

It automatically closes resources implementing `AutoCloseable`.

Example:

```java
try (
    BufferedReader reader =
        new BufferedReader(
            new FileReader("data.txt")
        )
) {

    System.out.println(
        reader.readLine()
    );

} catch (IOException e) {

    e.printStackTrace();
}
```

---

## 🔥 Q62. What are suppressed exceptions?

If an exception occurs while closing a resource during try-with-resources, it can be attached to the primary exception as a suppressed exception.

They can be accessed using:

```java
Throwable[] suppressed =
    exception.getSuppressed();
```

---

# 14. Multithreading Questions

## 🔥 Q63. Process vs Thread?

```text
Process
→ independent running program/resource environment


Thread
→ execution path within a process
```

Threads in the same process generally share process resources.

---

## 🔥 Q64. What is a race condition?

A race condition occurs when the result depends on the timing or interleaving of concurrent operations on shared state.

Example:

```text
Thread A → read value
Thread B → read value
Thread A → update
Thread B → update
```

The final result may not represent both operations correctly.

---

## 🔥 Q65. What is synchronization?

Synchronization coordinates access to shared mutable state.

Example:

```java
synchronized void increment() {

    count++;
}
```

It can provide mutual exclusion and establishes relevant memory-visibility guarantees.

---

## 🔥 Q66. `volatile` vs `synchronized`?

```text
volatile
→ visibility and ordering guarantees for a variable


synchronized
→ mutual exclusion + memory synchronization
```

`volatile` does not make compound operations such as:

```text
count++
```

atomic.

---

## 🔥 Q67. What is deadlock?

Deadlock occurs when threads wait indefinitely for resources held by each other.

Typical structure:

```text
Thread A
holds Lock 1
waits for Lock 2

Thread B
holds Lock 2
waits for Lock 1
```

---

## 🔥 Q68. What is ExecutorService?

`ExecutorService` provides an abstraction for submitting tasks to managed worker threads.

Example:

```java
ExecutorService service =
    Executors.newFixedThreadPool(
        2
    );
```

Submit a task:

```java
service.submit(
    () -> System.out.println(
        "Task running"
    )
);
```

Shutdown:

```java
service.shutdown();
```

---

# 15. Collection Framework Questions

## 🔥 Q69. ArrayList vs LinkedList?

```text
ArrayList
→ dynamic array


LinkedList
→ linked-node structure
```

ArrayList usually provides better random access.

LinkedList can provide efficient insertion/removal at a known node position, but finding that position may itself require traversal.

---

## 🔥 Q70. HashMap vs Hashtable?

```text
HashMap
→ modern general-purpose map
→ allows one null key and null values
→ not synchronized by default


Hashtable
→ legacy synchronized map
→ does not allow null keys/values
```

---

## 🔥 Q71. HashMap vs ConcurrentHashMap?

```text
HashMap
→ not thread-safe


ConcurrentHashMap
→ designed for concurrent access
```

ConcurrentHashMap provides thread-safe operations with concurrency-oriented implementation strategies.

---

## 🔥 Q72. How does HashMap work conceptually?

The map uses:

```text
hashCode()
    ↓
hash processing
    ↓
bucket/index
    ↓
key comparison
```

For keys that map to the same bucket, the implementation handles collisions using internal structures.

In modern Java, heavily collided buckets can use tree-based structures under certain conditions.

---

## 🔥 Q73. Why should `equals()` and `hashCode()` be consistent?

If two objects are equal according to `equals()`, they must return the same hash code.

Otherwise hash-based collections can fail to locate logically equal keys correctly.

---

# 16. Java I/O Questions

## 🔥 Q74. Byte stream vs character stream?

```text
Byte streams
→ binary/raw byte data


Character streams
→ character-oriented text data
```

Examples:

```text
InputStream
OutputStream

Reader
Writer
```

---

## 🔥 Q75. Why use Buffered streams?

Buffering reduces the number of direct I/O operations by reading/writing chunks of data.

Examples:

```java
BufferedReader
BufferedWriter
BufferedInputStream
BufferedOutputStream
```

---

## 🔥 Q76. What is serialization?

Serialization converts an object's state into a byte stream representation.

A common Java mechanism uses:

```java
Serializable
```

---

## 🔥 Q77. What is `serialVersionUID`?

It identifies the serialized form of a Serializable class for compatibility checks during deserialization.

Example:

```java
private static final long
    serialVersionUID = 1L;
```

---

# 17. Modern Java Questions

## 🔥 Q78. What is a record?

A record is a concise way to declare a class intended primarily to model immutable data.

Example:

```java
record User(
    String name,
    int age
) {
}
```

The compiler provides members such as:

```text
Accessor methods
equals()
hashCode()
toString()
canonical constructor
```

---

## 🔥 Q79. Are records completely immutable?

Records are shallowly immutable by design.

The record components cannot be reassigned after construction, but if a component refers to a mutable object, that referenced object can still change.

---

## 🔥 Q80. What is a sealed class?

A sealed class restricts which classes can directly extend it.

Example:

```java
sealed class Shape
    permits Circle, Square {
}
```

Permitted subclasses:

```java
final class Circle
    extends Shape {
}
```

```java
final class Square
    extends Shape {
}
```

---

## 🔥 Q81. What is pattern matching?

Pattern matching allows type checks and extraction to be expressed more concisely.

Example:

```java
if (obj instanceof String text) {

    System.out.println(
        text.length()
    );
}
```

---

## 🔥 Q82. What is Optional?

`Optional<T>` represents a value that may or may not be present.

Example:

```java
Optional<String> name =
    Optional.of("Java");
```

It is primarily intended to make absence explicit in APIs, especially return values.

---

# 18. Design Pattern Comparison Questions

## 🔥 Q83. Factory vs Abstract Factory vs Builder

| Pattern | Main Purpose |
|---|---|
| Factory | Encapsulate object creation |
| Abstract Factory | Create related product families |
| Builder | Construct complex objects step by step |

---

## 🔥 Q84. Adapter vs Facade

| Adapter | Facade |
|---|---|
| Converts interface | Simplifies subsystem |
| Usually solves compatibility | Usually solves complexity |
| Client expects one interface | Client gets simplified interface |

---

## 🔥 Q85. Proxy vs Decorator

| Proxy | Decorator |
|---|---|
| Controls access | Adds behavior |
| Can delay/cross-check access | Wraps functionality |
| Often preserves same interface | Usually preserves same interface |

---

## 🔥 Q86. Strategy vs Template Method

| Strategy | Template Method |
|---|---|
| Composition-oriented | Inheritance-oriented |
| Entire behavior can be replaced | Selected steps are overridden |
| Runtime behavior can be changed | Algorithm skeleton controlled by superclass |

---

# 19. Scenario-Based Questions

## 🔥 Q87. You have multiple payment methods. Which pattern can you use?

A possible design is:

```text
Strategy Pattern
```

Each payment method implements a common strategy interface.

```text
PaymentStrategy
├── UPI
├── Card
└── Cash
```

---

## 🔥 Q88. Your application has many optional constructor parameters. Which pattern?

A common choice is:

```text
Builder Pattern
```

---

## 🔥 Q89. An old third-party API has an incompatible interface. Which pattern?

A common choice is:

```text
Adapter Pattern
```

---

## 🔥 Q90. You need to add behavior to objects dynamically. Which pattern?

A common choice is:

```text
Decorator Pattern
```

---

## 🔥 Q91. You need a simple API over many complicated classes. Which pattern?

A common choice is:

```text
Facade Pattern
```

---

## 🔥 Q92. Multiple components need notification when an event occurs. Which pattern?

A common design is:

```text
Observer Pattern
```

---

## 🔥 Q93. You need to control access to a real object. Which pattern?

A common choice is:

```text
Proxy Pattern
```

---

## 🔥 Q94. You need to represent undo operations. Which pattern may help?

A possible pattern is:

```text
Memento
```

Another possible design is:

```text
Command
```

depending on whether the requirement focuses on storing state or encapsulating reversible actions.

---

# 20. Rapid-Fire Questions

## Q95. Can an enum implement an interface?

Yes.

---

## Q96. Can an enum extend a class?

No, because every enum implicitly extends `java.lang.Enum`.

---

## Q97. Can a static nested class access outer instance fields directly?

No.

It does not have an enclosing outer instance.

---

## Q98. Can an inner class access outer private members?

Yes.

---

## Q99. Can an anonymous class implement an interface?

Yes.

---

## Q100. Can an anonymous class extend a class?

Yes.

---

## Q101. Can an interface have a constructor?

No.

Interfaces do not represent instantiable classes and therefore do not have constructors.

---

## Q102. Can an abstract class have a constructor?

Yes.

Its constructor can initialize state when a subclass instance is created.

---

## Q103. Can an abstract class be final?

No.

`abstract` requires subclassing for instantiation, while `final` prevents subclassing.

---

## Q104. Can a final method be overridden?

No.

---

## Q105. Can a static method be overridden?

Static methods are hidden, not overridden.

---

## Q106. Can private methods be overridden?

No.

Private methods are not inherited in the normal overriding sense.

---

## Q107. Can a constructor be final?

No.

Constructors are not inherited or overridden.

---

## Q108. Can a class be both abstract and final?

No.

They represent contradictory class-level inheritance constraints.

---

## Q109. Can an interface extend multiple interfaces?

Yes.

```java
interface C
    extends A, B {
}
```

---

## Q110. Can a class implement multiple interfaces?

Yes.

```java
class C
    implements A, B {
}
```

---

## Q111. Is String immutable?

Yes.

---

## Q112. Why is String immutable?

Immutability supports:

```text
String pool sharing
Thread safety
Hash-code stability
Security
Predictable behavior
```

---

## Q113. StringBuilder vs StringBuffer?

```text
StringBuilder
→ generally preferred for single-threaded mutation


StringBuffer
→ synchronized legacy mutable string class
```

---

## Q114. Is Java pass-by-reference?

No.

Java is pass-by-value.

For object variables, the value passed is the reference value.

---

## Q115. What is the difference between shallow copy and deep copy?

```text
Shallow copy
→ copies references


Deep copy
→ recursively copies relevant mutable objects
```

---

# 21. Top 30 Must-Know Questions

For fresher and backend interviews, prioritize understanding these:

```text
1. What is Reflection?
2. What is Class.forName()?
3. What are annotations?
4. SOURCE vs CLASS vs RUNTIME retention?
5. What is an enum?
6. Why can't enum be instantiated with new?
7. Static nested class vs inner class?
8. What is an anonymous class?
9. Anonymous class vs lambda?
10. What is JPMS?
11. requires vs exports vs opens?
12. What are GoF design patterns?
13. Singleton vs Factory?
14. Factory vs Builder?
15. Adapter vs Decorator?
16. Strategy vs State?
17. What is Dependency Injection?
18. What is ClassLoader?
19. What is class loading?
20. What is type erasure?
21. Why can't generic arrays be created directly?
22. What is PECS?
23. Exception vs Error?
24. What is try-with-resources?
25. volatile vs synchronized?
26. What is deadlock?
27. HashMap internal working?
28. equals() and hashCode() contract?
29. What is a record?
30. What is a sealed class?
```

---

# 22. 30-Second Interview Answer

> Advanced Java covers runtime features, language mechanisms, APIs, concurrency, I/O, the module system, and design techniques beyond basic syntax. Important areas include Reflection and annotations, enums and nested classes, JPMS, class loading, generics and type erasure, concurrency, collections, I/O, records, sealed classes, pattern matching, and design patterns. In interviews, I focus not only on definitions but also on internal behavior, use cases, limitations, and comparisons between similar concepts.

---

# 23. Advanced Java Cheat Sheet

```text
========================================================
                 ADVANCED JAVA CHEAT SHEET
========================================================


REFLECTION
--------------------------------------------------------

Class
Method
Field
Constructor

Purpose:
Runtime inspection/manipulation


========================================================

ANNOTATIONS
--------------------------------------------------------

Metadata


Retention:
SOURCE
CLASS
RUNTIME


Target:
Controls where annotation can be applied.


========================================================

ENUM
--------------------------------------------------------

Fixed set of constants

Can have:
Fields
Methods
Constructors
Interfaces


Cannot:
Extend another class
Use new to create enum constants


========================================================

NESTED CLASSES
--------------------------------------------------------

Static Nested
→ no outer instance required


Inner
→ associated with outer instance


Local
→ declared inside block/method


Anonymous
→ unnamed class instantiated inline


========================================================

MODULES
--------------------------------------------------------

module-info.java


requires
→ dependency


exports
→ public package access to other modules


opens
→ reflective access


========================================================

DESIGN PATTERNS
--------------------------------------------------------

Creational
→ Singleton
→ Factory
→ Abstract Factory
→ Builder
→ Prototype


Structural
→ Adapter
→ Bridge
→ Composite
→ Decorator
→ Facade
→ Proxy
→ Flyweight


Behavioral
→ Strategy
→ Observer
→ Command
→ State
→ Iterator
→ Template Method
→ Chain of Responsibility
→ Mediator
→ Memento
→ Visitor
→ Interpreter


========================================================

JVM
--------------------------------------------------------

ClassLoader
↓
Loading
↓
Linking
↓
Initialization


ClassLoaders:
Bootstrap
Platform
Application


========================================================

GENERICS
--------------------------------------------------------

Type Erasure


PECS:

Producer
→ extends


Consumer
→ super


========================================================

EXCEPTIONS
--------------------------------------------------------

Throwable
├── Error
└── Exception


Try-with-resources
→ AutoCloseable


Suppressed exceptions
→ secondary exceptions from resource closing


========================================================

CONCURRENCY
--------------------------------------------------------

volatile
→ visibility/order guarantees


synchronized
→ mutual exclusion + memory synchronization


Deadlock
→ cyclic waiting


ExecutorService
→ task execution management


========================================================

COLLECTIONS
--------------------------------------------------------

ArrayList
→ dynamic array


LinkedList
→ linked nodes


HashMap
→ hash-based map


ConcurrentHashMap
→ concurrent map


equals()
+
hashCode()
→ must obey their contract


========================================================

I/O
--------------------------------------------------------

Byte:
InputStream
OutputStream


Character:
Reader
Writer


Buffered:
BufferedReader
BufferedWriter


Serialization:
Object → byte stream


Deserialization:
byte stream → object


========================================================

MODERN JAVA
--------------------------------------------------------

Record
→ concise data carrier


Sealed class
→ controlled inheritance


Pattern matching
→ type check + binding


Optional
→ explicit representation of possible absence


========================================================

OOP
--------------------------------------------------------

Inheritance
→ IS-A


Composition
→ HAS-A


Interface
→ contract/abstraction


Abstract class
→ shared abstraction/state/behavior


========================================================

IMPORTANT DIFFERENCES
--------------------------------------------------------

Factory
→ chooses/creates object


Builder
→ constructs complex object


Adapter
→ converts interface


Decorator
→ adds behavior


Facade
→ simplifies subsystem


Proxy
→ controls access


Strategy
→ interchangeable algorithm


State
→ behavior depends on state


Observer
→ notifications


Command
→ request as object


========================================================
```

---

# 24. Final Revision Checklist

## Reflection

- [ ] Reflection definition
- [ ] `Class`
- [ ] `Class.forName()`
- [ ] Field
- [ ] Method
- [ ] Constructor
- [ ] Reflection use cases
- [ ] Reflection limitations

## Annotations

- [ ] Annotation definition
- [ ] Built-in annotations
- [ ] Custom annotations
- [ ] Retention policies
- [ ] `@Target`
- [ ] Meta-annotations

## Enums

- [ ] Enum definition
- [ ] Enum fields
- [ ] Enum methods
- [ ] Enum constructors
- [ ] Enum + interface
- [ ] Enum Singleton

## Nested Classes

- [ ] Static nested class
- [ ] Inner class
- [ ] Local class
- [ ] Anonymous class
- [ ] Static nested vs inner
- [ ] Anonymous class vs lambda

## Modules

- [ ] JPMS
- [ ] `module-info.java`
- [ ] `requires`
- [ ] `exports`
- [ ] `opens`
- [ ] Module encapsulation

## Design Patterns

- [ ] GoF
- [ ] 23 patterns
- [ ] Creational
- [ ] Structural
- [ ] Behavioral
- [ ] Singleton
- [ ] Factory
- [ ] Abstract Factory
- [ ] Builder
- [ ] Prototype
- [ ] Adapter
- [ ] Decorator
- [ ] Facade
- [ ] Proxy
- [ ] Observer
- [ ] Strategy
- [ ] Command
- [ ] State
- [ ] Iterator

## JVM

- [ ] ClassLoader
- [ ] Delegation model
- [ ] Loading
- [ ] Linking
- [ ] Initialization

## Generics

- [ ] Type erasure
- [ ] PECS
- [ ] `extends`
- [ ] `super`
- [ ] Generic arrays

## Concurrency

- [ ] Race condition
- [ ] Synchronization
- [ ] `volatile`
- [ ] `synchronized`
- [ ] Deadlock
- [ ] ExecutorService

## Collections

- [ ] ArrayList
- [ ] LinkedList
- [ ] HashMap
- [ ] ConcurrentHashMap
- [ ] equals/hashCode

## I/O

- [ ] Byte streams
- [ ] Character streams
- [ ] Buffered streams
- [ ] Serialization
- [ ] Deserialization
- [ ] Suppressed exceptions

## Modern Java

- [ ] Records
- [ ] Sealed classes
- [ ] Pattern matching
- [ ] Optional

---

# 🧠 Final Mental Model

```text
ADVANCED JAVA
     |
     +── Runtime
     |     ├── Reflection
     |     ├── ClassLoader
     |     └── Modules
     |
     +── Language Features
     |     ├── Annotations
     |     ├── Enums
     |     ├── Nested Classes
     |     ├── Records
     |     └── Sealed Classes
     |
     +── Object Design
     |     ├── SOLID
     |     ├── Dependency Injection
     |     └── Design Patterns
     |
     +── Type System
     |     ├── Generics
     |     ├── Type Erasure
     |     └── PECS
     |
     +── Concurrency
     |     ├── Threads
     |     ├── Synchronization
     |     ├── volatile
     |     └── ExecutorService
     |
     +── Core APIs
           ├── Collections
           ├── I/O
           ├── Exceptions
           └── Optional


========================================================

INTERVIEW THINKING


"What is it?"
      ↓
"Why does it exist?"
      ↓
"How does it work?"
      ↓
"When would I use it?"
      ↓
"What are its limitations?"
      ↓
"What is it commonly confused with?"


========================================================

MASTER THESE COMPARISONS


Reflection
vs
Compile-time access


Static Nested
vs
Inner


Anonymous Class
vs
Lambda


exports
vs
opens


Factory
vs
Builder


Adapter
vs
Decorator


Strategy
vs
State


volatile
vs
synchronized


HashMap
vs
ConcurrentHashMap


Exception
vs
Error


Record
vs
Normal Class


Composition
vs
Inheritance


========================================================

FINAL MEMORY


Advanced Java is not about memorizing
more syntax.

It is about understanding:

Runtime behavior
+
Object design
+
JVM mechanisms
+
Concurrency
+
Type system
+
Modern language features
+
Trade-offs
```
