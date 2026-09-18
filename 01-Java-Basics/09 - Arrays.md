# ☕ 09 — Arrays

> An array is a fixed-size, indexed data structure used to store multiple values of the same type.

---

# 1. Why Do We Need Arrays?

Suppose we need to store 5 marks.

Without an array:

```java
int mark1 = 80;
int mark2 = 75;
int mark3 = 90;
int mark4 = 85;
int mark5 = 70;
```

With an array:

```java
int[] marks = {80, 75, 90, 85, 70};
```

Now we can process the values using loops.

```java
for (int mark : marks) {
    System.out.println(mark);
}
```

---

# 2. Important Properties

Java arrays have these important properties:

```text
1. Fixed size
2. Homogeneous elements
3. Zero-based indexing
4. Objects in Java
5. Direct index-based access
6. Can store primitives
7. Can store object references
8. Array length is fixed after creation
```

---

# 3. Array Declaration

```java
int[] numbers;
```

or:

```java
int numbers[];
```

The first style is generally preferred.

At this point, no array object has been created.

```text
numbers
   ↓
null
```

---

# 4. Array Creation

```java
int[] numbers = new int[5];
```

This creates an array capable of storing 5 `int` values.

Indexes:

```text
Index:   0   1   2   3   4
         ↓   ↓   ↓   ↓   ↓
        [0] [0] [0] [0] [0]
```

The valid indexes are:

```text
0 → length - 1
```

---

# 5. Array Initialization

```java
int[] numbers = {10, 20, 30, 40, 50};
```

This creates and initializes the array.

```text
Index:    0    1    2    3    4
Value:   10   20   30   40   50
```

---

# 6. Array Indexing

```java
int[] numbers = {10, 20, 30};

System.out.println(numbers[0]);
```

Output:

```text
10
```

Changing an element:

```java
numbers[1] = 100;
```

Array becomes:

```text
[10, 100, 30]
```

---

# 7. Array Index Starts at 0

For:

```java
int[] arr = new int[5];
```

indexes are:

```text
0
1
2
3
4
```

There is no index `5`.

Trying:

```java
System.out.println(arr[5]);
```

causes:

```text
ArrayIndexOutOfBoundsException
```

---

# 8. `length`

Every array has a `length` field.

```java
int[] arr = {10, 20, 30, 40};

System.out.println(arr.length);
```

Output:

```text
4
```

Important:

```java
arr.length
```

not:

```java
arr.length()
```

`length` is a field, not a method.

---

# 9. Array Size Is Fixed

Once created:

```java
int[] arr = new int[5];
```

its size cannot become 10.

You cannot do:

```java
arr.length = 10; // invalid
```

If you need a different size, create another array.

```java
int[] newArr = new int[10];
```

This fixed-size property is one major difference between arrays and dynamic collections such as `ArrayList`.

---

# 10. Default Values

When an array is created using `new`, its elements receive default values.

### Numeric types

```text
byte    → 0
short   → 0
int     → 0
long    → 0
float   → 0.0
double  → 0.0
```

### Other types

```text
char     → '\u0000'
boolean  → false
reference → null
```

Example:

```java
int[] arr = new int[3];

System.out.println(arr[0]);
```

Output:

```text
0
```

---

# 11. Array of Objects

Arrays can store object references.

```java
Student[] students = new Student[3];
```

This does **not** create three `Student` objects.

Initially:

```text
students
   ↓
[null] [null] [null]
```

You must create the objects separately:

```java
students[0] = new Student();
students[1] = new Student();
students[2] = new Student();
```

Now:

```text
students
   ↓
[Student] [Student] [Student]
```

---

# 12. Important Object Array Interview Trap

```java
Student[] students = new Student[3];

System.out.println(students[0].name);
```

This causes:

```text
NullPointerException
```

because:

```text
students[0] == null
```

No `Student` object exists at that index yet.

---

# 13. Primitive Array vs Object Array

Primitive array:

```java
int[] arr = new int[3];
```

Stores primitive values.

```text
[10] [20] [30]
```

Object array:

```java
Student[] arr = new Student[3];
```

Stores references.

```text
[ref] [ref] [null]
  ↓     ↓
 Obj   Obj
```

---

# 14. Array Is an Object

This is an important Java interview point.

```java
int[] arr = new int[5];
```

`arr` is a reference to an array object.

Therefore:

```java
System.out.println(arr.getClass());
```

can identify its runtime class.

Arrays are special JVM-supported objects.

---

