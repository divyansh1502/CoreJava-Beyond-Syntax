````md
# 🧠 Array Memory in Java

> **In Java, an array is an object created dynamically at runtime. The array variable does not contain the complete array itself; it contains a reference to the array object.**

---

# 📌 Table of Contents

1. [Why Learn Array Memory?](#1--why-learn-array-memory)
2. [Array as an Object](#2--array-as-an-object)
3. [Reference Variable vs Array Object](#3--reference-variable-vs-array-object)
4. [Basic Memory Model](#4--basic-memory-model)
5. [Where Is an Array Stored?](#5--where-is-an-array-stored)
6. [Stack vs Heap](#6--stack-vs-heap)
7. [Primitive Array Memory](#7--primitive-array-memory)
8. [Reference Array Memory](#8--reference-array-memory)
9. [Array Variable Assignment](#9--array-variable-assignment)
10. [Two References Pointing to Same Array](#10--two-references-pointing-to-same-array)
11. [Array Copy vs Reference Copy](#11--array-copy-vs-reference-copy)
12. [Changing Through Another Reference](#12--changing-through-another-reference)
13. [`new` and Array Allocation](#13--new-and-array-allocation)
14. [Default Values](#14--default-values)
15. [The `length` Property](#15--the-length-property)
16. [Array Index and Memory](#16--array-index-and-memory)
17. [Why Array Access Is O(1)](#17--why-array-access-is-o1)
18. [1D Array Memory](#18--1d-array-memory)
19. [2D Array Memory](#19--2d-array-memory)
20. [2D Array Is an Array of Arrays](#20--2d-array-is-an-array-of-arrays)
21. [Jagged Array Memory](#21--jagged-array-memory)
22. [2D Array Reference Diagram](#22--2d-array-reference-diagram)
23. [Primitive vs Reference Arrays](#23--primitive-vs-reference-arrays)
24. [Array of Objects](#24--array-of-objects)
25. [Array of Strings](#25--array-of-strings)
26. [Null References](#26--null-references)
27. [Garbage Collection and Arrays](#27--garbage-collection-and-arrays)
28. [Array Memory and Method Calls](#28--array-memory-and-method-calls)
29. [Java Is Pass-by-Value](#29--java-is-pass-by-value)
30. [Changing Array Elements in a Method](#30--changing-array-elements-in-a-method)
31. [Reassigning an Array Reference in a Method](#31--reassigning-an-array-reference-in-a-method)
32. [Common Memory Mistakes](#32--common-memory-mistakes)
33. [Interview Traps](#33--interview-traps)
34. [DSA Connection](#34--dsa-connection)
35. [DSA Patterns Related to Arrays](#35--dsa-patterns-related-to-arrays)
36. [How to Identify Array DSA Patterns](#36--how-to-identify-array-dsa-patterns)
37. [DSA Pattern 1 — Traversal](#37--dsa-pattern-1--traversal)
38. [DSA Pattern 2 — Two Pointers](#38--dsa-pattern-2--two-pointers)
39. [DSA Pattern 3 — Sliding Window](#39--dsa-pattern-3--sliding-window)
40. [DSA Pattern 4 — Prefix Sum](#40--dsa-pattern-4--prefix-sum)
41. [DSA Pattern 5 — Hashing / Frequency](#41--dsa-pattern-5--hashing--frequency)
42. [DSA Pattern 6 — Binary Search](#42--dsa-pattern-6--binary-search)
43. [DSA Pattern 7 — Sorting + Array](#43--dsa-pattern-7--sorting--array)
44. [DSA Pattern 8 — In-Place Modification](#44--dsa-pattern-8--in-place-modification)
45. [DSA Pattern 9 — Kadane's Algorithm](#45--dsa-pattern-9--kadanes-algorithm)
46. [DSA Pattern 10 — Difference Array](#46--dsa-pattern-10--difference-array)
47. [DSA Pattern 11 — Monotonic Stack Connection](#47--dsa-pattern-11--monotonic-stack-connection)
48. [DSA Pattern 12 — Matrix / 2D Array](#48--dsa-pattern-12--matrix--2d-array)
49. [How to Think About an Array Problem](#49--how-to-think-about-an-array-problem)
50. [Important DSA Questions](#50--important-dsa-questions)
51. [DSA Interview Traps](#51--dsa-interview-traps)
52. [Top 20 Java Array Interview Questions](#52--top-20-java-array-interview-questions)
53. [30-Second Interview Answer](#53--30-second-interview-answer)
54. [Cheat Sheet](#54--cheat-sheet)
55. [Memory Tricks](#55--memory-tricks)
56. [Final Revision Checklist](#56--final-revision-checklist)
57. [Master Memory Card](#57--master-memory-card)

---

# 1. 🔥 Why Learn Array Memory?

Understanding array memory helps you understand:

- References
- Objects
- Heap and stack
- `new`
- Array indexing
- Primitive vs reference arrays
- 2D arrays
- Jagged arrays
- Garbage collection
- Pass-by-value
- Shallow vs deep copying
- In-place algorithms
- Time and space complexity

It also helps answer common interview questions such as:

> Where is an array stored in Java?

> What does an array variable actually contain?

> Why does assigning one array to another not create a copy?

> Why can two array variables modify the same array?

> Why is array access O(1)?

> Why can many DSA array problems be solved in O(n)?

---

# 2. 🧱 Array as an Object

One of the most important facts:

> **Arrays are objects in Java.**

Even though arrays have special syntax, they are still objects created at runtime.

Example:

```java
int[] arr = new int[5];
```

The `new` keyword creates an array object.

Conceptually:

```text
arr ───────────────► [0, 0, 0, 0, 0]
                         Array Object
```

The variable `arr` stores a reference to the array object.

---

# 3. 🔗 Reference Variable vs Array Object

Consider:

```java
int[] arr = new int[5];
```

There are two conceptual parts.

### Reference Variable

```text
arr
```

### Array Object

```text
new int[5]
```

Conceptually:

```text
Stack                    Heap

arr ──────────────────► Array Object
                         [0][0][0][0][0]
```

The variable does not contain all five integers itself.

It contains a reference that allows the program to access the array object.

---

# 4. 🧠 Basic Memory Model

For learning purposes, Java memory is commonly visualized using:

- Stack
- Heap
- Method Area / Metaspace

For:

```java
int[] arr = new int[5];
```

Conceptually:

```text
STACK                         HEAP

arr ───────────────────────► int[] object

                              ┌───────────────┐
                              │ 0 │ 0 │ 0 │ 0 │ 0 │
                              └───────────────┘
```

> ⚠️ This is a conceptual JVM memory model. The exact physical implementation is JVM-dependent.

---

# 5. 📦 Where Is an Array Stored?

The array itself is an object.

Array objects are allocated in the heap.

Example:

```java
int[] arr = new int[5];
```

Conceptually:

```text
arr
 │
 │ reference
 ▼
Heap

┌─────────────────┐
│ Array Object    │
│ 0  0  0  0  0  │
└─────────────────┘
```

Important:

> The array object is allocated in the heap.

The local variable `arr` is conceptually represented in the current thread's stack frame.

---

# 6. ⚔️ Stack vs Heap

## Stack

The stack is associated with method execution and contains stack frames.

Local variables and references may be represented in stack frames.

Example:

```java
public static void main(String[] args) {
    int[] arr = new int[3];
}
```

Conceptually:

```text
Stack

┌───────────────┐
│ arr = ref     │
└───────┬───────┘
        │
        ▼

Heap

┌───────────────┐
│ [0][0][0]     │
└───────────────┘
```

## Heap

The heap is the runtime memory area where objects and arrays are allocated.

Example:

```java
new int[3]
```

creates an array object.

---

# 7. 🔢 Primitive Array Memory

Example:

```java
int[] arr = new int[4];
```

The array contains primitive `int` values.

Conceptually:

```text
arr ─────────► [10][20][30][40]
```

The elements themselves are integer values.

There is no separate object for each integer.

Example:

```java
arr[0] = 10;
```

means the first element contains the value `10`.

---

# 8. 🧩 Reference Array Memory

Consider:

```java
Student[] students = new Student[3];
```

This does **not** create three `Student` objects.

It creates an array capable of storing references to `Student` objects.

Initially:

```text
students
   │
   ▼
[ null ][ null ][ null ]
```

To create students:

```java
students[0] = new Student();
students[1] = new Student();
```

Now conceptually:

```text
students
   │
   ▼
[ ref ][ ref ][ null ]
   │     │
   ▼     ▼
Student Student
Object  Object
```

This distinction is extremely important.

---

# 9. 🔄 Array Variable Assignment

Consider:

```java
int[] a = {10, 20, 30};
int[] b = a;
```

Many beginners think this creates a new array.

It does **not**.

It copies the reference.

Conceptually:

```text
a ──────┐
        │
        ▼
      [10][20][30]
        ▲
        │
b ──────┘
```

Both variables refer to the same array object.

---

# 10. 👥 Two References Pointing to Same Array

Example:

```java
int[] a = {10, 20, 30};
int[] b = a;
```

Now:

```java
a == b
```

returns:

```text
true
```

because both references point to the same array object.

Conceptually:

```text
a ─────────┐
           │
           ▼
         [10][20][30]
           ▲
           │
b ─────────┘
```

---

# 11. 📋 Array Copy vs Reference Copy

This distinction is extremely important.

## Reference Copy

```java
int[] a = {1, 2, 3};
int[] b = a;
```

This means:

```text
a ────────► [1,2,3]
               ▲
               │
b ─────────────┘
```

Only the reference is copied.

## Actual Array Copy

To create a separate array:

```java
int[] a = {1, 2, 3};
int[] b = a.clone();
```

Now:

```text
a ─────► [1,2,3]

b ─────► [1,2,3]
```

These are separate array objects.

Therefore:

```java
a != b
```

but:

```java
a[0] == b[0]
```

because both contain the value `1`.

> ⚠️ `clone()` creates a shallow copy. For primitive arrays this usually behaves like a complete value copy, while reference arrays copy the references to the same referenced objects.

---

# 12. ✏️ Changing Through Another Reference

Example:

```java
int[] a = {10, 20, 30};
int[] b = a;

b[0] = 100;

System.out.println(a[0]);
```

Output:

```text
100
```

Why?

Because `a` and `b` refer to the same array.

Before:

```text
a ───────┐
         ▼
       [10][20][30]
         ▲
         │
b ───────┘
```

After:

```text
a ───────┐
         ▼
      [100][20][30]
         ▲
         │
b ───────┘
```

---

# 13. 🆕 `new` and Array Allocation

The `new` keyword creates a new array object.

Example:

```java
int[] a = new int[3];
int[] b = new int[3];
```

These are two different array objects.

Conceptually:

```text
a ─────► [0][0][0]

b ─────► [0][0][0]
```

Even though their contents are identical:

```java
a != b
```

because they are different objects.

---

# 14. 0️⃣ Default Values

When an array is created, its elements automatically receive default values.

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
| Reference types | `null` |

Example:

```java
int[] arr = new int[3];
```

Result:

```text
[0, 0, 0]
```

Example:

```java
boolean[] arr = new boolean[3];
```

Result:

```text
[false, false, false]
```

Example:

```java
String[] arr = new String[3];
```

Result:

```text
[null, null, null]
```

---

# 15. 📏 The `length` Property

Every Java array has a built-in `length` property.

Example:

```java
int[] arr = new int[5];

System.out.println(arr.length);
```

Output:

```text
5
```

Important:

```java
arr.length
```

is a property.

It is **not** a method.

Correct:

```java
arr.length
```

Wrong:

```java
arr.length()
```

---

# 16. 🔢 Array Index and Memory

Suppose:

```java
int[] arr = {10, 20, 30, 40};
```

Indexes:

```text
0 → 10
1 → 20
2 → 30
3 → 40
```

We access:

```java
arr[2]
```

to get:

```text
30
```

At the conceptual level, the index identifies the requested element directly.

This is why array element access is considered:

```text
O(1)
```

---

# 17. ⚡ Why Array Access Is O(1)

Suppose:

```java
int[] arr = new int[1000];
```

Access:

```java
arr[500]
```

does not require sequentially scanning:

```text
arr[0]
arr[1]
arr[2]
...
arr[499]
```

The array access mechanism uses the index to locate the element.

Therefore:

```text
arr[i] → O(1)
```

This is one of the biggest advantages of arrays.

### Important DSA Connection

When a problem says:

> "Access the element at index `i`."

Think immediately:

```text
Array → direct indexed access → O(1)
```

---

# 18. 📦 1D Array Memory

Consider:

```java
int[] arr = {10, 20, 30};
```

Conceptual model:

```text
Stack                         Heap

arr ───────────────────────► Array Object

                              ┌────┬────┬────┐
                              │ 10 │ 20 │ 30 │
                              └────┴────┴────┘
                               0    1    2
```

The reference variable points to the array object.

---

# 19. 🧩 2D Array Memory

Consider:

```java
int[][] arr = {
    {1, 2, 3},
    {4, 5, 6}
};
```

A common beginner misconception is:

```text
arr ─────► one giant rectangular block
```

Conceptually, Java instead has:

```text
arr
 │
 ▼
Outer Array

┌─────────┬─────────┐
│ ref     │ ref     │
└────┬────┴────┬────┘
     │         │
     ▼         ▼
  Row 0      Row 1

[1][2][3]  [4][5][6]
```

The outer array contains references to inner arrays.

---

# 20. 🧠 2D Array Is an Array of Arrays

This is one of the most important interview concepts.

Given:

```java
int[][] arr;
```

The outer array contains elements of type:

```text
int[]
```

Therefore:

```text
int[][]
```

can be understood conceptually as:

```text
array
  ↓
arrays
  ↓
int values
```

So:

```java
arr[0]
```

is an `int[]`.

And:

```java
arr[0][1]
```

is an `int`.

---

# 21. 🪚 Jagged Array Memory

Consider:

```java
int[][] arr = new int[3][];

arr[0] = new int[2];
arr[1] = new int[4];
arr[2] = new int[3];
```

Conceptually:

```text
arr
 │
 ▼
Outer Array

┌────────┬────────┬────────┐
│ ref    │ ref    │ ref    │
└───┬────┴───┬────┴───┬────┘
    │        │        │
    ▼        ▼        ▼

 [ ][ ]  [ ][ ][ ][ ]  [ ][ ][ ]
```

Different rows can have different lengths.

Therefore:

```java
arr[0].length
```

returns:

```text
2
```

```java
arr[1].length
```

returns:

```text
4
```

```java
arr[2].length
```

returns:

```text
3
```

---

# 22. 🔍 2D Array Reference Diagram

Consider:

```java
int[][] matrix = {
    {10, 20},
    {30, 40},
    {50, 60}
};
```

Conceptually:

```text
Stack

matrix
   │
   │ reference
   ▼

Heap

Outer Array

┌─────────┬─────────┬─────────┐
│ row ref │ row ref │ row ref │
└────┬────┴────┬────┴────┬────┘
     │         │         │
     ▼         ▼         ▼

   Row 0     Row 1     Row 2

 [10,20]   [30,40]   [50,60]
```

Important:

```java
matrix[0]
```

returns the reference to Row 0.

And:

```java
matrix[0][1]
```

accesses:

```text
Row 0 → index 1
```

which gives:

```text
20
```

---

# 23. ⚖️ Primitive vs Reference Arrays

## Primitive Array

```java
int[] arr = {10, 20, 30};
```

The array contains primitive values:

```text
[10][20][30]
```

## Reference Array

```java
String[] arr = {"A", "B", "C"};
```

The array contains references:

```text
[ref][ref][ref]
  │    │    │
  ▼    ▼    ▼
 "A"  "B"  "C"
```

The array itself is still an object.

---

# 24. 👨‍💻 Array of Objects

Consider:

```java
class Student {

    String name;

    Student(String name) {
        this.name = name;
    }
}
```

Now:

```java
Student[] students = new Student[3];
```

Initially:

```text
students
   ↓
[null][null][null]
```

Create objects:

```java
students[0] = new Student("A");
students[1] = new Student("B");
```

Now conceptually:

```text
students
   ↓
[ref][ref][null]
  │    │
  ▼    ▼
Student Student
  A      B
```

Important:

```java
new Student[3]
```

does **not** create three `Student` objects.

It creates an array containing three `Student` references.

---

# 25. 🔤 Array of Strings

Example:

```java
String[] names = new String[3];
```

Initially:

```text
[null][null][null]
```

Then:

```java
names[0] = "Yash";
names[1] = "Rahul";
names[2] = "Aman";
```

Conceptually:

```text
names
  ↓
[ref][ref][ref]
  │    │    │
  ▼    ▼    ▼
"Yash" "Rahul" "Aman"
```

Remember:

> The array contains references to `String` objects.

---

# 26. 🚫 Null References

Consider:

```java
int[] arr = null;
```

Here:

```text
arr
 ↓
null
```

The variable does not refer to any array object.

If we execute:

```java
System.out.println(arr.length);
```

Java throws:

```text
NullPointerException
```

Similarly:

```java
arr[0]
```

also causes:

```text
NullPointerException
```

because there is no array object being referenced.

---

# 27. 🗑️ Garbage Collection and Arrays

Arrays are objects.

Therefore, they are eligible for garbage collection when they become unreachable.

Example:

```java
int[] arr = new int[1000000];
```

Later:

```java
arr = null;
```

If there are no other references to the array, the array object becomes unreachable.

Conceptually:

```text
Before:

arr ─────► [large array]
```

After:

```text
arr ─────► null

[large array] ← unreachable
```

It may eventually be reclaimed by the garbage collector.

Important:

> `arr = null` does not immediately destroy the array.

It simply removes that particular reference.

---

# 28. 📞 Array Memory and Method Calls

Consider:

```java
static void change(int[] arr) {
    arr[0] = 100;
}

public static void main(String[] args) {

    int[] nums = {10, 20, 30};

    change(nums);

    System.out.println(nums[0]);
}
```

Output:

```text
100
```

Why?

Because the method receives a copy of the reference to the same array object.

Conceptually:

```text
main:

nums ───────┐
            ▼
          [10,20,30]
            ▲
            │
change: arr ┘
```

Inside the method:

```java
arr[0] = 100;
```

changes the shared array object.

---

# 29. 🎯 Java Is Pass-by-Value

This is an extremely important interview topic.

Java is always:

> **Pass-by-value.**

When an array is passed to a method, Java passes a copy of the reference value.

Example:

```java
int[] nums = {10, 20, 30};

change(nums);
```

Conceptually:

```text
nums = reference A

method receives:

arr = copy of reference A
```

So:

```text
nums ───────┐
            ▼
          [10,20,30]
            ▲
            │
arr ────────┘
```

Both references point to the same object.

---

# 30. ✏️ Changing Array Elements in a Method

Example:

```java
static void change(int[] arr) {
    arr[0] = 999;
}

public static void main(String[] args) {

    int[] nums = {10, 20, 30};

    change(nums);

    System.out.println(nums[0]);
}
```

Output:

```text
999
```

The reference itself was passed by value, but the copied reference still points to the same array.

Therefore, modifying the object is visible to the caller.

---

# 31. 🔄 Reassigning an Array Reference in a Method

Now consider:

```java
static void change(int[] arr) {
    arr = new int[]{100, 200, 300};
}

public static void main(String[] args) {

    int[] nums = {10, 20, 30};

    change(nums);

    System.out.println(nums[0]);
}
```

Output:

```text
10
```

Why?

Because:

```text
arr
```

inside the method is only a local copy of the reference.

Initially:

```text
nums ───────┐
            ▼
          [10,20,30]
            ▲
            │
arr ────────┘
```

After:

```java
arr = new int[]{100, 200, 300};
```

we get:

```text
nums ───────────► [10,20,30]

arr ────────────► [100,200,300]
```

The original `nums` reference still points to the original array.

---

# 32. ⚠️ Common Memory Mistakes

## ❌ Mistake 1 — Assignment Creates a New Array

Wrong assumption:

```java
int[] a = {1, 2, 3};
int[] b = a;
```

This does not create two arrays.

There is only one array object.

---

## ❌ Mistake 2 — Object Array Creates Objects

Wrong assumption:

```java
Student[] students = new Student[5];
```

It does not create five `Student` objects.

It creates five `Student` references initialized to `null`.

---

## ❌ Mistake 3 — `length()` Is a Method

Wrong:

```java
arr.length()
```

Correct:

```java
arr.length
```

---

## ❌ Mistake 4 — 2D Array Is Always One Block

Java's 2D array is an array of arrays.

Rows are separate array objects.

---

## ❌ Mistake 5 — `null` Immediately Destroys an Object

Wrong:

```java
arr = null;
```

This does not immediately destroy the array.

The object becomes eligible for GC when it is unreachable.

---

## ❌ Mistake 6 — Java Passes Arrays by Reference

Technically incorrect.

Java passes the array reference **by value**.

---

# 33. 🚨 Interview Traps

## Trap 1

What happens here?

```java
int[] a = {1, 2, 3};
int[] b = a;
```

Answer:

> Both references point to the same array.

---

## Trap 2

What does this create?

```java
Student[] students = new Student[5];
```

Answer:

> One array object containing five `Student` references, initially `null`.

---

## Trap 3

What happens here?

```java
int[] arr = null;

System.out.println(arr.length);
```

Answer:

```text
NullPointerException
```

---

## Trap 4

Difference between:

```java
int[] a = new int[3];
```

and:

```java
int[] b = a;
```

First:

```text
new int[3]
```

creates an array object.

Second:

```text
b = a
```

copies the reference.

---

## Trap 5

Why does modifying an array inside a method affect the original array?

Because the method receives a copy of the reference pointing to the same array object.

---

## Trap 6

Why doesn't reassigning the parameter affect the caller?

Because the parameter contains a separate copy of the reference.

---

# 34. 🧠 DSA Connection

Arrays are one of the most important data structures in DSA.

Many DSA problems are fundamentally about:

- Indexing
- Traversal
- Searching
- Sorting
- Prefix information
- Frequency counting
- Window maintenance
- Pointer movement
- In-place modification
- Subarrays
- Subsequences
- Matrix traversal

Understanding memory gives you the foundation for understanding why these algorithms have particular time and space complexities.

### Core Array Complexity

| Operation | Typical Complexity |
|---|---:|
| Access `arr[i]` | O(1) |
| Update `arr[i]` | O(1) |
| Traverse | O(n) |
| Search unsorted | O(n) |
| Binary search sorted | O(log n) |
| Copy array | O(n) |
| Reverse | O(n) |
| Insert at end with free space | O(1) |
| Insert at beginning | O(n) |
| Delete from beginning | O(n) |

> Complexity can depend on the exact operation and data structure implementation.

---

# 35. 🧩 DSA Patterns Related to Arrays

The most important array-related DSA patterns are:

1. Traversal
2. Two Pointers
3. Sliding Window
4. Prefix Sum
5. Hashing / Frequency Map
6. Binary Search
7. Sorting + Scanning
8. In-Place Modification
9. Kadane's Algorithm
10. Difference Array
11. Monotonic Stack
12. Matrix Traversal
13. Greedy Array Processing
14. Partitioning
15. Cyclic / Index Placement

The goal is not to memorize solutions.

The goal is to recognize the **shape of the problem**.

---

# 36. 🔎 How to Identify Array DSA Patterns

When you see an array problem, ask these questions in order.

### Question 1 — Do I simply need to inspect every element?

Think:

```text
Traversal
```

Typical clue:

> Find maximum, minimum, sum, count, etc.

---

### Question 2 — Is the array sorted?

Think:

```text
Binary Search
```

or:

```text
Two Pointers
```

depending on the problem.

---

### Question 3 — Is the problem asking about a pair?

Think:

```text
Two Pointers
```

or:

```text
Hashing
```

---

### Question 4 — Does the problem mention a contiguous subarray?

Think:

```text
Sliding Window
```

or:

```text
Prefix Sum
```

---

### Question 5 — Does it repeatedly ask for a range sum?

Think:

```text
Prefix Sum
```

---

### Question 6 — Does the problem ask for frequency/count occurrences?

Think:

```text
HashMap
```

or:

```text
Frequency Array
```

---

### Question 7 — Is the array sorted or can I sort it?

Think:

```text
Sorting + Two Pointers
```

or:

```text
Sorting + Greedy
```

---

### Question 8 — Must I modify the same array without using extra space?

Think:

```text
In-Place Algorithm
```

---

### Question 9 — Is it asking for maximum subarray sum?

Think:

```text
Kadane's Algorithm
```

---

### Question 10 — Is it a matrix/grid?

Think:

```text
2D Array / Matrix Traversal
```

---

# 37. 🔥 DSA Pattern 1 — Traversal

### Recognition

Use traversal when you simply need to inspect elements.

Typical keywords:

- Find maximum
- Find minimum
- Count
- Sum
- Check condition
- Print elements
- Find first occurrence

Example:

```java
int[] arr = {4, 7, 2, 9, 1};

int max = arr[0];

for (int i = 1; i < arr.length; i++) {
    if (arr[i] > max) {
        max = arr[i];
    }
}

System.out.println(max);
```

### Complexity

```text
Time: O(n)
Space: O(1)
```

### How to Think

Ask:

> Do I need information from every element?

If yes, start with a traversal.

### Practice Questions

- Find maximum element
- Find minimum element
- Find sum of array
- Count even numbers
- Find second largest
- Check if array is sorted
- Find missing number

---

# 38. 👉 DSA Pattern 2 — Two Pointers

Two pointers usually means maintaining two indexes.

Common forms:

```text
left → 
       ← right
```

or:

```text
slow →
fast  →
```

### Recognition

Look for:

- Sorted array
- Pair problems
- Reverse
- Remove duplicates
- Partitioning
- Opposite-end processing

Example:

```java
int[] arr = {1, 2, 3, 4, 6};

int left = 0;
int right = arr.length - 1;
int target = 7;

while (left < right) {

    int sum = arr[left] + arr[right];

    if (sum == target) {
        System.out.println("Pair found");
        break;
    }

    if (sum < target) {
        left++;
    } else {
        right--;
    }
}
```

### Why It Works

Because the array is sorted.

If:

```text
sum < target
```

we need a larger value, so:

```text
left++
```

If:

```text
sum > target
```

we need a smaller value, so:

```text
right--
```

### Complexity

```text
Time: O(n)
Space: O(1)
```

### Practice Questions

- Two Sum II
- Two Sum in sorted array
- 3Sum
- Container With Most Water
- Remove Duplicates from Sorted Array
- Move Zeroes
- Reverse Array
- Valid Palindrome

---

# 39. 🪟 DSA Pattern 3 — Sliding Window

Sliding Window is primarily used when the problem involves a **contiguous range**.

### Recognition Keywords

Look for:

- Subarray
- Contiguous
- Window
- Longest
- Shortest
- Maximum/minimum sum
- At most K
- Exactly K

Example:

> Find the maximum sum of any subarray of size `k`.

```java
int[] arr = {2, 1, 5, 1, 3, 2};
int k = 3;

int windowSum = 0;

for (int i = 0; i < k; i++) {
    windowSum += arr[i];
}

int maxSum = windowSum;

for (int i = k; i < arr.length; i++) {

    windowSum += arr[i];
    windowSum -= arr[i - k];

    maxSum = Math.max(maxSum, windowSum);
}

System.out.println(maxSum);
```

### Key Idea

Instead of recalculating:

```text
[2,1,5]
[1,5,1]
[5,1,3]
[1,3,2]
```

we remove the outgoing element and add the incoming element.

### Complexity

```text
Time: O(n)
Space: O(1)
```

### Practice Questions

- Maximum sum subarray of size K
- Maximum average subarray
- Longest substring without repeating characters
- Minimum size subarray sum
- Longest subarray with at most K distinct values

---

# 40. ➕ DSA Pattern 4 — Prefix Sum

Prefix Sum is useful when we repeatedly need information about ranges.

### Recognition

Look for:

- Range sum
- Multiple subarray sum queries
- Sum from `L` to `R`
- Subarray sum
- Cumulative information

Example:

```java
int[] arr = {2, 4, 6, 8};

int[] prefix = new int[arr.length];

prefix[0] = arr[0];

for (int i = 1; i < arr.length; i++) {
    prefix[i] = prefix[i - 1] + arr[i];
}
```

Now range sum can be calculated efficiently.

For range `[L, R]`:

```text
if L == 0:

prefix[R]

otherwise:

prefix[R] - prefix[L - 1]
```

### Complexity

Building prefix:

```text
O(n)
```

Each range query:

```text
O(1)
```

### Practice Questions

- Range Sum Query
- Subarray Sum Equals K
- Find pivot index
- Equilibrium index
- Product Except Self

---

# 41. 🧮 DSA Pattern 5 — Hashing / Frequency

Use hashing when you need to remember previously seen values.

### Recognition

Keywords:

- Duplicate
- Frequency
- Count occurrences
- Pair
- Seen before
- Unique
- First repeating
- Complement

Example:

```java
import java.util.HashMap;

int[] arr = {2, 7, 11, 2, 7};

HashMap<Integer, Integer> frequency = new HashMap<>();

for (int value : arr) {
    frequency.put(value, frequency.getOrDefault(value, 0) + 1);
}
```

### Why?

Instead of repeatedly scanning the array:

```text
O(n²)
```

we can often remember information using hashing.

Typical average complexity:

```text
Time: O(n)
Space: O(n)
```

### Practice Questions

- Two Sum
- Contains Duplicate
- Majority Element
- Top K Frequent Elements
- Longest Consecutive Sequence
- Subarray Sum Equals K

---

# 42. 🔍 DSA Pattern 6 — Binary Search

### Recognition

Think Binary Search when:

- Array is sorted
- Search space is ordered
- Need logarithmic search
- Find first/last occurrence
- Search answer space

Basic example:

```java
int[] arr = {1, 3, 5, 7, 9, 11};

int target = 7;

int left = 0;
int right = arr.length - 1;

while (left <= right) {

    int mid = left + (right - left) / 2;

    if (arr[mid] == target) {
        System.out.println("Found");
        break;
    }

    if (arr[mid] < target) {
        left = mid + 1;
    } else {
        right = mid - 1;
    }
}
```

### Complexity

```text
Time: O(log n)
Space: O(1)
```

### Important Thinking Pattern

Binary search works because each comparison eliminates a portion of the search space.

### Practice Questions

- Binary Search
- Search Insert Position
- First and Last Position
- Search in Rotated Sorted Array
- Find Minimum in Rotated Sorted Array
- Peak Element
- Square Root
- Koko Eating Bananas

---

# 43. 🔃 DSA Pattern 7 — Sorting + Array

Sometimes sorting makes a problem easier.

### Recognition

Consider sorting when:

- Order does not matter initially
- Need duplicates together
- Need pair/triplet relationships
- Need greedy processing
- Need interval ordering

Example:

```java
import java.util.Arrays;

int[] arr = {5, 2, 8, 1, 3};

Arrays.sort(arr);
```

Now:

```text
[1, 2, 3, 5, 8]
```

This can enable:

```text
Two Pointers
Greedy
Duplicate removal
Interval processing
```

### Trade-off

Sorting generally costs:

```text
O(n log n)
```

So do not sort automatically.

Ask:

> Does sorting give me useful structure that reduces the remaining work?

### Practice Questions

- 3Sum
- Merge Intervals
- Meeting Rooms
- Largest Number
- Sort Colors
- Non-overlapping Intervals

---

# 44. 🛠️ DSA Pattern 8 — In-Place Modification

An in-place algorithm modifies the original array instead of creating another array.

### Recognition

Look for:

> Use O(1) extra space.

or:

> Modify the array in-place.

Example:

```java
int[] arr = {0, 1, 0, 3, 12};

int write = 0;

for (int read = 0; read < arr.length; read++) {

    if (arr[read] != 0) {
        arr[write] = arr[read];
        write++;
    }
}

while (write < arr.length) {
    arr[write] = 0;
    write++;
}
```

### Pattern

```text
read  → scans
write → places
```

This is commonly called a **slow-fast pointer** technique.

### Practice Questions

- Move Zeroes
- Remove Duplicates
- Remove Element
- Sort Colors
- Remove Duplicates from Sorted Array
- Partition Array

---

# 45. 📈 DSA Pattern 9 — Kadane's Algorithm

Kadane's Algorithm is used for:

> Maximum subarray sum.

### Recognition

If the question says:

> Find the maximum sum of a contiguous subarray.

Immediately think:

```text
Kadane's Algorithm
```

Example:

```java
int[] arr = {-2, 1, -3, 4, -1, 2, 1, -5, 4};

int current = arr[0];
int best = arr[0];

for (int i = 1; i < arr.length; i++) {

    current = Math.max(arr[i], current + arr[i]);

    best = Math.max(best, current);
}

System.out.println(best);
```

### Core Idea

At every position ask:

> Is it better to extend the previous subarray or start a new subarray here?

### Complexity

```text
Time: O(n)
Space: O(1)
```

### Practice Questions

- Maximum Subarray
- Maximum Circular Subarray
- Maximum Product Subarray
- Best Time to Buy and Sell Stock

---

# 46. 🧮 DSA Pattern 10 — Difference Array

Difference arrays are useful when there are many range updates.

Suppose we repeatedly perform:

```text
Add X to every element from L to R
```

Instead of updating every element individually, use a difference array.

Basic idea:

```text
diff[L] += X
diff[R + 1] -= X
```

Then reconstruct the actual values using a prefix sum.

### Recognition

Look for:

- Many range updates
- Add value to interval
- Update `[L, R]`
- Apply many modifications before reading final values

### Complexity

Instead of:

```text
O(number_of_updates × range_length)
```

the technique can reduce update processing to approximately:

```text
O(number_of_updates + n)
```

### Practice Questions

- Range Addition
- Corporate Flight Bookings
- Car Pooling
- Range Increment Queries

---

# 47. 📚 DSA Pattern 11 — Monotonic Stack Connection

Some array problems are not solved with arrays alone.

They use an array plus a stack.

### Recognition

Look for:

- Next greater element
- Next smaller element
- Previous greater element
- Previous smaller element
- Temperature waiting days
- Histogram
- Nearest greater/smaller value

Example:

```java
import java.util.Stack;

int[] arr = {2, 1, 2, 4, 3};

Stack<Integer> stack = new Stack<>();
int[] result = new int[arr.length];

for (int i = arr.length - 1; i >= 0; i--) {

    while (!stack.isEmpty() && stack.peek() <= arr[i]) {
        stack.pop();
    }

    result[i] = stack.isEmpty() ? -1 : stack.peek();

    stack.push(arr[i]);
}
```

### Recognition Trick

If the question asks:

> "nearest greater/smaller"

think:

```text
Monotonic Stack
```

### Practice Questions

- Next Greater Element
- Daily Temperatures
- Next Smaller Element
- Largest Rectangle in Histogram
- Stock Span

---

# 48. 🧮 DSA Pattern 12 — Matrix / 2D Array

For 2D arrays, first identify the traversal direction.

Common patterns:

```text
Row-wise
Column-wise
Diagonal
Spiral
Boundary
BFS/DFS
```

Example:

```java
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};

for (int row = 0; row < matrix.length; row++) {

    for (int col = 0; col < matrix[row].length; col++) {

        System.out.print(matrix[row][col] + " ");
    }

    System.out.println();
}
```

### Recognition

If the problem gives:

```text
grid
matrix
rows
columns
cells
```

think:

```text
2D array
```

Then determine whether it requires:

```text
Simple traversal
Boundary traversal
Spiral traversal
DFS
BFS
```

### Practice Questions

- Matrix Diagonal Sum
- Spiral Matrix
- Rotate Image
- Set Matrix Zeroes
- Search a 2D Matrix
- Number of Islands
- Flood Fill

---

# 49. 🧠 How to Think About an Array Problem

Use this process in interviews.

## Step 1 — Understand the Input

Ask:

```text
1D or 2D?
Sorted or unsorted?
Positive, negative, or both?
Duplicates?
```

---

## Step 2 — Understand the Required Output

Ask:

```text
Single value?
Index?
Pair?
Subarray?
Modified array?
Count?
```

---

## Step 3 — Look for Keywords

| Keyword | Possible Pattern |
|---|---|
| Sorted | Binary Search / Two Pointers |
| Pair | Hashing / Two Pointers |
| Triplet | Sorting + Two Pointers |
| Contiguous | Sliding Window / Prefix Sum |
| Range Sum | Prefix Sum |
| Frequency | HashMap / Frequency Array |
| Duplicate | Hashing / Sorting |
| Maximum Subarray | Kadane |
| In-place | Two Pointers |
| Next Greater | Monotonic Stack |
| Many Range Updates | Difference Array |
| Matrix | 2D Traversal |
| Search | Binary Search / Hashing |
| Longest Subarray | Sliding Window / Prefix Sum |

---

## Step 4 — Start With Brute Force

Do not immediately jump to an optimized solution.

First ask:

> What is the simplest correct solution?

Then analyze:

```text
Time Complexity
Space Complexity
```

---

## Step 5 — Find Repeated Work

This is where optimization usually begins.

Ask:

> Am I calculating the same thing repeatedly?

If yes, consider:

```text
Hashing
Prefix Sum
Sliding Window
Sorting
Two Pointers
Dynamic Programming
```

---

## Step 6 — Check Space Constraint

If interviewer says:

> O(1) extra space

think:

```text
In-place
Two Pointers
Index Manipulation
```

---

## Step 7 — Verify Edge Cases

Always test:

```text
Empty array
One element
Two elements
All same values
Already sorted
Reverse sorted
Duplicates
Negative values
Very large values
```

---

# 50. 🔥 Important DSA Questions

## Beginner

### 1. Find Maximum Element

Pattern:

```text
Traversal
```

### 2. Find Minimum Element

Pattern:

```text
Traversal
```

### 3. Reverse an Array

Pattern:

```text
Two Pointers
```

### 4. Check if Array Is Sorted

Pattern:

```text
Traversal
```

### 5. Find Second Largest

Pattern:

```text
Traversal
```

### 6. Move Zeroes

Pattern:

```text
Two Pointers / In-place
```

### 7. Remove Duplicates from Sorted Array

Pattern:

```text
Two Pointers
```

---

## Intermediate

### 8. Two Sum

Patterns:

```text
Hashing
Two Pointers after sorting
```

### 9. Best Time to Buy and Sell Stock

Pattern:

```text
Greedy / One-pass traversal
```

### 10. Maximum Subarray

Pattern:

```text
Kadane
```

### 11. Product of Array Except Self

Patterns:

```text
Prefix / Suffix
```

### 12. Subarray Sum Equals K

Pattern:

```text
Prefix Sum + HashMap
```

### 13. 3Sum

Pattern:

```text
Sorting + Two Pointers
```

### 14. Container With Most Water

Pattern:

```text
Two Pointers
```

### 15. Longest Subarray With Condition

Possible patterns:

```text
Sliding Window
Prefix Sum
Hashing
```

---

## Advanced

### 16. Trapping Rain Water

Patterns:

```text
Two Pointers
Prefix/Suffix
Monotonic Stack
```

### 17. Largest Rectangle in Histogram

Pattern:

```text
Monotonic Stack
```

### 18. First Missing Positive

Pattern:

```text
In-place Index Placement
```

### 19. Merge Intervals

Pattern:

```text
Sorting + Greedy
```

### 20. Search in Rotated Sorted Array

Pattern:

```text
Binary Search
```

### 21. Median of Two Sorted Arrays

Pattern:

```text
Binary Search
```

### 22. Maximum Product Subarray

Pattern:

```text
Dynamic Programming / Kadane variation
```

---

# 51. 🚨 DSA Interview Traps

## Trap 1 — O(n²) Two Sum

A brute-force solution:

```java
for (int i = 0; i < arr.length; i++) {

    for (int j = i + 1; j < arr.length; j++) {

        if (arr[i] + arr[j] == target) {
            // pair found
        }
    }
}
```

Complexity:

```text
O(n²)
```

Possible optimization:

```text
HashMap → O(n) average
```

---

## Trap 2 — Recalculating Every Window

If you calculate every subarray sum from scratch, you may create:

```text
O(n²)
```

work.

Recognize:

```text
Contiguous + fixed window
```

and consider:

```text
Sliding Window
```

---

## Trap 3 — Sorting Automatically

Sorting can cost:

```text
O(n log n)
```

Do not sort unless it gives useful structure.

---

## Trap 4 — Using Extra Array Unnecessarily

If the interviewer asks for:

```text
O(1) extra space
```

creating:

```java
int[] result = new int[arr.length];
```

may violate the intended constraint.

Think:

```text
In-place
```

---

## Trap 5 — Confusing Subarray and Subsequence

### Subarray

Elements must be contiguous.

Example:

```text
[2,3,4]
```

from:

```text
[1,2,3,4,5]
```

### Subsequence

Elements do not need to be contiguous.

Example:

```text
[1,3,5]
```

from:

```text
[1,2,3,4,5]
```

This distinction strongly affects the algorithm.

---

# 52. 🔥 Top 20 Java Array Interview Questions

## Q1. Are arrays objects in Java?

Yes. Arrays are objects created at runtime.

---

## Q2. Where are array objects allocated?

Array objects are allocated in the heap.

---

## Q3. What does an array variable store?

It stores a reference to an array object.

---

## Q4. Does `int[] a = new int[5]` store five integers directly in `a`?

No.

`a` is a reference to the array object.

---

## Q5. What does `new` do with arrays?

It creates a new array object and initializes its elements with default values.

---

## Q6. What is the default value of an `int` array?

```text
0
```

---

## Q7. What is the default value of a reference-type array?

```text
null
```

---

## Q8. Is `length` a method?

No.

It is an array property.

Correct:

```java
arr.length
```

Incorrect:

```java
arr.length()
```

---

## Q9. What happens when two array variables are assigned?

They can refer to the same array object.

Example:

```java
int[] a = {1, 2};
int[] b = a;
```

---

## Q10. Does `b = a` create an array copy?

No.

It copies the reference.

---

## Q11. How do you create a separate array copy?

For example:

```java
int[] b = a.clone();
```

Other approaches include:

```java
int[] b = java.util.Arrays.copyOf(a, a.length);
```

or:

```java
System.arraycopy(a, 0, b, 0, a.length);
```

---

## Q12. Why is array access O(1)?

Because indexed array access directly identifies the requested position.

---

## Q13. Is a 2D array one rectangular object?

Conceptually, no.

It is an array whose elements are references to other arrays.

---

## Q14. Can Java have jagged arrays?

Yes.

---

## Q15. Can an array contain `null`?

Reference-type arrays can.

Example:

```java
String[] arr = new String[3];
```

---

## Q16. Can primitive arrays contain `null`?

No.

For example:

```java
int[] arr = new int[3];
```

contains `0`, not `null`.

---

## Q17. Is Java pass-by-reference?

No.

Java is pass-by-value.

For arrays, the value being passed is a copy of the array reference.

---

## Q18. Why can a method modify the caller's array?

Because both references point to the same array object.

---

## Q19. Does assigning `null` immediately destroy an array?

No.

The array becomes eligible for garbage collection when it becomes unreachable.

---

## Q20. What is the most important array-memory concept?

Remember:

```text
Reference variable
       ↓
Array object
```

The variable and object are conceptually separate.

---

# 53. 🎤 30-Second Interview Answer

> **In Java, arrays are objects and are allocated at runtime. An array variable stores a reference to the array object rather than containing the entire array itself. When one array variable is assigned to another, only the reference value is copied, so both variables can refer to the same array. A 2D array is actually an array of arrays, which is why Java supports jagged arrays. Array indexing provides O(1) access, and when an array is passed to a method, Java passes the reference by value.**

---

# 54. 🧾 Cheat Sheet

| Concept | Key Point |
|---|---|
| Array | Object |
| Array allocation | Heap |
| Variable | Holds array reference |
| `new` | Creates array object |
| `arr.length` | Number of elements |
| Indexing | Starts at 0 |
| Access | O(1) |
| Assignment | Copies reference |
| `clone()` | Creates separate array |
| Reference array | Stores references |
| 2D array | Array of arrays |
| Jagged array | Different row lengths |
| `null` | No referenced object |
| GC | Reclaims unreachable objects |
| Java parameter passing | Pass-by-value |
| Array argument | Copy of reference |

---

# 55. 🧠 Memory Tricks

## 🔥 Trick 1 — Variable vs Object

Remember:

```text
Variable
   ↓
Reference
   ↓
Object
```

For arrays:

```text
arr
 ↓
reference
 ↓
array object
```

---

## 🔥 Trick 2 — `new` Means New Object

Whenever you see:

```java
new int[5]
```

think:

```text
NEW ARRAY OBJECT
```

---

## 🔥 Trick 3 — Assignment Doesn't Mean Copy

This:

```java
b = a;
```

means:

```text
Copy reference
```

not:

```text
Copy array
```

---

## 🔥 Trick 4 — 2D Array

Remember:

```text
int[][]
   ↓
array
   ↓
arrays
   ↓
values
```

---

## 🔥 Trick 5 — Object Array

Remember:

```java
Student[] students = new Student[5];
```

means:

```text
5 references
```

NOT:

```text
5 Student objects
```

---

## 🔥 Trick 6 — Method Passing

Remember:

```text
Java → Pass-by-value

Array argument
      ↓
copy of reference
      ↓
same array object
```

---

# 56. ✅ Final Revision Checklist

Before moving to the next topic, make sure you can explain:

```text
[ ] Arrays are objects
[ ] Arrays are allocated in the heap
[ ] Array variables contain references
[ ] Difference between reference and object
[ ] What new does
[ ] Default values of array elements
[ ] What length means
[ ] Why indexing is O(1)
[ ] Difference between reference copy and array copy
[ ] Why a = b does not clone an array
[ ] How clone() creates a separate array
[ ] Primitive arrays
[ ] Reference arrays
[ ] Arrays of objects
[ ] Arrays of Strings
[ ] Null array references
[ ] Garbage collection of arrays
[ ] 2D array memory
[ ] Array of arrays
[ ] Jagged array memory
[ ] Java pass-by-value
[ ] Passing arrays to methods
[ ] Modifying arrays inside methods
[ ] Reassigning array references inside methods

DSA:

[ ] Array traversal
[ ] Two pointers
[ ] Sliding window
[ ] Prefix sum
[ ] Hashing / frequency
[ ] Binary search
[ ] Sorting + scanning
[ ] In-place modification
[ ] Kadane's algorithm
[ ] Difference array
[ ] Monotonic stack
[ ] Matrix traversal
[ ] How to identify subarray problems
[ ] How to identify sorted-array problems
[ ] How to identify pair problems
[ ] How to identify range-query problems
[ ] How to identify frequency problems
[ ] How to identify O(1)-space problems
[ ] How to distinguish subarray vs subsequence
```

---

# 57. 🏆 MASTER MEMORY CARD

```text
┌────────────────────────────────────────────────────┐
│              ARRAY MEMORY IN JAVA                   │
├────────────────────────────────────────────────────┤
│ Array → Object                                     │
│ Array object → Heap                                │
│ Variable → Reference                               │
│ new → Creates array object                         │
│ a = b → Reference copy                             │
│ clone() → Separate array object                    │
│ arr[i] → O(1) access                               │
│ 2D array → Array of arrays                         │
│ Jagged array → Different row lengths               │
│ Object[] → Array of references                     │
│ Java → Pass-by-value                               │
│ Array argument → Copy of reference                 │
├────────────────────────────────────────────────────┤
│                DSA PATTERN MAP                     │
├────────────────────────────────────────────────────┤
│ Simple scan → Traversal                            │
│ Sorted + pair → Two Pointers                       │
│ Contiguous range → Sliding Window / Prefix Sum     │
│ Frequency → HashMap / Frequency Array              │
│ Sorted search → Binary Search                      │
│ Need ordering → Sorting                            │
│ O(1) extra space → In-place / Two Pointers         │
│ Max subarray → Kadane                              │
│ Many range updates → Difference Array              │
│ Next greater/smaller → Monotonic Stack             │
│ Matrix/Grid → 2D Array Traversal                   │
└────────────────────────────────────────────────────┘
```

---

# ⭐ ONE-LINE INTERVIEW DEFINITION

> **In Java, an array is a runtime-created object accessed through a reference variable, with indexed O(1) access, while multidimensional arrays are implemented as arrays whose elements are references to other arrays.**

---

# 🧠 ONE-LINE DSA DEFINITION

> **Arrays provide constant-time indexed access and form the foundation for patterns such as traversal, two pointers, sliding window, prefix sum, hashing, binary search, in-place algorithms, and matrix traversal.**

---

# 🔗 ARRAY FOLDER PROGRESS

```text
05-Arrays/
│
├── 01-Array-Introduction.md
├── 02-One-Dimensional-Array.md
├── 03-Multidimensional-Array.md
├── 04-Array-Memory.md          ← YOU ARE HERE
├── 05-Arrays-Class.md
└── 06-Array-Interview-Questions.md
```

### Next

> **05 — Arrays Class**

This will cover:

- `java.util.Arrays`
- Sorting
- Searching
- Copying
- Filling
- Comparing
- `toString()`
- `deepToString()`
- `equals()`
- `deepEquals()`
- `binarySearch()`
- `copyOf()`
- `copyOfRange()`
- `fill()`
- `sort()`
- `parallelSort()`
- `mismatch()`
- `compare()`
- Important internal behavior
- Time complexities
- Interview traps
- DSA usage
- DSA patterns
- Practice questions
- How to identify when `Arrays` APIs can simplify a DSA solution
````
