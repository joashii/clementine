# Verilog Study Guide: 8-Bit Full Adder

## 1. The Complete Code

```verilog
// 1-bit full adder module
module full_adder_1bit (
  input a,
  input b,
  input cin,
  output sum,
  output cout
);
  // calculate sum output
  assign sum = a ^ b ^ cin;
  // calculate carry output
  assign cout = (a & b) | (cin & (a ^ b));
endmodule

// 8-bit full adder module (8 connected 1-bit full adders)
module full_adder_8bit (
  input [7:0] a,
  input [7:0] b,
  input cin,
  output [7:0] sum,
  output cout
);
  // declare carry wires
  wire c1, c2, c3, c4, c5, c6, c7;

  // connect individual adders
  full_adder_1bit fa0 (.a(a[0]), .b(b[0]), .cin(cin), .sum(sum[0]), .cout(c1));
  full_adder_1bit fa1 (.a(a[1]), .b(b[1]), .cin(c1),  .sum(sum[1]), .cout(c2));
  full_adder_1bit fa2 (.a(a[2]), .b(b[2]), .cin(c2),  .sum(sum[2]), .cout(c3));
  full_adder_1bit fa3 (.a(a[3]), .b(b[3]), .cin(c3),  .sum(sum[3]), .cout(c4));
  full_adder_1bit fa4 (.a(a[4]), .b(b[4]), .cin(c4),  .sum(sum[4]), .cout(c5));
  full_adder_1bit fa5 (.a(a[5]), .b(b[5]), .cin(c5),  .sum(sum[5]), .cout(c6));
  full_adder_1bit fa6 (.a(a[6]), .b(b[6]), .cin(c6),  .sum(sum[6]), .cout(c7));

  // final carry output
  full_adder_1bit fa7 (.a(a[7]), .b(b[7]), .cin(c7),  .sum(sum[7]), .cout(cout));
endmodule
```

---

## 2. Basic Logic Gates Used (Truth Tables)

Before reading the code, here are the three basic logic gates used inside `full_adder_1bit`:

### 1. AND Gate (`&`)
* **Rule:** Outputs `1` **only if both** inputs are `1`.

| `X` | `Y` | `X & Y` |
| :-: | :-: | :---: |
| 0 | 0 | **0** |
| 0 | 1 | **0** |
| 1 | 0 | **0** |
| 1 | 1 | **1** |

### 2. OR Gate (`|`)
* **Rule:** Outputs `1` if **at least one** input is `1`.

| `X` | `Y` | `X \| Y` |
| :-: | :-: | :---: |
| 0 | 0 | **0** |
| 0 | 1 | **1** |
| 1 | 0 | **1** |
| 1 | 1 | **1** |

### 3. XOR Gate (`^` — Exclusive OR)
* **Rule:** Outputs `1` **only if the two inputs are different** (one is `1` and the other is `0`).

| `X` | `Y` | `X ^ Y` |
| :-: | :-: | :---: |
| 0 | 0 | **0** |
| 0 | 1 | **1** |
| 1 | 0 | **1** |
| 1 | 1 | **0** |

---

## 3. Module-by-Module & Line-by-Line Breakdown

This code is divided into **2 modules**:
1. **`full_adder_1bit`**: A tiny building block that adds just two single bits (`1` or `0`) plus a carry-in.
2. **`full_adder_8bit`**: A larger chip that snaps 8 copies of that 1-bit block side-by-side to add 8-bit numbers.

---

### Module 1: `full_adder_1bit`

#### Code
```verilog
// 1-bit full adder module
module full_adder_1bit (
  input a,
  input b,
  input cin,
  output sum,
  output cout
);
  // calculate sum output
  assign sum = a ^ b ^ cin;
  // calculate carry output
  assign cout = (a & b) | (cin & (a ^ b));
endmodule
```

#### Line-by-Line Explanation
* **`// 1-bit full adder module`**
  * A comment for humans. Verilog completely ignores anything after `//`.
* **`module full_adder_1bit (`**
  * Starts defining a new hardware chip (called a `module`) named `full_adder_1bit`. The opening parenthesis `(` begins the list of pins sticking out of this chip.
* **`  input a,`**
  * Declares a 1-bit input pin named `a` (the first binary digit to add). The comma `,` separates it from the next pin.
