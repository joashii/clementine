# Verilog Study Guide: 8-Bit XOR

## 1. The Complete Code

```verilog
// 1-bit XOR module
module xor_1bit (
  input a,
  input b,
  output q
);
  // XOR of inputs
  assign q = a ^ b;
endmodule

// 8-bit XOR module (8 connected 1-bit XORs)
module xor_8bit (
  input [7:0] a,
  input [7:0] b,
  output [7:0] q
);
  // connects individual XORs
  xor_1bit x0 (.a(a[0]), .b(b[0]), .q(q[0]));
  xor_1bit x1 (.a(a[1]), .b(b[1]), .q(q[1]));
  xor_1bit x2 (.a(a[2]), .b(b[2]), .q(q[2]));
  xor_1bit x3 (.a(a[3]), .b(b[3]), .q(q[3]));
  xor_1bit x4 (.a(a[4]), .b(b[4]), .q(q[4]));
  xor_1bit x5 (.a(a[5]), .b(b[5]), .q(q[5]));
  xor_1bit x6 (.a(a[6]), .b(b[6]), .q(q[6]));
  xor_1bit x7 (.a(a[7]), .b(b[7]), .q(q[7]));
endmodule
```

---

## 2. Basic Logic Gate Used (Truth Table)

Before reading the code, here is the single basic logic gate used inside `xor_1bit`:

### XOR Gate (`^` — Exclusive OR)
* **Rule:** Outputs `1` **only if the two inputs are different** (one is `1` and the other is `0`). If both inputs are the same (`0` and `0`, or `1` and `1`), it outputs `0`.

| `a` | `b` | `a ^ b` (`q`) | Why? |
| :-: | :-: | :---: | :--- |
| 0 | 0 | **0** | Both inputs are the same (`0`) |
| 0 | 1 | **1** | Inputs are different |
| 1 | 0 | **1** | Inputs are different |
| 1 | 1 | **0** | Both inputs are the same (`1`) |

---

## 3. Module-by-Module & Line-by-Line Breakdown

This code is divided into **2 modules**:
1. **`xor_1bit`**: A tiny building block that compares just two single bits (`a` and `b`) using one XOR gate.
2. **`xor_8bit`**: A larger chip that places 8 copies of that 1-bit block side-by-side to compare 8-bit numbers all at once. *(Notice: Unlike the 8-bit Full Adder, XOR does **not** need carry wires `c1..c7` between blocks because each bit position is completely independent!)*

---

### Module 1: `xor_1bit`

#### Code
```verilog
// 1-bit XOR module
module xor_1bit (
  input a,
  input b,
  output q
);
  // XOR of inputs
  assign q = a ^ b;
endmodule
```

#### Line-by-Line Explanation
* **`// 1-bit XOR module`**
  * A comment for humans. Verilog completely ignores anything after `//`.
* **`module xor_1bit (`**
  * Starts defining a new hardware chip (called a `module`) named `xor_1bit`. The opening parenthesis `(` begins the list of pins sticking out of this chip.
* **`  input a,`**
  * Declares a 1-bit input pin named `a` (the first binary digit to compare). The comma `,` separates it from the next pin.
* **`  input b,`**
  * Declares a 1-bit input pin named `b` (the second binary digit to compare).
* **`  output q`**
  * Declares a 1-bit output pin named `q` (the result of the XOR gate). *(Notice there is no comma here because it is the last pin in the list.)*
* **` );`**
  * Closes the list of input/output pins for this chip. Every module header ends with a semicolon `;`.
* **`  // XOR of inputs`**
  * A comment explaining that the next line computes the XOR operation.
* **`  assign q = a ^ b;`**
  * **`assign`** acts like soldering a permanent wire between a logic gate and an output pin.
  * **`^`** is Verilog's symbol for an **XOR** gate. It continuously compares `a` and `b` and sends `1` to `q` if they are different, or `0` if they are the same.
* **`endmodule`**
  * Marks the end of the `xor_1bit` chip. *(Note: `endmodule` never takes a semicolon.)*

---

### Module 2: `xor_8bit`

