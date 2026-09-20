# try-catch in Java

## 1. Introduction

`try-catch` is Java's primary mechanism for **handling exceptions**.

It allows us to:

1. Put potentially risky code inside a `try` block.
2. Detect an exception if it occurs.
3. Handle that exception inside a `catch` block.
4. Continue program execution when appropriate.

Basic structure:

```java
try {
    // code that may throw an exception
}
catch (ExceptionType e) {
    // exception handling code
}
```

Example:

```java
try {
    int result = 10 / 0;
}
catch (ArithmeticException e) {
    System.out.println("Cannot divide by zero.");
}
```

Output:

```text
Cannot divide by zero.
```

---

# 2. Why Do We Need try-catch?

Without exception handling:

```java
public class Main {

    public static void main(String[] args) {

        System.out.println("Start");

        int result = 10 / 0;

        System.out.println("End");
    }
}
```

Output:

```text
Start
Exception in thread "main" java.lang.ArithmeticException: / by zero
```

`End` is never printed.

With `try-catch`:

```java
public class Main {

    public static void main(String[] args) {

        System.out.println("Start");

        try {
            int result = 10 / 0;
        }
        catch (ArithmeticException e) {
            System.out.println("Cannot divide by zero.");
        }

        System.out.println("End");
    }
}
```

Output:

```text
Start
Cannot divide by zero.
End
```

The exception was handled and normal execution continued after the `try-catch` construct.

---

# 3. Basic Structure

```java
try {
    // risky code
}
catch (ExceptionType e) {
    // handling code
}
```

Example:

```java
try {
    int number = Integer.parseInt("abc");
}
catch (NumberFormatException e) {
    System.out.println("Invalid number.");
}
```

Here:

```text
try
 ↓
Integer.parseInt("abc")
 ↓
NumberFormatException
 ↓
catch
 ↓
"Invalid number."
```

---

# 4. What is the `try` Block?

The `try` block contains code that may throw an exception.

Example:

```java
try {
    int result = 10 / 0;
}
```

The `try` block does **not** mean:

> "This code will definitely throw an exception."

It means:

> "This code may throw an exception, and if it does, Java should look for an appropriate handler."

---

# 5. What is the `catch` Block?

A `catch` block defines how to handle an exception thrown from the associated `try` block.

Example:

```java
try {
    int result = 10 / 0;
}
catch (ArithmeticException e) {
    System.out.println("Division by zero is not allowed.");
}
```

Here:

```java
ArithmeticException
```

is the type of exception the `catch` block can handle.

And:

```java
e
```

is a reference to the exception object.

---

# 6. Flow of try-catch

The basic execution flow is:

```text
              try
               ↓
        Execute statements
               ↓
        Exception occurs?
          /          \
        NO            YES
        ↓              ↓
  try completes    Find matching catch
        ↓              ↓
  skip catch       catch executes
        \              /
         \            /
          ↓          ↓
       Continue after try-catch
```

---

# 7. What Happens When No Exception Occurs?

Example:

```java
try {
    int a = 10;
    int b = 2;

    int result = a / b;

    System.out.println(result);
}
catch (ArithmeticException e) {
    System.out.println("Error");
}

System.out.println("Done");
```

Output:

```text
5
Done
```

The `catch` block does not execute.

Flow:

```text
try
 ↓
No exception
 ↓
try completes normally
 ↓
catch skipped
 ↓
continue after try-catch
```

---

# 8. What Happens When an Exception Occurs?

Example:

```java
try {
    System.out.println("A");

    int result = 10 / 0;

    System.out.println("B");
}
catch (ArithmeticException e) {
    System.out.println("C");
}

System.out.println("D");
```

Output:

```text
A
C
D
```

Why isn't `B` printed?

Because once:

```java
int result = 10 / 0;
```

throws the exception, execution immediately leaves the remaining part of the `try` block.

So:

```java
System.out.println("B");
```

is skipped.

Flow:

```text
A
↓
Exception
↓
Skip remaining try statements
↓
catch
↓
C
↓
D
```

---

# 9. Important Rule: Remaining try Statements Are Skipped

Consider:

