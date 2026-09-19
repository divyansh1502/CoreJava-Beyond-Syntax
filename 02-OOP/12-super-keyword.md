# ☕ Java OOP — `super` Keyword

> **`super` is a special reference used inside a subclass to access members of its immediate parent class.**

---

# 1. What is `super`?

`super` refers to the **immediate parent-class part of the current object**.

It is mainly used to:

```text
1. Access parent-class variables
2. Call parent-class methods
3. Call parent-class constructors
```

Example:

```java
class Parent {

    int x = 10;
}

class Child extends Parent {

    int x = 20;

    void show() {

        System.out.println(x);
        System.out.println(super.x);
    }
}
```

Output:

```text
20
10
```

Here:

```text
x
    ↓
Child.x

super.x
    ↓
Parent.x
```

---

# 2. Why Do We Need `super`?

The main reason is to explicitly access the **parent-class version** of something when the child has its own version.

For example:

```java
class Parent {

    String name = "Parent";
}

class Child extends Parent {

    String name = "Child";

    void show() {

        System.out.println(name);
        System.out.println(super.name);
    }
}
```

Output:

```text
Child
Parent
```

Without `super`, Java resolves `name` to the child field.

---

# 3. Three Main Uses of `super`

```text
                    super
                      |
          +-----------+-----------+
          |           |           |
       Variable     Method     Constructor
          |           |           |
      super.x     super.show()  super()
```

So remember:

```text
super.variable
super.method()
super()
```

---

# 4. `super` to Access Parent Variable

Example:

```java
class Parent {

    int value = 100;
}

class Child extends Parent {

    int value = 200;

    void display() {

        System.out.println(value);
        System.out.println(super.value);
    }
}
```

Output:

```text
200
100
```

Explanation:

```text
value
  ↓
Child.value

super.value
  ↓
Parent.value
```

---

# 5. `super` When There Is No Variable Shadowing

Suppose:

```java
class Parent {

    int age = 50;
}

class Child extends Parent {

    void show() {

        System.out.println(age);
        System.out.println(super.age);
    }
}
```

Both:

```java
age
```

and:

```java
super.age
```

refer to the inherited parent field.

So `super` is not always required.

It is mainly useful when you want to explicitly specify:

> "I want the parent-class member."

---

# 6. Variable Shadowing and `super`

Consider:

```java
class Parent {

    String name = "Parent";
}

class Child extends Parent {

    String name = "Child";

    void show() {

        System.out.println(this.name);
        System.out.println(super.name);
    }
}
```

Output:

```text
Child
Parent
```

Mental model:

```text
this.name
    ↓
Child's field

super.name
    ↓
Parent's field
```

---

# 7. `super` to Call Parent Method

Suppose a child overrides a method:

```java
class Parent {

    void show() {
        System.out.println("Parent");
    }
}

class Child extends Parent {

    @Override
    void show() {
        System.out.println("Child");
    }
}
```

Now:

```java
Child c = new Child();
c.show();
```

Output:

```text
Child
```

But what if the child wants to execute the parent's implementation too?

Use:

```java
super.show();
```

Example:

```java
class Child extends Parent {

    @Override
    void show() {

        System.out.println("Child");

        super.show();
    }
}
```

Output:

```text
Child
Parent
```

---

# 8. Why Is `super.method()` Important?

Suppose a parent contains common behavior:

```java
class Employee {

    void login() {
        System.out.println("Employee login");
    }
}
```

Child:

```java
class Manager extends Employee {

    @Override
    void login() {

        super.login();

        System.out.println("Manager-specific login");
    }
}
```

The child can reuse the parent's implementation and add its own behavior.

This is a common real-world use of `super`.

---

# 9. `super()` — Calling Parent Constructor

The third major use is:

```java
super();
```

It calls the constructor of the immediate parent class.

Example:

```java
class Parent {

    Parent() {
        System.out.println("Parent constructor");
    }
}

class Child extends Parent {

    Child() {

        super();

        System.out.println("Child constructor");
    }
}
```

Now:

```java
new Child();
```

Output:

```text
Parent constructor
Child constructor
```

---

# 10. Why Does the Parent Constructor Execute First?

A child object contains the inherited state of its parent.

Therefore, Java initializes the parent portion before the child portion.

Conceptually:

```text
new Child()
    ↓
Parent constructor
    ↓
Child constructor
```

