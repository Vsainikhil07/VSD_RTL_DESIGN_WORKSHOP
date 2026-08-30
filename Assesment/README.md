# Assessment - Sequence Detector

## Overview

This assessment demonstrates the complete RTL-to-GLS flow of the given `sequence_detector.v` design.

The design is first analyzed along with its testbench, followed by RTL simulation and waveform analysis. The RTL is then synthesized using Yosys and the synthesized netlist is verified using Gate-Level Simulation (GLS). Finally, the RTL and GLS waveforms are compared to verify functional equivalence.

---

## Table of Contents

| No. | Topic                   |
| --- | ----------------------- |
| 1   | RTL Code Analysis       |
| 2   | Testbench Code Analysis |
| 3   | RTL Simulation          |
| 4   | RTL Waveform            |
| 5   | Synthesis               |
| 6   | Synthesized Netlist     |
| 7   | Gate-Level Simulation   |
| 8   | GLS Waveform            |
| 9   | RTL vs GLS Comparison   |
| 10  | Overall Result          |
| 11  | Conclusion              |

---

# RTL Code Analysis

The `sequence_detector` module implements a sequence detector for the target sequence `0001010`.

* The design has `clk`, `reset`, and `din` as inputs and `detected` as the output.
* A 3-bit `state` register is used to represent 7 FSM states.
* `next_state` and `next_detected` are calculated using combinational logic.
* The FSM transitions depend on the current state and input `din`.
* The target sequence is detected when the FSM reaches the final matching state.
* The `detected` output is registered and updated on the positive edge of `clk`.
* The reset is active-high and synchronously resets the state and detection output to `0`.

---

# Testbench Code Analysis

The testbench verifies the functionality of the sequence detector by generating the clock, reset, and input stimulus.

* The clock is generated using `always #5 clk = ~clk`, giving a **10 ns clock period**.
* Reset is initially asserted for **4 clock cycles** and then released.
* The `drive_bit` task applies each input bit on the negative edge of the clock.
* The input is sampled by the design on the following positive edge.
* The testbench generates `dump.vcd` for waveform analysis using GTKWave.
* A `detection_count` variable counts the number of detected sequences.
* A final reset is applied for **2 clock cycles** before simulation ends.
* The testbench displays the final number of detected sequences.

---

# RTL Simulation

RTL simulation is performed to verify the functional behavior of the original RTL design before synthesis.

## Compile RTL

```bash
iverilog -o rtl_sim sequence_detector.v tb.v
```

## Run Simulation

```bash
vvp rtl_sim
```

The simulation displays the input and detection output at each clock cycle.

The testbench also prints the final detection count:

```text
FINAL_DETECTION_COUNT=4
```

## Open RTL Waveform

```bash
gtkwave dump.vcd
```

---

## RTL Simulation Waveform
<img width="1918" height="1073" alt="dumpvcd waveform" src="https://github.com/user-attachments/assets/44237a78-a7f5-4d12-9e62-b6a4b09376e4" />

## RTL Waveform Analysis

The RTL waveform confirms correct FSM state transitions and proper generation of the `detected` output.

- The clock has a 10 ns period and the input `din` is applied synchronously.
- The `state[2:0]` changes according to the input sequence.
- The `detected` signal goes high when the target sequence `0001010` is detected.

# Synthesis

After verifying the RTL functionality, the design is synthesized using Yosys with the SKY130 standard-cell library.

## Start Yosys

```bash
yosys
```

## Read Standard Cell Library

```yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

## Read RTL Design

```yosys
read_verilog sequence_detector.v
```

## Technology Mapping

```yosys
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```


## Generate Synthesized Netlist

```yosys
write_verilog -noattr synthesized.v
```

## Exit Yosys

```yosys
exit
```

---

# Synthesized Netlist

The RTL design is converted into a gate-level netlist using the SKY130 standard-cell library.

The synthesized netlist is generated as:

```text
synthesized.v
```

## Synthesis Result
<img width="817" height="567" alt="synthesise_netlist" src="https://github.com/user-attachments/assets/d31b4a1f-960e-407f-b6f5-b027348c5b64" />


<img width="720" height="726" alt="Screenshot 2026-08-29 113310" src="https://github.com/user-attachments/assets/663588c4-6dd9-4598-99bb-34f183df9f4f" />

---

# Gate-Level Simulation (GLS)

Gate-Level Simulation is performed using the synthesized netlist to verify that the synthesized design maintains the functionality of the RTL design.

The same testbench and input stimulus are used for GLS.

## Compile GLS

```bash
iverilog -o gls_sim synthesized.v tb.v
```

If the SKY130 standard-cell model is required:

```bash
iverilog -o gls_sim synthesized.v ../lib/sky130_fd_sc_hd.v tb.v
```

## Run GLS

```bash
vvp gls_sim
```

## Open GLS Waveform

```bash
gtkwave dump.vcd
```

The GLS simulation is expected to produce the same functional detection count:

<img width="889" height="752" alt="Screenshot 2026-08-29 115231" src="https://github.com/user-attachments/assets/9c92c4cd-e4f2-4df8-9fc6-c2fc6d79e392" />


```text
FINAL_DETECTION_COUNT=4
```

---

# GLS Waveform

## Gate-Level Simulation Waveform
<img width="1918" height="1078" alt="gls-original" src="https://github.com/user-attachments/assets/558e01e3-7861-4a0c-ae7e-1722cba41822" />



### Observation

- The GLS waveform shows the synthesized gate-level signals responding correctly to the applied input sequence.
- The `detected` output generates detection pulses at the expected positions.
- The waveform confirms that the synthesized design maintains the expected functional behavior.

---

# RTL vs GLS Comparison

The RTL and GLS simulations are compared using the same testbench and input stimulus.

| Parameter         | RTL         | GLS                 |
| ----------------- | ----------- | ------------------- |
| Design            | RTL Design  | Synthesized Netlist |
| Clock Period      | 10 ns       | 10 ns               |
| Reset             | Active-high | Active-high         |
| Input Stimulus    | Same        | Same                |
| Target Sequence   | `0001010`   | `0001010`           |
| Detection Count   | 4           | 4                   |
| Output Behavior   | Correct     | Matches RTL         |
| Functional Result | Correct     | Preserved           |

## RTL Waveform
<img width="1918" height="1073" alt="dumpvcd waveform" src="https://github.com/user-attachments/assets/44237a78-a7f5-4d12-9e62-b6a4b09376e4" />



## GLS Waveform

<img width="1918" height="1078" alt="gls-original" src="https://github.com/user-attachments/assets/558e01e3-7861-4a0c-ae7e-1722cba41822" />


### RTL vs GLS Waveform Comparison

- The RTL and GLS waveforms show the same functional response for the applied input sequence.
- The `detected` output occurs at the expected detection points in both simulations.
- This confirms that the synthesized gate-level design preserves the functionality of the RTL design.
---

# Timing Observation

The RTL simulation represents the logical behavior of the design, while GLS represents the synthesized gate-level implementation.

For a zero-delay GLS simulation, the RTL and GLS waveforms may appear nearly identical. If timing information is included, small propagation delays may be observed in the GLS waveform.

---

# Overall Result

The complete RTL-to-GLS flow was successfully performed:

```text
RTL Code Analysis
        |
        v
Testbench Analysis
        |
        v
RTL Simulation
        |
        v
RTL Waveform
        |
        v
Synthesis using Yosys
        |
        v
Synthesized Netlist
        |
        v
Gate-Level Simulation
        |
        v
GLS Waveform
        |
        v
RTL vs GLS Comparison
```

The target sequence `0001010` is detected four times in the given testbench input stream.

The expected final detection count is:

```text
FINAL_DETECTION_COUNT=4
```

---

# Conclusion

The sequence detector was successfully analyzed, simulated at RTL level, synthesized using Yosys, and verified through Gate-Level Simulation.  
The RTL and GLS waveforms show matching functional behavior for the given input sequence and confirm correct sequence detection.  
Overall, the assessment verifies that the synthesized gate-level implementation preserves the functional behavior of the original RTL design.
