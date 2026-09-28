
# 🧾 Java Records

> **Records are a special kind of class introduced in Java 16 that provide a concise way to model immutable data-carrying objects by automatically generating common members such as accessors, a canonical constructor, `equals()`, `hashCode()`, and `toString()`.**

---

# 📑 Table of Contents

- [1. What Are Records?](#1-what-are-records)
- [2. Why Were Records Introduced?](#2-why-were-records-introduced)
- [3. Traditional POJO vs Record](#3-traditional-pojo-vs-record)
- [4. Basic Record Syntax](#4-basic-record-syntax)
- [5. Record Components](#5-record-components)
- [6. Automatically Generated Members](#6-automatically-generated-members)
- [7. Canonical Constructor](#7-canonical-constructor)
- [8. Compact Canonical Constructor](#8-compact-canonical-constructor)
- [9. Validation in Records](#9-validation-in-records)
- [10. Accessor Methods](#10-accessor-methods)
- [11. Record Immutability](#11-record-immutability)
- [12. Reference-Type Immutability Trap](#12-reference-type-immutability-trap)
- [13. Custom Methods in Records](#13-custom-methods-in-records)
- [14. Static Members in Records](#14-static-members-in-records)
- [15. Instance Fields in Records](#15-instance-fields-in-records)
- [16. Constructors in Records](#16-constructors-in-records)
- [17. Extending Classes with Records](#17-extending-classes-with-records)
- [18. Implementing Interfaces](#18-implementing-interfaces)
- [19. Generic Records](#19-generic-records)
- [20. Nested Records](#20-nested-records)
- [21. Local Records](#21-local-records)
- [22. Record Serialization](#22-record-serialization)
- [23. Records and equals()](#23-records-and-equals)
- [24. Records and hashCode()](#24-records-and-hashcode)
- [25. Records and toString()](#25-records-and-tostring)
- [26. Record Restrictions](#26-record-restrictions)
- [27. Record vs Class](#27-record-vs-class)
- [28. Record vs Lombok](#28-record-vs-lombok)
- [29. When Should You Use Records?](#29-when-should-you-use-records)
- [30. When Should You Avoid Records?](#30-when-should-you-avoid-records)
- [31. Common Mistakes](#31-common-mistakes)
- [32. Interview Questions](#32-interview-questions)
- [33. 30-Second Interview Answer](#33-30-second-interview-answer)
- [34. Cheat Sheet](#34-cheat-sheet)
- [35. Final Mental Model](#35-final-mental-model)

---

# 1. What Are Records?

A record is a special kind of class designed primarily to model data.

Example:

```java
record Student(
    int id,
    String name
) {
}
```

Creating an object:

```java
Student student =
    new Student(
        101,
        "Rahul"
    );
```

Accessing values:

```java
System.out.println(
    student.id()
);

System.out.println(
    student.name()
);
```

A record automatically provides several common members.

---

# 2. Why Were Records Introduced?

Before records, a simple data class could require a lot of boilerplate.

For example:

```java
class Student {

    private final int id;

    private final String name;

    public Student(
        int id,
        String name
    ) {
        this.id = id;
        this.name = name;
    }

    public int getId() {
        return id;
    }

    public String getName() {
        return name;
    }

    @Override
    public boolean equals(
        Object obj
    ) {
        // implementation
        return true;
    }

    @Override
    public int hashCode() {
        // implementation
        return 0;
    }

    @Override
    public String toString() {
        // implementation
        return "";
    }
}
```

For a simple data carrier, much of this code is repetitive.

A record reduces that boilerplate:

```java
record Student(
    int id,
    String name
) {
}
```

The main idea is:

```text
Less boilerplate
+
Clear data model
+
Value-oriented semantics
```

---

# 3. Traditional POJO vs Record

Traditional class:

```java
class Student {

    private final int id;

    private final String name;

    public Student(
        int id,
        String name
    ) {
        this.id = id;
        this.name = name;
    }

    public int getId() {
        return id;
    }

    public String getName() {
        return name;
    }
}
```

Record:

```java
record Student(
    int id,
    String name
) {
}
```

The record declaration communicates the data structure directly.

---

# 4. Basic Record Syntax

General syntax:

```java
record RecordName(
    Type field1,
    Type field2
) {
}
```

Example:

```java
record Employee(
    int id,
    String name,
    double salary
) {
}
```

Create object:

```java
Employee employee =
    new Employee(
        101,
        "Aman",
        50000
    );
```

---

# 5. Record Components

The values declared inside the record header are called:

```text
record components
```

Example:

```java
record Employee(
    int id,
    String name,
    double salary
) {
}
```

The components are:

```text
id
name
salary
```

These components determine the record's state and participate in its automatically generated members.

---

# 6. Automatically Generated Members

For a record:

```java
record Employee(
    int id,
    String name
) {
}
```

Java provides:

```text
Canonical constructor
Accessor methods
equals()
hashCode()
toString()
```

Conceptually:

```text
record Employee(int id, String name)

        ↓

constructor
accessors
equals()
hashCode()
toString()
```

---

## Generated Constructor

Conceptually:

```java
new Employee(
    int id,
    String name
);
```

---

## Generated Accessors

For:

```java
record Employee(
    int id,
    String name
) {
}
```

Java provides:

```java
employee.id();
```

and:

```java
employee.name();
```

Notice:

```text
id()
```

not:

```text
getId()
```

---

# 7. Canonical Constructor

Every record has a canonical constructor corresponding to all record components.

Example:

```java
record Student(
    int id,
    String name
) {
}
```

Conceptually, the canonical constructor is:

```java
Student(
    int id,
    String name
)
```

You can explicitly declare it.

```java
record Student(
    int id,
    String name
) {

    public Student(
        int id,
        String name
    ) {
        this.id = id;
        this.name = name;
    }
}
```

The constructor must correctly initialize the record components.

---

# 8. Compact Canonical Constructor

Records provide a shorter form called a:

```text
compact canonical constructor
```

Example:

```java
record Student(
    int id,
    String name
) {

    public Student {
        System.out.println(
            "Creating Student"
        );
    }
}
```

Notice there are no parameters in the constructor declaration.

Java provides the parameter list automatically.

---

## Why Use It?

It is especially useful for validation.

Example:

```java
record Student(
    int id,
    String name
) {

    public Student {

        if (id <= 0) {
            throw new IllegalArgumentException(
                "Invalid id"
            );
        }
    }
}
```

The compiler handles the normal component assignment.

---

# 9. Validation in Records

Records are useful for validating data at construction time.

Example:

```java
record User(
    String username,
    int age
) {

    public User {

        if (username == null ||
            username.isBlank()) {

            throw new IllegalArgumentException(
                "Username cannot be empty"
            );
        }

        if (age < 18) {
            throw new IllegalArgumentException(
                "Age must be at least 18"
            );
        }
    }
}
```

Now:

```java
User user =
    new User(
        "Aman",
        22
    );
```

The validation executes during construction.

---

# 10. Accessor Methods

Records do not use the traditional JavaBean getter naming convention by default.

For:

```java
record Employee(
    int id,
    String name
) {
}
```

Use:

```java
employee.id();
```

and:

```java
employee.name();
```

Not:

```java
employee.getId();
```

unless you explicitly create such a method.

---

## Why This Syntax?

The record component itself becomes part of the record's public API.

The accessor has the same name as the component.

```text
component
    ↓
id
    ↓
accessor
    ↓
id()
```

---

# 11. Record Immutability

Record components are final-like state components.

Example:

```java
record Student(
    int id,
    String name
) {
}
```

You cannot directly modify them after construction.

This is invalid:

```java
Student student =
    new Student(
        101,
        "Aman"
    );

student.id = 200;
```

There are no setters automatically generated.

---

## No Setter Methods

A record does not generate:

```text
setId()
setName()
```

This supports its immutable data-carrier design.

---

# 12. Reference-Type Immutability Trap

This is an important interview concept.

Records provide final references, but they do not automatically make referenced objects deeply immutable.

Example:

```java
record Student(
    String name,
    List<String> skills
) {
}
```

The `skills` reference cannot be reassigned through the record component.

However, the list itself may still be mutable.

Example:

```java
List<String> skills =
    new ArrayList<>();

skills.add("Java");

Student student =
    new Student(
        "Aman",
        skills
    );
```

The list can still be changed through the original reference:

```java
skills.add("Spring");
```

Or through the returned reference:

```java
student.skills().add("SQL");
```

Therefore:

```text
Record
≠
Deep immutability
```

---

## Defensive Copy

If you need stronger immutability, make a defensive copy.

Example:

```java
record Student(
    String name,
    List<String> skills
) {

    public Student {

        skills =
            List.copyOf(skills);
    }
}
```

Now the record stores an unmodifiable copy.

---

# 13. Custom Methods in Records

Records can contain instance methods.

Example:

```java
record Rectangle(
    double length,
    double width
) {

    public double area() {
        return length * width;
    }
}
```

Use:

```java
Rectangle rectangle =
    new Rectangle(
        10,
        5
    );

System.out.println(
    rectangle.area()
);
```

Output:

```text
50.0
```

A record is still a class and can contain behavior.

---

# 14. Static Members in Records

Records can contain static fields and static methods.

Example:

```java
record MathValue(
    int value
) {

    static final int MAX =
        100;

    static void info() {
        System.out.println(
            "MathValue record"
        );
    }
}
```

Call static method:

```java
MathValue.info();
```

---

# 15. Instance Fields in Records

A record cannot declare additional instance fields outside its record components.

This is invalid:

```java
record Student(
    int id,
    String name
) {

    private int age;
}
```

The record's instance state is defined through its components.

However, you can define:

```text
static fields
static methods
instance methods
constructors
nested types
```

---

# 16. Constructors in Records

Records can have constructors.

There are important forms.

---

## Canonical Constructor

It corresponds to all components.

```java
record Student(
    int id,
    String name
) {

    public Student(
        int id,
        String name
    ) {
        this.id = id;
        this.name = name;
    }
}
```

---

## Compact Canonical Constructor

```java
record Student(
    int id,
    String name
) {

    public Student {

        if (id <= 0) {
            throw new IllegalArgumentException();
        }
    }
}
```

---

## Non-Canonical Constructor

A record can also have another constructor, but it must delegate to another constructor.

Example:

```java
record Student(
    int id,
    String name
) {

    public Student(int id) {
        this(id, "Unknown");
    }
}
```

The non-canonical constructor delegates using:

```java
this(...)
```

---

# 17. Extending Classes with Records

A record cannot extend an arbitrary class.

This is invalid:

```java
class Person {
}
```

```java
record Student(
    int id
) extends Person {
}
```

Records implicitly extend:

```text
java.lang.Record
```

and cannot extend another class.

---

## Important

A record is still a class, but it has restricted inheritance.

Conceptually:

```text
Object
   ↓
Record
   ↓
YourRecord
```

---

# 18. Implementing Interfaces

Records can implement interfaces.

Example:

```java
interface Printable {

    void print();
}
```

```java
record Student(
    int id,
    String name
) implements Printable {

    @Override
    public void print() {

        System.out.println(
            id + " " + name
        );
    }
}
```

Use:

```java
Student student =
    new Student(
        101,
        "Aman"
    );

student.print();
```

This is useful when a record needs to participate in an existing abstraction.

---

# 19. Generic Records

Records can be generic.

Example:

```java
record Pair<T, U>(
    T first,
    U second
) {
}
```

Use:

```java
Pair<Integer, String> pair =
    new Pair<>(
        101,
        "Java"
    );
```

Access:

```java
System.out.println(
    pair.first()
);

System.out.println(
    pair.second()
);
```

Generic records are useful for reusable data containers.

---

# 20. Nested Records

A record can be declared inside another class.

Example:

```java
class University {

    record Student(
        int id,
        String name
    ) {
    }
}
```

Create:

```java
University.Student student =
    new University.Student(
        101,
        "Aman"
    );
```

A nested record is implicitly static.

Therefore, it does not require an instance of the enclosing class.

---

# 21. Local Records

Records can also be declared locally inside a method.

Example:

```java
public class Test {

    public static void main(
        String[] args
    ) {

        record Point(
            int x,
            int y
        ) {
        }

        Point point =
            new Point(
                10,
                20
            );

        System.out.println(
            point
        );
    }
}
```

Local records are useful when a small data structure is required only within a limited scope.

---

# 22. Record Serialization

Records can implement:

```text
Serializable
```

if required by an application.

Example:

```java
import java.io.Serializable;

record Student(
    int id,
    String name
) implements Serializable {
}
```

Record serialization has specific semantics compared with ordinary serializable classes, particularly around reconstruction through the canonical constructor.

For normal application development, records should be used with serialization intentionally rather than assuming they behave exactly like ordinary JavaBeans.

---

# 23. Records and equals()

Records automatically provide an `equals()` implementation based on their components.

Example:

```java
record Student(
    int id,
    String name
) {
}
```

Create two objects:

```java
Student first =
    new Student(
        101,
        "Aman"
    );

Student second =
    new Student(
        101,
        "Aman"
    );
```

Compare:

```java
System.out.println(
    first.equals(second)
);
```

Output:

```text
true
```

The record compares its component values according to the record's generated equality semantics.

---

# 24. Records and hashCode()

Records automatically provide a `hashCode()` based on their components.

Example:

```java
record Student(
    int id,
    String name
) {
}
```

Two equal records should produce equal hash codes.

Example:

```java
Student first =
    new Student(
        101,
        "Aman"
    );

Student second =
    new Student(
        101,
        "Aman"
    );

System.out.println(
    first.hashCode() ==
    second.hashCode()
);
```

Output:

```text
true
```

This makes records convenient as value-like objects in collections such as:

```text
HashSet
HashMap
```

---

# 25. Records and toString()

Records automatically provide a useful `toString()` implementation.

Example:

```java
record Student(
    int id,
    String name
) {
}
```

```java
Student student =
    new Student(
        101,
        "Aman"
    );

System.out.println(
    student
);
```

Typical output:

```text
Student[id=101, name=Aman]
```

This is much more useful than the default class representation.

---

# 26. Record Restrictions

Records have several restrictions.

## 1. Cannot extend another class

```text
Record → cannot extend custom class
```

---

## 2. Cannot declare additional instance fields

The state must be represented through record components.

---

## 3. Cannot have instance initializers

A record cannot use an ordinary instance initializer block.

Validation belongs in the canonical constructor.

---

## 4. Components are final-like

There are no automatically generated setters.

---

## 5. Records cannot be abstract

A record represents a concrete data carrier.

---

## 6. Records are implicitly final

A record cannot be extended.

---

# 27. Record vs Class

| Feature | Record | Normal Class |
|---|---|---|
| Purpose | Data carrier | General-purpose object |
| Boilerplate | Low | Can be high |
| Components | Declared in header | Usually fields |
| Accessors | Automatically generated | Usually written manually |
| Setters | Not generated | Can be created |
| `equals()` | Generated | Usually manual |
| `hashCode()` | Generated | Usually manual |
| `toString()` | Generated | Usually manual |
| Additional instance fields | Not allowed | Allowed |
| Extends custom class | No | Yes |
| Can implement interfaces | Yes | Yes |
| Can have methods | Yes | Yes |
| Can have static members | Yes | Yes |
| Mutable design | Not intended | Fully possible |
| Inheritance | Final | Can be inherited unless final |

---

# 28. Record vs Lombok

Before records, developers sometimes used Lombok to reduce boilerplate.

For example:

```java
@Data
class Student {

    private final int id;

    private final String name;
}
```

Records provide language-level support for a similar data-carrier use case:

```java
record Student(
    int id,
    String name
) {
}
```

Important difference:

```text
Lombok
→ library / annotation processing approach

Record
→ Java language feature
```

Records are not a universal replacement for classes or Lombok.

If an object needs extensive mutable state, inheritance, or complex lifecycle behavior, a normal class may be more appropriate.

---

# 29. When Should You Use Records?

Records are well suited for data-oriented objects such as:

```text
DTOs
API response models
Request objects
Database projection objects
Coordinates
Configuration values
Small immutable value objects
Keys
Results returned from methods
```

Example DTO:

```java
record UserResponse(
    long id,
    String name,
    String email
) {
}
```

This communicates the purpose clearly.

---

# 30. When Should You Avoid Records?

A normal class may be more suitable when the object requires:

```text
Mutable state
Setters
Complex lifecycle
Inheritance from a custom class
Many additional instance fields
Framework-specific requirements
Identity-oriented behavior
```

For example, an entity whose state changes frequently may be modeled using a normal class depending on the framework and architecture.

The key question is:

```text
Is this primarily a data carrier?
```

If yes, a record may fit well.

---

# 31. Common Mistakes

## ❌ Mistake 1 — Using getName()

For:

```java
record Student(
    String name
) {
}
```

Use:

```java
student.name();
```

not automatically:

```java
student.getName();
```

---

## ❌ Mistake 2 — Thinking Records Are Deeply Immutable

This:

```java
record Student(
    List<String> skills
) {
}
```

does not automatically make the list immutable.

Use defensive copies when required.

---

## ❌ Mistake 3 — Trying to Add an Instance Field

Invalid:

```java
record Student(
    int id
) {

    private String name;
}
```

Use another record component if the state belongs to the record.

---

## ❌ Mistake 4 — Trying to Extend a Class

Invalid:

```java
record Student(
    int id
) extends Person {
}
```

Records already extend `java.lang.Record`.

---

## ❌ Mistake 5 — Expecting Setters

Records do not automatically provide:

```text
setId()
setName()
```

Their design is centered around immutable state.

---

## ❌ Mistake 6 — Assuming Record Means No Methods

Records can have:

```text
instance methods
static methods
constructors
nested types
```

They are still classes.

---

## ❌ Mistake 7 — Using Records for Every Class

A record is not simply a shorter class syntax.

It expresses a particular design intention:

```text
data carrier
+
value-oriented semantics
+
restricted mutable state
```

---

# 32. Interview Questions

## 🔥 Q1. What is a record in Java?

A record is a special kind of class designed primarily for immutable data-carrying objects, with automatic generation of common members such as accessors, a canonical constructor, `equals()`, `hashCode()`, and `toString()`.

---

## 🔥 Q2. In which Java version were records finalized?

Records became a standard Java language feature in:

```text
Java 16
```

They were introduced earlier as a preview feature.

---

## 🔥 Q3. What problem do records solve?

They reduce boilerplate when creating classes whose primary purpose is to carry data.

---

## 🔥 Q4. What methods are automatically generated for records?

Common automatically generated members include:

```text
Canonical constructor
Accessor methods
equals()
hashCode()
toString()
```

---

## 🔥 Q5. What are record components?

The fields declared in the record header are called record components.

Example:

```java
record Student(
    int id,
    String name
) {
}
```

Here:

```text
id
name
```

are record components.

---

## 🔥 Q6. What are record accessor methods called?

They use the component name.

For:

```java
record Student(
    int id
) {
}
```

the accessor is:

```java
student.id();
```

not automatically:

```java
student.getId();
```

---

## 🔥 Q7. Are records immutable?

Records provide final-like component state and do not provide setters, but they do not guarantee deep immutability of referenced mutable objects.

---

## 🔥 Q8. Can a record have methods?

Yes.

Example:

```java
record Rectangle(
    int length,
    int width
) {

    int area() {
        return length * width;
    }
}
```

---

## 🔥 Q9. Can a record implement an interface?

Yes.

```java
record Student(
    int id
) implements Printable {
}
```

---

## 🔥 Q10. Can a record extend a class?

No.

A record implicitly extends `java.lang.Record` and cannot extend another class.

---

## 🔥 Q11. Can a record be extended?

No.

Records are implicitly final.

---

## 🔥 Q12. Can records have static fields?

Yes.

```java
record Student(
    int id
) {

    static int count;
}
```

---

## 🔥 Q13. Can records have instance fields other than components?

No.

Additional instance fields are not permitted.

---

## 🔥 Q14. What is a compact canonical constructor?

It is a shorter form of the canonical constructor used especially for validation or normalization.

Example:

```java
record Student(
    int id
) {

    public Student {
        if (id <= 0) {
            throw new IllegalArgumentException();
        }
    }
}
```

---

## 🔥 Q15. Can records have constructors?

Yes.

They can have canonical, compact canonical, and appropriate non-canonical constructors.

---

## 🔥 Q16. Can records have custom methods?

Yes.

They can contain instance and static methods.

---

## 🔥 Q17. Why are records useful as DTOs?

Because DTOs often primarily carry data between layers, and records provide concise declarations, accessors, equality, hash code, and string representation.

---

## 🔥 Q18. Are records the same as Lombok `@Data`?

No.

Records are a Java language feature, while Lombok is a library-based code-generation approach.

Their capabilities and design constraints differ.

---

## 🔥 Q19. Can a record be generic?

Yes.

Example:

```java
record Pair<T, U>(
    T first,
    U second
) {
}
```

---

## 🔥 Q20. Can records be nested?

Yes.

They can be nested inside classes and interfaces.

---

## 🔥 Q21. Can records be local?

Yes.

A record can be declared inside a method as a local record.

---

## 🔥 Q22. Why doesn't a record provide setters?

Because records are designed around fixed component state and value-oriented data representation.

---

## 🔥 Q23. What is the difference between a record and a POJO?

A POJO is a broad concept referring to a regular Java object without special framework requirements.

A record is a specific Java language construct with defined restrictions and automatically generated members.

---

## 🔥 Q24. Does a record automatically make a List immutable?

No.

The reference is part of the record's fixed state, but the referenced list can still be mutable.

---

## 🔥 Q25. Why can records be useful as HashMap keys?

Their generated `equals()` and `hashCode()` are based on their components, making value-based key semantics convenient.

---

# 33. 30-Second Interview Answer

> A record is a special Java class designed for data-carrying objects. It became a standard feature in Java 16 and significantly reduces boilerplate by automatically providing a canonical constructor, component accessors, `equals()`, `hashCode()`, and `toString()`. Record components form the object's state and cannot be reassigned after construction. Records can implement interfaces and contain methods, constructors, and static members, but they cannot extend another class, declare additional instance fields, or be extended themselves. Records are particularly useful for DTOs and small value-oriented objects, although they do not guarantee deep immutability when they contain mutable references.

---

# 34. Cheat Sheet

```text
===================== JAVA RECORDS =====================


INTRODUCED
---------------------------------------------------------

Preview:
Java 14

Standard:
Java 16


PURPOSE
---------------------------------------------------------

Data-carrying classes
        ↓
Less boilerplate
        ↓
Value-oriented design


BASIC SYNTAX
---------------------------------------------------------

record Student(
    int id,
    String name
) {
}


RECORD COMPONENTS
---------------------------------------------------------

id
name

These define the record's state.


AUTOMATIC MEMBERS
---------------------------------------------------------

Canonical constructor
Accessor methods
equals()
hashCode()
toString()


ACCESSORS
---------------------------------------------------------

student.id()

student.name()


NOT:

student.getId()


IMMUTABILITY
---------------------------------------------------------

No setters
Component state is fixed

BUT:

Record
≠
Deep immutability


MUTABLE REFERENCE
---------------------------------------------------------

record Student(
    List<String> skills
) {
}

List itself can still be mutable.


DEFENSIVE COPY
---------------------------------------------------------

List.copyOf(skills)


CAN HAVE
---------------------------------------------------------

Instance methods
Static methods
Constructors
Nested types
Interfaces
Generic parameters


CANNOT HAVE
---------------------------------------------------------

Additional instance fields
Custom superclass
Subclassing


INHERITANCE
---------------------------------------------------------

YourRecord
    ↓
java.lang.Record
    ↓
java.lang.Object


RECORD IS
---------------------------------------------------------

final
concrete
data-oriented


CAN IMPLEMENT INTERFACE
---------------------------------------------------------

record Student(
    int id
) implements Printable {
}


CAN BE GENERIC
---------------------------------------------------------

record Pair<T, U>(
    T first,
    U second
) {
}


CAN BE NESTED
---------------------------------------------------------

class Outer {

    record Inner(
        int value
    ) {
    }
}


CAN BE LOCAL
---------------------------------------------------------

void test() {

    record Point(
        int x,
        int y
    ) {
    }
}


CAN HAVE STATIC MEMBERS
---------------------------------------------------------

record Student(
    int id
) {

    static int count;
}


CAN HAVE CUSTOM METHODS
---------------------------------------------------------

record Rectangle(
    int length,
    int width
) {

    int area() {
        return length * width;
    }
}


CAN HAVE VALIDATION
---------------------------------------------------------

record Student(
    int id
) {

    public Student {

        if (id <= 0) {
            throw new IllegalArgumentException();
        }
    }
}


CANONICAL CONSTRUCTOR
---------------------------------------------------------

Matches all record components.


COMPACT CANONICAL CONSTRUCTOR
---------------------------------------------------------

public Student {

    // validation
}


EQUALS
---------------------------------------------------------

Based on record components.


HASHCODE
---------------------------------------------------------

Based on record components.


TOSTRING
---------------------------------------------------------

Readable representation:

Student[id=101, name=Aman]


GOOD USE CASES
---------------------------------------------------------

DTOs
API responses
API requests
Projections
Value objects
Coordinates
Small data carriers
Keys


NORMAL CLASS BETTER WHEN
---------------------------------------------------------

Mutable state
Setters
Custom inheritance
Complex lifecycle
Additional instance state
Identity-heavy behavior


RECORD VS CLASS
---------------------------------------------------------

Record
→ data carrier

Class
→ general-purpose object


RECORD VS LOMBOK
---------------------------------------------------------

Record
→ Java language feature

Lombok
→ library/code generation


IMPORTANT INTERVIEW LINE
---------------------------------------------------------

A record provides shallow immutability,
not automatic deep immutability.


========================================================
```

---

# 35. Final Mental Model

```text
                         RECORD
                           |
                           ↓
                    DATA CARRIER
                           |
             +-------------+-------------+
             |             |             |
          Component     Component     Component
             |             |             |
             +-------------+-------------+
                           |
                           ↓
                  AUTOMATIC MEMBERS
                           |
        +----------+-------+-------+----------+
        |          |               |          |
   Constructor  Accessors      equals()   hashCode()
                                     
                           +
                           |
                       toString()


====================================================

NORMAL CLASS

class Student {

    fields
    constructor
    getters
    setters
    equals
    hashCode
    toString
}


RECORD

record Student(
    int id,
    String name
) {
}


LESS BOILERPLATE
        ↓
CLEAR DATA MODEL


====================================================

RECORD STATE

record Student(
    int id,
    String name
) {
}


        ↓

id
name

        ↓

fixed component state


BUT


record Student(
    List<String> skills
) {
}


        ↓

reference is fixed

        BUT

List object may still be mutable


Therefore:


RECORD
   ↓
Shallow immutability
   ≠
Deep immutability


====================================================

CONSTRUCTOR MODEL

record Student(
    int id,
    String name
) {


    public Student {

        // validation
    }
}


        ↓

Compact Canonical Constructor


        ↓

Compiler handles normal
component initialization


====================================================

INHERITANCE

Object
  ↓
Record
  ↓
YourRecord


Cannot:

YourRecord extends Person


Can:

YourRecord implements Interface


====================================================

RECORD DECISION

Does the object mainly
represent data?

        |
       YES
        ↓
Consider Record


       NO
        ↓
Consider Normal Class


====================================================

INTERVIEW MEMORY TRICK

RECORD

R → Reduced boilerplate
E → Equals
C → Components
O → Object representation
R → Restricted inheritance
D → Data carrier


====================================================

CORE IDEA

A record is NOT:

"Just a shorter class."


It communicates:

"This type primarily represents
a fixed set of data."


====================================================
```

---

# 🏁 Final Takeaways

- Records are specialized classes for data-oriented objects.
- Records became a standard Java feature in Java 16.
- They significantly reduce boilerplate for simple data carriers.
- Record components are declared directly in the record header.
- Java automatically provides a canonical constructor.
- Java automatically provides component accessor methods.
- Accessors use component names such as `student.name()`.
- `equals()`, `hashCode()`, and `toString()` are automatically provided.
- Records do not provide setters.
- Records cannot declare additional instance fields.
- Records cannot extend custom classes.
- Records implicitly extend `java.lang.Record`.
- Records are implicitly final.
- Records can implement interfaces.
- Records can contain instance methods.
- Records can contain static methods and fields.
- Records can contain constructors.
- Compact canonical constructors are useful for validation and normalization.
- Records can be generic.
- Records can be nested or local.
- Records provide shallow immutability, not guaranteed deep immutability.
- Mutable reference components such as `List` may still be modified.
- Defensive copies can be used when stronger immutability is required.
- Records are particularly useful for DTOs, API models, projections, and value objects.
- A normal class is still preferable when mutable state, inheritance, or complex object lifecycle is required.
- Records and Lombok are different: records are a Java language feature, while Lombok is a library-based code-generation approach.

---

