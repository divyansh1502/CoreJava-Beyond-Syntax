# ☕ Java OOP — Association, Aggregation & Composition

> **Association, Aggregation, and Composition describe relationships between objects. They are mainly used to model HAS-A / USES relationships and are fundamental to object-oriented design and LLD.**

---

# 1. Why Do We Need Object Relationships?

So far, we learned:

```text
Inheritance
    ↓
IS-A relationship
```

Example:

```text
Dog IS-A Animal
Car IS-A Vehicle
Student IS-A Person
```

But real-world systems also contain relationships where one object **uses, contains, or depends on another object**.

For example:

```text
Student → uses → Library
Car → has → Engine
House → contains → Room
Department → has → Professor
```

These are object relationships.

This is generally represented using:

```text
HAS-A
USES-A
KNOWS-A
CONTAINS-A
```

---

# 2. Three Important Object Relationships

The three important relationships are:

```text
Association
    ↓
Aggregation
    ↓
Composition
```

Think of them as different levels of relationship strength.

```text
Association
    ↓
General relationship

Aggregation
    ↓
Weak HAS-A relationship

Composition
    ↓
Strong HAS-A relationship
```

---

# 3. Association

## Definition

**Association is a general relationship between two independent objects where one object is connected to or uses another object.**

Example:

```text
Teacher ───── Student
```

A teacher teaches a student.

Both objects can exist independently.

```text
Teacher exists
Student exists
```

Neither object's lifetime depends on the other.

---

# 4. Simple Association Example

```java
class Student {

    String name;

    Student(String name) {
        this.name = name;
    }
}

class Teacher {

    String name;

    Teacher(String name) {
        this.name = name;
    }

    void teach(Student student) {
        System.out.println(name + " teaches " + student.name);
    }
}
```

Usage:

```java
Student s = new Student("Rahul");
Teacher t = new Teacher("Amit");

t.teach(s);
```

Output:

```text
Amit teaches Rahul
```

Here:

```text
Teacher ───── Student
```

is an association.

Both objects can exist independently.

---

# 5. Association Is Not Necessarily HAS-A

Association is broader than HAS-A.

It can represent:

```text
USES-A
KNOWS-A
WORKS-WITH
COMMUNICATES-WITH
```

Example:

```text
Customer ───── Bank
```

A customer interacts with a bank.

This doesn't necessarily mean the bank owns the customer.

---

# 6. Association Can Be One-to-One

Example:

```text
Person ───── Passport
```

One person may have one passport.

Java:

```java
class Person {

    Passport passport;

    Person(Passport passport) {
        this.passport = passport;
    }
}

class Passport {

}
```

Relationship:

```text
Person 1 ───── 1 Passport
```

---

# 7. Association Can Be One-to-Many

Example:

```text
Teacher ───── Students
```

One teacher can teach multiple students.

```java
class Teacher {

    List<Student> students;

    Teacher(List<Student> students) {
        this.students = students;
    }
}
```

Relationship:

```text
Teacher 1 ───── * Students
```

Where:

```text
1 = one
* = many
```

---

# 8. Association Can Be Many-to-Many

Example:

```text
Student ───── Course
```

A student can enroll in multiple courses.

A course can have multiple students.

```text
Student * ───── * Course
```

This is a many-to-many association.

---

# 9. Aggregation

## Definition

**Aggregation is a weak HAS-A relationship where one object contains or groups other objects, but the contained objects can exist independently.**

Example:

```text
Department ───── Professor
```

A department has professors.

But if the department is removed:

```text
Department ❌
Professor  ✅
```

The professors can still exist.

Therefore, this is aggregation.

---

# 10. Aggregation Example

```java
class Professor {

    String name;

    Professor(String name) {
        this.name = name;
    }
}

class Department {

    private Professor professor;

    Department(Professor professor) {
        this.professor = professor;
    }
}
```

Usage:

```java
Professor p = new Professor("Dr. Sharma");

Department d = new Department(p);
```

Both objects have independent lifetimes.

```text
Professor
    ↑
    |
Department
```

The department uses a professor, but does not control the professor's lifetime.

---

# 11. Important Property of Aggregation

In aggregation:

```text
Whole
  ↓
contains
  ↓
Part
```

But:

```text
Whole can be destroyed
Part can still exist
```

Example:

```text
University
    ↓
Department
```

