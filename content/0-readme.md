# README First: CMSC 132 Verilog Project & Crash Course

This document provides an architectural overview of the CMSC 132 project (`main_project` and its sub-modules), instructions for running the simulation in Google Colab, and a beginner-friendly Verilog crash course mapped to equivalent Python concepts.

---

## 1. Project Overview

The project implements an **8-bit Arithmetic Logic Unit (ALU)** named `main_project` using purely combinational logic (no clock signals or memory registers).

* **Total Input (`I[17:0]`):** 18 wires bundled into a single input bus:
  * `I[17]` (`I0`) and `I[16]` (`I1`): **2-bit Opcode** (selects which operation is routed to the output).
  * `I[15:8]` (`I2`–`I9`): **Operand `A`** (first 8-bit binary number).
  * `I[7:0]` (`I10`–`I17`): **Operand `B`** (second 8-bit binary number).
* **Total Output (`O[7:0]`):** 8 wires carrying the final result (`O[7]` is MSB, `O[0]` is LSB).

| Opcode (`I[17:16]`) | Component | Operation | Formula |
| :---: | :--- | :--- | :--- |
| **`00`** | `full_adder_8bit` | 8-bit Addition | $O = A + B$ *(carry out dropped)* |
| **`01`** | `xor_8bit` | 8-bit Bitwise XOR | $O = A \oplus B$ |
| **`10`** | `nand_8bit` | 8-bit Bitwise NAND | $O = \text{NOT}(A \text{ AND } B)$ |
| **`11`** | `function_8bit` | 8-bit Custom Function (AB2L) | $O = \text{NOT}(A) - \text{NAND}(B, B)$ |

---

## 2. Project Hierarchy & Module Organization

The hardware design is structured hierarchically from 1-bit primitives up to the top-level ALU:

1. **Level 1 — 1-Bit Building Blocks:**
   * `full_adder_1bit`, `xor_1bit`, and `nand_1bit` implement single-bit operations using basic logic gates.
2. **Level 2 — 8-Bit Operations:**
   * `full_adder_8bit`, `xor_8bit`, and `nand_8bit` each instantiate **8 copies** of their corresponding 1-bit module side-by-side.
   * `subtractor_8bit` and `function_8bit` reuse `nand_8bit` and `full_adder_8bit` via two's complement arithmetic.
3. **Level 3 — Control & Routing (`main_project`):**
   * `decoder` reads the 2-bit opcode (`I[17:16]`) and asserts exactly one select line (`M0`, `M1`, `M2`, or `M3`).
   * `main_project` computes all four 8-bit operations **in parallel**, then uses a multiplexer (`? :`) to route only the selected result to `O`.
4. **Level 4 — Verification (`testbench`):**
   * Evaluates 8 directed test cases followed by an exhaustive loop testing **all 262,144 possible 18-bit inputs** ($2^{18} = 262,144$) against a behavioral reference model.

> **Google Colab Execution Note:** In Section `[6]` of the notebook, the cell magic command must be **`%%verilog`** to compile and run the testbench simulation. Using `%%print_verilog` attempts to render a static gate-level schematic of the entire cell and fails on simulation-only tasks such as `$display`.

---

## 3. Verilog Crash Course (Mapped to Python Concepts)

For developers familiar with Python, Verilog syntax shares many familiar operators, though its execution model reflects physical hardware rather than sequential software.

### Core Principle: Parallel Hardware vs. Sequential Execution
* **Python:** Statements execute **sequentially** from top to bottom, one instruction at a time.
* **Verilog:** Code describes **physical wires and logic gates**. All `assign` statements and instantiated sub-modules operate **simultaneously** whenever input signals change.

---

### Side-by-Side Comparison: Python vs. Verilog

#### 1. Functions vs. `module`
In Python, reusable logic is encapsulated inside a `def` function. In Verilog, reusable hardware blocks are defined inside a `module ... endmodule` block with explicitly declared input and output ports.

* **Python:**
  ```python
  def xor_1bit(a, b):
      q = a ^ b
      return q
  ```
* **Verilog:**
  ```verilog
  module xor_1bit (
    input a,
    input b,
    output q
  );
    assign q = a ^ b;
  endmodule
  ```
  * **`assign`**: Continuously drives the output wire `q` with the result of `a ^ b`, analogous to soldering a permanent physical connection.

---

#### 2. Calling a Function vs. Instantiating a Module
Calling a function in Python executes it once and returns a value. Instantiating a module in Verilog places a **distinct physical copy** of that circuit onto the design, assigns it a unique instance identifier (such as `x0`), and wires its ports to external signals.

* **Python:**
  ```python
  q0 = xor_1bit(a=a[0], b=b[0])
  ```
* **Verilog:**
  ```verilog
  xor_1bit x0 (
    .a(a[0]),
    .b(b[0]),
    .q(q[0])
  );
  ```
  * **`.a(a[0])`**: `.a` refers to the port defined **inside** `xor_1bit`, while `(a[0])` is the external wire connected to that port.

