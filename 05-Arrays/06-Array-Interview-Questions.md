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

An **array** is an object that stores a fixed number of elements of the same type in indexed form.

Example:

    int[] nums = {10, 20, 30, 40};

Here:

    nums
       ↓
    [10, 20, 30, 40]

Each element has an index:

    Index:  0   1   2   3
    Value: 10  20  30  40

The first element is at index `0`.

---

# 2. 🤔 Why Are Arrays Used?

Suppose we need to store marks of 5 students.

Without an array:

    int marks1 = 80;
    int marks2 = 75;
    int marks3 = 90;
    int marks4 = 85;
    int marks5 = 70;

With an array:

    int[] marks = {80, 75, 90, 85, 70};

Arrays allow us to:

- Store multiple values under one variable
- Access elements using indexes
- Iterate efficiently
- Perform DSA operations
- Represent tables and matrices
- Store collections of primitive values efficiently

---

# 3. ⭐ Array Characteristics

Important characteristics:

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

    int[] arr;

## Style 2

    int arr[];

Both are valid.

Recommended style:

    int[] arr;

Why?

Because the type clearly appears as:

    int[]

---

# 5. 🏗️ Array Creation

Declaration does not create the array object.

Example:

    int[] arr;

At this point:

    arr

is only a reference variable.

We create the array using:

    arr = new int[5];

Now an array object containing 5 `int` elements is created.

Combined:

    int[] arr = new int[5];

---

# 6. ✍️ Array Initialization

We can initialize an array directly.

Example:

    int[] arr = {10, 20, 30, 40};

This creates an array containing four elements.

Another form:

    int[] arr = new int[]{10, 20, 30, 40};

Both represent an array containing:

    [10, 20, 30, 40]

---

# 7. 🟢 Default Values

When an array is created using `new`, its elements receive default values.

Example:

    int[] arr = new int[5];

Initially:

    [0, 0, 0, 0, 0]

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

    String[] names = new String[3];

Result conceptually:

    [null, null, null]

---

# 8. 🔢 Array Indexing

Java arrays use zero-based indexing.

Example:

    int[] arr = {10, 20, 30, 40};

Indexes:

    Index:  0   1   2   3
    Value: 10  20  30  40

Access:

    arr[0] → 10
    arr[1] → 20
    arr[2] → 30
    arr[3] → 40

---

## Why Does Index Start at 0?

The index represents an offset from the beginning of the array.

The first element has offset:

    0

The second:

    1

The third:

    2

This is a fundamental convention used throughout Java and many other languages.

---

# 9. 📏 Array Length

Arrays have a field called:

    length

Example:

    int[] arr = {10, 20, 30};

    System.out.println(arr.length);

Output:

    3

Important:

    array.length

is a field.

It is NOT:

    array.length()

---

## Array vs String

Array:

    arr.length

String:

    str.length()

This is a common interview trap.

---

# 10. 🧠 Array Memory

When we write:

    int[] arr = new int[5];

Conceptually:

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

The variable `arr` stores a reference to the array object.

The array object itself is created on the heap.

For interview purposes:

    Reference variable
        ↓
    points to
        ↓
    Array object on heap

---

# 11. 🧩 Are Arrays Objects in Java?

Yes.

Arrays are objects in Java.

Example:

    int[] arr = new int[5];

The array itself is an object.

This is why:

    arr.length

is available.

Also:

    arr instanceof Object

is valid.

Example:

    int[] arr = new int[5];

    System.out.println(arr instanceof Object);

Output:

    true

---

# 12. ⚖️ Primitive vs Reference Arrays

## Primitive Array

    int[] nums = {10, 20, 30};

The array contains primitive `int` values.

---

## Reference Array

    String[] names = {"A", "B", "C"};

The array contains references to `String` objects.

Conceptually:

    names
      │
      ▼
    [ref][ref][ref]
      │    │    │
      ▼    ▼    ▼
     "A"  "B"  "C"

---

