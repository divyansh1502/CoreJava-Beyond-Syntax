# 🔥 07 — flatMap()

`flatMap()` is one of the most important and commonly misunderstood operations in the Java Stream API.

> 💡 **Core idea:** `map()` transforms each element into another element, while `flatMap()` transforms each element into a Stream and then **flattens all those Streams into one Stream**.

```text
map()
    ↓
ONE input → ONE output

flatMap()
    ↓
ONE input → ZERO, ONE, or MANY outputs
    ↓
flatten
```

---

# 📌 Table of Contents

1. [Introduction](#1-introduction)
2. [Why flatMap() Exists](#2-why-flatmap-exists)
3. [map() vs flatMap()](#3-map-vs-flatmap)
4. [Basic flatMap() Example](#4-basic-flatmap-example)
5. [How flatMap() Works Internally](#5-how-flatmap-works-internally)
6. [Flattening Lists](#6-flattening-lists)
7. [Nested Collections](#7-nested-collections)
8. [flatMap() with Strings](#8-flatmap-with-strings)
9. [flatMap() with Arrays](#9-flatmap-with-arrays)
10. [flatMap() with Objects](#10-flatmap-with-objects)
11. [flatMap() with Optional](#11-flatmap-with-optional)
12. [map() Produces Nested Streams](#12-map-produces-nested-streams)
13. [flatMap() Removes Nesting](#13-flatmap-removes-nesting)
14. [flatMap() + filter()](#14-flatmap--filter)
15. [flatMap() + map()](#15-flatmap--map)
16. [flatMap() + distinct()](#16-flatmap--distinct)
17. [flatMap() + sorted()](#17-flatmap--sorted)
18. [flatMap() + reduce()](#18-flatmap--reduce)
19. [Real-World Examples](#19-real-world-examples)
20. [flatMap() vs mapMulti()](#20-flatmap-vs-mapmulti)
21. [flatMap() and Null Values](#21-flatmap-and-null-values)
22. [Complexity](#22-complexity)
23. [DSA Connection](#23-dsa-connection)
24. [Common Mistakes](#24-common-mistakes)
25. [Interview Traps](#25-interview-traps)
26. [Top 15 Interview Questions](#26-top-15-interview-questions)
27. [30-Second Interview Answer](#27-30-second-interview-answer)
28. [Cheat Sheet](#28-cheat-sheet)

---

# 1. Introduction

Suppose we have:

```java
List<List<Integer>> numbers = List.of(
        List.of(1, 2, 3),
        List.of(4, 5),
        List.of(6, 7, 8)
);
```

The structure is:

```text
[
    [1, 2, 3],
    [4, 5],
    [6, 7, 8]
]
```

We want:

```text
[1, 2, 3, 4, 5, 6, 7, 8]
```

This is called **flattening**.

`flatMap()` is designed for this type of operation.

```java
List<Integer> result = numbers.stream()
        .flatMap(List::stream)
        .toList();

System.out.println(result);
```

Output:

```text
[1, 2, 3, 4, 5, 6, 7, 8]
```

---

# 2. Why flatMap() Exists

The main problem solved by `flatMap()` is **nested data**.

Suppose:

```text
List<List<Integer>>
```

We want:

```text
Stream<Integer>
```

Using:

```java
map(List::stream)
```

we get:

```text
Stream<Stream<Integer>>
```

That is still nested.

Using:

```java
flatMap(List::stream)
```

we get:

```text
Stream<Integer>
```

So:

```text
map()
    ↓
nested structure remains

flatMap()
    ↓
nested structure flattened
```

---

# 3. map() vs flatMap()

This is the most important distinction.

## map()

`map()` performs a one-to-one transformation.

```text
T → R
```

Example:

```java
List<Integer> numbers = List.of(1, 2, 3);

List<Integer> result = numbers.stream()
        .map(n -> n * 2)
        .toList();

System.out.println(result);
```

Output:

```text
[2, 4, 6]
```

Each input produces one output:

```text
1 → 2
2 → 4
3 → 6
```

---

## flatMap()

`flatMap()` performs a one-to-many or one-to-zero transformation and then flattens the results.

Conceptually:

```text
T → Stream<R>
```

Example:

```java
List<List<Integer>> numbers = List.of(
        List.of(1, 2),
        List.of(3, 4)
);

List<Integer> result = numbers.stream()
        .flatMap(List::stream)
        .toList();

System.out.println(result);
```

Output:

```text
[1, 2, 3, 4]
```

Conceptually:

```text
[1,2] → Stream(1,2)
[3,4] → Stream(3,4)

        ↓ flatten

Stream(1,2,3,4)
```

---

# 4. Basic flatMap() Example

Consider:

```java
List<List<String>> names = List.of(
        List.of("Aman", "Rahul"),
        List.of("Alex", "Ravi"),
        List.of("Neha")
);
```

We want one list containing every name.

```java
List<String> result = names.stream()
        .flatMap(List::stream)
        .toList();

System.out.println(result);
```

Output:

```text
[Aman, Rahul, Alex, Ravi, Neha]
```

---

## Without flatMap()

Using `map()`:

```java
List<Stream<String>> result = names.stream()
        .map(List::stream)
        .toList();
```

The result type becomes:

```text
List<Stream<String>>
```

This is not what we want.

---

## With flatMap()

```java
List<String> result = names.stream()
        .flatMap(List::stream)
        .toList();
```

Result:

```text
List<String>
```

---

# 5. How flatMap() Works Internally

Consider:

```java
List<List<Integer>> numbers = List.of(
        List.of(1, 2),
        List.of(3, 4),
        List.of(5, 6)
);
```

The Stream initially contains:

```text
Stream<List<Integer>>
```

Each List is converted into a Stream:

```text
List(1,2) → Stream(1,2)

List(3,4) → Stream(3,4)

List(5,6) → Stream(5,6)
```

Conceptually:

```text
Stream<List<Integer>>
        ↓
Stream<Stream<Integer>>
        ↓
     flatten
        ↓
Stream<Integer>
```

Final:

```text
1, 2, 3, 4, 5, 6
```

The key operation is:

```text
flatten
```

---

# 6. Flattening Lists

This is the most common use case.

```java
List<List<Integer>> numbers = List.of(
        List.of(1, 2, 3),
        List.of(4, 5, 6),
        List.of(7, 8, 9)
);

List<Integer> result = numbers.stream()
        .flatMap(List::stream)
        .toList();

System.out.println(result);
```

Output:

```text
[1, 2, 3, 4, 5, 6, 7, 8, 9]
```

---

## Empty Inner Lists

`flatMap()` naturally handles empty Streams.

```java
List<List<Integer>> numbers = List.of(
        List.of(1, 2),
        List.of(),
        List.of(3, 4)
);

List<Integer> result = numbers.stream()
        .flatMap(List::stream)
        .toList();

System.out.println(result);
```

Output:

```text
[1, 2, 3, 4]
```

The empty List contributes zero elements.

Conceptually:

```text
List(1,2) → 2 elements
List()    → 0 elements
List(3,4) → 2 elements
```

---

# 7. Nested Collections

`flatMap()` becomes especially useful with multiple levels of nested data.

Example:

```java
List<List<String>> departments = List.of(
        List.of("Java", "Spring"),
        List.of("React", "JavaScript"),
        List.of("SQL", "Docker")
);
```

Flatten:

```java
List<String> technologies = departments.stream()
        .flatMap(List::stream)
        .toList();

System.out.println(technologies);
```

Output:

```text
[Java, Spring, React, JavaScript, SQL, Docker]
```

---

## Two-Level Nesting

Suppose:

```text
List<List<List<Integer>>>
```

Example:

```java
List<List<List<Integer>>> numbers = List.of(
        List.of(
                List.of(1, 2),
                List.of(3)
        ),
        List.of(
                List.of(4, 5)
        )
);
```

One `flatMap()` removes one level of nesting.

```java
List<List<Integer>> result = numbers.stream()
        .flatMap(List::stream)
        .toList();
```

Result:

```text
[
    [1,2],
    [3],
    [4,5]
]
```

To flatten another level:

```java
List<Integer> result = numbers.stream()
        .flatMap(List::stream)
        .flatMap(List::stream)
        .toList();
```

Result:

```text
[1, 2, 3, 4, 5]
```

---

# 8. flatMap() with Strings

A String can be converted into a Stream of characters.

Example:

```java
List<String> words = List.of(
        "Java",
        "Stream"
);
```

We want every character:

```text
J a v a S t r e a m
```

Using:

```java
flatMapToInt(String::chars)
```

we can process the character codes.

```java
List<Integer> characters = words.stream()
        .flatMapToInt(String::chars)
        .boxed()
        .toList();

System.out.println(characters);
```

The result contains Unicode code points represented as integers.

---

## Converting to Characters

```java
List<Character> characters = words.stream()
        .flatMapToInt(String::chars)
        .mapToObj(c -> (char) c)
        .toList();

System.out.println(characters);
```

Output:

```text
[J, a, v, a, S, t, r, e, a, m]
```

---

# 9. flatMap() with Arrays

Suppose:

```java
List<int[]> arrays = List.of(
        new int[]{1, 2, 3},
        new int[]{4, 5},
        new int[]{6, 7}
);
```

For primitive `int[]`, use `flatMapToInt()`:

```java
int[] result = arrays.stream()
        .flatMapToInt(Arrays::stream)
        .toArray();
```

Result:

```text
[1, 2, 3, 4, 5, 6, 7]
```

---

## Object Arrays

For arrays of reference types:

```java
List<String[]> arrays = List.of(
        new String[]{"Java", "Spring"},
        new String[]{"React", "SQL"}
);
```

Use:

```java
List<String> result = arrays.stream()
        .flatMap(Arrays::stream)
        .toList();

System.out.println(result);
```

Output:

```text
[Java, Spring, React, SQL]
```

---

# 10. flatMap() with Objects

`flatMap()` becomes extremely useful when objects contain collections.

Consider:

```java
record Student(
        String name,
        List<String> subjects
) {
}
```

Data:

```java
List<Student> students = List.of(
        new Student(
                "Aman",
                List.of("Java", "SQL")
        ),
        new Student(
                "Rahul",
                List.of("Spring", "Java")
        ),
        new Student(
                "Alex",
                List.of("React", "JavaScript")
        )
);
```

We want all subjects.

```java
List<String> subjects = students.stream()
        .flatMap(student -> student.subjects().stream())
        .toList();

System.out.println(subjects);
```

Output:

```text
[Java, SQL, Spring, Java, React, JavaScript]
```

---

## Unique Subjects

Combine `flatMap()` with `distinct()`:

```java
List<String> subjects = students.stream()
        .flatMap(student -> student.subjects().stream())
        .distinct()
        .toList();

System.out.println(subjects);
```

Output:

```text
[Java, SQL, Spring, React, JavaScript]
```

---

# 11. flatMap() with Optional

`Optional` also has a `flatMap()` method.

This is separate from `Stream.flatMap()`, but the underlying idea is similar:

```text
avoid nested wrapper
```

Example:

```java
Optional<String> name = Optional.of("Aman");

Optional<Integer> length = name
        .map(String::length);
```

Here:

```text
String
  ↓
Integer
```

Using `map()` is appropriate because:

```text
String → Integer
```

---

Suppose a method already returns `Optional`:

```java
Optional<String> findName() {
    return Optional.of("Aman");
}
```

If we use `map()`:

```java
Optional<Optional<String>> result =
        Optional.of("Student")
                .map(value -> findName());
```

This creates nesting:

```text
Optional<Optional<String>>
```

Using `flatMap()`:

```java
Optional<String> result =
        Optional.of("Student")
                .flatMap(value -> findName());
```

The nesting is flattened.

---

## Important

There are two different `flatMap()` concepts:

```text
Stream.flatMap()
Optional.flatMap()
```

Both follow the general idea:

```text
transform + flatten
```

but they operate on different types.

---

# 12. map() Produces Nested Streams

This is one of the most important interview concepts.

Suppose:

```java
List<List<Integer>> numbers = List.of(
        List.of(1, 2),
        List.of(3, 4)
);
```

Using:

```java
numbers.stream()
        .map(List::stream)
```

we get:

```text
Stream<Stream<Integer>>
```

Why?

Because:

```text
List<Integer>
     ↓
Stream<Integer>
```

and the outer Stream now contains Streams.

So:

```text
Stream<List<Integer>>
          ↓ map()
Stream<Stream<Integer>>
```

---

# 13. flatMap() Removes Nesting

Using:

```java
numbers.stream()
        .flatMap(List::stream)
```

we get:

```text
Stream<Integer>
```

Conceptually:

```text
Stream<List<Integer>>
          ↓
       flatMap()
          ↓
Stream<Integer>
```

---

## The Most Important Comparison

```text
map()
────────────────────────────
T → R

Example:

List<Integer>
    ↓
map(n → n * 2)
    ↓
List<Integer>
```

With nested data:

```text
map()
────────────────────────────
T → Stream<R>

Stream<List<Integer>>
        ↓
map(List::stream)
        ↓
Stream<Stream<Integer>>
```

---

```text
flatMap()
────────────────────────────
T → Stream<R>

Stream<List<Integer>>
        ↓
flatMap(List::stream)
        ↓
Stream<Integer>
```

---

# 14. flatMap() + filter()

You can filter after flattening.

Example:

```java
List<List<Integer>> numbers = List.of(
        List.of(1, 2, 3),
        List.of(4, 5, 6),
        List.of(7, 8, 9)
);

List<Integer> result = numbers.stream()
        .flatMap(List::stream)
        .filter(n -> n % 2 == 0)
        .toList();

System.out.println(result);
```

Output:

```text
[2, 4, 6, 8]
```

Pipeline:

```text
Nested Lists
     ↓
flatMap()
     ↓
Flat Stream
     ↓
filter()
     ↓
Even Numbers
```

---

# 15. flatMap() + map()

You can transform flattened elements.

Example:

```java
List<List<Integer>> numbers = List.of(
        List.of(1, 2),
        List.of(3, 4)
);

List<Integer> result = numbers.stream()
        .flatMap(List::stream)
        .map(n -> n * n)
        .toList();

System.out.println(result);
```

Output:

```text
[1, 4, 9, 16]
```

Pipeline:

```text
[[1,2], [3,4]]
        ↓
    flatMap()
        ↓
 [1,2,3,4]
        ↓
      map()
        ↓
 [1,4,9,16]
```

---

# 16. flatMap() + distinct()

Example:

```java
List<List<Integer>> numbers = List.of(
        List.of(1, 2, 3),
        List.of(2, 3, 4),
        List.of(4, 5, 6)
);

List<Integer> result = numbers.stream()
        .flatMap(List::stream)
        .distinct()
        .toList();

System.out.println(result);
```

Output:

```text
[1, 2, 3, 4, 5, 6]
```

Pipeline:

```text
Nested collections
        ↓
flatMap()
        ↓
[1,2,3,2,3,4,4,5,6]
        ↓
distinct()
        ↓
[1,2,3,4,5,6]
```

---

# 17. flatMap() + sorted()

Example:

```java
List<List<Integer>> numbers = List.of(
        List.of(5, 2),
        List.of(8, 1),
        List.of(4, 3)
);

List<Integer> result = numbers.stream()
        .flatMap(List::stream)
        .sorted()
        .toList();

System.out.println(result);
```

Output:

```text
[1, 2, 3, 4, 5, 8]
```

---

# 18. flatMap() + reduce()

Example:

```java
List<List<Integer>> numbers = List.of(
        List.of(1, 2),
        List.of(3, 4),
        List.of(5, 6)
);

int sum = numbers.stream()
        .flatMap(List::stream)
        .reduce(0, Integer::sum);

System.out.println(sum);
```

Output:

```text
21
```

Pipeline:

```text
[[1,2], [3,4], [5,6]]
            ↓
        flatMap()
            ↓
 [1,2,3,4,5,6]
            ↓
          reduce
            ↓
            21
```

---

# 19. Real-World Examples

## 19.1 Students and Subjects

```java
record Student(
        String name,
        List<String> subjects
) {
}
```

```java
List<Student> students = List.of(
        new Student("Aman", List.of("Java", "SQL")),
        new Student("Rahul", List.of("Java", "Spring")),
        new Student("Alex", List.of("React", "SQL"))
);
```

Get all unique subjects:

```java
List<String> subjects = students.stream()
        .flatMap(student -> student.subjects().stream())
        .distinct()
        .toList();

System.out.println(subjects);
```

Output:

```text
[Java, SQL, Spring, React]
```

---

## 19.2 Employees and Skills

```java
record Employee(
        String name,
        List<String> skills
) {
}
```

```java
List<Employee> employees = List.of(
        new Employee(
                "Aman",
                List.of("Java", "Spring Boot", "SQL")
        ),
        new Employee(
                "Rahul",
                List.of("Java", "Docker")
        ),
        new Employee(
                "Alex",
                List.of("React", "JavaScript")
        )
);
```

Find all unique skills:

```java
List<String> skills = employees.stream()
        .flatMap(employee -> employee.skills().stream())
        .distinct()
        .sorted()
        .toList();

System.out.println(skills);
```

Possible output:

```text
[Docker, Java, JavaScript, React, SQL, Spring Boot]
```

---

## 19.3 Orders and Products

```java
record Order(
        int id,
        List<String> products
) {
}
```

```java
List<Order> orders = List.of(
        new Order(
                1,
                List.of("Laptop", "Mouse")
        ),
        new Order(
                2,
                List.of("Keyboard", "Mouse")
        ),
        new Order(
                3,
                List.of("Monitor", "Keyboard")
        )
);
```

Find every unique product:

```java
List<String> products = orders.stream()
        .flatMap(order -> order.products().stream())
        .distinct()
        .toList();

System.out.println(products);
```

Output:

```text
[Laptop, Mouse, Keyboard, Monitor]
```

---

## 19.4 Words to Characters

```java
List<String> words = List.of(
        "Java",
        "Stream",
        "API"
);
```

```java
List<Character> characters = words.stream()
        .flatMapToInt(String::chars)
        .mapToObj(c -> (char) c)
        .toList();

System.out.println(characters);
```

Output:

```text
[J, a, v, a, S, t, r, e, a, m, A, P, I]
```

---

## 19.5 Sentences to Words

Suppose:

```java
List<String> sentences = List.of(
        "Java is powerful",
        "Streams are useful"
);
```

We can split each sentence into words:

```java
List<String> words = sentences.stream()
        .flatMap(sentence -> Arrays.stream(sentence.split(" ")))
        .toList();

System.out.println(words);
```

Output:

```text
[Java, is, powerful, Streams, are, useful]
```

This is an extremely common `flatMap()` interview pattern.

---

# 20. flatMap() vs mapMulti()

Modern Java also provides:

```java
mapMulti()
```

Both can perform one-to-many transformations.

---

## flatMap()

The mapper returns a Stream:

```java
stream.flatMap(value -> someStream(value))
```

Example:

```java
List<String> result = sentences.stream()
        .flatMap(sentence -> Arrays.stream(sentence.split(" ")))
        .toList();
```

---

## mapMulti()

`mapMulti()` allows the mapper to directly push multiple results to a consumer.

Conceptually:

```text
input
  ↓
mapMulti
  ↓
emit zero/many values
```

Example:

```java
List<String> result = sentences.stream()
        .<String>mapMulti((sentence, consumer) -> {
            for (String word : sentence.split(" ")) {
                consumer.accept(word);
            }
        })
        .toList();
```

---

## Difference

```text
flatMap()
    ↓
create/return Stream
    ↓
flatten

mapMulti()
    ↓
directly emit values
```

`mapMulti()` can be useful when producing a Stream for each input would be unnecessary or expensive.

For most beginner and interview situations, understand `flatMap()` first.

---

# 21. flatMap() and Null Values

A mapper used by `flatMap()` should return a Stream.

If the mapper returns `null`, this should not be treated as a normal Stream result.

Avoid:

```java
.flatMap(value -> null)
```

Instead, represent zero results with an empty Stream:

```java
.flatMap(value -> Stream.empty())
```

---

## Example

```java
List<String> names = List.of(
        "Aman",
        "Rahul"
);

List<String> result = names.stream()
        .flatMap(name -> {
            if (name.startsWith("A")) {
                return Stream.of(name);
            }

            return Stream.empty();
        })
        .toList();

System.out.println(result);
```

Output:

```text
[Aman]
```

This demonstrates an important idea:

```text
flatMap()
can produce zero, one, or many elements
```

---

# 22. Complexity

There is no single complexity for `flatMap()` because it depends on how many elements each input produces.

Suppose:

```text
n = number of outer elements
m = total number of flattened elements
```

Then traversing all produced elements is generally:

```text
O(m)
```

If every outer element produces approximately `k` inner elements:

```text
m ≈ n × k
```

so the work is approximately:

```text
O(n × k)
```

---

## Example

Suppose:

```text
100 students
```

and each has:

```text
5 subjects
```

Total flattened elements:

```text
100 × 5 = 500
```

So the flattening traversal processes approximately:

```text
O(500)
```

elements.

---

## Additional Operations

If you do:

```java
.flatMap(...)
.distinct()
.sorted()
```

then additional costs apply:

```text
flatMap  → O(m)
distinct → approximately O(m) average-case mental model
sorted   → O(m log m)
```

So sorting can dominate the overall complexity.

---

# 23. DSA Connection

`flatMap()` is strongly connected to common DSA patterns involving nested data.

---

## 23.1 Flatten a 2D Structure

Given:

```text
[
 [1,2],
 [3,4],
 [5,6]
]
```

Flatten:

```java
List<Integer> result = matrix.stream()
        .flatMap(List::stream)
        .toList();
```

Result:

```text
[1,2,3,4,5,6]
```

---

## 23.2 Find All Unique Values

```java
List<Integer> result = matrix.stream()
        .flatMap(List::stream)
        .distinct()
        .toList();
```

Pattern:

```text
nested
  ↓
flatten
  ↓
unique
```

---

## 23.3 Flatten + Filter

```java
List<Integer> result = matrix.stream()
        .flatMap(List::stream)
        .filter(n -> n > 5)
        .toList();
```

Pattern:

```text
flatten
  ↓
filter
```

---

## 23.4 Flatten + Sort

```java
List<Integer> result = matrix.stream()
        .flatMap(List::stream)
        .sorted()
        .toList();
```

---

## 23.5 Flatten + Aggregate

```java
int sum = matrix.stream()
        .flatMap(List::stream)
        .mapToInt(Integer::intValue)
        .sum();
```

Pattern:

```text
flatten
  ↓
primitive mapping
  ↓
aggregation
```

---

# 24. Common Mistakes

## Mistake 1: Confusing map() and flatMap()

If you have:

```text
List<List<Integer>>
```

and want:

```text
List<Integer>
```

you generally need:

```java
.flatMap(List::stream)
```

not:

```java
.map(List::stream)
```

---

## Mistake 2: Thinking flatMap() Only Works with Lists

It can work with any source where each element can be transformed into a Stream.

Examples:

```text
List
Set
Arrays
Strings
custom objects
Optional
```

---

## Mistake 3: Thinking flatMap() Always Produces More Elements

It can produce:

```text
zero
one
many
```

elements for each input.

Example:

```java
.flatMap(value -> Stream.empty())
```

produces zero elements.

---

## Mistake 4: Forgetting the Flattening Step

This:

```java
.map(List::stream)
```

does not flatten.

This:

```java
.flatMap(List::stream)
```

does.

---

## Mistake 5: Overusing flatMap()

If you simply need:

```text
T → R
```

use:

```java
map()
```

Use `flatMap()` when the transformation naturally produces a Stream or nested structure.

---

## Mistake 6: Confusing Stream.flatMap() with Optional.flatMap()

They follow a similar conceptual pattern, but they belong to different APIs:

```text
Stream.flatMap()
Optional.flatMap()
```

---

# 25. Interview Traps

## Trap 1

**What is the main purpose of flatMap()?**

To transform each element into a Stream and flatten the resulting Streams into one Stream.

---

## Trap 2

**What is the difference between map() and flatMap()?**

```text
map:
T → R

flatMap:
T → Stream<R>
then flatten
```

---

## Trap 3

**What happens when map() is used on nested collections?**

It can produce a nested Stream:

```text
Stream<Stream<T>>
```

if the mapper itself returns a Stream.

---

## Trap 4

**Can flatMap() produce zero elements?**

Yes.

An element can be mapped to:

```java
Stream.empty()
```

---

## Trap 5

**Can flatMap() produce multiple elements?**

Yes.

That is one of its main purposes.

---

## Trap 6

**Is flatMap() terminal?**

No.

It is an intermediate operation.

---

## Trap 7

**Is flatMap() lazy?**

Yes.

It is evaluated when a terminal operation consumes the pipeline.

---

## Trap 8

**Does flatMap() necessarily change the data type?**

Not necessarily.

Its generic output type can be the same or different from the input type.

---

## Trap 9

**Can flatMap() flatten multiple levels automatically?**

No.

One `flatMap()` generally removes one level of Stream nesting.

Multiple nested levels may require multiple flattening operations.

---

## Trap 10

**Does flatMap() guarantee a specific order?**

For ordered streams, encounter-order behavior is generally preserved by the sequential operation. Parallel pipelines have additional ordering considerations depending on the terminal operation and stream characteristics.

---

# 26. Top 15 Interview Questions

## Q1. What is flatMap()?

`flatMap()` is an intermediate Stream operation that transforms each input element into a Stream and then flattens those resulting Streams into a single Stream.

---

## Q2. Why is flatMap() used?

It is mainly used when dealing with nested structures such as:

```text
List<List<T>>
```

or when one input can produce multiple output elements.

---

## Q3. What is the difference between map() and flatMap()?

```text
map:
T → R

flatMap:
T → Stream<R> → flatten
```

---

## Q4. What happens when you use map(List::stream)?

You can get:

```text
Stream<Stream<T>>
```

which remains nested.

---

## Q5. What happens when you use flatMap(List::stream)?

You get:

```text
Stream<T>
```

because the nested Streams are flattened.

---

## Q6. Is flatMap() intermediate or terminal?

Intermediate.

---

## Q7. Is flatMap() lazy?

Yes.

---

## Q8. Can flatMap() produce zero elements?

Yes.

An element can return:

```java
Stream.empty()
```

---

## Q9. Can flatMap() produce multiple elements?

Yes.

For example:

```java
.flatMap(list -> list.stream())
```

can produce multiple elements for one input List.

---

## Q10. How do you flatten a List<List<Integer>>?

```java
List<Integer> result = numbers.stream()
        .flatMap(List::stream)
        .toList();
```

---

## Q11. How do you flatten sentences into words?

```java
List<String> words = sentences.stream()
        .flatMap(sentence -> Arrays.stream(sentence.split(" ")))
        .toList();
```

---

## Q12. Can flatMap() be combined with filter()?

Yes.

```java
numbers.stream()
        .flatMap(List::stream)
        .filter(n -> n > 10)
        .toList();
```

---

## Q13. Can flatMap() be combined with distinct()?

Yes.

```java
numbers.stream()
        .flatMap(List::stream)
        .distinct()
        .toList();
```

---

## Q14. What is the relationship between Optional.flatMap() and Stream.flatMap()?

Both flatten nested wrapper structures after a transformation, but they operate on different types:

```text
Optional.flatMap()
Stream.flatMap()
```

---

## Q15. When should you use map() instead of flatMap()?

Use `map()` when each input produces one output.

Use `flatMap()` when each input naturally produces a Stream of zero, one, or many outputs and you want one flattened Stream.

---

# 27. 30-Second Interview Answer

> **flatMap() is an intermediate Stream operation used to transform each element into a Stream and then flatten those Streams into a single Stream. It is especially useful for nested collections such as List<List<Integer>>. The key difference from map() is that map() performs a normal one-to-one transformation, while flatMap() supports zero-to-many results and removes the resulting nesting. For example, `listOfLists.stream().flatMap(List::stream)` converts a Stream of Lists into a Stream of their individual elements.**

---

# 28. Cheat Sheet

| Operation | Input | Transformation | Output |
|---|---|---|---|
| `map()` | `T` | `T → R` | `Stream<R>` |
| `flatMap()` | `T` | `T → Stream<R>` | `Stream<R>` |
| `flatMapToInt()` | `T` | `T → IntStream` | `IntStream` |
| `flatMapToLong()` | `T` | `T → LongStream` | `LongStream` |
| `flatMapToDouble()` | `T` | `T → DoubleStream` | `DoubleStream` |

---

## Basic flatMap()

```java
List<Integer> result = nestedList.stream()
        .flatMap(List::stream)
        .toList();
```

---

## map() vs flatMap()

```text
map()
────────────────────

[A, B]
   ↓
map(A → Stream(...))
   ↓
Stream<Stream<T>>
```

```text
flatMap()
────────────────────

[A, B]
   ↓
flatMap(A → Stream(...))
   ↓
Stream<T>
```

---

## Most Common Patterns

### Flatten Lists

```java
nestedLists.stream()
        .flatMap(List::stream)
```

### Flatten + Filter

```java
nestedLists.stream()
        .flatMap(List::stream)
        .filter(...)
```

### Flatten + Transform

```java
nestedLists.stream()
        .flatMap(List::stream)
        .map(...)
```

### Flatten + Unique

```java
nestedLists.stream()
        .flatMap(List::stream)
        .distinct()
```

### Flatten + Sort

```java
nestedLists.stream()
        .flatMap(List::stream)
        .sorted()
```

### Flatten + Aggregate

```java
nestedLists.stream()
        .flatMap(List::stream)
        .mapToInt(Integer::intValue)
        .sum()
```

---

# 🧠 Ultimate Memory Trick

Remember:

```text
map()
    ↓
TRANSFORM

flatMap()
    ↓
TRANSFORM + FLATTEN
```

Or even simpler:

```text
map    → ONE → ONE
flatMap → ONE → MANY → FLATTEN
```

Example:

```text
Input:

[
    [1,2],
    [3,4],
    [5,6]
]

flatMap()

        ↓

[1,2,3,4,5,6]
```

---

# 🎯 Final Takeaway

The most important thing to remember is:

```text
map()
    ↓
T → R

flatMap()
    ↓
T → Stream<R>
    ↓
flatten
```

Whenever you see:

```text
List<List<T>>
Set<List<T>>
Object containing List<T>
Stream<Stream<T>>
```

ask yourself:

> **"Do I need to flatten this nested structure?"**

If yes, `flatMap()` is likely the operation you need.

```text
Nested Data
     ↓
 flatMap()
     ↓
Flat Stream
     ↓
filter / map / distinct / sorted / reduce
     ↓
Final Result
```

> 🚀 **Interview memory line: `map()` transforms; `flatMap()` transforms and flattens.**