# finally in Java

## 1. Introduction

`finally` is a block used with exception handling to execute code that should generally run **after the `try`/`catch` processing**, whether an exception occurs or not.

Basic structure:

```java
try {
    // risky code
}
catch (Exception e) {
    // handle exception
}
finally {
    // cleanup code
}
```

Example:

```java
try {
    int result = 10 / 2;
    System.out.println(result);
}
catch (ArithmeticException e) {
    System.out.println("Arithmetic error");
}
finally {
    System.out.println("Finally executed");
}
```

Output:

```text
5
Finally executed
```

---

# 2. Why Do We Need finally?

Some operations require cleanup regardless of whether the operation succeeds or fails.

For example:

```text
Open resource
     ↓
Perform operation
     ↓
Success / Exception
     ↓
Cleanup resource
```

Typical cleanup operations include:

```text
Closing files
Closing streams
Releasing certain resources
Releasing locks
Cleaning temporary state
```

Historically, `finally` was commonly used for resource cleanup.

For modern resource management, **try-with-resources** is usually preferred for resources that implement `AutoCloseable`.

That topic is covered separately in:

```text
08-Try-with-Resources.md
```

---

# 3. Basic Flow

The general flow is:

```text
             try
              ↓
       Exception occurs?
        /            \
      NO              YES
      ↓                ↓
   catch skipped    matching catch
      ↓                ↓
      └───────┬────────┘
              ↓
           finally
              ↓
       continue execution
```

---

# 4. finally Executes When No Exception Occurs

Example:

```java
public class Main {

    public static void main(String[] args) {

        try {
            System.out.println("Inside try");
        }
        finally {
            System.out.println("Inside finally");
        }

    }
}
```

Output:

```text
Inside try
Inside finally
```

There is no exception, but `finally` still executes.

---

# 5. finally Executes When an Exception Occurs

Example:

```java
public class Main {

    public static void main(String[] args) {

        try {
            int result = 10 / 0;
        }
        catch (ArithmeticException e) {
            System.out.println("Exception handled");
        }
        finally {
            System.out.println("Finally executed");
        }

    }
}
```

Output:

```text
Exception handled
Finally executed
```

Flow:

```text
try
 ↓
Exception
 ↓
catch
 ↓
finally
```

---

# 6. try + finally Without catch

A `catch` block is not mandatory if a `finally` block is present.

This is valid:

```java
try {
    System.out.println("Try");
}
finally {
    System.out.println("Finally");
}
```

So the valid structures include:

### try + catch

```java
try {
}
catch (Exception e) {
}
```

### try + finally

```java
try {
}
finally {
}
```

### try + catch + finally

```java
try {
}
catch (Exception e) {
}
finally {
}
```

But this is invalid:

```java
try {
}
```

A `try` must be followed by either:

```text
catch
```

or:

```text
finally
```

---

# 7. finally With Unhandled Exception

Consider:

```java
public class Main {

    public static void main(String[] args) {

        try {
            int result = 10 / 0;
        }
        finally {
            System.out.println("Finally executed");
        }
    }
}
```

Output will include:

```text
Finally executed
```

followed by the uncaught exception information.

Why?

Because `finally` executes before the exception continues propagating out of the current method.

Flow:

```text
try
 ↓
ArithmeticException
 ↓
no catch
 ↓
finally
 ↓
exception propagates
```

---

# 8. finally Does Not Handle Exceptions

This is important.

`finally` is **not** an exception handler.

Example:

```java
try {
    int result = 10 / 0;
}
finally {
    System.out.println("Cleanup");
}
```

`finally` executes, but it does not catch the `ArithmeticException`.

The exception still propagates.

So:

```text
catch   → handles exception
finally → cleanup/finalization-style block
```

---

# 9. Execution Order

Consider:

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

System.out.println("D");
```

Output:

```text
A
C
D
```

Because there is no exception.

---

# 10. With an Exception

```java
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

System.out.println("E");
```

Output:

```text
A
C
D
E
```

Why?

```text
A
↓
Exception
↓
B skipped
↓
catch → C
↓
finally → D
↓
E
```

---

# 11. Important Rule

Remember:

> **If normal control flow reaches the end of a `try`/`catch` structure, `finally` executes before continuing beyond that structure.**

This includes normal execution and ordinary exception handling paths.

---

# 12. finally and return

This is one of the **most important interview topics**.

Consider:

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

So `finally` executes before the method actually completes its return.

Flow:

```text
try
 ↓
