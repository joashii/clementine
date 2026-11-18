# CMSC 124 — Lecture 8 Reviewer
### Semantic Issues of Variables: Variable Attributes and Bindings

> **Contents**
> - [A. Summarized Lecture](#a-summarized-lecture)
> - [B. Comprehensive Lecture](#b-comprehensive-lecture)
> - [C. Application Quiz (15 items)](#c-application-quiz)

---

## A. Summarized Lecture

A variable is more than a name and a value. It has **six attributes**:

| # | Attribute | One-line meaning |
|---|-----------|------------------|
| 1 | **Name** | The identifier (some variables have none, e.g. compiler temporaries) |
| 2 | **Address** | The memory location the variable is tied to *right now* |
| 3 | **Type** | The set of allowed values + legal operations (+ sometimes size/format) |
| 4 | **Value** | The bits stored in the memory cell(s) |
| 5 | **Lifetime** | The time the variable is bound to a memory location |
| 6 | **Scope** | The part of the source code where the name is visible |

- Same name can have **different addresses** (global vs. local `sum`, or each recursive call).
- **Aliases** = two or more names referring to the **same address** (e.g. pointer `b` and variable `a`).
- **L-value** = the *address* you write to; **R-value** = the *data* you read.
- A **binding** is an association between an attribute and an entity; **binding time** is *when* that association is fixed (compile, link, load, run time).
- **Type binding** can be **static** (before run time, fixed — Java) or **dynamic** (at run time, can change — Python).

---

## B. Comprehensive Lecture

### 1. Variable Attributes

A variable is described by six attributes: **Name, Address, Type, Value, Lifetime, Scope**.

---

### 2. Name

- A name is an **identifier**.
- Not all variables have names you write. Some are **temporary compiler variables** or **anonymous objects** (created without an identifier).
- The same name can be reused in different scopes, but each declaration is a *different variable*.

```c
int sum = 0;        // global
void f(){
    int sum = 5;    // local
}
```

The global `sum` and the local `sum` have **different addresses** even though they share a name.

> **Unfamiliar word — identifier:** a programmer-chosen name for a variable, function, etc.

---

### 3. Address

- The **address** is the memory location associated with the variable **at the current time**.
- It can change over the program's run (e.g. a local variable gets a new address each time its function is called).

```c
int fact(int n){
    int a = 1;      // a new address on each call
    ...
}
```

With **recursion**, the same local variable name (`a`) exists at **multiple addresses** — one per active call (one at a time is "current" for each activation).

---

### 4. Aliases

- **Alias:** two or more identifiers refer to the **same address**.
- If either name writes to that address, the other "sees" the change.

```c
int a = 5;
int *b = &a;      // b is an alias of a (b points to a's address)
*b = 9;           // writes to a's memory cell
printf("a = %d, *b = %d\n", a, *b);
```

**Output:** `a = 9, *b = 9`

> **Unfamiliar words**
> - **Pointer (`int *b`)** — a variable that stores a memory address.
> - **`&a`** — "address-of" operator; gives the address of `a`.
> - **`*b`** — "dereference" operator; accesses the value at the address stored in `b`.

---

### 5. Type

A type determines:

1. **The set of values** — e.g. a 16-bit INTEGER allows −32,768 .. 32,767.
2. **The legal operations** — e.g. `+`, `-`, `abs`.
3. Sometimes the **representation size/format** (implementation-defined).

---

### 6. Value

- The value is simply the **bits stored in the memory cell(s)**.
- Memory is modeled as **abstract cells**.

**L-value vs. R-value**

| Term | Meaning | Used when |
|------|---------|-----------|
| **L-value** | The **location (address)** you need to *write* | Left side of assignment |
| **R-value** | The **data contents** you *read* | Right side / reading |

```c
x = y + 1;
// x  -> needs its L-value (where to store the result)
// y  -> contributes its R-value (the data) to compute y + 1
//       (y + 1 is itself another R-value)
```

---

### 7. Lifetime

- **Lifetime:** activation → deactivation; the time during which the variable is **bound to a specific memory location**.

Stages (in order):

```
1. Allocate memory for the variable
2. Bind memory cell(s) to the variable
3. Use the variable
4. Unbind the memory cell(s)
5. Deallocate the memory of the variable
```

---

### 8. Scope

- **Scope:** the region of the program's **source code** where a name is **visible and can be accessed**.

```c
int a = 5;            // global scope
int main() {
    int a = 2;        // local scope
    {
        int a = 3;    // block scope
    }
}
```

> **Scope vs. Lifetime:** scope is about *where in the code* a name is visible; lifetime is about *when during execution* it has storage.

---

### 9. Bindings

- A **binding** is an **association between an attribute and an entity** ("just an association").

**Examples from the slides**

| Binding | When it is fixed |
|---------|------------------|
| Symbol `*` bound to the multiplication operation | (language definition / compile) |
| Identifier `count` bound to a type (`int`) | **Compile time** |
| A call to `printf` bound to library code | **Link time** |
| Variable `x` bound to a storage cell | **Load / run time** |

### 10. Binding Time

- **Binding time** = *when* the association is fixed.
- Understanding binding time is a **prerequisite to understanding the semantics** of a language.

### 11. Type Bindings

| | **Static Binding** | **Dynamic Binding** |
|---|---|---|
| When | Occurs **before run time** | Occurs **during run time** |
| Changes? | Stays **unchanged** the whole execution | **Can change** during execution |
| Example language | Java | Python |

```java
// Static binding (Java)
int x;
x = 5;          // ✅ valid
x = "hello";    // ❌ error: x is bound to int
```

```python
# Dynamic binding (Python)
x = 5           # ✅ x currently holds an int
x = "hello"     # ✅ x now holds a str — type changed at run time
```

---

### Quick Glossary

| Term | Meaning |
|------|---------|
| Identifier | Name of a variable/function |
| Activation | A single call/execution of a subprogram (lifetime start) |
| Alias | Another name for the same memory address |
| Binding | Association of an attribute with an entity |
| Binding time | The moment a binding is made |
| L-value / R-value | Address to write / data to read |
| Anonymous object | An object with no identifier |

---

## C. Application Quiz

**Instructions:** Answer all 15 items, then check the key.

**1.** Which variable attribute is the memory location associated with a variable at the current time?
a) Name  b) Address  c) Scope  d) Type

