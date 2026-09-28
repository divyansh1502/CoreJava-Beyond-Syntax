````md
# 🔢 Java Byte Streams

> **Byte streams are Java I/O streams that read and write data as individual bytes using `InputStream` and `OutputStream` hierarchies.**

---

# 📑 Table of Contents

- [1. What Are Byte Streams?](#1-what-are-byte-streams)
- [2. Why Byte Streams?](#2-why-byte-streams)
- [3. InputStream](#3-inputstream)
- [4. OutputStream](#4-outputstream)
- [5. FileInputStream](#5-fileinputstream)
- [6. FileOutputStream](#6-fileoutputstream)
- [7. Reading One Byte at a Time](#7-reading-one-byte-at-a-time)
- [8. Writing One Byte at a Time](#8-writing-one-byte-at-a-time)
- [9. Reading Using a Byte Array](#9-reading-using-a-byte-array)
- [10. Writing Using a Byte Array](#10-writing-using-a-byte-array)
- [11. The `read()` Method](#11-the-read-method)
- [12. The `write()` Method](#12-the-write-method)
- [13. End of File — EOF](#13-end-of-file--eof)
- [14. Why Does `read()` Return `int`?](#14-why-does-read-return-int)
- [15. File Copy Using Byte Streams](#15-file-copy-using-byte-streams)
- [16. Buffer Size and `bytesRead`](#16-buffer-size-and-bytesread)
- [17. FileOutputStream Append Mode](#17-fileoutputstream-append-mode)
- [18. Closing Byte Streams](#18-closing-byte-streams)
- [19. Try-With-Resources](#19-try-with-resources)
- [20. Byte Stream Hierarchy](#20-byte-stream-hierarchy)
- [21. Common Byte Stream Classes](#21-common-byte-stream-classes)
- [22. Byte Streams and Binary Files](#22-byte-streams-and-binary-files)
- [23. Common Mistakes](#23-common-mistakes)
- [24. Interview Questions](#24-interview-questions)
- [25. 30-Second Interview Answer](#25-30-second-interview-answer)
- [26. Cheat Sheet](#26-cheat-sheet)

---

# 1. What Are Byte Streams?

Byte streams are used to process data in the form of **bytes**.

The two fundamental abstract classes are:

```text
InputStream
OutputStream
```

Their basic purpose is:

```text
InputStream
→ Read bytes

OutputStream
→ Write bytes
```

The hierarchy looks conceptually like:

```text
             Byte Streams
                  |
        +---------+---------+
        |                   |
   InputStream        OutputStream
        |                   |
 FileInputStream     FileOutputStream
```

---

# 2. Why Byte Streams?

Computers ultimately store and transfer data as binary information.

Byte streams provide a low-level abstraction for handling that data.

They are especially useful for:

```text
Images
Audio
Video
PDF
ZIP
Executable files
Binary files
Serialized objects
Raw file data
```

For example:

```text
image.jpg
     ↓
FileInputStream
     ↓
bytes
     ↓
Java Program
```

---

# 3. InputStream

## 🎯 Definition

`InputStream` is an abstract class that represents an input stream of bytes.

It is the base class for many byte-oriented input streams.

Some important subclasses are:

```text
FileInputStream
BufferedInputStream
ByteArrayInputStream
ObjectInputStream
```

---

## 📌 Important Methods

Some commonly used methods include:

```text
read()
read(byte[])
read(byte[], offset, length)
available()
skip()
close()
```

---

## 🔥 `read()`

The basic form:

```java
int data = input.read();
```

It reads one byte and returns it as an `int`.

Possible results:

```text
0–255 → valid byte
-1    → end of stream
```

---

## 🔥 `read(byte[])`

Reads multiple bytes into an array.

```java
byte[] buffer = new byte[1024];

int bytesRead = input.read(buffer);
```

The return value tells you how many bytes were actually read.

---

# 4. OutputStream

## 🎯 Definition

`OutputStream` is an abstract class representing an output stream of bytes.

Common subclasses:

```text
FileOutputStream
BufferedOutputStream
ByteArrayOutputStream
ObjectOutputStream
```

---

## 📌 Important Methods

Common methods include:

```text
write(int)
write(byte[])
write(byte[], offset, length)
flush()
close()
```

---

## 🔥 `write(int)`

```java
output.write(65);
```

This writes the low-order 8 bits of the supplied `int` to the stream.

It does not mean:

```text
write the number "65" as two text characters
```

It means:

```text
write one byte
```

---

# 5. FileInputStream

## 🎯 Definition

`FileInputStream` is a byte input stream used to read data from a file.

It extends:

```text
InputStream
```

Hierarchy:

```text
Object
   ↓
InputStream
   ↓
FileInputStream
```

---

## 📥 Basic Example

Suppose `data.txt` contains:

```text
ABC
```

We can read it using:

```java
import java.io.FileInputStream;
import java.io.IOException;

public class ReadFile {
    public static void main(String[] args) throws IOException {

        FileInputStream input =
            new FileInputStream("data.txt");

        int data;

        while ((data = input.read()) != -1) {
            System.out.print((char) data);
        }

        input.close();
    }
}
```

Output:

```text
ABC
```

---

# 6. FileOutputStream

## 🎯 Definition

`FileOutputStream` is a byte output stream used to write data to a file.

It extends:

```text
OutputStream
```

Hierarchy:

```text
Object
   ↓
OutputStream
   ↓
FileOutputStream
```

---

## 📤 Basic Example

```java
import java.io.FileOutputStream;
import java.io.IOException;

public class WriteFile {
    public static void main(String[] args) throws IOException {

        FileOutputStream output =
            new FileOutputStream("data.txt");

        output.write(65);
        output.write(66);
        output.write(67);

        output.close();
    }
}
```

The file contains:

```text
ABC
```

because:

```text
65 → A
66 → B
67 → C
```

---

# 7. Reading One Byte at a Time

The simplest byte-stream reading approach is:

```java
import java.io.FileInputStream;
import java.io.IOException;

public class ReadOneByte {
    public static void main(String[] args) throws IOException {

        FileInputStream input =
            new FileInputStream("data.txt");

        int data;

        while ((data = input.read()) != -1) {
            System.out.println(data);
        }

        input.close();
    }
}
```

If the file contains:

```text
ABC
```

the approximate byte values are:

```text
65
66
67
```

---

## ⚠️ Why `int`?

Notice:

```java
int data;
```

not:

```java
byte data;
```

This is because `read()` needs to represent:

```text
valid byte → 0 to 255
EOF         → -1
```

More on this below.

---

# 8. Writing One Byte at a Time

```java
import java.io.FileOutputStream;
import java.io.IOException;

public class WriteOneByte {
    public static void main(String[] args) throws IOException {

        FileOutputStream output =
            new FileOutputStream("data.txt");

        output.write(72);
        output.write(105);

        output.close();
    }
}
```

The file contains:

```text
Hi
```

because:

```text
72 → H
105 → i
```

---

# 9. Reading Using a Byte Array

Reading one byte at a time can result in many method calls.

Instead, we can read multiple bytes into a buffer.

```java
import java.io.FileInputStream;
import java.io.IOException;

public class ReadBuffer {
    public static void main(String[] args) throws IOException {

        FileInputStream input =
            new FileInputStream("data.txt");

        byte[] buffer = new byte[1024];

        int bytesRead;

        while ((bytesRead = input.read(buffer)) != -1) {

            System.out.println(
                "Bytes read: " + bytesRead
            );
        }

        input.close();
    }
}
```

Here:

```text
buffer
↓
temporary memory area

bytesRead
↓
number of valid bytes currently stored in buffer
```

---

# 10. Writing Using a Byte Array

`OutputStream` can write an entire byte array.

```java
import java.io.FileOutputStream;
import java.io.IOException;

public class WriteBuffer {
    public static void main(String[] args) throws IOException {

        FileOutputStream output =
            new FileOutputStream("data.txt");

        byte[] data = {65, 66, 67};

        output.write(data);

        output.close();
    }
}
```

The file contains:

```text
ABC
```

---

# 11. The `read()` Method

The simplest `read()` method is:

```java
int read()
```

It attempts to read one byte.

---

## 📌 Return Values

```text
0–255
↓
A valid byte

-1
↓
End of stream
```

Example:

```java
int data = input.read();

if (data != -1) {
    System.out.println(data);
}
```

---

## 📌 Reading Multiple Bytes

```java
byte[] buffer = new byte[1024];

int bytesRead = input.read(buffer);
```

Suppose:

```text
buffer size = 1024
```

but only 300 bytes remain in the file.

Then:

```text
bytesRead = 300
```

not:

```text
1024
```

---

# 12. The `write()` Method

There are multiple forms of `write()`.

---

## `write(int)`

```java
output.write(65);
```

Writes one byte.

---

## `write(byte[])`

```java
byte[] data = {65, 66, 67};

output.write(data);
```

Writes the entire array.

---

## `write(byte[], offset, length)`

```java
output.write(data, 0, 2);
```

This means:

```text
Start index = 0
Number of bytes = 2
```

So:

```text
{65, 66, 67}
```

writes:

```text
65
66
```

---

# 13. End of File — EOF

EOF means:

```text
End Of File
```

More generally, in stream terminology, it represents the end of available input.

When no more data is available:

```java
input.read()
```

returns:

```text
-1
```

---

## 🔥 Standard Reading Pattern

This is extremely important:

```java
int data;

while ((data = input.read()) != -1) {
    // process data
}
```

Why is this pattern good?

Because it:

1. Reads the data.
2. Stores it in `data`.
3. Checks whether it is `-1`.
4. Processes it if it is valid.

---

## ❌ Common Mistake

Don't do this when you need the byte:

```java
while (input.read() != -1) {
    // The byte has already been consumed.
}
```

The `read()` call in the condition consumes the byte.

---

# 14. Why Does `read()` Return `int`?

This is a very common interview question.

A byte contains:

```text
8 bits
```

An unsigned byte has values:

```text
0 to 255
```

But Java's `byte` type is signed:

```text
-128 to 127
```

More importantly, `read()` needs a special value to represent EOF:

```text
-1
```

Therefore, Java uses `int` as the return type.

The contract is conceptually:

```text
0–255 → valid byte
-1    → EOF
```

This allows all valid byte values and EOF to be represented distinctly.

---

# 15. File Copy Using Byte Streams

Byte streams are commonly used to copy binary files.

Example:

```java
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class CopyFile {
    public static void main(String[] args) {

        try (
            FileInputStream input =
                new FileInputStream("source.jpg");

            FileOutputStream output =
                new FileOutputStream("copy.jpg")
        ) {

            byte[] buffer = new byte[4096];

            int bytesRead;

            while ((bytesRead = input.read(buffer)) != -1) {

                output.write(
                    buffer,
                    0,
                    bytesRead
                );
            }

        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

---

## 🧠 How It Works

Suppose:

```text
File size = 10,000 bytes
Buffer size = 4,096 bytes
```

The reads might look like:

```text
Read 1 → 4096 bytes
Read 2 → 4096 bytes
Read 3 → 1808 bytes
```

Total:

```text
4096 + 4096 + 1808 = 10000
```

The last read is smaller than the buffer.

That's why we need:

```java
output.write(buffer, 0, bytesRead);
```

---

# 16. Buffer Size and `bytesRead`

Suppose:

```java
byte[] buffer = new byte[1024];
```

This means:

```text
Maximum capacity of this buffer = 1024 bytes
```

It does **not** mean:

```text
Every read will return 1024 bytes.
```

A read may return fewer bytes.

Example:

```text
File remaining = 500 bytes

buffer capacity = 1024

bytesRead = 500
```

---

## 🔥 Correct Copy Pattern

```java
byte[] buffer = new byte[4096];

int bytesRead;

while ((bytesRead = input.read(buffer)) != -1) {
    output.write(buffer, 0, bytesRead);
}
```

This pattern is worth memorizing.

---

## ⚠️ Why Not This?

```java
while ((bytesRead = input.read(buffer)) != -1) {
    output.write(buffer);
}
```

Because the last read may contain fewer valid bytes than the buffer's full capacity.

Writing the entire buffer can write stale bytes left over from a previous iteration.

---

# 17. FileOutputStream Append Mode

By default:

```java
new FileOutputStream("data.txt");
```

opens the file for output in a way that can replace existing contents.

To append:

```java
new FileOutputStream("data.txt", true);
```

---

## 📌 Example

```java
import java.io.FileOutputStream;
import java.io.IOException;

public class AppendFile {
    public static void main(String[] args) throws IOException {

        FileOutputStream output =
            new FileOutputStream("data.txt", true);

        output.write(
            "\nNew Data".getBytes()
        );

        output.close();
    }
}
```

If the file initially contains:

```text
Hello
```

after the operation:

```text
Hello
New Data
```

---

# 18. Closing Byte Streams

After finishing with a stream, it should be closed.

Example:

```java
import java.io.FileInputStream;
import java.io.IOException;

public class CloseStream {
    public static void main(String[] args) throws IOException {

        FileInputStream input =
            new FileInputStream("data.txt");

        // Use stream here.

        input.close();
    }
}
```

---

## 🧠 Why Close?

A file stream may hold operating-system resources such as:

```text
File descriptor / handle
Native resources
Buffers
```

Not closing resources appropriately can cause resource leaks.

---

# 19. Try-With-Resources

The preferred modern pattern is usually try-with-resources.

```java
import java.io.FileInputStream;
import java.io.IOException;

public class AutomaticClose {
    public static void main(String[] args) {

        try (
            FileInputStream input =
                new FileInputStream("data.txt")
        ) {

            int data;

            while ((data = input.read()) != -1) {
                System.out.print((char) data);
            }

        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

When execution leaves the try block, the resource is automatically closed.

---

## 🔥 Multiple Resources

You can declare multiple resources.

```java
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class CopyFile {
    public static void main(String[] args) {

        try (
            FileInputStream input =
                new FileInputStream("source.txt");

            FileOutputStream output =
                new FileOutputStream("copy.txt")
        ) {

            byte[] buffer = new byte[1024];

            int bytesRead;

            while ((bytesRead = input.read(buffer)) != -1) {
                output.write(buffer, 0, bytesRead);
            }

        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

Resources are closed automatically.

---

# 20. Byte Stream Hierarchy

The important hierarchy is:

```text
                         Object
                            |
                 +----------+----------+
                 |                     |
            InputStream          OutputStream
                 |                     |
        +--------+--------+    +-------+--------+
        |                 |    |                |
FileInputStream   BufferedInputStream  FileOutputStream  BufferedOutputStream
```

There are many other subclasses, but these are especially important for file handling.

---

# 21. Common Byte Stream Classes

## `InputStream`

Abstract base class for byte input.

```text
Purpose:
Read bytes
```

---

## `OutputStream`

Abstract base class for byte output.

```text
Purpose:
Write bytes
```

---

## `FileInputStream`

```text
File → Java Program
```

Reads bytes from a file.

---

## `FileOutputStream`

```text
Java Program → File
```

Writes bytes to a file.

---

## `BufferedInputStream`

Adds buffering around another byte input stream.

Example:

```java
import java.io.BufferedInputStream;
import java.io.FileInputStream;
import java.io.IOException;

public class BufferedInputExample {
    public static void main(String[] args) throws IOException {

        try (
            BufferedInputStream input =
                new BufferedInputStream(
                    new FileInputStream("data.txt")
                )
        ) {

            int data;

            while ((data = input.read()) != -1) {
                System.out.print((char) data);
            }
        }
    }
}
```

Detailed buffering is covered separately in:

```text
04-Buffered-Streams.md
```

---

## `BufferedOutputStream`

Adds buffering around another byte output stream.

Example:

```java
import java.io.BufferedOutputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class BufferedOutputExample {
    public static void main(String[] args) throws IOException {

        try (
            BufferedOutputStream output =
                new BufferedOutputStream(
                    new FileOutputStream("data.txt")
                )
        ) {

            output.write("Hello Java".getBytes());
        }
    }
}
```

---

# 22. Byte Streams and Binary Files

Byte streams are particularly important for binary data.

Examples:

```text
photo.jpg
song.mp3
video.mp4
document.pdf
archive.zip
program.exe
```

For example:

```java
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class CopyImage {
    public static void main(String[] args) {

        try (
            FileInputStream input =
                new FileInputStream("photo.jpg");

            FileOutputStream output =
                new FileOutputStream("photo-copy.jpg")
        ) {

            byte[] buffer = new byte[8192];

            int bytesRead;

            while ((bytesRead = input.read(buffer)) != -1) {
                output.write(buffer, 0, bytesRead);
            }

        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

---

## ⚠️ Why Not Read Binary Data Using Character Streams?

Binary files do not represent data as ordinary text characters.

Trying to interpret arbitrary binary bytes as text can cause:

```text
Data corruption
Encoding problems
Incorrect conversion
Loss of information
```

For raw binary copying, byte streams are appropriate.

---

# 23. Common Mistakes

## ❌ Mistake 1 — Confusing `byte` and `int` in `read()`

This:

```java
int data = input.read();
```

is correct.

Don't assume the return type should be:

```java
byte
```

because `-1` must also be represented for EOF.

---

## ❌ Mistake 2 — Losing the Read Value

Avoid:

```java
while (input.read() != -1) {
    // You already consumed the byte.
}
```

Prefer:

```java
int data;

while ((data = input.read()) != -1) {
    // Use data.
}
```

---

## ❌ Mistake 3 — Writing the Entire Buffer

Avoid:

```java
output.write(buffer);
```

when the last read may contain fewer valid bytes.

Prefer:

```java
output.write(buffer, 0, bytesRead);
```

---

## ❌ Mistake 4 — Forgetting Append Mode

These are different:

```java
new FileOutputStream("data.txt");
```

and:

```java
new FileOutputStream("data.txt", true);
```

The second enables append mode.

---

## ❌ Mistake 5 — Using Character Conversion for Binary Files

This is dangerous for arbitrary binary data:

```java
(char) data
```

Character conversion is useful when you intentionally know the data represents text in the appropriate encoding.

---

## ❌ Mistake 6 — Not Closing Streams

Always make sure streams are closed.

Prefer:

```java
try (FileInputStream input =
         new FileInputStream("data.txt")) {

}
```

---

## ❌ Mistake 7 — Assuming Buffer Size Equals Bytes Read

If:

```java
byte[] buffer = new byte[4096];
```

then:

```text
buffer capacity = 4096
```

It does not guarantee:

```text
bytesRead = 4096
```

---

# 24. Interview Questions

## 🔥 Q1. What are byte streams in Java?

Byte streams process data as bytes and are represented primarily by the `InputStream` and `OutputStream` class hierarchies.

---

## 🔥 Q2. What is `InputStream`?

`InputStream` is an abstract class representing byte-oriented input.

---

## 🔥 Q3. What is `OutputStream`?

`OutputStream` is an abstract class representing byte-oriented output.

---

## 🔥 Q4. What is `FileInputStream`?

`FileInputStream` is an `InputStream` implementation used to read bytes from a file.

---

## 🔥 Q5. What is `FileOutputStream`?

`FileOutputStream` is an `OutputStream` implementation used to write bytes to a file.

---

## 🔥 Q6. Why does `read()` return `int`?

Because it must represent all valid byte values from `0` to `255` plus `-1` to indicate EOF.

---

## 🔥 Q7. What does `read()` return at EOF?

```text
-1
```

---

## 🔥 Q8. What does `read(byte[])` return?

It returns the number of bytes actually read into the array.

If the end of the stream has already been reached, it returns:

```text
-1
```

---

## 🔥 Q9. What is the purpose of `bytesRead`?

`bytesRead` tells us how many positions in the buffer contain valid data from the current read operation.

---

## 🔥 Q10. Why do we use `write(buffer, 0, bytesRead)`?

Because the buffer may be larger than the amount of valid data returned by the latest read.

---

## 🔥 Q11. What is append mode in `FileOutputStream`?

Append mode writes new data after the existing contents instead of replacing them.

```java
FileOutputStream output =
    new FileOutputStream("data.txt", true);
```

---

## 🔥 Q12. Can byte streams read text files?

Yes.

Text files are ultimately stored as bytes, so byte streams can read them.

However, if you need proper character decoding and text processing, character-oriented APIs are usually more appropriate.

---

## 🔥 Q13. Can byte streams read images?

Yes.

In fact, byte streams are appropriate for copying and processing raw binary image data.

---

## 🔥 Q14. What is the difference between `InputStream` and `FileInputStream`?

```text
InputStream
→ Abstract base class

FileInputStream
→ Concrete implementation for file input
```

---

## 🔥 Q15. What is the difference between `OutputStream` and `FileOutputStream`?

```text
OutputStream
→ Abstract base class

FileOutputStream
→ Concrete implementation for file output
```

---

## 🔥 Q16. Why should we use a buffer while copying a file?

Reading/writing larger chunks can reduce the number of I/O method calls and generally improve performance compared with processing every byte through a separate read operation.

---

## 🔥 Q17. Does `new FileOutputStream("data.txt")` append data?

No. Opening it this way does not enable append mode.

For append mode:

```java
new FileOutputStream("data.txt", true);
```

---

## 🔥 Q18. What happens if the output file doesn't exist?

`FileOutputStream` can create a new file when opened for output, assuming the path and permissions allow it.

---

## 🔥 Q19. What happens if the input file doesn't exist?

Creating a `FileInputStream` for a nonexistent file generally results in:

```text
FileNotFoundException
```

which is a subclass of `IOException`.

---

## 🔥 Q20. What is the difference between `write(int)` and `write(byte[])`?

```text
write(int)
→ writes one byte

write(byte[])
→ writes bytes from the array
```

---

## 🔥 Q21. What does `write(byte[], offset, length)` mean?

It writes:

```text
length
```

bytes starting from:

```text
offset
```

within the array.

Example:

```java
output.write(buffer, 10, 50);
```

means:

```text
Start at index 10
Write 50 bytes
```

---

## 🔥 Q22. Is `InputStream` an interface?

No.

`InputStream` is an **abstract class**.

Similarly:

```text
OutputStream → abstract class
Reader       → abstract class
Writer       → abstract class
```

---

## 🔥 Q23. What is the difference between `close()` and `flush()`?

For output:

```text
flush()
→ pushes buffered data to the underlying destination

close()
→ closes the resource and performs the required final output handling
```

`flush()` does not mean the resource has been closed.

---

## 🔥 Q24. Can multiple streams be combined?

Yes.

This is commonly done by wrapping one stream inside another.

Example:

```java
BufferedInputStream input =
    new BufferedInputStream(
        new FileInputStream("data.txt")
    );
```

Here:

```text
FileInputStream
        ↓
BufferedInputStream
```

The outer stream adds functionality around the inner stream.

---

## 🔥 Q25. What is the standard pattern for copying a file?

```java
byte[] buffer = new byte[4096];

int bytesRead;

while ((bytesRead = input.read(buffer)) != -1) {
    output.write(buffer, 0, bytesRead);
}
```

This is one of the most useful byte-stream patterns to remember.

---

# 25. 30-Second Interview Answer

> Byte streams are Java I/O streams used to read and write raw byte data. They are based on the `InputStream` and `OutputStream` abstract classes. `FileInputStream` reads bytes from files and `FileOutputStream` writes bytes to files. Byte streams are particularly suitable for binary data such as images, PDFs, videos, and ZIP files. The `read()` method returns an `int` so that it can represent byte values from 0 to 255 as well as `-1` for EOF. For efficient file copying, we usually read multiple bytes into a buffer and write only the number of bytes actually returned by the read operation.

---

# 26. Cheat Sheet

```text
================ BYTE STREAMS =================

BYTE STREAMS
     |
     +----------------------+
     |                      |
InputStream            OutputStream
     |                      |
     ↓                      ↓
Read bytes             Write bytes
     |                      |
     ↓                      ↓
FileInputStream        FileOutputStream


READ
---------------------------------

read()
    ↓
int

0–255
    ↓
Valid byte

-1
    ↓
EOF


BUFFER READ
---------------------------------

byte[] buffer = new byte[4096];

int bytesRead;

while ((bytesRead = input.read(buffer)) != -1) {

    output.write(
        buffer,
        0,
        bytesRead
    );
}


FILE OUTPUT
---------------------------------

Normal:

new FileOutputStream("data.txt")

Append:

new FileOutputStream(
    "data.txt",
    true
)


IMPORTANT CLASSES
---------------------------------

InputStream
OutputStream

FileInputStream
FileOutputStream

BufferedInputStream
BufferedOutputStream


USE BYTE STREAMS FOR
---------------------------------

Images
Audio
Video
PDF
ZIP
Binary files
Raw file copying


KEY RULE
---------------------------------

InputStream
→ Read bytes

OutputStream
→ Write bytes

FileInputStream
→ Read bytes FROM file

FileOutputStream
→ Write bytes TO file
```

---

# 🧠 Final Mental Model

```text
                         BYTE STREAMS
                              |
                 +------------+------------+
                 |                         |
             INPUT                       OUTPUT
                 |                         |
           InputStream                OutputStream
                 |                         |
       FileInputStream            FileOutputStream
                 |                         |
                 ↓                         ↓
              FILE                      FILE
                 |                         |
             Read bytes               Write bytes


              EFFICIENT COPY

Source File
     |
     ↓
FileInputStream
     |
     ↓
byte[] buffer
     |
     ↓
FileOutputStream
     |
     ↓
Destination File


IMPORTANT:

read()
↓
int
↓
0–255 = byte
-1    = EOF

read(buffer)
↓
bytesRead
↓
Number of valid bytes

write(buffer, 0, bytesRead)
↓
Write only valid bytes
```

---

# 🏁 Key Takeaways

- Byte streams process data as bytes.
- `InputStream` is the base abstraction for byte input.
- `OutputStream` is the base abstraction for byte output.
- `FileInputStream` reads bytes from a file.
- `FileOutputStream` writes bytes to a file.
- `read()` returns `0–255` for valid bytes and `-1` for EOF.
- `read(byte[])` returns the number of bytes actually read.
- A buffer's capacity is not necessarily the number of bytes returned by each read.
- `write(buffer, 0, bytesRead)` prevents stale buffer data from being written.
- `FileOutputStream(..., true)` enables append mode.
- Byte streams are especially useful for binary files.
- Try-with-resources is the preferred way to manage stream resources.
- `BufferedInputStream` and `BufferedOutputStream` add buffering around byte streams.
- `InputStream` and `OutputStream` are abstract classes, not interfaces.
- Byte-stream file copying follows the pattern:

```text
Read chunk → Process/Write chunk → Read next chunk
```

---
````