```java
try {
    statement1();
    statement2();
    statement3();
}
catch (Exception e) {
    statement4();
}

statement5();
```

If `statement2()` throws an exception:

```text
statement1()
     ↓
statement2()
     ↓
Exception
     ↓
statement3()  ← skipped
     ↓
catch
     ↓
statement4()
     ↓
statement5()
```

This is one of the most important execution-flow rules.

---

# 10. What If the Exception Type Doesn't Match?

Example:

```java
try {
    int result = 10 / 0;
}
catch (NullPointerException e) {
    System.out.println("Null problem");
}
```

The actual exception is:

```text
ArithmeticException
```

but the handler expects:

```text
NullPointerException
```

Therefore, this `catch` does not handle the exception.

The exception continues propagating to the caller.

If no appropriate handler is found, the thread terminates through the uncaught-exception mechanism.

---

# 11. Matching Based on Inheritance

Exception matching follows Java's inheritance rules.

Consider:

```text
ArithmeticException
       ↓
RuntimeException
       ↓
Exception
       ↓
Throwable
```

Therefore:

```java
catch (ArithmeticException e)
```

can handle:

```text
ArithmeticException
```

And:

```java
catch (RuntimeException e)
```

can also handle:

```text
ArithmeticException
```

And:

```java
catch (Exception e)
```

can also handle:

```text
ArithmeticException
```

because `Exception` is its ancestor.

---

# 12. Specific Catch vs General Catch

Specific:

```java
catch (ArithmeticException e) {
    System.out.println("Arithmetic problem");
}
```

General:

```java
catch (Exception e) {
    System.out.println("Some exception occurred");
}
```

The specific handler gives more precise control.

The general handler can handle a broader range of exceptions.

---

# 13. Multiple catch Blocks

Java allows multiple `catch` blocks for one `try`.

Example:

```java
try {

    int number = Integer.parseInt("abc");

}
catch (NumberFormatException e) {

    System.out.println("Invalid number.");

}
catch (ArithmeticException e) {

    System.out.println("Arithmetic problem.");

}
catch (Exception e) {

    System.out.println("Some other exception.");

}
```

Only the first matching `catch` block is executed.

---

# 14. Why Do We Need Multiple catch Blocks?

Different exceptions may require different handling.

Example:

```text
NumberFormatException
        ↓
Tell user to enter a valid number

IOException
        ↓
Tell user file could not be read

SQLException
        ↓
Handle database problem

Other Exception
        ↓
Generic handling
```

This makes error handling more meaningful.

---

# 15. Catch Order Rule

When using multiple `catch` blocks:

> **The more specific exception must come before the more general exception.**

Correct:

```java
try {
    int x = 10 / 0;
}
catch (ArithmeticException e) {
    System.out.println("Arithmetic");
}
catch (RuntimeException e) {
    System.out.println("Runtime");
}
catch (Exception e) {
    System.out.println("Exception");
}
```

Hierarchy:

```text
ArithmeticException
       ↓
RuntimeException
       ↓
Exception
```

Therefore:

```text
Specific
   ↓
General
```

---

# 16. Incorrect Catch Order

This is invalid:

```java
try {
    int x = 10 / 0;
}
catch (Exception e) {
    System.out.println("Exception");
}
catch (ArithmeticException e) {
    System.out.println("Arithmetic");
}
```

Why?

Because:

```java
catch (Exception e)
```

can already catch `ArithmeticException`.

Therefore the second block can never be reached.

The compiler reports an error because the later `catch` is unreachable.

---

# 17. Can We Have Only try?

No.

This is invalid:

```java
try {
    // code
}
```

A `try` must be followed by at least one:

```text
catch
```

or:

```text
finally
```

Valid:

```java
try {
}
catch (Exception e) {
}
```

Valid:

```java
try {
}
finally {
}
```

Valid:

```java
try {
}
catch (Exception e) {
}
finally {
}
```

---

# 18. Can We Have Multiple catch Blocks?

Yes.

```java
try {
    // code
}
catch (ArithmeticException e) {
}
catch (NullPointerException e) {
}
catch (Exception e) {
}
```

But only one matching `catch` executes for a particular exception occurrence.

---

# 19. Can We Have Multiple try Blocks?

