# MODULE 5 – OPTIMIZATION IN SYNTHESIS

## Overview

This module focuses on understanding how RTL coding styles influence synthesis results and the hardware generated from Verilog designs. The experiments explore incomplete conditional statements, `IF-ELSE` constructs, different `CASE` statement implementations, and combinational logic structures.

Through RTL schematics, simulations, and synthesis analysis, the module demonstrates how incomplete or improper assignments can lead to unintended hardware such as inferred latches, while complete and properly structured RTL code allows synthesis tools to generate the intended combinational logic efficiently.

The module also covers the implementation and analysis of multiplexers, demultiplexers, and ripple carry adders to understand how different RTL descriptions are translated into hardware structures.

# Understanding `CASE` Statements

The `CASE` statement is an important decision-making construct in Verilog used to select a particular operation or output based on the value of a given expression. It is commonly used in multiplexers, decoders, control logic, and other combinational circuits.

In synthesis, the way a `CASE` statement is written directly influences the hardware inferred by the synthesis tool. A complete `CASE` statement provides defined output values for all required conditions, whereas incomplete assignments may result in unintended latch inference.

Different forms of `CASE` coding are explored in this module to understand how complete, incomplete, partial, and improper assignments affect RTL schematics, simulation behavior, and synthesized hardware.
# `for` Loop and `for` Generate Loop

## `for` Loop

A `for` loop is used to **repeat a set of procedural statements** multiple times. It is mainly used inside procedural blocks such as `always` or `initial`.

## `for` Generate Loop

A `for` generate loop is used to **create multiple hardware instances or repeated structures during elaboration**. It is commonly used when the same hardware logic needs to be generated multiple times.

## Simple Comparison

| `for` Loop | `for` Generate Loop |
|---|---|
| Repeats procedural statements | Generates repeated hardware structures |
| Used inside procedural blocks | Used at module/generate level |
| Executes during simulation | Creates hardware during elaboration |
| Useful for repeated operations | Useful for scalable hardware generation |

### Key Points

- `CASE` statements provide structured multi-way decision logic.
- They are widely used in combinational RTL design.
- Complete assignments help prevent unintended latch inference.
- Incomplete assignments can cause storage elements to be inferred.
- Proper `CASE` coding helps achieve predictable and synthesizable hardware.
## Table of Contents

### 1. Incomplete Conditional Statements
- 1.1 Incomplete `if` Statement
- 1.2 RTL Simulation and Waveform Analysis
- 1.3 Incomplete `if2` Statement

### 2. Case Statement Analysis
- 2.1 Incomplete `case` Statement
- 2.2 Complete `case` Statement
- 2.3 Partial Case Assignment
- 2.4 Improper Case Assignment and Its Effects

### 3. Combinational Circuit Optimization
- 3.1 Multiplexer Implementation and Optimization
- 3.2 Demultiplexer Design Analysis
- 3.3 Ripple Carry Adder Implementation

### 4. Overall Results
- 4.1 Synthesis and Optimization Results
- 4.2 RTL and Synthesized Circuit Comparison

### 5. Conclusion

# 1.Incomplete Conditional Statements

### Introduction

Conditional statements are widely used in RTL design to describe decision-making logic. The way these statements are written directly affects how synthesis tools interpret the intended hardware. In particular, incomplete assignments inside `IF` or `IF-ELSE` statements may cause synthesis tools to infer storage elements such as latches.

This section examines incomplete conditional statements through RTL schematics and simulations. The results help demonstrate the relationship between Verilog coding style, simulation behavior, and the resulting synthesized hardware. By comparing incomplete and complete conditional descriptions, the importance of assigning outputs under all required conditions becomes clear.

## 1.1 Incomplete `if` Statement

An incomplete `if` statement occurs when an output is assigned only for a specific condition, without defining its behavior for all possible input conditions. During synthesis, this may result in the inference of a latch to retain the previous output value.

