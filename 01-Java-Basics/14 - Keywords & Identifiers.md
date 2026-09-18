# ☕ 14 — Keywords & Identifiers

> Java has reserved words with predefined meanings, while identifiers are names created by programmers for classes, methods, variables, packages, and other program elements.

---

# 1. What Is a Keyword?

A **keyword** is a reserved word in Java that has a predefined meaning to the compiler.

Example:

```java
public class Employee {

    private int salary;

}
```

Here:

```text
public
class
private
int
```

are keywords.

You cannot use Java keywords as normal identifiers.

---

# 2. Why Do Keywords Exist?

Keywords tell the compiler what a particular piece of code means.

For example:

```java
class Employee {
}
```

`class` tells Java that we are declaring a class.

Similarly:

```java
if (age >= 18) {
}
```

`if` tells Java that a conditional statement is being used.

---

# 3. Java Keywords

The commonly recognized Java keyword set includes:

```text
abstract
assert
boolean
break
byte
case
catch
char
class
const
continue
default
do
double
else
enum
extends
final
finally
float
for
goto
if
implements
import
instanceof
int
interface
long
native
new
package
private
protected
public
return
short
static
strictfp
super
switch
synchronized
this
throw
throws
transient
try
void
volatile
while
```

Modern Java also has:

```text
record
sealed
permits
non-sealed
yield
var
```

Important nuance:

Some modern Java terms such as `var` and `yield` are **contextual keywords**, meaning their restrictions differ from traditional reserved keywords.

---

# 4. Keyword vs Contextual Keyword

Traditional keyword:

```java
class
```

has a reserved language meaning.

Contextual keywords are recognized as special words only in particular contexts.

Examples include:

```text
var
yield
record
sealed
permits
non-sealed
```

Interview point:

> Don't blindly assume every special Java language word has exactly the same lexical status.

---

# 5. `true`, `false`, and `null`

A common interview trap:

```text
true
false
null
```

are **not technically keywords** in Java.

They are **literals**.

Example:

```java
boolean active = true;

String name = null;
```

So:

```text
true  → boolean literal
false → boolean literal
null  → null literal
```

---

# 6. What Is an Identifier?

An **identifier** is a name given by the programmer to a program element.

Examples:

```java
class Employee {

    int salary;

    void displaySalary() {
    }
}
```

Identifiers include:

```text
Employee
salary
displaySalary
```

---

# 7. Where Are Identifiers Used?

Identifiers can name:

* Classes
* Interfaces
* Methods
* Variables
* Fields
* Parameters
* Packages
* Enum constants
* Records and other declarations

Example:

```java
class Employee {

    int salary;

    void calculateSalary(int bonus) {

        int total = salary + bonus;

    }
}
```

Identifiers include:

```text
Employee
salary
calculateSalary
bonus
total
```

---

# 8. Rules for Identifiers

An identifier:

### Rule 1 — Cannot be a keyword

Invalid:

```java
int class = 10;
```

❌ `class` is a keyword.

---

### Rule 2 — Cannot start with a digit

Invalid:

```java
int 123age = 20;
```

❌

Valid:

```java
int age123 = 20;
```

✅

---

### Rule 3 — Can contain letters

```java
int age = 20;
```

✅

---

### Rule 4 — Can contain digits

```java
int age20 = 20;
```

✅

But the first character cannot be a digit.

---

### Rule 5 — Underscore is allowed

```java
int employee_salary = 50000;
```

✅

---

### Rule 6 — `$` is technically allowed

```java
int $salary = 50000;
```

This is legal Java syntax.

However, `$` is generally avoided in normal application naming because it is commonly used in generated/compiler-related names.

---

### Rule 7 — Spaces are not allowed

Invalid:

```java
int employee salary = 50000;
```

❌

Use:

```java
int employeeSalary = 50000;
```

---

# 9. Java Is Case-Sensitive

These are different identifiers:

```java
int age = 20;
int Age = 30;
int AGE = 40;
```

All three can technically coexist because:

```text
age
Age
AGE
```

are different identifiers.

---

# 10. Identifier Naming Convention

Java naming conventions are not the same as identifier syntax rules.

### Class

Use PascalCase:

```java
EmployeeManagementSystem
BankAccount
Student
```

### Method

Use camelCase:

```java
calculateSalary()
getBalance()
displayDetails()
```

### Variable

Use camelCase:

```java
employeeName
totalSalary
accountNumber
```

### Constant

Use UPPER_SNAKE_CASE:

