# Checked vs Unchecked Exceptions in Java

## 1. Introduction

Java exceptions can broadly be divided into two categories:

```text
Exception
│
├── Checked Exceptions
│
└── Unchecked Exceptions
     └── RuntimeException and its subclasses
```

The main difference is **when the compiler forces you to deal with the exception**.

```text
Checked Exception
    ↓
Compiler checks it
    ↓
Must handle or declare it

Unchecked Exception
    ↓
Compiler does NOT force handling
    ↓
Can handle it, but not mandatory
```

This distinction is extremely important in Java because it affects:

* compilation
* method declarations
* API design
* exception propagation
* application architecture
* interview questions

---

# 2. First Understand the Hierarchy

The distinction becomes easy once the hierarchy is clear:

```text
Object
   │
   └── Throwable
        ├── Error
        │
        └── Exception
             ├── RuntimeException
             │    ├── NullPointerException
             │    ├── ArithmeticException
             │    ├── NumberFormatException
             │    └── ...
             │
             └── Other Exceptions
                  ├── IOException
                  ├── SQLException
                  ├── ClassNotFoundException
                  └── ...
```

### Important rule

> **Checked exceptions are generally exceptions that are subclasses of `Exception` but NOT subclasses of `RuntimeException`.**

And:

> **Unchecked exceptions include `RuntimeException` and its subclasses.**

`Error` types are also unchecked in the sense that the compiler does not require them to be caught or declared, but they are **not classified as exceptions**.

---

# 3. What is a Checked Exception?

A **checked exception** is an exception that the Java compiler checks at compile time.

If a method can throw a checked exception, the code must generally do one of two things:

```text
1. Handle it using try-catch

OR

2. Declare it using throws
```

Otherwise, the program will not compile.

---

# 4. Example of a Checked Exception

Consider:

```java
import java.io.FileReader;

public class Main {

    public static void main(String[] args) {

        FileReader reader = new FileReader("data.txt");

    }
}
```

This produces a compilation error because opening the file can result in:

```text
FileNotFoundException
```

which is a checked exception.

The compiler essentially says:

```text
"Deal with this exception."
```

---

# 5. Handling a Checked Exception

We can use `try-catch`:

```java
import java.io.FileReader;
import java.io.FileNotFoundException;

public class Main {

    public static void main(String[] args) {

        try {
            FileReader reader = new FileReader("data.txt");
        }
        catch (FileNotFoundException e) {
            System.out.println("File not found.");
        }
    }
}
```

Now the compiler is satisfied because the exception has been handled.

---

# 6. Declaring a Checked Exception

Instead of handling the exception, we can declare it using `throws`.

```java
import java.io.FileReader;
import java.io.FileNotFoundException;

public class Main {

    static void readFile() throws FileNotFoundException {

        FileReader reader = new FileReader("data.txt");

    }
}
```

Here:

```java
throws FileNotFoundException
```

means:

> This method may allow the exception to propagate to its caller.

The method is not handling the exception itself.

---

# 7. Handle or Declare Rule

This is one of the most important rules in Java.

For a checked exception:

```text
                  Checked Exception
                         │
              ┌──────────┴──────────┐
              ↓                     ↓
           catch                  throws
              ↓                     ↓
           Handle             Declare/propagate
```

Example:

```java
try {
    riskyOperation();
}
catch (IOException e) {
    // handle
}
```

OR:

```java
void method() throws IOException {
    riskyOperation();
}
```

You generally cannot simply ignore a checked exception.

---

# 8. Why Does Java Have Checked Exceptions?

Checked exceptions are designed to force developers to consciously consider certain recoverable or externally influenced failure conditions.

For example:

```text
File does not exist
Database unavailable
Network operation fails
Input/output operation fails
Requested class cannot be found
```

These situations may depend on things outside the immediate control of the program.

For example:

```text
Your code
   ↓
Open file
   ↓
Does file exist?
   ↓
Maybe YES
Maybe NO
```

