# 📦 Java Modules

> **A Java module is a named, self-describing collection of packages that explicitly declares which packages it exposes and which other modules it requires.**

---

# 📑 Table of Contents

- [1. What Is a Module?](#1-what-is-a-module)
- [2. Why Were Modules Introduced?](#2-why-were-modules-introduced)
- [3. Java Module System](#3-java-module-system)
- [4. Module vs Package](#4-module-vs-package)
- [5. Module vs JAR](#5-module-vs-jar)
- [6. `module-info.java`](#6-module-infojava)
- [7. Basic Module Declaration](#7-basic-module-declaration)
- [8. `requires`](#8-requires)
- [9. `exports`](#9-exports)
- [10. `opens`](#10-opens)
- [11. `requires transitive`](#11-requires-transitive)
- [12. `requires static`](#12-requires-static)
- [13. `exports ... to`](#13-exports--to)
- [14. `opens ... to`](#14-opens--to)
- [15. `uses`](#15-uses)
- [16. `provides ... with`](#16-provides--with)
- [17. Service Provider Architecture](#17-service-provider-architecture)
- [18. Named Modules](#18-named-modules)
- [19. Automatic Modules](#19-automatic-modules)
- [20. Unnamed Module](#20-unnamed-module)
- [21. Module Path](#21-module-path)
- [22. Classpath vs Module Path](#22-classpath-vs-module-path)
- [23. Strong Encapsulation](#23-strong-encapsulation)
- [24. Module Readability](#24-module-readability)
- [25. Package Accessibility](#25-package-accessibility)
- [26. Public Does Not Always Mean Accessible](#26-public-does-not-always-mean-accessible)
- [27. Split Packages](#27-split-packages)
- [28. Cyclic Dependencies](#28-cyclic-dependencies)
- [29. Root Modules](#29-root-modules)
- [30. `jlink`](#30-jlink)
- [31. `jdeps`](#31-jdeps)
- [32. `jar` and Modules](#32-jar-and-modules)
- [33. Module Naming](#33-module-naming)
- [34. Module Graph](#34-module-graph)
- [35. Reflection and Modules](#35-reflection-and-modules)
- [36. Advantages](#36-advantages)
- [37. Limitations](#37-limitations)
- [38. Common Mistakes](#38-common-mistakes)
- [39. Top 25 Interview Questions](#39-top-25-interview-questions)
- [40. 30-Second Interview Answer](#40-30-second-interview-answer)
- [41. Cheat Sheet](#41-cheat-sheet)
- [42. Final Mental Model](#42-final-mental-model)

---

# 1. What Is a Module?

A module is a higher-level unit of organization than a package.

Before Java 9, applications were mainly organized as:

```text
Application
    |
    +── Packages
          |
          +── Classes
```

With the Java Platform Module System:

```text
Application
    |
    +── Modules
          |
          +── Packages
                |
                +── Classes
```

A module can explicitly define:

```text
What it requires
What it exports
What it opens for reflection
What services it uses
What services it provides
```

---

# 2. Why Were Modules Introduced?

The Java Module System was introduced in **Java 9**.

It was designed to address problems that become significant in large applications and the JDK itself.

Important goals include:

```text
Strong encapsulation
Reliable configuration
Explicit dependencies
Better maintainability
Smaller runtime images
Better organization
```

Before modules, a JAR could contain many packages, but there was no standard module-level declaration saying:

```text
These packages are public API.
These dependencies are required.
These packages should remain internal.
```

Modules provide that explicit boundary.

---

# 3. Java Module System

The Java Platform Module System is commonly called:

```text
JPMS
```

It was introduced in:

```text
Java 9
```

The central idea is:

```text
Module
    |
    +── requires
    |
    +── exports
    |
    +── opens
    |
    +── uses
    |
    +── provides
```

The module system operates at the module level while Java's normal access modifiers continue to operate at the class/member level.

---

# 4. Module vs Package

These are not the same.

## Package

A package groups related classes and interfaces.

Example:

```java
package com.example.service;
```

A module can contain multiple packages.

Example:

```text
com.example.app
    |
    +── com.example.service
    +── com.example.model
    +── com.example.repository
```

Conceptually:

```text
Module
  ├── Package
  ├── Package
  └── Package
```

Think:

```text
Package
→ groups classes

Module
→ groups packages + declares dependencies and boundaries
```

---

# 5. Module vs JAR

A JAR is a packaging format.

A module is a logical unit in JPMS.

A JAR can be:

```text
Modular JAR
```

or:

```text
Non-modular JAR
```

A modular JAR normally contains:

```text
module-info.class
```

which is compiled from:

```text
module-info.java
```

Therefore:

```text
JAR
→ packaging format

Module
→ dependency/encapsulation unit
```

---

# 6. `module-info.java`

The module declaration is written in:

```text
module-info.java
```

Example:

```java
module com.example.app {

}
```

This file describes the module.

After compilation, it becomes:

```text
module-info.class
```

The file is normally placed at the root of the module's source structure.

Example:

```text
src/
└── com.example.app/
    ├── module-info.java
    └── com/
        └── example/
            └── app/
                └── Main.java
```

---

# 7. Basic Module Declaration

Basic syntax:

```java
module module.name {

}
```

Example:

```java
module com.example.app {

}
```

A module name should be unique in the module graph.

A common naming convention is reverse-domain style:

```text
com.example.app
```

---

# 8. `requires`

The `requires` directive declares a dependency on another module.

Example:

```java
module com.example.app {

    requires com.example.database;
}
```

Meaning:

```text
com.example.app
        |
        ↓
com.example.database
```

The application module can read the required module.

Example:

```text
Module A
   |
   | requires
   ↓
Module B
```

---

# 9. `exports`

The `exports` directive makes a package accessible to other modules.

Example:

```java
module com.example.library {

    exports com.example.api;
}
```

Suppose the module contains:

```text
com.example.api
com.example.internal
```

Only:

```text
com.example.api
```

is exported.

Therefore:

```text
api
→ accessible to other modules

internal
→ not exported
```

This provides strong encapsulation.

---

# 10. `opens`

`opens` is primarily related to deep reflection.

Example:

```java
module com.example.app {

    opens com.example.model;
}
```

This allows reflective access to the package.

Important distinction:

```text
exports
→ normal access

opens
→ reflective access
```

Frameworks that use reflection may need packages to be opened.

---

# 11. `requires transitive`

Normally:

```java
requires B;
```

means:

```text
A can read B
```

It does not automatically mean that another module reading A can read B.

With:

```java
requires transitive B;
```

the dependency becomes part of A's readable API relationship.

Example:

```java
module A {

    requires transitive B;
}
```

If:

```text
C requires A
```

then C can also read B through A's transitive requirement.

Conceptually:

```text
C
 |
 ↓
A
 |
 ↓
B
```

The readability of B is propagated through A.

Use this when the dependency is intentionally part of the module's exposed API relationship.

---

# 12. `requires static`

`requires static` declares a compile-time dependency that is optional at runtime.

Example:

```java
module com.example.app {

    requires static com.example.optional;
}
```

Meaning:

```text
Required for compilation
+
Not necessarily required at runtime
```

This is useful for optional dependencies such as:

```text
Annotations
Compile-time tooling
Optional integrations
```

---

# 13. `exports ... to`

A package can be exported only to specific modules.

Example:

```java
module com.example.library {

    exports com.example.api
        to com.example.client;
}
```

This is called a qualified export.

Meaning:

```text
com.example.api
→ accessible to com.example.client

not generally exported to every module
```

---

# 14. `opens ... to`

A package can also be opened only to specific modules.

Example:

```java
module com.example.app {

    opens com.example.model
        to framework.module;
}
```

This is called a qualified open.

It gives the specified module reflective access.

---

# 15. `uses`

The `uses` directive declares that a module consumes a service.

Example:

```java
module com.example.application {

    uses com.example.spi.PaymentService;
}
```

The module is saying:

```text
I need implementations of PaymentService.
```

The actual implementation can be discovered through Java's service-loading mechanism.

---

# 16. `provides ... with`

The `provides` directive declares that a module provides an implementation of a service.

Example:

```java
module com.example.payment {

    provides com.example.spi.PaymentService
        with com.example.payment.UPIPaymentService;
}
```

This means:

```text
Service:
PaymentService

Implementation:
UPIPaymentService
```

A service-consuming module can discover the provider through:

```text
ServiceLoader
```

---

# 17. Service Provider Architecture

Java modules support service-oriented design.

Example:

```text
              Service Interface
                     |
                     ↓
              PaymentService
                     |
          +----------+----------+
          |                     |
          ↓                     ↓
      Provider A            Provider B
```

Consumer module:

```java
module com.example.app {

    uses com.example.PaymentService;
}
```

Provider module:

```java
module com.example.provider {

    provides com.example.PaymentService
        with com.example.UPIPaymentService;
}
```

The service can be loaded with:

```java
ServiceLoader.load(
    PaymentService.class
);
```

This allows implementations to be discovered without directly depending on concrete provider classes.

---

# 18. Named Modules

A named module has an explicit module identity.

Example:

```java
module com.example.app {

}
```

Named modules can be:

```text
Explicit named modules
Automatic modules
```

A module with:

```text
module-info.java
```

is an explicit named module.

---

# 19. Automatic Modules

A non-modular JAR placed on the module path can become an automatic module.

For example:

```text
legacy-library.jar
```

does not contain:

```text
module-info.class
```

but if placed on the module path, Java can treat it as an automatic module.

Its module name is derived from the JAR name unless a manifest entry such as:

```text
Automatic-Module-Name
```

provides a name.

Automatic modules are useful during migration of existing libraries toward JPMS.

---

# 20. Unnamed Module

Code running on the traditional classpath belongs to the:

```text
Unnamed Module
```

There is one unnamed module per class loader.

It is not declared using:

```text
module-info.java
```

The unnamed module can read all named modules in the module graph, subject to the module system's rules.

Code in the unnamed module is common when an application continues to use the traditional classpath.

---

# 21. Module Path

The module path is used to locate modules.

It is conceptually similar to the classpath but understands modules.

Example:

```text
--module-path
```

or:

```text
-p
```

Example command:

```text
java --module-path mods \
     --module com.example.app/com.example.Main
```

Here:

```text
mods
→ module path

com.example.app
→ module name

com.example.Main
→ main class
```

---

# 22. Classpath vs Module Path

| Feature | Classpath | Module Path |
|---|---|---|
| Main unit | Classes/JARs | Modules |
| Explicit dependencies | No module declarations | Yes |
| Strong encapsulation | No module-level encapsulation | Yes |
| `module-info.java` | Not required | Used by named modules |
| JPMS-aware | No | Yes |
| Legacy compatibility | Strong | Supports modular applications |

Classpath remains an important part of Java.

The module system did not eliminate the classpath.

---

# 23. Strong Encapsulation

One of the major goals of modules is stronger encapsulation.

Suppose:

```text
com.example.library
```

contains:

```text
com.example.api
com.example.internal
```

Module declaration:

```java
module com.example.library {

    exports com.example.api;
}
```

Then:

```text
api
→ public module API

internal
→ module implementation detail
```

Even if a class in `internal` is declared:

```java
public
```

it is not automatically accessible to another module because the package is not exported.

---

# 24. Module Readability

For module-level access, there are two important concepts:

```text
Readability
+
Accessibility
```

Suppose:

```text
A requires B
```

Then:

```text
A can read B
```

But the required package must also be exported for normal cross-module access.

Conceptually:

```text
A
 |
 | requires
 ↓
B
 |
 | exports
 ↓
Package
```

Both relationships matter.

---

# 25. Package Accessibility

Suppose module B contains:

```text
com.example.api
com.example.internal
```

and:

```java
module B {

    exports com.example.api;
}
```

Module A:

```java
module A {

    requires B;
}
```

A can access exported packages:

```text
com.example.api
```

but not:

```text
com.example.internal
```

unless additional mechanisms such as qualified exports or reflective opening apply.

---

# 26. Public Does Not Always Mean Accessible

This is a very common interview trap.

Suppose:

```java
package com.example.internal;

public class Secret {

}
```

The class is:

```text
public
```

But if its package is not exported by the module:

```java
module com.example.library {

}
```

another named module cannot normally access it.

Therefore:

```text
public
→ Java language-level visibility

exports
→ module-level package accessibility
```

Both layers matter.

---

# 27. Split Packages

A split package occurs when the same package is present in multiple modules.

For example:

```text
Module A
└── com.example.common

Module B
└── com.example.common
```

This creates ambiguity in the module system.

JPMS strongly discourages and generally prevents split packages across modules in the same module graph.

Best practice:

```text
One package
→ one module
```

---

# 28. Cyclic Dependencies

Module dependencies should form a valid module graph.

For example:

```text
Module A
   |
   ↓
Module B
   |
   ↓
Module A
```

This creates a cycle.

JPMS does not allow cycles in the module dependency graph.

If:

```java
module A {

    requires B;
}
```

and:

```java
module B {

    requires A;
}
```

the module graph is invalid.

The dependency structure should be designed as a directed acyclic graph.

---

# 29. Root Modules

When the Java runtime resolves modules, it starts from one or more root modules and resolves their dependencies.

For example:

```text
Application
    |
    ↓
Root Module
    |
    +── requires A
            |
            +── requires B
```

The module system resolves the required modules to construct the observable module graph.

This is particularly important when using tools such as:

```text
java
jlink
jdeps
```

---

# 30. `jlink`

`jlink` creates a custom runtime image containing selected Java modules and application modules.

Example concept:

```text
Full JDK
   |
   ↓
Selected modules
   |
   ↓
Custom runtime image
```

This can reduce the size of a deployed runtime compared with shipping an entire JDK.

Example command:

```text
jlink --module-path mods:$JAVA_HOME/jmods \
      --add-modules com.example.app \
      --output my-runtime
```

The exact module path syntax differs across operating systems.

---

# 31. `jdeps`

`jdeps` is a Java dependency analysis tool.

It can help identify dependencies of:

```text
Classes
JAR files
Modules
```

Example:

```text
jdeps myapp.jar
```

It is useful during modularization because it can help identify dependencies that need to be represented in module declarations.

---

# 32. `jar` and Modules

The `jar` tool can package modular applications.

A modular JAR commonly contains:

```text
module-info.class
```

Example structure:

```text
myapp.jar
│
├── module-info.class
│
└── com/
    └── example/
        └── Main.class
```

The module descriptor tells Java how the JAR participates in the module graph.

---

# 33. Module Naming

Common naming convention:

```text
com.company.application
```

Examples:

```text
com.example.app
com.company.payment
org.example.library
```

Module names are identifiers, not package names in the strict sense, although reverse-domain naming is common.

Avoid unnecessary names that are likely to conflict with other modules.

---

# 34. Module Graph

Modules and their dependencies form a graph.

Example:

```text
             Application
                  |
          +-------+-------+
          ↓               ↓
       Service          Database
          |
          ↓
        Common
```

Each:

```text
requires
```

relationship creates a readability edge.

The JVM can resolve the graph before running a modular application.

---

# 35. Reflection and Modules

Reflection interacts with module boundaries.

Suppose a framework needs deep reflective access to:

```text
com.example.model
```

The module may use:

```java
opens com.example.model;
```

or:

```java
opens com.example.model
    to framework.module;
```

Important distinction:

```text
exports
→ normal Java access


opens
→ reflective access
```

A package can be opened without being exported.

This is important for frameworks that inspect or modify fields reflectively.

---

# 36. Advantages

## 1. Strong Encapsulation

Modules can hide implementation packages.

---

## 2. Explicit Dependencies

Dependencies are declared using:

```text
requires
```

---

## 3. Better Maintainability

Large applications can be divided into clear module boundaries.

---

## 4. Reliable Configuration

The module graph makes dependencies explicit.

---

## 5. Smaller Runtime Images

`jlink` can build custom runtime images.

---

## 6. Better Dependency Analysis

Tools such as:

```text
jdeps
```

help analyze dependencies.

---

## 7. Service-Based Architecture

Modules support:

```text
uses
provides ... with
```

and `ServiceLoader`.

---

# 37. Limitations

## 1. Migration Complexity

Adding modules to a large existing application can require significant restructuring.

---

## 2. Framework Compatibility

Libraries that rely heavily on reflection may need packages to be opened.

---

## 3. More Configuration

Projects gain:

```text
module-info.java
requires
exports
opens
```

and related configuration.

---

## 4. Classpath and Module Path Complexity

Developers may need to understand both models during migration.

---

## 5. Legacy Libraries

Older libraries may not provide explicit module descriptors.

Automatic modules can help with migration, but they are not identical to carefully designed explicit modules.

---

# 38. Common Mistakes

## ❌ Mistake 1 — Confusing Package with Module

Remember:

```text
Package
→ classes

Module
→ packages + dependencies + boundaries
```

---

## ❌ Mistake 2 — Thinking `public` Automatically Makes a Class Available

A package must also be accessible through the module system.

---

## ❌ Mistake 3 — Confusing `exports` and `opens`

```text
exports
→ normal access

opens
→ reflection
```

---

## ❌ Mistake 4 — Thinking `requires` Exports a Package

No.

```text
requires
→ dependency/readability

exports
→ package accessibility
```

---

## ❌ Mistake 5 — Thinking JAR and Module Are the Same

A JAR is a packaging format.

A module is a dependency and encapsulation unit.

---

## ❌ Mistake 6 — Forgetting `module-info.java`

An explicit named module needs a module descriptor.

---

## ❌ Mistake 7 — Assuming Java 9 Removed Classpath

It did not.

Classpath is still supported.

---

## ❌ Mistake 8 — Thinking All JARs Are Named Modules

A JAR can be:

```text
Explicit modular JAR
Automatic module
Classpath JAR
```

---

## ❌ Mistake 9 — Creating Cyclic Module Dependencies

Module dependencies must form a valid acyclic graph.

---

# 39. Top 25 Interview Questions

## 🔥 Q1. What is a Java module?

A named, self-describing collection of packages that declares dependencies and package boundaries.

---

## 🔥 Q2. When was JPMS introduced?

Java 9.

---

## 🔥 Q3. What is JPMS?

Java Platform Module System.

---

## 🔥 Q4. What is `module-info.java`?

The source file containing the module declaration and its directives.

---

## 🔥 Q5. What does `requires` mean?

It declares that one module depends on and can read another module.

---

## 🔥 Q6. What does `exports` mean?

It makes a package available for normal access by other modules.

---

## 🔥 Q7. What does `opens` mean?

It allows reflective access to a package.

---

## 🔥 Q8. What is `requires transitive`?

It makes a required module readable through the requiring module's API dependency relationship.

---

## 🔥 Q9. What is `requires static`?

It declares an optional compile-time dependency.

---

## 🔥 Q10. What is a qualified export?

An export restricted to specific target modules.

Example:

```java
exports com.example.api
    to com.example.client;
```

---

## 🔥 Q11. What is `uses`?

It declares that a module consumes a service.

---

## 🔥 Q12. What is `provides ... with`?

It declares a service implementation provided by a module.

---

## 🔥 Q13. What is an automatic module?

A non-modular JAR placed on the module path and treated as a module by JPMS.

---

## 🔥 Q14. What is the unnamed module?

Classpath code is associated with the unnamed module.

---

## 🔥 Q15. What is a modular JAR?

A JAR containing a module descriptor, typically represented by:

```text
module-info.class
```

---

## 🔥 Q16. What is the difference between classpath and module path?

Classpath operates with traditional class/JAR loading, while the module path participates in JPMS module resolution and encapsulation.

---

## 🔥 Q17. Does `public` override module boundaries?

No.

A public type in a non-exported package is not normally accessible across named-module boundaries.

---

## 🔥 Q18. Can one module contain multiple packages?

Yes.

---

## 🔥 Q19. Can two modules contain the same package?

This creates a split-package problem and is generally not allowed in a valid module graph.

---

## 🔥 Q20. Can modules have cyclic dependencies?

No. The module dependency graph must be acyclic.

---

## 🔥 Q21. What is `jlink`?

A tool that creates custom runtime images from selected modules.

---

## 🔥 Q22. What is `jdeps`?

A dependency analysis tool for Java classes, JARs, and modules.

---

## 🔥 Q23. What is strong encapsulation?

The ability of modules to hide non-exported packages from other modules.

---

## 🔥 Q24. Why is `opens` useful for frameworks?

Frameworks can use reflection to access members of opened packages even when those packages are not normally exported.

---

## 🔥 Q25. What is the biggest conceptual difference between packages and modules?

```text
Package
→ organizes classes

Module
→ organizes packages
  + declares dependencies
  + controls package boundaries
```

---

# 40. 30-Second Interview Answer

> Java's module system, introduced in Java 9, provides a higher-level unit of organization above packages. A module is declared using `module-info.java` and can explicitly define dependencies with `requires`, expose packages with `exports`, and allow reflective access with `opens`. Modules provide stronger encapsulation and more reliable dependency configuration than the traditional classpath model. JPMS also supports service providers through `uses` and `provides`, while tools such as `jlink` can create custom runtime images and `jdeps` can analyze dependencies.

---

# 41. Cheat Sheet

```text
========================================================
                    JAVA MODULES
========================================================


INTRODUCED
--------------------------------------------------------

Java 9


JPMS
--------------------------------------------------------

Java Platform Module System


========================================================

MODULE
--------------------------------------------------------

Module
→ named unit
→ contains packages
→ declares dependencies
→ controls exports
→ controls reflective opening


========================================================

MODULE DESCRIPTOR
--------------------------------------------------------

module-info.java


Compiled:

module-info.class


========================================================

BASIC
--------------------------------------------------------

module com.example.app {

}


========================================================

REQUIRES
--------------------------------------------------------

requires B;


Meaning:

A
 |
 ↓
B


A can read B.


========================================================

EXPORTS
--------------------------------------------------------

exports com.example.api;


Meaning:

Package
→ normal cross-module access


========================================================

OPENS
--------------------------------------------------------

opens com.example.model;


Meaning:

Package
→ reflective access


========================================================

TRANSITIVE
--------------------------------------------------------

requires transitive B;


Meaning:

Dependency is exposed
through the module's
readability relationship.


========================================================

STATIC
--------------------------------------------------------

requires static B;


Meaning:

Compile-time dependency
that can be optional at runtime.


========================================================

QUALIFIED EXPORT
--------------------------------------------------------

exports com.example.api
    to com.example.client;


Only selected module
gets normal access.


========================================================

QUALIFIED OPEN
--------------------------------------------------------

opens com.example.model
    to framework.module;


Only selected module
gets reflective access.


========================================================

SERVICE
--------------------------------------------------------

uses Service;


Provider:

provides Service
    with Implementation;


========================================================

MODULE TYPES
--------------------------------------------------------

Explicit Named Module
→ module-info.java


Automatic Module
→ non-modular JAR on module path


Unnamed Module
→ classpath code


========================================================

JAR
--------------------------------------------------------

JAR
→ packaging format


Modular JAR
→ JAR containing module descriptor


========================================================

CLASS PATH
--------------------------------------------------------

Traditional Java loading


MODULE PATH
--------------------------------------------------------

JPMS-aware module loading


========================================================

ENCAPSULATION
--------------------------------------------------------

public class
        +
exported package
        ↓
cross-module access


public class
        +
non-exported package
        ↓
not normally accessible
from another named module


========================================================

TOOLS
--------------------------------------------------------

jlink
→ custom runtime image


jdeps
→ dependency analysis


jar
→ packaging


========================================================

SPLIT PACKAGE
--------------------------------------------------------

Same package
inside multiple modules


→ problematic / generally prohibited


========================================================

CYCLES
--------------------------------------------------------

A → B → A


→ invalid module dependency graph


========================================================

KEY DIFFERENCE
--------------------------------------------------------

requires
→ dependency


exports
→ normal access


opens
→ reflection


uses
→ consumes service


provides
→ supplies service


========================================================
```

---

# 42. Final Mental Model

```text
                         JAVA MODULE SYSTEM
                                |
                                ↓
                              Module
                                |
              +-----------------+-----------------+
              |                 |                 |
              ↓                 ↓                 ↓
           Packages        Dependencies       Boundaries
                                |                 |
                                ↓                 ↓
                            requires          exports
                                                |
                                                ↓
                                              opens
                                           (reflection)


========================================================

MODULE STRUCTURE


Module
 |
 +── module-info.java
 |
 +── Package
 |     |
 |     +── Class
 |     +── Interface
 |
 +── Package
       |
       +── Class


========================================================

DEPENDENCY


Module A
   |
   | requires
   ↓
Module B


A can read B.


========================================================

NORMAL ACCESS


Module A
   |
   | requires
   ↓
Module B
   |
   | exports
   ↓
Package
   |
   ↓
Classes


========================================================

REFLECTION


Module B
   |
   | opens
   ↓
Package
   |
   ↓
Reflective access


========================================================

SERVICE MODEL


Consumer
   |
   | uses
   ↓
Service Interface
   ↑
   |
   | provides ... with
   |
Provider


========================================================

THREE MODULE MODELS


                 Java Code
                    |
          +---------+---------+
          |                   |
          ↓                   ↓
      Module Path         Classpath
          |                   |
          ↓                   ↓
    Named Modules       Unnamed Module
          |
      +---+---+
      |       |
      ↓       ↓
   Explicit Automatic


========================================================

PUBLIC vs EXPORTS


public
  ↓
Java access modifier


exports
  ↓
Module boundary


Both can matter.


========================================================

MOST IMPORTANT INTERVIEW LINE


"JPMS adds a module-level boundary
above packages, allowing Java applications
to explicitly declare dependencies and
control which packages are accessible."


========================================================

SECOND MOST IMPORTANT


requires
→ dependency

exports
→ normal access

opens
→ reflection

uses
→ service consumer

provides
→ service provider


========================================================

FINAL MEMORY MAP


                    MODULE
                       |
        +--------------+--------------+
        |              |              |
        ↓              ↓              ↓
    requires        exports         opens
        |              |              |
        ↓              ↓              ↓
   dependency      normal API     reflection


                       |
                       +----------------+
                       |                |
                       ↓                ↓
                     uses          provides
                       |                |
                       ↓                ↓
                  consumer          provider


========================================================

ONE-LINE MEMORY


"Module = Packages + Dependencies
       + Encapsulation + Services."


========================================================
```

---

# 🏁 Final Revision Checklist

- [ ] Definition of Java module
- [ ] JPMS
- [ ] Java 9 introduction
- [ ] Module vs package
- [ ] Module vs JAR
- [ ] `module-info.java`
- [ ] Basic module declaration
- [ ] `requires`
- [ ] `exports`
- [ ] `opens`
- [ ] `requires transitive`
- [ ] `requires static`
- [ ] Qualified exports
- [ ] Qualified opens
- [ ] `uses`
- [ ] `provides ... with`
- [ ] ServiceLoader concept
- [ ] Named modules
- [ ] Automatic modules
- [ ] Unnamed module
- [ ] Module path
- [ ] Classpath
- [ ] Strong encapsulation
- [ ] Module readability
- [ ] Package accessibility
- [ ] `public` vs `exports`
- [ ] Split packages
- [ ] Cyclic dependencies
- [ ] Module graph
- [ ] Root modules
- [ ] Reflection and `opens`
- [ ] `jlink`
- [ ] `jdeps`
- [ ] Modular JAR
- [ ] Advantages
- [ ] Limitations
- [ ] Common mistakes
- [ ] Interview questions

---

# 🧠 One-Line Memory

```text
Java Module
= Named collection of packages
+ Explicit dependencies
+ Controlled exports
+ Strong encapsulation

Java 9
→ JPMS

module-info.java
→ module declaration

requires
→ depends on

exports
→ normal access

opens
→ reflection

uses
→ consumes service

provides ... with
→ provides service

jlink
→ custom runtime

jdeps
→ dependency analysis
```