This experiment demonstrates how incomplete RTL assignments affect the synthesized hardware and highlights the importance of complete conditional assignments when designing combinational logic.
## 1.2 RTL Simulation and Waveform Analysis

The RTL simulation is performed to observe the behavior of the incomplete `IF` statement for different input conditions. The waveform helps identify how the output responds when the specified condition is true and how it retains its previous value when the condition is not satisfied.


### OBSERVATION:
<img width="965" height="618" alt="Screenshot 2026-08-24 010840" src="https://github.com/user-attachments/assets/a260e402-d80c-4dad-bbfd-98961cdda6d8" />
<img width="610" height="396" alt="Screenshot 2026-08-24 011241" src="https://github.com/user-attachments/assets/febc140e-a45c-4aca-8413-44db8ed73d93" />

* The RTL schematic shows that the incomplete `IF` statement causes the synthesis tool to infer a latch. Since the output is not assigned when the `IF` condition is false, storage logic is required to hold the previous output value.

The waveform shows that the output changes when the `IF` condition is satisfied, but retains its previous value when the condition becomes false. This behavior indicates latch inference due to the incomplete assignment.

## 1.3 Incomplete `IF` Statement – `incompif2`

The `incompif2` design demonstrates another form of an incomplete `IF` statement where the output is not assigned for all possible input conditions. This allows the synthesis tool to infer storage behavior for the unassigned condition.

### OBSERVATION:
<img width="949" height="616" alt="Screenshot 2026-08-24 011643" src="https://github.com/user-attachments/assets/998c0be9-61e4-4389-94ef-b9dba1933c97" />
<img width="968" height="736" alt="Screenshot 2026-08-24 011942" src="https://github.com/user-attachments/assets/99c46f6f-7e82-416e-90ea-544d09f296e2" />

* The RTL schematic shows that the incomplete assignment results in latch inference. Since the output is not assigned when the specified condition is not satisfied, the inferred latch holds the previous output value.
* The RTL analysis shows that the incomplete assignment causes latch inference, as the output must retain its previous value when the specified condition is not satisfied.
# 2. Case Statement Analysis

## 2.1 Incomplete `CASE` Statement

An incomplete `CASE` statement occurs when all possible input conditions are not explicitly handled. When an output is left unassigned for one or more conditions, the synthesis tool may infer a latch to maintain the previous output value.
 ### Different Case Statement Codes:
 <img width="952" height="856" alt="Screenshot 2026-08-24 171118" src="https://github.com/user-attachments/assets/354b125f-26e4-4819-a786-41c34f039515" />

### OBSERVATION:
<img width="938" height="570" alt="Screenshot 2026-08-24 171506" src="https://github.com/user-attachments/assets/60162f5f-b422-4145-91ed-46641c49f9e8" />
<img width="1081" height="462" alt="Screenshot 2026-08-24 171812" src="https://github.com/user-attachments/assets/d782f798-5bbf-4b3e-b550-693e91f9f0f4" />

The RTL schematic shows that the incomplete `CASE` statement leads to latch inference because the output is not assigned for every possible input combination. The inferred latch allows the output to retain its previous value for the unassigned conditions.
The RTL analysis shows that the incomplete `CASE` statement can result in latch inference because the output is not assigned for every possible input combination.
## 2.2 Complete `CASE` Statement

A complete `CASE` statement provides an output assignment for every possible input condition. This ensures that the combinational logic has a defined output for all input combinations and prevents unintended storage elements from being inferred during synthesis.

### Observation:
<img width="1035" height="685" alt="Screenshot 2026-08-24 171946" src="https://github.com/user-attachments/assets/8f6ddd0a-280e-4f52-bf84-b4fe097ca804" />
<img width="1919" height="817" alt="Screenshot 2026-08-24 172115" src="https://github.com/user-attachments/assets/12f4e88b-4da2-4aaa-8929-544249f94f15" />

