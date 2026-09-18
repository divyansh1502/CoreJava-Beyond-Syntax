# ☕ 12 — Packages & Access Modifiers

> Packages organize Java classes, while access modifiers control where classes, methods, variables, and constructors can be accessed.

---

# 1. What Is a Package?

A **package** is a namespace used to organize related Java classes and interfaces.

Example:

```java
package com.example.bank;
```

Now classes can belong to:

```text
com.example.bank
```

Think of a package like a **folder/namespace for Java classes**.

---

# 2. Why Do We Need Packages?

Packages provide:

* Organization
* Namespace management
* Access control
* Avoiding class-name conflicts
* Better project structure

Example:

```text
com.company
│
├── model
│   ├── Employee.java
│   └── Department.java
│
├── service
│   └── EmployeeService.java
│
├── controller
│   └── EmployeeController.java
│
└── repository
    └── EmployeeRepository.java
```

This type of structure is very common in Spring Boot applications.

---

# 3. Declaring a Package

The `package` statement normally appears at the top of the Java source file.

```java
package com.example.model;

public class Employee {

}
```

The package declaration tells Java:

```text
Employee
↓
belongs to
↓
com.example.model
```

---

# 4. Package Naming Convention

Java packages are generally written in lowercase.

Good:

```java
package com.company.project;
```

Avoid:

```java
package Com.Company.Project;
```

Common convention:

```text
reverse-domain-name
+
project/module
```

Example:

```text
com.google
org.springframework
com.mycompany.hrms
```

---

# 5. `import`

If a class belongs to another package, you can import it.

Suppose:

```java
package com.example.model;

public class Employee {
}
```

Another class:

```java
package com.example.service;

import com.example.model.Employee;

public class EmployeeService {

    Employee e = new Employee();

}
```

`import` allows you to refer to the class by its simple name.

---

# 6. Fully Qualified Class Name

Instead of importing:

```java
import com.example.model.Employee;
```

you can write:

```java
com.example.model.Employee e =
        new com.example.model.Employee();
```

This is called using the **fully qualified class name (FQCN)**.

It contains:

```text
package + class name
```

Example:

```text
com.example.model.Employee
```

---

# 7. `java.lang` Package

Java automatically imports `java.lang` into every Java source file.

Therefore you can directly use:

```java
String
System
Math
Integer
Object
```

without:

```java
import java.lang.String;
```

For example:

```java
String name = "Divyansh";
```

works automatically.

---

# 8. Wildcard Import

You can import all accessible types from a package using `*`.

```java
import java.util.*;
```

Now classes such as:

```java
ArrayList
LinkedList
HashMap
Scanner
```

can be referenced by their simple names.

Important:

```java
import java.util.*;
```

does **not** mean "import every subpackage."

For example:

```text
java.util
java.util.concurrent
```

are different packages.

---

# 9. Static Import

Java also supports static imports.

Example:

```java
import static java.lang.Math.*;

public class Test {

    public static void main(String[] args) {

        System.out.println(sqrt(25));

    }
}
```

Normally:

```java
Math.sqrt(25);
```

With static import:

```java
sqrt(25);
```

Static import is for **static members**, not ordinary instance members.

---

# 10. What Are Access Modifiers?

Access modifiers control the visibility/accessibility of classes and class members.

Java has four access levels:

```text
public
protected
default
private
```

Important:

> `default` is not written using the keyword `default` for ordinary member access. It means no access modifier is specified.

Example:

```java
class Employee {

    int salary;
}
```

Here `salary` has **package-private/default access**.

---

# 11. `public`

`public` provides the widest accessibility.

Example:

```java
public class Employee {

    public int salary;

    public void display() {
        System.out.println(salary);
    }
}
```

A public member can generally be accessed from classes in other packages, subject to normal type/access rules.

---

# 12. `private`

`private` is the most restrictive member-level access modifier.

```java
class Employee {

    private int salary;

}
```

The field can be accessed directly only inside the same class.

```java
class Employee {

    private int salary;

    void setSalary(int salary) {
        this.salary = salary;
    }

    int getSalary() {
        return salary;
    }
}
```

This is the foundation of **encapsulation**.

---

# 13. `protected`

`protected` provides access:

1. Within the same package
2. In subclasses in other packages, subject to Java's protected-access rules

Example:

```java
class Employee {

    protected int salary;

}
```

A subclass can access the protected member.

---

# 14. Default / Package-Private

If you don't specify an access modifier:

```java
class Employee {

    int salary;

}
```

then `salary` has **package-private/default access**.

It can be accessed from classes in the **same package**.

It is not directly accessible from an unrelated class in another package.

---

# 15. The Most Important Access Table