* **`  input b,`**
  * Declares a 1-bit input pin named `b` (the second binary digit to add).
* **`  input cin,`**
  * Declares a 1-bit input pin named `cin` (**c**arry-**in**), which receives any leftover `1` carried over from a previous addition.
* **`  output sum,`**
  * Declares a 1-bit output pin named `sum` (the main answer digit for this column).
* **`  output cout`**
  * Declares a 1-bit output pin named `cout` (**c**arry-**out**), which sends a `1` to the next column on the left if the sum is `2` or `3`. *(Notice there is no comma here because it is the last pin in the list.)*
* **` );`**
  * Closes the list of input/output pins for this chip. Every declaration header in Verilog ends with a semicolon `;`.
* **`  // calculate sum output`**
  * A comment explaining that the next line computes the `sum` pin.
* **`  assign sum = a ^ b ^ cin;`**
  * **`assign`** acts like soldering a permanent wire between logic gates and an output pin.
  * **`^`** is the **XOR** (Exclusive OR) gate. It calculates `a XOR b XOR cin` and sends the result continuously to the `sum` pin.
* **`  // calculate carry output`**
  * A comment explaining that the next line computes the `cout` pin.
* **`  assign cout = (a & b) | (cin & (a ^ b));`**
  * **`&`** is the **AND** gate; **`|`** is the **OR** gate; **`^`** is the **XOR** gate.
  * This wires up AND, OR, and XOR gates to check if at least two inputs are `1`, sending the result to the `cout` pin.
* **`endmodule`**
  * Marks the end of the `full_adder_1bit` chip. *(Note: `endmodule` never takes a semicolon.)*

---

### Module 2: `full_adder_8bit`

#### Code
```verilog
// 8-bit full adder module (8 connected 1-bit full adders)
module full_adder_8bit (
  input [7:0] a,
  input [7:0] b,
  input cin,
  output [7:0] sum,
  output cout
);
  // declare carry wires
  wire c1, c2, c3, c4, c5, c6, c7;

  // connect individual adders
  full_adder_1bit fa0 (.a(a[0]), .b(b[0]), .cin(cin), .sum(sum[0]), .cout(c1));
  full_adder_1bit fa1 (.a(a[1]), .b(b[1]), .cin(c1),  .sum(sum[1]), .cout(c2));
  full_adder_1bit fa2 (.a(a[2]), .b(b[2]), .cin(c2),  .sum(sum[2]), .cout(c3));
  full_adder_1bit fa3 (.a(a[3]), .b(b[3]), .cin(c3),  .sum(sum[3]), .cout(c4));
  full_adder_1bit fa4 (.a(a[4]), .b(b[4]), .cin(c4),  .sum(sum[4]), .cout(c5));
  full_adder_1bit fa5 (.a(a[5]), .b(b[5]), .cin(c5),  .sum(sum[5]), .cout(c6));
  full_adder_1bit fa6 (.a(a[6]), .b(b[6]), .cin(c6),  .sum(sum[6]), .cout(c7));

  // final carry output
  full_adder_1bit fa7 (.a(a[7]), .b(b[7]), .cin(c7),  .sum(sum[7]), .cout(cout));
endmodule
```

#### Line-by-Line Explanation
* **`// 8-bit full adder module (8 connected 1-bit full adders)`**
  * A comment describing the second module.
* **`module full_adder_8bit (`**
  * Starts defining the larger 8-bit adder chip named `full_adder_8bit`.
* **`  input [7:0] a,`**
  * Declares an **8-bit input bus** (a bundle of 8 wires) named `a`. `[7:0]` means the wires are numbered from `7` (leftmost/MSB) down to `0` (rightmost/LSB).
* **`  input [7:0] b,`**
  * Declares an **8-bit input bus** named `b` (the second 8-bit number to add).
* **`  input cin,`**
  * Declares a single 1-bit input pin named `cin` for the initial carry-in entering the rightmost bit.
* **`  output [7:0] sum,`**
  * Declares an **8-bit output bus** named `sum` that will hold the final 8-bit answer.
* **`  output cout`**
  * Declares a single 1-bit output pin named `cout` for the final carry-out overflowing from the leftmost bit (`[7]`).