So constructor execution follows the inheritance chain from parent toward child.

---

# 11. `super()` Must Be the First Statement

This is invalid:

```java
class Child extends Parent {

    Child() {

        System.out.println("Hello");

        super(); // ❌
    }
}
```

Correct:

```java
class Child extends Parent {

    Child() {

        super();

        System.out.println("Hello");
    }
}
```

Rule:

> **An explicit `super(...)` constructor invocation must be the first statement of a constructor.**

---

# 12. `super()` Is Added Automatically

If you don't explicitly write a constructor invocation, Java may insert:

```java
super();
```

automatically.

Example:

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

Conceptually:

```java
Child() {

    super();

    System.out.println("Child");
}
```

Output:

```text
Parent
Child
```

---

# 13. Important Exception — Parent Has No No-Arg Constructor

Consider:

```java
class Parent {

    Parent(int x) {
        System.out.println(x);
    }
}
```

Now:

```java
class Child extends Parent {

    Child() {
        System.out.println("Child");
    }
}
```

This produces a compile-time error.

Why?

Because Java tries to insert:

```java
super();
```

But `Parent` does not have a no-argument constructor.

Correct:

```java
class Child extends Parent {

    Child() {

        super(10);

        System.out.println("Child");
    }
}
```

---

# 14. `super(int)` / Parameterized Parent Constructor

You can pass arguments to the parent constructor.

```java
class Parent {

    int age;

    Parent(int age) {
        this.age = age;
    }
}

class Child extends Parent {

    Child(int age) {

        super(age);
    }
}
```

Now:

```java
Child c = new Child(21);
```

Execution:

```text
Child constructor
       ↓
super(21)
       ↓
Parent(int)
       ↓
Parent.age = 21
```

---

# 15. `super()` vs `this()`

This is extremely important.

| `super()`                      | `this()`                                |
| ------------------------------ | --------------------------------------- |
| Calls parent constructor       | Calls another constructor of same class |
| Constructor chaining to parent | Constructor chaining within same class  |
| Immediate superclass           | Current class                           |
| `super()`                      | `this()`                                |

Example:

```java
class Child extends Parent {

    Child() {
        super();
    }

    Child(int x) {
        this();
    }
}
```

Mental model:

```text
this()
 ↓
same class constructor

super()
 ↓
parent class constructor
```

---

# 16. Can We Use `this()` and `super()` Together?

No, not as constructor-invocation statements in the same constructor.

For example:

```java
Child() {

    this(10);
    super(); // ❌
}
```

Why?

Both must be the first statement.

Only one constructor invocation can be first.

Therefore:

```text
this() + super()
       ↓
same constructor
       ↓
❌ not allowed
```

However, constructor chaining can still eventually involve both.

Example:

```java
class Child extends Parent {

    Child() {
        this(10);
    }

    Child(int x) {
        super(x);
    }
}
```

Here:

```text
Child()
   ↓
this(10)
   ↓
Child(int)
   ↓
super(x)
   ↓
Parent(int)
```

So they cannot be directly used together in one constructor, but they can participate in a constructor chain.

---

# 17. `super` in Method Overriding

This is one of the most important uses.

```java
class Animal {

    void sound() {
        System.out.println("Animal sound");
    }
}

class Dog extends Animal {

    @Override
    void sound() {

        System.out.println("Dog barks");

        super.sound();
    }
}
```

Calling:

```java
Dog d = new Dog();
d.sound();
```

Output:

```text
Dog barks
Animal sound
```

Here:

```text
sound()
   ↓
Dog implementation

super.sound()
   ↓
Parent implementation
```

---

# 18. `super` Prevents Recursive Calls in Overriding

Consider:

```java
class Parent {

    void show() {
        System.out.println("Parent");
    }
}

class Child extends Parent {

    @Override
    void show() {

        this.show();
    }
}
```

This causes:

```text
Child.show()
   ↓
Child.show()
   ↓
Child.show()
   ↓
...
```

Eventually:

```text
StackOverflowError
```

Instead:

```java
super.show();
```

calls the parent's implementation.

---

# 19. `this` vs `super` in Overriding

Example:

```java
class Parent {

    void show() {
        System.out.println("Parent");
    }
}

class Child extends Parent {

    @Override
    void show() {

        System.out.println("Child");

        super.show();
    }
}
```

Think:

```text
this
 ↓
current object / current implementation

super
 ↓
parent implementation
```

---

# 20. `super` and Multilevel Inheritance

Consider:

```java
class Animal {

    void sound() {
        System.out.println("Animal");
    }
}

class Dog extends Animal {

    @Override
    void sound() {
        System.out.println("Dog");
    }
}

class Puppy extends Dog {

    @Override
    void sound() {

        System.out.println("Puppy");

        super.sound();
    }
}
```

Now:

```java
Puppy p = new Puppy();
p.sound();
```

Output:

```text
Puppy
Dog
```

Important:

```text
super
 ↓
immediate parent
```

So from `Puppy`:

```java
super.sound();
```

calls:

```text
Dog.sound()
```

not directly:

```text
Animal.sound()
```

---

# 21. Does `super` Mean "Any Ancestor"?

No.

This is an important interview trap.

`super` refers to the **immediate superclass**.

For:

```text
Animal
   ↑
  Dog
   ↑
 Puppy
```

Inside `Puppy`:

```java
super
```

refers to:

```text
Dog
```

not directly to `Animal`.

---

# 22. Can We Directly Access Grandparent Using `super`?

No direct syntax like:

```java
super.super.show(); // ❌
```

is not allowed.

Example:

```text
Animal
   ↑
 Dog
   ↑
Puppy
```

Inside `Puppy`:

```java
super.show();
```

means:

```text
Dog.show()
```

There is no:

```java
super.super.show();
```

---

# 23. `super` and Fields Are Not Dynamic Dispatch

Consider:

```java
class Parent {

    int value = 10;
}

class Child extends Parent {

    int value = 20;

    void show() {

        System.out.println(super.value);
    }
}
```

Output:

```text
10
```

`super.value` explicitly refers to the parent field.

Fields do not participate in overriding like instance methods do.

---

# 24. `super` and Static Members

Static members belong to classes rather than individual objects.

Java can allow inherited static members to be accessed through `super` in certain contexts, but this is generally not the intended or clearest usage.

Prefer:

```java
Parent.staticMethod();
Parent.staticVariable;
```

rather than using `super` for static members.

For interviews, remember:

```text
super
 ↓
primarily used for parent instance members
and parent constructor invocation
```

---

# 25. Can `super` Access Private Members?

No.

Example:

```java
class Parent {

    private int value = 10;
}

class Child extends Parent {

    void show() {

        System.out.println(super.value); // ❌
    }
}
```

Why?

Because private members are accessible only within the class that declares them.

Therefore:

```text
Parent private member
        ↓
not directly accessible in Child
        ↓
super.value ❌
```

---

# 26. Can `super` Access Protected Members?

Yes.

```java
class Parent {

    protected int value = 10;
}

class Child extends Parent {

    void show() {

        System.out.println(super.value);
    }
}
```

This is valid.

---

# 27. Can `super` Access Public Members?

Yes.

```java
class Parent {

    public void show() {
        System.out.println("Parent");
    }
}

class Child extends Parent {

    void display() {

        super.show();
    }
}
```

Valid.

---

# 28. Can `super` Access Default Members?

It depends on package accessibility.

If the parent member has package-private/default access:

```java
class Parent {

    void show() {
    }
}
```

A subclass in the same package can access it.

```java
super.show();
```

If the subclass is in another package, package-private access is not available merely because of inheritance.

---

# 29. `super` and Constructor Initialization Order

Consider:

```java
class Parent {

    Parent() {
        System.out.println("Parent constructor");
    }
}

class Child extends Parent {

    Child() {
        super();
        System.out.println("Child constructor");
    }
}
```

Calling:

```java
new Child();
```

produces:

```text
Parent constructor
Child constructor
```

General order:

```text
Object constructor
      ↓
Ancestor constructors
      ↓
Parent constructor
      ↓
Child constructor
```

For a direct parent-child relationship:

```text
Parent constructor
      ↓
Child constructor
```

---

# 30. `super` and Instance Initializers

Consider:

```java
class Parent {

    {
        System.out.println("Parent initializer");
    }

    Parent() {
        System.out.println("Parent constructor");
    }
}

class Child extends Parent {

    {
        System.out.println("Child initializer");
    }

    Child() {
        System.out.println("Child constructor");
    }
}
```

Creating:

```java
new Child();
```

gives:

```text
Parent initializer
Parent constructor
Child initializer
Child constructor
```

