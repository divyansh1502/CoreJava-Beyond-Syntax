# Custom Exceptions in Java

## 1. Introduction

A **custom exception** is an exception class created by the programmer for a specific application or business requirement.

Java already provides many built-in exceptions:

```text
ArithmeticException
NullPointerException
IllegalArgumentException
IOException
SQLException
```

But real applications often have rules that do not have a suitable built-in exception.

For example:

```text
InsufficientBalance
UserNotFound
InvalidAge
InvalidPassword
ProductOutOfStock
OrderNotFound
InvalidAccount
```

Instead of using a generic exception everywhere, we can create our own meaningful exception classes.

---

# 2. Why Do We Need Custom Exceptions?

Suppose we are creating a banking application.

```java
if (amount > balance) {
    throw new Exception("Insufficient balance");
}
```

This works, but the exception type is very generic.

A better approach is:

```java
if (amount > balance) {
    throw new InsufficientBalanceException(
        "Insufficient balance"
    );
}
```

Now the exception itself communicates what went wrong.

```text
Generic approach
        ↓
Exception

Custom approach
        ↓
InsufficientBalanceException
```

This makes the code easier to:

```text
Understand
Debug
Handle
Maintain
Log
Test
```

---

# 3. What Is a Custom Exception?

A custom exception is simply a class that extends an existing exception class.

Example:

```java
class InsufficientBalanceException
        extends Exception {

}
```

Now:

```text
InsufficientBalanceException
          ↓
       Exception
          ↓
       Throwable
```

Because it extends `Exception`, it becomes a **checked exception**.

---

# 4. Exception Hierarchy

The important hierarchy is:

```text
Object
   ↓
Throwable
   ├── Error
   │
   └── Exception
        ├── RuntimeException
        │    ├── ArithmeticException
        │    ├── NullPointerException
        │    └── IllegalArgumentException
        │
        └── Other checked exceptions
             ├── IOException
             └── SQLException
```

Custom exceptions can be placed under different branches depending on whether we want them checked or unchecked.

---

# 5. Creating a Checked Custom Exception

Example:

```java
class InvalidAgeException extends Exception {

    public InvalidAgeException(String message) {
        super(message);
    }
}
```

Now we can use it:

```java
static void checkAge(int age)
        throws InvalidAgeException {

    if (age < 18) {
        throw new InvalidAgeException(
            "Age must be 18 or above"
        );
    }

    System.out.println("Eligible");
}
```

---

# 6. Why Extend Exception?

When we write:

```java
class InvalidAgeException extends Exception
```

we are saying:

> `InvalidAgeException` is a type of exception and should behave like other checked exceptions.

Because `Exception` ultimately extends `Throwable`, the object can be used with:

```java
throw
```

and:

```java
catch
```

Example:

```java
try {
    checkAge(15);
}
catch (InvalidAgeException e) {
    System.out.println(e.getMessage());
}
```

---

# 7. Complete Checked Custom Exception Example

```java
class InvalidAgeException extends Exception {

    public InvalidAgeException(String message) {
        super(message);
    }
}

public class Main {

    static void checkAge(int age)
            throws InvalidAgeException {

        if (age < 18) {
            throw new InvalidAgeException(
                "Age must be 18 or above"
            );
        }

        System.out.println("Eligible");
    }

    public static void main(String[] args) {

        try {

            checkAge(15);

        }
        catch (InvalidAgeException e) {

            System.out.println(
                e.getMessage()
            );

        }
    }
}
```

Output:

```text
Age must be 18 or above
```

Flow:

```text
checkAge(15)
      ↓
age < 18
      ↓
throw new InvalidAgeException()
      ↓
method declares throws
      ↓
caller
      ↓
catch
      ↓
handled
```

---

# 8. Custom Exception With No-Argument Constructor

We can create a custom exception with a no-argument constructor.

```java
class InvalidAgeException extends Exception {

    public InvalidAgeException() {
        super();
    }
}
```

Usage:

```java
throw new InvalidAgeException();
```

However, exceptions are often more useful when they contain a meaningful message.

---

# 9. Custom Exception With Message Constructor

Common approach:

```java
class InvalidAgeException extends Exception {

    public InvalidAgeException(String message) {
        super(message);
    }
}
```

Then:

```java
throw new InvalidAgeException(
    "Age must be 18 or above"
);
```

The message can later be accessed using:

```java
e.getMessage();
```

---

# 10. Why Do We Call `super(message)`?

Consider:

```java
class InvalidAgeException extends Exception {

    public InvalidAgeException(String message) {
        super(message);
    }
}
```

`super(message)` calls the constructor of the parent class:

```text
InvalidAgeException
        ↓
Exception(String message)
        ↓
Throwable(String message)
```

The message is stored by the exception infrastructure.

Later:

```java
e.getMessage();
```

returns that message.

---

# 11. Custom Exception With Multiple Constructors

We can provide multiple constructors.

```java
class InvalidAgeException extends Exception {

    public InvalidAgeException() {
        super();
    }

    public InvalidAgeException(String message) {
        super(message);
    }

    public InvalidAgeException(
            String message,
            Throwable cause) {

        super(message, cause);
    }
}
```

Now we can create it in different ways:

```java
new InvalidAgeException();

new InvalidAgeException("Invalid age");

new InvalidAgeException(
    "Invalid age",
    originalException
);
```

---

# 12. What Is `cause`?

Sometimes one exception happens because of another exception.

Example:

```text
DatabaseException
       ↓
caused by
       ↓
SQLException
```

The original exception is called the **cause**.

We can preserve it:

```java
throw new DatabaseException(
    "Database operation failed",
    e
);
```

where:

```java
e
```

is the original exception.

This is called **exception chaining**.

---

# 13. Custom Unchecked Exception

We can also create an unchecked custom exception.

Instead of:

```java
extends Exception
```

use:

```java
extends RuntimeException
```

Example:

```java
class InvalidAgeException
        extends RuntimeException {

    public InvalidAgeException(String message) {
        super(message);
    }
}
```

Now:

```java
static void checkAge(int age) {

    if (age < 18) {

        throw new InvalidAgeException(
            "Age must be 18 or above"
        );

    }
}
```

Notice:

```text
No throws declaration required.
```

Why?

Because:

```text
RuntimeException
      ↓
unchecked
```

---

# 14. Checked vs Unchecked Custom Exception

### Checked

```java
class InvalidAgeException
        extends Exception {
}
```

Caller must handle or declare it.

```java
void test()
        throws InvalidAgeException {
}
```

or:

```java
try {
    test();
}
catch (InvalidAgeException e) {
}
```

---

### Unchecked

```java
class InvalidAgeException
        extends RuntimeException {
}
```

No compile-time handling requirement.

```java
void test() {

    throw new InvalidAgeException();

}
```

---

# 15. How to Decide Between Checked and Unchecked?

There is no single rule that applies perfectly to every application, but a useful design guideline is:

### Checked exception

Use when the caller can reasonably be expected to **recover from or explicitly handle** the condition.

Examples can include certain external/resource failures:

```text
File operation failure
Some recoverable external operation
```

### Unchecked exception

Often appropriate for:

```text
Programming errors
Invalid method arguments
Broken object state
Business/application conditions where explicit compile-time handling
would add unnecessary coupling
```

Modern Java applications, especially backend applications, commonly use custom `RuntimeException` subclasses together with centralized exception handling.

---

# 16. Real-World Example: Bank

Create:

```java
class InsufficientBalanceException
        extends RuntimeException {

    public InsufficientBalanceException(
            String message) {

        super(message);
    }
}
```

Bank account:

```java
class BankAccount {

    private double balance;

    public BankAccount(double balance) {
        this.balance = balance;
    }

    public void withdraw(double amount) {

        if (amount > balance) {

            throw new InsufficientBalanceException(
                "Insufficient balance"
            );

        }

        balance -= amount;
    }
}
```

Usage:

```java
BankAccount account =
    new BankAccount(5000);

account.withdraw(7000);
```

Flow:

```text
balance = 5000
amount = 7000

7000 > 5000
      ↓
true
      ↓
throw InsufficientBalanceException
```

This is much more meaningful than:

```java
throw new Exception("Error");
```

