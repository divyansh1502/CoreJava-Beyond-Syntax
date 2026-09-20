# throw and throws in Java

## 1. Introduction

Java provides two important keywords for working with exceptions:

```text
throw
throws
```

They look similar, but their purposes are completely different.

```text
throw  → actually throws an exception
throws → declares that a method may throw an exception
```

The easiest memory trick:

```text
throw  = action
throws = declaration
```

---

# 2. `throw` Keyword

The `throw` keyword is used to **explicitly throw an exception object**.

Syntax:

```java
throw new ExceptionType();
```

Example:

```java
throw new ArithmeticException();
```

Example:

```java
public class Main {

    public static void main(String[] args) {

        throw new ArithmeticException("Invalid calculation");

    }
}
```

Output will contain an uncaught exception similar to:

```text
java.lang.ArithmeticException: Invalid calculation
```

---

# 3. Why Do We Need `throw`?

Java automatically throws exceptions for many runtime problems.

Example:

```java
int result = 10 / 0;
```

Java automatically throws:

```text
ArithmeticException
```

But sometimes **we want to create an exceptional condition ourselves**.

Example:

```java
int age = 15;

if (age < 18) {
    throw new IllegalArgumentException("Age must be 18 or above");
}
```

Here Java did not automatically detect the business rule.

We explicitly decided:

```text
age < 18
     ↓
invalid condition
     ↓
throw exception
```

---

# 4. Basic Syntax of throw

```java
throw new ExceptionType();
```

With a message:

```java
throw new ExceptionType("message");
```

Example:

```java
throw new IllegalArgumentException("Invalid age");
```

---

# 5. `throw` Requires an Exception Object

Correct:

```java
throw new ArithmeticException();
```

Correct:

```java
ArithmeticException e = new ArithmeticException();
throw e;
```

Incorrect:

```java
throw "Error";
```

A `throw` statement requires a value whose type is compatible with `Throwable`.

---

# 6. What Can Be Thrown?

The object being thrown must be a subclass of:

```text
Throwable
```

Hierarchy:

```text
Throwable
├── Error
└── Exception
    ├── RuntimeException
    └── other Exceptions
```

Therefore, these can technically be thrown:

```java
throw new Exception();
throw new RuntimeException();
throw new ArithmeticException();
throw new Error();
```

Although throwing `Error` manually is generally not normal application design.

---

# 7. `throw` With RuntimeException

Example:

```java
public class Main {

    static void checkAge(int age) {

        if (age < 18) {
            throw new IllegalArgumentException(
                "Age must be at least 18"
            );
        }

        System.out.println("Allowed");
    }

    public static void main(String[] args) {

        checkAge(15);

    }
}
```

Flow:

```text
checkAge(15)
      ↓
age < 18
      ↓
throw IllegalArgumentException
      ↓
exception propagates
```

---

# 8. `throw` With try-catch

An explicitly thrown exception can be handled using `try-catch`.

```java
public class Main {

    public static void main(String[] args) {

        try {

            throw new ArithmeticException(
                "Something went wrong"
            );

        }
        catch (ArithmeticException e) {

            System.out.println(e.getMessage());

        }
    }
}
```

Output:

```text
Something went wrong
```

Flow:

```text
try
 ↓
throw
 ↓
exception object
 ↓
matching catch
 ↓
handled
```

---

# 9. `throw` Does Not Mean `throws`

This is one of the most important differences.

### throw

```java
throw new IOException();
```

Means:

> Throw this exception now.

### throws

```java
void readFile() throws IOException
```

Means:

> This method declares that it may throw `IOException`.

---

# 10. `throws` Keyword

The `throws` keyword is used in a **method declaration** to declare possible exceptions.

Syntax:

```java
returnType methodName() throws ExceptionType {
    // method body
}
```

Example:

```java
static void readFile() throws IOException {
    // file operation
}
```

The method is saying:

```text
This method may throw IOException.
```

---

# 11. Basic throws Example

```java
import java.io.IOException;

public class Main {

    static void readFile() throws IOException {

        throw new IOException("File error");

    }

    public static void main(String[] args)
            throws IOException {

        readFile();

    }
}
```

Here:

```java
static void readFile() throws IOException
```

declares the possibility.

And:

```java
throw new IOException("File error");
```

actually creates and throws the exception.

---

# 12. `throw` vs `throws`

| `throw`                               | `throws`                             |
| ------------------------------------- | ------------------------------------ |
| Used to explicitly throw an exception | Used to declare possible exceptions  |
| Used inside method/block              | Used in method declaration           |
| Throws an exception object            | Declares exception types             |
| One exception object at a time        | Can declare multiple exception types |
| Followed by an object                 | Followed by exception class names    |

Example:

```java
throw new IOException();
```

versus:

```java
void read() throws IOException
```

Memory trick:

```text
throw  → "Do it"
throws → "Warning / declaration"
```

---

# 13. `throw` Can Be Used Anywhere an Executable Statement Is Allowed

Example:

```java
if (age < 18) {
    throw new IllegalArgumentException();
}
```

Another example:

```java
if (balance < amount) {
    throw new IllegalStateException(
        "Insufficient balance"
    );
}
```

Another:

```java
if (username == null) {
    throw new NullPointerException(
        "Username cannot be null"
    );
}
```

The exact exception type should match the condition and API design.

---

# 14. `throws` Is Used in Method Declaration

Example:

```java
static void test() throws IOException {
}
```

It appears after the parameter list:

```text
methodName(parameters) throws ExceptionType
```

Example:

```java
static void read() throws IOException {
}
```

---

# 15. Multiple Exceptions With throws

A method can declare multiple exceptions.

```java
static void process()
        throws IOException, SQLException {

    // code
}
```

This means the method may propagate either:

```text
IOException
```

or:

```text
SQLException
```

---

# 16. Multiple Exceptions With throw

A method can also explicitly throw different exceptions depending on conditions.

```java
static void check(int value) {

    if (value < 0) {
        throw new IllegalArgumentException(
            "Negative value"
        );
    }

    if (value == 0) {
        throw new ArithmeticException(
            "Zero is not allowed"
        );
    }
}
```

Here there are multiple possible `throw` statements.

---

# 17. `throw` With Checked Exception

Consider:

```java
import java.io.IOException;

static void test() {

    throw new IOException();

}
```

This produces a compile-time error because `IOException` is checked.

The compiler requires the method to either:

```text
handle it
```

or:

```text
declare it
```

So this is valid:

```java
import java.io.IOException;

static void test() throws IOException {

    throw new IOException();

}
```

---

# 18. `throw` With try-catch

Another valid solution is to handle it:

```java
import java.io.IOException;

static void test() {

    try {

        throw new IOException();

    }
    catch (IOException e) {

        System.out.println("Handled");

    }
}
```

Therefore, for checked exceptions:

```text
throw checked exception
        ↓
must be handled or declared
```

---

# 19. `throw` With Unchecked Exception

Consider:

```java
static void test() {

    throw new ArithmeticException();

}
```

This is valid without `throws`.

Why?

Because:

```text
ArithmeticException
       ↓
RuntimeException
```

and `RuntimeException` is unchecked.

So Java does not require the method to declare it.

---

# 20. Can We Use throws With RuntimeException?

Yes.

Example:

```java
static void test()
        throws ArithmeticException {

    throw new ArithmeticException();

}
```

This is legal.

But it is usually unnecessary because `ArithmeticException` is unchecked.

---

# 21. Can We Use throws With Checked Exceptions?

Yes.

This is one of its main purposes.

```java
static void readFile()
        throws IOException {

    // operation
}
```

---

# 22. `throws` Does Not Throw the Exception

This is a common misconception.

Consider:

```java
static void test() throws IOException {
}
```

This code does not automatically throw `IOException`.

It only declares:

```text
This method may propagate IOException.
```

Something must actually throw it for the exception to occur.

---

# 23. `throw` Actually Throws

Compare:

```java
static void test() {

    throw new RuntimeException();

}
```

This immediately creates and throws the exception when execution reaches that statement.

Therefore:

```text
throws → declaration
throw  → actual throwing
```

---

# 24. Exception Propagation With throws

Consider:

```java
static void methodA() throws IOException {

    throw new IOException("Problem");

}

static void methodB() throws IOException {

    methodA();

}

public static void main(String[] args)
        throws IOException {

    methodB();

}
```