If the university object is removed, a department object could theoretically continue to exist independently in the model.

---

# 12. Composition

## Definition

**Composition is a strong HAS-A relationship where the contained object's lifecycle is strongly tied to the containing object.**

Example:

```text
House ───── Room
```

A room is considered a part of a particular house.

Conceptually:

```text
House created
    ↓
Rooms belong to the House

House destroyed
    ↓
Those Rooms no longer have an independent role
```

This is composition.

---

# 13. Composition Example

```java
class Engine {

    void start() {
        System.out.println("Engine started");
    }
}

class Car {

    private Engine engine;

    Car() {
        engine = new Engine();
    }

    void startCar() {
        engine.start();
    }
}
```

Usage:

```java
Car car = new Car();

car.startCar();
```

Here:

```text
Car
 ↓
creates
 ↓
Engine
```

The `Car` controls the creation of its `Engine`.

This is a strong form of ownership.

---

# 14. Key Difference: Aggregation vs Composition

This is one of the **most important interview questions**.

### Aggregation

```text
Whole ───── Part
```

Part can exist independently.

Example:

```text
Department ───── Professor
```

### Composition

```text
Whole ───── Part
```

Part's lifecycle is tied to the whole.

Example:

```text
House ───── Room
```

Memory trick:

```text
Aggregation
    ↓
Weak ownership

Composition
    ↓
Strong ownership
```

---

# 15. Real-World Example

Consider a university.

```text
University
    |
    +── Department
    |
    +── Professor
    |
    +── Student
```

Possible relationships:

```text
University ───── Professor
       Aggregation

University ───── Student
       Aggregation

University ───── Department
       Aggregation
```

But inside a particular object model:

```text
House ───── Room
       Composition
```

The exact classification depends on the **domain model and lifecycle semantics**.

Do not blindly memorize examples.

---

# 16. Association vs Aggregation vs Composition

| Feature               | Association          | Aggregation | Composition       |
| --------------------- | -------------------- | ----------- | ----------------- |
| Relationship          | General              | Weak HAS-A  | Strong HAS-A      |
| Ownership             | None/weak            | Weak        | Strong            |
| Lifecycle dependency  | No                   | Usually no  | Yes               |
| Independent existence | Yes                  | Yes         | No                |
| Represents            | Uses/knows/connected | Whole-part  | Strong whole-part |
| Strength              | General              | Medium      | Strong            |

---

# 17. Simple Memory Trick

Remember:

```text
Association
    ↓
Connected

Aggregation
    ↓
Has, but independent

Composition
    ↓
Has, and dependent
```

Or:

```text
A → Association → Any relationship

G → Aggregation → Grouping

C → Composition → Controlled lifecycle
```

---

# 18. Association vs Inheritance

These are completely different relationships.

### Inheritance

```text
Dog IS-A Animal
```

### Association

```text
Dog USES-A Toy
```

Therefore:

```text
IS-A
    ↓
Inheritance

HAS-A / USES-A
    ↓
Association / Aggregation / Composition
```

---

# 19. IS-A vs HAS-A

This is an important interview concept.

### IS-A

Represents inheritance.

```text
Dog IS-A Animal
```

Java:

```java
class Dog extends Animal {

}
```

### HAS-A

Represents composition/aggregation/association.

```text
Car HAS-A Engine
```

Java:

```java
class Car {

    Engine engine;

}
```

---

# 20. Inheritance vs Composition

A common design principle is:

> **Prefer composition over inheritance when the relationship is not genuinely an IS-A relationship.**

Example:

Bad conceptual model:

```text
Car extends Engine
```

This says:

```text
Car IS-A Engine
```

which is incorrect.

Better:

```java
class Car {

    private Engine engine;

}
```

This says:

```text
Car HAS-A Engine
```

---

# 21. Why Is Composition Powerful?

Composition allows a class to use another class without becoming that class.

Example:

```java
class PaymentService {

    private PaymentGateway gateway;

    PaymentService(PaymentGateway gateway) {
        this.gateway = gateway;
    }
}
```

Now:

```text
PaymentService
      |
      HAS-A
      ↓
PaymentGateway
```

The service can use different gateway implementations.

This becomes extremely useful in:

```text
Spring Boot
Dependency Injection
LLD
Design Patterns
System Design
```

---

# 22. Composition Through Constructor Injection

A very common design:

