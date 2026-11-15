# Verilog Study Guide: `main_project` (Top-Level Module)

## 1. The Complete Code

```verilog
// MAIN_PROJECT: 18-bit input, 8-bit output, no registers
// Input bits: I[17] is I0 (opcode MSB), I[16] is I1, I[15:8] is A (I2 to I9), I[7:0] is B (I10 to I17)
// Output bits: O[7] is O0 (MSB) down to O[0] is O7
module main_project (
  input [17:0] I,
  output [7:0] O
);
  wire [7:0] A = I[15:8];
  wire [7:0] B = I[7:0];
  wire M0, M1, M2, M3;                        // decoder select lines
  wire [7:0] add_out, xor_out, nand_out, func_out;
  wire add_cout;                              // carry out of the adder, not used

  // decoder reads the 2 opcode bits
  decoder dec (.I0(I[17]), .I1(I[16]), .M0(M0), .M1(M1), .M2(M2), .M3(M3));

  // the four components all see the same operands
  full_adder_8bit add_unit  (.a(A), .b(B), .cin(1'b0), .sum(add_out), .cout(add_cout));
  xor_8bit        xor_unit  (.a(A), .b(B), .q(xor_out));
  nand_8bit       nand_unit (.a(A), .b(B), .q(nand_out));
  function_8bit   func_unit (.a(A), .b(B), .o(func_out));

  // the decoder line that is 1 decides which result reaches the output
  assign O = M0 ? add_out  :
             M1 ? xor_out  :
             M2 ? nand_out :
             M3 ? func_out : 8'b00000000;
endmodule
```

---

## 2. Core Concepts Used (Bus Slicing & Multiplexer)

Before reading the code line-by-line, here are the two main wiring techniques used in `main_project`:

### 1. Bus Slicing (`I[17:0]` $\rightarrow$ Opcode, `A`, and `B`)
Your project receives all inputs bundled into a single **18-bit highway** (`I[17:0]`). `main_project` splits that bundle into three smaller groups:

| Verilog Bit Slice | Spec Pin Names | Width | Purpose |
| :--- | :--- | :---: | :--- |
| **`I[17]`** | `I0` (MSB) | 1 bit | First bit of the **2-bit Opcode** |
| **`I[16]`** | `I1` | 1 bit | Second bit of the **2-bit Opcode** |
| **`I[15:8]`** | `I2` to `I9` | 8 bits | **Operand `A`** (8-bit number) |
| **`I[7:0]`** | `I10` to `I17` (LSB) | 8 bits | **Operand `B`** (8-bit number) |

### 2. The 4-to-1 Multiplexer (`? :` Ternary Operator)
A **Multiplexer (MUX)** is a hardware selector switch. It looks at the four control flags (`M0`, `M1`, `M2`, `M3`) coming out of the `decoder` and connects **only the winning 8-bit answer** to the final output bus `O`:

| `M0` | `M1` | `M2` | `M3` | Which 8-bit bus connects to `O`? |
| :-: | :-: | :-: | :-: | :--- |
| **1** | 0 | 0 | 0 | **`add_out`** (Full Adder result) |
| 0 | **1** | 0 | 0 | **`xor_out`** (XOR result) |
| 0 | 0 | **1** | 0 | **`nand_out`** (NAND result) |
| 0 | 0 | 0 | **1** | **`func_out`** (AB2L Function result) |
| 0 | 0 | 0 | 0 | **`8'b00000000`** (Fallback safety default) |

---

## 3. Module-by-Module & Line-by-Line Breakdown

This file contains **1 top-level module**:
1. **`main_project`**: The "motherboard" of your entire CPU/ALU circuit. It unpacks the 18-bit input `I`, feeds `A` and `B` to all four math/logic chips simultaneously, asks the `decoder` which operation was requested, and routes that single 8-bit result to `O`.

---

### Module 1: `main_project`

