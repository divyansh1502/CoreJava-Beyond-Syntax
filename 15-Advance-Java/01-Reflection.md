
# 🔍 Reflection in Java

> **Reflection is a Java mechanism that allows a program to inspect and interact with classes, methods, fields, constructors, and other type information at runtime.**

---

# 📑 Table of Contents

- [1. What Is Reflection?](#1-what-is-reflection)
- [2. Why Is Reflection Used?](#2-why-is-reflection-used)
- [3. Compile Time vs Runtime](#3-compile-time-vs-runtime)
- [4. The `Class` Object](#4-the-class-object)
- [5. Ways to Obtain a `Class` Object](#5-ways-to-obtain-a-class-object)
- [6. Using `.class`](#6-using-class)
- [7. Using `getClass()`](#7-using-getclass)
- [8. Using `Class.forName()`](#8-using-classforname)
- [9. Comparing the Three Approaches](#9-comparing-the-three-approaches)
- [10. Inspecting Class Information](#10-inspecting-class-information)
- [11. Getting Class Name](#11-getting-class-name)
- [12. Getting Modifiers](#12-getting-modifiers)
- [13. Getting Superclass](#13-getting-superclass)
- [14. Getting Interfaces](#14-getting-interfaces)
- [15. Getting Constructors](#15-getting-constructors)
- [16. Getting Methods](#16-getting-methods)
- [17. `getMethods()` vs `getDeclaredMethods()`](#17-getmethods-vs-getdeclaredmethods)
- [18. Getting Fields](#18-getting-fields)
- [19. `getFields()` vs `getDeclaredFields()`](#19-getfields-vs-getdeclaredfields)
- [20. Invoking Methods Dynamically](#20-invoking-methods-dynamically)
- [21. Accessing Private Members](#21-accessing-private-members)
- [22. Creating Objects with Reflection](#22-creating-objects-with-reflection)
- [23. Accessing Fields Dynamically](#23-accessing-fields-dynamically)
- [24. Reflection and Generics](#24-reflection-and-generics)
- [25. Reflection and Arrays](#25-reflection-and-arrays)
- [26. Reflection and Annotations](#26-reflection-and-annotations)
- [27. Reflection Exceptions](#27-reflection-exceptions)
- [28. Reflection and Class Loaders](#28-reflection-and-class-loaders)
- [29. Reflection and Encapsulation](#29-reflection-and-encapsulation)
- [30. Advantages of Reflection](#30-advantages-of-reflection)
- [31. Disadvantages of Reflection](#31-disadvantages-of-reflection)
- [32. Common Real-World Uses](#32-common-real-world-uses)
- [33. Common Mistakes](#33-common-mistakes)
- [34. Interview Questions](#34-interview-questions)
- [35. 30-Second Interview Answer](#35-30-second-interview-answer)
- [36. Cheat Sheet](#36-cheat-sheet)
- [37. Final Mental Model](#37-final-mental-model)

---

# 1. What Is Reflection?

Reflection is the ability of Java code to inspect information about classes and objects at runtime.

Normally, we write code knowing the class at compile time.

Example:

```java
Student student =
    new Student();
```

With reflection, we can inspect the class dynamically.

Example:

```java
Class<?> clazz =
    student.getClass();

System.out.println(
    clazz.getName()
);
```

Reflection can inspect:

```text
Class
    ↓
Constructors
    ↓
Methods
    ↓
Fields
    ↓
Modifiers
    ↓
Interfaces
    ↓
Superclass
    ↓
Annotations
```

It can also be used to dynamically:

```text
create objects
invoke methods
read fields
modify fields
```

---

# 2. Why Is Reflection Used?

Reflection is useful when the exact class or members are not known at compile time.

For example, a framework may receive:

```text
Class name
    ↓
load class
    ↓
inspect annotations
    ↓
find constructor
    ↓
create object
    ↓
invoke method
```

This allows frameworks and libraries to work with classes dynamically.

Common areas where reflection is used include:

```text
Dependency Injection
ORM frameworks
Testing frameworks
Serialization
Web frameworks
Plugin systems
Framework configuration
Annotation processing
Debugging tools
```

Examples of technologies that make significant use of reflection include frameworks such as:

```text
Spring
Hibernate
JUnit
```

---

# 3. Compile Time vs Runtime

Without reflection:

```text
Source Code
    ↓
Compiler
    ↓
Type information known
    ↓
Program execution
```

With reflection:

```text
Program running
    ↓
Inspect class metadata
    ↓
Discover members
    ↓
Interact dynamically
```

Example:

```java
Class<?> clazz =
    Class.forName(
        "java.lang.String"
    );
```

The class is identified using a name at runtime.

---

# 4. The `Class` Object

The central class of Java reflection is:

```text
java.lang.Class
```

Every loaded Java type has a corresponding `Class` object.

For example:

```java
Class<String> clazz =
    String.class;
```

Here:

```text
String.class
```

represents the runtime class metadata for `String`.

A `Class` object can provide information about:

```text
name
modifiers
constructors
methods
fields
superclass
interfaces
annotations
```

---

# 5. Ways to Obtain a `Class` Object

There are three commonly discussed ways:

```text
1. Type.class

2. object.getClass()

3. Class.forName()
```

Example:

```java
Class<String> a =
    String.class;
```

```java
String value =
    "Hello";

Class<?> b =
    value.getClass();
```

```java
Class<?> c =
    Class.forName(
        "java.lang.String"
    );
```

All three can refer to the same runtime class.

---

# 6. Using `.class`

The `.class` syntax obtains the `Class` object for a known type.

Example:

```java
Class<String> clazz =
    String.class;
```

For a custom class:

```java
class Student {
}
```

```java
Class<Student> clazz =
    Student.class;
```

This is useful when the type is already known at compile time.

---

# 7. Using `getClass()`

`getClass()` is an instance method inherited from `Object`.

Example:

```java
Student student =
    new Student();

Class<?> clazz =
    student.getClass();
```

This gives the actual runtime class of the object.

This is useful when you have an object but do not necessarily know its concrete class directly.

Example:

```java
Object obj =
    new Student();

Class<?> clazz =
    obj.getClass();
```

The runtime class is:

```text
Student
```

not merely:

```text
Object
```

---

# 8. Using `Class.forName()`

`Class.forName()` loads a class using its fully qualified name.

Example:

```java
Class<?> clazz =
    Class.forName(
        "java.lang.String"
    );
```

For a custom class:

```java
Class<?> clazz =
    Class.forName(
        "com.example.Student"
    );
```

Important:

```text
Class.forName()
→ works with a class name represented as String
```

This makes it useful for dynamic loading.

---

# 9. Comparing the Three Approaches

| Approach | Requires object? | Class name as String? | Typical use |
|---|---:|---:|---|
| `Student.class` | No | No | Known type |
| `obj.getClass()` | Yes | No | Runtime object |
| `Class.forName()` | No | Yes | Dynamic class loading |

Memory trick:

```text
.class
→ I know the type

getClass()
→ I have an object

forName()
→ I have the class name
```

---

# 10. Inspecting Class Information

Suppose:

```java
class Student {

    private int id;

    public void study() {
    }
}
```

We can inspect it:

```java
Class<?> clazz =
    Student.class;

System.out.println(
    clazz.getName()
);
```

We can also inspect:

```text
constructors
methods
fields
interfaces
superclass
modifiers
annotations
```

Reflection essentially provides access to the class's runtime metadata.

---

# 11. Getting Class Name

There are several name-related methods.

### `getName()`

```java
Class<?> clazz =
    String.class;

System.out.println(
    clazz.getName()
);
```

Output:

```text
java.lang.String
```

### `getSimpleName()`

```java
System.out.println(
    clazz.getSimpleName()
);
```

Output:

```text
String
```

### `getCanonicalName()`

```java
System.out.println(
    clazz.getCanonicalName()
);
```

For a normal top-level class, this commonly resembles:

```text
package.ClassName
```

---

# 12. Getting Modifiers

Reflection can inspect modifiers.

Example:

```java
import java.lang.reflect.Modifier;

class Student {
}
```

```java
Class<?> clazz =
    Student.class;

int modifiers =
    clazz.getModifiers();

System.out.println(
    Modifier.isPublic(
        modifiers
    )
);
```

`Modifier` provides utility methods such as:

```text
isPublic()
isPrivate()
isProtected()
isStatic()
isFinal()
isAbstract()
isInterface()
```

---

# 13. Getting Superclass

Use:

```java
getSuperclass()
```

Example:

```java
Class<?> clazz =
    String.class;

Class<?> parent =
    clazz.getSuperclass();

System.out.println(
    parent.getName()
);
```

For `String`, the superclass is:

```text
java.lang.Object
```

For:

```java
class Child extends Parent {
}
```

the result is:

```text
Parent
```

---

# 14. Getting Interfaces

Use:

```java
getInterfaces()
```

Example:

```java
interface Printable {
}
```

```java
class Report
    implements Printable {
}
```

```java
Class<?> clazz =
    Report.class;

Class<?>[] interfaces =
    clazz.getInterfaces();

for (Class<?> type :
        interfaces) {

    System.out.println(
        type.getName()
    );
}
```

This returns the interfaces directly implemented by the class.

---

# 15. Getting Constructors

Reflection provides constructor metadata through:

```text
Constructor
```

from:

```text
java.lang.reflect
```

Example:

```java
import java.lang.reflect.Constructor;

class Student {

    public Student() {
    }

    public Student(
        int id
    ) {
    }
}
```

```java
Class<Student> clazz =
    Student.class;

Constructor<?>[] constructors =
    clazz.getDeclaredConstructors();

for (
    Constructor<?> constructor :
    constructors
) {

    System.out.println(
        constructor
    );
}
```

---

# 16. Getting Methods

Reflection provides method metadata through:

```text
java.lang.reflect.Method
```

Example:

```java
import java.lang.reflect.Method;

class Student {

    public void study() {
    }

    private void sleep() {
    }
}
```

```java
Class<?> clazz =
    Student.class;

Method[] methods =
    clazz.getDeclaredMethods();

for (Method method :
        methods) {

    System.out.println(
        method.getName()
    );
}
```

---

# 17. `getMethods()` vs `getDeclaredMethods()`

This is a common interview question.

### `getMethods()`

Returns public methods that are accessible through the class, including inherited public methods.

Example:

```java
Method[] methods =
    clazz.getMethods();
```

It can include methods inherited from:

```text
superclasses
interfaces
Object
```

### `getDeclaredMethods()`

Returns methods declared directly by that class.

It includes methods regardless of access modifier.

Example:

```java
Method[] methods =
    clazz.getDeclaredMethods();
```

Therefore:

```text
getMethods()
→ public methods
→ includes inherited public methods


getDeclaredMethods()
→ methods declared by this class
→ includes private/protected/package-private/public
```

---

# 18. Getting Fields

Reflection provides field metadata through:

```text
java.lang.reflect.Field
```

Example:

```java
import java.lang.reflect.Field;

class Student {

    private int id;

    public String name;
}
```

```java
Class<?> clazz =
    Student.class;

Field[] fields =
    clazz.getDeclaredFields();

for (Field field :
        fields) {

    System.out.println(
        field.getName()
    );
}
```

---

# 19. `getFields()` vs `getDeclaredFields()`

### `getFields()`

Returns public fields accessible through the class, including inherited public fields.

```java
Field[] fields =
    clazz.getFields();
```

### `getDeclaredFields()`

Returns fields declared directly in the class regardless of access modifier.

```java
Field[] fields =
    clazz.getDeclaredFields();
```

Memory trick:

```text
getMethods()
→ public + inherited

getDeclaredMethods()
→ declared here + all access levels


getFields()
→ public + inherited

getDeclaredFields()
→ declared here + all access levels
```

---

# 20. Invoking Methods Dynamically

Reflection can invoke a method at runtime.

Example:

```java
import java.lang.reflect.Method;

class Student {

    public void study() {

        System.out.println(
            "Student is studying"
        );
    }
}
```

```java
Student student =
    new Student();

Method method =
    Student.class.getMethod(
        "study"
    );

method.invoke(
    student
);
```

The method is selected using:

```text
method name
+
parameter types
```

and invoked using:

```text
Method.invoke()
```

---

# 21. Accessing Private Members

Reflection can discover private members using:

```text
getDeclared...
```

Example:

```java
class Student {

    private String name =
        "Aman";
}
```

```java
import java.lang.reflect.Field;

Student student =
    new Student();

Field field =
    Student.class
        .getDeclaredField(
            "name"
        );
```

The field is private, so normal access rules apply.

Historically, reflection could attempt to bypass access checks using:

```java
field.setAccessible(true);
```

Example:

```java
field.setAccessible(true);

System.out.println(
    field.get(student)
);
```

However, modern Java's strong encapsulation and module system can restrict reflective access, especially for strongly encapsulated platform/module code.

Therefore:

```text
Reflection does not mean
all access restrictions
are automatically removed.
```

---

# 22. Creating Objects with Reflection

Objects can be created dynamically using constructors.

Modern reflection commonly uses:

```text
Constructor.newInstance()
```

Example:

```java
import java.lang.reflect.Constructor;

class Student {

    private int id;

    public Student(
        int id
    ) {

        this.id = id;
    }
}
```

```java
Constructor<Student> constructor =
    Student.class.getConstructor(
        int.class
    );

Student student =
    constructor.newInstance(
        101
    );
```

The important flow is:

```text
Class
 ↓
Constructor
 ↓
newInstance()
 ↓
Object
```

---

# 23. Accessing Fields Dynamically

Reflection can read and modify fields.

Example:

```java
class Student {

    public String name =
        "Aman";
}
```

```java
import java.lang.reflect.Field;

Student student =
    new Student();

Field field =
    Student.class.getField(
        "name"
    );

System.out.println(
    field.get(student)
);
```

A value can also be changed:

```java
field.set(
    student,
    "Rahul"
);
```

Then:

```java
System.out.println(
    student.name
);
```

The field now contains:

```text
Rahul
```

---

# 24. Reflection and Generics

Reflection can inspect generic type information when that metadata is available.

Example:

```java
class Box {

    private java.util.List<String>
        names;
}
```

We can inspect the generic type:

```java
import java.lang.reflect.Field;

Field field =
    Box.class.getDeclaredField(
        "names"
    );

System.out.println(
    field.getGenericType()
);
```

The result can contain generic information such as:

```text
List<String>
```

This is different from ordinary runtime object type checks, where type erasure still applies.

Important interview distinction:

```text
Java erases generic type information
for many runtime operations.

But reflective metadata can preserve
generic declarations where the compiler
stored the relevant Signature metadata.
```

---

# 25. Reflection and Arrays

Reflection provides array utilities through:

```text
java.lang.reflect.Array
```

Example:

```java
import java.lang.reflect.Array;

Object array =
    Array.newInstance(
        int.class,
        5
    );
```

This creates an integer array dynamically.

Values can be set:

```java
Array.set(
    array,
    0,
    100
);
```

Values can be retrieved:

```java
int value =
    Array.getInt(
        array,
        0
    );
```

This is useful when the component type and size are known dynamically.

---

# 26. Reflection and Annotations

Reflection can inspect runtime-visible annotations.

Example:

```java
@Retention(
    RetentionPolicy.RUNTIME
)
@interface Important {
}
```

```java
@Important
class Student {
}
```

Then:

```java
Class<Student> clazz =
    Student.class;

boolean present =
    clazz.isAnnotationPresent(
        Important.class
    );
```

We can also retrieve the annotation:

```java
Important annotation =
    clazz.getAnnotation(
        Important.class
    );
```

This is one reason runtime-retained annotations are important in frameworks.

---

# 27. Reflection Exceptions

Reflection APIs commonly throw checked exceptions.

Important examples:

```text
ClassNotFoundException
NoSuchMethodException
NoSuchFieldException
InstantiationException
IllegalAccessException
InvocationTargetException
```

Example:

```java
Class<?> clazz =
    Class.forName(
        "com.example.Student"
    );
```

This can throw:

```text
ClassNotFoundException
```

Method lookup can throw:

```text
NoSuchMethodException
```

Field lookup can throw:

```text
NoSuchFieldException
```

Constructor invocation can involve:

```text
InstantiationException
IllegalAccessException
InvocationTargetException
```

---

# 28. Reflection and Class Loaders

Class loading and reflection are related but not identical concepts.

A class loader is responsible for loading class definitions.

Reflection allows code to inspect and interact with loaded type information.

Example:

```java
Class<?> clazz =
    Class.forName(
        "com.example.Student"
    );
```

Conceptually:

```text
Class name
    ↓
Class loading
    ↓
Class object
    ↓
Reflection
    ↓
Inspect / invoke / create
```

Class loaders are part of the JVM's class-loading architecture.

---

# 29. Reflection and Encapsulation

Reflection interacts with Java's access control mechanisms.

Normal Java code respects:

```text
public
protected
package-private
private
```

Reflection can inspect members declared with different access levels.

However, modern Java has stronger encapsulation through the module system.

Therefore, reflective access can be restricted depending on:

```text
module boundaries
access rules
runtime configuration
```

This is especially important when dealing with:

```text
JDK internal APIs
strongly encapsulated modules
```

A common mistake is thinking:

```text
Reflection = unlimited access
```

That is not correct.

---

# 30. Advantages of Reflection

## 1. Dynamic Behavior

Classes can be discovered at runtime.

```text
class name
→ Class
→ inspect
→ interact
```

---

## 2. Framework Development

Reflection allows generic frameworks to operate on user-defined classes.

---

## 3. Annotation-Based Configuration

Frameworks can inspect annotations and use them to determine behavior.

---

## 4. Plugin Systems

Applications can discover and load classes dynamically.

---

## 5. Testing

Testing frameworks can discover:

```text
test methods
annotations
constructors
```

and execute them dynamically.

---

# 31. Disadvantages of Reflection

## 1. Performance Overhead

Reflective calls can introduce additional overhead compared with direct method calls.

Direct:

```java
student.study();
```

Reflective:

```java
method.invoke(
    student
);
```

The reflective path requires runtime metadata handling and access checks.

---

## 2. Reduced Compile-Time Safety

Direct code:

```java
student.study();
```

The compiler can verify:

```text
method exists
method signature
argument types
```

Reflection often moves some of these checks to runtime.

---

## 3. More Complex Code

Reflection APIs are more verbose.

---

## 4. Encapsulation Concerns

Reflection can expose implementation details that normal API usage would keep hidden.

---

## 5. Harder Debugging

Errors may occur at runtime rather than compile time.

---

## 6. Module Restrictions

Modern Java's module system can restrict reflective access across module boundaries.

---

# 32. Common Real-World Uses

Reflection is commonly associated with:

### Dependency Injection

A framework may inspect constructors and annotations to determine how to create objects.

Conceptually:

```text
@Service
    ↓
Reflection
    ↓
Find class
    ↓
Find constructor
    ↓
Create object
```

---

### ORM

An ORM framework may inspect:

```text
fields
annotations
constructors
methods
```

to map Java objects to database structures.

---

### Testing Frameworks

A testing framework can discover methods marked with test annotations.

Conceptually:

```text
Class
 ↓
Methods
 ↓
@Test
 ↓
Invoke method
```

---

### Serialization

A framework can inspect object structure and determine how fields should be serialized.

---

### Plugin Systems

An application can load classes dynamically and inspect whether they implement a required interface.

---

# 33. Common Mistakes

## ❌ Mistake 1 — Thinking Reflection Is a Separate JVM

Reflection is an API mechanism built on top of Java's runtime type information.

It is not a separate runtime.

---

## ❌ Mistake 2 — Confusing `Class` With `class`

These are different concepts.

```text
Class
→ java.lang.Class type

class
→ Java language keyword
```

Example:

```java
Class<?> clazz =
    Student.class;
```

Here:

```text
Class
→ type

Student.class
→ class literal
```

---

## ❌ Mistake 3 — Using `getMethods()` to Find Private Methods

`getMethods()` returns public methods.

For methods declared by the class regardless of access level:

```java
getDeclaredMethods()
```

---

## ❌ Mistake 4 — Assuming `getDeclaredMethods()` Includes Inherited Methods

It does not return inherited methods simply because they are inherited.

It returns methods declared by that specific class.

---

## ❌ Mistake 5 — Assuming Reflection Always Bypasses `private`

Modern Java's access controls and module boundaries can restrict reflective access.

---

## ❌ Mistake 6 — Using `Class.forName()` When You Already Have an Object

If you already have an object:

```java
obj.getClass();
```

is generally the direct approach.

---

## ❌ Mistake 7 — Forgetting `InvocationTargetException`

When a reflected method throws an exception, `Method.invoke()` can report it through:

```text
InvocationTargetException
```

The underlying exception can be obtained from:

```java
exception.getCause();
```

---

## ❌ Mistake 8 — Assuming Reflection Is Always Slow Enough to Matter

Reflection generally has more overhead than direct invocation, but whether that matters depends on the workload.

Frameworks often use reflection selectively and may cache metadata.

---

# 34. Interview Questions

## 🔥 Q1. What is Reflection in Java?

Reflection is the ability to inspect and interact with classes, methods, fields, constructors, and other type information at runtime.

---

## 🔥 Q2. Which class is central to Java Reflection?

```text
java.lang.Class
```

---

## 🔥 Q3. What are the three common ways to obtain a `Class` object?

```text
Type.class
object.getClass()
Class.forName()
```

---

## 🔥 Q4. Difference between `.class` and `getClass()`?

```text
Student.class
→ class known directly

student.getClass()
→ runtime class of an object
```

---

## 🔥 Q5. What does `Class.forName()` do?

It obtains a `Class` object using a fully qualified class name represented as a string and performs class loading/initialization behavior according to the `Class.forName` API semantics.

Example:

```java
Class<?> clazz =
    Class.forName(
        "java.lang.String"
    );
```

---

## 🔥 Q6. What is the difference between `getMethods()` and `getDeclaredMethods()`?

```text
getMethods()
→ public methods
→ includes inherited public methods

getDeclaredMethods()
→ methods declared by the class
→ all access levels
```

---

## 🔥 Q7. What is the difference between `getFields()` and `getDeclaredFields()`?

```text
getFields()
→ public fields
→ includes inherited public fields

getDeclaredFields()
→ fields declared by the class
→ all access levels
```

---

## 🔥 Q8. How can a method be invoked using reflection?

Using:

```text
Method.invoke()
```

Example:

```java
Method method =
    Student.class.getMethod(
        "study"
    );

method.invoke(
    student
);
```

---

## 🔥 Q9. How can an object be created using reflection?

Using a constructor:

```java
Constructor<Student> constructor =
    Student.class.getConstructor();

Student student =
    constructor.newInstance();
```

---

## 🔥 Q10. Can reflection access private members?

Reflection can discover declared private members, but actual access can be restricted by Java's access controls and module encapsulation.

---

## 🔥 Q11. What is `setAccessible(true)`?

It historically allowed reflective code to suppress certain Java language access checks for a reflective object.

However, modern Java's module system can still prevent access to strongly encapsulated members.

---

## 🔥 Q12. What is `Method` in reflection?

```text
java.lang.reflect.Method
```

represents a method and provides information and reflective invocation capabilities.

---

## 🔥 Q13. What is `Field`?

```text
java.lang.reflect.Field
```

represents a field declared by a class or interface.

---

## 🔥 Q14. What is `Constructor`?

```text
java.lang.reflect.Constructor
```

represents a constructor and can be used to create instances reflectively.

---

## 🔥 Q15. What are the disadvantages of reflection?

Important disadvantages include:

```text
Runtime errors
Performance overhead
More complex code
Reduced compile-time safety
Encapsulation concerns
Module access restrictions
```

---

## 🔥 Q16. Why do frameworks use reflection?

Frameworks often need to work with classes that application developers provide without hardcoding every class and method.

Reflection enables:

```text
dynamic discovery
annotation inspection
object creation
method invocation
field inspection
```

---

## 🔥 Q17. What is `InvocationTargetException`?

It is an exception used by reflective invocation APIs when the underlying invoked method or constructor throws an exception.

The underlying exception can often be accessed using:

```java
exception.getCause();
```

---

## 🔥 Q18. Can reflection inspect annotations?

Yes, provided the annotation's retention and visibility allow it to be available through reflection.

Example:

```java
clazz.isAnnotationPresent(
    Important.class
);
```

---

## 🔥 Q19. Can reflection create arrays?

Yes.

The `java.lang.reflect.Array` class provides dynamic array creation and access.

---

## 🔥 Q20. Is reflection type-safe?

Reflection provides runtime APIs and therefore shifts some checks from compile time to runtime.

It should not be considered equivalent to ordinary compile-time type checking.

---

## 🔥 Q21. Does reflection break encapsulation?

It can bypass or interact with access boundaries in ways ordinary code cannot, but modern Java's module system and access controls place important restrictions on reflective access.

---

## 🔥 Q22. What is the difference between compile-time and runtime type information?

Compile-time information is checked by the compiler.

Reflection operates primarily using metadata available at runtime.

---

## 🔥 Q23. Can reflection invoke static methods?

Yes.

Example:

```java
Method method =
    Math.class.getMethod(
        "abs",
        int.class
    );

int result =
    (int) method.invoke(
        null,
        -10
    );
```

For a static method, the object argument to `invoke()` is typically `null`.

---

## 🔥 Q24. Can reflection modify a final field?

Modern Java places significant restrictions around modifying final fields reflectively, and relying on reflective modification of final fields is generally inappropriate.

Do not treat `final` fields as freely modifiable through reflection.

---

## 🔥 Q25. Why should reflection be used carefully?

Because it trades some compile-time guarantees and straightforward code for runtime flexibility.

It is powerful for infrastructure and frameworks but can increase:

```text
complexity
runtime failure risk
maintenance cost
access-control concerns
```

---

# 35. 30-Second Interview Answer

> Reflection in Java is a runtime mechanism for inspecting and interacting with classes and their members. The central API is `java.lang.Class`, through which we can discover constructors, methods, fields, interfaces, superclasses, and annotations. Reflection can also create objects, invoke methods, and access fields dynamically. It is widely used by frameworks, dependency injection systems, ORM tools, testing frameworks, and plugin systems. The main trade-offs are runtime errors, additional complexity, performance overhead, reduced compile-time checking, and access restrictions introduced by modern Java's encapsulation and module system.

---

# 36. Cheat Sheet

```text
========================================================
                    JAVA REFLECTION
========================================================


CORE IDEA
--------------------------------------------------------

Reflection
→ inspect and interact with types at runtime


========================================================

CENTRAL CLASS
--------------------------------------------------------

java.lang.Class


========================================================

GET CLASS OBJECT
--------------------------------------------------------

1. Type.class

Student.class


2. object.getClass()

student.getClass()


3. Class.forName()

Class.forName(
    "com.example.Student"
)


MEMORY TRICK

.class
→ I know the type

getClass()
→ I have the object

forName()
→ I have the name


========================================================

CLASS INFORMATION
--------------------------------------------------------

getName()
getSimpleName()
getCanonicalName()

getModifiers()

getSuperclass()

getInterfaces()


========================================================

CONSTRUCTORS
--------------------------------------------------------

getConstructor()
getConstructors()

getDeclaredConstructor()
getDeclaredConstructors()

Constructor.newInstance()


========================================================

METHODS
--------------------------------------------------------

getMethod()
getMethods()

getDeclaredMethod()
getDeclaredMethods()

Method.invoke()


========================================================

FIELDS
--------------------------------------------------------

getField()
getFields()

getDeclaredField()
getDeclaredFields()

Field.get()
Field.set()


========================================================

IMPORTANT DIFFERENCE
--------------------------------------------------------

getMethods()
→ public
→ inherited public methods included


getDeclaredMethods()
→ declared by this class
→ all access levels


getFields()
→ public
→ inherited public fields included


getDeclaredFields()
→ declared by this class
→ all access levels


========================================================

ARRAY
--------------------------------------------------------

java.lang.reflect.Array

Array.newInstance()

Array.get()

Array.set()


========================================================

ANNOTATIONS
--------------------------------------------------------

isAnnotationPresent()

getAnnotation()

getAnnotations()

getDeclaredAnnotations()


========================================================

COMMON EXCEPTIONS
--------------------------------------------------------

ClassNotFoundException
NoSuchMethodException
NoSuchFieldException
InstantiationException
IllegalAccessException
InvocationTargetException


========================================================

ADVANTAGES
--------------------------------------------------------

Dynamic discovery
Framework development
Plugin systems
Testing
Annotation processing
ORM
Dependency injection


========================================================

DISADVANTAGES
--------------------------------------------------------

Runtime errors
Performance overhead
Complexity
Reduced compile-time safety
Encapsulation concerns
Module restrictions


========================================================

REFLECTION FLOW
--------------------------------------------------------

Class Name
    ↓
Class.forName()
    ↓
Class Object
    ↓
Inspect Metadata
    ↓
Find Constructor / Method / Field
    ↓
Interact Dynamically


========================================================

OBJECT CREATION
--------------------------------------------------------

Class
 ↓
Constructor
 ↓
newInstance()
 ↓
Object


========================================================

METHOD INVOCATION
--------------------------------------------------------

Class
 ↓
Method
 ↓
invoke()
 ↓
Result


========================================================

FIELD ACCESS
--------------------------------------------------------

Class
 ↓
Field
 ↓
get() / set()
 ↓
Value


========================================================
```

---

# 37. Final Mental Model

```text
                         REFLECTION
                              |
                              ↓
                     java.lang.Class
                              |
        +---------------------+---------------------+
        |                     |                     |
        ↓                     ↓                     ↓
   Constructors            Methods               Fields
        |                     |                     |
        ↓                     ↓                     ↓
 newInstance()             invoke()             get()/set()
        |                     |                     |
        +---------------------+---------------------+
                              |
                              ↓
                       Runtime Behavior


========================================================

NORMAL JAVA

Compiler knows:

Student
   ↓
study()
   ↓
compile-time checking
   ↓
direct invocation


========================================================

REFLECTION

Runtime discovers:

Class
   ↓
Method
   ↓
method name
   ↓
parameter types
   ↓
invoke()
   ↓
runtime result


========================================================

THINK OF REFLECTION AS:

"Java code examining Java code
and Java objects at runtime."


========================================================

CLASS OBJECT MEMORY TRICK

.class
→ type is known

getClass()
→ object is known

forName()
→ name is known


========================================================

GET vs GET DECLARED

getMethods()
→ public + inherited

getDeclaredMethods()
→ declared here + all access levels


getFields()
→ public + inherited

getDeclaredFields()
→ declared here + all access levels


========================================================

FRAMEWORK MENTAL MODEL

Developer writes:

@Service
class UserService {
}


Framework:

Class
 ↓
Reflection
 ↓
Find annotation
 ↓
Find constructor
 ↓
Create object
 ↓
Register object
 ↓
Use object


========================================================

REFLECTION TRADE-OFF

                 Reflection
                     |
          +----------+----------+
          |                     |
      Flexibility            Cost
          |                     |
      Dynamic              Runtime errors
      behavior             Overhead
      Frameworks            Complexity
      Plugins              Less compile-time
                           checking


========================================================

IMPORTANT INTERVIEW LINE

"Reflection provides runtime flexibility,
but it moves some checks from compile time
to runtime."


========================================================

FINAL MEMORY MAP

Class
 ↓
Metadata
 ↓
Constructors
Methods
Fields
Annotations
Interfaces
Superclass
Modifiers
 ↓
Dynamic Interaction


========================================================

CORE IDEA

Normal Java:
"I know what I want at compile time."

Reflection:
"I want to discover what I have
at runtime."


========================================================
```

---

# 🏁 Final Takeaways

- Reflection allows Java programs to inspect and interact with types at runtime.
- The central reflection type is `java.lang.Class`.
- A `Class` object can be obtained using `.class`, `getClass()`, or `Class.forName()`.
- `.class` is useful when the type is known at compile time.
- `getClass()` gives the runtime class of an object.
- `Class.forName()` works with a class name represented as a string.
- Reflection can inspect constructors, methods, fields, modifiers, interfaces, superclasses, and annotations.
- `getMethods()` returns public methods, including inherited public methods.
- `getDeclaredMethods()` returns methods declared directly by the class, regardless of access modifier.
- `getFields()` returns public fields, including inherited public fields.
- `getDeclaredFields()` returns fields declared directly by the class, regardless of access modifier.
- `Method.invoke()` can invoke methods dynamically.
- `Constructor.newInstance()` can create objects dynamically.
- `Field.get()` and `Field.set()` can read and modify fields when access is permitted.
- `java.lang.reflect.Array` supports dynamic array operations.
- Reflection is heavily used in frameworks, testing, ORM, dependency injection, annotation processing, and plugin systems.
- Reflection can introduce runtime errors and performance overhead.
- Reflection reduces some compile-time guarantees because names, signatures, and access may be resolved dynamically.
- Modern Java's module system and strong encapsulation can restrict reflective access.
- `setAccessible(true)` should not be understood as an unlimited bypass of modern access controls.
- Reflection is powerful infrastructure technology, but direct Java code is generally simpler when the required types and members are already known.

---

