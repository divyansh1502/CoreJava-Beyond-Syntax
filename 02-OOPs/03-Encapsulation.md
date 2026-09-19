# 🔐 Java OOP — Encapsulation

> **Encapsulation is the practice of keeping an object's state and the operations that manage that state together, while controlling how the internal state can be accessed or modified.**

---

# 📚 Table of Contents

* [1. What is Encapsulation?](#-1-what-is-encapsulation)
* [2. Why Do We Need Encapsulation?](#-2-why-do-we-need-encapsulation)
* [3. Real-World Analogy](#-3-real-world-analogy)
* [4. Encapsulation in Java](#-4-encapsulation-in-java)
* [5. Basic Example](#-5-basic-example)
* [6. Data Hiding](#-6-data-hiding)
* [7. Encapsulation vs Data Hiding](#-7-encapsulation-vs-data-hiding)
* [8. Access Modifiers](#-8-access-modifiers)
* [9. Private Members](#-9-private-members)
* [10. Getters and Setters](#-10-getters-and-setters)
* [11. Why Not Make Everything Public?](#-11-why-not-make-everything-public)
* [12. Controlled Access](#-12-controlled-access)
* [13. Validation](#-13-validation)
* [14. Encapsulation and Invariants](#-14-encapsulation-and-invariants)
* [15. Encapsulation Does Not Mean Getters/Setters](#-15-encapsulation-does-not-mean-gettersetters)
* [16. Encapsulation vs Abstraction](#-16-encapsulation-vs-abstraction)
* [17. Encapsulation vs Inheritance](#-17-encapsulation-vs-inheritance)
* [18. Encapsulation and Constructors](#-18-encapsulation-and-constructors)
* [19. Encapsulation and Immutability](#-19-encapsulation-and-immutability)
* [20. Defensive Copying](#-20-defensive-copying)
* [21. Encapsulation in Real Projects](#-21-encapsulation-in-real-projects)
* [22. Benefits](#-22-benefits)
* [23. Disadvantages / Trade-offs](#-23-disadvantages--trade-offs)
* [24. Common Mistakes](#-24-common-mistakes)
* [25. Interview Questions](#-25-interview-questions)
* [26. Top 10 Interview Questions](#-26-top-10-interview-questions)
* [27. Quick Revision](#-27-quick-revision)
* [28. 30-Second Interview Answer](#-28-30-second-interview-answer)
* [29. Memory Trick](#-29-memory-trick)

---

# 🧠 1. What is Encapsulation?

**Encapsulation** comes from the idea of enclosing/bundling things together.

In OOP, it means keeping:

```text
State
+
Behavior that manages the state
```

together inside a class, while providing controlled access to that state.

Example:

```java
class BankAccount {

    private double balance;

    public void deposit(double amount) {

        if (amount > 0) {
            balance += amount;
        }
    }

    public double getBalance() {
        return balance;
    }
}
```

Here:

```text
BankAccount
│
├── State
│     └── balance
│
└── Behavior
      ├── deposit()
      └── getBalance()
```

And:

```java
private double balance;
```

prevents direct access from unrelated classes.

---

# 🤔 2. Why Do We Need Encapsulation?

Suppose we write:

```java
class BankAccount {

    public double balance;
}
```

Now anyone can do:

```java
BankAccount account = new BankAccount();

account.balance = -50000;
```

This may violate the rules of the domain.

A bank account should not normally allow arbitrary code to directly change its balance.

Instead:

```java
class BankAccount {

    private double balance;

    public void deposit(double amount) {

        if (amount <= 0) {
            return;
        }

        balance += amount;
    }
}
```

Now the class controls how its state changes.

### Core idea

```text
Without encapsulation:

External Code
      ↓
Directly modifies state
      ↓
Invalid state possible


With encapsulation:

External Code
      ↓
Controlled method
      ↓
Validation / business rules
      ↓
State changes safely
```

---

# 🌎 3. Real-World Analogy

Think about an ATM.

You don't directly manipulate the bank's database.

You interact through controlled operations:

```text
Withdraw
Deposit
Transfer
Check Balance
```

Conceptually:

```text
                 ATM
                  │
        ┌─────────┼─────────┐
        ▼         ▼         ▼
     Withdraw   Deposit   Balance
        │         │
        └────┬────┘
             ▼
       Controlled access
             │
             ▼
       Account State
```

You don't get direct access to:

```text
database.balance
```

Instead, operations control what can happen.

This is similar to encapsulation.

---

# ☕ 4. Encapsulation in Java

Java provides several mechanisms that help implement encapsulation.

Most importantly:

```text
Classes
+
Access Modifiers
+
Private Fields
+
Controlled Methods
```

Example:

```java
class Employee {

    private double salary;

    public double getSalary() {
        return salary;
    }

    public void setSalary(double salary) {

        if (salary >= 0) {
            this.salary = salary;
        }
    }
}
```

The field:

```java
private double salary;
```

cannot be directly accessed from unrelated classes.

---

# 💻 5. Basic Example

## ❌ Without Encapsulation

```java
class Employee {

    public double salary;
}
```

Usage:

```java
Employee e = new Employee();

e.salary = -100000;
```

There is no control over the state change.

---

## ✅ With Encapsulation

```java
class Employee {

    private double salary;

    public double getSalary() {
        return salary;
    }

    public void setSalary(double salary) {

        if (salary >= 0) {
            this.salary = salary;
        }
    }
}
```

Usage:

```java
Employee e = new Employee();

e.setSalary(50000);

System.out.println(e.getSalary());
```

Output:

```text
50000.0
```

Invalid values can be rejected:

```java
e.setSalary(-1000);
```

---

# 🔒 6. Data Hiding

**Data hiding** means restricting direct access to internal data.

For example:

```java
private double balance;
```

External classes cannot directly do:

```java
account.balance;
```

if `balance` is private.

Instead, the class exposes controlled operations.

```java
account.deposit(1000);
```

### Important relationship

```text
Encapsulation
      │
      ├── Bundles state + behavior
      │
      └── Controls access
               │
               ▼
          Data hiding
```

Data hiding is therefore closely related to encapsulation, but they are **not exactly synonymous**.

---

# 🆚 7. Encapsulation vs Data Hiding

| Encapsulation                                    | Data Hiding                                              |
| ------------------------------------------------ | -------------------------------------------------------- |
| Broader design concept                           | Specific access-control goal                             |
| Bundles state and behavior                       | Restricts visibility/access                              |
| Focuses on controlled interaction                | Focuses on hiding implementation/state                   |
| Achieved through class design and access control | Commonly implemented using access modifiers              |
| Can include behavior-level APIs                  | Primarily concerns what outside code can directly access |

### Interview answer

> **Data hiding is a technique/goal of restricting access to internal details, while encapsulation is the broader practice of bundling state and behavior together and controlling how that state is accessed.**

---

# 🛡️ 8. Access Modifiers

Java provides four access levels:

```text
public
protected
default / package-private
private
```

### Visibility overview

| Modifier    | Same Class | Same Package | Subclass in Other Package | Other Package |
| ----------- | ---------: | -----------: | ------------------------: | ------------: |
| `private`   |          ✅ |            ❌ |                         ❌ |             ❌ |
| default     |          ✅ |            ✅ |  ⚠️ Through package rules |             ❌ |
| `protected` |          ✅ |            ✅ |                        ✅* |            ❌* |
| `public`    |          ✅ |            ✅ |                         ✅ |             ✅ |

`protected` has special rules for subclasses in other packages.

---

# 🔐 9. Private Members

The most commonly used access modifier for encapsulating internal state is:

```java
private
```

Example:

```java
class Account {

    private double balance;
}
```

Another class cannot directly access:

```java
account.balance;
```

This gives the class control over how the state is exposed or modified.

---

# 🎯 10. Getters and Setters

Getters and setters are conventional methods used to read and modify fields.

Example:

```java
class Student {

    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

Usage:

```java
Student s = new Student();

s.setName("Rahul");

System.out.println(s.getName());
```

### Getter

Reads state:

```java
getName()
```

### Setter

Changes state:

```java
setName(...)
```

---

# ⚠️ 11. Why Not Make Everything Public?

Consider:

```java
class BankAccount {

    public double balance;
}
```

External code can do:

```java
account.balance = -50000;
```

There is no centralized validation.

With:

```java
private double balance;
```

you can enforce rules:

```java
public void deposit(double amount) {

    if (amount <= 0) {
        throw new IllegalArgumentException("Invalid amount");
    }

    balance += amount;
}
```

Now the class controls the state transition.

---

# 🎛️ 12. Controlled Access

Encapsulation doesn't mean:

> "Nobody can access the data."

It means:

> **Outside code interacts with the object through a controlled API.**

For example:

```java
class BankAccount {

    private double balance;

    public void deposit(double amount) {

        if (amount > 0) {
            balance += amount;
        }
    }

    public boolean withdraw(double amount) {

        if (amount <= 0 || amount > balance) {
            return false;
        }

        balance -= amount;
        return true;
    }

    public double getBalance() {
        return balance;
    }
}
```

External code doesn't directly manipulate:

```java
balance
```

Instead:

```text
deposit()
withdraw()
getBalance()
```

form the public API.

---

# ✅ 13. Validation

One major advantage of encapsulation is enforcing validation.

Example:

```java
class Employee {

    private int age;

    public void setAge(int age) {

        if (age >= 18 && age <= 60) {
            this.age = age;
        }
    }

    public int getAge() {
        return age;
    }
}
```

Now:

```java
Employee e = new Employee();

e.setAge(25);   // valid
e.setAge(10);   // rejected
```

The object can maintain valid state.

---

# 🧠 14. Encapsulation and Invariants

An **invariant** is a condition that should remain true for a valid object.

For example:

```text
Bank Account:

balance >= 0
```

If we expose:

```java
public double balance;
```

external code can violate the invariant.

With encapsulation:

```java
private double balance;
```

we can make state changes pass through controlled operations.

```java
public void withdraw(double amount) {

    if (amount <= balance) {
        balance -= amount;
    }
}
```

The class is responsible for protecting its own rules.

### Important design principle

> **An object should ideally protect the conditions that must remain true for it to be valid.**

---

# 🚫 15. Encapsulation Does Not Mean Getters/Setters

This is a **very important interview trap**.

Many beginners memorize:

> Encapsulation = private variables + getters + setters.

That's incomplete.

Consider:

```java
class BankAccount {

    private double balance;

    public void deposit(double amount) {

        if (amount > 0) {
            balance += amount;
        }
    }
}
```

There is no setter:

```java
setBalance()
```

And that's intentional.

Why?

Because allowing:

```java
account.setBalance(999999);
```

may bypass business rules.

Instead:

```java
account.deposit(1000);
```

represents a meaningful state transition.

### Better encapsulation

```text
Don't expose:
setBalance()

Expose:
deposit()
withdraw()
```

when those operations better represent the domain.

---

# 🆚 16. Encapsulation vs Abstraction

These are frequently confused.

| Encapsulation                           | Abstraction                              |
| --------------------------------------- | ---------------------------------------- |
| Controls access to state/implementation | Focuses on essential behavior            |
| Protects object internals               | Hides unnecessary implementation details |
| Often uses access modifiers             | Often uses interfaces/abstract classes   |
| Focus: controlled access                | Focus: what should be exposed            |
| "How do I control access?"              | "What should the user need to know?"     |

### Example

```java
class Car {

    private Engine engine;

    public void start() {
        engine.start();
    }
}
```

Encapsulation:

```text
engine is internal
```

Abstraction:

```text
user calls start()
without needing to know engine's internal implementation
```

They often work together.

---

# 🧬 17. Encapsulation vs Inheritance

These concepts solve different problems.

### Encapsulation

Protects and controls internal state.

```text
private balance
```

### Inheritance

Creates a relationship where a class extends another class.

```java
class Dog extends Animal {
}
```

Think:

```text
Encapsulation → Protection / controlled access

Inheritance   → Reuse / specialization
```

---

# 🏗️ 18. Encapsulation and Constructors

Constructors can establish valid initial state.

Example:

```java
class Employee {

    private String name;
    private double salary;

    Employee(String name, double salary) {

        if (salary < 0) {
            throw new IllegalArgumentException("Salary cannot be negative");
        }

        this.name = name;
        this.salary = salary;
    }
}
```

Now an object cannot be constructed with an invalid salary through this constructor.

```java
Employee e = new Employee("Rahul", 50000);
```

This is another way to protect object invariants.

---

# 🧊 19. Encapsulation and Immutability

These concepts are related but **not the same**.

### Encapsulation

Controls access to state.

### Immutability

Means an object's observable state cannot be changed after construction.

Example:

```java
final class Person {

    private final String name;
    private final int age;

    Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }
}
```

There are no setters.

The fields are:

```java
private final
```

This supports immutability.

### Important

```text
Encapsulation ≠ Immutability
```

An encapsulated object can still be mutable.

---

# 🛡️ 20. Defensive Copying

This is an advanced but important encapsulation technique.

Suppose:

```java
class Student {

    private List<String> subjects;
}
```

If we return the actual mutable list:

```java
public List<String> getSubjects() {
    return subjects;
}
```

external code can modify internal state:

```java
student.getSubjects().clear();
```

That can break encapsulation.

A defensive approach could return an unmodifiable view or copy depending on the design.

For example:

```java
public List<String> getSubjects() {
    return List.copyOf(subjects);
}
```

Now callers cannot use the returned list to mutate the original list.

### Core idea

> **Don't accidentally expose mutable internal objects if the class needs to control them.**

---

# 🏢 21. Encapsulation in Real Projects

Consider your **Bank Management System**.

Bad design:

```java
class Account {

    public double balance;
}
```

Then any part of the program can do:

```java
account.balance = -999999;
```

Better:

```java
class Account {

    private double balance;

    public void deposit(double amount) {

        if (amount > 0) {
            balance += amount;
        }
    }

    public boolean withdraw(double amount) {

        if (amount > 0 && amount <= balance) {
            balance -= amount;
            return true;
        }

        return false;
    }

    public double getBalance() {
        return balance;
    }
}
```

Now the account controls its own state.

This is exactly the type of thinking expected in real backend development.

---

# 🚀 22. Benefits

## 🔐 1. Data Protection

Internal state can be protected from direct modification.

---

## ✅ 2. Validation

Business rules can be applied before changing state.

---

## 🧩 3. Maintainability

Implementation can change without requiring every caller to know the internal details.

---

## 🔄 4. Controlled Modification

State changes can happen only through approved operations.

---

## 🧠 5. Better Design

Objects become responsible for managing their own state.

---

## 🔧 6. Reduced Coupling

External code depends on the object's public API rather than its internal representation.

---

## 🧪 7. Easier Testing

Business rules can be centralized in methods.

---

# ⚖️ 23. Disadvantages / Trade-offs

Encapsulation is generally useful, but poor use can create unnecessary complexity.

### 1. More code

Private fields and methods may require additional API code.

### 2. Too many getters/setters

Blindly generating getters and setters for every field can expose too much internal state.

### 3. Poor API design

If the public API exposes implementation details, changing the internal implementation becomes harder.

### 4. Over-encapsulation

Making every tiny operation private/public without considering actual design needs can make code unnecessarily complicated.

---

# ⚠️ 24. Common Mistakes

## ❌ Mistake 1

> "Encapsulation means making variables private."

Incomplete.

Better:

> Encapsulation is about bundling state and behavior together while controlling access to internal state.

---

## ❌ Mistake 2

> "Every private field needs a getter and setter."

False.

Sometimes a field should have:

```text
getter only
setter only
neither
domain-specific methods
```

depending on the design.

---

## ❌ Mistake 3

> "Getter/setter automatically provides perfect encapsulation."

False.

This:

```java
public void setBalance(double balance) {
    this.balance = balance;
}
```

may expose too much control.

---

## ❌ Mistake 4

> "Encapsulation and abstraction are the same."

No.

```text
Encapsulation → controlled access
Abstraction   → essential interface / hide unnecessary detail
```

---

## ❌ Mistake 5

> "Private means completely inaccessible."

Not exactly.

A private member is inaccessible through normal direct access from unrelated classes, but Java provides mechanisms such as reflection that can interact with private members subject to runtime/module access rules.

For normal OOP design:

```text
private → class-level access only
```

---

## ❌ Mistake 6

> "Encapsulation means data cannot change."

No.

Encapsulated objects can be mutable.

```java
private int age;

public void setAge(int age) {
    this.age = age;
}
```

The state can still change.

---

# 💼 25. Interview Questions

## 🟢 Basic

### 1. What is encapsulation?

Encapsulation is the practice of keeping state and the behavior that manages it together while controlling access to the internal state.

---

### 2. How is encapsulation achieved in Java?

Commonly through:

```text
Classes
Access modifiers
Private fields
Controlled methods
```

---

### 3. What is data hiding?

Data hiding means restricting direct access to internal data or implementation details.

---

### 4. Why are fields usually private?

To prevent unrestricted external modification and allow the class to control access and maintain valid state.

---

### 5. What are getters and setters?

Methods conventionally used to read and modify object state.

---

### 6. Can encapsulation exist without getters and setters?

Yes.

For example:

```java
private double balance;

public void deposit(double amount) {
    // validation + state change
}
```

---

### 7. What are access modifiers in Java?

```text
private
default/package-private
protected
public
```

---

### 8. What is the purpose of `private`?

It restricts direct access to a member from outside the declaring top-level class.

---

### 9. What is the main benefit of encapsulation?

Controlled access to object state and the ability to maintain object invariants.

---

### 10. Is encapsulation the same as data hiding?

No.

Data hiding is closely related to encapsulation, but encapsulation is broader.

---

# 🟡 Intermediate

### 11. What is the difference between encapsulation and abstraction?

```text
Encapsulation → Controls access
Abstraction   → Exposes essentials and hides unnecessary details
```

---

### 12. Does encapsulation improve security?

It can improve software-level protection by preventing arbitrary direct access to state.

However, it should not be confused with complete application security.

---

### 13. Why should we avoid public fields?

Because callers can directly modify state and bypass validation or business rules.

---

### 14. Why shouldn't every field have a setter?

A setter may expose unrestricted mutation.

For example:

```java
setBalance()
```

could allow invalid or unauthorized state changes.

---

### 15. Can a class be immutable and encapsulated?

Yes.

Immutability is often implemented using encapsulation techniques.

---

### 16. Can an encapsulated class be mutable?

Yes.

For example:

```java
private int balance;

public void deposit(int amount) {
    balance += amount;
}
```

---

### 17. What is controlled access?

Allowing external code to interact with internal state through methods or APIs that enforce the class's rules.

---

### 18. How does encapsulation reduce coupling?

External code depends on the public API rather than the internal representation.

---

### 19. Can constructors help encapsulation?

Yes.

Constructors can establish valid initial state.

---

### 20. What is an invariant?

A condition that should remain true for a valid object.

Example:

```text
balance >= 0
```

---

# 🔴 Advanced / Tricky

### 21. Is encapsulation achieved only through access modifiers?

No.

Access modifiers are important tools, but good encapsulation also depends on API design, state management, immutability where appropriate, defensive copying, and hiding implementation details.

---

### 22. Is a getter always good encapsulation?

No.

A getter can expose internal mutable state.

For example:

```java
public List<String> getItems() {
    return items;
}
```

may allow callers to mutate internal state.

---

### 23. What is defensive copying?

Returning or storing a copy of mutable data so external code cannot directly modify the original internal object.

Example:

```java
public List<String> getItems() {
    return List.copyOf(items);
}
```

---

### 24. Can a setter violate encapsulation?

Yes.

Consider:

```java
public void setBalance(double balance) {
    this.balance = balance;
}
```

If negative values are invalid, this setter allows invalid state.

A domain-specific method may be better:

```java
public void deposit(double amount) {
    // validate
}
```

---

### 25. What is the difference between encapsulation and immutability?

```text
Encapsulation
→ controls access to state

Immutability
→ state cannot change after construction
```

---

### 26. Does private guarantee complete security?

No.

`private` is a language-level access-control mechanism, not a complete security boundary.

---

### 27. Can a private member be accessed indirectly?

Yes.

For example, public methods may expose or modify its state.

```java
private int age;

public int getAge() {
    return age;
}
```

This is controlled access.

---

### 28. Why is exposing internal collections dangerous?

Because callers can modify the collection directly.

Bad:

```java
return items;
```

Potentially safer:

```java
return List.copyOf(items);
```

---

### 29. What is the relationship between encapsulation and coupling?

Good encapsulation can reduce coupling because clients depend on a stable public API rather than internal implementation details.

---

### 30. Is encapsulation only about variables?

No.

It applies to the overall design of an object's state, behavior, and public interface.

---

# 🔥 26. Top 10 Interview Questions

## 1️⃣ What is encapsulation?

> Encapsulation is the practice of bundling state and behavior together while controlling access to internal state.

---

## 2️⃣ How is encapsulation implemented in Java?

Commonly through:

```text
Classes
private members
access modifiers
controlled methods
```

---

## 3️⃣ What is data hiding?

> Restricting direct access to internal data or implementation details.

---

## 4️⃣ Is encapsulation the same as data hiding?

No.

```text
Encapsulation → broader concept
Data hiding   → restricting direct access
```

---

## 5️⃣ Is encapsulation the same as getters and setters?

No.

Getters/setters are only one technique.

---

## 6️⃣ Why should fields be private?

To prevent unrestricted external modification and allow the class to control state changes.

---

## 7️⃣ Can encapsulation exist without setters?

Absolutely.

Example:

```java
private double balance;

public void deposit(double amount) {
    if (amount > 0) {
        balance += amount;
    }
}
```

---

## 8️⃣ Encapsulation vs abstraction?

```text
Encapsulation → Controls access to state/implementation
Abstraction   → Focuses on essential behavior/interface
```

---

## 9️⃣ Encapsulation vs immutability?

```text
Encapsulation → Controlled access
Immutability  → No state changes after construction
```

---

## 🔟 Why is encapsulation important?

It helps:

```text
Protect state
Enforce rules
Maintain invariants
Reduce coupling
Improve maintainability
Design better APIs
```

---

# 🧠 27. Quick Revision

```text
                 🔐 ENCAPSULATION
                        │
             ┌──────────┴──────────┐
             ▼                     ▼
        Bundle Together        Control Access
             │                     │
       State + Behavior       private / API
             │                     │
             └──────────┬──────────┘
                        ▼
                 Valid Object State
```

### Remember:

```text
private field
      ↓
not directly accessible
      ↓
controlled method
      ↓
validation / business rules
      ↓
state changes
```

### Four important concepts

```text
Encapsulation → Controls access

Data Hiding   → Restricts visibility

Abstraction   → Hides unnecessary details

Immutability  → Prevents state changes
```

They are related, but **not interchangeable**.

---

# ⚡ 28. 30-Second Interview Answer

> **"Encapsulation is an OOP principle where an object's state and the behavior that manages that state are kept together, while access to the internal state is controlled. In Java, we commonly achieve it using classes, private fields, access modifiers, and public methods. For example, instead of exposing a bank account's balance directly, we can keep it private and provide methods such as `deposit()`, `withdraw()`, and `getBalance()`. This allows the class to validate changes and maintain its invariants. Encapsulation is broader than simply using getters and setters, and it is different from abstraction and immutability."**

---

# 🏆 29. Memory Trick

Remember:

```text
🔐 ENCAPSULATION

DATA
  ↓
Private
  ↓
CONTROLLED METHOD
  ↓
VALIDATION
  ↓
SAFE STATE
```

### The best mental model:

> **"Don't let outside code directly control my state; make it ask me through my API."**

Example:

```text
❌ account.balance = -5000

✅ account.withdraw(5000)
```

The object itself decides whether the operation is valid.

---

# 🎯 Final Takeaway

The biggest misconception to remove from your mind is:

```text
Encapsulation
≠
private + getter + setter
```

Instead remember:

```text
              ENCAPSULATION
                    │
       ┌────────────┴────────────┐
       ▼                         ▼
  Bundle State              Control Access
       │                         │
       ▼                         ▼
  Fields + Methods         Public API
                                 │
                                 ▼
                         Validation / Rules
                                 │
                                 ▼
                          Valid Object State
```

A well-encapsulated object **owns responsibility for managing its own state**.

> 🔥 **Best design mindset:**
> **Don't expose data just because you can. Expose behavior that makes sense for the object.**
