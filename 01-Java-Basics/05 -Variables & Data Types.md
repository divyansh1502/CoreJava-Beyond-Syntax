# ☕ 05 — Variables & Data Types

> Variables store data, while data types define what kind of data can be stored and how that data is interpreted by Java.

---

# 1. What is a Variable?

A **variable** is a named storage location used to hold a value.

```java
int age = 22;
String name = "Divyansh";
```

Here:

```text
int  → Data Type
age  → Variable Name
22   → Value
=    → Assignment Operator
```

---

# 2. Variable Declaration vs Initialization

### Declaration

```java
int age;
```

The variable is declared but no value is assigned.

### Initialization

```java
age = 22;
```

A value is assigned.

### Declaration + Initialization

```java
int age = 22;
```

---

# 3. Types of Variables

Java has three main types of variables:

```text
Variables
│
├── Local Variable
├── Instance Variable
└── Static Variable
```

---

## 3.1 Local Variable

Declared inside a method, constructor, or block.

```java
void display() {

    int age = 22;

    System.out.println(age);
}
```

### Important

Local variables:

* Have method/block scope
* Do not receive default values
* Must be initialized before use

```java
void test() {

    int x;

    System.out.println(x); // Compile-time error
}
```

---

## 3.2 Instance Variable

Declared inside a class but outside methods, without `static`.

```java
class Student {

    int age;
    String name;
}
```

Each object gets its own copy.

```java
Student s1 = new Student();
Student s2 = new Student();

s1.age = 20;
s2.age = 25;
```

Conceptually:

```text
s1 → age = 20
s2 → age = 25
```

Instance variables receive **default values**.

---

## 3.3 Static Variable

Declared using `static`.

```java
class Student {

    static String college = "AIET";
}
```

A static variable belongs to the **class**, rather than to each individual object.

```java
Student.college
```

There is normally one class-level variable shared by instances.

---

# 4. Default Values

Instance and static variables receive default values.

| Data Type       | Default Value |
| --------------- | ------------- |
| `byte`          | `0`           |
| `short`         | `0`           |
| `int`           | `0`           |
| `long`          | `0L`          |
| `float`         | `0.0f`        |
| `double`        | `0.0d`        |
| `char`          | `'\u0000'`    |
| `boolean`       | `false`       |
| Reference types | `null`        |

Local variables **do not get automatic default values**.

---

# 5. Data Types

Java is a **statically typed language**.

The type of a variable is known at compile time.

```java
int age = 22;
```

You cannot directly assign a `String` to an `int`.

```java
int age = "22"; // Compile-time error
```

---

# 6. Primitive Data Types

Java has **8 primitive data types**.

```text
Primitive
│
├── Integer
│   ├── byte
│   ├── short
│   ├── int
│   └── long
│
├── Floating Point
│   ├── float
│   └── double
│
├── Character
│   └── char
│
└── Boolean
    └── boolean
```

---

## 6.1 Integer Types

| Type    |   Size | Approximate Range |
| ------- | -----: | ----------------- |
| `byte`  |  8-bit | -128 to 127       |
| `short` | 16-bit | -32,768 to 32,767 |
| `int`   | 32-bit | -2³¹ to 2³¹-1     |
| `long`  | 64-bit | -2⁶³ to 2⁶³-1     |

Example:

```java
byte b = 100;
short s = 1000;
int i = 100000;
long l = 10000000000L;
```

### Interview point

An integer literal such as:

```java
10
```

is normally treated as an `int`.

For a `long` literal:

```java
10L
```

---

# 7. Floating-Point Types

```java
float price = 99.5f;
double salary = 50000.75;
```

### Important

A decimal literal is normally a `double`.

Therefore:

```java
float x = 10.5; // Error
```

Correct:

```java
float x = 10.5f;
```

For most general-purpose calculations:

```java
double
```

is preferred over `float` because it provides greater precision.

---

# 8. char

`char` represents a single UTF-16 code unit.

```java
char grade = 'A';
```

Single quotes:

```java
'A'
```

Double quotes:

```java
"A"
```

represent different types:

```text
'A' → char
"A" → String
```

Example:

```java
char ch = 65;

System.out.println(ch);
```

Output:

```text
A
```

This happens because `65` corresponds to `A` in Unicode.

---

# 9. boolean

```java
boolean isLoggedIn = true;
boolean isAdmin = false;
```

Java's `boolean` has two logical values:

```text
true
false
```

