
# 🎯 Java I/O Interview Questions

> **A focused collection of Java I/O interview questions covering streams, readers/writers, buffering, files, serialization, deserialization, and important internal concepts.**

---

# 📑 Table of Contents

- [1. What Is Java I/O?](#1-what-is-java-io)
- [2. What Is a Stream?](#2-what-is-a-stream)
- [3. Byte Stream vs Character Stream](#3-byte-stream-vs-character-stream)
- [4. InputStream vs OutputStream](#4-inputstream-vs-outputstream)
- [5. Reader vs Writer](#5-reader-vs-writer)
- [6. FileInputStream](#6-fileinputstream)
- [7. FileOutputStream](#7-fileoutputstream)
- [8. FileReader](#8-filereader)
- [9. FileWriter](#9-filewriter)
- [10. Buffered Streams](#10-buffered-streams)
- [11. Why Do We Need Buffering?](#11-why-do-we-need-buffering)
- [12. BufferedReader vs Scanner](#12-bufferedreader-vs-scanner)
- [13. flush() vs close()](#13-flush-vs-close)
- [14. try-with-resources](#14-try-with-resources)
- [15. What Is the File Class?](#15-what-is-the-file-class)
- [16. Does new File() Create a File?](#16-does-new-file-create-a-file)
- [17. mkdir() vs mkdirs()](#17-mkdir-vs-mkdirs)
- [18. list() vs listFiles()](#18-list-vs-listfiles)
- [19. getPath() vs getAbsolutePath() vs getCanonicalPath()](#19-getpath-vs-getabsolutepath-vs-getcanonicalpath)
- [20. File vs Path](#20-file-vs-path)
- [21. Serialization](#21-serialization)
- [22. Deserialization](#22-deserialization)
- [23. Serializable](#23-serializable)
- [24. ObjectOutputStream](#24-objectoutputstream)
- [25. ObjectInputStream](#25-objectinputstream)
- [26. transient](#26-transient)
- [27. static Fields and Serialization](#27-static-fields-and-serialization)
- [28. serialVersionUID](#28-serialversionuid)
- [29. Constructors During Deserialization](#29-constructors-during-deserialization)
- [30. Object Graph](#30-object-graph)
- [31. NotSerializableException](#31-notserializableexception)
- [32. ClassNotFoundException](#32-classnotfoundexception)
- [33. IOException](#33-ioexception)
- [34. Common I/O Mistakes](#34-common-io-mistakes)
- [35. Top 10 Must-Know Questions](#35-top-10-must-know-questions)
- [36. 30-Second Interview Answer](#36-30-second-interview-answer)
- [37. Cheat Sheet](#37-cheat-sheet)

---

# 1. What Is Java I/O?

Java I/O means **Input/Output**.

It provides APIs for:

```text
Reading data
Writing data
Working with files
Working with streams
Reading characters
Reading bytes
Object serialization
Object deserialization
```

The traditional I/O APIs are mainly found in:

```text
java.io
```

Modern Java also provides NIO/NIO.2 APIs under:

```text
java.nio
java.nio.file
```

---

# 2. What Is a Stream?

A stream is an abstraction representing a flow of data.

There are two basic directions:

```text
Input
  ↓
Program
```

and:

```text
Program
  ↓
Output
```

---

## Input Stream

Data comes **into** the program.

```text
File
 ↓
InputStream
 ↓
Program
```

---

## Output Stream

Data goes **out of** the program.

```text
Program
 ↓
OutputStream
 ↓
File
```

---

# 3. Byte Stream vs Character Stream

This is one of the most important Java I/O questions.

## Byte Streams

Used for byte-oriented data.

Main abstract classes:

```text
InputStream
OutputStream
```

Examples:

```text
FileInputStream
FileOutputStream
BufferedInputStream
BufferedOutputStream
```

---

## Character Streams

Used for character-oriented data.

Main abstract classes:

```text
Reader
Writer
```

Examples:

```text
FileReader
FileWriter
BufferedReader
BufferedWriter
```

---

## Comparison

| Byte Stream | Character Stream |
|---|---|
| Works with bytes | Works with characters |
| `InputStream` | `Reader` |
| `OutputStream` | `Writer` |
| Useful for binary data | Useful for text |
| Examples: images, PDFs | Examples: text files |

---

## 🧠 Interview Rule

Do not think:

```text
Byte stream = only numbers
Character stream = only letters
```

The important distinction is the **data representation and character decoding/encoding model**.

---

# 4. InputStream vs OutputStream

Both are abstract classes from:

```text
java.io
```

---

## InputStream

Used for reading bytes.

```java
InputStream input;
```

Basic operation:

```java
int data = input.read();
```

---

## OutputStream

Used for writing bytes.

```java
OutputStream output;
```

Basic operation:

```java
output.write(65);
```

---

## Mental Model

```text
InputStream
→ Read bytes

OutputStream
→ Write bytes
```

---

# 5. Reader vs Writer

These are the character-stream counterparts of `InputStream` and `OutputStream`.

---

## Reader

Reads characters.

```java
Reader reader;
```

Example:

```java
int ch = reader.read();
```

---

## Writer

Writes characters.

```java
Writer writer;
```

Example:

```java
writer.write("Hello");
```

---

## Mental Model

```text
InputStream
→ byte input

OutputStream
→ byte output

Reader
→ character input

Writer
→ character output
```

---

# 6. FileInputStream

`FileInputStream` reads raw bytes from a file.

Package:

```text
java.io
```

Example:

```java
import java.io.FileInputStream;
import java.io.IOException;

public class ReadBytes {

    public static void main(String[] args)
        throws IOException {

        try (
            FileInputStream input =
                new FileInputStream("data.txt")
        ) {

            int data;

            while ((data = input.read()) != -1) {
                System.out.print((char) data);
            }
        }
    }
}
```

---

## `read()` Return Type

This is an important interview question.

```java
int read()
```

returns:

```text
0–255
```

for a byte value, or:

```text
-1
```

when the end of the stream is reached.

---

# 7. FileOutputStream

`FileOutputStream` writes raw bytes to a file.

Example:

```java
import java.io.FileOutputStream;
import java.io.IOException;

public class WriteBytes {

    public static void main(String[] args)
        throws IOException {

        try (
            FileOutputStream output =
                new FileOutputStream("data.txt")
        ) {

            output.write(65);
            output.write(66);
            output.write(67);
        }
    }
}
```

The bytes correspond to:

```text
A
B
C
```

under the ASCII/UTF-8-compatible encoding for these characters.

---

# 8. FileReader

`FileReader` is a character stream used to read text from a file.

Example:

```java
import java.io.FileReader;
import java.io.IOException;

public class ReadCharacters {

    public static void main(String[] args)
        throws IOException {

        try (
            FileReader reader =
                new FileReader("data.txt")
        ) {

            int ch;

            while ((ch = reader.read()) != -1) {
                System.out.print((char) ch);
            }
        }
    }
}
```

---

# 9. FileWriter

`FileWriter` is a character stream used to write text to a file.

Example:

```java
import java.io.FileWriter;
import java.io.IOException;

public class WriteCharacters {

    public static void main(String[] args)
        throws IOException {

        try (
            FileWriter writer =
                new FileWriter("data.txt")
        ) {

            writer.write("Hello Java");
        }
    }
}
```

---

# 10. Buffered Streams

Buffered streams use an in-memory buffer to reduce direct interaction with the underlying resource.

Examples:

```text
BufferedInputStream
BufferedOutputStream
BufferedReader
BufferedWriter
```

---

## Example

```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class BufferedExample {

    public static void main(String[] args)
        throws IOException {

        try (
            BufferedReader reader =
                new BufferedReader(
                    new FileReader("data.txt")
                )
        ) {

            String line;

            while ((line = reader.readLine()) != null) {
                System.out.println(line);
            }
        }
    }
}
```

---

# 11. Why Do We Need Buffering?

Without buffering:

```text
Program
  ↓
I/O operation
  ↓
Operating system / file
```

can happen frequently.

With buffering:

```text
Program
  ↓
Buffer
  ↓
Underlying I/O
```

Multiple small operations can be grouped into larger operations.

This can reduce the number of expensive underlying I/O operations.

---

## 🧠 Important

Buffering does not magically make the storage device faster.

It reduces overhead by reducing the frequency of direct I/O interactions.

---

# 12. BufferedReader vs Scanner

Both can be used to read input, but they serve different purposes.

| BufferedReader | Scanner |
|---|---|
| Character-based buffered input | Token-oriented input utility |
| Generally faster for large input | Convenient parsing |
| `readLine()` available | `nextInt()`, `nextDouble()`, etc. |
| Requires manual parsing in many cases | Built-in token parsing |
| Common in competitive programming | Convenient for interactive/simple input |

Example:

```java
BufferedReader reader =
    new BufferedReader(
        new InputStreamReader(System.in)
    );
```

Reading a line:

```java
String line =
    reader.readLine();
```

---

# 13. flush() vs close()

This is a common interview question.

## `flush()`

For output streams/writers, `flush()` forces buffered output to be written to the underlying destination.

Example:

```java
writer.flush();
```

---

## `close()`

`close()` releases the resource and generally flushes buffered output as part of closing an output stream/writer.

Example:

```java
writer.close();
```

---

## Difference

```text
flush()
→ push buffered output
→ resource remains usable

close()
→ release resource
→ stream/writer should no longer be used
```

---

# 14. try-with-resources

`try-with-resources` automatically closes resources that implement:

```text
AutoCloseable
```

Example:

```java
import java.io.FileReader;
import java.io.IOException;

public class ResourceExample {

    public static void main(String[] args) {

        try (
            FileReader reader =
                new FileReader("data.txt")
        ) {

            int ch;

            while ((ch = reader.read()) != -1) {
                System.out.print((char) ch);
            }

        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

---

## Why Use It?

Without try-with-resources, we may need:

```text
try
finally
close()
```

With try-with-resources:

```text
Resource created
      ↓
Used inside try
      ↓
Automatically closed
```

---

# 15. What Is the File Class?

`File` belongs to:

```text
java.io
```

It represents a pathname for a file or directory.

Example:

```java
import java.io.File;

File file =
    new File("data.txt");
```

---

## Important

Creating the object:

```java
new File("data.txt");
```

does **not** create the physical file.

---

# 16. Does new File() Create a File?

No.

This:

```java
File file =
    new File("data.txt");
```

only creates a Java object representing the pathname.

To create an actual file:

```java
file.createNewFile();
```

---

## 🧠 Interview Answer

> `new File()` creates a `File` object, not a physical filesystem file.

---

# 17. mkdir() vs mkdirs()

## `mkdir()`

Creates a single directory.

```java
File directory =
    new File("documents");

directory.mkdir();
```

---

## `mkdirs()`

Creates the directory and missing parent directories.

```java
File directory =
    new File("a/b/c");

directory.mkdirs();
```

---

## Remember

```text
mkdir()
→ one directory

mkdirs()
→ directory + missing parents
```

---

# 18. list() vs listFiles()

## `list()`

Returns:

```text
String[]
```

Example:

```java
String[] names =
    directory.list();
```

---

## `listFiles()`

Returns:

```text
File[]
```

Example:

```java
File[] files =
    directory.listFiles();
```

---

## Difference

```text
list()
→ names

listFiles()
→ File objects
```

---

# 19. getPath() vs getAbsolutePath() vs getCanonicalPath()

## `getPath()`

Returns the pathname used to construct the `File`.

```java
file.getPath();
```

---

## `getAbsolutePath()`

Returns an absolute pathname.

```java
file.getAbsolutePath();
```

---

## `getCanonicalPath()`

Returns a canonicalized pathname.

It can resolve:

```text
.
..
```

and symbolic links where supported.

```java
file.getCanonicalPath();
```

---

## Interview Shortcut

```text
getPath()
→ original pathname representation

getAbsolutePath()
→ absolute pathname

getCanonicalPath()
→ canonicalized pathname
```

---

# 20. File vs Path

`File` is part of the older `java.io` filesystem API.

`Path` belongs to the newer NIO.2 API:

```text
java.nio.file
```

---

## File

```java
File file =
    new File("data.txt");
```

---

## Path

```java
Path path =
    Path.of("data.txt");
```

---

## Modern API

```text
Path
Files
```

are generally preferred for new filesystem code.

---

## Example

```java
import java.nio.file.Files;
import java.nio.file.Path;

public class ModernFileCheck {

    public static void main(String[] args) {

        Path path =
            Path.of("data.txt");

        System.out.println(
            Files.exists(path)
        );
    }
}
```

---

# 21. Serialization

Serialization converts an object's state into a byte stream.

```text
Object
   ↓
Serialization
   ↓
Byte Stream
```

The class normally implements:

```java
Serializable
```

---

## Example

```java
import java.io.Serializable;

class Employee implements Serializable {

    int id;
    String name;
}
```

---

# 22. Deserialization

Deserialization reconstructs an object from serialized data.

```text
Byte Stream
   ↓
Deserialization
   ↓
Object
```

Main API:

```java
ObjectInputStream
```

Main method:

```java
readObject()
```

---

# 23. Serializable

`Serializable` is a marker interface.

Package:

```text
java.io
```

Example:

```java
import java.io.Serializable;

class Employee implements Serializable {
}
```

It does not require implementing methods.

---

## 🧠 Marker Interface

A marker interface provides information to the Java runtime or related infrastructure.

Examples:

```text
Serializable
Cloneable
```

---

# 24. ObjectOutputStream

`ObjectOutputStream` writes objects to an output stream.

Example:

```java
ObjectOutputStream output =
    new ObjectOutputStream(
        new FileOutputStream("employee.ser")
    );
```

Then:

```java
output.writeObject(employee);
```

---

## Flow

```text
Object
 ↓
ObjectOutputStream
 ↓
FileOutputStream
 ↓
File
```

---

# 25. ObjectInputStream

`ObjectInputStream` reads serialized objects.

Example:

```java
ObjectInputStream input =
    new ObjectInputStream(
        new FileInputStream("employee.ser")
    );
```

Then:

```java
Employee employee =
    (Employee) input.readObject();
```

---

## Flow

```text
File
 ↓
FileInputStream
 ↓
ObjectInputStream
 ↓
readObject()
 ↓
Object
```

---

# 26. transient

The `transient` keyword excludes an instance field from default Java serialization.

Example:

```java
import java.io.Serializable;

class User implements Serializable {

    String username;

    transient String password;
}
```

During serialization:

```text
username
→ serialized

password
→ not serialized
```

After deserialization, the transient field normally has its default value.

---

# 27. static Fields and Serialization

Static fields belong to the class rather than an individual object.

Therefore they are not serialized as part of an object's instance state.

Example:

```java
import java.io.Serializable;

class Employee implements Serializable {

    int id;

    static String company =
        "ABC";
}
```

```text
id
→ instance state

company
→ class-level state
```

---

# 28. serialVersionUID

`serialVersionUID` is a version identifier used by Java serialization.

Example:

```java
private static final long serialVersionUID = 1L;
```

It helps Java determine whether the serialized data is compatible with the current class definition.

---

## Incompatibility

Can result in:

```text
InvalidClassException
```

---

# 29. Constructors During Deserialization

The serializable class's constructor is not normally invoked during default deserialization.

Example:

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

When the object is normally deserialized:

```text
Employee constructor
→ not normally called
```

However, the first non-serializable superclass's no-argument constructor is invoked.

---

# 30. Object Graph

Serialization and deserialization can operate on an object graph.

Example:

```text
Employee
   |
   +---- Address
   |
   +---- Department
```

If these referenced objects are eligible for serialization, their state can be serialized and later reconstructed.

---

# 31. NotSerializableException

This exception can occur when serialization encounters a non-serializable object that is required as part of the serialized object graph.

Example:

```java
class Address {
}
```

```java
import java.io.Serializable;

class Employee implements Serializable {

    Address address;
}
```

Serializing an `Employee` with a non-transient `address` can cause:

```text
NotSerializableException
```

---

## Solutions

Make the referenced class serializable:

```java
import java.io.Serializable;

class Address implements Serializable {
}
```

or exclude the reference:

```java
transient Address address;
```

---

# 32. ClassNotFoundException

During deserialization, Java needs the class definition corresponding to the serialized object.

If the required class cannot be found:

```text
ClassNotFoundException
```

can occur.

Example:

```java
Employee employee =
    (Employee) input.readObject();
```

---

# 33. IOException

`IOException` represents many input/output failures.

Examples:

```text
File not found
Permission failure
Read failure
Write failure
Stream failure
Connection-related I/O failure
```

Example:

```java
try {

    FileInputStream input =
        new FileInputStream("data.txt");

} catch (IOException e) {

    e.printStackTrace();
}
```

---

# 34. Common I/O Mistakes

## ❌ Mistake 1 — Confusing `File` Object With Physical File

Wrong assumption:

```java
new File("data.txt");
```

creates the physical file.

Correct:

```java
file.createNewFile();
```

---

## ❌ Mistake 2 — Using Character Streams for Everything

Binary data such as:

```text
Images
PDFs
Audio
Video
```

should generally be handled using byte-oriented APIs.

---

## ❌ Mistake 3 — Forgetting to Close Resources

Bad:

```java
FileInputStream input =
    new FileInputStream("data.txt");
```

with no closing.

Prefer:

```java
try (
    FileInputStream input =
        new FileInputStream("data.txt")
) {
}
```

---

## ❌ Mistake 4 — Forgetting `flush()`

If manually managing buffered output, data may remain buffered until it is flushed or the stream is closed.

---

## ❌ Mistake 5 — Using `mkdir()` for Nested Directories

Use:

```java
mkdirs();
```

when missing parent directories need to be created.

---

## ❌ Mistake 6 — Forgetting the Cast From `readObject()`

`readObject()` returns:

```text
Object
```

So:

```java
Employee employee =
    (Employee) input.readObject();
```

is commonly required.

---

## ❌ Mistake 7 — Ignoring `serialVersionUID`

Class changes can make old serialized data incompatible.

---

## ❌ Mistake 8 — Deserializing Untrusted Data

Never blindly deserialize attacker-controlled serialized data.

---

# 35. Top 10 Must-Know Questions

## 🔥 Q1. Difference between byte stream and character stream?

```text
Byte stream
→ InputStream / OutputStream
→ byte-oriented

Character stream
→ Reader / Writer
→ character-oriented
```

---

## 🔥 Q2. Why use BufferedReader?

It provides buffered character input and convenient line-based reading through:

```java
readLine()
```

Buffering can reduce the number of underlying I/O operations.

---

## 🔥 Q3. Difference between `flush()` and `close()`?

```text
flush()
→ pushes buffered output
→ resource remains open

close()
→ releases resource
→ stream should no longer be used
```

---

## 🔥 Q4. Why use try-with-resources?

To automatically close resources implementing `AutoCloseable`.

---

## 🔥 Q5. Does `new File()` create a file?

No.

It only creates a Java `File` object representing a pathname.

---

## 🔥 Q6. Difference between `File` and `Path`?

```text
File
→ older java.io API

Path
→ modern java.nio.file API
```

`Path` and `Files` provide a richer filesystem API.

---

## 🔥 Q7. What is serialization?

```text
Object
 ↓
Byte Stream
```

---

## 🔥 Q8. What is deserialization?

```text
Byte Stream
 ↓
Object
```

---

## 🔥 Q9. What does transient do?

It excludes an instance field from default Java serialization.

---

## 🔥 Q10. Why is Java deserialization dangerous?

Because deserializing untrusted input can enable serious security vulnerabilities.

---

# 36. 30-Second Interview Answer

> Java I/O provides APIs for reading and writing data through streams. Byte streams use `InputStream` and `OutputStream` and are suitable for byte-oriented data, while character streams use `Reader` and `Writer` for character-oriented data. Buffered streams reduce the overhead of frequent underlying I/O operations, and try-with-resources automatically closes resources. The older `File` API represents filesystem paths, while modern code generally uses `Path` and `Files`. Java also provides object serialization through `ObjectOutputStream` and deserialization through `ObjectInputStream`. `transient` excludes fields from default serialization, and untrusted serialized data should never be blindly deserialized.

---

# 37. Cheat Sheet

```text
================ JAVA I/O CHEAT SHEET ================


I/O
---------------------------------

Input
→ Data comes into program

Output
→ Data leaves program


BYTE STREAM
---------------------------------

InputStream
OutputStream

Examples:

FileInputStream
FileOutputStream
BufferedInputStream
BufferedOutputStream


CHARACTER STREAM
---------------------------------

Reader
Writer

Examples:

FileReader
FileWriter
BufferedReader
BufferedWriter


READ BYTE
---------------------------------

input.read()


WRITE BYTE
---------------------------------

output.write(data)


READ LINE
---------------------------------

reader.readLine()


BUFFERING
---------------------------------

BufferedInputStream
BufferedOutputStream
BufferedReader
BufferedWriter


BUFFERING PURPOSE
---------------------------------

Reduce frequent underlying I/O operations


FLUSH
---------------------------------

flush()
→ Push buffered output
→ Keep resource open


CLOSE
---------------------------------

close()
→ Release resource


RESOURCE MANAGEMENT
---------------------------------

try-with-resources
→ Automatic closing


FILE CLASS
---------------------------------

java.io.File

Represents:
→ File pathname
→ Directory pathname


PHYSICAL FILE CREATION
---------------------------------

file.createNewFile()


DIRECTORY
---------------------------------

mkdir()
→ one directory

mkdirs()
→ directory + missing parents


DIRECTORY LISTING
---------------------------------

list()
→ String[]

listFiles()
→ File[]


PATH INFORMATION
---------------------------------

getPath()
getAbsolutePath()
getCanonicalPath()


MODERN FILESYSTEM API
---------------------------------

java.nio.file.Path
java.nio.file.Files


SERIALIZATION
---------------------------------

Object
 ↓
ObjectOutputStream
 ↓
Byte Stream


DESERIALIZATION
---------------------------------

Byte Stream
 ↓
ObjectInputStream
 ↓
Object


SERIALIZABLE
---------------------------------

Serializable
→ Marker interface


WRITE OBJECT
---------------------------------

writeObject()


READ OBJECT
---------------------------------

readObject()
→ returns Object


TRANSIENT
---------------------------------

transient
→ Excluded from default serialization


STATIC
---------------------------------

static
→ Not part of serialized instance state


VERSIONING
---------------------------------

serialVersionUID


VERSION MISMATCH
---------------------------------

InvalidClassException


MISSING CLASS
---------------------------------

ClassNotFoundException


NON-SERIALIZABLE OBJECT
---------------------------------

NotSerializableException


GENERAL I/O FAILURE
---------------------------------

IOException


SECURITY
---------------------------------

Never blindly deserialize
untrusted data.
```

---

# 🧠 Final I/O Mental Model

```text
                         JAVA I/O
                            |
          +-----------------+-----------------+
          |                                   |
        INPUT                               OUTPUT
          |                                   |
          ↓                                   ↓
    InputStream                           OutputStream
          |                                   |
    Byte-oriented                        Byte-oriented
          |                                   |
 FileInputStream                      FileOutputStream
          |                                   |
          +-----------------+-----------------+
                            |
                       Character I/O
                            |
                    +-------+-------+
                    |               |
                  Reader          Writer
                    |               |
               FileReader       FileWriter
                    |               |
               BufferedReader  BufferedWriter


                    FILESYSTEM
                        |
                        ↓
                       File
                        |
                        ↓
                 Path + Files
                  (modern API)


                  OBJECT I/O
                        |
          +-------------+-------------+
          |                           |
    Serialization                Deserialization
          |                           |
          ↓                           ↓
ObjectOutputStream             ObjectInputStream
          |                           |
  writeObject()                 readObject()
          |                           |
          ↓                           ↓
    Byte Stream                  Java Object


                  IMPORTANT
                        |
        +---------------+---------------+
        |               |               |
    transient        static      serialVersionUID
        |               |               |
    excluded       class-level    compatibility
    field           state           checking


                  SECURITY
                        |
                        ↓
          Untrusted serialized input
                        |
                        ↓
                 DO NOT TRUST
```

---

# 🏁 Final Revision Points

- Java I/O means input and output operations.
- Streams represent a flow of data.
- `InputStream` and `OutputStream` work with bytes.
- `Reader` and `Writer` work with characters.
- `FileInputStream` reads bytes from files.
- `FileOutputStream` writes bytes to files.
- `FileReader` reads characters from files.
- `FileWriter` writes characters to files.
- Buffered streams reduce the overhead of frequent underlying I/O operations.
- `BufferedReader` provides convenient line-based reading.
- `flush()` pushes buffered output without closing the resource.
- `close()` releases the resource.
- try-with-resources automatically closes `AutoCloseable` resources.
- `File` represents a filesystem pathname; `new File()` does not create a physical file.
- `mkdir()` creates one directory.
- `mkdirs()` can create missing parent directories.
- `list()` returns names.
- `listFiles()` returns `File` objects.
- `Path` and `Files` are the modern NIO.2 filesystem APIs.
- Serialization converts object state into a byte stream.
- Deserialization reconstructs an object from serialized data.
- `Serializable` is a marker interface.
- `ObjectOutputStream` serializes objects.
- `ObjectInputStream` deserializes objects.
- `transient` excludes an instance field from default serialization.
- Static fields are not part of an object's serialized instance state.
- `serialVersionUID` helps with serialization compatibility.
- `NotSerializableException` can occur when a required object cannot be serialized.
- `ClassNotFoundException` can occur when the required class cannot be found during deserialization.
- `InvalidClassException` can occur when serialized data is incompatible with the current class.
- Never blindly deserialize untrusted data.

---

