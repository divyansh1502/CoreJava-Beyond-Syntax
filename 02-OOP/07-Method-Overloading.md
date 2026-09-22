# ☕ Java OOP — Method Overloading

> **Method Overloading means defining multiple methods with the same name in the same class, but with different parameter lists.**

---

# 📚 Table of Contents

- [1. What is Method Overloading?](#1-what-is-method-overloading)
- [2. Why Do We Need Method Overloading?](#2-why-do-we-need-method-overloading)
- [3. Rules of Method Overloading](#3-rules-of-method-overloading)
- [4. Overloading by Number of Parameters](#4-overloading-by-number-of-parameters)
- [5. Overloading by Parameter Type](#5-overloading-by-parameter-type)
- [6. Overloading by Order of Parameters](#6-overloading-by-order-of-parameters)
- [7. What is NOT Method Overloading?](#7-what-is-not-method-overloading)
- [8. Method Signature](#8-method-signature)
- [9. Is Method Name Part of the Signature?](#9-is-method-name-part-of-the-signature)
- [10. Method Overloading is Compile-Time Polymorphism](#10-method-overloading-is-compile-time-polymorphism)
- [11. How Does the Compiler Choose an Overloaded Method?](#11-how-does-the-compiler-choose-an-overloaded-method)
- [12. Exact Match Has Priority](#12-exact-match-has-priority)
- [13. Primitive Widening During Overloading](#13-primitive-widening-during-overloading)
- [14. Primitive Widening Order](#14-primitive-widening-order)
- [15. Widening vs Narrowing](#15-widening-vs-narrowing)
- [16. Overloading with Boxing](#16-overloading-with-boxing)
- [17. Widening vs Boxing](#17-widening-vs-boxing)
- [18. Boxing vs Varargs](#18-boxing-vs-varargs)
- [19. Varargs and Overloading](#19-varargs-and-overloading)
- [20. Important Overload Resolution Order](#20-important-overload-resolution-order)
- [21. Overloading with null](#21-overloading-with-null)
- [22. null with Parent and Child](#22-null-with-parent-and-child)
- [23. Ambiguous null Example](#23-ambiguous-null-example)
- [24. Overloading and Inheritance](#24-overloading-and-inheritance)
- [25. Overloading vs Overriding](#25-overloading-vs-overriding)
- [26. Can main() Be Overloaded?](#26-can-main-be-overloaded)
- [27. Can Constructors Be Overloaded?](#27-can-constructors-be-overloaded)
- [28. Constructor Overloading vs Method Overloading](#28-constructor-overloading-vs-method-overloading)
- [29. Can Static Methods Be Overloaded?](#29-can-static-methods-be-overloaded)
- [30. Can Private Methods Be Overloaded?](#30-can-private-methods-be-overloaded)
- [31. Can Final Methods Be Overloaded?](#31-can-final-methods-be-overloaded)
- [32. Return Type and Overloading](#32-return-type-and-overloading)
- [33. Access Modifiers and Overloading](#33-access-modifiers-and-overloading)
- [34. Generic Methods and Overloading](#34-generic-methods-and-overloading)
- [35. Overloading and Compile-Time Type](#35-overloading-and-compile-time-type)
- [36. Overloading + Overriding Together](#36-overloading--overriding-together)
- [37. Real-World Example](#37-real-world-example)
- [38. Advantages of Method Overloading](#38-advantages-of-method-overloading)
- [39. Disadvantages / Limitations](#39-disadvantages--limitations)
- [40. Common Interview Traps](#40-common-interview-traps)
- [41. Important Interview Questions](#41-important-interview-questions)
- [42. Quick Revision Cheat Sheet](#42-quick-revision-cheat-sheet)
- [43. Most Important Conversion Priority](#43-most-important-conversion-priority)
- [44. The Golden Difference](#44-the-golden-difference)
- [45. 30-Second Interview Answer](#45-30-second-interview-answer)
- [46. Final Memory Trick](#46-final-memory-trick)

---

# 1. What is Method Overloading?

Method overloading allows a class to have multiple methods with the same name but different parameters.

Example:

```java
class Calculator {

    int add(int a, int b) {
        return a + b;
    }

    int add(int a, int b, int c) {
        return a + b + c;
    }

    double add(double a, double b) {
        return a + b;
    }
}
```

Here, all methods have the same name:

```text
add()
```

But their parameter lists are different:

```text
add(int, int)
add(int, int, int)
add(double, double)
```

This is **method overloading**.

---

# 2. Why Do We Need Method Overloading?

Suppose we want to add different numbers.

Without overloading:

```java
int addTwoNumbers(int a, int b) {
    return a + b;
}

int addThreeNumbers(int a, int b, int c) {
    return a + b + c;
}
```

We need different method names.

With overloading:

```java
int add(int a, int b) {
    return a + b;
}

int add(int a, int b, int c) {
    return a + b + c;
}
```

Now the method name represents the same logical operation:

```text
add()
```

while the parameters determine which version is required.

This improves:

```text
Readability
Consistency
API design
Code organization
```

---

# 3. Rules of Method Overloading

Methods can be overloaded by changing the:

```text
1. Number of parameters
2. Type of parameters
3. Order of parameters
```

Example:

```java
class Demo {

    void show(int a) {
    }

    void show(int a, int b) {
    }

    void show(double a) {
    }

    void show(int a, double b) {
    }

    void show(double a, int b) {
    }
}
```

All of these are valid overloads.

---

# 4. Overloading by Number of Parameters

```java
class Calculator {

    void add(int a, int b) {
        System.out.println(a + b);
    }

    void add(int a, int b, int c) {
        System.out.println(a + b + c);
    }
}
```

Calling:

```java
Calculator c = new Calculator();

c.add(10, 20);

c.add(10, 20, 30);
```

The compiler selects the appropriate method based on the number of arguments.

---

# 5. Overloading by Parameter Type

```java
class Printer {

    void print(int value) {
        System.out.println("Integer");
    }

    void print(double value) {
        System.out.println("Double");
    }

    void print(String value) {
        System.out.println("String");
    }
}
```

Now:

```java
Printer p = new Printer();

p.print(10);
p.print(10.5);
p.print("Hello");
```

Output:

```text
Integer
Double
String
```

---

# 6. Overloading by Order of Parameters

Parameter order can also create a valid overload.

```java
class Demo {

    void show(int a, double b) {
        System.out.println("int-double");
    }

    void show(double a, int b) {
        System.out.println("double-int");
    }
}
```

These are different signatures:

```text
show(int, double)
show(double, int)
```

Therefore, they can coexist.

---

# 7. What is NOT Method Overloading?

Changing only the return type is **not enough**.

This is illegal:

```java
class Demo {

    int show() {
        return 10;
    }

    double show() {
        return 10.5;
    }
}
```

The compiler reports a duplicate method error.

Why?

Because the parameter list is identical:

```text
show()
show()
```

Java does not use the return type alone to distinguish overloaded methods.

---

# 8. Method Signature

For overloading, Java identifies a method primarily by:

```text
Method name + parameter types
```

Example:

```java
void show(int a, double b)
```

The relevant signature is:

```text
show(int, double)
```

Not:

```text
show(int, double, void)
```

The return type is not part of the method signature used to distinguish overloads.

---

# 9. Is Method Name Part of the Signature?

Yes, conceptually the method declaration is identified by its name and parameter types.

For overloading:

```java
show(int)
show(double)
```

are different methods.

But:

```java
show(int)
show(int)
```

are duplicate declarations.

---

# 10. Method Overloading is Compile-Time Polymorphism

Method overloading is commonly called:

> **Compile-time polymorphism**

Example:

```java
class Calculator {

    void add(int a, int b) {
        System.out.println("int");
    }

    void add(double a, double b) {
        System.out.println("double");
    }
}
```

```java
Calculator c = new Calculator();

c.add(10, 20);
```

The compiler determines which method is applicable.

Therefore:

```text
Method Overloading
        ↓
Compile-Time Polymorphism
        ↓
Early / Static Binding
```

---

# 11. How Does the Compiler Choose an Overloaded Method?

This is an important interview topic.

Suppose:

```java
class Demo {

    void show(int x) {
        System.out.println("int");
    }

    void show(double x) {
        System.out.println("double");
    }
}
```

Now:

```java
Demo d = new Demo();

d.show(10);
```

The argument is:

```text
int
```

The compiler finds:

```text
show(int)
```

which is an exact match.

So:

```text
int → show(int)
```

---

# 12. Exact Match Has Priority

Suppose:

```java
void show(int x)
void show(double x)
```

Call:

```java
show(10);
```

The compiler prefers:

```text
show(int)
```

because the argument is already an `int`.

Exact match generally has higher priority than conversion.

---

# 13. Primitive Widening During Overloading

Java can use primitive widening when an exact match is unavailable.

Example:

```java
class Demo {

    void show(long x) {
        System.out.println("long");
    }

    void show(double x) {
        System.out.println("double");
    }
}
```

Call:

```java
Demo d = new Demo();

d.show(10);
```

`10` is an `int`.

There is no:

```text
show(int)
```

So Java can widen:

```text
int → long
```

Therefore:

```text
show(long)
```

is selected.

---

# 14. Primitive Widening Order

Common widening conversions:

```text
byte
  ↓
short
  ↓
int
  ↓
long
  ↓
float
  ↓
double
```

Also:

```text
char → int → long → float → double
```

Widening is allowed.

Narrowing is not performed automatically.

Example:

```java
void show(int x) {
}

void show(byte x) {
}
```

Calling:

```java
show(10);
```

selects:

```text
show(int)
```

Java does not automatically narrow:

```text
int → byte
```

---

# 15. Widening vs Narrowing

### Widening

```java
int x = 10;

long y = x;
```

Allowed.

### Narrowing

```java
long x = 10;

int y = x;
```

Not allowed without casting.

Therefore, overloading does not freely perform narrowing conversions.

---

# 16. Overloading with Boxing

Java also considers boxing conversions.

Example:

```java
class Demo {

    void show(int x) {
        System.out.println("int");
    }

    void show(Integer x) {
        System.out.println("Integer");
    }
}
```

Call:

```java
show(10);
```

Output:

```text
int
```

Why?

Because exact primitive match is preferred over boxing.

```text
int
 ↓
show(int)
```

rather than:

```text
int
 ↓ boxing
Integer
 ↓
show(Integer)
```

---

# 17. Widening vs Boxing

Consider:

```java
class Demo {

    void show(long x) {
        System.out.println("long");
    }

    void show(Integer x) {
        System.out.println("Integer");
    }
}
```

Call:

```java
show(10);
```

The argument is:

```text
int
```

Java prefers widening:

```text
int → long
```

over boxing:

```text
int → Integer
```

So:

```text
long
```

is printed.

Important interview rule:

> **Primitive widening is preferred over boxing.**

---

# 18. Boxing vs Varargs

Example:

```java
class Demo {

    void show(Integer x) {
        System.out.println("Integer");
    }

    void show(int... x) {
        System.out.println("varargs");
    }
}
```

Call:

```java
show(10);
```

The compiler prefers:

```text
show(Integer)
```

over:

```text
show(int...)
```

because varargs is generally considered later in overload resolution.

---

# 19. Varargs and Overloading

Varargs can also be overloaded.

```java
class Demo {

    void show(int x) {
        System.out.println("int");
    }

    void show(int... x) {
        System.out.println("varargs");
    }
}
```

Call:

```java
show(10);
```

Output:

```text
int
```

Because the fixed-arity method is more specific than the varargs form for this call.

But:

```java
show(10, 20, 30);
```

selects:

```text
show(int...)
```

---

# 20. Important Overload Resolution Order

A simplified mental model for common cases is:

```text
1. Exact match
       ↓
2. Primitive widening
       ↓
3. Boxing / unboxing
       ↓
4. Varargs
```

Important:

> Actual Java overload resolution has more detailed rules, especially around combinations of reference conversions, boxing/unboxing, generic methods, and `null`. Use this as a practical interview model, not a complete specification.

---

# 21. Overloading with null

This is a classic interview question.

```java
class Demo {

    void show(String s) {
        System.out.println("String");
    }

    void show(Integer i) {
        System.out.println("Integer");
    }
}
```

Now:

```java
show(null);
```

This causes:

```text
Compile-time error
```

Why?

Because `null` can be assigned to both:

```text
String
Integer
```

Neither type is more specific than the other because they are unrelated reference types.

So the call is ambiguous.

---

# 22. null with Parent and Child

Now consider:

```java
class Animal {
}

class Dog extends Animal {
}

class Demo {

    void show(Animal a) {
        System.out.println("Animal");
    }

    void show(Dog d) {
        System.out.println("Dog");
    }
}
```

Call:

```java
show(null);
```

This selects:

```text
show(Dog)
```

Why?

Because:

```text
Dog
 ↓
Animal
```

`Dog` is more specific than `Animal`.

Therefore the compiler chooses:

```text
Dog
```

---

# 23. Ambiguous null Example

```java
void show(String s) {
}

void show(StringBuilder s) {
}

show(null);
```

This is ambiguous because:

```text
String
StringBuilder
```

are unrelated classes.

Neither is more specific than the other.

---

# 24. Overloading and Inheritance

A child class can also overload an inherited method.

```java
class Parent {

    void show(int x) {
        System.out.println("Parent int");
    }
}

class Child extends Parent {

    void show(double x) {
        System.out.println("Child double");
    }
}
```

Now:

```java
Child c = new Child();

c.show(10);
c.show(10.5);
```

Output:

```text
Parent int
Child double
```

The child class has:

```text
Inherited:
show(int)

Own:
show(double)
```

Together, they form an overloaded set available through the child reference.

---

# 25. Overloading vs Overriding

This is one of the most important OOP comparisons.

| Feature              | Overloading                            | Overriding                       |
| -------------------- | -------------------------------------- | -------------------------------- |
| Meaning              | Same method name, different parameters | Child redefines inherited method |
| Classes              | Usually same class                     | Parent + child                   |
| Parameters           | Must differ                            | Must be same                     |
| Binding              | Compile time                           | Runtime                          |
| Polymorphism         | Compile-time                           | Runtime                          |
| Inheritance required | No                                     | Yes                              |
| Return type alone    | Cannot overload                        | Covariant return allowed         |
| Access modifier      | Can differ subject to normal rules     | Cannot reduce visibility         |
| `static`             | Can overload                           | Cannot truly override            |
| `final`              | Can be overloaded                      | Cannot be overridden             |

---

# 26. Can main() Be Overloaded?

Yes.

```java
class Demo {

    public static void main(String[] args) {
        System.out.println("Main");
    }

    public static void main(int x) {
        System.out.println("int main");
    }
}
```

This is valid overloading.

However, the JVM starts the program using the standard entry-point signature:

```java
public static void main(String[] args)
```

The overloaded version is not automatically used as the entry point.

---

# 27. Can Constructors Be Overloaded?

Yes.

Constructors cannot be overridden, but they can be overloaded.

```java
class Student {

    Student() {
    }

    Student(String name) {
    }

    Student(String name, int age) {
    }
}
```

Now:

```java
Student s1 = new Student();
Student s2 = new Student("Rahul");
Student s3 = new Student("Rahul", 21);
```

This is **constructor overloading**.

---

# 28. Constructor Overloading vs Method Overloading

Both follow the same basic idea:

```text
Same name
Different parameter lists
```

But:

```text
Method
   ↓
Has return type

Constructor
   ↓
No return type
   ↓
Name must match class name
```

---

# 29. Can Static Methods Be Overloaded?

Yes.

```java
class Demo {

    static void show(int x) {
    }

    static void show(String x) {
    }
}
```

This is valid.

Remember:

```text
Static methods cannot be overridden,
but they can be overloaded.
```

---

# 30. Can Private Methods Be Overloaded?

Yes.

```java
class Demo {

    private void show(int x) {
    }

    private void show(String x) {
    }
}
```

This is valid because overloading happens based on different parameter lists.

---

# 31. Can Final Methods Be Overloaded?

Yes.

```java
class Demo {

    final void show(int x) {
    }

    final void show(String x) {
    }
}
```

`final` prevents overriding, not overloading.

---

# 32. Return Type and Overloading

This is extremely important:

```java
int show(int x)
double show(int x)
```

❌ Invalid.

But:

```java
int show(int x)
double show(double x)
```

✅ Valid.

Because the parameter lists are different.

Remember:

> **Return type cannot distinguish overloaded methods.**

---

# 33. Access Modifiers and Overloading

Overloaded methods can have different access modifiers.

```java
class Demo {

    public void show(int x) {
    }

    private void show(String x) {
    }
}
```

This is valid.

Access modifier does not determine whether two methods are overloads.

---

# 34. Generic Methods and Overloading

Generic methods can also participate in overloading, but there are restrictions because of **type erasure**.

For example, these cannot coexist:

```java
void process(List<String> list) {
}

void process(List<Integer> list) {
}
```

After type erasure, both effectively become:

```text
process(List)
```

So they have the same erased signature.

This causes a compile-time error.

This becomes especially important when you study **Generics and Type Erasure**.

---

# 35. Overloading and Compile-Time Type

Because overloading is resolved at compile time, the compiler uses the **compile-time type of the reference**.

Example:

```java
class Animal {
}

class Dog extends Animal {
}

class Demo {

    void show(Animal a) {
        System.out.println("Animal");
    }

    void show(Dog d) {
        System.out.println("Dog");
    }
}
```

Now:

```java
Animal a = new Dog();

Demo d = new Demo();

d.show(a);
```

Output:

```text
Animal
```

Even though the actual object is `Dog`.

Why?

Because overload resolution happens at compile time.

The compiler sees:

```text
reference type = Animal
```

So it selects:

```text
show(Animal)
```

This is different from runtime overriding.

---

# 36. Overloading + Overriding Together

This is a very common interview trap.

```java
class Parent {

    void show(Object o) {
        System.out.println("Parent Object");
    }
}

class Child extends Parent {

    @Override
    void show(Object o) {
        System.out.println("Child Object");
    }

    void show(String s) {
        System.out.println("Child String");
    }
}
```

Now:

```java
Parent p = new Child();

p.show("Hello");
```

What happens?

At compile time, the reference type is:

```text
Parent
```

Parent has:

```text
show(Object)
```

So the compiler selects:

```text
show(Object)
```

At runtime, that method is overridden by Child.

Therefore output:

```text
Child Object
```

The `show(String)` method in `Child` is not considered because the reference type is `Parent`.

This demonstrates:

```text
Overloading → Compile time
Overriding  → Runtime
```

---

# 37. Real-World Example

Imagine an `OrderService`:

```java
class OrderService {

    void createOrder(String product) {
        System.out.println("Creating single-product order");
    }

    void createOrder(String product, int quantity) {
        System.out.println("Creating quantity order");
    }

    void createOrder(String product, int quantity, String coupon) {
        System.out.println("Creating discounted order");
    }
}
```

Usage:

```java
OrderService service = new OrderService();

service.createOrder("Burger");

service.createOrder("Burger", 3);

service.createOrder("Burger", 3, "SAVE20");
```

Same logical operation:

```text
createOrder()
```

Different parameter lists.

This is a practical use of overloading.

---

# 38. Advantages of Method Overloading

### 1. Readability

Same operation gets the same method name.

### 2. Convenience

Users don't need to remember many method names.

### 3. Flexibility

Methods can accept different types or numbers of arguments.

### 4. Maintainability

Related operations stay grouped together.

### 5. Compile-Time Type Checking

The compiler can detect invalid calls before execution.

---

# 39. Disadvantages / Limitations

Overloading can become confusing if too many similar methods exist.

Example:

```text
process(int)
process(long)
process(float)
process(double)
process(Integer)
process(Long)
process(Object)
process(int...)
```

Too many overloads can make API behavior difficult to understand, especially when implicit conversions are involved.

Therefore, overloads should have:

```text
Clear intent
Predictable behavior
Meaningful parameter differences
```

---

# 40. Common Interview Traps

### Trap 1

```java
int show()
double show()
```

❌ Not overloading.

---

### Trap 2

```java
void show(int x)
void show(Integer x)
```

✅ These are different overloads.

---

### Trap 3

```java
void show(int x)
void show(long x)
```

Calling:

```java
show(10);
```

selects:

```text
show(int)
```

---

### Trap 4

```java
void show(long x)
void show(Integer x)
```

Calling:

```java
show(10);
```

normally selects:

```text
show(long)
```

because primitive widening is preferred over boxing in the applicable phases.

---

### Trap 5

```java
void show(String s)
void show(Integer i)

show(null);
```

❌ Ambiguous.

---

### Trap 6

```java
void show(Animal a)
void show(Dog d)

show(null);
```

If `Dog extends Animal`:

```text
show(Dog)
```

is selected.

---

### Trap 7

Static methods can be overloaded.

```text
static show(int)
static show(String)
```

✅ Valid.

---

### Trap 8

Final methods can be overloaded.

```text
final show(int)
final show(String)
```

✅ Valid.

---

# 41. Important Interview Questions

### Basic

1. What is method overloading?
2. Why do we use method overloading?
3. What are the rules for method overloading?
4. Can methods be overloaded by changing only the return type?
5. What is compile-time polymorphism?
6. Why is overloading called compile-time polymorphism?

### Intermediate

7. Can static methods be overloaded?
8. Can private methods be overloaded?
9. Can final methods be overloaded?
10. Can constructors be overloaded?
11. Can `main()` be overloaded?
12. What is a method signature?
13. Is return type part of the method signature?
14. Can parameter order create an overload?
15. How does Java choose between overloaded methods?

### Advanced

16. What happens when overloading involves primitive widening?
17. What happens when overloading involves boxing?
18. What happens when overloading involves varargs?
19. What happens when `null` is passed to overloaded methods?
20. Why can `show(String)` and `show(Integer)` cause ambiguity with `null`?
21. Why does `show(Dog)` win over `show(Animal)` for `null`?
22. How does compile-time reference type affect overload resolution?
23. Can generic methods be overloaded?
24. How does type erasure affect generic method overloading?
25. What happens when overloading and overriding are combined?

---

# 42. Quick Revision Cheat Sheet

```text
METHOD OVERLOADING
        ↓
Same method name
        +
Different parameter list
        ↓
Compile-Time Polymorphism
        ↓
Early / Static Binding
```

Valid ways:

```text
Different number of parameters
Different parameter types
Different parameter order
```

Not valid:

```text
Only return type changed
Only access modifier changed
Only static/final changed
```

---

# 43. Most Important Conversion Priority

For common interview situations, remember:

```text
Exact Match
     ↓
Primitive Widening
     ↓
Boxing / Unboxing
     ↓
Varargs
```

And:

```text
int → long       ✅ Widening
int → Integer    ✅ Boxing
int → byte       ❌ Automatic narrowing
```

---

# 44. The Golden Difference

```text
OVERLOADING
    ↓
Which method?
    ↓
Compiler decides
    ↓
Compile time
```

```text
OVERRIDING
    ↓
Which implementation?
    ↓
Actual object matters
    ↓
Runtime
```

Example:

```java
Animal a = new Dog();

a.sound();
```

If `sound()` is overridden:

```text
Dog.sound()
```

runs at runtime.

But:

```java
demo.show(a);
```

with overloaded methods is resolved using the compile-time type of `a`.

---

# 45. 30-Second Interview Answer

> **Method overloading is a feature of Java where multiple methods have the same name but different parameter lists. The difference can be in the number, type, or order of parameters. Return type alone cannot overload a method. Method overloading is resolved at compile time, so it is commonly called compile-time polymorphism or static binding.**

Example:

```java
class Calculator {

    int add(int a, int b) {
        return a + b;
    }

    double add(double a, double b) {
        return a + b;
    }
}
```

The compiler chooses the appropriate `add()` method based on the arguments supplied.

---

# ⭐ Final Memory Trick

```text
OVERLOADING = SAME NAME + DIFFERENT PARAMETERS

                    ↓

              COMPILE TIME
                    ↓
               POLYMORPHISM
```

> **Remember: Return type doesn't matter for overloading. Parameters do.**