Flow:

```text
main()
  ↓
methodB()
  ↓
methodA()
  ↓
throw IOException
  ↓
methodA doesn't catch
  ↓
methodB doesn't catch
  ↓
main doesn't catch
  ↓
exception reaches JVM
```

Each method declares that it may propagate the checked exception.

---

# 25. Handling Instead of Propagating

Instead of:

```java
static void methodB() throws IOException {
    methodA();
}
```

we can handle it:

```java
static void methodB() {

    try {

        methodA();

    }
    catch (IOException e) {

        System.out.println("Handled");

    }
}
```

So a checked exception can be:

```text
handled
```

or:

```text
propagated using throws
```

---

# 26. `throw` + `throws` Together

Very commonly, both are used together.

```java
static void checkAge(int age)
        throws Exception {

    if (age < 18) {

        throw new Exception(
            "Age must be 18 or above"
        );

    }
}
```

Here:

```java
throw new Exception(...)
```

actually throws the exception.

And:

```java
throws Exception
```

declares that the method may propagate it.

---

# 27. Real-World Example

Consider a bank account:

```java
class BankAccount {

    private double balance;

    void withdraw(double amount) {

        if (amount > balance) {

            throw new IllegalArgumentException(
                "Insufficient balance"
            );

        }

        balance -= amount;
    }
}
```

Here the application defines a business rule:

```text
amount > balance
        ↓
invalid withdrawal
        ↓
throw exception
```

This is a common use of `throw`.

---

# 28. `throw` for Validation

Example:

```java
static void register(String username) {

    if (username == null || username.isBlank()) {

        throw new IllegalArgumentException(
            "Username cannot be empty"
        );

    }

    System.out.println("Registration successful");
}
```

This is useful when invalid input violates a method's contract.

---

# 29. `throw` With Custom Exceptions

You can create your own exception class.

Example:

```java
class InvalidAgeException extends Exception {

    public InvalidAgeException(String message) {
        super(message);
    }
}
```

Then:

```java
static void checkAge(int age)
        throws InvalidAgeException {

    if (age < 18) {

        throw new InvalidAgeException(
            "Age must be 18 or above"
        );

    }
}
```

Here all three concepts work together:

```text
Custom exception
       ↓
throws declaration
       ↓
throw statement
```

Custom exceptions are covered more deeply in:

```text
07-Custom-Exceptions.md
```

---

# 30. `throw` and `finally`

A `throw` statement can occur inside `try`, `catch`, or even `finally`.

Example:

```java
try {

    throw new RuntimeException();

}
catch (RuntimeException e) {

    System.out.println("Caught");

}
finally {

    System.out.println("Finally");

}
```

Output:

```text
Caught
Finally
```

---

# 31. Rethrowing an Exception

A `catch` block can throw the same exception again.

Example:

```java
static void test() {

    try {

        riskyOperation();

    }
    catch (Exception e) {

        System.out.println("Logging error");

        throw e;
    }
}
```

This is called **rethrowing** the exception.

Flow:

```text
exception
   ↓
catch
   ↓
log/process
   ↓
throw again
   ↓
caller
```

---

# 32. Wrapping an Exception

Instead of throwing the same exception, we can throw another exception while preserving the original as the cause.

Example:

```java
try {

    riskyOperation();

}
catch (IOException e) {

    throw new RuntimeException(
        "Operation failed",
        e
    );
}
```

Here:

```text
RuntimeException
      ↓
cause
      ↓
IOException
```

This is commonly used when translating lower-level exceptions into exceptions meaningful to a higher layer.

---

# 33. Exception Chaining

Example:

```java
catch (IOException e) {

    throw new RuntimeException(
        "Could not process file",
        e
    );

}
```

The second argument:

```java
e
```

is the original cause.

This preserves the original exception information.

---

# 34. `throws` and Method Calling

Suppose:

```java
static void read()
        throws IOException {
}
```

If another method calls it:

```java
static void process()
        throws IOException {

    read();

}
```

`process()` must also handle or declare the checked exception.

Alternatively:

```java
static void process() {

    try {
        read();
    }
    catch (IOException e) {
        System.out.println("Handled");
    }
}
```

---

# 35. `throws` and Method Overriding

Exception rules become important when overriding methods.

Suppose parent class:

```java
class Parent {

    void show() throws IOException {
    }
}
```

A child class can override it with:

```java
class Child extends Parent {

    @Override
    void show() throws IOException {
    }
}
```

This is valid.

But a child cannot declare a broader checked exception:

```java
class Child extends Parent {

    @Override
    void show() throws Exception {
    }
}
```

This is invalid because:

```text
Exception
   ↑
IOException
```

The child method cannot widen the checked exception contract.

---

# 36. Can Overriding Method Throw a Narrower Checked Exception?

Yes.

Parent:

```java
class Parent {

    void show() throws Exception {
    }
}
```

Child:

```java
class Child extends Parent {

    @Override
    void show() throws IOException {
    }
}
```

`IOException` is narrower than `Exception`.

This is allowed.

---

# 37. Can Overriding Method Throw No Exception?

Yes.

Parent:

```java
class Parent {

    void show() throws IOException {
    }
}
```

Child:

```java
class Child extends Parent {

    @Override
    void show() {
    }
}
```

This is valid.

The child is not required to throw the exception.

---

# 38. Runtime Exceptions and Overriding

Unchecked exceptions have more freedom.

Parent:

```java
class Parent {

    void show() throws IOException {
    }
}
```

Child:

```java
class Child extends Parent {

    @Override
    void show() throws RuntimeException {
    }
}
```

An unchecked exception does not impose the same compile-time restriction as checked exceptions.

---

# 39. `throw` vs `return`

Do not confuse:

```java
return;
```

with:

```java
throw new Exception();
```

### return

Ends the method normally.

```java
return;
```

### throw

Exits the current normal execution path by raising an exception.

```java
throw new RuntimeException();
```

Conceptually:

```text
return → normal completion
throw  → exceptional completion
```

---

# 40. `throw` vs `throws` vs `try-catch`

These three concepts have different responsibilities:

```text
throw
 ↓
actually raises exception

throws
 ↓
declares possible propagation

try-catch
 ↓
handles exception
```

Example:

```java
static void checkAge(int age)
        throws Exception {

    if (age < 18) {

        throw new Exception(
            "Not eligible"
        );

    }
}
```

Caller:

```java
try {

    checkAge(15);

}
catch (Exception e) {

    System.out.println(e.getMessage());

}
```

Full flow:

```text
checkAge()
    ↓
throw
    ↓
throws declaration
    ↓
caller
    ↓
catch
    ↓
handled
```

---

# 41. Common Mistake: `throws` Inside Method Body

Incorrect:

```java
void test() {

    throws IOException;

}
```

`throws` belongs in the method declaration.

Correct:

```java
void test() throws IOException {

}
```

---

# 42. Common Mistake: `throw` in Method Declaration

Incorrect:

```java
void test() throw IOException {
}
```

Correct:

```java
void test() throws IOException {
}
```

Or:

```java
void test() {
    throw new IOException();
}
```

For the second version, because `IOException` is checked, the method must also handle or declare it.

---

# 43. Common Mistake: Throwing a String

Incorrect:

```java
throw "Something went wrong";
```

Java requires a throwable object.

Correct:

```java
throw new RuntimeException("Something went wrong");
```

---

# 44. Common Mistake: Thinking throws Handles Exceptions

Consider:

```java
static void test() throws IOException {
}
```

`throws` does not handle anything.

It only declares possible propagation.

Handling requires:

```java
try {
}
catch (IOException e) {
}
```

---

# 45. Common Mistake: Thinking throws Is Required for Every Exception

This is unnecessary for unchecked exceptions:

```java
static void test() throws ArithmeticException {

    throw new ArithmeticException();

}
```

It is legal, but the declaration is generally unnecessary.

For checked exceptions, however, the method must handle or declare them.

---

# 46. Checked vs Unchecked With throw

### Checked

```java
static void test()
        throws IOException {

    throw new IOException();

}
```

Must handle or declare.

### Unchecked

```java
static void test() {

    throw new RuntimeException();

}
```

No declaration required.

