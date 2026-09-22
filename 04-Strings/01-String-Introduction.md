# 🔤 String — Introduction

> **A String in Java is an object that represents a sequence of characters. `String` is a `final` class from `java.lang`, and String objects are immutable.**

---

# 📌 Table of Contents

1. [What is a String?](#1--what-is-a-string)
2. [Why Do We Need String?](#2--why-do-we-need-string)
3. [String as a Class](#3--string-as-a-class)
4. [String Is Not a Primitive](#4--string-is-not-a-primitive)
5. [Creating Strings](#5--creating-strings)
6. [String Literal](#6--string-literal)
7. [Creating String Using `new`](#7--creating-string-using-new)
8. [String Characteristics](#8--string-characteristics)
9. [String Immutability](#9--string-immutability)
10. [String Concatenation](#10--string-concatenation)
11. [String Length](#11--string-length)
12. [String and `char[]`](#12--string-and-char)
13. [`==` vs `equals()`](#13--vs-equals)
14. [Basic Memory View](#14--basic-memory-view)
15. [Why Is String Special?](#15--why-is-string-special)
16. [Important String Methods — Preview](#16--important-string-methods--preview)
17. [String and Unicode](#17--string-and-unicode)
18. [String and `null`](#18--string-and-null)
19. [Common Mistakes](#19--common-mistakes)
20. [Interview Traps](#20--interview-traps)
21. [Top 15 Interview Questions](#21--top-15-interview-questions)
22. [DSA with Strings](#22--dsa-with-strings)
23. [How to Identify String DSA Patterns](#23--how-to-identify-string-dsa-patterns)
24. [Important String DSA Patterns](#24--important-string-dsa-patterns)
25. [Important String DSA Questions](#25--important-string-dsa-questions)
26. [String DSA Snippets](#26--string-dsa-snippets)
27. [30-Second Interview Answer](#27--30-second-interview-answer)
28. [Cheat Sheet](#28--cheat-sheet)
29. [Memory Tricks](#29--memory-tricks)
30. [Next Topics](#30--next-topics)

---

# 1. 🔹 What is a String?

A **String** is a sequence of characters.

Example:

```java
String name = "Divyansh";
```

The value:

```text
D i v y a n s h
```

represents textual data.

In Java, String is represented by:

```java
java.lang.String
```

class.

Because `java.lang` is automatically imported, we normally write:

```java
String name = "Java";
```

instead of:

```java
java.lang.String name = "Java";
```

### 🎤 Interview Definition

> **String is a final class in the `java.lang` package that represents a sequence of characters. String objects are immutable.**

---

# 2. 🎯 Why Do We Need String?

Programs constantly work with textual data.

| Data | Example |
|---|---|
| Name | `"Divyansh"` |
| City | `"Lucknow"` |
| Email | `"user@gmail.com"` |
| Password | `"abc123"` |
| URL | `"https://example.com"` |
| Message | `"Hello Java"` |
| File Path | `"C:/Users/Admin"` |
| JSON | `"{\"name\":\"John\"}"` |
| User Input | `"Java Developer"` |

Without String, we would have to manually manage every character.

For example:

```java
char c1 = 'J';
char c2 = 'a';
char c3 = 'v';
char c4 = 'a';
```

Instead:

```java
String language = "Java";
```

This makes handling text much easier.

### 🧠 In Short

```text
String
  ↓
Used to represent textual data
  ↓
Sequence of characters
  ↓
Very common in almost every Java application
```

---

# 3. 🧱 String as a Class

One of the most important things to remember:

> **`String` is a class, not a primitive data type.**

Its fully qualified name is:

```java
java.lang.String
```

Consider:

```java
String name = "Java";
```

Conceptually:

```text
String
   ↓
Class / Reference Type

name
   ↓
Reference Variable

"Java"
   ↓
String Object
```

## 🔍 Package of String

String belongs to:

```java
java.lang
```

The `java.lang` package is automatically imported by Java.

Therefore:

```java
String name = "Java";
```

works without manually writing:

```java
import java.lang.String;
```

---

# 4. ❌ String Is Not a Primitive

Java has exactly **8 primitive data types**:

| Primitive | Example |
|---|---|
| `byte` | `10` |
| `short` | `100` |
| `int` | `1000` |
| `long` | `10000L` |
| `float` | `10.5f` |
| `double` | `10.5` |
| `char` | `'A'` |
| `boolean` | `true` |

`String` is not one of them.

```java
String name = "Java";
```

`String` is a **reference type**.

## 🆚 `char` vs `String`

```java
char c = 'A';
```

`char`:

- Primitive
- Represents one UTF-16 code unit
- Uses single quotes

Whereas:

```java
String s = "A";
```

`String`:

- Reference type
- Represents a sequence of characters/code units
- Uses double quotes
- Is an object

| Feature | `char` | `String` |
|---|---|---|
| Type | Primitive | Reference |
| Represents | One UTF-16 code unit | Sequence of UTF-16 code units |
| Syntax | `'A'` | `"A"` |
| Class | ❌ | `java.lang.String` |
| Mutable | N/A | ❌ |

---

# 5. 🏗️ Creating Strings

There are two commonly discussed ways to create a String.

## 5.1 String Literal

```java
String s1 = "Java";
```

This is the most common way.

## 5.2 Using `new`

```java
String s2 = new String("Java");
```

This explicitly creates a new String object.

## 🆚 Comparison

| Creation | Example | Main Idea |
|---|---|---|
| Literal | `String s = "Java";` | Uses String Pool |
| `new` | `String s = new String("Java");` | Creates a new String object |

The difference becomes extremely important when studying:

> 🔥 **String Pool**

---

# 6. 🏊 String Literal

A String literal is a sequence of characters written directly inside double quotes.

Examples:

```java
"Java"
"Hello"
"Divyansh"
"123"
""
```

Example:

```java
String language = "Java";
```

String literals can be stored/shared through the **String Pool**.

Consider:

```java
String s1 = "Java";
String s2 = "Java";
```

Conceptually:

```text
             String Pool

          ┌──────────────┐
          │    "Java"    │
          └───────┬──────┘
                  │
             ┌────┴────┐
             ↓         ↓
            s1        s2
```

Both references can point to the same pooled String object.

Therefore:

```java
System.out.println(s1 == s2);
```

can produce:

```text
true
```

### ⚠️ Important

This does **not** mean `==` compares String content.

`==` compares references.

It is `true` here because both references point to the same object.

---

# 7. 🆕 Creating String Using `new`

Example:

```java
String s1 = new String("Java");
```

The `new` keyword explicitly creates a new String object.

Compare:

```java
String s1 = "Java";
String s2 = new String("Java");
```

Conceptually:

```text
String Pool

┌──────────────┐
│    "Java"    │
└──────┬───────┘
       ↑
       │
      s1


Heap

┌──────────────┐
│    "Java"    │
└──────┬───────┘
       ↑
       s2
```

Therefore:

```java
System.out.println(s1 == s2);
```

Output:

```text
false
```

But:

```java
System.out.println(s1.equals(s2));
```

Output:

```text
true
```

Because:

```text
==        → compares references
equals()  → compares content
```

---

# 8. ⭐ String Characteristics

| Property | String |
|---|---|
| Type | Reference type |
| Class | `java.lang.String` |
| Primitive? | ❌ No |
| Immutable? | ✅ Yes |
| Final? | ✅ Yes |
| String Pool | ✅ Yes |
| Represents | Sequence of characters |
| Supports `equals()` | ✅ Yes |
| Supports `==` | ✅ Reference comparison |
| Can be subclassed | ❌ No |
| Common HashMap key | ✅ Yes |

---

# 9. 🔒 String Immutability

One of the most important String concepts is:

> **String objects are immutable.**

Immutable means:

> **Once a String object is created, its content cannot be changed.**

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

Because:

```java
s.concat(" Programming");
```

does not modify the existing `"Java"` object.

It creates another String.

Conceptually:

```text
Original:

"Java"


After concat():

"Java"                  "Java Programming"
  ↑                            ↑
  s                       new String
```

To store the new String:

```java
s = s.concat(" Programming");
```

Now:

```text
Java Programming
```

Methods such as:

```java
concat()
toUpperCase()
toLowerCase()
replace()
substring()
trim()
strip()
```

do not modify the existing String object.

They return a String result.

---

# 10. ➕ String Concatenation

Concatenation means joining Strings together.

Java provides the `+` operator for String concatenation.

Example:

```java
String firstName = "Divyansh";
String lastName = "Singh";

String fullName = firstName + " " + lastName;

System.out.println(fullName);
```

Output:

```text
Divyansh Singh
```

## 🔢 String + Number

Java can also concatenate Strings with primitive values.

```java
int age = 22;

String result = "Age: " + age;

System.out.println(result);
```

Output:

```text
Age: 22
```

## ⚠️ Important: Left-to-Right Evaluation

Consider:

```java
System.out.println(10 + 20 + "Java");
```

Output:

```text
30Java
```

Why?

```text
10 + 20
  ↓
30

30 + "Java"
  ↓
"30Java"
```

Now:

```java
System.out.println("Java" + 10 + 20);
```

Output:

```text
Java1020
```

Because once String concatenation starts:

```text
"Java" + 10
      ↓
"Java10"

"Java10" + 20
      ↓
"Java1020"
```

---

# 11. 📏 String Length

To find the number of UTF-16 code units in a String:

```java
String s = "Java";

System.out.println(s.length());
```

Output:

```text
4
```

Notice:

```java
str.length()
```

not:

```java
str.length
```

## 🆚 Array vs String vs Collection

| Data Structure | Size |
|---|---|
| Array | `arr.length` |
| String | `str.length()` |
| Collection | `collection.size()` |

### 🧠 Memory Trick

```text
Array       → length
String      → length()
Collection  → size()
```

---

# 12. 🔤 String and `char[]`

A String represents a sequence of characters.

```java
String s = "Java";
```

Conceptually:

```text
J → a → v → a
```

## String → `char[]`

```java
String s = "Java";

char[] chars = s.toCharArray();

for (char c : chars) {
    System.out.println(c);
}
```

Output:

```text
J
a
v
a
```

## `char[]` → String

```java
char[] chars = {'J', 'a', 'v', 'a'};

String s = new String(chars);

System.out.println(s);
```

Output:

```text
Java
```

## 🆚 String vs `char[]`

| Feature | String | `char[]` |
|---|---|---|
| Type | Class | Array |
| Mutable | ❌ No | ✅ Yes |
| Represents | Character sequence | Characters |
| Has methods | ✅ Many | ❌ Array has no String methods |
| Change individual character | ❌ No | ✅ Yes |

Example:

```java
char[] chars = {'J', 'a', 'v', 'a'};

chars[0] = 'K';

System.out.println(chars);
```

Output:

```text
Kava
```

But:

```java
String s = "Java";
```

You cannot do:

```java
s[0] = 'K'; // ❌ Invalid Java
```

---

# 13. 🆚 `==` vs `equals()`

This is one of the most frequently asked Java interview questions.

## `==`

For references, `==` compares **reference identity**.

It asks:

> "Are these references pointing to the same object?"

Example:

```java
String s1 = new String("Java");
String s2 = new String("Java");

System.out.println(s1 == s2);
```

Output:

```text
false
```

They are different objects.

## `equals()`

For String, `equals()` compares the **content**.

```java
System.out.println(s1.equals(s2));
```

Output:

```text
true
```

Because both contain:

```text
Java
```

## 🔥 Comparison Table

| Operator / Method | Compares |
|---|---|
| `==` | Reference identity |
| `equals()` | String content |

### 🧠 Golden Rule

> **For comparing String content, use `equals()`, not `==`.**

---

# 14. 🧠 Basic Memory View

Consider:

```java
String s = "Java";
```

A simplified conceptual model:

```text
        Stack Frame

┌─────────────────┐
│ s               │
│ reference       │
└────────┬────────┘
         │
         ▼

    String Pool / Heap

┌─────────────────┐
│     "Java"      │
└─────────────────┘
```

The important idea is:

```text
Reference variable
       ↓
String object
```

## Another Example

```java
String s1 = "Java";
String s2 = "Java";
```

Conceptually:

```text
          String Pool

       ┌───────────────┐
       │    "Java"     │
       └───────┬───────┘
               │
          ┌────┴────┐
          ↓         ↓
         s1        s2
```

Therefore:

```java
s1 == s2
```

can be:

```text
true
```

because both references can point to the same pooled object.

### ⚠️ Important

Do not say:

> "String is stored in stack."

A local reference may exist in a stack frame, while the String object itself is on the heap in modern HotSpot implementations.

---

# 15. ⭐ Why Is String Special?

String has several properties that make it different from ordinary classes.

### 1. Immutable

```text
String object cannot be modified
```

### 2. Final

```java
public final class String
```

It cannot be subclassed.

### 3. String Pool

String literals can be shared.

### 4. Frequently Used

Almost every Java application works with Strings.

### 5. HashMap-Friendly

String is commonly used as a key because it is immutable and provides content-based `equals()` and `hashCode()` behavior.

```java
Map<String, Integer> map = new HashMap<>();

map.put("Java", 100);
```

### 6. Security Benefits

Strings are commonly used for values involved in class loading, file paths, URLs, configuration, and other security-sensitive operations.

Immutability helps prevent a String value from being changed unexpectedly after it has been created or shared.

---

# 16. 🛠️ Important String Methods — Preview

A detailed method-by-method discussion should be covered separately.

| Method | Purpose |
|---|---|
| `length()` | Returns UTF-16 code-unit count |
| `charAt()` | Returns character/code unit at index |
| `substring()` | Extracts part of String |
| `equals()` | Compares content |
| `equalsIgnoreCase()` | Case-insensitive comparison |
| `contains()` | Checks whether sequence exists |
| `startsWith()` | Checks starting sequence |
| `endsWith()` | Checks ending sequence |
| `indexOf()` | Finds index |
| `lastIndexOf()` | Finds last occurrence |
| `toUpperCase()` | Converts to uppercase |
| `toLowerCase()` | Converts to lowercase |
| `trim()` | Removes leading/trailing characters ≤ U+0020 |
| `strip()` | Removes Unicode-aware leading/trailing whitespace |
| `replace()` | Replaces literal characters/sequences |
| `replaceAll()` | Regex-based replacement |
| `split()` | Splits String |
| `concat()` | Concatenates String |
| `isEmpty()` | Checks `length() == 0` |
| `isBlank()` | Checks empty or whitespace-only String |
| `toCharArray()` | Converts to `char[]` |

---

# 17. 🌍 String and Unicode

Java Strings are designed to represent Unicode text.

Example:

```java
String s = "Hello 🌍";
```

A String can contain:

```text
English
Hindi
Chinese
Japanese
Arabic
Emojis
etc.
```

Example:

```java
String hindi = "नमस्ते";
String japanese = "こんにちは";
String emoji = "🚀";
```

## ⚠️ Important Interview Point

Do not always assume:

```java
str.length()
```

means:

> "number of visible characters."

Java's `String.length()` returns the number of **UTF-16 code units**.

For many ordinary characters:

```text
1 character ≈ 1 code unit
```

But some Unicode characters require more than one UTF-16 code unit.

Example:

```java
String emoji = "🚀";

System.out.println(emoji.length());
```

This can print:

```text
2
```

even though it appears visually as one symbol.

For Unicode code points:

```java
emoji.codePointCount(0, emoji.length());
```

can be used.

### 🧠 Interview-Level Point

> `String.length()` returns UTF-16 code-unit count, not necessarily the number of user-perceived characters.

---

# 18. 🕳️ String and `null`

A String reference can contain `null`.

```java
String s = null;
```

This means:

```text
s
↓
null
```

There is no String object being referenced.

## ⚠️ Calling Method on `null`

```java
String s = null;

System.out.println(s.length());
```

This causes:

```text
NullPointerException
```

because you are trying to call a method through a null reference.

## Safer Comparison

Instead of:

```java
if (s.equals("Java")) {
    // ...
}
```

when `s` might be null, use:

```java
if ("Java".equals(s)) {
    // ...
}
```

Because the literal `"Java"` is not null.

---

# 19. ⚠️ Common Mistakes

## ❌ Mistake 1: Thinking String is primitive

Wrong:

```text
String → primitive
```

Correct:

```text
String → class / reference type
```

## ❌ Mistake 2: Using `==` for content comparison

Wrong:

```java
if (s1 == s2) {
    // ...
}
```

when you want to compare text content.

Correct:

```java
if (s1.equals(s2)) {
    // ...
}
```

## ❌ Mistake 3: Thinking String can be modified

Wrong:

```java
String s = "Java";

s.concat(" World");

System.out.println(s);
```

Output:

```text
Java
```

Correct understanding:

```text
String is immutable.
```

## ❌ Mistake 4: Confusing `length` and `length()`

Array:

```java
arr.length
```

String:

```java
str.length()
```

Collection:

```java
collection.size()
```

## ❌ Mistake 5: Confusing `char` and String

Wrong:

```java
String s = 'A'; // ❌
```

Correct:

```java
String s = "A";
```

And:

```java
char c = 'A';
```

## ❌ Mistake 6: Thinking `new String()` is better

Usually:

```java
String s = "Java";
```

is preferred when you simply need a String literal.

Using:

```java
new String("Java");
```

is generally unnecessary for ordinary use.

---

# 20. 🚨 Interview Traps

## Trap 1

```java
String a = "Java";
String b = "Java";

System.out.println(a == b);
```

Output:

```text
true
```

Reason:

Both literals can refer to the same pooled object.

## Trap 2

```java
String a = new String("Java");
String b = new String("Java");

System.out.println(a == b);
```

Output:

```text
false
```

Reason:

Each `new` creates a separate String object.

## Trap 3

```java
String a = new String("Java");
String b = new String("Java");

System.out.println(a.equals(b));
```

Output:

```text
true
```

Reason:

Their contents are equal.

## Trap 4

```java
System.out.println(10 + 20 + "Java");
```

Output:

```text
30Java
```

## Trap 5

```java
System.out.println("Java" + 10 + 20);
```

Output:

```text
Java1020
```

## Trap 6

```java
String s = "Java";

s.concat("World");

System.out.println(s);
```

Output:

```text
Java
```

Reason:

String is immutable and the returned String was ignored.

## Trap 7

```java
String s = null;

System.out.println(s.length());
```

Result:

```text
NullPointerException
```

---

# 21. 🔥 Top 15 Interview Questions

## Q1. What is String in Java?

**Answer:**

String is a `final` class from the `java.lang` package that represents a sequence of characters.

---

## Q2. Is String a primitive data type?

**Answer:**

No.

String is a reference type and an object of the `java.lang.String` class.

---

## Q3. Why is String called immutable?

**Answer:**

Because once a String object is created, its content cannot be changed.

Operations that appear to modify a String return a new String instead.

---

## Q4. Why is String final?

**Answer:**

`String` is declared as:

```java
public final class String
```

Therefore it cannot be subclassed.

This helps preserve String's designed behavior and works together with immutability and safe sharing.

---

## Q5. What is the difference between `==` and `equals()`?

**Answer:**

```text
==        → compares reference identity
equals()  → compares String content
```

Example:

```java
String a = new String("Java");
String b = new String("Java");

System.out.println(a == b);
System.out.println(a.equals(b));
```

Output:

```text
false
true
```

---

## Q6. What is the difference between String literal and `new String()`?

**Answer:**

```java
String a = "Java";
```

uses the String Pool mechanism.

While:

```java
String b = new String("Java");
```

explicitly creates a new String object.

---

## Q7. Why does Java use a String Pool?

**Answer:**

Strings are frequently used and immutable.

The pool allows eligible equal String literals to be shared, reducing unnecessary duplicate objects.

---

## Q8. Is String thread-safe?

**Answer:**

String objects cannot be modified after creation because they are immutable.

Therefore, multiple threads can safely share the same String object without one thread changing its contents.

However, this does not mean every operation involving String references or mutable surrounding state is automatically thread-safe.

---

## Q9. What is the difference between `char` and String?

**Answer:**

```text
char

→ primitive
→ one UTF-16 code unit
→ 'A'

String

→ reference type
→ sequence of UTF-16 code units
→ "A"
```

---

## Q10. What does `length()` return?

**Answer:**

It returns the number of UTF-16 code units in the String.

For ordinary English characters this usually matches the visible character count, but not always for supplementary Unicode characters or emoji.

---

## Q11. Why does this print `30Java`?

```java
System.out.println(10 + 20 + "Java");
```

**Answer:**

Evaluation occurs from left to right:

```text
10 + 20
→ 30

30 + "Java"
→ "30Java"
```

---

## Q12. Why does this print `Java1020`?

```java
System.out.println("Java" + 10 + 20);
```

**Answer:**

Once the String is encountered, the remaining `+` operations perform String concatenation:

```text
"Java" + 10
→ "Java10"

"Java10" + 20
→ "Java1020"
```

---

## Q13. Can we modify a String character directly?

**Answer:**

No.

This is invalid:

```java
String s = "Java";

s[0] = 'K'; // ❌
```

Strings are immutable.

If character-level mutation is required, use `char[]`, `StringBuilder`, or another appropriate mutable structure.

---

## Q14. Why is String commonly used as a HashMap key?

**Answer:**

Because String is immutable and provides content-based `equals()` and `hashCode()` behavior.

Once used as a key, its contents cannot change.

Example:

```java
Map<String, Integer> map = new HashMap<>();

map.put("Java", 100);
```

---

## Q15. Where does a String object reside in memory?

**Answer:**

A String object is stored on the heap in modern HotSpot JVM implementations.

A local reference may exist in a stack frame.

String literals use the JVM's String Pool, which is associated with heap memory in modern HotSpot implementations.

---

# 22. 🧠 DSA with Strings

String is one of the most important topics for DSA interviews.

Most String problems are not about memorizing methods.

The real skill is:

> **Recognizing the underlying pattern from the problem statement.**

A String problem can often be converted into:

```text
String
  ↓
Characters / indices
  ↓
Array-like processing
  ↓
Pattern recognition
  ↓
DSA solution
```

Important String DSA topics:

```text
1. Traversal
2. Frequency Counting
3. Hashing
4. Two Pointers
5. Sliding Window
6. Prefix / Suffix
7. String Matching
8. Stack
9. Sorting
10. Binary Search
11. Greedy
12. Dynamic Programming
13. Trie
14. Anagram Pattern
15. Palindrome Pattern
16. Substring Problems
17. Subsequence Problems
18. Character Mapping
19. Parsing
20. String Construction
```

---

# 23. 🔍 How to Identify String DSA Patterns

This is extremely important for interviews.

## Pattern 1 — "Count characters"

Look for:

```text
frequency
count
occurrence
how many times
duplicate characters
most frequent
```

Think:

```text
HashMap / int[]
```

Example:

> Count frequency of every character.

Pattern:

```text
Frequency Counting
```

---

## Pattern 2 — "Two ends"

Look for:

```text
reverse
palindrome
compare from both sides
remove from beginning/end
```

Think:

```text
Two Pointers
```

Example:

> Check whether a String is a palindrome.

Pattern:

```text
Two Pointers
```

---

## Pattern 3 — "Longest / shortest substring"

Look for:

```text
longest substring
smallest substring
minimum window
maximum window
at most K
at least K
without repeating
```

Think:

```text
Sliding Window
```

---

## Pattern 4 — "Anagram"

Look for:

```text
anagram
same characters
rearrangement
permutation of characters
```

Think:

```text
Frequency Array / HashMap
```

---

## Pattern 5 — "Find substring"

Look for:

```text
pattern
text
find occurrence
search pattern
matching
```

Think:

```text
String Matching
```

Basic:

```text
Brute Force
```

Advanced:

```text
KMP
Rabin-Karp
Z Algorithm
```

---

## Pattern 6 — "Next greater/smaller character"

Look for:

```text
next greater
previous greater
remove characters
monotonic behavior
```

Think:

```text
Stack
```

---

## Pattern 7 — "Dictionary / prefixes"

Look for:

```text
prefix
word search
dictionary
autocomplete
starts with
multiple words
```

Think:

```text
Trie
```

---

## Pattern 8 — "Subsequence"

Look for:

```text
subsequence
delete some characters
preserve order
can form
```

Think:

```text
Two Pointers
```

or:

```text
Dynamic Programming
```

depending on the problem.

---

## Pattern 9 — "Minimum/maximum transformation"

Look for:

```text
minimum operations
maximum length
minimum deletions
maximum subsequence
```

Think:

```text
Dynamic Programming / Greedy
```

---

## Pattern 10 — "Repeated substring"

Look for:

```text
repeated pattern
periodic string
repeated prefix
```

Think:

```text
Prefix Function / KMP
```

---

# 24. 🧩 Important String DSA Patterns

## 24.1 Frequency Array

For lowercase English letters:

```java
String s = "banana";

int[] freq = new int[26];

for (char c : s.toCharArray()) {
    freq[c - 'a']++;
}
```

### Why `c - 'a'`?

Characters have numeric Unicode values.

```text
'a' - 'a' = 0
'b' - 'a' = 1
'c' - 'a' = 2
...
'z' - 'a' = 25
```

So:

```text
character
   ↓
index
   ↓
frequency
```

### Complexity

```text
Time  → O(n)
Space → O(26) = O(1)
```

---

# 24.2 HashMap Frequency

Use this when the character set is not limited to lowercase English letters.

```java
Map<Character, Integer> freq = new HashMap<>();

for (char c : s.toCharArray()) {
    freq.put(c, freq.getOrDefault(c, 0) + 1);
}
```

### Think

```text
Unknown / large character set
        ↓
HashMap
```

---

# 24.3 Two Pointers

Classic palindrome pattern:

```java
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
```

### Complexity

```text
Time  → O(n)
Space → O(1)
```

### Trigger Words

```text
palindrome
reverse
both ends
left/right
```

---

# 24.4 Sliding Window

Classic:

> Longest substring without repeating characters.

```java
Set<Character> set = new HashSet<>();

int left = 0;
int maxLength = 0;

for (int right = 0; right < s.length(); right++) {

    while (set.contains(s.charAt(right))) {
        set.remove(s.charAt(left));
        left++;
    }

    set.add(s.charAt(right));

    maxLength = Math.max(
        maxLength,
        right - left + 1
    );
}
```

### Complexity

```text
Time  → O(n)
Space → O(k)
```

where `k` is the number of distinct characters in the window.

### Trigger Words

```text
longest substring
without repeating
unique characters
window
at most K
```

---

# 24.5 Character Mapping

Used when two Strings must follow the same character pattern.

Example idea:

```text
egg
add
```

Mapping:

```text
e → a
g → d
```

But mapping must be consistent.

Basic approach:

```java
Map<Character, Character> map = new HashMap<>();
Map<Character, Character> reverse = new HashMap<>();

for (int i = 0; i < s.length(); i++) {

    char a = s.charAt(i);
    char b = t.charAt(i);

    if (map.containsKey(a) && map.get(a) != b) {
        return false;
    }

    if (reverse.containsKey(b) && reverse.get(b) != a) {
        return false;
    }

    map.put(a, b);
    reverse.put(b, a);
}

return true;
```

Typical problem:

```text
Isomorphic Strings
```

---

# 24.6 Anagram

Two Strings are anagrams when they contain the same character frequencies.

Example:

```text
listen
silent
```

Frequency approach:

```java
int[] freq = new int[26];

for (char c : s.toCharArray()) {
    freq[c - 'a']++;
}

for (char c : t.toCharArray()) {
    freq[c - 'a']--;
}

for (int count : freq) {
    if (count != 0) {
        return false;
    }
}

return true;
```

### Complexity

```text
Time  → O(n)
Space → O(1)
```

### Trigger Words

```text
anagram
rearrangement
same characters
same frequency
```

---

# 24.7 Prefix / Suffix

A prefix is the beginning of a String.

```text
String = "flower"

Prefixes:

f
fl
flo
flow
flowe
flower
```

A suffix is the ending portion.

```text
flower

r
er
wer
ower
lower
flower
```

Common problems:

```text
Longest Common Prefix
Prefix Matching
Prefix Function
KMP
```

---

# 24.8 Stack + String

Use a Stack when characters must be removed based on previous characters.

Typical trigger words:

```text
remove adjacent duplicates
valid parentheses
backspace
undo
nested structure
```

Example:

```java
StringBuilder stack = new StringBuilder();

for (char c : s.toCharArray()) {

    int n = stack.length();

    if (n > 0 && stack.charAt(n - 1) == c) {
        stack.deleteCharAt(n - 1);
    } else {
        stack.append(c);
    }
}

return stack.toString();
```

---

# 24.9 String Matching

Suppose:

```text
Text    = "ababcabc"
Pattern = "abc"
```

We need to determine where the pattern occurs.

Basic approaches:

```text
1. Brute Force
2. KMP
3. Rabin-Karp
4. Z Algorithm
```

For basic DSA:

```text
Brute Force
```

For advanced interviews:

```text
KMP
Rabin-Karp
Z Algorithm
```

---

# 24.10 Trie

Trie is useful when dealing with many Strings and prefixes.

Typical problems:

```text
Word Dictionary
Autocomplete
Prefix Search
Word Search
Starts With
```

Basic structure:

```text
        root
       /    \
      a      b
      |
      p
      |
      p
```

Important operations:

```text
insert()
search()
startsWith()
```

Typical complexity:

```text
Insert    → O(L)
Search    → O(L)
Prefix    → O(L)
```

where `L` is the length of the word/prefix.

---

# 24.11 Subsequence

A subsequence does not require contiguous characters.

Example:

```text
String = "abcde"

"ace" → subsequence
"acd" → subsequence
"ae"  → subsequence
```

But:

```text
"aec"
```

is not a subsequence because order is changed.

Basic two-pointer approach:

```java
int i = 0;
int j = 0;

while (i < s.length() && j < t.length()) {

    if (s.charAt(i) == t.charAt(j)) {
        i++;
    }

    j++;
}

return i == s.length();
```

### Complexity

```text
Time  → O(n)
Space → O(1)
```

---

# 24.12 Substring

A substring must be contiguous.

For:

```text
"abcde"
```

Examples:

```text
"abc"
"bcd"
"cde"
"bc"
```

But:

```text
"ace"
```

is not a substring.

### Important Difference

```text
Substring
→ contiguous

Subsequence
→ not necessarily contiguous
→ order maintained
```

This distinction is extremely important in DSA.

---

# 24.13 Binary Search on Strings

Binary search can be used when Strings are sorted.

Example:

```java
String[] words = {
    "apple",
    "banana",
    "cat",
    "dog"
};

int left = 0;
int right = words.length - 1;

while (left <= right) {

    int mid = left + (right - left) / 2;

    int cmp = words[mid].compareTo("cat");

    if (cmp == 0) {
        return mid;
    } else if (cmp < 0) {
        left = mid + 1;
    } else {
        right = mid - 1;
    }
}

return -1;
```

Trigger:

```text
sorted strings
search efficiently
```

---

# 24.14 Sorting Characters

Sometimes the simplest way to compare Strings is sorting.

```java
char[] chars = s.toCharArray();

Arrays.sort(chars);

String sorted = new String(chars);
```

Useful for:

```text
Anagram
Grouping Anagrams
Canonical representation
```

Complexity:

```text
Time  → O(n log n)
Space → depends on implementation / copied array
```

For fixed lowercase alphabets, frequency counting can often achieve:

```text
O(n)
```

---

# 24.15 Prefix Sum / Running Count

Some String problems can be transformed into an array problem.

Example:

```text
binary String
"101101"
```

We can maintain counts.

```java
int ones = 0;

for (char c : s.toCharArray()) {

    if (c == '1') {
        ones++;
    }
}
```

For range-based queries, prefix arrays can be useful.

---

# 24.16 Dynamic Programming with Strings

DP is important for problems involving:

```text
minimum operations
maximum length
matching
edit distance
palindromic subsequence
common subsequence
```

Major problems:

```text
Longest Common Subsequence
Longest Palindromic Subsequence
Edit Distance
Distinct Subsequences
Word Break
Interleaving String
```

Basic LCS state:

```text
dp[i][j]

→ answer using first i characters of s
  and first j characters of t
```

---

# 25. 🔥 Important String DSA Questions

These are the questions you should know from basic → advanced.

## 🟢 Level 1 — Basic

### 1. Reverse a String

Pattern:

```text
Two Pointers
```

```java
char[] arr = s.toCharArray();

int left = 0;
int right = arr.length - 1;

while (left < right) {

    char temp = arr[left];
    arr[left] = arr[right];
    arr[right] = temp;

    left++;
    right--;
}

return new String(arr);
```

---

### 2. Check Palindrome

Pattern:

```text
Two Pointers
```

```java
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
```

---

### 3. Count Character Frequency

Pattern:

```text
Frequency Array
```

```java
int[] freq = new int[26];

for (char c : s.toCharArray()) {
    freq[c - 'a']++;
}
```

---

### 4. Find First Non-Repeating Character

Pattern:

```text
Frequency + Traversal
```

```java
int[] freq = new int[26];

for (char c : s.toCharArray()) {
    freq[c - 'a']++;
}

for (char c : s.toCharArray()) {

    if (freq[c - 'a'] == 1) {
        return c;
    }
}

return '\0';
```

---

### 5. Check Anagram

Pattern:

```text
Frequency Counting
```

```java
int[] freq = new int[26];

for (char c : s.toCharArray()) {
    freq[c - 'a']++;
}

for (char c : t.toCharArray()) {
    freq[c - 'a']--;
}

for (int count : freq) {
    if (count != 0) {
        return false;
    }
}

return true;
```

---

## 🟡 Level 2 — Medium

### 6. Longest Substring Without Repeating Characters

Pattern:

```text
Sliding Window + Set
```

```java
Set<Character> set = new HashSet<>();

int left = 0;
int maxLength = 0;

for (int right = 0; right < s.length(); right++) {

    while (set.contains(s.charAt(right))) {
        set.remove(s.charAt(left));
        left++;
    }

    set.add(s.charAt(right));

    maxLength = Math.max(
        maxLength,
        right - left + 1
    );
}

return maxLength;
```

---

### 7. Valid Palindrome

Pattern:

```text
Two Pointers
```

When spaces and punctuation should be ignored:

```java
int left = 0;
int right = s.length() - 1;

while (left < right) {

    while (left < right && !Character.isLetterOrDigit(s.charAt(left))) {
        left++;
    }

    while (left < right && !Character.isLetterOrDigit(s.charAt(right))) {
        right--;
    }

    if (Character.toLowerCase(s.charAt(left))
            != Character.toLowerCase(s.charAt(right))) {
        return false;
    }

    left++;
    right--;
}

return true;
```

---

### 8. Longest Common Prefix

Pattern:

```text
Prefix Matching
```

```java
String prefix = strs[0];

for (int i = 1; i < strs.length; i++) {

    while (!strs[i].startsWith(prefix)) {

        prefix = prefix.substring(0, prefix.length() - 1);

        if (prefix.isEmpty()) {
            return "";
        }
    }
}

return prefix;
```

---

### 9. Isomorphic Strings

Pattern:

```text
Two-way Character Mapping
```

```java
Map<Character, Character> map = new HashMap<>();
Map<Character, Character> reverse = new HashMap<>();

for (int i = 0; i < s.length(); i++) {

    char a = s.charAt(i);
    char b = t.charAt(i);

    if (map.containsKey(a) && map.get(a) != b) {
        return false;
    }

    if (reverse.containsKey(b) && reverse.get(b) != a) {
        return false;
    }

    map.put(a, b);
    reverse.put(b, a);
}

return true;
```

---

### 10. Group Anagrams

Pattern:

```text
Canonical Form + HashMap
```

Basic idea:

```java
Map<String, List<String>> map = new HashMap<>();

for (String word : strs) {

    char[] chars = word.toCharArray();

    Arrays.sort(chars);

    String key = new String(chars);

    map.computeIfAbsent(key, k -> new ArrayList<>())
       .add(word);
}

return new ArrayList<>(map.values());
```

---

### 11. Longest Palindromic Substring

Important patterns:

```text
Expand Around Center
Dynamic Programming
```

Expand-around-center idea:

```java
int start = 0;
int end = 0;

for (int i = 0; i < s.length(); i++) {

    int len1 = expand(s, i, i);
    int len2 = expand(s, i, i + 1);

    int len = Math.max(len1, len2);

    if (len > end - start + 1) {

        start = i - (len - 1) / 2;
        end = i + len / 2;
    }
}
```

Helper:

```java
private int expand(String s, int left, int right) {

    while (left >= 0 &&
           right < s.length() &&
           s.charAt(left) == s.charAt(right)) {

        left--;
        right++;
    }

    return right - left - 1;
}
```

Complexity:

```text
Time  → O(n²)
Space → O(1)
```

---

## 🔴 Level 3 — Advanced

### 12. Longest Common Subsequence

Pattern:

```text
Dynamic Programming
```

Core recurrence:

```text
if characters match:

dp[i][j] = 1 + dp[i-1][j-1]

otherwise:

dp[i][j] = max(
    dp[i-1][j],
    dp[i][j-1]
)
```

---

### 13. Edit Distance

Pattern:

```text
Dynamic Programming
```

Operations:

```text
Insert
Delete
Replace
```

Core recurrence when characters differ:

```text
dp[i][j] =
1 + min(
    dp[i-1][j],     // delete
    dp[i][j-1],     // insert
    dp[i-1][j-1]    // replace
)
```

---

### 14. Minimum Window Substring

Pattern:

```text
Sliding Window + Frequency Map
```

Trigger:

```text
minimum substring
contains all required characters
```

Core technique:

```text
Expand right
    ↓
Satisfy requirement
    ↓
Shrink left
    ↓
Record minimum
```

---

### 15. Word Break

Pattern:

```text
Dynamic Programming
```

Think:

```text
Can prefix [0...i] be formed?

dp[i] = true / false
```

---

### 16. Implement `strStr()` / Find Pattern

Pattern:

```text
String Matching
```

Approaches:

```text
Brute Force
KMP
Rabin-Karp
Z Algorithm
```

---

### 17. KMP Pattern Matching

Know these concepts:

```text
Pattern
Prefix
Suffix
LPS Array
Failure Function
```

Important:

```text
LPS[i]
=
length of longest proper prefix
which is also a suffix
for pattern[0...i]
```

Complexity:

```text
Time  → O(n + m)
Space → O(m)
```

---

### 18. Rabin-Karp

Pattern:

```text
Rolling Hash
```

Useful when:

```text
Searching a pattern
Multiple pattern comparisons
Hash-based matching
```

Core idea:

```text
Current window hash
        ↓
Remove outgoing character
        ↓
Add incoming character
        ↓
Compare hash
```

Average performance is useful, but hash collisions must be handled correctly.

---

### 19. Trie Problems

Important questions:

```text
Implement Trie
Search Word
Starts With Prefix
Word Dictionary
Autocomplete
Maximum XOR
Word Search
```

---

# 26. 🧰 String DSA Snippets

## 26.1 Reverse String

```java
StringBuilder sb = new StringBuilder(s);

return sb.reverse().toString();
```

---

## 26.2 Reverse Using Two Pointers

```java
char[] arr = s.toCharArray();

int left = 0;
int right = arr.length - 1;

while (left < right) {

    char temp = arr[left];
    arr[left] = arr[right];
    arr[right] = temp;

    left++;
    right--;
}

return new String(arr);
```

---

## 26.3 Count Vowels

```java
int count = 0;

for (char c : s.toLowerCase().toCharArray()) {

    if (c == 'a' ||
        c == 'e' ||
        c == 'i' ||
        c == 'o' ||
        c == 'u') {

        count++;
    }
}

return count;
```

---

## 26.4 Count Digits

```java
int count = 0;

for (char c : s.toCharArray()) {

    if (Character.isDigit(c)) {
        count++;
    }
}

return count;
```

---

## 26.5 Remove Spaces

```java
String result = s.replace(" ", "");
```

For all whitespace:

```java
String result = s.replaceAll("\\s+", "");
```

---

## 26.6 Check Only Digits

```java
for (char c : s.toCharArray()) {

    if (!Character.isDigit(c)) {
        return false;
    }
}

return true;
```

---

## 26.7 Count Words

Simple whitespace-based approach:

```java
String trimmed = s.trim();

if (trimmed.isEmpty()) {
    return 0;
}

return trimmed.split("\\s+").length;
```

For interview problems, clarify how punctuation and whitespace should be treated.

---

## 26.8 Character Frequency

```java
int[] freq = new int[26];

for (char c : s.toCharArray()) {
    freq[c - 'a']++;
}
```

---

## 26.9 Maximum Frequency Character

```java
int[] freq = new int[26];

for (char c : s.toCharArray()) {
    freq[c - 'a']++;
}

int maxIndex = 0;

for (int i = 1; i < 26; i++) {

    if (freq[i] > freq[maxIndex]) {
        maxIndex = i;
    }
}

char answer = (char) ('a' + maxIndex);
```

---

## 26.10 First Non-Repeating Character

```java
int[] freq = new int[26];

for (char c : s.toCharArray()) {
    freq[c - 'a']++;
}

for (char c : s.toCharArray()) {

    if (freq[c - 'a'] == 1) {
        return c;
    }
}

return '\0';
```

---

## 26.11 Remove Duplicate Characters

Preserve first occurrence order:

```java
Set<Character> seen = new LinkedHashSet<>();

for (char c : s.toCharArray()) {
    seen.add(c);
}

StringBuilder result = new StringBuilder();

for (char c : seen) {
    result.append(c);
}

return result.toString();
```

---

## 26.12 Check Anagram

```java
if (s.length() != t.length()) {
    return false;
}

int[] freq = new int[26];

for (int i = 0; i < s.length(); i++) {
    freq[s.charAt(i) - 'a']++;
    freq[t.charAt(i) - 'a']--;
}

for (int count : freq) {

    if (count != 0) {
        return false;
    }
}

return true;
```

---

## 26.13 Longest Substring Without Repeating Characters

```java
Set<Character> set = new HashSet<>();

int left = 0;
int maxLength = 0;

for (int right = 0; right < s.length(); right++) {

    while (set.contains(s.charAt(right))) {
        set.remove(s.charAt(left));
        left++;
    }

    set.add(s.charAt(right));

    maxLength = Math.max(
        maxLength,
        right - left + 1
    );
}

return maxLength;
```

---

## 26.14 Check Subsequence

```java
int i = 0;

for (int j = 0; j < t.length() && i < s.length(); j++) {

    if (s.charAt(i) == t.charAt(j)) {
        i++;
    }
}

return i == s.length();
```

---

## 26.15 Character Mapping

```java
Map<Character, Character> map = new HashMap<>();

for (int i = 0; i < s.length(); i++) {

    char source = s.charAt(i);
    char target = t.charAt(i);

    if (map.containsKey(source)) {

        if (map.get(source) != target) {
            return false;
        }

    } else {
        map.put(source, target);
    }
}
```

For true isomorphism, also ensure two different source characters cannot map to the same target character.

---

## 26.16 Expand Around Center

```java
private int expand(String s, int left, int right) {

    while (left >= 0 &&
           right < s.length() &&
           s.charAt(left) == s.charAt(right)) {

        left--;
        right++;
    }

    return right - left - 1;
}
```

---

## 26.17 Sort Characters

```java
char[] chars = s.toCharArray();

Arrays.sort(chars);

String sorted = new String(chars);
```

---

## 26.18 StringBuilder for Repeated Modification

```java
StringBuilder sb = new StringBuilder();

for (char c : s.toCharArray()) {

    if (Character.isLetter(c)) {
        sb.append(c);
    }
}

return sb.toString();
```

---

# 27. 🎤 30-Second Interview Answer

> **String in Java is a final class from the `java.lang` package that represents a sequence of characters. It is a reference type, not a primitive, and String objects are immutable. Java provides String literals and a String Pool that allows eligible equal Strings to be shared. We normally use `equals()` to compare String content because `==` compares object references. In DSA, common String patterns include frequency counting, hashing, two pointers, sliding window, character mapping, stack, prefix/suffix processing, string matching, Trie, and dynamic programming.**

---

# 28. 🧾 Cheat Sheet

```text
╔══════════════════════════════════════════════════╗
║                 STRING CHEAT SHEET               ║
╠══════════════════════════════════════════════════╣
║ Class        → java.lang.String                  ║
║ Type         → Reference type                    ║
║ Primitive?   → ❌ No                             ║
║ Immutable?   → ✅ Yes                            ║
║ Final?       → ✅ Yes                            ║
║ String Pool  → ✅ Yes                            ║
║ Represents   → Sequence of UTF-16 code units     ║
║ length       → length()                          ║
║ Array size   → length                            ║
║ Collection   → size()                            ║
║ ==           → Reference identity               ║
║ equals()     → Content equality                  ║
║ char         → One UTF-16 code unit              ║
╠══════════════════════════════════════════════════╣
║ DSA PATTERNS                                      ║
╠══════════════════════════════════════════════════╣
║ Frequency    → Count / occurrence                ║
║ HashMap      → Unknown character set             ║
║ Two Pointer  → Palindrome / two ends             ║
║ Window       → Longest / shortest substring      ║
║ Stack        → Adjacent removal / nesting        ║
║ Mapping      → Isomorphic strings                ║
║ Prefix       → Common prefix / KMP               ║
║ Trie         → Prefix / dictionary               ║
║ DP           → LCS / Edit Distance / Word Break ║
║ Sorting      → Anagram / canonical form          ║
║ KMP          → Fast pattern matching             ║
╚══════════════════════════════════════════════════╝
```

---

# 29. 🧠 Memory Tricks

## 🔥 String Properties — "SIFP"

```text
S → Sequence
I → Immutable
F → Final
P → Pool
```

Think:

```text
String
  ↓
Sequence
  ↓
Immutable
  ↓
Final
  ↓
Pool
```

---

## 🧠 `==` vs `equals()`

```text
==

↓
Identity

equals()

↓
Content
```

Easy rule:

> **Same object? → `==`**

> **Same content? → `equals()`**

---

## 🧠 Size Methods

```text
Array       → length
String      → length()
Collection  → size()
```

---

## 🧠 DSA Pattern Recognition

```text
"count" / "frequency"
        ↓
Frequency Array / HashMap


"palindrome" / "both ends"
        ↓
Two Pointers


"longest substring"
        ↓
Sliding Window


"anagram"
        ↓
Frequency / Sorting


"mapping pattern"
        ↓
HashMap


"remove adjacent"
        ↓
Stack


"prefix / dictionary"
        ↓
Trie


"subsequence"
        ↓
Two Pointers / DP


"minimum operations"
        ↓
DP / Greedy


"find pattern"
        ↓
String Matching


"common subsequence"
        ↓
DP
```

---

# 30. 🔗 Next Topics

The String playlist:

```text
04-Strings/

│
├── 01-String-Introduction.md
│
├── 02-String-Pool.md
│
├── 03-String-Immutability.md
│
├── 04-String-Methods.md
│
├── 05-StringBuilder.md
│
├── 06-StringBuffer.md
│
├── 07-String-vs-StringBuilder-vs-StringBuffer.md
│
└── 08-String-Interview-Questions.md
```

### 📚 Learning Flow

```text
String Introduction
        │
        ▼
   String Pool
        │
        ▼
 String Immutability
        │
        ▼
  String Methods
        │
        ▼
  StringBuilder
        │
        ▼
  StringBuffer
        │
        ▼
String vs Builder vs Buffer
        │
        ▼
Interview Questions
```

---

# 🚀 Final Revision

Before moving to the next topic, remember these **20 points**:

```text
1.  String is a class.

2.  String belongs to java.lang.

3.  String is a reference type.

4.  String is not a primitive.

5.  String objects are immutable.

6.  String is final.

7.  String literals can use the String Pool.

8.  == compares reference identity.

9.  equals() compares String content.

10. length() returns UTF-16 code-unit count.

11. char represents one UTF-16 code unit.

12. String represents a sequence of UTF-16 code units.

13. String is commonly used as a HashMap key.

14. Frequency problems → int[] / HashMap.

15. Palindrome problems → Two Pointers.

16. Longest substring problems → Sliding Window.

17. Anagram problems → Frequency / Sorting.

18. Pattern searching → Brute Force / KMP / Rabin-Karp.

19. Prefix problems → Trie / Prefix algorithms.

20. Complex String optimization → DP / Greedy / Advanced matching.
```

---

# 🎯 DSA Interview Thinking Framework

Whenever you receive a String DSA problem, ask these questions **in this exact order**:

```text
STEP 1
What exactly is being asked?

        ↓

STEP 2
Is it about:

count?
frequency?
substring?
subsequence?
palindrome?
prefix?
pattern?
minimum/maximum?

        ↓

STEP 3
Look for trigger words.

        ↓

STEP 4
Choose the pattern.

        ↓

STEP 5
Ask whether the alphabet is fixed.

        ↓

Fixed lowercase English?
        ↓
int[26]

Unknown / large character set?
        ↓
HashMap

        ↓

STEP 6
Can I use Two Pointers?

        ↓

STEP 7
Can I use Sliding Window?

        ↓

STEP 8
Does the problem depend on previous characters?

        ↓
Stack / DP / Hashing

        ↓

STEP 9
Does it involve prefixes?

        ↓
Trie / Prefix Algorithm

        ↓

STEP 10
Analyze:

Time Complexity
Space Complexity
Edge Cases
```

---

# ⭐ Core Idea

> **String DSA is less about memorizing String methods and more about recognizing patterns.**

The most important recognition map is:

```text
STRING
  │
  ├── Count / Frequency
  │       └── int[] / HashMap
  │
  ├── Palindrome / Two Ends
  │       └── Two Pointers
  │
  ├── Longest / Shortest Substring
  │       └── Sliding Window
  │
  ├── Anagram
  │       └── Frequency / Sorting
  │
  ├── Character Relationship
  │       └── HashMap / Mapping
  │
  ├── Adjacent Characters / Nesting
  │       └── Stack
  │
  ├── Prefix / Dictionary
  │       └── Trie
  │
  ├── Pattern Search
  │       └── KMP / Rabin-Karp / Z
  │
  ├── Subsequence
  │       └── Two Pointers / DP
  │
  └── Optimization / Transformation
          └── DP / Greedy
```

> **For String DSA, first identify the pattern, then choose the data structure, then write the code.**