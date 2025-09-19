
# <img width="70" height="70" alt="image" src="https://github.com/user-attachments/assets/143fdf2a-2c67-4cea-9092-b6436290abff" />  RISC-V Tapeout Program

Welcome to VSD'S RISCV tapeout program.  Let's begin the adventure by exploring Day0 topic such as Tool Installation.

 # <img width="60" height="60" alt="image" src="https://github.com/user-attachments/assets/0fc94fea-b4ab-4ed1-aca0-eebd6ccb00f7" /> Task-1 

# <img width="60" height="60" alt="image" src="https://github.com/user-attachments/assets/4056685c-3bb4-4320-9b9b-65ac505e33a1" /> Summary of the video























# <img width="60" height="60" alt="image" src="https://github.com/user-attachments/assets/0fc94fea-b4ab-4ed1-aca0-eebd6ccb00f7" /> Task -2 

# <img width="60" height="60" alt="image" src="https://github.com/user-attachments/assets/5c0e1f14-e767-4697-990e-680e1e294748" /> Opensource Tools Installation

Before start with tool installation, lets describe about opensource tools, why we used? and  

# About Opensource tools:

  Open-source tools are software programs distributed with their source code, which is freely available for anyone to use, study, modify, and distribute under an open-source license.
 
# <img width="40" height="40" alt="image" src="https://github.com/user-attachments/assets/7ee2fb21-a025-4f68-803f-b396021be9d7" /> Why are we used?
  
  Open-source VLSI tools are used for the following reasons:
  
   1. High Cost of Proprietary EDA Tools:
         Commercial tools like Synopsys, Cadence, Mentor, Ansys are extremely expensive (licenses can cost crores of rupees per year). Open-source alternatives (like OpenROAD, Magic, KLayout, Verilator, Yosys) make learning and experimenting possible without financial barriers.
 
   2. Research & Innovation:
              Researchers can modify open-source tools, add features, and experiment with new algorithms (placement, routing, ML in VLSI, etc.). Faster innovation compared to closed tools where only the vendor controls updates.

 3. Education & Skill Development:
              Universities and students can use open-source flows to learn ASIC/SoC design without needing access to costly industry tools. Helps in building a stronger talent pipeline for semiconductor industry.
  
 4. Bridging Academia & Industry:
              Students trained on open-source tools can transition faster into industry by learning concepts. Industry also benefits when new algorithms are tested in open-source first, then adopted into commercial tools.

 5. Community Collaboration:
              Just like Linux or RISC-V, an ecosystem builds around open-source EDA tools. Shared development reduces duplication of effort and increases quality.

# <img width="60" height="60" alt="image" src="https://github.com/user-attachments/assets/5c0e1f14-e767-4697-990e-680e1e294748" /> Different Opensource Tools available on VLSI and their purpose:

## Front-End (RTL → Netlist):

1. Icarus Verilog:

   * Purpose: Verilog simulation
   * Inventor: Stephen Williams (1998, still maintained by community)

3. Verilator:

   * Purpose: Compiles Verilog/SystemVerilog into C++/SystemC for fast simulation
   * Inventor: Wilson Snyder (initial developer, still community-driven)

4. Yosys:

   * Purpose: Logic synthesis (RTL → Gate-level netlist)
   * Inventor: Clifford Wolf (2012, Germany)

## Back-End (Netlist → Layout → GDSII):

1. Magic VLSI:

   * Purpose: Layout editor, DRC, extraction
   * Inventor: John Ousterhout (1980s, UC Berkeley)

2. KLayout:

   * Purpose: Layout viewer/editor, GDS manipulation
   * Inventor: Matthias Köfferlein (2008)

3. Qflow:

   * Purpose: Complete RTL-to-GDS flow using Yosys + GrayWolf + Magic
   * Inventor: Tim Edwards (2010s, Efabless)

4. GrayWolf:

   * Purpose: Placement tool
   * Inventor: Originally by AT\&T Bell Labs (1980s), open-sourced later

