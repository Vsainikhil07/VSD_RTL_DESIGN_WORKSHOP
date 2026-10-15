
# RTL Design, Synthesis & Physical Design Workshop

**A collection of hands-on experiments in Verilog RTL design, functional verification, logic synthesis, optimization, timing analysis, and open-source ASIC implementation.**

This repository contains my practical learning activities in RTL design and VLSI implementation. The work covers Verilog programming, testbench development, waveform analysis, logic synthesis using Yosys, gate-level simulation, RISC-V experiments, and physical design using open-source EDA tools with the SKY130 PDK.

---

## Quick Navigation

### RTL Design Modules
- [Module 0](./Module_0/README.md)
- [Module 1](./Module_1/README.md)
- [Module 2](./Module_2/README.md)
- [Module 3](./Module_3/README.md)
- [Module 4](./Module_4/README.md)
- [Module 5](./Module_5/README.md)

### Physical Design Modules
- [Physical Design – Module 1](./PHYSICAL_DESIGN/Module_1/)
- [Physical Design – Module 2](./PHYSICAL_DESIGN/Module_2/)
- [Physical Design – Module 3](./PHYSICAL_DESIGN/Module_3/)
- [Physical Design – Module 4](./PHYSICAL_DESIGN/Module_4/)
- [Physical Design – Module 5](./PHYSICAL_DESIGN/Module_5/)

### Other Sections
- [Assessment](./Assessment/)
- [Session 1](./Session%20-1/)
- [Session 2](./Session-2/)
- [VSDBabySoC](./VSDBabySoC/README.md)
---

## Repository Overview

| Section | Description |
|---|---|
| **RTL Design and Synthesis** | Verilog coding, simulation, synthesis, and optimization |
| **Physical Design Modules** | Floorplanning, placement, clock tree synthesis, routing, and timing analysis |
| **Mid-Term Practical Work** | RISC-V development, RTL verification, and OpenROAD experiments |
| **My Assignments** | Assignment PDFs provided as part of the workshop |
| **Tools and Technologies** | Open-source EDA tools and the SKY130 technology platform |

---

# RTL Design and Synthesis

This section documents practical exercises involving digital logic design, RTL development, simulation, and synthesis.

| Module | Focus Area | Documentation |
|---|---|---|
| Module 0 | Workshop Setup and Introduction | [View README](./Module_0/README.md) |
| Module 1 | Verilog RTL Design | [View README](./Module_1/README.md) |
| Module 2 | RTL Design and Synthesis | [View README](./Module_2/README.md) |
| Module 3 | RTL Optimization | [View README](./Module_3/README.md) |
| Module 4 | Simulation and Synthesis Verification | [View README](./Module_4/README.md) |
| Module 5 | Synthesis Optimization | [View README](./Module_5/README.md) |

## Module 0 – Workshop Setup

**Topics Covered**

- Workshop introduction and overview
- Cloud-based laboratory environment
- Local installation of EDA tools
- Development environment configuration

**Documentation:** [Module 0 README](./Module_0/README.md)

---

## Module 1 – RTL Design Fundamentals

**Topics Covered**

- Digital design and RTL fundamentals
- Verilog HDL programming
- Combinational circuit development
- Testbench creation
- Simulation using Icarus Verilog
- Waveform inspection with GTKWave
- Introduction to logic synthesis

**Documentation:** [Module 1 README](./Module_1/README.md)

---

## Module 2 – RTL Design and Synthesis

**Topics Covered**

- Verilog module implementation
- Hierarchical design and module connectivity
- Multi-module RTL development
- Functional simulation and verification
- Logic synthesis with Yosys
- Gate-level netlist generation
- Technology mapping
- Synthesis output analysis

**Documentation:** [Module 2 README](./Module_2/README.md)

---

## Module 3 – RTL Optimization

**Topics Covered**

- Combinational and sequential logic optimization
- Constant propagation
- State optimization
- Logic sharing and cloning
- Retiming fundamentals
- Synthesis optimization experiments

**Documentation:** [Module 3 README](./Module_3/README.md)

---

## Module 4 – Simulation and Synthesis Verification

**Topics Covered**

- RTL functional verification
- Gate-Level Simulation (GLS)
- Blocking and non-blocking assignments
- RTL and synthesized circuit behavior
- Multiplexer implementation and testing
- Testbench-based verification
- Netlist inspection
- Debugging common RTL issues

**Documentation:** [Module 4 README](./Module_4/README.md)

---

## Module 5 – Synthesis Optimization

**Topics Covered**

- If-else and case statements
- Nested conditional expressions
- Latch inference
- Loop-based RTL coding
- Generate constructs
- Multiplexer and demultiplexer design
- Ripple Carry Adder implementation
- Synthesis analysis and optimization

**Documentation:** [Module 5 README](./Module_5/README.md)

---

# Physical Design Modules

This section covers the ASIC physical design flow, including floorplanning, standard-cell placement, timing analysis, clock tree synthesis, routing, and design verification.

| Module | Focus Area | Documentation |
|---|---|---|
| Module 1 – PD | Open-Source EDA and SKY130 Introduction | [View README](./module-1-pd/README.md) |
| Module 2 – PD | Floorplanning and Standard Cells | [View README](./module-2-pd/README.md) |
| Module 3 – PD | Layout Design and Characterization | [View README](./module-3-pd/README.md) |
| Module 4 – PD | Timing Analysis and Clock Tree Synthesis | [View README](./module-4-pd/README.md) |
| Module 5 – PD | Routing and Timing Closure | [View README](./module-5-pd/README.md) |

---

