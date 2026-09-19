# ☕ Java OOP — `final` Keyword

> **The `final` keyword is used to restrict modification, overriding, or inheritance depending on where it is applied.**

---

# 1. What is `final` in Java?

`final` is a Java keyword used to impose restrictions.

It can be applied to:

```text
final variable
final method
final class
```

The meaning depends on where it is used.

```text
final variable
     ↓
Cannot be reassigned

final method
     ↓
Cannot be overridden

final class
     ↓
Cannot be inherited
```

---

# 2. Why Do We Need `final`?

Sometimes we want to prevent further modification of something.

For example:

```java
final double PI = 3.14159;
```

We don't want:

```java
PI = 10;
```

Similarly, a class may be designed so that nobody can extend it:

```java
final class SecurityManager {
}
```

And a method may contain behavior that subclasses must not replace:

```java
final void authenticate() {
}
```

So `final` provides a form of **restriction and design control**.

---

# 3. Three Main Uses of `final`

```text
                    final
                      |
          +-----------+-----------+
          |           |           |
       Variable     Method      Class
          |           |           |
     No reassignment  No         No
                    overriding  inheritance
```

Remember:

```text
final variable → cannot reassign
final method   → cannot override
final class    → cannot extend
```

---

# 4. `final` Variable

A final variable can be assigned only once.

Example:

```java
final int x = 10;

x = 20; // ❌ Compile-time error
```

Once assigned:

```text
x = 10
 ↓
locked
```

It cannot be reassigned.

---

# 5. Final Variable Must Be Initialized

A final variable must receive a value before it is read.

This is invalid:

```java
final int x;

System.out.println(x); // ❌
```

But this can be valid:

```java
final int x;

x = 10;

System.out.println(x);
```

The important rule is:

> A final variable must be definitely assigned exactly once before use.

---

# 6. Final Local Variable

A final variable declared inside a method is called a final local variable.

```java
void show() {

    final int age = 21;

    System.out.println(age);
}
```

This is invalid:

```java
age = 25;
```

because `age` is final.

---

# 7. Final Parameter

Method parameters can also be final.

```java
void calculate(final int x) {

    System.out.println(x);
}
```

Inside the method:

```java
x = 20; // ❌
```

is not allowed.

Why?

Because the parameter itself cannot be reassigned.

---

# 8. Final Reference Variable

This is one of the **most important interview concepts**.

Consider:

```java
final Student s = new Student();
```

`final` prevents changing the reference to point to another object.

This is invalid:

```java
s = new Student(); // ❌
```

But modifying the object's internal state may still be allowed:

```java
s.name = "Rahul"; // ✅
```

assuming `name` is accessible and not itself final.

---

# 9. Final Reference Does NOT Make the Object Immutable

This is a major interview trap.

Example:

```java
class Student {

    String name;
}

final Student s = new Student();

s.name = "Rahul"; // ✅
```

But:

```java
s = new Student(); // ❌
```

Therefore:

```text
final reference
       ↓
Reference cannot change
       ↓
Object may still change
```

It does **not automatically mean**:

```text
Object is immutable
```

---

# 10. Final Primitive vs Final Reference

### Primitive

```java
final int x = 10;
```

Cannot change:

```java
x = 20; // ❌
```

### Reference

```java
final Student s = new Student();
```

Cannot change the reference:

```java
s = new Student(); // ❌
```

But the object may be mutable:

```java
s.name = "Amit"; // ✅
```

Mental model:

```text
final primitive
      ↓
value cannot be reassigned

final reference
      ↓
reference cannot point elsewhere
```

---

# 11. Blank Final Variable

A final variable can be declared without an immediate value.

```java
final int x;
```

This is called a **blank final variable**.

It must be initialized exactly once before use.

```java
final int x;

x = 100;

System.out.println(x);
```

Valid.

But:

```java
x = 200;
```

afterward is invalid.

---

# 12. Final Instance Variable

A final instance variable belongs to each object.

```java
class Student {

    final int id;

    Student(int id) {
        this.id = id;
    }
}
```