```java
class OrderService {

    private PaymentService paymentService;

    OrderService(PaymentService paymentService) {
        this.paymentService = paymentService;
    }
}
```

Usage:

```java
PaymentService payment = new PaymentService();

OrderService order = new OrderService(payment);
```

Here:

```text
OrderService
      ↓
depends on
      ↓
PaymentService
```

This is composition/association through object references.

---

# 23. Important Interview Point

**Having a reference to another object does not automatically mean composition.**

For example:

```java
class Teacher {

    Student student;

}
```

This only tells us:

```text
Teacher has a reference to Student
```

Whether it is:

```text
Association
Aggregation
Composition
```

depends on the **ownership and lifecycle relationship** in the domain.

This is a very important nuance.

---

# 24. Aggregation Using Constructor Injection

```java
class Employee {

    String name;

    Employee(String name) {
        this.name = name;
    }
}

class Company {

    private Employee employee;

    Company(Employee employee) {
        this.employee = employee;
    }
}
```

Usage:

```java
Employee e = new Employee("Rahul");

Company c = new Company(e);
```

The `Employee` exists independently:

```text
Employee created first
       ↓
Company receives Employee
```

This models aggregation/association depending on the domain semantics.

---

# 25. Composition Through Internal Creation

```java
class Heart {

}

class Human {

    private Heart heart;

    Human() {
        heart = new Heart();
    }
}
```

The `Human` creates its `Heart`.

Conceptually:

```text
Human
  ↓
owns
  ↓
Heart
```

This represents stronger composition.

---

# 26. Important Trap: Java Does Not Have Special Keywords

Java has no:

```text
aggregation
composition
```

keywords.

You simply use:

```java
class
objects
references
constructors
methods
```

The relationship is determined by the **design and lifecycle semantics**, not by a special Java keyword.

---

# 27. UML Notation

These relationships are commonly represented in UML.

### Association

```text
Class A ───────── Class B
```

### Aggregation

```text
Class A ◇──────── Class B
```

Open diamond:

```text
◇
```

means aggregation.

### Composition

```text
Class A ◆──────── Class B
```

Filled diamond:

```text
◆
```

means composition.

Memory trick:

```text
◇ → Aggregation
◆ → Composition
```

---

# 28. Example UML

```text
Department ◇──────── Professor
```

Aggregation.

```text
House ◆──────── Room
```

Composition.

```text
Teacher ───────── Student
```

Association.

---

# 29. Can Aggregation Become Composition?

Yes, depending on the domain model.

Suppose initially:

```text
Team ───── Player
```

Players can exist independently.

This may be aggregation.

But if a model defines:

```text
Order ◆──── OrderItem
```

where an `OrderItem` exists only as part of its specific order, this is naturally modeled as composition.

The classification depends on:

```text
Ownership
+
Lifecycle
+
Domain meaning
```

---

# 30. Association Is the Broadest Concept

Think of the hierarchy conceptually as:

```text
Object Relationship
       |
       +── Association
              |
              +── Aggregation
              |
              +── Composition
```

But remember:

**Aggregation and composition are specialized forms of association in common OOP/UML terminology.**

---

# 31. Example — QRder

This concept is directly useful for your QRder project.

Suppose:

```text
Restaurant
    |
    +── Food
    |
    +── Order
```

A restaurant may have food items.

```text
Restaurant ───── Food
```

Depending on your business model, food objects might exist independently of a particular restaurant.

That could be modeled as aggregation/association.

Now consider:

```text
Order
   |
   +── OrderItem
   +── OrderItem
   +── OrderItem
```

An `OrderItem` represents an item belonging to a particular order.

Conceptually:

```text
Order ◆──── OrderItem
```

This is a strong candidate for composition because the order item has meaning as part of that order.

---

# 32. Composition in Your QRder Model

For example:

```java
class OrderItem {

    private Food food;
    private int quantity;

    OrderItem(Food food, int quantity) {
        this.food = food;
        this.quantity = quantity;
    }
}
```

Then:

```java
class Order {

    private ArrayList<OrderItem> items;

    Order() {
        items = new ArrayList<>();
    }
}
```

Conceptually:

```text
Order
  |
  ◆
  |
OrderItem
```

The order owns its collection of order items.

---

# 33. Association Example in Backend

Consider:

```text
Customer ───── SupportAgent
```

A customer may interact with a support agent.

Neither owns the other.

