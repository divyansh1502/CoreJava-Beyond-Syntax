# ☕ 11 — Wrapper Classes, Autoboxing & Unboxing

> Wrapper classes provide object representations of Java's primitive data types.

Java has **8 primitive data types**, and each has a corresponding wrapper class.

---

# 1. Primitive → Wrapper

| Primitive | Wrapper     |
| --------- | ----------- |
| `byte`    | `Byte`      |
| `short`   | `Short`     |
| `int`     | `Integer`   |
| `long`    | `Long`      |
| `float`   | `Float`     |
| `double`  | `Double`    |
| `char`    | `Character` |
| `boolean` | `Boolean`   |

Remember:

```text
byte    → Byte
short   → Short
int     → Integer
long    → Long
float   → Float
double  → Double
char    → Character
boolean → Boolean
```

---

# 2. Why Do We Need Wrapper Classes?

Primitive types are not objects.

```java
int x = 10;
```

`x` is a primitive value.

But many Java APIs require objects.

For example:

```java
ArrayList<Integer> list = new ArrayList<>();
```

You cannot write:

```java
ArrayList<int> list = new ArrayList<>();
```

❌ Invalid.

Generics work with reference types, not primitive types.

Therefore:

```text
int
↓
Integer
```

---

# 3. Primitive vs Wrapper

```java
int a = 10;

Integer b = 10;
```

Conceptually:

```text
int
→ primitive value

Integer
→ object/reference type
```

Wrapper classes are classes, so they can be used wherever an object/reference type is required.

---

# 4. Creating Wrapper Objects

Old-style explicit construction:

```java
Integer x = new Integer(10);
```

Modern Java should generally **not** use this constructor.

Prefer:

```java
Integer x = Integer.valueOf(10);
```

or simply:

```java
Integer x = 10;
```

The second form uses autoboxing.

---

# 5. `valueOf()`

Wrapper classes provide `valueOf()` methods.

Example:

```java
Integer x = Integer.valueOf(100);
```

This returns an `Integer` object representing `100`.

You can also convert a String:

```java
Integer x = Integer.valueOf("100");
```

Now:

```java
System.out.println(x);
```

Output:

```text
100
```

---

# 6. `parseInt()`

`Integer.parseInt()` converts a String into a primitive `int`.

```java
int x = Integer.parseInt("100");
```

Important difference:

```text
Integer.valueOf("100")
→ Integer

Integer.parseInt("100")
→ int
```

Example:

```java
Integer a = Integer.valueOf("100");
int b = Integer.parseInt("100");
```

---

# 7. `valueOf()` vs `parseInt()`

| Method                   | Returns   |
| ------------------------ | --------- |
| `Integer.valueOf("10")`  | `Integer` |
| `Integer.parseInt("10")` | `int`     |

Interview trap:

```java
Integer x = Integer.valueOf("10");
```

vs

```java
int x = Integer.parseInt("10");
```

---

# 8. What Is Autoboxing?

**Autoboxing** is the automatic conversion of a primitive into its corresponding wrapper object.

Example:

```java
int x = 10;

Integer y = x;
```

Java automatically converts:

```text
int
↓
Integer
```

Conceptually similar to:

```java
Integer y = Integer.valueOf(x);
```

---

# 9. What Is Unboxing?

**Unboxing** is the automatic conversion of a wrapper object into its corresponding primitive.

Example:

```java
Integer x = 10;

int y = x;
```

Conceptually:

```java
int y = x.intValue();
```

So:

```text
Autoboxing
primitive → wrapper

Unboxing
wrapper → primitive
```

---

# 10. Autoboxing Example

```java
int a = 10;

Integer b = a;

System.out.println(b);
```

Output:

```text
10
```

Java automatically performs boxing.

---

# 11. Unboxing Example

```java
Integer a = 10;

int b = a;

System.out.println(b);
```

Java automatically unboxes `a`.

---

# 12. Both Together

```java
Integer x = 10;
```

Here:

```text
10
↓
autoboxing
↓
Integer
```

Then:

```java
int y = x;
```

