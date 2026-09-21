# 08 — Stack

> **Package:** `java.util`  
> **Since:** Java 1.0  
> **Type:** Class  
> **Parent Class:** `Vector`  
> **Purpose:** LIFO (Last In, First Out) data structure  
> **Modern Alternative:** `Deque` / `ArrayDeque`

---

# Table of Contents

1. [Introduction](#1-introduction)
2. [What is Stack](#2-what-is-stack)
3. [LIFO Principle](#3-lifo-principle)
4. [Stack Hierarchy](#4-stack-hierarchy)
5. [Why Stack Exists](#5-why-stack-exists)
6. [Creating a Stack](#6-creating-a-stack)
7. [push()](#7-push)
8. [pop()](#8-pop)
9. [peek()](#9-peek)
10. [empty()](#10-empty)
11. [search()](#11-search)
12. [Common Stack Operations](#12-common-stack-operations)
13. [Internal Working](#13-internal-working)
14. [Stack and Vector Relationship](#14-stack-and-vector-relationship)
15. [Stack and ArrayDeque](#15-stack-and-arraydeque)
16. [Stack vs Queue](#16-stack-vs-queue)
17. [Stack vs ArrayList](#17-stack-vs-arraylist)
18. [Stack vs LinkedList](#18-stack-vs-linkedlist)
19. [Stack Overflow and Underflow](#19-stack-overflow-and-underflow)
20. [Time Complexity](#20-time-complexity)
21. [Real-World Applications](#21-real-world-applications)
22. [DSA Applications](#22-dsa-applications)
23. [Advantages](#23-advantages)
24. [Disadvantages](#24-disadvantages)
25. [Common Mistakes](#25-common-mistakes)
26. [Interview Traps](#26-interview-traps)
27. [Practical Examples](#27-practical-examples)
28. [Top Interview Questions](#28-top-interview-questions)
29. [30-Second Interview Answer](#29-30-second-interview-answer)
30. [Cheat Sheet](#30-cheat-sheet)
31. [Quick Revision](#31-quick-revision)

---

# 1. Introduction

`Stack` is a legacy class in Java used to implement a:

    LIFO

data structure.

LIFO means:

    Last In, First Out

The element inserted last is removed first.

Example:

    push(10)
    push(20)
    push(30)

Stack:

    30  <- Top
    20
    10

If we call:

    pop()

`30` will be removed first.

---

# 2. What is Stack?

`Stack` is a class from:

    java.util

It extends:

    Vector

Therefore:

    Stack
       |
       extends
       |
    Vector

Since Vector implements `List`, Stack also inherits the List-based behavior from Vector.

The primary Stack-specific operations are:

    push()
    pop()
    peek()
    empty()
    search()

---

# 3. LIFO Principle

LIFO means:

    Last In
    First Out

Example:

    push(A)
    push(B)
    push(C)

Stack:

    C <- Top
    B
    A

Now:

    pop()

removes:

    C

Then:

    pop()

removes:

    B

Then:

    pop()

removes:

    A

Therefore:

    Last inserted
         ↓
    First removed

---

# 4. Stack Hierarchy

The simplified hierarchy is:

    Object
       |
    AbstractCollection
       |
    AbstractList
       |
    Vector
       |
    Stack

And through Vector:

    Stack
       |
       +--> List
       |
       +--> RandomAccess
       |
       +--> Cloneable
       |
       +--> Serializable

Important:

> Stack does not directly implement List. It inherits the List behavior through Vector.

---

# 5. Why Stack Exists

Stack was introduced in:

    Java 1.0

It was designed as a LIFO data structure.

However, Stack is now considered a:

    legacy class

because it inherits from Vector.

Modern Java provides the `Deque` interface for stack operations.

For example:

    Deque<Integer> stack =
        new ArrayDeque<>();

This is generally preferred over:

    Stack<Integer> stack =
        new Stack<>();

for new code.

---

# 6. Creating a Stack

Import:

    import java.util.Stack;

Example:

    Stack<Integer> stack =
        new Stack<>();

Now we can perform:

    push()
    pop()
    peek()
    empty()
    search()

Example:

    import java.util.Stack;

    public class Main {

        public static void main(String[] args) {

            Stack<Integer> stack =
                new Stack<>();

            stack.push(10);
            stack.push(20);
            stack.push(30);

            System.out.println(stack);
        }
    }

Output:

    [10, 20, 30]

The rightmost element represents the top.

---

# 7. push()

`push()` adds an element to the top of the Stack.

Syntax:

    stack.push(element);

Example:

    Stack<Integer> stack =
        new Stack<>();

    stack.push(10);
    stack.push(20);
    stack.push(30);

Stack:

    30 <- Top
    20
    10

---

## Return Value

`push()` returns the element that was pushed.

Example:

    Integer value =
        stack.push(40);

    System.out.println(value);

Output:

    40

---

## Another Example

    Stack<String> stack =
        new Stack<>();

    stack.push("Java");
    stack.push("Spring");
    stack.push("React");

Conceptually:

    React <- Top
    Spring
    Java

---

# 8. pop()

`pop()` removes and returns the top element.

Example:

    Stack<Integer> stack =
        new Stack<>();

    stack.push(10);
    stack.push(20);
    stack.push(30);

    int value =
        stack.pop();

    System.out.println(value);

Output:

    30

Stack after pop:

    20 <- Top
    10

---

## Important

`pop()` performs two operations:

    1. Reads the top element
    2. Removes the top element

Therefore:

    pop()
        = get top + remove top

---

## Empty Stack

If `pop()` is called on an empty Stack:

    Stack<Integer> stack =
        new Stack<>();

    stack.pop();

it throws:

    EmptyStackException

This is a runtime exception.

---

# 9. peek()

`peek()` returns the top element without removing it.

Example:

    Stack<Integer> stack =
        new Stack<>();

    stack.push(10);
    stack.push(20);
    stack.push(30);

    int value =
        stack.peek();

    System.out.println(value);

Output:

    30

Stack remains:

    30 <- Top
    20
    10

---

# push vs pop vs peek

    push()
        -> add element

    pop()
        -> return + remove top

    peek()
        -> return top without removing

Memory trick:

    PUSH = Put
    POP = Pull Out
    PEEK = Look

---

# 10. empty()

`empty()` checks whether the Stack contains no elements.

Example:

    Stack<Integer> stack =
        new Stack<>();

    System.out.println(
        stack.empty()
    );

Output:

    true

After:

    stack.push(10);

Then:

    System.out.println(
        stack.empty()
    );

Output:

    false

---

## empty() vs isEmpty()

Because Stack extends Vector, it also inherits:

    isEmpty()

Both can be used.

Example:

    stack.empty();

and:

    stack.isEmpty();

For normal Collection-style code, `isEmpty()` is the more general Collection API method.

---

# 11. search()

Stack provides a legacy-specific method:

    search()

It searches for an element and returns its position from the top.

Example:

    Stack<String> stack =
        new Stack<>();

    stack.push("A");
    stack.push("B");
    stack.push("C");

Stack:

    C <- position 1
    B <- position 2
    A <- position 3

Code:

    System.out.println(
        stack.search("C")
    );

Output:

    1

And:

    System.out.println(
        stack.search("A")
    );

Output:

    3

---

## If Element Does Not Exist

    System.out.println(
        stack.search("X")
    );

Output:

    -1

Therefore:

    found
        -> 1 or greater

    not found
        -> -1

---

# 12. Common Stack Operations

| Operation | Purpose | Result |
|---|---|---|
| `push()` | Add to top | Adds element |
| `pop()` | Remove top | Returns + removes |
| `peek()` | View top | Returns without removing |
| `empty()` | Check empty | `true` / `false` |
| `search()` | Search from top | Position / `-1` |
| `size()` | Number of elements | Integer |
| `isEmpty()` | Check empty | `true` / `false` |
| `clear()` | Remove all | Empty Stack |
| `contains()` | Search for existence | `true` / `false` |

---

# 13. Internal Working

Stack itself does not introduce a completely separate storage mechanism.

It inherits Vector's array-based implementation.

Conceptually:

    Stack
      |
      v
    Vector
      |
      v
    Object[]

Example:

    stack.push(10);
    stack.push(20);
    stack.push(30);

Internal conceptual representation:

    [10][20][30]
              ^
              |
             top

The top of the Stack is represented by the last element of the Vector.

Therefore:

    push()
        -> add element at end

    pop()
        -> remove last element

    peek()
        -> access last element

---

# 14. Stack and Vector Relationship

This is extremely important for interviews.

Stack:

    extends Vector

Therefore Stack inherits many Vector methods.

For example:

    add()
    get()
    set()
    remove()
    size()
    contains()
    clear()

are available on Stack.

Example:

    Stack<Integer> stack =
        new Stack<>();

    stack.push(10);
    stack.push(20);

    stack.add(30);

This is technically allowed.

But it can violate the conceptual restriction of a pure Stack because `add()` is a List operation and allows insertion through positions other than the top-oriented Stack API.

This is one reason the inheritance design of `Stack` is considered less ideal.

---

# 15. Stack vs ArrayDeque

This is one of the most important modern Java comparisons.

| Feature | Stack | ArrayDeque |
|---|---|---|
| Type | Class | Class |
| Implements | List through Vector | Deque |
| Legacy | Yes | No |
| Introduced | Java 1.0 | Java 6 |
| Stack operations | Yes | Yes |
| Internal structure | Vector's dynamic array | Resizable array |
| Synchronization | Inherited synchronized behavior | Not synchronized |
| Null elements | Allowed | Not allowed |
| Modern recommendation | Usually avoid for new code | Preferred for stack use |

Modern stack:

    Deque<Integer> stack =
        new ArrayDeque<>();

Example:

    stack.push(10);
    stack.push(20);
    stack.push(30);

    System.out.println(
        stack.pop()
    );

Output:

    30

---

# 16. Stack vs Queue

Stack:

    LIFO

Queue:

    FIFO

Example Stack:

    push(A)
    push(B)
    push(C)

    pop()
        -> C

Example Queue:

    offer(A)
    offer(B)
    offer(C)

    poll()
        -> A

Comparison:

| Feature | Stack | Queue |
|---|---|---|
| Principle | LIFO | FIFO |
| Insert | Top | Rear |
| Remove | Top | Front |
| Example | Undo | Printer queue |
| Main operations | push/pop | offer/poll |

---

# 17. Stack vs ArrayList

Both are backed by Vector/array-style storage in their implementations, but their intended APIs differ.

| Feature | Stack | ArrayList |
|---|---|---|
| Purpose | LIFO | General List |
| Legacy | Yes | No |
| Indexed access | Yes | Yes |
| `push()` | Yes | No |
| `pop()` | Yes | No |
| `peek()` | Yes | No |
| Allows duplicates | Yes | Yes |
| Allows null | Yes | Yes |
| Modern stack choice | No | Not specifically |

A Stack is conceptually restricted to:

    top-based access

while ArrayList is a:

    general-purpose List

---

# 18. Stack vs LinkedList

`LinkedList` implements both:

    List
    Deque

Therefore it can also be used as a Stack.

Example:

    Deque<Integer> stack =
        new LinkedList<>();

    stack.push(10);
    stack.push(20);

    System.out.println(
        stack.pop()
    );

However, for a normal stack implementation, `ArrayDeque` is generally preferred over LinkedList because it avoids the node-based memory overhead of LinkedList.

---

# 19. Stack Overflow and Underflow

These are important DSA concepts.

## Stack Underflow

Underflow occurs when we attempt to remove an element from an empty Stack.

Example:

    Stack<Integer> stack =
        new Stack<>();

    stack.pop();

Result:

    EmptyStackException

Conceptually:

    Empty Stack
        |
        | pop()
        v
    Underflow

---

## Stack Overflow

In a fixed-capacity stack, overflow means attempting to push into a full stack.

Conceptually:

    Full Stack
        |
        | push()
        v
    Overflow

However, Java's `Stack` is dynamically resizable because it inherits Vector's dynamic-array behavior.

Therefore, ordinary `Stack.push()` does not normally produce a fixed-capacity "stack overflow" merely because the Stack reaches its current capacity; the underlying Vector can grow.

Do not confuse this with:

    StackOverflowError

which is a JVM error generally associated with excessive call-stack usage, such as uncontrolled recursion.

---

# 20. Time Complexity

For the main Stack operations:

    push()
        -> O(1) amortized

    pop()
        -> O(1)

    peek()
        -> O(1)

    empty()
        -> O(1)

    size()
        -> O(1)

    search()
        -> O(n)

Why is search O(n)?

Because the Stack may need to inspect multiple elements from the top.

---

## Push and Dynamic Growth

Normally:

    push()
        -> O(1)

But when the underlying array needs resizing:

    push()
        -> O(n)

for that particular operation because elements may need to be copied.

Therefore we say:

    O(1) amortized

---

# 21. Real-World Applications

Stacks are used in many systems.

## 1. Undo

Example:

    Type A
    Type B
    Type C

Undo:

    C
    B
    A

The latest action is undone first.

---

## 2. Browser History

Conceptually, recently visited pages can be managed using stack-like behavior.

---

## 3. Function Calls

The JVM uses a call stack for method execution.

Example:

    main()
       |
       v
    methodA()
       |
       v
    methodB()

When `methodB()` finishes:

    methodB()
        ↓
    removed first

This follows LIFO behavior.

---

## 4. Expression Evaluation

Stacks are heavily used for:

    Infix
    Prefix
    Postfix

expression processing.

---

## 5. Parentheses Matching

Example:

    ({[]})

A stack can keep track of opening brackets.

---

# 22. DSA Applications

Stack is extremely important in DSA.

Common problems include:

    Balanced Parentheses
    Next Greater Element
    Previous Greater Element
    Next Smaller Element
    Previous Smaller Element
    Stock Span
    Largest Rectangle in Histogram
    Min Stack
    Expression Evaluation
    Infix to Postfix
    Postfix Evaluation
    DFS
    Backtracking
    Monotonic Stack

---

# 23. Advantages

## 1. Simple LIFO API

The primary operations are easy to understand:

    push()
    pop()
    peek()

---

## 2. Dynamic Size

Because Stack extends Vector, it can grow dynamically.

---

## 3. O(1) Core Operations

Normally:

    push()
    pop()
    peek()

are O(1) amortized/constant-time operations.

---

## 4. Built-In Class

Java directly provides Stack.

---

## 5. Useful for Legacy Code

Existing Java applications may still use Stack.

---

# 24. Disadvantages

## 1. Legacy Design

Stack is an old class from Java 1.0.

---

## 2. Extends Vector

Because Stack extends Vector, it exposes many List operations that are not necessary for a pure Stack abstraction.

---

## 3. Synchronization Overhead

It inherits Vector's synchronization behavior.

---

## 4. Modern Alternative Exists

For most new stack implementations:

    Deque
        +
    ArrayDeque

is preferred.

---

## 5. Not an Ideal Abstraction

A Stack should conceptually allow operations at one end.

But because Stack inherits List operations from Vector, users can perform operations that bypass normal Stack semantics.

Example:

    stack.add(0, 100);

This is possible even though it is not normal Stack behavior.

---

# 25. Common Mistakes

## Mistake 1 — Thinking Stack Uses a Linked List

Java's `Stack` extends `Vector`.

Therefore its inherited storage is array-based.

---

## Mistake 2 — Thinking Stack Means JVM Stack

These are different concepts.

Java:

    java.util.Stack

is a collection class.

JVM:

    Thread Stack

is runtime memory used for method calls, local variables, and frames.

Do not confuse them.

---

## Mistake 3 — Thinking pop() Only Reads

Wrong.

`pop()`:

    returns
    +
    removes

the top element.

---

## Mistake 4 — Confusing peek() and pop()

    peek()
        -> returns top
        -> does NOT remove

    pop()
        -> returns top
        -> removes top

---

## Mistake 5 — Thinking search() Returns Index

`search()` returns a 1-based position from the top.

Example:

    Top
     |
     v
    C -> 1
    B -> 2
    A -> 3

It does not return the normal zero-based List index.

---

## Mistake 6 — Thinking empty() and isEmpty() Are Completely Different

Both can tell you whether the Stack is empty.

`empty()` is Stack's legacy method.

`isEmpty()` comes from the Collection/List hierarchy.

---

## Mistake 7 — Thinking Stack Overflow Means StackOverflowError

They are different.

DSA stack overflow:

    Data structure is full.

`StackOverflowError`:

    JVM thread call stack has been exhausted.

---

## Mistake 8 — Thinking Stack Is Recommended for New Projects

Usually not.

Modern Java code generally prefers:

    Deque
        +
    ArrayDeque

for stack behavior.

---

# 26. Interview Traps

## Trap 1

Question:

> What is the principle followed by Stack?

Answer:

    LIFO

---

## Trap 2

Question:

> Which class does Stack extend?

Answer:

    Vector

---

## Trap 3

Question:

> Is Stack a legacy class?

Answer:

Yes.

---

## Trap 4

Question:

> What is the modern alternative to Stack?

Answer:

    Deque

Usually with:

    ArrayDeque

---

## Trap 5

Question:

> What does push() do?

Answer:

Adds an element to the top.

---

## Trap 6

Question:

> What does pop() do?

Answer:

Returns and removes the top element.

---

## Trap 7

Question:

> What does peek() do?

Answer:

Returns the top element without removing it.

---

## Trap 8

Question:

> What does search() return?

Answer:

The 1-based position of the element from the top, or `-1` if absent.

---

## Trap 9

Question:

> What exception does pop() throw on an empty Stack?

Answer:

    EmptyStackException

---

## Trap 10

Question:

> Is Stack synchronized?

Answer:

Yes, because it extends Vector and inherits its synchronized legacy behavior.

---

## Trap 11

Question:

> Can Stack contain null?

Answer:

Yes.

---

## Trap 12

Question:

> Can Stack contain duplicates?

Answer:

Yes.

---

## Trap 13

Question:

> Is Stack the same thing as JVM stack memory?

Answer:

No.

`java.util.Stack` is a collection class, while JVM stack memory is part of the runtime execution model.

---

## Trap 14

Question:

> Why is ArrayDeque preferred over Stack?

Answer:

ArrayDeque is a modern Deque implementation designed for efficient double-ended queue and stack operations without the legacy Vector synchronization model.

---

## Trap 15

Question:

> Does Stack implement Deque?

Answer:

No.

Stack extends Vector.

---

## Trap 16

Question:

> Is Stack FIFO?

Answer:

No.

Stack is LIFO.

---

## Trap 17

Question:

> Is Queue LIFO?

Answer:

No.

A normal Queue follows FIFO.

---

## Trap 18

Question:

> Is search() zero-based?

Answer:

No.

It returns a 1-based position from the top.

---

# 27. Practical Examples

## Example 1 — Basic Stack

    import java.util.Stack;

    public class Main {

        public static void main(String[] args) {

            Stack<Integer> stack =
                new Stack<>();

            stack.push(10);
            stack.push(20);
            stack.push(30);

            System.out.println(stack);
        }
    }

Output:

    [10, 20, 30]

---

## Example 2 — push() and pop()

    import java.util.Stack;

    public class Main {

        public static void main(String[] args) {

            Stack<Integer> stack =
                new Stack<>();

            stack.push(10);
            stack.push(20);
            stack.push(30);

            System.out.println(
                stack.pop()
            );

            System.out.println(stack);
        }
    }

Output:

    30
    [10, 20]

---

## Example 3 — peek()

    import java.util.Stack;

    public class Main {

        public static void main(String[] args) {

            Stack<String> stack =
                new Stack<>();

            stack.push("Java");
            stack.push("Spring");
            stack.push("React");

            System.out.println(
                stack.peek()
            );

            System.out.println(stack);
        }
    }

Output:

    React
    [Java, Spring, React]

Notice:

    peek()
        -> did not remove React

---

## Example 4 — empty()

    import java.util.Stack;

    public class Main {

        public static void main(String[] args) {

            Stack<Integer> stack =
                new Stack<>();

            System.out.println(
                stack.empty()
            );

            stack.push(10);

            System.out.println(
                stack.empty()
            );
        }
    }

Output:

    true
    false

---

## Example 5 — search()

    import java.util.Stack;

    public class Main {

        public static void main(String[] args) {

            Stack<String> stack =
                new Stack<>();

            stack.push("A");
            stack.push("B");
            stack.push("C");

            System.out.println(
                stack.search("C")
            );

            System.out.println(
                stack.search("A")
            );

            System.out.println(
                stack.search("X")
            );
        }
    }

Output:

    1
    3
    -1

---

## Example 6 — EmptyStackException

    import java.util.Stack;

    public class Main {

        public static void main(String[] args) {

            Stack<Integer> stack =
                new Stack<>();

            System.out.println(
                stack.pop()
            );
        }
    }

Result:

    java.util.EmptyStackException

---

## Example 7 — Parentheses Matching

    import java.util.Stack;

    public class Main {

        public static boolean isBalanced(
            String expression
        ) {

            Stack<Character> stack =
                new Stack<>();

            for (
                char ch : expression.toCharArray()
            ) {

                if (
                    ch == '(' ||
                    ch == '{' ||
                    ch == '['
                ) {

                    stack.push(ch);

                } else if (
                    ch == ')' ||
                    ch == '}' ||
                    ch == ']'
                ) {

                    if (stack.empty()) {
                        return false;
                    }

                    char top =
                        stack.pop();

                    if (
                        (ch == ')' && top != '(') ||
                        (ch == '}' && top != '{') ||
                        (ch == ']' && top != '[')
                    ) {

                        return false;
                    }
                }
            }

            return stack.empty();
        }

        public static void main(String[] args) {

            System.out.println(
                isBalanced("({[]})")
            );

            System.out.println(
                isBalanced("({[})")
            );
        }
    }

Output:

    true
    false

---

## Example 8 — Modern Stack Using ArrayDeque

    import java.util.ArrayDeque;
    import java.util.Deque;

    public class Main {

        public static void main(String[] args) {

            Deque<Integer> stack =
                new ArrayDeque<>();

            stack.push(10);
            stack.push(20);
            stack.push(30);

            System.out.println(
                stack.peek()
            );

            System.out.println(
                stack.pop()
            );

            System.out.println(stack);
        }
    }

Output:

    30
    30
    [20, 10]

This is the preferred style for many modern Java applications.

---

# 28. Top Interview Questions

## Q1. What is Stack in Java?

Stack is a legacy class in `java.util` that provides LIFO operations.

---

## Q2. What principle does Stack follow?

    LIFO

Last In, First Out.

---

## Q3. Which class does Stack extend?

    Vector

---

## Q4. Is Stack a legacy class?

Yes.

It was introduced in Java 1.0.

---

## Q5. What are the main Stack methods?

    push()
    pop()
    peek()
    empty()
    search()

---

## Q6. Difference between push() and pop()?

    push()
        -> adds to top

    pop()
        -> removes and returns top

---

## Q7. Difference between pop() and peek()?

    pop()
        -> returns + removes

    peek()
        -> returns only

---

## Q8. What happens when pop() is called on an empty Stack?

It throws:

    EmptyStackException

---

## Q9. What does search() return?

A 1-based position from the top.

If not found:

    -1

---

## Q10. Is Stack synchronized?

Yes, because Stack extends Vector.

---

## Q11. Is Stack recommended in modern Java?

Usually no.

Use:

    Deque
        +
    ArrayDeque

for typical stack behavior.

---

## Q12. Why is Stack considered poorly designed?

Because it extends Vector and therefore inherits general List operations that are not necessary for a strict Stack abstraction.

---

## Q13. Can Stack store null?

Yes.

---

## Q14. Can Stack contain duplicate elements?

Yes.

---

## Q15. Does Stack implement Deque?

No.

---

## Q16. What is the time complexity of push()?

O(1) amortized.

A resize can make an individual operation O(n).

---

## Q17. What is the time complexity of pop()?

O(1).

---

## Q18. What is the time complexity of peek()?

O(1).

---

## Q19. What is the time complexity of search()?

O(n).

---

## Q20. What is Stack underflow?

Attempting to remove an element from an empty Stack.

---

## Q21. What is Stack overflow?

In the DSA sense, it means pushing into a full fixed-capacity stack.

Java's `Stack` dynamically grows, so reaching its current capacity does not normally cause this kind of overflow.

---

## Q22. Is Stack overflow the same as StackOverflowError?

No.

`StackOverflowError` generally occurs when a thread's JVM call stack is exhausted.

---

## Q23. Can we use ArrayDeque as a Stack?

Yes.

Example:

    Deque<Integer> stack =
        new ArrayDeque<>();

---

## Q24. Why is ArrayDeque generally preferred?

It is a modern Deque implementation designed for efficient insertion and removal at both ends and avoids the legacy synchronization model of Stack.

---

## Q25. Does ArrayDeque allow null?

No.

This is an important difference from Stack.

---

## Q26. Can Stack be accessed using indexes?

Yes.

Because Stack extends Vector and therefore inherits List operations.

Example:

    stack.get(0);

But indexed access is not the normal abstraction of a Stack.

---

## Q27. What happens internally during push()?

The element is added to the end of the underlying Vector storage.

---

## Q28. Where is the top of Stack represented?

Conceptually, at the end of the underlying Vector.

---

## Q29. What is the difference between Stack and Queue?

    Stack
        -> LIFO

    Queue
        -> FIFO

---

## Q30. What is the difference between Stack and JVM Stack?

    java.util.Stack
        -> Collection class

    JVM Stack
        -> Runtime memory structure for method execution

---

# 29. 30-Second Interview Answer

If the interviewer asks:

> "What is Stack in Java?"

Answer:

> "`Stack` is a legacy class from `java.util` that extends `Vector` and represents a LIFO data structure. Its main operations are `push()`, `pop()`, `peek()`, `empty()`, and `search()`. `push()` adds to the top, `pop()` removes and returns the top, while `peek()` only returns it. Since Stack extends Vector, it inherits synchronized behavior and List operations. For modern Java code, `Deque` with `ArrayDeque` is generally preferred for stack functionality."

---

# 30. Cheat Sheet

## Core Concept

    Stack
       |
       v
    LIFO

    Last In
       ↓
    First Out

---

## Main Methods

    push()
        -> Add to top

    pop()
        -> Remove + return top

    peek()
        -> Return top

    empty()
        -> Check empty

    search()
        -> Position from top

---

## Hierarchy

    Object
       |
    AbstractCollection
       |
    AbstractList
       |
    Vector
       |
    Stack

---

## Key Facts

    Java version:
        1.0

    Package:
        java.util

    Parent:
        Vector

    Type:
        Class

    Design:
        Legacy

    Principle:
        LIFO

    Allows duplicates:
        Yes

    Allows null:
        Yes

    Synchronized:
        Yes

    Modern alternative:
        Deque + ArrayDeque

---

## Complexity

    push()
        -> O(1) amortized

    pop()
        -> O(1)

    peek()
        -> O(1)

    empty()
        -> O(1)

    search()
        -> O(n)

---

## Exceptions

    pop() on empty Stack
        -> EmptyStackException

---

# 31. Quick Revision

Remember:

    Stack = LIFO

    push()
        -> Put

    pop()
        -> Pull Out

    peek()
        -> Look

---

Important hierarchy:

    Stack
      |
      v
    Vector

Therefore:

    Stack inherits Vector behavior.

---

Most important interview facts:

    1. Stack is a class.

    2. Stack belongs to java.util.

    3. Stack was introduced in Java 1.0.

    4. Stack is a legacy class.

    5. Stack extends Vector.

    6. Stack follows LIFO.

    7. push() adds an element.

    8. pop() removes and returns the top.

    9. peek() returns the top without removing it.

    10. empty() checks whether the Stack is empty.

    11. search() returns a 1-based position from the top.

    12. search() returns -1 when the element is absent.

    13. pop() on an empty Stack throws EmptyStackException.

    14. Stack allows duplicates.

    15. Stack allows null.

    16. Stack inherits Vector's synchronized behavior.

    17. Stack also inherits List operations.

    18. Stack uses Vector's array-based storage.

    19. push() is O(1) amortized.

    20. pop() is O(1).

    21. peek() is O(1).

    22. search() is O(n).

    23. Stack is not the same as JVM stack memory.

    24. Stack is generally not preferred for new Java code.

    25. Deque + ArrayDeque is generally preferred.

---

# Final Memory Trick

Think of a stack of plates:

    Add plate
       ↓
      push

    ┌───────┐
    │   30  │ ← TOP
    ├───────┤
    │   20  │
    ├───────┤
    │   10  │
    └───────┘

    Remove plate
       ↓
       pop

The last plate placed on top is the first plate removed.

Therefore:

    STACK = LIFO

And for modern Java:

    Stack              → Legacy
    Deque + ArrayDeque → Preferred