# Try-with-Resources in Java

## 1. Introduction

**Try-with-resources** is a Java feature used to automatically close resources after they are no longer needed.

It was introduced in **Java 7**.

Before try-with-resources, we usually had to manually close resources inside a `finally` block.

Example resources:

```text
FileInputStream
FileOutputStream
BufferedReader
BufferedWriter
Scanner
Database Connection
PreparedStatement
ResultSet
```

The main purpose is:

```text
Open resource
      ↓
Use resource
      ↓
Automatically close resource
```

---

# 2. Why Do We Need It?

Consider reading a file.

```java
FileReader reader = new FileReader("data.txt");

try {

    // read file

}
finally {

    reader.close();

}
```

We have to remember to close the resource manually.

If we forget:

```text
Resource opened
      ↓
Resource not closed
      ↓
Resource leak
```

In large applications, resource leaks can become serious problems.

Try-with-resources solves this by automatically closing the resource.

---

# 3. Basic Syntax

The syntax is:

```java
try (resource) {

    // use resource

}
catch (Exception e) {

}
```

Example:

```java
try (FileReader reader =
        new FileReader("data.txt")) {

    // use reader

}
catch (IOException e) {

    e.printStackTrace();

}
```

When execution leaves the `try` block, Java automatically closes `reader`.

---

# 4. What Is a Resource?

A resource is generally an object that represents something that needs to be closed after use.

Examples:

```text
File
 ├── FileInputStream
 ├── FileOutputStream
 ├── FileReader
 └── FileWriter

Database
 ├── Connection
 ├── Statement
 ├── PreparedStatement
 └── ResultSet
```

Other examples:

```text
BufferedReader
BufferedWriter
Scanner
Socket
```

---

# 5. `AutoCloseable`

The key concept behind try-with-resources is:

```java
AutoCloseable
```

A resource used directly in try-with-resources must implement:

```java
AutoCloseable
```

or implement an interface that extends it, such as:

```java
Closeable
```

Hierarchy:

```text
Object
   ↓
AutoCloseable
   ↑
Closeable
```

Many Java resource classes implement `Closeable` or `AutoCloseable`.

---

# 6. AutoCloseable Interface

The important method is:

```java
void close() throws Exception;
```

Conceptually:

```java
public interface AutoCloseable {

    void close() throws Exception;

}
```

When try-with-resources finishes, Java calls:

```java
close()
```

automatically.

---

# 7. Simple Example

```java
import java.io.FileReader;
import java.io.IOException;

public class Main {

    public static void main(String[] args) {

        try (FileReader reader =
                new FileReader("data.txt")) {

            System.out.println("File opened");

        }
        catch (IOException e) {

            System.out.println(e.getMessage());

        }
    }
}
```

Flow:

```text
new FileReader()
       ↓
resource opened
       ↓
try block executes
       ↓
try block finishes
       ↓
reader.close()
       ↓
resource released
```

---

# 8. The Resource Is Automatically Closed

Suppose:

```java
try (FileReader reader =
        new FileReader("data.txt")) {

    System.out.println("Reading file");

}
```

You don't need:

```java
reader.close();
```

because Java handles it automatically.

Conceptually:

```text
try
 │
 ├── use reader
 │
 └── finally
       ↓
   reader.close()
```

The exact compiler-generated implementation is more nuanced, especially when exceptions occur, but thinking of it as automatic cleanup is a useful starting point.

---

# 9. Try-with-Resources vs Normal Try-Catch

### Normal try-catch

```java
FileReader reader = null;

try {

    reader = new FileReader("data.txt");

}
catch (IOException e) {

}
finally {

    if (reader != null) {

        reader.close();

    }
}
```

We manually manage the resource.

### Try-with-resources

```java
try (FileReader reader =
        new FileReader("data.txt")) {

}
catch (IOException e) {

}
```

Much cleaner.

---

# 10. Why `finally` Was Used Before

Before Java 7:

```java
FileReader reader = null;

try {

    reader = new FileReader("data.txt");

    // work

}
finally {

    if (reader != null) {

        reader.close();

    }
}
```

The `finally` block was commonly used because it normally executes whether the operation succeeds or fails.

But this approach has more boilerplate and has tricky cases involving exceptions during cleanup.

