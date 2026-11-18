# CMSC 124 — Mock Lecture Exam
**Design and Implementation of Programming Languages**

| | |
|---|---|
| **Format** | 40 multiple-choice with analysis questions |
| **Time** | 40 minutes (≈ 1 minute per item) |
| **Coverage** | Semantic Issues of Variables · Types of Binding · Scoping (Static & Dynamic) · Data Types: Primitives & Character Strings · Array Types · Expressions |

**Directions:** Choose the **best** answer for each item. For code items, trace the code carefully. Assume the code is otherwise correct unless the question says it has an error. Don't open the answer key until you finish.

**Topic map**

| Items | Topic |
|---|---|
| 1 – 5 | Semantic Issues of Variables |
| 6 – 8 | Types of Binding |
| 9 – 16 | Scoping (Static and Dynamic) |
| 17 – 23 | Data Types: Primitives and Character Strings |
| 24 – 32 | Array Types |
| 33 – 40 | Expressions |

---

## PART I — Semantic Issues of Variables (Items 1–5)

**1.** Study the code.
```c
int sum = 0;
void f(){
    int sum = 5;
}
```
Which statement is correct?
- A. Both `sum` variables share the same address because they have the same name.
- B. The two `sum` variables are different variables with different addresses.
- C. This causes a redeclaration compile error.
- D. When `f` runs, the global `sum` is overwritten with 5.

**2.** What is printed?
```c
int a = 5;
int *b = &a;
*b = 9;
a = a + 1;
printf("%d %d", a, *b);
```
- A. `9 9`
- B. `10 9`
- C. `10 10`
- D. `6 9`

**3.** In the statement `x = y + 1;`, which attribute-related statement is correct?
- A. `x` needs its l-value; `y` contributes its r-value.
- B. `x` needs its r-value; `y` needs its l-value.
- C. Both `x` and `y` need l-values.
- D. Both `x` and `y` need r-values.

**4.** A recursive function is defined as follows. How many different addresses does the local variable `a` occupy at the moment the deepest call is running after `fact(4)` is called?
```c
int fact(int n){
    int a = 1;
    if (n == 1) return a;
    return n * fact(n - 1);
}
```
- A. 1
- B. 3
- C. 4
- D. 5

**5.** Which shows the correct order of a variable's lifetime?
- A. Bind → Allocate → Use → Deallocate → Unbind
- B. Allocate → Bind → Use → Unbind → Deallocate
- C. Use → Allocate → Bind → Unbind → Deallocate
- D. Allocate → Use → Deallocate → Bind → Unbind

---

## PART II — Types of Binding (Items 6–8)

**6.** At what binding time is a call to `printf` bound to the library code?
- A. Link time
- B. Compile time
- C. Load time
- D. Run time

**7.** Analyze both snippets.
```java
// Java
int x;
x = 5;
x = "hello";
```
```python
# Python
x = 5
x = "hello"
```
- A. Both are valid.
- B. Both produce errors.
- C. Java is valid; Python produces an error.
- D. Java produces an error (static type binding); Python is valid (dynamic type binding).

**8.** Which pair is correctly matched with its binding time?
- A. The call to `printf` → compile time
- B. Identifier `count` to type `int` → compile time
- C. Variable `x` to a storage cell → link time
- D. The call to `printf` → run time only

---

## PART III — Scoping: Static and Dynamic (Items 9–16)

**9.** Which statement about static (lexical) scoping is correct?
- A. Scope is decided by the sequence of calls at run time.
- B. Inner blocks can never see variables of outer blocks.
- C. Scope is fixed at compile time; inner blocks can see outer variables unless they are shadowed.
- D. C and Java use dynamic scoping.

**10.** What is printed? (Assume static scoping, as in C.)
```c
int x = 10;
void demo() {
    int x = 20;
    printf("%d ", x);
}
int main() {
    printf("%d ", x);
    demo();
    printf("%d ", x);
    return 0;
}
```
- A. `10 10 10`
- B. `10 20 20`
- C. `20 20 10`
- D. `10 20 10`

**11.** Assume **static scoping**. What is printed?
```c
int x = 1;
void f()  { x = 2; }
void g()  { int x = 3; f(); printf("%d ", x); }
int main(){ g(); printf("%d", x); }
```
- A. `2 1`
- B. `3 2`
- C. `3 1`
- D. `2 2`

**12.** Now assume the **same program is run under dynamic scoping**. What is printed?
- A. `2 1`
- B. `3 2`
- C. `1 1`
- D. `3 1`

**13.** What is printed by this Perl program?
```perl
our $x = 20;
sub a { my $x = 30; c(); }
sub b { local $x = 25; c(); }
sub c { print "$x\n"; }
a(); b();
```
- A. `30` then `25`
- B. `20` then `20`
- C. `30` then `30`
- D. `20` then `25`