# 15. Array Class Names

You may encounter:

```java
int[].class
```

and:

```java
String[].class
```

Arrays have JVM class representations.

For example:

```java
int[] arr = new int[3];

System.out.println(arr.getClass().getName());
```

The name is:

```text
[I
```

For:

```java
String[] arr = new String[3];
```

the class name is:

```text
[Ljava.lang.String;
```

These are JVM internal type descriptors.

---

# 16. Memory Concept

Consider:

```java
int[] arr = new int[5];
```

Conceptually:

```text
Stack                         Heap

arr ──────────────────────→  Array Object
                              ┌────┬────┬────┬────┬────┐
                              │ 0  │ 0  │ 0  │ 0  │ 0  │
                              └────┴────┴────┴────┴────┘
                               0    1    2    3    4
```

The local reference variable is associated with the current stack frame, while the array object is allocated in the heap.

---

# 17. Array Access Complexity

Direct index access:

```java
arr[i]
```

takes:

```text
O(1)
```

because Java can directly locate the requested array element using its index.

This is one of the biggest reasons arrays are important in DSA.

---

# 18. Traversal Complexity

```java
for (int i = 0; i < arr.length; i++) {
    System.out.println(arr[i]);
}
```

If the array contains `n` elements:

```text
Time → O(n)
```

because every element is visited.

---

# 19. Searching

Linear search:

```java
static int search(int[] arr, int target) {

    for (int i = 0; i < arr.length; i++) {

        if (arr[i] == target) {
            return i;
        }
    }

    return -1;
}
```

Complexity:

```text
Best case  → O(1)
Worst case → O(n)
```

If the array is sorted, binary search can reduce searching to:

```text
O(log n)
```

---

# 20. Insertion in Array

Arrays have fixed size.

If you want to insert an element into the middle of an existing logical array, elements may need to be shifted.

Example:

```text
Before:

[10, 20, 30, 40]

Insert 25 at index 2

After:

[10, 20, 25, 30, 40]
```

Elements after the insertion position must move.

Typical complexity:

```text
O(n)
```

---

# 21. Deletion in Array

Deleting from the middle also requires shifting.

```text
Before:

[10, 20, 30, 40]

Delete index 1

After:

[10, 30, 40]
```

Typical complexity:

```text
O(n)
```

---

# 22. Multidimensional Arrays

Java supports arrays of arrays.

Example:

```java
int[][] matrix = new int[3][4];
```

Conceptually:

```text
3 rows
×
4 columns
```

```text
[0 0 0 0]
[0 0 0 0]
[0 0 0 0]
```

Access:

```java
matrix[1][2]
```

---

# 23. Java Multidimensional Arrays Are Arrays of Arrays

This is a very important interview point.

Java does not technically implement a multidimensional array as one rectangular block in the same way some languages describe it.

For:

```java
int[][] arr;
```

the outer array contains references to inner arrays.

Conceptually:

```text
arr
 ↓
[ref] ──→ [10 20 30]
[ref] ──→ [40 50]
[ref] ──→ [60 70 80 90]
```

Therefore rows can have different lengths.

---

# 24. Jagged Array

Example:

```java
int[][] arr = new int[3][];

arr[0] = new int[2];
arr[1] = new int[4];
arr[2] = new int[3];
```

This creates:

```text
Row 0 → [0, 0]
Row 1 → [0, 0, 0, 0]
Row 2 → [0, 0, 0]
```

Such an array is called a:

> Jagged array

or irregular array.

---

# 25. `arr.length` vs `arr[i].length`

For:

```java
int[][] arr = new int[3][4];
```

```java
arr.length
```

gives:

```text
3
```

Number of rows.

```java
arr[0].length
```

gives:

```text
4
```

Number of elements in row 0.

For jagged arrays, each row can have a different length.

---

# 26. Traversing 2D Array

```java
for (int i = 0; i < arr.length; i++) {

    for (int j = 0; j < arr[i].length; j++) {

        System.out.print(arr[i][j] + " ");
    }

    System.out.println();
}
```

Using:

```java
arr[i].length
```

is safer for jagged arrays than assuming every row has the same length.

---

# 27. Array Reference Assignment

Consider:

```java
int[] a = {10, 20, 30};

int[] b = a;
```

This does **not** create a new array.

Both references point to the same array.

```text
a ───────┐
         ↓
      [10 20 30]
         ↑
b ───────┘
```

Therefore:

```java
b[0] = 100;
```

causes:

```java
System.out.println(a[0]);
```

