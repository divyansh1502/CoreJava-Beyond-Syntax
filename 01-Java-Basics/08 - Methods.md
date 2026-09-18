# ☕ 08 — Methods

> A method is a block of code that performs a specific task and can be invoked when required.

---

# 1. Basic Method Structure

```java
returnType methodName(parameters) {

    // method body

    return value;
}
```

Example:

```java
static int add(int a, int b) {

    return a + b;
}
```

Calling:

```java
int result = add(10, 20);
```

---

# 2. Why Use Methods?

Methods provide:

* Code reusability
* Better organization
* Separation of responsibilities
* Easier testing
* Easier maintenance
* Reduced code duplication

Instead of:

```java
System.out.println(10 + 20);
System.out.println(30 + 40);
System.out.println(50 + 60);
```

we can write:

```java
static int add(int a, int b) {
    return a + b;
}
```

and reuse it.

---

# 3. Components of a Method

```java
public static int add(int a, int b) {
    return a + b;
}
```

| Part           | Meaning                 |
| -------------- | ----------------------- |
| `public`       | Access modifier         |
| `static`       | Method belongs to class |
| `int`          | Return type             |
| `add`          | Method name             |
| `int a, int b` | Parameters              |
| `return`       | Returns result          |

---

# 4. Parameter vs Argument

This is commonly asked.

### Parameter

Variable declared in the method definition.

```java
static int add(int a, int b)
```

Here:

```text
a, b → parameters
```

### Argument

Actual value passed during method invocation.

```java
add(10, 20);
```

Here:

```text
10, 20 → arguments
```

### Memory Trick

```text
Parameter → definition
Argument  → call
```

---

# 5. Return Type

A method may return a value.

```java
static int square(int x) {

    return x * x;
}
```

Or it can return nothing using `void`.

```java
static void display() {

    System.out.println("Hello");
}
```

A `void` method does not return a value to the caller.

---

# 6. `return`

`return`:

1. Terminates the current method.
2. Can return a value when the method has a non-void return type.

Example:

```java
static int test() {

    return 10;

    // System.out.println("Hello"); // unreachable
}
```

---

# 7. Static Method

A static method belongs to the class rather than an individual object.

```java
class Calculator {

    static int add(int a, int b) {
        return a + b;
    }
}
```

Call:

```java
Calculator.add(10, 20);
```

An object is not required.

---

# 8. Instance Method

An instance method belongs to an object.

```java
class Student {

    void display() {
        System.out.println("Student");
    }
}
```

Call:

```java
Student s = new Student();

s.display();
```

---

# 9. Static vs Instance Method

| Static Method                    | Instance Method                               |
| -------------------------------- | --------------------------------------------- |
| Belongs to class                 | Belongs to object                             |
| Object not required              | Object required                               |
| Called using class name          | Usually called using reference                |
| Directly accesses static members | Can directly access instance + static members |

Example:

```java
class Test {

    static int x = 10;
    int y = 20;

    static void staticMethod() {
        System.out.println(x);
        // System.out.println(y); // invalid directly
    }

    void instanceMethod() {
        System.out.println(x);
        System.out.println(y);
    }
}
```

---

# 10. Can a Static Method Access Instance Variables?

Not directly.

```java
class Test {

    int x = 10;

    static void display() {

        // System.out.println(x); // error
    }
}
```

Why?

A static method can execute without any object.

But `x` belongs to an object.

There may be:

```text
Object 1 → x = 10
Object 2 → x = 50
Object 3 → x = 90
```

Which `x` should the static method use?

Therefore, it cannot access the instance variable directly.

---

# 11. How Can Static Method Access Instance Data?

Through an object reference.

```java
class Test {

    int x = 10;

    static void display() {

        Test obj = new Test();

        System.out.println(obj.x);
    }
}
```

Now the object explicitly identifies which instance variable should be accessed.

---

# 12. Method Overloading

Method overloading means defining multiple methods with the same name but different parameter lists.

```java
static int add(int a, int b) {
    return a + b;
}

static int add(int a, int b, int c) {
    return a + b + c;
}
```

Both are named:

```text
add()
```

but have different parameter lists.

---

# 13. What Can Change During Overloading?

You can overload by changing:

### Number of parameters

```java
add(int, int)
add(int, int, int)
```

### Type of parameters

```java
add(int, int)
add(double, double)
```

