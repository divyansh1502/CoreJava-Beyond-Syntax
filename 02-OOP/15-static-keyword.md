# ☕ Java OOP — `static` Keyword

> **The `static` keyword makes a member belong to the class rather than to individual objects.**

---

# 1. What is `static` in Java?

`static` is a keyword used to declare members that belong to the **class itself**, rather than to each individual object.

It can be used with:

```text
static variable
static method
static block
static nested class
```

The most important idea:

```text
Without static
     ↓
belongs to object

With static
     ↓
belongs to class
```

Example:

```java
class Student {

    int age;              // instance variable
    static String college; // static variable
}
```

Here:

```text
age
 ↓
Each Student object gets its own copy

college
 ↓
One class-level copy shared by Student objects
```

---

# 2. Why Do We Need `static`?

Suppose 100 students belong to the same college.

Without `static`:

```java
class Student {

    String college;
}
```

Every object gets its own `college` variable.

```text
Student 1 → college
Student 2 → college
Student 3 → college
...
Student 100 → college
```

But the college is common to all students.

So instead:

```java
class Student {

    static String college;
}
```

Now:

```text
Student
   |
   ↓
static college
   |
   ↓
one shared value
```

This avoids unnecessary duplication and represents class-level data.

---

# 3. Instance vs Static

Consider:

```java
class Student {

    int id;
    String name;

    static String college;
}
```

For:

```java
Student s1 = new Student();
Student s2 = new Student();
```

Memory conceptually looks like:

```text
Class Student
      |
      +---- static college
      |
      |
Objects
      |
      +---- s1
      |      |
      |      +---- id
      |      +---- name
      |
      +---- s2
             |
             +---- id
             +---- name
```

Therefore:

```text
instance variable
→ one copy per object

static variable
→ one class-level copy
```

---

# 4. Static Variable

A variable declared with `static` is called a **static variable** or **class variable**.

Example:

```java
class Student {

    int id;

    static String college = "AIET";
}
```

Now:

```java
Student s1 = new Student();
Student s2 = new Student();
```

Both objects can access:

```java
Student.college
```

Example:

```java
System.out.println(Student.college);
```

Output:

```text
AIET
```

---

# 5. Why Are Static Variables Shared?

Because a static field belongs to the class, not to each object.

Example:

```java
class Student {

    static int count = 0;

    Student() {
        count++;
    }
}
```

Now:

```java
Student s1 = new Student();
Student s2 = new Student();
Student s3 = new Student();

System.out.println(Student.count);
```

Output:

```text
3
```

There is one shared `count`.

Each constructor invocation modifies the same class-level variable.

---

# 6. Static Variable vs Instance Variable

```java
class Student {

    int id;

    static String college = "AIET";
}
```

### Instance variable

```text
id
 ↓
belongs to object
 ↓
each object has separate value
```

### Static variable

```text
college
 ↓
belongs to class
 ↓
shared by objects
```

| Feature          | Instance Variable      | Static Variable     |
| ---------------- | ---------------------- | ------------------- |
| Belongs to       | Object                 | Class               |
| Copies           | One per object         | One per class       |
| Access           | Object                 | Class preferred     |
| Memory           | Associated with object | Class-level storage |
| Shared?          | No                     | Yes                 |
| Requires object? | Generally yes          | No                  |

---

# 7. How to Access a Static Variable

Preferred way:

```java
class Student {

    static String college = "AIET";
}

System.out.println(Student.college);
```

Use:

```text
ClassName.staticMember
```

You can technically access a static member through an object:

```java
Student s = new Student();

System.out.println(s.college);
```

But this is discouraged because `college` belongs to the class, not the object.

Prefer:

```java
Student.college
```

---

# 8. Can Static Variables Be Modified?

Yes, unless they are also `final`.

Example:

```java
class Student {

    static String college = "AIET";
}
```

We can do:

```java
Student.college = "IIT";
```

Now all accesses to that static variable see:

```text
IIT
```

But:

```java
static final String college = "AIET";
```

