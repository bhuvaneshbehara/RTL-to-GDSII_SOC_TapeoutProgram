
# <img width="50" height="50" alt="image" src="https://github.com/user-attachments/assets/26232741-3a60-4be7-9c70-30c6e76345f6" /> Day-2: Timing Libraries, Synthesis Approaches, and Efficient Flip-Flop Coding

🚀 Welcome to Day 2 of the RTL-to-GDSII SoC Tapeout Program! 🎉

You’ve already taken your first steps into simulation and synthesis—now it’s time to go deeper into the world of RTL design. Today’s session will strengthen your foundation with three crucial topics:

```
🔹 Understanding the .lib Timing Library

– Dive into the sky130_fd_sc_hd__tt_025C_1v80.lib, the cornerstone of open-source PDKs. Learn how this library captures timing, power, and functional characteristics of standard cells that bring your design to life.

🔹 Hierarchical vs. Flat Synthesis

– Discover the trade-offs between these two synthesis strategies. Learn how the choice impacts design performance, optimization, and scalability, preparing you for real-world chip challenges.

🔹 Efficient RTL Coding for Flip-Flops

– Master coding styles that make flip-flops both synthesis-friendly and timing-efficient, ensuring your designs are robust and implementation-ready.

✨ By the end of Day 2, you won’t just understand libraries, synthesis strategies, and RTL practices—you’ll be thinking like a true VLSI designer, making smart choices for performance, area, and power.

```
---

📘 Contents of the Day 

