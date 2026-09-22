````md
# 📚 Arrays — Introduction

> Arrays are one of the most fundamental data structures in Java and the foundation for many DSA techniques such as searching, sorting, two pointers, sliding window, prefix sum, and binary search.

---

## 📌 Table of Contents

1. [What is an Array?](#-what-is-an-array)
2. [Why Do We Need Arrays?](#-why-do-we-need-arrays)
3. [Real-World Example](#-real-world-example)
4. [Basic Syntax](#-basic-syntax)
5. [Creating an Array](#-creating-an-array)
6. [Initializing an Array](#-initializing-an-array)
7. [Declaration vs Creation vs Initialization](#-declaration-vs-creation-vs-initialization)
8. [Accessing Array Elements](#-accessing-array-elements)
9. [Array Indexing](#-array-indexing)
10. [Array Length](#-array-length)
11. [Default Values](#-default-values)
12. [How Arrays Work Internally](#-how-arrays-work-internally)
13. [Arrays and Heap Memory](#-arrays-and-heap-memory)
14. [Reference Variable](#-reference-variable)
15. [Fixed Size](#-fixed-size)
16. [Homogeneous Elements](#-homogeneous-elements)
17. [Types of Arrays](#-types-of-arrays)
18. [One-Dimensional Array](#-one-dimensional-array)
19. [Multidimensional Array](#-multidimensional-array)
20. [Jagged Array](#-jagged-array)
21. [Traversing an Array](#-traversing-an-array)
22. [For Loop vs Enhanced For Loop](#-for-loop-vs-enhanced-for-loop)
23. [Taking Array Input](#-taking-array-input)
24. [Printing an Array](#-printing-an-array)
25. [Array Object and Object Class](#-array-object-and-object-class)
26. [Important Array Properties](#-important-array-properties)
27. [Common Operations](#-common-array-operations)
28. [Time Complexity](#-time-complexity)
29. [Common Mistakes](#-common-mistakes)
30. [Interview Traps](#-interview-traps)
31. [Advantages](#-advantages)
32. [Disadvantages](#-disadvantages)
33. [Array vs Variable](#-array-vs-variable)
34. [Array vs ArrayList](#-array-vs-arraylist)
35. [DSA Patterns Related to Arrays](#-dsa-patterns-related-to-arrays)
36. [Important DSA Questions](#-important-dsa-questions)
37. [How to Think About Array Problems](#-how-to-think-about-array-problems)
38. [Top 10 Interview Questions](#-top-10-interview-questions)
39. [30-Second Interview Answer](#-30-second-interview-answer)
40. [Cheat Sheet](#-cheat-sheet)

---

# 🔹 What is an Array?

An **array** is an object in Java that stores a fixed number of elements of the same component type.

Each element can be accessed using an integer index.

```java
int[] numbers = {10, 20, 30, 40, 50};
````

Here:

* `numbers` → reference variable
* `int[]` → array type
* `10, 20, 30, 40, 50` → elements
* `0, 1, 2, 3, 4` → indexes
* `5` → length

### Simple Definition

> An array is a fixed-size, indexed collection of elements having the same component type.

### Interview Definition

> In Java, an array is an object that contains a fixed number of components of the same type, with each component accessed using a zero-based integer index.

---

# 🔹 Why Do We Need Arrays?

Suppose we want to store marks of five students.

Without an array:

```java
int marks1 = 80;
int marks2 = 75;
int marks3 = 90;
int marks4 = 85;
int marks5 = 70;
```

This becomes difficult to manage as the amount of data increases.

Using an array:

```java
int[] marks = {80, 75, 90, 85, 70};
```

Now we can process all values using a loop:

```java
for (int i = 0; i < marks.length; i++) {
    System.out.println(marks[i]);
}
```

### Arrays are useful because they provide:

* Multiple values under one reference
* Indexed access
* Efficient traversal
* Easy searching
* Easy sorting
* A foundation for many DSA algorithms

---

# 🔹 Real-World Example

Consider marks of five students:

```text
Student       Marks
-------------------
Student 1      80
Student 2      75
Student 3      90
Student 4      85
Student 5      70
```

We can represent them as:

```text
Index:    0    1    2    3    4
          ↓    ↓    ↓    ↓    ↓
Marks:   80   75   90   85   70
```

So:

```java
marks[0] → 80
marks[1] → 75
marks[2] → 90
marks[3] → 85
marks[4] → 70
```

---

# 🔹 Basic Syntax

## Declaration

```java
int[] arr;
```

Another valid syntax:

```java
int arr[];
```

Preferred Java style:

```java
int[] arr;
```

---

## Creation

```java
arr = new int[5];
```

This creates an integer array capable of storing five elements.

---

## Declaration + Creation

```java
int[] arr = new int[5];
```

---

## Declaration + Initialization

```java
int[] arr = {10, 20, 30, 40, 50};
```

---

# 🔹 Creating an Array

The `new` keyword creates the array object.

```java
int[] arr = new int[5];
```

Conceptually:

```text
Stack                         Heap
-----                         ----
arr  --------------------->  int[5]
                              ↓
                         [0, 0, 0, 0, 0]
```

The reference variable `arr` refers to the array object.

---

# 🔹 Initializing an Array

We can initialize an array directly:

```java
int[] arr = {10, 20, 30, 40, 50};
```

Java determines the size automatically.

```java
System.out.println(arr.length);
```

Output:

```text
5
```

We can also initialize individual elements:

```java
int[] arr = new int[5];

arr[0] = 10;
arr[1] = 20;
arr[2] = 30;
arr[3] = 40;
arr[4] = 50;
```

---

# 🔹 Declaration vs Creation vs Initialization

This distinction is very important in interviews.

## 1. Declaration

```java
int[] arr;
```

A reference variable is declared.

No array object has been created yet.

---

## 2. Creation

```java
arr = new int[5];
```

The array object is created.

---

## 3. Initialization

```java
arr[0] = 10;
arr[1] = 20;
```

Values are assigned to array components.

---

## Combined

```java
int[] arr = new int[5];
```

This performs:

```text
Declaration + Object Creation
```

While:

```java
int[] arr = {10, 20, 30, 40, 50};
```

performs:

```text
Declaration + Creation + Initialization
```

---

# 🔹 Accessing Array Elements

Array elements are accessed using their index.

```java
int[] arr = {10, 20, 30, 40, 50};

System.out.println(arr[0]);
System.out.println(arr[2]);
System.out.println(arr[4]);
```

Output:

```text
10
30
50
```

---

# 🔹 Array Indexing

Java arrays use **zero-based indexing**.

For:

```java
int[] arr = {10, 20, 30, 40, 50};
```

The structure is:

```text
Index:    0    1    2    3    4
          ↓    ↓    ↓    ↓    ↓
Value:   10   20   30   40   50
```

Therefore:

```java
arr[0] → 10
arr[1] → 20
arr[2] → 30
arr[3] → 40
arr[4] → 50
```

The last valid index is:

```java
arr.length - 1
```

---

# 🔹 Array Length

The `length` property gives the number of elements in an array.

```java
int[] arr = {10, 20, 30, 40, 50};

System.out.println(arr.length);
```

Output:

```text
5
```

### Important

For arrays:

```java
arr.length
```

For Strings:

```java
str.length()
```

For collections such as `ArrayList`:

```java
list.size()
```

Do not confuse them.

---

# 🔹 Default Values

When an array is created using `new`, its elements automatically receive default values.

```java
int[] arr = new int[5];
```

Initially:

```text
[0, 0, 0, 0, 0]
```

### Default Values

| Data Type      | Default Value |
| -------------- | ------------- |
| `byte`         | `0`           |
| `short`        | `0`           |
| `int`          | `0`           |
| `long`         | `0L`          |
| `float`        | `0.0f`        |
| `double`       | `0.0d`        |
| `char`         | `'\u0000'`    |
| `boolean`      | `false`       |
| Reference type | `null`        |

Example:

```java
String[] names = new String[3];

System.out.println(names[0]);
```

Output:

```text
null
```

---

# 🔹 How Arrays Work Internally

When we write:

```java
int[] arr = new int[5];
```

Java performs several conceptual steps.

```text
1. Reference variable is declared
          ↓
2. Array object is created
          ↓
3. Memory is allocated for 5 int components
          ↓
4. Components receive default values
          ↓
5. Reference points to the array object
```

Conceptually:

```text
Stack
┌──────────────┐
│ arr          │
│ reference ─────────────┐
└──────────────┘         │
                         ↓
                    Heap
                ┌───────────────┐
                │ Array Object  │
                ├───────────────┤
                │ 0 │ 0 │ 0 │ 0 │ 0 │
                └───────────────┘
```

> Exact JVM implementation details are JVM-dependent, but Java arrays are objects and are normally allocated in heap memory.

---

# 🔹 Arrays and Heap Memory

Arrays are objects.

Therefore:

```java
int[] arr = new int[5];
```

creates an array object.

The object is stored in heap memory.

The variable:

```java
arr
```

holds a reference to that object.

### Important Interview Point

```text
Array object → Heap
Reference variable → depends on where the variable itself is declared
```

For example, a local reference variable is associated with a stack frame, while the array object itself is allocated in the heap.

---

# 🔹 Reference Variable

Consider:

```java
int[] arr = new int[3];
```

`arr` is **not the array itself**.

It is a reference variable that refers to the array object.

```text
arr
 ↓
[0, 0, 0]
```

Another reference can point to the same array:

```java
int[] arr1 = {10, 20, 30};

int[] arr2 = arr1;

arr2[0] = 100;

System.out.println(arr1[0]);
```

Output:

```text
100
```

Why?

Because both references point to the same array object.

```text
arr1 ─────┐
          ↓
       [100, 20, 30]
          ↑
arr2 ─────┘
```

---

# 🔹 Fixed Size

An array has a fixed length.

```java
int[] arr = new int[5];
```

Its length is permanently:

```text
5
```

You cannot increase it to 10 directly.

This is invalid:

```java
arr.length = 10;    // ❌
```

If a larger array is required, a new array must be created.

```java
int[] oldArr = {10, 20, 30};

int[] newArr = new int[5];

for (int i = 0; i < oldArr.length; i++) {
    newArr[i] = oldArr[i];
}
```

This fixed-size property is one of the major differences between arrays and dynamic collections such as `ArrayList`.

---

# 🔹 Homogeneous Elements

A normal array stores elements of one component type.

```java
int[] numbers = {10, 20, 30};
```

All elements are `int`.

This is invalid:

```java
int[] numbers = {10, 20, "Hello"};  // ❌
```

However, reference-type arrays have an important concept called **array covariance**.

Example:

```java
Object[] arr = new String[3];

arr[0] = "Hello";
```

This is allowed because `String` is a subtype of `Object`.

But:

```java
arr[1] = 100;
```

causes:

```text
ArrayStoreException
```

because the actual array object is a `String[]`.

---

# 🔹 Types of Arrays

Arrays can be classified based on dimensions.

### 1. One-dimensional array

```java
int[] arr;
```

### 2. Two-dimensional array

```java
int[][] matrix;
```

### 3. Multidimensional array

```java
int[][][] cube;
```

Java technically supports arrays of arrays, which allows structures with multiple dimensions.

---

# 🔹 One-Dimensional Array

A one-dimensional array stores elements in a single sequence.

```java
int[] arr = {10, 20, 30, 40, 50};
```

Representation:

```text
[10] [20] [30] [40] [50]
  0    1    2    3    4
```

Example:

```java
for (int i = 0; i < arr.length; i++) {
    System.out.println(arr[i]);
}
```

---

# 🔹 Multidimensional Array

Java does not have a separate built-in matrix type.

A multidimensional array is essentially an **array whose elements are themselves arrays**.

Example:

```java
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};
```

Representation:

```text
        0  1  2
      ┌─────────
0     │ 1  2  3
1     │ 4  5  6
2     │ 7  8  9
```

Access:

```java
System.out.println(matrix[1][2]);
```

Output:

```text
6
```

---

# 🔹 Jagged Array

Since Java arrays are arrays of arrays, each inner array can have a different length.

```java
int[][] arr = new int[3][];

arr[0] = new int[2];
arr[1] = new int[4];
arr[2] = new int[3];
```

Representation:

```text
Row 0 → [0, 0]

Row 1 → [0, 0, 0, 0]

Row 2 → [0, 0, 0]
```

This is called a **jagged array**.

Example:

```java
int[][] arr = {
    {1, 2},
    {3, 4, 5, 6},
    {7, 8, 9}
};
```

---

# 🔹 Traversing an Array

Traversal means visiting each element of an array.

## Using Traditional For Loop

```java
int[] arr = {10, 20, 30, 40, 50};

for (int i = 0; i < arr.length; i++) {
    System.out.println(arr[i]);
}
```

Output:

```text
10
20
30
40
50
```

---

# 🔹 For Loop vs Enhanced For Loop

## Traditional For Loop

```java
for (int i = 0; i < arr.length; i++) {
    System.out.println(arr[i]);
}
```

Useful when:

* Index is required
* We need to modify elements
* We need to traverse partially
* We need to traverse backwards

Example:

```java
for (int i = arr.length - 1; i >= 0; i--) {
    System.out.println(arr[i]);
}
```

---

## Enhanced For Loop

```java
for (int value : arr) {
    System.out.println(value);
}
```

Useful when:

* Only values are required
* Index is not required
* Simple traversal is needed

---

# 🔹 Taking Array Input

Using `Scanner`:

```java
import java.util.Scanner;

public class ArrayInput {
    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        int n = sc.nextInt();

        int[] arr = new int[n];

        for (int i = 0; i < arr.length; i++) {
            arr[i] = sc.nextInt();
        }

        for (int value : arr) {
            System.out.println(value);
        }

        sc.close();
    }
}
```

Example Input:

```text
5
10 20 30 40 50
```

---

# 🔹 Printing an Array

This does **not** print array elements correctly:

```java
System.out.println(arr);
```

It generally prints a class-name/hash-style representation.

For example:

```text
[I@5e91993f
```

For primitive arrays, use:

```java
import java.util.Arrays;

System.out.println(Arrays.toString(arr));
```

Output:

```text
[10, 20, 30, 40, 50]
```

For multidimensional arrays:

```java
System.out.println(Arrays.deepToString(matrix));
```

---

# 🔹 Array Object and Object Class

Every Java array is an object.

Arrays implicitly inherit methods from `Object`.

For example:

```java
int[] arr = {10, 20, 30};

System.out.println(arr.getClass());
System.out.println(arr.toString());
System.out.println(arr.hashCode());
```

Arrays do not override methods like `toString()` to display their contents.

Therefore:

```java
System.out.println(arr);
```

does not produce:

```text
[10, 20, 30]
```

Use:

```java
Arrays.toString(arr);
```

instead.

---

# 🔹 Important Array Properties

Arrays have an important built-in property:

```java
arr.length
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

### Important

`length` is a property, not a method.

Correct:

```java
arr.length
```

Incorrect:

```java
arr.length()
```

---

# 🔹 Common Array Operations

| Operation     | Example           | Typical Complexity |
| ------------- | ----------------- | -----------------: |
| Access        | `arr[i]`          |               O(1) |
| Update        | `arr[i] = x`      |               O(1) |
| Traverse      | loop              |               O(n) |
| Linear Search | loop              |               O(n) |
| Binary Search | sorted array      |           O(log n) |
| Find Minimum  | loop              |               O(n) |
| Find Maximum  | loop              |               O(n) |
| Reverse       | two pointers      |               O(n) |
| Copy          | `Arrays.copyOf()` |               O(n) |

---

# 🔹 Time Complexity

Array access is generally:

```text
O(1)
```

because the JVM can directly access an element using its index.

For example:

```java
arr[500];
```

does not require visiting:

```text
arr[0]
arr[1]
arr[2]
...
arr[499]
```

The index is used to locate the required component directly.

### Traversal

```java
for (int i = 0; i < arr.length; i++) {
    System.out.println(arr[i]);
}
```

Time:

```text
O(n)
```

### Linear Search

```text
O(n)
```

### Binary Search

For a sorted array:

```text
O(log n)
```

---

# 🔹 Common Mistakes

## Mistake 1: Using Invalid Index

```java
int[] arr = {10, 20, 30};

System.out.println(arr[3]); // ❌
```

Valid indexes:

```text
0
1
2
```

Result:

```text
ArrayIndexOutOfBoundsException
```

---

## Mistake 2: Using `length()`

```java
arr.length(); // ❌
```

Correct:

```java
arr.length;
```

---

## Mistake 3: Confusing Array and ArrayList Size

Array:

```java
arr.length
```

ArrayList:

```java
list.size()
```

---

## Mistake 4: Assuming Array Can Grow

```java
int[] arr = new int[5];
```

The array length cannot be changed.

---

## Mistake 5: Printing Directly

```java
System.out.println(arr);
```

Use:

```java
System.out.println(Arrays.toString(arr));
```

---

## Mistake 6: Forgetting Zero-Based Indexing

For:

```java
int[] arr = new int[5];
```

The last index is:

```java
4
```

not:

```java
5
```

---

# 🔹 Interview Traps

### Trap 1: Is an array an object?

Yes.

```java
int[] arr = new int[5];
```

The array itself is an object.

---

### Trap 2: Can an array store primitive values?

Yes.

```java
int[] arr = {1, 2, 3};
```

The components are primitive `int` values.

The array object itself is still an object.

---

### Trap 3: Is array size dynamic?

No.

Once created:

```java
int[] arr = new int[5];
```

its length is fixed.

---

### Trap 4: Can arrays contain objects?

Yes.

```java
String[] names = {"A", "B", "C"};
```

The array contains references to `String` objects.

---

### Trap 5: What happens if the index is invalid?

An exception is thrown:

```text
ArrayIndexOutOfBoundsException
```

---

### Trap 6: Does `arr.length` return the last index?

No.

It returns the number of elements.

```java
int[] arr = new int[5];

arr.length      // 5
arr.length - 1  // 4 → last valid index
```

---

# 🔹 Advantages

### 1. Fast Random Access

```java
arr[index]
```

typically provides:

```text
O(1)
```

access.

### 2. Simple Structure

Arrays are easy to understand and use.

### 3. Memory Efficiency

Primitive arrays can store primitive values directly as components without wrapper objects.

### 4. Foundation of DSA

Many DSA problems are based on arrays.

Examples:

* Searching
* Sorting
* Prefix Sum
* Two Pointers
* Sliding Window
* Binary Search
* Kadane's Algorithm
* Frequency Counting

### 5. Predictable Size

The fixed size can be useful when the required number of elements is known.

---

# 🔹 Disadvantages

### 1. Fixed Size

Cannot grow or shrink after creation.

### 2. Insertion Can Be Expensive

Inserting into the middle may require shifting elements.

### 3. Deletion Can Be Expensive

Deleting from the middle may require shifting elements.

### 4. Limited Built-In Operations

Arrays provide basic indexing and length information, while collections provide many additional methods.

### 5. Homogeneous Components

A normal array has a single component type.

---

# 🔹 Array vs Variable

| Feature   | Variable          | Array                    |
| --------- | ----------------- | ------------------------ |
| Stores    | Usually one value | Multiple values          |
| Example   | `int x = 10`      | `int[] arr`              |
| Indexing  | No                | Yes                      |
| Size      | One value         | Fixed number of elements |
| Data Type | Declared type     | Component type           |
| DSA Usage | Limited           | Very high                |

---

# 🔹 Array vs ArrayList

| Feature                    | Array          | ArrayList            |
| -------------------------- | -------------- | -------------------- |
| Size                       | Fixed          | Dynamic              |
| Stores primitives directly | Yes            | No                   |
| Stores objects             | Yes            | Yes                  |
| Syntax                     | `int[]`        | `ArrayList<Integer>` |
| Access                     | `arr[i]`       | `list.get(i)`        |
| Length                     | `arr.length`   | `list.size()`        |
| Add element                | Not directly   | `add()`              |
| Remove element             | Not directly   | `remove()`           |
| Performance                | Lower overhead | More abstraction     |
| Generics                   | No             | Yes                  |

Example:

```java
int[] arr = new int[5];
```

vs

```java
ArrayList<Integer> list = new ArrayList<>();
```

---

# 🔹 DSA Patterns Related to Arrays

Arrays are extremely important in DSA.

## 1. Linear Traversal

Pattern:

```java
for (int i = 0; i < arr.length; i++) {
    // process arr[i]
}
```

Used for:

* Sum
* Minimum
* Maximum
* Counting
* Searching

---

## 2. Two Pointers

Typical structure:

```java
int left = 0;
int right = arr.length - 1;

while (left < right) {

    // process

    left++;
    right--;
}
```

Used for:

* Reverse array
* Two Sum in sorted array
* Pair problems
* Palindrome-like problems

---

## 3. Sliding Window

Used when dealing with contiguous subarrays.

Example pattern:

```java
int left = 0;

for (int right = 0; right < arr.length; right++) {

    // expand window

    while (/* condition */) {
        // shrink window
        left++;
    }
}
```

Used for:

* Maximum subarray with constraints
* Longest/shortest valid subarray
* Fixed-size windows

---

## 4. Prefix Sum

Used for efficient range-sum queries.

```java
int[] prefix = new int[arr.length];

prefix[0] = arr[0];

for (int i = 1; i < arr.length; i++) {
    prefix[i] = prefix[i - 1] + arr[i];
}
```

---

## 5. Frequency Counting

For values with a manageable range:

```java
int[] frequency = new int[10];

for (int value : arr) {
    frequency[value]++;
}
```

---

## 6. Binary Search

Works on a sorted array.

```java
int left = 0;
int right = arr.length - 1;

while (left <= right) {

    int mid = left + (right - left) / 2;

    if (arr[mid] == target) {
        return mid;
    }

    if (arr[mid] < target) {
        left = mid + 1;
    } else {
        right = mid - 1;
    }
}
```

Time complexity:

```text
O(log n)
```

---

# 🔹 Important DSA Questions

## 1. Find Maximum Element

```java
int max = arr[0];

for (int i = 1; i < arr.length; i++) {
    if (arr[i] > max) {
        max = arr[i];
    }
}
```

Time:

```text
O(n)
```

---

## 2. Find Minimum Element

```java
int min = arr[0];

for (int i = 1; i < arr.length; i++) {
    if (arr[i] < min) {
        min = arr[i];
    }
}
```

Time:

```text
O(n)
```

---

## 3. Reverse an Array

```java
int left = 0;
int right = arr.length - 1;

while (left < right) {

    int temp = arr[left];
    arr[left] = arr[right];
    arr[right] = temp;

    left++;
    right--;
}
```

Time:

```text
O(n)
```

Space:

```text
O(1)
```

---

## 4. Linear Search

```java
for (int i = 0; i < arr.length; i++) {

    if (arr[i] == target) {
        return i;
    }
}

return -1;
```

Time:

```text
O(n)
```

---

## 5. Find Sum

```java
int sum = 0;

for (int value : arr) {
    sum += value;
}
```

Time:

```text
O(n)
```

---

# 🔹 How to Think About Array Problems

When you see an array problem, ask these questions:

### Step 1 — What is the input?

```text
Array?
Sorted array?
Unsorted array?
Positive numbers?
Negative numbers?
Duplicates?
```

### Step 2 — What is being asked?

```text
Search?
Maximum?
Minimum?
Pair?
Subarray?
Subsequence?
Frequency?
Sorting?
Modification?
```

### Step 3 — Is the array sorted?

If yes, consider:

```text
Binary Search
Two Pointers
```

### Step 4 — Is the problem about a contiguous section?

Consider:

```text
Sliding Window
Prefix Sum
Kadane's Algorithm
```

### Step 5 — Is frequency involved?

Consider:

```text
HashMap
HashSet
Frequency Array
```

### Step 6 — Can we solve it in-place?

Ask:

> Do I really need another array?

If not, try to solve it using:

```text
O(1) extra space
```

---

# 🔹 Top 10 Interview Questions

### 1. What is an array in Java?

An array is an object that stores a fixed number of components of the same type and provides indexed access to them.

---

### 2. Is an array an object in Java?

Yes.

```java
int[] arr = new int[5];
```

The array itself is an object.

---

### 3. Where is an array stored?

The array object is allocated in heap memory under the normal Java memory model.

---

### 4. Is array size fixed?

Yes.

Once an array is created, its length cannot be changed.

---

### 5. What is the first index of an array?

```text
0
```

Java arrays use zero-based indexing.

---

### 6. What is the last valid index?

```java
arr.length - 1
```

---

### 7. What is the difference between `length` and `length()`?

For arrays:

```java
arr.length
```

For Strings:

```java
str.length()
```

---

### 8. What happens when an invalid index is accessed?

An:

```text
ArrayIndexOutOfBoundsException
```

is thrown.

---

### 9. Can an array store primitive values?

Yes.

```java
int[] arr = {10, 20, 30};
```

---

### 10. Can an array size be increased?

Not directly.

A new array must be created and elements copied if more capacity is required.

---

# 🔹 30-Second Interview Answer

> "An array in Java is an object used to store a fixed number of elements of the same component type. It uses zero-based indexing, so elements can be accessed using an integer index, generally in O(1) time. Arrays are created using the `new` keyword or an array initializer, and their length is fixed after creation. Arrays are widely used in DSA because they provide efficient random access and form the foundation for techniques such as searching, sorting, two pointers, sliding window, and prefix sums."

---

# 🔹 Cheat Sheet

```text
ARRAY
│
├── Object
│
├── Fixed Size
│
├── Same Component Type
│
├── Zero-Based Indexing
│
├── Random Access → O(1)
│
├── Length → arr.length
│
├── Last Index → arr.length - 1
│
├── Creation
│      └── new int[5]
│
├── Initialization
│      └── {10, 20, 30}
│
├── Traversal
│      ├── for loop
│      └── enhanced for loop
│
├── Dimensions
│      ├── 1D
│      ├── 2D
│      └── Multidimensional
│
└── DSA Patterns
       ├── Linear Traversal
       ├── Two Pointers
       ├── Sliding Window
       ├── Prefix Sum
       ├── Binary Search
       ├── Frequency Counting
       └── Kadane's Algorithm
```

---

# 🧠 Quick Memory Trick

```text
ARRAY = FIXED + SAME TYPE + INDEXED

F → Fixed Size
S → Same Component Type
I → Indexed Access
```

Remember:

```java
arr.length        // number of elements
arr.length - 1    // last valid index
arr[i]            // access element
```

---

# ⚠️ Important Interview Traps

```text
Array is an object                → YES
Array has fixed length            → YES
Array starts from index 0         → YES
Array length can be changed       → NO
arr.length()                      → WRONG
arr.length                        → CORRECT
Array object                      → Heap
Array reference                   → depends on variable scope
Invalid index                     → ArrayIndexOutOfBoundsException
Array access                      → O(1)
Array traversal                   → O(n)
```

---

# 🚀 DSA Importance

Arrays are one of the **most important foundations of DSA**.

Before moving deeply into advanced data structures, become comfortable with:

* Array traversal
* Searching
* Sorting
* Reversal
* Rotation
* Prefix sum
* Two pointers
* Sliding window
* Binary search
* Subarrays
* Frequency counting
* In-place modification
* Kadane's algorithm
* Matrix problems

> **Mastering arrays makes many later DSA topics significantly easier.**

---

# 📌 Final Summary

An array in Java:

```text
✔ Is an object
✔ Stores a fixed number of components
✔ Has a single component type
✔ Uses zero-based indexing
✔ Provides O(1) indexed access
✔ Has a fixed length
✔ Uses arr.length
✔ Can contain primitive values
✔ Can contain object references
✔ Can be multidimensional
✔ Is heavily used in DSA
```

### Core Example

```java
public class ArrayDemo {

    public static void main(String[] args) {

        int[] arr = {10, 20, 30, 40, 50};

        for (int i = 0; i < arr.length; i++) {
            System.out.println("Index " + i + " = " + arr[i]);
        }
    }
}
```

Output:

```text
Index 0 = 10
Index 1 = 20
Index 2 = 30
Index 3 = 40
Index 4 = 50
```

---

# 🎯 Key Takeaway

> **An array is a fixed-size Java object containing elements of the same component type, where each element is accessed through a zero-based index.**

For DSA:

```text
Array
  ↓
Traversal
  ↓
Searching / Sorting
  ↓
Two Pointers
  ↓
Sliding Window
  ↓
Prefix Sum
  ↓
Binary Search
  ↓
Advanced DSA
```

```
```
