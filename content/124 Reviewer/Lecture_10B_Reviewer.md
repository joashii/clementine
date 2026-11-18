# CMSC 124 — Lecture 10B Reviewer
### Data Types: Array Types

> **Contents**
> - [A. Summarized Lecture](#a-summarized-lecture)
> - [B. Comprehensive Lecture](#b-comprehensive-lecture)
> - [C. Application Quiz (15 items)](#c-application-quiz)

---

## A. Summarized Lecture

- An **array** is a **homogeneous collection** of data where each element is identified by its **position** (index/subscript).
- **Strings vs. arrays:** some languages treat strings as **primitive** types (Java, Rust, PHP, Python, Go, Swift) with many built-in operations; others as **arrays of char** (C, Haskell, C++, C#) → simpler, closer to memory, but more manual and error-prone.
- C strings end with a **null terminator** `'\0'`.
- **Design issues of arrays:** Subscript types · Range checking · Binding of subscript type & range · Storage binding · Initialization · Row length · Array slices.
- **Storage binding categories:**

| Category | Range bound | Storage allocated | Example |
|---|---|---|---|
| Static | statically | before run time | `static int a[5]` (C) |
| Fixed stack-dynamic | statically | when declaration is elaborated | `int arr[30];` |
| Stack-dynamic | dynamically | dynamically (once, at elaboration) | `int arr[size];` |
| Fixed heap-dynamic | at run time request | at run time (heap) | `malloc(...)` |
| Heap-dynamic | can grow/shrink | heap | Python list |

- Arrays may be **rectangular** or **ragged**; stored in **row-major** or **column-major** order; may support **slices**.

---

## B. Comprehensive Lecture

### 1. Recall: Categories of Data Type
Primitive ✅ → Character String ✅ → **Array Types** ✅ (this lecture).

---

### 2. Strings and Arrays

- **String** = a data type whose values are **sequences of characters** (`"Hello"`, `"Madlang People"`, `"Mabuhay"`).
- Slide provocation: *"A string is just an array with a fancy name — there's really no difference."* The lecture then shows that languages **disagree**, so treat this as a starting claim, not the whole story.

> **Core design question:** Should the language give **you** total, granular, **low-level control**, or a super convenient, high-level, **safe** experience?

#### Primitive vs. Array-of-char Strings

| **Primitive strings** | **Array-of-char strings** |
|---|---|
| Java, Rust, PHP, Python, Go, Swift | C, Haskell, C++, C# *(as grouped in the slide)* |
| ✅ More convenient; many built-in operations | ✅ Simpler implementation; closer to memory |
| ❌ Implementation is more complex | ❌ More manual work; more error-prone |

**Python: lists and strings are both sequences**

```python
scores = [90, 85, 88, 92, 80]
# index:   0   1   2   3   4

str_msg = "Hello"
# index:  H e l l o
#         0 1 2 3 4
```

**Common string operations (Python)**

```python
# Concatenation
str_msg1 = "Hello"
str_msg2 = "Madlang People"
str_msg3 = "Mabuhay"
s = str_msg1 + str_msg2 + str_msg3

# Lexical comparison
"Pen" < "Pineapple"          # True

# Pattern matching
import re
re.search(r"\d+", "CMSC 124 Data Types")   # finds "124"
```

---

### 3. Arrays

> **Array:** a **homogeneous** collection of data in which each element is identified by its **position**.

> **Unfamiliar word — homogeneous:** all elements are of the **same type**.

**C: null terminator**
```c
char charArr[5] = "ABCD";
// index:   0    1    2    3    4
// value:  'A'  'B'  'C'  'D'  '\0'
```
`'\0'` marks the end of a C string, so the array needs **one extra slot**.

**C: partial initialization**
```c
int intArr[5] = {1, 2, 4};
// index: 0 1 2 3 4
// value: 1 2 4 0 0     <- unspecified elements become 0
```

**Accessing elements** — by array name + index/subscript. Syntax varies by language:
`array[2]`, `array{i}`, `array(3)`.

**Indexing:** may be **zero-based**, or some languages let the user choose their own indexing type.

**Perl: mixed arrays (not homogeneous!)**
```perl
my @arr = (1, 2, 4);
my @mix = (1, 2.0, "4");    # int, float, string together
```
The slide shows the array on the runtime stack with its elements (`1`, `2.0`, `"4"`) on the **heap**. Perl can convert a numeric string like `"4"` to a number in arithmetic.

---

### 4. Design Issues of Array Types

1. Subscript Types
2. Range Checking
3. Binding of Subscript Type and Range
4. Storage Binding
5. Initialization
6. Row Length
7. Array Slices

---

#### 4.1 Subscript Types
*What are the valid values for array subscripts?*

```c
arr[1]
arr[i]            // int i
arr[index()]      // function returning int
arr[i*j]          // int i * j
```
In **C**, subscripts must be integers (expressions that evaluate to int).

```javascript
arr[5]
arr[5.0]          // valid (same as 5)
arr[5.26]         // valid, but returns undefined
```
**JavaScript** is lenient: non-integer subscripts don't crash, they just return `undefined`.

---

#### 4.2 Range Checking
*Does the language check whether the subscript is within bounds?*

```c
int arr[5];
arr[2]  = 10;     // valid
arr[5]  = 11;     // valid?  ❌ out of bounds (valid indices are 0–4)
arr[10] = 22;     // valid?? ❌ out of bounds
```
C does **not** check; out-of-range access is a bug (undefined behavior), not a guaranteed error.

```python
arr = [1, 2, 3, 4, 5]
print(arr[2])     # valid  → 3
print(arr[5])     # valid? → ❌ IndexError (only 0–4)
print(arr[-4])    # valid?? → ✅ 2 (negative index counts from the end)
```

> **Rule:** languages that **check the range** of subscripts are **more reliable**.

---

#### 4.3 Binding of Subscript Type and Range

- The **type** of the subscript is usually bound **statically**.
- The **range** of valid subscripts is usually bound **dynamically**.

```python
arr = [1, 2, 3, 4, 5]       # Python
```
```c
int arr[] = {1, 2, 3, 4, 5};   // C  (slide writes: int arr = {1,2,3,4,5};)
```

---

#### 4.4 Storage Binding
*Arrays differ in **when their size is known** and **where they live in memory**.*

**① Static Array**
- Range bound **statically**; storage allocated **before run time**.
```c
static int days_in_week[5] = { 17, 18, 19, 20, 21 };
```
- ✅ More efficient (no dynamic allocation/deallocation); lifetime is the whole run → **no leak / dangling-pointer** from deallocation mistakes.
- ❌ **Less flexible.**

```c
// static array (global, file scope)
int x[5] = {1, 2, 3, 4, 5};
int main(void) {
    printf("%d\n", x[2]);   // prints 3
    return 0;
}
```

```c
// function-local static array
int counter_demo() {
    static int x[3] = {5, 2, 1};
    x[0]++;
    printf("%d", x[0]);
}
int main() {
    counter_demo();   // prints 6
    counter_demo();   // prints 7   (static keeps its value between calls)
}
```

**② Fixed Stack-Dynamic Array**
- Range bound **statically**; storage allocated when the **declaration is elaborated** (at run time, on the stack).
```c
int arr[30];
```
- ✅ Subprograms can **share the same space** when not in use.

> **Unfamiliar word — elaborated:** when execution actually reaches the declaration and allocates it.

**③ Stack-Dynamic Array**
- Range and storage are bound **dynamically**, but **fixed once elaborated**.
```c
int size;
printf("Enter num: ");
scanf("%d", &size);
int arr[size];            // size known only at run time
```
- ✅ More flexibility.
- *(The slide labels this snippet "Fixed Stack-Dynamic", but because the size comes from user input it matches the Stack-Dynamic definition.)*

**④ Fixed Heap-Dynamic Array**
- Range and storage are bound when the program **requests them during execution** (from the **heap**); fixed afterward.
```c
int *ptr;
ptr = (int *) malloc(sizeof(int) * 5);   // 5 uninitialized int slots on the heap
for (int i = 0; i < 5; i++) {
    ptr[i] = i;
}
```
`ptr` itself lives on the **runtime stack**; the 5 slots `ptr[0]`…`ptr[4]` live on the **heap**.

**⑤ Heap-Dynamic Array**
- Array can **grow and shrink** during execution.
```python
arr = [1, 2, 3]
arr.append(4)     # slide writes: arr = append(4)
arr.append(5)
```

---

#### 4.5 Initialization

```python
arr = [1, 2, 3]
```
```c
int arr[] = {1, 2, 3};
```

---

#### 4.6 Row Length (Multidimensional Arrays)
*Are rectangular or ragged arrays supported?*

| Type | Meaning |
|---|---|
| **Rectangular** | Every row has the same number of columns; every column has the same number of rows |
| **Ragged** | Rows can have **different** numbers of columns |

```
Rectangular        Ragged
1 2 3              1 2 3
4 5 6              4 5
7 8 9              6 7 8 9
```

**Mapping arrays to memory:**
- **Row-major order** — stored row by row.
- **Column-major order** — stored column by column.

---

#### 4.7 Array Slices
*Are slice operations (taking a sub-part of an array) allowed?*

```python
arr = [10, 20, 30, 40, 50]
arr[1:4]      # [20, 30, 40]
```

---

### 5. Recap
- Arrays and strings differ in **indexing, operations, and safety features** across languages.
- Array design determines **size, memory allocation, structure, and slicing support**.

---

## C. Application Quiz

**1.** An array is a ______ collection of data where elements are identified by position.
a) heterogeneous  b) homogeneous  c) random  d) sorted