### Order of parameter types

```java
display(int, String)

display(String, int)
```

---

# 14. Return Type Alone Cannot Overload

This is invalid:

```java
int test() {
    return 10;
}

double test() {
    return 10.5;
}
```

Why?

Because the method signature does not differ.

Return type is **not sufficient** for overloading.

---

# 15. Method Signature

For Java methods, the method signature consists of:

```text
Method Name
+
Parameter Types
```

Example:

```java
add(int, int)
```

The following are **not** part of the method signature:

```text
Return type
Parameter names
Access modifier
static
throws clause
```

Therefore:

```java
int add(int a, int b)
```

and:

```java
double add(int x, int y)
```

have the same signature and cannot coexist as overloaded methods solely based on return type.

---

# 16. Compile-Time Polymorphism

Method overloading is called **compile-time polymorphism**.

```java
add(10, 20);
add(10, 20, 30);
```

The compiler determines which overloaded method should be invoked based on the arguments.

Therefore:

```text
Overloading
↓
Compile time
↓
Compile-time polymorphism
```

---

# 17. Varargs

Varargs allow a method to accept a variable number of arguments.

Syntax:

```java
static int sum(int... numbers) {

    int total = 0;

    for (int number : numbers) {
        total += number;
    }

    return total;
}
```

Calls:

```java
sum();
sum(10);
sum(10, 20);
sum(10, 20, 30, 40);
```

---

# 18. How Varargs Work Internally

Important interview point:

```java
int... numbers
```

is treated essentially as:

```java
int[] numbers
```

inside the method.

Conceptually:

```java
sum(10, 20, 30);
```

becomes an array-like parameter containing:

```text
[10, 20, 30]
```

---

# 19. Varargs Rules

A method can have only **one varargs parameter**.

Valid:

```java
void test(int a, String... values) {
}
```

Invalid:

```java
void test(String... a, int... b) {
}
```

The varargs parameter must be the **last parameter**.

Valid:

```java
void test(int x, String... values) {
}
```

Invalid:

```java
void test(String... values, int x) {
}
```

---

# 20. Varargs and Overloading Trap

Consider:

```java
void test(int... x) {
    System.out.println("varargs");
}

void test(int x) {
    System.out.println("normal");
}
```

Calling:

```java
test(10);
```

prints:

```text
normal
```

Why?

Java prefers the fixed-arity method over the variable-arity method when both are applicable.

---

# 21. Java Is Always Pass-by-Value

This is one of the **most important Java interview questions**.

Java is strictly:

> **Pass-by-value.**

There is no pass-by-reference parameter passing in Java.

---

# 22. Primitive Pass-by-Value

```java
static void change(int x) {
    x = 100;
}

public static void main(String[] args) {

    int a = 10;

    change(a);

    System.out.println(a);
}
```

Output:

```text
10
```

Why?

The method receives a copy of `a`.

```text
a = 10
 ↓
copy = 10
 ↓
copy = 100
```

Original `a` remains:

```text
10
```

---

# 23. Object References Are Also Passed by Value

This is where many interviews become tricky.

Consider:

```java
class Student {
    int age;
}

static void change(Student s) {

    s.age = 30;
}
```

Then:

```java
Student student = new Student();

student.age = 20;

change(student);

System.out.println(student.age);
```

Output:

```text
30
```

Did Java pass the object by reference?

**No.**

Java copied the reference value.

Conceptually:

```text
Original reference
       ↓
   Student Object
       ↑
Copied reference
```

Both references point to the same object.

Therefore:

```java
s.age = 30;
```

changes the shared object.

---

# 24. The Important Pass-by-Value Trap

Consider:

```java
static void change(Student s) {

    s = new Student();

    s.age = 50;
}
```

And:

```java
Student student = new Student();

student.age = 20;

change(student);

System.out.println(student.age);
```

Output:

```text
20
```

Why?

Inside the method:

```text
Original reference
      ↓
Original object

Copied reference
      ↓
New object
```

Changing the copied reference does not change the caller's reference.

---

# 25. Pass-by-Value Mental Model

For primitives:

```text
a = 10

change(a)
    ↓
copy = 10
```

For objects:

```text
student
   ↓
Object A

change(student)
   ↓
copy of reference
   ↓
Object A
```

Both references initially point to the same object.

But:

```java
s = new Student();
```

