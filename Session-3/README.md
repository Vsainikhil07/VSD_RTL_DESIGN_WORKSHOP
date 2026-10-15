# BabySoC RTL Synthesis and Gate-Level Simulation

## Overview

This section explains the RTL synthesis and Gate-Level Simulation (GLS) workflow performed on the BabySoC design. The Verilog RTL files were processed using Yosys, mapped to the SKY130 standard-cell library, and simulated using Icarus Verilog. GTKWave was used to inspect and compare the simulation waveforms.

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
Gate-Level Netlist Generation
       ↓
Post-Synthesis Simulation
       ↓
Waveform Analysis and Comparison
```

---

## 1. Importing RTL Source Files into Yosys

The process begins by launching Yosys and loading the Verilog source files required for the BabySoC design.

```bash
yosys
read_verilog src/module/vsdbabysoc.v
read_verilog -I src/include/ src/module/rvmyth.v
read_verilog -I src/include/ src/module/clk_gate.v
```

The `vsdbabysoc` module acts as the top-level module, while `rvmyth` and `clk_gate` provide supporting processor and clock-gating functionality.

### Figure 1: Loading BabySoC RTL Files

<img width="1920" height="1060" alt="Loading BabySoC RTL files into Yosys" src="https://github.com/user-attachments/assets/be80524e-5f66-49d8-bf79-24ba47af4333?raw=true" />

The source files establish the design structure that will be processed during synthesis.

---

## 2. Configuring Libraries and Running Synthesis

Before technology mapping, the required Liberty library files are loaded into Yosys. These files describe the characteristics and available cells for the PLL, DAC, and SKY130 standard-cell library.

```bash
read_liberty -lib src/lib/avsdpll.lib
read_liberty -lib src/lib/avsddac.lib
read_liberty -lib src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib

synth -top vsdbabysoc
```

The `synth` command performs the synthesis process with `vsdbabysoc` selected as the top-level module.

### Figure 2: BabySoC Module Hierarchy

<img width="1920" height="1060" alt="BabySoC module hierarchy" src="https://github.com/user-attachments/assets/08abefc6-1854-435e-b297-a831388c2553?raw=true" />

The hierarchy view shows the organization of the design and the relationships between its functional modules.

---

## 3. Reviewing Synthesis Statistics

After synthesis, Yosys provides statistics about the processed design, including the number of wires, ports, cells, and module information.

### Figure 3: Synthesized Design Statistics

<img width="1920" height="1060" alt="Synthesized design statistics" src="https://github.com/user-attachments/assets/fca7327c-12ef-43e0-9d46-a691bdb3cbe1?raw=true" />

These statistics help understand the structure and complexity of the synthesized design and provide an overview of the logic produced by the synthesis process.

---

## 4. Performing Structural Verification

The Yosys `check` pass is used to identify common structural problems in the synthesized design.

The reported result was:

```text
Found and reported 0 problems.
```

This indicates that the check pass did not detect any structural issues within the design at that stage.

### Figure 4: Yosys Structural Verification

<img width="1920" height="1060" alt="Yosys structural verification output" src="https://github.com/user-attachments/assets/f93166d7-9dc7-46ba-b4d5-47413e41932f?raw=true" />

This check provides an initial verification of the circuit structure. It does not, by itself, prove that the design is functionally correct.

---

## 5. Mapping Logic to SKY130 Standard Cells

Technology mapping converts the generic logic produced during synthesis into cells available in the target standard-cell library.

Sequential elements are mapped using:

```bash
dfflibmap -liberty src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

ABC is then used to optimize and map combinational logic:

```bash
abc -liberty src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

The mapping process selects suitable library cells, such as logic gates and flip-flops, to implement the synthesized design.

### Figure 5: Standard-Cell Mapping Output

<img width="1920" height="1060" alt="SKY130 standard-cell mapping output" src="https://github.com/user-attachments/assets/804e5497-7e5b-4fcc-bb97-f79f?raw=true" />

The ABC output provides information about the logic optimization and standard-cell mapping performed during this stage.

---

## 6. Inspecting the Synthesized Circuit

The Yosys `show` command can be used to visualize the synthesized circuit and inspect its logic connections.

```bash
show vsdbabysoc
```

### Figure 6: Synthesized Circuit Diagram

<img width="1920" height="1060" alt="Synthesized BabySoC circuit diagram" src="https://github.com/user-attachments/assets/5e189e6c-5e49-45a6-a953-3f81abe6baff?raw=true" />

The generated schematic provides a graphical representation of the circuit and its internal logic connections.

---

## 7. Understanding the Main BabySoC Blocks

The BabySoC design combines clock generation, processing, and output-conversion functionality. A simplified representation of the main functional blocks is shown below.

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

The PLL generates the clock used by the processor, while the processor produces digital output data for the DAC. The DAC converts the digital data into an analog output.

This diagram is a simplified functional overview; the actual signal connections depend on the RTL implementation.

---

## 8. Flattening and Cleaning the Design

Before generating the gate-level netlist, the design can be flattened and cleaned to simplify its internal representation.

```bash
flatten
setundef -zero
clean -purge
rename -enumerate
```

The commands perform the following operations:

- **`flatten`** — combines the module hierarchy into a flattened design.
- **`setundef -zero`** — replaces undefined values with zero.
- **`clean -purge`** — removes unused logic and unnecessary design objects.
- **`rename -enumerate`** — assigns systematic names to internal objects.

These operations prepare the design for netlist generation. The effect of each command should be considered carefully because replacing undefined values may affect the modeled behavior.

---

## 9. Exporting the Gate-Level Netlist

The synthesized design is exported as a Verilog netlist using the following command:

```bash
write_verilog -noattr baby_soc_netlist3.v
```

The output file contains the synthesized circuit representation, including internal connections and mapped logic cells.

### Figure 7: Exported BabySoC Netlist

<img width="1920" height="1060" alt="Exported BabySoC gate-level netlist" src="https://github.com/user-attachments/assets/8cd53d63-3940-4309-bdb7-f7e0a5365ecf?raw=true" />

The generated netlist serves as the design input for post-synthesis gate-level simulation.

---

## 10. Configuring the Simulation Testbench

A testbench is used to provide stimulus to the BabySoC design and observe its outputs. Conditional compilation directives allow the testbench to select the appropriate configuration for RTL simulation or post-synthesis simulation.

For RTL simulation, the testbench uses:

```verilog
`ifdef PRE_SYNTH_SIM
```

