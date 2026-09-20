# Exception Hierarchy in Java

## 1. Why Does Java Have an Exception Hierarchy?

Java does not treat every exceptional condition as the same type of problem.

For example:

```text
Division by zero
Null reference
Invalid array index
File access failure
Out of memory
```

These represent different situations.

Java organizes exceptions and errors into a **class hierarchy** so that:

* related exceptions can be grouped
* specific exceptions can be handled separately
* broader exception types can handle multiple related exceptions
* inheritance and polymorphism can be used for exception handling

The root of this hierarchy is:

```java
Throwable
```

---

# 2. Complete High-Level Hierarchy

The most important structure is:

```text
Object
   │
   └── Throwable
        ├── Error
        │    ├── VirtualMachineError
        │    │    ├── OutOfMemoryError
        │    │    └── StackOverflowError
        │    │
        │    └── Other Errors
        │
        └── Exception
             ├── RuntimeException
             │    ├── NullPointerException
             │    ├── ArithmeticException
             │    ├── ArrayIndexOutOfBoundsException
             │    ├── NumberFormatException
             │    └── Other Runtime Exceptions
             │
             └── Other Exceptions
                  ├── IOException
                  ├── SQLException
                  ├── ClassNotFoundException
                  └── Other Checked Exceptions
```

The most important relationship to remember is:

```text
Object
   ↓
Throwable
   ├── Error
   └── Exception
        └── RuntimeException
```

---

# 3. `Throwable`

`Throwable` is the **root class of Java's throwable hierarchy**.

Conceptually:

```java
public class Throwable extends Object
```

Both:

```text
Error
Exception
```

extend `Throwable`.

Therefore, both can technically be thrown and caught.

Example:

```java
try {
    // risky code
}
catch (Throwable t) {
    // handles Throwable and its subclasses
}
```

However, catching `Throwable` is generally too broad for normal application-level exception handling because it also includes `Error`.

---

# 4. Why Is `Throwable` Important?

Because Java's exception mechanism works with objects belonging to the `Throwable` hierarchy.

Simplified:

```text
Throwable
   │
   ├── Error
   │
   └── Exception
```

Therefore:

```java
catch (Throwable t)
```

can potentially catch both:

```text
Error
Exception
```

But normally we handle more specific types instead.

---

# 5. `Error`

`Error` is a subclass of `Throwable`.

Hierarchy:

```text
Throwable
   │
   └── Error
```

Errors generally represent serious problems associated with the JVM or runtime environment.

Examples:

```text
OutOfMemoryError
StackOverflowError
NoClassDefFoundError
```

Example:

```java
public class Main {

    static void recursion() {
        recursion();
    }

    public static void main(String[] args) {
        recursion();
    }
}
```

This can eventually produce:

```text
StackOverflowError
```

---

# 6. Should We Normally Catch `Error`?

Generally, **no**.

For example:

```java
try {
    // code
}
catch (Error e) {
    // ...
}
```

is usually not appropriate as normal application-level exception handling.

Why?

Because errors such as:

```text
OutOfMemoryError
StackOverflowError
```

often indicate serious runtime problems that the application may not be able to safely recover from.

The important distinction is:

```text
Exception → conditions applications may often handle

Error     → serious JVM/runtime problems
```

This is a general distinction, not an absolute rule that every `Exception` is recoverable or every `Error` is impossible to handle.

---

# 7. `Exception`

`Exception` is another direct subclass of `Throwable`.

Hierarchy:

```text
Throwable
   │
   └── Exception
```

Exceptions generally represent conditions that application code may be able to handle.

Examples:

```text
IOException
SQLException
ClassNotFoundException
RuntimeException
```

---

# 8. Two Important Branches Under `Exception`

The `Exception` branch can be broadly understood as:

```text
Exception
   │
   ├── RuntimeException
   │
   └── Other Exceptions
```

This distinction becomes important when discussing **checked and unchecked exceptions**.

### RuntimeException branch

Examples:

```text
NullPointerException
ArithmeticException
NumberFormatException
IndexOutOfBoundsException
```

These are **unchecked exceptions**.

