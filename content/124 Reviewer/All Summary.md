# CMSC 124 — Summary of All Lectures
### Lectures 8 · 9 (9A + 9B) · 10A · 10B · 11

> **How to use this file**
> - **A. Topics** — outline only: topics and their classifications, with no explanations. Use it as a recall checklist.
> - **B. Summary** — short explanation and description of every topic.
> - **C. Remember!** — key facts, trace results, tables, and traps.

| Lecture | Title |
|---|---|
| **8** | Semantic Issues of Variables: Variable Attributes and Bindings |
| **9** | Scoping of Variables: Static Scoping (9A) and Dynamic Scoping (9B) |
| **10A** | Data Types: Primitives and Character String Types |
| **10B** | Data Types: Array Types |
| **11** | Expressions (and Assignment Statements) |

---

# A. TOPICS

## Lecture 8 — Semantic Issues of Variables

**Variable attributes (6)**
1. Name
2. Address
   - Aliases
3. Type
4. Value
   - L-value
   - R-value
5. Lifetime
6. Scope

**Lifetime stages (5):** Allocate → Bind → Use → Unbind → Deallocate

**Bindings**
- Binding time: compile time · link time · load/run time
- Type bindings: **Static** binding · **Dynamic** binding

---

## Lecture 9 — Scoping of Variables

**Static scoping (lexical scope)**
1. Variable hiding (shadowing) and qualified names (`main.x` in Ada, `::x` in C++)
2. Blocks
3. Evaluation of static scoping
   - Advantages
   - Problems

**Dynamic scoping**
1. Activation
2. Dynamic parent
3. Dynamic ancestor
4. Perl example (`our` · `my` · `local`)

---

## Lecture 10A — Data Types: Primitives and Character Strings

**Why have data types? (3)**
1. Better error detection
2. Better modularization
3. Better documentation

**Categories of data type (3):** Primitive · Character string · Array

**Primitive data types (3)**
1. Numeric: Integer · Floating-point (float, double) · Complex · Decimal
2. Boolean
3. Character

**Character string design considerations (3)**
1. Implementation: array of char · primitive (built-in) type
2. String length: static · limited dynamic · dynamic
3. Operations: assignment · concatenation · substring · lexical comparison · pattern matching

---

## Lecture 10B — Data Types: Array Types

**Strings vs. arrays:** primitive strings · array-of-char strings

**Design issues of array types (7)**
1. Subscript types
2. Range checking
3. Binding of subscript type and range
4. Storage binding
   - Static array
   - Fixed stack-dynamic array
   - Stack-dynamic array
   - Fixed heap-dynamic array
   - Heap-dynamic array
5. Initialization
6. Row length: rectangular · ragged
   - Memory mapping: row-major · column-major
7. Array slices

---

## Lecture 11 — Expressions

**Kinds of expressions (by operands):** Unary · Binary · Ternary

**Operator categories:** Arithmetic · Relational · Logical · Bitwise · Assignment

**Design issues of expressions (4)**
1. Operator evaluation order
2. Operand evaluation order
3. Operator overloading
4. Type conversions
   - Widening
   - Narrowing
   - Implicit (coercion)
   - Explicit (typecast)

**Short-circuit evaluation**

**Assignment statements (5)**
1. Simple assignment
2. Conditional targets
3. Compound assignment
4. Unary assignment
5. Multiple assignments

---

# B. SUMMARY

## Lecture 8 — Semantic Issues of Variables

A variable is described by **six attributes**: name, address, type, value, lifetime, and scope.

| Attribute | Short explanation |
|---|---|
| **Name** | The identifier. Some variables have none (compiler temporaries, anonymous objects). |
| **Address** | The memory location tied to the variable *at the current time*. It can change (e.g. each recursive call gets a new one). |
| **Alias** | Two or more identifiers referring to the same address; a write through one is seen by the others. |
| **Type** | The set of allowed values, the legal operations, and sometimes the representation size/format. |
| **Value** | The bits stored in the memory cell(s). |
| **L-value / R-value** | L-value = the location you write to. R-value = the data you read. |
| **Lifetime** | The time the variable is bound to a specific memory location (allocate → bind → use → unbind → deallocate). |
| **Scope** | The region of source code where the name is visible. |

