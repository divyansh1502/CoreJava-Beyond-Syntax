# 🔢 Multidimensional Array in Java

> **A multidimensional array in Java is an array whose elements are themselves arrays. The most common multidimensional array is a 2D array, which is commonly represented as rows and columns.**

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
37. [Common Mistakes](#37--common-mistakes)
38. [Interview Traps](#38--interview-traps)
39. [Top 20 Interview Questions](#39--top-20-interview-questions)
40. [Coding Problems](#40--coding-problems)
41. [30-Second Interview Answer](#41--30-second-interview-answer)
42. [Cheat Sheet](#42--cheat-sheet)
43. [Memory Tricks](#43--memory-tricks)
44. [Final Revision Checklist](#44--final-revision-checklist)

---

# 1. 🔹 What is a Multidimensional Array?

A multidimensional array is an array containing other arrays.

The most commonly used multidimensional array is a **two-dimensional array**.

Example:

    int[][] matrix = {
        {1, 2, 3},
        {4, 5, 6},
        {7, 8, 9}
    };

Visual representation:

    Column
       0   1   2
    ┌───┬───┬───┐
 0  │ 1 │ 2 │ 3 │
    ├───┼───┼───┤
 1  │ 4 │ 5 │ 6 │
    ├───┼───┼───┤
 2  │ 7 │ 8 │ 9 │
    └───┴───┴───┘

To access `5`:

    matrix[1][1]

Here:

    1 → row
    1 → column

Therefore:

    matrix[row][column]

---

# 2. 🎯 Why Do We Need Multidimensional Arrays?

A one-dimensional array is useful for linear data:

    10 20 30 40 50

But some data naturally has multiple dimensions.

Examples:

### 🏫 Student Marks

    Student      Maths    Java    DBMS
       0           80      90      75
       1           70      85      80
       2           95      88      90

This can be represented as:

    int[][] marks;

### 🧮 Matrix

    1 2 3
    4 5 6
    7 8 9

### 🎮 Game Board

    X . X
    . X .
    X . .

### 🖼️ Image Data

An image can be represented using rows and columns of pixels.

---

# 3. 🧱 2D Array

A two-dimensional array contains multiple rows.

Example:

    int[][] arr = new int[3][4];

This represents:

    3 rows
    4 columns

Visual:

             Columns
          0   1   2   3
        ┌───┬───┬───┬───┐
    0   │ 0 │ 0 │ 0 │ 0 │
        ├───┼───┼───┼───┤
    1   │ 0 │ 0 │ 0 │ 0 │
        ├───┼───┼───┼───┤
    2   │ 0 │ 0 │ 0 │ 0 │
        └───┴───┴───┴───┘

Total elements:

    3 × 4 = 12

Because this is an `int` array, every element initially contains:

    0

---

# 4. 📝 Declaration

Recommended syntax:

    int[][] arr;

Other valid syntax:

    int arr[][];

    int[] arr[];

All three represent a two-dimensional array reference.

Recommended:

    int[][] arr;

because it clearly communicates that the array has two dimensions.

---

# 5. 🏗️ Creation

Use the `new` keyword:

    int[][] arr = new int[3][4];

This creates:

    3 row arrays
    4 elements in each row

Conceptually:

    arr
     ↓
    Row 0 → [0, 0, 0, 0]
    Row 1 → [0, 0, 0, 0]
    Row 2 → [0, 0, 0, 0]

---

# 6. 🧩 Declaration + Creation

These can be written separately.

### Declaration

    int[][] arr;

### Creation

    arr = new int[3][4];

### Combined

    int[][] arr = new int[3][4];

---

# 7. 🎯 Initialization

Elements can be initialized individually.

    int[][] arr = new int[2][3];

    arr[0][0] = 10;
    arr[0][1] = 20;
    arr[0][2] = 30;

    arr[1][0] = 40;
    arr[1][1] = 50;
    arr[1][2] = 60;

Result:

    10 20 30
    40 50 60

---

# 8. ⚡ Direct Initialization

We can initialize the complete array directly.

    int[][] arr = {
        {10, 20, 30},
        {40, 50, 60}
    };

Visual:

    10 20 30
    40 50 60

Another example:

    int[][] matrix = {
        {1, 2},
        {3, 4},
        {5, 6}
    };

This contains:

    3 rows
    2 columns

---

# 9. 📐 Rows and Columns

Suppose:

    int[][] arr = new int[3][4];

Then:

    Number of rows = 3
    Number of columns = 4

To get the number of rows:

    arr.length

Result:

    3

To get the number of columns in a particular row:

    arr[0].length

Result:

    4

Important:

    arr.length
        ↓
    number of rows

    arr[i].length
        ↓
    number of elements in row i

---

# 10. 👀 Accessing Elements

Syntax:

    arr[row][column]

Example:

    int[][] arr = {
        {10, 20, 30},
        {40, 50, 60},
        {70, 80, 90}
    };

Access:

    arr[0][0] → 10
    arr[0][1] → 20
    arr[1][0] → 40
    arr[1][2] → 60
    arr[2][1] → 80
    arr[2][2] → 90

Example:

    System.out.println(arr[2][2]);

Output:

    90

---

# 11. ✏️ Updating Elements

A 2D array is mutable.

Example:

    int[][] arr = {
        {10, 20},
        {30, 40}
    };

    arr[1][0] = 99;

Before:

    10 20
    30 40

After:

    10 20
    99 40

---

# 12. 🔄 Traversing a 2D Array

Since we have two indexes, we generally use nested loops.

Example:

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

Output:

    10 20 30
    40 50 60

---

# 13. 🔁 Nested `for` Loop

This is one of the most important patterns for 2D arrays.

    for (int i = 0; i < arr.length; i++) {

        for (int j = 0; j < arr[i].length; j++) {

            System.out.println(arr[i][j]);
        }
    }

Think of it like this:

    i → row
    j → column

Therefore:

    arr[i][j]

means:

> Element at row `i` and column `j`.

---

## 🧠 Dry Run

Suppose:

    int[][] arr = {
        {10, 20, 30},
        {40, 50, 60}
    };

### First iteration

    i = 0

Inner loop:

    j = 0 → arr[0][0] → 10
    j = 1 → arr[0][1] → 20
    j = 2 → arr[0][2] → 30

### Second iteration

    i = 1

Inner loop:

    j = 0 → arr[1][0] → 40
    j = 1 → arr[1][1] → 50
    j = 2 → arr[1][2] → 60

Traversal:

    10 → 20 → 30 → 40 → 50 → 60

---

# 14. 🚀 Enhanced `for` Loop

A 2D array can also be traversed using nested enhanced `for` loops.

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

Output:

    10 20 30
    40 50 60

---

## How Does This Work?

Outer loop:

    for (int[] row : arr)

means:

> Take each row array from `arr`.

Inner loop:

    for (int value : row)

means:

> Take each value from the current row.

So:

    arr
     ↓
    row
     ↓
    value

---

# 15. ⌨️ Taking 2D Array Input

Example:

    Scanner sc = new Scanner(System.in);

    int rows = sc.nextInt();
    int columns = sc.nextInt();

    int[][] arr = new int[rows][columns];

    for (int i = 0; i < rows; i++) {

        for (int j = 0; j < columns; j++) {

            arr[i][j] = sc.nextInt();
        }
    }

Input:

    3 4

    1 2 3 4
    5 6 7 8
    9 10 11 12

Array:

    1  2  3  4
    5  6  7  8
    9 10 11 12

---

# 16. 🖨️ Printing a 2D Array

Using nested loops:

    for (int i = 0; i < arr.length; i++) {

        for (int j = 0; j < arr[i].length; j++) {

            System.out.print(arr[i][j] + " ");
        }

        System.out.println();
    }

Output:

    10 20 30
    40 50 60

---

## Using `Arrays.deepToString()`

For displaying a multidimensional array:

    System.out.println(Arrays.deepToString(arr));

Output:

    [[10, 20, 30], [40, 50, 60]]

`deepToString()` is useful because a multidimensional array contains nested arrays.

The `Arrays` class is covered in:

    05-Arrays-Class.md

---

# 17. 📏 Understanding `arr.length`

Consider:

    int[][] arr = {
        {1, 2, 3},
        {4, 5, 6}
    };

Then:

    arr.length

is:

    2

Why?

Because the outer array contains two row arrays.

Conceptually:

    arr
     ↓
    [ row0, row1 ]

Therefore:

    arr.length = 2

---

# 18. 📏 Understanding `arr[i].length`

For:

    int[][] arr = {
        {1, 2, 3},
        {4, 5, 6}
    };

We have:

    arr[0].length → 3
    arr[1].length → 3

Therefore:

    arr.length

means:

> Number of rows.

And:

    arr[i].length

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

    int[][] arr = {
        {10, 20},
        {30, 40},
        {50, 60}
    };

Conceptually:

    arr
     ↓
    ┌────────┬────────┬────────┐
    │ row 0  │ row 1  │ row 2  │
    │   ↓    │   ↓    │   ↓    │
    └───┼────┴───┼────┴───┼────┘
        ↓        ↓        ↓
      [10,20]  [30,40]  [50,60]

The outer array contains references to the row arrays.

So:

    int[][]

is essentially:

    array of int[]

---

# 20. 🧩 Array of Arrays

Think of:

    int[][] arr;

as:

    Array
      |
      ├── int[]
      ├── int[]
      └── int[]

Each element of the outer array is itself an integer array.

This is why Java can support rows with different lengths.

---

# 21. 🪚 Jagged Arrays

A jagged array is a multidimensional array where different rows can have different lengths.

Example:

    int[][] arr = {
        {1, 2},
        {3, 4, 5},
        {6},
        {7, 8, 9, 10}
    };

Visual:

    Row 0 → 1 2
    Row 1 → 3 4 5
    Row 2 → 6
    Row 3 → 7 8 9 10

This is completely valid Java.

---

# 22. 🏗️ Creating a Jagged Array

First create the outer array:

    int[][] arr = new int[3][];

Notice:

    new int[3][]

The number of rows is specified:

    3

But the length of each row is not specified yet.

Then create rows individually:

    arr[0] = new int[2];
    arr[1] = new int[4];
    arr[2] = new int[3];

Now:

    Row 0 → 2 elements
    Row 1 → 4 elements
    Row 2 → 3 elements

---

# 23. 🎯 Initializing a Jagged Array

Example:

    int[][] arr = new int[3][];

    arr[0] = new int[]{10, 20};

    arr[1] = new int[]{30, 40, 50};

    arr[2] = new int[]{60, 70, 80, 90};

Result:

    10 20
    30 40 50
    60 70 80 90

Traversal must use:

    for (int i = 0; i < arr.length; i++) {

        for (int j = 0; j < arr[i].length; j++) {

            System.out.print(arr[i][j] + " ");
        }

        System.out.println();
    }

Notice:

    arr[i].length

instead of:

    arr[0].length

This is important because each row can have a different size.

---

# 24. 🧊 3D Arrays

Java supports more than two dimensions.

Example:

    int[][][] arr = new int[2][3][4];

This can be visualized as:

    2 blocks
    3 rows per block
    4 columns per row

Conceptually:

    Block 0
        Row 0 → 4 elements
        Row 1 → 4 elements
        Row 2 → 4 elements

    Block 1
        Row 0 → 4 elements
        Row 1 → 4 elements
        Row 2 → 4 elements

Access:

    arr[block][row][column]

Example:

    arr[1][2][3]

---

# 25. 🧱 General Multidimensional Syntax

### 1D Array

    int[] arr;

### 2D Array

    int[][] arr;

### 3D Array

    int[][][] arr;

### 4D Array

    int[][][][] arr;

The pattern continues.

However, in practical Java development and DSA, 1D and 2D arrays are much more commonly used.

---

# 26. ➕ Finding Sum

Example:

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

Output:

    210

Calculation:

    10 + 20 + 30 + 40 + 50 + 60 = 210

---

# 27. 📈 Finding Maximum and Minimum

## Maximum

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

Output:

    80

---

## Minimum

    int min = arr[0][0];

    for (int i = 0; i < arr.length; i++) {

        for (int j = 0; j < arr[i].length; j++) {

            if (arr[i][j] < min) {
                min = arr[i][j];
            }
        }
    }

    System.out.println(min);

Output:

    10

---

# 28. 🔍 Searching in a 2D Array

Suppose:

    int[][] arr = {
        {10, 20, 30},
        {40, 50, 60},
        {70, 80, 90}
    };

Target:

    50

Linear search through the 2D array:

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

Output:

    Found at row 1, column 1

---

# 29. ➕ Matrix Addition

Two matrices can be added when they have the same dimensions.

Example:

    Matrix A:

    1 2
    3 4

    Matrix B:

    5 6
    7 8

Result:

    6  8
    10 12

Code:

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

---

# 30. 🔄 Transpose of a Matrix

Transpose means:

> Convert rows into columns and columns into rows.

Original:

    1 2 3
    4 5 6

Transpose:

    1 4
    2 5
    3 6

For an `m × n` matrix, transpose becomes:

    n × m

Code:

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

Result:

    1 4
    2 5
    3 6

---

# 31. 🔲 Diagonal Elements

For a square matrix:

    1 2 3
    4 5 6
    7 8 9

Main diagonal:

    1
       5
          9

Indexes:

    [0][0]
    [1][1]
    [2][2]

The pattern is:

    arr[i][i]

Example:

    for (int i = 0; i < arr.length; i++) {
        System.out.println(arr[i][i]);
    }

Output:

    1
    5
    9

---

## Secondary Diagonal

For:

    1 2 3
    4 5 6
    7 8 9

Secondary diagonal:

    3
       5
          7

Indexes:

    [0][2]
    [1][1]
    [2][0]

General pattern:

    arr[i][n - 1 - i]

---

# 32. 📤 Passing 2D Array to a Method

A 2D array can be passed to a method.

Example:

    static void printMatrix(int[][] arr) {

        for (int i = 0; i < arr.length; i++) {

            for (int j = 0; j < arr[i].length; j++) {

                System.out.print(arr[i][j] + " ");
            }

            System.out.println();
        }
    }

Call:

    int[][] matrix = {
        {1, 2},
        {3, 4}
    };

    printMatrix(matrix);

---

# 33. 📥 Returning a 2D Array

A method can return a 2D array.

Example:

    static int[][] createMatrix() {

        int[][] matrix = {
            {1, 2},
            {3, 4}
        };

        return matrix;
    }

Call:

    int[][] result = createMatrix();

Now:

    result

refers to the returned 2D array.

---

# 34. 🚫 Multidimensional Array and `null`

Because a 2D array is an array of row references, individual rows can also be `null`.

Example:

    int[][] arr = new int[3][];

At this point:

    arr[0] = null
    arr[1] = null
    arr[2] = null

If we do:

    System.out.println(arr[0].length);

we get:

    NullPointerException

because `arr[0]` does not currently refer to an inner array.

We can create a row:

    arr[0] = new int[3];

Now:

    arr[0].length

is:

    3

---

# 35. 💥 Common Exceptions

## 1. ArrayIndexOutOfBoundsException

Example:

    int[][] arr = {
        {10, 20},
        {30, 40}
    };

    System.out.println(arr[2][0]);

Valid row indexes are:

    0
    1

So row `2` is invalid.

---

## 2. NullPointerException

Example:

    int[][] arr = new int[3][];

    System.out.println(arr[0].length);

`arr[0]` is `null`.

---

## 3. ArrayStoreException

This can occur when an incompatible object is stored into an array whose runtime component type does not allow it.

Example:

    Object[][] arr = new String[2][];

    arr[0] = new String[2];

    Object[] row = arr[0];

    row[0] = 100;

The runtime row is actually a `String[]`, so storing an `Integer` can cause:

    ArrayStoreException

---

# 36. ⏱️ Time Complexity

Suppose a matrix contains:

    R rows
    C columns

Then:

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

    O(R × C)

becomes:

    O(n²)

---

# 37. ⚠️ Common Mistakes

## ❌ Mistake 1 — Using `arr.length` for columns

Wrong for general 2D arrays:

    for (int j = 0; j < arr.length; j++)

Correct:

    for (int j = 0; j < arr[i].length; j++)

Why?

Because:

    arr.length
        ↓
    number of rows

while:

    arr[i].length
        ↓
    number of elements in row i

---

## ❌ Mistake 2 — Assuming every row has the same length

This is unsafe for jagged arrays.

Wrong assumption:

    arr[0].length == arr[1].length

It may be true for rectangular arrays, but not necessarily for jagged arrays.

---

## ❌ Mistake 3 — Forgetting the second index

For a 2D array:

    arr[i]

returns a row array.

To access an individual element:

    arr[i][j]

---

## ❌ Mistake 4 — Confusing row and column

Remember:

    arr[row][column]

So:

    arr[1][2]

means:

    row 1
    column 2

---

## ❌ Mistake 5 — Forgetting zero-based indexing

For:

    int[][] arr = new int[3][4];

Valid rows:

    0, 1, 2

Valid columns:

    0, 1, 2, 3

---

# 38. 🚨 Interview Traps

## Trap 1

What is:

    int[][] arr = new int[3][4];

Answer:

    3 rows
    4 elements per row

---

## Trap 2

What is:

    arr.length

Answer:

    Number of rows.

---

## Trap 3

What is:

    arr[0].length

Answer:

    Number of elements in row 0.

---

## Trap 4

Is a 2D array actually a rectangular matrix internally?

Answer:

> Not necessarily. In Java, a 2D array is an array of arrays, so rows can have different lengths.

---

## Trap 5

Is this valid?

    int[][] arr = {
        {1, 2},
        {3, 4, 5}
    };

Yes.

This is a jagged array.

---

## Trap 6

Is this valid?

    int[][] arr = new int[3][];

Yes.

The outer array has three row references, and individual rows can be created later.

---

## Trap 7

What is:

    arr[0]

for a 2D array?

Answer:

> It is a reference to the first row array.

---

## Trap 8

What is:

    arr[0][1]

Answer:

> The element at row `0`, column `1`.

---

# 39. 🔥 Top 20 Interview Questions

## Q1. What is a multidimensional array?

A multidimensional array is an array whose elements are themselves arrays.

---

## Q2. What is the most common multidimensional array?

A two-dimensional array.

---

## Q3. How do you declare a 2D array?

    int[][] arr;

---

## Q4. How do you create a 2D array?

    int[][] arr = new int[3][4];

---

## Q5. How do you access an element?

    arr[row][column]

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

    int[][] arr = new int[3][];

    arr[0] = new int[2];
    arr[1] = new int[4];
    arr[2] = new int[3];

---

## Q12. How do you traverse a 2D array?

Usually with nested loops.

---

## Q13. What does `arr[i][j]` represent?

The element at row `i` and column `j`.

---

## Q14. Can a 2D array contain `null` rows?

Yes, especially when created like:

    new int[3][]

before individual rows are initialized.

---

## Q15. Can methods accept 2D arrays?

Yes.

Example:

    static void display(int[][] arr)

---

## Q16. Can methods return 2D arrays?

Yes.

Example:

    static int[][] createMatrix()

---

## Q17. What is the complexity of traversing an `R × C` matrix?

    O(R × C)

---

## Q18. What is a 3D array?

An array with three dimensions.

Example:

    int[][][] arr;

---

## Q19. What is the difference between `arr.length` and `arr[i].length`?

`arr.length` gives the number of rows, while `arr[i].length` gives the length of a particular row.

---

## Q20. Why can Java have jagged arrays?

Because a multidimensional array is an array of arrays, and each inner array can have a different length.

---

# 40. 💻 Coding Problems

## Problem 1 — Print a Matrix

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

Output:

    1 2 3
    4 5 6
    7 8 9

---

## Problem 2 — Find Sum

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

Output:

    21

---

## Problem 3 — Find Maximum

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

Output:

    60

---

## Problem 4 — Search an Element

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

Output:

    Found at row 1, column 1

---

## Problem 5 — Print Main Diagonal

For:

    1 2 3
    4 5 6
    7 8 9

Code:

    int[][] matrix = {
        {1, 2, 3},
        {4, 5, 6},
        {7, 8, 9}
    };

    for (int i = 0; i < matrix.length; i++) {
        System.out.println(matrix[i][i]);
    }

Output:

    1
    5
    9

---

## Problem 6 — Create a Jagged Array

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

Output:

    1 2
    3 4 5
    6 7 8 9

---

# 41. 🎤 30-Second Interview Answer

> **A multidimensional array in Java is an array whose elements are themselves arrays. The most common example is a 2D array, which is generally represented as rows and columns. We access an element using two indexes such as `arr[i][j]`. An important point is that Java's 2D arrays are actually arrays of arrays, which means Java supports jagged arrays where different rows can have different lengths.**

---

# 42. 🧾 Cheat Sheet

## Declaration

    int[][] arr;

## Creation

    int[][] arr = new int[3][4];

## Direct Initialization

    int[][] arr = {
        {1, 2},
        {3, 4}
    };

## Access

    arr[i][j]

## Rows

    arr.length

## Columns of Row `i`

    arr[i].length

## Traverse

    for (int i = 0; i < arr.length; i++) {
        for (int j = 0; j < arr[i].length; j++) {
            // arr[i][j]
        }
    }

## Enhanced Traversal

    for (int[] row : arr) {
        for (int value : row) {
            // value
        }
    }

## Jagged Array

    int[][] arr = new int[3][];

## 3D Array

    int[][][] arr;

## Print Nested Array

    Arrays.deepToString(arr)

---

# 43. 🧠 Memory Tricks

## 🔥 Trick 1 — Remember the Index

For a 2D array:

    arr[row][column]

Think:

    First → Row
    Second → Column

---

## 🔥 Trick 2 — Remember the Length

    arr.length
        ↓
    Rows

    arr[i].length
        ↓
    Columns / elements in row i

---

## 🔥 Trick 3 — Nested Loops

Think:

    Outer loop  → Rows
    Inner loop  → Columns

Therefore:

    for each row
        for each column
            process element

---

## 🔥 Trick 4 — 2D Array Internals

Never think:

    2D array = one giant block

Think:

    2D array
        ↓
    array of arrays
        ↓
    row references
        ↓
    individual row arrays

---

## 🔥 Trick 5 — Jagged Array

Remember:

    Different rows
        ↓
    Different lengths
        ↓
    Jagged array

---

## 🔥 Trick 6 — Dimensions

    int[]       → 1D
    int[][]     → 2D
    int[][][]   → 3D

---

# 44. ✅ Final Revision Checklist

Before moving to the next topic, make sure you can explain:

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

---

# 🏆 MASTER MEMORY CARD

    ┌─────────────────────────────────────────────┐
    │       MULTIDIMENSIONAL ARRAY IN JAVA        │
    ├─────────────────────────────────────────────┤
    │ 2D array → array of arrays                  │
    │ Access → arr[row][column]                   │
    │ arr.length → number of rows                │
    │ arr[i].length → length of row i            │
    │ Traversal → nested loops                   │
    │ Can contain jagged rows                    │
    │ Can have null row references               │
    │ 3D → int[][][]                              │
    │ Access element → O(1)                      │
    │ Traverse R × C → O(R × C)                  │
    └─────────────────────────────────────────────┘

---

# ⭐ ONE-LINE INTERVIEW DEFINITION

> **A multidimensional array in Java is an array of arrays, commonly used to represent multidimensional data such as matrices, with each element accessed using multiple indexes.**

---

# 🔗 ARRAY FOLDER PROGRESS

    05-Arrays/
    │
    ├── 01-Array-Introduction.md
    ├── 02-One-Dimensional-Array.md
    ├── 03-Multidimensional-Array.md       ← YOU ARE HERE
    ├── 04-Array-Memory.md
    ├── 05-Arrays-Class.md
    └── 06-Array-Interview-Questions.md

### Next:

> **04 — Array Memory**

This will cover how arrays are represented in JVM memory, array objects, references, heap allocation, `length`, primitive arrays vs reference arrays, 1D/2D memory structure, and important interview traps.