# 13. 🔗 Array Assignment

Consider:

    int[] a = {10, 20, 30};

    int[] b = a;

This does NOT create another array.

Both variables point to the same array.

Conceptually:

    a ─────┐
           │
           ▼
        [10,20,30]
           ▲
           │
    b ─────┘

Therefore:

    a == b

returns:

    true

---

## Modifying Through b

    b[0] = 100;

Now:

    a

also becomes:

    [100, 20, 30]

because both references point to the same array.

---

# 14. 📋 Copying Arrays

If we want a separate array, we need to copy the elements.

One option:

    int[] a = {10, 20, 30};

    int[] b = Arrays.copyOf(a, a.length);

Now:

    a != b

but:

    Arrays.equals(a, b)

is:

    true

---

## Other Ways to Copy

### `Arrays.copyOf()`

    int[] b = Arrays.copyOf(a, a.length);

### `System.arraycopy()`

    System.arraycopy(a, 0, b, 0, a.length);

### `clone()`

    int[] b = a.clone();

### Manual Copy

    int[] b = new int[a.length];

    for(int i = 0; i < a.length; i++) {
        b[i] = a[i];
    }

---

# 15. 🧠 Shallow Copy

Shallow copying becomes important with arrays of objects.

Example:

    Student[] a = {student1, student2};

    Student[] b = a.clone();

A new array object is created.

But the references to the students are copied.

Conceptually:

    a ─────► [ref1][ref2]
               │     │
               ▼     ▼
            Student Student

    b ─────► [ref1][ref2]
               │     │
               ▼     ▼
            Student Student

Therefore:

    a != b

but:

    a[0] == b[0]

can be true.

The array is new, but the referenced objects are shared.

---

# 16. 🌳 Multidimensional Arrays

Java supports arrays of arrays.

Example:

    int[][] matrix = {
        {1, 2, 3},
        {4, 5, 6}
    };

Conceptually:

    matrix
       │
       ▼
    [ ref ][ ref ]
       │      │
       ▼      ▼
    [1,2,3] [4,5,6]

Access:

    matrix[0][0] → 1
    matrix[0][1] → 2
    matrix[1][2] → 6

---

# 17. 🪜 Jagged Arrays

Java does not require all rows of a multidimensional array to have the same length.

Example:

    int[][] arr = new int[3][];

    arr[0] = new int[2];
    arr[1] = new int[4];
    arr[2] = new int[1];

Conceptually:

    row 0 → [0, 0]
    row 1 → [0, 0, 0, 0]
    row 2 → [0]

This is called a:

    Jagged Array

or:

    Ragged Array

---

# 18. 🧰 Arrays Class

Java provides:

    java.util.Arrays

It is a utility class containing many static methods for working with arrays.

Important methods:

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

---

# 19. 🖨️ Arrays.toString()

Used to get a readable representation of a one-dimensional array.

Example:

    int[] arr = {10, 20, 30};

    System.out.println(Arrays.toString(arr));

Output:

    [10, 20, 30]

---

# 20. 🌳 Arrays.deepToString()

Used for nested arrays.

Example:

    int[][] arr = {
        {1, 2},
        {3, 4}
    };

    System.out.println(Arrays.deepToString(arr));

Output:

    [[1, 2], [3, 4]]

Memory:

    1D      → toString()
    Nested  → deepToString()

---

# 21. 🔃 Arrays.sort()

Sorts an array.

Example:

    int[] arr = {5, 2, 8, 1};

    Arrays.sort(arr);

Result:

    [1, 2, 5, 8]

It modifies the original array.

---

# 22. 🔎 Arrays.binarySearch()

Searches a sorted array using binary search.

Example:

    int[] arr = {10, 20, 30, 40, 50};

    int index = Arrays.binarySearch(arr, 30);

Result:

    2

Typical complexity:

    O(log n)

Important:

> The array should be sorted according to the required ordering before using binary search.

---

# 23. 📋 Arrays.copyOf()

