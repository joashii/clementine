# Verilog Study Guide: 8-Bit FUNCTION (`subtractor_8bit` & `function_8bit`)

## 1. The Complete Code

*(Note: In your notebook, Section `[5]` repeats `full_adder_1bit`, `full_adder_8bit`, `nand_1bit`, and `nand_8bit` at the top so the cell can compile on its own. Below are the two new modules introduced in Section `[5]`.)*

```verilog
// 8-bit subtractor: a - b = a + (~b) + 1 (two's complement), reuses the 8-bit adder
module subtractor_8bit (
  input [7:0] a,
  input [7:0] b,
  output [7:0] diff
);
  wire unused_cout;   // carry out is not needed
  full_adder_8bit sub_adder (.a(a), .b(~b), .cin(1'b1), .sum(diff), .cout(unused_cout));
endmodule

// 8-bit FUNCTION for AB2L: O = NOT(A) - NAND(B, B)
module function_8bit (
  input [7:0] a,
  input [7:0] b,
  output [7:0] o
);
  wire [7:0] not_a;     // NOT(A) is the same as NAND(A, A)
  wire [7:0] nand_bb;   // NAND(B, B)

  nand_8bit       f_not  (.a(a), .b(a), .q(not_a));
  nand_8bit       f_nand (.a(b), .b(b), .q(nand_bb));
  subtractor_8bit f_sub  (.a(not_a), .b(nand_bb), .diff(o));
endmodule
```

---

## 2. Core Logic Rules Used (Truth Tables & Identities)

Instead of inventing brand-new logic gates from scratch, Section `[5]` cleverly **reuses** your `nand_8bit` and `full_adder_8bit` chips using two fundamental digital logic rules:

### 1. Universal NAND Identity: `NAND(X, X) = NOT(X)`
* **Rule:** When you feed the exact same signal `X` into both inputs of a NAND gate, it behaves identically to a **NOT (`~`)** inverter.

| `X` | Step 1: `X & X` | Step 2: `~(X & X)` = `NAND(X, X)` | Compare with `~X` (`NOT(X)`) |
| :-: | :---: | :---: | :---: |
| 0 | 0 | **1** | **1** |
| 1 | 1 | **0** | **0** |