cannot be reassigned.

---

# 9. `static final`

This is commonly used for constants.

```java
class MathConstants {

    static final double PI = 3.14159;
}
```

Access:

```java
System.out.println(MathConstants.PI);
```

Breakdown:

```text
static
 ↓
belongs to class

final
 ↓
cannot be reassigned
```

Therefore:

```text
static final
→ class-level constant
```

---

# 10. Static Method

A method declared with `static` is called a **static method** or **class method**.

Example:

```java
class Calculator {

    static int add(int a, int b) {
        return a + b;
    }
}
```

Call it without creating an object:

```java
int result = Calculator.add(10, 20);
```

Output:

```text
30
```

Why?

Because:

```text
add()
 ↓
belongs to Calculator class
 ↓
object is not required
```

---

# 11. Why Can We Call Static Methods Without Objects?

Because static methods belong to the class.

Example:

```java
Calculator.add(10, 20);
```

The call is associated with:

```text
Calculator
```

rather than:

```text
some Calculator object
```

Therefore:

```text
ClassName.method()
```

is the preferred syntax.

---

# 12. Static Method vs Instance Method

Example:

```java
class Student {

    static void collegeInfo() {
        System.out.println("AIET");
    }

    void studentInfo() {
        System.out.println("Student");
    }
}
```

Static:

```java
Student.collegeInfo();
```

Instance:

```java
Student s = new Student();

s.studentInfo();
```

Mental model:

```text
static method
     ↓
class-level behavior

instance method
     ↓
object-level behavior
```

---

# 13. Important Rule — Static Method Cannot Directly Access Instance Members

Example:

```java
class Student {

    int age = 20;

    static void show() {

        System.out.println(age); // ❌
    }
}
```

Why?

Because `age` belongs to an object.

But `show()` belongs to the class.

There may not even be an object when `show()` executes.

---

# 14. How Can a Static Method Access an Instance Variable?

Through an object reference.

```java
class Student {

    int age = 20;

    static void show() {

        Student s = new Student();

        System.out.println(s.age);
    }
}
```

Now:

```text
static method
      ↓
creates/accesses object
      ↓
object reference
      ↓
instance variable
```

---

# 15. Static Method Can Directly Access Static Members

Example:

```java
class Student {

    static String college = "AIET";

    static void showCollege() {

        System.out.println(college);
    }
}
```

This is valid.

Because both:

```text
college
showCollege()
```

belong to the class.

---

# 16. Why Can't Static Methods Directly Access Instance Members?

Consider:

```java
class Student {

    int age;

    static void show() {
        System.out.println(age);
    }
}
```

The problem is:

```text
static show()
      ↓
class-level
      ↓
no particular Student object
      ↓
which age?
```

There could be:

```text
Student s1 → age = 20
Student s2 → age = 25
Student s3 → age = 30
```

Which one should the static method use?

There is no implicit object.

Therefore direct access is not allowed.

---

# 17. Static Method Can Access Static Variables

Example:

```java
class Counter {

    static int count = 0;

    static void increment() {

        count++;
    }
}
```

Call:

```java
Counter.increment();
Counter.increment();

System.out.println(Counter.count);
```

Output:

```text
2
```

---

# 18. Can Static Methods Access `this`?

No.

Example:

```java
class Student {

    static void show() {

        System.out.println(this); // ❌
    }
}
```

Why?

Because `this` refers to the **current object**.

A static method does not have an implicit current object.

Therefore:

```text
static method
 ↓
no implicit this
```

---

# 19. Can Static Methods Use `super`?

No.

Example:

```java
static void show() {

    super.show(); // ❌
}
```

`super` refers to the parent part of the current object.

Since static context has no implicit object:

```text
static
 ↓
no this
 ↓
no super
```

---

# 20. Can Static Methods Be Overridden?

No.

Static methods are not overridden in the runtime-polymorphism sense.

They are **hidden**.

Example:

```java
class Parent {

    static void show() {
        System.out.println("Parent");
    }
}

class Child extends Parent {

    static void show() {
        System.out.println("Child");
    }
}
```

Now:

```java
Parent p = new Child();

p.show();
```

Output:

```text
Parent
```

This is method hiding.

---

# 21. Static Method Hiding

Static methods are selected based on the reference/class context rather than the runtime object in the same way as overridden instance methods.

Example:

```java
Parent.show();
Child.show();
```

Output:

```text
Parent
Child
```

This is not runtime overriding.

Remember:

```text
instance method
→ overriding
→ runtime dispatch

static method
→ hiding
→ compile-time/class-based selection
```

---

# 22. Can Static Methods Be Overloaded?

Yes.

Example:

```java
class Calculator {

    static int add(int a, int b) {
        return a + b;
    }

    static int add(int a, int b, int c) {
        return a + b + c;
    }
}
```

Both methods are static.

This is valid:

```java
Calculator.add(10, 20);

Calculator.add(10, 20, 30);
```

Therefore:

```text
static methods
→ can be overloaded
→ cannot be overridden
```

---

# 23. Static Block

A static block is a block declared with `static`.

Example:

```java
class Test {

    static {
        System.out.println("Static block");
    }
}
```

A static block is used for class-level initialization.

---

# 24. When Does a Static Block Execute?

A static block executes when the class is initialized by the JVM.

Example:

```java
class Test {

    static {
        System.out.println("Static block");
    }

    public static void main(String[] args) {
        System.out.println("Main");
    }
}
```

Output:

```text
Static block
Main
```

The static initialization happens before the class's `main()` method executes.

---

# 25. Multiple Static Blocks

A class can have multiple static blocks.

```java
class Test {

    static {
        System.out.println("Block 1");
    }

    static {
        System.out.println("Block 2");
    }

    public static void main(String[] args) {
        System.out.println("Main");
    }
}
```

Output:

```text
Block 1
Block 2
Main
```

They execute in textual order.

---

# 26. Static Block vs Constructor

These are different.

### Static block

```text
Runs during class initialization
```

### Constructor

```text
Runs when an object is created
```

Example:

```java
class Student {

    static {
        System.out.println("Static block");
    }

    Student() {
        System.out.println("Constructor");
    }
}
```

If:

```java
Student s1 = new Student();
Student s2 = new Student();
```

Output conceptually:

```text
Static block
Constructor
Constructor
```

The static block executes once per class initialization, while the constructor executes for each object creation.

---

# 27. Static Block and Object Creation

A static block does not require an object to execute.

Example:

```java
class Test {

    static {
        System.out.println("Hello");
    }
}
```

When the class is initialized:

```text
Test class initialized
      ↓
static block executes
```

No object is required.

---

# 28. Static Initialization

Static fields and static blocks participate in class initialization.

Example:

```java
class Test {

    static int x = 10;

    static {
        x = 20;
    }
}
```

After initialization:

```text
x = 20
```

The class initialization process executes static field initializers and static initializer blocks in textual order.

---

# 29. Static Initialization Order

Example:

```java
class Test {

    static int x = 10;

    static {
        x = 20;
    }

    static int y = 30;

    static {
        y = 40;
    }
}
```

Conceptually:

```text
x = 10
 ↓
static block → x = 20
 ↓
y = 30
 ↓
static block → y = 40
```

Final values:

```text
x = 20
y = 40
```

---

# 30. Static Nested Class

Java allows a nested class to be declared `static`.

Example:

```java
class Outer {

    static class Inner {

        void show() {
            System.out.println("Inner");
        }
    }
}
```

Create it using:

```java
Outer.Inner obj = new Outer.Inner();

obj.show();
```

Notice:

```text
No Outer object is required
```

for creating the static nested class instance.

---

# 31. Static Nested Class vs Inner Class

### Static nested class

```java
class Outer {

    static class Inner {
    }
}
```

It does not require an instance of `Outer`.

### Non-static inner class

```java
class Outer {

    class Inner {
    }
}
```