Creates a new array with a specified length.

Example:

    int[] arr = {10, 20, 30};

    int[] copy = Arrays.copyOf(arr, 5);

Result:

    [10, 20, 30, 0, 0]

If the requested size is smaller:

    int[] copy = Arrays.copyOf(arr, 2);

Result:

    [10, 20]

---

# 24. ✂️ Arrays.copyOfRange()

Copies a specific range.

Example:

    int[] arr = {10, 20, 30, 40, 50};

    int[] copy = Arrays.copyOfRange(arr, 1, 4);

Result:

    [20, 30, 40]

Remember:

    from → inclusive
    to   → exclusive

---

# 25. 🪣 Arrays.fill()

Fills all or part of an array.

Example:

    int[] arr = new int[5];

    Arrays.fill(arr, 10);

Result:

    [10, 10, 10, 10, 10]

Range:

    int[] arr = {1, 2, 3, 4, 5};

    Arrays.fill(arr, 1, 4, 100);

Result:

    [1, 100, 100, 100, 5]

---

# 26. ⚖️ Arrays.equals()

Compares two one-dimensional arrays element by element.

Example:

    int[] a = {1, 2, 3};
    int[] b = {1, 2, 3};

    Arrays.equals(a, b);

Result:

    true

---

# 27. 🌳 Arrays.deepEquals()

Used for nested arrays.

Example:

    int[][] a = {
        {1, 2},
        {3, 4}
    };

    int[][] b = {
        {1, 2},
        {3, 4}
    };

    Arrays.deepEquals(a, b);

Result:

    true

---

# 28. 📦 Arrays.asList()

Converts an array of reference types into a fixed-size List view backed by the array.

Example:

    String[] arr = {"A", "B", "C"};

    List<String> list = Arrays.asList(arr);

Result conceptually:

    [A, B, C]

Important:

    Arrays.asList()

does NOT create a normal resizable `ArrayList`.

This works:

    list.set(0, "X");

But this does not:

    list.add("D");

It throws:

    UnsupportedOperationException

---

## ⚠️ Primitive Array Trap

This:

    int[] nums = {1, 2, 3};

    Arrays.asList(nums);

does not produce:

    List<Integer>

Instead, the primitive array is treated as a single argument.

For:

    Integer[] nums = {1, 2, 3};

this works as expected:

    List<Integer> list = Arrays.asList(nums);

---

# 29. 🌊 Arrays.stream()

Creates a Stream from an array.

Example:

    int[] nums = {10, 20, 30};

    int sum = Arrays.stream(nums).sum();

Result:

    60

For:

    int[]

the result is an:

    IntStream

For:

    long[]

the result is:

    LongStream

For:

    double[]

the result is:

    DoubleStream

---

# 30. 🆚 `==` vs `Arrays.equals()`

This is one of the most important array interview questions.

Example:

    int[] a = {1, 2, 3};
    int[] b = {1, 2, 3};

    System.out.println(a == b);

Output:

    false

Why?

Because they are different array objects.

But:

    System.out.println(Arrays.equals(a, b));

Output:

    true

Because their contents are equal.

### Remember

    ==

checks:

    Same object/reference?

While:

    Arrays.equals()

checks:

    Same elements?

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

    int[] arr = new int[5];

ArrayList:

    ArrayList<Integer> list = new ArrayList<>();

    list.add(10);
    list.add(20);

Array:

    arr.length

ArrayList:

    list.size()

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

---

# 33. 🚨 Common Interview Traps

## Trap 1 — `length` vs `length()`

Array:

    arr.length

String:

    str.length()

ArrayList:

    list.size()

---

## Trap 2 — Array indexing

First element:

    arr[0]

Last element:

    arr[arr.length - 1]

---

## Trap 3 — Out of Bounds

If:

    int[] arr = new int[5];

valid indexes are:

    0, 1, 2, 3, 4

This is invalid:

    arr[5]

It throws:

    ArrayIndexOutOfBoundsException