Here:

```text
Integer
↓
unboxing
↓
int
```

---

# 13. Wrapper Classes Are Immutable

Wrapper objects such as:

```text
Integer
Double
Long
Boolean
Character
```

are immutable.

Example:

```java
Integer x = 10;

x = 20;
```

The existing Integer object isn't modified.

The variable is simply made to refer to another Integer value.

---

# 14. Why Are Wrapper Classes Immutable?

Important reasons include:

* Safe sharing
* Predictable behavior
* Thread-safety benefits
* Compatibility with caching
* Stable `equals()` and `hashCode()` behavior

---

# 15. Integer Caching

This is one of the **most important interview questions**.

Consider:

```java
Integer a = 100;
Integer b = 100;

System.out.println(a == b);
```

Typically:

```text
true
```

Why?

Autoboxing can use cached Integer objects for commonly cached values.

---

# 16. Integer Cache

Java specifies caching for certain wrapper values, and implementations may cache additional values.

For `Integer`, the required cache covers:

```text
-128 to 127
```

Therefore:

```java
Integer a = 100;
Integer b = 100;

System.out.println(a == b);
```

typically prints:

```text
true
```

because both references can refer to the same cached Integer object.

---

# 17. Integer Cache Trap

Now:

```java
Integer a = 200;
Integer b = 200;

System.out.println(a == b);
```

Do **not** use `==` for wrapper value comparison.

The result is generally:

```text
false
```

because values outside the guaranteed cache range are not guaranteed to be shared.

Correct:

```java
System.out.println(a.equals(b));
```

Output:

```text
true
```

---

# 18. Why `==` Is Dangerous With Wrappers

For wrapper objects:

```java
Integer a = 100;
Integer b = 100;
```

`==` checks:

```text
reference identity
```

while:

```java
a.equals(b)
```

checks:

```text
value/content equality
```

Therefore:

```text
Wrapper comparison
↓
Use equals()
```

when you mean value equality.

---

# 19. Important Output Question

```java
Integer a = 100;
Integer b = 100;

System.out.println(a == b);
```

Answer:

```text
true
```

because of the guaranteed/common cache behavior.

---

# 20. Another Output Question

```java
Integer a = 200;
Integer b = 200;

System.out.println(a == b);
```

Common result:

```text
false
```

But the key interview answer is:

> `==` is not a valid way to test wrapper value equality because caching may affect reference identity.

---

# 21. `new Integer()` vs `valueOf()`

```java
Integer a = new Integer(100);
Integer b = Integer.valueOf(100);
```

`new` explicitly creates a distinct object.

`valueOf()` can return a cached object.

Modern Java:

```text
Prefer valueOf()
Avoid new Integer(...)
```

---

# 22. Wrapper Methods

Wrapper classes provide useful conversion methods.

For `Integer`:

```java
Integer.parseInt()
Integer.valueOf()
Integer.toString()
Integer.compare()
Integer.max()
Integer.min()
Integer.sum()
```

Example:

```java
int x = Integer.parseInt("50");
```

---

# 23. Converting Primitive to String

```java
int x = 100;

String s = Integer.toString(x);
```

or:

```java
String s = String.valueOf(x);
```

Both can produce:

```text
"100"
```

---

# 24. Converting String to Primitive

```java
String s = "100";

int x = Integer.parseInt(s);
```

Now:

```text
s → String
x → int
```

---

# 25. Converting String to Wrapper

```java
String s = "100";

Integer x = Integer.valueOf(s);
```

Now:

```text
s → String
x → Integer
```

---

# 26. Wrapper Classes and Collections

This is one of the biggest practical reasons wrappers matter.

You can write:

```java
ArrayList<Integer> numbers =
        new ArrayList<>();
```

But:

```java
ArrayList<int> numbers =
        new ArrayList<>();
```

❌ Invalid.

Why?

Because generics require reference types.

Therefore:

```text
int
↓
Integer
↓
ArrayList<Integer>
```

---

# 27. Autoboxing in Collections

