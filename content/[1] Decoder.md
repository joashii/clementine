# Verilog Study Guide: 2-to-4 Decoder

## 1. The Complete Code

```verilog
// 2-to-4 decoder: I0 is the MSB of the opcode
module decoder (
  input I0,
  input I1,
  output M0,    // Full Adder Module
  output M1,    // XOR Module
  output M2,    // NAND Module
  output M3     // Function Module
);
  assign M0 = ~I0 & ~I1;   // opcode 00
  assign M1 = ~I0 & I1;    // opcode 01
  assign M2 = I0 & ~I1;    // opcode 10
  assign M3 = I0 & I1;     // opcode 11
endmodule
```

---

## 2. Basic Logic Gates Used (Truth Tables)

Before reading the code, here are the two basic logic gates used inside the `decoder` module:

### 1. NOT Gate (`~` — Inverter)
* **Rule:** Flips the input to its opposite (`0` becomes `1`, and `1` becomes `0`).

| `X` | `~X` |
| :-: | :---: |
| 0 | **1** |
| 1 | **0** |

### 2. AND Gate (`&`)
* **Rule:** Outputs `1` **only if both** inputs are `1`. If either input is `0`, the output is `0`.

| `X` | `Y` | `X & Y` |
| :-: | :-: | :---: |
| 0 | 0 | **0** |
| 0 | 1 | **0** |
| 1 | 0 | **0** |
| 1 | 1 | **1** |

---

## 3. Module-by-Module & Line-by-Line Breakdown

This code contains **1 module**:
1. **`decoder`**: A 2-to-4 selector chip that reads the 2-bit opcode (`I0` and `I1`) and turns ON (`1`) **only one** of the four control wires (`M0`, `M1`, `M2`, or `M3`) while keeping the other three OFF (`0`).

---

### Module 1: `decoder`

#### Code
```verilog
// 2-to-4 decoder: I0 is the MSB of the opcode
module decoder (
  input I0,
  input I1,
  output M0,    // Full Adder Module
  output M1,    // XOR Module
  output M2,    // NAND Module
  output M3     // Function Module
);
  assign M0 = ~I0 & ~I1;   // opcode 00
  assign M1 = ~I0 & I1;    // opcode 01
  assign M2 = I0 & ~I1;    // opcode 10
  assign M3 = I0 & I1;     // opcode 11
endmodule
```

#### Line-by-Line Explanation
* **`// 2-to-4 decoder: I0 is the MSB of the opcode`**
  * A comment for humans. It notes that this chip takes **2** input wires and controls **4** output wires, with `I0` acting as the **M**ost **S**ignificant **B**it (the left digit of the 2-bit opcode).
* **`module decoder (`**
  * Starts defining a new hardware chip (called a `module`) named `decoder`. The opening parenthesis `(` begins the list of pins sticking out of this chip.
* **`  input I0,`**
  * Declares a 1-bit input pin named `I0` (the first/left bit of the opcode). The comma `,` separates it from the next pin.
* **`  input I1,`**
  * Declares a 1-bit input pin named `I1` (the second/right bit of the opcode).
* **`  output M0,    // Full Adder Module`**
  * Declares a 1-bit output pin named `M0`. When `M0 = 1`, it signals `main_project` to select the **Full Adder** result.
* **`  output M1,    // XOR Module`**
  * Declares a 1-bit output pin named `M1`. When `M1 = 1`, it signals `main_project` to select the **XOR** result.
* **`  output M2,    // NAND Module`**
  * Declares a 1-bit output pin named `M2`. When `M2 = 1`, it signals `main_project` to select the **NAND** result.
* **`  output M3     // Function Module`**
  * Declares a 1-bit output pin named `M3`. When `M3 = 1`, it signals `main_project` to select the **AB2L Function** result. *(Notice there is no comma here because it is the last pin in the list.)*
* **` );`**
  * Closes the list of input/output pins for this chip. Every module header ends with a semicolon `;`.
* **`  assign M0 = ~I0 & ~I1;   // opcode 00`**
  * **`assign`** acts like soldering a permanent wire between logic gates and an output pin.
  * **`~`** is the **NOT** gate; **`&`** is the **AND** gate.
  * This flips both `I0` and `I1`, then ANDs them together so `M0` becomes `1` **only when `I0 = 0` and `I1 = 0`** (opcode `00`).
* **`  assign M1 = ~I0 & I1;    // opcode 01`**
  * Flips `I0` and keeps `I1` as is, then ANDs them so `M1` becomes `1` **only when `I0 = 0` and `I1 = 1`** (opcode `01`).
* **`  assign M2 = I0 & ~I1;    // opcode 10`**
  * Keeps `I0` as is and flips `I1`, then ANDs them so `M2` becomes `1` **only when `I0 = 1` and `I1 = 0`** (opcode `10`).
* **`  assign M3 = I0 & I1;     // opcode 11`**
  * Keeps both `I0` and `I1` as is and ANDs them so `M3` becomes `1` **only when `I0 = 1` and `I1 = 1`** (opcode `11`).
* **`endmodule`**
  * Marks the end of the `decoder` chip. *(Note: `endmodule` never takes a semicolon.)*

---

## 4. How Did We Arrive at the Formulas?

