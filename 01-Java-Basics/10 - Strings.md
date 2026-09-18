# ☕ 10 — Strings

> A `String` in Java is an object that represents a sequence of characters.

```java
String name = "Java";
```

Although strings look like primitive values, `String` is actually a **class** in Java.

```java
String
```

belongs to:

```text
java.lang
```

So no import is required.

---

# 1. Why Is String Important?

Strings are everywhere in Java:

```text
Usernames
Passwords
Emails
URLs
JSON
File paths
Database data
API requests/responses
Logs
```

And Java treats strings specially because they are:

* Objects
* Immutable
* Frequently used
* Stored efficiently through the String Pool
* Heavily involved in interview questions

---

# 2. Creating a String

There are two common ways.

### Using String literal

```java
String s1 = "Java";
```

### Using `new`

```java
String s2 = new String("Java");
```

They look similar but have important memory differences.

---

# 3. String Literal

```java
String s1 = "Java";
```

When Java encounters a string literal, the JVM can use the **String Pool** to reuse an existing string with the same contents.

Conceptually:

```text
String Pool

"Java"
  ↑
  |
s1
```

---

# 4. String Pool

The **String Pool** is a special pool associated with the JVM's string handling where interned string literals are reused.

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
        ↑
        |
s2 ─────┘
```

Both references can point to the same pooled string object.

Therefore:

```java
System.out.println(s1 == s2);
```

Output:

```text
true
```

---

# 5. `new String()`

Now:

```java
String s1 = new String("Java");
String s2 = new String("Java");
```

Each `new String(...)` creates a distinct String object.

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

because the contents are equal.

---

# 6. `==` vs `equals()`

This is one of the **most important String interview questions**.

### `==`

Checks whether two references refer to the same object.

### `equals()`

For `String`, checks whether the character sequences have equal contents.

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

# 7. String Immutability

A `String` object is **immutable**.

Immutable means:

> Once a String object is created, its contents cannot be changed.

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

`concat()` does not modify the existing String.

It creates/returns another String.

Correct:

```java
s = s.concat(" Programming");
```

Now:

```text
Java Programming
```

---

# 8. Important Immutability Example

```java
String s = "Java";

s = s + " World";
```

The original `"Java"` String is not modified.

Conceptually:

```text
Before:

s
↓
"Java"


After:

s
↓
"Java World"
```

The original `"Java"` object remains unchanged.

---

# 9. Why Is String Immutable?

String immutability provides several advantages.

### 1. Security

Strings are commonly used for:

```text
File paths
URLs
Class names
Database connections
Security-related values
```

If strings could change unexpectedly, security-sensitive operations could become problematic.

### 2. String Pool

Because Strings cannot change, the JVM can safely share pooled String objects.

### 3. Thread Safety

Immutable objects can safely be shared between threads without synchronization for state mutation.

### 4. Hashing

Strings are frequently used as keys in hash-based collections.

Their contents don't change after creation, so their hash-based behavior remains stable.

---

# 10. Why String Pool Needs Immutability

Suppose:

```java
String a = "Java";
String b = "Java";
```

Both may refer to the same object.

If Strings were mutable:

```text
a ─────┐
       ↓
     "Java"
       ↑
       |
b ─────┘
```

If `a` changed the shared object:

```text
"Python"
```

then `b` would unexpectedly see `"Python"` too.

Because Strings are immutable, this problem doesn't occur.

---

# 11. String Concatenation

```java
String s = "Hello" + " World";
```

The result is:

```text
Hello World
```

Java supports concatenation using:

```java
+
```

---

# 12. Compile-Time String Concatenation

Consider:

```java
String s = "Ja" + "va";
```

Both operands are compile-time constants.

The compiler can treat the result as:

```text
"Java"
```

This can interact with the String Pool.

Example:

```java
String a = "Java";
String b = "Ja" + "va";

System.out.println(a == b);
```

Output:

```text
true
```

---

# 13. Runtime Concatenation

Consider:

```java
String a = "Ja";