Yes.

There is no restriction preventing multiple independent `try-catch` structures.

```java
try {
    // operation 1
}
catch (Exception e) {
}

try {
    // operation 2
}
catch (Exception e) {
}
```

Each `try-catch` handles its own associated code.

---

# 20. Nested try-catch

A `try` block can contain another `try-catch`.

Example:

```java
try {

    System.out.println("Outer try");

    try {

        int x = 10 / 0;

    }
    catch (ArithmeticException e) {

        System.out.println("Inner catch");

    }

}
catch (Exception e) {

    System.out.println("Outer catch");

}
```

Output:

```text
Outer try
Inner catch
```

The inner `catch` handled the exception, so it did not propagate to the outer `catch`.

---

# 21. Nested try-catch When Inner Catch Doesn't Match

Example:

```java
try {

    try {

        int x = 10 / 0;

    }
    catch (NullPointerException e) {

        System.out.println("Inner catch");

    }

}
catch (ArithmeticException e) {

    System.out.println("Outer catch");

}
```

The inner handler expects:

```text
NullPointerException
```

but the actual exception is:

```text
ArithmeticException
```

So the inner `catch` does not handle it.

The exception propagates outward:

```text
Inner try
   ↓
ArithmeticException
   ↓
Inner catch doesn't match
   ↓
Outer catch matches
```

Output:

```text
Outer catch
```

---

# 22. Exception Object Inside catch

The variable in:

```java
catch (ArithmeticException e)
```

is a reference to the thrown exception object.

You can use it to obtain information.

```java
try {
    int x = 10 / 0;
}
catch (ArithmeticException e) {

    System.out.println(e.getMessage());
    System.out.println(e.getClass());

}
```

Possible output:

```text
/ by zero
class java.lang.ArithmeticException
```

---

# 23. `printStackTrace()`

One of the most useful methods for debugging is:

```java
e.printStackTrace();
```

Example:

```java
try {
    int x = 10 / 0;
}
catch (ArithmeticException e) {
    e.printStackTrace();
}
```

It prints information including:

```text
Exception type
Message
Location
Call stack
```

Example:

```text
java.lang.ArithmeticException: / by zero
    at Main.main(Main.java:5)
```

The exact line numbers depend on your source file.

---

# 24. `getMessage()`

```java
catch (Exception e) {
    System.out.println(e.getMessage());
}
```

Returns the exception's message.

Example:

```text
/ by zero
```

It may return `null` if the exception has no message.

---

# 25. `toString()`

```java
catch (Exception e) {
    System.out.println(e.toString());
}
```

Typically gives the exception class name along with its message.

Example:

```text
java.lang.ArithmeticException: / by zero
```

---

# 26. `getClass()`

```java
catch (Exception e) {
    System.out.println(e.getClass());
}
```

Possible output:

```text
class java.lang.ArithmeticException
```

This tells you the runtime class of the exception object.

---

# 27. Multiple Exceptions With One catch

Sometimes several exception types require the same handling logic.

Java supports **multi-catch**.

Syntax:

```java
try {
    // code
}
catch (IOException | SQLException e) {
    System.out.println("Operation failed.");
}
```

This was introduced in **Java 7**.

Here one `catch` handles either:

```text
IOException
```

or:

```text
SQLException
```

---

# 28. Multi-catch Rules

The exception types in a multi-catch cannot have a parent-child relationship.

Invalid:

```java
catch (Exception | IOException e) {
}
```

Why?

Because:

```text
IOException
   ↓
Exception
```

`Exception` already covers `IOException`.

So Java does not allow both in the same multi-catch.

Another invalid example:

```java
catch (RuntimeException | NullPointerException e) {
}
```

because:

```text
NullPointerException
       ↓
RuntimeException
```

---

# 29. Multi-catch Variable

In:

```java
catch (IOException | SQLException e) {
}
```

the variable:

```java
e
```

can represent either exception type.

But you cannot assume methods or properties that are specific to only one of those exception types unless they are available through their common type.

---

# 30. try-catch With Checked Exceptions

Example:

```java
import java.io.FileReader;
import java.io.FileNotFoundException;

public class Main {

    public static void main(String[] args) {

        try {

            FileReader reader =
                new FileReader("data.txt");

        }
        catch (FileNotFoundException e) {

            System.out.println("File not found.");

        }
    }
}
```

This is especially important because `FileNotFoundException` is checked.

The compiler requires us to either:

```text
handle
```

or:

```text
declare
```

the exception.

Here we handle it with `catch`.

---

# 31. try-catch With Unchecked Exceptions

Example:

```java
public class Main {

    public static void main(String[] args) {

        try {

            int result = 10 / 0;

        }
        catch (ArithmeticException e) {

            System.out.println("Cannot divide by zero.");

        }
    }
}
```

This is also completely valid.

But unlike a checked exception, the compiler does not force us to write this `catch`.

---

# 32. Does catch Fix the Problem?

Not automatically.

Consider:

```java
try {
    int result = 10 / 0;
}
catch (ArithmeticException e) {
    System.out.println("Error occurred.");
}
```

The `catch` block doesn't magically make:

```text
10 / 0
```

valid.

The operation failed.

The `catch` block simply provides a response to that failure.

---

# 33. Can We Throw Another Exception From catch?

Yes.

Example:

```java
try {

    int x = 10 / 0;

}
catch (ArithmeticException e) {

    throw new IllegalStateException(
        "Calculation failed",
        e
    );

}
```

Now the `catch` block handles the original exception by creating and throwing another exception.

This is often used for exception translation/wrapping.

---

# 34. Can catch Contain return?

Yes.

Example:

```java
static int divide(int a, int b) {

    try {
        return a / b;
    }
    catch (ArithmeticException e) {
        return 0;
    }
}
```

If division by zero occurs, the `catch` returns `0`.

However, be careful with `return` in `finally`, because it can override earlier returns. That behavior belongs to the `finally` topic.

---

# 35. Can We Put Multiple Statements in try?

Yes.

```java
try {

    int a = 10;
    int b = 2;

    int result = a / b;

    System.out.println(result);

}
catch (ArithmeticException e) {

    System.out.println("Arithmetic problem.");

}
```

The `try` block can contain multiple statements.

---

# 36. Scope of Variables in try-catch

A variable declared inside the `try` block is generally local to that block.

Example:

```java
try {
    int result = 10 / 2;
}
catch (Exception e) {
}

System.out.println(result); // ERROR
```

`result` is not accessible outside the `try` block.

If you need it outside:

```java
int result = 0;

try {
    result = 10 / 2;
}
catch (Exception e) {
    result = -1;
}

System.out.println(result);
```

---

# 37. Important Execution Rule

Consider:

```java
try {
    statement1();
    statement2();
    statement3();
}
catch (Exception e) {
    statement4();
}

statement5();
```

If:

```text
statement1 → succeeds
statement2 → throws exception
```

then:

```text
statement1  → executes
statement2  → executes and fails
statement3  → skipped
statement4  → executes
statement5  → executes
```

This is the fundamental `try-catch` execution model.

---

# 38. Does catch Execute Immediately?

Yes, once a matching exception is thrown from the associated `try` block, control transfers to the matching `catch` block.

Example:

```java
try {

    System.out.println("A");

    int x = 10 / 0;

    System.out.println("B");

}
catch (ArithmeticException e) {

    System.out.println("C");

}
```

Output:

```text
A
C
```

---

# 39. Can One try Have Multiple catch Blocks That Execute?

No.

For one exception occurrence, Java selects the first compatible `catch` block.

Example:

```java
try {
    int x = 10 / 0;
}
catch (ArithmeticException e) {
    System.out.println("A");
}
catch (Exception e) {
    System.out.println("B");
}
```

Output:

```text
A
```

`B` is not executed.

---

# 40. Exception Propagation From try-catch

Suppose:

```java
static void test() {

    try {
        int x = 10 / 0;
    }
    catch (NullPointerException e) {
        System.out.println("Handled");
    }
}
```

The actual exception is:

```text
ArithmeticException
```

The catch doesn't match.

Therefore, the exception leaves `test()` and propagates to its caller.

This demonstrates:

> `try-catch` only handles exceptions for which a compatible handler exists.

---