#### Code
```verilog
// 8-bit XOR module (8 connected 1-bit XORs)
module xor_8bit (
  input [7:0] a,
  input [7:0] b,
  output [7:0] q
);
  // connects individual XORs
  xor_1bit x0 (.a(a[0]), .b(b[0]), .q(q[0]));
  xor_1bit x1 (.a(a[1]), .b(b[1]), .q(q[1]));
  xor_1bit x2 (.a(a[2]), .b(b[2]), .q(q[2]));
  xor_1bit x3 (.a(a[3]), .b(b[3]), .q(q[3]));
  xor_1bit x4 (.a(a[4]), .b(b[4]), .q(q[4]));
  xor_1bit x5 (.a(a[5]), .b(b[5]), .q(q[5]));
  xor_1bit x6 (.a(a[6]), .b(b[6]), .q(q[6]));
  xor_1bit x7 (.a(a[7]), .b(b[7]), .q(q[7]));
endmodule
```

#### Line-by-Line Explanation
* **`// 8-bit XOR module (8 connected 1-bit XORs)`**
  * A comment describing the second module.
* **`module xor_8bit (`**
  * Starts defining the larger 8-bit XOR chip named `xor_8bit`.
* **`  input [7:0] a,`**
  * Declares an **8-bit input bus** (a bundle of 8 wires) named `a`. `[7:0]` means the wires are numbered from `7` (leftmost/MSB) down to `0` (rightmost/LSB).
* **`  input [7:0] b,`**
  * Declares an **8-bit input bus** named `b` (the second 8-bit number).
* **`  output [7:0] q`**
  * Declares an **8-bit output bus** named `q` that will hold the final 8-bit XOR result. *(No comma because it is the last pin.)*
* **` );`**
  * Closes the pin list for `xor_8bit`.
* **`  // connects individual XORs`**
  * A comment introducing the 8 copies of `xor_1bit`.
* **`  xor_1bit x0 (.a(a[0]), .b(b[0]), .q(q[0]));`**
  * Places the first copy of `xor_1bit` and gives this copy a unique label: **`x0`** (for bit `0`, the rightmost bit).
  * **`.a(a[0])`**: Connects wire `[0]` of input bus `a` to the `.a` pin of `x0`.
  * **`.b(b[0])`**: Connects wire `[0]` of input bus `b` to the `.b` pin of `x0`.
  * **`.q(q[0])`**: Sends `x0`'s `.q` output to wire `[0]` of the `q` output bus.
* **`  xor_1bit x1 (.a(a[1]), .b(b[1]), .q(q[1]));`**
  * Places copy **`x1`** for bit `1`. Computes `a[1] ^ b[1]` and sends the result to `q[1]`.
* **`  xor_1bit x2 (.a(a[2]), .b(b[2]), .q(q[2]));`**
  * Places copy **`x2`** for bit `2`. Computes `a[2] ^ b[2]` and sends the result to `q[2]`.
* **`  xor_1bit x3 (.a(a[3]), .b(b[3]), .q(q[3]));`**
  * Places copy **`x3`** for bit `3`. Computes `a[3] ^ b[3]` and sends the result to `q[3]`.
* **`  xor_1bit x4 (.a(a[4]), .b(b[4]), .q(q[4]));`**
  * Places copy **`x4`** for bit `4`. Computes `a[4] ^ b[4]` and sends the result to `q[4]`.
* **`  xor_1bit x5 (.a(a[5]), .b(b[5]), .q(q[5]));`**
  * Places copy **`x5`** for bit `5`. Computes `a[5] ^ b[5]` and sends the result to `q[5]`.
* **`  xor_1bit x6 (.a(a[6]), .b(b[6]), .q(q[6]));`**
  * Places copy **`x6`** for bit `6`. Computes `a[6] ^ b[6]` and sends the result to `q[6]`.
* **`  xor_1bit x7 (.a(a[7]), .b(b[7]), .q(q[7]));`**
  * Places the final copy **`x7`** for bit `7` (the leftmost bit). Computes `a[7] ^ b[7]` and sends the result to `q[7]`.
* **`endmodule`**
  * Marks the end of the `xor_8bit` chip.

---

## 4. How Did We Arrive at the Formula?

