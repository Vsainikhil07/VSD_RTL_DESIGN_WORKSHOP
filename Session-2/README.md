# RTL Design, Verification, and Synthesis with Open-Source EDA Tools

**Author:** Vadla Sai Nikhil

## Overview

This repository documents hands-on experiments involving RTL design, functional verification, logic synthesis, hierarchical circuit visualization, and waveform analysis using open-source Electronic Design Automation (EDA) tools.

The practical work uses Icarus Verilog, GTKWave, and Yosys to explore the workflow from Verilog source code to simulation results and synthesized logic representations.

### Experiments Included

1. 2:1 Multiplexer Design and Functional Verification
2. Hierarchical RTL Synthesis and Circuit Visualization
3. BabySoC Design Exploration and Netlist Generation
4. Pre-Synthesis Simulation and Waveform Inspection

---

# Experiment 1: Multiplexer Design and Verification

## Objective

To implement and verify a 2:1 multiplexer in Verilog, simulate different input conditions, and examine its synthesized circuit representation.

## 1.1 Testbench Preparation

A testbench was developed for the `good_mux` module to apply different combinations of input signals and the select line.

The following system tasks were used to capture simulation activity:

```verilog
$dumpfile("tb_good_mux.vcd");
$dumpvars(0, tb_good_mux);
```

The signals `i0`, `i1`, `sel`, and `y` were monitored to observe the relationship between the inputs, select signal, and output.

### Screenshot: Multiplexer Testbench

<img width="1001" height="1600" alt="MUX Testbench" src="https://github.com/user-attachments/assets/0b43588a-9301-4d53-b7bd-2f684ac3df32?raw=true" />

## 1.2 Simulation Compilation and Execution

The RTL design and testbench were compiled using Icarus Verilog.

```bash
iverilog -o tb_good_mux.out good_mux.v tb_good_mux.v
```

The simulation was then executed with:

```bash
vvp tb_good_mux.out
```

The testbench generated the `tb_good_mux.vcd` waveform file for subsequent analysis.

## 1.3 Waveform Analysis

GTKWave was used to open the generated waveform:

```bash
gtkwave tb_good_mux.vcd
```

The waveform was inspected to understand how the output `y` responds to changes in `i0`, `i1`, and `sel`.

### Screenshot: Simulation Waveform

<img width="1136" height="524" alt="MUX Simulation Waveform" src="https://github.com/user-attachments/assets/c3891025-f081-420f-ab91-4bb77497758d?raw=true" />

## 1.4 Logic Synthesis with Yosys

Yosys was used to process and optimize the multiplexer RTL description.

The source file can be inspected using:

```bash
vi good_mux.v
```

The synthesis process translates the RTL into a logic representation and can generate a netlist containing generic or technology-specific cells, depending on the selected synthesis flow.

## 1.5 Viewing the Synthesized Circuit

The Yosys `show` command was used to visualize the selected design.

```tcl
show
```

The schematic provides a graphical representation of the logical connections between the multiplexer inputs, select signal, and output.

### Screenshot: Synthesized Multiplexer Schematic

<img width="1524" height="808" alt="Synthesized MUX Schematic" src="https://github.com/user-attachments/assets/55fc92ae-4960-4d35-8466-25fbbcadb7a1?raw=true" />

### Result

The multiplexer was simulated and its waveform was inspected. Synthesis and schematic visualization provided a clearer understanding of how a Verilog description can be represented as a logic circuit.

---

# Experiment 2: Hierarchical RTL Synthesis

## Objective

To explore interconnected Verilog modules, understand their signal connectivity, and visualize the resulting circuit using Yosys.

## 2.1 Processing Multiple RTL Modules

The multi-module RTL design was processed through synthesis operations, including logic optimization and module processing. Cell mapping depends on the synthesis script and target library.

### Screenshot: Multi-Module Synthesis

<img width="1599" height="899" alt="Multi-Module Synthesis" src="https://github.com/user-attachments/assets/3e43f919-d01a-4d49-94e2-035b79b672a3?raw=true" />

## 2.2 Circuit Schematic Generation

The selected design was displayed using the Yosys visualization command:

```tcl
show
```

The schematic helps identify logic elements, signal paths, and the connections between circuit inputs and outputs.

### Screenshot: Synthesized Circuit

<img width="1599" height="899" alt="Synthesized Circuit View" src="https://github.com/user-attachments/assets/8bddff3a-6de4-4e15-b8c5-179cf999675c?raw=true" />

## 2.3 Understanding Hierarchical Connectivity