**14.** In a program, `main` calls `a()`, and `a` calls `c()`. Which statement about `c` is correct?
- A. Its dynamic parent is `main`; it has no dynamic ancestors.
- B. It has no dynamic parent.
- C. Its dynamic parent is `a`; its dynamic ancestors are `a` and `main`.
- D. Its dynamic parent is `a`; its only dynamic ancestor is `a`.

**15.** Which is a drawback of static scoping?
- A. Over-exposure of data and poor encapsulation, often leading to excessive global variables
- B. Scope cannot be determined before execution
- C. Variables cannot be shadowed
- D. Blocks cannot declare local variables

**16.** What happens in this code?
```c
void get(int a, int b){
    if (a > b) {
        int c = a;
        printf("%d", c);
    }
    printf("%d", c);
}
```
- A. `c` is printed twice.
- B. `c` is a global variable.
- C. The second `printf` is an error because `c` is not visible outside the `if` block.
- D. `c` lives until the program ends.

---

## PART IV — Data Types: Primitives and Character Strings (Items 17–23)

**17.** Which is **NOT** a benefit of having data types?
- A. Better error detection
- B. Better modularization
- C. Guaranteed faster execution
- D. Better documentation

**18.** What happens when this Python code runs?
```python
x = "5"
print(x + 2)
```
- A. Prints `7`
- B. Raises a `TypeError`
- C. Prints `52`
- D. Prints `5 2`

**19.** Which numeric type is the best choice for exact financial (money) computations?
- A. Decimal
- B. Float
- C. Double
- D. Complex

**20.** In C, what happens when a **signed** integer exceeds its maximum value?
- A. It always wraps around to the minimum.
- B. It is undefined behavior.
- C. It always raises a runtime error.
- D. It is automatically promoted to a `long`.

**21.** Analyze:
```c
int add(int a, int b){
    return a - b;
}
int r = add(2, 3);
```
- A. It fails type checking.
- B. Type checking guarantees `r` is 5.
- C. Python would raise a `TypeError` for it.
- D. It passes type checking but is logically wrong (`r` is −1).

**22.** Which string length category does this code illustrate?
```c
char str[30];
strcpy(str, "A");
strcpy(str, "Albacea");
```
- A. Limited dynamic length
- B. Static length
- C. Dynamic length
- D. None of these

**23.** Which best describes **dynamic length** strings?
- A. The length is fixed once declared.
- B. The length can vary, but only up to a maximum.
- C. The length can vary without limits, but with additional overhead.
- D. The length is limited by the declared array size.

---

## PART V — Array Types (Items 24–32)

**24.** What is stored in `charArr[4]`?
```c
char charArr[5] = "ABCD";
```
- A. `'D'`
- B. `' '` (space)
- C. Garbage value
- D. `'\0'` (null terminator)

**25.** What is the sum of all elements of `intArr`?
```c
int intArr[5] = {1, 2, 4};
```
- A. 7
- B. 3
- C. 12
- D. Unpredictable (garbage)

**26.** What is printed?
```python
arr = [10, 20, 30, 40, 50]
print(arr[-4], arr[1:3])
```
- A. `IndexError`
- B. `20 [20, 30, 40]`
- C. `20 [20, 30]`
- D. `30 [20, 30]`

**27.** What best describes this C code?
```c
int arr[5];
arr[5] = 11;
```
- A. Valid; it sets the last element.
- B. A guaranteed compile-time error.
- C. A Python-style `IndexError` at run time.
- D. Out of bounds (valid indices are 0–4); C does not range-check, so behavior is undefined.

**28.** What is printed by the three calls?
```c
int counter_demo() {
    static int x[3] = {5, 2, 1};
    x[0]++;
    printf("%d ", x[0]);
}
int main() {
    counter_demo();
    counter_demo();
    counter_demo();
}
```
- A. `6 7 8`
- B. `6 6 6`
- C. `5 6 7`
- D. `6 7 7`

**29.** Classify this array by storage binding.
```c
int *ptr;
ptr = (int *) malloc(sizeof(int) * 5);
```
- A. Static array
- B. Fixed stack-dynamic array
- C. Stack-dynamic array
- D. Fixed heap-dynamic array

**30.** Classify this array declared inside a function.
```c
int arr[30];
```
- A. Static array
- B. Fixed heap-dynamic array
- C. Fixed stack-dynamic array
- D. Heap-dynamic array

**31.** Which array category can **grow and shrink** during program execution?
- A. Static array
- B. Fixed stack-dynamic array
- C. Fixed heap-dynamic array
- D. Heap-dynamic array (e.g. a Python list using `append`)