#### Code
```verilog
// MAIN_PROJECT: 18-bit input, 8-bit output, no registers
// Input bits: I[17] is I0 (opcode MSB), I[16] is I1, I[15:8] is A (I2 to I9), I[7:0] is B (I10 to I17)
// Output bits: O[7] is O0 (MSB) down to O[0] is O7
module main_project (
  input [17:0] I,
  output [7:0] O
);
  wire [7:0] A = I[15:8];
  wire [7:0] B = I[7:0];
  wire M0, M1, M2, M3;                        // decoder select lines
  wire [7:0] add_out, xor_out, nand_out, func_out;
  wire add_cout;                              // carry out of the adder, not used

  // decoder reads the 2 opcode bits
  decoder dec (.I0(I[17]), .I1(I[16]), .M0(M0), .M1(M1), .M2(M2), .M3(M3));

  // the four components all see the same operands
  full_adder_8bit add_unit  (.a(A), .b(B), .cin(1'b0), .sum(add_out), .cout(add_cout));
  xor_8bit        xor_unit  (.a(A), .b(B), .q(xor_out));
  nand_8bit       nand_unit (.a(A), .b(B), .q(nand_out));
  function_8bit   func_unit (.a(A), .b(B), .o(func_out));

  // the decoder line that is 1 decides which result reaches the output
  assign O = M0 ? add_out  :
             M1 ? xor_out  :
             M2 ? nand_out :
             M3 ? func_out : 8'b00000000;
endmodule
```

#### Line-by-Line Explanation
* **`// MAIN_PROJECT: 18-bit input, 8-bit output, no registers`**
  * A comment summarizing the project requirements: 18 input pins, 8 output pins, and purely combinational logic (no clock or memory registers).
* **`// Input bits: I[17] is I0 (opcode MSB), I[16] is I1, I[15:8] is A (I2 to I9), I[7:0] is B (I10 to I17)`**
  * A comment mapping the project specification's pin names (`I0`–`I17`) to Verilog's bit indices (`I[17]` down to `I[0]`).
* **`// Output bits: O[7] is O0 (MSB) down to O[0] is O7`**
  * A comment noting that `O[7]` is the leftmost/Most Significant Bit and `O[0]` is the rightmost/Least Significant Bit.
* **`module main_project (`**
  * Starts defining the top-level hardware chip named `main_project`.
* **`  input [17:0] I,`**
  * Declares an **18-bit input bus** named `I` (wires numbered `17` down to `0`).
* **`  output [7:0] O`**
  * Declares an **8-bit output bus** named `O` (wires numbered `7` down to `0`). *(No comma because it is the last pin.)*
* **` );`**
  * Closes the pin list for `main_project`.
* **`  wire [7:0] A = I[15:8];`**
  * Creates an internal 8-bit wire bundle named `A` and permanently connects it to bits `15` down to `8` of input `I` (extracting operand `A`).
* **`  wire [7:0] B = I[7:0];`**
  * Creates an internal 8-bit wire bundle named `B` and permanently connects it to bits `7` down to `0` of input `I` (extracting operand `B`).
* **`  wire M0, M1, M2, M3;                        // decoder select lines`**
  * Creates 4 single-bit internal wires (`M0`–`M3`) that will carry the ON/OFF select flags from the decoder to the multiplexer.
* **`  wire [7:0] add_out, xor_out, nand_out, func_out;`**
  * Creates **four separate 8-bit internal buses** to hold the 8-bit answers coming out of each of the four math/logic units.
* **`  wire add_cout;                              // carry out of the adder, not used`**
  * Creates a 1-bit dummy wire to catch the overflow carry-out from the 8-bit adder, since the project spec states *"carry out dropped"*.
* **`  // decoder reads the 2 opcode bits`**
  * A comment introducing the decoder chip.
* **`  decoder dec (.I0(I[17]), .I1(I[16]), .M0(M0), .M1(M1), .M2(M2), .M3(M3));`**
  * Places one copy of `decoder` labeled **`dec`**:
    * Plugs the two opcode bits `I[17]` and `I[16]` into `.I0` and `.I1`.
    * Connects its four outputs `.M0`–`.M3` to the internal wires `M0`, `M1`, `M2`, `M3`.
* **`  // the four components all see the same operands`**
  * A comment explaining that all four operation chips are wired to `A` and `B` in parallel.
* **`  full_adder_8bit add_unit  (.a(A), .b(B), .cin(1'b0), .sum(add_out), .cout(add_cout));`**
  * Places `full_adder_8bit` labeled **`add_unit`**. Feeds `A`, `B`, and `cin = 0` (`1'b0`), outputting the sum to `add_out` and dropping the carry into `add_cout`.