---

# 47. `throw` Can Throw Only One Object Per Statement

Example:

```java
throw new IOException();
```

A single `throw` statement throws one exception object.

But a method can have multiple possible throw statements:

```java
if (x < 0) {
    throw new IllegalArgumentException();
}

if (x == 0) {
    throw new ArithmeticException();
}
```

---

# 48. `throws` Can Declare Multiple Exceptions

Example:

```java
static void process()
        throws IOException, SQLException {

}
```

So remember:

```text
throw
→ one exception object per statement

throws
→ multiple exception types can be declared
```

---

# 49. Can `throws` Declare `Throwable`?

Yes.

```java
static void test()
        throws Throwable {
}
```

But this is extremely broad and usually not appropriate for ordinary application APIs.

Likewise:

```java
throws Exception
```

can be valid, but declaring the most specific meaningful checked exception is often clearer.

---

# 50. Can `throw` Throw `Throwable` Directly?

Technically:

```java
throw new Throwable();
```

is legal.

But using `Throwable` directly is generally too broad for ordinary application code.

Prefer meaningful exception types.

---

# 51. `throw` and Control Flow

Example:

```java
static void test() {

    System.out.println("A");

    throw new RuntimeException();

    // System.out.println("B"); // unreachable
}
```

Output:

```text
A
```

Then the exception propagates.

The statement after the unconditional `throw` is unreachable.

---

# 52. `throw` Inside Conditional Logic

Example:

```java
static void validate(int number) {

    if (number < 0) {
        throw new IllegalArgumentException(
            "Number cannot be negative"
        );
    }

    System.out.println("Valid");
}
```

If:

```text
number = -5
```

then:

```text
condition true
     ↓
throw
     ↓
"Valid" is not executed
```

If:

```text
number = 5
```

then:

```text
condition false
     ↓
throw skipped
     ↓
Valid
```

---

# 53. `throw` and Custom Business Rules

This is one of the most practical uses.

Example:

```java
static void withdraw(double balance, double amount) {

    if (amount <= 0) {
        throw new IllegalArgumentException(
            "Amount must be positive"
        );
    }

    if (amount > balance) {
        throw new IllegalArgumentException(
            "Insufficient balance"
        );
    }

    System.out.println("Withdrawal successful");
}
```

The exception is being used to communicate that the requested operation violates the method's rules.

---

# 54. `throw` in Backend Development

In Spring Boot applications, you'll frequently see patterns conceptually like:

```java
if (user == null) {
    throw new RuntimeException("User not found");
}
```

In production applications, more specific custom exceptions are often preferable:

```java
if (user == null) {
    throw new UserNotFoundException(
        "User not found"
    );
}
```

Then an appropriate exception handler can translate that exception into an HTTP response.

For example:

```text
Service
   ↓
throw UserNotFoundException
   ↓
Exception handler
   ↓
HTTP 404 response
```

This is a very important connection between Core Java exception handling and Spring Boot backend development.

---

# 55. `throw` vs `throws` — Visual

```text
                  EXCEPTION
                     │
          ┌──────────┴──────────┐
          │                     │
        throw                 throws
          │                     │
    actually throws        declares possible
     exception object        propagation
          │                     │
          ↓                     ↓
 throw new X()            method() throws X
```

---

# 56. Complete Example

```java
import java.io.IOException;

public class Main {

    static void processAge(int age)
            throws IOException {

        if (age < 18) {

            throw new IOException(
                "Age is below required limit"
            );

        }

        System.out.println("Processing allowed");
    }

    public static void main(String[] args) {

        try {

            processAge(15);

        }
        catch (IOException e) {

            System.out.println(
                "Error: " + e.getMessage()
            );

        }
    }
}
```

Flow:

```text
main()
 ↓
processAge(15)
 ↓
age < 18
 ↓
throw IOException
 ↓
method declares throws IOException
 ↓
exception propagates to main
 ↓
catch
 ↓
handled
```

---

# 57. Interview Questions

## Q1. What is `throw`?

`throw` is used to explicitly throw an exception object.

Example:

```java
throw new IllegalArgumentException("Invalid input");
```

---

## Q2. What is `throws`?