Unlike C/C++, Java does not treat:

```java
1
0
```

as boolean values.

```java
boolean x = 1; // Compile-time error
```

---

# 10. Reference Data Types

Reference variables store references to objects rather than directly storing object data.

Examples:

```java
String name = "Divyansh";

Student student = new Student();

int[] numbers = new int[5];
```

Common reference types include:

```text
Class
Interface
Array
Enum
String
```

A reference variable can contain:

```java
null
```

Example:

```java
Student s = null;
```

---

# 11. Primitive vs Reference Type

| Primitive                | Reference                       |
| ------------------------ | ------------------------------- |
| `int`                    | `String`                        |
| `double`                 | `Student`                       |
| `char`                   | `int[]`                         |
| `boolean`                | `ArrayList`                     |
| Stores a primitive value | Stores a reference to an object |
| 8 primitive types        | Many reference types            |
| Cannot be `null`         | Can be `null`                   |

Example:

```java
int x = 10;

Student s = new Student();
```

---

# 12. Type Casting

Type casting means converting one data type into another compatible type.

There are two major types:

```text
Type Casting
│
├── Widening
└── Narrowing
```

---

# 13. Widening Casting

Smaller compatible numeric type → larger compatible numeric type.

```java
int x = 100;

long y = x;
```

Conceptually:

```text
int
 ↓
long
```

No explicit cast is normally required.

Another example:

```java
int x = 10;

double y = x;
```

---

# 14. Narrowing Casting

Larger numeric type → smaller numeric type.

Explicit casting is required.

```java
double x = 10.5;

int y = (int) x;
```

Result:

```text
10
```

The fractional part is discarded.

---

# 15. Numeric Overflow

When a value exceeds the range of an integer type, overflow can occur.

```java
int x = Integer.MAX_VALUE;

x++;

System.out.println(x);
```

Result:

```text
-2147483648
```

Why?

Because Java's signed integer arithmetic uses fixed-width two's-complement representation.

---

# 16. final Variable

`final` prevents reassignment after initialization.

```java
final int MAX_AGE = 100;
```

This is invalid:

```java
MAX_AGE = 200;
```

Compile-time error.

### Important

`final` variable ≠ immutable object.

Example:

```java
final ArrayList<Integer> list = new ArrayList<>();

list.add(10); // Allowed
```

The reference cannot point to another object:

```java
list = new ArrayList<>(); // Not allowed
```

But the referenced object's state may still change.

---

# 17. var

Java supports local variable type inference using `var`.

```java
var age = 22;
var name = "Divyansh";
```

The compiler determines the type.

Conceptually:

```java
var age = 22;
```

becomes a variable whose inferred type is `int`.

### Important

`var` does **not** mean dynamically typed.

This is still statically typed:

```java
var age = 22;

// age = "Hello"; // Compile-time error
```

### Restrictions

`var` can be used for local variables but not as a general replacement for every type declaration.

For example:

```java
var x = 10; // valid local variable
```

But:

```java
class Test {

    var x = 10; // invalid as an instance field
}
```

---

# 18. Literals

A literal is a fixed value written directly in source code.

Examples:

```java
10
10L
10.5
10.5f
'A'
"Hello"
true
false
```

Examples:

```java
int x = 10;
long y = 10L;
float z = 10.5f;
char c = 'A';
String s = "Hello";
boolean b = true;
```

---

# 19. Important Literal Interview Points

### Binary

```java
int x = 0b1010;
```

### Octal

```java
int x = 012;
```

### Hexadecimal

```java
int x = 0xA;
```

All represent:

```text
10
```

---

# 20. Underscores in Numeric Literals

Java allows underscores for readability.

```java
int population = 1_000_000;
long amount = 10_000_000_000L;
```

The underscores have no effect on the actual value.

---

# 21. Variable Scope

Scope defines where a variable can be accessed.

```java
class Test {

    int instanceVariable = 10;

    static int staticVariable = 20;

    void method() {

        int localVariable = 30;

        System.out.println(instanceVariable);
        System.out.println(staticVariable);
        System.out.println(localVariable);
    }
}
```

Conceptually:

```text
Local variable
→ Method/block scope

Instance variable
→ Object scope

Static variable
→ Class scope
```

---

# 22. Variable Shadowing

A local variable can have the same name as an instance variable.

```java
class Student {

    int age = 20;

    void display() {

        int age = 25;

        System.out.println(age);
    }
}
```

