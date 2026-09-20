# Exception Handling — Interview Questions

## 1. What is an exception?

An **exception** is an event that occurs during program execution and disrupts the normal flow of the program.

Example:

```java
int result = 10 / 0;
```

This causes:

```text
ArithmeticException
```

---

# 2. What is exception handling?

Exception handling is a mechanism used to detect, handle, and manage runtime errors so that the program can respond appropriately instead of terminating unexpectedly.

Java provides:

```text
try
catch
finally
throw
throws
```

---

# 3. What is the root class of Java exceptions?

The root class is:

```java
Throwable
```

Hierarchy:

```text
Object
   ↓
Throwable
   ├── Error
   │
   └── Exception
        ├── RuntimeException
        └── Other Exceptions
```

---

# 4. What is the difference between Error and Exception?

### Error

Represents serious problems generally outside normal application handling.

Examples:

```text
OutOfMemoryError
StackOverflowError
```

### Exception

Represents conditions that an application may handle.

Examples:

```text
IOException
SQLException
ArithmeticException
NullPointerException
```

---

# 5. What are checked exceptions?

Checked exceptions are exceptions checked by the compiler.

Examples:

```text
IOException
SQLException
ClassNotFoundException
```

They generally must be:

```text
caught
```

or:

```text
declared using throws
```

Example:

```java
void readFile() throws IOException {
}
```

---

# 6. What are unchecked exceptions?

Unchecked exceptions are exceptions that are not checked by the compiler for mandatory handling.

They are subclasses of:

```java
RuntimeException
```

Examples:

```text
ArithmeticException
NullPointerException
ArrayIndexOutOfBoundsException
IllegalArgumentException
```

---

# 7. Checked vs Unchecked

```text
                 Exception
                    │
          ┌─────────┴─────────┐
          │                   │
   RuntimeException      Other Exceptions
          │                   │
      Unchecked             Checked
```

### Checked

```text
Compiler checks
Must catch or declare
```

### Unchecked

```text
Compiler doesn't require
catch/throws
```

---

# 8. What is the difference between `throw` and `throws`?

### `throw`

Actually throws an exception object.

```java
throw new IllegalArgumentException(
    "Invalid age"
);
```

### `throws`

Declares that a method may propagate an exception.

```java
void readFile()
        throws IOException {

}
```

Memory trick:

```text
throw
→ actually throws

throws
→ declares
```

---

# 9. Can we use `throw` without `throws`?

Yes, depending on the exception type.

For an unchecked exception:

```java
throw new RuntimeException();
```

No `throws` declaration is required.

For a checked exception:

```java
throw new IOException();
```

the method must handle it or declare it.

Example:

```java
void test()
        throws IOException {

    throw new IOException();

}
```

---

# 10. Can we use `throws` without actually throwing an exception?

Yes.

```java
void test()
        throws IOException {

    System.out.println("Hello");

}
```

The method declares that an `IOException` may propagate, even though this particular implementation doesn't actually throw one.

---

# 11. Can a method declare multiple exceptions?

Yes.

```java
void test()
        throws IOException, SQLException {

}
```

Multiple exceptions can be declared using commas.

---

# 12. What is a `try` block?

A `try` block contains code that may produce an exception.

```java
try {

    int result = 10 / 0;

}
```

A `try` block must generally be followed by:

```text
catch
or
finally
```

---

# 13. Can we have `try` without `catch`?

Yes, if it has a `finally` block.

```java
try {

    System.out.println("Hello");

}
finally {

    System.out.println("Cleanup");

}
```

---

# 14. Can we have `try` without `catch` and without `finally`?

No.

This is invalid:

```java
try {

    System.out.println("Hello");

}
```

A `try` statement must be followed by `catch`, `finally`, or both.

---

# 15. Can we have multiple catch blocks?

Yes.

```java
try {

    // risky code

}
catch (ArithmeticException e) {

}
catch (NullPointerException e) {

}
catch (Exception e) {

}
```

---

# 16. What is catch block ordering?