String b = a + "va";
```

Here `a` is a variable whose value participates at runtime.

The result is not simply the same compile-time literal expression.

Therefore:

```java
String c = "Java";

System.out.println(b == c);
```

should not be relied upon as `true`.

For content comparison:

```java
b.equals(c)
```

is the correct approach.

---

# 14. `intern()`

`intern()` returns the canonical pooled representation of a string.

Example:

```java
String s1 = new String("Java");

String s2 = s1.intern();

String s3 = "Java";
```

Now:

```java
System.out.println(s2 == s3);
```

Output:

```text
true
```

Because `s2` refers to the pooled/canonical `"Java"` representation.

---

# 15. String Pool vs Heap

A common simplified interview diagram:

```text
Stack
│
├── s1 ───────────────┐
│                     ↓
│                String Pool
│                  "Java"
│
└── s2 ───────────────┘
```

For:

```java
String s = new String("Java");
```

there can be:

```text
String Pool
    ↓
"Java"

Heap
    ↓
new String object
```

The exact JVM implementation details are more nuanced, so avoid saying "all Strings are always stored in the String Pool."

---

# 16. Important Correction

A common interview statement is:

> "String objects are stored in the String Pool."

This is incomplete.

Better:

> String literals are interned and can be stored/reused through the JVM's String Pool. Explicitly created Strings using `new` create distinct String objects.

---

# 17. String Class Is Final

`String` is declared approximately as:

```java
public final class String
```

Therefore:

```text
String cannot be subclassed.
```

Example:

```java
class MyString extends String {
}
```

❌ Not allowed.

---

# 18. Why Is String Final?

Important reasons include:

* Preserving immutability guarantees
* Preventing subclasses from changing String behavior
* Supporting security assumptions
* Making String behavior predictable

---

# 19. String Implements Interfaces

`String` implements important interfaces such as:

```text
CharSequence
Comparable<String>
Serializable
Constable
ConstantDesc
```

For basic interviews, remember:

```text
String → CharSequence
String → Comparable<String>
```

---

# 20. `String` and `CharSequence`

`CharSequence` is an interface representing a readable sequence of characters.

Examples include:

```text
String
StringBuilder
StringBuffer
```

This allows APIs to accept a broader character-sequence type.

Example:

```java
void print(CharSequence value) {
    System.out.println(value);
}
```

You can pass:

```java
String
StringBuilder
StringBuffer
```

---

# 21. Important String Methods

You should know these very well:

```text
length()
charAt()
substring()
indexOf()
lastIndexOf()
contains()
equals()
equalsIgnoreCase()
compareTo()
compareToIgnoreCase()
startsWith()
endsWith()
toUpperCase()
toLowerCase()
trim()
strip()
replace()
replaceAll()
replaceFirst()
split()
concat()
isEmpty()
isBlank()
toCharArray()
```

---

# 22. `length()`

Returns the number of UTF-16 code units in the String.

```java
String s = "Java";

System.out.println(s.length());
```

Output:

```text
4
```

Important:

```text
String → length()
Array  → length
```

---

# 23. `charAt()`

Returns the `char` at a specified index.

```java
String s = "Java";

System.out.println(s.charAt(0));
```

Output:

```text
J
```

Indexes:

```text
J  a  v  a
0  1  2  3
```

Invalid index:

```java
s.charAt(4);
```

causes:

```text
StringIndexOutOfBoundsException
```

---

# 24. `substring()`

```java
String s = "JavaProgramming";

System.out.println(s.substring(4));
```

Output:

```text
Programming
```

---

### `substring(begin, end)`

```java
s.substring(0, 4);
```

Output:

```text
Java
```

Rule:

```text
[begin, end)
```

Begin is inclusive.

End is exclusive.

---

# 25. `indexOf()`

```java
String s = "Java Programming";

System.out.println(s.indexOf("Pro"));
```

Returns the starting index.

If not found:

```text
-1
```

---

# 26. `lastIndexOf()`

Returns the last occurrence.

```java
String s = "Java Java";