Java requires the programmer to account for such possibilities when using APIs that declare checked exceptions.

---

# 9. Common Checked Exceptions

Some commonly encountered checked exceptions include:

```text
IOException
FileNotFoundException
SQLException
ClassNotFoundException
InterruptedException
```

Hierarchy examples:

```text
Exception
├── IOException
│    └── FileNotFoundException
│
├── SQLException
│
├── ClassNotFoundException
│
└── InterruptedException
```

---

# 10. What is an Unchecked Exception?

An **unchecked exception** is an exception that the compiler does not force you to handle or declare.

The main category is:

```text
RuntimeException
```

and its subclasses.

Examples:

```text
NullPointerException
ArithmeticException
NumberFormatException
IllegalArgumentException
IndexOutOfBoundsException
```

---

# 11. Example of an Unchecked Exception

```java
public class Main {

    public static void main(String[] args) {

        int result = 10 / 0;

        System.out.println(result);
    }
}
```

This code compiles.

But when it runs:

```text
ArithmeticException: / by zero
```

The compiler did not force us to write:

```java
try-catch
```

or:

```java
throws ArithmeticException
```

---

# 12. Why Doesn't the Compiler Force Runtime Exceptions?

Runtime exceptions commonly indicate programming mistakes, invalid assumptions, or invalid state that could potentially be prevented by correcting the code or validating inputs.

Examples:

```java
String name = null;
name.length();
```

Problem:

```text
NullPointerException
```

Another example:

```java
int[] arr = {10, 20, 30};

System.out.println(arr[10]);
```

Problem:

```text
ArrayIndexOutOfBoundsException
```

Another:

```java
Integer.parseInt("abc");
```

Problem:

```text
NumberFormatException
```

Java does not require every such possibility to be surrounded by `try-catch`.

---

# 13. Checked vs Unchecked — Core Difference

| Feature                  | Checked                                             | Unchecked                         |
| ------------------------ | --------------------------------------------------- | --------------------------------- |
| Compiler checks          | Yes                                                 | No                                |
| Must handle/declare      | Yes                                                 | No                                |
| Main hierarchy           | `Exception` excluding `RuntimeException` subclasses | `RuntimeException` and subclasses |
| Compile-time enforcement | Yes                                                 | No                                |
| Example                  | `IOException`                                       | `NullPointerException`            |
| Typical cause            | External/recoverable conditions                     | Programming errors/invalid state  |
| `try-catch` mandatory?   | Generally yes if not declared                       | No                                |

---

# 14. Simple Example

### Checked

```java
FileReader reader = new FileReader("data.txt");
```

Compiler:

```text
"Handle or declare the exception."
```

### Unchecked

```java
int x = 10 / 0;
```

Compiler:

```text
"No compilation error."
```

Runtime:

```text
ArithmeticException
```

---

# 15. Checked Exception Flow

Suppose:

```java
void read() throws IOException {
    // file operation
}
```

and:

```java
void process() throws IOException {
    read();
}
```

and:

```java
public static void main(String[] args) throws IOException {
    process();
}
```

The exception can propagate:

```text
main()
   ↓
process()
   ↓
read()
   ↓
IOException
```

Each method can choose to:

```text
handle
```

or:

```text
declare
```

the checked exception.

---

# 16. Checked Exception Propagation

Example:

```java
static void method3() throws IOException {
    throw new IOException("File problem");
}

static void method2() throws IOException {
    method3();
}

static void method1() throws IOException {
    method2();
}
```

Here the exception propagates upward:

```text
method3()
   ↓
method2()
   ↓
method1()
```

because each method declares:

```java
throws IOException
```

Eventually a caller must handle it or continue declaring it.

---

# 17. Unchecked Exception Propagation

Runtime exceptions can also propagate.

Example:

```java
static void method3() {
    int x = 10 / 0;
}

static void method2() {
    method3();
}

static void method1() {
    method2();
}
```