The formulas inside `decoder` come from standard **Binary-to-One-Hot Decoding** (Minterms) in digital logic.

### Step 1: Build the 2-to-4 Decoder Truth Table
With 2 input bits (`I0` and `I1`), there are $2^2 = 4$ possible binary combinations (`00`, `01`, `10`, `11`). We want each combination to turn ON (`1`) its own dedicated output wire while forcing all other output wires to `0`:

| Opcode (`I0` `I1`) | Selected Operation | `M0` (Adder) | `M1` (XOR) | `M2` (NAND) | `M3` (Func) |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **`0` `0`** | Full Adder | **1** | 0 | 0 | 0 |
| **`0` `1`** | XOR | 0 | **1** | 0 | 0 |
| **`1` `0`** | NAND | 0 | 0 | **1** | 0 |
| **`1` `1`** | AB2L Function | 0 | 0 | 0 | **1** |

### Step 2: Turn Each Row into an AND Formula (Minterm)
Remember that an **AND (`&`) gate** outputs `1` **only when every input feeding into it is `1`**.
* If an input bit is supposed to be **`1`**, we feed it directly into the AND gate (`I0` or `I1`).
* If an input bit is supposed to be **`0`**, we pass it through a **NOT (`~`) gate** first (`~I0` or `~I1`) so that a `0` turns into a `1` before entering the AND gate!

Applying that rule to each row of the table gives the exact 4 equations:
1. **Row `00` (`I0 = 0, I1 = 0`):** Both inputs are `0`, so invert both $\rightarrow$ **`M0 = ~I0 & ~I1`**
2. **Row `01` (`I0 = 0, I1 = 1`):** `I0` is `0` (invert it) and `I1` is `1` (keep it) $\rightarrow$ **`M1 = ~I0 & I1`**
3. **Row `10` (`I0 = 1, I1 = 0`):** `I0` is `1` (keep it) and `I1` is `0` (invert it) $\rightarrow$ **`M2 = I0 & ~I1`**
4. **Row `11` (`I0 = 1, I1 = 1`):** Both inputs are `1`, so keep both $\rightarrow$ **`M3 = I0 & I1`**

---

## 5. Elaborated Step-by-Step Example (Selecting NAND with Opcode `10`)

Suppose we send an 18-bit input `I` into `main_project` where the first two opcode bits are **`10`** (meaning we want the **NAND** operation):
* **`I0` (`I[17]`) = `1`**
* **`I1` (`I[16]`) = `0`**

Because `decoder` is physical hardware, **all four `assign` lines evaluate at the exact same instant**. Let's trace what happens on each of the four output wires:

---

### Step 1: Evaluate `M0` (`~I0 & ~I1`)
* Plug in `I0 = 1` and `I1 = 0`:
  * `~1 & ~0`
* Apply the **NOT (`~`)** gates first:
  * `~1` becomes **`0`**, and `~0` becomes **`1`**.
* Apply the **AND (`&`)** gate:
  * `0 & 1` = **`0`**
* **Result:** **`M0 = 0`** (Full Adder path is **OFF**).

---

### Step 2: Evaluate `M1` (`~I0 & I1`)
* Plug in `I0 = 1` and `I1 = 0`:
  * `~1 & 0`
* Apply the **NOT (`~`)** gate:
  * `~1` becomes **`0`**.
* Apply the **AND (`&`)** gate:
  * `0 & 0` = **`0`**
* **Result:** **`M1 = 0`** (XOR path is **OFF**).

---

### Step 3: Evaluate `M2` (`I0 & ~I1`)
* Plug in `I0 = 1` and `I1 = 0`:
  * `1 & ~0`
* Apply the **NOT (`~`)** gate:
  * `~0` becomes **`1`**.
* Apply the **AND (`&`)** gate:
  * `1 & 1` = **`1`**
* **Result:** **`M2 = 1`** (NAND path is **ON!**).

---

### Step 4: Evaluate `M3` (`I0 & I1`)
* Plug in `I0 = 1` and `I1 = 0`:
  * `1 & 0`
* Apply the **AND (`&`)** gate:
  * `1 & 0` = **`0`**
* **Result:** **`M3 = 0`** (AB2L Function path is **OFF**).

---

### Final Result Summary & How `main_project` Uses It

| Output Wire | Formula | Plugged-in Values (`I0=1, I1=0`) | Simplified | Final Signal | Status |
| :--- | :--- | :---: | :---: | :---: | :--- |
| **`M0`** | `~I0 & ~I1` | `~1 & ~0` | `0 & 1` | **`0`** | OFF |
| **`M1`** | `~I0 & I1` | `~1 & 0` | `0 & 0` | **`0`** | OFF |
| **`M2`** | `I0 & ~I1` | `1 & ~0` | `1 & 1` | **`1`** | **ON (Selected)** |
| **`M3`** | `I0 & I1` | `1 & 0` | `1 & 0` | **`0`** | OFF |

* **How `main_project` reacts:** Down in `main_project`, the multiplexer checks `M0 ? add_out : M1 ? xor_out : M2 ? nand_out : ...`. Because `M0` is `0`, `M1` is `0`, and **`M2` is `1`**, the multiplexer routes **`nand_out`** directly to the final output bus `O`!