---

## Trap 4 — Null Array

Example:

    int[] arr = null;

Trying:

    arr.length

causes:

    NullPointerException

---

## Trap 5 — Assignment Is Not Copy

    int[] b = a;

does not create a new array.

Both point to the same object.

---

## Trap 6 — `Arrays.equals()` vs `==`

    ==

checks reference identity.

    Arrays.equals()

checks contents.

---

## Trap 7 — `toString()` vs `deepToString()`

    toString()
        → 1D

    deepToString()
        → nested

---

## Trap 8 — `Arrays.sort()` changes the array

It sorts the original array.

---

## Trap 9 — Binary search requires ordering

Do not blindly binary-search an unsorted array.

---

## Trap 10 — `Arrays.asList()` and primitive arrays

    int[]

does not become:

    List<Integer>

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

    int[]

stores `int` values.

But an array whose component type is a common superclass/interface can hold compatible objects.

Example:

    Object[] arr = {
        10,
        "Java",
        3.14
    };

---

## Q3. Is array size fixed?

Yes.

Once an array is created, its length cannot be changed.

If we need a different size, we create another array.

---

## Q4. Can we change an array's length?

No.

This is impossible:

    arr.length = 10;

`length` is not assignable.

---

## Q5. Can arrays contain objects?

Yes.

Example:

    String[] names = new String[5];

---

## Q6. Can arrays contain primitives?

Yes.

Example:

    int[] nums = new int[5];

---

## Q7. Why is array access O(1)?

Because the runtime can directly calculate the location of an element from the array's starting location and index.

Conceptually:

    address = base + index × element-size

The actual JVM representation is more complex, but the important DSA property is constant-time indexed access.

---

## Q8. What happens when an invalid index is accessed?

Java throws:

    ArrayIndexOutOfBoundsException

Example:

    int[] arr = new int[3];

    arr[3];

Valid indexes:

    0, 1, 2

---

## Q9. What happens if the array reference is null?

Example:

    int[] arr = null;

    System.out.println(arr.length);

Result:

    NullPointerException

---

## Q10. Can an array be empty?

Yes.

Example:

    int[] arr = new int[0];

Its length is:

    0

But the reference itself is not null.

---

# 35. 💻 Top Coding Questions

## Q1. Find the Maximum Element

Approach:

    Keep a variable max.

    Traverse the array.

    If current element > max,
    update max.

Example:

    int[] arr = {10, 5, 30, 20};

    int max = arr[0];

    for(int i = 1; i < arr.length; i++) {

        if(arr[i] > max) {
            max = arr[i];
        }
    }

Result:

    30

Complexity:

    Time  → O(n)
    Space → O(1)

---

# Q2. Find the Minimum Element

Approach:

    Start with first element.

    Compare every other element.

Example:

    int[] arr = {10, 5, 30, 20};

    int min = arr[0];

    for(int i = 1; i < arr.length; i++) {

        if(arr[i] < min) {
            min = arr[i];
        }
    }

Result:

    5

---

# Q3. Find Sum of Array

Example:

    int[] arr = {10, 20, 30};

    int sum = 0;

    for(int i = 0; i < arr.length; i++) {
        sum += arr[i];
    }

Result:

    60

Complexity:

    O(n)

---

# Q4. Reverse an Array

Example:

    int[] arr = {1, 2, 3, 4, 5};

Use two pointers:

    left = 0
    right = arr.length - 1

Swap:

    arr[left]
    arr[right]

Then move:

    left++
    right--

Result:

    [5, 4, 3, 2, 1]

Complexity:

    Time  → O(n)
    Space → O(1)

---

# Q5. Linear Search

Example:

    int[] arr = {10, 20, 30, 40};

    int target = 30;

    for(int i = 0; i < arr.length; i++) {

        if(arr[i] == target) {
            System.out.println(i);
            break;
        }
    }

Result:

    2

Complexity:

    O(n)

---

# Q6. Count Occurrences

