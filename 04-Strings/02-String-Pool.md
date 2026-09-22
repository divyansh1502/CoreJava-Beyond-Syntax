````md
# 🏊 String Pool in Java

> **The String Pool is a JVM-managed mechanism that maintains canonical `String` instances so that identical String literals and explicitly interned Strings can be shared.**

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
10. [`equals()` with String Pool](#10-equals-with-string-pool)
11. [String Pool and Immutability](#11-string-pool-and-immutability)
12. [Compile-Time String Constants](#12-compile-time-string-constants)
13. [String Concatenation and Pool](#13-string-concatenation-and-pool)
14. [Runtime Concatenation](#14-runtime-concatenation)
15. [`final` Variables and String Pool](#15-final-variables-and-string-pool)
16. [`intern()` Method](#16-intern-method)
17. [`intern()` Internal Idea](#17-intern-internal-idea)
18. [Important Examples](#18-important-examples)
19. [Object Counting Questions](#19-object-counting-questions)
20. [String Pool and Garbage Collection](#20-string-pool-and-garbage-collection)
21. [Advantages](#21-advantages)
22. [Disadvantages / Limitations](#22-disadvantages--limitations)
23. [Common Mistakes](#23-common-mistakes)
24. [Interview Traps](#24-interview-traps)
25. [DSA Relevance](#25-dsa-relevance)
26. [How to Identify String Pool Questions](#26-how-to-identify-string-pool-questions)
27. [Problem-Solving Snippets](#27-problem-solving-snippets)
28. [Top 20 Interview Questions](#28-top-20-interview-questions)
29. [30-Second Interview Answer](#29-30-second-interview-answer)
30. [Cheat Sheet](#30-cheat-sheet)
31. [Memory Tricks](#31-memory-tricks)
32. [Next Topic](#32-next-topic)

---

# 1. 🔹 What is String Pool?

The **String Pool**, also called the **String Intern Pool**, is a JVM-managed mechanism used to maintain canonical `String` instances.

Its main purpose is:

> **Reuse identical immutable String instances instead of unnecessarily creating duplicate instances.**

Example:

```java
String s1 = "Java";
String s2 = "Java";
```

Both references can point to the same pooled `"Java"` object.

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
System.out.println(s1 == s2);
```

Output:

```text
true
```

### Core Idea

```text
String
   ↓
Immutable
   ↓
Safe to share
   ↓
String Pool
   ↓
Reuse identical canonical Strings
```

---

# 2. 🎯 Why Does Java Need String Pool?

Strings are extremely common in Java programs.

For example:

```java
String a = "Java";
String b = "Java";
String c = "Java";
String d = "Java";
```

Creating a separate object for every identical literal would be unnecessary.

Instead, the JVM can maintain one canonical pooled `"Java"` and allow multiple references to point to it.

```text
                String Pool

              ┌──────────────┐
              │    "Java"    │
              └──────┬───────┘
                     │
             ┌───────┼───────┐
             ↓       ↓       ↓
            a        b       c
```

### Main Benefits

- Reduces duplicate String objects.
- Saves memory when identical Strings are reused.
- Works safely because `String` is immutable.
- Provides canonical representations through interning.

### 🧠 Memory Trick

```text
String
   ↓
Immutable
   ↓
Safe Sharing
   ↓
Pooling
```

---

# 3. 📍 Where is String Pool Located?

This is an important interview question.

In modern HotSpot JVMs, interned Strings are stored on the **heap**.

Historically, interned Strings were associated with **PermGen** before Java 7.

### Historical View

```text
Before Java 7

Heap
 ├── Objects
 └── ...

PermGen
 └── String Pool
```

### Java 7+

```text
Heap
 ├── Regular Objects
 ├── String Objects
 └── String Pool / Interned Strings
```

### ⚠️ Important

Do not say:

> "String Pool is stored in the stack."

That is incorrect.

A local reference such as:

```java
String s = "Java";
```

may have its reference associated with a stack frame, but the String object itself is not a stack object.

### Interview Answer

> In modern HotSpot JVMs, interned Strings are stored on the heap. Before Java 7, the String Pool was associated with PermGen.

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

participates in String pooling.

If the same literal appears again:

```java
String s1 = "Java";
String s2 = "Java";
```

both can refer to the same canonical pooled String.

### Important Distinction

Not every String object automatically becomes a pooled String.

Compare:

```java
String a = "Java";
String b = new String("Java");
```

Here:

```text
a → pooled "Java"

b → separate String object
```

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

### Why `==` is true

```java
s1 == s2
```

Both references point to the same pooled String object.

### Why `equals()` is true

```java
s1.equals(s2)
```

Both Strings contain the same characters.

### 🧠 Golden Rule

```text
==

↓

Reference identity


equals()

↓

Content equality
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
          ┌────────────────────┐
          │      "Java"        │
          │      "Python"      │
          └────────────────────┘
              ↑           ↑
              │           │
          ┌───┴───┐       │
          │       │       │
          s1      s2      s3
```

The general idea is:

```text
First "Java"
      ↓
Find/create canonical pooled String

Second "Java"
      ↓
Find existing canonical String
      ↓
Reuse it
```

This is called **interning**.

---

# 7. 🆚 String Pool vs Heap

The String Pool is associated with heap memory in modern HotSpot JVMs.

Consider:

```java
String s1 = "Java";
String s2 = new String("Java");
```

Conceptually:

```text
             Heap

       String Pool
      ┌───────────────┐
      │    "Java"     │
      └───────▲───────┘
              │
             s1


      Separate Object
      ┌───────────────┐
      │    "Java"     │
      └───────▲───────┘
              │
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

### Important

The pool is **not a completely separate memory area like stack or heap** in the sense of being a distinct top-level JVM memory region.

In modern HotSpot, the objects representing interned Strings are on the heap.

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

creates a **new String object**.

The `"Java"` literal used as the constructor argument is itself a pooled literal.

Conceptually:

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

Therefore:

```java
System.out.println(s1 == s2);
```

is:

```text
false
```

while:

```java
System.out.println(s1.equals(s2));
```

is:

```text
true
```

---

# 9. 🔍 `==` with String Pool

`==` checks **reference identity** when used with object references.

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

### With `new`

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
a ──→ String Object A
b ──→ String Object B
```

The contents are equal, but the objects are different.

---

# 10. 🟰 `equals()` with String Pool

`String.equals()` compares the **contents** of two Strings.

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

Both contain:

```text
Java
```

### Golden Rule

```text
==

↓

Same reference/object?

equals()

↓

Same String content?
```

### Recommended Practice

For String content comparison, use:

```java
if (a.equals(b)) {
    // same content
}
```

For null-safe comparison, you can use:

```java
if (Objects.equals(a, b)) {
    // same content or both null
}
```

---

# 11. 🔒 String Pool and Immutability

The String Pool works especially well because `String` is immutable.

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

If Strings were mutable, changing the object through `s1` could unexpectedly affect `s2`.

But String is immutable.

Therefore:

```java
s1 = "Python";
```

does not modify `"Java"`.

It changes the reference stored in `s1`.

Conceptually:

```text
Before:

       "Java"
        ↑   ↑
       s1  s2


After:

"Java"          "Python"
  ↑                 ↑
  s2                s1
```

### Important

Assignment:

```java
s1 = "Python";
```

changes the reference.

It does **not** modify the existing `"Java"` object.

---

# 12. 🔥 Compile-Time String Constants

Java can evaluate certain String expressions during compilation.

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

Therefore this can behave like:

```java
String s1 = "Java";
```

Now:

```java
String s1 = "Ja" + "va";
String s2 = "Java";

System.out.println(s1 == s2);
```

Output:

```text
true
```

### Why?

Because `"Ja" + "va"` is a **compile-time constant expression**.

---

# 13. ➕ String Concatenation and Pool

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

Because the expression can be resolved at compile time.

Conceptually:

```text
"Ja" + "va"
      ↓
Compile Time
      ↓
"Java"
      ↓
String Pool
```

---

## Case 2 — Constant Variables

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

Because `a` is a compile-time constant variable.

---

## Case 3 — Non-final Variable

```java
String a = "Ja";

String s1 = a + "va";
String s2 = "Java";

System.out.println(s1 == s2);
```

Typically:

```text
false
```

because the concatenation is not a compile-time constant expression.

---

# 14. ⚙️ Runtime Concatenation

Consider:

```java
String part = "Ja";

String s1 = part + "va";
String s2 = "Java";

System.out.println(s1 == s2);
```

The value of `part` is treated as a runtime variable for constant-expression rules.

Therefore, you should not assume that `s1` and `s2` refer to the same object.

Typically:

```text
s1 → runtime-created String result

s2 → pooled "Java"
```

So:

```java
System.out.println(s1 == s2);
```

typically produces:

```text
false
```

But:

```java
System.out.println(s1.equals(s2));
```

produces:

```text
true
```

### ⚠️ Important Interview Rule

Never determine String identity only by looking at the final text.

Always ask:

```text
Was the String:

1. A literal?
2. A compile-time constant expression?
3. Created using new?
4. Created by runtime concatenation?
5. Explicitly interned?
```

---

# 15. 🧮 `final` Variables and String Pool

This is a classic interview topic.

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

is a compile-time constant variable.

Therefore:

```java
a + "va"
```

can be resolved during compilation.

Conceptually:

```text
a
↓
"Ja"

"Ja" + "va"
↓
"Java"
↓
String Pool
```

### Compare

Without `final`:

```java
String a = "Ja";

String s1 = a + "va";
String s2 = "Java";

System.out.println(s1 == s2);
```

Typically:

```text
false
```

With `final`:

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

### 🧠 Memory Trick

```text
final + constant String value
        ↓
Compile-Time Constant
        ↓
Compile-Time Folding
        ↓
Potential Pool Sharing
```

---

# 16. 🔄 `intern()` Method

The `intern()` method returns the canonical representation of a String.

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

### What Does `intern()` Do?

Conceptually:

```text
String object
     ↓
intern()
     ↓
Find canonical String in pool
     ↓
Return canonical reference
```

If an equal String already exists in the pool, `intern()` returns that pooled reference.

If it does not exist, the JVM adds the appropriate canonical String representation to the pool and returns it.

---

# 17. ⚙️ `intern()` Internal Idea

Suppose:

```java
String s1 = new String("Java");
```

Conceptually:

```text
String Pool

┌──────────────┐
│    "Java"    │
└──────────────┘


Separate Object

┌──────────────┐
│    "Java"    │ ← s1
└──────────────┘
```

Now:

```java
String s2 = s1.intern();
```

`intern()` returns the canonical pooled `"Java"`.

Conceptually:

```text
String Pool

┌──────────────┐
│    "Java"    │
└───────▲──────┘
        │
        s2


Separate Object

┌──────────────┐
│    "Java"    │
└───────▲──────┘
        │
        s1
```

Therefore:

```java
System.out.println(s1 == s2);
```

Output:

```text
false
```

while:

```java
System.out.println(s2 == "Java");
```

Output:

```text
true
```

### Important

`intern()` does **not** magically change `s1` into the pooled object.

It returns the canonical pooled reference.

Therefore:

```java
String s2 = s1.intern();
```

is important.

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
Same canonical pooled String
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
Two separate String objects
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
System.out.println(a.equals(b));
```

Typical output:

```text
false
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

## Example 8 — Runtime Result Then `intern()`

```java
String x = "Ja";

String a = x + "va";
String b = a.intern();
String c = "Java";

System.out.println(a == c);
System.out.println(b == c);
```

Typical output:

```text
false
true
```

The runtime-created String `a` is separate, while `intern()` returns the canonical pooled `"Java"`.

---

## Example 9 — Empty String

```java
String a = "";
String b = "";

System.out.println(a == b);
```

Output:

```text
true
```

The empty String literal is also a pooled literal.

---

# 19. 🔢 Object Counting Questions

String Pool questions frequently test:

- String literals
- `new`
- compile-time constants
- runtime concatenation
- interning
- reference identity
- object creation

---

## Example 1

```java
String s1 = "Java";
String s2 = "Java";
```

Conceptually:

```text
String Pool

┌──────────────┐
│    "Java"    │
└──────┬───────┘
       │
   ┌───┴───┐
   ↓       ↓
  s1      s2
```

Assuming `"Java"` was not already present:

```text
1 pooled String object
2 references
```

---

## Example 2

```java
String s1 = new String("Java");
```

If `"Java"` is not already in the pool, this can involve:

```text
1 pooled "Java" object
+
1 separately created String object
```

So:

```text
2 String objects
```

may be involved.

### ⚠️ Important Nuance

If `"Java"` was already present in the pool, the statement does not create another pooled `"Java"` object.

It creates the new String object.

Therefore object-count questions depend on the initial state.

---

## Example 3

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
└──────┬───┬───┘
       │   │
      s1  s3


Separate Object

┌──────────────┐
│    "Java"    │
└──────┬───────┘
       │
      s2
```

Objects:

```text
2 String objects
```

References:

```text
3 references
```

---

## Example 4 — Compile-Time Concatenation

```java
String a = "Ja" + "va";
String b = "Java";
```

The expression can be folded into `"Java"`.

So conceptually:

```text
"Java"

 ↑   ↑
 a   b
```

One canonical pooled String can serve both references.

---

## Example 5 — Runtime Concatenation

```java
String x = "Ja";
String a = x + "va";
String b = "Java";
```

Conceptually:

```text
Pool:
"Ja"
"Java"

Runtime-created result:
"Java"
```

The runtime result and pooled `"Java"` should not be assumed to be the same object.

---

# 20. ♻️ String Pool and Garbage Collection

String Pool objects are not necessarily immortal.

Modern JVMs can garbage-collect unused interned Strings when they become unreachable.

Conceptually:

```text
String Pool
     │
     ▼
"TemporaryValue"
     │
No reachable references
     │
     ▼
Eligible for GC
```

### Important

Do not say:

> "String Pool objects can never be garbage collected."

That is an outdated oversimplification.

The exact behavior depends on JVM implementation and garbage collector details.

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

can use one canonical pooled object.

---

## 2. Reduced Duplicate Objects

Repeated String literals do not need separate objects.

---

## 3. Safe Sharing

String immutability makes sharing safe.

---

## 4. Canonical Representation

`intern()` can provide a canonical representation.

---

## 5. Useful for Identity-Based Optimization

When references are known to be canonicalized, reference identity can sometimes be useful.

However, normal application code should still use `equals()` for String content comparison.

---

# 22. ⚠️ Disadvantages / Limitations

## 1. Excessive Interning Can Consume Memory

Interning huge numbers of unique Strings can increase heap usage.

---

## 2. `intern()` Has a Cost

Interning requires the JVM to maintain and search the pool.

Therefore, blindly calling `intern()` on every runtime-created String is not automatically beneficial.

---

## 3. `==` Can Cause Confusion

This:

```java
String a = "Java";
String b = "Java";

System.out.println(a == b);
```

prints:

```text
true
```

but this does **not** mean `==` compares String content.

The result is true because both references refer to the same canonical object.

---

# 23. ❌ Common Mistakes

## Mistake 1

> "String Pool is stored in stack."

❌ Wrong.

Modern HotSpot stores interned String objects on the heap.

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

`new String(...)` creates a new String object.

---

## Mistake 4

> "Every String is automatically pooled."

❌ Wrong.

String literals participate in pooling, and `intern()` can place/use a String's canonical representation in the pool.

Ordinary runtime-created Strings are not automatically pooled simply because they are Strings.

---

## Mistake 5

> "String Pool exists because Strings are mutable."

❌ Opposite.

Immutability makes safe sharing possible.

---

## Mistake 6

> "String Pool and String class are the same."

❌ Wrong.

```text
String
↓
Java class

String Pool
↓
JVM mechanism for canonical String instances
```

---

## Mistake 7

> "`final` always means the String is pooled."

❌ Wrong.

`final` alone is not enough.

The variable must qualify as a compile-time constant variable.

For example:

```java
final String a = "Ja";
```

can be a compile-time constant.

But:

```java
final String a = new String("Ja");
```

is not a compile-time constant variable.

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

Reason:

```text
Compile-time constant expression
```

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

Reason:

```text
x is a compile-time constant variable
```

---

## Trap 8

```java
String a = new String("Java");
String b = a.intern();

System.out.println(a == b);
```

Answer:

```text
false
```

Because `a` is still the separately created object.

---

## Trap 9

```java
String a = new String("Java");
String b = a.intern();

System.out.println(b == "Java");
```

Answer:

```text
true
```

---

## Trap 10

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

# 25. 🧩 DSA Relevance

The String Pool itself is primarily a **Java/JVM concept**, not a DSA algorithm.

However, understanding it is useful when solving String-based DSA problems because Java Strings are objects and their creation, comparison, and memory behavior can affect your implementation.

### Important DSA Connections

| Concept | DSA Relevance |
|---|---|
| `String` immutability | Repeated concatenation can create many objects |
| `StringBuilder` | Efficient repeated string construction |
| `equals()` | Content comparison |
| `==` | Usually not appropriate for content comparison |
| `HashMap<String, Integer>` | Frequency counting |
| `HashSet<String>` | Unique String tracking |
| `intern()` | Canonicalization in specialized cases |
| Character frequency | Common String pattern |
| Anagrams | Hashing/frequency arrays |
| Duplicate detection | HashSet / HashMap |
| Substrings | Sliding window / hashing |
| Palindrome | Two pointers |
| String construction | Builder-based optimization |

### ⚠️ Important DSA Interview Point

Do not use the String Pool as an algorithmic optimization unless you specifically understand why.

For example, for checking equality:

```java
if (s1 == s2) {
    // Do not use this as a general String-content check.
}
```

Use:

```java
if (s1.equals(s2)) {
    // Content equality
}
```

---

# 26. 🧠 How to Identify String Pool Questions

When you see a String interview problem, immediately look for these keywords.

## Pattern 1 — Double Quotes

```java
String a = "Java";
String b = "Java";
```

Ask:

```text
Are these literals?
↓
Are they canonical pooled Strings?
↓
Will references be shared?
```

---

## Pattern 2 — `new String()`

```java
String a = new String("Java");
```

Ask:

```text
Is there a separate String object?
↓
Yes
```

---

## Pattern 3 — `==`

```java
a == b
```

Ask:

```text
Are a and b references to the same object?
```

Do not ask:

```text
Do they contain the same text?
```

---

## Pattern 4 — `equals()`

```java
a.equals(b)
```

Ask:

```text
Do the two Strings have the same content?
```

---

## Pattern 5 — Concatenation

```java
"Ja" + "va"
```

Ask:

```text
Compile-time constant expression?
```

Then compare with:

```java
String x = "Ja";
x + "va"
```

Ask:

```text
Does the expression depend on a variable?
```

---

## Pattern 6 — `final`

```java
final String x = "Ja";
```

Ask:

```text
Is x a compile-time constant variable?
```

If yes, concatenation involving it may be folded at compile time.

---

## Pattern 7 — `intern()`

```java
String b = a.intern();
```

Ask:

```text
What canonical pooled reference does intern() return?
```

---

## 🔥 Fast Identification Formula

```text
String Question
      ↓
Look for "..."
      ↓
Look for new
      ↓
Look for ==
      ↓
Look for equals()
      ↓
Look for +
      ↓
Look for final
      ↓
Look for intern()
      ↓
Determine reference identity
```

---

# 27. 🛠️ Problem-Solving Snippets

These are useful snippets to remember for Java String/DSA interviews.

---

## 1. Correct String Comparison

```java
String a = "Java";
String b = new String("Java");

if (a.equals(b)) {
    System.out.println("Same content");
}
```

---

## 2. Null-Safe String Comparison

```java
String a = null;
String b = "Java";

if (Objects.equals(a, b)) {
    System.out.println("Equal");
}
```

---

## 3. Frequency Counting

```java
String s = "banana";

Map<Character, Integer> frequency = new HashMap<>();

for (char ch : s.toCharArray()) {
    frequency.put(ch, frequency.getOrDefault(ch, 0) + 1);
}

System.out.println(frequency);
```

Expected conceptual result:

```text
b → 1
a → 3
n → 2
```

### DSA Pattern

```text
String
 ↓
Characters
 ↓
HashMap
 ↓
Frequency
```

---

## 4. Duplicate Character Detection

```java
String s = "programming";

Set<Character> set = new HashSet<>();

for (char ch : s.toCharArray()) {
    if (!set.add(ch)) {
        System.out.println("Duplicate: " + ch);
    }
}
```

### Pattern

```text
HashSet
   ↓
Track already-seen elements
   ↓
Duplicate detection
```

---

## 5. Palindrome Check

```java
String s = "madam";

int left = 0;
int right = s.length() - 1;

boolean palindrome = true;

while (left < right) {

    if (s.charAt(left) != s.charAt(right)) {
        palindrome = false;
        break;
    }

    left++;
    right--;
}

System.out.println(palindrome);
```

### Pattern

```text
Two Pointers
   ↓
Compare left/right
   ↓
Move inward
```

Time:

```text
O(n)
```

Extra space:

```text
O(1)
```

---

## 6. Reverse String with `StringBuilder`

```java
String s = "Java";

String reversed = new StringBuilder(s)
        .reverse()
        .toString();

System.out.println(reversed);
```

---

## 7. Efficient Repeated Concatenation

Avoid repeatedly doing:

```java
String result = "";

for (int i = 0; i < 1000; i++) {
    result += i;
}
```

Prefer:

```java
StringBuilder result = new StringBuilder();

for (int i = 0; i < 1000; i++) {
    result.append(i);
}

System.out.println(result);
```

### Why?

`String` is immutable.

Repeated concatenation can create many intermediate String objects.

`StringBuilder` is mutable and is generally preferred for repeated construction in single-threaded code.

---

## 8. Anagram Frequency Pattern

```java
String s1 = "listen";
String s2 = "silent";

int[] frequency = new int[26];

for (char ch : s1.toCharArray()) {
    frequency[ch - 'a']++;
}

for (char ch : s2.toCharArray()) {
    frequency[ch - 'a']--;
}

boolean anagram = true;

for (int count : frequency) {
    if (count != 0) {
        anagram = false;
        break;
    }
}

System.out.println(anagram);
```

### Pattern

```text
String
 ↓
Character frequency
 ↓
Array[26]
 ↓
Compare counts
```

Time:

```text
O(n)
```

Space:

```text
O(1)
```

for a fixed 26-character lowercase alphabet.

---

# 28. 🔥 Top 20 Interview Questions

## Q1. What is String Pool?

**Answer:**

String Pool is a JVM-managed mechanism that maintains canonical String instances, especially String literals and explicitly interned Strings, allowing equal Strings to be shared.

---

## Q2. Why does Java have a String Pool?

**Answer:**

Strings are used frequently and are immutable. Therefore, equal String instances can safely be shared, reducing duplicate objects and memory usage.

---

## Q3. Where is String Pool stored?

**Answer:**

In modern HotSpot JVMs, interned Strings are stored on the heap. Before Java 7, interned Strings were associated with PermGen.

---

## Q4. What happens here?

```java
String a = "Java";
String b = "Java";
```

**Answer:**

Both references can point to the same canonical pooled `"Java"` object.

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

Because `String.equals()` compares String contents rather than reference identity.

---

## Q7. Why is String Pool possible?

**Answer:**

Because String is immutable.

If Strings were mutable, sharing one String object between multiple references could create unexpected changes.

---

## Q8. What is `intern()`?

**Answer:**

`intern()` returns the canonical representation of a String from the String Pool.

---

## Q9. Difference between `==` and `equals()`?

**Answer:**

```text
==        → reference identity
equals()  → content equality
```

---

## Q10. Is every String stored in the String Pool?

**Answer:**

No.

String literals participate in the pool, and `intern()` can provide the canonical pooled representation of a String. Ordinary runtime-created Strings are not automatically pooled merely because they are Strings.

---

## Q11. Why does this return true?

```java
String a = "Ja" + "va";
String b = "Java";

System.out.println(a == b);
```

**Answer:**

Because `"Ja" + "va"` is a compile-time constant expression and can be folded into `"Java"`.

---

## Q12. Why can this return false?

```java
String x = "Ja";

String a = x + "va";
String b = "Java";

System.out.println(a == b);
```

**Answer:**

The concatenation depends on a variable and is not a compile-time constant expression, so the result should not be assumed to be the same pooled reference.

---

## Q13. Why does `final` change the result?

```java
final String x = "Ja";
```

**Answer:**

Because a `final` String variable initialized with a constant expression can be a compile-time constant variable.

Therefore:

```java
x + "va"
```

can be folded at compile time.

---

## Q14. Does `new String("Java")` create one or two objects?

**Answer:**

If the `"Java"` literal is not already present in the pool, the statement can involve:

```text
1 pooled String
+
1 separately created String
```

If the pooled literal already exists, only the new object is created by that statement.

---

## Q15. Can pooled Strings be garbage collected?

**Answer:**

Yes. Unreachable interned Strings can become eligible for garbage collection in modern JVM implementations.

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

It provides the canonical pooled representation of an equal String.

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

Because identical canonical Strings can be shared by multiple references.

---

## Q20. What is the relationship between String Pool and immutability?

**Answer:**

Immutability makes sharing safe because multiple references cannot modify the shared String object's contents.

---

# 29. 🎤 30-Second Interview Answer

> **String Pool is a JVM-managed mechanism that maintains canonical String instances, especially String literals and interned Strings. When the same String literal appears multiple times, the JVM can reuse the same pooled object. This saves memory because String is immutable, so sharing is safe. For example, `String a = "Java"` and `String b = "Java"` can refer to the same pooled object, making `a == b` true. However, `==` checks reference identity, while `equals()` checks String content.**

---

# 30. 🧾 Cheat Sheet

```text
╔══════════════════════════════════════════════════════╗
║                 STRING POOL CHEAT SHEET              ║
╠══════════════════════════════════════════════════════╣
║ String Pool     → JVM-managed interning mechanism    ║
║ Main Purpose    → Reuse canonical String instances  ║
║ String Literal  → Participates in String Pool       ║
║ intern()        → Returns canonical pooled reference║
║ String          → Immutable                         ║
║ String          → final class                       ║
║ Modern HotSpot  → Interned Strings on heap          ║
║ ==              → Reference identity                ║
║ equals()        → Content equality                  ║
║ new String()    → Creates a new String object       ║
║ "Java"          → Pooled literal                    ║
║ "Ja" + "va"     → Compile-time constant expression  ║
║ variable + "va" → Runtime concatenation             ║
║ final constant  → Can enable compile-time folding   ║
╚══════════════════════════════════════════════════════╝
```

---

# 31. 🧠 Memory Tricks

## 🔥 String Pool = "Share Because Safe"

Remember:

```text
String
   ↓
Immutable
   ↓
Cannot be changed
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
Canonical Pool

new String("Java")
   ↓
New Object
```

---

## 🔥 `intern()`

```text
String object
     ↓
intern()
     ↓
Canonical Pool Reference
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
Do not assume same reference
```

---

## 🔥 DSA Memory Trick

```text
String
 ↓
Immutable
 ↓
Repeated modification?
 ↓
Use StringBuilder

String problem
 ↓
Need frequency?
 ↓
HashMap / frequency array

Need uniqueness?
 ↓
HashSet

Need palindrome?
 ↓
Two pointers

Need substring/window?
 ↓
Sliding Window

Need anagram?
 ↓
Frequency counting
```

---

# 32. 🔗 Next Topic

The String playlist continues:

```text
04-Strings/

│
├── 01-String-Introduction.md
│
├── 02-String-Pool.md                  ← YOU ARE HERE
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

Before moving to `03-String-Immutability.md`, remember these **15 points**:

```text
1. String Pool maintains canonical String instances.

2. String literals participate in the String Pool.

3. String is immutable.

4. Immutability makes sharing safe.

5. Modern HotSpot stores interned String objects on the heap.

6. == checks reference identity.

7. equals() checks String content.

8. new String("Java") creates a separate String object.

9. intern() returns the canonical pooled String.

10. "Ja" + "va" can be resolved at compile time.

11. variable + "va" is generally runtime concatenation.

12. final constant String variables can enable compile-time folding.

13. Runtime-created Strings are not automatically pooled merely because they are Strings.

14. String Pool itself is not a DSA data structure.

15. String Pool knowledge helps understand Java String behavior and avoid identity/comparison mistakes in DSA code.
```

> ⭐ **Core Idea:**
>
> **String Pool + Immutability = Safe String Sharing + Reduced Duplicate Objects.**

---

# 🎯 DSA Quick Revision

Before a String-based DSA question, ask:

```text
1. Do I need character frequency?
   → int[] / HashMap

2. Do I need to detect duplicates?
   → HashSet

3. Do I need key-value counts?
   → HashMap

4. Do I need to compare from both ends?
   → Two Pointers

5. Do I need the longest/shortest substring?
   → Sliding Window

6. Do I need an anagram check?
   → Frequency Counting

7. Do I repeatedly build a String?
   → StringBuilder

8. Do I only need String content comparison?
   → equals()

9. Am I comparing String references?
   → ==

10. Am I seeing "..." / new / final / + / intern()?
    → Think String Pool
```

### 🔥 Final Mental Model

```text
              JAVA STRING
                   │
        ┌──────────┼──────────┐
        ↓          ↓          ↓
     Literal      new       Runtime
        │          │          │
        ↓          ↓          ↓
      Pool      New Object   Result
        │
        ↓
    Canonical
    Reference
        │
        ├───────────────┐
        ↓               ↓
       ==            equals()
        ↓               ↓
    Identity          Content
```

> 💡 **Interview Rule:** Never answer a String `==` question by looking only at the text. First trace **how each String was created**.
````
