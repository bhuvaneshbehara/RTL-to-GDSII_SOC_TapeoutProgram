

# <img width="60" height="60" alt="image" src="https://github.com/user-attachments/assets/0fc94fea-b4ab-4ed1-aca0-eebd6ccb00f7" /> BabySoC Fundamentals & Functional Modelling 

# Part-1 – Theory (Conceptual Understanding) of Soc



# contents of the day

1. [What is a System-on-Chip (SoC)?](#1-What-is-a-System-on-Chip-(SoC)?)
   - [Why SoC?](#why-soc?)
   - [Features of SoC](#features-of-soc)
   - [Challenges with SoC](#challenges-with-soc)
   - [How SoC will work?](#how-soc-will-work)     
2. [Components of a typical SoC](#2-components-of-a-typical-soc)
3. [Types of SoC](#3-types-of-soc)
4. [Introduction to BabySoC](#4-introduction-to-babysoc)
5. [Why BabySoC is a simplified model for learning SoC concepts?](#4-why-babysoc-is-a-simplified-model-for-learning-soc-concepts?)
6. [The role of functional modelling before RTL and physical design stages](#5-the-role-of-functional-modelling-before-rtl-and-physical-design-stages)
7. [Summary](#summary)


## What is a System-on-Chip (SoC)?

- A System-on-Chip (SoC) is an integrated circuit (IC) that combines all the essential components of a complete computer or electronic system into a single chip.

- Instead of using multiple chips on a board (CPU, memory, I/O controllers), an SoC integrates them into one silicon die.

## Why SoC?

A System-on-Chip (SoC) is chosen because modern applications (like smartphones, IoT devices, AI accelerators, automotive electronics) demand high performance, low power, small size, and cost efficiency.

SoC integrates everything into one chip, providing:

- High performance: Faster data transfer (no off-chip delays).

- Low power: Shorter interconnects, optimized hardware.

- Reduced size: Ideal for mobile, IoT, and embedded systems.

- Lower cost per unit: Mass production is cheaper than multiple ICs.


## Features of SoC

- **Integration** – Combines CPU, memory, I/O interfaces, and custom accelerators (GPU, DSP, AI engine).

- **Compact Size** – Small footprint compared to multi-chip systems.

- **High Performance** – On-chip interconnect reduces latency; specialized blocks accelerate tasks (e.g., GPUs for graphics, NPUs for AI).

- **Low Power** – Optimized power management techniques (DVFS, clock gating, power gating).

- **Customizability** – Different SoCs designed for specific applications (e.g., Qualcomm Snapdragon for mobile, NVIDIA Tegra for automotive, Apple M-series for laptops).

- **Heterogeneous Computing** – Multiple cores and specialized processors work together (CPU + GPU + DSP + NPU).

- **High-Speed Interfaces** – USB, PCIe, DDR, Ethernet, etc.


## Challenges with SoC

- **Design Complexity** – Integration of billions of transistors and multiple IP blocks.

- **Verification Effort** – Ensuring all modules (CPU, memory, peripherals) interact correctly is time-consuming.

- **Power & Thermal Management** – Packing high-performance components in small area leads to heating issues.

- **Manufacturing Cost** – Initial design and fabrication cost (tape-out) is very high.

- **Scalability** – Hard to upgrade one part (e.g., CPU) without redesigning the entire SoC.

- **Testing & Debugging** – Debugging at chip-level is harder compared to board-level systems.

- **Security Concerns** – Multiple third-party IPs may introduce vulnerabilities.

## How SoC Will Work?

The working of an SoC can be explained in functional flow:

1. CPU Executes Instructions

    - The CPU fetches instructions from on-chip memory (ROM/Flash) or external memory (DDR).

    - Instructions include arithmetic, logic, control, and data transfer.

2. Memory Operations

    - On-chip SRAM/Cache stores temporary data for high-speed access.

    - Larger storage (e.g., DDR/Flash) is accessed through memory controllers.

3. Interconnect / Bus System

    - AXI/AHB/APB or Network-on-Chip ensures communication between CPU, memory, and peripherals.

    - Acts like "highways" for data flow inside the chip.

4. Peripherals and I/O

    - Input from sensors, keyboard, or wireless signals enters via peripherals (GPIO, UART, SPI, I²C).

    - Output is sent to displays, actuators, or communication modules.

5. Specialized Blocks

    - GPU handles graphics/image processing.

    - DSP processes real-time signals.

    - NPU/AI engine accelerates machine learning workloads.

6. Power & Clock Management

    - Dynamic Voltage and Frequency Scaling (DVFS), clock gating, and power gating reduce power when blocks are idle.

👉 **Example**:

In a smartphone SoC (like Qualcomm Snapdragon or Apple A-series):

  - CPU runs the operating system and apps.

  - GPU renders the display and gaming graphics.

  - ISP (Image Signal Processor) processes camera input.

  - Modem block handles 4G/5G connectivity.

  - Memory subsystem provides fast access for apps and OS.

  - Interconnect ties it all together for smooth operation.



<div align="center"> <img width="500" height="500" alt="Soc Wok" src="https://github.com/user-attachments/assets/57e26984-b6ac-4b51-8a4e-fdc1727a4f94" /> </div>

## 2. Components of a typical SoC

A typical **SoC** consists of four main building blocks:

1. Central Processing Unit (CPU)

    - The “brain” of the chip.

    - Executes instructions and controls overall operations.

    - Could be ARM Cortex, RISC-V, or custom-designed cores.

    - Often includes multiple cores for parallelism.

2. Memory Subsystem

    - On-chip memory (SRAM, cache) for fast access.

    - Off-chip memory controller for DRAM, Flash, DDR.

    - Provides storage for instructions and data.

    - Critical for performance, as memory bandwidth often limits system speed.

3. Peripherals & Accelerators

    - Interfaces for external communication (UART, SPI, I²C, USB, Ethernet).

    - Application-specific accelerators (GPU, DSP, AI/ML blocks, Crypto engines).

    - Manage I/O operations and reduce CPU workload.

4. Interconnect (Bus/Fabric/NoC)

    - Communication backbone that links CPU, memory, and peripherals.

    - Could be a shared bus (AMBA, Wishbone), crossbar switch, or Network-on-Chip (NoC) for scalability.

    - Ensures efficient data movement with arbitration, QoS, and bandwidth control.



## 3. Types of SoC

  In general, the types of **systems-on-a-chip (SoC)** in Very-Large-Scale Integration (VLSI) are distinguished by their primary processor architecture and target application. The main categories include SoCs based on microcontrollers, microprocessors, application-specific integrated circuits (ASICs), and programmable logic. 

1. **Microcontroller-based SoCs**
   
     These SoCs integrate a less powerful processor core with on-chip memory and standard peripherals for simpler, resource-constrained tasks. 


      - **Target applications** : Embedded systems, IoT devices, home automation, and simple consumer electronics.

      - **Processor core** : Often an ARM Cortex-M or RISC-V core.

      - **Key components** : Microcontroller cores, limited memory (RAM, ROM), and basic I/O interfaces like SPI, I²C, and UART. 

2. **Microprocessor-based SoCs**

     Designed for more complex computing tasks, these SoCs feature more powerful processors and often include specialized accelerators for high-performance mobile and desktop computing. 

  
      - Target applications: Smartphones (e.g., Apple A-series, Qualcomm Snapdragon), tablets, and high-end consumer electronics.

      - Processor core: Typically a powerful CPU architecture, such as ARM Cortex-A.

      - Key components:

          --> CPU: One or more powerful processor cores.

          --> GPU: A graphics processing unit for accelerating visuals.

          --> DSP: Digital Signal Processors for audio, video, and signal processing.

          --> Connectivity: Wireless modules (Wi-Fi, Bluetooth, 4G/5G) are often integrated. 

4. **Application-Specific Integrated Circuit (ASIC) SoCs**

      **ASIC SoCs** are custom-built for one specific purpose and offer the highest performance and lowest power consumption for that function. 


      - **Target applications**: High-volume, single-purpose devices that justify the high non-recurring engineering (NRE) costs. Examples include specialized communication equipment and defense electronics.

      - **Processor core** : A custom instruction set or a simple core optimized for the application.

      - **Key features** : The components are precisely tailored to the application's needs, often with hardwired accelerators for the most critical tasks. 

5. **Programmable SoCs (or FPGA-based SoCs)**

      These SoCs combine one or more hard processor cores with a field-programmable gate array (FPGA) fabric, providing both the flexibility of programmable logic and the performance of a dedicated processor. 

      - **Target applications** : Prototyping new designs, applications with evolving standards, and high-performance, real-time control systems like those in industrial automation.

      - **Processor core** : A hard IP core like ARM, paired with the programmable FPGA logic.

      - **Key components** :
  
          --> Hard Processor System: Includes the CPU, memory controller, and other peripherals.
  
          --> FPGA Fabric: The programmable logic gates that can be configured to create custom hardware circuits.
  
          --> High-Speed Interconnect: Enables fast communication between the processor and the programmable logic.


**SoC Design Flow**

<div align="center"> <img width="481" height="500" alt="Screenshot 2025-09-25 181723" src="https://github.com/user-attachments/assets/999a041f-7555-4360-994e-ff4b372a1d66" /> </div>

## Introduction to BabySoC

  VSDBabySoC is a small yet powerful RISCV-based SoC. The main purpose of designing such a small SoC is to test three open-source IP cores together for the first time and calibrate the analog part of it. VSDBabySoC contains one RVMYTH microprocessor, an 8x-PLL to generate a stable clock, and a 10-bit DAC to communicate with other analog devices.

**components of BabySoC**

1. **RVMYTH**

    RVMYTH core is a simple RISCV-based CPU, introduced in a workshop by RedwoodEDA and VSD. And it is the brain of BabySoC, based on the open-source RISC-V design. It's a simple, customizable CPU that handles processing tasks and communicates with other parts of the SoC. This flexibility makes RVMYTH ideal for learning and experimenting with CPU architecture.

2. **PLL**

    A phase-locked loop or PLL is a control system that generates an output signal whose phase is related to the phase of an input signal. PLLs are widely used for synchronization purposes, including 
clock generation and distribution.

   A PLL typically consists of three main components:

     - **Error Detector:** Compares the input signal (reference) with the output signal from the oscillator and generates an error signal based on the phase difference.

     - **Loop Filter:** Usually a low-pass filter that processes the error signal to produce a control voltage.
     
     - **Voltage-Controlled Oscillator (VCO):** Adjusts its frequency based on the control voltage to match the input frequency.
  
  <div align="center">  <img width="938" height="370" alt="image" src="https://github.com/user-attachments/assets/ad1e731f-bb60-4f4b-87b2-76a40f451da0" /> </div>

**Functionality:**
   
- The PLL aims to lock the output frequency to the input frequency, maintaining a constant phase relationship between the two signals.
   
- In some cases, a frequency divider may be used in the feedback loop to produce an output that is a multiple of the reference frequency.

3. **DAC**

    A digital-to-analog converter or DAC is a system that converts a digital signal into an analog signal. DACs are widely used in modern communication systems enabling the 
generation of digitally-defined transmission signals. As a result, high-speed DACs are used for mobile communications and ultra-high-speed DACs are employed in optical 
communications systems.

* There are two types of DACs :

   i. Weighted Resistor DAC

   ii. R-2R Ladder DAC

   i. **Weighted Resistor DAC**

     * A weighted resistor DAC produces an analog output, which is almost equal to the digital (binary) input by using binary weighted resistors in the inverting adder circuit. In short, a binary weighted resistor DAC is called as weighted resistor DAC.

     * The circuit diagram of a 3-bit binary weighted resistor DAC is shown in the following figure :
       
    <div align="center"> <img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/edd20cba-77bc-4e76-8a9e-aee50f118d2c" /> </div> 

    ii. **R-2R Ladder DAC**
   

   * The R-2R Ladder DAC overcomes the disadvantages of a binary weighted resistor DAC. As the name suggests, R-2R Ladder DAC produces an analog output, which is almost equal to the digital (binary) input by using a R-2R ladder network in the inverting adder circuit.
  
   * The circuit diagramof a 3-bit R-2R Ladder DAC is shown in the following figure :

  <div align="center"> <img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/30fec60a-57de-4075-911f-f818d260c7c3"/></div>

   **Architecture** 

  * The basic idea is to divide the voltage into N different voltage values in the range of VREFH and VREFL- for an N-Bit DAC.   
  * The design used here to achieve this is the simple resistor string DAC which consists of resistors in series. 
  * These resistors are then connected to various switches in such a fashion that it routes the exact voltage to the output. 
  * The problem of the largeness of the circuit is reduced by building hierarchical subcircuits of 10-Bit potentiometric DAC – Switch, 2-bit, 3-bit, 4-bit, 5-bit, 6-bit, 7-bit, 8-bit, 9-bit and 10-bit.


<div align="center"> <img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/b798e385-4f11-4bdb-bc9f-8a5050bdf702" /> </div>

# Why BabySoC is a simplified model for learning SoC concepts?


- BabySoC is a minimal, educational version of a SoC used for training and research.

- Instead of implementing complex CPUs and high-speed interconnects, BabySoC includes:

  - A small RISC-V core or microcontroller-like CPU.

  - Limited memory (small SRAM/ROM).
 
  - A few simple peripherals (GPIO, UART).

  - A basic bus interconnect (e.g., APB or AXI-lite).

- Purpose:

  - Helps students and beginners understand SoC architecture without being overwhelmed by industry-scale designs.

  - Easier to simulate and synthesize.

  - Reduces design complexity while covering core SoC concepts.
 
  ## The role of functional modelling before RTL and physical design stages

  SoC design is a multi-stage process. Before going into RTL coding (Verilog/VHDL) and Physical Design (layout, place & route), functional modelling plays a critical role:

  1. High-Level Concept Validation
  
      - Models system behavior in C/C++/SystemC/Python.

      - Checks whether architecture meets performance, latency, and bandwidth goals.

  2. Early Verification

      - Simulates functionality without worrying about low-level details.

      - Detects design flaws before RTL implementation → reduces rework.

  3. Exploration of Trade-offs

      - Helps architects explore CPU vs accelerator, cache sizes, bus vs NoC interconnects.

      - Guides RTL engineers with validated design specs.

  4. Bridging to RTL

      - Functional models → Reference for RTL developers.

      - Defines expected behavior → helps verification team with testbench creation.

  5. Faster Prototyping

      - Can be mapped onto FPGA/emulators for early software development even before silicon exists.

**key point**

👉 Without functional modelling, RTL and physical design teams risk implementing a system that is functionally incorrect, suboptimal, or not aligned with product requirements.

# Summary

- SoCs enable compact, efficient, high-performance embedded systems.

- BabySoC is a great learning platform for understanding SoC architecture.

- Functional modeling ensures early validation and fewer design errors.

- Prepares the designer for RTL coding and physical design implementation.


