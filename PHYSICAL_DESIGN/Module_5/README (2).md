# SKY130 Module 5: Routing, Verification and Post-Route Analysis

**VLSI Design & Implementation Workshop**  
**Author:** Vadla Sai Nikhil  
**Focus:** Routing · DRC · Power Distribution Network · TritonRoute · RTL-to-GDSII

---

## Module Overview

This module explores the final stages of the RTL-to-GDSII physical-design process, including signal routing, Design Rule Checking (DRC), Power Distribution Network (PDN) construction, global routing, detailed routing, and TritonRoute.

The aim is to understand how a placed design is transformed into a routed layout while maintaining electrical connectivity and observing technology-specific physical rules.

### End-to-End Flow

```text
Placed Design
     ↓
Power Distribution Network
     ↓
Global Routing
     ↓
Detailed Routing
     ↓
TritonRoute
     ↓
Design Rule Check
     ↓
Post-Route Verification
     ↓
Final Routed Design
```

## Contents

- [1. Routing Concepts and DRC](#1-routing-concepts-and-drc)
- [2. Power Delivery and Signal Routing](#2-power-delivery-and-signal-routing)
- [3. TritonRoute and Connectivity](#3-tritonroute-and-connectivity)
- [4. Post-Route Outputs](#4-post-route-outputs)
- [Conclusion](#conclusion)

---

## 1. Routing Concepts and DRC

**Section:** `SKY130_D5_SK1`

This section introduces maze-routing fundamentals and the physical checks used to review a routed layout.

### Labs Covered

| Lab | Topic |
|---|---|
| `SKY_L1` | Introduction to Maze Routing — Lee's Algorithm |
| `SKY_L2` | Lee's Algorithm — Conclusion |
| `SKY_L3` | Design Rule Check |

### 1.1 Maze Routing with Lee's Algorithm

Lee's algorithm is a grid-based search method for connecting a source and destination while avoiding obstacles. The search expands through available grid points and then backtracks from the destination to form a route.

```text
Source
  ↓
Grid Expansion
  ↓
Obstacle Avoidance
  ↓
Target Reached
  ↓
Backtracking
  ↓
Final Route
```

**Main concepts:** routing grids, source and destination points, obstacles, wave propagation, path search, backtracking, and route generation.

**Figure 1 — Lee's algorithm lab view**

<img width="1206" height="598" alt="Lee's algorithm lab view" src="https://github.com/user-attachments/assets/83ae8c8b-b2e3-4dec-afd3-7959280604fb?raw=1" />

**Figure 2 — Lee's algorithm result**

<img width="1070" height="592" alt="Lee's algorithm result" src="https://github.com/user-attachments/assets/710d257b-edeb-415c-bc2e-8d40b067e874?raw=1" />

### 1.2 Lee's Algorithm: Key Observations

This lab considers the strengths and limitations of maze routing.

- The search explores the routing grid systematically.
- It can find a route if a valid path exists and the search space is explored.
- Obstacles can be avoided during path construction.
- Large grids can increase memory use and computational effort.
- These fundamentals help explain the routing engines used in physical-design tools.

**Figure 3 — Lee's algorithm conclusion**

<img width="1104" height="595" alt="Lee's algorithm conclusion" src="https://github.com/user-attachments/assets/fb938a91-4a24-4bd1-bd49-97dea7c8b0ad?raw=1" />

**Figure 4 — Additional routing result**

<img width="1263" height="607" alt="Additional routing result" src="https://github.com/user-attachments/assets/a3d0fb93-7892-4d9f-92f4-d72d12a72de4?raw=1" />

### 1.3 Design Rule Check (DRC)

DRC checks layout geometry against the manufacturing constraints for the target technology. Typical checks include minimum metal width and spacing, via spacing and enclosure, layer connectivity, routing geometry, and other design-rule violations.

```text
Routed Layout
      ↓
Physical Design Rules
      ↓
DRC Engine
      ↓
Violation Detection
      ↓
Debugging
      ↓
Corrected Layout
```

DRC is a key checkpoint before the remaining physical-verification activities.

**Figure 5 — DRC check output**

<img width="1275" height="592" alt="DRC check output" src="https://github.com/user-attachments/assets/d10f8183-12a9-4e86-9d27-ec237f77ea25?raw=1" />

**Figure 6 — Additional DRC result**

<img width="1265" height="601" alt="Additional DRC result" src="https://github.com/user-attachments/assets/f2ef2391-03b1-4f90-9bf5-46290674d5db?raw=1" />

---

## 2. Power Delivery and Signal Routing

**Section:** `SKY130_D5_SK2`

This section covers the Power Distribution Network and the two main stages of signal routing.

### Labs Covered

| Lab | Topic |
|---|---|
| `SKY_L1` | Steps to build the Power Distribution Network |
| `SKY_L2` | Connecting power straps to standard-cell power |
| `SKY_L3` | Global routing, detailed routing, and TritonRoute configuration |

### 2.1 Power Distribution Network (PDN)

The PDN distributes power and ground across the layout so that standard cells receive their required supplies.

```text
Power Source
     ↓
Power Ring
     ↓
Power Straps
     ↓
Power Rails
     ↓
Standard Cells
```

A connected power network links the supply source to the power pins of the standard cells.

### 2.2 Connecting Power Straps to Cell Rails

The upper-level power network must connect to the rails used by standard cells.

```text
Power Straps
     ↓
Power Rails
     ↓
Standard-Cell VDD
     ↓
Standard-Cell GND
```

This connection provides the intended power and ground paths across the placed cells.

### 2.3 Global Routing and Detailed Routing

**Global routing** estimates paths for nets and allocates routing resources. Its output includes route guides for the detailed router.

```text
Nets
 ↓
Routing Resources
 ↓
Global Routing
 ↓
Route Guides
```

**Detailed routing** turns those guides into physical wires and vias while following the required design rules.

```text
Global Route
     ↓
Track Assignment
     ↓
Detailed Routing
     ↓
Physical Wires and Vias
```

**Figure 7 — Timing report and OpenROAD routing activity**

<img width="1059" height="635" alt="Timing report and OpenROAD routing activity" src="https://github.com/user-attachments/assets/d5ad7467-a51c-4482-b80b-c36c1e8878f1?raw=1" />

**Figure 8 — Detailed-routing initialization and design information**

<img width="1058" height="631" alt="Detailed-routing initialization and design information" src="https://github.com/user-attachments/assets/e7f7fee7-187a-4a65-9bca-6c50ba1aa968?raw=1" />

**Figure 9 — Track assignment and route-guide processing**

<img width="1365" height="635" alt="Track assignment and route-guide processing" src="https://github.com/user-attachments/assets/bdb9afb1-c1b7-45ee-a002-1ab8ec5cf4c0?raw=1" />

**Figure 10 — Detailed-routing optimization iterations**

<img width="1235" height="634" alt="Detailed-routing optimization iterations" src="https://github.com/user-attachments/assets/d8761bd2-e278-4ef9-abf1-f7292b744df7?raw=1" />

**Figure 11 — Final detailed-routing statistics**

<img width="1248" height="635" alt="Final detailed-routing statistics" src="https://github.com/user-attachments/assets/c8c1f2ca-ae7b-451d-9be6-6e0c2952305e?raw=1" />

---

## 3. TritonRoute and Connectivity

**Section:** `SKY130_D5_SK3`

TritonRoute is the detailed-routing engine used in the OpenROAD flow. It uses information from global routing to build physical connections between the terminals of each net.

### Labs Covered

| Lab | Topic |
|---|---|
| `SKY_L1` | TritonRoute Feature 1 — Pre-processed route guides |
| `SKY_L2` | Features 2 and 3 — Inter-guide connectivity and intra-/inter-layer routing |
| `SKY_L3` | TritonRoute connectivity handling |
| `SKY_L4` | Routing topology and final files after routing |

### 3.1 Pre-Processed Route Guides

Route guides define the approximate regions in which nets are expected to be routed. TritonRoute uses them to guide detailed route generation.

```text
Global Routing
      ↓
Route Guides
      ↓
TritonRoute
      ↓
Detailed Routing
```

### 3.2 Inter-Guide Connectivity

Inter-guide connectivity joins segments of the same net that pass through separate route guides. The completed route must form a continuous electrical path between the required connection points.

```text
Guide A
   │
   │ Connectivity
   ↓
Guide B
   │
   ↓
Complete Net
```

### 3.3 Intra-Layer and Inter-Layer Routing

**Intra-layer routing** creates connections within a single metal layer.

```text
Metal Layer
───────────────
     Route
───────────────
```

**Inter-layer routing** uses vias to connect routes on different metal layers.

```text
Metal Layer 2
───────────────
       │
      VIA
       │
───────────────
Metal Layer 1
```

Together, these approaches support connections across the available metal stack.

### 3.4 Connectivity Handling

The routing engine uses netlist connectivity, route guides, and routing topology to create wires and vias and connect the required pins.

```text
Netlist Connectivity
       ↓
Routing Guides
       ↓
Routing Topology
       ↓
Wire and Via Generation
       ↓
Connectivity Verification
```

### 3.5 Routing Topology

Routing topology defines how the terminals of a net are connected. For multi-terminal nets, the topology must account for routing resources, obstacles, metal layers, vias, design rules, route guides, and connectivity. The selected topology is then implemented with physical wires and vias.

### 3.6 Outputs After Routing

Depending on the flow and completed stages, post-route outputs can include a routed DEF, routed netlist, route-guide files, physical connectivity data, reports, DRC-related information, and routing databases. These files support later verification and timing analysis.

**Figure 12 — OpenSTA timing analysis and RC extraction after routing**

<img width="1260" height="634" alt="OpenSTA timing analysis and RC extraction after routing" src="https://github.com/user-attachments/assets/95c8f65e-de28-44fc-815a-d53f9d15934d?raw=1" />

**Figure 13 — FastRoute guide file contents**

<img width="1210" height="632" alt="FastRoute guide file contents" src="https://github.com/user-attachments/assets/d4a71e19-ba2d-4e29-ac46-304fcd04b854?raw=1" />

**Figure 14 — SPEF extractor repository files**

<img width="1260" height="632" alt="SPEF extractor repository files" src="https://github.com/user-attachments/assets/e73c8b14-aee9-4a7d-963d-7e4d43aa0d48?raw=1" />

**Figure 15 — OpenLane temporary routing files**

<img width="949" height="594" alt="OpenLane temporary routing files" src="https://github.com/user-attachments/assets/fdbc817a-4bc9-4110-8e82-007bebd61a58?raw=1" />

**Figure 16 — Final routing results containing DEF and SPEF files**

<img width="1058" height="621" alt="Final routing results containing DEF and SPEF files" src="https://github.com/user-attachments/assets/a6576f16-4f09-499c-a370-3f40b1104cc8?raw=1" />

**Figure 17 — SPEF extraction setup and results**

<img width="1061" height="627" alt="SPEF extraction setup and results" src="https://github.com/user-attachments/assets/b8e50f66-9bb8-48bd-9638-774f2c60d9c3?raw=1" />

**Figure 18 — OpenLane synthesis and routing results**

<img width="1065" height="632" alt="OpenLane synthesis and routing results" src="https://github.com/user-attachments/assets/3c07ba80-336b-419f-a1e0-32ba86743d00?raw=1" />

---

## Conclusion

SKY130 Module 5 provides practical exposure to the closing stages of physical design. The labs cover Lee's maze-routing algorithm, DRC, PDN construction, global and detailed routing, and TritonRoute's route-guide and connectivity concepts.

These exercises connect routing theory with real tool output and show how route guides, metal layers, vias, power distribution, connectivity checks, and post-route reports contribute to the RTL-to-GDSII implementation flow.