to print:

```text
100
```

---

# 28. Shallow Copy Concept

Reference assignment:

```java
int[] b = a;
```

is not an independent copy.

It simply copies the reference.

This is sometimes described as aliasing rather than copying the array contents.

---

# 29. Copying an Array

To create a separate array, you can use:

```java
int[] b = a.clone();
```

or:

```java
int[] b = Arrays.copyOf(a, a.length);
```

or:

```java
int[] b = Arrays.copyOfRange(a, 0, a.length);
```

These create a new array object.

---

# 30. `clone()`

Example:

```java
int[] a = {10, 20, 30};

int[] b = a.clone();

b[0] = 100;

System.out.println(a[0]);
```

Output:

```text
10
```

Because `b` is a separate array.

---

# 31. Important `clone()` Point

For a primitive array:

```java
int[] a
```

`clone()` creates a separate array containing copied primitive values.

For an object array:

```java
Student[] a
```

the array itself is copied, but the object references inside it are copied.

Therefore, the contained objects are still shared.

This is an important **shallow-copy** concept.

---

# 32. `Arrays` Utility Class

Java provides:

```java
java.util.Arrays
```

for useful array operations.

Common methods include:

```text
sort()
binarySearch()
equals()
deepEquals()
fill()
copyOf()
copyOfRange()
toString()
deepToString()
asList()
```

---

# 33. `Arrays.toString()`

Directly printing an array:

```java
int[] arr = {10, 20, 30};

System.out.println(arr);
```

does not print its contents in normal readable form.

Use:

```java
System.out.println(Arrays.toString(arr));
```

Output:

```text
[10, 20, 30]
```

---

# 34. `Arrays.sort()`

```java
int[] arr = {50, 10, 30, 20};

Arrays.sort(arr);

System.out.println(Arrays.toString(arr));
```

Output:

```text
[10, 20, 30, 50]
```

For primitive arrays, the common implementation uses an optimized sorting algorithm; for object arrays, Java uses a stable TimSort-based approach in the relevant APIs.

For interviews, remember:

```text
Arrays.sort()
→ sorts array
```

and always check the exact overload/type if asked for implementation details.

---

# 35. `Arrays.binarySearch()`

```java
int[] arr = {10, 20, 30, 40, 50};

int index = Arrays.binarySearch(arr, 30);

System.out.println(index);
```

Output:

```text
2
```

Important:

> Binary search expects the array to be sorted for meaningful results.

---

# 36. `Arrays.copyOf()`

```java
int[] arr = {10, 20, 30};

int[] copy = Arrays.copyOf(arr, 5);

System.out.println(Arrays.toString(copy));
```

Output:

```text
[10, 20, 30, 0, 0]
```

The new array has length 5.

---

# 37. `Arrays.copyOfRange()`

Syntax:

```java
Arrays.copyOfRange(original, from, to)
```

Important:

> `from` is inclusive, `to` is exclusive.

Example:

```java
int[] arr = {10, 20, 30, 40, 50};

int[] copy = Arrays.copyOfRange(arr, 1, 4);
```

Result:

```text
[20, 30, 40]
```

Indexes:

```text
1 → included
2 → included
3 → included
4 → excluded
```

Memory trick:

```text
[from, to)
```

---

# 38. `Arrays.equals()`

```java
int[] a = {10, 20, 30};
int[] b = {10, 20, 30};

System.out.println(Arrays.equals(a, b));
```

Output:

```text
true
```

It compares corresponding elements.

---

# 39. `==` vs `Arrays.equals()`

```java
int[] a = {1, 2, 3};
int[] b = {1, 2, 3};

System.out.println(a == b);
```

Output:

```text
false
```

Because `a` and `b` refer to different array objects.

But:

```java
System.out.println(Arrays.equals(a, b));
```

prints:

```text
true
```

because their contents are equal.

---

# 40. `Arrays.fill()`

```java
int[] arr = new int[5];

Arrays.fill(arr, 10);

System.out.println(Arrays.toString(arr));
```

Output:

```text
[10, 10, 10, 10, 10]
```

---

# 41. Passing Array to a Method

Arrays can be passed as arguments.

```java
static void printArray(int[] arr) {

    for (int x : arr) {
        System.out.println(x);
    }
}
```

Call:

```java
int[] numbers = {10, 20, 30};

printArray(numbers);
```

The method receives a copy of the array reference.

Therefore, Java's pass-by-value rule still applies.

---

# 42. Modifying Array Inside Method