## Module 1 – Open-Source EDA and SKY130 Introduction

**Folder:** `module-1-pd`

**Topics Covered**

- Introduction to open-source EDA tools
- Overview of the SKY130 Process Design Kit
- RISC-V architecture fundamentals
- System-on-Chip design concepts
- OpenLANE design flow
- RTL-to-GDSII implementation overview
- Design preparation and synthesis
- Analysis of synthesis results

**Documentation:** [Module 1 – Physical Design](./module-1-pd/README.md)

---

## Module 2 – Floorplanning and Standard-Cell Design

**Folder:** `module-2-pd`

**Topics Covered**

- Chip floorplanning
- Core utilization and aspect ratio
- Pre-placed cells and placement blockages
- Power distribution planning
- Pin placement
- Standard-cell library concepts
- Initial and optimized placement
- Placement congestion analysis
- Cell characterization fundamentals

**Documentation:** [Module 2 – Physical Design](./module-2-pd/README.md)

---

## Module 3 – Layout Design and Characterization

**Folder:** `module-3-pd`

**Topics Covered**

- CMOS inverter design
- SPICE netlist and simulation setup
- Circuit simulation using ngspice
- CMOS fabrication fundamentals
- Standard-cell layout development
- Layout extraction
- SKY130 technology files
- Layout editing with Magic
- Design Rule Checking (DRC)
- Standard-cell characterization

**Documentation:** [Module 3 – Physical Design](./module-3-pd/README.md)

---

## Module 4 – Timing Analysis and Clock Tree Synthesis

**Folder:** `module-4-pd`

**Topics Covered**

- Standard-cell timing libraries
- Delay models and timing tables
- Setup and hold timing
- Clock uncertainty and jitter
- Static Timing Analysis using OpenSTA
- Timing optimization
- Clock Tree Synthesis using TritonCTS
- Ideal-clock and propagated-clock analysis
- Clock buffering and timing verification

**Documentation:** [Module 4 – Physical Design](./module-4-pd/README.md)

---

## Module 5 – Routing and Timing Closure

**Folder:** `module-5-pd`

**Topics Covered**

- Maze-routing algorithms
- Design Rule Checking
- Power Distribution Network construction
- Global and detailed routing
- TritonRoute
- Routing connectivity
- Route topology analysis
- Post-route verification
- Final stages of the RTL-to-GDSII flow

**Documentation:** [Module 5 – Physical Design](./module-5-pd/README.md)

---

# Mid-Term Practical Work

The mid-term practical sessions extend the workshop activities to processor development, RTL verification, synthesis, and physical design.

## Session 1 – RISC-V and OpenROAD

**Topics Covered**

- RISC-V development environment
- GitHub Codespaces
- C program compilation
- Assembly inspection using `objdump`
- Program execution using Spike
- RTL simulation
- Waveform analysis with GTKWave
- Introduction to OpenROAD
- RTL-to-GDSII workflow exploration

---

## Session 2 – RTL Simulation and Synthesis

**Topics Covered**

- Yosys synthesis workflow
- Good and bad multiplexer implementations
- Verilog testbench development
- Functional simulation
- Synthesized netlist inspection
- Gate-Level Simulation
- Comparison of RTL and gate-level behavior

---

## Session 3 – Gate-Level Verification

**Topics Covered**

- RTL simulation
- Logic synthesis
- Gate-level netlist generation
- Post-synthesis simulation
- Waveform inspection
- Functional verification

---

# My Assignments

This section provides direct access to the two assignment PDFs given as part of my coursework. Upload both PDF files into the `Assignments` folder in this repository.

| Assignment | PDF Document |
|---|---|
| Assignment 1 | [Open Assignment 1 PDF](./Assignments/Assignment-1.pdf) |
| Assignment 2 | [Open Assignment 2 PDF](./Assignments/Assignment-2.pdf) |

**How to access:** Click the relevant assignment link to view the PDF on GitHub.

---

# Tools and Technologies

| Tool / Technology | Purpose |
|---|---|
| **Verilog HDL** | RTL design and hardware description |
| **Icarus Verilog** | RTL and gate-level simulation |
| **GTKWave** | Waveform visualization |
| **Yosys** | Logic synthesis and technology mapping |
| **SKY130 PDK** | Open-source process design kit |
| **OpenLANE** | ASIC implementation workflow |
| **OpenROAD** | Physical design and routing |
| **RePlAce** | Placement optimization |
| **TritonCTS** | Clock tree synthesis |
| **TritonRoute** | Detailed routing |
| **OpenSTA** | Static timing analysis |
| **Magic** | Layout editing and design rule checking |
| **ngspice** | Circuit simulation |
| **Git** | Version control |
| **GitHub** | Repository hosting and documentation |
| **Linux Terminal** | EDA tool execution and automation |

---

# Learning Outcomes

Through these practical activities, I have explored:

- Developing and verifying Verilog RTL designs.
- Creating testbenches and debugging simulation results.
- Understanding logic synthesis and technology mapping.
- Inspecting gate-level netlists and simulation behavior.
- Working with open-source ASIC design tools.
- Learning floorplanning, placement, and routing concepts.
- Studying timing analysis and clock tree synthesis.
- Understanding the overall RTL-to-GDSII implementation process.

---

# Author

**Vadla Sai Nikhil**

- **College:** Anurag University
- **Branch:** Electronics and Communication Engineering (ECE)

This repository documents my practical work and learning in RTL design, digital verification, synthesis optimization, timing analysis, and open-source VLSI design.

---

*Maintained as part of my RTL Design and VLSI learning journey.*
