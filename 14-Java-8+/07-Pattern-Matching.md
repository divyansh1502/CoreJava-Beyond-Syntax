
# 🎯 Pattern Matching in Java

> **Pattern matching allows Java to combine type checking, type casting, and conditional logic into a more concise and type-safe form, reducing boilerplate in operations such as `instanceof` checks and `switch` expressions.**

---

# 📑 Table of Contents

- [1. What Is Pattern Matching?](#1-what-is-pattern-matching)
- [2. Why Was Pattern Matching Introduced?](#2-why-was-pattern-matching-introduced)
- [3. Traditional `instanceof`](#3-traditional-instanceof)
- [4. Pattern Matching with `instanceof`](#4-pattern-matching-with-instanceof)
- [5. Type Pattern](#5-type-pattern)
- [6. Pattern Variable](#6-pattern-variable)
- [7. Scope of Pattern Variables](#7-scope-of-pattern-variables)
- [8. Pattern Matching with Logical Operators](#8-pattern-matching-with-logical-operators)
- [9. `&&` with Pattern Matching](#9--with-pattern-matching)
- [10. `||` with Pattern Matching](#10--with-pattern-matching)
- [11. Negated Patterns](#11-negated-patterns)
- [12. Pattern Matching and `null`](#12-pattern-matching-and-null)
- [13. Pattern Matching with `switch`](#13-pattern-matching-with-switch)
- [14. Traditional `switch` vs Pattern Matching](#14-traditional-switch-vs-pattern-matching)
- [15. Type Patterns in `switch`](#15-type-patterns-in-switch)
- [16. Guarded Pattern Logic](#16-guarded-pattern-logic)
- [17. `when` Guards](#17-when-guards)
- [18. Pattern Matching with Sealed Classes](#18-pattern-matching-with-sealed-classes)
- [19. Exhaustive `switch`](#19-exhaustive-switch)
- [20. Record Patterns](#20-record-patterns)
- [21. Nested Record Patterns](#21-nested-record-patterns)
- [22. Record Patterns with `instanceof`](#22-record-patterns-with-instanceof)
- [23. Record Patterns with `switch`](#23-record-patterns-with-switch)
- [24. Generic Type Patterns](#24-generic-type-patterns)
- [25. Pattern Dominance](#25-pattern-dominance)
- [26. `default` and Pattern Matching](#26-default-and-pattern-matching)
- [27. Pattern Matching Evolution](#27-pattern-matching-evolution)
- [28. Common Mistakes](#28-common-mistakes)
- [29. Interview Questions](#29-interview-questions)
- [30. 30-Second Interview Answer](#30-30-second-interview-answer)
- [31. Cheat Sheet](#31-cheat-sheet)
- [32. Final Mental Model](#32-final-mental-model)

---

# 1. What Is Pattern Matching?

Pattern matching allows Java to combine:

```text
Type checking
+
Type casting
+
Variable declaration
```

into a single expression.

Traditional code:

```java
if (obj instanceof String) {

    String str =
        (String) obj;

    System.out.println(
        str.length()
    );
}
```

Pattern matching:

```java
if (obj instanceof String str) {

    System.out.println(
        str.length()
    );
}
```

The second form is shorter and eliminates the explicit cast.

---

# 2. Why Was Pattern Matching Introduced?

Before pattern matching, code often repeated the same operation:

```text
Check type
   ↓
Cast object
   ↓
Store casted object
   ↓
Use object
```

Example:

```java
if (obj instanceof String) {

    String str =
        (String) obj;

    System.out.println(
        str.length()
    );
}
```

Pattern matching combines the first three steps:

```text
Check type
+
Cast
+
Create variable
```

Example:

```java
if (obj instanceof String str) {

    System.out.println(
        str.length()
    );
}
```

---

# 3. Traditional `instanceof`

Before pattern matching:

```java
Object obj =
    "Hello";

if (obj instanceof String) {

    String str =
        (String) obj;

    System.out.println(
        str.toUpperCase()
    );
}
```

There are two separate operations:

```text
instanceof
+
explicit cast
```

This creates unnecessary boilerplate.

---

# 4. Pattern Matching with `instanceof`

Modern Java allows:

```java
Object obj =
    "Hello";

if (obj instanceof String str) {

    System.out.println(
        str.toUpperCase()
    );
}
```

Syntax:

```text
object instanceof Type variable
```

Example:

```java
obj instanceof String str
```

Meaning:

```text
If obj is a String,
bind it to variable str.
```

---

# 5. Type Pattern

The combination:

```java
String str
```

inside:

```java
obj instanceof String str
```

is called a:

```text
type pattern
```

General syntax:

```text
expression instanceof Type variable
```

Example:

```java
if (obj instanceof Integer number) {

    System.out.println(
        number + 10
    );
}
```

Here:

```text
Integer
→ type

number
→ pattern variable
```

---

# 6. Pattern Variable

The variable introduced by a pattern is called a:

```text
pattern variable
```

Example:

```java
if (obj instanceof String str) {

    System.out.println(
        str.length()
    );
}
```

Here:

```text
str
```

is the pattern variable.

The variable is automatically initialized with the correctly cast value.

---

# 7. Scope of Pattern Variables

A pattern variable is available only where the compiler knows that the pattern has matched.

Example:

```java
if (obj instanceof String str) {

    System.out.println(
        str.length()
    );
}
```

`str` is available inside the `if` block.

Outside the block:

```java
System.out.println(
    str.length()
);
```

This is invalid because `str` is out of scope.

---

# 8. Pattern Matching with Logical Operators

Pattern variables interact with Java's flow analysis.

Example:

```java
if (obj instanceof String str &&
    str.length() > 5) {

    System.out.println(
        str
    );
}
```

This works because:

```text
instanceof succeeds
        ↓
str is available
        ↓
str.length() is evaluated
```

---

# 9. `&&` with Pattern Matching

The `&&` operator is especially useful.

Example:

```java
if (obj instanceof String str &&
    !str.isEmpty()) {

    System.out.println(
        str
    );
}
```

The second condition can use `str` because the first condition must succeed before the second condition is evaluated.

This is called:

```text
flow scoping
```

---

# 10. `||` with Pattern Matching

Be careful with `||`.

Example:

```java
if (obj instanceof String str ||
    str.length() > 5) {
}
```

This does not work.

Why?

Because if the first condition is false, Java must evaluate the second condition.

At that point:

```text
str
```

may not exist.

Conceptually:

```text
condition 1 succeeds
        ↓
str exists


condition 1 fails
        ↓
condition 2 executes
        ↓
str may not exist
```

Therefore the compiler does not allow this usage.

---

# 11. Negated Patterns

Pattern variables can also work with negation.

Example:

```java
if (!(obj instanceof String str)) {

    return;
}

System.out.println(
    str.length()
);
```

After the `if` exits through `return`, the compiler knows that:

```text
obj must be a String
```

Therefore `str` is available after the check.

This is another example of:

```text
flow scoping
```

---

# 12. Pattern Matching and `null`

For:

```java
obj instanceof String str
```

if:

```text
obj == null
```

the result is:

```text
false
```

The pattern does not match `null`.

Example:

```java
Object obj =
    null;

if (obj instanceof String str) {

    System.out.println(
        str.length()
    );
}
```

Nothing is printed.

Important:

```text
null instanceof AnyReferenceType
→ false
```

---

# 13. Pattern Matching with `switch`

Pattern matching can also be used with `switch`.

Example:

```java
static String describe(
    Object obj
) {

    return switch (obj) {

        case String str ->
            "String: " + str;

        case Integer number ->
            "Integer: " + number;

        default ->
            "Other";
    };
}
```

The switch checks the object's runtime type.

---

# 14. Traditional `switch` vs Pattern Matching

Traditional switch:

```java
int value =
    10;

switch (value) {

    case 10:
        System.out.println("Ten");
        break;

    default:
        System.out.println("Other");
}
```

Traditional `switch` primarily matches:

```text
constant values
```

Pattern matching allows matching based on:

```text
type
+
conditions
+
structure
```

Example:

```java
return switch (obj) {

    case String str ->
        str.length();

    case Integer number ->
        number * 2;

    default ->
        0;
};
```

---

# 15. Type Patterns in `switch`

A type pattern allows a case to match an object based on its runtime type.

Example:

```java
static String typeOf(
    Object obj
) {

    return switch (obj) {

        case String str ->
            "String";

        case Integer number ->
            "Integer";

        case Double number ->
            "Double";

        default ->
            "Other";
    };
}
```

The pattern variable can be used directly inside the case.

---

# 16. Guarded Pattern Logic

Sometimes checking only the type is not enough.

For example:

```text
String
AND
length > 5
```

Modern Java supports guard conditions using `when` in pattern `switch` cases.

Example:

```java
static String classify(
    Object obj
) {

    return switch (obj) {

        case String str
            when str.length() > 5 ->
                "Long string";

        case String str ->
                "Short string";

        default ->
                "Other";
    };
}
```

The first case handles strings whose length is greater than 5.

The second handles the remaining strings.

---

# 17. `when` Guards

A `when` guard adds an additional condition to a pattern.

Syntax:

```text
case Pattern when condition -> result
```

Example:

```java
case Integer number
    when number > 0 ->
        "Positive";
```

Another example:

```java
case String str
    when str.isEmpty() ->
        "Empty";
```

This allows:

```text
Type pattern
+
Boolean condition
```

in a single case.

---

# 18. Pattern Matching with Sealed Classes

Pattern matching becomes especially powerful with sealed hierarchies.

Example:

```java
sealed interface Shape
    permits Circle, Rectangle {
}
```

```java
record Circle(
    double radius
) implements Shape {
}
```

```java
record Rectangle(
    double width,
    double height
) implements Shape {
}
```

The compiler knows that `Shape` can have only:

```text
Circle
Rectangle
```

as its direct permitted implementations.

---

# 19. Exhaustive `switch`

Because the compiler knows the sealed hierarchy, a `switch` can handle all permitted cases.

Example:

```java
static double area(
    Shape shape
) {

    return switch (shape) {

        case Circle c ->
            Math.PI *
            c.radius() *
            c.radius();

        case Rectangle r ->
            r.width() *
            r.height();
    };
}
```

There is no `default` required here because the compiler can determine that the switch is exhaustive.

This is one of the strongest practical connections between:

```text
Sealed types
+
Pattern matching
```

---

# 20. Record Patterns

Record patterns allow Java to match a record and extract its components directly.

Suppose:

```java
record Point(
    int x,
    int y
) {
}
```

Traditional approach:

```java
if (obj instanceof Point point) {

    System.out.println(
        point.x()
    );

    System.out.println(
        point.y()
    );
}
```

Record pattern:

```java
if (obj instanceof Point(int x, int y)) {

    System.out.println(x);

    System.out.println(y);
}
```

The record is matched and its components are extracted in one operation.

---

# 21. Nested Record Patterns

Record patterns can be nested.

Example:

```java
record Point(
    int x,
    int y
) {
}
```

```java
record Rectangle(
    Point topLeft,
    Point bottomRight
) {
}
```

Now:

```java
Rectangle rectangle =
    new Rectangle(
        new Point(0, 10),
        new Point(20, 0)
    );
```

We can destructure it:

```java
if (rectangle instanceof Rectangle(
        Point(int x1, int y1),
        Point(int x2, int y2)
    )) {

    System.out.println(
        x1
    );

    System.out.println(
        y1
    );

    System.out.println(
        x2
    );

    System.out.println(
        y2
    );
}
```

This avoids manually calling:

```text
rectangle.topLeft()
rectangle.bottomRight()
```

and then extracting their components.

---

# 22. Record Patterns with `instanceof`

Record patterns can be used with `instanceof`.

Example:

```java
record Person(
    String name,
    int age
) {
}
```

Then:

```java
Object obj =
    new Person(
        "Aman",
        22
    );

if (obj instanceof Person(
        String name,
        int age
    )) {

    System.out.println(
        name
    );

    System.out.println(
        age
    );
}
```

The pattern performs:

```text
type check
+
cast
+
component extraction
```

---

# 23. Record Patterns with `switch`

Record patterns can also be used inside `switch`.

Example:

```java
record Point(
    int x,
    int y
) {
}
```

```java
static String describe(
    Point point
) {

    return switch (point) {

        case Point(0, 0) ->
            "Origin";

        case Point(int x, int y)
            when x == 0 ->
                "Y-axis";

        case Point(int x, int y)
            when y == 0 ->
                "X-axis";

        case Point(int x, int y) ->
            "Other point";
    };
}
```

The record pattern extracts:

```text
x
y
```

directly.

---

# 24. Generic Type Patterns

Java's pattern matching still follows Java's generic type rules.

You cannot normally use a parameterized type pattern such as:

```java
if (obj instanceof List<String> list) {
}
```

because of type erasure.

At runtime, Java cannot distinguish:

```text
List<String>
List<Integer>
List<Double>
```

They are all represented as:

```text
List
```

Instead, a reifiable type such as:

```java
if (obj instanceof List<?> list) {
}
```

can be used.

This is an important interview trap.

---

# 25. Pattern Dominance

In a `switch`, more general patterns must not make more specific patterns unreachable.

Example:

```java
switch (obj) {

    case Object value ->
        "Object";

    case String str ->
        "String";
}
```

The second case can never be reached because every `String` is already an `Object`.

Therefore, the compiler rejects the unreachable pattern.

The ordering should generally go from:

```text
more specific
        ↓
more general
```

Example:

```java
switch (obj) {

    case String str ->
        "String";

    case Object value ->
        "Object";
}
```

---

# 26. `default` and Pattern Matching

A `default` case handles values that are not matched by previous cases.

Example:

```java
return switch (obj) {

    case String str ->
        "String";

    case Integer number ->
        "Integer";

    default ->
        "Other";
};
```

With a sealed hierarchy, a `default` may not be necessary if every permitted variant is explicitly handled.

However, adding `default` can change how future extensions of the hierarchy are handled.

---

# 27. Pattern Matching Evolution

Pattern matching was introduced gradually through several Java releases.

A useful timeline:

```text
Java 14
→ Pattern matching for instanceof
  preview


Java 16
→ Pattern matching for instanceof
  finalized


Java 17
→ Pattern matching groundwork
  with modern language evolution


Java 21
→ Pattern matching for switch
  finalized

Java 21
→ Record patterns
  finalized
```

The important finalized features for modern Java are:

```text
Pattern matching for instanceof
→ Java 16

Pattern matching for switch
→ Java 21

Record patterns
→ Java 21
```

---

# 28. Common Mistakes

## ❌ Mistake 1 — Explicitly Casting Again

Unnecessary:

```java
if (obj instanceof String str) {

    String value =
        (String) str;
}
```

`str` is already a `String`.

---

## ❌ Mistake 2 — Using Pattern Variable Outside Its Scope

Invalid:

```java
if (obj instanceof String str) {

    System.out.println(
        str
    );
}

System.out.println(
    str
);
```

The second use is outside the pattern variable's scope.

---

## ❌ Mistake 3 — Using Pattern Variable Incorrectly with `||`

Invalid concept:

```java
if (obj instanceof String str ||
    str.length() > 5) {
}
```

The second condition may execute when the pattern did not match.

---

## ❌ Mistake 4 — Forgetting `null`

For `instanceof`:

```text
null instanceof Type
→ false
```

But a `switch` over `null` requires explicit consideration because `switch` pattern matching has its own null behavior.

---

## ❌ Mistake 5 — Putting General Pattern First

Incorrect ordering:

```java
switch (obj) {

    case Object value ->
        "Object";

    case String str ->
        "String";
}
```

The `String` pattern is dominated by `Object`.

---

## ❌ Mistake 6 — Expecting Generic Type Information at Runtime

This is not allowed:

```java
obj instanceof List<String> list
```

Use a reifiable type such as:

```java
obj instanceof List<?> list
```

---

## ❌ Mistake 7 — Confusing Record Patterns with Records

A record is a type.

A record pattern is a way to match and extract the components of a record.

```text
Record
→ defines data structure

Record pattern
→ extracts data structure
```

---

# 29. Interview Questions

## 🔥 Q1. What is pattern matching in Java?

Pattern matching combines type checking, type casting, and variable binding into a concise syntax.

---

## 🔥 Q2. What problem does pattern matching solve?

It reduces repetitive code such as:

```text
instanceof
+
explicit cast
+
variable declaration
```

---

## 🔥 Q3. What is a pattern variable?

A variable introduced by a pattern.

Example:

```java
if (obj instanceof String str) {
}
```

Here:

```text
str
```

is the pattern variable.

---

## 🔥 Q4. When was pattern matching for `instanceof` finalized?

Java 16.

---

## 🔥 Q5. Can `null` match an `instanceof` type pattern?

No.

```text
null instanceof AnyReferenceType
→ false
```

---

## 🔥 Q6. Why does pattern matching work with `&&`?

Because Java's flow analysis knows that the right-hand side of `&&` is evaluated only if the left-hand side succeeds.

Example:

```java
if (obj instanceof String str &&
    str.length() > 5) {
}
```

---

## 🔥 Q7. Why can't a pattern variable normally be used after `||`?

Because the pattern may fail, meaning the variable may not have been initialized.

---

## 🔥 Q8. What is pattern matching for `switch`?

It allows switch cases to match objects based on their runtime type and bind them to pattern variables.

---

## 🔥 Q9. When was pattern matching for `switch` finalized?

Java 21.

---

## 🔥 Q10. What are record patterns?

Record patterns allow a record to be matched and its components to be extracted directly.

Example:

```java
if (obj instanceof Point(int x, int y)) {
}
```

---

## 🔥 Q11. When were record patterns finalized?

Java 21.

---

## 🔥 Q12. What are sealed classes useful for in pattern matching?

They allow the compiler to know the permitted hierarchy, making exhaustive pattern matching possible.

---

## 🔥 Q13. What is an exhaustive switch?

A switch is exhaustive when every possible input type/value that the compiler considers relevant is covered.

---

## 🔥 Q14. What is pattern dominance?

Pattern dominance occurs when an earlier pattern is so general that a later pattern can never match.

Example:

```java
case Object obj ->
case String str ->
```

The `String` pattern is dominated by `Object`.

---

## 🔥 Q15. Can we use `List<String>` in an `instanceof` pattern?

No, because of generic type erasure.

A reifiable type such as `List<?>` can be used.

---

## 🔥 Q16. What is the difference between a type pattern and a record pattern?

A type pattern primarily checks and binds an object to a type.

A record pattern additionally deconstructs the record and extracts its components.

---

## 🔥 Q17. Does pattern matching remove casting completely from Java?

No.

It reduces explicit casts in supported pattern-matching situations, but traditional casts remain part of Java where needed.

---

## 🔥 Q18. Does pattern matching change Java's type system?

No.

It provides more expressive syntax and compiler flow analysis while preserving Java's type-safety rules.

---

## 🔥 Q19. Can record patterns be nested?

Yes.

A record pattern can contain other record patterns.

---

## 🔥 Q20. Why are sealed classes and pattern matching a good combination?

Sealed classes define a closed set of permitted types, while pattern matching can use that known set to process each variant exhaustively.

---

# 30. 30-Second Interview Answer

> Pattern matching in Java reduces boilerplate around type checking, casting, and variable declaration. With `instanceof`, instead of checking a type and then explicitly casting, we can write a type pattern such as `obj instanceof String str`. Modern Java also supports pattern matching in `switch`, allowing cases to match runtime types and bind variables. Record patterns go further by extracting record components directly. Pattern matching works especially well with sealed hierarchies because the compiler knows the permitted variants and can verify exhaustive handling. These features make type-based code more concise while preserving Java's static type safety.

---

# 31. Cheat Sheet

```text
================= PATTERN MATCHING ====================


MAIN IDEA
--------------------------------------------------------

Type check
    +
Cast
    +
Variable
    ↓
One pattern


TRADITIONAL
--------------------------------------------------------

if (obj instanceof String) {

    String str =
        (String) obj;
}


MODERN
--------------------------------------------------------

if (obj instanceof String str) {

    System.out.println(
        str.length()
    );
}


TYPE PATTERN
--------------------------------------------------------

obj instanceof Type variable


Example:

obj instanceof String str


PATTERN VARIABLE
--------------------------------------------------------

str

Automatically has the matched type.


FLOW SCOPING
--------------------------------------------------------

Pattern variable is available
where the compiler knows the
pattern has matched.


AND
--------------------------------------------------------

if (obj instanceof String str &&
    str.length() > 5) {
}


OR
--------------------------------------------------------

Pattern variable generally
cannot be used on the right
side if the pattern may fail.


NOT
--------------------------------------------------------

if (!(obj instanceof String str)) {

    return;
}

System.out.println(
    str.length()
);


NULL
--------------------------------------------------------

null instanceof String
→ false


SWITCH
--------------------------------------------------------

return switch (obj) {

    case String str ->
        str.length();

    case Integer number ->
        number * 2;

    default ->
        0;
};


GUARD
--------------------------------------------------------

case String str
    when str.length() > 5 ->
        "Long";


SEALED + PATTERN MATCHING
--------------------------------------------------------

sealed interface Shape
    permits Circle, Rectangle


        ↓

Compiler knows variants.


RECORD PATTERN
--------------------------------------------------------

record Point(
    int x,
    int y
) {
}


if (obj instanceof Point(int x, int y)) {

    System.out.println(x);
    System.out.println(y);
}


NESTED RECORD PATTERN
--------------------------------------------------------

record Rectangle(
    Point topLeft,
    Point bottomRight
) {
}


Pattern can extract nested
record components.


GENERIC PATTERN
--------------------------------------------------------

Not allowed:

obj instanceof List<String> list


Because of:

type erasure


Allowed:

obj instanceof List<?> list


DOMINANCE
--------------------------------------------------------

More general pattern
        ↓
can dominate
        ↓
more specific pattern


Wrong:

case Object obj ->
case String str ->


Correct:

case String str ->
case Object obj ->


VERSION MEMORY
--------------------------------------------------------

Java 16
→ instanceof pattern matching


Java 21
→ switch pattern matching
→ record patterns


CORE CONNECTION
--------------------------------------------------------

Sealed Types
      +
Pattern Matching
      ↓
Known variants
      ↓
Exhaustive processing


========================================================
```

---

# 32. Final Mental Model

```text
                 PATTERN MATCHING
                        |
          +-------------+-------------+
          |             |             |
       instanceof     switch       record
          |             |           patterns
          |             |             |
       type check    type cases    destructure
          |             |             |
          +-------------+-------------+
                        |
                        ↓
                LESS BOILERPLATE


====================================================

OLD STYLE

Object
  |
  ↓
instanceof
  |
  ↓
explicit cast
  |
  ↓
local variable
  |
  ↓
use object


NEW STYLE

Object
  |
  ↓
type pattern
  |
  ↓
matched variable
  |
  ↓
use object


====================================================

INSTANCEOF PATTERN

obj instanceof String str


             |
             +── String
             |
             +── str
                    |
                    ↓
              matched String


====================================================

FLOW SCOPING

if (
    obj instanceof String str
    &&
    str.length() > 5
) {

    use(str);
}


Pattern succeeds
      ↓
str available
      ↓
second condition
      ↓
block


====================================================

RECORD PATTERN

Point(x, y)


         ↓

     Point
      /  \
     x    y
     |    |
     ↓    ↓
    10   20


Instead of:

point.x()
point.y()


====================================================

SEALED + PATTERN

             Shape
               |
        +------+------+
        |             |
      Circle       Rectangle
        |             |
     pattern        pattern
        |             |
        +------+------+
               |
               ↓
          exhaustive
            switch


====================================================

PATTERN MATCHING TIMELINE

Java 14
   ↓
preview instanceof patterns

Java 16
   ↓
instanceof patterns finalized

Java 21
   ↓
switch patterns
+
record patterns


====================================================

MEMORY TRICK

INSTANCEOF
→ Check + Cast

SWITCH
→ Match + Handle

RECORD PATTERN
→ Match + Extract

SEALED
→ Known Types

TOGETHER
→ Concise + Exhaustive Type Logic


====================================================

CORE IDEA

Traditional Java says:

"First check the type,
then cast the object."


Pattern matching says:

"Check the type
and bind the object
at the same time."


====================================================
```

---

# 🏁 Final Takeaways

- Pattern matching combines type checking, casting, and variable binding.
- Pattern matching for `instanceof` was finalized in Java 16.
- A type pattern looks like `obj instanceof Type variable`.
- The variable introduced by a pattern is called a pattern variable.
- Pattern variables follow compiler-controlled flow scoping.
- `&&` works naturally with pattern variables because the right side executes only after the left side succeeds.
- `||` requires care because the pattern variable may not exist on every execution path.
- `null instanceof SomeType` is `false`.
- Pattern matching can be used with modern `switch` expressions.
- Pattern matching for `switch` was finalized in Java 21.
- `when` can add a guard condition to a pattern case.
- Record patterns allow records to be matched and deconstructed directly.
- Record patterns were finalized in Java 21.
- Record patterns can be nested.
- Generic type patterns still follow Java's type-erasure rules.
- `List<String>` cannot normally be used as an `instanceof` pattern type.
- `List<?>` is reifiable and can be used.
- Pattern dominance prevents unreachable cases.
- Sealed classes and interfaces provide a known set of permitted variants.
- Sealed hierarchies work particularly well with exhaustive pattern matching.
- Pattern matching does not replace Java's type system or eliminate all explicit casts.
- The main benefit is less boilerplate with clearer, type-safe control flow.

---