The exception propagates:

```text
method3()
   ↓
method2()
   ↓
method1()
```

But none of these methods are required by the compiler to write:

```java
throws ArithmeticException
```

because `ArithmeticException` is unchecked.

---

# 18. `throws` with Unchecked Exceptions

You technically can declare an unchecked exception:

```java
static void divide() throws ArithmeticException {
    int x = 10 / 0;
}
```

This is legal.

But it is **not required**.

Why?

Because:

```text
ArithmeticException
       ↓
RuntimeException
```

and runtime exceptions are unchecked.

So:

```java
static void divide() {
}
```

and:

```java
static void divide() throws ArithmeticException {
}
```

are both legal.

The `throws` declaration can still communicate an intended possibility to callers, even though the compiler does not require it.

---

# 19. `throws Exception`

You can declare:

```java
void method() throws Exception {
    // ...
}
```

This is legal.

But remember:

```text
Exception
├── RuntimeException
└── Checked Exceptions
```

So a broad:

```java
throws Exception
```

can represent that the method may propagate various exception types.

However, broad declarations can hide which specific failures callers should expect.

---

# 20. `catch (Exception e)`

Similarly:

```java
try {
    // code
}
catch (Exception e) {
    // handle
}
```

can catch many exception types because:

```text
Exception
```

is a parent of many exception classes.

For example:

```text
Exception
├── IOException
├── SQLException
├── RuntimeException
│    ├── NullPointerException
│    └── ArithmeticException
└── ...
```

Therefore:

```java
catch (Exception e)
```

can catch both checked and unchecked exceptions that extend `Exception`.

---

# 21. Are All `Exception` Classes Checked?

**No.**

This is a very common interview trap.

Consider:

```text
Exception
├── RuntimeException
│    ├── NullPointerException
│    └── ArithmeticException
│
└── IOException
```

`NullPointerException` is an `Exception`, but it is unchecked because it extends `RuntimeException`.

So the correct rule is:

> **Checked exceptions are subclasses of `Exception` excluding `RuntimeException` and its subclasses.**

---

# 22. Are Errors Checked or Unchecked?

`Error` is not an `Exception`.

But the compiler does not force you to catch or declare `Error` types.

Therefore, from the compiler-enforcement perspective:

```text
Checked
    ↓
Compiler forces handling/declaration

Unchecked
    ↓
Compiler does not force handling/declaration
```

`Error` belongs to the latter category.

But do not say:

> "Error is an unchecked exception."

That terminology is inaccurate.

Better:

> **Errors are throwable types that are not checked exceptions; the compiler does not require them to be caught or declared.**

---

# 23. Important Terminology

### Checked Exception

A subclass of `Exception` that is **not** a subclass of `RuntimeException`.

Examples:

```text
IOException
SQLException
InterruptedException
ClassNotFoundException
```

### Unchecked Exception

A `RuntimeException` or one of its subclasses.

Examples:

```text
NullPointerException
ArithmeticException
NumberFormatException
IllegalArgumentException
```

### Error

A separate branch under `Throwable`.

Examples:

```text
OutOfMemoryError
StackOverflowError
```

---

# 24. Compiler Perspective

This is the easiest way to understand the difference.

### Checked

```java
void readFile() {

    FileReader reader = new FileReader("data.txt");

}
```

Compiler effectively requires:

```text
catch
OR
throws
```

Otherwise:

```text
COMPILATION ERROR
```

---

### Unchecked

```java
void divide() {

    int result = 10 / 0;

}
```

Compiler:

```text
Compilation succeeds
```

Runtime:

```text
ArithmeticException
```

---

# 25. Compile Time vs Runtime

This distinction is critical.

### Checked exception

The **requirement to handle/declare** is checked at compile time.

The actual exception itself still occurs at runtime.

So don't say:

> "Checked exceptions happen at compile time."

That's wrong.

Correct:

> **The compiler checks whether checked exceptions are handled or declared, but the exception itself occurs during program execution.**

---

# 26. Example to Understand This Clearly

```java
try {
    FileReader reader = new FileReader("data.txt");
}
catch (FileNotFoundException e) {
    System.out.println("File doesn't exist.");
}
```

There are two different stages:

```text
COMPILE TIME
     ↓
Compiler checks:
"Is FileNotFoundException handled or declared?"
     ↓
YES
     ↓
Compilation succeeds


RUNTIME
     ↓
Java attempts to open file
     ↓
File exists?
     ├── YES → continue
     └── NO  → FileNotFoundException occurs
```

This distinction is extremely important.

---

# 27. Why `NullPointerException` Is Unchecked

Consider:

```java
String name = null;

System.out.println(name.length());
```

The compiler cannot generally require every dereference to be wrapped in:

```java
try-catch
```

Instead, Java treats this kind of problem as a runtime exception.

The developer should normally prevent invalid state or validate the reference where appropriate.

---

# 28. Why `IOException` Is Checked

Consider:

```java
FileReader reader = new FileReader("data.txt");
```

Whether the file exists depends on the external environment.

Your code might be correct, but:

```text
File deleted
File moved
Wrong path
Permission denied
Disk problem
```

can still cause I/O failures.

Java therefore requires the programmer to explicitly account for the declared checked exception.

---

# 29. Checked Exceptions and API Design

Checked exceptions influence method signatures.

For example:

```java
void readFile() throws IOException
```

tells callers:

> "This operation has a checked failure condition you must account for."

This becomes part of the method's API contract.

The caller can then decide:

```text
handle it
```

or:

```text
propagate it
```

---

# 30. Unchecked Exceptions and API Design

Suppose:

```java
void withdraw(double amount) {

    if (amount < 0) {
        throw new IllegalArgumentException("Invalid amount");
    }
}
```

`IllegalArgumentException` is unchecked.

The caller is not forced to write:

```java
try-catch
```

The API can communicate invalid arguments through the exception itself.

---

# 31. When Should You Use Checked Exceptions?

Checked exceptions can be useful when:

* the caller can reasonably recover
* the condition is outside the immediate control of the program
* the API wants to force callers to acknowledge a failure possibility

Examples:

```text
File I/O
Database operations
Some network operations
Thread interruption
```

---

# 32. When Are Unchecked Exceptions Appropriate?

Unchecked exceptions are commonly used for:

* invalid method arguments
* invalid object state
* programming errors
* violated assumptions
* null references
* invalid indexes
* invalid conversions

Examples:

```text
IllegalArgumentException
IllegalStateException
NullPointerException
IndexOutOfBoundsException
NumberFormatException
```

---

# 33. Important Design Point

Do not blindly use:

```java
catch (Exception e)
```

everywhere.

Bad:

```java
try {
    // huge amount of code
}
catch (Exception e) {
    // ignore
}
```

Problems:

* hides bugs
* makes debugging harder
* loses useful information
* can make application behavior unpredictable

Prefer handling the exception at an appropriate level.

---

# 34. Converting Checked to Unchecked

Sometimes applications wrap a checked exception inside an unchecked exception.

Example:

```java
try {
    // file operation
}
catch (IOException e) {
    throw new RuntimeException(e);
}
```

Why?

Because the current layer may not be able to recover meaningfully and may want to propagate the failure without adding a checked exception requirement to its own API.

This is an architectural/design decision and should be used deliberately.

---

# 35. Checked vs Unchecked Example

Consider a backend application.

### Checked

```text
Database connection
      ↓
Database unavailable
      ↓
SQLException
      ↓
Compiler requires handling/declaring
```

### Unchecked

```text
User provides invalid index
      ↓
Index operation
      ↓
IndexOutOfBoundsException
      ↓
Compiler does not require handling
```

---

# 36. Common Interview Traps