* The RTL schematic shows well-defined combinational logic without any latch inference. Since the output is assigned for all possible conditions in the complete `CASE` statement, the synthesis tool does not need to infer storage elements.
* The RTL analysis shows that the complete `CASE` statement produces well-defined combinational logic without latch inference, as the output is assigned for all possible conditions.
## 2.3 Partial Case Assignment

A partial `CASE` assignment occurs when only some output signals are assigned within the different `CASE` branches, while other signals remain unassigned for certain conditions. This coding style can lead to unintended storage behavior during synthesis.

### OBSERVATION:
<img width="607" height="491" alt="Screenshot 2026-08-24 172610" src="https://github.com/user-attachments/assets/c8808f61-5b0c-422f-9f32-1b46ac351ce9" />
* The RTL schematic shows that the partially assigned `CASE` statement results in additional storage logic for the output that is not assigned in every case branch. This indicates that the synthesis tool has inferred latch behavior from the incomplete assignment.

## 2.4 Improper Case Assignment and Its Effects

An improper `CASE` assignment occurs when the output logic is not completely defined across all possible case conditions. Such coding can cause the synthesis tool to infer unintended hardware instead of the expected combinational logic.

### Observation:
<img width="1913" height="599" alt="Screenshot 2026-08-24 172953" src="https://github.com/user-attachments/assets/3f1b4253-a8da-47af-be4b-19a35c84fe29" />

The RTL simulation waveform shows that the output retains its previous value when the corresponding case condition does not provide a new assignment. This confirms the storage behavior introduced by the incomplete `CASE` assignment.
## 3.1 Multiplexer Implementation and Optimization

The `mux_generate.v` design implements a multiplexer using a **`for` loop** in the RTL code. The loop is used to describe the repeated selection logic for multiple input signals, making the code compact and scalable.

### Code Analysis:
<img width="949" height="410" alt="Screenshot 2026-08-24 174101" src="https://github.com/user-attachments/assets/f11457e7-5507-4079-969d-33c8de43e1e3" />

* The `for` loop iterates through the input signals and generates the required selection logic based on the select signal. This approach avoids writing separate statements for each input and provides a structured way to describe a larger multiplexer.

### Waveform Observation:
<img width="1020" height="533" alt="Screenshot 2026-08-24 174508" src="https://github.com/user-attachments/assets/30dbef03-c81c-48ee-8d62-2362f642e14b" />

* The simulation waveform confirms that the output follows the input selected by the corresponding select signal, demonstrating the correct functional behavior of the generated multiplexer.

## 3.2 Demultiplexer Design Analysis

Two different RTL coding approaches are used to implement the demultiplexer: **`demux_case`** and **`demux_using_for_generate`**. Both designs perform the same basic function, but the way the hardware is described in Verilog is different.

### 3.2.1 `demux using case`

The `demux_case` implementation uses a `CASE` statement to select which output receives the input signal. Each select value corresponds to a particular output line.

#### Code Analysis:
<img width="1065" height="812" alt="Screenshot 2026-08-24 174748" src="https://github.com/user-attachments/assets/63b8a5ef-7a61-4e60-8d46-a68871385a57" />

* The `CASE` statement provides a direct and easy-to-understand description of the demultiplexer. Each case condition explicitly defines the selected output, making the RTL code simple to read and debug.

#### Waveform Observation:
<img width="1057" height="708" alt="Screenshot 2026-08-24 175140" src="https://github.com/user-attachments/assets/308fd54e-0798-4197-942f-f219e150fc1b" />

* The waveform shows that the input signal is transferred to the selected output, while the remaining outputs stay inactive for each select condition.

### 3.2.2 `demux_using_for_generate loop`

The `demux_using_for_generate` implementation uses a **`for` generate loop** to create multiple instances of similar selection logic. This approach is useful when the same hardware structure needs to be repeated for several output lines.

### `for` Generate Loop – Functional Analysis

The `for` generate loop creates the required demultiplexer logic for each output line by repeatedly applying the same selection condition. During operation, the select input determines which generated logic path becomes active.

