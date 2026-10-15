
# Verilog RTL Design and Synthesis – Module 1

## 1. Introduction

This module introduced the fundamentals of RTL design and synthesis using Verilog HDL. The practical exercises focused on describing digital logic, validating functionality through simulation, and understanding how synthesis tools translate RTL code into a hardware-oriented representation.

The session used Icarus Verilog for simulation, GTKWave for waveform analysis, and Yosys for synthesis.

## 2. Topics Covered

- Fundamentals of RTL verification
- Verilog simulation environment
- Implementation of a 2:1 multiplexer
- Testbench development and waveform analysis
- RTL synthesis and technology mapping
- Gate-level schematic and netlist generation

## 3. RTL Verification Fundamentals

### Why Simulation Is Important

Simulation allows a digital design to be tested before hardware implementation. Different input combinations can be applied to check whether the output follows the intended logic.

### RTL Description

Register Transfer Level (RTL) describes the operation and data flow of a digital circuit. A 2:1 multiplexer was used in this exercise to understand how a selection signal determines the output.

![RTL design example](https://github.com/user-attachments/assets/640577fe-2f83-4fc3-8c38-114d4bd1053a?raw=true)

### Testbench Overview

A testbench supplies input patterns to the design and monitors its output. This helps identify functional errors before proceeding to synthesis.

![Testbench verification](https://github.com/user-attachments/assets/61079fc7-1b57-457c-920d-05e7e3151487?raw=true)

## 4. Simulation Using Icarus Verilog

### Simulation Workflow

Icarus Verilog compiles the design and its testbench into an executable simulation. The simulation can produce a VCD file containing signal changes over time, which can then be inspected in GTKWave.

**Workflow:**

RTL source + Testbench → Compilation → Simulation → VCD File → GTKWave

![Simulation environment](https://github.com/user-attachments/assets/333118ab-fc2f-4d01-96a6-ff64961cbde4?raw=true)

### Installing the Required Tools

```bash
sudo apt install iverilog
sudo apt install gtkwave
```

### Compiling the Design

```bash
iverilog good_mux.v tb_good_mux.v
```

### Running the Simulation

```bash
./a.out
```

### Opening the Waveform

```bash
gtkwave tb_good_mux.vcd
```

The waveform viewer makes it possible to compare the select signal, input signals, and output transitions during simulation.

![Multiplexer waveform](https://github.com/user-attachments/assets/03651cd1-1904-48b8-a02a-44e26db28a46?raw=true)

## 5. Analysis of the 2:1 Multiplexer

A 2:1 multiplexer routes one of two input signals to a single output based on the select input.

### Signal Details

- `i0`: First data input
- `i1`: Second data input
- `sel`: Input selection control
- `y`: Output signal

| Select (`sel`) | Output (`y`) |
|---|---|
| 0 | `i0` |
| 1 | `i1` |

The simulation was used to check whether the output followed the selected input for the applied test cases.

![Multiplexer logic](https://github.com/user-attachments/assets/5cae1f32-eb3e-4785-a819-2f5118d59581?raw=true)

## 6. RTL Synthesis with Yosys

### Purpose of Synthesis

Synthesis converts a Verilog RTL description into a logic representation suitable for hardware implementation. Technology mapping then associates the logic with cells available in a target standard-cell library.

Yosys was used for synthesis, with the SKY130 standard-cell library as the target technology.

**Synthesis flow:**

Verilog RTL → Logic Synthesis → Technology Mapping → Gate-Level Representation

![Synthesis process](https://github.com/user-attachments/assets/23e568ef-4a54-4c14-85b4-dbc5c40f7a8a?raw=true)

### Starting Yosys

```bash
yosys
```

### Loading the Standard-Cell Library

Use the actual Liberty file path available in your environment:

```tcl
read_liberty -lib my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

### Reading the RTL Source

```tcl
read_verilog good_mux.v
```

### Synthesizing the Multiplexer

```tcl
synth -top good_mux
```

### Mapping the Logic to SKY130 Cells

```tcl
abc -liberty my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

### Viewing the Schematic

```tcl
show
```

The schematic provides a visual view of the logic structure produced by the synthesis process.

![Synthesized schematic](https://github.com/user-attachments/assets/191c5b86-3d3c-44ee-962d-6cced8693fbe?raw=true)

### Exporting the Netlist

```tcl
write_verilog -noattr good_mux_netlist.v
```

The generated Verilog netlist represents the synthesized circuit. The exact contents depend on the synthesis commands and whether technology mapping has been completed successfully.

## 7. Summary of the Workflow

The practical session covered the following sequence:

**RTL Design → Testbench Creation → Simulation → Waveform Inspection → Synthesis → Technology Mapping → Schematic Inspection → Netlist Export**

This exercise connected functional verification with the initial stages of digital hardware implementation.

## 8. Learning Outcomes

After completing this module, I gained practical familiarity with:

- Writing and understanding basic Verilog RTL
- Using testbenches to validate digital logic
- Compiling and simulating Verilog designs
- Inspecting signal transitions in GTKWave
- Understanding synthesis and technology mapping
- Exploring the SKY130 standard-cell library
- Viewing synthesized logic as a schematic
- Exporting a synthesized Verilog netlist

## Author

**Vadla Sai Nikhil**

B.Tech – Electronics and Communication Engineering (ECE)  
Anurag University

**RTL Design Workshop Repository**

