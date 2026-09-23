# ⚔️ String vs StringBuilder vs StringBuffer

> **`String` is immutable, while `StringBuilder` and `StringBuffer` are mutable. `StringBuilder` is generally preferred for mutable string manipulation when synchronization is not required, while `StringBuffer` provides synchronized methods for mutable string operations.**

---

# 📌 Table of Contents

1. [Introduction](#1--introduction)
2. [The Big Picture](#2--the-big-picture)
3. [String](#3--string)
4. [StringBuilder](#4--stringbuilder)
5. [StringBuffer](#5--stringbuffer)
6. [Mutability](#6--mutability)
7. [Thread Safety](#7--thread-safety)
8. [Synchronization](#8--synchronization)
9. [Performance](#9--performance)
10. [Memory Behavior](#10--memory-behavior)
11. [String Pool](#11--string-pool)
12. [Common Methods](#12--common-methods)
13. [append() Comparison](#13--append-comparison)
14. [Modification Comparison](#14--modification-comparison)
15. [Conversion Between Them](#15--conversion-between-them)
16. [When to Use String](#16--when-to-use-string)
17. [When to Use StringBuilder](#17--when-to-use-stringbuilder)
18. [When to Use StringBuffer](#18--when-to-use-stringbuffer)
19. [Real-World Examples](#19--real-world-examples)
20. [Performance Example](#20--performance-example)
21. [Decision Tree](#21--decision-tree)
22. [Detailed Comparison](#22--detailed-comparison)
23. [Advantages and Disadvantages](#23--advantages-and-disadvantages)
24. [Common Mistakes](#24--common-mistakes)
25. [Interview Traps](#25--interview-traps)
26. [Top 25 Interview Questions](#26--top-25-interview-questions)
27. [30-Second Interview Answer](#27--30-second-interview-answer)
28. [1-Minute Interview Answer](#28--1-minute-interview-answer)
29. [Cheat Sheet](#29--cheat-sheet)
30. [Memory Tricks](#30--memory-tricks)
31. [Final Revision](#31--final-revision)
32. [Next Topic](#32--next-topic)

---

# 1. 🔤 Introduction

Java provides three important classes for working with character data:

- `String`
- `StringBuilder`
- `StringBuffer`

All three can represent sequences of characters, but they are designed for different situations.

The biggest differences are:

```text
String
    ↓
Immutable

StringBuilder
    ↓
Mutable + Not Synchronized

StringBuffer
    ↓
Mutable + Synchronized
```

---

# 2. 🌳 The Big Picture

Think of them like this:

```text
┌────────────────────────────────────────────┐
│                TEXT DATA                   │
└────────────────────────────────────────────┘
                    │
         ┌──────────┼──────────┐
         ↓          ↓          ↓
      String   StringBuilder  StringBuffer
         │          │          │
     Immutable   Mutable      Mutable
                    │          │
               Not Sync.   Synchronized
```

## Easy Memory Trick

```text
String
    ↓
Can't modify the object

StringBuilder
    ↓
Can modify the object

StringBuffer
    ↓
Can modify + synchronized methods
```

---

# 3. 🧵 String

`String` is a class from:

```text
java.lang
```

Example:

```java
String name = "Java";
```

The most important property of String is:

> **String is immutable.**

Once a String object is created, its contents cannot be changed.

---

## Example

```java
String s = "Java";

s = s + " Programming";

System.out.println(s);
```

Output:

```text
Java Programming
```

It may look as if `"Java"` was modified.

But that is not what happened.

Conceptually:

```text
Before:

s
│
↓
"Java"
```

After:

```text
s
│
↓
"Java Programming"
```

A new String object is created for the concatenated result.

The original `"Java"` object remains unchanged.

---

# 4. 🏗️ StringBuilder

`StringBuilder` is a mutable sequence of characters.

It is also part of:

```text
java.lang
```

Example:

```java
StringBuilder sb = new StringBuilder("Java");

sb.append(" Programming");

System.out.println(sb);
```

Output:

```text
Java Programming
```

Here the same StringBuilder object is modified.

---

## Why StringBuilder?

Suppose we repeatedly modify text:

```text
append()
append()
append()
append()
```

Using String can create multiple intermediate String objects.

StringBuilder is designed for this type of repeated modification.

---

## Example

```java
StringBuilder sb = new StringBuilder();

sb.append("Java");
sb.append(" ");
sb.append("Backend");
sb.append(" ");
sb.append("Developer");

System.out.println(sb);
```

Output:

```text
Java Backend Developer
```

---

# 5. 🧵 StringBuffer

`StringBuffer` is also a mutable sequence of characters.

It belongs to:

```text
java.lang
```

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

The major difference from StringBuilder is:

> **StringBuffer's methods are synchronized.**

Therefore:

```text
StringBuffer
    ↓
Mutable
    +
Synchronized
```

---

# 6. 🔄 Mutability

## What does mutable mean?

Mutable means:

> The object's contents can be changed after the object has been created.

---

## String

```java
String s = "Java";
```

String is:

```text
Immutable
```

Its existing object's content cannot be modified.

---

## StringBuilder

```java
StringBuilder sb = new StringBuilder("Java");

sb.append(" Developer");
```

The same object can be modified.

Therefore:

```text
StringBuilder → Mutable
```

---

## StringBuffer

```java
StringBuffer sb = new StringBuffer("Java");

sb.append(" Developer");
```

The same object can be modified.

Therefore:

```text
StringBuffer → Mutable
```

---

## Summary

```text
String
    → Immutable

StringBuilder
    → Mutable

StringBuffer
    → Mutable
```

---

# 7. 🧵 Thread Safety

Thread safety becomes important when multiple threads access shared mutable data.

---

## String

String is immutable.

Therefore, once a String object exists, its character contents cannot be changed.

This makes immutable String objects naturally safe to share in many situations.

---

## StringBuilder

StringBuilder is:

```text
Not synchronized
```

Therefore, it should not be treated as thread-safe for concurrent modification of the same instance.

It is generally intended for situations where synchronization is not required.

---

## StringBuffer

StringBuffer methods are synchronized.

Therefore, individual operations on a shared StringBuffer instance are synchronized.

---

## Simple Comparison

```text
String
    → Immutable

StringBuilder
    → Mutable
    → Not synchronized

StringBuffer
    → Mutable
    → Synchronized
```

---

# 8. 🔒 Synchronization

Synchronization controls access to shared mutable state between threads.

StringBuffer provides synchronized methods.

Conceptually:

```text
Thread 1
    │
    ↓
┌──────────────┐
│ StringBuffer │
│      🔒      │
└──────────────┘
    ↑
    │
Thread 2
```

---

## Important Interview Point

Do NOT say:

> "StringBuffer makes every multi-step operation atomic."

That is incorrect.

For example:

```java
if (sb.length() > 0) {
    sb.deleteCharAt(0);
}
```

The two method calls together are not automatically one atomic operation.

Synchronization of individual methods does not automatically make an entire sequence of operations atomic.

---

# 9. ⚡ Performance

In general:

```text
StringBuilder
    ↓
Faster than StringBuffer
```

Why?

Because StringBuffer synchronizes its methods.

Synchronization introduces additional overhead.

Therefore:

```text
Single-threaded / synchronization not required
            ↓
      StringBuilder

Synchronized mutable operations required
            ↓
       StringBuffer
```

---

## Important

Do not say:

> "StringBuilder is always faster."

Better interview answer:

> StringBuilder generally has less overhead than StringBuffer because it does not synchronize its methods.

Actual performance depends on the workload and implementation.

---

# 10. 🧠 Memory Behavior

## String

Repeated modification can create new String objects.

Example:

```java
String s = "";

s = s + "Java";
s = s + " ";
s = s + "Developer";
```

Conceptually, multiple String objects can be involved.

---

## StringBuilder

A mutable internal buffer is used.

Example:

```java
StringBuilder sb = new StringBuilder();

sb.append("Java");
sb.append(" Developer");
```

The existing builder can grow and modify its internal character storage.

---

## StringBuffer

StringBuffer also maintains mutable internal storage.

The difference is that its methods are synchronized.

---

# 11. 🏊 String Pool

String literals can be stored in the String Pool.

Example:

```java
String s1 = "Java";
String s2 = "Java";
```

Both can refer to the same pooled String object.

Conceptually:

```text
s1 ─────┐
        ↓
      "Java"
    String Pool
        ↑
s2 ─────┘
```

---

## StringBuilder and StringBuffer

They are objects created using constructors.

Example:

```java
StringBuilder builder = new StringBuilder("Java");
StringBuffer buffer = new StringBuffer("Java");
```

The String argument `"Java"` may come from the String Pool, but the StringBuilder/StringBuffer object itself is a separate mutable object.

---

# 12. 🛠️ Common Methods

StringBuilder and StringBuffer provide many similar methods.

Important ones include:

- `append()`
- `insert()`
- `replace()`
- `delete()`
- `deleteCharAt()`
- `reverse()`
- `charAt()`
- `setCharAt()`
- `length()`
- `capacity()`
- `substring()`
- `indexOf()`
- `lastIndexOf()`
- `setLength()`
- `ensureCapacity()`
- `trimToSize()`
- `toString()`

---

## String

Important String methods include:

- `length()`
- `charAt()`
- `substring()`
- `indexOf()`
- `lastIndexOf()`
- `equals()`
- `equalsIgnoreCase()`
- `startsWith()`
- `endsWith()`
- `contains()`
- `replace()`
- `replaceAll()`
- `split()`
- `trim()`
- `strip()`
- `toLowerCase()`
- `toUpperCase()`

But String modification methods do not modify the original String.

They return a new String when a changed result is required.

---

# 13. ➕ append() Comparison

## String

String does not have an `append()` method.

You can concatenate:

```java
String s = "Java";

s = s + " Developer";
```

---

## StringBuilder

```java
StringBuilder sb = new StringBuilder("Java");

sb.append(" Developer");
```

---

## StringBuffer

```java
StringBuffer sb = new StringBuffer("Java");

sb.append(" Developer");
```

---

## Comparison

```text
String
    → + or concat()

StringBuilder
    → append()

StringBuffer
    → append()
```

---

# 14. ✏️ Modification Comparison

Suppose we want:

```text
Java
```

to become:

```text
Kava
```

---

## String

```java
String s = "Java";

s = "Kava";
```

A new String value is assigned.

The original String object was not modified.

---

## StringBuilder

```java
StringBuilder sb = new StringBuilder("Java");

sb.setCharAt(0, 'K');
```

Result:

```text
Kava
```

The same mutable object is modified.

---

## StringBuffer

```java
StringBuffer sb = new StringBuffer("Java");

sb.setCharAt(0, 'K');
```

Result:

```text
Kava
```

Again, the mutable object is modified.

---

# 15. 🔄 Conversion Between Them

## String → StringBuilder

```java
String s = "Java";

StringBuilder sb = new StringBuilder(s);
```

---

## String → StringBuffer

```java
String s = "Java";

StringBuffer sb = new StringBuffer(s);
```

---

## StringBuilder → String

```java
StringBuilder sb = new StringBuilder("Java");

String s = sb.toString();
```

---

## StringBuffer → String

```java
StringBuffer sb = new StringBuffer("Java");

String s = sb.toString();
```

---

## StringBuilder → StringBuffer

There is no direct conversion method specifically required.

You can use:

```java
StringBuilder builder = new StringBuilder("Java");

StringBuffer buffer = new StringBuffer(builder.toString());
```

---

## StringBuffer → StringBuilder

Similarly:

```java
StringBuffer buffer = new StringBuffer("Java");

StringBuilder builder = new StringBuilder(buffer.toString());
```

---

# 16. 🎯 When to Use String

Use String when:

- The text does not need frequent modification.
- Immutability is desirable.
- You are representing fixed textual data.
- You want String-specific functionality.
- You want to safely share immutable text.

Examples:

```java
String name = "Divyansh";
String email = "user@example.com";
String country = "India";
```

---

## Typical Examples

```text
User's name
Email address
Country
Status
URL
Configuration value
Constant text
```

---

# 17. 🏗️ When to Use StringBuilder

Use StringBuilder when:

- You need frequent modifications.
- You are building text in loops.
- Synchronization is not required.
- You want efficient mutable string construction.

Example:

```java
StringBuilder sb = new StringBuilder();

for (int i = 1; i <= 5; i++) {
    sb.append(i);
    sb.append(" ");
}

System.out.println(sb);
```

Output:

```text
1 2 3 4 5
```

---

## Very Common Use Case

Generating a large String:

```java
StringBuilder result = new StringBuilder();

for (int i = 0; i < 1000; i++) {
    result.append(i);
}

String output = result.toString();
```

This is a common pattern.

---

# 18. 🛡️ When to Use StringBuffer

Use StringBuffer when:

- You need mutable text.
- The same buffer may be accessed by multiple threads.
- Synchronized individual operations are appropriate.

Example:

```java
StringBuffer buffer = new StringBuffer();

buffer.append("Data");
```

The key reason to choose it over StringBuilder is synchronization.

---

## Important Modern Perspective

Do not automatically use StringBuffer just because an application uses multiple threads.

First determine:

- Is the same mutable buffer actually shared?
- Do operations need synchronization?
- Is external synchronization already being used?
- Would another concurrency design be better?

---

# 19. 🌍 Real-World Examples

## Example 1 — User Name

```java
String name = "Divyansh";
```

Best fit:

```text
String
```

Because a name is generally treated as a value rather than something repeatedly modified character-by-character.

---

## Example 2 — Building a Large Message

```java
StringBuilder message = new StringBuilder();

message.append("Hello ");
message.append("Divyansh");
message.append("!");
```

Best fit:

```text
StringBuilder
```

---

## Example 3 — Shared Mutable Buffer

Suppose multiple threads need to perform synchronized operations on the same mutable character buffer.

A possible choice is:

```java
StringBuffer buffer = new StringBuffer();

buffer.append("Data");
```

The exact concurrency design should still be considered.

---

# 20. ⚡ Performance Example

Consider:

```java
String result = "";

for (int i = 0; i < 1000; i++) {
    result += i;
}
```

Repeated concatenation may involve creating many intermediate String objects.

A mutable builder is generally more suitable:

```java
StringBuilder result = new StringBuilder();

for (int i = 0; i < 1000; i++) {
    result.append(i);
}
```

This avoids repeatedly creating a new immutable String for every modification step.

---

## Important Compiler Note

Simple String concatenation expressions may be optimized by the Java compiler/runtime.

Therefore, do not claim:

> Every `+` operation always creates a new String object in exactly the same way.

For explicit repeated construction, StringBuilder is the standard choice.

---

# 21. 🌳 Decision Tree

Use this simple decision process:

```text
Do I need text?
      │
      ↓
Is frequent modification required?
      │
   ┌──┴──┐
  NO    YES
   │      │
   ↓      ↓
 String   Is synchronization required?
              │
           ┌──┴──┐
          NO    YES
          │      │
          ↓      ↓
   StringBuilder StringBuffer
```

---

# 22. 📊 Detailed Comparison

| Feature | String | StringBuilder | StringBuffer |
|---|---|---|---|
| Package | `java.lang` | `java.lang` | `java.lang` |
| Mutable | ❌ | ✅ | ✅ |
| Immutable | ✅ | ❌ | ❌ |
| Synchronized methods | Not applicable | ❌ | ✅ |
| Thread-safe shared mutation | Naturally safe due to immutability | ❌ | Individual methods synchronized |
| Performance for repeated mutation | Usually less suitable | Generally fastest of the three for this use | Generally slower than StringBuilder |
| String Pool | Literals can use pool | Object itself not a pooled String | Object itself not a pooled String |
| `append()` | ❌ | ✅ | ✅ |
| `insert()` | ❌ | ✅ | ✅ |
| `delete()` | ❌ | ✅ | ✅ |
| `reverse()` | ❌ | ✅ | ✅ |
| `setCharAt()` | ❌ | ✅ | ✅ |
| `toString()` | Already String | Converts to String | Converts to String |
| Best general use | Immutable text | Mutable text | Synchronized mutable text |

---

# 23. ⚖️ Advantages and Disadvantages

## String

### ✅ Advantages

- Immutable
- Safe to share
- Supports String Pool
- Rich String API
- Excellent for fixed text
- Useful as keys in hash-based collections because it is immutable

### ❌ Disadvantages

- Repeated modification can create many intermediate objects
- Not designed for repeated character modifications

---

## StringBuilder

### ✅ Advantages

- Mutable
- Efficient for repeated modifications
- Generally faster than StringBuffer
- Excellent for loops and text construction
- Simple API

### ❌ Disadvantages

- Not synchronized
- Not appropriate for unsynchronized concurrent mutation of the same instance

---

## StringBuffer

### ✅ Advantages

- Mutable
- Synchronized methods
- Useful for shared mutable buffer operations where synchronization is appropriate
- Similar API to StringBuilder

### ❌ Disadvantages

- Synchronization overhead
- Generally slower than StringBuilder
- Often unnecessary when only one thread modifies the buffer

---

# 24. ⚠️ Common Mistakes

## ❌ Mistake 1

> StringBuilder is immutable.

Wrong.

Correct:

```text
StringBuilder → Mutable
```

---

## ❌ Mistake 2

> StringBuffer is immutable.

Wrong.

Correct:

```text
StringBuffer → Mutable
```

---

## ❌ Mistake 3

> StringBuilder is synchronized.

Wrong.

Correct:

```text
StringBuilder → Not synchronized
```

---

## ❌ Mistake 4

> StringBuffer is always faster.

Wrong.

Correct:

```text
StringBuilder is generally faster when synchronization is unnecessary.
```

---

## ❌ Mistake 5

> String is a primitive data type.

Wrong.

Correct:

```text
String is a class.
```

---

## ❌ Mistake 6

> StringBuilder and StringBuffer objects are stored in the String Pool.

Wrong.

The String Pool is specifically associated with String objects/literals.

---

## ❌ Mistake 7

> Thread-safe means every multi-operation sequence is atomic.

Wrong.

Individual synchronized methods do not automatically make a sequence of calls atomic.

---

# 25. 🚨 Interview Traps

## Trap 1

```java
String s = "Java";

s.concat(" Developer");

System.out.println(s);
```

Output:

```text
Java
```

Why?

Because String is immutable and the returned String was ignored.

---

## Trap 2

```java
StringBuilder sb = new StringBuilder("Java");

sb.append(" Developer");

System.out.println(sb);
```

Output:

```text
Java Developer
```

Why?

Because StringBuilder is mutable.

---

## Trap 3

```java
StringBuffer sb = new StringBuffer("Java");

sb.append(" Developer");

System.out.println(sb);
```

Output:

```text
Java Developer
```

Why?

Because StringBuffer is mutable.

---

## Trap 4

Which one is synchronized?

```text
StringBuffer
```

---

## Trap 5

Which one is generally preferred for repeated modification in a single-threaded context?

```text
StringBuilder
```

---

## Trap 6

Which one is immutable?

```text
String
```

---

## Trap 7

Which one generally has the lowest synchronization overhead?

```text
StringBuilder
```

It has no method synchronization.

---

# 26. 🔥 Top 25 Interview Questions

## Q1. What is the main difference between String, StringBuilder and StringBuffer?

**Answer:**

String is immutable.

StringBuilder is mutable and not synchronized.

StringBuffer is mutable and synchronized.

---

## Q2. Which one is immutable?

**Answer:**

String.

---

## Q3. Which ones are mutable?

**Answer:**

StringBuilder and StringBuffer.

---

## Q4. Which one is synchronized?

**Answer:**

StringBuffer provides synchronized methods.

---

## Q5. Is StringBuilder thread-safe?

**Answer:**

No. It is not synchronized for concurrent modification of the same instance.

---

## Q6. Is StringBuffer thread-safe?

**Answer:**

Its methods are synchronized, providing thread-safe individual operations on the same instance.

---

## Q7. Which is faster: StringBuilder or StringBuffer?

**Answer:**

StringBuilder is generally faster because it does not have synchronization overhead.

---

## Q8. Why is StringBuilder faster?

**Answer:**

Because its methods are not synchronized.

---

## Q9. Why is StringBuffer slower?

**Answer:**

Synchronization introduces additional overhead.

---

## Q10. When should we use String?

**Answer:**

Use String when text is immutable or does not require frequent modification.

---

## Q11. When should we use StringBuilder?

**Answer:**

Use StringBuilder for frequent string modifications when synchronization is not required.

---

## Q12. When should we use StringBuffer?

**Answer:**

Use StringBuffer when mutable string data needs synchronized method operations.

---

## Q13. What does mutable mean?

**Answer:**

It means the object's state/content can be modified after creation.

---

## Q14. What does immutable mean?

**Answer:**

It means the object's state/content cannot be changed after creation.

---

## Q15. Why is String good for HashMap keys?

**Answer:**

String is immutable, so its hash code and equality-relevant content cannot change after insertion.

---

## Q16. Can StringBuilder be converted to String?

**Answer:**

Yes.

Use:

```java
StringBuilder sb = new StringBuilder("Java");

String s = sb.toString();
```

---

## Q17. Can StringBuffer be converted to String?

**Answer:**

Yes.

Use:

```java
StringBuffer sb = new StringBuffer("Java");

String s = sb.toString();
```

---

## Q18. Can String be converted to StringBuilder?

**Answer:**

Yes.

Example:

```java
String s = "Java";

StringBuilder sb = new StringBuilder(s);
```

---

## Q19. Can String be converted to StringBuffer?

**Answer:**

Yes.

Example:

```java
String s = "Java";

StringBuffer sb = new StringBuffer(s);
```

---

## Q20. Do StringBuilder and StringBuffer use the String Pool for their objects?

**Answer:**

No. Their mutable objects are separate objects. A String argument passed to their constructors may itself be a pooled String.

---

## Q21. Why is String immutable?

**Answer:**

Immutability provides benefits such as safe sharing, String Pool support, stable hash codes, and easier reasoning about String values.

---

## Q22. Does StringBuffer guarantee atomic multi-method operations?

**Answer:**

No.

Individual methods are synchronized, but a sequence of method calls is not automatically atomic.

---

## Q23. What should I use inside a loop for repeated string building?

**Answer:**

Usually StringBuilder when synchronization is not required.

---

## Q24. Is String concatenation always inefficient?

**Answer:**

No.

The compiler/runtime can optimize some concatenation expressions. However, for explicit repeated string construction, StringBuilder is generally the appropriate tool.

---

## Q25. Give the simplest way to remember all three.

**Answer:**

```text
String
    → Immutable

StringBuilder
    → Mutable + Fast + Not synchronized

StringBuffer
    → Mutable + Synchronized
```

---

# 27. 🎤 30-Second Interview Answer

> **String is immutable, meaning its content cannot be changed after creation. StringBuilder and StringBuffer are mutable classes designed for modifying character sequences. StringBuilder is not synchronized and is generally preferred when synchronization is unnecessary because it has less overhead. StringBuffer provides synchronized methods and can be useful when synchronized access to a shared mutable buffer is required.**

---

# 28. 🎤 1-Minute Interview Answer

> **Java provides String, StringBuilder, and StringBuffer for working with text. String is immutable, so modifications produce a new String rather than changing the existing object. StringBuilder and StringBuffer are mutable, so operations such as append, insert, delete, and reverse can modify the same object. The main difference between StringBuilder and StringBuffer is synchronization. StringBuilder is not synchronized and is generally faster, making it suitable for most single-threaded or externally synchronized string-building tasks. StringBuffer has synchronized methods, so it can be useful for shared mutable string operations where that synchronization is appropriate.**

---

# 29. 🧾 Cheat Sheet

```text
┌─────────────────────────────────────────────────────┐
│                    STRING                            │
├─────────────────────────────────────────────────────┤
│ Immutable                                           │
│ String Pool                                         │
│ Safe to share because contents cannot change        │
│ Good for fixed text                                 │
└─────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────┐
│                 STRINGBUILDER                       │
├─────────────────────────────────────────────────────┤
│ Mutable                                             │
│ Not synchronized                                    │
│ Generally faster than StringBuffer                  │
│ Good for repeated string construction               │
└─────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────┐
│                  STRINGBUFFER                       │
├─────────────────────────────────────────────────────┤
│ Mutable                                             │
│ Synchronized methods                                │
│ Synchronization overhead                            │
│ Useful for synchronized mutable operations          │
└─────────────────────────────────────────────────────┘
```

---

# 30. 🧠 Memory Tricks

## 🔥 Trick 1 — I / M / M

```text
String
    → I
    → Immutable

StringBuilder
    → M
    → Mutable

StringBuffer
    → M
    → Mutable
```

---

## 🔥 Trick 2 — Builder vs Buffer

Think:

```text
Builder
    ↓
Build quickly
    ↓
No synchronization

Buffer
    ↓
Shared mutable buffer
    ↓
Synchronization
```

---

## 🔥 Trick 3 — One-Line Formula

```text
String = Immutable

StringBuilder = Mutable + Fast + No Sync

StringBuffer = Mutable + Sync
```

---

## 🔥 Trick 4 — Modification

```text
String
    ↓
New object/result

StringBuilder
    ↓
Modify existing mutable object

StringBuffer
    ↓
Modify existing mutable object
    +
Synchronization
```

---

# 31. 🚀 Final Revision

Before moving to the next topic, you should be able to explain:

- ✅ What is String?
- ✅ What is StringBuilder?
- ✅ What is StringBuffer?
- ✅ Which one is immutable?
- ✅ Which ones are mutable?
- ✅ Which one is synchronized?
- ✅ Why is StringBuilder generally faster?
- ✅ Why does StringBuffer have synchronization overhead?
- ✅ When should String be used?
- ✅ When should StringBuilder be used?
- ✅ When should StringBuffer be used?
- ✅ What is the String Pool?
- ✅ Why aren't StringBuilder objects in the String Pool?
- ✅ How do you convert StringBuilder to String?
- ✅ How do you convert StringBuffer to String?
- ✅ What does thread-safe mean?
- ✅ Does synchronized mean every sequence of operations is atomic?
- ✅ Why is String useful as a HashMap key?
- ✅ What happens during repeated String modification?
- ✅ Why is StringBuilder commonly used inside loops?

---

# ⭐ The Most Important Comparison

| Requirement | Recommended Concept |
|---|---|
| Fixed / immutable text | `String` |
| Frequent modifications | `StringBuilder` |
| Frequent modifications + synchronized methods | `StringBuffer` |
| String literals / String Pool | `String` |
| General single-threaded string building | `StringBuilder` |
| Shared mutable buffer requiring synchronized individual operations | `StringBuffer` |

---

# 🎯 Final Interview Line

> **String is immutable, StringBuilder is mutable and unsynchronized, and StringBuffer is mutable with synchronized methods. StringBuilder is generally preferred for repeated string manipulation when synchronization is not required, while StringBuffer is useful when synchronized access to a shared mutable buffer is needed.**

---

# 32. 🔗 Next Topic

String playlist:

```text
04-Strings/
│
├── 01-String-Introduction.md
├── 02-String-Pool.md
├── 03-String-Immutability.md
├── 04-String-Methods.md
├── 05-StringBuilder.md
├── 06-StringBuffer.md
├── 07-String-vs-StringBuilder-vs-StringBuffer.md  ← YOU ARE HERE
└── 08-String-Interview-Questions.md
```

### Learning Flow

```text
String Introduction
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
Interview Questions
```

---

# 🏆 FINAL MEMORY CARD

```text
┌──────────────────────────────────────┐
│              STRING                  │
│                                      │
│          IMMUTABLE                   │
│          String Pool                 │
│          Fixed Text                  │
└──────────────────────────────────────┘

                  VS

┌──────────────────────────────────────┐
│          STRINGBUILDER               │
│                                      │
│          MUTABLE                     │
│          NOT SYNCHRONIZED            │
│          GENERALLY FASTER            │
└──────────────────────────────────────┘

                  VS

┌──────────────────────────────────────┐
│           STRINGBUFFER               │
│                                      │
│          MUTABLE                     │
│          SYNCHRONIZED                │
│          MORE OVERHEAD               │
└──────────────────────────────────────┘
```

> 💡 **Remember:**
>
> `String` → **Immutable**
>
> `StringBuilder` → **Mutable + No Synchronization**
>
> `StringBuffer` → **Mutable + Synchronization**