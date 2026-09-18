# ☕ 06 — Operators

> Operators are symbols that perform operations on values or variables.

---

# 1. Types of Operators

Java provides several categories of operators:

```text
Operators
│
├── Arithmetic
├── Unary
├── Relational
├── Logical
├── Bitwise
├── Shift
├── Assignment
├── Ternary
└── instanceof
```

---

# 2. Arithmetic Operators

Used for mathematical operations.

```java
int a = 10;
int b = 3;

System.out.println(a + b); // 13
System.out.println(a - b); // 7
System.out.println(a * b); // 30
System.out.println(a / b); // 3
System.out.println(a % b); // 1
```

### Important Interview Point

Integer division removes the fractional part:

```java
System.out.println(10 / 3);
```

Output:

```text
3
```

But:

```java
System.out.println(10.0 / 3);
```

produces a floating-point result.

---

# 3. Unary Operators

Operate on a single operand.

```java
int x = 10;

+x;
-x;

++x;
--x;

x++;
x--;

!flag;
```

Common unary operators:

```text
+    Positive
-    Negative
++   Increment
--   Decrement
!    Logical NOT
~    Bitwise complement
```

---

# 4. Pre-Increment vs Post-Increment

One of the most common interview traps.

### Pre-increment

```java
int x = 10;

int y = ++x;
```

Order:

```text
x becomes 11
↓
11 assigned to y
```

Result:

```text
x = 11
y = 11
```

---

### Post-increment

```java
int x = 10;

int y = x++;
```

Order:

```text
current value 10 used
↓
x becomes 11
```

Result:

```text
x = 11
y = 10
```

### Memory Trick

```text
++x → increment first
x++ → use first
```

---

# 5. Relational Operators

Used to compare values.

```java
int a = 10;
int b = 20;

System.out.println(a == b);
System.out.println(a != b);
System.out.println(a > b);
System.out.println(a < b);
System.out.println(a >= b);
System.out.println(a <= b);
```

Result is always:

```text
true
```

or

```text
false
```

---

# 6. `==` Operator

For primitive types, `==` compares values.

```java
int a = 10;
int b = 10;

System.out.println(a == b);
```

Output:

```text
true
```

For objects, `==` compares **references**, not object content.

```java
String a = new String("Java");
String b = new String("Java");

System.out.println(a == b);
```

Output:

```text
false
```

The two references point to different objects.

---

# 7. `.equals()` vs `==`

For objects:

```java
String a = new String("Java");
String b = new String("Java");

System.out.println(a == b);
System.out.println(a.equals(b));
```

Output:

```text
false
true
```

Conceptually:

```text
==       → reference identity
.equals  → logical/content equality
```

> Exact behavior of `.equals()` depends on how the class implements/overrides it.

---

# 8. Logical Operators

Used with boolean expressions.

```text
&& → AND
|| → OR
!  → NOT
```

Example:

```java
int age = 22;

System.out.println(age >= 18 && age <= 60);
```

Output:

```text
true
```

---

# 9. Short-Circuit Operators

This is an important interview topic.

## `&&`

If the left side is `false`, Java does not evaluate the right side.

```java
false && something
```

The second expression is skipped.

---

## `||`

If the left side is `true`, Java does not evaluate the right side.

```java
true || something
```

The second expression is skipped.

---

# 10. Short-Circuit Example

```java
int x = 10;

if (x > 20 && ++x > 10) {
    System.out.println("Yes");
}

System.out.println(x);
```

Output:

```text
10
```

Why?

```text
x > 20
↓
false
↓
&& short-circuits
↓
++x never executes
```

Therefore:

```text
x = 10
```

---

# 11. `&` vs `&&`

Very important distinction.

### `&&`

Logical AND with short-circuiting.

```java
condition1 && condition2
```

### `&`

Can be used as a bitwise AND for integers and as a non-short-circuit logical AND for booleans.

```java
boolean result = true & false;
```

Unlike `&&`, both operands are evaluated.

---

# 12. `|` vs `||`

Similarly:

```text
|| → logical OR + short-circuit
|  → bitwise OR / non-short-circuit boolean OR
```

Example:

```java
boolean result = true | false;
```

Both operands are evaluated.

---

# 13. Bitwise Operators

Operate on individual bits of integral values.

```text
&   AND
|   OR
^   XOR
~   NOT
```

Example:

```java
int a = 5;
int b = 3;

System.out.println(a & b);
```

Binary:

```text
5 → 0101
3 → 0011
    ----
    0001
```

Result:

```text
1
```

---

# 14. XOR `^`

XOR returns `1` when the corresponding bits are different.

```text
0 ^ 0 → 0
0 ^ 1 → 1
1 ^ 0 → 1
1 ^ 1 → 0
```

Example:

```java
System.out.println(5 ^ 3);
```

```text
0101
0011
----
0110
```

Output:

```text
6
```

---

# 15. Bitwise NOT `~`

