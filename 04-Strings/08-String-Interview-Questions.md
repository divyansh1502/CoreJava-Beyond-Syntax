````md
# 🎯 String — Interview Questions

> **A complete interview-focused revision of Java Strings, covering fundamentals, String Pool, immutability, methods, StringBuilder, StringBuffer, comparisons, internal behavior, traps, and practical interview questions.**

---

# 📌 Table of Contents

1. [String Quick Revision](#1--string-quick-revision)
2. [String Fundamentals](#2--string-fundamentals)
3. [String Creation](#3--string-creation)
4. [String Pool](#4--string-pool)
5. [String Immutability](#5--string-immutability)
6. [`==` vs `equals()`](#6----vs-equals)
7. [String Methods](#7--string-methods)
8. [StringBuilder](#8--stringbuilder)
9. [StringBuffer](#9--stringbuffer)
10. [String vs StringBuilder vs StringBuffer](#10--string-vs-stringbuilder-vs-stringbuffer)
11. [String and Memory](#11--string-and-memory)
12. [Common Coding Questions](#12--common-coding-questions)
13. [Interview Traps](#13--interview-traps)
14. [Top 30 Interview Questions](#14--top-30-interview-questions)
15. [Rapid-Fire Questions](#15--rapid-fire-questions)
16. [Output-Based Questions](#16--output-based-questions)
17. [30-Second Interview Answer](#17--30-second-interview-answer)
18. [1-Minute Interview Answer](#18--1-minute-interview-answer)
19. [Cheat Sheet](#19--cheat-sheet)
20. [Memory Tricks](#20--memory-tricks)
21. [Final Revision Checklist](#21--final-revision-checklist)

---

# 1. 🔤 String Quick Revision

## What is String?

> A String in Java is an object that represents a sequence of characters.

`String` is:

- A class
- Part of `java.lang`
- Immutable
- `final`
- Commonly used for textual data

Example:

```java
String name = "Java";
```

## Most Important String Properties

```text
String
   ↓
Class
   ↓
java.lang
   ↓
final
   ↓
Immutable
   ↓
Supports String Pool
```

---

# 2. 📚 String Fundamentals

## Q1. Is String a primitive data type?

### Answer

No.

`String` is a class.

Example:

```java
String name = "Divyansh";
```

Here:

```text
String       → class/type
name         → reference variable
"Divyansh"   → String object/value
```

---

## Q2. Why can we use String without importing it?

Because `String` belongs to:

```text
java.lang
```

Classes from `java.lang` are automatically available.

Examples:

```text
String
Object
System
Math
Integer
Thread
```

---

## Q3. Is String a final class?

Yes.

Conceptually:

```java
public final class String
```

Because `String` is final, it cannot be subclassed.

You cannot do:

```java
class MyString extends String {
}
```

---

## Q4. Why is String final?

Important reasons include:

- Preserving immutability guarantees
- Preventing subclasses from changing String behavior
- Security
- Predictable behavior
- Stable use in collections and APIs

---

## Q5. What does immutable mean?

Immutable means:

> Once a String object is created, its content cannot be changed.

Example:

```java
String s = "Java";

s.concat(" Developer");

System.out.println(s);
```

Output:

```text
Java
```

The original String remains unchanged.

The `concat()` method returns another String.

---

# 3. 🏗️ String Creation

There are two major ways to create Strings.

## Method 1 — String Literal

```java
String s1 = "Java";
```

## Method 2 — Using `new`

```java
String s2 = new String("Java");
```

These two forms can have different memory/reference behavior.

---

## Q6. Difference between String literal and `new String()`?

### Literal

```java
String s = "Java";
```

The JVM can use the String Pool.

### `new`

```java
String s = new String("Java");
```

A new String object is explicitly created.

The `"Java"` literal can still be present in the String Pool.

---

# 4. 🏊 String Pool

The String Pool is a JVM-managed pool used to share canonical String objects, particularly String literals.

Example:

```java
String s1 = "Java";
String s2 = "Java";
```

Conceptually:

```text
s1 ─────┐
        ↓
      "Java"
   String Pool
        ↑
s2 ─────┘
```

Therefore:

```java
System.out.println(s1 == s2);
```

Output:

```text
true
```

---

## Q7. What is String Pool?

> String Pool is a special pool maintained by the JVM for canonical String objects, particularly String literals, allowing identical literal values to be shared.

---

## Q8. What happens with `new String()`?

Example:

```java
String s1 = "Java";
String s2 = new String("Java");

System.out.println(s1 == s2);
System.out.println(s1.equals(s2));
```

Output:

```text
false
true
```

Conceptually:

```text
String Pool:

    "Java"
       ↑
      s1

Heap:

    new String("Java")
           ↑
          s2
```

The references are different, but their contents are equal.

---

## Q9. What does `intern()` do?

`intern()` returns the canonical representation of a String from the String Pool.

Example:

```java
String s1 = new String("Java");
String s2 = s1.intern();

System.out.println(s2 == "Java");
```

Output:

```text
true
```

---

# 5. 🔒 String Immutability

## Q10. Why is String immutable?

Important benefits include:

### 1. Security

Strings are commonly used for:

- File paths
- URLs
- Class names
- Database URLs
- Configuration
- Credentials

If Strings could be changed unexpectedly, security and correctness could be affected.

### 2. String Pool

Because Strings are immutable, the JVM can safely share identical pooled String objects.

### 3. Thread Safety

Immutable objects can be safely shared because their state cannot be changed.

### 4. Hashing

A String's content and hash code remain stable.

This makes String useful as a key in:

```text
HashMap
HashSet
Hashtable
```

---

## Q11. Does `concat()` modify the original String?

No.

Example:

```java
String s = "Java";

s.concat(" Developer");

System.out.println(s);
```

Output:

```text
Java
```

Correct:

```java
String s = "Java";

s = s.concat(" Developer");

System.out.println(s);
```

Output:

```text
Java Developer
```

---

# 6. ⚖️ `==` vs `equals()`

This is one of the most important String interview topics.

## `==`

For references, `==` checks whether two references refer to the same object.

## `equals()`

String's `equals()` checks logical/content equality.

---

## Example

```java
String s1 = "Java";
String s2 = "Java";

System.out.println(s1 == s2);
System.out.println(s1.equals(s2));
```

Output:

```text
true
true
```

Because both literals can refer to the same pooled object.

---

## Another Example

```java
String s1 = new String("Java");
String s2 = new String("Java");

System.out.println(s1 == s2);
System.out.println(s1.equals(s2));
```

Output:

```text
false
true
```

Different objects:

```text
==       → reference identity
equals() → content equality
```

---

## Interview Rule

Remember:

```text
==

   ↓

Same object?
```

```text
equals()

   ↓

Same content?
```

---

# 7. 🛠️ String Methods

Important String methods include:

```text
length()
charAt()
substring()
indexOf()
lastIndexOf()
equals()
equalsIgnoreCase()
contains()
startsWith()
endsWith()
concat()
replace()
replaceAll()
split()
trim()
strip()
toLowerCase()
toUpperCase()
```

---

## Q12. What does `length()` return?

Number of UTF-16 code units in the String.

For basic ASCII text, this corresponds to the number of characters.

Example:

```java
String s = "Java";

System.out.println(s.length());
```

Output:

```text
4
```

---

## Q13. What does `charAt()` return?

A `char` at a specified index.

Example:

```java
String s = "Java";

System.out.println(s.charAt(2));
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

---

## Q14. What does `substring()` do?

Returns a portion of a String.

Example:

```java
String s = "Java Programming";

System.out.println(s.substring(5));
```

Output:

```text
Programming
```

---

## Q15. Is `substring()` inclusive or exclusive?

For:

```java
substring(beginIndex, endIndex)
```

the:

```text
beginIndex → inclusive
endIndex   → exclusive
```

Example:

```java
String s = "Java";

System.out.println(s.substring(1, 3));
```

Output:

```text
av
```

---

## Q16. What does `indexOf()` return?

The index of the first occurrence.

Example:

```java
String s = "Java Java";

System.out.println(s.indexOf("Java"));
```

Output:

```text
0
```

If not found:

```text
-1
```

---

## Q17. What does `lastIndexOf()` do?

Returns the index of the last occurrence.

Example:

```java
String s = "Java Java";

System.out.println(s.lastIndexOf("Java"));
```

Output:

```text
5
```

---

## Q18. Difference between `equals()` and `equalsIgnoreCase()`?

`equals()` is case-sensitive.

`equalsIgnoreCase()` ignores case differences.

Example:

```java
System.out.println("Java".equals("java"));
System.out.println("Java".equalsIgnoreCase("java"));
```

Output:

```text
false
true
```

---

## Q19. What does `contains()` do?

Checks whether a sequence exists.

Example:

```java
String s = "Java Developer";

System.out.println(s.contains("Dev"));
```

Output:

```text
true
```

---

## Q20. What does `startsWith()` do?

Checks whether a String starts with a specified prefix.

Example:

```java
System.out.println("Java Developer".startsWith("Java"));
```

Output:

```text
true
```

---

## Q21. What does `endsWith()` do?

Checks whether a String ends with a specified suffix.

Example:

```java
System.out.println("Java Developer".endsWith("Developer"));
```

Output:

```text
true
```

---

# 8. 🏗️ StringBuilder

StringBuilder is:

```text
Mutable
Not synchronized
Generally faster than StringBuffer
```

Example:

```java
StringBuilder sb = new StringBuilder("Java");

sb.append(" Developer");

System.out.println(sb);
```

Output:

```text
Java Developer
```

---

## Q22. Why use StringBuilder?

Use it for frequent String modifications.

Example:

```java
StringBuilder sb = new StringBuilder();

for (int i = 0; i < 5; i++) {
    sb.append(i);
}

System.out.println(sb);
```

Output:

```text
01234
```

---

## Important StringBuilder Methods

```text
append()
insert()
delete()
deleteCharAt()
replace()
reverse()
charAt()
setCharAt()
substring()
length()
capacity()
toString()
```

---

# 9. 🧵 StringBuffer

StringBuffer is:

```text
Mutable
Synchronized
```

Example:

```java
StringBuffer sb = new StringBuffer("Java");

sb.append(" Developer");

System.out.println(sb);
```

Output:

```text
Java Developer
```

---

## Q23. Difference between StringBuilder and StringBuffer?

| Feature | StringBuilder | StringBuffer |
|---|---|---|
| Mutable | Yes | Yes |
| Synchronized | No | Yes |
| Generally faster | Yes | No |
| Thread-safe individual methods | No | Yes |
| Package | `java.lang` | `java.lang` |
| Introduced | Java 5 | Java 1.0 |

---

## Q24. Which should you use when synchronization is unnecessary?

Generally:

```text
StringBuilder
```

---

# 10. ⚔️ String vs StringBuilder vs StringBuffer

| Feature | String | StringBuilder | StringBuffer |
|---|---|---|---|
| Mutable | ❌ | ✅ | ✅ |
| Immutable | ✅ | ❌ | ❌ |
| Synchronized methods | N/A | ❌ | ✅ |
| Repeated modification | Less suitable | Excellent | Good |
| Generally fastest for mutable building | ❌ | ✅ | ❌ |
| String Pool | Yes | No | No |
| `append()` | ❌ | ✅ | ✅ |
| `reverse()` | ❌ | ✅ | ✅ |
| `setCharAt()` | ❌ | ✅ | ✅ |
| `toString()` | Already String | Converts to String | Converts to String |

---

# 11. 🧠 String and Memory

## Example 1

```java
String s1 = "Java";
String s2 = "Java";

System.out.println(s1 == s2);
```

Output:

```text
true
```

Conceptually:

```text
String Pool

    "Java"
     ↑  ↑
     │  │
    s1 s2
```

---

## Example 2

```java
String s1 = new String("Java");
String s2 = new String("Java");

System.out.println(s1 == s2);
System.out.println(s1.equals(s2));
```

Output:

```text
false
true
```

Conceptually:

```text
Pool:

    "Java"

Heap:

    String object
         ↑
        s1

    String object
         ↑
        s2
```

---

# 12. 💻 Common Coding Questions

## Q25. Reverse a String

### Using StringBuilder

```java
String s = "Java";

String reversed = new StringBuilder(s)
        .reverse()
        .toString();

System.out.println(reversed);
```

Output:

```text
avaJ
```

---

## Q26. Reverse a String Without StringBuilder

Use a loop:

```java
String s = "Java";
StringBuilder reversed = new StringBuilder();

for (int i = s.length() - 1; i >= 0; i--) {
    reversed.append(s.charAt(i));
}

System.out.println(reversed);
```

Output:

```text
avaJ
```

For learning the algorithm, a loop is useful. For repeated string construction, using a StringBuilder avoids unnecessary intermediate String objects.

---

## Q27. Check Palindrome

Example:

```java
String s = "madam";

String reversed = new StringBuilder(s)
        .reverse()
        .toString();

if (s.equals(reversed)) {
    System.out.println("Palindrome");
} else {
    System.out.println("Not Palindrome");
}
```

Output:

```text
Palindrome
```

---

## Q28. Count Characters

```java
String s = "Java";

int count = s.length();

System.out.println(count);
```

Output:

```text
4
```

---

## Q29. Count a Specific Character

```java
String s = "banana";

int count = 0;

for (int i = 0; i < s.length(); i++) {
    if (s.charAt(i) == 'a') {
        count++;
    }
}

System.out.println(count);
```

Output:

```text
3
```

---

## Q30. Remove Spaces

```java
String s = "Java Developer";

String result = s.replace(" ", "");

System.out.println(result);
```

Output:

```text
JavaDeveloper
```

---

# 13. 🚨 Interview Traps

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

Reason:

```text
String → Immutable
```

---

## Trap 2

```java
String s = "Java";

s = s.concat(" Developer");

System.out.println(s);
```

Output:

```text
Java Developer
```

---

## Trap 3

```java
String a = "Java";
String b = "Java";

System.out.println(a == b);
```

Output:

```text
true
```

Identical literals can refer to the same pooled object.

---

## Trap 4

```java
String a = new String("Java");
String b = new String("Java");

System.out.println(a == b);
```

Output:

```text
false
```

Different objects.

---

## Trap 5

```java
String a = new String("Java");
String b = new String("Java");

System.out.println(a.equals(b));
```

Output:

```text
true
```

Same content.

---

## Trap 6

```java
StringBuilder sb = new StringBuilder("Java");

sb.append(" Developer");

System.out.println(sb);
```

Output:

```text
Java Developer
```

Because StringBuilder is mutable.

---

## Trap 7

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

## Trap 8

```java
String s = null;

System.out.println(s.length());
```

This throws:

```text
NullPointerException
```

Because `s` does not reference a String object.

---

# 14. 🔥 Top 30 Interview Questions

## Q1. What is String?

### Answer

String is a class representing a sequence of characters.

---

## Q2. Is String primitive?

### Answer

No. String is a class.

---

## Q3. Is String immutable?

### Answer

Yes.

---

## Q4. Why is String immutable?

### Answer

Immutability supports safe sharing, String Pooling, security, stable hashing, and predictable behavior.

---

## Q5. Is String final?

### Answer

Yes.

---

## Q6. Which package contains String?

### Answer

```text
java.lang
```

---

## Q7. What is String Pool?

### Answer

A JVM-managed pool used to share canonical String objects, particularly String literals.

---

## Q8. What is the difference between `==` and `equals()`?

### Answer

For references, `==` checks reference identity, while String's `equals()` checks content equality.

---

## Q9. What happens with `new String("Java")`?

### Answer

It explicitly creates a new String object, while the literal `"Java"` can also exist in the String Pool.

---

## Q10. What does `intern()` do?

### Answer

It returns the canonical pooled representation of the String.

---

## Q11. Can String be modified?

### Answer

No. String is immutable.

---

## Q12. What does `concat()` return?

### Answer

A String containing the concatenated result.

---

## Q13. What is StringBuilder?

### Answer

A mutable sequence of characters designed for efficient string construction and modification.

---

## Q14. Is StringBuilder synchronized?

### Answer

No.

---

## Q15. What is StringBuffer?

### Answer

A mutable sequence of characters whose methods are synchronized.

---

## Q16. Is StringBuffer synchronized?

### Answer

Its methods are synchronized.

---

## Q17. Which is generally faster, StringBuilder or StringBuffer?

### Answer

StringBuilder is generally faster when synchronization is not required.

---

## Q18. Why is StringBuilder faster?

### Answer

It does not incur method synchronization overhead.

---

## Q19. What is the difference between mutable and immutable?

### Answer

Mutable objects can change their state after creation. Immutable objects cannot.

---

## Q20. Can StringBuilder be converted to String?

### Answer

Yes:

```java
StringBuilder sb = new StringBuilder("Java");

String s = sb.toString();
```

---

## Q21. Can StringBuffer be converted to String?

### Answer

Yes:

```java
StringBuffer sb = new StringBuffer("Java");

String s = sb.toString();
```

---

## Q22. What does charAt() return?

### Answer

A `char` at the specified index.

---

## Q23. What does length() return?

### Answer

The number of UTF-16 code units in the String.

---

## Q24. What does substring() return?

### Answer

A new String representing the requested range.

---

## Q25. What does indexOf() return if the value is not found?

### Answer

```text
-1
```

---

## Q26. Difference between equals() and equalsIgnoreCase()?

### Answer

`equals()` is case-sensitive, while `equalsIgnoreCase()` ignores case.

---

## Q27. Why can String be used as a HashMap key?

### Answer

Because String is immutable, its equality-relevant content and hash code remain stable after insertion.

---

## Q28. What happens if you modify a String?

### Answer

You cannot modify the existing String object. Operations that appear to modify it return a new String.

---

## Q29. What should you use for repeated concatenation inside a loop?

### Answer

Generally `StringBuilder` when synchronization is not required.

---

## Q30. Give the difference in one line.

### Answer

```text
String

    → Immutable

StringBuilder

    → Mutable + Not synchronized

StringBuffer

    → Mutable + Synchronized
```

---

# 15. ⚡ Rapid-Fire Questions

| Question | Answer |
|---|---|
| String primitive? | No |
| String class? | Yes |
| String package? | `java.lang` |
| String final? | Yes |
| String mutable? | No |
| StringBuilder mutable? | Yes |
| StringBuffer mutable? | Yes |
| StringBuilder synchronized? | No |
| StringBuffer synchronized? | Yes |
| String Pool? | Yes |
| `==` checks? | Reference identity |
| `equals()` checks? | Content |
| `charAt()` returns? | `char` |
| `length()` returns? | `int` |
| `indexOf()` not found? | `-1` |
| StringBuilder → String? | `toString()` |
| StringBuffer → String? | `toString()` |
| Reverse String? | `StringBuilder.reverse()` |
| StringBuilder generally faster? | Yes |
| StringBuffer synchronization overhead? | Yes |

---

# 16. 🧪 Output-Based Questions

## Question 1

```java
String s1 = "Java";
String s2 = "Java";

System.out.println(s1 == s2);
```

### Answer

```text
true
```

### Reason

Both literals can refer to the same pooled String object.

---

## Question 2

```java
String s1 = new String("Java");
String s2 = new String("Java");

System.out.println(s1 == s2);
```

### Answer

```text
false
```

### Reason

Two separate String objects are explicitly created.

---

## Question 3

```java
String s1 = new String("Java");
String s2 = new String("Java");

System.out.println(s1.equals(s2));
```

### Answer

```text
true
```

### Reason

Their contents are equal.

---

## Question 4

```java
String s = "Java";

s.concat(" Developer");

System.out.println(s);
```

### Answer

```text
Java
```

### Reason

The returned String was not assigned back.

---

## Question 5

```java
String s = "Java";

s = s.concat(" Developer");

System.out.println(s);
```

### Answer

```text
Java Developer
```

---

## Question 6

```java
StringBuilder sb = new StringBuilder("Java");

sb.append(" Developer");

System.out.println(sb);
```

### Answer

```text
Java Developer
```

---

## Question 7

```java
StringBuffer sb = new StringBuffer("Java");

sb.reverse();

System.out.println(sb);
```

### Answer

```text
avaJ
```

---

## Question 8

```java
String s = "Java";

System.out.println(s.charAt(1));
```

### Answer

```text
a
```

---

## Question 9

```java
String s = "Java";

System.out.println(s.substring(1, 3));
```

### Answer

```text
av
```

---

## Question 10

```java
String s = "Java";

System.out.println(s.indexOf("v"));
```

### Answer

```text
2
```

---

# 17. 🎤 30-Second Interview Answer

> **String is a final, immutable class in Java's `java.lang` package that represents a sequence of characters. Java also provides StringBuilder and StringBuffer for mutable character sequences. StringBuilder is not synchronized and is generally preferred for repeated string manipulation when synchronization is unnecessary. StringBuffer is synchronized and can be useful when synchronized operations on a shared mutable buffer are required. String also has special support through the String Pool, which allows identical String literals to be shared.**

---

# 18. 🎤 1-Minute Interview Answer

> **In Java, String is an immutable class representing a sequence of characters. Because it is immutable, operations such as concatenation don't modify the existing String; they produce a new String result. String literals can be stored and shared through the String Pool. For frequent string modifications, Java provides StringBuilder and StringBuffer. Both are mutable. StringBuilder is not synchronized and generally has better performance, so it is commonly used when synchronization isn't required. StringBuffer provides synchronized methods and can be useful for synchronized operations on shared mutable character data. One important interview distinction is that `==` compares reference identity, while `equals()` compares String content.**

---

# 19. 🧾 Cheat Sheet

## String

```text
Immutable
Final
java.lang
String Pool
Content comparison → equals()
Reference comparison → ==
```

## StringBuilder

```text
Mutable
Not synchronized
Generally faster
Repeated modifications
append()
insert()
delete()
reverse()
```

## StringBuffer

```text
Mutable
Synchronized methods
More synchronization overhead
Shared mutable operations
append()
insert()
delete()
reverse()
```

## Important String Methods

```text
length()
charAt()
substring()
indexOf()
lastIndexOf()
equals()
equalsIgnoreCase()
contains()
startsWith()
endsWith()
concat()
replace()
split()
trim()
strip()
toLowerCase()
toUpperCase()
```

---

# 20. 🧠 Memory Tricks

## 🔥 Trick 1

Remember:

```text
S
↓
String
↓
Still / Stable
↓
Immutable
```

---

## 🔥 Trick 2

```text
Builder
   ↓
Build
   ↓
Mutable
   ↓
No synchronization
```

---

## 🔥 Trick 3

```text
Buffer
   ↓
Shared buffer
   ↓
Synchronization
```

---

## 🔥 Trick 4

For comparison:

```text
String
    = Immutable

StringBuilder
    = Mutable + No Sync

StringBuffer
    = Mutable + Sync
```

---

## 🔥 Trick 5 — `==` vs `equals()`

```text
==
   ↓
Same object?

equals()
   ↓
Same content?
```

---

# 21. ✅ Final Revision Checklist

Before considering the String chapter complete, make sure you can explain all of these without looking at the notes.

## Fundamentals

```text
[ ] What is String?
[ ] Is String primitive?
[ ] Why is String a class?
[ ] Which package contains String?
[ ] Why is String final?
[ ] What does immutable mean?
```

## Creation

```text
[ ] String literal
[ ] new String()
[ ] Difference between them
[ ] String Pool
[ ] intern()
```

## Immutability

```text
[ ] Why is String immutable?
[ ] What happens during concatenation?
[ ] Why is immutability useful?
[ ] Why is String useful as a HashMap key?
```

## Comparison

```text
[ ] == vs equals()
[ ] equalsIgnoreCase()
[ ] String vs StringBuilder
[ ] StringBuilder vs StringBuffer
[ ] String vs StringBuffer
```

## Methods

```text
[ ] length()
[ ] charAt()
[ ] substring()
[ ] indexOf()
[ ] lastIndexOf()
[ ] contains()
[ ] startsWith()
[ ] endsWith()
[ ] concat()
[ ] replace()
[ ] split()
[ ] trim()
[ ] strip()
```

## Mutable Strings

```text
[ ] StringBuilder
[ ] StringBuffer
[ ] append()
[ ] insert()
[ ] delete()
[ ] reverse()
[ ] setCharAt()
[ ] capacity()
[ ] toString()
```

## Interview Concepts

```text
[ ] String Pool
[ ] Immutability
[ ] Final class
[ ] Reference equality
[ ] Content equality
[ ] Thread safety
[ ] Synchronization
[ ] Performance
[ ] Memory behavior
```

---

# 🏆 MASTER MEMORY CARD

```text
┌────────────────────────────────────────────┐
│                  STRING                    │
├────────────────────────────────────────────┤
│ Immutable                                  │
│ final class                                │
│ java.lang                                  │
│ String Pool                                │
│ == → reference identity                    │
│ equals() → content equality                │
└────────────────────────────────────────────┘

                      VS

┌────────────────────────────────────────────┐
│              STRINGBUILDER                 │
├────────────────────────────────────────────┤
│ Mutable                                    │
│ Not synchronized                           │
│ Generally faster                           │
│ Repeated string construction               │
└────────────────────────────────────────────┘

                      VS

┌────────────────────────────────────────────┐
│               STRINGBUFFER                 │
├────────────────────────────────────────────┤
│ Mutable                                    │
│ Synchronized methods                       │
│ More synchronization overhead              │
│ Shared mutable operations                  │
└────────────────────────────────────────────┘
```

---

# ⭐ THE 3-LINE INTERVIEW REVISION

```text
String
    → Immutable

StringBuilder
    → Mutable + Not Synchronized

StringBuffer
    → Mutable + Synchronized
```

> 💡 If you remember only these three lines, you already have the core distinction between the three classes.

---

# 🚀 STRING CHAPTER COMPLETE

```text
04-Strings/
│
├── 01-String-Introduction.md
├── 02-String-Pool.md
├── 03-String-Immutability.md
├── 04-String-Methods.md
├── 05-StringBuilder.md
├── 06-StringBuffer.md
├── 07-String-vs-StringBuilder-vs-StringBuffer.md
└── 08-String-Interview-Questions.md  ← YOU ARE HERE
```

---

# 🎯 Final Takeaway

> **String is immutable and ideal for fixed textual values. StringBuilder is mutable and generally preferred for efficient string construction when synchronization is unnecessary. StringBuffer is also mutable but provides synchronized methods for situations where synchronized access to a shared buffer is required.**

# 🔥 STRING MASTERED
````