---

# 17. Real-World Example: User Not Found

```java
class UserNotFoundException
        extends RuntimeException {

    public UserNotFoundException(
            String message) {

        super(message);
    }
}
```

Service:

```java
class UserService {

    public String findUser(int id) {

        if (id <= 0) {

            throw new UserNotFoundException(
                "User not found: " + id
            );
        }

        return "User found";
    }
}
```

Now the exception itself describes the problem:

```text
UserNotFoundException
```

instead of a generic:

```text
Exception
```

---

# 18. Real-World Example: Product Out of Stock

```java
class ProductOutOfStockException
        extends RuntimeException {

    public ProductOutOfStockException(
            String message) {

        super(message);
    }
}
```

Usage:

```java
if (quantity == 0) {

    throw new ProductOutOfStockException(
        "Product is currently out of stock"
    );

}
```

This is common in e-commerce applications.

---

# 19. Custom Exception Can Contain Extra Data

A custom exception doesn't have to contain only a message.

Example:

```java
class InsufficientBalanceException
        extends RuntimeException {

    private final double balance;
    private final double requestedAmount;

    public InsufficientBalanceException(
            double balance,
            double requestedAmount) {

        super("Insufficient balance");

        this.balance = balance;
        this.requestedAmount = requestedAmount;
    }

    public double getBalance() {
        return balance;
    }

    public double getRequestedAmount() {
        return requestedAmount;
    }
}
```

Now:

```java
throw new InsufficientBalanceException(
    5000,
    7000
);
```

The exception carries structured information.

---

# 20. Why Extra Data Can Be Useful

Instead of only:

```text
Insufficient balance
```

the exception can contain:

```text
Current balance = 5000
Requested amount = 7000
```

This can be useful for:

```text
Logging
Debugging
Error responses
Monitoring
Exception handlers
```

However, sensitive information should not be exposed to clients just because it exists inside an exception.

---

# 21. Custom Exception Naming Convention

Java convention is to end exception class names with:

```text
Exception
```

Examples:

```text
InvalidAgeException
UserNotFoundException
InsufficientBalanceException
PaymentFailedException
ProductOutOfStockException
```

Avoid names such as:

```text
InvalidAge
UserError
BadUser
Problem
```

Prefer:

```text
InvalidAgeException
UserNotFoundException
```

---

# 22. Custom Exception Should Be Specific

Prefer:

```java
throw new UserNotFoundException(
    "User not found"
);
```

over:

```java
throw new Exception(
    "User not found"
);
```

Why?

Because callers can distinguish different problems:

```java
catch (UserNotFoundException e) {
    // user-specific handling
}
```

versus everything being:

```java
catch (Exception e) {
}
```

---

# 23. Multiple Custom Exceptions

A project can have many custom exceptions.

For example:

```text
exceptions/
│
├── UserNotFoundException.java
├── InvalidPasswordException.java
├── InsufficientBalanceException.java
├── ProductNotFoundException.java
└── ProductOutOfStockException.java
```

Each exception represents a meaningful application condition.

---

# 24. Custom Exception Hierarchy

Custom exceptions can also have their own hierarchy.

Example:

```text
ApplicationException
       │
       ├── UserException
       │      ├── UserNotFoundException
       │      └── InvalidUserException
       │
       └── OrderException
              ├── OrderNotFoundException
              └── OrderAlreadyCancelledException
```

For example:

```java
class ApplicationException
        extends RuntimeException {

    public ApplicationException(String message) {
        super(message);
    }
}
```

Then:

```java
class UserException
        extends ApplicationException {

    public UserException(String message) {
        super(message);
    }
}
```

And:

```java
class UserNotFoundException
        extends UserException {

    public UserNotFoundException(String message) {
        super(message);
    }
}
```

This allows handling at different levels.

---

# 25. Handling a Specific Custom Exception

```java
try {

    service.findUser(10);

}
catch (UserNotFoundException e) {

    System.out.println(
        "User does not exist"
    );
}
```

The catch block can specifically target that exception.

---

# 26. Handling Parent Custom Exception

Suppose:

```text
UserNotFoundException
       ↓
UserException
       ↓
ApplicationException
       ↓
RuntimeException
```

