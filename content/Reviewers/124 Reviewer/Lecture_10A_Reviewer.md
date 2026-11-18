# CMSC 124 — Lecture 10A Reviewer
### Data Types: Primitives and Character String Types

> **Contents**
> - [A. Summarized Lecture](#a-summarized-lecture)
> - [B. Comprehensive Lecture](#b-comprehensive-lecture)
> - [C. Application Quiz (15 items)](#c-application-quiz)

---

## A. Summarized Lecture

- A **data type** is not just a label; it defines **what operations make sense** for a value.
- **Why have data types?** → Better **error detection**, **modularization**, and **documentation**.
- **Categories:** Primitive types, Character String types, Array types (arrays are in Lecture 10B).
- **Primitive types** (not defined in terms of other types): **Numeric** (Integer, Floating-point, Complex, Decimal), **Boolean**, **Character**.
  - Integers are finite → **overflow** possible (detected, wraparound, or undefined behavior in C for signed).
  - Float/double are approximate; **Decimal** gives exact decimal representation (finance).
- **Character strings** — design considerations: **(1) Implementation** (array of char vs. primitive), **(2) Length** (static, limited dynamic, dynamic), **(3) Operations** (assignment, concatenation, substring, lexical comparison, pattern matching).

---

## B. Comprehensive Lecture

### 1. Why Data Types?

> **Your code knows what kind of information it's handling.**

Guide question: *If data types are so basic, why do languages still disagree on how they should work?*

```cpp
int x = 124;                // C++  : x is an int, always
```
```python
x = "hello"                 # Python: x can be reassigned to any type
```
```javascript
x = 12 + "Twelve";          // JS: coerces → "12Twelve"
```

**Thought experiment — "what if everything were just text?"**
`"12/25/96"` (date), `"Jing Quan"` (name), `"100"` (number) would all look the same; the computer couldn't tell what operations (compare dates? add numbers?) make sense. In real apps, using the wrong type (e.g. a phone number stored as a number loses leading `0`) causes bugs.

> **Key idea:** A data type is **not simply a label** attached to a value. It also determines **which operations make sense** for that value.

#### Three benefits

**① Better error detection** — the compiler/interpreter catches misuse.

```cpp
int x = "hello";        // C++ error: invalid conversion from 'const char*' to 'int'
```
```java
String name = 123;      // Java error: incompatible types: int cannot be converted to String
```
```python
x = "5"
print(x + 2)            # TypeError: can only concatenate str (not "int") to str
```

**② Better modularization** — function signatures state what goes in and out.

```cpp
int foo(int a, int b){
    int sum = a + b;
    return sum;
}
foo(2, 3);
```

**③ Better documentation** — the type tells readers what the variable is.

```c
bool isLoggedIn;        // obviously true/false
```

---

### 2. Categories of Data Type

1. **Primitive Data Types**
2. **Character String Types**
3. **Array Types** *(Lecture 10B)*

---

### 3. Primitive Data Types

Basic building blocks — **not defined in terms of other types**.

- Numeric types · Boolean type · Character type

#### 3.1 Numeric Data Types
Represent numbers: **Integer, Floating-point, Complex, Decimal**.

**Integer** — whole numbers.
```c
int age = 21;
```

Typical ranges for an *n*-bit integer:

| Type | Signed range | Unsigned range |
|------|--------------|----------------|
| general *n*-bit | −2^(n−1) … 2^(n−1) − 1 | 0 … 2^n − 1 |
| `short int` (16-bit) | −2^15 … 2^15 − 1 | 0 … 2^16 − 1 |
| `int` (32-bit) | −2^31 … 2^31 − 1 | 0 … 2^32 − 1 |
| `long int` (64-bit) | −2^63 … 2^63 − 1 | 0 … 2^64 − 1 |

> *Note:* the slide writes these ranges in a shorthand (e.g. `-(2^n-1)`); the standard form above is what the notation means. Bit sizes are typical, not guaranteed.

**Integer overflow** — *"Mathematically, integers are unlimited but computers have finite storage."* What if a number exceeds its maximum?
- Some languages **detect** the overflow.
- Others use **wraparound** behavior in certain contexts.
- In **C**, **signed** integer overflow is **undefined behavior**.

> **Unfamiliar word — undefined behavior:** the language standard places no requirements on what happens; the program may crash, give garbage, or seem fine.

**Float and Double**
- **Float** — real numbers with decimals; **Double** — double-precision values (more digits).
```c
float temp = 36.5 + 3.4;
```

**Complex** — numbers with a real and an imaginary part.
```python
z = 2 + 3j
```

**Decimal** — **exact** decimal representation (used in **financial apps**).
```python
from decimal import Decimal
Decimal('3.14')
```

#### 3.2 Boolean
Only two values: `true` / `false`.
```c
isValid = False
```
*(as written on the slide)*

#### 3.3 Character Type
A **single character**, stored as **numeric codes** (ASCII, Unicode).
```c
char grade = 'A';
```

> **Unfamiliar words:** **ASCII** = 7-bit code for English characters (e.g. `'A'` = 65). **Unicode** = a much larger standard covering most world scripts.

> **Design question:** *What kind of numerical behavior does our application require?* (Whole numbers? Decimals? Exact money? → choose the type accordingly.)

---

### 4. Character String Types

> **Big question:** Should a string be a **primitive/built-in** type, or a **special kind of array**?

**Design considerations:**
1. Implementation
2. String length
3. Operations

#### 4.1 Implementation

| | **Array of char** | **Primitive (built-in)** |
|---|---|---|
| Idea | Sequence of chars stored in an array | Language has a real `string` type |
| Example | ```char str[8];``` ```strcpy(str, "CMSC 124");``` (C) | ```subj = "CMSC 124"``` (Python) |

#### 4.2 String Length

**Static length** — fixed once declared.
```c
char name[20] = "Erika";
```

**Limited dynamic length** — can vary, but **up to a maximum**.
```c
char str[30];
strcpy(str, "Eliezer");
strcpy(str, "A");
strcpy(str, "Albacea");
```

**Dynamic length** — can change **without any limit**.
```perl
$my cand1 = "Miss Dubai";
$my cand2 = "Miss Urdaneta";
$my cand3 = "Miss Pares France";
```
*(Perl; as written on the slide)*

> ⚠️ Flexible, **but** extra **overhead** (resizing/reallocation) must be considered.

#### 4.3 String Operations

| Operation | Example |
|-----------|---------|
| **Assignment** | `name = "UPLB"` |
| **Concatenation** | `"CMSC" + "124"` |
| **Substring** | `text[0:4]` |
| **Lexical comparison** | `"abc" < "bcd"` → `True` |
| **Pattern matching** (regex) | `r"^[A-Z]{3}\d{3}$"` |

> **Unfamiliar words:** **Concatenation** = joining strings. **Lexical comparison** = dictionary-order comparison. **Regex** = a pattern language for matching text (the example matches 3 capital letters + 3 digits).

---

### 5. Recap

- Data types define **what data is stored** and **how it is handled**.
- Using the right type **prevents errors** and **improves program structure**.
- Strings and arrays show **different approaches** to memory representation and length management.

---

## C. Application Quiz

**1.** Which is **not** a stated benefit of having data types?
a) Error detection  b) Modularization  c) Documentation  d) Faster internet

