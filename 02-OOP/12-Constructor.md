# 🧱 Constructor in Java

> A **constructor** is a special member of a class used to initialize the state of an object. It has the same name as the class, has no return type, and is invoked during object creation.

---

## 📌 Table of Contents

1. [What is a Constructor?](#-what-is-a-constructor)
2. [Why Do We Need Constructors?](#-why-do-we-need-constructors)
3. [Constructor Syntax](#-constructor-syntax)
4. [Basic Example](#-basic-example)
5. [How Constructor Invocation Works](#-how-constructor-invocation-works)
6. [Types of Constructors](#-types-of-constructors)
7. [No-Argument Constructor](#-no-argument-constructor)
8. [Default Constructor](#-default-constructor)
9. [Parameterized Constructor](#-parameterized-constructor)
10. [Constructor Overloading](#-constructor-overloading)
11. [Constructor Chaining](#-constructor-chaining)
12. [`this()` Constructor Call](#-this-constructor-call)
13. [`super()` Constructor Call](#-super-constructor-call)
14. [Constructor and Inheritance](#-constructor-and-inheritance)
15. [Constructor Execution Order](#-constructor-execution-order)
16. [Access Modifiers](#-access-modifiers)
17. [Private Constructor](#-private-constructor)
18. [Constructor vs Method](#-constructor-vs-method)
19. [Constructor and Instance Variables](#-constructor-and-instance-variables)
20. [Constructor and Object Creation](#-constructor-and-object-creation)
21. [JVM / Internal Working](#-jvm--internal-working)
22. [Can Constructor Be Inherited?](#-can-constructor-be-inherited)
23. [Can Constructor Be Overridden?](#-can-constructor-be-overridden)
24. [Can Constructor Be `static`, `final`, or `abstract`?](#-can-constructor-be-static-final-or-abstract)
25. [Common Mistakes](#-common-mistakes)
26. [Interview Traps](#-interview-traps)
27. [Pros and Limitations](#-pros-and-limitations)
28. [Top 10 Interview Questions](#-top-10-interview-questions)
29. [30-Second Interview Answer](#-30-second-interview-answer)
30. [Cheat Sheet](#-cheat-sheet)

---

# 🔹 What is a Constructor?

A constructor is a special member of a class that is primarily used to initialize the **state of an object** when the object is created.

Example:

```java
class Student {

    String name;
    int age;

    Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

Object creation:

```java
Student s = new Student("Divyansh", 22);
```

The constructor initializes:

```text
name → Divyansh
age  → 22
```

---

# 🎯 Why Do We Need Constructors?

Without explicitly initializing fields, instance variables receive their default values.

```java
class Student {

    String name;
    int age;
}

Student s = new Student();

System.out.println(s.name); // null
System.out.println(s.age);  // 0
```

A constructor lets us initialize the object with meaningful values:

```java
class Student {

    String name;
    int age;

    Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

Now:

```java
Student s = new Student("Divyansh", 22);
```

The object starts with a valid state.

### Main purpose

```text
Object Creation
      ↓
Constructor
      ↓
Initialize Object State
      ↓
Ready-to-use Object
```

---

# 🧩 Constructor Syntax

```java
class ClassName {

    ClassName() {
        // initialization
    }
}
```

Example:

```java
class Car {

    String brand;

    Car() {
        brand = "Toyota";
    }
}
```

---

# 🧪 Basic Example

```java
class Employee {

    String name;
    int salary;

    Employee(String name, int salary) {
        this.name = name;
        this.salary = salary;
    }

    void display() {
        System.out.println(name);
        System.out.println(salary);
    }
}

public class Main {

    public static void main(String[] args) {

        Employee e = new Employee("Rahul", 50000);

        e.display();
    }
}
```

Output:

```text
Rahul
50000
```

---

# ⚙️ How Constructor Invocation Works

Consider:

```java
Student s = new Student("Divyansh", 22);
```

Conceptually, the process is:

```text
new Student(...)
       ↓
Memory for object is allocated
       ↓
Instance fields receive default values
       ↓
Constructor is invoked
       ↓
Constructor initializes fields
       ↓
Reference is assigned to s
```

### Important clarification

The constructor **does not create the object by itself**.

The `new` expression:

```java
new Student(...)
```

causes object creation/allocation and invokes the matching constructor.

The constructor's job is primarily to initialize the newly created object's state.

---

# 🔢 Types of Constructors

Commonly discussed types:

### 1. No-Argument Constructor

A constructor that accepts no parameters.

```java
Student() {
}
```

### 2. Parameterized Constructor

A constructor that accepts parameters.

```java
Student(String name, int age) {
    this.name = name;
    this.age = age;
}
```

### 3. Compiler-Provided Default Constructor

If no constructor is declared in the class, the compiler provides a default no-argument constructor.

```java
class Student {
}
```

The compiler provides constructor behavior equivalent in basic terms to:

```java
Student() {
    super();
}
```

---

# 🟢 No-Argument Constructor

A no-argument constructor has zero parameters.

```java
class Student {

    Student() {
        System.out.println("Constructor called");
    }
}
```

Usage:

```java
Student s = new Student();
```

---

# ⚠️ Default Constructor

A **default constructor** is specifically the no-argument constructor supplied by the compiler when the programmer declares **no constructor**.

Example:

```java
class Student {
}
```

The compiler supplies a default constructor.

### Important

If you declare:

```java
class Student {

    Student(String name) {
    }
}
```

Java will **not** automatically provide:

```java
Student() {
}
```

Therefore:

```java
Student s = new Student(); // ❌ Compile-time error
```

---

# 🧱 Parameterized Constructor

A constructor with parameters is called a parameterized constructor.

```java
class Student {

    String name;
    int age;

    Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

Usage:

```java
Student s1 = new Student("Amit", 21);
Student s2 = new Student("Rahul", 22);
```

Each object can have different initial state.

---

# 🔄 Constructor Overloading

A class can have multiple constructors with different parameter lists.

```java
class Student {

    String name;
    int age;

    Student() {
        name = "Unknown";
        age = 0;
    }

    Student(String name) {
        this.name = name;
        age = 0;
    }

    Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

This is **constructor overloading**.

### Rule

Constructors must have different parameter lists.

These are valid:

```java
Student()
Student(String)
Student(String, int)
Student(int)
```

These are invalid:

```java
Student(String)
Student(String) // ❌ duplicate
```

---

# 🔗 Constructor Chaining

Constructor chaining means one constructor calls another constructor.

There are two major forms:

```text
this()
 ↓
Another constructor in SAME class

super()
 ↓
Constructor of PARENT class
```

---

# 🔄 `this()` Constructor Call

`this()` is used to invoke another constructor of the **same class**.

Example:

```java
class Student {

    String name;
    int age;

    Student() {
        this("Unknown", 0);
    }

    Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

When:

```java
Student s = new Student();
```

execution becomes:

```text
Student()
   ↓
this("Unknown", 0)
   ↓
Student(String, int)
   ↓
Fields initialized
```

### Important rule

`this()` must be the **first statement** inside the constructor.

Correct:

```java
Student() {
    this("Unknown", 0);
}
```

Incorrect:

```java
Student() {
    System.out.println("Hello");
    this("Unknown", 0); // ❌
}
```

---

# 🧬 `super()` Constructor Call

`super()` invokes a constructor of the parent class.

```java
class Animal {

    Animal() {
        System.out.println("Animal constructor");
    }
}

class Dog extends Animal {

    Dog() {
        super();
        System.out.println("Dog constructor");
    }
}
```

Creating:

```java
Dog d = new Dog();
```

Output:

```text
Animal constructor
Dog constructor
```

### Important

If you don't explicitly write `super()` in a constructor, Java inserts an implicit call to the accessible no-argument parent constructor, when applicable.

Conceptually:

```java
Dog() {
    super();
}
```

---

# 🧬 Constructor and Inheritance

Constructors are **not inherited** by child classes.

Example:

```java
class Parent {

    Parent(int x) {
    }
}

class Child extends Parent {

    Child() {
        super(10);
    }
}
```

The child does not inherit:

```java
Parent(int)
```

Instead, its constructor explicitly invokes it using:

```java
super(10);
```

---

# 🔢 Constructor Execution Order

Consider:

```java
class A {

    A() {
        System.out.println("A");
    }
}

class B extends A {

    B() {
        System.out.println("B");
    }
}

class C extends B {

    C() {
        System.out.println("C");
    }
}
```

Now:

```java
C obj = new C();
```

Output:

```text
A
B
C
```

### Why?

Construction proceeds from the parent part of the object toward the child part:

```text
Object
  ↓
A
  ↓
B
  ↓
C
```

A useful interview statement:

> **Parent constructor executes before child constructor.**

---

# 🔐 Access Modifiers

Constructors can have:

### `public`

```java
public Student() {
}
```

Accessible from anywhere where the class itself is accessible.

### `protected`

```java
protected Student() {
}
```

Accessible within the package and through inheritance rules.

### Package-private

```java
Student() {
}
```

Accessible within the same package.

### `private`

```java
private Student() {
}
```

Accessible only inside the class.

---

# 🔒 Private Constructor

A private constructor prevents normal construction from outside the class.

```java
class Test {

    private Test() {
    }
}
```

This is invalid:

```java
Test t = new Test(); // ❌
```

because the constructor is private.

### Why use private constructors?

Common uses include:

* Singleton patterns
* Utility classes
* Preventing object creation
* Factory-based object creation

Example utility-style class:

```java
class MathUtil {

    private MathUtil() {
    }

    static int square(int x) {
        return x * x;
    }
}
```

The class can provide static utility methods without allowing normal object creation.

---

# 🆚 Constructor vs Method

| Feature     | Constructor             | Method                            |
| ----------- | ----------------------- | --------------------------------- |
| Name        | Same as class           | Can have any valid name           |
| Return type | No return type          | Must have a return type or `void` |
| Invocation  | During object creation  | Explicitly called                 |
| Purpose     | Initialize object state | Perform behavior/operation        |
| Overloading | ✅ Yes                   | ✅ Yes                             |
| Overriding  | ❌ No                    | ✅ Yes                             |
| Inherited   | ❌ No                    | Methods can be inherited          |
| `static`    | ❌ No                    | ✅ Yes                             |
| `final`     | ❌ No                    | ✅ Yes                             |
| `abstract`  | ❌ No                    | ✅ Yes                             |

---

# 🎯 Constructor and Instance Variables

Constructors are frequently used to initialize instance variables.

```java
class Employee {

    String name;
    int salary;

    Employee(String name, int salary) {

        this.name = name;
        this.salary = salary;
    }
}
```

Here:

```java
this.name = name;
```

means:

```text
this.name → instance variable
name      → constructor parameter
```

Without `this`, the names can become ambiguous.

---

# 🧠 Constructor and Object Creation

Remember this statement:

```java
Student s = new Student("A", 20);
```

It contains multiple concepts:

```text
Student
   ↓
Reference type

s
   ↓
Reference variable

new
   ↓
Creates/allocates a new object

Student("A", 20)
   ↓
Invokes matching constructor
```

The constructor initializes the newly created object's state.

---

# ⚙️ JVM / Internal Working

A simplified conceptual sequence for:

```java
Student s = new Student("Amit", 22);
```

is:

```text
1. Class information must be available to the JVM
                ↓
2. Memory is allocated for the object
                ↓
3. Instance fields receive default values
                ↓
4. Constructor invocation begins
                ↓
5. Parent construction is handled first
                ↓
6. Current class constructor executes
                ↓
7. Instance fields are initialized according to constructor logic
                ↓
8. Reference points to the created object
```

### Important distinction

Do not say:

> "Constructor allocates memory."

Better:

> **"The `new` expression is responsible for creating/allocating the object, while the constructor initializes the object's state."**

The exact JVM implementation details are more involved, but this is the correct conceptual model for interviews.

---

# ❌ Can Constructor Be Inherited?

**No.**

Constructors are not inherited by subclasses.

```java
class Parent {

    Parent() {
    }
}

class Child extends Parent {
}
```

`Child` does not inherit `Parent()` as a constructor.

The child has its own construction process.

---

# ❌ Can Constructor Be Overridden?

**No.**

Constructor overriding is impossible because constructors are not inherited.

```java
class Parent {
    Parent() {}
}

class Child extends Parent {
    Child() {}
}
```

This is not overriding.

It is simply two separate constructors belonging to two different classes.

---

# ❌ Can Constructor Be `static`, `final`, or `abstract`?

### `static`

❌ Not allowed.

Constructors initialize objects, while `static` members belong to the class rather than a particular object.

### `final`

❌ Not allowed.

`final` prevents overriding, but constructors cannot be overridden anyway.

### `abstract`

❌ Not allowed.

An abstract constructor would not make sense because constructors are used during actual object construction.

---

# 🧠 Common Mistakes

### Mistake 1: Giving constructor a return type

```java
void Student() {
}
```

This is **not a constructor**.

It is a method named `Student`.

Correct:

```java
Student() {
}
```

---

### Mistake 2: Thinking constructor creates the object

Not exactly.

```java
new Student();
```

The `new` expression creates/allocates the object and invokes the constructor.

---

### Mistake 3: Thinking constructor is inherited

❌ Constructors are not inherited.

---

### Mistake 4: Thinking constructors can be overridden

❌ They cannot.

---

### Mistake 5: Forgetting constructor availability

```java
class Student {

    Student(String name) {
    }
}

Student s = new Student(); // ❌
```

A no-argument constructor was not automatically created because a constructor was already declared.

---

### Mistake 6: Putting `this()` somewhere other than first

```java
Student() {

    System.out.println("Hello");

    this("Unknown"); // ❌
}
```

`this()` must be the first statement.

---

### Mistake 7: Putting `super()` somewhere other than first

```java
Child() {

    System.out.println("Child");

    super(); // ❌
}
```

`super()` must be the first statement.

---

# ⚠️ Interview Traps

### Trap 1

**Can a constructor have `void` as return type?**

No.

```java
void Student() {}
```

is a method.

---

### Trap 2

**Can a constructor be overloaded?**

Yes.

---

### Trap 3

**Can a constructor be overridden?**

No.

---

### Trap 4

**Can constructors be inherited?**

No.

---

### Trap 5

**Can a constructor be private?**

Yes.

---

### Trap 6

**Can a constructor be static?**

No.

---

### Trap 7

**What happens if no constructor is written?**

The compiler provides a default no-argument constructor, subject to normal class construction rules.

---

### Trap 8

**What happens if a parameterized constructor is written but no no-argument constructor is written?**

A no-argument constructor is not automatically provided.

---

# ✅ Advantages and Limitations

## Advantages

* Initializes objects during construction.
* Helps establish a valid initial object state.
* Supports constructor overloading.
* Supports constructor chaining.
* Can enforce required initialization through parameters.
* Can restrict object creation using private constructors.

## Limitations

* Constructors cannot be inherited.
* Constructors cannot be overridden.
* Constructor logic should generally focus on initialization rather than large business operations.
* A poorly designed constructor can make object creation unnecessarily complicated.

---

# 🔥 Top 10 Constructor Interview Questions

## 1. What is a constructor?

A constructor is a special member of a class used to initialize an object's state. It has the same name as the class and has no return type.

---

## 2. Why does a constructor not have a return type?

A constructor is not a normal method that returns a value. Its role is to initialize an object during construction.

---

## 3. Is a constructor automatically called?

A constructor is invoked as part of object creation when an object is created using an appropriate constructor invocation such as:

```java
new Student();
```

---

## 4. Can constructors be overloaded?

Yes.

```java
Student() {}
Student(String name) {}
Student(String name, int age) {}
```

---

## 5. Can constructors be overridden?

No. Constructors are not inherited, so constructor overriding is not possible.

---

## 6. Can constructors be inherited?

No.

A subclass can invoke a superclass constructor using `super()`, but it does not inherit the constructor itself.

---

## 7. What is the difference between default and no-argument constructor?

A no-argument constructor is any constructor with zero parameters.

A default constructor specifically refers to the no-argument constructor supplied by the compiler when no constructor is declared.

---

## 8. What is constructor chaining?

Constructor chaining means one constructor invokes another constructor.

Same class:

```java
this();
```

Parent class:

```java
super();
```

---

## 9. Can a constructor be private?

Yes.

Private constructors are commonly used to restrict object creation, including in singleton or utility-style designs.

---

## 10. Can a constructor be static, final, or abstract?

No.

Constructors cannot be:

```java
static
final
abstract
```

---

# 🎤 30-Second Interview Answer

> **A constructor is a special member of a class used to initialize the state of an object. It has the same name as the class and does not have a return type, not even `void`. Constructors are invoked during object creation and can be parameterized, overloaded, and chained using `this()` or `super()`. Constructors are not inherited and cannot be overridden. Java provides a default constructor only when no constructor is explicitly declared.**

---

# 🧾 Cheat Sheet

```text
╔══════════════════════════════════════════════╗
║              CONSTRUCTOR CHEAT SHEET        ║
╠══════════════════════════════════════════════╣
║ Purpose       → Initialize object state      ║
║ Name          → Same as class               ║
║ Return type   → None                         ║
║ Invocation    → During object construction   ║
║ Overloading   → Yes                           ║
║ Overriding    → No                            ║
║ Inheritance   → No                            ║
║ static        → Not allowed                   ║
║ final         → Not allowed                   ║
║ abstract      → Not allowed                   ║
║ private       → Allowed                       ║
║ this()        → Same-class constructor       ║
║ super()       → Parent constructor           ║
║ this()/super()→ Must be first statement      ║
╚══════════════════════════════════════════════╝
```

### 🧠 Memory Trick

```text
Constructor = SAME + NO RETURN + INITIALIZE

SAME
→ Same name as class

NO RETURN
→ No return type

INITIALIZE
→ Initializes object state
```

### Constructor Flow

```text
new ClassName(...)
        ↓
Object creation/allocation
        ↓
Default field initialization
        ↓
Parent constructor
        ↓
Current constructor
        ↓
Object initialized
```

---

# 🚀 Quick Revision

```java
class Student {

    String name;
    int age;

    Student() {
        this("Unknown", 0);
    }

    Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

```java
Student s = new Student("Divyansh", 22);
```

Remember:

```text
new
 ↓
creates object
 ↓
constructor invoked
 ↓
object state initialized
```

> **Interview golden line:**
> A constructor doesn't return a value and isn't inherited or overridden; its primary purpose is to initialize the state of a newly constructed object.
