# ☕ 13 — Command-Line Arguments

> Command-line arguments allow us to pass values to a Java program when starting it from the command line.

---

# 1. What Are Command-Line Arguments?

Command-line arguments are values supplied to a Java program **when the program is executed**.

Example:

```bash
java Main Java 21
```

Here:

```text
Java
21
```

are command-line arguments.

Java receives them through:

```java
public static void main(String[] args)
```

---

# 2. The `args` Parameter

The standard `main()` method is:

```java
public static void main(String[] args) {

}
```

Here:

```text
String[]
```

means an array of Strings.

```text
args
↓
String[]
↓
contains command-line arguments
```

---

# 3. Basic Example

```java
public class Main {

    public static void main(String[] args) {

        System.out.println(args[0]);
        System.out.println(args[1]);

    }
}
```

Run:

```bash
java Main Java Backend
```

Output:

```text
Java
Backend
```

---

# 4. Arguments Are Strings

This is extremely important.

Suppose:

```bash
java Main 10 20
```

The values:

```text
10
20
```

are received as:

```java
String
```

not as `int`.

Conceptually:

```text
"10"
"20"
```

Therefore:

```java
System.out.println(args[0] + args[1]);
```

produces:

```text
1020
```

not:

```text
30
```

---

# 5. Converting Arguments to Integers

Use:

```java
Integer.parseInt()
```

Example:

```java
public class Main {

    public static void main(String[] args) {

        int a = Integer.parseInt(args[0]);
        int b = Integer.parseInt(args[1]);

        System.out.println(a + b);

    }
}
```

Run:

```bash
java Main 10 20
```

Output:

```text
30
```

Flow:

```text
"10"
 ↓
Integer.parseInt()
 ↓
10
 ↓
int
```

---

# 6. Converting to Other Types

### Integer

```java
int x = Integer.parseInt(args[0]);
```

### Double

```java
double x = Double.parseDouble(args[0]);
```

### Float

```java
float x = Float.parseFloat(args[0]);
```

### Long

```java
long x = Long.parseLong(args[0]);
```

### Boolean

```java
boolean x = Boolean.parseBoolean(args[0]);
```

---

# 7. `args.length`

`args.length` tells us how many arguments were supplied.

Example:

```java
public class Main {

    public static void main(String[] args) {

        System.out.println(args.length);

    }
}
```

Run:

```bash
java Main Java Spring Boot
```

Output:

```text
3
```

Because:

```text
args[0] → Java
args[1] → Spring
args[2] → Boot
```

---

# 8. Indexing

Command-line arguments are stored in an array.

Therefore indexing starts at:

```text
0
```

Example:

```bash
java Main A B C
```

```text
args[0] → A
args[1] → B
args[2] → C
```

Last argument:

```java
args[args.length - 1]
```

---

# 9. No Arguments

You can run:

```bash
java Main
```

Then:

```java
args.length
```

is:

```text
0
```

But:

```java
System.out.println(args[0]);
```

will cause:

```text
ArrayIndexOutOfBoundsException
```

because there is no element at index `0`.

---

# 10. Safe Argument Checking

Instead of directly accessing:

```java
System.out.println(args[0]);
```

you can check:

```java
if (args.length > 0) {
    System.out.println(args[0]);
}
```

This avoids accessing an index that doesn't exist.

---

# 11. Loop Through Arguments

Since `args` is an array:

```java
public class Main {

    public static void main(String[] args) {

        for (int i = 0; i < args.length; i++) {
            System.out.println(args[i]);
        }

    }
}
```

Run:

```bash
java Main Java Python C++
```

Output:

```text
Java
Python
C++
```

---

# 12. Enhanced For Loop

You can also use:

```java
for (String arg : args) {
    System.out.println(arg);
}
```

This is often cleaner when you don't need the index.

---

# 13. Command-Line Arguments vs Scanner

### Command-line arguments

Values are supplied **before the program starts**.

```bash
java Main Yashu 21
```

### Scanner

Values are entered **while the program is running**.

```java
Scanner sc = new Scanner(System.in);

String name = sc.nextLine();
```

Simple difference:

```text
Command Line
↓
input before/during program launch

Scanner
↓
interactive runtime input
```

---

# 14. Example — Student Information

```java
public class Student {

    public static void main(String[] args) {

        String name = args[0];
        int age = Integer.parseInt(args[1]);

        System.out.println("Name: " + name);
        System.out.println("Age: " + age);

    }
}
```