5. OpenROAD:

   * Purpose: Full digital flow (synthesis → placement → routing → timing closure)
   * Inventor: DARPA-funded project (2018–present, led by Prof. Andrew Kahng, UCSD + collaborators)

## Analog / Mixed-Signal:

1. Ngspice:

   * Purpose: SPICE circuit simulator
   * Inventor: Derived from Berkeley SPICE (1973, Donald Pederson, UC Berkeley); open-source branch maintained by Holger Vogt and community.

2. Xyce:

   * Purpose: High-performance circuit simulator
   * Inventor: Sandia National Laboratories (2003, US DOE project)


# Integrated Flows: 

1. OpenLane:

   * Purpose: Automated ASIC design flow that integrates several open-source tools, such as Yosys, Magic, and OpenROAD, to complete the RTL-to-GDSII design process with predefined commands. 

  
## Supporting Tools:

1. OpenSTA:

   * Purpose: Static timing analysis
   * Inventor: OpenROAD community (Chris Zhu contributed major parts)

2. OpenPhySyn:

   * Purpose: Physical synthesis (timing/area optimization)
   * Inventor: Brown University (Dimitrios Kourkoulis et al.)

3. OpenRAM:

   * Purpose: SRAM compiler
   * Inventor: Prof. Matthew Guthaus, UC Santa Cruz

4. Fault:

   * Purpose: DFT (scan-chain insertion, ATPG)
   * Inventor: University of Southern California (Dr. Shahin Nazarian’s group)
  
# <img width="40" height="40" alt="image" src="https://github.com/user-attachments/assets/135c9367-1501-4123-ab31-3554a9e5408e" /> Tools Installation and Checks:





# Yosys 

Steps to install yosys

$ sudo apt-get update 

$ git clone https://github.com/YosysHQ/yosys.git

$ sudo apt install git # if git is not installed

$ cd yosys 

$ sudo apt install make (If make is not installed please install it)  

$ sudo apt-get install build-essential clang bison flex \ 
libreadline-dev gawk tcl-dev libffi-dev git \ 
graphviz xdot pkg-config python3 libboost-system-dev \ 
libboost-python-dev libboost-filesystem-dev zlib1g-dev 

$ make config-gcc 

#Yosys build depends on a Git submodule called abc, which hasn't been initialized yet. You need to run the following command before running make

$ git submodule update --init --recursive

$ make  

$ sudo make install 


# Installation check

<img width="870" height="276" alt="VirtualBox_Opensource_eda_ubuntu_19_09_2025_16_27_21" src="https://github.com/user-attachments/assets/da838c4d-3fce-4be9-bdf1-30948241dd4a" />










# Iverilog 

Steps to install iverilog 

sudo apt-get update 

sudo apt-get install iverilog 

# Installation Check

<img width="959" height="309" alt="VirtualBox_Opensource_eda_ubuntu_19_09_2025_16_27_56" src="https://github.com/user-attachments/assets/1eabe5d6-a24e-4462-957f-49ea910475cd" />





# gtkwave 

Steps to install gtkwave 

sudo apt-get update 

sudo apt install gtkwave 

# Installation Check

<img width="1198" height="663" alt="VirtualBox_Opensource_eda_ubuntu_19_09_2025_16_29_34" src="https://github.com/user-attachments/assets/6b6b35ba-3143-4082-b67f-f35660d464f9" />














# ngspice 
After downloading the tarball from https://sourceforge.net/projects/ngspice/files/ to a local 
directory, unpack it using: 
$ tar -zxvf /home/bhuvanesh/Downloads/ngspice-45.2.tar.gz 
$ cd ngspice-45.2
$ mkdir release 
$ cd release 
$ ../configure  --with-x --with-readline=yes --disable-debug 
#if after run the make then throws an errors like not found automake and autoconf then you need to run below commands
$ sudo apt update
$ sudo apt install build-essential autoconf automake
$ make 
$ sudo make install


# Installation Check

<img width="1280" height="800" alt="VirtualBox_Opensource_eda_ubuntu_19_09_2025_16_43_37" src="https://github.com/user-attachments/assets/b36469a0-8f7f-40a4-91b5-0bb033b161ba" />
















  