changes only the local copied reference.

---

# 26. Recursion

Recursion occurs when a method calls itself.

Example:

```java
static int factorial(int n) {

    if (n == 0) {
        return 1;
    }

    return n * factorial(n - 1);
}
```

Call:

```java
factorial(5);
```

Conceptually:

```text
factorial(5)
 ↓
5 × factorial(4)
 ↓
4 × factorial(3)
 ↓
3 × factorial(2)
 ↓
2 × factorial(1)
 ↓
1 × factorial(0)
 ↓
1
```

---

# 27. Base Case

Every useful recursive algorithm needs a condition that stops recursion.

```java
if (n == 0) {
    return 1;
}
```

This is the **base case**.

Without a terminating condition, recursion can continue until the call stack is exhausted.

This can result in:

```text
StackOverflowError
```

---

# 28. Recursive Call and Stack

Each method invocation gets its own stack frame.

For:

```java
factorial(3)
```

conceptually:

```text
Stack

factorial(1)
factorial(2)
factorial(3)
main()
```

When the base case returns, frames are removed in reverse order.

```text
factorial(1) returns
      ↓
factorial(2) returns
      ↓
factorial(3) returns
      ↓
main()
```

---

# 29. Recursion Complexity

For simple recursion:

```java
factorial(n)
```

Time complexity:

```text
O(n)
```

Space complexity due to call stack:

```text
O(n)
```

Important:

> Recursion does not automatically mean `O(n)` or `O(n²)`. Complexity depends on the recurrence and number of calls.

Example:

```java
fib(n)
```

with naive recursive Fibonacci has exponential time complexity.

---

# 30. Method Overloading with `null`

Consider:

```java
void test(String s) {
    System.out.println("String");
}

void test(Integer i) {
    System.out.println("Integer");
}
```

Calling:

```java
test(null);
```

causes:

```text
Compile-time error
```

Why?

`null` can match both `String` and `Integer`, and neither type is more specific than the other because they are unrelated reference types.

---

# 31. Overloading with Parent and Child

Consider:

```java
class Animal {}

class Dog extends Animal {}
```

Methods:

```java
void test(Animal a) {
    System.out.println("Animal");
}

void test(Dog d) {
    System.out.println("Dog");
}
```

Call:

```java
Dog d = new Dog();

test(d);
```

Output:

```text
Dog
```

The more specific overload is selected at compile time.

---

# 32. Overloading and Type Promotion

Consider:

```java
void test(int x) {
    System.out.println("int");
}

void test(long x) {
    System.out.println("long");
}
```

Call:

```java
test(10);
```

Output:

```text
int
```

Because `10` is an `int` literal.

If only:

```java
test(long)
```

exists, Java can widen the `int` to `long`.

---

# 33. Overloading Resolution — General Idea

When multiple overloaded methods exist, Java considers applicable methods and chooses according to conversion specificity.

A simplified preference is:

```text
Exact match
     ↓
Widening
     ↓
Boxing / unboxing
     ↓
Varargs
```

This is a simplified interview model; actual overload resolution follows detailed compile-time rules.

---

# 34. Method Arguments and Expressions

Arguments do not need to be literals.

```java
int a = 10;
int b = 20;

add(a, b);
```

They can be expressions:

```java
add(a + 5, b * 2);
```

The expressions are evaluated before the method receives the argument values.

---

# 35. Methods and Memory

When a method is invoked, a new stack frame is created for that invocation.

Conceptually:

```text
JVM Stack
│
├── main() frame
│
└── add() frame
     ├── parameters
     ├── local variables
     └── execution state
```

After the method returns, its stack frame is removed.

---

# 36. Common Interview Traps

## Trap 1

```java
void test(int x) {}
void test(int y) {}
```

❌ Not overloading.

Parameter names do not form part of the method signature.

---

## Trap 2

```java
int test() {
    return 10;
}

double test() {
    return 10.5;
}
```

❌ Invalid.

Return type alone cannot overload a method.

---

## Trap 3

```java
void test(int... x) {}
void test(int[] x) {}
```

❌ Cannot coexist.

For method parameter declaration purposes, varargs is represented as an array type.

---

## Trap 4

```java
static void test() {
    System.out.println(instanceVariable);
}
```

❌ Cannot directly access an instance variable from a static context.

---

## Trap 5

