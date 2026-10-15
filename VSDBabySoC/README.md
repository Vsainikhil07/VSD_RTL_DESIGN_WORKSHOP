# RTL Design, Synthesis & Physical Design Workshop

A collection of hands-on experiments in Verilog RTL design, functional verification, logic synthesis, optimization, and open-source VLSI implementation.

This repository documents my practical learning activities in RTL design and digital hardware implementation. It includes Verilog programming, simulation, testbench development, waveform analysis, logic synthesis, synthesis optimization, and practical assignments. It also documents my ongoing exploration of open-source physical design tools and the SKY130 technology platform.

---

## Repository Overview

| Section | Description |
|---|---|
| **RTL Design Modules** | Verilog programming, digital logic implementation, simulation, synthesis, and optimization |
| **Physical Design Modules** | Practical exploration of the ASIC physical design flow using open-source EDA tools |
| **Assessment** | Practical assignments and Verilog-based design exercises, including a sequence detector |
| **Practical Sessions** | Hands-on exercises involving RTL simulation, synthesis, and verification |
| **VSDBabySoC** | Practical work associated with the VSDBabySoC project |
| **Tools and Technologies** | Open-source EDA tools used for RTL design, verification, synthesis, and physical design |

---

# RTL Design and Synthesis

This section documents practical exercises in digital logic design using Verilog HDL. The work focuses on developing RTL code, testing functionality, understanding synthesized logic, and exploring optimization techniques.

## Module 0 – Workshop Setup

**Topics Covered**
- Introduction to the workshop and its objectives
- Laboratory environment setup
- EDA tool installation and configuration
- Preparation of the development environment

## Module 1 – RTL Design Fundamentals

**Topics Covered**
- Fundamentals of digital design and RTL
- Introduction to Verilog HDL
- Combinational logic implementation
- Testbench development
- Functional simulation using Icarus Verilog
- Waveform analysis using GTKWave
- Introduction to logic synthesis

## Module 2 – RTL Design and Synthesis

**Topics Covered**
- Verilog module implementation
- Module connectivity and hierarchical design
- Multi-module RTL development
- Functional simulation and verification
- Logic synthesis using Yosys
- Gate-level netlist generation
- Technology mapping
- Analysis of synthesis results

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
- Comparison of RTL and synthesized netlist behavior
- Multiplexer implementation and testing
- Testbench-based verification
- Netlist inspection and debugging

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

# Assessment and Practical Assignments

This section contains practical assessment work and assignments involving digital logic design and hardware description using Verilog HDL.

The exercises provide opportunities to apply RTL coding concepts, develop functional hardware descriptions, and verify circuit behavior through simulation.

## Sequence Detector

The sequence detector assessment focuses on implementing a digital circuit that identifies a specified input sequence.

**Areas of Work**
- Understanding the required input sequence
- Developing the Verilog RTL implementation
- Handling sequential input data
- Testing the design using a testbench
- Simulating the circuit and observing its behavior
- Verifying the detection output for different input sequences

## Other Assignments

The repository also includes assignment-related material provided as part of the workshop.

These assignments support practical learning and help reinforce concepts related to Verilog programming, digital logic implementation, simulation, and verification.

---

# Physical Design

This section documents practical learning and experiments related to the ASIC physical design flow using open-source EDA tools and the SKY130 Process Design Kit (PDK).

**Topics Covered**
- Introduction to open-source EDA tools
- SKY130 PDK and standard-cell library concepts
- RTL-to-GDSII implementation flow
- Floorplanning and core utilization
- Standard-cell placement
- Power Distribution Network (PDN) concepts
- Timing analysis fundamentals
- Clock Tree Synthesis (CTS)
- Global and detailed routing
- Layout verification concepts

The physical design work builds on the fundamentals of digital logic and synthesis to explore how a logical design progresses toward a physical chip layout.

---

# Practical Sessions

The practical sessions provide additional hands-on experience with RTL design, simulation, synthesis, and verification.

## Session 1

This session documents the practical exercises completed as part of the first workshop session.

The work focuses on the tools, commands, and procedures introduced during the session.

## Session 2

This session documents additional practical exercises involving RTL simulation, synthesis, and design verification.

The work emphasizes understanding circuit functionality, examining simulation results, and comparing RTL behavior with synthesized implementations.

---

# VSDBabySoC

This section contains practical work associated with the VSDBabySoC project.

It documents the project-related activities and experiments carried out during the workshop. The section will be updated as further implementation and verification work is completed.

---

# Tools and Technologies

| Tool / Technology | Purpose |
|---|---|
| **Verilog HDL** | Hardware description and RTL design |
| **Icarus Verilog** | RTL simulation |
| **GTKWave** | Waveform visualization |
| **Yosys** | Logic synthesis and technology mapping |
| **SKY130 PDK** | Open-source process design kit |
| **OpenLANE** | ASIC implementation flow |
| **OpenROAD** | Physical design implementation |
| **OpenSTA** | Static Timing Analysis |
| **Magic** | Layout editing and design rule checking |
| **ngspice** | Circuit simulation |
| **Git** | Version control |
| **GitHub** | Repository hosting and documentation |
| **Linux Terminal** | Tool execution and workflow management |

---

# Learning Outcomes

Through these practical activities, I am developing an understanding of:

- Writing Verilog RTL for digital logic circuits.
- Developing testbenches and verifying circuit functionality.
- Running simulations and interpreting waveforms.
- Performing logic synthesis using Yosys.
- Understanding technology mapping and gate-level netlists.
- Exploring RTL optimization techniques.
- Debugging functional issues in digital designs.
- Learning the fundamentals of ASIC physical design.
- Exploring open-source EDA tools and the SKY130 platform.

---

# Author

**Vadla Sai Nikhil**

- **College:** Anurag University
- **Branch:** Electronics and Communication Engineering (ECE)

This repository documents my ongoing practical learning in RTL design, digital verification, synthesis, optimization, and open-source VLSI implementation.

---

*Maintained as part of my RTL Design and VLSI learning journey.*