More specific exceptions should come before more general exceptions.

Correct:

```java
try {

}
catch (ArithmeticException e) {

}
catch (RuntimeException e) {

}
catch (Exception e) {

}
```

Incorrect:

```java
try {

}
catch (Exception e) {

}
catch (ArithmeticException e) {

}
```

Why?

Because:

```text
ArithmeticException
       ↓
RuntimeException
       ↓
Exception
```

If `Exception` is caught first, it can already catch `ArithmeticException`.

---

# 17. Can we catch `Throwable`?

Yes.

```java
try {

}
catch (Throwable t) {

}
```

But catching `Throwable` broadly is generally not recommended for normal application logic because it also includes `Error`.

---

# 18. What is `finally`?

`finally` is a block generally used for cleanup code.

Example:

```java
try {

    System.out.println("Try");

}
finally {

    System.out.println("Finally");

}
```

It generally executes when control leaves the try/catch structure, subject to exceptional JVM termination cases.

---

# 19. Is `finally` always executed?

Not literally always.

Usually it executes, but there are situations where normal Java cleanup cannot complete.

For example:

```java
System.exit(0);
```

can terminate the JVM before normal `finally` completion.

Also, catastrophic JVM/process termination can prevent it.

So the interview-safe answer is:

> `finally` normally executes whether an exception occurs or not, but it is not guaranteed under JVM termination scenarios such as `System.exit()`.

---

# 20. What happens if `return` is inside `try`?

Example:

```java
static int test() {

    try {

        return 10;

    }
    finally {

        System.out.println("Finally");

    }
}
```

Output:

```text
Finally
```

Return value:

```text
10
```

The `finally` block executes before the method actually returns.

---

# 21. What if `return` is inside `finally`?

Example:

```java
static int test() {

    try {

        return 10;

    }
    finally {

        return 20;

    }
}
```

Result:

```text
20
```

The `finally` return overrides the earlier return.

### Important:

Avoid returning from `finally`.

It can hide exceptions and make control flow confusing.

---

# 22. What if `finally` throws an exception?

Example:

```java
try {

    throw new Exception("Try");

}
finally {

    throw new RuntimeException("Finally");

}
```

The exception from `finally` can replace the exception from the `try` block in normal propagation.

This is one reason throwing from `finally` is generally discouraged.

---

# 23. What happens if both try and finally contain `return`?

```java
static int test() {

    try {

        return 10;

    }
    finally {

        return 20;

    }
}
```

Result:

```text
20
```

The `finally` return wins.

---

# 24. Can `finally` exist without `catch`?

Yes.

```java
try {

    System.out.println("Try");

}
finally {

    System.out.println("Cleanup");

}
```

---

# 25. Can `finally` exist with multiple `catch` blocks?

Yes.

```java
try {

}
catch (IOException e) {

}
catch (SQLException e) {

}
finally {

}
```

---

# 26. What is try-with-resources?

Try-with-resources automatically closes resources after use.

Introduced in:

```text
Java 7
```

Example:

```java
try (FileReader reader =
        new FileReader("data.txt")) {

    // use reader

}
```

The resource is automatically closed.

---

# 27. What interface is important for try-with-resources?

```java
AutoCloseable
```

A resource used in try-with-resources must satisfy the `AutoCloseable` contract.

Example:

```java
class MyResource
        implements AutoCloseable {

    @Override
    public void close() {

        System.out.println("Closed");

    }
}
```

---

# 28. In what order are multiple resources closed?

Reverse declaration order.

```java
try (
    Resource1 r1 = new Resource1();
    Resource2 r2 = new Resource2();
    Resource3 r3 = new Resource3()
) {

}
```

Closing order:

```text
r3
 ↓
r2
 ↓
r1
```

Memory trick:

```text
First opened
→ Last closed
```

---

# 29. What are suppressed exceptions?

Suppose:

```text
try block
   ↓
throws Exception A

close()
   ↓
throws Exception B
```

Exception A is generally the primary exception.