```java
MAX_VALUE
DEFAULT_TIMEOUT
PI
```

### Package

Use lowercase:

```text
com.example.project
```

---

# 11. Rules vs Conventions

This distinction is very important.

### Rule

Must be followed by the compiler.

Example:

```java
int 123age;
```

❌ Invalid.

### Convention

Recommended style.

Example:

```java
int EmployeeAge;
```

This is syntactically valid, but:

```java
int employeeAge;
```

is the conventional style for a variable.

---

# 12. Valid Identifiers

All of these are syntactically valid:

```java
int age;
int age2;
int _age;
int $age;
int employeeSalary;
int employee_salary;
```

---

# 13. Invalid Identifiers

These are invalid:

```java
int 2age;
int employee salary;
int class;
int public;
int my-name;
```

Reasons:

```text
2age
→ starts with digit

employee salary
→ contains space

class
→ keyword

public
→ keyword

my-name
→ '-' is not allowed in an identifier
```

---

# 14. Unicode Identifiers

Java identifiers are not restricted to basic English letters.

Unicode characters can be used in identifiers when permitted by Java's identifier rules.

For example:

```java
int café = 10;
```

can be legal.

However, conventional Java code generally uses readable English-based identifiers for maintainability.

---

# 15. `_` as an Identifier

Modern Java has an important special case.

You cannot use `_` alone as an identifier.

For example:

```java
int _ = 10;
```

❌ Invalid in modern Java.

This changed with Java 9.

However:

```java
int _value = 10;
```

can be valid because the identifier is not exactly `_`.

---

# 16. Keyword `var`

Modern Java supports local variable type inference:

```java
var name = "Java";
var age = 21;
```

Here:

```text
var
```

is a contextual keyword.

The compiler determines the variable's type from its initializer.

Example:

```java
var name = "Java";
```

is inferred as:

```text
String
```

---

# 17. `var` Is Not Dynamic Typing

This is a common interview trap.

```java
var age = 21;
```

does NOT mean:

```text
age can become any type
```

The compiler infers:

```text
age → int
```

So:

```java
age = "Java";
```

❌ Compile-time error.

Java remains statically typed.

---

# 18. `var` Requires Initialization

Invalid:

```java
var age;
```

❌

The compiler cannot infer the type.

Valid:

```java
var age = 21;
```

✅

---

# 19. `var` Is Limited

`var` can be used for local variables, including appropriate local contexts such as:

```java
var x = 10;
```

It cannot be used as a normal field declaration:

```java
class Test {

    var x = 10;   // ❌

}
```

Nor as a method parameter:

```java
void test(var x) {
}
```

❌

---

# 20. `final` Is a Keyword

Example:

```java
final int MAX = 100;
```

`final` tells Java that the variable cannot be reassigned after initialization.

```java
MAX = 200;
```

❌

Other uses of `final` include:

```text
final variable
final method
final class
```

---

# 21. `static` Is a Keyword

Example:

```java
static int count;
```

It means the member belongs to the class rather than to each individual object.

You will study `static` much more deeply in OOP.

---

# 22. `this` Is a Keyword

Example:

```java
class Employee {

    int salary;

    Employee(int salary) {

        this.salary = salary;

    }
}
```

`this` refers to the current object.

---

# 23. `super` Is a Keyword

Used to refer to superclass members or constructors.

Example:

```java
class Child extends Parent {

    Child() {
        super();
    }
}
```

You will study this deeply in inheritance.

---

# 24. `new` Is a Keyword

Used to create objects and arrays.

Example:

```java
Employee e = new Employee();
```

And:

```java
int[] arr = new int[5];
```

---

# 25. `instanceof` Is a Keyword

Used to test whether an object reference is compatible with a specified type.

Example:

```java
if (obj instanceof String) {
    System.out.println("String");
}
```

Modern Java also supports pattern matching with `instanceof`.

Example:

```java
if (obj instanceof String s) {
    System.out.println(s.length());
}
```

---

# 26. `extends` vs `implements`

Both are keywords.

```java
class Dog extends Animal {
}
```

`extends` is used for class inheritance.

```java
class Dog implements Runnable {
}
```

`implements` is used when a class implements an interface.

For interfaces:

```java
interface B extends A {
}
```

An interface can extend another interface.

---

# 27. `throw` vs `throws`

Both are keywords but serve different purposes.

### `throw`

Actually throws an exception:

```java
throw new RuntimeException();
```

### `throws`

Declares exceptions that a method may propagate:

```java
void read() throws IOException {
}
```

