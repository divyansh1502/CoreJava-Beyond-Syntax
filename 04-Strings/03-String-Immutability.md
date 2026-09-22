````md
# 🔒 String Immutability in Java

> **String is immutable in Java, which means once a String object is created, its character sequence cannot be changed. Any operation that appears to modify a String creates or returns another String instead of modifying the original object.**

---

# 📌 Table of Contents

1. [What is Immutability?](#1--what-is-immutability)
2. [What Does String Immutability Mean?](#2--what-does-string-immutability-mean)
3. [Simple Example](#3--simple-example)
4. [Mutation vs Reassignment](#4--mutation-vs-reassignment)
5. [How String Modification Actually Works](#5--how-string-modification-actually-works)
6. [Memory Visualization](#6--memory-visualization)
7. [Why is String Immutable?](#7--why-is-string-immutable)
8. [String Pool and Immutability](#8--string-pool-and-immutability)
9. [Immutability and Security](#9--immutability-and-security)
10. [Immutability and HashMap](#10--immutability-and-hashmap)
11. [Immutability and Thread Safety](#11--immutability-and-thread-safety)
12. [String and hashCode()](#12--string-and-hashcode)
13. [Why is String final?](#13--why-is-string-final)
14. [String vs StringBuilder](#14--string-vs-stringbuilder)
15. [Common String Operations](#15--common-string-operations)
16. [DSA Connection](#16--dsa-connection)
17. [How to Know Which DSA Pattern to Use](#17--how-to-know-which-dsa-pattern-to-use)
18. [Important String DSA Patterns](#18--important-string-dsa-patterns)
19. [Important DSA Questions](#19--important-dsa-questions)
20. [Common Mistakes](#20--common-mistakes)
21. [Interview Traps](#21--interview-traps)
22. [Top 20 Interview Questions](#22--top-20-interview-questions)
23. [30-Second Interview Answer](#23--30-second-interview-answer)
24. [Cheat Sheet](#24--cheat-sheet)
25. [Memory Tricks](#25--memory-tricks)
26. [Next Topic](#26--next-topic)
27. [Final Revision](#27--final-revision)

---

# 1. 🔹 What is Immutability?

**Immutable** means:

> An object's state cannot be changed after the object has been created.

For example:

```text
Object
   |
   ↓
State = "Java"
```

If the object is immutable, its state cannot become `"Python"` through modification of that same object.

Instead, another object is created.

### Simple Idea

```text
Immutable Object
       |
       ↓
   Created once
       |
       ↓
 State cannot change
```

---

# 2. 🔤 What Does String Immutability Mean?

String immutability means:

> Once a String object is created, its character sequence cannot be modified.

Example:

```java
String s = "Java";
```

The String object contains:

```text
Java
```

We cannot modify that same String object into:

```text
Python
```

Instead, if we perform an operation that produces a different value, Java returns another String.

### Important

```text
Original String
      ↓
   unchanged

New String
      ↓
 new result
```

---

# 3. 🧪 Simple Example

Consider:

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
s.concat(" Programming")
```

does not modify the original String.

It returns another String.

But we did not store the returned value anywhere.

Therefore:

```text
s → "Java"
```

---

## ✅ Correct Way

```java
String s = "Java";

s = s.concat(" Programming");

System.out.println(s);
```

Output:

```text
Java Programming
```

Now:

```text
s
|
↓
"Java Programming"
```

The original String `"Java"` was not modified.

The reference `s` was simply made to point to another String.

---

# 4. 🔄 Mutation vs Reassignment

This is one of the most important concepts in String immutability.

## Mutation

Mutation means:

> Changing the internal state of an existing object.

Conceptually:

```text
Object
   |
   ↓
State changes
```

Example:

```text
Object state:

"Java"

  ↓ mutation

"Python"
```

For an immutable String, this is not allowed.

---

## Reassignment

Reassignment means:

> Changing which object a reference points to.

Example:

```java
String s = "Java";

s = "Python";
```

Before:

```text
s
|
↓
"Java"
```

After:

```text
s
|
↓
"Python"
```

The `"Java"` object was NOT changed.

The reference `s` simply points to another object.

---

## ⭐ Important Difference

```text
Mutation
   ↓
Changes existing object

Reassignment
   ↓
Changes reference
```

Therefore:

```java
String s = "Java";

s = "Python";
```

is:

```text
❌ Mutation
✅ Reassignment
```

---

# 5. ⚙️ How String Modification Actually Works

Consider:

```java
String s = "Java";

s = s.concat(" World");
```

Let's understand what happens.

## Step 1 — Create String

```java
String s = "Java";
```

Conceptually:

```text
s
|
↓
"Java"
```

---

## Step 2 — Call concat()

```java
s.concat(" World");
```

Conceptually:

```text
"Java" + " World"
          |
          ↓
    "Java World"
```

A new String is produced.

The original:

```text
"Java"
```

remains unchanged.

---

## Step 3 — Assignment

```java
s = s.concat(" World");
```

Now:

```text
s
|
↓
"Java World"
```

The old String `"Java"` was not modified.

---

# 6. 🧠 Memory Visualization

Consider:

```java
String s = "Java";
```

Initially:

```text
┌──────────────┐
│    "Java"    │
└──────────────┘
       ↑
       |
       s
```

Now:

```java
s = s.concat(" Programming");
```

A new String is produced:

```text
┌──────────────┐
│    "Java"    │
└──────────────┘

┌─────────────────────┐
│ "Java Programming"  │
└─────────────────────┘
          ↑
          |
          s
```

The original `"Java"` String was not modified.

---

# 7. 🎯 Why is String Immutable?

String immutability provides several important benefits.

| Reason | Benefit |
|---|---|
| String Pool | Safe sharing |
| Security | Stable values |
| Hashing | Stable hash code |
| Thread Sharing | Safe sharing of immutable state |
| Predictability | String value remains stable |
| Performance | Enables certain optimizations |
| Collection Keys | Suitable for HashMap/HashSet keys |

Let's understand these one by one.

---

# 8. 🏊 String Pool and Immutability

String Pool allows multiple references to share the same String object.

Example:

```java
String s1 = "Java";
String s2 = "Java";
```

Conceptually:

```text
String Pool

       "Java"
       /    \
      /      \
    s1        s2
```

Both references can point to the same object.

Now imagine String were mutable.

Suppose:

```text
s1 changes "Java" → "Python"
```

Then `s2` could unexpectedly see:

```text
Python
```

That would be dangerous.

But String is immutable.

Therefore:

```java
s1 = "Python";
```

does not modify `"Java"`.

Instead:

```text
String Pool

"Java"              "Python"
  ↑                    ↑
  s2                   s1
```

This makes String Pool sharing safe.

---

# 9. 🔐 Immutability and Security

Strings are commonly used for values such as:

```text
File paths
URLs
Class names
Database URLs
Configuration values
Authentication-related information
```

Suppose a value is validated:

```java
String path = "/safe/file.txt";
```

A security check validates:

```text
/safe/file.txt
```

If the String could later be modified into:

```text
/secret/file.txt
```

the validation could become unreliable.

Because String is immutable, the character sequence of that String cannot be changed through normal String operations.

Therefore:

> Once a String value has been created and validated, its contents remain stable.

### Interview Point

String immutability helps make security-sensitive values more predictable and resistant to modification through ordinary APIs.

---

# 10. 🗺️ Immutability and HashMap

String is frequently used as a key in:

```text
HashMap
HashSet
ConcurrentHashMap
```

Example:

```java
Map<String, Integer> map = new HashMap<>();

String key = "Java";

map.put(key, 100);

System.out.println(map.get(key));
```

Output:

```text
100
```

Hash-based collections depend on:

```text
hashCode()
equals()
```

Conceptually:

```text
"Java"
   |
   ↓
hashCode()
   |
   ↓
 Bucket
```

Now imagine the String could be modified after insertion.

For example:

```text
"Java" → "Python"
```

Its hash code could change.

Then the collection could have an entry stored according to the old hash value while the key now produces a different hash value.

That would create problems during lookup.

Because String is immutable:

```text
String content
      ↓
remains stable
      ↓
hashCode remains stable
      ↓
safe as HashMap key
```

---

# 11. 🧵 Immutability and Thread Safety

Immutable objects are easier to safely share between threads.

Example:

```java
String s = "Java";
```

Suppose three threads access it:

```text
             "Java"
            /  |  \
           /   |   \
         T1   T2   T3
```

All threads can safely read the String.

Why?

Because none of them can modify the character sequence of that String object.

Therefore, there is no race condition involving modification of that String's internal character data.

### Important Interview Point

Do NOT say:

```text
"String is thread-safe because it uses synchronization."
```

That is incorrect.

Better:

> String is safe to share because its state cannot be changed after construction.

### ⚠️ Important Nuance

String immutability does not automatically make every program involving Strings thread-safe.

For example:

```java
String[] arr;
```

or:

```java
List<String> list;
```

may still be mutable.

The String objects themselves are immutable, but surrounding objects may not be.

---

# 12. #️⃣ String and hashCode()

String overrides:

```java
hashCode()
```

The hash code is based on its contents.

Example:

```java
String a = "Java";
String b = "Java";

System.out.println(a.hashCode());
System.out.println(b.hashCode());
```

Both Strings have the same content, so their hash codes are equal.

Conceptually:

```text
"Java"
   |
   ↓
hashCode()
   |
   ↓
Stable hash value
```

Because the String cannot change, its content-based hash code remains stable.

This is particularly important when Strings are used as keys in hash-based collections.

---

# 13. 🔒 Why is String final?

String is declared as a final class.

Conceptually:

```java
public final class String
```

`final` means:

> String cannot be subclassed.

Why is that useful?

Because Java wants to maintain the behavior and guarantees associated with String.

If arbitrary subclasses could change important behavior, assumptions around:

```text
Immutability
equals()
hashCode()
Security
Sharing
```

could become more difficult to guarantee.

Therefore:

```text
final
  +
immutable
  +
controlled implementation
  ↓
reliable String behavior
```

### Interview Answer

> String is final so that it cannot be subclassed and its designed behavior and guarantees cannot be altered through inheritance.

---

# 14. 🆚 String vs StringBuilder

If you frequently modify text, String may not be the most efficient choice.

Example:

```java
String result = "";

for (int i = 0; i < 1000; i++) {
    result = result + i;
}
```

Because String is immutable, repeated concatenation can create many intermediate String objects.

For repeated modifications, `StringBuilder` is generally preferred.

Example:

```java
StringBuilder sb = new StringBuilder();

for (int i = 0; i < 1000; i++) {
    sb.append(i);
}
```

### Comparison

| Feature | String | StringBuilder |
|---|---|---|
| Mutable | ❌ No | ✅ Yes |
| Immutable | ✅ Yes | ❌ No |
| Modification | Produces another String | Modifies builder |
| Best for | Fixed/mostly fixed text | Frequent modifications |
| Thread-safe due to immutability | Easy to share | Not inherently thread-safe |
| Repeated concatenation | Can be inefficient | Generally better |

---

# 15. 🛠️ Common String Operations

Most String transformation methods do not modify the original String.

Instead, they return another String.

---

## `concat()`

```java
String s = "Java";
String result = s.concat(" World");

System.out.println(s);
System.out.println(result);
```

Output:

```text
Java
Java World
```

---

## `toUpperCase()`

```java
String s = "java";
String result = s.toUpperCase();

System.out.println(s);
System.out.println(result);
```

Output:

```text
java
JAVA
```

---

## `toLowerCase()`

```java
String s = "JAVA";
String result = s.toLowerCase();

System.out.println(s);
System.out.println(result);
```

Output:

```text
JAVA
java
```

---

## `replace()`

```java
String s = "Java";
String result = s.replace('a', 'o');

System.out.println(s);
System.out.println(result);
```

Output:

```text
Java
Jovo
```

---

## `substring()`

```java
String s = "Java Programming";
String result = s.substring(5);

System.out.println(s);
System.out.println(result);
```

Output:

```text
Java Programming
Programming
```

---

# 16. 🧠 DSA Connection

String immutability itself is a Java language/API concept, but it directly affects how we implement **String-based DSA problems**.

Important DSA areas connected with Strings include:

```text
String
  ↓
Character Processing
  ↓
Frequency Counting
  ↓
Hashing
  ↓
Two Pointers
  ↓
Sliding Window
  ↓
Stack
  ↓
String Construction
  ↓
Pattern Matching
```

The key thing to understand is:

> In DSA, knowing String methods is not enough. You must recognize the underlying problem pattern.

---

# 17. 🔍 How to Know Which DSA Pattern to Use

When you see a String problem, do not immediately start coding.

First identify the problem structure.

## Pattern Recognition Table

| Problem Clue | Think About |
|---|---|
| Count characters | Frequency Array / HashMap |
| Find duplicate characters | Frequency Array / HashMap / Set |
| Check anagram | Frequency Counting |
| Check palindrome | Two Pointers |
| Reverse a String | Two Pointers / StringBuilder |
| Longest substring | Sliding Window |
| Smallest/largest valid substring | Sliding Window |
| Substring with unique characters | Sliding Window + Set/Map |
| Find pair of characters | Two Pointers / Hashing |
| Balanced brackets | Stack |
| Remove adjacent duplicates | Stack |
| Build result repeatedly | StringBuilder |
| Search pattern in text | String Matching |
| Prefix-based problem | Prefix / Trie / KMP depending on problem |
| Repeated lookup | HashMap / HashSet |
| Need sorted characters | Sorting / Counting |
| Compare two strings efficiently | Frequency / Hashing / Character processing |

---

# 18. 🔥 Important String DSA Patterns

## 18.1 Frequency Counting

### Recognize It

Look for words such as:

```text
count
frequency
occurrences
duplicates
anagram
same characters
```

### Example

> Count the frequency of every lowercase English character.

Use an array:

```java
String s = "banana";

int[] freq = new int[26];

for (char ch : s.toCharArray()) {
    freq[ch - 'a']++;
}
```

### Why `ch - 'a'`?

For lowercase English letters:

```text
'a' → 0
'b' → 1
'c' → 2
...
'z' → 25
```

Therefore:

```java
freq[ch - 'a']++;
```

maps each character to an array index.

### Complexity

```text
Time  → O(n)
Space → O(1)
```

The space is `O(1)` because the array always contains 26 positions.

---

# 18.2 HashMap Frequency Counting

Use `HashMap` when the character set is not fixed or when the problem requires more general keys.

Example:

```java
String s = "programming";

Map<Character, Integer> freq = new HashMap<>();

for (char ch : s.toCharArray()) {
    freq.put(ch, freq.getOrDefault(ch, 0) + 1);
}
```

### Recognize It

Use this approach when:

```text
Character set is unknown
Unicode/general characters
Need key-value association
Need flexible frequency counting
```

### Complexity

Average:

```text
Time  → O(n)
Space → O(k)
```

Where `k` is the number of distinct characters.

---

# 18.3 Two Pointers

Two pointers are extremely important for String problems.

### Recognize It

Think **Two Pointers** when:

```text
Palindrome
Reverse
Compare from both ends
Remove characters from both ends
Symmetric comparison
```

### Example — Palindrome

Problem:

> Check whether a String is a palindrome.

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

### How to Think

```text
m a d a m
↑       ↑
L       R

Compare

Move inward

  ↓

  a d a
  ↑   ↑

Continue
```

### Complexity

```text
Time  → O(n)
Space → O(1)
```

---

# 18.4 Sliding Window

Sliding Window is one of the most important String patterns.

### Recognize It

Look for:

```text
substring
longest
shortest
maximum length
minimum length
without repeating
at most K
exactly K
contiguous characters
```

### Example — Longest Substring Without Repeating Characters

```java
String s = "abcabcbb";

Set<Character> set = new HashSet<>();

int left = 0;
int maxLength = 0;

for (int right = 0; right < s.length(); right++) {

    while (set.contains(s.charAt(right))) {
        set.remove(s.charAt(left));
        left++;
    }

    set.add(s.charAt(right));

    maxLength = Math.max(maxLength, right - left + 1);
}

System.out.println(maxLength);
```

Output:

```text
3
```

The longest substring is:

```text
abc
```

### How to Think

```text
abcabcbb
↑  ↑
L  R

Expand R

If duplicate appears:

Move L

Keep window valid
```

### Complexity

```text
Time  → O(n)
Space → O(k)
```

Where `k` is the number of distinct characters stored in the window.

---

# 18.5 Hashing

Hashing is useful when you need fast lookup.

### Recognize It

Look for:

```text
Have I seen this before?
Duplicate?
Frequency?
Pair?
Matching?
Lookup?
```

Example:

```java
String s = "hello";

Set<Character> seen = new HashSet<>();

for (char ch : s.toCharArray()) {

    if (seen.contains(ch)) {
        System.out.println("Duplicate: " + ch);
        break;
    }

    seen.add(ch);
}
```

Average lookup:

```text
O(1)
```

Overall:

```text
Time  → O(n)
Space → O(k)
```

---

# 18.6 Stack

Stack is useful when the problem involves nested or recently opened characters.

### Recognize It

Think **Stack** when you see:

```text
Parentheses
Brackets
Nested structure
Undo
Remove adjacent pairs
Previous unmatched character
```

### Example — Valid Parentheses

```java
String s = "({[]})";

Stack<Character> stack = new Stack<>();

for (char ch : s.toCharArray()) {

    if (ch == '(' || ch == '{' || ch == '[') {
        stack.push(ch);
    } else {

        if (stack.isEmpty()) {
            System.out.println(false);
            return;
        }

        char top = stack.pop();

        if ((ch == ')' && top != '(') ||
            (ch == '}' && top != '{') ||
            (ch == ']' && top != '[')) {

            System.out.println(false);
            return;
        }
    }
}

System.out.println(stack.isEmpty());
```

### Complexity

```text
Time  → O(n)
Space → O(n)
```

---

# 18.7 StringBuilder for Result Construction

Because String is immutable, repeatedly doing:

```java
result = result + ch;
```

can create unnecessary intermediate Strings.

For repeated construction, use `StringBuilder`.

Example:

```java
String s = "hello";

StringBuilder sb = new StringBuilder();

for (int i = s.length() - 1; i >= 0; i--) {
    sb.append(s.charAt(i));
}

String reversed = sb.toString();

System.out.println(reversed);
```

Output:

```text
olleh
```

### Complexity

```text
Time  → O(n)
Space → O(n)
```

---

# 18.8 Sorting-Based String Problems

Sometimes the easiest way to compare two Strings is to sort their characters.

Example:

```text
listen
silent
```

After sorting:

```text
eilnst
eilnst
```

Therefore they are anagrams.

### Basic Approach

```java
String a = "listen";
String b = "silent";

char[] arr1 = a.toCharArray();
char[] arr2 = b.toCharArray();

Arrays.sort(arr1);
Arrays.sort(arr2);

boolean result = Arrays.equals(arr1, arr2);

System.out.println(result);
```

### Complexity

```text
Time  → O(n log n)
Space → O(n)
```

For lowercase English letters, frequency counting can improve this to:

```text
Time  → O(n)
Space → O(1)
```

---

# 18.9 Prefix / Pattern Matching

When the problem involves searching patterns inside larger Strings, recognize:

```text
Pattern matching
Prefix
Repeated pattern
Substring search
```

Important algorithms to learn later:

```text
Naive Pattern Matching
KMP
Rabin-Karp
Z Algorithm
Trie
```

Do not memorize these algorithms blindly.

First understand:

```text
What problem does the algorithm solve?
Why is brute force inefficient?
What information can be reused?
```

---

# 19. 🔥 Important DSA Questions

## Q1. Reverse a String

### How to Recognize

```text
reverse
reverse characters
reverse in-place
```

### Approach

Two pointers can be used if working with a mutable character array.

```java
char[] arr = "hello".toCharArray();

int left = 0;
int right = arr.length - 1;

while (left < right) {

    char temp = arr[left];
    arr[left] = arr[right];
    arr[right] = temp;

    left++;
    right--;
}

System.out.println(new String(arr));
```

### Complexity

```text
Time  → O(n)
Space → O(n)
```

The resulting character array requires `O(n)` space.

---

## Q2. Check Palindrome

### Pattern

```text
Two Pointers
```

### Approach

Compare first and last characters.

```java
String s = "racecar";

int left = 0;
int right = s.length() - 1;

boolean isPalindrome = true;

while (left < right) {

    if (s.charAt(left) != s.charAt(right)) {
        isPalindrome = false;
        break;
    }

    left++;
    right--;
}

System.out.println(isPalindrome);
```

### Complexity

```text
Time  → O(n)
Space → O(1)
```

---

## Q3. Valid Anagram

### Pattern

```text
Frequency Counting
```

```java
String s = "listen";
String t = "silent";

if (s.length() != t.length()) {
    System.out.println(false);
    return;
}

int[] freq = new int[26];

for (char ch : s.toCharArray()) {
    freq[ch - 'a']++;
}

for (char ch : t.toCharArray()) {
    freq[ch - 'a']--;
}

boolean isAnagram = true;

for (int count : freq) {
    if (count != 0) {
        isAnagram = false;
        break;
    }
}

System.out.println(isAnagram);
```

### Complexity

```text
Time  → O(n)
Space → O(1)
```

---

## Q4. First Non-Repeating Character

### Pattern

```text
Frequency Counting
+
Second Traversal
```

```java
String s = "leetcode";

int[] freq = new int[26];

for (char ch : s.toCharArray()) {
    freq[ch - 'a']++;
}

for (int i = 0; i < s.length(); i++) {

    if (freq[s.charAt(i) - 'a'] == 1) {
        System.out.println(i);
        break;
    }
}
```

### How to Think

First pass:

```text
Count everything
```

Second pass:

```text
Find the first character whose count is 1
```

### Complexity

```text
Time  → O(n)
Space → O(1)
```

---

## Q5. Contains Duplicate Character

### Pattern

```text
HashSet
```

```java
String s = "programming";

Set<Character> seen = new HashSet<>();

boolean duplicate = false;

for (char ch : s.toCharArray()) {

    if (!seen.add(ch)) {
        duplicate = true;
        break;
    }
}

System.out.println(duplicate);
```

### Complexity

```text
Time  → O(n) average
Space → O(k)
```

---

## Q6. Longest Substring Without Repeating Characters

### Pattern

```text
Sliding Window
+
HashSet
```

```java
String s = "abcabcbb";

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

System.out.println(maxLength);
```

### Complexity

```text
Time  → O(n)
Space → O(k)
```

---

## Q7. Valid Parentheses

### Pattern

```text
Stack
```

Core idea:

```text
Opening bracket → push

Closing bracket → compare with top
```

```java
String s = "()[]{}";

Stack<Character> stack = new Stack<>();

for (char ch : s.toCharArray()) {

    if (ch == '(' || ch == '[' || ch == '{') {
        stack.push(ch);
        continue;
    }

    if (stack.isEmpty()) {
        System.out.println(false);
        return;
    }

    char top = stack.pop();

    if ((ch == ')' && top != '(') ||
        (ch == ']' && top != '[') ||
        (ch == '}' && top != '{')) {

        System.out.println(false);
        return;
    }
}

System.out.println(stack.isEmpty());
```

### Complexity

```text
Time  → O(n)
Space → O(n)
```

---

## Q8. Reverse Words in a String

### Pattern

```text
String processing
+
StringBuilder
```

Example:

```text
Input:
"Java is powerful"

Output:
"powerful is Java"
```

One possible approach:

```java
String s = "Java is powerful";

String[] words = s.trim().split("\\s+");

StringBuilder result = new StringBuilder();

for (int i = words.length - 1; i >= 0; i--) {

    result.append(words[i]);

    if (i != 0) {
        result.append(" ");
    }
}

System.out.println(result);
```

### Complexity

```text
Time  → O(n)
Space → O(n)
```

---

## Q9. Character Frequency

### Pattern

```text
Frequency Array
```

```java
String s = "banana";

int[] freq = new int[26];

for (char ch : s.toCharArray()) {
    freq[ch - 'a']++;
}

for (int i = 0; i < 26; i++) {

    if (freq[i] > 0) {
        System.out.println(
            (char) ('a' + i) + " = " + freq[i]
        );
    }
}
```

---

## Q10. Remove Duplicate Characters

### Pattern

```text
HashSet
+
StringBuilder
```

```java
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

### Complexity

```text
Time  → O(n) average
Space → O(k)
```

---

# 🧠 DSA Problem-Solving Workflow

When you receive a String problem in an interview, use this process.

```text
                String Problem
                      |
                      ↓
             Understand the task
                      |
                      ↓
          What exactly is required?
                      |
                      ↓
        ┌─────────────┴─────────────┐
        ↓                           ↓
   Character based             Substring based
        ↓                           ↓
 Frequency / Hashing        Sliding Window
        |                           |
        ↓                           ↓
   Duplicate?                  Longest?
   Anagram?                    Shortest?
   Count?                      At most K?
        |
        ↓
   Two Pointers?
        |
        ↓
 Palindrome / Reverse?
        |
        ↓
     Stack?
        |
        ↓
 Brackets / Nested?
        |
        ↓
 StringBuilder?
        |
        ↓
Repeated result construction
```

---

# 🎯 DSA Recognition Cheat Sheet

## If the problem says...

### "Count"

Think:

```text
Frequency Array
HashMap
```

### "Duplicate"

Think:

```text
HashSet
Frequency Array
```

### "Anagram"

Think:

```text
Frequency Counting
Sorting
```

### "Palindrome"

Think:

```text
Two Pointers
```

### "Longest substring"

Think:

```text
Sliding Window
```

### "Without repeating"

Think:

```text
Sliding Window
+
Set / Map
```

### "At most K"

Think:

```text
Sliding Window
```

### "Parentheses"

Think:

```text
Stack
```

### "Reverse"

Think:

```text
Two Pointers
StringBuilder
```

### "Build a String repeatedly"

Think:

```text
StringBuilder
```

### "Fast lookup"

Think:

```text
HashSet
HashMap
```

### "Pattern matching"

Think:

```text
KMP
Rabin-Karp
Z Algorithm
Trie
```

---

# 📊 DSA Complexity Cheat Sheet

| Technique | Typical Time | Typical Space |
|---|---:|---:|
| Character traversal | O(n) | O(1) |
| Frequency array | O(n) | O(1) |
| HashMap frequency | O(n) average | O(k) |
| HashSet lookup | O(1) average | O(k) |
| Two pointers | O(n) | O(1) |
| Sliding window | O(n) | O(k) |
| Sorting characters | O(n log n) | O(n) |
| Stack processing | O(n) | O(n) |
| StringBuilder construction | O(n) | O(n) |

> Always distinguish **input-dependent space** from **fixed auxiliary space**. For example, an `int[26]` is `O(1)` because its size does not grow with input length.

---

# 🧩 How to Improve Your DSA Thinking

Do not memorize:

```text
Problem → Code
```

Instead learn:

```text
Problem
   ↓
Clues
   ↓
Pattern
   ↓
Data Structure
   ↓
Algorithm
   ↓
Complexity
   ↓
Code
```

For example:

```text
"Longest substring without repeating characters"

        ↓

"Longest substring"

        ↓

Sliding Window

        ↓

Need to know whether character exists

        ↓

HashSet / HashMap

        ↓

Expand right

        ↓

Duplicate appears

        ↓

Move left

        ↓

Track maximum window
```

This is the actual DSA skill.

---

# 20. ⚠️ Common Mistakes

## ❌ Mistake 1 — Thinking concat() modifies String

```java
String s = "Java";

s.concat(" World");

System.out.println(s);
```

Output:

```text
Java
```

Why?

Because the returned String was ignored.

---

## ❌ Mistake 2 — Thinking reassignment is mutation

```java
String s = "Java";

s = "Python";
```

This does NOT modify `"Java"`.

It changes what `s` refers to.

---

## ❌ Mistake 3 — Thinking String Pool would work with mutable Strings

If pooled Strings were mutable, different references could accidentally affect the same shared object.

Immutability makes sharing safe.

---

## ❌ Mistake 4 — Using String for heavy modifications

Repeated concatenation:

```java
result = result + value;
```

can create many intermediate Strings.

For heavy modification, consider:

```java
StringBuilder
```

---

## ❌ Mistake 5 — Thinking the reference must be final

This:

```java
String s = "Java";
```

does not mean `s` cannot change.

You can do:

```java
s = "Python";
```

The reference is not final.

The String object itself is immutable.

---

## ❌ Mistake 6 — Using `==` for String content

```java
String a = "Java";
String b = new String("Java");

System.out.println(a == b);
```

This is:

```text
false
```

Use:

```java
a.equals(b)
```

for content comparison.

---

# 21. 🚨 Interview Traps

## Trap 1

```java
String s = "Java";

s.concat(" World");

System.out.println(s);
```

Output:

```text
Java
```

Reason:

`concat()` returns a new String, but the result was ignored.

---

## Trap 2

```java
String s = "Java";

s = s.concat(" World");

System.out.println(s);
```

Output:

```text
Java World
```

Reason:

The returned String was assigned to `s`.

---

## Trap 3

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

## Trap 4

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

## Trap 5

```java
String s = "Java";

System.out.println(s == "Java");
```

Output:

```text
true
```

This is related to String Pooling.

It is NOT because `==` compares String contents.

---

## Trap 6

```java
String a = new String("Java");
String b = new String("Java");

System.out.println(a == b);
```

Output:

```text
false
```

The two references point to different String objects.

---

# 22. 🔥 Top 20 Interview Questions

## Q1. What is String immutability?

**Answer:**

String immutability means that once a String object is created, its character sequence cannot be changed.

---

## Q2. Why is String immutable?

**Answer:**

String immutability provides benefits such as safe String Pool sharing, security properties, stable hash codes, predictable behavior, and easy sharing between threads.

---

## Q3. What happens when we modify a String?

**Answer:**

The original String is not modified. A new String is returned when the operation produces a different value.

---

## Q4. Is this mutation?

```java
String s = "Java";

s = "Python";
```

**Answer:**

No.

This is reassignment.

The reference `s` now points to another String.

---

## Q5. What happens here?

```java
String s = "Java";

s.concat(" World");
```

**Answer:**

A result String is produced, but its reference is ignored. Therefore `s` still refers to `"Java"`.

---

## Q6. How do you store the result?

```java
s = s.concat(" World");
```

Now `s` points to the new String.

---

## Q7. How does immutability help String Pool?

**Answer:**

Multiple references can safely share the same String object because no reference can modify that shared object's contents.

---

## Q8. How does immutability help security?

**Answer:**

Once a String has been created and validated, its contents cannot be modified through normal String APIs.

---

## Q9. Why is String useful as a HashMap key?

**Answer:**

Because its content remains stable, its content-based hash code also remains stable.

---

## Q10. Why is String easy to share between threads?

**Answer:**

Because its state cannot be modified after creation, multiple threads can safely read the same String object.

---

## Q11. Why is String final?

**Answer:**

String is final so it cannot be subclassed and its designed behavior cannot be altered through inheritance.

---

## Q12. Is String immutable because the reference is final?

**Answer:**

No.

String objects are immutable, but a normal String reference can be reassigned.

```java
String s = "Java";

s = "Python";
```

---

## Q13. What is the difference between immutable and final?

**Answer:**

`final` is a language modifier.

For a reference, `final` means the reference cannot be reassigned.

Immutable means the object's state cannot be changed.

Example:

```java
final String s = "Java";
```

Here the reference cannot be reassigned and the String object is immutable.

But:

```java
String s = "Java";
```

allows reassignment even though the String object is immutable.

---

## Q14. Why can repeated String concatenation be inefficient?

**Answer:**

Because String is immutable. Repeated concatenation can create many intermediate String objects.

For repeated modifications, `StringBuilder` is generally preferred.

---

## Q15. Does toUpperCase() modify the original String?

**Answer:**

No.

It returns a String containing the uppercase result.

---

## Q16. Does replace() modify the original String?

**Answer:**

No.

It returns another String containing the replacement result.

---

## Q17. Does String use synchronization to become thread-safe?

**Answer:**

No.

String's immutability is the important reason its instances are safe to share.

---

## Q18. Can String's immutability be bypassed using unusual low-level mechanisms?

**Answer:**

Normal Java APIs cannot modify String contents. Privileged or low-level mechanisms may bypass normal encapsulation, but that is outside normal Java programming.

For normal Java development:

```text
String = immutable
```

---

## Q19. What is the relationship between String immutability and hashCode()?

**Answer:**

String's content remains stable, so its content-based hash code remains stable. This is important when Strings are used as keys in hash-based collections.

---

## Q20. What are the major benefits of String immutability?

**Answer:**

The major benefits are:

```text
1. Safe String Pool sharing
2. Better security properties
3. Stable hash codes
4. Easy thread sharing
5. Predictable behavior
6. Safe use as collection keys
```

---

# 🎯 DSA Interview Questions You Must Practice

For String DSA, these are the important categories to master:

## Beginner

```text
1. Reverse String
2. Check Palindrome
3. Count Characters
4. Character Frequency
5. Count Vowels
6. Count Words
7. Remove Spaces
8. Remove Duplicate Characters
9. First Non-Repeating Character
10. First Repeating Character
```

## Intermediate

```text
11. Valid Anagram
12. Group Anagrams
13. Longest Common Prefix
14. Longest Substring Without Repeating Characters
15. Longest Palindromic Substring
16. Valid Parentheses
17. Reverse Words in a String
18. String Compression
19. Isomorphic Strings
20. Word Pattern
```

## Advanced

```text
21. Minimum Window Substring
22. Find All Anagrams in a String
23. Permutation in String
24. Implement strStr()
25. KMP Pattern Matching
26. Rabin-Karp
27. Z Algorithm
28. Edit Distance
29. Word Break
30. Palindrome Partitioning
```

### ⭐ Priority Patterns

Before attempting advanced String algorithms, become comfortable with:

```text
1. Frequency Array
2. HashMap
3. HashSet
4. Two Pointers
5. Sliding Window
6. Stack
7. Sorting
8. StringBuilder
```

---

# 23. 🎤 30-Second Interview Answer

> **String is immutable in Java, meaning once a String object is created, its character sequence cannot be changed. If we perform operations such as `concat()`, `replace()`, or `toUpperCase()`, the original String remains unchanged and a result String is returned. Immutability allows Strings to be safely shared through the String Pool, provides stable hash codes for use as keys, makes Strings easier to share between threads, and provides useful security and predictability benefits.**

---

# 24. 🧾 Cheat Sheet

| Concept | Key Point |
|---|---|
| Immutable | Object state cannot change |
| String | Immutable |
| `concat()` | Returns another String |
| `replace()` | Returns another String |
| `substring()` | Returns another String |
| `toUpperCase()` | Returns another String |
| Reassignment | Changes reference |
| Mutation | Changes object state |
| String Pool | Safe sharing because String is immutable |
| HashMap Key | Stable content/hashCode |
| Thread Sharing | Easy because String state cannot change |
| `String` | Final class |
| Frequent Modification | Prefer StringBuilder |

---

# 🧠 DSA Cheat Sheet

| Pattern | Recognition Clue | Typical Approach |
|---|---|---|
| Frequency Array | Count/frequency/anagram | `int[26]` |
| HashMap | General frequency/lookup | Key-value |
| HashSet | Duplicate/seen-before | Set |
| Two Pointers | Palindrome/reverse | Left + right |
| Sliding Window | Longest/shortest substring | Left + right window |
| Stack | Brackets/nested structure | Push/pop |
| Sorting | Compare character composition | Sort arrays |
| StringBuilder | Build result repeatedly | Append |
| KMP | Efficient pattern matching | Prefix table |
| Rabin-Karp | Pattern matching with hashing | Rolling hash |
| Trie | Prefix/search dictionary | Prefix tree |
| DP | Overlapping String subproblems | Memoization/table |

---

# 25. 🧠 Memory Tricks

## 🔥 Remember "ISH"

String immutability gives you:

```text
I → Immutable
S → Safe Sharing
H → Hash Stability
```

---

## 🔥 Remember Mutation vs Reassignment

```text
Mutation
   ↓
Change object

Reassignment
   ↓
Change reference
```

Example:

```java
String s = "Java";

s = "Python";
```

Remember:

```text
Reference changed
Object did not
```

---

## 🔥 String Method Rule

When you see:

```java
s.someStringMethod();
```

ask yourself:

> "Does this method return a new String?"

For String transformation methods:

```text
Original String
      ↓
  unchanged

Returned String
      ↓
transformed result
```

---

## 🔥 DSA Pattern Rule

Remember:

```text
Count
 ↓
Frequency

Duplicate
 ↓
Set / Frequency

Palindrome
 ↓
Two Pointers

Longest Substring
 ↓
Sliding Window

Brackets
 ↓
Stack

Build String
 ↓
StringBuilder

Fast Lookup
 ↓
Hashing
```

---

# 26. 🔗 Next Topic

Our String playlist:

```text
04-Strings/

│
├── 01-String-Introduction.md
│
├── 02-String-Pool.md
│
├── 03-String-Immutability.md     ← YOU ARE HERE
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
Interview Questions
```

---

# 27. 🚀 Final Revision

Before moving to the next topic, remember these points:

```text
1. String objects are immutable.

2. Immutable means the object's state cannot be changed.

3. String methods return new Strings when a changed result is required.

4. Reassigning a reference is not mutation.

5. String Pool relies on immutability for safe sharing.

6. Immutability gives Strings stable content.

7. Stable content means stable content-based hashCode.

8. String is suitable as a HashMap key.

9. Immutable Strings are easy to safely share between threads.

10. String is final and cannot be subclassed.

11. Repeated String modification can create intermediate objects.

12. StringBuilder is generally preferred for frequent modifications.

13. Frequency problems usually suggest a frequency array or HashMap.

14. Palindrome problems usually suggest two pointers.

15. Longest/shortest substring problems often suggest sliding window.

16. Duplicate/seen-before problems often suggest HashSet.

17. Parentheses and nested-character problems often suggest Stack.

18. Repeated result construction usually suggests StringBuilder.

19. Always identify the DSA pattern before writing code.

20. Always analyze time and space complexity.
```

---

# ⭐ One-Line Interview Memory

> **String is immutable → its contents cannot change → safe sharing becomes possible → String Pool works safely → hashing and thread sharing become easier.**

# ⭐ One-Line DSA Memory

> **String problem → identify the clue → choose the pattern → choose the data structure → optimize complexity → then code.**
````