### Other Exception subclasses

Examples:

```text
IOException
SQLException
ClassNotFoundException
```

These are generally **checked exceptions**.

The checked-vs-unchecked distinction is covered in detail in:

```text
03-Checked-vs-Unchecked.md
```

---

# 9. `RuntimeException`

`RuntimeException` is a subclass of `Exception`.

Hierarchy:

```text
Throwable
   ↓
Exception
   ↓
RuntimeException
```

Examples:

```java
NullPointerException
ArithmeticException
NumberFormatException
ArrayIndexOutOfBoundsException
```

These exceptions are called **unchecked exceptions**.

---

# 10. Common Runtime Exceptions

## `NullPointerException`

Occurs when an operation is attempted on a `null` reference where an object is required.

Example:

```java
String name = null;

System.out.println(name.length());
```

Possible result:

```text
NullPointerException
```

Hierarchy:

```text
Throwable
 → Exception
   → RuntimeException
     → NullPointerException
```

---

## `ArithmeticException`

Occurs for certain invalid arithmetic operations.

Example:

```java
int result = 10 / 0;
```

Result:

```text
ArithmeticException
```

Hierarchy:

```text
Throwable
 → Exception
   → RuntimeException
     → ArithmeticException
```

---

## `NumberFormatException`

Occurs when a string cannot be converted into the requested numeric format.

Example:

```java
int number = Integer.parseInt("abc");
```

Result:

```text
NumberFormatException
```

Hierarchy:

```text
Throwable
 → Exception
   → RuntimeException
     → IllegalArgumentException
       → NumberFormatException
```

Notice something important here:

`NumberFormatException` does **not** directly extend `RuntimeException`.

It extends:

```text
IllegalArgumentException
```

which itself extends:

```text
RuntimeException
```

---

# 11. `IllegalArgumentException`

Hierarchy:

```text
Throwable
   ↓
Exception
   ↓
RuntimeException
   ↓
IllegalArgumentException
```

It is used when a method receives an argument that is inappropriate or invalid.

Example:

```java
Thread.sleep(-100);
```

can result in an `IllegalArgumentException`.

Another example:

```java
public void setAge(int age) {

    if (age < 0) {
        throw new IllegalArgumentException("Age cannot be negative");
    }

}
```

---

# 12. `IndexOutOfBoundsException`

This is a runtime exception related to invalid indexes.

Hierarchy:

```text
Throwable
   ↓
Exception
   ↓
RuntimeException
   ↓
IndexOutOfBoundsException
```

It has important subclasses such as:

```text
ArrayIndexOutOfBoundsException
StringIndexOutOfBoundsException
```

Example:

```java
int[] arr = {10, 20, 30};

System.out.println(arr[5]);
```

This can produce:

```text
ArrayIndexOutOfBoundsException
```

---

# 13. Checked Exception Branch

Not every `Exception` is a `RuntimeException`.

For example:

```text
Exception
   │
   ├── RuntimeException
   │
   └── IOException
```

`IOException` is a checked exception.

Example:

```java
FileReader reader = new FileReader("data.txt");
```

Opening a file can involve an `IOException`, so Java requires checked exceptions to be handled or declared according to the language rules.

Example:

```java
try {
    FileReader reader = new FileReader("data.txt");
}
catch (IOException e) {
    System.out.println("File operation failed.");
}
```

---

# 14. `IOException`

`IOException` represents an input/output related problem.

Hierarchy:

```text
Throwable
   ↓
Exception
   ↓
IOException
```

Common situations include:

```text
File operations
Input/output streams
Network I/O
```

Example:

```java
FileReader reader = new FileReader("data.txt");
```

If the file operation cannot be performed, an `IOException` may be involved.

---

# 15. `SQLException`

`SQLException` represents problems related to database access and other JDBC operations.

Hierarchy:

```text
Throwable
   ↓
Exception
   ↓
SQLException
```

Example:

```java
Connection connection = DriverManager.getConnection(
    url,
    username,
    password
);
```

Database-related operations can throw `SQLException`.

This is particularly important in backend development because JDBC and database operations commonly involve checked exceptions.