---

# 28. `try`, `catch`, `finally`

These are exception-handling keywords.

```java
try {

}
catch (Exception e) {

}
finally {

}
```

You have already covered exception handling in the roadmap.

---

# 29. `break` vs `continue`

### `break`

Terminates the applicable loop/switch.

```java
break;
```

### `continue`

Skips the current loop iteration and proceeds to the next iteration.

```java
continue;
```

---

# 30. `return`

Returns control from a method and may return a value.

```java
return salary;
```

For a `void` method:

```java
return;
```

---

# 31. Keywords vs Identifiers

| Keyword                                  | Identifier                   |
| ---------------------------------------- | ---------------------------- |
| Reserved by Java                         | Created by programmer        |
| Has predefined meaning                   | Names program elements       |
| Cannot normally be used as an identifier | Must follow identifier rules |
| Example: `class`                         | Example: `Employee`          |

Example:

```java
public class Employee {

    int salary;

}
```

Here:

```text
public → keyword
class  → keyword
Employee → identifier
salary → identifier
```

---

# 32. Keywords vs Literals

This is another common interview question.

### Keywords

```text
class
public
static
int
if
return
```

### Literals

```java
10
3.14
'A'
"Java"
true
false
null
```

A literal represents a value directly in source code.

---

# 33. Identifier vs Literal

Example:

```java
int age = 21;
```

Here:

```text
age
↓
identifier

21
↓
integer literal
```

---

# 34. Can a Keyword Be Used Inside an Identifier?

Yes.

For example:

```java
int className = 10;
```

`className` is valid.

Why?

Because the entire identifier is:

```text
className
```

not:

```text
class
```

The keyword merely appears as part of the name.

---

# 35. Can a Keyword Be Used as an Identifier?

No.

```java
int class = 10;
```

❌

```java
int public = 20;
```

❌

---

# 36. Interview Trick — `main`

Is `main` a keyword?

No.

```java
public static void main(String[] args)
```

Here:

```text
main
```

is a method name.

It has special significance as the standard Java application entry point, but it is **not a Java keyword**.

---

# 37. Interview Trick — `String`

Is `String` a keyword?

No.

```java
String name;
```

`String` is a class from:

```text
java.lang
```

Similarly:

```text
System
Object
Integer
```

are not keywords.

---

# 38. Interview Trick — `Integer`

Is `Integer` a keyword?

No.

It is a class:

```text
java.lang.Integer
```

But:

```text
int
```

is a keyword representing a primitive type.

Therefore:

```text
int
→ keyword

Integer
→ class
```

---

# 39. Interview Trick — `null`

Is `null` a keyword?

Technically:

```text
No.
```

It is a null literal.

Similarly:

```text
true
false
```

are boolean literals.

---

# 40. Interview Trick — `const` and `goto`

Java reserves:

```text
const
goto
```

but does not use them as active Java language constructs.

Therefore they cannot be used as normal identifiers.

Example:

```java
int goto = 10;
```

❌

---

# 41. Keyword Categories

You can mentally group keywords.

### Access

```text
public
private
protected
```

### OOP

```text
class
interface
extends
implements
new
this
super
```

### Control Flow

```text
if
else
switch
case
default
for
while
do
break
continue
return
```

### Exception Handling

```text
try
catch
finally
throw
throws
```

### Modifiers

```text
static
final
abstract
synchronized
volatile
transient
native
strictfp
```

### Primitive Types

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

### Other

```text
package
import
instanceof
enum
assert
void
```

---

# 42. Common Naming Conventions

### Class

```java
BankAccount
EmployeeManagementSystem
CustomerService
```

### Method

```java
calculateTotal()
getBalance()
openAccount()
```

### Variable

```java
customerName
totalAmount
accountNumber
```

### Constant

```java
MAX_RETRY
DEFAULT_TIMEOUT
MIN_BALANCE
```

### Package

```text
com.example.bank
com.company.project
```

---

# 43. Naming Convention in Your Projects

For example, in your Bank Management System:

```java
class Customer {

    private String accountNumber;

    private double balance;

    public void depositMoney(double amount) {
    }
}
```

Good naming:

```text
Customer
accountNumber
balance
depositMoney
```

This makes the code easier to read and maintain.

---

# 44. Interview Questions

### Q1. What is a keyword?

A reserved word with a predefined meaning in Java.

### Q2. What is an identifier?

A programmer-defined name for a program element.

### Q3. Can keywords be used as identifiers?

No.

