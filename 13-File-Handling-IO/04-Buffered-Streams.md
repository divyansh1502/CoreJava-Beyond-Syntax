````md
# 🚀 Java Buffered Streams

> **Buffered streams improve I/O efficiency by temporarily storing data in an in-memory buffer, reducing the number of direct read/write operations performed on the underlying stream.**

---

# 📑 Table of Contents

- [1. What Are Buffered Streams?](#1-what-are-buffered-streams)
- [2. Why Do We Need Buffered Streams?](#2-why-do-we-need-buffered-streams)
- [3. Buffer Concept](#3-buffer-concept)
- [4. BufferedInputStream](#4-bufferedinputstream)
- [5. BufferedOutputStream](#5-bufferedoutputstream)
- [6. BufferedReader](#6-bufferedreader)
- [7. BufferedWriter](#7-bufferedwriter)
- [8. Byte Streams vs Buffered Byte Streams](#8-byte-streams-vs-buffered-byte-streams)
- [9. Character Streams vs Buffered Character Streams](#9-character-streams-vs-buffered-character-streams)
- [10. How BufferedInputStream Works](#10-how-bufferedinputstream-works)
- [11. How BufferedOutputStream Works](#11-how-bufferedoutputstream-works)
- [12. How BufferedReader Works](#12-how-bufferedreader-works)
- [13. How BufferedWriter Works](#13-how-bufferedwriter-works)
- [14. Reading a File Using BufferedInputStream](#14-reading-a-file-using-bufferedinputstream)
- [15. Writing a File Using BufferedOutputStream](#15-writing-a-file-using-bufferedoutputstream)
- [16. Reading Text Using BufferedReader](#16-reading-text-using-bufferedreader)
- [17. Reading Line by Line](#17-reading-line-by-line)
- [18. Writing Text Using BufferedWriter](#18-writing-text-using-bufferedwriter)
- [19. `readLine()`](#19-readline)
- [20. `newLine()`](#20-newline)
- [21. `flush()`](#21-flush)
- [22. Buffer Size](#22-buffer-size)
- [23. File Copy Using Buffered Streams](#23-file-copy-using-buffered-streams)
- [24. Wrapping Streams](#24-wrapping-streams)
- [25. Try-With-Resources](#25-try-with-resources)
- [26. Buffered Streams and Performance](#26-buffered-streams-and-performance)
- [27. Common Mistakes](#27-common-mistakes)
- [28. Interview Questions](#28-interview-questions)
- [29. 30-Second Interview Answer](#29-30-second-interview-answer)
- [30. Cheat Sheet](#30-cheat-sheet)

---

# 1. What Are Buffered Streams?

A buffered stream is a stream that uses an **in-memory buffer** between the Java program and the underlying I/O resource.

Instead of repeatedly communicating with the underlying file/device for every small operation, data is transferred in larger chunks.

Conceptually:

```text
Without Buffer:

Java Program
     ↓
File
     ↓
File
     ↓
File
     ↓
File
```

With buffering:

```text
Java Program
     ↓
Buffer
     ↓
File
```

The buffer acts as temporary memory.

---

# 2. Why Do We Need Buffered Streams?

Direct I/O operations can be relatively expensive because they may involve interaction with the operating system and underlying resources.

Suppose a file contains:

```text
1,000,000 bytes
```

Reading one byte at a time can result in a very large number of read operations.

Instead, a buffered stream can fetch data in chunks.

For example:

```text
File
 ↓
4096-byte chunk
 ↓
Buffer
 ↓
Java Program
```

The program can then consume the buffered data without requiring a direct file operation for every individual byte.

---

## 🔥 Main Benefits

Buffered streams can provide:

- Fewer underlying I/O operations.
- Better I/O efficiency.
- Convenient APIs.
- Smoother interaction with files and other streams.
- Line-oriented reading with `BufferedReader`.

---

# 3. Buffer Concept

A buffer is simply a temporary area of memory used to hold data.

Conceptually:

```text
             FILE
              |
              | large chunk
              ↓
        +-------------+
        |   BUFFER    |
        |             |
        |  1024 bytes |
        |             |
        +-------------+
              |
              | small reads
              ↓
        JAVA PROGRAM
```

The important idea is:

```text
Underlying resource
        ↓
     Buffer
        ↓
Application
```

---

# 4. BufferedInputStream

## 🎯 Definition

`BufferedInputStream` is a byte-input stream that adds buffering to another `InputStream`.

Hierarchy:

```text
Object
   ↓
InputStream
   ↓
FilterInputStream
   ↓
BufferedInputStream
```

It is commonly used like this:

```text
FileInputStream
        ↓
BufferedInputStream
```

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

---

# 5. BufferedOutputStream

## 🎯 Definition

`BufferedOutputStream` is a byte-output stream that adds buffering to another `OutputStream`.

Hierarchy:

```text
Object
   ↓
OutputStream
   ↓
FilterOutputStream
   ↓
BufferedOutputStream
```

Common structure:

```text
Java Program
     ↓
BufferedOutputStream
     ↓
FileOutputStream
     ↓
File
```

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

# 6. BufferedReader

## 🎯 Definition

`BufferedReader` is a character-input stream that adds buffering to a `Reader`.

It also provides convenient methods such as:

```text
readLine()
```

Hierarchy:

```text
Object
   ↓
Reader
   ↓
BufferedReader
```

Example:

```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class BufferedReaderExample {
    public static void main(String[] args) throws IOException {

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

# 7. BufferedWriter

## 🎯 Definition

`BufferedWriter` is a character-output stream that adds buffering to a `Writer`.

It also provides:

```text
newLine()
```

Hierarchy:

```text
Object
   ↓
Writer
   ↓
BufferedWriter
```

Example:

```java
import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;

public class BufferedWriterExample {
    public static void main(String[] args) throws IOException {

        try (
            BufferedWriter writer =
                new BufferedWriter(
                    new FileWriter("data.txt")
                )
        ) {

            writer.write("Java");
            writer.newLine();
            writer.write("Spring Boot");
        }
    }
}
```

The file contains:

```text
Java
Spring Boot
```

---

# 8. Byte Streams vs Buffered Byte Streams

| Byte Stream | Buffered Byte Stream |
|---|---|
| `FileInputStream` | `BufferedInputStream` |
| `FileOutputStream` | `BufferedOutputStream` |
| Directly interacts with underlying stream | Adds an in-memory buffer |
| Basic byte I/O | Buffered byte I/O |
| Can be less efficient for many small operations | Can reduce underlying I/O operations |

Memory trick:

```text
FileInputStream
↓
Reads bytes from file

BufferedInputStream
↓
Reads bytes through a buffer
```

---

# 9. Character Streams vs Buffered Character Streams

| Character Stream | Buffered Character Stream |
|---|---|
| `FileReader` | `BufferedReader` |
| `FileWriter` | `BufferedWriter` |
| Character-oriented | Character-oriented + buffering |
| Basic text I/O | Efficient text I/O + convenience methods |

Important:

```text
FileReader
      ↓
BufferedReader
```

and:

```text
FileWriter
      ↓
BufferedWriter
```

The buffered class wraps the underlying stream.

---

# 10. How BufferedInputStream Works

Suppose:

```text
File size = 10,000 bytes
Buffer size = 4096 bytes
```

A simplified sequence can look like:

```text
Initial read:

File
 ↓
4096 bytes
 ↓
Buffer
```

The program then reads from the buffer.

When the buffer becomes empty:

```text
File
 ↓
Next 4096 bytes
 ↓
Buffer
```

Then:

```text
File
 ↓
Remaining 1808 bytes
 ↓
Buffer
```

So instead of repeatedly asking the file for every byte, the buffered stream obtains larger chunks.

---

## 🧠 Important

The exact internal implementation and behavior should be understood as an abstraction rather than assuming a particular low-level system call pattern.

The important concept is:

```text
BufferedInputStream
→ maintains internal buffering
→ reads from underlying InputStream
→ serves data from its buffer
```

---

# 11. How BufferedOutputStream Works

Suppose the program writes:

```text
A
B
C
D
E
```

Without buffering:

```text
A → File
B → File
C → File
D → File
E → File
```

With buffering:

```text
A
B
C
D
E
 ↓
Buffer
 ↓
Underlying OutputStream
```

Once the buffer needs to be flushed or becomes full, buffered data is sent to the underlying stream.

---

## 🔥 Simplified Model

```text
Application
     ↓
BufferedOutputStream
     ↓
Internal Buffer
     ↓
FileOutputStream
     ↓
File
```

---

# 12. How BufferedReader Works

`BufferedReader` maintains an internal character buffer.

Instead of obtaining each character directly from the underlying `Reader`, it can read a block of characters and then serve them from memory.

Conceptually:

```text
File
 ↓
FileReader
 ↓
BufferedReader
 ↓
Character Buffer
 ↓
Application
```

This also enables:

```java
reader.readLine();
```

---

# 13. How BufferedWriter Works

`BufferedWriter` maintains an internal character buffer.

Conceptually:

```text
Application
     ↓
BufferedWriter
     ↓
Character Buffer
     ↓
FileWriter
     ↓
File
```

The program can write characters to the buffer.

The buffered data is eventually sent to the underlying writer.

---

# 14. Reading a File Using BufferedInputStream

```java
import java.io.BufferedInputStream;
import java.io.FileInputStream;
import java.io.IOException;

public class ReadBytes {
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

If the file contains:

```text
Hello
```

the output is:

```text
Hello
```

---

# 15. Writing a File Using BufferedOutputStream

```java
import java.io.BufferedOutputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class WriteBytes {
    public static void main(String[] args) throws IOException {

        try (
            BufferedOutputStream output =
                new BufferedOutputStream(
                    new FileOutputStream("data.txt")
                )
        ) {

            output.write('H');
            output.write('i');
        }
    }
}
```

The buffered stream handles the transfer of the buffered data to the underlying output stream.

Because try-with-resources closes the stream, pending buffered output is also handled during closing.

---

# 16. Reading Text Using BufferedReader

```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class ReadText {
    public static void main(String[] args) throws IOException {

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

If the file contains:

```text
Java
Spring Boot
React
```

the output is:

```text
Java
Spring Boot
React
```

---

# 17. Reading Line by Line

One of the biggest conveniences of `BufferedReader` is:

```java
readLine()
```

Example:

```java
String line = reader.readLine();
```

It returns:

```text
A line of text
```

without the line-termination characters.

---

## 🔥 Standard Pattern

```java
String line;

while ((line = reader.readLine()) != null) {
    System.out.println(line);
}
```

The loop stops when:

```text
readLine()
↓
null
```

---

## 🧠 Important Difference

For byte/character `read()` methods:

```text
EOF → -1
```

For `BufferedReader.readLine()`:

```text
EOF → null
```

This is an important interview point.

---

# 18. Writing Text Using BufferedWriter

```java
import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;

public class WriteText {
    public static void main(String[] args) throws IOException {

        try (
            BufferedWriter writer =
                new BufferedWriter(
                    new FileWriter("data.txt")
                )
        ) {

            writer.write("Java");
            writer.newLine();
            writer.write("Spring Boot");
            writer.newLine();
            writer.write("React");
        }
    }
}
```

The resulting file:

```text
Java
Spring Boot
React
```

---

# 19. `readLine()`

`BufferedReader.readLine()` reads a line of text.

Example:

```java
String line = reader.readLine();
```

Possible results:

```text
"Hello Java"
"Spring Boot"
""
null
```

At the end of the stream:

```text
null
```

is returned.

---

## ⚠️ Empty Line vs EOF

This is important.

Suppose the file contains:

```text
Hello

Java
```

An empty line is still a valid line.

So:

```java
String line = reader.readLine();
```

can return:

```text
""
```

for an empty line.

EOF returns:

```text
null
```

Therefore:

```text
""   → Empty line
null → End of stream
```

---

# 20. `newLine()`

`BufferedWriter` provides:

```java
writer.newLine();
```

It writes the platform's line separator.

Example:

```java
writer.write("Java");
writer.newLine();
writer.write("Spring Boot");
```

Result:

```text
Java
Spring Boot
```

---

## 🧠 Why Use `newLine()`?

Instead of manually assuming a specific line separator:

```text
\n
```

`newLine()` uses the line separator appropriate to the current platform.

---

# 21. `flush()`

`flush()` forces buffered output to be written to the underlying output stream/writer.

Example:

```java
import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;

public class FlushExample {
    public static void main(String[] args) throws IOException {

        try (
            BufferedWriter writer =
                new BufferedWriter(
                    new FileWriter("data.txt")
                )
        ) {

            writer.write("Hello");

            writer.flush();
        }
    }
}
```

---

## 🔥 Why Is `flush()` Important?

Suppose data is currently inside the buffer:

```text
Java Program
     ↓
Buffer
     ↓
File
```

Calling:

```java
flush()
```

causes pending buffered output to be sent toward the underlying stream.

Conceptually:

```text
Buffer
  ↓
flush()
  ↓
Underlying Stream
```

---

## ⚠️ `flush()` vs `close()`

```text
flush()
→ pushes pending output

close()
→ closes the stream and also handles pending output as part of closing
```

After:

```java
writer.close();
```

you should not continue using the writer.

---

# 22. Buffer Size

Buffered streams use an internal buffer.

Conceptually:

```text
Buffer
↓
Fixed amount of memory
```

A larger buffer can reduce the number of refill/flush operations in some workloads, but larger is not automatically better.

---

## Custom Buffer Size

Some constructors allow specifying a buffer size.

Example:

```java
import java.io.BufferedInputStream;
import java.io.FileInputStream;
import java.io.IOException;

public class CustomBuffer {
    public static void main(String[] args) throws IOException {

        try (
            BufferedInputStream input =
                new BufferedInputStream(
                    new FileInputStream("data.txt"),
                    8192
                )
        ) {

            int data;

            while ((data = input.read()) != -1) {
                // Process data
            }
        }
    }
}
```

Here:

```text
8192 bytes
```

is supplied as the requested buffer size.

---

## 🧠 Important

Don't memorize a single buffer size as universally "best."

The appropriate size depends on:

```text
Workload
Underlying resource
Access pattern
Operating system
Application requirements
```

---

# 23. File Copy Using Buffered Streams

Buffered streams are commonly used when copying files.

## 📦 Binary File Copy

```java
import java.io.BufferedInputStream;
import java.io.BufferedOutputStream;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class CopyFile {
    public static void main(String[] args) {

        try (
            BufferedInputStream input =
                new BufferedInputStream(
                    new FileInputStream("source.jpg")
                );

            BufferedOutputStream output =
                new BufferedOutputStream(
                    new FileOutputStream("copy.jpg")
                )
        ) {

            byte[] buffer = new byte[4096];

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

## 🧠 Flow

```text
source.jpg
    ↓
FileInputStream
    ↓
BufferedInputStream
    ↓
byte[] buffer
    ↓
BufferedOutputStream
    ↓
FileOutputStream
    ↓
copy.jpg
```

---

# 24. Wrapping Streams

A major Java I/O concept is **stream wrapping**.

One stream can wrap another stream to add functionality.

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

The outer stream uses the inner stream.

---

## Another Example

```java
BufferedReader reader =
    new BufferedReader(
        new FileReader("data.txt")
    );
```

Here:

```text
FileReader
    ↓
BufferedReader
```

`BufferedReader` adds buffering and `readLine()` to the underlying reader.

---

## 🧠 General Pattern

```text
Base Stream
    ↓
Wrapper Stream
    ↓
Additional Functionality
```

This is a fundamental pattern throughout Java I/O.

---

# 25. Try-With-Resources

Buffered streams should generally be used with try-with-resources.

Example:

```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class ResourceExample {
    public static void main(String[] args) {

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

        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

The outer resource is closed automatically.

---

## Multiple Resources

```java
import java.io.BufferedInputStream;
import java.io.BufferedOutputStream;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class CopyWithResources {
    public static void main(String[] args) {

        try (
            BufferedInputStream input =
                new BufferedInputStream(
                    new FileInputStream("source.txt")
                );

            BufferedOutputStream output =
                new BufferedOutputStream(
                    new FileOutputStream("copy.txt")
                )
        ) {

            byte[] buffer = new byte[4096];

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

# 26. Buffered Streams and Performance

The primary reason for buffering is to reduce the overhead associated with many small interactions with the underlying I/O resource.

Consider:

```text
1,000,000 bytes
```

Reading individually can involve many read operations.

Using a buffer:

```text
1,000,000 bytes
       ↓
larger chunks
       ↓
fewer underlying operations
```

---

## ⚠️ Important Performance Point

Do not say:

> "Buffered streams always make Java programs faster."

A more accurate statement is:

> "Buffered streams can improve I/O efficiency by reducing the number of operations performed against the underlying stream, especially when many small reads or writes are involved."

Actual performance depends on the workload and environment.

---

# 27. Common Mistakes

## ❌ Mistake 1 — Confusing Buffer With File

A buffer is temporary memory.

It is not the file itself.

```text
File
↓
Underlying stream
↓
Buffer
↓
Application
```

---

## ❌ Mistake 2 — Forgetting `flush()`

When using buffered output, pending data may still be in memory.

Use:

```java
writer.flush();
```

when you need to explicitly push pending output.

Closing the stream also handles pending output.

---

## ❌ Mistake 3 — Using `readLine()` and Checking for `-1`

Wrong:

```java
while (reader.readLine() != -1) {
}
```

`readLine()` returns a `String` or `null`, not an `int`.

Correct:

```java
String line;

while ((line = reader.readLine()) != null) {
    System.out.println(line);
}
```

---

## ❌ Mistake 4 — Confusing Empty Line With EOF

```text
""   → Empty line
null → EOF
```

---

## ❌ Mistake 5 — Thinking Buffering Changes the Data

Buffering does not change the logical data.

It changes how the data is transferred.

```text
Same data
Different I/O strategy
```

---

## ❌ Mistake 6 — Using Character Streams for Binary Files

For arbitrary binary data:

```text
Image
PDF
ZIP
Video
Audio
```

use byte-oriented streams.

For text:

```text
TXT
CSV
LOG
JSON
XML
```

character-oriented streams are generally appropriate.

---

## ❌ Mistake 7 — Assuming a Bigger Buffer Is Always Better

A larger buffer consumes more memory and may not provide meaningful performance improvements for every workload.

---

## ❌ Mistake 8 — Double Buffering Without a Reason

For example, unnecessarily wrapping an already buffered stream in another buffering layer can complicate the design without necessarily improving performance.

Understand what each wrapper is doing.

---

# 28. Interview Questions

## 🔥 Q1. What is a buffered stream?

A buffered stream uses an in-memory buffer to reduce the number of direct operations performed on the underlying I/O stream.

---

## 🔥 Q2. Why do we use buffered streams?

They can improve I/O efficiency by reducing the number of interactions with the underlying resource.

---

## 🔥 Q3. What is `BufferedInputStream`?

`BufferedInputStream` is a byte-input stream that adds buffering to another `InputStream`.

---

## 🔥 Q4. What is `BufferedOutputStream`?

`BufferedOutputStream` is a byte-output stream that adds buffering to another `OutputStream`.

---

## 🔥 Q5. What is `BufferedReader`?

`BufferedReader` is a character-input stream that adds buffering to a `Reader` and provides convenient methods such as `readLine()`.

---

## 🔥 Q6. What is `BufferedWriter`?

`BufferedWriter` is a character-output stream that adds buffering to a `Writer` and provides methods such as `newLine()`.

---

## 🔥 Q7. What is the difference between `FileInputStream` and `BufferedInputStream`?

```text
FileInputStream
→ directly represents file byte input

BufferedInputStream
→ wraps an InputStream and adds buffering
```

---

## 🔥 Q8. What is the difference between `FileReader` and `BufferedReader`?

```text
FileReader
→ reads character data from a file

BufferedReader
→ wraps a Reader, adds buffering,
  and provides readLine()
```

---

## 🔥 Q9. What does `BufferedReader.readLine()` return at EOF?

```text
null
```

---

## 🔥 Q10. What is the difference between `read()` and `readLine()`?

```text
read()
→ reads a character/code unit
→ returns int
→ -1 indicates EOF

readLine()
→ reads a complete line
→ returns String
→ null indicates EOF
```

---

## 🔥 Q11. What is `flush()`?

`flush()` forces pending buffered output to be written to the underlying output stream or writer.

---

## 🔥 Q12. Does `flush()` close the stream?

No.

```text
flush()
→ sends pending output

close()
→ closes the resource
```

---

## 🔥 Q13. Does `close()` flush a buffered output stream?

Closing an output stream performs the necessary final flushing of pending output as part of closing.

---

## 🔥 Q14. Why does `BufferedWriter` have `newLine()`?

It writes the platform-specific line separator instead of requiring the programmer to manually specify one.

---

## 🔥 Q15. Why is `BufferedReader` useful?

Besides buffering, it provides convenient text-oriented operations such as:

```java
readLine()
```

---

## 🔥 Q16. Is buffering only useful for files?

No.

Buffered streams can wrap other streams too.

For example:

```text
Network stream
Memory stream
File stream
```

The exact benefits depend on the underlying stream and workload.

---

## 🔥 Q17. Can we wrap one buffered stream around another?

Technically wrappers can be layered, but unnecessary double buffering usually provides little benefit and can complicate the I/O stack.

---

## 🔥 Q18. What is stream wrapping?

Stream wrapping means creating one stream around another stream to add behavior or functionality.

Example:

```java
BufferedReader reader =
    new BufferedReader(
        new FileReader("data.txt")
    );
```

---

## 🔥 Q19. Why is `readLine()` not available in `FileReader`?

`FileReader` provides character reading.

`BufferedReader` provides the higher-level line-reading operation:

```java
readLine()
```

---

## 🔥 Q20. Can `BufferedInputStream` read text?

Yes, it can read the raw bytes of a text file.

However, it does not perform character decoding. If you need characters, use a character-oriented API such as `Reader`.

---

## 🔥 Q21. Can `BufferedReader` read binary files?

It can technically consume bytes through a character-decoding layer, but it is intended for text data. Arbitrary binary data should normally be processed using byte streams.

---

## 🔥 Q22. What happens when a `BufferedOutputStream` buffer becomes full?

The buffered data is written to the underlying output stream, allowing the buffer to accept more output.

---

## 🔥 Q23. What happens when `BufferedInputStream` needs more data?

It obtains more data from its underlying input stream and stores it in its internal buffer.

---

## 🔥 Q24. Is buffering mandatory in Java I/O?

No.

It is an optional layer used when its behavior is appropriate for the workload.

---

## 🔥 Q25. What is the relationship between buffering and performance?

Buffering can improve I/O efficiency by reducing the number of underlying read/write operations, particularly when the application performs many small operations.

---

## 🔥 Q26. What is the difference between a buffer and a cache?

They are related but not identical concepts.

A:

```text
Buffer
→ temporary area used to smooth data transfer.
```

A:

```text
Cache
→ stores data so future access can potentially reuse it more quickly.
```

Java I/O buffering primarily concerns efficient data transfer.

---

## 🔥 Q27. What is the difference between `BufferedInputStream` and `BufferedReader`?

```text
BufferedInputStream
→ buffered byte input

BufferedReader
→ buffered character input
→ provides readLine()
```

---

## 🔥 Q28. What is the difference between `BufferedOutputStream` and `BufferedWriter`?

```text
BufferedOutputStream
→ buffered byte output

BufferedWriter
→ buffered character output
→ provides newLine()
```

---

## 🔥 Q29. Why should buffered streams be closed?

Closing releases the underlying resources and ensures pending buffered output is handled appropriately.

---

## 🔥 Q30. What is the standard buffered file-copy pattern?

```java
byte[] buffer = new byte[4096];

int bytesRead;

while ((bytesRead = input.read(buffer)) != -1) {
    output.write(buffer, 0, bytesRead);
}
```

---

# 29. 30-Second Interview Answer

> Buffered streams improve Java I/O efficiency by using an in-memory buffer between the application and the underlying stream. `BufferedInputStream` and `BufferedOutputStream` are used for byte-oriented I/O, while `BufferedReader` and `BufferedWriter` are used for character-oriented I/O. Buffering reduces the number of direct operations against the underlying resource. `BufferedReader` also provides `readLine()`, while `BufferedWriter` provides `newLine()`. For buffered output, `flush()` explicitly pushes pending data to the underlying stream, and try-with-resources should generally be used to close resources safely.

---

# 30. Cheat Sheet

```text
================ BUFFERED STREAMS =================

BUFFER
---------------------------------

Temporary memory area

Underlying Resource
        ↓
      Buffer
        ↓
    Application


BYTE BUFFERED STREAMS
---------------------------------

Input:

FileInputStream
       ↓
BufferedInputStream


Output:

FileOutputStream
       ↓
BufferedOutputStream


CHARACTER BUFFERED STREAMS
---------------------------------

Input:

FileReader
    ↓
BufferedReader


Output:

FileWriter
    ↓
BufferedWriter


IMPORTANT METHODS
---------------------------------

BufferedInputStream
→ read()

BufferedOutputStream
→ write()
→ flush()

BufferedReader
→ read()
→ readLine()

BufferedWriter
→ write()
→ newLine()
→ flush()


EOF
---------------------------------

read()
→ -1

readLine()
→ null


BUFFER FLOW
---------------------------------

INPUT:

File
 ↓
Underlying InputStream
 ↓
Buffer
 ↓
Application


OUTPUT:

Application
 ↓
Buffer
 ↓
Underlying OutputStream
 ↓
File


FLUSH
---------------------------------

Application
     ↓
   Buffer
     ↓
  flush()
     ↓
Underlying Stream


COMMON COMBINATIONS
---------------------------------

BufferedInputStream(
    FileInputStream
)


BufferedOutputStream(
    FileOutputStream
)


BufferedReader(
    FileReader
)


BufferedWriter(
    FileWriter
)


MEMORY TRICK
---------------------------------

BIS → Byte Input
BOS → Byte Output

BR  → Buffered Reader
BW  → Buffered Writer


KEY DIFFERENCE
---------------------------------

InputStream
→ Bytes

Reader
→ Characters

BufferedInputStream
→ Buffered Bytes

BufferedReader
→ Buffered Characters
```

---

# 🧠 Final Mental Model

```text
                    BUFFERED I/O
                         |
             +-----------+-----------+
             |                       |
          BYTE I/O               CHARACTER I/O
             |                       |
      +------+-------+        +------+-------+
      |              |        |              |
    INPUT          OUTPUT    INPUT          OUTPUT
      |              |        |              |
    BIS            BOS       BR             BW
      |              |        |              |
      ↓              ↓        ↓              ↓
   Bytes          Bytes   Characters     Characters


BIS = BufferedInputStream
BOS = BufferedOutputStream
BR  = BufferedReader
BW  = BufferedWriter


WHY BUFFER?

Without:
Application → Underlying Resource
Application → Underlying Resource
Application → Underlying Resource
Application → Underlying Resource

With:
Application → Buffer → Underlying Resource

             ↓
      Fewer underlying
       I/O operations


TEXT:

File
 ↓
FileReader
 ↓
BufferedReader
 ↓
readLine()
 ↓
String


BINARY:

File
 ↓
FileInputStream
 ↓
BufferedInputStream
 ↓
byte[]
 ↓
Application
```

---

# 🏁 Key Takeaways

- A buffer is temporary memory used to improve I/O efficiency.
- Buffered streams reduce the number of direct operations against the underlying stream.
- `BufferedInputStream` is for buffered byte input.
- `BufferedOutputStream` is for buffered byte output.
- `BufferedReader` is for buffered character input.
- `BufferedWriter` is for buffered character output.
- `BufferedReader.readLine()` returns `null` at EOF.
- `Reader.read()` returns `-1` at EOF.
- `BufferedWriter.newLine()` writes the platform-specific line separator.
- `flush()` pushes pending buffered output to the underlying stream.
- `flush()` does not close the stream.
- Closing a buffered output stream handles pending buffered output as part of closing.
- Buffered streams are wrappers around other streams.
- Buffering does not change the logical data; it changes how the data is transferred.
- A larger buffer is not automatically better.
- Byte-buffered streams are appropriate for raw/binary data.
- Character-buffered streams are appropriate for text.
- `BufferedReader` is especially useful when reading files line by line.
- Try-with-resources is the preferred way to manage these resources.
- The core idea is:

```text
Buffer = temporary memory between the application
         and the underlying I/O resource.
```

---
````