* **`  xor_8bit        xor_unit  (.a(A), .b(B), .q(xor_out));`**
  * Places `xor_8bit` labeled **`xor_unit`**. Feeds `A` and `B`, outputting the 8-bit XOR result to `xor_out`.
* **`  nand_8bit       nand_unit (.a(A), .b(B), .q(nand_out));`**
  * Places `nand_8bit` labeled **`nand_unit`**. Feeds `A` and `B`, outputting the 8-bit NAND result to `nand_out`.
* **`  function_8bit   func_unit (.a(A), .b(B), .o(func_out));`**
  * Places `function_8bit` labeled **`func_unit`**. Feeds `A` and `B`, outputting the 8-bit AB2L Function result to `func_out`.
* **`  // the decoder line that is 1 decides which result reaches the output`**
  * A comment introducing the multiplexer.
* **`  assign O = M0 ? add_out  :`**
  * **`             M1 ? xor_out  :`**
  * **`             M2 ? nand_out :`**
  * **`             M3 ? func_out : 8'b00000000;`**
  * A chained **ternary (`condition ? if_true : if_false`)** statement acting as a 4-to-1 Multiplexer:
    * If `M0` is `1`, send `add_out` to `O`.
    * Else if `M1` is `1`, send `xor_out` to `O`.
    * Else if `M2` is `1`, send `nand_out` to `O`.
    * Else if `M3` is `1`, send `func_out` to `O`.
    * Else (if none are `1`), output `8'b00000000` (all zeros).
* **`endmodule`**
  * Marks the end of the `main_project` chip.

---

## 4. How Does the Overall Architecture Work?

In software, you would write an `if / else` statement to run only one function so you don't waste CPU time. **Physical hardware works differently:**
1. **Parallel Computation:** All four chips (`add_unit`, `xor_unit`, `nand_unit`, `func_unit`) are physically wired to `A` and `B` and compute all four answers **at the exact same time**.
2. **Control & Selection:** Simultaneously, `dec` decodes `I[17:16]` so that **only one** select line (`M0`–`M3`) becomes `1`.
3. **Output Routing:** The multiplexer (`assign O = ...`) acts like a railroad track switch—it blocks the three unwanted answers and connects the single selected answer straight to `O`.

---

## 5. Elaborated Step-by-Step Example (`I = 18'b01_10101010_01010101`)

Let's trace what happens when the testbench sends this 18-bit input into `main_project`:
* **`I` = `01_10101010_01010101`** (underscores added for readability)

---

### Step 1: Bus Slicing Splits `I` into 3 Parts
* **Opcode (`I[17:16]`):** `I[17] = 0`, `I[16] = 1` $\rightarrow$ **`01`** (XOR operation)
* **Operand `A` (`I[15:8]`):** **`10101010`**
* **Operand `B` (`I[7:0]`):** **`01010101`**

---

### Step 2: `dec` (Decoder) Activates `M1`
* Inputs to `dec`: `I0 = 0`, `I1 = 1`
* Outputs from `dec`:
  * `M0 = 0`
  * **`M1 = 1`** (Selected!)
  * `M2 = 0`
  * `M3 = 0`

---

### Step 3: All 4 Units Compute Their Answers in Parallel
At the exact same instant, all four sub-modules calculate their results using `A = 10101010` and `B = 01010101`:
1. **`add_unit` (`A + B`):** `10101010 + 01010101` $\rightarrow$ **`add_out = 11111111`**
2. **`xor_unit` (`A ^ B`):** `10101010 ^ 01010101` $\rightarrow$ **`xor_out = 11111111`**
3. **`nand_unit` (`~(A & B)`):** `~(10101010 & 01010101)` $\rightarrow$ **`nand_out = 11111111`**
4. **`func_unit` (`~A - ~(B & B)`):** `01010101 - 10101010` $\rightarrow$ **`func_out = 10101011`**

---

### Step 4: The Multiplexer Chooses the Winner for `O`
Now evaluate `assign O = M0 ? add_out : M1 ? xor_out : M2 ? nand_out : M3 ? func_out : 8'b00000000;`:
1. Is `M0 == 1`? **No** (`M0 = 0`), move to the next check.
2. Is `M1 == 1`? **Yes!** (`M1 = 1`), stop checking and connect **`xor_out`** (`11111111`) directly to **`O`**.

### Final Result
* **`O` = `11111111`** (which matches the expected output for Opcode `01`!).