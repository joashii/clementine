# Verilog for Beginners: Understanding the CMSC 132 Project

A tutorial for someone who has never written Verilog, built around the CMSC 132 project (lab section **AB2L**) and the group notebook you uploaded. Every code example here was compiled and run in a simulator before going in.

**Contents**

1. [What the project asks for](#1-what-the-project-asks-for)
2. [The right mindset: hardware, not software](#2-the-right-mindset-hardware-not-software)
3. [Verilog crash course](#3-verilog-crash-course)
4. [Binary and two's complement (needed for AB2L)](#4-binary-and-twos-complement-needed-for-ab2l)
5. [Walking through the project, component by component](#5-walking-through-the-project-component-by-component)
6. [Test benches](#6-test-benches)
7. [Running Verilog in Google Colab](#7-running-verilog-in-google-colab)
8. [The AB2L spec example doesn't match the formula](#8-the-ab2l-spec-example-doesnt-match-the-formula)
9. [Common beginner errors](#9-common-beginner-errors)
10. [Preparing for the Q&A](#10-preparing-for-the-qa)
11. [Practice exercises](#11-practice-exercises)

---

## 1. What the project asks for

You build one circuit, `main_project`:

- **Input:** 18 bits
- **Output:** 8 bits
- **No registers** and no clock. The output depends only on the current input (this is *combinational* logic).

The 18 input bits are split like this (the spec numbers them I0 to I17, left to right):

| Bits | Meaning |
|---|---|
| I0, I1 | **Opcode** (2 bits, I0 is the most significant) |
| I2 to I9 | Operand **A** (8 bits) |
| I10 to I17 | Operand **B** (8 bits) |

The opcode picks which of four operations runs on A and B:

```
              +-----------+
 I0,I1 -----> |  DECODER  | --> one of 4 select lines turns on
              +-----------+
                    |
   A, B ----+-------+-----------+-------------+
            |       |           |             |
       +--------+ +-----+   +--------+   +----------+
       | 8-bit  | | 8-bit|  | 8-bit  |   | 8-bit    |
       | ADDER  | | XOR  |  | NAND   |   | FUNCTION |
       +--------+ +-----+   +--------+   +----------+
            \        |          |             /
             +-------+----------+------------+
                            |
                  pick the one the decoder
                  selected  -->  8-bit output O
```

The five components the spec wants:

1. **Decoder** (turns the 2 opcode bits into 4 select lines)
2. **8-bit full adder**, built from eight 1-bit full adders
3. **8-bit XOR**, built from eight 1-bit XORs
4. **8-bit NAND**, built from eight 1-bit NANDs
5. **8-bit FUNCTION**, specific to your lab section. For **AB2L**: `O = NOT(A) - NAND(B, B)`

Rules worth remembering:

- Components 2 to 5 must have their own **sub-components** (e.g. the 8-bit adder is eight connected 1-bit adders). You can't just write `a + b`.
- **Which opcode maps to which component is up to you.**
- Only **one test bench** (for the whole `main_project`) is submitted, and it must activate at least one of the components.
- Submit the Colab notebook with inline comments, text documentation, APA references (including people who helped you), and the list of tasks per member.
- Deadline: **November 15, 2026, 11:59 PM PHT**. Late means no submission.
- Grading is 30 points: **A** correctness (10), **B** documentation and attribution (10), **C** testing (10). You may be quizzed on how your code works and why tests passed or failed.

---

## 2. The right mindset: hardware, not software

If you know Python or Java, forget the idea that code runs line by line.

- A Verilog file **describes a circuit**. Every `assign` and every module instance is a piece of hardware that physically exists.
- All the pieces work **at the same time**. Order of the lines in a module mostly doesn't matter, the same way the order you draw gates on paper doesn't matter.
- You don't "call a function". You **instantiate** a module, which is like placing a chip on a breadboard and wiring its pins.
- When an input changes, the outputs update after the signals propagate. That is why the test bench waits (`#10`) before reading the output.

You already know this from the lab: Verilog is a text version of a logic diagram.

---

## 3. Verilog crash course

### 3.1 Modules

A **module** is a box with pins. It is the basic building block.

```verilog
module logic_function (
  input  B,
  input  C,
  input  D,
  input  E,
  output A
);

  assign A = (~B & C) | (~D & E);

endmodule
```

- `module name ( ... );` opens the box. `endmodule` closes it.
- Inside the parentheses are the **ports** (pins), each marked `input` or `output`.
- `assign A = ...;` says: "A is permanently wired to this expression." This is the line that describes the logic.

This is exactly the handout's example, `A = (NOT(B) AND C) + (NOT(D) AND E)`, where the `+` in that formula means OR.

### 3.2 Operators you will use

| Verilog | Meaning | Example |
|---|---|---|
| `~` | NOT (bitwise) | `~a` |
| `&` | AND (bitwise) | `a & b` |
| `\|` | OR (bitwise) | `a \| b` |
| `^` | XOR (bitwise) | `a ^ b` |
| `~(a & b)` | NAND | there is no single NAND symbol, so wrap it |
| `+` `-` | arithmetic | `a - b` |
| `? :` | pick one of two values (a mux) | `sel ? y : x` |

Bitwise operators work on **every bit at once** when used on multi-bit values. `a & b` on two 8-bit values ANDs each pair of matching bits.

### 3.3 Wires and vectors

A single wire is 1 bit. For multiple bits, use a **range** (a vector):

```verilog
wire       one_bit;
wire [7:0] eight_bits;   // 8 wires: eight_bits[7] ... eight_bits[0]
```

- `[7:0]` means 8 bits, with bit 7 as the **most significant** (leftmost) and bit 0 the least significant.
- Pick a single bit: `eight_bits[3]`
- Pick a slice: `eight_bits[7:4]` (the top 4 bits)
- **Concatenate** (glue together) with `{ }`: `{v[3:0], v[7:4]}` swaps the two halves of `v`.

### 3.4 Number literals

Format: `<width>'<base><digits>`

| Literal | Meaning |
|---|---|
| `1'b1` | a 1-bit binary 1 |
| `8'b10000000` | an 8-bit binary value |
| `8'd10` | an 8-bit decimal 10 |
| `2'b01` | a 2-bit binary value |

Underscores are allowed for readability: `8'b1010_0011`.

### 3.5 `wire` vs `reg`

- **`wire`**: a connection. Driven by an `assign` or a module output. Used everywhere in the circuit itself.
- **`reg`**: a variable that holds a value inside procedural code (`initial` / `always` blocks). In this project, you only use `reg` in the **test bench**, to hold inputs you set by hand.

Rule of thumb for this project: circuit modules use `input`, `output`, `wire`. The test bench uses `reg` (things you drive) and `wire` (things you observe).

### 3.6 Building big things from small things (instantiation)

This is the core skill for the project. Define a small module once, then place copies of it:

```verilog
module half_adder (
  input  a,
  input  b,
  output sum,
  output carry
);
  assign sum   = a ^ b;
  assign carry = a & b;
endmodule
```

Using it inside another module:

```verilog
half_adder ha (.a(x), .b(y), .sum(s), .carry(c));
```

Read this as:

- `half_adder` is the **type** of module you are placing.
- `ha` is the **name of this copy** (called an *instance*). Every copy needs a unique name.
- `.a(x)` means "connect the module's pin `a` to my signal `x`". This is **named port connection**: the dot-name is the pin inside the module, the value in parentheses is the signal on the outside. Using names means the order doesn't matter, and it prevents wiring mistakes.

Your project uses this everywhere: `full_adder_8bit` instantiates eight `full_adder_1bit` modules, `main_project` instantiates the decoder and all four components, and so on.

### 3.7 The mux with `? :`

```verilog
assign z = sel ? y : x;   // if sel is 1, z = y; otherwise z = x
```

Chain them to choose among many options. Your `main_project` uses this to pick which component's result reaches the output:

```verilog
assign O = M0 ? add_out  :
           M1 ? xor_out  :
           M2 ? nand_out :
           M3 ? func_out : 8'b00000000;
```

Read it top to bottom: "if M0 is 1, use `add_out`; else if M1 is 1, use `xor_out`; ..."

### 3.8 Comments

```verilog
// single-line comment
```

The project is graded on documentation, so comment **why**, not just what.

### 3.9 A first complete example you can run

This is the handout's example. Put the module and the test bench in the **same** Colab cell.

```verilog
module logic_function (
  input B, input C, input D, input E,
  output A
);
  assign A = (~B & C) | (~D & E);
endmodule

module testbench;
  reg B, C, D, E;
  wire A;

  logic_function dut (.B(B), .C(C), .D(D), .E(E), .A(A));

  initial begin
    B = 0; C = 1; D = 0; E = 1;
    #10;
    $display("Test Case 1 - A = %b", A);

    B = 1; C = 0; D = 1; E = 0;
    #10;
    $display("Test Case 2 - A = %b", A);

    $finish;
  end
endmodule
```

Output:

```
Test Case 1 - A = 1
Test Case 2 - A = 0
```

Check Test Case 1 by hand: B=0, C=1, D=0, E=1 gives `(~0 & 1) | (~0 & 1)` = `1 | 1` = `1`. 

---

## 4. Binary and two's complement (needed for AB2L)

Your FUNCTION uses **subtraction** on 8-bit numbers, so you need to be comfortable with this.

### 4.1 8-bit values wrap around

With 8 bits you can only hold 0 to 255. If a result goes outside that, it **wraps** (the extra bit is dropped). `255 + 1 = 0`. `5 - 10 = 251`.

Verified in the simulator: `8'd5 - 8'd10` gives `11111011`, which is 251.

### 4.2 Subtraction using an adder

Hardware doesn't have a separate subtract circuit. It uses:

```
A - B  =  A + (NOT B) + 1
```

- `NOT B` flips every bit of B (called the one's complement).
- Adding 1 makes it the **two's complement**, which acts as the negative of B.
- The `+ 1` is done by feeding a carry-in of `1` into the adder.

Worked example: `5 - 3` in 8 bits.

```
A       = 00000101   (5)
B       = 00000011   (3)
NOT B   = 11111100
NOT B+1 = 11111101   (this is -3)

  00000101
+ 11111101
----------
 100000010   <- 9 bits; drop the leftmost (carry out) => 00000010 = 2  ✓
```

This is exactly what your `subtractor_8bit` does: it feeds `~b` into the adder and sets `cin` to `1'b1`.

### 4.3 Reading a result as signed or unsigned

The hardware doesn't care. `10000001` is 129 if you treat it as unsigned, or -127 if you treat it as signed two's complement. Same bits, different interpretation. The project just shows the bits.

---

## 5. Walking through the project, component by component

This follows the notebook you uploaded. Each section: what it does, the code, and the idea behind it.

### 5.1 The decoder

**Job:** take the 2 opcode bits and turn on exactly **one** of four output lines.

| I0 | I1 | Output that turns on | Used for |
|---|---|---|---|
| 0 | 0 | M0 | Full adder |
| 0 | 1 | M1 | XOR |
| 1 | 0 | M2 | NAND |
| 1 | 1 | M3 | FUNCTION |

```verilog
module decoder (
  input  I0,
  input  I1,
  output M0,
  output M1,
  output M2,
  output M3
);
  assign M0 = ~I0 & ~I1;   // opcode 00
  assign M1 = ~I0 &  I1;   // opcode 01
  assign M2 =  I0 & ~I1;   // opcode 10
  assign M3 =  I0 &  I1;   // opcode 11
endmodule
```

Each line is an AND that is only true for one specific opcode. That's a 2-to-4 decoder. Your opcode assignment is your own choice, and it's recorded in the table at the top of your notebook.

### 5.2 The full adder

**1-bit full adder.** Adds three 1-bit inputs: `a`, `b`, and a carry-in `cin`. Produces `sum` and a carry-out `cout`.

```verilog
module full_adder_1bit (
  input  a,
  input  b,
  input  cin,
  output sum,
  output cout
);
  assign sum  = a ^ b ^ cin;
  assign cout = (a & b) | (cin & (a ^ b));
endmodule
```

Check it with `a=1, b=1, cin=1`: sum = `1^1^1` = 1, cout = `(1&1) | (1&(1^1))` = `1 | 0` = 1. And 1+1+1 = 3 = binary `11`, so sum 1 and carry 1. ✓

**8-bit full adder (ripple-carry).** Chain eight 1-bit adders. The carry-out of each feeds the carry-in of the next:

```
 bit0 --> [FA0] -c1-> [FA1] -c2-> [FA2] ... -c7-> [FA7] --> cout
 cin ->                                                    
```

```verilog
module full_adder_8bit (
  input  [7:0] a,
  input  [7:0] b,
  input        cin,
  output [7:0] sum,
  output       cout
);
  wire c1, c2, c3, c4, c5, c6, c7;   // the carries between adders

  full_adder_1bit fa0 (.a(a[0]), .b(b[0]), .cin(cin), .sum(sum[0]), .cout(c1));
  full_adder_1bit fa1 (.a(a[1]), .b(b[1]), .cin(c1),  .sum(sum[1]), .cout(c2));
  // ... fa2 to fa6 follow the same pattern ...
  full_adder_1bit fa7 (.a(a[7]), .b(b[7]), .cin(c7),  .sum(sum[7]), .cout(cout));
endmodule
```

Things to be able to explain:

- Why there are **7** internal carry wires (`c1` to `c7`): eight adders have eight carry-outs, but the first takes the external `cin` and the last drives the external `cout`, leaving 7 in the middle.
- Why it is called **ripple-carry**: the carry has to travel through every stage in turn.
- Why `main_project` sets `cin` to `1'b0`: for plain addition there is nothing to carry in.

### 5.3 The 8-bit XOR and NAND

The same pattern: build the 1-bit gate, then put eight side by side. Each bit is independent, so there are no carries between them.

```verilog
module xor_1bit (input a, input b, output q);
  assign q = a ^ b;
endmodule

module xor_8bit (
  input  [7:0] a,
  input  [7:0] b,
  output [7:0] q
);
  xor_1bit x0 (.a(a[0]), .b(b[0]), .q(q[0]));
  xor_1bit x1 (.a(a[1]), .b(b[1]), .q(q[1]));
  // ... x2 to x6 ...
  xor_1bit x7 (.a(a[7]), .b(b[7]), .q(q[7]));
endmodule
```

`nand_1bit` and `nand_8bit` are identical except for the gate: `assign q = ~(a & b);`.

### 5.4 The FUNCTION for AB2L: `O = NOT(A) - NAND(B, B)`

Break the formula into pieces:

1. **`NOT(A)`**: a NAND with both inputs tied together is a NOT, since `NAND(x, x) = ~(x & x) = ~x`. So `nand_8bit` with both inputs set to `a` gives `NOT(A)`.
2. **`NAND(B, B)`**: the same trick gives `NOT(B)`.
3. **Subtract** the second from the first using the adder trick from section 4.2.

```verilog
module subtractor_8bit (
  input  [7:0] a,
  input  [7:0] b,
  output [7:0] diff
);
  wire unused_cout;   // carry out isn't needed

  full_adder_8bit sub_adder (.a(a), .b(~b), .cin(1'b1), .sum(diff), .cout(unused_cout));
endmodule

module function_8bit (
  input  [7:0] a,
  input  [7:0] b,
  output [7:0] o
);
  wire [7:0] not_a;     // NOT(A)  = NAND(A,A)
  wire [7:0] nand_bb;   // NAND(B,B)

  nand_8bit       f_not  (.a(a), .b(a), .q(not_a));
  nand_8bit       f_nand (.a(b), .b(b), .q(nand_bb));
  subtractor_8bit f_sub  (.a(not_a), .b(nand_bb), .diff(o));
endmodule
```

Hand trace with `A = 10000000`, `B = 00000001`:

```
NOT(A)       = 01111111   (127)
NAND(B,B)    = 11111110   (254)
127 - 254    = -127  -> in 8-bit two's complement = 10000001
```

So the circuit outputs `10000001`. Also see [section 8](#8-the-ab2l-spec-example-doesnt-match-the-formula) about the spec's stated answer.

### 5.5 `main_project`: putting it together

```verilog
module main_project (
  input  [17:0] I,
  output [7:0]  O
);
  wire [7:0] A = I[15:8];
  wire [7:0] B = I[7:0];
  ...
```

**The bit-numbering gotcha.** The spec names bits **I0 (leftmost) to I17 (rightmost)**. Verilog vectors written `[17:0]` number the *other* way: the leftmost bit is `I[17]`, the rightmost is `I[0]`. So:

| Spec name | Verilog index | Role |
|---|---|---|
| I0 | `I[17]` | opcode MSB |
| I1 | `I[16]` | opcode LSB |
| I2 to I9 | `I[15:8]` | operand A |
| I10 to I17 | `I[7:0]` | operand B |

Same for the output: the spec's O0 is `O[7]` and O7 is `O[0]`. This is a very likely thing for your instructor to ask about.

The rest of the module:

```verilog
  wire M0, M1, M2, M3;
  wire [7:0] add_out, xor_out, nand_out, func_out;
  wire add_cout;

  decoder dec (.I0(I[17]), .I1(I[16]), .M0(M0), .M1(M1), .M2(M2), .M3(M3));

  full_adder_8bit add_unit  (.a(A), .b(B), .cin(1'b0), .sum(add_out), .cout(add_cout));
  xor_8bit        xor_unit  (.a(A), .b(B), .q(xor_out));
  nand_8bit       nand_unit (.a(A), .b(B), .q(nand_out));
  function_8bit   func_unit (.a(A), .b(B), .o(func_out));

  assign O = M0 ? add_out  :
             M1 ? xor_out  :
             M2 ? nand_out :
             M3 ? func_out : 8'b00000000;
endmodule
```

Key idea: **all four components compute at the same time on every input.** The decoder and the `? :` chain only decide which result reaches `O`. The "activated" component is the one that is selected, not the only one running.

---

## 6. Test benches

A test bench is a module with **no ports**. It is not a circuit; it is a script that drives inputs into your circuit and checks the output. It only runs in simulation.

### 6.1 The pieces

| Piece | What it does |
|---|---|
| `reg [17:0] I;` | variable you control (the input) |
| `wire [7:0] O;` | observes the circuit's output |
| `main_project dut (.I(I), .O(O));` | places your circuit (`dut` = "device under test") |
| `initial begin ... end` | runs once at the start of simulation, top to bottom |
| `I = {op, ia, ib};` | build the 18-bit input by gluing opcode, A, B together |
| `#10;` | wait 10 time units so the signals settle |
| `$display("...", x);` | print a line (`%b` shows binary) |
| `$finish;` | end the simulation |

### 6.2 Your notebook's `check` task

A **task** is a reusable block of test code, like a function:

```verilog
task check(input [1:0] op, input [7:0] ia, input [7:0] ib, input [7:0] exp);
  begin
    I = {op, ia, ib};
    #10;
    if (O === exp) $display("PASS op=%b A=%b B=%b O=%b", op, ia, ib, O);
    else           $display("FAIL op=%b A=%b B=%b O=%b expected=%b", op, ia, ib, O, exp);
  end
endtask
```

Each call applies one input, waits, and compares. Note `===` (strict equality) rather than `==`. It also treats unknown (`x`) values as a mismatch, which is what you want when testing.

Example call: `check(2'b00, 8'b11111111, 8'b00000001, 8'b00000000);` tests that 255 + 1 wraps to 0.

### 6.3 The exhaustive loop

```verilog
for (k = 0; k < 262144; k = k + 1) begin
  I = k;
  #1;
  a = I[15:8];
  b = I[7:0];
  case (I[17:16])
    2'b00: expected = a + b;
    2'b01: expected = a ^ b;
    2'b10: expected = ~(a & b);
    2'b11: expected = (~a) - (~(b & b));
  endcase
  if (O !== expected) errors = errors + 1;
end
```

- 2^18 = **262,144** possible inputs, and the loop tries every one.
- It compares against a **reference model** written with plain Verilog operators (`+`, `^`, `-`). This is a good testing idea: build the circuit the hard way (gate by gate), then check it against the easy way.
- Because the reference model and the structural design are written independently, agreement between them is real evidence the circuit is right.

I ran the full design from your notebook in a simulator. Output ends with `ALL 262144 INPUTS PASS`.

---

## 7. Running Verilog in Google Colab

Colab normally runs Python, so the handout installs a plugin first.

**Setup cell (run once at the start):**

```
!pip install git+https://github.com/lesc-ufv/cad4u.git &> /dev/null
!git clone https://github.com/lesc-ufv/cad4u &> /dev/null
%load_ext plugin
```

A green check means it worked.

**Then, per cell:**

| Cell starts with | What you get |
|---|---|
| `%%verilog` | compiles and runs the code (prints `$display` output) |
| `%%print_verilog` | draws the circuit diagram of the module |

Important behavior:

- **Each cell is compiled on its own.** A cell can only use modules defined **in that same cell**. That's why your FUNCTION cell repeats the adder and NAND modules, and why the final cell contains every module plus the test bench.
- To **test**, the module and test bench must be in the **same** cell, and the cell must start with `%%verilog`. (`%%print_verilog` is for viewing circuits.)
- Wait for the green check. The first run can be slow.

---

## 8. The AB2L spec example doesn't match the formula

The spec table says for AB2L:

> `O = NOT(A) - NAND(B,B)`, and `Function(10000000, 00000001) = 00000001`

But by hand (section 5.4), the formula gives `10000001`, not `00000001`.

Your notebook already flags this in its note, and it is the right approach: **implement the formula as written, and ask your lab instructor which one they expect.** Don't silently change the circuit to force the example to match. When you ask, show your hand calculation. Checking the other rows by hand, most examples (AB3L, AB4L, A1L, A2L, A3L, A4L, A5L) match their formulas, while AB1L's example also looks off. So the handout may simply have a typo or two, but only your instructor can confirm.

For the Q&A, be able to say: "We implemented the formula as written, here's our hand calculation, and we asked about the discrepancy."

---

## 9. Common beginner errors

| Mistake | Symptom | Fix |
|---|---|---|
| Forgot `%%verilog` at top of cell | Python syntax errors | Add it as the first line |
| Used a module defined in another cell | "Unknown module type" | Copy the module into this cell |
| Missing `;` after `assign` or port list `);` | Syntax error | Check every statement ends with `;` |
| No `endmodule` | Syntax error at the next `module` | Close every module |
| Two instances with the same name | Duplicate-name error | Give each instance a unique name |
| Wrong bit order (`I[0]` vs spec's I0) | Output is garbage but compiles | Remember `I0` = `I[17]` |
| Forgot to wait (`#10`) in test bench | Outputs show `x` or old values | Add a delay after changing inputs |
| `reg` in the circuit, `wire` for test inputs | Compile errors | Test bench drives with `reg`; circuit uses `wire` |
| Widths don't match | Silent truncation or warnings | Count bits on both sides |
| Missing `$finish;` | Simulation never ends | Add it at the end of `initial` |

If the test prints `x`, that means **unknown**: usually an input never got a value, or a wire is unconnected.

---

## 10. Preparing for the Q&A

Criteria A and C both say you may be quizzed on how the code works and why tests passed or failed. **Everyone in the group should be able to answer these without reading the notebook:**

**Design**
1. What does the decoder do, and what happens for each opcode?
2. Why are there 7 carry wires in the 8-bit adder?
3. Why is `cin` set to 0 in `main_project`? Why is it 1 in the subtractor?
4. How does NAND act as a NOT gate?
5. Why does subtraction use `~b` and a carry-in of 1?
6. Does only the selected component run? (No. All run; the mux selects the output.)
7. Spec says I0 is the MSB, but the code uses `I[17]`. Why?

**Testing**
8. Why `===` and not `==`?
9. Why `#10`?
10. What does the exhaustive loop prove that two directed tests per opcode don't?
11. Why does the FUNCTION test expect `10000001` while the spec table shows `00000001`?

**Practice for the hand-trace question.** An instructor might give you an input and ask for the output. Pick random 18-bit values and trace them by hand: split into opcode / A / B, run the operation, write the 8-bit result. If you can do it for all four opcodes, you understand the project.

**Academic honesty.** The spec treats academic dishonesty seriously and grades attribution. Your notebook says the decoder structure follows a friend's notebook. Make sure every reference is complete and accurate, that the inline comments point to where each reference was used, and that you describe honestly how any other help (including AI) was used. Check your instructor's rules on this. More importantly, you must be able to explain every module yourself, since that is what criterion A tests.

---

## 11. Practice exercises

Try these in Colab before the deadline. Answers are at the bottom.

**Exercise 1.** Write a 1-bit module `or_1bit` with inputs `a`, `b` and output `q`.

**Exercise 2.** Write a 3-input AND gate module using one `assign`.

**Exercise 3.** Given `v = 8'b11001010`, what are `v[7:4]`, `v[0]`, and `{v[3:0], v[7:4]}`?

**Exercise 4.** Compute `00000011 - 00000101` in 8-bit two's complement, using the `A + NOT(B) + 1` method.

**Exercise 5.** For `main_project` with your opcode table, trace the output for `I = 11 00000001 10000000` (opcode `11`, A = `00000001`, B = `10000000`).

**Exercise 6.** Write a 2-to-1 mux for 8-bit inputs with a select line, using `? :`.

### Answers

1. 
   ```verilog
   module or_1bit (input a, input b, output q);
     assign q = a | b;
   endmodule
   ```
2. 
   ```verilog
   module and3 (input a, input b, input c, output q);
     assign q = a & b & c;
   endmodule
   ```
3. `v[7:4] = 1100`, `v[0] = 0`, `{v[3:0], v[7:4]} = 10101100`.
4. `A = 00000011`, `NOT(B) = 11111010`, plus 1 = `11111011`. Sum: `00000011 + 11111011 = 11111110` (carry dropped). That is -2 in two's complement (254 unsigned). ✓
5. Opcode `11` selects FUNCTION. `NOT(A) = 11111110`, `NAND(B,B) = 01111111`. `11111110 - 01111111 = 254 - 127 = 127 = 01111111`. (This matches the second directed test in your notebook.)
6. 
   ```verilog
   module mux2_8bit (input [7:0] x, input [7:0] y, input sel, output [7:0] z);
     assign z = sel ? y : x;
   endmodule
   ```

---

## Quick reference card

```verilog
module name (input a, input [7:0] b, output [7:0] q);   // ports
  wire [7:0] w;                                          // internal wire
  assign q = ~(a & b[0]);                                // logic
  other_module inst (.pin(signal), .pin2(signal2));      // instantiate
endmodule

// literals:   1'b1   8'b10000000   8'd10
// slice:      v[7:4]     bit: v[0]     concat: {x, y}
// mux:        sel ? y : x
// test bench: reg to drive, wire to observe, #10 to wait,
//             $display("%b", sig); $finish;
```