
String s2 = s1.intern();

System.out.println(s1 == s2);

Output:

false

Why?

s1 still refers to the separately created object.

intern() does not change s1.

It returns the pooled reference.

Correct mental model:

String s2 = s1.intern();

means:

"Give me the pooled/canonical reference for this content."

🧠 Pool and Heap

A common older explanation says:

String Pool → PermGen

That is outdated for modern HotSpot JVMs.

The String Pool is associated with the JVM's heap in modern HotSpot implementations; since Java 7, interned Strings were moved out of PermGen.

Interview-safe answer

The String Pool is maintained by the JVM, and in modern HotSpot JVMs the interned String objects are stored in the heap.

Do not say:

"String Pool is stored in stack."

❌ Wrong.

The reference variable may be on a stack frame, but the String object is not a stack object.

➕ Compile-Time String Concatenation

Consider:

String s1 = "Java" + "World";
String s2 = "JavaWorld";

System.out.println(s1 == s2);

Output:

true

Why?

Both can resolve to the same pooled String because the concatenation consists entirely of compile-time constants.

Conceptually:

"Java" + "World"
       ↓
"JavaWorld"
       ↓
String Pool

⚡ Runtime String Concatenation

Now:

String a = "Java";
String b = "World";

String s1 = a + b;
String s2 = "JavaWorld";

System.out.println(s1 == s2);

Do not assume true.

The expression:

a + b

is evaluated at runtime because a and b are ordinary variables.

Modern Java compilers/JVMs may use different string-concatenation machinery internally, but the resulting String should not be assumed to be the same reference as the pooled literal.

If you need the pooled representation:

String s1 = (a + b).intern();

🔒 String Pool and Immutability

String Pool and immutability are strongly related.

Suppose:

String s1 = "Java";
String s2 = "Java";

Both may share the same object:

s1 ──┐
     ├──→ "Java"
s2 ──┘

Because String is immutable:

s1 = "Python";

does not change the "Java" object.

Instead, s1 is made to refer to another String.

s1 ─────→ "Python"

s2 ─────→ "Java"

This makes pooled sharing safe.

🔐 String Pool and final Variables

Consider:

final String a = "Java";
final String b = "World";

String s1 = a + b;
String s2 = "JavaWorld";

System.out.println(s1 == s2);

Because a and b are compile-time constants, the concatenation can be folded at compile time.

Conceptually:

a + b
 ↓
"JavaWorld"
 ↓
Pool

Therefore s1 and s2 can refer to the same pooled String.

Compare with:

String a = "Java";
String b = "World";

String s1 = a + b;

Here a and b are not compile-time constants merely because they contain literals.

🧪 Important intern() Examples

Example 1

String s1 = "Java";
String s2 = new String("Java");

System.out.println(s1 == s2);

Output:

false

Example 2

String s1 = "Java";
String s2 = new String("Java").intern();

System.out.println(s1 == s2);

Output:

true

Example 3

String s1 = new String("Java");

s1.intern();

System.out.println(s1 == "Java");

Output:

false

Why?

The returned pooled reference was ignored.

Correct:

s1 = s1.intern();

if you want s1 itself to point to the pooled representation.

⚙️ JVM / Internal Working

For:

String s1 = "Java";
String s2 = "Java";

a simplified conceptual process is:

Class loading / runtime preparation
          ↓
String literal "Java" is recognized
          ↓
JVM uses the String Pool
          ↓
If equivalent pooled String exists
          ↓
Reuse it
          ↓
s1 and s2 can reference the same object

For:

String s = new String("Java");

conceptually:

"Java"
  ↓
pooled String representation

new String(...)
  ↓
new separate String object

The exact JVM implementation and optimization details should not be reduced to a simple "stack vs heap" diagram.

⚠️ Common Mistakes

Mistake 1: == compares String content

❌ Wrong.

==

compares reference identity.

Use:

equals()

for content equality.

Mistake 2: Every String is automatically pooled

❌ Not every String object is automatically shared as a pooled object.

For example:

new String("Java")

creates a distinct String object.

Mistake 3: intern() modifies the original String

❌ No.

s.intern();

returns a pooled reference.

It does not change the reference stored in s.

Mistake 4: String Pool is in Stack

❌ Wrong.

The String object is not stored in the stack simply because you wrote:

String s = "Java";

The local reference may be associated with a stack frame, while the object is on the heap in modern HotSpot implementations.

Mistake 5: String Pool is PermGen

❌ Outdated for modern Java.

Since Java 7, interned Strings are associated with the heap rather than PermGen in HotSpot.

🚨 Interview Traps

