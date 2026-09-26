# 🧠 Object Creation in Java

> Creating an object in Java involves much more than simply writing `new`. The JVM may load the class, allocate memory for the object, initialize its fields, execute the constructor, and finally assign the object's reference to a variable.

---

## 📌 Table of Contents

1. [Introduction](#-introduction)
2. [What Is an Object?](#-what-is-an-object)
3. [What Is Object Creation?](#-what-is-object-creation)
4. [The `new` Keyword](#-the-new-keyword)
5. [Basic Object Creation Syntax](#-basic-object-creation-syntax)
6. [Step-by-Step Object Creation](#-step-by-step-object-creation)
7. [Step 1 — Class Loading](#-step-1--class-loading)
8. [Step 2 — Memory Allocation](#-step-2--memory-allocation)
9. [Step 3 — Default Initialization](#-step-3--default-initialization)
10. [Step 4 — Constructor Execution](#-step-4--constructor-execution)
11. [Step 5 — Reference Assignment](#-step-5--reference-assignment)
12. [Complete Object Creation Flow](#-complete-object-creation-flow)
13. [Example](#-example)
14. [Object Memory Representation](#-object-memory-representation)
15. [Instance Variables and Object Memory](#-instance-variables-and-object-memory)
16. [Static Variables During Object Creation](#-static-variables-during-object-creation)
17. [Instance Initializer Blocks](#-instance-initializer-blocks)
18. [Constructor Execution Order](#-constructor-execution-order)
19. [Inheritance and Object Creation](#-inheritance-and-object-creation)
20. [Object Creation with Inheritance](#-object-creation-with-inheritance)
21. [Reference Variable vs Object](#-reference-variable-vs-object)
22. [Multiple References to One Object](#-multiple-references-to-one-object)
23. [Creating Multiple Objects](#-creating-multiple-objects)
24. [Anonymous Objects](#-anonymous-objects)
25. [Objects Without `new`](#-objects-without-new)
26. [Factory Methods](#-factory-methods)
27. [Reflection and Object Creation](#-reflection-and-object-creation)
28. [Cloning](#-cloning)
29. [Deserialization](#-deserialization)
30. [Is `new` Always Required?](#-is-new-always-required)
31. [Object Header](#-object-header)
32. [Object Size](#-object-size)
33. [Heap Allocation](#-heap-allocation)
34. [Escape Analysis](#-escape-analysis)
35. [Stack Allocation Myth](#-stack-allocation-myth)
36. [Constructor vs Object Creation](#-constructor-vs-object-creation)
37. [Garbage Collection and Objects](#-garbage-collection-and-objects)
38. [When Does an Object Become Garbage?](#-when-does-an-object-become-garbage)
39. [Common Misconceptions](#-common-misconceptions)
40. [Interview Traps](#-interview-traps)
41. [Quick Comparison](#-quick-comparison)
42. [Cheat Sheet](#-cheat-sheet)
43. [30-Second Interview Answer](#-30-second-interview-answer)
44. [Top 10 Interview Questions](#-top-10-interview-questions)

---

# 🔹 Introduction

Consider:

```java
Student s = new Student();
```

At first glance, this looks like one simple operation.

Internally, several things are involved:

```text
Student s = new Student();
       |
       +── class information must be available
       |
       +── memory must be allocated
       |
       +── object state must be initialized
       |
       +── constructor must execute
       |
       +── reference is assigned to s
```

The exact internal implementation is JVM-dependent, but this is the correct conceptual model.

---

# 🔹 What Is an Object?

An object is a runtime entity created from a class.

Example:

```java
class Student {

    int age;
    String name;
}
```

Creating:

```java
Student s = new Student();
```

produces a `Student` object.

Conceptually:

```text
Student class
     |
     | creates
     ↓
Student object
```

The class defines the structure and behavior.

The object represents an actual runtime instance.

---

# 🔹 What Is Object Creation?

Object creation means creating an instance of a class at runtime.

Example:

```java
Student s = new Student();
```

Here:

```text
Student
```

is the class type.

```text
new Student()
```

creates an object.

```text
s
```

is a reference variable that refers to that object.

---

# 🔹 The `new` Keyword

The `new` keyword is commonly used to create objects.

Example:

```java
Student s = new Student();
```

Conceptually:

```text
new
 ↓
request object creation
 ↓
allocate object memory
 ↓
initialize object
 ↓
execute constructor
 ↓
return reference
```

The exact allocation strategy is handled by the JVM.

---

# 🔹 Basic Object Creation Syntax

```java
ClassName reference = new ClassName();
```

Example:

```java
Student student = new Student();
```

Break it down:

```text
Student
   ↑
reference type

student
   ↑
reference variable

new Student()
   ↑
object creation expression
```

---

# 🔹 Step-by-Step Object Creation

For:

```java
Student s = new Student();
```

think of the process as:

```text
1. Check whether Student class is loaded
             ↓
2. Load/link/initialize class when required
             ↓
3. Allocate memory for object
             ↓
4. Initialize fields to default values
             ↓
5. Execute instance initialization
             ↓
6. Execute constructor
             ↓
7. Return object reference
             ↓
8. Store reference in s
```

This is a conceptual sequence.

The JVM and compiler may optimize the actual implementation.

---

# 🔹 Step 1 — Class Loading

Before creating an object, the JVM must have the class available.

Suppose:

```java
Student s = new Student();
```

If `Student` has not yet been loaded:

```text
Student.class
     ↓
ClassLoader
     ↓
Class loading
     ↓
Linking
     ↓
Initialization when required
```

After the class is properly initialized, object creation can proceed.

---

## Important

Class loading does **not** happen every time you write:

```java
new Student();
```

Once a class has been loaded by a particular class loader, it can generally be reused for subsequent object creation.

---

# 🔹 Step 2 — Memory Allocation

The JVM allocates memory for the new object.

Conceptually:

```java
Student s = new Student();
```

creates something like:

```text
Heap

+----------------------+
| Student Object       |
|----------------------|
| age                  |
| name reference       |
+----------------------+
```

The object is conceptually allocated in the heap.

---

# 🔹 Step 3 — Default Initialization

Before the constructor's body executes, instance fields receive their default values as part of object initialization.

Example:

```java
class Student {

    int age;
    boolean active;
    String name;
}
```

Initially:

```text
age     → 0
active  → false
name    → null
```

Default values include:

| Type | Default |
|---|---|
| `byte` | `0` |
| `short` | `0` |
| `int` | `0` |
| `long` | `0L` |
| `float` | `0.0f` |
| `double` | `0.0d` |
| `char` | `'\u0000'` |
| `boolean` | `false` |
| Reference | `null` |

---

# 🔹 Step 4 — Constructor Execution

After the object's memory has been initialized appropriately, constructor initialization takes place.

Example:

```java
class Student {

    int age;

    Student() {
        age = 20;
    }
}
```

Then:

```java
Student s = new Student();
```

results conceptually in:

```text
Allocate object
      ↓
Default initialization
      ↓
Constructor executes
      ↓
age becomes 20
```

---

# 🔹 Step 5 — Reference Assignment

The result of:

```java
new Student()
```

is a reference to the newly created object.

That reference is assigned to:

```java
s
```

Conceptually:

```text
Stack

s
│
│ reference
↓
Heap

Student object
┌───────────────┐
│ age = 20      │
└───────────────┘
```

The variable `s` does not contain the complete object.

It contains a reference to the object.

---

# 🔥 Complete Object Creation Flow

For:

```java
Student s = new Student();
```

remember:

```text
                Student.class
                     |
                     ↓
                ClassLoader
                     |
                     ↓
              Class available
                     |
                     ↓
                new Student()
                     |
                     ↓
              Allocate object
                     |
                     ↓
           Default initialization
                     |
                     ↓
          Instance initialization
                     |
                     ↓
             Constructor
                     |
                     ↓
             Object reference
                     |
                     ↓
                      s
```

---

# 🔹 Example

```java
class Student {

    int age;
    String name;

    Student() {
        age = 20;
        name = "Yashu";
    }

    void study() {
        System.out.println("Studying");
    }
}
```

Then:

```java
Student s = new Student();
```

Conceptually:

```text
Heap

Student Object
┌──────────────────────┐
│ age = 20             │
│ name ────────────────┼──→ "Yashu"
└──────────────────────┘
```

And:

```text
Stack

s
│
└──────────────→ Student Object
```

---

# 🔹 Object Memory Representation

An object is more than just its fields.

A JVM implementation may maintain information associated with an object such as:

```text
Object
┌───────────────────────────┐
│ Object Header             │
├───────────────────────────┤
│ Instance Field 1          │
├───────────────────────────┤
│ Instance Field 2          │
├───────────────────────────┤
│ ...                       │
└───────────────────────────┘
```

In HotSpot, object headers contain implementation-specific information such as:

- Mark Word
- Class pointer / compressed class pointer where applicable

The exact layout depends on JVM version, architecture, configuration, and object type.

---

# 🔹 Instance Variables and Object Memory

Consider:

```java
class Employee {

    int id;
    String name;
}
```

Create:

```java
Employee e1 = new Employee();
Employee e2 = new Employee();
```

There are two separate objects.

Conceptually:

```text
Heap

e1 object
┌──────────────┐
│ id           │
│ name         │
└──────────────┘

e2 object
┌──────────────┐
│ id           │
│ name         │
└──────────────┘
```

Each object has its own instance state.

---

# 🔹 Static Variables During Object Creation

Consider:

```java
class Employee {

    static int count = 0;

    int id;
}
```

When:

```java
Employee e = new Employee();
```

`count` is not created separately inside every object.

Conceptually:

```text
Class-level state
Employee.count

        ↓

Shared by Employee instances

        ↓

e1     e2     e3
```

The exact physical storage of static state is JVM implementation-dependent.

The important Java-level concept is:

> A static field belongs to the class rather than to an individual object.

---

# 🔹 Instance Initializer Blocks

Java allows instance initializer blocks:

```java
class Student {

    int age;

    {
        age = 20;
    }

    Student() {
        System.out.println("Constructor");
    }
}
```

When an object is created:

```java
Student s = new Student();
```

the instance initializer executes as part of instance initialization before the constructor body.

Conceptually:

```text
Object allocation
      ↓
Default initialization
      ↓
Instance field initializers / instance initializer blocks
      ↓
Constructor body
```

---

# 🔹 Constructor Execution Order

Consider:

```java
class Student {

    int age = 18;

    {
        System.out.println("Instance block");
    }

    Student() {
        System.out.println("Constructor");
    }
}
```

Creating:

```java
Student s = new Student();
```

produces:

```text
Default field initialization
        ↓
Instance field initializer
        ↓
Instance initializer block
        ↓
Constructor body
```

Output:

```text
Instance block
Constructor
```

---

# 🔹 Inheritance and Object Creation

Now consider:

```java
class Parent {

    Parent() {
        System.out.println("Parent");
    }
}

class Child extends Parent {

    Child() {
        System.out.println("Child");
    }
}
```

Then:

```java
Child c = new Child();
```

The parent portion must be initialized before the child portion.

Output:

```text
Parent
Child
```

---

# 🔹 Object Creation with Inheritance

Conceptually:

```text
new Child()
    |
    ↓
Create Child object
    |
    ↓
Initialize Parent portion
    |
    ↓
Parent constructor
    |
    ↓
Initialize Child portion
    |
    ↓
Child constructor
```

This is why the superclass constructor executes before the subclass constructor body.

---

# 🔥 Constructor Chain

Consider:

```java
class Parent {

    Parent() {
        System.out.println("Parent constructor");
    }
}

class Child extends Parent {

    Child() {
        System.out.println("Child constructor");
    }
}
```

The compiler implicitly inserts:

```java
super();
```

at the beginning of the child constructor when no explicit constructor invocation is written.

So conceptually:

```java
Child() {
    super();
    System.out.println("Child constructor");
}
```

---

# 🔹 Reference Variable vs Object

This is one of the most important Java concepts.

Consider:

```java
Student s = new Student();
```

There are two different things:

```text
s
↓
Reference variable

new Student()
↓
Object
```

The variable:

```java
s
```

is not the object itself.

It refers to the object.

---

# 🔹 Multiple References to One Object

Consider:

```java
Student s1 = new Student();

Student s2 = s1;
```

Now:

```text
Stack

s1 ──────┐
         │
s2 ──────┤
         ↓
       Heap

    Student object
```

Both references point to the same object.

Therefore:

```java
s1 == s2
```

returns:

```text
true
```

---

# 🔹 Creating Multiple Objects

Consider:

```java
Student s1 = new Student();
Student s2 = new Student();
```

Two `new` expressions generally create two distinct objects.

Conceptually:

```text
s1 ─────→ Object 1

s2 ─────→ Object 2
```

Therefore:

```java
s1 == s2
```

is:

```text
false
```

assuming distinct object creations and no special behavior affecting the references.

---

# 🔹 Anonymous Objects

An object can be created without storing its reference in a named variable.

Example:

```java
new Student();
```

This is commonly called an:

> **Anonymous object**

Another example:

```java
new Student().study();
```

The object is created and immediately used.

After there are no reachable references to it, it can eventually become eligible for garbage collection.

---

# 🔹 Objects Without `new`

`new` is not the only mechanism by which an object can come into existence.

For example:

```java
String s = "Hello";
```

does not explicitly use `new`.

The JVM/runtime can use an existing interned `String` object or create one as required.

---

# 🔹 Factory Methods

Objects can also be created through factory methods.

Example:

```java
Integer number = Integer.valueOf(10);
```

Here we don't explicitly write:

```java
new Integer(10)
```

The method returns an object/reference according to its implementation and API contract.

Another example:

```java
Calendar calendar = Calendar.getInstance();
```

The caller does not directly invoke a constructor.

---

# 🔹 Reflection and Object Creation

Objects can also be created using reflection.

For example:

```java
Class<?> clazz = Student.class;

Student s = (Student) clazz.getDeclaredConstructor().newInstance();
```

Conceptually:

```text
Class object
    ↓
Reflection API
    ↓
Constructor
    ↓
Object
```

Modern Java code should generally prefer normal constructors or appropriate factory APIs unless reflection is actually required.

---

# 🔹 Cloning

Java also provides object copying through cloning mechanisms.

Example:

```java
class Student implements Cloneable {

    int age;

    public Student clone() throws CloneNotSupportedException {
        return (Student) super.clone();
    }
}
```

Then:

```java
Student s2 = s1.clone();
```

A new object can be produced based on the original object's state.

However, cloning has several subtleties involving:

- Shallow copying
- Deep copying
- `Cloneable`
- `Object.clone()`

So cloning should not be treated as simply "another form of `new`."

---

# 🔹 Deserialization

An object can also be reconstructed from serialized data.

For example, using Java serialization:

```java
ObjectInputStream in =
        new ObjectInputStream(inputStream);

Student s = (Student) in.readObject();
```

The normal constructor path is different from ordinary:

```java
new Student();
```

This is one reason:

> "Constructor always creates the object"

is not a complete statement.

---

# 🔹 Is `new` Always Required?

No.

Common object-creation/reconstruction mechanisms include:

```text
new
│
├── Constructors
│
├── Factory methods
│
├── Reflection
│
├── Cloning
│
└── Deserialization
```

But these mechanisms have different semantics.

---

# 🔹 Object Header

In HotSpot, every ordinary object has an implementation-specific object layout.

A simplified conceptual layout:

```text
Object
┌────────────────────────────┐
│ Mark Word                  │
├────────────────────────────┤
│ Class Pointer              │
├────────────────────────────┤
│ Instance Fields            │
├────────────────────────────┤
│ Padding (if required)      │
└────────────────────────────┘
```

### Mark Word

Can contain information associated with:

- Object identity/hash information
- Locking state
- GC-related information

### Class Pointer

Identifies the object's class metadata representation.

Depending on configuration, HotSpot may use a:

```text
Compressed Class Pointer
```

---

# ⚠️ Important

Do not memorize an object header as having one universal size.

Its exact layout depends on:

- JVM implementation
- JVM version
- CPU architecture
- Object type
- Compressed references/class pointers
- JVM options

---

# 🔹 Object Size

Suppose:

```java
class Student {

    int age;
    boolean active;
}
```

You cannot reliably calculate the exact object size simply by adding:

```text
4 bytes + 1 byte
```

because the JVM may also need:

- Object header
- Alignment
- Padding
- Reference fields
- Implementation-specific metadata

For example:

```text
Fields
  +
Object Header
  +
Alignment/Padding
  =
Actual object size
```

---

# 🔹 Heap Allocation

At the Java language level, objects and arrays are allocated in the heap's conceptual model.

However, HotSpot may optimize allocation internally.

For example:

```text
Thread
  ↓
TLAB
  ↓
Object allocation
```

---

# 🔹 TLAB

TLAB stands for:

> **Thread-Local Allocation Buffer**

HotSpot can give a thread a small allocation region inside the heap.

Conceptually:

```text
Heap
│
├── Thread A → TLAB
│
├── Thread B → TLAB
│
└── Other heap regions
```

If a new object fits in a thread's TLAB, allocation can often be performed efficiently without contending for a shared allocation path.

---

# 🔹 Escape Analysis

The JVM's JIT compiler can analyze whether an object escapes a method or thread.

Example:

```java
void calculate() {

    Point p = new Point();

    System.out.println(p.x);
}
```

If the JIT determines that an object does not escape certain scopes, it may optimize the allocation or eliminate the object entirely where safe.

Possible optimizations include:

```text
Scalar replacement
Allocation elimination
Lock elimination
```

---

# 🔥 Stack Allocation Myth

A common statement is:

> "If an object is local, it is stored on the stack."

This is not a correct general rule.

Example:

```java
void test() {

    Student s = new Student();
}
```

At the Java conceptual level:

```text
s
→ reference/local variable

Student object
→ heap
```

HotSpot's JIT may optimize the allocation in some circumstances.

Therefore, don't memorize:

```text
local object → stack
```

as a Java rule.

A better statement:

> Java objects are conceptually heap allocated, although JVM optimizations such as escape analysis may eliminate or optimize actual allocation.

---

# 🔹 Constructor vs Object Creation

These are not the same thing.

### Object creation

```java
new Student()
```

involves obtaining/allocating and initializing an object.

### Constructor

```java
Student() {
}
```

is a special initialization mechanism executed as part of normal constructor-based object creation.

Therefore:

```text
Object creation
      +
Constructor initialization
```

are related but conceptually different operations.

---

# 🔹 Garbage Collection and Objects

Suppose:

```java
Student s = new Student();
```

Later:

```java
s = null;
```

If there are no other reachable references:

```text
Student object
      ↓
unreachable
      ↓
eligible for garbage collection
```

Important:

> Becoming unreachable does not mean the object is immediately destroyed.

The JVM's garbage collector determines when memory can be reclaimed.

---

# 🔹 When Does an Object Become Garbage?

An object becomes eligible for garbage collection when it is no longer reachable from GC roots.

Examples of GC roots can include:

- Active thread stacks
- Static references
- JNI references
- Other JVM/runtime roots

Example:

```java
Student s = new Student();

s = null;
```

If no other reference exists:

```text
GC Root
  X
  |
  X

Student Object
     ↓
unreachable
```

The object is eligible for reclamation.

---

# 🔥 Complete Memory Picture

For:

```java
Student s = new Student();
```

think:

```text
                 JVM
                  |
       +----------+----------+
       |                     |
   Class Runtime          Heap
    Structures              |
       |                    |
   Student metadata     Student object
       |                    |
       |                    |
       +--------------------+
                ↑
                |
             reference
                |
              Stack
                |
                s
```

---

# 🔹 Common Misconceptions

## ❌ 1. `new` creates the class

Wrong.

The class must already be loaded/available.

`new` creates an object/instance.

---

## ❌ 2. Every `new` loads the class

Wrong.

A class is not loaded from scratch for every object creation.

Once loaded by a given class loader, its runtime class information can be reused.

---

## ❌ 3. Reference variable contains the object

Wrong.

```java
Student s = new Student();
```

`s` contains a reference to the object.

---

## ❌ 4. Constructor creates memory for the object

The constructor initializes the object.

Object allocation and constructor execution are distinct conceptual steps.

---

## ❌ 5. Every local object is stored on the stack

Wrong.

Java's conceptual object allocation model uses the heap.

JIT optimizations may eliminate or optimize allocations.

---

## ❌ 6. Setting reference to `null` immediately destroys the object

Wrong.

```java
s = null;
```

only removes that particular reference.

The object becomes eligible for GC only if no other reachable references remain.

---

## ❌ 7. Garbage collection happens immediately after an object becomes unreachable

Wrong.

The object becomes eligible for collection.

The JVM decides when/how to reclaim it.

---

## ❌ 8. Constructor is responsible for allocating object memory

Not exactly.

Normal constructor-based object creation involves:

```text
Allocation
   ↓
Initialization
   ↓
Constructor execution
```

The constructor's job is initialization.

---

# 🔹 Interview Traps

### Trap 1

**Q: Where is an object created?**

Typical answer:

> Objects are conceptually allocated in the heap.

Implementation details and JIT optimizations can alter the physical allocation behavior.

---

### Trap 2

**Q: Where is the reference variable stored?**

If it is a local variable, it is associated with the current thread's stack frame.

If it is an instance field or static field, its storage follows the corresponding object/class-level representation.

Do not say:

> "All references are stored in stack."

---

### Trap 3

**Q: Does `new` call the constructor first and then allocate memory?**

No.

Conceptually, memory is allocated and initialized before the constructor body executes.

---

### Trap 4

**Q: Does `new` always create a new object?**

For normal class instantiation:

```java
new Student()
```

creates a new instance.

But expressions such as:

```java
Integer.valueOf(10)
```

may return an existing object instead of explicitly creating a new one.

---

### Trap 5

**Q: Can objects be created without constructors?**

Object creation/reconstruction mechanisms such as deserialization and cloning have different initialization semantics from ordinary constructor-based creation.

---

# 🔹 Quick Comparison

| Concept | Meaning |
|---|---|
| Class | Blueprint/type definition |
| Object | Runtime instance |
| `new` | Normal object creation expression |
| Constructor | Initializes a newly created object |
| Reference | Identifies/refers to an object |
| Heap | Conceptual memory area for objects/arrays |
| Stack frame | Method execution context |
| Metaspace | HotSpot class metadata storage |
| GC | Reclaims unreachable objects |

---

# 🔥 Object Creation Cheat Sheet

```text
Student s = new Student();
```

### Breakdown

```text
Student
   ↓
Reference type

s
   ↓
Reference variable

new
   ↓
Object creation

Student()
   ↓
Constructor

new Student()
   ↓
Reference to newly created object
```

### Conceptual Flow

```text
Class available
      ↓
Allocate memory
      ↓
Default initialization
      ↓
Instance initializers
      ↓
Constructor chain
      ↓
Constructor body
      ↓
Reference returned
      ↓
Reference assigned
```

---

# 🧠 Inheritance Object Creation

```text
new Child()
     ↓
Object allocation
     ↓
Parent initialization
     ↓
Parent constructor
     ↓
Child initialization
     ↓
Child constructor
```

---

# 🧠 Garbage Collection

```text
Object created
      ↓
Referenced
      ↓
Used
      ↓
Reference removed
      ↓
No reachable references
      ↓
Eligible for GC
      ↓
Eventually reclaimed
```

---

# 🔥 30-Second Interview Answer

### Q: What happens when we create an object using `new`?

> When an object is created using `new`, the JVM first ensures that the required class is loaded and initialized if necessary. It then allocates memory for the object, initializes its fields to default values, performs instance initialization and constructor execution, and produces a reference to the object. That reference is assigned to the reference variable. Objects are conceptually allocated on the heap, while local reference variables are associated with stack frames. JVM optimizations such as escape analysis may change the physical implementation without changing Java's semantics.

---

# 🔥 Top 10 Interview Questions

## 1. What happens when we write `new Student()`?

**Answer:**

The JVM performs normal object creation: it allocates and initializes the object and executes the appropriate constructor, producing a reference to the newly created instance.

---

## 2. Where is an object stored?

**Answer:**

Conceptually, Java objects and arrays are allocated in the heap.

---

## 3. Where is a local reference variable stored?

**Answer:**

A local variable is associated with the current method's stack frame. However, not every Java reference is necessarily a stack variable; instance and static references have different storage contexts.

---

## 4. Does `new` load a class?

**Answer:**

If the required class has not yet been loaded/initialized, the JVM may perform the necessary class-loading process. But the class is not loaded again for every `new`.

---

## 5. What is the difference between an object and a reference?

**Answer:**

An object is the runtime instance containing state and behavior according to its class. A reference identifies the object.

Example:

```java
Student s = new Student();
```

Here:

```text
s → reference
new Student() → object
```

---

## 6. What happens before the constructor body executes?

**Answer:**

The object has been allocated and its instance fields have received default values. Instance field initializers and instance initializer blocks execute according to Java's initialization rules before the constructor body.

---

## 7. Does a constructor create the object?

**Answer:**

The constructor is responsible for initialization, not the entire object-creation process. Allocation and constructor execution are separate conceptual steps.

---

## 8. When does an object become eligible for GC?

**Answer:**

When it is no longer reachable from the JVM's GC roots, assuming no other reachable references exist.

---

## 9. Can Java create objects without `new`?

**Answer:**

Yes.

Objects can also be obtained or reconstructed through mechanisms such as:

```text
Factory methods
Reflection
Cloning
Deserialization
String literals/interning
```

Their semantics differ from normal constructor-based creation.

---

## 10. Is every object physically allocated on the heap?

**Answer:**

The Java conceptual model treats objects as heap allocated. However, a JIT compiler can optimize allocations using techniques such as escape analysis, scalar replacement, or allocation elimination.

---

# 🏆 Final Memory Trick

Remember this:

```text
CLASS
  ↓
Loaded
  ↓
new
  ↓
Allocate
  ↓
Default values
  ↓
Instance initialization
  ↓
Constructor
  ↓
OBJECT
  ↓
REFERENCE
```

And:

```text
CLASS
   ↓
Class Metadata
   ↓
Metaspace (HotSpot)

OBJECT
   ↓
Heap

LOCAL REFERENCE
   ↓
Stack Frame
```

> ⭐ **Golden Rule:**  
> **`new` does not simply "call a constructor." Normal object creation conceptually involves class availability, allocation, default initialization, instance initialization, constructor execution, and finally obtaining a reference to the object.**