```java
static void change(int x) {
    x = 100;
}
```

Changing `x` does not change the caller's primitive variable.

---

## Trap 6

```java
static void change(Student s) {
    s = new Student();
}
```

Reassigning `s` does not change the caller's reference.

---

## Trap 7

```java
static void change(Student s) {
    s.age = 100;
}
```

This can change the caller-visible object's state because both references initially refer to the same object.

---

# 37. Interview Questions — Complete Set

## Fundamentals

### Q1. What is a method?

A reusable block of code that performs a specific task.

### Q2. What is the difference between parameter and argument?

Parameter → declared in method definition.

Argument → supplied during method invocation.

### Q3. What is a return type?

It specifies the type of value returned by a method.

### Q4. What is a void method?

A method that does not return a value.

### Q5. Can a method return multiple values?

Java methods have one declared return type, but multiple values can be represented through an object, array, collection, or another container.

---

## Static / Instance

### Q6. What is a static method?

A method associated with the class rather than an individual object.

### Q7. Can a static method directly access instance variables?

No.

### Q8. Can an instance method access static variables?

Yes.

### Q9. Can an instance method call a static method?

Yes.

### Q10. Can a static method call another static method directly?

Yes.

---

## Overloading

### Q11. What is method overloading?

Multiple methods with the same name but different parameter lists.

### Q12. Is method overloading compile-time or runtime polymorphism?

Compile-time polymorphism.

### Q13. Can methods be overloaded by changing only return type?

No.

### Q14. Can methods be overloaded by changing parameter names?

No.

### Q15. Can methods be overloaded by changing access modifier?

No.

### Q16. Can static methods be overloaded?

Yes.

```java
static void test(int x) {}
static void test(String x) {}
```

### Q17. Can constructors be overloaded?

Yes.

### Q18. Can `main()` be overloaded?

Yes, you can define additional overloaded `main` methods, but the JVM specifically looks for the recognized entry-point signature.

---

## Varargs

### Q19. What is varargs?

A feature that allows a method to accept a variable number of arguments.

### Q20. What does varargs become internally?

It is represented as an array parameter.

### Q21. Can a method have multiple varargs parameters?

No.

### Q22. Where must varargs appear?

As the last parameter.

### Q23. Can a varargs method be called with zero arguments?

Yes.

```java
test();
```

---

## Pass-by-Value

### Q24. Is Java pass-by-value or pass-by-reference?

Java is strictly pass-by-value.

### Q25. Why can a method modify an object's fields if Java is pass-by-value?

Because the value being copied for an object parameter is the reference value. Both the caller and method can therefore refer to the same object.

### Q26. Can a method change the caller's object reference?

No. Reassigning the copied reference does not reassign the caller's reference.

### Q27. Can a method modify the object referred to by a parameter?

Yes, if the object's state is mutable and accessible.

---

## Recursion

### Q28. What is recursion?

A method calling itself.

### Q29. What is a base case?

The condition that terminates recursive calls.

### Q30. What happens without a proper base case?

Recursive calls can continue until stack space is exhausted, potentially causing `StackOverflowError`.

### Q31. Where are recursive method calls stored?

In the JVM thread's call stack, with a stack frame for each active invocation.

### Q32. Does recursion always have better performance than iteration?

No. Recursion can introduce function-call overhead and additional stack usage.

---

## Advanced

### Q33. What is a method signature?

Method name + parameter types.

### Q34. Are parameter names part of a method signature?

No.

### Q35. Is return type part of a method signature?

No.

### Q36. What happens when overloaded methods receive `null`?

Java chooses the most specific applicable overload. If multiple unrelated reference types are equally applicable, the call can become ambiguous.

### Q37. How does Java choose between overloaded methods?

At compile time according to overload-resolution rules.

### Q38. What is the difference between overloading and overriding?

```text
Overloading
→ Same class/inheritance context
→ Different parameter list
→ Compile-time

Overriding
→ Inheritance
→ Same method signature
→ Runtime dispatch
```

Overriding will be covered deeply in the OOP section.

---

# 🧩 Output-Based Questions

## Question 1

```java
static void change(int x) {
    x = 100;
}

public static void main(String[] args) {

    int x = 10;

    change(x);

    System.out.println(x);
}
```

### Answer

```text
10
```

Because the primitive value is passed by value.

---

## Question 2