Run:

```bash
java Student Divyansh 22
```

Output:

```text
Name: Divyansh
Age: 22
```

---

# 15. Example — Addition

```java
public class Calculator {

    public static void main(String[] args) {

        int a = Integer.parseInt(args[0]);
        int b = Integer.parseInt(args[1]);

        System.out.println("Sum = " + (a + b));

    }
}
```

Run:

```bash
java Calculator 50 25
```

Output:

```text
Sum = 75
```

---

# 16. What Happens Internally?

When the JVM starts your application, it invokes the `main()` method and provides the command-line arguments as the `String[]` parameter.

Conceptually:

```text
Command Line
     ↓
java Main A B C
     ↓
JVM
     ↓
main(String[] args)
     ↓
args
     ↓
┌─────┬─────┬─────┐
│  A  │  B  │  C  │
└─────┴─────┴─────┘
   0     1     2
```

---

# 17. Is `args` a Keyword?

No.

`args` is simply a parameter name.

You can technically write:

```java
public static void main(String[] values) {

    System.out.println(values[0]);

}
```

This is valid.

Even:

```java
public static void main(String[] x) {

}
```

is valid.

The important part is:

```java
String[]
```

The name `args` is just the conventional name.

---

# 18. Can We Change the Name?

Yes.

```java
public static void main(String[] values) {

    System.out.println(values.length);

}
```

This works exactly the same way.

Interview point:

> `args` is not special syntax or a keyword.

---

# 19. Can We Overload `main()`?

Yes, you can define overloaded methods named `main`.

Example:

```java
public class Main {

    public static void main(String[] args) {
        System.out.println("Main");
    }

    public static void main(int x) {
        System.out.println(x);
    }

}
```

However, the JVM looks for the recognized application entry-point signature.

The overloaded:

```java
main(int x)
```

is not the standard entry point.

---

# 20. Can `main()` Have `String... args`?

Yes.

This is valid:

```java
public static void main(String... args) {

}
```

Because varargs:

```java
String... args
```

is compiled/treated as:

```java
String[] args
```

for the parameter type.

---

# 21. Can `main()` Be Written With `final`?

Yes.

For example:

```java
public static void main(final String[] args) {

}
```

The reference variable `args` cannot be reassigned inside the method.

But this does not make the array itself immutable.

For example, the elements can still be changed:

```java
args[0] = "Changed";
```

assuming an element exists.

---

# 22. Important: Arguments With Spaces

Suppose you run:

```bash
java Main Java Backend Developer
```

The JVM receives:

```text
args[0] = Java
args[1] = Backend
args[2] = Developer
```

If you want:

```text
Java Backend Developer
```

as one argument, quote it according to the command shell:

```bash
java Main "Java Backend Developer"
```

Then:

```text
args[0] = Java Backend Developer
```

---

# 23. Command-Line Arguments and Exceptions

Because all arguments initially arrive as Strings, conversion can fail.

Example:

```java
int age = Integer.parseInt(args[0]);
```

Run:

```bash
java Main hello
```

This causes:

```text
NumberFormatException
```

because:

```text
"hello"
```

cannot be converted to an integer.

---

# 24. Two Common Errors

### Missing argument

```java
args[0]
```

when no argument exists:

```text
ArrayIndexOutOfBoundsException
```

### Invalid numeric argument

```java
Integer.parseInt("abc")
```

results in:

```text
NumberFormatException
```

These are useful interview connections between command-line arguments and exception handling.

---

# 25. Command-Line Arguments vs Environment Variables

These are different concepts.

### Command-line argument

Passed when launching the program:

```bash
java Main production
```

Accessed through:

```java
args[0]
```

### Environment variable

Provided by the operating-system process environment.

Accessed using:

```java
System.getenv("NAME");
```

Don't confuse the two.

---

# 26. Command-Line Arguments vs System Properties

Java can also receive system properties using `-D`.

Example:

```bash
java -Denv=production Main
```

Access:

```java
String env = System.getProperty("env");
```

So:

```text
Command-line argument
→ args[]

System property
→ System.getProperty()

Environment variable
→ System.getenv()
```

---

# 27. Interview Questions

### Q1. What are command-line arguments?

Values passed to a Java program when it is launched from the command line.

### Q2. Where are command-line arguments stored?

In the `String[]` parameter of `main()`.

### Q3. What is `args`?

A reference to the array containing command-line arguments.

### Q4. Is `args` a keyword?