**2.** What does Python do here?
```python
x = "5"
print(x + 2)
```

**3.** Which numeric type should be used for exact money calculations?
a) float  b) double  c) Decimal  d) char

**4.** `z = 2 + 3j` in Python is an example of which type?

**5.** *True or False:* In C, signed integer overflow is undefined behavior.

**6.** Which of the following is **not** a primitive data type category in the lecture?
a) Numeric  b) Boolean  c) Character  d) Linked list

**7.** What type does `char grade = 'A';` represent, and how is it stored?

**8.** In C, what is the size-class of this string?
```c
char name[20] = "Erika";
```
a) Static length  b) Limited dynamic  c) Dynamic  d) None

**9.** Which strategy lets strings change length without any limit, at the cost of overhead?

**10.** `char str[30]; strcpy(str, "A"); strcpy(str, "Albacea");` is an example of:
a) Static length  b) Limited dynamic length  c) Dynamic length  d) Invalid code

**11.** What does `"abc" < "bcd"` evaluate to?

**12.** *True or False:* A dynamically typed language does not perform type checking.

**13.** *True or False:* Using a floating-point type guarantees decimal values are represented exactly.

**14.** *True or False:* A program can pass type checking and still produce logically incorrect results.

**15.** *True or False:* In C, a character array needs extra space for the null terminator when storing a valid null-terminated string.

---

### ✅ Answer Key

| # | Answer | Short Explanation |
|---|--------|-------------------|
| 1 | **d)** | The three benefits are error detection, modularization, documentation. |
| 2 | **`TypeError`** | Python can't concatenate `str` and `int`. |
| 3 | **c) Decimal** | Decimal gives exact representation; floats are approximate. |
| 4 | **Complex** | Real part 2, imaginary part 3. |
| 5 | **True** | Slide: signed overflow in C is undefined behavior. |
| 6 | **d)** | Primitives: numeric, Boolean, character. |
| 7 | **Character; as a numeric code (ASCII/Unicode)** | Chars are stored as numbers. |
| 8 | **a) Static length** | Array size fixed at declaration. |
| 9 | **Dynamic length strings** | No limit, but with additional overhead. |
| 10 | **b)** | Length varies, but only up to the 30-char array limit. |
| 11 | **True** | Lexical (dictionary) order: `a` comes before `b`. |
| 12 | **False** | Dynamic languages check types at run time; they just don't require declarations. |
| 13 | **False** | Floats are approximate (e.g. 0.1 can't be stored exactly). |
| 14 | **True** | Types can be right while logic is wrong (e.g. adding instead of subtracting). |
| 15 | **True** | `"ABCD"` needs 5 chars: 4 + `'\0'`. |