### 2. Two's Complement Subtraction: `A - B = A + (~B) + 1`
* **Rule:** Digital circuits do not need a separate "minus" gate. To subtract `B` from `A` in 8-bit binary:
  1. Invert every bit of `B` (`~B`, called the **one's complement**).
  2. Add `1` (`cin = 1'b1`, turning `~B + 1` into the **two's complement**, which represents `-B`).
  3. Add that to `A` using your regular `full_adder_8bit` chip, and drop the final carry-out (`unused_cout`).

---

## 3. Module-by-Module & Line-by-Line Breakdown

This code is divided into **2 modules**:
1. **`subtractor_8bit`**: Wraps around `full_adder_8bit` to perform 8-bit two's complement subtraction (`a - b`).
2. **`function_8bit`**: Connects two `nand_8bit` chips and one `subtractor_8bit` chip together to compute your AB2L equation: $O = \text{NOT}(A) - \text{NAND}(B, B)$.

---

### Module 1: `subtractor_8bit`

#### Code
```verilog
// 8-bit subtractor: a - b = a + (~b) + 1 (two's complement), reuses the 8-bit adder
module subtractor_8bit (
  input [7:0] a,
  input [7:0] b,
  output [7:0] diff
);
  wire unused_cout;   // carry out is not needed
  full_adder_8bit sub_adder (.a(a), .b(~b), .cin(1'b1), .sum(diff), .cout(unused_cout));
endmodule
```

#### Line-by-Line Explanation
* **`// 8-bit subtractor: a - b = a + (~b) + 1 (two's complement), reuses the 8-bit adder`**
  * A comment explaining that this chip subtracts `b` from `a` using two's complement arithmetic by reusing `full_adder_8bit`.
* **`module subtractor_8bit (`**
  * Starts defining a new hardware chip named `subtractor_8bit`.
* **`  input [7:0] a,`**
  * Declares an **8-bit input bus** named `a` (the number being subtracted from).
* **`  input [7:0] b,`**
  * Declares an **8-bit input bus** named `b` (the number to subtract).
* **`  output [7:0] diff`**
  * Declares an **8-bit output bus** named `diff` (short for *difference*, holding the result of `a - b`). *(No comma because it is the last pin.)*
* **` );`**
  * Closes the pin list for `subtractor_8bit`.
* **`  wire unused_cout;   // carry out is not needed`**
  * Creates a 1-bit internal dummy wire named `unused_cout`. Every output pin of a module must be connected to something when you instantiate it, so this wire catches the final carry-out from the adder and safely ignores it (since 8-bit subtraction wraps around modulo 256).
* **`  full_adder_8bit sub_adder (.a(a), .b(~b), .cin(1'b1), .sum(diff), .cout(unused_cout));`**
  * Places one copy of your `full_adder_8bit` chip and labels it **`sub_adder`**:
    * **`.a(a)`**: Feeds the 8-bit bus `a` straight into the adder's `.a` input.
    * **`.b(~b)`**: Uses the bitwise NOT operator `~` to flip all 8 bits of `b` before feeding them into the adder's `.b` input.
    * **`.cin(1'b1)`**: Ties the adder's carry-in pin permanently to **`1`** (`1-bit binary 1`), which adds the `+ 1` needed for two's complement subtraction!
    * **`.sum(diff)`**: Sends the 8-bit sum directly to `subtractor_8bit`'s `diff` output bus.
    * **`.cout(unused_cout)`**: Sends the leftover carry-out into the dummy wire `unused_cout`.
* **`endmodule`**
  * Marks the end of the `subtractor_8bit` chip.

---

### Module 2: `function_8bit`

#### Code
```verilog
// 8-bit FUNCTION for AB2L: O = NOT(A) - NAND(B, B)
module function_8bit (
  input [7:0] a,
  input [7:0] b,
  output [7:0] o
);
  wire [7:0] not_a;     // NOT(A) is the same as NAND(A, A)
  wire [7:0] nand_bb;   // NAND(B, B)

  nand_8bit       f_not  (.a(a), .b(a), .q(not_a));
  nand_8bit       f_nand (.a(b), .b(b), .q(nand_bb));
  subtractor_8bit f_sub  (.a(not_a), .b(nand_bb), .diff(o));
endmodule
```

#### Line-by-Line Explanation
* **`// 8-bit FUNCTION for AB2L: O = NOT(A) - NAND(B, B)`**
  * A comment stating the exact formula assigned to lab section AB2L.
* **`module function_8bit (`**
  * Starts defining the custom function chip named `function_8bit`.
* **`  input [7:0] a,`**
  * Declares the 8-bit input bus `a` (operand `A`).
* **`  input [7:0] b,`**
  * Declares the 8-bit input bus `b` (operand `B`).
* **`  output [7:0] o`**
  * Declares the 8-bit output bus `o` that carries the final answer of the function.
* **` );`**
  * Closes the pin list for `function_8bit`.
* **`  wire [7:0] not_a;     // NOT(A) is the same as NAND(A, A)`**
  * Creates an internal **8-bit wire bundle** named `not_a` to hold the intermediate result of $\text{NOT}(A)$.
* **`  wire [7:0] nand_bb;   // NAND(B, B)`**
  * Creates an internal **8-bit wire bundle** named `nand_bb` to hold the intermediate result of $\text{NAND}(B, B)$.
* **`  nand_8bit       f_not  (.a(a), .b(a), .q(not_a));`**
  * Places a copy of `nand_8bit` labeled **`f_not`**. By plugging `a` into **both** `.a(a)` and `.b(a)`, it computes $\text{NAND}(A, A) = \text{NOT}(A)$ and stores the 8-bit result on wire `not_a`.
* **`  nand_8bit       f_nand (.a(b), .b(b), .q(nand_bb));`**
  * Places a second copy of `nand_8bit` labeled **`f_nand`**. By plugging `b` into **both** `.a(b)` and `.b(b)`, it computes $\text{NAND}(B, B)$ and stores the 8-bit result on wire `nand_bb`.
* **`  subtractor_8bit f_sub  (.a(not_a), .b(nand_bb), .diff(o));`**
  * Places one copy of `subtractor_8bit` labeled **`f_sub`**. It takes `not_a` as its first input, `nand_bb` as its second input, subtracts them (`not_a - nand_bb`), and sends the final 8-bit answer straight to output `o`.
* **`endmodule`**
  * Marks the end of the `function_8bit` chip.

---

## 4. How Did We Arrive at the Formulas?

1. **Project Spec Equation:** Your CMSC 132 project specification defines Opcode `11` for lab section **AB2L** as:
   $$O = \text{NOT}(A) - \text{NAND}(B, B)$$
2. **Breaking It Into 3 Hardware Stages:**
   * **Term 1 ($\text{NOT}(A)$):** Rather than creating a separate 8-bit inverter module, we use the universal NAND rule $\text{NAND}(A, A) = \text{NOT}(A)$ by passing `a` into both inputs of `nand_8bit`.
   * **Term 2 ($\text{NAND}(B, B)$):** We pass `b` into both inputs of a second `nand_8bit` chip.
   * **Subtraction ($X - Y$):** Using two's complement math, subtracting $Y$ is identical to adding the negative of $Y$:
     $$X - Y = X + (-Y) = X + (\sim Y + 1)$$
     Setting `.b(~b)` and `.cin(1'b1)` on `full_adder_8bit` performs that exact equation in hardware.

---

## 5. Elaborated Step-by-Step Example (`A = 10000000`, `B = 00000001`)

Let's trace the exact test case from your notebook's testbench:
* **`a` = `10000000`** ($128$ in unsigned decimal)
* **`b` = `00000001`** ($1$ in unsigned decimal)

---

### Stage 1: `f_not` Computes `not_a = NAND(a, a)`
* Flip every bit of `a = 10000000`:
  * **`not_a` = `01111111`** ($127$ in decimal)

---

### Stage 2: `f_nand` Computes `nand_bb = NAND(b, b)`
* Flip every bit of `b = 00000001`:
  * **`nand_bb` = `11111110`** ($254$ in unsigned decimal)

---

### Stage 3: `f_sub` Computes `o = not_a - nand_bb`
Inside `subtractor_8bit`, we need to compute `01111111 - 11111110`. The hardware converts this into `sub_adder`:
* **Adder input `.a`:** `not_a` = **`01111111`**
* **Adder input `.b`:** `~nand_bb` = `~(11111110)` = **`00000001`**
* **Adder input `.cin`:** **`1`** (`1'b1`)

Now `full_adder_8bit` adds `01111111 + 00000001 + 1` bit-by-bit from right (`[0]`) to left (`[7]`):

| Bit Index | `a[i]` (`not_a`) | `~b[i]` (`~nand_bb`) | Incoming Carry | Sum (`o[i]`) | Outgoing Carry |
| :---: | :---: | :---: | :---: | :---: | :---: |
| **`[7]`** (MSB) | `0` | `0` | `c7 = 1` | **`1`** | `unused_cout = 0` |
| **`[6]`** | `1` | `0` | `c6 = 1` | **`0`** | `c7 = 1` |
| **`[5]`** | `1` | `0` | `c5 = 1` | **`0`** | `c6 = 1` |
| **`[4]`** | `1` | `0` | `c4 = 1` | **`0`** | `c5 = 1` |
| **`[3]`** | `1` | `0` | `c3 = 1` | **`0`** | `c4 = 1` |
| **`[2]`** | `1` | `0` | `c2 = 1` | **`0`** | `c3 = 1` |
| **`[1]`** | `1` | `0` | `c1 = 1` | **`0`** | `c2 = 1` |
| **`[0]`** (LSB) | `1` | `1` | `cin = 1` | **`1`** | `c1 = 1` |

* **How `[0]` works:** $1 + 1 + 1 = 3$ (binary `11`), so **`o[0] = 1`** and **`c1 = 1`**.
* **How `[1]` through `[6]` work:** $1 + 0 + 1 = 2$ (binary `10`), so **`o[1..6] = 0`** and the carry `1` ripples all the way up to `[7]`.
* **How `[7]` works:** $0 + 0 + 1 = 1$ (binary `01`), so **`o[7] = 1`** and **`unused_cout = 0`**.

### Final Result
* Reading `o[7:0]` from top (`[7]`) to bottom (`[0]`) gives **`10000001`** (which is $-127$ in 8-bit two's complement, or $129$ unsigned—matching $127 - 254 \equiv 129 \pmod{256}$)!