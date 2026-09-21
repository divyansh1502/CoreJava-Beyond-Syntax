# 🧱 StringBuilder in Java

> **`StringBuilder` is a mutable sequence of characters used when String data needs to be modified frequently. Unlike `String`, it does not create a new object for every modification, making it generally more efficient for repeated string manipulation.**

---

# 📌 Table of Contents

1. [What is StringBuilder?](#1--what-is-stringbuilder)
2. [Why Do We Need StringBuilder?](#2--why-do-we-need-stringbuilder)
3. [String vs StringBuilder](#3--string-vs-stringbuilder)
4. [StringBuilder Class](#4--stringbuilder-class)
5. [Creating StringBuilder](#5--creating-stringbuilder)
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
28. [Chaining Methods](#28--chaining-methods)
29. [StringBuilder and + Operator](#29--stringbuilder-and--operator)
30. [StringBuilder and Memory](#30--stringbuilder-and-memory)
31. [Capacity Growth](#31--capacity-growth)
32. [Time Complexity](#32--time-complexity)
33. [StringBuilder and Thread Safety](#33--stringbuilder-and-thread-safety)
34. [StringBuilder vs StringBuffer](#34--stringbuilder-vs-stringbuffer)
35. [StringBuilder vs String](#35--stringbuilder-vs-string)
36. [Common Mistakes](#36--common-mistakes)
37. [Interview Traps](#37--interview-traps)
38. [Top 20 Interview Questions](#38--top-20-interview-questions)
39. [30-Second Interview Answer](#39--30-second-interview-answer)
40. [Cheat Sheet](#40--cheat-sheet)
41. [Memory Tricks](#41--memory-tricks)
42. [Next Topic](#42--next-topic)

---

# 1. 🔤 What is StringBuilder?

`StringBuilder` is a class in:

    java.lang

It represents a:

> **Mutable sequence of characters.**

The important word is:

    Mutable

which means its content can be changed without creating a completely new String object for every modification.

Example:

    StringBuilder sb = new StringBuilder("Java");

    sb.append(" Programming");

    System.out.println(sb);

Output:

    Java Programming

The same `StringBuilder` object can be modified.

---

# 2. 🤔 Why Do We Need StringBuilder?

Consider this:

    String s = "Java";

    s = s + " ";
    s = s + "Programming";
    s = s + " Language";

Because `String` is immutable, every modification can involve creating another String object.

For a small number of operations this is usually fine.

But suppose we repeatedly modify text inside a loop:

    String result = "";

    for(int i = 0; i < 10000; i++) {
        result = result + i;
    }

This can create many intermediate String objects.

Instead:

    StringBuilder sb = new StringBuilder();

    for(int i = 0; i < 10000; i++) {
        sb.append(i);
    }

This is generally much more efficient for repeated modifications.

---

# 3. 🆚 String vs StringBuilder

| Feature | String | StringBuilder |
|---|---|---|
| Mutability | Immutable | Mutable |
| Package | `java.lang` | `java.lang` |
| Can modify same object? | No | Yes |
| Repeated modifications | Less suitable | More suitable |
| Thread-safe | Immutable, therefore inherently safe to share | No |
| Synchronization | Not applicable | No synchronization |
| Performance for repeated changes | Usually lower | Usually better |
| Main use | Fixed text | Frequently changing text |

### Golden Rule

    String
       ↓
    Immutable

    StringBuilder
       ↓
    Mutable

---

# 4. 🏗️ StringBuilder Class

`StringBuilder` is a class from:

    java.lang

Therefore, no import is required.

You can directly write:

    StringBuilder sb = new StringBuilder();

### Hierarchy

Conceptually:

    Object
       │
       └── AbstractStringBuilder
                │
                ├── StringBuilder
                │
                └── StringBuffer

`StringBuilder` and `StringBuffer` are both mutable character sequences.

---

# 5. 🆕 Creating StringBuilder

There are several constructors.

Common ones are:

    new StringBuilder()

    new StringBuilder(String str)

    new StringBuilder(int capacity)

Example:

    StringBuilder sb1 = new StringBuilder();

    StringBuilder sb2 = new StringBuilder("Java");

    StringBuilder sb3 = new StringBuilder(100);

---

# 6. 🔹 Default Constructor

Example:

    StringBuilder sb = new StringBuilder();

This creates an empty StringBuilder with a default initial capacity.

The default capacity is:

    16 characters

So conceptually:

    length   = 0
    capacity = 16

Important:

> Capacity is NOT the same as length.

---

# 7. 🔹 Constructor with String

Example:

    StringBuilder sb = new StringBuilder("Java");

Now:

    content = "Java"

Length:

    4

Initial capacity:

    4 + 16 = 20

So:

    length   = 4
    capacity = 20

### Important Formula

For this constructor:

    initial capacity = string length + 16

---

# 8. 🔹 Constructor with Capacity

You can directly specify an initial capacity.

Example:

    StringBuilder sb = new StringBuilder(100);

Now the initial capacity is:

    100

Length is:

    0

So:

    length   = 0
    capacity = 100

### Why Specify Capacity?

If you already have an approximate idea of the required size, preallocating capacity can reduce the number of internal expansions.

---

# 9. 🔄 Mutability

This is the most important concept.

### String

    String s = "Java";

    s.concat(" Programming");

The original String remains:

    Java

because String is immutable.

---

### StringBuilder

    StringBuilder sb = new StringBuilder("Java");

    sb.append(" Programming");

Now the same StringBuilder contains:

    Java Programming

### Visualization

String:

    "Java"
       ↓
    concat()
       ↓
    "Java Programming"

New String object is returned.

StringBuilder:

    "Java"
       ↓
    append()
       ↓
    "Java Programming"

The existing mutable object is modified.

---

# 10. ⚙️ Internal Working

A StringBuilder maintains a mutable character sequence internally.

Conceptually:

    StringBuilder
         │
         ↓
    internal character storage
         │
         ├── J
         ├── a
         ├── v
         ├── a
         └── ...

When more characters are added:

    append(" Programming")

the internal storage is updated.

If the current capacity is insufficient, a larger internal storage area is allocated and the existing contents are copied.

### Important

Modern Java implementations use internal representation details that may differ across JDK versions.

For interview purposes, remember:

> StringBuilder maintains expandable internal storage for its mutable character sequence.

---

# 11. 📦 Capacity

Capacity represents how many characters can be stored before the internal storage needs to grow.

Example:

    StringBuilder sb = new StringBuilder();

Initially:

    length   = 0
    capacity = 16

After:

    sb.append("Java");

Now:

    length   = 4
    capacity = 16

Notice:

    capacity ≠ length

### Check Capacity

    System.out.println(sb.capacity());

---

# 12. 📏 length()

`length()` returns the number of characters currently stored.

Example:

    StringBuilder sb = new StringBuilder("Java");

    System.out.println(sb.length());

Output:

    4

### Difference

    length()
        ↓
    Characters currently stored

    capacity()
        ↓
    Available internal capacity before expansion

---

# 13. ➕

# append()

`append()` adds data to the end.

Example:

    StringBuilder sb = new StringBuilder("Java");

    sb.append(" Programming");

    System.out.println(sb);

Output:

    Java Programming

### Append Different Types

`append()` is overloaded for many data types.

Examples:

    sb.append(100);
    sb.append(10.5);
    sb.append(true);
    sb.append('A');

Example:

    StringBuilder sb = new StringBuilder();

    sb.append("Age: ");
    sb.append(22);

Result:

    Age: 22

### Append String

    sb.append("Java");

### Append Character

    sb.append('!');

### Append Boolean

    sb.append(true);

### Append Object

    sb.append(object);

The object is converted to its String representation.

---

# 14. ➕ insert()

`insert()` adds data at a specified index.

Example:

    StringBuilder sb = new StringBuilder("Jav");

    sb.insert(3, 'a');

    System.out.println(sb);

Output:

    Java

Another example:

    StringBuilder sb = new StringBuilder("Java");

    sb.insert(4, " Programming");

Result:

    Java Programming

### Visualization

Before:

    Java
    0123

Insert at index 4:

    Java| Programming

After:

    Java Programming

---

# 15. 🔄 replace()

`replace()` replaces characters between a specified range.

Syntax:

    replace(start, end, str)

Example:

    StringBuilder sb = new StringBuilder("Java Programming");

    sb.replace(0, 4, "Python");

Result:

    Python Programming

### Important

Just like `substring()`:

    start → inclusive
    end   → exclusive

---

# 16. 🗑️ delete()

`delete()` removes characters from a range.

Syntax:

    delete(start, end)

Example:

    StringBuilder sb = new StringBuilder("Java Programming");

    sb.delete(4, 5);

Result:

    JavaProgramming

Here:

    index 4

contains the space.

So the space is removed.

### Range Rule

    start → inclusive
    end   → exclusive

---

# 17. 🗑️ deleteCharAt()

Removes the character at a specific index.

Example:

    StringBuilder sb = new StringBuilder("Java");

    sb.deleteCharAt(1);

Result:

    Jva

Indexes:

    J a v a
    0 1 2 3

Index 1:

    a

is removed.

---

# 18. 🔄 reverse()

`reverse()` reverses the character sequence.

Example:

    StringBuilder sb = new StringBuilder("Java");

    sb.reverse();

    System.out.println(sb);

Output:

    avaJ

### Very Useful for Number/String Problems

Example:

    StringBuilder sb = new StringBuilder("12345");

    sb.reverse();

Result:

    54321

### Important

`reverse()` modifies the existing StringBuilder.

---

# 19. 🔤 charAt()

`charAt()` returns the character at a specified index.

Example:

    StringBuilder sb = new StringBuilder("Java");

    System.out.println(sb.charAt(2));

Output:

    v

Indexes:

    J a v a
    0 1 2 3

---

# 20. ✏️ setCharAt()

`setCharAt()` replaces the character at a specified index.

Example:

    StringBuilder sb = new StringBuilder("Java");

    sb.setCharAt(0, 'K');

    System.out.println(sb);

Output:

    Kava

Before:

    Java

After:

    Kava

### Important Difference

String:

    No setCharAt()

StringBuilder:

    setCharAt()

because StringBuilder is mutable.

---

# 21. ✂️ substring()

StringBuilder also supports `substring()`.

Example:

    StringBuilder sb = new StringBuilder("Java Programming");

    String result = sb.substring(5);

Result:

    Programming

### Important

`substring()` returns a:

    String

not a:

    StringBuilder

This is an important interview point.

---

# 22. 🔎 indexOf()

StringBuilder provides `indexOf()`.

Example:

    StringBuilder sb = new StringBuilder("Java Programming");

    System.out.println(sb.indexOf("Programming"));

Output:

    5

If not found:

    -1

---

# 23. 🔍 lastIndexOf()

Returns the index of the last occurrence.

Example:

    StringBuilder sb = new StringBuilder("Java Java");

    System.out.println(sb.lastIndexOf("Java"));

Output:

    5

The first `"Java"` starts at:

    0

The second starts at:

    5

---

# 24. 📏 setLength()

`setLength()` changes the logical length of the StringBuilder.

Example:

    StringBuilder sb = new StringBuilder("Java");

    sb.setLength(2);

    System.out.println(sb);

Output:

    Ja

### Increasing Length

You can also increase the length.

Conceptually, newly created positions are filled with:

    '\u0000'

the null character.

Example:

    StringBuilder sb = new StringBuilder("Java");

    sb.setLength(6);

Now the logical length is:

    6

The newly added positions contain null characters.

### Important

`setLength()` changes the length, not necessarily the capacity.

---

# 25. 📦 ensureCapacity()

`ensureCapacity()` ensures that the internal capacity is at least the requested amount.

Example:

    StringBuilder sb = new StringBuilder();

    sb.ensureCapacity(100);

Now the builder has capacity sufficient for at least:

    100 characters

### Why Use It?

Useful when you know approximately how much data will be appended.

It can reduce repeated capacity expansion.

---

# 26. ✂️ trimToSize()

`trimToSize()` attempts to reduce the internal capacity to match the current length.

Example:

    StringBuilder sb = new StringBuilder(100);

    sb.append("Java");

Before:

    length   = 4
    capacity = 100

After:

    sb.trimToSize();

The capacity can be reduced to approximately:

    4

### Important

It is generally used when minimizing unused internal storage matters.

---

# 27. 🔤 toString()

`toString()` converts the StringBuilder content into a `String`.

Example:

    StringBuilder sb = new StringBuilder("Java");

    String s = sb.toString();

Now:

    sb → StringBuilder
    s  → String

### Why Is It Important?

Many APIs expect a `String`, not a `StringBuilder`.

So:

    String result = sb.toString();

is commonly used at the end of StringBuilder processing.

---

# 28. 🔗 Chaining Methods

Many StringBuilder methods return the same StringBuilder object.

Therefore, methods can be chained.

Example:

    StringBuilder sb = new StringBuilder();

    sb.append("Java")
      .append(" ")
      .append("Programming");

Result:

    Java Programming

Another:

    StringBuilder sb = new StringBuilder("Java");

    sb.append(" Programming")
      .reverse();

Result:

    gnimmargorP avaJ

### Why Does Chaining Work?

Methods such as:

    append()
    insert()
    delete()
    replace()
    reverse()

return the current StringBuilder object.

Conceptually:

    sb.append(...)
      ↓
    same sb
      ↓
    .append(...)
      ↓
    same sb

---

# 29. ⚡ StringBuilder and + Operator

Consider:

    String result = "";

    for(int i = 0; i < 10000; i++) {
        result += i;
    }

Repeated String concatenation can create many intermediate String objects.

For explicit repeated building:

    StringBuilder sb = new StringBuilder();

    for(int i = 0; i < 10000; i++) {
        sb.append(i);
    }

    String result = sb.toString();

### Important Modern Java Note

The Java compiler/runtime can optimize many simple `+` concatenation expressions, especially within a single expression.

For example:

    String result = "Hello " + name + "!";

The compiler may translate this efficiently.

However, for explicit repeated modifications, especially in loops, `StringBuilder` is still a standard and clear choice.

---

# 30. 🧠 StringBuilder and Memory

Consider:

    String s = "Java";

With String:

    s = s + " Programming";

The original String cannot be modified.

A new String result is produced.

With StringBuilder:

    StringBuilder sb = new StringBuilder("Java");

    sb.append(" Programming");

The mutable character sequence is updated.

### Conceptual Difference

String:

    String object
         ↓
       "Java"

    + " Programming"
         ↓
    new String object

StringBuilder:

    StringBuilder
         ↓
    mutable storage
         ↓
    "Java"

    append()
         ↓
    "Java Programming"

---

# 31. 📈 Capacity Growth

When the current capacity is insufficient, StringBuilder expands its internal storage.

The commonly documented growth rule for the standard implementation is approximately:

    newCapacity = oldCapacity * 2 + 2

Example:

    old capacity = 16

Potential new capacity:

    16 * 2 + 2
    = 34

If the requested minimum capacity is larger, the implementation ensures the resulting capacity is large enough for that requirement.

### Important

Do not assume every Java implementation must use exactly the same internal strategy forever.

For interviews, remember:

> StringBuilder automatically grows its internal capacity when required.

---

# 32. ⏱️ Time Complexity

Complexity depends on the operation and whether internal storage expansion occurs.

| Operation | Typical Complexity |
|---|---:|
| `charAt()` | O(1) |
| `setCharAt()` | O(1) |
| `append()` | Amortized O(1) |
| `insert()` | O(n) |
| `delete()` | O(n) |
| `deleteCharAt()` | O(n) |
| `replace()` | O(n) |
| `reverse()` | O(n) |
| `substring()` | O(k), where k is result length |
| `indexOf()` | O(n) |
| `lastIndexOf()` | O(n) |
| `toString()` | O(n) |

### Important

`append()` is usually described as:

    Amortized O(1)

because most appends are cheap, while occasional capacity expansions are more expensive.

---

# 33. 🧵 StringBuilder and Thread Safety

`StringBuilder` is:

> **Not synchronized and not thread-safe for concurrent modifications.**

If multiple threads modify the same StringBuilder without external synchronization, race conditions can occur.

For single-threaded use:

    StringBuilder

is usually the preferred mutable string-building class.

For synchronized mutable string operations:

    StringBuffer

may be considered.

---

# 34. 🆚 StringBuilder vs StringBuffer

| Feature | StringBuilder | StringBuffer |
|---|---|---|
| Mutable | Yes | Yes |
| Thread-safe | No | Yes |
| Synchronized | No | Yes |
| Performance in single-threaded use | Generally faster | Generally slower |
| Introduced | Java 5 | Java 1.0 |
| Common use | Single-threaded string building | Legacy/concurrent synchronized scenarios |

### Easy Rule

    StringBuilder
        ↓
    Mutable + Not synchronized

    StringBuffer
        ↓
    Mutable + Synchronized

---

# 35. 🆚 StringBuilder vs String

| Feature | String | StringBuilder |
|---|---|---|
| Mutable | ❌ | ✅ |
| Can change existing object | ❌ | ✅ |
| Repeated modifications | Less suitable | Suitable |
| `append()` | ❌ | ✅ |
| `reverse()` | ❌ | ✅ |
| `setCharAt()` | ❌ | ✅ |
| Thread-safe sharing | Immutable | No |
| Common use | Fixed text | Building/modifying text |

### Example

String:

    String s = "Java";

    s = s + " World";

StringBuilder:

    StringBuilder sb = new StringBuilder("Java");

    sb.append(" World");

---

# 36. ⚠️ Common Mistakes

## ❌ Mistake 1 — Thinking StringBuilder is immutable

Wrong:

> StringBuilder cannot be changed.

Correct:

> StringBuilder is mutable.

---

## ❌ Mistake 2 — Forgetting to call toString()

If an API specifically requires a String:

    String result = sb.toString();

---

## ❌ Mistake 3 — Confusing length and capacity

Example:

    StringBuilder sb = new StringBuilder();

Initially:

    length = 0
    capacity = 16

They are different concepts.

---

## ❌ Mistake 4 — Thinking substring() returns StringBuilder

It returns:

    String

Example:

    String result = sb.substring(1, 3);

---

## ❌ Mistake 5 — Thinking StringBuilder is thread-safe

It is not synchronized.

---

## ❌ Mistake 6 — Forgetting index rules

For:

    replace()
    delete()
    substring()

the usual range convention is:

    start → inclusive
    end   → exclusive

---

# 37. 🚨 Interview Traps

## Trap 1

    StringBuilder sb = new StringBuilder("Java");

    sb.append(" World");

    System.out.println(sb);

Output:

    Java World

---

## Trap 2

    StringBuilder sb = new StringBuilder("Java");

    sb.reverse();

    System.out.println(sb);

Output:

    avaJ

---

## Trap 3

    StringBuilder sb = new StringBuilder("Java");

    sb.setCharAt(0, 'K');

    System.out.println(sb);

Output:

    Kava

---

## Trap 4

    StringBuilder sb = new StringBuilder("Java");

    String s = sb.substring(1, 3);

What is the type of `s`?

Answer:

    String

---

## Trap 5

    StringBuilder sb = new StringBuilder();

    System.out.println(sb.length());
    System.out.println(sb.capacity());

Output:

    0
    16

---

## Trap 6

    StringBuilder sb = new StringBuilder("Java");

    sb.append(" World");

    String s = sb.toString();

Now:

    sb → StringBuilder
    s  → String

---

## Trap 7

    StringBuilder sb = new StringBuilder("Java");

    sb.delete(1, 3);

Result:

    Ja

Why?

Original:

    J a v a
    0 1 2 3

Indexes `1` and `2` are deleted.

End index `3` is exclusive.

---

## Trap 8

    StringBuilder sb = new StringBuilder("Java");

    sb.insert(1, "XX");

Result:

    JXXava

---

# 38. 🔥 Top 20 Interview Questions

## Q1. What is StringBuilder?

**Answer:**

`StringBuilder` is a mutable sequence of characters used for efficient modification and construction of strings.

---

## Q2. Is StringBuilder mutable?

**Answer:**

Yes.

Its character sequence can be modified after creation.

---

## Q3. Where is StringBuilder located?

**Answer:**

It belongs to:

    java.lang

No explicit import is required.

---

## Q4. Is StringBuilder thread-safe?

**Answer:**

No.

StringBuilder is not synchronized.

---

## Q5. What is the difference between String and StringBuilder?

**Answer:**

String is immutable, while StringBuilder is mutable.

---

## Q6. Why is StringBuilder generally faster for repeated modifications?

**Answer:**

Because it modifies a mutable character sequence instead of requiring a new immutable String result for every modification.

---

## Q7. What is the default capacity of StringBuilder?

**Answer:**

The default initial capacity is:

    16

---

## Q8. What is the initial capacity when using StringBuilder(String)?

**Answer:**

It is:

    string.length() + 16

---

## Q9. What is the difference between length() and capacity()?

**Answer:**

`length()` is the number of characters currently stored.

`capacity()` is the amount of internal character storage available before expansion is required.

---

## Q10. What does append() do?

**Answer:**

It adds data to the end of the StringBuilder and returns the same StringBuilder instance.

---

## Q11. What does insert() do?

**Answer:**

It inserts data at a specified index.

---

## Q12. What does delete() do?

**Answer:**

It removes characters within a specified range.

The start index is inclusive and the end index is exclusive.

---

## Q13. What does reverse() do?

**Answer:**

It reverses the character sequence in the StringBuilder.

---

## Q14. What does setCharAt() do?

**Answer:**

It replaces the character at a specified index.

---

## Q15. What does toString() do?

**Answer:**

It converts the StringBuilder content into a String.

---

## Q16. Can StringBuilder be chained?

**Answer:**

Yes.

Many mutating methods return the same StringBuilder object.

Example:

    sb.append("Java")
      .append(" ")
      .append("Programming");

---

## Q17. What happens when StringBuilder capacity becomes insufficient?

**Answer:**

Its internal storage automatically grows to accommodate additional characters.

---

## Q18. What is amortized O(1) append?

**Answer:**

Most append operations are constant-time, but occasional internal resizing and copying can be more expensive. Averaged over many operations, append is amortized O(1).

---

## Q19. Difference between StringBuilder and StringBuffer?

**Answer:**

Both are mutable character sequences.

`StringBuilder` is not synchronized.

`StringBuffer` is synchronized.

---

## Q20. When should you use StringBuilder?

**Answer:**

Use it when you need to repeatedly build or modify text, especially in loops or other performance-sensitive string-building operations.

---

# 39. 🎤 30-Second Interview Answer

> **StringBuilder is a mutable sequence of characters provided by the `java.lang` package. Unlike String, which is immutable, StringBuilder allows modifications such as append, insert, delete, replace, and reverse on the same mutable object. It is useful when strings need to be modified repeatedly, especially inside loops. StringBuilder is not synchronized, so it is generally preferred for single-threaded string construction.**

---

# 40. 🧾 Cheat Sheet

| Method | Purpose | Return Type |
|---|---|---|
| `length()` | Current character count | `int` |
| `capacity()` | Current internal capacity | `int` |
| `append()` | Add at end | `StringBuilder` |
| `insert()` | Insert at index | `StringBuilder` |
| `replace()` | Replace range | `StringBuilder` |
| `delete()` | Delete range | `StringBuilder` |
| `deleteCharAt()` | Delete character | `StringBuilder` |
| `reverse()` | Reverse content | `StringBuilder` |
| `charAt()` | Get character | `char` |
| `setCharAt()` | Modify character | `void` |
| `substring()` | Extract portion | `String` |
| `indexOf()` | Find first occurrence | `int` |
| `lastIndexOf()` | Find last occurrence | `int` |
| `setLength()` | Change logical length | `void` |
| `ensureCapacity()` | Ensure minimum capacity | `void` |
| `trimToSize()` | Reduce unused capacity | `void` |
| `toString()` | Convert to String | `String` |

---

# 41. 🧠 Memory Tricks

## 🔥 Modification Methods

Remember:

    A I R D R

    A → append()
    I → insert()
    R → replace()
    D → delete()
    R → reverse()

These are the major mutation operations.

---

## 🔥 Character Methods

Remember:

    C S

    C → charAt()
    S → setCharAt()

Think:

    charAt()
        ↓
    Read character

    setCharAt()
        ↓
    Change character

---

## 🔥 Capacity

Remember:

    L ≠ C

    L → length
    C → capacity

Length tells you:

    "How many characters do I currently have?"

Capacity tells you:

    "How much internal space do I currently have?"

---

## 🔥 Conversion

Remember:

    Builder → String

Use:

    toString()

---

# ⭐ Most Important StringBuilder Methods

For interviews and DSA, prioritize:

    append()
    insert()
    delete()
    deleteCharAt()
    replace()
    reverse()
    charAt()
    setCharAt()
    substring()
    indexOf()
    length()
    capacity()
    toString()

---

# 42. 🔗 Next Topic

Our String playlist:

    04-Strings/
    │
    ├── 01-String-Introduction.md
    ├── 02-String-Pool.md
    ├── 03-String-Immutability.md
    ├── 04-String-Methods.md
    ├── 05-StringBuilder.md        ← YOU ARE HERE
    ├── 06-StringBuffer.md
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

Before moving to StringBuffer, make sure you can explain:

    1. What is StringBuilder?
    2. Why is it mutable?
    3. Why is it useful for repeated String modifications?
    4. What is its default capacity?
    5. Difference between length and capacity
    6. append()
    7. insert()
    8. replace()
    9. delete()
    10. deleteCharAt()
    11. reverse()
    12. charAt()
    13. setCharAt()
    14. substring()
    15. indexOf()
    16. lastIndexOf()
    17. setLength()
    18. ensureCapacity()
    19. trimToSize()
    20. toString()
    21. Why append() is amortized O(1)
    22. Why StringBuilder is not thread-safe
    23. StringBuilder vs String
    24. StringBuilder vs StringBuffer

> ⭐ **Core Idea:**  
> **String is immutable, StringBuilder is mutable. When you need to repeatedly construct or modify text, StringBuilder lets you work with a mutable character sequence instead of repeatedly creating new String results.**