| Modifier    | Same Class | Same Package | Subclass Different Package | Different Package |
| ----------- | ---------: | -----------: | -------------------------: | ----------------: |
| `private`   |          ✅ |            ❌ |                          ❌ |                 ❌ |
| default     |          ✅ |            ✅ |                          ❌ |                 ❌ |
| `protected` |          ✅ |            ✅ |                         ✅* |                ❌* |
| `public`    |          ✅ |            ✅ |                          ✅ |                 ✅ |

`*` Protected access from another package has an important restriction: it is available through inheritance, not as unrestricted access through any object/reference.

---

# 16. Easy Memory Trick

Remember:

```text
private
   ↓
default
   ↓
protected
   ↓
public
```

Accessibility generally increases as you move downward.

Think:

```text
PRIVATE
↓
MY CLASS

DEFAULT
↓
MY PACKAGE

PROTECTED
↓
MY PACKAGE + SUBCLASS

PUBLIC
↓
EVERYWHERE
```

---

# 17. `private` Example

```java
class Employee {

    private int salary = 50000;

    void display() {
        System.out.println(salary);
    }
}
```

This works:

```java
Employee e = new Employee();
e.display();
```

But:

```java
System.out.println(e.salary);
```

❌ Compile-time error.

Because `salary` is private.

---

# 18. Default Example

Package:

```text
com.example.model
```

```java
class Employee {

    int salary = 50000;

}
```

Another class in the same package:

```java
package com.example.model;

class Test {

    public static void main(String[] args) {

        Employee e = new Employee();

        System.out.println(e.salary);

    }
}
```

✅ Works.

---

# 19. Default Across Packages

Suppose:

```text
com.example.model
    Employee

com.example.service
    Test
```

If:

```java
class Employee {

    int salary = 50000;

}
```

Then `Test` cannot directly access `salary`.

❌ Because default/package-private members are accessible only within the same package.

---

# 20. Protected Example — Same Package

```java
class Employee {

    protected int salary = 50000;

}
```

Another class in the same package:

```java
Employee e = new Employee();

System.out.println(e.salary);
```

✅ Works.

Protected provides package-level access as well.

---

# 21. Protected Example — Different Package

Package A:

```java
package pack1;

public class Employee {

    protected int salary = 50000;

}
```

Package B:

```java
package pack2;

import pack1.Employee;

public class Manager extends Employee {

    void display() {

        System.out.println(salary);

    }
}
```

✅ Works because `Manager` is a subclass.

---

# 22. Protected Interview Trap

This is important.

Suppose:

```java
package pack2;

import pack1.Employee;

public class Manager extends Employee {

    void display(Employee e) {

        System.out.println(e.salary);

    }
}
```

This is **not generally allowed** merely because `Manager` extends `Employee`.

Across packages, protected access is tied to the subclass context and must be accessed through the subclass type/current subclass context, rather than arbitrary superclass references.

A safe example is:

```java
System.out.println(this.salary);
```

or:

```java
System.out.println(salary);
```

inside the subclass.

---

# 23. Public Example

```java
package pack1;

public class Employee {

    public int salary = 50000;

}
```

From another package:

```java
Employee e = new Employee();

System.out.println(e.salary);
```

✅ Allowed, assuming the class itself is accessible.

---

# 24. Important: Class Access vs Member Access

Consider:

```java
class Employee {

    public int salary;

}
```

Even though `salary` is public, the class `Employee` itself is package-private.

Therefore another package cannot access `Employee` simply because its field is public.

Both levels matter:

```text
Class accessibility
        ↓
Member accessibility
```

---

# 25. Top-Level Classes

A top-level class can generally have only:

```text
public
```

or:

```text
package-private
```

access.

You cannot declare a top-level class as:

```java
private class Employee {
}
```

❌ Invalid.

Nor:

```java
protected class Employee {
}
```

❌ Invalid.

---

# 26. Public Top-Level Class Rule

A public top-level class normally must be declared in a source file whose name matches the class name.

Example:

```java
public class Employee {
}
```

File:

```text
Employee.java
```

---

# 27. Can One `.java` File Have Multiple Classes?

Yes.

Example:

```java
class Employee {
}

class Manager {
}
```

Multiple top-level classes can exist in one source file.

But only one top-level class can be `public`, and its name must match the file name.

Example:

```java
public class Employee {
}

class Manager {
}
```

File:

```text
Employee.java
```

✅ Valid.

---

# 28. Access Modifiers on Members

The following can use access modifiers:

```text
fields
methods
constructors
nested classes
```

Example:

```java
public class Employee {

    private int salary;

    protected void calculate() {
    }

    public void display() {
    }

}
```

---

# 29. Constructors and Access Modifiers

Constructors can also have access modifiers.

```java
public Employee() {
}
```

```java
private Employee() {
}
```

