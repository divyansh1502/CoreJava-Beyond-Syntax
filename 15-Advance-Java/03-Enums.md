
# 🔢 Enums in Java

> **An enum (enumeration) is a special Java type used to define a fixed set of named constants.**

---

# 📑 Table of Contents

- [1. What Is an Enum?](#1-what-is-an-enum)
- [2. Why Use Enums?](#2-why-use-enums)
- [3. Basic Enum Syntax](#3-basic-enum-syntax)
- [4. Creating an Enum](#4-creating-an-enum)
- [5. Using Enum Constants](#5-using-enum-constants)
- [6. Enum Variables](#6-enum-variables)
- [7. Enum in `switch`](#7-enum-in-switch)
- [8. `values()` Method](#8-values-method)
- [9. `valueOf()` Method](#9-valueof-method)
- [10. `name()` Method](#10-name-method)
- [11. `ordinal()` Method](#11-ordinal-method)
- [12. `toString()` in Enums](#12-tostring-in-enums)
- [13. Comparing Enum Constants](#13-comparing-enum-constants)
- [14. Enum with Fields](#14-enum-with-fields)
- [15. Enum Constructors](#15-enum-constructors)
- [16. Enum Methods](#16-enum-methods)
- [17. Enum with Constructor and Method](#17-enum-with-constructor-and-method)
- [18. Enum with Abstract Methods](#18-enum-with-abstract-methods)
- [19. Enum Implementing an Interface](#19-enum-implementing-an-interface)
- [20. Enum vs Class](#20-enum-vs-class)
- [21. Enum vs Constants](#21-enum-vs-constants)
- [22. Enum and Type Safety](#22-enum-and-type-safety)
- [23. EnumSet](#23-enumset)
- [24. EnumMap](#24-enummap)
- [25. Enum as Singleton](#25-enum-as-singleton)
- [26. Enum Serialization](#26-enum-serialization)
- [27. Enum and `==`](#27-enum-and-)
- [28. Enum and `switch` Internals](#28-enum-and-switch-internals)
- [29. Enum Reflection](#29-enum-reflection)
- [30. Enum Methods from `Object`](#30-enum-methods-from-object)
- [31. Enum Limitations](#31-enum-limitations)
- [32. Common Mistakes](#32-common-mistakes)
- [33. Top 25 Interview Questions](#33-top-25-interview-questions)
- [34. 30-Second Interview Answer](#34-30-second-interview-answer)
- [35. Cheat Sheet](#35-cheat-sheet)
- [36. Final Mental Model](#36-final-mental-model)

---

# 1. What Is an Enum?

An enum is a special Java type used when a variable should contain one value from a predefined set.

Example:

```java
enum Day {
    MONDAY,
    TUESDAY,
    WEDNESDAY,
    THURSDAY,
    FRIDAY,
    SATURDAY,
    SUNDAY
}
```

Here:

```text
MONDAY
TUESDAY
WEDNESDAY
...
```

are enum constants.

The set of valid values is fixed.

---

# 2. Why Use Enums?

Suppose we need to represent days.

A poor approach would be:

```java
String day = "Monday";
```

This allows invalid values:

```java
String day = "Someday";
```

We could also use integers:

```java
int day = 1;
```

But:

```text
1 = ?
2 = ?
3 = ?
```

is not very readable.

An enum gives us:

```java
Day day =
    Day.MONDAY;
```

Benefits:

```text
Type safety
Readability
Fixed set of values
Better switch support
Useful methods
Works with EnumSet and EnumMap
```

---

# 3. Basic Enum Syntax

General syntax:

```java
enum EnumName {

    CONSTANT1,
    CONSTANT2,
    CONSTANT3
}
```

Example:

```java
enum Status {

    PENDING,
    APPROVED,
    REJECTED
}
```

---

# 4. Creating an Enum

Example:

```java
enum Direction {

    NORTH,
    SOUTH,
    EAST,
    WEST
}
```

Now we can declare:

```java
Direction direction =
    Direction.NORTH;
```

The variable can only contain values of type:

```text
Direction
```

---

# 5. Using Enum Constants

Each enum constant is accessed using:

```text
EnumType.CONSTANT
```

Example:

```java
enum TrafficLight {

    RED,
    YELLOW,
    GREEN
}
```

```java
TrafficLight light =
    TrafficLight.RED;
```

Another example:

```java
if (light ==
    TrafficLight.RED) {

    System.out.println(
        "Stop"
    );
}
```

---

# 6. Enum Variables

An enum variable is strongly typed.

Example:

```java
enum Size {

    SMALL,
    MEDIUM,
    LARGE
}
```

```java
Size size =
    Size.MEDIUM;
```

This is invalid:

```java
Size size =
    "MEDIUM";
```

Because:

```text
String
≠
Size
```

This type safety is one of the major advantages of enums.

---

# 7. Enum in `switch`

Enums work naturally with `switch`.

Example:

```java
enum Day {

    MONDAY,
    TUESDAY,
    WEDNESDAY
}
```

```java
Day day =
    Day.MONDAY;

switch (day) {

    case MONDAY:
        System.out.println(
            "Start of week"
        );
        break;

    case TUESDAY:
        System.out.println(
            "Tuesday"
        );
        break;

    case WEDNESDAY:
        System.out.println(
            "Wednesday"
        );
        break;
}
```

Inside the switch, we normally write:

```text
MONDAY
```

rather than:

```text
Day.MONDAY
```

---

# 8. `values()` Method

Every enum type has a compiler-provided `values()` method.

It returns an array containing the enum constants in declaration order.

Example:

```java
enum Day {

    MONDAY,
    TUESDAY,
    WEDNESDAY
}
```

```java
Day[] days =
    Day.values();

for (Day day :
        days) {

    System.out.println(
        day
    );
}
```

Output:

```text
MONDAY
TUESDAY
WEDNESDAY
```

Important:

```text
values()
→ all enum constants
→ declaration order
```

---

# 9. `valueOf()` Method

Every enum type has a static `valueOf(String)` method.

It converts an exact enum constant name into its enum object.

Example:

```java
enum Status {

    PENDING,
    APPROVED,
    REJECTED
}
```

```java
Status status =
    Status.valueOf(
        "APPROVED"
    );
```

Result:

```text
Status.APPROVED
```

The name must match exactly.

This:

```java
Status.valueOf(
    "approved"
);
```

throws:

```text
IllegalArgumentException
```

because enum constant names are case-sensitive.

---

# 10. `name()` Method

`name()` returns the exact name of the enum constant as declared.

Example:

```java
enum Status {

    PENDING,
    APPROVED
}
```

```java
Status status =
    Status.APPROVED;

System.out.println(
    status.name()
);
```

Output:

```text
APPROVED
```

Important:

```text
name()
→ exact declared constant name
```

---

# 11. `ordinal()` Method

`ordinal()` returns the zero-based position of an enum constant in its declaration.

Example:

```java
enum Size {

    SMALL,
    MEDIUM,
    LARGE
}
```

```java
System.out.println(
    Size.SMALL.ordinal()
);
```

Output:

```text
0
```

```java
System.out.println(
    Size.MEDIUM.ordinal()
);
```

Output:

```text
1
```

```java
System.out.println(
    Size.LARGE.ordinal()
);
```

Output:

```text
2
```

Important:

```text
ordinal()
→ declaration position
→ starts at 0
```

### ⚠️ Important Warning

Do not use `ordinal()` as a permanent business value.

Bad:

```java
int databaseValue =
    status.ordinal();
```

Why?

If the enum changes:

```java
enum Status {

    PENDING,
    CANCELLED,
    APPROVED
}
```

the ordinal values change.

Use explicit fields when a stable business value is required.

---

# 12. `toString()` in Enums

By default, enum `toString()` returns the enum constant name.

Example:

```java
enum Status {

    PENDING,
    APPROVED
}
```

```java
System.out.println(
    Status.PENDING.toString()
);
```

Output:

```text
PENDING
```

However, `toString()` can be overridden.

Example:

```java
enum Status {

    PENDING,
    APPROVED;

    @Override
    public String toString() {

        return name()
            .toLowerCase();
    }
}
```

Now:

```java
System.out.println(
    Status.PENDING
);
```

Output:

```text
pending
```

Important difference:

```text
name()
→ fixed declared name

toString()
→ can be customized
```

---

# 13. Comparing Enum Constants

Enum constants can be compared using:

```text
==
```

Example:

```java
enum Status {

    PENDING,
    APPROVED
}
```

```java
Status status =
    Status.APPROVED;

if (status ==
    Status.APPROVED) {

    System.out.println(
        "Approved"
    );
}
```

Using `==` is preferred for enum identity comparison.

Why?

Each enum constant is a specific enum instance.

---

# 14. Enum with Fields

Enums can contain fields.

Example:

```java
enum Status {

    PENDING(1),
    APPROVED(2),
    REJECTED(3);

    private final int code;

    Status(int code) {

        this.code = code;
    }

    public int getCode() {

        return code;
    }
}
```

Use:

```java
int code =
    Status.APPROVED.getCode();

System.out.println(
    code
);
```

Output:

```text
2
```

This is useful when enum constants need associated data.

---

# 15. Enum Constructors

Enums can have constructors.

Example:

```java
enum Level {

    LOW(1),
    MEDIUM(2),
    HIGH(3);

    private final int value;

    Level(int value) {

        this.value = value;
    }
}
```

Important:

Enum constructors are not called directly using `new`.

This is invalid:

```java
new Level(1);
```

Enum constants are created by the Java runtime as part of the enum type.

---

# 16. Enum Methods

Enums can contain methods just like classes.

Example:

```java
enum Operation {

    ADD,
    SUBTRACT;

    int calculate(
        int a,
        int b
    ) {

        if (this ==
            ADD) {

            return a + b;
        }

        return a - b;
    }
}
```

Use:

```java
int result =
    Operation.ADD.calculate(
        10,
        5
    );
```

Output:

```text
15
```

---

# 17. Enum with Constructor and Method

A common pattern is:

```text
Constant
 ↓
Constructor
 ↓
Field
 ↓
Method
```

Example:

```java
enum Currency {

    USD("$"),
    INR("₹"),
    EUR("€");

    private final String symbol;

    Currency(
        String symbol
    ) {

        this.symbol = symbol;
    }

    public String getSymbol() {

        return symbol;
    }
}
```

Use:

```java
System.out.println(
    Currency.INR.getSymbol()
);
```

Output:

```text
₹
```

This allows each constant to carry its own data.

---

# 18. Enum with Abstract Methods

Each enum constant can provide its own implementation of an abstract method.

Example:

```java
enum Operation {

    ADD {

        @Override
        int apply(
            int a,
            int b
        ) {

            return a + b;
        }
    },

    MULTIPLY {

        @Override
        int apply(
            int a,
            int b
        ) {

            return a * b;
        }
    };

    abstract int apply(
        int a,
        int b
    );
}
```

Use:

```java
System.out.println(
    Operation.ADD.apply(
        10,
        5
    )
);
```

Output:

```text
15
```

Here each enum constant has its own behavior.

---

# 19. Enum Implementing an Interface

An enum can implement interfaces.

Example:

```java
interface Describable {

    String describe();
}
```

```java
enum Status
    implements Describable {

    PENDING,
    APPROVED;

    @Override
    public String describe() {

        return name()
            .toLowerCase();
    }
}
```

Use:

```java
System.out.println(
    Status.APPROVED.describe()
);
```

Enums cannot extend another class because every enum implicitly extends:

```text
java.lang.Enum
```

But they can implement interfaces.

---

# 20. Enum vs Class

| Feature | Enum | Class |
|---|---|---|
| Fixed constants | Yes | Not inherently |
| `new` for instances | No | Yes |
| Extends another class | No | Yes, one class |
| Implements interfaces | Yes | Yes |
| Constructors | Yes, restricted | Yes |
| Fields | Yes | Yes |
| Methods | Yes | Yes |
| Type-safe fixed values | Excellent use case | Depends |

Example enum:

```java
enum Direction {

    NORTH,
    SOUTH
}
```

Example class:

```java
class Student {

    String name;
}
```

Use enum when the possible values form a fixed, well-defined set.

---

# 21. Enum vs Constants

Before enums, developers often used constants:

```java
class Status {

    public static final int
        PENDING = 1;

    public static final int
        APPROVED = 2;
}
```

Problem:

```java
int status =
    999;
```

The compiler allows it.

With enum:

```java
enum Status {

    PENDING,
    APPROVED
}
```

Now:

```java
Status status =
    Status.APPROVED;
```

A random integer cannot be assigned to `Status`.

Therefore:

```text
Constants
→ primitive-based
→ weaker type safety

Enum
→ dedicated type
→ stronger type safety
```

---

# 22. Enum and Type Safety

Suppose:

```java
enum PaymentStatus {

    PENDING,
    SUCCESS,
    FAILED
}
```

Method:

```java
void process(
    PaymentStatus status
) {
}
```

Valid:

```java
process(
    PaymentStatus.SUCCESS
);
```

Invalid:

```java
process(
    "SUCCESS"
);
```

Also invalid:

```java
process(
    1
);
```

The compiler protects the method from unrelated values.

---

# 23. EnumSet

`EnumSet` is a specialized `Set` implementation designed specifically for enum types.

Example:

```java
import java.util.EnumSet;

enum Permission {

    READ,
    WRITE,
    DELETE
}
```

```java
EnumSet<Permission> permissions =
    EnumSet.of(
        Permission.READ,
        Permission.WRITE
    );
```

Check:

```java
boolean canWrite =
    permissions.contains(
        Permission.WRITE
    );
```

`EnumSet` is generally more compact and efficient for enum elements than a general-purpose set implementation.

---

# 24. EnumMap

`EnumMap` is a specialized `Map` implementation where keys must be enum types.

Example:

```java
import java.util.EnumMap;

enum Day {

    MONDAY,
    TUESDAY,
    WEDNESDAY
}
```

```java
EnumMap<Day, String> schedule =
    new EnumMap<>(
        Day.class
    );
```

```java
schedule.put(
    Day.MONDAY,
    "Java"
);
```

Retrieve:

```java
System.out.println(
    schedule.get(
        Day.MONDAY
    )
);
```

`EnumMap` is optimized for enum keys.

---

# 25. Enum as Singleton

An enum can be used to implement a singleton.

Example:

```java
enum DatabaseConnection {

    INSTANCE;

    public void connect() {

        System.out.println(
            "Connected"
        );
    }
}
```

Use:

```java
DatabaseConnection.INSTANCE
    .connect();
```

Why is this useful?

The enum type has a fixed set of instances, and:

```text
INSTANCE
```

is one unique enum constant.

Java's enum serialization and instantiation rules also provide strong guarantees around enum constants.

---

# 26. Enum Serialization

Enums have special serialization behavior.

When an enum constant is serialized, Java preserves the enum constant identity rather than treating it like an ordinary serializable object whose state is reconstructed through constructors.

Example:

```java
enum Status {

    PENDING,
    APPROVED
}
```

If:

```java
Status status =
    Status.APPROVED;
```

is serialized and later deserialized, the resulting value refers to:

```text
Status.APPROVED
```

The enum constructor is not used to recreate a new enum constant during deserialization.

This is one reason enum singletons are often discussed as serialization-safe singleton implementations.

---

# 27. Enum and `==`

Enum constants are singleton instances within their enum type.

Therefore:

```java
if (status ==
    Status.APPROVED) {
}
```

is appropriate.

`==` compares object identity.

For enum constants, identity is exactly what we normally want.

You can use `equals()` too:

```java
if (status.equals(
    Status.APPROVED
)) {
}
```

But `==` is generally clearer for enum constants.

---

# 28. Enum and `switch` Internals

When switching on an enum, the compiler may generate implementation details such as a synthetic mapping structure to connect enum ordinals to switch cases.

Conceptually:

```text
Enum constant
    ↓
ordinal
    ↓
switch mapping
    ↓
case body
```

The exact bytecode representation is an implementation detail and should not be relied upon in application code.

The important programming-level fact is:

```text
switch supports enum types directly.
```

---

# 29. Enum Reflection

Enums can be inspected using reflection.

Example:

```java
enum Status {

    PENDING,
    APPROVED
}
```

```java
Class<Status> clazz =
    Status.class;

System.out.println(
    clazz.isEnum()
);
```

Output:

```text
true
```

Reflection can also inspect:

```text
enum constants
methods
fields
constructors
annotations
```

You can retrieve constants through:

```java
Status[] values =
    clazz.getEnumConstants();
```

---

# 30. Enum Methods from `Object`

Enums inherit behavior through their class hierarchy.

The important relationship is:

```text
java.lang.Enum
        ↑
     YourEnum
```

`Enum` itself provides important methods such as:

```text
name()
ordinal()
compareTo()
equals()
hashCode()
toString()
```

Some methods are final or have semantics that should not be changed.

---

# 31. Enum Limitations

## 1. Cannot Extend Another Class

An enum already extends:

```text
java.lang.Enum
```

Therefore:

```java
enum Status
    extends SomeClass {
}
```

is invalid.

---

## 2. Cannot Create Enum Objects with `new`

Invalid:

```java
new Status();
```

---

## 3. Fixed Set of Constants

Enums are designed for a known set of constants.

If values need to be dynamically created at runtime, a normal class may be more appropriate.

---

## 4. Enum Constants Are Fixed

You cannot dynamically add a new enum constant through normal Java language mechanisms.

---

# 32. Common Mistakes

## ❌ Mistake 1 — Using `ordinal()` as a Database ID

Bad:

```java
int id =
    Status.APPROVED.ordinal();
```

If constants are reordered, values change.

Use explicit fields instead.

---

## ❌ Mistake 2 — Comparing Enum Names with `==`

Wrong approach:

```java
if (status.name() ==
    "APPROVED") {
}
```

`name()` returns a `String`.

Use:

```java
if (status ==
    Status.APPROVED) {
}
```

or:

```java
if ("APPROVED".equals(
    status.name()
)) {
}
```

---

## ❌ Mistake 3 — Assuming `valueOf()` Is Case-Insensitive

This:

```java
Status.valueOf(
    "approved"
);
```

does not match:

```java
APPROVED
```

Enum names are case-sensitive.

---

## ❌ Mistake 4 — Forgetting `valueOf()` Can Throw

If the string does not match a constant:

```text
IllegalArgumentException
```

is thrown.

---

## ❌ Mistake 5 — Thinking Enum Constructors Are Public

Enum constructors cannot be invoked externally with `new`.

---

## ❌ Mistake 6 — Thinking Enum Cannot Have Methods

Enums can contain:

```text
Fields
Constructors
Methods
Abstract methods
Implementations
Interfaces
```

---

## ❌ Mistake 7 — Thinking Enum Can Extend a Class

It cannot because enum types already extend:

```text
java.lang.Enum
```

---

# 33. Top 25 Interview Questions

## 🔥 Q1. What is an enum?

An enum is a special Java type used to define a fixed set of named constants.

---

## 🔥 Q2. Why use enums instead of integers?

Enums provide:

```text
Type safety
Readability
Fixed valid values
Better API design
```

---

## 🔥 Q3. Can an enum have constructors?

Yes.

But enum constructors cannot be called directly with `new`.

---

## 🔥 Q4. Can an enum have methods?

Yes.

Enums can contain instance and static methods, fields, and constructors.

---

## 🔥 Q5. Can an enum implement an interface?

Yes.

Example:

```java
enum Status
    implements Runnable {
}
```

---

## 🔥 Q6. Can an enum extend another class?

No.

Every enum already extends:

```text
java.lang.Enum
```

---

## 🔥 Q7. What is `values()`?

It returns an array containing all enum constants in declaration order.

---

## 🔥 Q8. What is `valueOf()`?

It returns the enum constant whose name exactly matches the supplied string.

---

## 🔥 Q9. What does `ordinal()` return?

The zero-based declaration position of the enum constant.

---

## 🔥 Q10. Should `ordinal()` be used as a database ID?

Generally no.

Enum declaration changes can change ordinal values.

---

## 🔥 Q11. Can enum constants have fields?

Yes.

Example:

```java
enum Status {

    SUCCESS(200);

    private final int code;

    Status(int code) {

        this.code = code;
    }
}
```

---

## 🔥 Q12. Can each enum constant have different behavior?

Yes.

Enum constants can provide different implementations of an abstract method.

---

## 🔥 Q13. Can enum implement an interface?

Yes.

---

## 🔥 Q14. Can enum contain abstract methods?

Yes.

Each enum constant must provide an implementation if the enum declares an abstract method.

---

## 🔥 Q15. Why is `==` preferred for enum comparison?

Because each enum constant represents a unique instance within its enum type, and `==` directly compares identity.

---

## 🔥 Q16. What is `EnumSet`?

A specialized `Set` implementation optimized for enum elements.

---

## 🔥 Q17. What is `EnumMap`?

A specialized `Map` implementation optimized for enum keys.

---

## 🔥 Q18. Can an enum be serialized?

Yes.

Enums have special serialization semantics.

---

## 🔥 Q19. Why is enum useful for Singleton?

A singleton can be represented by one enum constant:

```java
enum Singleton {

    INSTANCE
}
```

The enum mechanism provides strong guarantees around the constant's identity and serialization.

---

## 🔥 Q20. What is the parent class of an enum?

Conceptually:

```text
java.lang.Enum
```

---

## 🔥 Q21. Are enum constants objects?

Yes.

Each enum constant is an instance of its enum type.

---

## 🔥 Q22. Can enum constants be created dynamically?

No, not through ordinary Java code.

---

## 🔥 Q23. Is `valueOf()` case-sensitive?

Yes.

---

## 🔥 Q24. Can an enum contain static members?

Yes.

Enums can contain static fields and methods subject to normal Java restrictions and initialization rules.

---

## 🔥 Q25. What is the difference between `name()` and `toString()`?

```text
name()
→ exact declared constant name

toString()
→ normally same by default
→ can be overridden
```

---

# 34. 30-Second Interview Answer

> An enum in Java is a special type used to represent a fixed set of named constants. Enums provide type safety and readability compared with integer or string constants. An enum can have fields, constructors, methods, abstract methods, and can implement interfaces, but it cannot extend another class because it already extends `java.lang.Enum`. Important methods include `values()`, `valueOf()`, `name()`, and `ordinal()`. Enums also work with specialized collections such as `EnumSet` and `EnumMap`, and a single enum constant can be used to implement a robust singleton pattern.

---

# 35. Cheat Sheet

```text
========================================================
                       JAVA ENUM
========================================================


CORE IDEA
--------------------------------------------------------

Enum
→ fixed set of named constants


========================================================

BASIC SYNTAX
--------------------------------------------------------

enum Status {

    PENDING,
    APPROVED,
    REJECTED
}


========================================================

USE
--------------------------------------------------------

Status status =
    Status.APPROVED;


========================================================

IMPORTANT METHODS
--------------------------------------------------------

values()
→ all constants


valueOf("APPROVED")
→ matching constant


name()
→ exact declared name


ordinal()
→ zero-based declaration position


toString()
→ normally name
→ can be overridden


========================================================

COMPARISON
--------------------------------------------------------

status ==
Status.APPROVED


Preferred for enum identity.


========================================================

ENUM FEATURES
--------------------------------------------------------

Fields
Constructors
Methods
Abstract methods
Interfaces
Annotations
Static members


========================================================

ENUM CANNOT
--------------------------------------------------------

Extend another class

Use new to create constants

Dynamically add constants


========================================================

INHERITANCE
--------------------------------------------------------

java.lang.Enum
       ↑
    MyEnum


Therefore:

enum
→ cannot extend another class


But:

enum
→ can implement interfaces


========================================================

ENUM WITH DATA
--------------------------------------------------------

enum Status {

    SUCCESS(200),
    ERROR(500);

    private final int code;

    Status(int code) {

        this.code = code;
    }
}


========================================================

ENUMSET
--------------------------------------------------------

EnumSet<Status>


Specialized Set
for enum values.


========================================================

ENUMMAP
--------------------------------------------------------

EnumMap<Status, String>


Specialized Map
with enum keys.


========================================================

SINGLETON
--------------------------------------------------------

enum Singleton {

    INSTANCE
}


Use:

Singleton.INSTANCE


========================================================

REFLECTION
--------------------------------------------------------

Status.class.isEnum()


Status.class.getEnumConstants()


========================================================

IMPORTANT WARNING
--------------------------------------------------------

Do not use:

ordinal()

as a permanent business/database ID.


Use explicit fields.


========================================================

STRING CONVERSION
--------------------------------------------------------

String
   ↓
valueOf()
   ↓
Enum


Enum
   ↓
name()
   ↓
String


========================================================
```

---

# 36. Final Mental Model

```text
                         ENUM
                          |
                          ↓
                Fixed Set of Values
                          |
        +-----------------+-----------------+
        |                 |                 |
        ↓                 ↓                 ↓
     Type Safe         Readable          Structured
        |                 |                 |
        ↓                 ↓                 ↓
   Status.APPROVED    PENDING           Fields
                                      Methods
                                      Constructors


========================================================

ENUM CONSTANT


Status.APPROVED
       |
       ↓
   Enum Object
       |
       +--------→ name()
       |
       +--------→ ordinal()
       |
       +--------→ toString()
       |
       +--------→ custom methods
       |
       +--------→ custom fields


========================================================

ENUM CREATION


enum Status {

    PENDING,
    APPROVED
}


Java creates the enum constants.


Status.PENDING
Status.APPROVED


You do NOT use:

new Status()


========================================================

ENUM DATA MODEL


Constant
   ↓
Constructor
   ↓
Fields
   ↓
Methods


Example:


APPROVED(200)
     ↓
code = 200
     ↓
getCode()
     ↓
200


========================================================

ENUM BEHAVIOR


ADD
 ↓
add implementation


SUBTRACT
 ↓
subtract implementation


MULTIPLY
 ↓
multiply implementation


Each constant can provide
its own implementation.


========================================================

ENUM vs CONSTANTS


Integer constants:

1
2
3


Problem:

Any int can be passed.


Enum:

PENDING
APPROVED
REJECTED


Only valid enum values
can be passed.


========================================================

ENUM COLLECTIONS


Enum
 |
 +------→ EnumSet
 |
 +------→ EnumMap


EnumSet
→ Set of enum values


EnumMap
→ Map with enum keys


========================================================

ENUM SINGLETON


Singleton
    ↓
Enum
    ↓
INSTANCE
    ↓
One enum constant


========================================================

MOST IMPORTANT INTERVIEW LINE


"An enum is a type-safe way to represent
a fixed set of related constants."


========================================================

FINAL MEMORY MAP


Enum
 |
 +-- Constants
 |
 +-- Fields
 |
 +-- Constructor
 |
 +-- Methods
 |
 +-- Abstract Methods
 |
 +-- Interfaces
 |
 +-- values()
 |
 +-- valueOf()
 |
 +-- name()
 |
 +-- ordinal()
 |
 +-- EnumSet
 |
 +-- EnumMap
 |
 +-- Singleton


========================================================
```

---

# 🏁 Final Revision Checklist

- [ ] What is an enum?
- [ ] Why use enums?
- [ ] Enum syntax
- [ ] Enum constants
- [ ] Enum variables
- [ ] Enum with `switch`
- [ ] `values()`
- [ ] `valueOf()`
- [ ] `name()`
- [ ] `ordinal()`
- [ ] `toString()`
- [ ] Comparing enums with `==`
- [ ] Enum fields
- [ ] Enum constructors
- [ ] Enum methods
- [ ] Enum abstract methods
- [ ] Enum implementing interfaces
- [ ] Enum vs class
- [ ] Enum vs constants
- [ ] Enum type safety
- [ ] `EnumSet`
- [ ] `EnumMap`
- [ ] Enum singleton
- [ ] Enum serialization
- [ ] Enum reflection
- [ ] Why enum cannot extend another class
- [ ] Why ordinal should not be used as a database ID
- [ ] `name()` vs `toString()`
- [ ] `values()` vs `valueOf()`

---

# 🧠 One-Line Memory

```text
Enum = Fixed + Type-Safe + Named Values

values()     → All
valueOf()    → Find
name()       → Exact name
ordinal()    → Position
==           → Compare

Enum
→ can have fields, constructors, methods, interfaces
→ cannot extend another class
→ cannot be created with new
```

---