```java
ArrayList<Integer> list = new ArrayList<>();

list.add(10);
list.add(20);
list.add(30);
```

You are passing primitives:

```text
10
20
30
```

But the list stores:

```text
Integer
```

Java automatically boxes them.

Conceptually:

```java
list.add(Integer.valueOf(10));
```

---

# 28. Unboxing From Collections

```java
ArrayList<Integer> list = new ArrayList<>();

list.add(100);

int x = list.get(0);
```

`get(0)` returns:

```text
Integer
```

but Java automatically unboxes it into:

```text
int
```

Conceptually:

```java
int x = list.get(0).intValue();
```

---

# 29. Wrapper and `null`

This is a **very important interview trap**.

```java
Integer x = null;

int y = x;
```

This causes:

```text
NullPointerException
```

Why?

Unboxing requires obtaining the primitive value from the wrapper.

But:

```text
null
↓
no object
```

There is no Integer object to unbox.

---

# 30. `null` + Autounboxing Trap

```java
Integer x = null;

System.out.println(x + 10);
```

This also causes:

```text
NullPointerException
```

Why?

Java needs to unbox:

```text
Integer x
↓
int
```

before performing numeric addition.

But `x` is null.

---

# 31. Important Interview Example

```java
Integer a = 10;
Integer b = 20;

Integer c = a + b;
```

What happens?

The operands are unboxed:

```text
Integer
↓
int
```

Addition occurs:

```text
10 + 20
```

Then the result is boxed again:

```text
int
↓
Integer
```

So:

```text
Integer + Integer
↓
unboxing
↓
int + int
↓
int
↓
autoboxing
↓
Integer
```

---

# 32. Wrapper Comparison With Primitive

Consider:

```java
Integer a = 1000;

int b = 1000;

System.out.println(a == b);
```

This is:

```text
Integer == int
```

Java unboxes `a`.

Conceptually:

```java
a.intValue() == b
```

Therefore this compares numeric values.

Result:

```text
true
```

---

# 33. Wrapper Comparison With Wrapper

```java
Integer a = 1000;
Integer b = 1000;

System.out.println(a == b);
```

Both are references.

Therefore:

```text
reference comparison
```

Result is commonly:

```text
false
```

Use:

```java
a.equals(b)
```

for value comparison.

---

# 34. Boolean Wrapper

```java
Boolean a = true;
Boolean b = true;

System.out.println(a == b);
```

Wrapper caching can make this true.

Again:

```text
Do not rely on == for wrapper value equality.
```

Use:

```java
a.equals(b)
```

---

# 35. Character Wrapper

```java
Character c = 'A';
```

`Character` wraps a primitive:

```text
char
```

Useful methods include:

```java
Character.isLetter(c)
Character.isDigit(c)
Character.isUpperCase(c)
Character.isLowerCase(c)
Character.toUpperCase(c)
Character.toLowerCase(c)
```

Example:

```java
System.out.println(
    Character.isDigit('5')
);
```

Output:

```text
true
```

---

# 36. Number Wrapper Classes

These wrapper classes extend `Number`:

```text
Byte
Short
Integer
Long
Float
Double
```

Conceptually:

```text
Number
├── Byte
├── Short
├── Integer
├── Long
├── Float
└── Double
```

This is useful when learning the Java class hierarchy.

---

# 37. `Number`

`Number` is an abstract class that provides a common base for numeric wrapper classes.

It provides conversion methods such as:

```java
intValue()
longValue()
floatValue()
doubleValue()
shortValue()
byteValue()
```

Example:

```java
Number n = 100;

double x = n.doubleValue();
```

---

# 38. Wrapper Classes and OOP

Wrapper classes demonstrate how Java can represent primitive values as objects.

```text
primitive
↓
wrapper object
↓
can participate in object-based APIs
```

This is especially important for:

```text
Collections
Generics
Object parameters
Reflection
Utility methods
```

---

# 39. Important Interview Difference

### Primitive

```java
int x = 10;
```

Characteristics:

```text
not an object
stores a primitive value
cannot call methods
```

### Wrapper

```java
Integer x = 10;
```

