# 🔢 Multidimensional Array in Java

> **A multidimensional array in Java is an array whose elements are themselves arrays. The most common multidimensional array is a 2D array, commonly used to represent rows and columns.**

---

# 📌 Table of Contents

1. [What is a Multidimensional Array?](#1--what-is-a-multidimensional-array)
2. [Why Do We Need Multidimensional Arrays?](#2--why-do-we-need-multidimensional-arrays)
3. [2D Array](#3--2d-array)
4. [Declaration](#4--declaration)
5. [Creation](#5--creation)
6. [Declaration + Creation](#6--declaration--creation)
7. [Initialization](#7--initialization)
8. [Direct Initialization](#8--direct-initialization)
9. [Rows and Columns](#9--rows-and-columns)
10. [Accessing Elements](#10--accessing-elements)
11. [Updating Elements](#11--updating-elements)
12. [Traversing a 2D Array](#12--traversing-a-2d-array)
13. [Nested for Loop](#13--nested-for-loop)
14. [Enhanced for Loop](#14--enhanced-for-loop)
15. [Taking 2D Array Input](#15--taking-2d-array-input)
16. [Printing a 2D Array](#16--printing-a-2d-array)
17. [Understanding `arr.length`](#17--understanding-arrlength)
18. [Understanding `arr[i].length`](#18--understanding-arri-length)
19. [How a 2D Array Actually Works](#19--how-a-2d-array-actually-works)
20. [Array of Arrays](#20--array-of-arrays)
21. [Jagged Arrays](#21--jagged-arrays)
22. [Creating a Jagged Array](#22--creating-a-jagged-array)
23. [Initializing a Jagged Array](#23--initializing-a-jagged-array)
24. [3D Arrays](#24--3d-arrays)
25. [General Multidimensional Syntax](#25--general-multidimensional-syntax)
26. [Finding Sum](#26--finding-sum)
27. [Finding Maximum and Minimum](#27--finding-maximum-and-minimum)
28. [Searching in a 2D Array](#28--searching-in-a-2d-array)
29. [Matrix Addition](#29--matrix-addition)
30. [Transpose of a Matrix](#30--transpose-of-a-matrix)
31. [Diagonal Elements](#31--diagonal-elements)
32. [Passing 2D Array to a Method](#32--passing-2d-array-to-a-method)
33. [Returning a 2D Array](#33--returning-a-2d-array)
34. [Multidimensional Array and `null`](#34--multidimensional-array-and-null)
35. [Common Exceptions](#35--common-exceptions)
36. [Time Complexity](#36--time-complexity)
37. [DSA Patterns](#37--dsa-patterns)
38. [How to Identify 2D Array Problems](#38--how-to-identify-2d-array-problems)
39. [DSA Problem-Solving Approach](#39--dsa-problem-solving-approach)
40. [DSA Coding Problems](#40--dsa-coding-problems)
41. [Common Mistakes](#41--common-mistakes)
42. [Interview Traps](#42--interview-traps)
43. [Top 20 Interview Questions](#43--top-20-interview-questions)
44. [30-Second Interview Answer](#44--30-second-interview-answer)
45. [Cheat Sheet](#45--cheat-sheet)
46. [Memory Tricks](#46--memory-tricks)
47. [Final Revision Checklist](#47--final-revision-checklist)
48. [Master Memory Card](#48--master-memory-card)
49. [One-Line Interview Definition](#49--one-line-interview-definition)

---

# 1. 🔹 What is a Multidimensional Array?

A multidimensional array is an array whose elements are themselves arrays.

The most commonly used multidimensional array is a **two-dimensional array**.

Example:

```java
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};
```

Visual representation:

```text
       Column

         0   1   2

     ┌───┬───┬───┐
  0  │ 1 │ 2 │ 3 │
     ├───┼───┼───┤
  1  │ 4 │ 5 │ 6 │
     ├───┼───┼───┤
  2  │ 7 │ 8 │ 9 │
     └───┴───┴───┘
```

To access `5`:

```java
matrix[1][1];
```

Here:

```text
1 → row
1 → column
```

Therefore:

```text
matrix[row][column]
```

---

# 2. 🎯 Why Do We Need Multidimensional Arrays?

A one-dimensional array is useful for linear data:

```text
10 20 30 40 50
```

But some data naturally has multiple dimensions.

## 🏫 Student Marks

```text
Student     Maths   Java   DBMS

   0          80     90     75
   1          70     85     80
   2          95     88     90
```

This can be represented as:

```java
int[][] marks;
```

## 🧮 Matrix

```text
1 2 3
4 5 6
7 8 9
```

## 🎮 Game Board

```text
X . X
. X .
X . .
```

## 🖼️ Image Data

An image can be represented using rows and columns of pixels.

## 🌳 DSA Usage

2D arrays are extremely common in:

- Matrix problems
- Grid problems
- Dynamic Programming
- Graph adjacency matrices
- Game boards
- BFS/DFS grid problems
- Prefix sum problems
- Island/grid traversal
- Simulation problems

---

# 3. 🧱 2D Array

A two-dimensional array contains multiple rows.

Example:

```java
int[][] arr = new int[3][4];
```

This represents:

```text
3 rows
4 columns
```

Visual:

```text
         Columns

         0   1   2   3

     ┌───┬───┬───┬───┐
  0  │ 0 │ 0 │ 0 │ 0 │
     ├───┼───┼───┼───┤
  1  │ 0 │ 0 │ 0 │ 0 │
     ├───┼───┼───┼───┤
  2  │ 0 │ 0 │ 0 │ 0 │
     └───┴───┴───┴───┘
```

Total elements:

```text
3 × 4 = 12
```

Because this is an `int` array, every element initially contains:

```text
0
```

---

# 4. 📝 Declaration

Recommended syntax:

```java
int[][] arr;
```

Other valid syntaxes:

```java
int arr[][];
```

```java
int[] arr[];
```

All three represent a two-dimensional array reference.

Recommended:

```java
int[][] arr;
```

because it clearly communicates that the array has two dimensions.

---

# 5. 🏗️ Creation

Use the `new` keyword:

```java
int[][] arr = new int[3][4];
```

This creates:

```text
3 row arrays
4 elements in each row
```

Conceptually:

```text
arr
 ↓
Row 0 → [0, 0, 0, 0]
Row 1 → [0, 0, 0, 0]
Row 2 → [0, 0, 0, 0]
```

---

# 6. 🧩 Declaration + Creation

These can be written separately.

## Declaration

```java
int[][] arr;
```

## Creation

```java
arr = new int[3][4];
```

## Combined

```java
int[][] arr = new int[3][4];
```

---

# 7. 🎯 Initialization

Elements can be initialized individually.

```java
int[][] arr = new int[2][3];

arr[0][0] = 10;
arr[0][1] = 20;
arr[0][2] = 30;

arr[1][0] = 40;
arr[1][1] = 50;
arr[1][2] = 60;
```

Result:

```text
10 20 30
40 50 60
```

---

# 8. ⚡ Direct Initialization

We can initialize the complete array directly.

```java
int[][] arr = {
    {10, 20, 30},
    {40, 50, 60}
};
```

Visual:

```text
10 20 30
40 50 60
```

Another example:

```java
int[][] matrix = {
    {1, 2},
    {3, 4},
    {5, 6}
};
```

This contains:

```text
3 rows
2 columns
```

---

# 9. 📐 Rows and Columns

Suppose:

```java
int[][] arr = new int[3][4];
```

Then:

```text
Number of rows = 3
Number of columns = 4
```

To get the number of rows:

```java
arr.length;
```

Result:

```text
3
```

To get the number of columns in a particular row:

```java
arr[0].length;
```

Result:

```text
4
```

Important:

```text
arr.length
    ↓
number of rows

arr[i].length
    ↓
number of elements in row i
```

---

# 10. 👀 Accessing Elements

Syntax:

```text
arr[row][column]
```

Example:

```java
int[][] arr = {
    {10, 20, 30},
    {40, 50, 60},
    {70, 80, 90}
};
```

Access:

```text
arr[0][0] → 10
arr[0][1] → 20
arr[1][0] → 40
arr[1][2] → 60
arr[2][1] → 80
arr[2][2] → 90
```

Example:

```java
System.out.println(arr[2][2]);
```

Output:

```text
90
```

---

# 11. ✏️ Updating Elements

A 2D array is mutable.

Example:

```java
int[][] arr = {
    {10, 20},
    {30, 40}
};

arr[1][0] = 99;
```

Before:

```text
10 20
30 40
```

After:

```text
10 20
99 40
```

---

# 12. 🔄 Traversing a 2D Array

Since we have two indexes, we generally use nested loops.

Example:

```java
int[][] arr = {
    {10, 20, 30},
    {40, 50, 60}
};

for (int i = 0; i < arr.length; i++) {
    for (int j = 0; j < arr[i].length; j++) {
        System.out.print(arr[i][j] + " ");
    }

    System.out.println();
}
```

Output:

```text
10 20 30
40 50 60
```

---

# 13. 🔁 Nested `for` Loop

This is one of the most important patterns for 2D arrays.

```java
for (int i = 0; i < arr.length; i++) {
    for (int j = 0; j < arr[i].length; j++) {
        System.out.println(arr[i][j]);
    }
}
```

Think of it like this:

```text
i → row
j → column
```

Therefore:

```text
arr[i][j]
```

means:

> Element at row `i` and column `j`.

## 🧠 Dry Run

Suppose:

```java
int[][] arr = {
    {10, 20, 30},
    {40, 50, 60}
};
```

### First iteration

```text
i = 0
```

Inner loop:

```text
j = 0 → arr[0][0] → 10
j = 1 → arr[0][1] → 20
j = 2 → arr[0][2] → 30
```

### Second iteration

```text
i = 1
```

Inner loop:

```text
j = 0 → arr[1][0] → 40
j = 1 → arr[1][1] → 50
j = 2 → arr[1][2] → 60
```

Traversal:

```text
10 → 20 → 30 → 40 → 50 → 60
```

---

# 14. 🚀 Enhanced `for` Loop

A 2D array can also be traversed using nested enhanced `for` loops.

```java
int[][] arr = {
    {10, 20, 30},
    {40, 50, 60}
};

for (int[] row : arr) {
    for (int value : row) {
        System.out.print(value + " ");
    }

    System.out.println();
}
```

Output:

```text
10 20 30
40 50 60
```

## How Does This Work?

Outer loop:

```java
for (int[] row : arr)
```

means:

> Take each row array from `arr`.

Inner loop:

```java
for (int value : row)
```

means:

> Take each value from the current row.

So:

```text
arr
 ↓
row
 ↓
value
```

---

# 15. ⌨️ Taking 2D Array Input

Example:

```java
Scanner sc = new Scanner(System.in);

int rows = sc.nextInt();
int columns = sc.nextInt();

int[][] arr = new int[rows][columns];

for (int i = 0; i < rows; i++) {
    for (int j = 0; j < columns; j++) {
        arr[i][j] = sc.nextInt();
    }
}
```

Input:

```text
3 4
1 2 3 4
5 6 7 8
9 10 11 12
```

Array:

```text
1  2  3  4
5  6  7  8
9 10 11 12
```

---

# 16. 🖨️ Printing a 2D Array

Using nested loops:

```java
for (int i = 0; i < arr.length; i++) {
    for (int j = 0; j < arr[i].length; j++) {
        System.out.print(arr[i][j] + " ");
    }

    System.out.println();
}
```

Output:

```text
10 20 30
40 50 60
```

## Using `Arrays.deepToString()`

For displaying a multidimensional array:

```java
System.out.println(Arrays.deepToString(arr));
```

Output:

```text
[[10, 20, 30], [40, 50, 60]]
```

`deepToString()` is useful because a multidimensional array contains nested arrays.

The `Arrays` class is covered in:

```text
05-Arrays-Class.md
```

---

# 17. 📏 Understanding `arr.length`

Consider:

```java
int[][] arr = {
    {1, 2, 3},
    {4, 5, 6}
};
```

Then:

```java
arr.length;
```

is:

```text
2
```

Why?

Because the outer array contains two row arrays.

Conceptually:

```text
arr
 ↓
[ row0, row1 ]
```

Therefore:

```text
arr.length = 2
```

---

# 18. 📏 Understanding `arr[i].length`

For:

```java
int[][] arr = {
    {1, 2, 3},
    {4, 5, 6}
};
```

We have:

```text
arr[0].length → 3
arr[1].length → 3
```

Therefore:

```text
arr.length
```

means:

> Number of rows.

And:

```text
arr[i].length
```

means:

> Number of elements in row `i`.

This becomes especially important for jagged arrays.

---

# 19. 🧠 How a 2D Array Actually Works

This is an important interview concept.

Beginners often imagine a 2D array as one large rectangular block.

But internally, Java's 2D array is an:

> **Array of arrays.**

Consider:

```java
int[][] arr = {
    {10, 20},
    {30, 40},
    {50, 60}
};
```

Conceptually:

```text
arr
 ↓
┌────────┬────────┬────────┐
│ row 0  │ row 1  │ row 2  │
│   ↓    │   ↓    │   ↓    │
└───┼────┴───┼────┴───┼────┘
    ↓        ↓        ↓
 [10,20]  [30,40]  [50,60]
```

The outer array contains references to the row arrays.

So:

```text
int[][]
```

is essentially:

```text
array of int[]
```

---

# 20. 🧩 Array of Arrays

Think of:

```java
int[][] arr;
```

as:

```text
Array
  |
  ├── int[]
  ├── int[]
  └── int[]
```

Each element of the outer array is itself an integer array.

This is why Java can support rows with different lengths.

---

# 21. 🪚 Jagged Arrays

A jagged array is a multidimensional array where different rows can have different lengths.

Example:

```java
int[][] arr = {
    {1, 2},
    {3, 4, 5},
    {6},
    {7, 8, 9, 10}
};
```

Visual:

```text
Row 0 → 1 2
Row 1 → 3 4 5
Row 2 → 6
Row 3 → 7 8 9 10
```

This is completely valid Java.

---

# 22. 🏗️ Creating a Jagged Array

First create the outer array:

```java
int[][] arr = new int[3][];
```

Notice:

```text
new int[3][]
```

The number of rows is specified:

```text
3
```

But the length of each row is not specified yet.

Then create rows individually:

```java
arr[0] = new int[2];
arr[1] = new int[4];
arr[2] = new int[3];
```

Now:

```text
Row 0 → 2 elements
Row 1 → 4 elements
Row 2 → 3 elements
```

---

# 23. 🎯 Initializing a Jagged Array

Example:

```java
int[][] arr = new int[3][];

arr[0] = new int[]{10, 20};
arr[1] = new int[]{30, 40, 50};
arr[2] = new int[]{60, 70, 80, 90};
```

Result:

```text
10 20
30 40 50
60 70 80 90
```

Traversal must use:

```java
for (int i = 0; i < arr.length; i++) {
    for (int j = 0; j < arr[i].length; j++) {
        System.out.print(arr[i][j] + " ");
    }

    System.out.println();
}
```

Notice:

```text
arr[i].length
```

instead of:

```text
arr[0].length
```

This is important because each row can have a different size.

---

# 24. 🧊 3D Arrays

Java supports more than two dimensions.

Example:

```java
int[][][] arr = new int[2][3][4];
```

This can be visualized as:

```text
2 blocks
3 rows per block
4 columns per row
```

Conceptually:

```text
Block 0

    Row 0 → 4 elements
    Row 1 → 4 elements
    Row 2 → 4 elements

Block 1

    Row 0 → 4 elements
    Row 1 → 4 elements
    Row 2 → 4 elements
```

Access:

```text
arr[block][row][column]
```

Example:

```java
arr[1][2][3];
```

---

# 25. 🧱 General Multidimensional Syntax

## 1D Array

```java
int[] arr;
```

## 2D Array

```java
int[][] arr;
```

## 3D Array

```java
int[][][] arr;
```

## 4D Array

```java
int[][][][] arr;
```

The pattern continues.

However, in practical Java development and DSA, 1D and 2D arrays are much more commonly used.

---

# 26. ➕ Finding Sum

Example:

```java
int[][] arr = {
    {10, 20, 30},
    {40, 50, 60}
};

int sum = 0;

for (int i = 0; i < arr.length; i++) {
    for (int j = 0; j < arr[i].length; j++) {
        sum += arr[i][j];
    }
}

System.out.println(sum);
```

Output:

```text
210
```

Calculation:

```text
10 + 20 + 30 + 40 + 50 + 60 = 210
```

### DSA Pattern

This is the **complete matrix traversal pattern**.

Whenever every cell contributes to the answer:

```java
for (int i = 0; i < matrix.length; i++) {
    for (int j = 0; j < matrix[i].length; j++) {
        // process matrix[i][j]
    }
}
```

---

# 27. 📈 Finding Maximum and Minimum

## Maximum

```java
int[][] arr = {
    {10, 50, 30},
    {80, 20, 60}
};

int max = arr[0][0];

for (int i = 0; i < arr.length; i++) {
    for (int j = 0; j < arr[i].length; j++) {
        if (arr[i][j] > max) {
            max = arr[i][j];
        }
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
int min = arr[0][0];

for (int i = 0; i < arr.length; i++) {
    for (int j = 0; j < arr[i].length; j++) {
        if (arr[i][j] < min) {
            min = arr[i][j];
        }
    }
}

System.out.println(min);
```

Output:

```text
10
```

### DSA Pattern

This is the **running best / running answer pattern**.

Think:

```text
Initialize answer
        ↓
Visit every cell
        ↓
Compare current value
        ↓
Update answer
```

---

# 28. 🔍 Searching in a 2D Array

Suppose:

```java
int[][] arr = {
    {10, 20, 30},
    {40, 50, 60},
    {70, 80, 90}
};
```

Target:

```text
50
```

Linear search through the 2D array:

```java
int target = 50;

for (int i = 0; i < arr.length; i++) {
    for (int j = 0; j < arr[i].length; j++) {
        if (arr[i][j] == target) {
            System.out.println(
                "Found at row " + i + ", column " + j
            );
        }
    }
}
```

Output:

```text
Found at row 1, column 1
```

### DSA Pattern

This is a **grid linear search**.

Use it when:

- No sorted property exists.
- You need to inspect every cell.
- The question asks whether a target exists.
- The question asks for the coordinates of a target.

Complexity:

```text
O(R × C)
```

---

# 29. ➕ Matrix Addition

Two matrices can be added when they have the same dimensions.

Example:

```text
Matrix A:

1 2
3 4

Matrix B:

5 6
7 8
```

Result:

```text
6  8
10 12
```

Code:

```java
int[][] a = {
    {1, 2},
    {3, 4}
};

int[][] b = {
    {5, 6},
    {7, 8}
};

int[][] result = new int[2][2];

for (int i = 0; i < a.length; i++) {
    for (int j = 0; j < a[i].length; j++) {
        result[i][j] = a[i][j] + b[i][j];
    }
}
```

### DSA Pattern

This is an **element-wise matrix operation**.

General idea:

```text
result[i][j] = operation(a[i][j], b[i][j])
```

This pattern also appears in:

- Matrix addition
- Matrix subtraction
- Grid comparison
- Pixel manipulation
- State comparison

---

# 30. 🔄 Transpose of a Matrix

Transpose means:

> Convert rows into columns and columns into rows.

Original:

```text
1 2 3
4 5 6
```

Transpose:

```text
1 4
2 5
3 6
```

For an `m × n` matrix, transpose becomes:

```text
n × m
```

Code:

```java
int[][] arr = {
    {1, 2, 3},
    {4, 5, 6}
};

int rows = arr.length;
int columns = arr[0].length;

int[][] transpose = new int[columns][rows];

for (int i = 0; i < rows; i++) {
    for (int j = 0; j < columns; j++) {
        transpose[j][i] = arr[i][j];
    }
}
```

Result:

```text
1 4
2 5
3 6
```

### DSA Pattern

Important coordinate transformation:

```text
Original:
(i, j)

Transpose:
(j, i)
```

Whenever a problem says:

- transpose
- swap rows and columns
- mirror across diagonal

think about changing:

```text
matrix[i][j]
```

into:

```text
matrix[j][i]
```

---

# 31. 🔲 Diagonal Elements

For a square matrix:

```text
1 2 3
4 5 6
7 8 9
```

Main diagonal:

```text
1
  5
    9
```

Indexes:

```text
[0][0]
[1][1]
[2][2]
```

The pattern is:

```text
arr[i][i]
```

Example:

```java
for (int i = 0; i < arr.length; i++) {
    System.out.println(arr[i][i]);
}
```

Output:

```text
1
5
9
```

## Secondary Diagonal

For:

```text
1 2 3
4 5 6
7 8 9
```

Secondary diagonal:

```text
    3
  5
7
```

Indexes:

```text
[0][2]
[1][1]
[2][0]
```

General pattern:

```text
arr[i][n - 1 - i]
```

Example:

```java
int n = arr.length;

for (int i = 0; i < n; i++) {
    System.out.println(arr[i][n - 1 - i]);
}
```

### DSA Pattern

For square matrices, memorize:

```text
Main diagonal:
[i][i]

Secondary diagonal:
[i][n - 1 - i]
```

---

# 32. 📤 Passing 2D Array to a Method

A 2D array can be passed to a method.

Example:

```java
static void printMatrix(int[][] arr) {
    for (int i = 0; i < arr.length; i++) {
        for (int j = 0; j < arr[i].length; j++) {
            System.out.print(arr[i][j] + " ");
        }

        System.out.println();
    }
}
```

Call:

```java
int[][] matrix = {
    {1, 2},
    {3, 4}
};

printMatrix(matrix);
```

---

# 33. 📥 Returning a 2D Array

A method can return a 2D array.

Example:

```java
static int[][] createMatrix() {
    int[][] matrix = {
        {1, 2},
        {3, 4}
    };

    return matrix;
}
```

Call:

```java
int[][] result = createMatrix();
```

Now:

```text
result
```

refers to the returned 2D array.

---

# 34. 🚫 Multidimensional Array and `null`

Because a 2D array is an array of row references, individual rows can also be `null`.

Example:

```java
int[][] arr = new int[3][];
```

At this point:

```text
arr[0] = null
arr[1] = null
arr[2] = null
```

If we do:

```java
System.out.println(arr[0].length);
```

we get:

```text
NullPointerException
```

because `arr[0]` does not currently refer to an inner array.

We can create a row:

```java
arr[0] = new int[3];
```

Now:

```java
arr[0].length;
```

is:

```text
3
```

---

# 35. 💥 Common Exceptions

## 1. `ArrayIndexOutOfBoundsException`

Example:

```java
int[][] arr = {
    {10, 20},
    {30, 40}
};

System.out.println(arr[2][0]);
```

Valid row indexes are:

```text
0
1
```

So row `2` is invalid.

---

## 2. `NullPointerException`

Example:

```java
int[][] arr = new int[3][];

System.out.println(arr[0].length);
```

`arr[0]` is `null`.

---

## 3. `ArrayStoreException`

This can occur when an incompatible object is stored into an array whose runtime component type does not allow it.

Example:

```java
Object[][] arr = new String[2][];

arr[0] = new String[2];

Object[] row = arr[0];

row[0] = 100;
```

The runtime row is actually a `String[]`.

Therefore storing an `Integer` can cause:

```text
ArrayStoreException
```

---

# 36. ⏱️ Time Complexity

Suppose a matrix contains:

```text
R rows
C columns
```

| Operation | Complexity |
|---|---:|
| Access element | O(1) |
| Update element | O(1) |
| Traverse | O(R × C) |
| Search | O(R × C) |
| Find sum | O(R × C) |
| Find maximum | O(R × C) |
| Matrix addition | O(R × C) |
| Transpose | O(R × C) |

For a square matrix of size `n × n`:

```text
O(R × C)
```

becomes:

```text
O(n²)
```

---

# 37. 🧠 DSA Patterns

2D arrays are extremely important in DSA because many **matrix and grid problems** are based on a small number of reusable patterns.

The goal is not to memorize every problem.

The goal is to recognize the underlying pattern.

---

## Pattern 1 — Complete Matrix Traversal

### Idea

Visit every cell exactly once.

```java
for (int i = 0; i < matrix.length; i++) {
    for (int j = 0; j < matrix[i].length; j++) {
        // process matrix[i][j]
    }
}
```

### Identify It When

The question says:

- process every element
- calculate sum
- find maximum
- find minimum
- count cells
- search every cell
- modify every cell

### Complexity

```text
O(R × C)
```

---

# Pattern 2 — Coordinate-Based Access

Every matrix element is represented by:

```text
(row, column)
```

Code:

```java
int value = matrix[row][column];
```

### Identify It When

The problem talks about:

- coordinates
- rows
- columns
- positions
- neighboring cells
- moving through a grid

---

# Pattern 3 — Directional Traversal

Grid problems often require movement:

```text
Up
Down
Left
Right
```

The four direction vectors are commonly represented as:

```java
int[][] directions = {
    {-1, 0},
    {1, 0},
    {0, -1},
    {0, 1}
};
```

For a cell:

```text
(row, col)
```

the neighboring cells become:

```text
(row - 1, col) → Up
(row + 1, col) → Down
(row, col - 1) → Left
(row, col + 1) → Right
```

### Identify It When

The problem contains words such as:

- neighboring
- adjacent
- connected
- move
- direction
- grid
- island
- maze
- shortest path

This pattern commonly leads to:

- BFS
- DFS
- flood fill

---

# Pattern 4 — Boundary Checking

When moving through a grid, always verify that the position is valid.

```java
if (row >= 0 &&
    row < matrix.length &&
    col >= 0 &&
    col < matrix[row].length) {
    
    // valid cell
}
```

### Identify It When

You are calculating:

```text
row ± 1
col ± 1
```

because those operations can move outside the matrix.

---

# Pattern 5 — Main Diagonal

Main diagonal:

```text
matrix[i][i]
```

Example:

```java
for (int i = 0; i < matrix.length; i++) {
    System.out.println(matrix[i][i]);
}
```

### Identify It When

The problem says:

- main diagonal
- primary diagonal
- diagonal from top-left
- diagonal elements

---

# Pattern 6 — Secondary Diagonal

Secondary diagonal:

```text
matrix[i][n - 1 - i]
```

Example:

```java
int n = matrix.length;

for (int i = 0; i < n; i++) {
    System.out.println(matrix[i][n - 1 - i]);
}
```

### Identify It When

The problem says:

- secondary diagonal
- anti-diagonal
- top-right to bottom-left

---

# Pattern 7 — Row-Wise Processing

Sometimes the problem asks you to process each row independently.

Example:

```java
for (int i = 0; i < matrix.length; i++) {
    int rowSum = 0;

    for (int j = 0; j < matrix[i].length; j++) {
        rowSum += matrix[i][j];
    }

    System.out.println(rowSum);
}
```

### Identify It When

The problem asks:

- sum of each row
- maximum element in every row
- row with maximum sum
- process students row by row

---

# Pattern 8 — Column-Wise Processing

Process each column independently.

For a rectangular matrix:

```java
for (int j = 0; j < matrix[0].length; j++) {
    int columnSum = 0;

    for (int i = 0; i < matrix.length; i++) {
        columnSum += matrix[i][j];
    }

    System.out.println(columnSum);
}
```

### Identify It When

The problem asks:

- sum of each column
- maximum column
- column-wise statistics
- process vertical data

For jagged arrays, column-wise traversal requires additional care because rows may have different lengths.

---

# Pattern 9 — Transpose / Coordinate Swap

Original:

```text
matrix[i][j]
```

Transpose:

```text
transpose[j][i]
```

Code:

```java
transpose[j][i] = matrix[i][j];
```

### Identify It When

The problem says:

- transpose
- rows become columns
- columns become rows
- reflect across the main diagonal

---

# Pattern 10 — Layer / Boundary Traversal

Some matrix problems process the outer boundary first and then move inward.

Typical examples:

- Spiral Matrix
- Spiral traversal
- Rotate matrix
- Print matrix layer by layer

Think in terms of:

```text
top
bottom
left
right
```

Example state:

```java
int top = 0;
int bottom = matrix.length - 1;
int left = 0;
int right = matrix[0].length - 1;
```

### Identify It When

The problem mentions:

- spiral
- boundary
- clockwise
- anticlockwise
- layers
- rings

---

# Pattern 11 — Matrix Rotation

A common approach for rotating a square matrix by 90 degrees clockwise is:

```text
1. Transpose
2. Reverse every row
```

Example:

```java
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};

for (int i = 0; i < matrix.length; i++) {
    for (int j = i + 1; j < matrix.length; j++) {
        int temp = matrix[i][j];
        matrix[i][j] = matrix[j][i];
        matrix[j][i] = temp;
    }
}

for (int i = 0; i < matrix.length; i++) {
    int left = 0;
    int right = matrix[i].length - 1;

    while (left < right) {
        int temp = matrix[i][left];
        matrix[i][left] = matrix[i][right];
        matrix[i][right] = temp;

        left++;
        right--;
    }
}
```

### Identify It When

The problem says:

- rotate matrix
- rotate 90 degrees
- rotate clockwise
- rotate in-place

---

# Pattern 12 — Grid BFS / DFS

A grid can be treated as a graph.

Each cell can represent a node.

Adjacent cells can represent edges.

Typical movement:

```java
int[][] directions = {
    {-1, 0},
    {1, 0},
    {0, -1},
    {0, 1}
};
```

### Identify It When

Look for:

- islands
- connected components
- shortest path
- flood fill
- maze
- rotten oranges
- surrounded regions
- number of regions
- connected cells

This usually means:

```text
2D Grid
   ↓
Neighbors
   ↓
BFS / DFS
```

---

# Pattern 13 — Prefix Sum Matrix

A 2D prefix sum allows repeated rectangular-sum queries to be answered efficiently.

For a prefix matrix:

```text
prefix[i][j]
```

stores cumulative information from the top-left region.

Typical formula:

```java
prefix[i][j] =
    matrix[i][j]
    + prefix[i - 1][j]
    + prefix[i][j - 1]
    - prefix[i - 1][j - 1];
```

### Identify It When

The problem contains:

- many rectangle sum queries
- submatrix sum
- sum of a rectangular region
- repeated range queries

The important idea is:

```text
Many queries
    ↓
Precompute
    ↓
Answer each query faster
```

---

# Pattern 14 — Dynamic Programming on a Grid

A 2D array is frequently used as a DP table.

Example:

```java
int[][] dp = new int[rows][columns];
```

Each cell stores the answer to a smaller subproblem.

### Identify It When

Look for:

- minimum path
- maximum path
- number of ways
- grid paths
- minimum cost
- previous cell
- state
- recurrence

Typical relationship:

```text
dp[i][j]
depends on
dp[i-1][j]
dp[i][j-1]
```

---

# 38. 🔎 How to Identify 2D Array Problems

When you see a DSA problem, first identify the **shape of the data**.

## Step 1 — Is the input a grid or matrix?

Look for:

```text
matrix
grid
rows
columns
cells
board
table
```

If yes, think:

```text
2D array
```

---

## Step 2 — Does Every Cell Need Processing?

If yes:

```text
Nested loops
```

Pattern:

```java
for (int i = 0; i < matrix.length; i++) {
    for (int j = 0; j < matrix[i].length; j++) {
        // process
    }
}
```

---

## Step 3 — Does the Problem Mention Neighbors?

If yes, think:

```text
Directions
```

Usually:

```java
int[][] directions = {
    {-1, 0},
    {1, 0},
    {0, -1},
    {0, 1}
};
```

Then consider:

```text
BFS / DFS
```

---

## Step 4 — Does It Mention Diagonals?

Think:

```text
Main diagonal:
[i][i]

Secondary diagonal:
[i][n - 1 - i]
```

---

## Step 5 — Does It Mention Rotation?

Think:

```text
Transpose
+
Reverse
```

---

## Step 6 — Does It Mention Spiral?

Think:

```text
top
bottom
left
right
```

Use boundary/layer traversal.

---

## Step 7 — Does It Ask Many Rectangle Sums?

Think:

```text
2D Prefix Sum
```

---

## Step 8 — Does It Ask Paths or Minimum/Maximum Cost?

Think:

```text
2D Dynamic Programming
```

---

## Step 9 — Does It Ask Connectivity?

Think:

```text
BFS / DFS
```

---

# 39. 🧠 DSA Problem-Solving Approach

When you get a matrix problem, do not immediately start coding.

Use this process.

```text
1. Understand dimensions
        ↓
2. Identify what one cell represents
        ↓
3. Determine movement
        ↓
4. Determine whether every cell is visited
        ↓
5. Identify the pattern
        ↓
6. Choose traversal
        ↓
7. Handle boundaries
        ↓
8. Analyze complexity
```

## Question 1 — What does one cell represent?

For example:

```text
matrix[i][j]
```

could represent:

- a number
- a character
- a blocked cell
- an island
- a cost
- a DP state

---

## Question 2 — Do I need neighbors?

If yes, identify directions.

```text
Up
Down
Left
Right
```

Possibly also diagonals:

```text
Top-left
Top-right
Bottom-left
Bottom-right
```

---

## Question 3 — Is the matrix sorted?

This is important.

If the matrix has a special ordering property, you may not need to inspect every cell.

For example, a sorted matrix can sometimes support:

```text
O(R + C)
```

instead of:

```text
O(R × C)
```

---

## Question 4 — Is the answer local or global?

### Local

Each cell is processed independently.

Example:

```text
sum
count
maximum
minimum
```

Usually:

```text
O(R × C)
```

### Global

Cells depend on other cells.

Examples:

```text
path
connectivity
minimum cost
number of ways
```

Consider:

```text
BFS
DFS
DP
```

---

# 40. 💻 DSA Coding Problems

## Problem 1 — Print a Matrix

```java
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};

for (int i = 0; i < matrix.length; i++) {
    for (int j = 0; j < matrix[i].length; j++) {
        System.out.print(matrix[i][j] + " ");
    }

    System.out.println();
}
```

Output:

```text
1 2 3
4 5 6
7 8 9
```

### Pattern

```text
Complete traversal
```

---

## Problem 2 — Find Sum

```java
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6}
};

int sum = 0;

for (int i = 0; i < matrix.length; i++) {
    for (int j = 0; j < matrix[i].length; j++) {
        sum += matrix[i][j];
    }
}

System.out.println(sum);
```

Output:

```text
21
```

### Pattern

```text
Complete traversal + running answer
```

---

## Problem 3 — Find Maximum

```java
int[][] matrix = {
    {10, 20, 30},
    {40, 50, 60}
};

int max = matrix[0][0];

for (int i = 0; i < matrix.length; i++) {
    for (int j = 0; j < matrix[i].length; j++) {
        if (matrix[i][j] > max) {
            max = matrix[i][j];
        }
    }
}

System.out.println(max);
```

Output:

```text
60
```

### Pattern

```text
Running maximum
```

---

## Problem 4 — Search an Element

```java
int[][] matrix = {
    {10, 20, 30},
    {40, 50, 60}
};

int target = 50;

for (int i = 0; i < matrix.length; i++) {
    for (int j = 0; j < matrix[i].length; j++) {
        if (matrix[i][j] == target) {
            System.out.println(
                "Found at row " + i + ", column " + j
            );
        }
    }
}
```

Output:

```text
Found at row 1, column 1
```

### Pattern

```text
Grid linear search
```

---

## Problem 5 — Print Main Diagonal

For:

```text
1 2 3
4 5 6
7 8 9
```

Code:

```java
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};

for (int i = 0; i < matrix.length; i++) {
    System.out.println(matrix[i][i]);
}
```

Output:

```text
1
5
9
```

### Pattern

```text
matrix[i][i]
```

---

## Problem 6 — Create a Jagged Array

```java
int[][] arr = new int[3][];

arr[0] = new int[]{1, 2};
arr[1] = new int[]{3, 4, 5};
arr[2] = new int[]{6, 7, 8, 9};

for (int i = 0; i < arr.length; i++) {
    for (int j = 0; j < arr[i].length; j++) {
        System.out.print(arr[i][j] + " ");
    }

    System.out.println();
}
```

Output:

```text
1 2
3 4 5
6 7 8 9
```

### Pattern

```text
Array of arrays
```

---

## Problem 7 — Row Sum

Given a matrix, print the sum of every row.

```java
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};

for (int i = 0; i < matrix.length; i++) {
    int sum = 0;

    for (int j = 0; j < matrix[i].length; j++) {
        sum += matrix[i][j];
    }

    System.out.println(sum);
}
```

Output:

```text
6
15
24
```

### Pattern

```text
Outer loop = row
Inner loop = elements of current row
```

---

## Problem 8 — Column Sum

```java
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};

for (int j = 0; j < matrix[0].length; j++) {
    int sum = 0;

    for (int i = 0; i < matrix.length; i++) {
        sum += matrix[i][j];
    }

    System.out.println(sum);
}
```

Output:

```text
12
15
18
```

### Pattern

```text
Outer loop = column
Inner loop = rows
```

---

## Problem 9 — Main Diagonal Sum

```java
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};

int sum = 0;

for (int i = 0; i < matrix.length; i++) {
    sum += matrix[i][i];
}

System.out.println(sum);
```

Output:

```text
15
```

---

## Problem 10 — Secondary Diagonal Sum

```java
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};

int n = matrix.length;
int sum = 0;

for (int i = 0; i < n; i++) {
    sum += matrix[i][n - 1 - i];
}

System.out.println(sum);
```

Output:

```text
15
```

---

## Problem 11 — Count Even Numbers

```java
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};

int count = 0;

for (int i = 0; i < matrix.length; i++) {
    for (int j = 0; j < matrix[i].length; j++) {
        if (matrix[i][j] % 2 == 0) {
            count++;
        }
    }
}

System.out.println(count);
```

Output:

```text
4
```

### Pattern

```text
Traversal + condition + counter
```

---

## Problem 12 — Matrix Addition

```java
int[][] a = {
    {1, 2},
    {3, 4}
};

int[][] b = {
    {5, 6},
    {7, 8}
};

int[][] result = new int[a.length][a[0].length];

for (int i = 0; i < a.length; i++) {
    for (int j = 0; j < a[i].length; j++) {
        result[i][j] = a[i][j] + b[i][j];
    }
}
```

Result:

```text
6 8
10 12
```

---

## Problem 13 — Transpose

```java
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6}
};

int rows = matrix.length;
int columns = matrix[0].length;

int[][] transpose = new int[columns][rows];

for (int i = 0; i < rows; i++) {
    for (int j = 0; j < columns; j++) {
        transpose[j][i] = matrix[i][j];
    }
}
```

Result:

```text
1 4
2 5
3 6
```

---

## Problem 14 — Find Number of Islands

A classic grid problem.

Example:

```text
1 1 0
1 0 0
0 0 1
```

The problem asks for the number of connected groups of `1`s.

### Identification

The words:

```text
island
connected
adjacent
grid
```

should immediately make you think:

```text
DFS / BFS
```

Typical DFS structure:

```java
static void dfs(char[][] grid, int row, int col) {
    if (row < 0 ||
        row >= grid.length ||
        col < 0 ||
        col >= grid[row].length ||
        grid[row][col] != '1') {
        return;
    }

    grid[row][col] = '0';

    dfs(grid, row - 1, col);
    dfs(grid, row + 1, col);
    dfs(grid, row, col - 1);
    dfs(grid, row, col + 1);
}
```

The important DSA idea is:

```text
Current cell
     ↓
Visit neighbors
     ↓
Mark visited
     ↓
Continue
```

---

## Problem 15 — Rotate Matrix 90° Clockwise

Common in interviews.

The standard in-place strategy for a square matrix is:

```text
Transpose
    ↓
Reverse every row
```

Implementation:

```java
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};

int n = matrix.length;

for (int i = 0; i < n; i++) {
    for (int j = i + 1; j < n; j++) {
        int temp = matrix[i][j];
        matrix[i][j] = matrix[j][i];
        matrix[j][i] = temp;
    }
}

for (int i = 0; i < n; i++) {
    int left = 0;
    int right = n - 1;

    while (left < right) {
        int temp = matrix[i][left];
        matrix[i][left] = matrix[i][right];
        matrix[i][right] = temp;

        left++;
        right--;
    }
}
```

### Identification

If the problem says:

```text
rotate matrix 90 degrees clockwise
```

think:

```text
Transpose + Reverse rows
```

---

# 41. ⚠️ Common Mistakes

## ❌ Mistake 1 — Using `arr.length` for columns

Wrong for general 2D arrays:

```java
for (int j = 0; j < arr.length; j++) {
    // ...
}
```

Correct:

```java
for (int j = 0; j < arr[i].length; j++) {
    // ...
}
```

Why?

Because:

```text
arr.length
    ↓
number of rows
```

while:

```text
arr[i].length
    ↓
number of elements in row i
```

---

## ❌ Mistake 2 — Assuming every row has the same length

This is unsafe for jagged arrays.

Wrong assumption:

```text
arr[0].length == arr[1].length
```

It may be true for rectangular arrays, but not necessarily for jagged arrays.

---

## ❌ Mistake 3 — Forgetting the second index

For a 2D array:

```java
arr[i];
```

returns a row array.

To access an individual element:

```java
arr[i][j];
```

---

## ❌ Mistake 4 — Confusing row and column

Remember:

```text
arr[row][column]
```

So:

```java
arr[1][2];
```

means:

```text
row 1
column 2
```

---

## ❌ Mistake 5 — Forgetting zero-based indexing

For:

```java
int[][] arr = new int[3][4];
```

Valid rows:

```text
0, 1, 2
```

Valid columns:

```text
0, 1, 2, 3
```

---

## ❌ Mistake 6 — Assuming a 2D array is always rectangular

Java allows:

```java
int[][] arr = {
    {1, 2},
    {3, 4, 5}
};
```

Therefore:

```text
2D array ≠ necessarily rectangular matrix
```

---

## ❌ Mistake 7 — Accessing a null row

This can fail:

```java
int[][] arr = new int[3][];

System.out.println(arr[0][0]);
```

because:

```text
arr[0] == null
```

---

# 42. 🚨 Interview Traps

## Trap 1

What is:

```java
int[][] arr = new int[3][4];
```

Answer:

```text
3 rows
4 elements in each row
```

---

## Trap 2

What is:

```java
arr.length;
```

Answer:

> Number of rows.

---

## Trap 3

What is:

```java
arr[0].length;
```

Answer:

> Number of elements in row `0`.

---

## Trap 4

Is a 2D array actually a rectangular matrix internally?

Answer:

> Not necessarily. In Java, a 2D array is an array of arrays, so rows can have different lengths.

---

## Trap 5

Is this valid?

```java
int[][] arr = {
    {1, 2},
    {3, 4, 5}
};
```

Yes.

This is a jagged array.

---

## Trap 6

Is this valid?

```java
int[][] arr = new int[3][];
```

Yes.

The outer array has three row references, and individual rows can be created later.

---

## Trap 7

What is:

```java
arr[0];
```

Answer:

> It is a reference to the first row array.

---

## Trap 8

What is:

```java
arr[0][1];
```

Answer:

> The element at row `0`, column `1`.

---

## Trap 9

What is the difference between:

```java
arr.length
```

and:

```java
arr[0].length
```

Answer:

```text
arr.length       → number of rows
arr[0].length    → number of elements in row 0
```

---

## Trap 10

Can a 2D array have `null` rows?

Yes.

```java
int[][] arr = new int[3][];
```

Initially:

```text
arr[0] → null
arr[1] → null
arr[2] → null
```

---

# 43. 🔥 Top 20 Interview Questions

## Q1. What is a multidimensional array?

A multidimensional array is an array whose elements are themselves arrays.

---

## Q2. What is the most common multidimensional array?

A two-dimensional array.

---

## Q3. How do you declare a 2D array?

```java
int[][] arr;
```

---

## Q4. How do you create a 2D array?

```java
int[][] arr = new int[3][4];
```

---

## Q5. How do you access an element?

```java
arr[row][column];
```

---

## Q6. What does `arr.length` represent?

The number of rows in a 2D array.

---

## Q7. What does `arr[i].length` represent?

The number of elements in row `i`.

---

## Q8. Is a 2D array actually an array of arrays?

Yes.

---

## Q9. Can Java have jagged arrays?

Yes.

---

## Q10. What is a jagged array?

A multidimensional array where different rows can have different lengths.

---

## Q11. Can rows be created separately?

Yes.

Example:

```java
int[][] arr = new int[3][];

arr[0] = new int[2];
arr[1] = new int[4];
arr[2] = new int[3];
```

---

## Q12. How do you traverse a 2D array?

Usually with nested loops.

---

## Q13. What does `arr[i][j]` represent?

The element at row `i` and column `j`.

---

## Q14. Can a 2D array contain `null` rows?

Yes, especially when created like:

```java
new int[3][]
```

before individual rows are initialized.

---

## Q15. Can methods accept 2D arrays?

Yes.

Example:

```java
static void display(int[][] arr) {
    // ...
}
```

---

## Q16. Can methods return 2D arrays?

Yes.

Example:

```java
static int[][] createMatrix() {
    // ...
}
```

---

## Q17. What is the complexity of traversing an `R × C` matrix?

```text
O(R × C)
```

---

## Q18. What is a 3D array?

An array with three dimensions.

Example:

```java
int[][][] arr;
```

---

## Q19. What is the difference between `arr.length` and `arr[i].length`?

`arr.length` gives the number of rows, while `arr[i].length` gives the length of a particular row.

---

## Q20. Why can Java have jagged arrays?

Because a multidimensional array is an array of arrays, and each inner array can have a different length.

---

# 44. 🎤 30-Second Interview Answer

> **A multidimensional array in Java is an array whose elements are themselves arrays. The most common example is a 2D array, which is generally represented as rows and columns. We access an element using two indexes such as `arr[i][j]`. An important point is that Java's 2D arrays are actually arrays of arrays, which means Java supports jagged arrays where different rows can have different lengths.**

---

# 45. 🧾 Cheat Sheet

## Declaration

```java
int[][] arr;
```

## Creation

```java
int[][] arr = new int[3][4];
```

## Direct Initialization

```java
int[][] arr = {
    {1, 2},
    {3, 4}
};
```

## Access

```java
arr[i][j];
```

## Rows

```java
arr.length;
```

## Columns of Row `i`

```java
arr[i].length;
```

## Traverse

```java
for (int i = 0; i < arr.length; i++) {
    for (int j = 0; j < arr[i].length; j++) {
        // arr[i][j]
    }
}
```

## Enhanced Traversal

```java
for (int[] row : arr) {
    for (int value : row) {
        // value
    }
}
```

## Jagged Array

```java
int[][] arr = new int[3][];
```

## 3D Array

```java
int[][][] arr;
```

## Print Nested Array

```java
Arrays.deepToString(arr);
```

## Main Diagonal

```java
arr[i][i];
```

## Secondary Diagonal

```java
arr[i][n - 1 - i];
```

## Direction Array

```java
int[][] directions = {
    {-1, 0},
    {1, 0},
    {0, -1},
    {0, 1}
};
```

---

# 46. 🧠 Memory Tricks

## 🔥 Trick 1 — Remember the Index

For a 2D array:

```text
arr[row][column]
```

Think:

```text
First  → Row
Second → Column
```

---

## 🔥 Trick 2 — Remember the Length

```text
arr.length
    ↓
Rows
```

```text
arr[i].length
    ↓
Elements in row i
```

---

## 🔥 Trick 3 — Nested Loops

Think:

```text
Outer loop  → Rows
Inner loop  → Columns
```

Therefore:

```text
for each row
    for each column
        process element
```

---

## 🔥 Trick 4 — 2D Array Internals

Never think:

```text
2D array = one giant block
```

Think:

```text
2D array
    ↓
array of arrays
    ↓
row references
    ↓
individual row arrays
```

---

## 🔥 Trick 5 — Jagged Array

Remember:

```text
Different rows
      ↓
Different lengths
      ↓
Jagged array
```

---

## 🔥 Trick 6 — Dimensions

```text
int[]       → 1D
int[][]     → 2D
int[][][]   → 3D
```

---

## 🔥 Trick 7 — DSA Pattern Recognition

```text
Every cell
    ↓
Nested loops

Neighbors
    ↓
Directions
    ↓
BFS / DFS

Diagonal
    ↓
[i][i] or [i][n-1-i]

Rotate
    ↓
Transpose + Reverse

Spiral
    ↓
Top / Bottom / Left / Right

Many rectangle queries
    ↓
2D Prefix Sum

Path / minimum / maximum / ways
    ↓
2D DP
```

---

# 47. ✅ Final Revision Checklist

Before moving to the next topic, make sure you can explain:

```text
[ ] What is a multidimensional array?
[ ] What is a 2D array?
[ ] How to declare a 2D array?
[ ] How to create a 2D array?
[ ] How to initialize it?
[ ] How to access an element?
[ ] What does arr[i][j] mean?
[ ] What does arr.length mean?
[ ] What does arr[i].length mean?
[ ] How to traverse a 2D array?
[ ] Why do we use nested loops?
[ ] How to use enhanced for loops?
[ ] How to take 2D array input?
[ ] How to print a 2D array?
[ ] How does a 2D array work internally?
[ ] What is an array of arrays?
[ ] What is a jagged array?
[ ] How to create a jagged array?
[ ] What is a 3D array?
[ ] How to find sum?
[ ] How to find maximum?
[ ] How to search?
[ ] How to add matrices?
[ ] What is matrix transpose?
[ ] What are diagonal elements?
[ ] How to pass a 2D array to a method?
[ ] How to return a 2D array?
[ ] What exceptions can occur?
[ ] What is the traversal complexity?
[ ] Why are Java 2D arrays called arrays of arrays?
[ ] How to identify a grid problem?
[ ] When should I use BFS/DFS?
[ ] When should I use directions?
[ ] How do I recognize diagonal problems?
[ ] How do I recognize matrix rotation?
[ ] How do I recognize spiral traversal?
[ ] When should I think about 2D prefix sum?
[ ] When should I think about 2D DP?
```

---

# 48. 🏆 MASTER MEMORY CARD

```text
┌────────────────────────────────────────────────────┐
│          MULTIDIMENSIONAL ARRAY IN JAVA            │
├────────────────────────────────────────────────────┤
│ 2D array → array of arrays                         │
│ Access → arr[row][column]                          │
│ arr.length → number of rows                        │
│ arr[i].length → length of row i                   │
│ Traversal → nested loops                           │
│ Can contain jagged rows                            │
│ Can have null row references                       │
│ 3D → int[][][]                                     │
│ Access element → O(1)                             │
│ Traverse R × C → O(R × C)                         │
├────────────────────────────────────────────────────┤
│ DSA PATTERNS                                       │
├────────────────────────────────────────────────────┤
│ Every cell → Nested traversal                      │
│ Neighbors → Directions + BFS/DFS                   │
│ Main diagonal → [i][i]                             │
│ Secondary diagonal → [i][n-1-i]                    │
│ Rotate → Transpose + Reverse                       │
│ Spiral → Boundary / Layer traversal                │
│ Rectangle queries → 2D Prefix Sum                  │
│ Grid paths → 2D DP                                 │
└────────────────────────────────────────────────────┘
```

---

# 49. ⭐ One-Line Interview Definition

> **A multidimensional array in Java is an array of arrays, commonly used to represent multidimensional data such as matrices and grids, with each element accessed using multiple indexes.**

---

# 🔗 ARRAY FOLDER PROGRESS

```text
05-Arrays/

│
├── 01-Array-Introduction.md
├── 02-One-Dimensional-Array.md
├── 03-Multidimensional-Array.md     ← YOU ARE HERE
├── 04-Array-Memory.md
├── 05-Arrays-Class.md
└── 06-Array-Interview-Questions.md
```

### Next:

> **04 — Array Memory**

This will cover:

- Array objects
- Array references
- Heap allocation
- `length`
- Primitive arrays
- Reference arrays
- 1D array memory structure
- 2D array memory structure
- Array of arrays
- Jagged array memory
- References between outer and inner arrays
- JVM-level interview concepts
- Common memory traps
- DSA relevance of array memory