---

#### 3. Local Variables vs. `wire`
* **Python:** Intermediate values are stored in local variables in memory (e.g., `temp = a + b`).
* **Verilog:** Internal connections between sub-modules are declared as `wire` (single-bit) or `wire [7:0]` (8-bit bus):
  ```verilog
  wire c1;            // A single internal wire (1 bit)
  wire [7:0] not_a;   // A bundle of 8 internal wires (8 bits)
  ```

---

#### 4. Lists / Slicing vs. Bit Buses `[7:0]`
* **Python:** Sequences are indexed left-to-right starting at `0` (`A[0]` is the leftmost element).
* **Verilog:** Multi-bit hardware buses are typically indexed **right-to-left** from Most Significant Bit (MSB) down to Least Significant Bit (LSB):
  ```verilog
  input [17:0] I;         // 18 wires indexed 17 (leftmost/MSB) down to 0 (rightmost/LSB)
  wire [7:0] A = I[15:8]; // Extracts 8 wires from index 15 down to index 8
  wire [7:0] B = I[7:0];  // Extracts 8 wires from index 7 down to index 0
  ```
  * **`A[0]`** represents the **rightmost** bit ($2^0 = 1$).
  * **`A[7]`** represents the **leftmost** bit ($2^7 = 128$).

---

#### 5. Binary Literal Notation (`<width>'b<value>`)
* **Python:** Binary literals are written as `0b101` or `0`.
* **Verilog:** Constants specify the exact physical bus width using `<bit_width>'b<binary_digits>`:
  * `1'b0` $\rightarrow$ 1-bit wire tied to `0` (Ground / Logic Low).
  * `1'b1` $\rightarrow$ 1-bit wire tied to `1` (Power / Logic High).
  * `2'b10` $\rightarrow$ 2-bit bus carrying `1` and `0`.
  * `8'b00000000` $\rightarrow$ 8-bit bus with all wires set to `0`.

---

#### 6. Bitwise Logic Operators
Python and Verilog use identical symbols for standard bitwise operations:

| Logic Gate | Python | Verilog | Behavior |
| :--- | :---: | :---: | :--- |
| **NOT** (Inverter) | `~a` | `~a` | Inverts every bit (`0` $\rightarrow$ `1`, `1` $\rightarrow$ `0`) |
| **AND** | `a & b` | `a & b` | Outputs `1` only when both bits are `1` |
| **OR** | `a \| b` | `a \| b` | Outputs `1` when at least one bit is `1` |
| **XOR** | `a ^ b` | `a ^ b` | Outputs `1` when the two bits differ |
| **NAND** | `~(a & b)` | `~(a & b)` | Outputs `0` only when both bits are `1` |

---

#### 7. `if / elif / else` vs. The Multiplexer (`? :`)
Continuous `assign` statements cannot contain multi-line `if / else` blocks. Instead, Verilog uses the **ternary operator (`condition ? value_if_true : value_if_false`)** to build a hardware multiplexer, functioning like Python's inline conditional expression:

* **Python:**
  ```python
  O = (add_out  if M0 else
       xor_out  if M1 else
       nand_out if M2 else
       func_out if M3 else 0)
  ```
* **Verilog:**
  ```verilog
  assign O = M0 ? add_out  :
             M1 ? xor_out  :
             M2 ? nand_out :
             M3 ? func_out : 8'b00000000;
  ```

---

#### 8. `reg` vs. `wire` (Simulation vs. Hardware)
Both `reg` and `wire` appear inside `module testbench`:
* **`wire`**: Cannot hold state on its own; it must be continuously driven by an `assign` statement or a module output port.
* **`reg`**: Retains whatever value is assigned to it inside a procedural `initial begin ... end` block until updated, behaving like a standard variable during testbench simulation.

---

## 4. Recommended Reading Order for the Study Guides

1. **`README.md`** *(This file)*
2. **2-to-4 Decoder (`decoder`)** — How the 2-bit opcode asserts a single select line.
3. **8-Bit Full Adder (`full_adder_1bit` & `full_adder_8bit`)** — How 1-bit full adders chain together via ripple-carry wires.
4. **8-Bit XOR (`xor_1bit` & `xor_8bit`)** — How parallel bitwise operations execute independently.
5. **8-Bit NAND (`nand_1bit` & `nand_8bit`)** — How universal NAND gates operate bitwise.
6. **8-Bit FUNCTION (`subtractor_8bit` & `function_8bit`)** — How two's complement subtraction and NAND reuse implement $O = \text{NOT}(A) - \text{NAND}(B, B)$.
7. **Top-Level Integration (`main_project` & `testbench`)** — How all modules integrate and undergo exhaustive verification across all 262,144 input combinations.