Exception B can become a **suppressed exception**.

You can access it using:

```java
e.getSuppressed();
```

---

# 30. What is exception propagation?

If an exception isn't handled in the current method, it can propagate to the caller.

Example:

```java
void method3() {

    int x = 10 / 0;

}

void method2() {

    method3();

}

void method1() {

    method2();

}
```

Flow:

```text
method1()
   ↓
method2()
   ↓
method3()
   ↓
ArithmeticException
   ↓
method2()
   ↓
method1()
```

If no matching handler is found, the exception can reach the JVM's uncaught-exception handling mechanism.

---

# 31. What is stack unwinding?

When an exception propagates backward through method calls, Java searches the call stack for a matching exception handler.

Example:

```text
main()
  ↓
method1()
  ↓
method2()
  ↓
method3()
  ↓
exception
```

If `method3()` doesn't handle it:

```text
method3()
   ↓
method2()
```

If `method2()` doesn't handle it:

```text
method2()
   ↓
method1()
```

This process is called:

```text
Stack unwinding
```

---

# 32. What is the difference between `final`, `finally`, and `finalize`?

### `final`

Keyword.

Used with:

```text
variable
method
class
```

Example:

```java
final int x = 10;
```

### `finally`

Exception-handling block.

```java
finally {

}
```

### `finalize`

An old `Object` method associated with garbage collection.

It has been **deprecated for removal** and should not be used for resource cleanup.

Modern Java uses mechanisms such as:

```text
try-with-resources
AutoCloseable
```

instead.

---

# 33. Can we have nested try blocks?

Yes.

```java
try {

    try {

        int x = 10 / 0;

    }
    catch (ArithmeticException e) {

        System.out.println(
            "Inner catch"
        );

    }

}
catch (Exception e) {

    System.out.println(
        "Outer catch"
    );

}
```

---

# 34. Can one try block have multiple catch blocks?

Yes.

```java
try {

}
catch (ArithmeticException e) {

}
catch (NullPointerException e) {

}
catch (Exception e) {

}
```

Only the first matching catch block is executed.

---

# 35. Can one catch block handle multiple exception types?

Yes.

This is called **multi-catch**.

```java
try {

}
catch (ArithmeticException |
       NullPointerException e) {

    System.out.println(
        "Exception occurred"
    );
}
```

Introduced in:

```text
Java 7
```

---

# 36. What is multi-catch?

Multi-catch allows multiple unrelated exception types to be handled by one catch block.

Example:

```java
catch (IOException |
       SQLException e) {

    System.out.println(
        e.getMessage()
    );
}
```

This avoids duplicate handling code.

---

# 37. Can related exceptions be used in multi-catch?

No.

This is invalid:

```java
catch (Exception |
       IOException e) {
}
```

because:

```text
IOException
       ↓
Exception
```

The types overlap.

The more general `Exception` already covers `IOException`.

---

# 38. Can a catch variable be reassigned in multi-catch?

The multi-catch parameter is implicitly final.

So:

```java
catch (IOException |
       SQLException e) {

}
```

you cannot reassign:

```java
e = anotherException;
```

---

# 39. What is a custom exception?

A custom exception is a user-defined exception class.

Example:

```java
class UserNotFoundException
        extends RuntimeException {

    public UserNotFoundException(
            String message) {

        super(message);
    }
}
```

Then:

```java
throw new UserNotFoundException(
    "User not found"
);
```

---

# 40. Is every custom exception checked?

No.

### Checked:

```java
class MyException
        extends Exception {
}
```

### Unchecked:

```java
class MyException
        extends RuntimeException {
}
```

---

# 41. Why use custom exceptions?

They allow an application to represent meaningful domain-specific conditions.

Examples:

```text
UserNotFoundException
InsufficientBalanceException
ProductOutOfStockException
PaymentFailedException
```

Instead of:

```text
Exception
```

everywhere.

---

# 42. Can custom exceptions have fields?

Yes.

