# Object Class in Java

## 1. Introduction

`Object` is the **root class of the Java class hierarchy**.

It belongs to:

```java
java.lang.Object
```

Every Java class directly or indirectly inherits from `Object`.

Example:

```java
class Student {
}
```

Conceptually:

```text
Student
   ↓
Object
```

Even though we don't explicitly write:

```java
class Student extends Object {
}
```

Java automatically treats the class as extending `Object`.

Therefore:

```java
Student s = new Student();
```

has access to methods inherited from `Object`.

---

# 2. Why Does Object Class Exist?

Java needs a common parent for objects.

If every class ultimately derives from `Object`, Java can provide common operations that work with objects of any class.

For example:

```java
Object obj = new Student();
Object obj2 = new String("Hello");
Object obj3 = new Integer(10);
```

All of these are valid because:

```text
Student ───────┐
String ────────┤
Integer ───────┤
                ↓
             Object
```

This provides a common type for all Java objects.

---

# 3. Object Class Package

`Object` is inside the `java.lang` package.

```java
java.lang.Object
```

`java.lang` is automatically imported by Java.

Therefore, we don't need:

```java
import java.lang.Object;
```

We can simply use:

```java
Object obj;
```

---

# 4. Object Class Hierarchy

The basic hierarchy is:

```text
                    Object
                      │
        ┌─────────────┼─────────────┐
        │             │             │
     String        Student       Employee
        │
   subclasses...
```

More generally:

```text
Object
  ↑
Any Java Class
  ↑
Your Classes
```

For example:

```java
class Animal {
}

class Dog extends Animal {
}
```

The complete inheritance chain is:

```text
Dog
 ↓
Animal
 ↓
Object
```

Therefore:

```java
Dog d = new Dog();
```

has inherited `Object` behavior.

---

# 5. Is Object a Class or Interface?

`Object` is a **class**.

```java
public class Object
```

It is not:

```text
Interface ❌
Abstract class ❌
Final class ❌
```

It is the root class of the Java class hierarchy.

---

# 6. Does Every Class Extend Object?

Almost every ordinary Java class ultimately extends `Object`.

Example:

```java
class Student {
}
```

is effectively:

```java
class Student extends Object {
}
```

If there is inheritance:

```java
class Animal {
}

class Dog extends Animal {
}
```

then:

```text
Dog
 ↓
Animal
 ↓
Object
```

`Dog` indirectly inherits from `Object`.

---

# 7. What About Interfaces?

An interface does **not** extend `Object`.

For example:

```java
interface Animal {
}
```

This is NOT:

```text
Animal
  ↓
Object
```

However, objects of classes implementing the interface still ultimately inherit from `Object`.

Example:

```java
interface Animal {
}

class Dog implements Animal {
}
```

Hierarchy:

```text
        Object
          ↑
         Dog
          ↑
       Animal
      (interface)
```

More accurately, `Dog` extends `Object` and implements `Animal`.

Important interview point:

> An interface does not inherit from `Object`, but every class implementing an interface still inherits from `Object`.

---

# 8. Object Reference

Because `Object` is the root class, an `Object` reference can refer to an instance of any class.

Example:

```java
Object obj;

obj = new Student();
obj = new String("Hello");
obj = new Integer(100);
```

This is possible because:

```text
Student ──┐
String ───┤
Integer ──┤
          ↓
       Object
```

This is an example of **upcasting**.

---

# 9. Object as a Method Parameter

You can accept an object of any class using `Object`.

```java
static void print(Object obj) {

    System.out.println(obj);
}
```

Now:

```java
print("Hello");
print(100);
print(new Student());
```

All are valid.

Why?

Because all these objects are ultimately descendants of `Object`.

---

# 10. Object as a Return Type

A method can return `Object`.

```java
static Object getValue() {

    return "Hello";
}
```

It can also return:

```java
static Object getValue() {

    return new Student();
}
```

The actual object can be different, but the reference returned is typed as `Object`.

---

# 11. Important Methods of Object

The `Object` class provides several important methods.

The most important ones for interviews are:

```text
toString()
equals()
hashCode()
getClass()
clone()
wait()
notify()
notifyAll()
```

Historically, `finalize()` was also part of the discussion around `Object`, but it is deprecated for removal and should not be used for modern resource management.

---

# 12. `toString()`

`toString()` returns a string representation of an object.

Declaration:

```java
public String toString()
```

Example:

```java
class Student {

    String name = "Divyansh";
}
```

Now:

```java
Student s = new Student();

System.out.println(s.toString());
```

If `Student` doesn't override `toString()`, the inherited implementation from `Object` produces a string based on the object's class name and identity hash information.

It commonly looks like:

```text
Student@5e91993f
```

The exact value is not guaranteed.

---

# 13. Why Override `toString()`?

Instead of:

```text
Student@5e91993f
```

we can provide meaningful information.

```java
class Student {

    String name;
    int age;

    @Override
    public String toString() {

        return "Student{name='"
                + name
                + "', age="
                + age
                + "}";
    }
}
```

Now:

```java
Student s = new Student();

s.name = "Divyansh";
s.age = 22;

System.out.println(s);
```

Output:

```text
Student{name='Divyansh', age=22}
```

Notice:

```java
System.out.println(s);
```

internally invokes string conversion using `toString()`.

---

# 14. `equals()`

`equals()` is used to compare objects for **logical equality**.

Declaration:

```java
public boolean equals(Object obj)
```

Example:

```java
String a = new String("Java");
String b = new String("Java");

System.out.println(a.equals(b));
```

Output:

```text
true
```

`String` overrides `equals()` to compare the contents.

---

# 15. `==` vs `equals()`

This is one of the most important Java interview topics.

### `==`

For object references, `==` compares whether the two references refer to the same object.

```java
Student s1 = new Student();
Student s2 = new Student();

System.out.println(s1 == s2);
```

Usually:

```text
false
```

because they are different objects.

### `equals()`

Checks logical equality according to the class's implementation.

```java
s1.equals(s2);
```

If `Student` hasn't overridden `equals()`, it inherits `Object.equals()`, whose behavior is effectively based on reference identity.

---

# 16. Default `Object.equals()`

The `Object` implementation of `equals()` is based on reference identity.

Conceptually:

```java
public boolean equals(Object obj) {

    return this == obj;
}
```

Therefore:

```java
Student s1 = new Student();
Student s2 = new Student();

System.out.println(s1.equals(s2));
```

returns:

```text
false
```

unless `Student` overrides `equals()`.

---

# 17. Overriding `equals()`

Suppose:

```java
class Student {

    int id;

    Student(int id) {
        this.id = id;
    }

    @Override
    public boolean equals(Object obj) {

        if (this == obj) {
            return true;
        }

        if (!(obj instanceof Student)) {
            return false;
        }

        Student other = (Student) obj;

        return this.id == other.id;
    }
}
```

Now:

```java
Student s1 = new Student(101);
Student s2 = new Student(101);

System.out.println(s1.equals(s2));
```

Output:

```text
true
```

because both students have the same logical identity according to our implementation.

---

# 18. `hashCode()`

`hashCode()` returns an integer hash value representing the object.

Declaration:

```java
public int hashCode()
```

Example:

```java
Student s = new Student();

System.out.println(s.hashCode());
```

Hash codes are especially important for hash-based collections:

```text
HashMap
HashSet
Hashtable
```

---

# 19. The `equals()` and `hashCode()` Contract

This is extremely important.

If:

```java
a.equals(b)
```

is `true`, then:

```java
a.hashCode() == b.hashCode()
```

must also be true.

In short:

```text
equals() == true
        ↓
same hashCode()
```

But the reverse is NOT guaranteed.

```text
same hashCode()
        ↓
does NOT necessarily mean
equals() == true
```

This is because different objects can have the same hash code.

---

# 20. Why Is This Important?

Consider:

```java
HashSet<Student> set;
```

A hash-based collection uses hashing to efficiently locate possible matches.

Conceptually:

```text
Object
  ↓
hashCode()
  ↓
Hash bucket
  ↓
equals()
  ↓
Confirm equality
```

Therefore, if you override `equals()` without correctly overriding `hashCode()`, hash-based collections can behave incorrectly.

This will be covered deeply in:

```text
08-Map-Framework/
04-HashCode-and-Equals.md
```

and in the Collections section.

---

# 21. `getClass()`

`getClass()` returns the runtime class of the object.

Example:

```java
Student s = new Student();

System.out.println(s.getClass());
```

Possible output:

```text
class Student
```

You can also use:

```java
System.out.println(s.getClass().getName());
```

which may produce:

```text
Student
```

---

# 22. `getClass()` and Runtime Type

Consider:

```java
Animal a = new Dog();
```

The reference type is:

```text
Animal
```

but the actual object is:

```text
Dog
```

Therefore:

```java
System.out.println(a.getClass());
```