Try-with-resources was introduced to make resource management safer and cleaner.

---

# 11. Resource Must Be Closeable

This works:

```java
try (FileReader reader =
        new FileReader("data.txt")) {

}
```

because `FileReader` supports the required resource-closing contract.

But an arbitrary class:

```java
class Student {

}
```

cannot simply be used:

```java
try (Student s = new Student()) {

}
```

because `Student` does not implement `AutoCloseable`.

---

# 12. Creating Your Own AutoCloseable Class

We can make our own class compatible with try-with-resources.

```java
class MyResource implements AutoCloseable {

    public void use() {

        System.out.println("Using resource");

    }

    @Override
    public void close() {

        System.out.println("Resource closed");

    }
}
```

Now:

```java
public class Main {

    public static void main(String[] args) {

        try (MyResource resource =
                new MyResource()) {

            resource.use();

        }
    }
}
```

Output:

```text
Using resource
Resource closed
```

Notice:

```java
resource.close();
```

was never explicitly called.

---

# 13. How It Works

The important idea is:

```text
try-with-resources
        ↓
Java knows resource implements AutoCloseable
        ↓
try block executes
        ↓
resource.close()
```

So:

```java
try (MyResource resource =
        new MyResource()) {

    resource.use();

}
```

automatically performs cleanup.

---

# 14. `close()` Is Called Even If Exception Occurs

This is one of the most important properties.

Example:

```java
try (MyResource resource =
        new MyResource()) {

    resource.use();

    throw new RuntimeException(
        "Something went wrong"
    );
}
```

Even though an exception occurs:

```text
resource.use()
      ↓
exception
      ↓
close()
```

The resource is still closed.

---

# 15. Complete Example With Exception

```java
class MyResource implements AutoCloseable {

    public void use() {

        System.out.println("Using resource");

    }

    @Override
    public void close() {

        System.out.println("Resource closed");

    }
}

public class Main {

    public static void main(String[] args) {

        try (MyResource resource =
                new MyResource()) {

            resource.use();

            throw new RuntimeException(
                "Something went wrong"
            );

        }
        catch (RuntimeException e) {

            System.out.println(
                e.getMessage()
            );

        }
    }
}
```

Output:

```text
Using resource
Resource closed
Something went wrong
```

The cleanup happens before the exception is handled by the surrounding `catch`.

---

# 16. Multiple Resources

We can declare multiple resources.

```java
try (
    FileReader reader =
        new FileReader("data.txt");

    BufferedReader br =
        new BufferedReader(reader)
) {

    System.out.println(
        br.readLine()
    );

}
```

Both resources are automatically closed.

---

# 17. Closing Order

Multiple resources are closed in the **reverse order of their declaration**.

Example:

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

Remember:

```text
First opened
     ↓
Last closed
```

This is similar to a stack.

---

# 18. Why Reverse Order?

Consider:

```java
try (
    FileInputStream input =
        new FileInputStream("data.txt");

    BufferedInputStream buffer =
        new BufferedInputStream(input)
) {

}
```

The buffered stream depends on the underlying input stream.

So Java closes:

```text
BufferedInputStream
        ↓
FileInputStream
```

This prevents the underlying resource from being closed before the wrapper that depends on it.

---

# 19. Try-with-Resources With `catch`

You can use:

```java
try (resource) {

}
catch (Exception e) {

}
```

Example:

```java
try (FileReader reader =
        new FileReader("data.txt")) {

    // use reader

}
catch (IOException e) {

    System.out.println(
        "File error"
    );

}
```

---

# 20. Try-with-Resources With `finally`

You can also use:

```java
try (resource) {

}
catch (Exception e) {

}
finally {

}
```

Example:

```java
try (MyResource resource =
        new MyResource()) {

    resource.use();

}
catch (Exception e) {

    System.out.println(
        e.getMessage()
    );

}
finally {

    System.out.println(
        "Finally executes"
    );

}
```

Resource cleanup is handled by try-with-resources.

`finally` is still available for other cleanup logic that isn't represented by the resource itself.

---

# 21. Java 7 vs Java 9 Syntax

Originally, the resource had to be declared inside the parentheses:

```java
try (
    FileReader reader =
        new FileReader("data.txt")
) {

}
```

Java 9 introduced a more flexible form where an already-created effectively final variable can be used.