---

# 16. `ClassNotFoundException`

`ClassNotFoundException` occurs when an application attempts to load a class by name and the class cannot be found.

Hierarchy:

```text
Throwable
   ↓
Exception
   ↓
ClassNotFoundException
```

Example:

```java
Class.forName("com.example.SomeClass");
```

If the requested class cannot be found, `ClassNotFoundException` may occur.

---

# 17. Important Difference: `ClassNotFoundException` vs `NoClassDefFoundError`

These names are easy to confuse.

### `ClassNotFoundException`

```text
Exception
```

It is a **checked exception**.

It can occur when code explicitly attempts to load a class and the class cannot be found.

### `NoClassDefFoundError`

```text
Error
```

It is an **Error**, not an Exception.

It indicates that the JVM/class-loading process cannot find a class definition that was expected to be available at runtime.

Therefore:

```text
ClassNotFoundException → Exception
NoClassDefFoundError   → Error
```

Do not treat them as the same thing.

---

# 18. `OutOfMemoryError`

Hierarchy:

```text
Throwable
   ↓
Error
   ↓
VirtualMachineError
   ↓
OutOfMemoryError
```

It can occur when the JVM cannot allocate enough memory for an object or other required memory operation.

Example situations include:

```text
Very large allocations
Excessive object creation
Memory leaks
Insufficient JVM heap
```

This is generally a serious runtime condition.

---

# 19. `StackOverflowError`

Hierarchy:

```text
Throwable
   ↓
Error
   ↓
VirtualMachineError
   ↓
StackOverflowError
```

A common cause is excessive recursion.

Example:

```java
static void test() {
    test();
}
```

There is no terminating condition, so calls continue to accumulate on the stack.

Eventually:

```text
StackOverflowError
```

may occur.

---

# 20. `VirtualMachineError`

`VirtualMachineError` is a subclass of `Error`.

Hierarchy:

```text
Throwable
   ↓
Error
   ↓
VirtualMachineError
```

It represents serious problems related to the Java Virtual Machine.

Important subclasses include:

```text
OutOfMemoryError
StackOverflowError
InternalError
UnknownError
```

---

# 21. Complete Practical Hierarchy

For interview purposes, remember this structure:

```text
Object
   │
   └── Throwable
        │
        ├── Error
        │    │
        │    ├── VirtualMachineError
        │    │    ├── OutOfMemoryError
        │    │    └── StackOverflowError
        │    │
        │    └── Other Errors
        │
        └── Exception
             │
             ├── RuntimeException
             │    │
             │    ├── NullPointerException
             │    │
             │    ├── ArithmeticException
             │    │
             │    ├── IllegalArgumentException
             │    │    └── NumberFormatException
             │    │
             │    └── IndexOutOfBoundsException
             │         ├── ArrayIndexOutOfBoundsException
             │         └── StringIndexOutOfBoundsException
             │
             └── Other Exceptions
                  ├── IOException
                  ├── SQLException
                  └── ClassNotFoundException
```

This is a **simplified practical hierarchy**, not an exhaustive list of every Java throwable class.

---

# 22. Inheritance Is Why Broad Catching Works

Because exceptions use inheritance, a parent exception type can refer to a child exception object.

Example:

```java
try {
    int x = 10 / 0;
}
catch (RuntimeException e) {
    System.out.println("Runtime exception handled.");
}
```

The actual exception is:

```text
ArithmeticException
```

But:

```text
ArithmeticException
       ↓
RuntimeException
       ↓
Exception
       ↓
Throwable
```

Therefore, `RuntimeException` can catch it.

Even:

```java
catch (Exception e)
```

can catch it because `Exception` is its ancestor.

---

# 23. Specific vs General Catch

Consider:

```java
try {
    int x = 10 / 0;
}
catch (ArithmeticException e) {
    System.out.println("Arithmetic problem");
}
```

This is specific.

You can also write:

```java
catch (RuntimeException e) {
    System.out.println("Runtime problem");
}
```

This is broader.

Or:

```java
catch (Exception e) {
    System.out.println("Exception occurred");
}
```