returns:

```text
class Dog
```

This demonstrates that `getClass()` tells us the object's **runtime class**, not merely the reference type.

---

# 23. `getClass()` and Reflection

`getClass()` returns a `Class<?>` object.

Example:

```java
Student s = new Student();

Class<?> clazz = s.getClass();
```

Now:

```java
clazz.getName();
clazz.getSimpleName();
clazz.getMethods();
clazz.getFields();
```

can be used to inspect class metadata.

This connects `Object` with the `Class` class and Java Reflection.

Reflection will be covered later in:

```text
15-Advanced-Java/
02-Reflection.md
```

---

# 24. `clone()`

`clone()` is used to create a copy of an object.

The method is defined in `Object`.

However, cloning has special rules.

A class generally needs to implement:

```java
Cloneable
```

if it wants to use the traditional `Object.clone()` mechanism successfully.

Example:

```java
class Student implements Cloneable {

    int id;

    Student(int id) {
        this.id = id;
    }

    @Override
    public Student clone()
            throws CloneNotSupportedException {

        return (Student) super.clone();
    }
}
```

Then:

```java
Student s1 = new Student(101);
Student s2 = s1.clone();
```

Now:

```text
s1
 ↓
Student object

s2
 ↓
another Student object
```

---

# 25. What Is `Cloneable`?

`Cloneable` is a marker interface.

```java
public interface Cloneable
```

It does not declare a `clone()` method.

Instead, it indicates that the object can participate in the traditional `Object.clone()` mechanism.

Important:

```text
Cloneable
   ↓
Marker interface
```

No methods are declared in it.

---

# 26. Shallow Copy

`Object.clone()` performs a **shallow copy**.

Suppose:

```java
class Student implements Cloneable {

    int id;
    Address address;
}
```

When cloned:

```text
Original
 ├── id
 └── address ───────┐
                    │
Clone               │
 ├── id             │
 └── address ───────┘
```

The primitive/value fields are copied, while object references are copied as references.

Therefore, both objects can refer to the same nested mutable object.

Deep cloning requires additional logic.

---

# 27. `wait()`

`wait()` is a method of `Object`.

It is used in thread coordination.

Example conceptually:

```java
synchronized (lock) {

    lock.wait();

}
```

Calling `wait()` causes the current thread to wait and releases the monitor associated with that object.

The thread later needs to be notified or otherwise awakened according to the waiting mechanism.

---

# 28. Why Is `wait()` in Object?

This is a very common interview question.

Every Java object can act as a **monitor/lock** for synchronization.

Therefore, thread coordination methods are defined in `Object` so that any object can be used for monitor-based coordination.

Example:

```java
Object lock = new Object();
```

Then:

```java
synchronized (lock) {

    lock.wait();

}
```

---

# 29. `notify()`

`notify()` wakes one thread waiting on the object's monitor.

Example:

```java
synchronized (lock) {

    lock.notify();

}
```

Important:

`notify()` does not immediately transfer execution to the awakened thread.

It makes one waiting thread eligible to compete for the monitor.

---

# 30. `notifyAll()`

`notifyAll()` wakes all threads waiting on the object's monitor.

```java
synchronized (lock) {

    lock.notifyAll();

}
```

Those threads then compete for the monitor.

---

# 31. `wait()`, `notify()`, `notifyAll()`

These three methods are connected:

```text
Object
│
├── wait()
├── notify()
└── notifyAll()
```

They are used for traditional monitor-based thread coordination.

Important rule:

They must be called while the current thread owns the object's monitor, normally by executing inside a corresponding `synchronized` context.

Otherwise:

```text
IllegalMonitorStateException
```

can occur.

Multithreading will cover these methods deeply.

---

# 32. `finalize()` — Historical Topic

Older Java discussions often included:

```java
protected void finalize()
```

It was associated with cleanup before garbage collection.

However, `finalize()` has been **deprecated for removal** in modern Java.

It should not be used for resource management.

Prefer:

```text
try-with-resources
AutoCloseable
explicit cleanup
```

For your interview preparation:

> Know what `finalize()` historically meant, but know that modern Java discourages and deprecates it.

---

# 33. Complete Object Method Map

The important methods to remember:

```text
Object
│
├── toString()
├── equals(Object)
├── hashCode()
├── getClass()
├── clone()
├── wait()
├── notify()
├── notifyAll()
└── finalize()  [deprecated for removal]
```

---

# 34. Methods by Category

### Object identity / representation