Now:

```java
Student s1 = new Student(101);
Student s2 = new Student(102);
```

Each object gets its own final value:

```text
s1.id → 101
s2.id → 102
```

The value cannot be reassigned after initialization.

---

# 13. Final Instance Variable and Constructor

A final instance variable can be initialized inside a constructor.

```java
class Student {

    final int id;

    Student(int id) {
        this.id = id;
    }
}
```

This is valid because every object receives the value during construction.

But assigning it again is not allowed:

```java
Student(int id) {

    this.id = id;

    this.id = 20; // ❌
}
```

---

# 14. Final Static Variable

A static final variable belongs to the class and cannot be reassigned.

```java
static final double PI = 3.14159;
```

Usually constants are declared as:

```java
public static final
```

Example:

```java
public static final int MAX_USERS = 100;
```

Convention:

```text
UPPER_CASE_WITH_UNDERSCORES
```

---

# 15. Constants in Java

A common Java constant pattern is:

```java
public static final int MAX_SIZE = 100;
```

Break it down:

```text
public
 ↓
accessible broadly

static
 ↓
belongs to class

final
 ↓
cannot be reassigned
```

So:

```text
public static final
        ↓
Class-level constant
```

---

# 16. Is Every `final` Variable a Constant?

No.

This:

```java
final int age = 21;
```

is a final variable, but it is not necessarily a class constant.

A conventional Java constant is usually:

```java
static final
```

and often:

```java
public static final
```

Example:

```java
public static final int DAYS_IN_WEEK = 7;
```

---

# 17. `final` Method

A final method cannot be overridden by a subclass.

Example:

```java
class Parent {

    final void show() {
        System.out.println("Parent");
    }
}
```

This is illegal:

```java
class Child extends Parent {

    @Override
    void show() {
        System.out.println("Child");
    }
}
```

Compile-time error.

---

# 18. Why Use a Final Method?

A final method is useful when the parent class wants to prevent subclasses from replacing a particular implementation.

For example:

```java
class BankAccount {

    final void validateAccount() {

        System.out.println("Validating account");
    }
}
```

A subclass cannot replace this method.

Conceptually:

```text
Parent defines critical behavior
             ↓
final method
             ↓
Subclass cannot override it
```

---

# 19. Can a Final Method Be Inherited?

Yes.

This is an important distinction.

A final method cannot be overridden, but it can be inherited.

```java
class Parent {

    final void show() {
        System.out.println("Parent");
    }
}

class Child extends Parent {
}
```

Then:

```java
Child c = new Child();

c.show(); // ✅
```

The child inherits the method but cannot replace it.

---

# 20. Final Method vs Private Method

Both cannot be overridden, but for different reasons.

```text
final method
     ↓
Inherited
     ↓
Cannot override

private method
     ↓
Not accessible/inherited for overriding
     ↓
Cannot override
```

This distinction is frequently asked in interviews.

---

# 21. Final Method and Overloading

A final method cannot be overridden, but it **can be overloaded**.

Example:

```java
class Parent {

    final void show() {
        System.out.println("show()");
    }

    void show(int x) {
        System.out.println("show(int)");
    }
}
```

Or:

```java
class Child extends Parent {

    void show(int x) {
        System.out.println("Child show(int)");
    }
}
```

This is not overriding the final `show()` because the parameter list is different.

---

# 22. Final Class

A final class cannot be extended.

Example:

```java
final class Animal {
}
```

This is invalid:

```java
class Dog extends Animal {
}
```

Compile-time error.

Because:

```text
final class
     ↓
cannot be inherited
```

---

# 23. Why Use a Final Class?

A final class is useful when the class should not be extended.

It can prevent subclasses from changing or extending its behavior.

Examples from Java include:

```text
String
Integer
Double
```

`String` is final.

Conceptually:

```text
final String
      ↓
cannot create subclass of String
```

---

# 24. `final` Class vs `final` Method