**32.** An array whose rows may have different numbers of columns is called ______. If memory stores it row after row, the order is called ______.
- A. rectangular; column-major
- B. ragged; row-major
- C. ragged; column-major
- D. rectangular; row-major

---

## PART VI — Expressions (Items 33–40)

**33.** Assume `a` and `c` are initially 0. After this statement, what are `a` and `c`?
```c
a = 8 + (c = 2 + 3) - 4 / 2
```
- A. `a = 9, c = 5`
- B. `a = 11, c = 5`
- C. `a = 11, c = 0`
- D. `a = 13, c = 5`

**34.** What is `x`? (`pop()` removes and returns the last item of a list.)
```python
arr = [1, 2]
x = arr.pop() + len(arr)
```
- A. 4
- B. 2
- C. 3
- D. 1

**35.** After this code runs, what are `x` and `y`?
```c
int x = 5;
int change_x(void){
    x = 10;
    return 1;
}
int y = change_x() && (x == 10);
```
- A. `x = 5, y = 0`
- B. `x = 10, y = 0`
- C. `x = 5, y = 1`
- D. `x = 10, y = 1`

**36.** What is printed by these Python statements?
```python
print(3 + 4)
print("Mama" + "Bomba")
print([1, 2] + [3, 4])
```
- A. `7`, `TypeError`, `[4, 6]`
- B. `7`, `MamaBomba`, `[1, 2, 3, 4]`
- C. `34`, `MamaBomba`, `[1, 2, 3, 4]`
- D. `7`, `MamaBomba`, `[4, 6]`

**37.** Classify each conversion (Java).
```java
// I
int x = 25;
double y = x;
// II
double p = 1.43;
int q = (int) p;
```
- A. I is narrowing; II is widening.
- B. Both are narrowing.
- C. I is widening; II is narrowing, and `q` is 1.
- D. I is widening; II is narrowing, and `q` is 1.43.

**38.** What are `r1`, `r2`, and `r3`?
```java
int x = 5;
int y = 2;
double r1 = x + y;
double r2 = (double) x / y;
double r3 = (double) (x / y);
```
- A. `7.0`, `2.0`, `2.5`
- B. `7.0`, `2.5`, `2.0`
- C. `7`, `2.5`, `2.5`
- D. `7.0`, `2.5`, `2.5`

**39.** What is the output?
```c
int divisor = 0;
if (divisor != 0 && 10 / divisor) {
    printf("OK ");
}
printf("Done");
```
- A. A division-by-zero crash
- B. `OK Done`
- C. A compile-time error
- D. `Done`

**40.** Why does this loop condition work?
```c
while ((ch = fgetc(fp)) != EOF) {
    // do something
}
```
- A. In C, an assignment expression returns the value assigned, which is then compared with `EOF`.
- B. `fgetc` always returns `EOF`.
- C. The parentheses force the comparison to be done before the assignment.
- D. It is invalid; assignments cannot appear in conditions.

---

# ✅ ANSWER KEY (check only after finishing)

| # | Ans | # | Ans | # | Ans | # | Ans |
|---|-----|---|-----|---|-----|---|-----|
| 1 | **B** | 11 | **B** | 21 | **D** | 31 | **D** |
| 2 | **C** | 12 | **A** | 22 | **A** | 32 | **B** |
| 3 | **A** | 13 | **D** | 23 | **C** | 33 | **B** |
| 4 | **C** | 14 | **C** | 24 | **D** | 34 | **C** |
| 5 | **B** | 15 | **A** | 25 | **A** | 35 | **D** |
| 6 | **A** | 16 | **C** | 26 | **C** | 36 | **B** |
| 7 | **D** | 17 | **C** | 27 | **D** | 37 | **C** |
| 8 | **B** | 18 | **B** | 28 | **A** | 38 | **B** |
| 9 | **C** | 19 | **A** | 29 | **D** | 39 | **D** |
| 10 | **D** | 20 | **B** | 30 | **C** | 40 | **A** |

---

## 📝 Explanations

### Part I — Semantic Issues of Variables
1. **B.** Same name in different scopes → different variables → different addresses. The local `sum` hides the global one inside `f`.
2. **C.** `b` is an alias of `a` (same address). `*b = 9` makes `a` 9; `a = a + 1` makes it 10, and `*b` sees 10 too.
3. **A.** The assignment target needs its **l-value** (address to write); `y` supplies its **r-value** (data).
4. **C.** Each activation gets its own `a`: calls for n = 4, 3, 2, 1 → 4 addresses.
5. **B.** Allocate → bind → use → unbind → deallocate.

### Part II — Types of Binding
6. **A.** Library calls are bound at link time.
7. **D.** Java uses static type binding (`x` is an `int` for good); Python uses dynamic type binding (type can change at run time).
8. **B.** Binding `count` to `int` happens at compile time. `printf` → library code is link time; `x` → storage cell is load/run time.

