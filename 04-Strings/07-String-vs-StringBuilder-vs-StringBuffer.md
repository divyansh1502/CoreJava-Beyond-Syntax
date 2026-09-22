````md
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
26. [DSA Relevance](#26--dsa-relevance)
27. [DSA Patterns](#27--dsa-patterns)
28. [DSA Practice Questions](#28--dsa-practice-questions)
29. [Top 25 Interview Questions](#29--top-25-interview-questions)
30. [30-Second Interview Answer](#30--30-second-interview-answer)
31. [1-Minute Interview Answer](#31--1-minute-interview-answer)
32. [Cheat Sheet](#32--cheat-sheet)
33. [Memory Tricks](#33--memory-tricks)
34. [Final Revision](#34--final-revision)
35. [Next Topic](#35--next-topic)

---

# 1. 🔤 Introduction

Java provides three important classes for working with character data:

- `String`
- `StringBuilder`
- `StringBuffer`

All three represent sequences of characters, but they are designed for different situations.

The fundamental difference is:

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

### Core Comparison

| Type | Mutable | Synchronized | Main Use |
|---|---|---|---|
| `String` | ❌ | Not applicable | Immutable text |
| `StringBuilder` | ✅ | ❌ | Mutable text construction |
| `StringBuffer` | ✅ | ✅ | Synchronized mutable operations |

---

# 2. 🌳 The Big Picture

Think of them like this:

```text
                    CHARACTER DATA
                         │
              ┌──────────┼──────────┐
              ↓          ↓          ↓
           String    StringBuilder StringBuffer
              │          │          │
         Immutable     Mutable     Mutable
                         │          │
                    Not Sync.    Synchronized
```

### Easy Memory Trick

```text
String
    ↓
Immutable

StringBuilder
    ↓
Mutable
    ↓
No synchronization

StringBuffer
    ↓
Mutable
    ↓
Synchronization
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

Once a String object is created, its character content cannot be changed.

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

It may look like `"Java"` was modified.

It was not.

Conceptually:

```text
Before:

s
│
↓
"Java"


After:

s
│
↓
"Java Programming"
```

A new String result is created for the concatenation.

The original `"Java"` object remains unchanged.

---

# 4. 🏗️ StringBuilder

`StringBuilder` is a mutable sequence of characters.

It belongs to:

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

The existing `StringBuilder` object is modified.

## Why StringBuilder?

Suppose we repeatedly modify text:

```text
append()
append()
append()
append()
```

Using immutable `String` values repeatedly can involve creating intermediate String results.

`StringBuilder` is designed for efficient mutable string construction.

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

> **StringBuffer provides synchronized methods.**

Therefore:

```text
StringBuffer
    ↓
Mutable
    +
Synchronized methods
```

---

# 6. 🔄 Mutability

## What Does Mutable Mean?

Mutable means:

> The object's state/content can be changed after the object has been created.

---

## String

```java
String s = "Java";
```

`String` is immutable.

Its existing object's content cannot be modified.

---

## StringBuilder

```java
StringBuilder sb = new StringBuilder("Java");

sb.append(" Developer");
```

The same mutable object can be modified.

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

The same mutable object can be modified.

Therefore:

```text
StringBuffer → Mutable
```

---

## Summary

```text
String
    ↓
Immutable

StringBuilder
    ↓
Mutable

StringBuffer
    ↓
Mutable
```

---

# 7. 🧵 Thread Safety

Thread safety becomes important when multiple threads access shared mutable data.

## String

String is immutable.

Once a String object exists, its character contents cannot be changed.

Therefore immutable String objects are safe to share in many situations.

---

## StringBuilder

`StringBuilder` is not synchronized.

Therefore, it should not be treated as thread-safe for concurrent modification of the same instance.

It is generally intended for situations where synchronization is not required.

---

## StringBuffer

StringBuffer provides synchronized methods.

Therefore, individual method operations on a shared StringBuffer instance are synchronized.

---

## Simple Comparison

```text
String
    ↓
Immutable

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

# 8. 🔒 Synchronization

Synchronization coordinates access to shared mutable state between threads.

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

When synchronized methods are invoked on the same object, access is coordinated through the object's monitor.

## Important Interview Point

Do **not** say:

> StringBuffer makes every multi-step operation atomic.

That is incorrect.

Example:

```java
if (sb.length() > 0) {
    sb.deleteCharAt(0);
}
```

The two method calls together are not automatically one atomic operation.

Individual synchronized methods do not automatically make an entire sequence of operations atomic.

---

# 9. ⚡ Performance

In general:

```text
StringBuilder
    ↓
Less synchronization overhead

StringBuffer
    ↓
More synchronization overhead
```

Therefore, when synchronization is unnecessary:

```text
StringBuilder
```

is generally preferred.

When synchronized mutable operations are specifically required:

```text
StringBuffer
```

may be appropriate.

### Important

Do not say:

> StringBuilder is always faster.

Better interview answer:

> StringBuilder generally has less overhead than StringBuffer because it does not synchronize its methods.

Actual performance depends on the workload and implementation.

---

# 10. 🧠 Memory Behavior

## String

Repeated modification can produce new String results.

Example:

```java
String result = "";

result = result + "Java";
result = result + " ";
result = result + "Developer";
```

Conceptually, multiple String objects/results may be involved.

---

## StringBuilder

A mutable internal buffer is used.

Example:

```java
StringBuilder result = new StringBuilder();

result.append("Java");
result.append(" Developer");
```

The existing builder can modify and grow its internal storage.

---

## StringBuffer

StringBuffer also maintains mutable internal storage.

The major difference is that its methods provide synchronization.

---

# 11. 🏊 String Pool

String literals can be stored in the String Pool.

Example:

```java
String s1 = "Java";
String s2 = "Java";

System.out.println(s1 == s2);
```

Output:

```text
true
```

Both references can refer to the same pooled String object.

Conceptually:

```text
s1 ─────┐
        ↓
      "Java"
   String Pool
        ↑
s2 ─────┘
```

## StringBuilder and StringBuffer

They are mutable objects created separately.

Example:

```java
StringBuilder builder = new StringBuilder("Java");
StringBuffer buffer = new StringBuffer("Java");
```

The String argument `"Java"` may be a pooled String, but the `StringBuilder` and `StringBuffer` objects themselves are separate objects.

---

# 12. 🛠️ Common Methods

StringBuilder and StringBuffer provide many similar methods.

Important methods include:

| Method | Purpose |
|---|---|
| `append()` | Add data at the end |
| `insert()` | Insert data |
| `replace()` | Replace a range |
| `delete()` | Delete a range |
| `deleteCharAt()` | Delete one character |
| `reverse()` | Reverse content |
| `charAt()` | Read a character |
| `setCharAt()` | Modify one character |
| `length()` | Current character count |
| `capacity()` | Current capacity |
| `substring()` | Extract a String |
| `indexOf()` | Find first occurrence |
| `lastIndexOf()` | Find last occurrence |
| `setLength()` | Change logical length |
| `ensureCapacity()` | Ensure minimum capacity |
| `trimToSize()` | Reduce unused capacity |
| `toString()` | Convert to String |

## String

Important String methods include:

```text
length()
charAt()
substring()
indexOf()
lastIndexOf()
equals()
equalsIgnoreCase()
startsWith()
endsWith()
contains()
replace()
replaceAll()
split()
trim()
strip()
toLowerCase()
toUpperCase()
concat()
```

String modification methods do not modify the original String.

They return a new String when a changed result is required.

---

# 13. ➕ append() Comparison

## String

String does not provide an `append()` method.

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
    ↓
+ or concat()

StringBuilder
    ↓
append()

StringBuffer
    ↓
append()
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

System.out.println(sb);
```

Output:

```text
Kava
```

The mutable object is modified.

---

## StringBuffer

```java
StringBuffer sb = new StringBuffer("Java");

sb.setCharAt(0, 'K');

System.out.println(sb);
```

Output:

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

There is no special direct conversion method required.

A simple approach is:

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

- Text does not need frequent modification.
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

Typical examples:

```text
User name
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

## Very Common Use Case

Generating a large String:

```java
StringBuilder result = new StringBuilder();

for (int i = 0; i < 1000; i++) {
    result.append(i);
}

String output = result.toString();
```

This is a very common DSA and backend pattern.

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

Suppose multiple threads need synchronized operations on the same mutable character buffer.

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

Repeated concatenation can involve creating many intermediate String results.

A mutable builder is generally more suitable:

```java
StringBuilder result = new StringBuilder();

for (int i = 0; i < 1000; i++) {
    result.append(i);
}
```

This is generally a better pattern for explicit repeated string construction.

## Important Compiler Note

Simple String concatenation expressions may be optimized by the compiler/runtime.

Therefore, do not claim:

> Every `+` operation always creates a new String object in exactly the same way.

For explicit repeated string construction, StringBuilder is the standard choice.

---

# 21. 🌳 Decision Tree

Use this decision process:

```text
Do I need text?
      │
      ↓
Is frequent modification required?
      │
   ┌──┴──┐
  NO     YES
  │       │
  ↓       ↓
String   Is synchronization required?
             │
          ┌──┴──┐
         NO     YES
         │       │
         ↓       ↓
 StringBuilder StringBuffer
```

### One-Line Decision

```text
Fixed text
    → String

Frequent modification
    → StringBuilder

Frequent modification + synchronized methods
    → StringBuffer
```

---

# 22. 📊 Detailed Comparison

| Feature | String | StringBuilder | StringBuffer |
|---|---|---|---|
| Package | `java.lang` | `java.lang` | `java.lang` |
| Mutable | ❌ | ✅ | ✅ |
| Immutable | ✅ | ❌ | ❌ |
| Synchronized methods | Not applicable | ❌ | ✅ |
| Shared concurrent mutation | Safe due to immutability | Not synchronized | Individual methods synchronized |
| Repeated modification | Less suitable | Generally preferred | More overhead |
| String Pool | String literals can use pool | Object itself is not pooled | Object itself is not pooled |
| `append()` | ❌ | ✅ | ✅ |
| `insert()` | ❌ | ✅ | ✅ |
| `delete()` | ❌ | ✅ | ✅ |
| `reverse()` | ❌ | ✅ | ✅ |
| `setCharAt()` | ❌ | ✅ | ✅ |
| `toString()` | Already String | Converts to String | Converts to String |
| General use | Immutable text | Mutable text | Synchronized mutable text |

---

# 23. ⚖️ Advantages and Disadvantages

## String

### ✅ Advantages

- Immutable
- Safe to share
- String Pool support
- Rich String API
- Excellent for fixed text
- Useful as keys in hash-based collections
- Stable hash code because its contents cannot change

### ❌ Disadvantages

- Repeated modification can create intermediate String results
- Not designed for repeated character modifications

---

## StringBuilder

### ✅ Advantages

- Mutable
- Efficient for repeated modifications
- Generally less overhead than StringBuffer
- Excellent for loops and text construction
- Simple API
- Very useful in DSA string-building problems

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
- Generally more overhead than StringBuilder
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
StringBuilder generally has less synchronization overhead.
```

---

## ❌ Mistake 5

> String is a primitive data type.

Wrong.

Correct:

```text
String → Class
```

---

## ❌ Mistake 6

> StringBuilder and StringBuffer objects are stored in the String Pool.

Wrong.

Their mutable objects are ordinary objects.

A String argument passed to their constructors may itself be a pooled String.

---

## ❌ Mistake 7

> Thread-safe means every multi-operation sequence is atomic.

Wrong.

Individual synchronized methods do not automatically make a sequence of method calls atomic.

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

Which one provides synchronized methods?

```text
StringBuffer
```

---

## Trap 5

Which one is generally preferred for repeated modification when synchronization is unnecessary?

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

Which one has no built-in method synchronization?

```text
StringBuilder
```

---

## Trap 8

Does StringBuffer make this entire operation atomic?

```java
if (buffer.length() > 0) {
    buffer.deleteCharAt(0);
}
```

Answer:

```text
No.
```

The individual methods are synchronized, but the complete check-then-act sequence is not automatically atomic.

---

# 26. 🧩 DSA Relevance

StringBuilder is **highly relevant to DSA**, especially when solving String, Array, Two Pointer, Sliding Window, Backtracking, Recursion, and construction problems.

The main reason is:

> DSA problems frequently require building or modifying strings repeatedly.

Using immutable String concatenation unnecessarily can create extra objects and make the solution less efficient.

### Important DSA Principle

```text
Repeated string construction
        ↓
Prefer mutable builder
        ↓
StringBuilder
```

---

## Why StringBuilder Matters in DSA

Suppose we need to build a result character-by-character.

Instead of:

```java
String result = "";

for (char ch : chars) {
    result += ch;
}
```

Prefer:

```java
StringBuilder result = new StringBuilder();

for (char ch : chars) {
    result.append(ch);
}
```

At the end:

```java
String answer = result.toString();
```

---

## Common DSA Situations

StringBuilder is useful for:

- Reversing a String
- Building a palindrome result
- Constructing output strings
- Backtracking
- Generating permutations
- Generating subsets
- Removing characters
- Reconstructing paths
- Building encoded strings
- Building decoded strings
- Formatting answers
- Simulation problems

---

# 27. 🧠 DSA Patterns

## Pattern 1 — Reverse String

```java
String s = "hello";

StringBuilder sb = new StringBuilder(s);

sb.reverse();

String answer = sb.toString();

System.out.println(answer);
```

Output:

```text
olleh
```

### Complexity

```text
Time:  O(n)
Space: O(n)
```

---

## Pattern 2 — Build Result in a Loop

```java
StringBuilder result = new StringBuilder();

for (int i = 0; i < 10; i++) {
    result.append(i);
}

return result.toString();
```

### Complexity

Approximately:

```text
Time:  O(n)
Space: O(n)
```

---

## Pattern 3 — Remove Last Character

A very common DSA/backtracking operation:

```java
StringBuilder sb = new StringBuilder("Java");

sb.deleteCharAt(sb.length() - 1);

System.out.println(sb);
```

Output:

```text
Jav
```

This is useful when implementing backtracking.

---

## Pattern 4 — Backtracking

A common pattern is:

```java
void backtrack(StringBuilder path) {

    if (/* base condition */) {
        System.out.println(path);
        return;
    }

    path.append('A');

    backtrack(path);

    path.deleteCharAt(path.length() - 1);
}
```

The important idea is:

```text
Choose
  ↓
Modify path
  ↓
Recurse
  ↓
Undo modification
```

This is one of the most important StringBuilder patterns in DSA.

---

## Pattern 5 — Character Frequency Result

When constructing a result from frequency information:

```java
StringBuilder result = new StringBuilder();

for (int i = 0; i < 26; i++) {

    for (int count = 0; count < frequency[i]; count++) {
        result.append((char) ('a' + i));
    }
}

return result.toString();
```

This pattern appears in sorting/counting/string-construction problems.

---

# 28. 🏆 DSA Practice Questions

## Q1. Reverse a String

**Problem:**

Given a String, return its reverse.

### Solution

```java
class Solution {

    public String reverse(String s) {

        StringBuilder sb = new StringBuilder(s);

        return sb.reverse().toString();
    }
}
```

### Complexity

```text
Time:  O(n)
Space: O(n)
```

---

## Q2. Reverse Words in a String

**Problem:**

Reverse the order of words.

Example:

```text
Input:
"Java is powerful"

Output:
"powerful is Java"
```

### Approach

1. Split words.
2. Traverse from right to left.
3. Build the answer using StringBuilder.

```java
String[] words = s.trim().split("\\s+");

StringBuilder result = new StringBuilder();

for (int i = words.length - 1; i >= 0; i--) {

    result.append(words[i]);

    if (i != 0) {
        result.append(" ");
    }
}

return result.toString();
```

### Complexity

```text
Time:  O(n)
Space: O(n)
```

---

## Q3. Build String From Character Array

```java
char[] chars = {'J', 'a', 'v', 'a'};

StringBuilder sb = new StringBuilder();

for (char ch : chars) {
    sb.append(ch);
}

String result = sb.toString();
```

### Complexity

```text
Time:  O(n)
Space: O(n)
```

---

## Q4. Remove Last Character During Backtracking

```java
StringBuilder path = new StringBuilder();

path.append('A');
path.append('B');

System.out.println(path);

path.deleteCharAt(path.length() - 1);

System.out.println(path);
```

Output:

```text
AB
A
```

### Pattern

```text
append()
   ↓
Explore
   ↓
deleteCharAt(length - 1)
   ↓
Backtrack
```

---

## Q5. Check Palindrome

```java
public boolean isPalindrome(String s) {

    StringBuilder reversed = new StringBuilder(s);

    reversed.reverse();

    return s.equals(reversed.toString());
}
```

### Complexity

```text
Time:  O(n)
Space: O(n)
```

### Better DSA Approach

For a simple palindrome check, StringBuilder is not necessary.

Two pointers can achieve:

```text
Time:  O(n)
Space: O(1)
```

Example:

```java
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
```

### Interview Insight

> StringBuilder can simplify implementation, but the best DSA solution depends on the required space complexity.

---

# 29. 🔥 Top 25 Interview Questions

## Q1. What is the main difference between String, StringBuilder and StringBuffer?

**Answer:**

```text
String
    → Immutable

StringBuilder
    → Mutable + Not synchronized

StringBuffer
    → Mutable + Synchronized
```

---

## Q2. Which one is immutable?

**Answer:**

```text
String
```

---

## Q3. Which ones are mutable?

**Answer:**

```text
StringBuilder
StringBuffer
```

---

## Q4. Which one provides synchronized methods?

**Answer:**

```text
StringBuffer
```

---

## Q5. Is StringBuilder thread-safe?

**Answer:**

No. It does not synchronize its methods for concurrent access to the same instance.

---

## Q6. Is StringBuffer thread-safe?

**Answer:**

Its methods are synchronized, providing thread-safe individual operations on the same instance.

---

## Q7. Which is generally faster: StringBuilder or StringBuffer?

**Answer:**

StringBuilder is generally faster because it does not have synchronization overhead.

---

## Q8. Why is StringBuilder faster?

**Answer:**

Because its methods are not synchronized.

---

## Q9. Why can StringBuffer have more overhead?

**Answer:**

Synchronization can introduce additional overhead.

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

Use StringBuffer when mutable string data requires synchronized individual operations.

---

## Q13. What does mutable mean?

**Answer:**

It means the object's state/content can be modified after creation.

---

## Q14. What does immutable mean?

**Answer:**

It means the object's state/content cannot be changed after creation.

---

## Q15. Why is String useful as a HashMap key?

**Answer:**

String is immutable, so its equality-relevant content and hash code do not change after insertion.

---

## Q16. Can StringBuilder be converted to String?

**Answer:**

Yes.

```java
StringBuilder sb = new StringBuilder("Java");

String s = sb.toString();
```

---

## Q17. Can StringBuffer be converted to String?

**Answer:**

Yes.

```java
StringBuffer sb = new StringBuffer("Java");

String s = sb.toString();
```

---

## Q18. Can String be converted to StringBuilder?

**Answer:**

Yes.

```java
String s = "Java";

StringBuilder sb = new StringBuilder(s);
```

---

## Q19. Can String be converted to StringBuffer?

**Answer:**

Yes.

```java
String s = "Java";

StringBuffer sb = new StringBuffer(s);
```

---

## Q20. Are StringBuilder and StringBuffer objects stored in the String Pool?

**Answer:**

No.

Their mutable objects are ordinary objects.

A String argument passed to their constructors may itself be a pooled String.

---

## Q21. Why is String immutable?

**Answer:**

Immutability provides benefits such as:

- Safe sharing
- String Pool support
- Stable hash codes
- Easier reasoning about String values
- Better security characteristics for immutable textual values

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

The compiler/runtime can optimize some concatenation expressions.

However, for explicit repeated string construction, StringBuilder is generally appropriate.

---

## Q25. Give the simplest way to remember all three.

**Answer:**

```text
String
    → Immutable

StringBuilder
    → Mutable + No synchronization

StringBuffer
    → Mutable + Synchronization
```

---

# 30. 🎤 30-Second Interview Answer

> **String is immutable, meaning its content cannot be changed after creation. StringBuilder and StringBuffer are mutable classes designed for modifying character sequences. StringBuilder is not synchronized and is generally preferred when synchronization is unnecessary because it has less overhead. StringBuffer provides synchronized methods and can be useful when synchronized access to a shared mutable buffer is required.**

---

# 31. 🎤 1-Minute Interview Answer

> **Java provides String, StringBuilder, and StringBuffer for working with text. String is immutable, so modifications produce a new String result rather than changing the existing object. StringBuilder and StringBuffer are mutable, so operations such as append, insert, delete, and reverse can modify the same object. The main difference between StringBuilder and StringBuffer is synchronization. StringBuilder is not synchronized and generally has less overhead, making it suitable for most string-building tasks where synchronization is unnecessary. StringBuffer has synchronized methods, so it can be useful for shared mutable string operations where synchronized access is appropriate.**

---

# 32. 🧾 Cheat Sheet

| Feature | String | StringBuilder | StringBuffer |
|---|---|---|---|
| Mutable | ❌ | ✅ | ✅ |
| Immutable | ✅ | ❌ | ❌ |
| Synchronized | Not applicable | ❌ | ✅ |
| String Pool | ✅ | ❌ | ❌ |
| `append()` | ❌ | ✅ | ✅ |
| `insert()` | ❌ | ✅ | ✅ |
| `delete()` | ❌ | ✅ | ✅ |
| `reverse()` | ❌ | ✅ | ✅ |
| `setCharAt()` | ❌ | ✅ | ✅ |
| `toString()` | Already String | ✅ | ✅ |
| Repeated modification | Less suitable | Excellent | Good |
| Synchronization overhead | Not applicable | None from method synchronization | Yes |
| Common DSA usage | Moderate | High | Low |
| General choice for building | ❌ | ✅ | Only when synchronization is specifically needed |

---

# 33. 🧠 Memory Tricks

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
Build
    ↓
Mutable
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

StringBuilder = Mutable + No Synchronization

StringBuffer = Mutable + Synchronization
```

---

## 🔥 Trick 4 — Modification

```text
String
    ↓
New result

StringBuilder
    ↓
Modify existing object

StringBuffer
    ↓
Modify existing object
+
Synchronization
```

---

# 34. 🚀 Final Revision

Before moving to the next topic, you should be able to explain:

- What is String?
- What is StringBuilder?
- What is StringBuffer?
- Which one is immutable?
- Which ones are mutable?
- Which one provides synchronized methods?
- Why is StringBuilder generally faster than StringBuffer?
- Why does StringBuffer have synchronization overhead?
- When should String be used?
- When should StringBuilder be used?
- When should StringBuffer be used?
- What is the String Pool?
- Why aren't StringBuilder objects in the String Pool?
- How do you convert StringBuilder to String?
- How do you convert StringBuffer to String?
- What does thread-safe mean?
- Does synchronization make every operation sequence atomic?
- Why is String useful as a HashMap key?
- What happens during repeated String modification?
- Why is StringBuilder commonly used inside loops?
- Why is StringBuilder important in DSA?
- How can StringBuilder be used in backtracking?
- What is the complexity of reversing a String using StringBuilder?
- When is a two-pointer solution better than StringBuilder?

---

# ⭐ Most Important Comparison

| Requirement | Concept |
|---|---|
| Fixed / immutable text | `String` |
| Frequent modifications | `StringBuilder` |
| Frequent modifications + synchronized methods | `StringBuffer` |
| String literals / String Pool | `String` |
| General string building | `StringBuilder` |
| DSA string construction | `StringBuilder` |
| DSA backtracking path | `StringBuilder` |
| Shared mutable buffer requiring synchronized individual operations | `StringBuffer` |

---

# 🧩 DSA Quick Revision

```text
String
    ↓
Immutable
    ↓
Good for reading/comparing fixed text

StringBuilder
    ↓
Mutable
    ↓
append / delete / reverse
    ↓
Excellent for construction
    ↓
Very useful in DSA

StringBuffer
    ↓
Mutable
    ↓
Synchronized methods
    ↓
Less common in DSA
```

### DSA Complexity Reminder

For a String of length `n`:

```text
StringBuilder.reverse()
    Time  → O(n)
    Space → O(n)
```

Building a result with `n` appended characters is generally:

```text
Time  → O(n)
Space → O(n)
```

Backtracking pattern:

```text
append()
   ↓
recurse()
   ↓
deleteCharAt(length - 1)
   ↓
continue
```

This pattern is extremely important for:

- Permutations
- Combinations
- Subsets
- Parentheses generation
- Path construction
- Recursive string generation

---

# 🎯 Final Interview Line

> **String is immutable, StringBuilder is mutable and unsynchronized, and StringBuffer is mutable with synchronized methods. StringBuilder is generally preferred for repeated string manipulation when synchronization is not required, while StringBuffer is useful when synchronized access to a shared mutable buffer is specifically needed.**

---

# 35. 🔗 Next Topic

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
DSA Applications
        ↓
Interview Questions
```

---

# 🏆 FINAL MEMORY CARD

```text
┌──────────────────────────────────────┐
│              STRING                  │
│                                      │
│           IMMUTABLE                  │
│           String Pool                │
│           Fixed Text                 │
└──────────────────────────────────────┘

                  VS

┌──────────────────────────────────────┐
│          STRINGBUILDER               │
│                                      │
│           MUTABLE                    │
│           NOT SYNCHRONIZED           │
│           GENERALLY LESS OVERHEAD     │
│           ⭐ DSA FRIENDLY             │
└──────────────────────────────────────┘

                  VS

┌──────────────────────────────────────┐
│           STRINGBUFFER               │
│                                      │
│           MUTABLE                    │
│           SYNCHRONIZED               │
│           MORE OVERHEAD              │
└──────────────────────────────────────┘
```

> 💡 **Remember:**

> `String` → **Immutable**

> `StringBuilder` → **Mutable + No Synchronization**

> `StringBuffer` → **Mutable + Synchronization**

> ⭐ **DSA Rule:** When repeatedly constructing or modifying a string, think **StringBuilder** first unless a specific requirement says otherwise.
````
