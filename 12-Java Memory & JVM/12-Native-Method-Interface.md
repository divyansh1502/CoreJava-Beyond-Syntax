# 11 — Native Method Interface (JNI)

## 📚 Table of Contents

1. [Introduction](#1-introduction)
2. [What is JNI?](#2-what-is-jni)
3. [Why JNI Exists](#3-why-jni-exists)
4. [Where JNI Fits in JVM](#4-where-jni-fits-in-jvm)
5. [Java Code vs Native Code](#5-java-code-vs-native-code)
6. [Native Methods](#6-native-methods)
7. [native Keyword](#7-native-keyword)
8. [How JNI Works](#8-how-jni-works)
9. [JNI Architecture](#9-jni-architecture)
10. [JNI Function Naming](#10-jni-function-naming)
11. [JNIEnv](#11-jnienv)
12. [JavaVM](#12-javavm)
13. [Native Method Libraries](#13-native-method-libraries)
14. [Loading Native Libraries](#14-loading-native-libraries)
15. [System.loadLibrary()](#15-systemloadlibrary)
16. [System.load()](#16-systemload)
17. [JNI Data Types](#17-jni-data-types)
18. [Primitive Types in JNI](#18-primitive-types-in-jni)
19. [Reference Types in JNI](#19-reference-types-in-jni)
20. [Calling Java from Native Code](#20-calling-java-from-native-code)
21. [Calling Native Code from Java](#21-calling-native-code-from-java)
22. [JNI Method Signature](#22-jni-method-signature)
23. [JNI and Object References](#23-jni-and-object-references)
24. [Local References](#24-local-references)
25. [Global References](#25-global-references)
26. [Weak Global References](#26-weak-global-references)
27. [JNI Exceptions](#27-jni-exceptions)
28. [JNI and Threads](#28-jni-and-threads)
29. [Native Memory and JVM Memory](#29-native-memory-and-jvm-memory)
30. [JNI Performance](#30-jni-performance)
31. [JNI Security Risks](#31-jni-security-risks)
32. [JNI Advantages](#32-jni-advantages)
33. [JNI Disadvantages](#33-jni-disadvantages)
34. [Common Use Cases](#34-common-use-cases)
35. [Common Misconceptions](#35-common-misconceptions)
36. [Interview Traps](#36-interview-traps)
37. [Top 15 Interview Questions](#37-top-15-interview-questions)
38. [30-Second Interview Answer](#38-30-second-interview-answer)
39. [Cheat Sheet](#39-cheat-sheet)

---

# 1. Introduction

Java is designed to provide platform-independent execution through the JVM.

However, sometimes Java applications need to interact with code written in languages such as:

```text
C
C++
Rust
Assembly
Operating-system APIs
Existing native libraries
```

Java provides the **Java Native Interface (JNI)** for this purpose.

### Definition

> JNI (Java Native Interface) is a standard programming interface that allows Java code running inside a JVM to interact with native code and native libraries.

The basic idea is:

```text
Java Code
    ↓
   JNI
    ↓
Native Code
    ↓
Operating System / Hardware / Native Library
```

---

# 2. What is JNI?

JNI stands for:

> **Java Native Interface**

It is an interface between:

```text
Java/JVM world
```

and:

```text
Native-code world
```

For example:

```text
Java
  |
  v
native method
  |
  v
JNI
  |
  v
C/C++
  |
  v
Operating System
```

JNI allows Java code to call native functions and allows native code to interact with JVM objects and methods.

---

# 3. Why JNI Exists

Java provides many APIs directly, but some operations may require native code.

### Common reasons

#### 1. Existing Native Libraries

A company may already have a C/C++ library.

Instead of rewriting everything in Java:

```text
Existing C Library
       ↓
      JNI
       ↓
     Java
```

#### 2. Operating-System APIs

Some OS-specific APIs may only be exposed through native interfaces.

#### 3. Hardware Access

Certain hardware libraries may provide native APIs.

#### 4. Performance-Critical Native Components

Some specialized operations may already have optimized native implementations.

#### 5. Legacy Systems

Old native applications can sometimes be integrated with Java using JNI.

---

# 4. Where JNI Fits in JVM

A simplified JVM architecture:

```text
                    JVM
                     |
       +-------------+-------------+
       |             |             |
       v             v             v
 Class Loader   Runtime Areas   Execution Engine
                                   |
                                   v
                              Native Interface
                                   |
                                   v
                              Native Libraries
                                   |
                                   v
                                OS / CPU
```

The JVM can interact with native code through native interfaces.

### Important JVM Components

```text
Class Loader
Runtime Data Areas
Execution Engine
JNI
Native Method Libraries
```

JNI is not itself the JVM.

It is an interface used to connect Java and native code.

---

# 5. Java Code vs Native Code

### Java Code

```java
public class Demo {

    public static void main(String[] args) {
        System.out.println("Hello");
    }
}
```

This executes through the JVM.

### Native Code

Native code is compiled for a particular native platform.

Examples:

```text
Windows → native DLL
Linux   → native shared library
macOS   → native dynamic library
```

Conceptually:

```text
Java Bytecode
     ↓
    JVM
     ↓
    JNI
     ↓
Native Binary
     ↓
Operating System
```

---

# 6. Native Methods

Java provides the `native` keyword.

Example:

```java
public class NativeDemo {

    public native void printMessage();

}
```

This declares a native method.

The method has:

```text
Declaration
```

but no Java implementation body.

### Important

This is valid:

```java
public native void printMessage();
```

But this is invalid:

```java
public native void printMessage() {
    System.out.println("Hello");
}
```

A native method is implemented outside Java source code.

---

# 7. `native` Keyword

The `native` keyword tells the JVM:

> This method is implemented in native code rather than Java bytecode.

Example:

```java
public class NativeMath {

    public native int add(int a, int b);

}
```

There is no method body.

Conceptually:

```text
Java declaration
      ↓
native method
      ↓
JNI implementation
      ↓
C/C++ function
```

### Important

`native` does not mean:

```text
"this method is automatically faster"
```

It only indicates that the method implementation is provided by native code.

---

# 8. How JNI Works

A simplified JNI workflow:

```text
1. Write Java native method
          ↓
2. Compile Java class
          ↓
3. Generate/define native interface
          ↓
4. Implement native function
          ↓
5. Compile native source
          ↓
6. Create native library
          ↓
7. Load library into JVM
          ↓
8. Call native method
```

Conceptually:

```text
Java
 |
 | native method
 v
JNI
 |
 v
Native Library
 |
 v
C/C++
 |
 v
OS
```

---

# 9. JNI Architecture

Consider:

```java
public class NativeMath {

    public native int add(int a, int b);

    static {
        System.loadLibrary("nativeMath");
    }

    public static void main(String[] args) {

        NativeMath obj = new NativeMath();

        int result = obj.add(10, 20);

        System.out.println(result);
    }
}
```

Architecture:

```text
NativeMath.java
      |
      v
javac
      |
      v
NativeMath.class
      |
      v
JVM
      |
      +------ native method call
      |
      v
libnativeMath
      |
      v
Native implementation
```

---

# 10. JNI Function Naming

Historically, JNI native implementations can use names based on the Java class and method.

For example:

```java
package com.example;

public class NativeMath {

    public native int add(int a, int b);

}
```

A native implementation may use a JNI naming convention such as:

```text
Java_com_example_NativeMath_add
```

Conceptually:

```text
Java_
  +
package
  +
class
  +
method
```

### Important

Modern JNI code can also use explicit registration through:

```text
RegisterNatives
```

This allows native functions to be mapped to Java methods without relying solely on the traditional exported-name convention.

---

# 11. `JNIEnv`

`JNIEnv` is one of the most important JNI concepts.

It provides native code with access to many JNI functions.

Conceptually:

```text
Native Code
     |
     v
 JNI Environment
     |
     +---- Find classes
     +---- Call methods
     +---- Access fields
     +---- Create objects
     +---- Throw exceptions
     +---- Manage references
```

In C, a JNI function commonly looks conceptually like:

```c
JNIEXPORT jint JNICALL
Java_NativeMath_add(
        JNIEnv *env,
        jobject obj,
        jint a,
        jint b) {

    return a + b;
}
```

The first parameter provides access to the JNI environment for the current thread.

---

# 12. `JavaVM`

`JavaVM` represents the JVM instance from the native side.

It is different from `JNIEnv`.

### `JNIEnv`

Used for JNI operations associated with a particular native thread/JNI environment.

### `JavaVM`

Represents the JVM and can be used for operations such as obtaining a `JNIEnv` for an attached native thread.

Conceptually:

```text
JavaVM
   |
   +---- JVM instance
          |
          +---- Thread 1 → JNIEnv
          |
          +---- Thread 2 → JNIEnv
          |
          +---- Thread 3 → JNIEnv
```

### Important

Do not treat `JNIEnv*` as a globally shareable object between arbitrary threads.

JNI environments are thread-specific.

---

# 13. Native Method Libraries

Native code is generally compiled into platform-specific native libraries.

Typical formats include:

```text
Windows → DLL
Linux   → .so
macOS   → .dylib
```

Conceptually:

```text
C/C++ Source
     ↓
Native Compiler
     ↓
Native Library
     ↓
JVM loads library
     ↓
JNI method available
```

### Important

Native libraries are platform-dependent.

This is one reason JNI code can reduce Java application's portability.

---

# 14. Loading Native Libraries

Java provides two common methods:

```java
System.loadLibrary(...)
```

and:

```java
System.load(...)
```

Example:

```java
static {
    System.loadLibrary("nativeMath");
}
```

The JVM then attempts to load the corresponding native library.

---

# 15. `System.loadLibrary()`

`System.loadLibrary()` loads a native library using its library name.

Example:

```java
System.loadLibrary("nativeMath");
```

The JVM searches for the appropriate platform-specific library.

Conceptually:

```text
nativeMath
    ↓
Windows → nativeMath.dll
Linux   → libnativeMath.so
macOS   → libnativeMath.dylib
```

The exact naming and search behavior depends on the platform and JVM.

### Common Pattern

```java
public class NativeDemo {

    static {
        System.loadLibrary("nativeMath");
    }

    public native int add(int a, int b);
}
```

---

# 16. `System.load()`

`System.load()` loads a native library using an explicit path.

Example:

```java
System.load("/absolute/path/to/libnativeMath.so");
```

### Difference

```text
System.loadLibrary()
→ Library name

System.load()
→ Explicit path
```

### Memory Trick

```text
loadLibrary()
→ "Find this library"

load()
→ "Load this exact path"
```

---

# 17. JNI Data Types

JNI provides types corresponding to Java types.

Examples:

```text
Java          JNI

boolean   →   jboolean
byte      →   jbyte
char      →   jchar
short     →   jshort
int       →   jint
long      →   jlong
float     →   jfloat
double    →   jdouble
```

For Java references:

```text
Java Object       → jobject
Java String       → jstring
Java Class        → jclass
Java array        → jobjectArray / primitive-array types
```

---

# 18. Primitive Types in JNI

JNI defines native-side types representing Java primitive types.

Example:

```c
jint
```

corresponds to Java:

```java
int
```

Example native function:

```c
JNIEXPORT jint JNICALL
Java_NativeMath_add(
        JNIEnv *env,
        jobject obj,
        jint a,
        jint b) {

    return a + b;
}
```

Java:

```java
public native int add(int a, int b);
```

Conceptually:

```text
Java int
   ↓
 JNI jint
   ↓
Native implementation
```

---

# 19. Reference Types in JNI

JNI also provides reference types for Java objects.

Examples:

```text
jobject
jclass
jstring
jarray
jobjectArray
```

### Example

A Java:

```java
String
```

can be represented on the JNI side by:

```c
jstring
```

A Java object:

```java
Employee
```

can generally be represented through:

```c
jobject
```

### Important

JNI references are not ordinary C pointers to Java heap objects.

They are handles/references managed according to JNI rules by the JVM.

---

# 20. Calling Java from Native Code

JNI is bidirectional.

Native code can call Java methods.

Conceptually:

```text
Java
  ↓
Native method
  ↓
C/C++
  ↓
JNI
  ↓
Java method
```

For example, native code can:

```text
Find a class
Find a method
Call the method
Pass arguments
Receive the result
```

Conceptually:

```c
jclass clazz =
        (*env)->FindClass(env, "com/example/Demo");

jmethodID method =
        (*env)->GetMethodID(
            env,
            clazz,
            "print",
            "()V"
        );

(*env)->CallVoidMethod(
    env,
    object,
    method
);
```

The exact JNI syntax depends on C/C++ usage.

---

# 21. Calling Native Code from Java

The most common direction is:

```text
Java
 ↓
native method
 ↓
JNI
 ↓
C/C++
```

Example:

```java
public class NativeMath {

    public native int multiply(int a, int b);

    static {
        System.loadLibrary("nativeMath");
    }

    public static void main(String[] args) {

        NativeMath math = new NativeMath();

        int result = math.multiply(5, 6);

        System.out.println(result);
    }
}
```

Native implementation:

```c
JNIEXPORT jint JNICALL
Java_NativeMath_multiply(
        JNIEnv *env,
        jobject obj,
        jint a,
        jint b) {

    return a * b;
}
```

---

# 22. JNI Method Signature

JNI uses method signatures to identify:

```text
Parameter types
Return type
```

Example:

```java
int add(int a, int b)
```

has a descriptor conceptually represented as:

```text
(II)I
```

Meaning:

```text
(II)
 ↓
two int parameters

I
 ↓
int return type
```

### Common JNI Type Descriptors

```text
boolean → Z
byte    → B
char    → C
short   → S
int     → I
long    → J
float   → F
double  → D
void    → V
```

Object:

```text
Ljava/lang/String;
```

Array:

```text
[I
```

means:

```text
int[]
```

---

# 23. JNI and Object References

When native code receives a Java object:

```c
jobject obj
```

it receives a JNI reference to that Java object.

The native code must follow JNI's reference-management rules.

### Important

Do not assume:

```c
jobject
```

is simply a raw memory address of a Java object.

The JVM controls the actual object representation and garbage collection.

JNI provides APIs that allow native code to interact with Java objects safely according to JVM rules.

---

# 24. Local References

A JNI function can create local references.

Example:

```text
FindClass()
NewObject()
GetObjectClass()
```

These can create JNI references associated with the current native call/thread context.

Local references are normally automatically released when the native method returns.

### Conceptual Flow

```text
Native Method Starts
       ↓
Local References Created
       ↓
JNI Operations
       ↓
Native Method Returns
       ↓
Local References Automatically Released
```

### Important

A local reference should not be assumed to remain valid indefinitely.

---

# 25. Global References

Sometimes native code needs a Java object reference to survive beyond the current native method call.

JNI provides global references.

Conceptually:

```text
Local Reference
      ↓
NewGlobalRef()
      ↓
Global Reference
```

Example:

```c
jobject globalRef =
        (*env)->NewGlobalRef(env, obj);
```

The global reference must eventually be released:

```c
(*env)->DeleteGlobalRef(env, globalRef);
```

### Important

Holding unnecessary global references can prevent Java objects from becoming garbage collectible.

Therefore:

```text
Create global reference
        ↓
Use it
        ↓
Delete it when no longer needed
```

---

# 26. Weak Global References

JNI also provides weak global references.

They are useful when native code wants to hold a reference without strongly preventing the Java object from being garbage collected.

Conceptually:

```text
Weak Global Reference
        |
        v
Java Object
```

If the object becomes otherwise unreachable, the JVM can collect it.

Native code can check whether the weak reference still refers to a live object.

### Important

Weak global references follow special JNI rules and should not be treated as ordinary strong global references.

---

# 27. JNI Exceptions

Java exceptions can interact with native code.

Suppose a JNI operation causes a Java exception.

Native code can check:

```c
(*env)->ExceptionCheck(env)
```

Conceptually:

```text
JNI operation
     ↓
Exception occurs
     ↓
Pending Java exception
     ↓
Native code checks/handles appropriately
```

Native code can also throw a Java exception using JNI APIs.

For example:

```c
(*env)->ThrowNew(
    env,
    exceptionClass,
    "Something went wrong"
);
```

### Important

JNI code must handle pending exceptions carefully.

Continuing with arbitrary JNI operations while a pending exception exists can produce incorrect behavior or additional failures depending on the operation.

---

# 28. JNI and Threads

JNI can be used by native threads, but native threads that want to interact with the JVM generally need to be attached to the JVM.

Conceptually:

```text
Native Thread
     ↓
Attach to JVM
     ↓
Obtain JNIEnv
     ↓
Perform JNI operations
     ↓
Detach when finished
```

### Important

A native thread does not automatically have a valid `JNIEnv*`.

For a native-created thread, the appropriate JVM attachment mechanism must be used before making JNI calls.

---

# 29. Native Memory and JVM Memory

JNI introduces native memory into the application.

Conceptually:

```text
Java Application
       |
       +---- Java Heap
       |
       +---- JVM Runtime Areas
       |
       +---- Native Memory
                |
                +---- Native Libraries
                +---- Native Allocations
                +---- JNI-related structures
```

### Important

Java Garbage Collection does not automatically manage arbitrary memory allocated directly by native code.

For example:

```c
void* memory = malloc(1024);
```

That memory is native memory.

The native code is responsible for releasing it appropriately:

```c
free(memory);
```

---

# 30. JNI Performance

JNI calls can introduce overhead because execution crosses a boundary:

```text
Java
  ↓
JNI boundary
  ↓
Native code
```

The cost depends on the operation, JVM, platform, and amount of data being transferred.

### Potential Sources of Overhead

```text
Java ↔ Native transitions
Data conversion
Object access
String conversion
Array copying or pinning
Thread attachment
JNI reference management
```

### Important

JNI is not automatically faster than Java.

A native implementation can sometimes be faster for specific workloads, but the JNI boundary itself has costs.

---

# 31. JNI Security Risks

Native code operates outside many of Java's normal safety guarantees.

Potential problems include:

```text
Buffer overflows
Use-after-free
Memory corruption
Invalid pointers
Native crashes
Data races
Resource leaks
```

A native bug can potentially crash the entire JVM process.

For example:

```text
Java exception
→ Usually affects Java execution

Native segmentation fault
→ Can terminate the JVM process
```

This is one of the major reasons JNI should be used carefully.

---

# 32. JNI Advantages

## 32.1 Access Existing Native Libraries

```text
C/C++ library
      ↓
     JNI
      ↓
    Java
```

## 32.2 Access Platform APIs

Useful when required OS functionality is not conveniently exposed through Java APIs.

## 32.3 Hardware Integration

Can interact with native hardware SDKs.

## 32.4 Legacy Integration

Allows Java applications to communicate with existing native systems.

## 32.5 Specialized Native Algorithms

Can integrate highly optimized native implementations when appropriate.

---

# 33. JNI Disadvantages

## 33.1 Platform Dependency

Native binaries are platform-specific.

```text
Windows
   ≠
Linux
   ≠
macOS
```

## 33.2 Increased Complexity

You now have:

```text
Java
+
Native code
+
Build system
+
Native libraries
```

## 33.3 Memory Safety Risks

Native code can introduce:

```text
Segmentation faults
Memory corruption
Leaks
```

## 33.4 Debugging Difficulty

Debugging Java + native interactions can be more difficult than debugging pure Java.

## 33.5 Performance Boundary

Crossing between Java and native code has overhead.

---

# 34. Common Use Cases

JNI can be useful for:

```text
Operating-system integration
Hardware SDKs
Existing C/C++ libraries
Legacy native systems
High-performance native libraries
Graphics libraries
Cryptography implementations
Database/native drivers
Embedded systems
```

### Important

A native implementation should be used because there is a concrete requirement, not simply because:

> "Native code must be faster."

---

# 35. Common Misconceptions

## Misconception 1

> JNI means Java becomes platform dependent.

### Correct

Java itself remains platform-independent, but native components introduced through JNI are platform-specific.

---

## Misconception 2

> Every native method is faster than a Java method.

### Correct

Not necessarily.

JNI calls introduce overhead, and modern JVM JIT optimizations can make Java code extremely efficient.

---

## Misconception 3

> `native` means the method is implemented by the operating system.

### Correct

`native` means its implementation is provided outside Java bytecode, typically through native code.

---

## Misconception 4

> `JNIEnv*` can be shared freely between threads.

### Correct

JNI environment pointers are associated with threads and should not be treated as globally shareable.

---

## Misconception 5

> Java GC automatically frees all native memory.

### Correct

Java GC manages Java objects. Native memory allocated directly by native code must be managed appropriately by the native side.

---

## Misconception 6

> JNI only allows Java to call C.

### Correct

JNI is commonly used with C and C++, and native implementations can use other native languages through suitable interoperability mechanisms.

---

## Misconception 7

> A `jobject` is a normal C pointer to the Java object.

### Correct

JNI references are managed by the JVM and should be manipulated through JNI APIs rather than treated as raw object addresses.

---

## Misconception 8

> `System.loadLibrary()` accepts an absolute path.

### Correct

`System.loadLibrary()` takes a library name.

`System.load()` is used with a path.

---

# 36. Interview Traps

## Trap 1

### Question

What does `native` mean?

### Answer

It declares that the method implementation is provided by native code rather than Java bytecode.

---

## Trap 2

### Question

What does JNI stand for?

### Answer

Java Native Interface.

---

## Trap 3

### Question

Why use JNI?

### Answer

To allow Java applications to interact with native code, native libraries, OS APIs, hardware interfaces, or legacy systems.

---

## Trap 4

### Question

What is `JNIEnv`?

### Answer

It provides native code with access to JNI functions associated with the current native thread/JNI environment.

---

## Trap 5

### Question

What is `JavaVM`?

### Answer

It represents the JVM instance from the native side and can be used for JVM-level operations such as attaching native threads.

---

## Trap 6

### Question

Difference between `System.load()` and `System.loadLibrary()`?

### Answer

```text
System.load()
→ explicit library path

System.loadLibrary()
→ library name
```

---

## Trap 7

### Question

What happens to native memory during Java GC?

### Answer

Java GC does not automatically manage arbitrary memory allocated directly by native code.

---

## Trap 8

### Question

Can native code call Java methods?

### Answer

Yes.

JNI provides APIs for native code to find classes, methods, fields, create objects, and invoke Java methods.

---

## Trap 9

### Question

Can a native-created thread call JNI?

### Answer

Yes, but it generally must first attach itself to the JVM and obtain a valid `JNIEnv*`.

---

## Trap 10

### Question

What happens if native code crashes?

### Answer

A serious native crash can terminate the JVM process because native code executes inside the same process as the JVM.

---

# 37. Top 15 Interview Questions

## Q1. What is JNI?

### Answer

JNI is the Java Native Interface, a standard interface that allows Java code running inside the JVM to interact with native code and native libraries.

---

## Q2. Why do we need JNI?

### Answer

JNI is useful for integrating existing native libraries, accessing platform-specific APIs or hardware, working with legacy native code, and using specialized native implementations.

---

## Q3. What is a native method?

### Answer

A native method is a Java method declared using the `native` keyword whose implementation is provided outside Java bytecode.

---

## Q4. What is the `native` keyword?

### Answer

It tells the JVM that the implementation of the method is provided by native code rather than a Java method body.

---

## Q5. What is `JNIEnv`?

### Answer

`JNIEnv` provides access to JNI functions for native code running in a particular thread context.

---

## Q6. What is `JavaVM`?

### Answer

`JavaVM` represents the JVM from native code and can be used for operations such as attaching native threads and obtaining their JNI environments.

---

## Q7. Difference between `JNIEnv` and `JavaVM`?

### Answer

```text
JNIEnv
→ Thread-specific JNI interface

JavaVM
→ Represents the JVM instance
```

---

## Q8. Difference between `System.load()` and `System.loadLibrary()`?

### Answer

```text
System.load(path)
→ Loads using an explicit path

System.loadLibrary(name)
→ Loads using a library name
```

---

## Q9. What is a JNI reference?

### Answer

It is a JVM-managed reference used by native code to interact with Java objects. It should not be treated as a normal raw C pointer.

---

## Q10. What is a global reference?

### Answer

A global JNI reference is a reference that can remain valid beyond the current native method invocation and must eventually be explicitly deleted.

---

## Q11. What is a local reference?

### Answer

A local reference is generally valid during the relevant native call and is normally released automatically when the native method returns.

---

## Q12. Can JNI code throw Java exceptions?

### Answer

Yes. JNI provides APIs for detecting pending exceptions and throwing Java exceptions from native code.

---

## Q13. Can Java GC manage native memory?

### Answer

Not arbitrary native memory allocated directly by native code. That memory must be managed by the native side.

---

## Q14. Why can JNI reduce portability?

### Answer

Native libraries are compiled for particular operating systems, CPU architectures, and ABIs, so different platforms may require different native binaries.

---

## Q15. Is JNI always faster than Java?

### Answer

No. JNI introduces boundary and data-conversion overhead. Native code may be beneficial for particular workloads, but native does not automatically mean faster.

---

# 38. 30-Second Interview Answer

> JNI stands for Java Native Interface and allows Java code running inside the JVM to communicate with native code such as C or C++ libraries. A Java method can be declared using the `native` keyword, and the native implementation is loaded through mechanisms such as `System.loadLibrary()`. JNI provides interfaces such as `JNIEnv` for native operations and `JavaVM` for JVM-level interaction. JNI is useful for legacy libraries, OS APIs, hardware, and specialized native functionality, but it introduces platform dependency, complexity, memory-safety risks, and Java-to-native call overhead.

---

# 39. Cheat Sheet

```text
╔══════════════════════════════════════════════════════════╗
║                         JNI                              ║
╠══════════════════════════════════════════════════════════╣
║ JNI              → Java Native Interface                ║
║ native           → Method implemented outside Java      ║
║ JNIEnv           → Thread-specific JNI interface        ║
║ JavaVM           → JVM instance from native side        ║
║ System.load()    → Explicit library path                ║
║ System.loadLibrary() → Library name                     ║
║ jobject          → General Java object reference        ║
║ jstring          → Java String reference                ║
║ jint             → Java int                             ║
║ jlong            → Java long                            ║
║ jclass           → Java Class reference                 ║
╚══════════════════════════════════════════════════════════╝
```

## 🧠 Easy Memory Trick

```text
JNI
↓
Java ↔ Native

native
↓
"Implementation is outside Java"

JNIEnv
↓
"Work with JNI from this thread"

JavaVM
↓
"Work with the JVM"

loadLibrary()
↓
"Library NAME"

load()
↓
"Library PATH"
```

## 🔥 Basic JNI Flow

```text
Java Source
    ↓
native method
    ↓
javac
    ↓
.class
    ↓
JVM
    ↓
Load Native Library
    ↓
JNI
    ↓
Native Function
    ↓
C/C++
    ↓
OS / Hardware
```

## 🔥 Native Method Example

```java
public class NativeMath {

    public native int add(int a, int b);

    static {
        System.loadLibrary("nativeMath");
    }
}
```

Conceptually:

```text
NativeMath.add()
       ↓
      JNI
       ↓
nativeMath library
       ↓
C/C++ implementation
```

## 🔥 JNI References

```text
Local Reference
    ↓
Usually valid for current native call

Global Reference
    ↓
Can survive beyond current native call
    ↓
Must be explicitly deleted

Weak Global Reference
    ↓
Does not strongly keep object alive
```

## 🔥 JNI Data Types

```text
Java       JNI

boolean →  jboolean
byte    →  jbyte
char    →  jchar
short   →  jshort
int     →  jint
long    →  jlong
float   →  jfloat
double  →  jdouble

Object  →  jobject
String  →  jstring
Class   →  jclass
```

## 🔥 Java ↔ Native

```text
Java → Native
    ↓
native method
    ↓
JNI
    ↓
C/C++

Native → Java
    ↓
JNIEnv
    ↓
FindClass / GetMethodID
    ↓
Call Java Method
```

## ⭐ Final Interview Rule

```text
JNI
→ Bridge between Java and native code

native
→ Native implementation

JNIEnv
→ Access JNI functions for current thread

JavaVM
→ Represents JVM

loadLibrary()
→ Load by library name

load()
→ Load by path

Local Reference
→ Current native-call lifetime

Global Reference
→ Explicitly managed longer-lived reference

Native Memory
→ Not automatically freed by Java GC

JNI Crash
→ Can crash entire JVM process
```

## 🚀 One-Line Revision

```text
Java → native method → JNI → Native Library → C/C++ → OS/Hardware
```