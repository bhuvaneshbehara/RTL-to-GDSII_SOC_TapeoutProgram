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
4. [LAB: How to use GTKWave and Iverilog with a 2-to-1 Multiplexer example](#4-LAB:-How-to-use-GTKWave-and-verilog-with-a-2-to-1-Multiplexer-example)
5. [Verilog and test bench code analysis of the 2-to-1 Multiplexer](#5-Verilog-and-test-bench-code-analysis-of-the-2-to-1-Multiplexer)
6. [Overview of Yosys and Setup Guide](#6-Overview-of-Yosys-and-Setup-Guide)
7. [Introduction to Synthesis and gate libraries](#7-Introduction-to-Synthesis-and-gate-libraries)
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

<div align="center">  <img width="530" height="530" alt="image" src="https://github.com/user-attachments/assets/bbd919e1-5683-4807-84c6-751cdfe8cefd" /> </div>


## 2. Introduction to iverilog based flow


Here is an explanation of the iverilog-based simulation flow

* **Design & Test Bench:** The simulation process starts with two main inputs: the **design** (your Verilog code for the circuit you're creating) and the **test bench** (another Verilog module that generates input stimuli and checks the output of your design).
* **Icarus Verilog (iverilog):** These two files are compiled together by the **iverilog** compiler. Icarus Verilog is a free and open-source Verilog simulator. It takes the design and test bench files and creates a simulation executable.
* **VCD File Generation:** During the simulation, the iverilog executable generates a **VCD (Value Change Dump) file**. This file records all the signal changes over time in your simulation. It's a text-based format that logs when and to what value a signal changes.
* **GTKWave:** The VCD file, being a raw text log, isn't easy to read. You use a waveform viewer like **GTKWave** to open and visualize the data from the VCD file. GTKWave presents the signal changes as a graphical **waveform**, allowing you to see the timing relationships and values of all the signals in your design. * **Waveform Visualization:** The final output is the waveform display in GTKWave, which is crucial for debugging and verifying that your design behaves as expected according to the test bench stimuli.

Here’s the typical simulation flow image:


<div align="center"> <img width="530" height="530" alt="Iverilog simulation flow" src="https://github.com/user-attachments/assets/4106699f-ab7c-41e3-8310-2e40b965e325" /> </div>



## 3. Install the Required Tools 
 
If installed already then ignore this

```
sudo apt install iverilog
sudo apt install gtkwave
```


## 4. LAB: How to use GTKWave and Iverilog with a 2-to-1 Multiplexer example 

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

<div align="center"> <img width="500" height="500" alt="out" src="https://github.com/user-attachments/assets/52283d57-7ead-42f3-9557-7cc7bdb0bf06" /> </div>



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

<div align="center"> <img width="500" height="500" alt="MUX and tb" src="https://github.com/user-attachments/assets/1ed69a51-6dc3-48c0-91e8-3b06f2d94640" /> </div>



## 6. Overview of Yosys and Setup Guide 

  Before introducing yosys tool, lets know about synthesizer.

### What is synthesizer?

  **Synthesizer** is a tool which converts RTL to gate-level netlist. Here, we are using yosys as a synthesizer tool.

###  What is Yosys?

**Yosys** is a powerful open-source synthesis tool for digital hardware. It takes your Verilog code and converts it into a gate-level netlist—a hardware blueprint.

#### **Yosys** Features
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

## **Yosys** tool setup

```
Inputs:

Verilog Design file → read_verilog.

Library file (.lib) (defines standard cells) → read_liberty.

Yosys processes them to perform logic synthesis.

Output: Netlist file → written using write_verilog.

In short: Yosys takes design + library, runs synthesis, and generates a netlist.

```

Here’s the typical yosys setup image:

<div align="center"> <img width="500" height="500" alt="Yosys image" src="https://github.com/user-attachments/assets/828c1edb-67db-4633-a412-c20c5da9e814" /> </div>


## How to verify the Synthesis 

Note: Primary inputs/outputs don’t change → the same testbench can be reused.

```
Inputs: Netlist (from synthesis) + Testbench.

Both are given to iverilog (the simulator).

Output is a VCD file that shows waveforms.

Open in GTKWave to compare behavior.

Key point: If the synthesized netlist produces the same output as the RTL simulation → synthesis is correct.


```

Here’s the typical verifying synthesis image:

<div align="center"> <img width="500" height="500" alt="verify synthesis" src="https://github.com/user-attachments/assets/b2e2bd73-a4b1-470e-ae6b-fc7aa917881f" /> </div>






## 7. Introduction to Synthesis and gate libraries

First, look at synthesis and then dive into gate libraries
 
### What is synthesis? 

 **synthesis** means that converting from one form of level to another form of level. 


 so, lets dive into what is logic synthesis?

 ## What is logic synthesis?

 **Logic synthesis** means that converting from RTLcode into optimized gate-level netlist w.r.t Performace, Power and Timing (PPA).

## Gate libraries

## What is .lib?

 **.lib file** is a short of liberty timing file. It is an ASCII representation of timing, power parameter associated with cells inside the std cell library of a particular technology node.

## Why Do Libraries Have Different Gate "Flavors"?

  Libraries have different **gate “flavors”** because a single logic gate (like NAND, NOR, INV) can be implemented in **multiple variants** to optimize for **speed, area, or power** depending on design needs.

Here’s why:

---

### 🔹 1. **Drive Strength Variants**

* Same gate is available with different **drive strengths** (e.g., INV\_X1, INV\_X2, INV\_X4).
* Higher drive strength = stronger transistor sizes → faster but larger area and higher power.
* Lower drive strength = smaller, slower but saves area and power.

---

### 🔹 2. **Threshold Voltage (Vt) Variants**

* **HVT (High Vt):** Low leakage, slower → good for low-power designs.
* **SVT (Standard Vt):** Balanced performance and leakage.
* **LVT (Low Vt):** Fast, but higher leakage → used in speed-critical paths.

---

### 🔹 3. **Functional Variants**

* A library may include **different logic implementations** (like AOI, OAI, MUX, complex gates).
* Using a more complex gate instead of multiple simple gates reduces delay and area.

---

### 🔹 4. **Special Purpose Variants**

* Some gates optimized for **low-power**, **low-noise**, or **high-density**.
* Multi-bit flip-flops, clock-gating cells, isolation cells, etc.

---


## 8. Synthesis Lab with Yosys

Let’s synthesize the `good_mux` design using Yosys!


###  Yosys Flow: A Step-by-Step Guide

1. **Start Yosys**

   ```
    yosys

    ```

3. **Read the liberty library**

   ```
   
    read_liberty -lib /address/to/your/sky130/file/sky130_fd_sc_hd__tt_025C_1v80.lib

    ```

5. **Read the Verilog code**
    ```

    read_verilog /home/vsduser/VLSI/sky130RTLDesignAndSynthesisWorkshop/verilog_files/good_mux.v

    ```

7. **Synthesize the design**
    ```
    synth -top good_mux

    ```

8. **Technology mapping**
    ```

    abc -liberty /address/to/your/sky130/file/sky130_fd_sc_hd__tt_025C_1v80.lib

    ```
   <div align="center"> <img width="660" height="206" alt="abc" src="https://github.com/user-attachments/assets/7710c08f-e5e7-417a-8f13-789b78f25678" /> </div>

	

9. **Visualize the gate-level netlist**

    ```

    show

    ```

<div align="center"> <img width="600" height="600" alt="GLN" src="https://github.com/user-attachments/assets/cd2496e7-ca74-4d80-bbf6-ace8581f91d9" /> </div>

10. **Generating the Finalized Gate-Level Netlist**

```
write_verilog -noattr good_mux_netlist.v

```
<div align="center"> <img width="500" height="500" alt="netlist" src="https://github.com/user-attachments/assets/3664b79f-93a0-42ce-9923-2ec0a5883225" /> </div>



## 9. Summary

Completing Day 1 of your journey toward mastering the RTL-to-GDSII flow! 🎉

This is what you learned today:

* Understood the importance of simulators, designs, and testbenches in digital design verification.

* Ran your first Verilog simulation with iverilog and visualized real signal activity using GTKWave.

* Explored and analyzed a 2-to-1 multiplexer design, connecting theory with practical simulation.

* Discovered Yosys for synthesis and learned why standard cell libraries come in different flavors to optimize for speed, power, and area.

💡 These are not just small steps—they are the building blocks of every modern chip design. Each concept you practiced today forms the foundation for more advanced stages in the tapeout flow.


