```java
protected Employee() {
}
```

```java
Employee() {
}
```

Constructor accessibility controls who can create objects using that constructor.

---

# 30. Private Constructor

Example:

```java
class Singleton {

    private Singleton() {
    }
}
```

Now:

```java
Singleton s = new Singleton();
```

❌ Cannot be done from outside the class.

Private constructors are commonly used in patterns such as Singleton and utility-class designs, although modern application design often uses dependency injection instead.

---

# 31. Package + Access Modifier Relationship

Packages and access modifiers work together.

Example:

```text
Package A
│
├── Employee
└── Manager

Package B
│
└── Test
```

Access depends on:

```text
same class?
same package?
subclass?
different package?
```

This is why package knowledge becomes important before deeper OOP.

---

# 32. Package Is Not the Same as Folder

A package usually corresponds to a directory structure in source/class output, but conceptually a package is a **Java namespace**.

For example:

```java
package com.example.model;
```

typically corresponds to:

```text
com/example/model/
```

The package identity is determined by Java's package/class naming and compilation/runtime structure, not merely by a folder name.

---

# 33. Package Compilation Concept

Suppose:

```java
package com.example;

public class Employee {
}
```

Compile with an output directory:

```bash
javac -d . Employee.java
```

You may get:

```text
com/
└── example/
    └── Employee.class
```

The `-d` option specifies where generated class files should be placed.

---

# 34. Import Does NOT Copy a Class

Important interview point.

When you write:

```java
import java.util.ArrayList;
```

Java does not copy `ArrayList` into your project.

It simply allows you to use:

```java
ArrayList
```

instead of:

```java
java.util.ArrayList
```

---

# 35. Import vs Package

```text
package
→ tells where your class belongs

import
→ tells which external type you want to refer to by simple name
```

Example:

```java
package com.example.service;

import com.example.model.Employee;
```

---

# 36. Package vs Access Modifier

| Concept     | Purpose                      |
| ----------- | ---------------------------- |
| Package     | Organizes classes/namespaces |
| `private`   | Restricts member to class    |
| default     | Restricts to package         |
| `protected` | Package + subclass access    |
| `public`    | Broad access                 |

---

# 37. Interview Trap — `default` Keyword

Don't confuse:

```java
default
```

with:

```text
default/package-private access
```

For a class/member:

```java
class Employee {
    int salary;
}
```

`salary` is package-private.

You don't write:

```java
default int salary;
```

That would be invalid for ordinary field/method access.

The `default` keyword has other uses, such as default methods in interfaces and switch constructs.

---

# 38. Interview Trap — `protected`

Don't memorize protected as simply:

> "Accessible everywhere in subclass."

More accurately:

```text
same package
+
subclass access across packages under protected-access rules
```

This distinction is frequently tested.

---

# 39. Interview Trap — Private and Inheritance

A subclass does not directly inherit/access a superclass's private members as accessible members.

Example:

```java
class Parent {

    private int x = 10;

}

class Child extends Parent {

    void display() {

        System.out.println(x);

    }
}
```

❌ Compile-time error.

The child cannot directly access `x`.

The parent can expose controlled access using:

```java
protected
```

or:

```java
public getter
```

depending on the design.

---

# 40. Encapsulation Connection

Access modifiers are heavily connected to encapsulation.

Instead of:

```java
class BankAccount {

    public double balance;

}
```

we generally prefer:

```java
class BankAccount {

    private double balance;

    public double getBalance() {
        return balance;
    }

    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
        }
    }
}
```

Now external code cannot freely modify the balance.

This is **encapsulation**.

---

# 41. Why `private` Is Commonly Used

Fields are often declared:

```java
private
```

and exposed through:

```java
public/protected methods
```

because this provides control over how data is accessed or changed.

Example:

```java
private double salary;

public double getSalary() {
    return salary;
}

public void setSalary(double salary) {
    if (salary >= 0) {
        this.salary = salary;
    }
}
```

---

# 42. Access Modifier Cheat Sheet

```text
┌─────────────┬──────────────┐
│ Modifier    │ Accessibility│
├─────────────┼──────────────┤
│ private     │ Same class   │
│ default     │ Same package │
│ protected   │ Package +    │
│             │ subclasses   │
│ public      │ Broadest     │
└─────────────┴──────────────┘
```

---

# 43. Interview Questions

## Basic

### Q1. What is a package?

A namespace used to organize related Java types and avoid naming conflicts.

### Q2. Why do we use packages?

For organization, namespace management, access control, and maintainability.

### Q3. What is `import`?

It allows a type from another package to be referred to by its simple name.

### Q4. What is a fully qualified class name?

Package name + class name.

Example:

```text
java.util.ArrayList
```

### Q5. What package is automatically imported?

