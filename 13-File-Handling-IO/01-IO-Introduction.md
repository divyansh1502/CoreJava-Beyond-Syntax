````md
# 📥 Java I/O — Introduction

> **Java I/O (Input/Output) is the mechanism used to read data from a source and write data to a destination using streams and related APIs.**

---

# 📑 Table of Contents

- [1. What is I/O?](#1-what-is-io)
- [2. Input vs Output](#2-input-vs-output)
- [3. What is a Stream?](#3-what-is-a-stream)
- [4. Why Does Java Use Streams?](#4-why-does-java-use-streams)
- [5. Java I/O Package](#5-java-io-package)
- [6. Types of I/O Streams](#6-types-of-io-streams)
- [7. Byte Streams](#7-byte-streams)
- [8. Character Streams](#8-character-streams)
- [9. Byte Stream vs Character Stream](#9-byte-stream-vs-character-stream)
- [10. InputStream and OutputStream](#10-inputstream-and-outputstream)
- [11. Reader and Writer](#11-reader-and-writer)
- [12. Source and Destination](#12-source-and-destination)
- [13. Basic I/O Flow](#13-basic-io-flow)
- [14. I/O and Memory](#14-io-and-memory)
- [15. I/O Exceptions](#15-io-exceptions)
- [16. Try-With-Resources](#16-try-with-resources)
- [17. Important I/O Classes](#17-important-io-classes)
- [18. Common Mistakes](#18-common-mistakes)
- [19. Interview Questions](#19-interview-questions)
- [20. 30-Second Interview Answer](#20-30-second-interview-answer)
- [21. Cheat Sheet](#21-cheat-sheet)

---

# 1. What is I/O?

I/O stands for:

```text
Input / Output
```

It represents the movement of data between a Java application and an external source or destination.

Examples of external sources/destinations:

- Keyboard
- Console
- File
- Network
- Memory
- Database through higher-level APIs
- Other devices

The basic idea is:

```text
Input:

External Source
      ↓
Java Program


Output:

Java Program
      ↓
External Destination
```

---

# 2. Input vs Output

## 📥 Input

Input means data enters the Java program.

Examples:

```text
Keyboard → Program
File → Program
Network → Program
```

For example, when a user enters:

```text
25
```

through the keyboard, the Java program receives that data as input.

---

## 📤 Output

Output means data leaves the Java program.

Examples:

```text
Program → Console
Program → File
Program → Network
```

For example:

```java
System.out.println("Hello Java");
```

The program sends output to the console.

---

# 3. What is a Stream?

A **stream** is an abstraction representing a flow of data.

Think of a stream like a pipeline through which data moves.

### Input Stream

```text
Source
  ↓
Stream
  ↓
Java Program
```

### Output Stream

```text
Java Program
  ↓
Stream
  ↓
Destination
```

The important point is:

> A stream represents the flow of data, not necessarily the physical file itself.

---

# 4. Why Does Java Use Streams?

Java can interact with many different sources and destinations.

For example:

```text
File
Keyboard
Network
Memory
Console
```

Instead of designing completely unrelated APIs for every source, Java provides common stream abstractions.

For example:

```text
InputStream
OutputStream
Reader
Writer
```

This gives Java a common model for data transfer.

---

# 5. Java I/O Package

Traditional Java I/O APIs are mainly provided by:

```text
java.io
```

Some important classes and interfaces are:

```text
InputStream
OutputStream

Reader
Writer

File
FileInputStream
FileOutputStream

FileReader
FileWriter

BufferedInputStream
BufferedOutputStream

BufferedReader
BufferedWriter

ObjectInputStream
ObjectOutputStream

Serializable
```

Example:

```java
import java.io.File;
```

---

# 6. Types of I/O Streams

Java's traditional I/O streams can broadly be divided into two categories:

```text
                    I/O
                     |
          +----------+----------+
          |                     |
     Byte Streams         Character Streams
          |                     |
 InputStream              Reader
 OutputStream             Writer
```

---

# 7. Byte Streams

## 🎯 Definition

Byte streams process data as bytes.

The main abstract classes are:

```text
InputStream
OutputStream
```

A byte is an 8-bit unit.

Byte streams are particularly useful for binary data such as:

```text
Images
Audio
Video
PDF
ZIP
Executable files
Other binary files
```

---

## 📥 InputStream

`InputStream` is the base abstraction for reading byte-oriented data.

Common subclasses include:

```text
FileInputStream
BufferedInputStream
ByteArrayInputStream
ObjectInputStream
```

---

## 📤 OutputStream

`OutputStream` is the base abstraction for writing byte-oriented data.

Common subclasses include:

```text
FileOutputStream
BufferedOutputStream
ByteArrayOutputStream
ObjectOutputStream
```

---

## 🧠 Byte Stream Flow

```text
Binary File
     ↓
FileInputStream
     ↓
InputStream
     ↓
Java Program
```

For output:

```text
Java Program
     ↓
OutputStream
     ↓
FileOutputStream
     ↓
Binary File
```

---

# 8. Character Streams

## 🎯 Definition

Character streams are designed for character-oriented text I/O.

The main abstract classes are:

```text
Reader
Writer
```

They are useful when working with textual data.

Examples:

```text
TXT
CSV
Source Code
Configuration Files
Logs
```

---

## 📥 Reader

`Reader` is the base abstraction for character input.

Common subclasses include:

```text
FileReader
BufferedReader
InputStreamReader
StringReader
```

---

## 📤 Writer

`Writer` is the base abstraction for character output.

Common subclasses include:

```text
FileWriter
BufferedWriter
OutputStreamWriter
StringWriter
```

---

# 9. Byte Stream vs Character Stream

| Feature | Byte Stream | Character Stream |
|---|---|---|
| Base input | `InputStream` | `Reader` |
| Base output | `OutputStream` | `Writer` |
| Main unit | Byte | Character |
| Common use | Binary data | Text data |
| Examples | Image, PDF, ZIP | TXT, CSV, source code |
| Common file input | `FileInputStream` | `FileReader` |
| Common file output | `FileOutputStream` | `FileWriter` |

### Memory Trick

```text
BYTE
↓
InputStream / OutputStream

CHARACTER
↓
Reader / Writer
```

---

# 10. InputStream and OutputStream

## 📥 InputStream

`InputStream` represents an input source from which bytes can be read.

Important method:

```java
int read();
```

The method returns:

```text
0–255 → byte value
-1    → end of stream
```

Example:

```java
import java.io.ByteArrayInputStream;
import java.io.IOException;

public class InputStreamExample {
    public static void main(String[] args) throws IOException {

        byte[] data = {65, 66, 67};

        InputStreamExample.readData(data);
    }

    static void readData(byte[] data) throws IOException {

        try (ByteArrayInputStream input =
                 new ByteArrayInputStream(data)) {

            int value;

            while ((value = input.read()) != -1) {
                System.out.println(value);
            }
        }
    }
}
```

The important concept is the return value of `read()`:

```text
Valid byte → 0 to 255
EOF        → -1
```

---

## 📤 OutputStream

`OutputStream` represents a destination to which bytes can be written.

Important method:

```java
void write(int b);
```

Example:

```java
import java.io.ByteArrayOutputStream;
import java.io.IOException;

public class OutputStreamExample {
    public static void main(String[] args) throws IOException {

        try (ByteArrayOutputStream output =
                 new ByteArrayOutputStream()) {

            output.write(65);
            output.write(66);
            output.write(67);

            System.out.println(output);
        }
    }
}
```

The values:

```text
65 → A
66 → B
67 → C
```

---

# 11. Reader and Writer

## 📥 Reader

`Reader` is the base class for character input.

Important method:

```java
int read();
```

Although it is character-oriented, `read()` still returns `int`.

Why?

Because it needs a special value for end-of-stream:

```text
Valid character → non-negative value
EOF             → -1
```

---

## 📤 Writer

`Writer` is the base class for character output.

It provides methods for writing characters and strings.

Example:

```java
import java.io.StringWriter;
import java.io.IOException;

public class WriterExample {
    public static void main(String[] args) throws IOException {

        try (StringWriter writer = new StringWriter()) {

            writer.write("Hello Java");

            System.out.println(writer);
        }
    }
}
```

---

# 12. Source and Destination

A useful way to understand I/O is to identify:

```text
Source
Destination
```

## 📥 Input

The source provides data.

```text
File
Keyboard
Network
Memory
```

Example:

```text
File
 ↓
Input Stream
 ↓
Java Program
```

---

## 📤 Output

The destination receives data.

```text
File
Console
Network
Memory
```

Example:

```text
Java Program
 ↓
Output Stream
 ↓
File
```

---

# 13. Basic I/O Flow

A typical file-reading operation looks conceptually like:

```text
                FILE
                  |
                  | bytes
                  ↓
          FileInputStream
                  |
                  ↓
             InputStream
                  |
                  ↓
            Java Program
```

For writing:

```text
            Java Program
                  |
                  ↓
            OutputStream
                  |
                  ↓
         FileOutputStream
                  |
                  | bytes
                  ↓
                FILE
```

For text:

```text
                FILE
                  |
                  ↓
             FileReader
                  |
                  ↓
               Reader
                  |
                  ↓
            Java Program
```

---

# 14. I/O and Memory

One important misconception is:

> Reading a file does not necessarily mean loading the entire file into memory.

For example, a program can read a large file in small chunks.

```text
Large File
    ↓
Read chunk
    ↓
Process chunk
    ↓
Read next chunk
    ↓
Process chunk
    ↓
...
```

This is especially important when working with large files.

---

## 🧠 Buffering

Java can use buffers to reduce the number of direct I/O operations.

Conceptually:

```text
Without Buffer:

Program
  ↓
File
  ↓
Program
  ↓
File
  ↓
Program
  ↓
File
```

With buffering:

```text
Program
  ↓
Buffer
  ↓
File
```

More details about buffering are covered in:

```text
04-Buffered-Streams.md
```

---

# 15. I/O Exceptions

I/O operations can fail.

Examples:

```text
File does not exist
Permission denied
Disk problem
Invalid path
Connection problem
Unexpected end of data
```

Java commonly represents these problems using:

```text
IOException
```

Example:

```java
import java.io.FileInputStream;
import java.io.IOException;

public class IOExceptionExample {
    public static void main(String[] args) {

        try {
            FileInputStream input =
                new FileInputStream("data.txt");

            input.close();

        } catch (IOException e) {
            System.out.println("I/O operation failed.");
        }
    }
}
```

---

## 🧠 Checked Exception

`IOException` is a checked exception.

Therefore, Java requires the exception to be handled or declared.

### Handling

```java
try {
    // I/O operation
} catch (IOException e) {
    e.printStackTrace();
}
```

### Declaring

```java
public static void readFile() throws IOException {
    // I/O operation
}
```

---

# 16. Try-With-Resources

I/O resources should generally be closed after use.

Java provides **try-with-resources** for automatic resource management.

Example:

```java
import java.io.FileInputStream;
import java.io.IOException;

public class TryWithResourcesExample {
    public static void main(String[] args) {

        try (FileInputStream input =
                 new FileInputStream("data.txt")) {

            int value;

            while ((value = input.read()) != -1) {
                System.out.print((char) value);
            }

        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

The stream is automatically closed when the try block finishes.

---

## 🔥 Why Is This Better?

Without try-with-resources, you may need:

```text
Open resource
     ↓
Use resource
     ↓
Close resource
```

and must ensure closing happens even when an exception occurs.

Try-with-resources handles the closing automatically.

---

## 🧩 AutoCloseable

Try-with-resources works with resources implementing:

```text
AutoCloseable
```

Many Java I/O classes implement `Closeable`, which extends `AutoCloseable`.

Conceptually:

```text
AutoCloseable
      ↑
   Closeable
      ↑
Many I/O Classes
```

---

# 17. Important I/O Classes

## 📌 Core Byte Classes

```text
InputStream
OutputStream

FileInputStream
FileOutputStream

BufferedInputStream
BufferedOutputStream
```

---

## 📌 Core Character Classes

```text
Reader
Writer

FileReader
FileWriter

BufferedReader
BufferedWriter
```

---

## 📌 File and Object Classes

```text
File

ObjectInputStream
ObjectOutputStream

Serializable
```

---

## 🧠 What Each One Does

| Class | Purpose |
|---|---|
| `File` | Represents filesystem path |
| `FileInputStream` | Reads bytes from file |
| `FileOutputStream` | Writes bytes to file |
| `FileReader` | Reads characters from file |
| `FileWriter` | Writes characters to file |
| `BufferedInputStream` | Buffers byte input |
| `BufferedOutputStream` | Buffers byte output |
| `BufferedReader` | Buffers character input |
| `BufferedWriter` | Buffers character output |
| `ObjectInputStream` | Reads serialized objects |
| `ObjectOutputStream` | Writes serialized objects |

---

# 18. Common Mistakes

## ❌ Mistake 1 — Thinking `File` Reads Data

This:

```java
File file = new File("data.txt");
```

does not read the contents of the file.

It creates a `File` object representing the path.

For reading:

```text
FileInputStream
FileReader
BufferedReader
```

are examples of appropriate APIs.

---

## ❌ Mistake 2 — Using Byte Streams for Text Without Understanding Encoding

Text is ultimately represented using bytes when stored or transmitted.

But directly treating arbitrary text bytes as characters can cause encoding problems.

For text, prefer character-oriented APIs with the appropriate encoding when needed.

---

## ❌ Mistake 3 — Forgetting EOF

A common pattern is:

```java
int data;

while ((data = input.read()) != -1) {
    // process data
}
```

Do not ignore the returned value if you need the data.

---

## ❌ Mistake 4 — Forgetting to Close Resources

Bad resource management can lead to resource leaks.

Prefer:

```java
try (FileInputStream input =
         new FileInputStream("data.txt")) {

}
```

---

## ❌ Mistake 5 — Assuming Streams Are Always Files

A stream can represent many kinds of data sources or destinations.

Examples:

```text
File
Memory
Network
Console
```

Therefore:

```text
Stream ≠ File
```

A file is one possible source or destination for a stream.

---

## ❌ Mistake 6 — Confusing Stream Direction

Remember:

```text
Input
→ data comes INTO the program

Output
→ data goes OUT OF the program
```

---

# 19. Interview Questions

## 🔥 Q1. What is Java I/O?

Java I/O is the mechanism through which a Java application reads data from sources and writes data to destinations.

---

## 🔥 Q2. What is a stream?

A stream is an abstraction representing a sequential flow of data between a source and a destination.

---

## 🔥 Q3. What are the two major types of streams?

Traditional Java I/O has:

```text
Byte Streams
Character Streams
```

Byte streams use:

```text
InputStream
OutputStream
```

Character streams use:

```text
Reader
Writer
```

---

## 🔥 Q4. What is the difference between Input and Output?

```text
Input
→ Data enters the Java program.

Output
→ Data leaves the Java program.
```

---

## 🔥 Q5. What is the difference between byte streams and character streams?

Byte streams operate on bytes and are commonly used for binary data.

Character streams are designed for text and character-oriented data.

---

## 🔥 Q6. Why does `InputStream.read()` return `int`?

Because it must represent both a valid byte value and the special EOF value:

```text
0–255 → valid byte
-1    → EOF
```

---

## 🔥 Q7. Is `File` a stream?

No.

`File` represents a filesystem path and provides filesystem-related operations.

It is not itself a stream used to read or write file contents.

---

## 🔥 Q8. Is a stream always connected to a file?

No.

Streams can work with different sources and destinations, including:

```text
Files
Memory
Network
Console
```

---

## 🔥 Q9. Why do we close I/O resources?

To release underlying system resources and ensure output operations are properly completed.

---

## 🔥 Q10. What is try-with-resources?

It is a Java feature that automatically closes resources implementing `AutoCloseable`.

---

## 🔥 Q11. What is `IOException`?

`IOException` represents many failures that can occur during input/output operations.

It is a checked exception.

---

## 🔥 Q12. What package contains traditional Java I/O APIs?

The traditional Java I/O APIs are primarily provided by:

```text
java.io
```

---

## 🔥 Q13. What is the difference between `Reader` and `InputStream`?

```text
InputStream
→ byte-oriented input

Reader
→ character-oriented input
```

---

## 🔥 Q14. What is the difference between `Writer` and `OutputStream`?

```text
OutputStream
→ byte-oriented output

Writer
→ character-oriented output
```

---

## 🔥 Q15. Does reading a file mean loading the entire file into RAM?

No.

Data can be read incrementally or through buffers.

---

# 20. 30-Second Interview Answer

> Java I/O provides APIs for reading and writing data between a Java application and external sources or destinations. The traditional `java.io` package mainly provides byte streams and character streams. Byte streams use `InputStream` and `OutputStream` and are generally used for binary data, while character streams use `Reader` and `Writer` for text. Java also provides buffering classes for efficient I/O and try-with-resources for automatic resource management. A stream represents the flow of data, while a class like `File` represents a filesystem path.

---

# 21. Cheat Sheet

```text
================ JAVA I/O CHEAT SHEET ================

I/O
│
├── INPUT
│   │
│   ├── Byte
│   │   └── InputStream
│   │       └── FileInputStream
│   │
│   └── Character
│       └── Reader
│           └── FileReader
│
└── OUTPUT
    │
    ├── Byte
    │   └── OutputStream
    │       └── FileOutputStream
    │
    └── Character
        └── Writer
            └── FileWriter


BUFFERING
│
├── BufferedInputStream
├── BufferedOutputStream
├── BufferedReader
└── BufferedWriter


FILESYSTEM
│
└── File


OBJECT I/O
│
├── ObjectInputStream
├── ObjectOutputStream
└── Serializable


KEY RULE:

Byte
↓
InputStream / OutputStream

Character
↓
Reader / Writer

Input
↓
Data comes INTO program

Output
↓
Data goes OUT OF program

Stream
↓
Flow of data

File
↓
Filesystem path
```

---

# 🧠 Memory Tricks

### Trick 1 — Direction

```text
IN  → INTO program
OUT → OUT of program
```

### Trick 2 — Data Type

```text
BYTE
↓
Stream

CHARACTER
↓
Reader / Writer
```

### Trick 3 — File

```text
File
↓
Path / metadata

FileInputStream
↓
Read bytes

FileOutputStream
↓
Write bytes

FileReader
↓
Read characters

FileWriter
↓
Write characters
```

### Trick 4 — Buffer

```text
Buffered + Existing Stream
=
Extra buffering / convenience
```

---

# 🎯 Final Mental Model

```text
                         JAVA I/O
                            |
             +--------------+--------------+
             |                             |
           INPUT                         OUTPUT
             |                             |
       +-----+-----+                 +-----+-----+
       |           |                 |           |
     BYTE      CHARACTER           BYTE      CHARACTER
       |           |                 |           |
 InputStream   Reader          OutputStream   Writer
       |           |                 |           |
 FileInput    FileReader       FileOutput    FileWriter
       |           |                 |           |
 Buffered     Buffered         Buffered      Buffered
 Input        Reader           Output        Writer


                        FILE
                         |
                    File Class
                         |
                  Filesystem Path


                     OBJECT
                       |
                Serialization
                       |
                Byte Stream
```

---

# 🏁 Key Takeaways

- I/O means **Input/Output**.
- A stream represents a **flow of data**.
- `InputStream` and `OutputStream` are byte-oriented.
- `Reader` and `Writer` are character-oriented.
- Byte streams are commonly used for binary data.
- Character streams are designed for text.
- `File` represents a filesystem path; it is not a stream.
- `IOException` is a common checked exception for I/O failures.
- Try-with-resources automatically closes compatible resources.
- Data does not have to be loaded completely into memory to be processed.
- Buffering can improve I/O efficiency.
- `java.io` contains the traditional Java I/O APIs.

---
````