It is associated with an enclosing `Outer` instance.

Example:

```java
Outer outer = new Outer();

Outer.Inner inner = outer.new Inner();
```

---

# 32. Can a Top-Level Class Be Static?

No.

This is invalid:

```java
static class Student {
}
```

for a top-level class.

`static` applies to nested classes, not top-level classes.

---

# 33. Why Is `main()` Static?

The Java entry point is:

```java
public static void main(String[] args)
```

The `main()` method is static because the JVM needs to invoke it without first creating an object of the application class.

Conceptually:

```text
Program starts
     ↓
JVM loads/initializes class
     ↓
JVM invokes static main()
     ↓
Program execution begins
```

There is no requirement for the JVM to create an object just to start the program.

---

# 34. Why Is `main()` Public?

Traditionally, the JVM needs to be able to access the entry-point method from outside the class.

Therefore:

```java
public static void main(String[] args)
```

contains:

```text
public
→ accessible to JVM

static
→ callable without object

void
→ returns nothing

main
→ recognized entry-point name

String[]
→ receives command-line arguments
```

---

# 35. Can `main()` Be Overloaded?

Yes.

Example:

```java
class Test {

    public static void main(String[] args) {
        System.out.println("Main");
    }

    public static void main(int x) {
        System.out.println(x);
    }
}
```

This is valid overloading.

But the JVM looks for the recognized entry-point signature to start execution.

---

# 36. Can `main()` Be Overridden?

`main()` is static.

Therefore it is not overridden.

A subclass can declare another static `main()` method, but that is method hiding, not runtime overriding.

---

# 37. Static Import

Java allows static members to be imported directly.

Instead of:

```java
Math.sqrt(25);
```

you can use:

```java
import static java.lang.Math.sqrt;
```

Then:

```java
sqrt(25);
```

Similarly:

```java
import static java.lang.Math.PI;
```

Then:

```java
System.out.println(PI);
```

Static import can reduce repetitive class names, but excessive use may reduce readability.

---

# 38. Static Import vs Normal Import

Normal import:

```java
import java.util.ArrayList;
```

allows:

```java
ArrayList<Integer> list = new ArrayList<>();
```

Static import:

```java
import static java.lang.Math.PI;
```

allows:

```java
System.out.println(PI);
```

Difference:

```text
normal import
→ imports types

static import
→ imports static members
```

---

# 39. Static and Inheritance

Static members can be inherited depending on accessibility and class structure, but they remain class members.

Example:

```java
class Parent {

    static int x = 10;
}

class Child extends Parent {
}
```

You can write:

```java
System.out.println(Child.x);
```

But the field still belongs to the class hierarchy's static member, not to individual `Child` objects.

It is important not to think of static members as object state.

---

# 40. Static Variable Hiding

A subclass can declare a static field with the same name.

```java
class Parent {

    static int x = 10;
}

class Child extends Parent {

    static int x = 20;
}
```

Now:

```java
System.out.println(Parent.x);
System.out.println(Child.x);
```

Output:

```text
10
20
```

This is field hiding, not dynamic field overriding.

---

# 41. Static vs Instance Fields in Polymorphism

Example:

```java
class Parent {

    static int x = 10;

    int y = 10;
}

class Child extends Parent {

    static int x = 20;

    int y = 20;
}
```

Now:

```java
Parent p = new Child();

System.out.println(p.x);
System.out.println(p.y);
```

Output:

```text
10
10
```

Fields are not dynamically dispatched.

Both static and instance fields are resolved based on the declared/reference type in this kind of access.

---

# 42. Static Methods vs Instance Methods

| Feature                           | Static Method | Instance Method           |
| --------------------------------- | ------------- | ------------------------- |
| Belongs to                        | Class         | Object                    |
| Object required                   | No            | Yes for normal invocation |
| Direct access to instance members | No            | Yes                       |
| `this` available                  | No            | Yes                       |
| `super` available                 | No            | Yes                       |
| Overloading                       | Yes           | Yes                       |
| Overriding                        | No            | Yes                       |
| Method hiding                     | Yes           | No                        |
| Runtime polymorphism              | No            | Yes                       |