Output:

```text
25
```

The local variable shadows the instance variable.

Use `this` to access the instance variable:

```java
class Student {

    int age = 20;

    void display() {

        int age = 25;

        System.out.println(age);
        System.out.println(this.age);
    }
}
```

Output:

```text
25
20
```

---

# 23. Important Interview Traps

## Trap 1 — Local variables have default values

Wrong:

```java
void test() {

    int x;

    System.out.println(x);
}
```

This does **not** compile.

---

## Trap 2 — `long` accepts every integer literal

Not necessarily.

```java
long x = 2147483648;
```

This fails because the literal itself is treated as an `int` unless represented appropriately.

Correct:

```java
long x = 2147483648L;
```

---

## Trap 3 — Decimal literals are float

Wrong:

```java
float x = 10.5;
```

Correct:

```java
float x = 10.5f;
```

---

## Trap 4 — `final` makes objects immutable

False.

```java
final ArrayList<Integer> list = new ArrayList<>();

list.add(10); // allowed
```

`final` prevents reassignment of the reference.

---

## Trap 5 — `var` is dynamic typing

False.

```java
var x = 10;
```

The type is inferred at compile time.

---

# 24. Interview Questions

## Q1. What are the 8 primitive data types in Java?

```text
byte
short
int
long
float
double
char
boolean
```

---

## Q2. What is the difference between primitive and reference types?

Primitive variables represent primitive values, while reference variables refer to objects.

---

## Q3. What is the default value of a local variable?

There is **no default value** for local variables. They must be definitely initialized before use.

---

## Q4. What is the default value of an instance variable?

It receives the default value associated with its type.

For example:

```java
int → 0
boolean → false
reference → null
```

---

## Q5. What is widening and narrowing?

```text
Widening
Smaller → Larger
Usually automatic

Narrowing
Larger → Smaller
Explicit cast required
```

Example:

```java
int x = 10;
double y = x;       // widening

double a = 10.5;
int b = (int) a;    // narrowing
```

---

## Q6. Why do we use `L` with long?

Because integer literals are normally `int` by default.

```java
long x = 100L;
```

The `L` explicitly represents a `long` literal.

---

## Q7. Why do we use `f` with float?

Because decimal literals are normally `double`.

```java
float x = 10.5f;
```

The `f` makes it a `float` literal.

---

## Q8. What is the difference between `final` and immutable?

`final` prevents reassignment of a variable/reference.

Immutability means the object's state cannot be changed after creation.

They are related concepts but **not the same thing**.

---

## Q9. Is Java statically or dynamically typed?

Java is **statically typed**.

Types are determined and checked at compile time.

---

## Q10. Is `var` dynamically typed?

No.

`var` performs **local variable type inference** while Java remains statically typed.

---

# 🔥 Top 10 Must-Know Interview Questions

```text
1. What are the 8 primitive data types?
2. Primitive vs reference type?
3. Local vs instance vs static variable?
4. Why don't local variables have default values?
5. What are widening and narrowing conversions?
6. Why is `10.5` double by default?
7. Why do we use L and F suffixes?
8. What is variable shadowing?
9. Is var dynamically typed?
10. Does final make an object immutable?
```

---

# 🎯 30-Second Interview Answer

> Java variables are used to store data and every variable has a type. Java provides eight primitive data types: byte, short, int, long, float, double, char, and boolean. Apart from primitives, Java also has reference types that refer to objects. Variables can be local, instance, or static depending on their scope and ownership. Java supports automatic widening conversions and explicit narrowing conversions. The `final` keyword prevents reassignment, while `var` provides compile-time local variable type inference without making Java dynamically typed.

---

# ⚡ Quick Revision

```text
VARIABLE
↓
Named storage/reference used to hold data

3 VARIABLE TYPES
↓
Local
Instance
Static

8 PRIMITIVES
↓
byte
short
int
long
float
double
char
boolean

CASTING
↓
Widening  → automatic
Narrowing → explicit

DEFAULT VALUES
↓
Instance/Static → yes
Local           → no

FINAL
↓
Prevents reassignment

VAR
↓
Compile-time local type inference

JAVA
↓
Statically Typed
```

---

# 🧠 Remember

```text
"Primitive stores the value.
Reference points to the object."

"Local must be initialized.
Instance and static get defaults."

"Widening is generally automatic.
Narrowing needs casting."

"final reference ≠ immutable object."

"var ≠ dynamic typing."
```