We can catch the parent:

```java
catch (UserException e) {

    System.out.println(
        "Some user-related problem"
    );
}
```

This can catch:

```text
UserNotFoundException
InvalidUserException
```

because both are subclasses of `UserException`.

---

# 27. Catch Ordering

Specific exceptions should generally come before their parent exceptions.

Correct:

```java
try {

}
catch (UserNotFoundException e) {

}
catch (UserException e) {

}
```

Incorrect:

```java
try {

}
catch (UserException e) {

}
catch (UserNotFoundException e) {

}
```

Why?

Because the first catch already catches `UserNotFoundException`.

The second catch becomes unreachable.

---

# 28. Custom Exception + `throw`

The basic pattern is:

```java
throw new CustomException(
    "message"
);
```

Example:

```java
throw new UserNotFoundException(
    "User with ID 10 not found"
);
```

---

# 29. Custom Exception + `throws`

For a checked custom exception:

```java
static void findUser()
        throws UserNotFoundException {

    throw new UserNotFoundException(
        "User not found"
    );
}
```

Here:

```text
throws
→ declares

throw
→ actually throws
```

---

# 30. Custom Exception + try-catch

```java
try {

    findUser();

}
catch (UserNotFoundException e) {

    System.out.println(
        e.getMessage()
    );
}
```

Complete flow:

```text
method call
    ↓
business condition
    ↓
throw custom exception
    ↓
propagation
    ↓
catch custom exception
    ↓
handle
```

---

# 31. Custom Exception + Exception Chaining

Suppose database code throws:

```java
SQLException
```

The service layer might translate it:

```java
try {

    databaseOperation();

}
catch (SQLException e) {

    throw new UserRepositoryException(
        "Unable to load user",
        e
    );
}
```

Now the higher layer sees:

```text
UserRepositoryException
```

while the original cause remains:

```text
SQLException
```

This preserves debugging information.

---

# 32. `getMessage()`

The message can be retrieved using:

```java
e.getMessage();
```

Example:

```java
class InvalidAgeException
        extends Exception {

    public InvalidAgeException(String message) {
        super(message);
    }
}
```

Then:

```java
try {

    throw new InvalidAgeException(
        "Age is invalid"
    );

}
catch (InvalidAgeException e) {

    System.out.println(
        e.getMessage()
    );

}
```

Output:

```text
Age is invalid
```

---

# 33. `getCause()`

If an exception has a cause:

```java
catch (Exception e) {

    Throwable cause = e.getCause();

}
```

Example:

```java
throw new RuntimeException(
    "Operation failed",
    originalException
);
```

Then:

```java
e.getCause()
```

returns the original exception.

---

# 34. `printStackTrace()`

A custom exception behaves like normal Java exceptions.

You can call:

```java
e.printStackTrace();
```

It prints information about the exception and its stack trace.

Example:

```java
catch (UserNotFoundException e) {

    e.printStackTrace();

}
```

In production applications, logging frameworks are generally preferred over directly printing stack traces.

---

# 35. Custom Exception and Stack Trace

When you create:

```java
throw new UserNotFoundException(
    "User not found"
);
```

Java records information about where the exception was created/thrown.

This information helps identify the execution path that led to the error.

Conceptually:

```text
throw
 ↓
stack trace information
 ↓
caller
 ↓
higher caller
```

This is extremely useful for debugging.

---

# 36. Custom Exception Constructors

A commonly useful pattern is:

```java
class UserNotFoundException
        extends RuntimeException {

    public UserNotFoundException() {
        super();
    }

    public UserNotFoundException(String message) {
        super(message);
    }

    public UserNotFoundException(
            String message,
            Throwable cause) {

        super(message, cause);
    }

    public UserNotFoundException(
            Throwable cause) {

        super(cause);
    }
}
```

This provides flexibility for different situations.

---

# 37. Should Every Custom Exception Have Four Constructors?

No.

You don't have to create all four.

Use constructors that make sense for your application.

For a simple custom exception:

```java
class UserNotFoundException
        extends RuntimeException {

    public UserNotFoundException(String message) {
        super(message);
    }
}
```

