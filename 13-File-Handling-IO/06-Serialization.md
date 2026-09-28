
# 📦 Java Serialization

> **Serialization is the process of converting an object's state into a byte stream so that it can be stored or transmitted and later reconstructed through deserialization.**

---

# 📑 Table of Contents

- [1. What Is Serialization?](#1-what-is-serialization)
- [2. Why Do We Need Serialization?](#2-why-do-we-need-serialization)
- [3. Serialization Flow](#3-serialization-flow)
- [4. Serializable Interface](#4-serializable-interface)
- [5. How to Make a Class Serializable](#5-how-to-make-a-class-serializable)
- [6. ObjectOutputStream](#6-objectoutputstream)
- [7. writeObject()](#7-writeobject)
- [8. Complete Serialization Example](#8-complete-serialization-example)
- [9. What Exactly Gets Serialized?](#9-what-exactly-gets-serialized)
- [10. transient Keyword](#10-transient-keyword)
- [11. transient Example](#11-transient-example)
- [12. static Fields and Serialization](#12-static-fields-and-serialization)
- [13. serialVersionUID](#13-serialversionuid)
- [14. Why serialVersionUID Is Important](#14-why-serialversionuid-is-important)
- [15. Serialization and Inheritance](#15-serialization-and-inheritance)
- [16. Serializable Parent and Child](#16-serializable-parent-and-child)
- [17. Non-Serializable Parent](#17-non-serializable-parent)
- [18. Constructors and Serialization](#18-constructors-and-serialization)
- [19. Object Graph](#19-object-graph)
- [20. Serialization of Referenced Objects](#20-serialization-of-referenced-objects)
- [21. NotSerializableException](#21-notserializableexception)
- [22. Serialization of Collections](#22-serialization-of-collections)
- [23. Serialization vs Externalization](#23-serialization-vs-externalization)
- [24. Serialization vs JSON](#24-serialization-vs-json)
- [25. Advantages](#25-advantages)
- [26. Disadvantages](#26-disadvantages)
- [27. Security Concerns](#27-security-concerns)
- [28. Common Mistakes](#28-common-mistakes)
- [29. Interview Questions](#29-interview-questions)
- [30. 30-Second Interview Answer](#30-30-second-interview-answer)
- [31. Cheat Sheet](#31-cheat-sheet)

---

# 1. What Is Serialization?

Serialization converts an object's state into a byte stream.

Conceptually:

```text
Java Object
    ↓
Serialization
    ↓
Byte Stream
```

The byte stream can then be:

```text
Stored in a file
Sent through a network
Stored somewhere else
```

Later, the byte stream can be converted back into an object through deserialization.

```text
Byte Stream
    ↓
Deserialization
    ↓
Java Object
```

---

# 2. Why Do We Need Serialization?

Java objects normally exist in memory while the application is running.

For example:

```java
class Employee {

    int id;
    String name;

}
```

An object can be created:

```java
Employee employee = new Employee();
```

The object exists in memory.

But when the application terminates, that in-memory object is gone.

Serialization provides a way to represent the object's state as a byte stream.

---

## Common Uses

Historically, Java serialization has been used for:

```text
Object persistence
Inter-process communication
Network communication
Caching
Saving object state
```

However, modern applications often use formats such as:

```text
JSON
Protocol Buffers
Avro
Other application-specific formats
```

depending on the use case.

---

# 3. Serialization Flow

The basic flow is:

```text
                SERIALIZATION

        Java Object
             ↓
      ObjectOutputStream
             ↓
         Byte Stream
             ↓
      File / Network / Storage
```

Reverse:

```text
                DESERIALIZATION

File / Network / Storage
             ↓
      ObjectInputStream
             ↓
         Java Object
```

---

# 4. Serializable Interface

To use Java's built-in object serialization mechanism, the class generally implements:

```java
java.io.Serializable
```

Example:

```java
import java.io.Serializable;

class Employee implements Serializable {

    int id;
    String name;
}
```

---

## 🧠 Important

`Serializable` is a **marker interface**.

It does not define methods that your class must implement.

Its purpose is to indicate that instances of the class are eligible for Java's serialization mechanism.

---

## Marker Interface

A marker interface provides metadata to the Java runtime/framework.

Examples:

```text
Serializable
Cloneable
```

So:

```java
class Employee implements Serializable {
}
```

is enough to mark the class as serializable.

---

# 5. How to Make a Class Serializable

Basic steps:

### Step 1

Implement `Serializable`.

```java
import java.io.Serializable;

class Employee implements Serializable {

    int id;
    String name;
}
```

### Step 2

Create an object.

```java
Employee employee =
    new Employee();
```

### Step 3

Create an `ObjectOutputStream`.

```java
ObjectOutputStream output =
    new ObjectOutputStream(
        new FileOutputStream("employee.ser")
    );
```

### Step 4

Write the object.

```java
output.writeObject(employee);
```

### Step 5

Close the stream.

```java
output.close();
```

---

# 6. ObjectOutputStream

`ObjectOutputStream` is used to write Java objects to an `OutputStream`.

Package:

```text
java.io
```

Hierarchy:

```text
Object
   ↓
OutputStream
      ↓
FilterOutputStream
      ↓
ObjectOutputStream
```

Typical usage:

```text
FileOutputStream
       ↓
ObjectOutputStream
       ↓
Java Object
```

Example:

```java
ObjectOutputStream output =
    new ObjectOutputStream(
        new FileOutputStream("employee.ser")
    );
```

---

# 7. writeObject()

The most important method for serialization is:

```java
writeObject()
```

Example:

```java
output.writeObject(employee);
```

Conceptually:

```text
employee object
      ↓
writeObject()
      ↓
Serialized byte stream
```

`writeObject()` can serialize an object graph, not merely one isolated object's fields.

---

# 8. Complete Serialization Example

## Employee Class

```java
import java.io.Serializable;

class Employee implements Serializable {

    private int id;
    private String name;

    Employee(int id, String name) {
        this.id = id;
        this.name = name;
    }

    void display() {
        System.out.println(
            id + " " + name
        );
    }
}
```

---

## Serialization

```java
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.ObjectOutputStream;

public class SerializeExample {
    public static void main(String[] args) {

        Employee employee =
            new Employee(101, "Rahul");

        try (
            FileOutputStream fileOutput =
                new FileOutputStream("employee.ser");

            ObjectOutputStream output =
                new ObjectOutputStream(fileOutput)
        ) {

            output.writeObject(employee);

            System.out.println(
                "Object serialized"
            );

        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

After execution:

```text
employee.ser
```

contains the serialized representation of the object.

---

# 9. What Exactly Gets Serialized?

Java serialization primarily serializes the **state of the object**, including eligible instance fields.

Suppose:

```java
class Employee implements Serializable {

    int id;
    String name;
}
```

The state includes:

```text
id
name
```

---

## Methods Are Not Serialized

For example:

```java
class Employee implements Serializable {

    int id;

    void display() {
        System.out.println(id);
    }
}
```

The method:

```text
display()
```

is not serialized.

Methods belong to the class definition, not to each object's serialized state.

---

## Class Metadata

The serialization mechanism also stores information needed to reconstruct the object, including class-related serialization metadata.

---

# 10. transient Keyword

Sometimes we do not want a field to be serialized.

Use:

```java
transient
```

Example:

```java
import java.io.Serializable;

class Employee implements Serializable {

    int id;
    String name;

    transient String password;
}
```

Here:

```text
id
→ serialized

name
→ serialized

password
→ not serialized
```

---

# 11. transient Example

```java
import java.io.Serializable;

class User implements Serializable {

    String username;

    transient String password;

    User(String username, String password) {
        this.username = username;
        this.password = password;
    }
}
```

During serialization:

```text
username
→ saved

password
→ skipped
```

After deserialization, a transient field is not restored from the serialized stream.

For a non-static transient field, its value will be the default value for its type unless the class restores it through custom deserialization logic.

For example:

```text
int
→ 0

boolean
→ false

reference
→ null
```

---

# 12. static Fields and Serialization

`static` fields belong to the class, not to an individual object.

Therefore, static fields are not serialized as part of an object's instance state.

Example:

```java
import java.io.Serializable;

class Employee implements Serializable {

    int id;

    static String company =
        "ABC Technologies";
}
```

Here:

```text
id
→ instance field
→ serialized

company
→ static field
→ not serialized as object state
```

---

## 🧠 Interview Trap

People sometimes say:

> "static fields are transient."

That is not technically the right explanation.

The better explanation is:

> Static fields belong to the class rather than the serialized object's instance state, so they are not serialized as instance fields.

---

# 13. serialVersionUID

`serialVersionUID` is a version identifier used by Java's serialization mechanism.

Typical declaration:

```java
private static final long serialVersionUID = 1L;
```

Example:

```java
import java.io.Serializable;

class Employee implements Serializable {

    private static final long serialVersionUID = 1L;

    int id;
    String name;
}
```

---

# 14. Why serialVersionUID Is Important

Suppose version 1 of a class is serialized:

```text
Employee version 1
        ↓
Serialized data
```

Later, the class changes:

```text
Employee version 2
```

During deserialization, Java checks serialization compatibility using the class's serialization version information.

If the serialized object's version is incompatible with the current class, deserialization can fail with:

```text
InvalidClassException
```

---

## Explicit serialVersionUID

```java
private static final long serialVersionUID = 1L;
```

If you intentionally maintain serialization compatibility across certain class changes, explicitly controlling the identifier makes the compatibility contract clearer.

---

## 🧠 Interview Answer

> `serialVersionUID` is a version identifier used during Java serialization to verify compatibility between the serialized object and the current class definition.

---

# 15. Serialization and Inheritance

Serialization interacts with inheritance in an important way.

Consider:

```text
Parent
  ↓
Child
```

If the child class implements `Serializable`, the child can be serialized even if the parent class does not implement `Serializable`.

However, the non-serializable superclass's state is handled differently.

---

# 16. Serializable Parent and Child

If the parent is serializable:

```java
import java.io.Serializable;

class Parent implements Serializable {

    int parentValue;
}
```

and child is also serializable:

```java
class Child extends Parent {

    int childValue;
}
```

then both inherited and child instance state can participate in serialization.

---

# 17. Non-Serializable Parent

Consider:

```java
class Parent {

    int parentValue;

    Parent() {
        parentValue = 100;
    }
}
```

Child:

```java
import java.io.Serializable;

class Child extends Parent
    implements Serializable {

    int childValue;
}
```

The child can be serialized.

However, the state of the non-serializable superclass is not serialized through the normal serializable-field mechanism.

When deserializing the child, the no-argument constructor of the first non-serializable superclass is invoked.

Therefore, that superclass must have an accessible no-argument constructor for normal deserialization.

---

## Example

```java
class Parent {

    int parentValue;

    Parent() {
        parentValue = 100;
    }
}
```

```java
import java.io.Serializable;

class Child extends Parent
    implements Serializable {

    int childValue;

    Child() {
        childValue = 200;
    }
}
```

The serialized representation contains the serializable object's eligible state, while the non-serializable superclass portion is initialized through the superclass constructor during deserialization.

---

# 18. Constructors and Serialization

This is a very common interview question.

Suppose:

```java
import java.io.Serializable;

class Employee implements Serializable {

    Employee() {
        System.out.println(
            "Constructor called"
        );
    }
}
```

During normal deserialization, the serializable class's constructor is **not invoked in the normal way** to reconstruct its serialized state.

Instead, Java reconstructs the serializable object's state from the stream.

However, constructors of the first non-serializable superclass are invoked during deserialization.

---

## 🧠 Remember

```text
Serializable class constructor
→ not invoked normally during deserialization

First non-serializable superclass constructor
→ invoked
```

This is an important interview concept.

---

# 19. Object Graph

Serialization is not limited to one object.

Suppose:

```java
class Department implements Serializable {

    String name;
}
```

and:

```java
class Employee implements Serializable {

    String name;

    Department department;
}
```

If we serialize:

```java
Employee employee;
```

Java follows references and can serialize the reachable object graph.

Conceptually:

```text
Employee
   |
   ↓
Department
```

Both objects can be serialized if the referenced objects are serializable.

---

# 20. Serialization of Referenced Objects

Example:

```java
import java.io.Serializable;

class Address implements Serializable {

    String city;

    Address(String city) {
        this.city = city;
    }
}
```

```java
import java.io.Serializable;

class Employee implements Serializable {

    String name;
    Address address;

    Employee(
        String name,
        Address address
    ) {
        this.name = name;
        this.address = address;
    }
}
```

If:

```text
Employee → Serializable
Address  → Serializable
```

then the referenced `Address` can also be serialized.

---

## ⚠️ Important

If a reachable non-transient field refers to an object that is not serializable, serialization can fail.

---

# 21. NotSerializableException

Suppose:

```java
class Address {

    String city;
}
```

`Address` does not implement `Serializable`.

Now:

```java
import java.io.Serializable;

class Employee implements Serializable {

    String name;
    Address address;
}
```

Trying to serialize an `Employee` whose `address` is non-transient can result in:

```text
NotSerializableException
```

---

## Fix 1 — Make the Referenced Class Serializable

```java
import java.io.Serializable;

class Address implements Serializable {

    String city;
}
```

---

## Fix 2 — Make the Reference Transient

```java
import java.io.Serializable;

class Employee implements Serializable {

    String name;

    transient Address address;
}
```

Then the `Address` object is not serialized through that field.

---

# 22. Serialization of Collections

Many standard Java collection classes are serializable.

For example:

```java
ArrayList
HashMap
HashSet
```

implement `Serializable`.

Example:

```java
import java.io.Serializable;
import java.util.ArrayList;

class Employee implements Serializable {

    String name;

    ArrayList<String> skills =
        new ArrayList<>();
}
```

The collection can participate in serialization.

---

## ⚠️ Important

The elements inside the collection must also be serializable if they are to be serialized as part of the object graph.

For example:

```text
ArrayList
   ↓
Element 1
Element 2
Element 3
```

If one reachable element is not serializable, serialization can fail.

---

# 23. Serialization vs Externalization

Java also provides:

```java
Externalizable
```

which extends:

```java
Serializable
```

Conceptually:

```text
Serializable
→ standard automatic serialization mechanism

Externalizable
→ gives the class explicit control over writing/reading its state
```

With `Externalizable`, the class implements:

```java
writeExternal()
readExternal()
```

Example:

```java
import java.io.Externalizable;
import java.io.IOException;
import java.io.ObjectInput;
import java.io.ObjectOutput;

class Employee implements Externalizable {

    int id;
    String name;

    public Employee() {
    }

    @Override
    public void writeExternal(
        ObjectOutput out
    ) throws IOException {

        out.writeInt(id);
        out.writeObject(name);
    }

    @Override
    public void readExternal(
        ObjectInput in
    ) throws IOException, ClassNotFoundException {

        id = in.readInt();
        name = (String) in.readObject();
    }
}
```

---

## Comparison

| Serializable | Externalizable |
|---|---|
| Marker interface | Interface with methods |
| Automatic serialization mechanism | Explicit control |
| Less code | More code |
| Easier to use | More responsibility |
| Uses serialization machinery | Programmer controls state |

---

# 24. Serialization vs JSON

Java serialization and JSON are different concepts.

| Java Serialization | JSON |
|---|---|
| Java-specific binary format | Text-based data format |
| Uses Java serialization APIs | Uses JSON libraries |
| Can preserve object graph relationships | Usually represents data structures |
| Tied closely to Java class serialization | Language-independent format |
| Can be convenient inside Java systems | Common for APIs and interoperability |

Example JSON:

```text
{
    "id": 101,
    "name": "Rahul"
}
```

Java serialization produces a binary stream rather than human-readable JSON.

---

## 🧠 Modern Backend Perspective

For REST APIs, you will commonly encounter:

```text
Java Object
     ↓
JSON
     ↓
HTTP
     ↓
Other Application
```

rather than Java native serialization.

---

# 25. Advantages

## ✅ 1. Easy Object Persistence

Objects can be converted into a stream without manually writing every field.

---

## ✅ 2. Supports Object Graphs

Referenced serializable objects can be handled as part of the object graph.

---

## ✅ 3. Built Into Java

No third-party library is required for the basic mechanism.

---

## ✅ 4. Convenient for Java-to-Java Use Cases

When both sides use Java serialization-compatible classes, it can be convenient.

---

# 26. Disadvantages

## ❌ 1. Java-Specific

Java native serialization is not an ideal interoperability format for heterogeneous systems.

---

## ❌ 2. Security Risks

Deserializing untrusted data can be dangerous.

---

## ❌ 3. Version Compatibility Issues

Changes to classes can affect deserialization compatibility.

---

## ❌ 4. Less Suitable for Public APIs

For modern REST APIs, formats such as JSON are commonly preferred.

---

## ❌ 5. Tight Coupling

The serialized data is closely connected to Java class structure and serialization rules.

---

# 27. Security Concerns

Java deserialization of untrusted data is a major security concern.

A maliciously crafted serialized stream can potentially trigger dangerous behavior through object construction and gadget chains.

Therefore:

```text
DO NOT
blindly deserialize untrusted data.
```

---

## Safer Principle

Only deserialize data from trusted sources when possible.

For external communication, consider data formats and serialization frameworks designed around explicit schemas and safer data handling.

---

# 28. Common Mistakes

## ❌ Mistake 1 — Thinking `Serializable` Has Methods

It is a marker interface.

```java
class Employee implements Serializable {
}
```

No method implementation is required.

---

## ❌ Mistake 2 — Thinking `new File()` Serializes an Object

Serialization is performed using object streams.

```text
ObjectOutputStream
```

is the key API.

---

## ❌ Mistake 3 — Forgetting Referenced Objects

If an object contains a non-transient reference to a non-serializable object, serialization can fail.

---

## ❌ Mistake 4 — Thinking `transient` Means Permanent Deletion

`transient` means that the field is excluded from default serialization.

The field still exists in the Java object while the program is running.

---

## ❌ Mistake 5 — Thinking Static Fields Are Serialized

Static fields belong to the class, not the serialized object's instance state.

---

## ❌ Mistake 6 — Assuming Constructors Always Run During Deserialization

The serializable class's constructor is not invoked normally during default deserialization.

The first non-serializable superclass's no-argument constructor is invoked.

---

## ❌ Mistake 7 — Ignoring `serialVersionUID`

Class evolution can cause compatibility problems.

Explicitly declaring:

```java
private static final long serialVersionUID = 1L;
```

makes the version identifier explicit.

---

## ❌ Mistake 8 — Deserializing Untrusted Data

This can create serious security risks.

Never blindly deserialize untrusted input.

---

# 29. Interview Questions

## 🔥 Q1. What is serialization?

Serialization is the process of converting an object's state into a byte stream so that it can be stored or transmitted.

---

## 🔥 Q2. What is deserialization?

Deserialization is the process of reconstructing an object from a serialized byte stream.

---

## 🔥 Q3. Which interface is used for Java serialization?

```java
Serializable
```

from:

```text
java.io
```

---

## 🔥 Q4. Is `Serializable` a marker interface?

Yes.

It does not define methods that implementing classes must provide.

---

## 🔥 Q5. Which class is used to serialize objects?

```java
ObjectOutputStream
```

---

## 🔥 Q6. Which method is used to serialize an object?

```java
writeObject()
```

---

## 🔥 Q7. Which class is used for deserialization?

```java
ObjectInputStream
```

---

## 🔥 Q8. What is `serialVersionUID`?

It is a version identifier used to check serialization compatibility between the serialized object and the current class definition.

---

## 🔥 Q9. What happens if `serialVersionUID` is incompatible?

Deserialization can throw:

```text
InvalidClassException
```

---

## 🔥 Q10. What is `transient`?

`transient` marks an instance field to be excluded from default serialization.

---

## 🔥 Q11. What happens to a transient field after deserialization?

It is not restored from the serialized stream and normally receives its default value unless custom deserialization restores it.

---

## 🔥 Q12. Are static variables serialized?

No, static fields are not part of an individual object's serialized instance state.

---

## 🔥 Q13. Are methods serialized?

No.

Methods are part of the class definition, not the object's serialized state.

---

## 🔥 Q14. Can a child class be serialized if the parent is not serializable?

Yes, if the child implements `Serializable`.

However, the non-serializable superclass's state is not serialized through the normal serializable-field mechanism, and its no-argument constructor is invoked during deserialization.

---

## 🔥 Q15. Is a constructor called during deserialization?

The serializable class's constructor is not invoked normally during default deserialization.

The first non-serializable superclass's no-argument constructor is invoked.

---

## 🔥 Q16. What is `NotSerializableException`?

It can occur when Java serialization encounters an object that cannot be serialized.

A common case is a non-transient field referring to a non-serializable object.

---

## 🔥 Q17. What happens if an object contains another object?

Java serialization can follow the object's reachable graph.

The referenced object must also be serializable unless that reference is excluded from serialization, such as with `transient`.

---

## 🔥 Q18. Can collections be serialized?

Yes, many standard collections implement `Serializable`.

Their reachable elements must also be serializable when they are included in the serialized graph.

---

## 🔥 Q19. What is the difference between Serializable and Externalizable?

```text
Serializable
→ marker interface
→ standard automatic mechanism

Externalizable
→ explicit control
→ writeExternal()
→ readExternal()
```

---

## 🔥 Q20. Why is Java serialization considered risky?

Because deserializing untrusted serialized data can enable security vulnerabilities, including gadget-chain attacks.

---

## 🔥 Q21. Why is Java serialization not commonly used for REST APIs?

REST APIs commonly need language-independent, interoperable formats such as JSON.

Java native serialization is Java-specific and tightly coupled to Java class serialization.

---

## 🔥 Q22. Can we serialize a class without implementing Serializable?

Not through the standard default Java object serialization mechanism.

The class normally needs to implement `Serializable` or use another supported serialization mechanism such as `Externalizable`.

---

## 🔥 Q23. What is object graph serialization?

It means serialization can follow references from the root object and serialize reachable serializable objects as part of the graph.

---

## 🔥 Q24. What happens if one referenced object is not serializable?

If that reference is not excluded from serialization, serialization can fail with:

```text
NotSerializableException
```

---

## 🔥 Q25. Is `Serializable` inherited?

The `Serializable` marker is inherited through the class hierarchy in the sense that a subclass of a serializable superclass is itself serializable. A subclass can also explicitly implement `Serializable` even when its superclass does not.

---

## 🔥 Q26. What is the purpose of `ObjectOutputStream`?

It writes primitive values and objects to an underlying `OutputStream`, providing the serialization mechanism for serializable objects.

---

## 🔥 Q27. Why do we use `.ser` files?

`.ser` is a common naming convention for files containing Java serialized objects.

It is not a requirement.

Java serialization does not require a particular file extension.

---

## 🔥 Q28. Is serialized data human-readable?

Normally no.

Java native serialization produces a binary representation.

---

## 🔥 Q29. Can we serialize a `String`?

Yes.

`String` is serializable.

---

## 🔥 Q30. What is the most important security rule regarding deserialization?

Do not blindly deserialize untrusted data.

---

# 30. 30-Second Interview Answer

> Serialization in Java is the process of converting an object's state into a byte stream so it can be stored or transmitted. A class normally implements the `Serializable` marker interface, and `ObjectOutputStream.writeObject()` is used to serialize it. `transient` fields are excluded from default serialization, while static fields are not part of the object's instance state. `serialVersionUID` helps with compatibility between serialized data and the current class definition. Serialization can also traverse an object graph, so referenced objects generally need to be serializable. One major concern is security: untrusted serialized data should not be blindly deserialized.

---

# 31. Cheat Sheet

```text
================ SERIALIZATION =================


DEFINITION
---------------------------------

Object
  ↓
Serialization
  ↓
Byte Stream


REVERSE
---------------------------------

Byte Stream
  ↓
Deserialization
  ↓
Object


MARKER INTERFACE
---------------------------------

Serializable


SERIALIZE
---------------------------------

ObjectOutputStream
        ↓
writeObject()


DESERIALIZE
---------------------------------

ObjectInputStream
        ↓
readObject()


BASIC FLOW
---------------------------------

Object
  ↓
ObjectOutputStream
  ↓
FileOutputStream
  ↓
.ser file


TRANSIENT
---------------------------------

transient field
       ↓
Excluded from default serialization


STATIC
---------------------------------

static field
       ↓
Not part of object instance state


METHODS
---------------------------------

Methods
       ↓
Not serialized


VERSIONING
---------------------------------

serialVersionUID


INCOMPATIBLE VERSION
---------------------------------

InvalidClassException


REFERENCED OBJECT
---------------------------------

Employee
   ↓
Address

Both serializable
       ↓
Can serialize object graph


NON-SERIALIZABLE REFERENCE
---------------------------------

Employee
   ↓
Address
   ↓
Not Serializable

       ↓
NotSerializableException


INHERITANCE
---------------------------------

Serializable Parent
        ↓
Serializable Child


Non-Serializable Parent
        ↓
Serializable Child

Child can be serialized,
but parent state is handled
through the non-serializable
superclass rules.


CONSTRUCTOR
---------------------------------

Serializable class constructor
→ Not normally invoked

First non-serializable
superclass constructor
→ Invoked


EXTERNALIZATION
---------------------------------

Externalizable
       ↓
writeExternal()
readExternal()


SECURITY
---------------------------------

Never blindly deserialize
untrusted data.


MODERN ALTERNATIVES
---------------------------------

JSON
Protocol Buffers
Avro
Other schema-based formats
```

---

# 🧠 Final Mental Model

```text
                     SERIALIZATION
                           |
                           ↓
                    Java Object
                           |
                           ↓
                  ObjectOutputStream
                           |
                           ↓
                     Byte Stream
                           |
              +------------+------------+
              |                         |
             File                    Network
              |
              ↓
        Serialized Data


                     DESERIALIZATION

        Serialized Data
              |
              ↓
       ObjectInputStream
              |
              ↓
          Java Object


IMPORTANT RULES
---------------------------------

Serializable
→ Marks class as eligible

transient
→ Excludes instance field from
  default serialization

static
→ Not object instance state

serialVersionUID
→ Serialization compatibility identifier

Object graph
→ Referenced serializable objects
  can also be serialized

Non-serializable reachable object
→ Can cause NotSerializableException

Untrusted serialized data
→ Do not blindly deserialize
```

---

# 🏁 Key Takeaways

- Serialization converts object state into a byte stream.
- Deserialization reconstructs an object from a byte stream.
- `Serializable` is a marker interface.
- `ObjectOutputStream` performs object serialization.
- `writeObject()` writes an object to the stream.
- `ObjectInputStream` is used for deserialization.
- `transient` excludes an instance field from default serialization.
- Static fields are not part of an object's serialized instance state.
- Methods are not serialized.
- Referenced objects can become part of the serialized object graph.
- Non-serializable reachable objects can cause `NotSerializableException`.
- `serialVersionUID` is used for serialization compatibility.
- An incompatible serialization version can cause `InvalidClassException`.
- The serializable class's constructor is not normally invoked during default deserialization.
- The first non-serializable superclass's no-argument constructor is invoked during deserialization.
- `Externalizable` provides explicit control over serialization.
- Java native serialization is binary and Java-specific.
- JSON and schema-based formats are commonly used for modern application communication.
- Never blindly deserialize untrusted data.
- The core mental model is:

```text
Java Obj07-Deserialization.mdect
    ↓
ObjectOutputStream
    ↓
Byte Stream
    ↓
Storage / Network
```

---