prepare return value
 ↓
finally
 ↓
method returns
```

---

# 13. return in try + finally

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

Result:

```text
Finally
10
```

The `finally` block does not change the return value here because it does not itself return or otherwise override the control flow.

---

# 14. return in finally

Now look at this:

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

The returned value is:

```text
20
```

Why?

Because the `return` inside `finally` takes control away from the earlier return.

Conceptually:

```text
try
 ↓
return 10
 ↓
finally
 ↓
return 20
 ↓
method returns 20
```

---

# 15. Why return in finally Is Dangerous

Consider:

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

The original:

```java
return 10;
```

is effectively overridden by:

```java
return 20;
```

This can make code confusing and can suppress an exception as well.

Therefore:

> **Avoid using `return` inside `finally` in normal application code.**

---

# 16. finally Can Suppress an Exception

This is an important interview trap.

Example:

```java
static void test() {

    try {
        throw new RuntimeException("Original exception");
    }
    finally {
        return;
    }
}
```

The `return` in `finally` causes the method to return normally.

The original exception is therefore not propagated.

This is one reason `return` in `finally` is considered dangerous.

---

# 17. finally With return in catch

Example:

```java
static int test() {

    try {
        int x = 10 / 0;
        return 10;
    }
    catch (ArithmeticException e) {
        return 20;
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
20
```

Flow:

```text
try
 ↓
exception
 ↓
catch
 ↓
return 20 prepared
 ↓
finally
 ↓
method returns 20
```

---

# 18. finally With return

Now:

```java
static int test() {

    try {
        int x = 10 / 0;
        return 10;
    }
    catch (ArithmeticException e) {
        return 20;
    }
    finally {
        return 30;
    }
}
```

Returned value:

```text
30
```

Because the `finally` return overrides the earlier control flow.

---

# 19. finally With Exception in try

Example:

```java
static void test() {

    try {
        throw new RuntimeException("Problem");
    }
    finally {
        System.out.println("Cleanup");
    }
}
```

Output includes:

```text
Cleanup
```

and the exception remains unhandled and propagates afterward.

Flow:

```text
try
 ↓
exception
 ↓
finally
 ↓
exception propagates
```

---

# 20. finally Itself Can Throw an Exception

Example:

```java
try {
    System.out.println("Try");
}
finally {
    throw new RuntimeException("Finally exception");
}
```

The exception thrown from `finally` can become the exception that propagates out of the construct.

If another exception was already in progress, an exception thrown by `finally` can interfere with the original exception's propagation.

This is another reason to avoid throwing from `finally` unless there is a deliberate reason.

---

# 21. Exception in try + Exception in finally

Example:

```java
static void test() {

    try {
        throw new RuntimeException("Try exception");
    }
    finally {
        throw new RuntimeException("Finally exception");
    }
}
```

The exception from `finally` takes precedence for ordinary propagation from this construct.

The original exception is not simply propagated as the primary exception.

This can make debugging difficult.

---

# 22. return in try + Exception in finally

Example:

```java
static int test() {

    try {
        return 10;
    }
    finally {
        throw new RuntimeException("Error");
    }
}
```

The method does **not** return `10`.

The exception from `finally` prevents the normal return from completing.

Flow:

```text
try
 ↓
return 10 prepared
 ↓
finally
 ↓
exception
 ↓
method exits exceptionally
```

---

# 23. return in try + return in finally

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

Rule:

> A `return` in `finally` overrides a return from `try` or `catch`.

---

# 24. System.exit() and finally

This is a famous interview question.

Example:

```java
public static void main(String[] args) {

    try {
        System.out.println("Try");
        System.exit(0);
    }
    finally {
        System.out.println("Finally");
    }
}
```

Output:

```text
Try
```

`Finally` normally does not execute here because:

```java
System.exit(0);
```

requests termination of the JVM.

So the statement:

> "`finally` always executes."

is **not absolutely true**.

---

# 25. Does finally Always Execute?

The interview-safe answer is:

> **`finally` normally executes whenever control leaves the associated `try`/`catch` construct through ordinary Java control flow, but it is not guaranteed in every possible situation.**

Cases where it may not execute include situations such as:

```text
JVM/process termination
System.exit()
abrupt external termination
catastrophic system failure
```

Do not simply say:

> "finally always executes."

That is an interview trap.

---

# 26. `System.exit()` vs return

Compare:

### return

```java
try {
    return;
}
finally {
    System.out.println("Finally");
}
```

`finally` executes.

### System.exit()

```java
try {
    System.exit(0);
}
finally {
    System.out.println("Finally");
}
```

`finally` normally does not execute because the JVM is terminated.

---

# 27. finally and Loops

`finally` also executes when control leaves a loop through `break`, assuming normal execution continues through the enclosing `try` structure.

Example:

```java
try {

    for (int i = 0; i < 5; i++) {

        if (i == 2) {
            break;
        }

    }

}
finally {

    System.out.println("Finally");

}
```

Output:

```text
Finally
```

---

# 28. finally and continue

Similarly, `continue` does not prevent a `finally` associated with the enclosing control flow from executing when that `try` is exited.

Example:

```java
for (int i = 0; i < 3; i++) {

    try {

        if (i == 1) {
            continue;
        }

        System.out.println(i);

    }
    finally {

        System.out.println("Finally");

    }
}
```

The exact output follows the normal loop + `try`/`finally` control flow.

---

# 29. Nested finally

You can have nested `try`/`finally` structures.

Example:

```java
try {

    try {
        System.out.println("Inner try");
    }
    finally {
        System.out.println("Inner finally");
    }

}
finally {

    System.out.println("Outer finally");

}
```

Output:

```text
Inner try
Inner finally
Outer finally
```

The inner `finally` executes before the outer `finally`.

---

# 30. finally and Resource Management

Historically, developers often wrote:

```java
FileReader reader = null;

try {
    reader = new FileReader("data.txt");

    // use reader

}
catch (Exception e) {

    // handle

}
finally {

    if (reader != null) {
        reader.close();
    }

}
```

The idea was:

```text
Use resource
     ↓
success / exception
     ↓
finally
     ↓
close resource
```

This pattern is still possible.

However, Java provides a cleaner mechanism:

```text
try-with-resources
```

for resources implementing `AutoCloseable`.

That is covered in:

```text
08-Try-with-Resources.md
```

---

# 31. finally vs try-with-resources

### Traditional

```java
try {
    // use resource
}
finally {
    // manually close resource
}
```

### Modern

```java
try (FileReader reader = new FileReader("data.txt")) {
    // use resource
}
```

The resource is automatically closed.

Therefore:

```text
finally
→ general cleanup/control-flow mechanism

try-with-resources
→ preferred automatic resource management
```

---

# 32. Scope of finally

Variables declared inside `finally` are local to that block.

Example:

```java
try {
}
finally {
    int x = 10;
}

System.out.println(x); // ERROR
```

`x` is not accessible outside `finally`.

---

# 33. Empty finally

Technically:

```java
try {
}
finally {
}
```

is valid.

But an empty `finally` generally has no practical purpose.

---

# 34. Multiple finally Blocks?

One `try` statement cannot have multiple sequential `finally` blocks.

Invalid:

```java
try {
}
finally {
}
finally {
}
```

But nested structures can have multiple `finally` blocks:

```java
try {
    try {
    }
    finally {
    }
}
finally {
}
```

---

# 35. finally With Multiple catch Blocks

Valid:

```java
try {

}
catch (ArithmeticException e) {

}
catch (NullPointerException e) {

}
finally {

}
```

Execution:

```text
try
 ↓
matching catch, if needed
 ↓
finally
 ↓
continue
```

Regardless of which matching catch handles the exception, `finally` normally executes afterward.

---

# 36. finally and Exception Propagation

Consider:

```java
static void method() {

    try {
        throw new RuntimeException("Problem");
    }
    finally {
        System.out.println("Cleanup");
    }
}
```

Flow:

```text
method()
   ↓
try
   ↓
RuntimeException
   ↓
finally
   ↓
exception propagates to caller
```

`finally` does not automatically stop propagation.

---

# 37. Important Interview Question

### Q: What is the purpose of finally?

Good answer:

> `finally` is used for code that should normally execute regardless of whether an exception occurs, especially cleanup or releasing resources. It executes after the normal `try`/`catch` path before control leaves the construct.

---

# 38. Important Interview Question

### Q: Does finally execute if there is a return in try?

Yes.

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

`finally` executes before the method completes its return.

---

# 39. Important Interview Question

### Q: What happens if finally also has return?

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

---

# 40. Important Interview Question

### Q: Can finally execute without catch?

Yes.

```java
try {
    // code
}
finally {
    // cleanup
}
```

---

# 41. Important Interview Question

### Q: Can finally execute without try?

No.

`finally` must be associated with a `try`.

---

# 42. Important Interview Question

### Q: Can finally throw an exception?

Yes.

```java
try {
}
finally {
    throw new RuntimeException();
}
```

The exception can propagate out of the construct.

---

# 43. Important Interview Question

### Q: Can finally change a return value?

Yes, if it itself performs a `return`.

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

Returns:

```text
20
```

---

# 44. Important Interview Question

### Q: Does finally always execute?

Not absolutely.

Normally yes, but termination of the JVM/process or other abnormal termination can prevent it.

Classic example:

```java
System.exit(0);
```

---

# 45. Common Interview Traps

## Trap 1

> "finally always executes."

❌ Too absolute.

Better:

> `finally` normally executes, except when execution is terminated before it can run, such as `System.exit()`.

---

## Trap 2

> "`finally` handles exceptions."

❌ Incorrect.

`catch` handles exceptions.

`finally` is primarily for cleanup/control flow that should normally occur regardless of exception handling.

---

## Trap 3

> "try must always have catch."

❌ Incorrect.

This is valid:

```java
try {
}
finally {
}
```

---

## Trap 4

> "return in try means finally won't execute."

❌ Incorrect.

`finally` executes before the method completes the return.

---

## Trap 5

> "return in finally is harmless."

❌ Incorrect.

It can override earlier returns and suppress exceptions.

---

# 46. Most Important Output Questions

### Question 1

```java
static int test() {

    try {
        return 10;
    }
    finally {
        System.out.println("Hello");
    }
}
```

What happens?

```text
Hello
```

Return value:

```text
10
```

---

### Question 2

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

Return value:

```text
20
```

---

### Question 3

```java
static int test() {

    try {
        throw new RuntimeException();
    }
    finally {
        System.out.println("Finally");
    }
}
```

Result:

```text
Finally
```

Then the exception propagates because it is not handled.

---

### Question 4

```java
static int test() {

    try {
        return 10;
    }
    finally {
        throw new RuntimeException();
    }
}
```

Result:

```text
Exception
```

The method does not successfully return `10`.

---

# 47. Execution Cheat Sheet

```text
                  try
                   │
        ┌──────────┴──────────┐
        │                     │
   No exception           Exception
        │                     │
        │               Matching catch?
        │                /          \
        │              YES           NO
        │               │             │
        │             catch           │
        │               │             │
        └───────┬───────┴─────────────┘
                ↓
             finally
                ↓
        continue / return /
        propagate exception
```

---

# 48. The `return` Trap

Memorize this:

```text
try return
    ↓
finally executes
    ↓
normal return completes
```

But:

```text
try return
    ↓
finally return
    ↓
finally's return wins
```

And:

```text
try exception
    ↓
finally exception
    ↓
finally's exception affects propagation
```

---

# 49. 30-Second Interview Answer

> **`finally` is a block associated with a `try` statement that normally executes whether an exception occurs or not. It is commonly used for cleanup operations. A `try` can be followed by `catch`, `finally`, or both. `finally` executes even when `try` or `catch` contains a `return`, before the method actually completes. However, a `return` or exception inside `finally` can override earlier control flow, so returning or throwing from `finally` should generally be avoided. Also, `finally` is not absolutely guaranteed—for example, JVM termination through `System.exit()` can prevent it from executing.**

---

# 50. Quick Revision

```text
finally
────────────────────────────────────

✓ Used for cleanup/control-flow code.

✓ Normally executes whether exception occurs or not.

✓ Can be used with try without catch.

✓ try + finally is valid.

✓ try alone is invalid.

✓ Executes after matching catch.

✓ Executes before a return from try/catch completes.

✓ return in finally overrides earlier return.

✓ Exception in finally can affect/suppress original exception.

✓ System.exit() can prevent finally from executing.

✓ Modern resource management should generally use
  try-with-resources for AutoCloseable resources.

✓ finally does NOT itself handle exceptions.
```

## 🔥 One-Line Memory Trick

```text
try    → Try the risky operation
catch  → Catch the problem
finally → Finish the cleanup
```

> **Most important interview point:** `finally` normally runs even when `try`/`catch` returns, but a `return` inside `finally` can override that return and should generally be avoided.
