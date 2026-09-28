# 🔤 String — Interview Questions & Answers

> **A focused collection of Java String interview questions with concise, interview-ready answers.**

---

# 📑 Table of Contents

- [1. String Basics](#1-string-basics)
- [2. String Pool and Memory](#2-string-pool-and-memory)
- [3. String Immutability](#3-string-immutability)
- [4. String Comparison](#4-string-comparison)
- [5. String Methods](#5-string-methods)
- [6. StringBuilder and StringBuffer](#6-stringbuilder-and-stringbuffer)
- [7. String Internals](#7-string-internals)
- [8. String Tricky Questions](#8-string-tricky-questions)
- [9. String Coding Questions](#9-string-coding-questions)
- [10. Rapid String Revision](#10-rapid-string-revision)

---

# 1. String Basics

## 1. What is String in Java?

`String` is a class in Java used to represent sequences of characters.

It belongs to:

```text
java.lang
```

Because `java.lang` is automatically imported, no explicit import is required.

---

## 2. Is String a primitive data type?

No.

`String` is a reference type and is an object of the `java.lang.String` class.

---

## 3. Is String a class?

Yes.

```text
java.lang.String
```

is a final class in Java.

---

## 4. Why is String declared final?

String is final so that its behavior and immutability characteristics cannot be changed through subclassing.

This also supports security, pooling, and predictable behavior.

---

## 5. How many ways can you create a String?

Two common forms are:

### String literal

```java
String s =
    "Java";
```

### Using `new`

```java
String s =
    new String("Java");
```

They can differ in how objects are created and reused through the String pool.

---

## 6. What is the difference between String literal and `new String()`?

```java
String a =
    "Java";
```

The literal can use an existing pooled String.

```java
String b =
    new String("Java");
```

The `new` expression creates a distinct String object.

So:

```text
String literal
→ may reuse pooled object


new String()
→ creates a new String object
```

---

## 7. Is String immutable?

Yes.

Once a String object is created, its character sequence cannot be changed.

Operations that appear to modify a String return another String.

---

## 8. What does immutable mean?

Immutable means that an object's state cannot be changed after the object has been created.

For String:

```text
"Java"
```

cannot be changed into:

```text
"JAVA"
```

The operation creates or returns another String instead.

---

# 2. String Pool and Memory

## 9. What is the String Pool?

The String Pool is a JVM-managed pool of canonical String instances.

String literals are interned and can share the same object when they represent the same character sequence.

---

## 10. Why does Java use the String Pool?

String pooling can reduce duplicate String objects and therefore reduce unnecessary memory usage.

It also allows equal string literals to share a canonical instance.

---

## 11. Where is the String Pool located?

In modern JVM implementations, the String Pool is associated with the heap.

Older Java versions had different memory arrangements, so answers should specify the Java version when discussing historical behavior.

---

## 12. What happens when we write this?

```java
String s =
    "Java";
```

The JVM uses the String pool for the literal.

If an equivalent interned String already exists, the reference can point to that existing object.

Otherwise, an appropriate pooled String is created.

---

## 13. What happens with this?

```java
String s =
    new String("Java");
```

The literal `"Java"` is resolved through the String pool, while the `new String(...)` expression creates a separate String object.

Conceptually:

```text
String Pool
    |
   "Java"
    ↑
literal

Heap
    |
new String("Java")
```

---

## 14. What is `intern()`?

`intern()` returns the canonical representation of a String from the String pool.

Example:

```java
String a =
    new String("Java");

String b =
    a.intern();
```

`b` refers to the pooled representation of `"Java"`.

---

## 15. What is the purpose of `intern()`?

It can be used when canonicalized strings are useful, such as when many duplicate string values exist and identity sharing is desirable.

However, indiscriminate interning is not automatically beneficial and should be used with an understanding of memory and application behavior.

---

# 3. String Immutability

## 16. Why is String immutable?

Important reasons include:

```text
Security
String pooling
Thread-safety benefits
Hash-code stability
Predictable behavior
```

---

## 17. How does immutability help String Pooling?

Suppose multiple references point to the same pooled String:

```text
"Java"
   ↑
   |
 +---+---+
 |       |
 s1      s2
```

If String were mutable, changing it through one reference would unexpectedly affect the other reference.

Immutability makes sharing safe.

---

## 18. How does immutability help HashMap?

Strings are commonly used as keys.

Because String content cannot change after creation, its hash code remains consistent.

This allows it to work reliably as a hash-based collection key.

---

## 19. Does `toUpperCase()` modify the original String?

No.

Example:

```java
String s =
    "java";

s.toUpperCase();
```

The original String remains:

```text
"java"
```

The returned String is:

```text
"JAVA"
```

To store it:

```java
s =
    s.toUpperCase();
```

Now `s` refers to the new String.

---

## 20. Does the old String still exist after reassignment?

If no reachable reference points to the old String, it becomes eligible for garbage collection.

It is not necessarily removed immediately.

---

## 21. Does String immutability mean the reference variable cannot change?

No.

The reference variable can be reassigned.

Example:

```java
String s =
    "Java";

s =
    "Python";
```

The variable `s` changed what object it references.

The original `"Java"` object itself was not modified.

---

# 4. String Comparison

## 22. Difference between `==` and `equals()` for String?

`==` compares reference identity.

`equals()` compares String contents.

Example:

```java
String a =
    new String("Java");

String b =
    new String("Java");
```

```text
a == b
→ false
```

```text
a.equals(b)
→ true
```

---

## 23. Why can `==` return true for String literals?

Because identical string literals can refer to the same pooled String.

Example:

```java
String a =
    "Java";

String b =
    "Java";
```

Typically:

```text
a == b
→ true
```

because both references can refer to the same pooled object.

---

## 24. Is `==` recommended for String content comparison?

No.

Use:

```java
equals()
```

for content comparison.

---

## 25. What is `equalsIgnoreCase()`?

It compares two Strings while ignoring case differences.

Example:

```java
"Java".equalsIgnoreCase(
    "JAVA"
);
```

returns:

```text
true
```

---

## 26. What is the difference between `equals()` and `compareTo()`?

```text
equals()
→ returns boolean
→ checks equality


compareTo()
→ returns integer
→ provides lexicographical ordering information
```

---

## 27. What does `compareTo()` return?

Conceptually:

```text
0
→ strings are equal in ordering


negative
→ first string comes before second


positive
→ first string comes after second
```

The exact positive or negative value should not generally be treated as a specific number.

---

## 28. What is `compareToIgnoreCase()`?

It performs lexicographical comparison while ignoring case differences.

---

# 5. String Methods

## 29. What does `length()` do?

It returns the number of UTF-16 code units in the String.

Example:

```java
String s =
    "Java";

int length =
    s.length();
```

---

## 30. What does `charAt()` do?

It returns the UTF-16 `char` at the specified index.

Example:

```java
String s =
    "Java";

char c =
    s.charAt(0);
```

Result:

```text
J
```

---

## 31. What does `substring()` do?

It returns a portion of a String.

Example:

```java
String s =
    "JavaProgramming";

String result =
    s.substring(4);
```

Result:

```text
Programming
```

---

## 32. What is the difference between `substring(beginIndex)` and `substring(beginIndex, endIndex)`?

```text
substring(beginIndex)
→ beginIndex to end


substring(beginIndex, endIndex)
→ beginIndex inclusive
→ endIndex exclusive
```

---

## 33. What does `concat()` do?

It returns a new String formed by appending another String.

Example:

```java
String result =
    "Java".concat(
        "Developer"
    );
```

Result:

```text
JavaDeveloper
```

---

## 34. `concat()` vs `+`?

Both can concatenate Strings.

The `+` operator is more flexible because it can concatenate values of different types.

Example:

```java
String result =
    "Age: " + 20;
```

For repeated concatenation, the compiler/JVM may use StringBuilder-like mechanisms depending on the expression and Java version.

---

## 35. Can `concat()` directly accept an integer?

No.

`concat()` expects a String.

This works:

```java
"Age: ".concat(
    String.valueOf(20)
);
```

But this does not:

```java
"Age: ".concat(20);
```

---

## 36. What does `String.valueOf()` do?

It converts values into their String representation.

Example:

```java
String s =
    String.valueOf(20);
```

For objects, the result generally comes from their String representation, with special handling for `null`.

---

## 37. Can `String.valueOf()` work with float?

Yes.

Example:

```java
float value =
    10.5f;

String s =
    String.valueOf(value);
```

---

## 38. What does `toString()` do?

It returns a String representation of an object.

The implementation can be overridden by a class to provide meaningful output.

---

## 39. What does `trim()` do?

`trim()` removes leading and trailing characters that are less than or equal to U+0020.

It is different from the Unicode-aware whitespace behavior provided by methods such as `strip()`.

---

## 40. What does `strip()` do?

`strip()` removes leading and trailing Unicode whitespace according to the relevant Unicode definition used by Java.

It was introduced in Java 11.

---

## 41. `trim()` vs `strip()`?

```text
trim()
→ older behavior based on characters <= U+0020


strip()
→ Unicode-aware whitespace handling
```

---

## 42. What does `isEmpty()` do?

It checks whether the String has zero characters.

Example:

```java
"".isEmpty();
```

returns:

```text
true
```

---

## 43. What does `isBlank()` do?

It checks whether the String is empty or contains only Unicode whitespace characters.

It was introduced in Java 11.

---

## 44. `isEmpty()` vs `isBlank()`?

```text
isEmpty()
→ length is 0


isBlank()
→ empty or only whitespace
```

---

## 45. What does `contains()` do?

It checks whether the String contains a specified character sequence.

Example:

```java
"Java".contains(
    "av"
);
```

returns:

```text
true
```

---

## 46. What does `startsWith()` do?

It checks whether the String begins with a specified prefix.

---

## 47. What does `endsWith()` do?

It checks whether the String ends with a specified suffix.

---

## 48. What does `indexOf()` return?

It returns the index of the first occurrence of the specified character or substring.

If not found:

```text
-1
```

---

## 49. What does `lastIndexOf()` do?

It returns the index of the last occurrence of a character or substring.

If not found:

```text
-1
```

---

## 50. What is the difference between `replace()` and `replaceAll()`?

```text
replace()
→ literal character/sequence replacement


replaceAll()
→ replacement using a regular expression
```

Example:

```java
String result =
    "a1b2".replaceAll(
        "\\d",
        ""
    );
```

Result:

```text
ab
```

---

## 51. What is `replaceFirst()`?

It replaces the first substring matching a regular expression.

---

## 52. What is `split()`?

`split()` divides a String according to a regular expression and returns an array of Strings.

Example:

```java
String[] parts =
    "Java,Spring".split(
        ","
    );
```

---

## 53. Is the argument of `split()` a regular expression?

Yes.

Therefore, special regex characters may need escaping.

---

## 54. What does `join()` do?

`String.join()` combines multiple Strings using a delimiter.

Example:

```java
String result =
    String.join(
        "-",
        "Java",
        "Spring",
        "SQL"
    );
```

Result:

```text
Java-Spring-SQL
```

---

# 6. StringBuilder and StringBuffer

## 55. What is StringBuilder?

`StringBuilder` is a mutable sequence of characters designed for efficient modification of strings.

---

## 56. Why use StringBuilder?

Repeated String concatenation can create many intermediate String objects because String is immutable.

StringBuilder allows modifications to the same mutable object.

---

## 57. What is StringBuffer?

`StringBuffer` is a mutable character sequence whose methods are synchronized.

It is generally used less often than StringBuilder in modern single-threaded code.

---

## 58. StringBuilder vs StringBuffer?

```text
StringBuilder
→ mutable
→ generally unsynchronized
→ usually preferred for single-threaded use


StringBuffer
→ mutable
→ synchronized methods
→ legacy thread-safe option
```

---

## 59. Is StringBuilder thread-safe?

No.

StringBuilder is not synchronized.

If multiple threads mutate the same instance concurrently, external synchronization or another concurrency strategy is required.

---

## 60. Is StringBuffer thread-safe?

Its methods are synchronized, providing synchronization for individual operations.

However, compound sequences of operations may still require external synchronization to maintain higher-level invariants.

---

## 61. What is the default capacity of StringBuilder?

The default initial capacity is:

```text
16 characters
```

---

## 62. What happens when StringBuilder exceeds its capacity?

It expands its internal storage.

The exact growth strategy is implementation-specific, but commonly the capacity grows approximately as:

```text
oldCapacity * 2 + 2
```

Do not rely on the exact formula as a universal API guarantee.

---

## 63. What is `append()`?

It adds a value to the end of the mutable sequence.

Example:

```java
StringBuilder sb =
    new StringBuilder();

sb.append("Java");
```

---

## 64. What is `insert()`?

It inserts a value at a specified position.

---

## 65. What is `delete()`?

It removes characters from a specified range.

---

## 66. What is `reverse()`?

It reverses the sequence of characters in the StringBuilder or StringBuffer.

---

## 67. How do you convert StringBuilder to String?

Use:

```java
String result =
    sb.toString();
```

---

# 7. String Internals

## 68. Why does String implement `CharSequence`?

`CharSequence` defines a general read-only sequence-of-characters abstraction.

String implements it so String can be used wherever a CharSequence is accepted.

---

## 69. Does String implement Serializable?

Yes.

String implements several interfaces, including:

```text
Serializable
Comparable<String>
CharSequence
Constable
ConstantDesc
```

---

## 70. What is String's `hashCode()` behavior?

The hash code is calculated from the characters of the String.

Because String is immutable, the hash code remains stable for the object's lifetime.

---

## 71. Why is String a good HashMap key?

Because:

```text
String is immutable
+
equals() and hashCode() are content-based
```

Therefore, its key identity remains stable after insertion.

---

## 72. What is lexical ordering of Strings?

Lexicographical ordering compares Strings based on their character sequence.

Methods such as:

```text
compareTo()
compareToIgnoreCase()
```

can be used for ordering.

---

## 73. Does String use UTF-8 internally?

Not exactly.

Java's `String` represents text using UTF-16 code units conceptually through its `char`-based API.

Modern JDK implementations may use compact internal representations for memory efficiency, such as byte arrays with an encoding indicator.

---

## 74. What is the difference between `char` and Unicode code point?

A Java `char` represents a UTF-16 code unit.

A Unicode code point may require one or two UTF-16 code units.

Therefore, one Unicode character as perceived by a user is not always represented by one Java `char`.

---

## 75. What are `codePointAt()` and `codePoints()`?

They allow working with Unicode code points rather than only UTF-16 code units.

This is important for characters represented using surrogate pairs.

---

# 8. String Tricky Questions

## 76. What is the output?

```java
String a =
    "Java";

String b =
    "Java";

System.out.println(
    a == b
);
```

Answer:

```text
true
```

Both literals can refer to the same pooled String.

---

## 77. What is the output?

```java
String a =
    new String("Java");

String b =
    new String("Java");

System.out.println(
    a == b
);
```

Answer:

```text
false
```

They are distinct objects created by separate `new` expressions.

---

## 78. What is the output?

```java
String a =
    new String("Java");

String b =
    "Java";

System.out.println(
    a == b
);
```

Answer:

```text
false
```

`a` refers to the newly created object while `b` refers to the pooled literal.

---

## 79. What is the output?

```java
String a =
    new String("Java");

String b =
    "Java";

System.out.println(
    a.equals(b)
);
```

Answer:

```text
true
```

`equals()` compares String content.

---

## 80. What is the output?

```java
String s =
    "Java";

s.concat("Developer");

System.out.println(s);
```

Answer:

```text
Java
```

The returned String was ignored.

---

## 81. What is the output?

```java
String s =
    "Java";

s =
    s.concat("Developer");

System.out.println(s);
```

Answer:

```text
JavaDeveloper
```

The variable now refers to the new String.

---

## 82. What is the output?

```java
String a =
    "Ja" + "va";

String b =
    "Java";

System.out.println(
    a == b
);
```

Answer:

```text
true
```

The concatenation of compile-time String constants can be resolved to the same pooled literal.

---

## 83. What is the output?

```java
String x =
    "Ja";

String a =
    x + "va";

String b =
    "Java";

System.out.println(
    a == b
);
```

Answer:

```text
false
```

The concatenation involving a runtime variable produces a String through runtime concatenation rather than being the same compile-time constant expression.

---

## 84. What is the output?

```java
String a =
    new String("Java");

String b =
    a.intern();

String c =
    "Java";

System.out.println(
    b == c
);
```

Answer:

```text
true
```

`intern()` returns the canonical pooled representation.

---

## 85. Is String mutable through reflection?

Modern Java strongly encapsulates implementation details, and relying on reflective mutation of String internals is not a valid normal programming technique.

String should be treated as immutable.

---

# 9. String Coding Questions

## 86. How do you reverse a String?

A common approach is StringBuilder.

```java
String s =
    "Java";

String reversed =
    new StringBuilder(s)
        .reverse()
        .toString();
```

---

## 87. How do you check whether a String is a palindrome?

Compare the String with its reverse.

```java
String s =
    "madam";

String reversed =
    new StringBuilder(s)
        .reverse()
        .toString();

boolean palindrome =
    s.equals(reversed);
```

---

## 88. How do you count characters in a String?

A common approach is to use a frequency array or map.

For lowercase English letters:

```java
String s =
    "banana";

int[] freq =
    new int[26];

for (char c : s.toCharArray()) {

    freq[c - 'a']++;
}
```

---

## 89. How do you remove spaces from a String?

For ordinary space characters:

```java
String result =
    s.replace(" ", "");
```

For broader whitespace handling, the appropriate regex or Unicode-aware approach depends on the requirement.

---

## 90. How do you check whether two Strings are anagrams?

One approach is:

```text
Convert to character arrays
↓
Sort
↓
Compare
```

Another approach is frequency counting.

---

## 91. How do you find duplicate characters?

Use a frequency structure.

Example concept:

```text
character
   ↓
frequency map
   ↓
frequency > 1
   ↓
duplicate
```

---

## 92. How do you find the first non-repeating character?

Use two passes:

```text
Pass 1
→ count frequency


Pass 2
→ find first character with frequency 1
```

---

## 93. How do you check whether a String contains only digits?

One approach is to examine every character and verify that it is a digit.

Java also provides regular-expression-based approaches, but for simple validation a character-by-character scan can be clearer and more efficient.

---

## 94. How do you convert String to char array?

Use:

```java
char[] chars =
    s.toCharArray();
```

---

## 95. How do you convert char array to String?

Use:

```java
String s =
    new String(chars);
```

---

# 10. Rapid String Revision

## 96. Is String primitive?

No.

---

## 97. Is String final?

Yes.

---

## 98. Is String immutable?

Yes.

---

## 99. Is String thread-safe?

String is immutable, so its state cannot be changed concurrently.

Therefore, sharing a String between threads does not create the usual mutable-state race condition.

---

## 100. `==` or `equals()` for String content?

```text
equals()
```

---

## 101. What does `==` compare for objects?

Reference identity.

---

## 102. What does `equals()` compare for String?

String content.

---

## 103. Where are String literals stored?

They are maintained in the JVM's String pool, which is associated with the heap in modern JVMs.

---

## 104. What does `intern()` do?

Returns the canonical pooled representation of a String.

---

## 105. String vs StringBuilder?

```text
String
→ immutable


StringBuilder
→ mutable
```

---

## 106. StringBuilder vs StringBuffer?

```text
StringBuilder
→ generally unsynchronized


StringBuffer
→ synchronized
```

---

## 107. Default StringBuilder capacity?

```text
16
```

---

## 108. Does `concat()` modify String?

No.

It returns a new String.

---

## 109. Does `replace()` use regex?

No.

---

## 110. Does `replaceAll()` use regex?

Yes.

---

## 111. Does `split()` use regex?

Yes.

---

## 112. `length` or `length()` for String?

```text
length()
```

---

## 113. `length` or `length()` for array?

```text
length
```

---

## 114. `trim()` vs `strip()`?

```text
trim()
→ older ASCII-oriented whitespace rule


strip()
→ Unicode-aware whitespace
```

---

## 115. `isEmpty()` vs `isBlank()`?

```text
isEmpty()
→ zero length


isBlank()
→ empty or whitespace-only
```

---

## 116. Can String be used as a HashMap key?

Yes.

Its immutability and content-based `equals()`/`hashCode()` make it suitable.

---

## 117. Why is String immutable?

Key reasons:

```text
Security
String Pool
Hashing
Thread-safety benefits
Predictability
```

---

## 118. Can String be subclassed?

No.

String is final.

---

## 119. Does String store Unicode?

Yes, Java Strings represent Unicode text using UTF-16 code units conceptually.

---

## 120. Is one Java `char` always one Unicode character?

No.

Some Unicode code points require two UTF-16 code units.

---

# 🧠 String Interview Memory Map

```text
                       STRING
                          |
          +---------------+---------------+
          |               |               |
      Immutable        String Pool      Final Class
          |               |               |
       New String       intern()       No subclass
          |
    New object returned
          |
          +-------------------------------+
          |
       Comparison
       /         \
      ==        equals()
      |            |
   identity      content
```

---

# ⚡ Most Important String Comparisons

```text
String
vs
StringBuilder


StringBuilder
vs
StringBuffer


==
vs
equals()


equals()
vs
compareTo()


replace()
vs
replaceAll()


trim()
vs
strip()


isEmpty()
vs
isBlank()


String literal
vs
new String()


char
vs
Unicode code point
```

---