Characteristics:

```text
reference type
represents value as an object
has methods
can be null
works with generics
```

---

# 40. Primitive Can Be `null`?

No.

```java
int x = null;
```

❌ Compile-time error.

But:

```java
Integer x = null;
```

✅ Valid.

This is one reason wrapper types are useful when "no value" must be represented.

---

# 41. Wrapper Classes Are `final`

Wrapper classes such as `Integer`, `Long`, `Double`, etc. are final.

Therefore they cannot normally be subclassed.

Example:

```java
class MyInteger extends Integer {
}
```

❌ Not allowed.

---

# 42. Why Wrapper Classes Matter in Backend Development

You will frequently see:

```java
Integer id;
Long userId;
Boolean active;
Double price;
```

instead of primitives.

Especially with:

```text
JPA / Hibernate
Spring Boot
Database entities
DTOs
Collections
Generics
```

A wrapper can represent:

```text
actual value
+
null/no value
```

whereas a primitive always has a value of its primitive type.

---

# 43. Important Wrapper Default Values

For fields:

```text
Primitive
int → 0
boolean → false
double → 0.0
```

Wrapper references:

```text
Integer → null
Boolean → null
Double → null
```

This distinction can matter in Java objects and database/entity models.

---

# 44. Wrapper Classes and `equals()`

```java
Integer a = 1000;
Integer b = 1000;

System.out.println(a.equals(b));
```

Output:

```text
true
```

Because `Integer.equals()` compares numeric values.

---

# 45. Wrapper Classes and `hashCode()`

Wrapper classes provide value-based `equals()` and corresponding `hashCode()` implementations.

Therefore:

```java
Integer a = 100;
Integer b = 100;

a.equals(b)
```

is true, and:

```java
a.hashCode() == b.hashCode()
```

is also true.

This matters in:

```text
HashMap
HashSet
```

---

# 46. Parsing Invalid Strings

```java
int x = Integer.parseInt("hello");
```

This causes:

```text
NumberFormatException
```

Similarly:

```java
Double.parseDouble("abc");
```

also throws:

```text
NumberFormatException
```

This connects directly with your Exception Handling topic.

---

# 47. Autoboxing Is Compile-Time Language Support

Autoboxing is not simply a magical runtime feature.

The Java compiler inserts the required conversion operations.

For example:

```java
Integer x = 10;
```

is conceptually similar to:

```java
Integer x = Integer.valueOf(10);
```

And:

```java
int y = x;
```

is conceptually similar to:

```java
int y = x.intValue();
```

---

# 48. Performance Consideration

Primitive:

```java
int
```

is generally more lightweight than:

```java
Integer
```

because `Integer` is an object/reference type.

Using wrappers can involve:

```text
object representation
memory overhead
boxing/unboxing
```

However, JVM optimizations and caching can reduce some costs.

For performance-sensitive numeric computation, primitives are generally preferred when object semantics are unnecessary.

---

# 49. Common Mistakes

### Mistake 1

```java
ArrayList<int> list;
```

❌ Invalid.

Use:

```java
ArrayList<Integer> list;
```

---

### Mistake 2

```java
Integer a = 1000;
Integer b = 1000;

if (a == b)
```

❌ Don't use `==` for wrapper value comparison.

Use:

```java
a.equals(b)
```

---

### Mistake 3

```java
Integer x = null;

int y = x;
```

❌ Causes `NullPointerException`.

---

### Mistake 4

```java
Integer.parseInt("10")
```

returns:

```text
int
```

not Integer.

---

### Mistake 5

```java
Integer.valueOf("10")
```

returns:

```text
Integer
```

---

# 50. Interview Questions — Complete Set

## Fundamentals

### Q1. What are wrapper classes?

Classes that represent primitive values as objects.

### Q2. Why do we need wrapper classes?

Because many Java APIs, especially generics and collections, require reference types rather than primitives.

### Q3. Is Integer a primitive?

No.

`Integer` is a wrapper class for `int`.

### Q4. Is Integer immutable?

Yes.

### Q5. Can wrapper classes be null?