No.

### Q5. What is the type of `args`?

```java
String[]
```

### Q6. Are command-line arguments Strings?

Yes. They are provided to `main()` as Strings.

### Q7. How do you convert a String argument to int?

```java
Integer.parseInt(args[0]);
```

### Q8. What does `args.length` represent?

The number of command-line arguments supplied.

### Q9. What happens if `args[0]` is accessed when no argument was supplied?

An `ArrayIndexOutOfBoundsException` occurs.

### Q10. What happens when `"abc"` is passed to `Integer.parseInt()`?

`NumberFormatException`.

---

# 28. More Interview Questions

### Q11. Can the parameter name `args` be changed?

Yes.

```java
public static void main(String[] values)
```

is valid.

### Q12. Can `main()` use varargs?

Yes.

```java
public static void main(String... args)
```

### Q13. Is `String...` the same as `String[]`?

For the parameter declaration, yes, varargs is represented as an array type.

### Q14. Can we overload `main()`?

Yes, but only the recognized application entry-point signature is used to start the program.

### Q15. Can command-line arguments contain spaces?

Yes, if the shell passes the text as one quoted argument.

Example:

```bash
java Main "Java Backend"
```

### Q16. What is the difference between command-line arguments and Scanner?

Command-line arguments are supplied at launch; Scanner reads input from an input source during execution.

### Q17. Can command-line arguments be numbers?

They can represent numbers, but they initially arrive as Strings and must be parsed.

### Q18. How do you get the last command-line argument?

```java
args[args.length - 1]
```

provided at least one argument exists.

### Q19. What happens if no command-line arguments are passed?

`args` is an empty array, so:

```java
args.length
```

is `0`.

### Q20. Can `args` be `null`?

For normal JVM invocation of the Java application's `main` entry point, the JVM supplies an array; when no arguments are supplied, it is an empty array rather than `null`.

---

# 🔥 TOP 10 VVVVV IMPORTANT

## 1. What are command-line arguments?

Values passed to a Java program when it is launched.

---

## 2. What is the type of `args`?

```java
String[]
```

---

## 3. Are command-line arguments Strings?

Yes.

```bash
java Main 10
```

means:

```text
args[0] = "10"
```

not an `int`.

---

## 4. How do you convert `"10"` into `10`?

```java
int x = Integer.parseInt(args[0]);
```

---

## 5. What does `args.length` return?

The number of command-line arguments supplied.

---

## 6. Is `args` a keyword?

No.

This is also valid:

```java
public static void main(String[] values)
```

---

## 7. What happens if no argument is supplied but `args[0]` is accessed?

```text
ArrayIndexOutOfBoundsException
```

---

## 8. What happens if you parse an invalid number?

```java
Integer.parseInt("abc");
```

results in:

```text
NumberFormatException
```

---

## 9. Can we write `String... args`?

Yes.

```java
public static void main(String... args)
```

Varargs is represented as an array parameter.

---

## 10. Command-line argument vs environment variable?

```text
Command-line argument
→ args[]

Environment variable
→ System.getenv()

System property
→ System.getProperty()
```

---

# 🎤 30-SECOND INTERVIEW ANSWER

> Command-line arguments are values passed to a Java application when it is launched. They are received by the `main()` method through its `String[]` parameter. Every argument initially comes in as a String, so numeric values need to be parsed using methods such as `Integer.parseInt()`. The number of arguments is obtained using `args.length`. The name `args` itself is not a keyword and can be changed. If we access an index that doesn't exist, an `ArrayIndexOutOfBoundsException` can occur, while invalid numeric conversion can cause a `NumberFormatException`.

---

# ⚡ QUICK REVISION

```text
java Main A B C
        ↓
     JVM
        ↓
main(String[] args)
        ↓
args[0] → "A"
args[1] → "B"
args[2] → "C"
```

```text
args.length
↓
number of arguments
```

```text
args[0]
↓
String
```

```text
Integer.parseInt(args[0])
↓
int
```

```text
No argument + args[0]
↓
ArrayIndexOutOfBoundsException
```

```text
parseInt("abc")
↓
NumberFormatException
```

```text
args
→ just a variable name

String[]
→ important parameter type
```

---

# 🚀 NEXT

```text
13 → Command-Line Arguments ✓
14 → Keywords & Identifiers
```

> **14 will be a short but useful topic: Java keywords, reserved words, identifiers, naming rules, naming conventions, literals, and interview traps.**