The multi-module design was visualized using:

```tcl
show multiple_modules
```

The design illustrates how two submodules can communicate through an intermediate signal.

The main components and signals are:

- Inputs: `a`, `b`, and `c`
- First submodule: `submodule1`
- Intermediate signal: `net1`
- Second submodule: `submodule2`
- Output: `y`

### Screenshot: Module Interconnections

<img width="1599" height="899" alt="Hierarchical Module Connections" src="https://github.com/user-attachments/assets/e202bfd0-5bba-4dc6-bc4c-8905c441dfe5?raw=true" />

### Result

This experiment demonstrated how separate RTL modules can be interconnected to form a larger digital circuit. The generated schematic made the signal flow and hierarchical relationships easier to examine.

---

# Experiment 3: BabySoC RTL Exploration and Netlist Generation

## Objective

To examine the BabySoC RTL hierarchy, review its source files, and explore netlist generation and hierarchy manipulation using Yosys.

## 3.1 Exploring the BabySoC Architecture

BabySoC integrates a processor core with supporting blocks such as PLL, DAC, clock logic, and other functional modules.

The following command writes the current Yosys design to a Verilog netlist:

```tcl
write_verilog -noattr multiple_modules_netlist.v
```

The design hierarchy can be flattened using:

```tcl
flatten
```

The selected design can then be visualized with:

```tcl
show multiple_modules
```

**Note:** The `flatten` command modifies the in-memory design hierarchy. Its result depends on the modules that were read and selected before the command was executed.

### Screenshot: BabySoC Design Hierarchy

<img width="1920" height="1060" alt="BabySoC Design Hierarchy" src="https://github.com/user-attachments/assets/34b6b0b0-04dd-4268-a148-37bfbf604f5c?raw=true" />

## 3.2 Exporting the Processed Netlist

After hierarchy processing, the design can be exported to another Verilog file:

```tcl
write_verilog -noattr multiple_modules_netlist2.v
```

The generated file represents the design in its current state after the hierarchy operations performed by Yosys.

## 3.3 Reviewing the RTL Source Files

The BabySoC project contains multiple Verilog source files, each implementing a particular part of the system.

| File | Purpose |
|---|---|
| `avsdac.v` | DAC module |
| `avspll.v` | PLL module |
| `clk_gate.v` | Clock-gating logic |
| `pseudo_rand.v` | Pseudorandom logic |
| `pseudo_rand_gen.v` | Pseudorandom generator |
| `rvmyth.v` | Processor core |
| `rvmyth_gen.v` | Processor-related generation logic |
| `testbench.v` | Simulation testbench |
| `vsdbabysoc.v` | Top-level BabySoC module |

### Screenshot: BabySoC RTL Files

<img width="1920" height="1060" alt="BabySoC RTL Source Files" src="https://github.com/user-attachments/assets/2e0aca5c-4208-44ce-aec9-b8f0d9fedc6f?raw=true" />

## 3.4 Examining the RVMYTH Processor Core

The `rvmyth.v` file contains the RVMYTH processor implementation. Its logic includes clock and reset interfaces, instruction-processing operations, and program-counter-related functionality.

The processor interacts with the other BabySoC modules as part of the complete system.

### Screenshot: RVMYTH RTL Implementation

<img width="1920" height="1060" alt="RVMYTH Processor RTL" src="https://github.com/user-attachments/assets/b10b537f-a770-4601-a017-74575d843095?raw=true" />

---

# Experiment 4: BabySoC Pre-Synthesis Simulation

## Objective

To compile the BabySoC RTL with its testbench, execute a pre-synthesis simulation, and inspect the resulting waveform signals.

## 4.1 Inspecting the Testbench

The following commands help locate the top-level module declaration and include directives in `testbench.v`.

```bash
grep -n "module vsdbabysoc" testbench.v
```

```bash
grep -n "include" testbench.v
```

These commands help identify relevant declarations and source-file references. They do not, by themselves, establish that all required modules compile successfully.

## 4.2 Compiling the RTL Design

The testbench was compiled using Icarus Verilog with the pre-synthesis macro and include directory.

```bash
iverilog -o pre_synth_sim.out -DPRE_SYNTH_SIM -I ./include testbench.v
```

### Command Options

| Option | Description |
|---|---|
| `iverilog` | Verilog compiler |
| `-o pre_synth_sim.out` | Specifies the output executable |
| `-DPRE_SYNTH_SIM` | Defines the `PRE_SYNTH_SIM` macro |
| `-I ./include` | Adds the include directory to the search path |
| `testbench.v` | Specifies the testbench source file |

