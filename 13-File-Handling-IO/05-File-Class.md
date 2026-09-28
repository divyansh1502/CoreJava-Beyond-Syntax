````md
# 📁 Java File Class

> **The `File` class represents a pathname for a file or directory and provides methods to inspect and manipulate filesystem entries such as creating, deleting, renaming, and checking files and directories.**

---

# 📑 Table of Contents

- [1. What Is the File Class?](#1-what-is-the-file-class)
- [2. Why Do We Need the File Class?](#2-why-do-we-need-the-file-class)
- [3. File vs Actual File](#3-file-vs-actual-file)
- [4. Creating a File Object](#4-creating-a-file-object)
- [5. File Path](#5-file-path)
- [6. Absolute Path vs Relative Path](#6-absolute-path-vs-relative-path)
- [7. Important File Methods](#7-important-file-methods)
- [8. Checking File Existence](#8-checking-file-existence)
- [9. Checking File or Directory](#9-checking-file-or-directory)
- [10. Getting File Information](#10-getting-file-information)
- [11. Creating a New File](#11-creating-a-new-file)
- [12. Creating Directories](#12-creating-directories)
- [13. `mkdir()` vs `mkdirs()`](#13-mkdir-vs-mkdirs)
- [14. Deleting Files and Directories](#14-deleting-files-and-directories)
- [15. Renaming and Moving](#15-renaming-and-moving)
- [16. Listing Directory Contents](#16-listing-directory-contents)
- [17. `list()`](#17-list)
- [18. `listFiles()`](#18-listfiles)
- [19. File Permissions](#19-file-permissions)
- [20. Hidden Files](#20-hidden-files)
- [21. File Size](#21-file-size)
- [22. Last Modified Time](#22-last-modified-time)
- [23. Absolute and Canonical Paths](#23-absolute-and-canonical-paths)
- [24. Path Separators](#24-path-separators)
- [25. File Class with Streams](#25-file-class-with-streams)
- [26. File Class Limitations](#26-file-class-limitations)
- [27. `File` vs `Path`](#27-file-vs-path)
- [28. Common Mistakes](#28-common-mistakes)
- [29. Interview Questions](#29-interview-questions)
- [30. 30-Second Interview Answer](#30-30-second-interview-answer)
- [31. Cheat Sheet](#31-cheat-sheet)

---

# 1. What Is the File Class?

`File` is a class from:

```text
java.io
```

Fully qualified name:

```text
java.io.File
```

It represents a **pathname** to a filesystem entry.

That entry may be:

```text
File
Directory
```

Example:

```java
import java.io.File;

public class FileExample {
    public static void main(String[] args) {

        File file = new File("data.txt");

        System.out.println(file);
    }
}
```

---

# 2. Why Do We Need the File Class?

The `File` class provides methods for interacting with filesystem entries.

It can help us:

```text
Check existence
Create files
Create directories
Delete files
Delete directories
Rename files
Get file size
Get file name
Get path
Check permissions
List directory contents
```

For example:

```java
import java.io.File;

public class FileInfo {
    public static void main(String[] args) {

        File file = new File("data.txt");

        System.out.println(file.exists());
        System.out.println(file.getName());
        System.out.println(file.length());
    }
}
```

---

# 3. File vs Actual File

This is one of the most important concepts.

Creating a `File` object does **not** create an actual file on disk.

Example:

```java
File file = new File("data.txt");
```

This only creates a Java object representing the pathname.

It does **not** automatically create:

```text
data.txt
```

on the filesystem.

---

## 🔥 Actual File Creation

To create the actual file:

```java
file.createNewFile();
```

Example:

```java
import java.io.File;
import java.io.IOException;

public class CreateFile {
    public static void main(String[] args) throws IOException {

        File file = new File("data.txt");

        boolean created = file.createNewFile();

        System.out.println(created);
    }
}
```

---

## 🧠 Remember

```text
new File(...)
        ↓
Creates Java File object

createNewFile()
        ↓
Creates actual filesystem file
```

---

# 4. Creating a File Object

The simplest constructor is:

```java
File file = new File("data.txt");
```

---

## Directory Path

```java
File directory = new File("documents");
```

---

## Parent + Child

```java
File file =
    new File("documents", "data.txt");
```

Conceptually:

```text
documents/
    data.txt
```

---

## Parent File + Child

```java
File parent =
    new File("documents");

File file =
    new File(parent, "data.txt");
```

This is useful when constructing paths programmatically.

---

# 5. File Path

A path tells Java where a filesystem entry is located.

Example:

```text
data.txt
```

or:

```text
documents/data.txt
```

or an absolute path such as:

```text
C:\Users\User\Documents\data.txt
```

---

## Relative Path

```java
File file = new File("data.txt");
```

This path is interpreted relative to the Java process's current working directory.

---

## Absolute Path

```java
File file =
    new File("C:\\Users\\User\\Documents\\data.txt");
```

The path identifies a location independently of the current working directory.

---

# 6. Absolute Path vs Relative Path

## Relative Path

A relative path does not start from the filesystem root.

Example:

```java
File file =
    new File("data.txt");
```

It depends on the current working directory.

---

## Absolute Path

An absolute path identifies the complete location.

Example on Windows:

```java
File file =
    new File("C:\\Users\\User\\Documents\\data.txt");
```

Example on Linux:

```java
File file =
    new File("/home/user/data.txt");
```

---

## 🧠 Interview Point

Do not assume:

```text
relative path = project folder
```

The actual base is the Java process's **current working directory**.

You can inspect it using:

```java
System.out.println(
    System.getProperty("user.dir")
);
```

---

# 7. Important File Methods

Some commonly used methods are:

| Method | Purpose |
|---|---|
| `exists()` | Checks whether path exists |
| `isFile()` | Checks whether it is a regular file |
| `isDirectory()` | Checks whether it is a directory |
| `createNewFile()` | Creates a new empty file |
| `mkdir()` | Creates one directory |
| `mkdirs()` | Creates required parent directories |
| `delete()` | Deletes a filesystem entry |
| `getName()` | Returns name |
| `getPath()` | Returns pathname |
| `getAbsolutePath()` | Returns absolute pathname |
| `getCanonicalPath()` | Returns canonical pathname |
| `length()` | Returns file size in bytes |
| `lastModified()` | Returns modification timestamp |
| `list()` | Lists directory names |
| `listFiles()` | Lists entries as `File` objects |
| `canRead()` | Checks read permission |
| `canWrite()` | Checks write permission |
| `canExecute()` | Checks execute permission |
| `isHidden()` | Checks whether entry is hidden |

---

# 8. Checking File Existence

Use:

```java
exists()
```

Example:

```java
import java.io.File;

public class CheckExists {
    public static void main(String[] args) {

        File file = new File("data.txt");

        if (file.exists()) {
            System.out.println("File exists");
        } else {
            System.out.println("File does not exist");
        }
    }
}
```

---

## 🧠 Important

`exists()` checks the filesystem.

It does not check whether the Java object exists.

The object itself obviously exists if you successfully created it.

---

# 9. Checking File or Directory

Use:

```java
isFile()
```

and:

```java
isDirectory()
```

Example:

```java
import java.io.File;

public class FileOrDirectory {
    public static void main(String[] args) {

        File entry = new File("data.txt");

        if (entry.isFile()) {
            System.out.println("It is a file");
        }

        if (entry.isDirectory()) {
            System.out.println("It is a directory");
        }
    }
}
```

---

## 🧠 Important

A path can represent different filesystem entry types.

Therefore:

```text
exists()
```

does not tell you whether it is a file or directory.

Use:

```text
isFile()
isDirectory()
```

for that distinction.

---

# 10. Getting File Information

The `File` class provides several information methods.

---

## File Name

```java
import java.io.File;

public class FileName {
    public static void main(String[] args) {

        File file =
            new File("documents/data.txt");

        System.out.println(file.getName());
    }
}
```

Output:

```text
data.txt
```

---

## Parent

```java
System.out.println(file.getParent());
```

Possible output:

```text
documents
```

---

## Path

```java
System.out.println(file.getPath());
```

Possible output:

```text
documents/data.txt
```

---

# 11. Creating a New File

Use:

```java
createNewFile()
```

Example:

```java
import java.io.File;
import java.io.IOException;

public class CreateNewFile {
    public static void main(String[] args) {

        File file = new File("data.txt");

        try {

            if (file.createNewFile()) {
                System.out.println("File created");
            } else {
                System.out.println("File already exists");
            }

        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

---

## Return Value

`createNewFile()` returns:

```text
true
→ file was created

false
→ file already existed
```

---

## ⚠️ Important

The parent directory must already exist.

For example:

```text
documents/
```

must exist before:

```text
documents/data.txt
```

can be created using `createNewFile()`.

---

# 12. Creating Directories

Use:

```java
mkdir()
```

Example:

```java
import java.io.File;

public class CreateDirectory {
    public static void main(String[] args) {

        File directory =
            new File("documents");

        if (directory.mkdir()) {
            System.out.println("Directory created");
        } else {
            System.out.println("Directory not created");
        }
    }
}
```

---

# 13. `mkdir()` vs `mkdirs()`

This is an important interview question.

## `mkdir()`

Creates one directory.

```java
File directory =
    new File("parent/child");

directory.mkdir();
```

This will work only if:

```text
parent/
```

already exists.

---

## `mkdirs()`

Creates the directory and any required parent directories.

```java
File directory =
    new File("parent/child/grandchild");

directory.mkdirs();
```

It can create:

```text
parent/
    child/
        grandchild/
```

---

## Comparison

| `mkdir()` | `mkdirs()` |
|---|---|
| Creates one directory | Creates required parent directories too |
| Parent must generally exist | Parent directories can be created |
| Returns boolean | Returns boolean |

---

# 14. Deleting Files and Directories

Use:

```java
delete()
```

Example:

```java
import java.io.File;

public class DeleteFile {
    public static void main(String[] args) {

        File file = new File("data.txt");

        if (file.delete()) {
            System.out.println("Deleted");
        } else {
            System.out.println("Could not delete");
        }
    }
}
```

---

## 🧠 Important Directory Rule

A non-empty directory generally cannot be deleted directly using `File.delete()`.

Suppose:

```text
documents/
    a.txt
    b.txt
```

You must first remove the contents before deleting:

```text
documents/
```

---

# 15. Renaming and Moving

`File` does not have a separate `move()` method.

Traditionally, `renameTo()` can be used to rename or move a filesystem entry.

Example:

```java
import java.io.File;

public class RenameFile {
    public static void main(String[] args) {

        File oldFile =
            new File("old.txt");

        File newFile =
            new File("new.txt");

        if (oldFile.renameTo(newFile)) {
            System.out.println("Renamed");
        } else {
            System.out.println("Rename failed");
        }
    }
}
```

---

## ⚠️ Modern Java Recommendation

For robust file-copy, move, and replacement operations, prefer the NIO.2 API:

```text
java.nio.file.Path
java.nio.file.Files
```

Example:

```java
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;

public class MoveFile {
    public static void main(String[] args) throws IOException {

        Path source = Path.of("old.txt");
        Path target = Path.of("new.txt");

        Files.move(source, target);
    }
}
```

`Files.move()` provides more explicit control and better-defined behavior.

---

# 16. Listing Directory Contents

Suppose we have:

```text
project/
    A.txt
    B.txt
    C.txt
```

We can represent the directory:

```java
File directory =
    new File("project");
```

Then list its contents.

---

# 17. `list()`

`list()` returns the names of entries inside a directory.

Example:

```java
import java.io.File;

public class ListNames {
    public static void main(String[] args) {

        File directory =
            new File("project");

        String[] names =
            directory.list();

        if (names != null) {

            for (String name : names) {
                System.out.println(name);
            }
        }
    }
}
```

Possible output:

```text
A.txt
B.txt
C.txt
```

---

## ⚠️ Important

`list()` returns:

```text
String[]
```

not:

```text
File[]
```

---

# 18. `listFiles()`

`listFiles()` returns the entries as `File` objects.

Example:

```java
import java.io.File;

public class ListFiles {
    public static void main(String[] args) {

        File directory =
            new File("project");

        File[] files =
            directory.listFiles();

        if (files != null) {

            for (File file : files) {
                System.out.println(
                    file.getName()
                );
            }
        }
    }
}
```

---

## `list()` vs `listFiles()`

| `list()` | `listFiles()` |
|---|---|
| Returns `String[]` | Returns `File[]` |
| Gives names | Gives `File` objects |
| Less information | Can inspect each entry |
| Simple listing | Better when metadata is needed |

---

# 19. File Permissions

The `File` class provides permission-checking methods.

---

## Read Permission

```java
file.canRead();
```

---

## Write Permission

```java
file.canWrite();
```

---

## Execute Permission

```java
file.canExecute();
```

Example:

```java
import java.io.File;

public class Permissions {
    public static void main(String[] args) {

        File file =
            new File("data.txt");

        System.out.println(
            "Readable: " + file.canRead()
        );

        System.out.println(
            "Writable: " + file.canWrite()
        );

        System.out.println(
            "Executable: " + file.canExecute()
        );
    }
}
```

---

## 🧠 Important

These methods report permissions as visible to the Java process and operating system.

They should not be treated as a universal security authorization mechanism.

---

# 20. Hidden Files

Use:

```java
isHidden()
```

Example:

```java
import java.io.File;

public class HiddenFile {
    public static void main(String[] args) {

        File file =
            new File(".config");

        System.out.println(
            file.isHidden()
        );
    }
}
```

Whether a file is considered hidden depends on the underlying operating system/filesystem conventions.

---

# 21. File Size

Use:

```java
length()
```

Example:

```java
import java.io.File;

public class FileSize {
    public static void main(String[] args) {

        File file =
            new File("data.txt");

        System.out.println(
            "Size: " + file.length() + " bytes"
        );
    }
}
```

---

## ⚠️ Important

For a regular file:

```text
length()
→ file size in bytes
```

For directories, the returned value is not a portable way to determine the total size of everything inside the directory.

---

# 22. Last Modified Time

Use:

```java
lastModified()
```

Example:

```java
import java.io.File;
import java.util.Date;

public class LastModified {
    public static void main(String[] args) {

        File file =
            new File("data.txt");

        long time =
            file.lastModified();

        System.out.println(
            new Date(time)
        );
    }
}
```

The method returns the modification time as a millisecond-based timestamp.

---

# 23. Absolute and Canonical Paths

These are frequently asked in interviews.

---

## `getPath()`

Returns the pathname used to create the `File` object.

```java
File file =
    new File("data.txt");

System.out.println(
    file.getPath()
);
```

Possible output:

```text
data.txt
```

---

## `getAbsolutePath()`

Returns an absolute pathname.

```java
System.out.println(
    file.getAbsolutePath()
);
```

Possible output:

```text
C:\project\data.txt
```

---

## `getCanonicalPath()`

Returns the canonical pathname.

It can resolve:

```text
.
..
```

and resolve symbolic links where supported.

Example:

```java
import java.io.File;
import java.io.IOException;

public class Paths {
    public static void main(String[] args)
        throws IOException {

        File file =
            new File("project/../data.txt");

        System.out.println(
            file.getPath()
        );

        System.out.println(
            file.getAbsolutePath()
        );

        System.out.println(
            file.getCanonicalPath()
        );
    }
}
```

---

## Comparison

```text
getPath()
→ Original pathname representation

getAbsolutePath()
→ Absolute pathname

getCanonicalPath()
→ Canonicalized pathname
```

---

# 24. Path Separators

Different operating systems traditionally use different separators.

Windows commonly uses:

```text
\
```

Unix-like systems commonly use:

```text
/
```

Java provides:

```java
File.separator
```

Example:

```java
import java.io.File;

public class Separator {
    public static void main(String[] args) {

        System.out.println(
            File.separator
        );
    }
}
```

---

## 🧠 Better Modern Approach

Modern Java applications commonly use:

```text
Path
Files
```

from:

```text
java.nio.file
```

because they provide a more powerful and flexible filesystem API.

---

# 25. File Class with Streams

The `File` class and streams solve different problems.

`File` represents a filesystem path and provides filesystem-related operations.

Streams perform data I/O.

Example:

```java
import java.io.File;
import java.io.FileReader;
import java.io.IOException;

public class FileWithReader {
    public static void main(String[] args)
        throws IOException {

        File file =
            new File("data.txt");

        try (FileReader reader =
                 new FileReader(file)) {

            int ch;

            while ((ch = reader.read()) != -1) {
                System.out.print((char) ch);
            }
        }
    }
}
```

---

## 🧠 Think Like This

```text
File
↓
Where is the data?

Stream
↓
How do I read/write the data?
```

---

# 26. File Class Limitations

Although `File` is useful, it is an older API.

For advanced filesystem operations, modern Java provides:

```text
java.nio.file.Path
java.nio.file.Files
```

The NIO.2 API offers better support for:

```text
File operations
Path manipulation
Copying
Moving
Symbolic links
File attributes
Directory traversal
Exceptions
Filesystem providers
```

---

# 27. `File` vs `Path`

This is an important modern Java interview topic.

| `File` | `Path` |
|---|---|
| Older `java.io` API | Modern `java.nio.file` API |
| Represents pathname | Represents filesystem path |
| Limited filesystem operations | Richer API |
| Uses methods like `delete()` | Uses `Files.delete()` |
| Uses `renameTo()` | Uses `Files.move()` |
| `listFiles()` | `Files.list()` / directory APIs |
| Still widely encountered | Preferred for new code |

---

## Example

Old style:

```java
File file =
    new File("data.txt");

boolean exists =
    file.exists();
```

Modern style:

```java
import java.nio.file.Files;
import java.nio.file.Path;

Path path =
    Path.of("data.txt");

boolean exists =
    Files.exists(path);
```

---

## Modern File Creation

```java
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;

public class ModernFileCreation {
    public static void main(String[] args)
        throws IOException {

        Path path =
            Path.of("data.txt");

        if (Files.notExists(path)) {
            Files.createFile(path);
        }
    }
}
```

---

# 28. Common Mistakes

## ❌ Mistake 1 — Thinking `new File()` Creates a File

Wrong assumption:

```java
File file =
    new File("data.txt");
```

This does not create the physical file.

Correct:

```java
file.createNewFile();
```

---

## ❌ Mistake 2 — Using `mkdir()` for Nested Directories

This may fail:

```java
new File("a/b/c").mkdir();
```

if:

```text
a/
b/
```

do not already exist.

Use:

```java
new File("a/b/c").mkdirs();
```

---

## ❌ Mistake 3 — Assuming `delete()` Deletes Non-Empty Directories

A directory containing files generally must be emptied before it can be deleted.

---

## ❌ Mistake 4 — Confusing `list()` and `listFiles()`

```text
list()
→ String[]

listFiles()
→ File[]
```

---

## ❌ Mistake 5 — Assuming `length()` Gives Directory Size

It is not a portable method for calculating the total size of directory contents.

---

## ❌ Mistake 6 — Assuming Relative Paths Are Relative to the `.java` File

They are normally resolved relative to the Java process's current working directory.

---

## ❌ Mistake 7 — Using `renameTo()` for Robust File Moves

`renameTo()` has platform-dependent behavior and limited error reporting.

For modern applications, prefer:

```java
Files.move(...)
```

---

## ❌ Mistake 8 — Using `File` for Every Modern Filesystem Operation

For new code, consider:

```text
Path
Files
```

from NIO.2.

---

# 29. Interview Questions

## 🔥 Q1. What is the `File` class?

`File` is a class in `java.io` that represents a pathname for a file or directory and provides methods for filesystem-related operations.

---

## 🔥 Q2. Does `new File("data.txt")` create the file?

No.

It creates a Java `File` object representing the pathname.

To create the physical file:

```java
file.createNewFile();
```

---

## 🔥 Q3. What is the difference between `File` and a physical file?

```text
File object
→ Java object representing a pathname

Physical file
→ Actual filesystem entry
```

---

## 🔥 Q4. What does `exists()` do?

It checks whether the filesystem entry represented by the pathname exists.

---

## 🔥 Q5. Difference between `isFile()` and `isDirectory()`?

```text
isFile()
→ checks whether the path represents a regular file

isDirectory()
→ checks whether the path represents a directory
```

---

## 🔥 Q6. Difference between `mkdir()` and `mkdirs()`?

```text
mkdir()
→ creates one directory

mkdirs()
→ creates the directory and required parent directories
```

---

## 🔥 Q7. What does `createNewFile()` return?

```text
true
→ file was successfully created

false
→ file already existed
```

It can throw `IOException` when creation fails due to an I/O problem.

---

## 🔥 Q8. What does `delete()` return?

```text
true
→ deletion succeeded

false
→ deletion failed
```

It can delete files and empty directories.

---

## 🔥 Q9. What is `list()`?

It returns the names of entries inside a directory as a `String[]`.

---

## 🔥 Q10. What is `listFiles()`?

It returns the entries inside a directory as an array of `File` objects.

---

## 🔥 Q11. Difference between `list()` and `listFiles()`?

```text
list()
→ String[]

listFiles()
→ File[]
```

`listFiles()` is useful when you need to inspect each entry further.

---

## 🔥 Q12. What does `length()` return?

For a regular file, it returns the file's size in bytes.

---

## 🔥 Q13. What does `lastModified()` return?

It returns the last-modified timestamp as a `long` representing milliseconds from the Unix epoch.

---

## 🔥 Q14. Difference between `getPath()` and `getAbsolutePath()`?

```text
getPath()
→ pathname used by the File object

getAbsolutePath()
→ absolute pathname
```

---

## 🔥 Q15. Difference between `getAbsolutePath()` and `getCanonicalPath()`?

```text
getAbsolutePath()
→ converts the path to an absolute path

getCanonicalPath()
→ produces a canonicalized path,
  resolving . and .. and symbolic links
  where supported
```

---

## 🔥 Q16. What is a relative path?

A path interpreted relative to the current working directory.

Example:

```java
new File("data.txt");
```

---

## 🔥 Q17. What is an absolute path?

A path that identifies a location from the filesystem root or equivalent absolute namespace.

---

## 🔥 Q18. Can `File` read file contents?

No, `File` itself is primarily a filesystem/path abstraction.

Use streams or other I/O APIs to read/write contents.

---

## 🔥 Q19. Difference between `File` and `FileReader`?

```text
File
→ represents a filesystem path

FileReader
→ reads character data from a file
```

---

## 🔥 Q20. Difference between `File` and `FileInputStream`?

```text
File
→ represents filesystem location

FileInputStream
→ reads bytes from a file
```

---

## 🔥 Q21. Can a `File` object represent a directory?

Yes.

```java
File directory =
    new File("documents");
```

The same class represents both files and directories.

---

## 🔥 Q22. Can `File.delete()` delete a non-empty directory?

Generally no.

The directory must first be emptied.

---

## 🔥 Q23. What is `File.separator`?

It provides the platform-specific default name-separator character.

---

## 🔥 Q24. Is `File` the preferred API for all modern file operations?

No.

For new code, `java.nio.file.Path` and `java.nio.file.Files` generally provide a richer filesystem API.

---

## 🔥 Q25. What is `Path`?

`Path` is the modern NIO.2 representation of a filesystem path.

---

## 🔥 Q26. What is `Files`?

`Files` is a utility class in `java.nio.file` containing static methods for filesystem operations involving `Path`.

---

## 🔥 Q27. How do you move a file using modern Java?

Use:

```java
Files.move(source, target);
```

---

## 🔥 Q28. How do you check whether a `Path` exists?

Use:

```java
Files.exists(path);
```

---

## 🔥 Q29. Why is `Path` preferred over `File` in many new applications?

Because the NIO.2 API provides a richer, more consistent, and more flexible filesystem API with better support for modern filesystem operations.

---

## 🔥 Q30. Is `File` completely obsolete?

No.

It remains part of Java and is still encountered in existing code and APIs. However, `Path` and `Files` are generally preferred for new filesystem work.

---

# 30. 30-Second Interview Answer

> The `File` class belongs to `java.io` and represents a pathname for a file or directory. It can check existence, distinguish files from directories, create files and directories, delete entries, list directory contents, inspect size and timestamps, and retrieve path information. Importantly, creating a `File` object does not create a physical file. For modern Java development, `Path` and `Files` from `java.nio.file` are generally preferred for advanced filesystem operations.

---

# 31. Cheat Sheet

```text
================= FILE CLASS =================

PACKAGE
---------------------------------

java.io.File


IMPORTANT CONCEPT
---------------------------------

new File("data.txt")
        ↓
Java object representing pathname

NOT:
        ↓
Physical file creation


CREATE FILE
---------------------------------

file.createNewFile();


CREATE DIRECTORY
---------------------------------

file.mkdir();


CREATE NESTED DIRECTORIES
---------------------------------

file.mkdirs();


CHECK EXISTENCE
---------------------------------

file.exists();


CHECK TYPE
---------------------------------

file.isFile();
file.isDirectory();


DELETE
---------------------------------

file.delete();


GET NAME
---------------------------------

file.getName();


GET PARENT
---------------------------------

file.getParent();


GET PATH
---------------------------------

file.getPath();


GET ABSOLUTE PATH
---------------------------------

file.getAbsolutePath();


GET CANONICAL PATH
---------------------------------

file.getCanonicalPath();


FILE SIZE
---------------------------------

file.length();


LAST MODIFIED
---------------------------------

file.lastModified();


PERMISSIONS
---------------------------------

file.canRead();
file.canWrite();
file.canExecute();


HIDDEN
---------------------------------

file.isHidden();


LIST NAMES
---------------------------------

file.list();


LIST FILE OBJECTS
---------------------------------

file.listFiles();


RENAME
---------------------------------

file.renameTo(other);


MODERN MOVE
---------------------------------

Files.move(source, target);


MODERN API
---------------------------------

Path
Files


FILE vs STREAM
---------------------------------

File
→ Where is it?

Stream
→ How do I read/write it?


OLD API
---------------------------------

java.io.File


MODERN API
---------------------------------

java.nio.file.Path
java.nio.file.Files
```

---

# 🧠 Final Mental Model

```text
                       FILESYSTEM
                           |
             +-------------+-------------+
             |                           |
           FILE                       DIRECTORY
             |                           |
             +-------------+-------------+
                           |
                         File
                        Object
                           |
       +-------------------+-------------------+
       |                   |                   |
    Check                Create              Inspect
       |                   |                   |
   exists()          createNewFile()       length()
   isFile()           mkdir()              getName()
   isDirectory()      mkdirs()             getPath()
                                            getParent()
                                            lastModified()
                                            canRead()
                                            canWrite()
       |
     Manage
       |
   delete()
   renameTo()
   list()
   listFiles()


IMPORTANT:

File object
    ↓
represents pathname

FileReader
    ↓
reads characters

FileInputStream
    ↓
reads bytes

Path + Files
    ↓
modern filesystem API
```

---

# 🏁 Key Takeaways

- `File` belongs to `java.io`.
- It represents a pathname for a file or directory.
- `new File(...)` does not create a physical file.
- `createNewFile()` creates an actual file.
- `mkdir()` creates one directory.
- `mkdirs()` can create required parent directories.
- `exists()` checks whether the filesystem entry exists.
- `isFile()` and `isDirectory()` identify the entry type.
- `delete()` deletes a file or empty directory.
- `list()` returns directory entry names.
- `listFiles()` returns `File` objects.
- `length()` returns the size of a regular file in bytes.
- `getPath()` returns the pathname representation.
- `getAbsolutePath()` returns an absolute path.
- `getCanonicalPath()` returns a canonicalized path.
- `File` does not read or write file contents itself.
- Streams such as `FileReader` and `FileInputStream` handle data I/O.
- `renameTo()` is an older way to rename/move entries.
- `Files.move()` is generally preferred for modern file movement.
- `Path` and `Files` provide the modern NIO.2 filesystem API.
- The most important distinction is:

```text
File
→ represents WHERE

Stream
→ handles HOW data is read/written

Path + Files
→ modern filesystem API
```

---
````