Example:

```java
FileReader reader =
    new FileReader("data.txt");

try (reader) {

    // use reader

}
```

The variable must satisfy the relevant final/effectively-final requirement.

---

# 22. What Does Effectively Final Mean?

A variable is **effectively final** if its value is assigned once and is not reassigned.

Example:

```java
FileReader reader =
    new FileReader("data.txt");

try (reader) {

}
```

Here:

```text
reader
 ↓
assigned once
 ↓
never reassigned
 ↓
effectively final
```

But:

```java
FileReader reader =
    new FileReader("data.txt");

reader = anotherReader;
```

means the variable is reassigned and therefore is not effectively final.

---

# 23. Suppressed Exceptions

This is one of the most important advanced concepts.

Suppose both:

```text
try block
```

and:

```text
close()
```

throw exceptions.

Which exception should become the primary exception?

The exception from the try body generally remains the **primary exception**, while the exception thrown during closing can become a **suppressed exception**.

Example:

```java
class MyResource implements AutoCloseable {

    @Override
    public void close() {

        throw new RuntimeException(
            "Close failed"
        );

    }
}
```

Then:

```java
try (MyResource resource =
        new MyResource()) {

    throw new RuntimeException(
        "Main operation failed"
    );

}
```

The main exception is:

```text
Main operation failed
```

and the close exception can be attached as suppressed.

---

# 24. `getSuppressed()`

Suppressed exceptions can be accessed using:

```java
e.getSuppressed();
```

Example:

```java
catch (Exception e) {

    System.out.println(
        e.getMessage()
    );

    for (Throwable suppressed :
            e.getSuppressed()) {

        System.out.println(
            suppressed.getMessage()
        );
    }
}
```

This allows us to inspect exceptions that occurred during automatic resource closing.

---

# 25. Why Suppressed Exceptions Matter

Without proper handling, an exception from `close()` could hide or interfere with the original exception.

Try-with-resources preserves both pieces of information:

```text
Primary exception
       +
Suppressed exception
```

This makes debugging resource-management failures much better.

---

# 26. Example of Suppressed Exception

```java
class MyResource implements AutoCloseable {

    @Override
    public void close() {

        throw new RuntimeException(
            "Close failed"
        );
    }
}

public class Main {

    public static void main(String[] args) {

        try (MyResource resource =
                new MyResource()) {

            throw new RuntimeException(
                "Main operation failed"
            );

        }
        catch (Exception e) {

            System.out.println(
                "Main: " + e.getMessage()
            );

            for (Throwable t :
                    e.getSuppressed()) {

                System.out.println(
                    "Suppressed: " +
                    t.getMessage()
                );
            }
        }
    }
}
```

Conceptually:

```text
Main operation failed
        ↓
primary exception

Close failed
        ↓
suppressed exception
```

---

# 27. What If Only `close()` Throws?

Suppose:

```java
try (MyResource resource =
        new MyResource()) {

    System.out.println("Success");

}
```

and:

```java
close()
```

throws an exception.

Then the close exception can become the exception that propagates because there is no exception from the try body competing with it.

---

# 28. Compiler Perspective

Try-with-resources is not just:

```java
resource.close();
```

The compiler generates bytecode that implements the required cleanup and exception handling behavior.

Conceptually, you can think of:

```java
try (Resource r = new Resource()) {

    use(r);

}
```

as being transformed into logic similar to:

```java
Resource r = new Resource();

try {

    use(r);

}
finally {

    r.close();

}
```

But this is only a simplified mental model.

The actual semantics also handle:

```text
Primary exceptions
Suppressed exceptions
Multiple resources
Null handling
Resource closing order
```

---

# 29. Important Difference From Manual `finally`

Consider manual cleanup:

```java
Resource r = null;

try {

    r = new Resource();

}
finally {

    if (r != null) {
        r.close();
    }
}
```

With try-with-resources:

```java
try (Resource r =
        new Resource()) {

}
```

Java handles the resource lifecycle and suppressed-exception behavior for you.

Therefore, try-with-resources is generally preferred when working with `AutoCloseable` resources.

---

# 30. Resource Declaration and Initialization

The resource is initialized before the try body executes.

Example:

```java
try (
    MyResource resource =
        new MyResource()
) {

    resource.use();

}
```