Yes.

Example:

```java
Integer x = null;
```

---

# 51. Autoboxing Questions

### Q6. What is autoboxing?

Automatic conversion from primitive to wrapper.

```java
int x = 10;
Integer y = x;
```

### Q7. What is unboxing?

Automatic conversion from wrapper to primitive.

```java
Integer x = 10;
int y = x;
```

### Q8. Does Java internally use methods for boxing/unboxing?

Conceptually yes:

```text
boxing
→ valueOf()

unboxing
→ xxxValue()
```

---

# 52. Parsing Questions

### Q9. `parseInt()` vs `valueOf()`?

```text
parseInt()
→ int

valueOf()
→ Integer
```

### Q10. What happens if parsing fails?

```java
Integer.parseInt("abc");
```

throws:

```text
NumberFormatException
```

---

# 53. Caching Questions

### Q11. What is Integer caching?

Java provides a cache for a range of Integer values, with `-128` through `127` guaranteed.

### Q12. Why can this happen?

```java
Integer a = 100;
Integer b = 100;

a == b
```

Because both can refer to the same cached object.

### Q13. Can you rely on this?

Only within the specified caching guarantees. Never use `==` as a general wrapper-value comparison.

---

# 54. Collection Questions

### Q14. Why can't we use `int` in `ArrayList<int>`?

Generics require reference types.

### Q15. Why does this work?

```java
ArrayList<Integer> list = new ArrayList<>();

list.add(10);
```

Because Java automatically boxes `10` into an `Integer`.

### Q16. What happens here?

```java
int x = list.get(0);
```

The returned Integer is automatically unboxed.

---

# 55. Null Questions

### Q17. What happens here?

```java
Integer x = null;
int y = x;
```

`NullPointerException`.

### Q18. Why?

Because Java attempts to unbox a null reference.

### Q19. Can `int` contain null?

No.

### Q20. Can `Integer` contain null?

Yes.

---

# 56. Advanced Interview Questions

### Q21. Why are wrapper classes immutable?

For predictable value semantics, safe sharing/caching, and thread-safety benefits.

### Q22. Why are wrapper classes final?

To prevent subclassing and preserve their defined behavior.

### Q23. What is the parent class of Integer, Double, Long etc.?

```text
Number
```

### Q24. What is the difference between primitive and wrapper?

```text
Primitive
→ value
→ cannot be null
→ no methods

Wrapper
→ object/reference
→ can be null
→ methods available
→ usable with generics
```

### Q25. Why might a backend entity use `Integer` instead of `int`?

Because `Integer` can represent a missing/null value, which can be meaningful when mapping optional database fields.

---

# 57. Output-Based Interview Questions

## Q1

```java
Integer a = 100;
Integer b = 100;

System.out.println(a == b);
```

Common answer:

```text
true
```

Because of Integer caching.

---

## Q2

```java
Integer a = 200;
Integer b = 200;

System.out.println(a == b);
```

Common result:

```text
false
```

But the important lesson is:

> Never use `==` for general wrapper value comparison.

---

## Q3

```java
Integer a = 1000;
int b = 1000;

System.out.println(a == b);
```

Answer:

```text
true
```

`a` is unboxed before comparison.

---

## Q4

```java
Integer a = null;

System.out.println(a + 10);
```

Answer:

```text
NullPointerException
```

---

## Q5

```java
Integer x = Integer.valueOf("100");
System.out.println(x);
```

Output:

```text
100
```

---

## Q6

```java
int x = Integer.parseInt("100");
System.out.println(x);
```

Output:

```text
100
```

---

## Q7

```java
Integer a = 10;
Integer b = 20;

Integer c = a + b;
```

Conceptually:

```text
a → unbox
b → unbox
10 + 20
result → box
```

So:

```text
c = 30
```

---

## Q8

```java
Integer a = 10;

System.out.println(a.equals(10));
```

Output:

```text
true
```

The primitive argument can be boxed for the method call.

---

## Q9

```java
Integer a = null;

System.out.println(a == 0);
```