This reinforces the idea that parent initialization occurs before child initialization.

---

# 31. `super` in Abstract Classes

`super` can call a concrete method from an abstract parent class.

```java
abstract class Animal {

    void eat() {
        System.out.println("Animal eats");
    }

    abstract void sound();
}

class Dog extends Animal {

    @Override
    void sound() {
        System.out.println("Bark");
    }

    void test() {
        super.eat();
    }
}
```

Here:

```java
super.eat();
```

calls the concrete implementation in the abstract parent.

---

# 32. Can `super` Call an Abstract Method?

You cannot use `super.method()` to invoke an abstract method implementation because an abstract method has no implementation in the abstract superclass.

Example:

```java
abstract class Animal {

    abstract void sound();
}

class Dog extends Animal {

    @Override
    void sound() {
        System.out.println("Bark");
    }

    void test() {
        super.sound(); // ❌
    }
}
```

There is no concrete superclass implementation to execute.

---

# 33. `super` with Interfaces

A class can use:

```java
InterfaceName.super.method();
```

for certain interface default-method situations.

Example:

```java
interface A {

    default void show() {
        System.out.println("A");
    }
}

class Test implements A {

    @Override
    public void show() {

        A.super.show();

        System.out.println("Test");
    }
}
```

Output:

```text
A
Test
```

Here:

```java
A.super.show();
```

explicitly invokes the interface's default method.

This is different from:

```java
super.show();
```

which refers to the superclass.

---

# 34. Multiple Interfaces with Same Default Method

Suppose:

```java
interface A {

    default void show() {
        System.out.println("A");
    }
}

interface B {

    default void show() {
        System.out.println("B");
    }
}
```

A class implementing both must resolve the conflict:

```java
class Test implements A, B {

    @Override
    public void show() {

        A.super.show();
    }
}
```

Output:

```text
A
```

Or:

```java
B.super.show();
```

could be used instead.

This is one of the important uses of `InterfaceName.super`.

---

# 35. `super` vs `this` — Complete Comparison

| Feature                | `this`                               | `super`                        |
| ---------------------- | ------------------------------------ | ------------------------------ |
| Refers to              | Current object/current class context | Immediate parent class context |
| Variable               | `this.x`                             | `super.x`                      |
| Method                 | `this.show()`                        | `super.show()`                 |
| Constructor            | `this()`                             | `super()`                      |
| Constructor target     | Same class                           | Parent class                   |
| Used in static context | ❌                                    | ❌ as instance context          |
| Main purpose           | Current class/object                 | Parent class                   |

Memory trick:

```text
this
 ↓
ME

super
 ↓
MY PARENT
```

---

# 36. `super()` vs `super.method()`

These are different.

```text
super()
```

calls:

```text
Parent constructor
```

while:

```text
super.show()
```

calls:

```text
Parent's show() method
```

Example:

```java
class Child extends Parent {

    Child() {
        super();
    }

    void display() {
        super.show();
    }
}
```

---

# 37. `super.variable` vs `super.method()`

```text
super.variable
      ↓
Access parent field

super.method()
      ↓
Call parent method
```

Example:

```java
class Parent {

    int x = 10;

    void show() {
        System.out.println("Parent");
    }
}

class Child extends Parent {

    int x = 20;

    void test() {

        System.out.println(super.x);

        super.show();
    }
}
```

---

# 38. `super` and Method Overriding

`super` is especially useful when extending rather than completely replacing parent behavior.

Example:

```java
class Employee {

    void work() {
        System.out.println("Employee works");
    }
}

class Developer extends Employee {

    @Override
    void work() {

        super.work();

        System.out.println("Developer writes code");
    }
}
```

Output:

```text
Employee works
Developer writes code
```

This allows the child to:

```text
reuse parent behavior
       +
add specialized behavior
```

---

# 39. Real-World Example — Bank Account

```java
class Account {

    protected double balance;

    Account(double balance) {
        this.balance = balance;
    }

    void displayBalance() {
        System.out.println("Balance: " + balance);
    }
}

class SavingsAccount extends Account {

    private double interestRate;

    SavingsAccount(double balance, double interestRate) {

        super(balance);

        this.interestRate = interestRate;
    }

    @Override
    void displayBalance() {

        super.displayBalance();

        System.out.println("Interest Rate: " + interestRate);
    }
}
```