This is even broader.

Hierarchy:

```text
ArithmeticException
       ↑
RuntimeException
       ↑
Exception
       ↑
Throwable
```

The higher you go, the broader the set of exceptions the handler can match.

---

# 24. Why Catch Order Matters

Consider:

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

This causes a **compile-time error**.

Why?

Because:

```text
ArithmeticException
       ↓
Exception
```

The first `catch` already catches `ArithmeticException`.

Therefore the second block can never be reached.

Correct order:

```java
try {
    int x = 10 / 0;
}
catch (ArithmeticException e) {
    System.out.println("Arithmetic");
}
catch (Exception e) {
    System.out.println("Exception");
}
```

General rule:

> **More specific exceptions must be caught before broader exceptions.**

---

# 25. Parent Reference and Child Exception

This is the same inheritance principle used elsewhere in Java.

Example:

```java
RuntimeException e = new ArithmeticException();
```

This is valid because:

```text
ArithmeticException IS-A RuntimeException
```

Similarly:

```java
Exception e = new ArithmeticException();
```

is valid because:

```text
ArithmeticException IS-A Exception
```

And:

```java
Throwable t = new ArithmeticException();
```

is also valid.

---

# 26. Exception Hierarchy and Polymorphism

Exception handling uses polymorphism.

Example:

```java
catch (Exception e)
```

can handle many different exception objects:

```text
ArithmeticException
NullPointerException
NumberFormatException
IOException
SQLException
...
```

provided they are subclasses of `Exception`.

This is possible because of inheritance and polymorphism.

---

# 27. `Throwable` Methods

Because all exceptions and errors ultimately inherit from `Throwable`, they inherit important functionality.

Common methods include:

```java
getMessage()
getCause()
getStackTrace()
printStackTrace()
toString()
```

Example:

```java
try {
    int x = 10 / 0;
}
catch (Exception e) {

    System.out.println(e.getMessage());
    System.out.println(e.toString());

    e.printStackTrace();
}
```

---

# 28. Why `Throwable` Is the Root Instead of `Exception`

A common interview question:

> Why isn't `Exception` the root of the hierarchy?

Because Java needs to represent both:

```text
Exceptions
Errors
```

under one common throwable type.

Therefore:

```text
Throwable
├── Error
└── Exception
```

provides a common root.

---

# 29. `Throwable` vs `Exception` vs `RuntimeException`

Remember the relationship:

```text
Throwable
   │
   └── Exception
        │
        └── RuntimeException
```

### `Throwable`

Root of Java's throwable hierarchy.

### `Exception`

Represents application-level exceptional conditions and has both checked and unchecked subclasses.

### `RuntimeException`

Represents unchecked exceptions.

---

# 30. Quick Comparison

| Type                    | Parent                     | General Meaning                                |
| ----------------------- | -------------------------- | ---------------------------------------------- |
| `Throwable`             | `Object`                   | Root of throwable hierarchy                    |
| `Error`                 | `Throwable`                | Serious JVM/runtime problems                   |
| `Exception`             | `Throwable`                | Exceptional conditions applications may handle |
| `RuntimeException`      | `Exception`                | Unchecked exceptions                           |
| `IOException`           | `Exception`                | I/O-related checked exception                  |
| `SQLException`          | `Exception`                | Database/JDBC-related checked exception        |
| `NullPointerException`  | `RuntimeException`         | Invalid use of `null` reference                |
| `ArithmeticException`   | `RuntimeException`         | Certain invalid arithmetic operations          |
| `NumberFormatException` | `IllegalArgumentException` | Invalid numeric conversion                     |
| `OutOfMemoryError`      | `VirtualMachineError`      | JVM cannot satisfy required memory allocation  |
| `StackOverflowError`    | `VirtualMachineError`      | Stack exhaustion                               |

---

# 31. Common Interview Traps

### Trap 1: Is `Throwable` an interface?

No.

```text
Throwable → class
```

---

### Trap 2: Is `Exception` an interface?

No.

```text
Exception → class
```

---

### Trap 3: Is `RuntimeException` an interface?

No.

```text
RuntimeException → class
```

