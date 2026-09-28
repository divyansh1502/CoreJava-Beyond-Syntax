# 🧠 Tricky Java Questions — Interview Questions & Answers

> **A focused collection of tricky Java interview questions designed to test concepts, edge cases, output prediction, and common misconceptions.**

---

# 📑 Table of Contents

- [1. Strings](#1-strings)
- [2. Operators and Expressions](#2-operators-and-expressions)
- [3. OOP and Inheritance](#3-oop-and-inheritance)
- [4. Static, Final and Initialization](#4-static-final-and-initialization)
- [5. Arrays](#5-arrays)
- [6. Collections](#6-collections)
- [7. Exceptions](#7-exceptions)
- [8. Wrapper Classes](#8-wrapper-classes)
- [9. Multithreading](#9-multithreading)
- [10. JVM and Memory](#10-jvm-and-memory)
- [11. Tricky Output Questions](#11-tricky-output-questions)
- [12. Rapid-Fire Tricky Questions](#12-rapid-fire-tricky-questions)

---

# 1. Strings

## 1. Is String mutable or immutable?

`String` is immutable.

Once a String object is created, its contents cannot be changed.

---

## 2. What happens here?

```java
String s = "Java";

s.concat(" Programming");

System.out.println(s);
```

Output:

```text
Java
```

`concat()` creates a new String because String is immutable.

The returned String was not assigned.

---

## 3. What happens here?

```java
String s = "Java";

s = s.concat(" Programming");

System.out.println(s);
```

Output:

```text
Java Programming
```

The original String is unchanged.

The reference `s` is updated to point to the new String.

---

## 4. What is the output?

```java
String a = "Java";
String b = "Java";

System.out.println(a == b);
```

Output:

```text
true
```

Both string literals are interned and normally refer to the same pooled String object.

---

## 5. What is the output?

```java
String a = new String("Java");
String b = new String("Java");

System.out.println(a == b);
System.out.println(a.equals(b));
```

Output:

```text
false
true
```

`==` compares references.

`equals()` compares String contents.

---

## 6. What is the output?

```java
String a = "Ja";
String b = "va";

String c = a + b;
String d = "Java";

System.out.println(c == d);
```

Usually:

```text
false
```

Because `a + b` involves runtime concatenation, while `"Java"` is a compile-time constant literal.

---

## 7. What happens here?

```java
String a = "Java";
String b = "Ja" + "va";

System.out.println(a == b);
```

Output:

```text
true
```

The concatenation consists entirely of compile-time constants and can be resolved to the same interned literal.

---

## 8. What is the difference between `==` and `equals()`?

```text
==
→ compares references for objects


equals()
→ compares logical equality according to the class implementation
```

For String:

```text
equals()
→ content comparison
```

---

## 9. What does `intern()` do?

`intern()` returns the canonical representation of the String from the String pool.

Example:

```java
String a = new String("Java");
String b = a.intern();

System.out.println(b == "Java");
```

Output:

```text
true
```

---

## 10. Is String final?

Yes.

`String` is declared as a final class, so it cannot be subclassed.

---

## 11. Why is String immutable?

Important reasons include:

```text
Security
Thread safety
String pool sharing
Hash-code caching
Predictable behavior
```

---

# 2. Operators and Expressions

## 12. What is the output?

```java
int x = 10;

System.out.println(x++);
System.out.println(x);
```

Output:

```text
10
11
```

Post-increment returns the old value and then increments.

---

## 13. What is the output?

```java
int x = 10;

System.out.println(++x);
System.out.println(x);
```

Output:

```text
11
11
```

Pre-increment increments first and then returns the new value.

---

## 14. What is the output?

```java
int x = 5;

System.out.println(x++ + ++x);
```

Output:

```text
12
```

Step-by-step:

```text
x++ → 5, x becomes 6
++x → x becomes 7, value = 7

5 + 7 = 12
```

---

## 15. What is integer division?

When both operands are integers, division produces an integer result.

```java
System.out.println(5 / 2);
```

Output:

```text
2
```

---

## 16. What is the output?

```java
System.out.println(5 / 2.0);
```

Output:

```text
2.5
```

Because one operand is a floating-point value.

---

## 17. What is the output?

```java
System.out.println(10 + 20 + "Java");
```

Output:

```text
30Java
```

Evaluation proceeds left to right.

---

## 18. What is the output?

```java
System.out.println("Java" + 10 + 20);
```

Output:

```text
Java1020
```

Once String concatenation begins, subsequent values are converted to String representations.

---

## 19. What is the output?

```java
System.out.println(10 + 20 + "Java" + 30 + 40);
```

Output:

```text
30Java3040
```

---

## 20. What happens with `+` when one operand is String?

The `+` operator performs String concatenation if one operand is a String after the relevant evaluation rules are applied.

---

## 21. Can a boolean be converted to int automatically?

No.

Java does not support:

```text
boolean ↔ int
```

implicit conversion.

---

## 22. Can an int be assigned to a byte?

Not without narrowing conversion when the value is represented as an int expression.

```java
byte b = 10;
```

is valid because `10` is an integer constant representable as a byte.

But:

```java
int x = 10;
byte b = x;
```

does not compile without a cast.

---

## 23. What is the output?

```java
byte b = 10;

b = (byte) (b + 20);

System.out.println(b);
```

Output:

```text
30
```

Arithmetic on `byte` values generally promotes them to `int`.

---

## 24. What happens with byte overflow?

```java
byte b = 127;

b++;
System.out.println(b);
```

Output:

```text
-128
```

The increment is converted back to byte with two's-complement wraparound.

---

## 25. What is short-circuit evaluation?

Java's:

```text
&&
||
```

operators use short-circuit evaluation.

For `&&`, if the left side is false, the right side is not evaluated.

For `||`, if the left side is true, the right side is not evaluated.

---

## 26. What is the difference between `&` and `&&`?

```text
&
→ bitwise AND for integral values
→ logical AND for booleans
→ does not short-circuit


&&
→ logical AND
→ short-circuits
```

---

## 27. What is the difference between `|` and `||`?

```text
|
→ bitwise OR for integral values
→ logical OR for booleans
→ does not short-circuit


||
→ logical OR
→ short-circuits
```

---

# 3. OOP and Inheritance

## 28. Can a constructor be inherited?

No.

Constructors belong to the class in which they are declared.

---

## 29. Can a constructor be overridden?

No.

Constructors are not inherited and therefore cannot be overridden.

---

## 30. Can a constructor be overloaded?

Yes.

A class can have multiple constructors with different parameter lists.

---

## 31. Can an abstract class have a constructor?

Yes.

Its constructor executes as part of constructing a concrete subclass object.

---

## 32. Can an abstract class have a static method?

Yes.

Static methods belong to the class, so they do not require an instance.

---

## 33. Can an abstract class be instantiated?

No.

You cannot directly create an object of an abstract class.

---

## 34. Can an abstract class have zero abstract methods?

Yes.

A class can be abstract even if it has no abstract methods.

---

## 35. Can an interface have a constructor?

No.

Interfaces are not instantiated directly, so they do not have constructors.

---

## 36. Can an interface contain variables?

Yes.

Fields declared in an interface are implicitly:

```text
public
static
final
```

---

## 37. Can an interface have methods with implementation?

Yes.

Modern Java interfaces can contain:

```text
default methods
static methods
private methods
```

along with abstract methods.

---

## 38. Can a class extend multiple classes?

No.

Java does not support multiple inheritance of classes.

---

## 39. Can a class implement multiple interfaces?

Yes.

```java
class Test
    implements A, B, C {
}
```

---

## 40. Can an interface extend multiple interfaces?

Yes.

```java
interface C
    extends A, B {
}
```

---

## 41. Can a private method be overridden?

No.

Private methods are not inherited by subclasses.

---

## 42. Can a static method be overridden?

No.

Static methods are hidden, not overridden.

---

## 43. Can a final method be overridden?

No.

A final method cannot be overridden by a subclass.

---

## 44. Can a final class be inherited?

No.

A final class cannot be extended.

---

## 45. Can a final reference point to a different object?

No.

The reference cannot be reassigned after initialization.

However, the referenced object's mutable state may still change.

---

## 46. What is the difference between final reference and immutable object?

```text
final reference
→ reference cannot point to another object


immutable object
→ object's state cannot change
```

A final reference does not automatically make the object immutable.

---

## 47. What happens when a parent reference points to a child object?

Example:

```java
Animal a = new Dog();
```

The reference type determines what members are accessible at compile time.

The actual object type determines overridden instance-method behavior at runtime.

---

## 48. What is method overloading resolution based on?

Overloading is resolved primarily at compile time based on the method signature and the compile-time types of arguments.

---

## 49. What is method overriding resolution based on?

Overridden instance methods use dynamic dispatch at runtime based on the actual object's class.

---

## 50. Can overloaded methods differ only by return type?

No.

Return type alone is not sufficient to distinguish overloaded methods.

---

# 4. Static, Final and Initialization

## 51. Can a static method access instance variables directly?

No.

A static method does not have an implicit `this` reference.

It must access instance state through an object reference.

---

## 52. Can an instance method access static variables?

Yes.

Instance methods can access static members.

---

## 53. Can static blocks access instance variables directly?

No.

Static blocks execute in a class context and do not have an implicit object reference.

---

## 54. How many times does a static block execute?

Class initialization executes static initialization code once per class initialization in a given class-loader context.

---

## 55. What is the output?

```java
class Test {

    static {
        System.out.println("Static");
    }

    public static void main(String[] args) {
        System.out.println("Main");
    }
}
```

Output:

```text
Static
Main
```

---

## 56. What is initialization order?

For a typical class initialization and object creation sequence:

```text
Parent class initialization
        ↓
Child class initialization
        ↓
Parent instance initialization
        ↓
Parent constructor
        ↓
Child instance initialization
        ↓
Child constructor
```

The exact details follow Java initialization rules.

---

## 57. What happens first: static block or constructor?

Static initialization happens before an instance constructor for the class is executed.

---

## 58. Can static methods be called using an object?

Technically yes, but it is discouraged.

Example:

```java
Test obj = new Test();

obj.display();
```

If `display()` is static, the call is resolved based on the reference type rather than object polymorphism.

Prefer:

```java
Test.display();
```

---

## 59. What is a blank final variable?

A final variable declared without an initializer.

It must be assigned exactly once before use.

---

## 60. Can a final variable be initialized in a constructor?

Yes, for an instance final field.

Each constructor path must ensure it receives a value before construction completes.

---

# 5. Arrays

## 61. What is the default value of an int array?

```text
0
```

---

## 62. What is the default value of a boolean array?

```text
false
```

---

## 63. What is the default value of a reference-type array?

```text
null
```

---

## 64. What is the output?

```java
int[] arr = new int[3];

System.out.println(arr[0]);
```

Output:

```text
0
```

---

## 65. Can array size be changed after creation?

No.

Java arrays have fixed length.

---

## 66. Is an array an object?

Yes.

Arrays are objects in Java.

---

## 67. Can an array contain different data types?

A normal array has a single component type.

However, an array whose component type is a superclass or interface can hold compatible subtype objects.

Example:

```java
Object[] arr = {
    10,
    "Java",
    20.5
};
```

---

## 68. What happens when an invalid array index is accessed?

Java throws:

```text
ArrayIndexOutOfBoundsException
```

---

## 69. What is the difference between `length` and `length()`?

```text
array.length
→ field


String.length()
→ method
```

---

## 70. What is the output?

```java
int[] a = {1, 2, 3};
int[] b = a;

b[0] = 100;

System.out.println(a[0]);
```

Output:

```text
100
```

Both references point to the same array.

---

# 6. Collections

## 71. Does ArrayList store primitive types directly?

No.

Generics require reference types.

Wrapper classes are used:

```java
ArrayList<Integer>
```

Autoboxing/unboxing makes usage convenient.

---

## 72. Why can't we write this?

```java
ArrayList<int> list;
```

Generics cannot use primitive type arguments.

Use:

```java
ArrayList<Integer> list;
```

---

## 73. What is autoboxing?

Automatic conversion from a primitive to its corresponding wrapper type.

Example:

```java
Integer x = 10;
```

---

## 74. What is unboxing?

Automatic conversion from a wrapper object to its corresponding primitive type.

Example:

```java
Integer x = 10;

int y = x;
```

---

## 75. What is the output?

```java
Integer a = 100;
Integer b = 100;

System.out.println(a == b);
```

Typically:

```text
true
```

because the values are within the commonly cached Integer range.

Do not use `==` for general Integer value comparison.

---

## 76. What is the output?

```java
Integer a = 1000;
Integer b = 1000;

System.out.println(a == b);
```

Typically:

```text
false
```

because these values are generally different Integer objects.

Use:

```java
a.equals(b)
```

for value comparison.

---

## 77. What is the output?

```java
Integer a = null;

int b = a;
```

This throws:

```text
NullPointerException
```

because unboxing requires accessing the Integer's value.

---

## 78. Can HashMap contain null keys?

Yes.

A standard `HashMap` permits one null key and multiple null values.

---

## 79. Can HashSet contain null?

Yes.

A standard `HashSet` permits a null element.

---

## 80. Can TreeSet contain null?

Do not assume null is supported.

With natural ordering, inserting null generally results in:

```text
NullPointerException
```

because null cannot be compared as an ordinary element.

---

## 81. What happens if equals() is overridden but hashCode() is not?

It can break the contract required by hash-based collections.

Objects that are equal according to `equals()` must return the same hash code.

---

## 82. What is the equals-hashCode contract?

If:

```text
a.equals(b) == true
```

then:

```text
a.hashCode() == b.hashCode()
```

must also be true.

The reverse is not required.

---

## 83. Can two unequal objects have the same hash code?

Yes.

This is called a hash collision.

---

## 84. Can HashMap store duplicate keys?

No.

Adding a value for an existing key replaces the previous mapping.

---

## 85. Can HashMap store duplicate values?

Yes.

Multiple keys can map to equal values.

---

## 86. What happens if a mutable object used as a HashMap key changes after insertion?

If the mutation changes fields involved in `equals()` or `hashCode()`, the entry may become difficult or impossible to locate using the mutated key.

This is why keys should generally be immutable with respect to equality and hashing.

---

# 7. Exceptions

## 87. Can we have try without catch?

Yes, if it has a `finally` block.

```java
try {

    // code

} finally {

    // cleanup
}
```

---

## 88. Can we have try without catch and without finally?

No.

A try statement must be followed by at least one `catch` or a `finally`.

---

## 89. Can we have multiple catch blocks?

Yes.

---

## 90. What is the correct order of catch blocks?

More specific exceptions should generally appear before broader exceptions.

Example:

```java
try {

} catch (ArithmeticException e) {

} catch (Exception e) {

}
```

---

## 91. What happens if a parent exception is caught before a child exception?

The child catch becomes unreachable and causes a compilation error.

---

## 92. Does finally always execute?

Normally, `finally` executes when control leaves the associated try/catch structure.

Important exceptions include abrupt JVM termination, such as:

```java
System.exit(0);
```

or a JVM crash.

---

## 93. What happens if both try and finally return?

The `finally` return takes precedence.

Example:

```java
static int test() {

    try {
        return 10;
    } finally {
        return 20;
    }
}
```

Result:

```text
20
```

This pattern is strongly discouraged because it can hide the original return value or exception.

---

## 94. Can finally modify a returned primitive value?

If the return expression has already produced the primitive value, changing the local variable in finally does not change that saved return value.

---

## 95. Can finally modify a returned object?

It can mutate the returned object's state before the method completes.

It cannot replace the already evaluated return reference with a different reference unless the method itself returns from finally.

---

## 96. What is the difference between throw and throws?

```text
throw
→ explicitly throws an exception object


throws
→ declares exceptions that a method may propagate
```

---

## 97. Can we throw a checked exception without declaring it?

Normally, no.

A checked exception must be caught or declared, subject to Java's precise exception-analysis rules.

---

## 98. Can finally block throw an exception?

Yes.

If finally throws an exception while another exception is already propagating, the finally exception can replace the original exception.

---

# 8. Wrapper Classes

## 99. What is Integer caching?

Java's wrapper classes may cache certain values.

For Integer, values from:

```text
-128 to 127
```

are guaranteed to be cached by the Java specification.

Implementations may cache additional values.

---

## 100. What is the output?

```java
Integer a = 127;
Integer b = 127;

System.out.println(a == b);
```

Output:

```text
true
```

The values are within the guaranteed Integer cache range.

---

## 101. What is the output?

```java
Integer a = 128;
Integer b = 128;

System.out.println(a == b);
```

The result should not be relied upon as a general value comparison.

Typically:

```text
false
```

Use:

```java
a.equals(b)
```

---

## 102. What is the difference between parseInt() and valueOf()?

```text
Integer.parseInt("10")
→ returns primitive int


Integer.valueOf("10")
→ returns Integer
```

---

## 103. What does `Integer.valueOf()` do?

It returns an Integer object representing the specified value and may reuse cached instances.

---

## 104. What does `Integer.parseInt()` return?

A primitive:

```text
int
```

---

# 9. Multithreading

## 105. What happens if `run()` is called directly?

It executes in the current thread.

No new thread is created.

---

## 106. What happens if `start()` is called twice?

The second call throws:

```text
IllegalThreadStateException
```

---

## 107. Does `sleep()` release a monitor lock?

No.

---

## 108. Does `wait()` release the monitor?

Yes.

---

## 109. Can wait() be called without owning the monitor?

No.

It can result in:

```text
IllegalMonitorStateException
```

---

## 110. Is `count++` atomic?

No.

It involves multiple steps:

```text
Read
Modify
Write
```

---

## 111. Does volatile make compound operations atomic?

No.

For example:

```java
volatile int count;

count++;
```

is still not atomic.

---

## 112. Is synchronized reentrant?

Yes.

A thread can acquire the same monitor multiple times.

---

## 113. Can two threads execute synchronized instance methods on the same object simultaneously?

Not when both require the same object's monitor.

One thread must acquire the monitor first.

---

## 114. Can synchronized methods of different objects execute simultaneously?

Yes.

Different objects have different monitors.

---

## 115. Does thread priority guarantee execution order?

No.

---

## 116. Does yield() guarantee that another thread will execute?

No.

It is only a scheduling hint.

---

# 10. JVM and Memory

## 117. Is JVM memory divided into stack and heap only?

No.

The JVM has multiple runtime data areas.

Important ones include:

```text
Heap
JVM Stack
Method Area
PC Register
Native Method Stack
```

---

## 118. Is heap memory shared between threads?

Yes.

---

## 119. Is stack memory shared between threads?

No.

Each thread has its own JVM stack.

---

## 120. Can Java have memory leaks?

Yes.

Garbage collection does not remove objects that are still reachable.

---

## 121. Can System.gc() force garbage collection?

No.

It only requests or suggests that the JVM perform garbage collection.

---

## 122. What is StackOverflowError usually caused by?

A common cause is uncontrolled recursion.

Example:

```java
static void recurse() {

    recurse();
}
```

---

## 123. What is OutOfMemoryError?

It occurs when the JVM cannot satisfy a required memory allocation.

---

## 124. Is every object physically stored on the heap?

That is an oversimplification.

The Java specification does not prescribe a physical memory location for every object.

JIT optimizations can eliminate allocations or represent values differently.

---

## 125. What replaced PermGen?

Metaspace in HotSpot starting with Java 8.

---

# 11. Tricky Output Questions

## 126. What is the output?

```java
public class Test {

    static int x = 10;

    public static void main(String[] args) {

        int x = 20;

        System.out.println(x);
        System.out.println(Test.x);
    }
}
```

Output:

```text
20
10
```

The local variable shadows the static field.

---

## 127. What is the output?

```java
public class Test {

    static int x = 10;

    public static void main(String[] args) {

        {
            int x = 20;
            System.out.println(x);
        }

        System.out.println(x);
    }
}
```

Output:

```text
20
10
```

The inner local variable exists only within its scope.

---

## 128. What is the output?

```java
public class Test {

    static {
        System.out.println("A");
    }

    public static void main(String[] args) {

        System.out.println("B");
    }
}
```

Output:

```text
A
B
```

---

## 129. What is the output?

```java
class Parent {

    Parent() {
        System.out.println("Parent");
    }
}

class Child extends Parent {

    Child() {
        System.out.println("Child");
    }
}

public class Test {

    public static void main(String[] args) {

        new Child();
    }
}
```

Output:

```text
Parent
Child
```

The parent constructor executes before the child constructor.

---

## 130. What is the output?

```java
class Parent {

    void show() {
        System.out.println("Parent");
    }
}

class Child extends Parent {

    @Override
    void show() {
        System.out.println("Child");
    }
}

public class Test {

    public static void main(String[] args) {

        Parent p = new Child();

        p.show();
    }
}
```

Output:

```text
Child
```

Overridden instance methods use runtime dispatch.

---

## 131. What is the output?

```java
class Parent {

    static void show() {
        System.out.println("Parent");
    }
}

class Child extends Parent {

    static void show() {
        System.out.println("Child");
    }
}

public class Test {

    public static void main(String[] args) {

        Parent p = new Child();

        p.show();
    }
}
```

Output:

```text
Parent
```

Static methods are hidden, not dynamically overridden.

---

## 132. What is the output?

```java
public class Test {

    public static void main(String[] args) {

        String s = null;

        System.out.println(s);
    }
}
```

Output:

```text
null
```

Printing a null reference does not itself cause a NullPointerException.

---

## 133. What is the output?

```java
public class Test {

    public static void main(String[] args) {

        String s = null;

        System.out.println(s.length());
    }
}
```

Result:

```text
NullPointerException
```

Because `length()` is invoked on a null reference.

---

## 134. What is the output?

```java
public class Test {

    public static void main(String[] args) {

        Integer x = null;

        System.out.println(x + 10);
    }
}
```

Result:

```text
NullPointerException
```

The Integer must be unboxed before arithmetic.

---

## 135. What is the output?

```java
public class Test {

    public static void main(String[] args) {

        System.out.println(10 + 20 + "30");
    }
}
```

Output:

```text
3030
```

---

## 136. What is the output?

```java
public class Test {

    public static void main(String[] args) {

        System.out.println("10" + 20 + 30);
    }
}
```

Output:

```text
102030
```

---

## 137. What is the output?

```java
public class Test {

    public static void main(String[] args) {

        int x = 10;

        if (x > 5)
            if (x > 15)
                System.out.println("A");
            else
                System.out.println("B");
    }
}
```

Output:

```text
B
```

The `else` associates with the nearest unmatched `if`.

---

## 138. What is the output?

```java
public class Test {

    public static void main(String[] args) {

        for (int i = 0; i < 3; i++) {
            System.out.print(i);
        }
    }
}
```

Output:

```text
012
```

---

## 139. What is the output?

```java
public class Test {

    public static void main(String[] args) {

        int i = 0;

        while (i++ < 3) {
            System.out.print(i);
        }
    }
}
```

Output:

```text
123
```

---

## 140. What is the output?

```java
public class Test {

    public static void main(String[] args) {

        int x = 10;

        switch (x) {

            case 10:
                System.out.println("Ten");

            case 20:
                System.out.println("Twenty");

            default:
                System.out.println("Default");
        }
    }
}
```

Output:

```text
Ten
Twenty
Default
```

There is no `break`, so execution falls through.

---

## 141. What is the output?

```java
public class Test {

    public static void main(String[] args) {

        int[] arr = new int[3];

        System.out.println(arr.length);
    }
}
```

Output:

```text
3
```

---

## 142. What happens here?

```java
public class Test {

    public static void main(String[] args) {

        int[] arr = new int[3];

        System.out.println(arr[3]);
    }
}
```

Result:

```text
ArrayIndexOutOfBoundsException
```

Valid indexes are:

```text
0
1
2
```

---

# 12. Rapid-Fire Tricky Questions

## 143. Is String mutable?

```text
No.
```

---

## 144. Is String final?

```text
Yes.
```

---

## 145. Can constructor be inherited?

```text
No.
```

---

## 146. Can constructor be overridden?

```text
No.
```

---

## 147. Can constructor be overloaded?

```text
Yes.
```

---

## 148. Can abstract class have constructor?

```text
Yes.
```

---

## 149. Can abstract class have static methods?

```text
Yes.
```

---

## 150. Can interface have static methods?

```text
Yes.
```

---

## 151. Can interface have private methods?

```text
Yes.
```

---

## 152. Can interface have default methods?

```text
Yes.
```

---

## 153. Can class extend multiple classes?

```text
No.
```

---

## 154. Can class implement multiple interfaces?

```text
Yes.
```

---

## 155. Can static methods be overridden?

```text
No.
```

They are hidden.

---

## 156. Can private methods be overridden?

```text
No.
```

---

## 157. Can final methods be overridden?

```text
No.
```

---

## 158. Can final reference be reassigned?

```text
No.
```

---

## 159. Does final make an object immutable?

```text
No.
```

---

## 160. Can overloaded methods differ only by return type?

```text
No.
```

---

## 161. Can HashMap have a null key?

```text
Yes.
```

---

## 162. Can HashMap have duplicate keys?

```text
No.
```

A new value replaces the previous mapping.

---

## 163. Can HashMap have duplicate values?

```text
Yes.
```

---

## 164. Can two unequal objects have the same hash code?

```text
Yes.
```

---

## 165. Is ArrayList thread-safe?

```text
No.
```

---

## 166. Is StringBuilder thread-safe?

```text
No.
```

---

## 167. Is StringBuffer synchronized?

```text
Yes.
```

---

## 168. Does `run()` create a new thread?

```text
No.
```

---

## 169. Does `start()` create/schedule new thread execution?

```text
Yes.
```

---

## 170. Can `start()` be called twice?

```text
No.
```

---

## 171. Does `sleep()` release monitor lock?

```text
No.
```

---

## 172. Does `wait()` release monitor lock?

```text
Yes.
```

---

## 173. Is `count++` atomic?

```text
No.
```

---

## 174. Does volatile make `count++` atomic?

```text
No.
```

---

## 175. Can System.gc() force GC?

```text
No.
```

---

## 176. Can Java have memory leaks?

```text
Yes.
```

---

## 177. What replaced PermGen?

```text
Metaspace
```

in HotSpot Java 8+.

---

## 178. Is heap shared between threads?

```text
Yes.
```

---

## 179. Is JVM stack shared?

```text
No.
```

Each thread has its own JVM stack.

---

## 180. What is the most important rule for `==` with objects?

```text
== compares references.
```

For value equality, use the appropriate `equals()` implementation.

---

# 🧠 Final Tricky-Java Memory Map

```text
                         TRICKY JAVA
                              |
        +---------------------+---------------------+
        |                     |                     |
      String                 OOP                Collections
        |                     |                     |
   Immutable             Override              HashMap
   Pool                  Hide                  HashSet
   intern()              Overload              equals()
   == vs equals          final                  hashCode()
        |                     |                     |
        +---------------------+---------------------+
                              |
                         Runtime Behavior
                              |
          +-------------------+-------------------+
          |                   |                   |
      Exceptions        Multithreading           JVM
          |                   |                   |
       finally            wait/sleep           Heap
       throw              synchronized         Stack
       throws             volatile              GC
                              |
                              |
                    Output-Based Questions
                              |
                   Predict → Trace → Answer
```

---

# ⚡ Golden Rules for Tricky Java Questions

```text
1. == checks references for objects.

2. equals() checks logical equality according to the class.

3. String is immutable.

4. String literals are interned.

5. Constructors are not inherited or overridden.

6. Static methods are hidden, not overridden.

7. Private methods are not overridden.

8. Final methods cannot be overridden.

9. Return type alone cannot overload a method.

10. final reference ≠ immutable object.

11. count++ is not atomic.

12. volatile ≠ synchronization.

13. sleep() does not release a monitor.

14. wait() releases the monitor.

15. run() does not create a new thread.

16. start() can be called only once on a Thread.

17. HashMap allows one null key.

18. Equal objects must have equal hash codes.

19. System.gc() does not guarantee garbage collection.

20. Heap is shared; each thread has its own JVM stack.

21. Array length is fixed after creation.

22. Integer wrapper comparison with == should not be used for general value comparison.

23. Autoboxing/unboxing can cause NullPointerException.

24. finally can override a return or exception if it itself returns or throws.

25. When solving output questions, trace execution line by line instead of guessing.
```

---

