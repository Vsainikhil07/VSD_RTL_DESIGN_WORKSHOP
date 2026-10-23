# BabySoC RTL Synthesis and Gate-Level Simulation

## Overview

This section describes the synthesis and Gate-Level Simulation (GLS) process carried out on the BabySoC design. The RTL source files were processed using Yosys, mapped to the SKY130 standard-cell library, and functionally verified through Icarus Verilog and GTKWave.

The major stages of the implementation flow are shown below:

```text
RTL Source Files
       ↓
RTL Synthesis Using Yosys
       ↓
SKY130 Technology Mapping
       ↓
Structural Verification
       ↓
Gate-Level Netlist Creation
       ↓
Post-Synthesis Simulation
       ↓
Waveform Comparison
```

---

## 1. Importing RTL Source Files into Yosys

The process starts by opening Yosys and loading the Verilog files required for the BabySoC design.

```bash
yosys
read_verilog src/module/vsdbabysoc.v

read_verilog -I src/include/src/module/rvmyth.v

read_verilog -I src/include/src/module/clk_gate.v
```

The `vsdbabysoc` module acts as the top-level design, while `rvmyth` and `clk_gate` provide the supporting processor and clock-gating functionality.

### Figure 1: Loading BabySoC RTL Files

<img width="1920" height="1060" alt="image" src="https://github.com/user-attachments/assets/be80524e-5f66-49d8-bf79-24ba47af4333" />

The RTL files are loaded into Yosys to establish the design structure for the subsequent synthesis stages.

---

## 2. Configuring Libraries and Running Synthesis

Before synthesis, the required Liberty files are read into Yosys. They provide the cell definitions and relevant implementation information for the PLL, DAC, and SKY130 standard cells.

```bash
read_liberty -lib src/lib/avsdpll.lib

read_liberty -lib src/lib/avsddac.lib

read_liberty -lib src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib

synth -top vsdbabysoc
```

The `synth` command processes the RTL hierarchy and performs synthesis with `vsdbabysoc` selected as the top module.

### Figure 2: BabySoC Module Hierarchy

<img width="1920" height="1060" alt="image" src="https://github.com/user-attachments/assets/08abefc6-1854-435e-b297-a831388c2553" />

The hierarchy view illustrates the organization of the design and the relationships between its functional modules.

---

## 3. Reviewing Synthesis Statistics

Once synthesis is complete, Yosys provides statistics describing the resulting design, including wires, ports, cells, and module information.

### Figure 3: Synthesized Design Statistics

<img width="1920" height="1060" alt="image" src="https://github.com/user-attachments/assets/fca7327c-12ef-43e0-9d46-a691bdb3cbe1" />

These statistics help assess the structure and complexity of the synthesized BabySoC implementation.

---

## 4. Checking the Synthesized Circuit

The Yosys `check` pass is used to inspect the synthesized design for common structural problems.

The reported result was:

```text
Found and reported 0 problems.
```

This indicates that the check pass did not identify any structural issues in the design at that stage.

### Figure 4: Yosys Structural Verification

<img width="1920" height="1060" alt="image" src="https://github.com/user-attachments/assets/f93166d7-9dc7-46ba-b4d5-47413e41932f" />

The successful check provides an initial verification of the synthesized circuit structure.

---

## 5. Mapping Logic to SKY130 Cells

The synthesized design is mapped to cells from the SKY130 high-density standard-cell library.

Sequential elements are mapped using the following command:

```bash
dfflibmap -liberty src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

ABC is then used for combinational logic mapping and optimization:

```bash
abc -liberty src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

The mapping process replaces generic logic with available library cells, including logic gates, multiplexers, and flip-flops.

### Figure 5: Standard-Cell Mapping Output

<img width="1920" height="1060" alt="image" src="https://github.com/user-attachments/assets/804e5497-7e5b-4fcc-bb97-55c9526f8df8" />

The ABC results show the standard-cell types selected during the technology-mapping stage.

---

## 6. Inspecting the Mapped Netlist

The synthesized circuit can be examined graphically using the Yosys `show` command.

```bash
show vsdbabysoc
```

### Figure 6: Synthesized Circuit Diagram

<img width="1920" height="1060" alt="image" src="https://github.com/user-attachments/assets/5e189e6c-5e49-45a6-a953-3f81abe6baff" />

The generated diagram provides a view of the circuit connections and the logic present in the synthesized implementation.

---

## 7. Understanding the Main BabySoC Blocks

The major functional blocks of the BabySoC design can be represented using the following simplified connectivity diagram:

```text
ENb_CP ─┐
ENb_VCO ┤
REF ────┤
VCO_IN ─┘
        │
        ▼
   +------------+
   |  avsdpll   |
   |    PLL     |
   +------------+
        │ CLK
        ▼
   +------------+
   |   rvmyth   |
   | RISC-V Core|
   +------------+
        │ RV_TO_DAC
        ▼
   +------------+
   |   avsdac   |
   |    DAC     |
   +------------+
        │
        ▼
       OUT
```

This simplified representation highlights the clock-generation, processing, and output-conversion blocks involved in the design.

---

## 8. Flattening and Cleaning the Design

Before exporting the final gate-level netlist, the design is flattened and cleaned to prepare its internal representation.