---

# 43. Static Block vs Instance Block

Java also supports instance initializer blocks.

### Static block

```java
static {
    System.out.println("Static");
}
```

Runs during class initialization.

### Instance initializer

```java
{
    System.out.println("Instance");
}
```

Runs when an object is initialized, as part of constructor execution.

Example:

```java
class Test {

    static {
        System.out.println("Static");
    }

    {
        System.out.println("Instance");
    }

    Test() {
        System.out.println("Constructor");
    }
}
```

When:

```java
new Test();
```

Output:

```text
Static
Instance
Constructor
```

---

# 44. Static Context

A static method/block is called a **static context**.

Inside static context, you cannot directly use:

```text
this
super
instance variables
instance methods
```

unless an object/reference is explicitly available.

Example:

```java
class Test {

    int x;

    static void show() {

        // x;       ❌
        // this.x;  ❌
        // this;    ❌
    }
}
```

---

# 45. Static Context and Object Reference

You can access instance members if you explicitly create/use an object.

```java
class Test {

    int x = 10;

    static void show() {

        Test obj = new Test();

        System.out.println(obj.x);
    }
}
```

The important distinction is:

```text
implicit object
→ unavailable in static context

explicit object
→ can access instance members
```

---

# 46. Static Does Not Mean "Stored in Stack"

This is an interview trap.

Do not memorize:

```text
static → stack
```

or:

```text
static → heap
```

as a simplistic universal rule.

The JVM specification does not define Java memory exactly as:

```text
static = one specific memory area
```

Modern JVM implementations manage class metadata, static fields, objects, and runtime structures according to the JVM implementation.

For interviews, the important conceptual point is:

```text
static member
→ associated with the class
→ not each individual object
```

---

# 47. Static Variables and Garbage Collection

A static field can keep an object reachable.

Example:

```java
class Cache {

    static Object data;
}
```

If:

```java
Cache.data = new Object();
```

the object remains reachable through the static field while that reference remains.

Therefore, static references can contribute to objects staying alive longer than expected.

Important:

```text
static
≠
automatically memory leak
```

But careless static references can cause memory-retention problems.

---

# 48. Static Initialization and Class Loading

A simplified model:

```text
Class requested
      ↓
Class loaded
      ↓
Class linked
      ↓
Class initialized
      ↓
Static initialization executes
```

During class initialization, static field initializers and static initializer blocks are executed according to Java's class-initialization rules.

This is why:

```java
static {
    System.out.println("Hello");
}
```

can execute before `main()`.

---

# 49. When Is a Class Initialized?

A class is initialized when Java determines that initialization is required, such as through active use of the class.

Common examples include:

```text
Accessing certain static fields
Invoking a static method
Creating an instance
Reflective initialization
```

Not every way of mentioning a class necessarily causes initialization.

This distinction becomes important in JVM/interview questions.

---

# 50. Static Method and Thread Safety

A static method is not automatically thread-safe.

Example:

```java
class Counter {

    static int count = 0;

    static void increment() {
        count++;
    }
}
```

Multiple threads can execute:

```java
Counter.increment();
```

at the same time.

The fact that the method is static does not provide synchronization.

Therefore:

```text
static
≠
thread-safe
```

Thread safety requires appropriate synchronization or concurrency mechanisms.

---

# 51. Static Does Not Mean Constant

Compare:

```java
static int x = 10;
```

and:

```java
static final int x = 10;
```

The first can change:

```java
x = 20;
```

The second cannot:

```java
x = 20; // ❌
```

Therefore:

```text
static
→ class-level

final
→ cannot reassign
```

They solve different problems.

---

# 52. Real-World Example — Student Counter

```java
class Student {

    static int totalStudents = 0;

    Student() {
        totalStudents++;
    }
}
```

Usage:

