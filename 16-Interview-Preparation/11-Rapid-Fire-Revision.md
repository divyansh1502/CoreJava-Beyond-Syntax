# ⚡ Rapid-Fire Java Revision

> **Fast interview revision through short Java questions and direct answers.**

---

# 📑 Table of Contents

- [1. Core Java](#1-core-java)
- [2. OOP](#2-oop)
- [3. Strings](#3-strings)
- [4. Collections](#4-collections)
- [5. Generics](#5-generics)
- [6. Multithreading](#6-multithreading)
- [7. JVM](#7-jvm)
- [8. Java 8+](#8-java-8)
- [9. Exceptions](#9-exceptions)
- [10. Coding and DSA](#10-coding-and-dsa)
- [11. Tricky Questions](#11-tricky-questions)
- [12. Last-Minute Revision](#12-last-minute-revision)

---

# 1. Core Java

## Q1. What is Java?

**Answer:**  
Java is a high-level, class-based, object-oriented programming language designed to be platform-independent through the JVM.

---

## Q2. Why is Java platform-independent?

**Answer:**  
Java source code is compiled into bytecode, which can run on any platform having a compatible JVM.

```text
.java → javac → .class bytecode → JVM → Machine Code
```

---

## Q3. What is JVM?

**Answer:**  
JVM (Java Virtual Machine) executes Java bytecode and provides runtime services such as memory management, garbage collection, and JIT compilation.

---

## Q4. JDK vs JRE vs JVM?

**Answer:**

```text
JDK = JRE + Development Tools
JRE = JVM + Runtime Libraries
JVM = Executes Bytecode
```

---

## Q5. Is Java purely object-oriented?

**Answer:**  
No. Java supports primitive data types such as `int`, `char`, `boolean`, and `double`, so it is not considered purely object-oriented.

---

## Q6. What are Java primitive data types?

**Answer:**

```text
byte
short
int
long
float
double
char
boolean
```

---

## Q7. What is the default value of an instance `int` variable?

**Answer:**

```text
0
```

Local variables do not receive default values automatically.

---

## Q8. What is type casting?

**Answer:**  
Type casting converts a value from one data type to another.

```text
Widening  → automatic
Narrowing → explicit
```

---

## Q9. What is the difference between `==` and `equals()`?

**Answer:**

```text
==       → compares primitive values or object references
equals() → compares logical/content equality when overridden
```

---

## Q10. What is `final`?

**Answer:**

```text
final variable → cannot be reassigned
final method   → cannot be overridden
final class    → cannot be inherited
```

---

## Q11. What is `static`?

**Answer:**  
`static` members belong to the class rather than individual objects.

---

## Q12. Can a static method directly access an instance variable?

**Answer:**  
No. A static method has no implicit object reference, so it cannot directly access instance members.

---

## Q13. What is a constructor?

**Answer:**  
A constructor initializes an object and has the same name as the class with no return type.

---

## Q14. Can a constructor be `static`?

**Answer:**  
No. Constructors belong to object creation, while `static` members belong to the class.

---

## Q15. Can a constructor be inherited?

**Answer:**  
No. Constructors are not inherited.

---

# 2. OOP

## Q16. What are the four pillars of OOP?

**Answer:**

```text
Encapsulation
Inheritance
Polymorphism
Abstraction
```

---

## Q17. What is encapsulation?

**Answer:**  
Encapsulation bundles data and methods together and controls access to internal state.

A common implementation uses:

```text
private fields
public getters/setters
```

---

## Q18. What is inheritance?

**Answer:**  
Inheritance allows a class to acquire accessible properties and behavior from another class.

---

## Q19. What is polymorphism?

**Answer:**  
Polymorphism means one interface/reference can represent different implementations.

Two major forms:

```text
Compile-time → Method Overloading
Runtime      → Method Overriding
```

---

## Q20. What is abstraction?

**Answer:**  
Abstraction hides implementation details and exposes essential behavior.

It can be achieved using:

```text
abstract classes
interfaces
```

---

## Q21. Overloading vs overriding?

**Answer:**

```text
Overloading:
- Same method name
- Different parameter list
- Compile-time resolution

Overriding:
- Subclass provides implementation of inherited method
- Same signature
- Runtime dispatch
```

---

## Q22. Can a static method be overridden?

**Answer:**  
No. Static methods are hidden, not overridden.

---

## Q23. Can a final method be overridden?

**Answer:**  
No.

---

## Q24. Can a final class be inherited?

**Answer:**  
No.

---

## Q25. Can an abstract class have a constructor?

**Answer:**  
Yes. Its constructor runs as part of subclass object construction.

---

## Q26. Can an abstract class have concrete methods?

**Answer:**  
Yes.

---

## Q27. Can an interface have methods with implementation?

**Answer:**  
Yes. Modern Java interfaces can contain `default`, `static`, and private methods with implementations.

---

## Q28. Does Java support multiple inheritance through classes?

**Answer:**  
No. A class cannot extend multiple classes.

Java supports multiple inheritance of type through interfaces.

---

## Q29. What is `this`?

**Answer:**  
`this` refers to the current object.

---

## Q30. What is `super`?

**Answer:**  
`super` refers to the immediate parent-class portion of the current object and can be used to access parent members or invoke the parent constructor.

---

# 3. Strings

## Q31. Why is String immutable?

**Answer:**  
Once a `String` object is created, its character sequence cannot be changed. Operations that appear to modify it create or return another String.

---

## Q32. Does `toUpperCase()` modify the original String?

**Answer:**  
No.

```java
String s = "java";
String result = s.toUpperCase();
```

`s` remains `"java"` while `result` refers to `"JAVA"`.

---

## Q33. What is String Pool?

**Answer:**  
The String Pool is a JVM-managed area associated with interned String objects, allowing identical string literals to be shared.

---

## Q34. Difference between String, StringBuilder, and StringBuffer?

**Answer:**

```text
String        → immutable
StringBuilder → mutable, generally faster, not synchronized
StringBuffer  → mutable, synchronized
```

---

## Q35. What is the default capacity of StringBuilder?

**Answer:**

```text
16 characters
```

---

## Q36. What happens when StringBuilder capacity is exceeded?

**Answer:**  
It grows its internal storage automatically.

---

## Q37. `equals()` vs `equalsIgnoreCase()`?

**Answer:**

```text
equals()         → case-sensitive
equalsIgnoreCase → ignores letter case
```

---

## Q38. `replace()` vs `replaceAll()`?

**Answer:**

```text
replace()    → literal character/sequence replacement
replaceAll() → regex-based replacement
```

---

## Q39. Why is `StringBuilder` preferred for repeated concatenation?

**Answer:**  
Because it is mutable and avoids creating a new String object for every concatenation operation.

---

## Q40. What does `intern()` do?

**Answer:**  
It returns the canonical pooled representation of a String.

---

# 4. Collections

## Q41. What is Collection Framework?

**Answer:**  
It is a set of interfaces and classes for storing and manipulating groups of objects.

---

## Q42. List vs Set vs Map?

**Answer:**

```text
List → ordered, duplicates allowed
Set  → uniqueness enforced
Map  → key-value association
```

---

## Q43. ArrayList vs LinkedList?

**Answer:**

```text
ArrayList  → dynamic array
LinkedList → linked-node structure
```

ArrayList generally provides faster random access.

---

## Q44. HashSet vs LinkedHashSet vs TreeSet?

**Answer:**

```text
HashSet       → no guaranteed iteration order
LinkedHashSet → insertion order
TreeSet       → sorted order
```

---

## Q45. HashMap vs LinkedHashMap vs TreeMap?

**Answer:**

```text
HashMap       → no guaranteed iteration order
LinkedHashMap → insertion order
TreeMap       → sorted by keys
```

---

## Q46. Can HashMap contain a null key?

**Answer:**  
Yes. A `HashMap` permits one null key and multiple null values.

---

## Q47. Can HashSet contain null?

**Answer:**  
Yes, typically one null element.

---

## Q48. Does HashMap allow duplicate keys?

**Answer:**  
No. Inserting the same key again replaces its associated value.

---

## Q49. How does HashMap work internally?

**Answer:**  
It uses hashing to determine a bucket and then searches within that bucket using key equality. Modern Java can use balanced tree structures for heavily populated buckets.

---

## Q50. What is the purpose of `hashCode()`?

**Answer:**  
It provides a hash value used by hash-based collections to help locate objects efficiently.

---

## Q51. Contract between `equals()` and `hashCode()`?

**Answer:**  
If two objects are equal according to `equals()`, they must return the same `hashCode()`.

The reverse is not required.

---

## Q52. Comparable vs Comparator?

**Answer:**

```text
Comparable → natural ordering inside the class
Comparator → external/custom ordering
```

---

## Q53. What is fail-fast behavior?

**Answer:**  
Some collection iterators detect structural modification during iteration and may throw `ConcurrentModificationException`.

---

## Q54. Iterator vs ListIterator?

**Answer:**

```text
Iterator:
- Forward traversal
- Works with many collections

ListIterator:
- Forward + backward traversal
- Only for List implementations
- Supports add/set operations
```

---

# 5. Generics

## Q55. What are generics?

**Answer:**  
Generics provide compile-time type safety and allow classes and methods to operate on parameterized types.

---

## Q56. Why use generics?

**Answer:**

```text
Type safety
Less explicit casting
Reusable code
Compile-time error detection
```

---

## Q57. Can generics use primitive types?

**Answer:**  
No.

Use wrapper classes:

```text
int     → Integer
double  → Double
char    → Character
boolean → Boolean
```

---

## Q58. What is type erasure?

**Answer:**  
Java implements generics primarily through compile-time type checking and erasure of generic type information from most runtime representations.

---

## Q59. What does `<?>` mean?

**Answer:**  
It represents an unknown type.

---

## Q60. `<? extends T>` vs `<? super T>`?

**Answer:**

```text
<? extends T> → unknown subtype of T
<? super T>   → unknown supertype of T
```

Memory rule:

```text
PECS:
Producer Extends
Consumer Super
```

---

# 6. Multithreading

## Q61. What is a thread?

**Answer:**  
A thread is an independent path of execution within a process.

---

## Q62. Process vs thread?

**Answer:**

```text
Process → independent program execution unit
Thread  → execution unit within a process
```

---

## Q63. How can a thread be created in Java?

**Answer:**

```text
Extend Thread
Implement Runnable
Implement Callable
Use Executor framework
```

---

## Q64. Runnable vs Callable?

**Answer:**

```text
Runnable → does not return a result
Callable → returns a result and can throw checked exceptions
```

---

## Q65. `start()` vs `run()`?

**Answer:**

```text
start() → starts a new thread of execution
run()   → ordinary method invocation if called directly
```

---

## Q66. What is synchronization?

**Answer:**  
Synchronization controls concurrent access to shared mutable state so that operations requiring mutual exclusion can execute safely.

---

## Q67. What is `synchronized`?

**Answer:**  
It provides mutual exclusion and establishes visibility guarantees around synchronized regions.

---

## Q68. What is deadlock?

**Answer:**  
Deadlock occurs when threads wait indefinitely for resources held by each other.

---

## Q69. What is race condition?

**Answer:**  
A race condition occurs when program behavior depends on unpredictable timing between concurrent operations accessing shared state.

---

## Q70. What is volatile?

**Answer:**  
`volatile` ensures that reads and writes of the variable have the required visibility and ordering semantics between threads.

It does not make compound operations such as increment automatically atomic.

---

## Q71. What is ExecutorService?

**Answer:**  
`ExecutorService` manages a pool of worker threads and provides methods for submitting and managing asynchronous tasks.

---

## Q72. What is `sleep()`?

**Answer:**  
`Thread.sleep()` pauses the current thread for a specified duration.

It does not release intrinsic locks held by the thread.

---

## Q73. What is `wait()`?

**Answer:**  
`wait()` causes the current thread to wait and releases the monitor associated with the object until it is notified or otherwise awakened.

---

# 7. JVM

## Q74. What is JVM memory?

**Answer:**  
Important runtime memory areas include:

```text
Heap
JVM Stacks
Method Area / Metaspace
PC Register
Native Method Stack
```

---

## Q75. Heap vs Stack?

**Answer:**

```text
Heap:
- Objects
- Shared among threads
- Managed by garbage collection

Stack:
- Per-thread
- Method frames
- Local variables and execution state
```

---

## Q76. What is garbage collection?

**Answer:**  
Garbage collection automatically reclaims heap memory occupied by objects that are no longer reachable.

---

## Q77. Can we force garbage collection?

**Answer:**  
No. `System.gc()` only requests garbage collection; it does not guarantee that collection will occur.

---

## Q78. What is JIT?

**Answer:**  
JIT (Just-In-Time) compilation compiles frequently executed bytecode into optimized native machine code during runtime.

---

## Q79. What is class loading?

**Answer:**  
The JVM loads class definitions into memory through class loaders when they are needed.

---

## Q80. What are the main phases of class loading?

**Answer:**

```text
Loading
Linking
Initialization
```

Linking includes:

```text
Verification
Preparation
Resolution
```

---

## Q81. What is `OutOfMemoryError`?

**Answer:**  
It occurs when the JVM cannot satisfy an allocation request because sufficient required memory cannot be made available.

---

## Q82. What is `StackOverflowError`?

**Answer:**  
It commonly occurs when a thread's stack cannot accommodate additional stack frames, often because of excessively deep recursion.

---

# 8. Java 8+

## Q83. What is a lambda expression?

**Answer:**  
A lambda is a concise way to represent behavior, especially an implementation of a functional interface.

```java
Runnable task = () -> System.out.println("Hello");
```

---

## Q84. What is a functional interface?

**Answer:**  
An interface containing exactly one abstract method.

Examples:

```text
Runnable
Comparator
Predicate
Function
Consumer
Supplier
```

---

## Q85. What is Stream API?

**Answer:**  
Stream API provides a declarative way to process sequences of data through operations such as filtering, mapping, sorting, and reduction.

---

## Q86. `map()` vs `filter()`?

**Answer:**

```text
map()    → transforms elements
filter() → selects elements based on a condition
```

---

## Q87. What is `reduce()`?

**Answer:**  
`reduce()` combines stream elements into a single result.

---

## Q88. What is Optional?

**Answer:**  
`Optional<T>` is a container that may or may not contain a non-null value and can make absence explicit in APIs.

---

## Q89. What is a default method in an interface?

**Answer:**  
A default method is an interface method with an implementation that implementing classes can inherit.

---

## Q90. What is a record?

**Answer:**  
A record is a concise Java construct designed for transparent data-carrier classes.

---

## Q91. What is a sealed class?

**Answer:**  
A sealed class restricts which classes can directly extend it.

---

# 9. Exceptions

## Q92. What is an exception?

**Answer:**  
An exception is an event represented by an object that disrupts the normal flow of program execution.

---

## Q93. Checked vs unchecked exceptions?

**Answer:**

```text
Checked:
- Checked by compiler
- Must be handled or declared

Unchecked:
- RuntimeException and subclasses
- Compiler does not require handling
```

---

## Q94. Error vs Exception?

**Answer:**

```text
Error     → serious JVM/application environment problem
Exception → conditions applications commonly may handle
```

---

## Q95. `throw` vs `throws`?

**Answer:**

```text
throw  → explicitly throws an exception object
throws → declares exceptions a method may propagate
```

---

## Q96. Can we have multiple catch blocks?

**Answer:**  
Yes.

More specific exception types should generally be caught before their broader parent types.

---

## Q97. Can finally execute without catch?

**Answer:**  
Yes.

```java
try {
    // code
} finally {
    // cleanup
}
```

---

## Q98. Does finally always execute?

**Answer:**  
Normally it executes when control leaves the `try`/`catch`, but abnormal JVM termination such as `System.exit()` can prevent it.

---

# 10. Coding and DSA

## Q99. What is time complexity?

**Answer:**  
Time complexity describes how an algorithm's running work grows with input size.

---

## Q100. What is space complexity?

**Answer:**  
Space complexity describes how an algorithm's additional memory requirements grow with input size.

---

## Q101. Complexity of linear search?

**Answer:**

```text
O(n)
```

---

## Q102. Complexity of binary search?

**Answer:**

```text
O(log n)
```

for a sorted array with random access.

---

## Q103. Complexity of HashMap lookup?

**Answer:**  
Average-case lookup is generally:

```text
O(1)
```

Worst-case behavior depends on collisions and implementation details.

---

## Q104. What pattern is commonly used for sorted-array pair problems?

**Answer:**

```text
Two Pointers
```

---

## Q105. What pattern is commonly used for contiguous subarray/substring problems?

**Answer:**

```text
Sliding Window
```

when the problem's constraints support it.

---

## Q106. What algorithm finds maximum subarray sum?

**Answer:**

```text
Kadane's Algorithm
```

---

## Q107. How do you find a missing number from `1..n`?

**Answer:**

```text
Sum formula
or
XOR
```

---

## Q108. How do you detect duplicates efficiently?

**Answer:**

```text
HashSet
```

when extra space is allowed.

---

## Q109. How do you count frequencies?

**Answer:**

```text
HashMap
```

---

## Q110. How do you check balanced brackets?

**Answer:**

```text
Stack
```

---

## Q111. How do you find the first occurrence using binary search?

**Answer:**  
When the target is found, store the index and continue searching toward the left.

---

## Q112. How do you find the last occurrence?

**Answer:**  
When the target is found, store the index and continue searching toward the right.

---

# 11. Tricky Questions

## Q113. What is the output?

```java
String a = "Java";
String b = "Java";

System.out.println(a == b);
```

**Answer:**

```text
true
```

because identical string literals can refer to the same pooled String object.

---

## Q114. What is the output?

```java
String a = new String("Java");
String b = new String("Java");

System.out.println(a == b);
System.out.println(a.equals(b));
```

**Answer:**

```text
false
true
```

`==` compares references, while `equals()` compares String contents.

---

## Q115. Can we modify a String after creation?

**Answer:**  
No. String objects are immutable.

---

## Q116. Is `final` object immutable?

**Answer:**  
No.

`final` prevents reassignment of the reference; it does not automatically make the referenced object immutable.

---

## Q117. What happens here?

```java
final int x = 10;
x = 20;
```

**Answer:**  
Compile-time error because a final variable cannot be reassigned.

---

## Q118. Can an overloaded method differ only by return type?

**Answer:**  
No.

Return type alone is insufficient to distinguish overloaded methods.

---

## Q119. Can private methods be overridden?

**Answer:**  
No. Private methods are not inherited by subclasses.

---

## Q120. Can an interface have variables?

**Answer:**  
Yes. Interface fields are implicitly:

```text
public
static
final
```

---

## Q121. Can an abstract class be instantiated?

**Answer:**  
No.

---

## Q122. Can an interface be instantiated?

**Answer:**  
No. However, an interface reference can refer to an object of an implementing class.

---

## Q123. Can `main()` be overloaded?

**Answer:**  
Yes, other overloads can be declared, but the JVM uses the standard entry-point signature to launch the application.

---

## Q124. Can `main()` be overridden?

**Answer:**  
No in the normal overriding sense because it is static. Static methods are hidden.

---

## Q125. Can Java pass objects by reference?

**Answer:**  
Java is strictly pass-by-value.

When an object is passed, the value being copied is the reference.

---

## Q126. What happens when an object reference is passed to a method?

**Answer:**  
The reference value is copied. Both the caller and method parameter can refer to the same object, but assigning a new object to the parameter does not change the caller's reference.

---

## Q127. Can we catch `Exception` before `ArithmeticException`?

**Answer:**  
No, if both are sibling catch alternatives for the same try block.

The broader `Exception` catch would make the later `ArithmeticException` catch unreachable.

---

## Q128. Can we have `try` without `catch`?

**Answer:**  
Yes, if it has a `finally` block.

---

## Q129. Can we have `catch` without `try`?

**Answer:**  
No.

---

## Q130. Can we have `finally` without `try`?

**Answer:**  
No.

---

# 12. Last-Minute Revision

## Java Fundamentals

```text
Java
  ↓
Source Code
  ↓
Bytecode
  ↓
JVM
  ↓
Machine Code
```

---

## OOP

```text
Encapsulation
Inheritance
Polymorphism
Abstraction
```

---

## Polymorphism

```text
Overloading  → compile time
Overriding   → runtime
```

---

## Access Modifiers

```text
private
default
protected
public
```

From most restrictive to least restrictive:

```text
private → default → protected → public
```

---

## String

```text
String        → immutable
StringBuilder → mutable
StringBuffer  → mutable + synchronized
```

---

## Collections

```text
List → ordered + duplicates
Set  → unique elements
Map  → key-value
```

---

## Hash-Based Collections

```text
HashMap
HashSet
LinkedHashMap
LinkedHashSet
```

Remember:

```text
equals() + hashCode()
```

---

## Generics

```text
<?>             → unknown
<? extends T>   → producer
<? super T>     → consumer
```

Memory trick:

```text
PECS
Producer Extends
Consumer Super
```

---

## Threads

```text
start() ≠ run()
```

```text
sleep() → does not release monitor
wait()  → releases object's monitor
```

---

## JVM Memory

```text
Heap       → objects
Stack      → method frames, local execution state
Metaspace  → class metadata
```

---

## Exceptions

```text
try
catch
finally
throw
throws
```

---

## Java 8+

```text
Lambda
Functional Interface
Stream
Optional
Default Methods
```

---

## Common DSA Patterns

```text
Sorted array pair     → Two Pointers
Contiguous range      → Sliding Window
Frequency             → HashMap
Uniqueness            → HashSet
LIFO                  → Stack
Sorted search         → Binary Search
Maximum subarray      → Kadane
```

---

# 🚀 30-Second Interview Revision

If an interviewer asks you to explain your Java fundamentals quickly:

```text
Java is a class-based, object-oriented language that achieves
platform independence by compiling source code into bytecode
executed by the JVM.

Its major OOP principles are encapsulation, inheritance,
polymorphism, and abstraction.

Java provides collections such as List, Set, and Map for
different data-management requirements.

Strings are immutable, while StringBuilder and StringBuffer
provide mutable character sequences.

Generics provide compile-time type safety.

Java supports multithreading through Thread, Runnable,
Callable, and the Executor framework.

The JVM manages runtime execution, memory, garbage collection,
class loading, and JIT compilation.

Modern Java provides lambdas, functional interfaces, streams,
Optional, records, sealed classes, and other language features.
```

---

# 🧠 Final Interview Memory Map

```text
JAVA
│
├── OOP
│   ├── Encapsulation
│   ├── Inheritance
│   ├── Polymorphism
│   └── Abstraction
│
├── String
│   ├── Immutable
│   ├── String Pool
│   ├── StringBuilder
│   └── StringBuffer
│
├── Collections
│   ├── List
│   ├── Set
│   ├── Map
│   └── Queue
│
├── Generics
│   ├── Type Safety
│   ├── Wildcards
│   └── PECS
│
├── Multithreading
│   ├── Thread
│   ├── Runnable
│   ├── Callable
│   ├── Synchronization
│   └── Executor
│
├── JVM
│   ├── Heap
│   ├── Stack
│   ├── Metaspace
│   ├── GC
│   └── JIT
│
├── Java 8+
│   ├── Lambda
│   ├── Functional Interface
│   ├── Stream
│   ├── Optional
│   └── Modern Java
│
└── DSA
    ├── Two Pointers
    ├── Sliding Window
    ├── Hashing
    ├── Binary Search
    ├── Stack
    └── Kadane
```