### Part III — Scoping
9. **C.** Static scope is decided from program text at compile time; inner blocks see outer names unless shadowed.
10. **D.** `main` prints global 10 → `demo` prints its local 20 → global 10 again.
11. **B.** Static: `f` refers to the **global** `x` (where `f` is written), so global becomes 2. `g` prints its local 3; `main` prints global 2 → `3 2`.
12. **A.** Dynamic: `f` looks up `x` through the call chain, finding `g`'s local `x` first and setting it to 2. `g` prints 2; `main` prints the untouched global 1 → `2 1`.
13. **D.** `a()`: `my $x` is lexical/private, so `c` sees global `20`. `b()`: `local $x = 25` temporarily replaces the global while `b` (and the functions it calls) run, so `c` prints `25`.
14. **C.** Dynamic parent = the caller (`a`); dynamic ancestors = all calls leading to it (`a`, `main`).
15. **A.** Static scoping can over-expose data, hurt encapsulation, and push programs toward excessive globals.
16. **C.** `c` is block-scoped: it is created on block entry and destroyed on exit, so the second `printf` can't see it.

### Part IV — Data Types
17. **C.** The three benefits: error detection, modularization, documentation. Types don't guarantee speed.
18. **B.** Python can't concatenate `str` and `int` → `TypeError`.
19. **A.** Decimal gives exact decimal representation, used in financial applications.
20. **B.** Signed overflow in C is undefined behavior.
21. **D.** Types match, so it passes type checking, but the logic (`-` instead of `+`) is wrong. A program can pass type checking and still be logically incorrect.
22. **A.** Length varies (`"A"` to `"Albacea"`) but only up to the 30-char limit.
23. **C.** Dynamic length strings have no limit, at the cost of extra overhead (B and D describe limited dynamic length).

### Part V — Array Types
24. **D.** `"ABCD"` fills indices 0–3; index 4 holds the null terminator.
25. **A.** Remaining elements are initialized to 0: 1 + 2 + 4 + 0 + 0 = 7.
26. **C.** `arr[-4]` is the 4th from the end → `20`. `arr[1:3]` takes indices 1 and 2 (end is exclusive) → `[20, 30]`.
27. **D.** Valid indices are 0–4. C does no range checking → out-of-bounds access is undefined behavior.
28. **A.** `static` storage persists between calls: 6, 7, 8.
29. **D.** Size requested at run time via `malloc` and then fixed → fixed heap-dynamic.
30. **C.** Size bound statically; storage allocated when the declaration is elaborated (on the stack) → fixed stack-dynamic. *(If the size came from user input, e.g. `int arr[size];`, it would be stack-dynamic.)*
31. **D.** Only heap-dynamic arrays can grow/shrink during execution.
32. **B.** Rows with differing column counts = ragged; storing row by row = row-major.

### Part VI — Expressions
33. **B.** `c = 2 + 3 = 5` and the assignment evaluates to 5; `8 + 5 − 2 = 11`. (Parentheses set grouping, not full execution order.)
34. **C.** `pop()` returns 2 (list becomes `[1]`), then `len(arr)` is 1 → `2 + 1 = 3`.
35. **D.** `change_x()` sets `x` to 10 and returns 1 (true). Then `&&` evaluates `x == 10`, which is true → `y = 1`.
36. **B.** `+` is overloaded (operator overloading): integer addition, string concatenation, list concatenation.
37. **C.** I: `int` → `double` is widening. II: `double` → `int` is narrowing via explicit cast; the decimal part is lost, so `q = 1`.
38. **B.** `x + y` is `7` widened to `7.0` (implicit coercion). `(double) x / y` → `5.0 / 2` = `2.5`. `(double)(x / y)` → integer division `5 / 2 = 2` first, then cast → `2.0`.
39. **D.** Short-circuit: `divisor != 0` is false, so `10 / divisor` is never evaluated. Nothing in the `if` runs; `Done` prints.
40. **A.** In C, an assignment is an expression that returns the assigned value, so `(ch = fgetc(fp))` yields the character read, which is then compared with `EOF`.

---

## 🎯 Score Guide

| Score | Meaning |
|---|---|
| 36–40 | Excellent — exam-ready |
| 30–35 | Good — review the parts you missed |
| 24–29 | Fair — re-read the comprehensive reviewers |
| < 24 | Needs more review — redo the quizzes in each reviewer |

**Item-to-lecture map for review:** Items 1–8 → Lecture 8 · Items 9–16 → Lecture 9 · Items 17–23 → Lecture 10A · Items 24–32 → Lecture 10B · Items 33–40 → Lecture 11.
