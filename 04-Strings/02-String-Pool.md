# 🏊 String Pool in Java

> **The String Pool is a special mechanism used by the JVM to store and reuse String literals so that identical immutable Strings can be shared instead of creating unnecessary duplicate objects.**

---

# 📌 Table of Contents

1. [What is String Pool?](#1--what-is-string-pool)
2. [Why Does Java Need String Pool?](#2--why-does-java-need-string-pool)
3. [Where is String Pool Located?](#3--where-is-string-pool-located)
4. [String Literal and Pool](#4--string-literal-and-pool)
5. [Basic Example](#5--basic-example)
6. [How String Pool Sharing Works](#6--how-string-pool-sharing-works)
7. [String Pool vs Heap](#7--string-pool-vs-heap)
8. [Using `new String()`](#8--using-new-string)
9. [`==` with String Pool](#9--with-string-pool)
10. [`equals()` with String Pool](#10--equals-with-string-pool)
11. [String Pool and Immutability](#11--string-pool-and-immutability)
12. [Compile-Time String Constants](#12--compile-time-string-constants)
13. [String Concatenation and Pool](#13--string-concatenation-and-pool)
14. [Runtime Concatenation](#14--runtime-concatenation)
15. [`final` Variables and String Pool](#15--final-variables-and-string-pool)
16. [`intern()` Method](#16--intern-method)
17. [`intern()` Internal Idea](#17--intern-internal-idea)
18. [Important Examples](#18--important-examples)
19. [Object Counting Questions](#19--object-counting-questions)
20. [String Pool and Garbage Collection](#20--string-pool-and-garbage-collection)
21. [Advantages](#21--advantages)
22. [Disadvantages / Limitations](#22--disadvantages--limitations)
23. [Common Mistakes](#23--common-mistakes)
24. [Interview Traps](#24--interview-traps)
25. [Top 20 Interview Questions](#25--top-20-interview-questions)
26. [30-Second Interview Answer](#26--30-second-interview-answer)
27. [Cheat Sheet](#27--cheat-sheet)
28. [Memory Tricks](#28--memory-tricks)
29. [Next Topic](#29--next-topic)

---

# 1. 🔹 What is String Pool?

The **String Pool**, also called the **String Intern Pool**, is a special area/mechanism maintained by the JVM for String literals and interned Strings.

Its main purpose is:

> **Reuse identical immutable String objects instead of creating duplicate objects.**

Example:

```java
String s1 = "Java";
String s2 = "Java";
```

Instead of creating two separate `"Java"` objects, Java can reuse the same pooled String.

Conceptually:

```text
                 String Pool
              ┌───────────────┐
              │    "Java"     │
              └───────┬───────┘
                      │
                ┌─────┴─────┐
                ↓           ↓
               s1          s2
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

# 2. 🎯 Why Does Java Need String Pool?

Strings are used **very frequently** in Java programs.

Consider an application containing:

```java
"Java"
"Java"
"Java"
"Java"
"Java"
```

If every occurrence created a completely separate object, memory usage could increase unnecessarily.

Because Strings are immutable, sharing the same String object is safe.

For example:

```java
String s1 = "Java";
String s2 = "Java";
String s3 = "Java";
```

The JVM can allow all three references to point to the same pooled object.

```text
              String Pool
           ┌──────────────┐
           │    "Java"    │
           └──────┬───────┘
                  │
          ┌───────┼───────┐
          ↓       ↓       ↓
         s1      s2      s3
```

### 🧠 Main Reason

```text
String
   ↓
Immutable
   ↓
Cannot be changed
   ↓
Safe to share
   ↓
String Pool can reuse objects
```

---

# 3. 📍 Where is String Pool Located?

This is an important interview topic.

In modern HotSpot JVMs, the String Pool is associated with the **heap**.

Older Java versions had different implementation details, and before Java 7 the pool was associated with the PermGen area.

### Historical View

```text
Older JVMs
───────────────

Heap
│
├── Objects
│
└── ...

PermGen
│
└── String Pool
```

From Java 7 onward, interned Strings were moved to the heap.

```text
Modern HotSpot JVM
────────────────────────

Heap
│
├── Regular Objects
│
├── String Objects
│
└── String Pool
```

### ⚠️ Interview Point

Do not simply say:

> "String Pool is stored in stack."

❌ Wrong.

The String objects are associated with heap memory in modern HotSpot JVMs.

---

# 4. 📝 String Literal and Pool

A String literal is written directly using double quotes.

Example:

```java
String s = "Java";
```

The literal:

```text
"Java"
```

is eligible for the String Pool.

If the same literal appears again:

```java
String s1 = "Java";
String s2 = "Java";
```

the JVM can reuse the pooled String.

---

# 5. 🧪 Basic Example

Consider:

```java
public class Main {

    public static void main(String[] args) {

        String s1 = "Java";
        String s2 = "Java";

        System.out.println(s1 == s2);
        System.out.println(s1.equals(s2));
    }
}
```

Output:

```text
true
true
```

Why?

### `==`

```java
s1 == s2
```

Both references point to the same pooled String.

Therefore:

```text
true
```

### `equals()`

```java
s1.equals(s2)
```

Both contain:

```text
Java
```

Therefore:

```text
true
```

---

# 6. ⚙️ How String Pool Sharing Works

Consider:

```java
String s1 = "Java";
String s2 = "Java";
String s3 = "Python";
```

Conceptually:

```text
                  JVM
                   │
                   ▼
              String Pool
          ┌─────────────────┐
          │     "Java"      │
          │     "Python"    │
          └─────────────────┘
              ↑         ↑
              │         │
        ┌─────┴───┐     │
        │         │     │
        s1        s2    s3
```

The important idea is:

```text
First "Java"
      ↓
Create/find pooled object

Second "Java"
      ↓
Find existing pooled object
      ↓
Reuse it
```

---

# 7. 🆚 String Pool vs Heap

This distinction is extremely important.

Consider:

```java
String s1 = "Java";
String s2 = new String("Java");
```

Conceptually:

```text
             String Pool
          ┌───────────────┐
          │    "Java"     │
          └───────▲───────┘
                  │
                 s1


                 Heap
          ┌───────────────┐
          │    "Java"     │
          └───────▲───────┘
                  │
                 s2
```

`"Java"` is a pooled literal.

`new String("Java")` explicitly creates another String object.

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

---

# 8. 🆕 Using `new String()`

Consider:

```java
String s1 = "Java";
String s2 = new String("Java");
```

Important:

```java
new String("Java")
```

does not mean:

> "Use the pooled object as my object."

It creates a new String object.

The `"Java"` literal itself can already exist in the pool.

So conceptually:

```text
String Pool
┌──────────────┐
│    "Java"    │ ← literal
└──────────────┘

Heap
┌──────────────┐
│    "Java"    │ ← new String(...)
└──────────────┘
```

Thus:

```java
s1 == s2
```

is:

```text
false
```

---

# 9. 🔍 `==` with String Pool

`==` checks whether two references refer to the same object.

Example:

```java
String a = "Java";
String b = "Java";

System.out.println(a == b);
```

Output:

```text
true
```

Because:

```text
a ──┐
    ├──→ "Java" in String Pool
b ──┘
```

---

## Another Example

```java
String a = new String("Java");
String b = new String("Java");

System.out.println(a == b);
```

Output:

```text
false
```

Because:

```text
a ──→ Object A: "Java"

b ──→ Object B: "Java"
```

The content is the same, but the objects are different.

---

# 10. 🟰 `equals()` with String Pool

`String.equals()` compares String content.

Example:

```java
String a = "Java";
String b = new String("Java");

System.out.println(a.equals(b));
```

Output:

```text
true
```

Because both contain:

```text
Java
```

### 🧠 Golden Rule

```text
== 
↓
Same object/reference?

equals()
↓
Same content?
```

---

# 11. 🔒 String Pool and Immutability

The String Pool works especially well because String is immutable.

Suppose:

```java
String s1 = "Java";
String s2 = "Java";
```

Both may refer to the same object:

```text
       "Java"
        ↑   ↑
       s1  s2
```

What if String were mutable?

Suppose:

```text
s1 changes "Java" → "Python"
```

Then `s2` could unexpectedly see:

```text
"Python"
```

That would be dangerous.

But String is immutable.

So:

```java
s1 = "Python";
```

does not modify the old `"Java"` object.

Instead:

```text
Before:

"Java"
 ↑   ↑
s1  s2


After:

"Java"       "Python"
  ↑             ↑
 s2             s1
```

This is one of the major reasons pooling is safe.

---

# 12. 🔥 Compile-Time String Constants

Java can determine some String expressions during compilation.

Example:

```java
String s1 = "Ja" + "va";
```

The compiler can evaluate:

```text
"Ja" + "va"
```

as:

```text
"Java"
```

So conceptually it can behave like:

```java
String s1 = "Java";
```

Now:

```java
String s2 = "Java";

System.out.println(s1 == s2);
```

Output:

```text
true
```

---

# 13. ➕ String Concatenation and Pool

String concatenation has an important relationship with the pool.

## Case 1 — Literal + Literal

```java
String s1 = "Ja" + "va";
String s2 = "Java";

System.out.println(s1 == s2);
```

Output:

```text
true
```

Why?

Because:

```java
"Ja" + "va"
```

can be evaluated at compile time.

The result is effectively:

```text
"Java"
```

which is eligible for pooling.

---

# 14. ⚙️ Runtime Concatenation

Now consider:

```java
String part = "Ja";

String s1 = part + "va";
String s2 = "Java";

System.out.println(s1 == s2);
```

Here:

```java
part + "va"
```

depends on the value of a variable at runtime.

It is not the same compile-time constant expression as:

```java
"Ja" + "va"
```

Therefore `s1` should not be assumed to refer to the same pooled object as `s2`.

Typically:

```text
s1 → newly created result
s2 → "Java" in pool
```

So:

```java
s1 == s2
```

is typically:

```text
false
```

Use:

```java
s1.equals(s2)
```

to compare content.

---

# 15. 🧮 `final` Variables and String Pool

This is a classic interview trap.

Consider:

```java
final String a = "Ja";

String s1 = a + "va";
String s2 = "Java";

System.out.println(s1 == s2);
```

Output:

```text
true
```

Why?

Because:

```java
final String a = "Ja";
```

is a compile-time constant variable because it is:

```text
final
+
String constant expression
```

Therefore the compiler can evaluate:

```java
a + "va"
```

as:

```text
"Java"
```

---

## Compare

### Without `final`

```java
String a = "Ja";

String s1 = a + "va";
String s2 = "Java";

System.out.println(s1 == s2);
```

Do not expect pooling based on compile-time concatenation.

---

### With `final`

```java
final String a = "Ja";

String s1 = a + "va";
String s2 = "Java";

System.out.println(s1 == s2);
```

The expression can be folded into the same String literal.

Therefore:

```text
true
```

---

# 16. 🔄 `intern()` Method

The `intern()` method is defined by String:

```java
public native String intern();
```

Conceptually, `intern()` gives you the canonical pooled representation of a String.

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

# 17. ⚙️ `intern()` Internal Idea

Suppose:

```java
String s1 = new String("Java");
```

You have a separate String object.

Now:

```java
String s2 = s1.intern();
```

Conceptually:

```text
Before:

String Pool
┌──────────────┐
│    "Java"    │
└──────────────┘

Heap
┌──────────────┐
│    "Java"    │ ← s1
└──────────────┘
```

After:

```java
String s2 = s1.intern();
```

Conceptually:

```text
String Pool
┌──────────────┐
│    "Java"    │
└───────▲──────┘
        │
        s2
```

`s1` itself is still the separately created object.

```text
Heap
┌──────────────┐
│    "Java"    │
└───────▲──────┘
        │
        s1
```

So:

```java
s1 == s2
```

is:

```text
false
```

while:

```java
s2 == "Java"
```

is:

```text
true
```

assuming the corresponding literal is the canonical pooled String.

---

# 18. 🧪 Important Examples

## Example 1 — Same Literal

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

```text
Same pooled String
```

---

## Example 2 — `new`

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

```text
Two different objects
```

---

## Example 3 — Content Comparison

```java
String a = new String("Java");
String b = new String("Java");

System.out.println(a.equals(b));
```

Output:

```text
true
```

---

## Example 4 — Literal vs `new`

```java
String a = "Java";
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

## Example 5 — Compile-Time Concatenation

```java
String a = "Ja" + "va";
String b = "Java";

System.out.println(a == b);
```

Output:

```text
true
```

---

## Example 6 — Runtime Concatenation

```java
String x = "Ja";

String a = x + "va";
String b = "Java";

System.out.println(a == b);
```

Typical output:

```text
false
```

But:

```java
System.out.println(a.equals(b));
```

Output:

```text
true
```

---

## Example 7 — `intern()`

```java
String a = new String("Java");
String b = a.intern();

System.out.println(a == b);
System.out.println(b == "Java");
```

Output:

```text
false
true
```

---

# 19. 🔢 Object Counting Questions

String Pool questions are frequently used in interviews to test your understanding of:

- String literals
- `new`
- pooling
- compile-time constants
- runtime concatenation

---

## Example 1

```java
String s1 = "Java";
String s2 = "Java";
```

### Objects

One pooled String:

```text
"Java"
```

References:

```text
s1 ──┐
     ├──→ "Java"
s2 ──┘
```

### Result

Conceptually:

```text
1 String object
2 references
```

---

# Example 2

```java
String s1 = new String("Java");
```

Potentially relevant String objects:

```text
"Java" literal → pooled object
new String(...) → separate object
```

Therefore, when the literal was not already present, this statement can involve **two String objects**:

```text
String Pool
┌──────────────┐
│    "Java"    │
└──────────────┘

Heap
┌──────────────┐
│    "Java"    │
└──────────────┘
```

### ⚠️ Interview Nuance

Object-count questions can depend on whether the literal already exists in the pool.

For example, if `"Java"` was previously used, the pooled object already exists.

So always ask yourself:

```text
Does the literal already exist?
```

before counting newly created objects.

---

# Example 3

```java
String s1 = "Java";
String s2 = new String("Java");
String s3 = "Java";
```

Conceptually:

```text
String Pool

┌──────────────┐
│    "Java"    │
└──────▲───▲───┘
       │   │
      s1  s3


Heap

┌──────────────┐
│    "Java"    │
└──────▲───────┘
       │
      s2
```

So there are:

```text
2 String objects
3 references
```

assuming no relevant prior objects are counted.

---

# 20. ♻️ String Pool and Garbage Collection

String Pool objects are not magically immortal.

Modern JVM implementations can allow unused interned Strings to become eligible for garbage collection when they are no longer reachable.

Conceptually:

```text
String Pool
    │
    ▼
"Java"
    │
No references
    │
    ▼
Eligible for GC
```

However, exact garbage collection behavior depends on the JVM and runtime conditions.

### 🧠 Important

Do not say:

> "String Pool objects can never be garbage collected."

That is an outdated oversimplification.

---

# 21. ✅ Advantages of String Pool

## 1. Memory Efficiency

Identical Strings can be shared.

```text
"Java"
"Java"
"Java"
"Java"
```

can refer to one pooled object.

---

## 2. Reduced Duplicate Objects

Without pooling, repeated identical literals could create unnecessary duplicate objects.

---

## 3. Faster Identity Comparison in Some Cases

When two references point to the same pooled String:

```java
s1 == s2
```

can immediately evaluate to:

```text
true
```

However, this should **not** be used as the normal way to compare String content.

Use:

```java
equals()
```

---

## 4. Works Well with Immutability

Because Strings cannot be changed, sharing is safe.

---

# 22. ⚠️ Disadvantages / Limitations

## 1. Pooling Is Not a Replacement for Good String Design

Do not manually intern every String without understanding the workload.

---

## 2. Large Numbers of Unique Strings

If an application creates huge numbers of unique Strings and interns them unnecessarily, the pool can consume significant heap memory.

---

## 3. `==` Can Cause Confusion

Developers may see:

```java
String a = "Java";
String b = "Java";

a == b
```

and incorrectly conclude:

> "`==` compares String content."

It does not.

It compares references.

The result is true because of pooling.

---

# 23. ❌ Common Mistakes

## Mistake 1

> "String Pool is stored in stack."

❌ Wrong.

Modern HotSpot implementations associate the String Pool with the heap.

---

## Mistake 2

> "`==` compares String content."

❌ Wrong.

For object references:

```java
==
```

checks reference identity.

---

## Mistake 3

> "`new String()` uses the pooled object directly."

❌ Wrong.

It explicitly creates a new String object.

---

## Mistake 4

> "Every String is automatically pooled."

❌ Too broad.

String literals and explicitly interned Strings participate in the pool.

Ordinary runtime-created Strings are not automatically pooled merely because they are Strings.

---

## Mistake 5

> "String Pool exists because Strings are mutable."

❌ Opposite.

Pooling is practical precisely because Strings are immutable.

---

## Mistake 6

> "String Pool and String class are the same thing."

❌ No.

```text
String
↓
A Java class

String Pool
↓
JVM mechanism/area for canonical String instances
```

---

# 24. 🚨 Interview Traps

## Trap 1

```java
String s1 = "Java";
String s2 = "Java";

System.out.println(s1 == s2);
```

Answer:

```text
true
```

---

## Trap 2

```java
String s1 = new String("Java");
String s2 = new String("Java");

System.out.println(s1 == s2);
```

Answer:

```text
false
```

---

## Trap 3

```java
String s1 = new String("Java");
String s2 = "Java";

System.out.println(s1 == s2);
```

Answer:

```text
false
```

---

## Trap 4

```java
String s1 = new String("Java");
String s2 = "Java";

System.out.println(s1.equals(s2));
```

Answer:

```text
true
```

---

## Trap 5

```java
String s1 = "Ja" + "va";
String s2 = "Java";

System.out.println(s1 == s2);
```

Answer:

```text
true
```

Because the concatenation can be evaluated at compile time.

---

## Trap 6

```java
String x = "Ja";

String s1 = x + "va";
String s2 = "Java";

System.out.println(s1 == s2);
```

Typical answer:

```text
false
```

because the concatenation depends on a runtime variable.

---

## Trap 7

```java
final String x = "Ja";

String s1 = x + "va";
String s2 = "Java";

System.out.println(s1 == s2);
```

Answer:

```text
true
```

because `x` is a compile-time constant variable.

---

# 25. 🔥 Top 20 Interview Questions

## Q1. What is String Pool?

**Answer:**

String Pool is a JVM mechanism used to maintain canonical instances of String literals and interned Strings so that identical immutable Strings can be shared.

---

## Q2. Why does Java have a String Pool?

**Answer:**

Strings are used very frequently and are immutable. Therefore, identical Strings can safely be shared, reducing unnecessary duplicate objects.

---

## Q3. Where is String Pool stored?

**Answer:**

In modern HotSpot JVMs, the String Pool is associated with the heap. Before Java 7, interned Strings were associated with PermGen.

---

## Q4. What happens here?

```java
String a = "Java";
String b = "Java";
```

**Answer:**

Both references can point to the same pooled `"Java"` object.

Therefore:

```java
a == b
```

is:

```text
true
```

---

## Q5. What happens here?

```java
String a = new String("Java");
String b = new String("Java");
```

**Answer:**

Two separate String objects are created.

Therefore:

```java
a == b
```

is:

```text
false
```

---

## Q6. Why does `equals()` return true?

```java
a.equals(b)
```

**Answer:**

Because `String.equals()` compares the contents of the Strings.

---

## Q7. Why is String Pool possible?

**Answer:**

Because String is immutable.

If pooled Strings could be modified, sharing them could cause one reference to unexpectedly affect another.

---

## Q8. What is `intern()`?

**Answer:**

`intern()` returns the canonical representation of the String from the String Pool.

Example:

```java
String a = new String("Java");

String b = a.intern();
```

`b` refers to the pooled `"Java"`.

---

## Q9. What is the difference between `==` and `equals()` for Strings?

**Answer:**

```text
==       → reference identity
equals() → content equality
```

---

## Q10. Is every String stored in the String Pool?

**Answer:**

No.

String literals and Strings explicitly interned with `intern()` participate in the String Pool.

Ordinary runtime-created Strings do not automatically become pooled simply because they are Strings.

---

## Q11. Why does this return true?

```java
String a = "Ja" + "va";
String b = "Java";

System.out.println(a == b);
```

**Answer:**

Because `"Ja" + "va"` is a compile-time constant expression and can be folded into the literal `"Java"`.

---

## Q12. Why can this return false?

```java
String x = "Ja";

String a = x + "va";
String b = "Java";

System.out.println(a == b);
```

**Answer:**

Because the concatenation depends on a variable at runtime, so it should not be assumed to produce the same pooled reference.

---

## Q13. Why does `final` change the result?

```java
final String x = "Ja";
```

**Answer:**

Because a `final` String initialized with a constant expression can be a compile-time constant variable.

Therefore:

```java
x + "va"
```

can be evaluated during compilation.

---

## Q14. Does `new String("Java")` create one or two objects?

**Answer:**

Potentially two String objects are involved if `"Java"` is not already in the pool:

```text
1. Pooled literal "Java"
2. New String object
```

If the pooled literal already exists, the statement creates only the new object at that point.

---

## Q15. Can pooled Strings be garbage collected?

**Answer:**

Yes. Modern JVMs can garbage-collect unused interned Strings when they are no longer reachable, subject to JVM implementation and GC behavior.

---

## Q16. Should we use `==` to compare Strings?

**Answer:**

Normally, no.

Use:

```java
equals()
```

for content comparison.

---

## Q17. What is the purpose of `intern()`?

**Answer:**

It allows a String to use the canonical pooled representation.

---

## Q18. Is String Pool the same as String class?

**Answer:**

No.

```text
String
→ Java class

String Pool
→ JVM mechanism for canonical String instances
```

---

## Q19. Why is String Pool memory-efficient?

**Answer:**

Because identical pooled Strings can be shared by multiple references.

---

## Q20. What is the relationship between String Pool and immutability?

**Answer:**

Immutability makes sharing safe.

If multiple references point to the same String object, one reference cannot modify the object's contents.

---

# 26. 🎤 30-Second Interview Answer

> **String Pool is a JVM mechanism used to store and reuse canonical String literals and interned Strings. When the same String literal occurs multiple times, the JVM can reuse the same pooled object. This saves memory because Strings are immutable, so sharing them is safe. For example, `String a = "Java"` and `String b = "Java"` can point to the same pooled object, making `a == b` true. However, `==` compares references, while `equals()` compares String content.**

---

# 27. 🧾 Cheat Sheet

```text
╔════════════════════════════════════════════════════╗
║                 STRING POOL CHEAT SHEET            ║
╠════════════════════════════════════════════════════╣
║ String Pool        → JVM String interning mechanism ║
║ Main Purpose       → Reuse identical Strings       ║
║ String Literals    → Participate in Pool           ║
║ intern()           → Returns canonical pooled ref  ║
║ String             → Immutable                     ║
║ String             → final class                   ║
║ Modern HotSpot     → Pool associated with heap     ║
║ ==                 → Reference identity            ║
║ equals()           → Content equality              ║
║ new String()       → New String object             ║
║ "Java"             → Pooled literal                ║
║ "Ja" + "va"        → Compile-time constant         ║
║ variable + "va"    → Runtime concatenation         ║
╚════════════════════════════════════════════════════╝
```

---

# 28. 🧠 Memory Tricks

## 🔥 String Pool = "Share Because Safe"

Remember:

```text
String
  ↓
Immutable
  ↓
Cannot change
  ↓
Safe to share
  ↓
String Pool
```

---

## 🔥 `==` vs `equals()`

```text
== 
↓
Identity

equals()
↓
Content
```

---

## 🔥 Literal vs `new`

```text
"Java"
   ↓
Pool

new String("Java")
   ↓
New Object
```

---

## 🔥 Compile Time vs Runtime

```text
"Ja" + "va"
      ↓
Compile Time
      ↓
"Java"
      ↓
Pool
```

But:

```text
variable + "va"
       ↓
Runtime
       ↓
Don't assume same reference
```

---

# 29. 🔗 Next Topic

The String playlist continues:

```text
04-Strings/
│
├── 01-String-Introduction.md
│
├── 02-String-Pool.md              ← YOU ARE HERE
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
01 String Introduction
          │
          ▼
02 String Pool
          │
          ▼
03 String Immutability
          │
          ▼
04 String Methods
          │
          ▼
05 StringBuilder
          │
          ▼
06 StringBuffer
          │
          ▼
07 String vs Builder vs Buffer
          │
          ▼
08 String Interview Questions
```

---

# 🚀 Final Revision

Before moving to `03-String-Immutability.md`, remember these **12 points**:

```text
1. String Pool stores/reuses canonical String instances.
2. String literals participate in the String Pool.
3. String is immutable.
4. Immutability makes sharing safe.
5. Modern HotSpot associates the String Pool with the heap.
6. == checks reference identity.
7. equals() checks String content.
8. new String("Java") creates a separate String object.
9. intern() returns the canonical pooled String.
10. "Ja" + "va" can be resolved at compile time.
11. variable + "va" is generally runtime concatenation.
12. final constant String variables can enable compile-time folding.
```

> ⭐ **Core Idea:**  
> **String Pool + Immutability = Safe String Sharing + Reduced Duplicate Objects.**