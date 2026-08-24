# Module 4 – Gate-Level Simulation and Blocking vs Non-Blocking Assignments

## Introduction

Gate-Level Simulation (GLS) is used to verify the functionality of a synthesized gate-level netlist and compare its behavior with the original RTL design. Blocking (`=`) and non-blocking (`<=`) assignments are important Verilog constructs that determine how statements and signal updates are handled during simulation.

This module focuses on understanding the behavior of blocking and non-blocking assignments through simulation and waveform analysis. It also covers the GLS flow, where the synthesized netlist is verified using the SKY130 standard-cell library.

## Objectives

- To understand the difference between blocking (`=`) and non-blocking (`<=`) assignments in Verilog HDL.
- To study how blocking and non-blocking assignments behave during RTL simulation.
- To understand the importance of choosing the appropriate assignment type for combinational and sequential logic.
- To understand the concept of Gate-Level Simulation (GLS) and its purpose in digital design verification.
- To perform GLS using a synthesized netlist and standard-cell library.
- To compare RTL simulation results with Gate-Level Simulation results.
- To analyze waveforms using GTKWave and observe differences in signal behavior.

  ## Table of Contents

| No. | Topic                                           |
| --: | ----------------------------------------------- |
|   1 | Introduction to Gate-Level Simulation (GLS)     |
|   2 | Blocking vs Non-Blocking Assignments            |
|   3 | Blocking and Non-Blocking Assignment Analysis   |
|   4 | RTL Simulation and Waveform Analysis            |
|   5 | Synthesis and Netlist Generation                |
|   6 | Gate-Level Simulation Using Synthesized Netlist |
|   7 | RTL vs GLS Waveform Comparison                  |
|   8 | Analysis of Simulation Results                  |
|   9 | Conclusion                                      |

# 1. Introduction to Gate-Level Simulation (GLS)

## Overview

Gate-Level Simulation (GLS) is a verification step performed after synthesis to check whether the synthesized gate-level netlist behaves as expected. Unlike RTL simulation, GLS uses the actual synthesized logic and standard-cell library models.

GLS helps verify that the functionality of the design is preserved after synthesis and provides confidence that the synthesized implementation matches the intended RTL behavior.

<img width="1357" height="742" alt="what is gls" src="https://github.com/user-attachments/assets/54ecddd1-9bb0-43bb-8e69-d3b6515ff368" />

<img width="1362" height="736" alt="image" src="https://github.com/user-attachments/assets/8f359545-bc86-4d29-9a9a-3189250cd014" />


## GLS Flow

```text
RTL Design
    ↓
RTL Simulation
    ↓
Synthesis
    ↓
Gate-Level Netlist
    ↓
Gate-Level Simulation
    ↓
Waveform Analysis
```

## Purpose of GLS

* To verify the synthesized gate-level netlist.
* To check whether the intended functionality is preserved after synthesis.
* To identify differences between RTL and synthesized behavior.
* To analyze the circuit using standard-cell library models.
* To verify the design before moving to further implementation stages.

  # 2. Blocking vs Non-Blocking Assignments

## Overview

Blocking (`=`) and non-blocking (`<=`) are the two main procedural assignment types in Verilog HDL. Their execution behavior determines when assigned values become available during simulation.

Blocking assignments update the assigned value immediately, while non-blocking assignments schedule the update for later in the current simulation time step.

Understanding this difference is important for writing correct and synthesizable RTL code.
<img width="1327" height="706" alt="blocking and non blocking" src="https://github.com/user-attachments/assets/4aa33099-0430-457c-a8e2-994bbffc32d8" />
## Blocking vs Non-Blocking Assignments

| Feature             | Blocking (`=`)                             | Non-Blocking (`<=`)                     |
| ------------------- | ------------------------------------------ | --------------------------------------- |
| Execution           | Executes immediately                       | Update is scheduled                     |
| Statement Order     | Executes sequentially                      | Statements are evaluated together       |
| Value Update        | Updated immediately                        | Updated after the current evaluation    |
| Common Usage        | Combinational logic                        | Sequential logic                        |
| Typical Block       | `always_comb`                              | `always_ff` / clocked `always`          |
| Simulation Behavior | Later statements can use the updated value | Later statements see the previous value |
| Main Purpose        | Describes immediate procedural behavior    | Models clocked and sequential behavior  |