## Trap 1: "Checked exceptions occur at compile time."

❌ Incorrect.

Correct:

> The compiler checks whether checked exceptions are handled or declared. The exception itself occurs at runtime.

---

## Trap 2: "All subclasses of Exception are checked."

❌ Incorrect.

`RuntimeException` and its subclasses are unchecked.

---

## Trap 3: "RuntimeException is not an Exception."

❌ Incorrect.

```text
RuntimeException extends Exception
```

---

## Trap 4: "Error is a checked exception."

❌ Incorrect.

`Error` is a separate branch under `Throwable`.

---

## Trap 5: "Unchecked exceptions cannot be caught."

❌ Incorrect.

You can catch them:

```java
try {
    int x = 10 / 0;
}
catch (ArithmeticException e) {
    System.out.println("Handled");
}
```

The difference is that the compiler **does not force you** to catch them.

---

## Trap 6: "You cannot use throws with RuntimeException."

❌ Incorrect.

This is legal:

```java
void test() throws NullPointerException {
}
```

It is simply not required.

---

## Trap 7: "Checked means the exception will definitely happen."

❌ Incorrect.

It means the compiler requires the possibility to be handled or declared.

The exception may never actually occur at runtime.

---

# 37. Interview Question: Why Are Runtime Exceptions Unchecked?

A good answer:

> Runtime exceptions are unchecked because Java does not require programmers to explicitly handle every potential programming error or invalid runtime state. Requiring every possible runtime programming mistake to be caught could make code unnecessarily verbose and obscure the actual logic.

---

# 38. Interview Question: Why Are Checked Exceptions Checked?

A good answer:

> Checked exceptions are checked by the compiler because they often represent conditions that the caller may reasonably be expected to handle or propagate, such as I/O failures. Java forces the programmer to explicitly acknowledge these possibilities.

---

# 39. Interview Question: Can We Catch an Unchecked Exception?

Yes.

```java
try {
    int x = 10 / 0;
}
catch (ArithmeticException e) {
    System.out.println("Handled");
}
```

The difference is:

```text
Checked
→ handling/declaring is mandatory according to compiler rules

Unchecked
→ handling/declaring is optional
```

---

# 40. Interview Question: Can We Throw an Unchecked Exception Manually?

Yes.

```java
throw new IllegalArgumentException("Age cannot be negative");
```

This is very common.

---

# 41. Interview Question: Can We Throw a Checked Exception Manually?

Yes.

```java
throw new IOException("File operation failed");
```

But because `IOException` is checked, the surrounding method must handle it or declare it.

Example:

```java
void test() throws IOException {

    throw new IOException("File operation failed");

}
```

---

# 42. Interview Question: What Is the Difference Between `throw` and `throws`?

### `throw`

Actually throws an exception object.

```java
throw new IOException();
```

### `throws`

Declares that a method may propagate exceptions.

```java
void read() throws IOException {
}
```

Detailed discussion is covered in:

```text
06-throw-and-throws.md
```

---

# 43. Interview Question: Can One Method Throw Both Checked and Unchecked Exceptions?

Yes.

Example:

```java
void process() throws IOException {

    if (someCondition) {
        throw new IOException();
    }

    if (anotherCondition) {
        throw new IllegalArgumentException();
    }
}
```

Only `IOException` needs compiler-enforced handling/declaration.

`IllegalArgumentException` is unchecked.

---

# 44. Interview Question: Can a Method Declare `throws RuntimeException`?

Yes.

```java
void test() throws RuntimeException {
}
```

But the compiler does not require callers to handle it.

---

# 45. Interview Question: Can `main()` Declare a Checked Exception?

Yes.

Example:

```java
public static void main(String[] args) throws Exception {

    FileReader reader = new FileReader("data.txt");

}
```

This is legal.

Here `main()` is declaring that the exception may propagate from `main()`.

If it reaches the uncaught-exception mechanism, the program's main thread terminates.