**2.** What is stored in `charArr[4]`?
```c
char charArr[5] = "ABCD";
```

**3.** What is the content of `intArr` after this declaration?
```c
int intArr[5] = {1, 2, 4};
```

**4.** *True or False:* In Python, `arr[-4]` on `arr = [1,2,3,4,5]` is valid.

**5.** What happens with `arr[5]` in Python when `arr = [1,2,3,4,5]`?

**6.** Which is more reliable: a language with or without range checking?

**7.** In JavaScript, what does `arr[5.26]` return?

**8.** *True or False:* A fixed heap-dynamic array can automatically grow during execution.

**9.** Classify:
```c
int *arr;
arr = (int *) malloc(sizeof(int) * 5);
```
a) Static  b) Fixed stack-dynamic  c) Fixed heap-dynamic  d) Heap-dynamic

**10.** Classify: `int arr[30];` declared inside a function.

**11.** What is printed by the second call?
```c
int counter_demo() {
    static int x[3] = {5, 2, 1};
    x[0]++;
    printf("%d ", x[0]);
}
int main() { counter_demo(); counter_demo(); }
```

**12.** Which array category allows growing and shrinking at run time? (Give a language example.)

**13.** What is a **ragged** array?

**14.** Row-major order means the array is stored ______.
a) column by column  b) row by row  c) randomly  d) diagonally