# 3. Blocking and Non-Blocking Assignment Analysis

## Overview

Blocking (`=`) and non-blocking (`<=`) assignments behave differently during Verilog simulation. Blocking assignments update values immediately and execute statements sequentially, while non-blocking assignments schedule value updates for later in the current simulation time step.

The difference can affect the values observed by subsequent statements and can lead to different simulation behavior. Understanding these differences is important when deciding which assignment type to use for combinational and sequential logic.

## Blocking Assignment

Blocking assignments use the `=` operator and update the assigned value immediately.

```verilog
variable = expression;
```

They are generally used for describing combinational logic.

## Non-Blocking Assignment

Non-blocking assignments use the `<=` operator and schedule the value update.

```verilog
variable <= expression;
```

They are generally used for describing sequential logic, particularly in clock-driven blocks.

## Key Observation

The behavior of both assignment types can be verified through RTL simulation and waveform analysis. Comparing their signal transitions helps demonstrate how assignment type affects simulation results.
## 4. RTL Simulation and Waveform Analysis

RTL simulation is used to verify the functionality of the Verilog design before synthesis. The RTL code is simulated using a testbench, and the output waveforms are observed using a waveform viewer such as GTKWave.
 ### RTL SIMULATION OF A 2:1 MULTIPLEXER:
 A 2×1 multiplexer was implemented using the Verilog ternary operator to understand combinational logic design. The RTL design was simulated using Icarus Verilog, and the output waveform was verified using GTKWave. The experiment demonstrates how the output changes based on the select signal and validates the functional correctness of the multiplexer before synthesis.
 ### SIMULATION COMMANDS:
* iverilog -o mux ternary_operator_mux.v tb_ternary_operator_mux.v
*gtkwave ternary_operator_mux.vcd
### OUTPUT WAVEFORM:
<img width="1920" height="1080" alt="ternary_mux_waveform" src="https://github.com/user-attachments/assets/f068737d-b918-4e3d-8261-a3227c6cb910" />

## OBSERVATION:
The waveform confirmed correct multiplexer operation for all input combinations.
## Analysis of an Incorrect Multiplexer Design

### Overview

This experiment analyzes an incorrectly coded multiplexer and its effect on RTL behavior. The incomplete assignment inside the `always` block prevents the output from being updated for every possible input condition. During synthesis, this can lead to latch inference and may cause the synthesized circuit to behave differently from the intended combinational multiplexer.

### Simulation Commands
* iverilog -o bad_mux bad_mux.v tb_bad_mux.v
* gtkwave bad_mux.vcd
### OUTPUT WAVEFORM:
<img width="973" height="642" alt="bad_mux waveform" src="https://github.com/user-attachments/assets/b18b6202-f55d-4430-8a94-03a5f3702fe1" />
### FUNCTIONAL VERIFICATION:
<img width="1918" height="707" alt="ternary_mux_netlist" src="https://github.com/user-attachments/assets/64f5d7dc-bd45-4e16-9b67-8a5619f92d82" />
````markdown
## Synthesis of Blocking Assignment Circuit

### Overview

The blocking assignment circuit was synthesized using Yosys to examine how the RTL description is converted into hardware. The synthesized design shows how the Verilog logic is interpreted and mapped to cells from the SKY130 standard-cell library.

### Synthesis Commands

```text
yosys

read_verilog blocking_caveat.v

synth -top blocking_caveat

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show
```

**Image**

### Technology-Mapped Circuit

![Uploading Screenshot 2026-08-24 001451.png…]()

[blocking cavaet ]
<img width="955" height="457" alt="blocking caveat waveform" src="https://github.com/user-attachments/assets/00d7039a-c1f1-496b-a66c-ece89a21afe2" />


### Observation

The synthesized circuit represents the blocking-assignment logic after technology mapping and shows how the RTL design is implemented using SKY130 standard cells.
````