```java
Student s1 = new Student();
Student s2 = new Student();
Student s3 = new Student();

System.out.println(Student.totalStudents);
```

Output:

```text
3
```

Why?

Because all objects modify the same static variable.

---

# 53. Real-World Example — Utility Class

A class containing utility methods often uses static methods.

Example:

```java
class MathUtils {

    static int square(int n) {
        return n * n;
    }

    static int cube(int n) {
        return n * n * n;
    }
}
```

Usage:

```java
MathUtils.square(5);
MathUtils.cube(3);
```

No object is required because the methods don't need object-specific state.

---

# 54. Real-World Example — Configuration

```java
class AppConfig {

    static final String APP_NAME = "QRder";
    static final int MAX_ITEMS = 50;
}
```

Usage:

```java
System.out.println(AppConfig.APP_NAME);
System.out.println(AppConfig.MAX_ITEMS);
```

These values conceptually belong to the application configuration rather than a particular object.

---

# 55. Common Interview Traps

### Trap 1

```java
static int x;
```

Does this mean `x` is final?

```text
❌ No
```

Static means class-level.

---

### Trap 2

```java
final int x;
```

Does this mean static?

```text
❌ No
```

Final and static are different concepts.

---

### Trap 3

Can static methods directly access instance variables?

```text
❌ No
```

They need an object/reference.

---

### Trap 4

Can static methods use `this`?

```text
❌ No
```

There is no implicit current object.

---

### Trap 5

Can static methods use `super`?

```text
❌ No
```

No implicit object context exists.

---

### Trap 6

Can static methods be overridden?

```text
❌ No
```

They are hidden.

---

### Trap 7

Can static methods be overloaded?

```text
✅ Yes
```

---

### Trap 8

Does `static` make a variable constant?

```text
❌ No
```

Use `static final` for a class-level constant.

---

### Trap 9

Does static mean thread-safe?

```text
❌ No
```

---

### Trap 10

Does static mean memory is always allocated in one specific JVM memory area?

```text
❌ Do not use this simplistic model.
```

Focus on class-level ownership.

---

# 56. Static vs Final

These two are commonly confused.

| `static`                              | `final`                                                        |
| ------------------------------------- | -------------------------------------------------------------- |
| Controls ownership                    | Controls modification                                          |
| Belongs to class                      | Prevents reassignment/override/inheritance depending on target |
| Object not required for static member | Object requirement depends on member                           |
| Can change unless final               | Can be instance or static                                      |
| Used for class-level members          | Used for restrictions                                          |

Example:

```java
static int count;
```

means:

```text
class-level variable
```

while:

```java
final int count;
```

means:

```text
cannot be reassigned
```

And:

```java
static final int MAX = 100;
```

means:

```text
class-level
+
cannot be reassigned
```

---

# 57. Static vs Instance — Mental Model

Think about a bank.

```text
Bank
 |
 +---- bankName
 |
 +---- totalCustomers
 |
 +---- createAccount()
```

These may be class-level concepts.

But:

```text
Customer
 |
 +---- name
 +---- accountNumber
 +---- balance
```

belong to individual objects.

So:

```text
Shared/common behavior or data
→ static may be appropriate

Object-specific state or behavior
→ instance member
```

---

# 58. 30-Second Interview Answer

> **The `static` keyword makes a member belong to the class rather than individual objects. Static variables are shared at the class level, static methods can be called without creating an object, and static blocks execute during class initialization. Static methods cannot directly access instance members, `this`, or `super`, and static methods are hidden rather than overridden.**

Example:

```java
class Counter {

    static int count = 0;

    Counter() {
        count++;
    }
}

Counter c1 = new Counter();
Counter c2 = new Counter();

System.out.println(Counter.count);
```

Output:

```text
2
```

---

# 🔥 Top 10 Most Important Interview Questions + Answers

## 1. What is the `static` keyword in Java?

**Answer:**

`static` makes a member belong to the class rather than individual objects.

```text
static variable → class-level data
static method   → class-level behavior
static block    → class initialization logic
```

