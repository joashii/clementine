# Test Bench (`testbench`) Study Guide

## 1. Complete `testbench` Code

```verilog
module testbench;
  reg  [17:0] I;
  wire [7:0]  O;
  reg  [7:0]  a, b, expected;
  integer     k, errors;

  // create the main_project module
  main_project dut (.I(I), .O(O));

  // apply an input and compare the result
  task check(input [1:0] op, input [7:0] ia, input [7:0] ib, input [7:0] exp);
    begin
      I = {op, ia, ib};
      #10; // wait for the signals to update
      if (O === exp) $display("PASS op=%b A=%b B=%b O=%b", op, ia, ib, O);
      else           $display("FAIL op=%b A=%b B=%b O=%b expected=%b", op, ia, ib, O, exp);
    end
  endtask

  initial begin
    // opcode 00: full adder
    check(2'b00, 8'b10000000, 8'b00000001, 8'b10000001);
    check(2'b00, 8'b11111111, 8'b00000001, 8'b00000000);  // wraps around

    // opcode 01: xor
    check(2'b01, 8'b10000000, 8'b00000001, 8'b10000001);
    check(2'b01, 8'b10101010, 8'b01010101, 8'b11111111);

    // opcode 10: nand
    check(2'b10, 8'b10000000, 8'b00000001, 8'b11111111);
    check(2'b10, 8'b11111111, 8'b11111111, 8'b00000000);

    // opcode 11: function
    // the formula gives 10000001, even though the table says 00000001
    check(2'b11, 8'b10000000, 8'b00000001, 8'b10000001);

    // 11111110 - 01111111 = 01111111
    check(2'b11, 8'b00000001, 8'b10000000, 8'b01111111);

    // test all possible 18-bit inputs
    errors = 0;
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

    if (errors == 0) $display("ALL 262144 INPUTS PASS");
    else             $display("%0d MISMATCHES", errors);

    $finish;
  end
endmodule
```

---

## 2. Basic Concepts Used

* **`reg` vs. `wire` in a Test Bench:**
  * Inputs to the circuit under test (`I`) are declared as **`reg`** because the test bench actively stores and changes their values over time inside an `initial` block.
  * Outputs from the circuit under test (`O`) are declared as **`wire`** because the `main_project` module continuously drives them.
* **Concatenation `{op, ia, ib}`:**
  * Curly braces `{}` join smaller bit vectors together into one wider bus from MSB to LSB.
  * Joining a 2-bit `op`, an 8-bit `ia`, and an 8-bit `ib` creates the exact 18-bit input `I[17:0]`.
* **Simulation Delay (`#10` and `#1`):**
  * Tells the simulator to pause for 10 or 1 time units so combinational signals have time to propagate through the gates before reading `O`.
* **Case Equality (`===` and `!==`):**
  * Unlike `==`, the triple-equals `===` checks an exact bit-by-bit match, including unknown (`x`) or high-impedance (`z`) states. If `O` is `8'bxxxxxxxx`, `O === exp` properly evaluates to false.
* **Exhaustive Loop ($2^{18} = 262,144$):**
  * Because `I` is 18 bits wide, there are $2^{18} = 262,144$ possible binary combinations (`0` to `262,143`).

---

## 3. Strict Line-by-Line Explanation

### Signal Declarations & DUT Instantiation
* `module testbench;` — Declares a self-contained simulation module with no external input or output ports.
* `reg [17:0] I;` — 18-bit register used to drive test inputs into `main_project`.
* `wire [7:0] O;` — 8-bit wire that captures the output coming out of `main_project`.
* `reg [7:0] a, b, expected;` — Helper registers used during the exhaustive loop to hold sliced operands and the reference answer.
* `integer k, errors;` — Loop counter (`k`) and mismatch counter (`errors`).
* `main_project dut (.I(I), .O(O));` — Instantiates `main_project` as the Device Under Test (`dut`), connecting testbench signals `I` and `O` to its ports.

### The Reusable `check` Task
* `task check(input [1:0] op, input [7:0] ia, input [7:0] ib, input [7:0] exp);` — Defines a helper routine that accepts a 2-bit opcode, two 8-bit operands, and the expected 8-bit output.
* `begin ... end` — Groups the statements inside the task.
* `I = {op, ia, ib};` — Packs the opcode and operands into the 18-bit bus `I`.
* `#10;` — Waits 10 simulation time units for `main_project` to compute `O`.
* `if (O === exp) $display("PASS op=%b A=%b B=%b O=%b", op, ia, ib, O);` — Prints `PASS` along with the binary values if `O` matches `exp`.
* `else $display("FAIL op=%b A=%b B=%b O=%b expected=%b", op, ia, ib, O, exp);` — Prints `FAIL` and shows both the actual and expected outputs if they differ.