🔹[Timing Libraries](#timing-libraries)

  - [SKY130 PDK Overview](#sky130-pdk-overview)

    - [Introduction to the Sky Water open-source PDK and its role in digital design](#introduction-to-sky-water-open-source-pdk-and-its-role-in-digital-design)

  - [Decoding tt_025C_1v80 in the SKY130 PDK](#decoding-tt_025c_1v80-in-the-sky130-pdk)

    - [Understanding process, voltage, and temperature (PVT) corners](#understanding-process-voltage-and-temperature-pvt-corners)

  - [Opening and Exploring the .lib File](#opening-and-exploring-the-lib-file)

    - [Structure of a Liberty file and the key information it provides (cells, pins, timing, power)](#structure-of-a-liberty-file-and-the-key-information-it-provides-cells-pins-timing-power)

🔹[Hierarchical vs. Flattened Synthesis](#hierarchial-vs-flattened-synthesis)

  - [Hierarchical Synthesis](#hierarchical-synthesis)

  - [Flattened Synthesis](#flattened-synthesis)


  - [Key Differences](#key-differences)

    - [Comparing trade-offs in terms of performance, area, and design complexity](#comparing-trade-offs-in-terms-of-performance-area-and-design-complexity)

🔹[Flip-Flop Coding Styles](#flip-flop-coding-styles)

  - [Asynchronous Reset D Flip-Flop](#asynchronous-reset-d-flip-flop)

    - [Coding and timing behavior with asynchronous reset control](#coding-and-timing-behaviour-with-asynchronous-reset-control)

  - [Asynchronous Set D Flip-Flop](#asynchronous-set-d-flip-flop)

    - [Coding and timing behavior with asynchronous set control](#coding-and-timing-behaviour-with-asynchronous-set-control)

  - [Synchronous Reset D Flip-Flop](#synchronous-reset-d-flip-flop)

    - [Coding and timing behavior with synchronous reset control](#coding-and-timing-behaviour-with-synchronous-reset-control)

🔹[Simulation and Synthesis Workflow](#simulation-and-synthesis-workflow)

  - [Icarus Verilog Simulation](#icarus-verilog-simulation)

    - [Running testbenches and visualizing waveforms using GTKWave](#running-testbenches-and-visualizing-waveform-using-gtkwave)

- [Synthesis with Yosys](#synthesis-with-yosys)

    - [Translating RTL into a gate-level netlist](#translating-rtl-into-a-gate-level-netlist)
  
🔹[Summary](#summary)

---

# 🔹Timing Libraries

 ## SKY130 PDK Overview

  ##  Introduction to the Sky Water open-source PDK and its role in digital design

  **PDK (Process Design Kit):** A collection of files that describe how a chip is fabricated in a particular technology node.

  **SkyWater SKY130:** An open-source 130nm CMOS PDK, released by SkyWater Technology in collaboration with Google.

  
## Role

- Supports both digital and analog design flows.

- Provides:

  - Standard cell libraries (.lib, .lef, .gds) for synthesis and layout.

  - Device models (SPICE) for simulation.

  - DRC/LVS rule decks for physical verification.

- Widely used in open-source EDA flows (like OpenLane, Yosys, Magic, KLayout).

 In short: SKY130 is the world’s first open-source industrial-grade PDK that makes chip design accessible to everyone.

---

## 🔎 Decoding tt_025C_1v80 in the SKY130 PDK

This naming refers to a PVT corner (Process, Voltage, Temperature) used in timing libraries.

- tt → Typical-Typical process corner (transistors behave at nominal/average speed).

- 025C → Temperature = 25°C (room temperature).

- 1v80 → Supply voltage = 1.80 V.

---

## Understanding process, voltage, and temperature (PVT) corners


### 🔎 PVT Corners in VLSI

  A PVT corner represents the different operating conditions under which a chip’s timing and power must be verified. Since real silicon never behaves ideally, we test designs across these variations.

1️⃣ Process (P) : manufacturing variations

Due to fabrication differences, transistors may turn out faster or slower.

Common process corners:

- TT (Typical-Typical): Nominal/average behavior.

- FF (Fast-Fast): Both NMOS & PMOS are faster than typical.

- SS (Slow-Slow): Both NMOS & PMOS are slower.

- FS / SF: One device is fast, the other is slow.

2️⃣ Voltage (V) : supply voltage variations

Chips may run at slightly higher or lower voltages due to design or environment.

Example: In SKY130, nominal is 1.8 V, but libraries also exist for 1.6 V and 1.95 V.

- Lower voltage → slower but less power.

- Higher voltage → faster but more power and heat.

3️⃣ Temperature (T) : environmental variations

Devices behave differently at different temperatures.

Example: -40°C, 25°C, 85°C, 125°C.

- High temperature → slower switching.

- Low temperature → faster but risk of reliability issues.

### ✅ Why PVT matters:

Designs must meet timing and power requirements across all PVT corners to ensure chips work reliably in real-world conditions.

---


## Opening and Exploring the .lib File

To open the sky130_fd_sc_hd__tt_025C_1v80.lib file:

1. **Install a text editor:**

   ```
   sudo apt install vim-gtk3
   ```
   
2. **Open the file:**

   ```
   gvim sky130_fd_sc_hd__tt_025C_1v80.lib

   ```
<div align="center"> <img width="700" height="700" alt="lib file" src="https://github.com/user-attachments/assets/f7cba608-4cbc-4837-8210-7e4f080b47c7" /> </div>

 
---

## Structure of a Liberty file and the key information it provides (cells, pins, timing, power)

### Structure of a Liberty File (.lib)

  A Liberty file is a text-based file that describes the characteristics of standard cells for synthesis, timing, and power analysis.

##### 🔑 Key Information Inside a .lib

```
1. Library Header

  --> Defines units (time, voltage, power, capacitance).

  --> Example: time_unit : "1ns";

2. Cell Definitions

  --> Each standard cell (e.g., INV, NAND2, DFF) is described in a cell (...) {} block.

3. Pins

  --> Inside each cell, pin (...) {} defines:

    ---> Direction (input/output/inout).

    ---> Function (logic equation for outputs).

    ---> Capacitance (input load).

4. Timing Information

  --> Delay, setup, hold times using lookup tables.

  --> Describes how input changes affect output timing.

5. Power Information

  --> Internal power, leakage power, dynamic power.

```
In short: A .lib file tells the tool what cells exist, what pins they have, how fast they are, and how much power they consume.

---

# 🔹Hierarchical vs. Flattened Synthesis
  
### Hierarchical Synthesis

- **Definition**: Retains the module hierarchy as defined in RTL, synthesizing modules separately.
- **How it Works**: Tools like Yosys process each module independently, using commands such as `hierarchy` to analyze and set up the design structure.

**Advantages:**
- Faster synthesis time for large designs.
- Improved debugging and analysis due to maintained module boundaries.
- Modular approach, aiding integration with other tools.

**Disadvantages:**
- Cross-module optimizations are limited.
- Reporting can require additional configuration.

## Example: 

<div align="center"> <img width="500" height="500" alt="hierarchy" src="https://github.com/user-attachments/assets/c7cb7d69-70c8-4473-9304-e8ce0dc8657b" /> </div>
 


### Flattened Synthesis

- **Definition**: Merges all modules into a single flat netlist, eliminating hierarchy.
- **How it Works**: The `flatten` command in Yosys collapses the hierarchy, allowing whole-design optimizations.

**Advantages:**
- Enables aggressive, cross-module optimizations.
- Results in a unified netlist, sometimes simplifying downstream processes.

**Disadvantages:**
- Longer runtime for large designs.
- Loss of hierarchy complicates debugging and reporting.
- Can increase memory usage and netlist complexity.

## Example: 
<div align="center"> <img width="700" height="700" alt="flatten1" src="https://github.com/user-attachments/assets/1076bc13-de98-4d07-a82e-50b94c416e87" />
</div>



# Key Differences
  ## Comparing trade-offs in terms of performance, area, and design complexity

|Feature	         |Hierarchical	            |Flattened                   |
|------------------|--------------------------|----------------------------|
|Optimization	     |Moderate (per block)	    |High (whole design)         |
|Performance	     |Slightly lower	          |Higher                      |
|Area	Slightly     |larger	                  |Smaller (more efficient)    |
|Design Complexity |Low (manageable)	        |High (hard to debug/verify) |

---

# 🔹Flip-Flop Coding Styles

## Asynchronous Reset D Flip-Flop

### Definition:

A D Flip-Flop where the reset (usually rst or reset) acts independently of the clock.

  - When the reset is asserted, the flip-flop output Q is immediately cleared (or set), regardless of the clock edge.

  - Use Case: Ensures that a circuit can be initialized immediately during power-up or in case of errors, without waiting for a clock transition.

## Coding and timing behavior with asynchronous reset control

### verilog code example

```
module async_reset_dff (input wire clk,input wire rst_n, input wire D,output reg Q);

always @(posedge clk or negedge rst_n) begin
    if (!rst_n)         // asynchronous reset
        Q <= 1'b0;      // reset output to 0 immediately
    else
        Q <= D;         // capture input on clock edge
end

endmodule

```

### Explanation:

```
@(posedge clk or negedge rst_n) → sensitivity list includes clock and reset.

if (!rst_n) → if reset is active, output is immediately set to 0.

else Q <= D; → normal D Flip-Flop behavior on clock edge.
```

## Behavior

|Clock	| Reset	 |  D	  |  Q (next)            |
|-------|--------|------|----------------------|
|0→1	  | 0	     |  X	  |  0                   |
|0→1	  | 1	     |  1	  |  1                   |
|Any	  | 0	     |  X	  |  0 (immediate, async)|

- Even if the clock does not transition, asserting reset immediately drives Q to 0.

- Once reset is deasserted, the flip-flop resumes normal clocked operation.
  

## ASynchronous set D Flip-Flop]

### Definition:

A D Flip-Flop where the set input (set or preset) acts independently of the clock.

  - When the set is asserted, the flip-flop output Q is immediately set to 1, regardless of the clock edge.

  - Use Case: Ensures that a circuit can be initialized or forced to 1 immediately during power-up or error recovery.

## Coding and timing behavior with asynchronous set control


### verilog code example

```
module async_set_dff (input wire clk, input wire set_n, input wire D, output reg Q);

always @(posedge clk or negedge set_n) begin
    if (!set_n)         // asynchronous set
        Q <= 1'b1;      // set output to 1 immediately
    else
        Q <= D;         // normal D Flip-Flop behavior on clock edge
end

endmodule

```
### Explanation:

```
@(posedge clk or negedge set_n) → sensitivity list includes clock and set.

if (!set_n) → if set is active, output is immediately driven to 1.

else Q <= D; → normal D Flip-Flop behavior on clock edge.
```

## Behaviour

|Clock	 | set  	| D	      | Q (next)                                |
|--------|--------|---------|-----------------------------------------|
|0→1	   | 0      |	X	      | 1                                       |
|0→1	   | 1	    | 1	      | 1                                       |
|Any	   | 0	    | x       |  1                                      |

- Asserting set immediately drives Q to 1, regardless of the clock.

- Once set is deasserted, the flip-flop resumes normal clocked operation.

## Synchronous Reset D Flip-Flop

### Definition:

A D Flip-Flop where the reset (rst) affects the output only on the active clock edge.

  - The output Q changes to 0 (or preset value) synchronously with the clock.

  - Use Case: Ensures reset aligns with the clock domain, avoiding timing or glitch issues in fully synchronous designs.

## Coding and timing behavior with synchronous reset control


### Verilog code example

```
module sync_reset_dff (input wire clk, input wire rst_n, input wire D, output reg Q);

always @(posedge clk) begin
    if (!rst_n)       // synchronous reset
        Q <= 1'b0;    // reset output on clock edge
    else
        Q <= D;       // normal D Flip-Flop behavior
end

endmodule
```
### Explanation:

```
@(posedge clk) → sensitive only to clock edge.

if (!rst_n) → reset applied only at clock edge.

else Q <= D; → captures input D normally on clock edge
```

## Behavior 

|Clock	  |Reset	   | D	       | Q (next)                        |
|---------|----------|-----------|---------------------------------|
|0→1	    | 0	       |X	         | 0 (reset applied on clock edge) |
|0→1	    | 1	       | 1	       | 1 (normal operation)            |
|0→1	    | 0	       | 1	       | 0 (reset applied at clock edge) |

- Reset waits for the next clock edge to take effect.

- Normal operation resumes once reset is inactive.


----

# 🔹Simulation and Synthesis Workflow


## Icarus Verilog Simulation

### Running testbenches and visualizing waveforms using GTKWave

1. **Compile:**

   ```

   iverilog dff_syncres.v tb_dff_syncres.v

   ```
2. **Run:**

   ```
   
   ./a.out

   ```
3. **View Waveform:**
   ```
   

   gtkwave tb_dff_syncres.vcd

   ```
<div align="center"> <img width="700" height="700" alt="gtkwave syncres" src="https://github.com/user-attachments/assets/4efe4bfe-7f03-40a2-a26f-60e46867e0f0" />
</div>
  




## Synthesis with Yosys

  ### Translating RTL into a gate-level netlist
  
1. Start Yosys:
   ```
   
   yosys

   ```
2. Read Liberty library:

   ```

   read_liberty -lib /address/to/your/sky130/file/sky130_fd_sc_hd__tt_025C_1v80.lib

   ```
4. Read Verilog code:
   ```

   read_verilog /path/to/dff_syncres.v

   ```
5. Synthesize:

   ```

   synth -top dff_syncres

   ```
7. Map flip-flops:

   ```

   dfflibmap -liberty /address/to/your/sky130/file/sky130_fd_sc_hd__tt_025C_1v80.lib

   ```
9. Technology mapping:

   ```

   abc -liberty /address/to/your/sky130/file/sky130_fd_sc_hd__tt_025C_1v80.lib

   ```
11. Visualize the gate-level netlist:
   
   ```

show

   ```

<div align="center"> <img width="700" height="700" alt="dffsyncres" src="https://github.com/user-attachments/assets/58e7a8cf-78b7-4e8a-b875-057fc7af3121" />
</div>

---

# 🔹Summary

 Completing Day 1 of your journey toward mastering the RTL-to-GDSII flow! 🎉
  
This is what you learned today:

- You understood the role of the .lib timing library (sky130_fd_sc_hd__tt_025C_1v80.lib) and how it defines the behavior, timing, and power of standard cells in open-source PDKs.

- You compared hierarchical and flat synthesis, learning the trade-offs between modular design flexibility and global optimization.

- You practiced efficient RTL coding styles for flip-flops, ensuring your designs are both synthesis-friendly and timing-robust.

💡 With these insights, you’ve taken a big step toward thinking like a chip architect, making conscious decisions that impact performance, area, and power.