---

## 2. What is the difference between static and instance variables?

**Answer:**

An instance variable belongs to an object, while a static variable belongs to the class.

```java
class Student {

    int id;

    static String college;
}
```

```text
id
→ separate for every object

college
→ shared at class level
```

---

## 3. Can a static method access an instance variable directly?

**Answer:**

No.

```java
class Student {

    int age;

    static void show() {
        System.out.println(age); // ❌
    }
}
```

A static method has no implicit object.

It can access the variable through an explicit object:

```java
Student s = new Student();

System.out.println(s.age);
```

---

## 4. Why can't a static method use `this`?

**Answer:**

`this` represents the current object.

A static method belongs to the class and is not invoked with an implicit object.

Therefore:

```java
static void show() {

    System.out.println(this); // ❌
}
```

is invalid.

---

## 5. Can static methods be overridden?

**Answer:**

No.

Static methods are **hidden**, not overridden.

```java
class Parent {
    static void show() {}
}

class Child extends Parent {
    static void show() {}
}
```

This is method hiding.

---

## 6. Can static methods be overloaded?

**Answer:**

Yes.

```java
class Calculator {

    static int add(int a, int b) {
        return a + b;
    }

    static int add(int a, int b, int c) {
        return a + b + c;
    }
}
```

The methods have different parameter lists.

---

## 7. Why is `main()` static?

**Answer:**

Because the JVM needs to invoke the program's entry-point method without first creating an object of the class.

```java
public static void main(String[] args)
```

`static` allows class-level invocation.

---

## 8. What is a static block?

**Answer:**

A static block is used for class-level initialization.

```java
class Test {

    static {
        System.out.println("Initializing");
    }
}
```

It executes when the class is initialized, before normal execution proceeds into `main()` when `main()` is the trigger for that initialization.

---

## 9. What is the difference between `static` and `static final`?

**Answer:**

```java
static int x = 10;
```

means the variable belongs to the class and can be changed.

```java
static final int x = 10;
```

means it belongs to the class and cannot be reassigned.

```text
static
→ class-level

final
→ restriction on reassignment
```

---

## 10. What is the difference between static method hiding and method overriding?

**Answer:**

Instance methods participate in runtime overriding:

```java
Parent p = new Child();

p.show();
```

If `show()` is overridden, the child's implementation executes.

Static methods do not participate in this runtime overriding mechanism.

```java
Parent p = new Child();

p.show();
```

If `show()` is static in both classes, the method is selected according to the reference/class context.

Therefore:

```text
Instance method
→ overriding
→ runtime polymorphism

Static method
→ hiding
→ not runtime overriding
```

---

# ⭐ Final Revision Cheat Sheet

```text
                    static
                       |
          +------------+------------+
          |            |            |
       Variable      Method       Block
          |            |            |
       Shared       No object    Class
       by class     required     initialization
```

### Static Variable

```java
static int count;
```

```text
→ One class-level field
→ Shared by objects
```

### Static Method

```java
static void show() {}
```

```text
→ Call using ClassName.show()
→ No implicit this
→ Cannot directly access instance members
→ Can be overloaded
→ Cannot be overridden
```

### Static Block

```java
static {
}
```

```text
→ Class initialization
→ Executes when class is initialized
```

### Static Nested Class

```java
class Outer {

    static class Inner {
    }
}
```

```text
→ Does not require Outer object
```

### Static + Final

```java
static final int MAX = 100;
```

```text
→ Class-level constant
```

---

# 🧠 Ultimate Memory Trick

> **`static` = "This belongs to the CLASS, not to a particular OBJECT."**

```text
static variable
      ↓
shared class-level data

static method
      ↓
class-level behavior

static block
      ↓
class initialization

static nested class
      ↓
nested type not tied to an Outer object
```

And remember the biggest interview distinction:

```text
INSTANCE METHOD
      ↓
can override
      ↓
runtime polymorphism

STATIC METHOD
      ↓
cannot override
      ↓
method hiding
```
