# CMSC 124 — Lecture 9 Reviewer (9A + 9B)
### Scoping of Variables: Static Scoping & Dynamic Scoping

> **Contents**
> - [A. Summarized Lecture](#a-summarized-lecture)
> - [B. Comprehensive Lecture](#b-comprehensive-lecture)
> - [C. Application Quiz (15 items)](#c-application-quiz)

---

## A. Summarized Lecture

- **Scope** = the range of statements where a variable is visible and can be referenced/modified.
- **Static (lexical) scoping**: scope is decided at **compile time** by the program's *text structure*. Used by C, C++, Java, Python, Pascal. Introduced in **ALGOL 60**.
  - Inner blocks see outer variables **unless shadowed** (variable hiding).
  - Hidden outer variables can be reached with a qualified name (`main.x` in Ada, `::x` in C++).
  - A **block** creates a new scope; its variables are created on entry and destroyed on exit.
  - **Pros:** clear visibility rules, efficient. **Cons:** over-exposure of data, poor encapsulation, excessive globals, awkward program structure.
- **Dynamic scoping**: scope is decided at **run time** by the *sequence of calls*.
  - **Dynamic parent** = the subprogram that called the current one.
  - **Dynamic ancestors** = all calls that led to the current subprogram.
  - Perl example: `my` (lexical) vs. `local` (dynamic) → the output is `20` then `25`.
- Each function call is an **activation**; each activation has its own local environment (important for recursion).

---

## B. Comprehensive Lecture

# PART 1 — STATIC SCOPING (Lecture 9A)

### 1. Understanding Scope

- **Scope** refers to the **range of statements** in which a variable is **visible** and can be **referenced**.
- It determines **where a variable can be accessed or modified**.
- Slide analogy: a towel (`twalya`) in the *bathroom* can't be reached from the *bedroom* — the variable (towel) is only visible where it's declared (`bathroom()` vs `bedroom()`).

```c
void a(){}          // C
int n = 12;
```
```python
def a():
    # stmt
```

---

### 2. Static Scoping (Lexical Scope)

- **Introduced in ALGOL 60**; used by most modern languages (**C, C++, Java, Python**).
- Scope is determined at **compile time**, **before execution**.
- **Inner blocks can access variables from enclosing (outer) scopes unless shadowed.**

```c
int num = 12;
void get(int a, int b){
    if(a > b){
        int c = a;
        printf(c);
    }
    printf(c);     // ❌ error — c only exists inside the if block
}
```

> **Unfamiliar word — lexical:** relating to the *text/layout* of the code. "Lexical scope" = scope decided by where things are written in the source.

---

### 3. Variable Hiding (Shadowing)

When a variable with the **same name** is declared in a **nested block**, it **hides** the outer variable.

```c
int x = 10;
void demo() {
    int x = 20;                       // hides global x
    printf("Inside demo, x = %d\n", x);
}
```

The outer variable can still be reached using a **qualified name**:
- Ada: `main.x`
- C++: `::x` (scope-resolution operator)

#### Trace (run-time stack)

```c
int x = 10;
void demo() {
    int x = 20;
    printf(x);
}
int main() {
    printf(x);   // prints 10  (global x)
    demo();      // prints 20  (demo's local x hides global)
    printf(x);   // prints 10  (back in main; global x again)
    return 0;
}
```

**Output:** `10`, `20`, `10`

The run-time stack shows frames pushed/popped in order: `printf(x)` → `demo()` → `printf(x)`.

> **Unfamiliar word — run-time stack:** memory region that holds each active function call (its parameters and locals); a frame is pushed on call and popped on return.

#### Same idea in Pascal (also static scoping)

```pascal
program main;
var x : integer;
    procedure sub1;
    var x : integer;
    begin
        x := 5;      { hides main's x }
    end;
begin
    x := 10;
    sub1;
end.
```

`sub1`'s `x := 5` changes only the **local** `x`; `main`'s `x` stays `10`.
**Pascal (like C) uses static/lexical scoping.**

---

### 4. Blocks in Programming

- A **block** is a section of code that **introduces a new scope** for variables.
- Variables declared in a block are **created when the block is entered** and **destroyed when it is exited**.

```c
if(list[i] < list[j]) {
    int temp;                 // temp lives only in this block
    temp = list[i];
    list[i] = list[j];
    list[j] = temp;
}
```

- **Block-structured languages** (Pascal, Ada, C++) allow local declarations inside blocks.

```ada
declare TEMP : integer;
begin
    TEMP := FIRST;
    FIRST := SECOND;
    SECOND := TEMP;
end;
```

- In **Python, indentation = block**. Per the slide, a variable inside a block has a "limited lifeline" and disappears when the block finishes:

```python
while True:
    num = int(input("Enter n: "))
    if num != 0:
        continue
    else:
        break
# num no longer exists.   <-- as stated in the slide
```

> ⚠️ **Heads-up:** In real Python, `while`/`if` blocks don't create a new scope (scope is per *function/module*), so `num` would still exist after the loop. For exams, answer according to the lecture's framing, but know this nuance.

---

### 5. Evaluation of Static Scoping

**Good:** clear visibility rules; efficient to implement.

**Problems:**
- Procedures can access variables they **shouldn't**.
- Errors may not appear until **runtime**.
- **Over-exposure** of data → poor **encapsulation**.

#### Problems of static scoping (example)

```c
int x = 20;
void a(){ int y = 25; };
void b(){ int z = 30; };
void main() {
    a();
    b();
}
```

- Hard to **restrict access** to variables only where needed.
- Changing one part of a program may require **moving entire procedures or variables**.
- Moving declarations (e.g. moving `x` into `main`) can **reduce readability**.
- May lead to **excessive global variables** and **less modular code**.
- Final design may be **awkward/unintuitive** due to forced structure.

> **Unfamiliar word — encapsulation:** hiding data so only the code that needs it can touch it.

---

# PART 2 — DYNAMIC SCOPING (Lecture 9B)

### 6. Activation

- Each call of a function is an **activation**.
- If the function has local variables, **each activation has its own local environment**. Crucial for **recursion**, where many activations of the same function exist at once.

```python
def greet():
    print("Hello")
greet()   # activation 1
greet()   # activation 2
```

---

### 7. Dynamic Scoping

- Variable scope is determined by the **sequence of calls** to subprograms (decided at **run time**), *not* by where the code is written.

| Term | Meaning |
|------|---------|
| **Dynamic parent** | The subprogram that **called** the current subprogram |
| **Dynamic ancestor** | **All** subprogram calls that **led to** the current subprogram |

**Example call chain (from the C program above):**

```c
int x = 20;
void a(){ int y = 25; };
void b(){ int z = 30; };
void main() {
    a();
    b();
}
```
- Dynamic parent of `a` = `main`; dynamic parent of `b` = `main`.
- Dynamic ancestors of `a` = `main` (and anything above it).
- **Under static scoping**, `y` is visible only inside `a`, `z` only inside `b`, `x` everywhere.
- **Under dynamic scoping**, a name is looked up by following the **chain of callers** instead.

---

### 8. Dynamic Scoping Example in Perl

```perl
our $x = 20;
sub a { my $x = 30; c(); }
sub b { local $x = 25; c(); }
sub c { print "$x\n"; }
a(); b();
```

| Keyword | Kind of scoping | Effect |
|---------|-----------------|--------|
| `our $x` | Global (package) variable | Creates the global `$x = 20` |
| `my $x`  | **Lexical (static)** | Private to `a`; `c()` **cannot** see it |
| `local $x` | **Dynamic** | Temporarily replaces the global `$x` for the *duration of `b` and everything it calls* |

**Trace (run-time stack: `a()` → `c()`, later `b()` → `c()`):**
1. `a()` → `my $x = 30` is invisible to `c()` → `c` prints global **`20`**.
2. `b()` → `local $x = 25` overrides global while `b` runs → `c()` (called by `b`) prints **`25`**.

**Output:** `20` then `25`

---

### 9. Static vs. Dynamic Scoping — Side by Side

| | **Static (Lexical)** | **Dynamic** |
|---|---|---|
| Decided at | **Compile time** | **Run time** |
| Based on | Program **text/nesting** | **Call sequence** |
| Looks up non-local names in | Enclosing blocks | Dynamic parent / ancestors |
| Languages (slides) | C, C++, Java, Python, Pascal | Perl (`local`) |
| Advantage | Predictable, efficient | Callee sees caller's context |
| Weakness | Over-exposure, excess globals | Result depends on who calls |

---

## C. Application Quiz

**1.** When is scope determined in static scoping?
a) Run time  b) Compile time  c) Link time  d) Never

