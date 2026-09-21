🔤 String in Java — Introduction

A String in Java is an object that represents a sequence of characters. Strings are objects of the java.lang.String class and are immutable.

📌 Table of Contents

What is a String?

Why Do We Need Strings?

String as an Object

Creating Strings

String Literal vs new String()

String Class

Strings Are Immutable

String and Character Array

String Concatenation

String Length

Important String Characteristics

String in Memory — Basic View

Why String Is Special in Java

Common Mistakes

Interview Traps

Top 10 Interview Questions

30-Second Interview Answer

Cheat Sheet

🔹 What is a String?

A String is an object that represents a sequence of characters.

Example:

String name = "Divyansh";

Here:

"Divyansh"
    ↓
String object
    ↓
sequence of characters

Characters:

D i v y a n s h

In Java, String is a class:

java.lang.String

Because java.lang is automatically imported, we normally write:

String name;

instead of:

java.lang.String name;

🎯 Why Do We Need Strings?

Programs constantly work with textual data:

Names
Passwords
Emails
Messages
URLs
File paths
JSON
User input
Database data

Example:

String username = "divyansh1502";
String email = "user@example.com";
String city = "Lucknow";

Without a convenient string abstraction, handling text character-by-character would be unnecessarily difficult.

🧱 String as an Object

One important Java concept is:

String is not a primitive data type.

These are primitive types:

int
char
boolean
byte
short
long
float
double

But:

String

is a reference type / class.

Example:

String name = "Java";

The variable name holds a reference to a String object.

🏗️ Creating Strings

There are two common ways to create a String.

1. Using a String Literal

String s1 = "Java";

This is the most common form.

2. Using new

String s2 = new String("Java");

Both represent the text:

Java

But their object creation and memory behavior can differ.

This becomes important when studying the String Pool.

🆚 String Literal vs new String()

Consider:

String s1 = "Java";
String s2 = "Java";
String s3 = new String("Java");

A simplified conceptual picture:

String Pool:

"Java"
  ↑
  ├── s1
  └── s2


Heap:

String object "Java"
  ↑
  └── s3

Therefore:

s1 == s2

is typically:

true

because both literal references can refer to the same pooled String object.

But:

s1 == s3

is:

false

because new String("Java") explicitly creates a separate String object.

However:

s1.equals(s3)

is:

true

because equals() compares String content.

The complete memory behavior is covered in 02-String-Pool.md.

🔒 Strings Are Immutable

One of the most important String concepts:

A String object cannot be changed after it is created.

Example:

String s = "Java";

s.concat(" Programming");

System.out.println(s);

Output:

Java

Why?

Because:

s.concat(" Programming");

creates a new String instead of modifying the existing String object.

To use the new String:

s = s.concat(" Programming");

Now:

Java Programming

The complete internal reason for String immutability is covered in:

03-String-Immutability.md

🔤 String and Character Array

A String represents a sequence of characters.

For example:

String s = "Java";

Conceptually:

J → a → v → a

You can convert between String and character arrays.

String → char[]

char[] chars = s.toCharArray();

char[] → String

char[] chars = {'J', 'a', 'v', 'a'};

String s = new String(chars);

Important:

A char[] is mutable, while a String is immutable.

➕ String Concatenation

Strings can be concatenated using +.

String first = "Java";
String second = "Programming";

String result = first + " " + second;

Result:

Java Programming

You can also concatenate other data types:

int age = 22;

String result = "Age: " + age;

Output:

Age: 22

Java performs string conversion and concatenation.

For repeated or complex string modifications, StringBuilder is generally preferred.

That topic is covered in:

05-StringBuilder.md

📏 String Length

Use:

String s = "Java";

System.out.println(s.length());

Output:

4

Important:

For arrays:

arr.length

For String:

str.length()

For collections:

list.size()

Memory trick

Array       → length
String      → length()
Collection  → size()

⭐ Important String Characteristics

Property

String

Type

Class / reference type

Package

java.lang

Primitive?

❌ No

Mutable?

❌ No

Immutable?

✅ Yes

Thread-safe due to immutability?

String objects cannot be mutated

String pool

✅ Yes

Can be concatenated

✅ Yes

Can be compared using equals()

✅ Yes

Can use ==

✅ Yes, but compares references

Can be subclassed

❌ No, String is final

🧠 Why is String final?

String is declared approximately as:

public final class String

Therefore:

class MyString extends String {
}

is not allowed.

String being immutable and final helps Java safely use String objects in areas such as:

String pooling

Security-sensitive values

Hash-based collections

Class loading and related infrastructure

Caching

Do not reduce this to:

"final is the reason String is immutable."

These are separate properties.

🧠 String in Memory — Basic View

For:

String s = "Java";

a simplified conceptual model is:

Stack
┌──────────┐
│ s        │
│ reference│
└────┬─────┘
     │
     ▼
String Pool / Heap
┌─────────────┐
│ "Java"      │
└─────────────┘