System.out.println(s.lastIndexOf("Java"));
```

It returns the starting index of the final occurrence.

---

# 27. `contains()`

```java
String s = "Java Programming";

System.out.println(s.contains("Java"));
```

Output:

```text
true
```

Returns a boolean.

---

# 28. `equals()`

```java
String a = "Java";
String b = "Java";

System.out.println(a.equals(b));
```

Output:

```text
true
```

Compares contents.

---

# 29. `equalsIgnoreCase()`

```java
String a = "JAVA";
String b = "java";

System.out.println(a.equalsIgnoreCase(b));
```

Output:

```text
true
```

---

# 30. `compareTo()`

Used for lexicographical comparison.

```java
String a = "Apple";
String b = "Banana";

System.out.println(a.compareTo(b));
```

General meaning:

```text
negative → a comes before b
0        → equal
positive → a comes after b
```

Do not rely on the exact numeric value unless you specifically need it.

---

# 31. `startsWith()` and `endsWith()`

```java
String s = "Java Programming";

s.startsWith("Java");       // true
s.endsWith("Programming");  // true
```

Useful for prefix/suffix checking.

---

# 32. `toUpperCase()` / `toLowerCase()`

```java
String s = "Java";

System.out.println(s.toUpperCase());
```

Output:

```text
JAVA
```

Remember:

String is immutable.

Therefore:

```java
s.toUpperCase();
```

does not change `s`.

You need:

```java
s = s.toUpperCase();
```

if you want to update the reference.

---

# 33. `trim()` vs `strip()`

Both remove leading and trailing whitespace, but they are not identical.

```java
String s = "   Java   ";

System.out.println(s.trim());
System.out.println(s.strip());
```

`strip()` uses Unicode-aware whitespace handling.

`trim()` uses the older narrower definition based around characters up to U+0020.

For modern Java, know:

```text
trim()  → older whitespace behavior
strip() → Unicode-aware whitespace
```

---

# 34. `replace()`

Replaces literal character/sequence occurrences.

```java
String s = "Java Java";

System.out.println(s.replace("Java", "Python"));
```

Output:

```text
Python Python
```

---

# 35. `replaceAll()`

Uses a regular expression.

```java
String s = "Java123";

System.out.println(s.replaceAll("\\d", ""));
```

Output:

```text
Java
```

Important difference:

```text
replace()
→ literal replacement

replaceAll()
→ regex-based replacement
```

---

# 36. `replaceFirst()`

Replaces the first substring matching a regular expression.

```java
String s = "Java Java";

System.out.println(
    s.replaceFirst("Java", "Python")
);
```

Output:

```text
Python Java
```

---

# 37. `split()`

Splits a String using a regular expression.

```java
String s = "Java,Python,C++";

String[] arr = s.split(",");

for (String x : arr) {
    System.out.println(x);
}
```

Output:

```text
Java
Python
C++
```

Return type:

```text
String[]
```

---

# 38. `isEmpty()`

Checks whether length is zero.

```java
String s = "";

System.out.println(s.isEmpty());
```

Output:

```text
true
```

---

# 39. `isBlank()`

Checks whether a String is empty or contains only whitespace characters according to its Unicode-aware whitespace rules.

```java
String s = "   ";

System.out.println(s.isBlank());
```

Output:

```text
true
```

Difference:

```text
isEmpty()
→ length == 0

isBlank()
→ empty OR whitespace-only
```

---

# 40. `toCharArray()`

Converts a String into a character array.

```java
String s = "Java";

char[] arr = s.toCharArray();
```

Conceptually:

```text
['J', 'a', 'v', 'a']
```

---

# 41. `concat()`

```java
String a = "Hello";
String b = "World";

String c = a.concat(b);
```

Result:

```text
HelloWorld
```

Unlike `+`, it requires a String argument.

---

# 42. `+` vs `concat()`

```java
String a = "Hello";