```java
static void change(Student s) {
    s.age = 100;
}
```

```java
Student s = new Student();

s.age = 20;

change(s);

System.out.println(s.age);
```

### Answer

```text
100
```

Both references point to the same object.

---

## Question 3

```java
static void change(Student s) {
    s = new Student();
    s.age = 100;
}
```

```java
Student s = new Student();

s.age = 20;

change(s);

System.out.println(s.age);
```

### Answer

```text
20
```

The method changed only its local copy of the reference.

---

## Question 4

```java
static void test(int... x) {
    System.out.println(x.length);
}

test();
```

### Answer

```text
0
```

---

## Question 5

```java
static void test(int x) {
    System.out.println("int");
}

static void test(long x) {
    System.out.println("long");
}

test(10);
```

### Answer

```text
int
```

---

## Question 6

```java
static void test(String x) {
    System.out.println("String");
}

static void test(Object x) {
    System.out.println("Object");
}

test("Java");
```

### Answer

```text
String
```

The `String` overload is more specific.

---

## Question 7

```java
static int factorial(int n) {

    if (n == 0)
        return 1;

    return n * factorial(n - 1);
}

System.out.println(factorial(4));
```

### Answer

```text
24
```

---

# 🔥 TOP 10 VVVV IMPORTANT INTERVIEW QUESTIONS

> These are the **10 highest-priority questions** for revision. The complete interview bank above is still worth knowing.

### 1. Is Java pass-by-value or pass-by-reference?

**Java is strictly pass-by-value.**

For objects, the copied value is the reference value.

---

### 2. Why can Java methods modify object fields if Java is pass-by-value?

Because both the original reference and its copied value point to the same object.

---

### 3. Can Java methods be overloaded by changing only return type?

**No.**

The parameter list must differ.

---

### 4. What is a method signature?

```text
Method name + parameter types
```

Return type and parameter names are not part of it.

---

### 5. What is method overloading?

Same method name with different parameter lists.

It is compile-time polymorphism.

---

### 6. Static method vs instance method?

```text
Static
→ class-level
→ object not required

Instance
→ object-level
→ object/reference normally required
```

---

### 7. Can a static method directly access instance variables?

**No**, because a static method can execute without an object.

---

### 8. What is varargs and how does it work?

```java
void test(int... values)
```

allows a variable number of arguments and is represented as an array parameter.

---

### 9. What is recursion and why can it cause StackOverflowError?

Recursion is a method calling itself. Every active invocation consumes a stack frame. Excessive recursion can exhaust stack space.

---

### 10. Overloading vs overriding?

```text
Overloading
→ different parameters
→ compile-time

Overriding
→ same signature in subclass
→ runtime dispatch
```

---

# 🎤 30-Second Interview Answer

> A method is a reusable block of code that performs a specific task. Java supports static and instance methods, method overloading, varargs, and recursion. One of the most important concepts is that Java is strictly pass-by-value. When an object is passed, the value of its reference is copied, so both references can point to the same object, but reassigning the parameter doesn't affect the caller's reference. Method overloading is resolved at compile time and requires different parameter lists; return type alone cannot overload a method.

---

# ⚡ Quick Revision

```text
METHOD
↓
Reusable block of code

PARAMETER
↓
Definition

ARGUMENT
↓
Method call

STATIC METHOD
↓
Class-level

INSTANCE METHOD
↓
Object-level

OVERLOADING
↓
Same name + different parameters
↓
Compile-time

METHOD SIGNATURE
↓
Name + parameter types

VARARGS
↓
type... name
↓
Array internally

JAVA
↓
Always pass-by-value

OBJECT PARAMETER
↓
Copy of reference value

RECURSION
↓
Method calls itself

BASE CASE
↓
Stops recursion

TOO MUCH RECURSION
↓
StackOverflowError
```

---

# 🧠 Memory Tricks

```text
Parameter → Placeholder
Argument  → Actual value

Overloading → Same name, different parameters

Return type → NOT part of signature

varargs → "many arguments"

Java → "Always pass the value"

Object parameter
→ copied reference
→ same object can be modified
→ caller reference itself cannot be replaced

Recursion
→ Call yourself
→ Need a stopping condition
```

---

# 🚀 Next Topic

```text
08 → Methods ✓

09 → Arrays
```

Arrays will introduce the foundation we need before moving into Strings and the Collection Framework.
