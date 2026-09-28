# 🎯 Java Output-Based Questions — Interview Questions & Answers

> **A collection of Java output-prediction interview questions covering core syntax, OOP, strings, collections, exceptions, inheritance, static/final behavior, and tricky execution flow.**

---

# 📑 Table of Contents

- [1. Basics](#1-basics)
- [2. Operators](#2-operators)
- [3. Strings](#3-strings)
- [4. Arrays](#4-arrays)
- [5. OOP](#5-oop)
- [6. Static and Initialization](#6-static-and-initialization)
- [7. Inheritance and Polymorphism](#7-inheritance-and-polymorphism)
- [8. Exceptions](#8-exceptions)
- [9. Wrappers and Autoboxing](#9-wrappers-and-autoboxing)
- [10. Collections](#10-collections)
- [11. Loops and Control Flow](#11-loops-and-control-flow)
- [12. Multithreading](#12-multithreading)
- [13. Advanced Output Questions](#13-advanced-output-questions)
- [14. Rapid-Fire Output Revision](#14-rapid-fire-output-revision)

---

# 1. Basics

## 1. What is the output?

```java
public class Test {
    public static void main(String[] args) {
        int a = 10;
        int b = 20;

        System.out.println(a + b);
    }
}
```

### Answer

```text
30
```

---

## 2. What is the output?

```java
public class Test {
    public static void main(String[] args) {
        int a = 10;

        System.out.println(a++);
        System.out.println(a);
    }
}
```

### Answer

```text
10
11
```

Post-increment uses the current value first and then increments it.

---

## 3. What is the output?

```java
public class Test {
    public static void main(String[] args) {
        int a = 10;

        System.out.println(++a);
        System.out.println(a);
    }
}
```

### Answer

```text
11
11
```

---

## 4. What is the output?

```java
public class Test {
    public static void main(String[] args) {
        int x = 5;

        System.out.println(x++ + ++x);
    }
}
```

### Answer

```text
12
```

Evaluation:

```text
x++ → 5, x becomes 6
++x → x becomes 7, value = 7

5 + 7 = 12
```

---

## 5. What is the output?

```java
public class Test {
    public static void main(String[] args) {
        int x = 10;

        System.out.println(x++ + x++ + ++x);
    }
}
```

### Answer

```text
33
```

Evaluation:

```text
x++ → 10, x = 11
x++ → 11, x = 12
++x → 13

10 + 11 + 13 = 34
```

**Correction:** The actual output is:

```text
34
```

---

# 2. Operators

## 6. What is the output?

```java
public class Test {
    public static void main(String[] args) {
        System.out.println(10 + 20 + "Java");
    }
}
```

### Answer

```text
30Java
```

The arithmetic happens before String concatenation.

---

## 7. What is the output?

```java
public class Test {
    public static void main(String[] args) {
        System.out.println("Java" + 10 + 20);
    }
}
```

### Answer

```text
Java1020
```

Once String concatenation begins, subsequent values are converted to String.

---

## 8. What is the output?

```java
public class Test {
    public static void main(String[] args) {
        System.out.println(10 + 20 + "Java" + 30 + 40);
    }
}
```

### Answer

```text
30Java3040
```

---

## 9. What is the output?

```java
public class Test {
    public static void main(String[] args) {
        System.out.println(10 / 3);
        System.out.println(10 / 3.0);
    }
}
```

### Answer

```text
3
3.3333333333333335
```

---

## 10. What is the output?

```java
public class Test {
    public static void main(String[] args) {
        int x = 10;

        if (x > 5 && x++ > 10) {
            System.out.println("A");
        }

        System.out.println(x);
    }
}
```

### Answer

```text
10
```

The first condition is true, so the second condition is evaluated.

Actually:

```text
x > 5 → true
x++ > 10 → 10 > 10 → false
x becomes 11
```

Therefore the actual output is:

```text
11
```

---

## 11. What is the output?

```java
public class Test {
    public static void main(String[] args) {
        int x = 10;

        if (x < 5 && x++ > 10) {
            System.out.println("A");
        }

        System.out.println(x);
    }
}
```

### Answer

```text
10
```

Because `&&` short-circuits.

The second condition is never evaluated.

---

## 12. What is the output?

```java
public class Test {
    public static void main(String[] args) {
        int x = 10;

        if (x > 5 || x++ > 10) {
            System.out.println("A");
        }

        System.out.println(x);
    }
}
```

### Answer

```text
A
10
```

The second condition is skipped because the first condition is already true.

---

## 13. What is the output?

```java
public class Test {
    public static void main(String[] args) {
        int x = 5;

        System.out.println(x << 1);
    }
}
```

### Answer

```text
10
```

Left shifting by one position is equivalent to multiplying by 2 for this positive value.

---

## 14. What is the output?

```java
public class Test {
    public static void main(String[] args) {
        int x = 10;

        System.out.println(x >> 1);
    }
}
```

### Answer

```text
5
```

---

# 3. Strings

## 15. What is the output?

```java
public class Test {
    public static void main(String[] args) {
        String a = "Java";
        String b = "Java";

        System.out.println(a == b);
    }
}
```

### Answer

```text
true
```

Both are references to the same interned String literal.

---

## 16. What is the output?

```java
public class Test {
    public static void main(String[] args) {
        String a = new String("Java");
        String b = new String("Java");

        System.out.println(a == b);
    }
}
```

### Answer

```text
false
```

Two different String objects are created.

---

## 17. What is the output?

```java
public class Test {
    public static void main(String[] args) {
        String a = new String("Java");
        String b = new String("Java");

        System.out.println(a.equals(b));
    }
}
```

### Answer

```text
true
```

`equals()` compares String contents.

---

## 18. What is the output?

```java
public class Test {
    public static void main(String[] args) {
        String a = "Java";
        String b = "Ja" + "va";

        System.out.println(a == b);
    }
}
```

### Answer

```text
true
```

The concatenation consists entirely of compile-time constants.

---

## 19. What is the output?

```java
public class Test {
    public static void main(String[] args) {
        String a = "Ja";
        String b = "va";
        String c = a + b;
        String d = "Java";

        System.out.println(c == d);
    }
}
```

### Answer

```text
false
```

The concatenation occurs at runtime and creates a different String object.

---

## 20. What is the output?

```java
public class Test {
    public static void main(String[] args) {
        String s = "Java";

        s.concat(" Programming");

        System.out.println(s);
    }
}
```

### Answer

```text
Java
```

Strings are immutable.

---

## 21. What is the output?

```java
public class Test {
    public static void main(String[] args) {
        String s = "Java";

        s = s.concat(" Programming");

        System.out.println(s);
    }
}
```

### Answer

```text
Java Programming
```

---

## 22. What is the output?

```java
public class Test {
    public static void main(String[] args) {
        String s = null;

        System.out.println(s);
    }
}
```

### Answer

```text
null
```

---

## 23. What happens here?

```java
public class Test {
    public static void main(String[] args) {
        String s = null;

        System.out.println(s.length());
    }
}
```

### Answer

```text
NullPointerException
```

---

# 4. Arrays

## 24. What is the output?

```java
public class Test {
    public static void main(String[] args) {
        int[] arr = new int[3];

        System.out.println(arr[0]);
    }
}
```

### Answer

```text
0
```

---

## 25. What is the output?

```java
public class Test {
    public static void main(String[] args) {
        int[] arr = {10, 20, 30};

        System.out.println(arr.length);
    }
}
```

### Answer

```text
3
```

---

## 26. What is the output?

```java
public class Test {
    public static void main(String[] args) {
        int[] arr = {10, 20, 30};

        int[] b = arr;

        b[0] = 100;

        System.out.println(arr[0]);
    }
}
```

### Answer

```text
100
```

Both references point to the same array.

---

## 27. What is the output?

```java
public class Test {
    public static void main(String[] args) {
        int[] arr = {1, 2, 3};

        for (int i : arr) {
            System.out.print(i);
        }
    }
}
```

### Answer

```text
123
```

---

## 28. What happens here?

```java
public class Test {
    public static void main(String[] args) {
        int[] arr = {1, 2, 3};

        System.out.println(arr[3]);
    }
}
```

### Answer

```text
ArrayIndexOutOfBoundsException
```

The valid indexes are:

```text
0
1
2
```

---

## 29. What is the output?

```java
public class Test {
    public static void main(String[] args) {
        Object[] arr = {
            10,
            "Java",
            20.5
        };

        for (Object value : arr) {
            System.out.println(value);
        }
    }
}
```

### Answer

```text
10
Java
20.5
```

---

# 5. OOP

## 30. What is the output?

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

### Answer

```text
Child
```

Overridden instance methods use runtime polymorphism.

---

## 31. What is the output?

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

### Answer

```text
Parent
```

Static methods are hidden, not overridden.

---

## 32. What is the output?

```java
class Parent {

    void show() {
        System.out.println("Parent");
    }
}

class Child extends Parent {

    void show(int x) {
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

### Answer

```text
Parent
```

`show(int)` is an overloaded method, not an override of `show()`.

---

## 33. What is the output?

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

### Answer

```text
Parent
Child
```

The parent constructor executes before the child constructor.

---

## 34. What is the output?

```java
class Parent {

    int x = 10;
}

class Child extends Parent {

    int x = 20;
}

public class Test {

    public static void main(String[] args) {
        Parent p = new Child();

        System.out.println(p.x);
    }
}
```

### Answer

```text
10
```

Fields are not polymorphic in the same way overridden instance methods are.

Field access is resolved using the reference type.

---

## 35. What is the output?

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

    void test() {
        super.show();
    }
}

public class Test {

    public static void main(String[] args) {
        Child c = new Child();

        c.test();
    }
}
```

### Answer

```text
Parent
```

`super.show()` explicitly invokes the parent implementation.

---

# 6. Static and Initialization

## 36. What is the output?

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

### Answer

```text
Static
Main
```

---

## 37. What is the output?

```java
class Test {

    static int x = 10;

    static {
        x = 20;
    }

    public static void main(String[] args) {
        System.out.println(x);
    }
}
```

### Answer

```text
20
```

---

## 38. What is the output?

```java
class Test {

    static int x;

    static {
        x = 10;
    }

    static {
        x = 20;
    }

    public static void main(String[] args) {
        System.out.println(x);
    }
}
```

### Answer

```text
20
```

Static initialization occurs in source order.

---

## 39. What is the output?

```java
class Test {

    int x = 10;

    Test() {
        System.out.println(x);
    }

    public static void main(String[] args) {
        new Test();
    }
}
```

### Answer

```text
10
```

The instance field initializer executes before the constructor body.

---

## 40. What is the output?

```java
class Test {

    int x = 10;

    {
        x = 20;
    }

    Test() {
        System.out.println(x);
    }

    public static void main(String[] args) {
        new Test();
    }
}
```

### Answer

```text
20
```

The instance initializer executes before the constructor body.

---

## 41. What is the output?

```java
class Test {

    static int x = 10;

    public static void main(String[] args) {
        int x = 20;

        System.out.println(x);
        System.out.println(Test.x);
    }
}
```

### Answer

```text
20
10
```

The local variable shadows the static field.

---

# 7. Inheritance and Polymorphism

## 42. What is the output?

```java
class A {

    void show() {
        System.out.println("A");
    }
}

class B extends A {

    @Override
    void show() {
        System.out.println("B");
    }
}

class C extends B {

    @Override
    void show() {
        System.out.println("C");
    }
}

public class Test {

    public static void main(String[] args) {
        A obj = new C();

        obj.show();
    }
}
```

### Answer

```text
C
```

Runtime dispatch selects the most specific overridden implementation.

---

## 43. What is the output?

```java
class A {

    static void show() {
        System.out.println("A");
    }
}

class B extends A {

    static void show() {
        System.out.println("B");
    }
}

class C extends B {

    static void show() {
        System.out.println("C");
    }
}

public class Test {

    public static void main(String[] args) {
        A obj = new C();

        obj.show();
    }
}
```

### Answer

```text
A
```

Static method calls are resolved using the reference type.

---

## 44. What is the output?

```java
class Parent {

    Parent() {
        show();
    }

    void show() {
        System.out.println("Parent");
    }
}

class Child extends Parent {

    int x = 10;

    @Override
    void show() {
        System.out.println(x);
    }
}

public class Test {

    public static void main(String[] args) {
        new Child();
    }
}
```

### Answer

```text
0
```

The parent constructor executes before the child instance field initializer.

At the time `show()` is dynamically dispatched to `Child.show()`, `x` still has its default value:

```text
0
```

This is one reason calling overridable methods from constructors is dangerous.

---

## 45. What is the output?

```java
class Parent {

    int x = 10;
}

class Child extends Parent {

    int x = 20;

    void print() {
        System.out.println(x);
        System.out.println(super.x);
    }
}

public class Test {

    public static void main(String[] args) {
        new Child().print();
    }
}
```

### Answer

```text
20
10
```

---

# 8. Exceptions

## 46. What is the output?

```java
public class Test {

    public static void main(String[] args) {

        try {
            System.out.println("Try");
        } catch (Exception e) {
            System.out.println("Catch");
        } finally {
            System.out.println("Finally");
        }
    }
}
```

### Answer

```text
Try
Finally
```

---

## 47. What is the output?

```java
public class Test {

    public static void main(String[] args) {

        try {
            System.out.println(10 / 0);
        } catch (ArithmeticException e) {
            System.out.println("Catch");
        } finally {
            System.out.println("Finally");
        }
    }
}
```

### Answer

```text
Catch
Finally
```

---

## 48. What is the output?

```java
public class Test {

    public static void main(String[] args) {

        try {
            System.out.println("Try");
            return;
        } finally {
            System.out.println("Finally");
        }
    }
}
```

### Answer

```text
Try
Finally
```

`finally` executes before the method returns.

---

## 49. What is the output?

```java
public class Test {

    static int test() {

        try {
            return 10;
        } finally {
            return 20;
        }
    }

    public static void main(String[] args) {
        System.out.println(test());
    }
}
```

### Answer

```text
20
```

The return from `finally` overrides the pending return.

Avoid this pattern in real code.

---

## 50. What is the output?

```java
public class Test {

    static int test() {

        int x = 10;

        try {
            return x;
        } finally {
            x = 20;
        }
    }

    public static void main(String[] args) {
        System.out.println(test());
    }
}
```

### Answer

```text
10
```

The primitive return value has already been evaluated before `finally` modifies `x`.

---

## 51. What happens here?

```java
public class Test {

    public static void main(String[] args) {

        try {
            int x = 10 / 0;
        } catch (Exception e) {
            System.out.println("Exception");
        }
    }
}
```

### Answer

```text
Exception
```

`ArithmeticException` is a subclass of `Exception`.

---

# 9. Wrappers and Autoboxing

## 52. What is the output?

```java
public class Test {

    public static void main(String[] args) {

        Integer a = 100;
        Integer b = 100;

        System.out.println(a == b);
    }
}
```

### Answer

```text
true
```

The value is within the guaranteed Integer cache range.

---

## 53. What is the output?

```java
public class Test {

    public static void main(String[] args) {

        Integer a = 1000;
        Integer b = 1000;

        System.out.println(a == b);
    }
}
```

### Answer

Typically:

```text
false
```

`==` compares object references.

---

## 54. What is the output?

```java
public class Test {

    public static void main(String[] args) {

        Integer a = 1000;
        Integer b = 1000;

        System.out.println(a.equals(b));
    }
}
```

### Answer

```text
true
```

`equals()` compares Integer values.

---

## 55. What happens here?

```java
public class Test {

    public static void main(String[] args) {

        Integer x = null;

        int y = x;
    }
}
```

### Answer

```text
NullPointerException
```

Unboxing requires retrieving the primitive value from the Integer object.

---

## 56. What is the output?

```java
public class Test {

    public static void main(String[] args) {

        Integer x = 10;

        System.out.println(x + 20);
    }
}
```

### Answer

```text
30
```

The Integer is automatically unboxed.

---

# 10. Collections

## 57. What is the output?

```java
import java.util.ArrayList;

public class Test {

    public static void main(String[] args) {

        ArrayList<Integer> list = new ArrayList<>();

        list.add(10);
        list.add(20);
        list.add(10);

        System.out.println(list.size());
    }
}
```

### Answer

```text
3
```

ArrayList allows duplicate elements.

---

## 58. What is the output?

```java
import java.util.HashSet;

public class Test {

    public static void main(String[] args) {

        HashSet<Integer> set = new HashSet<>();

        set.add(10);
        set.add(20);
        set.add(10);

        System.out.println(set.size());
    }
}
```

### Answer

```text
2
```

A Set does not allow duplicate elements according to its equality semantics.

---

## 59. What is the output?

```java
import java.util.HashMap;

public class Test {

    public static void main(String[] args) {

        HashMap<Integer, String> map = new HashMap<>();

        map.put(1, "Java");
        map.put(1, "Spring");

        System.out.println(map.get(1));
    }
}
```

### Answer

```text
Spring
```

The second value replaces the first mapping for the same key.

---

## 60. What is the output?

```java
import java.util.HashMap;

public class Test {

    public static void main(String[] args) {

        HashMap<Integer, String> map = new HashMap<>();

        map.put(null, "Java");

        System.out.println(map.get(null));
    }
}
```

### Answer

```text
Java
```

HashMap permits a null key.

---

## 61. What is the output?

```java
import java.util.HashMap;

public class Test {

    public static void main(String[] args) {

        HashMap<String, Integer> map = new HashMap<>();

        map.put("Java", 10);
        map.put("Spring", 20);

        System.out.println(map.size());
    }
}
```

### Answer

```text
2
```

---

## 62. What is the output?

```java
import java.util.ArrayList;

public class Test {

    public static void main(String[] args) {

        ArrayList<Integer> list = new ArrayList<>();

        list.add(10);
        list.add(20);

        System.out.println(list.remove(1));
    }
}
```

### Answer

```text
20
```

For `ArrayList<Integer>`, `remove(int)` treats `1` as an index.

---

## 63. What is the output?

```java
import java.util.ArrayList;

public class Test {

    public static void main(String[] args) {

        ArrayList<Integer> list = new ArrayList<>();

        list.add(10);
        list.add(20);
        list.add(30);

        list.remove(Integer.valueOf(20));

        System.out.println(list);
    }
}
```

### Answer

```text
[10, 30]
```

Here the Integer object overload is used, so the value `20` is removed.

---

# 11. Loops and Control Flow

## 64. What is the output?

```java
public class Test {

    public static void main(String[] args) {

        for (int i = 0; i < 3; i++) {
            System.out.print(i);
        }
    }
}
```

### Answer

```text
012
```

---

## 65. What is the output?

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

### Answer

```text
123
```

---

## 66. What is the output?

```java
public class Test {

    public static void main(String[] args) {

        for (int i = 0; i < 5; i++) {

            if (i == 3) {
                break;
            }

            System.out.print(i);
        }
    }
}
```

### Answer

```text
012
```

---

## 67. What is the output?

```java
public class Test {

    public static void main(String[] args) {

        for (int i = 0; i < 5; i++) {

            if (i == 3) {
                continue;
            }

            System.out.print(i);
        }
    }
}
```

### Answer

```text
0124
```

---

## 68. What is the output?

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

### Answer

```text
B
```

The `else` belongs to the nearest unmatched `if`.

---

## 69. What is the output?

```java
public class Test {

    public static void main(String[] args) {

        int x = 10;

        switch (x) {

            case 10:
                System.out.println("A");

            case 20:
                System.out.println("B");

            default:
                System.out.println("C");
        }
    }
}
```

### Answer

```text
A
B
C
```

There is no `break`, so execution falls through.

---

## 70. What is the output?

```java
public class Test {

    public static void main(String[] args) {

        int x = 20;

        switch (x) {

            case 10:
                System.out.println("A");
                break;

            case 20:
                System.out.println("B");
                break;

            default:
                System.out.println("C");
        }
    }
}
```

### Answer

```text
B
```

---

# 12. Multithreading

## 71. What is the output?

```java
public class Test {

    public static void main(String[] args) {

        Thread t = new Thread(() -> {
            System.out.println("Child");
        });

        t.run();

        System.out.println("Main");
    }
}
```

### Answer

```text
Child
Main
```

`run()` executes like a normal method in the current thread.

---

## 72. Does this always print `Child` then `Main`?

```java
public class Test {

    public static void main(String[] args) {

        Thread t = new Thread(() -> {
            System.out.println("Child");
        });

        t.start();

        System.out.println("Main");
    }
}
```

### Answer

No fixed order is guaranteed.

Possible output:

```text
Child
Main
```

or:

```text
Main
Child
```

Thread scheduling determines the order.

---

## 73. What is the output?

```java
public class Test {

    public static void main(String[] args)
            throws InterruptedException {

        Thread t = new Thread(() -> {
            System.out.println("Child");
        });

        t.start();
        t.join();

        System.out.println("Main");
    }
}
```

### Answer

```text
Child
Main
```

`join()` makes the main thread wait for `t` to finish.

---

## 74. What happens if `start()` is called twice?

```java
Thread t = new Thread();

t.start();
t.start();
```

### Answer

The second `start()` throws:

```text
IllegalThreadStateException
```

---

## 75. Does `sleep()` release the monitor?

No.

A sleeping thread retains any monitor locks it already owns.

---

## 76. Does `wait()` release the monitor?

Yes.

The thread releases the monitor associated with the object and waits to be notified/interrupted.

---

# 13. Advanced Output Questions

## 77. What is the output?

```java
class A {

    static {
        System.out.println("A Static");
    }
}

class B extends A {

    static {
        System.out.println("B Static");
    }
}

public class Test {

    public static void main(String[] args) {

        B obj = new B();
    }
}
```

### Answer

```text
A Static
B Static
```

The superclass is initialized before the subclass.

---

## 78. What is the output?

```java
class A {

    {
        System.out.println("A Instance");
    }

    A() {
        System.out.println("A Constructor");
    }
}

class B extends A {

    {
        System.out.println("B Instance");
    }

    B() {
        System.out.println("B Constructor");
    }
}

public class Test {

    public static void main(String[] args) {

        new B();
    }
}
```

### Answer

```text
A Instance
A Constructor
B Instance
B Constructor
```

---

## 79. What is the output?

```java
class A {

    static int x = 10;

    static {
        x++;
    }
}

public class Test {

    public static void main(String[] args) {

        System.out.println(A.x);
    }
}
```

### Answer

```text
11
```

---

## 80. What is the output?

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

### Answer

```text
20
10
```

---

## 81. What is the output?

```java
public class Test {

    static int test() {

        try {
            return 1;
        } catch (Exception e) {
            return 2;
        } finally {
            System.out.println("Finally");
        }
    }

    public static void main(String[] args) {

        System.out.println(test());
    }
}
```

### Answer

```text
Finally
1
```

---

## 82. What is the output?

```java
public class Test {

    public static void main(String[] args) {

        String a = "Java";
        String b = new String("Java");

        System.out.println(a == b);
        System.out.println(a.equals(b));
    }
}
```

### Answer

```text
false
true
```

---

## 83. What is the output?

```java
public class Test {

    public static void main(String[] args) {

        String a = "Java";
        String b = new String("Java");

        System.out.println(a == b.intern());
    }
}
```

### Answer

```text
true
```

`intern()` returns the canonical pooled representation.

---

## 84. What is the output?

```java
public class Test {

    public static void main(String[] args) {

        int x = 10;

        System.out.println(x > 5 ? "Yes" : "No");
    }
}
```

### Answer

```text
Yes
```

---

## 85. What is the output?

```java
public class Test {

    public static void main(String[] args) {

        int x = 10;
        int y = 20;

        System.out.println(x > y ? x : y);
    }
}
```

### Answer

```text
20
```

---

## 86. What is the output?

```java
public class Test {

    public static void main(String[] args) {

        int x = 10;

        System.out.println(x == 10 ? x++ : ++x);
        System.out.println(x);
    }
}
```

### Answer

```text
10
11
```

The condition is true, so `x++` executes.

---

## 87. What is the output?

```java
public class Test {

    public static void main(String[] args) {

        int x = 10;

        if (x++ == 10) {
            System.out.println(x);
        }
    }
}
```

### Answer

```text
11
```

The comparison uses `10`, then `x` becomes `11`.

---

## 88. What is the output?

```java
public class Test {

    public static void main(String[] args) {

        int x = 10;

        if (++x == 10) {
            System.out.println("A");
        } else {
            System.out.println(x);
        }
    }
}
```

### Answer

```text
11
```

---

## 89. What is the output?

```java
public class Test {

    public static void main(String[] args) {

        int x = 10;

        System.out.println(x++ == 10);
        System.out.println(x);
    }
}
```

### Answer

```text
true
11
```

---

## 90. What is the output?

```java
public class Test {

    public static void main(String[] args) {

        int x = 10;

        System.out.println(++x == 10);
        System.out.println(x);
    }
}
```

### Answer

```text
false
11
```

---

# 14. Rapid-Fire Output Revision

## 91. `"Java" == "Java"`?

```text
true
```

---

## 92. `new String("Java") == new String("Java")`?

```text
false
```

---

## 93. `new String("Java").equals(new String("Java"))`?

```text
true
```

---

## 94. `10 + 20 + "Java"`?

```text
30Java
```

---

## 95. `"Java" + 10 + 20`?

```text
Java1020
```

---

## 96. `10 / 3`?

```text
3
```

---

## 97. `10 / 3.0`?

```text
3.3333333333333335
```

---

## 98. Default value of `int[]` element?

```text
0
```

---

## 99. Default value of `boolean[]` element?

```text
false
```

---

## 100. Default value of reference array element?

```text
null
```

---

## 101. `array.length`?

```text
Array size
```

---

## 102. `String.length()`?

```text
Number of characters
```

---

## 103. Overridden instance method?

```text
Runtime dispatch
```

---

## 104. Static method?

```text
Static binding / method hiding
```

---

## 105. Field access in inheritance?

```text
Based on reference type
```

---

## 106. `run()`?

```text
Normal method call
```

---

## 107. `start()`?

```text
Starts a new thread
```

---

## 108. `sleep()` releases monitor?

```text
No
```

---

## 109. `wait()` releases monitor?

```text
Yes
```

---

## 110. `System.gc()` guarantees GC?

```text
No
```

---

## 111. `count++` atomic?

```text
No
```

---

## 112. `volatile` makes `count++` atomic?

```text
No
```

---

## 113. HashMap duplicate key?

```text
Previous value is replaced
```

---

## 114. HashSet duplicate element?

```text
Not added
```

---

## 115. Integer `100 == 100`?

```text
true
```

for cached Integer objects.

---

## 116. Integer `1000 == 1000`?

```text
Typically false
```

Do not use `==` for general wrapper-value comparison.

---

## 117. `Integer null` unboxed to `int`?

```text
NullPointerException
```

---

## 118. `finally` after `return`?

```text
Normally executes before return completes
```

---

## 119. `return` inside `finally`?

```text
Overrides the pending return
```

---

## 120. Main rule for output questions?

```text
Never guess.

Trace:
1. Initial values
2. Evaluation order
3. Variable changes
4. Method dispatch
5. Object/reference relationships
6. Exceptions
7. Final output
```

---

# 🧠 Output Question Solving Formula

```text
                OUTPUT QUESTION
                       |
                       ↓
              Read the code once
                       |
                       ↓
              Identify tricky part
                       |
          +------------+------------+
          |            |            |
       ++ / --       == / equals   OOP
          |            |            |
       Trace         Reference      Binding
       values        vs value       rules
          |            |            |
          +------------+------------+
                       |
                       ↓
                 Execute mentally
                       |
                       ↓
                Check exceptions
                       |
                       ↓
                   OUTPUT
```

---

# ⚡ Golden Rules

```text
1. Trace post-increment after using the old value.

2. Trace pre-increment before using the value.

3. String literals are commonly interned.

4. == compares object references.

5. equals() compares logical values when properly overridden.

6. String is immutable.

7. Instance methods use dynamic dispatch.

8. Static methods are hidden.

9. Fields are not polymorphic.

10. Parent initialization happens before child initialization.

11. Parent constructor executes before child constructor.

12. finally normally executes before method completion.

13. A return inside finally can replace an earlier return.

14. Array indexes start from 0.

15. Array length is fixed.

16. HashMap replaces a value when the same key is inserted.

17. HashSet does not allow duplicate elements.

18. Integer wrapper objects should be compared with equals(), not ==.

19. Unboxing null causes NullPointerException.

20. run() does not create a new thread.

21. start() creates/schedules new thread execution.

22. sleep() does not release a monitor.

23. wait() releases the object's monitor.

24. volatile does not make compound operations atomic.

25. When output is uncertain because of thread scheduling, do not assume a fixed order.

26. For output questions, follow execution—not intuition.
```

