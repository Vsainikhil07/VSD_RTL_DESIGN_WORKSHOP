# Practical Study of RISC-V, RTL Verification, and OpenROAD Physical Design

**Author:** Vadla Sai Nikhil

## Introduction

This document presents my practical exploration of open-source tools for RISC-V software development, RTL verification, waveform analysis, and digital physical design.

The activities covered include:

- RISC-V development using GitHub Codespaces
- Cross-compilation of C programs for RISC-V
- Machine-code analysis using `objdump`
- Program execution using Spike
- RTL simulation using Icarus Verilog
- Waveform inspection using GTKWave
- OpenROAD RTL-to-GDS flow exploration
- Makefile-based design configuration
- GDS layout visualization using KLayout

---

# Section 1: RISC-V Development Environment

## Step 1: Explore the RISC-V Repository

I accessed the RISC-V project repository on GitHub to review its source files, sample programs, and development environment.

![RISC-V GitHub repository](https://github.com/user-attachments/assets/9c2ac25c-8e4b-4fc7-a92e-795bb8113d5e?raw=true)

## Step 2: Create a GitHub Codespace

A Codespace was launched from the repository to provide a cloud-based VS Code environment with terminal access.

The steps involved were:

1. Open the RISC-V repository.
2. Select **Code**.
3. Open the **Codespaces** tab.
4. Create a new Codespace.
5. Wait for the workspace to initialize.

![GitHub Codespaces environment](https://github.com/user-attachments/assets/bc37547c-3bcd-4e26-93c4-180a61179de0?raw=true)

## Step 3: Inspect the Workspace

After initialization, I explored the workspace and used the terminal to access project files and prepare for the practical exercises.

![RISC-V workspace](https://github.com/user-attachments/assets/8330d74b-a1c0-4e37-904d-c6850aa7ddfd?raw=true)

---

# Section 2: RISC-V Compilation

## Step 4: Access the Sample Programs

The sample programs were accessed through the `samples` directory.

```bash
cd samples/
```

This directory contains example C programs used for RISC-V compilation and execution.

## Step 5: Compile the C Program

The RISC-V cross-compiler was used to compile the sample program.

```bash
riscv64-unknown-elf-gcc -o sum1ton.o sum1ton.c
```

**Command explanation:**

- `riscv64-unknown-elf-gcc` invokes the RISC-V cross-compiler.
- `-o sum1ton.o` specifies the output filename.
- `sum1ton.c` is the input C source file.

Successful compilation produces an executable for further analysis and simulation.

![RISC-V compilation output](https://github.com/user-attachments/assets/3b069b2b-0f76-4aed-bcc3-f6e1903ed50b?raw=true)

---

# Section 3: Machine-Code Analysis

## Step 6: Disassemble the Executable

The `objdump` utility was used to display the executable's machine instructions and their assembly representation.

```bash
objdump -d sum1ton.o
```

The disassembly helps examine:

- Generated assembly instructions
- Register operations
- Function execution
- Machine-level program flow

![Objdump disassembly output](https://github.com/user-attachments/assets/aae59e3e-3a3b-496f-82e5-a059554161ea?raw=true)

---

# Section 4: RISC-V Program Execution

## Step 7: Run the Program Using Spike

Spike was used to execute the compiled RISC-V program in an instruction set simulator.

```bash
spike pk sum1ton.o
```

Expected program output:

```text
Sum from 1 to 9 is 45
```

The simulator provides a way to explore program execution without running the program on physical RISC-V hardware.

![Spike execution](https://github.com/user-attachments/assets/df37322c-fa10-4a87-92d4-42b6f37dff57?raw=true)

### Additional Simulator Exploration

Further simulator interaction was used to explore execution details and available debugging information.

![Spike simulator details](https://github.com/user-attachments/assets/9261f031-e13a-472b-9ad0-ce68ddaba431?raw=true)

---

# Section 5: RTL Functional Verification

## Overview

RTL simulation helps verify whether a Verilog design behaves according to its intended functionality.

The verification process involves:

1. Preparing the RTL design
2. Writing a testbench
3. Compiling the Verilog files
4. Running the simulation
5. Generating waveform data
6. Inspecting signal transitions

## Step 8: Compile the RTL Design and Testbench

Icarus Verilog was used to compile the design and its testbench.

```bash
iverilog -o gmux verilog_files/good_mux.v verilog_files/tb_good_mux.v
```

**Command explanation:**

- `iverilog` compiles the Verilog source files.
- `good_mux.v` contains the RTL implementation.
- `tb_good_mux.v` contains the testbench.
- `-o gmux` specifies the generated simulation executable.

![Icarus Verilog compilation](https://github.com/user-attachments/assets/646e882e-11c7-49d2-b927-d56cfd37d7d3?raw=true)

---

# Section 6: Waveform Analysis with GTKWave

## Step 9: Inspect Simulation Signals

GTKWave was used to visualize signal transitions generated during RTL simulation.

Waveform analysis helps examine:

- Input signal behavior
- Select signal transitions
- Output responses
- Timing relationships between signals

This makes it easier to compare the observed behavior with the expected RTL functionality.

![GTKWave waveform analysis](https://github.com/user-attachments/assets/eba8326a-bfce-479d-a545-4e261e0de256?raw=true)

---

# Section 7: Understanding the RTL-to-GDS Flow

## Overview

I explored the OpenROAD project to understand the main stages involved in transforming an RTL design into a physical layout.

A typical digital physical design flow includes:

```text
RTL Design
    ↓
Logic Synthesis
    ↓
Floorplanning
    ↓
Placement
    ↓
Clock Tree Synthesis
    ↓
Routing
    ↓
GDS Generation
```

## Step 10: Explore the OpenROAD Repository

The OpenROAD-flow-scripts repository was examined to understand its directory structure, scripts, and design configuration files.

The repository includes directories and resources for design examples, automation scripts, and implementation tools.

![OpenROAD flow repository](https://github.com/user-attachments/assets/8bc54af3-5001-4011-b9e9-940245a3dc2a?raw=true)

---

# Section 8: Design Flow Configuration

## Step 11: Examine Makefile Settings

The Makefile and related configuration files were explored to understand how different designs and technology platforms are selected.

Example configuration entries:

```text
DESIGN_CONFIG=./designs/gf12/aes/config.mk
DESIGN_CONFIG=./designs/sky130hd/aes/config.mk
DESIGN_CONFIG=./designs/scl180/fs120/config.mk
```

These configuration settings can specify the design configuration used by the flow.

They help organize:

- Target design selection
- Technology-specific parameters
- Design configuration paths
- Automated implementation tasks
- Physical design stages

![Makefile configuration](https://github.com/user-attachments/assets/0520f858-34c8-4b8a-8a4d-798de9f73535?raw=true)

---

# Section 9: Physical Layout Visualization

## Step 12: Inspect the GDS Layout Using KLayout

KLayout was used to visualize and inspect the physical layout of a digital design.

The layout viewer helps examine the geometric representation of the design and its technology layers.

Examples of layers that may appear in a physical layout include:

- Metal layers
- Polysilicon layers
- Diffusion layers
- Well layers
- Contact layers
- Routing layers

The layout view provides a visual representation of the physical implementation produced by the design flow.

![KLayout GDS layout](https://github.com/user-attachments/assets/a106ac83-e7c4-4a8c-adee-1df9b950706e?raw=true)

---

# Skills and Concepts Practiced

The practical activities provided exposure to:

- Setting up a RISC-V development environment
- Using GitHub Codespaces
- Cross-compiling C programs for RISC-V
- Inspecting machine instructions using `objdump`
- Executing programs with Spike
- Compiling and simulating RTL designs
- Using testbenches for functional verification
- Analyzing waveforms with GTKWave
- Exploring the OpenROAD RTL-to-GDS workflow
- Understanding Makefile-based configuration
- Inspecting physical layouts using KLayout

---

# Conclusion

These practical exercises connected RISC-V software development, instruction-level analysis, RTL verification, and digital physical design.

Working with open-source tools provided an opportunity to explore how software is compiled and executed, how digital hardware is verified through simulation, and how physical design tools contribute to the implementation of integrated circuits.

The activities established a practical foundation for further learning in computer architecture, digital design, and VLSI physical design.