Example:

    int[] arr = {1, 2, 2, 3, 2};

    int target = 2;
    int count = 0;

    for(int i = 0; i < arr.length; i++) {

        if(arr[i] == target) {
            count++;
        }
    }

Result:

    3

---

# Q7. Check if Array Is Sorted

Example:

    int[] arr = {1, 2, 3, 4, 5};

Compare:

    arr[i] > arr[i + 1]

If this happens for ascending order, the array is not sorted.

Concept:

    for(int i = 0; i < arr.length - 1; i++) {

        if(arr[i] > arr[i + 1]) {
            return false;
        }
    }

    return true;

Complexity:

    O(n)

---

# Q8. Find Second Largest Element

A common approach is to maintain:

    largest
    secondLargest

Example:

    int[] arr = {10, 30, 20, 40};

Concept:

    largest = Integer.MIN_VALUE;
    secondLargest = Integer.MIN_VALUE;

Traverse the array and update both appropriately.

Expected result:

    30

Be careful with duplicate values and with the exact definition of "second largest" in the problem.

---

# Q9. Remove Duplicates From Sorted Array

This is a classic two-pointer problem.

Example:

    [1, 1, 2, 2, 3]

Goal:

    [1, 2, 3]

Use a write pointer.

General idea:

    read pointer
        ↓
    scans array

    write pointer
        ↓
    places unique values

This pattern appears frequently in array problems.

---

# Q10. Move Zeroes to End

Example:

    [0, 1, 0, 3, 12]

Goal:

    [1, 3, 12, 0, 0]

Common approach:

    Two pointers

Keep track of where the next non-zero value should be placed.

---

# Q11. Find Missing Number

Given:

    [0, 1, 3]

Numbers should be:

    0, 1, 2, 3

Missing:

    2

Possible approaches:

    Sum formula
    XOR

XOR is especially useful because:

    x ^ x = 0

and:

    x ^ 0 = x

---

# Q12. Find Single Number

Classic problem:

    [4, 1, 2, 1, 2]

Every number appears twice except one.

Use XOR:

    result = 0;

    for(int num : nums) {
        result = result ^ num;
    }

Why?

Because:

    x ^ x = 0

Therefore pairs cancel.

Remaining value:

    4

---

# Q13. Majority Element

A majority element appears more than:

    n / 2

times.

A famous O(n) time and O(1) space solution is:

    Boyer-Moore Voting Algorithm

Core idea:

    candidate
    count

If count becomes zero:

    candidate = current element

If current equals candidate:

    count++

Otherwise:

    count--

---

# Q14. Best Time to Buy and Sell Stock

Given prices:

    [7, 1, 5, 3, 6, 4]

Maintain:

    minimum price seen so far

and:

    maximum profit

At every element:

    profit = currentPrice - minimumPrice

Then update maximum profit.

Complexity:

    Time  → O(n)
    Space → O(1)

---

# Q15. Rotate Array

Example:

    [1, 2, 3, 4, 5]

Rotate right by 2:

    [4, 5, 1, 2, 3]

Common optimal approach:

    Reverse entire array
    Reverse first k elements
    Reverse remaining elements

This is a classic array manipulation pattern.

---

# 36. 🧩 Important DSA Patterns

Learning arrays is not just about syntax.

Arrays are the foundation for many DSA patterns.

---

## Pattern 1 — Traversal

Basic:

    for(int i = 0; i < arr.length; i++)

Use when:

- Visiting every element
- Calculating sum
- Finding min/max
- Counting values

---

## Pattern 2 — Two Pointers

Example:

    left = 0
    right = arr.length - 1

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

    arr = [2, 4, 3]

Prefix:

    [2, 6, 9]

Useful for:

- Range sum
- Subarray calculations
- Query problems

---

## Pattern 5 — Hashing

Use:

    HashMap
    HashSet

Useful for:

- Frequency
- Duplicates
- Two Sum
- Fast lookup

---

