# Verilog Study Guide: 8-Bit NAND

## 1. The Complete Code

```verilog
// 1-bit NAND module
module nand_1bit (
  input a,
  input b,
  output q
);
  // NAND of inputs
  assign q = ~(a & b);
endmodule

// 8-bit NAND module (8 connected 1-bit NANDs)
module nand_8bit (
  input [7:0] a,
  input [7:0] b,
  output [7:0] q
);
  // connects individual NANDs
  nand_1bit n0 (.a(a[0]), .b(b[0]), .q(q[0]));
  nand_1bit n1 (.a(a[1]), .b(b[1]), .q(q[1]));
  nand_1bit n2 (.a(a[2]), .b(b[2]), .q(q[2]));
  nand_1bit n3 (.a(a[3]), .b(b[3]), .q(q[3]));
  nand_1bit n4 (.a(a[4]), .b(b[4]), .q(q[4]));
  nand_1bit n5 (.a(a[5]), .b(b[5]), .q(q[5]));
  nand_1bit n6 (.a(a[6]), .b(b[6]), .q(q[6]));
  nand_1bit n7 (.a(a[7]), .b(b[7]), .q(q[7]));
endmodule
```

---

## 2. Basic Logic Gates Used (Truth Tables)

**NAND** stands for **NOT-AND**. Inside `nand_1bit`, it is built by combining an **AND (`&`)** gate followed immediately by a **NOT (`~`)** gate:

### 1. AND Gate (`&`)
* **Rule:** Outputs `1` **only if both** inputs are `1`.

| `a` | `b` | `a & b` |
| :-: | :-: | :---: |
| 0 | 0 | **0** |
| 0 | 1 | **0** |
| 1 | 0 | **0** |
| 1 | 1 | **1** |

### 2. NOT Gate (`~` — Inverter)
* **Rule:** Flips the input to its opposite (`0` becomes `1`, and `1` becomes `0`).

| `X` | `~X` |
| :-: | :---: |
| 0 | **1** |
| 1 | **0** |

### 3. Combined NAND Gate (`~(a & b)`)
* **Rule:** Outputs `0` **only if both** inputs are `1`. In every other case, it outputs `1` (the exact opposite of an AND gate).

| `a` | `b` | Step 1: `a & b` (AND) | Step 2: `~(a & b)` (`q` — NAND) | Why? |
| :-: | :-: | :---: | :---: | :--- |
| 0 | 0 | 0 | **1** | `0 & 0 = 0`, then flipped to `1` |
| 0 | 1 | 0 | **1** | `0 & 1 = 0`, then flipped to `1` |
| 1 | 0 | 0 | **1** | `1 & 0 = 0`, then flipped to `1` |
| 1 | 1 | 1 | **0** | `1 & 1 = 1`, then flipped to `0` |

---

## 3. Module-by-Module & Line-by-Line Breakdown

This code is divided into **2 modules**:
1. **`nand_1bit`**: A tiny building block that performs `NOT(a AND b)` on two single bits (`a` and `b`).
2. **`nand_8bit`**: A larger chip that places 8 copies of that 1-bit block side-by-side (`n0` through `n7`) to perform NAND on 8-bit numbers all at once. *(Just like XOR, no carry wires are needed between blocks because each bit position is completely independent!)*

---

### Module 1: `nand_1bit`

#### Code
```verilog
// 1-bit NAND module
module nand_1bit (
  input a,
  input b,
  output q
);
  // NAND of inputs
  assign q = ~(a & b);
endmodule
```

#### Line-by-Line Explanation
* **`// 1-bit NAND module`**
  * A comment for humans. Verilog completely ignores anything after `//`.
* **`module nand_1bit (`**
  * Starts defining a new hardware chip (called a `module`) named `nand_1bit`. The opening parenthesis `(` begins the list of pins sticking out of this chip.
* **`  input a,`**
  * Declares a 1-bit input pin named `a` (the first binary digit). The comma `,` separates it from the next pin.
* **`  input b,`**
  * Declares a 1-bit input pin named `b` (the second binary digit).
* **`  output q`**
  * Declares a 1-bit output pin named `q` (the result of the NAND operation). *(Notice there is no comma here because it is the last pin in the list.)*
* **` );`**
  * Closes the list of input/output pins for this chip. Every module header ends with a semicolon `;`.
* **`  // NAND of inputs`**
  * A comment explaining that the next line computes the NAND operation.
* **`  assign q = ~(a & b);`**
  * **`assign`** acts like soldering a permanent wire between logic gates and an output pin.
  * **`(a & b)`** computes the **AND** of `a` and `b` first (because of the parentheses).
  * **`~`** inverts (flips) that AND result and sends the final signal continuously to `q`.
