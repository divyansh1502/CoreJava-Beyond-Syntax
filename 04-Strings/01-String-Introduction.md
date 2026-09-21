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
22. [30-Second Interview Answer](#22--30-second-interview-answer)
23. [Cheat Sheet](#23--cheat-sheet)
24. [Memory Tricks](#24--memory-tricks)
25. [Next Topics](#25--next-topics)

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

In Java, String is represented by the:

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

---

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

---

## 🆚 `char` vs `String`

This is a very common beginner confusion.

```java
char c = 'A';
```

`char`:

- Primitive
- Represents one character
- Uses single quotes

Whereas:

```java
String s = "A";
```

`String`:

- Reference type
- Represents a sequence of characters
- Uses double quotes
- Is an object

| Feature | `char` | `String` |
|---|---|---|
| Type | Primitive | Reference |
| Represents | One character | Sequence of characters |
| Syntax | `'A'` | `"A"` |
| Class | ❌ | `java.lang.String` |
| Immutable object | Not applicable | ✅ |

---

# 5. 🏗️ Creating Strings

There are two commonly discussed ways to create a String.

## 5.1 String Literal

```java
String s1 = "Java";
```

This is the most common way.

---

## 5.2 Using `new`

```java
String s2 = new String("Java");
```

This explicitly creates a new String object.

---

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

The String Pool is covered deeply in:

```text
02-String-Pool.md
```

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
==       → compares references
equals() → compares content
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
| Can be concatenated | ✅ Yes |
| Supports `equals()` | ✅ Yes |
| Supports `==` | ✅ Yes, but reference comparison |
| Can be subclassed | ❌ No |
| Thread-safe due to immutability | String contents cannot be mutated |

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

"Java"              "Java Programming"
  ↑                         ↑
  s                    new String
```

To store the new String:

```java
s = s.concat(" Programming");
```

Now:

```text
Java Programming
```

### 🧠 Important

Methods such as:

```java
concat()
toUpperCase()
toLowerCase()
replace()
substring()
trim()
```

do not modify the existing String object.

They return a String result.

Immutability is covered deeply in:

```text
03-String-Immutability.md
```

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

---

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

The integer is converted into a String representation as part of the concatenation.

---

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

### 🧠 Interview Trap

```java
10 + 20 + "Java"
```

→ `30Java`

```java
"Java" + 10 + 20
```

→ `Java1020`

---

# 11. 📏 String Length

To find the number of characters in a String:

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

---

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

Example:

```java
String s = "Java";
```

Conceptually:

```text
J → a → v → a
```

We can convert a String into a character array.

## String → `char[]`

```java
String s = "Java";

char[] chars = s.toCharArray();

for(char c : chars) {
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

---

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

---

## 🆚 String vs `char[]`

| Feature | String | `char[]` |
|---|---|---|
| Type | Class | Array |
| Mutable | ❌ No | ✅ Yes |
| Represents | Character sequence | Characters |
| Has methods | ✅ Many | ❌ Array has no String methods |
| Can change individual character | ❌ No | ✅ Yes |

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

For objects, `==` compares **reference identity**.

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

---

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

---

## 🔥 Comparison Table

| Operator / Method | Compares | Example |
|---|---|---|
| `==` | Reference identity | `s1 == s2` |
| `equals()` | String content | `s1.equals(s2)` |

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
        Stack
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

---

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

Do not oversimplify this as:

> "String is stored in stack."

Wrong.

The local reference can be associated with a stack frame, while the String object is stored in the heap in modern HotSpot JVM implementations.

The String Pool is discussed deeply in:

```text
02-String-Pool.md
```

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

String is commonly used as a key because it is immutable and has content-based `hashCode()` behavior.

Example:

```java
Map<String, Integer> map = new HashMap<>();

map.put("Java", 100);
```

### 6. Security

Strings are commonly used for values involved in class loading, file paths, URLs, configuration, and other security-sensitive operations.

Immutability helps prevent the value from changing unexpectedly after it has been created or shared.

---

# 16. 🛠️ Important String Methods — Preview

String has many useful methods.

A detailed method-by-method discussion will be covered in:

```text
04-String-Methods.md
```

Here is a preview:

| Method | Purpose |
|---|---|
| `length()` | Returns length |
| `charAt()` | Returns character at index |
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
| `trim()` | Removes leading/trailing old-style whitespace |
| `strip()` | Removes leading/trailing Unicode-aware whitespace |
| `replace()` | Replaces characters/sequences |
| `replaceAll()` | Regex-based replacement |
| `split()` | Splits String |
| `concat()` | Concatenates String |
| `isEmpty()` | Checks length == 0 |
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

---

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

But some Unicode characters, including many supplementary characters and certain emoji sequences, can require more than one UTF-16 code unit.

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

> `String.length()` returns the number of UTF-16 code units, not necessarily the number of user-perceived characters.

---

# 18. 🕳️ String and `null`

A String reference can contain `null`.

Example:

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

---

## ⚠️ Calling Method on `null`

```java
String s = null;

System.out.println(s.length());
```

This causes:

```text
NullPointerException
```

because you are trying to call a method through a `null` reference.

---

## Safer Comparison

Instead of:

```java
if(s.equals("Java"))
```

when `s` might be `null`, you can use:

```java
if("Java".equals(s))
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

---

## ❌ Mistake 2: Using `==` for content comparison

Wrong:

```java
if(s1 == s2)
```

when you want to compare text content.

Correct:

```java
if(s1.equals(s2))
```

---

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

---

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

---

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

---

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

explicitly creates another String object and is generally unnecessary for ordinary use.

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

---

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

---

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

---

## Trap 4

```java
System.out.println(10 + 20 + "Java");
```

Output:

```text
30Java
```

---

## Trap 5

```java
System.out.println("Java" + 10 + 20);
```

Output:

```text
Java1020
```

---

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

---

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

String is a `final` class from `java.lang` that represents a sequence of characters.

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

`String` is declared as a final class:

```java
public final class String
```

Therefore it cannot be subclassed.

This helps preserve the designed behavior of String and works together with its immutability and safe sharing.

---

## Q5. What is the difference between `==` and `equals()`?

**Answer:**

```text
==       → compares reference identity
equals() → compares String content
```

Example:

```java
String a = new String("Java");
String b = new String("Java");

System.out.println(a == b);       // false
System.out.println(a.equals(b));  // true
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

Because Strings are frequently used and immutable.

The pool allows eligible equal Strings to be shared, reducing unnecessary duplicate objects.

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
→ sequence of characters/code units
→ "A"
```

---

## Q10. What does `length()` return?

**Answer:**

It returns the number of UTF-16 code units in the String.

For ordinary English characters this usually matches the visible character count, but not always for Unicode supplementary characters or emoji.

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

If character-level mutation is required, use a mutable structure such as `char[]` or `StringBuilder`, depending on the use case.

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

String literals can use the JVM's String Pool, which is associated with the heap in modern HotSpot implementations.

---

# 22. 🎤 30-Second Interview Answer

> **String in Java is a final class from the `java.lang` package that represents a sequence of characters. It is a reference type, not a primitive, and String objects are immutable. Java provides String literals and a String Pool that allows eligible equal Strings to be shared. We normally use `equals()` to compare String content because `==` compares object references. For repeated String modifications, mutable classes such as `StringBuilder` are generally preferred.**

---

# 23. 🧾 Cheat Sheet

```text
╔══════════════════════════════════════════════╗
║              STRING CHEAT SHEET              ║
╠══════════════════════════════════════════════╣
║ Class        → java.lang.String              ║
║ Type         → Reference type                ║
║ Primitive?   → ❌ No                         ║
║ Immutable?   → ✅ Yes                        ║
║ Final?       → ✅ Yes                        ║
║ String Pool  → ✅ Yes                        ║
║ Represents   → Sequence of characters       ║
║ length       → length()                      ║
║ Array size   → length                        ║
║ Collection   → size()                        ║
║ ==           → Reference identity            ║
║ equals()     → Content equality              ║
║ char         → One UTF-16 code unit          ║
║ String       → Sequence of UTF-16 units     ║
╚══════════════════════════════════════════════╝
```

---

# 24. 🧠 Memory Tricks

## 🔥 Remember String with "S-I-F-P"

```text
S → Sequence of characters
I → Immutable
F → Final
P → Pool
```

So whenever someone asks:

> "Tell me important properties of String."

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

Remember:

```text
== 
↓
Identity

equals()
↓
Content
```

### Easy Rule

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

# 25. 🔗 Next Topics

The String playlist continues:

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

Before moving to the next topic, remember these **10 points**:

```text
1. String is a class.
2. String belongs to java.lang.
3. String is a reference type.
4. String is not a primitive.
5. String objects are immutable.
6. String is final.
7. String literals can use the String Pool.
8. == compares references.
9. equals() compares String content.
10. length() returns the number of UTF-16 code units.
```

> ⭐ **Core Idea:** String is not just "text". For Java interviews, you must understand it as a `final`, immutable object with special JVM support through the String Pool.