---

# 46. Interview Question: Is `Throwable` Checked?

The concept of "checked" is specifically about exception classes subject to compiler checking.

`Throwable` itself is the root type and includes both `Error` and `Exception`.

You should not describe `Throwable` simply as "a checked exception."

Instead, remember:

```text
Throwable
├── Error
└── Exception
     ├── RuntimeException → unchecked
     └── other Exception subclasses → generally checked
```

---

# 47. Quick Comparison Table

| Property              | Checked Exception                      | Unchecked Exception              |
| --------------------- | -------------------------------------- | -------------------------------- |
| Compiler enforcement  | Yes                                    | No                               |
| Must catch or declare | Yes                                    | No                               |
| Main superclass path  | `Exception` but not `RuntimeException` | `RuntimeException`               |
| Occurs at             | Runtime                                | Runtime                          |
| Checked at            | Compile time                           | Not compiler-enforced            |
| Common examples       | `IOException`, `SQLException`          | `NPE`, `ArithmeticException`     |
| Can manually throw?   | Yes                                    | Yes                              |
| Can manually catch?   | Yes                                    | Yes                              |
| Can use `throws`?     | Yes                                    | Yes                              |
| Typical focus         | Recoverable/external conditions        | Programming errors/invalid state |

---

# 48. Mental Model

Think of Java's compiler as a gatekeeper.

### Checked

```text
You write code
     ↓
Compiler sees checked exception
     ↓
"Did you handle it?"
     ├── YES → compile
     └── NO  → compilation error
```

### Unchecked

```text
You write code
     ↓
RuntimeException possibility
     ↓
Compiler does not force handling
     ↓
Compile succeeds
     ↓
Exception may occur at runtime
```

---

# 49. One Diagram to Remember

```text
                         Throwable
                        /         \
                       /           \
                   Error         Exception
                                  /       \
                                 /         \
                    RuntimeException      Checked
                           │                 │
                           │                 ├── IOException
                           │                 ├── SQLException
                           │                 └── ClassNotFoundException
                           │
                           ├── NullPointerException
                           ├── ArithmeticException
                           ├── IllegalArgumentException
                           │       └── NumberFormatException
                           └── IndexOutOfBoundsException
```

Remember:

```text
RuntimeException → Unchecked
Other Exception subclasses → Checked
Error → separate branch, not an Exception
```

---

# 50. 30-Second Interview Answer

> **Checked exceptions are exceptions whose handling or declaration is enforced by the Java compiler. They are generally subclasses of `Exception` that are not subclasses of `RuntimeException`, such as `IOException` and `SQLException`. Unchecked exceptions are `RuntimeException` and its subclasses, such as `NullPointerException`, `ArithmeticException`, and `IllegalArgumentException`. Both types occur at runtime; the difference is that the compiler forces checked exceptions to be handled or declared, while it does not impose that requirement for unchecked exceptions.**

---

# 51. Final Cheat Sheet

```text
CHECKED
────────────────────────────────────
Subclass of Exception
BUT NOT RuntimeException

Examples:
IOException
SQLException
ClassNotFoundException
InterruptedException

Compiler:
"Handle it or declare it."


UNCHECKED
────────────────────────────────────
RuntimeException + subclasses

Examples:
NullPointerException
ArithmeticException
NumberFormatException
IllegalArgumentException
IndexOutOfBoundsException

Compiler:
"Handling is not mandatory."


ERROR
────────────────────────────────────
Separate branch under Throwable

Examples:
OutOfMemoryError
StackOverflowError

Compiler:
Does not require catch/throws.

BUT:
Error ≠ Exception
```

## 🔥 Most Important Interview Rule

```text
Checked Exception
= Exception
  └── NOT RuntimeException

Unchecked Exception
= RuntimeException
  └── all its subclasses
```

> **Never memorize checked vs unchecked only by individual class names. Learn the hierarchy and derive the answer from inheritance.**