```bash
flatten
setundef -zero
clean -purge
rename -enumerate
```

The commands perform the following operations:

- `flatten` combines the module hierarchy into a flattened design.
- `setundef -zero` replaces undefined logic values with zero.
- `clean -purge` removes unused logic and unnecessary design objects.
- `rename -enumerate` assigns systematic names to internal objects.

These steps prepare the synthesized circuit for Verilog netlist generation.

---

## 9. Exporting the Gate-Level Netlist

The processed design is written to a Verilog file using the following command:

```bash
write_verilog -noattr baby_soc_netlist3.v
```

The output file contains the synthesized circuit representation, including its internal connections and mapped logic cells.

### Figure 7: Exported BabySoC Netlist

<img width="1920" height="1060" alt="image" src="https://github.com/user-attachments/assets/8cd53d63-3940-4309-bdb7-f7e0a5365ecf" />

The generated netlist serves as the design input for post-synthesis gate-level simulation.

---

## 10. Setting Up the Simulation Testbench

A shared testbench is used to simulate the BabySoC design before and after synthesis. Conditional compilation directives determine which design files are selected for each simulation mode.

For RTL simulation, the testbench uses:

```verilog
`ifdef PRE_SYNTH_SIM
```

For gate-level simulation, the corresponding section is selected using:

```verilog
`elsif POST_SYNTH_SIM
```

The testbench initializes and applies stimulus to signals such as:

- `reset`
- `VREF`
- `REF`
- `ENb_CP`
- `ENb_VCO`
- `VCO_IN`

### Figure 8: Simulation Testbench Configuration

<img width="1920" height="1060" alt="image" src="https://github.com/user-attachments/assets/e7cd2cc5-6f0f-4e62-8fed-d30faf6cde9e" />

The same testbench structure supports functional comparison between the original RTL and the synthesized implementation.

---

## 11. Running Gate-Level Functional Simulation

Icarus Verilog is used to compile the testbench for post-synthesis simulation. The `POST_SYNTH_SIM` and `FUNCTIONAL` definitions select the required simulation configuration and functional cell models.

```bash
sudo iverilog -DPOST_SYNTH_SIM -DFUNCTIONAL -I src/include/ -I ../../sky130RTLDesignAndSynthesisWorkshop/my_lib/verilog_model/ -I src/module/ src/module/testbench.v
```

The purpose of each compilation option is summarized below:

| Option | Description |
|---|---|
| `-DPOST_SYNTH_SIM` | Selects the post-synthesis simulation branch |
| `-DFUNCTIONAL` | Enables functional simulation models |
| `-I src/include/` | Adds the design include directory |
| `-I ../../sky130RTLDesignAndSynthesisWorkshop/my_lib/verilog_model/` | Adds the SKY130 Verilog model directory |
| `-I src/module/` | Adds the module source directory |

After compilation, the simulation executable is run:

```bash
./a.out
```

The resulting waveform file is opened in GTKWave:

```bash
gtkwave post_synth_sim.vcd
```

GTKWave allows the simulation signals and their transitions to be examined over time.

---

## 12. Comparing RTL and Gate-Level Waveforms

The final stage compares the pre-synthesis and post-synthesis simulation waveforms to examine whether the synthesized design preserves the expected functional behavior.

The main signals observed include:

- `CLK`
- `reset`
- `OUT`
- `RV_TO_DAC`

### Figure 9: RTL and Post-Synthesis Waveform Analysis

<img width="1592" height="768" alt="post and pre" src="https://github.com/user-attachments/assets/2c49be38-d656-429b-9763-c303e018534f" />

The waveforms are inspected in GTKWave to compare signal transitions and verify functional consistency between the two simulation stages.

---

## Results Summary

The BabySoC synthesis and gate-level simulation workflow covered the following activities:

- Loaded the required RTL source files into Yosys.
- Imported the PLL, DAC, and SKY130 Liberty libraries.
- Synthesized the design using `vsdbabysoc` as the top module.
- Examined the module hierarchy and synthesis statistics.
- Ran the Yosys structural check, which reported **0 problems**.
- Mapped sequential and combinational logic to SKY130 standard cells.
- Inspected the synthesized circuit using the Yosys visualization tools.
- Flattened and cleaned the design before exporting the netlist.
- Generated the gate-level Verilog file.
- Configured the testbench for post-synthesis simulation.
- Compiled and executed the simulation using Icarus Verilog.
- Viewed the generated waveforms in GTKWave.
- Compared pre-synthesis and post-synthesis signal behavior.

---

## Conclusion

The BabySoC design was synthesized using Yosys and mapped to the SKY130 standard-cell library. Structural verification was performed, and the resulting design was exported as a gate-level Verilog netlist.

Post-synthesis functional simulation was carried out using Icarus Verilog with the appropriate compilation definitions and functional cell models. GTKWave was then used to inspect and compare the pre-synthesis and post-synthesis waveforms.

This experiment demonstrates the major stages of a digital design implementation workflow, from RTL processing and technology mapping to gate-level netlist generation and functional simulation.

**RTL Design → Synthesis → Technology Mapping → Netlist Generation → Gate-Level Simulation → Waveform Verification**
