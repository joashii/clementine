# CMSC 124 — Lecture 11 Reviewer
### Expressions (and Assignment Statements)

> **Contents**
> - [A. Summarized Lecture](#a-summarized-lecture)
> - [B. Comprehensive Lecture](#b-comprehensive-lecture)
> - [C. Application Quiz (15 items)](#c-application-quiz)

---

## A. Summarized Lecture

- **Expressions** are the means of specifying computations. Classified by operand count: **unary, binary, ternary**; by kind: arithmetic, relational, logical, bitwise, assignment.
- **Four design issues:**
  1. **Operator evaluation order** — precedence, associativity, parentheses, side effects.
  2. **Operand evaluation order** — which operand is evaluated first; **side effects** can change the result.
  3. **Operator overloading** — one operator symbol, different operations depending on operand types.
  4. **Type conversions** — widening, narrowing, implicit (coercion), explicit (cast).
- **Short-circuit evaluation** skips the rest of a logical expression once the result is known (also protects against errors like divide-by-zero).
- **Assignment statements** dynamically change the binding of values to variables: simple, conditional targets, compound, unary, multiple. In some languages (C), assignment **returns the assigned value**.

---

## B. Comprehensive Lecture

### 1. Opening Puzzle

```c
a = 8 + (c = 2 + 3) - 4 / 2
```
Assume `a` and `c` start at `0`.

```
a = 8 + (c = 2 + 3) - 4 / 2
a = 8 + ( c = 5 )   - 4 / 2      // c becomes 5, and the assignment evaluates to 5
a = 8 + 5 - 2
a = 11
```

> ⚠️ **Parentheses determine *grouping*, not the *execution order* of every sub-expression.** In C, `4 / 2` could be evaluated **before** `2 + 3`.

---

### 2. Expressions

> **Expression:** a means of specifying computations in a program.

| Kind | Operands | Examples |
|---|---|---|
| **Unary** | single operand | `++`, `--`, `+`, `-`, `!`, `not`, `sizeof`, `&` |
| **Binary** | two operands | `a + b`, `x < y` |
| **Ternary** | three operands (slide: "multiple") | `cond ? a : b` |

**Operator categories:** Arithmetic · Relational/Comparison · Logical · Bitwise · Assignment (`+=`, `*=`, etc.)

---

### 3. Design Issues of Expressions

1. Operator Evaluation Order
2. Operand Evaluation Order
3. Operator Overloading
4. Type Conversions

---

### 4. Operator Evaluation Order

Questions to answer:
- What are the **precedence** rules? *(which operator goes first, e.g. `*` before `+`)*
- What is the **associativity**? *(for equal precedence, left-to-right or right-to-left)*
- **Parentheses** can alter precedence and associativity.
- Will using variables in the expression create a **side effect**?

> **Unfamiliar words:** **Precedence** = priority of operators. **Associativity** = direction in which same-priority operators group (`a - b - c` = `(a - b) - c`, left-associative).

---

### 5. Operand Evaluation Order

- The order in which a compiler/interpreter evaluates an operator's **operands/sub-expressions**.
- If evaluating an operand changes a variable, that change is a **side effect**, and **the order can affect the final result**.

> **Side effect:** happens when a function **changes a variable's value** that is used later in the same expression.

**Possible solutions**
- **Disallow** functions in expressions that may cause side effects.
- Define a **fixed order** of evaluation.

#### Example 1 (Python)

```python
arr = [1, 2]
x = arr.pop() + len(arr)
print(x)        # 3
```
`pop()` removes and returns the last item (`2`) → then `len(arr)` is now `1` → `2 + 1 = 3`.
*(`pop()` removes and returns an item from a collection, most commonly a list.)*

#### Example 2 (C)

```c
int x = 5;
int change_x(void){
    x = 10;          // side effect: modifies global x
    return 1;
}
...
int y = change_x();             // y = 1  (and x is now 10)
int y = change_x() && (x == 10); // right side of && sees the updated x (10) → y = 1
```

---

### 6. Operator Overloading

> **Operator overloading:** the same operator performs **different operations** depending on the **types of its operands**.

**C — same symbol `&`, different meanings**
```c
int a = 10;
printf("%d\n", 3 + 5 & a);   // binary & = bitwise AND → (3+5) & 10 = 8 & 10 = 8
printf("%d\n", 3 + & a);     // unary & = address-of → 3 + &a (pointer arithmetic)
```

**Python — `+` overloaded**
```python
print(3 + 4)                 # 7          (int addition)
print(2.5 + 1.2)             # 3.7        (float addition)
print("Mama" + "Bomba")      # MamaBomba  (string concatenation)
print([1,2] + [3,4])         # [1,2,3,4]  (list concatenation)
```

**Design question:** *Is the programmer allowed to define a different operation for an operator?* (Languages differ — a design issue.)

---

### 7. Type Conversions

> Allows values to be converted from one type to another. **Four kinds:**

| Kind | Meaning | Example (Java) |
|---|---|---|
| **Widening** | Convert to a type with a **wider range** that can hold/approximate the original | `int x = 25; double y = x;` |
| **Narrowing** | Convert to a **smaller range / less precision**; **may lose information** | `double x = 1.43; int y = (int) x;` → `1` *(slide writes `(int) y`, a typo)* |
| **Implicit (coercion)** | **Compiler automatically** converts based on operand types | `int x = 5; int y = 2; double result = x + y;` → `7.0` |
| **Explicit (typecast)** | **Programmer directly** tells the program to convert | `double z = (double) x / y;` → `2.5` |

> **Why `(double) x / y` = 2.5?** The cast applies to `x` first, so the division becomes `5.0 / 2` → `2.5`. Without the cast, `5 / 2` is integer division = `2`.

---

### 8. Short-Circuit Evaluation

> **Short-circuit evaluation:** skips the rest of a logical expression **once its result is known**.

- `A && B` → if `A` is false, `B` is **not** evaluated.
- `A || B` → if `A` is true, `B` is **not** evaluated.

```c
if (door == open && light == wiggles)   // slide's illustrated pattern
```

**Practical use — avoid errors:**
```c
int divisor = 0;
if (divisor != 0 && 10 / divisor) {   // divisor != 0 is false → 10/divisor never runs
    printf("%d", divisor);
}
return 0;
```
No divide-by-zero happens because the second operand is skipped.

---

### 9. Assignment Statements

> Allow the programmer to **dynamically change the binding of values to variables**.

Five forms:

| Form | Description | Example |
|---|---|---|
| **Simple** | One target gets one value | `number = 12` (Python) · `define age = 21` (as on slide; Scheme-style) |
| **Conditional targets** | Target chosen by a condition | `($x ? $a : $b) = 0;` (Perl) ≡ `if($x){ $a = 0 } else { $b = 0 }` |
| **Compound** | Operator + assign | `count += 124` (Python) · `n *= 2;` (Java) |
| **Unary** | Increment/decrement | `count++` (Java) · `--$number;` (PHP) |
| **Multiple** | Several assignments at once (tuple unpacking in Python) | `a, b = "marine", "biology"` |

**Assignment as an expression:** in some languages, an assignment **returns the value assigned**.

```c
while ((ch = fgetc(fp)) != EOF) {
    // do something
}
```
`ch = fgetc(fp)` reads a character, stores it in `ch`, **and** yields that value to be compared with `EOF`.

---

### 10. Recap
Expressions, operators and types, and assignments: how expressions are **evaluated**, how **types affect operator behavior**, and how **assignments update variables**.

---

## C. Application Quiz

**1.** Evaluate: `a = 8 + (c = 2 + 3) - 4 / 2` (a, c start at 0). What are `a` and `c`?

**2.** Which is a **unary** operator?
a) + (addition)  b) ++  c) &&  d) ==

