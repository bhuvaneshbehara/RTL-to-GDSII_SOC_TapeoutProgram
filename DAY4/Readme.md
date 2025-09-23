
# <img width="50" height="50" alt="Day4" src="https://github.com/user-attachments/assets/ffd03c1f-14fc-48cb-8d8d-83622f8285dc" /> Day4:  Gate-Level Simulation (GLS), Blocking vs. Non-Blocking in Verilog, and Synthesis-Simulation Mismatch

🚀 Welcome to Day 4 of the RTL-to-GDSII SoC Tapeout Program! 🎉

You’ve come a long way—building a strong foundation in RTL design, synthesis, and optimization. Today, we step into the critical verification phase that ensures our designs truly behave as intended.

```

🔹 First, we’ll dive into Gate-Level Simulation (GLS) to see how real gate-level netlists behave compared to RTL.

🔹 Next, we’ll unravel the difference between Blocking and Non-Blocking Assignments in Verilog—one of the most important coding principles for avoiding tricky bugs.

🔹 Finally, we’ll explore Synthesis-Simulation Mismatch, a common pitfall every digital designer must learn to prevent.

```

✨ By mastering these concepts, you’ll gain the skills to bridge the gap between RTL and hardware reality, preparing you to design more reliable, silicon-ready circuits.

# Contents of the Day

