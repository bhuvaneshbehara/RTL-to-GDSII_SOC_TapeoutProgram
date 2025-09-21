# <img width="50" height="50" alt="image" src="https://github.com/user-attachments/assets/449ccc60-6b0b-4476-9a94-65ecfcf457ba" /> Day-1: Introduction to Verilog RTL Design & Synthesis

Welcome to Day 1 of the RTL-to-GDSII SoC Tapeout Program! 🎉

```

Today, your journey into VLSI design begins with a deep dive into simulation and synthesis using cutting-edge open-source tools.

🔹 You’ll discover what an open-source simulator is and get hands-on with Icarus Verilog (iverilog) to simulate Verilog RTL designs.

🔹 You’ll learn the fundamentals of logic synthesis, understanding how RTL transforms into gate-level logic.

🔹 Finally, you’ll roll up your sleeves and perform practical synthesis with Yosys, an open-source tool trusted by designers worldwide.

This is not just theory—it’s about real, practical experience. By the end of the day, you’ll have simulated and synthesized your own designs, taking the very first step toward a complete SoC tapeout. The journey starts here—let’s make it count! 🚀✨

```

---

## Contents of the Day

1. [What is a Simulator, Design, and Testbench?](#1-what-is-a-simulator-design-and-testbench)
2. [Introduction to iverilog based flow](#2-Introduction-to-iverilog-based-flow)
3. [Setup the environment](#3-setup-the-environment)
4. [How to use GTKWave and Iverilog with a 2-to-1 Multiplexer example](#4-How-to-use-GTKWave-and-verilog-with-a-2-to-1-Multiplexer-example)
5. [Verilog Code Analysis](#5-verilog-code-analysis)
6. [Introduction to Yosys & Gate Libraries](#5-introduction-to-yosys--gate-libraries)
7. [Synthesis Lab with Yosys](#6-synthesis-lab-with-yosys)
8. [Summary](#7-summary)

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



## 3. Setup the environment 


###  Step 1: Clone the Repository

```shell
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
cd sky130RTLDesignAndSynthesisWorkshop/verilog_files
```

###  Step 2: Install Required Tools

```shell
sudo apt install iverilog
sudo apt install gtkwave
```


# 4. How to use GTKWave and Iverilog with a 2-to-1 Multiplexer example 

Let’s simulate a simple **2-to-1 multiplexer** using iverilog!


###  Step 1: Simulate the Design

Compile the design and testbench:

```shell
iverilog good_mux.v tb_good_mux.v
```


Run the simulation:

```shell
./a.out
```

View the waveform:

```shell
gtkwave tb_good_mux.vcd
```


## 4. Verilog Code Analysis

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