| `final class`              | `final method`                     |
| -------------------------- | ---------------------------------- |
| Cannot be extended         | Cannot be overridden               |
| Applies to class           | Applies to method                  |
| Prevents inheritance       | Prevents method replacement        |
| Entire class is restricted | Only specific method is restricted |

Example:

```java
final class A {
}
```

means:

```text
No class can extend A
```

While:

```java
class A {

    final void show() {
    }
}
```

means:

```text
A can be extended
but show() cannot be overridden
```

---

# 25. Can a Final Class Have Final Methods?

Yes.

But it is not necessary for preventing overriding because no class can extend the final class anyway.

Example:

```java
final class Student {

    final void show() {
    }
}
```

Valid.

However, `final` on the method is redundant with respect to subclass overriding because subclasses cannot exist.

---

# 26. Can a Final Class Have Non-Final Methods?

Yes.

```java
final class Student {

    void show() {
        System.out.println("Student");
    }
}
```

Valid.

No subclass can override `show()` because no subclass can be created.

---

# 27. Can a Final Class Be Abstract?

No.

These concepts conflict:

```text
abstract class
      ↓
designed to be inherited

final class
      ↓
cannot be inherited
```

Therefore:

```java
abstract final class A {
}
```

is invalid.

---

# 28. Can a Final Method Be Abstract?

No.

These also conflict:

```text
abstract method
      ↓
must be implemented by subclass

final method
      ↓
cannot be overridden
```

Therefore:

```java
abstract final void show();
```

is invalid.

---

# 29. `final` and Inheritance

Think of `final` as controlling inheritance.

```text
final class
     ↓
STOP inheritance

final method
     ↓
STOP overriding

final variable
     ↓
STOP reassignment
```

This is the easiest way to remember it.

---

# 30. `final` and Constructor

Constructors cannot be declared `final`.

This is invalid:

```java
class Student {

    final Student() {
    }
}
```

Why?

Because constructors are not inherited and therefore cannot be overridden.

So `final` has no meaningful role for constructors.

---

# 31. Can a Constructor Be Overridden?

No.

Constructors are not inherited.

Therefore:

```text
constructor
     ↓
not inherited
     ↓
cannot be overridden
```

And:

```java
final Constructor()
```

is invalid syntax.

---

# 32. `final` and Static Methods

A static method can be declared final.

```java
class Parent {

    static final void show() {
        System.out.println("Parent");
    }
}
```

This prevents the method from being hidden by a subclass's static method with the same signature.

The key distinction is:

```text
static
 ↓
normally hidden, not overridden

static final
 ↓
cannot be hidden by a subclass
```

---

# 33. `final` and `static`

Compare:

```java
static
```

with:

```java
final
```

They solve different problems.

```text
static
 ↓
belongs to class

final
 ↓
cannot be reassigned/overridden/inherited
```

Together:

```java
static final int MAX = 100;
```

means:

```text
belongs to class
+
cannot be reassigned
```

---

# 34. `final` and `String`

`String` is a final class in Java.

Conceptually:

```java
public final class String {
}
```

Therefore:

```java
class MyString extends String {
}
```

is not allowed.

This is separate from the fact that `String` objects are immutable.

Important:

```text
final class String
        ≠
String immutable
```

`String` immutability is a property of how the class is designed.

---

# 35. `final` Does Not Mean Immutable

This is one of the biggest interview traps.

Consider:

```java
final List<String> names = new ArrayList<>();
```

This is allowed:

```java
names.add("Rahul");
names.add("Aman");
```

But this is not:

```java
names = new ArrayList<>(); // ❌
```

Therefore:

```text
final reference
     ↓
reference cannot change

object state
     ↓
may still change
```

---

# 36. Final Object and Mutable State

Example:

```java
class Student {

    int marks;
}

final Student s = new Student();

s.marks = 90; // ✅
```

The reference is final.

The object is still mutable.

So:

```text
final ≠ immutable
```

---

# 37. How to Make an Object Truly Immutable?

`final` alone is not enough.

Typical immutable-class design involves things such as:

