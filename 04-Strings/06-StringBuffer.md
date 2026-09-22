# 🧵 StringBuffer in Java

> **`StringBuffer` is a mutable sequence of characters whose methods are synchronized, making it suitable when synchronized access to a shared mutable character sequence is required.**

---

# 📌 Table of Contents

1. [What is StringBuffer?](#1--what-is-stringbuffer)
2. [Why Do We Need StringBuffer?](#2--why-do-we-need-stringbuffer)
3. [StringBuffer as a Class](#3--stringbuffer-as-a-class)
4. [Package](#4--package)
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
29. [Synchronization](#29--synchronization)
30. [Thread Safety](#30--thread-safety)
31. [Capacity Growth](#31--capacity-growth)
32. [Time Complexity](#32--time-complexity)
33. [StringBuffer vs String](#33--stringbuffer-vs-string)
34. [StringBuffer vs StringBuilder](#34--stringbuffer-vs-stringbuilder)
35. [Performance](#35--performance)
36. [Advantages](#36--advantages)
37. [Disadvantages](#37--disadvantages)
38. [Common Mistakes](#38--common-mistakes)
39. [Interview Traps](#39--interview-traps)
40. [DSA & Problem-Solving](#40--dsa--problem-solving)
41. [Top 20 Interview Questions](#41--top-20-interview-questions)
42. [30-Second Interview Answer](#42--30-second-interview-answer)
43. [Cheat Sheet](#43--cheat-sheet)
44. [Memory Tricks](#44--memory-tricks)
45. [Next Topic](#45--next-topic)

---

# 1. 🧵 What is StringBuffer?

`StringBuffer` is a class in Java that represents a:

> **Mutable sequence of characters.**

The two most important properties are:

- **Mutable**
- **Synchronized**

Example:

```java
StringBuffer sb = new StringBuffer("Java");

sb.append(" Programming");

System.out.println(sb);
```

Output:

```text
Java Programming
```

The existing `StringBuffer` object is modified.

---

# 2. 🤔 Why Do We Need StringBuffer?

Java provides three important choices for text:

```text
String
   ↓
Immutable

StringBuilder
   ↓
Mutable + Unsynchronized

StringBuffer
   ↓
Mutable + Synchronized
```

`String` is immutable.

`StringBuilder` is mutable but does not synchronize its methods.

`StringBuffer` is mutable and its methods are synchronized.

Therefore, `StringBuffer` can be useful when:

- Text needs frequent modification.
- The mutable buffer is shared between threads.
- Synchronized method-level access is appropriate.

> **Important:** Synchronization of individual methods does not automatically make a multi-step sequence of operations atomic.

---

# 3. 🏗️ StringBuffer as a Class

`StringBuffer` is a class.

Its inheritance relationship is conceptually:

```text
Object
   ↓
AbstractStringBuilder
   ├── StringBuilder
   └── StringBuffer
```

Both `StringBuilder` and `StringBuffer` provide mutable character sequences.

Their major difference is synchronization.

```text
StringBuilder
    ↓
Mutable
    ↓
Not synchronized

StringBuffer
    ↓
Mutable
    ↓
Synchronized
```

---

# 4. 📦 Package

`StringBuffer` belongs to:

```text
java.lang
```

Therefore, no explicit import is required.

Example:

```java
StringBuffer sb = new StringBuffer();
```

---

# 5. 🆕 Creating StringBuffer

Common constructors include:

```java
new StringBuffer()
new StringBuffer(String str)
new StringBuffer(int capacity)
```

Example:

```java
StringBuffer sb1 = new StringBuffer();

StringBuffer sb2 = new StringBuffer("Java");

StringBuffer sb3 = new StringBuffer(100);
```

---

# 6. 🔹 Default Constructor

Example:

```java
StringBuffer sb = new StringBuffer();
```

The default initial capacity is:

```text
16
```

Initially:

```text
length   = 0
capacity = 16
```

Therefore:

> **Capacity and length are different concepts.**

`length()` tells us how many characters are currently stored.

`capacity()` tells us the current internal capacity.

---

# 7. 🔹 Constructor with String

Example:

```java
StringBuffer sb = new StringBuffer("Java");
```

The string contains:

```text
Java
```

Length:

```text
4
```

Initial capacity:

```text
4 + 16 = 20
```

Therefore:

```text
length   = 4
capacity = 20
```

### Formula

```text
Initial capacity = string.length() + 16
```

Example:

```java
StringBuffer sb = new StringBuffer("Hello");
System.out.println(sb.length());
System.out.println(sb.capacity());
```

Output:

```text
5
21
```

---

# 8. 🔹 Constructor with Capacity

You can specify the initial capacity manually.

Example:

```java
StringBuffer sb = new StringBuffer(100);
```

Initially:

```text
length   = 0
capacity = 100
```

This is useful when you already have an approximate idea of how much text will be stored.

---

# 9. 🔄 Mutability

StringBuffer is mutable.

Example:

```java
StringBuffer sb = new StringBuffer("Java");

sb.append(" Programming");

System.out.println(sb);
```

Output:

```text
Java Programming
```

The existing object is modified.

### Contrast with String

```java
String s = "Java";

s.concat(" Programming");

System.out.println(s);
```

Output:

```text
Java
```

Why?

Because:

```text
String       → Immutable
StringBuffer → Mutable
```

---

# 10. ⚙️ Internal Working

StringBuffer maintains an expandable mutable character sequence internally.

Conceptually:

```text
StringBuffer
      ↓
Mutable internal storage
      ↓
J a v a _ _ _ _ _ ...
```

When characters are appended:

```java
StringBuffer sb = new StringBuffer("Java");

sb.append(" Programming");
```

The existing character sequence is modified.

If the current capacity is insufficient, StringBuffer grows its internal storage.

### Important

The exact internal representation can vary between JDK implementations and versions.

For interview purposes:

> **StringBuffer maintains mutable character storage and synchronizes its methods.**

---

# 11. 📦 Capacity

Capacity represents the current amount of internal storage available before additional growth is required.

Example:

```java
StringBuffer sb = new StringBuffer();

System.out.println(sb.length());
System.out.println(sb.capacity());
```

Output:

```text
0
16
```

After:

```java
sb.append("Java");
```

The values are conceptually:

```text
length   = 4
capacity = 16
```

Therefore:

> **Capacity is not the same as length.**

---

# 12. 📏 length()

`length()` returns the number of characters currently stored.

Example:

```java
StringBuffer sb = new StringBuffer("Java");

System.out.println(sb.length());
```

Output:

```text
4
```

Remember:

```text
length()
    ↓
Number of characters currently stored

capacity()
    ↓
Current internal storage capacity
```

---

# 13. ➕ append()

`append()` adds data to the end of the StringBuffer.

Example:

```java
StringBuffer sb = new StringBuffer("Java");

sb.append(" Programming");

System.out.println(sb);
```

Output:

```text
Java Programming
```

### Append Integer

```java
StringBuffer sb = new StringBuffer("Age: ");

sb.append(22);

System.out.println(sb);
```

Output:

```text
Age: 22
```

### Append Character

```java
StringBuffer sb = new StringBuffer();

sb.append('A');

System.out.println(sb);
```

### Append Boolean

```java
StringBuffer sb = new StringBuffer();

sb.append(true);

System.out.println(sb);
```

### Append Multiple Values

```java
StringBuffer sb = new StringBuffer();

sb.append("Name: ")
  .append("Divyansh")
  .append(", Age: ")
  .append(22);

System.out.println(sb);
```

`append()` is overloaded for many data types.

---

# 14. ➕ insert()

`insert()` inserts data at a specified index.

Example:

```java
StringBuffer sb = new StringBuffer("Jav");

sb.insert(3, 'a');

System.out.println(sb);
```

Output:

```text
Java
```

Another example:

```java
StringBuffer sb = new StringBuffer("Java");

sb.insert(4, " Programming");

System.out.println(sb);
```

Output:

```text
Java Programming
```

### Index Visualization

```text
J a v a
0 1 2 3

Insert at index 4:

J a v a | Programming
```

The insertion shifts the existing characters at and after that position to the right.

---

# 15. 🔄 replace()

`replace()` replaces characters within a specified range.

Syntax:

```java
replace(start, end, str)
```

Example:

```java
StringBuffer sb = new StringBuffer("Java Programming");

sb.replace(0, 4, "Python");

System.out.println(sb);
```

Output:

```text
Python Programming
```

### Range Rule

```text
start → inclusive
end   → exclusive
```

Therefore:

```java
sb.replace(0, 4, "Python");
```

replaces indexes:

```text
0
1
2
3
```

but not index `4`.

---

# 16. 🗑️ delete()

`delete()` removes characters from a specified range.

Syntax:

```java
delete(start, end)
```

Example:

```java
StringBuffer sb = new StringBuffer("Java Programming");

sb.delete(4, 5);

System.out.println(sb);
```

Output:

```text
JavaProgramming
```

Index `4` contains the space.

The range is:

```text
start → inclusive
end   → exclusive
```

Therefore:

```java
sb.delete(4, 5);
```

deletes only index `4`.

---

# 17. 🗑️ deleteCharAt()

`deleteCharAt()` removes one character at a specified index.

Example:

```java
StringBuffer sb = new StringBuffer("Java");

sb.deleteCharAt(1);

System.out.println(sb);
```

Output:

```text
Jva
```

Indexes:

```text
J a v a
0 1 2 3
```

Index `1` contains:

```text
a
```

So that character is removed.

---

# 18. 🔄 reverse()

`reverse()` reverses the character sequence.

Example:

```java
StringBuffer sb = new StringBuffer("Java");

sb.reverse();

System.out.println(sb);
```

Output:

```text
avaJ
```

Another example:

```java
StringBuffer sb = new StringBuffer("12345");

sb.reverse();

System.out.println(sb);
```

Output:

```text
54321
```

### Important

`reverse()` modifies the existing StringBuffer.

---

# 19. 🔤 charAt()

`charAt()` returns the character at a specified index.

Example:

```java
StringBuffer sb = new StringBuffer("Java");

System.out.println(sb.charAt(2));
```

Output:

```text
v
```

Indexes:

```text
J a v a
0 1 2 3
```

Therefore:

```text
charAt(2) → 'v'
```

---

# 20. ✏️ setCharAt()

`setCharAt()` replaces the character at a specified index.

Example:

```java
StringBuffer sb = new StringBuffer("Java");

sb.setCharAt(0, 'K');

System.out.println(sb);
```

Output:

```text
Kava
```

Before:

```text
Java
```

After:

```text
Kava
```

### Important Difference

```text
charAt()
    ↓
Read a character

setCharAt()
    ↓
Modify a character
```

---

# 21. ✂️ substring()

`substring()` extracts a portion of the character sequence.

Example:

```java
StringBuffer sb = new StringBuffer("Java Programming");

String result = sb.substring(5);

System.out.println(result);
```

Output:

```text
Programming
```

### Very Important

`substring()` returns:

```text
String
```

not:

```text
StringBuffer
```

Example:

```java
StringBuffer sb = new StringBuffer("Java Programming");

String result = sb.substring(0, 4);

System.out.println(result);
```

Output:

```text
Java
```

---

# 22. 🔎 indexOf()

`indexOf()` returns the index of the first occurrence of a specified string.

Example:

```java
StringBuffer sb = new StringBuffer("Java Programming");

System.out.println(sb.indexOf("Programming"));
```

Output:

```text
5
```

If the string is not found:

```text
-1
```

Example:

```java
StringBuffer sb = new StringBuffer("Java");

System.out.println(sb.indexOf("Python"));
```

Output:

```text
-1
```

---

# 23. 🔍 lastIndexOf()

`lastIndexOf()` returns the index of the last occurrence.

Example:

```java
StringBuffer sb = new StringBuffer("Java Java");

System.out.println(sb.lastIndexOf("Java"));
```

Output:

```text
5
```

The occurrences start at:

```text
0
5
```

Therefore the last occurrence starts at index `5`.

---

# 24. 📏 setLength()

`setLength()` changes the logical length of the StringBuffer.

Example:

```java
StringBuffer sb = new StringBuffer("Java");

sb.setLength(2);

System.out.println(sb);
```

Output:

```text
Ja
```

### Increasing Length

You can also increase the logical length.

```java
StringBuffer sb = new StringBuffer("Java");

sb.setLength(6);

System.out.println(sb.length());
```

Output:

```text
6
```

The additional positions contain the null character:

```text
'\u0000'
```

### Important

`setLength()` changes the logical length.

It does not necessarily make the capacity equal to the new length.

---

# 25. 📦 ensureCapacity()

`ensureCapacity()` ensures that the capacity is at least the requested amount.

Example:

```java
StringBuffer sb = new StringBuffer();

sb.ensureCapacity(100);

System.out.println(sb.capacity());
```

The capacity will be sufficient for at least `100` characters.

### Why Use It?

If you know approximately how much data will be stored, preallocating capacity can reduce repeated resizing.

Example:

```java
StringBuffer sb = new StringBuffer();

sb.ensureCapacity(1000);

for (int i = 0; i < 1000; i++) {
    sb.append(i);
}
```

---

# 26. ✂️ trimToSize()

`trimToSize()` attempts to reduce the capacity so that unused internal storage is minimized.

Example:

```java
StringBuffer sb = new StringBuffer(100);

sb.append("Java");

System.out.println(sb.capacity());

sb.trimToSize();

System.out.println(sb.capacity());
```

After trimming, the capacity may be reduced to approximately the current length.

### Important

`trimToSize()` affects capacity, not the logical content.

---

# 27. 🔤 toString()

`toString()` converts the StringBuffer content into a `String`.

Example:

```java
StringBuffer sb = new StringBuffer("Java");

String result = sb.toString();

System.out.println(result);
```

Now:

```text
sb     → StringBuffer
result → String
```

This is commonly used when string construction is complete.

Typical pattern:

```java
StringBuffer sb = new StringBuffer();

sb.append("Java")
  .append(" ")
  .append("Developer");

String result = sb.toString();
```

---

# 28. 🔗 Method Chaining

Many StringBuffer modification methods return the same StringBuffer object.

Therefore, chaining is possible.

Example:

```java
StringBuffer sb = new StringBuffer();

sb.append("Java")
  .append(" ")
  .append("Programming");

System.out.println(sb);
```

Output:

```text
Java Programming
```

Another example:

```java
StringBuffer sb = new StringBuffer("Java");

sb.append(" Programming")
  .reverse();

System.out.println(sb);
```

Output:

```text
gnimmargorP avaJ
```

### Why Does Chaining Work?

Conceptually:

```text
sb.append(...)
       ↓
   same sb
       ↓
   .append(...)
       ↓
   same sb
```

---

# 29. 🔒 Synchronization

This is the major difference between StringBuffer and StringBuilder.

StringBuffer's methods are synchronized.

Conceptually:

```text
Thread 1
   ↓
StringBuffer
   ↓
acquire monitor
   ↓
execute synchronized method
   ↓
release monitor
```

Another thread attempting to enter a synchronized method on the same object may need to wait until the monitor is available.

Example:

```java
StringBuffer sb = new StringBuffer();

sb.append("Hello");
```

The synchronization is handled by the class's synchronized methods.

### Important

Synchronization provides protection for individual synchronized operations.

It does **not** automatically make an entire group of operations atomic.

---

# 30. 🧵 Thread Safety

StringBuffer is commonly described as thread-safe because its methods are synchronized.

For example:

```java
StringBuffer sb = new StringBuffer();

Thread t1 = new Thread(() -> {
    sb.append("Hello");
});

Thread t2 = new Thread(() -> {
    sb.append("World");
});

t1.start();
t2.start();
```

Individual `append()` calls are synchronized.

However, consider a compound operation:

```java
if (sb.length() > 0) {
    sb.deleteCharAt(0);
}
```

Even though `length()` and `deleteCharAt()` are individually synchronized, the entire sequence is not automatically atomic.

Another thread can potentially modify the buffer between the two calls.

### Key Interview Point

> **Thread-safe individual methods ≠ automatically atomic multi-method operation.**

For compound operations that must be atomic, additional synchronization or another concurrency design may be required.

---

# 31. 📈 Capacity Growth

When the current capacity is insufficient, StringBuffer automatically expands its internal storage.

The commonly documented growth calculation is approximately:

```text
newCapacity = oldCapacity × 2 + 2
```

For example:

```text
old capacity = 16

new capacity
= 16 × 2 + 2
= 34
```

If the requested minimum capacity is larger than that calculated value, the implementation ensures enough capacity for the required content.

### Important

Do not treat the exact growth strategy as a permanent implementation guarantee for every future JDK.

For interviews, remember:

> **StringBuffer automatically grows its capacity when required.**

---

# 32. ⏱️ Time Complexity

The complexity depends on the operation and whether internal resizing or character shifting is required.

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

### Why is append() Amortized O(1)?

Most append operations do not require resizing.

Occasionally, capacity becomes insufficient:

```text
append
  ↓
capacity insufficient
  ↓
grow storage
  ↓
copy/move existing characters
  ↓
append new data
```

That particular operation can cost O(n).

But over many append operations, the average cost is amortized O(1).

---

# 33. 🆚 StringBuffer vs String

| Feature | String | StringBuffer |
|---|---|---|
| Mutable | ❌ | ✅ |
| Immutable | ✅ | ❌ |
| Modification | New String result | Modifies mutable buffer |
| `append()` | ❌ | ✅ |
| `reverse()` | ❌ | ✅ |
| `setCharAt()` | ❌ | ✅ |
| Repeated modification | Less suitable | Suitable |
| Synchronization | Not needed due to immutability | Methods synchronized |
| Main purpose | Immutable text | Mutable synchronized text |

### Simple Rule

```text
String
   ↓
Immutable text

StringBuffer
   ↓
Mutable + Synchronized
```

---

# 34. 🆚 StringBuffer vs StringBuilder

This is one of the most frequently asked Java interview questions.

| Feature | StringBuilder | StringBuffer |
|---|---|---|
| Mutable | ✅ | ✅ |
| Synchronized | ❌ | ✅ |
| Individual methods synchronized | ❌ | ✅ |
| General single-threaded performance | Generally faster | Generally slower |
| Introduced | Java 5 | Java 1.0 |
| Use case | General mutable string building | Synchronized mutable operations |

### Easy Memory Trick

```text
StringBuilder
      ↓
Build
      ↓
Mutable
      ↓
No synchronization

StringBuffer
      ↓
Buffer
      ↓
Mutable
      ↓
Synchronization
```

---

# 35. ⚡ Performance

StringBuffer generally has additional synchronization overhead compared with StringBuilder.

Therefore:

```text
No synchronization requirement
        ↓
StringBuilder
```

When synchronized mutable operations on the same buffer are specifically required:

```text
Synchronization required
        ↓
StringBuffer
```

### Important

Do not say:

> StringBuffer is always slow.

A better statement is:

> **StringBuffer generally has more synchronization overhead than StringBuilder.**

Actual performance depends on workload, JVM, contention, and implementation details.

---

# 36. ✅ Advantages

## 1. Mutable

The character sequence can be modified without creating a new immutable String result for every modification.

## 2. Synchronized

Its methods are synchronized.

## 3. Useful for Shared Mutable Data

It can be useful when synchronized access to a shared mutable character sequence is required.

## 4. Rich API

It supports operations such as:

```text
append()
insert()
delete()
replace()
reverse()
charAt()
setCharAt()
substring()
indexOf()
lastIndexOf()
```

## 5. Resizable

Its capacity automatically grows when required.

---

# 37. ❌ Disadvantages

## 1. Synchronization Overhead

Synchronization can add overhead compared with StringBuilder.

## 2. Usually Unnecessary for Single-Threaded Code

If synchronization is not required, StringBuilder is generally the more appropriate mutable builder.

## 3. Compound Operations Need Care

Synchronized individual methods do not automatically make a multi-step operation atomic.

## 4. Shared Mutable State

When multiple threads share the same mutable object, the overall concurrency design still matters.

---

# 38. ⚠️ Common Mistakes

## ❌ Mistake 1 — Thinking StringBuffer is immutable

Wrong:

```text
StringBuffer is immutable.
```

Correct:

```text
StringBuffer is mutable.
```

---

## ❌ Mistake 2 — Thinking StringBuilder is synchronized

Wrong:

```text
StringBuilder is thread-safe because it is mutable.
```

Correct:

```text
StringBuilder is not synchronized.
```

---

## ❌ Mistake 3 — Thinking StringBuffer is always faster

Wrong:

```text
StringBuffer is faster because it supports threads.
```

Correct:

```text
StringBuffer generally has synchronization overhead.
```

---

## ❌ Mistake 4 — Confusing length and capacity

Remember:

```text
length()
    ↓
Current number of characters

capacity()
    ↓
Current internal capacity
```

---

## ❌ Mistake 5 — Thinking substring() returns StringBuffer

It returns:

```text
String
```

---

## ❌ Mistake 6 — Thinking every sequence of methods is atomic

Individual methods are synchronized, but a sequence of multiple method calls is not automatically one atomic operation.

---

# 39. 🚨 Interview Traps

## Trap 1

```java
StringBuffer sb = new StringBuffer("Java");

sb.append(" Programming");

System.out.println(sb);
```

Output:

```text
Java Programming
```

---

## Trap 2

```java
StringBuffer sb = new StringBuffer("Java");

sb.reverse();

System.out.println(sb);
```

Output:

```text
avaJ
```

---

## Trap 3

```java
StringBuffer sb = new StringBuffer();

System.out.println(sb.length());
System.out.println(sb.capacity());
```

Output:

```text
0
16
```

---

## Trap 4

```java
StringBuffer sb = new StringBuffer("Java");

sb.setCharAt(0, 'K');

System.out.println(sb);
```

Output:

```text
Kava
```

---

## Trap 5

```java
StringBuffer sb = new StringBuffer("Java");

String s = sb.substring(1, 3);

System.out.println(s);
```

Output:

```text
av
```

Type of `s`:

```text
String
```

---

## Trap 6

```java
StringBuffer sb = new StringBuffer("Java");

sb.delete(1, 3);

System.out.println(sb);
```

Output:

```text
Ja
```

Why?

```text
J a v a
0 1 2 3
```

Indexes `1` and `2` are removed.

Index `3` is exclusive.

---

## Trap 7

```java
StringBuffer sb = new StringBuffer();

sb.append("Java")
  .append(" ")
  .append("Developer");

System.out.println(sb);
```

Output:

```text
Java Developer
```

---

## Trap 8

Which is generally preferred when synchronization is unnecessary?

```text
StringBuilder
```

Not:

```text
StringBuffer
```

---

# 40. 🧠 DSA & Problem-Solving

StringBuffer is useful in several string-manipulation problems, although in modern Java DSA solutions `StringBuilder` is usually preferred unless synchronization is specifically needed.

## 🔥 DSA Pattern 1 — Reverse a String

### Problem

Reverse a string.

Example:

```text
Input:  "hello"
Output: "olleh"
```

### StringBuffer Solution

```java
StringBuffer sb = new StringBuffer("hello");

sb.reverse();

System.out.println(sb);
```

Output:

```text
olleh
```

### Complexity

```text
Time  → O(n)
Space → O(n)
```

---

## 🔥 DSA Pattern 2 — Build a String Efficiently

### Problem

Create a string containing numbers from `1` to `n`.

```java
int n = 5;

StringBuffer sb = new StringBuffer();

for (int i = 1; i <= n; i++) {
    sb.append(i);
}

System.out.println(sb);
```

Output:

```text
12345
```

### Pattern

```text
Repeated string modification
        ↓
Mutable builder
        ↓
append()
```

### Complexity

```text
Time  → Amortized O(n)
Space → O(n)
```

---

## 🔥 DSA Pattern 3 — Remove Characters

Suppose we want to remove all occurrences of a particular character.

```java
StringBuffer sb = new StringBuffer("banana");

for (int i = sb.length() - 1; i >= 0; i--) {
    if (sb.charAt(i) == 'a') {
        sb.deleteCharAt(i);
    }
}

System.out.println(sb);
```

Output:

```text
bnn
```

### Why Traverse Backward?

When deleting from a mutable sequence, deleting from right to left avoids invalidating the indexes of characters that still need to be processed on the left.

### Problem-Solving Pattern

```text
Mutation while traversing
        ↓
Delete from right to left
```

---

## 🔥 DSA Pattern 4 — Palindrome Check

A palindrome reads the same forward and backward.

Example:

```text
madam
```

### Using StringBuffer

```java
String str = "madam";

StringBuffer sb = new StringBuffer(str);

String reversed = sb.reverse().toString();

if (str.equals(reversed)) {
    System.out.println("Palindrome");
} else {
    System.out.println("Not Palindrome");
}
```

Output:

```text
Palindrome
```

### Complexity

```text
Time  → O(n)
Space → O(n)
```

### Interview Note

For a memory-efficient palindrome check, a two-pointer approach can avoid creating a reversed copy:

```java
String str = "madam";

int left = 0;
int right = str.length() - 1;

boolean palindrome = true;

while (left < right) {
    if (str.charAt(left) != str.charAt(right)) {
        palindrome = false;
        break;
    }

    left++;
    right--;
}

System.out.println(palindrome);
```

This demonstrates an important DSA lesson:

> **Do not automatically use a mutable string builder when a two-pointer solution can solve the problem with O(1) auxiliary space.**

---

## 🔥 DSA Pattern 5 — Remove Adjacent Duplicates

Example:

```text
Input:  "abbaca"
Output: "ca"
```

A mutable builder can act like a stack.

```java
String str = "abbaca";

StringBuilder sb = new StringBuilder();

for (char ch : str.toCharArray()) {

    int n = sb.length();

    if (n > 0 && sb.charAt(n - 1) == ch) {
        sb.deleteCharAt(n - 1);
    } else {
        sb.append(ch);
    }
}

System.out.println(sb);
```

Output:

```text
ca
```

### DSA Insight

The builder represents the current stack:

```text
append()
   ↓
push

deleteCharAt(last index)
   ↓
pop
```

This is a very useful string + stack pattern.

---

## 🔥 DSA Pattern 6 — Reverse Words

Example:

```text
Input:
"Java is powerful"

Output:
"powerful is Java"
```

A simple approach is:

```java
String str = "Java is powerful";

String[] words = str.split(" ");

StringBuilder sb = new StringBuilder();

for (int i = words.length - 1; i >= 0; i--) {

    sb.append(words[i]);

    if (i != 0) {
        sb.append(" ");
    }
}

System.out.println(sb);
```

Output:

```text
powerful is Java
```

### DSA Pattern

```text
Input string
     ↓
Split into words
     ↓
Traverse backward
     ↓
Build answer using StringBuilder
```

---

## 🎯 DSA Problem-Solving Checklist

When solving a string problem, ask:

```text
1. Do I need to modify characters?
        ↓
       Yes
        ↓
   Mutable builder may help

2. Am I repeatedly appending?
        ↓
       Yes
        ↓
   Use StringBuilder

3. Am I deleting while traversing?
        ↓
       Yes
        ↓
   Consider traversing backward

4. Can two pointers solve it?
        ↓
       Yes
        ↓
   Check whether O(1) extra space is possible

5. Do I need a stack-like structure?
        ↓
       Yes
        ↓
   StringBuilder can sometimes model a stack

6. Do I actually need thread synchronization?
        ↓
       Yes
        ↓
   StringBuffer may be relevant
```

### Important DSA Note

For competitive programming and typical interview DSA in Java:

```text
StringBuilder
      ↓
Usually preferred
```

StringBuffer is generally not chosen merely because it is mutable.

Use StringBuffer when its synchronization characteristics are actually relevant.

---

# 41. 🔥 Top 20 Interview Questions

## Q1. What is StringBuffer?

**Answer:**

`StringBuffer` is a mutable sequence of characters whose methods are synchronized.

---

## Q2. Is StringBuffer mutable?

**Answer:**

Yes.

Its character sequence can be modified after creation.

---

## Q3. Where is StringBuffer located?

**Answer:**

It belongs to:

```text
java.lang
```

No explicit import is required.

---

## Q4. Is StringBuffer thread-safe?

**Answer:**

Its methods are synchronized, providing thread-safe individual method operations on the same StringBuffer instance.

---

## Q5. Why is StringBuffer synchronized?

**Answer:**

Synchronization coordinates access when multiple threads operate on the same mutable buffer.

---

## Q6. What is the default capacity of StringBuffer?

**Answer:**

The default initial capacity is:

```text
16
```

---

## Q7. What is the initial capacity of `new StringBuffer("Java")`?

**Answer:**

```text
4 + 16 = 20
```

---

## Q8. Difference between length() and capacity()?

**Answer:**

`length()` gives the number of characters currently stored.

`capacity()` gives the current internal capacity.

---

## Q9. Is StringBuffer mutable?

**Answer:**

Yes.

Methods such as `append()`, `insert()`, `delete()`, and `reverse()` modify its character sequence.

---

## Q10. What does append() do?

**Answer:**

It adds data to the end of the StringBuffer.

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

It converts the StringBuffer content into a String.

---

## Q14. StringBuffer vs StringBuilder?

**Answer:**

Both are mutable character sequences.

StringBuffer synchronizes its methods, while StringBuilder does not.

---

## Q15. Which is generally faster: StringBuilder or StringBuffer?

**Answer:**

StringBuilder is generally faster when synchronization is unnecessary because StringBuffer has synchronization overhead.

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

Length represents the number of characters currently stored.

Capacity represents the current internal storage capacity.

---

## Q19. Does StringBuffer guarantee that a multi-method operation is atomic?

**Answer:**

No.

Individual methods are synchronized, but a sequence of multiple method calls is not automatically one atomic operation.

---

## Q20. When should you use StringBuffer?

**Answer:**

Use StringBuffer when you need mutable string manipulation and synchronized access to the same buffer is specifically required.

---

# 42. 🎤 30-Second Interview Answer

> **StringBuffer is a mutable sequence of characters provided by the `java.lang` package. Unlike String, which is immutable, StringBuffer allows operations such as append, insert, delete, replace, and reverse to modify the same mutable object. Its methods are synchronized, providing thread-safe individual operations on the buffer. Because synchronization adds overhead, StringBuilder is generally preferred when synchronization is not required.**

---

# 43. 🧾 Cheat Sheet

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

# 44. 🧠 Memory Tricks

## 🔥 The Three String Classes

Remember:

```text
String
   ↓
Immutable

StringBuilder
   ↓
Mutable + Unsynchronized

StringBuffer
   ↓
Mutable + Synchronized
```

---

## 🔥 Builder vs Buffer

Think:

```text
Builder
   ↓
Build
   ↓
Mutable building
   ↓
No synchronization

Buffer
   ↓
Shared mutable buffer
   ↓
Synchronization
```

---

## 🔥 Mutation Methods

Remember:

```text
A I R D R

A → append()
I → insert()
R → replace()
D → delete()
R → reverse()
```

---

## 🔥 Character Methods

```text
charAt()
    ↓
Read

setCharAt()
    ↓
Modify
```

---

## 🔥 Capacity

Remember:

```text
Length ≠ Capacity

length()
    ↓
Actual content

capacity()
    ↓
Internal storage capacity
```

---

## 🔥 Conversion

Remember:

```text
StringBuffer
      ↓
toString()
      ↓
String
```

---

# ⭐ Most Important Interview Points

Before moving ahead, make sure you understand:

```text
1. StringBuffer is mutable.

2. StringBuffer belongs to java.lang.

3. StringBuffer methods are synchronized.

4. StringBuffer represents a mutable character sequence.

5. Default capacity is 16.

6. String constructor capacity = length + 16.

7. length() and capacity() are different.

8. append() adds data at the end.

9. insert() adds data at an index.

10. delete() removes a range.

11. deleteCharAt() removes one character.

12. replace() replaces a range.

13. reverse() reverses the buffer.

14. charAt() reads a character.

15. setCharAt() changes a character.

16. substring() returns String.

17. toString() converts to String.

18. StringBuffer generally has more synchronization overhead than StringBuilder.

19. Individual synchronized methods do not make compound operations automatically atomic.

20. StringBuilder is generally preferred when synchronization is unnecessary.
```

---

# 45. 🔗 Next Topic

Our String playlist:

```text
04-Strings/
│
├── 01-String-Introduction.md
├── 02-String-Pool.md
├── 03-String-Immutability.md
├── 04-String-Methods.md
├── 05-StringBuilder.md
├── 06-StringBuffer.md              ← YOU ARE HERE
├── 07-String-vs-StringBuilder-vs-StringBuffer.md
└── 08-String-Interview-Questions.md
```

### Learning Flow

```text
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
```

---

# 🚀 Final Revision

Before moving to the comparison topic, you should be able to answer:

```text
❓ What is StringBuffer?

❓ Why is StringBuffer mutable?

❓ Why are StringBuffer methods synchronized?

❓ What is its default capacity?

❓ What is the difference between length and capacity?

❓ How does append() work?

❓ How does insert() work?

❓ How does delete() work?

❓ How does replace() work?

❓ How does reverse() work?

❓ What does setCharAt() do?

❓ What does substring() return?

❓ What does toString() return?

❓ Is StringBuffer thread-safe for individual method operations?

❓ Is every sequence of StringBuffer operations atomic?

❓ StringBuffer vs StringBuilder?

❓ StringBuffer vs String?

❓ Why can StringBuilder be faster?

❓ Which DSA problems can use a mutable string builder?

❓ When can a two-pointer approach use less space than reversing a string?
```

> ⭐ **Core Idea:**

> **StringBuffer = Mutable + Synchronized.**

> If you remember only one line for the interview:

> **String is immutable, StringBuilder is mutable and unsynchronized, while StringBuffer is mutable and its methods are synchronized.**