# 41. try-catch Does Not Catch Compilation Errors

Consider:

```java
int x = ;
```

This is a syntax error.

The program does not compile.

You cannot use:

```java
try {
    int x = ;
}
catch (Exception e) {
}
```

to handle a compilation error.

`try-catch` handles exceptions that occur during execution, not Java syntax errors detected by the compiler.

---

# 42. try-catch Does Not Catch Every Problem

Example:

```java
try {
    int[] arr = new int[Integer.MAX_VALUE];
}
catch (Exception e) {
    System.out.println("Exception");
}
```

A serious `Error` such as `OutOfMemoryError` is not an `Exception`.

Remember:

```text
Throwable
├── Error
└── Exception
```

A `catch (Exception e)` does not catch `Error`.

---

# 43. `catch (Throwable t)`

Technically:

```java
try {
    // code
}
catch (Throwable t) {
    // ...
}
```

can catch both:

```text
Exception
Error
```

because both extend `Throwable`.

However, catching `Throwable` is generally too broad for ordinary application code.

You usually want to catch the specific exception you can meaningfully handle.

---

# 44. Good Exception Handling

Prefer:

```java
try {
    readFile();
}
catch (IOException e) {
    System.out.println("Unable to read file.");
}
```

over:

```java
try {
    readFile();
}
catch (Exception e) {
    // everything
}
```

Specific handling makes the program easier to understand and debug.

---

# 45. Bad Practice: Empty catch

Avoid:

```java
try {
    riskyOperation();
}
catch (Exception e) {
}
```

Why?

The exception is silently ignored.

This can make bugs extremely difficult to find.

If intentionally ignoring something is truly justified, the reason should be clear in the code/design.

---

# 46. Bad Practice: Printing Only Generic Messages

Avoid losing useful diagnostic information:

```java
catch (Exception e) {
    System.out.println("Something went wrong");
}
```

Depending on the application, useful logging or exception information may be necessary.

For debugging:

```java
catch (Exception e) {
    e.printStackTrace();
}
```

In production applications, proper logging frameworks are generally preferred over directly printing stack traces.

---

# 47. Real-World Example

Imagine a banking application:

```java
public void withdraw(double amount) {

    try {

        if (amount > balance) {
            throw new IllegalArgumentException(
                "Insufficient balance"
            );
        }

        balance -= amount;

    }
    catch (IllegalArgumentException e) {

        System.out.println(e.getMessage());

    }
}
```

The `try-catch` separates:

```text
Normal operation
      ↓
Withdrawal
```

from:

```text
Exceptional situation
      ↓
Invalid withdrawal
```

---

# 48. Real-World Backend Example

Suppose a backend reads data from a database:

```java
try {

    // database operation

}
catch (SQLException e) {

    // handle database failure

}
```

The application may:

```text
Log the failure
Return an appropriate response
Retry where appropriate
Notify another layer
Rollback a transaction
```

The correct action depends on the application architecture.

---

# 49. Important Interview Questions

## Q1. What is the purpose of try-catch?

`try-catch` provides a structured mechanism to detect and handle exceptions that occur during execution.

---

## Q2. What happens when an exception occurs inside a try block?

The remaining statements in that `try` block are skipped, and Java searches for a matching `catch` handler.

---

## Q3. What happens if no catch block matches?

The exception propagates to the caller.

If no suitable handler exists anywhere in the call chain, the uncaught-exception mechanism handles it.

---

## Q4. Can we have multiple catch blocks?

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

Only the first matching handler executes.

---

## Q5. Can catch blocks be written in any order?

No.

Specific exceptions must appear before their parent/general exception types.

---

## Q6. Can we catch RuntimeException?

Yes.

```java
try {
    int x = 10 / 0;
}
catch (RuntimeException e) {
    System.out.println("Handled");
}
```

---

## Q7. Can we catch Error?

Technically yes:

```java
try {
}
catch (Error e) {
}
```

But it is generally not appropriate to use this as normal application exception handling.

---

## Q8. Can we catch Throwable?

Technically yes:

```java
catch (Throwable t)
```

But it is usually too broad because it includes both `Exception` and `Error`.

---

## Q9. Can try exist without catch?