may be completely sufficient.

---

# 38. Custom Exceptions in Layered Applications

In a backend application, exception flow may look like:

```text
Controller
    ↓
Service
    ↓
Repository
    ↓
Database
```

Suppose a repository cannot find a user.

```text
Repository
    ↓
UserNotFoundException
    ↓
Service
    ↓
Controller / Exception Handler
    ↓
HTTP response
```

This separates:

```text
Business logic
```

from:

```text
Error handling
```

---

# 39. Custom Exceptions in Spring Boot

A common Spring Boot architecture is:

```text
Service
   ↓
throw UserNotFoundException
   ↓
Global Exception Handler
   ↓
HTTP response
```

For example:

```java
throw new UserNotFoundException(
    "User not found"
);
```

A centralized exception handler can then convert it into an appropriate API response.

This is why understanding custom exceptions in Core Java becomes directly useful when you move into Spring Boot.

---

# 40. Don't Use Exceptions for Normal Control Flow

Avoid code like:

```java
try {

    findUser();

}
catch (UserNotFoundException e) {

    // normal expected program flow

}
```

if the condition can naturally be represented without an exception.

Exceptions should generally represent exceptional/error conditions rather than being used as a normal branching mechanism.

---

# 41. Don't Create Too Many Generic Exceptions

Avoid:

```text
ApplicationException
SomethingWentWrongException
GeneralException
```

for every possible problem.

Prefer meaningful types where the distinction matters:

```text
UserNotFoundException
PaymentFailedException
InsufficientBalanceException
```

The goal is clarity, not simply creating more classes.

---

# 42. Don't Expose Internal Exception Details

Suppose the server has:

```java
throw new RuntimeException(
    "SQL connection failed at database server..."
);
```

You generally should not blindly expose that complete internal message to an API client.

Instead:

```text
Internal exception
       ↓
log detailed information
       ↓
return safe client-facing message
```

For example:

```text
Client:
"Unable to process request"

Server log:
detailed database exception + stack trace
```

This is particularly important in backend development.

---

# 43. Custom Exception vs Built-in Exception

### Built-in

```java
throw new IllegalArgumentException(
    "Invalid age"
);
```

### Custom

```java
throw new InvalidAgeException(
    "Age must be 18 or above"
);
```

Use a custom exception when the distinction provides meaningful value to your application.

---

# 44. When NOT to Create a Custom Exception

Do not create:

```java
class InvalidNumberException
        extends RuntimeException {
}
```

just because a built-in exception already expresses the same condition well.

For example:

```java
throw new IllegalArgumentException(
    "Number must be positive"
);
```

may already be perfectly appropriate.

Custom exceptions should add semantic value.

---

# 45. Checked Custom Exception Example

```java
class InvalidAgeException extends Exception {

    public InvalidAgeException(String message) {
        super(message);
    }
}

class AgeValidator {

    static void validate(int age)
            throws InvalidAgeException {

        if (age < 18) {

            throw new InvalidAgeException(
                "Age must be 18 or above"
            );

        }
    }
}
```

Usage:

```java
try {

    AgeValidator.validate(16);

}
catch (InvalidAgeException e) {

    System.out.println(e.getMessage());

}
```

---

# 46. Unchecked Custom Exception Example

```java
class InvalidAgeException
        extends RuntimeException {

    public InvalidAgeException(String message) {
        super(message);
    }
}
```

Usage:

```java
class AgeValidator {

    static void validate(int age) {

        if (age < 18) {

            throw new InvalidAgeException(
                "Age must be 18 or above"
            );

        }
    }
}
```

Caller:

```java
AgeValidator.validate(16);
```

No `throws` declaration is required.

---

# 47. Key Difference

```text
Custom Exception extends Exception
        ↓
Checked custom exception
        ↓
Handle OR declare
```

```text
Custom Exception extends RuntimeException
        ↓
Unchecked custom exception
        ↓
No compile-time handling requirement
```

---

# 48. Interview Questions

## Q1. What is a custom exception?

A custom exception is a user-defined exception class created to represent a specific application or business condition.

---

## Q2. How do you create a custom exception?

