
# <img width="50" height="50" alt="image" src="https://github.com/user-attachments/assets/1db443d2-e83e-45ec-8414-d1afb5a0bd80" /> Day 3: 🔧 Optimization in Synthesis – Types & Techniques


Welcome to Day 3 of the RTL-to-GDSII_SOC_TapeoutProgram! 🎉

So far, you’ve learned how to simulate designs, explore synthesis, and understand libraries. Today, we step into the heart of digital design optimization.

```
🔹 You’ll explore different types of optimization and its techniques.

🔹 You’ll explore how to optimize combinational circuits to reduce delay, area, and power.

🔹 You’ll dive into sequential circuit optimization, ensuring efficient flip-flop and latch usage for better performance.

🔹 You’ll discover practical techniques that make your RTL more synthesis- and implementation-friendly.

```

✨ By mastering these concepts, you move from simply writing RTL to crafting high-quality, optimized designs ready for real silicon.


# Contents of the day

🔹 [What is optimization?](#what-is-optimization)
  
🔹 [Types of Optimization](#types-of-optimization)

  - [1. Combinational logic Optimization](#1-combinational-logic-optimization)

  - [2. Sequential logic Optimization](#2-sequential-logic-optimization)

🔹 [🔧 Different techniques used in combinational logic optimization](#-different-techniques-used-in-combinational-logic-optimization)

   - [i. Squeezing the Logic](#i-squeezing-the-logic)
   
   - [ii. Constant Propagation](#ii-constant-propagation)
   
   - [iii. Boolean Logic Optimisation](#iii-boolean-logic-optimisation)


🔹 [🔧 Different techniques used in sequential logic optimization](#-different-techniques-used-in-sequential-logic-optimization)

  - [Basic](#basic)

      - [i. Sequential Constant Propagation](#i-sequential-constant-propagation)
  
  - [Advanced](#advanced)
    
      - [ii. State Optimisation](#ii-state-optimization)
      
      -  [iii. Retiming](#iii-retiming)
      
      -  [iv. Sequential Logic Cloning (Floorplan Aware Synthesis)](#iv-sequential-logic-cloning-floorplan-aware-synthesis) 


🔹 [Labs on Optimization](#labs-on-optimization)

  - [Lab 1](#lab-1)
  - [Lab 2](#lab-2)
  - [Lab 3](#lab-3)
  - [Lab 4](#lab-4)
  - [Lab 5](#lab-5)
  - [Lab Work](#lab-work)

🔹 [Summary](#summary)


# what is optimization? 

  **Optimization** is the process of using algorithms and techniques to enhance the performance of an integrated circuit while fusing to a set of design constraints. 

## Primary Goals of optimization

- **Performance (Speed):** The objective is to maximize the operating frequency by minimizing the signal propagation delay. This involves ensuring that signals travel from one component to another fast enough to meet the chip's timing requirements.

- **Power consumption:** The aim is to reduce the power drawn by the circuit, which is crucial for mobile devices to extend battery life and for high-performance chips to manage heat dissipation.

- **Area:** This refers to the physical size of the circuit on the silicon chip. Smaller circuits are less expensive to manufacture and allow for more functionality to be packed into a single chip.

- **Reliability:** The chip must be robust and error-free. This includes ensuring signal integrity (protecting against noise and distortion) and addressing thermal issues from heat buildup. 



# Types of Optimization

## 1. Combinational logic Optimization

### 📌 Definition

**Combinational Logic Optimization** is the process of simplifying or restructuring purely combinational logic circuits (no memory elements) to make the design smaller, faster, or consume less power while keeping the functionality the same.

### 📌 Why it is needed

- Reduce gate count → area savings

- Reduce switching activity → power savings

- Reduce logic depth → timing improvement


## 2. Sequential logic Optimization

### 📌 Definition

**Sequential Logic Optimization** is the process of simplifying or restructuring purely sequential logic circuits (memory elements) to make the design smaller, faster, or consume less power while keeping the functionality the same.

### 📌 Why it is needed

- Improve speed: Reduces critical path delay by better placing flip-flops.

- Save area: Minimizes unnecessary flip-flops and redundant logic.

- Lower power: Less switching in sequential elements.

- Simplify design: Optimizes FSMs and sequential paths for easier implementation.

# 🔧 Different techniques used in combinational logic optimization

## i. Squeezing the Logic

- This means minimizing the number of gates and logic depth.

- The tool restructures logic expressions so that fewer resources (area) are used.

## ✨ How It Works

- Identify duplicate logic: Detect if the same logic expression appears multiple times.

  - Example: X = A & B, Y = A & B & C → can reuse A & B.

- Simplify boolean expressions: Use boolean algebra or Karnaugh map–style simplifications.

  - Example: X = A & 1 → simplify to X = A.

- Eliminate redundant gates: Gates whose outputs are constants or unused.

- Combine small expressions: Reduce the number of intermediate wires.


## <img width="50" height="40" alt="image" src="https://github.com/user-attachments/assets/72fd4468-7fea-4563-9e77-a6c72cdb367e" /> Benefits:

- Area savings → fewer gates → smaller chip size

- Power savings → less switching activity and fewer transistors

## ii. Constant Propagation

**Constant propagation** is the process of identifying signals (wires or inputs) in a combinational circuit that always have a constant value (0 or 1) and then replacing them with that constant throughout the logic. This helps to simplify the logic, because gates fed by constants often produce predictable outputs.

 
 ## ✨ How It Works

- The tool scans the logic network.

- Detects signals that are always 0 or 1 due to circuit design or previous optimizations.

- Replaces those signals with the constant in all places they are used.

- Propagates the simplification further (sometimes enabling more constant simplifications downstream).

## 📌 Example

- Suppose you have a small combinational logic:
```
Y = A & B

```

- If the tool detects that A = 1 (constant), then:

```
Y = 1 & B = B

```
 The AND gate becomes unnecessary; the logic is simplified to just Y = B.

- Similarly, if A = 0, then:

```
Y = 0 & B = 0

```
 The output is constant 0, and the gate can be removed entirely.

## <img width="50" height="40" alt="image" src="https://github.com/user-attachments/assets/72fd4468-7fea-4563-9e77-a6c72cdb367e" /> Benefits

- Reduces gate count → smaller area.

- Reduces delay → fewer gates in the critical path.

- Simplifies further optimizations → constant propagation often triggers other optimizations like dead code removal.

## iii. Boolean Logic Optimisation

**Boolean optimization** is the process of simplifying combinational logic expressions using Boolean algebra rules. The goal is to minimize the number of gates and logic levels without changing the circuit’s functionality.  
 
## ✨ How It Works

- Identify logic expressions in the design.

- Apply Boolean algebra rules to simplify:

  - Idempotent law: A & A = A, A | A = A

  - Null law: A & 0 = 0, A | 1 = 1

  - Complement law: A & ~A = 0, A | ~A = 1

  - Distributive, associative, and De Morgan’s laws

- Reduce gate count and logic levels by replacing complex expressions with simpler equivalents.

- Check for redundancy: Remove gates or wires that are no longer needed.

## 📌 Example

- Original logic:
```
Y = (A & B) | (A & ~B)
```

- Apply Boolean rules:
```
Y = A & (B | ~B)   # factor A
Y = A & 1           # B | ~B = 1
Y = A               # simplified
```

- Result: Original two AND gates and one OR gate → simplified to a single wire.

## <img width="50" height="40" alt="image" src="https://github.com/user-attachments/assets/72fd4468-7fea-4563-9e77-a6c72cdb367e" /> Benefits

- Smaller area → fewer gates and wires.

- Faster timing → shorter critical paths.

- Lower power → fewer transitions.

- Enables further optimization → simpler logic makes sequential and retiming optimizations more effective.

# 🔧 Different techniques used in sequential logic optimization

## Basic

## i. Sequential Constant Propagation

**Sequential constant propagation** is the process of identifying signals in sequential circuits (with flip-flops or registers) that always take a constant value over time and propagating that constant through the logic.

## ✨ How it works

- Analyze sequential elements like D flip-flops and registers.

- Determine constants: Find flip-flops or signals whose outputs are always 0 or 1 due to reset conditions, initial values, or logic dependencies.

- Propagate constants: Replace these signals in downstream combinational logic with their constant values.

- Simplify affected logic: The propagation may trigger combinational optimizations like removing gates or simplifying expressions.

## Example

- Suppose we have a sequential circuit:

```
reg A;  // flip-flop
reg B;

always @(posedge clk) begin
    A <= 1'b0;   // A is always 0
    B <= A & C;
end

```

- Step 1: Detect that A is always 0.

- Step 2: Propagate the constant:

 ```
 B <= 0 & C
 B <= 0   // simplified
 ```

- Step 3: The AND gate driving B can be removed.

## <img width="50" height="40" alt="image" src="https://github.com/user-attachments/assets/72fd4468-7fea-4563-9e77-a6c72cdb367e" /> Benefits

- Reduces flip-flops and combinational logic → smaller area.

- Shortens critical paths → faster timing.

- Simplifies FSMs and sequential paths → easier further optimizations.

- Triggers other sequential optimizations, like retiming or FSM minimization.

## Advanced

## ii. State Optimization

**State optimization** reduces the number of states in a finite state machine (FSM) without changing its functionality.

## ✨ How it works:

- Detects equivalent or unused states.

- Merges or removes them to simplify the FSM.

- Minimizes the number of flip-flops needed to encode states.

## 📌 Example:
```
Original FSM: 8 states, some perform the same output.

After state optimization: 5 states → fewer flip-flops and simpler logic.

```

## <img width="50" height="40" alt="image" src="https://github.com/user-attachments/assets/72fd4468-7fea-4563-9e77-a6c72cdb367e" /> Benefits:

- Reduces area (fewer flip-flops).

- Simplifies combinational logic for next-state logic.

- Improves timing due to smaller critical paths.


## iii. Retiming

**Retiming** moves flip-flops across combinational logic to optimize timing, area, or power.

## ✨ How it works:

- Shifts registers forward or backward in the circuit.

- Does not change functional behavior.

- Targets critical path reduction.

## 📌 Example:

```
Before Retiming:  A --[AND]--[FF]-- B

After Retiming:   [FF]-- A --[AND]-- B

```
- Moves the flip-flop to balance delays.

## <img width="50" height="40" alt="image" src="https://github.com/user-attachments/assets/72fd4468-7fea-4563-9e77-a6c72cdb367e" /> Benefits:

- Reduces critical path delay → faster circuits.

- Sometimes reduces area if logic sharing is improved.

- Enables better timing closure in sequential designs.

## iv. Sequential Logic Cloning (Floorplan Aware Synthesis)

**Sequential logic cloning** creates duplicate copies of sequential logic elements (flip-flops, registers, small combinational blocks) to improve performance while respecting floorplan constraints.

## ✨ How it works:

- Identify heavily loaded or timing-critical flip-flops.

- Clone sequential blocks and place copies in different locations.

- Reduces wire delay by physically bringing logic closer to where it is used.

## 📌 Example:

```

A single flip-flop drives 4 distant combinational blocks → cloning creates 2 or more copies to reduce interconnect delay.

```
## <img width="50" height="40" alt="image" src="https://github.com/user-attachments/assets/72fd4468-7fea-4563-9e77-a6c72cdb367e" /> Benefits:

- Improves timing by reducing long interconnect delays.

- Reduces routing congestion in floorplanned designs.

- Can slightly increase area, but improves overall performance.

# Labs on Optimization
  
## Lab 1

Below is the Verilog code for Lab 1:

```verilog
module opt_check (input a , input b , output y);
	assign y = a?b:0;
endmodule
```

**Explanation:**
- `assign y = a ? b : 0;` means:
  - If `a` is true, `y` is assigned the value of `b`.
  - If `a` is false, `y` is 0.

### Steps

Follow the steps from [Day 1 Synthesis Lab](https://github.com/bhuvaneshbehara/RTL-to-GDSII_SOC_TapeoutProgram/tree/main/DAY1#8-synthesis-lab-with-yosys) and add the following between `abc -liberty` and `synth -top`:

```shell
opt_clean -purge
```
<div align="center"> <img width="500" height="500" alt="optcheck" src="https://github.com/user-attachments/assets/a7815388-5d64-475d-834e-4b3e5fd55505" /> </div>


## Lab 2

Verilog code:

```verilog
module opt_check2 (input a , input b , output y);
	assign y = a?1:b;
endmodule
```

**Code Analysis:**
- Acts as a multiplexer:
  - `y = 1` if `a` is true.
  - `y = b` if `a` is false
  
<div align="center"> <img width="500" height="500" alt="optcheck2" src="https://github.com/user-attachments/assets/b7562db6-efa3-449b-a0ca-6d686c0f86a2" /> </div>

## Lab 3

Verilog code:

```verilog
module opt_check4 (input a , input b , input c , output y);
 assign y = a?(b?(a & c ):c):(!c);
 endmodule
```

**Functionality:**
- Three inputs (`a`, `b`, `c`), output `y`.
- Nested ternary logic:
  - If `a = 1`, `y = c`.
  - If `a = 0`, `y = !c`.
- Logic simplifies to:  
  `y = a ? c : !c`

 <div align="center"> <img width="500" height="500" alt="optcheck4" src="https://github.com/user-attachments/assets/e02d6fa4-d10e-40a1-8b95-1a4961d1ad8c" /> </div>


## Lab 4

Verilog code:

```verilog
module dff_const1(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
	if(reset)
		q <= 1'b0;
	else
		q <= 1'b1;
end
endmodule
```

**Functionality:**
- D flip-flop with:
  - Asynchronous reset to 0
  - Loads constant `1` when not in reset

    
<div align="center"> <img width="500" height="500" alt="dffconst1tb" src="https://github.com/user-attachments/assets/8bfe982d-fd78-4ae7-b50d-46a3e4413e21" /> </div>

<div align="center"> <img width="500" height="500" alt="dffconst1out" src="https://github.com/user-attachments/assets/09acb296-831a-4475-aee2-c303b4e381a2" /> </div>



## Lab 5

Verilog code:

```verilog
module dff_const2(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
	if(reset)
		q <= 1'b1;
	else
		q <= 1'b1;
end
endmodule
```

**Functionality:**
- D flip-flop always sets output `q` to `1` (regardless of reset or clock).


<div align="center"> <img width="500" height="500" alt="dffconst2tb" src="https://github.com/user-attachments/assets/6ac7d690-c439-46a7-9290-00c5f0ce5e48" /> </div>

<div align="center"> <img width="500" height="500" alt="dffconst2out" src="https://github.com/user-attachments/assets/542736ad-2a1e-4326-af78-e5e69f053bf2" /> </div>


## Lab Work

verilog code:

```
module dff_const4(input clk, input reset, output reg q);
reg q1;
always @(posedge clk, posedge reset)
begin
	if(reset)
  begin
		q <= 1'b1;
    q1<=1'b1;
end
else
begin
		q1 <= 1'b1;
    q  <= q1;
  end
end
endmodule

```
**Functionality**

- D flip-flop dff_const4 always sets output `q` to `1` after reset, regardless of clock.

   - `q1` is constantly set to 1.

   - `q` follows `q1` on the next clock edge, so it quickly becomes `1` and stays `1`. 

  
<div align="center"> <img width="500" height="500" alt="dffconst4out" src="https://github.com/user-attachments/assets/2a2eb279-b4f6-49e4-9e02-e29d213175cd" /> </div>

# Summary

Completing Day 3 of your journey toward mastering the RTL-to-GDSII flow! 🎉

This is what you learned today:

- You explored techniques to improve combinational circuits for faster, smaller, and lower-power logic.

- You practiced methods to optimize sequential circuits, ensuring reliable and efficient storage elements.

- You gained insights into how optimization decisions affect the balance of performance, power, and area.

💡 These skills separate a coder from a chip designer. With optimization in your toolkit, you’re one step closer to building professional-grade SoCs.