```java
class PaymentException
        extends RuntimeException {

    private int paymentId;

    public PaymentException(
            String message,
            int paymentId) {

        super(message);
        this.paymentId = paymentId;
    }
}
```

A custom exception is a normal class that inherits exception behavior.

---

# 43. Why do we use `super(message)`?

Example:

```java
class UserNotFoundException
        extends RuntimeException {

    public UserNotFoundException(
            String message) {

        super(message);
    }
}
```

`super(message)` calls the parent constructor and stores the message in the inherited exception state.

Later:

```java
e.getMessage();
```

can retrieve it.

---

# 44. What is exception chaining?

Exception chaining means preserving the original exception as the cause of another exception.

Example:

```java
try {

    databaseOperation();

}
catch (SQLException e) {

    throw new RuntimeException(
        "Database operation failed",
        e
    );
}
```

Now:

```java
e.getCause();
```

can provide access to the original exception.

---

# 45. Why is exception chaining useful?

It allows us to:

```text
Add higher-level context
+
Preserve original cause
```

For example:

```text
Service layer:
User creation failed

Original cause:
SQLException
```

This is useful for debugging and logging.

---

# 46. What is `getMessage()`?

Returns the exception's detail message.

Example:

```java
catch (Exception e) {

    System.out.println(
        e.getMessage()
    );
}
```

---

# 47. What is `getCause()`?

Returns the cause of an exception.

Example:

```java
Throwable cause = e.getCause();
```

It is especially useful with exception chaining.

---

# 48. What is `printStackTrace()`?

It prints information about the exception and its stack trace.

```java
catch (Exception e) {

    e.printStackTrace();

}
```

The stack trace helps identify where the exception originated and how execution reached that point.

In production applications, a logging framework is generally preferred over direct console printing.

---

# 49. Can we throw a null exception?

Technically:

```java
throw null;
```

results in:

```text
NullPointerException
```

because the expression after `throw` must evaluate to a `Throwable` object, and `null` does not reference an actual exception object.

This is mostly an interview/trick question and should not be used in real code.

---

# 50. Can we throw an object that is not a Throwable?

No.

This is invalid:

```java
throw new Student();
```

unless:

```java
Student
```

extends a class implementing `Throwable`, which normal application classes should not do.

The expression after `throw` must represent a `Throwable`.

---

# 51. Can we catch an object that is not a Throwable?

No.

Invalid:

```java
catch (Student e) {

}
```

The catch parameter must be a subtype of:

```java
Throwable
```

---

# 52. Can we catch `Exception` and `Error` separately?

Yes.

```java
try {

}
catch (Exception e) {

}
catch (Error e) {

}
```

They are siblings under:

```text
Throwable
├── Exception
└── Error
```

---

# 53. Can we catch `Throwable` instead?

Yes:

```java
catch (Throwable t) {

}
```

But broad catching of `Throwable` is usually inappropriate because it catches both `Exception` and `Error`.

---

# 54. What happens if an exception occurs in a catch block?

Example:

```java
try {

    throw new Exception();

}
catch (Exception e) {

    int x = 10 / 0;

}
```

The new `ArithmeticException` is not automatically handled by another catch block belonging to the same try statement.

It can propagate outward to a surrounding caller/handler.

---

# 55. What happens if an exception occurs in finally?

Example:

```java
try {

    System.out.println("Try");

}
finally {

    int x = 10 / 0;

}
```

The exception from `finally` propagates outward unless handled elsewhere.

---

# 56. Can constructors throw exceptions?

Yes.

```java
class Student {

    Student(int age)
            throws Exception {

        if (age < 18) {

            throw new Exception(
                "Invalid age"
            );
        }
    }
}
```

Constructors follow normal exception rules.

---

# 57. Can a static method throw an exception?

Yes.

```java
static void test()
        throws IOException {

    throw new IOException();

}
```

Exception handling is not related to whether a method is static or instance.

---

# 58. Can an abstract method declare exceptions?

Yes.

```java
abstract class Parent {

    abstract void test()
            throws IOException;
}
```

