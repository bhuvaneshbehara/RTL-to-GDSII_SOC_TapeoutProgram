# <img width="50" height="50" alt="image" src="https://github.com/user-attachments/assets/449ccc60-6b0b-4476-9a94-65ecfcf457ba" /> Day-1: Introduction to Verilog RTL Design & Synthesis

Welcome to Day 1 of the RTL-to-GDSII SoC Tapeout Program! 🎉

Today, your journey into VLSI design begins with a deep dive into simulation and synthesis using cutting-edge open-source tools.

```
🔹 You’ll discover what an open-source simulator is and get hands-on with Icarus Verilog (iverilog) to simulate Verilog RTL designs.

🔹 You’ll learn the fundamentals of logic synthesis, understanding how RTL transforms into gate-level logic.

🔹 Finally, you’ll roll up your sleeves and perform practical synthesis with Yosys, an open-source tool trusted by designers worldwide.

This is not just theory—it’s about real, practical experience. By the end of the day, you’ll have simulated and synthesized your own designs, taking the very first step toward a complete SoC tapeout. The journey starts here—let’s make it count! 🚀✨

```

---

## Contents of the Day

1. [What is a Simulator, Design, and Testbench?](#1-what-is-a-simulator-design-and-testbench)
2. [Introduction to iverilog based flow](#2-Introduction-to-iverilog-based-flow)
3. [Install the Required Tools ](#3-Install-the-Required-Tools)
4. [How to use GTKWave and Iverilog with a 2-to-1 Multiplexer example](#4-How-to-use-GTKWave-and-verilog-with-a-2-to-1-Multiplexer-example)
5. [Verilog and test bench code analysis of the 2-to-1 Multiplexer](#5-Verilog-and-test-bench-code-analysis-of-the-2-to-1-Multiplexer)
6. [Overview of Yosys and Setup Guide](#6-Overview-of-Yosys-and-Setup-Guide)
7. [Introduction to Synthesis and .lib](#7-Introduction-to-Synthesis-and-.-lib)
8. [Synthesis Lab with Yosys](#8-synthesis-lab-with-yosys)
9. [Summary](#7-summary)

---


## 1. What is a Simulator, Design, and Testbench?

## simulator

A simulator is a software tool that executes a design model along with its testbench to mimic the behavior of a digital circuit. It evaluates how the circuit responds over time by processing the input signals and applying the defined logic of the design.

In the context of Verilog, a Verilog simulator allows designers to:

---> Execute and verify the functionality of a Verilog design.

---> Test the design with a testbench before moving to actual hardware implementation.

---> Observe and debug the circuit’s behavior in a safe, virtual environment.

In short, a simulator helps validate the correctness of a design at the RTL (Register Transfer Level) before it progresses to synthesis and physical design, saving both time and cost.


## Design

A design in Verilog is the hardware description (RTL code) that specifies how a digital circuit should work. It defines the structure and behavior of the circuit using modules, inputs, outputs, and logic.

## Testbench

A test bench is a simulation-only environment written in a Hardware Verification Language (HVL) like Verilog or SystemVerilog that applies input stimuli to a Design Under Test (DUT) and checks its output responses to verify the design's functional correctness.

<div align="center">  <img width="730" height="250" alt="image" src="https://github.com/user-attachments/assets/bbd919e1-5683-4807-84c6-751cdfe8cefd" /> </div>


## 2. Introduction to iverilog based flow


Here is an explanation of the iverilog-based simulation flow

* **Design & Test Bench:** The simulation process starts with two main inputs: the **design** (your Verilog code for the circuit you're creating) and the **test bench** (another Verilog module that generates input stimuli and checks the output of your design).
* **Icarus Verilog (iverilog):** These two files are compiled together by the **iverilog** compiler. Icarus Verilog is a free and open-source Verilog simulator. It takes the design and test bench files and creates a simulation executable.
* **VCD File Generation:** During the simulation, the iverilog executable generates a **VCD (Value Change Dump) file**. This file records all the signal changes over time in your simulation. It's a text-based format that logs when and to what value a signal changes.
* **GTKWave:** The VCD file, being a raw text log, isn't easy to read. You use a waveform viewer like **GTKWave** to open and visualize the data from the VCD file. GTKWave presents the signal changes as a graphical **waveform**, allowing you to see the timing relationships and values of all the signals in your design. * **Waveform Visualization:** The final output is the waveform display in GTKWave, which is crucial for debugging and verifying that your design behaves as expected according to the test bench stimuli.

Here’s the typical simulation flow image:


<div align="center"> <img width="730" height="730" alt="Iverilog simulation flow" src="https://github.com/user-attachments/assets/4106699f-ab7c-41e3-8310-2e40b965e325" /> </div>



## 3. Install the Required Tools 
 
If installed already then no need this

```
sudo apt install iverilog
sudo apt install gtkwave
```


# 4. How to use GTKWave and Iverilog with a 2-to-1 Multiplexer example 

Let’s simulate a simple **2-to-1 multiplexer** using iverilog and gtkwave.


###  Step 1: Clone the Repository

```
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git

cd sky130RTLDesignAndSynthesisWorkshop/verilog_files

```

###  Step 2: Simulate the Design

Compile the design and testbench:

```
iverilog good_mux.v tb_good_mux.v
```


Run the simulation:

```
./a.out
```

View the waveform:

```
gtkwave tb_good_mux.vcd
```


## 5. Verilog and test bench code analysis of the 2-to-1 Multiplexer

**The code for the multiplexer (`good_mux.v`):**

```verilog
module good_mux (input i0, input i1, input sel, output reg y);
always @ (*)
begin
    if(sel)
        y <= i1;
    else 
        y <= i0;
end
endmodule
```

###  **How It Works**

- **Inputs:** `i0`, `i1` (data), `sel` (select line)
- **Output:** `y` (registered output)
- **Logic:** If `sel` is 1, `y` gets `i1`; if `sel` is 0, `y` gets `i0`.


```Verilog with testbench
`timescale 1ns / 1ps
module tb_good_mux;
	// Inputs
	reg i0,i1,sel;
	// Outputs
	wire y;

        // Instantiate the Unit Under Test (UUT)
	good_mux uut (
		.sel(sel),
		.i0(i0),
		.i1(i1),
		.y(y)
	);

	initial begin
	$dumpfile("tb_good_mux.vcd");
	$dumpvars(0,tb_good_mux);
	// Initialize Inputs
	sel = 0;
	i0 = 0;
	i1 = 0;
	#300 $finish;
	end

always #75 sel = ~sel;
always #10 i0 = ~i0;
always #55 i1 = ~i1;
endmodule
```

## **How It Works**

- At t=0, all inputs (sel, i0, i1) = 0.

- Signals start toggling at their specified delays.

    * i0 → fastest changing (every 10 ns).

    * i1 → medium speed (every 55 ns).

    * sel → slowest (every 75 ns).

- The DUT (good_mux) receives these inputs and produces output y accordingly.

- All changes are recorded in tb_good_mux.vcd, so you can open it in GTKWave and watch the waveforms.

- At t=300 ns, simulation ends.

Here’s the typical Verilog and test bench code of 2-to-1 Multiplexer image:




## 6. Overview of Yosys and Setup Guide 

  Before introducing yosys tool, lets know about synthesizer.

### What is synthesizer?

  **Synthesizer** is a tool which converts RTL to gate-level netlist. Here, we are using yosys as a synthesizer tool.

###  What is Yosys?

**Yosys** is a powerful open-source synthesis tool for digital hardware. It takes your Verilog code and converts it into a gate-level netlist—a hardware blueprint.

#### Yosys Features
```
--> Open-source logic synthesis tool for Verilog designs.

--> Supports RTL synthesis (Verilog → gate-level netlist).

--> Provides technology mapping to standard cell libraries.

--> Performs optimization (area, timing, redundancy removal).

--> Supports formal verification (equivalence checking).

--> Can generate netlists in formats like BLIF, EDIF, JSON, etc.

--> Integrates with ABC tool for logic optimization and mapping.

--> Extensible with custom passes and plugins.

--> Widely used in OpenLANE and RTL-to-GDSII flows.

```

## Yosys tool setup

```
Inputs:

Verilog Design file → read_verilog.

Library file (.lib) (defines standard cells) → read_liberty.

Yosys processes them to perform logic synthesis.

Output: Netlist file → written using write_verilog.

In short: Yosys takes design + library, runs synthesis, and generates a netlist.

```

Here’s the typical yosys setup image:



## Verify the Synthesis 

Note: Primary inputs/outputs don’t change → the same testbench can be reused.

```
Inputs: Netlist (from synthesis) + Testbench.

Both are given to iverilog (the simulator).

Output is a VCD file that shows waveforms.

Open in GTKWave to compare behavior.

Key point: If the synthesized netlist produces the same output as the RTL simulation → synthesis is correct.


```

Here’s the typical verifying synthesis image:







## 7. Introduction to Synthesis and .lib

First, look at synthesis and then dive into gate libraries
 
### What is synthesis? 

```
 **synthesis** means that converting from one form of level to another form of level. 

```

 so, lets dive into what is logic synthesis?

 ## What is logic synthesis?

 **Logic synthesis** means that converting from RTLcode into optimized gate-level netlist w.r.t Performace, Power and Timing (PPA).

## What is .lib, and what are its contents?

## What is .lib?

 **.lib file** is a short of liberty timing file. It is an ASCII representation of timing, power parameter associated with cells inside the std cell library of a particular technology node.

## Contents of .lib

```
Cell definitions → list of all standard cells (e.g., NAND2, NOR2, INV, DFF).

Pin information → input/output pins, direction, function.

Timing data → delay, setup, hold times, clock-to-Q, etc.

Power data → dynamic and leakage power.

Constraints → drive strength, fanout limits, load capacitance.

```






