Trap 1

String a = "Java";
String b = "Java";

System.out.println(a == b);

Answer:

true

because the literals can share the pooled object.

Trap 2

String a = new String("Java");
String b = new String("Java");

System.out.println(a == b);

Answer:

false

Each new creates a distinct String object.

Trap 3

String a = new String("Java");
String b = a.intern();

System.out.println(a == b);

Usually:

false

because a is the separately created object and b is the pooled reference.

Trap 4

String a = "Java" + "World";
String b = "JavaWorld";

System.out.println(a == b);

Answer:

true

because the expression is made from compile-time constants and can be folded into the same pooled literal.

🔥 Top 15 Interview Questions

1. What is the String Pool?

A JVM-managed pool that enables sharing of String objects for String literals and explicitly interned Strings.

2. Why does Java use a String Pool?

To avoid unnecessary duplicate String objects and enable safe sharing of immutable Strings.

3. Where is the String Pool stored?

In modern HotSpot JVMs, interned Strings are stored on the heap.

4. What is the difference between == and equals()?

==       → reference identity
equals() → String content equality

5. Why does this return true?

String a = "Java";
String b = "Java";

a == b

Because both literals can refer to the same pooled String object.

6. Why does this return false?

String a = new String("Java");
String b = new String("Java");

a == b

Each new expression creates a distinct String object.

7. What does intern() do?

It returns the canonical pooled representation of the String.

8. Does intern() modify the original String?

No. It returns a reference. The returned reference must be captured if needed.

9. How many objects can new String("Java") create?

If "Java" is not already in the pool, the expression can involve a pooled String for the literal plus a separate String object created by new. If the pooled literal already exists, only the new object needs to be created by that expression.

10. Why is String immutability important for the String Pool?

Because multiple references can safely share the same String object without one reference changing the value seen by another.

11. Does every String literal create a new object?

No. Equal literals can reuse the same pooled String object.

12. What happens with "A" + "B"?

When both are compile-time constants, the compiler can fold them into "AB", allowing pooled sharing.

13. What happens with runtime concatenation?

A runtime concatenation produces a resulting String through Java's concatenation machinery; you should not assume that the result has the same reference as an existing pooled literal.

14. Can a String created with new be added to the String Pool?

Yes, through intern().

String s = new String("Java");
String pooled = s.intern();

15. Why is == discouraged for String content comparison?

Because it checks whether two references identify the same object, not whether their character sequences are equal.

Use:

a.equals(b)

for content comparison.

🎤 30-Second Interview Answer

The String Pool is a JVM-managed pool used to share eligible String objects, especially String literals and interned Strings. When two equal String literals are used, they can refer to the same pooled object, which saves memory. This sharing is safe because String objects are immutable. == compares references, while equals() compares String content. A String created with new is a separate object, but intern() can return the canonical pooled representation.

🧾 Cheat Sheet

╔══════════════════════════════════════════════╗
║              STRING POOL CHEAT SHEET         ║
╠══════════════════════════════════════════════╣
║ String literal → Can use String Pool         ║
║ new String()   → Creates separate object     ║
║ ==             → Reference identity         ║
║ equals()       → Content equality            ║
║ intern()       → Returns pooled reference    ║
║ String         → Immutable                   ║
║ Pool           → JVM-managed                 ║
║ Modern HotSpot → Heap                        ║
║ "A" + "B"      → Compile-time constant       ║
║ a + b          → Runtime if variables aren't ║
║                  compile-time constants       ║
╚══════════════════════════════════════════════╝

🧠 Most Important Diagram

                 STRING CREATION
                       │
          ┌────────────┴────────────┐
          │                         │
     String literal             new String()
          │                         │
          ▼                         ▼
     String Pool               New Object
          │
          │
    ┌─────┴─────┐
    │           │
   s1           s2
    │           │
    └─────┬─────┘
          ▼
      Same object
          │
       s1 == s2
          │
         true

🔗 Connection With Next Topic

02-String-Pool.md
        ↓
03-String-Immutability.md

The next topic answers the deeper question:

Why exactly did Java make String immutable, and what happens internally when we try to modify a String?

That topic should cover object replacement, references, security, caching, hash codes, String Pool safety, and interview-level reasoning.

🚀 Final Revision

Remember these five lines:

1. String literals can be shared through the String Pool.
2. String is immutable, making sharing safe.
3. == compares references.
4. equals() compares String content.
5. intern() returns the canonical pooled representation.

Golden interview concept: String Pool + immutability + reference comparison are tightly connected. Understanding these three concepts makes most String interview questions much easier.