String b = a + " World";
```

and:

```java
String b = a.concat(" World");
```

both produce:

```text
Hello World
```

But `+` participates in Java's general string-concatenation semantics, including non-String operands.

Example:

```java
String s = "Age: " + 20;
```

Result:

```text
Age: 20
```

---

# 43. StringBuilder

`StringBuilder` is a mutable sequence of characters.

```java
StringBuilder sb = new StringBuilder("Java");

sb.append(" Programming");

System.out.println(sb);
```

Output:

```text
Java Programming
```

Unlike String:

```text
String
→ immutable

StringBuilder
→ mutable
```

---

# 44. Why StringBuilder?

Consider:

```java
String s = "";

for (int i = 0; i < 1000; i++) {
    s += i;
}
```

Repeated String concatenation can create many intermediate String objects.

For repeated modifications, use:

```java
StringBuilder
```

Example:

```java
StringBuilder sb = new StringBuilder();

for (int i = 0; i < 1000; i++) {
    sb.append(i);
}
```

This is generally more efficient for repeated single-threaded modifications.

---

# 45. Common StringBuilder Methods

```text
append()
insert()
delete()
deleteCharAt()
replace()
reverse()
charAt()
setCharAt()
length()
capacity()
```

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

---

# 46. StringBuffer

`StringBuffer` is also mutable.

```java
StringBuffer sb = new StringBuffer("Java");

sb.append(" Programming");
```

Main interview difference:

```text
StringBuilder
→ mutable
→ not synchronized

StringBuffer
→ mutable
→ synchronized
```

Because StringBuffer's methods are synchronized, it can provide thread-safe operations for its own mutable state, but that does not automatically make every larger operation involving it thread-safe.

---

# 47. String vs StringBuilder vs StringBuffer

| Feature                                     | String             | StringBuilder                    | StringBuffer                         |
| ------------------------------------------- | ------------------ | -------------------------------- | ------------------------------------ |
| Mutable                                     | ❌                  | ✅                                | ✅                                    |
| Thread-safe by immutability/synchronization | Immutable          | ❌                                | Synchronized methods                 |
| Modification                                | Creates new String | Modifies existing builder        | Modifies existing buffer             |
| Typical use                                 | Fixed text         | Repeated single-threaded changes | Legacy/thread-synchronized use cases |
| Performance for repeated modifications      | Usually worse      | Usually better                   | Usually slower than StringBuilder    |

---

# 48. StringBuilder Capacity

```java
StringBuilder sb = new StringBuilder();
```

It maintains an internal character-storage capacity.

You can inspect:

```java
System.out.println(sb.capacity());
```

Capacity and length are different.

```text
length
→ actual characters

capacity
→ current storage capacity before expansion is required
```

---

# 49. `length()` vs `capacity()`

```java
StringBuilder sb = new StringBuilder("Java");

System.out.println(sb.length());
System.out.println(sb.capacity());
```

Length:

```text
4
```

Capacity is larger than the current length.

Remember:

```text
length()
→ actual content size

capacity()
→ allocated character storage capacity
```

---

# 50. StringBuilder vs String Concatenation

For:

```java
String result = a + b;
```

Java may use compiler/JVM optimizations involving concatenation machinery.

For modern Java, do not blindly say:

> "`+` always creates a new StringBuilder."

That was a common oversimplification.

For repeated concatenation in loops, however:

```java
StringBuilder
```

is still the standard explicit choice when you need mutable accumulation.

---

# 51. String and Hashing

Strings are commonly used as keys:

```java
HashMap<String, Integer>
```

String's immutability is particularly useful because changing a key after insertion would make hash-based lookup problematic.

Example concept:

```text
"Java"
  ↓
hashCode()
  ↓