* **`endmodule`**
  * Marks the end of the `nand_1bit` chip. *(Note: `endmodule` never takes a semicolon.)*

---

### Module 2: `nand_8bit`

#### Code
```verilog
// 8-bit NAND module (8 connected 1-bit NANDs)
module nand_8bit (
  input [7:0] a,
  input [7:0] b,
  output [7:0] q
);
  // connects individual NANDs
  nand_1bit n0 (.a(a[0]), .b(b[0]), .q(q[0]));
  nand_1bit n1 (.a(a[1]), .b(b[1]), .q(q[1]));
  nand_1bit n2 (.a(a[2]), .b(b[2]), .q(q[2]));
  nand_1bit n3 (.a(a[3]), .b(b[3]), .q(q[3]));
  nand_1bit n4 (.a(a[4]), .b(b[4]), .q(q[4]));
  nand_1bit n5 (.a(a[5]), .b(b[5]), .q(q[5]));
  nand_1bit n6 (.a(a[6]), .b(b[6]), .q(q[6]));
  nand_1bit n7 (.a(a[7]), .b(b[7]), .q(q[7]));
endmodule
```

#### Line-by-Line Explanation
* **`// 8-bit NAND module (8 connected 1-bit NANDs)`**
  * A comment describing the second module.
* **`module nand_8bit (`**
  * Starts defining the larger 8-bit NAND chip named `nand_8bit`.
* **`  input [7:0] a,`**
  * Declares an **8-bit input bus** (a bundle of 8 wires) named `a`. `[7:0]` means the wires are numbered from `7` (leftmost/MSB) down to `0` (rightmost/LSB).
* **`  input [7:0] b,`**
  * Declares an **8-bit input bus** named `b` (the second 8-bit number).
* **`  output [7:0] q`**
  * Declares an **8-bit output bus** named `q` that will hold the final 8-bit NAND result. *(No comma because it is the last pin.)*
* **` );`**
  * Closes the pin list for `nand_8bit`.
* **`  // connects individual NANDs`**
  * A comment introducing the 8 copies of `nand_1bit`.
* **`  nand_1bit n0 (.a(a[0]), .b(b[0]), .q(q[0]));`**
  * Places the first copy of `nand_1bit` and gives this copy a unique label: **`n0`** (for bit `0`, the rightmost bit).
  * **`.a(a[0])`**: Connects wire `[0]` of input bus `a` to the `.a` pin of `n0`.
  * **`.b(b[0])`**: Connects wire `[0]` of input bus `b` to the `.b` pin of `n0`.
  * **`.q(q[0])`**: Sends `n0`'s `.q` output to wire `[0]` of the `q` output bus.
* **`  nand_1bit n1 (.a(a[1]), .b(b[1]), .q(q[1]));`**
  * Places copy **`n1`** for bit `1`. Computes `~(a[1] & b[1])` and sends the result to `q[1]`.
* **`  nand_1bit n2 (.a(a[2]), .b(b[2]), .q(q[2]));`**
  * Places copy **`n2`** for bit `2`. Computes `~(a[2] & b[2])` and sends the result to `q[2]`.
* **`  nand_1bit n3 (.a(a[3]), .b(b[3]), .q(q[3]));`**
  * Places copy **`n3`** for bit `3`. Computes `~(a[3] & b[3])` and sends the result to `q[3]`.
* **`  nand_1bit n4 (.a(a[4]), .b(b[4]), .q(q[4]));`**
  * Places copy **`n4`** for bit `4`. Computes `~(a[4] & b[4])` and sends the result to `q[4]`.
* **`  nand_1bit n5 (.a(a[5]), .b(b[5]), .q(q[5]));`**
  * Places copy **`n5`** for bit `5`. Computes `~(a[5] & b[5])` and sends the result to `q[5]`.
* **`  nand_1bit n6 (.a(a[6]), .b(b[6]), .q(q[6]));`**
  * Places copy **`n6`** for bit `6`. Computes `~(a[6] & b[6])` and sends the result to `q[6]`.
* **`  nand_1bit n7 (.a(a[7]), .b(b[7]), .q(q[7]));`**
  * Places the final copy **`n7`** for bit `7` (the leftmost bit). Computes `~(a[7] & b[7])` and sends the result to `q[7]`.
* **`endmodule`**
  * Marks the end of the `nand_8bit` chip.

---

## 4. How Did We Arrive at the Formula?

