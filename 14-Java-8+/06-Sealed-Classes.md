
# 🔒 Sealed Classes

> **Sealed classes and interfaces, finalized in Java 17, restrict which classes or interfaces are allowed to directly extend or implement them, giving developers controlled inheritance and stronger modeling of a type hierarchy.**

---

# 📑 Table of Contents

- [1. What Are Sealed Classes?](#1-what-are-sealed-classes)
- [2. Why Were Sealed Classes Introduced?](#2-why-were-sealed-classes-introduced)
- [3. Basic Syntax](#3-basic-syntax)
- [4. The `sealed` Keyword](#4-the-sealed-keyword)
- [5. The `permits` Keyword](#5-the-permits-keyword)
- [6. Sealed Classes Example](#6-sealed-classes-example)
- [7. Direct Subclasses](#7-direct-subclasses)
- [8. What Can a Permitted Subclass Be?](#8-what-can-a-permitted-subclass-be)
- [9. `final` Permitted Subclass](#9-final-permitted-subclass)
- [10. `sealed` Permitted Subclass](#10-sealed-permitted-subclass)
- [11. `non-sealed` Permitted Subclass](#11-non-sealed-permitted-subclass)
- [12. `sealed` vs `final` vs `non-sealed`](#12-sealed-vs-final-vs-non-sealed)
- [13. Sealed Interfaces](#13-sealed-interfaces)
- [14. Permitting Interfaces](#14-permitting-interfaces)
- [15. Multi-Level Sealed Hierarchies](#15-multi-level-sealed-hierarchies)
- [16. Same File Rule](#16-same-file-rule)
- [17. Package and Module Restrictions](#17-package-and-module-restrictions)
- [18. Direct vs Indirect Subclasses](#18-direct-vs-indirect-subclasses)
- [19. Sealed Classes and Abstract Classes](#19-sealed-classes-and-abstract-classes)
- [20. Sealed Classes and Records](#20-sealed-classes-and-records)
- [21. Sealed Classes and Pattern Matching](#21-sealed-classes-and-pattern-matching)
- [22. Exhaustive Pattern Matching](#22-exhaustive-pattern-matching)
- [23. Sealed Types with `switch`](#23-sealed-types-with-switch)
- [24. Benefits of Sealed Classes](#24-benefits-of-sealed-classes)
- [25. Limitations](#25-limitations)
- [26. Common Mistakes](#26-common-mistakes)
- [27. Sealed vs Abstract vs Final](#27-sealed-vs-abstract-vs-final)
- [28. Interview Questions](#28-interview-questions)
- [29. 30-Second Interview Answer](#29-30-second-interview-answer)
- [30. Cheat Sheet](#30-cheat-sheet)
- [31. Final Mental Model](#31-final-mental-model)

---

# 1. What Are Sealed Classes?

A sealed class is a class that explicitly controls which classes are allowed to extend it.

Example:

```java
sealed class Payment
    permits CardPayment, CashPayment {
}
```

Only:

```text
CardPayment
CashPayment
```

can directly extend `Payment`.

This prevents arbitrary classes from extending the sealed class.

---

# 2. Why Were Sealed Classes Introduced?

Traditional inheritance allows a class to be extended by almost any accessible class.

Example:

```java
class Payment {
}
```

Another class can extend it:

```java
class CardPayment extends Payment {
}
```

Another class can also extend it:

```java
class CryptoPayment extends Payment {
}
```

If the original designer wants to allow only specific implementations, ordinary inheritance does not directly provide that restriction.

Sealed classes solve this problem.

```text
Payment
   |
   +── CardPayment
   |
   +── CashPayment
```

The hierarchy becomes explicitly controlled.

---

# 3. Basic Syntax

General syntax:

```java
sealed class Parent
    permits Child1, Child2 {
}
```

Example:

```java
sealed class Vehicle
    permits Car, Bike {
}
```

Then:

```java
final class Car
    extends Vehicle {
}
```

```java
final class Bike
    extends Vehicle {
}
```

Now only the permitted classes can directly extend `Vehicle`.

---

# 4. The `sealed` Keyword

The `sealed` keyword declares that a class or interface has restricted inheritance.

Example:

```java
sealed class Shape
    permits Circle, Rectangle {
}
```

The class says:

```text
I control my direct subclasses.
```

The permitted subclasses must explicitly choose how their own inheritance should continue.

They must be declared as one of:

```text
final
sealed
non-sealed
```

---

# 5. The `permits` Keyword

The `permits` clause specifies which classes or interfaces are allowed to directly extend or implement the sealed type.

Example:

```java
sealed class Payment
    permits CardPayment, CashPayment {
}
```

Here:

```text
Payment
   |
   +── CardPayment
   |
   +── CashPayment
```

Only those classes are permitted direct subclasses.

---

# 6. Sealed Classes Example

Let's create a payment hierarchy.

```java
sealed class Payment
    permits CardPayment, CashPayment {
}
```

Allowed:

```java
final class CardPayment
    extends Payment {
}
```

Allowed:

```java
final class CashPayment
    extends Payment {
}
```

Not allowed:

```java
class CryptoPayment
    extends Payment {
}
```

The last class causes a compilation error because it is not listed in `permits`.

---

# 7. Direct Subclasses

The `permits` list controls:

```text
direct subclasses
```

For example:

```java
sealed class Animal
    permits Dog, Cat {
}
```

Here:

```text
Animal
   |
   +── Dog
   |
   +── Cat
```

Only `Dog` and `Cat` can directly extend `Animal`.

---

# 8. What Can a Permitted Subclass Be?

A permitted direct subclass must explicitly declare one of:

```text
final
sealed
non-sealed
```

Example:

```java
sealed class Animal
    permits Dog, Cat {
}
```

`Dog` can be:

```java
final class Dog
    extends Animal {
}
```

or:

```java
sealed class Dog
    extends Animal
    permits Labrador, Beagle {
}
```

or:

```java
non-sealed class Dog
    extends Animal {
}
```

---

# 9. `final` Permitted Subclass

A `final` subclass cannot be extended further.

Example:

```java
sealed class Animal
    permits Dog, Cat {
}
```

```java
final class Dog
    extends Animal {
}
```

Now:

```text
Animal
   |
   +── Dog
```

No class can extend `Dog`.

---

# 10. `sealed` Permitted Subclass

A permitted subclass can itself be sealed.

Example:

```java
sealed class Animal
    permits Dog, Cat {
}
```

```java
sealed class Dog
    extends Animal
    permits Labrador, Beagle {
}
```

Now:

```text
Animal
   |
   +── Dog
        |
        +── Labrador
        |
        +── Beagle
   |
   +── Cat
```

The hierarchy can therefore have multiple controlled levels.

---

# 11. `non-sealed` Permitted Subclass

A permitted subclass can use:

```text
non-sealed
```

This removes inheritance restrictions from that point downward.

Example:

```java
sealed class Animal
    permits Dog, Cat {
}
```

```java
non-sealed class Dog
    extends Animal {
}
```

Now any accessible class can extend `Dog`.

Example:

```java
class Labrador
    extends Dog {
}
```

The hierarchy becomes:

```text
Animal
   |
   +── Dog
        |
        +── Labrador
        +── Beagle
        +── ...
```

The sealed restriction stops at `Dog`.

---

# 12. `sealed` vs `final` vs `non-sealed`

These three keywords are extremely important.

```text
sealed
```

means:

```text
Only explicitly permitted subclasses.
```

```text
final
```

means:

```text
No subclasses allowed.
```

```text
non-sealed
```

means:

```text
Inheritance restrictions are removed.
```

Visual model:

```text
sealed
   |
   +── controlled inheritance


final
   |
   +── no inheritance


non-sealed
   |
   +── unrestricted inheritance
```

---

# 13. Sealed Interfaces

Interfaces can also be sealed.

Example:

```java
sealed interface Payment
    permits CardPayment, CashPayment {
}
```

Classes can implement it:

```java
final class CardPayment
    implements Payment {
}
```

```java
final class CashPayment
    implements Payment {
}
```

The interface controls which classes can directly implement it.

---

# 14. Permitting Interfaces

A sealed interface can permit another interface.

Example:

```java
sealed interface Payment
    permits OnlinePayment, OfflinePayment {
}
```

Then:

```java
sealed interface OnlinePayment
    extends Payment
    permits CardPayment {
}
```

And:

```java
final class CardPayment
    implements OnlinePayment {
}
```

Hierarchy:

```text
Payment
   |
   +── OnlinePayment
          |
          +── CardPayment
```

---

# 15. Multi-Level Sealed Hierarchies

Sealed types can create controlled multi-level hierarchies.

Example:

```java
sealed class Shape
    permits Circle, Quadrilateral {
}
```

```java
final class Circle
    extends Shape {
}
```

```java
sealed class Quadrilateral
    extends Shape
    permits Rectangle, Square {
}
```

```java
final class Rectangle
    extends Quadrilateral {
}
```

```java
final class Square
    extends Quadrilateral {
}
```

The hierarchy is:

```text
Shape
 |
 +── Circle
 |
 +── Quadrilateral
       |
       +── Rectangle
       |
       +── Square
```

Every level can define its own inheritance policy.

---

# 16. Same File Rule

If permitted subclasses are declared in the same source file, the `permits` clause can sometimes be omitted because the compiler can infer the permitted direct subclasses.

Example:

```java
sealed class Shape {
}

final class Circle
    extends Shape {
}
```

```java
final class Rectangle
    extends Shape {
}
```

The compiler can determine the direct subclasses when they are declared in the same compilation context.

For clarity, many developers still explicitly write `permits` when it helps communicate the intended hierarchy.

---

# 17. Package and Module Restrictions

Permitted subclasses must satisfy Java's accessibility and sealed-type rules.

For a named module, the permitted direct subclasses must be in the same module as the sealed type.

For an unnamed module, the permitted direct subclasses must generally be in the same package.

This prevents a sealed hierarchy from being spread arbitrarily across unrelated modules or packages.

---

# 18. Direct vs Indirect Subclasses

The `permits` clause controls direct subclasses.

Example:

```java
sealed class Animal
    permits Dog {
}
```

```java
non-sealed class Dog
    extends Animal {
}
```

Then:

```java
class Labrador
    extends Dog {
}
```

`Labrador` is not required to appear in:

```java
permits
```

because it is not a direct subclass of `Animal`.

Hierarchy:

```text
Animal
   |
   +── Dog
        |
        +── Labrador
```

The restriction is defined at each level.

---

# 19. Sealed Classes and Abstract Classes

A sealed class can also be abstract.

Example:

```java
sealed abstract class Shape
    permits Circle, Rectangle {
}
```

Then:

```java
final class Circle
    extends Shape {
}
```

```java
final class Rectangle
    extends Shape {
}
```

This combines:

```text
abstract
+
controlled inheritance
```

The abstract class cannot be instantiated directly.

---

# 20. Sealed Classes and Records

Records are implicitly final.

Therefore, records work naturally as leaf types in sealed hierarchies.

Example:

```java
sealed interface Shape
    permits Circle, Rectangle {
}
```

```java
record Circle(
    double radius
) implements Shape {
}
```

```java
record Rectangle(
    double width,
    double height
) implements Shape {
}
```

Because records are implicitly final, they cannot have further subclasses.

Hierarchy:

```text
Shape
 |
 +── Circle
 |
 +── Rectangle
```

This is especially useful for modeling a finite set of data-oriented variants.

---

# 21. Sealed Classes and Pattern Matching

Sealed types become particularly useful with pattern matching.

Suppose:

```java
sealed interface Shape
    permits Circle, Rectangle {
}
```

```java
record Circle(
    double radius
) implements Shape {
}
```

```java
record Rectangle(
    double width,
    double height
) implements Shape {
}
```

A program knows the permitted hierarchy:

```text
Shape
 |
 +── Circle
 |
 +── Rectangle
```

This makes exhaustive processing possible.

---

# 22. Exhaustive Pattern Matching

Because the compiler knows the permitted direct subclasses, it can determine whether all variants have been handled in certain pattern-matching contexts.

Example:

```java
static double area(
    Shape shape
) {

    return switch (shape) {

        case Circle c ->
            Math.PI *
            c.radius() *
            c.radius();

        case Rectangle r ->
            r.width() *
            r.height();
    };
}
```

The `switch` handles all permitted implementations.

No `default` is required in this example when the compiler can establish exhaustiveness from the sealed hierarchy.

---

# 23. Sealed Types with `switch`

Pattern matching with sealed types can produce clean type-based logic.

Example:

```java
sealed interface Payment
    permits CardPayment, CashPayment {
}
```

```java
record CardPayment(
    double amount
) implements Payment {
}
```

```java
record CashPayment(
    double amount
) implements Payment {
}
```

Then:

```java
static String process(
    Payment payment
) {

    return switch (payment) {

        case CardPayment c ->
            "Card: " + c.amount();

        case CashPayment c ->
            "Cash: " + c.amount();
    };
}
```

The compiler can reason about the permitted type hierarchy.

---

# 24. Benefits of Sealed Classes

## 1. Controlled inheritance

The author explicitly defines which types can directly extend or implement the sealed type.

---

## 2. Clear domain modeling

A domain with a known set of variants can be represented explicitly.

Example:

```text
Payment
 |
 +── Card
 +── Cash
 +── UPI
```

---

## 3. Better compiler knowledge

The compiler knows more about the possible type hierarchy.

This is useful for:

```text
pattern matching
switch
exhaustiveness checking
```

---

## 4. Prevents unintended extension

A library or API can prevent arbitrary classes from becoming part of a hierarchy.

---

## 5. Works well with records

Records provide convenient final leaf types.

---

## 6. More expressive design

The type hierarchy itself communicates the intended architecture.

---

# 25. Limitations

Sealed classes also introduce restrictions.

## 1. Limited extensibility

External code cannot freely add new direct subclasses.

---

## 2. More design decisions

The hierarchy must be planned.

---

## 3. Coupling between parent and permitted types

The sealed type explicitly knows its direct permitted subclasses.

---

## 4. Not suitable for every abstraction

Open extension points should generally remain normal classes or interfaces.

---

# 26. Common Mistakes

## ❌ Mistake 1 — Forgetting `final`, `sealed`, or `non-sealed`

This is invalid:

```java
sealed class Animal
    permits Dog {
}
```

```java
class Dog
    extends Animal {
}
```

The direct permitted subclass must declare an appropriate inheritance modifier.

Correct:

```java
final class Dog
    extends Animal {
}
```

---

## ❌ Mistake 2 — Extending a Sealed Class Without Permission

Invalid:

```java
sealed class Animal
    permits Dog {
}
```

```java
final class Cat
    extends Animal {
}
```

`Cat` is not permitted.

---

## ❌ Mistake 3 — Thinking `non-sealed` Means Final

It means exactly the opposite regarding further inheritance.

```text
non-sealed
→ inheritance is open
```

---

## ❌ Mistake 4 — Thinking `permits` Lists Every Descendant

It lists direct permitted subclasses or implementations.

Indirect descendants are controlled by their immediate parent.

---

## ❌ Mistake 5 — Thinking Sealed Means Abstract

A sealed class can be concrete.

Example:

```java
sealed class Payment
    permits CardPayment {
}
```

Sealed and abstract are separate concepts.

---

## ❌ Mistake 6 — Thinking Sealed Means Immutable

Sealed classes control inheritance.

They do not automatically make object state immutable.

```text
sealed
≠
immutable
```

---

## ❌ Mistake 7 — Thinking Records Need `final`

A record is already implicitly final.

This is unnecessary:

```java
final record Student(
    int id
) {
}
```

Use:

```java
record Student(
    int id
) {
}
```

---

# 27. Sealed vs Abstract vs Final

These keywords solve different problems.

| Keyword | Main Purpose |
|---|---|
| `abstract` | Prevent direct instantiation and define incomplete behavior |
| `final` | Prevent inheritance |
| `sealed` | Restrict inheritance to specific permitted types |
| `non-sealed` | Reopen inheritance below a sealed type |

Example:

```text
abstract
    ↓
"Cannot create object directly"


final
    ↓
"Cannot extend me"


sealed
    ↓
"Only these types can extend me"


non-sealed
    ↓
"Anyone can extend me from here"
```

---

# 28. Interview Questions

## 🔥 Q1. What is a sealed class?

A sealed class is a class that restricts which classes can directly extend it.

---

## 🔥 Q2. In which Java version were sealed classes finalized?

Sealed classes and interfaces became standard in:

```text
Java 17
```

They were introduced earlier as a preview feature.

---

## 🔥 Q3. Which keywords are associated with sealed classes?

The important keywords are:

```text
sealed
permits
non-sealed
```

Permitted subclasses must also choose:

```text
final
sealed
non-sealed
```

---

## 🔥 Q4. What does `permits` do?

It specifies the classes or interfaces that are allowed to directly extend or implement the sealed type.

---

## 🔥 Q5. Can a sealed class have an indirect subclass that isn't in `permits`?

Yes.

`permits` controls direct subclasses.

For example, if:

```text
Animal
   ↓
non-sealed Dog
   ↓
Labrador
```

then `Labrador` does not need to appear in `Animal`'s permits list.

---

## 🔥 Q6. What is `non-sealed`?

It removes the sealed inheritance restriction from that subclass downward.

---

## 🔥 Q7. What happens if a permitted subclass doesn't specify `final`, `sealed`, or `non-sealed`?

It results in a compilation error because a direct permitted subclass must specify how inheritance continues.

---

## 🔥 Q8. Can an interface be sealed?

Yes.

```java
sealed interface Payment
    permits CardPayment, CashPayment {
}
```

---

## 🔥 Q9. Can a sealed class be abstract?

Yes.

```java
sealed abstract class Shape
    permits Circle, Rectangle {
}
```

---

## 🔥 Q10. Can a sealed class be final?

No.

A class cannot meaningfully be both:

```text
sealed
+
final
```

A sealed class exists to control its subclasses, while a final class prohibits all subclasses.

---

## 🔥 Q11. Can a permitted subclass be final?

Yes.

```java
final class Circle
    extends Shape {
}
```

This makes it a leaf in the hierarchy.

---

## 🔥 Q12. Can a permitted subclass be sealed?

Yes.

```java
sealed class Dog
    extends Animal
    permits Labrador {
}
```

---

## 🔥 Q13. Can a permitted subclass be non-sealed?

Yes.

```java
non-sealed class Dog
    extends Animal {
}
```

This reopens inheritance.

---

## 🔥 Q14. Can a record extend a sealed class?

A record cannot extend a class other than `java.lang.Record`, so it cannot extend a sealed class.

However, a record can implement a sealed interface.

---

## 🔥 Q15. Why do records work well with sealed interfaces?

Records are implicitly final, so they naturally become leaf implementations in a sealed hierarchy.

---

## 🔥 Q16. Are sealed classes immutable?

No.

Sealed controls inheritance, not object mutability.

---

## 🔥 Q17. What is the benefit of sealed classes with pattern matching?

The compiler can know the permitted type hierarchy, which can support exhaustive pattern matching and `switch` expressions.

---

## 🔥 Q18. What is the difference between `final` and `sealed`?

```text
final
→ no subclass allowed

sealed
→ only specified subclasses allowed
```

---

## 🔥 Q19. What is the difference between `sealed` and `non-sealed`?

```text
sealed
→ restricted inheritance

non-sealed
→ unrestricted inheritance from that point
```

---

## 🔥 Q20. Can a sealed class implement an interface?

Yes.

A sealed class can implement interfaces like any other class.

---

## 🔥 Q21. Can a sealed interface permit another interface?

Yes.

A permitted interface can itself be `sealed`, `non-sealed`, or otherwise appropriately declared according to the inheritance hierarchy.

---

## 🔥 Q22. Why would you use a sealed class instead of an abstract class?

An abstract class can be extended by permitted accessible classes.

A sealed abstract class adds an explicit restriction on which classes may directly extend it.

---

## 🔥 Q23. Does `permits` allow arbitrary classes?

No.

Only the explicitly permitted direct subclasses or implementations are allowed.

---

## 🔥 Q24. What happens if an unauthorized class tries to extend a sealed class?

The program fails to compile.

---

## 🔥 Q25. What is the main design idea behind sealed classes?

Controlled inheritance.

The type designer explicitly defines the legal direct members of a hierarchy.

---

# 29. 30-Second Interview Answer

> Sealed classes and interfaces, finalized in Java 17, allow developers to restrict which classes can directly extend or implement a type. A sealed type uses the `permits` clause to define its allowed direct subclasses. Each permitted subclass must declare whether it is `final`, `sealed`, or `non-sealed`. `final` stops inheritance, `sealed` continues controlled inheritance, and `non-sealed` reopens inheritance. Sealed types are particularly useful for modeling a known set of domain variants and work well with records and pattern matching because the compiler can reason about the permitted hierarchy.

---

# 30. Cheat Sheet

```text
=================== SEALED CLASSES =====================


STANDARD VERSION
---------------------------------------------------------

Java 17


PURPOSE
---------------------------------------------------------

Controlled inheritance


BASIC SYNTAX
---------------------------------------------------------

sealed class Shape
    permits Circle, Rectangle {
}


DIRECT SUBCLASSES
---------------------------------------------------------

Shape
 |
 +── Circle
 |
 +── Rectangle


PERMITTED SUBCLASS
---------------------------------------------------------

Must declare:

final
OR
sealed
OR
non-sealed


FINAL
---------------------------------------------------------

final class Circle
    extends Shape {
}


Meaning:

No further inheritance


SEALED
---------------------------------------------------------

sealed class Shape
    permits Circle, Rectangle {
}


Meaning:

Controlled inheritance continues


NON-SEALED
---------------------------------------------------------

non-sealed class Circle
    extends Shape {
}


Meaning:

Inheritance opens again


PERMITS
---------------------------------------------------------

Controls direct subclasses


IMPORTANT:

permits
≠
all descendants


SEALED INTERFACE
---------------------------------------------------------

sealed interface Payment
    permits Card, Cash {
}


RECORD + SEALED INTERFACE
---------------------------------------------------------

sealed interface Shape
    permits Circle, Rectangle {
}


record Circle(
    double radius
) implements Shape {
}


record Rectangle(
    double width,
    double height
) implements Shape {
}


RECORDS
---------------------------------------------------------

Records are implicitly final.

Therefore they work naturally
as leaf implementations.


ABSTRACT + SEALED
---------------------------------------------------------

sealed abstract class Shape
    permits Circle, Rectangle {
}


ABSTRACT
→ no direct objects

SEALED
→ controlled subclasses


FINAL VS SEALED
---------------------------------------------------------

final
→ zero subclasses

sealed
→ specific subclasses


SEALED VS NON-SEALED
---------------------------------------------------------

sealed
→ restricted

non-sealed
→ open


INHERITANCE MODEL
---------------------------------------------------------

Sealed
   |
   +── final
   |
   +── sealed
   |
   +── non-sealed


PATTERN MATCHING
---------------------------------------------------------

sealed hierarchy
        ↓
compiler knows variants
        ↓
pattern matching
        ↓
exhaustiveness


SAME FILE
---------------------------------------------------------

When permitted types are declared
in the same source context,
the compiler can infer permitted
direct subclasses in applicable cases.


SEALED ≠ IMMUTABLE
---------------------------------------------------------

sealed
→ controls inheritance

immutable
→ controls state changes


SEALED ≠ ABSTRACT
---------------------------------------------------------

sealed
→ controls inheritance

abstract
→ controls instantiation /
   incomplete implementation


========================================================
```

---

# 31. Final Mental Model

```text
                    SEALED TYPE
                         |
              "I control my children"
                         |
          +--------------+--------------+
          |              |              |
        final          sealed       non-sealed
          |              |              |
          ↓              ↓              ↓
      STOP HERE     CONTROL MORE     OPEN AGAIN


====================================================

EXAMPLE

sealed interface Payment
    permits CardPayment,
            CashPayment,
            UpiPayment;


              Payment
                 |
       +---------+---------+
       |         |         |
      Card      Cash       UPI
       |         |         |
     final     final     final


The hierarchy is known.


====================================================

MULTI-LEVEL

sealed class Animal
    permits Dog, Cat;


              Animal
              /    \
             /      \
           Dog       Cat
            |
         sealed
            |
       +----+----+
       |         |
     Lab       Beagle


Each level controls
its own children.


====================================================

NON-SEALED

sealed class Animal
    permits Dog;


             Animal
                |
          non-sealed Dog
                |
       +--------+--------+
       |        |        |
      Lab     Beagle    ...
       
Restriction stops at Dog.


====================================================

RECORD + SEALED

sealed interface Shape
    permits Circle, Rectangle;


          Shape
          /   \
         /     \
     Circle   Rectangle
       |          |
     record     record
       |          |
     final       final


Ideal for finite,
data-oriented variants.


====================================================

PATTERN MATCHING

sealed hierarchy
        |
        ↓
known possible types
        |
        ↓
switch / pattern matching
        |
        ↓
exhaustiveness checking


====================================================

MEMORY TRICK

SEALED
→ "Some children only"

FINAL
→ "No children"

NON-SEALED
→ "Anyone can be a child"

PERMITS
→ "Approved children"


====================================================

CORE IDEA

Normal inheritance:

"Anyone may extend me."


Sealed inheritance:

"Only these direct types
may extend me."


====================================================
```

---

# 🏁 Final Takeaways

- Sealed classes restrict direct inheritance.
- Sealed interfaces restrict direct implementations and extensions.
- Sealed types became a standard feature in Java 17.
- `permits` identifies the allowed direct subclasses or implementations.
- A permitted direct subclass must be `final`, `sealed`, or `non-sealed`.
- `final` stops inheritance.
- `sealed` continues controlled inheritance.
- `non-sealed` reopens inheritance.
- `permits` controls direct descendants, not every indirect descendant.
- A sealed class can also be abstract.
- A sealed class cannot be final.
- A sealed type is not automatically immutable.
- A sealed type does not have to be abstract.
- Records cannot extend sealed classes because records already extend `java.lang.Record`.
- Records can implement sealed interfaces and are naturally useful as final leaf types.
- Sealed hierarchies can contain multiple levels.
- Sealed types work particularly well with pattern matching.
- The compiler can use knowledge of a sealed hierarchy for exhaustiveness checking.
- Sealed classes are useful when a domain has a controlled and known set of variants.
- Use normal open inheritance when external extension is intentionally part of the design.

---