The exact JVM memory implementation is more nuanced, but this model is useful for understanding String pooling.

🔥 Why is String Special in Java?

Strings are used extremely frequently.

Instead of creating unnecessary duplicate String objects:

String a = "Java";
String b = "Java";
String c = "Java";

Java can reuse the same pooled String object for equal literals.

This provides opportunities for:

Memory efficiency

Reuse

Faster reference comparisons in certain situations

String pooling is discussed deeply in:

02-String-Pool.md

⚠️ Common Mistakes

Mistake 1: Thinking String is primitive

String name;

String is a class, not a primitive.

Mistake 2: Comparing Strings with ==

String a = new String("Java");
String b = new String("Java");

System.out.println(a == b);

Output:

false

== compares references.

For content comparison:

a.equals(b);

Output:

true

Mistake 3: Thinking String methods modify the original String

String s = "Java";

s.concat(" World");

System.out.println(s);

Output:

Java

Because String is immutable.

Mistake 4: Confusing length and length()

arr.length       // array
str.length()     // String

Mistake 5: Thinking new String() is always better

Usually:

String s = "Java";

is preferred for ordinary String creation.

Explicit new String(...) is generally unnecessary when you simply need a String with the same contents.

⚠️ Interview Traps

Q: Is String a primitive data type?

No.

It is a class in java.lang.

Q: Is String mutable?

No.

String objects are immutable.

Q: Why does == sometimes return true for Strings?

Because == compares references, and String literals can refer to the same object in the String pool.

Q: Does concat() modify the original String?

No.

It returns a new String.

Q: Is String thread-safe?

A String object cannot be modified after creation, so there are no mutation races on the String object's contents.

Do not confuse this with every operation involving references to Strings being automatically thread-safe.

🔥 Top 10 Interview Questions

1. What is String in Java?

String is a final class in java.lang that represents a sequence of characters.

2. Is String a primitive type?

No. String is a reference type and an object of the java.lang.String class.

3. Why is String immutable?

String immutability supports safe sharing, string pooling, stable hash values, and security-related use cases.

The complete explanation belongs in the String Immutability topic.

4. What is the difference between == and equals() for Strings?

==

compares references.

equals()

compares String content.

5. What is the String Pool?

The String Pool is a JVM-managed pool of String literals and interned strings that allows eligible equal strings to be shared.

6. Why is String final?

String is final, so it cannot be subclassed. This helps preserve its designed behavior and works together with immutability and safe sharing.

7. Can a String object be modified?

No.

Operations that appear to modify a String actually create another String.

8. What happens when Strings are concatenated?

Depending on the expression and compilation context, Java creates the resulting String rather than modifying the original String objects.

For repeated modifications, StringBuilder is usually more appropriate.

9. Why is String commonly used as a HashMap key?

Because String is immutable and has a stable content-based hashCode().

10. Difference between String, StringBuilder, and StringBuffer?

String
→ Immutable

StringBuilder
→ Mutable
→ Generally preferred for single-threaded string construction

StringBuffer
→ Mutable
→ Synchronized methods

A detailed comparison belongs in:

07-String-vs-StringBuilder-vs-StringBuffer.md

🎤 30-Second Interview Answer

String in Java is a final class from the java.lang package that represents a sequence of characters. It is a reference type, not a primitive, and String objects are immutable. Java provides String literals and a String Pool to enable sharing of eligible String objects. We normally compare String contents using equals() rather than ==, because == compares object references.

🧾 Cheat Sheet

╔══════════════════════════════════════════════╗
║              STRING CHEAT SHEET              ║
╠══════════════════════════════════════════════╣
║ Class        → java.lang.String              ║
║ Type         → Reference type                ║
║ Primitive?   → ❌ No                         ║
║ Immutable?   → ✅ Yes                        ║
║ Final?       → ✅ Yes                        ║
║ String Pool  → ✅ Yes                        ║
║ Content      → Sequence of characters       ║
║ Comparison   → equals() for content         ║
║ ==           → Reference comparison          ║
║ Length       → length()                      ║
║ Array length → length                        ║
║ Collection   → size()                        ║
╚══════════════════════════════════════════════╝

🧠 Quick Memory Trick

Remember:

STRING

S → Sequence of characters
T → Type is a class, not primitive
R → Reference type
I → Immutable
N → `java.lang`
G → Gets pooled when represented by eligible literals/interned strings

🔗 Next Topics

After this introduction, continue in this order:

01-String-Introduction.md
        ↓
02-String-Pool.md
        ↓
03-String-Immutability.md
        ↓
04-String-Methods.md
        ↓
05-StringBuilder.md
        ↓
06-StringBuffer.md
        ↓
07-String-vs-StringBuilder-vs-StringBuffer.md
        ↓
08-String-Interview-Questions.md

Core idea: String is a final, immutable Java class used to represent textual data. Understanding its immutability, pooling, reference behavior, and methods is essential for Java interviews.