`~` flips every bit.

```java
int x = 5;

System.out.println(~x);
```

Output:

```text
-6
```

Important identity:

```text
~x = -(x + 1)
```

So:

```text
~5 = -6
```

---

# 16. Shift Operators

Java provides:

```text
<<   Left shift
>>   Signed right shift
>>>  Unsigned right shift
```

---

## Left Shift `<<`

```java
int x = 5;

System.out.println(x << 1);
```

Binary concept:

```text
0101
 ↓
1010
```

Result:

```text
10
```

For appropriate values, left shifting by one is similar to multiplying by 2.

---

## Signed Right Shift `>>`

Preserves the sign bit.

```java
int x = 8;

System.out.println(x >> 1);
```

Result:

```text
4
```

---

## Unsigned Right Shift `>>>`

Shifts bits right and fills the left side with zeros.

This makes a major difference for negative numbers.

```java
int x = -8;

System.out.println(x >> 1);
System.out.println(x >>> 1);
```

For positive values, `>>` and `>>>` generally produce the same numerical result.

For negative values, they can differ significantly.

---

# 17. Assignment Operators

Basic assignment:

```java
int x = 10;
```

Compound assignment:

```text
+=
-=
*=
/=
%=
&=
|=
^=
<<=
>>=
>>>=
```

Example:

```java
int x = 10;

x += 5;
```

Equivalent conceptually to:

```java
x = x + 5;
```

---

# 18. Compound Assignment — Interview Trap

Consider:

```java
byte x = 10;

x += 5;
```

This is valid.

But:

```java
byte x = 10;

x = x + 5;
```

does not compile.

Why?

Arithmetic operations on `byte` and `short` generally undergo numeric promotion to `int`.

But compound assignment includes an implicit narrowing conversion.

Conceptually:

```java
x += 5;
```

acts roughly like:

```java
x = (byte) (x + 5);
```

---

# 19. Ternary Operator

The ternary operator is a compact conditional expression.

Syntax:

```java
condition ? expression1 : expression2
```

Example:

```java
int age = 22;

String result = age >= 18 ? "Adult" : "Minor";
```

Result:

```text
Adult
```

It is an **expression**, so it produces a value.

---

# 20. `instanceof`

`instanceof` checks whether an object is compatible with a specified type.

```java
String name = "Java";

System.out.println(name instanceof String);
```

Output:

```text
true
```

It is commonly used when working with inheritance and polymorphism.

Example:

```java
Object obj = "Hello";

if (obj instanceof String) {
    System.out.println("It is a String");
}
```

---

# 21. Operator Precedence

When multiple operators appear in one expression, precedence determines evaluation order.

Example:

```java
int result = 10 + 5 * 2;
```

Multiplication happens first:

```text
10 + (5 * 2)
10 + 10
20
```

Not:

```text
(10 + 5) * 2
```

---

# 22. Important Precedence Order

A simplified order:

```text
Highest
   ↓
()
++
--
!
*
/
%
+
-
<
>
<=
>=
==
!=
&
^
|
&&
||
?:
=
   ↓
Lowest
```

### Interview Advice

Don't try to memorize every precedence rule.

When the expression is complicated:

```java
int x = (a + b) * c;
```

use parentheses explicitly.

---

# 23. Associativity

When operators have the same precedence, associativity determines evaluation direction.

Most binary arithmetic operators are evaluated:

```text
Left → Right
```

Example:

```java
int result = 20 / 5 * 2;
```

Evaluation:

```text
(20 / 5) * 2
= 4 * 2
= 8
```

Assignment operators are generally evaluated right-to-left:

```java
int a, b, c;

a = b = c = 10;
```

---

# 24. String Concatenation and `+`

The `+` operator is also used for String concatenation.

```java
System.out.println("Java " + "Developer");
```

Output:

```text
Java Developer
```

---

## Important Trap

```java
System.out.println(10 + 20 + "Java");
```

Evaluation:

```text
10 + 20
↓
30
↓
"30" + "Java"
↓
"30Java"
```

Output:

```text
30Java
```

But:

```java
System.out.println("Java" + 10 + 20);
```

Output:

```text
Java1020
```

Once String concatenation starts, subsequent `+` operations concatenate.

---

# 25. `+` and Evaluation Order

```java
int x = 10;

System.out.println(x + x + " " + x);
```

Evaluation occurs from left to right according to the relevant operator rules.

Result:

```text
20 10
```

---

# 26. Division by Zero

### Integer

```java
int x = 10 / 0;
```

Causes:

```text
ArithmeticException
```

### Floating Point

```java
double x = 10.0 / 0.0;
```

Produces:

```text
Infinity
```

And:

```java
double x = 0.0 / 0.0;
```

produces:

```text
NaN
```

This is an important interview distinction.

---

# 27. Common Interview Traps

## Trap 1

```java
int x = 5;

System.out.println(x++ + ++x);
```

