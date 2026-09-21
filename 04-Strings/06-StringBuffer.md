# 🧵 StringBuffer in Java

> **`StringBuffer` is a mutable sequence of characters. It is similar to `StringBuilder`, but its methods are synchronized, making it suitable for use when multiple threads may access and modify the same object.**

---

# 📌 Table of Contents

1. [What is StringBuffer?](#1--what-is-stringbuffer)
2. [Why Do We Need StringBuffer?](#2--why-do-we-need-stringbuffer)
3. [StringBuffer as a Class](#3--stringbuffer-as-a-class)
4. [StringBuffer Package](#4--stringbuffer-package)
5. [Creating StringBuffer](#5--creating-stringbuffer)
6. [Default Constructor](#6--default-constructor)
7. [Constructor with String](#7--constructor-with-string)
8. [Constructor with Capacity](#8--constructor-with-capacity)
9. [Mutability](#9--mutability)
10. [Internal Working](#10--internal-working)
11. [Capacity](#11--capacity)
12. [length()](#12--length)
13. [append()](#13--append)
14. [insert()](#14--insert)
15. [replace()](#15--replace)
16. [delete()](#16--delete)
17. [deleteCharAt()](#17--deletecharat)
18. [reverse()](#18--reverse)
19. [charAt()](#19--charat)
20. [setCharAt()](#20--setcharat)
21. [substring()](#21--substring)
22. [indexOf()](#22--indexof)
23. [lastIndexOf()](#23--lastindexof)
24. [setLength()](#24--setlength)
25. [ensureCapacity()](#25--ensurecapacity)
26. [trimToSize()](#26--trimtosize)
27. [toString()](#27--tostring)
28. [Method Chaining](#28--method-chaining)
29. [StringBuffer and Synchronization](#29--stringbuffer-and-synchronization)
30. [Thread Safety](#30--thread-safety)
31. [Capacity Growth](#31--capacity-growth)
32. [StringBuffer vs String](#32--stringbuffer-vs-string)
33. [StringBuffer vs StringBuilder](#33--stringbuffer-vs-stringbuilder)
34. [Performance](#34--performance)
35. [Advantages](#35--advantages)
36. [Disadvantages](#36--disadvantages)
37. [Common Mistakes](#37--common-mistakes)
38. [Interview Traps](#38--interview-traps)
39. [Top 20 Interview Questions](#39--top-20-interview-questions)
40. [30-Second Interview Answer](#40--30-second-interview-answer)
41. [Cheat Sheet](#41--cheat-sheet)
42. [Memory Tricks](#42--memory-tricks)
43. [Next Topic](#43--next-topic)

---

# 1. 🧵 What is StringBuffer?

`StringBuffer` is a:

> **Mutable sequence of characters.**

The most important properties are:

- Mutable
- Synchronized
- Thread-safe for individual method operations
- Resizable
- Part of `java.lang`

Example:

    StringBuffer sb = new StringBuffer("Java");

    sb.append(" Programming");

    System.out.println(sb);

Output:

    Java Programming

The existing StringBuffer object is modified.

---

# 2. 🤔 Why Do We Need StringBuffer?

We already have:

    String
    StringBuilder

So why StringBuffer?

The key reason is:

> **Thread-safe mutable string manipulation.**

`String` is immutable.

`StringBuilder` is mutable but not synchronized.

`StringBuffer` is mutable and synchronized.

### Simple Comparison

    String
       ↓
    Immutable

    StringBuilder
       ↓
    Mutable + Not synchronized

    StringBuffer
       ↓
    Mutable + Synchronized

Therefore, StringBuffer can be useful when multiple threads access the same mutable character sequence and synchronized operations are required.

---

# 3. 🏗️ StringBuffer as a Class

`StringBuffer` is a class.

Conceptually:

    Object
       │
       └── AbstractStringBuilder
                │
                ├── StringBuilder
                │
                └── StringBuffer

Both:

    StringBuilder
    StringBuffer

provide mutable character sequences.

Their major difference is synchronization.

---

# 4. 📦 StringBuffer Package

`StringBuffer` belongs to:

    java.lang

Therefore, no explicit import is required.

You can directly write:

    StringBuffer sb = new StringBuffer();

---

# 5. 🆕 Creating StringBuffer

Common constructors include:

    new StringBuffer()

    new StringBuffer(String str)

    new StringBuffer(int capacity)

Example:

    StringBuffer sb1 = new StringBuffer();

    StringBuffer sb2 = new StringBuffer("Java");

    StringBuffer sb3 = new StringBuffer(100);

---

# 6. 🔹 Default Constructor

Example:

    StringBuffer sb = new StringBuffer();

The default initial capacity is:

    16

So initially:

    length   = 0
    capacity = 16

### Important

Capacity and length are different.

    length()
        ↓
    Number of characters currently stored

    capacity()
        ↓
    Internal capacity available before expansion

---

# 7. 🔹 Constructor with String

Example:

    StringBuffer sb = new StringBuffer("Java");

The String contains:

    Java

Length:

    4

The initial capacity is:

    4 + 16

Therefore:

    capacity = 20

### Formula

For a String constructor:

    initial capacity = string.length() + 16

---

# 8. 🔹 Constructor with Capacity

You can specify the initial capacity.

Example:

    StringBuffer sb = new StringBuffer(100);

Initially:

    length   = 0
    capacity = 100

This can be useful if you know approximately how much text you are going to build.

---

# 9. 🔄 Mutability

StringBuffer is mutable.

Example:

    StringBuffer sb = new StringBuffer("Java");

    sb.append(" Programming");

    System.out.println(sb);

Output:

    Java Programming

The same StringBuffer object can be modified.

### Contrast with String

    String s = "Java";

    s.concat(" Programming");

The original String does not change.

Why?

Because:

    String → Immutable

while:

    StringBuffer → Mutable

---

# 10. ⚙️ Internal Working

StringBuffer maintains a mutable character sequence internally.

Conceptually:

    StringBuffer
          │
          ↓
    Internal character storage
          │
          ├── J
          ├── a
          ├── v
          └── a

When characters are appended or inserted, the internal storage is modified.

If there is not enough capacity, the internal storage grows.

### Important Interview Point

You do not need to know the exact internal representation for normal StringBuffer usage.

Remember:

> StringBuffer maintains expandable mutable character storage and synchronizes its methods.

---

# 11. 📦 Capacity

Capacity tells us how much internal storage is currently available before expansion becomes necessary.

Example:

    StringBuffer sb = new StringBuffer();

Initially:

    length   = 0
    capacity = 16

After:

    sb.append("Java");

Now:

    length   = 4
    capacity = 16

The capacity does not automatically become 4.

---

# 12. 📏 length()

`length()` returns the number of characters currently stored.

Example:

    StringBuffer sb = new StringBuffer("Java");

    System.out.println(sb.length());

Output:

    4

### Remember

    length()
        ↓
    Current characters

    capacity()
        ↓
    Current internal capacity

---

# 13. ➕

# append()

`append()` adds data to the end of the StringBuffer.

Example:

    StringBuffer sb = new StringBuffer("Java");

    sb.append(" Programming");

    System.out.println(sb);

Output:

    Java Programming

### Append Integer

    sb.append(100);

### Append Character

    sb.append('A');

### Append Boolean

    sb.append(true);

### Append Double

    sb.append(10.5);

`append()` is overloaded for many types.

---

# 14. ➕

# insert()

`insert()` inserts data at a specified index.

Example:

    StringBuffer sb = new StringBuffer("Jav");

    sb.insert(3, 'a');

Result:

    Java

Another example:

    StringBuffer sb = new StringBuffer("Java");

    sb.insert(4, " Programming");

Result:

    Java Programming

### Index Visualization

    J a v a
    0 1 2 3

Insert at:

    4

means:

    Java| Programming

---

# 15. 🔄 replace()

`replace()` replaces characters between two indexes.

Syntax:

    replace(start, end, str)

Example:

    StringBuffer sb = new StringBuffer("Java Programming");

    sb.replace(0, 4, "Python");

Result:

    Python Programming

### Range Rule

    start → inclusive
    end   → exclusive

---

# 16. 🗑️ delete()

`delete()` removes characters from a specified range.

Syntax:

    delete(start, end)

Example:

    StringBuffer sb = new StringBuffer("Java Programming");

    sb.delete(4, 5);

Result:

    JavaProgramming

The character at index 4 is the space.

### Range

    start → inclusive
    end   → exclusive

---

# 17. 🗑️ deleteCharAt()

Removes one character from a specific index.

Example:

    StringBuffer sb = new StringBuffer("Java");

    sb.deleteCharAt(1);

Result:

    Jva

Indexes:

    J a v a
    0 1 2 3

Index `1` is:

    a

---

# 18. 🔄 reverse()

`reverse()` reverses the entire character sequence.

Example:

    StringBuffer sb = new StringBuffer("Java");

    sb.reverse();

    System.out.println(sb);

Output:

    avaJ

Another example:

    StringBuffer sb = new StringBuffer("12345");

    sb.reverse();

Result:

    54321

### Important

`reverse()` modifies the StringBuffer itself.

---

# 19. 🔤 charAt()

`charAt()` returns the character at a specified index.

Example:

    StringBuffer sb = new StringBuffer("Java");

    System.out.println(sb.charAt(2));

Output:

    v

Indexes:

    J a v a
    0 1 2 3

---

# 20. ✏️ setCharAt()

`setCharAt()` replaces a character at a specified index.

Example:

    StringBuffer sb = new StringBuffer("Java");

    sb.setCharAt(0, 'K');

Result:

    Kava

Before:

    Java

After:

    Kava

---

# 21. ✂️ substring()

`substring()` extracts a portion of the character sequence.

Example:

    StringBuffer sb = new StringBuffer("Java Programming");

    String result = sb.substring(5);

Result:

    Programming

### Very Important

`substring()` returns:

    String

not:

    StringBuffer

Example:

    String result = sb.substring(0, 4);

---

# 22. 🔎 indexOf()

Returns the index of the first occurrence of a specified String.

Example:

    StringBuffer sb = new StringBuffer("Java Programming");

    System.out.println(sb.indexOf("Programming"));

Output:

    5

If the string does not exist:

    -1

---

# 23. 🔍 lastIndexOf()

Returns the index of the last occurrence.

Example:

    StringBuffer sb = new StringBuffer("Java Java");

    System.out.println(sb.lastIndexOf("Java"));

Output:

    5

---

# 24. 📏 setLength()

`setLength()` changes the logical length of the StringBuffer.

Example:

    StringBuffer sb = new StringBuffer("Java");

    sb.setLength(2);

    System.out.println(sb);

Output:

    Ja

### Increasing Length

Example:

    StringBuffer sb = new StringBuffer("Java");

    sb.setLength(6);

The logical length becomes:

    6

The additional positions contain the null character:

    '\u0000'

### Important

`setLength()` changes the logical length.

It does not mean the capacity becomes the same value.

---

# 25. 📦 ensureCapacity()

`ensureCapacity()` ensures that the StringBuffer has at least the requested capacity.

Example:

    StringBuffer sb = new StringBuffer();

    sb.ensureCapacity(100);

Now the buffer has sufficient capacity for at least:

    100 characters

This can reduce the need for repeated resizing when the expected size is known.

---

# 26. ✂️ trimToSize()

`trimToSize()` attempts to reduce the capacity so that it is closer to the current length.

Example:

    StringBuffer sb = new StringBuffer(100);

    sb.append("Java");

Before:

    length   = 4
    capacity = 100

After:

    sb.trimToSize();

The capacity may be reduced to approximately the current length.

---

# 27. 🔤 toString()

`toString()` converts the StringBuffer into a String.

Example:

    StringBuffer sb = new StringBuffer("Java");

    String result = sb.toString();

Now:

    sb     → StringBuffer
    result → String

This is commonly done when string construction is complete.

---

# 28. 🔗 Method Chaining

Many StringBuffer modification methods return the same StringBuffer instance.

Therefore, chaining is possible.

Example:

    StringBuffer sb = new StringBuffer();

    sb.append("Java")
      .append(" ")
      .append("Programming");

Result:

    Java Programming

Another example:

    StringBuffer sb = new StringBuffer("Java");

    sb.append(" Programming")
      .reverse();

Result:

    gnimmargorP avaJ

---

# 29. 🔒 StringBuffer and Synchronization

This is the most important difference between StringBuffer and StringBuilder.

StringBuffer methods are synchronized.

Conceptually:

    Thread 1
       │
       ↓
    StringBuffer
       │
       🔒
       │
       ↓
    method execution

Another thread attempting to execute a synchronized method on the same object may have to wait for the lock.

### Why?

To prevent multiple threads from simultaneously performing conflicting modifications through those synchronized operations.

---

# 30. 🧵 Thread Safety

StringBuffer is commonly described as thread-safe because its public methods are synchronized.

Example:

    StringBuffer sb = new StringBuffer();

    Thread 1 → append("Hello")
    Thread 2 → append("World")

The synchronization on methods provides mutual exclusion for those individual method calls on the same object.

### Important Nuance

Thread-safe does NOT automatically mean:

> Every sequence of multiple operations is atomic.

For example:

    if(sb.length() > 0) {
        sb.deleteCharAt(0);
    }

The entire sequence is not automatically one indivisible operation merely because the individual methods are synchronized.

If a larger compound operation must be atomic, external synchronization or another concurrency design may be required.

---

# 31. 📈 Capacity Growth

When StringBuffer does not have enough capacity, it expands its internal storage.

The commonly documented growth rule is approximately:

    newCapacity = oldCapacity * 2 + 2

Example:

    old capacity = 16

Potential new capacity:

    16 × 2 + 2
    = 34

If the required minimum capacity is larger, the implementation ensures enough capacity for the requested content.

### Interview Tip

Do not treat the exact growth strategy as an immutable language guarantee for all future implementations.

Remember:

> StringBuffer automatically expands when its capacity is insufficient.

---

# 32. 🆚 StringBuffer vs String

| Feature | String | StringBuffer |
|---|---|---|
| Mutable | ❌ | ✅ |
| Immutable | ✅ | ❌ |
| Modification | Creates new String result | Modifies mutable buffer |
| Thread-safe sharing | Immutable | Synchronized methods |
| `append()` | ❌ | ✅ |
| `reverse()` | ❌ | ✅ |
| `setCharAt()` | ❌ | ✅ |
| Repeated modifications | Less suitable | Suitable |
| Main purpose | Immutable text | Synchronized mutable text |

### Simple Rule

    String
       ↓
    Fixed / immutable text

    StringBuffer
       ↓
    Mutable + synchronized text

---

# 33. 🆚 StringBuffer vs StringBuilder

This is one of the most frequently asked Java interview questions.

| Feature | StringBuilder | StringBuffer |
|---|---|---|
| Mutable | ✅ | ✅ |
| Synchronized | ❌ | ✅ |
| Thread-safe for individual methods | ❌ | ✅ |
| Performance | Generally faster | Generally slower |
| Introduced | Java 5 | Java 1.0 |
| Use case | General mutable string building | Synchronized mutable operations |

### Easy Memory Trick

    Builder
       ↓
    Build fast
       ↓
    No synchronization

    Buffer
       ↓
    Shared mutable buffer
       ↓
    Synchronization

---

# 34. ⚡ Performance

StringBuffer generally has more synchronization overhead than StringBuilder.

Therefore, when thread safety is not required:

    StringBuilder

is generally preferred.

When synchronized mutable string operations are required:

    StringBuffer

may be appropriate.

### Important

Do not say:

> StringBuffer is always slow.

Better:

> StringBuffer generally has additional synchronization overhead compared with StringBuilder.

---

# 35. ✅ Advantages

### 1. Mutable

Text can be modified without creating a new immutable String for every operation.

### 2. Synchronized

Its methods provide synchronization for individual operations.

### 3. Useful for Shared Mutable Data

It can be useful when multiple threads access the same StringBuffer and synchronized method operations are appropriate.

### 4. Rich API

It supports:

    append()
    insert()
    delete()
    replace()
    reverse()
    charAt()
    setCharAt()
    substring()
    etc.

### 5. Resizable

Its internal capacity can automatically grow.

---

# 36. ❌ Disadvantages

### 1. Synchronization Overhead

Synchronization can add overhead compared with StringBuilder.

### 2. Usually Unnecessary for Single-Threaded Code

If only one thread is modifying the buffer, StringBuilder is usually the more appropriate choice.

### 3. Mutable Shared State Requires Care

Even with synchronized individual methods, multi-operation logic may require additional synchronization.

---

# 37. ⚠️ Common Mistakes

## ❌ Mistake 1 — Thinking StringBuffer is immutable

Wrong:

    StringBuffer is immutable.

Correct:

    StringBuffer is mutable.

---

## ❌ Mistake 2 — Thinking StringBuilder is synchronized

Wrong:

    StringBuilder is thread-safe.

Correct:

    StringBuilder is not synchronized.

---

## ❌ Mistake 3 — Thinking StringBuffer is always faster

Wrong:

    StringBuffer is faster because it is optimized for threads.

Correct:

    Synchronization adds overhead, so StringBuilder is generally faster when synchronization is unnecessary.

---

## ❌ Mistake 4 — Confusing length and capacity

Remember:

    length()
        ↓
    Current content size

    capacity()
        ↓
    Internal capacity

---

## ❌ Mistake 5 — Thinking substring() returns StringBuffer

It returns:

    String

---

## ❌ Mistake 6 — Thinking every sequence of synchronized methods is atomic

Individual methods are synchronized, but a sequence of multiple method calls is not automatically one atomic operation.

---

# 38. 🚨 Interview Traps

## Trap 1

    StringBuffer sb = new StringBuffer("Java");

    sb.append(" Programming");

    System.out.println(sb);

Output:

    Java Programming

---

## Trap 2

    StringBuffer sb = new StringBuffer("Java");

    sb.reverse();

    System.out.println(sb);

Output:

    avaJ

---

## Trap 3

    StringBuffer sb = new StringBuffer();

    System.out.println(sb.length());
    System.out.println(sb.capacity());

Output:

    0
    16

---

## Trap 4

    StringBuffer sb = new StringBuffer("Java");

    sb.setCharAt(0, 'K');

Output:

    Kava

---

## Trap 5

    StringBuffer sb = new StringBuffer("Java");

    String s = sb.substring(1, 3);

Type of `s`:

    String

---

## Trap 6

    StringBuffer sb = new StringBuffer("Java");

    sb.delete(1, 3);

Result:

    Ja

Why?

Indexes:

    J a v a
    0 1 2 3

Indexes 1 and 2 are removed.

Index 3 is exclusive.

---

## Trap 7

    StringBuffer sb = new StringBuffer();

    sb.append("Java")
      .append(" ")
      .append("Developer");

Result:

    Java Developer

---

## Trap 8

Which is generally preferred when synchronization is unnecessary?

    StringBuilder

Not:

    StringBuffer

---

# 39. 🔥 Top 20 Interview Questions

## Q1. What is StringBuffer?

**Answer:**

StringBuffer is a mutable sequence of characters whose methods are synchronized.

---

## Q2. Is StringBuffer mutable?

**Answer:**

Yes.

Its character sequence can be modified after creation.

---

## Q3. Where is StringBuffer located?

**Answer:**

It is part of:

    java.lang

---

## Q4. Is StringBuffer thread-safe?

**Answer:**

Its methods are synchronized, providing thread-safe individual method operations on a StringBuffer instance.

---

## Q5. Why is StringBuffer synchronized?

**Answer:**

Synchronization is used to coordinate access when multiple threads operate on the same mutable buffer.

---

## Q6. What is the default capacity of StringBuffer?

**Answer:**

The default initial capacity is:

    16

---

## Q7. What is the initial capacity of new StringBuffer("Java")?

**Answer:**

    4 + 16 = 20

---

## Q8. Difference between length() and capacity()?

**Answer:**

`length()` gives the number of characters currently stored.

`capacity()` gives the current internal capacity.

---

## Q9. Is StringBuffer mutable?

**Answer:**

Yes.

Methods such as `append()`, `insert()`, `delete()`, and `reverse()` modify its content.

---

## Q10. What does append() do?

**Answer:**

It adds data at the end of the StringBuffer.

---

## Q11. What does reverse() do?

**Answer:**

It reverses the character sequence.

---

## Q12. What does setCharAt() do?

**Answer:**

It replaces the character at a specified index.

---

## Q13. What does toString() do?

**Answer:**

It converts the StringBuffer into a String.

---

## Q14. StringBuffer vs StringBuilder?

**Answer:**

Both are mutable.

StringBuffer provides synchronization.

StringBuilder does not.

---

## Q15. Which is generally faster: StringBuilder or StringBuffer?

**Answer:**

StringBuilder is generally faster when synchronization is not required because StringBuffer has synchronization overhead.

---

## Q16. StringBuffer vs String?

**Answer:**

String is immutable.

StringBuffer is mutable and synchronized.

---

## Q17. Can StringBuffer capacity increase automatically?

**Answer:**

Yes.

It automatically expands its internal storage when required.

---

## Q18. What is the difference between capacity and length?

**Answer:**

Length represents the number of characters currently stored, while capacity represents the internal storage available.

---

## Q19. Does StringBuffer guarantee that a multi-method operation is atomic?

**Answer:**

No.

Individual synchronized methods are protected, but a sequence of multiple method calls may require additional synchronization.

---

## Q20. When should you use StringBuffer?

**Answer:**

Use it when you need mutable string manipulation and synchronized access to the same buffer is required.

---

# 40. 🎤 30-Second Interview Answer

> **StringBuffer is a mutable sequence of characters provided by the `java.lang` package. It is similar to StringBuilder, but its methods are synchronized, which provides thread-safe individual operations on the buffer. Because of this synchronization, it generally has more overhead than StringBuilder. StringBuffer is useful when mutable string data is shared between threads and synchronized operations are required.**

---

# 41. 🧾 Cheat Sheet

| Method | Purpose | Return Type |
|---|---|---|
| `length()` | Current character count | `int` |
| `capacity()` | Current internal capacity | `int` |
| `append()` | Add at end | `StringBuffer` |
| `insert()` | Insert at index | `StringBuffer` |
| `replace()` | Replace range | `StringBuffer` |
| `delete()` | Delete range | `StringBuffer` |
| `deleteCharAt()` | Delete character | `StringBuffer` |
| `reverse()` | Reverse content | `StringBuffer` |
| `charAt()` | Read character | `char` |
| `setCharAt()` | Modify character | `void` |
| `substring()` | Extract portion | `String` |
| `indexOf()` | Find first occurrence | `int` |
| `lastIndexOf()` | Find last occurrence | `int` |
| `setLength()` | Change logical length | `void` |
| `ensureCapacity()` | Ensure minimum capacity | `void` |
| `trimToSize()` | Reduce unused capacity | `void` |
| `toString()` | Convert to String | `String` |

---

# 42. 🧠 Memory Tricks

## 🔥 The Three Strings

Remember:

    String
       ↓
    Immutable

    StringBuilder
       ↓
    Mutable + Not synchronized

    StringBuffer
       ↓
    Mutable + Synchronized

---

## 🔥 Builder vs Buffer

Think:

    Builder
       ↓
    Build
       ↓
    Fast mutable building

    Buffer
       ↓
    Shared buffer
       ↓
    Synchronization

---

## 🔥 Mutation Methods

Remember:

    A I R D R

    A → append()
    I → insert()
    R → replace()
    D → delete()
    R → reverse()

---

## 🔥 Character Methods

    charAt()
        ↓
    Read

    setCharAt()
        ↓
    Modify

---

## 🔥 Capacity

Remember:

    Length ≠ Capacity

    length()
        ↓
    Actual content

    capacity()
        ↓
    Internal available storage

---

# ⭐ Most Important Interview Points

Before moving ahead, make sure you understand these:

    1. StringBuffer is mutable.
    2. StringBuffer belongs to java.lang.
    3. StringBuffer is synchronized.
    4. StringBuffer supports mutable character sequences.
    5. Default capacity is 16.
    6. String constructor capacity = length + 16.
    7. length() and capacity() are different.
    8. append() adds data at the end.
    9. insert() adds data at an index.
    10. delete() removes a range.
    11. reverse() reverses the buffer.
    12. setCharAt() changes one character.
    13. substring() returns String.
    14. toString() converts to String.
    15. StringBuffer generally has more overhead than StringBuilder.
    16. Individual methods are synchronized.
    17. Multiple operations are not automatically one atomic transaction.
    18. StringBuilder is generally preferred when synchronization is unnecessary.

---

# 43. 🔗 Next Topic

Our String playlist:

    04-Strings/
    │
    ├── 01-String-Introduction.md
    ├── 02-String-Pool.md
    ├── 03-String-Immutability.md
    ├── 04-String-Methods.md
    ├── 05-StringBuilder.md
    ├── 06-StringBuffer.md        ← YOU ARE HERE
    ├── 07-String-vs-StringBuilder-vs-StringBuffer.md
    └── 08-String-Interview-Questions.md

### Learning Flow

    String
      ↓
    String Pool
      ↓
    String Immutability
      ↓
    String Methods
      ↓
    StringBuilder
      ↓
    StringBuffer
      ↓
    String vs StringBuilder vs StringBuffer
      ↓
    String Interview Questions

---

# 🚀 Final Revision

Before moving to the comparison topic, you should be able to answer:

    ❓ What is StringBuffer?
    ❓ Why is StringBuffer mutable?
    ❓ Why is StringBuffer synchronized?
    ❓ What is its default capacity?
    ❓ What is the difference between length and capacity?
    ❓ How does append() work?
    ❓ How does insert() work?
    ❓ How does delete() work?
    ❓ How does reverse() work?
    ❓ What does setCharAt() do?
    ❓ What does substring() return?
    ❓ What does toString() return?
    ❓ Is StringBuffer thread-safe?
    ❓ Is every sequence of StringBuffer operations atomic?
    ❓ StringBuffer vs StringBuilder?
    ❓ StringBuffer vs String?
    ❓ Why can StringBuilder be faster?

> ⭐ **Core Idea:**  
> **StringBuffer = Mutable + Synchronized.**
>
> If you remember only one line for the interview, remember this:
>
> **String is immutable, StringBuilder is mutable and unsynchronized, while StringBuffer is mutable and synchronized.**