```text
1. Prevent subclassing when appropriate
2. Keep fields private
3. Make state final
4. Initialize state through constructor
5. Do not provide setters
6. Defensively copy mutable objects
7. Return safe copies when necessary
```

This is why:

```text
final reference
```

and:

```text
immutable object
```

are different concepts.

---

# 38. Final Variable and Memory

Consider:

```java
final int x = 10;
```

`final` is primarily a **language-level restriction**.

It tells the compiler that the variable cannot be assigned again after definite assignment.

Do not think:

```text
final
 ↓
special memory area
```

There is no separate "final memory" in Java.

---

# 39. Final and Compile-Time Constants

A `static final` primitive or `String` initialized with a compile-time constant expression can be a compile-time constant.

Example:

```java
static final int MAX = 100;
```

or:

```java
static final String APP_NAME = "QRder";
```

These can be treated specially by the compiler.

But:

```java
static final int MAX = getValue();
```

is not a compile-time constant because the value depends on runtime evaluation.

---

# 40. Final Variable and Assignment

Consider:

```java
final int x;
```

This is allowed:

```java
x = 10;
```

But not:

```java
x = 20;
```

The rule is:

```text
exactly one assignment
```

before the variable is used.

---

# 41. Final Variable in Constructor

This is valid:

```java
class User {

    final int id;

    User(int id) {
        this.id = id;
    }
}
```

The constructor can assign the final field because each object gets initialized once.

But:

```java
User(int id) {

    this.id = id;

    this.id = 100; // ❌
}
```

is invalid.

---

# 42. Final Variable in Multiple Constructors

Consider:

```java
class User {

    final int id;

    User() {
        id = 0;
    }

    User(int id) {
        this.id = id;
    }
}
```

This is valid.

Each constructor must ensure that `id` receives exactly one value for the object being created.

---

# 43. Final Variable and Static Initialization

A static final field can be initialized in a static initializer.

```java
class Config {

    static final int MAX_USERS;

    static {
        MAX_USERS = 100;
    }
}
```

This is valid.

It is useful when initialization requires more than a simple expression.

---

# 44. Final Field and Initialization Timing

Final fields can be initialized through:

```text
1. Declaration
2. Instance initializer
3. Constructor
```

For static final fields:

```text
1. Declaration
2. Static initializer
```

The key requirement is still:

```text
must be definitely assigned
and cannot be assigned again
```

---

# 45. Final Method — Interview Example

```java
class Payment {

    final void validatePayment() {
        System.out.println("Payment validated");
    }

    void pay() {
        System.out.println("Payment processed");
    }
}

class UPI extends Payment {

    @Override
    void pay() {
        System.out.println("UPI payment");
    }
}
```

Here:

```text
validatePayment()
      ↓
cannot override

pay()
      ↓
can override
```

This lets the parent lock selected behavior while still allowing customization elsewhere.

---

# 46. Final Class — Interview Example

```java
final class SecurityConfig {

    void validate() {
        System.out.println("Validating");
    }
}
```

This is illegal:

```java
class CustomSecurityConfig extends SecurityConfig {
}
```

Because:

```text
SecurityConfig
      ↓
final
      ↓
cannot extend
```

---

# 47. `final` vs `finally` vs `finalize`

Very common interview question.

| Keyword/Method | Meaning                                                                   |
| -------------- | ------------------------------------------------------------------------- |
| `final`        | Restricts variable/method/class                                           |
| `finally`      | Block used with exception handling                                        |
| `finalize()`   | Legacy `Object` method associated with GC cleanup; deprecated for removal |

Example:

```java
final int x = 10;
```

`finally`:

```java
try {
    // code
}
finally {
    // cleanup
}
```

`finalize()`:

```java
@Override
protected void finalize() throws Throwable {
}
```

Do not confuse these three.

---

# 48. Is `finalize()` Related to `final`?

No.

Despite the similar name:

```text
final
finalize()
```

are completely different.

`final` is a Java keyword.

