
# 🧩 Java Optional

> **`Optional<T>` is a container object introduced in Java 8 that represents a value that may be present or absent, making optional results explicit and helping reduce accidental `NullPointerException` caused by unchecked null values.**

---

# 📑 Table of Contents

- [1. What Is Optional?](#1-what-is-optional)
- [2. Why Was Optional Introduced?](#2-why-was-optional-introduced)
- [3. The Null Problem](#3-the-null-problem)
- [4. Creating Optional Objects](#4-creating-optional-objects)
- [5. Optional.of()](#5-optionalof)
- [6. Optional.ofNullable()](#6-optionalofnullable)
- [7. Optional.empty()](#7-optionalempty)
- [8. Checking Whether a Value Is Present](#8-checking-whether-a-value-is-present)
- [9. get()](#9-get)
- [10. ifPresent()](#10-ifpresent)
- [11. ifPresentOrElse()](#11-ifpresentorelse)
- [12. orElse()](#12-orelse)
- [13. orElseGet()](#13-orelseget)
- [14. orElseThrow()](#14-orelsethrow)
- [15. orElse vs orElseGet](#15-orelse-vs-orelseget)
- [16. filter()](#16-filter)
- [17. map()](#17-map)
- [18. flatMap()](#18-flatmap)
- [19. Optional Chaining](#19-optional-chaining)
- [20. Optional as a Return Type](#20-optional-as-a-return-type)
- [21. Optional in Method Parameters](#21-optional-in-method-parameters)
- [22. Optional in Fields](#22-optional-in-fields)
- [23. Optional and NullPointerException](#23-optional-and-nullpointerexception)
- [24. Optional vs null](#24-optional-vs-null)
- [25. Optional vs Empty Collection](#25-optional-vs-empty-collection)
- [26. Primitive Optional Types](#26-primitive-optional-types)
- [27. Common Mistakes](#27-common-mistakes)
- [28. Interview Questions](#28-interview-questions)
- [29. 30-Second Interview Answer](#29-30-second-interview-answer)
- [30. Cheat Sheet](#30-cheat-sheet)
- [31. Final Mental Model](#31-final-mental-model)

---

# 1. What Is Optional?

`Optional<T>` is a container that can contain:

```text
A value
```

or:

```text
No value
```

It belongs to:

```text
java.util
```

Example:

```java
Optional<String> name =
    Optional.of("Java");
```

Conceptually:

```text
Optional<String>
       |
       +── "Java"
```

An empty Optional:

```java
Optional<String> name =
    Optional.empty();
```

Conceptually:

```text
Optional<String>
       |
       +── no value
```

---

# 2. Why Was Optional Introduced?

Before Java 8, methods commonly returned `null` to indicate that a value was unavailable.

Example:

```java
String findUsername() {
    return null;
}
```

The caller had to remember to check:

```java
String username =
    findUsername();

if (username != null) {
    System.out.println(
        username.length()
    );
}
```

If the check was forgotten:

```java
String username =
    findUsername();

System.out.println(
    username.length()
);
```

the program could throw:

```text
NullPointerException
```

Optional makes the possibility of absence explicit in the method's return type:

```java
Optional<String> findUsername() {
    return Optional.empty();
}
```

Now the caller knows:

```text
The result may not exist.
```

---

# 3. The Null Problem

Consider:

```java
class User {

    String name;

    User(String name) {
        this.name = name;
    }
}
```

Suppose:

```java
User user = findUser();
```

The result could be:

```text
User object
```

or:

```text
null
```

Then:

```java
System.out.println(
    user.name
);
```

can throw:

```text
NullPointerException
```

Optional provides an explicit abstraction:

```text
Optional<User>
```

instead of silently returning:

```text
null
```

---

# 4. Creating Optional Objects

There are three fundamental creation methods:

```text
Optional.of()
Optional.ofNullable()
Optional.empty()
```

---

# 5. Optional.of()

`Optional.of(value)` creates an Optional containing a non-null value.

Example:

```java
Optional<String> name =
    Optional.of("Divyansh");
```

The value is present.

---

## Important

`Optional.of()` does **not** accept `null`.

Example:

```java
String name = null;

Optional<String> optional =
    Optional.of(name);
```

This throws:

```text
NullPointerException
```

---

## When to Use

Use `of()` when you know the value must not be null.

```java
Optional<String> name =
    Optional.of("Java");
```

---

# 6. Optional.ofNullable()

`ofNullable()` accepts either:

```text
non-null value
```

or:

```text
null
```

Example:

```java
String name = null;

Optional<String> optional =
    Optional.ofNullable(name);
```

Result:

```text
Optional.empty
```

With a value:

```java
String name = "Java";

Optional<String> optional =
    Optional.ofNullable(name);
```

Result:

```text
Optional[Java]
```

---

## Mental Model

```text
of()
      ↓
must be non-null

ofNullable()
      ↓
null is allowed
      ↓
null → Optional.empty()
```

---

# 7. Optional.empty()

Creates an empty Optional.

Example:

```java
Optional<String> name =
    Optional.empty();
```

It represents:

```text
No value
```

---

## Important

An empty Optional is not the same thing as:

```java
null
```

The Optional object itself exists.

It simply contains no value.

Conceptually:

```text
null
→ no object

Optional.empty()
→ Optional object containing no value
```

---

# 8. Checking Whether a Value Is Present

Two commonly used methods are:

```text
isPresent()
isEmpty()
```

`isPresent()` was introduced with Optional in Java 8.

`isEmpty()` was added later in Java 11.

---

## isPresent()

Returns `true` if a value exists.

```java
Optional<String> name =
    Optional.of("Java");

System.out.println(
    name.isPresent()
);
```

Output:

```text
true
```

---

## Empty Optional

```java
Optional<String> name =
    Optional.empty();

System.out.println(
    name.isPresent()
);
```

Output:

```text
false
```

---

## isEmpty()

Returns `true` when no value is present.

```java
Optional<String> name =
    Optional.empty();

System.out.println(
    name.isEmpty()
);
```

Output:

```text
true
```

---

## Version Note

```text
isPresent()
→ Java 8

isEmpty()
→ Java 11
```

---

# 9. get()

`get()` returns the contained value.

Example:

```java
Optional<String> name =
    Optional.of("Java");

String value =
    name.get();

System.out.println(value);
```

Output:

```text
Java
```

---

## Empty Optional

```java
Optional<String> name =
    Optional.empty();

String value =
    name.get();
```

This throws:

```text
NoSuchElementException
```

---

## ⚠️ Interview Trap

Many developers use:

```java
optional.get();
```

without checking whether a value exists.

That defeats much of the benefit of using Optional.

Avoid blindly calling:

```java
get()
```

on an Optional whose state is unknown.

---

# 10. ifPresent()

`ifPresent()` executes an action only when a value exists.

Example:

```java
Optional<String> name =
    Optional.of("Java");

name.ifPresent(
    value -> System.out.println(value)
);
```

Output:

```text
Java
```

If the Optional is empty:

```java
Optional<String> name =
    Optional.empty();

name.ifPresent(
    value -> System.out.println(value)
);
```

Nothing is printed.

---

## Method Reference

```java
name.ifPresent(
    System.out::println
);
```

---

# 11. ifPresentOrElse()

`ifPresentOrElse()` allows handling both cases:

```text
Value present
Value absent
```

It was introduced in:

```text
Java 9
```

Example:

```java
Optional<String> name =
    Optional.of("Java");

name.ifPresentOrElse(
    value -> System.out.println(value),
    () -> System.out.println("No value")
);
```

If empty:

```java
Optional<String> name =
    Optional.empty();

name.ifPresentOrElse(
    value -> System.out.println(value),
    () -> System.out.println("No value")
);
```

Output:

```text
No value
```

---

# 12. orElse()

`orElse()` provides a fallback value when the Optional is empty.

Example:

```java
Optional<String> name =
    Optional.empty();

String result =
    name.orElse("Unknown");

System.out.println(result);
```

Output:

```text
Unknown
```

If the value exists:

```java
Optional<String> name =
    Optional.of("Java");

String result =
    name.orElse("Unknown");
```

Result:

```text
Java
```

---

# 13. orElseGet()

`orElseGet()` accepts a `Supplier` that generates the fallback value when needed.

Example:

```java
Optional<String> name =
    Optional.empty();

String result =
    name.orElseGet(
        () -> "Unknown"
    );

System.out.println(result);
```

---

## Why Is It Different?

The supplier is invoked only when the Optional is empty.

This makes `orElseGet()` useful when the fallback computation is expensive.

---

# 14. orElseThrow()

`orElseThrow()` throws an exception when no value is present.

Java 10 introduced the no-argument form.

Example:

```java
Optional<String> name =
    Optional.of("Java");

String result =
    name.orElseThrow();
```

If empty:

```java
Optional<String> name =
    Optional.empty();

String result =
    name.orElseThrow();
```

This throws:

```text
NoSuchElementException
```

---

## Custom Exception

Example:

```java
Optional<String> name =
    Optional.empty();

String result =
    name.orElseThrow(
        () -> new IllegalArgumentException(
            "Name is required"
        )
    );
```

---

## Version Note

```text
orElseThrow(Supplier)
→ Java 8

orElseThrow()
→ Java 10
```

---

# 15. orElse vs orElseGet

This is one of the most important Optional interview questions.

Consider:

```java
Optional<String> name =
    Optional.of("Java");
```

Using `orElse()`:

```java
String result =
    name.orElse(
        expensiveOperation()
    );
```

The argument to `orElse()` is evaluated before the method is invoked.

So:

```text
value present
      ↓
fallback expression still evaluated
```

With `orElseGet()`:

```java
String result =
    name.orElseGet(
        () -> expensiveOperation()
    );
```

The supplier is invoked only if the Optional is empty.

---

## Comparison

| `orElse()` | `orElseGet()` |
|---|---|
| Takes a value | Takes a `Supplier` |
| Fallback expression is evaluated eagerly | Supplier is evaluated lazily |
| Simple fallback | Useful for expensive fallback |
| Can execute unnecessary work | Avoids unnecessary fallback computation |

---

## Example

```java
static String createDefault() {

    System.out.println(
        "Creating default"
    );

    return "Default";
}
```

```java
Optional<String> value =
    Optional.of("Java");

String result =
    value.orElse(
        createDefault()
    );
```

`createDefault()` is evaluated even though `"Java"` is already present.

With:

```java
Optional<String> value =
    Optional.of("Java");

String result =
    value.orElseGet(
        () -> createDefault()
    );
```

the supplier is not invoked.

---

# 16. filter()

`Optional.filter()` keeps the value only when it satisfies a predicate.

Example:

```java
Optional<Integer> number =
    Optional.of(20);

Optional<Integer> result =
    number.filter(
        n -> n > 10
    );
```

Result:

```text
Optional[20]
```

If condition fails:

```java
Optional<Integer> number =
    Optional.of(5);

Optional<Integer> result =
    number.filter(
        n -> n > 10
    );
```

Result:

```text
Optional.empty
```

---

## Mental Model

```text
Optional[value]
       ↓
   filter(condition)
       ↓
condition true
       ↓
Optional[value]
```

or:

```text
condition false
       ↓
Optional.empty()
```

---

# 17. map()

`Optional.map()` transforms the contained value.

Example:

```java
Optional<String> name =
    Optional.of("Java");

Optional<Integer> length =
    name.map(
        String::length
    );
```

Result:

```text
Optional[4]
```

---

## Without Optional

```java
String name = "Java";

Integer length =
    name == null
        ? null
        : name.length();
```

Optional lets us express the transformation more declaratively:

```java
Optional<String> name =
    Optional.ofNullable("Java");

Optional<Integer> length =
    name.map(String::length);
```

---

## Important

If the Optional is empty, `map()` does not execute the mapping function.

```text
Optional.empty()
       ↓
map()
       ↓
Optional.empty()
```

---

# 18. flatMap()

`flatMap()` is used when the mapping function already returns an Optional.

Suppose:

```java
Optional<String> name =
    Optional.of("Java");
```

Using `map()` with a function returning Optional can create nested Optional:

```text
Optional<Optional<String>>
```

Example conceptually:

```java
Optional<Optional<String>> result =
    name.map(
        value -> Optional.of(value)
    );
```

`flatMap()` avoids this nesting.

```java
Optional<String> result =
    name.flatMap(
        value -> Optional.of(value)
    );
```

---

## map vs flatMap

```text
map()
→ transforms value

Optional<T>
   ↓
Function<T, R>
   ↓
Optional<R>
```

With an Optional-returning function:

```text
map()
→ Optional<Optional<R>>
```

Whereas:

```text
flatMap()
→ Optional<R>
```

---

## Simple Mental Trick

```text
map()
→ one Optional layer

flatMap()
→ flatten nested Optional
```

---

# 19. Optional Chaining

Optional methods can be chained.

Example:

```java
Optional<String> name =
    Optional.of("Java");

String result =
    name
        .filter(
            value -> value.length() > 3
        )
        .map(String::toUpperCase)
        .orElse("UNKNOWN");
```

Result:

```text
JAVA
```

---

## Pipeline

```text
Optional["Java"]
       ↓
filter(length > 3)
       ↓
Optional["Java"]
       ↓
map(toUpperCase)
       ↓
Optional["JAVA"]
       ↓
orElse("UNKNOWN")
       ↓
"JAVA"
```

---

# 20. Optional as a Return Type

This is one of the most appropriate uses of Optional.

Suppose a repository searches for a user.

Instead of:

```java
User findUserById(int id) {
    return null;
}
```

we can use:

```java
Optional<User> findUserById(int id) {
    return Optional.empty();
}
```

Caller:

```java
Optional<User> user =
    findUserById(101);

user.ifPresent(
    System.out::println
);
```

The return type communicates:

```text
A User may or may not exist.
```

---

# 21. Optional in Method Parameters

Using Optional as a method parameter is generally discouraged as a default API design.

Instead of:

```java
void findUser(Optional<String> name) {
}
```

prefer designing the method according to the actual API semantics, often with:

```java
void findUser(String name) {
}
```

or separate methods when different behaviors are meaningful.

---

## Why?

An Optional is primarily intended to model an optional **result**, especially as a return type.

It is not a universal replacement for null in every position.

---

# 22. Optional in Fields

Using Optional as a class field is also generally discouraged in ordinary Java domain models.

Avoid automatically doing:

```java
class User {

    Optional<String> name;
}
```

unless there is a specific design reason.

For object state, frameworks such as serialization and persistence may have their own expectations, and Optional is primarily designed as a return-value abstraction.

---

# 23. Optional and NullPointerException

Optional can reduce certain null-related errors, but it does not make Java null-safe.

Example:

```java
Optional<String> name =
    Optional.ofNullable(null);
```

Now:

```text
Optional.empty()
```

instead of a null reference.

But this is still dangerous:

```java
Optional<String> name = null;
```

Now:

```java
name.isPresent();
```

can itself throw:

```text
NullPointerException
```

---

## Important Rule

The purpose is:

```text
Avoid Optional being null.
```

Prefer:

```java
Optional.empty();
```

instead of:

```java
null;
```

---

# 24. Optional vs null

| Optional | null |
|---|---|
| Explicitly represents absence | Implicit absence |
| Provides API for handling absence | No methods |
| Can encourage deliberate handling | Easy to forget checks |
| Useful as a return type | Common legacy technique |
| Part of Java API | Language-level null reference |

---

## Important

Optional does not completely eliminate `null`.

Java still allows:

```java
String name = null;
```

Optional is a tool for modeling optional values.

---

# 25. Optional vs Empty Collection

Suppose a method returns users.

One approach:

```java
List<User> findUsers() {
    return Collections.emptyList();
}
```

Another:

```java
Optional<List<User>> findUsers() {
    return Optional.empty();
}
```

These communicate different semantics.

### Empty Collection

Usually means:

```text
The operation succeeded,
but there are zero users.
```

### Optional Empty

Usually means:

```text
There is no result/value.
```

For collection-returning methods, returning an empty collection is often clearer than wrapping a collection inside Optional.

---

# 26. Primitive Optional Types

Using:

```java
Optional<Integer>
```

works, but Java also provides specialized primitive Optional types:

```text
OptionalInt
OptionalLong
OptionalDouble
```

Example:

```java
OptionalInt number =
    OptionalInt.of(100);

System.out.println(
    number.getAsInt()
);
```

---

## Why?

They avoid wrapping primitive values as ordinary objects in the same way `Optional<Integer>` does.

---

## Other Examples

```java
OptionalLong value =
    OptionalLong.of(1000L);
```

```java
OptionalDouble value =
    OptionalDouble.of(10.5);
```

---

# 27. Common Mistakes

## ❌ Mistake 1 — `Optional.of(null)`

Wrong:

```java
Optional<String> name =
    Optional.of(null);
```

Throws:

```text
NullPointerException
```

Use:

```java
Optional<String> name =
    Optional.ofNullable(null);
```

---

## ❌ Mistake 2 — Blindly Calling `get()`

Dangerous:

```java
String name =
    optional.get();
```

when the Optional may be empty.

Prefer:

```java
String name =
    optional.orElse("Unknown");
```

or:

```java
String name =
    optional.orElseThrow();
```

when absence should be an error.

---

## ❌ Mistake 3 — Using `orElse()` for Expensive Computation

Potentially unnecessary:

```java
optional.orElse(
    expensiveOperation()
);
```

Prefer:

```java
optional.orElseGet(
    () -> expensiveOperation()
);
```

when lazy fallback evaluation matters.

---

## ❌ Mistake 4 — Making Optional Fields Everywhere

Optional is not a universal replacement for every nullable field.

---

## ❌ Mistake 5 — Returning null From an Optional Method

Bad:

```java
Optional<String> findName() {
    return null;
}
```

Prefer:

```java
Optional<String> findName() {
    return Optional.empty();
}
```

---

## ❌ Mistake 6 — Using Optional Just to Call `get()`

This:

```java
optional.get();
```

without meaningful absence handling often provides little benefit over the original design.

---

## ❌ Mistake 7 — Confusing map() and flatMap()

Remember:

```text
map()
→ normal transformation

flatMap()
→ transformation returning Optional
→ flatten one Optional layer
```

---

# 28. Interview Questions

## 🔥 Q1. What is Optional in Java?

`Optional<T>` is a container that may contain a value or may be empty.

It was introduced in Java 8 to make optional results explicit and provide APIs for handling absence.

---

## 🔥 Q2. Why was Optional introduced?

Primarily to improve APIs around potentially absent return values and reduce certain classes of null-related errors.

---

## 🔥 Q3. Difference between `of()` and `ofNullable()`?

```text
of()
→ null not allowed
→ null causes NullPointerException

ofNullable()
→ null allowed
→ null becomes Optional.empty()
```

---

## 🔥 Q4. What does `Optional.empty()` mean?

It represents an Optional containing no value.

---

## 🔥 Q5. What happens if `get()` is called on an empty Optional?

It throws:

```text
NoSuchElementException
```

---

## 🔥 Q6. What is `isPresent()`?

It returns `true` when the Optional contains a value.

---

## 🔥 Q7. What is `isEmpty()`?

It returns `true` when the Optional contains no value.

It was introduced in Java 11.

---

## 🔥 Q8. Difference between `orElse()` and `orElseGet()`?

```text
orElse()
→ fallback expression is evaluated eagerly

orElseGet()
→ fallback supplier is evaluated only when needed
```

---

## 🔥 Q9. What is `orElseThrow()`?

It returns the value if present; otherwise it throws an exception.

The no-argument version was added in Java 10.

---

## 🔥 Q10. What does `ifPresent()` do?

It executes a Consumer only when a value is present.

---

## 🔥 Q11. What is `ifPresentOrElse()`?

It handles both cases:

```text
value present
value absent
```

It was introduced in Java 9.

---

## 🔥 Q12. What is `map()` in Optional?

It transforms the contained value when present.

Example:

```java
Optional<String> name =
    Optional.of("Java");

Optional<Integer> length =
    name.map(String::length);
```

---

## 🔥 Q13. What is `flatMap()`?

It is used when the mapping function already returns an Optional, preventing nested:

```text
Optional<Optional<T>>
```

---

## 🔥 Q14. Difference between `map()` and `flatMap()`?

```text
map()
→ transformation

flatMap()
→ transformation + flatten Optional
```

---

## 🔥 Q15. Can Optional itself be null?

Yes, Java technically allows:

```java
Optional<String> value = null;
```

But this defeats the purpose of Optional and should generally be avoided.

---

## 🔥 Q16. Is Optional a replacement for null everywhere?

No.

It is primarily useful for modeling optional results, especially return values.

---

## 🔥 Q17. Should Optional be used as a field?

Generally, not by default.

Use it deliberately based on the API/domain requirements rather than treating it as a universal replacement for nullable fields.

---

## 🔥 Q18. Should Optional be used as a method parameter?

Generally avoid it unless there is a specific API-design reason.

---

## 🔥 Q19. Should a method returning Optional return null?

No.

It should return:

```java
Optional.empty();
```

when no value exists.

---

## 🔥 Q20. What are OptionalInt, OptionalLong and OptionalDouble?

Specialized Optional types for primitive values:

```text
OptionalInt
OptionalLong
OptionalDouble
```

---

## 🔥 Q21. Does Optional eliminate NullPointerException?

No.

It can reduce certain null-related errors, but Java still permits null references.

---

## 🔥 Q22. Which is better: Optional or empty collection?

It depends on semantics.

```text
Empty collection
→ valid result containing zero elements

Optional.empty()
→ absence of a result/value
```

For collection-returning methods, an empty collection is often preferable.

---

## 🔥 Q23. Why is `orElseGet()` sometimes more efficient?

Because the fallback Supplier is evaluated only if the Optional is empty.

---

## 🔥 Q24. Which Java version introduced Optional?

Java 8.

---

## 🔥 Q25. Which Java version introduced `ifPresentOrElse()`?

Java 9.

---

## 🔥 Q26. Which Java version introduced `Optional.isEmpty()`?

Java 11.

---

## 🔥 Q27. Which Java version introduced no-argument `orElseThrow()`?

Java 10.

---

# 29. 30-Second Interview Answer

> `Optional` was introduced in Java 8 as a container that can either contain a value or be empty. It is especially useful for representing potentially absent return values instead of returning null. Common methods include `of()`, `ofNullable()`, `empty()`, `isPresent()`, `ifPresent()`, `orElse()`, `orElseGet()`, `orElseThrow()`, `map()`, and `flatMap()`. One important difference is that `orElse()` evaluates its fallback eagerly, while `orElseGet()` evaluates the supplier only when needed. Optional does not eliminate null from Java and should not be treated as a universal replacement for null.

---

# 30. Cheat Sheet

```text
================ OPTIONAL CHEAT SHEET ================


OPTIONAL
---------------------------------

Optional<T>

Value
   OR
No value


PACKAGE
---------------------------------

java.util.Optional


CREATE WITH VALUE
---------------------------------

Optional.of(value)


CREATE SAFELY
---------------------------------

Optional.ofNullable(value)


EMPTY
---------------------------------

Optional.empty()


IMPORTANT
---------------------------------

of(null)
→ NullPointerException

ofNullable(null)
→ Optional.empty()


CHECK
---------------------------------

isPresent()
→ value exists

isEmpty()
→ no value

isEmpty()
→ Java 11


GET
---------------------------------

get()
→ returns value

Empty + get()
→ NoSuchElementException


ACTION
---------------------------------

ifPresent()
→ execute if value exists


BOTH CASES
---------------------------------

ifPresentOrElse()
→ Java 9


DEFAULT
---------------------------------

orElse(value)
→ fallback


LAZY DEFAULT
---------------------------------

orElseGet(Supplier)
→ lazy fallback


THROW
---------------------------------

orElseThrow()
→ Java 10


CUSTOM THROW
---------------------------------

orElseThrow(Supplier)
→ Java 8


TRANSFORM
---------------------------------

map()
→ transform contained value


FLATTEN
---------------------------------

flatMap()
→ flatten Optional


FILTER
---------------------------------

filter()
→ keep value if condition true


RETURN TYPE
---------------------------------

Optional<T>
→ excellent use case


METHOD PARAMETER
---------------------------------

Usually avoid unless
API semantics justify it.


FIELD
---------------------------------

Usually avoid by default.


COLLECTION
---------------------------------

Prefer empty collection
when "zero elements" is
a valid result.


PRIMITIVE TYPES
---------------------------------

OptionalInt
OptionalLong
OptionalDouble


MOST IMPORTANT DIFFERENCE
---------------------------------

orElse()
→ eager fallback

orElseGet()
→ lazy fallback


MOST IMPORTANT MAP RULE
---------------------------------

map()
→ Optional<R>

flatMap()
→ Optional<R>
when mapper already returns Optional
```

---

# 31. Final Mental Model

```text
                       OPTIONAL<T>
                            |
                 +----------+----------+
                 |                     |
              PRESENT                EMPTY
                 |                     |
              value                  no value
                 |                     |
       +---------+---------+           |
       |         |         |           |
     map()    filter()  ifPresent()    |
       |         |         |           |
       +---------+---------+-----------+
                            |
                    +-------+-------+
                    |               |
                 orElse()      orElseGet()
                    |               |
                 default        lazy default


====================================================

CREATION

value
  |
  +── Optional.of(value)
  |
  +── Optional.ofNullable(value)
  |
  +── Optional.empty()


====================================================

NULL HANDLING

null
 |
 +── of(null)
 |      ↓
 |   Exception
 |
 +── ofNullable(null)
        ↓
   Optional.empty()


====================================================

TRANSFORMATION

Optional<String>
       |
      map()
       ↓
Optional<Integer>


====================================================

NESTED OPTIONAL

Optional<T>
       |
     map()
       |
mapper returns Optional<R>
       ↓
Optional<Optional<R>>


Optional<T>
       |
   flatMap()
       |
mapper returns Optional<R>
       ↓
Optional<R>


====================================================

DEFAULT VALUE

Optional[value]
      |
      +── orElse(default)
      |
      +── orElseGet(() -> default)
      |
      +── orElseThrow()


====================================================

IMPORTANT VERSION MAP

Java 8
  → Optional
  → of
  → ofNullable
  → empty
  → map
  → flatMap
  → filter
  → orElse
  → orElseGet
  → orElseThrow(Supplier)
  → ifPresent

Java 9
  → ifPresentOrElse

Java 10
  → no-arg orElseThrow

Java 11
  → isEmpty


====================================================

BEST MENTAL MODEL

Optional is NOT:

"null but better syntax"

Optional IS:

"An explicit API representation
of a value that may be absent."


====================================================

GOOD API

Optional<User> findUserById(int id)


NO RESULT

Optional.empty()


RESULT EXISTS

Optional.of(user)


CALLER DECIDES

if present
→ use it

if absent
→ fallback
OR
→ throw
OR
→ handle absence
```

---

# 🏁 Final Takeaways

- `Optional<T>` represents a value that may or may not be present.
- It was introduced in Java 8.
- `Optional.of()` requires a non-null value.
- `Optional.ofNullable()` safely handles null.
- `Optional.empty()` represents absence.
- `isPresent()` checks whether a value exists.
- `isEmpty()` checks whether no value exists and was introduced in Java 11.
- `get()` can throw `NoSuchElementException`.
- `ifPresent()` executes an action when a value exists.
- `ifPresentOrElse()` handles both present and absent cases.
- `orElse()` provides a fallback value.
- `orElseGet()` provides a lazily evaluated fallback.
- `orElseThrow()` throws when no value exists.
- `map()` transforms the contained value.
- `flatMap()` handles Optional-returning transformations without nesting.
- `filter()` keeps the value only when a condition is satisfied.
- Optional is particularly useful as a method return type.
- Optional is not a universal replacement for null.
- Returning `null` from an `Optional`-returning method defeats the API's purpose.
- Empty collections are often better than `Optional<Collection>` when zero elements simply means an empty result.
- `OptionalInt`, `OptionalLong`, and `OptionalDouble` support primitive values.
- `orElse()` and `orElseGet()` differ importantly in evaluation behavior.
- Optional helps make absence explicit, but it does not make Java completely null-safe.

---