1. **Definition from the Project Specs:** Your CMSC 132 specification defines Opcode `10` as **`O = NOT(A AND B)`**.
2. **Translating to Verilog Symbols:**
   * `A AND B` becomes **`(a & b)`**.
   * Wrapping that in `NOT(...)` uses the bitwise inverter **`~`**, giving **`~(a & b)`**.
   * *(Note: The parentheses around `(a & b)` are crucial! Without them, `~a & b` would only invert `a` first instead of inverting the entire AND result.)*
3. **Why NAND is Extra Special in Your Project:**
   * A NAND gate is a **universal gate**. If you tie both inputs of a NAND gate to the exact same wire (`NAND(X, X)`), look at the truth table:
     * `~(0 & 0)` = `~0` = **`1`**
     * `~(1 & 1)` = `~1` = **`0`**
   * It acts identically to a **NOT (`~`) gate**! Your project reuses `nand_8bit` in Section `[5]` (`function_8bit`) to compute both `NOT(A)` and `NAND(B, B)`.

---

## 5. Elaborated Step-by-Step Example (`11001010 NAND 10101100`)

Let's trace what happens inside `nand_8bit` when we pass in two 8-bit numbers:
* **`a` = `11001010`**
* **`b` = `10101100`**

First, here is how each individual bit of `a` and `b` maps to its wire index (`[7]` is leftmost, `[0]` is rightmost):

| Bit Index | `[7]` | `[6]` | `[5]` | `[4]` | `[3]` | `[2]` | `[1]` | `[0]` |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **`a`** | `1` | `1` | `0` | `0` | `1` | `0` | `1` | `0` |
| **`b`** | `1` | `0` | `1` | `0` | `1` | `1` | `0` | `0` |

Because `nand_8bit` is physical hardware, **all 8 blocks (`n0` through `n7`) calculate `~(a[i] & b[i])` at the exact same instant**:

* **Step 1 (`n0` — Bit `[0]`):** `a[0] = 0`, `b[0] = 0` $\rightarrow$ `~(0 & 0)` = `~0` = **`q[0] = 1`**
* **Step 2 (`n1` — Bit `[1]`):** `a[1] = 1`, `b[1] = 0` $\rightarrow$ `~(1 & 0)` = `~0` = **`q[1] = 1`**
* **Step 3 (`n2` — Bit `[2]`):** `a[2] = 0`, `b[2] = 1` $\rightarrow$ `~(0 & 1)` = `~0` = **`q[2] = 1`**
* **Step 4 (`n3` — Bit `[3]`):** `a[3] = 1`, `b[3] = 1` $\rightarrow$ `~(1 & 1)` = `~1` = **`q[3] = 0`** *(Both inputs are `1`, so NAND outputs `0`!)*
* **Step 5 (`n4` — Bit `[4]`):** `a[4] = 0`, `b[4] = 0` $\rightarrow$ `~(0 & 0)` = `~0` = **`q[4] = 1`**
* **Step 6 (`n5` — Bit `[5]`):** `a[5] = 0`, `b[5] = 1` $\rightarrow$ `~(0 & 1)` = `~0` = **`q[5] = 1`**
* **Step 7 (`n6` — Bit `[6]`):** `a[6] = 1`, `b[6] = 0` $\rightarrow$ `~(1 & 0)` = `~0` = **`q[6] = 1`**
* **Step 8 (`n7` — Bit `[7]`):** `a[7] = 1`, `b[7] = 1` $\rightarrow$ `~(1 & 1)` = `~1` = **`q[7] = 0`** *(Both inputs are `1`, so NAND outputs `0`!)*

### Final Result Summary Table

| Block | Bit Index | `a[i]` | `b[i]` | Step 1: `a[i] & b[i]` | Step 2: `~(a[i] & b[i])` | **`q[i]`** |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **`n7`** (MSB) | `[7]` | `1` | `1` | `1` | `~1` | **`0`** |
| **`n6`** | `[6]` | `1` | `0` | `0` | `~0` | **`1`** |
| **`n5`** | `[5]` | `0` | `1` | `0` | `~0` | **`1`** |
| **`n4`** | `[4]` | `0` | `0` | `0` | `~0` | **`1`** |
| **`n3`** | `[3]` | `1` | `1` | `1` | `~1` | **`0`** |
| **`n2`** | `[2]` | `0` | `1` | `0` | `~0` | **`1`** |
| **`n1`** | `[1]` | `1` | `0` | `0` | `~0` | **`1`** |
| **`n0`** (LSB) | `[0]` | `0` | `0` | `0` | `~0` | **`1`** |

* **Final 8-bit `q` (`q[7:0]`):** Reading from top (`[7]`) to bottom (`[0]`) gives **`01110111`**.