bucket
```

Because the String cannot change, its content-based hash remains stable.

---

# 52. `hashCode()` and `equals()`

For Strings:

```java
a.equals(b)
```

being true implies:

```java
a.hashCode() == b.hashCode()
```

This is important when Strings are used in:

```text
HashMap
HashSet
Hashtable
```

---

# 53. String Pool and Garbage Collection

Interned strings are managed by the JVM and can become eligible for garbage collection when no longer strongly reachable, subject to JVM behavior.

Do not memorize the old statement:

> "String Pool is always in PermGen."

That is outdated.

Modern HotSpot JVMs use the heap for the String Pool/interned strings; PermGen itself was removed in Java 8.

---

# 54. Java 9+ Compact Strings

Modern Java implementations may use a compact internal representation for Strings depending on their contents.

The implementation can store strings using a compact byte-based representation when possible rather than always using two bytes per character.

For interviews:

> Modern Java uses implementation optimizations such as Compact Strings to reduce memory usage for suitable strings.

Do not depend on exact internal fields because they are implementation details.

---

# 55. Unicode and `char`

Java's `char` represents a UTF-16 code unit.

Important:

> A Java `char` is not guaranteed to represent a complete Unicode code point.

Some Unicode characters require a **surrogate pair**, meaning two UTF-16 code units.

Therefore:

```java
String s = "...";
s.length()
```

counts UTF-16 code units, not necessarily user-perceived characters.

For code-point-aware processing, APIs such as:

```java
codePointAt()
codePoints()
```

are relevant.

---

# 56. `String.length()` Interview Trap

Do not always say:

> `length()` gives the number of characters.

More precise:

> `String.length()` returns the number of UTF-16 code units.

For ordinary ASCII text, this normally matches the number of characters people expect.

---

# 57. String Comparison

There are three common concepts:

```text
==

equals()

compareTo()
```

### `==`

Reference identity.

### `equals()`

Content equality.

### `compareTo()`

Lexicographical ordering.

---

# 58. `equalsIgnoreCase()`

```java
String a = "Java";
String b = "JAVA";

System.out.println(a.equalsIgnoreCase(b));
```

Output:

```text
true
```

Useful when case should not matter.

---

# 59. Null and String Methods

Consider:

```java
String s = null;

System.out.println(s.length());
```

This causes:

```text
NullPointerException
```

because `s` does not refer to a String object.

---

# 60. Safe Constant Comparison

Instead of:

```java
if (input.equals("yes"))
```

if `input` might be null, you can write:

```java
if ("yes".equals(input))
```

Why?

`"yes"` is a non-null String literal.

Therefore the call is safe even if:

```text
input == null
```

---

# 61. String and `null`

Important difference:

```java
String s = null;
```

means:

```text
s → no object
```

while:

```java
String s = "";
```

means:

```text
s → empty String object
```

And:

```java
"   "
```

is neither null nor empty; it contains whitespace.

---

# 62. Empty vs Blank vs Null

```text
null
→ no String object

""
→ empty String

"   "
→ blank String
```

Methods:

```text
isEmpty()
→ checks empty

isBlank()
→ checks empty or whitespace-only
```

Neither should be called on a null reference without a null check.

---

# 63. Common String Exceptions

### `StringIndexOutOfBoundsException`

Example:

```java
String s = "Java";

s.charAt(10);
```

---

### `NullPointerException`

Example:

```java
String s = null;

s.length();
```

---

# 64. String as Method Parameter

```java
static void change(String s) {
    s = "Python";
}
```

Calling:

```java
String s = "Java";

change(s);

System.out.println(s);
```

Output:

```text
Java
```

Why?

Java passes the reference value by value, and Strings are immutable.

The local parameter is simply reassigned.

---

# 65. String Immutability + Pass-by-Value

This is a very common interview combination.

```java
static void change(String s) {

    s = s.concat(" World");
}
```

The caller's String does not change.

Why?

```text
1. Java passes reference value by value.
2. String is immutable.
3. concat() returns a new String.
4. Reassignment changes only the local parameter.
```

---

# 66. `String.valueOf()`

Converts values into String representations.

```java
int x = 100;

