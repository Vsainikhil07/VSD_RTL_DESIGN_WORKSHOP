# Module 1 — Introduction to ASIC Physical Design

## Overview

This module introduces the **ASIC Physical Design flow** and the process of converting a synthesized digital design into a physical chip layout.

The practical work uses **OpenLane**, **OpenROAD**, and the **SKY130 PDK** to understand the major stages of physical implementation.

The module covers:

- ASIC Physical Design Flow
- Floorplanning
- Power Planning
- Placement
- Clock Tree Synthesis
- Routing
- Antenna Checking
- Static Timing Analysis
- Parasitic Extraction
- DRC and LVS
- OpenLane and SKY130
- Physical Design Results
- Design-Space Exploration

**Image: ASIC Physical Design Flow**

> Add a clean ASIC physical-design flow diagram here.

---

## Objectives

The main objectives of this module are:

- Understand the complete ASIC Physical Design flow.
- Learn the purpose of each physical-design stage.
- Understand floorplanning and core utilization.
- Study power distribution and standard-cell placement.
- Understand Clock Tree Synthesis and routing.
- Perform timing and physical verification checks.
- Gain practical exposure to OpenLane and SKY130.
- Analyze how physical-design parameters affect implementation results.

---

## ASIC Physical Design Flow

The physical implementation process follows a sequence of stages:

```text
RTL Design
    ↓
Synthesis
    ↓
Floorplanning
    ↓
Power Planning
    ↓
Placement
    ↓
Clock Tree Synthesis
    ↓
Routing
    ↓
Parasitic Extraction
    ↓
Static Timing Analysis
    ↓
Antenna / DRC / LVS
    ↓
Final Layout
```

Each stage contributes to converting the logical design into a physically valid and manufacturable layout.

**Image: Physical Design Flow**

> Add physical-design flow screenshot or diagram here.

---

## Floorplanning

Floorplanning is the first major physical-design stage.

It defines the physical organization of the chip, including the core area, die area, aspect ratio, utilization, I/O locations, and space required for power distribution.

### Important Parameters

| Parameter | Purpose |
|---|---|
| Core Utilization | Controls the percentage of core area occupied by cells |
| Aspect Ratio | Defines the relationship between core width and height |
| Core Area | Defines the available placement region |
| I/O Placement | Determines the location of input and output pins |
| Power Planning | Provides the required power distribution structure |

Proper floorplanning helps achieve good **area, timing, power, and routability**.

**Image: Floorplan**

> Add floorplan screenshot here.

---

## Power Planning

Power planning creates a reliable network for distributing power and ground throughout the design.

The Power Distribution Network (PDN) connects the power sources to standard cells and helps maintain stable power delivery.

### Main Elements

- Power rings
- Power straps
- Standard-cell power connections
- VDD and VSS distribution

**Image: Power Distribution Network**

> Add PDN / power-grid screenshot here.

---

## Placement

Placement determines the physical locations of standard cells inside the core area.

The placement process generally includes:

1. Global placement
2. Placement optimization
3. Detailed placement

The objective is to achieve good cell distribution while reducing congestion and improving timing.

**Image: Standard-Cell Placement**

> Add placement screenshot here.

---

## Clock Tree Synthesis

Clock Tree Synthesis (CTS) creates a clock distribution network from the clock source to sequential elements.

The main objective is to distribute the clock with controlled delay and low skew.

### Important Concepts

- Clock latency
- Clock skew
- Clock buffers
- Clock routing
- Timing optimization

A well-designed clock tree helps maintain reliable timing across the design.

**Image: Clock Tree**

> Add CTS screenshot here.

---

## Routing

Routing creates physical metal connections between the placed cells.

The routing process consists mainly of:

- Global Routing
- Detailed Routing

Routing must satisfy connectivity and technology-specific design rules while minimizing congestion and timing problems.

**Image: Routed Layout**

> Add routing screenshot here.

---

## Antenna Checking

During fabrication, long metal connections can accumulate charge and potentially damage transistor gates.

Antenna checking identifies such fabrication-related violations.

### Antenna Violation Repair

Violations can be addressed using techniques such as:

- Antenna diodes
- Routing modifications
- Layer changes

**Image: Antenna Check**

> Add antenna report or violation screenshot here.

---

## Static Timing Analysis

Static Timing Analysis (STA) is used to determine whether the implemented design satisfies its timing constraints.

Important timing parameters include:

- Setup time
- Hold time
- Clock period
- Propagation delay
- Slack

### Slack

Slack indicates the timing margin available in a path.

```text
Positive Slack  → Timing requirement satisfied
Negative Slack  → Timing violation
```

OpenSTA can be used to analyze timing after physical implementation.

**Image: STA Report**

> Add timing report screenshot here.

---

## Parasitic Extraction

Physical interconnects introduce parasitic resistance and capacitance.

Parasitic extraction determines these values from the physical layout.

The extracted information is commonly stored in **SPEF (Standard Parasitic Exchange Format)**.

The extracted parasitics can then be used for more accurate timing analysis.

**Image: SPEF / Parasitic Extraction**

> Add SPEF file or extraction report screenshot here.

---

## Physical Verification

Physical verification checks whether the final layout is physically correct and follows the required technology rules.

### Design Rule Check (DRC)

DRC verifies that the layout follows manufacturing rules defined by the technology.

Typical checks include:

- Minimum metal width
- Minimum spacing
- Via rules
- Layer restrictions
- Geometrical constraints

### Layout Versus Schematic (LVS)

LVS compares the connectivity of the extracted layout with the intended circuit netlist.

A successful LVS indicates that the physical layout represents the intended circuit correctly.

**Image: DRC / LVS Results**

> Add DRC and LVS result screenshots here.

---

## Logic Equivalence Check

Logic Equivalence Checking verifies that the synthesized or optimized design maintains the same logical behavior as the original design.

It helps ensure that implementation and optimization steps have not changed the intended functionality.

**Image: Logic Equivalence Check**

> Add equivalence-check result screenshot here.

---

## OpenLane

**OpenLane** is an automated RTL-to-GDSII implementation flow that integrates multiple open-source EDA tools.

It automates major stages such as:

```text
Synthesis
    ↓
Floorplanning
    ↓
Placement
    ↓
CTS
    ↓
Routing
    ↓
Extraction
    ↓
Timing Analysis
    ↓
Physical Verification
```

This makes it useful for learning and experimenting with ASIC physical implementation.

**Image: OpenLane Flow**

> Add OpenLane flow or terminal screenshot here.

---

## OpenROAD

OpenROAD is the main physical implementation engine used within the OpenLane flow.

It supports important physical-design operations such as:

- Floorplanning
- Power planning
- Placement
- Optimization
- Clock Tree Synthesis
- Routing

**Image: OpenROAD**

> Add OpenROAD execution or tool-flow screenshot here.

---

## SKY130 PDK

The project uses the **SkyWater SKY130 Process Design Kit (PDK)** as the target technology.

The PDK provides technology-specific information required by the physical-design tools.

### Important PDK Components

- Standard-cell libraries
- LEF files
- Liberty timing libraries
- Design rules
- Metal-layer information
- Physical abstracts

These files allow the design to be implemented and verified according to the target technology.

**Image: SKY130 PDK**

> Add SKY130 PDK / library screenshot here.

---

## OpenLane Configuration

OpenLane uses configuration parameters to control the implementation flow.

Important configuration areas include:

- Core utilization
- Aspect ratio
- Clock period
- I/O placement
- Power planning
- Routing settings
- Timing constraints

Different configurations can produce different area, timing, and routing results.

**Image: OpenLane Configuration**

> Add `config.tcl` or configuration screenshot here.

---

## Physical Implementation

The physical implementation stage combines the major physical-design operations to produce the final routed layout.

The main stages are:

```text
Floorplan
   ↓
Power Planning
   ↓
Placement
   ↓
CTS
   ↓
Routing
   ↓
Optimization
   ↓
Final Layout
```

OpenLane automates these stages while using the technology information provided by the SKY130 PDK.

**Image: Physical Implementation**

> Add OpenLane implementation result or terminal screenshot here.

---

## Project Execution

The design is implemented by providing the required RTL, configuration files, technology files, and constraints to the OpenLane flow.

A typical project structure is:

```text
Design
├── src/
├── config.tcl
└── runs/
    └── <run_directory>/
        ├── results/
        ├── reports/
        ├── logs/
        └── tmp/
```

The flow generates intermediate results, reports, logs, and final physical-design outputs.

**Image: Project Execution**

> Add terminal / OpenLane execution screenshot here.

---

## Physical Design Results

The final implementation can be evaluated using important physical-design metrics.

| Metric | Purpose |
|---|---|
| Area | Measures the physical size of the design |
| Cell Count | Number of standard cells used |
| Utilization | Percentage of the core occupied by cells |
| Timing | Determines whether timing requirements are satisfied |
| Slack | Indicates available timing margin |
| Routing | Confirms successful physical connectivity |
| DRC | Checks manufacturing design rules |
| LVS | Checks layout-to-netlist connectivity |
| Antenna | Checks antenna-related violations |

**Image: Physical Design Results**

> Add final reports / metrics screenshot here.

---

## Design-Space Exploration

Physical-design parameters can be changed and evaluated to study their effect on the final implementation.

Parameters such as:

- Core utilization
- Aspect ratio
- Timing constraints
- Placement settings

can influence:

- Area
- Timing
- Routing congestion
- Utilization
- Design-rule violations

Comparing different configurations helps identify a suitable implementation.

**Image: Design-Space Exploration**

> Add comparison table, report, or screenshot here.

---

## Key Learnings

Through this module, the following concepts were studied:

- Complete ASIC Physical Design flow
- OpenLane automated implementation
- SKY130 PDK
- Floorplanning and utilization
- Power distribution
- Standard-cell placement
- Clock Tree Synthesis
- Routing
- Antenna checking
- Parasitic extraction
- Static Timing Analysis
- DRC and LVS
- Logic Equivalence Checking
- Design-space exploration

The module provided practical understanding of how different physical-design stages are connected and how implementation parameters influence the final design.

---

## Conclusion

This module provided practical exposure to the **ASIC Physical Design flow using OpenLane and the SKY130 PDK**.

The major stages from floorplanning to routing, timing analysis, and physical verification were studied as part of the implementation flow.

The work demonstrated the importance of proper physical planning, placement, clock distribution, routing, timing analysis, and verification in achieving a reliable ASIC layout.

---

## Author

**Vadla Sai Nikhil**  
**Anurag University, ECE — 3rd Year**