This requires unboxing `a` and therefore throws:

```text
NullPointerException
```

---

## Q10

```java
Integer a = 10;

int b = a;

System.out.println(b);
```

Output:

```text
10
```

Automatic unboxing occurs.

---

# 🔥 TOP 10 VVVV IMPORTANT INTERVIEW QUESTIONS

### 1. What is a Wrapper Class?

A wrapper class represents a primitive value as an object.

```text
int → Integer
double → Double
char → Character
```

---

### 2. Why are wrapper classes needed?

Generics and Collections work with reference types, so primitives such as `int` cannot directly be used as type arguments.

```java
ArrayList<Integer>
```

---

### 3. What is Autoboxing?

Automatic:

```text
primitive → wrapper
```

Example:

```java
Integer x = 10;
```

---

### 4. What is Unboxing?

Automatic:

```text
wrapper → primitive
```

Example:

```java
int x = Integer.valueOf(10);
```

---

### 5. `parseInt()` vs `valueOf()`?

```text
Integer.parseInt("10")
→ int

Integer.valueOf("10")
→ Integer
```

---

### 6. Why can `Integer a = 100; Integer b = 100; a == b` be true?

Because Java's Integer caching can reuse the same cached object for values in the guaranteed cache range.

---

### 7. Why should `==` not be used with wrapper classes?

Because `==` compares object references, and caching can make reference identity differ from value equality.

Use:

```java
a.equals(b)
```

for value comparison.

---

### 8. What happens when unboxing `null`?

```java
Integer x = null;
int y = x;
```

throws:

```text
NullPointerException
```

---

### 9. Primitive vs Wrapper?

```text
int
→ primitive
→ cannot be null
→ lightweight value

Integer
→ object/reference
→ can be null
→ methods
→ works with generics
```

---

### 10. What is Integer caching?

Java guarantees caching of Integer values from:

```text
-128 to 127
```

and implementations may cache additional values.

Therefore, never use `==` as a general-purpose wrapper value comparison.

---

# 🎤 30-Second Interview Answer

> Wrapper classes are object representations of Java's primitive data types. For example, `int` has `Integer`, `double` has `Double`, and `char` has `Character`. They are important because generics and collections require reference types. Autoboxing automatically converts primitives into wrappers, while unboxing converts wrappers back into primitives. One important interview trap is wrapper caching: `Integer` values in the guaranteed cache range can be reused, so `==` may produce unexpected results. For wrapper value comparison, `equals()` should be used. Also, unboxing a null wrapper causes a `NullPointerException`.

---

# ⚡ QUICK REVISION

```text
PRIMITIVE       WRAPPER

byte       →    Byte
short      →    Short
int        →    Integer
long       →    Long
float      →    Float
double     →    Double
char       →    Character
boolean    →    Boolean
```

```text
Autoboxing
primitive → wrapper

Unboxing
wrapper → primitive
```

```text
parseInt()
String → int

valueOf()
String → Integer
```

```text
Integer
↓
Number
```

```text
int
→ cannot be null

Integer
→ can be null
```

```text
Wrapper == Wrapper
→ reference comparison

Wrapper.equals()
→ value comparison
```

```text
Integer cache
→ -128 to 127 guaranteed
→ implementations may cache more
```

```text
Integer x = null;
int y = x;

→ NullPointerException
```

---

# 🧠 MEMORY TRICKS

```text
BOX
primitive → wrapper

UNBOX
wrapper → primitive

PARSE
String → primitive

VALUE OF
String/value → wrapper/value object
```

### The easiest way to remember:

```text
int
 ↓ BOX
Integer
 ↓ UNBOX
int
```

And:

```text
"100"
 ↓ parseInt()
100  (int)

"100"
 ↓ valueOf()
100  (Integer)
```

---

# 🚀 NEXT

```text
11 → Wrapper Classes ✓

12 → Packages & Access Modifiers
```

`12` will cover:

```text
package
import
public
private
protected
default
accessibility
same class
same package
subclass
different package
interview traps
```

This will also prepare the foundation for **Inheritance + OOP access control**.
