# VSD-RTL-DESIGN-WORKSHOP
# Module 3 – Logic Optimization

This module focuses on the fundamentals and practical implementation of **logic optimization in RTL design**.

The module covers combinational and sequential logic optimization techniques, including gate-level optimization, constant propagation, D flip-flop optimization, and counter optimization. Verilog implementations are simulated and synthesized to analyze the effect of optimization on the resulting hardware.

## Objective

The objective of this module is to understand how RTL optimization can simplify hardware while maintaining the intended functionality, and to observe the resulting changes through simulation and synthesis.

## Tools & Technologies


```bash
iverilog
* yosys synthesizer
gtkwave waveform analyser
```


# MODULE 3 – LOGIC OPTIMIZATION

## Table of Contents

### 1. Fundamentals of Logic Optimization
- [1.1 Introduction to Logic Optimization](#11-introduction-to-logic-optimization)
- [1.2 Objectives of Logic Optimization](#12-objectives-of-logic-optimization)

### 2. Combinational Logic Optimization
- [2.1 AND Gate Optimization — ](#21-and-gate-optimization)
- [2.2 OR Gate Optimization — ](#22-or-gate-optimization)
- [2.3 Three-Input AND Gate Optimization — ](#23-three-input-and-gate-optimization)

### 3. Sequential Logic Optimization
- [3.1 Introduction to Sequential Optimization](#31-introduction-to-sequential-optimization)
- [3.2 D Flip-Flop Constant Propagation](#32-d-flip-flop-constant-propagation)
- [3.3 Verilog Implementation](#33-verilog-implementation)
- [3.4 Simulation of `dff_const1`](#34-simulation-of-dff_const1)
- [3.5 Simulation of `dff_const2`](#35-simulation-of-dff_const2)

### 4. D Flip-Flop Optimization Analysis
- [4.1 D Flip-Flop Netlist Before Optimization](#41-d-flip-flop-netlist-before-optimization)
- [4.2 Optimization Results](#42-optimization-results)
- [4.3 D Flip-Flop Constraint Simulation](#43-d-flip-flop-constraint-simulation)
- [4.4 Synthesized D Flip-Flop Circuit](#44-synthesized-d-flip-flop-circuit)

### 5. Counter Optimization
- [5.1 Counter Optimization](#51-counter-optimization)
- [5.2 Counter Optimization Results](#52-counter-optimization-results)
- [5.3 Optimized Counter Circuit](#53-optimized-counter-circuit)
- [5.4 Optimized Counter Netlist](#54-optimized-counter-netlist)

### 6. Conclusion
- [6.1 Summary](#61-summary)
- [6.2 Conclusion](#62-conclusion)

# 1.1 Introduction to Logic Optimization

## Overview

Logic optimization is the process of simplifying a digital circuit while preserving its original functionality. The main goal is to obtain a more efficient hardware implementation by reducing unnecessary logic and improving the overall design.

In RTL design, optimization can be performed by analyzing the Verilog/SystemVerilog description and identifying redundant logic, constant values, unnecessary gates, and other structures that can be simplified.

## Objectives of Logic Optimization

The major objectives are:

* Reduce hardware area
* Reduce power consumption
* Improve circuit performance
* Reduce unnecessary logic
* Simplify the synthesized circuit
* Maintain the original functionality of the design

## Importance in RTL Design

RTL optimization plays an important role in the synthesis process. A well-optimized RTL description can result in a simpler and more efficient gate-level implementation.

The optimization process can be observed by comparing the **RTL/netlist before optimization** with the **optimized netlist after synthesis**.

## Key Idea

> **Optimize the hardware implementation without changing the intended functionality of the design.**

## Summary

Logic optimization helps transform an RTL design into a more efficient hardware implementation. In this module, different optimization techniques are explored through Verilog examples, simulations, netlist analysis, and synthesis results.
# 1.2 Objectives of Logic Optimization

## Overview

The primary objective of logic optimization is to obtain an efficient hardware implementation while maintaining the original functionality of the RTL design.

### 1. Reduce Hardware Area

Optimization removes redundant gates and simplifies logic structures, resulting in a smaller hardware implementation.

### 2. Reduce Power Consumption

Reducing unnecessary logic and switching activity can help minimize the power consumed by the circuit.

### 3. Improve Performance

Simplifying logic paths can reduce propagation delays and improve the overall timing performance of the design.

### 4. Eliminate Redundant Logic

Logic that does not contribute to the required output can be identified and removed during optimization.

### 5. Simplify Circuit Structure

Complex logic expressions can be transformed into simpler equivalent structures that require fewer hardware resources.

### 6. Preserve Functionality

The optimized circuit must maintain the intended behavior of the original RTL design.

### 7. Improve Resource Utilization

Optimization helps make better use of available hardware resources by avoiding unnecessary logic elements and structures.

## Key Parameters

The effectiveness of optimization is commonly evaluated using:

* **Area** – Hardware resources required by the design
* **Power** – Power consumed during circuit operation
* **Timing** – Speed and delay characteristics of the circuit
* **Logic Complexity** – Number and complexity of logic elements
  # 2.1 Introduction to Combinational Logic Optimization

## Overview
<img width="1812" height="847" alt="combi prop" src="https://github.com/user-attachments/assets/95b510af-1bf0-49db-984f-2d4145acdc71" />

Combinational logic circuits are digital circuits whose outputs depend only on the current inputs. They do not contain memory elements or depend on previous states.

Combinational logic optimization focuses on simplifying these circuits while maintaining the same input-output functionality.

## Common Optimization Techniques

### 1. Constant Propagation

Known constant values such as `0` and `1` can be propagated through the logic to simplify the circuit.

### 2. Boolean Simplification

Boolean expressions can be simplified using Boolean algebra and logical identities.

### 3. Redundant Logic Removal

Unnecessary gates or logic paths that do not affect the output can be removed.

### 4. Logic Sharing

Common logic expressions can be shared instead of implementing the same logic multiple times.

### 5. Gate Reduction

Multiple logic operations can sometimes be replaced by a smaller number of equivalent gates.

## Example

Consider the following logic:

```text
Y = A & 1
```

Since `A AND 1 = A`, the expression can be simplified to:

```text
Y = A
```

The optimized circuit therefore requires less logic while producing the same output.

## RTL Implementation

Combinational logic can be described in Verilog using continuous assignments or combinational procedural blocks.

Example:

```verilog
assign y = a & 1'b1;
```

After optimization, the logic can effectively become:

```verilog
assign y = a;
```

This type of simplification can be observed during synthesis and netlist analysis.
# 2.2 AND Gate Optimization — `opt_check`

## Overview

The `opt_check` example demonstrates optimizing an AND gate implemented in RTL.

An AND gate produces a logic `1` at the output only when all of its inputs are `1`.

## RTL Implementation

The AND gate can be described in Verilog using a continuous assignment:

```verilog
module opt_check (
    input  a,
    input  b,
    output y
);

assign y = a & b;

endmodule
```
## Yosys Commands
```bash
yosys
read_verilog opt_check.v
synth -top opt_check
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

## Truth Table

| A | B | Y = A & B |
| - | - | --------- |
| 0 | 0 | 0         |
| 0 | 1 | 0         |
| 1 | 0 | 0         |
| 1 | 1 | 1         |

## Optimization Analysis

During synthesis, the RTL description is analyzed and converted into an equivalent gate-level implementation.

For a basic AND operation, the synthesized circuit can be represented using an AND gate without introducing unnecessary additional logic.

The optimization process ensures that the generated hardware remains functionally equivalent to the RTL description.

## Simulation

The behavior of the AND gate can be verified by applying different combinations of inputs and observing the output waveform.

## Synthesis Result

You can inspect the synthesized result to determine the logic structure generated from the RTL code.
<img width="1918" height="811" alt="opt_check1" src="https://github.com/user-attachments/assets/54140e07-82c9-4871-b8f0-f78ed4b39d49" />


# 2.3 Three-Input AND Gate Optimization — `opt_check3`

## Overview

The `opt_check3` example demonstrates a three-input AND gate using RTL. The output becomes logic `1` only when all three inputs are logic `1`.

## RTL Implementation

The three-input AND operation can be described in Verilog as:

```verilog
module opt_check3 (
    input  a,
    input  b,
    input  c,
    output y
);

assign y = a & b & c;

endmodule
```
## Yosys Commands
```bash
yosys
read_verilog opt_check3.v
synth -top opt_check3
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

## Truth Table

| A | B | C | Y |
| - | - | - | - |
| 0 | 0 | 0 | 0 |
| 0 | 0 | 1 | 0 |
| 0 | 1 | 0 | 0 |
| 0 | 1 | 1 | 0 |
| 1 | 0 | 0 | 0 |
| 1 | 0 | 1 | 0 |
| 1 | 1 | 0 | 0 |
| 1 | 1 | 1 | 1 |
<img width="1908" height="771" alt="optcheck3 3ip nand" src="https://github.com/user-attachments/assets/9f0c6efd-527d-4115-8be9-d4599cfb8954" />


## Logic Optimization

The expression `y = a & b & c` represents a three-input AND operation. During synthesis, the tool analyzes the expression and generates an equivalent hardware implementation.

The optimization process ensures that the generated circuit implements the required Boolean function without unnecessary logic.

## Functional Behavior

The output remains `0` whenever at least one of the inputs is `0`. The output becomes `1` only when:

```text
A = 1, B = 1, C = 1
```

This makes the circuit useful for conditions where multiple signals must be simultaneously active.

## Key Points

* The circuit is combinational.
* It has three inputs and one output.
* The output is `1` only when all inputs are `1`.
* The RTL directly represents the required Boolean operation.
* Synthesis converts the RTL description into an equivalent hardware implementation.

  # 3.1 Introduction to Sequential Logic Optimization

## Overview

Sequential logic circuits are digital circuits whose outputs depend on both the current inputs and the previous state of the circuit. Unlike combinational circuits, sequential circuits contain memory elements such as flip-flops and registers.

Sequential logic optimization focuses on simplifying the hardware implementation while preserving the required behavior and state transitions.

## Sequential Logic Elements

Common sequential elements include:

* D Flip-Flops
* Registers
* Counters
* Shift Registers
* Finite State Machines

## Optimization Techniques

### 1. Constant Propagation

Known constant values can be propagated through sequential logic to simplify the resulting hardware.

### 2. Redundant Logic Removal

Unnecessary logic that does not affect the state or output can be eliminated.

### 3. Register Optimization

Registers or flip-flops that are unnecessary for the required functionality can potentially be removed or simplified.

### 4. Logic Simplification

Combinational logic connected to sequential elements can be simplified to reduce the overall circuit complexity.

### 5. State Optimization

In larger sequential designs such as finite state machines, equivalent or unnecessary states can be eliminated to reduce hardware requirements.

## Optimization Considerations

Sequential optimization must preserve:

* Correct state transitions
* Clock behavior
* Reset behavior
* Output functionality
* Timing requirements

Unlike simple combinational optimization, sequential optimization must consider the relationship between the current state and future states.
<img width="762" height="408" alt="image" src="https://github.com/user-attachments/assets/63141129-066b-4a29-8538-c3871ceaf650" />

<img width="1912" height="1076" alt="sequential opti" src="https://github.com/user-attachments/assets/bf08b052-2c4a-4d90-8eac-cb2bdc0e6e35" />
Simply:

**Sequential Constant Propagation** means finding a signal in a flip-flop that will always have the same value and replacing it with that constant.

Here:

* `D = 0`
* On the clock edge, the flip-flop stores `0`
* Therefore, `Q = 0`
* Since `Q` is always `0`, the synthesis tool can use `0` directly instead of keeping unnecessary logic.

So:

**D = 0 → Flip-Flop → Q = 0**

The tool recognizes this and **simplifies the circuit**.

## 3.2 D Flip-Flop Constant Propagation

D Flip-Flop Constant Propagation is an optimization technique where the synthesis tool identifies a constant value at the input of a flip-flop.

For example, if:

```text
D = 0
```

then after the clock edge:

```text
Q = 0
```

Since `Q` always becomes `0`, the tool can propagate this constant value to the following logic and simplify the circuit.



## 3.3 Verilog Implementation

The D flip-flop is described in Verilog using a clocked `always` block.

```verilog
always @(posedge clk)
    Q <= D;
```

When `D` is connected to a constant value such as `0`, the synthesis tool can identify that `Q` will also become constant and optimize the circuit.



## 3.4 Simulation of `dff_const1`

The `dff_const1` circuit is simulated to observe the behavior of the D flip-flop.

When the clock edge occurs, the value at `D` is transferred to `Q`. If `D` is fixed at `0`, the output `Q` becomes `0`.

This simulation verifies the constant behavior of the flip-flop.



## 3.5 Simulation of `dff_const2`

The `dff_const2` circuit is used to check the optimized behavior of the D flip-flop.

The simulation shows that the output remains at the expected constant value. This confirms that the synthesis tool can simplify the original sequential logic without changing its required behavior.
<img width="642" height="866" alt="dff_codes sc" src="https://github.com/user-attachments/assets/c0dc1287-2221-4457-846f-f27412a870f8" />

*dff_const1 output waveform

<img width="1920" height="1080" alt="dff1 waveform" src="https://github.com/user-attachments/assets/ef82da9a-8e00-44a5-8052-e76c77199881" />

*dff_const2 output waveform

<img width="1472" height="672" alt="dff2_waveform" src="https://github.com/user-attachments/assets/0ae0905c-0a6a-4151-9020-babca7d0307c" />


## 4.1 D Flip-Flop Netlist Before Optimization

Before optimization, the netlist contains the **D flip-flop and its connected logic**.

The flip-flop stores the input value on the clock edge, and the output `Q` is connected to the next logic.

The circuit still contains the flip-flop even when its output can be determined as a constant.



## 4.2 Optimization Results

After optimization, the synthesis tool identifies that the flip-flop output has a **constant value**.

The unnecessary logic is removed or simplified, reducing the hardware required for the circuit.



## 4.3 D Flip-Flop Constraint Simulation

Constraint simulation checks the circuit behavior under the given conditions.

For a constant input, the simulation confirms that the output of the flip-flop reaches the expected constant value after the clock edge.



## 4.4 Synthesized D Flip-Flop Circuit

The synthesized circuit shows the **optimized hardware structure** after constant propagation.

The resulting circuit is simpler because the synthesis tool removes logic that does not affect the final output.

## Commands
```bash
vi dff_const1.v
vi dff_const2.v
```


*dff1_netlist

<img width="1897" height="527" alt="dff1 netlist" src="https://github.com/user-attachments/assets/918d6c02-eddd-4439-a540-25d436548f04" />
## Commands
```bash
iverilog -o dff_const1.out dff_const1.v tb_dff_const1.v
gtkwave tb_dff_const1.vcd
```

*dff2_netlist

<img width="808" height="683" alt="Screenshot 2026-08-22 221537" src="https://github.com/user-attachments/assets/2441d191-a10b-4745-991d-20d9e8c986bf" />

## Commands
```bash
iverilog -o dff_const2.out dff_const1.v tb_dff_const2.v
gtkwave tb_dff_const2.vcd
## 5.1 Counter Optimization
```

Counter optimization is the process of simplifying a counter circuit by removing unnecessary logic.

The synthesis tool analyzes the counter's behavior and identifies parts that do not affect the required output. These parts can then be simplified or removed.



## 5.2 Counter Optimization Results

After optimization, the counter uses **less unnecessary logic** while maintaining the same required functionality.

The optimized design can have reduced hardware resources and a simpler circuit structure.



## 5.3 Optimized Counter Circuit

The optimized counter contains only the logic required for its counting operation.

Unnecessary gates or logic elements identified during synthesis are removed, making the circuit simpler.



## 5.4 Optimized Counter Netlist

The optimized netlist represents the final hardware structure generated by the synthesis tool.

It shows the reduced logic after optimization while preserving the original counter behavior.
## Commands
```bash
yosys
read_verilog counter_opt.v
synth -top counter_opt
show
```

*counteropt netlist
<img width="890" height="156" alt="counteropt" src="https://github.com/user-attachments/assets/0cd3b98a-a648-47eb-bd9a-9a9088135e77" />

# 6. Conclusion

## 6.1 Summary

Logic optimization simplifies a circuit by removing unnecessary logic and reducing hardware resources. The examples covered combinational logic, D flip-flops, and counters.

## 6.2 Conclusion

The optimization results show that synthesis tools can produce **simpler and more efficient hardware** while maintaining the required functionality of the original design.
