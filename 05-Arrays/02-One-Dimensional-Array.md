# 🔢 One-Dimensional Array in Java

> **A one-dimensional array is a linear collection of elements of the same declared component type, accessed using a single index.**

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
13. [Difference Between for and Enhanced for](#13--difference-between-for-and-enhanced-for)
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
29. [Null and One-Dimensional Arrays](#29--null-and-one-dimensional-arrays)
30. [Common Exceptions](#30--common-exceptions)
31. [Time Complexity](#31--time-complexity)
32. [Common Mistakes](#32--common-mistakes)
33. [Interview Traps](#33--interview-traps)
34. [Top 20 Interview Questions](#34--top-20-interview-questions)
35. [Coding Problems](#35--coding-problems)
36. [30-Second Interview Answer](#36--30-second-interview-answer)
37. [Cheat Sheet](#37--cheat-sheet)
38. [Memory Tricks](#38--memory-tricks)
39. [Final Revision Checklist](#39--final-revision-checklist)

---

# 1. 🔹 What is a One-Dimensional Array?

A **one-dimensional array** stores elements in a single linear sequence.

Example:

    int[] nums = {10, 20, 30, 40, 50};

Visual representation:

    Index:     0    1    2    3    4
               ↓    ↓    ↓    ↓    ↓
    Value:    10   20   30   40   50

Only **one index** is required to access an element.

Example:

    nums[2]

Output:

    30

---

# 2. 🧱 Basic Structure

A one-dimensional array can be visualized as:

    ┌────┬────┬────┬────┬────┐
    │ 10 │ 20 │ 30 │ 40 │ 50 │
    └────┴────┴────┴────┴────┘
      0    1    2    3    4
      ↑
    index

Important:

    First index = 0
    Last index = length - 1

---

# 3. 📝 Declaration

The recommended syntax is:

    int[] arr;

This declares a reference variable that can refer to an integer array.

Other valid forms:

    int arr[];

Both are valid.

Recommended:

    int[] arr;

because it makes the array type visually clear.

---

## Different Array Types

    int[] numbers;

    double[] prices;

    char[] letters;

    boolean[] flags;

    String[] names;

---

# 4. 🏗️ Creation

Use the `new` keyword to create an array object.

Example:

    int[] arr = new int[5];

This creates an array containing five integer elements.

Initially:

    [0, 0, 0, 0, 0]

Indexes:

    0  1  2  3  4

---

## Important

This:

    new int[5]

means:

> Create an integer array capable of storing 5 elements.

It does NOT mean:

    indexes 1 to 5

Instead:

    indexes 0 to 4

---

# 5. 🎯 Initialization

You can initialize elements individually.

    int[] arr = new int[5];

    arr[0] = 10;
    arr[1] = 20;
    arr[2] = 30;
    arr[3] = 40;
    arr[4] = 50;

Final array:

    [10, 20, 30, 40, 50]

---

## Array Literal

You can also directly initialize:

    int[] arr = {10, 20, 30, 40, 50};

The compiler determines the length automatically.

Therefore:

    arr.length

is:

    5

---

# 6. 🧩 Declaration + Creation + Initialization

These are separate concepts.

## Declaration

    int[] arr;

Only the reference variable is declared.

---

## Creation

    arr = new int[5];

The array object is created.

---

## Initialization

    arr[0] = 10;
    arr[1] = 20;

Values are assigned.

---

## Combined

    int[] arr = {10, 20, 30};

This performs declaration and initialization together, with the array object created as part of the array initializer expression.

---

# 7. 🔢 Indexing

Java uses **zero-based indexing**.

Example:

    int[] arr = {100, 200, 300, 400};

    Index:    0    1    2    3
    Value:  100  200  300  400

Therefore:

    arr[0] → 100
    arr[1] → 200
    arr[2] → 300
    arr[3] → 400

---

## Formula

For an array of length `n`:

    Minimum index = 0

    Maximum index = n - 1

---

# 8. 👀 Accessing Elements

Use:

    arr[index]

Example:

    int[] nums = {10, 20, 30};

    System.out.println(nums[0]);

Output:

    10

Another example:

    System.out.println(nums[2]);

Output:

    30

---

## Accessing the Last Element

Instead of:

    nums[4]

we can use:

    nums[nums.length - 1]

This is useful when the array length is unknown.

---

# 9. ✏️ Updating Elements

Arrays are mutable.

Example:

    int[] nums = {10, 20, 30};

    nums[1] = 99;

Now:

    [10, 99, 30]

The element at index `1` was replaced.

---

## Important

This does not create a new array.

The same array object is modified.

---

# 10. 🔄 Traversing an Array

Traversal means:

> Visiting each element of an array one by one.

Example:

    int[] nums = {10, 20, 30, 40};

Traversal:

    10
    20
    30
    40

The most common ways are:

    1. Traditional for loop
    2. Enhanced for loop

---

# 11. 🔁 Traditional for Loop

The traditional loop gives you access to the index.

    int[] nums = {10, 20, 30, 40};

    for (int i = 0; i < nums.length; i++) {
        System.out.println(nums[i]);
    }

Output:

    10
    20
    30
    40

---

## Why `i < nums.length`?

Suppose:

    nums.length = 4

Valid indexes:

    0
    1
    2
    3

So:

    i < 4

allows:

    0, 1, 2, 3

But:

    i <= 4

would eventually access:

    nums[4]

which is invalid.

---

# 12. 🚀 Enhanced for Loop

Also called:

> for-each loop

Syntax:

    for (type variable : array) {
        // body
    }

Example:

    int[] nums = {10, 20, 30, 40};

    for (int num : nums) {
        System.out.println(num);
    }

Output:

    10
    20
    30
    40

---

## How to Read It

    for (int num : nums)

means:

> For every element of `nums`, place its value into `num`.

---

# 13. ⚔️ Difference Between for and Enhanced for

| Feature | Traditional `for` | Enhanced `for` |
|---|---|---|
| Index available | ✅ | ❌ |
| Direct value available | Yes | Yes |
| Easy traversal | Yes | Yes |
| Update using index | ✅ | ❌ |
| Simpler syntax | ❌ | ✅ |
| Reverse traversal | Easy | Not directly |
| Skip selected indexes | Easy | Less convenient |

---

## When Should You Use Traditional `for`?

Use it when you need the index.

Example:

    for (int i = 0; i < arr.length; i++) {
        if (arr[i] == target) {
            System.out.println(i);
        }
    }

---

## When Should You Use Enhanced `for`?

Use it when you only need the values.

Example:

    for (int value : arr) {
        System.out.println(value);
    }

---

# 14. ⌨️ Taking Array Input

Using `Scanner`:

    Scanner sc = new Scanner(System.in);

    int n = sc.nextInt();

    int[] arr = new int[n];

    for (int i = 0; i < arr.length; i++) {
        arr[i] = sc.nextInt();
    }

The user provides:

    5
    10 20 30 40 50

The array becomes:

    [10, 20, 30, 40, 50]

---

## Important Pattern

Remember this pattern:

    int n = sc.nextInt();

    int[] arr = new int[n];

    for (int i = 0; i < n; i++) {
        arr[i] = sc.nextInt();
    }

This pattern appears constantly in DSA.

---

# 15. 🖨️ Printing an Array

If you directly print an array:

    int[] arr = {10, 20, 30};

    System.out.println(arr);

you do NOT get:

    [10, 20, 30]

Instead, you generally get a type/hash-style representation.

For readable output, use:

    Arrays.toString(arr)

Example:

    System.out.println(Arrays.toString(arr));

Output:

    [10, 20, 30]

The `Arrays` class will be covered in detail in:

    05-Arrays-Class.md

---

# 16. ➕ Finding Sum

Example:

    int[] arr = {10, 20, 30, 40};

    int sum = 0;

    for (int i = 0; i < arr.length; i++) {
        sum += arr[i];
    }

    System.out.println(sum);

Output:

    100

---

## Logic

Start:

    sum = 0

Then:

    sum = 0 + 10
    sum = 10 + 20
    sum = 30 + 30
    sum = 60 + 40

Final:

    100

---

# 17. 📈 Finding Maximum and Minimum

## Maximum

    int[] arr = {10, 50, 20, 80, 30};

    int max = arr[0];

    for (int i = 1; i < arr.length; i++) {
        if (arr[i] > max) {
            max = arr[i];
        }
    }

    System.out.println(max);

Output:

    80

---

## Minimum

    int[] arr = {10, 50, 20, 80, 30};

    int min = arr[0];

    for (int i = 1; i < arr.length; i++) {
        if (arr[i] < min) {
            min = arr[i];
        }
    }

    System.out.println(min);

Output:

    10

---

## Why Start With `arr[0]`?

Because we need an actual element as the initial comparison value.

For maximum:

    max = arr[0]

For minimum:

    min = arr[0]

This works correctly even when the array contains negative numbers.

---

# 18. 🔍 Searching

Searching means checking whether a target value exists in the array.

Example:

    int[] arr = {10, 20, 30, 40};

    int target = 30;

We need to determine whether:

    30

exists.

---

# 19. 🔎 Linear Search

The simplest searching technique for an unsorted array is:

> Linear Search

We check each element one by one.

Example:

    int[] arr = {10, 20, 30, 40};

    int target = 30;

    for (int i = 0; i < arr.length; i++) {

        if (arr[i] == target) {
            System.out.println("Found at index " + i);
            break;
        }
    }

Output:

    Found at index 2

---

## Linear Search Complexity

Best case:

    O(1)

Worst case:

    O(n)

Average case:

    O(n)

---

# 20. 📋 Array Copying

Suppose:

    int[] a = {10, 20, 30};

We want another array containing the same values.

There are different concepts of copying.

---

# 21. 🔗 Reference Copy vs Actual Copy

## Reference Copy

    int[] a = {10, 20, 30};

    int[] b = a;

Now:

    a ──────┐
            ↓
        [10,20,30]
            ↑
            └────── b

Both references point to the same array object.

If:

    b[0] = 99;

then:

    a[0]

is also:

    99

---

## Actual Copy

A separate array object can be created.

Conceptually:

    a → [10,20,30]

    b → [10,20,30]

Now modifying `b` does not modify `a`.

Different copying techniques include:

    Arrays.copyOf()
    System.arraycopy()
    clone()

These APIs are discussed in detail later.

---

# 22. 📤 Passing Array to a Method

Arrays can be passed to methods.

Example:

    static void printArray(int[] arr) {

        for (int value : arr) {
            System.out.println(value);
        }
    }

Call:

    int[] nums = {10, 20, 30};

    printArray(nums);

---

## Important Concept

The array variable is passed by value, but the copied value is a reference to the same array object.

Therefore, a method can modify the array elements.

Example:

    static void change(int[] arr) {
        arr[0] = 100;
    }

    int[] nums = {10, 20, 30};

    change(nums);

Now:

    nums[0]

is:

    100

---

# 23. 📥 Returning an Array

A method can return an array.

Example:

    static int[] createArray() {

        int[] arr = {10, 20, 30};

        return arr;
    }

Calling:

    int[] nums = createArray();

Now:

    nums

refers to the returned array object.

---

## Example — Return Squares

    static int[] squares(int[] arr) {

        int[] result = new int[arr.length];

        for (int i = 0; i < arr.length; i++) {
            result[i] = arr[i] * arr[i];
        }

        return result;
    }

---

# 24. 🔧 Arrays with Methods

Arrays are commonly used with methods.

Example:

    static int sum(int[] arr) {

        int sum = 0;

        for (int value : arr) {
            sum += value;
        }

        return sum;
    }

Call:

    int[] nums = {10, 20, 30};

    System.out.println(sum(nums));

Output:

    60

---

# 25. 🔗 Array Aliasing

Aliasing occurs when multiple references refer to the same object.

Example:

    int[] a = {10, 20, 30};

    int[] b = a;

Now:

    a == b

is:

    true

because both references point to the same array.

---

## Visual

    a ─────┐
           ↓
       [10,20,30]
           ↑
    b ─────┘

Changing through either reference changes the same object.

---

# 26. ⚖️ Array Comparison

Do not use:

    arr1 == arr2

to compare array contents.

`==` checks whether both references point to the same array object.

Example:

    int[] a = {1, 2, 3};
    int[] b = {1, 2, 3};

    System.out.println(a == b);

Output:

    false

Even though contents are identical.

---

For content comparison, use:

    Arrays.equals(a, b)

which returns:

    true

The `Arrays` class is covered later.

---

# 27. 🔢 Array of Primitive Values

Example:

    int[] numbers = {10, 20, 30};

Conceptually:

    numbers
       ↓
    ┌────┬────┬────┐
    │ 10 │ 20 │ 30 │
    └────┴────┴────┘

The array contains integer values.

---

# 28. 👥 Array of References

Example:

    String[] names = new String[3];

Initially:

    [null, null, null]

Then:

    names[0] = "Java";
    names[1] = "Python";
    names[2] = "C++";

Conceptually:

    names
      ↓
    ┌───────┬────────┬───────┐
    │   ↓   │   ↓    │   ↓   │
    └───┼───┴───┼────┴───┼───┘
        ↓       ↓        ↓
      "Java" "Python"  "C++"

The array contains references to String objects.

---

# 29. 🚫 Null and One-Dimensional Arrays

Example:

    int[] arr = null;

The reference doesn't point to an array object.

This causes:

    arr.length

to throw:

    NullPointerException

---

## Empty Array vs Null Array

These are different:

    int[] a = new int[0];

and:

    int[] b = null;

### Empty array

An array object exists.

Length:

    0

### Null reference

No array object is referenced.

---

# 30. 💥 Common Exceptions

## 1. ArrayIndexOutOfBoundsException

Example:

    int[] arr = {10, 20};

    System.out.println(arr[2]);

Invalid index.

---

## 2. NullPointerException

Example:

    int[] arr = null;

    System.out.println(arr.length);

No array object exists.

---

## 3. ArrayStoreException

Example:

    Object[] arr = new String[2];

    arr[0] = 100;

The runtime array type is String[].

---

# 31. ⏱️ Time Complexity

| Operation | Complexity |
|---|---:|
| Access by index | O(1) |
| Update by index | O(1) |
| Traverse | O(n) |
| Linear search | O(n) |
| Find max | O(n) |
| Find min | O(n) |
| Find sum | O(n) |
| Reverse | O(n) |
| Copy | O(n) |
| Insert in middle | O(n) |
| Delete from middle | O(n) |

---

# 32. ⚠️ Common Mistakes

## ❌ Mistake 1

    for (int i = 0; i <= arr.length; i++)

Wrong.

Correct:

    for (int i = 0; i < arr.length; i++)

---

## ❌ Mistake 2

Thinking:

    arr.length()

Correct:

    arr.length

---

## ❌ Mistake 3

Thinking:

    int[] b = a;

creates a new array.

It does not.

It copies the reference.

---

## ❌ Mistake 4

Using `==` to compare contents.

Use:

    Arrays.equals()

for one-dimensional array content comparison.

---

## ❌ Mistake 5

Using:

    arr[0]

without checking whether the array has at least one element.

An empty array has:

    length = 0

and no valid index.

---

# 33. 🚨 Interview Traps

## Trap 1

    int[] arr = new int[5];

What is:

    arr.length?

Answer:

    5

---

## Trap 2

What is the last index?

Answer:

    4

---

## Trap 3

    int[] arr = new int[0];

Does the array exist?

Answer:

    Yes.

It is an empty array.

---

## Trap 4

    int[] arr = null;

Does an array object exist?

Answer:

    No object is referenced by arr.

---

## Trap 5

    int[] a = {1, 2, 3};
    int[] b = a;

    b[0] = 100;

What is:

    a[0]?

Answer:

    100

---

## Trap 6

    int[] a = {1, 2, 3};
    int[] b = {1, 2, 3};

    a == b

Answer:

    false

Different array objects.

---

## Trap 7

    int[] a = {1, 2, 3};
    int[] b = a;

    a == b

Answer:

    true

Same array object.

---

# 34. 🔥 Top 20 Interview Questions

## Q1. What is a one-dimensional array?

**Answer:**

A one-dimensional array is a linear collection of elements accessed using a single index.

---

## Q2. How does Java index arrays?

**Answer:**

Using zero-based indexing.

---

## Q3. How do you find the length?

**Answer:**

Using:

    arr.length

---

## Q4. Is length a method?

**Answer:**

No. For arrays, `length` is a field.

---

## Q5. What is the last valid index?

**Answer:**

    arr.length - 1

---

## Q6. How do you traverse an array?

**Answer:**

Using a traditional `for` loop or enhanced `for` loop.

---

## Q7. Difference between for and enhanced for?

**Answer:**

Traditional `for` provides direct index control, while enhanced `for` is simpler for value-based traversal.

---

## Q8. Can arrays be passed to methods?

**Answer:**

Yes.

---

## Q9. Can methods return arrays?

**Answer:**

Yes.

---

## Q10. What happens when an array is assigned to another variable?

**Answer:**

The reference is copied, not the array object.

---

## Q11. What is aliasing?

**Answer:**

When multiple references point to the same array object.

---

## Q12. How do you compare array contents?

**Answer:**

For one-dimensional arrays, use:

    Arrays.equals()

---

## Q13. What is the access complexity?

**Answer:**

Typically:

    O(1)

---

## Q14. What is linear search complexity?

**Answer:**

Worst case:

    O(n)

---

## Q15. Can an array have length zero?

**Answer:**

Yes.

Example:

    new int[0]

---

## Q16. What happens when an invalid index is used?

**Answer:**

`ArrayIndexOutOfBoundsException`.

---

## Q17. What happens when a null array reference is accessed?

**Answer:**

Usually `NullPointerException`.

---

## Q18. Can an array store objects?

**Answer:**

Yes. It stores references to objects.

---

## Q19. Can array size be changed?

**Answer:**

No.

A new array must be created.

---

## Q20. What is the difference between an empty array and null?

**Answer:**

An empty array is a real array object with length `0`; `null` means the reference does not refer to an array object.

---

# 35. 💻 Coding Problems

## Problem 1 — Print All Elements

    int[] arr = {10, 20, 30, 40};

    for (int i = 0; i < arr.length; i++) {
        System.out.println(arr[i]);
    }

---

## Problem 2 — Find Sum

    int[] arr = {10, 20, 30};

    int sum = 0;

    for (int value : arr) {
        sum += value;
    }

    System.out.println(sum);

Output:

    60

---

## Problem 3 — Find Maximum

    int[] arr = {10, 50, 20, 80, 30};

    int max = arr[0];

    for (int i = 1; i < arr.length; i++) {

        if (arr[i] > max) {
            max = arr[i];
        }
    }

    System.out.println(max);

Output:

    80

---

## Problem 4 — Find Minimum

    int[] arr = {10, 50, 20, 80, 30};

    int min = arr[0];

    for (int i = 1; i < arr.length; i++) {

        if (arr[i] < min) {
            min = arr[i];
        }
    }

    System.out.println(min);

Output:

    10

---

## Problem 5 — Count Even Numbers

    int[] arr = {10, 15, 20, 25, 30};

    int count = 0;

    for (int value : arr) {

        if (value % 2 == 0) {
            count++;
        }
    }

    System.out.println(count);

Output:

    3

---

## Problem 6 — Linear Search

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

Output:

    2

---

## Problem 7 — Reverse an Array

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

Final array:

    [50, 40, 30, 20, 10]

---

## Problem 8 — Count Occurrence of a Number

    int[] arr = {10, 20, 10, 30, 10};

    int target = 10;

    int count = 0;

    for (int value : arr) {

        if (value == target) {
            count++;
        }
    }

    System.out.println(count);

Output:

    3

---

# 36. 🎤 30-Second Interview Answer

> **A one-dimensional array in Java is a fixed-size linear collection of elements accessed using a single zero-based index. It can store primitive values or references to objects. We use `arr.length` to get its length, and accessing or updating an element by index is typically O(1). Arrays can be traversed using traditional or enhanced for loops, but their size cannot be changed after creation.**

---

# 37. 🧾 Cheat Sheet

## Declaration

    int[] arr;

## Creation

    arr = new int[5];

## Initialization

    int[] arr = {10, 20, 30};

## Access

    arr[index]

## Update

    arr[index] = value;

## Length

    arr.length

## First Element

    arr[0]

## Last Element

    arr[arr.length - 1]

## Traverse

    for (int i = 0; i < arr.length; i++)

## Enhanced Traverse

    for (int value : arr)

---

# 38. 🧠 Memory Tricks

## 🔥 Trick 1

For an array:

    Length = n
    First index = 0
    Last index = n - 1

---

## 🔥 Trick 2

Remember:

    Array → length
    String → length()
    Collection → size()

---

## 🔥 Trick 3

For loops:

    i < arr.length

Never normally:

    i <= arr.length

---

## 🔥 Trick 4

Reference assignment:

    b = a

means:

    Same object

not:

    New copy

---

## 🔥 Trick 5

For maximum/minimum:

    max = arr[0]
    min = arr[0]

Then compare remaining elements.

---

## 🔥 Trick 6

Searching:

    Unsorted array
         ↓
    Linear Search
         ↓
    O(n)

---

# 39. ✅ Final Revision Checklist

Before moving forward, make sure you can explain:

    [ ] What is a one-dimensional array?
    [ ] How to declare it?
    [ ] How to create it?
    [ ] How to initialize it?
    [ ] What is zero-based indexing?
    [ ] What is the last index?
    [ ] How to access an element?
    [ ] How to update an element?
    [ ] How to traverse?
    [ ] Traditional for loop
    [ ] Enhanced for loop
    [ ] Difference between both
    [ ] How to take input
    [ ] How to calculate sum
    [ ] How to find max/min
    [ ] What is linear search?
    [ ] How to pass an array to a method
    [ ] How to return an array
    [ ] What is array aliasing?
    [ ] Reference copy vs actual copy
    [ ] How to compare arrays
    [ ] Empty array vs null
    [ ] ArrayIndexOutOfBoundsException
    [ ] ArrayStoreException
    [ ] NullPointerException
    [ ] Array access complexity

---

# 🏆 MASTER MEMORY CARD

    ┌──────────────────────────────────────────────┐
    │          ONE-DIMENSIONAL ARRAY               │
    ├──────────────────────────────────────────────┤
    │ Linear structure                             │
    │ One index                                    │
    │ Zero-based                                   │
    │ Fixed size                                   │
    │ arr.length                                   │
    │ Access → O(1)                                │
    │ Search → O(n)                                │
    │ Allows duplicates                            │
    │ Mutable elements                             │
    │ Can contain primitives                       │
    │ Can contain object references                │
    └──────────────────────────────────────────────┘

---

# ⭐ ONE-LINE INTERVIEW DEFINITION

> **A one-dimensional array is a fixed-size linear object in Java whose elements are accessed using a single zero-based index.**

---

# 🔗 NEXT TOPIC

    05-Arrays/
    │
    ├── 01-Array-Introduction.md
    ├── 02-One-Dimensional-Array.md       ← YOU ARE HERE
    ├── 03-Multidimensional-Array.md
    ├── 04-Array-Memory.md
    ├── 05-Arrays-Class.md
    └── 06-Array-Interview-Questions.md

### Next:

> **03 — Multidimensional Array**

We will cover 2D arrays, matrices, rows and columns, nested arrays, jagged arrays, memory structure, traversal, input, output, and interview traps.