A subclass must follow the rules for overriding checked exceptions.

---

# 59. Can an overriding method throw a broader checked exception?

No.

Example:

```java
class Parent {

    void test()
            throws IOException {
    }
}
```

This is not allowed:

```java
class Child extends Parent {

    @Override
    void test()
            throws Exception {
    }
}
```

because:

```text
Exception
```

is broader than:

```text
IOException
```

---

# 60. Can an overriding method throw a narrower checked exception?

Yes.

```java
class Parent {

    void test()
            throws IOException {
    }
}
```

Child:

```java
class Child extends Parent {

    @Override
    void test()
            throws FileNotFoundException {
    }
}
```

This is allowed because:

```text
FileNotFoundException
        ↓
IOException
```

---

# 61. Can an overriding method throw an unchecked exception?

Yes.

An overriding method may throw unchecked exceptions without the checked-exception restrictions that apply to checked exceptions.

Example:

```java
class Parent {

    void test() {
    }
}

class Child extends Parent {

    @Override
    void test()
            throws RuntimeException {
    }
}
```

This is allowed.

---

# 62. Can an overriding method throw a checked exception if the parent method doesn't?

No.

Example:

```java
class Parent {

    void test() {
    }
}
```

This is invalid:

```java
class Child extends Parent {

    @Override
    void test()
            throws IOException {
    }
}
```

because the parent method did not declare that checked exception.

---

# 63. What happens when a checked exception is not handled?

Compilation fails.

Example:

```java
void test() {

    throw new IOException();

}
```

The compiler requires the checked exception to be handled or declared.

---

# 64. What happens when an unchecked exception isn't handled?

The code compiles.

Example:

```java
void test() {

    throw new RuntimeException();

}
```

If nobody catches it during execution, it propagates and may terminate the current thread.

---

# 65. Is `NullPointerException` checked or unchecked?

Unchecked.

Hierarchy:

```text
NullPointerException
        ↓
RuntimeException
        ↓
Exception
        ↓
Throwable
```

---

# 66. Is `IOException` checked or unchecked?

Checked.

Hierarchy:

```text
IOException
    ↓
Exception
    ↓
Throwable
```

It does not extend `RuntimeException`.

---

# 67. Is `ArithmeticException` checked or unchecked?

Unchecked.

```text
ArithmeticException
       ↓
RuntimeException
       ↓
Exception
```

---

# 68. Can we use `finally` with `return`?

Yes, but be careful.

Example:

```java
static int test() {

    try {

        return 10;

    }
    finally {

        System.out.println("Cleanup");

    }
}
```

The output occurs before returning `10`.

---

# 69. Why should we avoid `return` in finally?

Because:

```java
finally {
    return 20;
}
```

can override:

```java
try {
    return 10;
}
```

and can also suppress an exception that would otherwise propagate.

Therefore, returning from `finally` is generally considered bad practice.

---

# 70. What is the difference between exception handling and exception prevention?

Exception handling:

```text
Exception occurs
       ↓
Handle it
```

Prevention:

```text
Validate condition
       ↓
Avoid invalid operation
```

Example:

Instead of blindly doing:

```java
int result = a / b;
```

we can validate:

```java
if (b == 0) {

    System.out.println(
        "Cannot divide by zero"
    );

}
else {

    int result = a / b;

}
```

Both concepts are useful, but they solve different problems.

---

# 71. Should we catch every exception using `Exception`?

Not necessarily.

Instead of:

```java
catch (Exception e) {

}
```

prefer a more specific exception when you know what you can handle:

```java
catch (IOException e) {

}
```

Specific handling usually makes the code clearer and safer.

---

# 72. Should catch blocks be empty?

Generally no.

Avoid:

```java
catch (Exception e) {

}
```

This silently ignores failures.

If you intentionally ignore an exception, the reason should be clear and justified.

---

# 73. What is a resource leak?

A resource leak occurs when a resource is acquired but not properly released.

Examples:

```text
File not closed
Database connection not closed
Socket not closed
Stream not closed
```