String s = String.valueOf(x);
```

Now:

```text
s → "100"
```

It has overloads for many primitive/reference types.

---

# 67. `String.join()`

Useful for joining multiple strings.

```java
String result =
        String.join("-", "Java", "Python", "C++");

System.out.println(result);
```

Output:

```text
Java-Python-C++
```

---

# 68. String Formatting

Modern Java provides formatting mechanisms such as:

```java
String.format(...)
```

Example:

```java
String name = "Java";

String result = String.format(
        "Language: %s", name
);
```

Result:

```text
Language: Java
```

For modern applications, also be aware of newer formatting APIs such as `formatted()` and `Formatter`.

---

# 69. Common Interview Traps

## Trap 1

```java
String a = "Java";
String b = "Java";

System.out.println(a == b);
```

Usually:

```text
true
```

because both literals can refer to the same pooled String.

But use:

```java
equals()
```

for content comparison.

---

## Trap 2

```java
String a = new String("Java");
String b = new String("Java");

a == b
```

```text
false
```

Different String objects.

---

## Trap 3

```java
String s = "Java";

s.concat(" World");

System.out.println(s);
```

Output:

```text
Java
```

String is immutable.

---

## Trap 4

```java
String s = null;

s.length();
```

```text
NullPointerException
```

---

## Trap 5

```java
String s = "";
```

This is not null.

It is an empty String.

---

## Trap 6

```java
String s = "Java";

System.out.println(s.length);
```

❌ Invalid.

String uses:

```java
s.length()
```

---

## Trap 7

```java
String s = "Java";

s.charAt(4);
```

❌ Invalid index.

Valid indexes:

```text
0 1 2 3
```

---

## Trap 8

```java
String s = "Java";

s.substring(1, 3);
```

Result:

```text
av
```

Because:

```text
[1, 3)
```

---

# 70. Interview Questions — Complete Set

## Fundamentals

### Q1. What is String in Java?

A `final` class representing a sequence of characters.

### Q2. Is String a primitive type?

No. String is a class/object type.

### Q3. Is String immutable?

Yes.

### Q4. Why is String immutable?

Important reasons include security, safe sharing through the String Pool, thread-safety benefits from immutability, and stable hashing.

### Q5. Why is String final?

To prevent subclassing from changing behavior and potentially violating important String guarantees.

---

# 71. String Pool Questions

### Q6. What is the String Pool?

A JVM-managed pool used for canonical/interned Strings, especially string literals.

### Q7. Why does Java use a String Pool?

To reduce duplicate String objects and improve memory efficiency when identical strings are reused.

### Q8. What happens with:

```java
String a = "Java";
String b = "Java";
```

Both can refer to the same pooled String.

### Q9. What happens with:

```java
new String("Java")
```

A distinct String object is created.

### Q10. What does `intern()` do?

It returns the canonical pooled representation of the String.

---

# 72. Comparison Questions

### Q11. Difference between `==` and `equals()` for String?

```text
==       → reference identity
equals() → content equality
```

### Q12. Why should we use `equals()` for String comparison?

Because it compares String contents rather than object identity.

### Q13. What is `compareTo()`?

It performs lexicographical comparison.

### Q14. Difference between `equals()` and `compareTo()`?

```text
equals()
→ boolean
→ equality

compareTo()
→ int
→ ordering relationship
```

---

# 73. Immutability Questions

### Q15. What does immutable mean?

The object's state cannot be changed after creation.

### Q16. Does `concat()` modify the original String?

No. It returns a String result.

### Q17. Why can Strings be safely shared?

Because their contents cannot be modified.

### Q18. Does:

```java
s = s + "Java";
```

modify the old String?

No. A new String result is produced and assigned to `s`.

---

# 74. StringBuilder / Buffer Questions

### Q19. Why use StringBuilder?

For efficient repeated mutable String construction, especially in single-threaded code.

### Q20. Is StringBuilder immutable?

No. It is mutable.

### Q21. Is StringBuffer mutable?

Yes.

### Q22. StringBuilder vs StringBuffer?

```text
StringBuilder
→ mutable
→ not synchronized
→ generally preferred for single-threaded mutation

