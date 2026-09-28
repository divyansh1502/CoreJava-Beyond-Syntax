````md
# 🔤 Java Character Streams

> **Character streams are Java I/O streams designed for reading and writing character-oriented text data using the `Reader` and `Writer` hierarchies.**

---

# 📑 Table of Contents

- [1. What Are Character Streams?](#1-what-are-character-streams)
- [2. Why Character Streams?](#2-why-character-streams)
- [3. Reader](#3-reader)
- [4. Writer](#4-writer)
- [5. FileReader](#5-filereader)
- [6. FileWriter](#6-filewriter)
- [7. Reading Characters](#7-reading-characters)
- [8. Writing Characters](#8-writing-characters)
- [9. Reading Using a Character Array](#9-reading-using-a-character-array)
- [10. Writing Using a Character Array](#10-writing-using-a-character-array)
- [11. Reading and Writing Strings](#11-reading-and-writing-strings)
- [12. The `read()` Method](#12-the-read-method)
- [13. The `write()` Method](#13-the-write-method)
- [14. Character Encoding](#14-character-encoding)
- [15. FileReader and Encoding](#15-filereader-and-encoding)
- [16. FileWriter and Encoding](#16-filewriter-and-encoding)
- [17. InputStreamReader](#17-inputstreamreader)
- [18. OutputStreamWriter](#18-outputstreamwriter)
- [19. Reader vs InputStream](#19-reader-vs-inputstream)
- [20. Writer vs OutputStream](#20-writer-vs-outputstream)
- [21. Character Streams and Text Files](#21-character-streams-and-text-files)
- [22. Closing Character Streams](#22-closing-character-streams)
- [23. Try-With-Resources](#23-try-with-resources)
- [24. Common Character Stream Classes](#24-common-character-stream-classes)
- [25. Common Mistakes](#25-common-mistakes)
- [26. Interview Questions](#26-interview-questions)
- [27. 30-Second Interview Answer](#27-30-second-interview-answer)
- [28. Cheat Sheet](#28-cheat-sheet)

---

# 1. What Are Character Streams?

Character streams are designed for **character-oriented input and output**.

The two main abstract classes are:

```text
Reader
Writer
```

Their basic purpose is:

```text
Reader
→ Read characters

Writer
→ Write characters
```

The hierarchy can be visualized as:

```text
             Character Streams
                    |
          +---------+---------+
          |                   |
       Reader              Writer
          |                   |
      FileReader          FileWriter
```

---

# 2. Why Character Streams?

Textual data is made up of characters.

Examples:

```text
.txt
.csv
.java
.xml
.json
.log
```

Character streams provide APIs designed around text rather than raw bytes.

For example:

```text
Text File
   ↓
FileReader
   ↓
Reader
   ↓
Java Program
```

And:

```text
Java Program
   ↓
Writer
   ↓
FileWriter
   ↓
Text File
```

---

# 3. Reader

## 🎯 Definition

`Reader` is an abstract class representing a character input stream.

It is the base class for many character-oriented input classes.

Common subclasses include:

```text
FileReader
BufferedReader
InputStreamReader
StringReader
CharArrayReader
```

---

## 📌 Important Methods

Common methods include:

```text
read()
read(char[])
read(char[], offset, length)
skip()
ready()
close()
```

---

## 🔥 `read()`

The basic form is:

```java
int ch = reader.read();
```

It reads one character/code unit and returns it as an `int`.

The return values are conceptually:

```text
Non-negative value → character/code unit
-1                  → end of stream
```

---

# 4. Writer

## 🎯 Definition

`Writer` is an abstract class representing a character output stream.

Common subclasses include:

```text
FileWriter
BufferedWriter
OutputStreamWriter
StringWriter
CharArrayWriter
```

---

## 📌 Important Methods

Common methods include:

```text
write(int)
write(char[])
write(char[], offset, length)
write(String)
append(...)
flush()
close()
```

---

# 5. FileReader

## 🎯 Definition

`FileReader` is a character input stream used to read text from a file.

It extends the `InputStreamReader` family and ultimately works with the underlying byte-oriented file input.

Conceptually:

```text
File
 ↓
FileReader
 ↓
Reader
 ↓
Java Program
```

---

## 📥 Basic Example

Suppose `data.txt` contains:

```text
Hello Java
```

We can read it using:

```java
import java.io.FileReader;
import java.io.IOException;

public class ReadText {
    public static void main(String[] args) throws IOException {

        FileReader reader =
            new FileReader("data.txt");

        int ch;

        while ((ch = reader.read()) != -1) {
            System.out.print((char) ch);
        }

        reader.close();
    }
}
```

Output:

```text
Hello Java
```

---

# 6. FileWriter

## 🎯 Definition

`FileWriter` is a character output stream used to write text to a file.

Conceptually:

```text
Java Program
      ↓
FileWriter
      ↓
File
```

---

## 📤 Basic Example

```java
import java.io.FileWriter;
import java.io.IOException;

public class WriteText {
    public static void main(String[] args) throws IOException {

        FileWriter writer =
            new FileWriter("data.txt");

        writer.write("Hello Java");

        writer.close();
    }
}
```

The file contains:

```text
Hello Java
```

---

# 7. Reading Characters

The simplest approach is reading one character at a time.

```java
import java.io.FileReader;
import java.io.IOException;

public class ReadCharacters {
    public static void main(String[] args) throws IOException {

        try (FileReader reader =
                 new FileReader("data.txt")) {

            int ch;

            while ((ch = reader.read()) != -1) {
                System.out.print((char) ch);
            }
        }
    }
}
```

If the file contains:

```text
ABC
```

the program reads the characters sequentially.

---

# 8. Writing Characters

`Writer` provides character-oriented output.

```java
import java.io.FileWriter;
import java.io.IOException;

public class WriteCharacters {
    public static void main(String[] args) throws IOException {

        try (FileWriter writer =
                 new FileWriter("data.txt")) {

            writer.write('A');
            writer.write('B');
            writer.write('C');
        }
    }
}
```

The file contains:

```text
ABC
```

---

## 🧠 Character Literal vs String

Both can be written:

```java
writer.write('A');
```

and:

```java
writer.write("A");
```

The first uses a `char`.

The second uses a `String`.

`Writer` provides overloads to support both forms.

---

# 9. Reading Using a Character Array

Instead of reading one character at a time, we can read multiple characters into an array.

```java
import java.io.FileReader;
import java.io.IOException;

public class ReadCharArray {
    public static void main(String[] args) throws IOException {

        try (FileReader reader =
                 new FileReader("data.txt")) {

            char[] buffer = new char[1024];

            int charsRead;

            while ((charsRead = reader.read(buffer)) != -1) {

                System.out.println(
                    "Characters read: " + charsRead
                );
            }
        }
    }
}
```

Here:

```text
char[] buffer
↓
Temporary character storage

charsRead
↓
Number of characters actually read
```

---

# 10. Writing Using a Character Array

A `Writer` can write a character array.

```java
import java.io.FileWriter;
import java.io.IOException;

public class WriteCharArray {
    public static void main(String[] args) throws IOException {

        try (FileWriter writer =
                 new FileWriter("data.txt")) {

            char[] data = {'J', 'a', 'v', 'a'};

            writer.write(data);
        }
    }
}
```

The file contains:

```text
Java
```

---

## 🔥 Writing Part of a Character Array

```java
import java.io.FileWriter;
import java.io.IOException;

public class WritePartOfArray {
    public static void main(String[] args) throws IOException {

        try (FileWriter writer =
                 new FileWriter("data.txt")) {

            char[] data = {
                'J', 'a', 'v', 'a', '1', '7'
            };

            writer.write(data, 0, 4);
        }
    }
}
```

Only:

```text
Java
```

is written.

Because:

```text
offset = 0
length = 4
```

---

# 11. Reading and Writing Strings

One major advantage of character-oriented output is convenient string handling.

```java
import java.io.FileWriter;
import java.io.IOException;

public class WriteString {
    public static void main(String[] args) throws IOException {

        try (FileWriter writer =
                 new FileWriter("data.txt")) {

            writer.write("Java");
            writer.write(" is");
            writer.write(" powerful.");
        }
    }
}
```

The file contains:

```text
Java is powerful.
```

---

## 🧠 `Writer.write(String)`

A `Writer` can directly accept a string.

This is convenient for text processing because you don't have to manually convert the string into a byte array.

---

# 12. The `read()` Method

The basic `Reader.read()` method is:

```java
int read();
```

It reads one character/code unit.

Example:

```java
int ch = reader.read();
```

---

## 📌 Return Values

```text
Non-negative value
↓
Character/code unit

-1
↓
End of stream
```

---

## 🔥 Standard Reading Pattern

```java
int ch;

while ((ch = reader.read()) != -1) {
    System.out.print((char) ch);
}
```

This pattern is important.

---

## ⚠️ Why `int` Instead of `char`?

Because the method needs to represent both:

```text
Character/code unit
-1 → EOF
```

A `char` cannot represent `-1`.

Therefore:

```text
read()
↓
int
```

---

# 13. The `write()` Method

`Writer` provides multiple ways to write character data.

---

## `write(int)`

```java
writer.write('A');
```

Writes a character/code unit.

---

## `write(char[])`

```java
char[] data = {'J', 'a', 'v', 'a'};

writer.write(data);
```

Writes the entire array.

---

## `write(char[], offset, length)`

```java
writer.write(data, 0, 2);
```

Writes two characters starting from index `0`.

---

## `write(String)`

```java
writer.write("Hello Java");
```

Writes the string.

---

## `write(String, offset, length)`

```java
writer.write("Hello Java", 0, 5);
```

Writes:

```text
Hello
```

---

# 14. Character Encoding

This is one of the most important concepts in character I/O.

A computer stores data as bytes.

But text consists of characters.

Therefore, we need a mapping between:

```text
Characters
     ↕
Bytes
```

This mapping is called:

```text
Character Encoding
```

Examples:

```text
UTF-8
UTF-16
ISO-8859-1
US-ASCII
```

---

## 🧠 Example

Suppose we have:

```text
Hello
```

The characters must be encoded into bytes when stored in a file.

Conceptually:

```text
Characters
    ↓
Encoding
    ↓
Bytes
    ↓
File
```

When reading:

```text
File
 ↓
Bytes
 ↓
Decoding
 ↓
Characters
```

---

# 15. FileReader and Encoding

A very important modern Java point:

`FileReader` is a convenient character-stream class, but for explicit control over the charset, `InputStreamReader` with a specified `Charset` is often preferable.

For example:

```java
import java.io.FileInputStream;
import java.io.IOException;
import java.io.InputStreamReader;
import java.nio.charset.StandardCharsets;

public class UTF8Reader {
    public static void main(String[] args) throws IOException {

        try (
            InputStreamReader reader =
                new InputStreamReader(
                    new FileInputStream("data.txt"),
                    StandardCharsets.UTF_8
                )
        ) {

            int ch;

            while ((ch = reader.read()) != -1) {
                System.out.print((char) ch);
            }
        }
    }
}
```

Here the encoding is explicitly specified as:

```text
UTF-8
```

---

## 🔥 Why Explicit Charset Matters

If the writer and reader use incompatible character encodings, text can become corrupted or appear incorrectly.

Example:

```text
Writer
 ↓
UTF-8

Reader
 ↓
Different encoding
```

The resulting characters may not match the original text.

---

# 16. FileWriter and Encoding

Similarly, when writing text where a specific charset is important, using `OutputStreamWriter` with an explicit charset provides control.

Example:

```java
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.OutputStreamWriter;
import java.io.Writer;
import java.nio.charset.StandardCharsets;

public class UTF8Writer {
    public static void main(String[] args) throws IOException {

        try (
            Writer writer =
                new OutputStreamWriter(
                    new FileOutputStream("data.txt"),
                    StandardCharsets.UTF_8
                )
        ) {

            writer.write("Hello Java");
            writer.write("\nनमस्ते");
        }
    }
}
```

This explicitly uses:

```text
UTF-8
```

for encoding the characters.

---

# 17. InputStreamReader

## 🎯 Definition

`InputStreamReader` is a bridge from byte streams to character streams.

It converts:

```text
Bytes
 ↓
Characters
```

Conceptually:

```text
InputStream
     ↓
InputStreamReader
     ↓
Reader
```

Example:

```java
import java.io.InputStreamReader;
import java.io.BufferedReader;
import java.io.IOException;
import java.nio.charset.StandardCharsets;

public class InputStreamReaderExample {
    public static void main(String[] args) throws IOException {

        try (
            BufferedReader reader =
                new BufferedReader(
                    new InputStreamReader(
                        System.in,
                        StandardCharsets.UTF_8
                    )
                )
        ) {

            String line = reader.readLine();

            System.out.println(line);
        }
    }
}
```

---

## 🧠 Why Is It Called a Bridge?

Because it connects:

```text
Byte World
     ↓
InputStream
     ↓
InputStreamReader
     ↓
Character World
     ↓
Reader
```

---

# 18. OutputStreamWriter

## 🎯 Definition

`OutputStreamWriter` is a bridge from character streams to byte streams.

It converts:

```text
Characters
     ↓
Bytes
```

Conceptually:

```text
Writer
  ↓
OutputStreamWriter
  ↓
OutputStream
```

Example:

```java
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.OutputStreamWriter;
import java.io.Writer;
import java.nio.charset.StandardCharsets;

public class OutputStreamWriterExample {
    public static void main(String[] args) throws IOException {

        try (
            Writer writer =
                new OutputStreamWriter(
                    new FileOutputStream("data.txt"),
                    StandardCharsets.UTF_8
                )
        ) {

            writer.write("Hello Java");
        }
    }
}
```

---

# 19. Reader vs InputStream

This is a very common interview comparison.

| `InputStream` | `Reader` |
|---|---|
| Byte-oriented | Character-oriented |
| Reads bytes | Reads characters/code units |
| Suitable for raw binary data | Designed for text |
| `read()` returns `int` | `read()` returns `int` |
| Example: `FileInputStream` | Example: `FileReader` |

Memory trick:

```text
InputStream
↓
Bytes

Reader
↓
Characters
```

---

# 20. Writer vs OutputStream

| `OutputStream` | `Writer` |
|---|---|
| Byte-oriented | Character-oriented |
| Writes bytes | Writes characters |
| Suitable for binary data | Designed for text |
| `write(int)` writes a byte | `write(int)` writes a character/code unit |
| Example: `FileOutputStream` | Example: `FileWriter` |

Memory trick:

```text
OutputStream
↓
Bytes

Writer
↓
Characters
```

---

# 21. Character Streams and Text Files

Character streams are particularly useful for:

```text
TXT
CSV
LOG
JAVA
XML
JSON
Properties
Configuration files
```

Example:

```java
import java.io.FileReader;
import java.io.IOException;

public class ReadLog {
    public static void main(String[] args) throws IOException {

        try (FileReader reader =
                 new FileReader("application.log")) {

            int ch;

            while ((ch = reader.read()) != -1) {
                System.out.print((char) ch);
            }
        }
    }
}
```

---

## ⚠️ Text Is Still Stored as Bytes

A text file on disk ultimately contains bytes.

Character streams provide an abstraction that converts between:

```text
Bytes
↕
Characters
```

using character encoding.

So:

```text
Character Stream ≠ Characters physically stored on disk
```

Rather:

```text
Character Stream
→ convenient character-oriented API
```

---

# 22. Closing Character Streams

Character streams should also be closed after use.

Example:

```java
import java.io.FileWriter;
import java.io.IOException;

public class CloseWriter {
    public static void main(String[] args) throws IOException {

        FileWriter writer =
            new FileWriter("data.txt");

        writer.write("Hello");

        writer.close();
    }
}
```

---

## 🔥 Why Close a Writer?

Closing a writer:

- Releases resources.
- Ensures pending buffered/encoded output is handled.
- Makes the resource unavailable for further use.

---

# 23. Try-With-Resources

The preferred approach is generally try-with-resources.

```java
import java.io.FileWriter;
import java.io.IOException;

public class TryWithResourcesWriter {
    public static void main(String[] args) {

        try (
            FileWriter writer =
                new FileWriter("data.txt")
        ) {

            writer.write("Hello Java");

        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

The writer is automatically closed.

---

# 24. Common Character Stream Classes

## `Reader`

Abstract base class for character input.

```text
Purpose:
Read characters
```

---

## `Writer`

Abstract base class for character output.

```text
Purpose:
Write characters
```

---

## `FileReader`

Reads characters from a file.

```text
File → Characters
```

---

## `FileWriter`

Writes characters to a file.

```text
Characters → File
```

---

## `BufferedReader`

Adds buffering and useful line-based reading.

```text
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

Detailed buffering is covered in:

```text
04-Buffered-Streams.md
```

---

## `BufferedWriter`

Adds buffering to a character writer.

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

---

# 25. Common Mistakes

## ❌ Mistake 1 — Thinking Character Streams Store Characters Directly

Files ultimately contain bytes.

Character streams provide a character-oriented abstraction over the byte representation.

---

## ❌ Mistake 2 — Ignoring Character Encoding

This can cause corrupted text when the encoding used for writing differs from the encoding used for reading.

Prefer explicit charsets when the encoding matters.

---

## ❌ Mistake 3 — Assuming `char` Equals One Byte

A Java `char` is a UTF-16 code unit and occupies 16 bits in memory.

But a character's encoded representation in a file can require a different number of bytes depending on the charset.

For example, UTF-8 uses a variable number of bytes for different Unicode characters.

---

## ❌ Mistake 4 — Forgetting EOF

Correct:

```java
int ch;

while ((ch = reader.read()) != -1) {
    System.out.print((char) ch);
}
```

---

## ❌ Mistake 5 — Forgetting to Close the Reader/Writer

Prefer:

```java
try (FileReader reader =
         new FileReader("data.txt")) {

}
```

---

## ❌ Mistake 6 — Using Character Streams for Arbitrary Binary Data

Don't use character conversion as a general-purpose way to copy binary files.

For:

```text
Images
PDFs
ZIP files
Videos
Audio
```

byte streams are generally appropriate.

---

## ❌ Mistake 7 — Confusing `Reader` With `BufferedReader`

`Reader` is the abstract base class.

`BufferedReader` is a concrete class that wraps a `Reader` and adds buffering and convenient methods such as:

```java
readLine()
```

---

# 26. Interview Questions

## 🔥 Q1. What are character streams?

Character streams are Java I/O streams designed for character-oriented text processing. They are based on the `Reader` and `Writer` class hierarchies.

---

## 🔥 Q2. What is `Reader`?

`Reader` is an abstract class representing character input.

---

## 🔥 Q3. What is `Writer`?

`Writer` is an abstract class representing character output.

---

## 🔥 Q4. What is `FileReader`?

`FileReader` is a character input stream used to read text from a file.

---

## 🔥 Q5. What is `FileWriter`?

`FileWriter` is a character output stream used to write text to a file.

---

## 🔥 Q6. Why are character streams useful?

They provide APIs designed around characters and text rather than requiring the programmer to manually handle raw byte-to-character conversion.

---

## 🔥 Q7. What does `Reader.read()` return?

It returns:

```text
Non-negative value → character/code unit
-1                  → EOF
```

The return type is `int`.

---

## 🔥 Q8. Why does `Reader.read()` return `int` instead of `char`?

Because it needs to represent `-1` as the end-of-stream indicator, and `char` cannot represent negative values.

---

## 🔥 Q9. What is character encoding?

Character encoding defines how characters are represented as bytes and how bytes are decoded back into characters.

Examples:

```text
UTF-8
UTF-16
US-ASCII
```

---

## 🔥 Q10. What is UTF-8?

UTF-8 is a Unicode encoding that represents Unicode text using a variable number of bytes per encoded code point.

---

## 🔥 Q11. Is Java `char` the same thing as a Unicode character?

Not always.

A Java `char` is a 16-bit UTF-16 code unit. Some Unicode code points require a pair of `char` values called a surrogate pair.

This distinction matters when discussing Unicode text.

---

## 🔥 Q12. What is `InputStreamReader`?

`InputStreamReader` is a bridge that converts bytes from an `InputStream` into characters for the `Reader` API.

---

## 🔥 Q13. What is `OutputStreamWriter`?

`OutputStreamWriter` is a bridge that converts characters written through the `Writer` API into bytes for an underlying `OutputStream`.

---

## 🔥 Q14. Why use `InputStreamReader` with a charset?

To explicitly control how incoming bytes are decoded into characters.

Example:

```java
new InputStreamReader(
    input,
    StandardCharsets.UTF_8
);
```

---

## 🔥 Q15. Why use `OutputStreamWriter` with a charset?

To explicitly control how characters are encoded into bytes.

Example:

```java
new OutputStreamWriter(
    output,
    StandardCharsets.UTF_8
);
```

---

## 🔥 Q16. Difference between `InputStream` and `Reader`?

```text
InputStream
→ byte-oriented

Reader
→ character-oriented
```

---

## 🔥 Q17. Difference between `OutputStream` and `Writer`?

```text
OutputStream
→ byte-oriented

Writer
→ character-oriented
```

---

## 🔥 Q18. Can `FileReader` read binary files?

It can read the bytes through a character-decoding mechanism, but it is intended for character/text data.

For arbitrary binary files, use byte streams.

---

## 🔥 Q19. What is the difference between `FileReader` and `BufferedReader`?

```text
FileReader
→ Reads characters from a file.

BufferedReader
→ Wraps a Reader, adds buffering, and provides methods such as readLine().
```

---

## 🔥 Q20. Can a `Writer` write a String directly?

Yes.

```java
writer.write("Hello Java");
```

---

## 🔥 Q21. Can a `Reader` read an entire String using `read()`?

No.

The basic `read()` method reads one character/code unit at a time.

For convenient line-based text reading, `BufferedReader.readLine()` is commonly used.

---

## 🔥 Q22. Why should we specify a charset explicitly?

Because text can be encoded in different ways. Explicitly specifying the expected charset prevents platform/environment differences from silently changing how bytes are interpreted.

---

## 🔥 Q23. Are character streams faster than byte streams?

There is no universal rule that one is simply "faster."

They solve different problems:

```text
Byte streams
→ raw byte data

Character streams
→ character/text data
```

Performance depends on the API, buffering, encoding, data, and workload.

---

## 🔥 Q24. Can character streams be buffered?

Yes.

Common classes:

```text
BufferedReader
BufferedWriter
```

---

## 🔥 Q25. Why is `BufferedReader` commonly used with `FileReader`?

Because it adds buffering and provides convenient methods such as:

```java
readLine()
```

---

## 🔥 Q26. What happens if the writer uses UTF-8 but the reader uses another incompatible encoding?

The decoded text may be incorrect or corrupted because the reader interprets the bytes using the wrong character mapping.

---

## 🔥 Q27. Is `Reader` an interface?

No.

`Reader` is an **abstract class**.

Similarly:

```text
Writer
InputStream
OutputStream
```

are abstract classes.

---

## 🔥 Q28. What is the relationship between byte streams and character streams?

Character streams ultimately operate over byte-oriented data through character encoding and decoding.

Conceptually:

```text
Bytes
 ↓
Decoder
 ↓
Characters
```

and:

```text
Characters
 ↓
Encoder
 ↓
Bytes
```

---

# 27. 30-Second Interview Answer

> Character streams are Java I/O APIs designed for text and character-oriented data. They are based on the `Reader` and `Writer` abstract classes. `FileReader` reads characters from a file and `FileWriter` writes characters to a file. Character I/O involves converting between bytes and characters using a character encoding such as UTF-8. When explicit charset control is required, `InputStreamReader` and `OutputStreamWriter` are commonly used. For efficient line-based text processing, `BufferedReader` is frequently used.

---

# 28. Cheat Sheet

```text
================ CHARACTER STREAMS =================

CHARACTER STREAMS
        |
        +----------------------+
        |                      |
      INPUT                  OUTPUT
        |                      |
      Reader                 Writer
        |                      |
   FileReader              FileWriter
        |                      |
 BufferedReader          BufferedWriter


CHARACTER INPUT
---------------------------------

File
 ↓
FileReader
 ↓
Reader
 ↓
Java Program


CHARACTER OUTPUT
---------------------------------

Java Program
 ↓
Writer
 ↓
FileWriter
 ↓
File


BYTE → CHARACTER
---------------------------------

InputStream
     ↓
InputStreamReader
     ↓
Reader


CHARACTER → BYTE
---------------------------------

Writer
     ↓
OutputStreamWriter
     ↓
OutputStream


ENCODING
---------------------------------

Characters
    ↓
Encoding
    ↓
Bytes

Bytes
    ↓
Decoding
    ↓
Characters


IMPORTANT CLASSES
---------------------------------

Reader
Writer

FileReader
FileWriter

BufferedReader
BufferedWriter

InputStreamReader
OutputStreamWriter


USE CHARACTER STREAMS FOR
---------------------------------

TXT
CSV
LOG
JAVA
XML
JSON
Configuration
Other text


KEY RULE
---------------------------------

InputStream
→ Bytes

Reader
→ Characters

OutputStream
→ Bytes

Writer
→ Characters
```

---

# 🧠 Final Mental Model

```text
                         CHARACTER I/O
                              |
                 +------------+------------+
                 |                         |
               INPUT                     OUTPUT
                 |                         |
              Reader                     Writer
                 |                         |
            FileReader                FileWriter
                 |                         |
                 ↓                         ↓
               FILE                      FILE


                 CHARACTER ENCODING

              Reading
                 |
               Bytes
                 ↓
              Decoder
                 ↓
            Characters


              Writing
                 |
            Characters
                 ↓
              Encoder
                 ↓
               Bytes


              BRIDGE CLASSES

Bytes
 ↓
InputStream
 ↓
InputStreamReader
 ↓
Reader
 ↓
Characters


Characters
 ↓
Writer
 ↓
OutputStreamWriter
 ↓
OutputStream
 ↓
Bytes
```

---

# 🏁 Key Takeaways

- Character streams are designed for text and character-oriented I/O.
- `Reader` is the base abstract class for character input.
- `Writer` is the base abstract class for character output.
- `FileReader` reads text from files.
- `FileWriter` writes text to files.
- `Reader.read()` returns an `int`, with `-1` representing EOF.
- Character streams work with character encoding and decoding.
- UTF-8 is a Unicode encoding with variable-length byte representation.
- Java `char` is a 16-bit UTF-16 code unit, not necessarily a complete Unicode code point.
- `InputStreamReader` converts bytes into characters.
- `OutputStreamWriter` converts characters into bytes.
- `BufferedReader` provides buffering and convenient line-based reading.
- `BufferedWriter` provides buffered character output.
- Character streams are appropriate for text; byte streams are generally appropriate for arbitrary binary data.
- When encoding matters, explicitly specifying a charset such as UTF-8 is safer than relying on an implicit environment choice.
- Try-with-resources should generally be used to manage I/O resources.

---
````
