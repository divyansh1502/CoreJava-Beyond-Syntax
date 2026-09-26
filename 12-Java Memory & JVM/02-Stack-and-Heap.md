# 🧠 Stack and Heap Memory in Java

> The **Stack** and **Heap** are two important runtime memory areas used by the JVM. The stack primarily manages **thread-specific execution frames**, while the heap is the main runtime area where **objects and arrays are allocated**.

---

## 📌 Table of Contents

1. [Introduction](#-introduction)
2. [Why Do We Need Stack and Heap?](#-why-do-we-need-stack-and-heap)
3. [Stack Memory](#-stack-memory)
4. [Stack Frames](#-stack-frames)
5. [What Does a Stack Frame Contain?](#-what-does-a-stack-frame-contain)
6. [Local Variables](#-local-variables)
7. [Method Parameters](#-method-parameters)
8. [References on the Stack](#-references-on-the-stack)
9. [Primitive Variables and Stack](#-primitive-variables-and-stack)
10. [Heap Memory](#-heap-memory)
11. [Objects on the Heap](#-objects-on-the-heap)
12. [Arrays on the Heap](#-arrays-on-the-heap)
13. [Primitive Array vs Wrapper Array](#-primitive-array-vs-wrapper-array)
14. [Reference Variables vs Objects](#-reference-variables-vs-objects)
15. [Stack and Heap Together](#-stack-and-heap-together)
16. [Method Call Example](#-method-call-example)
17. [Object Creation Example](#-object-creation-example)
18. [Passing Primitive Values](#-passing-primitive-values)
19. [Passing Object References](#-passing-object-references)
20. [Stack Lifecycle](#-stack-lifecycle)
21. [Heap Lifecycle](#-heap-lifecycle)
22. [StackOverflowError](#-stackoverflowerror)
23. [OutOfMemoryError](#-outofmemoryerror)
24. [Garbage Collection and Heap](#-garbage-collection-and-heap)
25. [Is Everything on Stack or Heap?](#-is-everything-on-stack-or-heap)
26. [JVM Implementation Details](#-jvm-implementation-details)
27. [Common Misconceptions](#-common-misconceptions)
28. [Interview Traps](#-interview-traps)
29. [Quick Comparison](#-quick-comparison)
30. [Cheat Sheet](#-cheat-sheet)
31. [30-Second Interview Answer](#-30-second-interview-answer)
32. [Top 10 Interview Questions](#-top-10-interview-questions)

---

# 🔹 Introduction

When a Java program runs, the JVM needs memory to manage:

- Method execution
- Local variables
- Method parameters
- Objects
- Arrays
- References
- Runtime data

Two commonly discussed JVM runtime memory areas are:

```text
                 JVM Runtime
                      |
             +--------+--------+
             |                 |
          Stack              Heap
             |                 |
       Thread-specific     Shared area
       method execution   Objects / Arrays
```

A very important point:

> **Stack and Heap have different purposes.**

They should not simply be remembered as:

> "Primitive = Stack, Object = Heap."

That shortcut is often too simplistic.

---

# 🔹 Why Do We Need Stack and Heap?

Consider:

```java
class Student {

    int age;

    Student(int age) {
        this.age = age;
    }
}

public class Main {

    public static void main(String[] args) {

        int x = 10;

        Student s = new Student(20);
    }
}
```

The JVM needs to manage:

```text
main() execution
      ↓
local variable x
      ↓
reference s
      ↓
Student object
```

Conceptually:

```text
Stack                          Heap

main() frame
┌───────────────┐
│ x = 10        │
│ s ─────────────┼──────────────→ Student object
└───────────────┘                  age = 20
```

The stack manages the execution context of `main()`.

The heap contains the dynamically allocated `Student` object.

---

# 🔹 Stack Memory

## Definition

> **The JVM stack is a thread-private runtime memory area that stores stack frames created for method invocations.**

Every Java thread has its own JVM stack.

Conceptually:

```text
             JVM
              |
      +-------+-------+
      |               |
   Thread 1        Thread 2
      |               |
   Stack 1          Stack 2
```

The stacks are not shared between threads.

---

# 🔹 Important Properties of Stack

| Property | Stack |
|---|---|
| Associated with | Individual thread |
| Main purpose | Method execution |
| Contains | Stack frames |
| Lifetime | Depends on thread |
| Allocation | Method/frame based |
| Access | Very fast conceptually |
| Garbage collected? | No |
| Typical failure | `StackOverflowError` |

---

# 🔹 Stack Frames

Whenever a method is invoked, the JVM creates a **new frame** for that method invocation.

Example:

```java
public static void main(String[] args) {
    calculate();
}

static void calculate() {
    int x = 10;
}
```

Conceptually:

```text
main()
   |
   ↓
calculate()
```

Stack:

```text
┌──────────────────────┐
│ calculate() frame    │
│ x = 10               │
├──────────────────────┤
│ main() frame         │
│ args                 │
└──────────────────────┘
```

The most recently created frame is at the top.

When `calculate()` returns:

```text
calculate() frame
       ↓
    removed
```

---

# 🔹 Stack Frame Lifecycle

For a method:

```java
calculate();
```

the conceptual lifecycle is:

```text
Method invocation
       ↓
Create stack frame
       ↓
Execute bytecode
       ↓
Method returns
       ↓
Frame discarded
```

Therefore:

> A stack frame exists only while its method invocation is active.

---

# 🔹 What Does a Stack Frame Contain?

The JVM specification defines a frame as containing conceptual components including:

```text
Stack Frame
│
├── Local Variables
├── Operand Stack
└── Reference to Runtime Constant Pool
```

These are important for understanding JVM bytecode execution.

---

# 🔹 Local Variables

Consider:

```java
static void calculate() {

    int x = 10;
    double price = 99.5;
    boolean active = true;
}
```

The method's local variables are associated with its stack frame.

Conceptually:

```text
calculate() frame
┌─────────────────────┐
│ x = 10              │
│ price = 99.5        │
│ active = true       │
└─────────────────────┘
```

---

# 🔹 Important: "Primitive = Stack" Is Not a Rule

This is a very common interview misconception.

People often memorize:

```text
Primitive → Stack
Object → Heap
```

This is not a reliable description of the Java specification.

A better mental model is:

```text
Stack
  ↓
Execution frames + local variables

Heap
  ↓
Objects + arrays
```

A primitive local variable can be represented in a stack frame.

But a primitive field belongs to an object and is part of that object's storage.

Example:

```java
class Student {
    int age;
}
```

If:

```java
Student s = new Student();
```

then the `age` field is part of the `Student` object.

Conceptually:

```text
Stack                    Heap

s ───────────────────→  Student
                         age = 20
```

So:

> The location of a variable depends on its role, not simply on whether its type is primitive.

---

# 🔹 Method Parameters

Consider:

```java
static int add(int a, int b) {

    return a + b;
}
```

The parameters:

```java
a
b
```

are associated with the method's frame/local-variable structure.

Conceptually:

```text
add() frame
┌──────────────┐
│ a = 10       │
│ b = 20       │
└──────────────┘
```

---

# 🔹 References on the Stack

Consider:

```java
Student student = new Student();
```

There are two different things here:

### Reference variable

```java
student
```

### Object

```java
new Student()
```

Conceptually:

```text
Stack                         Heap

student ───────────────────→ Student object
                             fields...
```

The reference variable is associated with the current execution context.

The object is allocated in the heap.

---

# 🔹 What Is a Reference?

A reference is a value that allows Java code to access an object.

Example:

```java
Student s = new Student();
```

Think of it conceptually as:

```text
s
│
│ reference
↓
Student object
```

Do not think:

```text
s = object itself
```

Instead:

```text
s = reference to object
```

---

# 🔹 Heap Memory

## Definition

> **The JVM heap is the runtime memory area from which memory for objects and arrays is allocated.**

The heap is shared among JVM threads.

Conceptually:

```text
             JVM Heap
                |
       +--------+--------+
       |        |        |
    Object A Object B  Array
```

Multiple threads may access objects in the heap if they have references to them.

---

# 🔹 Important Properties of Heap

| Property | Heap |
|---|---|
| Shared between threads | Yes |
| Main purpose | Object/array allocation |
| Stores objects | Yes |
| Stores arrays | Yes |
| Garbage collection | Yes |
| Lifetime | Objects can outlive method calls |
| Typical failure | `OutOfMemoryError` |

---

# 🔹 Objects on the Heap

Example:

```java
class Employee {

    String name;
    int salary;
}

Employee e = new Employee();
```

Conceptually:

```text
Stack                     Heap

e ───────────────────→  Employee object
                         |
                         ├── name
                         └── salary
```

The object can remain alive even after the method that created it returns, as long as it is still reachable.

---

# 🔹 Arrays on the Heap

This is especially important.

Consider:

```java
int[] numbers = new int[5];
```

The array itself is an object.

Therefore:

```text
numbers
   |
   ↓
int[] object
┌────────────────────┐
│ 0 │ 0 │ 0 │ 0 │ 0 │
└────────────────────┘
```

Conceptually:

```text
Stack                         Heap

numbers ─────────────────→ int[] object
                            [0, 0, 0, 0, 0]
```

---

# 🔥 Primitive Array vs Wrapper Array

This connects directly to a common confusion.

## Primitive Array

```java
int[] arr = new int[3];
```

The array is an object.

But its elements are primitive `int` values.

Conceptually:

```text
Heap
┌─────────────────────┐
│ int[] object        │
│                     │
│  10 │ 20 │ 30       │
└─────────────────────┘
```

There is **no autoboxing** involved.

---

## Wrapper Array

```java
Integer[] arr = new Integer[3];
```

The array itself is an object.

Its elements are references to `Integer` objects.

Conceptually:

```text
Stack                    Heap

arr ───────────────→ Integer[] array
                         |
                         +──→ Integer object
                         |
                         +──→ Integer object
```

So:

```text
int[]
```

contains primitive `int` elements.

Whereas:

```text
Integer[]
```

contains references to `Integer` objects.

---

# 🔥 Does an Array Cause Autoboxing?

No.

This:

```java
int[] arr = {10, 20, 30};
```

does **not** perform:

```text
int
 ↓
Integer
```

There is no automatic conversion to wrapper objects.

The array directly stores primitive `int` values.

Autoboxing happens in contexts where a primitive must be converted to its corresponding wrapper type.

Example:

```java
Integer x = 10;
```

Here:

```text
int 10
  ↓
Integer object
```

is autoboxing.

But:

```java
int[] arr = new int[5];
```

does not box each element.

---

# 🔹 Reference Variables vs Objects

This distinction is extremely important.

Consider:

```java
Student s = new Student();
```

Break it down:

```text
Student s
```

declares a reference variable.

And:

```java
new Student()
```

creates an object.

Conceptually:

```text
Stack                         Heap

s ─────────────────────────→ Student object
                              |
                              | fields
```

Therefore:

```text
Reference ≠ Object
```

---

# 🔹 Stack and Heap Together

Consider:

```java
class Student {

    int age;

    Student(int age) {
        this.age = age;
    }
}

public class Main {

    public static void main(String[] args) {

        int x = 10;

        Student s = new Student(20);

        change(s);
    }

    static void change(Student student) {

        student.age = 30;
    }
}
```

Conceptually:

```text
                    STACK
                      |
        +-------------+-------------+
        |                           |
   main() frame                change() frame
        |                           |
      x = 10                    student ────────┐
      s ────────────────────────────────────────┤
                                                ↓
                    HEAP
                      |
                Student object
                ┌──────────────┐
                │ age = 30     │
                └──────────────┘
```

Both:

```text
s
```

and:

```text
student
```

can refer to the same heap object during the method call.

---

# 🔹 Method Call Example

Consider:

```java
public static void main(String[] args) {

    int x = 10;

    calculate(x);
}

static void calculate(int value) {

    int result = value * 2;

    System.out.println(result);
}
```

Execution:

### Step 1

`main()` starts.

```text
Stack

┌───────────────┐
│ main()        │
│ x = 10        │
└───────────────┘
```

### Step 2

`calculate(10)` is called.

```text
Stack

┌────────────────────┐
│ calculate()        │
│ value = 10         │
│ result = 20        │
├────────────────────┤
│ main()             │
│ x = 10             │
└────────────────────┘
```

### Step 3

`calculate()` returns.

Its frame is discarded.

```text
Stack

┌───────────────┐
│ main()        │
│ x = 10        │
└───────────────┘
```

---

# 🔹 Object Creation Example

Consider:

```java
public static void main(String[] args) {

    Student s = new Student();

    s.age = 20;
}
```

Conceptually:

### Before object creation

```text
Stack

main()
```

### After:

```java
Student s = new Student();
```

Conceptually:

```text
Stack                     Heap

main()
s ───────────────────→ Student object
```

### After:

```java
s.age = 20;
```

```text
Stack                     Heap

main()
s ───────────────────→ Student
                         age = 20
```

---

# 🔹 Passing Primitive Values

Java is always **pass-by-value**.

Consider:

```java
static void change(int x) {
    x = 100;
}

public static void main(String[] args) {

    int value = 10;

    change(value);

    System.out.println(value);
}
```

Output:

```text
10
```

Why?

A copy of the value is passed.

Conceptually:

```text
main()                  change()

value = 10    ─copy→    x = 10

                         x = 100
```

The original:

```text
value = 10
```

is unchanged.

---

# 🔹 Passing Object References

Now:

```java
class Student {
    int age;
}

static void change(Student s) {
    s.age = 100;
}

public static void main(String[] args) {

    Student student = new Student();

    student.age = 20;

    change(student);

    System.out.println(student.age);
}
```

Output:

```text
100
```

Why?

Because Java passes the **reference value by value**.

Conceptually:

```text
main()                      change()

student ───────→ Object
                  ↑
                  |
              s ──┘
```

The reference itself is copied.

Both references point to the same object.

Therefore:

```java
s.age = 100;
```

modifies the shared object.

---

# 🔥 Important Interview Statement

Never say:

> "Java passes objects by reference."

The technically correct statement is:

> **Java is always pass-by-value. When an object is passed, the value being copied is the reference to that object.**

---

# 🔹 Stack Lifecycle

The stack is associated with a thread.

When a thread starts:

```text
Thread
  ↓
JVM Stack
```

When a method is called:

```text
Method call
    ↓
New frame
```

When it returns:

```text
Frame
  ↓
Discarded
```

When the thread terminates:

```text
Thread terminates
       ↓
Its JVM stack lifecycle ends
```

---

# 🔹 Heap Lifecycle

Objects have a different lifecycle.

Example:

```java
Student s = new Student();
```

Object:

```text
Created
  ↓
Reachable
  ↓
Used
  ↓
No longer reachable
  ↓
Eligible for GC
  ↓
Eventually reclaimed
```

Important:

> **Becoming unreachable does not mean the object is immediately destroyed.**

It becomes **eligible for garbage collection**.

---

# 🔹 StackOverflowError

A thread's JVM stack has a finite capacity.

Consider infinite recursion:

```java
public static void recursive() {

    recursive();
}
```

Each invocation creates another stack frame.

Conceptually:

```text
recursive()
recursive()
recursive()
recursive()
recursive()
...
```

Eventually:

```text
Stack capacity exceeded
        ↓
StackOverflowError
```

Example:

```java
public class Main {

    static void test() {
        test();
    }

    public static void main(String[] args) {
        test();
    }
}
```

Likely result:

```text
java.lang.StackOverflowError
```

---

# 🔹 OutOfMemoryError

Heap exhaustion can result in:

```text
java.lang.OutOfMemoryError
```

Example:

```java
List<byte[]> list = new ArrayList<>();

while (true) {
    list.add(new byte[1024 * 1024]);
}
```

The program continuously retains objects.

Eventually the JVM may be unable to allocate more heap memory.

Result:

```text
OutOfMemoryError
```

---

# 🔹 StackOverflowError vs OutOfMemoryError

| Feature | StackOverflowError | OutOfMemoryError |
|---|---|---|
| Common cause | Excessive stack usage | Insufficient memory for allocation |
| Typical example | Infinite recursion | Huge object allocation |
| Associated area | JVM stack | Often heap |
| Error type | `Error` | `Error` |
| Garbage collection necessarily solves it? | No | Not necessarily |

---

# 🔹 Garbage Collection and Heap

Garbage Collection primarily manages objects in the heap.

Consider:

```java
Student s = new Student();

s = null;
```

If there are no other references to the object:

```text
Student object
      ↓
No longer reachable
      ↓
Eligible for GC
```

The object is not immediately removed.

The JVM's garbage collector determines when and how memory is reclaimed.

---

# 🔹 Is Everything on Stack or Heap?

No.

This is another important interview trap.

The JVM has multiple runtime data areas.

Conceptually:

```text
JVM Runtime Data Areas
│
├── Heap
│
├── JVM Stacks
│
├── Method Area
│
├── Runtime Constant Pool
│
├── PC Registers
│
└── Native Method Stacks
```

So saying:

> "Everything is either stack or heap"

is incorrect.

---

# 🔹 JVM Implementation Details

The JVM specification defines runtime data areas, but it does not require every implementation to physically organize memory in exactly the same way.

For example:

> "Object is always physically located in RAM heap memory exactly like this diagram"

is an oversimplification.

JVM implementations can perform optimizations.

Examples include:

- Escape analysis
- Scalar replacement
- Object allocation optimizations
- Stack allocation in certain optimized situations
- Register allocation

Therefore, diagrams such as:

```text
Stack → Reference → Heap Object
```

are extremely useful for learning the Java execution model, but they should not be interpreted as a guarantee of exact physical machine memory placement in every JVM implementation.

---

# 🔹 Escape Analysis

The JIT compiler can analyze whether an object escapes the scope where it is created.

Example:

```java
void calculate() {

    Student s = new Student();

    s.age = 20;

    System.out.println(s.age);
}
```

If the JVM determines that the object does not escape the method/thread, JIT optimizations may eliminate the object allocation or replace its fields with scalar values.

This is an implementation optimization.

Therefore:

> Do not say "Java objects can never be allocated outside the heap."

The Java specification and JVM implementation details are more nuanced.

---

# 🔹 Stack vs Heap — Conceptual Model

```text
                JVM
                 |
        +--------+--------+
        |                 |
      Stack             Heap
        |                 |
   Thread-specific     Shared
   execution           objects
        |                 |
   +----+----+       +----+----+
   |         |       |         |
Frames    Local     Object    Array
          data
```

---

# 🔹 Stack and Heap Comparison

| Feature | Stack | Heap |
|---|---|---|
| Scope | Thread-specific | Shared |
| Main purpose | Method execution | Object/array allocation |
| Unit | Stack frame | Object/array |
| Contains | Local variables, operand stack, frame data | Objects, arrays |
| Created during | Method invocation | Object/array allocation |
| Lifetime | Method invocation | Object reachability / GC |
| GC managed | No | Yes |
| Typical error | `StackOverflowError` | `OutOfMemoryError` |
| Shared between threads | No | Yes |
| Size | Usually more limited | Usually much larger |

---

# 🔹 Stack Frame vs Heap Object

| Stack Frame | Heap Object |
|---|---|
| Associated with method invocation | Created through object/array allocation |
| Thread-specific | Potentially shared |
| Temporary execution context | Can live beyond creating method |
| Contains local-variable/operand-stack data | Contains object state |
| Discarded when invocation returns | Reclaimed when no longer reachable and GC processes it |

---

# 🔹 Common Misconceptions

## ❌ 1. Every primitive is stored on the stack

Not universally correct.

A primitive local variable is associated with a stack frame, but a primitive field is part of an object.

```java
class Student {
    int age;
}
```

`age` belongs to the `Student` object.

---

## ❌ 2. Every object is physically guaranteed to be on the heap

The JVM's abstract memory model says objects are allocated in the heap, but implementations can optimize allocation and representation.

Do not confuse the conceptual JVM model with physical machine-level placement.

---

## ❌ 3. Arrays are not objects

Wrong.

In Java:

```java
int[] arr = new int[5];
```

creates an array object.

---

## ❌ 4. Primitive arrays use autoboxing

Wrong.

```java
int[] arr = {1, 2, 3};
```

stores primitive `int` values.

There is no automatic conversion to `Integer`.

---

## ❌ 5. Java passes objects by reference

Incorrect terminology.

Java is always pass-by-value.

For objects, the copied value is the reference.

---

## ❌ 6. When a method returns, its objects are destroyed

Wrong.

The stack frame disappears.

Objects can remain alive if they are still reachable.

---

## ❌ 7. `null` immediately destroys an object

Wrong.

Example:

```java
Student s = new Student();

s = null;
```

The object becomes eligible for GC only if there are no other reachable references to it.

---

## ❌ 8. Heap is automatically thread-safe because it is shared

Wrong.

The heap being shared means multiple threads can access the same objects.

It does not provide synchronization.

---

# 🔹 Interview Traps

### Trap 1

**Q: Where are local primitive variables stored?**

A safe answer:

> Local variables are represented in a method's stack frame, although exact physical placement can be optimized by the JVM/JIT.

---

### Trap 2

**Q: Where is an `int[]` stored?**

The array itself is an object and is allocated in the heap in the JVM's conceptual model.

Its elements are primitive `int` values; they are not boxed into `Integer` objects.

---

### Trap 3

**Q: Where is an object reference stored?**

Do not give an unconditional answer like:

> "Always stack."

A local reference variable is associated with the current method's frame, but fields that hold references are part of their containing objects.

---

### Trap 4

**Q: Is the heap thread-safe?**

No.

The heap is shared memory. Thread safety depends on how shared objects are accessed.

---

### Trap 5

**Q: What happens when a method returns?**

Its stack frame is discarded.

Objects referenced by that frame may continue to exist if they remain reachable from elsewhere.

---

# 🔹 Best Practices for Understanding Memory

### Remember these three layers:

```text
1. Java Language Model
        ↓
2. JVM Specification
        ↓
3. JVM Implementation / Hardware
```

Do not mix them.

---

### Use this mental model

```text
Method execution
      ↓
Stack frame

Object creation
      ↓
Heap object

Object no longer reachable
      ↓
Eligible for GC
```

---

# 🔹 Cheat Sheet

```text
STACK
│
├── Thread-private
├── Contains stack frames
├── Frame created per method invocation
├── Local variables
├── Operand stack
├── Frame metadata
└── StackOverflowError

HEAP
│
├── Shared between threads
├── Objects
├── Arrays
├── Garbage collected
├── Objects can outlive methods
└── OutOfMemoryError

IMPORTANT
│
├── Array = Object
├── int[] = array object containing primitive ints
├── Integer[] = array object containing references
├── No boxing in int[]
├── Java = pass-by-value
├── Object reference value is copied
└── Primitive ≠ automatically "stack"
```

---

# 🔥 Memory Diagram

```text
                         JVM
                          |
          +---------------+---------------+
          |                               |
       THREAD 1                        THREAD 2
          |                               |
       JVM Stack                       JVM Stack
          |                               |
     main() frame                    run() frame
          |                               |
     local variables                  local variables
          |                               |
          +---------------+---------------+
                          |
                         HEAP
                          |
              +-----------+-----------+
              |                       |
         Student object            int[] object
              |                       |
          age = 20              [10, 20, 30]
```

---

# 🔥 The Most Important Concept

Consider:

```java
int[] arr = new int[3];
```

Do **not** think:

```text
int → Integer
int → Integer
int → Integer
```

Think:

```text
arr
 |
 | reference
 ↓
int[] array object
┌───────────────┐
│  int │ int │ int │
│   10 │ 20  │ 30  │
└───────────────┘
```

The array is an object.

Its elements remain primitive values.

---

# 🔹 30-Second Interview Answer

### Q: What is the difference between Stack and Heap in Java?

> The JVM stack is thread-private and primarily manages stack frames created during method invocation. These frames contain local-variable and operand-stack information needed for execution. The heap is shared among threads and is the main runtime area for objects and arrays, which are managed by garbage collection. Stack memory is associated with method execution and commonly results in `StackOverflowError` when exhausted, while excessive heap allocation can result in `OutOfMemoryError`. However, the simple rule "primitives are on the stack and objects are on the heap" is an oversimplification because exact representation can be affected by JVM implementation and JIT optimizations.

---

# 🔥 Top 10 Interview Questions

## 1. What is stack memory in Java?

**Answer:**

Stack memory is a thread-private JVM runtime area that manages stack frames created during method invocation.

---

## 2. What is heap memory?

**Answer:**

Heap memory is a shared JVM runtime area used for object and array allocation and is managed by the garbage collector.

---

## 3. What is a stack frame?

**Answer:**

A stack frame is a runtime data structure created for a method invocation. It conceptually contains local variables, an operand stack, and a reference to the runtime constant pool.

---

## 4. Are objects stored in stack or heap?

**Answer:**

In the JVM's conceptual runtime model, objects and arrays are allocated in the heap. JVM implementations may apply optimizations such as escape analysis and scalar replacement.

---

## 5. Are primitives always stored in stack?

**Answer:**

No.

A primitive local variable is associated with a method's stack frame, but primitive fields are part of their containing objects.

---

## 6. Is an array an object in Java?

**Answer:**

Yes.

All Java arrays are objects.

For example:

```java
int[] arr = new int[5];
```

creates an array object.

---

## 7. Does `int[]` perform autoboxing?

**Answer:**

No.

`int[]` directly stores primitive `int` elements.

`Integer[]` stores references to `Integer` objects.

---

## 8. What happens to a stack frame when a method returns?

**Answer:**

The frame associated with that method invocation is discarded.

Objects referenced by the frame may remain alive if they are still reachable elsewhere.

---

## 9. What causes `StackOverflowError`?

**Answer:**

Excessive stack usage, commonly caused by deep or infinite recursion, can exhaust a thread's stack and cause `StackOverflowError`.

---

## 10. What is the difference between `StackOverflowError` and `OutOfMemoryError`?

**Answer:**

`StackOverflowError` commonly indicates that a thread's stack cannot accommodate additional frames or stack usage.

`OutOfMemoryError` indicates that the JVM cannot satisfy a memory allocation request or has otherwise exhausted an applicable memory resource.

---

# 🧠 Final Memory Trick

```text
STACK
"How is my method executing?"

HEAP
"Where are my objects and arrays allocated?"

FRAME
"One method invocation"

OBJECT
"Can survive beyond the method if still reachable"

ARRAY
"An object"

int[]
"Primitive values — NO boxing"

Integer[]
"References → Integer objects"

Java Parameter Passing
"Always pass-by-value"

Object Argument
"Reference value is copied"

GC
"Reclaims unreachable heap objects"

StackOverflowError
"Stack exhausted"

OutOfMemoryError
"Memory allocation/resource exhausted"
```

> ⭐ **Golden rule:**  
> **Don't memorize "primitive = stack, object = heap." Instead, understand what is being stored: method execution uses stack frames, while objects and arrays are allocated in the heap's conceptual JVM model.**