StringBuffer
→ mutable
→ synchronized
→ legacy thread-safe alternative
```

### Q23. Why is StringBuilder generally faster than StringBuffer?

Because it does not pay the same synchronization overhead for its methods.

---

# 75. Memory Questions

### Q24. Where are Strings stored?

String objects are heap objects. String literals/interned strings are managed through the JVM's String Pool.

### Q25. Is String Pool in heap?

In modern HotSpot JVMs, yes, interned strings are heap-managed.

### Q26. Is String Pool the same as stack?

No.

### Q27. Does `new String("Java")` create a new object?

Yes, the `new` expression creates a distinct String object.

---

# 76. Advanced Questions

### Q28. What is String interning?

Using a canonical representation of equal String contents through the String Pool.

### Q29. Why is String immutability important for HashMap?

A String used as a key cannot have its contents changed after insertion, helping keep its hash/equality behavior stable.

### Q30. What does `String.length()` actually count?

UTF-16 code units.

### Q31. Can one Unicode character require two Java `char` values?

Yes. Some Unicode code points are represented by surrogate pairs.

### Q32. What is `CharSequence`?

An interface representing a readable sequence of characters.

### Q33. Which classes implement CharSequence?

Common examples:

```text
String
StringBuilder
StringBuffer
```

### Q34. What is the difference between `trim()` and `strip()`?

`trim()` uses older limited whitespace rules; `strip()` uses Unicode-aware whitespace handling.

### Q35. Difference between `replace()` and `replaceAll()`?

```text
replace()
→ literal replacement

replaceAll()
→ regex replacement
```

---

# 77. Output-Based Questions

## Q1

```java
String a = "Java";
String b = "Java";

System.out.println(a == b);
```

Answer:

```text
true
```

---

## Q2

```java
String a = new String("Java");
String b = new String("Java");

System.out.println(a == b);
System.out.println(a.equals(b));
```

Answer:

```text
false
true
```

---

## Q3

```java
String s = "Java";

s.concat(" Programming");

System.out.println(s);
```

Answer:

```text
Java
```

---

## Q4

```java
String s = "Java";

s = s.concat(" Programming");

System.out.println(s);
```

Answer:

```text
Java Programming
```

---

## Q5

```java
String a = "Java";
String b = new String("Java");

System.out.println(a == b);
System.out.println(a.equals(b));
```

Answer:

```text
false
true
```

---

## Q6

```java
String a = new String("Java");
String b = a.intern();

String c = "Java";

System.out.println(b == c);
```

Answer:

```text
true
```

---

## Q7

```java
String a = "Ja" + "va";
String b = "Java";

System.out.println(a == b);
```

Answer:

```text
true
```

The concatenation can be resolved as a compile-time constant.

---

## Q8

```java
String a = "Ja";

String b = a + "va";
String c = "Java";

System.out.println(b == c);
```

Do not use `==` for content comparison here.

The important result is:

```text
b.equals(c) → true
```

while reference identity should not be relied upon as equal.

---

## Q9

```java
String s = "";

System.out.println(s.isEmpty());
System.out.println(s.isBlank());
```

Answer:

```text
true
true
```

---

## Q10

```java
String s = "   ";