* **` );`**
  * Closes the pin list for `full_adder_8bit`.
* **`  // declare carry wires`**
  * A comment introducing the internal connections.
* **`  wire c1, c2, c3, c4, c5, c6, c7;`**
  * Creates 7 internal single-bit wires (`c1` through `c7`). Unlike `input` and `output`, `wire`s stay strictly inside the chip—they act like bridges passing the carry from one 1-bit adder to the next.
* **`  // connect individual adders`**
  * A comment introducing the 1-bit adder copies.
* **`  full_adder_1bit fa0 (.a(a[0]), .b(b[0]), .cin(cin), .sum(sum[0]), .cout(c1));`**
  * Places the first copy of `full_adder_1bit` and gives this copy a unique name: **`fa0`** (for bit `0`, the rightmost bit).
  * **`.a(a[0])`**: Connects bit `0` of input bus `a` to the `.a` pin of `fa0`.
  * **`.b(b[0])`**: Connects bit `0` of input bus `b` to the `.b` pin of `fa0`.
  * **`.cin(cin)`**: Connects the main external `cin` input pin to the `.cin` pin of `fa0`.
  * **`.sum(sum[0])`**: Sends `fa0`'s `.sum` output to bit `0` of the `sum` output bus.
  * **`.cout(c1)`**: Sends `fa0`'s `.cout` carry-out into internal wire `c1`.
* **`  full_adder_1bit fa1 (.a(a[1]), .b(b[1]), .cin(c1),  .sum(sum[1]), .cout(c2));`**
  * Places copy **`fa1`** for bit `1`. Notice **`.cin(c1)`**—it receives the carry `c1` produced by `fa0`, and sends its own carry out to wire **`c2`**.
* **`  full_adder_1bit fa2 (.a(a[2]), .b(b[2]), .cin(c2),  .sum(sum[2]), .cout(c3));`**
  * Places copy **`fa2`** for bit `2`. Takes carry `c2` from `fa1` and passes its carry to `c3`.
* **`  full_adder_1bit fa3 (.a(a[3]), .b(b[3]), .cin(c3),  .sum(sum[3]), .cout(c4));`**
  * Places copy **`fa3`** for bit `3`. Takes carry `c3` from `fa2` and passes its carry to `c4`.
* **`  full_adder_1bit fa4 (.a(a[4]), .b(b[4]), .cin(c4),  .sum(sum[4]), .cout(c5));`**
  * Places copy **`fa4`** for bit `4`. Takes carry `c4` from `fa3` and passes its carry to `c5`.
* **`  full_adder_1bit fa5 (.a(a[5]), .b(b[5]), .cin(c5),  .sum(sum[5]), .cout(c6));`**
  * Places copy **`fa5`** for bit `5`. Takes carry `c5` from `fa4` and passes its carry to `c6`.
* **`  full_adder_1bit fa6 (.a(a[6]), .b(b[6]), .cin(c6),  .sum(sum[6]), .cout(c7));`**
  * Places copy **`fa6`** for bit `6`. Takes carry `c6` from `fa5` and passes its carry to `c7`.
* **`  // final carry output`**
  * A comment noting the last block in the chain.
* **`  full_adder_1bit fa7 (.a(a[7]), .b(b[7]), .cin(c7),  .sum(sum[7]), .cout(cout));`**
  * Places the final copy **`fa7`** for bit `7` (the leftmost bit). Takes carry `c7` from `fa6`, outputs `sum[7]`, and connects its carry-out directly to the main external **`cout`** pin of the 8-bit adder.
* **`endmodule`**
  * Marks the end of the `full_adder_8bit` chip.

---

## 4. How Did We Arrive at the Formulas?

The formulas inside `full_adder_1bit` were **not** invented for Verilog—they come from standard **Binary Addition and Truth Tables** in digital logic.

### Step 1: List All Possible 1-Bit Additions (Full Adder Truth Table)
When you add three 1-bit numbers (`a`, `b`, and `cin`), the biggest possible result is $1 + 1 + 1 = 3$ (which is `11` in binary: `cout = 1`, `sum = 1`). Here are all 8 possible combinations:

| `a` | `b` | `cin` | Decimal Total | `cout` (Carry) | `sum` (Digit) |
| :-: | :-: | :-: | :---: | :---: | :---: |
| 0 | 0 | 0 | 0 | **0** | **0** |
| 0 | 0 | 1 | 1 | **0** | **1** |
| 0 | 1 | 0 | 1 | **0** | **1** |
| 0 | 1 | 1 | 2 | **1** | **0** |
| 1 | 0 | 0 | 1 | **0** | **1** |
| 1 | 0 | 1 | 2 | **1** | **0** |
| 1 | 1 | 0 | 2 | **1** | **0** |
| 1 | 1 | 1 | 3 | **1** | **1** |

### Step 2: Deriving the `sum` Formula
Look at the **`sum`** column in the table above. When is `sum` equal to `1`?
* Only when the decimal total is **1** (one `1` among the inputs) or **3** (three `1`s among the inputs).
* In other words, `sum = 1` whenever there is an **odd number of `1`s**.
* In digital logic, the **XOR (`^`)** gate is an "odd-1s detector." Chaining two XOR gates together checks if `a`, `b`, and `cin` have an odd number of `1`s:
  ```verilog
  sum = a ^ b ^ cin
  ```

### Step 3: Deriving the `cout` Formula
Now look at the **`cout`** column in the table above. When do you need to carry a `1` (`cout = 1`)?
* Only when the decimal total is **2 or 3**—meaning **at least two of the three inputs (`a`, `b`, `cin`) are `1`**.
* There are two ways this can happen:
  1. **`a` and `b` are BOTH `1`**: That gives a carry right away, regardless of `cin`. In logic gates, that is **`(a & b)`**.
  2. **Exactly ONE of `a` or `b` is `1` (`a ^ b`), AND `cin` is also `1`**: That makes two `1`s total, which also produces a carry. In logic gates, that is **`(cin & (a ^ b))`**.
* Combine those two possibilities with an **OR (`|`)** gate ("either case 1 happens OR case 2 happens"):
  ```verilog
  cout = (a & b) | (cin & (a ^ b))
  ```

---

## 5. Elaborated Step-by-Step Example (`15 + 1 = 16`)

Let's trace what happens inside `full_adder_8bit` when we add:
* **`a` = `00001111`** ($15$ in decimal)
* **`b` = `00000001`** ($1$ in decimal)
* **`cin` = `1'b0`** ($0$, since we are doing normal addition)

First, here is how each individual bit of `a` and `b` maps to its wire index (`[7]` is leftmost, `[0]` is rightmost):

| Bit Index | `[7]` | `[6]` | `[5]` | `[4]` | `[3]` | `[2]` | `[1]` | `[0]` |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **`a` (`15`)** | `0` | `0` | `0` | `0` | `1` | `1` | `1` | `1` |
| **`b` (`1`)** | `0` | `0` | `0` | `0` | `0` | `0` | `0` | `1` |

Now, let's watch the electricity ripple from **right to left** (`fa0` $\rightarrow$ `fa7`) using the two formulas:
* `sum = a ^ b ^ cin`
* `cout = (a & b) | (cin & (a ^ b))`

---

### Step 1: `fa0` (Rightmost Bit — Index `[0]`)
* **Inputs:** `a[0] = 1`, `b[0] = 1`, `cin = 0`
* **Calculate `sum[0]`:**
  * `1 ^ 1 ^ 0` = `0 ^ 0` = **`0`**
* **Calculate `c1` (carry-out):**
  * `(1 & 1) | (0 & (1 ^ 1))` = `1 | (0 & 0)` = `1 | 0` = **`1`**
* **What happens next:** `sum[0]` becomes **`0`**, and internal wire **`c1` carries `1`** into `fa1`.

---

### Step 2: `fa1` (Index `[1]`)
* **Inputs:** `a[1] = 1`, `b[1] = 0`, `cin = c1` (`1`)
* **Calculate `sum[1]`:**
  * `1 ^ 0 ^ 1` = `1 ^ 1` = **`0`**
* **Calculate `c2` (carry-out):**
  * `(1 & 0) | (1 & (1 ^ 0))` = `0 | (1 & 1)` = `0 | 1` = **`1`**
* **What happens next:** `sum[1]` becomes **`0`**, and internal wire **`c2` carries `1`** into `fa2`.

---