Evaluate carefully:

```text
x++ → uses 5, then x = 6
++x → x = 7, uses 7
```

Result:

```text
12
```

Final:

```text
x = 7
```

---

## Trap 2

```java
boolean a = false;
boolean b = true;

System.out.println(a && ++someValue);
```

The right side is not evaluated because `&&` short-circuits.

---

## Trap 3

```java
System.out.println(10 + 20 + "Hello" + 10 + 20);
```

Result:

```text
30Hello1020
```

---

## Trap 4

```java
System.out.println(10 / 3);
```

Result:

```text
3
```

Not:

```text
3.333...
```

---

## Trap 5

```java
System.out.println(10.0 / 0.0);
```

Result:

```text
Infinity
```

Not `ArithmeticException`.

---

# 28. Important Comparisons

| Operator     | Meaning                                     |                                           |                  |
| ------------ | ------------------------------------------- | ----------------------------------------- | ---------------- |
| `=`          | Assignment                                  |                                           |                  |
| `==`         | Equality comparison                         |                                           |                  |
| `!=`         | Not equal                                   |                                           |                  |
| `&&`         | Short-circuit AND                           |                                           |                  |
| `&`          | Bitwise AND / non-short-circuit boolean AND |                                           |                  |
| `            |                                             | `                                         | Short-circuit OR |
| `            | `                                           | Bitwise OR / non-short-circuit boolean OR |                  |
| `++x`        | Increment, then use                         |                                           |                  |
| `x++`        | Use, then increment                         |                                           |                  |
| `>>`         | Signed right shift                          |                                           |                  |
| `>>>`        | Unsigned right shift                        |                                           |                  |
| `?:`         | Ternary conditional                         |                                           |                  |
| `instanceof` | Type compatibility check                    |                                           |                  |

---

# 🎯 Top 10 Interview Questions

## 1. What is the difference between `==` and `.equals()`?

For primitives, `==` compares values.

For object references, `==` checks reference identity, while `.equals()` checks logical equality according to the class's implementation.

---

## 2. What is the difference between `&&` and `&`?

`&&` is a short-circuit logical AND.

`&` performs bitwise AND for integral operands and does not short-circuit for boolean operands.

---

## 3. What is the difference between `||` and `|`?

`||` short-circuits when the left operand is `true`.

`|` does not short-circuit.

---

## 4. Difference between `++i` and `i++`?

```text
++i → increment first, then use
i++ → use first, then increment
```

---

## 5. Why does `byte +=` work while `byte = byte +` may not?

Binary numeric operations promote `byte` to `int`.

Compound assignment includes an implicit conversion back to the left-hand type.

---

## 6. What is short-circuit evaluation?

Java may skip evaluating the right-hand operand of `&&` or `||` when its result is already determined.

---

## 7. What is the difference between `>>` and `>>>`?

```text
>>>
→ unsigned right shift
→ fills with zeros

>>
→ signed right shift
→ preserves sign
```

---

## 8. What is operator precedence?

It determines which operators are evaluated before others when parentheses are not used.

---

## 9. Why does `10 + 20 + "Java"` produce `30Java`?

The arithmetic operation occurs before String concatenation:

```text
10 + 20 → 30
30 + "Java" → "30Java"
```

---

## 10. What happens when dividing integers by zero vs floating-point values by zero?

```text
Integer → ArithmeticException
Floating point → Infinity / -Infinity / NaN
```

depending on the operands.

---

# 🔥 Top 10 Must Remember

```text
1. == with primitives → value comparison
2. == with references → reference identity
3. equals() → logical equality
4. && and || → short-circuit
5. & and | → don't short-circuit
6. ++x → increment first
7. x++ → use first
8. >> → signed right shift
9. >>> → unsigned right shift
10. int division by zero → ArithmeticException
```

---

# 🎤 30-Second Interview Answer

> Operators in Java are symbols used to perform operations on values and variables. Java provides arithmetic, unary, relational, logical, bitwise, shift, assignment, ternary, and `instanceof` operators. An important distinction is that `==` compares primitive values but compares reference identity for objects, while `.equals()` is generally used for logical equality. Java also supports short-circuit operators `&&` and `||`, where the second operand may not be evaluated. Operator precedence determines evaluation order, and parentheses can be used to make expressions explicit.

---

# ⚡ Quick Revision

```text
Arithmetic
+ - * / %

Unary
++ -- + - !

Relational
== != > < >= <=

Logical
&& || !

Bitwise
& | ^ ~

Shift
<< >> >>>

Assignment
= += -= *= /= %=

Ternary
?:

Type Check
instanceof
```

---

# 🧠 Memory Tricks

```text
&& → "Both, but skip if first is false"

|| → "Either, but skip if first is true"

++x → "Change first"

x++ → "Change later"

>> → "Keep the sign"

>>> → "Fill with zero"

== → "Same reference for objects"

.equals() → "Same logical content"
```