`throws` is used in a method declaration to specify checked exceptions that the method may propagate.

Example:

```java
void read() throws IOException {
}
```

---

## Q3. Difference between `throw` and `throws`?

```text
throw
→ actually throws an exception

throws
→ declares possible exception propagation
```

---

## Q4. Can we use `throw` without `throws`?

Yes, for unchecked exceptions.

```java
throw new RuntimeException();
```

For checked exceptions, the method must handle or declare them.

---

## Q5. Can we use `throws` without `throw`?

Yes.

```java
void test() throws IOException {
}
```

The method can declare an exception even if no explicit `throw` appears in its body.

The exception could arise from another method called inside it.

---

## Q6. Can `throws` declare multiple exceptions?

Yes.

```java
void test()
    throws IOException, SQLException {
}
```

---

## Q7. Can `throw` throw multiple exceptions?

A single `throw` statement throws one exception object, but a method can contain multiple `throw` statements.

---

## Q8. Can we throw a checked exception?

Yes, but it must be handled or declared.

---

## Q9. Can we throw an unchecked exception?

Yes, and no declaration is required.

---

## Q10. Can we throw a custom exception?

Yes.

```java
throw new InvalidAgeException();
```

---

## Q11. Can `throws` be used with unchecked exceptions?

Yes.

```java
void test() throws RuntimeException {
}
```

But it is generally unnecessary.

---

## Q12. Can `throw` be used inside catch?

Yes.

```java
catch (Exception e) {
    throw e;
}
```

This is called rethrowing.

---

## Q13. What is exception chaining?

Wrapping one exception inside another while preserving the original as the cause.

```java
throw new RuntimeException("Failed", e);
```

---

## Q14. Can an overriding method declare a broader checked exception?

No.

If parent declares:

```java
void test() throws IOException
```

child cannot declare:

```java
void test() throws Exception
```

because `Exception` is broader.

---

# 58. Common Interview Traps

### Trap 1

```java
throw IOException;
```

❌ Wrong.

`throw` needs an exception object.

Correct:

```java
throw new IOException();
```

---

### Trap 2

```java
void test() throw IOException
```

❌ Wrong.

Correct:

```java
void test() throws IOException
```

---

### Trap 3

Thinking:

```java
throws IOException
```

means the exception is already thrown.

❌ No.

It only declares possible propagation.

---

### Trap 4

Thinking `throws` is required for `RuntimeException`.

❌ No.

Unchecked exceptions do not require declaration.

---

### Trap 5

Thinking `throw` can throw anything.

❌ No.

The thrown object must be compatible with `Throwable`.

---

### Trap 6

Thinking `throw` handles an exception.

❌ No.

`throw` raises it.

`catch` handles it.

---

# 59. 30-Second Interview Answer

> **`throw` and `throws` are both used for exception handling but serve different purposes. `throw` is used inside executable code to explicitly throw an exception object, while `throws` is used in a method declaration to declare that the method may propagate one or more exceptions. For checked exceptions, the method must either handle them or declare them using `throws`. Unchecked exceptions do not require a `throws` declaration.**

---

# 60. Quick Revision

```text
throw
────────────────────────────
✓ Explicitly throws an exception object
✓ Used inside method/block
✓ Syntax: throw new ExceptionType();
✓ One object per throw statement
✓ Can throw checked or unchecked exceptions
✓ Checked exception must be handled or declared


throws
────────────────────────────
✓ Declares possible exception propagation
✓ Used in method declaration
✓ Syntax: method() throws ExceptionType
✓ Can declare multiple exceptions
✓ Mainly important for checked exceptions
✓ Does NOT itself throw an exception


TRY-CATCH
────────────────────────────
✓ Handles exceptions
```

## 🔥 Ultimate Memory Trick

```text
throw  → THROW the exception
throws → TELLS the caller about the exception
catch  → CATCH the exception
finally → FINISH/CLEAN UP
```

### One complete example:

```java
static void check(int age) throws Exception {

    if (age < 18) {
        throw new Exception("Under age");
    }
}
```

Think:

```text
throws
  ↓
"This method may propagate Exception"

throw
  ↓
"Here is the actual exception"

catch
  ↓
"Now I will handle it"
```