Yes, if it has `finally`.

```java
try {
}
finally {
}
```

But a standalone `try` without `catch` or `finally` is invalid.

---

## Q10. Can there be multiple catch blocks?

Yes.

---

## Q11. Can one catch handle multiple exception types?

Yes, using multi-catch:

```java
catch (IOException | SQLException e) {
}
```

---

## Q12. Can multi-catch contain parent and child exceptions?

No.

Invalid:

```java
catch (Exception | IOException e) {
}
```

because `IOException` is already covered by `Exception`.

---

## Q13. Does catch execute if no exception occurs?

No.

---

## Q14. Does the remaining try code execute after an exception?

No.

Once the exception is thrown, the remaining statements in that `try` block are skipped.

---

# 50. Common Interview Traps

### Trap 1

```java
try {
}
```

❌ Invalid.

Needs `catch` or `finally`.

---

### Trap 2

```java
catch (Exception e) {
}
catch (ArithmeticException e) {
}
```

❌ Invalid because the second handler is unreachable.

---

### Trap 3

```java
catch (IOException | Exception e) {
}
```

❌ Invalid because `IOException` is a subclass of `Exception`.

---

### Trap 4

Thinking that:

```java
catch (Exception e)
```

catches `Error`.

❌ It does not.

---

### Trap 5

Thinking every `catch` executes after an exception.

❌ Only the first compatible handler executes.

---

### Trap 6

Thinking the remaining `try` statements execute after the exception.

❌ They are skipped.

---

# 51. Complete Execution Example

```java
public class Main {

    public static void main(String[] args) {

        System.out.println("1");

        try {

            System.out.println("2");

            int result = 10 / 0;

            System.out.println("3");

        }
        catch (ArithmeticException e) {

            System.out.println("4");

        }

        System.out.println("5");
    }
}
```

Output:

```text
1
2
4
5
```

Why?

```text
1 → normal
2 → normal
division → exception
3 → skipped
4 → catch
5 → normal flow resumes
```

This is the execution model you should remember.

---

# 52. 30-Second Interview Answer

> **`try-catch` is Java's mechanism for handling exceptions. Code that may throw an exception is placed inside the `try` block, and a matching `catch` block handles the exception. If no exception occurs, the catch block is skipped. If an exception occurs, the remaining statements in the try block are skipped and Java searches for the first compatible catch handler. With multiple catch blocks, specific exceptions must appear before their parent types. If no matching handler exists, the exception propagates to the caller.**

---

# 53. Quick Revision

```text
try
 ↓
Potentially risky code

Exception?
 ├── NO
 │    ↓
 │  skip catch
 │    ↓
 │  continue
 │
 └── YES
      ↓
   skip remaining try statements
      ↓
   find matching catch
      ↓
   catch executes
      ↓
   continue after try-catch
```

### Core syntax

```java
try {
    // risky code
}
catch (ExceptionType e) {
    // handling
}
```

### Multiple catch

```java
try {
}
catch (SpecificException e) {
}
catch (GeneralException e) {
}
```

### Multi-catch

```java
try {
}
catch (IOException | SQLException e) {
}
```

### Remember

```text
try      → risky code
catch    → handling
specific → before general
one exception → first matching catch
exception → remaining try code skipped
no match → propagation
```

---

# 54. Final Takeaways

```text
✓ try contains code that may throw an exception.

✓ catch handles a matching exception.

✓ If no exception occurs, catch is skipped.

✓ If an exception occurs, remaining try statements are skipped.

✓ Only the first compatible catch executes.

✓ Multiple catch blocks are allowed.

✓ Specific catch blocks must come before general ones.

✓ Multi-catch allows multiple unrelated exception types.

✓ Parent and child exception types cannot be combined in multi-catch.

✓ try must be followed by catch or finally.

✓ try-catch can handle both checked and unchecked exceptions.

✓ catch (Exception e) does not catch Error.

✓ catch (Throwable t) can catch both Exception and Error,
  but is usually too broad for normal application handling.

✓ Exceptions that are not matched propagate to the caller.
```

> **Core idea:** `try-catch` does not prevent an exception from occurring. It provides a controlled path for responding to an exception when it occurs.
