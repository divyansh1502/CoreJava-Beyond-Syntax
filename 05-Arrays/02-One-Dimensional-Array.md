````markdown
# 🔢 One-Dimensional Array in Java

> **A one-dimensional array is a fixed-size linear collection of elements of the same declared component type, accessed using a single zero-based index.**

---

# 📌 Table of Contents

1. [What is a One-Dimensional Array?](#1--what-is-a-one-dimensional-array)
2. [Basic Structure](#2--basic-structure)
3. [Declaration](#3--declaration)
4. [Creation](#4--creation)
5. [Initialization](#5--initialization)
6. [Declaration + Creation + Initialization](#6--declaration--creation--initialization)
7. [Indexing](#7--indexing)
8. [Accessing Elements](#8--accessing-elements)
9. [Updating Elements](#9--updating-elements)
10. [Traversing an Array](#10--traversing-an-array)
11. [Traditional for Loop](#11--traditional-for-loop)
12. [Enhanced for Loop](#12--enhanced-for-loop)
13. [for vs Enhanced for](#13--for-vs-enhanced-for)
14. [Taking Array Input](#14--taking-array-input)
15. [Printing an Array](#15--printing-an-array)
16. [Finding Sum](#16--finding-sum)
17. [Finding Maximum and Minimum](#17--finding-maximum-and-minimum)
18. [Searching](#18--searching)
19. [Linear Search](#19--linear-search)
20. [Array Copying](#20--array-copying)
21. [Reference Copy vs Actual Copy](#21--reference-copy-vs-actual-copy)
22. [Passing Array to a Method](#22--passing-array-to-a-method)
23. [Returning an Array](#23--returning-an-array)
24. [Arrays with Methods](#24--arrays-with-methods)
25. [Array Aliasing](#25--array-aliasing)
26. [Array Comparison](#26--array-comparison)
27. [Array of Primitive Values](#27--array-of-primitive-values)
28. [Array of References](#28--array-of-references)
29. [Null and Empty Arrays](#29--null-and-empty-arrays)
30. [Common Exceptions](#30--common-exceptions)
31. [Array Memory Basics](#31--array-memory-basics)
32. [Time Complexity](#32--time-complexity)
33. [Common Mistakes](#33--common-mistakes)
34. [Interview Traps](#34--interview-traps)
35. [Top 20 Interview Questions](#35--top-20-interview-questions)
36. [Coding Problems](#36--coding-problems)
37. [DSA Patterns](#37--dsa-patterns)
38. [30-Second Interview Answer](#38--30-second-interview-answer)
39. [Cheat Sheet](#39--cheat-sheet)
40. [Memory Tricks](#40--memory-tricks)
41. [Final Revision Checklist](#41--final-revision-checklist)

---

# 1. 🔹 What is a One-Dimensional Array?

A **one-dimensional array** is a linear data structure that stores multiple elements under a single array object.

Each element is accessed using **one index**.

Example:

```java
int[] nums = {10, 20, 30, 40, 50};
```

Conceptually:

```text
Index:    0    1    2    3    4
          ↓    ↓    ↓    ↓    ↓
Value:   10   20   30   40   50
```

To access `30`:

```java
System.out.println(nums[2]);
```

Output:

```text
30
```

### Key Properties

- Linear structure
- Zero-based indexing
- Fixed length after creation
- Stores elements of one declared component type
- Supports duplicate values
- Elements can be modified
- Random access by index
- Arrays are objects in Java
- Can store primitives
- Can store references to objects

---

# 2. 🧱 Basic Structure

A one-dimensional array can be visualized as:

```text
┌────┬────┬────┬────┬────┐
│ 10 │ 20 │ 30 │ 40 │ 50 │
└────┴────┴────┴────┴────┘
  0    1    2    3    4
  ↑                    ↑
first                last
index                index
```

For an array of length `n`:

```text
First index = 0
Last index  = n - 1
```

Therefore:

```text
length = 5
valid indexes = 0, 1, 2, 3, 4
```

---

# 3. 📝 Declaration

Declaration tells Java that a variable can refer to an array of a particular component type.

Recommended syntax:

```java
int[] arr;
```

Another valid syntax:

```java
int arr[];
```

Both are legal Java.

However, this is generally preferred:

```java
int[] arr;
```

because the array type is visually associated with `int`.

### Different Array Types

```java
int[] numbers;

double[] prices;

char[] letters;

boolean[] flags;

String[] names;
```

At declaration time, no array object has been created yet.

---

# 4. 🏗️ Creation

The `new` keyword creates the array object.

```java
int[] arr = new int[5];
```

This creates an array capable of storing `5` integers.

Default contents:

```text
[0, 0, 0, 0, 0]
```

Valid indexes:

```text
0  1  2  3  4
```

### Important

```java
new int[5]
```

means:

> Create an integer array with length 5.

It does **not** mean indexes `1` through `5`.

The indexes are:

```text
0 through 4
```

---

# 5. 🎯 Initialization

After creating an array, individual elements can be assigned.

```java
int[] arr = new int[5];

arr[0] = 10;
arr[1] = 20;
arr[2] = 30;
arr[3] = 40;
arr[4] = 50;
```

Final array:

```text
[10, 20, 30, 40, 50]
```

### Default Values

When an array is created, its elements receive default values.

| Component Type | Default Value |
|---|---|
| `byte` | `0` |
| `short` | `0` |
| `int` | `0` |
| `long` | `0L` |
| `float` | `0.0f` |
| `double` | `0.0d` |
| `char` | `'\u0000'` |
| `boolean` | `false` |
| Reference type | `null` |

Example:

```java
int[] numbers = new int[3];

System.out.println(numbers[0]);
```

Output:

```text
0
```

---

# 6. 🧩 Declaration + Creation + Initialization

These concepts can happen separately or together.

## Declaration

```java
int[] arr;
```

A reference variable is declared.

## Creation

```java
arr = new int[5];
```

The array object is created.

## Initialization

```java
arr[0] = 10;
arr[1] = 20;
```

Values are assigned.

## Combined Initialization

```java
int[] arr = {10, 20, 30};
```

This creates and initializes the array.

The length is inferred automatically:

```text
arr.length = 3
```

### Important Syntax Difference

This is valid:

```java
int[] arr = {10, 20, 30};
```

But this is not valid:

```java
int[] arr;

arr = {10, 20, 30};
```

When assigning an initializer after declaration, use `new`:

```java
int[] arr;

arr = new int[]{10, 20, 30};
```

---

# 7. 🔢 Indexing

Java arrays use **zero-based indexing**.

Example:

```java
int[] arr = {100, 200, 300, 400};
```

Representation:

```text
Index:   0    1    2    3
Value: 100  200  300  400
```

Therefore:

```java
arr[0]
```

returns:

```text
100
```

And:

```java
arr[3]
```

returns:

```text
400
```

### Formula

For an array of length `n`:

```text
Minimum index = 0
Maximum index = n - 1
```

---

# 8. 👀 Accessing Elements

Use the array variable followed by an index.

```java
int[] nums = {10, 20, 30};

System.out.println(nums[0]);
System.out.println(nums[2]);
```

Output:

```text
10
30
```

### Accessing the Last Element

Use:

```java
nums[nums.length - 1]
```

Example:

```java
int[] nums = {10, 20, 30, 40, 50};

System.out.println(nums[nums.length - 1]);
```

Output:

```text
50
```

---

# 9. ✏️ Updating Elements

Arrays are mutable.

An existing element can be replaced.

```java
int[] nums = {10, 20, 30};

nums[1] = 99;
```

Now:

```text
[10, 99, 30]
```

The same array object has been modified.

```java
System.out.println(nums[1]);
```

Output:

```text
99
```

---

# 10. 🔄 Traversing an Array

Traversal means visiting array elements one by one.

The two common approaches are:

1. Traditional `for` loop
2. Enhanced `for` loop

Example array:

```java
int[] nums = {10, 20, 30, 40};
```

Traversal produces:

```text
10
20
30
40
```

---

# 11. 🔁 Traditional `for` Loop

The traditional `for` loop provides direct access to the index.

```java
int[] nums = {10, 20, 30, 40};

for (int i = 0; i < nums.length; i++) {
    System.out.println(nums[i]);
}
```

Output:

```text
10
20
30
40
```

### Why `i < nums.length`?

Suppose:

```text
nums.length = 4
```

Valid indexes:

```text
0
1
2
3
```

Therefore:

```text
i < 4
```

allows:

```text
0, 1, 2, 3
```

But:

```text
i <= 4
```

would eventually attempt:

```text
nums[4]
```

which is invalid.

---

# 12. 🚀 Enhanced `for` Loop

The enhanced `for` loop is also called the **for-each loop**.

Syntax:

```java
for (type variable : array) {
    // body
}
```

Example:

```java
int[] nums = {10, 20, 30, 40};

for (int num : nums) {
    System.out.println(num);
}
```

Output:

```text
10
20
30
40
```

### How to Read It

```java
for (int num : nums)
```

means:

> For every element in `nums`, assign its value to `num`.

---

# 13. ⚔️ `for` vs Enhanced `for`

| Feature | Traditional `for` | Enhanced `for` |
|---|---|---|
| Index available | ✅ | ❌ |
| Direct value available | ✅ | ✅ |
| Easy traversal | ✅ | ✅ |
| Index-based update | ✅ | ❌ |
| Reverse traversal | ✅ | ❌ Directly |
| Skip selected indexes | ✅ | Less convenient |
| Syntax | More verbose | Simpler |

### Use Traditional `for` When

You need the index.

```java
for (int i = 0; i < arr.length; i++) {
    if (arr[i] == target) {
        System.out.println(i);
    }
}
```

### Use Enhanced `for` When

You only need the values.

```java
for (int value : arr) {
    System.out.println(value);
}
```

### Important Trap

Changing the enhanced-for variable does not modify the primitive array element.

```java
int[] arr = {10, 20, 30};

for (int value : arr) {
    value = 100;
}
```

The array is still:

```text
[10, 20, 30]
```

Because `value` receives a copy of each primitive value.

---

# 14. ⌨️ Taking Array Input

Using `Scanner`:

```java
import java.util.Scanner;

Scanner sc = new Scanner(System.in);

int n = sc.nextInt();

int[] arr = new int[n];

for (int i = 0; i < arr.length; i++) {
    arr[i] = sc.nextInt();
}
```

For input:

```text
5
10 20 30 40 50
```

The resulting array is:

```text
[10, 20, 30, 40, 50]
```

### Important DSA Pattern

Memorize this:

```java
int n = sc.nextInt();

int[] arr = new int[n];

for (int i = 0; i < n; i++) {
    arr[i] = sc.nextInt();
}
```

This pattern appears frequently in DSA problems.

---

# 15. 🖨️ Printing an Array

Directly printing an array does not print its contents.

```java
int[] arr = {10, 20, 30};

System.out.println(arr);
```

The output is generally a type/hash-style representation.

For readable output, use `Arrays.toString()`:

```java
import java.util.Arrays;

int[] arr = {10, 20, 30};

System.out.println(Arrays.toString(arr));
```

Output:

```text
[10, 20, 30]
```

`Arrays` is a utility class from `java.util`.

Its methods will be covered in detail in:

```text
05-Arrays-Class.md
```

---

# 16. ➕ Finding Sum

Example:

```java
int[] arr = {10, 20, 30, 40};

int sum = 0;

for (int i = 0; i < arr.length; i++) {
    sum += arr[i];
}

System.out.println(sum);
```

Output:

```text
100
```

### Logic

```text
sum = 0

sum = 0 + 10
sum = 10 + 20
sum = 30 + 30
sum = 60 + 40

Final = 100
```

Time complexity:

```text
O(n)
```

---

# 17. 📈 Finding Maximum and Minimum

## Maximum

```java
int[] arr = {10, 50, 20, 80, 30};

int max = arr[0];

for (int i = 1; i < arr.length; i++) {
    if (arr[i] > max) {
        max = arr[i];
    }
}

System.out.println(max);
```

Output:

```text
80
```

## Minimum

```java
int[] arr = {10, 50, 20, 80, 30};

int min = arr[0];

for (int i = 1; i < arr.length; i++) {
    if (arr[i] < min) {
        min = arr[i];
    }
}

System.out.println(min);
```

Output:

```text
10
```

### Why Start With `arr[0]`?

Using:

```java
int max = arr[0];
```

is safer than assuming a value such as:

```java
int max = 0;
```

because the array could contain only negative values.

Example:

```text
[-50, -20, -100]
```

Starting with `0` would produce the wrong maximum.

---

# 18. 🔍 Searching

Searching means checking whether a target value exists in an array.

Example:

```java
int[] arr = {10, 20, 30, 40};

int target = 30;
```

We want to determine whether `30` exists.

For an unsorted array, a common approach is **linear search**.

---

# 19. 🔎 Linear Search

Linear search checks elements sequentially.

```java
int[] arr = {10, 20, 30, 40};

int target = 30;

for (int i = 0; i < arr.length; i++) {
    if (arr[i] == target) {
        System.out.println("Found at index " + i);
        break;
    }
}
```

Output:

```text
Found at index 2
```

### Complexity

| Case | Complexity |
|---|---:|
| Best | O(1) |
| Average | O(n) |
| Worst | O(n) |

If the target is at index `0`, we find it immediately.

If the target is absent or at the last index, we may inspect every element.

---

# 20. 📋 Array Copying

Suppose:

```java
int[] a = {10, 20, 30};
```

There are two important concepts:

1. Copying the reference
2. Creating a separate array object

These are fundamentally different.

---

# 21. 🔗 Reference Copy vs Actual Copy

## Reference Copy

```java
int[] a = {10, 20, 30};

int[] b = a;
```

Now both references point to the same array.

Conceptually:

```text
a ───────┐
         ↓
     [10, 20, 30]
         ↑
b ───────┘
```

Therefore:

```java
b[0] = 99;

System.out.println(a[0]);
```

Output:

```text
99
```

### Why?

Because no new array was created.

Only the reference value was copied.

---

## Actual Copy

A separate array object can be created.

Conceptually:

```text
a → [10, 20, 30]

b → [10, 20, 30]
```

Now changes to `b` do not modify `a`.

Common copying mechanisms include:

```java
Arrays.copyOf()
```

```java
System.arraycopy()
```

```java
clone()
```

These APIs are covered in detail later.

---

# 22. 📤 Passing Array to a Method

Arrays can be passed as method arguments.

```java
static void printArray(int[] arr) {
    for (int value : arr) {
        System.out.println(value);
    }
}
```

Call:

```java
int[] nums = {10, 20, 30};

printArray(nums);
```

### Important Java Concept

Java is **always pass-by-value**.

When an array is passed to a method, the value copied is the **reference value**.

Therefore, both the caller and method parameter can refer to the same array object.

Example:

```java
static void change(int[] arr) {
    arr[0] = 100;
}

int[] nums = {10, 20, 30};

change(nums);

System.out.println(nums[0]);
```

Output:

```text
100
```

The method modified the shared array object.

---

# 23. 📥 Returning an Array

A method can return an array.

```java
static int[] createArray() {
    int[] arr = {10, 20, 30};
    return arr;
}
```

Calling:

```java
int[] nums = createArray();

System.out.println(nums[0]);
```

Output:

```text
10
```

### Returning a New Array

```java
static int[] createNumbers() {
    return new int[]{10, 20, 30};
}
```

---

# 24. 🔧 Arrays with Methods

Arrays are commonly used as method inputs and outputs.

Example:

```java
static int sum(int[] arr) {
    int sum = 0;

    for (int value : arr) {
        sum += value;
    }

    return sum;
}
```

Call:

```java
int[] nums = {10, 20, 30};

System.out.println(sum(nums));
```

Output:

```text
60
```

This style is very common in DSA.

---

# 25. 🔗 Array Aliasing

**Aliasing** occurs when multiple references point to the same object.

Example:

```java
int[] a = {10, 20, 30};

int[] b = a;
```

Now:

```java
System.out.println(a == b);
```

Output:

```text
true
```

Visual:

```text
a ─────┐
       ↓
   [10, 20, 30]
       ↑
b ─────┘
```

Changing through either reference changes the same array.

```java
b[1] = 99;

System.out.println(a[1]);
```

Output:

```text
99
```

---

# 26. ⚖️ Array Comparison

Do not use `==` to compare array contents.

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

Because `==` compares reference identity for arrays.

The two variables refer to different array objects.

For content comparison, use:

```java
import java.util.Arrays;

System.out.println(Arrays.equals(a, b));
```

Output:

```text
true
```

### Remember

```text
==                  → same array object?
Arrays.equals()     → same one-dimensional contents?
```

---

# 27. 🔢 Array of Primitive Values

Example:

```java
int[] numbers = {10, 20, 30};
```

The array's component type is `int`.

Conceptually:

```text
numbers
   ↓
┌────┬────┬────┐
│ 10 │ 20 │ 30 │
└────┴────┴────┘
```

The array stores primitive values.

Other examples:

```java
double[] prices = {10.5, 20.5, 30.5};

char[] letters = {'A', 'B', 'C'};

boolean[] flags = {true, false, true};
```

---

# 28. 👥 Array of References

Arrays can also contain references to objects.

Example:

```java
String[] names = new String[3];
```

Initially:

```text
[null, null, null]
```

Then:

```java
names[0] = "Java";
names[1] = "Python";
names[2] = "C++";
```

Conceptually:

```text
names
  ↓
┌────────┬────────┬────────┐
│   ↓    │   ↓    │   ↓    │
└───┼────┴───┼────┴───┼────┘
    ↓        ↓        ↓
 "Java"   "Python"   "C++"
```

The array's component type is `String`.

The elements are references to `String` objects.

---

# 29. 🚫 Null and Empty Arrays

These are different:

```java
int[] a = new int[0];

int[] b = null;
```

## Empty Array

```java
int[] a = new int[0];

System.out.println(a.length);
```

Output:

```text
0
```

An array object exists.

It simply contains zero elements.

## Null Reference

```java
int[] b = null;
```

The variable does not refer to an array object.

Therefore:

```java
System.out.println(b.length);
```

throws:

```text
NullPointerException
```

### Key Difference

```text
Empty array → object exists, length = 0

null array  → no array object is referenced
```

---

# 30. 💥 Common Exceptions

## 1. ArrayIndexOutOfBoundsException

Example:

```java
int[] arr = {10, 20};

System.out.println(arr[2]);
```

Valid indexes are:

```text
0
1
```

Index `2` is invalid.

---

## 2. NullPointerException

Example:

```java
int[] arr = null;

System.out.println(arr.length);
```

The reference is `null`.

---

## 3. ArrayStoreException

This can occur with reference arrays when the runtime array type does not allow the value being stored.

Example:

```java
Object[] arr = new String[2];

arr[0] = 100;
```

The runtime array object is actually a `String[]`.

Therefore, storing an `Integer` causes:

```text
ArrayStoreException
```

---

# 31. 🧠 Array Memory Basics

An array is an **object** in Java.

For:

```java
int[] arr = new int[5];
```

conceptually:

```text
Stack/reference context
        │
        │ arr
        ↓
Heap
┌──────────────────────┐
│ Array Object         │
│ length = 5           │
│                      │
│ [0][0][0][0][0]      │
└──────────────────────┘
```

The exact JVM memory implementation is JVM-dependent, but conceptually:

- The variable holds a reference.
- The array object exists on the heap.
- The array has a fixed length.
- Elements are stored as the array's components.

### Important

Do not oversimplify this as:

> "The reference is always on stack and the array is always on heap."

JVM implementation details can vary, and optimized execution can change physical storage behavior.

For interview fundamentals, remember:

```text
Array = object
Array reference = points to array object
Array length = fixed after creation
```

---

# 32. ⏱️ Time Complexity

| Operation | Complexity |
|---|---:|
| Access by index | O(1) |
| Update by index | O(1) |
| Traversal | O(n) |
| Linear search | O(n) |
| Find maximum | O(n) |
| Find minimum | O(n) |
| Find sum | O(n) |
| Reverse | O(n) |
| Copy | O(n) |
| Insert in middle | O(n) |
| Delete from middle | O(n) |

### Why Is Index Access O(1)?

Given:

```java
arr[i]
```

the runtime can directly locate the component associated with index `i` without scanning all previous elements.

Therefore:

```text
Access → O(1)
```

---

# 33. ⚠️ Common Mistakes

## ❌ Mistake 1 — Using `<=`

Wrong:

```java
for (int i = 0; i <= arr.length; i++) {
    System.out.println(arr[i]);
}
```

Correct:

```java
for (int i = 0; i < arr.length; i++) {
    System.out.println(arr[i]);
}
```

---

## ❌ Mistake 2 — Calling `length()`

Wrong:

```java
arr.length();
```

Correct:

```java
arr.length;
```

Remember:

```text
Array      → length
String     → length()
Collection → size()
```

---

## ❌ Mistake 3 — Assuming Assignment Copies the Array

Wrong assumption:

```java
int[] b = a;
```

means:

> Create a new array.

Actually:

```text
Copies the reference value.
```

---

## ❌ Mistake 4 — Comparing Contents With `==`

Wrong for content comparison:

```java
a == b
```

Use:

```java
Arrays.equals(a, b)
```

---

## ❌ Mistake 5 — Accessing an Empty Array

```java
int[] arr = new int[0];

System.out.println(arr[0]);
```

There is no valid index.

---

## ❌ Mistake 6 — Assuming Array Size Is Dynamic

After:

```java
int[] arr = new int[5];
```

the array length remains:

```text
5
```

You cannot resize that array directly.

A new array must be created.

---

# 34. 🚨 Interview Traps

## Trap 1

```java
int[] arr = new int[5];

System.out.println(arr.length);
```

Answer:

```text
5
```

---

## Trap 2

What is the last valid index?

```text
arr.length - 1
```

---

## Trap 3

```java
int[] arr = new int[0];
```

Does an array object exist?

Answer:

```text
Yes.
```

It is an empty array.

---

## Trap 4

```java
int[] arr = null;
```

Does an array object exist?

Answer:

```text
The reference does not refer to an array object.
```

---

## Trap 5

```java
int[] a = {1, 2, 3};

int[] b = a;

b[0] = 100;

System.out.println(a[0]);
```

Answer:

```text
100
```

Both references refer to the same array.

---

## Trap 6

```java
int[] a = {1, 2, 3};

int[] b = {1, 2, 3};

System.out.println(a == b);
```

Answer:

```text
false
```

Different array objects.

---

## Trap 7

```java
int[] a = {1, 2, 3};

int[] b = a;

System.out.println(a == b);
```

Answer:

```text
true
```

Same array object.

---

## Trap 8

```java
int[] arr = new int[5];
```

Number of elements:

```text
5
```

Number of valid indexes:

```text
5
```

Highest valid index:

```text
4
```

---

## Trap 9

```java
int[] arr = {};
```

This is a valid empty array.

Its length is:

```text
0
```

---

## Trap 10

```java
int[] arr = null;

System.out.println(arr.length);
```

Result:

```text
NullPointerException
```

---

# 35. 🔥 Top 20 Interview Questions

## Q1. What is a one-dimensional array?

**Answer:**

A one-dimensional array is a fixed-size array object whose elements are accessed using a single zero-based index.

---

## Q2. How does Java index arrays?

**Answer:**

Java uses zero-based indexing.

---

## Q3. What is the first index?

**Answer:**

```text
0
```

---

## Q4. What is the last valid index?

**Answer:**

```text
arr.length - 1
```

---

## Q5. How do you find an array's length?

**Answer:**

Using the `length` field.

```java
arr.length
```

---

## Q6. Is `length` a method?

**Answer:**

No.

For arrays, `length` is a field.

---

## Q7. Can an array contain duplicate values?

**Answer:**

Yes.

```java
int[] arr = {10, 10, 20, 20};
```

---

## Q8. Can an array size change after creation?

**Answer:**

No.

An array has a fixed length after creation.

---

## Q9. Can arrays store objects?

**Answer:**

Yes.

They store references to objects.

---

## Q10. Can arrays store primitives?

**Answer:**

Yes.

For example:

```java
int[] numbers = {1, 2, 3};
```

---

## Q11. What happens when an array is assigned to another variable?

**Answer:**

The reference value is copied.

The array object is not automatically copied.

---

## Q12. What is array aliasing?

**Answer:**

Aliasing occurs when multiple references refer to the same array object.

---

## Q13. How do you compare one-dimensional array contents?

**Answer:**

Use:

```java
Arrays.equals(a, b)
```

---

## Q14. What is the complexity of array access?

**Answer:**

Typically:

```text
O(1)
```

---

## Q15. What is linear search complexity?

**Answer:**

Worst case:

```text
O(n)
```

---

## Q16. Can an array have length zero?

**Answer:**

Yes.

```java
int[] arr = new int[0];
```

---

## Q17. What happens when an invalid index is accessed?

**Answer:**

An `ArrayIndexOutOfBoundsException` is thrown.

---

## Q18. What happens when a null array reference is accessed?

**Answer:**

Usually a `NullPointerException` occurs.

---

## Q19. Can a method return an array?

**Answer:**

Yes.

```java
static int[] createArray() {
    return new int[]{1, 2, 3};
}
```

---

## Q20. What is the difference between an empty array and null?

**Answer:**

An empty array is an actual array object with length `0`, while `null` means the reference does not refer to an array object.

---

# 36. 💻 Coding Problems

## Problem 1 — Print All Elements

```java
int[] arr = {10, 20, 30, 40};

for (int i = 0; i < arr.length; i++) {
    System.out.println(arr[i]);
}
```

---

## Problem 2 — Find Sum

```java
int[] arr = {10, 20, 30};

int sum = 0;

for (int value : arr) {
    sum += value;
}

System.out.println(sum);
```

Output:

```text
60
```

---

## Problem 3 — Find Maximum

```java
int[] arr = {10, 50, 20, 80, 30};

int max = arr[0];

for (int i = 1; i < arr.length; i++) {
    if (arr[i] > max) {
        max = arr[i];
    }
}

System.out.println(max);
```

Output:

```text
80
```

---

## Problem 4 — Find Minimum

```java
int[] arr = {10, 50, 20, 80, 30};

int min = arr[0];

for (int i = 1; i < arr.length; i++) {
    if (arr[i] < min) {
        min = arr[i];
    }
}

System.out.println(min);
```

Output:

```text
10
```

---

## Problem 5 — Count Even Numbers

```java
int[] arr = {10, 15, 20, 25, 30};

int count = 0;

for (int value : arr) {
    if (value % 2 == 0) {
        count++;
    }
}

System.out.println(count);
```

Output:

```text
3
```

---

## Problem 6 — Linear Search

```java
int[] arr = {10, 20, 30, 40};

int target = 30;

int index = -1;

for (int i = 0; i < arr.length; i++) {
    if (arr[i] == target) {
        index = i;
        break;
    }
}

System.out.println(index);
```

Output:

```text
2
```

---

## Problem 7 — Reverse an Array

```java
int[] arr = {10, 20, 30, 40, 50};

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

Final array:

```text
[50, 40, 30, 20, 10]
```

### DSA Pattern

This uses the **two-pointer technique**.

```text
left  → 
        [ ... ]
              ← right
```

---

## Problem 8 — Count Occurrences

```java
int[] arr = {10, 20, 10, 30, 10};

int target = 10;

int count = 0;

for (int value : arr) {
    if (value == target) {
        count++;
    }
}

System.out.println(count);
```

Output:

```text
3
```

---

## Problem 9 — Find First Occurrence

```java
int[] arr = {10, 20, 30, 20, 40};

int target = 20;

int index = -1;

for (int i = 0; i < arr.length; i++) {
    if (arr[i] == target) {
        index = i;
        break;
    }
}

System.out.println(index);
```

Output:

```text
1
```

---

## Problem 10 — Check If Array Is Sorted

```java
int[] arr = {10, 20, 30, 40, 50};

boolean sorted = true;

for (int i = 1; i < arr.length; i++) {
    if (arr[i] < arr[i - 1]) {
        sorted = false;
        break;
    }
}

System.out.println(sorted);
```

Output:

```text
true
```

---

# 37. 🧠 DSA Patterns

One-dimensional arrays are the foundation of many DSA patterns.

## 1. Traversal

Basic pattern:

```java
for (int i = 0; i < arr.length; i++) {
    // process arr[i]
}
```

Used for:

- Sum
- Count
- Search
- Maximum
- Minimum
- Frequency
- Transformation

---

## 2. Two Pointers

Typical structure:

```java
int left = 0;
int right = arr.length - 1;

while (left < right) {
    // process arr[left] and arr[right]

    left++;
    right--;
}
```

Used for:

- Reverse array
- Pair problems
- Palindrome-style problems
- Partitioning
- Sorted-array problems

---

## 3. Running Variable

Example:

```java
int max = arr[0];

for (int i = 1; i < arr.length; i++) {
    max = Math.max(max, arr[i]);
}
```

Used for:

- Maximum
- Minimum
- Running sum
- Best/worst value

---

## 4. Search With Sentinel

A common search pattern:

```java
int index = -1;

for (int i = 0; i < arr.length; i++) {
    if (arr[i] == target) {
        index = i;
        break;
    }
}
```

Meaning:

```text
-1 → not found
0+ → found index
```

---

## 5. Prefix Sum

A common DSA technique:

```java
int[] arr = {2, 4, 6, 8};

int[] prefix = new int[arr.length];

prefix[0] = arr[0];

for (int i = 1; i < arr.length; i++) {
    prefix[i] = prefix[i - 1] + arr[i];
}
```

Result:

```text
arr    = [2, 4, 6, 8]

prefix = [2, 6, 12, 20]
```

Prefix sums can reduce repeated range-sum calculations.

---

# 38. 🎤 30-Second Interview Answer

> **A one-dimensional array in Java is a fixed-size array object whose elements are accessed using a single zero-based index. It can store primitive values or references to objects. Its length is accessed using the `length` field and cannot be changed after creation. Accessing or updating an element by index is typically O(1), while traversal and linear search take O(n). Arrays are commonly used as the foundation for many DSA techniques such as traversal, two pointers, prefix sums, and searching.**

---

# 39. 🧾 Cheat Sheet

## Declaration

```java
int[] arr;
```

## Creation

```java
arr = new int[5];
```

## Initialization

```java
int[] arr = {10, 20, 30};
```

## Access

```java
arr[index]
```

## Update

```java
arr[index] = value;
```

## Length

```java
arr.length
```

## First Element

```java
arr[0]
```

## Last Element

```java
arr[arr.length - 1]
```

## Traditional Traversal

```java
for (int i = 0; i < arr.length; i++) {
    System.out.println(arr[i]);
}
```

## Enhanced Traversal

```java
for (int value : arr) {
    System.out.println(value);
}
```

## Linear Search

```java
for (int i = 0; i < arr.length; i++) {
    if (arr[i] == target) {
        return i;
    }
}
```

## Array Content Comparison

```java
Arrays.equals(a, b);
```

## Array Printing

```java
Arrays.toString(arr);
```

---

# 40. 🧠 Memory Tricks

## 🔥 Trick 1 — Index Formula

Remember:

```text
Length = n
First index = 0
Last index = n - 1
```

---

## 🔥 Trick 2 — Array vs String vs Collection

```text
Array      → length
String     → length()
Collection → size()
```

---

## 🔥 Trick 3 — Loop Boundary

Remember:

```java
i < arr.length
```

not normally:

```java
i <= arr.length
```

---

## 🔥 Trick 4 — Assignment

Remember:

```java
b = a;
```

means:

```text
Reference copied
```

not:

```text
Array copied
```

---

## 🔥 Trick 5 — Search

```text
Unsorted array
      ↓
Linear Search
      ↓
O(n)
```

---

## 🔥 Trick 6 — Access

```text
Known index
    ↓
arr[index]
    ↓
O(1)
```

---

## 🔥 Trick 7 — Reverse

```text
left  →       ←  right
```

Move both pointers toward the center.

---

# 41. ✅ Final Revision Checklist

Before moving forward, make sure you can explain:

```text
[ ] What is a one-dimensional array?
[ ] How to declare an array?
[ ] How to create an array?
[ ] How to initialize an array?
[ ] What is zero-based indexing?
[ ] What is the first index?
[ ] What is the last index?
[ ] How to access an element?
[ ] How to update an element?
[ ] What is arr.length?
[ ] Difference between length and length()
[ ] Traditional for loop
[ ] Enhanced for loop
[ ] Difference between both
[ ] How to take array input
[ ] How to print an array
[ ] How to calculate sum
[ ] How to find maximum
[ ] How to find minimum
[ ] What is linear search?
[ ] Reference copy vs actual copy
[ ] What is array aliasing?
[ ] How arrays are passed to methods
[ ] How arrays are returned from methods
[ ] Array of primitives
[ ] Array of references
[ ] Empty array vs null
[ ] ArrayIndexOutOfBoundsException
[ ] NullPointerException
[ ] ArrayStoreException
[ ] Array access complexity
[ ] Two-pointer pattern
[ ] Prefix-sum pattern
```

---

# 🏆 MASTER MEMORY CARD

```text
┌──────────────────────────────────────────────┐
│          ONE-DIMENSIONAL ARRAY               │
├──────────────────────────────────────────────┤
│ Linear structure                             │
│ Single index                                 │
│ Zero-based indexing                          │
│ Fixed length                                 │
│ Array is an object                           │
│ arr.length                                   │
│ Access → O(1)                                │
│ Update → O(1)                                │
│ Search → O(n)                                │
│ Traverse → O(n)                              │
│ Allows duplicates                            │
│ Mutable elements                             │
│ Can contain primitive values                 │
│ Can contain object references                │
│ Supports two-pointer problems                │
│ Supports prefix-sum problems                 │
└──────────────────────────────────────────────┘
```

---

# ⭐ ONE-LINE INTERVIEW DEFINITION

> **A one-dimensional array is a fixed-size array object in Java whose elements are accessed using a single zero-based index.**

---

# 🔗 NEXT TOPIC

```text
05-Arrays/

│
├── 01-Array-Introduction.md
├── 02-One-Dimensional-Array.md      ← YOU ARE HERE
├── 03-Multidimensional-Array.md
├── 04-Array-Memory.md
├── 05-Arrays-Class.md
└── 06-Array-Interview-Questions.md
```

### Next:

> **03 — Multidimensional Array**

We will cover:

- 2D arrays
- Matrices
- Rows and columns
- Nested arrays
- Jagged arrays
- Memory structure
- Traversal
- Input and output
- Common mistakes
- Interview traps
- DSA patterns
- Coding problems
````