### Directed Test Cases (`initial begin`)
* `initial begin` — Starts the sequential test script once at simulation time `0`.
* `check(2'b00, 8'b10000000, 8'b00000001, 8'b10000001);` — Tests opcode `00` (Adder): `128 + 1 = 129` (`10000001`).
* `check(2'b00, 8'b11111111, 8'b00000001, 8'b00000000);` — Tests adder overflow wrap-around: `255 + 1 = 256`, which wraps to `00000000` in 8 bits.
* `check(2'b01, 8'b10000000, 8'b00000001, 8'b10000001);` — Tests opcode `01` (XOR) on single-bit operands.
* `check(2'b01, 8'b10101010, 8'b01010101, 8'b11111111);` — Tests opcode `01` (XOR) on alternating bit patterns (`10101010 XOR 01010101 = 11111111`).
* `check(2'b10, 8'b10000000, 8'b00000001, 8'b11111111);` — Tests opcode `10` (NAND) where no bit positions are both `1`.
* `check(2'b10, 8'b11111111, 8'b11111111, 8'b00000000);` — Tests opcode `10` (NAND) where all bit positions are `1`, inverting to all `0`s.
* `check(2'b11, 8'b10000000, 8'b00000001, 8'b10000001);` — Tests opcode `11` (AB2L Function) using the specification's input values (`NOT(10000000) - NAND(00000001, 00000001) = 10000001`).
* `check(2'b11, 8'b00000001, 8'b10000000, 8'b01111111);` — Tests opcode `11` (AB2L Function) with reversed operands (`11111110 - 01111111 = 01111111`).

### Exhaustive Verification Loop
* `errors = 0;` — Resets the error counter before starting the full sweep.
* `for (k = 0; k < 262144; k = k + 1) begin` — Iterates through every integer from `0` to `262,143`.
* `I = k;` — Assigns the 18-bit binary representation of `k` directly to input `I`.
* `#1;` — Waits 1 time unit for `dut` to settle.
* `a = I[15:8];` and `b = I[7:0];` — Extracts operands `A` and `B` for the behavioral reference calculation.
* `case (I[17:16]) ... endcase` — Computes the expected 8-bit answer using built-in Verilog operators (`+`, `^`, `~`, `&`, `-`) based on the opcode bits `I[17:16]`.
* `if (O !== expected) errors = errors + 1;` — Increments `errors` if the gate-level `main_project` disagrees with the reference model.
* `if (errors == 0) $display("ALL 262144 INPUTS PASS");` — Confirms zero mismatches across the entire input space.
* `else $display("%0d MISMATCHES", errors);` — Reports how many combinations failed if any error occurred.
* `$finish;` — Ends the simulation cleanly.

---

## 4. How the Verification Architecture Works

1. **Directed Sanity Checks (`check` task):**
   * Runs 8 hand-picked test cases (2 per opcode) covering normal cases and edge cases (such as 8-bit overflow wrap-around and all-ones inputs) while printing human-readable `PASS`/`FAIL` lines to the console.
2. **Exhaustive Equivalence Checking (`for` loop):**
   * Treats `main_project` (built from structural 1-bit gates) as a black box and compares it against a behavioral "golden reference model" written with high-level Verilog operators inside the `case` statement.
   * Testing all $2^{18} = 262,144$ combinations guarantees 100% input coverage with no untested corner cases.

---

## 5. Step-by-Step Trace Example

Let's trace the seventh `check` call in `testbench`:
`check(2'b11, 8'b10000000, 8'b00000001, 8'b10000001);`

1. **Task Call & Concatenation:**
   * `op = 2'b11`, `ia = 8'b10000000`, `ib = 8'b00000001`, `exp = 8'b10000001`.
   * `I = {op, ia, ib}` joins them into the 18-bit vector:
     `I = 18'b11_10000000_00000001`
2. **Propagation Delay (`#10`):**
   * `I` enters `main_project dut`.
   * `decoder` sees `I[17:16] = 11` and sets `M3 = 1` (`M0 = M1 = M2 = 0`).
   * `function_8bit` computes `NOT(10000000) - NAND(00000001, 00000001) = 01111111 - 11111110 = 10000001`.
   * The multiplexer selects `func_out`, driving `O = 8'b10000001`.
3. **Comparison & Output:**
   * After `#10`, `testbench` evaluates `O === exp` (`8'b10000001 === 8'b10000001`), which is **true**.
   * The console prints:
     `PASS op=11 A=10000000 B=00000001 O=10000001`
4. **Exhaustive Loop Confirmation:**
   * Later, when the `for` loop reaches `k = 229377` (`18'b11_10000000_00000001`), the `case` block computes `(~8'b10000000) - (~(8'b00000001 & 8'b00000001)) = 8'b10000001`, confirms `O === expected`, and leaves `errors = 0`.