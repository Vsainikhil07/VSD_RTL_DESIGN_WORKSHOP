# Hands-On Exploration of RISC-V, RTL Simulation, and the OpenROAD RTL-to-GDS Flow

**Author:** Vadla Sai Nikhil

---

## Overview

This repository documents my hands-on experiments with open-source tools used across the digital design pipeline, from writing and running software on RISC-V to simulating hardware in Verilog and finally studying how a design is turned into a physical chip layout.

### What this project covers

- Preparing a RISC-V workspace on GitHub Codespaces
- Cross-compiling a C program for the RISC-V target
- Disassembling the generated binary with `objdump`
- Running the program on the Spike ISA simulator
- Simulating Verilog RTL with Icarus Verilog
- Inspecting waveforms in GTKWave
- Studying the OpenROAD RTL-to-GDS flow
- Understanding Makefile-driven design configuration
- Viewing the final GDS layout in KLayout

### Tools used

| Tool | Purpose |
|------|---------|
| GitHub Codespaces | Cloud-based development environment |
| `riscv64-unknown-elf-gcc` | RISC-V cross compiler |
| `objdump` | Binary disassembly |
| Spike (+ `pk`) | RISC-V ISA simulator |
| Icarus Verilog | RTL compilation and simulation |
| GTKWave | Waveform viewer |
| OpenROAD / ORFS | Automated physical design flow |
| KLayout | GDS layout viewer |

> **Note on images:** All screenshots are stored in the `images/` folder. Update the paths below if you place them elsewhere.

---

## Part 1: Preparing the RISC-V Workspace

### 1.1 Open the project repository

I began by opening the RISC-V project on GitHub and going through its source files, example programs, and setup instructions. Instead of installing everything locally, I used GitHub Codespaces to get a ready-to-use cloud workspace.