Extend `Exception` or `RuntimeException`.

Example:

```java
class UserNotFoundException
        extends RuntimeException {

    public UserNotFoundException(String message) {
        super(message);
    }
}
```

---

## Q3. How do you create a checked custom exception?

Extend:

```java
Exception
```

Example:

```java
class InvalidAgeException
        extends Exception {
}
```

---

## Q4. How do you create an unchecked custom exception?

Extend:

```java
RuntimeException
```

Example:

```java
class InvalidAgeException
        extends RuntimeException {
}
```

---

## Q5. Why use custom exceptions?

They provide:

```text
Meaningful error types
Better readability
Specific exception handling
Better debugging
Better application design
```

---

## Q6. Why call `super(message)`?

To pass the message to the parent exception class so it can later be retrieved using:

```java
getMessage()
```

---

## Q7. Can a custom exception contain fields?

Yes.

Example:

```java
class PaymentException
        extends RuntimeException {

    private final int paymentId;

    public PaymentException(
            String message,
            int paymentId) {

        super(message);
        this.paymentId = paymentId;
    }
}
```

---

## Q8. Can a custom exception have constructors?

Yes.

It can have one or multiple constructors.

---

## Q9. Can custom exceptions have a cause?

Yes.

```java
super(message, cause);
```

---

## Q10. What is exception chaining?

Preserving an original exception as the cause of another exception.

Example:

```java
throw new RuntimeException(
    "Operation failed",
    e
);
```

---

## Q11. Can we catch a custom exception?

Yes.

```java
catch (UserNotFoundException e) {
}
```

---

## Q12. Can we throw a custom exception?

Yes.

```java
throw new UserNotFoundException(
    "User not found"
);
```

---

## Q13. Can a custom exception extend another custom exception?

Yes.

Example:

```text
ApplicationException
      ↓
UserException
      ↓
UserNotFoundException
```

---

## Q14. Is a custom exception always checked?

No.

It depends on its parent.

```text
extends Exception
→ checked

extends RuntimeException
→ unchecked
```

---

# 49. Common Interview Traps

### Trap 1

> Every custom exception must extend `Exception`.

❌ No.

It can extend:

```text
Exception
```

or:

```text
RuntimeException
```

depending on the desired behavior.

---

### Trap 2

> Custom exceptions don't support `getMessage()`.

❌ Wrong.

If they inherit from `Throwable`, they inherit exception functionality such as:

```java
getMessage()
getCause()
printStackTrace()
```

---

### Trap 3

> Custom exceptions cannot have fields.

❌ Wrong.

They are normal Java classes and can contain:

```text
fields
constructors
methods
```

---

### Trap 4

> `extends RuntimeException` means the exception is not an exception.

❌ Wrong.

`RuntimeException` itself is a subclass of `Exception`.

---

### Trap 5

> We should create a custom exception for every error.

❌ Not necessarily.

Use one when it adds meaningful semantic value.

---

### Trap 6

> Custom exceptions automatically handle themselves.

❌ No.

They still follow normal exception propagation rules.

---

# 50. Custom Exception Design Pattern

A clean basic pattern:

```java
class UserNotFoundException
        extends RuntimeException {

    public UserNotFoundException(String message) {
        super(message);
    }
}
```

Use:

```java
if (user == null) {

    throw new UserNotFoundException(
        "User not found"
    );

}
```

Handle:

```java
try {

    findUser();

}
catch (UserNotFoundException e) {

    System.out.println(
        e.getMessage()
    );
}
```

---

# 51. Complete Real-World Example

```java
class InsufficientBalanceException
        extends RuntimeException {

    public InsufficientBalanceException(
            String message) {

        super(message);
    }
}

class BankAccount {

    private double balance;

    public BankAccount(double balance) {
        this.balance = balance;
    }

    public void withdraw(double amount) {

        if (amount <= 0) {

            throw new IllegalArgumentException(
                "Amount must be positive"
            );
        }

        if (amount > balance) {

            throw new InsufficientBalanceException(
                "Insufficient balance"
            );
        }

        balance -= amount;
    }

    public double getBalance() {
        return balance;
    }
}

public class Main {

    public static void main(String[] args) {

        BankAccount account =
            new BankAccount(5000);

        try {

            account.withdraw(7000);

        }
        catch (InsufficientBalanceException e) {

            System.out.println(
                e.getMessage()
            );
        }

    }
}
```