**15.** Which is a *pro* of array-of-char strings?
a) Many built-in operations  b) Simpler implementation, closer to memory  c) Safer by default  d) Automatic resizing

---

### ✅ Answer Key

| # | Answer | Short Explanation |
|---|--------|-------------------|
| 1 | **b) homogeneous** | All elements share the same type. |
| 2 | **`'\0'` (null terminator)** | `"ABCD"` uses indices 0–3; index 4 holds the terminator. |
| 3 | **`1 2 4 0 0`** | Remaining elements default to 0. |
| 4 | **True** | Negative indices count from the end → `arr[-4]` = 2. |
| 5 | **IndexError** | Valid indices are 0–4; Python checks the range. |
| 6 | **With range checking** | Checking catches out-of-bounds access. |
| 7 | **`undefined`** | JS allows it but returns `undefined`. |
| 8 | **False** | Fixed heap-dynamic is fixed once allocated; *heap-dynamic* can grow. |
| 9 | **c) Fixed heap-dynamic** | Size chosen at run time via `malloc`, fixed afterwards. |
| 10 | **Fixed stack-dynamic** | Size static; storage allocated when the declaration is elaborated. |
| 11 | **`7`** | Static array keeps its value between calls: 6, then 7. |
| 12 | **Heap-dynamic array (e.g. Python list)** | Can grow/shrink via `append`, etc. |
| 13 | **An array whose rows have different column counts** | Opposite of rectangular. |
| 14 | **b) row by row** | Column-major stores column by column. |
| 15 | **b)** | Pros: simpler implementation, closer to memory (but more manual, error-prone). |
