
# Module 2 — Timing Libraries, Synthesis and Flip-Flop RTL

## 1. Overview

This module explores the relationship between standard-cell libraries, sequential RTL design, functional verification, and synthesis. The practical work uses the SKY130 technology library and Yosys to examine how Verilog descriptions are processed into hardware representations.

The exercises include Liberty-file inspection, hierarchical and flattened synthesis, D flip-flop implementations, waveform verification, and constant-arithmetic optimization.

## 2. Learning Objectives

- Explore the SKY130 standard-cell library.
- Understand timing and operating-condition information in Liberty files.
- Compare hierarchical and flattened synthesis.
- Implement D flip-flops with different reset and set mechanisms.
- Verify sequential logic using Icarus Verilog.
- Inspect waveforms using GTKWave.
- Process RTL designs using Yosys.
- Examine synthesis optimization and generated netlists.

## 3. Tools and Technologies

| Purpose | Tool or Technology |
|---|---|
| Hardware description | Verilog HDL |
| RTL simulation | Icarus Verilog |
| Waveform analysis | GTKWave |
| Logic synthesis | Yosys |
| Standard-cell library | SKY130 PDK |

## 4. Exploring the SKY130 Technology Library

### 4.1 Liberty File Overview

The SKY130 PDK provides technology information used during digital design and implementation. Liberty files describe the characteristics of standard cells so that synthesis and timing tools can evaluate their behavior.

The library examined during this exercise was:

```text
sky130_fd_sc_hd__tt_025C_1v80.lib
```

The filename identifies a particular library configuration, process corner, temperature, and supply voltage.

### 4.2 Information Available in Liberty Files

A Liberty description can contain:

- Standard-cell names and definitions
- Input and output pin information
- Logical functions
- Timing arcs and delay characteristics
- Power-related information
- Operating-condition details