```text
java.lang
```

---

# 44. Access Modifier Questions

### Q6. What are the four access levels in Java?

```text
private
default/package-private
protected
public
```

### Q7. Which is the most restrictive?

```text
private
```

### Q8. Which is the most accessible?

```text
public
```

### Q9. What is default access?

When no access modifier is specified, the member/type has package-private access.

### Q10. What is protected access?

Accessible throughout the same package and available to subclasses in other packages subject to protected-access rules.

---

# 45. Advanced Interview Questions

### Q11. Can a top-level class be private?

No.

### Q12. Can a top-level class be protected?

No.

### Q13. What access modifiers can a top-level class have?

Generally:

```text
public
package-private
```

### Q14. Can constructors be private?

Yes.

### Q15. Can methods be private?

Yes.

### Q16. Can fields be protected?

Yes.

### Q17. Can interfaces have private methods?

Yes, modern Java supports private methods in interfaces for internal code reuse.

### Q18. Can an abstract class have a private constructor?

Yes.

But the constructor cannot be used directly by subclasses as an accessible superclass constructor.

---

# 46. Output-Based Questions

## Q1

```java
class Test {

    private int x = 10;

}

public class Main {

    public static void main(String[] args) {

        Test t = new Test();

        System.out.println(t.x);

    }
}
```

Result:

```text
Compile-time error
```

because `x` is private.

---

## Q2

```java
class Test {

    int x = 10;

}

class Main {

    public static void main(String[] args) {

        Test t = new Test();

        System.out.println(t.x);

    }
}
```

If both classes are in the same package:

```text
10
```

because `x` has package-private access.

---

## Q3

```java
class Test {

    protected int x = 10;

}
```

A class in the same package can access:

```java
Test t = new Test();

System.out.println(t.x);
```

✅ Allowed.

---

## Q4

```java
public class Test {

    public int x = 10;

}
```

A class in another package can access:

```java
Test t = new Test();

System.out.println(t.x);
```

✅ Allowed, assuming the class is accessible.

---

# 🔥 TOP 10 VVVV IMPORTANT

### 1. What are Java's access modifiers?

```text
private
default/package-private
protected
public
```

---

### 2. Difference between all four?

```text
private
→ same class

default
→ same package

protected
→ same package + subclass access across packages

public
→ broadest access
```

---

### 3. What is a package?

A namespace used to organize related Java types and control naming/access boundaries.

---

### 4. Why do we use packages?

```text
organization
namespace
access control
avoid naming conflicts
maintainability
```

---

### 5. What is the difference between package and import?

```text
package
→ defines where your class belongs

import
→ lets you use another package's type by simple name
```

---

### 6. Can a top-level class be private/protected?

No.

A top-level class can generally be:

```text
public
or
package-private
```

---

### 7. What is default access?

When no access modifier is specified.

```java
class Employee {

    int salary;

}
```

`salary` is accessible within the same package.

---

### 8. Explain protected access.

`protected` members are accessible within the same package and can also be accessed by subclasses in other packages, subject to Java's protected-access rules.

---

### 9. Why are fields commonly private?

To achieve encapsulation and control how the object's state is accessed or modified.

---

### 10. Can a constructor be private?

Yes.

It prevents unrestricted object creation from outside the class and can be useful for patterns/designs such as Singleton or utility classes.

---

# 🎤 30-SECOND INTERVIEW ANSWER

> A package in Java is a namespace used to organize related classes and interfaces, avoid naming conflicts, and provide an access boundary. Java provides four access levels: private, package-private or default, protected, and public. Private members are accessible only inside the same class, default members within the same package, protected members within the package and through subclass access across packages subject to protected rules, and public members have the broadest accessibility. Access modifiers are especially important for encapsulation because we usually keep fields private and expose controlled behavior through methods.

---

# ⚡ QUICK REVISION

```text
PACKAGE
↓
organizes classes
↓
namespace
↓
access boundary
```

```text
private
↓
same class

default
↓
same package

protected
↓
same package
+
subclass access

public
↓
broadest
```

### Memory Trick

```text
PRIVATE  → ME
DEFAULT  → MY PACKAGE
PROTECTED → MY PACKAGE + CHILD
PUBLIC   → EVERYONE
```

---

# 🚀 NEXT

```text
01 → Java Introduction
02 → JVM / JRE / JDK
03 → Java Features / Architecture
04 → Variables, Data Types, Operators, Methods
05 → Exception Handling
06 → ...
...
11 → Wrapper Classes
12 → Packages & Access Modifiers ✓

13 → Strings
```

> **13 will cover `String`, String Pool, immutability, `==` vs `equals()`, `StringBuilder`, `StringBuffer`, important methods, memory behavior, and interview traps.**