```text
toString()
equals()
hashCode()
getClass()
```

### Object copying

```text
clone()
```

### Thread coordination

```text
wait()
notify()
notifyAll()
```

### Historical lifecycle mechanism

```text
finalize()
```

---

# 35. Object Class and Polymorphism

Because every class ultimately extends `Object`, polymorphism can work through `Object`.

Example:

```java
class Dog {
}

class Cat {
}
```

We can write:

```java
Object obj1 = new Dog();
Object obj2 = new Cat();
```

The reference type is:

```text
Object
```

but the actual runtime types are:

```text
Dog
Cat
```

This is an example of upcasting and runtime polymorphism concepts.

---

# 36. Object Class and Method Overriding

Methods such as:

```text
toString()
equals()
hashCode()
```

can be overridden by subclasses.

Example:

```java
class Student {

    @Override
    public String toString() {

        return "Student object";
    }
}
```

Now:

```java
Object obj = new Student();

System.out.println(obj);
```

calls the overridden `Student.toString()` at runtime.

This demonstrates dynamic method dispatch.

---

# 37. Why `Object` Is Important in Collections

Before generics, collections commonly stored objects through `Object` references.

Conceptually:

```java
ArrayList list = new ArrayList();

list.add("Java");
list.add(100);
list.add(new Student());
```

Everything can be stored because the elements are ultimately objects.

But retrieving values required casting:

```java
String s = (String) list.get(0);
```

Generics solved much of this problem:

```java
ArrayList<String> list =
        new ArrayList<>();
```

Now type safety is provided at compile time.

This connects:

```text
Object
   ↓
Collections
   ↓
Generics
```

---

# 38. Object and Primitive Types

Primitive types are not objects.

For example:

```java
int
double
char
boolean
```

are primitive types.

But wrapper classes are objects:

```text
int     → Integer
double  → Double
char    → Character
boolean → Boolean
```

Example:

```java
Object obj = 10;
```

This works because Java performs autoboxing:

```java
Object obj = Integer.valueOf(10);
```

So technically, the `Object` reference refers to an `Integer` object, not a primitive `int`.

---

# 39. Object vs Primitive

```text
Primitive
   ↓
int
   ↓
not an Object
```

But:

```text
Integer
   ↓
Number
   ↓
Object
```

Therefore:

```java
Object obj = 10;
```

works through boxing.

---

# 40. Important Interview Trap: Object Reference

Consider:

```java
Object obj = 10;
```

Question:

> Is `obj` storing an `int`?

No.

Conceptually:

```text
10
↓
autoboxing
↓
Integer object
↓
Object reference
```

---

# 41. `Object` vs `Class`

These are very different.

### `Object`

Represents an actual object instance.

```java
Student s = new Student();
```

### `Class`

Represents runtime metadata about a class.

```java
Class<?> c = s.getClass();
```

Think:

```text
Object
→ actual instance

Class
→ information/metadata about the class
```

Example:

```java
Student s = new Student();

Class<?> c = s.getClass();
```

Here:

```text
s → Student object
c → Class object describing Student
```

---

# 42. Object Class vs Class Class

This is a common interview confusion.

```java
Object.class
```

returns a `Class` object describing `Object`.

```java
Object obj = new Student();

obj.getClass();
```

returns the runtime `Class` object describing `Student`.

So:

```text
Object
↓
is a class

Class
↓
is also a class
```

And the `Class` class itself is represented by a `Class` object at runtime.

---

# 43. `getClass()` vs `.class`

These are related but different.

### `getClass()`

Requires an object:

```java
Student s = new Student();

s.getClass();
```

### `.class`

Works with the type itself:

```java
Student.class
```

Both give a `Class` object describing `Student`.

---

# 44. Common Mistake: Object Does Not Mean Everything Is a Reference

Java has two broad categories of types:

```text
Java Types
│
├── Primitive types
│
└── Reference types
```

All reference-type objects ultimately derive from `Object`.

Primitive values themselves do not.

---

# 45. Common Mistake: `Object` Does Not Make Primitive Variables Objects

This:

```java
int x = 10;
```

doesn't mean:

```text
x → Object
```

But:

```java
Object x = 10;
```

causes boxing:

```text
10
 ↓
Integer
 ↓
Object reference
```

---

# 46. Common Mistake: Same Hash Code Means Same Object

Wrong:

```text
same hashCode()
     ↓
same object
```

Correct:

```text
equals() == true
     ↓
must have same hashCode()
```

But:

```text
same hashCode()
     ↓
does NOT guarantee equals() == true
```

Hash collisions are possible.

---

# 47. Common Mistake: `equals()` Always Compares Contents

Not true.

If your class doesn't override `equals()`, it inherits `Object.equals()`, which uses reference identity.

Example:

```java
Student s1 = new Student();
Student s2 = new Student();

System.out.println(s1.equals(s2));
```

Normally:

```text
false
```

unless the class overrides `equals()`.

---

# 48. Common Mistake: `==` Always Compares Values

For objects:

```java
a == b
```

compares references/identity.

For primitives:

```java
a == b
```

compares primitive values.

So the meaning depends on the operands.

---

# 49. Interview Trap: `toString()` Is Called Automatically

Consider:

```java
Student s = new Student();

System.out.println(s);
```

Conceptually, `println` converts the object to text, which invokes its string representation, normally through `toString()`.

So overriding:

```java
toString()
```

changes what you see when printing the object.

---

# 50. Interview Trap: `wait()` Is Not `sleep()`

`wait()`:

```text
Object method
```

`sleep()`:

```text
Thread method
```

Major difference:

```text
wait()
→ releases the object's monitor while waiting

sleep()
→ does not release monitors held by the sleeping thread
```

Both can pause thread execution, but they serve different purposes.

---

# 51. Interview Trap: `notify()` Does Not Release the Lock Immediately

Suppose:

```java
synchronized (lock) {

    lock.notify();

}
```

`notify()` wakes a waiting thread conceptually, but the notifying thread still owns the monitor until it exits the synchronized region.

The awakened thread must acquire the monitor before continuing.

---

# 52. Object Class in One Diagram

```text
                         Object
                           │
          ┌────────────────┼────────────────┐
          │                │                │
      toString()         equals()        hashCode()
          │                │                │
     representation      equality         hashing
          │                │                │
          └────────────────┼────────────────┘
                           │
                       getClass()
                           │
                      runtime type
                           │
                  ┌────────┴────────┐
                  │                 │
               clone()         Thread methods
                  │             ├── wait()
             copying            ├── notify()
                                └── notifyAll()
```

---

# 53. Object Class — Internal Perspective

When you create:

```java
Student s = new Student();
```

conceptually:

```text
Stack
┌─────────────┐
│ s           │ ──────────────┐
└─────────────┘               │
                              ↓
                         Heap Object
                       ┌──────────────┐
                       │ Student      │
                       │ fields       │
                       └──────────────┘
                              │
                              │ inherits
                              ↓
                         Object methods
```

The reference `s` refers to a `Student` object.

The object participates in the inheritance hierarchy that ultimately reaches `Object`.

---

# 54. Why Object Methods Matter So Much

You will repeatedly encounter these methods in:

```text
OOP
Collections
HashMap
HashSet
Multithreading
Reflection
Debugging
Logging
Backend development
```

Especially:

```text
equals()
hashCode()
toString()
getClass()
wait()
notify()
notifyAll()
```

Understanding `Object` makes many later Java topics easier.

---

# 55. Important Relationships

### Object + Collections

```text
Object
  ↓
equals()
hashCode()
  ↓
HashMap / HashSet
```

### Object + Reflection

```text
Object
  ↓
getClass()
  ↓
Class
  ↓
Reflection
```

### Object + Multithreading

```text
Object
  ↓
wait()
notify()
notifyAll()
  ↓
Thread coordination
```

### Object + Debugging

```text
Object
  ↓
toString()
  ↓
Readable object representation
```

---

# 56. Top Interview Questions

## Q1. What is the Object class?

`Object` is the root class of the Java class hierarchy. Every class directly or indirectly inherits from `Object`.

---

## Q2. Why is Object important?

It provides common methods such as:

```text
toString()
equals()
hashCode()
getClass()
clone()
wait()
notify()
notifyAll()
```

which are available to objects throughout Java's class hierarchy.

---

## Q3. Is Object a class or interface?

`Object` is a class in `java.lang`.

---

## Q4. Does an interface extend Object?

No. Interfaces do not extend `Object`.

---

## Q5. What is the difference between `==` and `equals()`?

For object references:

```text
==       → reference identity
equals() → logical equality according to implementation
```

---

## Q6. What is the relationship between equals() and hashCode()?

If:

```java
a.equals(b)
```

is true, then:

```java
a.hashCode() == b.hashCode()
```

must be true.

---

## Q7. What does getClass() return?

It returns a `Class<?>` object representing the runtime class of the object.