For gate-level simulation, the corresponding branch is selected using:

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

<img width="1920" height="1060" alt="BabySoC simulation testbench configuration" src="https://github.com/user-attachments/assets/e7cd2cc5-6f0f-4e62-8fed-d30faf6cde9e?raw=true" />

Using the same testbench structure helps maintain consistent stimulus when comparing RTL and gate-level simulation results.

---

## 11. Running Gate-Level Functional Simulation

Icarus Verilog is used to compile the testbench for post-synthesis simulation. The `POST_SYNTH_SIM` and `FUNCTIONAL` definitions select the appropriate simulation configuration and functional cell models.

```bash
sudo iverilog -DPOST_SYNTH_SIM -DFUNCTIONAL \
-I src/include/ \
-I ../../sky130RTLDesignAndSynthesisWorkshop/my_lib/verilog_model/ \
-I src/module/ \
src/module/testbench.v
```

The compilation options are summarized below:

| Option | Description |
|---|---|
| `-DPOST_SYNTH_SIM` | Selects the post-synthesis simulation branch. |
| `-DFUNCTIONAL` | Enables the relevant functional simulation models. |
| `-I src/include/` | Adds the design include directory. |
| `-I ../../sky130RTLDesignAndSynthesisWorkshop/my_lib/verilog_model/` | Adds the SKY130 Verilog model directory. |
| `-I src/module/` | Adds the module source directory. |

After successful compilation, run the simulation executable:

```bash
./a.out
```

Open the generated waveform file in GTKWave:

```bash
gtkwave post_synth_sim.vcd
```

GTKWave displays signal transitions over time, allowing the internal and output behavior of the synthesized design to be inspected.

**Note:** The source paths, netlist filename, cell models, and waveform filename must match the files in your actual project. The commands above assume that the simulation setup generates `a.out` and `post_synth_sim.vcd`.

---

## 12. Comparing RTL and Gate-Level Waveforms

The final stage involves inspecting the RTL and post-synthesis simulation waveforms to determine whether the synthesized design preserves the expected functional behavior.

The main signals observed include:

- `CLK`
- `reset`
- `OUT`
- `RV_TO_DAC`

### Figure 9: RTL and Post-Synthesis Waveform Analysis

<img width="1592" height="768" alt="RTL and post-synthesis waveform comparison" src="https://github.com/user-attachments/assets/2c49be38-d656-429b-9763-c303e018534f?raw=true" />

The waveforms are examined in GTKWave to compare signal transitions, output behavior, and timing relationships between the two simulation stages.

A successful functional comparison requires the RTL and gate-level simulations to exhibit the expected corresponding behavior under equivalent inputs and reset conditions. Differences caused by timing models or unknown values should also be considered when interpreting the waveforms.

---

## Results Summary

The BabySoC RTL synthesis and gate-level simulation workflow covered the following activities:

- Loaded the required Verilog RTL source files into Yosys.
- Imported the PLL, DAC, and SKY130 Liberty libraries.
- Synthesized the design using `vsdbabysoc` as the top-level module.
- Examined the module hierarchy and synthesis statistics.
- Performed a structural check that reported **0 problems**.
- Mapped sequential and combinational logic using the SKY130 standard-cell library.
- Inspected the synthesized circuit using Yosys visualization tools.
- Flattened and cleaned the design before exporting the netlist.
- Generated the gate-level Verilog netlist.
- Configured the testbench for post-synthesis simulation.
- Compiled and executed the simulation using Icarus Verilog.
- Opened the generated waveforms in GTKWave.
- Inspected and compared the RTL and post-synthesis signal behavior.

---

## Conclusion

The BabySoC design was synthesized using Yosys and mapped to the SKY130 standard-cell library. Structural verification was performed, and the synthesized circuit was exported as a gate-level Verilog netlist.

Post-synthesis functional simulation was configured using Icarus Verilog and the required functional cell models. GTKWave was then used to inspect the simulation waveforms and compare the behavior of the RTL and synthesized implementations.

This experiment demonstrates the key stages of the digital design implementation workflow, from RTL processing and technology mapping to gate-level netlist generation and functional simulation.

**RTL Design → Synthesis → Technology Mapping → Netlist Generation → Gate-Level Simulation → Waveform Analysis**