### Waveform Observation:
<img width="1065" height="765" alt="Screenshot 2026-08-24 175415" src="https://github.com/user-attachments/assets/6bee322c-0137-42c0-96a0-4ac1cff33a4f" />

* The waveform shows that for each select combination, the input signal is routed to the corresponding output generated by the loop, while the other outputs remain inactive. As the select input changes, the active output changes accordingly. This demonstrates that the `for` generate loop successfully creates multiple instances of the same selection logic and provides the expected demultiplexer functionality.

## 3.3 Ripple Carry Adder Implementation

A ripple carry adder (`rca`) is a multi-bit binary adder formed by connecting multiple **full adder (`fa`)** modules in sequence. Each `fa` performs the addition of two input bits along with the carry received from the previous stage.

### Code Analysis:
<img width="1046" height="723" alt="Screenshot 2026-08-24 175652" src="https://github.com/user-attachments/assets/98e9796e-56e0-4778-b852-c56ccd63419b" />

* The design consists of a separate **`fa` module**, which acts as the basic building block of the `rca`. The `fa` module generates two outputs for each stage: the **sum** and the **carry**.

* The `rca` module uses the previously defined `fa` module through **module instantiation**. Multiple instances of the `fa` are created inside the `rca`, with the carry output of one `fa` connected to the carry input of the next `fa`.

* This creates a carry chain in which the carry generated by one stage is passed to the next stage and continues through the remaining stages. Thus, the individual `fa` modules are combined to form the complete multi-bit `rca`.

### Waveform Observation
<img width="1012" height="496" alt="Screenshot 2026-08-24 180212" src="https://github.com/user-attachments/assets/d798e2c6-d0aa-4521-b508-7845197e2612" />

* The waveform shows the sum outputs changing according to the input combinations, while the carry signal propagates from one `fa` stage to the next. The observed outputs verify the correct binary addition functionality of the `rca`.

# 4. Overall Results

## 4.1 Synthesis and Optimization Results

* The experiments in this module demonstrate how different RTL coding styles are interpreted during synthesis. Incomplete `IF` and `CASE` statements can lead to unintended latch inference, while complete assignments result in proper combinational logic.

* The multiplexer, demultiplexer, and ripple carry adder experiments further demonstrate how structured RTL descriptions are converted into corresponding hardware. Different coding approaches, such as `CASE` statements and `for` generate loops, can describe similar functionality while providing different levels of code compactness and scalability.

### Overall Observation

RTL schematics and simulation waveforms confirm that the synthesized hardware depends strongly on how the Verilog code is written. Complete and well-structured RTL coding helps achieve the intended combinational hardware and avoids unnecessary storage elements.

## 4.2 RTL and Synthesized Circuit Comparison

The comparison between RTL descriptions, simulated behavior, and synthesized schematics shows that the synthesis tool translates the written RTL code into hardware according to the specified conditions and assignments. The experiments highlight the importance of understanding synthesis behavior while writing efficient and predictable RTL designs.

# 5. Conclusion

This module provided a comprehensive understanding of how RTL coding styles influence synthesis, hardware inference, and the final circuit structure. The experiments demonstrated that seemingly small differences in Verilog coding, such as incomplete assignments, `IF` conditions, and `CASE` statements, can significantly affect the hardware generated by the synthesis tool.

The analysis of incomplete conditional and case statements highlighted the importance of assigning outputs for all required conditions to avoid unintended latch inference. The multiplexer and demultiplexer experiments demonstrated different approaches to describing repeated combinational logic using `CASE` statements and `for` generate loops. The ripple carry adder further introduced hierarchical RTL design, where multiple `fa` modules are instantiated and interconnected to construct a complete `rca`.

By comparing RTL code, simulation waveforms, and synthesized schematics, the relationship between behavioral descriptions and actual hardware implementation became clear. Overall, the experiments emphasized the importance of writing clear, complete, scalable, and synthesis-friendly RTL code for developing reliable digital circuits and achieving the intended hardware structure.
