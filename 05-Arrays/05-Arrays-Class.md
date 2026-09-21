# 🧰 Arrays Class in Java

> **`Arrays` is a utility class from `java.util` that provides static methods for performing common operations on arrays such as sorting, searching, copying, comparing, filling, and converting arrays into readable strings.**

---

# 📌 Table of Contents

1. [What is Arrays Class?](#1--what-is-arrays-class)
2. [Why Do We Need Arrays Class?](#2--why-do-we-need-arrays-class)
3. [Importing Arrays](#3--importing-arrays)
4. [Arrays as a Utility Class](#4--arrays-as-a-utility-class)
5. [Important Methods](#5--important-methods)
6. [`Arrays.toString()`](#6--arraystostring)
7. [`Arrays.deepToString()`](#7--arraysdeeptostring)
8. [`Arrays.sort()`](#8--arrayssort)
9. [Sorting a Range](#9--sorting-a-range)
10. [`Arrays.parallelSort()`](#10--arraysparallelsort)
11. [`Arrays.binarySearch()`](#11--arraysbinarysearch)
12. [`Arrays.copyOf()`](#12--arrayscopyof)
13. [`Arrays.copyOfRange()`](#13--arrayscopyofrange)
14. [`Arrays.fill()`](#14--arraysfill)
15. [`Arrays.equals()`](#15--arraysequals)
16. [`Arrays.deepEquals()`](#16--arraysdeepequals)
17. [`Arrays.compare()`](#17--arrayscompare)
18. [`Arrays.compareUnsigned()`](#18--arrayscompareunsigned)
19. [`Arrays.mismatch()`](#19--arraysmismatch)
20. [`Arrays.asList()`](#20--arraysaslist)
21. [`Arrays.stream()`](#21--arraysstream)
22. [`Arrays.hashCode()`](#22--arrayshashcode)
23. [`Arrays.deepHashCode()`](#23--arraysdeephashcode)
24. [Arrays of Objects](#24--arrays-of-objects)
25. [Primitive Arrays vs Object Arrays](#25--primitive-arrays-vs-object-arrays)
26. [Shallow Copy and Arrays](#26--shallow-copy-and-arrays)
27. [Arrays Class vs Array](#27--arrays-class-vs-array)
28. [Important Range Rule](#28--important-range-rule)
29. [Time Complexity](#29--time-complexity)
30. [Common Mistakes](#30--common-mistakes)
31. [Interview Traps](#31--interview-traps)
32. [Top 20 Interview Questions](#32--top-20-interview-questions)
33. [30-Second Interview Answer](#33--30-second-interview-answer)
34. [Cheat Sheet](#34--cheat-sheet)
35. [Memory Tricks](#35--memory-tricks)
36. [Final Revision Checklist](#36--final-revision-checklist)

---

# 1. 🔹 What is Arrays Class?

Java provides a utility class called:

    java.util.Arrays

It contains many useful `static` methods for manipulating arrays.

Example:

    import java.util.Arrays;

    public class Main {

        public static void main(String[] args) {

            int[] arr = {5, 2, 8, 1, 3};

            Arrays.sort(arr);

            System.out.println(Arrays.toString(arr));
        }
    }

Output:

    [1, 2, 3, 5, 8]

Instead of manually implementing common operations, we can use the methods provided by `Arrays`.

---

# 2. 🤔 Why Do We Need Arrays Class?

A Java array itself provides only a small set of direct functionality.

For example:

    arr.length

gives the number of elements.

But if we want to:

- Sort an array
- Search an array
- Copy an array
- Compare arrays
- Fill an array
- Print array contents
- Convert an array into a Stream

we can use the `Arrays` class.

| Requirement | Method |
|---|---|
| Print 1D array | `Arrays.toString()` |
| Print nested array | `Arrays.deepToString()` |
| Sort | `Arrays.sort()` |
| Parallel sort | `Arrays.parallelSort()` |
| Search | `Arrays.binarySearch()` |
| Copy | `Arrays.copyOf()` |
| Copy range | `Arrays.copyOfRange()` |
| Fill | `Arrays.fill()` |
| Compare 1D arrays | `Arrays.equals()` |
| Compare nested arrays | `Arrays.deepEquals()` |
| Lexicographical comparison | `Arrays.compare()` |
| Find first mismatch | `Arrays.mismatch()` |
| Convert object array to List | `Arrays.asList()` |
| Create Stream | `Arrays.stream()` |
| Generate hash | `Arrays.hashCode()` |

---

# 3. 📦 Importing Arrays

`Arrays` belongs to:

    java.util

Therefore:

    import java.util.Arrays;

Then we can use:

    Arrays.sort(arr);

Alternative:

    import java.util.*;

This also imports `Arrays`.

---

# 4. 🛠️ Arrays as a Utility Class

`Arrays` is a utility class.

Its commonly used methods are `static`.

Therefore, we call them using the class name:

    Arrays.sort(arr);

We normally do NOT create an `Arrays` object.

Think:

    Arrays
       ↓
    Utility Class
       ↓
    Static Methods
       ↓
    ClassName.method()

Example:

    Arrays.sort(arr);
    Arrays.fill(arr, 5);
    Arrays.equals(a, b);

---

# 5. 🔥 Important Methods

The most important `Arrays` methods for Core Java and interviews are:

    Arrays.toString()
    Arrays.deepToString()

    Arrays.sort()
    Arrays.parallelSort()

    Arrays.binarySearch()

    Arrays.copyOf()
    Arrays.copyOfRange()

    Arrays.fill()

    Arrays.equals()
    Arrays.deepEquals()

    Arrays.compare()
    Arrays.compareUnsigned()

    Arrays.mismatch()

    Arrays.asList()

    Arrays.stream()

    Arrays.hashCode()
    Arrays.deepHashCode()

---

# 6. 🖨️ Arrays.toString()

`Arrays.toString()` returns a readable string representation of a one-dimensional array.

Example:

    int[] arr = {10, 20, 30};

    System.out.println(Arrays.toString(arr));

Output:

    [10, 20, 30]

---

## Why Do We Need It?

If we directly print an array:

    int[] arr = {10, 20, 30};

    System.out.println(arr);

we do not normally get:

    [10, 20, 30]

Instead, we may get something similar to:

    [I@5acf9800

The exact output is implementation-dependent, but it represents the array object's default object-style string representation.

`Arrays.toString()` gives us a readable representation of the elements.

---

## Syntax

    Arrays.toString(array);

---

## Example

    int[] nums = {5, 10, 15};

    String result = Arrays.toString(nums);

    System.out.println(result);

Output:

    [5, 10, 15]

---

## Works With Primitive Arrays

It has overloads for primitive array types such as:

    byte[]
    short[]
    int[]
    long[]
    float[]
    double[]
    char[]
    boolean[]

Example:

    char[] chars = {'J', 'a', 'v', 'a'};

    System.out.println(Arrays.toString(chars));

Output:

    [J, a, v, a]

---

# 7. 🌳 Arrays.deepToString()

`Arrays.deepToString()` is useful for arrays containing nested arrays.

Most commonly, we use it with multidimensional arrays.

Example:

    int[][] arr = {
        {1, 2, 3},
        {4, 5, 6}
    };

    System.out.println(Arrays.deepToString(arr));

Output:

    [[1, 2, 3], [4, 5, 6]]

---

## `toString()` vs `deepToString()`

For a 1D array:

    int[] arr = {1, 2, 3};

    Arrays.toString(arr);

For a nested array:

    int[][] arr = {
        {1, 2},
        {3, 4}
    };

    Arrays.deepToString(arr);

### Memory Trick

    1D array       → toString()
    Nested arrays  → deepToString()

---

# 8. 🔃 Arrays.sort()

`Arrays.sort()` sorts an array in ascending order.

Example:

    int[] arr = {5, 2, 8, 1, 3};

    Arrays.sort(arr);

    System.out.println(Arrays.toString(arr));

Output:

    [1, 2, 3, 5, 8]

---

## Important

`Arrays.sort()` modifies the original array.

Example:

    int[] arr = {5, 2, 8};

    Arrays.sort(arr);

After sorting:

    arr = [2, 5, 8]

It does not normally create a separate sorted array for you.

---

## Sorting String Array

For objects such as strings, natural ordering is used when applicable.

Example:

    String[] names = {"Charlie", "Alice", "Bob"};

    Arrays.sort(names);

Result:

    [Alice, Bob, Charlie]

---

# 9. 🎯 Sorting a Range

We can sort only a specific portion of an array.

Syntax:

    Arrays.sort(array, fromIndex, toIndex);

Important:

    fromIndex → inclusive
    toIndex   → exclusive

Example:

    int[] arr = {9, 7, 5, 3, 1};

    Arrays.sort(arr, 1, 4);

Indexes:

    Index:  0   1   2   3   4
    Value:  9   7   5   3   1

The selected range is:

    index 1
    index 2
    index 3

So:

    7, 5, 3

gets sorted.

Result:

    [9, 3, 5, 7, 1]

---

# 10. ⚡ Arrays.parallelSort()

Java also provides:

    Arrays.parallelSort()

It is designed to use parallelism for sorting when beneficial.

Example:

    int[] arr = {5, 2, 8, 1, 3};

    Arrays.parallelSort(arr);

    System.out.println(Arrays.toString(arr));

Output:

    [1, 2, 3, 5, 8]

For normal DSA problems, `Arrays.sort()` is generally the method you will use most often.

---

# 11. 🔎 Arrays.binarySearch()

`Arrays.binarySearch()` searches for an element in a sorted array.

Example:

    int[] arr = {10, 20, 30, 40, 50};

    int index = Arrays.binarySearch(arr, 30);

    System.out.println(index);

Output:

    2

Because:

    Index:  0   1   2   3   4
    Value: 10  20  30  40  50
                    ↑

---

## ⚠️ Array Should Be Sorted

Before using binary search, the array should be sorted according to the required ordering.

Correct:

    int[] arr = {10, 20, 30, 40, 50};

    Arrays.binarySearch(arr, 30);

Do NOT assume binary search works correctly on an arbitrary unsorted array.

---

## If Element Is Found

A non-negative index is returned.

Example:

    Arrays.binarySearch(arr, 30);

Result:

    2

---

## If Element Is Not Found

A negative value is returned.

The exact value follows the API's insertion-point convention.

For interview memory:

    Found      → non-negative index
    Not found  → negative value

---

## Complexity

Binary search:

    O(log n)

---

# 12. 📋 Arrays.copyOf()

`Arrays.copyOf()` creates a new array containing elements from the original array.

Example:

    int[] arr = {10, 20, 30};

    int[] copy = Arrays.copyOf(arr, arr.length);

    System.out.println(Arrays.toString(copy));

Output:

    [10, 20, 30]

Conceptually:

    arr  ─────► [10, 20, 30]

    copy ─────► [10, 20, 30]

These are different array objects.

Therefore:

    arr == copy

returns:

    false

---

## Copy With Larger Size

    int[] arr = {10, 20, 30};

    int[] copy = Arrays.copyOf(arr, 5);

Result:

    [10, 20, 30, 0, 0]

The additional positions receive the default value of the component type.

---

## Copy With Smaller Size

    int[] arr = {10, 20, 30, 40, 50};

    int[] copy = Arrays.copyOf(arr, 3);

Result:

    [10, 20, 30]

---

# 13. ✂️ Arrays.copyOfRange()

`copyOfRange()` copies a specific range.

Syntax:

    Arrays.copyOfRange(array, from, to);

Again:

    from → inclusive
    to   → exclusive

Example:

    int[] arr = {10, 20, 30, 40, 50};

    int[] copy = Arrays.copyOfRange(arr, 1, 4);

Result:

    [20, 30, 40]

Because:

    index 1 → included
    index 2 → included
    index 3 → included
    index 4 → excluded

---

## Visual

    Index:  0   1   2   3   4
    Value: 10  20  30  40  50
                └──────────┘
                1 to 4

Remember:

    from = included
    to   = excluded

---

# 14. 🪣 Arrays.fill()

`Arrays.fill()` fills an entire array with a specified value.

Example:

    int[] arr = new int[5];

    Arrays.fill(arr, 7);

    System.out.println(Arrays.toString(arr));

Output:

    [7, 7, 7, 7, 7]

---

## Fill a Range

Syntax:

    Arrays.fill(array, fromIndex, toIndex, value);

Example:

    int[] arr = {1, 2, 3, 4, 5};

    Arrays.fill(arr, 1, 4, 100);

Result:

    [1, 100, 100, 100, 5]

Because:

    1 → included
    4 → excluded

---

# 15. ⚖️ Arrays.equals()

`Arrays.equals()` compares two one-dimensional arrays element by element.

Example:

    int[] a = {1, 2, 3};
    int[] b = {1, 2, 3};

    System.out.println(Arrays.equals(a, b));

Output:

    true

---

## Why Not `==`?

Consider:

    int[] a = {1, 2, 3};
    int[] b = {1, 2, 3};

    System.out.println(a == b);

Output:

    false

Because `a` and `b` refer to different array objects.

But:

    Arrays.equals(a, b)

compares their elements.

Therefore:

    a == b

asks:

    "Are these the same object?"

while:

    Arrays.equals(a, b)

asks:

    "Do these arrays contain equal elements in the same order?"

---

# 16. 🌳 Arrays.deepEquals()

`Arrays.deepEquals()` compares nested arrays recursively.

Example:

    int[][] a = {
        {1, 2},
        {3, 4}
    };

    int[][] b = {
        {1, 2},
        {3, 4}
    };

    System.out.println(Arrays.deepEquals(a, b));

Output:

    true

---

## `equals()` vs `deepEquals()`

For normal 1D arrays:

    Arrays.equals(a, b);

For nested arrays:

    Arrays.deepEquals(a, b);

### Memory Trick

    1D      → equals()
    Nested  → deepEquals()

---

# 17. 🆚 Arrays.compare()

`Arrays.compare()` performs a lexicographical comparison.

Example:

    int[] a = {1, 2, 3};
    int[] b = {1, 2, 3};

    System.out.println(Arrays.compare(a, b));

Output:

    0

Meaning:

    Arrays are equal according to lexicographical comparison.

---

## Return Value

Generally:

    0
    ↓
    Arrays are equal

    Negative
    ↓
    First array is lexicographically smaller

    Positive
    ↓
    First array is lexicographically greater

Example:

    int[] a = {1, 2};
    int[] b = {1, 3};

    Arrays.compare(a, b);

At the first different position:

    2 < 3

Therefore the result is negative.

---

## What Does Lexicographical Mean?

It is similar to dictionary ordering.

Compare from left to right.

Example:

    [1, 2, 5]
    [1, 3, 0]

First:

    1 == 1

Then:

    2 < 3

Therefore:

    [1,2,5] < [1,3,0]

---

# 18. 🔢 Arrays.compareUnsigned()

For integral primitive arrays, Java also provides:

    Arrays.compareUnsigned()

It compares integer values as if they were unsigned.

This matters particularly for types such as:

    byte
    short
    int
    long

where Java's primitive integer types are normally signed.

For normal beginner-level array problems, you will rarely need this method.

But it is useful to know that `Arrays` provides both:

    Arrays.compare()

and:

    Arrays.compareUnsigned()

---

# 19. 🔍 Arrays.mismatch()

`Arrays.mismatch()` finds the first index where two arrays differ.

Example:

    int[] a = {10, 20, 30, 40};
    int[] b = {10, 20, 99, 40};

    System.out.println(Arrays.mismatch(a, b));

Output:

    2

Because:

    Index:  0    1    2    3
    A:     10   20   30   40
    B:     10   20   99   40
                       ↑
                    mismatch

---

## If Arrays Are Equal

Example:

    int[] a = {1, 2, 3};
    int[] b = {1, 2, 3};

    Arrays.mismatch(a, b);

returns:

    -1

Meaning:

    No mismatch found.

---

# 20. 📋 Arrays.asList()

`Arrays.asList()` converts an array into a `List` view for reference-type arrays.

Example:

    String[] names = {"A", "B", "C"};

    List<String> list = Arrays.asList(names);

Now:

    list = [A, B, C]

---

## Important Restriction

`Arrays.asList()` works with arrays of reference types.

For example:

    String[] names = {"A", "B", "C"};

    List<String> list = Arrays.asList(names);

works as expected.

But:

    int[] nums = {1, 2, 3};

    Arrays.asList(nums);

does NOT create:

    List<Integer>

Instead, because `int[]` itself is one object, it is treated as a single argument to the varargs parameter.

This is a famous interview trap.

---

## Example

    int[] nums = {1, 2, 3};

    System.out.println(Arrays.asList(nums));

This does not produce a normal `List<Integer>` containing three integers.

---

## With Integer[]

Use:

    Integer[] nums = {1, 2, 3};

    List<Integer> list = Arrays.asList(nums);

Now:

    [1, 2, 3]

---

## Is Arrays.asList() Mutable?

The returned list is a fixed-size list backed by the array.

You can use:

    list.set(0, 100);

But operations that change the size, such as:

    list.add(100);

or:

    list.remove(0);

throw `UnsupportedOperationException`.

---

## Backed by Original Array

Example:

    String[] arr = {"A", "B", "C"};

    List<String> list = Arrays.asList(arr);

    arr[0] = "X";

Now:

    list

also reflects:

    [X, B, C]

because the list is backed by the original array.

---

# 21. 🌊 Arrays.stream()

`Arrays.stream()` creates a Java Stream from an array.

Example:

    int[] nums = {1, 2, 3, 4, 5};

    Arrays.stream(nums)
          .forEach(System.out::println);

Output:

    1
    2
    3
    4
    5

---

## Sum Example

    int[] nums = {10, 20, 30};

    int sum = Arrays.stream(nums).sum();

    System.out.println(sum);

Output:

    60

---

## Important

For primitive arrays:

    int[]     → IntStream
    long[]    → LongStream
    double[]  → DoubleStream

For object arrays:

    String[]  → Stream<String>

Example:

    String[] names = {"A", "B", "C"};

    Arrays.stream(names)
          .forEach(System.out::println);

---

# 22. #️⃣ Arrays.hashCode()

`Arrays.hashCode()` calculates a hash code based on the contents of a one-dimensional array.

Example:

    int[] a = {1, 2, 3};

    int hash = Arrays.hashCode(a);

It considers the array elements when calculating the result.

This is useful when implementing content-based hashing behavior.

---

## Important

Do not confuse:

    Arrays.hashCode(arr)

with:

    arr.hashCode()

For arrays, `arr.hashCode()` does not provide a content-based array hash in the same way.

`Arrays.hashCode()` is specifically designed for content-based hashing of arrays.

---

# 23. 🌳 Arrays.deepHashCode()

For nested arrays:

    Arrays.deepHashCode()

can be used.

Example:

    int[][] arr = {
        {1, 2},
        {3, 4}
    };

    int hash = Arrays.deepHashCode(arr);

It recursively considers nested array contents.

Memory trick:

    1D array
        ↓
    hashCode()

    Nested array
        ↓
    deepHashCode()

---

# 24. 👨‍💻 Arrays of Objects

`Arrays` methods also work with arrays containing objects.

Example:

    String[] names = {
        "Charlie",
        "Alice",
        "Bob"
    };

    Arrays.sort(names);

Result:

    [Alice, Bob, Charlie]

For objects, sorting depends on either:

- Natural ordering
- A provided `Comparator`

Example:

    Arrays.sort(names, Comparator.reverseOrder());

Result:

    [Charlie, Bob, Alice]

---

# 25. ⚖️ Primitive Arrays vs Object Arrays

There is an important distinction.

## Primitive Array

    int[] nums = {1, 2, 3};

Contains:

    int values

---

## Object Array

    Integer[] nums = {1, 2, 3};

Contains:

    Integer references

This distinction becomes especially important with:

    Arrays.asList()

For:

    Integer[]

you get:

    List<Integer>

For:

    int[]

you do not get:

    List<Integer>

---

# 26. 🧠 Shallow Copy and Arrays

Suppose we have:

    int[] a = {1, 2, 3};

    int[] b = Arrays.copyOf(a, a.length);

For a primitive array, the values are copied into the new array.

But for an object array:

    Student[] a = {
        student1,
        student2
    };

    Student[] b = Arrays.copyOf(a, a.length);

the new array contains copies of the references.

Conceptually:

    a ───────► [ref1][ref2]
                 │     │
                 ▼     ▼
              Student Student

    b ───────► [ref1][ref2]
                 │     │
                 ▼     ▼
              Student Student

So:

    a != b

but:

    a[0] == b[0]

can be true.

This is called a shallow copy.

---

# 27. 🆚 Arrays Class vs Array

Do not confuse these two.

## Array

Example:

    int[] arr = new int[5];

An array is an object that stores multiple elements.

---

## Arrays Class

Example:

    Arrays.sort(arr);

`Arrays` is a utility class containing static methods for operating on arrays.

### Simple Difference

    Array
      ↓
    Stores data

    Arrays
      ↓
    Provides utility operations

---

# 28. 📐 Important Range Rule

Several `Arrays` methods use:

    fromIndex
    toIndex

The rule is:

    fromIndex → inclusive
    toIndex   → exclusive

This appears in methods such as:

    Arrays.sort()
    Arrays.fill()
    Arrays.copyOfRange()

Example:

    Arrays.sort(arr, 2, 5);

means:

    index 2
    index 3
    index 4

are included.

Index `5` is excluded.

---

## 🧠 Memory Trick

Think:

    [from, to)

This is the standard half-open range.

---

# 29. ⏱️ Time Complexity

Exact implementation details can vary by Java version and overload, but the following are useful DSA-level expectations.

| Operation | Typical Complexity |
|---|---:|
| `Arrays.toString()` | O(n) |
| `Arrays.deepToString()` | O(n) relative to total elements |
| `Arrays.sort()` primitive | O(n log n) typical |
| `Arrays.binarySearch()` | O(log n) |
| `Arrays.copyOf()` | O(n) |
| `Arrays.copyOfRange()` | O(n) |
| `Arrays.fill()` | O(n) |
| `Arrays.equals()` | O(n) |
| `Arrays.deepEquals()` | O(n) relative to total elements |
| `Arrays.compare()` | O(n) |
| `Arrays.mismatch()` | O(n) worst case |
| `Arrays.asList()` | O(1) view creation |
| `Arrays.stream()` | O(1) stream creation |

---

# 30. ⚠️ Common Mistakes

## ❌ Mistake 1 — Forgetting the import

Wrong:

    Arrays.sort(arr);

without importing `Arrays` when no wildcard/import is otherwise available.

Correct:

    import java.util.Arrays;

---

## ❌ Mistake 2 — Using `==` to compare contents

Wrong:

    a == b

when you want element-wise comparison.

Use:

    Arrays.equals(a, b)

---

## ❌ Mistake 3 — Using `toString()` for nested arrays

For:

    int[][] arr

prefer:

    Arrays.deepToString(arr)

---

## ❌ Mistake 4 — Binary searching an unsorted array

Do not blindly use:

    Arrays.binarySearch(arr, target);

on an unsorted array.

---

## ❌ Mistake 5 — Forgetting range is exclusive

Remember:

    from → included
    to   → excluded

---

## ❌ Mistake 6 — Thinking `Arrays.copyOf()` returns the same array

It creates a new array object.

Therefore:

    original != copy

---

## ❌ Mistake 7 — Misunderstanding `Arrays.asList(int[])`

This:

    int[] nums = {1, 2, 3};

    Arrays.asList(nums);

does not create a `List<Integer>` containing three integers.

---

## ❌ Mistake 8 — Thinking `Arrays.asList()` is fully resizable

The returned list has fixed size.

This fails:

    list.add(10);

---

# 31. 🚨 Interview Traps

### Trap 1

What is the difference?

    arr == copy

vs

    Arrays.equals(arr, copy)

Answer:

    ==

checks whether the references point to the same array object.

    Arrays.equals()

checks element-by-element equality for one-dimensional arrays.

---

### Trap 2

What is the difference?

    Arrays.toString()
    Arrays.deepToString()

Answer:

    toString()
        ↓
    1D array

    deepToString()
        ↓
    nested arrays

---

### Trap 3

What is the difference?

    Arrays.equals()
    Arrays.deepEquals()

Answer:

    equals()
        ↓
    normal 1D arrays

    deepEquals()
        ↓
    nested arrays

---

### Trap 4

What does this do?

    int[] a = {1, 2, 3};

    int[] b = Arrays.copyOf(a, a.length);

Answer:

It creates a separate array containing the same primitive values.

---

### Trap 5

What does this do?

    int[] a = {1, 2, 3};

    int[] b = a;

Answer:

It copies the reference.

Both variables point to the same array.

---

### Trap 6

What happens here?

    int[] nums = {1, 2, 3};

    Arrays.asList(nums);

Answer:

It does not produce a `List<Integer>` containing `1, 2, 3`.

Because `int[]` is itself an object and primitive arrays do not become lists of wrapper objects through this call.

---

### Trap 7

What does `Arrays.binarySearch()` return if the element is not found?

A negative value based on the insertion point.

---

### Trap 8

What does this mean?

    Arrays.mismatch(a, b) == -1

It means there is no mismatch; the arrays are equal over their compared contents.

---

# 32. 🔥 Top 20 Interview Questions

## Q1. What is the Arrays class?

`Arrays` is a utility class in `java.util` that provides static methods for manipulating arrays.

---

## Q2. Is Arrays a class or an array?

`Arrays` is a class.

Example:

    java.util.Arrays

---

## Q3. Why are Arrays methods called using the class name?

Because the commonly used methods are static.

Example:

    Arrays.sort(arr);

---

## Q4. What does Arrays.toString() do?

It returns a readable string representation of a one-dimensional array.

---

## Q5. Why can't we simply print an array using System.out.println()?

Because arrays are objects and their default string representation does not normally display their elements.

`Arrays.toString()` provides a readable content representation.

---

## Q6. What is the difference between toString() and deepToString()?

    toString()
        → one-dimensional arrays

    deepToString()
        → nested arrays

---

## Q7. What does Arrays.sort() do?

It sorts the array into ascending order according to the applicable ordering.

---

## Q8. Does Arrays.sort() modify the original array?

Yes.

The array is sorted in place.

---

## Q9. What is Arrays.binarySearch()?

It searches a sorted array using binary search.

Typical complexity:

    O(log n)

---

## Q10. Why should the array be sorted before binarySearch()?

Binary search relies on ordered data.

---

## Q11. What does Arrays.copyOf() do?

It creates a new array with a specified length and copies elements from the original array.

---

## Q12. What does Arrays.copyOfRange() do?

It creates a new array containing elements from a specified range.

The start index is inclusive and the end index is exclusive.

---

## Q13. What does Arrays.fill() do?

It fills all or part of an array with a specified value.

---

## Q14. What is the difference between == and Arrays.equals()?

    ==
        → reference identity

    Arrays.equals()
        → element-wise equality for 1D arrays

---

## Q15. What is deepEquals()?

It recursively compares nested arrays.

---

## Q16. What is Arrays.compare()?

It performs lexicographical comparison of arrays.

---

## Q17. What is Arrays.mismatch()?

It returns the first index where two arrays differ.

If there is no mismatch, it returns:

    -1

---

## Q18. What is Arrays.asList()?

It creates a fixed-size List view backed by an array for reference-type arrays.

---

## Q19. What is the problem with Arrays.asList(int[])?

A primitive array such as `int[]` is treated as a single object rather than being converted into a `List<Integer>`.

---

## Q20. What is Arrays.stream()?

It creates a Stream from an array.

For example:

    Arrays.stream(intArray)

produces an `IntStream`.

---

# 33. 🎤 30-Second Interview Answer

> **`Arrays` is a utility class from `java.util` that provides static methods for common array operations. It can sort arrays using `sort()`, search sorted arrays using `binarySearch()`, copy arrays using `copyOf()` and `copyOfRange()`, fill arrays using `fill()`, compare arrays using `equals()` or `deepEquals()`, and create readable representations using `toString()` or `deepToString()`. It also provides methods such as `stream()`, `asList()`, `compare()`, and `mismatch()`.**

---

# 34. 🧾 Cheat Sheet

| Method | Purpose | Important Point |
|---|---|---|
| `toString()` | Print 1D array | Readable output |
| `deepToString()` | Print nested array | Recursive |
| `sort()` | Sort | Modifies array |
| `parallelSort()` | Parallel sorting | Uses parallelism when beneficial |
| `binarySearch()` | Search | Array should be sorted |
| `copyOf()` | Copy | Creates new array |
| `copyOfRange()` | Copy range | End exclusive |
| `fill()` | Fill | Modifies array |
| `equals()` | Compare 1D arrays | Element-wise |
| `deepEquals()` | Compare nested arrays | Recursive |
| `compare()` | Lexicographical comparison | Negative/0/positive |
| `compareUnsigned()` | Unsigned comparison | Integral arrays |
| `mismatch()` | First difference | `-1` means none |
| `asList()` | Array → List view | Reference arrays |
| `stream()` | Array → Stream | Useful with Java 8+ |
| `hashCode()` | Content-based hash | 1D |
| `deepHashCode()` | Deep hash | Nested |

---

# 35. 🧠 Memory Tricks

## 🔥 Printing

    toString()
        ↓
    1D

    deepToString()
        ↓
    Nested

---

## 🔥 Comparing

    equals()
        ↓
    1D

    deepEquals()
        ↓
    Nested

---

## 🔥 Copying

    copyOf()
        ↓
    Copy with length

    copyOfRange()
        ↓
    Copy selected range

---

## 🔥 Searching

    binarySearch()
        ↓
    Sorted array
        ↓
    O(log n)

---

## 🔥 Range

Always remember:

    [from, to)

Meaning:

    from → included
    to   → excluded

---

## 🔥 Assignment vs Copy

    b = a

means:

    Same array

while:

    b = Arrays.copyOf(a, a.length)

means:

    New array

---

# 36. ✅ Final Revision Checklist

Before moving to the next topic, make sure you can explain:

    [ ] What is java.util.Arrays?
    [ ] Why is Arrays called a utility class?
    [ ] Why are Arrays methods commonly static?
    [ ] Arrays.toString()
    [ ] Arrays.deepToString()
    [ ] Arrays.sort()
    [ ] Sorting a range
    [ ] Arrays.parallelSort()
    [ ] Arrays.binarySearch()
    [ ] Why binary search requires sorted data
    [ ] Arrays.copyOf()
    [ ] Arrays.copyOfRange()
    [ ] Arrays.fill()
    [ ] Arrays.equals()
    [ ] Arrays.deepEquals()
    [ ] Arrays.compare()
    [ ] Arrays.compareUnsigned()
    [ ] Arrays.mismatch()
    [ ] Arrays.asList()
    [ ] Why Arrays.asList(int[]) is tricky
    [ ] Arrays.stream()
    [ ] Arrays.hashCode()
    [ ] Arrays.deepHashCode()
    [ ] Primitive vs object arrays
    [ ] Shallow copying of object arrays
    [ ] Arrays Class vs array object
    [ ] Range is [from, to)
    [ ] Important time complexities

---

# 🏆 MASTER MEMORY CARD

    ┌──────────────────────────────────────────────────┐
    │                 java.util.Arrays                 │
    ├──────────────────────────────────────────────────┤
    │ toString()        → Print 1D array               │
    │ deepToString()    → Print nested array           │
    │ sort()            → Sort array                   │
    │ parallelSort()    → Parallel sorting             │
    │ binarySearch()    → Search sorted array          │
    │ copyOf()          → Copy with specified length   │
    │ copyOfRange()     → Copy [from, to)              │
    │ fill()            → Fill values                  │
    │ equals()          → Compare 1D arrays            │
    │ deepEquals()      → Compare nested arrays       │
    │ compare()         → Lexicographical comparison   │
    │ mismatch()        → First differing index       │
    │ asList()          → Array → fixed-size List view │
    │ stream()          → Array → Stream               │
    │ hashCode()        → Content-based hash           │
    │ deepHashCode()    → Deep content-based hash      │
    └──────────────────────────────────────────────────┘

---

# ⭐ ONE-LINE INTERVIEW DEFINITION

> **`java.util.Arrays` is a utility class containing static methods that provide common operations such as sorting, searching, copying, comparing, filling, hashing, and converting arrays into readable or other useful forms.**

---

# 🔗 ARRAY FOLDER PROGRESS

    05-Arrays/
    │
    ├── 01-Array-Introduction.md
    ├── 02-One-Dimensional-Array.md
    ├── 03-Multidimensional-Array.md
    ├── 04-Array-Memory.md
    ├── 05-Arrays-Class.md              ← YOU ARE HERE
    └── 06-Array-Interview-Questions.md

### 🚀 Next

    06-Array-Interview-Questions.md

This will combine the important concepts from the complete Arrays folder into interview-focused questions with answers and explanations.