Therefore:

```text
Customer
    │
    │ communicates with
    ↓
SupportAgent
```

This is association.

---

# 34. Aggregation Example in Backend

Consider:

```text
Department ───── Employee
```

The department groups employees, but employees can exist independently.

```text
Department
    ◇
    |
Employee
```

Aggregation.

---

# 35. Composition Example in Backend

Consider:

```text
Order ───── OrderItem
```

An order contains its order items.

```text
Order
  ◆
  |
OrderItem
```

Composition.

---

# 36. Interview Question: Is Composition Stronger Than Aggregation?

### Answer

Yes.

Composition represents stronger ownership.

```text
Aggregation
    ↓
Weak whole-part relationship
    ↓
Part can exist independently

Composition
    ↓
Strong whole-part relationship
    ↓
Part's lifecycle depends on whole
```

---

# 37. Interview Question: Is Composition Inheritance?

### Answer

No.

Composition represents:

```text
HAS-A
```

Inheritance represents:

```text
IS-A
```

Example:

```text
Dog IS-A Animal
```

Inheritance.

```text
Car HAS-A Engine
```

Composition/association depending on lifecycle semantics.

---

# 38. Interview Question: Which Is More Flexible — Inheritance or Composition?

Composition is often more flexible for combining behavior because objects can be composed from different collaborating components without creating a rigid inheritance hierarchy.

For example:

```java
class Car {

    private Engine engine;
}
```

The car can collaborate with different engine implementations.

This is one reason modern software design frequently favors composition where appropriate.

---

# 39. Interview Question: Why Prefer Composition Over Inheritance?

Common reasons include:

```text
1. Lower coupling

2. Greater flexibility

3. Easier replacement of components

4. Avoids deep inheritance hierarchies

5. Supports dependency injection

6. Better separation of responsibilities
```

But this does **not** mean inheritance is bad.

Use inheritance when there is a genuine:

```text
IS-A
```

relationship and polymorphism is appropriate.

---

# 40. Interview Question: Can Composition Exist Without Inheritance?

Yes.

Example:

```java
class Car {

    private Engine engine;

}
```

No inheritance is required.

The relationship is created through object references.

---

# 41. Interview Question: Can Association Exist Without Aggregation?

Yes.

Association is the general relationship.

Example:

```java
class Teacher {

    void teach(Student student) {

    }
}
```

The teacher simply interacts with the student.

No ownership is required.

---

# 42. Interview Question: Can Aggregation Exist Without Composition?

Yes.

They represent different lifecycle/ownership semantics.

```text
Aggregation
    ↓
Independent part

Composition
    ↓
Dependent part
```

---

# 43. Interview Question: What Happens When the Whole Object Is Destroyed?

### Association

No lifecycle dependency.

```text
A destroyed
B can exist
```

### Aggregation

The part can still exist.

```text
Whole destroyed
    ↓
Part survives
```

### Composition

The part's lifecycle is tied to the whole.

```text
Whole destroyed
    ↓
Part has no independent existence in that model
```

---

# 44. Interview Trap — "Composition Means Physically Creating the Object"

Not necessarily.

Composition is fundamentally about:

```text
Strong ownership
+
Lifecycle dependency
```

Creating the object internally is a common implementation technique:

```java
class Car {

    private Engine engine = new Engine();

}
```

But simply using `new` is not the definition of composition.

---

# 45. Interview Trap — "Aggregation Means ArrayList"

Wrong.

This:

```java
class Department {

    ArrayList<Employee> employees;

}
```

does not automatically mean aggregation.

You must consider:

```text
Who owns the employees?
Can employees exist independently?
What is the domain lifecycle?
```

The collection itself does not determine the relationship.

---

# 46. Interview Trap — "HAS-A Always Means Composition"

Wrong.

HAS-A can describe different relationships.

```text
HAS-A
  |
  +── Association
  +── Aggregation
  +── Composition
```

The lifecycle and ownership determine the more precise classification.

---

# 47. Interview Trap — "Inheritance Is Always Better for Reuse"

Wrong.

Inheritance should represent a meaningful:

```text
IS-A
```

relationship.

For simple code reuse, composition may be a better design.

---

# 48. Composition and Dependency Injection

Composition is closely related to dependency injection.

Example:

```java
class OrderService {

    private PaymentService paymentService;

    OrderService(PaymentService paymentService) {
        this.paymentService = paymentService;
    }
}
```

