# RTL Design, Synthesis & Physical Design Workshop

**A collection of hands-on experiments in Verilog RTL design, functional verification, logic synthesis, optimization, timing analysis, and open-source ASIC implementation.**

This repository documents my practical learning activities in RTL design and VLSI implementation. It covers Verilog programming, testbench development, waveform analysis, logic synthesis using Yosys, gate-level simulation, RISC-V experiments, and physical design using open-source EDA tools with the SKY130 PDK.

---

## Repository Overview

| Section | Description |
|---|---|
| **RTL Design and Synthesis** | Verilog coding, functional simulation, logic synthesis, and RTL optimization |
| **Physical Design** | Floorplanning, standard-cell placement, clock tree synthesis, routing, and timing analysis |
| **Assessment** | Practical assessment tasks and related design implementations |
| **Practical Sessions** | Hands-on exercises involving RTL verification, synthesis, and physical design |
| **VSDBabySoC** | Practical exploration of a RISC-V-based System-on-Chip |
| **Tools and Technologies** | Open-source EDA tools and the SKY130 technology platform |

---

# RTL Design and Synthesis

This section documents practical exercises involving digital logic design, RTL development, simulation, verification, and synthesis.

## Module 0 – Workshop Setup

**Topics Covered**
- Workshop introduction and overview
- Laboratory environment setup
- Installation and configuration of EDA tools
- Development environment preparation

## Module 1 – RTL Design Fundamentals

**Topics Covered**
- Digital design and RTL fundamentals
- Verilog HDL programming
- Combinational circuit development
- Testbench creation
- Simulation using Icarus Verilog
- Waveform inspection using GTKWave
- Introduction to logic synthesis

## Module 2 – RTL Design and Synthesis

**Topics Covered**
- Verilog module implementation
- Hierarchical design and module connectivity
- Multi-module RTL development
- Functional simulation and verification
- Logic synthesis using Yosys
- Gate-level netlist generation
- Technology mapping
- Synthesis output analysis

## Module 3 – RTL Optimization

**Topics Covered**
- Combinational and sequential logic optimization
- Constant propagation
- State optimization
- Logic sharing and cloning
- Introduction to retiming
- Synthesis optimization experiments

## Module 4 – Simulation and Synthesis Verification

**Topics Covered**
- RTL functional verification
- Gate-Level Simulation (GLS)
- Blocking and non-blocking assignments
- Comparison of RTL and synthesized circuit behavior
- Multiplexer implementation and testing
- Testbench-based verification
- Netlist inspection
- Debugging common RTL issues

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

---

# Physical Design

Physical design converts a synthesized logical circuit into a physical layout that can be manufactured as an ASIC.

**Topics Covered**
- Introduction to open-source EDA tools
- SKY130 Process Design Kit (PDK)
- RTL-to-GDSII implementation flow
- Chip floorplanning and core utilization
- Power Distribution Network (PDN) construction
- Standard-cell placement and optimization
- Standard-cell layout and characterization
- Static Timing Analysis (STA)
- Setup and hold timing analysis
- Clock Tree Synthesis (CTS)
- Global and detailed routing
- Design Rule Checking (DRC)
- Post-route timing analysis and verification

---

# Practical Sessions

The practical sessions provide additional experience in RTL development, processor-related experiments, simulation, synthesis, and physical design.

## Session 1 – RISC-V and OpenROAD

**Topics Covered**
- Introduction to RISC-V architecture
- Development environment setup
- C program compilation
- Assembly inspection using `objdump`
- Program execution using Spike
- RTL simulation and waveform analysis
- Introduction to OpenROAD
- RTL-to-GDSII workflow exploration

## Session 2 – RTL Simulation and Synthesis

**Topics Covered**
- Yosys synthesis workflow
- Multiplexer implementation and optimization
- Verilog testbench development
- Functional simulation
- Synthesized netlist inspection
- Gate-Level Simulation
- Comparison of RTL and gate-level behavior

---

# Assessment

The assessment section documents practical tasks designed to evaluate RTL design, Verilog programming, digital logic implementation, and functional verification skills.

**Areas of Work**
- Verilog module development
- Sequential and combinational logic design
- Testbench creation and functional verification
- Simulation and waveform analysis
- Debugging and result analysis

---

# VSDBabySoC Project

VSDBabySoC provides practical exposure to the design and implementation of a RISC-V-based System-on-Chip.

**Areas of Exploration**
- RISC-V processor architecture
- System-on-Chip design concepts
- RTL integration and module connectivity
- Design configuration and synthesis
- Physical design implementation
- Timing analysis and verification

---

# Tools and Technologies

| Tool / Technology | Purpose |
|---|---|
| **Verilog HDL** | Hardware description and RTL design |
| **Icarus Verilog** | RTL and gate-level simulation |
| **GTKWave** | Waveform visualization |
| **Yosys** | Logic synthesis and technology mapping |
| **SKY130 PDK** | Open-source process design kit |
| **OpenLANE** | ASIC implementation workflow |
| **OpenROAD** | Physical design and routing |
| **OpenSTA** | Static Timing Analysis |
| **TritonCTS** | Clock tree synthesis |
| **TritonRoute** | Detailed routing |
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
- Learning RTL optimization techniques.
- Working with open-source ASIC design tools.
- Understanding floorplanning, placement, and routing.
- Studying timing analysis and clock tree synthesis.
- Exploring RISC-V and System-on-Chip design concepts.
- Understanding the overall RTL-to-GDSII implementation process.

---

# Author

**Vadla Sai Nikhil**

- **College:** Anurag University
- **Branch:** Electronics and Communication Engineering (ECE)

This repository documents my practical work and learning in RTL design, digital verification, synthesis optimization, timing analysis, and open-source VLSI design.

---

*Maintained as part of my RTL Design and VLSI learning journey.*