`finalize()` is a method historically associated with garbage collection, and it has been deprecated for removal.

---

# 49. Final vs Immutability

| `final`                                      | Immutable                                         |
| -------------------------------------------- | ------------------------------------------------- |
| Language restriction                         | Object-design property                            |
| Can restrict reassignment                    | Prevents observable state changes                 |
| Can apply to variable/method/class           | Describes object behavior                         |
| Does not automatically make object immutable | Object state cannot be changed after construction |

Example:

```java
final Student s = new Student();
```

does not automatically make `Student` immutable.

---

# 50. Final vs Constant

A final variable:

```java
final int x = 10;
```

cannot be reassigned.

A conventional constant is usually:

```java
static final int MAX_USERS = 100;
```

So:

```text
final
 ↓
one assignment

static final
 ↓
class-level constant-like field
```

---

# 51. Common Interview Traps

### Trap 1

```java
final Student s = new Student();

s = new Student();
```

❌ Invalid.

The reference cannot be reassigned.

---

### Trap 2

```java
final Student s = new Student();

s.name = "Rahul";
```

✅ Can be valid.

The object may still be mutable.

---

### Trap 3

Final method cannot be inherited.

❌ Wrong.

It can be inherited but cannot be overridden.

---

### Trap 4

Final class means every field is final.

❌ Wrong.

A final class can have mutable fields.

---

### Trap 5

Final makes an object immutable.

❌ Wrong.

Final reference does not make the referenced object immutable.

---

### Trap 6

Final method cannot be overloaded.

❌ Wrong.

A final method can be overloaded.

---

### Trap 7

Final constructor is allowed.

❌ Wrong.

Constructors cannot be final.

---

### Trap 8

Abstract final method is allowed.

❌ Wrong.

An abstract method must be overridden, while a final method cannot be overridden.

---

### Trap 9

Abstract final class is allowed.

❌ Wrong.

An abstract class is designed for inheritance, while a final class prohibits inheritance.

---

### Trap 10

Final static methods are overridden.

❌ Wrong.

Static methods are hidden, not overridden. A final static method cannot be hidden by a subclass.

---

# 52. `final` vs `static final`

```java
final int x = 10;
```

Each object may have its own `x`.

While:

```java
static final int x = 10;
```

there is one class-level field shared by the class.

Think:

```text
final
 ↓
one assignment per variable instance

static final
 ↓
one class-level value
```

---

# 53. `final` and Polymorphism

Final methods restrict runtime polymorphic overriding.

Example:

```java
class Parent {

    final void show() {
        System.out.println("Parent");
    }
}
```

A child cannot provide another implementation of `show()`.

Therefore:

```text
final method
     ↓
no overriding
     ↓
no subclass-specific implementation for that method
```

Other non-final methods can still participate in polymorphism.

---

# 54. Why Does Java Allow Final Classes?

One reason is to prevent inheritance when extending a class would violate its intended design or invariants.

For example:

```text
class
 ↓
must maintain certain behavior
 ↓
prevent subclass customization
 ↓
final
```

The `final` modifier communicates this design restriction to the compiler and developers.

---

# 55. Real-World Example — Payment Validation

```java
class Payment {

    final void validate() {

        System.out.println("Validating payment");
    }

    void pay() {

        System.out.println("Processing payment");
    }
}

class UPI extends Payment {

    @Override
    void pay() {

        System.out.println("Processing UPI payment");
    }
}
```

Here:

```text
validate()
    ↓
locked

pay()
    ↓
customizable
```

This demonstrates why a class may use a mixture of final and non-final methods.

---

# 56. Real-World Example — QRder

Imagine:

```java
class Order {

    final int orderId;

    Order(int orderId) {
        this.orderId = orderId;
    }
}
```

The order ID should not be reassigned after the order is created.

```java
Order order = new Order(1001);

order.orderId = 2000; // ❌
```

The idea is:

```text
Order created
     ↓
orderId assigned
     ↓
orderId cannot be reassigned
```

---

# 57. `final` — Complete Mental Model

