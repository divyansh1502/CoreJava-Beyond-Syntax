
# 🏷️ Annotations in Java

> **Annotations are metadata attached to Java program elements such as classes, methods, fields, parameters, and constructors that can be used by the compiler, tools, or runtime frameworks to provide additional information or control behavior.**

---

# 📑 Table of Contents

- [1. What Are Annotations?](#1-what-are-annotations)
- [2. Why Do We Need Annotations?](#2-why-do-we-need-annotations)
- [3. Basic Annotation Syntax](#3-basic-annotation-syntax)
- [4. Built-in Annotations](#4-built-in-annotations)
- [5. `@Override`](#5-override)
- [6. `@Deprecated`](#6-deprecated)
- [7. `@SuppressWarnings`](#7-suppresswarnings)
- [8. `@FunctionalInterface`](#8-functionalinterface)
- [9. `@SafeVarargs`](#9-safevarargs)
- [10. `@Native`](#10-native)
- [11. Meta-Annotations](#11-meta-annotations)
- [12. `@Target`](#12-target)
- [13. `@Retention`](#13-retention)
- [14. `@Documented`](#14-documented)
- [15. `@Inherited`](#15-inherited)
- [16. `@Repeatable`](#16-repeatable)
- [17. Retention Policies](#17-retention-policies)
- [18. SOURCE Retention](#18-source-retention)
- [19. CLASS Retention](#19-class-retention)
- [20. RUNTIME Retention](#20-runtime-retention)
- [21. Creating Custom Annotations](#21-creating-custom-annotations)
- [22. Annotation Elements](#22-annotation-elements)
- [23. Default Values](#23-default-values)
- [24. Using Custom Annotations](#24-using-custom-annotations)
- [25. Reading Annotations with Reflection](#25-reading-annotations-with-reflection)
- [26. Annotation Inheritance](#26-annotation-inheritance)
- [27. Repeatable Annotations](#27-repeatable-annotations)
- [28. Marker Annotations](#28-marker-annotations)
- [29. Annotations with Multiple Elements](#29-annotations-with-multiple-elements)
- [30. Annotation Processing](#30-annotation-processing)
- [31. Compile-Time vs Runtime Annotations](#31-compile-time-vs-runtime-annotations)
- [32. Annotations and Reflection](#32-annotations-and-reflection)
- [33. Annotations in Frameworks](#33-annotations-in-frameworks)
- [34. Advantages](#34-advantages)
- [35. Limitations](#35-limitations)
- [36. Common Mistakes](#36-common-mistakes)
- [37. Top 25 Interview Questions](#37-top-25-interview-questions)
- [38. 30-Second Interview Answer](#38-30-second-interview-answer)
- [39. Cheat Sheet](#39-cheat-sheet)
- [40. Final Mental Model](#40-final-mental-model)

---

# 1. What Are Annotations?

Annotations are metadata.

They provide additional information about Java code without directly becoming part of the normal business logic.

Example:

```java
@Override
public void run() {
}
```

Here:

```text
@Override
```

is an annotation.

Annotations can be applied to:

```text
Classes
Interfaces
Methods
Fields
Constructors
Parameters
Local variables
Packages
Modules
Type uses
```

depending on the annotation's allowed targets.

---

# 2. Why Do We Need Annotations?

Without annotations, frameworks and tools would often require external configuration or naming conventions.

Annotations allow information to be attached directly to code.

For example:

```java
@Override
public void run() {
}
```

The compiler can use the annotation to verify that the method actually overrides a superclass or interface method.

Framework example:

```java
@Service
class UserService {
}
```

A framework can inspect this metadata and use it to identify the class for a particular purpose.

Annotations are commonly used for:

```text
Compiler checks
Configuration
Framework behavior
Code generation
Documentation
Testing
Dependency injection
ORM mapping
Serialization
Build-time processing
```

---

# 3. Basic Annotation Syntax

An annotation starts with:

```text
@
```

Example:

```java
@Override
```

General form:

```text
@AnnotationName
```

Annotation with an element:

```java
@SuppressWarnings("unchecked")
```

Annotation with multiple elements:

```java
@Entity(
    name = "users"
)
```

The exact syntax depends on the annotation definition.

---

# 4. Built-in Annotations

Java provides many standard annotations.

Important ones include:

```text
@Override
@Deprecated
@SuppressWarnings
@FunctionalInterface
@SafeVarargs
@Native
```

There are also meta-annotations used to define annotation behavior:

```text
@Target
@Retention
@Documented
@Inherited
@Repeatable
```

---

# 5. `@Override`

`@Override` indicates that a method is intended to override a superclass or interface method.

Example:

```java
class Animal {

    void sound() {
    }
}
```

```java
class Dog extends Animal {

    @Override
    void sound() {

        System.out.println(
            "Bark"
        );
    }
}
```

The compiler verifies that the method actually overrides an inherited method.

If the method signature is incorrect:

```java
class Dog extends Animal {

    @Override
    void sounds() {
    }
}
```

the compiler reports an error.

Important:

```text
@Override
→ compile-time checking
```

It does not cause overriding itself.

---

# 6. `@Deprecated`

`@Deprecated` indicates that an API should generally no longer be used for new code.

Example:

```java
class Calculator {

    @Deprecated
    int oldAdd(
        int a,
        int b
    ) {

        return a + b;
    }
}
```

Using it may generate a compiler warning.

Java also supports a Javadoc-oriented form:

```java
@Deprecated(
    since = "2.0",
    forRemoval = true
)
```

Important elements:

```text
since
→ version in which deprecation was introduced

forRemoval
→ indicates that removal is intended
```

`@Deprecated` communicates API lifecycle information.

---

# 7. `@SuppressWarnings`

`@SuppressWarnings` tells the compiler that specific warnings should be suppressed in the annotated scope.

Example:

```java
@SuppressWarnings("unchecked")
static void example() {

    java.util.List list =
        new java.util.ArrayList();

    list.add("Java");
}
```

The annotation does not fix the underlying problem.

It only suppresses the specified compiler warning.

Common warning categories include:

```text
unchecked
deprecation
rawtypes
unused
```

The exact warnings recognized can depend on the compiler.

Best practice:

```text
Suppress only the warning
you understand and intend to suppress.
```

---

# 8. `@FunctionalInterface`

`@FunctionalInterface` indicates that an interface is intended to be a functional interface.

Example:

```java
@FunctionalInterface
interface Calculator {

    int calculate(
        int a,
        int b
    );
}
```

A functional interface must have exactly one abstract method.

This is invalid:

```java
@FunctionalInterface
interface Calculator {

    int add(
        int a,
        int b
    );

    int subtract(
        int a,
        int b
    );
}
```

The compiler reports an error.

Important:

Default and static methods do not count as abstract methods.

---

# 9. `@SafeVarargs`

`@SafeVarargs` can be used to indicate that a variable-arity method or constructor does not perform potentially unsafe operations on its varargs parameter.

It is primarily relevant when generic varargs are involved.

Example:

```java
class Utility {

    @SafeVarargs
    static <T> void print(
        T... values
    ) {

        for (T value : values) {

            System.out.println(
                value
            );
        }
    }
}
```

Important:

`@SafeVarargs` should not be used merely to hide warnings.

The programmer must ensure that the operation is actually safe.

---

# 10. `@Native`

`@Native` indicates that a field is referenced from native code.

Example:

```java
import java.lang.annotation.Native;

class NativeConstants {

    @Native
    static final int VALUE = 10;
}
```

It is primarily useful for tooling and native-code integration.

It does not itself make a method native.

---

# 11. Meta-Annotations

Meta-annotations are annotations used to describe other annotations.

Important meta-annotations include:

```text
@Target
@Retention
@Documented
@Inherited
@Repeatable
```

Think of them as:

```text
Annotation
    ↓
How should this annotation behave?
    ↓
Meta-annotations
```

---

# 12. `@Target`

`@Target` specifies where an annotation can be used.

Example:

```java
import java.lang.annotation.ElementType;
import java.lang.annotation.Target;

@Target(ElementType.METHOD)
@interface Loggable {
}
```

Now:

```java
class Service {

    @Loggable
    void execute() {
    }
}
```

The annotation is intended for methods.

Common `ElementType` values include:

```text
TYPE
FIELD
METHOD
PARAMETER
CONSTRUCTOR
LOCAL_VARIABLE
ANNOTATION_TYPE
PACKAGE
TYPE_PARAMETER
TYPE_USE
MODULE
RECORD_COMPONENT
```

---

# 13. `@Retention`

`@Retention` specifies how long annotation information should be retained.

Example:

```java
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;

@Retention(
    RetentionPolicy.RUNTIME
)
@interface Loggable {
}
```

The three policies are:

```text
SOURCE
CLASS
RUNTIME
```

---

# 14. `@Documented`

`@Documented` indicates that an annotation should appear in generated API documentation when used.

Example:

```java
import java.lang.annotation.Documented;

@Documented
@interface PublicApi {
}
```

This primarily affects documentation tools such as Javadoc.

---

# 15. `@Inherited`

`@Inherited` specifies that a class-level annotation can be inherited by subclasses when the annotation is queried through the class-level annotation API.

Example:

```java
import java.lang.annotation.Inherited;

@Inherited
@interface Important {
}
```

```java
@Important
class Parent {
}
```

```java
class Child
    extends Parent {
}
```

When querying `Child` using the relevant inherited-annotation lookup, the annotation can be found.

Important:

```text
@Inherited applies to class annotations.
```

It does not mean annotations are automatically inherited on:

```text
methods
fields
constructors
```

---

# 16. `@Repeatable`

`@Repeatable` allows the same annotation to be applied multiple times to the same declaration.

Example:

```java
@Repeatable(Roles.class)
@interface Role {

    String value();
}
```

Container annotation:

```java
@interface Roles {

    Role[] value();
}
```

Now:

```java
@Role("ADMIN")
@Role("USER")
class Account {
}
```

The same annotation type can appear multiple times.

---

# 17. Retention Policies

Java defines three annotation retention policies:

```text
SOURCE
CLASS
RUNTIME
```

Think:

```text
SOURCE
→ compiler only

CLASS
→ class file

RUNTIME
→ class file + runtime reflection
```

---

# 18. SOURCE Retention

`SOURCE` means the annotation is available in source code but is discarded by the compiler and does not need to be present in the generated class file.

Example:

```java
@Retention(
    RetentionPolicy.SOURCE
)
@interface GenerateCode {
}
```

Typical use:

```text
Compiler tools
Source analysis
Code-generation tools
```

It is not available through normal runtime reflection.

---

# 19. CLASS Retention

`CLASS` means the annotation is recorded in the `.class` file but is not necessarily available through runtime reflection.

Example:

```java
@Retention(
    RetentionPolicy.CLASS
)
@interface ClassInfo {
}
```

This is the default retention policy when `@Retention` is not specified.

Important:

```text
No @Retention
→ CLASS retention by default
```

---

# 20. RUNTIME Retention

`RUNTIME` means the annotation remains available at runtime and can generally be inspected through reflection.

Example:

```java
@Retention(
    RetentionPolicy.RUNTIME
)
@interface Entity {
}
```

Then:

```java
@Entity
class Student {
}
```

Reflection:

```java
boolean present =
    Student.class
        .isAnnotationPresent(
            Entity.class
        );
```

This is especially important for frameworks that discover configuration through runtime reflection.

---

# 21. Creating Custom Annotations

Custom annotations are declared using:

```text
@interface
```

Example:

```java
@interface Author {
}
```

This creates an annotation type.

Use it:

```java
@Author
class Book {
}
```

A custom annotation can also contain elements.

Example:

```java
@interface Author {

    String name();
}
```

Use:

```java
@Author(
    name = "Rahul"
)
class Book {
}
```

---

# 22. Annotation Elements

Annotation elements are declared similarly to methods.

Example:

```java
@interface User {

    String name();

    int age();
}
```

Usage:

```java
@User(
    name = "Rahul",
    age = 22
)
class Student {
}
```

Important:

Annotation elements can only use supported annotation element types.

Common valid types include:

```text
Primitive types
String
Class
Enum
Annotation
Arrays of the above
```

---

# 23. Default Values

Annotation elements can have default values.

Example:

```java
@interface User {

    String name();

    int age() default 18;
}
```

Now:

```java
@User(
    name = "Rahul"
)
class Student {
}
```

The value of:

```text
age
```

will be:

```text
18
```

If no default is specified, the element must be provided.

---

# 24. Using Custom Annotations

Example:

```java
@Retention(
    RetentionPolicy.RUNTIME
)
@Target(
    ElementType.TYPE
)
@interface Service {
}
```

Use it:

```java
@Service
class UserService {
}
```

The annotation itself does not automatically create a service object.

Some external mechanism must interpret it.

For example:

```text
Framework
    ↓
Find @Service
    ↓
Read metadata
    ↓
Apply framework behavior
```

This is a very important concept.

---

# 25. Reading Annotations with Reflection

Suppose:

```java
@Retention(
    RetentionPolicy.RUNTIME
)
@interface Author {

    String name();
}
```

And:

```java
@Author(
    name = "Rahul"
)
class Book {
}
```

Reflection:

```java
Author author =
    Book.class.getAnnotation(
        Author.class
    );
```

Then:

```java
System.out.println(
    author.name()
);
```

Output:

```text
Rahul
```

Common reflection methods:

```text
isAnnotationPresent()
getAnnotation()
getAnnotations()
getDeclaredAnnotation()
getDeclaredAnnotations()
```

---

# 26. Annotation Inheritance

Consider:

```java
@Inherited
@Retention(
    RetentionPolicy.RUNTIME
)
@interface Role {
}
```

Parent:

```java
@Role
class Parent {
}
```

Child:

```java
class Child
    extends Parent {
}
```

A class-level inherited annotation can be observed on `Child` through inherited lookup.

However:

```text
@Inherited
does not make method annotations inherited.
```

Example:

```java
class Parent {

    @SomeAnnotation
    void run() {
    }
}
```

A subclass does not automatically acquire that annotation on its overriding method simply because `@Inherited` exists.

---

# 27. Repeatable Annotations

Define:

```java
import java.lang.annotation.Repeatable;

@Repeatable(Roles.class)
@interface Role {

    String value();
}
```

Container:

```java
@interface Roles {

    Role[] value();
}
```

Apply:

```java
@Role("ADMIN")
@Role("MANAGER")
class User {
}
```

Reflection can retrieve repeated annotations.

Example:

```java
Role[] roles =
    User.class.getAnnotationsByType(
        Role.class
    );
```

---

# 28. Marker Annotations

A marker annotation has no elements.

Example:

```java
@interface Important {
}
```

Usage:

```java
@Important
class PaymentService {
}
```

The annotation acts as a flag.

Conceptually:

```text
@Important
→ this class has a special property
```

Another familiar example is:

```java
@Override
```

although its semantics are compiler-defined rather than being a simple framework marker.

---

# 29. Annotations with Multiple Elements

Example:

```java
@Retention(
    RetentionPolicy.RUNTIME
)
@Target(
    ElementType.TYPE
)
@interface Employee {

    String name();

    int id();

    String department()
        default "IT";
}
```

Usage:

```java
@Employee(
    name = "Rahul",
    id = 101,
    department = "Backend"
)
class Developer {
}
```

The values can be read through reflection.

---

# 30. Annotation Processing

Annotation processing is a mechanism that can inspect annotations during compilation.

A common annotation processor workflow is:

```text
Source Code
    ↓
Compiler
    ↓
Annotation Processor
    ↓
Inspect annotations
    ↓
Generate code / files / diagnostics
```

This is different from runtime reflection.

Annotation processors operate during compilation.

They can be used for:

```text
Code generation
Validation
Compile-time diagnostics
Metadata generation
```

A key distinction:

```text
Annotation Processing
→ compile time

Runtime Reflection
→ runtime
```

---

# 31. Compile-Time vs Runtime Annotations

Not every annotation is intended for runtime use.

Compare:

```text
@Retention(SOURCE)
```

with:

```text
@Retention(RUNTIME)
```

SOURCE:

```text
Source
 ↓
Compiler
 ↓
Annotation unavailable at runtime
```

RUNTIME:

```text
Source
 ↓
Compiler
 ↓
.class
 ↓
Runtime
 ↓
Reflection can inspect it
```

Therefore, if a framework needs to discover an annotation using reflection, that annotation generally needs:

```text
RetentionPolicy.RUNTIME
```

---

# 32. Annotations and Reflection

These concepts work together frequently.

Annotation:

```java
@Service
class UserService {
}
```

Reflection:

```java
Class<?> clazz =
    UserService.class;

boolean service =
    clazz.isAnnotationPresent(
        Service.class
    );
```

Conceptual relationship:

```text
Annotation
→ metadata

Reflection
→ mechanism for reading runtime metadata
```

An annotation does not automatically cause reflection.

Reflection can be used to inspect runtime-retained annotations.

---

# 33. Annotations in Frameworks

Annotations are heavily used in Java frameworks.

For example, a framework may define:

```java
@Service
class UserService {
}
```

Then inspect classes:

```text
Classpath
    ↓
Find class
    ↓
Read annotations
    ↓
@Service found
    ↓
Register class
```

Other common framework concepts include annotations for:

```text
Dependency injection
Database mapping
HTTP endpoints
Transactions
Validation
Configuration
Testing
Serialization
```

The important principle is:

```text
Annotation provides metadata.

Framework defines what that metadata means.
```

---

# 34. Advantages

## 1. Less Configuration

Metadata can stay close to the code it describes.

---

## 2. Better Framework Integration

Frameworks can discover behavior through annotations.

---

## 3. Compile-Time Validation

Some annotations allow compilers or processors to detect problems early.

---

## 4. Readability

Annotations can make intended behavior explicit.

Example:

```java
@Override
```

immediately communicates the programmer's intention.

---

## 5. Extensibility

Developers can create custom annotations for framework and application-specific requirements.

---

# 35. Limitations

## 1. Annotations Do Not Automatically Execute Logic

This:

```java
@Service
```

does not itself create a service object.

Something must process the annotation.

---

## 2. Excessive Use Can Hide Behavior

A large amount of framework behavior may happen indirectly through annotations.

---

## 3. Runtime Reflection Can Add Overhead

Runtime annotation processing may involve reflection and metadata lookup.

---

## 4. Annotation Values Are Restricted

Annotation elements cannot be arbitrary Java objects.

---

## 5. Retention Matters

An annotation that is discarded before runtime cannot be discovered through normal runtime reflection.

---

# 36. Common Mistakes

## ❌ Mistake 1 — Thinking `@Override` Performs Overriding

False.

The Java language's inheritance and overriding rules perform the override.

`@Override` asks the compiler to verify the programmer's intention.

---

## ❌ Mistake 2 — Thinking Every Annotation Is Available at Runtime

False.

Retention controls availability.

```text
SOURCE
CLASS
RUNTIME
```

---

## ❌ Mistake 3 — Thinking `@Inherited` Applies to Methods

False.

`@Inherited` concerns inheritance of class annotations.

---

## ❌ Mistake 4 — Thinking `@Retention(RUNTIME)` Automatically Executes Code

False.

It only keeps the annotation available for runtime inspection.

---

## ❌ Mistake 5 — Forgetting the Default Retention

If `@Retention` is omitted:

```text
CLASS
```

is the default retention policy.

---

## ❌ Mistake 6 — Confusing Annotation Processing with Reflection

They occur at different stages.

```text
Annotation Processing
→ compile time

Reflection
→ runtime
```

---

## ❌ Mistake 7 — Thinking Custom Annotations Have Built-In Behavior

A custom annotation such as:

```java
@Important
```

does nothing by itself unless some compiler tool, annotation processor, framework, or application code interprets it.

---

## ❌ Mistake 8 — Using Unsupported Annotation Element Types

You cannot use arbitrary objects as annotation elements.

For example, an element cannot simply be:

```java
Object value();
```

Annotation elements have restricted types.

---

# 37. Top 25 Interview Questions

## 🔥 Q1. What is an annotation?

An annotation is metadata attached to program elements that can be interpreted by the compiler, tools, annotation processors, or runtime frameworks.

---

## 🔥 Q2. Why are annotations used?

They are used for:

```text
Compiler checks
Configuration
Framework behavior
Code generation
Validation
Documentation
Testing
```

---

## 🔥 Q3. What symbol is used to declare an annotation?

```text
@
```

Usage:

```java
@Override
```

Declaration:

```java
@interface MyAnnotation {
}
```

---

## 🔥 Q4. What are meta-annotations?

Meta-annotations are annotations used to define the behavior or characteristics of other annotations.

Important examples:

```text
@Target
@Retention
@Documented
@Inherited
@Repeatable
```

---

## 🔥 Q5. What does `@Target` do?

It restricts the program elements where an annotation can be applied.

---

## 🔥 Q6. What does `@Retention` do?

It determines how long annotation information is retained.

```text
SOURCE
CLASS
RUNTIME
```

---

## 🔥 Q7. What is the default retention policy?

```text
CLASS
```

---

## 🔥 Q8. Which retention policy is needed for runtime reflection?

Usually:

```text
RUNTIME
```

---

## 🔥 Q9. What is `@Inherited`?

It allows a class-level annotation to be inherited by subclasses when using the relevant inherited annotation lookup.

It does not apply to method or field annotations.

---

## 🔥 Q10. What is `@Repeatable`?

It allows the same annotation type to be applied multiple times to one declaration.

---

## 🔥 Q11. What is a marker annotation?

An annotation with no elements.

Example:

```java
@interface Important {
}
```

---

## 🔥 Q12. What does `@Override` do?

It tells the compiler that a method is intended to override an inherited method and enables compile-time verification of that intent.

---

## 🔥 Q13. What does `@Deprecated` do?

It marks an API as deprecated and communicates that it should generally not be used for new code.

---

## 🔥 Q14. What does `@SuppressWarnings` do?

It suppresses specified compiler warnings in the annotated scope.

---

## 🔥 Q15. What does `@FunctionalInterface` do?

It indicates that an interface is intended to have exactly one abstract method and allows the compiler to verify that requirement.

---

## 🔥 Q16. How do you create a custom annotation?

Using:

```java
@interface MyAnnotation {
}
```

---

## 🔥 Q17. Can annotations have values?

Yes.

Example:

```java
@interface User {

    String name();
}
```

Usage:

```java
@User(
    name = "Rahul"
)
class Student {
}
```

---

## 🔥 Q18. Can annotation elements have default values?

Yes.

```java
@interface User {

    String role()
        default "USER";
}
```

---

## 🔥 Q19. What types can annotation elements have?

Common allowed types include:

```text
Primitive types
String
Class
Enum
Annotation
Arrays of these types
```

---

## 🔥 Q20. How can runtime annotations be read?

Using reflection.

Example:

```java
clazz.getAnnotation(
    MyAnnotation.class
);
```

---

## 🔥 Q21. What is the difference between annotation processing and reflection?

```text
Annotation Processing
→ compile time

Reflection
→ runtime
```

---

## 🔥 Q22. Does every annotation use reflection?

No.

Some annotations are used only by the compiler or compile-time tools.

---

## 🔥 Q23. Does `@Retention(RUNTIME)` execute the annotation?

No.

It only makes the annotation information available at runtime.

---

## 🔥 Q24. Can a custom annotation contain arbitrary Java objects?

No.

Annotation element types are restricted.

---

## 🔥 Q25. How do frameworks use annotations?

A framework can inspect annotations and use their metadata to decide what classes, methods, fields, or parameters should do.

For example:

```text
Annotation
 ↓
Reflection / processor
 ↓
Framework logic
 ↓
Application behavior
```

---

# 38. 30-Second Interview Answer

> Annotations in Java are metadata attached to program elements such as classes, methods, fields, and parameters. They can be used by the compiler, annotation processors, documentation tools, or runtime frameworks. Important built-in annotations include `@Override`, `@Deprecated`, `@SuppressWarnings`, and `@FunctionalInterface`. Custom annotations are declared using `@interface`, while meta-annotations such as `@Target` and `@Retention` define where and how long an annotation is retained. Runtime-retained annotations can be inspected using reflection, which is why annotations are heavily used in frameworks for configuration, dependency injection, ORM, testing, validation, and other infrastructure tasks.

---

# 39. Cheat Sheet

```text
========================================================
                    JAVA ANNOTATIONS
========================================================


CORE IDEA
--------------------------------------------------------

Annotation
→ metadata about Java code


========================================================

SYNTAX
--------------------------------------------------------

@AnnotationName


Custom annotation:

@interface MyAnnotation {
}


========================================================

IMPORTANT BUILT-IN ANNOTATIONS
--------------------------------------------------------

@Override
→ verify overriding


@Deprecated
→ API is deprecated


@SuppressWarnings
→ suppress compiler warnings


@FunctionalInterface
→ exactly one abstract method


@SafeVarargs
→ indicate safe varargs usage


@Native
→ native-code-related field metadata


========================================================

META-ANNOTATIONS
--------------------------------------------------------

@Target
→ where annotation can be used


@Retention
→ how long annotation survives


@Documented
→ include in generated documentation


@Inherited
→ class-level annotation inheritance


@Repeatable
→ same annotation multiple times


========================================================

RETENTION
--------------------------------------------------------

SOURCE
→ source code
→ not runtime reflection


CLASS
→ .class file
→ default


RUNTIME
→ runtime
→ reflection


========================================================

TARGET
--------------------------------------------------------

TYPE
FIELD
METHOD
PARAMETER
CONSTRUCTOR
LOCAL_VARIABLE
ANNOTATION_TYPE
PACKAGE
TYPE_PARAMETER
TYPE_USE
MODULE
RECORD_COMPONENT


========================================================

CUSTOM ANNOTATION
--------------------------------------------------------

@Retention(
    RetentionPolicy.RUNTIME
)
@Target(
    ElementType.TYPE
)
@interface Service {
}


========================================================

ANNOTATION ELEMENT
--------------------------------------------------------

@interface User {

    String name();

    int age();
}


Usage:

@User(
    name = "Rahul",
    age = 22
)


========================================================

DEFAULT VALUE
--------------------------------------------------------

@interface User {

    String role()
        default "USER";
}


========================================================

MARKER ANNOTATION
--------------------------------------------------------

@interface Important {
}


No elements.


========================================================

REPEATABLE
--------------------------------------------------------

@Repeatable(Roles.class)
@interface Role {

    String value();
}


@Role("ADMIN")
@Role("USER")
class User {
}


========================================================

READING WITH REFLECTION
--------------------------------------------------------

clazz.isAnnotationPresent(
    MyAnnotation.class
)


clazz.getAnnotation(
    MyAnnotation.class
)


clazz.getAnnotations()


clazz.getDeclaredAnnotations()


========================================================

INHERITED
--------------------------------------------------------

@Inherited
→ class annotations

NOT:

method annotations
field annotations
constructor annotations


========================================================

PROCESSING
--------------------------------------------------------

Annotation Processor
→ compile time


Reflection
→ runtime


========================================================

FRAMEWORK FLOW
--------------------------------------------------------

Annotation
    ↓
Metadata
    ↓
Framework discovers it
    ↓
Framework interprets it
    ↓
Behavior


========================================================

IMPORTANT TRUTH
--------------------------------------------------------

Annotation itself
≠
behavior


Annotation
→ information


Something else must
interpret that information.


========================================================
```

---

# 40. Final Mental Model

```text
                         ANNOTATIONS
                              |
                              ↓
                           METADATA
                              |
            +-----------------+-----------------+
            |                 |                 |
            ↓                 ↓                 ↓
        Compiler          Processor         Runtime
            |                 |                 |
            ↓                 ↓                 ↓
       @Override         Code Generation    Reflection
       @Deprecated       Validation         Frameworks
       @Suppress...      Diagnostics        Configuration
            |                 |                 |
            +-----------------+-----------------+
                              |
                              ↓
                         APPLICATION
                           BEHAVIOR


========================================================

ANNOTATION LIFECYCLE


SOURCE CODE
    |
    ↓
Annotation
    |
    +-----------------------------+
    |                             |
    ↓                             ↓
SOURCE retention              CLASS/RUNTIME
    |                             |
    ↓                             ↓
Compiler tools                 .class file
                                  |
                                  ↓
                              RUNTIME
                                  |
                                  ↓
                              Reflection


========================================================

THE FIVE IMPORTANT META-ANNOTATIONS


@Target
→ WHERE?


@Retention
→ HOW LONG?


@Documented
→ DOCUMENTATION?


@Inherited
→ CLASS INHERITANCE?


@Repeatable
→ CAN IT REPEAT?


========================================================

RETENTION MEMORY TRICK


SOURCE
→ Source only


CLASS
→ Class file


RUNTIME
→ Runtime reflection


S → C → R


========================================================

ANNOTATION vs REFLECTION


Annotation
→ describes metadata


Reflection
→ reads/interacts with runtime metadata


Think:


Annotation
    ↓
"Here is information."


Reflection
    ↓
"Let me inspect that information."


========================================================

ANNOTATION vs ANNOTATION PROCESSOR


Annotation
→ metadata


Annotation Processor
→ processes metadata at compile time


Reflection
→ processes runtime-visible metadata at runtime


========================================================

FRAMEWORK MENTAL MODEL


@Service
class UserService {
}


Framework:

Class
 ↓
Find annotation
 ↓
Read metadata
 ↓
Register class
 ↓
Create/manage object
 ↓
Application uses it


========================================================

MOST IMPORTANT INTERVIEW LINE


"Annotations provide metadata;
they do not automatically provide behavior.
The compiler, annotation processor,
tool, or framework must interpret them."


========================================================

FINAL MEMORY MAP


Built-in
   ↓
Custom
   ↓
Meta-Annotations
   ↓
Retention
   ↓
Targets
   ↓
Reflection / Processing
   ↓
Framework Behavior


========================================================
```

---

# 🏁 Final Revision Checklist

- [ ] What is an annotation?
- [ ] Why are annotations used?
- [ ] `@Override`
- [ ] `@Deprecated`
- [ ] `@SuppressWarnings`
- [ ] `@FunctionalInterface`
- [ ] `@SafeVarargs`
- [ ] `@Target`
- [ ] `@Retention`
- [ ] `@Documented`
- [ ] `@Inherited`
- [ ] `@Repeatable`
- [ ] SOURCE retention
- [ ] CLASS retention
- [ ] RUNTIME retention
- [ ] Default retention policy
- [ ] Custom annotations
- [ ] Annotation elements
- [ ] Default annotation values
- [ ] Marker annotations
- [ ] Repeatable annotations
- [ ] Annotation processing
- [ ] Runtime reflection
- [ ] Annotation vs reflection
- [ ] Annotation vs annotation processor
- [ ] How frameworks use annotations
- [ ] `@Inherited` limitations
- [ ] Supported annotation element types
- [ ] Why `RUNTIME` retention matters

---

# 🧠 One-Line Memory

```text
Annotation = Metadata
@Target = Where
@Retention = How long
@Inherited = Class inheritance
@Repeatable = Multiple times
Reflection = Read at runtime
Processor = Process at compile time
Framework = Gives the metadata meaning
```

---

