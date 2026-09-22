# 🧠 Arrays — Interview Questions

> **A complete interview-focused revision of Java Arrays, covering fundamentals, memory, multidimensional arrays, `java.util.Arrays`, common traps, DSA patterns, and coding questions.**

---

# 📌 Table of Contents

1. [What is an Array?](#1--what-is-an-array)
2. [Why Are Arrays Used?](#2--why-are-arrays-used)
3. [Array Characteristics](#3--array-characteristics)
4. [Array Declaration](#4--array-declaration)
5. [Array Creation](#5--array-creation)
6. [Array Initialization](#6--array-initialization)
7. [Default Values](#7--default-values)
8. [Array Indexing](#8--array-indexing)
9. [Array Length](#9--array-length)
10. [Array Memory](#10--array-memory)
11. [Are Arrays Objects in Java?](#11--are-arrays-objects-in-java)
12. [Primitive vs Reference Arrays](#12--primitive-vs-reference-arrays)
13. [Array Assignment](#13--array-assignment)
14. [Copying Arrays](#14--copying-arrays)
15. [Shallow Copy](#15--shallow-copy)
16. [Multidimensional Arrays](#16--multidimensional-arrays)
17. [Jagged Arrays](#17--jagged-arrays)
18. [`Arrays` Class](#18--arrays-class)
19. [`Arrays.toString()`](#19--arraystostring)
20. [`Arrays.deepToString()`](#20--arraysdeeptostring)
21. [`Arrays.sort()`](#21--arrayssort)
22. [`Arrays.binarySearch()`](#22--arraysbinarysearch)
23. [`Arrays.copyOf()`](#23--arrayscopyof)
24. [`Arrays.copyOfRange()`](#24--arrayscopyofrange)
25. [`Arrays.fill()`](#25--arraysfill)
26. [`Arrays.equals()`](#26--arraysequals)
27. [`Arrays.deepEquals()`](#27--arraysdeepequals)
28. [`Arrays.asList()`](#28--arraysaslist)
29. [`Arrays.stream()`](#29--arraysstream)
30. [`==` vs `Arrays.equals()`](#30--vs-arraysequals)
31. [Array vs ArrayList](#31--array-vs-arraylist)
32. [Time Complexity](#32--time-complexity)
33. [Common Interview Traps](#33--common-interview-traps)
34. [Top Conceptual Questions](#34--top-conceptual-questions)
35. [Top Coding Questions](#35--top-coding-questions)
36. [Important DSA Patterns](#36--important-dsa-patterns)
37. [30-Second Interview Answer](#37--30-second-interview-answer)
38. [Cheat Sheet](#38--cheat-sheet)
39. [Final Revision Checklist](#39--final-revision-checklist)

---

# 1. 🔹 What is an Array?

An **array** is an object that stores a fixed number of elements of the same component type in indexed form.

Example:

```java
int[] nums = {10, 20, 30, 40};
```

Conceptually:

```text
nums
 ↓
[10, 20, 30, 40]
```

Each element has an index:

```text
Index:  0   1   2   3
Value: 10  20  30  40
```

The first element is at index `0`.

---

# 2. 🤔 Why Are Arrays Used?

Suppose we need to store marks of 5 students.

Without an array:

```java
int marks1 = 80;
int marks2 = 75;
int marks3 = 90;
int marks4 = 85;
int marks5 = 70;
```

With an array:

```java
int[] marks = {80, 75, 90, 85, 70};
```

Arrays allow us to:

- Store multiple values under one variable
- Access elements using indexes
- Iterate efficiently
- Perform DSA operations
- Represent tables and matrices
- Store collections of primitive values efficiently

---

# 3. ⭐ Array Characteristics

| Property | Array |
|---|---|
| Size | Fixed |
| Indexing | Zero-based |
| Elements | Same component type |
| Access | Fast |
| Random access | Yes |
| Primitive support | Yes |
| Can store objects | Yes |
| `length` | Available |
| Dynamic resizing | No |
| Is object | Yes |

---

# 4. 📝 Array Declaration

There are two commonly seen declaration styles.

## Style 1

```java
int[] arr;
```

## Style 2

```java
int arr[];
```

Both are valid.

Recommended style:

```java
int[] arr;
```

Why?

Because the type clearly appears as:

```text
int[]
```

---

# 5. 🏗️ Array Creation

Declaration does not create the array object.

Example:

```java
int[] arr;
```

At this point:

```text
arr
```

is only a reference variable.

We create the array using:

```java
arr = new int[5];
```

Now an array object containing 5 `int` elements is created.

Combined:

```java
int[] arr = new int[5];
```

---

# 6. ✍️ Array Initialization

We can initialize an array directly.

Example:

```java
int[] arr = {10, 20, 30, 40};
```

This creates an array containing four elements.

Another form:

```java
int[] arr = new int[]{10, 20, 30, 40};
```

Both represent an array containing:

```text
[10, 20, 30, 40]
```

---

# 7. 🟢 Default Values

When an array is created using `new`, its elements receive default values.

Example:

```java
int[] arr = new int[5];
```

Initially:

```text
[0, 0, 0, 0, 0]
```

Default values:

| Type | Default |
|---|---|
| `byte` | `0` |
| `short` | `0` |
| `int` | `0` |
| `long` | `0L` |
| `float` | `0.0f` |
| `double` | `0.0d` |
| `char` | `'\u0000'` |
| `boolean` | `false` |
| Reference | `null` |

Example:

```java
String[] names = new String[3];
```

Result conceptually:

```text
[null, null, null]
```

---

# 8. 🔢 Array Indexing

Java arrays use zero-based indexing.

Example:

```java
int[] arr = {10, 20, 30, 40};
```

Indexes:

```text
Index:  0   1   2   3
Value: 10  20  30  40
```

Access:

```java
arr[0]; // 10
arr[1]; // 20
arr[2]; // 30
arr[3]; // 40
```

---

## Why Does Index Start at 0?

The index represents an offset from the beginning of the array.

The first element has offset:

```text
0
```

The second:

```text
1
```

The third:

```text
2
```

This is a fundamental convention used throughout Java and many other languages.

---

# 9. 📏 Array Length

Arrays have a field called:

```text
length
```

Example:

```java
int[] arr = {10, 20, 30};

System.out.println(arr.length);
```

Output:

```text
3
```

Important:

```java
array.length
```

is a field.

It is NOT:

```java
array.length()
```

---

## Array vs String

Array:

```java
arr.length
```

String:

```java
str.length()
```

This is a common interview trap.

---

# 10. 🧠 Array Memory

When we write:

```java
int[] arr = new int[5];
```

Conceptually:

```text
Stack

┌───────────────┐
│ arr           │
│ reference ────┼──────────────┐
└───────────────┘              │
                               ▼
                              Heap

                         ┌───────────────┐
                         │ 0 │ 0 │ 0 │ 0 │ 0 │
                         └───────────────┘
```

The variable `arr` stores a reference to the array object.

The array object itself is created on the heap.

For interview purposes:

```text
Reference variable
        ↓
    points to
        ↓
Array object on heap
```

> **Interview note:** The exact physical memory layout is JVM-implementation dependent. The stack/heap model above is the standard conceptual model used for explaining Java memory.

---

# 11. 🧩 Are Arrays Objects in Java?

Yes.

Arrays are objects in Java.

Example:

```java
int[] arr = new int[5];
```

The array itself is an object.

This is why:

```java
arr.length
```

is available.

Also:

```java
arr instanceof Object
```

is valid.

Example:

```java
int[] arr = new int[5];

System.out.println(arr instanceof Object);
```

Output:

```text
true
```

---

# 12. ⚖️ Primitive vs Reference Arrays

## Primitive Array

```java
int[] nums = {10, 20, 30};
```

The array contains primitive `int` values.

---

## Reference Array

```java
String[] names = {"A", "B", "C"};
```

The array contains references to `String` objects.

Conceptually:

```text
names
  │
  ▼
[ref][ref][ref]
  │    │    │
  ▼    ▼    ▼
 "A"  "B"  "C"
```

---

# 13. 🔗 Array Assignment

Consider:

```java
int[] a = {10, 20, 30};

int[] b = a;
```

This does NOT create another array.

Both variables point to the same array.

Conceptually:

```text
a ─────┐
       │
       ▼
    [10,20,30]
       ▲
       │
b ─────┘
```

Therefore:

```java
a == b
```

returns:

```text
true
```

---

## Modifying Through `b`

```java
b[0] = 100;
```

Now `a` also becomes:

```text
[100, 20, 30]
```

because both references point to the same array.

---

# 14. 📋 Copying Arrays

If we want a separate array, we need to copy the elements.

One option:

```java
int[] a = {10, 20, 30};

int[] b = Arrays.copyOf(a, a.length);
```

Now:

```java
a != b
```

but:

```java
Arrays.equals(a, b)
```

is:

```text
true
```

---

## Other Ways to Copy

### `Arrays.copyOf()`

```java
int[] b = Arrays.copyOf(a, a.length);
```

### `System.arraycopy()`

```java
int[] b = new int[a.length];

System.arraycopy(a, 0, b, 0, a.length);
```

### `clone()`

```java
int[] b = a.clone();
```

### Manual Copy

```java
int[] b = new int[a.length];

for (int i = 0; i < a.length; i++) {
    b[i] = a[i];
}
```

---

# 15. 🧠 Shallow Copy

Shallow copying becomes important with arrays of objects.

Example:

```java
Student[] a = {student1, student2};

Student[] b = a.clone();
```

A new array object is created.

But the references to the students are copied.

Conceptually:

```text
a ─────► [ref1][ref2]
           │     │
           ▼     ▼
        Student Student

b ─────► [ref1][ref2]
           │     │
           ▼     ▼
        Student Student
```

Therefore:

```java
a != b
```

but:

```java
a[0] == b[0]
```

can be true.

The array is new, but the referenced objects are shared.

---

# 16. 🌳 Multidimensional Arrays

Java supports arrays of arrays.

Example:

```java
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6}
};
```

Conceptually:

```text
matrix
   │
   ▼
[ ref ][ ref ]
   │     │
   ▼     ▼
[1,2,3] [4,5,6]
```

Access:

```java
matrix[0][0]; // 1
matrix[0][1]; // 2
matrix[1][2]; // 6
```

---

# 17. 🪜 Jagged Arrays

Java does not require all rows of a multidimensional array to have the same length.

Example:

```java
int[][] arr = new int[3][];

arr[0] = new int[2];
arr[1] = new int[4];
arr[2] = new int[1];
```

Conceptually:

```text
row 0 → [0, 0]

row 1 → [0, 0, 0, 0]

row 2 → [0]
```

This is called a:

```text
Jagged Array
```

or:

```text
Ragged Array
```

---

# 18. 🧰 Arrays Class

Java provides:

```java
java.util.Arrays
```

It is a utility class containing many static methods for working with arrays.

Import:

```java
import java.util.Arrays;
```

Important methods:

```java
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
Arrays.mismatch()
Arrays.asList()
Arrays.stream()
Arrays.hashCode()
```

---

# 19. 🖨️ `Arrays.toString()`

Used to get a readable representation of a one-dimensional array.

Example:

```java
int[] arr = {10, 20, 30};

System.out.println(Arrays.toString(arr));
```

Output:

```text
[10, 20, 30]
```

---

# 20. 🌳 `Arrays.deepToString()`

Used for nested arrays.

Example:

```java
int[][] arr = {
    {1, 2},
    {3, 4}
};

System.out.println(Arrays.deepToString(arr));
```

Output:

```text
[[1, 2], [3, 4]]
```

Memory:

```text
1D array       → toString()
Nested arrays  → deepToString()
```

---

# 21. 🔃 `Arrays.sort()`

Sorts an array.

Example:

```java
int[] arr = {5, 2, 8, 1};

Arrays.sort(arr);
```

Result:

```text
[1, 2, 5, 8]
```

It modifies the original array.

For primitive arrays, the standard `Arrays.sort()` uses an optimized dual-pivot quicksort for many primitive types. For object arrays, Java uses a stable TimSort-based implementation.

---

# 22. 🔎 `Arrays.binarySearch()`

Searches a sorted array using binary search.

Example:

```java
int[] arr = {10, 20, 30, 40, 50};

int index = Arrays.binarySearch(arr, 30);
```

Result:

```text
2
```

Typical complexity:

```text
O(log n)
```

Important:

> The array should be sorted according to the required ordering before using binary search.

If the array is not appropriately sorted, the result is unspecified.

---

# 23. 📋 `Arrays.copyOf()`

Creates a new array with a specified length.

Example:

```java
int[] arr = {10, 20, 30};

int[] copy = Arrays.copyOf(arr, 5);
```

Result:

```text
[10, 20, 30, 0, 0]
```

If the requested size is smaller:

```java
int[] copy = Arrays.copyOf(arr, 2);
```

Result:

```text
[10, 20]
```

---

# 24. ✂️ `Arrays.copyOfRange()`

Copies a specific range.

Example:

```java
int[] arr = {10, 20, 30, 40, 50};

int[] copy = Arrays.copyOfRange(arr, 1, 4);
```

Result:

```text
[20, 30, 40]
```

Remember:

```text
from → inclusive
to   → exclusive
```

---

# 25. 🪣 `Arrays.fill()`

Fills all or part of an array.

Example:

```java
int[] arr = new int[5];

Arrays.fill(arr, 10);
```

Result:

```text
[10, 10, 10, 10, 10]
```

Range:

```java
int[] arr = {1, 2, 3, 4, 5};

Arrays.fill(arr, 1, 4, 100);
```

Result:

```text
[1, 100, 100, 100, 5]
```

The range is:

```text
from → inclusive
to   → exclusive
```

---

# 26. ⚖️ `Arrays.equals()`

Compares two one-dimensional arrays element by element.

Example:

```java
int[] a = {1, 2, 3};

int[] b = {1, 2, 3};

System.out.println(Arrays.equals(a, b));
```

Result:

```text
true
```

---

# 27. 🌳 `Arrays.deepEquals()`

Used for nested arrays.

Example:

```java
int[][] a = {
    {1, 2},
    {3, 4}
};

int[][] b = {
    {1, 2},
    {3, 4}
};

System.out.println(Arrays.deepEquals(a, b));
```

Result:

```text
true
```

---

# 28. 📦 `Arrays.asList()`

Converts an array of reference types into a fixed-size List view backed by the array.

Example:

```java
String[] arr = {"A", "B", "C"};

List<String> list = Arrays.asList(arr);
```

Result conceptually:

```text
[A, B, C]
```

Important:

```java
Arrays.asList()
```

does NOT create a normal resizable `ArrayList`.

This works:

```java
list.set(0, "X");
```

But this does not:

```java
list.add("D");
```

It throws:

```text
UnsupportedOperationException
```

The list is fixed-size, but changes made through `set()` are reflected in the backing array and vice versa.

---

## ⚠️ Primitive Array Trap

This:

```java
int[] nums = {1, 2, 3};

List<int[]> list = Arrays.asList(nums);
```

produces a list containing the entire primitive array as one element.

It does not produce:

```text
List<Integer>
```

For:

```java
Integer[] nums = {1, 2, 3};

List<Integer> list = Arrays.asList(nums);
```

this works as expected.

---

# 29. 🌊 `Arrays.stream()`

Creates a Stream from an array.

Example:

```java
int[] nums = {10, 20, 30};

int sum = Arrays.stream(nums).sum();
```

Result:

```text
60
```

For:

```text
int[]
```

the result is an:

```text
IntStream
```

For:

```text
long[]
```

the result is:

```text
LongStream
```

For:

```text
double[]
```

the result is:

```text
DoubleStream
```

---

# 30. 🆚 `==` vs `Arrays.equals()`

This is one of the most important array interview questions.

Example:

```java
int[] a = {1, 2, 3};

int[] b = {1, 2, 3};

System.out.println(a == b);
```

Output:

```text
false
```

Why?

Because they are different array objects.

But:

```java
System.out.println(Arrays.equals(a, b));
```

Output:

```text
true
```

Because their contents are equal.

## Remember

```text
==
```

checks:

```text
Same object/reference?
```

While:

```java
Arrays.equals()
```

checks:

```text
Same elements?
```

---

# 31. ⚔️ Array vs ArrayList

| Feature | Array | ArrayList |
|---|---|---|
| Size | Fixed | Dynamic |
| Primitive values | Yes | No, uses wrappers |
| Generics | No | Yes |
| `length` | Yes | No |
| `size()` | No | Yes |
| Random access | Fast | Fast |
| Resizing | Manual/new array | Automatic |
| Utility class | `Arrays` | `Collections` |
| Syntax | `int[]` | `ArrayList<Integer>` |

---

## Example

Array:

```java
int[] arr = new int[5];
```

ArrayList:

```java
ArrayList<Integer> list = new ArrayList<>();

list.add(10);
list.add(20);
```

Array:

```java
arr.length;
```

ArrayList:

```java
list.size();
```

---

# 32. ⏱️ Time Complexity

| Operation | Typical Complexity |
|---|---:|
| Access `arr[i]` | O(1) |
| Update `arr[i]` | O(1) |
| Search unsorted array | O(n) |
| Binary search | O(log n) |
| Insert at end with free space | O(1) |
| Insert in middle | O(n) |
| Delete from middle | O(n) |
| `Arrays.sort()` | O(n log n) typical |
| `Arrays.copyOf()` | O(n) |
| `Arrays.copyOfRange()` | O(n) |
| `Arrays.fill()` | O(n) |
| `Arrays.equals()` | O(n) |
| `Arrays.mismatch()` | O(n) worst case |

> **Important:** A Java array itself has fixed size, so "insert at end with free space" is not an operation on the array in the same way it is for a dynamic array such as `ArrayList`. In a raw array, insertion generally means writing to an existing unused position or creating/copying into another array.

---

# 33. 🚨 Common Interview Traps

## Trap 1 — `length` vs `length()`

Array:

```java
arr.length;
```

String:

```java
str.length();
```

ArrayList:

```java
list.size();
```

---

## Trap 2 — Array indexing

First element:

```java
arr[0];
```

Last element:

```java
arr[arr.length - 1];
```

---

## Trap 3 — Out of Bounds

If:

```java
int[] arr = new int[5];
```

valid indexes are:

```text
0, 1, 2, 3, 4
```

This is invalid:

```java
arr[5];
```

It throws:

```text
ArrayIndexOutOfBoundsException
```

---

## Trap 4 — Null Array

Example:

```java
int[] arr = null;

System.out.println(arr.length);
```

causes:

```text
NullPointerException
```

---

## Trap 5 — Assignment Is Not Copy

```java
int[] b = a;
```

does not create a new array.

Both point to the same object.

---

## Trap 6 — `Arrays.equals()` vs `==`

```java
==
```

checks reference identity.

```java
Arrays.equals()
```

checks contents.

---

## Trap 7 — `toString()` vs `deepToString()`

```text
toString()
    → 1D

deepToString()
    → nested
```

---

## Trap 8 — `Arrays.sort()` changes the array

It sorts the original array.

---

## Trap 9 — Binary search requires ordering

Do not blindly binary-search an unsorted array.

---

## Trap 10 — `Arrays.asList()` and primitive arrays

```java
int[]
```

does not become:

```text
List<Integer>
```

through `Arrays.asList()`.

---

# 34. 🔥 Top Conceptual Questions

## Q1. Is an array a primitive or object?

An array is an object in Java.

Even an array of primitives is itself an object.

---

## Q2. Can an array store different data types?

Normally, an array has a fixed component type.

For example:

```java
int[]
```

stores `int` values.

But an array whose component type is a common superclass/interface can hold compatible objects.

Example:

```java
Object[] arr = {
    10,
    "Java",
    3.14
};
```

Here, boxing occurs for the primitive numeric literals before they are stored as `Object` references.

---

## Q3. Is array size fixed?

Yes.

Once an array is created, its length cannot be changed.

If we need a different size, we create another array.

---

## Q4. Can we change an array's length?

No.

This is impossible:

```java
arr.length = 10;
```

`length` is not assignable.

---

## Q5. Can arrays contain objects?

Yes.

Example:

```java
String[] names = new String[5];
```

---

## Q6. Can arrays contain primitives?

Yes.

Example:

```java
int[] nums = new int[5];
```

---

## Q7. Why is array access O(1)?

Because the runtime can directly locate an element using the array reference and index.

Conceptually:

```text
address = base + index × element-size
```

The actual JVM representation is more complex, but the important DSA property is constant-time indexed access.

---

## Q8. What happens when an invalid index is accessed?

Java throws:

```text
ArrayIndexOutOfBoundsException
```

Example:

```java
int[] arr = new int[3];

arr[3];
```

Valid indexes:

```text
0, 1, 2
```

---

## Q9. What happens if the array reference is null?

Example:

```java
int[] arr = null;

System.out.println(arr.length);
```

Result:

```text
NullPointerException
```

---

## Q10. Can an array be empty?

Yes.

Example:

```java
int[] arr = new int[0];
```

Its length is:

```text
0
```

But the reference itself is not null.

---

# 35. 💻 Top Coding Questions

## Q1. Find the Maximum Element

Approach:

```text
Keep a variable max.

Traverse the array.

If current element > max,
update max.
```

Example:

```java
int[] arr = {10, 5, 30, 20};

int max = arr[0];

for (int i = 1; i < arr.length; i++) {
    if (arr[i] > max) {
        max = arr[i];
    }
}

System.out.println(max);
```

Result:

```text
30
```

Complexity:

```text
Time  → O(n)
Space → O(1)
```

---

## Q2. Find the Minimum Element

Approach:

```text
Start with first element.

Compare every other element.
```

Example:

```java
int[] arr = {10, 5, 30, 20};

int min = arr[0];

for (int i = 1; i < arr.length; i++) {
    if (arr[i] < min) {
        min = arr[i];
    }
}

System.out.println(min);
```

Result:

```text
5
```

---

## Q3. Find Sum of Array

Example:

```java
int[] arr = {10, 20, 30};

int sum = 0;

for (int i = 0; i < arr.length; i++) {
    sum += arr[i];
}

System.out.println(sum);
```

Result:

```text
60
```

Complexity:

```text
O(n)
```

---

## Q4. Reverse an Array

Example:

```java
int[] arr = {1, 2, 3, 4, 5};

int left = 0;
int right = arr.length - 1;

while (left < right) {
    int temp = arr[left];
    arr[left] = arr[right];
    arr[right] = temp;

    left++;
    right--;
}

System.out.println(Arrays.toString(arr));
```

Result:

```text
[5, 4, 3, 2, 1]
```

Complexity:

```text
Time  → O(n)
Space → O(1)
```

---

## Q5. Linear Search

Example:

```java
int[] arr = {10, 20, 30, 40};

int target = 30;

for (int i = 0; i < arr.length; i++) {
    if (arr[i] == target) {
        System.out.println(i);
        break;
    }
}
```

Result:

```text
2
```

Complexity:

```text
O(n)
```

---

## Q6. Count Occurrences

Example:

```java
int[] arr = {1, 2, 2, 3, 2};

int target = 2;
int count = 0;

for (int i = 0; i < arr.length; i++) {
    if (arr[i] == target) {
        count++;
    }
}

System.out.println(count);
```

Result:

```text
3
```

---

## Q7. Check if Array Is Sorted

Example:

```java
int[] arr = {1, 2, 3, 4, 5};

boolean sorted = true;

for (int i = 0; i < arr.length - 1; i++) {
    if (arr[i] > arr[i + 1]) {
        sorted = false;
        break;
    }
}

System.out.println(sorted);
```

If this happens for ascending order:

```text
arr[i] > arr[i + 1]
```

the array is not sorted.

Complexity:

```text
O(n)
```

---

## Q8. Find Second Largest Element

A common approach is to maintain:

```text
largest
secondLargest
```

Example:

```java
int[] arr = {10, 30, 20, 40};

int largest = Integer.MIN_VALUE;
int secondLargest = Integer.MIN_VALUE;

for (int num : arr) {
    if (num > largest) {
        secondLargest = largest;
        largest = num;
    } else if (num > secondLargest && num != largest) {
        secondLargest = num;
    }
}

System.out.println(secondLargest);
```

Expected result:

```text
30
```

Be careful with duplicate values and with the exact definition of "second largest" in the problem.

---

## Q9. Remove Duplicates From Sorted Array

This is a classic two-pointer problem.

Example:

```text
[1, 1, 2, 2, 3]
```

Goal:

```text
[1, 2, 3]
```

Use a write pointer.

General idea:

```text
read pointer
     ↓
scans array

write pointer
     ↓
places unique values
```

Example:

```java
int[] nums = {1, 1, 2, 2, 3};

int write = 1;

for (int read = 1; read < nums.length; read++) {
    if (nums[read] != nums[read - 1]) {
        nums[write] = nums[read];
        write++;
    }
}
```

Here:

```text
write
```

represents the length of the unique portion.

---

## Q10. Move Zeroes to End

Example:

```text
[0, 1, 0, 3, 12]
```

Goal:

```text
[1, 3, 12, 0, 0]
```

Common approach:

```text
Two pointers

Keep track of where the next non-zero
value should be placed.
```

Example:

```java
int[] nums = {0, 1, 0, 3, 12};

int write = 0;

for (int read = 0; read < nums.length; read++) {
    if (nums[read] != 0) {
        int temp = nums[write];
        nums[write] = nums[read];
        nums[read] = temp;

        write++;
    }
}
```

---

## Q11. Find Missing Number

Given:

```text
[0, 1, 3]
```

Numbers should be:

```text
0, 1, 2, 3
```

Missing:

```text
2
```

Possible approaches:

```text
Sum formula
XOR
```

XOR is especially useful because:

```text
x ^ x = 0
```

and:

```text
x ^ 0 = x
```

Example:

```java
int[] nums = {0, 1, 3};

int xor = nums.length;

for (int i = 0; i < nums.length; i++) {
    xor ^= i;
    xor ^= nums[i];
}

System.out.println(xor);
```

Result:

```text
2
```

---

## Q12. Find Single Number

Classic problem:

```text
[4, 1, 2, 1, 2]
```

Every number appears twice except one.

Use XOR:

```java
int[] nums = {4, 1, 2, 1, 2};

int result = 0;

for (int num : nums) {
    result = result ^ num;
}

System.out.println(result);
```

Why?

Because:

```text
x ^ x = 0
```

Therefore pairs cancel.

Remaining value:

```text
4
```

---

## Q13. Majority Element

A majority element appears more than:

```text
n / 2
```

times.

A famous O(n) time and O(1) space solution is:

```text
Boyer-Moore Voting Algorithm
```

Core idea:

```text
candidate
count
```

If count becomes zero:

```text
candidate = current element
```

If current equals candidate:

```text
count++
```

Otherwise:

```text
count--
```

Example:

```java
int[] nums = {2, 2, 1, 1, 1, 2, 2};

int candidate = 0;
int count = 0;

for (int num : nums) {
    if (count == 0) {
        candidate = num;
    }

    if (num == candidate) {
        count++;
    } else {
        count--;
    }
}

System.out.println(candidate);
```

> If the problem does not guarantee that a majority element exists, verify the candidate with a second pass.

---

## Q14. Best Time to Buy and Sell Stock

Given prices:

```text
[7, 1, 5, 3, 6, 4]
```

Maintain:

```text
minimum price seen so far
```

and:

```text
maximum profit
```

At every element:

```text
profit = currentPrice - minimumPrice
```

Then update maximum profit.

Example:

```java
int[] prices = {7, 1, 5, 3, 6, 4};

int minPrice = prices[0];
int maxProfit = 0;

for (int i = 1; i < prices.length; i++) {
    minPrice = Math.min(minPrice, prices[i]);

    int profit = prices[i] - minPrice;

    maxProfit = Math.max(maxProfit, profit);
}

System.out.println(maxProfit);
```

Complexity:

```text
Time  → O(n)
Space → O(1)
```

---

## Q15. Rotate Array

Example:

```text
[1, 2, 3, 4, 5]
```

Rotate right by 2:

```text
[4, 5, 1, 2, 3]
```

Common optimal approach:

```text
Reverse entire array

Reverse first k elements

Reverse remaining elements
```

Example:

```java
int[] nums = {1, 2, 3, 4, 5};

int k = 2;
k %= nums.length;

reverse(nums, 0, nums.length - 1);
reverse(nums, 0, k - 1);
reverse(nums, k, nums.length - 1);
```

Helper method:

```java
static void reverse(int[] nums, int left, int right) {
    while (left < right) {
        int temp = nums[left];
        nums[left] = nums[right];
        nums[right] = temp;

        left++;
        right--;
    }
}
```

---

# 36. 🧩 Important DSA Patterns

Learning arrays is not just about syntax.

Arrays are the foundation for many DSA patterns.

---

## Pattern 1 — Traversal

Basic:

```java
for (int i = 0; i < arr.length; i++) {
    // process arr[i]
}
```

Use when:

- Visiting every element
- Calculating sum
- Finding min/max
- Counting values

---

## Pattern 2 — Two Pointers

Example:

```java
int left = 0;
int right = arr.length - 1;
```

Useful for:

- Reversing
- Pair problems
- Sorted arrays
- Removing duplicates
- Partitioning

---

## Pattern 3 — Sliding Window

Maintain a window over a continuous portion of the array.

Useful for:

- Maximum subarray/window
- Minimum window
- Fixed-size window
- Subarray problems

---

## Pattern 4 — Prefix Sum

Create cumulative sums.

Example:

```text
arr = [2, 4, 3]

Prefix:
[2, 6, 9]
```

Useful for:

- Range sum
- Subarray calculations
- Query problems

Example:

```java
int[] arr = {2, 4, 3};

int[] prefix = new int[arr.length];

prefix[0] = arr[0];

for (int i = 1; i < arr.length; i++) {
    prefix[i] = prefix[i - 1] + arr[i];
}
```

---

## Pattern 5 — Hashing

Use:

```text
HashMap
HashSet
```

Useful for:

- Frequency
- Duplicates
- Two Sum
- Fast lookup

---

## Pattern 6 — Sorting

Sorting can simplify many problems.

Example:

```java
Arrays.sort(arr);
```

After sorting, problems involving:

- Duplicates
- Pairs
- Intervals
- Binary search

may become easier.

---

## Pattern 7 — Binary Search

Used when the search space has an appropriate ordering or monotonic property.

Basic array version:

```java
Arrays.binarySearch(arr, target);
```

But in DSA, you should also learn to implement binary search manually.

Example:

```java
int left = 0;
int right = arr.length - 1;

while (left <= right) {
    int mid = left + (right - left) / 2;

    if (arr[mid] == target) {
        return mid;
    } else if (arr[mid] < target) {
        left = mid + 1;
    } else {
        right = mid - 1;
    }
}
```

---

## Pattern 8 — Kadane's Algorithm

Used to find maximum subarray sum.

Example:

```text
[-2,1,-3,4,-1,2,1,-5,4]
```

Maximum subarray:

```text
[4,-1,2,1]
```

Sum:

```text
6
```

Typical complexity:

```text
O(n)
```

Example:

```java
int[] nums = {-2, 1, -3, 4, -1, 2, 1, -5, 4};

int currentSum = nums[0];
int maxSum = nums[0];

for (int i = 1; i < nums.length; i++) {
    currentSum = Math.max(nums[i], currentSum + nums[i]);
    maxSum = Math.max(maxSum, currentSum);
}

System.out.println(maxSum);
```

---

# 37. 🎤 30-Second Interview Answer

> **An array in Java is an object that stores a fixed number of elements of the same component type and provides zero-based indexed access. Arrays provide O(1) random access, but their size cannot be changed after creation. Java also provides the `java.util.Arrays` utility class for operations such as sorting, searching, copying, comparing, filling, and converting arrays to readable strings. Arrays are fundamental in DSA because they form the basis for patterns such as two pointers, sliding window, prefix sum, binary search, and many hashing problems.**

---

# 38. 🧾 Cheat Sheet

## Declaration

```java
int[] arr;
```

## Creation

```java
int[] arr = new int[5];
```

## Initialization

```java
int[] arr = {1, 2, 3};
```

## Access

```java
arr[0];
```

## Update

```java
arr[0] = 100;
```

## Length

```java
arr.length;
```

## Last Element

```java
arr[arr.length - 1];
```

## Loop

```java
for (int i = 0; i < arr.length; i++) {
    System.out.println(arr[i]);
}
```

## Enhanced For Loop

```java
for (int num : arr) {
    System.out.println(num);
}
```

---

## Arrays Utility

```java
Arrays.toString(arr);

Arrays.deepToString(arr);

Arrays.sort(arr);

Arrays.binarySearch(arr, target);

Arrays.copyOf(arr, length);

Arrays.copyOfRange(arr, from, to);

Arrays.fill(arr, value);

Arrays.equals(a, b);

Arrays.deepEquals(a, b);

Arrays.asList(objectArray);

Arrays.stream(arr);
```

---

# 39. ✅ Final Revision Checklist

Before considering the Arrays chapter complete, make sure you can answer all of these without looking at your notes.

## Fundamentals

```text
[ ] What is an array?

[ ] Why are arrays used?

[ ] How do you declare an array?

[ ] How do you create an array?

[ ] How do you initialize an array?

[ ] Why does indexing start from 0?

[ ] What is arr.length?

[ ] Is array length fixed?

[ ] Can an array store primitives?

[ ] Can an array store objects?

[ ] Are arrays objects in Java?
```

## Memory

```text
[ ] Where is the array object created?

[ ] What does the array variable store?

[ ] What happens when two references point to one array?

[ ] What is shallow copying?

[ ] How are object arrays different from primitive arrays?
```

## Multidimensional

```text
[ ] What is a 2D array?

[ ] What is an array of arrays?

[ ] What is a jagged array?

[ ] Can rows have different lengths?
```

## Arrays Class

```text
[ ] What is java.util.Arrays?

[ ] Why are its methods static?

[ ] Arrays.toString()

[ ] Arrays.deepToString()

[ ] Arrays.sort()

[ ] Arrays.binarySearch()

[ ] Arrays.copyOf()

[ ] Arrays.copyOfRange()

[ ] Arrays.fill()

[ ] Arrays.equals()

[ ] Arrays.deepEquals()

[ ] Arrays.asList()

[ ] Arrays.stream()
```

## Interview Traps

```text
[ ] arr.length vs str.length()

[ ] arr == other vs Arrays.equals()

[ ] toString vs deepToString

[ ] equals vs deepEquals

[ ] assignment vs copying

[ ] primitive array + Arrays.asList()

[ ] binarySearch on unsorted data

[ ] array index out of bounds

[ ] null array vs empty array
```

## DSA

```text
[ ] Linear search

[ ] Maximum

[ ] Minimum

[ ] Reverse array

[ ] Frequency

[ ] Second largest

[ ] Sorted check

[ ] Remove duplicates

[ ] Move zeroes

[ ] Missing number

[ ] Single number

[ ] Majority element

[ ] Best time to buy and sell stock

[ ] Rotate array

[ ] Two pointers

[ ] Sliding window

[ ] Prefix sum

[ ] Binary search

[ ] Kadane's algorithm
```

---

# 🏆 FINAL ARRAY MINDSET

When you see an array problem, don't immediately start coding.

First ask:

```text
1. Is the array sorted?

2. Do I need to find something?

3. Do I need frequency?

4. Can I use two pointers?

5. Can I use a sliding window?

6. Can sorting simplify the problem?

7. Can hashing give O(1) average lookup?

8. Can prefix sum help?

9. Can binary search help?

10. Can I solve it in O(n) instead of O(n²)?
```

---

# 🧠 ARRAY INTERVIEW FORMULA

```text
ARRAY PROBLEM
      │
      ├── Need simple traversal?
      │       └── O(n)
      │
      ├── Sorted?
      │       ├── Binary Search
      │       └── Two Pointers
      │
      ├── Need frequency?
      │       └── HashMap / HashSet
      │
      ├── Continuous subarray?
      │       └── Sliding Window / Prefix Sum
      │
      ├── Pair problem?
      │       └── Hashing / Two Pointers
      │
      ├── Maximum subarray?
      │       └── Kadane
      │
      └── Need rearrangement?
              └── Two Pointers / In-place techniques
```

---

# 🚀 ARRAY FOLDER COMPLETE

```text
05-Arrays/
│
├── 01-Array-Introduction.md
├── 02-One-Dimensional-Array.md
├── 03-Multidimensional-Array.md
├── 04-Array-Memory.md
├── 05-Arrays-Class.md
└── 06-Array-Interview-Questions.md  ← YOU ARE HERE
```

---

# ⭐ ONE-LINE REVISION

> **Array = fixed-size, same-type, zero-indexed collection with O(1) indexed access; master traversal, two pointers, sliding window, hashing, prefix sums, sorting, and binary search to solve array problems efficiently.**