**2.** What is printed?
```c
int a = 5;
int *b = &a;
*b = 9;
printf("a = %d, *b = %d", a, *b);
```

**3.** *True or False:* The global `sum` and a local `sum` inside a function share the same memory address because they have the same name.

**4.** In `x = y + 1;`, which attribute of `x` is needed?
a) R-value  b) L-value  c) Type only  d) Lifetime

**5.** Lifetime of a variable refers to:
a) The region of code where it is visible
b) The time it is bound to a memory location
c) Its data type
d) Its name length

**6.** At what time is a call to `printf` bound to the library code?
a) Compile time  b) Link time  c) Run time  d) Design time

**7.** *True or False:* In Python, `x = 5` followed by `x = "hello"` is valid.

**8.** Why does this Java code fail?
```java
int x;
x = 5;
x = "hello";
```

**9.** Arrange the lifetime stages: (A) Use variable, (B) Allocate memory, (C) Deallocate memory, (D) Bind memory cell(s), (E) Unbind memory cell(s).

**10.** A recursive function `fact(int n)` has a local `int a = 1;`. If `fact` is called 3 times (nested), how many addresses does `a` occupy at the deepest point?

**11.** Which `a` is visible inside the innermost braces?
```c
int a = 5;
int main() {
    int a = 2;
    {
        int a = 3;
        // here
    }
}
```

**12.** Two identifiers referring to the same address are called:
a) Overloads  b) Aliases  c) Literals  d) Constants

**13.** A binding is best defined as:
a) A loop construct
b) An association between an attribute and an entity
c) A memory cell
d) A compiler error

**14.** *True or False:* Every variable has a name that the programmer writes.

**15.** Identifier `count` is bound to type `int` at:
a) Load time  b) Compile time  c) Link time  d) Run time (always)

---

### ✅ Answer Key

| # | Answer | Short Explanation |
|---|--------|-------------------|
| 1 | **b) Address** | Address = memory location tied to the variable *now*. |
| 2 | **`a = 9, *b = 9`** | `b` is an alias of `a`; writing through `*b` changes `a`. |
| 3 | **False** | Same name, different variables → different addresses. |
| 4 | **b) L-value** | `x` is being written to, so its location is needed; `y` gives an R-value. |
| 5 | **b)** | Lifetime = time bound to a memory location. (a) describes scope. |
| 6 | **b) Link time** | The library call is resolved when linking. |
| 7 | **True** | Python uses dynamic type binding. |
| 8 | **Static type binding** | `x` is bound to `int` at compile time and cannot hold a string. |
| 9 | **B → D → A → E → C** | Allocate, bind, use, unbind, deallocate. |
| 10 | **3** | Each activation gets its own local `a` (one address per call). |
| 11 | **`a = 3`** | The nearest (block-scope) declaration hides outer ones. |
| 12 | **b) Aliases** | Several names → one address. |
| 13 | **b)** | Binding = attribute ↔ entity association. |
| 14 | **False** | Some have no names (compiler temporaries, anonymous objects). |
| 15 | **b) Compile time** | Type binding to an identifier in a statically typed language happens at compile time. |