Output:

```text
Insufficient balance
```

Notice that we use:

```java
IllegalArgumentException
```

for an invalid argument:

```text
amount <= 0
```

and a custom exception for the meaningful business condition:

```text
amount > balance
```

This is a good example of using the right exception type for the right situation.

---

# 52. Internal Working

A custom exception is still an ordinary Java object.

For example:

```java
throw new UserNotFoundException(
    "User not found"
);
```

Conceptually:

```text
new
 ↓
Object created in heap
 ↓
constructor executes
 ↓
Throwable state initialized
 ↓
throw transfers control exceptionally
 ↓
JVM searches for matching handler
 ↓
catch / propagation
```

The JVM does not treat your custom exception as fundamentally different from built-in exceptions.

The important difference is the **type and meaning** you gave the class.

---

# 53. Memory Perspective

Suppose:

```java
throw new UserNotFoundException(
    "User not found"
);
```

The exception object is an object created on the heap like other Java objects.

It contains inherited exception state, such as:

```text
message
cause
stack trace
suppressed exceptions
```

plus any custom fields you define.

Conceptually:

```text
Heap
┌─────────────────────────────────┐
│ UserNotFoundException object    │
│                                 │
│ message                         │
│ cause                           │
│ stack trace                     │
│ custom fields                   │
└─────────────────────────────────┘
```

The JVM then uses the exception mechanism to search the call stack for a matching handler.

---

# 54. Custom Exceptions and Stack Unwinding

Suppose:

```text
main()
  ↓
service()
  ↓
repository()
  ↓
throw UserNotFoundException
```

If `repository()` doesn't handle it:

```text
repository()
     ↓
exception
     ↓
service()
```

If `service()` doesn't handle it:

```text
service()
     ↓
exception
     ↓
main()
```

This process is commonly described as **stack unwinding**.

Eventually Java searches for a matching `catch` block.

---

# 55. 30-Second Interview Answer

> **A custom exception is a user-defined exception class created to represent a specific application or business condition. We create one by extending `Exception` for a checked exception or `RuntimeException` for an unchecked exception. Custom exceptions make error handling more meaningful because callers can catch specific exception types instead of relying on generic exceptions. They can also contain custom fields, constructors, messages, and causes for better debugging and application design.**

---

# 56. Quick Revision

```text
CUSTOM EXCEPTION
────────────────────────────────────

Custom exception
→ User-defined exception class

Checked custom exception
→ extends Exception

Unchecked custom exception
→ extends RuntimeException

Create:
class UserNotFoundException
        extends RuntimeException {
}

Throw:
throw new UserNotFoundException(
    "User not found"
);

Checked propagation:
method() throws UserNotFoundException

Message:
e.getMessage()

Cause:
e.getCause()

Exception chaining:
new Exception(message, cause)

Custom exceptions can contain:
✓ fields
✓ constructors
✓ methods
✓ custom messages
✓ causes

Naming convention:
UserNotFoundException
PaymentFailedException
InvalidAgeException

Use custom exceptions when:
→ they provide meaningful semantic value

Avoid:
→ creating unnecessary generic exceptions
→ using exceptions as normal control flow
→ exposing sensitive internal error details
```

# 57. Ultimate Memory Trick

```text
Built-in Exception
       ↓
Not specific enough?
       ↓
Create Custom Exception
       ↓
extends Exception
       │
       └── Checked

or

extends RuntimeException
       │
       └── Unchecked
```

And remember:

```text
throw
   ↓
Actually throws the custom exception

throws
   ↓
Declares that it may propagate

catch
   ↓
Handles it
```

### The core pattern:

```java
class UserNotFoundException
        extends RuntimeException {

    public UserNotFoundException(String message) {
        super(message);
    }
}
```

```java
if (user == null) {

    throw new UserNotFoundException(
        "User not found"
    );

}
```

That's the foundation you'll use later in **Spring Boot service + global exception handling**.