---

## Q8. Why are wait(), notify(), and notifyAll() in Object?

Because any object can serve as a monitor for synchronized thread coordination.

---

## Q9. What does clone() do?

It provides the traditional mechanism for creating a copy of an object, with `Object.clone()` performing a shallow field-level copy.

---

## Q10. What is Cloneable?

`Cloneable` is a marker interface used with the traditional `Object.clone()` mechanism.

---

## Q11. Can Object reference hold any object?

Yes, any reference-type object can be assigned to an `Object` reference.

```java
Object obj = new Student();
Object obj2 = "Java";
```

---

## Q12. Are primitives Objects?

No. Primitive values are not objects, although autoboxing can convert them into wrapper objects.

---

# 57. Tricky Output Questions

### Question 1

```java
class Student {
}

public class Main {

    public static void main(String[] args) {

        Student s = new Student();

        System.out.println(
            s.getClass().getName()
        );
    }
}
```

Output:

```text
Student
```

---

### Question 2

```java
class Student {
}

public class Main {

    public static void main(String[] args) {

        Student s1 = new Student();
        Student s2 = new Student();

        System.out.println(s1 == s2);
        System.out.println(s1.equals(s2));
    }
}
```

Output:

```text
false
false
```

because `Student` inherits `Object.equals()` and the two objects are different instances.

---

### Question 3

```java
String s1 = new String("Java");
String s2 = new String("Java");

System.out.println(s1 == s2);
System.out.println(s1.equals(s2));
```

Output:

```text
false
true
```

because `String` overrides `equals()` to compare content.

---

### Question 4

```java
Object obj = "Java";

System.out.println(
    obj.getClass().getName()
);
```

Output:

```text
java.lang.String
```

The reference type is `Object`, but the runtime object is a `String`.

---

# 58. Quick Revision Table

| Method        | Purpose                                         |
| ------------- | ----------------------------------------------- |
| `toString()`  | String representation                           |
| `equals()`    | Logical equality                                |
| `hashCode()`  | Hash value                                      |
| `getClass()`  | Runtime class                                   |
| `clone()`     | Traditional object copying                      |
| `wait()`      | Wait for monitor coordination                   |
| `notify()`    | Wake one waiting thread                         |
| `notifyAll()` | Wake all waiting threads                        |
| `finalize()`  | Historical cleanup hook; deprecated for removal |

---

# 59. Object Class Cheat Sheet

```text
Object
│
├── java.lang.Object
│
├── Root of class hierarchy
│
├── toString()
│      → representation
│
├── equals()
│      → logical equality
│
├── hashCode()
│      → hashing
│
├── getClass()
│      → runtime type
│
├── clone()
│      → traditional shallow copy
│
├── wait()
│      → wait/monitor coordination
│
├── notify()
│      → wake one waiting thread
│
├── notifyAll()
│      → wake all waiting threads
│
└── finalize()
       → deprecated for removal
```

---

# 60. 30-Second Interview Answer

> **`Object` is the root class of Java's class hierarchy and belongs to the `java.lang` package. Every class directly or indirectly inherits from it. It provides fundamental methods such as `toString()`, `equals()`, `hashCode()`, `getClass()`, `clone()`, and the monitor methods `wait()`, `notify()`, and `notifyAll()`. `equals()` and `hashCode()` are particularly important for collections such as `HashMap` and `HashSet`, while `getClass()` connects objects to Java's Reflection API.**

---

# 61. Final Memory Trick

Remember:

```text
Object = Every Class's Ultimate Parent
```

Then remember its major methods:

```text
T → toString()
E → equals()
H → hashCode()
G → getClass()
C → clone()
W → wait()
N → notify()
N → notifyAll()
F → finalize() [deprecated]
```

Or simply:

```text
Representation → toString()
Equality       → equals()
Hashing        → hashCode()
Type           → getClass()
Copy           → clone()
Threading      → wait / notify / notifyAll()
```

---

# 62. Where These Topics Will Reappear

Don't treat this file as the final time you see these concepts.

```text
Object Class
     │
     ├── equals()
     │       ↓
     │   Collections
     │       ↓
     │   HashMap / HashSet
     │
     ├── hashCode()
     │       ↓
     │   Hashing
     │
     ├── getClass()
     │       ↓
     │   Class / Reflection
     │
     ├── wait()
     │
     ├── notify()
     │
     └── notifyAll()
             ↓
        Multithreading
```

The **Object class is therefore a foundation topic**, not an isolated API topic.