System.out.println(s.isEmpty());
System.out.println(s.isBlank());
```

Answer:

```text
false
true
```

---

# 78. More Interview Traps

### Trap — String is not a primitive

```java
String s = "Java";
```

`s` is a reference variable pointing to a String object.

---

### Trap — String Pool does not mean every String is pooled

```java
new String("Java")
```

creates a distinct object.

---

### Trap — `==` does not mean content equality

Always prefer:

```java
a.equals(b)
```

for String content comparison.

---

### Trap — String methods don't mutate

```java
s.toUpperCase();
```

does not update `s`.

---

### Trap — `replaceAll()` is regex-based

```java
s.replaceAll(".", "");
```

`.` is a regex metacharacter and does not mean a literal dot.

---

### Trap — `length()` counts UTF-16 code units

Don't blindly equate it with Unicode code points or user-perceived characters.

---

# 🔥 TOP 10 VVVV IMPORTANT INTERVIEW QUESTIONS

### 1. Why is String immutable?

For security, safe sharing/String Pool benefits, thread-safety advantages, and stable hash-based behavior.

---

### 2. `==` vs `equals()` for String?

```text
==       → reference identity
equals() → content equality
```

---

### 3. What is String Pool?

A JVM-managed pool used for canonical/interned Strings so identical strings can be reused.

---

### 4. What is the difference between:

```java
String a = "Java";
```

and:

```java
String a = new String("Java");
```

A literal can use the String Pool; `new String()` creates a distinct String object.

---

### 5. Why does this not change the String?

```java
String s = "Java";
s.concat(" World");
```

Because String is immutable and `concat()` returns a result rather than modifying the original.

---

### 6. String vs StringBuilder?

```text
String
→ immutable

StringBuilder
→ mutable
→ useful for repeated modifications
```

---

### 7. StringBuilder vs StringBuffer?

```text
StringBuilder
→ mutable
→ not synchronized
→ generally preferred for single-threaded work

StringBuffer
→ mutable
→ synchronized
```

---

### 8. What does `intern()` do?

Returns the canonical pooled representation of a String.

---

### 9. Is String a primitive type?

No.

It is a `final` class in `java.lang`.

---

### 10. What does `String.length()` actually return?

The number of UTF-16 code units in the String.

---

# 🎤 30-Second Interview Answer

> String is a final, immutable class in Java that represents a sequence of characters. Java provides a String Pool so identical interned strings can be reused. The most important distinction is that `==` checks reference identity while `equals()` checks String content. Because String is immutable, operations such as concatenation return new String results instead of modifying the original object. For repeated modifications, StringBuilder is generally preferred, while StringBuffer provides synchronized mutable operations. String is also commonly used in hash-based collections because its immutable content provides stable equality and hashing behavior.

---

# ⚡ Quick Revision

```text
STRING
↓
Class
↓
Object
↓
Immutable
↓
final
↓
java.lang
```

```text
"Java"
↓
String Pool / interned representation

new String("Java")
↓
Distinct String object
```

```text
==
↓
Reference identity

equals()
↓
Content equality

compareTo()
↓
Lexicographical ordering
```

```text
String
→ Immutable

StringBuilder
→ Mutable
→ Usually preferred for repeated single-threaded modifications

StringBuffer
→ Mutable
→ Synchronized
```

```text
length()
→ UTF-16 code units

charAt()
→ char at index

substring()
→ [begin, end)

indexOf()
→ index / -1

contains()
→ boolean

isEmpty()
→ length == 0

isBlank()
→ empty or whitespace-only

replace()
→ literal

replaceAll()
→ regex
```

---

# 🧠 Memory Tricks

```text
"== asks: SAME OBJECT?"

".equals() asks: SAME CONTENT?"

"String = Stone"
→ Can't change

"Builder = Clay"
→ Can modify

"Buffer = Builder + synchronization"

[begin, end)
→ Begin included
→ End excluded

Pool
→ Reuse

intern()
→ Get canonical pooled String

null
→ No object

""
→ Empty object

"   "
→ Blank content
```

---

# 🚀 Progress

```text
01 → Java Introduction              ✓
02 → JVM, JRE & JDK                  ✓
03 → Java Program Structure          ✓
04 → Variables / Data Types etc.     ✓
05 → Exception Handling              ✓
06 → ???                             ✓
07 → ???                             ✓
08 → Methods                         ✓
09 → Arrays                          ✓
10 → Strings                         ✓
```

> **Next:** `11 — Wrapper Classes & Autoboxing/Unboxing`

This is where `int` vs `Integer`, the `valueOf()`/caching behavior, boxing/unboxing, `==` traps, and why collections need wrapper types become important.
