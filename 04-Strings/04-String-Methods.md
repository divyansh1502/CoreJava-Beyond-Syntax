# 🛠️ String Methods in Java

> **The `String` class provides built-in methods for searching, comparing, extracting, transforming, validating, and converting String data. Since String is immutable, methods that produce changed text return a new String instead of modifying the original object.**

---

# 📌 Table of Contents

1. [Why String Methods?](#1--why-string-methods)
2. [String Method Categories](#2--string-method-categories)
3. [length()](#3--length)
4. [charAt()](#4--charat)
5. [substring()](#5--substring)
6. [concat()](#6--concat)
7. [equals()](#7--equals)
8. [equalsIgnoreCase()](#8--equalsignorecase)
9. [compareTo()](#9--compareto)
10. [compareToIgnoreCase()](#10--comparetoignorecase)
11. [contains()](#11--contains)
12. [startsWith()](#12--startswith)
13. [endsWith()](#13--endswith)
14. [indexOf()](#14--indexof)
15. [lastIndexOf()](#15--lastindexof)
16. [isEmpty()](#16--isempty)
17. [isBlank()](#17--isblank)
18. [toUpperCase()](#18--touppercase)
19. [toLowerCase()](#19--tolowercase)
20. [trim()](#20--trim)
21. [strip()](#21--strip)
22. [stripLeading()](#22--stripleading)
23. [stripTrailing()](#23--striptrailing)
24. [replace()](#24--replace)
25. [replaceFirst()](#25--replacefirst)
26. [replaceAll()](#26--replaceall)
27. [split()](#27--split)
28. [join()](#28--join)
29. [valueOf()](#29--valueof)
30. [toCharArray()](#30--tochararray)
31. [getBytes()](#31--getbytes)
32. [matches()](#32--matches)
33. [format()](#33--format)
34. [repeat()](#34--repeat)
35. [intern()](#35--intern)
36. [String Comparison](#36--string-comparison)
37. [Methods That Return String](#37--methods-that-return-string)
38. [Methods That Return boolean](#38--methods-that-return-boolean)
39. [Methods That Return int](#39--methods-that-return-int)
40. [Methods That Return char](#40--methods-that-return-char)
41. [Methods That Return Arrays](#41--methods-that-return-arrays)
42. [DSA Connection](#42--dsa-connection)
43. [Important String DSA Patterns](#43--important-string-dsa-patterns)
44. [Common String DSA Questions](#44--common-string-dsa-questions)
45. [DSA Problem-Solving Approach](#45--dsa-problem-solving-approach)
46. [Common Mistakes](#46--common-mistakes)
47. [Interview Traps](#47--interview-traps)
48. [Top 20 Interview Questions](#48--top-20-interview-questions)
49. [30-Second Interview Answer](#49--30-second-interview-answer)
50. [Cheat Sheet](#50--cheat-sheet)
51. [Memory Tricks](#51--memory-tricks)
52. [Next Topic](#52--next-topic)

---

# 1. 🔤 Why String Methods?

Strings are used everywhere in Java:

- User input
- Names
- Passwords
- URLs
- File paths
- JSON
- Database data
- API responses
- Logs
- Text processing

Java provides built-in methods so we do not have to manually implement common String operations.

Example:

```java
String name = "Divyansh";

System.out.println(name.length());
System.out.println(name.indexOf('y'));
System.out.println(name.toUpperCase());
```

---

# 2. 🧩 String Method Categories

| Category | Important Methods |
|---|---|
| Information | `length()`, `isEmpty()`, `isBlank()` |
| Character Access | `charAt()` |
| Extraction | `substring()` |
| Comparison | `equals()`, `equalsIgnoreCase()`, `compareTo()` |
| Searching | `contains()`, `indexOf()`, `lastIndexOf()` |
| Prefix/Suffix | `startsWith()`, `endsWith()` |
| Case Conversion | `toUpperCase()`, `toLowerCase()` |
| Whitespace | `trim()`, `strip()`, `stripLeading()`, `stripTrailing()` |
| Replacement | `replace()`, `replaceFirst()`, `replaceAll()` |
| Splitting | `split()` |
| Joining | `join()` |
| Conversion | `valueOf()`, `toCharArray()`, `getBytes()` |
| Regex | `matches()`, `replaceAll()`, `replaceFirst()` |
| Formatting | `format()` |
| Repetition | `repeat()` |
| Pool | `intern()` |

---

# 3. 📏 length()

## Definition

`length()` returns the number of UTF-16 code units in a String.

## Syntax

```java
string.length();
```

## Example

```java
String s = "Java";

System.out.println(s.length());
```

Output:

```text
4
```

### Important

For String:

```java
String s = "Java";

int length = s.length();
```

For an array:

```java
int[] arr = {10, 20, 30};

int length = arr.length;
```

Remember:

```text
String → length()
Array  → length
```

### DSA Importance

`length()` is one of the most frequently used String operations in DSA.

Typical usage:

```java
for (int i = 0; i < s.length(); i++) {
    System.out.println(s.charAt(i));
}
```

Time complexity:

```text
O(1)
```

---

# 4. 🔤 charAt()

`charAt(index)` returns the UTF-16 `char` at the specified index.

## Example

```java
String s = "Java";

System.out.println(s.charAt(0));
System.out.println(s.charAt(1));
System.out.println(s.charAt(2));
System.out.println(s.charAt(3));
```

Output:

```text
J
a
v
a
```

Index visualization:

```text
String:  J  a  v  a
Index:   0  1  2  3
```

Invalid index:

```java
String s = "Java";

System.out.println(s.charAt(4));
```

This throws:

```text
StringIndexOutOfBoundsException
```

### DSA Importance

`charAt()` is essential for:

- Frequency counting
- Palindrome checking
- Two-pointer problems
- Character comparisons
- Sliding-window problems

Example:

```java
String s = "hello";

for (int i = 0; i < s.length(); i++) {
    char ch = s.charAt(i);
    System.out.println(ch);
}
```

Time complexity:

```text
O(1) per charAt()
```

---

# 5. ✂️ substring()

`substring()` extracts a portion of a String.

There are two commonly used forms:

```java
substring(beginIndex)
```

and:

```java
substring(beginIndex, endIndex)
```

## substring(beginIndex)

```java
String s = "Java Programming";

System.out.println(s.substring(5));
```

Output:

```text
Programming
```

## substring(beginIndex, endIndex)

The start index is inclusive.

The end index is exclusive.

```java
String s = "Java";

System.out.println(s.substring(1, 3));
```

Output:

```text
av
```

Remember:

```text
[start, end)
```

### DSA Importance

`substring()` appears in:

- String partitioning
- Prefix/suffix problems
- Substring enumeration
- Sliding-window problems
- Brute-force String problems

Example:

```java
String s = "abc";

for (int i = 0; i < s.length(); i++) {
    for (int j = i + 1; j <= s.length(); j++) {
        System.out.println(s.substring(i, j));
    }
}
```

### Complexity Note

Creating a substring generally requires time proportional to the length of the resulting String.

If a substring of length `k` is created:

```text
Time → O(k)
Space → O(k)
```

---

# 6. ➕ concat()

`concat()` joins another String.

```java
String s1 = "Java";
String s2 = "Programming";

String result = s1.concat(s2);

System.out.println(result);
```

Output:

```text
JavaProgramming
```

Because String is immutable:

```java
String s = "Java";

s.concat(" World");

System.out.println(s);
```

Output:

```text
Java
```

Correct:

```java
String s = "Java";

s = s.concat(" World");

System.out.println(s);
```

Output:

```text
Java World
```

---

# 7. 🟰 equals()

`equals()` compares String contents.

```java
String a = "Java";
String b = "Java";

System.out.println(a.equals(b));
```

Output:

```text
true
```

Different contents:

```java
String a = "Java";
String b = "Python";

System.out.println(a.equals(b));
```

Output:

```text
false
```

### DSA Importance

`equals()` is frequently used when:

- Comparing words
- Checking patterns
- Comparing generated substrings
- Checking dictionary/map keys
- Validating String states

---

# 8. 🔤 equalsIgnoreCase()

Compares String contents while ignoring case.

```java
String a = "Java";
String b = "JAVA";

System.out.println(a.equalsIgnoreCase(b));
```

Output:

```text
true
```

---

# 9. 📊 compareTo()

`compareTo()` performs lexicographical comparison.

It returns:

```text
negative → first String comes before second
zero     → Strings are equal
positive → first String comes after second
```

Example:

```java
String a = "Apple";
String b = "Banana";

System.out.println(a.compareTo(b));
```

The result is negative.

Do not depend on the exact positive or negative number.

Focus on:

```text
< 0
== 0
> 0
```

### DSA Importance

`compareTo()` is useful in:

- Sorting Strings
- Custom ordering
- Tree-based collections
- Lexicographical problems

Example:

```java
String a = "Apple";
String b = "Banana";

if (a.compareTo(b) < 0) {
    System.out.println(a + " comes first");
}
```

---

# 10. 🔤 compareToIgnoreCase()

Works like `compareTo()` but ignores case.

```java
String a = "java";
String b = "JAVA";

System.out.println(a.compareToIgnoreCase(b));
```

Output:

```text
0
```

---

# 11. 🔎 contains()

Checks whether a String contains a specified sequence.

```java
String s = "Java Programming";

System.out.println(s.contains("Java"));
System.out.println(s.contains("Python"));
```

Output:

```text
true
false
```

It is case-sensitive:

```java
String s = "Java";

System.out.println(s.contains("java"));
```

Output:

```text
false
```

---

# 12. 🚀 startsWith()

Checks whether a String starts with a specified prefix.

```java
String s = "Java Programming";

System.out.println(s.startsWith("Java"));
System.out.println(s.startsWith("Python"));
```

Output:

```text
true
false
```

Overloaded version:

```java
String s = "HelloJava";

System.out.println(s.startsWith("Java", 5));
```

Output:

```text
true
```

---

# 13. 🏁 endsWith()

Checks whether a String ends with a specified suffix.

```java
String s = "Hello.java";

System.out.println(s.endsWith(".java"));
```

Output:

```text
true
```

Common DSA/application use:

```java
String filename = "Main.java";

if (filename.endsWith(".java")) {
    System.out.println("Java file");
}
```

---

# 14. 🔍 indexOf()

`indexOf()` returns the index of the first occurrence.

```java
String s = "Java Programming";

System.out.println(s.indexOf('a'));
```

Output:

```text
1
```

Searching for a String:

```java
String s = "Java Programming";

System.out.println(s.indexOf("Programming"));
```

Output:

```text
5
```

Not found:

```java
String s = "Java";

System.out.println(s.indexOf("Python"));
```

Output:

```text
-1
```

### DSA Importance

`indexOf()` is useful for:

- Searching
- Finding delimiters
- Parsing
- Prefix/suffix logic
- Brute-force String problems

---

# 15. 🔎 lastIndexOf()

Returns the last occurrence.

```java
String s = "Java";

System.out.println(s.lastIndexOf('a'));
```

Output:

```text
3
```

Not found:

```java
String s = "Java";

System.out.println(s.lastIndexOf('z'));
```

Output:

```text
-1
```

---

# 16. 🈳 isEmpty()

Checks whether String length is zero.

```java
String s = "";

System.out.println(s.isEmpty());
```

Output:

```text
true
```

Whitespace is not empty:

```java
String s = "   ";

System.out.println(s.isEmpty());
```

Output:

```text
false
```

---

# 17. 🧹 isBlank()

`isBlank()` checks whether a String is empty or contains only whitespace.

Available since Java 11.

```java
String s = "   ";

System.out.println(s.isBlank());
```

Output:

```text
true
```

Comparison:

| String | `isEmpty()` | `isBlank()` |
|---|---:|---:|
| `""` | true | true |
| `"   "` | false | true |
| `"Java"` | false | false |

---

# 18. 🔠 toUpperCase()

Converts characters to uppercase.

```java
String s = "java";

String result = s.toUpperCase();

System.out.println(result);
```

Output:

```text
JAVA
```

Original remains unchanged:

```java
String s = "java";

s.toUpperCase();

System.out.println(s);
```

Output:

```text
java
```

---

# 19. 🔡 toLowerCase()

Converts characters to lowercase.

```java
String s = "JAVA";

String result = s.toLowerCase();

System.out.println(result);
```

Output:

```text
java
```

---

# 20. 🧹 trim()

`trim()` removes leading and trailing characters whose code points are less than or equal to U+0020.

```java
String s = "   Java   ";

System.out.println(s.trim());
```

Output:

```text
Java
```

It does not remove whitespace in the middle.

---

# 21. 🧹 strip()

`strip()` removes leading and trailing Unicode whitespace.

Available since Java 11.

```java
String s = "   Java   ";

System.out.println(s.strip());
```

Output:

```text
Java
```

Comparison:

| Method | Introduced | Behavior |
|---|---:|---|
| `trim()` | Java 1.0 | Traditional whitespace rules |
| `strip()` | Java 11 | Unicode-aware whitespace |

---

# 22. ⬅️ stripLeading()

Removes leading whitespace.

```java
String s = "   Java   ";

System.out.println(s.stripLeading());
```

Conceptual result:

```text
Java   
```

---

# 23. ➡️ stripTrailing()

Removes trailing whitespace.

```java
String s = "   Java   ";

System.out.println(s.stripTrailing());
```

Conceptual result:

```text
   Java
```

---

# 24. 🔄 replace()

`replace()` performs literal replacement.

Character replacement:

```java
String s = "Java";

String result = s.replace('a', 'o');

System.out.println(result);
```

Output:

```text
Jovo
```

String replacement:

```java
String s = "Java Java";

String result = s.replace("Java", "Python");

System.out.println(result);
```

Output:

```text
Python Python
```

Important:

```text
replace() → literal replacement
```

It does not interpret the target as a regular expression.

---

# 25. 1️⃣ replaceFirst()

`replaceFirst()` replaces the first substring matching a regular expression.

```java
String s = "Java Java";

String result = s.replaceFirst("Java", "Python");

System.out.println(result);
```

Output:

```text
Python Java
```

---

# 26. 🔁 replaceAll()

`replaceAll()` replaces every substring matching a regular expression.

```java
String s = "Java123";

String result = s.replaceAll("[0-9]", "");

System.out.println(result);
```

Output:

```text
Java
```

Whitespace normalization example:

```java
String s = "Java    Programming";

String result = s.replaceAll("\\s+", " ");

System.out.println(result);
```

Output:

```text
Java Programming
```

Remember:

```text
replace()      → literal
replaceFirst() → regex + first match
replaceAll()   → regex + all matches
```

---

# 27. ✂️ split()

`split()` divides a String into a `String[]` using a regular expression.

```java
String s = "Java,Python,C++";

String[] languages = s.split(",");

for (String language : languages) {
    System.out.println(language);
}
```

Output:

```text
Java
Python
C++
```

### Important

The argument is a regex.

For a literal dot:

```java
String s = "a.b.c";

String[] parts = s.split("\\.");
```

### DSA Importance

`split()` is useful for:

- Tokenization
- Parsing input
- Word-based problems
- Sentence processing
- Delimiter-based problems

But remember that repeated `split()` can create many objects, so manual scanning may be preferable when performance matters.

---

# 28. 🔗 join()

`join()` combines Strings using a delimiter.

```java
String result = String.join("-", "2026", "09", "21");

System.out.println(result);
```

Output:

```text
2026-09-21
```

Another example:

```java
String result = String.join(", ", "Java", "Python", "C++");

System.out.println(result);
```

Output:

```text
Java, Python, C++
```

It is a static method:

```java
String.join(...)
```

---

# 29. 🔄 valueOf()

`String.valueOf()` converts values into their String representation.

```java
int num = 100;

String s = String.valueOf(num);

System.out.println(s);
```

Output:

```text
100
```

Examples:

```java
String a = String.valueOf(100);
String b = String.valueOf(10.5);
String c = String.valueOf(true);
String d = String.valueOf('A');
```

---

# 30. 🔡 toCharArray()

Converts a String into a character array.

```java
String s = "Java";

char[] arr = s.toCharArray();

for (char ch : arr) {
    System.out.println(ch);
}
```

Conceptually:

```text
"Java"
   ↓
['J', 'a', 'v', 'a']
```

### ⭐ DSA Importance

This is extremely useful when:

- Sorting characters
- Modifying characters
- Frequency counting
- Using array-based algorithms

Example:

```java
String s = "hello";

char[] chars = s.toCharArray();

chars[0] = 'H';

System.out.println(new String(chars));
```

Output:

```text
Hello
```

---

# 31. 🧱 getBytes()

Converts a String into bytes using a charset.

Prefer an explicit charset:

```java
import java.nio.charset.StandardCharsets;

String s = "Java";

byte[] bytes = s.getBytes(StandardCharsets.UTF_8);
```

For text processing involving encoding, always be conscious of the charset being used.

---

# 32. 🧪 matches()

`matches()` checks whether the entire String matches a regular expression.

```java
String s = "12345";

System.out.println(s.matches("\\d+"));
```

Output:

```text
true
```

Another example:

```java
String s = "Java";

System.out.println(s.matches("[A-Za-z]+"));
```

Output:

```text
true
```

Important:

```text
matches() → entire String must match
```

---

# 33. 📝 format()

`String.format()` creates a formatted String.

```java
String name = "Divyansh";
int age = 22;

String result = String.format(
    "Name: %s, Age: %d",
    name,
    age
);

System.out.println(result);
```

Output:

```text
Name: Divyansh, Age: 22
```

Common format specifiers:

| Specifier | Meaning |
|---|---|
| `%s` | String |
| `%d` | Integer |
| `%f` | Floating-point |
| `%c` | Character |
| `%b` | Boolean |
| `%n` | Platform-specific line separator |

---

# 34. 🔁 repeat()

`repeat()` repeats a String.

Available since Java 11.

```java
String s = "Hi";

System.out.println(s.repeat(3));
```

Output:

```text
HiHiHi
```

Another example:

```java
System.out.println("*".repeat(5));
```

Output:

```text
*****
```

The count cannot be negative.

---

# 35. 🏊 intern()

`intern()` returns the canonical representation of a String.

```java
String s1 = new String("Java");

String s2 = s1.intern();

System.out.println(s2 == "Java");
```

Output:

```text
true
```

`intern()` is related to the String Pool and should not be used casually.

---

# 36. 🆚 String Comparison

## `==`

Compares references.

```java
String a = new String("Java");
String b = new String("Java");

System.out.println(a == b);
```

Output:

```text
false
```

## `equals()`

Compares contents.

```java
System.out.println(a.equals(b));
```

Output:

```text
true
```

## `compareTo()`

Performs lexicographical comparison.

```java
String a = "Apple";
String b = "Banana";

System.out.println(a.compareTo(b) < 0);
```

Output:

```text
true
```

### Golden Rule

```text
==       → reference comparison
equals() → content comparison
compareTo() → ordering comparison
```

---

# 37. 🧾 Methods That Return String

| Method | Return Type |
|---|---|
| `concat()` | `String` |
| `substring()` | `String` |
| `toUpperCase()` | `String` |
| `toLowerCase()` | `String` |
| `trim()` | `String` |
| `strip()` | `String` |
| `stripLeading()` | `String` |
| `stripTrailing()` | `String` |
| `replace()` | `String` |
| `replaceFirst()` | `String` |
| `replaceAll()` | `String` |
| `repeat()` | `String` |
| `format()` | `String` |

Because String is immutable, these operations return a String rather than modifying the existing object.

---

# 38. ✅ Methods That Return boolean

| Method | Purpose |
|---|---|
| `equals()` | Content comparison |
| `equalsIgnoreCase()` | Case-insensitive comparison |
| `contains()` | Contains sequence |
| `startsWith()` | Prefix check |
| `endsWith()` | Suffix check |
| `isEmpty()` | Zero length |
| `isBlank()` | Empty/whitespace-only |
| `matches()` | Regex matching |

---

# 39. 🔢 Methods That Return int

| Method | Purpose |
|---|---|
| `length()` | Number of UTF-16 code units |
| `indexOf()` | First occurrence |
| `lastIndexOf()` | Last occurrence |
| `compareTo()` | Lexicographical comparison |
| `compareToIgnoreCase()` | Case-insensitive comparison |

---

# 40. 🔤 Methods That Return char

Main method:

```java
String s = "Java";

char ch = s.charAt(2);

System.out.println(ch);
```

Output:

```text
v
```

Return type:

```text
char
```

---

# 41. 📦 Methods That Return Arrays

## toCharArray()

Returns:

```text
char[]
```

Example:

```java
String s = "Java";

char[] arr = s.toCharArray();
```

## split()

Returns:

```text
String[]
```

Example:

```java
String s = "Java Python C++";

String[] arr = s.split(" ");
```

## getBytes()

Returns:

```text
byte[]
```

Example:

```java
byte[] arr = "Java".getBytes();
```

---

# 42. 🧠 DSA Connection

String methods are directly connected to many DSA problems.

A String can be treated conceptually as a sequence of characters:

```text
String
  ↓
Characters
  ↓
Index-based processing
  ↓
DSA algorithms
```

Important String methods for DSA:

| Method | DSA Use |
|---|---|
| `length()` | Traversal bounds |
| `charAt()` | Character access |
| `substring()` | Substring problems |
| `indexOf()` | Searching |
| `lastIndexOf()` | Reverse searching |
| `equals()` | Comparing sequences |
| `toCharArray()` | Array-based processing |
| `split()` | Tokenization |
| `contains()` | Basic searching |
| `startsWith()` | Prefix problems |
| `endsWith()` | Suffix problems |
| `compareTo()` | Lexicographical ordering |

### ⭐ Most Important DSA Idea

Do not memorize String methods separately from algorithms.

Learn to combine them.

For example:

```text
String
  +
charAt()
  +
HashMap
  +
frequency counting
```

This combination solves many problems.

---

# 43. 🔥 Important String DSA Patterns

## Pattern 1 — Character Frequency

Question:

> Count the frequency of each character.

Using an array:

```java
String s = "banana";

int[] freq = new int[26];

for (int i = 0; i < s.length(); i++) {
    char ch = s.charAt(i);

    freq[ch - 'a']++;
}
```

For lowercase English letters:

```text
Time  → O(n)
Space → O(1)
```

The space is O(1) because the array always has 26 positions.

---

## Pattern 2 — Frequency Map

Useful when characters are not limited to lowercase English letters.

```java
import java.util.HashMap;
import java.util.Map;

String s = "banana";

Map<Character, Integer> freq = new HashMap<>();

for (char ch : s.toCharArray()) {
    freq.put(ch, freq.getOrDefault(ch, 0) + 1);
}
```

Complexity:

```text
Average Time → O(n)
Space        → O(k)
```

Where `k` is the number of distinct characters.

---

# 44. 🧩 Common String DSA Questions

## 1. Reverse a String

### Approach

Use two pointers or a character array.

```java
String s = "hello";

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

String reversed = new String(arr);

System.out.println(reversed);
```

Output:

```text
olleh
```

Complexity:

```text
Time  → O(n)
Space → O(n)
```

---

## 2. Check Palindrome

A palindrome reads the same forward and backward.

Example:

```text
madam → palindrome
hello → not palindrome
```

Solution:

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

Complexity:

```text
Time  → O(n)
Space → O(1)
```

### Pattern

```text
Two Pointers
     ↓
left →      ← right
```

---

## 3. Count Vowels

```java
String s = "education";

int count = 0;

for (int i = 0; i < s.length(); i++) {
    char ch = s.charAt(i);

    if (ch == 'a' ||
        ch == 'e' ||
        ch == 'i' ||
        ch == 'o' ||
        ch == 'u') {

        count++;
    }
}

System.out.println(count);
```

Complexity:

```text
Time  → O(n)
Space → O(1)
```

---

## 4. First Non-Repeating Character

Pattern:

```text
Frequency Count
       ↓
Second Traversal
       ↓
First frequency == 1
```

Example:

```java
import java.util.HashMap;
import java.util.Map;

String s = "swiss";

Map<Character, Integer> freq = new HashMap<>();

for (char ch : s.toCharArray()) {
    freq.put(ch, freq.getOrDefault(ch, 0) + 1);
}

for (char ch : s.toCharArray()) {
    if (freq.get(ch) == 1) {
        System.out.println(ch);
        break;
    }
}
```

Output:

```text
w
```

Complexity:

```text
Average Time → O(n)
Space        → O(k)
```

---

## 5. Check Anagram

Two Strings are anagrams if they contain the same characters with the same frequencies.

Example:

```text
listen
silent
```

Frequency-array approach:

```java
String s1 = "listen";
String s2 = "silent";

if (s1.length() != s2.length()) {
    System.out.println(false);
    return;
}

int[] freq = new int[26];

for (int i = 0; i < s1.length(); i++) {
    freq[s1.charAt(i) - 'a']++;
    freq[s2.charAt(i) - 'a']--;
}

boolean anagram = true;

for (int value : freq) {
    if (value != 0) {
        anagram = false;
        break;
    }
}

System.out.println(anagram);
```

Complexity:

```text
Time  → O(n)
Space → O(1)
```

---

## 6. Remove Duplicate Characters

```java
import java.util.HashSet;
import java.util.Set;

String s = "programming";

Set<Character> seen = new HashSet<>();

StringBuilder result = new StringBuilder();

for (char ch : s.toCharArray()) {
    if (seen.add(ch)) {
        result.append(ch);
    }
}

System.out.println(result);
```

Output:

```text
progamin
```

Pattern:

```text
String
  ↓
HashSet
  ↓
Track seen characters
```

---

# 45. 🧠 DSA Problem-Solving Approach

When you see a String problem, ask these questions in order.

## Step 1 — What is the input?

```text
String?
String[]?
Character array?
```

## Step 2 — What is required?

```text
Search?
Count?
Compare?
Reverse?
Remove?
Find substring?
```

## Step 3 — Can I solve it with simple traversal?

Think:

```java
for (int i = 0; i < s.length(); i++) {
    char ch = s.charAt(i);
}
```

## Step 4 — Do I need extra data?

Possible tools:

```text
int[26]
HashMap
HashSet
char[]
StringBuilder
Deque
```

## Step 5 — Can two pointers work?

Typical problems:

```text
Palindrome
Reverse
Compare from both ends
Remove characters
```

## Step 6 — Is it a sliding-window problem?

Look for phrases like:

```text
longest substring
shortest substring
at most K characters
without repeating characters
minimum window
```

## Step 7 — Analyze complexity

Always identify:

```text
Time Complexity
Space Complexity
```

---

# 46. ⚠️ Common Mistakes

## ❌ Mistake 1 — Using `==`

Wrong:

```java
String a = "Java";
String b = new String("Java");

if (a == b) {
    System.out.println("Equal");
}
```

Correct:

```java
if (a.equals(b)) {
    System.out.println("Equal");
}
```

---

## ❌ Mistake 2 — Forgetting immutability

Wrong assumption:

```java
String s = "Java";

s.toUpperCase();

System.out.println(s);
```

Output:

```text
Java
```

Correct:

```java
s = s.toUpperCase();
```

---

## ❌ Mistake 3 — Confusing `isEmpty()` and `isBlank()`

```java
System.out.println("   ".isEmpty());
System.out.println("   ".isBlank());
```

Output:

```text
false
true
```

---

## ❌ Mistake 4 — Forgetting substring end is exclusive

```java
String s = "Java";

System.out.println(s.substring(1, 3));
```

Output:

```text
av
```

Remember:

```text
[start, end)
```

---

## ❌ Mistake 5 — Forgetting `indexOf()` returns `-1`

```java
String s = "Java";

System.out.println(s.indexOf('z'));
```

Output:

```text
-1
```

---

## ❌ Mistake 6 — Confusing `replace()` and `replaceAll()`

```text
replace()      → literal
replaceFirst() → regex + first
replaceAll()   → regex + all
```

---

## ❌ Mistake 7 — Forgetting `split()` uses regex

To split:

```text
a.b.c
```

Use:

```java
String s = "a.b.c";

String[] parts = s.split("\\.");
```

---

# 47. 🚨 Interview Traps

## Trap 1

```java
String s = "Java";

s.toUpperCase();

System.out.println(s);
```

Output:

```text
Java
```

---

## Trap 2

```java
String s = "Java";

s = s.toUpperCase();

System.out.println(s);
```

Output:

```text
JAVA
```

---

## Trap 3

```java
String s = "Java";

System.out.println(s.substring(1, 3));
```

Output:

```text
av
```

---

## Trap 4

```java
String s = "Java";

System.out.println(s.indexOf('a'));
```

Output:

```text
1
```

---

## Trap 5

```java
String s = "Java";

System.out.println(s.lastIndexOf('a'));
```

Output:

```text
3
```

---

## Trap 6

```java
String s = "   ";

System.out.println(s.isEmpty());
```

Output:

```text
false
```

---

## Trap 7

```java
String s = "   ";

System.out.println(s.isBlank());
```

Output:

```text
true
```

---

## Trap 8

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

## Trap 9

```java
String s = "Java Java";

System.out.println(
    s.replaceAll("Java", "Python")
);
```

Output:

```text
Python Python
```

---

## Trap 10

```java
String s = "Java";

System.out.println(s.contains("java"));
```

Output:

```text
false
```

Reason:

`contains()` is case-sensitive.

---

# 48. 🔥 Top 20 Interview Questions

## Q1. What is the difference between `length` and `length()`?

**Answer:**

For arrays:

```java
int length = arr.length;
```

For String:

```java
int length = s.length();
```

---

## Q2. What does `charAt()` return?

**Answer:**

It returns a `char` at the specified index.

---

## Q3. What happens with an invalid `charAt()` index?

**Answer:**

`StringIndexOutOfBoundsException` is thrown.

---

## Q4. What is the difference between `substring(2)` and `substring(2, 5)`?

**Answer:**

`substring(2)` goes from index 2 to the end.

`substring(2, 5)` goes from index 2 up to, but not including, index 5.

---

## Q5. Is the end index of `substring()` inclusive?

**Answer:**

No.

```text
start → inclusive
end   → exclusive
```

---

## Q6. What does `indexOf()` return if the element is not found?

**Answer:**

```text
-1
```

---

## Q7. Difference between `indexOf()` and `lastIndexOf()`?

**Answer:**

```text
indexOf()     → first occurrence
lastIndexOf() → last occurrence
```

---

## Q8. Difference between `equals()` and `equalsIgnoreCase()`?

**Answer:**

`equals()` is case-sensitive.

`equalsIgnoreCase()` ignores case differences.

---

## Q9. What does `compareTo()` return?

**Answer:**

```text
negative → first comes before second
zero     → equal
positive → first comes after second
```

---

## Q10. Difference between `isEmpty()` and `isBlank()`?

**Answer:**

```text
isEmpty() → length == 0

isBlank() → empty or whitespace-only
```

---

## Q11. Difference between `trim()` and `strip()`?

**Answer:**

`trim()` follows traditional U+0020-based trimming rules.

`strip()` is Unicode-aware and was introduced in Java 11.

---

## Q12. What does `replace()` do?

**Answer:**

It performs literal replacement and returns a new String.

---

## Q13. Difference between `replace()`, `replaceFirst()`, and `replaceAll()`?

**Answer:**

```text
replace()      → literal replacement
replaceFirst() → first regex match
replaceAll()   → all regex matches
```

---

## Q14. Does `replace()` modify the original String?

**Answer:**

No.

String is immutable.

---

## Q15. What does `split()` return?

**Answer:**

```text
String[]
```

---

## Q16. What does `String.join()` do?

**Answer:**

It joins multiple Strings using a delimiter.

---

## Q17. What does `toCharArray()` return?

**Answer:**

```text
char[]
```

---

## Q18. What does `String.valueOf()` do?

**Answer:**

It converts values into their String representation.

---

## Q19. What does `matches()` do?

**Answer:**

It checks whether the entire String matches a regular expression.

---

## Q20. Why are String methods that transform text returning a new String?

**Answer:**

Because String is immutable. The existing String cannot be changed.

---

# 49. 🎤 30-Second Interview Answer

> **The String class provides built-in methods for comparison, searching, extraction, conversion, replacement, and validation. Important methods include `length()`, `charAt()`, `substring()`, `equals()`, `compareTo()`, `contains()`, `indexOf()`, `replace()`, `split()`, and `toUpperCase()`. Since String is immutable, methods that produce changed text return a new String rather than modifying the original String. These methods are also heavily used in DSA problems such as palindrome checking, frequency counting, anagrams, substring problems, and sliding-window problems.**

---

# 50. 🧾 Cheat Sheet

| Method | Return Type | Purpose |
|---|---|---|
| `length()` | `int` | String length |
| `charAt()` | `char` | Character at index |
| `substring()` | `String` | Extract portion |
| `concat()` | `String` | Join Strings |
| `equals()` | `boolean` | Content comparison |
| `equalsIgnoreCase()` | `boolean` | Case-insensitive comparison |
| `compareTo()` | `int` | Lexicographical comparison |
| `compareToIgnoreCase()` | `int` | Case-insensitive ordering |
| `contains()` | `boolean` | Search sequence |
| `startsWith()` | `boolean` | Prefix check |
| `endsWith()` | `boolean` | Suffix check |
| `indexOf()` | `int` | First occurrence |
| `lastIndexOf()` | `int` | Last occurrence |
| `isEmpty()` | `boolean` | Zero length |
| `isBlank()` | `boolean` | Empty/whitespace-only |
| `toUpperCase()` | `String` | Uppercase |
| `toLowerCase()` | `String` | Lowercase |
| `trim()` | `String` | Traditional trimming |
| `strip()` | `String` | Unicode-aware trimming |
| `stripLeading()` | `String` | Remove leading whitespace |
| `stripTrailing()` | `String` | Remove trailing whitespace |
| `replace()` | `String` | Literal replacement |
| `replaceFirst()` | `String` | First regex replacement |
| `replaceAll()` | `String` | All regex replacements |
| `split()` | `String[]` | Split String |
| `join()` | `String` | Join Strings |
| `valueOf()` | `String` | Convert value to String |
| `toCharArray()` | `char[]` | String → char array |
| `getBytes()` | `byte[]` | String → bytes |
| `matches()` | `boolean` | Regex matching |
| `format()` | `String` | Format String |
| `repeat()` | `String` | Repeat String |
| `intern()` | `String` | Canonical pooled representation |

---

# 51. 🧠 Memory Tricks

## 🔥 Information

Remember:

```text
L C S

L → length()
C → charAt()
S → substring()
```

## 🔥 Searching

```text
C I L

C → contains()
I → indexOf()
L → lastIndexOf()
```

## 🔥 Comparing

```text
E E C

E → equals()
E → equalsIgnoreCase()
C → compareTo()
```

## 🔥 Checking

```text
S E B

S → startsWith()
E → endsWith()
B → isBlank()
```

## 🔥 Cleaning

```text
T S L T

T → trim()
S → strip()
L → stripLeading()
T → stripTrailing()
```

## 🔥 Replacement

```text
replace()
    ↓
Literal

replaceFirst()
    ↓
Regex + First

replaceAll()
    ↓
Regex + All
```

## 🔥 DSA Memory

Remember:

```text
String Problem
      ↓
Can I Traverse?
      ↓
charAt()
      ↓
Need Frequency?
      ↓
int[26] / HashMap
      ↓
Need Uniqueness?
      ↓
HashSet
      ↓
Need Both Ends?
      ↓
Two Pointers
      ↓
Need Contiguous Range?
      ↓
Sliding Window
```

---

# ⭐ Most Important String Methods for Interviews

```text
length()
charAt()
substring()
equals()
equalsIgnoreCase()
compareTo()
contains()
startsWith()
endsWith()
indexOf()
lastIndexOf()
isEmpty()
isBlank()
replace()
replaceFirst()
replaceAll()
split()
trim()
strip()
toCharArray()
valueOf()
```

---

# 🧠 Most Important String DSA Patterns

| Pattern | Typical Problems | Main Tool |
|---|---|---|
| Traversal | Character processing | `charAt()` |
| Frequency Counting | Anagram, duplicates | `int[]` / `HashMap` |
| Two Pointers | Palindrome, reverse | `left`, `right` |
| Hashing | First unique character | `HashMap` |
| Set | Remove duplicates | `HashSet` |
| Sliding Window | Longest substring | `HashMap` / `HashSet` |
| Sorting | Anagram / ordering | `char[]` |
| Prefix/Suffix | Prefix matching | `startsWith()` / `endsWith()` |
| Parsing | Token extraction | `split()` |
| String Building | Constructing result | `StringBuilder` |

---

# 52. 🔗 Next Topic

Our String playlist:

```text
04-Strings/
│
├── 01-String-Introduction.md
├── 02-String-Pool.md
├── 03-String-Immutability.md
├── 04-String-Methods.md          ← YOU ARE HERE
├── 05-StringBuilder.md
├── 06-StringBuffer.md
├── 07-String-vs-StringBuilder-vs-StringBuffer.md
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
String vs Builder vs Buffer
        ↓
String Interview Questions
```

---

# 🚀 Final Revision

Before moving to `StringBuilder`, make sure you understand:

1. `length()` → number of UTF-16 code units
2. `charAt()` → character at index
3. `substring()` → extracts part of String
4. `equals()` → content comparison
5. `compareTo()` → lexicographical comparison
6. `contains()` → checks sequence
7. `indexOf()` → first occurrence
8. `lastIndexOf()` → last occurrence
9. `isEmpty()` → length == 0
10. `isBlank()` → empty or whitespace-only
11. `replace()` → literal replacement
12. `replaceAll()` → regex replacement
13. `split()` → String to `String[]`
14. `join()` → combines Strings
15. `toCharArray()` → String to `char[]`
16. `valueOf()` → value to String
17. `trim()` → traditional whitespace trimming
18. `strip()` → Unicode-aware whitespace trimming
19. `matches()` → regex validation
20. `intern()` → String Pool canonical representation
21. String methods are heavily used in DSA
22. Frequency problems often use `int[]` or `HashMap`
23. Palindrome/reverse problems commonly use two pointers
24. Substring/window problems commonly use sliding window
25. `StringBuilder` is useful when repeatedly constructing Strings

> ⭐ **Core Idea:**
>
> **String methods make String processing easy, but for DSA the real skill is combining these methods with patterns such as frequency counting, hashing, two pointers, sliding window, and character-array processing.**