Here `super` is used to:

```text
1. Initialize parent state
2. Reuse parent method
```

---

# 40. Real-World Backend Example

Suppose:

```text
Notification
      ↑
EmailNotification
```

Parent:

```java
class Notification {

    void send() {
        System.out.println("Preparing notification");
    }
}
```

Child:

```java
class EmailNotification extends Notification {

    @Override
    void send() {

        super.send();

        System.out.println("Sending email");
    }
}
```

This allows the subclass to preserve common parent behavior while adding specialized behavior.

---

# 41. Common Interview Traps

### Trap 1 — `super` refers to any ancestor

❌ Wrong.

It refers to the **immediate superclass**.

---

### Trap 2 — `super.super` is valid

❌ Invalid.

```java
super.super.show(); // ❌
```

---

### Trap 3 — `super()` can appear anywhere

❌ Wrong.

It must be the first statement in the constructor.

---

### Trap 4 — `super()` calls the current constructor

❌ Wrong.

```text
this()  → same class constructor
super() → parent constructor
```

---

### Trap 5 — `super` can access private parent fields

❌ Wrong.

Private members are not directly accessible from the subclass.

---

### Trap 6 — `super` is required for every inherited member

❌ Wrong.

You can often access inherited members directly.

`super` is useful when you explicitly need the parent version.

---

### Trap 7 — `super.show()` uses runtime polymorphism to choose the child method

❌ Wrong.

`super.show()` explicitly targets the parent implementation.

---

### Trap 8 — `super()` can be used in a static method

❌ Wrong.

Constructor invocation is not available in a static method.

---

### Trap 9 — `super` creates a parent object

❌ Wrong.

It refers to the parent portion/context of the current object.

---

### Trap 10 — `super()` and `this()` can both be first

❌ Impossible.

A constructor can have only one first constructor invocation.

---

# 42. Important Concept — `super` Does NOT Create a Parent Object

Consider:

```java
class Parent {

    int x = 10;
}

class Child extends Parent {

    int y = 20;
}
```

When:

```java
Child c = new Child();
```

there is one `Child` object.

Conceptually:

```text
             Child Object
        ┌────────────────────┐
        │ Parent part        │
        │ x = 10             │
        │                    │
        │ Child part         │
        │ y = 20             │
        └────────────────────┘
```

`super` provides access to the parent-class portion of the same object.

It does not create another parent object.

---

# 43. `super` and Object Identity

Consider:

```java
class Parent {

    void show() {
        System.out.println(this == this);
    }
}

class Child extends Parent {

    void test() {
        super.show();
    }
}
```

Calling:

```java
Child c = new Child();
c.test();
```

still operates on the same `Child` object.

Conceptually:

```text
c
 ↓
Child object
 ↓
super.show()
 ↓
Parent implementation
```

The object has not changed.

Only the implementation being explicitly invoked changes.

---

# 44. `super` and `final` Methods

A final method cannot be overridden.

But a subclass can inherit and call it:

```java
class Parent {

    final void show() {
        System.out.println("Parent");
    }
}

class Child extends Parent {

    void test() {
        super.show();
    }
}
```

This is valid because the child is not overriding the final method.

It is simply invoking the inherited method.

---

# 45. `super` and Private Methods

Consider:

```java
class Parent {

    private void show() {
        System.out.println("Parent");
    }
}

class Child extends Parent {

    void show() {
        System.out.println("Child");
    }
}
```

These are not overriding methods.

The parent's private method is not inherited as an overridable method.

Therefore the child cannot use:

```java
super.show();
```

to invoke that private parent method.

---

# 46. `super` and Constructors — Complete Flow

Example:

```java
class A {

    A() {
        System.out.println("A");
    }
}

class B extends A {

    B() {
        super();
        System.out.println("B");
    }
}

class C extends B {

    C() {
        super();
        System.out.println("C");
    }
}
```

Creating:

```java
new C();
```

Output:

```text
A
B
C
```

Flow:

```text
C()
 ↓
B()
 ↓
A()
```

Constructors execute from the top of the inheritance hierarchy down to the actual class.

---

# 47. `super` and `this` Combined

Example:

```java
class Parent {

    int value = 10;

    Parent(int value) {
        this.value = value;
    }

    void show() {
        System.out.println("Parent");
    }
}

class Child extends Parent {

    int value = 20;

    Child(int value) {

        super(value);

        this.value = value + 10;
    }

    @Override
    void show() {

        System.out.println(this.value);

        super.show();
    }
}
```