Flow:

```text
Create resource
      ↓
Initialize resource
      ↓
Execute try body
      ↓
Close resource
```

If resource initialization itself fails, the try body does not execute.

---

# 31. If Multiple Resource Initialization Fails

Consider:

```java
try (
    Resource1 r1 = new Resource1();
    Resource2 r2 = new Resource2();
    Resource3 r3 = new Resource3()
) {

}
```

Suppose:

```text
r1 → created
r2 → created
r3 → initialization fails
```

Then resources that were successfully initialized before the failure are cleaned up appropriately.

Conceptually:

```text
r1 created
   ↓
r2 created
   ↓
r3 fails
   ↓
close r2
   ↓
close r1
```

This is another reason try-with-resources is useful.

---

# 32. Resource Dependency

A common pattern is:

```java
try (
    FileInputStream input =
        new FileInputStream("data.txt");

    BufferedInputStream buffer =
        new BufferedInputStream(input)
) {

}
```

Here:

```text
BufferedInputStream
       ↓
depends on
       ↓
FileInputStream
```

Closing order:

```text
BufferedInputStream
       ↓
FileInputStream
```

So resource declaration order matters.

---

# 33. Try-with-Resources and Scanner

`Scanner` implements `Closeable`, so it can be used.

Example:

```java
try (Scanner sc =
        new Scanner(System.in)) {

    int number = sc.nextInt();

    System.out.println(number);

}
```

However, be careful when using `Scanner(System.in)` inside larger applications.

Calling:

```java
sc.close();
```

also closes the underlying `System.in` stream.

So don't casually close a shared standard input resource if other parts of the application still need it.

---

# 34. Try-with-Resources and Files

Example:

```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class Main {

    public static void main(String[] args) {

        try (
            BufferedReader reader =
                new BufferedReader(
                    new FileReader("data.txt")
                )
        ) {

            String line;

            while ((line =
                    reader.readLine()) != null) {

                System.out.println(line);

            }

        }
        catch (IOException e) {

            System.out.println(
                e.getMessage()
            );
        }
    }
}
```

The reader is automatically closed.

---

# 35. Try-with-Resources and Database Connections

This concept becomes extremely important in backend development.

A database connection is a resource.

Conceptually:

```java
try (Connection connection =
        DriverManager.getConnection(...)) {

    // database operations

}
```

The connection is automatically closed after the block.

Similarly:

```java
try (
    Connection connection = ...;
    PreparedStatement statement = ...;
    ResultSet resultSet = ...
) {

    // database work

}
```

Resources are closed automatically in reverse order.

This pattern is extremely common in JDBC.

---

# 36. Important Interview Question

### Q: Does try-with-resources replace `finally` completely?

No.

It specifically solves **automatic resource management** for resources that implement the required closing contract.

`finally` still has legitimate uses for cleanup or actions that are not represented by an `AutoCloseable` resource.

---

# 37. Important Interview Question

### Q: Which interface is required for try-with-resources?

A resource must implement:

```text
AutoCloseable
```

or a subtype such as:

```text
Closeable
```

---

# 38. Important Interview Question

### Q: In what order are multiple resources closed?

Reverse declaration order.

Example:

```java
try (
    A a = new A();
    B b = new B();
    C c = new C()
) {

}
```

Closing:

```text
C
↓
B
↓
A
```

---

# 39. Important Interview Question

### Q: What happens if both try and close throw exceptions?

The exception from the try body is generally the primary exception.

The exception from `close()` is attached as a suppressed exception.

It can be retrieved using:

```java
e.getSuppressed();
```

---

# 40. Important Interview Question

### Q: Can we create our own resource for try-with-resources?

Yes.

Implement:

```java
AutoCloseable
```

Example:

```java
class MyResource implements AutoCloseable {

    @Override
    public void close() {
        System.out.println("Closed");
    }
}
```

---

# 41. Important Interview Question

### Q: When was try-with-resources introduced?

Java 7.

---

# 42. Important Interview Question

### Q: Can we use multiple resources?

Yes.

```java
try (
    Resource1 r1 = new Resource1();
    Resource2 r2 = new Resource2()
) {

}
```

They are closed automatically in reverse order.

---

# 43. Common Interview Traps

## Trap 1

> Try-with-resources only works with files.

