# 🧱 StringBuilder in Java

> **`StringBuilder` is a mutable sequence of characters used when String data needs to be modified frequently. Unlike `String`, it allows modifications to the same mutable object and is generally efficient for repeated string construction.**

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
29. [StringBuilder and `+` Operator](#29--stringbuilder-and--operator)
30. [StringBuilder and Memory](#30--stringbuilder-and-memory)
31. [Capacity Growth](#31--capacity-growth)
32. [Time Complexity](#32--time-complexity)
33. [Thread Safety](#33--thread-safety)
34. [StringBuilder vs StringBuffer](#34--stringbuilder-vs-stringbuffer)
35. [StringBuilder vs String](#35--stringbuilder-vs-string)
36. [Common Mistakes](#36--common-mistakes)
37. [Interview Traps](#37--interview-traps)
38. [DSA & Problem Solving](#38--dsa--problem-solving)
39. [Top 20 Interview Questions](#39--top-20-interview-questions)
40. [30-Second Interview Answer](#40--30-second-interview-answer)
41. [Cheat Sheet](#41--cheat-sheet)
42. [Memory Tricks](#42--memory-tricks)
43. [Next Topic](#43--next-topic)
44. [Final Revision](#44--final-revision)

---

# 1. 🔤 What is StringBuilder?

`StringBuilder` is a class from:

```java
java.lang
```

It represents a:

> **Mutable sequence of characters.**

Mutable means its character sequence can be changed after the object is created.

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

The same `StringBuilder` object can be modified.

---

# 2. 🤔 Why Do We Need StringBuilder?

`String` is immutable.

Consider:

```java
String s = "Java";

s = s + " ";
s = s + "Programming";
s = s + " Language";
```

Each concatenation produces a new String result.

For a small number of operations this is usually fine.

But repeated concatenation can become inefficient.

Example:

```java
String result = "";

for (int i = 0; i < 10000; i++) {
    result = result + i;
}
```

A better approach for repeated construction is:

```java
StringBuilder sb = new StringBuilder();

for (int i = 0; i < 10000; i++) {
    sb.append(i);
}

String result = sb.toString();
```

### Core Idea

```text
String
   ↓
Immutable
   ↓
New String result when modified

StringBuilder
   ↓
Mutable
   ↓
Modify existing builder
```

---

# 3. 🆚 String vs StringBuilder

| Feature | String | StringBuilder |
|---|---|---|
| Mutability | Immutable | Mutable |
| Package | `java.lang` | `java.lang` |
| Same object modified? | No | Yes |
| Repeated modification | Less suitable | More suitable |
| Thread-safe concurrent modification | Immutable | No |
| Synchronized methods | Not applicable | No |
| Typical use | Fixed text | Dynamic text construction |

### Golden Rule

```text
String
   ↓
Immutable

StringBuilder
   ↓
Mutable
```

---

# 4. 🏗️ StringBuilder Class

`StringBuilder` belongs to:

```java
java.lang
```

Therefore, no explicit import is required.

Example:

```java
StringBuilder sb = new StringBuilder();
```

### Conceptual Hierarchy

```text
Object
   │
   └── AbstractStringBuilder
           │
           ├── StringBuilder
           │
           └── StringBuffer
```

`StringBuilder` and `StringBuffer` are both mutable character-sequence classes.

> **Interview note:** `AbstractStringBuilder` is an implementation superclass. Developers normally work with `StringBuilder` or `StringBuffer`.

---

# 5. 🆕 Creating StringBuilder

Common constructors include:

```java
new StringBuilder()
```

```java
new StringBuilder(String str)
```

```java
new StringBuilder(int capacity)
```

Example:

```java
StringBuilder sb1 = new StringBuilder();

StringBuilder sb2 = new StringBuilder("Java");

StringBuilder sb3 = new StringBuilder(100);
```

---

# 6. 🔹 Default Constructor

Example:

```java
StringBuilder sb = new StringBuilder();

System.out.println(sb.length());

System.out.println(sb.capacity());
```

Output:

```text
0
16
```

The default constructor creates an empty builder with an initial capacity of 16.

Therefore:

```text
length   = 0
capacity = 16
```

> **Important:** Capacity and length are different concepts.

---

# 7. 🔹 Constructor with String

Example:

```java
StringBuilder sb = new StringBuilder("Java");

System.out.println(sb.length());

System.out.println(sb.capacity());
```

Output:

```text
4
20
```

The initial capacity is:

```text
string.length() + 16
```

For `"Java"`:

```text
4 + 16 = 20
```

### Formula

```text
Initial capacity
=
String length + 16
```

---

# 8. 🔹 Constructor with Capacity

You can specify an initial capacity.

Example:

```java
StringBuilder sb = new StringBuilder(100);

System.out.println(sb.length());

System.out.println(sb.capacity());
```

Output:

```text
0
100
```

Here:

```text
length   = 0
capacity = 100
```

### Why Specify Capacity?

If you already know approximately how much text will be generated, preallocating capacity can reduce internal expansions.

---

# 9. 🔄 Mutability

## String

Example:

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

Because `String` is immutable and `concat()` returns a new String.

Correct usage:

```java
String s = "Java";

s = s.concat(" Programming");

System.out.println(s);
```

Output:

```text
Java Programming
```

---

## StringBuilder

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

The builder's mutable character sequence was modified.

---

# 10. ⚙️ Internal Working

Conceptually, `StringBuilder` maintains mutable character storage.

```text
StringBuilder
      │
      ↓
Mutable character storage
      │
      ├── J
      ├── a
      ├── v
      ├── a
      └── unused capacity
```

When more characters are appended:

```java
StringBuilder sb = new StringBuilder("Java");

sb.append(" Programming");
```

If enough capacity exists, the existing storage can accommodate the characters.

If capacity is insufficient, a larger storage area is allocated and the existing contents are copied.

### Important

Modern JDK implementations can differ internally.

For interviews, remember:

> **StringBuilder maintains expandable internal storage for a mutable character sequence.**

---

# 11. 📦 Capacity

Capacity represents the amount of internal storage currently available before another expansion is required.

Example:

```java
StringBuilder sb = new StringBuilder();

System.out.println(sb.length());

System.out.println(sb.capacity());
```

Output:

```text
0
16
```

After appending four characters:

```java
StringBuilder sb = new StringBuilder();

sb.append("Java");

System.out.println(sb.length());

System.out.println(sb.capacity());
```

Output:

```text
4
16
```

Notice:

```text
capacity ≠ length
```

### Remember

```text
length()
   ↓
Characters currently stored

capacity()
   ↓
Current internal storage capacity
```

---

# 12. 📏 length()

`length()` returns the number of characters currently stored.

Example:

```java
StringBuilder sb = new StringBuilder("Java");

System.out.println(sb.length());
```

Output:

```text
4
```

### Important

```text
length()
```

means:

> How many characters are logically present?

While:

```text
capacity()
```

means:

> How much internal storage is currently available?

---

# 13. ➕ append()

`append()` adds data to the end of the StringBuilder.

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

### Append Integer

```java
StringBuilder sb = new StringBuilder();

sb.append("Age: ");
sb.append(22);

System.out.println(sb);
```

Output:

```text
Age: 22
```

### Append Character

```java
StringBuilder sb = new StringBuilder();

sb.append('J');
sb.append('a');
sb.append('v');
sb.append('a');

System.out.println(sb);
```

Output:

```text
Java
```

### Append Boolean

```java
StringBuilder sb = new StringBuilder();

sb.append(true);

System.out.println(sb);
```

Output:

```text
true
```

### Append Multiple Values

```java
StringBuilder sb = new StringBuilder();

sb.append("Java")
  .append(" ")
  .append(21)
  .append(" ")
  .append(true);

System.out.println(sb);
```

Output:

```text
Java 21 true
```

### Return Type

`append()` returns the same `StringBuilder` object.

This enables method chaining.

---

# 14. ➕ insert()

`insert()` inserts data at a specified index.

Example:

```java
StringBuilder sb = new StringBuilder("Jav");

sb.insert(3, 'a');

System.out.println(sb);
```

Output:

```text
Java
```

Another example:

```java
StringBuilder sb = new StringBuilder("Java");

sb.insert(4, " Programming");

System.out.println(sb);
```

Output:

```text
Java Programming
```

### Index Visualization

```text
Java
0123

Java| Programming
    ↑
 index 4
```

### Important

Insertion shifts the existing characters to the right.

Therefore, insertion in the middle is generally `O(n)`.

---

# 15. 🔄 replace()

`replace()` replaces characters in a specified range.

Syntax:

```java
replace(start, end, str)
```

Example:

```java
StringBuilder sb = new StringBuilder("Java Programming");

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

---

# 16. 🗑️ delete()

`delete()` removes characters from a range.

Syntax:

```java
delete(start, end)
```

Example:

```java
StringBuilder sb = new StringBuilder("Java Programming");

sb.delete(4, 5);

System.out.println(sb);
```

Output:

```text
JavaProgramming
```

The space at index `4` was removed.

### Range Rule

```text
start → inclusive
end   → exclusive
```

Another example:

```java
StringBuilder sb = new StringBuilder("abcdef");

sb.delete(1, 4);

System.out.println(sb);
```

Output:

```text
aef
```

Characters at indexes:

```text
1 → b
2 → c
3 → d
```

were deleted.

---

# 17. 🗑️ deleteCharAt()

`deleteCharAt()` removes one character at a specified index.

Example:

```java
StringBuilder sb = new StringBuilder("Java");

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
StringBuilder sb = new StringBuilder("Java");

sb.reverse();

System.out.println(sb);
```

Output:

```text
avaJ
```

Another example:

```java
StringBuilder sb = new StringBuilder("12345");

sb.reverse();

System.out.println(sb);
```

Output:

```text
54321
```

### Important

`reverse()` modifies the existing StringBuilder.

---

# 19. 🔤 charAt()

`charAt()` returns the character at a specified index.

Example:

```java
StringBuilder sb = new StringBuilder("Java");

char ch = sb.charAt(2);

System.out.println(ch);
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

### Complexity

```text
charAt()
   ↓
O(1)
```

---

# 20. ✏️ setCharAt()

`setCharAt()` replaces the character at a specified index.

Example:

```java
StringBuilder sb = new StringBuilder("Java");

sb.setCharAt(0, 'K');

System.out.println(sb);
```

Output:

```text
Kava
```

### Important Difference

`String` does not provide `setCharAt()` because it is immutable.

`StringBuilder` provides it because it is mutable.

### Complexity

```text
setCharAt()
   ↓
O(1)
```

---

# 21. ✂️ substring()

`StringBuilder` provides `substring()` methods.

Example:

```java
StringBuilder sb = new StringBuilder("Java Programming");

String result = sb.substring(5);

System.out.println(result);
```

Output:

```text
Programming
```

### Important

`substring()` returns:

```java
String
```

not:

```java
StringBuilder
```

Another example:

```java
StringBuilder sb = new StringBuilder("Java Programming");

String result = sb.substring(0, 4);

System.out.println(result);
```

Output:

```text
Java
```

### Return Type

```text
StringBuilder
     │
     │ substring()
     ↓
   String
```

---

# 22. 🔎 indexOf()

`indexOf()` returns the index of the first occurrence of a substring.

Example:

```java
StringBuilder sb = new StringBuilder("Java Programming");

int index = sb.indexOf("Programming");

System.out.println(index);
```

Output:

```text
5
```

If the substring is not found:

```java
StringBuilder sb = new StringBuilder("Java");

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
StringBuilder sb = new StringBuilder("Java Java");

System.out.println(sb.lastIndexOf("Java"));
```

Output:

```text
5
```

The occurrences begin at:

```text
0
5
```

Therefore, the last occurrence starts at index `5`.

---

# 24. 📏 setLength()

`setLength()` changes the logical length of the StringBuilder.

Example:

```java
StringBuilder sb = new StringBuilder("Java");

sb.setLength(2);

System.out.println(sb);
```

Output:

```text
Ja
```

### Increasing Length

You can also increase the logical length.

Example:

```java
StringBuilder sb = new StringBuilder("Java");

sb.setLength(6);

System.out.println(sb.length());
```

Output:

```text
6
```

The newly added positions contain the null character:

```text
'\u0000'
```

### Important

`setLength()` changes the logical length.

It does not necessarily reduce the capacity.

---

# 25. 📦 ensureCapacity()

`ensureCapacity()` ensures that the capacity is at least the requested minimum.

Example:

```java
StringBuilder sb = new StringBuilder();

sb.ensureCapacity(100);

System.out.println(sb.capacity());
```

The capacity will be at least `100`.

### Why Use It?

If you know approximately how much data will be appended, ensuring capacity beforehand can reduce repeated expansions.

---

# 26. ✂️ trimToSize()

`trimToSize()` attempts to reduce capacity to the current length.

Example:

```java
StringBuilder sb = new StringBuilder(100);

sb.append("Java");

System.out.println(sb.capacity());

sb.trimToSize();

System.out.println(sb.capacity());
```

Typical output:

```text
100
4
```

### Important

`trimToSize()` is useful when minimizing unused internal capacity matters.

It should not be used blindly because future modifications may require the builder to grow again.

---

# 27. 🔤 toString()

`toString()` converts the contents of the StringBuilder into a String.

Example:

```java
StringBuilder sb = new StringBuilder("Java");

String result = sb.toString();

System.out.println(result);
```

Output:

```text
Java
```

Now:

```text
sb
 ↓
StringBuilder

result
 ↓
String
```

### Why Is It Important?

Many APIs expect a `String`.

Therefore, after finishing string construction, you commonly write:

```java
String result = sb.toString();
```

---

# 28. 🔗 Chaining Methods

Many modifying StringBuilder methods return the same StringBuilder object.

Example:

```java
StringBuilder sb = new StringBuilder();

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
StringBuilder sb = new StringBuilder("Java");

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
same StringBuilder
       ↓
.append(...)
       ↓
same StringBuilder
```

### Common Mutating Methods That Return StringBuilder

```text
append()
insert()
delete()
deleteCharAt()
replace()
reverse()
```

---

# 29. ⚡ StringBuilder and `+` Operator

Consider:

```java
String result = "";

for (int i = 0; i < 10000; i++) {
    result += i;
}
```

Repeated concatenation can involve repeated creation of String results.

Using StringBuilder explicitly:

```java
StringBuilder sb = new StringBuilder();

for (int i = 0; i < 10000; i++) {
    sb.append(i);
}

String result = sb.toString();
```

### Modern Java Note

Do not blindly say:

> "`+` always creates a new String object for every concatenation."

The compiler and runtime can optimize string concatenation, especially simple expressions.

For example:

```java
String result = "Hello " + name + "!";
```

Modern Java can translate string concatenation efficiently.

However, explicit `StringBuilder` remains a standard choice when you are repeatedly building or modifying text, especially when the operation is inside a loop or algorithm.

---

# 30. 🧠 StringBuilder and Memory

Consider:

```java
String s = "Java";

s = s + " Programming";
```

Conceptually, because `String` is immutable:

```text
"Java"
   +
" Programming"
   ↓
new String result
```

With StringBuilder:

```java
StringBuilder sb = new StringBuilder("Java");

sb.append(" Programming");
```

Conceptually:

```text
StringBuilder
      ↓
mutable character storage
      ↓
"Java"
      ↓
append()
      ↓
"Java Programming"
```

### Important

StringBuilder does not mean:

> "No new memory is ever allocated."

It may allocate a larger internal storage area when capacity is insufficient.

The advantage is avoiding repeated immutable String reconstruction during the building process.

---

# 31. 📈 Capacity Growth

When the current capacity becomes insufficient, StringBuilder automatically grows its capacity.

The standard Java implementation uses a growth strategy based on:

```text
oldCapacity * 2 + 2
```

For example:

```text
old capacity = 16

new capacity
= 16 * 2 + 2
= 34
```

However, if the requested minimum capacity is larger, the implementation must ensure enough capacity for that requirement.

### Important Interview Point

Do not claim:

> "Every Java implementation will always use exactly 2 × old capacity + 2."

Implementation details can change between JDK versions.

The safer answer is:

> **StringBuilder automatically expands its internal capacity when necessary; the standard implementation commonly uses a roughly doubling growth strategy.**

---

# 32. ⏱️ Time Complexity

Complexity depends on the operation and whether internal resizing or character shifting is required.

| Operation | Typical Complexity |
|---|---:|
| `charAt()` | O(1) |
| `setCharAt()` | O(1) |
| `length()` | O(1) |
| `capacity()` | O(1) |
| `append()` | Amortized O(1) |
| `insert()` | O(n) |
| `delete()` | O(n) |
| `deleteCharAt()` | O(n) |
| `replace()` | O(n) |
| `reverse()` | O(n) |
| `substring()` | O(k) |
| `indexOf()` | O(n) typical |
| `lastIndexOf()` | O(n) typical |
| `toString()` | O(n) |

### Why Is append() Amortized O(1)?

Most append operations simply add characters into available capacity.

Occasionally:

```text
capacity insufficient
        ↓
grow storage
        ↓
copy characters
        ↓
append
```

That particular append can be expensive.

But averaged across many appends:

```text
append()
   ↓
Amortized O(1)
```

---

# 33. 🧵 Thread Safety

`StringBuilder` is:

> **Not synchronized and not thread-safe for concurrent modification.**

Example:

```java
StringBuilder sb = new StringBuilder();

sb.append("Hello");
```

Using one StringBuilder from multiple threads without proper synchronization can result in unsafe concurrent modifications.

For ordinary single-threaded string construction:

```text
StringBuilder
      ↓
Preferred
```

For synchronized mutable string operations:

```text
StringBuffer
```

may be considered.

---

# 34. 🆚 StringBuilder vs StringBuffer

| Feature | StringBuilder | StringBuffer |
|---|---|---|
| Mutable | Yes | Yes |
| Synchronized | No | Yes |
| Thread-safe for its synchronized operations | No | Yes |
| Single-thread performance | Generally faster | Generally slower |
| Introduced | Java 5 | Java 1.0 |
| Common use | General string building | Legacy/synchronized scenarios |

### Easy Rule

```text
StringBuilder
     ↓
Mutable + Not synchronized

StringBuffer
     ↓
Mutable + Synchronized
```

### Important

Thread safety depends on how an object is accessed.

Using `StringBuilder` from multiple threads without external synchronization is not safe for concurrent mutation.

---

# 35. 🆚 StringBuilder vs String

| Feature | String | StringBuilder |
|---|---|---|
| Mutable | ❌ | ✅ |
| Modify existing object | ❌ | ✅ |
| `append()` | ❌ | ✅ |
| `reverse()` | ❌ | ✅ |
| `setCharAt()` | ❌ | ✅ |
| Repeated modifications | Less suitable | Suitable |
| Common use | Fixed text | Dynamic text |

Example with String:

```java
String s = "Java";

s = s + " World";
```

Example with StringBuilder:

```java
StringBuilder sb = new StringBuilder("Java");

sb.append(" World");
```

---

# 36. ⚠️ Common Mistakes

## ❌ Mistake 1 — Thinking StringBuilder is immutable

Wrong:

```text
StringBuilder is immutable.
```

Correct:

```text
StringBuilder is mutable.
```

---

## ❌ Mistake 2 — Confusing length and capacity

Example:

```java
StringBuilder sb = new StringBuilder();

System.out.println(sb.length());

System.out.println(sb.capacity());
```

Output:

```text
0
16
```

Therefore:

```text
length != capacity
```

---

## ❌ Mistake 3 — Thinking substring() returns StringBuilder

Wrong assumption:

```text
substring() → StringBuilder
```

Correct:

```text
substring() → String
```

Example:

```java
StringBuilder sb = new StringBuilder("Java");

String result = sb.substring(1, 3);
```

---

## ❌ Mistake 4 — Thinking StringBuilder is thread-safe

It is not synchronized.

---

## ❌ Mistake 5 — Forgetting to call toString()

Example:

```java
StringBuilder sb = new StringBuilder();

sb.append("Java");

String result = sb.toString();
```

---

## ❌ Mistake 6 — Forgetting the range rule

For:

```text
replace()
delete()
substring()
```

the normal range convention is:

```text
start → inclusive
end   → exclusive
```

---

## ❌ Mistake 7 — Assuming every append is always O(1)

Correct:

```text
append()
→ Amortized O(1)
```

because resizing can occasionally require copying.

---

# 37. 🚨 Interview Traps

## Trap 1

```java
StringBuilder sb = new StringBuilder("Java");

sb.append(" World");

System.out.println(sb);
```

Output:

```text
Java World
```

---

## Trap 2

```java
StringBuilder sb = new StringBuilder("Java");

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
StringBuilder sb = new StringBuilder("Java");

sb.setCharAt(0, 'K');

System.out.println(sb);
```

Output:

```text
Kava
```

---

## Trap 4

```java
StringBuilder sb = new StringBuilder("Java");

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

## Trap 5

```java
StringBuilder sb = new StringBuilder();

System.out.println(sb.length());

System.out.println(sb.capacity());
```

Output:

```text
0
16
```

---

## Trap 6

```java
StringBuilder sb = new StringBuilder("Java");

sb.append(" World");

String s = sb.toString();

System.out.println(sb);
System.out.println(s);
```

Both contain:

```text
Java World
```

But:

```text
sb → StringBuilder
s  → String
```

---

## Trap 7

```java
StringBuilder sb = new StringBuilder("Java");

sb.delete(1, 3);

System.out.println(sb);
```

Output:

```text
Ja
```

Why?

Indexes `1` and `2` are deleted.

Index `3` is exclusive.

---

## Trap 8

```java
StringBuilder sb = new StringBuilder("Java");

sb.insert(1, "XX");

System.out.println(sb);
```

Output:

```text
JXXava
```

---

# 38. 🧩 DSA & Problem Solving

StringBuilder is especially useful in **string-based DSA problems** where we need to repeatedly construct, modify, or reverse characters.

---

## 38.1 🎯 DSA Patterns Related to StringBuilder

Important patterns:

```text
1. String Construction
2. Reverse String
3. Palindrome
4. Two Pointers
5. Character Replacement
6. Remove Characters
7. Build Answer Incrementally
8. Simulation
9. Stack-like Character Processing
10. Frequency-Based Construction
```

---

## 38.2 🧠 How to Think About StringBuilder in DSA

When solving a string problem, ask:

```text
Do I need to modify characters?
        │
        ├── No
        │    ↓
        │  String may be enough
        │
        └── Yes
             ↓
       Consider StringBuilder
```

Then ask:

```text
Am I repeatedly concatenating?
        │
        └── Yes
             ↓
       Prefer StringBuilder
```

For problems requiring modifications:

```text
Input String
     ↓
StringBuilder
     ↓
Modify / Build
     ↓
toString()
     ↓
Answer
```

---

# 38.3 🔥 DSA Problem 1 — Reverse a String

### Problem

Reverse a given string.

### Approach

Create a StringBuilder from the input and use `reverse()`.

### Solution

```java
class Solution {

    public String reverseString(String s) {

        StringBuilder sb = new StringBuilder(s);

        return sb.reverse().toString();
    }
}
```

### Complexity

```text
Time  → O(n)
Space → O(n)
```

### Thinking Pattern

```text
Need reverse?
     ↓
StringBuilder
     ↓
reverse()
     ↓
toString()
```

---

# 38.4 🔥 DSA Problem 2 — Check Palindrome

### Problem

Determine whether a string reads the same forward and backward.

Example:

```text
"madam" → true
"hello" → false
```

### Approach 1 — StringBuilder

```java
class Solution {

    public boolean isPalindrome(String s) {

        StringBuilder sb = new StringBuilder(s);

        String reversed = sb.reverse().toString();

        return s.equals(reversed);
    }
}
```

### Complexity

```text
Time  → O(n)
Space → O(n)
```

### Better DSA Approach — Two Pointers

For interview DSA, also understand the two-pointer solution:

```java
class Solution {

    public boolean isPalindrome(String s) {

        int left = 0;
        int right = s.length() - 1;

        while (left < right) {

            if (s.charAt(left) != s.charAt(right)) {
                return false;
            }

            left++;
            right--;
        }

        return true;
    }
}
```

### Complexity

```text
Time  → O(n)
Space → O(1)
```

### Interview Insight

StringBuilder gives a simple solution, but the two-pointer approach can achieve constant extra space.

---

# 38.5 🔥 DSA Problem 3 — Remove a Character

### Problem

Remove all occurrences of a particular character.

Example:

```text
Input:
"banana"

Remove:
'a'

Output:
"bnn"
```

### Solution

```java
class Solution {

    public String removeCharacter(String s, char target) {

        StringBuilder sb = new StringBuilder();

        for (int i = 0; i < s.length(); i++) {

            if (s.charAt(i) != target) {
                sb.append(s.charAt(i));
            }
        }

        return sb.toString();
    }
}
```

### Complexity

```text
Time  → O(n)
Space → O(n)
```

### Pattern

```text
Read character
     ↓
Check condition
     ↓
Append if valid
     ↓
Return built answer
```

This pattern appears frequently in string DSA problems.

---

# 38.6 🔥 DSA Problem 4 — Remove Vowels

### Problem

Remove all vowels from a string.

Example:

```text
Input:
"hello world"

Output:
"hll wrld"
```

### Solution

```java
class Solution {

    public String removeVowels(String s) {

        StringBuilder sb = new StringBuilder();

        for (int i = 0; i < s.length(); i++) {

            char ch = s.charAt(i);

            if (ch != 'a' &&
                ch != 'e' &&
                ch != 'i' &&
                ch != 'o' &&
                ch != 'u') {

                sb.append(ch);
            }
        }

        return sb.toString();
    }
}
```

### Complexity

```text
Time  → O(n)
Space → O(n)
```

---

# 38.7 🔥 DSA Problem 5 — Reverse Words

### Problem

Reverse the order of words.

Example:

```text
Input:
"I love Java"

Output:
"Java love I"
```

### Approach

Split the words, iterate from right to left, and construct the answer using StringBuilder.

```java
class Solution {

    public String reverseWords(String s) {

        String[] words = s.trim().split("\\s+");

        StringBuilder sb = new StringBuilder();

        for (int i = words.length - 1; i >= 0; i--) {

            sb.append(words[i]);

            if (i != 0) {
                sb.append(" ");
            }
        }

        return sb.toString();
    }
}
```

### Complexity

```text
Time  → O(n)
Space → O(n)
```

---

# 38.8 🔥 DSA Problem 6 — Build a String from Characters

### Problem

Given an array of characters, construct a String.

```java
class Solution {

    public String buildString(char[] chars) {

        StringBuilder sb = new StringBuilder();

        for (char ch : chars) {
            sb.append(ch);
        }

        return sb.toString();
    }
}
```

### Complexity

```text
Time  → O(n)
Space → O(n)
```

---

# 38.9 🔥 DSA Problem 7 — Compress Consecutive Characters

### Problem

Compress consecutive repeated characters.

Example:

```text
Input:
aaabbc

Output:
a3b2c1
```

### Approach

Use two pointers:

```text
i
↓
Start of group

j
↓
Find end of group
```

Then append the character and its frequency.

### Solution

```java
class Solution {

    public String compress(String s) {

        StringBuilder sb = new StringBuilder();

        int i = 0;

        while (i < s.length()) {

            char ch = s.charAt(i);

            int j = i;

            while (j < s.length() && s.charAt(j) == ch) {
                j++;
            }

            int count = j - i;

            sb.append(ch);
            sb.append(count);

            i = j;
        }

        return sb.toString();
    }
}
```

### Complexity

```text
Time  → O(n)
Space → O(n)
```

### Pattern

```text
Group consecutive elements
        ↓
Count group
        ↓
Append result
        ↓
Move to next group
```

---

# 38.10 🔥 DSA Problem 8 — Remove Adjacent Duplicates

### Problem

Remove adjacent duplicate characters.

Example:

```text
Input:
abbaca

Process:
abbaca
 ↓
aaca
 ↓
ca

Output:
ca
```

### StringBuilder as a Stack

A powerful idea:

> **StringBuilder can sometimes act like a character stack.**

Solution:

```java
class Solution {

    public String removeDuplicates(String s) {

        StringBuilder sb = new StringBuilder();

        for (char ch : s.toCharArray()) {

            int n = sb.length();

            if (n > 0 && sb.charAt(n - 1) == ch) {
                sb.deleteCharAt(n - 1);
            } else {
                sb.append(ch);
            }
        }

        return sb.toString();
    }
}
```

### Complexity

```text
Time  → O(n) typical
Space → O(n)
```

### DSA Pattern

```text
StringBuilder
      ↓
Treat end as stack top
      ↓
charAt(length - 1)
      ↓
Check top
      ↓
append() / deleteCharAt()
```

This is an important connection between:

```text
StringBuilder
      +
Stack pattern
      =
String DSA problems
```

---

# 38.11 🔥 DSA Problem 9 — Replace Characters

### Problem

Replace every space with `-`.

Example:

```text
Input:
"Java is fun"

Output:
"Java-is-fun"
```

### Solution

```java
class Solution {

    public String replaceSpaces(String s) {

        StringBuilder sb = new StringBuilder(s);

        for (int i = 0; i < sb.length(); i++) {

            if (sb.charAt(i) == ' ') {
                sb.setCharAt(i, '-');
            }
        }

        return sb.toString();
    }
}
```

### Complexity

```text
Time  → O(n)
Space → O(n)
```

---

# 38.12 🧠 DSA Thinking Pattern — Build Instead of Concatenate

Avoid repeatedly doing:

```java
String result = "";

for (char ch : chars) {
    result = result + ch;
}
```

Prefer:

```java
StringBuilder sb = new StringBuilder();

for (char ch : chars) {
    sb.append(ch);
}

String result = sb.toString();
```

### Mental Rule

```text
Repeated concatenation
        ↓
Think StringBuilder
```

---

# 38.13 🎯 Important String DSA Questions

Practice these:

| Problem | Main Pattern |
|---|---|
| Reverse String | StringBuilder / Two Pointers |
| Valid Palindrome | Two Pointers |
| Reverse Words | String Construction |
| Remove Vowels | Filtering |
| Remove Character | Filtering |
| Remove Adjacent Duplicates | Stack |
| String Compression | Two Pointers |
| Valid Parentheses | Stack |
| Add Strings | Digit Simulation |
| Multiply Strings | Simulation |
| Decode String | Stack |
| Backspace String Compare | Stack / Two Pointers |
| Longest Palindromic Substring | Two Pointers / DP |
| Group Anagrams | Frequency / Sorting |
| String Rotation | String Manipulation |

---

# 38.14 🧠 DSA Interview Rule

Do not use StringBuilder blindly.

Ask:

```text
What is the actual problem?
```

Then choose the pattern.

For example:

```text
Reverse
   ↓
Two pointers / reverse

Build result
   ↓
StringBuilder

Repeated removal from end
   ↓
StringBuilder can act like stack

Palindrome
   ↓
Two pointers

Frequency
   ↓
HashMap / array

Substring search
   ↓
String algorithms
```

### Key Lesson

> **StringBuilder is a tool, not a DSA pattern by itself.**

The important skill is recognizing when it can efficiently support a DSA pattern.

---

# 39. 🔥 Top 20 Interview Questions

## Q1. What is StringBuilder?

**Answer:**

`StringBuilder` is a mutable sequence of characters provided by `java.lang`. It is useful for repeatedly constructing or modifying character data.

---

## Q2. Is StringBuilder mutable?

**Answer:**

Yes.

Its character sequence can be modified after creation.

---

## Q3. Where is StringBuilder located?

**Answer:**

It belongs to:

```java
java.lang
```

Therefore, no explicit import is required.

---

## Q4. Is StringBuilder thread-safe?

**Answer:**

No.

StringBuilder is not synchronized for concurrent modifications.

---

## Q5. What is the difference between String and StringBuilder?

**Answer:**

`String` is immutable, while `StringBuilder` is mutable.

---

## Q6. Why is StringBuilder useful for repeated modifications?

**Answer:**

It allows modifications to a mutable character sequence instead of repeatedly producing new immutable String results.

---

## Q7. What is the default capacity?

**Answer:**

The default initial capacity is:

```text
16
```

---

## Q8. What is the initial capacity of `new StringBuilder("Java")`?

**Answer:**

The initial capacity is:

```text
length + 16
```

For `"Java"`:

```text
4 + 16 = 20
```

---

## Q9. Difference between length and capacity?

**Answer:**

`length()` tells us how many characters are currently present.

`capacity()` tells us the current internal storage capacity.

---

## Q10. What does append() do?

**Answer:**

It adds data to the end of the StringBuilder and returns the same builder instance.

---

## Q11. What does insert() do?

**Answer:**

It inserts data at a specified index.

---

## Q12. What does delete() do?

**Answer:**

It removes characters in a specified range.

The start index is inclusive and the end index is exclusive.

---

## Q13. What does reverse() do?

**Answer:**

It reverses the character sequence.

---

## Q14. What does setCharAt() do?

**Answer:**

It replaces a character at a specified index.

---

## Q15. What does toString() do?

**Answer:**

It converts the StringBuilder content into a String.

---

## Q16. Does substring() return StringBuilder?

**Answer:**

No.

It returns a:

```java
String
```

---

## Q17. What happens when capacity becomes insufficient?

**Answer:**

StringBuilder automatically grows its internal storage.

---

## Q18. Why is append() amortized O(1)?

**Answer:**

Most append operations use existing capacity. Occasionally, resizing and copying are required. Averaged over many operations, append is amortized O(1).

---

## Q19. Difference between StringBuilder and StringBuffer?

**Answer:**

Both are mutable character sequences.

`StringBuilder` is not synchronized, while `StringBuffer` provides synchronized methods.

---

## Q20. When should you use StringBuilder?

**Answer:**

Use it when repeatedly constructing or modifying text, especially when building strings in loops or algorithms.

---

# 40. 🎤 30-Second Interview Answer

> **StringBuilder is a mutable sequence of characters provided by the `java.lang` package. Unlike String, which is immutable, StringBuilder allows modifications such as append, insert, delete, replace, and reverse on its mutable character sequence. It is useful for repeated string construction, especially inside loops and DSA problems. StringBuilder is not synchronized, so it is generally preferred when synchronized concurrent mutation is not required.**

---

# 41. 🧾 Cheat Sheet

| Method | Purpose | Return Type |
|---|---|---|
| `length()` | Current character count | `int` |
| `capacity()` | Current capacity | `int` |
| `append()` | Add at end | `StringBuilder` |
| `insert()` | Insert at index | `StringBuilder` |
| `replace()` | Replace range | `StringBuilder` |
| `delete()` | Delete range | `StringBuilder` |
| `deleteCharAt()` | Delete one character | `StringBuilder` |
| `reverse()` | Reverse content | `StringBuilder` |
| `charAt()` | Read character | `char` |
| `setCharAt()` | Modify character | `void` |
| `substring()` | Extract portion | `String` |
| `indexOf()` | First occurrence | `int` |
| `lastIndexOf()` | Last occurrence | `int` |
| `setLength()` | Change logical length | `void` |
| `ensureCapacity()` | Ensure minimum capacity | `void` |
| `trimToSize()` | Reduce unused capacity | `void` |
| `toString()` | Convert to String | `String` |

---

# 42. 🧠 Memory Tricks

## 🔥 Modification Methods

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

Remember:

```text
C S

C → charAt()
S → setCharAt()
```

Think:

```text
charAt()
   ↓
Read

setCharAt()
   ↓
Change
```

---

## 🔥 Capacity

Remember:

```text
L ≠ C

L → length
C → capacity
```

Think:

```text
length
   ↓
How many characters?

capacity
   ↓
How much internal storage?
```

---

## 🔥 Conversion

Remember:

```text
StringBuilder
      ↓
toString()
      ↓
String
```

---

## 🔥 DSA

Remember:

```text
Build
   ↓
StringBuilder

Reverse
   ↓
reverse() / Two Pointers

Modify
   ↓
setCharAt()

Remove from end
   ↓
deleteCharAt()

Stack-like processing
   ↓
StringBuilder
```

---

# 43. 🔗 Next Topic

String playlist:

```text
04-Strings/
│
├── 01-String-Introduction.md
├── 02-String-Pool.md
├── 03-String-Immutability.md
├── 04-String-Methods.md
├── 05-StringBuilder.md       ← YOU ARE HERE
├── 06-StringBuffer.md
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

# 44. 🚀 Final Revision

Before moving to StringBuffer, make sure you can explain:

```text
1. What is StringBuilder?
2. Why is it mutable?
3. Why is it useful for repeated modifications?
4. What is the default capacity?
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
25. StringBuilder in DSA
26. StringBuilder as a stack-like structure
27. StringBuilder vs two-pointer solutions
```

---

# ⭐ Core Idea

> **String is immutable, while StringBuilder is mutable. When you need to repeatedly construct or modify text, StringBuilder allows you to work with a mutable character sequence instead of repeatedly creating new String results.**

### 🧠 DSA Core Idea

> **Use StringBuilder when your algorithm repeatedly builds or modifies a string. But always identify the underlying DSA pattern first—such as two pointers, stack, filtering, simulation, or frequency counting.**

---

# 🏆 One-Line Interview Memory

```text
StringBuilder = Mutable + Expandable + Not Synchronized + Efficient String Construction
```