### Q4. Is Java case-sensitive?

Yes.

### Q5. Can an identifier start with a digit?

No.

### Q6. Can an identifier contain digits?

Yes, but not as its first character.

### Q7. Can `$` be used in an identifier?

Yes, technically.

### Q8. Can `_` be used in an identifier?

Yes, but `_` alone is not permitted as an identifier in modern Java.

### Q9. Is `String` a keyword?

No. It is a class.

### Q10. Is `main` a keyword?

No. It is a method name.

---

# 45. More Interview Questions

### Q11. Is `null` a keyword?

No. It is a null literal.

### Q12. Are `true` and `false` keywords?

No. They are boolean literals.

### Q13. Is `int` a keyword?

Yes.

### Q14. Is `Integer` a keyword?

No. It is a wrapper class.

### Q15. Can a class name contain `$`?

Yes, technically.

### Q16. Can a variable name contain `_`?

Yes.

### Q17. Can an identifier contain spaces?

No.

### Q18. Is naming convention enforced by the compiler?

No. Naming conventions are style guidelines.

### Q19. Can `var` be used as a field?

No.

### Q20. Is Java statically typed even when using `var`?

Yes. The compiler determines the type at compile time.

---

# 🔥 TOP 10 VVVVV IMPORTANT

## 1. What is a keyword?

A reserved Java language word with a predefined meaning.

---

## 2. What is an identifier?

A programmer-defined name used for classes, methods, variables, fields, parameters, packages, etc.

---

## 3. Can a keyword be used as an identifier?

No.

```java
int class = 10; // ❌
```

---

## 4. Can an identifier start with a number?

No.

```java
int 123age; // ❌
```

But:

```java
int age123; // ✅
```

---

## 5. Is `String` a keyword?

No.

```text
String → java.lang.String class
```

But:

```text
int → keyword
```

---

## 6. Is `main` a keyword?

No.

`main` is a method name used as the standard application entry point.

---

## 7. Is `null` a keyword?

No.

It is a null literal.

Similarly:

```text
true
false
```

are boolean literals.

---

## 8. What is the difference between `int` and `Integer`?

```text
int
→ primitive type
→ Java keyword

Integer
→ wrapper class
→ java.lang.Integer
```

---

## 9. What is `var`?

`var` enables local variable type inference.

```java
var age = 21;
```

The compiler infers:

```text
age → int
```

It does not make Java dynamically typed.

---

## 10. Difference between rules and naming conventions?

Rules are enforced by the compiler.

Example:

```java
int 123age; // ❌
```

Naming conventions are recommended coding practices.

Example:

```java
int EmployeeAge; // valid, but non-standard variable naming
```

---

# 🎤 30-SECOND INTERVIEW ANSWER

> Keywords are reserved words in Java that have predefined meanings, such as `class`, `public`, `static`, and `if`. They cannot normally be used as identifiers. Identifiers are programmer-defined names for classes, methods, variables, fields, and other program elements. Identifiers cannot start with digits, cannot contain spaces, and are case-sensitive. Java also has naming conventions such as PascalCase for classes, camelCase for methods and variables, and UPPER_SNAKE_CASE for constants. A useful interview distinction is that `String` and `main` are not keywords, while `int` is a keyword; `true`, `false`, and `null` are literals.

---

# ⚡ QUICK REVISION

```text
KEYWORD
↓
Reserved by Java
↓
Predefined meaning
↓
class, public, static, int
```

```text
IDENTIFIER
↓
Programmer-defined name
↓
Employee
salary
calculateTotal()
```

### Identifier Rules

```text
❌ starts with digit
❌ spaces
❌ Java keyword

✅ letters
✅ digits (not first)
✅ _
✅ $
```

### Important Traps

```text
String
→ class, NOT keyword

Integer
→ class, NOT keyword

main
→ method name, NOT keyword

null
→ null literal, NOT keyword

true / false
→ boolean literals, NOT keywords

int
→ keyword
```

### Naming Convention

```text
Class       → PascalCase
Method      → camelCase
Variable    → camelCase
Constant    → UPPER_SNAKE_CASE
Package     → lowercase
```

---

# 🚀 NEXT

```text
13 → Command-Line Arguments ✓
14 → Keywords & Identifiers ✓

15 → OOP Introduction
```

> **15 is where the major Java section begins: OOP. We'll build it properly from the problem Java was designed to solve, then move through Class/Object → Encapsulation → Inheritance → Polymorphism → Abstraction → Interfaces, with interview questions and practical examples.**