![SKY130 Liberty library inspection](https://github.com/user-attachments/assets/2937c979-4190-4d1a-81e0-56bd3af355ae?raw=true)

**Observation:** Examining the library helped me understand how standard-cell characteristics are represented for synthesis and timing analysis.

## 5. Understanding RTL Hierarchy

RTL designs may consist of several interconnected modules. Synthesis can preserve these module boundaries or combine them into a flattened representation.

### 5.1 Hierarchical Synthesis

Hierarchical synthesis retains the organization of the original RTL design. Individual modules remain identifiable, making the structure easier to inspect.

![Hierarchical synthesis representation](https://github.com/user-attachments/assets/95d6c958-eb6a-42d2-9614-d3da4ec6ac54?raw=true)

**Observation:** The design hierarchy remained visible, allowing the individual blocks and their relationships to be examined.

### 5.2 Flattening the Design

Flattening removes module boundaries and combines the design into a unified representation.

The following Yosys command was used:

```tcl
flatten
```

![Flattened design representation](https://github.com/user-attachments/assets/088074be-4bbf-4728-ae59-811c084697db?raw=true)

**Observation:** The module boundaries were removed, producing a combined representation that can allow optimization across module boundaries.

### 5.3 Comparison

| Hierarchical Synthesis | Flattened Synthesis |
|---|---|
| Preserves module boundaries | Removes module boundaries |
| Retains the original organization | Combines the design structure |
| Helps inspect individual blocks | Can enable cross-module optimization |
| Maintains a structured representation | Produces a unified representation |

Both approaches can represent the same intended logic, although the internal organization of the synthesized design may differ.

## 6. D Flip-Flop Implementations

A D flip-flop stores one bit of data and updates its output according to a clock signal. Reset and set inputs determine how the stored value is controlled.

Three implementations were explored in this module.

### 6.1 D Flip-Flop with Asynchronous Reset

An asynchronous reset can clear the output immediately when the reset signal is asserted, without waiting for a clock edge.

**Verilog implementation:**

```verilog
module dff_asyncres (
    input clk,
    input async_reset,
    input d,
    output reg q
);

always @(posedge clk or posedge async_reset)
    if (async_reset)
        q <= 1'b0;
    else
        q <= d;

endmodule
```

When `async_reset` is high, `q` becomes `0`. Otherwise, the input `d` is captured on the rising edge of `clk`.

**Simulation commands:**

```bash
iverilog dff_asyncres.v tb_dff_asyncres.v
./a.out
gtkwave tb_dff_asyncres.vcd
```

![Asynchronous-reset D flip-flop waveform](https://github.com/user-attachments/assets/5e706d09-6a00-42f9-8b13-e0523d326a06?raw=true)

**Result:** The waveform was used to verify the asynchronous reset response and normal clocked data capture.

### 6.2 D Flip-Flop with Asynchronous Set

An asynchronous set forces the output to logic `1` when the set signal is asserted.

**Verilog implementation:**

```verilog
module dff_async_set (
    input clk,
    input async_set,
    input d,
    output reg q
);

always @(posedge clk or posedge async_set)
    if (async_set)
        q <= 1'b1;
    else
        q <= d;

endmodule
```

When `async_set` is high, `q` becomes `1`. When the set signal is inactive, `q` captures `d` at the rising clock edge.

**Simulation commands:**

```bash
iverilog dff_async_set.v tb_dff_async_set.v
./a.out
gtkwave tb_dff_async_set.vcd
```

![Asynchronous-set D flip-flop waveform](https://github.com/user-attachments/assets/3c16e086-d5ab-41e6-9c58-ae7373f6668d?raw=true)

**Result:** The waveform was used to examine the response to the asynchronous set input and the clocked data input.

### 6.3 D Flip-Flop with Synchronous Reset

A synchronous reset is evaluated only at the active clock edge. Changing the reset signal between clock edges does not immediately change the output.

**Verilog implementation:**

```verilog
module dff_syncres (
    input clk,
    input sync_reset,
    input d,
    output reg q
);

always @(posedge clk)
    if (sync_reset)
        q <= 1'b0;
    else
        q <= d;

endmodule
```

At each rising clock edge:

- If `sync_reset` is high, `q` becomes `0`.
- If `sync_reset` is low, `q` captures `d`.
- A reset transition alone does not directly update `q`.

**Simulation commands:**

```bash
iverilog dff_syncres.v tb_dff_syncres.v
./a.out
gtkwave tb_dff_syncres.vcd
```

![Synchronous-reset D flip-flop waveform](https://github.com/user-attachments/assets/970f7a3a-2f40-42de-a302-7332f7a5e60c?raw=true)

**Result:** The waveform was used to check that the reset operation occurred at the active clock edge.

## 7. RTL Verification and Synthesis

The RTL designs were simulated before synthesis to check their functional behavior. The following sequence summarizes the workflow:

```text
Verilog RTL
    |
    v
Icarus Verilog
    |
    v
Functional Simulation
    |
    v
VCD Waveform
    |
    v
GTKWave Inspection
    |
    v
Yosys Synthesis
    |
    v
Gate-Level Representation
```

### 7.1 Inspecting Simulation Waveforms

The clock, data input, control input, and output signals were examined to understand the behavior of the sequential circuits.

![RTL simulation waveform](https://github.com/user-attachments/assets/f525c76e-b3e4-4599-b3fd-b27441d87ef0?raw=true)

![Additional waveform analysis](https://github.com/user-attachments/assets/9ada128c-22f9-4cb3-b258-0b570dd4364d?raw=true)

**Observation:** Waveform inspection provides a visual method for checking the behavior of sequential RTL over simulation time.

### 7.2 Processing RTL with Yosys

Yosys was used to process the asynchronous-reset flip-flop design.

Start the tool:

```bash
yosys
```

Read the RTL source:

```tcl
read_verilog dff_asyncres.v
```

Select the top-level module:

```tcl
hierarchy -top dff_asyncres
```

Convert procedural RTL into internal logic:

```tcl
proc
```

Optimize the design:

```tcl
opt
```

Perform technology-independent mapping and optimization:

```tcl
techmap
opt
```

![Yosys synthesis processing](https://github.com/user-attachments/assets/0506bff6-1f4c-4697-9fba-08bfa8d441ca?raw=true)

**Observation:** These commands process the RTL into a lower-level representation. Actual mapping to SKY130 standard cells requires an appropriate technology-mapping flow and compatible library.

## 8. Synthesis Optimization of Constant Multiplication

Synthesis tools can simplify arithmetic expressions when the operation can be represented using a more efficient logic structure.

### 8.1 Multiplication by Two

The first example multiplies a three-bit input by the constant `2`.

**Verilog implementation:**

```verilog
module mul2 (
    input [2:0] a,
    output [3:0] y
);

assign y = a * 2;

endmodule
```

Multiplication by two is equivalent to shifting the binary value one position to the left. Therefore, a general-purpose multiplier is not required for this constant operation.

**Yosys commands:**

```tcl
read_verilog mul2.v
prep -top mul2
show
write_verilog -noattr mul2_net.v
```

To map the design to a target standard-cell library, use the correct Liberty file path and a compatible technology-mapping command.

![Multiplication-by-two synthesis](https://github.com/user-attachments/assets/25cd0c58-625b-43fa-8a3a-16cbfb435124?raw=true)

**Result:** The synthesized representation can be inspected to understand how the constant multiplication is implemented.

### 8.2 Multiplication by Eight

The second example multiplies the input by the constant `8`.

**Verilog implementation:**

```verilog
module mult8 (
    input [2:0] a,
    output [5:0] y
);

assign y = a * 8;

endmodule
```

Multiplication by eight is equivalent to shifting the binary value three positions to the left.

**Yosys commands:**

```tcl
read_verilog mult8.v
prep -top mult8
show
write_verilog -noattr mult8_net.v
```

![Multiplication-by-eight synthesis](https://github.com/user-attachments/assets/29c5e5ae-702d-4c94-9d18-a5fbebbe691e?raw=true)

**Result:** The generated representation can be examined to see how the synthesis tool simplifies the constant arithmetic operation.

### 8.3 Examining the Generated Netlists

The synthesized Verilog files can be opened for inspection:

```bash
gvim mul2_net.v
gvim mult8_net.v
```

These files provide a lower-level representation of the circuits after synthesis. Comparing the generated netlists with the original RTL helps demonstrate the transformation from behavioral expressions to hardware logic.

## 9. Key Takeaways

- Examined the SKY130 standard-cell Liberty library.
- Learned about process corners and operating conditions.
- Compared hierarchical and flattened design representations.
- Implemented asynchronous-reset and asynchronous-set flip-flops.
- Implemented a synchronous-reset flip-flop.
- Used Icarus Verilog for RTL simulation.
- Inspected signal waveforms using GTKWave.
- Processed Verilog RTL using Yosys.
- Explored constant-arithmetic optimization.
- Generated and examined synthesized Verilog representations.

## 10. Conclusion

This module provided practical exposure to standard-cell libraries, sequential RTL design, simulation, synthesis, and logic optimization.

The flip-flop exercises demonstrated the differences between synchronous and asynchronous control signals. The synthesis exercises showed how RTL can be transformed into a hardware-oriented representation and how constant arithmetic operations can be simplified.

Overall, the module strengthened my understanding of the RTL design flow and the role of open-source EDA tools in digital hardware development.

## Author

**Vadla Sai Nikhil**

B.Tech – Electronics and Communication Engineering (ECE)  
Anurag University

**RTL Design Workshop Repository**