**Bindings:** a binding is an association between an attribute and an entity. *Binding time* is when that association is fixed. Examples: `count` → `int` at compile time; a `printf` call → library code at link time; `x` → a storage cell at load/run time.

**Type binding:** *static* binding happens before run time and never changes (Java). *Dynamic* binding happens at run time and can change (Python).

---

## Lecture 9 — Scoping of Variables

**Scope** is the range of statements where a variable is visible and can be referenced or modified.

**Static (lexical) scoping**
- Scope is decided at **compile time** from the program text. Introduced in ALGOL 60 and used by C, C++, Java, Python, and Pascal.
- Inner blocks can access outer variables **unless shadowed**.
- **Variable hiding:** a nested declaration with the same name hides the outer one. The hidden variable can be reached with a qualified name (`main.x` in Ada, `::x` in C++).
- **Blocks** introduce a new scope. Their variables are created on entry and destroyed on exit.
- **Pros:** clear visibility rules, efficient to implement.
- **Cons:** too much access (procedures see variables they shouldn't), errors may appear only at run time, poor encapsulation, hard to restrict access, awkward structure, excessive globals.

**Dynamic scoping**
- Scope is decided at **run time** by the **sequence of calls**.
- An **activation** is one call of a function; each activation has its own local environment (key for recursion).
- **Dynamic parent** = the subprogram that called the current one. **Dynamic ancestors** = all calls that led to the current one.
- In the Perl example, `our` creates a global, `my` is lexical (private), and `local` temporarily replaces the global for the duration of the call chain, which is dynamic.

---

## Lecture 10A — Primitives and Character Strings

**Data types** tell the code what kind of information it is handling and which operations make sense. Their benefits are better error detection, modularization, and documentation.

**Primitive types** are the basic building blocks, not defined in terms of other types.

| Type | Description |
|---|---|
| **Integer** | Whole numbers. Finite storage means overflow is possible (detected, wraparound, or undefined behavior for signed ints in C). |
| **Float / Double** | Real numbers with decimals; double has double precision. |
| **Complex** | Real + imaginary part (`2 + 3j` in Python). |
| **Decimal** | Exact decimal representation, used in financial apps. |
| **Boolean** | Only `true` or `false`. |
| **Character** | A single character, stored as a numeric code (ASCII/Unicode). |

**Character strings** raise three design questions:
1. **Implementation:** an array of char (C) or a primitive built-in type (Python).
2. **Length:** *static* (fixed), *limited dynamic* (varies up to a max), or *dynamic* (no limit, but extra overhead).
3. **Operations:** assignment, concatenation, substring, lexical comparison, pattern matching (regex).

---

## Lecture 10B — Array Types

- An **array** is a homogeneous collection where each element is identified by its position (index/subscript).
- Strings can be **primitive** (more convenient, many built-ins, but a more complex implementation) or an **array of char** (simpler, closer to memory, but more manual and error-prone). In C, a string ends with the null terminator `'\0'`.

**Design issues**

| Issue | Summary |
|---|---|
| **Subscript types** | Which values are valid indices. C needs ints; JS allows `arr[5.0]` and gives `undefined` for `arr[5.26]`. |
| **Range checking** | Does the language check bounds? Languages that check are more reliable. C doesn't; Python does and allows negative indices. |
| **Binding of subscript type/range** | Subscript type is usually bound statically; the valid range is usually bound dynamically. |
| **Storage binding** | When the size is known and where the array lives (see table below). |
| **Initialization** | Giving initial values, e.g. `{1,2,3}` or `[1,2,3]`. |
| **Row length** | Rectangular arrays have equal-length rows; ragged arrays have rows of different lengths. Memory mapping is row-major or column-major. |
| **Array slices** | Whether sub-parts of an array can be taken (Python `arr[1:3]`). |

**Storage binding categories**

| Category | Range bound | Storage allocated | Example |
|---|---|---|---|
| Static | Statically | Before run time | `static int a[5]` |
| Fixed stack-dynamic | Statically | When the declaration is elaborated | `int arr[30];` |
| Stack-dynamic | Dynamically | Dynamically, then fixed once elaborated | `int arr[size];` |
| Fixed heap-dynamic | When requested at run time | Heap, then fixed | `malloc(...)` |
| Heap-dynamic | Can grow and shrink | Heap | Python list with `append` |

---

## Lecture 11 — Expressions

**Expressions** are the means of specifying computations. They can be unary (one operand), binary (two), or ternary (three). Operators are arithmetic, relational, logical, bitwise, or assignment.

**Four design issues**
1. **Operator evaluation order:** governed by precedence, associativity, and parentheses; side effects must be considered.
2. **Operand evaluation order:** the order in which operands are evaluated. A *side effect* (a function changing a variable used later in the same expression) can change the result. Solutions: disallow side-effect functions in expressions, or fix an evaluation order.
3. **Operator overloading:** one operator performs different operations depending on operand types (`+` on ints, floats, strings, lists). Whether programmers may define their own is a design question.
4. **Type conversions:** four kinds (table below).

| Conversion | Description | Java example |
|---|---|---|
| **Widening** | To a type with a wider range | `double y = x;` (x is int) |
| **Narrowing** | To a smaller range or less precision; may lose information | `int y = (int) x;` (x is 1.43 → 1) |
| **Implicit (coercion)** | Done automatically by the compiler | `double r = x + y;` → `7.0` |
| **Explicit (typecast)** | Done by the programmer | `(double) x / y` → `2.5` |

**Short-circuit evaluation** skips the rest of a logical expression once its result is known (for example, it avoids dividing by zero in `divisor != 0 && 10/divisor`).

**Assignment statements** dynamically change the binding of values to variables.

| Form | Example |
|---|---|
| Simple | `number = 12` |
| Conditional targets | `($x ? $a : $b) = 0;` (Perl) |
| Compound | `count += 124`, `n *= 2;` |
| Unary | `count++`, `--$number;` |
| Multiple | `a, b = "marine", "biology"` (Python tuple unpacking) |

In some languages (C), an assignment returns the assigned value, which makes `while ((ch = fgetc(fp)) != EOF)` work.

---

# C. REMEMBER!

## Lecture 8
- ⭐ **Six attributes:** Name, Address, Type, Value, Lifetime, Scope.
- Same name does **not** mean same variable: the global `sum` and a local `sum` have **different addresses**.
- **Alias example:** `int a = 5; int *b = &a; *b = 9;` → prints `a = 9, *b = 9`.
- In `x = y + 1;`, `x` needs its **l-value** and `y` contributes its **r-value**.
- **Lifetime order:** Allocate → Bind → Use → Unbind → Deallocate.
- **Scope vs. lifetime:** scope is *where in the code* the name is visible; lifetime is *when during execution* it has storage.
- **Binding times:** `count` → `int` = compile time · `printf` → library = **link** time · `x` → storage = load/run time.
- **Java** = static type binding (`x = "hello"` on an `int` is an error). **Python** = dynamic type binding (allowed).
- Recursion: one address for the local variable **per activation**.

## Lecture 9
- ⭐ **Static = compile time, program text. Dynamic = run time, call sequence.**
- Static scoping was introduced in **ALGOL 60**. Pascal and C are static.
- **Trace (shadowing example):** `printf(x); demo(); printf(x);` prints `10`, `20`, `10`.
- A variable declared in an `if` block cannot be used after it (`printf(c)` outside is an error).
- **Dynamic parent** = the direct caller. **Dynamic ancestors** = **all** calls leading to the current one.
- **Perl trace:** `a()` prints `20` (because `my $x = 30` is private); `b()` prints `25` (because `local $x = 25` is visible to callees).
- **Same program, two scoping rules:**

  ```c
  int x = 1;
  void f(){ x = 2; }
  void g(){ int x = 3; f(); printf("%d ", x); }
  int main(){ g(); printf("%d", x); }
  ```
  Static → `3 2`. Dynamic → `2 1`.

- ⚠️ **Slide nuance:** the lecture says a Python block variable disappears after the block. In real Python, scope is per function, not per block. Answer exam items using the lecture's framing, but know the difference.

## Lecture 10A
- ⭐ A data type is **not just a label**. It decides which operations make sense.
- **Three benefits:** error detection, modularization, documentation.
- `x = "5"; print(x + 2)` → **TypeError** in Python.
- **Signed integer overflow in C = undefined behavior.** Other languages may detect it or wrap around.
- **Decimal** = exact (money). **Float/double** do **not** guarantee exact decimal representation.
- A dynamically typed language **still performs type checking** (at run time). It just doesn't require declared types.
- A program can pass type checking and still be **logically wrong**.
- `char str[30]` holding strings of varying length (up to 30) = **limited dynamic**. `char name[20] = "Erika"` = **static**. Perl strings = **dynamic**.
- Dynamic strings are not "free": resizing has **overhead**.
- A C char array needs **one extra slot** for the null terminator.
- Lexical comparison: `"abc" < "bcd"` → `True`.

## Lecture 10B
- ⭐ **Fixed heap-dynamic ≠ heap-dynamic:** `malloc` arrays are fixed once allocated; only heap-dynamic arrays (e.g. Python lists) can grow.
- `char charArr[5] = "ABCD"` → last slot (`charArr[4]`) is `'\0'`.
- `int intArr[5] = {1,2,4}` → `1 2 4 0 0`.
- `arr = [1,2,3,4,5]`: `arr[5]` → **IndexError**; `arr[-4]` → `2` (valid).
- In C, `arr[5]` on `int arr[5]` is out of bounds but **not checked**.
- **Static local array example:** `static int x[3] = {5,2,1}; x[0]++;` → calls print `6`, then `7`, then `8`.
- **Subscript type** is usually bound **statically**; **range** usually **dynamically**.
- Static arrays: efficient, no deallocation errors, but **less flexible**.
- Fixed stack-dynamic: subprograms can **share space** when not in use.
- Rectangular = same row lengths. Ragged = different row lengths. Row-major = stored **by rows**; column-major = stored **by columns**.
- Primitive strings: Java, Rust, PHP, Python, Go, Swift. Array-of-char strings: C, Haskell, C++, C# (as grouped in the slide).
- ⚠️ **Slide nuance:** the `int arr[size];` snippet is labeled "Fixed Stack-Dynamic" on the slide, but by definition (size bound dynamically, fixed once elaborated) it is **stack-dynamic**.

## Lecture 11
- ⭐ **Four design issues:** operator evaluation order · operand evaluation order · operator overloading · type conversions.
- **Parentheses control grouping, not the execution order of every sub-expression.** In C, `4 / 2` may run before `2 + 3`.
- `a = 8 + (c = 2 + 3) - 4 / 2` → **a = 11, c = 5**.
- **Side-effect traces:**
  - `arr = [1,2]; x = arr.pop() + len(arr)` → **3**
  - `int y = change_x() && (x == 10);` → **y = 1** (and `x` becomes 10)
- **Python `+` overloading:** `7` · `3.7` · `MamaBomba` · `[1, 2, 3, 4]`.
- C's `&` is overloaded: **binary** = bitwise AND, **unary** = address-of.
- **Conversions:**
  - `int` → `double` = widening
  - `(int) 1.43` = narrowing → `1`
  - `x + y` into a `double` = implicit → `7.0`
  - `(double) x / y` = explicit → `2.5`
  - `(double)(x / y)` = `2.0`, because the integer division happens first
- **Short circuit:** `divisor != 0 && 10 / divisor` is safe because the division is skipped when `divisor` is 0.
- **Assignment returns a value in C**, so `while ((ch = fgetc(fp)) != EOF)` works.
- ⚠️ **Slide typos:** `(int) y` should be `(int) x`; "ternary" is shown as "multiple operands" (ternary means three).

---

## Cross-Lecture Quick Comparison

| Concept | Option 1 | Option 2 |
|---|---|---|
| Type binding | **Static** — before run time, fixed (Java) | **Dynamic** — at run time, can change (Python) |
| Scoping | **Static** — compile time, program text | **Dynamic** — run time, call sequence |
| Strings | **Primitive** — convenient, complex implementation | **Array of char** — simple, manual, error-prone |
| Array row length | **Rectangular** — equal rows | **Ragged** — unequal rows |
| Array order in memory | **Row-major** — by rows | **Column-major** — by columns |
| Type conversion | **Implicit** — compiler does it (coercion) | **Explicit** — programmer casts |
| Conversion direction | **Widening** — safe, wider type | **Narrowing** — may lose information |
| Address vs. value | **L-value** — location to write | **R-value** — data to read |