The dependency is supplied from outside.

This allows:

```text
OrderService
     ↓
PaymentService
```

to be easily replaced.

For example:

```java
PaymentService payment1 = new UpiPaymentService();
PaymentService payment2 = new CardPaymentService();
```

This idea becomes extremely important when you learn:

```text
Spring
Dependency Injection
Interfaces
Design Patterns
LLD
```

---

# 49. Composition vs Inheritance — Interview Comparison

| Feature             | Inheritance         | Composition                      |
| ------------------- | ------------------- | -------------------------------- |
| Relationship        | IS-A                | HAS-A                            |
| Reuse               | Through inheritance | Through delegation/collaboration |
| Coupling            | Often tighter       | Often looser                     |
| Flexibility         | Lower               | Higher                           |
| Runtime replacement | Limited             | Easier                           |
| Hierarchy           | Creates hierarchy   | Creates object relationships     |
| Example             | Dog → Animal        | Car → Engine                     |

---

# 50. Association vs Aggregation vs Composition — Final Comparison

```text
                 Object Relationship
                         |
        +----------------+----------------+
        |                |                |
   Association       Aggregation     Composition
        |                |                |
   General link      Weak HAS-A      Strong HAS-A
        |                |                |
   No ownership      Weak ownership  Strong ownership
        |                |                |
 Independent        Independent     Lifecycle dependent
 lifetime           lifetime        lifetime
```

---

# 51. Real-World Mental Model

Imagine a company:

```text
Company
   |
   +── Employees
   |
   +── Departments
```

Employees can exist independently:

```text
Company ───── Employee
    Aggregation
```

Now imagine:

```text
Order
   |
   +── OrderItems
```

Order items belong to the order:

```text
Order ◆──── OrderItem
    Composition
```

Now:

```text
Customer ───── DeliveryAgent
```

They simply interact:

```text
Customer ───── DeliveryAgent
       Association
```

---

# 52. The Most Important Rule

Don't memorize:

```text
Car → Engine = Composition
University → Student = Aggregation
Teacher → Student = Association
```

as absolute rules.

Instead ask:

```text
1. Are the objects related?
        ↓
   Association

2. Does one object weakly contain/group another?
        ↓
   Aggregation

3. Does one object strongly own the other's lifecycle?
        ↓
   Composition
```

The **domain and lifecycle** determine the relationship.

---

# 53. Common Interview Traps

### Trap 1

```text
IS-A = HAS-A
```

❌ Wrong.

```text
IS-A → Inheritance

HAS-A → Object relationship
```

---

### Trap 2

Aggregation and composition are the same.

❌ Wrong.

```text
Aggregation → weak ownership

Composition → strong ownership
```

---

### Trap 3

Creating an object using `new` automatically means composition.

❌ Wrong.

Lifecycle and ownership matter.

---

### Trap 4

Using `ArrayList` means aggregation.

❌ Wrong.

The collection implementation does not determine the semantic relationship.

---

### Trap 5

Composition requires inheritance.

❌ Wrong.

Composition can exist completely independently of inheritance.

---

### Trap 6

Composition is always better than inheritance.

❌ Wrong.

Use the relationship that correctly represents the domain.

---

### Trap 7

Association means ownership.

❌ Wrong.

Association can simply mean that two objects interact.

---

### Trap 8

Aggregation means the child object cannot exist independently.

❌ Wrong.

Independent existence is one of the main distinctions from composition.

---

### Trap 9

Composition is only about physical destruction.

Not exactly.

The important idea is **lifecycle dependency in the domain model**.

---

### Trap 10

All HAS-A relationships are composition.

❌ Wrong.

HAS-A is commonly used broadly; the precise relationship depends on ownership and lifecycle.

---

# 54. Top Interview Questions — With Answers

## Q1. What is Association?

**Answer:**

Association is a general relationship between two independent objects where they are connected, interact, or use each other.

Example:

```text
Teacher ───── Student
```

---

## Q2. What is Aggregation?

**Answer:**

Aggregation is a weak whole-part relationship where one object contains or groups another object, but the part can exist independently.

Example:

```text
Department ◇──── Employee
```

---

## Q3. What is Composition?

**Answer:**

Composition is a strong whole-part relationship where the part's lifecycle is tied to the whole.

Example:

```text
Order ◆──── OrderItem
```