❌ Wrong.

It works with any suitable `AutoCloseable` resource.

---

## Trap 2

> `AutoCloseable` has nothing to do with try-with-resources.

❌ Wrong.

It is the core contract that allows automatic closing.

---

## Trap 3

> Resources are closed in declaration order.

❌ Wrong.

They are closed in **reverse declaration order**.

---

## Trap 4

> If an exception occurs in the try block, the resource won't close.

❌ Wrong.

The resource is still closed.

---

## Trap 5

> An exception from `close()` always replaces the original exception.

❌ Wrong.

When both occur, the try-body exception generally remains primary and the close exception becomes suppressed.

---

## Trap 6

> `finally` cannot be used with try-with-resources.

❌ Wrong.

You can have:

```java
try (resource) {

}
catch (Exception e) {

}
finally {

}
```

---

## Trap 7

> Every object can be used as a resource.

❌ Wrong.

It needs to satisfy the `AutoCloseable` contract.

---

# 44. Best Practices

### 1. Prefer try-with-resources for closeable resources

Instead of manually closing:

```java
finally {
    resource.close();
}
```

prefer:

```java
try (resource) {

}
```

when appropriate.

---

### 2. Keep the resource scope small

Only keep the resource open for as long as necessary.

```text
Open
 ↓
Use
 ↓
Close
```

---

### 3. Don't ignore close failures

If cleanup can fail, don't assume it is always irrelevant.

Try-with-resources preserves those failures as suppressed exceptions when appropriate.

---

### 4. Understand resource dependencies

When multiple resources depend on each other, declare them in an order that allows reverse-order closing to work correctly.

---

### 5. Don't close shared resources blindly

For example:

```java
Scanner(System.in)
```

closing the scanner also closes `System.in`.

Understand the ownership of the resource before closing it.

---

# 45. Internal Working Summary

When Java sees:

```java
try (Resource r =
        new Resource()) {

    work(r);

}
```

the important conceptual flow is:

```text
Resource created
       ↓
Resource registered for automatic cleanup
       ↓
try body executes
       ↓
exception OR normal completion
       ↓
resource.close()
       ↓
exception propagation / normal completion
```

For multiple resources:

```text
r1 created
 ↓
r2 created
 ↓
r3 created
 ↓
try body
 ↓
r3.close()
 ↓
r2.close()
 ↓
r1.close()
```

---

# 46. 30-Second Interview Answer

> **Try-with-resources is a Java 7 feature used for automatic resource management. A resource used in it must implement `AutoCloseable` or a compatible subinterface such as `Closeable`. Java automatically calls `close()` when the try block finishes, even when an exception occurs. Multiple resources are closed in reverse declaration order. If both the try block and `close()` throw exceptions, the try-block exception is generally the primary exception and the close exception becomes suppressed, which can be accessed using `getSuppressed()`.**

---

# 47. Quick Revision

```text
TRY-WITH-RESOURCES
────────────────────────────────────

Introduced:
→ Java 7

Purpose:
→ Automatic resource management

Syntax:

try (Resource r = new Resource()) {

}

Resource requirement:
→ AutoCloseable

Important method:
→ close()

Automatic cleanup:
→ Yes

Works when exception occurs:
→ Yes

Multiple resources:
→ Yes

Closing order:
→ Reverse declaration order

Example:

try (
    A a = new A();
    B b = new B()
) {

}

Closing:
B → A

Java 9:
→ Existing effectively-final resources
   can be used in try-with-resources.

If try + close both throw:
→ try exception = primary
→ close exception = suppressed

Access suppressed exceptions:
→ e.getSuppressed()

Main advantage:
→ Safer + cleaner resource management
```

# 48. Ultimate Memory Trick

Remember:

```text
TWR
│
├── T → Try
│
├── W → With
│
└── R → Resources
```

And:

```text
OPEN
 ↓
USE
 ↓
AUTO CLOSE
```

For multiple resources:

```text
OPEN:  A → B → C

CLOSE: C → B → A
```

And the most important interview line:

```text
AutoCloseable
      ↓
try-with-resources
      ↓
automatic close()
```

### One-line definition:

> **Try-with-resources automatically manages and closes `AutoCloseable` resources, reducing resource leaks and simplifying exception-safe cleanup.**
