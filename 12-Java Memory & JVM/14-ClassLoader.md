# 14 — ClassLoader

## 📚 Table of Contents

1. [Introduction](#1-introduction)
2. [What is a ClassLoader?](#2-what-is-a-classloader)
3. [Why Do We Need ClassLoaders?](#3-why-do-we-need-classloaders)
4. [Where ClassLoader Fits in JVM](#4-where-classloader-fits-in-jvm)
5. [What Does Class Loading Mean?](#5-what-does-class-loading-mean)
6. [ClassLoader Lifecycle](#6-classloader-lifecycle)
7. [Loading](#7-loading)
8. [Linking](#8-linking)
9. [Verification](#9-verification)
10. [Preparation](#10-preparation)
11. [Resolution](#11-resolution)
12. [Initialization](#12-initialization)
13. [Class Loading Flow](#13-class-loading-flow)
14. [Built-in ClassLoaders](#14-built-in-classloaders)
15. [Bootstrap ClassLoader](#15-bootstrap-classloader)
16. [Platform ClassLoader](#16-platform-classloader)
17. [Application ClassLoader](#17-application-classloader)
18. [ClassLoader Hierarchy](#18-classloader-hierarchy)
19. [Parent Delegation Model](#19-parent-delegation-model)
20. [Why Parent Delegation?](#20-why-parent-delegation)
21. [How Parent Delegation Works](#21-how-parent-delegation-works)
22. [ClassLoader and Class Identity](#22-classloader-and-class-identity)
23. [Class Name Is Not Enough](#23-class-name-is-not-enough)
24. [Custom ClassLoader](#24-custom-classloader)
25. [When Are Custom ClassLoaders Used?](#25-when-are-custom-classloaders-used)
26. [Class.forName()](#26-classforname)
27. [ClassLoader.loadClass()](#27-classloaderloadclass)
28. [ClassLoader vs Class.forName()](#28-classloader-vs-classforname)
29. [ClassLoader and Reflection](#29-classloader-and-reflection)
30. [ClassLoader and JAR Files](#30-classloader-and-jar-files)
31. [ClassLoader and Modules](#31-classloader-and-modules)
32. [ClassLoader and Metaspace](#32-classloader-and-metaspace)
33. [ClassLoader and Garbage Collection](#33-classloader-and-garbage-collection)
34. [Class Unloading](#34-class-unloading)
35. [ClassLoader Leaks](#35-classloader-leaks)
36. [ClassLoader and Security](#36-classloader-and-security)
37. [ClassNotFoundException](#37-classnotfoundexception)
38. [NoClassDefFoundError](#38-noclassdeffounderror)
39. [ClassLoader vs JVM](#39-classloader-vs-jvm)
40. [Common Misconceptions](#40-common-misconceptions)
41. [Interview Traps](#41-interview-traps)
42. [Top 25 Interview Questions](#42-top-25-interview-questions)
43. [30-Second Interview Answer](#43-30-second-interview-answer)
44. [Cheat Sheet](#44-cheat-sheet)

---

# 1. Introduction

Before the JVM can execute a Java class, it must make that class available to the JVM.

This is where the:

> **ClassLoader**

comes into the picture.

A ClassLoader is responsible for loading class definitions into the JVM.

A simplified Java execution flow is:

```text
.java
  ↓
javac
  ↓
.class
  ↓
ClassLoader
  ↓
Class loaded into JVM
  ↓
Linking
  ↓
Initialization
  ↓
Execution
```

Class loading is one of the foundational concepts behind:

```text
JVM
Reflection
Dynamic loading
JAR files
Frameworks
Application servers
Plugins
Custom runtimes
```

---

# 2. What is a ClassLoader?

### Definition

> A ClassLoader is a JVM component responsible for loading class definitions into the JVM when they are needed.

In Java, `ClassLoader` is represented by the abstract class:

```java
java.lang.ClassLoader
```

Example:

```java
ClassLoader loader = MyClass.class.getClassLoader();

System.out.println(loader);
```

The ClassLoader does not simply "copy a `.class` file into memory."

It participates in the process through which the JVM obtains a class definition and makes the class available for execution.

---

# 3. Why Do We Need ClassLoaders?

Imagine a large application containing:

```text
1000 classes
```

It would be inefficient to assume that every class must be loaded and prepared immediately when the JVM starts.

Instead, classes can be loaded when needed.

For example:

```java
public class Main {

    public static void main(String[] args) {

        System.out.println("Application started");

        User user = new User();
    }
}
```

When `User` is needed, the JVM can arrange for its class definition to be loaded.

This provides:

```text
Dynamic loading
+
Lazy loading
+
Modularity
+
Extensibility
```

---

# 4. Where ClassLoader Fits in JVM

A simplified JVM architecture:

```text
                    JVM
                     │
             ┌───────┴────────┐
             │                │
             ▼                ▼
        ClassLoader      Runtime Areas
             │
             ▼
       Class Definition
             │
             ▼
          Linking
             │
             ▼
       Initialization
             │
             ▼
         Execution
```

The ClassLoader subsystem works before the class can be normally executed.

It works closely with:

```text
ClassLoader
    ↓
Method Area / Metaspace
    ↓
Execution Engine
```

---

# 5. What Does Class Loading Mean?

Class loading means obtaining the binary representation of a class and creating the corresponding `Class` object inside the JVM.

For a class:

```java
class Employee {
}
```

the JVM eventually has a runtime representation of that class.

Conceptually:

```text
Employee.class
     ↓
ClassLoader
     ↓
Class definition
     ↓
JVM runtime representation
     ↓
java.lang.Class object
```

The runtime class representation can then be used by:

```text
Object creation
Reflection
Method invocation
Field access
Type checking
```

---

# 6. ClassLoader Lifecycle

A useful high-level lifecycle is:

```text
Loading
   ↓
Linking
   ├── Verification
   ├── Preparation
   └── Resolution
   ↓
Initialization
```

Therefore:

```text
Loading
   ↓
Linking
   ↓
Initialization
```

with linking containing:

```text
Verification
Preparation
Resolution
```

### Important

Loading, linking, and initialization are distinct phases.

Do not treat them as one single operation.

---

# 7. Loading

During loading, the JVM obtains the binary representation of a class or interface and creates its runtime representation.

Conceptually:

```text
Class Name
    ↓
ClassLoader
    ↓
Find class bytes
    ↓
Define class
    ↓
Class object
```

For example:

```text
com.example.User
```

might be found in:

```text
User.class
```

inside a directory or JAR.

The actual source can be:

```text
File system
JAR
Network
Generated bytes
Custom source
```

A ClassLoader can obtain class data from different sources.

---

# 8. Linking

After loading, the class goes through linking.

Linking consists conceptually of:

```text
Linking
   │
   ├── Verification
   ├── Preparation
   └── Resolution
```

### Verification

Checks whether the loaded class is structurally and semantically valid according to JVM requirements.

### Preparation

Allocates memory for class-level/static fields and gives them their default values.

### Resolution

Transforms symbolic references into direct references when required; resolution may be lazy.

---

# 9. Verification

Verification ensures that loaded bytecode satisfies JVM constraints.

The JVM verifies things such as:

```text
Bytecode structure
Type correctness
Valid instruction usage
Valid control flow
Operand stack consistency
```

Conceptually:

```text
Class bytes
    ↓
Verification
    ↓
Valid?
  /   \
Yes    No
 |      |
 ↓      ↓
Continue Error
```

This is an important part of JVM bytecode safety and correctness.

---

# 10. Preparation

During preparation, the JVM prepares memory needed for class or interface variables and assigns their default values.

Example:

```java
class Demo {

    static int count = 10;
}
```

During preparation, `count` receives its default value:

```text
count = 0
```

The explicit initializer:

```java
count = 10;
```

belongs to initialization, not preparation.

### Important Interview Trap

```text
Preparation
→ default values

Initialization
→ explicit static initialization
```

For example:

```java
static int count = 10;
```

Conceptually:

```text
Preparation
→ count = 0

Initialization
→ count = 10
```

---

# 11. Resolution

Resolution involves replacing symbolic references with direct references.

Consider:

```java
User user = new User();
```

Bytecode contains symbolic references to classes, methods, and fields.

Conceptually:

```text
Symbolic Reference
       ↓
Resolution
       ↓
Direct Reference
```

### Important

Resolution does not necessarily happen immediately for every reference.

The JVM specification permits resolution to occur lazily.

Therefore:

```text
Loaded
   ↓
Linked
   ↓
Some resolution may happen later
```

---

# 12. Initialization

Initialization executes the class's initialization logic.

For example:

```java
class Demo {

    static int value = 100;

    static {
        System.out.println("Class initialized");
    }
}
```

During initialization:

```text
value = 100
```

and:

```text
static block executes
```

The JVM performs initialization through the class initialization method commonly represented internally as:

```text
<clinit>
```

### Important

`<clinit>` is not a Java method that you write directly.

It is generated by the compiler when class initialization requires it.

---

# 13. Class Loading Flow

The complete conceptual flow:

```text
                Class Needed
                     ↓
               ClassLoader
                     ↓
                  Loading
                     ↓
                  Linking
              ┌──────┼──────┐
              │      │      │
              ▼      ▼      ▼
         Verification Preparation Resolution
              │      │      │
              └──────┼──────┘
                     ↓
                Initialization
                     ↓
                  Execution
```

Memory trick:

```text
Load
 ↓
Link
 ↓
Initialize
```

And:

```text
Link
=
Verify + Prepare + Resolve
```

---

# 14. Built-in ClassLoaders

Modern Java uses several built-in class loaders.

The commonly discussed ones are:

```text
Bootstrap ClassLoader
        ↓
Platform ClassLoader
        ↓
Application ClassLoader
```

These form the basis of the parent-delegation hierarchy.

### Important

In modern Java, the old Java 8 terminology:

```text
Bootstrap
Extension
Application
```

changed after the Java Platform Module System was introduced.

The modern terminology is:

```text
Bootstrap
Platform
Application
```

---

# 15. Bootstrap ClassLoader

The Bootstrap ClassLoader loads fundamental Java runtime classes.

Examples include core classes such as:

```text
java.lang.Object
java.lang.String
java.lang.System
```

It is implemented by the JVM itself rather than as an ordinary Java class.

Therefore:

```java
System.out.println(Object.class.getClassLoader());
```

produces:

```text
null
```

This does **not** mean `Object` was not loaded.

It means the Bootstrap ClassLoader is represented as `null` from the Java `ClassLoader` API perspective.

### Important

Do not say:

> Bootstrap ClassLoader is a Java object whose reference is null.

A better statement is:

> The Bootstrap ClassLoader is implemented by the JVM, and `Class.getClassLoader()` returns `null` for classes loaded by it.

---

# 16. Platform ClassLoader

The Platform ClassLoader loads Java platform classes that are not loaded by the Bootstrap ClassLoader.

It replaced the old concept of the:

```text
Extension ClassLoader
```

in modern Java.

You can obtain it with:

```java
ClassLoader platform = ClassLoader.getPlatformClassLoader();

System.out.println(platform);
```

The exact set of classes associated with this loader depends on the Java platform/module configuration.

---

# 17. Application ClassLoader

The Application ClassLoader is also called the:

> System ClassLoader

It loads application classes and classes available through the application class path/module configuration.

Example:

```java
ClassLoader loader =
        ClassLoader.getSystemClassLoader();

System.out.println(loader);
```

A typical application class may report an application/system loader:

```java
System.out.println(MyClass.class.getClassLoader());
```

### Important

The exact implementation class name of the application loader can vary between Java versions and JVM implementations.

---

# 18. ClassLoader Hierarchy

A simplified modern hierarchy:

```text
             Bootstrap
                 │
                 ▼
             Platform
                 │
                 ▼
            Application
```

For a custom ClassLoader:

```text
             Bootstrap
                 │
                 ▼
             Platform
                 │
                 ▼
            Application
                 │
                 ▼
        Custom ClassLoader
```

The relationship is based on:

> Parent delegation

---

# 19. Parent Delegation Model

When a ClassLoader is asked to load a class, it normally delegates the request to its parent first.

Conceptually:

```text
Custom ClassLoader
        ↓
      Parent
        ↓
      Parent
        ↓
    Bootstrap
```

Only when the parent cannot load the class does the child attempt to load it itself.

Conceptually:

```text
Request class
      ↓
Ask parent
      ↓
Parent can load?
    /       \
  Yes        No
   ↓          ↓
Return     Child tries
```

This is called the:

> Parent Delegation Model

---

# 20. Why Parent Delegation?

The parent-delegation model provides several benefits.

## 20.1 Prevent Duplicate Core Classes

Suppose an application tries to provide its own:

```text
java.lang.String
```

If the application ClassLoader could immediately load its own version, the application could potentially interfere with core Java classes.

With delegation:

```text
Application ClassLoader
        ↓
Parent
        ↓
Bootstrap
        ↓
java.lang.String
```

The trusted platform class is loaded by the appropriate parent.

---

## 20.2 Consistent Class Identity

It helps ensure that core platform classes are loaded consistently.

---

## 20.3 Security and Isolation

Delegation reduces the ability of application-level code to replace fundamental platform classes.

---

# 21. How Parent Delegation Works

Consider:

```java
Class<?> clazz =
        Class.forName("com.example.User");
```

Conceptually:

```text
Application ClassLoader
        ↓
Ask parent
        ↓
Platform
        ↓
Ask parent
        ↓
Bootstrap
        ↓
Not found
        ↓
Return to child
        ↓
Application ClassLoader searches
        ↓
User.class found
        ↓
Class defined
```

For a JDK class:

```text
java.lang.String
```

the request can eventually be handled by the Bootstrap ClassLoader.

---

# 22. ClassLoader and Class Identity

One of the most important JVM concepts is:

> A class is identified not only by its fully qualified class name, but also by the ClassLoader that defines it.

Conceptually:

```text
Class Identity
=
Class Name
+
Defining ClassLoader
```

Suppose two ClassLoaders load:

```text
com.example.User
```

Then the JVM can treat them as different runtime classes.

```text
Loader A + com.example.User
        ≠
Loader B + com.example.User
```

even though the class names are identical.

---

# 23. Class Name Is Not Enough

Imagine:

```text
ClassLoader A
    ↓
com.example.User

ClassLoader B
    ↓
com.example.User
```

The names are the same.

But:

```text
User(A)
   ≠
User(B)
```

from the JVM's type-identity perspective.

This is why a value loaded by one ClassLoader can fail to cast to a class with the same name loaded by another ClassLoader.

Example conceptually:

```text
ClassCastException
```

can occur when incompatible class-loader identities are involved.

### Interview Memory Trick

```text
Class Identity
=
Fully Qualified Name
+
Defining ClassLoader
```

---

# 24. Custom ClassLoader

Java allows developers to create custom ClassLoaders.

You can extend:

```java
ClassLoader
```

and customize how class bytes are obtained or defined.

Example:

```java
public class MyClassLoader extends ClassLoader {

    @Override
    protected Class<?> findClass(String name)
            throws ClassNotFoundException {

        // Locate class bytes
        // Read bytes
        // Define class

        return super.findClass(name);
    }
}
```

A real custom implementation would normally obtain the bytecode bytes and call:

```java
defineClass(...)
```

appropriately.

---

# 25. When Are Custom ClassLoaders Used?

Custom ClassLoaders can be useful for:

```text
Plugin systems
Application servers
Modular applications
Dynamic code loading
Hot deployment
Isolation
Specialized class sources
Encrypted/processed class bytes
```

For example:

```text
Application
   │
   ├── Core classes
   │
   ├── Plugin A
   │      ↓
   │   ClassLoader A
   │
   └── Plugin B
          ↓
       ClassLoader B
```

Different plugins can have different class-loading boundaries.

---

# 26. Class.forName()

`Class.forName()` can load a class by its fully qualified name.

Example:

```java
Class<?> clazz =
        Class.forName("java.lang.String");

System.out.println(clazz);
```

The commonly used overload:

```java
Class.forName(String className)
```

initializes the class if it has not already been initialized.

There is also an overload:

```java
Class.forName(
    String className,
    boolean initialize,
    ClassLoader loader
);
```

This allows you to control whether initialization occurs and which ClassLoader is used.

Example:

```java
Class<?> clazz = Class.forName(
        "com.example.User",
        false,
        loader
);
```

Here:

```text
initialize = false
```

means the class is loaded without triggering initialization through this call.

---

# 27. ClassLoader.loadClass()

A ClassLoader provides:

```java
loadClass(String name)
```

Example:

```java
ClassLoader loader =
        ClassLoader.getSystemClassLoader();

Class<?> clazz =
        loader.loadClass("java.lang.String");
```

The `loadClass()` operation generally loads the class without initializing it.

Initialization happens later when required by JVM rules.

### Important

Do not confuse:

```java
loadClass()
```

with:

```java
Class.forName()
```

because their initialization behavior differs.

---

# 28. ClassLoader vs Class.forName()

| Feature | `ClassLoader.loadClass()` | `Class.forName()` |
|---|---|---|
| Loads class | Yes | Yes |
| Initializes by default | No | Yes |
| Can specify ClassLoader | Through the loader object | Yes, using overload |
| Main use | Explicit class loading | Loading and optionally initializing |
| Returns | `Class<?>` | `Class<?>` |

Example:

```java
ClassLoader loader =
        ClassLoader.getSystemClassLoader();

Class<?> a =
        loader.loadClass("com.example.User");
```

Compared with:

```java
Class<?> b =
        Class.forName("com.example.User");
```

The second form initializes the class by default if necessary.

---

# 29. ClassLoader and Reflection

Reflection works closely with the `Class` object.

Example:

```java
Class<?> clazz = String.class;

System.out.println(clazz.getName());
System.out.println(clazz.getMethods().length);
```

The `Class` object provides runtime information about the loaded class.

Conceptually:

```text
ClassLoader
     ↓
Loads class
     ↓
Class object
     ↓
Reflection API
     ↓
Inspect:
- methods
- fields
- constructors
- modifiers
- annotations
```

This is heavily used by frameworks.

---

# 30. ClassLoader and JAR Files

Classes do not have to exist as loose `.class` files.

They can be packaged inside:

```text
JAR
```

For example:

```text
application.jar
│
├── com/
│   └── example/
│       └── User.class
│
└── META-INF/
```

The application ClassLoader can obtain class definitions from the configured application class path/module path.

Other specialized ClassLoaders can also load classes from JARs or other sources.

---

# 31. ClassLoader and Modules

Since Java 9, Java includes the:

> Java Platform Module System (JPMS)

Modules introduced stronger boundaries around packages and dependencies.

Class loading and module resolution are related but are not identical concepts.

Conceptually:

```text
Module System
      +
Class Loading
      +
Access Control
```

The ClassLoader determines how class definitions are loaded, while the module system also determines module relationships, readability, exports, and encapsulation.

---

# 32. ClassLoader and Metaspace

When a class is loaded, the JVM needs runtime metadata associated with that class.

In HotSpot, class metadata is stored in:

> **Metaspace**

Conceptually:

```text
ClassLoader
     ↓
Load Class
     ↓
Class Metadata
     ↓
Metaspace
```

### Important

Do not say:

> ClassLoader stores classes inside Metaspace.

A better explanation is:

> The ClassLoader loads and defines classes, while the JVM stores associated class metadata in runtime structures such as Metaspace in HotSpot.

The exact implementation details are JVM-specific.

---

# 33. ClassLoader and Garbage Collection

A ClassLoader can itself become unreachable.

If a ClassLoader becomes unreachable and its loaded classes are no longer reachable in ways that prevent unloading, the JVM may unload those classes.

Conceptually:

```text
ClassLoader
    ↓
Loaded Classes
    ↓
ClassLoader becomes unreachable
    ↓
Classes become unloadable
    ↓
GC / class unloading
```

Class unloading is associated with the lifecycle of the defining ClassLoader.

---

# 34. Class Unloading

Class unloading is different from object garbage collection.

For normal Java objects:

```text
Object unreachable
     ↓
GC
     ↓
Object reclaimed
```

For classes:

```text
ClassLoader becomes unreachable
     ↓
Its classes may become unloadable
     ↓
Class metadata/resources can be reclaimed
```

### Important

The JVM does not normally unload individual classes arbitrarily while keeping the defining ClassLoader alive.

Class unloading is strongly connected to ClassLoader reachability.

---

# 35. ClassLoader Leaks

A ClassLoader leak occurs when an application unintentionally keeps references that prevent a ClassLoader and its classes from becoming unreachable.

This can be a serious problem in long-running systems such as:

```text
Application servers
Plugin systems
Hot deployment systems
```

Potential sources include:

```text
Static fields
ThreadLocal values
Running threads
Thread context ClassLoader references
Caches
Listeners
Registrations
```

Conceptually:

```text
Old ClassLoader
      ↓
Should be collected
      ↓
Hidden reference remains
      ↓
ClassLoader stays reachable
      ↓
Classes cannot unload
      ↓
Memory retained
```

---

# 36. ClassLoader and Security

The parent-delegation model helps protect core platform classes.

For example, an application should not simply replace:

```text
java.lang.String
```

with its own incompatible implementation.

Delegation causes the request to be handled by the appropriate parent loader first.

Conceptually:

```text
Application request
      ↓
Application ClassLoader
      ↓
Parent
      ↓
Bootstrap
      ↓
Core platform class
```

### Important

Modern Java's security model is broader than ClassLoader delegation alone.

Do not claim:

> ClassLoader is the entire Java security mechanism.

It is only one part of the JVM/platform architecture.

---

# 37. ClassNotFoundException

`ClassNotFoundException` is a checked exception.

It commonly occurs when code explicitly tries to load a class by name and the class cannot be found.

Example:

```java
Class.forName("com.example.DoesNotExist");
```

If the class cannot be found:

```text
ClassNotFoundException
```

may be thrown.

### Common Cause

```text
Wrong class name
Missing dependency
Incorrect classpath/module configuration
```

---

# 38. NoClassDefFoundError

`NoClassDefFoundError` is an `Error`, not an exception.

It can occur when the JVM or application tries to use a class that was available when the dependent class was compiled, but is unavailable or could not be defined at runtime.

Conceptually:

```text
Compilation
    ↓
Dependency available
    ↓
Runtime
    ↓
Dependency missing/unavailable
    ↓
NoClassDefFoundError
```

### Important Difference

```text
ClassNotFoundException
→ Often explicit/dynamic class loading failure

NoClassDefFoundError
→ JVM cannot find/define a class that is required at runtime
```

The exact cause of `NoClassDefFoundError` can vary, so the exception's cause chain and stack trace should be inspected.

---

# 39. ClassLoader vs JVM

These concepts should not be confused.

### JVM

Responsible for:

```text
Loading classes
Executing bytecode/native code
Memory management
Threads
GC
JIT
Runtime execution
```

### ClassLoader

Responsible for the class-loading portion:

```text
Locate class data
Load class definition
Delegate loading
Define classes
```

Conceptually:

```text
JVM
 │
 ├── ClassLoader subsystem
 ├── Runtime Data Areas
 ├── Execution Engine
 ├── Garbage Collector
 └── Other runtime components
```

---

# 40. Common Misconceptions

## Misconception 1

> ClassLoader loads `.java` files.

### Correct

ClassLoaders work with class definitions/bytecode, not Java source files in the normal execution model.

```text
.java
 ↓
javac
 ↓
.class
 ↓
ClassLoader
```

---

## Misconception 2

> ClassLoader creates Java objects.

### Correct

ClassLoader loads class definitions.

Objects are created using:

```java
new
```

or other mechanisms such as reflection.

---

## Misconception 3

> ClassLoader and JVM are the same thing.

### Correct

ClassLoader is a subsystem/component involved in class loading inside the JVM.

---

## Misconception 4

> Every class is loaded by the Application ClassLoader.

### Correct

Different classes can be loaded by different ClassLoaders.

Core platform classes are associated with the Bootstrap loader, for example.

---

## Misconception 5

> Bootstrap ClassLoader is a normal Java ClassLoader object.

### Correct

It is implemented by the JVM and appears as `null` through `Class.getClassLoader()` for classes it loads.

---

## Misconception 6

> Loading automatically means initialization.

### Correct

Loading, linking, and initialization are separate phases.

---

## Misconception 7

> Loading always means resolving every reference immediately.

### Correct

Resolution can be lazy.

---

## Misconception 8

> Class identity depends only on the class name.

### Correct

Class identity involves the class's name and its defining ClassLoader.

---

## Misconception 9

> ClassLoader loads a class only once globally.

### Correct

Different ClassLoaders can define classes with the same binary name as separate runtime types.

---

## Misconception 10

> `ClassLoader.loadClass()` always initializes the class.

### Correct

It generally loads without initializing.

---

## Misconception 11

> `Class.forName()` only loads a class.

### Correct

The common `Class.forName(String)` overload also initializes the class.

---

## Misconception 12

> Class unloading happens whenever an individual class is no longer used.

### Correct

Class unloading is strongly associated with the defining ClassLoader becoming unreachable and eligible for unloading.

---

# 41. Interview Traps

## Trap 1

### Question

What is a ClassLoader?

### Answer

A ClassLoader is responsible for loading class definitions into the JVM.

---

## Trap 2

### Question

What are the main built-in ClassLoaders?

### Answer

In modern Java:

```text
Bootstrap
Platform
Application
```

---

## Trap 3

### Question

What replaced the Extension ClassLoader?

### Answer

The Platform ClassLoader replaced the old Extension ClassLoader terminology in modern Java.

---

## Trap 4

### Question

Why is `Object.class.getClassLoader()` null?

### Answer

Because `Object` is loaded by the Bootstrap ClassLoader, which is implemented by the JVM and represented as `null` by this Java API.

---

## Trap 5

### Question

What is parent delegation?

### Answer

A ClassLoader normally asks its parent to load a class before attempting to load the class itself.

---

## Trap 6

### Question

Why use parent delegation?

### Answer

It helps maintain consistent loading of platform classes, avoids duplicate definitions of core classes, and contributes to isolation and security.

---

## Trap 7

### Question

What is class identity?

### Answer

A runtime class is identified by its binary name together with its defining ClassLoader.

---

## Trap 8

### Question

What is the difference between loading and initialization?

### Answer

Loading obtains and defines the class, while initialization executes the class's initialization logic such as static field initialization and static blocks.

---

## Trap 9

### Question

What are the stages of linking?

### Answer

```text
Verification
Preparation
Resolution
```

Resolution may occur lazily.

---

## Trap 10

### Question

What is the difference between `Class.forName()` and `loadClass()`?

### Answer

The common `Class.forName(String)` initializes the class, while `ClassLoader.loadClass()` generally does not.

---

## Trap 11

### Question

Can two ClassLoaders load the same class name?

### Answer

Yes. They can define separate runtime classes with the same binary name.

---

## Trap 12

### Question

Can two classes with the same name be incompatible?

### Answer

Yes, if they were defined by different ClassLoaders.

---

## Trap 13

### Question

When can classes be unloaded?

### Answer

When their defining ClassLoader becomes unreachable and the JVM determines that the classes can be unloaded.

---

## Trap 14

### Question

What is `ClassNotFoundException`?

### Answer

A checked exception commonly associated with explicit attempts to load a class by name when the class cannot be found.

---

## Trap 15

### Question

What is `NoClassDefFoundError`?

### Answer

An `Error` that can occur when a class required at runtime cannot be found or defined, often despite being available during compilation.

---

# 42. Top 25 Interview Questions

## Q1. What is a ClassLoader?

A ClassLoader loads class definitions into the JVM.

---

## Q2. Why do we need ClassLoaders?

They allow classes to be loaded dynamically and when needed, supporting modularity, plugins, frameworks, and large applications.

---

## Q3. What are the main ClassLoaders in modern Java?

```text
Bootstrap
Platform
Application
```

---

## Q4. What is the Bootstrap ClassLoader?

It is the JVM-provided loader responsible for fundamental Java runtime classes.

---

## Q5. Why does `Object.class.getClassLoader()` return null?

Because `Object` is loaded by the Bootstrap ClassLoader, which is represented as `null` through this API.

---

## Q6. What is the Platform ClassLoader?

It loads platform classes that are not handled by the Bootstrap ClassLoader.

---

## Q7. What is the Application ClassLoader?

It loads application classes and classes available through the application's class path/module configuration.

---

## Q8. What is parent delegation?

A ClassLoader normally asks its parent to load a class before trying to load it itself.

---

## Q9. Why is parent delegation important?

It helps avoid duplicate loading of platform classes and supports consistency, isolation, and security.

---

## Q10. What are the phases of class loading?

The broader lifecycle is:

```text
Loading
Linking
Initialization
```

Linking includes:

```text
Verification
Preparation
Resolution
```

---

## Q11. What happens during verification?

The JVM checks that the loaded class's bytecode satisfies required JVM constraints.

---

## Q12. What happens during preparation?

Memory is prepared for class/static fields and those fields receive default values.

---

## Q13. What happens during resolution?

Symbolic references may be converted into direct references. Resolution can be lazy.

---

## Q14. What happens during initialization?

Static initialization logic is executed, including explicit static field initializers and static blocks.

---

## Q15. What is `<clinit>`?

It is the JVM-level class initialization method generated when needed to execute static initialization logic.

---

## Q16. Can two ClassLoaders load classes with the same name?

Yes.

---

## Q17. Are two same-named classes loaded by different ClassLoaders necessarily the same type?

No. Their defining ClassLoaders are part of their runtime identity.

---

## Q18. What is a custom ClassLoader?

A ClassLoader implementation created to customize where or how class definitions are obtained and defined.

---

## Q19. Where are custom ClassLoaders useful?

```text
Plugins
Application servers
Dynamic loading
Isolation
Hot deployment
Specialized class sources
```

---

## Q20. What is `Class.forName()`?

It loads a class by name and, with its common overload, initializes it if necessary.

---

## Q21. What is `ClassLoader.loadClass()`?

It loads a class by name without normally initializing it.

---

## Q22. What is ClassLoader class identity?

A runtime type is identified by its binary name and defining ClassLoader.

---

## Q23. When can a class be unloaded?

When its defining ClassLoader becomes unreachable and the JVM can safely unload the associated classes.

---

## Q24. Difference between `ClassNotFoundException` and `NoClassDefFoundError`?

```text
ClassNotFoundException
→ Checked exception
→ Often explicit dynamic loading failure

NoClassDefFoundError
→ Error
→ Required class unavailable/unusable at runtime
```

---

## Q25. What is the relation between ClassLoader and Metaspace?

ClassLoader loads and defines classes, while HotSpot stores associated class metadata in Metaspace.

---

# 43. 30-Second Interview Answer

> A ClassLoader is a JVM component responsible for loading class definitions into the JVM. Modern Java has Bootstrap, Platform, and Application ClassLoaders, with parent delegation between them. When a class is needed, the JVM goes through loading, linking, and initialization. Linking includes verification, preparation, and resolution. Parent delegation means a loader normally asks its parent to load a class first. Class identity depends on both the class's binary name and its defining ClassLoader, which is why two loaders can create separate types with the same name. ClassLoaders are also important for reflection, frameworks, plugins, dynamic loading, and class unloading.

---

# 44. Cheat Sheet

```text
╔════════════════════════════════════════════════════════════╗
║                       CLASSLOADER                         ║
╠════════════════════════════════════════════════════════════╣
║ Purpose       → Load class definitions into JVM            ║
║ API           → java.lang.ClassLoader                      ║
║ Main loaders  → Bootstrap, Platform, Application           ║
║ Model         → Parent Delegation                          ║
║ Lifecycle     → Loading → Linking → Initialization         ║
║ Linking       → Verification + Preparation + Resolution    ║
║ Metadata      → Metaspace in HotSpot                       ║
║ Identity      → Class Name + Defining ClassLoader          ║
║ Custom Loader → Plugins / isolation / dynamic loading      ║
║ Unloading     → Strongly tied to defining ClassLoader      ║
╚════════════════════════════════════════════════════════════╝
```

## 🧠 Main Memory Trick

```text
CLASSLOADER
     ↓
"How does the JVM get this class?"
```

---

## 🔥 Class Loading Lifecycle

```text
                Class Needed
                     ↓
                  Loading
                     ↓
                  Linking
                     │
          ┌──────────┼──────────┐
          ↓          ↓          ↓
     Verification Preparation Resolution
          │          │          │
          └──────────┼──────────┘
                     ↓
               Initialization
                     ↓
                  Execution
```

### Remember

```text
LOAD
 ↓
LINK
 ↓
INITIALIZE
```

And:

```text
LINK
=
VERIFY
+
PREPARE
+
RESOLVE
```

---

## 🔥 ClassLoader Hierarchy

```text
             Bootstrap
                 │
                 ▼
             Platform
                 │
                 ▼
            Application
                 │
                 ▼
        Custom ClassLoader
```

---

## 🔥 Parent Delegation

```text
Request Class
      ↓
Child ClassLoader
      ↓
Ask Parent
      ↓
Parent can load?
   ┌──┴──┐
  Yes    No
   │      │
   ▼      ▼
Return   Child loads
```

---

## 🔥 Class Identity

```text
             Class Identity
                    │
          ┌─────────┴─────────┐
          ↓                   ↓
   Binary Class Name    Defining Loader
          │                   │
          └─────────┬─────────┘
                    ↓
              Runtime Type
```

Therefore:

```text
Loader A + User
      ≠
Loader B + User
```

even when:

```text
Both class names = com.example.User
```

---

## 🔥 `loadClass()` vs `Class.forName()`

```text
ClassLoader.loadClass()
        ↓
      Load
        ↓
Usually does NOT initialize


Class.forName()
        ↓
      Load
        ↓
Initialize by default
```

---

## 🔥 ClassNotFoundException vs NoClassDefFoundError

```text
ClassNotFoundException
        ↓
Checked Exception
        ↓
Explicit/dynamic class loading
        ↓
Class cannot be found


NoClassDefFoundError
        ↓
Error
        ↓
Required class unavailable/unusable
        ↓
Runtime failure
```

---

## 🔥 ClassLoader + Metaspace

```text
ClassLoader
     ↓
Load / Define Class
     ↓
JVM Runtime Representation
     ↓
Class Metadata
     ↓
Metaspace
```

---

## 🔥 Class Unloading

```text
ClassLoader
     ↓
Loaded Classes
     ↓
ClassLoader becomes unreachable
     ↓
Classes become eligible for unloading
     ↓
JVM may reclaim class-related resources
```

---

## ⭐ Final Interview Rule

```text
ClassLoader
→ Loads classes, not objects
→ Works with class definitions/bytecode
→ Bootstrap is JVM-provided
→ Platform replaced old Extension terminology
→ Application = System ClassLoader
→ Parent delegation is the normal model
→ Loading ≠ Linking ≠ Initialization
→ Linking = Verification + Preparation + Resolution
→ Resolution can be lazy
→ Class identity includes defining ClassLoader
→ Custom loaders enable plugins/isolation
→ loadClass() generally does not initialize
→ Class.forName() initializes by default
→ Class unloading is tied to ClassLoader lifecycle
```

## 🚀 One-Line Revision

```text
ClassLoader = The JVM mechanism that loads and defines classes, using parent delegation and participating in the Loading → Linking → Initialization lifecycle.
```