Here:

```text
super(value)
     ↓
parent constructor

this.value
     ↓
child field

super.show()
     ↓
parent method
```

This is a very useful mental model.

---

# 48. `super` — 30-Second Interview Answer

> **`super` is a special reference used inside a subclass to access members of its immediate superclass. It can access parent variables using `super.variable`, invoke parent methods using `super.method()`, and invoke parent constructors using `super()`. It is especially useful when a child hides a field or overrides a method and needs to explicitly access the parent's version. `super()` must be the first statement in a constructor.**

Example:

```java
class Parent {

    int x = 10;

    void show() {
        System.out.println("Parent");
    }
}

class Child extends Parent {

    int x = 20;

    @Override
    void show() {

        System.out.println(super.x);

        super.show();
    }
}
```

---

# 🔥 Top 10 Most Important Interview Questions + Answers

## 1. What is `super` in Java?

**Answer:**

`super` is a special reference used inside a subclass to access members of its immediate parent class.

It can access:

```text
Parent variables
Parent methods
Parent constructors
```

---

## 2. What are the three main uses of `super`?

**Answer:**

```java
super.variable;
super.method();
super();
```

They respectively:

```text
Access parent variable
Call parent method
Call parent constructor
```

---

## 3. What is the difference between `this` and `super`?

**Answer:**

```text
this
 ↓
Current object/current class context

super
 ↓
Immediate parent-class context
```

And:

```text
this()  → current-class constructor
super() → parent-class constructor
```

---

## 4. Why do we use `super` in method overriding?

**Answer:**

To explicitly call the parent's implementation from the overridden child method.

```java
@Override
void show() {

    System.out.println("Child");

    super.show();
}
```

This allows the child to reuse parent behavior and add its own behavior.

---

## 5. What is `super()`?

**Answer:**

`super()` invokes the no-argument constructor of the immediate parent class.

```java
class Child extends Parent {

    Child() {
        super();
    }
}
```

If the parent has no no-argument constructor, an appropriate parameterized constructor must be called explicitly.

---

## 6. Where must `super()` be written in a constructor?

**Answer:**

An explicit `super(...)` constructor invocation must be the **first statement** of the constructor.

```java
Child() {

    super();

    System.out.println("Child");
}
```

---

## 7. Can `this()` and `super()` be used together in one constructor?

**Answer:**

Not as constructor-invocation statements in the same constructor.

Both must be first, so only one can be used directly.

However, they can participate in different constructors within a constructor chain.

---

## 8. Can `super` access private members of the parent?

**Answer:**

No.

Private members are accessible only inside the class that declares them.

```java
class Parent {

    private int x = 10;
}

class Child extends Parent {

    void show() {

        System.out.println(super.x); // ❌
    }
}
```

---

## 9. Does `super` refer to the grandparent class?

**Answer:**

No.

`super` refers only to the **immediate parent class**.

For:

```text
Animal
   ↑
 Dog
   ↑
Puppy
```

inside `Puppy`:

```java
super
```

refers to:

```text
Dog
```

not directly to `Animal`.

---

## 10. Does `super` create a separate parent object?

**Answer:**

No.

There is still only one actual object.

```text
Child reference
      ↓
   Child object
   ┌───────────────┐
   │ Parent state  │
   │ Child state   │
   └───────────────┘
```

`super` simply provides access to the parent-class members/implementation associated with that object.

---

# ⚡ Final Revision Sheet

```text
                    super
                      |
          +-----------+-----------+
          |           |           |
       Variable     Method     Constructor
          |           |           |
      super.x     super.show()  super()
          |           |           |
      Parent field  Parent method Parent constructor
```

### Remember:

```text
super.x
   ↓
Parent variable

super.show()
   ↓
Parent method

super()
   ↓
Parent constructor

super(...)
   ↓
Parameterized parent constructor
```

And:

```text
this
 ↓
ME

super
 ↓
MY IMMEDIATE PARENT
```

### The Golden Rule

> **`this` works with the current class/object. `super` explicitly works with the immediate parent class.**

---

# 🎯 One-Line Interview Memory

> **`super` is used inside a subclass to access the immediate parent's fields, methods, and constructors, especially when the child needs to explicitly use the parent's implementation.**