**2.** Which language feature introduced static scoping?
a) C  b) ALGOL 60  c) Perl  d) Java

**3.** What does this print?
```c
int x = 10;
void demo() { int x = 20; printf("%d ", x); }
int main() { printf("%d ", x); demo(); printf("%d ", x); return 0; }
```

**4.** *True or False:* In C, `c` can be printed after the `if` block in which `int c = a;` was declared.

**5.** What is the term for an inner variable with the same name as an outer variable "hiding" it?

**6.** In C++, how can you access a global `x` hidden by a local `x`?

**7.** *True or False:* Pascal uses dynamic scoping.

**8.** A **block** is:
a) A syntax error  b) A section of code that introduces a new scope  c) A memory leak  d) A function call only

**9.** When are variables declared in a block created and destroyed?

**10.** Which is a drawback of static scoping?
a) Slow compile  b) Over-exposure of data / excess globals  c) Cannot use functions  d) No block support

**11.** In dynamic scoping, scope depends on:
a) Where code is written  b) The sequence of calls  c) File names  d) Variable types

**12.** The subprogram that called the current subprogram is the:
a) Dynamic ancestor  b) Static parent  c) Dynamic parent  d) Activation

**13.** What is printed by the Perl code?
```perl
our $x = 20;
sub a { my $x = 30; c(); }
sub b { local $x = 25; c(); }
sub c { print "$x\n"; }
a(); b();
```