```java
static void change(int[] arr) {

    arr[0] = 100;
}
```

Then:

```java
int[] numbers = {10, 20, 30};

change(numbers);

System.out.println(numbers[0]);
```

Output:

```text
100
```

Why?

The copied reference points to the same array object.

---

# 43. Reassigning Array Inside Method

```java
static void change(int[] arr) {

    arr = new int[]{100, 200, 300};
}
```

Then:

```java
int[] numbers = {10, 20, 30};

change(numbers);

System.out.println(Arrays.toString(numbers));
```

Output:

```text
[10, 20, 30]
```

The method changed only its local copy of the reference.

This is the same pass-by-value principle discussed in Methods.

---

# 44. Array Covariance

Java arrays are covariant.

If:

```java
Dog extends Animal
```

then:

```java
Dog[] dogs = new Dog[3];

Animal[] animals = dogs;
```

is allowed.

Because:

```text
Dog[] IS-A Animal[]
```

---

# 45. Array Covariance Trap

Consider:

```java
Animal[] animals = new Dog[3];

animals[0] = new Dog();      // valid
animals[1] = new Animal();   // problem
```

The second line throws:

```text
ArrayStoreException
```

at runtime.

Why?

The actual array object is:

```text
Dog[]
```

Even though the reference type is:

```text
Animal[]
```

The JVM checks the runtime component type when storing into the array.

---

# 46. Arrays vs Generics

Important interview comparison:

Arrays:

```java
Dog[] dogs = new Dog[3];
Animal[] animals = dogs;
```

are covariant.

Generic types such as:

```java
List<Dog>
```

are not automatically:

```java
List<Animal>
```

This difference becomes important when studying generics and PECS.

---

# 47. Variable-Length Arrays?

Java does not have C-style variable-length arrays whose size changes automatically.

This:

```java
int n = 10;
int[] arr = new int[n];
```

is valid because `n` is used to determine the size **when creating the array**.

But after creation:

```text
size remains fixed
```

---

# 48. Array vs ArrayList

| Array                         | ArrayList                             |
| ----------------------------- | ------------------------------------- |
| Fixed size                    | Dynamic size                          |
| Can store primitives directly | Stores objects/wrappers               |
| `arr.length`                  | `list.size()`                         |
| `arr[index]`                  | `list.get(index)`                     |
| Lower-level structure         | Collection Framework class            |
| Can have primitive arrays     | Requires wrapper types for primitives |

Example:

```java
int[] arr = {1, 2, 3};

ArrayList<Integer> list =
        new ArrayList<>();
```

We will study `ArrayList` deeply in the Collection Framework section.

---

# 49. Arrays and Generics

You cannot directly create a generic array like:

```java
T[] arr = new T[10];
```

because Java's generics are implemented using type erasure, while array creation requires a reifiable runtime component type.

Typical alternatives involve:

```java
Object[]
```

or creating arrays with a supplied runtime type.

This becomes much more important in the Generics section.

---

# 50. Common Exceptions Related to Arrays

### `ArrayIndexOutOfBoundsException`

Accessing an invalid index.

```java
arr[10];
```

when valid indexes are only:

```text
0 ... 4
```

---

### `NullPointerException`

Trying to access through a null array reference.

```java
int[] arr = null;

System.out.println(arr.length);
```

---

### `ArrayStoreException`

Storing an incompatible object into a covariant array.

```java
Animal[] a = new Dog[3];

a[0] = new Animal();
```

---

# 51. Interview Questions — Complete Set

## Fundamentals

### Q1. What is an array?

A fixed-size indexed structure that stores elements of a specified component type.

### Q2. Why does array indexing start at 0?

It follows the conventional zero-based indexing model, where an index represents an offset from the first element.

### Q3. Is an array an object in Java?

Yes.

### Q4. Is array size fixed?

Yes, after creation.

### Q5. Can an array store primitives?

Yes.

### Q6. Can an array store objects?

Yes, as references.

### Q7. What is the default value of an `int` array?

`0`.

### Q8. What is the default value of a reference-type array?

`null`.

### Q9. What is the difference between `length` and `length()`?

`length` is an array field.

`length()` is a method used by certain types such as `String`.

---

# 52. Memory / JVM Questions

### Q10. Where is an array stored?

The array object is allocated on the heap; a reference to it may be held in a local variable, field, or another object.

### Q11. Are array elements stored contiguously?

For Java's conceptual/programming model, arrays provide indexed element storage with predictable element addressing; JVM implementation details should not be oversimplified into a universal physical-memory guarantee.

