
# 🔄 Java Deserialization

> **Deserialization is the process of reconstructing an object from a serialized byte stream.**

---

# 📑 Table of Contents

- [1. What Is Deserialization?](#1-what-is-deserialization)
- [2. Why Do We Need Deserialization?](#2-why-do-we-need-deserialization)
- [3. Serialization vs Deserialization](#3-serialization-vs-deserialization)
- [4. Deserialization Flow](#4-deserialization-flow)
- [5. ObjectInputStream](#5-objectinputstream)
- [6. readObject()](#6-readobject)
- [7. Basic Deserialization Example](#7-basic-deserialization-example)
- [8. Complete Serialization and Deserialization](#8-complete-serialization-and-deserialization)
- [9. Return Type of readObject()](#9-return-type-of-readobject)
- [10. Type Casting During Deserialization](#10-type-casting-during-deserialization)
- [11. transient Fields During Deserialization](#11-transient-fields-during-deserialization)
- [12. static Fields During Deserialization](#12-static-fields-during-deserialization)
- [13. serialVersionUID and Deserialization](#13-serialversionuid-and-deserialization)
- [14. InvalidClassException](#14-invalidclassexception)
- [15. Deserialization and Constructors](#15-deserialization-and-constructors)
- [16. Deserialization and Inheritance](#16-deserialization-and-inheritance)
- [17. Deserialization of Object Graphs](#17-deserialization-of-object-graphs)
- [18. NotSerializableException](#18-notserializableexception)
- [19. ClassNotFoundException](#19-classnotfoundexception)
- [20. IOException](#20-ioexception)
- [21. Custom Deserialization](#21-custom-deserialization)
- [22. readObject() Custom Method](#22-readobject-custom-method)
- [23. readObject vs readResolve](#23-readobject-vs-readresolve)
- [24. ObjectInputStream and Primitive Data](#24-objectinputstream-and-primitive-data)
- [25. Deserialization of Collections](#25-deserialization-of-collections)
- [26. Deserialization Security](#26-deserialization-security)
- [27. Common Mistakes](#27-common-mistakes)
- [28. Interview Questions](#28-interview-questions)
- [29. 30-Second Interview Answer](#29-30-second-interview-answer)
- [30. Cheat Sheet](#30-cheat-sheet)

---

# 1. What Is Deserialization?

Deserialization is the reverse of serialization.

Serialization:

```text
Object
   ↓
Byte Stream
```

Deserialization:

```text
Byte Stream
   ↓
Object
```

Java provides:

```java
ObjectInputStream
```

for reading serialized objects.

---

# 2. Why Do We Need Deserialization?

Suppose an object was serialized and stored:

```text
Employee Object
      ↓
Serialization
      ↓
employee.ser
```

Later, the application starts again.

The object no longer exists in memory.

We can reconstruct it:

```text
employee.ser
      ↓
Deserialization
      ↓
Employee Object
```

This allows previously serialized object state to be restored.

---

# 3. Serialization vs Deserialization

| Serialization | Deserialization |
|---|---|
| Object → byte stream | Byte stream → object |
| Uses `ObjectOutputStream` | Uses `ObjectInputStream` |
| Uses `writeObject()` | Uses `readObject()` |
| Usually writes data | Reads and reconstructs data |
| `Serializable` required | Serialized class must be compatible |

---

# 4. Deserialization Flow

The basic flow is:

```text
Serialized File
      ↓
FileInputStream
      ↓
ObjectInputStream
      ↓
readObject()
      ↓
Java Object
```

Example:

```java
ObjectInputStream input =
    new ObjectInputStream(
        new FileInputStream("employee.ser")
    );

Object object =
    input.readObject();
```

---

# 5. ObjectInputStream

`ObjectInputStream` is used to read primitive data and objects from an `InputStream`.

Package:

```text
java.io
```

Typical usage:

```java
ObjectInputStream input =
    new ObjectInputStream(
        new FileInputStream("employee.ser")
    );
```

Conceptually:

```text
FileInputStream
       ↓
ObjectInputStream
       ↓
Serialized Object
```

---

# 6. readObject()

The main method used for deserialization is:

```java
readObject()
```

Example:

```java
Object object =
    input.readObject();
```

It reads the serialized representation from the stream and reconstructs the object.

---

## Return Type

`readObject()` returns:

```java
Object
```

Therefore, when the expected class is known, we normally cast it:

```java
Employee employee =
    (Employee) input.readObject();
```

---

# 7. Basic Deserialization Example

Suppose we already have:

```text
employee.ser
```

containing a serialized `Employee`.

Employee class:

```java
import java.io.Serializable;

class Employee implements Serializable {

    private static final long serialVersionUID = 1L;

    int id;
    String name;

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

Now deserialize it:

```java
import java.io.FileInputStream;
import java.io.IOException;
import java.io.ObjectInputStream;

public class DeserializeExample {

    public static void main(String[] args) {

        try (
            FileInputStream fileInput =
                new FileInputStream("employee.ser");

            ObjectInputStream input =
                new ObjectInputStream(fileInput)
        ) {

            Employee employee =
                (Employee) input.readObject();

            employee.display();

        } catch (
            IOException |
            ClassNotFoundException e
        ) {
            e.printStackTrace();
        }
    }
}
```

Possible output:

```text
101 Rahul
```

---

# 8. Complete Serialization and Deserialization

A simple complete example:

## Employee Class

```java
import java.io.Serializable;

class Employee implements Serializable {

    private static final long serialVersionUID = 1L;

    int id;
    String name;

    Employee(int id, String name) {
        this.id = id;
        this.name = name;
    }

    void display() {
        System.out.println(
            "ID: " + id
            + ", Name: " + name
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

public class SerializeEmployee {

    public static void main(String[] args) {

        Employee employee =
            new Employee(101, "Rahul");

        try (
            FileOutputStream file =
                new FileOutputStream("employee.ser");

            ObjectOutputStream output =
                new ObjectOutputStream(file)
        ) {

            output.writeObject(employee);

            System.out.println(
                "Serialization successful"
            );

        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

---

## Deserialization

```java
import java.io.FileInputStream;
import java.io.IOException;
import java.io.ObjectInputStream;

public class DeserializeEmployee {

    public static void main(String[] args) {

        try (
            FileInputStream file =
                new FileInputStream("employee.ser");

            ObjectInputStream input =
                new ObjectInputStream(file)
        ) {

            Employee employee =
                (Employee) input.readObject();

            employee.display();

        } catch (
            IOException |
            ClassNotFoundException e
        ) {
            e.printStackTrace();
        }
    }
}
```

---

# 9. Return Type of readObject()

The return type is:

```java
Object
```

Example:

```java
Object object =
    input.readObject();
```

Because Java cannot know at compile time which serialized class will be stored in the stream.

---

## Example

```java
Object object =
    input.readObject();
```

If we know the object should be an `Employee`:

```java
Employee employee =
    (Employee) object;
```

Or directly:

```java
Employee employee =
    (Employee) input.readObject();
```

---

# 10. Type Casting During Deserialization

Suppose the serialized object is:

```text
Employee
```

`readObject()` returns:

```text
Object
```

Therefore:

```java
Employee employee =
    (Employee) input.readObject();
```

The cast tells Java:

```text
Treat this Object as Employee
```

---

## ⚠️ Wrong Cast

Suppose the actual object is:

```text
Employee
```

but we write:

```java
Student student =
    (Student) input.readObject();
```

This can result in:

```text
ClassCastException
```

---

# 11. transient Fields During Deserialization

Suppose:

```java
import java.io.Serializable;

class User implements Serializable {

    String username;

    transient String password;

    User(
        String username,
        String password
    ) {
        this.username = username;
        this.password = password;
    }
}
```

During serialization:

```text
username
→ serialized

password
→ not serialized
```

After deserialization:

```text
username
→ restored

password
→ default value
```

For a reference type:

```text
password
→ null
```

Example:

```java
System.out.println(
    user.password
);
```

Output:

```text
null
```

---

# 12. static Fields During Deserialization

Static fields belong to the class, not to individual serialized objects.

Example:

```java
import java.io.Serializable;

class Employee implements Serializable {

    int id;

    static String company =
        "ABC Technologies";
}
```

The serialized object does not restore the static field from the stream as part of its instance state.

The value of the static field comes from the currently loaded class.

---

## 🧠 Important

Suppose:

```java
Employee.company = "ABC";
```

Then after deserialization:

```java
Employee.company
```

is still controlled by the current class's static field.

---

# 13. serialVersionUID and Deserialization

Consider:

```java
private static final long serialVersionUID = 1L;
```

This value is stored as part of the serialization metadata.

During deserialization, Java compares the serialized class version information with the currently loaded class.

Conceptually:

```text
Serialized Object
      |
      | serialVersionUID
      ↓
Current Class
      |
      | serialVersionUID
      ↓
Compatibility Check
```

If incompatible:

```text
InvalidClassException
```

can occur.

---

# 14. InvalidClassException

`InvalidClassException` can occur when the serialized data is incompatible with the current class definition.

A common cause is a mismatch in:

```text
serialVersionUID
```

Example:

```java
private static final long serialVersionUID = 1L;
```

Serialized object:

```text
UID = 1
```

Current class:

```text
UID = 2
```

Deserialization can fail.

---

# 15. Deserialization and Constructors

This is one of the most important interview topics.

Consider:

```java
import java.io.Serializable;

class Employee implements Serializable {

    Employee() {
        System.out.println(
            "Employee constructor"
        );
    }
}
```

During normal default deserialization:

```text
Employee constructor
```

is not called in the normal constructor-based way.

The serialized state is reconstructed from the stream.

---

## Non-Serializable Parent

Suppose:

```java
class Parent {

    Parent() {
        System.out.println(
            "Parent constructor"
        );
    }
}
```

and:

```java
import java.io.Serializable;

class Child extends Parent
    implements Serializable {
}
```

During deserialization:

```text
Child constructor
→ not normally called

First non-serializable superclass constructor
→ called
```

---

# 16. Deserialization and Inheritance

Consider:

```text
Parent
  ↓
Child
```

There are two important cases.

---

## Case 1 — Parent Is Serializable

```java
import java.io.Serializable;

class Parent implements Serializable {

    int parentValue;
}
```

```java
class Child extends Parent {

    int childValue;
}
```

The serializable state of both parent and child can participate in serialization and deserialization.

---

## Case 2 — Parent Is Not Serializable

```java
class Parent {

    int parentValue;
}
```

```java
import java.io.Serializable;

class Child extends Parent
    implements Serializable {

    int childValue;
}
```

The child can still be serialized.

But:

```text
Parent instance state
→ not serialized normally

Child instance state
→ serialized
```

During deserialization, the first non-serializable superclass's no-argument constructor is invoked.

---

# 17. Deserialization of Object Graphs

Serialization can involve an entire object graph.

Example:

```text
Employee
   |
   +---- Address
   |
   +---- Department
```

If the referenced objects are serializable, their state can be restored as part of the graph.

---

## Example

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

After deserialization:

```java
Employee employee =
    (Employee) input.readObject();

System.out.println(
    employee.address.city
);
```

The referenced `Address` object is reconstructed as part of the object graph.

---

# 18. NotSerializableException

Suppose:

```java
class Address {

    String city;
}
```

and:

```java
import java.io.Serializable;

class Employee implements Serializable {

    String name;
    Address address;
}
```

Because `Address` is not serializable, serializing an `Employee` containing a non-transient `address` can result in:

```text
NotSerializableException
```

---

## Solution 1

Make `Address` serializable:

```java
import java.io.Serializable;

class Address implements Serializable {

    String city;
}
```

---

## Solution 2

Make the reference transient:

```java
import java.io.Serializable;

class Employee implements Serializable {

    String name;

    transient Address address;
}
```

Then the `Address` reference is not serialized through that field.

---

# 19. ClassNotFoundException

`readObject()` declares:

```java
ClassNotFoundException
```

because Java may need to load the class represented in the serialized stream.

Example:

```java
Employee employee =
    (Employee) input.readObject();
```

The JVM must be able to locate the required class.

If the class cannot be found:

```text
ClassNotFoundException
```

can occur.

---

## 🧠 Why?

Serialized data can contain class information.

During deserialization Java needs the corresponding class definition to reconstruct the object.

---

# 20. IOException

Deserialization also involves I/O operations.

Therefore:

```java
IOException
```

can occur.

Example:

```java
try {

    ObjectInputStream input =
        new ObjectInputStream(
            new FileInputStream("employee.ser")
        );

} catch (IOException e) {

    e.printStackTrace();
}
```

Possible causes include:

```text
File does not exist
Permission problem
Corrupted stream
Underlying I/O failure
```

---

# 21. Custom Deserialization

Java allows a serializable class to customize how it is deserialized.

A class can define:

```java
private void readObject(
    ObjectInputStream in
)
```

This method can be used to control the restoration of the object's state.

---

# 22. readObject() Custom Method

Example:

```java
import java.io.IOException;
import java.io.ObjectInputStream;
import java.io.Serializable;

class User implements Serializable {

    private static final long serialVersionUID = 1L;

    String username;
    transient String password;

    User(
        String username,
        String password
    ) {
        this.username = username;
        this.password = password;
    }

    private void readObject(
        ObjectInputStream in
    ) throws IOException, ClassNotFoundException {

        in.defaultReadObject();

        password = "default-password";
    }
}
```

---

## `defaultReadObject()`

Inside custom `readObject()`:

```java
in.defaultReadObject();
```

asks the serialization mechanism to restore the default serializable fields.

Then custom logic can run.

Conceptually:

```text
Serialized data
      ↓
defaultReadObject()
      ↓
Restore normal fields
      ↓
Custom restoration logic
```

---

# 23. readObject vs readResolve

These methods have different purposes.

## `readObject()`

Used to customize deserialization of the object's state.

```java
private void readObject(
    ObjectInputStream in
)
```

---

## `readResolve()`

Used to control which object is returned after deserialization.

Example concept:

```java
private Object readResolve() {
    return INSTANCE;
}
```

This is commonly discussed in the context of:

```text
Singleton
Object replacement
Canonical instances
```

---

## Difference

```text
readObject()
→ controls state restoration

readResolve()
→ can replace the deserialized object
  with another object
```

---

# 24. ObjectInputStream and Primitive Data

`ObjectInputStream` is not limited to objects.

It can also read primitive values.

Examples:

```java
input.readInt();
```

```java
input.readDouble();
```

```java
input.readBoolean();
```

```java
input.readLong();
```

It also provides:

```java
readUTF()
```

for modified UTF-8 strings used by the data stream format.

---

## Example

```java
import java.io.FileInputStream;
import java.io.IOException;
import java.io.ObjectInputStream;

public class ReadData {

    public static void main(String[] args)
        throws IOException {

        try (
            ObjectInputStream input =
                new ObjectInputStream(
                    new FileInputStream("data.dat")
                )
        ) {

            int age =
                input.readInt();

            String name =
                input.readUTF();

            System.out.println(age);
            System.out.println(name);
        }
    }
}
```

The reading order must match the order in which the data was written.

---

# 25. Deserialization of Collections

Many standard collection classes implement `Serializable`.

For example:

```text
ArrayList
HashSet
HashMap
```

can participate in serialization.

Suppose an `ArrayList` was serialized:

```java
ArrayList<String> names =
    new ArrayList<>();
```

After deserialization:

```java
ArrayList<String> names =
    (ArrayList<String>)
        input.readObject();
```

The collection and its serializable elements can be restored.

---

## ⚠️ Important

If a collection contains a non-serializable element, serialization of the collection can fail.

For example:

```text
ArrayList
   |
   +--- String
   |
   +--- String
   |
   +--- NonSerializableObject
```

The non-serializable object can cause:

```text
NotSerializableException
```

---

# 26. Deserialization Security

This is one of the most important real-world topics.

Java native deserialization of untrusted data can be dangerous.

A malicious serialized stream may exploit classes available in the application's classpath through gadget chains.

Therefore:

```text
NEVER blindly deserialize untrusted input.
```

---

## Dangerous Pattern

```java
ObjectInputStream input =
    new ObjectInputStream(
        untrustedInputStream
    );

Object object =
    input.readObject();
```

If the input is attacker-controlled, this can be dangerous.

---

## Safer Approach

For external communication, prefer formats designed around explicit data models, such as:

```text
JSON
Protocol Buffers
Avro
Other schema-based formats
```

When native Java deserialization is unavoidable, apply strict controls such as input validation and appropriate serialization filters.

---

# 27. Common Mistakes

## ❌ Mistake 1 — Forgetting the Cast

Wrong:

```java
Employee employee =
    input.readObject();
```

Because `readObject()` returns:

```text
Object
```

Correct:

```java
Employee employee =
    (Employee) input.readObject();
```

---

## ❌ Mistake 2 — Ignoring `ClassNotFoundException`

`readObject()` can throw:

```text
ClassNotFoundException
```

because the required class may not be available.

---

## ❌ Mistake 3 — Assuming Constructors Run Normally

The serializable class's constructor is not normally invoked during default deserialization.

---

## ❌ Mistake 4 — Forgetting `transient`

Sensitive or intentionally excluded fields should not be serialized by default.

Use:

```java
transient
```

when appropriate.

---

## ❌ Mistake 5 — Expecting Static Fields to Be Restored

Static fields are class-level state, not object instance state.

---

## ❌ Mistake 6 — Ignoring `serialVersionUID`

Class evolution can cause:

```text
InvalidClassException
```

during deserialization.

---

## ❌ Mistake 7 — Deserializing Untrusted Data

Never blindly deserialize attacker-controlled serialized data.

---

## ❌ Mistake 8 — Reading Data in the Wrong Order

If data was written as:

```text
int
String
double
```

it must be read in the corresponding order:

```text
int
String
double
```

---

## ❌ Mistake 9 — Using the Wrong Type Cast

If the actual object is:

```text
Employee
```

but you cast it to:

```text
Student
```

you can get:

```text
ClassCastException
```

---

# 28. Interview Questions

## 🔥 Q1. What is deserialization?

Deserialization is the process of reconstructing an object from a serialized byte stream.

---

## 🔥 Q2. Which class is used for deserialization?

```java
ObjectInputStream
```

from:

```text
java.io
```

---

## 🔥 Q3. Which method is used for deserialization?

```java
readObject()
```

---

## 🔥 Q4. What is the return type of `readObject()`?

```java
Object
```

Therefore, a cast is normally required when assigning it to a specific class type.

---

## 🔥 Q5. Why does `readObject()` throw `ClassNotFoundException`?

Because Java may need to load the class represented by the serialized stream, and that class may not be available.

---

## 🔥 Q6. What happens to transient fields?

They are not restored from the serialized stream and normally receive default values unless custom deserialization restores them.

---

## 🔥 Q7. What happens to static fields?

They are not serialized as part of the object's instance state.

Their value comes from the currently loaded class.

---

## 🔥 Q8. Is a constructor called during deserialization?

The serializable class's constructor is not normally invoked during default deserialization.

The first non-serializable superclass's no-argument constructor is invoked.

---

## 🔥 Q9. What is `serialVersionUID`?

It is a version identifier used to verify serialization compatibility between the serialized data and the current class definition.

---

## 🔥 Q10. What happens when `serialVersionUID` is incompatible?

Deserialization can throw:

```text
InvalidClassException
```

---

## 🔥 Q11. What is `NotSerializableException`?

It can occur when serialization encounters an object that is not serializable and is not excluded from serialization.

---

## 🔥 Q12. Can a child class be deserialized if its parent is not serializable?

Yes, provided the child is serializable and the non-serializable superclass has an accessible no-argument constructor for normal deserialization.

The non-serializable superclass state is initialized through that constructor rather than restored from the serialized stream.

---

## 🔥 Q13. What is `defaultReadObject()`?

It is a method of `ObjectInputStream` used inside custom `readObject()` implementations to perform the default restoration of serializable fields.

---

## 🔥 Q14. What is custom deserialization?

Custom deserialization means defining a private `readObject()` method to control how an object's state is restored.

---

## 🔥 Q15. Difference between `readObject()` and `readResolve()`?

```text
readObject()
→ controls state restoration

readResolve()
→ can replace the deserialized object
```

---

## 🔥 Q16. Can `ObjectInputStream` read primitive values?

Yes.

Examples:

```java
readInt()
readLong()
readDouble()
readBoolean()
```

---

## 🔥 Q17. Can collections be deserialized?

Yes, if the collection and its serialized elements are compatible with Java serialization.

---

## 🔥 Q18. What happens if the serialized file is corrupted?

Deserialization can fail with an `IOException` or another serialization-related exception depending on the corruption and failure encountered.

---

## 🔥 Q19. What happens if the serialized class is missing?

A `ClassNotFoundException` can occur.

---

## 🔥 Q20. What is the biggest security concern with Java deserialization?

Deserializing untrusted data can lead to serious security vulnerabilities.

---

## 🔥 Q21. Why is a cast required after `readObject()`?

Because:

```java
readObject()
```

returns:

```text
Object
```

The cast converts the reference to the expected specific type.

---

## 🔥 Q22. What happens if the cast is wrong?

A:

```text
ClassCastException
```

can occur.

---

## 🔥 Q23. Can the same serialized object be deserialized multiple times?

Yes, if the serialized data remains available and valid.

Each deserialization operation reconstructs an object from the stream.

---

## 🔥 Q24. Does deserialization create a new object?

Normally, yes. It reconstructs an object instance from the serialized stream.

---

## 🔥 Q25. Why must read order match write order for primitive data?

Because data streams are sequential.

If the writer produces:

```text
int
String
double
```

the reader must consume the values in the corresponding order.

---

## 🔥 Q26. What is object graph deserialization?

It is the reconstruction of the root object and its serialized referenced objects from the serialized stream.

---

## 🔥 Q27. Can a transient field be restored manually?

Yes.

A custom `readObject()` method can assign a value to a transient field after calling:

```java
in.defaultReadObject();
```

---

## 🔥 Q28. What is `readResolve()` commonly used for?

It can be used to ensure that a particular object is returned after deserialization, such as maintaining canonical or singleton instances.

---

## 🔥 Q29. Is Java deserialization recommended for REST APIs?

Native Java serialization is generally not the usual choice for REST APIs. Language-independent formats such as JSON are commonly used instead.

---

## 🔥 Q30. What is the most important rule about deserialization?

Never blindly deserialize untrusted data.

---

# 29. 30-Second Interview Answer

> Deserialization is the process of reconstructing a Java object from a serialized byte stream. Java uses `ObjectInputStream`, and the main method is `readObject()`, which returns `Object`, so a cast is normally required. During deserialization, transient fields are not restored from the stream, static fields are not part of the object's serialized state, and the serializable class's constructor is not normally invoked. `serialVersionUID` is used for compatibility checking, and problems can result in exceptions such as `InvalidClassException` or `ClassNotFoundException`. Most importantly, untrusted serialized data should never be blindly deserialized because of security risks.

---

# 30. Cheat Sheet

```text
================ DESERIALIZATION =================


DEFINITION
---------------------------------

Byte Stream
     ↓
Deserialization
     ↓
Java Object


MAIN CLASS
---------------------------------

ObjectInputStream


MAIN METHOD
---------------------------------

readObject()


RETURN TYPE
---------------------------------

Object


TYPICAL CODE
---------------------------------

Employee employee =
    (Employee) input.readObject();


STREAM FLOW
---------------------------------

File
 ↓
FileInputStream
 ↓
ObjectInputStream
 ↓
readObject()
 ↓
Object


TRANSIENT
---------------------------------

transient field
       ↓
Not restored from serialized data
       ↓
Default value normally


STATIC
---------------------------------

static field
       ↓
Not part of object instance state
       ↓
Current class value is used


VERSION CHECK
---------------------------------

Serialized serialVersionUID
          ↓
Current class serialVersionUID
          ↓
Compatibility check


INCOMPATIBLE
---------------------------------

InvalidClassException


MISSING CLASS
---------------------------------

ClassNotFoundException


I/O PROBLEM
---------------------------------

IOException


NON-SERIALIZABLE OBJECT
---------------------------------

NotSerializableException


CUSTOM DESERIALIZATION
---------------------------------

private void readObject(
    ObjectInputStream in
)


DEFAULT RESTORATION
---------------------------------

in.defaultReadObject();


OBJECT REPLACEMENT
---------------------------------

readResolve()


CONSTRUCTOR
---------------------------------

Serializable class constructor
→ Not normally invoked

First non-serializable
superclass constructor
→ Invoked


OBJECT GRAPH
---------------------------------

Employee
   ↓
Address
   ↓
Department

All eligible serializable
objects can be reconstructed.


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
                  DESERIALIZATION
                         |
                         ↓
                 Serialized Data
                         |
                         ↓
                  FileInputStream
                         |
                         ↓
                 ObjectInputStream
                         |
                         ↓
                    readObject()
                         |
                         ↓
                    Java Object
                         |
             +-----------+-----------+
             |           |           |
          Fields      References   Metadata
             |           |           |
             ↓           ↓           ↓
          Restored    Object       Version
          State       Graph        Check


IMPORTANT:

readObject()
    ↓
returns Object

Therefore:

Employee employee =
    (Employee) input.readObject();


TRANSIENT
    ↓
not restored from stream

STATIC
    ↓
not object instance state

SERIALIZABLE CONSTRUCTOR
    ↓
not normally called

NON-SERIALIZABLE SUPERCLASS
    ↓
no-arg constructor called


POSSIBLE PROBLEMS:

ClassNotFoundException
InvalidClassException
IOException
NotSerializableException
ClassCastException


SECURITY:

Untrusted serialized input
          ↓
       DANGER
          ↓
Never blindly deserialize
```

---

# 🏁 Key Takeaways

- Deserialization converts serialized byte data back into an object.
- `ObjectInputStream` is the primary Java API for object deserialization.
- `readObject()` is the main method.
- `readObject()` returns `Object`.
- A cast is normally required when assigning the result to a specific class.
- `transient` fields are not restored from default serialized state.
- Static fields are not part of the object's serialized instance state.
- `serialVersionUID` is used for compatibility checking.
- Incompatible class versions can cause `InvalidClassException`.
- Missing classes can cause `ClassNotFoundException`.
- Non-serializable reachable objects can cause `NotSerializableException`.
- The serializable class's constructor is not normally invoked during default deserialization.
- The first non-serializable superclass's no-argument constructor is invoked.
- `defaultReadObject()` performs default field restoration inside custom `readObject()`.
- `readResolve()` can replace the object returned after deserialization.
- Object graphs can be reconstructed during deserialization.
- Collections can be deserialized when their contents are compatible with serialization.
- Primitive values can also be read through `ObjectInputStream`.
- Read order must match write order for sequential primitive/data operations.
- Never blindly deserialize untrusted data.
- For modern external communication, formats such as JSON or schema-based protocols are commonly preferred.

---

# 🔗 Serialization → Deserialization

```text
                 SERIALIZATION

        Java Object
             |
             ↓
    ObjectOutputStream
             |
             ↓
        Byte Stream
             |
             ↓
        File / Network


                    ↓
                    ↓
                    ↓


                DESERIALIZATION

        File / Network
             |
             ↓
        Byte Stream
             |
             ↓
    ObjectInputStream
             |
             ↓
        readObject()
             |
             ↓
        Java Object
```

---