When you see `final`, immediately ask:

```text
Where is final used?
```

### Variable

```text
final variable
      ↓
cannot be reassigned
```

### Method

```text
final method
      ↓
cannot be overridden
```

### Class

```text
final class
      ↓
cannot be extended
```

This is the most important memory trick.

---

# 58. 30-Second Interview Answer

> **The `final` keyword is used to restrict modification in Java. A final variable cannot be reassigned after initialization, a final method cannot be overridden by subclasses, and a final class cannot be extended. A final reference does not make the referenced object immutable; it only prevents the reference from being reassigned.**

Example:

```java
final int x = 10;

// x = 20; ❌
```

```java
class Parent {

    final void show() {
        System.out.println("Parent");
    }
}
```

```java
final class Student {
}
```

---

# 🔥 Top 10 Most Important Interview Questions + Answers

## 1. What is the `final` keyword in Java?

**Answer:**

`final` is used to restrict modification.

Depending on where it is used:

```text
final variable → cannot be reassigned
final method   → cannot be overridden
final class    → cannot be extended
```

---

## 2. What happens when a variable is declared `final`?

**Answer:**

It can be assigned only once.

```java
final int x = 10;

x = 20; // ❌
```

The variable cannot be reassigned after its value has been established.

---

## 3. Does a final reference make an object immutable?

**Answer:**

No.

```java
final Student s = new Student();

s.name = "Rahul"; // potentially ✅
s = new Student(); // ❌
```

`final` prevents changing the reference, not necessarily the object's internal state.

---

## 4. Can a final method be overridden?

**Answer:**

No.

```java
class Parent {

    final void show() {
    }
}

class Child extends Parent {

    void show() { // ❌
    }
}
```

A final method may be inherited, but it cannot be overridden.

---

## 5. Can a final method be overloaded?

**Answer:**

Yes.

```java
class Parent {

    final void show() {
    }

    void show(int x) {
    }
}
```

Overloading uses different parameter lists, so it is allowed.

---

## 6. Can a final class be inherited?

**Answer:**

No.

```java
final class Parent {
}

class Child extends Parent { // ❌
}
```

A final class cannot have subclasses.

---

## 7. Can an abstract class be final?

**Answer:**

No.

They represent contradictory inheritance rules:

```text
abstract → intended to be inherited
final    → cannot be inherited
```

Therefore:

```java
abstract final class A { } // ❌
```

---

## 8. Can an abstract method be final?

**Answer:**

No.

An abstract method requires a subclass to provide an implementation, while a final method prohibits overriding.

Therefore:

```java
abstract final void show(); // ❌
```

---

## 9. What is the difference between `final`, `finally`, and `finalize()`?

**Answer:**

```text
final
 ↓
keyword for restriction

finally
 ↓
exception-handling block

finalize()
 ↓
legacy Object method associated with GC cleanup;
deprecated for removal
```

They are completely different concepts.

---

## 10. What is the difference between `final` and immutable?

**Answer:**

`final` is a language-level restriction, while immutability is an object-design property.

```text
final reference
      ↓
reference cannot change

immutable object
      ↓
object state cannot change after construction
```

Therefore:

```text
final ≠ immutable
```

---

# ⭐ Final Revision Cheat Sheet

```text
                    final
                      |
          +-----------+-----------+
          |           |           |
       Variable     Method      Class
          |           |           |
     No reassign   No override  No extend
```

### Variable

```java
final int x = 10;
```

```text
x = 20; ❌
```

### Method

```java
final void show() {}
```

```text
Child cannot override show() ❌
```

### Class

```java
final class Student {}
```

```text
class Child extends Student {} ❌
```

### Most Important Trap

```java
final Student s = new Student();

s = new Student(); // ❌
s.name = "Rahul";   // potentially ✅
```

> **Final locks the variable's assignment, not automatically the object's state.**

---

# 🧠 One-Line Memory Trick

> **`final` variable → STOP reassignment | `final` method → STOP overriding | `final` class → STOP inheritance**