### Q12. What happens when an array is created?

Memory is allocated for the array object and its elements are initialized to their default values.

### Q13. Does `new Student[5]` create five Student objects?

No. It creates an array containing five null references.

---

# 53. Complexity Questions

### Q14. What is array access complexity?

```text
O(1)
```

### Q15. What is traversal complexity?

```text
O(n)
```

### Q16. What is linear search complexity?

```text
Best  → O(1)
Worst → O(n)
```

### Q17. What is binary search complexity?

```text
O(log n)
```

when applied appropriately to sorted data.

### Q18. Why is insertion in the middle generally O(n)?

Because elements may need to be shifted.

### Q19. Why is deletion in the middle generally O(n)?

Because elements may need to be shifted.

---

# 54. Reference Questions

### Q20. What happens here?

```java
int[] b = a;
```

Both references point to the same array.

### Q21. Does `int[] b = a` copy the array?

No.

### Q22. How do you create an independent array copy?

Examples:

```java
a.clone();
Arrays.copyOf(a, a.length);
Arrays.copyOfRange(a, 0, a.length);
```

### Q23. What does `==` compare for arrays?

Reference identity.

### Q24. How do you compare array contents?

Use:

```java
Arrays.equals()
```

for one-dimensional arrays.

---

# 55. Multidimensional Questions

### Q25. What is a 2D array?

An array whose elements are themselves arrays.

### Q26. Are Java multidimensional arrays true rectangular matrices internally?

Java represents them as arrays of arrays.

### Q27. What is a jagged array?

An array of arrays whose rows can have different lengths.

### Q28. Difference between:

```java
arr.length
```

and:

```java
arr[i].length
```

The first gives the number of rows in a 2D array; the second gives the length of a specific row.

---

# 56. Advanced Questions

### Q29. What is array covariance?

The ability to assign a subtype array to a supertype array reference.

```java
Animal[] a = new Dog[5];
```

### Q30. What problem can array covariance cause?

An invalid store can be detected only at runtime, producing `ArrayStoreException`.

### Q31. Why are arrays covariant but generics invariant?

Arrays retain runtime component-type information and are reified, while generic type arguments are generally erased at runtime.

### Q32. Can we create `new T[10]` in generic code?

Not directly, because the runtime component type of `T` is not known in the ordinary generic case.

### Q33. What is shallow copying?

The outer array is copied, but references inside an object array still point to the same objects.

### Q34. Can arrays be passed to methods?

Yes.

### Q35. Are arrays passed by reference in Java?

No.

Java passes the array reference value by value.

---

# 57. Output-Based Questions

## Q1

```java
int[] arr = new int[3];

System.out.println(arr[0]);
```

Output:

```text
0
```

---

## Q2

```java
String[] arr = new String[3];

System.out.println(arr[0]);
```

Output:

```text
null
```

---

## Q3

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

Both references point to the same array.

---

## Q4

```java
int[] a = {1, 2, 3};
int[] b = {1, 2, 3};

System.out.println(a == b);
System.out.println(Arrays.equals(a, b));
```

Output:

```text
false
true
```

---

## Q5

```java
int[] a = {10, 20, 30};

int[] b = Arrays.copyOf(a, a.length);

b[0] = 100;

System.out.println(a[0]);
```

Output:

```text
10
```

---

## Q6

```java
int[] arr = {10, 20, 30, 40, 50};

int[] result = Arrays.copyOfRange(arr, 1, 4);

System.out.println(Arrays.toString(result));
```

Output:

```text
[20, 30, 40]
```

Remember:

```text
from → inclusive
to   → exclusive
```

---

## Q7

```java
int[][] arr = new int[3][4];

System.out.println(arr.length);
System.out.println(arr[0].length);
```

Output:

```text
3
4
```

---

## Q8

```java
int[][] arr = new int[3][];

arr[0] = new int[2];
arr[1] = new int[5];
arr[2] = new int[3];

System.out.println(arr[1].length);
```

Output:

```text
5
```

---

## Q9

```java
int[] arr = {1, 2, 3};

System.out.println(arr);
```

This does **not** normally print:

```text
[1, 2, 3]
```

Use:

```java
System.out.println(Arrays.toString(arr));
```

---

## Q10

```java
class Animal {}

class Dog extends Animal {}

Animal[] animals = new Dog[2];

animals[0] = new Animal();
```

Result:

```text
ArrayStoreException
```

