# 🛠️ String Methods in Java

> **The `String` class provides many built-in methods for searching, comparing, extracting, modifying, checking, and converting String data. Since String is immutable, methods that appear to modify a String return a new String instead of changing the original object.**

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
42. [Common Mistakes](#42--common-mistakes)
43. [Interview Traps](#43--interview-traps)
44. [Top 20 Interview Questions](#44--top-20-interview-questions)
45. [30-Second Interview Answer](#45--30-second-interview-answer)
46. [Cheat Sheet](#46--cheat-sheet)
47. [Memory Tricks](#47--memory-tricks)
48. [Next Topic](#48--next-topic)

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

Java provides methods so we don't have to manually implement common operations.

For example:

    String name = "Divyansh";

Instead of manually counting characters:

    name.length();

Instead of manually searching for a character:

    name.indexOf('y');

Instead of manually converting to uppercase:

    name.toUpperCase();

---

# 2. 🧩 String Method Categories

String methods can be grouped conceptually.

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

### Definition

`length()` returns the number of characters in a String.

### Syntax

    string.length();

### Example

    String s = "Java";

    System.out.println(s.length());

Output:

    4

### Another Example

    String s = "Java Programming";

    System.out.println(s.length());

Output:

    16

Remember:

    "Java Programming"
     1234567890123456

### Important

`length()` is a method for String.

    String → length()

For arrays:

    array.length

Notice:

    String → length()
    Array  → length

---

# 4. 🔤 charAt()

### Definition

`charAt(index)` returns the character present at the specified index.

### Syntax

    string.charAt(index);

Indexes start from:

    0

Example:

    String s = "Java";

    System.out.println(s.charAt(0));
    System.out.println(s.charAt(1));
    System.out.println(s.charAt(2));
    System.out.println(s.charAt(3));

Output:

    J
    a
    v
    a

### Index Visualization

    String:  J  a  v  a
    Index:   0  1  2  3

### Invalid Index

    s.charAt(4);

throws:

    StringIndexOutOfBoundsException

because valid indexes are:

    0 to length - 1

---

# 5. ✂️ substring()

`substring()` extracts a portion of a String.

There are two commonly used forms:

    substring(beginIndex)

and:

    substring(beginIndex, endIndex)

---

## substring(beginIndex)

The substring starts from `beginIndex` and continues until the end.

Example:

    String s = "Java Programming";

    System.out.println(s.substring(5));

Output:

    Programming

Index:

    J a v a _ P r o g r a m m i n g
    0 1 2 3 4 5 6 7 8 9 ...

Starting from index `5`:

    Programming

---

## substring(beginIndex, endIndex)

The `beginIndex` is inclusive.

The `endIndex` is exclusive.

Example:

    String s = "Java";

    System.out.println(s.substring(1, 3));

Output:

    av

Indexes:

    J   a   v   a
    0   1   2   3

Range:

    1 → included
    3 → excluded

Therefore:

    index 1 = a
    index 2 = v

Result:

    av

### ⭐ Interview Rule

    substring(start, end)

means:

    start → inclusive
    end   → exclusive

Remember:

    [start, end)

---

# 6. ➕ concat()

`concat()` joins another String to the current String.

### Syntax

    string.concat(anotherString);

Example:

    String s1 = "Java";
    String s2 = "Programming";

    String result = s1.concat(s2);

    System.out.println(result);

Output:

    JavaProgramming

With a space:

    String result = s1.concat(" " + s2);

Output:

    Java Programming

### Important

String is immutable.

Therefore:

    String s = "Java";

    s.concat(" World");

does NOT change `s`.

Correct:

    s = s.concat(" World");

---

# 7. 🟰 equals()

`equals()` compares String contents.

### Example

    String a = "Java";
    String b = "Java";

    System.out.println(a.equals(b));

Output:

    true

### Different Content

    String a = "Java";
    String b = "Python";

    System.out.println(a.equals(b));

Output:

    false

### ⭐ Important

For String content comparison:

    equals()

is normally preferred over:

    ==

---

# 8. 🔤 equalsIgnoreCase()

Compares String contents while ignoring letter case.

Example:

    String a = "Java";
    String b = "JAVA";

    System.out.println(a.equalsIgnoreCase(b));

Output:

    true

Without ignoring case:

    a.equals(b)

returns:

    false

### Use Case

Useful when case should not matter.

Example:

    "yes"
    "YES"
    "Yes"

All can be treated as equal using:

    equalsIgnoreCase()

---

# 9. 📊 compareTo()

`compareTo()` compares two Strings lexicographically.

### Syntax

    string1.compareTo(string2);

It returns:

    0
    negative value
    positive value

### If Strings are Equal

    String a = "Java";
    String b = "Java";

    System.out.println(a.compareTo(b));

Output:

    0

### If First String Comes Before Second

    "Apple".compareTo("Banana")

returns a negative value.

### If First String Comes After Second

    "Banana".compareTo("Apple")

returns a positive value.

### Concept

Think:

    Negative → first String comes before second
    Zero     → both are equal
    Positive → first String comes after second

### ⚠️ Important

Do NOT depend on the exact positive or negative number.

Focus on:

    < 0
    == 0
    > 0

---

# 10. 🔤 compareToIgnoreCase()

Works like `compareTo()` but ignores case.

Example:

    String a = "java";
    String b = "JAVA";

    System.out.println(a.compareToIgnoreCase(b));

Output:

    0

Because they are equal ignoring case.

---

# 11. 🔎 contains()

Checks whether a String contains a specified sequence.

### Syntax

    string.contains(sequence);

Returns:

    true
    false

Example:

    String s = "Java Programming";

    System.out.println(s.contains("Java"));

Output:

    true

Another:

    System.out.println(s.contains("Python"));

Output:

    false

### Important

`contains()` is case-sensitive.

    "Java".contains("java")

returns:

    false

---

# 12. 🚀 startsWith()

Checks whether a String starts with a specified prefix.

Example:

    String s = "Java Programming";

    System.out.println(s.startsWith("Java"));

Output:

    true

Example:

    System.out.println(s.startsWith("Python"));

Output:

    false

### Overloaded Version

You can also specify where checking should start.

Conceptually:

    startsWith(prefix, offset)

Example:

    String s = "HelloJava";

    System.out.println(s.startsWith("Java", 5));

Output:

    true

---

# 13. 🏁 endsWith()

Checks whether a String ends with a specified suffix.

Example:

    String s = "Hello.java";

    System.out.println(s.endsWith(".java"));

Output:

    true

Example:

    System.out.println(s.endsWith(".txt"));

Output:

    false

### Common Use

Checking file extensions:

    filename.endsWith(".java")

---

# 14. 🔍 indexOf()

`indexOf()` returns the index of the first occurrence of a character or substring.

Example:

    String s = "Java Programming";

    System.out.println(s.indexOf('a'));

Output:

    1

Because:

    J a v a
    0 1 2 3

The first `a` occurs at index `1`.

### Searching for String

    String s = "Java Programming";

    System.out.println(s.indexOf("Programming"));

Output:

    5

### Not Found

    s.indexOf("Python")

returns:

    -1

### ⭐ Important

    indexOf()

returns the first occurrence.

---

# 15. 🔎 lastIndexOf()

`lastIndexOf()` returns the index of the last occurrence.

Example:

    String s = "Java";

    System.out.println(s.lastIndexOf('a'));

Output:

    3

Indexes:

    J a v a
    0 1 2 3

First `a`:

    1

Last `a`:

    3

### Not Found

Returns:

    -1

---

# 16. 🈳 isEmpty()

Checks whether the String has length `0`.

Example:

    String s = "";

    System.out.println(s.isEmpty());

Output:

    true

Example:

    String s = "Java";

    System.out.println(s.isEmpty());

Output:

    false

### Important

A String containing spaces is NOT empty.

    String s = "   ";

    System.out.println(s.isEmpty());

Output:

    false

Because:

    length > 0

---

# 17. 🧹 isBlank()

`isBlank()` checks whether a String is empty or contains only whitespace.

Available since:

    Java 11

Example:

    String s = "   ";

    System.out.println(s.isBlank());

Output:

    true

Example:

    String s = "Java";

    System.out.println(s.isBlank());

Output:

    false

### Difference

    isEmpty()
        ↓
    Checks length == 0

    isBlank()
        ↓
    Empty OR whitespace-only

### Comparison

| String | `isEmpty()` | `isBlank()` |
|---|---:|---:|
| `""` | true | true |
| `"   "` | false | true |
| `"Java"` | false | false |

---

# 18. 🔠 toUpperCase()

Converts characters to uppercase.

Example:

    String s = "java";

    String result = s.toUpperCase();

    System.out.println(result);

Output:

    JAVA

### Important

Original String remains unchanged.

    String s = "java";

    s.toUpperCase();

    System.out.println(s);

Output:

    java

Correct:

    s = s.toUpperCase();

---

# 19. 🔡 toLowerCase()

Converts characters to lowercase.

Example:

    String s = "JAVA";

    String result = s.toLowerCase();

    System.out.println(result);

Output:

    java

Again:

    Original String → unchanged
    Returned String → lowercase result

---

# 20. 🧹 trim()

`trim()` removes leading and trailing characters whose code points are less than or equal to U+0020.

In simple terms, it removes many traditional ASCII-style leading/trailing whitespace characters.

Example:

    String s = "   Java   ";

    System.out.println(s.trim());

Output:

    Java

### Important

It removes whitespace from:

    Beginning
    End

It does NOT remove spaces in the middle.

Example:

    "Java   Programming"

remains:

    "Java   Programming"

---

# 21. 🧹 strip()

`strip()` also removes leading and trailing whitespace.

Unlike `trim()`, it is Unicode-aware.

Available since:

    Java 11

Example:

    String s = "   Java   ";

    System.out.println(s.strip());

Output:

    Java

### Comparison

| Method | Since | Whitespace Handling |
|---|---:|---|
| `trim()` | Java 1.0 | Traditional/ASCII-oriented |
| `strip()` | Java 11 | Unicode-aware |

### Interview Point

Prefer `strip()` when Unicode-aware whitespace handling is required.

---

# 22. ⬅️ stripLeading()

Removes whitespace from the beginning only.

Example:

    String s = "   Java   ";

    System.out.println(s.stripLeading());

Result conceptually:

    "Java   "

The trailing spaces remain.

---

# 23. ➡️ stripTrailing()

Removes whitespace from the end only.

Example:

    String s = "   Java   ";

    System.out.println(s.stripTrailing());

Result conceptually:

    "   Java"

The leading spaces remain.

---

# 24. 🔄 replace()

`replace()` replaces characters or literal character sequences.

### Character Replacement

    String s = "Java";

    String result = s.replace('a', 'o');

    System.out.println(result);

Output:

    Jovo

### String Replacement

    String s = "Java Java";

    String result = s.replace("Java", "Python");

Output:

    Python Python

### Important

`replace()` treats the target as a literal value.

It does NOT interpret the target as a regular expression.

---

# 25. 1️⃣ replaceFirst()

`replaceFirst()` replaces the first substring that matches a regular expression.

Example:

    String s = "Java Java";

    String result = s.replaceFirst("Java", "Python");

Result:

    Python Java

### Important

It works with:

    Regular Expression

and replaces only the first matching occurrence.

---

# 26. 🔁 replaceAll()

`replaceAll()` replaces every substring matching a regular expression.

Example:

    String s = "Java123";

    String result = s.replaceAll("[0-9]", "");

    System.out.println(result);

Output:

    Java

Here:

    [0-9]

means digits from 0 to 9.

### Another Example

    String s = "Java   Programming";

    String result = s.replaceAll("\\s+", " ");

Result:

    Java Programming

### ⭐ Important Difference

    replace()
        ↓
    Literal replacement

    replaceFirst()
        ↓
    Regex + first match

    replaceAll()
        ↓
    Regex + all matches

---

# 27. ✂️ split()

`split()` divides a String into an array based on a regular expression.

Example:

    String s = "Java,Python,C++";

    String[] languages = s.split(",");

Result:

    languages[0] → "Java"
    languages[1] → "Python"
    languages[2] → "C++"

### Another Example

    String s = "Java Python C++";

    String[] arr = s.split(" ");

Result:

    Java
    Python
    C++

### Important

The argument is a regular expression.

Therefore, some characters need escaping.

For example:

    String s = "a.b.c";

To split on a literal dot:

    s.split("\\.");

because `.` has special meaning in regex.

---

# 28. 🔗 join()

`join()` combines multiple Strings using a delimiter.

Example:

    String result = String.join("-", "2026", "09", "21");

Output:

    2026-09-21

### Another Example

    String result = String.join(", ", "Java", "Python", "C++");

Output:

    Java, Python, C++

### Important

`join()` is a static method of String.

We call it using:

    String.join(...)

not:

    object.join(...)

---

# 29. 🔄 valueOf()

`String.valueOf()` converts different data types into a String representation.

Example:

    int num = 100;

    String s = String.valueOf(num);

Now:

    num → 100
    s   → "100"

### Examples

    String.valueOf(100);
    String.valueOf(10.5);
    String.valueOf(true);
    String.valueOf('A');

### Why Useful?

It provides a convenient way to convert primitive values into Strings.

---

# 30. 🔡 toCharArray()

Converts a String into a character array.

Example:

    String s = "Java";

    char[] arr = s.toCharArray();

Result:

    arr[0] → J
    arr[1] → a
    arr[2] → v
    arr[3] → a

Visualization:

    "Java"

       ↓

    ['J', 'a', 'v', 'a']

### Use Case

Useful when you need to process individual characters.

Example:

    String s = "Java";

    char[] arr = s.toCharArray();

    for(char ch : arr) {
        System.out.println(ch);
    }

---

# 31. 🧱 getBytes()

Converts a String into a byte array using a character encoding.

Example:

    String s = "Java";

    byte[] arr = s.getBytes();

For normal ASCII characters, the bytes correspond to their encoding values under the chosen/default encoding.

### Better Practice

When encoding matters, specify it explicitly.

For example:

    byte[] arr = s.getBytes(StandardCharsets.UTF_8);

This makes the encoding clear and predictable.

### Important

For production code, prefer an explicit charset such as:

    StandardCharsets.UTF_8

instead of relying on the platform default charset.

---

# 32. 🧪 matches()

`matches()` checks whether the entire String matches a regular expression.

Example:

    String s = "12345";

    System.out.println(s.matches("\\d+"));

Output:

    true

Here:

    \\d+

means:

    One or more digits

### Another Example

    String s = "Java";

    System.out.println(s.matches("[A-Za-z]+"));

Output:

    true

### ⭐ Important

`matches()` checks the entire String against the regex.

---

# 33. 📝 format()

`String.format()` creates a formatted String.

Example:

    String name = "Divyansh";
    int age = 22;

    String result = String.format(
        "Name: %s, Age: %d",
        name,
        age
    );

Result:

    Name: Divyansh, Age: 22

### Common Format Specifiers

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

`repeat()` creates a String by repeating the current String a specified number of times.

Available since:

    Java 11

Example:

    String s = "Hi";

    System.out.println(s.repeat(3));

Output:

    HiHiHi

Another example:

    System.out.println("*".repeat(5));

Output:

    *****

### Important

The count must not be negative.

---

# 35. 🏊 intern()

`intern()` returns the canonical representation of a String.

It is closely related to the String Pool.

Example:

    String s1 = new String("Java");

    String s2 = s1.intern();

Now `s2` refers to the pooled representation of `"Java"`.

Conceptually:

    String Pool
         |
         ↓
      "Java"
         ↑
         |
        s2

### Important

`intern()` is an advanced String Pool concept.

Don't use it casually for normal String processing.

---

# 36. 🆚 String Comparison

There are several ways to compare Strings.

## `==`

Compares references.

Example:

    String a = new String("Java");
    String b = new String("Java");

    System.out.println(a == b);

Output:

    false

Because they are different objects.

---

## `equals()`

Compares contents.

    System.out.println(a.equals(b));

Output:

    true

### ⭐ Rule

    ==      → reference comparison

    equals  → content comparison

---

## `compareTo()`

Used for lexicographical comparison.

Returns:

    Negative → first comes before second
    Zero     → equal
    Positive → first comes after second

---

# 37. 🧾 Methods That Return String

Important String methods that return another String include:

| Method | Return |
|---|---|
| `concat()` | String |
| `substring()` | String |
| `toUpperCase()` | String |
| `toLowerCase()` | String |
| `trim()` | String |
| `strip()` | String |
| `stripLeading()` | String |
| `stripTrailing()` | String |
| `replace()` | String |
| `replaceFirst()` | String |
| `replaceAll()` | String |
| `repeat()` | String |
| `format()` | String |

### Important

Because String is immutable, these operations do not modify the original String.

---

# 38. ✅ Methods That Return boolean

Important methods:

| Method | Purpose |
|---|---|
| `equals()` | Content comparison |
| `equalsIgnoreCase()` | Case-insensitive comparison |
| `contains()` | Contains sequence? |
| `startsWith()` | Starts with prefix? |
| `endsWith()` | Ends with suffix? |
| `isEmpty()` | Length zero? |
| `isBlank()` | Empty/whitespace only? |
| `matches()` | Matches regex? |

---

# 39. 🔢 Methods That Return int

Important methods:

| Method | Purpose |
|---|---|
| `length()` | Number of characters |
| `indexOf()` | First matching index |
| `lastIndexOf()` | Last matching index |
| `compareTo()` | Lexicographical comparison |
| `compareToIgnoreCase()` | Case-insensitive comparison |

---

# 40. 🔤 Methods That Return char

Main method:

    charAt()

Example:

    String s = "Java";

    char ch = s.charAt(2);

Result:

    v

---

# 41. 📦 Methods That Return Arrays

## toCharArray()

Returns:

    char[]

Example:

    String s = "Java";

    char[] arr = s.toCharArray();

---

## split()

Returns:

    String[]

Example:

    String s = "Java Python C++";

    String[] arr = s.split(" ");

---

## getBytes()

Returns:

    byte[]

Example:

    byte[] arr = "Java".getBytes();

---

# 42. ⚠️ Common Mistakes

## ❌ Mistake 1 — Using `==` for content comparison

Wrong:

    String a = "Java";
    String b = new String("Java");

    if(a == b) {
        // content is equal
    }

Correct:

    if(a.equals(b)) {
        // content is equal
    }

---

## ❌ Mistake 2 — Forgetting immutability

Wrong assumption:

    String s = "Java";

    s.toUpperCase();

    System.out.println(s);

Expected by beginner:

    JAVA

Actual:

    Java

Correct:

    s = s.toUpperCase();

---

## ❌ Mistake 3 — Confusing isEmpty() and isBlank()

    "   ".isEmpty()

returns:

    false

while:

    "   ".isBlank()

returns:

    true

---

## ❌ Mistake 4 — Forgetting substring end index is exclusive

    "Java".substring(1, 3)

returns:

    "av"

NOT:

    "ava"

Remember:

    [start, end)

---

## ❌ Mistake 5 — Forgetting indexOf() returns -1

If the value is not found:

    indexOf()

returns:

    -1

---

## ❌ Mistake 6 — Confusing replace() with replaceAll()

    replace()
        ↓
    Literal replacement

    replaceAll()
        ↓
    Regular expression replacement

---

## ❌ Mistake 7 — Forgetting split() uses regex

For:

    "a.b.c"

Use:

    split("\\.")

not:

    split(".")

because `.` has special meaning in regex.

---

# 43. 🚨 Interview Traps

## Trap 1

    String s = "Java";

    s.toUpperCase();

    System.out.println(s);

Output:

    Java

---

## Trap 2

    String s = "Java";

    s = s.toUpperCase();

    System.out.println(s);

Output:

    JAVA

---

## Trap 3

    String s = "Java";

    System.out.println(s.substring(1, 3));

Output:

    av

---

## Trap 4

    String s = "Java";

    System.out.println(s.indexOf('a'));

Output:

    1

---

## Trap 5

    String s = "Java";

    System.out.println(s.lastIndexOf('a'));

Output:

    3

---

## Trap 6

    String s = "   ";

    System.out.println(s.isEmpty());

Output:

    false

---

## Trap 7

    String s = "   ";

    System.out.println(s.isBlank());

Output:

    true

---

## Trap 8

    String s = "Java Java";

    System.out.println(
        s.replaceFirst("Java", "Python")
    );

Output:

    Python Java

---

## Trap 9

    String s = "Java Java";

    System.out.println(
        s.replaceAll("Java", "Python")
    );

Output:

    Python Python

---

## Trap 10

    String s = "Java";

    System.out.println(s.contains("java"));

Output:

    false

Reason:

`contains()` is case-sensitive.

---

# 44. 🔥 Top 20 Interview Questions

## Q1. What is the difference between `length` and `length()`?

**Answer:**

For arrays:

    array.length

For String:

    string.length()

`length` is an array property, while `length()` is a String method.

---

## Q2. What does `charAt()` return?

**Answer:**

It returns the character at the specified index.

Return type:

    char

---

## Q3. What happens if an invalid index is passed to `charAt()`?

**Answer:**

A `StringIndexOutOfBoundsException` is thrown.

---

## Q4. What is the difference between `substring(2)` and `substring(2, 5)`?

**Answer:**

`substring(2)` starts at index 2 and goes to the end.

`substring(2, 5)` starts at index 2 and stops before index 5.

---

## Q5. Is the end index of substring() inclusive?

**Answer:**

No.

The start index is inclusive and the end index is exclusive.

    [start, end)

---

## Q6. What does indexOf() return if an element is not found?

**Answer:**

It returns:

    -1

---

## Q7. What is the difference between indexOf() and lastIndexOf()?

**Answer:**

`indexOf()` returns the first occurrence.

`lastIndexOf()` returns the last occurrence.

---

## Q8. What is the difference between equals() and equalsIgnoreCase()?

**Answer:**

`equals()` compares content with case sensitivity.

`equalsIgnoreCase()` compares content while ignoring case differences.

---

## Q9. What does compareTo() return?

**Answer:**

It returns:

    Negative → first String comes before second
    Zero     → Strings are equal
    Positive → first String comes after second

---

## Q10. What is the difference between isEmpty() and isBlank()?

**Answer:**

`isEmpty()` checks whether length is zero.

`isBlank()` checks whether the String is empty or contains only whitespace.

---

## Q11. What is the difference between trim() and strip()?

**Answer:**

`trim()` uses traditional whitespace rules based around characters up to U+0020.

`strip()` is Unicode-aware and was introduced in Java 11.

---

## Q12. What does replace() do?

**Answer:**

It replaces literal characters or character sequences and returns a new String.

---

## Q13. What is the difference between replace(), replaceFirst(), and replaceAll()?

**Answer:**

    replace()
        → Literal replacement

    replaceFirst()
        → Regex replacement of first match

    replaceAll()
        → Regex replacement of all matches

---

## Q14. Does replace() modify the original String?

**Answer:**

No.

String is immutable, so a new String is returned.

---

## Q15. What does split() return?

**Answer:**

It returns a:

    String[]

It splits the String according to a regular expression.

---

## Q16. What does String.join() do?

**Answer:**

It joins multiple Strings using a specified delimiter.

Example:

    String.join("-", "A", "B", "C")

Result:

    A-B-C

---

## Q17. What does toCharArray() return?

**Answer:**

It converts a String into:

    char[]

---

## Q18. What does String.valueOf() do?

**Answer:**

It converts values of different types into their String representation.

---

## Q19. What does matches() do?

**Answer:**

It checks whether the entire String matches a specified regular expression.

---

## Q20. Why do most String transformation methods return a new String?

**Answer:**

Because String is immutable. The existing String cannot be modified, so a different String is returned when a changed value is required.

---

# 45. 🎤 30-Second Interview Answer

> **The String class provides many built-in methods for operations such as comparison, searching, extraction, conversion, replacement, and validation. Important methods include `length()`, `charAt()`, `substring()`, `equals()`, `compareTo()`, `contains()`, `indexOf()`, `replace()`, `split()`, and `toUpperCase()`. Since String is immutable, methods that produce modified text return a new String rather than changing the original String.**

---

# 46. 🧾 Cheat Sheet

| Method | Return Type | Purpose |
|---|---|---|
| `length()` | `int` | String length |
| `charAt()` | `char` | Character at index |
| `substring()` | `String` | Extract portion |
| `concat()` | `String` | Join Strings |
| `equals()` | `boolean` | Content comparison |
| `equalsIgnoreCase()` | `boolean` | Case-insensitive comparison |
| `compareTo()` | `int` | Lexicographical comparison |
| `contains()` | `boolean` | Search sequence |
| `startsWith()` | `boolean` | Check prefix |
| `endsWith()` | `boolean` | Check suffix |
| `indexOf()` | `int` | First occurrence |
| `lastIndexOf()` | `int` | Last occurrence |
| `isEmpty()` | `boolean` | Check zero length |
| `isBlank()` | `boolean` | Check blank String |
| `toUpperCase()` | `String` | Uppercase |
| `toLowerCase()` | `String` | Lowercase |
| `trim()` | `String` | Remove traditional leading/trailing whitespace |
| `strip()` | `String` | Remove Unicode-aware leading/trailing whitespace |
| `replace()` | `String` | Literal replacement |
| `replaceFirst()` | `String` | Replace first regex match |
| `replaceAll()` | `String` | Replace all regex matches |
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

# 47. 🧠 Memory Tricks

## 🔥 Information

Remember:

    L C S

    L → length()
    C → charAt()
    S → substring()

---

## 🔥 Searching

Remember:

    C I L

    C → contains()
    I → indexOf()
    L → lastIndexOf()

---

## 🔥 Comparing

Remember:

    E E C

    E → equals()
    E → equalsIgnoreCase()
    C → compareTo()

---

## 🔥 Checking

Remember:

    S E B

    S → startsWith()
    E → endsWith()
    B → isBlank()

---

## 🔥 Cleaning

Remember:

    T S L T

    T → trim()
    S → strip()
    L → stripLeading()
    T → stripTrailing()

---

## 🔥 Replacement

Remember:

    R → Replace

    replace()
    replaceFirst()
    replaceAll()

Think:

    replace()
       ↓
    Literal

    replaceFirst()
       ↓
    Regex + First

    replaceAll()
       ↓
    Regex + All

---

# ⭐ Most Important Methods for Interviews

If the interviewer asks you to quickly name important String methods, remember:

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

---

# 48. 🔗 Next Topic

Our String playlist:

    04-Strings/
    │
    ├── 01-String-Introduction.md
    ├── 02-String-Pool.md
    ├── 03-String-Immutability.md
    ├── 04-String-Methods.md        ← YOU ARE HERE
    ├── 05-StringBuilder.md
    ├── 06-StringBuffer.md
    ├── 07-String-vs-StringBuilder-vs-StringBuffer.md
    └── 08-String-Interview-Questions.md

### Learning Flow

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

---

# 🚀 Final Revision

Before moving to `StringBuilder`, make sure you understand:

    1. length() → number of characters
    2. charAt() → character at index
    3. substring() → extracts part of String
    4. equals() → content comparison
    5. compareTo() → lexicographical comparison
    6. contains() → checks sequence
    7. indexOf() → first occurrence
    8. lastIndexOf() → last occurrence
    9. isEmpty() → length == 0
    10. isBlank() → empty or whitespace-only
    11. replace() → literal replacement
    12. replaceAll() → regex replacement
    13. split() → String to String[]
    14. join() → combines Strings
    15. toCharArray() → String to char[]
    16. valueOf() → value to String
    17. trim() → traditional leading/trailing whitespace
    18. strip() → Unicode-aware leading/trailing whitespace
    19. matches() → regex validation
    20. intern() → String Pool canonical representation

> ⭐ **Core Idea:**  
> **String methods make String processing easy, but remember the golden rule: String is immutable, so methods that produce changed text return a new String instead of modifying the original.**