- [1. Gate-Level Simulation (GLS)](#1-gate-level-simulation-gls)
  
  - [1.1 Types of Gate-level Simulation](#11-types-of-gate-level-simulation)
    
- [2. Blocking vs. Non-Blocking Assignments in Verilog](#2-blocking-vs-non-blocking-assignments-in-verilog)
  
  - [2.1 Blocking Assignment](#21-blocking-assignment)
    
  - [2.2 Non-Blocking Assignment](#22-non-blocking-assignment)
    
  - [2.3 Comparison Table](#23-comparison-table)
    
- [3. Synthesis-Simulation Mismatch](#3-synthesis-simulation-mismatch)

- [4. Labs](#4-labs)

- [5. Summary](#5-summary)



# 1. Gate-Level Simulation (GLS)


### 📌 Definition:

**Gate-Level Simulation** is the process of simulating a digital circuit after synthesis, using the gate-level netlist generated from RTL. It includes timing information and represents how the circuit will behave in real hardware. GLS is more accurate than RTL simulation because it accounts for gate delays, interconnect delays, and glitches.

### ✨ Purpose of GLS:

- Verify that the synthesized netlist behaves exactly like the RTL design.

- Identify timing-related issues that cannot be caught in RTL simulation.

- Ensure correctness before FPGA implementation or ASIC tape-out.


## 1.1 Types of Gate-Level Simulation

- Functional GLS (Timing-Independent GLS)
  
- Timing-Accurate GLS (Timing-Dependent GLS)

## 🔹 Functional GLS (Timing-Independent GLS)

  - Simulates the gate-level netlist without considering delays.

  - Focuses on logical correctness of the synthesized design.

  - Useful for initial verification after synthesis.

## 🔹 Timing-Accurate GLS (Timing-Dependent GLS)

  - Includes gate and interconnect delays extracted from synthesis or static timing analysis (STA).

  - Detects race conditions, glitches, and setup/hold violations.

  - Essential for final verification before fabrication.

 ### 📌 Why Gate-Level Simulation?

- RTL alone may miss timing issues: RTL simulation assumes zero-delay behavior.

- Catches post-synthesis bugs: Certain constructs in RTL may synthesize differently.

- Verifies timing closure: Ensures setup, hold, and propagation delays are met.

- Reduces costly mistakes: Detecting errors before tape-out saves time and money.

### ✨ Benefits of GLS

- **Accurate Verification:** Reflects real hardware behavior, including glitches and hazards.

- **Timing Awareness:** Detects violations and performance issues early.

- **Design Reliability:** Ensures the design will function correctly when fabricated.

- **Debugging Synthesis Issues:** Helps find mismatches between RTL and netlist.

- **Confidence Before Fabrication:** Reduces risk of expensive design respins.   


# 2. Blocking vs. Non-Blocking Assignments in Verilog

Verilog has two types of assignments used in procedural blocks:

- Blocking Assignment `(=)`

- Non-Blocking Assignment `(<=)`

### 2.1 Blocking Assignment

Syntax:

```
variable = expression;

```

### Characteristics:

- Executes sequentially in the order written.

- The next statement waits until the current assignment is completed.

- Typically used for combinational logic inside `always @(*)` blocks.

### Example:

```verilog
always @(*) begin
    a = b;   // Executes first
    c = a;   // Executes after a = b
end

```
### ✨ Explanation:

- Here, `c` gets the new value of `a` immediately after `a = b`.


### 2.2 Non-Blocking Assignment

Syntax:

```
variable <= expression;

```

### ✨ Characteristics:

- Executes in parallel, not sequentially.

- The right-hand side is evaluated immediately, but the left-hand side is updated at the end of the time step.

- Typically used for sequential logic inside always @(posedge clk) blocks.

### Example:

```verilog
always @(posedge clk) begin
    a <= b;   // Scheduled to update at the end of this time step
    c <= a;   // c gets the OLD value of a, not the new b
end

```

### ✨ Explanation: 

- Both updates happen simultaneously at the end of the clock edge, preserving correct sequential behavior.

### 2.3 Comparison Table

|   Feature	        |   Blocking (=)	                                       |    Non-Blocking (<=)                         |
|-------------------|--------------------------------------------------------|----------------------------------------------|
| Execution	        | Sequential	                                           |    Parallel (scheduled at end of timestep)   |
| Usage	            | Combinational logic	                                   |    Sequential logic (flip-flops)             |
| RHS evaluation    |	Immediately	                                           |     Immediately, but LHS updated later       |
| Race conditions   |	Can cause race condition in sequential logic           |     Helps avoid race conditions in flip-flops|
                      	


### 📌 Why it Matters

- Blocking is suitable for combinational circuits, e.g., if-else, case, or logic expressions.

- Non-blocking is essential for clocked sequential circuits, e.g., D flip-flops or registers, to prevent unexpected behavior.

- Using the wrong type can cause synthesis-simulation mismatch, race conditions, or incorrect RTL behavior.








# 3. Synthesis-Simulation Mismatch


### 📌 Definition:

**Synthesis-Simulation Mismatch** occurs when the behavior of a synthesized netlist (gate-level design) does not match the RTL simulation results. Even if your RTL code passes functional simulation, the synthesized hardware might behave differently due to coding styles, unsupported constructs, or synthesis optimizations.

### ✨ Purpose of Studying SSM:

- Identify why the RTL design and synthesized netlist behave differently.

- Ensure your final hardware functions correctly before fabrication or FPGA implementation.

- Improve coding practices to avoid synthesis pitfalls.

### Causes of Synthesis-Simulation Mismatch

- Incorrect or Non-Synthesizable RTL Constructs

  - Some Verilog features (like delays #5, initial blocks) are ignored by synthesis tools.
 
- Inferred Latches or Flip-Flops

  - Improper coding (e.g., incomplete assignments in combinational always blocks) can lead to unintended storage elements.

- Blocking vs. Non-Blocking Assignment Issues

  - Misuse of = and <= can cause race conditions in synthesized designs.

- Tool Optimizations

  - Synthesis tools may reorder logic or remove redundant gates, slightly altering behavior.

- X-Propagation Differences

  - Simulators handle unknown states (X) differently than synthesized hardware, leading to mismatches.


### 📌 Why SSM Matters

- Detecting mismatches prevents functional bugs in real hardware.

- Helps designers understand limitations of RTL constructs and synthesis rules.

- Critical for high-reliability designs, like processors or communication systems.

### ✨ Benefits of Understanding and Resolving SSM

- **Functional Accuracy:** Ensures your RTL and synthesized design behave identically.

- **Better RTL Coding Practices:** Avoids constructs that lead to mismatches.

- **Early Bug Detection:** Prevents costly mistakes before FPGA/ASIC implementation.

- **Predictable Timing:** Ensures correct sequential logic behavior in the synthesized netlist.

- **Improved Design Confidence:** Guarantees robust and reliable hardware.


# 4. Labs


## Lab 1: Ternary Operator MUX

Verilog code for a simple 2:1 multiplexer using a ternary operator:

```verilog

module ternary_operator_mux (input i0, input i1, input sel, output y);
  assign y = sel ? i1 : i0;
endmodule

```

- **Function:** `y = i1` if `sel = 1`; else `y = i0`.

 <div align="center"> <img width="700" height="700" alt="glstb" src="https://github.com/user-attachments/assets/7711e3b1-228a-43a9-bc2e-5dc513198988" /> </div>


## Lab 2: Synthesis Using Yosys

Synthesize the above MUX using Yosys.  
_Follow the standard Yosys synthesis flow._

<div align="center"> <img width="700" height="700" alt="ternary" src="https://github.com/user-attachments/assets/cb67baf8-d241-4c6a-9a04-708931b3cd7c" /> </div>  


## Lab 3: Gate-Level Simulation (GLS) of MUX

Run GLS for the synthesized MUX.  
Use this command (adjust paths as needed):

```

iverilog /path/to/primitives.v /path/to/sky130_fd_sc_hd.v ternary_operator_mux.v testbench.v

```
<div align="center"> <img width="700" height="700" alt="glsternarytb" src="https://github.com/user-attachments/assets/ea101267-5503-4968-aab9-c11ae76806c8" /> </div>


## Lab 4: Bad MUX Example 

Verilog code with intentional issues:

```verilog

module bad_mux (input i0, input i1, input sel, output reg y);
  always @ (sel) begin
    if (sel)
      y <= i1;
    else 
      y <= i0;
  end
endmodule
```

### Issues:

- **Incomplete sensitivity list**: Should include `i0`, `i1`, and `sel`.

- **Non-blocking assignment in combinational logic**: Should use blocking assignments (`=`).


**Corrected version:**

```verilog

always @ (*) begin
  if (sel)
    y = i1;
  else
    y = i0;
end

```


## Lab 5: GLS of Bad MUX

Perform GLS on the `bad_mux`.  
Expect simulation mismatches or warnings due to above issues.

<div align="center"><img width="700" height="700" alt="glsbadmuxtb" src="https://github.com/user-attachments/assets/bcf37b29-76c6-4a0c-8467-291042d26799" /></div>



## Lab 6: Blocking Assignment Caveat

Verilog code:

```verilog

module blocking_caveat (input a, input b, input c, output reg d);
  reg x;
  always @ (*) begin
    d = x & c;
    x = a | b;
  end
endmodule

```

### What’s wrong?

- The order of assignments causes `d` to use the old value of `x`—not the newly computed value.

- **Best Practice:** Assign intermediate variables before using them.

**Corrected order:**

```verilog

always @ (*) begin
  x = a | b;
  d = x & c;
end

```

<div align="center"><img width="700" height="700" alt="caveattb" src="https://github.com/user-attachments/assets/d0d66a35-729f-4623-92ef-478508126675" /> </div>

## Lab 7: Synthesis of the Blocking Caveat Module

Synthesize the corrected version of the module and observe the results.

<div align="center"><img width="700" height="700" alt="synthesiscaveat" src="https://github.com/user-attachments/assets/154980f0-92f4-411f-9185-38f55cf52202" /> </div>


# 5. Summary

Completing Day 4 of your journey toward mastering the RTL-to-GDSII flow! 🎉

Today, we explored:

- How Gate-Level Simulation helps verify post-synthesis timing and functional behavior.

- The subtle but critical differences between blocking (=) and non-blocking (<=) assignments in Verilog and their impact on sequential logic.

- Common causes of Synthesis-Simulation Mismatches and strategies to detect and fix them early in your design flow.



  