because the actual array is a `Dog[]`.

---

# 58. Common Interview Traps

### Trap 1 — `length()`

```java
arr.length()
```

❌ Wrong.

Correct:

```java
arr.length
```

---

### Trap 2 — Array assignment

```java
int[] b = a;
```

❌ Not a copy.

Both point to the same array.

---

### Trap 3 — Object array creation

```java
Student[] students = new Student[5];
```

❌ Does not create five Student objects.

It creates five null references.

---

### Trap 4 — `==`

```java
a == b
```

checks whether both references point to the same array object.

It does not compare contents.

---

### Trap 5 — `Arrays.copyOfRange`

```java
Arrays.copyOfRange(arr, 1, 4)
```

means:

```text
1 included
4 excluded
```

---

### Trap 6 — 2D array

```java
int[][] arr = new int[3][4];
```

Don't think of it as a special primitive "matrix object".

It is an array whose elements are references to arrays.

---

### Trap 7 — `new int[5]`

Valid indexes:

```text
0–4
```

not:

```text
1–5
```

---

### Trap 8 — Array covariance

```java
Animal[] a = new Dog[5];
```

is valid, but:

```java
a[0] = new Animal();
```

can fail at runtime with `ArrayStoreException`.

---

# 🔥 TOP 10 VVVV IMPORTANT INTERVIEW QUESTIONS

### 1. Is an array an object in Java?

**Yes.**

Arrays are objects managed by the JVM.

---

### 2. Is array size fixed?

**Yes.**

Once created, its length cannot change.

---

### 3. What is the time complexity of array access?

```text
O(1)
```

using an index.

---

### 4. What is the difference between `arr.length` and `arr.length()`?

```text
arr.length
→ array field

arr.length()
→ not valid for arrays
```

---

### 5. What happens when you do `int[] b = a`?

No new array is created.

Both references point to the same array.

---

### 6. Is Java pass-by-reference when passing arrays?

**No.**

Java passes the array reference value by value.

---

### 7. What does `new Student[5]` create?

An array containing five `null` references.

It does not create five `Student` objects.

---

### 8. `==` vs `Arrays.equals()`?

```text
==               → reference identity
Arrays.equals()  → element/content comparison
```

---

### 9. What is a jagged array?

A multidimensional array where inner arrays can have different lengths.

---

### 10. What is array covariance and its major trap?

```java
Animal[] a = new Dog[5];
```

is allowed because arrays are covariant.

But storing an `Animal` that isn't a `Dog` can cause:

```text
ArrayStoreException
```

at runtime.

---

# 🎤 30-Second Interview Answer

> An array in Java is a fixed-size indexed object that stores elements of a particular component type. It provides O(1) index-based access and is heavily used in DSA. Arrays can store primitives or object references, and their elements receive default values when created. Java arrays are objects and are passed to methods by passing the reference value by value. Java also supports multidimensional arrays, which are technically arrays of arrays. Important interview concepts include array reference assignment, copying, `Arrays.equals()`, array covariance, jagged arrays, and `ArrayStoreException`.

---

# ⚡ Quick Revision

```text
ARRAY
↓
Fixed size
↓
Indexed
↓
0-based
↓
O(1) access
↓
Object in Java
```

```text
int[] arr = new int[5];

Indexes:
0 1 2 3 4

Length:
5
```

```text
arr.length
→ field

Arrays.toString(arr)
→ print contents

Arrays.equals(a, b)
→ compare contents

a == b
→ compare references
```

```text
int[] b = a
→ same array

a.clone()
→ new array

Arrays.copyOf()
→ new array

Arrays.copyOfRange()
→ new array
[from, to)
```

```text
2D array
↓
array of arrays
↓
can be jagged
```

```text
Animal[] a = new Dog[5]
→ valid

a[0] = new Animal()
→ ArrayStoreException
```

---

# 🧠 Memory Tricks

```text
ARRAY
"Fixed + Fast + Indexed"

length
→ Field

length()
→ Method

[FROM, TO)
→ From included
→ To excluded

==
→ Same array?

Arrays.equals()
→ Same contents?

new Student[5]
→ 5 references
→ NOT 5 objects

2D Array
→ Array of Arrays

Java
→ Passes reference VALUE
→ Never "pass-by-reference"
```

---

# 🚀 Next Topic

```text
09 → Arrays ✓

10 → Strings
```

Strings are especially important because Java's `String` is **immutable**, has the **String Pool**, special behavior with `==` vs `.equals()`, and has several interview traps.