Potential consequences include:

```text
Memory/resource pressure
Too many open files
Connection pool exhaustion
Application instability
```

Try-with-resources helps prevent many such problems.

---

# 74. Why is exception handling important in backend development?

Backend applications interact with:

```text
Database
Files
Network
APIs
Authentication
User input
External services
```

Failures can happen at every layer.

A well-designed exception strategy helps separate:

```text
Business logic
      ↓
Exception creation
      ↓
Exception propagation
      ↓
Centralized handling
      ↓
Client response / logging
```

This becomes especially important in Spring Boot.

---

# 75. Exception Handling Flow in Backend

A typical conceptual flow:

```text
Controller
    ↓
Service
    ↓
Repository
    ↓
Database
    ↓
Exception
    ↑
Repository
    ↑
Service
    ↓
Global Exception Handler
    ↓
HTTP Response
```

For example:

```text
User not found
      ↓
UserNotFoundException
      ↓
Global handler
      ↓
404 response
```

The exact implementation depends on the framework and application architecture.

---

# 76. Coding Question: What Is the Output?

```java
public class Main {

    public static void main(String[] args) {

        try {

            System.out.println("A");
            int x = 10 / 0;
            System.out.println("B");

        }
        catch (ArithmeticException e) {

            System.out.println("C");

        }
        finally {

            System.out.println("D");

        }
    }
}
```

Output:

```text
A
C
D
```

Why?

```text
A
 ↓
exception at 10 / 0
 ↓
B skipped
 ↓
catch executes
 ↓
finally executes
```

---

# 77. Coding Question: What Is the Output?

```java
try {

    System.out.println("A");

}
catch (Exception e) {

    System.out.println("B");

}
finally {

    System.out.println("C");

}
```

Output:

```text
A
C
```

No exception occurred, so `catch` is skipped.

---

# 78. Coding Question: What Is the Output?

```java
static int test() {

    try {

        return 10;

    }
    finally {

        System.out.println("Finally");

    }
}
```

Output:

```text
Finally
```

Returned value:

```text
10
```

---

# 79. Coding Question: What Is the Output?

```java
static int test() {

    try {

        return 10;

    }
    finally {

        return 20;

    }
}
```

Returned value:

```text
20
```

The `finally` return overrides the earlier return.

---

# 80. Coding Question: What Is the Output?

```java
try {

    int x = 10 / 0;

}
catch (Exception e) {

    System.out.println(
        "Exception"
    );

}
catch (ArithmeticException e) {

    System.out.println(
        "Arithmetic"
    );

}
```

Answer:

```text
Does not compile.
```

Reason:

```text
Exception
   ↓
already catches ArithmeticException
```

The second catch is unreachable.

---

# 81. Coding Question: What Is the Output?

```java
try {

    int[] arr = new int[3];

    System.out.println(arr[5]);

}
catch (ArrayIndexOutOfBoundsException e) {

    System.out.println("Array");

}
catch (RuntimeException e) {

    System.out.println("Runtime");

}
```

Output:

```text
Array
```

Because the first matching catch executes.

---

# 82. Coding Question: What Is the Output?

```java
try {

    String s = null;

    System.out.println(s.length());

}
catch (RuntimeException e) {

    System.out.println("Runtime");

}
catch (Exception e) {

    System.out.println("Exception");

}
```

Output:

```text
Runtime
```

Because:

```text
NullPointerException
       ↓
RuntimeException
```

---

# 83. Coding Question: What Is the Output?

```java
try {

    System.out.println("Try");

}
catch (Exception e) {

    System.out.println("Catch");

}
finally {

    System.out.println("Finally");

}
```

Output:

```text
Try
Finally
```

---

# 84. Coding Question: What Is the Output?

```java
try {

    throw new Exception();

}
catch (Exception e) {

    System.out.println("Catch");

}
finally {

    System.out.println("Finally");

}
```

Output:

```text
Catch
Finally
```

---

# 85. Coding Question: Can This Compile?

