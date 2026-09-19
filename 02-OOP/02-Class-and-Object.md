# ☕ Java OOP — Class & Object

> **A class defines a type by describing its state and behavior, while an object is a runtime instance of that class.**

---

# 📚 Table of Contents

* [1. What is a Class?](#-1-what-is-a-class)
* [2. Why Do We Need Classes?](#-2-why-do-we-need-classes)
* [3. Anatomy of a Class](#-3-anatomy-of-a-class)
* [4. What Can a Class Contain?](#-4-what-can-a-class-contain)
* [5. What is an Object?](#-5-what-is-an-object)
* [6. Creating an Object](#-6-creating-an-object)
* [7. What Does `new` Do?](#-7-what-does-new-do)
* [8. Reference Variable](#-8-reference-variable)
* [9. Object vs Reference](#-9-object-vs-reference)
* [10. State, Behavior & Identity](#-10-state-behavior--identity)
* [11. Multiple Objects](#-11-multiple-objects)
* [12. Multiple References to One Object](#-12-multiple-references-to-one-object)
* [13. `null` Reference](#-13-null-reference)
* [14. Object Creation Internally](#-14-object-creation-internally)
* [15. Stack vs Heap](#-15-stack-vs-heap)
* [16. Class Loading](#-16-class-loading)
* [17. Instance Variables](#-17-instance-variables)
* [18. Instance Methods](#-18-instance-methods)
* [19. Static vs Instance Members](#-19-static-vs-instance-members)
* [20. Accessing Object Members](#-20-accessing-object-members)
* [21. Anonymous Objects](#-21-anonymous-objects)
* [22. Objects as Method Arguments](#-22-objects-as-method-arguments)
* [23. Objects as Return Values](#-23-objects-as-return-values)
* [24. Object Assignment](#-24-object-assignment)
* [25. `==` with Objects](#-25--with-objects)
* [26. `equals()` with Objects](#-26-equals-with-objects)
* [27. Garbage Collection](#-27-garbage-collection)
* [28. Class vs Object](#-28-class-vs-object)
* [29. Common Mistakes](#-29-common-mistakes)
* [30. Interview Questions](#-30-interview-questions)
* [31. Top 10 Interview Questions](#-31-top-10-interview-questions)
* [32. Quick Revision](#-32-quick-revision)
* [33. 30-Second Interview Answer](#-33-30-second-interview-answer)
* [34. Memory Trick](#-34-memory-trick)

---

# 🧱 1. What is a Class?

A **class** is a user-defined reference type that defines the structure and behavior of objects.

A class can contain:

```text
Data
+
Behavior
```

For example:

```java
class Student {

    String name;
    int age;

    void study() {
        System.out.println("Student is studying");
    }
}
```

Here:

```text
Student
 ├── name
 ├── age
 └── study()
```

`Student` is a class.

It describes what a `Student` object can contain and do.

---

# 🤔 2. Why Do We Need Classes?

Imagine creating a student-management system.

Without classes, you might have:

```java
String student1Name;
int student1Age;

String student2Name;
int student2Age;

String student3Name;
int student3Age;
```

This becomes difficult to manage.

With a class:

```java
class Student {

    String name;
    int age;
}
```

we can create:

```java
Student s1 = new Student();
Student s2 = new Student();
Student s3 = new Student();
```

Now each object represents an individual student.

```text
Student Class
      │
      ├──────────────┐
      ▼              ▼
    Object          Object
      s1              s2
      │               │
 name = A         name = B
 age = 20         age = 21
```

### Core idea

> **A class gives structure; objects hold individual runtime state.**

---

# 🧩 3. Anatomy of a Class

A basic Java class:

```java
class Employee {

    String name;
    double salary;

    void work() {
        System.out.println(name + " is working");
    }
}
```

Break it down:

```text
class Employee
     │       │
     │       └── Class Name
     │
     └────────── Keyword
```

Inside:

```text
name
salary
```

are fields.

And:

```text
work()
```

is a method.

Conceptually:

```text
┌────────────────────────────┐
│        Employee            │
├────────────────────────────┤
│ Fields                     │
│  - name                    │
│  - salary                  │
├────────────────────────────┤
│ Methods                    │
│  - work()                  │
└────────────────────────────┘
```

---

# 📦 4. What Can a Class Contain?

A Java class can contain many kinds of declarations.

For example:

```java
class Employee {

    // Field
    String name;

    // Static field
    static String company = "ABC";

    // Constructor
    Employee(String name) {
        this.name = name;
    }

    // Instance method
    void work() {
        System.out.println("Working...");
    }

    // Static method
    static void companyInfo() {
        System.out.println(company);
    }

    // Nested class
    class Address {
    }
}
```

A class can contain:

```text
Fields
Methods
Constructors
Initializers
Nested classes/interfaces/enums
```

along with other supported declarations.

---

# 🧑‍💻 5. What is an Object?

An **object is an instance of a class**.

Suppose:

```java
class Car {

    String color;

    void drive() {
        System.out.println("Car is driving");
    }
}
```

Creating an object:

```java
Car c1 = new Car();
```

Here:

```text
Car
 ↑
Class / Type

c1
 ↑
Reference variable

new Car()
 ↑
New object instance
```

So:

```java
Car c1 = new Car();
```

contains three important concepts:

```text
Car       → Reference type
c1        → Reference variable
new Car() → Object creation expression
```

---

# 🏗️ 6. Creating an Object

The most common way to create an object is:

```java
ClassName reference = new ClassName();
```

Example:

```java
Student s = new Student();
```

Let's break it down.

### `Student`

```java
Student s
```

This declares a variable capable of holding a reference to a `Student` object.

### `s`

`s` is the reference variable.

### `new Student()`

This creates a new instance of `Student`.

---

# ⚙️ 7. What Does `new` Do?

The `new` operator is used to create a new object instance.

Example:

```java
Student s = new Student();
```

Conceptually:

```text
        new Student()
              │
              ▼
       ┌─────────────┐
       │ Student     │
       │ object      │
       ├─────────────┤
       │ name = null │
       │ age = 0     │
       └─────────────┘
              ▲
              │
              │ reference
              │
              s
```

During object creation, Java initializes the object's instance fields with their default values before constructor execution completes.

For example:

```java
class Student {

    String name;
    int age;
    boolean active;
}
```

Default values are conceptually:

```text
String   → null
int      → 0
boolean  → false
```

Then the constructor runs.

---

# 🔗 8. Reference Variable

This is one of the **most important concepts** in Java.

Consider:

```java
Student s = new Student();
```

Many beginners say:

> "`s` is the object."

Technically, that's not precise.

`s` is a **reference variable**.

The object is the instance created by:

```java
new Student()
```

Conceptually:

```text
Stack                         Heap

s ─────────────────────────► Student Object
                              ┌──────────────┐
                              │ name         │
                              │ age          │
                              └──────────────┘
```

The exact JVM implementation of references is not defined as literally "an address stored in stack," so the diagram is a useful conceptual model rather than a JVM specification guarantee.

---

# 🎯 9. Object vs Reference

Consider:

```java
Student s = new Student();
```

There are two different things:

### Reference

```java
s
```

### Object

```java
new Student()
```

Think of it like:

```text
Reference
   │
   │ points/references to
   ▼
Object
```

A reference can be changed:

```java
Student s1 = new Student();
Student s2 = new Student();

s1 = s2;
```

Now:

```text
s1 ──┐
     ├────► Object 2
s2 ──┘

Object 1
   ↑
   │
No reference
```

The first object may eventually become eligible for garbage collection if no other references exist.

---

# 🧠 10. State, Behavior & Identity

An object is commonly described using three characteristics.

## 1️⃣ State

State is represented by the current values of its fields.

```java
Student s = new Student();

s.name = "Rahul";
s.age = 21;
```

State:

```text
name = Rahul
age = 21
```

---

## 2️⃣ Behavior

Behavior is represented by methods.

```java
void study() {
    System.out.println("Studying...");
}
```

The object can perform:

```text
study()
```

---

## 3️⃣ Identity

Identity distinguishes one object instance from another.

```java
Student s1 = new Student();
Student s2 = new Student();
```

Even if:

```text
s1.name = "Rahul"
s2.name = "Rahul"
```

they can still be two separate objects.

---

# 👥 11. Multiple Objects

One class can create many objects.

```java
class Employee {

    String name;
    double salary;
}
```

Create:

```java
Employee e1 = new Employee();
Employee e2 = new Employee();
Employee e3 = new Employee();
```

Conceptually:

```text
                Employee Class
                      │
          ┌───────────┼───────────┐
          ▼           ▼           ▼
        e1 Object   e2 Object   e3 Object
        ┌───────┐   ┌───────┐   ┌───────┐
        │ name  │   │ name  │   │ name  │
        │ salary│   │ salary│   │ salary│
        └───────┘   └───────┘   └───────┘
```

Each object has its own instance state.

---

# 🔗 12. Multiple References to One Object

Java allows multiple reference variables to refer to the same object.

```java
Student s1 = new Student();

Student s2 = s1;
```

Now:

```text
s1 ─────┐
        │
        ▼
   ┌─────────────┐
   │ Student     │
   │ Object      │
   └─────────────┘
        ▲
        │
s2 ─────┘
```

If:

```java
s1.name = "Rahul";
```

then:

```java
System.out.println(s2.name);
```

prints:

```text
Rahul
```

Why?

Because both references refer to the **same object**.

---

# 🕳️ 13. `null` Reference

A reference variable can contain `null`.

```java
Student s = null;
```

This means:

```text
s
 │
 ▼
null
```

It does **not** mean an object was created.

For example:

```java
Student s = null;

s.study();
```

This causes:

```text
NullPointerException
```

because there is no object through which `study()` can be invoked.

---

# ⚙️ 14. Object Creation Internally

Consider:

```java
Student s = new Student();
```

A simplified conceptual sequence is:

```text
1. Student type is resolved/loaded if needed
             ↓
2. Memory for a new object is obtained
             ↓
3. Instance fields receive default values
             ↓
4. Constructor executes
             ↓
5. Reference to the object is produced
             ↓
6. Reference is assigned to s
```

Conceptually:

```text
                 new Student()
                      │
                      ▼
              Object allocation
                      │
                      ▼
              Default values
                      │
                      ▼
                Constructor
                      │
                      ▼
               Object reference
                      │
                      ▼
                      s
```

> ⚠️ This is a simplified model. JVM implementations can perform optimizations such as escape analysis and scalar replacement, so you should not treat every diagram as a literal physical memory layout.

---

# 🧠 15. Stack vs Heap

This is one of the most frequently asked Java interview topics.

Consider:

```java
Student s = new Student();
```

A simplified model:

```text
STACK                         HEAP

s ─────────────────────────► Student Object
                              ┌─────────────┐
                              │ name        │
                              │ age         │
                              └─────────────┘
```

### Stack

A thread's stack contains stack frames for method execution.

Local variables such as reference variables are associated with the relevant stack frame.

### Heap

Objects are generally allocated in the JVM heap.

The heap is the runtime memory area from which objects are allocated.

### Important distinction

```text
Reference variable ≠ Object
```

And:

```text
Stack ≠ "where all variables live"
Heap  ≠ "where everything else lives"
```

The JVM specification is more abstract than these simplified diagrams.

---

# 🧩 16. Class Loading

Before the JVM can use a class, its class information may need to be loaded.

A simplified view:

```text
.class bytecode
      ↓
Class Loading
      ↓
Linking
      ↓
Initialization
```

The JVM has class loaders responsible for loading classes.

Commonly discussed class loaders include:

```text
Bootstrap Class Loader
Platform Class Loader
Application/System Class Loader
```

This topic becomes important when studying:

* JVM
* ClassLoader
* static initialization
* reflection
* memory
* frameworks such as Spring

---

# 📦 17. Instance Variables

A variable declared inside a class but outside methods/constructors/blocks is a field.

When it is associated with each object, it is commonly called an **instance variable**.

Example:

```java
class Student {

    String name;
    int age;
}
```

Each object has its own instance state:

```java
Student s1 = new Student();
Student s2 = new Student();

s1.name = "Rahul";
s2.name = "Aman";
```

Conceptually:

```text
s1 → name = Rahul
s2 → name = Aman
```

Changing one does not automatically change the other.

---

# ⚙️ 18. Instance Methods

An instance method belongs to the object-level behavior of a class.

Example:

```java
class Student {

    String name;

    void study() {
        System.out.println(name + " is studying");
    }
}
```

Call:

```java
Student s = new Student();

s.name = "Rahul";
s.study();
```

Output:

```text
Rahul is studying
```

The method can access the instance state of the object on which it is invoked.

Conceptually:

```text
s.study()
   │
   ▼
study() executes for object s
```

---

# ⚡ 19. Static vs Instance Members

This distinction is extremely important.

### Instance member

Belongs conceptually to individual objects.

```java
class Student {

    String name;
}
```

Each object can have a different `name`.

---

### Static member

Belongs to the class rather than to individual object instances.

```java
class Student {

    static String college = "ABC College";
}
```

There is one class-level `college` field for the class rather than a separate independent value per object.

Conceptually:

```text
Student
  │
  └── static college

Objects
  ├── s1 → name
  ├── s2 → name
  └── s3 → name
```

---

# 🔍 20. Accessing Object Members

Use the dot operator:

```java
reference.member
```

Example:

```java
Student s = new Student();

s.name = "Rahul";
s.study();
```

Here:

```text
s.name
   ↑
instance field

s.study()
   ↑
instance method
```

---

# 👻 21. Anonymous Objects

An object does not always need to be assigned to a reference variable.

Example:

```java
new Student().study();
```

This creates an object and immediately invokes a method on it.

The object has no named reference variable in your code.

Another example:

```java
System.out.println(new Student().getName());
```

Such an object is commonly called an **anonymous object** in beginner-level Java terminology.

### When useful?

Mostly when the object is needed only once.

But repeatedly creating anonymous objects can make code harder to read.

---

# 📥 22. Objects as Method Arguments

Objects can be passed to methods.

Example:

```java
class Student {

    String name;
}

class Test {

    static void printStudent(Student s) {
        System.out.println(s.name);
    }

    public static void main(String[] args) {

        Student student = new Student();
        student.name = "Rahul";

        printStudent(student);
    }
}
```

Conceptually:

```text
student reference
       │
       ▼
Student Object
       │
       │ passed to method
       ▼
parameter s
```

### Important Java rule

Java is **always pass-by-value**.

For an object argument, the value being passed is the **reference value**.

This means the method receives a copy of the reference.

---

# 📤 23. Objects as Return Values

A method can return an object reference.

```java
class Student {

    String name;
}

class Test {

    static Student createStudent() {

        Student s = new Student();
        s.name = "Rahul";

        return s;
    }
}
```

Then:

```java
Student student = createStudent();
```

Conceptually:

```text
createStudent()
      │
      ▼
Student Object
      │
      ▼
reference returned
      │
      ▼
student
```

---

# 🔄 24. Object Assignment

Consider:

```java
Student s1 = new Student();
Student s2 = new Student();
```

Now:

```java
s1 = s2;
```

This does **not** copy the entire object.

It copies the reference value.

Before:

```text
s1 ──► Object A

s2 ──► Object B
```

After:

```text
s1 ──┐
     ├──► Object B
s2 ──┘

Object A
   ↑
No reference
```

If nothing else refers to Object A, it may become eligible for garbage collection.

---

# 🆚 25. `==` with Objects

When `==` compares object references, it checks whether the two references refer to the same object.

Example:

```java
Student s1 = new Student();
Student s2 = new Student();

System.out.println(s1 == s2);
```

Output:

```text
false
```

Why?

Because:

```text
s1 → Object A

s2 → Object B
```

Different objects.

Now:

```java
Student s1 = new Student();
Student s2 = s1;

System.out.println(s1 == s2);
```

Output:

```text
true
```

Because both references refer to the same object.

---

# 🟰 26. `equals()` with Objects

`equals()` is a method inherited from `Object`.

By default, `Object.equals()` uses reference identity semantics.

Classes can override `equals()` to define **logical equality**.

For example, two `Student` objects could be considered equal if they have the same student ID.

```java
Student s1 = new Student(101);
Student s2 = new Student(101);
```

With an appropriate `equals()` implementation:

```text
s1.equals(s2)
        ↓
      true
```

while:

```java
s1 == s2
```

may still be:

```text
false
```

because they are different object instances.

### Important

```text
==       → same reference/object identity
equals() → logical equality if the class defines it
```

---

# 🗑️ 27. Garbage Collection

Java automatically manages object memory through garbage collection.

Consider:

```java
Student s = new Student();

s = null;
```

If there are no other reachable references to the object, the object may become **eligible for garbage collection**.

```text
Before:

s ─────► Student Object


After:

s ─────► null

Student Object
      ↑
  unreachable
```

### Important interview point

Do not say:

> "Java immediately deletes the object."

Correct:

> **The object becomes eligible for garbage collection when it is no longer reachable, but the JVM determines when and how garbage collection occurs.**

---

# 🆚 28. Class vs Object

| Class                                      | Object                                     |
| ------------------------------------------ | ------------------------------------------ |
| Defines a type                             | Instance of a type                         |
| Describes possible state/behavior          | Has actual runtime state                   |
| Exists as program/runtime type information | Exists as an object instance at runtime    |
| Used to create instances                   | Created from a class/constructor mechanism |
| Does not represent one individual entity   | Represents an individual instance          |
| Example: `Student`                         | Example: `new Student()`                   |

### Simple memory trick

```text
CLASS  → Blueprint
OBJECT → Actual instance
```

---

# ⚠️ 29. Common Mistakes

## ❌ Mistake 1: Saying reference = object

Wrong:

```text
Student s = object
```

More precisely:

```text
s       → reference variable
new Student() → object instance
```

---

## ❌ Mistake 2: Thinking `new` creates the reference

The declaration:

```java
Student s;
```

declares a reference variable.

The expression:

```java
new Student()
```

creates an object instance.

The assignment connects them:

```java
Student s = new Student();
```

---

## ❌ Mistake 3: Thinking assignment copies objects

```java
s2 = s1;
```

does not create a copy of the object.

It copies the reference value.

---

## ❌ Mistake 4: Thinking every object has a separate static field

Static members are associated with the class, not independently with every object.

---

## ❌ Mistake 5: Thinking `null` is an object

```java
Student s = null;
```

`null` means the reference does not refer to an object.

---

## ❌ Mistake 6: Thinking garbage collection happens immediately

Be precise:

> The object becomes eligible for garbage collection when unreachable; collection timing is determined by the JVM.

---

## ❌ Mistake 7: Thinking Java passes objects by reference

Java is **pass-by-value**.

For object parameters, the value passed is a copy of the reference.

---

# 💼 30. Interview Questions

## 🟢 Basic

### 1. What is a class?

A class is a user-defined reference type that defines the structure and behavior of its instances.

---

### 2. What is an object?

An object is a runtime instance of a class.

---

### 3. How do you create an object in Java?

Commonly:

```java
Student s = new Student();
```

---

### 4. What is the `new` keyword?

`new` is an operator used to create a new object instance.

---

### 5. What is a reference variable?

A variable whose value is a reference to an object or `null`.

---

### 6. What is the difference between class and object?

```text
Class  → Defines a type
Object → Instance of that type
```

---

### 7. What are the characteristics of an object?

Commonly:

```text
State
Behavior
Identity
```

---

### 8. Can one class create multiple objects?

Yes.

```java
Student s1 = new Student();
Student s2 = new Student();
Student s3 = new Student();
```

---

### 9. Can multiple references refer to the same object?

Yes.

```java
Student s1 = new Student();
Student s2 = s1;
```

---

### 10. What happens if an object has no reference?

If it is unreachable from GC roots, it can become eligible for garbage collection.

---

# 🟡 Intermediate

### 11. Where are objects stored in Java?

Objects are generally allocated in the JVM heap, though JVM implementations can optimize allocation and object representation.

---

### 12. Where is a reference variable stored?

A local reference variable is associated with a method's stack frame in the common conceptual model.

But do not claim that every reference variable in every situation is physically stored on the stack. Fields, static fields, and implementation optimizations differ.

---

### 13. What happens when we write:

```java
Student s = new Student();
```

Conceptually:

```text
Reference variable declared
        ↓
Object allocated
        ↓
Fields initialized
        ↓
Constructor executes
        ↓
Reference assigned to s
```

---

### 14. What happens when we write:

```java
Student s1 = s2;
```

The reference value in `s2` is copied into `s1`.

No new object is created.

---

### 15. What is an anonymous object?

An object created without storing its reference in a named variable.

Example:

```java
new Student().study();
```

---

### 16. Can a method return an object?

Yes.

```java
static Student createStudent() {
    return new Student();
}
```

---

### 17. Can an object be passed to a method?

Yes.

```java
void display(Student s) {
}
```

---

### 18. Is Java pass-by-reference?

No.

Java is always pass-by-value.

When passing an object, the copied value is the object reference.

---

### 19. What is `null`?

`null` is a special value that can be assigned to reference types to indicate that the reference does not currently refer to an object.

---

### 20. What is the difference between `==` and `equals()`?

```text
==       → reference identity for object references
equals() → logical equality according to the implementation
```

---

# 🔴 Advanced / Tricky

### 21. Does `new` always create an object on the heap?

The Java language/JVM specifications do not require a simplistic physical interpretation that every object must literally reside in heap memory at all times.

JIT optimizations such as escape analysis may eliminate or transform allocations.

For interviews, the standard answer is:

> Objects are generally allocated in the heap.

---

### 22. Can a reference variable exist without an object?

Yes.

```java
Student s;
```

or:

```java
Student s = null;
```

No `Student` object is created by these statements alone.

---

### 23. Can an object exist without a reference variable?

Yes.

For example:

```java
new Student().study();
```

The object exists even though your source code does not store it in a named reference variable.

---

### 24. Can two objects have the same state?

Absolutely.

```java
Student s1 = new Student();
Student s2 = new Student();

s1.name = "Rahul";
s2.name = "Rahul";
```

Same state does not necessarily mean same object.

---

### 25. Can two references point to the same object?

Yes.

```java
Student s1 = new Student();
Student s2 = s1;
```

---

### 26. What happens to the object after:

```java
s1 = s2;
```

The old object referenced by `s1` does not automatically disappear.

It becomes eligible for garbage collection only if no other reachable references point to it.

---

### 27. What is object identity?

Object identity means the identity of a particular object instance, independent of whether another object contains the same data.

---

### 28. Why does `s1 == s2` sometimes return `true`?

Because both references can point to the exact same object.

```java
Student s1 = new Student();
Student s2 = s1;
```

---

### 29. Why can `s1.equals(s2)` be true while `s1 == s2` is false?

Because `equals()` may be overridden to compare logical state rather than object identity.

---

### 30. Is every class an object?

No.

A class is a type definition.

At runtime, Java also has `Class` objects representing loaded classes, but that does not make the concepts of class and object identical.

---

### 31. What is the default value of instance variables?

For example:

```text
byte      → 0
short     → 0
int       → 0
long      → 0L
float     → 0.0f
double    → 0.0d
char      → '\u0000'
boolean   → false
reference → null
```

These defaults apply to **fields**.

They do **not** automatically apply to local variables.

---

### 32. Why does this fail?

```java
public static void main(String[] args) {

    int age;

    System.out.println(age);
}
```

Because local variables do not receive Java's default field initialization.

They must be definitely assigned before use.

---

### 33. Can an object contain another object?

Yes.

This is the foundation of composition.

```java
class Engine {
}

class Car {

    Engine engine;
}
```

Conceptually:

```text
Car Object
    │
    └── Engine reference
             │
             ▼
        Engine Object
```

---

### 34. Can a class have no objects?

Yes.

A class can be declared without any instances being created.

---

### 35. Can an object exist without a class?

In normal Java programming, objects are instances of classes or class-based runtime types.

Java also has arrays, which are objects even though array types have special syntax rather than being user-declared classes.

---

# 🔥 31. Top 10 Interview Questions

## 1️⃣ What is a class?

A class is a reference type that defines the structure and behavior of its instances.

---

## 2️⃣ What is an object?

An object is a runtime instance of a class.

---

## 3️⃣ What is the difference between object and reference?

```text
Reference → Refers to an object
Object    → Actual runtime instance
```

---

## 4️⃣ What does `new` do?

It creates a new object instance.

---

## 5️⃣ What happens here?

```java
Student s = new Student();
```

Conceptually:

```text
Student → reference type
s       → reference variable
new Student() → object creation
```

---

## 6️⃣ What happens here?

```java
Student s2 = s1;
```

The reference value is copied.

No new object is created.

---

## 7️⃣ Can two references point to one object?

Yes.

```java
Student s1 = new Student();
Student s2 = s1;
```

---

## 8️⃣ What is `null`?

A special reference value indicating that the reference does not refer to an object.

---

## 9️⃣ Is Java pass-by-reference?

No.

Java is always pass-by-value.

For objects, the value passed is a copy of the reference.

---

## 🔟 Difference between `==` and `equals()`?

```text
==       → compares reference identity for objects
equals() → compares logical equality according to its implementation
```

---

# 🧠 32. Quick Revision

```text
                    CLASS
                      │
                      │ defines
                      ▼
                    OBJECT
                      │
             ┌────────┼────────┐
             ▼        ▼        ▼
           STATE   BEHAVIOR  IDENTITY
```

### Object Creation

```java
Student s = new Student();
```

Remember:

```text
Student
   ↓
Reference Type

s
   ↓
Reference Variable

new Student()
   ↓
Object
```

---

### Reference Concept

```text
s1 ───────► Object A

s2 = s1

s1 ──┐
     ├──────► Object A
s2 ──┘
```

---

### Assignment

```java
s1 = s2;
```

Means:

```text
Copy reference
NOT
Copy object
```

---

### `==` vs `equals()`

```text
==       → identity/reference comparison
equals() → logical equality if overridden
```

---

### Garbage Collection

```text
Reachable Object
      ↓
Still usable

Unreachable Object
      ↓
Eligible for GC
```

---

# ⚡ 33. 30-Second Interview Answer

> **"A class is a reference type that defines the structure and behavior of its objects, while an object is a runtime instance of that class. We commonly create an object using the `new` operator, such as `Student s = new Student()`. Here, `s` is a reference variable and `new Student()` creates the object. Objects have state, behavior, and identity. Multiple references can point to the same object, and assigning one reference to another copies the reference rather than the object. Java is always pass-by-value, including when object references are passed to methods. Objects are generally allocated in the heap, and unreachable objects can become eligible for garbage collection."**

---

# 🏆 34. Memory Trick

Remember:

```text
CLASS
 ↓
Blueprint / Type

OBJECT
 ↓
Actual Instance

REFERENCE
 ↓
Way to refer to Object

new
 ↓
Creates Object

null
 ↓
No Object Referenced

==
 ↓
Same Object?

equals()
 ↓
Logically Equal?
```

### 🔥 One-line master memory

> **Class defines → `new` creates → reference refers → object stores state → methods provide behavior.**

---

# 🎯 Final Takeaway

The most important line in this entire chapter is:

```java
Student s = new Student();
```

Understand it deeply:

```text
                Student s = new Student();
                ──────┬──── ─────────────
                     │          │
                     │          └── Object
                     │
                     └── Reference variable
```

And remember:

```text
Class ≠ Object
Reference ≠ Object
Reference assignment ≠ Object copying
null ≠ Object
== ≠ equals()
Java ≠ Pass-by-reference
```

> ☕ **If you truly understand classes, objects, and references, a huge part of Java OOP becomes much easier.**