## Pattern 6 — Sorting

Sorting can simplify many problems.

Example:

    Arrays.sort(arr);

After sorting, problems involving:

- Duplicates
- Pairs
- Intervals
- Binary search

may become easier.

---

## Pattern 7 — Binary Search

Used when the search space has an appropriate ordering/monotonic property.

Basic array version:

    Arrays.binarySearch()

But in DSA, you should also learn to implement binary search manually.

---

## Pattern 8 — Kadane's Algorithm

Used to find maximum subarray sum.

Example:

    [-2,1,-3,4,-1,2,1,-5,4]

Maximum subarray:

    [4,-1,2,1]

Sum:

    6

Typical complexity:

    O(n)

---

# 37. 🎤 30-Second Interview Answer

> **An array in Java is an object that stores a fixed number of elements of the same component type and provides zero-based indexed access. Arrays provide O(1) random access, but their size cannot be changed after creation. Java also provides the `java.util.Arrays` utility class for operations such as sorting, searching, copying, comparing, filling, and converting arrays to readable strings. Arrays are fundamental in DSA because they form the basis for patterns such as two pointers, sliding window, prefix sum, binary search, and many hashing problems.**

---

# 38. 🧾 Cheat Sheet

## Declaration

    int[] arr;

## Creation

    int[] arr = new int[5];

## Initialization

    int[] arr = {1, 2, 3};

## Access

    arr[0]

## Update

    arr[0] = 100;

## Length

    arr.length

## Last Element

    arr[arr.length - 1]

## Loop

    for(int i = 0; i < arr.length; i++) {
        System.out.println(arr[i]);
    }

## Enhanced For Loop

    for(int num : arr) {
        System.out.println(num);
    }

---

## Arrays Utility

    Arrays.toString(arr)

    Arrays.deepToString(arr)

    Arrays.sort(arr)

    Arrays.binarySearch(arr, target)

    Arrays.copyOf(arr, length)

    Arrays.copyOfRange(arr, from, to)

    Arrays.fill(arr, value)

    Arrays.equals(a, b)

    Arrays.deepEquals(a, b)

    Arrays.asList(objectArray)

    Arrays.stream(arr)

---

# 39. ✅ Final Revision Checklist

Before considering the Arrays chapter complete, make sure you can answer all of these without looking at your notes:

## Fundamentals

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

## Memory

    [ ] Where is the array object created?
    [ ] What does the array variable store?
    [ ] What happens when two references point to one array?
    [ ] What is shallow copying?
    [ ] How are object arrays different from primitive arrays?

## Multidimensional

    [ ] What is a 2D array?
    [ ] What is an array of arrays?
    [ ] What is a jagged array?
    [ ] Can rows have different lengths?

## Arrays Class

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

## Interview Traps

    [ ] arr.length vs str.length()
    [ ] arr == other vs Arrays.equals()
    [ ] toString vs deepToString
    [ ] equals vs deepEquals
    [ ] assignment vs copying
    [ ] primitive array + Arrays.asList()
    [ ] binarySearch on unsorted data
    [ ] array index out of bounds
    [ ] null array vs empty array

## DSA

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

---

# 🏆 FINAL ARRAY MINDSET

When you see an array problem, don't immediately start coding.

First ask:

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

---

# 🧠 ARRAY INTERVIEW FORMULA

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

---

# 🚀 ARRAY FOLDER COMPLETE

    05-Arrays/
    │
    ├── 01-Array-Introduction.md
    ├── 02-One-Dimensional-Array.md
    ├── 03-Multidimensional-Array.md
    ├── 04-Array-Memory.md
    ├── 05-Arrays-Class.md
    └── 06-Array-Interview-Questions.md  ← YOU ARE HERE

---

# ⭐ ONE-LINE REVISION

> **Array = fixed-size, same-type, zero-indexed collection with O(1) indexed access; master traversal, two pointers, sliding window, hashing, prefix sums, sorting, and binary search to solve array problems efficiently.**