Compilation must finish without errors before the simulation can be run.

### Screenshot: BabySoC Compilation

<img width="1920" height="1060" alt="BabySoC Compilation" src="https://github.com/user-attachments/assets/3a7f07f8-7bf7-4246-8833-0f3a1cdea093?raw=true" />

## 4.3 Running the Simulation

The compiled simulation executable was launched with:

```bash
./pre_synth_sim.out
```

The testbench is expected to generate a VCD waveform file named:

```text
pre_synth_sim.vcd
```

The actual waveform filename is determined by the dump-file statement in the testbench.

## 4.4 Opening the Waveform in GTKWave

The waveform file was opened using:

```bash
gtkwave pre_synth_sim.vcd
```

The following signals were selected for analysis:

- `CLK`
- `ENb_CP`
- `ENb_VCO`
- `OUT`
- `REF`
- `RV_TO_DAC`
- `VCO_IN`
- `VREFH`
- `reset`

### Screenshot: BabySoC Waveform Viewer

<img width="1920" height="1060" alt="BabySoC Waveform Viewer" src="https://github.com/user-attachments/assets/f5b189dc-4cd7-4567-bb82-29cf62871092?raw=true" />

## 4.5 Analyzing the RV_TO_DAC Bus

The `RV_TO_DAC` bus was expanded in GTKWave to examine its individual bits:

- `RV_TO_DAC[9]`
- `RV_TO_DAC[8]`
- `RV_TO_DAC[7]`
- `RV_TO_DAC[6]`
- `RV_TO_DAC[5]`
- `RV_TO_DAC[4]`
- `RV_TO_DAC[3]`
- `RV_TO_DAC[2]`
- `RV_TO_DAC[1]`
- `RV_TO_DAC[0]`

Inspecting the individual bits allows their transitions to be compared with the other signals in the waveform. Their exact values and timing should be interpreted from the simulation results.

### Screenshot: RV_TO_DAC Signal Analysis

<img width="1920" height="1060" alt="RV_TO_DAC Bus Analysis" src="https://github.com/user-attachments/assets/7041a770-d09d-4d46-b4d6-6d9d7e7cbf05?raw=true" />

---

# Command Reference

## Multiplexer Simulation

```bash
vi good_mux.v
iverilog -o tb_good_mux.out good_mux.v tb_good_mux.v
vvp tb_good_mux.out
gtkwave tb_good_mux.vcd
```

## Yosys Circuit Visualization

```tcl
show
show multiple_modules
```

## Netlist Generation and Hierarchy Processing

```tcl
write_verilog -noattr multiple_modules_netlist.v
flatten
show multiple_modules
write_verilog -noattr multiple_modules_netlist2.v
```

## BabySoC Compilation and Waveform Inspection

```bash
grep -n "module vsdbabysoc" testbench.v
grep -n "include" testbench.v
iverilog -o pre_synth_sim.out -DPRE_SYNTH_SIM -I ./include testbench.v
./pre_synth_sim.out
gtkwave pre_synth_sim.vcd
```

---

# Tools and Technologies

- **Verilog HDL:** RTL design and hardware description
- **Icarus Verilog:** Compilation and RTL simulation
- **GTKWave:** Waveform visualization and signal inspection
- **Yosys:** Logic synthesis, optimization, and netlist generation
- **Graphviz:** Graph-based schematic rendering when used by the Yosys visualization flow

---

# Key Learning Outcomes

Through these experiments, I explored:

- Developing and verifying a 2:1 multiplexer
- Compiling and executing Verilog simulations
- Interpreting signal transitions in GTKWave
- Synthesizing RTL using Yosys
- Visualizing hierarchical module connections
- Exploring the BabySoC source hierarchy
- Generating and exporting Verilog netlists
- Preparing a pre-synthesis simulation
- Examining processor-to-DAC interface signals

---

# Conclusion

These experiments provided practical exposure to RTL development, functional simulation, logic synthesis, hierarchical circuit analysis, and waveform inspection using open-source EDA tools.

The multiplexer exercise covered basic verification and synthesis, while the multi-module experiment explored hierarchical connectivity. The BabySoC activities extended this exploration to a larger RTL design, including netlist generation and pre-synthesis waveform analysis.

The overall workflow can be summarized as:

**RTL Design → Testbench Development → Simulation → Waveform Analysis → Logic Synthesis → Netlist Visualization**

This work provides a foundation for further exploration of digital design verification and open-source semiconductor design tools.

**Author:** Vadla Sai Nikhil