---

### Trap 4: Is `Error` a subclass of `Exception`?

No.

Both directly extend `Throwable`.

```text
Throwable
├── Error
└── Exception
```

---

### Trap 5: Is `ArithmeticException` checked?

No.

It extends `RuntimeException`, so it is unchecked.

---

### Trap 6: Is `IOException` unchecked?

No.

`IOException` is a checked exception.

---

### Trap 7: Is `NumberFormatException` directly under `RuntimeException`?

No.

Its hierarchy is:

```text
RuntimeException
    ↓
IllegalArgumentException
    ↓
NumberFormatException
```

---

### Trap 8: Is `OutOfMemoryError` an Exception?

No.

It belongs to the `Error` branch.

---

### Trap 9: Can `Exception` catch `RuntimeException`?

Yes.

Because:

```text
RuntimeException extends Exception
```

---

### Trap 10: Can `RuntimeException` catch `IOException`?

No.

They are sibling branches under `Exception`:

```text
Exception
├── RuntimeException
└── IOException
```

`RuntimeException` is not a parent of `IOException`.

---

# 32. Mental Model

Think of the hierarchy as a family tree:

```text
                    Throwable
                   /         \
                  /           \
              Error         Exception
                             /       \
                            /         \
              RuntimeException      Checked
                    |
             -----------------
             |       |       |
            NPE    AEx      IAE
                            |
                         NFE
```

Where:

```text
NPE = NullPointerException
AEx = ArithmeticException
IAE = IllegalArgumentException
NFE = NumberFormatException
```

The closer a class is to the bottom, the more specific the problem.

---

# 33. 30-Second Interview Answer

> **Java's throwable hierarchy starts with `Throwable`, which directly extends `Object`. `Throwable` has two major branches: `Error` and `Exception`. `Error` generally represents serious JVM or runtime problems, while `Exception` represents exceptional conditions that application code may handle. `RuntimeException` is a subclass of `Exception` and represents unchecked exceptions such as `NullPointerException`, `ArithmeticException`, and `NumberFormatException`. Other exceptions such as `IOException` and `SQLException` are checked exceptions. Because this hierarchy uses inheritance, a parent exception type can handle exceptions of its child types.**

---

# 34. Quick Revision Cheat Sheet

```text
Object
   ↓
Throwable
   ├── Error
   │    └── VirtualMachineError
   │         ├── OutOfMemoryError
   │         └── StackOverflowError
   │
   └── Exception
        ├── RuntimeException
        │    ├── NullPointerException
        │    ├── ArithmeticException
        │    ├── IllegalArgumentException
        │    │    └── NumberFormatException
        │    └── IndexOutOfBoundsException
        │         ├── ArrayIndexOutOfBoundsException
        │         └── StringIndexOutOfBoundsException
        │
        └── Other Exceptions
             ├── IOException
             ├── SQLException
             └── ClassNotFoundException
```

### One-line memory trick:

```text
Throwable
   = Error + Exception

Exception
   = RuntimeException + other Exceptions

RuntimeException
   = unchecked

Other relevant Exception subclasses
   = commonly checked
```

---

# 35. Key Takeaways

```text
✓ Throwable is the root of the throwable hierarchy.

✓ Throwable directly extends Object.

✓ Error and Exception directly extend Throwable.

✓ RuntimeException extends Exception.

✓ RuntimeException subclasses are unchecked.

✓ IOException, SQLException, etc. are checked exceptions.

✓ Error is NOT a subclass of Exception.

✓ Parent exception types can catch child exception types.

✓ More specific catch blocks must come before broader catch blocks.

✓ NumberFormatException → IllegalArgumentException → RuntimeException.

✓ OutOfMemoryError and StackOverflowError belong to Error.

✓ Exception hierarchy enables inheritance and polymorphic exception handling.
```

> **Core idea:** Java's exception hierarchy organizes exceptional conditions from broad categories (`Throwable`, `Exception`, `Error`) down to specific problems (`NullPointerException`, `IOException`, `ArithmeticException`, etc.), allowing developers to handle errors at the appropriate level of specificity.