### Step 3: `fa2` (Index `[2]`)
* **Inputs:** `a[2] = 1`, `b[2] = 0`, `cin = c2` (`1`)
* **Calculate `sum[2]`:**
  * `1 ^ 0 ^ 1` = `1 ^ 1` = **`0`**
* **Calculate `c3` (carry-out):**
  * `(1 & 0) | (1 & (1 ^ 0))` = `0 | (1 & 1)` = **`1`**
* **What happens next:** `sum[2]` becomes **`0`**, and internal wire **`c3` carries `1`** into `fa3`.

---

### Step 4: `fa3` (Index `[3]`)
* **Inputs:** `a[3] = 1`, `b[3] = 0`, `cin = c3` (`1`)
* **Calculate `sum[3]`:**
  * `1 ^ 0 ^ 1` = `1 ^ 1` = **`0`**
* **Calculate `c4` (carry-out):**
  * `(1 & 0) | (1 & (1 ^ 0))` = `0 | (1 & 1)` = **`1`**
* **What happens next:** `sum[3]` becomes **`0`**, and internal wire **`c4` carries `1`** into `fa4`.

---

### Step 5: `fa4` (Index `[4]`)
* **Inputs:** `a[4] = 0`, `b[4] = 0`, `cin = c4` (`1`)
* **Calculate `sum[4]`:**
  * `0 ^ 0 ^ 1` = `0 ^ 1` = **`1`**
* **Calculate `c5` (carry-out):**
  * `(0 & 0) | (1 & (0 ^ 0))` = `0 | (1 & 0)` = `0 | 0` = **`0`**
* **What happens next:** `sum[4]` absorbs the carried `1` and becomes **`1`**, and the carry ripple stops (`c5 = 0`).

---

### Step 6: `fa5` (Index `[5]`)
* **Inputs:** `a[5] = 0`, `b[5] = 0`, `cin = c5` (`0`)
* **Calculate `sum[5]`:** `0 ^ 0 ^ 0` = **`0`**
* **Calculate `c6`:** `(0 & 0) | (0 & (0 ^ 0))` = **`0`**

---

### Step 7: `fa6` (Index `[6]`)
* **Inputs:** `a[6] = 0`, `b[6] = 0`, `cin = c6` (`0`)
* **Calculate `sum[6]`:** `0 ^ 0 ^ 0` = **`0`**
* **Calculate `c7`:** `(0 & 0) | (0 & (0 ^ 0))` = **`0`**

---

### Step 8: `fa7` (Leftmost Bit — Index `[7]`)
* **Inputs:** `a[7] = 0`, `b[7] = 0`, `cin = c7` (`0`)
* **Calculate `sum[7]`:** `0 ^ 0 ^ 0` = **`0`**
* **Calculate `cout` (final external carry-out):** `(0 & 0) | (0 & (0 ^ 0))` = **`0`**

---

### Final Result Summary Table

| Block | Bit Index | `a[i]` | `b[i]` | Incoming Carry | **`sum[i]`** | Outgoing Carry |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **`fa7`** (MSB) | `[7]` | `0` | `0` | `c7 = 0` | **`0`** | `cout = 0` |
| **`fa6`** | `[6]` | `0` | `0` | `c6 = 0` | **`0`** | `c7 = 0` |
| **`fa5`** | `[5]` | `0` | `0` | `c5 = 0` | **`0`** | `c6 = 0` |
| **`fa4`** | `[4]` | `0` | `0` | `c4 = 1` | **`1`** | `c5 = 0` |
| **`fa3`** | `[3]` | `1` | `0` | `c3 = 1` | **`0`** | `c4 = 1` |
| **`fa2`** | `[2]` | `1` | `0` | `c2 = 1` | **`0`** | `c3 = 1` |
| **`fa1`** | `[1]` | `1` | `0` | `c1 = 1` | **`0`** | `c2 = 1` |
| **`fa0`** (LSB) | `[0]` | `1` | `1` | `cin = 0` | **`0`** | `c1 = 1` |

* **Final 8-bit `sum` (`sum[7:0]`):** Reading from top (`[7]`) to bottom (`[0]`) gives **`00010000`**, which is **$16$** in decimal!
* **Final `cout`:** **`0`** (no overflow past 8 bits).