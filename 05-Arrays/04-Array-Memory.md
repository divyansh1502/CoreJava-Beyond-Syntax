# 🧠 Array Memory in Java

> **In Java, an array is an object created dynamically on the heap. A variable holding an array does not contain the complete array itself; it contains a reference to the array object.**

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
34. [Top 20 Interview Questions](#34--top-20-interview-questions)
35. [30-Second Interview Answer](#35--30-second-interview-answer)
36. [Cheat Sheet](#36--cheat-sheet)
37. [Memory Tricks](#37--memory-tricks)
38. [Final Revision Checklist](#38--final-revision-checklist)

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

It also helps answer common interview questions such as:

> Where is an array stored in Java?

> What does an array variable actually contain?

> Why does assigning one array to another not create a copy?

> Why can two array variables modify the same array?

---

# 2. 🧱 Array as an Object

One of the most important facts:

> **Arrays are objects in Java.**

Even though arrays have special syntax, they are still objects created at runtime.

Example:

    int[] arr = new int[5];

The `new` keyword creates an array object.

Conceptually:

    arr ───────────────► [0, 0, 0, 0, 0]
                         Array Object

The variable `arr` stores a reference to the array object.

---

# 3. 🔗 Reference Variable vs Array Object

Consider:

    int[] arr = new int[5];

There are two conceptual parts:

### Reference variable

    arr

### Array object

    new int[5]

Conceptually:

    Stack                    Heap

    arr ──────────────────► Array Object
                             [0][0][0][0][0]

The variable does not contain all five integers itself.

It contains a reference that allows the program to access the array object.

---

# 4. 🧠 Basic Memory Model

For learning purposes, we commonly visualize Java memory using:

    Stack
    Heap
    Method Area / Metaspace

For an array:

    int[] arr = new int[5];

Conceptually:

    STACK                         HEAP

    arr ───────────────────────► int[] object
                                  ┌───────────────┐
                                  │ 0 │ 0 │ 0 │ 0 │ 0 │
                                  └───────────────┘

The exact JVM implementation details can vary, so this diagram is a conceptual model rather than a promise about physical memory layout.

---

# 5. 📦 Where Is an Array Stored?

The array itself is an object, and Java array objects are allocated in the heap.

Example:

    int[] arr = new int[5];

Conceptually:

    arr
     │
     │ reference
     ▼
    Heap
    ┌─────────────────┐
    │ Array Object     │
    │ 0  0  0  0  0  │
    └─────────────────┘

Important:

> The array object is on the heap.

The local variable `arr` is a local reference variable, commonly represented in the current thread's stack frame.

---

# 6. ⚔️ Stack vs Heap

## Stack

The stack is associated with method execution and contains stack frames.

Local variables and references may be stored in stack frames.

Example:

    public static void main(String[] args) {

        int[] arr = new int[3];

    }

Conceptually:

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

---

## Heap

The heap is the runtime memory area where objects and arrays are allocated.

Example:

    new int[3]

creates an array object.

---

# 7. 🔢 Primitive Array Memory

Example:

    int[] arr = new int[4];

The array contains primitive `int` values.

Conceptually:

    arr ─────────► [10][20][30][40]

The elements themselves are integer values.

There is no separate object for each integer.

Example:

    arr[0] = 10;

means the first element contains the value `10`.

---

# 8. 🧩 Reference Array Memory

Consider:

    Student[] students = new Student[3];

This does NOT create three `Student` objects.

It creates an array capable of storing references to `Student` objects.

Initially:

    students
        │
        ▼
    [ null ][ null ][ null ]

To create students:

    students[0] = new Student();
    students[1] = new Student();

Now conceptually:

    students
       │
       ▼
    [ ref ][ ref ][ null ]
       │      │
       ▼      ▼
    Student  Student
    Object   Object

This distinction is extremely important.

---

# 9. 🔄 Array Variable Assignment

Consider:

    int[] a = {10, 20, 30};

    int[] b = a;

Many beginners think this creates a new array.

It does NOT.

It copies the reference.

Conceptually:

    a ──────┐
            │
            ▼
          [10][20][30]
            ▲
            │
    b ──────┘

Both variables refer to the same array object.

---

# 10. 👥 Two References Pointing to Same Array

Example:

    int[] a = {10, 20, 30};

    int[] b = a;

Now:

    a == b

returns:

    true

because both references point to the same array object.

Conceptually:

    a ─────────┐
               │
               ▼
             [10][20][30]
               ▲
               │
    b ─────────┘

---

# 11. 📋 Array Copy vs Reference Copy

This distinction is extremely important.

## Reference Copy

    int[] a = {1, 2, 3};

    int[] b = a;

This does:

    a ────────► [1,2,3]
                  ▲
                  │
    b ────────────┘

Only the reference is copied.

---

## Actual Array Copy

To create a separate array:

    int[] a = {1, 2, 3};

    int[] b = a.clone();

Now:

    a ─────► [1,2,3]

    b ─────► [1,2,3]

These are separate array objects.

Therefore:

    a != b

but:

    a[0] == b[0]

because both contain the value `1`.

---

# 12. ✏️ Changing Through Another Reference

Example:

    int[] a = {10, 20, 30};

    int[] b = a;

    b[0] = 100;

Now:

    System.out.println(a[0]);

Output:

    100

Why?

Because `a` and `b` refer to the same array.

Before:

    a ───────┐
             ▼
           [10][20][30]
             ▲
             │
    b ───────┘

After:

    a ───────┐
             ▼
          [100][20][30]
             ▲
             │
    b ───────┘

---

# 13. 🆕 `new` and Array Allocation

The `new` keyword creates a new array object.

Example:

    int[] a = new int[3];

Then:

    int[] b = new int[3];

These are two different array objects.

Conceptually:

    a ─────► [0][0][0]

    b ─────► [0][0][0]

Even though their contents are identical:

    a != b

because they are different objects.

---

# 14. 0️⃣ Default Values

When an array is created, its elements automatically receive default values.

For primitive types:

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

For reference types:

    null

Example:

    int[] arr = new int[3];

Result:

    [0, 0, 0]

Example:

    boolean[] arr = new boolean[3];

Result:

    [false, false, false]

Example:

    String[] arr = new String[3];

Result:

    [null, null, null]

---

# 15. 📏 The `length` Property

Every Java array has a built-in `length` property.

Example:

    int[] arr = new int[5];

    System.out.println(arr.length);

Output:

    5

Important:

    array.length

is a property.

It is NOT a method.

Therefore:

    arr.length

is correct.

This is wrong:

    arr.length()

because arrays do not have a `length()` method.

---

# 16. 🔢 Array Index and Memory

Suppose:

    int[] arr = {10, 20, 30, 40};

Indexes:

    0 → 10
    1 → 20
    2 → 30
    3 → 40

We access:

    arr[2]

to get:

    30

At the conceptual level, an array uses its index to locate the corresponding element efficiently.

This is why array element access is considered O(1).

---

# 17. ⚡ Why Array Access Is O(1)

Suppose:

    int[] arr = new int[1000];

Access:

    arr[500]

does not require scanning:

    arr[0]
    arr[1]
    arr[2]
    ...
    arr[499]

The array access mechanism directly identifies the requested index.

Therefore:

    arr[i]

has constant-time access:

    O(1)

This is one of the biggest advantages of arrays.

---

# 18. 📦 1D Array Memory

Consider:

    int[] arr = {10, 20, 30};

Conceptual model:

    Stack                         Heap

    arr ───────────────────────► Array Object
                                  ┌────┬────┬────┐
                                  │ 10 │ 20 │ 30 │
                                  └────┴────┴────┘
                                   0    1    2

The reference variable points to the array object.

---

# 19. 🧩 2D Array Memory

Consider:

    int[][] arr = {
        {1, 2, 3},
        {4, 5, 6}
    };

A common beginner misconception is:

    arr ─────► one giant rectangular block

Conceptually, Java instead has:

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

The outer array contains references to inner arrays.

---

# 20. 🧠 2D Array Is an Array of Arrays

This is one of the most important interview concepts.

Given:

    int[][] arr;

The outer array contains elements of type:

    int[]

Therefore:

    int[][]

can be understood conceptually as:

    array of int arrays

So:

    arr[0]

is an `int[]`.

And:

    arr[0][1]

is an `int`.

Think:

    arr
     ↓
    int[] references
     ↓
    individual int values

---

# 21. 🪚 Jagged Array Memory

Consider:

    int[][] arr = new int[3][];

    arr[0] = new int[2];
    arr[1] = new int[4];
    arr[2] = new int[3];

Conceptually:

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

Different rows can have different lengths.

Therefore:

    arr[0].length → 2
    arr[1].length → 4
    arr[2].length → 3

---

# 22. 🔍 2D Array Reference Diagram

Consider:

    int[][] matrix = {
        {10, 20},
        {30, 40},
        {50, 60}
    };

Conceptually:

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

Important:

    matrix[0]

returns the reference to Row 0.

    matrix[0][1]

accesses:

    Row 0 → index 1

which gives:

    20

---

# 23. ⚖️ Primitive vs Reference Arrays

This is an important distinction.

## Primitive Array

    int[] arr = {10, 20, 30};

The array contains primitive values:

    [10][20][30]

---

## Reference Array

    String[] arr = {"A", "B", "C"};

The array contains references to `String` objects:

    [ref][ref][ref]
      │    │    │
      ▼    ▼    ▼
     "A"  "B"  "C"

The array itself is still an object.

---

# 24. 👨‍💻 Array of Objects

Consider:

    class Student {

        String name;

        Student(String name) {
            this.name = name;
        }
    }

Now:

    Student[] students = new Student[3];

Initially:

    students
       ↓
    [null][null][null]

Create objects:

    students[0] = new Student("A");
    students[1] = new Student("B");

Now conceptually:

    students
       ↓
    [ref][ref][null]
      │    │
      ▼    ▼
    Student Student
      A       B

Important:

    new Student[3]

does NOT create three Student objects.

It only creates an array containing three `Student` references.

---

# 25. 🔤 Array of Strings

Example:

    String[] names = new String[3];

Initially:

    [null][null][null]

Then:

    names[0] = "Yash";
    names[1] = "Rahul";
    names[2] = "Aman";

The array stores references to String objects.

Conceptually:

    names
      ↓
    [ref][ref][ref]
      │    │    │
      ▼    ▼    ▼
    "Yash" "Rahul" "Aman"

Remember:

> The array contains references, not the complete `String` objects themselves.

---

# 26. 🚫 Null References

Consider:

    int[] arr = null;

Here:

    arr

does not refer to any array object.

If we execute:

    System.out.println(arr.length);

Java throws:

    NullPointerException

Similarly:

    arr[0]

also causes:

    NullPointerException

because there is no array object being referenced.

---

# 27. 🗑️ Garbage Collection and Arrays

Arrays are objects.

Therefore, they are eligible for garbage collection when they become unreachable.

Example:

    int[] arr = new int[1000000];

Later:

    arr = null;

If there are no other references to the array, the array object becomes unreachable.

Conceptually:

    arr ─────► [large array]

After:

    arr = null;

    arr ─────► null

The old array no longer has a reachable reference from the program.

It may eventually be reclaimed by the garbage collector.

Important:

> `arr = null` does not immediately destroy the array.

It simply removes that particular reference.

---

# 28. 📞 Array Memory and Method Calls

Consider:

    static void change(int[] arr) {

        arr[0] = 100;
    }

    public static void main(String[] args) {

        int[] nums = {10, 20, 30};

        change(nums);

        System.out.println(nums[0]);
    }

Output:

    100

Why?

Because the method receives a copy of the reference to the same array object.

Conceptually:

    main:
    nums ───────┐
                ▼
              [10,20,30]
                ▲
                │
    change: arr ┘

Inside the method:

    arr[0] = 100;

changes the shared array object.

---

# 29. 🎯 Java Is Pass-by-Value

This is an extremely important interview topic.

Java is always:

> **Pass-by-value.**

When an array is passed to a method, Java passes a copy of the reference value.

Example:

    int[] nums = {10, 20, 30};

    change(nums);

Conceptually:

    nums = reference A

    method receives:

    arr = copy of reference A

So:

    nums ───────┐
                ▼
              [10,20,30]
                ▲
                │
    arr ────────┘

Both references point to the same object.

---

# 30. ✏️ Changing Array Elements in a Method

Example:

    static void change(int[] arr) {

        arr[0] = 999;
    }

    public static void main(String[] args) {

        int[] nums = {10, 20, 30};

        change(nums);

        System.out.println(nums[0]);
    }

Output:

    999

The reference itself was passed by value, but the copied reference still points to the same array.

Therefore, modifying the object is visible to the caller.

---

# 31. 🔄 Reassigning an Array Reference in a Method

Now consider:

    static void change(int[] arr) {

        arr = new int[]{100, 200, 300};
    }

    public static void main(String[] args) {

        int[] nums = {10, 20, 30};

        change(nums);

        System.out.println(nums[0]);
    }

Output:

    10

Why didn't `nums` change?

Because:

    arr

inside the method is only a local copy of the reference.

Initially:

    nums ───────┐
                ▼
              [10,20,30]
                ▲
                │
    arr ────────┘

After:

    arr = new int[]{100,200,300};

we get:

    nums ───────────► [10,20,30]

    arr ────────────► [100,200,300]

The original `nums` reference still points to the original array.

---

# 32. ⚠️ Common Memory Mistakes

## ❌ Mistake 1

Thinking this creates two arrays:

    int[] a = {1, 2, 3};

    int[] b = a;

It doesn't.

There is only one array.

---

## ❌ Mistake 2

Thinking this creates Student objects:

    Student[] students = new Student[5];

It doesn't.

It creates five `Student` references initialized to `null`.

---

## ❌ Mistake 3

Thinking `arr.length` is a method.

Wrong:

    arr.length()

Correct:

    arr.length

---

## ❌ Mistake 4

Thinking a 2D array is always one rectangular memory block.

Java's 2D array is an array of arrays.

Rows are separate arrays.

---

## ❌ Mistake 5

Thinking:

    arr = null;

immediately destroys the array.

It doesn't.

It only makes `arr` stop referring to that array.

---

## ❌ Mistake 6

Thinking Java passes arrays by reference.

Technically incorrect.

Java passes the array reference **by value**.

---

# 33. 🚨 Interview Traps

## Trap 1

What happens here?

    int[] a = {1, 2, 3};
    int[] b = a;

Answer:

> Both references point to the same array.

---

## Trap 2

What does this create?

    Student[] students = new Student[5];

Answer:

> One array object containing five `Student` references, initially `null`.

It does not create five `Student` objects.

---

## Trap 3

What happens here?

    int[] arr = null;
    System.out.println(arr.length);

Answer:

    NullPointerException

---

## Trap 4

What is the difference between:

    int[] a = new int[3];

and:

    int[] b = a;

First:

    new int[3]

creates an array object.

Second:

    b = a

copies the reference.

---

## Trap 5

Why does modifying an array inside a method affect the original array?

Because the method receives a copy of the reference pointing to the same array object.

---

## Trap 6

Why doesn't reassigning the parameter affect the caller's variable?

Because the parameter contains a separate copy of the reference.

---

# 34. 🔥 Top 20 Interview Questions

## Q1. Are arrays objects in Java?

Yes. Arrays are objects created at runtime.

---

## Q2. Where are array objects allocated?

Array objects are allocated in the heap.

---

## Q3. What does an array variable store?

An array variable stores a reference to an array object.

---

## Q4. Does `int[] a = new int[5]` store the five integers directly in the variable `a`?

No. `a` is a reference to the array object.

---

## Q5. What does `new` do when used with arrays?

It creates a new array object and initializes its elements with default values.

---

## Q6. What is the default value of an `int` array?

`0`.

---

## Q7. What is the default value of a reference-type array?

`null`.

---

## Q8. Is `length` a method?

No. It is an array property.

Correct:

    arr.length

Incorrect:

    arr.length()

---

## Q9. What happens when two array variables are assigned to each other?

They can refer to the same array object.

Example:

    int[] a = {1, 2};
    int[] b = a;

---

## Q10. Does `b = a` create a copy of the array?

No. It copies the reference.

---

## Q11. How do you create a separate copy?

For example:

    int[] b = a.clone();

Other approaches include:

    Arrays.copyOf(a, a.length);

or:

    System.arraycopy(...);

---

## Q12. Why is array access O(1)?

Because an array provides direct indexed access to its elements.

---

## Q13. Is a 2D array a single rectangular object?

Conceptually, no. It is an array whose elements are references to row arrays.

---

## Q14. Can Java have jagged arrays?

Yes.

---

## Q15. Can an array contain `null`?

Yes, if its component type is a reference type.

Example:

    String[] arr = new String[3];

---

## Q16. Can primitive arrays contain `null`?

No.

For example:

    int[] arr = new int[3];

contains `0`, not `null`.

---

## Q17. Is Java pass-by-reference?

No.

Java is pass-by-value.

For arrays, the value being passed is a copy of the reference.

---

## Q18. Why can a method modify the caller's array?

Because both the caller's reference and the method's copied reference point to the same array object.

---

## Q19. Does assigning `null` immediately destroy an array?

No.

The array becomes eligible for garbage collection only when it is unreachable.

---

## Q20. What is the most important array-memory concept?

Remember:

    Reference variable
          ↓
    Array object

The variable and object are conceptually separate.

---

# 35. 🎤 30-Second Interview Answer

> **In Java, arrays are objects and are allocated on the heap. An array variable stores a reference to the array object rather than containing the entire array itself. When one array variable is assigned to another, only the reference is copied, so both variables can refer to the same array. A 2D array is actually an array of arrays, which is why Java supports jagged arrays. Array indexing provides constant-time O(1) access, and when an array is passed to a method, Java passes the reference by value.**

---

# 36. 🧾 Cheat Sheet

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
| 2D array | Array of arrays |
| Jagged array | Rows can have different lengths |
| Primitive array | Contains primitive values |
| Reference array | Contains references |
| `null` | No referenced object |
| GC | Reclaims unreachable objects |
| Java parameter passing | Pass-by-value |

---

# 37. 🧠 Memory Tricks

## 🔥 Trick 1 — Variable vs Object

Remember:

    Variable
       ↓
    Reference
       ↓
    Object

For arrays:

    arr
     ↓
    reference
     ↓
    array object

---

## 🔥 Trick 2 — `new` Means New Object

Whenever you see:

    new int[5]

think:

    NEW ARRAY OBJECT

---

## 🔥 Trick 3 — Assignment Doesn't Mean Copy

This:

    b = a;

means:

    Copy reference

not:

    Copy array

---

## 🔥 Trick 4 — 2D Array

Remember:

    int[][]

means:

    array
      ↓
    arrays
      ↓
    values

---

## 🔥 Trick 5 — Object Array

Remember:

    Student[] students = new Student[5];

means:

    5 references

NOT:

    5 Student objects

---

## 🔥 Trick 6 — Method Passing

Remember:

    Java → Pass-by-value

For arrays:

    copy of reference → same array object

---

# 38. ✅ Final Revision Checklist

Before moving to the next topic, make sure you can explain:

    [ ] Arrays are objects
    [ ] Arrays are allocated on the heap
    [ ] Array variables contain references
    [ ] Difference between reference and object
    [ ] What `new` does
    [ ] Default values of array elements
    [ ] What `length` means
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

---

# 🏆 MASTER MEMORY CARD

    ┌──────────────────────────────────────────────┐
    │             ARRAY MEMORY IN JAVA             │
    ├──────────────────────────────────────────────┤
    │ Array → Object                               │
    │ Array object → Heap                          │
    │ Variable → Reference                         │
    │ new → Creates array object                   │
    │ a = b → Reference copy                       │
    │ clone() → Separate array                     │
    │ arr[i] → O(1) access                         │
    │ 2D array → Array of arrays                   │
    │ Jagged array → Different row lengths        │
    │ Object[] → Array of references               │
    │ Java → Pass-by-value                         │
    │ Array argument → Copy of reference           │
    └──────────────────────────────────────────────┘

---

# ⭐ ONE-LINE INTERVIEW DEFINITION

> **In Java, an array is a heap-allocated object accessed through a reference variable, and multidimensional arrays are implemented as arrays whose elements are references to other arrays.**

---

# 🔗 ARRAY FOLDER PROGRESS

    05-Arrays/
    │
    ├── 01-Array-Introduction.md
    ├── 02-One-Dimensional-Array.md
    ├── 03-Multidimensional-Array.md
    ├── 04-Array-Memory.md              ← YOU ARE HERE
    ├── 05-Arrays-Class.md
    └── 06-Array-Interview-Questions.md

### Next:

> **05 — Arrays Class**

This will cover `java.util.Arrays`, sorting, searching, copying, filling, comparing, `toString()`, `deepToString()`, `equals()`, `deepEquals()`, and other important interview APIs.