![RISC-V repository page](https://raw.githubusercontent.com/YOUR_USERNAME/YOUR_REPO/main/images/01-riscv-repository.png)

### 1.2 Create a Codespace

A fresh Codespace was created directly from the repository:

1. Open the RISC-V repository on GitHub.
2. Click the green **Code** button.
3. Switch to the **Codespaces** tab.
4. Choose **Create codespace**.
5. Wait for the environment to finish setting up.

Codespaces gives a browser-based VS Code editor with an integrated terminal.

![Creating a new Codespace](https://raw.githubusercontent.com/YOUR_USERNAME/YOUR_REPO/main/images/02-create-codespace.png)

### 1.3 Verify the workspace

Once the environment was ready, I used the terminal to browse the project files and confirm that the sample programs and RISC-V toolchain were available.

![Codespace workspace and terminal](https://raw.githubusercontent.com/YOUR_USERNAME/YOUR_REPO/main/images/03-workspace-check.png)

---

## Part 2: Building a Program for RISC-V

### 2.1 Go to the samples directory

```bash
cd samples/
```

This folder holds example C programs that can be built for the RISC-V architecture.

### 2.2 Cross-compile the C source

```bash
riscv64-unknown-elf-gcc -o sum1ton.o sum1ton.c
```

**Breaking down the command**

- `riscv64-unknown-elf-gcc` is the RISC-V GCC cross compiler.
- `-o sum1ton.o` names the output executable.
- `sum1ton.c` is the C source file being compiled.

A successful build produces a RISC-V executable that can be disassembled and run in a simulator.

![Compiling sum1ton.c](https://raw.githubusercontent.com/YOUR_USERNAME/YOUR_REPO/main/images/04-compile-sum1ton.png)

---

## Part 3: Examining the Generated Machine Code

### 3.1 Disassemble the binary

```bash
objdump -d sum1ton.o
```

`objdump -d` prints the machine instructions along with their assembly equivalents. Reading this output helps in understanding:

- The assembly instructions the compiler produced
- How registers are used
- How functions are laid out and called
- The overall low-level control flow of the program

**Disassembly output**

![objdump disassembly output](https://raw.githubusercontent.com/YOUR_USERNAME/YOUR_REPO/main/images/05-objdump-output.png)

---

## Part 4: Running the Program on Spike

### 4.1 Execute with the proxy kernel

```bash
spike pk sum1ton.o
```

Spike simulates a RISC-V processor, and `pk` (the proxy kernel) handles system calls for the program. The expected output is:

```
Sum from 1 to 9 is 45
```

Seeing this result confirms that the program was compiled correctly for RISC-V and behaves as intended.

![Spike execution, run 1](https://raw.githubusercontent.com/YOUR_USERNAME/YOUR_REPO/main/images/06-spike-run-1.png)

![Spike execution, run 2](https://raw.githubusercontent.com/YOUR_USERNAME/YOUR_REPO/main/images/07-spike-run-2.png)

---

## Part 5: RTL Functional Verification

### Overview

To check that a Verilog design works correctly, I followed a standard simulation-based verification flow:

1. Write the RTL design
2. Write a testbench
3. Compile both files
4. Run the simulation
5. Generate a waveform dump
6. Analyze the waveform

### 5.1 Compile the design and testbench

```bash
iverilog -o gmux verilog_files/good_mux.v verilog_files/tb_good_mux.v
```

**What this does**

- `iverilog` compiles the Verilog sources into a simulation executable.
- `good_mux.v` holds the RTL design under test (a multiplexer).
- `tb_good_mux.v` is the testbench that drives the inputs.
- The resulting `gmux` file is then run to carry out the functional check.

![Compiling the mux with Icarus Verilog](https://raw.githubusercontent.com/YOUR_USERNAME/YOUR_REPO/main/images/08-iverilog-compile.png)

---

## Part 6: Analyzing Signals in GTKWave

### 6.1 Open the waveform

After running the simulation, the generated dump file was loaded into GTKWave, which shows how each signal changes over time.

From the waveform I could check:

- How the input signals behave
- When the select line switches
- How the output responds to those changes
- The timing relationship between inputs and output

The waveform matched the expected multiplexer behavior, confirming the RTL is functionally correct.

![GTKWave waveform of the multiplexer](https://raw.githubusercontent.com/YOUR_USERNAME/YOUR_REPO/main/images/09-gtkwave-waveform.png)

---

## Part 7: Understanding the RTL-to-GDS Flow

### Overview

I explored the OpenROAD project to learn how a Verilog design is converted into a final GDS layout. OpenROAD automates many of the steps involved in digital IC physical design.

### Physical design pipeline

```
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

### 7.1 Explore the OpenROAD flow repository

I opened the OpenROAD RTL-to-GDS repository in GitHub Codespaces. It contains the OpenROAD-flow-scripts (ORFS) setup along with the configuration files needed to run complete physical design flows.

Key directories:

```
images/
orfs/
designs/
scripts/
tools/
```

The repository supports several technology nodes and multiple example designs.

![OpenROAD flow repository structure](https://raw.githubusercontent.com/YOUR_USERNAME/YOUR_REPO/main/images/10-openroad-repository.png)

---

## Part 8: Configuring the Flow with the Makefile

### 8.1 Review design configuration paths

The Makefile points to configuration files for different technology libraries and designs. A few examples:

```makefile
DESIGN_CONFIG=./designs/gf12/aes/config.mk
DESIGN_CONFIG=./designs/sky130hd/aes/config.mk
DESIGN_CONFIG=./designs/scl180/fs120/config.mk
```

**Role of these configuration files**

- Choose which design to implement
- Set technology-specific parameters
- Reference the design's configuration (`config.mk`)
- Automate the RTL-to-GDS process end to end
- Control each individual physical design stage

![Makefile design configuration](https://raw.githubusercontent.com/YOUR_USERNAME/YOUR_REPO/main/images/11-makefile-config.png)

---

## Part 9: Viewing the Final GDS Layout

### 9.1 Open the layout in KLayout

The finished layout produced by the physical design flow was opened in KLayout. The GDS file contains several technology layers that represent the physical structures of the chip.

**Layers visible in the layout**

- Metal layers
- Poly layers
- Diffusion layers
- Well layers
- Contact layers
- Routing layers

This view shows the end result of the RTL-to-GDS flow: the physical implementation of the design.

![Final GDS layout in KLayout](https://raw.githubusercontent.com/YOUR_USERNAME/YOUR_REPO/main/images/12-klayout-gds-layout.png)

---

## Skills and Concepts Practiced

- Setting up a RISC-V development environment
- Working with GitHub Codespaces
- Cross-compiling C code for RISC-V
- Reading disassembled executables
- Running programs on the Spike simulator
- Performing RTL simulation with Icarus Verilog
- Writing and using testbenches for functional verification
- Interpreting waveforms in GTKWave
- Understanding the OpenROAD RTL-to-GDS flow
- Navigating technology and design configuration files
- Understanding Makefile-based automation
- Inspecting GDS layouts in KLayout

---

## Conclusion

These experiments gave me practical exposure to multiple stages of digital design and VLSI implementation. Starting from RISC-V software compilation and RTL simulation, and moving on to physical design and layout visualization, the work covered a complete learning path using only open-source tools.

Working with RISC-V GCC, Spike, Icarus Verilog, GTKWave, OpenROAD, and KLayout helped me build a solid understanding of:

- Software compilation for RISC-V
- Instruction-level analysis
- RTL functional verification
- Digital waveform analysis
- Physical design automation
- The RTL-to-GDS implementation flow
- Final layout visualization

Overall, this workflow shows how open-source tools can be combined to explore the wider semiconductor design ecosystem.

---

**Author:** Vadla Sai Nikhil