**3.** Which design issue asks "which operand is evaluated first?"
a) Operator evaluation order  b) Operand evaluation order  c) Overloading  d) Type conversion

**4.** What is `x`?

```python
arr = [1, 2]
x = arr.pop() + len(arr)
```

**5.** *True or False:* Parentheses always control the execution order of all sub-expressions in C.

**6.** Define a **side effect** in an expression.

**7.** In Python, what does `"Mama" + "Bomba"` show, and what feature is this?

**8.** What is printed? `print([1,2] + [3,4])`

**9.** Classify: `int x = 25; double y = x;`
a) Widening  b) Narrowing  c) Explicit cast  d) Invalid

**10.** Classify and give the result: `double x = 1.43; int y = (int) x;`

**11.** What is `result`?
```java
int x = 5; int y = 2;
double result = x + y;
```

**12.** What is `z`?
```java
int x = 5; int y = 2;
double z = (double) x / y;
```

**13.** Will this cause a divide-by-zero? Why or why not?
```c
int divisor = 0;
if (divisor != 0 && 10 / divisor) { printf("%d", divisor); }
```

**14.** Which assignment form is `a, b = "marine", "biology"`?
a) Simple  b) Compound  c) Multiple  d) Unary

**15.** In `while ((ch = fgetc(fp)) != EOF)`, why does the loop condition work?

---

### ✅ Answer Key

| # | Answer | Short Explanation |
|---|--------|-------------------|
| 1 | **`a = 11`, `c = 5`** | `c = 2+3 = 5` (and evaluates to 5); `8 + 5 - 2 = 11`. |
| 2 | **b) `++`** | Unary = single operand (`++`, `--`, `!`, `sizeof`, `&`). |
| 3 | **b)** | Operand evaluation order. |
| 4 | **`3`** | `pop()` returns 2 (list becomes `[1]`), `len(arr)` = 1 → 2 + 1. |
| 5 | **False** | Parentheses set *grouping*; e.g. `4 / 2` may still run before `2 + 3`. |
| 6 | **A function/operand changes a variable used later in the same expression** | Can make results depend on evaluation order. |
| 7 | **`MamaBomba`; operator overloading** | `+` concatenates strings, adds numbers. |
| 8 | **`[1, 2, 3, 4]`** | List concatenation. |
| 9 | **a) Widening** | `int` → `double` (also happens implicitly). |
| 10 | **Narrowing, explicit cast → `1`** | Decimal part is lost. |
| 11 | **`7.0`** | Implicit conversion (coercion): `5 + 2 = 7` then widened to double. |
| 12 | **`2.5`** | Cast makes it `5.0 / 2`. Without the cast it would be `2`. |
| 13 | **No** | Short-circuit: `divisor != 0` is false, so `10 / divisor` is skipped. |
| 14 | **c) Multiple** | Tuple unpacking in Python. |
| 15 | **Assignment returns the assigned value** | `ch` is assigned, then that value is compared to `EOF`. |