```java
try {

}
catch (IOException e) {

}
```

If the try block cannot throw a checked `IOException` in a context where the compiler knows it is impossible, this can produce an unreachable-catch compilation error.

The compiler checks checked-exception reachability.

---

# 86. Coding Question: Can This Compile?

```java
try {

}
catch (RuntimeException e) {

}
```

Yes.

Unchecked exceptions do not have the same compile-time reachability requirement as checked exceptions.

---

# 87. Coding Question: What Is the Output?

```java
static void test() {

    try {

        throw new RuntimeException(
            "Error"
        );

    }
    catch (RuntimeException e) {

        System.out.println(
            e.getMessage()
        );
    }
}
```

Output:

```text
Error
```

---

# 88. Coding Question: What Happens Here?

```java
static void test()
        throws Exception {

    throw new Exception(
        "Error"
    );
}
```

The method does not handle the checked exception.

Instead, it declares:

```java
throws Exception
```

Therefore, the caller is responsible for handling or further declaring it.

---

# 89. What Is the Difference Between `getMessage()` and `printStackTrace()`?

### `getMessage()`

Returns the exception's message.

```java
e.getMessage();
```

### `printStackTrace()`

Prints the exception and its stack trace.

```java
e.printStackTrace();
```

Example:

```text
getMessage()
→ "File not found"

printStackTrace()
→ exception type
→ message
→ stack trace
```

---

# 90. What Is the Difference Between `throw` and `throws`?

| `throw`                             | `throws`                             |
| ----------------------------------- | ------------------------------------ |
| Used to actually throw an exception | Used to declare possible exceptions  |
| Used inside method/block            | Used in method/constructor signature |
| Followed by an exception object     | Followed by exception type(s)        |
| Throws one exception at a time      | Can declare multiple exceptions      |

Example:

```java
throw new IOException();
```

```java
void test()
        throws IOException {
}
```

---

# 91. What Is the Difference Between `final` and `finally`?

| `final`                                               | `finally`                |
| ----------------------------------------------------- | ------------------------ |
| Keyword                                               | Exception-handling block |
| Used with variable/class/method                       | Used with try-catch      |
| Prevents certain modifications/inheritance/overriding | Used for cleanup code    |

---

# 92. What Is the Difference Between `finally` and Try-with-Resources?

### `finally`

You manually manage resource cleanup.

```java
finally {

    resource.close();

}
```

### Try-with-resources

Java automatically manages the resource.

```java
try (Resource resource =
        new Resource()) {

}
```

Try-with-resources also provides special handling for suppressed exceptions.

---

# 93. Can We Create Our Own Exception Hierarchy?

Yes.

Example:

```text
ApplicationException
       │
       ├── UserException
       │     ├── UserNotFoundException
       │     └── InvalidUserException
       │
       └── OrderException
             ├── OrderNotFoundException
             └── PaymentException
```

This can be useful in larger applications.

---

# 94. Best Practices for Exception Handling

### 1. Catch specific exceptions

Prefer:

```java
catch (IOException e)
```

over unnecessarily broad:

```java
catch (Exception e)
```

when you know what you can handle.

---

### 2. Don't silently ignore exceptions

Avoid:

```java
catch (Exception e) {
}
```

---

### 3. Don't use exceptions for normal control flow

Exceptions should represent exceptional/error situations.

---

### 4. Preserve the original cause

Use exception chaining:

```java
throw new CustomException(
    "Operation failed",
    e
);
```

---

### 5. Use meaningful custom exceptions

Examples:

```text
UserNotFoundException
PaymentFailedException
InsufficientBalanceException
```

---

### 6. Use try-with-resources for resources

```java
try (Resource r = new Resource()) {

}
```

---

### 7. Don't expose sensitive internal details

Log detailed technical information internally while returning an appropriate external error message.

---

# 95. Most Important Interview Traps

Remember these:

```text
1. Exception hierarchy starts at Throwable.

2. Error and Exception are siblings under Throwable.

3. RuntimeException → unchecked.

4. IOException → checked.

5. throw → actually throws.

6. throws → declares.

7. finally normally executes, but not literally in
   every possible JVM termination scenario.

8. More specific catch must come before general catch.

9. Multiple resources close in reverse order.

10. Try-with-resources requires AutoCloseable-compatible resources.

11. try + close exceptions:
    try exception → primary
    close exception → suppressed.

12. Custom exception can be checked or unchecked.

13. finally return can override try return.

14. Avoid return/throw from finally.

15. Overriding methods cannot throw broader checked
    exceptions than allowed by the parent method.

16. Unchecked exceptions don't have the same
    compile-time declaration restrictions.
```

---

# 96. 30-Second Interview Answer — Exception Handling

> **Java exception handling is a mechanism for managing abnormal conditions during program execution. The root class is `Throwable`, which has `Error` and `Exception` as major branches. Exceptions can be checked or unchecked. Java provides `try`, `catch`, `finally`, `throw`, and `throws` for handling and propagating exceptions. For resource management, Java provides try-with-resources using `AutoCloseable`. We can also create custom exceptions by extending `Exception` or `RuntimeException` when application-specific error types are needed.**

---

# 97. Exception Handling Cheat Sheet

```text
                    Throwable
                       │
             ┌─────────┴─────────┐
             │                   │
           Error              Exception
                                 │
                    ┌────────────┴────────────┐
                    │                         │
             RuntimeException          Other Exceptions
                    │                         │
                Unchecked                  Checked
```

### Keywords

```text
try
→ risky code

catch
→ handle exception

finally
→ cleanup code

throw
→ actually throw exception

throws
→ declare exception
```

### Resource management

```text
try-with-resources
        ↓
AutoCloseable
        ↓
automatic close()
```

### Custom exception

```java
class UserNotFoundException
        extends RuntimeException {

    public UserNotFoundException(
            String message) {

        super(message);
    }
}
```

### Exception chaining

```java
throw new CustomException(
    "Operation failed",
    originalException
);
```

### Suppressed exceptions

```java
e.getSuppressed();
```

---

# 98. Final Interview Revision Map

```text
EXCEPTION HANDLING
│
├── Throwable
│   ├── Error
│   └── Exception
│       ├── RuntimeException
│       └── Checked Exceptions
│
├── try
│
├── catch
│   ├── multiple catch
│   ├── catch ordering
│   └── multi-catch
│
├── finally
│   ├── cleanup
│   ├── return interaction
│   └── exception interaction
│
├── throw
│
├── throws
│
├── Propagation
│   └── Stack Unwinding
│
├── Try-with-Resources
│   ├── AutoCloseable
│   ├── automatic close
│   ├── multiple resources
│   ├── reverse closing
│   └── suppressed exceptions
│
└── Custom Exceptions
    ├── extends Exception
    ├── extends RuntimeException
    ├── custom fields
    ├── constructors
    └── exception chaining
```

# 99. One-Minute Memory Trick

```text
THROW
→ Do it

THROWS
→ Declare it

TRY
→ Risky code

CATCH
→ Handle it

FINALLY
→ Cleanup

AUTOCLOSEABLE
→ Auto cleanup

CUSTOM EXCEPTION
→ Your own meaningful error type

PROPAGATION
→ Exception moves up the call stack

STACK UNWINDING
→ Java searches callers for a handler

SUPPRESSED
→ Extra exception from resource closing
```

## Exception Handling Section Complete

```text
03-Exception-Handling/
│
├── 01-Exception-Introduction.md
├── 02-Exception-Hierarchy.md
├── 03-Checked-vs-Unchecked.md
├── 04-try-catch.md
├── 05-finally.md
├── 06-throw-and-throws.md
├── 07-Custom-Exceptions.md
├── 08-Try-with-Resources.md
└── 09-Exception-Interview-Questions.md
```

**You now have the complete Exception Handling section from fundamentals → JVM flow → practical usage → custom exceptions → resource management → interview preparation.**