**14.** Why can't `c()` see the `$x = 30` in `a()`?

**15.** *True or False:* Each call to a function creates a separate activation with its own local environment.

---

### ✅ Answer Key

| # | Answer | Short Explanation |
|---|--------|-------------------|
| 1 | **b) Compile time** | Static scope is fixed from program text before execution. |
| 2 | **b) ALGOL 60** | Slide: introduced in ALGOL 60. |
| 3 | **`10 20 10`** | `main` prints global (10); `demo` prints its local (20); back to global (10). |
| 4 | **False** | `c` is block-scoped; using it outside causes an error. |
| 5 | **Variable hiding (shadowing)** | Nested declaration hides the outer one. |
| 6 | **`::x`** | Scope-resolution operator (Ada uses `main.x`). |
| 7 | **False** | Pascal (like C) is static/lexical. |
| 8 | **b)** | A block introduces a new scope. |
| 9 | **Created on block entry, destroyed on block exit** | Limited lifetime inside the block. |
| 10 | **b)** | Static scoping can over-expose data and encourage globals. |
| 11 | **b)** | Dynamic scoping follows the call sequence. |
| 12 | **c) Dynamic parent** | Dynamic ancestors = *all* calls leading to the current one. |
| 13 | **`20` then `25`** | `a` → `my` is private → global 20; `b` → `local` makes 25 visible to `c`. |
| 14 | **`my` is lexical** | `my $x` is private to `a`'s own block, not to functions it calls. |
| 15 | **True** | Matters most for recursion. |