---

## Q4. What is the difference between Aggregation and Composition?

**Answer:**

The key difference is lifecycle and ownership.

```text
Aggregation
→ weak ownership
→ part can exist independently

Composition
→ strong ownership
→ part's lifecycle depends on whole
```

---

## Q5. What is the difference between Association and Inheritance?

**Answer:**

Inheritance represents an:

```text
IS-A
```

relationship.

Association represents a general relationship between objects such as:

```text
USES-A
HAS-A
KNOWS-A
```

---

## Q6. Why is composition often preferred over inheritance?

**Answer:**

Composition can provide lower coupling, greater flexibility, easier component replacement, and better separation of responsibilities.

However, inheritance is appropriate when there is a genuine IS-A relationship.

---

## Q7. Can composition exist without inheritance?

**Answer:**

Yes.

```java
class Car {

    private Engine engine;

}
```

The relationship uses object composition without inheritance.

---

## Q8. Does creating an object inside another class automatically mean composition?

**Answer:**

No.

```java
class Car {

    Engine engine = new Engine();

}
```

This can indicate strong ownership, but `new` itself does not define composition.

The domain relationship and lifecycle are what matter.

---

## Q9. What are the UML symbols for aggregation and composition?

**Answer:**

```text
Aggregation  → ◇ Open diamond

Composition  → ◆ Filled diamond
```

---

## Q10. What is the relationship between Association, Aggregation, and Composition?

**Answer:**

Association is the broad general relationship.

Aggregation and composition are commonly treated as more specific whole-part forms of association.

```text
Association
    |
    +── Aggregation
    |
    +── Composition
```

---

## Q11. What is HAS-A relationship?

**Answer:**

HAS-A represents a relationship where one object contains, uses, or collaborates with another object.

Example:

```java
class Car {

    Engine engine;

}
```

Conceptually:

```text
Car HAS-A Engine
```

---

## Q12. What is IS-A relationship?

**Answer:**

IS-A represents inheritance.

```java
class Dog extends Animal {

}
```

Conceptually:

```text
Dog IS-A Animal
```

---

## Q13. Can aggregation and composition be implemented using the same Java syntax?

**Answer:**

Yes.

Both can be represented using object references.

For example:

```java
class A {

    B b;

}
```

The Java syntax alone does not tell us whether the relationship is association, aggregation, or composition. The design semantics determine that.

---

## Q14. Which has stronger ownership: aggregation or composition?

**Answer:**

Composition.

```text
Aggregation → weak ownership

Composition → strong ownership
```

---

## Q15. What happens to the part when the whole is destroyed?

**Answer:**

In aggregation, the part can continue to exist independently.

In composition, the part's lifecycle is tied to the whole in the domain model.

---

# 55. Top 10 Most Important Interview Questions

```text
1. What is Association?

2. What is Aggregation?

3. What is Composition?

4. What is the difference between Aggregation and Composition?

5. What is the difference between IS-A and HAS-A?

6. Why is composition often preferred over inheritance?

7. Can composition exist without inheritance?

8. Does creating an object using new automatically mean composition?

9. What are the UML symbols for Aggregation and Composition?

10. How are Association, Aggregation, and Composition related?
```

---

# 56. 30-Second Interview Answer

> **Association, Aggregation, and Composition represent relationships between objects. Association is a general relationship where objects interact or use each other. Aggregation is a weak whole-part relationship where the part can exist independently. Composition is a strong whole-part relationship where the part's lifecycle is tied to the whole. In Java, these relationships are implemented using object references, constructors, collections, and method interactions rather than special keywords.**

---

# 57. Final Memory Trick

```text
             OBJECT RELATIONSHIPS
                     |
        +------------+------------+
        |            |            |
   Association   Aggregation  Composition
        |            |            |
    Connected      Weak HAS-A   Strong HAS-A
        |            |            |
    No ownership   Independent   Dependent
                   lifetime      lifecycle
```

And remember:

```text
IS-A
 ↓
Inheritance

HAS-A
 ↓
Association / Aggregation / Composition

WEAK HAS-A
 ↓
Aggregation

STRONG HAS-A
 ↓
Composition
```

> **Interview shortcut:**
>
> **Association = "They are related."**
>
> **Aggregation = "I have it, but it can live without me."**
>
> **Composition = "I own it; its lifecycle belongs to me."**