1. **Direct Hardware Mapping:** Unlike addition (which requires combining `AND`, `OR`, and `XOR` gates to handle carries), **XOR** is already a fundamental logic gate built directly into Verilog using the **`^`** operator (`assign q = a ^ b;`).
2. **Bitwise Operation:** A "bitwise" 8-bit XOR simply means performing `a ^ b` on each matching pair of bits (`a[0]` with `b[0]`, `a[1]` with `b[1]`, ..., `a[7]` with `b[7]`). Because one bit's XOR result never affects its neighbor, all 8 blocks (`x0` through `x7`) run completely in parallel with zero wires connecting them to each other.

---

## 5. Elaborated Step-by-Step Example (`10101010 ^ 01010101`)

Let's trace the second XOR test case from your notebook's testbench:
* **`a` = `10101010`**
* **`b` = `01010101`**

First, here is how each individual bit of `a` and `b` maps to its wire index (`[7]` is leftmost, `[0]` is rightmost):

| Bit Index | `[7]` | `[6]` | `[5]` | `[4]` | `[3]` | `[2]` | `[1]` | `[0]` |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **`a`** | `1` | `0` | `1` | `0` | `1` | `0` | `1` | `0` |
| **`b`** | `0` | `1` | `0` | `1` | `0` | `1` | `0` | `1` |

Because `xor_8bit` is physical hardware, **all 8 blocks (`x0` through `x7`) calculate their outputs at the exact same instant**:

* **Step 1 (`x0` — Bit `[0]`):** `a[0] = 0`, `b[0] = 1` $\rightarrow$ Different $\rightarrow$ `0 ^ 1` = **`q[0] = 1`**
* **Step 2 (`x1` — Bit `[1]`):** `a[1] = 1`, `b[1] = 0` $\rightarrow$ Different $\rightarrow$ `1 ^ 0` = **`q[1] = 1`**
* **Step 3 (`x2` — Bit `[2]`):** `a[2] = 0`, `b[2] = 1` $\rightarrow$ Different $\rightarrow$ `0 ^ 1` = **`q[2] = 1`**
* **Step 4 (`x3` — Bit `[3]`):** `a[3] = 1`, `b[3] = 0` $\rightarrow$ Different $\rightarrow$ `1 ^ 0` = **`q[3] = 1`**
* **Step 5 (`x4` — Bit `[4]`):** `a[4] = 0`, `b[4] = 1` $\rightarrow$ Different $\rightarrow$ `0 ^ 1` = **`q[4] = 1`**
* **Step 6 (`x5` — Bit `[5]`):** `a[5] = 1`, `b[5] = 0` $\rightarrow$ Different $\rightarrow$ `1 ^ 0` = **`q[5] = 1`**
* **Step 7 (`x6` — Bit `[6]`):** `a[6] = 0`, `b[6] = 1` $\rightarrow$ Different $\rightarrow$ `0 ^ 1` = **`q[6] = 1`**
* **Step 8 (`x7` — Bit `[7]`):** `a[7] = 1`, `b[7] = 0` $\rightarrow$ Different $\rightarrow$ `1 ^ 0` = **`q[7] = 1`**

### Final Result Summary Table

| Block | Bit Index | `a[i]` | `b[i]` | Are `a[i]` and `b[i]` different? | Formula (`a[i] ^ b[i]`) | **`q[i]`** |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **`x7`** (MSB) | `[7]` | `1` | `0` | Yes | `1 ^ 0` | **`1`** |
| **`x6`** | `[6]` | `0` | `1` | Yes | `0 ^ 1` | **`1`** |
| **`x5`** | `[5]` | `1` | `0` | Yes | `1 ^ 0` | **`1`** |
| **`x4`** | `[4]` | `0` | `1` | Yes | `0 ^ 1` | **`1`** |
| **`x3`** | `[3]` | `1` | `0` | Yes | `1 ^ 0` | **`1`** |
| **`x2`** | `[2]` | `0` | `1` | Yes | `0 ^ 1` | **`1`** |
| **`x1`** | `[1]` | `1` | `0` | Yes | `1 ^ 0` | **`1`** |
| **`x0`** (LSB) | `[0]` | `0` | `1` | Yes | `0 ^ 1` | **`1`** |

* **Final 8-bit `q` (`q[7:0]`):** Reading from top (`[7]`) to bottom (`[0]`) gives **`11111111`** (which matches the expected output in your testbench!).